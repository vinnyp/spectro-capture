# Data Export PRD — owner decisions (fences)

Owner decisions on this PRD. A fence is settled: reviewers do not re-litigate it, and a row that names one does so for provenance only. Owner-locked rows: none yet. Review log: `../../agent-reviews/2026-09-09-prd-data-foundation-peer-reviews.md` — this PRD and the [Data Foundation PRD](../data-foundation/prd-data-foundation.md) share one log and one fresh-lens ledger, so the lens set is not re-run for the split and round 3 onward reads both documents.

## Phase 0 — research inventory (2026-09-09)

Inherited whole from the [Data Foundation PRD's Phase 0](../data-foundation/prd-data-foundation-fences.md#phase-0--research-inventory-2026-09-09), because these rows were ingested and reviewed there: every brief under `docs/briefs/` on `main` at `dd43c13`; the [browsing-a-collection-at-scale research results v2](../../briefs/browsing-a-collection-at-scale-research-results-v2.md) (supersedes the first pass on any point where they conflict — §8 on the gamut flag having no interoperable name is the one this document rests on); the [Nix universal SDK audit](../../briefs/nix-universal-sdk-audit-findings.md) (§2, what the raw payload round-trips and which spaces the toolkit supplies); and the X-Rite SDK and open-source prior-art findings brought in the same way (CxF and export shapes, which set [OQ 3](prd-data-export.md#open-questions)'s frame). The three locked PRDs and their obligations tables. Deferred by the owner: the Spectro 2 software competitive brief (positioning, not data). No project-supplied prior-art query command.

### F1 — Data Export is the CSV contract, split out of Data Foundation (2026-09-09)

**Decision:** This PRD states the export contract and nothing else: what a canonical and a history export contain and which items get a row, the column names and the reserved prefix, the dialect, the wavelength column set and what bumps the export format version, the failures an export can end in, and what a test asserts against a checked-in golden. The file those exports are read out of, and every guarantee it keeps, stays in the [Data Foundation PRD](../data-foundation/prd-data-foundation.md). Word budget for the PRD body: 4,000 words. Shape: the sibling five-file shape — PRD, journeys, copy, OQ results, fences; no author line; row IDs `R<section>.<n>`, `E<n>`, `M<n>`; every requirement row at most two sentences.

**Why:** Transcribed from [the Data Foundation PRD's F30](../data-foundation/prd-data-foundation-fences.md), 2026-09-09. The export is a consumer contract with its own audience (U6's downstream tools) and its own fresh-lens set; Data Foundation is the file's promise to its owner. That document's round-2 fix pass landed at 7,690 words with nothing left to cut but rules, and a document that cannot fit because it has too many rows is two PRDs (writing-prds, Phase 2). Moved IDs are retired at the source naming their destination and recorded in this document's [Traceability](prd-data-export.md#traceability) naming their origin; the decisions the moved rows carry are transcribed below and kept there as history.

### F2 — Export is canonical-only by default with version history as a v1 option (2026-09-09)

**Decision:** The default CSV export is one row per item carrying its canonical value; an explicit option exports every version. The canonical export is P0; the history option is P1. Closes [OQ 2](prd-data-export.md#open-questions) and settles the export rows' priority.

**Provenance:** transcribed from [the Data Foundation PRD's F5](../data-foundation/prd-data-foundation-fences.md), 2026-09-09 (its OQ 9 is this document's OQ 2).

**Clarification (1), 2026-09-09:** [R4.4](prd-data-export.md#4-verifiability) — that document's R7.6, the test row that asserts the export's column set — is P0 with the rows it verifies (process rule 3).

### F3 — The CSV carries the vendor's raw payload column (2026-09-09)

**Decision:** One opaque column per row, documented as the vendor's round-trip string, so the export carries the file's fidelity promise and [M1](prd-data-export.md#success-metrics) reads as written; a reading with no payload leaves it empty and marked. **Amended (round 2, FX2-4; round 3, FX3-12):** the column is empty where none exists; no separate mark.

**Provenance:** transcribed from [the Data Foundation PRD's F18](../data-foundation/prd-data-foundation-fences.md), 2026-09-09 (its M3 is this document's M1). Amended by [F8](#f8--the-csv-carries-one-payload-column-per-sample-slot-2026-09-09): one column per sample slot.

### F4 — The gamut-clipped export column is `sc_sRGB_gamut_clipped` (2026-09-09)

**Decision:** Closes [OQ 1](prd-data-export.md#open-questions) by owner decision; revisited only if ISO 17972-4's schema becomes readable.

**Provenance:** transcribed from [the Data Foundation PRD's F20](../data-foundation/prd-data-foundation-fences.md), 2026-09-09 (its OQ 8 is this document's OQ 1).

### F5 — Export column names and the empty derived field (2026-09-09)

**Decision:** (a) Every column the app emits in the CSV carries the prefix `sc_`; an imported column that would collide is emitted as `import_<name>` and the export surface says so ([R2.4](prd-data-export.md#2-columns-names-dialect-and-the-version)). (b) An absent derived value exports as an empty field, never a zero ([R2.1](prd-data-export.md#2-columns-names-dialect-and-the-version)).

**Provenance:** transcribed from [the Data Foundation PRD's F22](../data-foundation/prd-data-foundation-fences.md), 2026-09-09 — its (a) whole, and the export half of its (c). That fence's second-file rule and its COMPATIBILITY_FLOOR half are about the file, not the export, and stay there.

### F6 — The `sc_` prefix applies to every app column (2026-09-09)

**Decision:** [F5](#f5--export-column-names-and-the-empty-derived-field-2026-09-09)(a) subsumes [F4](#f4--the-gamut-clipped-export-column-is-sc_srgb_gamut_clipped-2026-09-09) and the device PRD's `simulated` column: the export columns are `sc_sRGB_gamut_clipped`, `sc_sRGB_source_space`, `sc_sRGB_rendering_intent`, `sc_simulated`, and `sc_nm_<wavelength>` for the spectral columns. F4's answer and [OQ 1's results section](prd-data-export-oq-results.md#oq-1--what-the-gamut-clipped-export-column-is-called) are restated under the prefix; [the device PRD's](../device-management/prd-device-management.md#6-mock-device-layer) "emits a `simulated` column" obligation is read as `sc_simulated`, carried as a post-lock amendment line under this document's ["what this PRD imposes on others"](prd-data-export.md#inherited-obligations). No exemption list exists.

**Why:** One rule the golden header can be written against; a closed exemption list would re-open the collision F5 exists to close. Transcribed from [the Data Foundation PRD's F23](../data-foundation/prd-data-foundation-fences.md), 2026-09-09.

### F7 — The export format version fixes the wavelength column set (2026-09-09)

**Decision:** The wavelength column set — start, end, interval — is part of the export format version; v1's set is the v1 instrument family's reported grid, carried as [OQ 4](prd-data-export.md#open-questions) with a per-SDK-docs candidate closed on hardware. A reading whose wavelengths fall outside the set exports those columns empty and marked; any change to the set bumps the export format version ([R2.5](prd-data-export.md#2-columns-names-dialect-and-the-version)). **Amended (round 3, FX3-12):** an out-of-grid column is empty, with no separate mark, and the branch cannot arise in v1 (R2.3).

**Why:** [R2.3](prd-data-export.md#2-columns-names-dialect-and-the-version)'s "column order is fixed" and [R4.1](prd-data-export.md#4-verifiability)'s golden are undefined without a fixed set; a header that follows the data is unusable to a consumer allocating columns before reading. Transcribed from [the Data Foundation PRD's F24](../data-foundation/prd-data-foundation-fences.md), 2026-09-09 (its OQ 16 is this document's OQ 4).

### F8 — The CSV carries one payload column per sample slot (2026-09-09)

**Decision:** The canonical export carries `sc_sample_1_payload` … `sc_sample_5_payload`, one column per sample slot up to the capture PRD's maximum of five, empty where a slot is unused or a sample has no payload. [F3](#f3--the-csv-carries-the-vendors-raw-payload-column-2026-09-09)'s "one opaque column" becomes up to five; the header stays fixed.

**Why:** A reading of N samples has N payloads; a fixed slot set keeps [R4.1](prd-data-export.md#4-verifiability)'s golden well-defined and gives [M1](prd-data-export.md#success-metrics) every sample it reconstructs from. Transcribed from [the Data Foundation PRD's F26](../data-foundation/prd-data-foundation-fences.md), 2026-09-09, which amends its F18 the same way.

### F9 — The canonical export emits the collection's chosen condition (2026-09-09)

**Decision:** The canonical export emits one row per item on the chosen condition — the set for the collection's chosen scan mode ([the capture PRD's R1.10](../capture-mode/prd-capture-mode.md#1-collections)), which [the Data Foundation PRD's R3.1](../data-foundation/prd-data-foundation.md#3-derived-values-and-gamut-honesty) keeps current and always present — the condition column saying which; the header stays fixed. **Amended (round 3, FX3-6; round 4, owner):** the chosen condition's set is absent where the reading carries no measurement in it (that PRD's R3.1 and R3.5); a non-chosen condition's set kept under that PRD's F31 is readable in the file and not exported, in either export kind.

**Why:** The capture PRD keeps every mode a reading arrives with and works colour values out from the chosen mode; one row per item ([F2](#f2--export-is-canonical-only-by-default-with-version-history-as-a-v1-option-2026-09-09)) and a fixed header ([F7](#f7--the-export-format-version-fixes-the-wavelength-column-set-2026-09-09)) survive only if the export names one condition per row. Transcribed from the export half of [the Data Foundation PRD's F28](../data-foundation/prd-data-foundation-fences.md), 2026-09-09; that fence's rule that derived sets are keyed per reading and condition is about the file and stays there.

### F10 — Every item gets an export row (2026-09-09)

**Decision:** Every item in the collection gets a row in the canonical export; an item with no canonical value carries its identity and import columns with its colour, spectral, and payload columns empty, and a column states whether it is never-scanned or quarantined. Both cases join [M1](prd-data-export.md#success-metrics)'s population.

**Why:** A half-scanned collection is the ordinary state between sittings, and the export must reconcile against the spreadsheet the inventory came from. Transcribed from [the Data Foundation PRD's F29](../data-foundation/prd-data-foundation-fences.md), 2026-09-09.

### F11 — The canonical export carries when each current reading was measured (2026-09-10, round 5)

**Decision:** Every canonical row carries the time its current reading was measured, under an `sc_` name, the export preview naming it. An appended column, so no export format version bump.

**Why:** A Data consumer cannot otherwise tell from the file when a swatch was scanned until the history export lands; the serial is already disclosed at the preview, so the added linkability is small and the provenance is what U6 expects. Taken over the privacy lens's reading of the omission as minimization — the owner's call.

### F12 — A change in the form a value is written in bumps the export format version (2026-09-15, round 7)

**Decision:** [R2.5](prd-data-export.md#2-columns-names-dialect-and-the-version)'s bump list includes an app column's values changing the form they are written in — precision, a boolean's spelling, a time's shape; [R4.1](prd-data-export.md#4-verifiability)'s in-place golden refresh covers an appended column and a DERIVATION_VERSION bump only, a form change cutting a new golden under the new version.

**Why:** R2.2 made value form part of the contract per released version (round 6); a consumer comparing two releases' files under one declared version must see one form, or the version says nothing a name-keyed parser can rely on. Taken over the alternative — form outside the version, recorded per release in the help docs — which the standards and architecture lenses both read as a silent change at the second release. The owner's call.

### F13 — A passthrough column's value is emitted as stored (2026-09-15, round 7)

**Decision:** [R2.2](prd-data-export.md#2-columns-names-dialect-and-the-version)'s value-form rule governs app columns only; a column an import brought in is emitted with its value exactly as stored, [R2.4](prd-data-export.md#2-columns-names-dialect-and-the-version) saying so.

**Why:** U6's "nothing is stranded" and the import PRD's "exactly as entered": a user's own column never changes shape in their own export — an imported `0007` or `1,234.5` comes back out as it went in. Taken over normalising every column to the dialect's forms, which the interface lens showed would alter a script-visible value the user supplied.

## Fence → row map

A row "carries" a fence when the fence's decision is what the row now states; this file, not the row, holds the rationale. Every entry below is the corresponding entry in [the Data Foundation PRD's map](../data-foundation/prd-data-foundation-fences.md#fence--row-map) rewritten into this document's IDs.

| Fence | Rows that carry it |
| :--- | :--- |
| F1 | Every row in the PRD body, plus its Background scope statement, its shape, its 4,000-word budget, and its [Traceability](prd-data-export.md#traceability) origin table; no requirement row of its own. |
| F2 | [R1.1](prd-data-export.md#1-what-the-export-contains) (canonical default, P0), [R1.3](prd-data-export.md#1-what-the-export-contains) (history option, P1); the P0/P1 split in the [Legend](prd-data-export.md#legend), and the Pri cells of [R1.2](prd-data-export.md#1-what-the-export-contains), [R2.1](prd-data-export.md#2-columns-names-dialect-and-the-version), [R3.1](prd-data-export.md#3-when-an-export-cannot-finish), [R4.4](prd-data-export.md#4-verifiability); [OQ 2](prd-data-export.md#open-questions). |
| F3 | [R1.1](prd-data-export.md#1-what-the-export-contains)'s raw-payload column; [M1](prd-data-export.md#success-metrics)'s statistic. Amended by F8: one column per sample slot. |
| F4 | [R2.1](prd-data-export.md#2-columns-names-dialect-and-the-version); [OQ 1](prd-data-export.md#open-questions). |
| F5 | [R2.4](prd-data-export.md#2-columns-names-dialect-and-the-version) (a); [R2.1](prd-data-export.md#2-columns-names-dialect-and-the-version) (b). |
| F6 | [R1.2](prd-data-export.md#1-what-the-export-contains), [R2.1](prd-data-export.md#2-columns-names-dialect-and-the-version), [R2.3](prd-data-export.md#2-columns-names-dialect-and-the-version), [R2.4](prd-data-export.md#2-columns-names-dialect-and-the-version); [OQ 1's results section](prd-data-export-oq-results.md#oq-1--what-the-gamut-clipped-export-column-is-called); the Device Management export line both ways in [Inherited obligations](prd-data-export.md#inherited-obligations). |
| F7 | [R2.3](prd-data-export.md#2-columns-names-dialect-and-the-version), [R2.5](prd-data-export.md#2-columns-names-dialect-and-the-version), [R4.1](prd-data-export.md#4-verifiability), [R4.2](prd-data-export.md#4-verifiability); [OQ 4](prd-data-export.md#open-questions). |
| F8 | [R1.1](prd-data-export.md#1-what-the-export-contains), [R4.1](prd-data-export.md#4-verifiability), [R4.2](prd-data-export.md#4-verifiability); [M1](prd-data-export.md#success-metrics). |
| F9 | [R1.1](prd-data-export.md#1-what-the-export-contains), [R1.3](prd-data-export.md#1-what-the-export-contains), [R2.1](prd-data-export.md#2-columns-names-dialect-and-the-version), [R2.3](prd-data-export.md#2-columns-names-dialect-and-the-version); the Data Foundation line in [Inherited obligations](prd-data-export.md#inherited-obligations) (the chosen condition's set). |
| F10 | [R1.1](prd-data-export.md#1-what-the-export-contains), [R2.1](prd-data-export.md#2-columns-names-dialect-and-the-version); [M1](prd-data-export.md#success-metrics)'s population; [E1](prd-data-export-copy.md#error--state-copy). |
| F11 | [R1.1](prd-data-export.md#1-what-the-export-contains); [E1](prd-data-export-copy.md#error--state-copy); the inbound Data Foundation line (both time axes). |
| F12 | [R2.5](prd-data-export.md#2-columns-names-dialect-and-the-version)'s bump list; [R4.1](prd-data-export.md#4-verifiability)'s in-place refresh list; [R2.2](prd-data-export.md#2-columns-names-dialect-and-the-version)'s per-version help-docs sentence. |
| F13 | [R2.2](prd-data-export.md#2-columns-names-dialect-and-the-version) (app columns only); [R2.4](prd-data-export.md#2-columns-names-dialect-and-the-version) (emitted as stored). |

## Rejected findings

None yet. The findings rejected on these rows before the split are recorded in [the Data Foundation PRD's fence file](../data-foundation/prd-data-foundation-fences.md#rejected-findings); neither of the two standing there touches an export row.
