# PRD: Inventory Import

Author: Vinny Pasceri

Status: draft

Companion files: the journeys are in [prd-inventory-import-journeys.md](prd-inventory-import-journeys.md), the shipping copy in [prd-inventory-import-copy.md](prd-inventory-import-copy.md), the answers to closed open questions in [prd-inventory-import-oq-results.md](prd-inventory-import-oq-results.md), and the owner's decisions in [prd-inventory-import-fences.md](prd-inventory-import-fences.md).

# Background

A Cataloger's swatch list already exists in a spreadsheet, and typing it into the app one row at a time is the work this document removes. Inventory import reads that file, maps its columns onto swatches, and lands every row in a collection as pending. It serves [U1](../vision.md#use-cases), bulk-digitize a predefined inventory, and it is the first step of the inventory-first wedge. Import ends at ready-to-capture and never starts a session; the run that follows is the [capture PRD](../capture-mode/prd-capture-mode.md)'s.

## User Journeys

Every journey — UJ 2, UJ 2.1, UJ 2.2, and the flow diagram that covers all three — is in [prd-inventory-import-journeys.md](prd-inventory-import-journeys.md). They keep the numbers they carried in the capture PRD, so citations from the capture journeys stay true (fence F49).

## Requirements

### Vocabulary

The terms the rows below use that the [capture PRD's Vocabulary](../capture-mode/prd-capture-mode.md#vocabulary) does not define ([AGENTS.md §8](../../../AGENTS.md#8-vocabulary)). Pending, Captured, and Session are defined there and are not restated here.


- **Swatch Code** — the one column an import must map; it is how a file row is matched to a collection row on a re-import, and how the Cataloger finds a swatch later ([R2.2](#2-target-mapping-and-the-matching-rule), [R3.3](#3-preview-and-commit)).
- **The one matching rule** — how two Swatch Codes, or two collection names, are judged to be the same ([R2.3](#2-target-mapping-and-the-matching-rule)).
- **Header signature** — a file's set of column names, in any order, which a remembered mapping is keyed by ([R1.2](#1-reading-the-file)).
- **Ready to capture** — a collection holding the imported rows as pending, in file order, with no session started; import ends here ([R3.2](#3-preview-and-commit)).


### Legend

**Priority**

Priority is build order within v1, not a cut line — everything in this document ships in v1, and every row here is P0, the first build phase, because a bulk session has nothing to scan until an inventory has landed. The P0/P1 split is fence F24, recorded in the [capture fence file](../capture-mode/prd-capture-mode-fences.md).

Provisional constants: every TBD-on-spike constant is a named provisional constant carrying its candidate value and its Open Questions id; mechanisms build against these constants, and no provisional constant ships in a release without its OQ resolved. Two bear on import and neither is closed here: IMPORT_BUDGET, at candidate TBD, and ROWS_CEILING, which [R3.1](#3-preview-and-commit) warns against — both are [the capture PRD's R3.12](../capture-mode/prd-capture-mode.md#3-the-capture-session)'s and are closed by its OQ 13. An "OQ n" written without a qualifier is this document's; another PRD's are named as its own. A row ID written without a qualifier is this document's; a row cited from another PRD names that PRD in the link.

**For the engineering plan.** These are the plan's to answer, not this document's.

- IMPORT_BUDGET sits at "candidate TBD" under the capture PRD's OQ 13.
- One P0 row defers, in part, to an open question with no interim rule: [R1.2](#1-reading-the-file) (OQ 1).

**Row IDs.** This document numbers its requirement rows from R1.1 under fence F49 and keeps the `E<n>` numbers it inherited; the IDs those rows carried in the capture PRD are retired there and never reused ([Traceability](#traceability)).

**Status of this document.** Every row is 🤝 Aligned: the moved rows came from the locked capture PRD with their rules unchanged and only their citations rewritten (fence F49), and the F49 verification rounds (25, 26) are recorded in the [capture review log](../../agent-reviews/2026-09-06-prd-capture-mode-peer-reviews.md). The Commit PR column is empty throughout, and this document is locked with the capture PRD.

**Status**

A status cell in this document and in the [copy file](prd-inventory-import-copy.md#error--state-copy) holds one of the six values below and nothing else. The [Open Questions](#open-questions) table has its own two values, open and answered.

- ⌛️ Ready for Alignment - Waiting for cross-functional team to align on requirements
- ✋ Needs Discussion - Cross-functional team needs to discuss with PM
- 🤝 Aligned - Cross-functional team aligned on the requirement
- 🦺 In Progress - Implementation in flight (Optional status)
- ✅ Completed - Implementation completed & merged. PR # & link added in the "Commit PR" column.
- ✂️ Deferred - Deferred from current release


### Traceability

Three row-ID families, one per dispositionable table, all under one rule: an ID is assigned once and never renumbered — a row that is cut or deferred keeps its ID rather than freeing it for reuse.

- Every requirement row in [§1](#1-reading-the-file) through [§4](#4-demo-device-and-verifiability) carries an ID of the form `R<section>.<n>` — for example `R3.1`.
- Every state row in the [copy file](prd-inventory-import-copy.md#error--state-copy) carries an ID of the form `E<n>`.
- Every metric row in [Success Metrics](#success-metrics) carries an ID of the form `M<n>`.

The Commit PR column is where a row maps onto the work that lands it. Owner decisions are recorded in the [fence file](prd-inventory-import-fences.md); where a row names one it is for provenance only, and no row re-argues one. Which rows carry each fence is that file's "Fence → row map".

**Where these rows came from.** Every row below moved from the capture PRD's §2 under fence F49; the left-hand IDs are retired there and never reused. The copy states E4–E14, E38, and E40 moved with their numbers unchanged, and so did journeys UJ 2, UJ 2.1, and UJ 2.2.

| In the capture PRD | Here |
| :--- | :--- |
| R2.1 | [R1.1](#1-reading-the-file) |
| R2.2 | [R1.2](#1-reading-the-file) |
| R2.3 | [R1.3](#1-reading-the-file) |
| R2.4 | [R1.4](#1-reading-the-file) |
| R2.5 | [R2.1](#2-target-mapping-and-the-matching-rule) |
| R2.6 | [R2.2](#2-target-mapping-and-the-matching-rule) |
| R2.7 | [R2.3](#2-target-mapping-and-the-matching-rule) |
| R2.8 | [R2.4](#2-target-mapping-and-the-matching-rule) |
| R2.9 | [R3.1](#3-preview-and-commit) |
| R2.10 | [R3.2](#3-preview-and-commit) |
| R2.11 | [R3.3](#3-preview-and-commit) |
| R2.12 | [R3.4](#3-preview-and-commit) |
| R2.13 | [R3.5](#3-preview-and-commit) |
| R2.14 | [R3.6](#3-preview-and-commit) |
| R11.15h | [R4.1](#4-demo-device-and-verifiability) |
| M7 | [M1](#success-metrics) |
| OQ 12 | [OQ 1](#open-questions) |

[R4.2](#4-demo-device-and-verifiability) is not in the map. It carries over these states the obligation the [capture PRD's R11.12](../capture-mode/prd-capture-mode.md#11-demo-device-and-verifiability) held before the split, and that row stays live there.

### Surfaces

This table names the one import-owned surface. The collection surface it is reached from, and every other surface in the app, is the [capture PRD's](../capture-mode/prd-capture-mode.md#surfaces).


| Surface | Shows | The [copy](prd-inventory-import-copy.md#error--state-copy) states in this surface's flow — its entry offer and its cancel notice included |
| :--- | :--- | :--- |
| Import flow | The file that was picked, what was detected in it, the column mapping, and the pre-commit preview with its issue list | No header row; can't read the file; nothing to import; rows with the wrong number of columns; mapping incomplete; blank codes excluded; duplicate codes in the file; ambiguous code; unnamed column; file changed on disk; file gone before the import; changed metadata on a captured row; import blocked by a session |

### 1. Reading the file

Traces [UJ 2](prd-inventory-import-journeys.md#uj-2-full-collection-bootstrap-via-csv-import); serves [U1](../vision.md#use-cases), the inventory-first wedge.

#### As a Cataloger, I can import my inventory from a spreadsheet export so that I never type my swatch list into the app.


| ID | Release | Pri | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- | :--- | :--- |
| R1.1 | v1 | P0 | Import starts from an explicit user action and accepts a file by picker or by drag and drop. | 🤝 Aligned |  |
| R1.2 | v1 | P0 | The app shows what it detected — the header row and the row count — and shows the encoding and the delimiter only when detection failed or the user asks. A mapping is remembered by the file's header signature — the set of column names in any order, each compared by [R2.3](#2-target-mapping-and-the-matching-rule), a generated name for a blank header carrying its column's position — and arrives pre-filled for the next file with the same signature; the detection rules, and whether a remembered mapping becomes a named template, are OQ 1. | 🤝 Aligned |  |
| R1.3 | v1 | P0 | Four read failures each end the import with nothing imported, each its own named state: no header row detected ([E4](prd-inventory-import-copy.md#error--state-copy)), where the user names the columns or picks the header row; text that cannot be read ([E5](prd-inventory-import-copy.md#error--state-copy)), where the user chooses an encoding or re-saves the file; a file with zero data rows ([E6](prd-inventory-import-copy.md#error--state-copy)); and a file that has moved, been renamed, or gone away between being picked and being committed ([E38](prd-inventory-import-copy.md#error--state-copy)), which offers the file picker again. | 🤝 Aligned |  |
| R1.4 | v1 | P0 | Rows with the wrong number of columns are listed by row number and excluded, never silently imported ([E7](prd-inventory-import-copy.md#error--state-copy)); the user proceeds without them or fixes the file and re-picks it. | 🤝 Aligned |  |

### 2. Target, mapping and the matching rule

Traces [UJ 2](prd-inventory-import-journeys.md#uj-2-full-collection-bootstrap-via-csv-import), [UJ 2.2](prd-inventory-import-journeys.md#uj-22-mapping-metadata-fields); serves [U1](../vision.md#use-cases).

#### As a Cataloger, I can say where my file lands and what its columns mean so that my spreadsheet arrives as swatches rather than as text.


| ID | Release | Pri | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- | :--- | :--- |
| R2.1 | v1 | P0 | The target collection — new or existing — is chosen in the app before columns are mapped, and the collection name is never a mapped column in v1. A new collection collects the same name, samples-per-row, scan mode, and optional display defaults as [§1 of the capture PRD](../capture-mode/prd-capture-mode.md#1-collections). | 🤝 Aligned |  |
| R2.2 | v1 | P0 | Swatch Code must be mapped and the mapping cannot be saved without it ([E8](prd-inventory-import-copy.md#error--state-copy)); Swatch Name, Swatch Alternate Code, and Swatch Alternate Name are optional. Every other column is imported as extra metadata the collection can surface, and a column with a blank header arrives under a generated name listed in the preview ([E12](prd-inventory-import-copy.md#error--state-copy)). (Inherited obligation for the Collection Mode PRD: renaming an imported column.) | 🤝 Aligned |  |
| R2.3 | v1 | P0 | Two Swatch Codes, or two collection names, are the same when they match after the text is brought to one standard form, spaces at either end trimmed, inner runs of space collapsed to one, and letter case ignored the same way in every language; any whitespace character — a tab or a non-breaking space included — counts as a space. The value is shown exactly as entered, and this one rule governs uniqueness, re-import matching, find, the ad-hoc duplicate check, and collection names, exercised with accented, tab, and non-breaking-space inputs. (Inherited obligation for the Data Foundation PRD.) | 🤝 Aligned |  |
| R2.4 | v1 | P0 | Rows with a blank Swatch Code ([E9](prd-inventory-import-copy.md#error--state-copy)), and rows whose codes repeat within the file or merge into one another under [R2.3](#2-target-mapping-and-the-matching-rule) ([E10](prd-inventory-import-copy.md#error--state-copy)), are all listed by row number and excluded — every offending row, never first-row-wins, and the app never picks a winner. The user proceeds without them or fixes the file and re-picks it. | 🤝 Aligned |  |

### 3. Preview and commit

Traces [UJ 2](prd-inventory-import-journeys.md#uj-2-full-collection-bootstrap-via-csv-import), [UJ 2.1](prd-inventory-import-journeys.md#uj-21-import-additional-rows-into-an-existing-collection); serves [U1](../vision.md#use-cases).

#### As a Cataloger, I can see exactly what will land before it lands so that an import never surprises me.


| ID | Release | Pri | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- | :--- | :--- |
| R3.1 | v1 | P0 | A preview precedes every commit, showing the row count — for an existing collection the three-way count new / updated / unchanged plus a line reading "N rows in the collection are not in this file — left untouched" — and the issue list. A file that changed on disk between being read and being committed is re-read and re-previewed ([E13](prd-inventory-import-copy.md#error--state-copy)), and a file that would take the collection past ROWS_CEILING ([the capture PRD's R3.12](../capture-mode/prd-capture-mode.md#3-the-capture-session)) is warned about by number in the preview and imported anyway. | 🤝 Aligned |  |
| R3.2 | v1 | P0 | A commit is all-or-none, so a failure partway leaves the collection exactly as it was, and every imported row lands pending, in file order, appended after any existing rows. Import ends at ready-to-capture and never starts a session, and importing into a collection whose session is active, paused, or interrupted is refused, naming that session and offering both a way to it and a way to end it from here, so a blocked import is never a dead end ([E40](prd-inventory-import-copy.md#error--state-copy)). (Inherited obligation for the Data Foundation PRD: an import that either lands whole or not at all.) | 🤝 Aligned |  |


#### As a Cataloger, I can re-import a corrected file so that fixing my spreadsheet never costs me my measurements.


| ID | Release | Pri | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- | :--- | :--- |
| R3.3 | v1 | P0 | Swatch Code is how a file row is matched to a collection row: a matched row keeps its place, its state, and its measurements whatever the file says. Importing the same file twice changes nothing, a re-import never deletes a row, and rows the file does not mention are left exactly as they are. | 🤝 Aligned |  |
| R3.4 | v1 | P0 | A code in the file that matches more than one row in the collection is a hard failure listed in the preview ([E11](prd-inventory-import-copy.md#error--state-copy)) and is neither created nor updated. | 🤝 Aligned |  |
| R3.5 | v1 | P0 | Where an updated row is already captured, the preview offers a keep-or-overwrite toggle on each such row with one default applied to all of them, never a series of dialogs ([E14](prd-inventory-import-copy.md#error--state-copy)). The default is to take the new details, and the state says that measurements — the canonical value and its version history — are never touched either way. | 🤝 Aligned |  |
| R3.6 | v1 | P0 | A mapped column present in the file but blank for a row counts as a change, so it appears under "updated" and the preview says so. A column omitted from the file entirely leaves existing values untouched. | 🤝 Aligned |  |

### 4. Demo Device and verifiability

Traces [UJ 3.9](../capture-mode/prd-capture-mode-journeys.md#uj-39-capture-with-the-demo-device-contributor); serves [U9](../vision.md#use-cases) and vision [J5](../vision.md#j5-first-contribution-contributor). Builds on the [device PRD §6](../device-management/prd-device-management.md#6-mock-device-layer) mock-device layer, as the capture PRD's [§11](../capture-mode/prd-capture-mode.md#11-demo-device-and-verifiability) does; neither that layer's rules nor §11's are restated here.

#### As a Contributor, I can walk the import flow and read back what its preview offers so that I can contribute without an instrument or a license.


| ID | Release | Pri | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- | :--- | :--- |
| R4.1 | v1 | P0 | A test lists what the import preview shows, without matching wording ([R3.1](#3-preview-and-commit)). | 🤝 Aligned |  |
| R4.2 | v1 | P0 | A test observes, without matching wording, which named [copy](prd-inventory-import-copy.md#error--state-copy) state is up. | 🤝 Aligned |  |

### Inherited obligations

Each line is a requirement on the document named, not a suggestion. A row cited here carries the rule; a line with no row is one this document hands over whole.

| Target PRD | Obligation | Rows |
| :--- | :--- | :--- |
| Data Foundation | The one matching rule for Swatch Codes and collection names, and an import that either lands whole or not at all | [R2.3](#2-target-mapping-and-the-matching-rule), [R3.2](#3-preview-and-commit) |
| Collection Mode | Renaming an imported column | [R2.2](#2-target-mapping-and-the-matching-rule) |
| Capture Mode | A collection's pending rows, in file order, are what a session scans; an import is refused while a session on that collection is in flight, and the routes [E40](prd-inventory-import-copy.md#error--state-copy) offers resolve to the capture PRD's §7 paths — an active or paused session ended from there follows [the capture PRD's R7.5](../capture-mode/prd-capture-mode.md#7-pause-end-interruption-and-resume) | [R3.2](#3-preview-and-commit) |

### 5. Error & State Copy

The shipping copy for every error, waiting, choice, and confirmation state in this PRD is in [prd-inventory-import-copy.md](prd-inventory-import-copy.md). Every state there is a distinct named state whose identity is stable even when its wording changes, so behaviour can be asserted independently of copy ([R4.2](#4-demo-device-and-verifiability)). The Labels and Placeholders rules that govern that table are the [capture PRD's §12](../capture-mode/prd-capture-mode.md#12-error--state-copy)'s, read with "this table" meaning that one, and are not restated here.

## Success Metrics

Numeric targets below are proposals, not commitments.


| ID | Metric | Definition (start event, end event, statistic, population) | Candidate target | Method | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| M1 | Import success on real exports | Start: picking a file. End: a committed import. Statistic: the share of files that imported without the user editing the file first. Population: a corpus of real exports from the spreadsheets catalogers actually use — Numbers, Excel, Sheets. | ≥ 90% (proposal); the failures are what write the detection rules for OQ 1 | Timed walkthroughs against the corpus, n stated with the result | 🤝 Aligned |


How long an import takes to show up as capture is [the capture PRD's M8](../capture-mode/prd-capture-mode.md#success-metrics), which starts at [R3.2](#3-preview-and-commit)'s commit.

## Open Questions

One question, numbered from 1. It was OQ 12 in the capture PRD and moved here under fence F49; a question number is never reused.

| # | Question | Decision so far | Interim rule | Closer | Feeds | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | CSV detection, and named mapping templates | None. Open with it: whether a remembered mapping becomes a named, reusable template. | None; detection failures fall to the no-header-row and can't-read-the-file states. | Real exports from Numbers, Excel, and Sheets, with the detection rules written from what breaks — dogfood. | [R1.2](#1-reading-the-file), [M1](#success-metrics) | open |

Results file: [`prd-inventory-import-oq-results.md`](prd-inventory-import-oq-results.md), one `## OQ <id>` section per answer; an OQ's status may change only when its section exists there. A question number is never reused. Every provisional constant in this document carries its OQ id — a marker without a matching entry above, or in the capture PRD's [Open Questions](../capture-mode/prd-capture-mode.md#open-questions) where it says so, is invalid.
