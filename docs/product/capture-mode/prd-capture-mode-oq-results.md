# Capture Mode PRD — open-question results

One `## OQ <id>` section per answered question in `prd-capture-mode.md`'s Open Questions table. An OQ's status may change only when its section exists here. Owner decisions that close a question are recorded as fences in `prd-capture-mode-fences.md`; the section below points at the fence and states the answer the rows now carry.

## OQ 15 — Does find on the capture surface search the alternate code and alternate name?

**Answer (2026-09-06, owner decision, fence F21):** Yes. Find matches Swatch Code from the start, Swatch Name by any part, and the alternate code and alternate name the same two ways, all under the one matching rule (fence F11). When a hit matched an alternate, the result says which field matched. Carried by R6.1.

**Evidence:** owner call; no research bears on it. The alternates exist because a swatch is known by more than one name.

## OQ 18 — Does opening the deferred-row review mid-session discard the current item's partial set?

**Answer (2026-09-18 owner ratification under F60; F19 as refined by F42/F44):** Entry follows R8.1a–k, not a blanket discard rule. End-of-run and detour review from a live bulk session discard the current partial set; a look-through does not discard on entry, and a guard-held set remains intact. Under a device halt the partial set was already discarded and a completed unsaved set remains held; a one-row session's set is discarded only on selecting another row or settling its current swatch. Read the matrix for remembered-row, exit and session outcomes.

**Historical evidence:** F19's 2026-09-06 answer said yes to mid-session review entry; F42/F44 subsequently distinguished look-through, detour and end-of-run entry. This clarification carries those settled distinctions and does not re-open OQ 18.

## OQ 19 — How many store files does a user's work live across?

**Answer (2026-09-16, closed by the Data Foundation PRD's fences F2 and F14):** One. F2 settles that a user's work lives in one file with one open at a time, and F14 that the first launch asks where that file lives; both are carried by that PRD's R1.1, R1.3 and R1.8. A collection's rows, its queue order, its remembered row, and its sessions all travel together in that one file, so R1.9 stands unchanged.

**Evidence:** the Data Foundation PRD's decision, not a capture one — this question was always that document's to answer, and capture only needed the count to be settled.

## OQ 10 and OQ 17 — folded into OQ 8 (fence F20)

Not answered. The review's surface and the summary's form are settled together with the seam (OQ 8, ADR-0004) by the Demo Device prototype of each reading. This section exists so the fold is recorded; the answers land under `## OQ 8` when the prototype closes it.

## OQ 16 — Inherited notes for the device PRD

**Answered on Device re-lock (PR #19)** — 2026-09-18, Device F24 / Capture F50: The owner approved carrying the settled notes in the Device Management agent-build amendment ([Device F12](../device-management/prd-device-management-fences.md#f12--carry-captures-device-obligations-2026-09-18), mirrored by Capture F50). Device R2.6/R4.3/E7 permit non-spectral capture; R6.6/R6.9 add the trigger exception and spectral switch; R6.27 and Capture R11.3 agree on P0 DEMO_SCAN_CYCLE. Device §5 distinguishes same-session halt recovery from a new session after interruption, follows Capture's current-row rules, carries the End warning and makes R5.18 quit-from-halt P0; sleep remains a halt.

**Evidence:** [final review](https://github.com/vinnyp/spectro-capture/pull/19#pullrequestreview-5252472266) closed the amendment’s peer review on 2026-09-18; PR #19’s merge is the Device re-lock that answers Capture OQ 16 under D10/F24. This is document review, not hardware verification or implementation; Device OQs remain open/residual as before, and Capture OQ 2 still decides whether the physical-button trigger exception is needed.

**Clarified 2026-09-18:** Device F16 keeps its own E33 confirmation before Capture E23’s halted-session summary; this result section records the amendment’s contents, not review completion.

**Round-2 clarification (2026-09-18):** Device F28 skips the halted-session summary on confirmed Quit and leaves the session ended at next launch; F29 sends measurement-time drift to Capture’s per-scan path. OQ 16 still waits for Device re-lock.

**Closed 2026-09-18 ([final review](https://github.com/vinnyp/spectro-capture/pull/19#pullrequestreview-5252472266)):** peer review closed 2026-09-18 (PR #19); re-locked on merge. The pending statements in the earlier dated clarifications above are superseded; OQ 16 is answered on this Device re-lock under D10/F24.
