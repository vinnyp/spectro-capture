# Nix Toolkit import amendment — round 2 fixes

Resume point for round 2 (delta verification of f7cbde5; review log `docs/agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md`, Round 2). One box per finding group, naming every round-2 finding and every round-1 finding a lens marked PARTIAL or UNRESOLVED. Lens prefixes: PM-R2 (product manager), SSE2 (staff engineer; its review numbers them R2-*), TR2 (test), IF2 (interface), AR2 (architecture), PRIV2 (privacy), PMM2 (product marketing), PL2 (plan; its review numbers them R2-*), DB2 (database). No new owner decision was needed: every fix sits inside a recorded fence, and the dated round-2 Clarified lines under Import F71, F73, F76 and F82, DF F64, Collection Mode F221, Export F35, Device F33 and Capture F77 record them.

## Majors

- [x] **The commit recheck** — AR2-M1, TR2-M1, SSE2-M1, PL2-M4, DB2-m2, AR round-1 M5 partial, PL round-1 M6 partial. R3.8k rechecks the mode fit and each matched item's R6.8 outcome; a target whose mode or outcomes changed returns to a fresh preview through E13's new collection variant; one that no longer fits refuses with E46. UJ 3: the refusing case now has a target holding a reading; new cases for a pending-only target switched to M1 and for a matched item scanned under an open preview.
- [x] **Salvage of two current readings** — AR2-M2, IF2-M2, DB2-M1, PM-R2-m6, PL2-m1, SSE2-m4, TR2-m12, AR round-1 m1 partial, DB round-1 salvage partial. DF R5.5d keeps the reading R2.3j would leave current, the other kept behind with no question; otherwise the later-recorded. DJ6 gains a salvage case over the three pairings.
- [x] **The fixture range** — IF2-M1, TR2-M3, PL2-M5, AR2-m1, DB2-m3, SSE2-m9, PM-R2-m7, IF round-1 M10 partial, AR round-1 M7 partial, SSE round-1 M8 partial, PL round-1 G2 partial. R7.7a–o at Export R4.2, Export's Data Foundation line, Export's journeys header and DF's Data Export line.
- [x] **E48 overclaim** — PMM2-M1, PM-R2-m4, IF2-m4, SSE2-m1, DB2-n3. E48 says only the Lab is checked; it names "any other column" (PRIV2-3).
- [x] **Toolkit remedies written out** — PMM2-M2, IF2-m2, TR2-m11. E7, E9, E10 and E42 each carry a written Toolkit variant; the splice rule is gone; UJ 3 gains an E10 case.
- [x] **History-order case** — TR2-M4, IF2-m7, PL2-M2. UJ2.1-s reads E17 in its recorded order, not the P1 restore variant; UJ2.1-t discriminates both orders.
- [x] **Not-checked pairs** — TR2-M2, PL2-m2, SSE2-m11 (workable half). R6.6 ties "not checked" to a pair the app offers (Capture R1.5, OQ 21); UJ 3's case declares the offered set under Capture R11.6 and asserts TK-2's recorded provenance (TR2-m3).
- [x] **How imported fixtures are seeded** — PL2-M1, PL round-1 G1 partial. R6.10: another PRD's case may declare an imported reading directly; only Import's cases and DF R7.7o run the importer. Collection Mode gains a Build-dependencies row; post-lock gains the build-order item.
- [x] **Tolerance-relative offsets** — PL2-M3, SSE2-m7, TR2-m1. UJ 3's reference is the one DF R7.5 checks the build against; its offsets are multiples of DERIVATION_TOLERANCE.
- [x] **Bounding what dogfood records** — PRIV2-1. M2's and OQ 3's records are format facts and tallies by state; R6.10 says "any data value"; post-lock's dogfood item mirrors it.

## Minors and Nits

- [x] **E43 and E14 wording** — PMM2-m1, PMM2-m2, PMM2-m3, PMM2-m4, PMM2-m5, PMM2-m6, PMM2-n1, PMM2-n2, PMM2-n5, PM-R2-m1, PM-R2-n1, IF2-m3, SSE2-m8, AR2-n3, PL2-m6, DB2-n5, PM round-1 PM9 partial. "Now a swatch's colour", "Kept in history, not used as the colour" scoped to an instrument scan and to a later Toolkit reading, the same-moment line, the not-checked reason, E17's permanence wording, the claim scoped to what SpectroCapture takes, "Swatch details —" row counts, E14's Toolkit headline and instrument clause, set-aside items "no longer set aside".
- [x] **Bridge phrasing** — PMM2-n3. One form: "— the Toolkit's Measurement Mode —".
- [x] **E46 mixed-modes path** — PM-R2-m2, PMM2-n4, PM round-1 m13 (first half). A pointer: keep each measurement condition's colours in their own Toolkit collection.
- [x] **Mode switched away later** — PM round-1 m13 (second half). Post-lock item for Capture R1.10.
- [x] **E5 on a Toolkit export** — PM-R2-m3, IF2-m1, TR2-m10, SSE2-m2, PL2-m4, IF round-1 M1 partial. R6.1 and E5's Toolkit variant: no read control, the Toolkit remedy; UJ 3 case with an unescaped quote. *(Corrected in round 3: that claim was wrong for a file whose header decodes; R6.1 now sends a Toolkit export not decoded as UTF-8 throughout to E5's Toolkit encoding variant.)*
- [x] **M2's definition** — PM-R2-m5. Clean means no exclusion for any reason, no mixed-modes or E49 refusal, no differing record; not-checked tallied apart.
- [x] **Exclusion order** — TR2-m9, SSE2-m3, PL2-m3, TR2-n2. §1–§3's and R6.7's exclusions run before R6.4, then R6.11; UJ 3's E47-then-E46 case names "Continue without them".
- [x] **Quarantined readings** — AR2-m2, DB2-m1, SSE2-m12, TR2-m7. R6.8f: a quarantined reading is never the same; the ADR-0003 key exempts it; UJ 3 case.
- [x] **Time and number grammar** — SSE2-m6, DB2-n2, IF2-n7, PL2-m5. RFC 3339, truncated to the millisecond; the exponent's optional sign; E47 names every failing column.
- [x] **Values without a case** — TR2-m4, TR2-m5, TR2-m6, TR2-m14, TR2-m2, TR2-m8, PL2-n2. A negative reflectance, a blank Nix Device, an empty cell, `Infinity`, a comma decimal, a target holding a reading only in history, the tab branch with an R2.3-variant header, sRGB margins on TK-1 and TK-2, and TK-2's and TK-1's declared names in the scan-current case.
- [x] **Unknowns stored absent** — DB2-n1, TR2-n1, AR2-n2, SSE2-m11 (storage half). DF R2.3j "absent (NULL), never a placeholder"; Import R6.5's model and serial; DJ6 and Device UJ3-c read absent; DJ6 row 1 says "no payload, and no archive-unavailable mark".
- [x] **Model unknown rendered** — SSE2-m5, PMM2-n6, IF2-n2. Collection Mode R4.2d and its R4.2d imported and R5.2b lines; Device R1.21 "if any".
- [x] **Creation form label** — PMM2-m8. R6.3 and the copy note: "Measurement condition: ⟨mode⟩, set by this Nix Toolkit export".
- [x] **Target step before E48** — IF2-m5, SSE2-m10, PL2-n1, IF round-1 m12 partial. UJ 3's cases create the target before E48; R6.1 narrows "every later step" to mapping (E48) and preview (E43).
- [x] **E43's control note** — PMM2-m7, IF2-m6. Toolkit settings shown fixed without controls.
- [x] **Export residues** — PMM round-1 m5 partial, IF2-n5, IF2-n4, AR2-m4. E1's line names basis and archives; Export's Device line and EJ1's quarantined row name `sc_imported`; R1.1t labels a reading kept behind `superseded`. The status line is shortened so Export stays at 3,992 of 4,000 words.
- [x] **R6.8f and metadata** — IF2-n1. R6.8f "adds no reading and changes no reading or state; metadata follows R3.5/R3.6".
- [x] **Pre-fill text** — SSE2 nit (R6.3). The first record's Custom Collection Name pre-fills.
- [x] **F79's map** — IF2-n3, IF round-1 M8 partial. F79 names the Data Export line.
- [x] **Capture M8 scope** — IF2-n6. "A Nix Toolkit export's import".
- [x] **Restore then re-import** — AR2 Missing, DB2 Missing. A UJ 3 case, run once Collection Mode R5.5 lands.
- [x] **ADR-0003 inputs** — AR2-m3, AR2-n1, DB2-n4, DB2 Missing (one current per item). Exact spectral read-back; determinable by plain SQL; at most one current reading per item, enforceable by the schema; restores and quarantined readings exempt from any uniqueness constraint; the sentence break.
- [x] **Standing repo rule** — PRIV2-2. AGENTS.md §5 keeps real vendor-app exports and their values out of the repo.
- [x] **Vision** — PMM2-n7. The Problems headline and the first "Toolkit" naming the Nix Toolkit app.
- [x] **Round-1 fix file** — TR2-n3, PL2-n3, SSE2 nit, PM round-1 PM8 nit; TR2-n4. `1.04700000e+0` corrected; three uncited IDs listed.

## Recorded without change, each with its reason

- [x] **J7's heading** — PM round-1 PM-n1 partial. "(cataloger; v2 candidate)" stays because three documents link its anchor, and its first bullet is marked "In part superseded — see the update".
- [x] **`undefined` in other columns** — SSE2 round-1 m13 partial. Added to OQ 3's closer; until then only Note's `undefined` is special, and elsewhere the text is stored as given.
- [x] **PRIV-7's home** — PRIV2 nit. The cross-model rule lives in the review log, a review-process rule, not a post-lock item.
