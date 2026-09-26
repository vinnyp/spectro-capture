# Collection Mode PRD — round 5 fixes (review round 5, 2026-09-25)

The resume point for the fix pass over the findings of review round 5 (subject commit `7975d26`) and the first run of
the mechanical lock checks (`7975d26`'s `docs/product`), before the pre-lock round; this list drafted at `abbc2a0`.
The fence file `prd-collection-mode-fences.md` (F1–F188) is the written authorization for every change below: a
**fix** names the fence that authorizes it, or reads "editorial/testability — no new WHAT" where it only adds a case,
a seam grant, a fixture, a cite, an index entry or a wording fix that makes copy or a hand-off match an existing row.
A change no fence names is not made: it is reported to the orchestrator as an unratified WHAT. The owner has decided
this round's forks as F173–F177 and approved round-5 recommendations 1–11 as F178–F188, so no box below waits on the
owner but the one marked **needs owner**; anything the pass meets that no fence states is reported, not made. Each box
is ticked, with a Result note, as its fix lands.

**Word budgets.** Counted by rule 14's method, the body only, companions excluded:

~~~
perl -0pe 's/<!--.*?-->//gs; s/```.*?```//gs; s/\]\([^)]*\)/]/g; s/^\|[-: |]+\|\s*$//mg; s/\|/ /g' FILE | wc -w
~~~

Re-run at `abbc2a0`: **Collection Mode 11,997 of 12,000; Data Foundation 8,292 of 8,400 (F177); Export 3,723 of
4,000.** Every count below was taken on the drafted text by the same method; the pass re-counts.

- **The Collection Mode body is the binding constraint: 3 words of headroom.** Every body addition names, in its
  Result note, the rule-free text it replaces in the same pass; a fix that cannot be paid for is flagged, not squeezed
  in. Ledger on the drafted text:
  - Additions, required: check 2's phase marks +35 (the collection surface +27, the item detail +2, the version
    history view +6; L2-1); check 1c's qualified cites +4 (R1.3 +2, R4.6 +2; L1c-1); check 20's R8.10f extension +4
    (L20-1); R8.8's "its wipe" +1 (IF5-4) — **+44**.
  - Additions on the orchestrator's ruling: R2.6's qualified cite +2 (L1c-1); E10 on the item detail +6 (interface
    N4); F187's clause in the Outbound Data Foundation row +7 (F187-3) — up to **+15**.
  - Savings from findings: R8.1f −4 (interface N1); the inbound Capture R4.22 cite −3 (L1a-2) — **−7**.
  - Net **+37** (12,034 before any trim), or **+52** with every ruling taken (12,049). The pass needs **at least −34**,
    or **−49**, from the candidates below, and ends at or under 12,000; the pre-lock round and the lock status line
    (+1) need headroom after it, which the orchestrator sizes.
- **Data Foundation: 108 words of headroom, and F177 says "no trimming".** Every DF addition is drafted at its
  tightest; nothing in that body is trimmed. Ledger on the drafted text: R6.2a +15 (F157's order +1, F184 +2, F173
  +12; F173-1); R7.3j +4 (F174-1); R1.11 +13 (F182 +6, F176 +7; F182-1, F176-1); R6.3 +16 and OQ 20 +9 (F187-1,
  F187-2); R7.6d +3 (PERF5-3); the inbound Collection Mode line's Rows +1 (ARCH5-N1); the outbound Collection Mode line
  +37 (F188-1, tight form) — **+98, about 8,390**. Variants, each the orchestrator's: OQ 20 carrying F57 by its
  existing "As … R6.3 state" (−9); the seam line's full form (+6) or by-reference form (−23); F187's clause in the
  inbound line (+7). F175 needs no DF row text (F175-1). F177 expected "about 50"; the seam line is the difference.
  **Stop rule:** land each at its tightest form with the same meaning; if the body would pass 8,400, land everything
  else, stop and report the figure. Never cut a Data Foundation rule.
- **Export: 277 words of headroom.** R1.1 +2 (5N1), the inbound Collection Mode line +1 (interface N5), the new outbound
  Collection Mode line +17 (F188-4), the status clause about +24; R4.3's held-export input +11 on the orchestrator's
  ruling (PERF5-2) — about **+44 to +55**, about 3,767 to 3,778.

**Candidate trims of rule-free prose in the Collection Mode body** (counts by rule 14's method on the drafted text;
each is meaning-checked against the row, fence or table named before it goes; none touches a requirement row, so no
row's status moves):

1. **The three unmarked Surfaces "Shows" cells, cited to their rows** (−24; not template text). The collection list
   "Every collection in the file with its item count, …" → "R1.1's list, …" (−7); the All items view "Every item of
   every collection, one row each naming its collection, with search, filters and sorts" → "R1.9's rows, with R1.10's
   search, filters and sorts" (−8); the swatch grid "The collection surface's items as swatches at a chosen size, with
   their marks" → "R7.1's and R7.2's swatches" (−9). Must still state it: R1.1; R1.9 and R1.10; R7.1 and R7.2. It sits
   in the table check 2's marks already edit (L2-1).
2. **Build contract, paragraph 2: the clause after "neither reading is selected here"** — ": every row states behaviour
   on the collection surface without deciding how the user reached it or where it sits relative to capture, and where
   the session summary sits stays the capture PRD's F20" → "." (−33; not template text: the template asks only that the
   fork be named with its OQ and ADR and that neither reading be selected). Must still state it: the Build dependencies'
   last row ("Where capture sits relative to the collection | no row here | Stop: the capture PRD's OQ 8 and ADR-0004 —
   navigation and placement are never guessed") and the capture PRD's F20.
3. **Row transitions preamble, second sentence** — "The entity is an item or a collection as this PRD shows it; a
   reading's states are the Data Foundation PRD's and a row's queue states the capture PRD's, and a route changing one
   names the row it acts through." → "The entity is an item or a collection as this PRD shows it." (−27; not template
   text: the template's preamble is the first sentence alone). Must still state it: the Vocabulary preamble ("The Data
   Foundation PRD's reading, … and the capture PRD's pending, captured, set aside, settled, session and remembered row,
   are used unchanged") and the table's own lines naming the row they act through (R4.6/R5.5's "(the Data Foundation
   PRD's R2.3f)", R4.9's "which the capture PRD's R5.6 sets aside").
4. **OQ 7's Decision cell** — "The Data Foundation PRD's R3.4 states the stored flag's own test at sRGB (its F4, F53
   and F54; F127) and leaves the display check here; closing this question changes that test only through a Data
   Foundation fence and a new derivation version." → "The Data Foundation PRD's R3.4 states the stored flag's own test
   (F127, F146); the display check is this question's." (−23; not template text). Must still state it: DF R3.4 (sRGB,
   and "closing Collection Mode OQ 7 changes this test only through a fence here and a new derivation version"), F146
   and its dated line under F127; DF F4/F53/F54 stay in DF's own map.
5. **Build dependencies prose: the two "because" clauses** — "ADR-0003 stopping every row because every row reads or
   writes the file whose schema it fixes, and ADR-0006 because every row runs on its macOS floor" → "ADR-0003 and
   ADR-0006 stopping every row" (−20; not template text: the template's paragraph has no rationale). Must still state
   it: every row's "Stop: ADR-0003" cell, F124 (ADR-0006 a stop on every row, in this paragraph only).
6. **The Inbound Product README line's obligation** — "Browse at scale; search, filter, facet and sort; editing
   surfaces; selection and bulk operations; the version-history UI; the gamut-aware swatch grid; the UI half of U5 and
   the swatch-grid half of U7" → "§6's scope, the UI half of U5 and the swatch-grid half of U7" (−19; not template
   text). Must still state it: `docs/product/README.md` §6 (which lists exactly those six items) and the line's own Rows
   cell. Check 1a disposes this line as an index, not a PRD.
7. **OQ 1's Decision cell** — "F20, F42, F66, F99 and F118 hold the candidates; the capture PRD's FIND_BUDGET closes
   at or below BROWSE_RESPONSE_BUDGET (F90)." → "F20, F42, F66, F99 and F118's candidates." (−12; not template text;
   round 4's "F⟨n⟩'s candidate." form). Must still state it: the constants table's five OQ 1 rows, and the Outbound
   Capture Mode row ("its FIND_BUDGET closes at or below BROWSE_RESPONSE_BUDGET (its OQ 13)"), F90.
8. **OQ 5's Decision cell** — "F19 holds C* 3.0 as its candidate; no source measured." → "F19's candidate; no source
   measured." (−5; not template text). Must still state it: the constants table's NEUTRAL_CHROMA row.
9. **OQ 11's Decision cell** — "The journeys use CIEDE2000 test pairs 1–5 from Sharma, Wu and Dalal (2005). F64
   ratifies the interim." → "F64's interim: CIEDE2000 test pairs 1–5 from Sharma, Wu and Dalal (2005)." (−5; not
   template text). Must still state it: OQ 11's Interim cell and F64.
10. **Build contract, paragraph 1: "not yet written" twice** — "the QC & Comparison PRD, not yet written, owns checking
    a swatch against its reading; and the Color Visualization PRD, not yet written, owns the 3D plot." → "the QC &
    Comparison PRD owns checking a swatch against its reading, and the Color Visualization PRD the 3D plot, neither yet
    written." (−4; not template text). Must still state it: the same sentence.
11. **Build contract, paragraph 1: "which re-importing approximates"** (−3; not template text — rationale inside a
    non-goal). Must still state it: nothing; the non-goal "moving an item between collections" stands.

Candidates 1–11 total **−175**. **Template body text, not candidates:** the Inherited-obligations preamble's rationale
clause "because ID families, fence numbers included, repeat across PRDs" (−9), the User journeys paragraph, the §8
preamble, the Open-questions trailer, the Phase rule and Status vocabulary, the Traceability bullets; and R1.7 is never
trimmed.

The findings are the eight round-5 delta reviews in `docs/agent-reviews/2026-09-24-prd-collection-mode-peer-reviews.md`
(Round 5 › Per-lens reviews (verbatim)), in log order; that log's Round 5 verify-the-reviewer table ("log row n")
records the seven accepted Blockers and Majors: rows 1, 5, 6 and 7 are testability fixes, row 2 is F157's order
restored (editorial under F157, as its F173 line clarifies it), rows 3 and 4 the owner's F175 and F173. Each box
carries the reviewer's own ID and severity. A round-4 finding that a review's delta table marks PARTIAL or UNRESOLVED
gets its own box, labelled "carried: ⟨ID⟩". One box carries each fix, the first in log order, and every other box
raising the same defect says **covered by** it; where a later box adds a part the first lacks, that part is its own.
An item a review raised only in its Missing or Deferred list, carried by no rated finding, gets a box marked *unrated*
where it asks for a change, and a line in the review's closing paragraph where it asks for none. The fence edits are
boxed once, in "Owner decisions F173–F188 — the edits each requires" below (F173-1 and so on); the lock-check fixes
once, in "Lock-check run 1 — MISS fixes" (L1a-1 and so on). Sibling IDs name their document ("DF", "Capture",
"Import", "Export"); a bare ID is this PRD's own.

Dispositions: **fix**, **covered by**, **declined (existing F-rejection)** and **needs owner**. Needs owner is strict:
any new or changed product rule, copy meaning, constant or interim, or sibling rule that no fence F1–F188 states. This
list marks one, an unrated pointer (the Device mirror of DF R1.11); the pass makes nothing for it.

Drafted text for a sibling row is written without its link targets; each label keeps the target its cell already
uses, and a new label takes its row's section anchor as the cell's other links do.

## Orchestrator rulings on this list (recorded 2026-09-25, before the pass)

Each ruling below overrides the box it names; where a box says "on the orchestrator's ruling", this is that ruling.

- [ ] **Trims — take candidates 4, 5, 6, 7, 8, 9, 10 and 11 (−91), in that order; not 1, 2 or 3.**
  - Candidate 2 removes a build-contract instruction about how rows are read against the undecided seam.
  - Candidate 3 removes a table convention.
  - Candidate 1 rewrites the Surfaces table, which check 2's phase marks already edit.

  None of the three is clearly rule-free. Each trim still gets its meaning check, with the row that must still state it named in the Result note. With every ruling below taken, the body lands near 11,958, leaving headroom for the pre-lock round and the lock status line. If a trim fails its meaning check, skip it and take candidate 1 instead.
- [ ] **Objected rows a companion-only fix answers stay at needs-discussion** (the default and the round-3 and round-4 precedent): 94 aligned, 21 pre-alignment, 7 needs-discussion.
- [ ] **R2.6's qualified cite is editorial.** The sentence already names the device PRD's E22, so the label keeps the same owning document. R2.6 keeps its alignment.
- [ ] **E10 on the item detail (interface N4, +6): taken.** It is an index and Surfaces entry for a render R4.1 already makes (E10 over E9), so it is editorial.
- [ ] **F175 has no Data Foundation row text.** R6.2a's deferral, as F184 generalizes it to any read begun before the wipe, already covers Save a copy and a re-read's checks. The bound on every other read the app makes lives in the ADR-0003 input (F175-4 and so on), as F139's did.
- [ ] **F187's OQ 20 carries the deferral by reference,** through its existing "As R6.2a–c and R6.3 state", with R6.3 stating it (0 words). DF words are scarce, and F187's "R6.3 and its OQ 20 carry" is met: OQ 20 carries it through R6.3.
- [ ] **F187 on the obligation lines: not added.** "As its F52–F57 record" and "F52–F57 list" carry it by fence.
- [ ] **F188's Data Foundation seam line: the tight form (+37).**
- [ ] **DF E33's ‹P1› mark: editorial phase bookkeeping.** Only Collection Mode's P1 R6.3 opens E33 (F7).
- [ ] **Export R4.3's held-export input (PERF5-2, +11 Export): made.** It is a testability grant, alignment kept under Export F33, and EJ1 and UJ9.5-d's overlap run hold the export through it.
- [ ] **ADR-0003's "its wipe never holding a write": added.** It is F157's "nothing waiting on it" stated for the mechanism, and PERF5-2's stall is what it prevents.
- [ ] **A new dated line under F102 recording F175 and F184: made.** It keeps the fence line 5N3 names in step with the rows.
- [ ] **UJ9.5-g is not split.** Its held runs take R5-m5's, R5-n1's, IF5-2's and F176-3's edits in place.
- [ ] **The round-4 fix file's line 566: wrap the quoted snippet in a code span** to clear check 11. Nothing else in that history file changes.
- [ ] **UJ10.1-d keeps 48 pt.** The storage read compares a number against a baseline read of the same storage, taken before the case's When. A match already in the baseline doesn't count, so a window-frame "48" can't false-match. Write this into the Harness's storage-read sentence (companion only).
- [ ] **The Device mirror of DF R1.11: a post-lock item, not an owner question now.**
  - DF R1.11's own words, "no … other write to the file starts", already hold a saved-device write that ADR-0003 puts in the file.
  - The device PRD's citing line is bookkeeping that waits on ADR-0003's placement.
  - Add it to `docs/product/post-lock.md` under the ADR-0003 trigger, in that file's style: "when ADR-0003 puts saved devices in the file, the device PRD's R1.9/R1.17 writes cite Data Foundation R1.11".
  - The orchestrator reports this to the owner.

## Status bookkeeping

- [ ] **Flip to aligned** (the log's round-5 flip record, less the holds and rewrites below) — in the PRD's tables and
      the copy file's Status lines. The record lists 104 IDs; 8 are lettered sub-rows that hold with an objected lead
      and 2 are rewritten, so **94 read aligned**. **13 newly:** R2.11, R4.1, R4.2, R4.2a–h, R4.3, R8.3. **81 stay
      aligned:** E1, E2, E3, E4, E5, E7, E9, E11, E12, E13, E14, E15, E16, E17, E18, E19, M2, M3, M4, R1.1, R1.2,
      R1.4–R1.10, R2.1–R2.10 and R2.4a–i, R3.1–R3.5, R3.7–R3.9, R4.4, R4.5, R4.8, R4.9, R5.1–R5.8 and R5.2a–e,
      R6.1–R6.4, R8.2, R8.4, R8.5, R8.7, R8.9. Editorial edits keep alignment: check 13's quote marks on R1.10 and R8.2,
      check 10's semicolons on M2–M4. If a box rewrites one of the 94 after all, it lands at pre-alignment instead and
      the Result says so. Copy-file Status lines: E1–E5, E7, E9 and E11–E19 stay aligned; no copy state flips.
- [ ] **Sub-rows hold with their lead — flag to the orchestrator.** The record also lists R8.1a, R8.1b, R8.1d, R8.1e,
      R8.1g (5) and R8.10b, R8.10c, R8.10f (3). R8.1, R8.1c and R8.1f, and R8.10, R8.10a, R8.10d and R8.10e are objected,
      and each family is rewritten this pass (R8.1f by interface N1; R8.10a by 5N2, R8.10f by L20-1), so both families
      read pre-alignment. R2.4, R4.2 and R5.2 flip whole.
- [ ] **Aligned rows now objected or rewritten — flag to the orchestrator.** Five aligned rows move back: R1.3 and R4.6
      to pre-alignment (check 1c's cites name another PRD as owner — not editorial under the format's rule), and R3.6,
      R7.1 and R7.2 to needs-discussion (R5-B1's objection, answered in the companions only; round 3's R5.1 is the
      precedent). R2.6 moves too if the orchestrator rules its cite non-editorial.
- [ ] **Rows a round-5 fix rewrites → pre-alignment:** R1.3, R4.6 (L1c-1); R8.1 and R8.1a–g (R8.1f, interface N1; the
      lead objected); R8.8 (IF5-4); R8.10 and R8.10a–f (R8.10a, 5N2 and F185-1; R8.10f, L20-1); E6 (F179-1), E8
      (F178-1), E10 (F181-1; interface N4 if taken) — **21**. Copy-file Status lines: E6, E8 and E10 stay pre-alignment.
- [ ] **Rows objected and left unchanged → needs-discussion** (the Legend: an open objection), each named with the
      companion-only fix that answers it, so the pre-lock round can flip it once the lens re-checks:
  - R3.6, R7.1, R7.2 — R5-B1's decoded storage read and UJ9.8-c's storage controls; UJ10.1-d's declared size for R7.1
    and R7.2.
  - R4.7 — R5-B1 (storage), R5-m2 (UJ9.7-i's two undos), R5-m3 (UJ6.4-q's precondition read); moves from
    pre-alignment.
  - R8.6 — R5-B1 and PRIV5-2 (storage), PRIV5-3 (SyncedPreferences, UJ9.4-e's entitlements).
  - R8.11 — R5-m7 and PERF5-2 (UJ9.5-d's overlap run).
  - M1 — R5-M1 (UJ9.5-a); check 10's semicolons are editorial, and check 20's fix lands in R8.10f.
- [ ] **After the pass:** report the final lists; the record's 104 flip-eligible and 18 objected reconcile to the PRD's
      122. Expected: **94 aligned** (81 staying, 13 new); **21 pre-alignment** (R1.3, R4.6, R8.8, E6, E8, E10, and the
      R8.1 and R8.10 families, 8 and 7); **7 needs-discussion** (R3.6, R4.7, R7.1, R7.2, R8.6, R8.11, M1). If R2.6 is
      ruled non-editorial: 93, 22, 7. The Open-questions table's statuses do not change (OQ 8 and 9 aligned). No new
      row, state, variant or metric enters this PRD; new cases only (UJ7.1-s, and UJ9.5-h if the split is ruled in).
      No new sibling row or state enters either: DF E33 gains its ‹P1› mark and stays ⌛️ Ready for Alignment; DF R1.11,
      R7.6q and E35 stay ⌛️ Ready for Alignment; DF R6.2a, R6.3, R7.3j and R7.6d, Capture R8.14, Import R4.1 and
      Export R1.1 (and R4.3 if ruled in) keep their alignment.

## peer-product-manager-reviewer (Claude route)

- [ ] **PM5-1 (Major) — DF R6.2a's "once that read ends or the file next opens" makes the next open a deadline while
      the read still runs, and DJ4's quit-and-reopen run asserts the wipe without ending the read** (DF R6.2a; DF F56
      (2); the ADR-0003 byte input; DF DJ4). Also answers ARCH5-1, PERF5-1 (a), PRIV5-1 and 5MN1 (ii); log row 2.
      **fix (F157, as its F173 line clarifies it: "the wipe follows the end of the read, on its own or at the first
      open after it" — editorial, restoring the fence's order):** F173-1 (R6.2a "once that read has ended and the file
      is open"), F184-2 (the ADR-0003 byte input, the same words), SIB-2 (DF F56's Corrected line), F173-2 (DJ4: the
      quit-and-reopen run ends the read before reopening, and a new line reopens with the read still running, the text
      still in the bytes and E35 up at the open). 0 Collection Mode body words.
- [ ] **PM5-2 (Minor) — E35's lifetime across several writes is unstated** (DF E35, R7.6q, R6.2a; UJ9.7-h; DJ4). Also
      answers 5MJ1 and IF5-1; log row 4. **fix (F173):** F173-1 (R6.2a: one E35 naming another app's read until the
      wipe lands, OK hiding it until another such write), F173-2 (DJ4's two-edit line, PM5-2's own), F173-3 (UJ9.7-h:
      OK, a second removing edit, the release). R7.6q is unchanged: it lists what the notice shows and its action, and
      R6.2a states when it shows.
- [ ] **PM5-3 (Minor) — R1.11's wait has no branch for a held write that fails** (DF R1.11; F156; DF DJ3; UJ9.5-g).
      **fix (F176):** F176-1, F176-2, F176-3.
- [ ] **PM5-4 (Minor) — "the same file" in E34's picker is undefined, and a refused pick is silent** (DF R7.3j; F159;
      DF DJ3; UJ9.7-i). Also answers 5MN3. **fix (F174):** F174-1 (R7.3j "for the file wherever it now is"; the owner
      chose the open file, so PM5-4's path reading "at E34's ⟨path⟩" is not taken), F174-2 (DJ3), F174-3 (UJ9.7-i run
      (d)), F174-4 (the dated line under DF F54). PM5-4's optional copy change — E34 saying what was picked isn't the
      file — is a copy meaning no fence states (F159 and F174 keep E34 as it is): reported, not made.
- [ ] **PM5-5 (Nit) — E6 "elsewhere" says "changes that touch many swatches at once" for an empty or one-swatch
      collection's undo** (copy E6; R8.3; UJ1.3-h). Also answers N5-m2. **fix (F179):** F179-1.
- [ ] **PM5-6 (Nit) — E10's "collection" variant at zero swatches names nothing once the zero rule drops its first
      sentence** (copy E10; the Placeholders rule; UJ1.3-h). **fix (F181):** F181-1, F181-2.
- [ ] *unrated* **PM Missing — the device PRD's saved-device writes are not mirrored under DF R1.11** (the device PRD;
      DF R1.11 and R6.4; DF's outbound table). Also the interface review's Missing item. **needs owner:** DF R1.11
      already holds any write the file stores ("no … other write to the file"), but no fence F1–F188 names a Device
      mirror — F155 names the Collection Mode, Import and Capture PRDs — so a Device row citing R1.11, or a DF→Device
      obligation line, is a sibling amendment no fence states. Round 4 reported it under IF4-1; the pass makes nothing.

The review's second biggest risk, the Data Foundation budget, is answered by F177; its request that the next overflow
go to the owner rather than a compaction is this list's DF stop rule.

## peer-staff-software-engineer-reviewer (Claude route)

- [ ] **5MJ1 (Major) — DF E35's lifecycle is stated only by a case** (DF R6.2a, R7.6q, E35; UJ9.7-h; DJ4). **covered by
      PM5-2** (F173: one notice, hidden by OK, back on a later removing write, gone when the wipe lands). Its fourth
      point, how soon the app notices the read's end: UJ9.7-h's and DJ4's 5 s stay functional timeouts, and no DF
      constant is added (PERF5-1's caution).
- [ ] **5MN1 (Minor) — R6.2a defers only for a read begun before the write, and promises a wipe at the next open while
      the read runs** (DF R6.2a; DJ4; R8.8; the ADR-0003 byte input). Its (i): **fix (F184):** F173-1 (R6.2a "while a
      read begun before its wipe runs"), F184-2 (the ADR byte input "before its wipe"), F184-3 (DJ4's second-read line).
      Its (ii): **covered by PM5-1.**
- [ ] **5MN2 (Minor) — the Harness times its open-app read by a bound the ADR-0003 input no longer states, and gives the
      after-a-crash read none** (the Harness's Byte checks; the ADR-0003 input; DF F56 (4)). Also answers ARCH5-2,
      PERF5-4, interface N3, R5-m1 and carried ARCH4-2 (c); log row 3. **fix (F175, restoring F139's bound as F158
      left it):** F175-2 (the ADR-0003 input: every read this app makes but an export, Save a copy and a re-read's
      checks), F175-3 (the Harness: at the open-app moment and before the after-a-crash crash the check waits
      BROWSE_RESPONSE_BUDGET after the write lands; a case running one of those three long reads reads once it ends),
      SIB-2 (DF F56's Corrected line, its (4)). 0 body words.
- [ ] **5MN3 (Minor) — R7.3j's "another file chosen" needs a definition of "the same file"** (DF R7.3j, R7.6p; DJ3;
      UJ9.7-i). **covered by PM5-4** (F174); its "moved or not" is F174-1's "wherever it now is".
- [ ] **5N1 (Nit) — Export R1.1's "identical" contradicts its own mid-export edit clause** (Export R1.1). **fix (F158 —
      editorial: removes the contradiction F32's clause introduced; no new WHAT):** R1.1 "…and leaves every source
      value, mark and note identical at SQLITE_READER_FLOOR after success or failure (R4.3)." → "…and itself changes
      no source value, mark or note, as read at SQLITE_READER_FLOOR after success or failure (R4.3)." (+2 Export).
      Alignment kept, its Commit PR cell adding "(F33)"; recorded in SIB-5.
- [ ] **5N2 (Nit) — R8.10a's held input is wider than DF R1.11's list, and "move" is undefined here** (R8.10a; the
      Test-controls map's "the file" line). Also answers interface N2. **fix (F155, F164, F185 — testability,
      word-neutral):** F185-1.
- [ ] **5N3 (Nit) — F102's dated F157 line names only another app's read and an export** (fences, F102). **fix (F175,
      F184 — fence bookkeeping, on the orchestrator's ruling):** F175-5, a new dated line under F102; the existing line
      records F157 as the owner decided it and stays.

The review's eleven clarifying questions are answered: 1–3 by F173; 4 by the functional timeouts (5MJ1); 5 by F184; 6
by F157's line as F173 clarifies it (PM5-1: the text stays and E35 is up at that open); 7 by F174; 8 by F175; 9 by
5MN2's Harness text (the crash comes no sooner than BROWSE_RESPONSE_BUDGET after the write lands); 10 by 5N1; 11 by
5N2. Its Deferred items are carried by boxes above or below: architecture — 5MN1's mechanism is behaviour for ADR-0003
(F157, F184), and a deferred ROWS_CEILING wipe run after a hold lifts, beside a session started meanwhile, is answered
by the ADR-0003 input's "never holding a write" if ruled in (F175-2), otherwise reported; performance — the deferred
checkpoint's cost (PERF5-2) and the polling cadence (functional timeouts, no constant); test — 5MN2's crash timing
(5MN2) and UJ9.7-h's moments (R5-M2); product and marketing — one E35 per write (F173), the silent pick (F174, PM5-4),
F157-9's finality copy (F178; DF E8, E14 and E33 unchanged); Import E40's "imports aren't available in any collection"
when only the commit is refused is the wording the owner approved as F169, which no lens raised this round — reported,
not changed; privacy — removed text across a quit (PM5-1).

## peer-test-reviewer (Claude route)

- [ ] **R5-B1 (Blocker) — the app-storage read matches whole tokens only, so a binary property list's or keyed
      archive's marker letter, or an SQLite row's packed columns, hide every short string** (the Harness's Byte checks
      and storage list; the Test-controls map's app-storage line; UJ9.8-c; UJ10.1-d; R3.6, R4.7, R7.1, R7.2, R8.6,
      R8.10e). Also answers PRIV5-2 and carried R4-B1; log row 1. **fix (F35, F53, F102, F130 — testability; companion
      only, 0 body words):**
  - In the Harness's **Byte checks** paragraph, replace the two sentences from "A read of the app's own storage for a
    text (unified-log entries read decoded) exempts nothing: …" through "…or the case relaunches with window
    restoration on and reads R8.10b." with exactly:

    > A read of the app's own storage for a text exempts nothing, and it decodes before it matches: each property
    > list or keyed archive — the app's Preferences, Saved Application State's data.data decrypted with the key in
    > windows.plist beside it, and any other such file — is read as each string value it holds and each number value
    > as its decimal text; each SQLite database among those paths is read as the SQL read reads the file, every table,
    > its internal tables and any full-text index's vocabulary included; unified-log entries are read decoded; and any
    > other file is read as its raw contents. The read finds no match for the text, or for any word of it of four or
    > more letters, as a whole token — bounded on each side by the start or end of a decoded value or of the raw
    > contents, or by a character that is not a letter or digit — in any letter case or normalised form, in either
    > encoding. A case may also relaunch with window restoration on and read R8.10b, beside the decoded read, never in
    > place of it.

    "UJ9.8-c is the control this check must catch, its SQL-read run included." follows, reading "…its SQL-read run and
    its storage runs included." Whole tokens stay the rule for crash reports and logs, where "blue" in "CoreBluetooth"
    is the false match to avoid.
  - **UJ9.8-c.** Its Given adds, after "…prefix-compressed so the copy's raw bytes hold warm nowhere": "; and, in the
    app's own storage, the word warm alone as a string value in a binary property list among its Preferences; a row of
    three adjacent text columns holding ZX-010, warm grey 1 and Grey in an SQLite database under its Caches; and the
    word warm as a prefix-compressed full-text vocabulary term after war in that database". Its When reads "Run the
    Harness's byte check for the text Warm Grey 1 against each, the storage controls through its read of the app's own
    storage". Its Assert reads "The check reports a match in every run: in the file's fourth run from the SQL read
    alone, and in each storage run from the decoded read, the raw bytes there holding warm as no whole token". Rows
    unchanged (R8.10).
  - **UJ10.1-d** declares its size, so the size has a text: Given "…shown as a grid at a swatch size of 48 pt" (see the
    ruling on the value); Assert "Neither read holds the grid choice or the swatch size's text 48, and after reopening
    Studio Markers shows as the table".
  - **The map's app-storage line:** "Reading it with the app closed, open or after a crash, for a text by the Harness's
    byte check, as whole tokens, exempting nothing" → "Reading it with the app closed, open or after a crash, for a
    text by the Harness's byte check, decoded and as whole tokens within each decoded value, exempting nothing, and a
    control seeded in it for that check".
- [ ] **R5-M1 (Major) — UJ9.5-a can no longer deliver its 200 drags: its 200 capture saves use up its 200 pending rows,
      and the session spans the first choice of Scale after launch** (UJ9.5-a; M1; R2.9). Log row 6. **fix (F41, F129 —
      testability):** UJ9.5-a's Given "…as ZX-013 does, and 200 of its items pending; a bulk session on Scale on the
      Demo Device; a Release build…" → "…as ZX-013 does, and 400 of its items pending; the Demo Device connected; a
      Release build…"; its When "…have the Demo Device save 200 sets into Scale while it is searched, filtered and
      view-sorted, then end that session (the capture PRD's R11.6); …" → "…bring a bulk session on Scale on the Demo
      Device in flight (the capture PRD's R11.6), have it save 200 sets into Scale while it is searched, filtered and
      view-sorted, then end it; …". The session now starts after the twenty choices of Scale, the first after launch.
- [ ] **R5-M2 (Major) — an outside read opened read-write checkpoints the WAL itself when it closes, so UJ9.7-h's
      second run and DJ4 cannot catch a build that never wipes at the next open** (UJ9.7-h; DF DJ4; the map's "the
      file" line). Also answers ARCH5-N3; log row 7. **fix (F157, F164 — testability):** UJ9.7-h's Given "a read held
      open on the file from outside the app (R8.10a), begun before the When" → "a read transaction held open on the file
      from outside the app through a read-only connection at SQLITE_READER_FLOOR, having read a page, begun before the
      When (R8.10a)" (in F173-3's text); DJ4's line likewise (F173-2); the map's "the file" line reads "…and a read
      transaction held open on it from outside the app through a read-only connection, having read a page (R8.10a)".
      ARCH5-N3's own part is "having read a page": an idle connection or a raw file handle defers nothing.
- [ ] **R5-m1 (Minor) — the after-a-crash byte read has no lower bound on when the crash comes** (the Harness).
      **covered by 5MN2** (F175-3: the crash comes no sooner than BROWSE_RESPONSE_BUDGET after the write lands).
- [ ] **R5-m2 (Minor) — UJ9.7-i run (a)'s "Undo change still offered" proves nothing, the Magenta edit being itself an
      entry** (UJ9.7-i; R4.7). **fix (F159 — testability):** run (a) ends by firing "Undo change" twice, asserting
      ZX-003 reads Neon Magenta after the first and ZX-001 holds Family Blue and ZX-002 Green after the second (in
      F174-3's text, with PRIV5-4's storage reads).
- [ ] **R5-m3 (Minor) — UJ6.4-q passes without testing anything when the mark is saved at open, and "offered after
      the export" has no listing step** (UJ6.4-q; R4.7; F163). **fix (F163 — testability):** its When "…to a declared
      folder outside the case's folder; open ZX-017, then close its detail; …" → "…to a declared folder outside the
      case's folder, then list the actions offered; read a copy of the file by the Harness's SQL read; open ZX-017,
      then close its detail; …"; its Assert opens "The copy read before ZX-017 opens holds no archive-unavailable mark
      for ZX-017, or the case reports not exercised, never passed; …". Rows unchanged (R4.7).
- [ ] **R5-m4 (Minor) — E6 and E10 on the All items view have no case** (F167; E6, E10; the map's All items view
      line). **fix (F167 — testability):** a new case after UJ7.1-r:

  > | UJ7.1-s | The seeded file; R1.7 built | From the All items view open Studio Markers' ZX-010, fire "Delete swatch"
  > and the Data Foundation PRD's E8 delete action; bring a bulk session on Studio Markers in flight, in each in-flight
  > state; fire "Undo" on E10 | E10 renders with no variant, naming ZX-010 and Studio Markers, on the All items view,
  > which lists 14 items and no Studio Markers ZX-010; after "Undo", E6 renders with no variant, naming Studio Markers,
  > on the All items view, and it still lists 14 items | R1.7, R8.3, R1.10 |

  The map's All items view line adds "and the actions of E6 and E10" to its controlled input, and R1.7 and R8.3 to its
  Rows; F167's Carried-by and map line add UJ7.1-s. The UJ 7 preamble needs no change (UJ7.1-s runs where R1.9, R1.10
  and, by its Given, R1.7 have landed).
- [ ] **R5-m5 (Minor) — the inbound DF R1.11 → R8.1c rule has no case for an item detail's writes** (UJ9.5-g; R8.1c,
      R4.3). **fix (F155 — testability):** UJ9.5-g's first held run adds, after "list Scale Two's actions and try the
      capture PRD's start there;", "open a Scale Two item and list its detail's actions and editable fields;", and its
      Assert reads "…while the delete is held, Scale Two's write actions, that item detail's actions and editable fields,
      and each action tried show disabled, …".
- [ ] **R5-m6 (Minor) — Import R3.8i's "shown disabled while a DF R1.11 write runs" and DF E35's "never for this app's
      own export" have no case** (Import UJ 2.1; Export EJ1). **fix (F155, F157, F158 — testability):** Import UJ 2.1
      gains a line, after the held-commit line:

  > | The target holds an interrupted session (E40's target variant); a Collection Mode bulk write or delete on another
  > collection held running until released (Collection Mode R8.10a) | While it runs, fire E40's End that session; then
  > again once it lands | While it runs End that session shows disabled and no session ends; once it lands it is
  > offered, and confirming it follows Capture's ending path (F67, F68) | R3.8, R4.1 |

  and Export EJ1's mid-export line ends "…is in the file's bytes nowhere, and Data Foundation E35 does not render (R1.1,
  DF R6.2a)". Recorded in SIB-4 and SIB-5.
- [ ] **R5-m7 (Minor) — UJ9.5-d's second run may never overlap an export with the edits** (UJ9.5-d; R8.11). Also
      answers PERF5-2's UJ9.5-d half and carried PERF4-3 and PERF3-7. **fix (F139, F158 — testability):** the second
      run's "…and an export of another of UJ9.5-b's collections started at each set's first Demo sample, through
      "Export collection" and the Data Export PRD's E1, so that its one-snapshot read spans the edits and the saves" →
      "…and, at each set's last Demo sample while no export runs, an export of another of UJ9.5-b's collections started
      through "Export collection" and the Data Export PRD's E1 just before the keystroke, the header fire and the edit,
      the edit then setting the Family of one item of the exported collection, whether an export was running at each
      set's last sample being recorded"; its Assert adds "; in the second run each export's row for its edited item holds
      that item's Family from before the edit (the Data Export PRD's R1.1), and a run in which no export was running at
      any set's last sample reports not exercised, never passed". Rows unchanged (R8.11, R1.8).
- [ ] **R5-n1 (Nit) — UJ9.5-g checks the disabled state only under the test-build hold, and "the file then read" is
      ambiguous** (UJ9.5-g). **fix (F155, F156 — testability):** the unheld collection-delete timing run's Assert adds
      "at the first keystroke frame showing the delete's progress, Scale Two's "Rename collection" shows disabled" —
      "Rename collection" in place of R5-n1's "Set a field", which is P1 and absent in that run's phase (IF5-2's lesson);
      the third held run's "the file then read with the app closed" → "the original file then read with the app
      closed".
- [ ] **carried: R4-B1 (PARTIAL) — the whole-token clause misses strings in binary property lists** (the Harness).
      **covered by R5-B1.**

The review's Over-tested note (UJ6.4-q's export payload-cell assert) asks for nothing.

## peer-interface-reviewer (Claude route)

- [ ] **IF5-1 (Major) — when DF E35 goes away is stated in no row, but UJ9.7-h asserts it** (DF R6.2a, R7.6q, E35;
      UJ9.7-h; DJ4). **covered by PM5-2** (F173). Its DJ4 assertion ("once it ends, E35 is not up") is in F173-2.
- [ ] **IF5-2 (Major) — a P0 case fires the P1 "Set a field", in UJ9.5-g's second held run and in Import's UJ 2.1
      line** (UJ9.5-g; Import UJ 2.1). Log row 5. **fix (F155, F164 — testability):** UJ9.5-g's second held run "select
      two of Scale Three's items and fire "Set a field", try the capture PRD's start…" → "fire "Rename collection" on
      Scale Three, try the capture PRD's start…", its Assert "…"Set a field", the start and the end action show
      disabled…" → "…"Rename collection", the start and the end action show disabled…"; Import UJ 2.1's held-commit line
      "…Collection Mode's Set a field…" → "…Collection Mode's Rename collection…". Recorded in SIB-4.
- [ ] **IF5-3 (Minor) — E8's "What was there isn't kept in your file" against DF E35's "stays in your file"** (copy
      E8; DF E35). Also answers N5-m1. **fix (F178):** F178-1.
- [ ] **IF5-4 (Minor) — R8.8's "unless an earlier read defers it" can bind "it" to the write** (R8.8). **fix (F157 —
      editorial: the wipe waits, never the write):** "…unless an earlier read defers it (…)" → "…unless an earlier read
      defers its wipe (…)" (+1, paid for by N1). R8.8 reads pre-alignment.
- [ ] **N1 (Nit) — R8.1f's "the Data Foundation PRD's R1.11 holding other writes and sessions" is a partial paraphrase**
      (R8.1f). **fix (F155 — editorial):** → "the Data Foundation PRD's R1.11 applying" (−4). Meaning check: DF R1.11
      states the writes and starts it holds, the re-read, and the wait of close, switch and quit. The R8.1 family reads
      pre-alignment.
- [ ] **N2 (Nit) — R8.10a's "a bulk write, delete, import commit or move"** (R8.10a; the map). **covered by 5N2.**
- [ ] **N3 (Nit) — the Harness's "the bound the ADR-0003 input puts on this app's own reads" is too broad** (the
      Harness). **covered by 5MN2** (F175-3 names the bound F175 sets).
- [ ] **N4 (Nit) — E10's index row and Surfaces entry omit the item detail, where R4.1 puts E10 over E9** (the Surfaces
      preamble and table; the copy index's E10 row; R4.1). **fix (editorial — the index follows R4.1 and E9's
      surfaces; no new WHAT), on the orchestrator's ruling, with a trim:** the preamble "…and E10 by the collection
      list, the collection surface and the All items view." → "…and E10 by the collection list, the collection surface,
      the All items view and the item detail." (+3); the Surfaces table's item detail row adds E10 (+1); the copy index's
      E10 Surface "Collection list; collection surface; All items view" → "…; item detail" (+2); the map's item detail
      line adds E10 to its observable. E10 is at pre-alignment already (F181). If not ruled in: reported.
- [ ] **N5 (Nit) — Export's inbound Collection Mode line cites a fence where every other line cites a row** (Export's
      inbound line). **fix (editorial):** "([its F158])" → "([its R8.8], F158)" (+1 Export); recorded in SIB-5.
- [ ] *unrated* **interface Missing — the Device Management half of R1.11** — **covered by** PM's unrated box (needs
      owner).
- [ ] *unrated* **interface Over-engineered — split UJ9.5-g's held runs into a new UJ9.5-h** (UJ9.5-g; the UJ 9
      preamble; F155, F156 and F164 Carried-by). **fix (testability — no new WHAT), optional, on the orchestrator's
      ruling:** the three held runs move, as edited above, into UJ9.5-h (Given: Scale, Scale Two, Scale Three and the
      CSV as UJ9.5-g declares them, a test build; Rows R8.1), UJ9.5-g keeping its timing runs; the UJ 9 preamble's
      "UJ9.5-e to UJ9.5-g" → "UJ9.5-e to UJ9.5-h"; F155's, F156's, F164's and F176's Carried-by and map lines add
      UJ9.5-h; the sibling cites at the end of the Assert move with the held runs. If not ruled in: reported.
- [ ] **carried: IF-16 (UNRESOLVED; deferred by design) — `docs/product/README.md` line 18 still reads "queued"** (the
      product README). **fix (F56 — editorial), at the bookkeeping close**, as rounds 2–4 hold; nothing in this pass.

## peer-privacy-reviewer (Claude route)

- [ ] **PRIV5-1 (High) — "the file next opens" became a deadline while the read still runs** (DF R6.2a; DJ4; the ADR
      byte input; DF F56 (2); F102's dated line; R8.8). **covered by PM5-1.** Its own parts: the reopen during the read
      keeps the deferral with E35 up again (F173-2's reopen line); the F102 and F56 lines say "any read begun before
      it, this app's own included" (F175-5, SIB-2).
- [ ] **PRIV5-2 (Medium) — the whole-token storage match cannot see short strings in binary property lists or text
      packed into SQLite rows** (the Harness; the map; UJ9.8-c; UJ3.3-f, UJ7.1-r, UJ9.4-b, UJ6.4-j, UJ6.4-k). **covered
      by R5-B1.** Its own parts are in R5-B1's text: decoding is the Saved Application State route and a relaunch only
      an addition; the SQLite-row and prefix-compressed storage controls.
- [ ] **PRIV5-3 (Low) — storage a system daemon writes, or syncs off the machine, for the app is not observed** (the
      Harness's storage list; the map's app-storage and network lines; UJ9.4-e; R8.6). **fix (F52, F130 — testability;
      R8.6's "nothing it reads or writes leaves the machine"):** the Harness's unsandboxed list "…Preferences, Saved
      Application State and ~/Library/Logs entries…" → "…Preferences, Saved Application State, ~/Library/SyncedPreferences
      and ~/Library/Logs entries…", and the map's app-storage line likewise; UJ9.4-e's When adds "; read the Release
      build's entitlements through codesign", its Assert adds "; the Release build holds none of the iCloud key-value
      store, iCloud container and CloudKit entitlements", its Rows add R8.6; the map's network line adds "and the
      entitlements codesign lists" to its observable.
- [ ] **PRIV5-4 (Low) — nothing checks where the unsaved change behind E34 is held** (UJ9.7-i; DF R1.1). **fix (F53 —
      testability):** UJ9.7-i's runs (a) and (b) read the app's own storage while its E34 is up and again after
      quitting, asserting neither read holds the text Magenta (in F174-3's text).
- [ ] **PRIV5-5 (Info) — four scope notes** (the Harness; DF R1.5; the Vocabulary). (a) **fix (F53 — testability):**
      the trap sentence covers the crashes UJ6.4-k and UJ7.1-r make (in F175-3's text). (b) **fix (F53, F130 —
      testability; R8.10e's "everything the app keeps"):** the Harness's storage list "…as a file-activity trace records
      them, the file's bytes aside" → "…the file's bytes, and any export or copy the user chose to write, aside". (c) The
      app's own long reads defer a wipe silently: F175 settles it (no notice); a help-docs line naming them is not fenced
      — reported, not made. (d) The deleted state's "for good" holds by its cite of the Data Foundation PRD's R6.2a —
      reported, no change.

## peer-product-marketing-manager-reviewer (Claude route)

- [ ] **N5-m1 (Minor) — E8 says the replaced values aren't kept in the file; E35 then says they are** (copy E8; DF
      E35). **covered by IF5-3** (F178).
- [ ] **N5-m2 (Minor) — E6 "elsewhere" gives a reason that doesn't fit an empty collection's undo** (copy E6).
      **covered by PM5-5** (F179).
- [ ] **N5-n1 (Nit) — E35's "What it removed" fits a clear or a delete, not a rename** (DF E35). **fix (F180):** F180-1.

The review's four Missing pointers ask for no change: a different file picked in E34's picker changing nothing
silently is settled by the owner (F159, F174; PM5-4's box) and left to a dogfood observation; F158's one snapshot
going unmentioned in copy proposes nothing and no fence states such copy — reported; DF E9's "always safe" still holds
for the file's integrity, E35 covering the moment; E35's presentation is the build's, and F173 settles whether it comes
back after OK.

## peer-architecture-reviewer (Claude route)

- [ ] **ARCH5-1 (Major) — "once that read ends or the file next opens" promises a wipe SQLite cannot perform** (DF
      R6.2a; DJ4; the ADR byte input; DF F56 (2)). **covered by PM5-1.** Its DJ4 reopen line is F173-2's.
- [ ] **ARCH5-2 (Major) — the bound on this app's own reads was narrowed to cold loads without a fence** (the ADR-0003
      input; DF F56 (4); the Harness). **covered by 5MN2**; its owner check is F175 (Save a copy and a re-read's checks
      defer like an export).
- [ ] **ARCH5-3 (Minor) — DF R1.11 has no rule for a write the app makes by itself during a hold** (DF R1.11; DJ3).
      **fix (F182):** F182-1, F182-2.
- [ ] **ARCH5-4 (Minor) — F157's non-waiting save holds only on a local volume** (the ADR-0003 input; DF R1.5's help
      docs). **fix (F183):** F183-1, F183-2.
- [ ] **ARCH5-N1 (Nit) — DF's inbound Collection Mode line names E15 in its wording but not in its Rows** (DF's inbound
      line). **fix (F166 — editorial):** its Rows add E15, linked as the others (+1 DF); recorded in SIB-1.
- [ ] **ARCH5-N2 (Nit) — R8.10a holds other documents' operations for their tests** (R8.10a; DF R7.2). **fix
      (F185):** F185-1 (the grant stays), F185-2 (the post-lock item).
- [ ] **ARCH5-N3 (Nit) — "a read held open from outside the app" does not say what kind of read** (R8.10a; UJ9.7-h;
      DJ4; the map). **covered by R5-M2**; its "having read a page" is in R5-M2's text.
- [ ] **carried: ARCH4-2 (PARTIAL) — its (c), the bound on this app's own reads, regressed to cold loads** (the
      ADR-0003 input). **covered by 5MN2** (F175).

## peer-performance-reviewer (Claude route)

- [ ] **PERF5-1 (Major) — the new DJ4 line cannot be passed by a correct build: the reopen run never ends the read,
      and "once it ends" has no tolerance** (DF DJ4; DF R6.2a). **covered by PM5-1.** Its (b), "within 5 s of its end,
      a functional timeout", is in F173-2's lines; its caution stands — no DF latency constant.
- [ ] **PERF5-2 (Minor) — nothing reliably overlaps a read, a removing edit and a capture save** (UJ9.5-d; Export EJ1;
      R8.11; the ADR-0003 input). UJ9.5-d: **covered by R5-m7.** Its own parts: (1) **fix (F158 — testability seam
      grant), on the orchestrator's ruling:** Export R4.3's first sentence ends "…and that no file went missing
      ([R3.1]), and in a test build holds an export running until released." (+11 Export), and EJ1's mid-export line's
      Given reads "An export of a collection held running until released (R4.3); one of its items edited while it is
      held", recorded in SIB-5; (2) **fix (F157's "no other write waits"), on the orchestrator's ruling:** the ADR-0003
      input's wipe "never holding a write" (in F175-2's text).
- [ ] **PERF5-3 (Minor) — R1.11's "its progress showing" has no surface that lists a move's or an import commit's
      progress** (DF R1.11, R7.6d; Import R4.1; DJ3). **fix (F156 — testability: the progress F156 decided made
      listable; no new WHAT):** DF R7.6d's "What the test lists" cell "…the actions, each move failure named" → "…the
      actions, a move's progress, each move failure named" (+3 DF); Import R4.1 "…the actions offered in every import
      state, including all three E40 routes." → "…including all three E40 routes, and a commit's progress."; DJ3's
      second held line's state cell "Write progress, then the target's opening state or none" → "Write progress (R7.6d
      for a move, Import R4.1 for a commit, Collection Mode R8.10b for its writes), then the target's opening state or
      none". Recorded in SIB-1 and SIB-4.
- [ ] **PERF5-4 (Nit) — the Harness's 100 ms grace rests on a bound the ADR input does not state** (the Harness).
      **covered by 5MN2.**
- [ ] **carried: PERF4-3 (PARTIAL) — UJ9.5-d's export finishes before the edit** (UJ9.5-d). **covered by R5-m7.**
- [ ] **carried: PERF3-7 (PARTIAL) — its overlap half** (UJ9.5-d). **covered by R5-m7.**

The review's cautions stand under the fences: no new Data Foundation latency constant (the 5 s timeouts are
functional), and no blocking wipe (the ADR-0003 input, if "never holding a write" is ruled in). Its fifth risk, WAL
growth under a long outside read, is pre-existing and no fence bounds it — reported, not made.

## Lock-check run 1 — MISS fixes

From `scratchpad/mech1/results.md` (subject `7975d26`'s `docs/product`): eight checks MISS (1, 2, 7, 10, 11, 13, 17,
20). One box per MISS item; each is marked editorial, testability or fence-authorized. The second run re-checks every
one after the pre-lock round's last edit.

- [ ] **L1a-1 — items 1–4: this PRD's outbound Capture and Import lines have no inbound counterpart, those two PRDs
      having no inbound table** (PRD:624, 625, 627, 628). **fix (F188 — the owner accepted this in place of adding
      tables):** no table is added; the pass verifies that each outbound line is carried on the sibling side by a row
      that cites this PRD or a dated fence — PRD:624 by Capture R5.6 and F70/F71; PRD:625 by Import R2.6 and F65;
      PRD:627 by Capture R5.6, R8.18, R1.1, R1.3, R1.5, R1.10, R3.5, R7.13, R8.5, R9.3, R3.7, R11.15g, the Set-aside
      cause labels table and OQ 13, under F70–F74; PRD:628 by Import R3.2 and R3.8i under F66 and F67 — and names any
      line that fails in its Result; SIB-3 and SIB-4 record the acceptance.
- [ ] **L1a-2 — item 5: this PRD's inbound Capture line cites R4.22, the capture PRD's own capture-surface indicator**
      (PRD:643). **fix (editorial — an index cell; F188):** "| Capture Mode | Its R4.22 and R8.14: …" → "| Capture Mode |
      Its R8.14: …", and its Rows "the capture PRD's R4.22, R8.14 → …" → "the capture PRD's R8.14 → …" (−3). R8.14
      carries the obligation (it cites the capture PRD's R4.24 for the non-spectral mark, which R2.4d cites directly).
- [ ] **L1a-3 — items 6–7: the capture PRD's Collection Mode line omits R4.8 and the shared-surface rows R6.7, R7.8,
      R7.15, R7.19, R8.7, R8.16 and R10.3** (Capture's outbound Collection Mode line). **fix (F188):** F188-2.
- [ ] **L1a-4 — items 8–16: DF's outbound Collection Mode line omits R2.3, R6.2, R6.3, E8, E14, E33, R2.4, R2.8, E11,
      E26, R5.5, E4, E31, R6.2a, R6.2d, R7.6g, R5.3, R1.4, R1.5, E9 and R1.1** (DF:289). **fix (F188):** F188-1.
- [ ] **L1a-5 — item 17: the import PRD's Collection Mode line omits R2.3** (Import's outbound line). **fix (F188):**
      F188-3.
- [ ] **L1a-6 — item 18: the Data Export PRD's outbound table has no Collection Mode line** (Export's outbound table).
      **fix (F188):** F188-4.
- [ ] **L1b-1 — item 1: Capture R8.14 is P1; the rows carrying it here are P0** (Capture R8.14). **fix (F186):**
      F186-1.
- [ ] **L1b-2 — item 2: DF E33 is not marked ‹P1›, though only Collection Mode's P1 R6.3 opens it** (DF copy E33 and
      header). **fix (editorial — phase bookkeeping under F7; on the orchestrator's ruling):** DF copy's E33 State cell
      "Delete selected swatches" → "‹P1› Delete selected swatches"; the copy header's "…or, where the sentence names it,
      another PRD's row, as [E8]'s, [E14]'s and [E33]'s full-history sentence waits on [the export PRD's R1.3] — …" →
      "…another PRD's row, as [E33] waits on [Collection Mode R6.3], and [E8]'s, [E14]'s and [E33]'s full-history
      sentence on [the export PRD's R1.3] — …", the new label linked to
      `../collection-mode/prd-collection-mode.md#6-selection-and-bulk-operations`. 0 DF body words; recorded in SIB-1.
- [ ] **L1b-3 — item 3: R1.7 carries F57's conditional; DF R6.3 and OQ 20 carry none** (DF R6.3, OQ 20). **fix
      (F187):** F187-1, F187-2.
- [ ] **L1c-1 — two bare IDs in the PRD name this PRD's own state where the row means a sibling's, and one names none**
      (R1.3, R4.6; R2.6). **fix (not editorial under the format's rule — each names another PRD as owner):** R1.3 "…and
      E1's action each return to editing it" → "…and that PRD's E1 action each return to editing it" (+2); R4.6 "…R8.3
      governing E4's restore in flight" → "…R8.3 governing that PRD's E4 restore in flight" (+2); both read
      pre-alignment. The note-class R2.6 "…and E22's action applies…" → "…and that PRD's E22 action applies…" (+2), on
      the orchestrator's ruling (see the rulings).
- [ ] **L1c-2 — bare sibling IDs in the journeys** (T12; UJ9.7-h; UJ9.7-i). **fix (editorial/testability — each
      Assert names the state it already means):** T12 "…E11 is not up…" → "…the Data Foundation PRD's E11 is not up…";
      UJ9.7-h "…within 5 s of the release E35 is not up…" → "…its E35 is not up…" (in F173-3's text); UJ9.7-i's three
      bare E34s → "that PRD's E34" in the When and "its E34" in the Assert (in F174-3's text). Changed Asserts go to the
      lens delta; no row status moves.
- [ ] **L2-1 — 14 `[phase: action-absent]` marks in the copy file have no counterpart in the PRD** (the Surfaces table:
      the collection surface, item detail and version history view rows). **fix (editorial — the phases follow from the
      rows' Pri on the record):** the three "Shows" cells become exactly, each label quoted with its own mark as the copy
      file writes it (check 13 reads labels quoted; the check 2 script matches a label followed by its mark):
  - Collection surface (+27): "One collection's item table with search, filters and sorts, one-row selection, "Select
    all" [phase: action-absent], "Set a field" [phase: action-absent], "Delete selected" [phase: action-absent],
    reordering, "Use as scan order" [phase: action-absent], "Undo change" [phase: action-absent], "Columns" [phase:
    action-absent], "Rename column" [phase: action-absent], "Grid" [phase: action-absent] and "Table" [phase:
    action-absent], and the entries to rename, delete and export the collection, beside every entry point and state the
    capture PRD places there".
  - Item detail (+2): "R4.2's lines, its editable fields and its actions, "Find similar" [phase: action-absent],
    "Change code" [phase: action-absent] and "Undo change" [phase: action-absent] among them".
  - Version history view (+6): "Every reading one item holds, in recorded or measured order, with R5.2's lines, "Use
    this reading" [phase: action-absent] and "Compare" [phase: action-absent]".

  Meaning check: "— or, last, its swatch grid —" is carried by "Grid" and the Swatch grid row; "selection and bulk
  actions" by "one-row selection" and the three bulk labels; "undo" by "Undo change" and E10 in the state list; the
  item detail's line list by R4.2a–h; "each reading's facts and marks" by R5.2a–e. The drag, P1 too, keeps its
  unmarked "reordering": the copy file carries no drag label to pair. The marks cost 2 words each; this form adds no
  row text, so no row's status moves.
- [ ] **L7-1 — four constants list R8.1 as their owner, but only a lettered sub-row names them** (the constants
      table). **fix (editorial — index; word-neutral):** Owning rows: OPEN_COLLECTION_BUDGET "R8.1, R8.2" → "R8.1d,
      R8.2"; DROPPED_FRAME_SHARE "R8.1" → "R8.1e"; BULK_WRITE_BUDGET "R8.1" → "R8.1f"; DELETE_WRITE_BUDGET "R8.1" →
      "R8.1g". Each named row names its constant.
- [ ] **L10-1 — M1–M4's Definition cells have four sentences each** (the metrics table). **fix (editorial —
      punctuation; word-neutral):** ". End:" → "; end:", ". Statistic:" → "; statistic:", ". Population:" → ";
      population:" in each, as the format's example writes its metrics. M2–M4 keep their alignment.
- [ ] **L11-1 — one broken internal link, in the round-4 fix file's line 566** (`[R7.3b](#file-actions)`, a quoted DF
      snippet). **fix (editorial), on the orchestrator's ruling:** wrap that quoted markdown in a code span, so it reads
      as quoted text, not a link; nothing else in that record changes. This list writes no link targets for the same
      reason.
- [ ] **L13-1 — action labels written unquoted where a builder reads them** (rows R1.10, R7.1, R8.2; cases UJ1.3-d,
      UJ4.4-e, UJ6.3-d, UJ9.4-c, UJ9.5-b; prose PRD:84, 280, 299, 710, 715, 717, 754, 762 and journeys:96). **fix
      (editorial — typography; word-neutral):** quote each: "Find similar" in R1.10, R8.2, UJ9.4-c, UJ9.5-b and the
      prose sites; "the Grid or Table choice" → "the "Grid" or "Table" choice" in R7.1; "after Undo" → "after "Undo"" in
      UJ1.3-d, UJ4.4-e and UJ6.3-d. R1.10 and R8.2 keep alignment (typography). The disposed sites ("Close the file",
      "Close Pair", "All items shown", "Undo of a delete", "From All items") stay.
- [ ] **L17-1 — UJ9.5-c asserts "read back unchanged"** (UJ9.5-c). **fix (testability — values, not adjectives):** its
      Given "…one item holding a value of 201 characters, another a value of 10,000, …" → "…one item holding a value of
      201 characters, the letter a repeated, another a value of 10,000 characters, the digits 0123456789 repeated 1,000
      times, …"; its Assert "…both long values read back unchanged…" → "…the first long value reads back as 201 letters
      a and the second as 0123456789 repeated 1,000 times, 10,000 characters…". A changed Assert goes to the lens delta.
- [ ] **L20-1 — M1 times the R2.5 gamut change, but R8.10f states only R8.1 inputs** (R8.10f; UJ9.5-a). **fix (F41,
      F112 — the verification seam; testability, process rule 3):** R8.10f "The time from each R8.1 input to the first
      frame showing its result, …" → "The time from each R8.1 input or R2.5 gamut change to the first frame showing its
      result, …" (+4); UJ9.5-a's Rows "R8.1, M1" → "R8.1, R2.5, M1". M1 is unchanged. The R8.10 family reads
      pre-alignment.
- [ ] **Lock-time items — deferred to the lock pass, not this one.** The status line (`Status: draft` →
      `Status: locked (<date>)`, +1 body word); deleting the 42 guidance comments and the conditional template comment
      at PRD:633 (checks 15 and 16); rewriting "peer review pending" on the sibling status surfaces this change touches
      (Capture PRD:3 and its fences preamble, DF PRD:3, the device PRD:3 and its fences preamble, Export PRD:3, Import
      PRD:3) at the bookkeeping close; the product README's "queued" (IF-16); the PR number in post-lock.md's "number
      pending" tick (check 14); the fence preamble's first-lock "Resolved baselines" sentence; the Open-questions
      table's statuses at lock. None is done here.

## Owner decisions F173–F188 — the edits each requires

Next free IDs, verified by reading the files at `abbc2a0`: DF fences end at F56 (DF's own cites of "F57" all read
"Capture F57"), so the new Data Foundation fence is **DF F57**; Capture fences end at F74, so **Capture F75**; Import
fences end at F67, so **Import F68**; Export fences end at F32 ("F33" there reads "DF F33"), so **Export F33**. This
PRD's UJ 7 scenario 1 ends at UJ7.1-r, so the new case is **UJ7.1-s**; UJ9.5-h is free. No new requirement row or copy
state is needed in any document: DF R1.11, R6.2a, R6.3, R7.3j and R7.6d, DF E33 and E35, Capture R8.14 and Import R4.1
are amended in place.

### F173 — DF E35 is one notice that goes by itself

- [ ] **F173-1 — DF R6.2a, one edit for F157's order (PM5-1), F184 and F173** (DF R6.2a's Oracle cell). "…from the
      moment the delete lands (or, while an earlier read still runs, once that read ends or the file next opens, nothing
      waiting on it and [E35] naming another app's), while the file is open and after a crash" →

  > …from the moment the delete lands (or, while a read begun before its wipe runs, once that read has ended and the
  > file is open, nothing waiting on it and one [E35] naming another app's until the wipe lands, OK hiding it until
  > another such write), while the file is open and after a crash

  (+15 DF: +1 order, +2 F184, +12 F173). "Once that read has ended and the file is open" is F173's line "on its own or
  at the first open after it": the app wipes at once if the file is open when the read ends, else at the next open,
  and a reopen while the read runs keeps the deferral. R2.3's "deferred as R6.2a states" and R6.2/R6.2d's "as R6.2a
  states" carry it. Alignment kept. R7.6q unchanged.
- [ ] **F173-2 — DF DJ4** (DF journeys). The existing line becomes:

  > | A read begun before a delete or a clear, made from outside the app through a read-only connection ([Collection
  > Mode R8.10a]), still running | Delete (or clear); then end the read — and separately quit, end the read, then
  > reopen | The delete lands and shows done without waiting on the read, nothing refused; E35 is up while the read
  > runs; within 5 s of the read's end, a functional timeout, or after reopening, the removed text is in the file's
  > bytes nowhere and E35 is not up; closing and quitting never wait (R6.2a, F56, F57) | E35 |

  and three lines follow it:

  > | That read still running across a quit and a reopen | Delete (or clear); quit and reopen; then end the read | At
  > the reopen the removed text is still in the file's bytes and E35 is up; within 5 s of the read's end it is in the
  > file's bytes nowhere and E35 is not up (R6.2a, F57) | E35 |

  > | A read made from outside the app still running | Make two removing edits in turn, firing E35's OK after the
  > first; then end the read | After the first edit one E35 is up; after OK none is; after the second one E35 is up
  > again; within 5 s of the read's end the text both edits removed is in the file's bytes nowhere and E35 is not up
  > (R6.2a, F57) | E35 |

  F184-3 and F175-4 add two more.
- [ ] **F173-3 — UJ9.7-h** (journeys; with R5-M2 and L1c-2). Given "The seeded file; a test build; a read transaction
      held open on the file from outside the app through a read-only connection at SQLITE_READER_FLOOR, having read a
      page, begun before the When (R8.10a)". When "Open ZX-001, set Swatch Name to Harbour and press Return; list the
      detail and the sibling state up; fire the Data Foundation PRD's E35 action and list the sibling state up; set
      ZX-014's Swatch Name to Moss, press Return and list it again; release the outside read; in a second run, quit
      after the Harbour edit instead of releasing the read, read the file with the app closed, then release the read and
      reopen". Assert "Within 5 s, a functional timeout (timing is UJ9.5-a's), the detail shows Harbour, no write's
      progress shows (R8.10b), none of the Data Foundation PRD's E10, E15 and E34 renders, and its E35 is up once; after
      its E35 action none of its states is up, and after the Moss edit its E35 is up once again; within 5 s of the
      release its E35 is not up and the file's bytes hold neither the text Sky Blue nor Plum; in the second run the app
      quits within 5 s, the file read with the app closed holds Harbour for ZX-001, and after reopening its bytes hold
      the text Sky Blue nowhere". Rows R8.8, R8.10. (Plum is ZX-014's seeded name, a text no other seeded value holds.)

### F174 — "The same file" in E34's picker is the open file, wherever it now is

- [ ] **F174-1 — DF R7.3j** (DF File actions). "…run R7.3d's file-selection path for the file, which restores access
      only, …" → "…run R7.3d's file-selection path for the file wherever it now is, which restores access only, …"
      (+4 DF). "Another file chosen" then covers a copy, as F174 states. Alignment kept.
- [ ] **F174-2 — DF DJ3** (DF journeys), after the same-file line:

  > | File open, the app's permission to it lost; the file then renamed and moved to another folder outside the app
  > (R7.2) | Change an item's metadata; choose Choose the file again and pick the file under its new name and folder;
  > then choose Try again | The file-selection path restores access to that file; E34 stays up and no write is retried
  > until Try again, which lands the change whole in it (R7.3j, F54) | E34 |

- [ ] **F174-3 — UJ9.7-i** (journeys; with R5-m2, PRIV5-4 and L1c-2). Given adds "; for run (d), another folder". When:
      "…fire the Data Foundation PRD's E34 choose-the-file action and, in separate runs: (a) pick the same file, declare
      the permission restored, then fire that PRD's E34 try-again action; then fire "Undo change" twice; (b) pick the
      copy; (c) as (a) up to its try-again, with a bulk session on Gouache Set brought in flight after the clear; (d)
      before the pick, rename the file and move it to that folder outside the app, then pick it there under its new name
      and go on as (a) up to its try-again; in (a) and (b), read the app's own storage while its E34 is up and again after
      quitting. The delete comes before the clear because …" (the note unchanged). Assert: "(a) its E34 stays up until
      the try-again, the file then holds Magenta for ZX-003, "Undo change" and E10's "Undo" are still offered and the
      search field holds zx-0, and after the first "Undo change" ZX-003 reads Neon Magenta and after the second ZX-001
      holds Family Blue and ZX-002 Green; (b) its E34 stays up, the collection list names Gouache Set and Studio Markers
      and no Spare, each file's bytes read before the pick and after it are identical, and both undos are still offered;
      (c) as (a) up to the try-again, and the Data Foundation PRD's E22 does not render; (d) as (a) up to the try-again,
      the file under its new name holding Magenta for ZX-003; in (a) and (b), neither read of the app's own storage holds
      the text Magenta". Rows unchanged (R8.8, R4.7, R1.7). The map's "the file" line adds "renamed or moved outside the
      app while open" to its controlled input.
- [ ] **F174-4 — a dated line under DF F54** (DF fences), after the F165 line: "**Clarified 2026-09-25 ([the Collection
      Mode PRD's F174], owner decision D43):** "the file" in R7.3j's file-selection path is the open file wherever it now
      is, renamed or moved included; a copy or any other file changes nothing, as the F159 line above settled. R7.3j
      says so and DJ3 asserts it; R7.3j keeps its alignment. Peer review pending." DF F54's map line adds "Clarified
      2026-09-25 (the Collection Mode PRD's F174): R7.3j; DJ3."

### F175 — The app's own long reads defer a wipe as an export does

- [ ] **F175-1 — the Data Foundation row text: none at its tightest.** R6.2a's "a read begun before its wipe"
      (F173-1) already includes R7.3h's copy and R7.3c's checks, and "[E35] naming another app's" already keeps them
      silent; the bound on every other read is the ADR-0003 input's, where F139 put it and F175 restores it (F175-2).
      If the orchestrator rules the bound into a row instead, the tightest R6.2a text inserts after "while a read begun
      before its wipe runs,": "this app's own ending within [Collection Mode BROWSE_RESPONSE_BUDGET] but an export, R7.3h's
      copy and R7.3c's checks," (+16 DF, which the DF ledger then carries).
- [ ] **F175-2 — the ADR-0003 row** (`docs/decisions/README.md`, the 0003 row; with F183 and F184). Three edits, still
      eight inputs:
  - the byte input "…while the file is open and after a crash — or, while a read begun before that write still runs,
    this app's own included, from when that read ends or the file next opens;" → "…while the file is open and after a
    crash — or, while a read begun before its wipe runs, this app's own included, from when that read has ended and the
    file is open;" (F157's order, F184);
  - the removing-write input "a text-removing write is saved without waiting on any read, only its wipe following the
    end of every earlier read, and this app's own cold-load reads (the All items view's) run in transactions short
    enough that the wipe follows within Collection Mode's BROWSE_RESPONSE_BUDGET, so capture saves never wait on a
    removing edit;" → "on a local volume (Data Foundation R1.10's scope), a text-removing write is saved without waiting
    on any read, only its wipe following the end of every read begun before it, never holding a write, and every read
    this app makes but an export, Save a copy and a re-read's checks runs in transactions short enough that the wipe
    follows within Collection Mode's BROWSE_RESPONSE_BUDGET, so capture saves never wait on a removing edit;" (F183,
    F184, F175; "never holding a write" on the orchestrator's ruling, PERF5-2);
  - the fence list "[its F35, F50, F85, F102, F103, F127, F139, F146, F157, F158 and F171]; [Data Foundation F52, F53,
    F54 and F56]" → "[its F35, F50, F85, F102, F103, F127, F139, F146, F157, F158, F171, F175, F183 and F184]; [Data
    Foundation F52, F53, F54, F56 and F57]".

  The 0007 row is unchanged.
- [ ] **F175-3 — the Harness's Byte checks** (journeys; with 5MN2, R5-m1, interface N3, PERF5-4 and PRIV5-5 (a)).
      Replace "At the open-app moment the check reads once BROWSE_RESPONSE_BUDGET has passed after the write lands — …
      reads at the moments its When names. For the app's own storage, the after-a-crash run's crash is made by an
      in-process trap once the case's text has been handled, so a crash report exists, and the read includes that
      report's application-specific information." with exactly:

  > At the open-app moment, and before the after-a-crash run's crash, the check waits BROWSE_RESPONSE_BUDGET after the
  > write lands — the bound the ADR-0003 input puts on every read this app makes but an export, Save a copy and a
  > re-read's checks, a read begun before a wipe deferring it (the Data Foundation PRD's R6.2a); a case running an
  > export, Save a copy or a re-read's checks reads once it ends, and a case declaring a read held open from outside the
  > app (R8.10a) reads at the moments its When names. For the app's own storage, the after-a-crash run's crash, and the
  > crash UJ6.4-k and UJ7.1-r each make, is made by an in-process trap once the case's text has been handled, so a crash
  > report exists, and the read includes that report's application-specific information.

- [ ] **F175-4 — DF DJ4** (DF journeys), after F173-2's lines:

  > | Save a copy (R7.3h), or in a second run Read it again's checks (R7.3c), running on R7.7's ROWS_CEILING corpus |
  > Delete (or clear) while it runs | The delete shows done without waiting; E35 does not render; within 5 s of the
  > copy's or checks' end the removed text is in the active file's bytes nowhere; a run whose read ended before the
  > delete landed reports not exercised (R6.2a, F57) | none |

- [ ] **F175-5 — a dated line under F102** (this fence file; 5N3, PRIV5-1; on the orchestrator's ruling), after its
      F157 line: "**Clarified 2026-09-25 (owner decision D44 and approved round-5 recommendation 7, F175 and F184):** the
      read that defers a wipe is any read begun before that wipe, another app's or this app's own; of this app's reads
      only an export, Save a copy and a re-read's checks may run longer than BROWSE_RESPONSE_BUDGET." F102's Carried-by
      is unchanged.

### F176 — A held write that fails cancels the close, switch or quit

- [ ] **F176-1 — DF R1.11** (DF §1; with F182-1). "…closing the file, switching ([R1.3]) and quitting wait for it, its
      progress showing." → "…closing the file, switching ([R1.3]) and quitting wait for it, its progress showing, a
      failure cancelling them and staying up." (+7 DF; the clearer "…, and a failure cancels them, leaving its state up."
      is +9). The whole row, with F182-1, then reads:

  > While a [Collection Mode R8.1f/g] write, an [Import R3.2] commit or an R1.9 move runs, no capture starts or resumes
  > and no re-read or other write to the file starts, each shown disabled, one the app makes itself deferred; closing
  > the file, switching ([R1.3]) and quitting wait for it, its progress showing, a failure cancelling them and staying
  > up.

  (+13 DF for both; one sentence.) ⌛️ Ready for Alignment, unchanged.
- [ ] **F176-2 — DF DJ3** (DF journeys), after the second held line:

  > | No capture in flight; each of those writes in turn held running until released, the volume declared full before
  > its release | In separate runs, quit, close the file and open another file while it runs; then release the write |
  > The write fails and its failure state comes up (E15; E19 for a move; Import E44's no-room variant for a commit); the
  > quit, close or switch is cancelled, the file stays open and that state stays up (R1.11, F57) | E15 / E19 / Import
  > E44 |

- [ ] **F176-3 — UJ9.5-g's third held run** (journeys). Its When adds a fourth sub-run: "; and, with the volume
      declared full before the release, quit, then release the delete"; its Assert adds "; in the fourth sub-run the
      delete fails, the Data Foundation PRD's E15 renders, the quit is cancelled, the app stays open with E15 up, and the
      original file read with the app closed holds every Scale item (the Data Foundation PRD's R1.11)". The map's "the
      file" line already declares a full volume.

### F177 — The Data Foundation PRD's word budget is 8,400

- [ ] **F177-1 — the budget lines** (DF fences). A dated line under DF F55: "**Clarified 2026-09-25 ([the Collection
      Mode PRD's F177], owner decision D46):** the budget is 8,400 words — a third owner override of the agent-PRD
      format's never-raise rule, recorded here, so the round-5 rule text and the seam line the lock checks require land
      without trimming; the body stood at 8,292 when it was raised." (the pass appends the post-pass count); a dated line
      under DF F21: "…the budget is 8,400 words; see F55."; F55's map line "(the word budget, 8,400 since 2026-09-25)";
      F1's map line "(F21 and F55, 8,400 since 2026-09-25)". F177's Carried-by here stays "governs no rows".

### F178 — E8's wording on what the file keeps

- [ ] **F178-1 — E8** (copy file; E8's Body and its "clear" variant). "What was there isn't kept in your file." → "Your
      file won't keep what was there." in both. E8 stays pre-alignment, rewritten. UJ6.2-b and UJ6.2-d read E8 by
      state and variant, not wording; unchanged.

### F179 — E6's "elsewhere" names whole-collection changes

- [ ] **F179-1 — E6** (copy file; the "elsewhere" variant). "Until that session ends, changes that touch many swatches
      at once aren't available in any collection, …" → "Until that session ends, changes to a whole collection or to
      many swatches at once aren't available in any collection, …". R8.3 unchanged. E6 stays pre-alignment.

### F180 — DF E35's wording

- [ ] **F180-1 — DF E35** (DF copy). Body "Your change is saved. What it removed stays in your file until the other app
      stops reading it; …" → "Your change is saved. What was there before stays in your file until the other app stops
      reading it; …". Status ⌛️ Ready for Alignment, unchanged. Copy honesty re-checked against R6.2a as F173-1 leaves
      it ("SpectroCapture then wipes it on its own, or when you next open the file" is its order); recorded in SIB-1.

### F181 — E10's "collection" variant at zero swatches

- [ ] **F181-1 — E10** (copy file; E10's note, after "…it carries no phase mark of its own."): "When the deleted
      collection held no swatch, the "collection" variant renders ⟨collection⟩ is deleted. as its first sentence, in
      place of the one the zero rule leaves out (F181)." The Placeholders rule stays as it is; E10 stays pre-alignment.
- [ ] **F181-2 — UJ1.3-h** (journeys). Its Assert adds, after "…on the collection list,": "E10 renders its "collection"
      variant naming Inks, with ⟨n⟩ 1, and in the second run naming Inks with no ⟨n⟩, the zero form F181 gives,". Rows
      unchanged (R1.7, R8.3).

### F182 — An app-made write during a hold is deferred

- [ ] **F182-1 — DF R1.11** (DF §1). "…no re-read or other write to the file starts, each shown disabled;" → "…each
      shown disabled, one the app makes itself deferred;" (+6 DF; "each shown disabled, the app's own deferred" is +4
      but reads as if a user-fired re-read could be queued). The whole row is F176-1's.
- [ ] **F182-2 — DF DJ3** (DF journeys), after F176-2's line:

  > | No capture in flight; a [Collection Mode bulk write or delete] held running until released (Collection Mode
  > R8.10a); an item whose sample archive is unreadable and not yet marked (R5.5a, R7.2) | Open that item's detail while
  > the write runs; then release the write | The detail opens without waiting on the write (Collection Mode R8.1b); the
  > file holds no archive-unavailable mark for the item while the write runs, and holds it once the write lands (R1.11,
  > R5.5a, F57) | E31 |

### F183 — ADR-0003's non-waiting save is scoped to a local volume

- [ ] **F183-1 — the ADR-0003 input** — in F175-2 ("on a local volume (Data Foundation R1.10's scope), …").
- [ ] **F183-2 — the post-lock item** (`docs/product/post-lock.md`, § Documentation, after its last DF item):
      "- [ ] **DF** — the help docs (R1.5's) say that on a network volume reading the file elsewhere may delay saves as
      well as the wipe of removed text, the non-waiting save holding only within R1.10's local-volume scope (Collection
      Mode F183, its round-5 architecture review's ARCH5-4; added in this change's PR, number pending)."

### F184 — A read that starts before a wipe also defers it

- [ ] **F184-1 — DF R6.2a** — in F173-1 ("while a read begun before its wipe runs").
- [ ] **F184-2 — the ADR-0003 byte input** — in F175-2.
- [ ] **F184-3 — DF DJ4** (DF journeys), after F173-2's lines:

  > | A read made from outside the app still running; a delete then landing; a second outside read begun after it
  > landed | End the first read; then end the second | While the second runs the removed text is still in the file's
  > bytes and E35 is up; within 5 s of its end the text is in the file's bytes nowhere and E35 is not up (R6.2a, F57) |
  > E35 |

### F185 — R8.10a's held-write grant stays here for now

- [ ] **F185-1 — R8.10a** (R8.10a; with 5N2 and interface N2). The grant stays; its wording follows DF R1.11's list:
      "…and in a test build a bulk write, delete, import commit or move held running until released, and a read held
      open on the file from outside the app" → "…and in a test build a write the Data Foundation PRD's R1.11 names held
      running until released, and a read held open on the file from outside the app" (word-neutral). The map's "the file"
      line follows ("a write the Data Foundation PRD's R1.11 names held running until released"). Capture T7, DF DJ3 and
      Import UJ 2.1 already name the writes they hold and cite R8.10a; unchanged. The R8.10 family reads pre-alignment.
- [ ] **F185-2 — the post-lock item** (`docs/product/post-lock.md`, § Next pass over a locked PRD › Data Foundation,
      after its last item): "- [ ] **DF** — Collection Mode R8.10a's held-write input (a write R1.11 names, held running
      until released) moves into R7.2 as a declared state once this PRD's budget has room, R8.10a then citing it
      (Collection Mode F185, its round-5 architecture review's ARCH5-N2; added in this change's PR, number pending)."

### F186 — Capture R8.14 moves to P0

- [ ] **F186-1 — Capture R8.14** (Capture §8). Pri "P1" → "P0"; Commit PR cell "amended 2026-09-25 (F75), alignment
      kept". The row's text is unchanged; its Legend's P1 list does not name it, so nothing else moves.
- [ ] **F186-2 — Capture F75, its map line, Traceability and status** — in SIB-3.

### F187 — The Data Foundation PRD mirrors F57

- [ ] **F187-1 — DF R6.3** (DF §6). "…; it does not create empty replacement rows." → "…; it does not create empty
      replacement rows, and it is deferred, v1's deletes final, if OQ 20 is still open at v1 release." (+16 DF; two
      sentences still). Alignment kept, as F187 authorizes.
- [ ] **F187-2 — DF OQ 20** (DF Open Questions). Its Decision so far "As R6.2a–c and R6.3 state." → "As R6.2a–c and
      R6.3 state, R6.3 deferred if this is open at v1 release." (+9 DF), as this PRD's OQ 10 carries F57; or, on the
      orchestrator's ruling, unchanged (its "R6.3 state" then carries it by reference).
- [ ] **F187-3 — the obligation lines, on the orchestrator's ruling** (this PRD's Outbound Data Foundation row; DF's
      inbound Collection Mode line). Default: not added. If ruled in: "…its OQ 20 sizing an undoable ceiling delete; …"
      → "…its OQ 20 sizing an undoable ceiling delete, and its R6.3 deferred as R1.7 is; …" (+7), and DF's "OQ 20 sizing
      an undoable ceiling delete" → "OQ 20 sizing an undoable ceiling delete, and R6.3 deferred as its R1.7 is" (+7 DF).

### F188 — The seam lines on both sides

- [ ] **F188-1 — DF's outbound Collection Mode line** (DF "What this PRD imposes on others"; L1a-4). Its Obligation
      cell's "…R6.1's two deletion rules; R1.11's one-writer rule; history reachable from an item without costing the
      primary view; …" → tight form (+35):

  > …R6.1's two deletion rules; R1.11's one-writer rule; history, restore, the correction question and its unanswered
  > set reachable from an item without costing the primary view; the delete confirmations, export first and P1 undo;
  > the damage states, byte rule, detail data lines and read-only floor; re-reading, no outbound traffic and nothing
  > kept elsewhere; …

  (the full form, +41: "…history, restore, the correction question and the unanswered set reachable from an item
  without costing the primary view; the counted delete confirmations, export first and the P1 undo; the damage states;
  the byte rule; the detail's data lines; the read-only floor; re-reading, no outbound traffic and nothing kept outside
  the file; …"). Its Rows cell "[R2.5], [R3.4], [R3.5], [R6.1], [R1.11]" → "[R1.1/R1.4/R1.5/R1.11],
  [R2.3/R2.4/R2.5/R2.8], [R3.4/R3.5], [R5.3/R5.5], [R6.1–R6.3/R6.2a/R6.2d], [R7.6g],
  [E4/E8/E9/E11/E14/E26/E31/E33]", in DF's slash style, each group linked to its first row's section and the E group
  to the copy file (+2; one link per row is +21). By reference instead (+14, on the orchestrator's ruling): "…history
  reachable from an item without costing the primary view; and what that PRD's inbound lines name, by the rows at
  right; …" with the same Rows. The line then names every row this PRD's inbound Data Foundation lines cite (R2.3,
  R2.4, R2.5, R2.8, R3.4, R3.5, R5.3, R5.5, R6.1, R6.2, R6.2a, R6.2d, R6.3, R7.6g, R1.1, R1.4, R1.5, R1.11 and E4, E8,
  E9, E11, E14, E26, E31, E33).
- [ ] **F188-2 — the capture PRD's Collection Mode obligation line** (Capture "Inherited obligations"; L1a-3). Its
      Obligation "Rename and delete a collection; …" → "Rename and delete a collection under the one naming rule; …",
      and before "; and the import-rooted obligations…" it adds "; keeping an in-flight session's trigger acknowledgement
      and row confirmation within TRIGGER_ACK_WINDOW and ROW_CONFIRM_BUDGET; the shared collection surface — identical
      row states, vocabulary and counts, this document's states never hiding its entry points, its review, summary and
      resume offers there, view sorts leaving queue order alone, and re-scan entered from an item". Its Rows add R1.2,
      R4.8, R6.7, R7.8, R7.15, R7.19, R8.7, R8.16 and R10.3, each linked to its section as the others are. The line then
      names every row this PRD's inbound Capture lines cite (R1.2 and R1.8, R4.8, R4.11, R4.13, R5.6, R6.7, R6.8, R7.8,
      R7.15, R7.19, R8.7, R8.14, R8.16, R8.18, R9.9, R10.3; R6.9 through the Data Foundation line). Capture has no word
      budget.
- [ ] **F188-3 — the import PRD's Collection Mode obligation line** (Import "Inherited obligations"; L1a-5). Its
      Obligation adds, after "…(Collection Mode R4.8, F65).": " The one matching rule for codes and collection names,
      which Collection Mode's rename, search and code change apply (Collection Mode R1.3, R3.1, R4.4, F68)."; its Rows
      "R2.2, R2.6" → "R2.2, R2.3, R2.6".
- [ ] **F188-4 — the Data Export PRD's outbound Collection Mode line** (Export "What this PRD imposes on others";
      L1a-6). A new line after Inventory Import: "| Collection Mode | Its entries open an export at collection scope and
      at single-item scope ([its R1.8]) | [R1.1] |" (+17 Export). It pairs this PRD's inbound "Data Export | Its R1.1:
      export of a collection or a single item | the export PRD's R1.1 → R1.8".
- [ ] **F188-5 — this PRD's inbound Capture line** — L1a-2 (R4.22 dropped, −3).
- [ ] **F188-6 — the two PRDs without an inbound table** — L1a-1; recorded in SIB-3 and SIB-4.

### Sibling fences, status lines and the ADR queue

- [ ] **SIB-1 — DF F57** (DF fences; new, under a "## Collection Mode round-5 amendment (2026-09-25)" heading as F56
      has). Source paragraph: "Source: the owner's round-5 decisions in the Collection Mode PRD's adjudication of
      2026-09-25, recorded there as [its F173, F175 and F176], and its approved round-5 recommendations 3, 5, 6, 7, 10
      and 11, recorded as [its F180, F182, F183, F184, F187 and F188], with the editorial and testability halves its
      round-5 fix pass carries under its F7, F156, F157 and F166 and its first lock-check run. Its F174 (R7.3j, DJ3)
      lands as a dated line under F54 here, and its F177 under F55 and F21. This fence carries only the Data Foundation
      halves of those decisions; peer review pending." Then:
  - **Title:** "F57 — E35's lifetime, the wipe after the read, the app's own long reads, a failing held write, app-made
    writes deferred, the Collection Mode PRD's F57 mirrored, and the seam line (2026-09-25)".
  - **Authority:** [the Collection Mode PRD's F173, F175, F176, F180, F182, F183, F184, F187 and F188] — owner decisions
    D42, D44 and D45 of its round-5 adjudication, 2026-09-25 (F173, F175, F176), and its approved round-5
    recommendations 3, 5, 6, 7, 10 and 11 the same day (F180, F182, F183, F184, F187, F188).
  - **Decision:** (1) Under the Collection Mode PRD's F157, as its F173 line clarifies it, and its F184: R6.2a's
    deferral follows F157's order — while a read begun before the wipe runs, the removed text stays until that read has
    ended and the file is open, the app wiping it on its own or at the first open after, which corrects F56 (2)'s "until
    that read ends or the file next opens"; E35 is one notice while any wipe waits on another app's read, OK hiding it
    until another such write, and it goes on its own once the wipe lands; DJ4 asserts each, its quit-and-reopen run
    ending the read first, a reopen during the read keeping the text and E35, and a read begun after the delete
    deferring it too. (2) Under its F180, E35 reads "What was there before". (3) Under its F175, R7.3h's copy and R7.3c's
    checks defer a wipe as an export does, with no notice, and ADR-0003's input bounds every other read the app makes by
    that PRD's BROWSE_RESPONSE_BUDGET, restoring F54 (4)'s bound that F56 (4) narrowed to cold loads; DJ4 asserts the
    copy's and the checks' deferral. (4) Under its F176, a held write that fails cancels the close, switch or quit
    waiting on it, its failure state staying up; under its F182, a write the app makes by itself during a hold, R5.5a's
    mark among them, is deferred until the hold ends; R1.11 says both, and DJ3 asserts both. (5) Under its F183,
    ADR-0003's non-waiting save is scoped to R1.10's local-volume scope; the help-docs line for a network volume is a
    post-lock item. (6) Under its F187, R6.3 and OQ 20 carry the Collection Mode PRD's F57: if OQ 20 is still open at v1
    release, R6.3 is deferred and v1's deletes are final. (7) Under its F188, the outbound Collection Mode line names
    every row that PRD's inbound lines cite. (8) Editorial and testability, no owner decision: E33 is marked ‹P1›,
    appearing when Collection Mode R6.3 lands (its F7), the copy header naming that row; R7.6d lists a move's progress
    (the Collection Mode PRD's F156 made it shown); the inbound Collection Mode line's Rows add E15 (its F166) and its
    fence range reads F52–F57; the status line's range reads F50–F54 and F56–F57. R1.11, R7.6q and E35 stay ready for
    alignment; R6.2a, R6.3, R7.3j and R7.6d keep their alignment; E33 stays ready for alignment with its mark; E35
    changes wording only.
  - **Why:** each is the Data Foundation half of a Collection Mode round-5 decision or lock-check fix about the file, a
    state or surface this PRD owns, its copy, an input it hands ADR-0003, or the seam.
  - **Rows:** R1.11, R6.2a, R6.3, R7.6d, E33, E35, OQ 20, DJ3, DJ4, the copy header, the Collection Mode inbound and
    outbound lines, the dated F21, F54, F55 and F56 clarifications.
  - DF's map line: "| F57 | R1.11, R6.2a, R6.3, R7.6d, E33, E35, OQ 20; DJ3, DJ4; the copy header; the Collection Mode
    inbound and outbound lines in [Inherited obligations]; the dated F21, F54, F55 and F56 clarifications. |". DF's
    inbound Collection Mode line "…and the ADR-0003 and ADR-0007 inputs F52–F56 list" → "…F52–F57 list" (word-neutral),
    its Rows adding E15 (ARCH5-N1).
- [ ] **SIB-2 — a Corrected line under DF F56** (DF fences; PM5-1, 5MN2, PRIV5-1): "**Corrected 2026-09-25 ([the
      Collection Mode PRD's F157, as its F173 line clarifies it, and its F175]):** (2)'s "until that read ends or the file
      next opens" read as either trigger, which no build can meet while the read still runs; the wipe follows the read's
      end, on its own or at the first open after, as F57 (1) records. (4)'s "this app's own cold-load reads" narrowed F54
      (4)'s bound beyond the Collection Mode PRD's F158, which dropped only the export; every read this app makes but an
      export, R7.3h's copy and R7.3c's checks keeps that bound, as F57 (3) records. Peer review pending."
- [ ] **SIB-3 — Capture F75** (Capture fences; new, after F74):
  - "### F75 — Mirror Collection Mode's round-5 decisions: R8.14 moves to P0, and the Collection Mode obligation line
    names every row that PRD's inbound lines cite (2026-09-25)"
  - **Authority:** [the Collection Mode PRD's F186 and F188] — its approved round-5 recommendations 9 and 11, round-5
    adjudication 2026-09-25; peer review pending.
  - **Decision:** (1) Under the Collection Mode PRD's F186, R8.14 moves from P1 to P0, since Collection Mode builds the
    simulated, spread and non-spectral marks at P0 under its F1; R8.14 keeps its alignment. (2) Under its F188, the
    Collection Mode obligation line names every row Collection Mode's inbound lines cite — R1.2 beside R1.8, R4.8
    beside R4.13, and the shared collection surface's R6.7, R7.8, R7.15, R7.19, R8.7, R8.16 and R10.3 — with their
    obligations; and, as F188 accepts, this document, whose format has no inbound-obligations table, carries what
    Collection Mode imposes on it by its rows' cites and the dated fences F70–F75, no table added. Traceability's range
    reads F1–F75.
  - **Carried by:** R8.14, Collection Mode obligation line, Traceability.
  - The map line: "- **F75** Collection Mode round-5 mirror — R8.14, Collection Mode obligation line, Traceability.";
    Traceability "Owner decisions F1–F74" → "F1–F75"; the status clause in the Checks.
- [ ] **SIB-4 — Import F68** (Import fences; new, after F67): "### F68 — The Collection Mode line names the matching
      rule, a commit's progress is listed, and UJ 2.1 holds a P0 write (2026-09-25)". **Decision:** "Mirroring [the
      Collection Mode PRD's F188] (its approved round-5 recommendation 11, round-5 adjudication 2026-09-25), with the
      testability halves its round-5 fix pass carries under its F155, F156 and F164: the outbound Collection Mode line
      names R2.3's one matching rule, which Collection Mode's rename, search and code change apply, beside R2.6; this
      document, whose format has no inbound-obligations table, carries what Collection Mode imposes on it by its rows'
      cites and the dated fences F65–F68, no table added. R4.1 lists a commit's progress, which Data Foundation R1.11
      makes show while a close, switch or quit waits on it. UJ 2.1 holds Collection Mode's Rename collection, a P0 write,
      behind this import's commit in place of its P1 Set a field, and asserts R3.8i's End that session shown disabled
      while a Data Foundation R1.11 write runs. R4.1 keeps its alignment." **Why:** "the lock checks pair each seam both
      ways, and a first-phase build offers no Set a field. Source: the Collection Mode PRD's F188; peer review pending."
      The map line "- **F68** Collection Mode round-5 mirror — R4.1; Collection Mode inherited-obligation line; UJ 2.1.";
      Traceability's paragraph adds "; F68 mirrors its F188 (2026-09-25)".
- [ ] **SIB-5 — Export F33** (Export fences; new, after F32): "### F33 — The Collection Mode lines both ways, and R1.1's
      no-change clause (2026-09-25)". **Decision:** "'What this PRD imposes on others' gains a Collection Mode line: its
      entries open an export at collection scope and at single-item scope ([its R1.8]), carried by R1.1. The inbound
      Collection Mode line cites that PRD's row, ([its R8.8], F158). R1.1's clause reads 'itself changes no source value,
      mark or note, as read at SQLITE_READER_FLOOR after success or failure', so an edit made during an export (F32) no
      longer contradicts it; R1.1 keeps its alignment. EJ1's mid-export line asserts that Data Foundation E35 does not
      render for this app's own export." — adding, if the R4.3 grant is ruled in, "R4.3 gains a test-build input holding
      an export running until released, which EJ1's mid-export line declares; R4.3 keeps its alignment." **Why:** "the
      lock checks pair each seam both ways, and F32's clause made R1.1 contradict itself. Source: [the Collection Mode
      PRD's F188] (its approved round-5 recommendation 11), with the editorial and testability halves its round-5 fix
      pass carries under its F158; peer review pending." The map line "| F33 | R1.1; EJ1; the Collection Mode inbound and
      outbound lines |" (R4.3 added if ruled in); R1.1's Commit PR cell "…amended 2026-09-25 (F32, F33), alignment kept".
- [ ] **SIB-6 — the ADR-0003 row** — in F175-2. The 0007 row is unchanged.

## Checks

- [ ] **Testability pairing (process rule 3).** Every row, state and metric changed above has its acceptance case and
      its seam line in the same pass: R1.3 → T5 and UJ1.1-a–e (unchanged; the cite only); R4.6 → T12 (unchanged); R8.1f → UJ9.5-g
      (unchanged by N1); R8.8 → UJ9.7-h (F173-3), UJ9.7-i (F174-3); R8.10a → UJ9.5-g, UJ9.7-h and the map's "the file"
      line (F185-1); R8.10f → UJ9.5-a, its Rows adding R2.5 (L20-1); E6 → UJ1.3-g, UJ1.3-h, UJ7.1-s; E8 → UJ6.2-b/d
      (unchanged); E10 → UJ1.3-h (F181-2), UJ7.1-s; the Harness → UJ9.8-c's storage controls (R5-B1). Sibling pairs: DF
      R6.2a → DJ4's lines (F173-2, F175-4, F184-3); DF R1.11 → DJ3's lines (F176-2, F182-2) and UJ9.5-g (F176-3); DF
      R7.3j → DJ3 (F174-2) and UJ9.7-i (d); DF R6.3 → DJ4's P1 lines (unchanged; the deferral is a release fate, read at
      v1 release); DF R7.6d → DJ3's state cell (PERF5-3); DF E33 → DJ4's selection line (unchanged); DF E35 → DJ4,
      UJ9.7-h, EJ1's negative; Capture R8.14 → this PRD's R2.4c/R2.4d/R2.6/R2.7 cases (unchanged); Import R4.1 and R3.8i
      → UJ 2.1 (R5-m6); Export R1.1 → EJ1 (unchanged; R4.3's hold if ruled in). New cases: UJ7.1-s; new lines in DJ3,
      DJ4 and Import UJ 2.1.
- [ ] **Word counts** by rule 14's method. Collection Mode body: start 11,997, budget 12,000; each Result note that adds
      body words names its trim from the candidates; report the final count and the headroom left for the pre-lock
      round; flag any fix that cannot be paid for and any template-scaffolded paragraph trimmed. Data Foundation body:
      start 8,292, budget 8,400 (F177); report the final count; stop and report under the header's stop rule if it
      would pass 8,400. Export: start 3,723, budget 4,000. Report the companions' counts, unbudgeted.
- [ ] **Meaning check on every trim.** Before a trim lands, diff its removed text against the row, fence or table the
      candidate names, row by row, and name that row in the Result note — candidate 1 against R1.1, R1.9, R1.10, R7.1 and
      R7.2; 2 against the Build dependencies' last row and the capture PRD's F20; 3 against the Vocabulary preamble and
      the transition lines; 4 against DF R3.4, F146 and F127's dated line; 5 against the Stop cells and F124; 6 against
      README §6; 7 against the constants table and the Outbound Capture Mode row; 8 and 9 against the constants table and
      OQ 11's Interim cell; 10 and 11 against their own sentences. Also L2-1's replaced Shows text against R4.2a–h,
      R5.2a–e and the Swatch grid row; interface N1 against DF R1.11; L1a-2 against Capture R8.14. The DF additions are
      checked against their fences' Authority text, the owner's own words, not against this list's drafts (ARCH5's
      risk).
- [ ] **Fences and Carried-by.** Fill the `_(filled by the round-5 fix pass)_` Carried-by lines and map lines of F173–F176
      and F178–F188 in the Carried-by grammar (F177 already reads "governs no rows"). As drafted:
  - F173 — UJ9.7-h, the Data Foundation PRD R6.2a, the Data Foundation PRD E35, the Data Foundation PRD DJ4, the Data
    Foundation PRD F57
  - F174 — UJ9.7-i, the Data Foundation PRD R7.3j, the Data Foundation PRD DJ3, the Data Foundation PRD F54
  - F175 — the Data Foundation PRD DJ4, the Data Foundation PRD F56, the Data Foundation PRD F57 (and the Data
    Foundation PRD R6.2a if F175-1's row text is ruled in)
  - F176 — UJ9.5-g, the Data Foundation PRD R1.11, the Data Foundation PRD DJ3, the Data Foundation PRD F57
  - F178 — E8; F179 — E6; F181 — E10, UJ1.3-h
  - F180 — the Data Foundation PRD E35, the Data Foundation PRD F57
  - F182 — the Data Foundation PRD R1.11, the Data Foundation PRD DJ3, the Data Foundation PRD F57
  - F183 — the Data Foundation PRD F57
  - F184 — the Data Foundation PRD R6.2a, the Data Foundation PRD DJ4, the Data Foundation PRD F57
  - F185 — R8.10a
  - F186 — the capture PRD R8.14, the capture PRD F75
  - F187 — the Data Foundation PRD R6.3, the Data Foundation PRD F57 (OQ 20 is not an ID the grammar parses; F57 names
    it)
  - F188 — the Data Foundation PRD F57, the capture PRD F75, the import PRD F68, the Data Export PRD F33

  Also: F167's Carried-by and map line add UJ7.1-s; F157's add the Data Foundation PRD F57 (the order restored under
  it); F155's, F156's, F164's and F176's add UJ9.5-h if the split is ruled in. No placeholder is left.
- [ ] **Sibling status lines, each in its sibling's style, peer review pending.**
  - DF, keeping its compact fence-citing form: "…Collection Mode amendments 2026-09-24 and 2026-09-25 under F50–F54 and
    F56 (…)" → "…under F50–F54 and F56–F57 (…)" (word-neutral; F57 now among the fences whose Decision and Rows name the
    rows, states and questions — R6.3's and R7.6d's alignment kept, E33's ‹P1›, OQ 20's cell).
  - Capture: "; Collection Mode round-5 amendment 2026-09-25 under F75 (R8.14 moved to P0 with alignment kept; the
    Collection Mode obligation line names every row that PRD's inbound lines cite; peer review pending)".
  - Import: "; Collection Mode round-5 mirror under F68 dated 2026-09-25 (R4.1 amended with alignment kept; the
    Collection Mode obligation line names R2.3; peer review pending)".
  - Export: "; Collection Mode round-5 mirror 2026-09-25 under F33 (R1.1 amended with alignment kept; a Collection Mode
    outbound line added; peer review pending)" (and R4.3 if ruled in).

  This PRD's own status line stays "Status: draft".
- [ ] **Label check** (check 13, both directions, and the capture PRD's standing label check for its amendment). Every
      action a row or case names resolves character for character to a copy label — this PRD's quoted ("Find similar",
      "Grid", "Table", "Undo", "Undo change", "Delete swatch", "Rename collection", "Set a field", "Select all", "Delete
      selected", "Use as scan order", "Columns", "Rename column", "Change code", "Use this reading", "Compare"), a
      sibling's named by its document and state, unquoted (the Data Foundation PRD's E8, E14, E34 and E35 actions; the
      capture PRD's start, Flag, E25 end action; Import's End that session). Re-run after L13-1's quotes, L2-1's Surfaces
      labels, E6, E8 and E10's copy (F178-1, F179-1, F181-1) and DF E35 (F180-1). Direction 2 prints nothing; direction 1
      lists no unquoted label outside the disposed sites.
- [ ] **Traceability** "F1–F172" → "F1–F188" in this PRD (word-neutral); the capture PRD's "F1–F75"; the import PRD's
      Traceability paragraph adds F68. New case IDs take each journey's next free letter (UJ7.1-s); nothing is renumbered.
- [ ] **Seam check (the Inherited-obligations standing check; the second run's check 1a).** After F188-1 to F188-4,
      L1a-1, L1a-2 and SIB-1, each seam's two summaries state the same set: this PRD's Outbound Data Foundation row
      ("as its F52–F57 record", word-neutral) against DF's inbound Collection Mode line ("F52–F57 list"); this PRD's 14
      inbound Data Foundation lines against DF's outbound Collection Mode line (F188-1); the inbound Capture lines against
      Capture's Collection Mode line (F188-2); the inbound Import lines against Import's (F188-3); the inbound Data Export
      line against Export's new line (F188-4); the Outbound Data Export row against Export's inbound line (interface N5);
      the Outbound Capture Mode and Inventory Import rows against the rows and fences L1a-1 names. Report each pair.
- [ ] **ADR queue and post-lock.** `docs/decisions/README.md`'s 0003 row is edited as F175-2 states (still eight
      inputs); the 0007 row is unchanged. `docs/product/post-lock.md` gains F183-2's and F185-2's items, in its style,
      attributed to this PR. Its item "**DF** — whether a Collection Mode rename moves an imported column's stored name"
      (line 98) is still **NOT ticked**: it is ticked when the PR exists, at the bookkeeping close.
- [ ] **Constants and OQs.** No constant, candidate or interim changes. L7-1 changes four Owning-rows cells, and
      candidates 4, 7, 8 and 9 (if taken) change Decision cells only; the Interim-stated list is unchanged and the
      no-interim list stays None, derived from the Open questions table.

## Out of scope for this pass

- The lock-time items the last lock-check box lists: the status line, guidance comments, sibling "peer review pending" rewrites,
  `docs/product/README.md`'s index status (IF-16), the post-lock PR number — at lock or the bookkeeping close.
- A Data Foundation budget beyond 8,400, or a split, if its body would pass it — the owner's, reported under the stop
  rule.
- Any change a fence above does not authorize. Reported, not made: the Device mirror of DF R1.11 (needs owner); E34
  copy naming a refused pick (PM5-4); copy saying an export is one snapshot (the marketing pointer); a help-docs line on
  the app's own long reads deferring a wipe (PRIV5-5 (c)); the deleted state's Vocabulary line (PRIV5-5 (d)); a bound
  on WAL growth under a long outside read (performance); Import E40's wording when only the commit is refused (the
  engineering review's deferred item, settled by F169); and whichever of the orchestrator's rulings above are declined.
