# PRD: Inventory Import

Status: locked 2026-09-08; agent-build amendment 2026-09-17, pending merge.

Import a CSV inventory into one collection without entering metadata between scans. Import never starts capture; only new items become pending.

Companions: [acceptance journeys](prd-inventory-import-journeys.md), [shipping copy](prd-inventory-import-copy.md), [decisions and provenance](prd-inventory-import-fences.md), [open-question results](prd-inventory-import-oq-results.md).

## User Journeys

[UJ 2](prd-inventory-import-journeys.md#uj-2-full-collection-bootstrap-via-csv-import): new collection; [UJ 2.1](prd-inventory-import-journeys.md#uj-21-import-additional-rows-into-an-existing-collection): re-import; [UJ 2.2](prd-inventory-import-journeys.md#uj-22-mapping-metadata-fields): mapping. Their initial-state/action/result tables are acceptance cases for the requirements below.

## Requirements

### Vocabulary

- **Swatch Code:** required item match key, unique within a collection under R2.3; retain the entered text for display.
- **Swatch Name:** optional display name; **Swatch Alternate Code / Swatch Alternate Name:** optional additional search terms, never re-import match keys; search behavior belongs to [Capture R6.1](../capture-mode/prd-capture-mode.md#6-queue-navigation-and-reordering).
- **Header signature:** unordered set of R2.3 comparison keys for the final unique column names; generated names include their original column position.
- **Ready to capture:** import finished, new rows pending, existing rows preserved, no session started.
- **Row number:** one-based parsed record number in the source, including the header; a quoted multiline field does not add records.
- Pending, captured, and session use [Capture's vocabulary](../capture-mode/prd-capture-mode.md#vocabulary).

### Legend

All requirements are **v1 / P0**. Status and Commit PR track implementation: ⌛️ Ready for Alignment; ✋ Needs Discussion; 🤝 Aligned (specified, not implemented); 🦺 In Progress; ✅ Completed (merged PR linked); ✂️ Deferred. OQs are open or answered.

**Build dependencies:** use R1.5's detection baseline while OQ 1 remains open; named templates wait on OQ 2. [Capture OQ 13](../capture-mode/prd-capture-mode.md#open-questions) owns ROWS_TARGET, ROWS_CEILING and IMPORT_BUDGET; the engineering plan must set a dogfood import budget before performance checks, and unresolved provisional constants cannot ship in a release.

### Traceability

R, E, M, OQ, and UJ IDs are local to this PRD and never renumbered or reused. Requirements are normative; journeys exercise them and copy supplies their strings. Keep links to owning sibling rules rather than duplicating them; [fences and the historical ID map](prd-inventory-import-fences.md) preserve provenance, including F24, F45 and F49.

### Surfaces

Import flow: file selection and read settings → target collection → column mapping → preview and issue list → explicit commit. The [copy file](prd-inventory-import-copy.md#error--state-copy) enumerates states and offered actions; R3.8 defines their transitions. Collection entry points belong to [Capture](../capture-mode/prd-capture-mode.md#surfaces).

### 1. Reading the file

| ID | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- |
| R1.1 | Start import only by explicit action, accepting a file through the picker or drag and drop. | 🤝 Aligned | |
| R1.2 | Show the proposed header and data-row count; reveal encoding and delimiter on request or read failure. Remember mappings by header signature and pre-fill by column name, never by position, only after R2.5 validates the headers. | 🤝 Aligned | |
| R1.3 | Route missing headers to E4, unreadable text or invalid CSV syntax to E5, zero data records to E6, and a source moved, renamed or removed before commit to E38. These states write no imported data and resume only through R3.8's actions. | 🤝 Aligned | |
| R1.4 | List wrong-width records by source row number and exclude them (E7). Validate codes only on structurally valid records; never guess how a malformed record's fields align. | 🤝 Aligned | |
| R1.5 | Until OQ 1 closes, use the read contract below without additional guessing. Re-read and rebuild the preview after any read-setting change; keep all decoded field text, including leading zeros and whitespace, without numeric or date coercion. | 🤝 Aligned | |

**R1.5 — initial read contract**

| Input | Behavior |
| :--- | :--- |
| Encoding | Default UTF-8, accepting and stripping a leading UTF-8 BOM; manual choices UTF-8, UTF-16LE, UTF-16BE, Windows-1252; strip a matching leading BOM and reject a contradictory one. Decoding errors stop at E5; never replace invalid bytes silently. |
| Delimiter | Default comma; manual comma, semicolon or tab. No heuristic delimiter detection yet. |
| Records and quoting | Accept LF and CRLF record endings; double quotes delimit quoted fields, doubled quotes escape a quote, and quoted fields may contain delimiters and newlines. A terminal record ending adds no empty record; invalid quoting is E5, never an attempt to salvage shifted columns. |
| Header | Propose record 1 and allow the user to pick another record (earlier records ignored), or supply names with no header record (all records are data). An empty file or a proposed header with all fields blank goes to E4; generated names are for individual blank headers beside named ones. |
| Empty fields | Preserve empty strings, including trailing empty fields; a blank record is processed by the width and blank-code rules, not silently discarded. |

### 2. Target, mapping and the matching rule

| ID | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- |
| R2.1 | Choose or create one target collection before mapping; collection name is never a mapped field. Creation uses [Capture §1](../capture-mode/prd-capture-mode.md#1-collections), including name, samples-per-row, scan mode and optional display defaults. | 🤝 Aligned | |
| R2.2 | Require one source column for Swatch Code (E8); optionally map Swatch Name and both alternates, each source and target used at most once. Import remaining columns as metadata, preserving names, values and order under [Data Foundation R1.2](../data-foundation/prd-data-foundation.md#1-the-file-the-user-owns); subsequent imports retain existing column positions and append new columns in source order. | 🤝 Aligned | |
| R2.3 | For every code, collection-name or header comparison, trim Unicode White_Space at either end, collapse internal White_Space runs to U+0020, then apply Unicode canonical caseless matching (NFD → full default case fold → NFD), without locale tailoring or compatibility normalization. Use Unicode 17.0.0 for these comparisons and preserve entered text separately; uniqueness, re-import, find and ad-hoc duplicate checks share this rule. | 🤝 Aligned | |
| R2.4 | Exclude whitespace-only codes (E9) and every structurally valid record in a duplicate-code group under R2.3 (E10), listing source row numbers. No first-row-wins rule applies, and excluded records change nothing in the collection. | 🤝 Aligned | |
| R2.5 | Reject named headers that collide under R2.3, listing their names and positions (E41); do not restore or save a mapping until they are unique. For each blank header in source order, generate `Column N` using its one-based position, appending ` (2)`, ` (3)`, etc. until unique against all named and already generated headers under R2.3; show the resulting names (E12). | 🤝 Aligned | |

R2.3 defines observable equality using [Unicode 17.0.0 §3.13.5](https://www.unicode.org/versions/Unicode17.0.0/core-spec/chapter-3/); package selection and persistence of comparison keys remain engineering/ADR-0003 work. R2.2's passthrough columns reuse an existing stored column only when the name is exactly equal; a differently spelled name is a new column, even when its header signature restores the same identity mapping. Renaming stored columns remains Collection Mode's open obligation.

### 3. Preview and commit

| ID | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- |
| R3.1 | Before every commit, show new / updated / unchanged counts for eligible records, the excluded records with causes, and the count of existing items absent from the file; warn when existing item count plus new items exceeds ROWS_CEILING but allow import. If the source changes before commit, re-read it, revalidate headers and mapping, reset overwrite choices, and require a new preview (E13). | 🤝 Aligned | |
| R3.2 | Commit the previewed import whole or not at all, following the outcome table below, and never start a session. Refuse import while the target has an active, paused or interrupted session (E40), checking on target selection and again before commit. | 🤝 Aligned | |
| R3.3 | Match by Swatch Code using R2.3 and apply the outcome table, never deleting an existing item or changing its queue position, capture state or measurements. With the same mapping and choices and no intervening edits, repeating a committed file changes nothing. | 🤝 Aligned | |
| R3.4 | If a source code matches multiple existing items, exclude that source record (E11); change none of the matches and allow the other eligible records through preview. | 🤝 Aligned | |
| R3.5 | For each captured item with proposed metadata changes, offer keep/overwrite in the preview with an all-rows control, defaulting to overwrite (E14). Apply metadata changes to other matched items without this choice; neither path touches measurements or history. | 🤝 Aligned | |
| R3.6 | Apply the field-update table below to identity fields and passthrough metadata; a row is updated only if at least one stored value or metadata-column presence will change after its keep/overwrite choice. Recalculate counts when choices change; the omitted-item count is based on all structurally valid, nonblank source codes, including excluded duplicate or ambiguous codes. | 🤝 Aligned | |
| R3.7 | Creating a new target is the separate Capture §1 action and persists its empty collection; canceling or failing the import leaves it empty and leaves an existing target unchanged. If no source records remain eligible, show E42 with the issue list and disable commit; an all-unchanged import is eligible, with only any separately previewed column additions written. | 🤝 Aligned | |
| R3.8 | Offer the actions in the copy table and apply the transitions below; none implicitly commits. Cancel before commit exits without imported changes; an in-progress commit succeeds or rolls back under R3.2. | 🤝 Aligned | |

**Commit outcomes — R3.2/R3.3**

| Source disposition | Effect at commit |
| :--- | :--- |
| Eligible, no matching item | Create pending item without a measurement; append after existing rows, in source order. |
| Eligible, one match | Apply R3.5/R3.6 to metadata only; retain queue position, capture state, canonical value and all history. |
| Excluded record | No insert or update. |
| Existing item absent from source | No change. |

**Field updates — R3.6**

| Incoming field / selected choice | Result |
| :--- | :--- |
| Column omitted | Keep stored value. |
| Present, same decoded value (including blank-to-blank) | Unchanged. |
| Present, different decoded value | Replace; an empty string clears the old value. |
| Present, previously absent column | Add the column, even if its value is empty; retain the input name and text. |
| Captured row, keep selected | Keep all existing field values, including the displayed code spelling; create no row-specific imported values for that row. |

Adding a new passthrough column preserves its place in the collection's column list even if every matched captured row chooses keep; the preview lists added columns separately from row counts. Equality for metadata changes is exact decoded text, not R2.3's match equivalence.

**Action transitions — R3.8**

| State / action | Next state or effect |
| :--- | :--- |
| E4: choose header or name columns; E5: change encoding/delimiter | Re-read using the chosen settings, validate headers, then mapping and preview; if the selected settings still cannot parse the file, re-pick a corrected source. |
| E6, E38, E41, E42: pick/re-pick file | Read fresh, then mapping and preview. |
| E7, E9, E10, E11: continue without excluded rows | Keep all exclusions and return to the remaining mapping/preview steps; if none eligible, E42. |
| E8: choose code column | Return to mapping; continue only when valid. |
| E12: continue | Continue mapping, then preview. |
| E13: review again | Review fresh counts and choices; re-map first if headers changed. |
| E14: take details / keep | Update preview choices and counts; explicit commit still required. |
| E40: go to session | Leave import without changes; open the named session's current or resume surface. |
| E40: end session | Use [Capture R7.5/R7.13](../capture-mode/prd-capture-mode.md#7-pause-end-interruption-and-resume); canceled ending keeps import blocked, confirmed ending requires a fresh preview before import. |
| Cancel / pick another file wherever offered | Exit without imported changes / restart reading; preserve any separately created collection. |

### 4. Demo Device and verifiability

[UJ 2–2.2](prd-inventory-import-journeys.md#user-journeys) run without hardware or a license; use the existing [device/store test seams](../device-management/prd-device-management.md#6-mock-device-layer).

| ID | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- |
| R4.1 | Tests enumerate preview counts, exclusions, column additions, choices, and the actions offered in every import state, including all three E40 routes. Assert transitions and store effects against the acceptance journeys, including rollback and idempotent re-import. | 🤝 Aligned | |
| R4.2 | Tests observe named copy-state identity and cause variant independently of wording. Keep shipping-string checks separate from behavior assertions. | 🤝 Aligned | |

### Inherited obligations

| Owner | Contract |
| :--- | :--- |
| Data Foundation | R2.3 comparison semantics; R2.2 field preservation; R3.2 atomic import and R3.3 measurement preservation; decoded values stay directly queryable. |
| Collection Mode | Renaming imported columns (R2.2); whether a rename changes their stored names remains [post-lock work](../post-lock.md#cross-document). |
| Data Export | Export imported metadata and identity under [Export R2.3/R2.4](../export/prd-data-export.md#2-columns-names-dialect-and-the-version), preserving order and renaming export-name collisions without dropping columns. |
| Capture Mode | New items append pending; matched items retain state and position; E40's session-ending actions use Capture §7. Design scale and import timing remain [Capture R3.12/OQ 13](../capture-mode/prd-capture-mode.md#3-the-capture-session). |

### 5. Error & State Copy

[Shipping copy](prd-inventory-import-copy.md#error--state-copy) uses [Capture §12's label and placeholder rules](../capture-mode/prd-capture-mode.md#12-error--state-copy). E IDs identify states independently of strings; R3.8 owns action transitions.

## Success Metrics

| ID | Metric | Definition | Candidate target | Method | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| M1 | Import success on real exports | Start: file picked; end: committed import; share of Numbers, Excel and Sheets corpus files imported without editing the source file first. | ≥ 90%, proposal | Corpus walkthroughs with n, settings, failures and exclusions recorded; feeds OQ 1. | 🤝 Aligned |

[Capture M8](../capture-mode/prd-capture-mode.md#success-metrics) measures import commit to first captured row.

## Open Questions

| # | Question | Interim build contract | Closer / required evidence | Feeds | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | CSV detection | R1.5's explicit baseline; no additional heuristics. | Dogfood corpus of real Numbers, Excel and Sheets exports; record expected decoded fields, selected settings and failures, then publish deterministic detection/fallback rules and corpus results. | R1.2, R1.5, M1 | open |
| 2 | Named mapping templates | Automatic remembered mapping remains required; no named-template UI until decided. | Owner decides whether named templates belong in v1, informed by repeated mapping corrections from OQ 1's corpus/dogfood. | R1.2 | open |

OQ 2 was separated from OQ 1 in this amendment; neither is answered. [Results](prd-inventory-import-oq-results.md) require a matching `## OQ <id>` section before status changes; Capture OQ 13 retains ownership of the import performance constants.
