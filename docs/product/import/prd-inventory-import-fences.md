# Inventory Import PRD — fences

Owner decisions for `prd-inventory-import.md`. Existing owner decisions remain binding; F50 records this agent-build amendment and the scope of its authorization.

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

**Authorization:** The owner approved the eight-item Inventory Import audit proposal in this session ("proceed with your recommended changes"). The following details were selected by the agent within that approved scope; they are new specifications, not claims about prior owner decisions or completed dogfood evidence.

**Decision:**

1. Keep stable IDs and the PRD and four companion files; move the split history here, state v1/P0 once, retain implementation status, and express journeys as acceptance tables. Requirement rows remain normative; no architecture or module layout is selected.
2. Only new items append pending; matched items retain state, order and measurements. Blank-to-blank is unchanged, supplied blank clears a value, absent column preserves it, and keep/overwrite choices determine the preview counts; imported column additions are shown separately.
3. Specify R2.3 using Unicode 17.0.0 White_Space and canonical caseless matching, including full default folding and NFD, without locale tailoring or compatibility normalization. A future change to comparison results requires explicit review of existing identifiers, not a silent library-driven change; F11's forgiving whitespace/case rule remains intact.
4. Reject equivalent named headers before mapping; generate collision-free positional names for blanks. Reuse identity mappings by normalized header name, while passthrough storage uses exact names so an import cannot silently rename an existing column; mapping-storage lifecycle and Collection Mode renames remain the existing post-lock work.
5. Creating the target is Capture's separate action: a canceled or failed import leaves that empty collection. All-excluded input cannot commit; all-unchanged input can finish; import actions never bypass preview, and session-blocked actions are observable and follow Capture's existing ending/resume paths.
6. R1.5 specifies a minimal read contract and preserves field text; detection stays open as OQ 1, and named templates move to OQ 2 with no named-template UI before the owner's decision. M1 still needs real export evidence; this amendment does not fabricate it.

**Why:** A builder needs deterministic outcomes and an explicit unresolved boundary. Repeated human-facing narrative and process history obscure that contract; leaving equality, column identity and commit eligibility implicit forces agents to invent different behaviors.

## Fence → row map

Which rows in [`prd-inventory-import.md`](prd-inventory-import.md) carry each fence. Where a row names a fence it is for provenance only and no row re-argues one; this map is the link.

- **F4** the import target is chosen in the app — [R2.1](prd-inventory-import.md#2-target-mapping-and-the-matching-rule). **F11** Swatch Code normalisation — [R2.3](prd-inventory-import.md#2-target-mapping-and-the-matching-rule), [R2.4](prd-inventory-import.md#2-target-mapping-and-the-matching-rule). F11 also carries the capture PRD's R1.2, R6.1, and R9.2, which cite [R2.3](prd-inventory-import.md#2-target-mapping-and-the-matching-rule) across.
- In the [capture fence file](../capture-mode/prd-capture-mode-fences.md): **F24** the priority split — the [Legend](prd-inventory-import.md#legend). **F45** every requirement row is at most two sentences — every row in [§1](prd-inventory-import.md#1-reading-the-file) through [§4](prd-inventory-import.md#4-demo-device-and-verifiability). **F49** the split — this document, its four companions, and the historical ID map below.
- **F50** agent-build amendment — R1.2–R1.5, R2.2–R2.5, R3.1–R3.8, R4.1–R4.2, OQ 1/2, acceptance journeys and copy transitions. New requirement IDs: R1.5, R2.5, R3.7, R3.8; new copy IDs: E41, E42; existing IDs are retained.

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
