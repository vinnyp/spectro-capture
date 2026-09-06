# Nix Universal SDK (iOS/macOS) — audit findings

> **Provenance & how to read this.** Audit of the Nix Universal SDK 4.2.3 distribution, performed 2026-08-16. Sources: the full DocC documentation archive (518 pages), both example apps, the C/C++ wrapper project, and release notes 4.0.0–4.2.3. Everything below is from vendor docs/code (nothing validated against a live device); inferences are flagged inline. **Role in this repo:** the paper-evidence base for the hardware/SDK spike — this audit predates the device-management PRD and ADR-0001 and decides nothing (briefs are research, not decisions — [AGENTS.md §2](../../AGENTS.md#2-read-before-changing-anything)). The SDK is cross-Apple-platform and the audit discusses iOS throughout, while SpectroCapture is macOS-only, and sandboxing remains an open decision ([AGENTS.md §3](../../AGENTS.md#3-decided--recommended--open)) — the entitlements described in §3 describe the sandboxed path.

## 1. What this is and why it matters

The Nix Universal SDK is a native Apple-platform library (XCFramework, Swift + Objective-C) that lets a third-party app discover, connect to, and take measurements from Nix color-measurement hardware — the Mini/Pro colorimeter line and the Spectro 2 / Spectro L spectrophotometers. It handles Bluetooth LE (iOS + macOS) and USB (macOS only) transport, device lifecycle, measurement capture, color math (conversions + delta E), spectral data, and ISO density calculations.

The single most important product fact in the archive: **since v4.2.0 (Feb 2025), the SDK is dark by default.** Every device operation is disabled until a signed license key is activated at runtime, and the license — not the hardware — decides which device types can connect and which data types (basic color, spectral, density) come out. Nix has converted the SDK from a hardware accessory into a **licensed platform with entitlement-gated capabilities**.

The second most important fact: this is a **single-device, single-app, foreground-session SDK**. One app talks to one Nix device at a time; a device talks to one host at a time; license activation doesn't persist across launches. It is built for "operator points device at a thing and scans it" workflows — not fleets, not unattended monitoring, not multi-sensor rigs.

## 2. Hardware and capability matrix (what the license can even unlock)

Eight supported devices, in two meaningful tiers:

| Capability | Mini | Mini 2 | Mini 3 | Pro | Pro 2 | QC | Spectro 2 | Spectro L |
|---|---|---|---|---|---|---|---|---|
| Color (D50/2° always) | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Full 18-reference-white color | – | – | ✓ | – | – | – | ✓ | ✓ |
| Spectral output | – | – | – | – | – | – | ✓ | ✓ |
| ISO density (CMYK) | – | – | – | – | – | – | ✓ | ✓ |
| Field calibration (QR tile) | – | – | ✓ | – | – | ✓ | ✓ | ✓ |
| Measurement modes | M2 | M2 | M2 | M2 | M2 | M2 | M0/M1 (+M2 on F2.x firmware) | M0/M1/M2 |
| Haptic / RGB feedback | – | – | RGB only | – | – | – | ✓ | ✓ |
| Temperature compensation | – | ✓ | ✓ | – | ✓ | ✓ | ✓ | ✓ |

So-what: **the colorimeters (Mini/Pro/QC) and the spectrophotometers (Spectro 2/L) are effectively two different products behind one API.** A consumer "match this paint color" use case runs fine on the Mini tier with D50/2° CIELAB. Anything touching print QC, ink density, multi-illuminant proofing, or metamerism analysis requires Spectro-class hardware *and* the `SPECTRAL_DATA` / `DENSITY_DATA` license features. Runtime checks (`providesSpectral`, `providesDensity`) fold together hardware capability × license entitlement. (SpectroCapture targets the Spectro tier only — see [vision.md](../product/vision.md).)

The measurement data model is clean: one scan → an `IMeasurementData` per scan mode → from it, `IColorData` (CIEXYZ/LAB/LCH/LUV, always XYZ-backed, freely convertible), `ISpectralData` (wavelength + reflectance arrays), `IDensityData` (CMYK density per ISO status). A `raw` string round-trips the entire measurement (`fromRaw()`) — the documented persistence strategy is **store the raw string, not derived values** — independently confirming the project's canonical-raw-payload rule, and direct evidence for the queued storage ADR. Delta E comes in six flavors (CIE76, CIE94 graphics/textiles, CIE2000, CMC 1:1 and 2:1) via `compareTo` or the standalone `ColorUtils` math library (which also does CMYK↔XYZ with ICC-style LUTs, sRGB, chromatic adaptation).

## 3. User journeys

### Journey A: the integrating developer (the SDK's actual "user")

1. **Install** — Swift Package (recommended, GitHub-hosted), CocoaPods, or direct XCFramework embed. Must add Bluetooth usage strings to Info.plist (**app hard-crashes if missing** — the vendor's wording; Apple's own is softer — documented, and a classic first-hour failure) and, on macOS, four sandbox entitlements (Bluetooth, serial, USB, network client).
2. **Activate license** — `LicenseManager.activate(options:signature:)` with two strings, *every app launch* (activation is session-only). Error states fully enumerated.
3. **Scan** — `DeviceScanner` with a delegate; 20-second default scan window (docs: don't go below 10s — devices advertise 1/sec and packets get lost). Devices arrive via callback with `id`, `type`, `name`, `rssi`, `interfaceType`.
4. **Connect** — async `connect(delegate:)`; success or a typed disconnect reason. At connect time the SDK checks the device's allocation code against the license and may phone home to authorize the serial number.
5. **Measure** — async `measure(completion:)`; result is a dictionary of measurements keyed by scan mode, plus a rich status enum (busy, low battery, **ambient light leakage**, license error, etc.).
6. **Calibrate (device-dependent)** — scan the QR code on the reference tile → validate string → user places device on tile → `runFieldCalibration`. SDK signals when calibration is *due* (elapsed time + ambient temperature drift). Takes 5–10s on Spectro 2.
7. **Reconnect later** — `startSearchForId()` recalls a known device by Bluetooth ID without a full scan.

The docs are unusually opinionated about end-user UX, and the guidance is good: sort discovered devices by signal strength, render RSSI as icons not numbers, prompt the user to bring the device closer, distinguish USB vs BLE in the list, use serial number (not BLE id) as durable device identity. The example apps implement all of it — device list with signal icons, license screens, settings toggles, spectral plots, QR tile flow. They're a legitimate reference implementation, not toy demos.

### Journey B: the end user holding the device (implied)

Discover → pair → scan a surface → see color/spectral result → occasionally be walked through tile calibration when the app says it's due. Two moments where the integrating app has to do real design work, because the SDK punts them to it: (1) the calibration interruption ("your readings may be drifting — go find your tile"), and (2) the unauthorized-device dead end (`ERROR_UNAUTHORIZED` — a real purchased device this license won't serve; the example app shows a "contact Nix" alert; a shipped product needs something better).

### Journey C: the non-Apple-native team

A full C/C++ wrapper project ships for macOS (~85 exported functions, JSON-serialized measurement output, dylib + framework loaded via `dlopen`). No official plugins for React Native / Flutter / Unity / Xamarin — bridging is documented as possible and is left to the integrator. Android and Windows SDK variants exist as separate products.

## 4. The licensing system

How the license system works:

- **Entitlements**: license defines allowed device *types* and enabled *features* (`BASIC_DATA`, `SPECTRAL_DATA`, `DENSITY_DATA`), plus expiry date, UUID, and allocation codes. Density can be hardware-supported and still return `nil` because the license says no.
- **Device-level authorization**: each physical device's allocation code is checked against the license at connect. Unknown device → online check against Nix's auth server → cached 30 days → offline OK until re-check. Behind a firewall you must whitelist their serial endpoint. **A shipped app therefore has a soft online dependency**: a user whose network blocks the check, ~30 days after first connect, gets `ERROR_UNAUTHORIZED` on hardware they own.
- **Private-label carve-out**: private-labelled devices can be exempted from online checks.
- **Mandatory usage telemetry**: the SDK sends Google Analytics events on connect/scan/calibration (device type, batch ID, SDK version — no color data). This needs disclosure in the app's own privacy story.
- Housekeeping: the vendor's macOS example app embeds its own license key (not reproduced here) — never copy example-app code wholesale, and never vendor or commit the example apps ([AGENTS.md §5](../../AGENTS.md#5-hardware--the-public-repo-boundary)); every user supplies their own key. The options-string format (params + signature) shows entitlements are encoded client-side and signed — hence per-session activation with no server round-trip for activation itself.

Evidence label: all of the above is vendor documentation — strongest tier available here, but not observed behavior.

## 5. Limitations observed in the docs

**Hard constraints (architecture, not configuration):**

1. One device per app session; one app per device. No multi-device workflows, period.
2. USB is macOS-only; iPhone/iPad is BLE-only. No wired iOS capture station without a Mac.
3. No BLE in iOS Simulator — device testing needs physical iPhones (or an Apple-silicon Mac running the iOS build, which *does* get full Nix support — useful for dev loops).
4. License activation every launch; entitlement changes invalidate open connections.
5. Apple-native only. Cross-platform = build your own bridge, or use the separate Android/Windows SDKs and reconcile three surfaces.

**Deliberate scope boundaries (the SDK will never do this for you):**

6. **No color libraries.** PANTONE/RAL/NCS/paint-brand matching is explicitly the app developer's problem — the SDK gives you delta E math and nothing to compare against. If a product story were "scan → match to a named color," library licensing/data acquisition would be a whole separate workstream — not a cost for SpectroCapture, where library matching is an explicit non-goal.
7. No cloud, no storage, no sync — persistence is "save the raw string yourself."
8. No fleet management, provisioning, or MDM story.

**Operational frictions:**

9. The 30-day online re-authorization (§4) — matters most in offline-heavy environments (print floor, factory QC, field survey).
10. Measurement can fail for physical reasons the app must UX-handle: ambient light leakage, low battery, out-of-range temperature, calibration drift (`ERROR_SCAN_DELTA`).
11. Mandatory analytics telemetry (§4) needs disclosure in the integrating app's privacy story.

## 6. Assessment (as of the audit date)

Mature, well-documented, conservatively-scoped SDK — docs quality is top-decile for hardware vendors (opinionated UX guidance, honest capability matrices, enumerated error states), steady release cadence 4.0.0 (Jan 2023) → 4.2.3 (Apr 2026), with the big strategic move being the 4.2.0 license gate. Integration risk is low; every capability, device count, and offline behavior runs through the license entitlements.

The audit's original recommendation, preserved: get a device + eval license and build the small spike — activate, scan, connect, measure, print LAB + raw string — converting this paper audit into observed behavior in a day. The hardware/SDK spike is now scoped by the device-management PRD's Open Questions, which name the specific behaviors to probe; record the firmware version of the units used (the capability matrix's M2-on-F2.x line depends on it).
