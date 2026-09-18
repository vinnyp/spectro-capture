# PRD: Inventory Import

Status: locked 2026-09-08; agent-build amendment with owner decisions F51–F61 dated 2026-09-17; PR #16 pending merge.

Import a CSV inventory into one collection without entering metadata between scans. Import never starts capture; only new items become pending.

Companions: [acceptance journeys](prd-inventory-import-journeys.md), [shipping copy](prd-inventory-import-copy.md), [decisions and provenance](prd-inventory-import-fences.md), [open-question results](prd-inventory-import-oq-results.md).

## User Journeys

[UJ 2](prd-inventory-import-journeys.md#uj-2-full-collection-bootstrap-via-csv-import): new collection; [UJ 2.1](prd-inventory-import-journeys.md#uj-21-import-additional-rows-into-an-existing-collection): re-import; [UJ 2.2](prd-inventory-import-journeys.md#uj-22-mapping-metadata-fields): mapping. Their initial-state/action/result tables are acceptance cases for the requirements below.

## Requirements

### Vocabulary

- **Swatch Code:** required item match key; R3.3 owns uniqueness within the target collection.
- **Swatch Name:** optional display name; **Swatch Alternate Code / Swatch Alternate Name:** optional additional search terms, never re-import match keys; search behavior belongs to [Capture R6.1](../capture-mode/prd-capture-mode.md#6-queue-navigation-and-reordering).
- **The one matching rule:** R2.3 comparison for codes, collection names and headers, used wherever those values are matched.
- **Header signature:** unordered set of R2.3 comparison keys for the final unique column names; generated names include their original column position.
- **Ready to capture:** import finished, new rows pending, existing rows preserved, no session started.
- **Row number:** one-based parsed record number from the start of the source, never reset after a selected header or ignored preamble; in a headerless file, record 1 is data, and a quoted multiline field counts once (R1.4).
- **Absent field:** the item has never received that field; distinct from a present field holding an empty string (R3.6).
- Pending, captured, set aside, and session use [Capture's vocabulary](../capture-mode/prd-capture-mode.md#vocabulary).

### Legend

All requirements are **v1 / P0**. Status and Commit PR track implementation: ⌛️ Ready for Alignment; ✋ Needs Discussion; 🤝 Aligned (specified, not implemented); 🦺 In Progress; ✅ Completed (merged PR linked); ✂️ Deferred. OQs are open or answered.

**Build dependencies:** ADR-0003 owns comparison data/version governance and must verify R2.3 against the macOS floor once ADR-0006 selects it; no minimum OS or package is decided here. Use R1.5's detection baseline while OQ 1 remains open; named templates wait on OQ 2. [Capture OQ 13](../capture-mode/prd-capture-mode.md#open-questions) owns ROWS_TARGET, ROWS_CEILING and IMPORT_BUDGET; the engineering plan must set a dogfood import budget before performance checks, and unresolved provisional constants cannot ship in a release.

### Traceability

R, E, M, OQ, and UJ IDs are local to this PRD and never renumbered or reused. Requirements are normative; journeys exercise them and copy supplies their strings. Keep links to owning sibling rules rather than duplicating them; [fences and the historical ID map](prd-inventory-import-fences.md) preserve provenance, including F24, F45 and F49. The 2026-09-16 R2.2 column-order amendment and Data Export obligation landed in PR #14 (Data Foundation’s outbound line); Commit PR is reserved for implementation PRs. F51–F61 record the owner’s individual decisions from [PR #16](https://github.com/vinnyp/spectro-capture/pull/16#issuecomment-5723537817).

### Surfaces

Import flow: file selection and read settings → target collection → column mapping → preview and issue list → explicit commit. The [copy file](prd-inventory-import-copy.md#error--state-copy) enumerates states and offered actions; R3.8 defines their transitions. Collection entry points belong to [Capture](../capture-mode/prd-capture-mode.md#surfaces).

### 1. Reading the file

| ID | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- |
| R1.1 | Start import only by explicit action, accepting a file through the picker or drag and drop. | 🤝 Aligned | |
| R1.2 | Show the proposed header and data-row count, and always show the active encoding and delimiter in the preview. Remember mappings by header signature and pre-fill by resolved column name, never by position, after R2.5 resolves headers and R1.6’s one-column guard is satisfied. | 🤝 Aligned | |
| R1.3 | Route missing headers to E4, unreadable text or invalid CSV syntax to E5, zero data records to E6, and a source moved, renamed or removed before commit to E38. These states write no imported data and resume only through R3.8's actions. | 🤝 Aligned | |
| R1.4 | List wrong-width records by one-based parsed record number from the source start, counting header and preamble records and each quoted multiline record once, and exclude them (E7). Validate codes only on structurally valid records; never guess field alignment. | 🤝 Aligned | |
| R1.5 | Until OQ 1 closes, use R1.5a–e; the listed encodings, delimiters and defaults are provisional under that question. Re-read and rebuild the preview after any read-setting change; keep all decoded field text, including leading zeros and whitespace, without numeric or date coercion. | 🤝 Aligned | |
| R1.6 | A read yielding exactly one column requires explicit confirmation (E45), with the delimiter control beside it, before a mapping can be saved or reused. Clear that confirmation on any re-read; this guard remains until OQ 1 closes. | 🤝 Aligned | |

**R1.5 — initial read contract**

| ID | Input | Behavior |
| :--- | :--- | :--- |
| R1.5a | Encoding | Default UTF-8, accepting and stripping a leading UTF-8 BOM; manual choices UTF-8, UTF-16LE, UTF-16BE, Windows-1252; strip a matching leading BOM and reject a contradictory one. Decoding errors stop at E5; never replace invalid bytes silently; if all listed encodings fail, offer re-saving as UTF-8 CSV, "Pick the file again", and "Cancel". |
| R1.5b | Delimiter | Default comma; manual comma, semicolon or tab. No heuristic delimiter detection yet. |
| R1.5c | Records and quoting | Accept LF and CRLF record endings; double quotes delimit quoted fields, doubled quotes escape a quote, and quoted fields may contain delimiters and newlines. A terminal record ending adds no empty record; invalid quoting is E5, never an attempt to salvage shifted columns. |
| R1.5d | Header | Propose record 1 and allow the user to pick another record (earlier records ignored), or supply names with no header record (all records are data). A source with no records goes directly to E6; a proposed header with all fields blank goes to E4; generated names are for individual blank headers beside named ones. |
| R1.5e | Empty fields | Preserve empty strings, including trailing empty fields; a blank record is processed by the width and blank-code rules, not silently discarded. |

### 2. Target, mapping and the matching rule

| ID | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- |
| R2.1 | Choose or create one target collection before mapping, using [Capture §1](../capture-mode/prd-capture-mode.md#1-collections) for creation settings; collection name is never a mapped field. Selecting an existing target adds no confirmation dialog: R3.1’s named-target preview is the confirmation. | 🤝 Aligned | |
| R2.2 | Require one source column for Swatch Code (E8); optionally map Swatch Name and both alternates, each source and target used at most once. Import remaining columns as metadata under R2.5/R2.6 and [Data Foundation R1.2](../data-foundation/prd-data-foundation.md#1-the-file-the-user-owns), preserving decoded values and existing column positions and appending new columns in source order. | 🤝 Aligned | |
| R2.3 | For every code, collection-name or header comparison, trim Unicode White_Space at either end, collapse internal White_Space runs to U+0020, then apply canonical caseless matching (NFD → full default case fold → NFD), without locale tailoring or compatibility normalization. Preserve entered text separately; uniqueness, re-import, find and ad-hoc duplicate checks share this rule. | 🤝 Aligned | |
| R2.4 | Exclude whitespace-only codes (E9) and every structurally valid record in a duplicate-code group under R2.3 (E10), listing source row numbers. No first-row-wins rule applies, and excluded records change nothing in the collection. | 🤝 Aligned | |
| R2.5 | Resolve header names before mapping: reserve all nonblank input names under R2.3, retain each group’s first occurrence, and suffix later occurrences in source order until unique against reserved and assigned names (E41). Then name blank headers by source position, suffixing collisions the same way (E12); show original names, positions and resolved names using the [copy file’s naming templates](prd-inventory-import-copy.md#generated-column-names), with "Continue with the listed names", "Pick the file again", and "Cancel". | 🤝 Aligned | |
| R2.6 | A resolved passthrough header equal under R2.3 to a stored column name reuses that column, retaining its first-seen spelling and position; otherwise append a new column. Renaming stored columns remains Collection Mode’s obligation. | 🤝 Aligned | |

### 3. Preview and commit

| ID | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- |
| R3.1 | Before every commit, E43 names the target collection and existing item count, encoding/delimiter, new / updated / unchanged counts, added columns, exclusions and absent-item count; warn with resulting size and ROWS_CEILING when exceeded, but allow import. If the source changes, re-read, revalidate headers/mapping, reset overwrite choices, and require a new preview (E13). | 🤝 Aligned | |
| R3.2 | Commit the previewed import whole or not at all under R3.3a–d, never starting a session; a failed commit rolls back and shows E44 with its cause, using [Data Foundation E15](../data-foundation/prd-data-foundation-copy.md#error--state-copy) for no room. Refuse import while the target has an active, paused or interrupted session (E40), checking on target selection and again before commit. | 🤝 Aligned | |
| R3.3 | Swatch Codes must be unique within the target collection under R2.3; import never creates a second item with an equal code, and R3.4 handles pre-existing duplicates defensively. Apply R3.3a–d without deleting or reordering existing items or changing their capture state or measurements; repeating a committed file with the same mapping/choices and no intervening edits is idempotent. | 🤝 Aligned | |
| R3.4 | If a source code matches multiple existing items, exclude that source record (E11); change none of the matches and allow the other eligible records through preview. | 🤝 Aligned | |
| R3.5 | For each captured item with proposed changes to existing field values, offer "Take the new details" / "Keep what I have" with an all-rows control, defaulting to "Take the new details" (E14). "Keep what I have" preserves conflicts in existing fields but fills previously absent fields; other matched items take supplied changes without this choice, and neither path touches measurements/history. | 🤝 Aligned | |
| R3.6 | Apply R3.6a–i to identity fields and passthrough metadata, comparing values as exact decoded text rather than R2.3 match equivalence. New columns persist in the collection’s column list even when every captured match selects "Keep what I have"; list additions separately from row counts. | 🤝 Aligned | |
| R3.7 | Creating a new target is the separate Capture §1 action and persists its empty collection; canceling or failing the import leaves it empty and leaves an existing target unchanged. If no source records remain eligible, show E42 with the issue list and disable commit; an all-unchanged import is eligible, with only the separately previewed column additions and previously absent fields written. | 🤝 Aligned | |
| R3.8 | Offer the copy table’s actions and apply R3.8a–o; only E43’s explicit "Import" commits. "Cancel" before commit exits without imported changes; an in-progress commit succeeds or rolls back under R3.2. | 🤝 Aligned | |

**Commit outcomes — R3.2/R3.3**

| ID | Source disposition | Effect at commit |
| :--- | :--- | :--- |
| R3.3a | Eligible, no matching item | Create pending item without a measurement; append after existing rows, in source order. |
| R3.3b | Eligible, one match | Apply R3.5/R3.6 to metadata only; retain queue position, capture state, canonical value and all history. |
| R3.3c | Excluded record | No insert or update. |
| R3.3d | Existing item absent from source | No change. |

**Field updates and counts — R3.6**

| ID | Incoming field / selected choice | Result |
| :--- | :--- | :--- |
| R3.6a | Column omitted | Keep stored value. |
| R3.6b | Present field, same decoded value, including blank-to-blank | Unchanged. |
| R3.6c | Present field, different value; uncaptured item or "Take the new details" | Replace; an empty string clears the old value. |
| R3.6d | Previously absent field, including a captured item selecting "Keep what I have" | Store the incoming value, even if empty; create its column if needed under R2.6, with additions listed separately. |
| R3.6e | Previously present field, captured item selecting "Keep what I have" | Preserve the stored value, including blank values and displayed code spelling. |
| R3.6f | Matched pending/set-aside item, code spelling differs but is R2.3-equal | Replace displayed code with incoming spelling as a metadata update; item identity, order and measurements stay unchanged. |
| R3.6g | Classifying eligible records | No matching item → new; matched item with at least one previously present value changing after choices → updated; otherwise unchanged, even if previously absent fields are filled. Column creation by itself never counts as a row update. |
| R3.6h | Counting absent items | Count existing items whose code appears in none of the structurally valid, nonblank source codes; excluded duplicate/ambiguous codes still count as present. |
| R3.6i | Choices change | Recalculate preview counts before commit. |

**Action transitions — R3.8**

| ID | State / action | Next state or effect |
| :--- | :--- | :--- |
| R3.8a | E4: "Pick the header row" / "Name columns"; E5/E45: "Choose an encoding" / "Choose a separator" where offered | Re-read, revalidate headers and the one-column guard, then mapping/preview; another decoding or syntax failure returns to E5. |
| R3.8b | E5/E7/E9/E10/E12/E38/E41/E42: "Pick the file again"; E6: "Pick a different file" | Read fresh, then header resolution, guard, mapping and preview. |
| R3.8c | E7/E9/E10: "Continue without them"; E11: "Continue without it" | Keep all exclusions; return to remaining mapping/preview steps, or E42 if none eligible. |
| R3.8d | E8: "Choose the code column" | Return to mapping; continue only when valid. |
| R3.8e | E12/E41: "Continue with the listed names" | Accept the listed resolved headers; continue through the guard and mapping to preview. |
| R3.8f | E13: "Review again" | Review fresh counts and reset choices; re-map first if the signature changed. |
| R3.8g | E14: "Take the new details" / "Keep what I have" | Update choices and counts; explicit "Import" still required. |
| R3.8h | E40: "Go to the session" | Leave import without changes; open the named session’s current or resume surface. |
| R3.8i | E40: "End that session" | Use [Capture R7.5/R7.13](../capture-mode/prd-capture-mode.md#7-pause-end-interruption-and-resume); canceled ending stays blocked, confirmed ending returns through a fresh preview. |
| R3.8j | Every import state: "Cancel" | Exit without imported changes; preserve any separately created collection. |
| R3.8k | E43: "Import" | Recheck source and session gate, then commit once; a changed source → E13, blocked session → E40, write failure → E44. |
| R3.8l | E44: "Try again" | Retry from a fresh read and preview; never repeat a write blindly or insert duplicate rows. |
| R3.8m | E45: "Use this one column" | Confirm the current parse; enable valid mapping save/reuse, then preview. |
| R3.8n | Encoding/delimiter controls on request or in preview | Re-read and invalidate previous preview and one-column confirmation; apply R3.8a. |
| R3.8o | Multiple header notices | "Continue with the listed names" accepts all listed E12/E41 resolutions together; no notice skips mapping or preview. |

### 4. Demo Device and verifiability

[UJ 2–2.2](prd-inventory-import-journeys.md#user-journeys) run without hardware or a license; use the existing [device/store test seams](../device-management/prd-device-management.md#6-mock-device-layer).

| ID | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- |
| R4.1 | Tests enumerate preview counts, exclusions, column additions, choices, and the actions offered in every import state, including all three E40 routes. Assert transitions and store effects against the acceptance journeys, including rollback and idempotent re-import. | 🤝 Aligned | |
| R4.2 | Tests observe named copy-state identity and cause variant independently of wording. Keep shipping-string checks separate from behavior assertions. | 🤝 Aligned | |

### Inherited obligations

Each line is a requirement on the document named, not a suggestion; the Rows column identifies the import rules it carries.

| Target PRD | Obligation | Rows |
| :--- | :--- | :--- |
| Data Foundation | The one matching rule; field preservation and column identity; atomic imports; measurement preservation on re-import; decoded values stay directly queryable ([DF inbound mirror](../data-foundation/prd-data-foundation.md#inherited-obligations)). | R2.2, R2.3, R2.5, R2.6, R3.2, R3.3 |
| Collection Mode | Renaming imported columns; whether a rename changes their stored names remains [post-lock work](../post-lock.md#cross-document). | R2.2, R2.6 |
| Data Export | Every column an import brought in is carried through, and a column mapped to identity is emitted once rather than twice; order and export-name collisions follow [Export R2.3/R2.4](../export/prd-data-export.md#2-columns-names-dialect-and-the-version). | R2.2, R2.6 |
| Capture Mode | New items append pending and matched items retain state/position; E40’s "Go to the session" and "End that session" use Capture’s current/resume and §7 ending paths. Design scale and import timing remain [Capture R3.12/OQ 13](../capture-mode/prd-capture-mode.md#3-the-capture-session). | R3.2, R3.3, R3.8h–i |

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
| 1 | CSV detection | R1.5’s lists/defaults are provisional; R1.6’s one-column guard and visible preview settings remain until detection closes. | Dogfood corpus of real Numbers, Excel and Sheets exports; record expected decoded fields, selected settings and failures, then publish deterministic detection/fallback rules and corpus results. | R1.2, R1.5, R1.6, M1 | open |
| 2 | Named mapping templates | Automatic remembered mapping remains required; no named-template UI until decided. | Owner decides whether named templates belong in v1, informed by repeated mapping corrections from OQ 1's corpus/dogfood. | R1.2 | open |

OQ 2 was separated from OQ 1 in this amendment; neither is answered. [Results](prd-inventory-import-oq-results.md) require a matching `## OQ <id>` section before status changes; Capture OQ 13 retains ownership of the import performance constants.
