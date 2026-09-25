# Collection Mode PRD — round 4 fixes (review round 4, 2026-09-25)

The resume point for the fix pass over the findings of review round 4 (subject commit `d195d9b`; this list drafted at
`930ce6a`), before round 5. The fence file `prd-collection-mode-fences.md` (F1–F172) is the written authorization for
every change below: a **fix** names the fence that authorizes it, or reads "editorial/testability — no new WHAT" where
it only adds a case, a seam grant, a fixture, a cite, or a wording fix that makes copy or a hand-off match an existing
row. A change no fence names is not made: it is reported to the orchestrator as an unratified WHAT. The owner has
decided this round's forks as F155–F172, and recommendation 13's two declines stand in Rejected findings, so no box
below waits on the owner; anything the pass meets that no fence states is reported, not made. Each box is ticked, with
a Result note, as its fix lands.

**Word budgets.** Counted by rule 14's method, the body only, companions excluded:

~~~
perl -0pe 's/<!--.*?-->//gs; s/```.*?```//gs; s/\]\([^)]*\)/]/g; s/^\|[-: |]+\|\s*$//mg; s/\|/ /g' FILE | wc -w
~~~

- **Collection Mode body: budget 12,000; re-run at `930ce6a`: 11,997.** Three words of headroom. Every body addition
  names, in its Result note, the rule-free text it replaces in the same pass; a fix that cannot be paid for is flagged,
  not squeezed in. Ledger, estimated on the drafted text below (the pass re-counts):
  - Additions: R8.8 +6 (F157-3); R8.10a +26 (F164-1); R8.3 +2 (F161-1); R4.7 +2 (F163-1); the Outbound Inventory
    Import row +10 (F166-3); a new Outbound Data Export row +19 (F158-2); a new Inbound Data Foundation R1.11 line +14
    (F155-10); the Surfaces preamble, table and copy index +20 (F167); Build dependencies row 1 +1 (F157-2); OQ 1's
    Closer +3 (4N3) — about **+103**.
  - Savings: R8.1f −19 (F155-6); R8.10 −2 (F162-1); R4.1 −4 (F168-1); the Outbound Data Foundation row −37 (F166-1);
    the Outbound Capture Mode clause −5 (F155-10); the OQ 2, 3, 4, 6 and 12 Decision cells −36 (candidate 2) — about
    **−103**.
  - Net about 0, so the body lands near 11,997. The Decision-cell trim is needed for the ledger to close. The one
    reserve is candidate 3 (−9), template body text, not taken without the orchestrator's confirmation.
- **Data Foundation body: budget 8,300 (F160; its F55 as clarified); at `930ce6a` it stands at 8,164.** 136 words of
  headroom, and F160 chose "no further trimming, so no risk of changing meaning", so the DF text below is drafted
  tight and nothing in that body is trimmed. Estimated additions: R1.11 +47 at its tightest, +61 as drafted (F155-1);
  R6.2a's deferral +27 and R2.3's cite +4 (F157-1); R7.6q +24 (F157-2); R7.3j +13 (F159-1) and +5 (F155-5); R7.6p +7
  (F159-2); R3.4 +1 and R6.2a +4 (F165); R1.3 +1 (F155-2); R1.5 −7 (F155-3) and its help-docs line +1 (F157-5); R1.9
  −7 (F155-4); OQ 17 +9 (F155-9); the inbound Collection Mode line about +8 (F166-2); the outbound R1.11 lines about +17
  (F155-10); the status line about +6 (Checks) — about **+160 with R1.11 at its tightest (≈ 8,324), over 8,300**, and
  about **+131 (≈ 8,295)** without the three lowest-priority items, in order: F155-5 (a Nit whose rule R7.3j already
  reaches through "R1.9's move"), the DF outbound R1.11 lines (the citing sibling rows carry that side), F159-2 (DJ3
  pairs R7.3j without it). **Stop rule:** land R1.11 at its tightest; if the DF body would still pass 8,300, leave those
  three out in that order and report each; if it passes 8,300 even so, stop and report the figure — a further budget or
  a split is the owner's, not this pass's.

**Candidate trims the reviewers proposed** (counts by rule 14's method; each re-checked rule-free before it goes):

1. **The Outbound Data Foundation row, compacted to the Data Foundation PRD's form** — authorized by F166 (IF4-3, 4MN5;
   R4-M1 offered its search-input clause). 125 → about 88 words: −37 net of the new content F155 and F157 add (about
   −55 gross). Drafted in F166-1. Not template text.
2. **The OQ 2, 3, 4, 6 and 12 Decision cells**, each "F⟨n⟩ holds ⟨candidate⟩ as its candidate", which restate the
   constants table (4MJ1): 41 words → "None." in each, as the Data Foundation PRD writes an undecided cell (−36).
   Provenance survives: the Interim-stated list keeps F34, F16, F7, F20 and F43/F103 on those questions. OQ 5's cell
   ("…; no source measured") is not among them and stays. Not template text.
3. **The Inherited-obligations preamble's rationale clause** "because ID families, fence numbers included, repeat
   across PRDs" (PERF4-6; −9). **Template body text:** agent-prd v1's `prd-template.md` scaffolds this paragraph as
   body text, rationale included ("because the row-ID families repeat across PRDs"). Not taken without the
   orchestrator's confirmation — the same caveat as the §8 preamble and the Open-questions trailer, which are template
   body text too and are not candidates.
4. **R8.1f's own restatement, once it cites the Data Foundation rule** (F155): the row goes from 67 to 48 words (−19).
   Drafted in F155-6. Not template text.
5. **The Outbound Capture Mode row reworded** (PM4-3): its hold clause, 16 → 11 words under F155 (−5). Drafted in
   F155-10. Not template text.

The findings are the eight round-4 delta reviews in `docs/agent-reviews/2026-09-24-prd-collection-mode-peer-reviews.md`
(Round 4 › Per-lens reviews (verbatim)), in log order; that log's Round 4 verify-the-reviewer table ("log row n")
records the six accepted Blockers and Majors: rows 1, 5 and 6 are testability fixes, rows 2, 3 and 4 the owner's
F159, F155–F156 and F157. Each box carries the reviewer's own ID and severity. A round-3 finding that a review's delta
table marks PARTIAL or UNRESOLVED gets its own box, labelled "carried: ⟨ID⟩". One box carries each fix, the first in
log order, and every other box raising the same defect says **covered by** it and lists what it answers; where a later
box adds a part the first lacks, that part is its own. An item a review raised only in its Missing or Deferred list,
carried by no rated finding, gets a box marked *unrated*. The fence edits themselves are boxed once, in "Owner decisions
F155–F172 — the edits each requires" below (F155-1 and so on); a finding box points at them. Sibling IDs name their
document ("DF", "Capture", "Import", "Export"); a bare ID is this PRD's own.

Dispositions: **fix**, **covered by**, **declined (F-rejection, recommendation 13)** and **needs owner**. Needs owner
is strict: any new or changed product rule, copy meaning, constant or interim, or sibling rule that no fence F1–F172
states. This list marks none.

## Orchestrator rulings on this list (2026-09-25, before the pass)

These override the boxes they name. Where a box says "on the orchestrator's say-so", this section is that say-so.

- [ ] **Candidate trim 2 — not "None."** A fence did settle each of these OQs' candidate, so "None." would misstate
      the "Decision so far" column. Each of OQ 2, 3, 4, 6 and 12 reads instead "F⟨n⟩'s candidate." (F34, F16, F7, F20,
      F43): −30, not −36. Find the remaining words in rule-free prose, meaning-checked. Candidate 3 is **not taken**:
      it is template text.
- [ ] **F157-1 — the deferral covers every earlier read.** F139 already allows this app's own short cold-load reads to
      defer a wipe, and F157-4's Harness line relies on that. So R6.2a's clause must not list only "another app's or an
      Export R1.1 snapshot's". Land it as tight as it will go and still say all of this, for example: "— or, while a
      read begun before it still runs, once that read ends or the file next opens, E35 naming another app's —". The
      Export snapshot is still named in the Data Foundation F56 line and in Export R1.1. F157 lands as an amendment
      to R6.2a, not a new row: **confirmed**.
- [ ] **The Data Foundation stop rule, revised.**
  1. Tighten every Data Foundation addition to its shortest form with the same meaning: R1.11, R6.2a's clause and
     R7.6q.
  2. If still over 8,300, drop F155-5, then F159-2, and report each.
  3. **Keep the Data Foundation outbound R1.11 lines** (F155-10). Process rule 12 needs both sides of the seam, and the
     Seam check compares them.
  4. If the body would still pass 8,300, land everything else, stop, and report the figure. Never cut a rule.
- [ ] **F164-1's widening is confirmed.** The held-write input names import commits and moves. It is a testability
      grant under F155 and F164, not a product rule.
- [ ] **R4-m2's Z range is confirmed.** The D50 white is not pinned.
- [ ] **F157-6 and F171-1 (the ADR-0003 row in `docs/decisions/README.md`): go ahead**, as drafted and with the
      F157-1 ruling above. The 0007 row stays unchanged.
- [ ] **Editorial, pre-existing, fix now:**
  - DF copy E33's missing Status cell reads "⌛️ Ready for Alignment", as the DF status line records.
  - DF F1's map line "8,000 since 2026-09-14" names the current budget and points at F55.
  - Both are recorded under DF F56 as editorial.

## Status bookkeeping

- [ ] **Flip to aligned** (the log's round-4 flip record, less the holds and rewrites below) — in the PRD's tables and
      the copy file's Status lines. The record lists 101 IDs; 14 are lettered sub-rows that hold with an objected lead
      and one is rewritten, so **86 read aligned**. **19 newly:** R1.3, R1.4, R2.5, R2.8, R3.3, R3.9, R4.5, R4.9, R5.1,
      R5.3, R5.4, R6.2, R6.3, R6.4, R8.2, R8.9, E9, E12, E17. **67 stay aligned:** E1, E2, E3, E4, E5, E7, E11, E13,
      E14, E15, E16, E18, E19, M2, M3, M4, R1.1, R1.2, R1.5, R1.6, R1.7, R1.8, R1.9, R1.10, R2.1, R2.2, R2.3, R2.4 and
      R2.4a–i, R2.6, R2.7, R2.9, R2.10, R3.1, R3.2, R3.4, R3.5, R3.6, R3.7, R3.8, R4.4, R4.6, R4.8, R5.2 and R5.2a–e,
      R5.5, R5.6, R5.7, R5.8, R6.1, R7.1, R7.2, R8.4, R8.5, R8.7. No box below rewrites any of them; if one does after
      all, it lands at pre-alignment instead and the Result says so. Copy-file Status lines: E9, E12 and E17 read
      aligned; E1–E5, E7, E11, E13–E16, E18 and E19 stay aligned.
- [ ] **Sub-rows hold with their lead — flag to the orchestrator.** The record also lists R4.2a, R4.2b, R4.2d–h
      (7), R8.1a, R8.1b, R8.1d, R8.1e (4) and R8.10b, R8.10c, R8.10f (3). A lettered sub-row flips only with its lead
      and all its siblings: R4.2 and R4.2c (R4-m2), R8.1, R8.1c, R8.1f and R8.1g, and R8.10, R8.10a, R8.10d and R8.10e
      are objected, so all three families hold and carry their lead's status (below). R2.4 and R5.2 flip whole.
- [ ] **An aligned row now objected — flag to the orchestrator.** E10, aligned since round 3, is objected by IF4-4 and
      rewritten by F167 (its copy-index Surface cell), so it moves to pre-alignment — the one aligned row this round
      moves back.
- [ ] **Rows a round-4 fix rewrites → pre-alignment:** R4.1 (F168-1); R4.7 (F163-1); R8.1 and R8.1a–g (R8.1f rewritten,
      F155-6; the lead objected); R8.3 (F161-1); R8.8 (F157-3); R8.10 and R8.10a–f (F162-1, F164-1); E6 (F167-3,
      F170-3); E8 (F170-1 — flip-eligible, but rewritten); E10 (F167-3). Copy-file Status lines: E6 and E8 stay
      pre-alignment; E10 moves from aligned to pre-alignment.
- [ ] **Rows objected and left unchanged → needs-discussion** (the Legend: an open objection). Each whose objection a
      companion-only fix answers is named with that fix, so round 5 can flip it once the lens re-checks:
  - R2.11, R4.2 and R4.2a–h — R4-m2's Z range in the Named defaults' ZX-001 line and UJ4.1-a (companion only).
  - R8.6 — R4-B1's Byte checks text, PRIV4-1's UJ7.1-r moments, PRIV4-2's storage list, PRIV4-4's crash run and
    F172-1's journeys line (companion only).
  - R8.11 — PERF4-3's UJ9.5-d export run, F158-5 (companion only).
  - M1 — R4-m6's UJ9.5-a session lifecycle (companion only); M1 moves from pre-alignment.
  - R4.3 — not companion-only: 4MJ1's and PERF4-1's objection is answered by F157 in R8.8 (F157-3), DF R6.2a
    (F157-1), DF E35 (F157-2) and UJ9.7-h (F157-7); R4.3's byte clause states what is gone, R8.8 when.
- [ ] **After the pass:** report the final lists; the record's 101 flip-eligible and 21 objected reconcile to the PRD's
      122. Expected: **86 aligned** (67 staying, 19 new); **22 pre-alignment** (R4.1, R4.7, R8.3, R8.8, E6, E8, E10,
      and the R8.1 and R8.10 families, 8 and 7); **14 needs-discussion** (R2.11, R4.3, R8.6, R8.11, M1, and the R4.2
      family, 9). No new row, state, variant or metric enters this PRD; new cases only (UJ1.3-h, UJ3.4-l, UJ6.4-q,
      UJ9.7-h, UJ9.7-i). New sibling rows and states (DF R1.11, R7.6q, E35) enter ready for alignment in that PRD's
      vocabulary.

## peer-product-manager-reviewer (Claude route)

- [ ] **PM4-1 (Major) — the compaction dropped F151's "which restores access only" from DF R7.3j, so "Choose the file
      again" reads as R7.3b's close-and-reopen, ending the delete undo and "Undo change"; a different file's fate was
      undecided** (DF R7.3j, R7.6p, DJ3, DF F54's compaction paragraph; DF inbound Collection Mode line; this PRD's
      Outbound Data Foundation row; UJ9.7-g). Also answers N4-M1 and ARCH4-1. **fix (F151, F159; log row 2):** F159-1
      (R7.3j), F159-2 (R7.6p), F159-3 (DJ3), F159-4 (this PRD's case beside UJ9.7-g, UJ9.7-i), F159-5 (the compaction
      paragraph), F159-6 (the dated DF line). PM4-1's third bullet — E34's actions and "restores access only" back in
      the DF inbound line — is **covered by F166-1 and F166-2**: the owner chose the compact form on both sides, E34's
      actions named by DF R7.3j there, not restated.
- [ ] **PM4-2 (Minor) — "E10's "Undo" of a multi-item delete" reads two ways, so the undo of a one-item or empty
      collection's delete slips past R8.3 and then, under R8.1g, holds every write** (R8.3, R8.1g, R8.11; UJ1.3). Also
      answers R4-m3, IF N3 and ARCH4-N1. **fix (F161):** F161-1 (R8.3), F161-2 (Capture's obligations line), F161-3
      (UJ1.3-h).
- [ ] **PM4-3 (Minor) — F152's "every other write waits" is mirrored for three writers only; E25's "End that session"
      (Capture R7.13), set-aside decisions (Capture R8.5) and Import E40's "End that session" stay live** (R8.1f; the
      Outbound Capture Mode row; Capture R1.1, R3.5, R7.13, R8.5; Import E40 and R3.8i; UJ9.5-g). **fix (F155):**
      Capture R7.13 and R8.5 cite DF R1.11 (F155-8); Import R3.8i (F155-7); the Outbound Capture Mode row gets PM4-3's
      word-saving reword, citing DF R1.11 rather than R8.1f under F155 (F155-10); UJ9.5-g fires the capture PRD's E25
      end action on Scale Three during the held delete (F155-11).
- [ ] **PM4-4 (Nit) — Import E40's another-collection variant reads as a queue ("an import waits until it ends")**
      (Import E40). Also answers N4-m1 and IF N7. **fix (F169):** F169-1.
- [ ] **PM4-5 (Nit) — the "full" gate is unclear about "Use this reading"** (E6 "full" condition; R8.3; the Harness's
      Phases and builds). **fix (F170):** F170-3. R8.3 and the Harness keep "every P1 action that variant names".

PM's two biggest risks beyond its findings — the compaction's "no rule changed" claim, and the same risk in this body's
next trim — are carried by F159-5 and by the Checks' meaning check on every trim.

## peer-staff-software-engineer-reviewer (Claude route)

- [ ] **4MJ1 (Major) — F139's user-visible half ("an outside reader's hold leaves the edit shown not yet saved")
      reaches no row, copy state, seam input or case, and R4.3 and R8.8 read as if an edit lands at Return** (R4.3,
      R8.1, R8.1c, R8.8, R8.10a; the ADR-0003 row; DF R1.5; F102, F139). Also answers IF4-5, R4-m5, ARCH4-2, PERF4-1
      and carried ARCH3-4 and PERF3-7's outside-reader half. **fix (F157, F164; log row 4):** F157 replaces F139's
      "shown not yet saved": the change is saved at Return, and only the wipe of the text it removed waits for a read
      begun before it. F157-1 (the DF home, R6.2a), F157-2 (DF E35 and its surface), F157-3 (R8.8), F157-4 (the byte
      rule's carriers and the Harness), F157-5 (DF R1.5's help docs), F157-6 (the ADR-0003 input), F164-1 and F157-7
      (R8.10a's outside-read input; UJ9.7-h). "Lands" means saved for every write; R8.8 already defines it (the whole
      write in the file before any surface shows it done), so no Vocabulary entry is added. R4.3 is not rewritten (see
      Status bookkeeping). The trims 4MJ1 offered are candidates 1 and 2.
- [ ] **4MJ2 (Major) — F149 gives an import commit only the forward ordering; nothing stops a session's start or
      resume, or a Collection Mode bulk write, while a commit runs** (Import R3.2; Capture R3.5; R8.1f; Capture T7,
      Import UJ 2.1, UJ9.5-g). Also answers ARCH4-4's reverse gap and PERF4-6. **fix (F155; log row 3):** DF R1.11 names
      the import commit among the writes that hold every other write and a session's start (F155-1); Import R3.2 adds
      the reverse ordering (F155-7); Capture R3.5 cites R1.11 (F155-8); the runs: F155-11, F155-12, F155-14.
- [ ] **4MN1 (Minor) — closing the file, switching files or quitting is not ordered against a running bulk write or
      delete** (R8.1f, R8.1g, R8.8; DF R1.3, R1.9). **fix (F156):** R1.11's second sentence (F155-1), DF R1.3's cite
      (F155-2), UJ9.5-g's close, switch and quit run (F155-11) and DJ3's line (F155-13).
- [ ] **4MN2 (Minor) — R8.10 says every readback but R8.10f exists only in test builds, against UJ9.4-b and UJ9.4-d's
      Release-build reads** (R8.10; UJ9.4-b, UJ9.4-d; F132). Also answers R4-n2. **fix (F162):** F162-1; the dated
      line under F132 is already in the fence file.
- [ ] **4MN3 (Minor) — F152's Capture mirrors skip samples per row, illuminant and observer ("editable at any time")
      and scan mode, whose changes are file writes that start a regeneration** (Capture R1.3, R1.5, R1.10; T7). Also
      answers IF4-1's settings half. **fix (F155):** F155-8 (R1.5's "at any time" qualified), F155-12 (T7).
- [ ] **4MN4 (Minor) — the compaction made DF R3.4's appositive bind to sRGB's white, dropping the tie between the
      flag test and the stored derivation** (DF R3.4; DF F54 (3)). Also answers N4-m2. **fix (F165):** F165-1; F159-5
      records the meaning change.
- [ ] **4MN5 (Minor) — the compaction undid round 3b's seam symmetry, and DF's "R2.9's Flag removal for its restore of
      a flagged item" no longer parses** (the Outbound Data Foundation row; DF's inbound Collection Mode line). Also
      answers IF4-3, PRIV4-5's second half and ARCH4-N3's first. **fix (F166):** F166-1, F166-2. The round-3 fix
      file's 3b Result note is history and is not edited; the Checks' seam check stands in its place.
- [ ] **4MN6 (Minor) — the byte check's exemption doesn't say whether the Named defaults' untouched seeded values
      count, which decides T1, T6, UJ4.2-a and UJ4.4-b** (the Harness's Byte checks). **covered by R4-B1** — its text
      keys the exemption on the file's values at the moment of the read, "the Named defaults as its Given and When
      change them".
- [ ] **4N1 (Nit) — R4.7 would end the undo history on a write the app makes by itself; E8 implies only the user's
      actions do** (R4.7, E8; DF R5.5a–b). **fix (F163):** F163-1, F163-2.
- [ ] **4N2 (Nit) — UJ9.5-e requires "R8.2 built" although it is F148's closure oracle for P0 R8.1b** (UJ9.5-e).
      **fix (F148 — testability):** UJ9.5-e drops "R8.2 built" and its Rows read R8.1, R5.1, as R3-m8 did for
      UJ9.5-c; the UJ 9 preamble's phase rule for "UJ9.5-e to UJ9.5-g" then runs it in the first phase.
- [ ] **4N3 (Nit) — OQ 1's Closer omits the owner's estimate that F148 makes HISTORY_READINGS_CEILING's closure
      evidence** (OQ 1; the constants table). **fix (F148 — editorial: the Closer names what F148 names):** "…; the
      owner ratifies." → "…; the owner ratifies, and estimates HISTORY_READINGS_CEILING." (+3, in the ledger).
- [ ] **4N4 (Nit) — Traceability reads "Owner decisions F1–F153"** (Traceability). Also answers IF N4 and R4-n3.
      **fix (editorial):** "F1–F172" (word-neutral).
- [ ] **4N5 (Nit) — the Outbound Inventory Import row names only the R8.1f hold, not F149's refusal of a commit while
      any session is in flight** (the Outbound Inventory Import row). **fix (F166):** F166-3.

The review's thirteen clarifying questions are answered by the fences: 1–4 by F157; 5 by F158; 6 by F155; 7 by F156; 8
by F155; 9 by F162; 10 by R4-B1's text; 11 by F163; 12 by F165; 13 by 4N2. Its Deferred items are carried by boxes above
or below: architecture — 4MJ1's mechanism (F157), chunked exports (F158), import-commit arbitration (F155); performance —
R8.1c under a waiting edit (PERF4-4: under F157 no edit waits), and IMPORT_BUDGET at ROWS_CEILING under the byte scrub,
which under F155 becomes the length of the hold an import commit imposes — Capture OQ 13 owns IMPORT_BUDGET, so no box;
test — 4MN6, and symbol names in crash reports and unified-log entries false-matching "Pink" or "Yellow" (R4-B1's
whole-token storage rule); product marketing — Import E40 (F169), and E40's "Target: / Another collection: / All
variants append:" layout inside one copy cell, which no fence changes: reported, not made; product manager — whether
outside readers may delay edits (F157).

## peer-test-reviewer (Claude route)

- [ ] **R4-B1 (Blocker) — the byte check's whole-text exemption closes the paragraph that also governs reads of the
      app's own storage, so UJ9.4-d, UJ7.1-r, UJ3.3-f and UJ6.4-j each exempt the text they check** (the Harness's Byte
      checks; UJ9.4-d, UJ7.1-r, UJ3.3-f, UJ6.4-j; R8.6, R8.10d, R8.10e; the Test-controls map's app-storage line). Also
      answers PRIV4-1's first remediation, 4MN6, R4-m1's matching half, and carried R3-m5 and PRIV3-2. **fix (F35, F53,
      F102, F130 — testability; log row 1; companion only, 0 body words):** in the Harness's **Byte checks** paragraph,
      replace the sentences from "The raw scan, like every read of the app's own storage for a text…" through "…and so
      is the whole text, in any letter case." with exactly:

  > The raw scan finds no match for the text, or for any word of it of four or more letters, in any letter case or
  > in the import PRD's R2.3 normalised form, encoded as UTF-8 or as UTF-16 of either byte order. The SQL read
  > matches as the raw scan does, in any letter case or normalised form, but checks every word of the text whatever
  > its length, at SQLITE_READER_FLOOR, in every table the file holds, its internal tables and any full-text
  > index's vocabulary included. In the file's bytes and the SQL read, and nowhere else, a word is exempt where a
  > value the file holds at the moment of the read contains it — as the case declares that value, the Named
  > defaults as its Given and When change them — and so is the whole text, in any letter case. A read of the app's
  > own storage for a text (unified-log entries read decoded) exempts nothing: it finds no match for the text, or
  > for any word of it of four or more letters, as a whole token — bounded on each side by the start or end of the
  > data or by a character that is not a letter or digit — in any letter case or normalised form, in either
  > encoding.

  "UJ9.8-c is the control this check must catch." follows unchanged, adding ", its SQL-read run included" (R4-m1).
  The four cases then have teeth: UJ9.4-d's Harbour, UJ7.1-r's Sky Blue, Cerulean and Purple, and UJ3.3-f's zx-01
  are checked in storage with no exemption; UJ6.4-j's read before its undo keys on the file's values at that
  moment, the three Family values empty, so Pink and Yellow are not exempt. Whole tokens keep "blue" in
  "CoreBluetooth" from false-matching a storage read (PRIV4-1's second reading). The map's app-storage line adds
  "as whole tokens, exempting nothing". The value is the case's declared one, never read from the build's file
  (PRIV3-7).
- [ ] **R4-M1 (Major) — R8.1f's hold has no injection path, so UJ9.5-g either depends on the build being slow or skips,
      and the three sibling Givens declare a state no seam grants** (R8.1f, R8.1g, R8.10a; UJ9.5-g; Capture T7, the DF
      DJ3 line, the Import UJ 2.1 line). **fix (F164; log row 5):** F164-1 (the held-write input), F164-2 (UJ9.5-g's
      hold checks run with the write held, its timing runs unheld), F164-3 (the sibling Givens cite R8.10a). The trim
      R4-M1 offered, the Outbound Data Foundation row's search-input restatement, is inside candidate 1.
- [ ] **R4-m1 (Minor) — the SQL read's matching is unstated, and no control exercises it** (the Harness; UJ9.8-c). The
      matching: **covered by R4-B1** (its SQL-read sentence). The control: **fix (F35, F102 — testability):** UJ9.8-c
      gains a run whose control is the word warm stored as a full-text index vocabulary term after the term war,
      prefix-compressed so the copy's raw bytes hold warm nowhere; the check reports a match in that run, from the SQL
      read alone.
- [ ] **R4-m2 (Minor) — ZX-001's Z of 49.87 depends on which published D50 white the derivation uses** (the Named
      defaults' ZX-001 line; UJ4.1-a; R2.11, R4.2, R4.2c). **fix (F47, F105 — testability):** the line and UJ4.1-a read
      "Z 49.85, 49.86 or 49.87", as X already reads "26.79 or 26.80", the line saying why (the CIE/ASTM, CIE 15 and
      ICC D50 whites round Z differently). The reviewer's other option — naming the white (96.422, 100, 82.521) — is
      not taken: the white a stored derivation uses is the Data Foundation PRD's (its R7.5 reference values, OQ 6), and
      a harness constant would decide it; reported. R2.11 and the R4.2 family stay unchanged.
- [ ] **R4-m3 (Minor) — "a multi-item delete" is undefined** (R8.3, R8.1g; Capture's obligations line). **covered by
      PM4-2.**
- [ ] **R4-m4 (Minor) — R4.1's source-gone branch has no case, and closing a result's detail after a re-read removed
      E9's item is unstated** (R4.1; UJ3.4). **fix (F168):** F168-1, F168-2.
- [ ] **R4-m5 (Minor) — F139's outside-reader hold has no case** (R8.1c, R8.8). **covered by 4MJ1** — its case is
      UJ9.7-h (F157-7), asserting F157's "saved, wipe pending" rather than the reviewer's "reads the write as not done",
      which F157 supersedes.
- [ ] **R4-m6 (Minor) — UJ9.5-a never declares the session its 200 capture saves need, or when it ends, so the other
      R8.1c kinds could time E6** (UJ9.5-a; M1; R8.1c, R8.3). **fix (F41, F129 — testability):** UJ9.5-a's Given adds
      a bulk session on Scale on the Demo Device, and its When reads "…have the Demo Device save 200 sets into Scale
      while it is searched, filtered and view-sorted, then end that session (the capture PRD's R11.6); deliver 200 each
      of R8.1c's other kinds…".
- [ ] **R4-n1 (Nit) — UJ2.1-k compares E12's symbols with "its chip or history line" but opens no history view**
      (UJ2.1-k). **fix (F38 — testability):** UJ2.1-k's When also opens ZX-014's and ZX-013's version history views,
      and its Assert names never-true on ZX-014's P1 line and awaiting-answer on ZX-013's T2 line, as UJ9.6-a does.
- [ ] **R4-n2 (Nit) — R8.10's "Every readback below but R8.10f"** (R8.10). **covered by 4MN2.**
- [ ] **R4-n3 (Nit) — Traceability reads "F1–F153"**. **covered by 4N4.**
- [ ] **carried: R3-m5 (PARTIAL) — (b) carried the exemption into app-storage reads, (c)'s internal-table control is
      caught by the raw scan, and (d) states no SQL matching rule** (the Harness; UJ9.8-c). **covered by R4-B1**
      ((b), (d)) **and R4-m1** ((c)'s SQL-only control).

The review's Over-tested note (UJ3.3-f's storage half duplicating UJ9.4-b) asks for nothing.

## peer-interface-reviewer (Claude route)

- [ ] **IF4-1 (Major) — F152's "every other write waits" holds back only some writers, and Capture R1.5's "editable at
      any time" contradicts R8.1f** (R8.1f; Capture R1.3, R1.5, R1.10, R9.3 — the "Add a swatch" save, capture line
      318 — and T7; UJ9.5-g; the device PRD's saved-device changes). **fix (F155; log row 3):** F155-8 (the Capture
      rows, R9.3 among them), F155-12 (T7), F155-11 (UJ9.5-g changes Scale Two's illuminant and observer and saves an
      added swatch during the held delete). The device PRD's saved-device writes: DF R1.11 holds them if ADR-0003 puts
      saved devices in the file ("no other write to the file"); no fence names a Device mirror — reported, not made.
- [ ] **IF4-2 (Minor) — "as R8.1f states" points at text R8.1f does not hold (a re-read is not a write), and UJ9.5-g's
      Rows cite only R8.1** (the Outbound Data Foundation row; UJ9.5-g). The phrase: **covered by F166-1** (the row's
      rewrite drops it); DF R1.11 names the re-read itself (F155-1), so no hold rests on "write". UJ9.5-g's Rows:
      **fix (editorial — the Inherited-obligations cite rule):** beside R8.1, add "the Data Foundation PRD's R1.11, R1.5
      and R1.9, the import PRD's R3.2, the capture PRD's R1.1, R1.5, R3.5, R7.13 and R9.3" (journeys only).
- [ ] **IF4-3 (Minor) — the compaction undid round 3b's seam-symmetry fix** (the Outbound Data Foundation row; DF's
      inbound line). **covered by 4MN5.**
- [ ] **IF4-4 (Minor) — E10's "collection" variant, and the E6 it now triggers, render on surfaces the index does not
      name** (the Surfaces preamble and table; the copy index's E6 and E10 rows; UJ1.3-d, UJ1.3-g). **fix (F167):**
      F167-1 to F167-4.
- [ ] **IF4-5 (Minor) — F139's outside-reader half has no row, copy state or case** (R8.8, R8.10a). **covered by
      4MJ1.**
- [ ] **IF N1 (Nit) — R8.1f's "shown disabled until it lands" has no stated end on failure** (R8.1f; Import R3.2;
      Capture R1.1, R3.5). **covered by F155-6** (R8.1f cites DF R1.11, whose hold is "while it runs") **and F155-7,
      F155-8** (the sibling rows cite it too).
- [ ] **IF N2 (Nit) — R8.1f's "the surface meeting R8.1a–c throughout" while every R8.1c input is held** (R8.1f).
      **covered by F155-6** (its drafted cell reads "R8.1a–b").
- [ ] **IF N3 (Nit) — R8.3's "multi-item delete"** (R8.3; Capture's obligations line). **covered by PM4-2.**
- [ ] **IF N4 (Nit) — Traceability reads "F1–F153"**. **covered by 4N4.**
- [ ] **IF N5 (Nit) — the journeys quote "A Release build", the one quoted non-label** (the Harness's Phases and builds).
      **fix (editorial — the Labels rule):** drop the quote marks; check 13's direction 2 then prints nothing for it.
- [ ] **IF N6 (Nit) — the Named defaults' Build phase cites Rows "R8.10" while stating it is not a declared input** (the
      Named defaults). **fix (editorial):** its Rows cell reads "the Legend's phase rule". If the named-defaults check
      requires a row ID there, keep R8.10 and report.
- [ ] **IF N7 (Nit) — Import E40's "an import waits until it ends"** (Import E40). **covered by PM4-4.**
- [ ] **carried: IF-16 (UNRESOLVED; deferred by design) — `docs/product/README.md` line 18 still reads "queued"** (the
      product README). **fix (F56 — editorial), at the bookkeeping close**, as rounds 2 and 3 hold; nothing in this
      pass.

## peer-privacy-reviewer (Claude route)

- [ ] **PRIV4-1 (Medium) — the whole-text exemption reaches storage reads, so the storage-only privacy cases assert
      nothing** (the Harness; UJ3.3-f, UJ7.1-r, UJ9.4-d, UJ6.4-j; R8.6, R8.10e). The exemption, and the reverse
      substring risk: **covered by R4-B1.** UJ7.1-r's moments: **fix (F53, F130 — testability):** UJ7.1-r reads the
      app's own storage with the app open after UJ7.1-m's When, after a crash before reopening, and after quitting;
      none of the reads holds any of Sky Blue, Cerulean and Purple.
- [ ] **PRIV4-2 (Low) — the named storage locations miss places a build can keep content, and Saved Application State
      is unreadable raw** (the Harness's storage list; the Test-controls map's app-storage line; R8.10e). **fix (F53,
      F130 — testability; R8.10e's "wherever the build puts it"):** the Harness and the map add, in the unsandboxed
      reading, the app's ~/Library/Logs entries and its per-user temporary and cache folders under /var/folders, and in
      either reading ~/Library/Group Containers; the storage read also covers every path the app's processes create or
      write during the case, as a file-activity trace records them, the file's bytes aside; Saved Application State is
      read decoded (its data.data decrypted with the key in windows.plist beside it), or the case relaunches with
      window restoration on and reads R8.10b; and the seeded file's name and folder hold no fixture text. The
      reviewer's "verify the encryption on ADR-0006's floor" names no fence — reported for ADR-0006, not made.
- [ ] **PRIV4-3 (Low) — once the compaction removed R6.2d's carve-out sentence, R6.2a's "the app keeps beside it"
      reaches a pre-upgrade snapshot kept beside the file** (DF R6.2a, R5.8; DJ4). **fix (F165):** F165-2; DJ4's
      existing "Copy/snapshot predates delete/clear" line is its case.
- [ ] **PRIV4-4 (Info) — an outside SIGKILL writes no crash report, so "with its crash reports" can read nothing** (the
      Harness; DF R7.4). **fix (F53 — testability; R8.10e names crash reports):** the Harness's after-a-crash storage
      read follows a crash made by an in-process trap after the case's text was handled, so a crash report exists, and
      the read includes its application-specific information. The build half ("keep user text out of fatalError and
      precondition messages") is already R8.6's and R8.10e's; no row is added — reported.
- [ ] **PRIV4-5 (Info) — UJ7.1-r is in no fence's Carried by, and DF's inbound line no longer names the "index kept in
      the file" input** (F53; DF's inbound line). **fix (editorial):** F53's Carried by and map line add UJ7.1-r. The
      DF line: **covered by 4MN5** (F166).
- [ ] **PRIV4-6 (Info; pre-existing) — "Handoff" in R8.6 could be read to cover Universal Clipboard** (R8.6; UJ9.4-d).
      **fix (F172):** F172-1.
- [ ] **carried: PRIV3-2 (PARTIAL) — the storage-only case UJ7.1-r was voided by the exemption** (UJ7.1-r). **covered
      by R4-B1 and PRIV4-1.**

## peer-product-marketing-manager-reviewer (Claude route)

- [ ] **N4-M1 (Major) — "restores access only" and its fence cite left DF R7.3j, so "Choose the file again" can be
      built as a close-and-reopen that dead-ends in E22 or ends two undo promises** (DF R7.3j; R8.8). **covered by
      PM4-1.** Its own part — with a session in flight on another collection the action renders no DF E22 — lands in
      F159-3 (DJ3) and F159-4 (UJ9.7-i's third run).
- [ ] **N4-m1 (Minor) — Import E40's new variant brings back "waits" and "running"** (Import E40). **covered by
      PM4-4** (F169-1 also moves E40 to ready for alignment, as N4-m1 asks).
- [ ] **N4-m2 (Minor) — DF R3.4's appositive became ambiguous** (DF R3.4). **covered by 4MN4.**
- [ ] **N4-n1 (Nit) — E6's "do it again once the session has ended" is untested for undos** (UJ1.3-g, UJ6.3-g, UJ6.4-p;
      E6; R1.7, R4.7). **fix (F136 — testability; R4.7's "keeping its entry when refused", R1.7's window, E6's copy):**
      each of the three cases ends by declaring the session ended (the capture PRD's R11.6) and firing the undo again,
      asserting it lands — UJ1.3-g: E2 lists Gouache Set with 2 swatches; UJ6.3-g: the table lists ZX-010 and ZX-011;
      UJ6.4-p: the file holds the 11 items' seeded Family values. UJ1.3-h (F161-3) does the same.
- [ ] **N4-n2 (Nit) — a trailing clause on E6 for when an interrupted session is also held** (E6). **declined
      (F-rejection, recommendation 13):** recorded in Rejected findings; not fixed.
- [ ] **N4-n3 (Nit) — E8's closing clause is hard to parse** (E8). **fix (F170):** F170-1.
- [ ] **N4-n4 (Nit) — E34 repeats E15's unclear "before it"** (DF E15, E34). **fix (F170):** F170-2.
- [ ] *unrated* **PMM Missing — actions held on a surface where no progress shows say nothing about why** (R8.1f; DF
      R1.11). **covered by F155-1:** the owner chose "each shown unavailable" with no explanatory state (F155), as the
      interface lens's "two refusal styles, on purpose" notes; no copy is added — reported.

The review's two other Missing pointers are 4MN5's and 4N5's defects.

## peer-architecture-reviewer (Claude route)

- [ ] **ARCH4-1 (Major) — "Choose the file again" now points at the open-a-different-file path, which ends the delete
      undo on the same file and sends "Try again"'s write into a different one** (DF R7.3j; DJ3; DF F54's compaction
      paragraph). **covered by PM4-1.** Its own parts — DJ3 lines for a different file and for a pending undoable delete
      surviving — are F159-3.
- [ ] **ARCH4-2 (Minor) — the wait on outside readers reaches no row, and F152 makes it hold every write in the app**
      (R8.1, R8.1c, R8.1f, R8.1g, R8.8, R8.10a; the ADR-0003 row). **covered by 4MJ1.** Its (b) is answered by F157:
      no write waits on a read, only the wipe does, so a bulk write lands and R1.11's hold ends when it lands. Its (c)
      is the ADR-0003 input's bound on this app's own cold-load reads (F157-6).
- [ ] **ARCH4-3 (Minor) — F139's input splits an export's reads into short transactions that the Export PRD never
      agreed to** (the ADR-0003 row; Export R1.1). **fix (F158):** F158-1 to F158-6.
- [ ] **ARCH4-4 (Minor) — the one-writer rule lives in R8.1f as per-action holds with no single owner; a session can
      start during an import commit** (R8.1f; Import R3.2; Capture R3.5; DF OQ 17). **fix (F155):** the F155 boxes;
      regeneration's place in the rule is DF OQ 17's (F155-9).
- [ ] **ARCH4-N1 (Nit) — R8.3's "a multi-item delete" and R8.1g's class disagree** (R8.3). **covered by PM4-2.**
- [ ] **ARCH4-N2 (Nit) — "every other write in the app" takes in the export's CSV, Save a copy and saved-device
      records** (R8.1f). **covered by F155-1 and F155-6:** DF R1.11 reads "to the file", and R8.1f cites it.
- [ ] **ARCH4-N3 (Nit) — the seam lines differ, and DF R7.3j's move clause leaves out the hold R1.9 carries** (DF's
      inbound line; DF R7.3j). The lines: **covered by 4MN5.** The move clause: **fix (F155):** F155-5.
- [ ] **carried: ARCH3-4 (PARTIAL) — what the user sees while an edit waits reaches no row, copy state or case** (R8.8;
      DF R1.5; the ADR-0003 row). **covered by 4MJ1.**

## peer-performance-reviewer (Claude route)

- [ ] **PERF4-1 (Major) — a write held by an outside reader is "shown not yet saved" in F139 only; no row, copy state
      or case carries it, and close and quit are undecided** (R4.3, R8.8; DF R1.5, R1.10; F139). **covered by 4MJ1**
      (F157: saved at once; closing and quitting never wait; DF E35 the notice). UJ9.7-h asserts no DF E10, E15 or
      E34, which catches the SQLITE_BUSY-mapped-to-E10 build it names.
- [ ] **PERF4-2 (Major) — UJ9.5-g's session-start run targets a collection with no pending row, so a correct build
      fails it** (UJ9.5-g; UJ9.5-b; Capture R1.1). **fix (F137, F152 — testability; log row 6):** UJ9.5-g's Given
      adds "another of UJ9.5-b's collections, Scale Two, holding 100 pending items"; its keystrokes, selections,
      action listing and start go there. Folded into F155-11's UJ9.5-g rewrite.
- [ ] **PERF4-3 (Minor) — UJ9.5-d's second run never produces the read, edit and save overlap it was added for**
      (UJ9.5-d; R8.11). **fix (F139, F158 — testability):** F158-5.
- [ ] **PERF4-4 (Minor) — F139 caps this app's read transactions at the whole of R8.1c's from-input budget, leaving an
      edit waiting behind one no headroom** (the ADR-0003 row; R8.1c). **fix (F157):** under F157 the edit is saved
      without waiting on any read, so R8.1c's frame shows it saved; the ADR-0003 input keeps only the wipe's bound
      (F157-6).
- [ ] **PERF4-5 (Minor) — ADR-0003's search input leaves out R8.1c's budget, and its evidence names no machine** (the
      ADR-0003 row; DF F53). **fix (F171):** F171-1.
- [ ] **PERF4-6 (Minor) — the holds are one-way for import commits, moves and regeneration** (Import R3.2; Capture
      R3.5; DF R1.9, R3.3; DF OQ 17). **covered by ARCH4-4.** The trim it proposed is candidate 3 (template text, not
      taken without confirmation).
- [ ] **PERF4-7 (Nit) — hold other writes only while the bulk write's progress shows** (R8.1f). **declined
      (F-rejection, recommendation 13):** recorded in Rejected findings; not fixed.
- [ ] **PERF4-8 (Nit) — Capture T7 declares a bulk write with no size, so a small one lands before its actions are
      tried** (Capture T7). **fix (F137, F152, F164 — testability):** T7's Given reads a Collection Mode "Delete
      collection" of a ROWS_CEILING collection running, held until released (Collection Mode R8.10a). Folded into
      F155-12.
- [ ] **carried: PERF3-2 (PARTIAL) — its case cannot pass as declared** (UJ9.5-g). **covered by PERF4-2.**
- [ ] **carried: PERF3-7 (PARTIAL) — (c)'s overlap never happens, and F139's outside-reader half is carried nowhere**
      (UJ9.5-d; R8.8). **covered by PERF4-3 and 4MJ1.**

The review's cautions stand under the fences: BROWSE_RESPONSE_BUDGET is not lowered and no edit is exempted from the
byte rule (PERF4-4); no timeout discards an edit (PERF4-1, F157).

## Owner decisions F155–F172 — the edits each requires

Next free IDs, verified by reading the files at `930ce6a`: DF §1 holds R1.1–R1.10 (in the order R1.1, R1.8, R1.9, R1.2,
R1.6, R1.3, R1.4, R1.7, R1.5, R1.10), no retired-ID entry names R1.11 and no document cites it, so the new row is **DF
R1.11**. DF copy: E32 is retired and E33 and E34 are taken, so the new state is **DF E35**. DF Surfaces: R7.6a is
retired to Export R4.4a and R7.6b–p are taken, so the new surface row is **DF R7.6q**. Sibling fences: **DF F56** (after
F55), **Capture F74** (after F73), **Import F67** (after F66), **Export F32** (after F31). This PRD's cases: **UJ1.3-h**,
**UJ3.4-l**, **UJ6.4-q**, **UJ9.7-h** and **UJ9.7-i** are free. The capture PRD's "Add a swatch" save is **R9.3**
("Saving creates a pending row like any other…", its line 318, IF4-1's CAP:318); its E25 carries "End that session",
run by **R7.13**.

### F155 and F156 — one Data Foundation one-writer rule

- [ ] **F155-1 — DF R1.11 states the rule** (new row in DF §1, after R1.10; P0; ⌛️ Ready for Alignment). Draft:

  > While a [Collection Mode R8.1f or R8.1g] write, an [Import R3.2] commit or an R1.9 move runs, no capture
  > session starts or resumes and no re-read or other write to the file starts, each shown disabled while it runs.
  > Closing the file, switching files (R1.3) and quitting wait for it to land, its progress showing.

  The re-read is named because F152 held it and a re-read is not a write (IF4-2); "shown disabled" is F155's
  "shown unavailable" in the word R8.10b reads; "to the file" is ARCH4-N2's scope. About 61 words with its cells, 47
  at its tightest ("…, no session starts or resumes and no re-read or other write starts, each shown disabled;
  closing, switching (R1.3) and quitting wait for it, its progress showing") — tighten if the DF ledger binds, the
  meaning unchanged.
- [ ] **F155-2 — DF R1.3 cites R1.11** (DF R1.3): its closing parenthetical "([R7.3b](#file-actions); [Capture R3.5 …])"
      adds R1.11 first (+1), so a switch during an R1.11 write waits (F156). Alignment kept.
- [ ] **F155-3 — DF R1.5 cites R1.11 in place of Collection Mode R8.1f** (DF R1.5): "…and while a [Collection Mode
      R8.1f] bulk write or delete runs" → "…and as R1.11 states" (−7). Its help-docs clause is F157-5's. Alignment kept.
- [ ] **F155-4 — DF R1.9 cites R1.11 in place of Collection Mode R8.1f** (DF R1.9): "(disabled during
      active/paused/halted capture and while a [Collection Mode R8.1f] bulk write or delete runs)" → "(disabled during
      active/paused/halted capture and as R1.11 states)" (−7); the move is itself one of R1.11's writes, stated there.
      Alignment kept.
- [ ] **F155-5 — DF R7.3j's move clause cites R1.11** (DF R7.3j; ARCH4-N3): "move is disabled during
      active/paused/halted capture, enabled after interruption" → "…enabled after interruption, and held as R1.11 states"
      (+5). Lowest priority in the DF ledger: R7.3j already runs "R1.9's move", which F155-4 holds; if the ledger binds,
      skip it and report.
- [ ] **F155-6 — this PRD's R8.1f cites R1.11 instead of restating it** (R8.1f; R8.1g inherits it through "as R8.1f").
      Its cell becomes:

  > progress shown unless the write is done within BROWSE_RESPONSE_BUDGET, the surface meeting R8.1a–b throughout,
  > the Data Foundation PRD's R1.11 holding other writes and sessions, the write done within BULK_WRITE_BUDGET

  The row goes from 67 to 48 words (−19; candidate 4). This closes IF N1 (R1.11's hold is "while it runs"), IF N2
  ("R8.1a–b": every R8.1c input is a write R1.11 holds) and ARCH4-N2 ("to the file", in R1.11). The inherited-
  obligation marker goes with the restatement: the rule is DF's own row, and the Outbound Data Foundation row
  records it as made in this change (F166-1). The R8.1 family reads pre-alignment.
- [ ] **F155-7 — Import R3.2 cites R1.11 and adds the reverse ordering; R3.8i follows** (Import R3.2, R3.8i). R3.2's
      last clause "while a [Collection Mode R8.1f] bulk write or delete runs, the commit shows disabled until it lands
      (F66)" → "the commit follows [DF R1.11]'s one-writer rule, shown disabled while another write it names runs and
      holding every other write and a session's start or resume while it commits (F66, F67)". R3.8i (E40's "End that
      session") adds "; shown disabled while a [DF R1.11] write runs" — Import E40's "End that session" follows the
      rule (PM4-3). Both keep their alignment; R3.2 stays at two sentences.
- [ ] **F155-8 — the capture rows cite R1.11** (Capture R1.1, R1.3, R1.5, R1.10, R3.5, R7.13, R8.5, R9.3). Each keeps its
      alignment and two sentences (Capture F45), its Commit PR cell adding "amended 2026-09-25 (F74), alignment kept":
  - R1.1: "…and none is created while a Collection Mode bulk write or delete runs, creating one shown disabled until
    it lands ([its R8.1f], F73)" → "…and none is created while [Data Foundation R1.11] holds writes, creating one
    shown disabled (F73, F74)".
  - R1.3: "It is changeable between sessions; …" → "It is changeable between sessions, never while [Data
    Foundation R1.11] holds writes; …".
  - R1.5 (its "editable at any time" qualified): "…never required at creation and editable at any time; …" →
    "…never required at creation and editable at any time but while [Data Foundation R1.11] holds writes; …".
  - R1.10: "…and changeable between sessions." → "…and changeable between sessions, never while [Data Foundation
    R1.11] holds writes."
  - R3.5: "…and while a Collection Mode bulk write or delete runs no session starts or resumes, each shown disabled
    until it lands ([its R8.1f], F73)" → "…and while [Data Foundation R1.11] holds writes no session starts or
    resumes, each shown disabled (F73, F74)" — now also while an import commit or a move runs (4MJ2).
  - R7.13 (E25's "End that session"): "An interrupted session can instead be ended from the collection without
    opening capture, …" → "…without opening capture, never while [Data Foundation R1.11] holds writes, …".
  - R8.5: "…; it is offered only on a row not yet settled." → "…; it is offered only on a row not yet settled, and
    never while [Data Foundation R1.11] holds writes." R8.15 settles "under R8.5's terms" and needs no cite of its
    own — check and report.
  - R9.3 (the "Add a swatch" save): "Saving creates a pending row like any other, …" → "Saving, never while [Data
    Foundation R1.11] holds writes, creates a pending row like any other, …".
- [ ] **F155-9 — DF OQ 17's question adds where regeneration sits in the rule** (DF OQ 17): "Regeneration while a run is
      incomplete or another settings change arrives" → "…arrives, and where a regeneration run sits in R1.11's one-writer
      rule" (+9); Feeds add R1.11. Decision and Interim cells unchanged.
- [ ] **F155-10 — the obligation lines on both sides** (this PRD's Outbound and Inbound tables; DF's two obligation
      tables):
  - This PRD's Outbound Data Foundation row names "its R1.11 one-writer rule" in place of "its R1.5 and R1.9
    holding a re-read and a move as R8.1f states" — drafted whole in F166-1.
  - This PRD's Outbound Capture Mode row: "its R1.1 and R3.5 hold collection creation and a session's start and
    resume as R8.1f states" → "its writes and session starts follow the Data Foundation PRD's R1.11" (16 → 11
    words; candidate 5, PM4-3's reword under F155); Rows unchanged.
  - This PRD's Outbound Inventory Import row: drafted in F166-3.
  - This PRD's Inbound table gains, after the "Its R1.1" Data Foundation line: "| Data Foundation | Its R1.11: the
    one-writer rule | the Data Foundation PRD's R1.11 → R8.1c, R8.1f |" (+14) — the rule also holds this PRD's
    own writes while an import commit or a move runs.
  - DF's inbound Collection Mode line: "R1.5 and R1.9 holding a re-read and a move while its bulk write or delete
    runs ([its R8.1f])" → "R1.11's one-writer rule, which its R8.1f cites"; Rows add R1.11; its other edits are
    F166-2's.
  - DF's outbound table ("What this PRD imposes on others"): the Collection Mode and Capture Mode lines add
    "R1.11's one-writer rule" and R1.11 to Rows; a new line "| Inventory Import | R1.11's one-writer rule, its R3.2
    commit among the writes | R1.11 |". About +17 DF words; if the DF ledger binds, report these first — the
    citing sibling rows carry that side.
  - Capture's own Collection Mode obligation line is F161-2's; F155 adds nothing there, the rule being DF's.
- [ ] **F155-11 — UJ9.5-g** (journeys). Given adds "another of UJ9.5-b's collections, Scale Two, holding 100 pending
      items (PERF4-2), and a third, Scale Three, holding an unresumed interrupted bulk session". The collection-delete
      run sends its keystrokes, selections, action listing and the capture PRD's start to Scale Two. The further run, in
      a test build, holds the collection delete running through R8.10a's first input (F164) and, while it is held:
      lists Scale Two's actions and tries the capture PRD's start there; fires the preview's import commit of IM-001 and
      IM-002 into Scale Two, the Data Foundation PRD's E9 read-again action and "New collection" (as now); changes Scale
      Two's samples per row and its illuminant and observer through the capture PRD's settings; saves an added swatch
      into Scale Two through the capture PRD's Add a swatch; and fires the capture PRD's E25 end action on Scale
      Three — each shown disabled, nothing starting, changing, being created or ending; once released and landed, each
      is offered. A second held run holds instead that import commit (the reverse ordering, 4MJ2): during it, "Set a
      field" on Scale Three, the capture PRD's start on Scale Two and its E25 end action on Scale Three each show
      disabled and nothing starts; once it lands, each is offered. A third held run, in separate sub-runs during the
      held collection delete, closes the file, opens another file (the Data Foundation PRD's R1.3) and quits: each
      waits with the write's progress showing (R8.10b) and goes ahead only once the delete lands, the file then read
      with the app closed holding no Scale item — the confirmed delete stands (F156). Sibling actions are named by
      document, unquoted (check 13). Rows: F155-10's and IF4-2's. The timing runs stay unheld, in a Release build.
- [ ] **F155-12 — Capture T7** (capture journeys; PERF4-8, F164). Given: "a Collection Mode "Delete collection" of a
      ROWS_CEILING collection running, held until released ([Collection Mode R8.10a]), and in turn an import commit and
      a file move held the same way; another collection with pending rows, and a third holding an unresumed interrupted
      bulk session". When adds: change the second's samples per row, illuminant and observer, and scan mode; save an
      "Add a swatch" item into it (in R9's phase); fire "End that session" on the third's resume offer — each while the
      write is held, then again once released. Assert: all show disabled and nothing starts, resumes, changes, is
      created or ends; once it lands all are offered (F74). Rows: R1.1/R1.3/R1.5/R1.10/R3.5/R7.13/R9.3.
- [ ] **F155-13 — DF DJ3** (DF journeys): the line "No capture in flight; a Collection Mode bulk write or delete running
      (its R8.1f and R8.1g)" generalizes to "No capture in flight; in turn a Collection Mode bulk write or delete, an
      import commit and a move running, each held until released ([Collection Mode R8.10a])"; its Action adds opening
      another file, closing the file and quitting; its oracle: "While it runs Read it again and the move show disabled
      and nothing is re-read or moved; opening another file, closing and quitting each wait, progress showing, and go
      ahead once it lands, the write whole (R1.11, R1.3, F56)". One line or two, in DJ3's style.
- [ ] **F155-14 — Import UJ 2.1** (import journeys): the line "No session anywhere; a Collection Mode bulk write or
      delete running on another collection (its R8.1f and R8.1g)" becomes "…a Collection Mode bulk write or delete, or a
      Data Foundation move, running, held until released ([Collection Mode R8.10a])" (F67); and a new line: "No session
      anywhere; this import's commit held running ([Collection Mode R8.10a]) | Try a capture start on another
      collection, Collection Mode's Set a field and the Data Foundation re-read while it commits; then again once it
      lands | While it commits each shows disabled and nothing starts or is written; once it lands each is offered
      (F67) | R3.2, R4.1".
- [ ] **F155-15 — the sibling fences** (DF F56, Capture F74, Import F67):
  - **DF F56** (new, under a "## Collection Mode round-4 amendment (2026-09-25)" heading as F52–F54 have):
    Authority — the Collection Mode PRD's F155–F158, F166, F170 and F171 (owner decisions D36–D39 of its round-4
    adjudication, and its approved round-4 recommendations 6, 10 and 11). Decision — (1) F155, F156: R1.11, with
    R1.3, R1.5, R1.9 and R7.3j citing it and OQ 17's question widened; (2) F157: R6.2a's deferred wipe, R2.3 citing
    it, E35 and R7.6q, and R1.5's help-docs line, narrowing F53 (2); (3) F158: an export's read is one snapshot,
    among the reads R6.2a names; (4) the ADR-0003 inputs F157-6 and F171-1 name; (5) F166: the inbound line's R2.9
    phrase mended; (6) F170: E15 and E34 read "before that". R1.3, R1.5, R1.9, R2.3, R6.2a and R7.3j keep their
    alignment; R1.11, R7.6q and E35 are ready for alignment. Rows. Peer review pending. F159 and F165 land as
    dated lines under DF F54 (F159-6, F165-3); F160 under DF F55 and F21 (F160-1).
  - **Capture F74** (new): Authority — the Collection Mode PRD's F155, F156 and F161. Decision — the rows cite
    Data Foundation R1.11 (F155-8), T7 asserts it (F155-12), and the Collection Mode obligation line reads "a
    selection or collection delete" (F161-2); the rows keep their alignment. Carried by: R1.1, R1.3, R1.5, R1.10,
    R3.5, R7.13, R8.5, R9.3, T7, Collection Mode obligation line, Traceability. A dated line under F73: its (2)'s
    hold now lives in Data Foundation R1.11 (F74). F74's map line; Traceability "F1–F74".
  - **Import F67** (new): Authority — the Collection Mode PRD's F155, F156 and F169. Decision — R3.2 and R3.8i
    follow Data Foundation R1.11, R3.2 adding the reverse ordering; E40's another-collection variant and shared
    append are reworded and E40 moves to ⌛️ Ready for Alignment (F169-1); the UJ 2.1 lines (F155-14). A dated line
    under F66: its **Not decided** point's settlement now cites Data Foundation R1.11. F67's map line; the
    Traceability paragraph adds "F67 mirrors its F155, F156 and F169 (2026-09-25)".
      Status-line clauses: see Checks.
- [ ] **F155-16 — the Test-controls map** (journeys): the "sibling states opened here" line adds the capture PRD's
      settings changes, its Add a swatch save and E25's end action, the Data Foundation PRD's move, opening another
      file, closing the file and quitting, and its E35; its Rows add R8.8. The "the file" line is F164-4's.

### F157 — an edit shows saved; a wipe an outside read holds up waits, said

- [ ] **F157-1 — the rule's Data Foundation home: R6.2a, R2.3 citing it** (DF R6.2a, R2.3). F157 allows a new row or an
      amendment of R6.2a or R1.10. R1.10 is at two sentences, and a new row costs about 55 DF words against about 31
      here, so under F160's no-trim budget the pass amends R6.2a. Its oracle "…from the moment the delete lands, while
      the file is open and after a crash" becomes:

  > …from the moment the delete lands — or, while a read begun before it still runs, another app's or an [Export
  > R1.1] snapshot's, from when that read ends or the file next opens, nothing waiting on it and E35 naming
  > another app's — while the file is open and after a crash

  (+27; F165-2 adds "R5.8's copies aside" in the same cell). R2.3's "…from the moment the write lands, while the
  file is open…" → "…from the moment the write lands, deferred as R6.2a states, while the file is open…" (+4). R6.2
  and R6.2d already say "as R6.2a states". If the orchestrator prefers a row, it is DF R1.12 (next free after
  R1.11), R2.3 and R6.2a citing it — report the choice either way.
- [ ] **F157-2 — DF E35, the notice — a draft for the lenses** (DF copy; DF Surfaces; this PRD's copy file and Build
      dependencies). Drafted from F157's own words ("the removed text stays in the file until the other app stops
      reading it, and the app then wipes it on its own, or when the file next opens"):

  > | E35 | Removed text waiting on another app | Saved — another app is reading your file | Your change is saved.
  > What it removed stays in your file until the other app stops reading it; SpectroCapture then wipes it on its
  > own, or when you next open the file. | OK | ⌛️ Ready for Alignment |

  It renders while another app's read defers a wipe (R6.2a), never for this app's own export, and goes when the
  wipe lands. DF Surfaces gains "| R7.6q | Wipe-pending notice | That the change is saved, what stays until the
  other app's read ends, and the action | R6.2a | E35 | ⌛️ Ready for Alignment |" (+24). This PRD's copy file's
  sibling-owned list adds the Data Foundation PRD's E35; Build dependencies row 1's "the Data Foundation PRD's E10,
  E15 and E34" → "…E10, E15, E34 and E35" (+1 body word).
- [ ] **F157-3 — R8.8's byte clause** (R8.8): "…and text it removes is nowhere in the file's bytes from the moment it
      lands, open or after a crash (the Data Foundation PRD's R1.10 and R6.2a; …)" → "…from the moment it lands, open
      or after a crash, unless an earlier read defers it (the Data Foundation PRD's R1.10 and R6.2a; …)" (+6). "Lands"
      stays "saved" (F157). R8.8 reads pre-alignment. The deleted state's Vocabulary line cites R6.2a and needs no
      change.
- [ ] **F157-4 — the F102 carriers follow** (F102's Carried by: R1.3, R8.8, R8.10d, T1, T5, T6, UJ1.3-c, UJ4.2-a,
      UJ4.2-c, UJ4.4-b, UJ6.3-b, UJ9.8-c, DF R2.3, DF R6.2a, DF F53). R8.8 is F157-3; DF R2.3 and R6.2a are F157-1. R1.3,
      R4.3 and R6.2 state what is gone, not when, citing the DF rows that now carry the deferral — unchanged. R8.10d
      unchanged. The cases declare no outside read, so their oracles hold, but this app's own short reads can defer a
      wipe too, so the Harness's Byte checks add: "At the open-app moment the check reads once BROWSE_RESPONSE_BUDGET
      has passed after the write lands — the bound the ADR-0003 input puts on this app's own reads, a read begun before
      a write deferring its wipe (the Data Foundation PRD's R6.2a); a case declaring a read held open from outside the
      app (R8.10a) reads at the moments its When names." DF F56 (2) records F53 (2)'s narrowing.
- [ ] **F157-5 — DF R1.5's help-docs line** (DF R1.5): "the help docs say reading it elsewhere is safe, though during
      capture it may delay saves, and editing it while the app is open is not" → "the help docs say reading it
      elsewhere is safe, though it may delay wiping removed text (R6.2a), and editing it while the app is open is not"
      (+1). No save waits on a read (F157).
- [ ] **F157-6 — the ADR-0003 input from F139, rewritten** (`docs/decisions/README.md`, the 0003 row; with F158 and
      F171), on the orchestrator's say-so as rounds 1b and 3 had it:
  - the byte input: "…from the moment its write lands, while the file is open and after a crash" → "…from the
    moment its write lands, while the file is open and after a crash — or, while a read begun before that write
    still runs, another app's or an export's, from when that read ends or the file next opens";
  - the removing-write input: "a text-removing write lands once every earlier read has ended, this app's own reads
    (an export, the All items view's cold load) running in transactions no longer than Collection Mode's
    BROWSE_RESPONSE_BUDGET, so capture saves never wait on a removing edit" → "a text-removing write is saved
    without waiting on any read, only its wipe following the end of every earlier read, and this app's own
    cold-load reads (the All items view's) run in transactions short enough that the wipe follows within Collection
    Mode's BROWSE_RESPONSE_BUDGET, so capture saves never wait on a removing edit" — "an export" dropped (F158); the
    bound PERF4-4 asks for is F139's number, reused, not a new one;
  - the fence list "[its F35, F50, F85, F102, F103, F127, F139 and F146]; [Data Foundation F52, F53 and F54]" adds
    F157, F158 and F171, and DF F56; still eight inputs;
  - the search input is F171-1's.
- [ ] **F157-7 — the outside-read input and its case** (R8.10a; UJ9.7-h). R8.10a's input is F164-1. New case **UJ9.7-h**
      (R4-m5's number): Given — the seeded file; a test build; a read held open on the file from outside the app
      (R8.10a), begun before the When. When — open ZX-001, set Swatch Name to Harbour and press Return; list the detail
      and the sibling state up; release the outside read; in a second run, quit instead, then release the read and
      reopen. Assert — at once the detail shows Harbour, no write shows progress or waits (R8.10b), none of the Data
      Foundation PRD's E10, E15 and E34 renders, and its E35 is up; within 5 s of the release, a functional timeout,
      E35 is not up and the file's bytes hold the text Sky Blue nowhere; in the second run the app quits within 5 s
      without waiting, and after reopening the bytes hold Sky Blue nowhere. Rows R8.8, R8.10. The case's When names its
      read moments, as the Harness allows.
- [ ] **F157-8 — a Data Foundation case** (DF DJ4): a line "A read begun before a delete or a clear, made from outside
      the app, still running | Delete (or clear); then end the read — and separately quit and reopen | The delete lands
      and shows done at once, nothing refused; E35 is up while the read runs; once it ends, or after reopening, the
      removed text is in the file's bytes nowhere; closing and quitting never wait (R6.2a, F56) | E35".
- [ ] **F157-9 — reported, not changed** (copy honesty under F157): E8's "What was there isn't kept in your file" speaks
      of undo storage (R4.7), and DF E8, E14 and E33's finality lines speak of undo; no fence rewords them. The round-5
      lenses re-check them against R6.2a's deferral.

### F158 — an export is one snapshot

- [ ] **F158-1 — Export R1.1 and Export F32** (Export R1.1; its fences, map line and status line). R1.1's second
      sentence "Export only reads: every source value, mark and note remains identical at SQLITE_READER_FLOOR after
      success or failure (R4.3)." → "Export only reads, taking one snapshot, the file as it stood when the export
      started — an edit made while it runs saves at once and is not in it — and leaves every source value, mark and
      note identical at SQLITE_READER_FLOOR after success or failure (R4.3)." (about +20 Export words; Export stands at
      3,656 of its 4,000). Alignment kept, its Commit PR cell adding "amended 2026-09-25 (F32), alignment kept". This
      PRD's Build contract ("the Data Export PRD owns what an export contains once it is started") stays as it is.
      Export **F32** (new): Authority — the Collection Mode PRD's F158 (owner decision D39); Decision — an export is
      one snapshot; an edit made meanwhile saves at once and is not in it, the text it removed wiped when the export
      ends (Data Foundation R6.2a, DF F56); peer review pending. Map line: "| F32 | R1.1; EJ1; the Collection Mode
      inbound line |".
- [ ] **F158-2 — the obligation lines** (this PRD's Outbound table; Export's inbound table). No Outbound Data Export row
      exists at `930ce6a`; add one after Inventory Import: "| Data Export | Made in this change: its R1.1 reads one
      snapshot, the file as it stood when the export started | R1.8, R8.8 |" (+19). Export's "What other PRDs impose
      on this one" gains "| Collection Mode | An export reads one snapshot, the file as it stood when it started ([its
      F158]) | R1.1 |".
- [ ] **F158-3 — Export EJ1** (Export journeys): a line "Export running on a collection; one of its items edited while
      it runs | Export; make the edit mid-export | The export's rows are the collection as it stood when it started,
      the edited item's old value among them; the edit saves at once; once the export ends, the text the edit removed
      is in the file's bytes nowhere (R1.1, DF R6.2a)".
- [ ] **F158-4 — the ADR-0003 input drops "an export"**: in F157-6.
- [ ] **F158-5 — UJ9.5-d's export overlap run** (UJ9.5-d; PERF4-3): the second run also fires "Export collection" on
      another of UJ9.5-b's collections at each set's first Demo sample, so the export's one-snapshot read spans the
      edits and the saves; the R8.11 asserts stay; Rows add R1.8.
- [ ] **F158-6 — DF R6.2a names the export's read**: in F157-1.

### F159 — choosing another file in E34's picker changes nothing

- [ ] **F159-1 — DF R7.3j** (DF R7.3j): "…/ run R7.3d's file-selection path for the file, E34 staying up for Try again
      and no write retried / …" → "…/ run R7.3d's file-selection path for the file, which restores access only,
      another file chosen opening, switching and retrying nothing and the file staying open, E34 staying up for Try
      again and no write retried / …" (+13). Alignment kept.
- [ ] **F159-2 — DF R7.6p** (DF Surfaces): "…and that Choose the file again retries no write" → "…that Choose the file
      again retries no write, and that another file chosen changes nothing" (+7). If the DF ledger binds, DJ3 alone
      pairs R7.3j — report.
- [ ] **F159-3 — DF DJ3 lines** (DF journeys), after the same-file line: "File open, the app's permission lost, a P1
      delete pending undo | Change an item's metadata; choose Choose the file again and pick a different file | E34 stays
      up; the open file stays open and nothing is written to either file; the pending delete is still undoable (R7.3j,
      R6.3, F54) | E34"; and the same-file line run again with a session in flight on another collection, asserting
      E22 does not render (R7.3j).
- [ ] **F159-4 — this PRD's case beside UJ9.7-g** (PM4-1, N4-M1, ARCH4-1): new **UJ9.7-i**. Given — the seeded file; R1.7
      and R4.7 built; Studio Markers searched zx-0. When — delete ZX-010 through the Data Foundation PRD's E8; then
      clear Family on ZX-001 and ZX-002 through "Set a field"; declare the app's permission to the file revoked
      (R8.10a); set ZX-003's Swatch Name to Magenta and press Return; fire the Data Foundation PRD's E34
      choose-the-file action and, in separate runs: (a) pick the same file, declare the permission restored, then fire
      E34's try-again action; (b) pick a copy of the file kept elsewhere; (c) as (a), with a bulk session on Gouache Set
      in flight. Assert — (a) E34 stays up until the try-again, the file then holds Magenta for ZX-003, and "Undo
      change" and E10's "Undo" are still offered and the search field holds zx-0; (b) E34 stays up, the collection list
      and the file are the seeded file's, neither file changes, and both undos are still offered; (c) the Data
      Foundation PRD's E22 does not render. Rows R8.8, R4.7, R1.7. (The delete comes before the clear because R4.7's
      history ends on any other committed write.)
- [ ] **F159-5 — correct DF F54's compaction paragraph** (DF fences): the "Editorial compaction 2026-09-25" paragraph
      stays as written; a dated line under it: "**Corrected 2026-09-25 ([the Collection Mode PRD's F159 and F165]):**
      the compaction changed meaning in two rows despite the sentence above — R7.3j lost 'which restores access only'
      (F151), and R3.4's appositive came to name sRGB's white rather than the adaptation (F146, (3) above); both are
      restored, R7.3j under the F159 line below and R3.4 under the F165 line. Peer review pending."
- [ ] **F159-6 — a dated line under DF F54 for F159**: "**Clarified 2026-09-25 ([the Collection Mode PRD's F159], owner
      decision D40):** Choose the file again restores access only, as the F151 line above settled and the compaction
      dropped from R7.3j; a different file chosen in its picker opens nothing, switches nothing and retries nothing, E34
      staying up and the open file open. R7.3j says so, R7.6p lists it and DJ3 asserts it; R7.3j keeps its alignment.
      Peer review pending." DF F54's map line adds R7.6p's and DJ3's clarification.

### F160 — the Data Foundation PRD's word budget is 8,300

- [ ] **F160-1 — the budget lines** (DF fences): a dated line under DF F55: "**Clarified 2026-09-25 ([the Collection
      Mode PRD's F160], owner decision D41):** the budget is 8,300 words — a second owner override of the agent-PRD
      format's never-raise rule, recorded here, so the round-4 rule text lands without further trimming; the body stood
      at 8,164 when it was raised."; a dated line under DF F21, as F154's was: "…the budget is 8,300 words; see F55.";
      the map line for F55 reads "(the word budget, 8,300 since 2026-09-25)". The DF body states no budget (verified:
      no "8,200" in `prd-data-foundation.md`); F1's map line still reads "8,000 since 2026-09-14" — report it, as round
      3 left it.

### F161 — which delete's undo R8.3 refuses in flight

- [ ] **F161-1 — R8.3** (R8.3): "E10's "Undo" of a multi-item delete" → "E10's "Undo" of a selection or collection
      delete" (+2). R8.3 reads pre-alignment. Reported, not changed: E6 "elsewhere" calls what it refuses "changes that
      touch many swatches at once", loose for an empty or one-item collection's undo; no fence rewords it.
- [ ] **F161-2 — the capture PRD's obligations line** (Capture's Collection Mode obligation line): "…and the undo of a
      bulk set or clear or of a multi-item delete —" → "…or of a selection or collection delete —"; recorded in Capture
      F74 (F155-15). F72's and F73's dated lines are history and stay.
- [ ] **F161-3 — case UJ1.3-h** (journeys): Given — the seeded file plus a collection Inks holding one item, IK-001,
      Cyan, pending; R1.7 built. When — choose Inks, fire "Delete collection" and the Data Foundation PRD's E14 delete
      action; bring a bulk session on Studio Markers in flight, in each in-flight state; fire "Undo" on E10; declare the
      session ended (the capture PRD's R11.6) and fire "Undo" again; in a second run Inks holds no item. Assert — E6
      renders its "elsewhere" variant naming Studio Markers, on the collection list, and E2 does not list Inks; after
      the session ends the undo lands and E2 lists Inks. Rows R1.7, R8.3. The UJ 1 preamble's "UJ1.3-d to UJ1.3-g" →
      "UJ1.3-d to UJ1.3-h".

### F162 — R8.10d and R8.10e are outside reads

- [ ] **F162-1 — R8.10** (R8.10): "Every readback below but R8.10f exists only in test builds, …" → "R8.10b and R8.10c
      exist only in test builds, …" (−2; F162's "all but R8.10d–f", R8.10a being inputs, not readbacks). The F132
      dated line is already there. The R8.10 family reads pre-alignment. UJ9.4-b, UJ9.4-d and UJ9.4-e unchanged.

### F163 — a write the app makes by itself never ends "Undo change"

- [ ] **F163-1 — R4.7** (R4.7): "until any other committed write, …" → "until any other write the user commits, …"
      (+2); "no write a capture session makes counting" stays. F163's example, the Data Foundation PRD's damage mark
      saved on reading, goes in the case, not the row. R4.7 reads pre-alignment. E8 needs nothing: "you do anything
      but…" already names only the user's actions.
- [ ] **F163-2 — case UJ6.4-q** (journeys): Given — the seeded file plus ZX-017 as UJ4.1-e declares it, its
      archive-unavailable mark not yet in the file (the Data Foundation PRD's R7.2); R4.7 built. When — set ZX-001's
      Swatch Name to Harbour and press Return; open ZX-017, so the app reads its archive and marks it (the Data
      Foundation PRD's R5.5a); choose Studio Markers, list the actions offered and fire "Undo change"; quit and read the
      file. Assert — "Undo change" is offered, and afterwards the file holds Sky Blue for ZX-001; read with the app
      closed, the file holds ZX-017's archive-unavailable mark. Rows R4.7. If no declared step makes the app persist a
      damage mark after the edit, report the case as unwritable rather than inventing a trigger.

### F164 — two test-build inputs

- [ ] **F164-1 — R8.10a's two inputs** (R8.10a): append "; and in a test build a bulk write, delete, import commit or
      move held running until released, and a read held open on the file from outside the app" (+26). The first input
      names the import commit and the move beside F164's "bulk write or delete" so that R1.11's holds on them (F155)
      are testable; that widening is a seam grant beyond F164's literal text — editorial/testability, no product rule —
      and is reported. Without it, the import-commit and move runs in UJ9.5-g, T7, DJ3 and UJ 2.1 have no held Given,
      R4-M1's defect again. The R8.10 family reads pre-alignment.
- [ ] **F164-2 — UJ9.5-g's hold checks use the first input**: in F155-11; its timing runs stay unheld.
- [ ] **F164-3 — the sibling Givens cite it** (Capture T7; the DF DJ3 line; the Import UJ 2.1 lines): each declares its
      running write "held until released ([Collection Mode R8.10a])" — in F155-12, F155-13 and F155-14. Capture R11.6's
      "a failing commit" and DF R7.2's "write in flight" do not grant it.
- [ ] **F164-4 — the Test-controls map** (journeys): the "the file" line's controlled input adds "a bulk write, delete,
      import commit or move held running until released, and a read held open on it from outside the app (R8.10a)",
      its observable "whether a write's progress shows, which actions are offered or disabled, and the Data Foundation
      PRD's E35"; Rows add R8.1. The F157 case uses the second input (F157-7).

### F165 — the Data Foundation PRD's R3.4 and R6.2a wording

- [ ] **F165-1 — DF R3.4** (DF R3.4): "…after Bradford adaptation to sRGB's white, the one sRGB derivation uses, …" →
      "…after Bradford adaptation to sRGB's white, the adaptation its sRGB derivation uses, …" (+1). Alignment kept.
- [ ] **F165-2 — DF R6.2a** (DF R6.2a): "…any journal, log, index or other file the app keeps beside it, …" → "…the app
      keeps beside it, R5.8's copies aside, …" (+4), in the same cell as F157-1's clause.
- [ ] **F165-3 — a dated line under DF F54**: "**Clarified 2026-09-25 ([the Collection Mode PRD's F165], its approved
      round-4 recommendation 5):** R3.4 names the adaptation its sRGB derivation uses, as (3) above states and the
      compaction had blurred, and R6.2a leaves R5.8's copies aside from the files the app keeps beside the file. Both
      keep their alignment. Peer review pending."

### F166 — the seam lines match

- [ ] **F166-1 — this PRD's Outbound Data Foundation row takes DF's compact form** (the Outbound Data Foundation row;
      candidate 1). Its Obligation cell becomes:

  > Made in this change, as its F52–F56 record: its E8, E11, E15, E26, E34 and E35 wording, E34's actions as its
  > R7.3j states; its R1.2's identity under a new code and column visibility, R2.3's and R6.2a's byte rule and its
  > deferred wipe, R3.4's own flag test and R1.5's help docs; its R1.11 one-writer rule; its OQ 20 sizing an
  > undoable ceiling delete; and the ADR-0003 and ADR-0007 inputs those fences list

  Rows unchanged (R1.3, R1.7, R2.4b, R2.5, R2.10, R4.2h, R4.3, R4.4, R4.5, R5.7, R6.2, R8.1f, R8.2, R8.8, R8.11).
  125 → about 88 words (−37 net). It drops "as R8.1f states" (IF4-2) and the search-input restatement (R4-M1).
- [ ] **F166-2 — DF's inbound Collection Mode line** (DF Inherited obligations): "R2.9's Flag removal for its restore of
      a flagged item ([its R5.5])" → "R2.9 naming the Flag as removing a current value, for its restore of a flagged
      item ([its R5.5])"; "the E8, E11, E26, E33 and E34 wording" → "the E8, E11, E15, E26, E33, E34 and E35 wording";
      F155-10's R1.11 clause; "R2.3's and R6.2a's byte rule" → "…byte rule and its deferred wipe"; "F52–F54" →
      "F52–F56"; Rows add R1.11, R7.6q and E35 (about +8 DF words). The two lines then state the same set, E34's
      actions by R7.3j and the ADR inputs by the fences.
- [ ] **F166-3 — this PRD's Outbound Inventory Import row** (the Outbound table): "Made in this change: its R3.2 holds
      a commit as R8.1f states" → "Made in this change: its R3.2 refuses a commit while any session is in flight and
      follows the Data Foundation PRD's R1.11" (+10); Rows R8.1f.

### F167 — where E10 and E6 render

- [ ] **F167-1 — the Surfaces preamble**: "E6 by the collection surface, the item detail and the version history view;
      and E10 by the collection surface and the All items view" → "E6 by the collection list, the collection surface,
      the All items view, the item detail and the version history view; and E10 by the collection list, the collection
      surface and the All items view" (+10).
- [ ] **F167-2 — the Surfaces table**: the collection list's states "E1, E2" → "E1, E2, E6, E10"; the All items view's
      "E13, E4, E5, E9, E10, E12" adds E6 (+3).
- [ ] **F167-3 — the copy index**: E6's Surface "Collection surface; item detail; version history view" → "Collection
      list; collection surface; All items view; item detail; version history view"; E10's "Collection surface; All
      items view" → "Collection list; collection surface; All items view" (+7). E6 and E10 read pre-alignment, in the
      PRD and the copy file.
- [ ] **F167-4 — the Test-controls map**: the collection list line's observable adds "E6 and E10 with their variants
      and token values"; the All items view line adds E6. UJ1.3-d and UJ1.3-g read E10 and E6 on the collection list as
      they already do, and UJ1.3-h likewise.

### F168 — returning to E9 whose item is gone

- [ ] **F168-1 — R4.1**: "closing it returns to E9, worked out again, when opened from E9, otherwise to the table…" →
      "…when opened from E9 while E9's item remains, otherwise to the table…", and the second sentence drops ", the
      table showing if E9's item is gone," which "as closing does" now covers (−4 net). R4.1 reads pre-alignment.
- [ ] **F168-2 — case UJ3.4-l** (journeys): Given — Blues as UJ3.4-a declares it. When — open FS-000, fire "Find
      similar" and choose FS-002 in E9; then, outside the app, remove FS-000 from the file; fire the Data Foundation
      PRD's E9 read-again action; close FS-002's detail. Assert — E9 is not up, and the table lists Blues' items without
      FS-000. Rows R4.1, R8.5.

### F169 — the import PRD's E40 another-collection variant

- [ ] **F169-1 — Import E40** (Import copy; Import F67; Import status): E40's Body "Another collection: A session is
      running in ⟨collection⟩, and an import waits until it ends so the session's saves aren't held up." → "Another
      collection: A session in ⟨collection⟩ is active, paused or halted. Until it ends, imports aren't available in any
      collection, so the session's saves aren't held up."; "All variants append: End or finish the session, then import
      — nothing about the file has changed." → "All variants append: End or finish the session, then start the import
      again — nothing about the file has changed."; Status "🤝 Aligned" → "⌛️ Ready for Alignment"; recorded in Import
      F67 (F155-15). Import UJ 2.1's "once that session ends, a fresh preview commits" already matches; its actions are
      unchanged, so the capture PRD's standing label check passes.

### F170 — copy wording

- [ ] **F170-1 — E8** (copy file, E8's body and its "clear" variant): "— importing and hiding or showing a column
      included; scanning doesn't count." → "— including importing, or hiding or showing a column; scanning doesn't
      count." E8 stays at pre-alignment, rewritten.
- [ ] **F170-2 — DF E15 and E34** (DF copy): "On a local disk your file is exactly as it was before it." → "…exactly as
      it was before that." in both. E15 keeps its status, reworded as E8, E11 and E26 were; E34 stays ⌛️ Ready for
      Alignment. Recorded in DF F56 (6).
- [ ] **F170-3 — E6's "full" condition** (copy file, E6): "every P1 action that variant names is built" → "every P1
      action that variant names — "Use this reading" among them — is built". R8.3 and the Harness unchanged. E6 reads
      pre-alignment.

### F171 — ADR-0003's search input

- [ ] **F171-1 — the search input** (`docs/decisions/README.md`, the 0003 row; DF F53): "…and within Collection Mode
      R8.1f's, R8.1g's and R8.2's budgets on its OQ 1 Mac" → "…within Collection Mode R8.1c's, R8.1f's, R8.1g's and
      R8.2's budgets on its OQ 1 Mac"; "its evidence is the performance lens's round-3 measurements in [the Collection
      Mode review log]" → "…round-3 measurements, on an M5 Max, in [the Collection Mode review log]". The same two
      changes land in a dated line under DF F53, whose (5) line restates the input, as round 3's ARCH3-3 did.

### F172 — what "Handoff" means in R8.6

- [ ] **F172-1 — "Handoff"** (journeys): UJ9.4-d's Assert reads "…neither system search nor Handoff — the system
      handing an activity to another device, not the user's own copy and paste (F172) — holds the text Sky Blue or
      Harbour"; the Test-controls map's app-storage line likewise. R8.6 unchanged. The F130 dated line is already there.

## Checks

- [ ] **Testability pairing (process rule 3).** Every row, state and metric changed above has its acceptance case and
      its seam line (R8.10, the Harness, the Named defaults, the Test-controls map) in the same pass: R4.1 → UJ3.4-l;
      R4.7 → UJ6.4-q; R8.1f → UJ9.5-g's held runs; R8.3 → UJ1.3-h; R8.8 → UJ9.7-h, UJ9.7-i; R8.10 → UJ9.4-e (unchanged);
      R8.10a → UJ9.5-g, UJ9.7-h and the map's "the file" line; E6 and E10 → UJ1.3-g, UJ1.3-h and the map's collection
      list line; E8 → UJ6.2-b/d (unchanged). Sibling pairs: DF R1.11 → DJ3; DF R6.2a and E35 → DJ4's new line and
      R7.6q; DF R7.3j → DJ3 and R7.6p; DF R3.4 → R7.5 and R7.7f (unchanged); Capture rows → T7; Import R3.2, R3.8i → UJ
      2.1; Export R1.1 → EJ1. New cases: UJ1.3-h, UJ3.4-l, UJ6.4-q, UJ9.7-h, UJ9.7-i, and UJ9.8-c's SQL-only control run.
- [ ] **Word counts** by rule 14's method. Collection Mode body: start 11,997 at `930ce6a`, budget 12,000; each Result
      note that adds body words names its trim from the ledger; report the final count; flag any fix that cannot be
      paid for, and any template-scaffolded paragraph trimmed without the orchestrator's confirmation (candidate 3; the
      §8 preamble, the Open-questions trailer and R1.7 are not touched). Data Foundation body: start 8,164, budget 8,300
      (F160); report the final count, and stop and report under the header's stop rule if it would pass 8,300. Report
      the companions' counts, unbudgeted, and Export's body against its 4,000.
- [ ] **Meaning check on every trim** (PM's and PMM's biggest risk: a compaction called rule-free changed two rows).
      Before a trim lands, diff its removed text against the row or fence that must still state it, row by row, and
      name that row in the Result note — candidate 1 against DF R7.3j and the fences F52–F56, candidate 2 against the
      constants table and the Interim-stated list, candidate 4 against DF R1.11.
- [ ] **Fences and Carried-by.** Fill the "_(filled by the round-4 fix pass)_" Carried-by lines and map lines of F155–F159
      and F161–F172 with actual IDs in the Carried-by grammar (F160 already reads "governs no rows"). As drafted:
  - F155 — R8.1f, R8.1g, UJ9.5-g, the Data Foundation PRD R1.11, R1.3, R1.5, R1.9, R7.3j, DJ3, F56, the import PRD
    R3.2, R3.8i, F67, the capture PRD R1.1, R1.3, R1.5, R1.10, R3.5, R7.13, R8.5, R9.3, T7, F74 (each sibling ID
    preceded by its document's name, as the grammar requires);
  - F156 — UJ9.5-g, the Data Foundation PRD R1.11, the Data Foundation PRD R1.3, the Data Foundation PRD DJ3, the
    Data Foundation PRD F56;
  - F157 — R8.8, R8.10a, UJ9.7-h, the Data Foundation PRD R6.2a, R2.3, R1.5, R7.6q, E35, DJ4, F56;
  - F158 — UJ9.5-d, the export PRD R1.1, the export PRD F32, the Data Foundation PRD R6.2a, the Data Foundation PRD
    F56;
  - F159 — UJ9.7-i, the Data Foundation PRD R7.3j, R7.6p, DJ3, F54;
  - F161 — R8.3, UJ1.3-h, the capture PRD F74; F162 — R8.10; F163 — R4.7, UJ6.4-q;
  - F164 — R8.10a, UJ9.5-g, UJ9.7-h, the capture PRD T7, the Data Foundation PRD DJ3;
  - F165 — the Data Foundation PRD R3.4, R6.2a, F54; F166 — the Data Foundation PRD F56; F167 — E6, E10, UJ1.3-g,
    UJ1.3-h; F168 — R4.1, UJ3.4-l; F169 — the import PRD E40, the import PRD F67;
  - F170 — E6, E8, the Data Foundation PRD E15, E34, F56; F171 — the Data Foundation PRD F53, F56; F172 — UJ9.4-d.
      F53's Carried by and map line add UJ7.1-r (PRIV4-5). No placeholder is left.
- [ ] **Sibling status lines, each in its sibling's style, peer review pending.**
  - DF (merged into its Collection Mode clause): "…Collection Mode amendments 2026-09-24 under F50–F51 and
    2026-09-25 under F52–F54 and F56, peer review pending: R1.2, R1.3, R1.5, R1.9, R1.10, R2.3, R2.3f, R2.9, R3.4,
    R6.2, R6.2a, R6.2d, R7.2, R7.3j and R7.6k amended with alignment kept; R1.11, R7.6o, R7.6p, R7.6q, E33, E34 and
    E35 added, ready for alignment; E8, E11, E15 and E26 reworded; OQ 17's and OQ 20's questions widened." (about +6).
  - Capture: "; Collection Mode round-4 amendment 2026-09-25 under F74 (R1.1, R1.3, R1.5, R1.10, R3.5, R7.13, R8.5
    and R9.3 amended with alignment kept; peer review pending)".
  - Import: "; Collection Mode round-4 mirror under F67 dated 2026-09-25 (R3.2 and R3.8i amended with alignment kept;
    E40 reworded, ready for alignment; peer review pending)".
  - Export: "; Collection Mode round-4 mirror 2026-09-25 under F32 (R1.1 amended with alignment kept; peer review
    pending)".
      This PRD's own status line stays "Status: draft".
- [ ] **Label check** (check 13, and the capture PRD's standing label check for its amendment): every action a row or
      case names resolves character for character to a copy label — this PRD's in its copy file ("Set a field",
      "Delete collection", "Undo", "Undo change", "Find similar", "Export collection", "New collection", "Show
      history", "Use this reading", "Apply to ⟨n⟩ swatches"), a sibling's named by its document and state, unquoted
      (the capture PRD's start, Add a swatch, E25 end action and settings; the Data Foundation PRD's E8, E9, E14 and E34
      actions). Re-run after the copy changes: E6's condition (F170-3), E8 (F170-1), the sibling-owned list (F157-2),
      IF N5's quote marks, and the sibling copy — DF E15, E34 and E35 (F170-2, F157-2), Import E40 (F169-1). Direction 2
      prints nothing.
- [ ] **Traceability** "F1–F172" in this PRD (IF N4, 4N4, R4-n3), word-neutral; the capture PRD's "F1–F74"; the import
      PRD's Traceability paragraph adds F67. New case IDs take each journey's next free letter; nothing is renumbered.
- [ ] **Seam check (the Inherited-obligations standing check).** After F155-10, F158-2 and F166, each seam's two
      summaries state the same set: this PRD's Outbound Data Foundation row against DF's inbound Collection Mode line;
      this PRD's Inbound R1.11 line against DF's outbound Collection Mode line; the Outbound Data Export row against
      Export's inbound Collection Mode line; the Outbound Capture Mode and Inventory Import rows against the rows they
      name.
- [ ] **ADR queue and post-lock.** `docs/decisions/README.md`'s 0003 row is edited as F157-6 and F171-1 state, on the
      orchestrator's say-so; the 0007 row is unchanged. `docs/product/post-lock.md`'s item "**DF** — whether a Collection
      Mode rename moves an imported column's stored name" (line 98) is still NOT ticked: it is ticked when the PR exists,
      at the bookkeeping close.
- [ ] **Constants and OQs.** No constant, candidate or interim changes. OQ 1's Closer gains 4N3's clause; OQ 2, 3, 4, 6
      and 12's Decision cells read "None." (candidate 2); the Interim-stated list is unchanged and the no-interim list
      stays None, derived from the Open questions table.

## Out of scope for this pass

- `docs/product/README.md` (IF-16's index status) — at the bookkeeping close.
- Deleting guidance comments — at lock.
- The post-lock tick for this PRD's item — when the PR exists.
- A further Data Foundation budget, or a split, if its body would pass 8,300 — the owner's, reported under the stop rule.
- Any change a fence above does not authorize. Reported, not made: a Device mirror for saved-device writes (IF4-1);
  pinning the D50 white (R4-m2); ADR-0006's check of Saved Application State's encryption (PRIV4-2); a build rule on
  crash-message text (PRIV4-4); copy explaining a held action (PMM's unrated pointer); Import E40's cell layout (SSE's
  deferred item); E6 "elsewhere"'s "many swatches" wording for a one-item undo (F161-1); E8's and DF E8/E14/E33's
  finality wording under F157 (F157-9); R8.10a's widening to import commits and moves, made as testability (F164-1).
