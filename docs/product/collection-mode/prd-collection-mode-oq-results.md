# Open-question results: Collection Mode

One `## OQ <id>` section per answered Open Question in `docs/product/collection-mode/prd-collection-mode.md`, in ascending order. An OQ's Status in the PRD may change only when its section exists here; a question with no section here is still open, and its row in the PRD's Open questions table is what a builder reads instead.

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
