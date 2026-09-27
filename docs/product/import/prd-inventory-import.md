# PRD: Inventory Import

Status: locked 2026-09-08; agent-build amendment with owner decisions F51–F64 dated 2026-09-17; PR #16; Collection Mode rename mirror under F65 dated 2026-09-24 (R2.6 amended with alignment kept; peer review closed 2026-09-25 (PR #21)); Collection Mode one-writer mirror under F66 dated 2026-09-25 (R3.2 amended with alignment kept; E40 gains an another-collection variant; peer review closed 2026-09-25 (PR #21)); Collection Mode round-4 mirror under F67 dated 2026-09-25 (R3.2 and R3.8i amended with alignment kept; E40 reworded, ready for alignment; peer review closed 2026-09-25 (PR #21)); Collection Mode round-5 mirror under F68 dated 2026-09-25 (R4.1 amended with alignment kept; the Collection Mode obligation line names R2.3; peer review closed 2026-09-25 (PR #21)); Nix Toolkit export amendment under F69–F91 dated 2026-09-26 (§6 added; line 5, the Vocabulary, Build dependencies, R1.5, R3.2, R3.3, R3.5, R3.8, §4, M2, OQ 3 and the obligation lines amended; E5, E7, E9, E10, E14, E42 and E43 gain Toolkit variants, E13 a collection variant, E46–E49 new; peer review pending).

Import a CSV inventory into one collection without entering metadata between scans. Import never starts capture; only new items become pending and existing items keep their state, except a Nix Toolkit export's records, which arrive captured with their readings (§6).

Companions: [acceptance journeys](prd-inventory-import-journeys.md), [shipping copy](prd-inventory-import-copy.md), [decisions and provenance](prd-inventory-import-fences.md), [open-question results](prd-inventory-import-oq-results.md).

## User Journeys

[UJ 2](prd-inventory-import-journeys.md#uj-2-full-collection-bootstrap-via-csv-import): new collection; [UJ 2.1](prd-inventory-import-journeys.md#uj-21-import-additional-rows-into-an-existing-collection): re-import; [UJ 2.2](prd-inventory-import-journeys.md#uj-22-mapping-metadata-fields): mapping; [UJ 3](prd-inventory-import-journeys.md#uj-3-import-a-nix-toolkit-export-with-its-readings): a Nix Toolkit export. Their initial-state/action/result tables are acceptance cases for the requirements below.

## Requirements

### Vocabulary

- **Swatch Code:** required item match key; R3.3 owns uniqueness within the target collection.
- **Swatch Name:** optional display name; **Swatch Alternate Code / Swatch Alternate Name:** optional additional search terms, never re-import match keys; search behavior belongs to [Capture R6.1](../capture-mode/prd-capture-mode.md#6-queue-navigation-and-reordering).
- **The one matching rule:** R2.3 comparison for codes, collection names and headers, used wherever those values are matched.
- **Header signature:** unordered set of R2.3 comparison keys for the final unique column names; generated names include their original column position.
- **Ready to capture:** import finished, new rows pending, existing rows preserved, no session started; a Toolkit export's rows arrive captured instead (§6).
- **Nix Toolkit:** the vendor's mobile app, whose collection CSV §6 imports; not the vendor SDK other PRDs call the toolkit.
- **Same reading:** two Toolkit readings whose Date Saved is the same instant to the millisecond and whose every reflectance is the same number, whatever their written form (R6.8f).
- **Record number:** one-based parsed record number from the start of the source, never reset after a selected header or ignored preamble; in a headerless file, record 1 is data, and a quoted multiline field counts once (R1.4).
- **Absent field:** the item has never received that field; distinct from a present field holding an empty string (R3.6).
- Pending, captured, set aside, and session use [Capture's vocabulary](../capture-mode/prd-capture-mode.md#vocabulary).

### Legend

All requirements are **v1 / P0**. Status and Commit PR track implementation: ⌛️ Ready for Alignment; ✋ Needs Discussion; 🤝 Aligned (specified, not implemented); 🦺 In Progress; ✅ Completed (merged PR linked); ✂️ Deferred. OQs are open or answered.

**Build dependencies:** ADR-0003 owns comparison data/version governance and must verify R2.3 against the macOS floor once ADR-0006 selects it; no minimum OS or package is decided here. Use R1.5's detection baseline while OQ 1 remains open; named templates wait on OQ 2. [Capture OQ 13](../capture-mode/prd-capture-mode.md#open-questions) owns ROWS_TARGET, ROWS_CEILING and IMPORT_BUDGET; the engineering plan must set a dogfood import budget before performance checks, with release readiness governed by [Capture’s provisional-constant rule](../capture-mode/prd-capture-mode.md#legend) and OQ 13. §6 builds after [Device R1.21](../device-management/prd-device-management.md#1-device-pairing)'s imported kind and [DF R2.3j](../data-foundation/prd-data-foundation.md#2-canonical-value-and-version-history) and R3.1–R3.4's derivation run without an instrument; its check uses [DF OQ 6](../data-foundation/prd-data-foundation.md#open-questions)'s DERIVATION_TOLERANCE, a provisional constant that gates release.

### Traceability

R, E, M, OQ, and UJ IDs are local to this PRD and never renumbered or reused. Requirements are normative; journeys exercise them and copy supplies their strings. Keep links to owning sibling rules rather than duplicating them; [fences and the historical ID map](prd-inventory-import-fences.md) preserve provenance, including F24, F45 and F49. The 2026-09-16 R2.2 column-order amendment and Data Export obligation landed in PR #14 (Data Foundation’s outbound line); Commit PR is reserved for implementation PRs. F51–F61 record the owner’s [first-round decisions](https://github.com/vinnyp/spectro-capture/pull/16#issuecomment-5723537817); F62–F64 record the [second-round decisions](https://github.com/vinnyp/spectro-capture/pull/16#issuecomment-5723905420); F65 mirrors the Collection Mode PRD's F9 (2026-09-24); F66 mirrors its F149 (2026-09-25); F67 mirrors its F155, F156 and F169 (2026-09-25); F68 mirrors its F188 (2026-09-25); F69–F91 record the Nix Toolkit export amendment (2026-09-26), from owner decisions N1–N23 in its review log.

### Surfaces

Import flow: file selection and read settings → target collection → column mapping → preview and issue list → explicit commit. The [copy file](prd-inventory-import-copy.md#error--state-copy) enumerates states and offered actions; R3.8 defines their transitions. Collection entry points belong to [Capture](../capture-mode/prd-capture-mode.md#surfaces).

### 1. Reading the file

| ID | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- |
| R1.1 | Start import only by explicit action, accepting a file through the picker or drag and drop. | 🤝 Aligned | |
| R1.2 | Show the proposed header and data-row count, and always show the active encoding and delimiter in the preview. Remember mappings by header signature and pre-fill by resolved column name, never by position, after R2.5 resolves headers and R1.6’s one-column guard is satisfied. | 🤝 Aligned | |
| R1.3 | Route missing headers to E4, unreadable text or invalid CSV syntax to E5, zero data records to E6, and a source moved, renamed or removed before commit to E38. These states write no imported data and resume only through R3.8's actions. | 🤝 Aligned | |
| R1.4 | List wrong-width records by one-based parsed record number from the source start, counting header and preamble records and each quoted multiline record once, and exclude them (E7). Validate codes only on structurally valid records; never guess field alignment. | 🤝 Aligned | |
| R1.5 | Until OQ 1 closes, use R1.5a–e; the listed encodings, delimiters and defaults are provisional under that question. Re-read, reset overwrite choices, and rebuild the preview after any read-setting change; keep all decoded field text, including leading zeros and whitespace, without numeric or date coercion, except §6's reading columns (R6.7). | ⌛️ Ready for Alignment | |
| R1.6 | A read yielding exactly one column requires explicit confirmation (E45), with the delimiter control beside it, before a mapping can be saved or reused or the import committed. Clear that confirmation on any re-read; this guard remains until OQ 1 closes. | 🤝 Aligned | |

**R1.5 — initial read contract**

| ID | Input | Behavior |
| :--- | :--- | :--- |
| R1.5a | Encoding | Default UTF-8, accepting and stripping a leading UTF-8 BOM; manual choices UTF-8, UTF-16LE, UTF-16BE, Windows-1252; strip a matching leading BOM and reject a contradictory one. Decoding errors stop at E5; never replace invalid bytes silently; show the all-encodings-failed variant only after the default attempt and manual choices have attempted every listed encoding and failed for the current source and delimiter, offering re-saving as UTF-8 CSV, "Pick the file again", and "Cancel". |
| R1.5b | Delimiter | Default comma; manual comma, semicolon or tab. No heuristic delimiter detection yet, except R6.1's Toolkit signature. |
| R1.5c | Records and quoting | Accept LF and CRLF record endings; double quotes delimit quoted fields, doubled quotes escape a quote, and quoted fields may contain delimiters and newlines. A terminal record ending adds no empty record; invalid quoting is E5, never an attempt to salvage shifted columns. |
| R1.5d | Header | Propose record 1 and allow the user to pick another record (earlier records ignored), or supply names with no header record (all records are data). A source with no records goes directly to E6; a proposed header with all fields blank goes to E4; generated names are for individual blank headers beside named ones. |
| R1.5e | Empty fields | Preserve empty strings, including trailing empty fields; a blank record is processed by the width and blank-code rules, not silently discarded. |

### 2. Target, mapping and the matching rule

| ID | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- |
| R2.1 | Choose or create one target collection before mapping, using [Capture §1](../capture-mode/prd-capture-mode.md#1-collections) for creation settings; collection name is never a mapped field. Selecting an existing target adds no confirmation dialog: R3.1’s named-target preview is the confirmation. | 🤝 Aligned | |
| R2.2 | Require one source column for Swatch Code (E8); optionally map Swatch Name and both alternates, each source and target used at most once. Import remaining columns as metadata under R2.5/R2.6 and [Data Foundation R1.2](../data-foundation/prd-data-foundation.md#1-the-file-the-user-owns), preserving decoded values and existing column positions and appending new columns in source order. | 🤝 Aligned | |
| R2.3 | For every code, collection-name or header comparison, trim Unicode White_Space at either end, collapse internal White_Space runs to U+0020, then apply canonical caseless matching (NFD → full default case fold → NFD), without locale tailoring or compatibility normalization. Preserve entered text separately; uniqueness, re-import, find and ad-hoc duplicate checks share this rule. | 🤝 Aligned | |
| R2.4 | Exclude whitespace-only codes (E9) and every structurally valid record in a duplicate-code group under R2.3 (E10), listing source record numbers. No first-row-wins rule applies, and excluded records change nothing in the collection. | 🤝 Aligned | |
| R2.5 | Resolve header names before mapping: reserve all nonblank input names under R2.3, retain each group’s first occurrence, and suffix later occurrences in source order until unique against reserved and assigned names (E41). Then name blank headers by source position, suffixing collisions the same way (E12); show original names, positions and resolved names using the [copy file’s naming templates](prd-inventory-import-copy.md#generated-column-names), with "Continue with the listed names", "Pick the file again", and "Cancel". | 🤝 Aligned | |
| R2.6 | A resolved passthrough header equal under R2.3 to a stored column name reuses that column regardless of how its name arose, retaining its stored spelling — first-seen unless renamed in Collection Mode — and position; otherwise, including a header equal only to a column's name before such a rename, append a new column. Reuse within a collision group follows R2.5’s source-order resolution; renaming stored columns, which changes the stored name, is [Collection Mode R4.8](../collection-mode/prd-collection-mode.md#4-item-detail-and-editing)’s (F65). | 🤝 Aligned | |

### 3. Preview and commit

| ID | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- |
| R3.1 | Before every commit, E43 names the target collection and existing item count, encoding/delimiter, new / updated / unchanged counts, added columns, the count of matched rows gaining absent fields (R3.6j), exclusions and absent-item count, applying [Capture §12’s zero-count rule](../capture-mode/prd-capture-mode.md#12-error--state-copy); warn with resulting size and ROWS_CEILING when exceeded, but allow import. If the source changes, re-read, revalidate headers/mapping, reset overwrite choices, and require a new preview (E13). | 🤝 Aligned | |
| R3.2 | Commit the previewed import whole or not at all under R3.3a–d — for a Toolkit export, R6.8a–g with their derived values and R6.4's mode adoption — within [DF R1.7/R1.10’s local-volume scope](../data-foundation/prd-data-foundation.md#1-the-file-the-user-owns), never starting a session; a failed commit rolls back within that scope and shows E44 with its cause, using [Data Foundation E15](../data-foundation/prd-data-foundation-copy.md#error--state-copy) for no room. Refuse import while the target has an active, paused or interrupted session (E40), checking on target selection and again before commit, and refuse the commit while any other collection's session is active, paused or halted, E40 naming that collection (F66); the commit follows [DF R1.11](../data-foundation/prd-data-foundation.md#1-the-file-the-user-owns)'s one-writer rule, shown disabled while another write it names runs and holding every other write and a session's start or resume while it commits (F66, F67). | ⌛️ Ready for Alignment | |
| R3.3 | Swatch Codes must be unique within the target collection under R2.3; import never creates a second item with an equal code, and R3.4 handles pre-existing duplicates defensively. Apply R3.3a–d without deleting or reordering existing items or changing their capture state or measurements, except a Toolkit export's readings under R6.8; repeating a committed file with the same mapping/choices and no intervening edits is idempotent, readings included. | ⌛️ Ready for Alignment | |
| R3.4 | If a source code matches multiple existing items, exclude that source record (E11); change none of the matches and allow the other eligible records through preview. | 🤝 Aligned | |
| R3.5 | For each captured item with proposed changes to existing field values, offer "Take the new details" / "Keep what I have" as actions scoped to that row or all rows, defaulting to "Take the new details" (E14). "Keep what I have" preserves conflicts in existing fields but fills previously absent fields; other matched items take supplied changes without this choice, and neither path touches measurements/history, except a Toolkit export's readings under R6.8, which E14's Toolkit variant names. | ⌛️ Ready for Alignment | |
| R3.6 | Apply R3.6a–j to identity fields and passthrough metadata, comparing values as exact decoded text rather than R2.3 match equivalence. New columns persist in the collection’s column list even when every captured match selects "Keep what I have"; list additions separately from row counts. | 🤝 Aligned | |
| R3.7 | Creating a new target is the separate Capture §1 action and persists its empty collection (F64). Canceling or failing the import preserves that collection and an existing target under R3.2’s rollback scope; R3.9 owns commit eligibility. | 🤝 Aligned | |
| R3.8 | Offer the copy table’s actions and apply R3.8a–q; only E43’s explicit "Import" commits. "Cancel" before commit exits without imported changes; an in-progress commit succeeds or rolls back under R3.2. | ⌛️ Ready for Alignment | |
| R3.9 | If no source records remain eligible, show E42 with the issue list and disable commit. An all-unchanged import remains eligible, with only the column additions and absent-field fills separately previewed by E43’s added-columns list and rows-gaining-details line written (F62). | 🤝 Aligned | |

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
| R3.6j | Counting rows gaining absent fields | Count each eligible matched item receiving at least one previously absent field once, including empty values, whether the column is new or already exists. This tally overlaps updated/unchanged counts rather than adding a fourth disposition; show it on E43’s rows-gaining-details line. |

**Action transitions — R3.8**

| ID | State / action | Next state or effect |
| :--- | :--- | :--- |
| R3.8a | E4: "Pick the header row" / "Name columns"; E5/E45: "Choose an encoding" / "Choose a separator" where offered | Re-read, revalidate headers and the one-column guard, then mapping/preview; another decoding or syntax failure returns to E5. |
| R3.8b | E5/E7/E9/E10/E12/E38/E41/E42/E46/E47/E49: "Pick the file again"; E6: "Pick a different file" | Read fresh, then header resolution, guard, mapping and preview. |
| R3.8c | E7/E9/E10/E47: "Continue without them"; E11: "Continue without it" | Keep all exclusions; return to remaining mapping/preview steps, or E42 if none eligible. |
| R3.8d | E8: "Choose the code column" | Return to mapping; continue only when valid. |
| R3.8e | E12/E41: "Continue with the listed names" | Accept the listed resolved headers; continue through the guard and mapping to preview. |
| R3.8f | E13: "Review again" | Review fresh counts and reset choices, except that E13's collection variant keeps each still-offered row's choice; re-map first if the signature changed. |
| R3.8g | E14: "Take the new details" / "Keep what I have", scoped to one row or all rows | Update the selected row’s choice or every offered row’s choice, and recalculate counts; explicit "Import" still required. |
| R3.8h | E40: "Go to the session" | Leave import without changes; open the named session’s current or resume surface. |
| R3.8i | E40: "End that session" | Use [Capture R7.5/R7.13](../capture-mode/prd-capture-mode.md#7-pause-end-interruption-and-resume); canceled ending stays blocked, confirmed ending returns through a fresh preview; shown disabled while a [DF R1.11](../data-foundation/prd-data-foundation.md#1-the-file-the-user-owns) write runs. |
| R3.8j | Every import state: "Cancel" | Exit without imported changes; preserve any separately created collection. |
| R3.8k | E43: "Import" | Require E45 confirmation for a one-column read before E43; recheck the source, then, within the commit's write hold ([DF R1.11](../data-foundation/prd-data-foundation.md#1-the-file-the-user-owns)), the session gate and, for a Toolkit export, R6.4's mode fit and every eligible record's match and R6.8 outcome; then commit once. In that order: a changed source → E13; a blocked session → E40; a target that no longer fits → E46; a target whose mode, any eligible record's match or R6.8 outcome, or any count or list E43 showed changed since the preview → E13's collection variant and a fresh preview, E14's choices kept; a write failure → E44. |
| R3.8l | E44: "Try again" | Retry from a fresh read and preview; never repeat a write blindly or insert duplicate rows. |
| R3.8m | E45: "Use this one column" | Confirm the current parse; enable valid mapping save/reuse, then preview. |
| R3.8n | E43: "Choose an encoding" / "Choose a separator" | Re-read, reset overwrite choices, and invalidate previous preview and one-column confirmation; revalidate through R3.8a before returning to preview. |
| R3.8o | Multiple header notices | "Continue with the listed names" accepts all listed E12/E41 resolutions together; no notice skips mapping or preview. |
| R3.8p | E46: "Choose another collection" | Return to target selection with the read kept; the mode check runs again on the new target (R6.4). |
| R3.8q | E48: "Continue" | Accept the fixed mapping and continue to preview. |

### 4. Demo Device and verifiability

[UJ 2–2.2 and UJ 3](prd-inventory-import-journeys.md#user-journeys) run without hardware or a license; use the existing [device/store test seams](../device-management/prd-device-management.md#6-mock-device-layer).

| ID | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- |
| R4.1 | Tests enumerate R1.5a–e’s read contract, R2.3’s equality cases, preview counts, exclusions, column additions, choices, and the actions offered in every import state, including all three E40 routes, and a commit's progress. Assert transitions and store effects against the acceptance journeys, including rollback and idempotent re-import. | 🤝 Aligned | |
| R4.2 | Tests observe named copy-state identity and cause variant independently of wording. Keep shipping-string checks separate from behavior assertions. | 🤝 Aligned | |

### Inherited obligations

Each line is a requirement on the document named, not a suggestion; the Rows column identifies the import rules it carries.

| Target PRD | Obligation | Rows |
| :--- | :--- | :--- |
| Data Foundation | The one matching rule; field preservation and column identity; atomic imports within R3.2’s cited storage scope; measurement preservation on re-import; decoded values stay directly queryable ([DF inbound mirror](../data-foundation/prd-data-foundation.md#inherited-obligations)). A Toolkit export's reading is stored and made current or kept in history as [its R2.3j](../data-foundation/prd-data-foundation.md#2-canonical-value-and-version-history) states, a same reading creating none (R6.8f). UJ 3's Fixture T spectra join [its R7.5](../data-foundation/prd-data-foundation.md#7-verifiability) check (R6.10). | R2.2, R2.3, R2.5, R2.6, R3.2, R3.3, R6.5, R6.8, R6.10 |
| Collection Mode | Renaming imported columns: a rename changes the column's stored name, keeping its position and values, and a later import matches the new name ([Collection Mode R4.8](../collection-mode/prd-collection-mode.md#4-item-detail-and-editing), F65). The one matching rule for codes and collection names, which Collection Mode's rename, search and code change apply ([Collection Mode R1.3, R3.1 and R4.4](../collection-mode/prd-collection-mode.md#1-collections-and-the-collection-list), F68). Its R2.5 collision order and its copy file's Collision template, which Collection Mode's tagged-label collision form takes ([Collection Mode R2.1](../collection-mode/prd-collection-mode.md#2-the-collection-table-and-colour-honesty), the Collection Mode PRD's F206). A Toolkit reading carries the imported mark, and the item detail and history show what its file does not record as unknown or not recorded ([Collection Mode R2.4j, R2.7, R4.2d, R5.2b and R5.2e](../collection-mode/prd-collection-mode.md#2-the-collection-table-and-colour-honesty)). | R2.2, R2.3, R2.5, R2.6, R6.5 |
| Data Export | Every column an import brought in is carried through, and a column mapped to identity is emitted once rather than twice; order and export-name collisions follow [Export R2.3/R2.4](../export/prd-data-export.md#2-columns-names-dialect-and-the-version). A Toolkit reading exports marked by `sc_imported`, its unrecorded fields empty ([Export R1.2 and R1.1t](../export/prd-data-export.md#1-what-the-export-contains)). | R2.2, R2.6, R6.5 |
| Device Management | A Toolkit reading's acquiring-device snapshot is of the imported kind, naming the model its file gives, if any, serial and firmware unknown ([Device R1.21](../device-management/prd-device-management.md#1-device-pairing)); the import creates and occupies no saved-device record ([Device R1.22](../device-management/prd-device-management.md#1-device-pairing)). | R6.5 |
| Capture Mode | New items append pending and matched items retain state/position, except a Toolkit export's records: new ones arrive captured, and a matched item with no current value becomes captured (R6.8); E40’s "Go to the session" and "End that session" use Capture’s current/resume and §7 ending paths. Design scale and import timing remain [Capture R3.12/OQ 13](../capture-mode/prd-capture-mode.md#3-the-capture-session). A new target created by a Toolkit import takes the file's measurement mode as its chosen scan mode, its creation form reading "Measurement condition: ⟨mode⟩, set by this Nix Toolkit export", and an existing one must use it or hold no reading, adopting it with the commit ([Capture R1.10](../capture-mode/prd-capture-mode.md#1-collections)). A Toolkit export's import is left out of [Capture M8](../capture-mode/prd-capture-mode.md#success-metrics). | R3.2, R3.3, R3.8h–i, R6.3, R6.4, R6.8 |

### 5. Error & State Copy

[Shipping copy](prd-inventory-import-copy.md#error--state-copy) uses [Capture §12's label and placeholder rules](../capture-mode/prd-capture-mode.md#12-error--state-copy). E IDs identify states independently of strings; R3.8 owns action transitions.


### 6. Nix Toolkit exports

A Nix Toolkit export — the vendor mobile app's collection CSV — imports through this same flow with its readings (F69–F91). §6 states only where it differs from §1–§3; every rule there that §6 does not replace still applies.

| ID | Requirement | Status | Commit PR |
| :--- | :--- | :--- | :--- |
| R6.1 | A source whose first record, under the encoding it is read with and split on the semicolon, the comma or the tab — tried in that order, the first that fits used — holds under R2.3 the headers Custom Collection Name, Color Name, Color Code, Nix Device, Date Saved, Illuminant, Observer, Measurement Mode and R400 nm through R700 nm at every 10 nm is a Toolkit export: it is read as UTF-8 with that delimiter, both shown fixed, E43 offering neither "Choose an encoding" nor "Choose a separator"; mapping (E48) and preview (E43) name it a Toolkit export. A Toolkit export not decoded as UTF-8 throughout reaches E5's Toolkit encoding variant, and one with a record it cannot parse under R1.5c's quoting E5's Toolkit quotes variant, naming that record; the encoding variant is checked first, and neither offers a read control; E5, E7, E9, E10 and E42 show their Toolkit variants. Any other source follows §1–§3 unchanged (F73). | ⌛️ Ready for Alignment | |
| R6.2 | A Toolkit export's mapping is fixed and shown, never chosen (E48): Color Code is Swatch Code and Color Name is Swatch Name. Nix Device, Date Saved, Illuminant, Observer, Measurement Mode and the reflectances form each record's reading (R6.5); the file's first L, a and b are read for R6.6; Custom Collection Name is read for R6.3 and R6.11; Index and the file's other L, c, h, X, Y, Z, sRGB R, G, B and HEX are ignored — none of these is stored as a column. Note and the five Density columns import as metadata under R2.2/R2.6, a Note holding exactly `undefined` supplying no value, so a stored Note is kept (R3.6a) and a new item has none. Any other column imports as metadata, listed in E43's added columns. Its two L headers raise no E41 notice (F73, F81, F83). | ⌛️ Ready for Alignment | |
| R6.3 | Choosing or creating the target follows R2.1. Creating one pre-fills its name from the Custom Collection Name of the first record left after R6.4's exclusions, the user free to change it, and sets its scan mode to the file's, not editable in that creation and labelled as set by the export (R6.4); an existing target ignores that name (F76, F83). | ⌛️ Ready for Alignment | |
| R6.4 | When the file is read, §1–§3's file-level exclusions (E7, E9, E10) and R6.7's run first; then every remaining record's Measurement Mode is the same, or E46's mixed-modes variant refuses the file, listing the colours under each mode. E11 applies once a target is chosen and never reopens this check or R6.11's. A new target takes that mode as its chosen scan mode ([Capture R1.10](../capture-mode/prd-capture-mode.md#1-collections)); an existing target must already use it, or hold no reading — current or in history, QC records aside — and then adopt it, the adoption written by the commit and undone with it (R3.2) and named in E43; otherwise E46 refuses the import before mapping, naming both modes, and again at commit (R3.8k) (F76). | ⌛️ Ready for Alignment | |
| R6.5 | Each eligible record's reading is stored as [Data Foundation R2.3j](../data-foundation/prd-data-foundation.md#2-canonical-value-and-version-history) states: its reflectances as the finite numbers given, neither clamped nor rounded, values above 1.0 or below 0 included; its measurement mode; the file's illuminant and observer, as their text or absent where blank, as the reference the Toolkit's own values used, never a derived set's reference; Date Saved, read as an instant, as its measurement time to the millisecond, and the commit as its record time; its samples, averaging basis, agreement verdict and spread not recorded; an acquiring-device snapshot of the imported kind naming the Nix Device model — absent where the cell is blank — serial and firmware absent, unknown ([Device R1.21](../device-management/prd-device-management.md#1-device-pairing)); no raw payload, one never supplied; and every derived value worked out from its spectrum under the collection's reference at the current derivation version, the gamut-clipped flag included (F70, F77, F78). | ⌛️ Ready for Alignment | |
| R6.6 | Each record's Lab is worked out from its spectrum under the file's own illuminant and observer and compared with the file's first L, a and b. A record whose ΔE2000 exceeds [Data Foundation DERIVATION_TOLERANCE](../data-foundation/prd-data-foundation.md#open-questions) is listed in E43 as differing; one whose file L, a and b are not three readable numbers, or whose illuminant and observer are not a pair the app offers ([Capture R1.5 and OQ 21](../capture-mode/prd-capture-mode.md#1-collections)), is listed as not checked, never as matching; each still imports with the worked-out values (F81, F90). | ⌛️ Ready for Alignment | |
| R6.7 | A record whose Date Saved, Measurement Mode or any reflectance cannot be read is excluded and listed with the failing column (E47); the rest continue through R3.8c. E47 names every failing column of a record. A time is an RFC 3339 date-time with `Z` or a ±hh:mm offset, fractional seconds optional and truncated to the millisecond; a mode is exactly M0, M1 or M2; a number is an optional sign, digits, an optional `.` fraction and an optional `e` or `E` exponent with an optional sign, and finite — `NaN`, `Infinity`, an empty cell and a comma decimal cannot be read (F73). | ⌛️ Ready for Alignment | |
| R6.8 | A Toolkit record's reading lands as R6.8a–g state: R6.8f first, then R6.8b for a match with no readable current value, then by the current reading's snapshot kind — a restore carrying its source's; its metadata follows R3.5/R3.6, E14 showing its Toolkit variant (F70, F74, F82, F86, F87, F88). | ⌛️ Ready for Alignment | |
| R6.9 | E43's Toolkit variant names the export first and gives its reading counts, which partition the eligible records: readings becoming current (R6.8a, R6.8b, R6.8g and R6.8d's later), readings kept in history (R6.8c and R6.8d's earlier or same-dated) and readings already held (R6.8f). It also names the file's measurement mode, a target mode change (R6.4), each set-aside item it captures (R6.8b), R6.6's differing and not-checked records, and each same-dated record R6.8d keeps. Repeating a committed export with no intervening edits changes nothing (R3.3, F82). | ⌛️ Ready for Alignment | |
| R6.10 | Tests exercise R6.1–R6.11 and each R6.8 outcome on synthetic Toolkit exports — every collection name, code, name, note, date and value invented, none taken from a real export — built to UJ 3's stated format, the file's own values checked in from an independent implementation of the CIE 15 tabulation the build's derivation uses — its weighting table and its handling of the 400–700 nm range, both named in the engineering plan — never the app's code or derivation, save R6.6's offset cases, which set the file's Lab from the build's. Once [Data Foundation R7.5](../data-foundation/prd-data-foundation.md#7-verifiability)'s reference is named, it governs: Fixture T's values are regenerated to agree with it, never the build loosened. Every checked-in fixture or golden holding an imported reading derives from one; another PRD's case may instead declare an imported reading directly — an imported-kind snapshot and an invented spectrum, its derived values declared — so only this PRD's cases and Data Foundation R7.7o with its journey DJ6 run the importer. Neither a real export nor any data value from one enters the repository (F71). | ⌛️ Ready for Alignment | |
| R6.11 | Every record left after R6.4's exclusions has the same Custom Collection Name under R2.3, or E49 refuses the file, checked after R6.4's mixed-mode check, each Toolkit collection being exported and imported on its own (F89). | ⌛️ Ready for Alignment | |

**Toolkit commit outcomes — R6.8**

| ID | Source disposition | Effect at commit |
| :--- | :--- | :--- |
| R6.8f | One match that already holds the same reading, current or in history, a quarantined reading never counting | Adds no reading and changes no reading or state, counted as already held: a Flag, a restore or a set-aside decision stands; metadata follows R3.5/R3.6 (F82). |
| R6.8a | Eligible, no matching item | Create a captured item whose canonical value is the record's reading; append after existing rows, in source order. |
| R6.8b | One match with no current value — pending, set aside, or its current reading quarantined, whatever its kind | The reading becomes its canonical value, reason initial; the item is captured and keeps its queue position, E43 listing each set-aside item, an unreadable one included (F88). |
| R6.8c | One match whose current reading is of the live kind | The scan stays current; the reading is kept in version history, not current, reason initial (F74, F85). |
| R6.8g | One match whose current reading is of the simulated kind | The reading becomes current, reason re-measurement; the simulated reading stays in history (F86). |
| R6.8d | One match whose current reading is of the imported kind | A later Date Saved becomes current, reason re-measurement, the older reading going to history; an earlier one, or one with the same Date Saved and a different spectrum, is kept in history, not current, reason initial, E43 listing the latter (F82, F85, F87). |
| R6.8e | Excluded record or absent item | As R3.3c–d. |

## Success Metrics

| ID | Metric | Definition | Candidate target | Method | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| M1 | Import success on real exports | Start: file picked; end: committed import; share of Numbers, Excel and Sheets corpus files imported without editing the source file first. | ≥ 90%, proposal | Corpus walkthroughs with n, settings, failures and exclusions recorded; feeds OQ 1. | 🤝 Aligned |
| M2 | Toolkit exports imported cleanly | Start: a real Toolkit export picked; end: committed import; share imported with no record excluded by E7, E9, E10 or E47, no E5, E46 mixed-modes or E49 refusal and no R6.6 differing record; not-checked records are tallied apart. | ≥ 90%, proposal | The owner's own exports across devices, modes, illuminants and app versions, recording only format facts and tallies by state summed over two or more files — from a single file, only format facts and whether it imported cleanly — never a collection name, code, name, note, date, measured value, file name or per-file count (F71). | ⌛️ Ready for Alignment |

[Capture M8](../capture-mode/prd-capture-mode.md#success-metrics) measures import commit to first captured row.

## Open Questions

| # | Question | Interim build contract | Closer / required evidence | Feeds | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | CSV detection | R1.5’s lists/defaults are provisional; R1.6’s one-column guard and visible preview settings remain until detection closes. | Dogfood corpus of real Numbers, Excel and Sheets exports; record expected decoded fields, selected settings and failures, then publish deterministic detection/fallback rules and corpus results. | R1.2, R1.5, R1.6, M1 | open |
| 2 | Named mapping templates | Automatic remembered mapping remains required; no named-template UI until decided. | Owner decides whether named templates belong in v1, informed by repeated mapping corrections from OQ 1's corpus/dogfood. | R1.2 | open |
| 3 | Toolkit format variance | R6.1–R6.7 and UJ 3's format, from one studied export: its delimiter, quoting, number and time forms, 400–700 nm grid, modes and D50/2°. | Real Toolkit exports across locale, device, mode, illuminant and app version, recording only format facts and tallies by state, as M2 bounds them (F71): how a quote, delimiter or newline inside a name is written, whether a code can be blank or repeat, whether another export writes other number forms, whether `undefined` appears in a column other than Note, whether a serial or firmware column appears, whether a saved colour can move between Toolkit collections, which text encoding and byte-order mark each export uses. Revise R6.1–R6.7, R6.11, E5, E43, E46, E49, the Data Export PRD's E1 and UJ 3 from them. | R6.1–R6.7, M2 | open |

OQ 2 was separated from OQ 1 in the agent-build amendment and OQ 3 added by the Nix Toolkit amendment; none is answered. [Results](prd-inventory-import-oq-results.md) require a matching `## OQ <id>` section before status changes; Capture OQ 13 retains ownership of the import performance constants.
