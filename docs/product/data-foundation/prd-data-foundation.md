# PRD: Data Foundation

Status: locked (2026-09-16); PR #17 agent-build amendment approved 2026-09-17 (F33–F49); PR #18 Export-mirror amendment (R7.7a/e/i/l/n, Data Export obligation). Requirements remain aligned, not implemented.

Companions: [journeys](prd-data-foundation-journeys.md), [shipping copy](prd-data-foundation-copy.md), [answered questions](prd-data-foundation-oq-results.md), [owner decisions](prd-data-foundation-fences.md).

# Background

Contract: the user-owned SQLite file, canonical measurements, history, derived values, recovery and deletion. Product scope is fixed by the [vision](../vision.md); architecture is owned by [ADRs](../../decisions/README.md), including accepted ADR-0001 and pending ADR-0003.

Read [AGENTS.md](../../../AGENTS.md) and the [post-lock list](../post-lock.md) before implementation. Owner product decisions are in the fences companion; research is evidence, not architecture authority.

**In scope:** the file, where it lives, what it states about itself; canonical value, samples, version history; derived values and the gamut-clipped flag; migration; deletion and privacy; verifiability.

**Out of scope:** the storage schema, blob layout, SQLite library and migration mechanism, ADR-0003's and the technical spec's (fence F1); the export contract, [the export PRD](../export/prd-data-export.md)'s under fence F30; browsing and editing, Collection Mode's; the ΔE comparison, QC & Comparison's; CxF and migration in from a vendor app, v2 ([the export PRD's OQ 3](../export/prd-data-export.md#open-questions)); cloud sync ([AGENTS.md §4](../../../AGENTS.md#4-non-negotiables)).

Measurement identity and canonical selection are specified separately in [Measurement operations](#measurement-operations); replacing or restoring a value creates a new reading.

## User Journeys

The [journeys](prd-data-foundation-journeys.md) exercise requirements without adding rules; DJ1 moved to Export EJ1, and DJ2–DJ5 retain their IDs.

| Journey | Name | Rows exercised | Entry |
| :--- | :--- | :--- | :--- |
| DJ2 | Fix a bad scan by measuring it again | R2.1–R2.9, R3.1/R3.3/R3.5, R7.1/R7.2 | [DJ2](prd-data-foundation-journeys.md#dj2-fix-a-bad-scan-by-measuring-it-again) |
| DJ3 | Open a file made by an older or a newer app | R1.3/R1.5/R1.9, R2.9, R5.1–R5.8, R7.3/R7.7 | [DJ3](prd-data-foundation-journeys.md#dj3-open-a-file-made-by-an-older-or-a-newer-app) |
| DJ4 | Delete a swatch | R6.1–R6.5, R7.2, R7.6k/l | [DJ4](prd-data-foundation-journeys.md#dj4-delete-a-swatch) |
| DJ5 | Query the file without the app | R1.1–R1.6, R2.1–R2.9, R3.1–R3.5, R5.5/R5.6, R7.1 | [DJ5](prd-data-foundation-journeys.md#dj5-query-the-file-without-the-app) |

## Requirements

### Vocabulary

Used unchanged from [AGENTS.md §8](../../../AGENTS.md#8-vocabulary): raw payload, version history, gamut-clipped, canonical value — its raw-payload and canonical-value entries amended to fence F10. Added here:

- **The file** — the one SQLite file holding the user's collections and all the app keeps about them.
- **Sample** — one reading the instrument produced, kept as produced; a tier beneath a reading, never a version-history entry or an earlier reading.
- **Reading** — one saved set of 1–5 samples ([the capture PRD's R1.3](../capture-mode/prd-capture-mode.md#1-collections)), its stored mean the value the app works from; this document's usage is the axis, so [the capture PRD's R4.12](../capture-mode/prd-capture-mode.md#4-the-scan-loop) "readings" are samples here.
- **Re-measurement** — a later reading because the sample differs now; both were true, at different times.
- **Correction** — a later reading because the earlier was never true; that value is superseded and left out of over-time views.
- **Supersession reason** — why a reading superseded the one before: initial, re-measurement, correction, correction-unconfirmed, restore.
- **Derived value** — XYZ, Lab, LCh, Luv, sRGB or HSL, worked out from a reading under a stated illuminant, observer and condition, stamped with a **derivation version**.
- **Quarantined** — a reading whose authoritative measurement data cannot be read reliably; retained and marked, never silently promoted or removed. Archive-only damage is a separate mark ([Damage classification](#damage-classification)).
- **Chosen condition** — the measurement condition of the collection's chosen scan mode ([the capture PRD's R1.10](../capture-mode/prd-capture-mode.md#1-collections)); a reading's working set is the set the app works it from ([R3.1](#3-derived-values-and-gamut-honesty)).
- **Fixture** — a checked-in file at a released file format version that tests run on ([R7.7](#7-verifiability)).
- **Salvage output** — the fresh file [R5.5](#5-migration-and-compatibility) writes what is still readable into.
- **Golden** — [the export PRD's Vocabulary](../export/prd-data-export.md#vocabulary)'s.

### Legend

**Priority.** Build order within v1, not a cut line, and a recommendation rather than a decision: P1 is the second phase, holding the delete undo (fence F16); everything else is P0.

Provisional constants: each is named, carries its OQ id and a candidate until a fence fixes it, and none ships in a release with its OQ open — a dogfood build not being a release. An unqualified "OQ n" or row ID is this document's; DERIVATION_VERSION is a stamp, not a constant, and ROWS_CEILING is the capture PRD's, under [its OQ 13](../capture-mode/prd-capture-mode.md#open-questions).

**Retired row IDs.** Never reuse them; the [retired-ID map](prd-data-foundation-fences.md#retired-id-map) is in the fences companion.

**Status.** Allowed values (questions use open/answered):

- ⌛️ Ready for Alignment — awaiting cross-functional alignment · ✋ Needs Discussion — the team needs the PM · 🤝 Aligned — the team agrees
- 🦺 In Progress — implementation in flight (optional) · ✅ Completed — merged, its PR in Commit PR · ✂️ Deferred — out of this release

### Traceability

All requirement rows target v1; Status tracks each row and Commit PR records implementation, not editorial review.

IDs never change: requirements use `R<section>.<n>`, copy uses `E<n>`, metrics use `M<n>`. Lettered subrows inherit parent priority, release and status; owner decisions live in the [fence companion](prd-data-foundation-fences.md).

### Surfaces

R7.6 tests each surface below; copy IDs identify states, not literal strings.

| ID | Surface | What the test lists | Req-IDs | Copy IDs | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| R7.6b | File-opening states | Which state came up, its file/app/compatibility versions and snapshot path where applicable, an upgrade's progress, the way forward offered, where the salvage output goes, and what the salvage had to settle | R1.5, R5.1–R5.8 | E1, E2, E3, E5, E9, E10, E16, E23, E24, E28–E30 | 🤝 Aligned |
| R7.6c | First-run file location | The default offered, both risks named, and the actions | R1.8 | E13 | 🤝 Aligned |
| R7.6d | File location and switching | The location shown, where the app believes the file sits — a local disk, a syncing folder or a network volume — the actions, each move failure named | R1.3, R1.5, R1.9 | E10, E19–E22 | 🤝 Aligned |
| R7.6e | Sync or network warning | Correct location warning (network when both), both risks, every action, in-flight disabled actions and End session route | R1.7 | E25, E27 | 🤝 Aligned |
| R7.6f | Store-volume-full state | What it names as unwritten, and the actions | R1.10 | E15 | 🤝 Aligned |
| R7.6g | Item detail, data lines | The conditions, derivation version, every mark, the reading count | R2.4, R3.2, R3.5, R5.5 | E4, E11, E31 | 🤝 Aligned |
| R7.6h | Damaged reading or archive | The affected reading/sample/set, variant, current value, readable-history count, other items usable, applicable recovery actions | R2.9, R5.5a–c/f | E4, E31 | 🤝 Aligned |
| R7.6i | Unconfirmed-correction review | The set listed, the bulk answer offered, that it is always reachable | R2.8 | E11, E26 | 🤝 Aligned |
| R7.6j | History cost display | What history holds, and its size | R2.6 | — | 🤝 Aligned |
| R7.6k | Item delete confirmation | Each count for never-scanned, current-only and history-bearing items and the actions, no default delete | R6.2, R6.3 | E8 | 🤝 Aligned |
| R7.6l | Collection delete confirmation | The collection-scale counts and the actions, no default delete | R6.2, R6.3 | E14 | 🤝 Aligned |
| R7.6m | Save-a-copy state | What it offers, the size, where copies stay, the snapshots that exist and the version each goes back to, each copy failure named | R5.1, R5.5, R5.8 | E12, E28–E30 | 🤝 Aligned |
| R7.6n | Vendor-analytics disclosure | That it is present, and what it names | R6.6 | — | 🤝 Aligned |

### Evidence base

Research: [acquisition v2 §6](../../briefs/acquisition-experience-research-results-v2.md#6-durability-crash-safety-and-resumability), [browsing v2 §§7–10](../../briefs/browsing-a-collection-at-scale-research-results-v2.md#7-non-destructive-editing-and-version-history), and [SDK audit](../../briefs/nix-universal-sdk-audit-findings.md). F10/F11/F25 supersede their raw-payload-as-canonical premise; F33 distinguishes archive damage from measurement loss. [SQLite `data_version`](https://sqlite.org/pragma.html#pragma_data_version) can detect other connections' commits, including separate processes, by comparison on the same connection; detection by polling is possible, while SQLite provides no cross-process push notification. This establishes feasibility, not the OQ 14 policy or an implementation choice.

### 1. The file the user owns

| ID | Pri | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- | :--- |
| R1.1 | P0 | One file holds the user's collections and all the app keeps about them, at a place the user chooses and may move, rename, copy and back up. Nothing about a collection or session is kept elsewhere ([the capture PRD's R1.9](../capture-mode/prd-capture-mode.md#1-collections)), save a copy the app makes at the user's instruction or names to them ([R5.1](#5-migration-and-compatibility), [R5.5](#5-migration-and-compatibility), [R5.8](#5-migration-and-compatibility)). | 🤝 Aligned |  |
| R1.8 | P0 | The first launch asks where the file lives before anything else, offering a default accepted in one action ([E13](prd-data-foundation-copy.md#error--state-copy), fence F14). | 🤝 Aligned |  |
| R1.9 | P0 | The app shows where the file is and lets the user move it (disabled during active/paused/halted capture, F38) or open another from inside the app, not only in Finder ([R1.3](#1-the-file-the-user-owns)). The file is at exactly one location at all times: a move that cannot finish — no room, no permission, the destination gone — leaves the original untouched and says which ([E19](prd-data-foundation-copy.md#error--state-copy)–[E21](prd-data-foundation-copy.md#error--state-copy), [R7.8](#7-verifiability)). | 🤝 Aligned |  |
| R1.2 | P0 | Plain SQLite types expose all decoded sample/reading values, conditions, derived sets, canonical selection, supersession reasons, quarantine, per-sample archive-unavailable and unreadable-set marks, readable with the app closed without extensions, app-defined functions or app-only computation (R7.1, fences F11/F25); imported columns retain first-seen resolved names/positions under [Import R2.5/R2.6](../import/prd-inventory-import.md#2-target-mapping-and-the-matching-rule), with later columns appended and decoded values preserved/updated under [Import R3.6](../import/prd-inventory-import.md#3-preview-and-commit) and user edits (R2.3). SQLITE_READER_FLOOR, the lowest SQLite version an outside reader needs, is derived from the features stored (OQ 2), documented in help, and never exceeded by stored features. | 🤝 Aligned |  |
| R1.6 | P0 | A vendor's raw payload is an archived artifact beside the value it belongs to rather than being it, one per sample, and may be compressed ([R2.1](#2-canonical-value-and-version-history), fences F11, F25); no guarantee in [R1.2](#1-the-file-the-user-owns) rests on it. | 🤝 Aligned |  |
| R1.3 | P0 | One file is active (F2), with [Import R2.3](../import/prd-inventory-import.md#2-target-mapping-and-the-matching-rule)'s matching rule applied everywhere: codes unique per collection, collection names unique across the file ([Capture R1.2](../capture-mode/prd-capture-mode.md#1-collections)). Refuse switching during active/paused/halted capture (E22, F34/F45); otherwise precheck the target before closing the original, preserving interrupted sessions ([R7.3b](#file-actions), F44; [Capture R3.5 in-flight trio / R3.2 status enum](../capture-mode/prd-capture-mode.md#3-the-capture-session)). | 🤝 Aligned |  |
| R1.4 | P0 | The app writes only at the user's chosen location and sends none of the file's contents elsewhere; location-managed sync is R1.7's exception. Inject [Device R6.12](../device-management/prd-device-management.md#6-mock-device-layer) reachability and enumerate R6.17 outbound attempts: Demo permits enabled telemetry and user-controlled software-update checks (Device R2.13), live additionally permits vendor authorization/SDK analytics (Device F23), and Device R6.30's simulated-kind double uses that live set for authorization traffic (Device F27); observed requests must be a subset of that configuration's permitted recipients and carry no item, reading or payload. | 🤝 Aligned | PR #19 round-2 revision |
| R1.7 | P0 | Detected sync folders/network volumes show E25/E27 with damage risk and, for sync folders, complete-copy transmission to the provider; offer continue or move, subject to F38, and use the network state for paths in both classes (F13). R1.10 atomicity is not promised there; help says quit before sync and never let two machines write one file, with verified volume classes governed by R7.4 ([File actions](#file-actions)). | 🤝 Aligned |  |
| R1.5 | P0 | The app shows the file as of its last read and an explicit re-read is available whenever no capture is in flight ([E9](prd-data-foundation-copy.md#error--state-copy)); automatic external-write detection and its response remain OQ 14, with re-read disabled during active/paused/halted capture, but not interrupted sessions (F38, OQ 19); the help docs say reading it elsewhere is safe and editing it while the app is open is not. A second running copy is refused by name rather than allowed to write over the first ([E10](prd-data-foundation-copy.md#error--state-copy), [R7.3](#7-verifiability)), and a hold left by a dead process never blocks the owner from opening their file ([File actions](#file-actions)). | 🤝 Aligned |  |
| R1.10 | P0 | On a local volume any write lands whole or not at all: a crash, a power loss or a volume taken away mid-write leaves the file as it was, no half-written reading and no file that will not open ([the import PRD's R3.2](../import/prd-inventory-import.md#3-preview-and-commit), [R1.7](#1-the-file-the-user-owns), [R7.4](#7-verifiability)). A reading is durable before the operator is told it landed ([the capture PRD's R11.10](../capture-mode/prd-capture-mode.md#11-demo-device-and-verifiability), [M1](#success-metrics)), and no room on the store's volume is named rather than silently dropping the write ([E15](prd-data-foundation-copy.md#error--state-copy)) ([File actions](#file-actions)). | 🤝 Aligned |  |

### 2. Canonical value and version history

| ID | Pri | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- | :--- |
| R2.1 | P0 | Keep every sample of a saved set as produced, with decoded spectrum/colour values at R1.2's floor and archived payload under R1.6; the canonical value is the stored mean with conditions, acquiring-device snapshot, averaging basis and derivation version ([Capture R4.12/R4.24](../capture-mode/prd-capture-mode.md#4-the-scan-loop), [Device R1.21](../device-management/prd-device-management.md#1-device-pairing), F10/F25). Each reading has a per-item sequence, measurement time and record time; history orders by record time, over-time views by measurement time, and payload round-trip fidelity remains hardware OQ 15 (per SDK docs). | 🤝 Aligned |  |
| R2.2 | P0 | An item has at most one canonical value at a time, and which reading that is can be told from the file without the app ([R1.2](#1-the-file-the-user-owns), [R7.2](#7-verifiability)); an imported row has none until scanned ([the import PRD's R3.3](../import/prd-inventory-import.md#3-preview-and-commit)). A file carrying two current readings on one item opens read-only, named as such ([E5](prd-data-foundation-copy.md#error--state-copy)), never repaired unasked. | 🤝 Aligned |  |
| R2.9 | P0 | An unreadable reading remains marked in the file, and no historical reading is automatically promoted; only damage to the current reading removes the item's current value ([Damage classification](#damage-classification), fence F33). A re-scan after that loss creates a reading with reason initial and no correction question; restoring a readable earlier reading creates one with reason restore ([R2.3](#2-canonical-value-and-version-history)). | 🤝 Aligned |  |
| R2.3 | P0 | A later reading supersedes it and the prior is kept as version history, never overwritten, never deleted ([the capture PRD's R8.9](../capture-mode/prd-capture-mode.md#8-deferred-row-review-and-corrections), [its R8.13](../capture-mode/prd-capture-mode.md#8-deferred-row-review-and-corrections)); a restore writes a new canonical value equal to an earlier reading, carrying its measurement time, and deletes nothing. The immutability rule covers measurements — the activity record is [R6.5](#6-deletion-and-privacy)'s — and deleting the item is the only act that removes a reading ([§6](#6-deletion-and-privacy)); metadata and notes are editable and clearable instead, an edit leaving no prior text and a clear putting it beyond an outside reader's recovery (fence F17, [R7.2](#7-verifiability)). | 🤝 Aligned |  |
| R2.4 | P0 | Every reading records the supersession reason — initial, re-measurement, correction, correction-unconfirmed, restore — and the app asks rather than guessing ([E11](prd-data-foundation-copy.md#error--state-copy)). It asks once, outside the heads-down loop, a capture-time re-scan over a current value being correction-unconfirmed until answered (no current value follows R2.3a) (fences F3, F12). (Inherited obligation for the Capture Mode PRD.) | 🤝 Aligned |  |
| R2.5 | P0 | A reading superseded by a confirmed correction is marked never true and excluded from every over-time view of that item; one superseded by a re-measurement stays a legitimate earlier point, and one still unconfirmed is shown and marked rather than dropped. (Inherited obligation for the Collection Mode and QC & Comparison PRDs.) | 🤝 Aligned |  |
| R2.8 | P0 | Every reading whose correction is unconfirmed is enumerable, the set reachable from the app at any time and from each item and offered whole after a session so it can be answered in one pass ([E11](prd-data-foundation-copy.md#error--state-copy), [E26](prd-data-foundation-copy.md#error--state-copy), [R7.1](#7-verifiability)). Nothing ever expires an unconfirmed reading into a confirmed correction. | 🤝 Aligned |  |
| R2.6 | P0 | Version history is kept for HISTORY_RETENTION — for good, nothing aged out (fence F6) — and [M6](#success-metrics)'s corpus stays within STORE_SIZE_BUDGET (OQ 5), a design target, never a runtime cap on the user's file. The app can enumerate what history holds and show its cost in megabytes on disk. | 🤝 Aligned |  |
| R2.7 | P0 | A QC comparison reads the canonical value and never becomes it, however far the verdict falls; its reading is kept as a record of its own, not a supersession, and records no supersession reason ([U4](../vision.md#use-cases), [R2.4](#2-canonical-value-and-version-history)). It is not counted among an item's readings ([R6.2](#6-deletion-and-privacy)). (Inherited obligation for the QC & Comparison PRD.) | 🤝 Aligned |  |

#### Measurement operations

A = old current reading; B = new reading; H = readable history source; R2.3a–i specify operations, not schema.

| ID | Operation | Current value and history | Reason and time oracle |
| :--- | :--- | :--- | :--- |
| R2.3a | First saved set, including after current-value quarantine | Create B from 1–5 samples and their stored mean; keep any quarantined record | B = initial; new measurement and record times; no correction question |
| R2.3b | Re-scan during capture, except no current value (R2.3a) | Create B as current; retain A and show it as unsettled in over-time views | B = correction-unconfirmed; no question mid-loop |
| R2.3c | Answer that the swatch changed | Retain both readings as legitimate time points; no new reading from answering | Incoming reason becomes re-measurement; measurement/record times unchanged |
| R2.3d | Answer that the old reading was wrong | Retain A, mark A never true, exclude A from over-time views and QC references | Incoming reason becomes correction; measurement/record times unchanged |
| R2.3e | Defer the answer | Keep the unresolved relationship enumerable through E11/E26; no expiry | Reason remains correction-unconfirmed |
| R2.3f | Restore H | Create B equal to H as current with its own copies of H's samples/decoded values; compute six spaces fresh at current DERIVATION_VERSION under collection reference, subject to R3.5; H, its sets and history unchanged | B = restore; H's measurement time, new record time and sequence |
| R2.3g | QC measurement | Keep its independent comparison record; leave current selection/history unchanged | No supersession reason; not part of item reading counts |
| R2.3h | Record a supersession | Reason belongs to incoming reading and describes its predecessor relationship | Never-true mark belongs to predecessor |
| R2.3i | Answer B in A → B → C | Answer addresses A even when C is current; never promote B | No new reading or timestamp change |

### 3. Derived values and gamut honesty

| ID | Pri | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- | :--- |
| R3.1 | P0 | Each reading, including history, has six derived spaces (XYZ, Lab, LCh, Luv, sRGB, HSL), none canonical, subject to R3.5's missing-condition exception. Sets are keyed by reading/condition with one live derivation and older sets retained as superseded; the collection's chosen scan mode selects its working set, while non-chosen sets are computed on request, persisted and regenerated ([Regeneration matrix](#regeneration-matrix), fences F27/F28/F31; OQ 15 checks toolkit coverage on hardware). | 🤝 Aligned |  |
| R3.2 | P0 | A derived value never stands alone: it carries its illuminant, observer, measurement condition, and derivation version, so it can be reproduced or refuted. | 🤝 Aligned |  |
| R3.3 | P0 | DERIVATION_VERSION changes only when the derivation algorithm changes; regeneration is never an upgrade the user must accept and follows [R3.3a–g](#regeneration-matrix), retaining superseded sets and never altering measurements, raw payloads or agreement verdicts (R3.1, R3.5). Every run is resumable, with stale or mismatched working sets enumerable through R7.1; partial-run presentation and competing requests require OQ 17 to close before that workflow is built. | 🤝 Aligned |  |
| R3.4 | P0 | The gamut-clipped flag asserts one thing and says which: this value's sRGB derivation fell outside GAMUT_REFERENCE_SPACE under GAMUT_RENDERING_INTENT (sRGB and relative colorimetric — fence F4), fixed on the reading rather than on whichever display is attached, and never a licence to substitute the nearest renderable colour. The mark travels into export ([the export PRD's R2.1](../export/prd-data-export.md#2-columns-names-dialect-and-the-version)) and every view; what a given display can render is a separate live question. (Inherited obligation for the Collection Mode PRD.) | 🤝 Aligned |  |
| R3.5 | P0 | A reading with no spectral curve ([the capture PRD's R4.24](../capture-mode/prd-capture-mode.md#4-the-scan-loop)) still carries all six spaces, worked out under the fixed reference it was taken at, marked non-spectral and not re-derivable under another illuminant or observer. Only where a condition is missing is a derived value absent and marked so rather than filled with a plausible number, what the export writes being [the export PRD's R2.1](../export/prd-data-export.md#2-columns-names-dialect-and-the-version)'s ([R7.2](#7-verifiability)). (Inherited obligation for the Collection Mode PRD.) | 🤝 Aligned |  |

#### Regeneration matrix

Applies to current/history readings; sets include six spaces, conditions, version and gamut result. R7.5 tests each applicable row with interruption/resume and R7.1 read-back.

| ID | Trigger / input | Required result |
| :--- | :--- | :--- |
| R3.3a | Algorithm changes, or derived-set damage (R5.5f) | Algorithm changes bump DERIVATION_VERSION and regenerate all eligible sets; R5.5f regenerates damaged sets at the current version without a bump; include non-chosen sets and fixed-reference non-spectral sets, exclude R3.3g, retain prior sets as superseded, with one live set per reading/condition |
| R3.3b | Change illuminant or observer; spectral reading | Regenerate under the selected reference at the same DERIVATION_VERSION; retain old sets, do not re-scan |
| R3.3c | Change chosen scan mode; spectral measurement exists in that condition | Select/create its working set and regenerate existing sets under the collection reference at the same version; retain other conditions' sets |
| R3.3d | Chosen condition has no measurement | Mark the working value absent; never substitute another condition's value |
| R3.3e | Change collection reference; non-spectral reading | Keep six spaces at the original reference; mark non-spectral and reference mismatch when applicable, never fabricate a new illuminant/observer derivation |
| R3.3f | Request a non-chosen condition's values | Compute on demand when that measurement exists, persist the set, and include it in subsequent regenerations |
| R3.3g | Authoritative measurement unreadable (R5.5b/c) | Do not regenerate; retain sets/stamp; omit from completion enumerations and enumerate separately as excluded by damage |

An interrupted run preserves original measurements and prior sets. Completion excludes R3.3g's damaged readings and means no eligible live set on an older algorithm stamp and no unresolved spectral working-set mismatch (a missing condition must have an absent mark); the non-spectral reference mismatch is an intentional exception, not unfinished work.

### 5. Migration and compatibility

| ID | Pri | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- | :--- |
| R5.6 | P0 | The file states its own format version in one place and form that never changes from release to release, readable at SQLITE_READER_FLOOR before anything else in it is interpreted ([R1.2](#1-the-file-the-user-owns), [R7.1](#7-verifiability)). This is the file format version, a different number from the export format version ([the export PRD's R2.5](../export/prd-data-export.md#2-columns-names-dialect-and-the-version)). | 🤝 Aligned |  |
| R5.7 | P0 | State the oldest file version this app upgrades; v1 writes the first real format, and later below-floor files open read-only instead of upgrading (test-only older fixture: R7.7m). E16 and R5.2's lossy-refusal E23 name file/app versions and the last app version that can change the file; R7.3 asserts them. | 🤝 Aligned |  |
| R5.1 | P0 | Before upgrading an older file, safely write and verify an openable snapshot, tell the user its path, then show upgrade progress (E2, R7.6b). Both this snapshot and the user-requested E12 copy use a method documented safe for an open database, not plain copying; help explains it and tests declare a write in flight, then independently open both copies at SQLITE_READER_FLOOR with the app closed and read every reading (R7.1/R7.2, M5). | 🤝 Aligned |  |
| R5.8 | P0 | Keep pre-upgrade snapshots until the user removes them and list them in E12 with their recovery app version; copies retain data deleted/cleared since, and a test asserts an existing snapshot survives an app run and another upgrade (R7.2). No room for a pre-upgrade snapshot produces E24 naming the space needed without starting migration; failed copy/snapshot/salvage leaves no partial output, names no-room/no-permission/destination-gone through E28–E30, and is never listed as recovery (R7.3). | 🤝 Aligned |  |
| R5.2 | P0 | An upgrade lands whole or not at all: one failing partway leaves the file as it was, names the version found against the one expected, and points at the snapshot ([E3](prd-data-foundation-copy.md#error--state-copy)). An upgrade that would lose a reading is refused rather than performed, the reading named and every reading, mark and note in the file reading back identical at SQLITE_READER_FLOOR ([E23](prd-data-foundation-copy.md#error--state-copy), [R7.3](#7-verifiability)). | 🤝 Aligned |  |
| R5.3 | P0 | A newer file opens read-only (E1), never partially interpreted as complete; E1/E16/E23/E24 read-only views show at least the item list, canonical values and history marks (F39). E1 points to updating the app; OQ 18 covers additional capabilities, with existing export restrictions retained. | 🤝 Aligned |  |
| R5.4 | P0 | The app checks the file on open within INTEGRITY_CHECK_BUDGET (candidate ≤ 1 s at ROWS_CEILING ([the capture PRD's OQ 13](../capture-mode/prd-capture-mode.md#open-questions)) — this document's OQ 13, [M8](#success-metrics)), reserving the slower, fuller check for a user's request or a failed quick one, and never repairs file-level damage unasked (derived-set regeneration: R5.5f). | 🤝 Aligned |  |
| R5.5 | P0 | Handle file-level damage, isolated measurement damage, archive-only damage and failed upgrades according to [Damage classification](#damage-classification), with file-level source damage never repaired unasked (F33/F47). Salvage writes readable data to a user-chosen fresh file, retains it until the user removes it, resolves source invariants as specified there, and follows [File actions](#file-actions) for completion and failures. | 🤝 Aligned |  |

#### Damage classification

R7.2/R7.3 induce each damage class independently. An archive the instrument never supplied is not corruption; persisted derived-set damage is R5.5f.

| ID | Damage | Result and recovery |
| :--- | :--- | :--- |
| R5.5a | Archived vendor payload only; stored mean and decoded measurements intact | Persistently mark the affected sample archive unavailable (E31); retain its bytes, canonical selection, decoded values and derived sets; the collection remains usable; export runs with unavailable payload cells empty, no added CSV column/state (F37, OQ 21) |
| R5.5b | Current reading's authoritative mean or decoded measurement data unreadable | Retain and persistently quarantine that reading (E4 current variant); item has no current value, other items remain usable; offer re-scan or restore of a readable earlier reading, never automatic promotion |
| R5.5c | Historical reading's authoritative measurement unreadable; current reading intact | Retain and persistently quarantine only the damaged history (E4 history variant); keep current selection and values; do not offer the damaged reading as a restore source |
| R5.5d | File-level damage or invariant violation, including two current readings | Open source read-only (E5); salvage only on request to a fresh file, leaving the source intact; output must be complete for readable data and open read-write with each resolution named; of two current readings, the later-recorded stays current, the other retained as correction-unconfirmed (R2.2, R2.4) |
| R5.5e | Upgrade fails or would lose a reading | R5.2 applies: source unchanged, E3/E23, never proceed with a lossy upgrade |
| R5.5f | Persisted derived set unreadable; measurement intact | Mark unreadable and automatically regenerate from intact measurement under R3.3a at current DERIVATION_VERSION without a bump; retain the damaged set as superseded, with canonical selection and reading marks unchanged (F47) |

Per-invariant rules, including equal record times, belong to ADR-0003 without narrowing the general salvage promise (F36).

#### File actions

Retry revalidates source, destination and gates; picker cancellation starts nothing; read-only/refused paths preserve the source.

| ID | State / action | Outcome |
| :--- | :--- | :--- |
| R7.3a | Active, paused or halted capture; open another file | E22, current file/session unchanged; Cancel dismisses, Go to the session returns to that session; pausing does not lift the gate |
| R7.3b | No in-flight capture (including a persisted interrupted session); open another file | Check target version/integrity/ownership before closing original; failure leaves original open and shows target state; on acceptance close original and open target, preserving interrupted sessions without resuming them |
| R7.3c | E9 Read it again | Re-read the same file through applicable integrity/version checks and refresh from persisted data; not an automatic repair or merge of external edits; disabled during active/paused/halted capture, enabled after interruption; Go to the session routes to [Capture’s End session](../capture-mode/prd-capture-mode.md#7-pause-end-interruption-and-resume), or [Device R5.11](../device-management/prd-device-management.md#5-mid-session-device-failure) from a halt (F38) |
| R7.3d | E10 Try again / Open a different file | Recheck the holder / run the file-selection path; a dead process's stale hold cannot prevent open |
| R7.3e | E3 or E24 Try again | Recheck snapshot prerequisites, confirm an openable snapshot, then attempt the whole upgrade again; another failure leaves source unchanged |
| R7.3f | E3 Show me the copy; E23/E24 Open it read-only; E1/E23 Check for updates | Reveal the confirmed snapshot / enter read-only without migration / assert the update-check action leaves the file unmodified, without selecting an updater; read-only floor is R5.3 |
| R7.3g | E5 Save what's readable / Leave it alone | Choose a fresh output destination and run R5.5d / dismiss without writing; successful salvage lists its path and resolutions, failure E28–E30 leaves no partial output |
| R7.3h | E12 Save a copy | Choose destination, take a snapshot safe during writes, verify openable, then list it; E28–E30 on failure, never list a failed copy as recovery |
| R7.3i | E19–E21 or E28–E30 Try again / Choose somewhere else | Retry the same operation and destination / select another destination; preserve whether the operation was move, copy, pre-upgrade snapshot or salvage, and never start an upgrade before its snapshot succeeds |
| R7.3j | E15 Try again / Move my file; E25/E27 Move my file / Keep it here | Retry the unsaved operation after revalidation / run R1.9's move / acknowledge the location risks; move is disabled during active/paused/halted capture, enabled after interruption; failure leaves source unmoved; Go to the session routes to [Capture’s End session](../capture-mode/prd-capture-mode.md#7-pause-end-interruption-and-resume), or [Device R5.11](../device-management/prd-device-management.md#5-mid-session-device-failure) from a halt (F38) |
| R7.3k | E4 current variant: Scan again / Use a previous reading | Start the existing capture/re-scan flow with its normal session gates / select a readable earlier reading and create a new restore reading; offer restore only when such a reading exists |

R7.3 tests failures/cancellation, including target precheck failure leaving the original open. R7.6b asserts each state's file/app/compatibility versions and snapshot path as applicable, independently of literal wording.

### 6. Deletion and privacy

| ID | Pri | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- | :--- |
| R6.1 | P0 | The user may delete an item or a collection and nothing else: a single reading cannot be deleted out of an item's history. Throwing away the whole file is Finder's job, which the help docs say, and delete is never the default action on any surface (fence F15). | 🤝 Aligned |  |
| R6.2 | P0 | Before deleting, E8/E14 state item/collection-scale counts of current and historical readings (not samples), attempts and decisions, and whether undo is available (R7.2). Export first leaves deletion unperformed and returns to confirmation: canonical export plus a complete-copy option until [Export R1.3](../export/prd-data-export.md#1-what-the-export-contains)'s history export lands; final deleted content is unrecoverable from the active file by an outside reader ([Capture R1.8](../capture-mode/prd-capture-mode.md#1-collections)). | 🤝 Aligned |  |
| R6.3 | P1 | At P1, an item or collection delete is undoable during the current open-file lifetime; quit, crash or closing that file ends DELETE_UNDO_WINDOW and makes the deletion final (fence F7, clarified by F35). Undo restores the deleted content and history intact, subject to [Deletion lifecycle](#deletion-lifecycle); it does not create empty replacement rows. | 🤝 Aligned |  |
| R6.4 | P0 | Removing a saved device never alters, orphans or cascades into a measurement: the acquiring device's snapshot is part of the reading and outlives the device record ([the device PRD's R1.21](../device-management/prd-device-management.md#1-device-pairing)). A test removes a saved device and reads back every snapshot unchanged. | 🤝 Aligned |  |
| R6.5 | P0 | The file's closed privacy inventory is the [Device R1.21](../device-management/prd-device-management.md#1-device-pairing) snapshot, [Capture R8.13](../capture-mode/prd-capture-mode.md#8-deferred-row-review-and-corrections) activity, user-entered data, and measuring-build-only [Capture R11.16](../capture-mode/prd-capture-mode.md#11-demo-device-and-verifiability) interaction records (F32, Capture OQ 24): session-scoped, outside deletion, retained untouched when a release opens a measuring-build file; no account/machine data. Tests record build configuration, assert no interaction record in release-written files, and no vendor credential in the store at SQLITE_READER_FLOOR or any export ([Export R4.2](../export/prd-data-export.md#4-verifiability), R7.1). | 🤝 Aligned |  |
| R6.6 | P0 | v1 does not ship without a disclosure of the vendor SDK's own analytics naming the recipient, the events (connect, scan, calibration), that they fire per event and cannot be switched off, and that they are separate from the app's own telemetry (fence F19, OQ 12). (Inherited obligation for the Telemetry PRD and the help docs: the wording theirs, the gate and content floor here.) | 🤝 Aligned |  |

#### Deletion lifecycle

R6.2a–d specify deletion; undo cases require P1 R6.3. “Session” means open-file lifetime, not capture session.

| ID | Event | Oracle |
| :--- | :--- | :--- |
| R6.2a | P0 delete confirmed | Delete final; item/collection content is unrecoverable from the active file by an outside reader |
| R6.2b | P1 delete, then undo before file closes | Restore content, samples, readings, history, attempts and decisions; no new measurement identities; pending-undo outside-reader visibility and identity conflicts are gated by OQ 20 |
| R6.2c | P1 delete, then quit, crash or close file (including switching files) | Undo lost, delete stands; reopen does not resurrect it and the final-deletion oracle applies |
| R6.2d | Clear a note or imported value | Prior text is unrecoverable from the active file under R2.3; test each independently |

R5.8 copies and pre-upgrade snapshots retain their prior contents; deletion and clearing do not rewrite them. R6.5's measuring-build interaction record is session-scoped and explicitly outside item/collection deletion; saved-device removal independently leaves all measurement snapshots intact (R6.4).

### 7. Verifiability

Additional tests: R1.4 outbound attempts; R5.1 safe copies; R5.8 snapshot retention; R6.4 device removal.

| ID | Pri | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- | :--- |
| R7.1 | P0 | With the app closed, tests using only SQLITE_READER_FLOOR match its format version, item identity/conditions, current/history derived values with illuminant/observer/condition/version, decoded samples, canonical selection, supersession reasons, never-true/gamut/quarantine/archive-unavailable/unreadable-set marks, and imported names/positions/values (R1.2, R2.4/R2.8). Enumerate R3.3g damage exclusions separately; among eligible readings enumerate live older-version sets and spectral working-set reference mismatches, including missing sets without an absent mark (R3.3/R3.5). | 🤝 Aligned |  |
| R7.2 | P0 | Tests declare rather than navigate to invalid and boundary states: two current readings, a derived value missing its illuminant/observer/condition/version stamp, an absent value for a missing condition, R5.5a–c/f damage, P1 undo pending, existing pre-upgrade snapshot, read-only file, and launch contexts (no file; local/sync/network path; moved file; write in flight). They exercise R2.3a–i, asserting incoming reasons and that answering changes neither reading count nor time axes, plus named states, deletion counts, export-first leaving deletion unperformed and confirmation open, and R6.2a–d's deletion/clear/undo outcomes at SQLITE_READER_FLOOR, including pending visibility once OQ 20 defines it ([Capture R11.6](../capture-mode/prd-capture-mode.md#11-demo-device-and-verifiability)). | 🤝 Aligned |  |
| R7.3 | P0 | Tests induce every [File actions](#file-actions) and [Damage classification](#damage-classification) case plus older/newer formats and below-floor once a release raises the floor (E16’s ‹floor› convention), mid-upgrade failure, lossy-upgrade refusal, snapshot/store full, source disappearance ([Capture E26](../capture-mode/prd-capture-mode-copy.md#error--state-copy)), live/dead ownership holds, and copy/snapshot/salvage failures or crashes. Assert named states without matching copy, no partial outputs or missing files, unchanged source readings/marks/notes on refusal/read-only paths at SQLITE_READER_FLOOR, R5.3’s browsing floor, and salvage complete for readable data and opening read-write with named invariant resolutions. | 🤝 Aligned |  |
| R7.4 | P0 | A test loses everything not yet safely written, at any moment it chooses, and reads back every reading the operator was told landed ([the capture PRD's R11.10](../capture-mode/prd-capture-mode.md#11-demo-device-and-verifiability), [R1.10](#1-the-file-the-user-owns)). The guarantee is proved on local volumes every PR, the same harness timing the file-opening quick check on [R7.7](#7-verifiability)'s corpus at ROWS_CEILING every release ([R5.4](#5-migration-and-compatibility), [M8](#success-metrics)); USB external and network volumes are `needs-hardware-verify`, each run recording its class ([M1](#success-metrics), [R1.7](#1-the-file-the-user-owns)). | 🤝 Aligned |  |
| R7.5 | P0 | A test checks derived values against DERIVATION_TOLERANCE over checked-in, independently known values from a published source named by the hardware spike, including gamut verdicts and boundary values (OQ 6, M2, M7); until that reference lands, the other assertions still run. It exercises every [Regeneration matrix](#regeneration-matrix) case, including illuminant, observer and chosen-mode changes, with interruption/resume that skips healthy sets already at the target version/reference, separately listed damage exclusions, retained superseded sets, unmodified measurements/payloads and the defined completion enumerations. | 🤝 Aligned |  |
| R7.6 | P0 | A test lists what each surface offers and shows, without matching wording; the [Surfaces](#surfaces) table is the rule, and a surface whose rows are all P1 is listed once they land. Rows R7.6c–R7.6e assert against [R7.2](#7-verifiability)'s declared launch context. | 🤝 Aligned |  |
| R7.7 | P0 | Check in every released-format fixture plus fabricated prior-version R7.7m; simulated snapshots use [Device R6.9](../device-management/prd-device-management.md#6-mock-device-layer), live-kind snapshots are hand-authored (F41); Device R6.30's live-flow double emits only simulated-provenance snapshots (Device F20). [R7.7a–n](#fixture-matrix) own the inventory; generate ROWS_CEILING with second-condition sets per reading for M6/M8 and [Export R4.1](../export/prd-data-export.md#4-verifiability)’s header-only golden, ADR-0003 owning determinism and Export R4.2 assertions. | 🤝 Aligned | PR #19 review decisions |
| R7.8 | P0 | A test induces each of the move's failures on demand — no room, no permission, the destination gone, a failure mid-write, which resolves to one of the three by cause ([R1.9](#1-the-file-the-user-owns), [E19](prd-data-foundation-copy.md#error--state-copy)–[E21](prd-data-foundation-copy.md#error--state-copy)) — observing which named state came up, that nothing partial was left behind, and that no file went missing. The export destination's failures are [the export PRD's R4.3](../export/prd-data-export.md#4-verifiability)'s. | 🤝 Aligned |  |

#### Fixture matrix

Sole R7.7/Data Export inventory; cases may share fixture files. Tests may mutate fixtures to declare R7.2/R7.3 invalid states.

| ID | Fixture coverage | Required consumer / oracle |
| :--- | :--- | :--- |
| R7.7a | Imported column colliding with export names; another whose rename collides again | Export R2.4/R4.2; literal `import_` name plus case/whitespace variants (`SC_Simulated`, ` sc_simulated `), each compared under Import R2.3 |
| R7.7b | Imported value the CSV dialect would reshape; column mapped to identity | Import → store → export preserves decoded values, resolved names and positions; exported quoting follows Export R2.2/R2.4 |
| R7.7c | Missing measurement condition and absent derived value | Explicit absence, no substituted number; Export R2.1 |
| R7.7d | Non-spectral reading | Six spaces at fixed reference and non-spectral/mismatch marks; R3.3e |
| R7.7e | Mock-supplied simulated snapshot and hand-authored live-kind snapshot | Preserve simulation identity; Export R4.2 asserts all three `sc_simulated` values without hardware (empty via R7.7h) |
| R7.7f | Gamut-clipped reading | Stored reference/intent and visible/exported flag; R3.4 |
| R7.7g | Confirmed correction and unresolved A → B → C history, plus restore | No lost readings; predecessor marks/times per R2.3a–i; restore owns copied samples/decoded values and fresh current-version derived spaces under collection reference (R3.5 exceptions), source unchanged |
| R7.7h | Never-scanned item; item with exactly one reading and no history | No canonical value for unscanned item; E8 never claims a current/earlier reading that does not exist; R7.6k counts readings, not samples |
| R7.7i | Archive-only damage; unused slots/sample with no instrument-supplied payload; damaged current with/without readable history; damaged history with healthy current; damaged derived set | R5.5a–c/f SQL-readable marks and current-selection outcomes; derived-set case: regenerated set readable, damaged set retained and marked, current selection unchanged; archive/quarantine export per F37 |
| R7.7j | Second condition's derived set | Persist and regenerate chosen/non-chosen sets; R3.3a–g |
| R7.7k | Collection imported into twice | Existing column positions preserved, new columns appended; Import R3.3/R3.6 preserve measurements/history |
| R7.7l | Queue order different from insertion order | Read-back and Export R1.1g/R2.3 follow queue order |
| R7.7m | Fabricated version below current; test-only supported reader/migration, not a production compatibility-floor change | From v1, exercise R5.1/R5.2, E2/E3/E23/E24 and M5: snapshot, success, failure, lossy refusal and no snapshot space |
| R7.7n | Newly created empty collection with no imported columns | Export R4.1h header-only golden and R1.1r preview |

### Inherited obligations

Each obligation binds; the cited source row is authoritative, and an obligation without a row is inherited whole.

**What other PRDs impose on this one**

| Source PRD | Obligation | Rows here |
| :--- | :--- | :--- |
| Capture Mode | Both Data Foundation lines of [its obligations table](../capture-mode/prd-capture-mode.md#inherited-obligations), carried whole; every mode a reading arrives with is kept ([its R4.5](../capture-mode/prd-capture-mode.md#4-the-scan-loop)) and the collection's chosen scan mode is the one a colour value is worked out from ([its R1.10](../capture-mode/prd-capture-mode.md#1-collections)); Capture F52/F56 separates sample-to-mean agreement at D50/2° from display derivation, and F53/F57/F66 counts each linked session’s own elapsed time/outcomes once, never inherited cumulative totals, while summary tallies reflect collection state; Capture R11.6/R11.11 own the declared/readable row and guard counters, partial sets and row evidence, and labelled own-session versus summary figures with the summary’s single-session/resumed-chain basis, inherited here without extending R7.1’s own list | [R1.1](#1-the-file-the-user-owns), [R2.1](#2-canonical-value-and-version-history), [R2.3](#2-canonical-value-and-version-history), [R3.1](#3-derived-values-and-gamut-honesty), [R3.2](#3-derived-values-and-gamut-honesty), [R3.4](#3-derived-values-and-gamut-honesty), [R6.5](#6-deletion-and-privacy), [R7.1](#7-verifiability), [R7.2](#7-verifiability), [R7.4](#7-verifiability) |
| Device Management | [Its R1.21](../device-management/prd-device-management.md#1-device-pairing)'s snapshot on every measurement, independent of the saved-device records whose removal never cascades into it; a simulated device never occupies a live device's record, even on the same serial ([its R1.22](../device-management/prd-device-management.md#1-device-pairing)) | [R1.1](#1-the-file-the-user-owns), [R2.1](#2-canonical-value-and-version-history), [R6.4](#6-deletion-and-privacy) |
| Device Management, outbound attempts | R1.4 uses Device R6.12 reachability and R6.17 attempt observations; both live and Demo permit user-controlled update checks under Device R2.13/F23, with no item/reading/payload transmitted | R1.4 |
| Data Export | [R4.1/R4.2](../export/prd-data-export.md#4-verifiability) assert goldens on [R7.7a–n](#fixture-matrix), including empty R7.7n; R6.5 forbids exported credentials; Export R4.3 checks its R1.1n refusal states declared by R7.2 and induced by R7.3 | [R6.5](#6-deletion-and-privacy), [R7.2/R7.3](#7-verifiability), [R7.7](#fixture-matrix) |
| Inventory Import | The one matching rule for codes, collection names and headers; field preservation, resolved column names and stable positions ([its R2.2/R2.3/R2.5/R2.6](../import/prd-inventory-import.md#2-target-mapping-and-the-matching-rule)); decoded values remain directly queryable; imports land whole or not at all and preserve canonical values/history on re-import ([its R3.2/R3.3](../import/prd-inventory-import.md#3-preview-and-commit)) | [R1.2](#1-the-file-the-user-owns), [R1.3](#1-the-file-the-user-owns), [R1.10](#1-the-file-the-user-owns), [R2.2](#2-canonical-value-and-version-history), [R2.3](#2-canonical-value-and-version-history), [R7.1](#7-verifiability) |

Capture obligation map (Capture → DF): R1.10 → R3.1; R4.5 → R2.1; R1.9/R3.1/R3.6/R3.8/R6.7/R7.12/R7.18 (session state and ordering) → R1.1; R4.6 → R3.2; R4.11/R4.12/R4.24 (samples, spread, basis) → R2.1; R4.13 → R7.4; R8.5/R8.9/R8.13/R8.17 → R2.3/R6.5; R11.6/R11.10/R11.11 → R7.1/R7.2/R7.4; R11.16 → R6.5 (F32); the second, gamut obligation → R3.4. Capture R6.9 passes through to Collection Mode; Capture R8.9 is P1 but its record shape is P0 under Capture F24, matching DF R2.3.

**What this PRD imposes on others**

| Target PRD | Obligation | Rows |
| :--- | :--- | :--- |
| Data Export | Export reads: [R1.2](#1-the-file-the-user-owns)'s floor and imported-column positions (F25); [R2.1](#2-canonical-value-and-version-history)'s snapshot, basis, sequence and both time axes; [R2.4](#2-canonical-value-and-version-history)'s supersession reason; [R2.2](#2-canonical-value-and-version-history) and [R2.9](#2-canonical-value-and-version-history) on current selection/absence; [R6.5](#6-deletion-and-privacy)'s credential in neither file nor export; and [R3.1](#3-derived-values-and-gamut-honesty)'s current chosen-condition set per reading, including history, absent only for missing measurement; non-chosen sets retained, not exported (F27, F28, F31). [R5.5a–c](#damage-classification): unavailable sample archives export empty payload cells; quarantined readings emit `quarantined`, including superseded history; export never writes the source (F30/F37); payload cells contain the original vendor string, decompressed and byte-identical after CSV unquoting (Export F30) | [R1.2](#1-the-file-the-user-owns), [R2.1](#2-canonical-value-and-version-history), [R2.2](#2-canonical-value-and-version-history), [R2.4](#2-canonical-value-and-version-history), [R2.9](#2-canonical-value-and-version-history), [R3.1](#3-derived-values-and-gamut-honesty), [R6.5](#6-deletion-and-privacy), [R5.5a–c](#damage-classification) |
| Collection Mode | [R2.5](#2-canonical-value-and-version-history)'s three over-time rules; the gamut flag and the absent-value mark are properties of the reading, any display-relative check that document's own; a single reading is never deletable out of history, delete is never a default action, and history is reachable from an item without costing the primary view; [the capture PRD's R6.9](../capture-mode/prd-capture-mode.md#6-queue-navigation-and-reordering)'s rule that codes compare the same way wherever they are ordered, carried by no row here | [R2.5](#2-canonical-value-and-version-history), [R3.4](#3-derived-values-and-gamut-honesty), [R3.5](#3-derived-values-and-gamut-honesty), [R6.1](#6-deletion-and-privacy) |
| QC & Comparison | A comparison reads the canonical value and never becomes it, its own reading kept as a record of its own rather than a supersession; a value superseded by a confirmed correction is never a comparison's reference | [R2.5](#2-canonical-value-and-version-history), [R2.7](#2-canonical-value-and-version-history) |
| Capture Mode | A re-scan over a current value records correction-unconfirmed without asking mid-loop; no-current recovery follows R2.3a; DF E11/E26 supply review until QC & Comparison consumes Capture E29 | [R2.4](#2-canonical-value-and-version-history), [R2.9](#2-canonical-value-and-version-history) |
| Telemetry, help docs | The vendor SDK's own usage analytics disclosed in plain language, separately from any telemetry the app sends; the wording is theirs, this document carrying the release gate and the content floor (fences F8, F19) | [R6.5](#6-deletion-and-privacy), [R6.6](#6-deletion-and-privacy) |

Historical sibling amendments: [fence companion](prd-data-foundation-fences.md#sibling-amendment-map); current sibling rows are authoritative.

### 8. Error & State Copy

[Shipping copy](prd-data-foundation-copy.md) defines placeholders, conditional variants and sibling-owned states; tests assert state identity independently of wording. R7.6's “—” cells are the numeric history-cost display and Telemetry/help-owned disclosure. E4, E10, E11, E31 and E28–E30 each appear on two surfaces.

## Success Metrics

Numeric targets are proposals.

| ID | Metric | Definition (start event, end event, statistic, population) | Candidate target | Method | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| M1 | Confirmed readings lost | Start: a row-success confirmation. End: the file read back after an induced loss of everything not safely written. Statistic: the count not readable back. Population: every induced-loss run, each recording its volume class. | 0, read per volume class | [R7.4](#7-verifiability) | 🤝 Aligned |
| M2 | Derived-value fidelity | Start: a reference payload with known values. End: the app's derived value. Statistic: the largest ΔE2000 across the set. Population: the fixed reference set, every PR. | ≤ DERIVATION_TOLERANCE, candidate ΔE2000 0.1 (OQ 6) | [R7.5](#7-verifiability) | 🤝 Aligned |
| M4 | Answers without the app | Start: the file, app closed. End: four questions answered at SQLITE_READER_FLOOR — the file format version; an item's canonical Lab; which reading is current, with each superseded reading's reason and never-true mark; which values are gamut-clipped. Statistic: how many are correct against what the app shows. Population: each release. | 4 of 4 | [R7.1](#7-verifiability) | 🤝 Aligned |
| M5 | Upgrades that neither lost nor half-finished | Start: an update opening an older file. End: the upgrade finishing or refusing. Statistic: the share that completed with every reading readable back, or left the file untouched. Population: every induced upgrade across [R7.7](#fixture-matrix)'s released and fabricated-prior-version fixtures, one made to fail partway. | 100% | [R7.3](#7-verifiability), [R7.7](#7-verifiability) | 🤝 Aligned |
| M6 | File size at scale | Start: a corpus at two population points — a typical collection of 1,000 items, and one at ROWS_CEILING — each × 10 saved sets × N samples at the averaging default (N = 3, [the capture PRD's R1.3](../capture-mode/prd-capture-mode.md#1-collections)) × six derived spaces per reading × the conditions kept per reading (the chosen condition plus one, as [R7.7](#7-verifiability)'s second-condition fixture holds). End: the file on disk. Statistic: bytes at each point. Population: that corpus after one derivation regeneration, re-measured whenever what is stored changes. | ≤ STORE_SIZE_BUDGET, read at ROWS_CEILING (OQ 5) | [R7.7](#7-verifiability) | 🤝 Aligned |
| M7 | Gamut-verdict accuracy | Start: a reference payload whose gamut verdict is independently known. End: the app's mark. Statistic: the share matching, boundary values counted apart. Population: the fixed reference set, every PR. | 100% | [R7.5](#7-verifiability) | 🤝 Aligned |
| M8 | Open-file check time | Start: the app opening a file. End: the quick check finishing. Statistic: the slowest run. Population: [R7.7](#7-verifiability)'s corpus at ROWS_CEILING, every release. | ≤ INTEGRITY_CHECK_BUDGET (OQ 13) | [R7.4](#7-verifiability), [R7.7](#7-verifiability) | 🤝 Aligned |
| M9 | Readings lost to a correction, a restore, or a re-measurement | Start: the act. End: every reading the item held before it, read back at SQLITE_READER_FLOOR. Statistic: the count not readable. Population: every such act across [R7.7](#7-verifiability)'s fixtures, and, once [R6.3](#6-deletion-and-privacy) lands, a delete undone inside its window. | 0 | [R7.1](#7-verifiability), [R7.2](#7-verifiability) | 🤝 Aligned |

## Open Questions

| # | Question | Decision so far | Interim rule | Closer | Feeds | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | How many files may a user's work live across, and may more than one be open? | One store, one open at a time, the matching rule and every cross-collection view spanning it (fence F2). | — | Closed — owner. | [R1.1](#1-the-file-the-user-owns), [R1.3](#1-the-file-the-user-owns) | answered |
| 2 | SQLITE_READER_FLOOR | None. Derived from the features kept — plain types, no generated columns, no app-private encodings — not from a rejected one. | Candidate: the `sqlite3` shipping with the app's minimum macOS version, once that floor lands. | ADR-0003 identifies required SQLite features; ADR-0006 selects the macOS floor; owner ratifies the reader floor after both. | [R1.2](#1-the-file-the-user-owns), [R5.1](#5-migration-and-compatibility), [R5.2](#5-migration-and-compatibility), [R5.6](#5-migration-and-compatibility), [R6.5](#6-deletion-and-privacy), [R7.1](#7-verifiability), [R7.2](#7-verifiability), [R7.3](#7-verifiability), [M4](#success-metrics), [M9](#success-metrics) | open |
| 3 | Is the correction-against-re-measurement question asked, and where? | Once, outside the loop; a capture-time re-scan is correction-unconfirmed until answered (fences F3, F12). | — | Closed — owner; amends [the capture PRD's E29](../capture-mode/prd-capture-mode-copy.md#error--state-copy). | [R2.4](#2-canonical-value-and-version-history), [R2.8](#2-canonical-value-and-version-history) | answered |
| 4 | HISTORY_RETENTION | Nothing is aged out; no retention window (fence F6). | — | Closed — owner. | [R2.6](#2-canonical-value-and-version-history) | answered |
| 5 | STORE_SIZE_BUDGET, and whether payloads are compressed | None. The payload may be compressed while nothing a reader needs is (fence F11); [M6](#success-metrics)'s population carries the further multipliers (fences F25, F27, F31). | Candidate: about 1.2 GB raw / 620 MB compressed, extrapolating round-1 41.06/20.53 MB by ×10 ceiling items and ×3 samples; F27/F31 factors still need corpus measurement. | A corpus at the ceiling ([R7.7](#7-verifiability), [M6](#success-metrics)), then owner — after ADR-0003. | [R1.6](#1-the-file-the-user-owns), [R2.6](#2-canonical-value-and-version-history), [M6](#success-metrics) | open |
| 6 | DERIVATION_TOLERANCE, and the reference the check runs against | None. | Candidate ΔE2000 0.1; no reference source named yet ([R7.5](#7-verifiability)). | Build the fixture set and run it, its published source sought on [the device PRD's hardware spike](../device-management/prd-device-management.md#legend) — engineering, then owner. | [R7.5](#7-verifiability), [M2](#success-metrics), [M7](#success-metrics) | open |
| 7 | GAMUT_REFERENCE_SPACE and GAMUT_RENDERING_INTENT | sRGB and relative colorimetric, stored with the reading (fence F4). | — | Closed — owner. | [R3.4](#3-derived-values-and-gamut-honesty) | answered |
| 10 | DELETE_UNDO_WINDOW | The open-file lifetime: undo ends on quit, crash or file close (fences F7, F35). | — | Closed — owner. | [R6.3](#6-deletion-and-privacy) | answered |
| 12 | The vendor SDK's analytics: can they be switched off? | Fence F8 hands the wording over and fence F19 gates release on it ([R6.6](#6-deletion-and-privacy)); whether they can be disabled is open. Events fire on connect, scan and calibration, no colour data (per SDK docs). | The app discloses them and does not claim they can be turned off. | Observe the traffic on [the device PRD's hardware spike](../device-management/prd-device-management.md#legend) — hardware; [R6.6](#6-deletion-and-privacy)'s content floor is re-derived when it closes. | [R6.5](#6-deletion-and-privacy), [R6.6](#6-deletion-and-privacy) | open |
| 13 | INTEGRITY_CHECK_BUDGET | None. The fuller check is superlinear, so running it every launch at the ceiling is unaffordable. | Candidate ≤ 1 s at ROWS_CEILING ([the capture PRD's R3.12](../capture-mode/prd-capture-mode.md#3-the-capture-session)). | Measure both checks at the ceiling — engineering. | [R5.4](#5-migration-and-compatibility), [M8](#success-metrics) | open |
| 14 | Whether the app can notice a write made outside it, and what it then offers | Detection is technically possible ([SQLite data_version](https://sqlite.org/pragma.html#pragma_data_version)); automatic detection policy and response are undecided. | A re-read available outside in-flight capture, promising no automatic detection ([E9](prd-data-foundation-copy.md#error--state-copy)); whether and how the app detects external commits is open. | Owner, gated on ADR-0003. | [R1.5](#1-the-file-the-user-owns) | open |
| 15 | What the raw payload round-trips, and which derived spaces the toolkit supplies | The vendor documents a raw string round-tripping a measurement; the toolkit supplies XYZ, Lab, LCh, Luv ([SDK audit §2](../../briefs/nix-universal-sdk-audit-findings.md), per SDK docs). | Archive the payload beside the stored mean, derive all six spaces from that mean. | Measure, store, reconstruct and compare on [the device PRD's hardware spike](../device-management/prd-device-management.md#legend) — hardware. | [R1.6](#1-the-file-the-user-owns), [R2.1](#2-canonical-value-and-version-history), [R3.1](#3-derived-values-and-gamut-honesty) | open |
| 17 | Regeneration while a run is incomplete or another settings change arrives | Resumable jobs, retained old sets and enumerable stale/mismatched values are required; publication and overlapping-request behavior are not decided. | Do not build the interactive partial-run workflow until the product behavior is approved; unit-test the matrix independently. | Owner behavior decision, then ADR-0003 progress/publishing design. | [R3.3](#regeneration-matrix), [R7.5](#7-verifiability) | open |
| 18 | Additional read-only capabilities | R5.3 guarantees item list, canonical values and history marks in E1/E16/E23/E24 (F39). | Existing export restrictions and requested salvage apply; additional export/salvage/search capabilities remain open. | Owner, then ADR-0003. | [R5.3/R5.5/R5.7](#5-migration-and-compatibility), [R7.3f](#file-actions); [Data Export R1.1](../export/prd-data-export.md#1-what-the-export-contains) | open |
| 19 | Future mid-capture move/re-read policy | Disabled during active, paused or halted capture; interruption does not disable them (F38). | End session to enable move/re-read; no undefined live button. | Owner decides whether to re-enable mid-capture. | [R1.5/R1.9](#1-the-file-the-user-owns), [R7.3c/j](#file-actions) | open |
| 20 | Pending-delete visibility and undo after identity reuse | Restore original content/history; final deletion is unrecoverable; undo ends at file close. | Do not implement P1 undo by inventing whether pending content is queryable or how a reused code/collection name is handled. | Owner visibility/conflict decision, then ADR-0003 undo representation. | [R6.3](#deletion-lifecycle), [R7.2](#7-verifiability) | open |
| 21 | Distinct export mark for unavailable archives | Export runs with unavailable payload cells empty, no new column/state (F37). | Keep this interim behavior; a distinct mark requires owner approval and an Export R2.5 format-version bump. | Owner with Data Export. | [R5.5a](#damage-classification), [R7.7i](#fixture-matrix); [Data Export R1.1](../export/prd-data-export.md#1-what-the-export-contains)/[R4.2](../export/prd-data-export.md#4-verifiability) | open |

Numbers 8, 9, 11 and 16 are retired under fence F30, never reused: they are now [the export PRD's OQ 1–4](../export/prd-data-export.md#open-questions), answers and evidence unchanged.

Results file: [`prd-data-foundation-oq-results.md`](prd-data-foundation-oq-results.md), one `## OQ <id>` section per answer; a status changes only when that section exists, and a number is never reused. Every provisional constant and every "per SDK docs" marker carries its OQ id — a marker with no entry above is invalid.
