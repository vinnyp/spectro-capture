# Inventory Import PRD — fences

Owner decisions for `prd-inventory-import.md`. Every entry is settled: it is carried into every review brief and editing dispatch, and is not re-litigated. Format per `operator-agents:writing-prds`.

Copied from the capture fence file under F49; the capture file remains the record of the decision date.

F49 itself — the split that made this document, and its clarification (1) — stays in the [capture fence file](../capture-mode/prd-capture-mode-fences.md) and is cited from here rather than restated. So do F24, the priority split, and F45, the two-sentence rule, both of which bind every row in this PRD.

## Fences

### F4 — Import target is chosen in the app (2026-09-06)

**Decision:** The target collection is picked or created in the app before columns are mapped. Collection name (and, per F1, library name) is not a CSV mapping target in v1. One CSV feeds one collection. Import ends at "collection ready to capture"; starting the session is a separate step. The bootstrap's UJ2 / UJ2.1 / UJ2.2 shape becomes UJ2 (new collection) plus UJ2.1 (idempotent re-import into an existing collection), with UJ2.2 as the shared column-mapping journey.

**Why:** A CSV is normally one swatch book. A collection-name column forces every file to carry a constant column and implies a multi-collection file that is not the primary case. Import-today-scan-tomorrow requires the queue to survive between import and session.

### F11 — Swatch Code normalisation (2026-09-06, round 1, R1-F9)

**Decision:** Wherever a Swatch Code or a collection name is compared (uniqueness, re-import match, find, ad-hoc duplicate), the comparison trims leading and trailing whitespace, collapses internal whitespace runs to one space, and is case-insensitive. The display value is preserved as entered. Stated once in UJ2.2 and cited everywhere else.

**Why:** A re-export from Numbers or an Excel autocorrect must not double the queue on an idempotent re-import. The owner chose the forgiving rule over the research default (case-sensitive) because spreadsheet drift is the common case for this persona.

## Fence → row map

Which rows in [`prd-inventory-import.md`](prd-inventory-import.md) carry each fence. Where a row names a fence it is for provenance only and no row re-argues one; this map is the link.

- **F4** the import target is chosen in the app — [R2.1](prd-inventory-import.md#2-target-mapping-and-the-matching-rule). **F11** Swatch Code normalisation — [R2.3](prd-inventory-import.md#2-target-mapping-and-the-matching-rule), [R2.4](prd-inventory-import.md#2-target-mapping-and-the-matching-rule). F11 also carries the capture PRD's R1.2, R6.1, and R9.2, which cite [R2.3](prd-inventory-import.md#2-target-mapping-and-the-matching-rule) across.
- In the [capture fence file](../capture-mode/prd-capture-mode-fences.md): **F24** the priority split — the Pri column throughout, and the [Legend](prd-inventory-import.md#legend). **F45** every requirement row is at most two sentences — every row in [§1](prd-inventory-import.md#1-reading-the-file) through [§4](prd-inventory-import.md#4-demo-device-and-verifiability). **F49** the split — this document, its four companions, and the ID map in [Traceability](prd-inventory-import.md#traceability).
