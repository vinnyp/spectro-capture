# Product Requirements Documents

This directory holds the product definition: [`vision.md`](vision.md) sets scope, personas, journeys, and the feature list; each PRD below takes a slice of that and specifies it to the level a builder can work from. Direction lives one level up in [STRATEGY.md](../../STRATEGY.md).

A PRD is not a decision about *how*. Architecture is decided only in [`docs/decisions/`](../decisions) ([AGENTS.md §2](../../AGENTS.md#2-read-before-changing-anything)) — but several ADRs are gated on a PRD answering the product question first. Those gates are tracked in the [ADR decision queue](../decisions/README.md#decision-queue) and restated in [PRD → ADR gates](#prd--adr-gates) below.

## The PRD set

Five PRDs remain to be written to cover v1. Priority is authoring order, not a cut line.

| # | PRD | Use cases | Status |
|---|---|---|---|
| 1 | [Device Management](device-management/prd-device-management.md) | U3, U8, U9 | **Written** |
| 2 | [Capture Mode](capture-mode/prd-capture-mode.md) | U1, U2 | **Locked** — review gate closed 2026-09-07 after 24 rounds; owner sign-off on the artifact set pending, no PR yet |
| 3 | [Inventory Import](import/prd-inventory-import.md) | — (the first step of U1, which Capture Mode owns) | **Drafted** from the locked capture rows under fence F49, pending its verification round |
| 4 | Data Foundation | U5, U6 | queued |
| 5 | Collection Mode | U5, U7 | queued |
| 6 | QC &amp; Comparison | U4 | queued |
| 7 | Color Visualization | U7 | queued |
| 8 | Telemetry | — | queued — v1.x, gated on the provider spike |

### 1. Device Management — written

Connect (BLE + USB), known-device management, licensing and offline pre-authorization, guided calibration, the pre-flight readiness gate, mid-session device failure and recovery, and the mock-device layer. Owns device-level failure end-to-end: detect → alert → reconnect → resume.

### 2. Capture Mode

The heads-down loop, and the product bet made tangible. The scan queue and row auto-advance · 1–5 sample averaging · inline per-scan failure handling (retry / skip / flag-row) and the dead-letter queue · ad-hoc single capture · session durability and resumability across launches.

The PRD is [prd-capture-mode.md](capture-mode/prd-capture-mode.md), with three companion files: the user journeys in [prd-capture-mode-journeys.md](capture-mode/prd-capture-mode-journeys.md), the shipping error and state copy in [prd-capture-mode-copy.md](capture-mode/prd-capture-mode-copy.md), and the answers to closed open questions in [prd-capture-mode-oq-results.md](capture-mode/prd-capture-mode-oq-results.md). Owner decisions are in [prd-capture-mode-fences.md](capture-mode/prd-capture-mode-fences.md). CSV inventory import moved out of this PRD under fence F49 and is now [Inventory Import](#3-inventory-import).

**Also owns the seam re-open.** The two capture-mode research passes disagree on whether capture is a modal takeover that hands off at session end, or writes directly into the live collection; the v2 pass reverses the first and says explicitly to re-open it before the ADR is written. This PRD is where that gets settled at the product level, which is the sole gate on ADR-0004.

### 3. Inventory Import

Reading a spreadsheet export, mapping its columns onto swatches, and landing every row in a collection as pending — the step that fills the queue a bulk session then scans. Import ends at ready-to-capture and never starts a session. Owns the one matching rule for Swatch Codes and collection names, the all-or-none commit, and the idempotent re-import that never costs the Cataloger a measurement.

The PRD is [prd-inventory-import.md](import/prd-inventory-import.md), with four companion files: the user journeys in [prd-inventory-import-journeys.md](import/prd-inventory-import-journeys.md), the shipping error and state copy in [prd-inventory-import-copy.md](import/prd-inventory-import-copy.md), the answers to closed open questions in [prd-inventory-import-oq-results.md](import/prd-inventory-import-oq-results.md), and the owner decisions in [prd-inventory-import-fences.md](import/prd-inventory-import-fences.md).

Its rows were split out of the locked Capture Mode PRD under fence F49 with no rule changed; they arrived 🤝 Aligned and one verification round over both documents is still owed.

### 4. Data Foundation

The user-owned SQLite file. Canonical raw payload · version history and the correction-vs-re-measurement model · derived spaces (Lab/XYZ/LCh/Luv/sRGB/HSL) and the gamut-clipped flag · CSV export · what the app guarantees to someone querying the file directly.

Gates ADR-0003, the sharpest one-way door in the project: this schema ships inside users' own files.

### 5. Collection Mode

Browsing and working with a collection after capture. Browse at scale · search, filter, facet, sort · editing surfaces · selection and bulk operations · the version-history UI · the gamut-aware swatch grid.

### 6. QC &amp; Comparison

QC scan against a saved item, ΔE2000 verdict versus the canonical value, the delta stored as its own record, canonical never overwritten. Small by design — this is where the strategy deliberately holds at parity rather than building QC depth, and the PRD's job is as much to draw that line as to specify the feature.

### 7. Color Visualization

The 3D absolute-space plot (P2). Unserved anywhere in the market, per the competitive analysis.

### 8. Telemetry

Opt-in and off by default · per-fork provider ID so no fork data reaches the project · the opt-in UX · fire-and-forget delivery that can never block, delay, or halt capture. v1.x, and gated on a provider spike (device PRD [OQ 12](device-management/prd-device-management.md#open-questions)).

## Coverage

Every v1 use case and feature maps to exactly one owning PRD. This table is the check that the set is complete.

### Use cases

| # | Use case | Owner |
|---|---|---|
| U1 | Bulk-digitize a predefined inventory | Capture Mode — its first step is [Inventory Import](import/prd-inventory-import.md) |
| U2 | Capture a single new item ad hoc | Capture Mode |
| U3 | Start a session with a healthy device | [Device Management](device-management/prd-device-management.md) |
| U4 | Verify a color still matches | QC &amp; Comparison |
| U5 | Fix a bad scan without losing history | Data Foundation (storage) · Collection Mode (UI) |
| U6 | Use the data outside the app | Data Foundation |
| U7 | See the collection honestly | Collection Mode (swatch grid) · Color Visualization (3D plot) |
| U8 | Scan where there is no internet | [Device Management](device-management/prd-device-management.md) |
| U9 | Contribute code without hardware | [Device Management](device-management/prd-device-management.md) §6 |

### v1 features

| Pri | Feature | Owner |
|---|---|---|
| P0 | Spectro 2/L connect (BLE + USB) | [Device Management](device-management/prd-device-management.md) |
| P0 | Known-device management | [Device Management](device-management/prd-device-management.md) |
| P0 | Tile calibration with due-prompts | [Device Management](device-management/prd-device-management.md) |
| P0 | CSV inventory import with column mapping | [Inventory Import](import/prd-inventory-import.md) |
| P0 | Queued bulk scan, 1–5 samples averaged | Capture Mode |
| P0 | Inline scan-failure handling | Capture Mode |
| P0 | Collections + version history | Data Foundation (model) · Collection Mode (UI) |
| P0 | CSV export, spectral + derived spaces | Data Foundation |
| P0 | Local SQLite store, raw payload canonical | Data Foundation |
| P0 | Offline operation + per-device pre-authorization | [Device Management](device-management/prd-device-management.md) |
| P0 | Mock-device layer | [Device Management](device-management/prd-device-management.md) §6 |
| P1 | Ad-hoc single capture | Capture Mode |
| P1 | QC delta E vs canonical | QC &amp; Comparison |
| P2 | 3D absolute-space plot | Color Visualization |
| P2 | Gamut-aware swatch grid | Collection Mode |

## Authoring order

**Capture Mode is locked** — its review gate closed after 24 rounds, with owner sign-off on the artifact set still pending — and **Inventory Import**, split out of it under fence F49, is awaiting the verification round over both documents. Capture Mode still holds the seam re-open, the only thing that unblocks ADR-0004.

**Data Foundation is next.** ADR-0003's scope is deliberately limited to the measurement/versioning core, which is independent of the seam; only the session-adjacent tables (queue, dead-letter) wait on ADR-0004.

Collection Mode follows both, since it renders what the data model defines and inherits the seam's outcome. QC &amp; Comparison and Color Visualization are P1/P2 and can follow at any point. Telemetry is last regardless — it is v1.x and blocked on a spike.

## PRD → ADR gates

| ADR | Decision | Gating PRD |
|---|---|---|
| [0003](../decisions/README.md#decision-queue) | Storage schema: canonical raw payload, version history, derived-value recompute, migration mechanism | Data Foundation |
| [0004](../decisions/README.md#decision-queue) | The capture → collection seam | Capture Mode (the product-level re-open) |

ADRs 0002, 0005, 0006, 0007, and 0008 carry no PRD gate — they are decided from research and the existing vision.

## Inherited obligations already parked

The device-management PRD specifies behavior it does not own and hands the obligation forward. Each future PRD inherits these; they are requirements, not suggestions.

**Capture Mode inherits:**

- What happens to the un-scanned remainder of the queue when a session ends from a halted state
- Per-scan error UX — retry / skip / flag-row — and the dead-letter queue, including the ambient-light and out-of-range-temperature cases the simulated layer must be able to inject
- Session and queue persistence across app launches

**Data Foundation inherits:**

- Every measurement permanently records the acquiring device's identity — kind (live or simulated), model, serial, firmware version — as an immutable snapshot independent of saved-device records. Removing a saved device can never alter, orphan, or cascade into measurements, and a simulated device can never collide with a live device's record even on the same serial.

**Collection Mode inherits:**

- The simulated-readings collection banner, which renders in collection mode but is specified by the device PRD

**Telemetry inherits:**

- Fire-and-forget delivery that can never block, delay, or halt capture
- With telemetry off, zero telemetry network attempts, assertable via the outbound-attempt observability the device PRD requires
- The Add Device task-completion and task-duration metrics, which the device PRD defines but cannot measure without it

## Scoping calls made here

Two boundaries were judgment calls rather than readings of the vision. Both are cheap to revisit before the PRDs are written, and expensive after.

**QC is its own PRD rather than a section of Collection Mode.** It straddles three: it uses the device and multi-sample averaging, it starts from an item in the collection, and it writes a delta record into the schema. A separate document makes the parity-minimum boundary explicit instead of burying it. Folding it into Collection Mode is a reasonable cheaper alternative.

**The 3D plot splits out, but the swatch grid stays in Collection Mode.** Gamut honesty is not a separable feature — it is a property of any surface that renders or exports color, so it belongs wherever color appears. The absolute-space plot is a genuinely distinct P2 surface. This splits STRATEGY.md's single "Honest collection view" track across two PRDs.

## Deliberately not PRDs

- **Contributor experience (J5).** The mock-device layer is [device PRD §6](device-management/prd-device-management.md); the rest is [CONTRIBUTING.md](../../CONTRIBUTING.md) and the `needs-hardware-verify` workflow. Engineering process, not product.
- **Distribution, sandboxing, minimum macOS version.** ADRs 0006–0008 carry no PRD gate. The only user-facing residue is the software-update check, already constrained by [device PRD §2](device-management/prd-device-management.md#2-licensing--pre-authorization). Write one only if the sandbox decision changes where the user's SQLite file lives — that would be a product question about file ownership, not a packaging detail.
- **App architecture, module layout, state ownership.** ADR territory (0001, 0005).

## v2 backlog

Not written until v2 is scoped. From the vision's v2 candidates: CxF import/export and the vendor-app migration path (J7) · multi-instrument support · printer/output-profile gamut analysis · Sheets/Excel direct import · density data for print workflows · multi-collection compare.
