# Open-question results: Collection Mode

One `## OQ <id>` section per answered Open Question in `docs/product/collection-mode/prd-collection-mode.md`, in ascending order. An OQ's Status in the PRD may change only when its section exists here; a question with no section here is still open, and its row in the PRD's Open questions table is what a builder reads instead.

## OQ 1

**Question.** The constants table's OQ 1 budgets and ceiling: how fast must browsing answer at scale, and on which machines?

**Answered.** 2026-09-26, by the owner, in the round-10 adjudication (decision D61), recorded as F209.

**Finding.** The PR #21 review asked the owner to explicitly ratify the interim values meant to be v1's contract, rather than leave them provisional in a locked, build-ready document. The owner chose to ratify all eight (OQ 1–7 and 12) as a set.

**Rule now in force.** F20, F42, F66, F99 and F118's candidates — BROWSE_RESPONSE_BUDGET 100 ms at the 95th percentile, OPEN_COLLECTION_BUDGET 1 s, DROPPED_FRAME_SHARE 1%, BULK_WRITE_BUDGET 2 s, DELETE_WRITE_BUDGET 10 s and HISTORY_READINGS_CEILING 500 readings — are v1's values. Each candidate, on an M1 MacBook Air with 8 GB, internal disk, the first open cold as the journeys' timing workload defines it and browsing warm; read every release there, per-PR timing only a tripwire; R8.11 against the engineering plan's ROW_CONFIRM_BUDGET while the capture PRD's OQ 5 is open.

**What still closes it.** Nothing decides the values further; engineering's Release-build timing runs at ROWS_CEILING on that Mac and one current Mac, and the owner's estimate of the longest real history (HISTORY_READINGS_CEILING), stay as post-lock checks. A failed check changes a value only through a new dated fence (F209).

## OQ 2

**Question.** FILE_ITEMS_CEILING: how many items must the file-wide views hold up under?

**Answered.** 2026-09-26, by the owner, in the round-10 adjudication (decision D61), recorded as F209.

**Finding.** As OQ 1's.

**Rule now in force.** F34's candidate, 100,000 items across the whole file, is v1's value.

**What still closes it.** Nothing; the owner's dogfood estimate of collections per file, checked by UJ9.5-b, stays as a post-lock check under the same rule as OQ 1's.

## OQ 3

**Question.** FIND_SIMILAR_DISTANCE.

**Answered.** 2026-09-26, by the owner, in the round-10 adjudication (decision D61), recorded as F209.

**Finding.** As OQ 1's.

**Rule now in force.** F16's candidate, ΔE2000 3.0 at or within, is v1's value.

**What still closes it.** Nothing; the dogfood "Find similar" pass over a real collection of at least 200 items stays as a post-lock check.

## OQ 4

**Question.** BULK_CONFIRM_COUNT.

**Answered.** 2026-09-26, by the owner, in the round-10 adjudication (decision D61), recorded as F209.

**Finding.** As OQ 1's.

**Rule now in force.** F7's candidate — a bulk set or clear across more than 10 items confirms first — is v1's value.

**What still closes it.** Nothing; dogfooding bulk edits stays as a post-lock check.

## OQ 5

**Question.** NEUTRAL_CHROMA: below what chroma is an item treated as having no meaningful hue?

**Answered.** 2026-09-26, by the owner, in the round-10 adjudication (decision D61), recorded as F209.

**Finding.** As OQ 1's.

**Rule now in force.** F19's candidate, C* 3.0, is v1's value.

**What still closes it.** Nothing; a dogfood hue sort of a collection holding a grey series stays as a post-lock check.

## OQ 6

**Question.** MIN_SWATCH_SIZE.

**Answered.** 2026-09-26, by the owner, in the round-10 adjudication (decision D61), recorded as F209.

**Finding.** As OQ 1's.

**Rule now in force.** F20's candidate, 24 pt, is v1's value; the swatch grid's default size and its range stay the build's choice, at or above 24 pt (F199).

**What still closes it.** Nothing; the owner's check at the P2 build, on the owner's display at working distance, with MIN_SWATCH_SIZE among the sizes the build offers (F199), stays as a post-lock check.

## OQ 7

**Question.** What decides that the display cannot show a colour: which gamut, which rendering intent, and a display reporting none?

**Answered.** 2026-09-26, by the owner, in the round-10 adjudication (decision D61), recorded as F209.

**Finding.** As OQ 1's, for the display-check half of this question only. The stored gamut-clipped flag's own test parameters are the Data Foundation PRD's, under its R3.4 and F54, and are not reopened by this ratification.

**Rule now in force.** The display check's test — adapt the current value to the display's white by Bradford and test it against the display's reported gamut under relative colorimetric at zero tolerance, treating a display reporting none as sRGB — is v1's value.

**What still closes it.** Nothing; the engineering spike on one sRGB and one Display P3 display with the harness's seeded file stays as a post-lock check.

## OQ 11

**Question.** Which published ΔE2000 values do the "Find similar" and distance cases use?

**Answered.** 2026-09-26. Engineering's check, not an owner decision — the closer OQ 11 already named ran.

**Finding.** Two independent CIEDE2000 implementations — one written from Sharma, Wu and Dalal (2005)'s equations, and colour-science's `delta_E(method='CIE 2000')` — each reproduced all 34 pairs of the paper's Table 1 to 4 decimals, against the authors' own data file `ciede2000testdata.txt` fetched from hajim.rochester.edu. Every quoted 4-decimal value in UJ3.4-a, UJ3.4-b, UJ3.4-d and UJ5.3-a equals the published value; every 2-decimal displayed value in UJ3.4-k, UJ5.4-a and UJ5.4-b equals the computed value rounded; UJ3.4-f's grey fixtures (3.0000 and 3.0035) are not Sharma pairs and match the computation exactly; every "Find similar" listing and its at-or-within-3.0 cut agrees with the computed distances. No discrepancy. No quoted value falls on a rounding tie, so the fixtures do not discriminate half-up from half-even rounding.

**Rule now in force.** F64's interim — CIEDE2000 test pairs 1–5 from Sharma, Wu and Dalal (2005) — is v1's value. Use the pairs as the journeys quote them.

**What still closes it.** Nothing; the question is closed.

## OQ 12

**Question.** IMPORTED_COLUMNS_CEILING: how wide may a collection's imported data be while the budgets hold?

**Answered.** 2026-09-26, by the owner, in the round-10 adjudication (decision D61), recorded as F209.

**Finding.** As OQ 1's.

**Rule now in force.** F43's candidate, 20 imported columns of up to 200 characters each, is v1's value.

**What still closes it.** Nothing; the widest inventory the owner dogfoods stays as a post-lock check.

## OQ 8

**Question.** Does renaming an imported column change the name the file stores?

**Answered.** 2026-09-24, by the owner, in the post-fill adjudication (decision D9), recorded as F9.

**Finding.** No research bears on it; it is a product decision. The owner chose that a rename changes the stored name — so the table, an export and an outside reader show the new name, and a later import matches on it — over a display-only name and over withholding the action.

**Rule now in force.** R4.8 offers "Rename column" at P1 with no withholding interim. The Data Foundation PRD's R1.2 and the Inventory Import PRD's R2.6 are amended in the same change (their fences F50 and F65): the stored name moves, the column keeps its position and values, and a source header equal to the old name arrives as a new column under the import PRD's existing rules. F9 closes the post-lock item that asked the question; that item is ticked when this change's pull request exists.

**What still closes it.** Nothing; the question is closed.

## OQ 9

**Question.** How is deleting a selection confirmed?

**Answered.** 2026-09-24, by the owner, in the post-fill adjudication (decision D7), recorded as F7.

**Finding.** No research bears on it; it is a product decision. Before this change the Data Foundation PRD named only its item-scale (E8) and collection-scale (E14) delete confirmations. The owner chose to add a selection-scale confirmation to that PRD in this same change, over withholding "Delete selected" until a later amendment.

**Rule now in force.** R6.3 opens the Data Foundation PRD's E33 — the selection-scale confirmation that PRD's R6.2 now names, tested by its R7.6o (its fence F50) — and carries no withholding interim.

**What still closes it.** Nothing; the question is closed.
