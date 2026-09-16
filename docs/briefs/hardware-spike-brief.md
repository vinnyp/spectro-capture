# Spike Brief: The Hardware Spike

## Context

**SpectroCapture** is an open-source macOS app for bulk color acquisition with the Nix Spectro 2 / Spectro L family over BLE or USB. Five PRDs are locked — Device Management, Capture Mode, Inventory Import, Data Foundation and Data Export — and between them they carry the open questions whose closer is the same thing: a person with a real instrument, a license key and a Mac, observing what the vendor SDK actually does. This brief is that spike's scope: the Device Management PRD's Legend scope (every OQ whose closer is "hardware", amended 2026-09-16 to include four questions the Data Foundation and Data Export PRDs hand it) and the Capture Mode PRD's declared spike scope (its OQ 1, 2, 4, 5, 21 and 22, plus the device PRD's OQ 10 and 20). It is the "questions asked" half of the pair; the findings land in `hardware-spike-results.md` beside it, in the shape `rust-boundary-spike-results.md` uses (a verdict table first, then evidence per question).

**Why now.** The Data Foundation and Data Export PRDs locked on 2026-09-16 with four constants gated on this spike, and the two earlier locks left theirs. Every open question either has an interim rule or a dogfood value the engineering plan sets (device fence F7, clarification (1)), and none may ship in a release with its question open. The spike therefore gates the first release, not the first build.

**Already decided — do not re-litigate on the spike.** macOS-only; SwiftUI; a local SQLite file the user owns; offline-first; the vendor SDK as a package dependency, never vendored; all device access through the `SpectroDevice` seam; one device at a time; no color-library matching. The SDK audit (`nix-universal-sdk-audit-findings.md`) is the documentary baseline — the spike confirms or refutes it on hardware, it does not re-read the docs.

**What the spike cannot do.** CI cannot run any of it (no hardware, no license key in CI — `AGENTS.md` §5). No SDK binary, header or license key may enter the repo. Every finding is recorded as an observation with the firmware version, SDK version, transport and macOS version it was made on.

---

## Before the session

The questions below assume the following exists on the day. Confirm each before booking the instrument; three of them need a vendor request raised in advance.

- **A throwaway probe harness** outside the repo (a scratch package against the SDK, never merged — the module-layout ADR is still open, so nothing here shapes the app's layout): it must take a measurement, store the raw payload, reconstruct a measurement from the stored payload and diff the toolkit's derived values (Q2), log SDK events with timestamps (Q7, Q16, Q17, Q18), and time scan cycles over many readings (Q11, Q21).
- **Both instrument models** where a question says so — Q1 asks for the Spectro 2 and the Spectro L; if only one is available, the results file says which and the other model's answer stays open.
- **License variants:** one license with the spectral entitlement and one without (Q19 and Q24 need both); the ability to change entitlements against a live session (Q17) — confirm with the vendor whether that is self-served or a support request, and raise it ahead of time.
- **A vendor confirmation path** for Q15 (per-serial refusal) if inducing a refusal is not possible on the bench.
- **A network-capture tool** on the Mac (Q3), and a **second app instance** or second SDK client (Q18).
- **The calibration tile** and any reference material the vendor supplies (Q4).
- **A time-box:** one instrument day for Q5–Q24, a second for Q1–Q4 if the harness is not ready on the first; a question that does not close on the day is recorded as open with what was tried, never guessed.

---

## Questions

Each question names the PRD row(s) it feeds and the constant or copy it closes. The four questions the Data Foundation and Data Export PRDs hand over come first; the Device Management PRD's own hardware questions follow; the Capture Mode PRD's close the list. Questions that close on first-build halt logs rather than on the spike (device OQ 4b, 7, 20b) are listed last so the spike does not try to answer them.

### 1. Wavelength grid — Data Export OQ 4 (`WAVELENGTH_GRID`)

- Read the grid the instrument reports — start, end, interval — for every measurement mode the Spectro 2 and Spectro L expose. Is it fixed per model, per firmware, or per mode?
- Is the grid readable without taking a measurement, or only from a measurement's payload?
- Feeds: the export PRD's R2.3 (the `sc_nm_<λ>` columns), R2.5 and R4.1 (the golden's wavelength block is re-cut when the grid is read).

### 2. Raw payload round-trip and the toolkit's spaces — Data Foundation OQ 15

- Take a measurement; store its vendor payload as the SDK hands it over; reconstruct a measurement from the stored payload; compare every value the toolkit derives from the original against the reconstruction. What survives the round-trip byte-for-byte, what is recomputed, and what is lost?
- Which derived spaces does the toolkit supply from a payload — XYZ, Lab, LCh, Luv, sRGB, HSL — under which illuminant/observer pairs, and which must the app compute itself? (Capture OQ 21 asks the same of the reference set — answer both from one probe, with and without the spectral entitlement.)
- Feeds: the Data Foundation PRD's R1.6, R2.1, R3.1, R3.2; the export PRD's `sc_sample_N_payload` columns.

### 3. The vendor analytics recipient — Data Foundation OQ 12 (with device OQ 16)

- Observe the SDK's network traffic on connect, scan and calibration: which host, what payload, and does any request carry an item, a reading or a payload?
- Can the analytics be switched off, suppressed or redirected — by API, by configuration, or not at all? What happens offline: block, delay, retry, or drop?
- Feeds: the Data Foundation PRD's R6.5 (the closed data inventory), R1.4 (the enumerated permitted-egress set) and R6.6 (the disclosure floor is re-derived when this closes); the device PRD's R2.8, R2.13.

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

### 11. Scan cycle time — device OQ 10 (and capture OQ 22)

- Measure the time from trigger to a measurement in hand, per mode and per transport, over enough scans to give a distribution. This sets the floor for every time-based target and the capture PRD's `DEMO_SCAN_CYCLE`.
- Feeds: the device PRD's R6.27, M1, M2; the capture PRD's R11.3.

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

- Is "not responding" distinguishable from "disconnected" in the SDK's events? Is haptic triggering exposed? The constants (OQ 20b) close on halt logs; record the raw event timings here. The capture PRD's spike scope names this question too.
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

### 20. Which scan modes a capture records — capture OQ 1

- Take a measurement on hardware and inspect what comes back: which measurement modes arrive with one reading, and does the set differ by model or by entitlement?
- Feeds: the capture PRD's R1.6, R1.10, R4.5, R4.6, R11.8.

### 21. The instrument's button as a trigger — capture OQ 2

- Probe the SDK's event surface: does a button press on the instrument arrive as an event the app can treat as a trigger, and with what latency?
- Feeds: the capture PRD's R4.1, R11.3.

### 22. Dead time after a reading — capture OQ 4 (`LOCKOUT_WINDOW`)

- Measure real scan cycles and try both a lockout and none against a swatch book: is there dead time after a reading, and does a second trigger inside it double-count or misfire?
- Feeds: the capture PRD's R4.3.

### 23. The timing constants — capture OQ 5

- Measure the acknowledgement, result-cue and row-confirm timings against the Demo Device's two clocks, then confirm on hardware and on each class of volume (local, USB external, network).
- Feeds: the capture PRD's R4.8, R4.13, R11.7, M1, M10.

### 24. Illuminant and observer pairs — capture OQ 21

- Read the toolkit's reference set on hardware, with and without the spectral entitlement: which illuminant/observer pairs may a collection choose? (Answered from the same probe as Q2's second bullet.)
- Feeds: the capture PRD's R1.5, R4.24.

### Not on the spike (close on first-build halt logs or on the plan)

- Device OQ 4b (battery constants), OQ 7 (reconnect retry bounds), OQ 20b (liveness and disconnect constants) — the spike records the raw readings that make those tunable; the numbers themselves come from the first ~10 real sessions.
- The capture PRD's dogfood entry point (its OQ 3, 6, 23, 25 and the import PRD's OQ 1) closes on dogfood data, not on the bench.

---

## Deliverable

`docs/briefs/hardware-spike-results.md`: a verdict table (question → answer in one line → the OQ it closes → confirms or refutes the SDK audit), then one section per question with the observation, the environment (firmware, SDK version, transport, macOS version), and the raw data or a pointer to a checked-in artifact where one is safe to check in (no payloads that carry a serial the owner has not cleared, no license material).

That file is **evidence, never the status change.** An open question closes only when its own PRD's OQ-results file gains the section the PRD's closing rule requires (the device PRD's `prd-device-management-oq-results.md`, and likewise for capture, Data Foundation and Data Export); the owner carries each answer there, and the constants it sets are recorded against their rows.
