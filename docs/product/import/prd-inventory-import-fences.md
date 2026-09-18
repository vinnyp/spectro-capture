# Inventory Import PRD — fences

Owner decisions for `prd-inventory-import.md`. Existing owner decisions remain binding except where the later individual decisions F51–F64 explicitly amend them; F50 records structural authorization.

F4 and F11 were copied under F49; their canonical text and original dates remain in the capture fence file. Import-local decisions start at F50; IDs are scoped to their document.

F49 itself — the split that made this document, and its clarification (1) — stays in the [capture fence file](../capture-mode/prd-capture-mode-fences.md) and is cited from here rather than restated. So do F24, the priority split, and F45, the two-sentence rule, both of which bind every row in this PRD.

## Fences

### F4 — Import target is chosen in the app (2026-09-06)

**Decision:** The target collection is picked or created in the app before columns are mapped. Collection name (and, per F1, library name) is not a CSV mapping target in v1. One CSV feeds one collection. Import ends at "collection ready to capture"; starting the session is a separate step. The bootstrap's UJ2 / UJ2.1 / UJ2.2 shape becomes UJ2 (new collection) plus UJ2.1 (idempotent re-import into an existing collection), with UJ2.2 as the shared column-mapping journey.

**Why:** A CSV is normally one swatch book. A collection-name column forces every file to carry a constant column and implies a multi-collection file that is not the primary case. Import-today-scan-tomorrow requires the queue to survive between import and session.

### F11 — Swatch Code normalisation (2026-09-06, round 1, R1-F9)

**Decision:** Wherever a Swatch Code or a collection name is compared (uniqueness, re-import match, find, ad-hoc duplicate), the comparison trims leading and trailing whitespace, collapses internal whitespace runs to one space, and is case-insensitive. The display value is preserved as entered. The normative rule now lives in Inventory Import R2.3 (moved under F49); all consumers cite it.

**Why:** A re-export from Numbers or an Excel autocorrect must not double the queue on an idempotent re-import. The owner chose the forgiving rule over the research default (case-sensitive) because spreadsheet drift is the common case for this persona.

### F50 — Agent-build amendment (2026-09-17)

**Authorization:** The owner approved the eight-item Inventory Import audit proposal in this session (“proceed with your recommended changes”). The first revision included agent-selected behavior; the review identified choices needing individual ratification, now recorded in F51–F61 from the [owner’s 2026-09-17 decisions](https://github.com/vinnyp/spectro-capture/pull/16#issuecomment-5723537817).

**Decision:** Keep stable IDs and the PRD and four companions; move split history here, state v1/P0 once, retain implementation status, and express journeys as acceptance tables. Requirement rows remain normative; no architecture, module layout, or completed dogfood evidence is claimed. F51–F61 supersede the first revision’s behavioral proposals, including exact-name column reuse, blocking duplicate headers, and the Unicode data-version pin.

**Why:** Builders need deterministic requirements and testable outcomes; repeated narrative and process history obscure that contract.

### F51 — Blank-to-blank is unchanged (2026-09-17)

**Decision:** A supplied blank over a stored blank counts as unchanged; this replaces the locked R3.6 wording.

**Why:** A no-op must not inflate the preview’s updated count.

### F52 — Preview confirms the target (2026-09-17)

**Decision:** Selecting an existing collection adds no confirmation dialog; the preview names the target collection and existing item count as a first-class line.

**Why:** The final review identifies the destination where the user commits, without an extra dialog.

### F53 — Header drift reuses the stored column (2026-09-17)

**Decision:** A passthrough header equal under R2.3 reuses the stored column, keeping its first-seen spelling and position; append only unmatched columns.

**Why:** Spreadsheet case and spacing drift must not multiply stored columns.

### F54 — Keep preserves conflicts only (2026-09-17)

**Decision:** A captured row choosing “Keep what I have” retains previously present fields, including blanks, but receives incoming values for previously absent fields. Column addition alone never counts a row as updated; list added columns separately.

**Why:** Keep protects existing choices while allowing new metadata to arrive; row counts must not conflate a column addition with an existing-value change.

### F55 — Duplicate headers are a notice (2026-09-17)

**Decision:** Disambiguate later named-header collisions under R2.3 with the same suffix mechanism used for blank headers; show the results in E12/E41 with “Continue with the listed names” and “Pick the file again”. Keep literal generated-name templates beside the copy.

**Why:** The user can import all columns without editing a spreadsheet first, while seeing exactly which names will be stored.

### F56 — Full default folding without locale tailoring (2026-09-17)

**Decision:** Retain canonical caseless matching with full default case folding and no locale tailoring: Straße equals STRASSE, while İ differs from i and ı differs from I; the nine-pair journey table specifies the observable cases.

**Why:** One locale-independent rule keeps import, uniqueness, find and duplicate checks consistent across machines.

### F57 — Comparison data version belongs to ADR-0003 (2026-09-17)

**Decision:** Remove the Unicode version pin from R2.3; ADR-0003 chooses comparison data/version governance, and a table upgrade requires re-checking existing identifiers. Add this to the decision queue, including implementation evidence against the macOS floor when ADR-0006 selects it.

**Why:** Product behavior must stay stable without pretending the PRD has selected an unverified dependency or OS floor.

### F58 — Guard single-column reads (2026-09-17)

**Decision:** Always show encoding and delimiter in preview; require explicit confirmation and offer the delimiter control for a one-column parse before mapping is saved or reused. The four encodings and three delimiters are provisional under OQ 1, the guard remains until that question closes, and E5 offers a UTF-8 re-save path when all encodings fail.

**Why:** A valid one-column parse can still be the wrong interpretation of a spreadsheet export; expose that ambiguity before remembering the mapping.

### F59 — Mirror storage obligations (2026-09-17)

**Decision:** Decoded values stay directly queryable as a Data Foundation obligation; mirror field/column preservation and re-import measurement preservation into its inbound table, with Rows columns on both sides. Record the cross-document amendment in the post-lock list.

**Why:** Agents implementing the store must receive the same preservation contract as agents implementing import.

### F60 — Changed source resets choices (2026-09-17)

**Decision:** On E13 re-read, reset overwrite choices and require a new preview, revalidating mapping first.

**Why:** Earlier overwrite approvals describe the old file and cannot authorize changed input.

### F61 — Accept remaining flow and open-question changes (2026-09-17)

**Decision:** Keep OQ 1 (detection) separate from OQ 2 (named templates); offer Cancel on every import state and use “Continue without them” at E7/E9/E10. All-unchanged imports may commit, all-excluded imports may not; source record numbering includes headers/preambles, starts at data record 1 for headerless input, and counts a quoted multiline record once.

**Why:** Open questions need separate evidence and owners; action labels must distinguish continuing from committing, and source numbering must remain usable when locating an issue.

### F62 — Preview absent-field fills separately (2026-09-17)

**Decision:** A matched row gaining a value in a field it never had remains unchanged in the row counts unless a previously present value changes, whether the column is new or already exists. E43 names the number of rows gaining details on a first-class line; split acceptance cases assert counts, the line, and stored values for both column cases.

**Why:** The preview must disclose these writes without changing F54’s separate treatment of row updates and column additions. Source: [round-2 owner decisions](https://github.com/vinnyp/spectro-capture/pull/16#issuecomment-5723905420).

### F63 — Preview follows shared zero-count copy rules (2026-09-17)

**Decision:** E43 omits zero-count sentences and empty list lines rather than displaying zero or “None”; Capture §12 owns the rule. Extend its placeholder index with Import’s preview and generated-name tokens, including the filled-row count, and record that editorial extension beside Capture F11.

**Why:** Import and its siblings must render counts consistently from one copy contract. Source: [round-2 owner decisions](https://github.com/vinnyp/spectro-capture/pull/16#issuecomment-5723905420).

### F64 — Separately created target persists (2026-09-17)

**Decision:** A collection created through Capture §1 persists empty when its import is canceled or fails under R3.2’s storage guarantee; creation is a separate action. R3.7 owns this lifecycle, with import eligibility separated into R3.9.

**Why:** Canceling import does not undo a separately completed collection creation. Accepted explicitly in the [round-2 owner decisions](https://github.com/vinnyp/spectro-capture/pull/16#issuecomment-5723905420).

## Fence → row map

Which rows in [`prd-inventory-import.md`](prd-inventory-import.md) carry each fence. Where a row names a fence it is for provenance only and no row re-argues one; this map is the link.

- **F4** the import target is chosen in the app — [R2.1](prd-inventory-import.md#2-target-mapping-and-the-matching-rule). **F11** Swatch Code normalisation — [R2.3](prd-inventory-import.md#2-target-mapping-and-the-matching-rule), [R2.4](prd-inventory-import.md#2-target-mapping-and-the-matching-rule). F11 also carries the capture PRD's R1.2, R6.1, and R9.2, which cite [R2.3](prd-inventory-import.md#2-target-mapping-and-the-matching-rule) across.
- In the [capture fence file](../capture-mode/prd-capture-mode-fences.md): **F24** the priority split — the [Legend](prd-inventory-import.md#legend). **F45** every requirement row is at most two sentences — every row in [§1](prd-inventory-import.md#1-reading-the-file) through [§4](prd-inventory-import.md#4-demo-device-and-verifiability). **F49** the split — this document, its four companions, and the historical ID map below.
- **F50** structural amendment — Legend, Traceability, acceptance-table shape and stable IDs. New base requirement IDs: R1.5, R1.6, R2.5, R2.6, R3.7, R3.8, R3.9; new copy IDs: E41–E45; existing IDs retained.
- **F51** blank-to-blank is unchanged — R3.6b/g.
- **F52** preview confirms the target — R2.1, R3.1; E43.
- **F53** header drift reuses the stored column — R2.2, R2.6.
- **F54** keep preserves conflicts only — R3.5, R3.6d–g; E14, E43.
- **F55** duplicate headers are a notice — R2.5; E12, E41.
- **F56** full default folding without locale tailoring — R2.3; UJ 2.2.
- **F57** comparison data version belongs to adr-0003 — R2.3; build dependencies; ADR-0003 queue.
- **F58** guard single-column reads — R1.2, R1.5, R1.6; E5, E45; OQ 1.
- **F59** mirror storage obligations — R2.2, R2.5, R2.6, R3.3; inherited obligations; DF R1.2/R2.3.
- **F60** changed source resets choices — R3.1, R3.8f; E13.
- **F61** accept remaining flow and open-question changes — R1.4, R3.8, R3.9; E7/E9/E10/E42; OQ 1/2.
- **F62** absent-field fills — R3.1, R3.6g/j, R3.9; E43; UJ 2.1.
- **F63** zero-count copy — R3.1; E43; Capture §12 placeholder index.
- **F64** target lifecycle — R3.7, R3.8j; UJ 2.

## Historical ID map

**Where these rows came from.** The rows in this map moved from the capture PRD under fence F49; the left-hand IDs are retired there and never reused. The copy states E4–E14, E38, and E40 moved with their numbers unchanged, and so did journeys UJ 2, UJ 2.1, and UJ 2.2.

| In the capture PRD | Here |
| :--- | :--- |
| R2.1 | [R1.1](prd-inventory-import.md#1-reading-the-file) |
| R2.2 | [R1.2](prd-inventory-import.md#1-reading-the-file) |
| R2.3 | [R1.3](prd-inventory-import.md#1-reading-the-file) |
| R2.4 | [R1.4](prd-inventory-import.md#1-reading-the-file) |
| R2.5 | [R2.1](prd-inventory-import.md#2-target-mapping-and-the-matching-rule) |
| R2.6 | [R2.2](prd-inventory-import.md#2-target-mapping-and-the-matching-rule) |
| R2.7 | [R2.3](prd-inventory-import.md#2-target-mapping-and-the-matching-rule) |
| R2.8 | [R2.4](prd-inventory-import.md#2-target-mapping-and-the-matching-rule) |
| R2.9 | [R3.1](prd-inventory-import.md#3-preview-and-commit) |
| R2.10 | [R3.2](prd-inventory-import.md#3-preview-and-commit) |
| R2.11 | [R3.3](prd-inventory-import.md#3-preview-and-commit) |
| R2.12 | [R3.4](prd-inventory-import.md#3-preview-and-commit) |
| R2.13 | [R3.5](prd-inventory-import.md#3-preview-and-commit) |
| R2.14 | [R3.6](prd-inventory-import.md#3-preview-and-commit) |
| R11.15h | [R4.1](prd-inventory-import.md#4-demo-device-and-verifiability) |
| M7 | [M1](prd-inventory-import.md#success-metrics) |
| OQ 12 | [OQ 1](prd-inventory-import.md#open-questions) |

[R4.2](prd-inventory-import.md#4-demo-device-and-verifiability) is not in the map. It carries over these states the obligation the [capture PRD's R11.12](../capture-mode/prd-capture-mode.md#11-demo-device-and-verifiability) held before the split, and that row stays live there.
