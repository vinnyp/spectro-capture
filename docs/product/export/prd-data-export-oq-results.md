# Data Export PRD — open-question results

One `## OQ <id>` section per answered question in [prd-data-export.md](prd-data-export.md)'s [Open Questions](prd-data-export.md#open-questions) table. An OQ's status may change only when its section exists here. Owner decisions that close a question are recorded as fences in [prd-data-export-fences.md](prd-data-export-fences.md); the section below points at the fence and states the answer the rows now carry.

Both sections were the [Data Foundation PRD's OQ 8 and OQ 9](../data-foundation/prd-data-foundation-oq-results.md), moved here under that document's fence F30 with their answers and evidence unchanged; those numbers are retired there and never reused.

## OQ 1 — What the gamut-clipped export column is called

Was the [Data Foundation PRD's OQ 8](../data-foundation/prd-data-foundation.md#open-questions).

**Answer (2026-09-09, owner decision, that PRD's fence F20, transcribed here as [F4](prd-data-export-fences.md#f4--the-gamut-clipped-export-column-is-sc_srgb_gamut_clipped-2026-09-09); restated round 2 under its fence F23, here [F6](prd-data-export-fences.md#f6--the-sc_-prefix-applies-to-every-app-column-2026-09-09)):** The column is `sc_sRGB_gamut_clipped`, beside `sc_sRGB_source_space` and `sc_sRGB_rendering_intent`. F4 fixed the name; F6 then made the `sc_` prefix apply to every column the app emits, with no exemption, so F4's answer is read under the prefix and no separate rule survives for this flag. F4 reads: "Closes OQ 8 by owner decision; revisited only if ISO 17972-4's schema becomes readable." Carried by [R2.1](prd-data-export.md#2-columns-names-dialect-and-the-version) and [R2.4](prd-data-export.md#2-columns-names-dialect-and-the-version), and asserted against the checked-in golden by [R4.1](prd-data-export.md#4-verifiability) and [R4.2](prd-data-export.md#4-verifiability).

**Evidence:** neither the interchange standards nor the platform defines a name for this flag ([browsing v2 §8](../../briefs/browsing-a-collection-at-scale-research-results-v2.md#gamut-containment--the-honesty-badge)), so the column is invented rather than adopted; the prior closer — read ISO 17972-4:2018's specification schema first — is named but unreachable, and the owner declined to hold the export contract open behind it.

## OQ 2 — Does an export carry version history, and in what shape?

Was the [Data Foundation PRD's OQ 9](../data-foundation/prd-data-foundation.md#open-questions).

**Answer (2026-09-09, owner decision, that PRD's fence F5, transcribed here as [F2](prd-data-export-fences.md#f2--export-is-canonical-only-by-default-with-version-history-as-a-v1-option-2026-09-09)):** The default CSV export is one row per item carrying its canonical value; an explicit option exports every version, and it ships in v1. The canonical export is P0 and the history option P1. Carried by [R1.1](prd-data-export.md#1-what-the-export-contains) and [R1.3](prd-data-export.md#1-what-the-export-contains).

**Evidence:** owner call on what a data consumer reaches for first; the history shape costs nothing to defer to the second build phase.
