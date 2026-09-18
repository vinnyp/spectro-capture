# PRD: Data Export

Status: locked (2026-09-16); PR #18 agent-build amendment (2026-09-17–18), F15–F30; requirements aligned, not implemented.

Companion files: the journeys are in [prd-data-export-journeys.md](prd-data-export-journeys.md), the shipping copy in [prd-data-export-copy.md](prd-data-export-copy.md), the answers to closed open questions in [prd-data-export-oq-results.md](prd-data-export-oq-results.md), and the owner's decisions in [prd-data-export-fences.md](prd-data-export-fences.md).

# Background

Build contract for [vision U6/J4](../vision.md#use-cases): export collection data as CSV without changing the source. [Data Foundation](../data-foundation/prd-data-foundation.md) owns the source file; [ADR-0003](../../decisions/README.md#decision-queue) owns its schema.

**In scope:** what a canonical and a history export contain, and which items get a row; column names, the reserved prefix, collisions, the dialect, the wavelength column set and the export format version; the failures an export can end in; what a test asserts against a checked-in golden.

**Out of scope:** the file the export reads and everything it guarantees, the [Data Foundation PRD](../data-foundation/prd-data-foundation.md)'s — including a derived set for a condition other than the collection's chosen one, which stays in the file and is exported by neither export kind ([its F31](../data-foundation/prd-data-foundation-fences.md), fence F9); the storage schema, ADR-0003's; browsing and editing, Collection Mode's; CxF and migration in from a vendor app, v2 ([OQ 3](#open-questions)); cloud sync ([AGENTS.md §4](../../../AGENTS.md#4-non-negotiables)).

## User Journeys

Acceptance scenarios are in the [journeys companion](prd-data-export-journeys.md); they assert these rows without adding rules.

| Journey | Name | Rows exercised | Entry |
| :--- | :--- | :--- | :--- |
| EJ1 | Export the collection | R1.1–R1.3, R2.1–R2.5, R3.1, R4.1–R4.4; DF R3.4/R3.5 | [EJ1](prd-data-export-journeys.md#ej1-export-the-collection) |

## Requirements

### Vocabulary

The terms these rows use are [AGENTS.md §8](../../../AGENTS.md#8-vocabulary)'s and the [Data Foundation PRD's Vocabulary](../data-foundation/prd-data-foundation.md#vocabulary)'s — canonical value, raw payload, sample, reading, derived value, supersession reason, quarantined, gamut-clipped, chosen condition, fixture — and are not restated here. Added here:

- **Canonical export** — the default: one row per item carrying its canonical value on the collection's chosen condition.
- **History export** — the explicit option: one row per version of every item, canonical and superseded alike.
- **Golden** — a checked-in file an export is asserted against — header and rows, or header alone where [R4.1](#4-verifiability) says so — one per fixture, per export kind, for every released export format version.
- **State column** — the closed set never-scanned, quarantined, current, and superseded, the last only in the history export, and where a row is both unreadable and superseded it reads quarantined; a new value in it is a change in meaning under [R2.5](#2-columns-names-dialect-and-the-version).

### Legend

**Priority.** Build order within v1, not a cut line, and a recommendation rather than a decision: P1 holds the history export ([R1.3](#1-what-the-export-contains), fence F2); everything else is P0.

Provisional constants: each is named, carries its OQ id and a candidate until a fence fixes it, and none ships in a release with its OQ open — a dogfood build not being a release. An unqualified "OQ n" or row ID is this document's; a row or question cited from another PRD names that PRD in the link. WAVELENGTH_GRID is this document's, under [OQ 4](#open-questions); DERIVATION_VERSION and SQLITE_READER_FLOOR are the [Data Foundation PRD](../data-foundation/prd-data-foundation.md#legend)'s, and ROWS_CEILING the capture PRD's, under [its OQ 13](../capture-mode/prd-capture-mode.md#open-questions).

**Status.** A status cell here and in the [copy file](prd-data-export-copy.md#error--state-copy) holds one of these six and nothing else; [Open Questions](#open-questions) has its own two, open and answered.

- ⌛️ Ready for Alignment — awaiting cross-functional alignment · ✋ Needs Discussion — the team needs the PM · 🤝 Aligned — the team agrees
- 🦺 In Progress — implementation in flight (optional) · ✅ Completed — merged, its PR in Commit PR · ✂️ Deferred — out of this release

### Traceability

Three row-ID families, one per dispositionable table, under one rule: an ID is assigned once and never renumbered, a row cut or deferred keeping its ID. Requirement rows in [§1](#1-what-the-export-contains)–[§4](#4-verifiability) carry `R<section>.<n>`, state rows in the [copy file](prd-data-export-copy.md#error--state-copy) `E<n>`, metric rows `M<n>`; lettered requirement subdivisions use `R<section>.<n><letter>` and copy variants `E<n><letter>`. Commit PR maps a row onto the work that lands it; owner decisions live in the [fence file](prd-data-export-fences.md), a row naming one for provenance only.

The historical ID mapping is in the [split-origin map](prd-data-export-fences.md#split-origin-map). Lettered rows decompose their parent requirement and inherit its priority and implementation tracking; clauses about history remain P1 under R1.3.

### Surfaces

This document governs one surface. The table is [R4.4](#4-verifiability)'s rule — what a test lists is what that surface shows — and its [copy IDs](prd-data-export-copy.md#error--state-copy) name states, never strings.

| ID | Surface | What the test lists | Req-IDs | Copy IDs | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| R4.4a | Export surface | All E1–E4 actions; R1.1o–r preview counts/disclosures and P1 gates; collision name pairs | R1.1–R1.3, R1.1o–r, R2.1–R2.5, R3.1 | E1, E1a–g, E2, E3, E4 | 🤝 Aligned |

Evidence and rationale: [fences and research inventory](prd-data-export-fences.md#phase-0--research-inventory-2026-09-09).

### 1. What the export contains

| ID | Release | Pri | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- | :--- | :--- |
| R1.1 | v1 | P0 | Export a collection or single item using [Row selection and fields](#row-selection-and-fields), [Missing-data matrix](#missing-data-matrix), [Export eligibility](#export-eligibility), and [Preview contract](#preview-contract); the default is canonical, one row per item on the collection’s chosen condition (F2/F9–F11/F16). Export only reads: every source value, mark and note remains identical at SQLITE_READER_FLOOR after success or failure (R4.3). | 🤝 Aligned | aligned round 7 |
| R1.2 | v1 | P0 | A non-spectral reading has empty wavelength cells, a non-spectral mark, averaging basis and derivation version ([Capture R4.24](../capture-mode/prd-capture-mode.md#4-the-scan-loop)). `sc_simulated` is true/false from a known acquiring snapshot, empty without one; consumers treat empty as unknown ([Device R6.5](../device-management/prd-device-management.md#6-mock-device-layer), F19). | 🤝 Aligned |  |
| R1.3 | v1 | P1 | The explicit history option emits every item reading, including current, superseded and quarantined readings, plus one identity row for each never-scanned item (F2). Each reading supplies its own fields and chosen-condition derived set, sequence, supersession reason and current flag under R1.1a–l; non-chosen sets stay in the file ([DF R3.1](../data-foundation/prd-data-foundation.md#3-derived-values-and-gamut-honesty)). | 🤝 Aligned | aligned round 7 |


#### Row selection and fields

| ID | Rule |
| :--- | :--- |
| R1.1a | Scope is the whole collection or one item, using the same surface; keys are Swatch Code for canonical and Swatch Code + version ordinal for history, except the never-scanned identity row has no ordinal |
| R1.1b | Every row carries identity as entered, preserved imported values, export format version, writing app version, export kind (`canonical`/`history`), chosen condition and state; Swatch Code equality is [Import R2.3](../import/prd-inventory-import.md#2-target-mapping-and-the-matching-rule)’s |
| R1.1c | Reading fields: measured-at (canonical: current reading, including quarantined-current provenance), acquiring [Device R1.21 snapshot](../device-management/prd-device-management.md#1-device-pairing), averaging basis, spectral data, six spaces (XYZ/Lab/LCh/Luv/sRGB/HSL), and R2.1’s qualifiers |
| R1.1d | `sc_sample_1_payload` … `sc_sample_5_payload` carry each vendor string exactly as supplied, decompressed if stored compressed, byte-identical after CSV unquoting (F30; vendor reconstruction fidelity: per SDK docs, [DF OQ 15](../data-foundation/prd-data-foundation.md#open-questions)); unused, missing or unavailable archives emit empty cells without a new state/column (F8/F14; [DF OQ 21](../data-foundation/prd-data-foundation.md#open-questions)) |
| R1.1e | History adds [DF R2.1/R2.4](../data-foundation/prd-data-foundation.md#2-canonical-value-and-version-history)’s sequence, incoming reason (`initial`, `re-measurement`, `correction`, `correction-unconfirmed`, `restore`) and current flag; predecessor never-true marks are not reasons |
| R1.1f | History uses each row’s measurement time; record time is not exported; current flag is true only for the canonical reading, false otherwise, including never-scanned and every row when current is quarantined |
| R1.1g | Order items by [Capture R1.9](../capture-mode/prd-capture-mode.md#1-collections)’s queue order and history by per-item sequence ascending; QC records and superseded derivations are not item versions |

#### Missing-data matrix

These rows apply to both kinds unless the Case cell names one; R1.1b’s common fields, including the chosen-condition column, are preserved in every case; empty means an empty field, not zero or false; qualifier cells are the per-value illuminant, observer, condition and derivation-version columns plus the three `sc_sRGB_*` qualifiers.

| ID | Case | State / emitted fields |
| :--- | :--- | :--- |
| R1.1h | Never-scanned item, either kind | `never-scanned`; reading fields/payloads/qualifiers empty, including snapshot/`sc_simulated`, non-spectral mark, basis and measured-at; history has empty ordinal/reason and false current flag |
| R1.1s | Quarantined current, canonical export | `quarantined`; keep readable snapshot/`sc_simulated`, basis and measured-at from that reading, unknown metadata empty; colour/spectral/qualifier cells empty, payload cells follow R1.1d/l (F21) |
| R1.1i | Quarantined reading, history export | `quarantined` overrides superseded; false current flag, colour/spectral/qualifier cells empty; payload cells follow R1.1d/l; retain readable sequence/reason, snapshot/`sc_simulated`, basis and measured-at; unknown metadata/marks empty, never guessed |
| R1.1j | Readable current or historical reading | `current`/`superseded`; emit its fields and live chosen-condition set; absent chosen-condition measurement leaves colour/spectral/qualifier cells empty, never substitutes another condition |
| R1.1k | Non-spectral reading | R1.1j’s missing-condition absence takes precedence; otherwise six spaces retain original-reference qualifiers ([DF R3.5](../data-foundation/prd-data-foundation.md#3-derived-values-and-gamut-honesty)), spectral cells empty and non-spectral mark present; mismatch is derivable from original-reference qualifiers and the chosen-condition column, with no separate export mark (F25) |
| R1.1l | Archive-only damage | Keep intact reading/derived fields and current/superseded state; only unavailable payload slots become empty; canonical export still succeeds (F14) |

#### Export eligibility

| ID | Source state | Result |
| :--- | :--- | :--- |
| R1.1m | Interpretable, healthy file, including read-only | Export offered and succeeds given a writable destination |
| R1.1n | Newer/below-floor file ([DF R5.3/R5.7](../data-foundation/prd-data-foundation.md#5-migration-and-compatibility)) or file-level damage/invariant failure ([DF R5.5d/R2.2](../data-foundation/prd-data-foundation.md#damage-classification)) | Export not offered, overriding R1.1m; per-reading/archive damage follows R1.1h–l/s, not this refusal; DF owns recovery, with R5.5f automatic derived-set recovery neither an export write nor an export exception |

#### Preview contract

R1.1o–r govern E1/E1a–g and R4.4a (F28); history clauses require P1 R1.3.

| ID | Input | Required preview |
| :--- | :--- | :--- |
| R1.1o | Canonical / history | Before writing, recompute disclosures for selected scope/kind; name kind, item count, chosen condition, export-format/app versions and measured-at meaning; history counts versions plus never-scanned rows, rendering row count only when different from item count |
| R1.1p | Output population | Count simulated, non-spectral and quarantined reading rows, never-scanned items, and unavailable sample archives; canonical quarantine counts affected item rows; distinguish unavailable archives from unused slots/samples without payload |
| R1.1q | Fields / renames | Describe present spaces, wavelength data, qualifiers, device serial and payload slots without claiming absent content; disclose that non-spectral readings retain their measured reference, identified by qualifiers; disclose original → emitted pairs from R2.4c |
| R1.1r | Empty collection | State that the collection is empty and output contains only column names; omit absent-data claims and zero-count clauses, retain applicable kind/destination actions and version disclosure |

### 2. Columns, names, dialect, and the version

| ID | Release | Pri | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- | :--- | :--- |
| R2.1 | v1 | P0 | The sRGB and HSL columns never appear without `sc_sRGB_source_space`, `sc_sRGB_rendering_intent`, `sc_sRGB_gamut_clipped`; each colour value carries illuminant, observer, condition and derivation version, with non-spectral qualifiers retaining original reference under R1.1k (F25); R1.1b’s chosen-condition column is separate from per-value qualifiers. Absent values/qualifiers are empty, never zero/substitutes; on current/superseded rows this means no measurement in the chosen condition ([DF R3.1/R3.5](../data-foundation/prd-data-foundation.md#3-derived-values-and-gamut-honesty)). | 🤝 Aligned | edited FX5-9, alignment kept |
| R2.2 | v1 | P0 | One dialect, no preference: a header row first, then one row per record — UTF-8 with no byte-order mark, comma-separated, quoted per RFC 4180, `.` as the decimal separator, LF line endings. Every app column's value but identity's is written in one form too — reflectance as a fraction of 1, times in RFC 3339 in UTC with the offset written as `Z`, booleans as `true` or `false`, numbers without a thousands separator at a fixed precision the golden records ([R4.1](#4-verifiability)) — and the help docs carry, per released export format version, each app column's meaning and value form. | 🤝 Aligned | edited FX6-13, alignment kept; edited FX7-14, alignment kept; edited FX8-6; aligned round 9 |
| R2.3 | v1 | P0 | Spectral columns are `sc_nm_<wavelength>` over WAVELENGTH_GRID (start/end/interval), fixed per export format version (OQ 4, F7); out-of-grid readings leave these cells empty, a branch unreachable for v1’s instrument-family grid. App columns precede passthrough columns in golden order; passthrough follows stored collection positions ([DF R1.2](../data-foundation/prd-data-foundation.md#1-the-file-the-user-owns)), identity-mapped columns appear only under their `sc_` names, items without imported values have empty passthrough cells, and R1.1g orders rows so unchanged inputs produce byte-identical exports. | 🤝 Aligned |  |
| R2.4 | v1 | P0 | Every app column uses `sc_` plus lower snake_case except exactly `sc_sRGB_gamut_clipped`, `sc_sRGB_source_space`, `sc_sRGB_rendering_intent`; other sRGB columns follow lower snake_case and unnamed columns are fixed by the golden (F24). The reserved prefix is documented and stable per format version; R2.4a–c rename passthrough headers without changing stored names/values or dropping columns, with only CSV quoting reshaping emitted values and E1 disclosing renames (F13/F22/F23). | 🤝 Aligned | edited FX6-26, alignment kept; edited FX7-14, alignment kept; edited FX8-3; edited FX9-1; edited FX10-5; aligned round 11 |
| R2.5 | v1 | P0 | The export format version — a whole number incremented by one and never reused — bumps when an app column is removed, renamed, reordered, changed in meaning or in the form its values are written in, or when WAVELENGTH_GRID changes, and not when an app column is strictly appended, that column landing at the end of the app block with the passthrough columns keeping their relative order after it, as [R1.3](#1-what-the-export-contains)'s columns do under the version in force when it lands. The help docs state that rule, that column position is no part of the contract — a consumer parsing by name — and that measured-at is not monotonic across an item's history rows; the file's own format version ([the Data Foundation PRD's R5.6](../data-foundation/prd-data-foundation.md#5-migration-and-compatibility)) is a different number, never read as this one. | 🤝 Aligned | edited FX6-18, FX6-27, alignment kept; edited FX7-13, alignment kept |


#### Collision allocation

| ID | Step / oracle |
| :--- | :--- |
| R2.4a | Allocate in persisted collection-column order across imports ([Import R2.5/R2.6](../import/prd-inventory-import.md#2-target-mapping-and-the-matching-rule)), excluding identity-mapped columns; both kinds allocate identically because R2.4b reserves the entire `sc_` namespace, including future app names.<br>Stored → emitted examples in order:<br>`sc_simulated`, `import_sc_simulated` → `import_sc_simulated`, `import_import_sc_simulated`;<br>`import_sc_simulated`, `sc_simulated` → `import_sc_simulated`, `import_import_sc_simulated`;<br>`sc_simulated`, `import_sc_simulated`, `import_import_sc_simulated` → `import_sc_simulated`, `import_import_sc_simulated`, `import_import_import_sc_simulated` |
| R2.4b | Start with the stored spelling and spacing, unnormalised, and compare under [Import R2.3](../import/prd-inventory-import.md#2-target-mapping-and-the-matching-rule); while its normalised candidate begins `sc_` or equals an allocated name under R2.3, prepend `import_` to the candidate’s spelling and spacing, unnormalised, and recheck; allocate only after neither predicate holds |
| R2.4c | Preserve emitted order/values and stored names; earlier stored columns win, so even a literal `import_` column can be renamed because another column collided; E1 lists every original → emitted pair |

### 3. When an export cannot finish

| ID | Release | Pri | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- | :--- | :--- |
| R3.1 | v1 | P0 | An export that cannot finish leaves no partial file and says which of three it was — no room, no permission, or the destination gone — offering a retry and somewhere else ([E2](prd-data-export-copy.md#error--state-copy), [E3](prd-data-export-copy.md#error--state-copy), [E4](prd-data-export-copy.md#error--state-copy), [R4.3](#4-verifiability)). | 🤝 Aligned |  |


### 4. Verifiability

| ID | Release | Pri | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- | :--- | :--- |
| R4.1 | v1 | P0 | Check in a golden per fixture, export kind and released export format version following [Golden acceptance](#golden-acceptance). The golden is that release’s contract; disagreements in this PRD or help docs receive errata in the same PR (F12). | 🤝 Aligned | edited FX7-13, FX7-15, FX7-16; edited FX8-10; edited FX9-2; edited FX10-6; edited FX11-2; aligned round 12 |
| R4.2 | v1 | P0 | Enumerate [DF R7.7a–n plus generated corpus](../data-foundation/prd-data-foundation.md#fixture-matrix): assert eligible cases’ R1.1 fields, R1.2/R2.1 values, R2.4a–c allocation, dialect and M1 against R4.1 goldens, including all three `sc_simulated` values and R7.7i’s unavailable/unused/never-supplied payload cases; no vendor credential ([DF R6.5](../data-foundation/prd-data-foundation.md#6-deletion-and-privacy)). States excluded by R1.1n (including R7.7m when below floor) have no golden and use R4.3 refusal/preservation checks; eligible migrated states use goldens and history assertions wait for R1.3. | 🤝 Aligned | edited FX7-17, alignment kept |
| R4.3 | v1 | P0 | A test induces each export-destination failure on demand — no room, no permission, the destination gone, a failure mid-write, which resolves to one of the three by cause — observing which named state came up, that nothing partial was left behind, and that no file went missing ([R3.1](#3-when-an-export-cannot-finish)). Across a successful export and each induced failure it reads every value, mark and note back identical at SQLITE_READER_FLOOR ([R1.1](#1-what-the-export-contains)), and runs one export against a file declared open read-only ([the Data Foundation PRD's R7.2](../data-foundation/prd-data-foundation.md#7-verifiability)) and one against each of [R1.1n](#export-eligibility)'s not-exportable states, asserting the export is not offered on it ([the Data Foundation PRD's R7.6b](../data-foundation/prd-data-foundation.md#7-verifiability)); the move's failures are [that PRD's R7.8](../data-foundation/prd-data-foundation.md#7-verifiability)'s. | 🤝 Aligned | aligned round 7 |
| R4.4 | v1 | P0 | A test lists what each surface offers and shows, without matching wording; the [Surfaces](#surfaces) table is the rule, and a surface whose rows are all P1 is listed once they land. | 🤝 Aligned |  |

#### Golden acceptance

| ID | Case | Required assertion / artifact |
| :--- | :--- | :--- |
| R4.1a | First golden | Fix unnamed app headers, app-column order and numeric precision; include R1.1b–f/R1.2/R2.1 fields and dialect, R2.2’s per-version meanings/forms, and R2.5’s bump rule, parse-by-name rule, non-monotonic measured-at and distinct file-format version; payload cells fix R1.1d’s decompressed, byte-identical vendor string; reflectance precision waits for R4.1f |
| R4.1b | Current fixture | Goldens retain the app-version column with literal `__APP_VERSION__` in each data cell; assert the actual export’s released-version string is present/well-formed, replace only those cells with that literal for byte comparison, and compare two unchanged-input/build exports in full without replacement |
| R4.1c | Append app column | Same format version; append after existing app columns, before passthrough; refresh current golden in place with reviewed diff; history-only additions follow R2.5 |
| R4.1d | DERIVATION_VERSION change | Same export format version; refresh current golden with diff confined to derived values, gamut marks and derivation-version fields |
| R4.1e | R2.5 format-breaking change | Increment format version, create new goldens; retain retired goldens as release documentation and inputs for a future old-export parser test |
| R4.1f | OQ 4 still open / closes | Until hardware closure, omit wavelength block from golden and comparison but assert remaining columns and dialect; on closure re-cut full goldens and fix reflectance precision by reviewed choice; no release with OQ 4 open |
| R4.1g | ROWS_CEILING corpus | Header-only golden; assert header/dialect, not every data row; DF R7.7 owns generation and ADR-0003 its determinism |
| R4.1h | P1 history / empty collection | History golden asserted once R1.3 lands; [DF R7.7n](../data-foundation/prd-data-foundation.md#fixture-matrix)’s empty collection emits the selected kind’s header and no rows, with R1.1r preview (F20/F27) |

### Inherited obligations

Each line is a requirement. A row cited here carries the rule, the naming PRD's own row authoritative for its wording; a line with no row is handed over whole.

**What other PRDs impose on this one**

| Source PRD | Obligation | Rows here |
| :--- | :--- | :--- |
| Data Foundation | [R1.2](../data-foundation/prd-data-foundation.md#1-the-file-the-user-owns): floor-readable samples, preserved imported names/positions/values; [R2.1/R2.2/R2.4/R2.9](../data-foundation/prd-data-foundation.md#2-canonical-value-and-version-history): snapshots, basis, sequence, time axes, reasons/current selection; [R3.1](../data-foundation/prd-data-foundation.md#3-derived-values-and-gamut-honesty): a chosen-condition set per reading with history included, absent only for missing measurement, non-chosen sets not exported ([DF F31](../data-foundation/prd-data-foundation-fences.md)); [R5.5a–c](../data-foundation/prd-data-foundation.md#damage-classification): unavailable archives emit empty payloads, quarantine overrides superseded, export never writes; [R6.5](../data-foundation/prd-data-foundation.md#6-deletion-and-privacy): no vendor credential; [R7.7n](../data-foundation/prd-data-foundation.md#fixture-matrix): empty collection | [R1.1–R1.3](#1-what-the-export-contains), [R2.1/R2.3](#2-columns-names-dialect-and-the-version), [R4.1h](#golden-acceptance), [R4.2/R4.3](#4-verifiability) |
| Capture Mode | A collection's queue order lives with the collection ([its R1.9](../capture-mode/prd-capture-mode.md#1-collections)), and an export's rows follow it; the basis an average was taken on and the non-spectral mark travel with the reading ([its R4.24](../capture-mode/prd-capture-mode.md#4-the-scan-loop)). | [R1.1g](#row-selection-and-fields), [R1.2](#1-what-the-export-contains), [R2.3](#2-columns-names-dialect-and-the-version) |
| Device Management, export | CSV export emits `sc_simulated` as true/false from a known acquiring snapshot, empty without one (F19) ([its R6.5](../device-management/prd-device-management.md#6-mock-device-layer)); every measurement carries the acquiring device's snapshot, which the export writes ([its R1.21](../device-management/prd-device-management.md#1-device-pairing)). | [R1.1](#1-what-the-export-contains), [R1.2](#1-what-the-export-contains) |
| Inventory Import | Every column an import brought in is carried through, and a column mapped to identity is emitted once rather than twice ([its R2.2/R2.6](../import/prd-inventory-import.md#2-target-mapping-and-the-matching-rule)) | [R1.1](#1-what-the-export-contains), [R2.3](#2-columns-names-dialect-and-the-version), [R2.4](#2-columns-names-dialect-and-the-version) |

**What this PRD imposes on others**

| Target PRD | Obligation | Rows |
| :--- | :--- | :--- |
| Data Foundation | [Its R7.7a–n fixture matrix, including empty collection R7.7n,](../data-foundation/prd-data-foundation.md#fixture-matrix) is the sole inventory for the files and generated scale corpus these goldens use; no duplicate list is maintained here. R1.1n's not-exportable states are declared by its R7.2 and induced by its R7.3; R4.3 asserts them, and no export carries the vendor license credential its R6.5 tests against | [R4.1](#4-verifiability), [R4.2](#4-verifiability), [R4.3](#4-verifiability) |
| Device Management | [Device R6.5](../device-management/prd-device-management.md#6-mock-device-layer) calls its provenance column `simulated`, emitted as `sc_simulated` under this contract; its [export obligation](../device-management/prd-device-management.md#inherited-obligations) targets this PRD for that column and R1.21’s acquiring snapshot (F6) | [R1.1](#1-what-the-export-contains), [R1.2](#1-what-the-export-contains) |
| Capture Mode | Existing contract: [its R1.9](../capture-mode/prd-capture-mode.md#1-collections)'s queue-order obligation names this PRD beside Data Foundation, an export's rows following that order, and its R4.24 basis obligation names this PRD too | [R1.1g](#row-selection-and-fields), [R1.2](#1-what-the-export-contains), [R2.3](#2-columns-names-dialect-and-the-version) |
| Inventory Import | Existing contract: [its R2.2](../import/prd-inventory-import.md#2-target-mapping-and-the-matching-rule)'s passthrough and identity mapping obligates this PRD, which carries those columns into the export and renames a collision rather than dropping it | [R2.3](#2-columns-names-dialect-and-the-version), [R2.4](#2-columns-names-dialect-and-the-version) |

### 5. Error & State Copy

The shipping copy for every state this PRD names is in [prd-data-export-copy.md](prd-data-export-copy.md), with the placeholder rules; each state's identity is stable even when its wording changes, so behaviour is asserted independently of copy ([R4.3](#4-verifiability)). The delete confirmations that offer an export first are the [Data Foundation PRD's E8 and E14](../data-foundation/prd-data-foundation-copy.md#error--state-copy)'s and are not restated.

## Success Metrics

Numeric targets are proposals, not commitments.

| ID | Metric | Definition (start event, end event, statistic, population) | Candidate target | Method | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| M1 | Export completeness | Start: an exported CSV. End: the canonical values reconstructed from it alone. Statistic: the share reconstructable to the fidelity its basis allows — an item with no canonical value counting as reconstructable when its identity and import columns round-trip and its state column reads so — and the share of sRGB and HSL columns carrying their qualifiers. Population: [the Data Foundation PRD's R7.7](../data-foundation/prd-data-foundation.md#7-verifiability) fixture set. | 100% of both | [R4.1](#4-verifiability), [R4.2](#4-verifiability) | 🤝 Aligned |

## Open Questions

| # | Question | Decision so far | Interim rule | Closer | Feeds | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | What the gamut-clipped export column is called (the Data Foundation PRD's OQ 8) | `sc_sRGB_gamut_clipped`, beside `sc_sRGB_source_space` and `sc_sRGB_rendering_intent`; no standard names this flag, so it is invented, and F6 exempts nothing (fences F4, F6). | — | Closed — owner; revisited only if ISO 17972-4's schema becomes readable. | [R2.1](#2-columns-names-dialect-and-the-version), [R2.4](#2-columns-names-dialect-and-the-version) | answered |
| 2 | Does an export carry version history, and in what shape? (the Data Foundation PRD's OQ 9) | Canonical values by default, one row per item; an explicit v1 option exports every version (fence F2). | — | Closed — owner. | [R1.1](#1-what-the-export-contains), [R1.3](#1-what-the-export-contains) | answered |
| 3 | CxF export and import (the Data Foundation PRD's OQ 11) | Out of scope for v1. The format is CxF/X-4 (ISO 17972-4); whether the vendor's mobile app exports at all is an open gap ([vision J7](../vision.md#j7-migrating-in-from-the-vendor-apps-cataloger-v2-candidate)). | None — v1 exports CSV. | A v2 scoping pass once the schema is readable — owner. | [R1.1](#1-what-the-export-contains) | open |
| 4 | WAVELENGTH_GRID — the wavelength column set the export format version fixes (the Data Foundation PRD's OQ 16) | None. The set belongs to the export format version, v1's being the v1 instrument family's reported grid (fence F7). | Candidate: the Spectro 2's grid — start, end, interval — per SDK docs; until it closes the golden leaves the wavelength block out ([R4.1](#4-verifiability)). | Read the grid off the instrument on [the device PRD's hardware spike](../device-management/prd-device-management.md#legend) — hardware. | [R2.3](#2-columns-names-dialect-and-the-version), [R2.5](#2-columns-names-dialect-and-the-version), [R4.1](#4-verifiability) | open |

Results file: [`prd-data-export-oq-results.md`](prd-data-export-oq-results.md), one `## OQ <id>` section per answer; a status changes only when that section exists, and a number is never reused. Every provisional constant and every "per SDK docs" marker carries its OQ id — a marker with no entry above is invalid.
