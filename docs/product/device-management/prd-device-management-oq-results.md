# Device Management PRD — open-question results

One `## OQ <n>` section per question in [prd-device-management.md](prd-device-management.md)'s [Open Questions](prd-device-management.md#open-questions) table that already carries evidence: the eight questions the [SDK audit](../../briefs/nix-universal-sdk-audit-findings.md) part-answered, each marked `residual` in that table, and the three it bears on without answering — OQ 4, OQ 19, OQ 25 — which stay `open`. An OQ's status may change only when its section exists here, and a section existing is necessary but not sufficient: a `residual` question keeps its section and its status until the hardware spike closes the remainder, and an `open` one keeps both until an answer rather than evidence arrives. The table keeps a one-line "Decision so far" and this file carries the evidence. Owner decisions that close a question are recorded as fences in [prd-device-management-fences.md](prd-device-management-fences.md).

## OQ 1 — Activation-before-discovery sequencing

**Evidence (SDK audit):** ANSWERED per the [SDK audit](../../briefs/nix-universal-sdk-audit-findings.md): app-level license activation precedes every device operation including discovery; device-level serial authorization happens at connect.

**Residual, and what closes it:** observe on hardware — discovery/connect attempted before and after activation.

**Evidence source:** SDK docs. **Gated on:** hardware.

**Carried by:** [R1.4](prd-device-management.md#1-device-pairing), [R1.9](prd-device-management.md#1-device-pairing), [R2.4](prd-device-management.md#2-licensing--pre-authorization); the [device workflow](prd-device-management-journeys.md#device-workflow) and [UJ1](prd-device-management-journeys.md#uj-1-first-run).

## OQ 2 — Calibration state exposure

**Evidence (SDK audit):** Due-ness is SDK-signaled from elapsed time plus ambient temperature drift, so a stored per-instrument constant is the wrong shape ([SDK audit](../../briefs/nix-universal-sdk-audit-findings.md)).

**Residual, and what closes it:** what does the SDK expose — a due flag, a timestamp, the temperature input? Probe the SDK's calibration state surface on hardware.

**Evidence source:** SDK docs. **Gated on:** hardware.

**Carried by:** [R3.2](prd-device-management.md#3-calibration); the due-signal seam in [R6.9](prd-device-management.md#6-mock-device-layer).

## OQ 4 — Battery level capability

**Evidence (SDK audit):** Low battery arrives in the measurement status ([SDK audit](../../briefs/nix-universal-sdk-audit-findings.md), per SDK docs) — evidence that a level exists, not that one can be read outside a measurement.

**Still open, and what closes it:** whether a battery level is pollable without taking a measurement, and at what granularity. Probe the battery surface on hardware.

**Evidence source:** SDK docs. **Gated on:** hardware.

**Carried by:** [R4.1](prd-device-management.md#4-pre-flight-device-health) and [R4.5](prd-device-management.md#4-pre-flight-device-health) (the last-known-level-with-its-age fallback), [R5.1](prd-device-management.md#5-mid-session-device-failure) and [R5.14](prd-device-management.md#5-mid-session-device-failure) (the battery halt and its user-initiated clear). The thresholds themselves are OQ 4b, which has no evidence yet.

## OQ 6 — Discovery timeout

**Evidence (SDK audit):** ANSWERED per the [SDK audit](../../briefs/nix-universal-sdk-audit-findings.md): 20 s vendor default, documented ≥ 10 s floor — devices advertise once per second.

**Residual, and what closes it:** confirm timing behavior on hardware.

**Evidence source:** SDK docs. **Gated on:** hardware.

**Carried by:** [R1.5](prd-device-management.md#1-device-pairing), whose provisional constant is that default and floor.

## OQ 8 — Offline-use window

**Evidence (SDK audit):** ANSWERED per the [SDK audit](../../briefs/nix-universal-sdk-audit-findings.md): ~30-day device-authorization cache; two expiries, gate on the earlier.

**Residual, and what closes it:** primary residual — is the cache expiry (or last-successful-check timestamp) readable from the SDK, and with what precision? Also open: can the app proactively renew/extend the cache (the "Extend offline use" and silent-renewal rows assume yes — unverified)? Probe the authorization surface on hardware.

**Evidence source:** SDK docs. **Gated on:** hardware.

**Carried by:** [R2.9](prd-device-management.md#2-licensing--pre-authorization) and [R2.15](prd-device-management.md#2-licensing--pre-authorization) (the approximate-date fallback), [R2.10](prd-device-management.md#2-licensing--pre-authorization) and [R2.11](prd-device-management.md#2-licensing--pre-authorization) (Extend and renewal).

## OQ 15 — Serial at discovery

**Evidence (SDK audit):** ANSWERED per the [SDK audit](../../briefs/nix-universal-sdk-audit-findings.md): discovery carries id, type, name, signal strength, transport — no serial; post-connect dedup is the primary path, per the vendor's serial-as-identity guidance.

**Residual, and what closes it:** confirm the discovery payload on hardware.

**Evidence source:** SDK docs. **Gated on:** hardware.

**Carried by:** [R1.6](prd-device-management.md#1-device-pairing), [R1.7](prd-device-management.md#1-device-pairing), [R1.16](prd-device-management.md#1-device-pairing); the discovery-entry seam in [R6.14](prd-device-management.md#6-mock-device-layer).

## OQ 16 — SDK network behavior

**Evidence (SDK audit):** Half-answered per the [SDK audit](../../briefs/nix-universal-sdk-audit-findings.md): mandatory Google Analytics events on connect/scan/calibration (no color data).

**Residual, and what closes it:** does SDK telemetry block, delay, or retry during offline operation, and is there a suppression hook? General residual: what network activity the SDK performs at all — including the connect-time authorization phone-home and the documented firewall-whitelisting requirement — beyond telemetry. Network observation on hardware: offline and firewalled runs.

**Evidence source:** SDK docs. **Gated on:** hardware.

**Carried by:** [R2.8](prd-device-management.md#2-licensing--pre-authorization) and [R2.13](prd-device-management.md#2-licensing--pre-authorization); the privacy story.

## OQ 18 — Per-serial refusal

**Evidence (SDK audit):** Confirmed possible per the [SDK audit](../../briefs/nix-universal-sdk-audit-findings.md).

**Residual, and what closes it:** what the SDK reports, and whether a permanent per-device refusal is distinguishable from an auth-server-unreachable failure. Induce a refusal on hardware, or vendor confirmation.

**Evidence source:** SDK docs. **Gated on:** hardware.

**Carried by:** [R2.15](prd-device-management.md#2-licensing--pre-authorization) and [R2.16](prd-device-management.md#2-licensing--pre-authorization); the not-covered state [E8](prd-device-management-copy.md#error--state-copy); the simulated outcome in [R6.10](prd-device-management.md#6-mock-device-layer).

## OQ 19 — Sandbox entitlement conflict

**Evidence (SDK audit):** The vendor documents four required macOS sandbox entitlements — Bluetooth, serial, USB, and network client ([SDK audit](../../briefs/nix-universal-sdk-audit-findings.md), per SDK docs) — while Apple's current entitlement reference does not list the serial one.

**Still open, and what closes it:** whether a sandboxed build can hold all four, and whether the app ships sandboxed at all. An owner decision plus a sandboxed build experiment; the experiment needs a Mac, not an instrument.

**Evidence source:** SDK docs. **Gated on:** owner.

**Carried by:** the sandboxing ADR, not yet written ([AGENTS.md §3](../../../AGENTS.md#3-decided--recommended--open) lists it open); [UJ1.1](prd-device-management-journeys.md#uj-11-cannot-complete-a-first-run)'s USB fallback.

## OQ 23 — macOS SDK USB: remaining half

**Evidence (SDK audit):** USB IS supported on macOS and each discovery entry reports its transport ([SDK audit](../../briefs/nix-universal-sdk-audit-findings.md), per SDK docs).

**Residual, and what closes it:** whether connecting a specific entry selects the transport, and whether the Bluetooth-ID reconnect gives auto-reconnect any handle over USB. The deliverable includes the persisted handle's schema shape (nullable per-transport vs BLE-only). Connect via a USB discovery entry on hardware.

**Evidence source:** SDK docs. **Gated on:** hardware.

**Carried by:** [R1.10](prd-device-management.md#1-device-pairing), [R1.11](prd-device-management.md#1-device-pairing), [R1.16](prd-device-management.md#1-device-pairing); the transport assertions in [R6.19](prd-device-management.md#6-mock-device-layer).

## OQ 25 — Entitlement change mid-session

**Evidence (SDK audit):** An entitlement change invalidates open connections ([SDK audit](../../briefs/nix-universal-sdk-audit-findings.md), per SDK docs), so a licensing event can drop a live session exactly as a link loss does.

**Still open, and what closes it:** whether the app can tell the two apart, and so whether the disconnect copy can name a licensing cause instead of a link one. Change entitlements against a live session on hardware.

**Evidence source:** SDK docs. **Gated on:** hardware.

**Carried by:** [R5.1](prd-device-management.md#5-mid-session-device-failure), whose halt taxonomy absorbs it; the disconnect halt state [E23](prd-device-management-copy.md#error--state-copy).
