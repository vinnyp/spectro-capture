# Capture Mode PRD — the F45 compaction pass

Resume point for the one-pass compaction of `prd-capture-mode.md` §7 authorised by fence F45 (owner, round 15): every requirement row at most two sentences, the table shorter rather than longer, rationale and fence citations out of the rows, the set-aside seam replaced by one state-by-exit table.

Before: 144 R rows, 17,253 words in §7, mean 120 words per row, longest row 1,035 words (R8.1).
After the compaction pass: 160 R rows, 11,155 words, mean 70.
After the follow-on tightening pass — 19 rows whose two sentences were still over 100 words, brought under about 80 without splitting a row or adding one: **160 R rows, 10,496 words, mean 66, longest row 151 words (R11.15). Rows over two sentences: 0.**

Two rows are still over 100, both closed enumerations that cannot reach 80 without dropping an item: R11.15 (151 — seven surfaces and about thirty things they show) and R11.9 (112 — the seven ordered steps of the contributor walk). Five more sit between 90 and 99 for the same reason: R8.13 (99, the version-history list), R11.12 (98, nine state-variant pairs), R10.7 (93, four observables plus how a mode slip is counted), R3.12 (92, three constants and their three bindings), R11.16 (90).

Row-by-row before/after word counts and the split map are in `compaction-map.md`, handed to the round-16 reviewers with this pass.

Every §7 row carries the status `⌛️ Ready for Alignment — compacted (F45), verify in round 16`, whatever it held before — all 160, including the 6 that were open. E27 carries `⌛️ Ready for Alignment — edited in the compaction pass, re-review`; no other E row and no M row changed. The journeys, the diagrams, §12's Labels and Placeholders paragraphs, the Surfaces table, the Success Metrics table, and the Open Questions table are byte-identical to the round-15 document.

## Checklist

- [x] §1 Collections — 10 rows, no splits; 965 → 555 words. R1.5 absorbed the OQ 21 reference-set note; R1.7 lost the E2 action list (E2 carries it) and cites R8.16 for the offer condition.
- [x] §2 Inventory import — 14 rows, no splits; 1,166 → 864 words. The header-signature rule folded into R2.2 and the blocked-import rule into R2.10; R2.8 absorbed the "codes that merge under R2.7" case.
- [x] §3 The capture session — 12 → 13 rows (new R3.13); 1,445 → 915 words. R3.6 and R3.8 now cite the state-by-exit table instead of restating the browse/detour/end-of-run cases.
- [x] §4 The scan loop — 22 → 26 rows (new R4.23, R4.24, R4.25, R4.26); 2,635 → 1,844 words.
- [x] §5 Per-scan failure and the guard — 12 → 14 rows (new R5.13, R5.14); 1,395 → 1,026 words.
- [x] §6 Queue navigation and reordering — 12 rows, no splits; 940 → 765 words.
- [x] §7 Pause, end, interruption, and resume — 15 → 19 rows (new R7.16, R7.17, R7.18, R7.19); 2,125 → 1,304 words. R7.1 went from 753 words to 79.
- [x] §8 Deferred-row review and corrections — 15 → 17 rows (new R8.16, R8.17); 3,114 → 1,287 words. R8.1 went from 1,035 words to 78.
- [x] §9 Ad-hoc capture and one-row sessions — 11 rows, no splits; 716 → 625 words.
- [x] §10 The seam — 7 → 8 rows (new R10.8); 597 → 462 words. The per-row "Gated on OQ 8" is dropped; the section preamble says it once.
- [x] §11 Demo Device and verifiability — 14 → 16 rows (new R11.15, R11.16); 2,155 → 1,508 words. R11.12's enumeration survives whole across R11.12 (states, variants, conditional lines and actions) and R11.15 (surfaces and the import preview).
- [x] The set-aside state-by-exit table — placed in §8 immediately after R8.1's row; 11 rows × 6 columns (entered with / opens as / discards / remembered row / leaving does / session outcome). Records F19, F31 as clarified 3×, F41, F42 as clarified 5×, F43, F44 as clarified 2× without reinterpreting them. R8.1, R8.6 and R8.15 point at it; R3.6, R3.8, R3.11, R7.1, R7.11 and R8.3 cite it instead of restating it.
- [x] The Traceability paragraph — rewritten as a fence-to-row map covering F1 through F45. Every fence citation is gone from the rows.
- [x] Final consistency check — 160 rows, 0 over two sentences, 0 dangling `[Rx.y]` or `[Ez]` citations, 0 unresolved heading anchors, no literal `&amp;`, all 6 mermaid blocks untouched, every action a row names still present in the Labels paragraph or a state's copy.

## Residue folded in

- R3.6 no longer glosses the end-of-run review's ways in; it cites the state-by-exit table, which is where R8.6's three ways in live.
- E27's "with everything the instrument did read" is now "with everything already kept on it", the phrase E39 uses (singular, because E27 names one swatch).
- R8.15's held-case reason — the one that was untrue of a save-failure halt holding a completed set (R7.2) — is dropped rather than repaired; the table's "session outcome" column carries the outcome without a reason.

## For round 16

- The Open Questions table's "Feeds (row IDs)" column was left untouched by instruction, so it still points only at the parent rows of the sixteen splits. OQ 3 now also feeds R5.13, R5.14 and R4.23; OQ 8 also feeds R10.8; OQ 13 also carries R3.12's bindings; OQ 16 also feeds R4.24 and R4.26; OQ 24 now feeds R11.16 rather than R11.13. Worth a pass once the compaction is verified.
