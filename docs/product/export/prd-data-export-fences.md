# Data Export PRD — owner decisions (fences)

Owner decisions on this PRD. A fence is settled: reviewers do not re-litigate it, and a row that names one does so for provenance only. Owner-locked rows: none yet. Review log: `../../agent-reviews/2026-09-09-prd-data-foundation-peer-reviews.md` — this PRD and the [Data Foundation PRD](../data-foundation/prd-data-foundation.md) share one log and one fresh-lens ledger, so the lens set is not re-run for the split and round 3 onward reads both documents.

## Phase 0 — research inventory (2026-09-09)

Inherited whole from the [Data Foundation PRD's Phase 0](../data-foundation/prd-data-foundation-fences.md#phase-0--research-inventory-2026-09-09), because these rows were ingested and reviewed there: every brief under `docs/briefs/` on `main` at `dd43c13`; the [browsing-a-collection-at-scale research results v2](../../briefs/browsing-a-collection-at-scale-research-results-v2.md) (supersedes the first pass on any point where they conflict — §8 on the gamut flag having no interoperable name is the one this document rests on); [browsing v2 §10](../../briefs/browsing-a-collection-at-scale-research-results-v2.md#10-empty-loading-and-error-states) (the inherited failure-class evidence for R3.1); the [Nix universal SDK audit](../../briefs/nix-universal-sdk-audit-findings.md) (§2, what the raw payload round-trips and which spaces the toolkit supplies); and the X-Rite SDK and open-source prior-art findings brought in the same way (CxF and export shapes, which set [OQ 3](prd-data-export.md#open-questions)'s frame). The three locked PRDs and their obligations tables. Deferred by the owner: the Spectro 2 software competitive brief (positioning, not data). No project-supplied prior-art query command.

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

**Clarification (2026-09-18, PR #18):** F10’s historical payload-blanking clause for quarantine is read under standing F14: available archives remain exportable; R1.1d/l control empty payload cells. [Owner decisions 2 and 4](https://github.com/vinnyp/spectro-capture/pull/18#issuecomment-5732380642) confirm this boundary and F21 specifies readable provenance.

### F11 — The canonical export carries when each current reading was measured (2026-09-10, round 5)

**Decision:** Every canonical row carries the time its current reading was measured, under an `sc_` name, the export preview naming it. An appended column, so no export format version bump.

**Why:** A Data consumer cannot otherwise tell from the file when a swatch was scanned until the history export lands; the serial is already disclosed at the preview, so the added linkability is small and the provenance is what U6 expects. Taken over the privacy lens's reading of the omission as minimization — the owner's call.

### F12 — A change in the form a value is written in bumps the export format version (2026-09-15, round 7)

**Decision:** [R2.5](prd-data-export.md#2-columns-names-dialect-and-the-version)'s bump list includes an app column's values changing the form they are written in — precision, a boolean's spelling, a time's shape; [R4.1](prd-data-export.md#4-verifiability)'s in-place golden refresh covers an appended column and a DERIVATION_VERSION bump only, a form change cutting a new golden under the new version.

**Why:** R2.2 made value form part of the contract per released version (round 6); a consumer comparing two releases' files under one declared version must see one form, or the version says nothing a name-keyed parser can rely on. Taken over the alternative — form outside the version, recorded per release in the help docs — which the standards and architecture lenses both read as a silent change at the second release. The owner's call.

### F13 — A passthrough column's value is emitted as stored (2026-09-15, round 7)

**Decision:** [R2.2](prd-data-export.md#2-columns-names-dialect-and-the-version)'s value-form rule governs app columns only; a column an import brought in is emitted with its value exactly as stored, [R2.4](prd-data-export.md#2-columns-names-dialect-and-the-version) saying so.

**Why:** U6's "nothing is stranded" and the import PRD's "exactly as entered": a user's own column never changes shape in their own export — an imported `0007` or `1,234.5` comes back out as the file holds it, the app reshaping nothing but its quoting on the way. Taken over normalising every column to the dialect's forms, which the interface lens showed would alter a script-visible value the user supplied.

### F14 — Archive damage and quarantine export semantics (2026-09-17, PR #17)

**Decision:** Transcribe the export half of DF F33 as settled by [DF F37](../data-foundation/prd-data-foundation-fences.md#f37--export-unavailable-archives-and-quarantined-readings-2026-09-17): canonical export runs when only an archive is damaged; unavailable sample payload cells are empty, with no new CSV column or state token. Any quarantined reading uses `quarantined` in the state column, including superseded history; DF OQ 21 remains open for a distinct archive mark and an Export R2.5 version bump.

**Why:** Intact canonical data remains exportable, and both builders and consumers need deterministic payload and state precedence. Source: [owner decision 2](https://github.com/vinnyp/spectro-capture/pull/17#issuecomment-5725041707).

### F15 — Agent-build contract and acceptance shape (2026-09-17)

**Decision:** The approved seven-item list permits decomposing dense requirements, replacing narration with acceptance scenarios, moving split history here and correcting stale obligations while preserving IDs, priorities and tracking. Detailed behavior is settled by F19–F29; retained retired goldens remain F12/R4.1’s pre-existing contract, not a new decision.

**Why:** Agents need observable rules at implementation time; the initial list approval did not decide every detail. [Owner clarification, 2026-09-18](https://github.com/vinnyp/spectro-capture/pull/18#issuecomment-5732380642) supplies the recorded provenance and requires the shared Legend wording under F29.

### F16 — Missing reading fields and quarantine rows (2026-09-17)

**Decision:** Audit item 2 authorizes an explicit missing-data matrix; F19 settles unknown simulation values and F21 settles canonical quarantine provenance. Payload availability follows standing F14/DF F37, restored under owner decision 2; quarantine does not suppress an available archive.

**Why:** An explicit matrix prevents guessed values; the blanket list approval did not authorize canonical/history asymmetry or payload suppression. [Owner decisions 1, 2 and 4](https://github.com/vinnyp/spectro-capture/pull/18#issuecomment-5732380642) resolve those details.

### F17 — Collision allocation follows stored collection order (2026-09-17)

**Decision:** Audit item 3 authorizes deterministic collision rules and examples; F22/F23 now settle order, consequences and comparisons. Under F22, this expressly supersedes R2.4’s prior “earlier in the import file’s order” clause with persisted collection-column order; R2.5 still governs released-format changes.

**Why:** The original wording has no single referent after repeated imports; the literal-`import_` outcome needs an explicit owner decision, not inferred approval. [Owner decisions 5–6](https://github.com/vinnyp/spectro-capture/pull/18#issuecomment-5732380642) supply it.

### F18 — Preview population and empty exports (2026-09-17)

**Decision:** Audit item 5 authorizes specifying preview variants; F27/F28 now settle empty output/copy and the disclosure set. F27 chooses a header-only export over disabling export at zero items.

**Why:** Builders need defined empty/single-item/history behavior; approval to clarify it was not approval of every chosen detail. [Owner decisions 10–11](https://github.com/vinnyp/spectro-capture/pull/18#issuecomment-5732380642) supply those details.

### F19 — Unknown simulation provenance (2026-09-18, PR #18)

**Decision:** `sc_simulated` is true/false from a known acquiring snapshot and empty without one; consumers treat empty as unknown. Amend Device R6.5 and its export obligation plus DF R7.7e to cover all three values.

**Why:** No reading or snapshot is not evidence of a live device. Source: [owner decision 1](https://github.com/vinnyp/spectro-capture/pull/18#issuecomment-5732380642).

### F20 — Empty collection belongs to the fixture inventory (2026-09-18, PR #18)

**Decision:** Add DF R7.7n for an empty collection, consumed by R4.1h’s header-only golden and R1.1r preview; name it in both obligation directions.

**Why:** The sole-inventory loop must actually exercise the empty case. Source: [owner decision 3](https://github.com/vinnyp/spectro-capture/pull/18#issuecomment-5732380642).

### F21 — Canonical quarantine preserves readable provenance (2026-09-18, PR #18)

**Decision:** Split never-scanned R1.1h from quarantined-current R1.1s; canonical quarantine keeps readable snapshot, `sc_simulated`, basis and measured-at without a sequence column, as history keeps them. Colour/spectral/qualifier cells are empty; payloads follow standing F14.

**Why:** Measurement damage does not erase readable provenance, and the two export kinds must agree. Source: [owner decision 4](https://github.com/vinnyp/spectro-capture/pull/18#issuecomment-5732380642).

### F22 — Persisted-order collision tie-break (2026-09-18, PR #18)

**Decision:** Persisted collection-column order under Import R2.5/R2.6 replaces “earlier in the import file’s order”; earlier allocations can rename a literal `import_` column even when its own original name was not reserved. Put the basis/examples in R2.4a and consequence in R2.4c; F17 explicitly supersedes the old clause.

**Why:** Repeated imports retain one stored order; the three examples now have ratified consequences. Source: [owner decision 5](https://github.com/vinnyp/spectro-capture/pull/18#issuecomment-5732380642).

### F23 — One comparator for both collision tests (2026-09-18, PR #18)

**Decision:** Use Import R2.3 for namespace-prefix and allocated-name comparisons; keep stored spelling with `import_` prepended in emitted headers. Extend DF R7.7a with case/whitespace variants.

**Why:** Byte-wise and canonical-caseless tests must not produce different headers. Source: [owner decision 6](https://github.com/vinnyp/spectro-capture/pull/18#issuecomment-5732380642).

### F24 — Exactly three mixed-case app names (2026-09-18, PR #18)

**Decision:** Only `sc_sRGB_gamut_clipped`, `sc_sRGB_source_space` and `sc_sRGB_rendering_intent` escape lower snake_case; every other sRGB column follows the ordinary rule.

**Why:** A wildcard would let first-golden authors freeze incompatible value-column spellings. Source: [owner decision 7](https://github.com/vinnyp/spectro-capture/pull/18#issuecomment-5732380642).

### F25 — Non-spectral qualifiers keep their original reference (2026-09-18, PR #18)

**Decision:** R2.1 excepts non-spectral rows: qualifiers carry their original reference; divergence from the row’s chosen-condition column is derivable, not a separate exported mark. R1.1k owns precedence: missing chosen-condition measurement remains absent; otherwise original-reference spaces/qualifiers persist.

**Why:** Do not fabricate a derivation or silently add a CSV mark. Source: [owner decision 8](https://github.com/vinnyp/spectro-capture/pull/18#issuecomment-5732380642).

### F26 — Payload-count branches have fixtures (2026-09-18, PR #18)

**Decision:** Extend DF R7.7i with a sample whose instrument never supplied a payload, beside unused slots and unavailable archives; mirror those cases in R4.2.

**Why:** Preview unavailable-archive counts must not include legitimate absent payloads. Source: [owner decision 9](https://github.com/vinnyp/spectro-capture/pull/18#issuecomment-5732380642).

### F27 — Empty collection remains exportable (2026-09-18, PR #18)

**Decision:** Export the selected kind’s header and no rows, with “This collection has no swatches. The export contains column names only.” Disabling export at zero items is the alternative not taken, also recorded in F18.

**Why:** A deterministic empty output and explicit preview are useful to downstream consumers. Source: [owner decision 10](https://github.com/vinnyp/spectro-capture/pull/18#issuecomment-5732380642).

### F28 — Preview disclosure set (2026-09-18, PR #18)

**Decision:** Keep R1.1o–r’s kind, item/row counts, chosen condition, five population counts, rename pairs and export-format/app versions; canonical quarantine counts affected items. Recompute for selected scope/kind before writing; omit redundant row counts when equal to item counts.

**Why:** The preview must describe the selected output, with explicit versions and accurate count units. Source: [owner decision 11](https://github.com/vinnyp/spectro-capture/pull/18#issuecomment-5732380642).

### F29 — Restore the shared Legend (2026-09-18, PR #18)

**Decision:** Restore the priority recommendation hedge and the six shared status-glyph definitions; lettered rule/copy IDs are added to Traceability without redefining those statuses.

**Why:** A document-shape audit does not redefine shared governance. Source: [owner decision 12](https://github.com/vinnyp/spectro-capture/pull/18#issuecomment-5732380642).

## Split-origin map

**Where these rows came from.** Every row, state, metric, question and journey below moved out of the [Data Foundation PRD](../data-foundation/prd-data-foundation.md) on 2026-09-09 under that document's fence F30, transcribed here as [F1](prd-data-export-fences.md#f1--data-export-is-the-csv-contract-split-out-of-data-foundation-2026-09-09). No rule changed in the move: only the IDs, the citations, and which section a row sits in. The left-hand IDs are retired there and never reused ([its Legend](../data-foundation/prd-data-foundation.md#legend)).

| In the Data Foundation PRD | Here |
| :--- | :--- |
| R4.1 | [R1.1](prd-data-export.md#1-what-the-export-contains) |
| R4.3 | [R1.2](prd-data-export.md#1-what-the-export-contains) |
| R4.4 | [R1.3](prd-data-export.md#1-what-the-export-contains) |
| R4.2 | [R2.1](prd-data-export.md#2-columns-names-dialect-and-the-version) |
| R4.6 | [R2.2](prd-data-export.md#2-columns-names-dialect-and-the-version) |
| R4.7 | [R2.3](prd-data-export.md#2-columns-names-dialect-and-the-version) |
| R4.8 | [R2.4](prd-data-export.md#2-columns-names-dialect-and-the-version) |
| R4.9 | [R2.5](prd-data-export.md#2-columns-names-dialect-and-the-version) |
| R4.5 | [R3.1](prd-data-export.md#3-when-an-export-cannot-finish) |
| R7.9 | [R4.1](prd-data-export.md#4-verifiability) |
| R7.7, the assertion half | [R4.2](prd-data-export.md#4-verifiability) |
| R7.8, the export-destination half | [R4.3](prd-data-export.md#4-verifiability) |
| R7.6, for this document's one surface | [R4.4](prd-data-export.md#4-verifiability) |
| R7.6a | [R4.4a](prd-data-export.md#surfaces) |
| M3 | [M1](prd-data-export.md#success-metrics) |
| OQ 8 | [OQ 1](prd-data-export.md#open-questions) |
| OQ 9 | [OQ 2](prd-data-export.md#open-questions) |
| OQ 11 | [OQ 3](prd-data-export.md#open-questions) |
| OQ 16 | [OQ 4](prd-data-export.md#open-questions) |
| E6 | [E1](prd-data-export-copy.md#error--state-copy) |
| E7 | [E2](prd-data-export-copy.md#error--state-copy) |
| E17 | [E3](prd-data-export-copy.md#error--state-copy) |
| E18 | [E4](prd-data-export-copy.md#error--state-copy) |
| DJ1 | [EJ1](prd-data-export-journeys.md#ej1-export-the-collection) |

Three rows are split rather than moved whole. [The Data Foundation PRD's R7.7](../data-foundation/prd-data-foundation.md#7-verifiability) keeps the fixtures and their content and hands the assertion here; [its R7.8](../data-foundation/prd-data-foundation.md#7-verifiability) keeps the move's failures and hands the export destination's here; [its R7.6](../data-foundation/prd-data-foundation.md#7-verifiability) stays live there for its own surfaces and this document states the same rule for its one.

## Fence → row map

A row "carries" a fence when the fence's decision is what the row now states; this file, not the row, holds the rationale. F1–F13 were transcribed from [Data Foundation’s map](../data-foundation/prd-data-foundation-fences.md#fence--row-map); later entries record amendments here.

| Fence | Rows that carry it |
| :--- | :--- |
| F1 | Every row in the PRD body, plus its Background scope statement, its shape, its 4,000-word budget, and its historical [split-origin map](#split-origin-map) (relocated by F15); no requirement row of its own. |
| F2 | [R1.1](prd-data-export.md#1-what-the-export-contains) (canonical default, P0), [R1.3](prd-data-export.md#1-what-the-export-contains) (history option, P1); the P0/P1 split in the [Legend](prd-data-export.md#legend), and the Pri cells of [R1.2](prd-data-export.md#1-what-the-export-contains), [R2.1](prd-data-export.md#2-columns-names-dialect-and-the-version), [R3.1](prd-data-export.md#3-when-an-export-cannot-finish), [R4.4](prd-data-export.md#4-verifiability); [OQ 2](prd-data-export.md#open-questions). |
| F3 | [R1.1](prd-data-export.md#1-what-the-export-contains)'s raw-payload column; [M1](prd-data-export.md#success-metrics)'s statistic. Amended by F8: one column per sample slot. |
| F4 | [R2.1](prd-data-export.md#2-columns-names-dialect-and-the-version), [R2.4](prd-data-export.md#2-columns-names-dialect-and-the-version); [OQ 1](prd-data-export.md#open-questions). |
| F5 | [R2.4](prd-data-export.md#2-columns-names-dialect-and-the-version) (a); [R2.1](prd-data-export.md#2-columns-names-dialect-and-the-version) (b). |
| F6 | [R1.2](prd-data-export.md#1-what-the-export-contains), [R2.1](prd-data-export.md#2-columns-names-dialect-and-the-version), [R2.3](prd-data-export.md#2-columns-names-dialect-and-the-version), [R2.4](prd-data-export.md#2-columns-names-dialect-and-the-version); [OQ 1's results section](prd-data-export-oq-results.md#oq-1--what-the-gamut-clipped-export-column-is-called); the Device Management export line both ways in [Inherited obligations](prd-data-export.md#inherited-obligations). |
| F7 | [R2.3](prd-data-export.md#2-columns-names-dialect-and-the-version), [R2.5](prd-data-export.md#2-columns-names-dialect-and-the-version), [R4.1](prd-data-export.md#4-verifiability), [R4.2](prd-data-export.md#4-verifiability); [OQ 4](prd-data-export.md#open-questions). |
| F8 | [R1.1](prd-data-export.md#1-what-the-export-contains), [R4.1](prd-data-export.md#4-verifiability), [R4.2](prd-data-export.md#4-verifiability); [M1](prd-data-export.md#success-metrics). |
| F9 | [R1.1](prd-data-export.md#1-what-the-export-contains), [R1.3](prd-data-export.md#1-what-the-export-contains), [R2.1](prd-data-export.md#2-columns-names-dialect-and-the-version), [R2.3](prd-data-export.md#2-columns-names-dialect-and-the-version); the Data Foundation line in [Inherited obligations](prd-data-export.md#inherited-obligations) (the chosen condition's set); the Background scope statement. |
| F10 | [R1.1](prd-data-export.md#1-what-the-export-contains), [R2.1](prd-data-export.md#2-columns-names-dialect-and-the-version); [M1](prd-data-export.md#success-metrics)'s population; [E1](prd-data-export-copy.md#error--state-copy). |
| F11 | [R1.1](prd-data-export.md#1-what-the-export-contains); [E1](prd-data-export-copy.md#error--state-copy); the inbound Data Foundation line (both time axes). |
| F12 | [R2.5](prd-data-export.md#2-columns-names-dialect-and-the-version)'s bump list; [R4.1](prd-data-export.md#4-verifiability)'s in-place refresh list; [R2.2](prd-data-export.md#2-columns-names-dialect-and-the-version)'s per-version help-docs sentence. |
| F13 | [R2.2](prd-data-export.md#2-columns-names-dialect-and-the-version) (app columns only); [R2.4](prd-data-export.md#2-columns-names-dialect-and-the-version) (emitted as stored). |
| F14 | R1.1/R1.3 and the inbound Data Foundation obligation; DF OQ 21 tracks the future distinct mark. |
| F15 | R1.1a–g/m–n, R4.1a–h, R4.2/R4.4a; EJ1; Traceability and this split-origin map; inherited obligations. |
| F16 | R1.1h–l/s, R1.2/R1.3, R4.2; E1 variants and EJ1 missing-data assertions. |
| F17 | R2.4/R2.4a–c, R4.2/R4.4a; E1 rename disclosure and EJ1 collision assertions. |
| F18 | R1.1o–r, R4.1h/R4.4a; E1 variants and EJ1 empty/preview assertions. |
| F19 | R1.2, R1.1h/i/s, R4.2; Device R6.5/export obligation; DF R7.7e; E1b/c and EJ1. |
| F20 | R4.1h/R4.2 and DF obligations; DF R7.7/R7.7n/export obligation; E1d/EJ1. |
| F21 | R1.1c/h/i/s, R4.2; E1/E1b/c and EJ1. |
| F22 | R2.4/R2.4a/c, F17; E1g/EJ1; DF R7.7a/k. |
| F23 | R2.4b/R4.2; DF R7.7a; EJ1. |
| F24 | R2.4/R4.1a; F6 remains the three-name source. |
| F25 | R2.1/R1.1k/R4.2; E1 and EJ1; DF R7.7d remains the storage oracle. |
| F26 | R1.1p/R4.2; DF R7.7i; E1f/EJ1. |
| F27 | R1.1r/R4.1h; E1d/EJ1; DF R7.7n. |
| F28 | R1.1o–r/R4.4a; E1/E1a–g; EJ1. |
| F29 | Legend/Traceability; E1 variant identity convention. |

## Rejected findings

None yet. The findings rejected on these rows before the split are recorded in [the Data Foundation PRD's fence file](../data-foundation/prd-data-foundation-fences.md#rejected-findings); neither of the two standing there touches an export row.
