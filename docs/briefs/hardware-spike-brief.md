# Spike Brief: The Hardware Spike

## Context

**SpectroCapture** is an open-source macOS app for bulk color acquisition with the Nix Spectro 2 / Spectro L family over BLE or USB. Five PRDs are locked — Device Management, Capture Mode, Inventory Import, Data Foundation and Data Export — and between them they carry twenty-one open questions whose closer is the same thing: a person with a real instrument, a license key and a Mac, observing what the vendor SDK actually does. This brief is that spike's scope. It is the "questions asked" half of the pair; the findings land in `hardware-spike-results.md` beside it, one section per question, in the shape `rust-boundary-spike-results.md` uses (a verdict table first, then evidence per criterion).

**Why now.** The Data Foundation and Data Export PRDs locked on 2026-09-16 with four constants gated on this spike, and the Device Management PRD's Legend scopes the spike as "every OQ whose closer is 'hardware'" plus those four (amended 2026-09-16). Every one of them has an interim rule the first build can run on, and none may ship in a release with its question open. The spike therefore gates the first release, not the first build.

**Already decided — do not re-litigate on the spike.** macOS-only; SwiftUI; a local SQLite file the user owns; offline-first; the vendor SDK as a package dependency, never vendored; all device access through the `SpectroDevice` seam; one device at a time; no color-library matching. The SDK audit (`nix-universal-sdk-audit-findings.md`) is the documentary baseline — the spike confirms or refutes it on hardware, it does not re-read the docs.

**What the spike cannot do.** CI cannot run any of it (no hardware, no license key in CI — `AGENTS.md` §5). No SDK binary, header or license key may enter the repo. Every finding is recorded as an observation with the firmware version, SDK version, transport and macOS version it was made on.

---

## Questions

Each question names the PRD row(s) it feeds and the constant or copy it closes. The device PRD's own hardware questions come first; the four the Data Foundation and Data Export PRDs hand over follow. Questions that close on first-build halt logs rather than on the spike (device OQ 4b, 7, 20b) are listed last so the spike does not try to answer them.

### 1. Wavelength grid — Data Export OQ 4 (`WAVELENGTH_GRID`)

- Read the grid the instrument reports — start, end, interval — for every measurement mode the Spectro 2 and Spectro L expose. Is it fixed per model, per firmware, or per mode?
- Is the grid readable without taking a measurement, or only from a measurement's payload?
- Feeds: the export PRD's R2.3, R2.5, R4.1 (the golden's wavelength block is cut when the grid is read); the Data Foundation PRD's R1.2 `sc_nm_<λ>` columns.

### 2. Raw payload round-trip and the toolkit's spaces — Data Foundation OQ 15

- Take a measurement; store its vendor payload as the SDK hands it over; reconstruct a measurement from the stored payload; compare every value the toolkit derives from the original against the reconstruction. What survives the round-trip byte-for-byte, what is recomputed, and what is lost?
- Which derived spaces does the toolkit supply from a payload — XYZ, Lab, LCh, Luv, sRGB, HSL — under which illuminant/observer pairs, and which must the app compute itself?
- Feeds: the Data Foundation PRD's R1.6, R3.1, R3.2, M2, M7; the export PRD's `sc_sample_N_payload` columns.

### 3. The vendor analytics recipient — Data Foundation OQ 12 (with device OQ 16)

- Observe the SDK's network traffic on connect, scan and calibration: which host, what payload, and does any request carry an item, a reading or a payload?
- Can the analytics be switched off, suppressed or redirected — by API, by configuration, or not at all? What happens offline: block, delay, retry, or drop?
- Feeds: the Data Foundation PRD's R1.4 (the enumerated permitted-egress set) and R6.6 (the disclosure floor is re-derived when this closes); the device PRD's R2.8, R2.13.

### 4. The published reference set — Data Foundation OQ 6 (`DERIVATION_TOLERANCE`)

- Identify a published source of payloads or reflectance curves with independently known Lab/XYZ values under the conditions the app derives in, so the fixture set R7.5 checks in has provenance. Does the vendor supply reference tiles or reference data? Does the calibration tile carry a published reference?
- Measure the calibration tile and any available reference material; record the ΔE2000 spread across repeated readings, which sets the floor `DERIVATION_TOLERANCE` (candidate 0.1) can sit on.
- Feeds: the Data Foundation PRD's R7.5, M2, M7.

### 5. Activation and discovery — device OQ 1

- Attempt discovery and connect before activation and after: what does each return, and what error value names the unactivated state?
- Feeds: R1.4, R1.9, R2.4, the device workflow.

### 6. Calibration state — device OQ 2

- What does the SDK expose about calibration state: a due flag, a timestamp, the temperature input, or nothing readable? How long does a calibration take, and how does the due signal present?
- Feeds: R3.2, R6.9; OQ 3 waits on this.

### 7. Battery surface — device OQ 4

- Is battery level readable on demand, only in the measurement status, or both? At what resolution and cadence?
- Feeds: R4.1, R4.5, R5.1, R5.14. The halt and resume thresholds themselves (OQ 4b) close on halt logs, not here — record the raw readings so they can be set later.

### 8. Discovery timing — device OQ 6

- Confirm the 20 s default and the 10 s floor; measure how long a device takes to appear after power-on, over BLE and over USB.
- Feeds: R1.5.

### 9. The authorization cache — device OQ 8

- Is the offline-use expiry readable, at what precision, and can the cache be renewed proactively while online? What exactly does the SDK report at the boundary?
- Feeds: R2.9, R2.10, R2.11, R2.15.

### 10. USB versus BLE — device OQ 23, then OQ 9

- Connect via a USB discovery entry: does connecting an entry select the transport, and does the Bluetooth-ID reconnect give USB a handle? Then, with both transports reachable, which should the app prefer and why (latency, reliability, power)?
- Feeds: R1.10, R1.11, R1.16, R6.19.

### 11. Scan cycle time — device OQ 10

- Measure the time from trigger to a measurement in hand, per mode and per transport, over enough scans to give a distribution. This sets the floor for every time-based target.
- Feeds: R6.27, M1, M2.

### 12. Mid-session expiry — device OQ 11

- Let the authorization window lapse during a session: does the SDK hard-stop, refuse the next scan, or carry on? What does the app see?
- Feeds: R2.17, R5.1.

### 13. Serial at discovery — device OQ 15

- Confirm what the discovery payload carries (id, type, name, signal strength, transport) and that the serial arrives only after connect.
- Feeds: R1.6, R1.7, R1.16, R6.14.

### 14. Vendor auto-reconnect — device OQ 17

- Does the SDK reconnect on its own after a link loss, and can that be disabled or subordinated to the app's single connection authority?
- Feeds: R1.15.

### 15. Per-serial refusal — device OQ 18

- Induce a per-serial refusal (or obtain vendor confirmation of what is reported): what error value, and is it distinguishable from a lapsed window?
- Feeds: R2.15, R2.16, R6.10, E8.

### 16. Liveness and haptics — device OQ 20

- Is "not responding" distinguishable from "disconnected" in the SDK's events? Is haptic triggering exposed? The constants (OQ 20b) close on halt logs; record the raw event timings here.
- Feeds: R5.4, R5.13, R6.9.

### 17. Entitlement change mid-session — device OQ 25

- Change entitlements against a live session: what does the app receive, and can the disconnect copy distinguish a license event from a link loss?
- Feeds: R5.1, E23.

### 18. Held by another app — device OQ 26

- Two-app connect experiment: does the SDK distinguish "held by another app" from other connect failures, and with what error value?
- Feeds: R1.12, R6.9, E9.

### 19. Spectral entitlement — device OQ 27

- Is `providesSpectral` (or its equivalent) answerable at pairing, without taking a measurement?
- Feeds: R2.6, R2.7.

### Not on the spike (close on first-build halt logs)

- Device OQ 4b (battery constants), OQ 7 (reconnect retry bounds), OQ 20b (liveness and disconnect constants) — the spike records the raw readings that make those tunable; the numbers themselves come from the first ~10 real sessions.

---

## Deliverable

`docs/briefs/hardware-spike-results.md`: a verdict table (question → answer in one line → the OQ it closes → confirms or refutes the SDK audit), then one section per question with the observation, the environment (firmware, SDK version, transport, macOS version), and the raw data or a pointer to a checked-in artifact where one is safe to check in (no payloads that carry a serial the owner has not cleared, no license material). Each closed question is then carried into its PRD's OQ results file by the owner, and the constants it sets are recorded against their rows.
