# Capture Mode PRD — open-question results

One `## OQ <id>` section per answered question in `prd-capture-mode.md`'s Open Questions table. An OQ's status may change only when its section exists here. Owner decisions that close a question are recorded as fences in `prd-capture-mode-fences.md`; the section below points at the fence and states the answer the rows now carry.

## OQ 15 — Does find on the capture surface search the alternate code and alternate name?

**Answer (2026-09-06, owner decision, fence F21):** Yes. Find matches Swatch Code from the start, Swatch Name by any part, and the alternate code and alternate name the same two ways, all under the one matching rule (fence F11). When a hit matched an alternate, the result says which field matched. Carried by R6.1.

**Evidence:** owner call; no research bears on it. The alternates exist because a swatch is known by more than one name.

## OQ 18 — Does opening the deferred-row review mid-session discard the current item's partial set?

**Answer (2026-09-06, owner decision, fence F19):** Yes. The one partial-set rule has no look-only exception: entering the review puts a different item under the instrument, so the samples taken so far are discarded and the row returns to "sample 0 of N". Carried by R7.1 and R8.1.

**Evidence:** owner call. The rule's value is that it has no exceptions; a look-only entry is a later refinement if dogfood asks for it.

## OQ 10 and OQ 17 — folded into OQ 8 (fence F20)

Not answered. The review's surface and the summary's form are settled together with the seam (OQ 8, ADR-0004) by the Demo Device prototype of each reading. This section exists so the fold is recorded; the answers land under `## OQ 8` when the prototype closes it.
