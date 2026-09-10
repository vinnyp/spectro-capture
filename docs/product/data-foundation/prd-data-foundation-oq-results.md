# Data Foundation PRD — open-question results

One `## OQ <id>` section per answered question in [prd-data-foundation.md](prd-data-foundation.md)'s [Open Questions](prd-data-foundation.md#open-questions) table. An OQ's status may change only when its section exists here. Owner decisions that close a question are recorded as fences in [prd-data-foundation-fences.md](prd-data-foundation-fences.md); the section below points at the fence and states the answer the rows now carry.

## OQ 1 — How many files may a user's work live across, and may more than one be open?

**Answer (2026-09-09, owner decision, fence F2):** A user's data lives in one store the user owns, and one store is open at a time; the matching rule and every cross-collection view span that one file. Carried by R1.1 and R1.3.

**Evidence:** the browsing research favours one store over many, and [the capture PRD's OQ 19](../capture-mode/prd-capture-mode.md#open-questions) already leaned this way; the count was handed here to be stated once.

## OQ 3 — Is the correction-against-re-measurement question asked, and where?

**Answer (2026-09-09, owner decision, fence F3; amended round 1, fence F12):** Asked, and asked once, outside the heads-down loop; a re-scan made during capture is never asked mid-loop, and the answer given later lands on the version. F12 amends what the unanswered state is: the supersession reason is correction-unconfirmed, not correction — the reading is shown and marked rather than treated as never true, the question offers "Ask me later", and the whole unanswered set is answerable in one pass after a session. Carried by R2.4 and R2.8, which hand [the capture PRD's E29](../capture-mode/prd-capture-mode-copy.md#error--state-copy) the correction default as a post-lock amendment, in this document's axis word — supersession reason.

**Evidence:** the research is unambiguous that the distinction cannot be inferred from the data, so not asking would fabricate it; asking mid-loop breaks heads-down capture. The amendment closes the gap the round-1 gate found — recording an unanswered re-scan as a correction and then hiding the superseded reading fabricates history just as surely.

## OQ 4 — HISTORY_RETENTION

**Answer (2026-09-09, owner decision, fence F6):** Nothing is aged out. No retention window exists, and HISTORY_RETENTION is "for good". Carried by R2.6.

**Evidence:** corrections never destroy data ([AGENTS.md §4](../../../AGENTS.md#4-non-negotiables)), and the research names retrofitting a policy onto a store that cannot enumerate its history as the trap.

## OQ 7 — GAMUT_REFERENCE_SPACE and GAMUT_RENDERING_INTENT

**Answer (2026-09-09, owner decision, fence F4):** GAMUT_REFERENCE_SPACE is sRGB and GAMUT_RENDERING_INTENT is relative colorimetric, stored with the datum so the flag means the same thing to every reader of the file. A live display-dependent check is Collection Mode's, not this file's. Carried by R3.4.

**Evidence:** owner call. "In gamut" is intent-relative and the platform never says which intent its own check uses, so the file states its own.

## OQ 8 — What the gamut-clipped export column is called

**Answer (2026-09-09, owner decision, fence F20):** The column is `sRGB_gamut_clipped`, beside `sRGB_source_space` and `sRGB_rendering_intent`. The fence reads: "Closes OQ 8 by owner decision; revisited only if ISO 17972-4's schema becomes readable." Carried by R4.2, and asserted against the checked-in golden export header by R7.7.

**Evidence:** neither the interchange standards nor the platform defines a name for this flag ([browsing v2 §8](../../briefs/browsing-a-collection-at-scale-research-results-v2.md#gamut-containment--the-honesty-badge)), so the column is invented rather than adopted; the prior closer — read ISO 17972-4:2018's specification schema first — is named but unreachable, and the owner declined to hold the export contract open behind it.

## OQ 9 — Does an export carry version history, and in what shape?

**Answer (2026-09-09, owner decision, fence F5):** The default CSV export is one row per item carrying its canonical value; an explicit option exports every version, and it ships in v1. The canonical export is P0 and the history option P1. Carried by R4.1 and R4.4.

**Evidence:** owner call on what a data consumer reaches for first; the history shape costs nothing to defer to the second build phase.

## OQ 10 — DELETE_UNDO_WINDOW

**Answer (2026-09-09, owner decision, fence F7):** DELETE_UNDO_WINDOW is the running session: a deleted item and its history can be restored until the app quits, and the deletion is final after. Carried by R6.3.

**Evidence:** owner call; the research notes a reversible bulk delete is cheap.
