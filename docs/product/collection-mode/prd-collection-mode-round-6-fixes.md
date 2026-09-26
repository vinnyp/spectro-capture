# Collection Mode PRD — round 6 fixes (review round 6, the pre-lock round, 2026-09-25)

The resume point for the fix pass over the findings of review round 6 — the pre-lock round, with the plan lens and
the Phase 5 priority pass — whose subject commit is `8d30627`; this list drafted at `71618a0`. The fence file
`prd-collection-mode-fences.md` (F1–F197) is the written authorization for every change below: a **fix** names the
fence that authorizes it, or reads "editorial/testability — no new WHAT" where it only adds a case, a seam grant, a
fixture, a cite, an index entry, a phase mark or a wording fix that makes copy, a case or a hand-off match an existing
row. A change no fence names is not made: it is reported to the orchestrator as an unratified WHAT. The owner has
decided this round's forks as F189–F192 and approved round-6 recommendations 1–5 as F193–F197; the boxes marked
**needs owner** below are the only ones that wait on the owner, and the pass makes nothing for them. Each box is ticked,
with a Result note, as its fix lands.

**Word budgets.** Counted by rule 14's method, the body only, companions excluded:

~~~
perl -0pe 's/<!--.*?-->//gs; s/```.*?```//gs; s/\]\([^)]*\)/]/g; s/^\|[-: |]+\|\s*$//mg; s/\|/ /g' FILE | wc -w
~~~

Re-run at `71618a0`: **Collection Mode 11,951 of 12,000; Data Foundation 8,381 of 8,400 (F177); Export 3,783 of
4,000.** Every count below was taken by the same method on a scratch copy with the drafted text applied; the pass
re-counts.

- **Collection Mode: 49 words of headroom, and the pass must leave at least 10** (the lock status line needs about 2,
  and the lock pass needs slack). So the body must end at or under **11,990**. Every body addition names, in its
  Result note, the headroom or the trim that pays for it. Ledger on the drafted text:
  - F189: R4.9's Pri cell 0; Build dependencies rows 2 and 7 −4 (row 2 +5 for R4.9, E18 and the capture PRD's R5.6
    and R9.9; row 7 −9); the Legend's P0 meaning +4; the inbound Capture R9.9 line +2; the Outbound Capture Mode row's
    R9.9 part +5 — **+7**.
  - F192 +1 (the Build dependencies paragraph); F196 +5 (row 6, tight form) — **+6**.
  - Findings: IF6-N3 +4 (the Outbound Capture Mode row's R8.14 part); IF6-N1 +2; PRIV6-2 +2; PLAN-4 +1; PLAN-6 +11
    (the paragraph +11; DF E14 moved from row 2 to row 1, net 0); PLAN-12 +6 (row 4 +2; R5.8 moved from row 9 to row 10
    +4, on the orchestrator's ruling); Phase 5: P5-m3 +7, P5-m6 +4, P5-n1 +3 — **+40**.
  - Additions **+53** (12,004 before any trim). Word-neutral: Traceability "F1–F197", the Outbound Data Foundation
    row's "F52–F58", every status cell.
  - **Trims needed: at least −14.** Candidates T-a and T-b below give −16 → **11,988 (12 left)**; adding T-c gives
    11,979 (21 left). If the orchestrator declines PLAN-12's R5.8 move (−4), PRIV6-2 (−2) or the Legend's P0 edit
    (−4), the count falls by as much. Alternative forms: F196's clearer row-6 wording +8 (not +5).
- **Data Foundation: 19 words of headroom, and F177 says no trimming.** Every DF addition is drafted at its tightest;
  nothing in that body is trimmed. Ledger on the drafted text: R1.11 +2 (F191-1); R7.6b +3 (F191-2); R6.2a +8
  (F190-1; the tighter form "…or open" is +6); the inbound Collection Mode line's Rows +1 (IF6-1); OQ 20's closer +1
  (F196-2); the status line and the inbound line's fence range 0 — **+15, 8,396 of 8,400** (8,394 with the tight
  R6.2a). **Stop rule:** land each at its drafted form; if the body would pass 8,400, land everything else, stop and
  report the figure. Never cut a Data Foundation rule. R7.3c needs no text: R7.6b's listing makes the re-read's
  progress a rule, the Surfaces table being the rule (DF R7.6).
- **Export: 217 words of headroom.** R4.3's no-change clause +4 and its Commit PR cell +1 (the performance review's
  unrated item); the status clause +15 — **+20, 3,803**.
- Capture and Import carry no word budget.

**Candidate trims of rule-free prose in the Collection Mode body** (counts on the drafted text; each is meaning-checked
against the row or fence named before it goes; none touches a requirement row, so no row's status moves):

- **T-a — OQ 10's Decision cell, last sentence** — " Still open at v1 release, R1.7 is deferred (F57)." (−9; not
  template text). Must still state it: R1.7's last sentence ("if it is still open at v1 release this row is deferred
  and v1 ships final deletes …"), F57, and the Legend's priority bullet as P5-m6 leaves it.
- **T-b — Build dependencies, last row** — " — navigation and placement are never guessed" (−7; rationale, not template
  text). Must still state it: the Build contract's paragraph 2 ("neither reading is selected here …") and the row's
  own "Stop: the capture PRD's OQ 8 and ADR-0004".
- **T-c — Surfaces, swatch grid Shows cell** — "The collection surface's items as swatches at a chosen size, with their
  marks" → "R7.1's and R7.2's swatches" (−9; round-5 candidate 1's third part). Must still state it: R7.1, R7.2.
- **T-d — Surfaces, All items view Shows cell** — "Every item of every collection, one row each naming its collection,
  with search, filters and sorts" → "R1.9's rows, with R1.10's search, filters and sorts" (−8). Must still state it:
  R1.9, R1.10.
- **T-e — Surfaces, collection list Shows cell** — "Every collection in the file with its item count, and the entries…"
  → "R1.1's list, and the entries…" (−7). Must still state it: R1.1.
- **T-f — Surfaces, collection surface Shows cell** — ", beside every entry point and state the capture PRD places
  there" (−11). Must still state it: R2.2 and the Vocabulary's "collection surface … carrying the capture PRD's entry
  points and states".

Recommended order: T-a, T-b, then T-c, T-d, T-e, T-f as more headroom is wanted. **Not candidates:** round-5 candidates
2 and 3 (still a build-contract instruction and a table convention); the M4 check sentence (F82 puts it "beside M4");
the Interim stated list's fence cites (the template's "row — question — fence" form); template body text — the
Inherited-obligations preamble, the User journeys paragraph, the §8 preamble, the Open-questions trailer, the Status
vocabulary, the Traceability bullets, the Release paragraph; and R1.7, which is never trimmed.

The findings are the ten round-6 reviews in `docs/agent-reviews/2026-09-24-prd-collection-mode-peer-reviews.md` (Round
6 › Per-lens reviews (verbatim)), in log order: the eight Claude-route delta reviews, then `peer-plan-reviewer` (the
pre-lock lens), then the Phase 5 priority pass. That log's Round 6 verify-the-reviewer table ("log row n") records the
six accepted Blockers and Majors: rows 1, 2, 4 and 5 are testability fixes, rows 3 and 6 the owner's F192 and F189.
Each box carries the reviewer's own ID and severity. A round-5 finding that a review's delta table marks PARTIAL or
UNRESOLVED gets its own box, labelled "carried: ⟨ID⟩". One box carries each fix, the first in log order, and every other
box raising the same defect says **covered by** it; where a later box adds a part the first lacks, that part is its
own. Where this list drafts one combined text for a companion cell several findings edit (UJ9.5-d, UJ9.7-i, UJ9.8-c,
the Harness's Byte checks, DJ3's and DJ4's lines), the text sits in one box — the first in log order, or the one the
orchestrator's brief named for drafting (PERF6-2 for UJ9.5-d) — and each other box says which part is its own. An item a review raised only in its Missing, Deferred or pointer list gets a box
marked *unrated* where it asks for a change, and a line in the review's closing paragraph where it asks for none. The
fence edits are boxed once, in "Owner decisions F189–F197 — the edits each requires" below (F189-1 and so on); the
sibling fences, status lines, ADR queue and post-lock edits in its last subsection (SIB-1 and so on). Sibling IDs name
their document ("DF", "Capture", "Import", "Export"); a bare ID is this PRD's own. Line numbers are at `71618a0`, which
matches `8d30627` in every file but this PRD's fence file (F189–F197 and six dated lines were added there); that file is
cited here by fence, not by line.

Dispositions: **fix**, **covered by**, **declined (existing F-rejection)** and **needs owner**. Needs owner is strict:
any new or changed product rule, copy meaning, constant or interim, or sibling rule that no fence F1–F197 states. This
list marks five: two parts of findings (PLAN-1's device-OQ-21 interim, ARCH6-3's help-docs line), two Nits (PLAN-10,
ARCH6-N2) and one unrated item (the privacy review's optional help-docs line). The pass makes nothing for any of them.

Drafted text for a sibling row is written without its link targets; each label keeps the target its cell already
uses, and a new label takes its row's section anchor as the cell's other links do.

## Orchestrator rulings on this list (recorded 2026-09-25, before the pass)

Each ruling overrides the box it names.

- [ ] **Trims: take T-a, T-b and T-c** (−25), each meaning-checked with the still-stating row named in its Result. With the rulings below, the body lands near 11,975, leaving room for the lock status line.
- [ ] **DF R6.2a (F190-1): the drafted form** ("…or the next open", +8). It is clearer, and DF stays within 8,400.
- [ ] **Build dependencies row 6 (F196-1): the tight form** ("Stop: ADR-0003 or a later undo-representation ADR", +5).
- [ ] **The Legend's P0 meaning (F189-3): not taken.** R4.9's Pri cell governs, and the Legend states no per-row priority.
- [ ] **PRIV6-2: taken.** R8.8 reads "unless a read begun before its wipe defers it" (+2), matching DF R6.2a under F184. R8.8 then reads pre-alignment; it is re-checked in round 7 either way.
- [ ] **PLAN-12: taken in full** (+4): row 4 says R6.2 lands with R4.7, and R5.8 moves to the P2 unit.
- [ ] **R8.10a "where built" (P5-n4): the companions only** (0 body words).
- [ ] **Post-lock placement: as drafted** — a new "## v1 release" section, and a "### Collection Mode" subsection under the next-pass heading. F201's two help-docs items join the existing help-docs list.
- [ ] **This PRD's own post-lock follow-ons (PLAN unrated): appended in this pass,** as drafted, so the round-7 lenses see them. The column-rename item stays unticked.
- [ ] **Import's editorial label fixes (IF6-N4): a dated line under Import F68.**
- [ ] **Capture's obligation line wording: "the surface's entry points",** as Capture F43 states it.
- [ ] **The five needs-owner items: all answered by the owner, and each is now a fix.**
  - PLAN-1's macOS CI lane → F198. The Harness, or Build dependencies row 1, names the interim: M2 and M3 run per PR on a Mac, recorded in the PR, until the device PRD's OQ 21 closes. Metric cells change only if needed, with 0–4 body words.
  - PLAN-10 → F199. R7.2 says the default and the range are the build's choice, at or above MIN_SWATCH_SIZE (+about 9 words, within headroom).
  - ARCH6-N2 → F200. DF R1.11's deferral of app-made writes excludes the wipe. Say so in DF F58, or in R1.11 if it costs 3 words or fewer and DF stays within 8,400; otherwise put it in the ADR-0003 input.
  - ARCH6-3's help-docs line and the privacy help-docs line → F201. They are two items in post-lock.md's help-docs list.

## Status bookkeeping

- [ ] **Flip to aligned** (the log's round-6 flip record, less the holds and rewrites below) — in the PRD's tables and
      the copy file's Status lines. The record lists 112 IDs; 12 are lettered sub-rows that hold with an objected lead
      and 2 are rewritten by this pass, so **98 read aligned**. **5 newly:** E8, E10, M1, R1.3, R4.6. **93 stay
      aligned:** E1–E5, E7, E9, E11–E19, M2–M4, R1.1, R1.2, R1.4–R1.10, R2.1–R2.11 with R2.4a–i, R3.1–R3.5, R3.7–R3.9,
      R4.1–R4.5 with R4.2a–h, R4.8, R5.1–R5.8 with R5.2a–e, R6.1–R6.4, R8.2–R8.5, R8.7, R8.9. Editorial edits keep
      alignment: E4's variant mark (P5-m5), the index and Legend edits that change no row (process rule 6). Copy-file
      Status lines: E8 and E10 flip to aligned; E1–E5, E7, E9 and E11–E19 stay aligned. If a box rewrites one of the 98
      after all, it lands at pre-alignment instead and the Result says so.
- [ ] **Sub-rows hold with their lead — flag to the orchestrator.** The record lists R8.1a–g (7) and R8.10a, R8.10b,
      R8.10c, R8.10d, R8.10f (5). R8.1 is objected (R6-m4), and R8.10 and R8.10e are objected (R6-M1, R6-m1, R6-m2,
      R6-m5, PRIV6-1), so both families read needs-discussion. R2.4, R4.2 and R5.2 flip whole.
- [ ] **Aligned rows now rewritten — flag to the orchestrator.** R4.9 moves from aligned to **pre-alignment**: its Pri
      changes P1 → P0 under F189 (F189-1), a priority change being a rewrite.
- [ ] **Rows a round-6 fix rewrites → pre-alignment:** R4.9 (F189-1), E6 (F189-4, its base body names flagging), R8.8
      (PRIV6-2, on the orchestrator's ruling) — **3**. Copy-file Status line: E6 stays pre-alignment.
- [ ] **Rows objected and left unchanged → needs-discussion** (the Legend: an open objection), each named with the
      companion-only fix that answers it, so a round-7 delta can flip it once its lens re-checks:
  - R3.6 — R6-M1 (the storage read), R6-m3 (UJ3.3-f's filter, UJ7.1-t).
  - R4.7 — R6-M1 (the storage read behind UJ6.4-j/k).
  - R7.1, R7.2 — R6-M1, R6-m2 (UJ10.1-d's relaunch run and the baseline).
  - R8.1 and R8.1a–g — R6-m4 (UJ9.5-g's fourth sub-run read).
  - R8.6 — R6-M1, PRIV6-1 (the storage read, UJ9.8-c's controls).
  - R8.10 and R8.10a–f — R6-M1, R6-m1, R6-m5 (R8.10); R6-M1, R6-m1, R6-m2, PRIV6-1 (R8.10e).
  - R8.11 — PERF6-2 (UJ9.5-d's outside-read hold).
  - R8.8, if PRIV6-2 is declined — R6-m5 (UJ9.7-h's 5 s), R6-m6 (UJ9.7-i (d)).
- [ ] **After the pass:** report the final lists; the record's 112 flip-eligible and 10 objected reconcile to the
      PRD's 122. Expected: **98 aligned** (93 staying, 5 new); **3 pre-alignment** (R4.9, E6, R8.8); **21
      needs-discussion** (R3.6, R4.7, R7.1, R7.2, R8.6, R8.11, the R8.1 family of 8, the R8.10 family of 7). If PRIV6-2
      is declined: 98, 2, 22. The Open-questions table's statuses do not change (OQ 8 and 9 aligned, the rest
      pre-alignment). No new row, state, variant or metric enters this PRD; one new case (UJ7.1-t). No new sibling row
      or state: DF R1.11, R7.6q, E34 and E35 stay ⌛️ Ready for Alignment; DF R6.2a, R7.6b and E15 keep their alignment;
      DF OQ 20 stays open; Capture R9.9 and Export R4.3 keep their alignment.
- [ ] **Rows the round-7 delta must re-disposition.** Every row not aligned after this pass — R4.9, E6, R8.8, R3.6,
      R4.7, R7.1, R7.2, R8.6, R8.11 and the R8.1 and R8.10 families (24) — and, keeping alignment unless a lens objects,
      every aligned row whose case, seam or index text this pass edits: R1.1 and R3.2 (the Harness's comparator
      sentence); R1.4 (UJ1.3-c, -e, -f); R1.7 and E10 (UJ1.3-h, UJ9.7-i); R1.9 (UJ7.1-t); R2.9 and R4.6 (Surfaces marks;
      UJ4.5-b's phase); R2.11 (ZX-001's rounding); R3.5 and E4 (E4's variant mark); R4.3 (UJ9.7-i (d)'s edit); R5.1 (UJ9.5-c); R6.3
      (UJ6.3-b; UJ 6's phase line); R8.3 and E18 (UJ 4 scenario 7 in the first phase; E6's base body); R8.4 (UJ9.2-a);
      M3 (UJ9.8-b). The sibling rows too: DF R1.11, R6.2a, R7.6b, E15, E34, E35 and OQ 20; Capture R9.9; Export R4.3.

## peer-product-manager-reviewer (Claude route)

- [ ] **PM6-1 (Minor) — UJ1.3-h reads E10's zero-count token one way and UJ7.1-h reads the same situation the other
      way** (UJ1.3-h; UJ7.1-h; R8.10b; the Placeholders rule; copy E10). **fix (F181 — testability):** UJ1.3-h's Assert
      "…and in the second run naming Inks with no ⟨n⟩, the zero form F181 gives," → "…and in the second run naming Inks
      with ⟨n⟩ 0, its first sentence the zero form F181 gives,". Rows unchanged (R1.7, R8.3); R1.7 and E10 unchanged.
- [ ] **PM6-2 (Minor) — whether E35's OK still holds after a quit and reopen, with the outside read still running, is
      unstated** (DF R6.2a; DJ4:67; DF F57 (1); F173). **fix (F190):** F190-1 (DF R6.2a: OK hides it "until another
      such write or the next open"), F190-2 (DJ4:67's second run: OK fired before the quit, E35 up at the reopen), SIB-1
      (DF F58 (1)). Its reading A is what the owner chose. 0 Collection Mode body words.

The review's first risk, budget headroom, is this list's ledger; its second, E34's silent refusal, is owner-settled
(F159, F174) and left to dogfood; its third, E35's edges, is PM6-2. Its false positives are recorded, not acted on: R8.8's
"an earlier read" (PRIV6-2 rewords it on the orchestrator's ruling), F179's "a whole collection" (N6-n3), the Surfaces
marks (acted on anyway by IF6-N1 and P5-m3, which other lenses rate), and DJ4's re-read run (superseded by F191).

## peer-staff-software-engineer-reviewer (Claude route)

- [ ] **6MJ1 (Major) — DJ4's second-read line asserts that removed text persists; no row requires it, and a correct
      build can fail it** (DJ4:69; DF R6.2a; the ADR-0003 input; DF F57 (1)). Also answers R6-M2, ARCH6-1 and PERF6-1;
      log row 1. **fix (F184, read as the deadline R6.2a states — editorial/testability, no new WHAT):**
  - **DJ4:69** (DF journeys), the whole line becomes:

    > | A read transaction held open from outside the app through a read-only connection at SQLITE_READER_FLOOR, having
    > read a page ([Collection Mode R8.10a]), still running; a delete then landing; a second such read, having read a
    > page, begun after it landed | End the first read; then end the second | The delete shows done and nothing waits
    > on either read; while the second runs, E35 is up whenever the removed text is still in the file's bytes; within
    > 5 s of the second read's end, a functional timeout, the text is in the file's bytes nowhere and E35 is not up; a
    > run in which the text is in the file's bytes nowhere while the second runs reports the second read's deferral not
    > exercised, never passed (R6.2a, F57, F58) | E35 |

    The "not exercised" clause and the declared second read are R6-M2's parts; "the delete shows done and nothing
    waits" is 6MJ1's. No presence assertion survives: a correct build may wipe once the first read ends (F184 is a
    deadline). ARCH6-1's alternative — a removed text written while the first read runs, so the second read pins its
    log frame — is not taken: the end-state assertion already fails a build that never wipes.
  - **The ADR-0003 row** (`docs/decisions/README.md`) — in F195-1's text: "only its wipe following the end of every read
    begun before that wipe" → "its wipe waiting at most until every read begun before that wipe has ended" ("that wipe",
    not "it", so the read cannot bind to the write, as round 5 settled).
  - **DF F57 (1)'s bookkeeping** — SIB-2, a Corrected line under DF F57: "a read begun after the delete deferring it
    too" → "…permitted to defer it".
  - PERF6-1's caution stands: the fix does not have the app hold off checkpoints while any reader exists. 0 Collection
    Mode body words, 0 DF body words.
- [ ] **6MN1 (Minor) — the byte-check moments still let a correct build fail on timing** (the Harness's Byte checks;
      UJ1.3-f; UJ6.4-k; UJ9.7-h run 2; Export EJ1). Also answers carried 5MN2 and PERF6-5; with IF6-N2, F191 and F195,
      which edit the same sentence. **fix (F175 — testability; F191 and F195 for the list):**
  - **The Harness's Byte checks** — replace the sentence from "At the open-app moment, and before the after-a-crash
    run's crash, the check waits BROWSE_RESPONSE_BUDGET after the write lands —" through "…reads at the moments its When
    names." with exactly:

    > At the open-app moment, and before any crash a case makes after a removing write — the after-a-crash run's,
    > UJ1.3-f's and UJ6.4-k's — the check waits BROWSE_RESPONSE_BUDGET after the write lands: the bound the ADR-0003
    > input puts on every read this app makes but an export and the Data Foundation PRD's Save a copy (its R7.3h), a
    > read begun before a wipe deferring it (its R6.2a), and that PRD's re-read checks (its R7.3c) and open-file check
    > (its R5.4) ending before the file takes a write (its R1.11); a case running an export or Save a copy reads within 5 s
    > of its end, a functional timeout, and a case declaring a read held open from outside the app (R8.10a) reads at the
    > moments its When names.

    The next sentence ("For the app's own storage, the after-a-crash run's crash, and the crash UJ6.4-k and UJ7.1-r each
    make, …") is unchanged. 6MN1's parts: (a) the crash list, (b) "within 5 s of its end, a functional timeout".
    IF6-N2's: the Data Foundation PRD's labels. F191's: a re-read's checks leave the "reads once it ends" clause, no
    edit overlapping them. F195's: the open-file check.
  - **UJ9.7-h, run 2** — "…and after reopening its bytes hold the text Sky Blue nowhere" → "…and within 5 s of reopening,
    a functional timeout, its bytes hold the text Sky Blue nowhere" (6MN1 (c)).
  - **Export EJ1's mid-export line** — "once the export ends, the text the edit removed is in the file's bytes nowhere"
    → "within 5 s of the export's end, a functional timeout, the text the edit removed is in the file's bytes nowhere";
    recorded in SIB-5. 0 body words.
- [ ] **6MN2 (Minor) — E35's end, "until the wipe lands", has no branch for closing the file or switching** (DF R6.2a;
      DF E35; DJ4:66–68). **fix (F190):** F190-1 (R6.2a "until the wipe lands or the file closes"), F190-2 (DJ4's new
      close-and-switch line). Its third point — another app's read ended while this app's export still holds the wipe
      — is settled by F173 ("One E35 while any erase is waiting … It goes away on its own once the erase happens"): E35
      stays until the wipe lands; no copy change is made (reported with the review's Deferred copy question below).
- [ ] **6MN3 (Minor) — renaming the file while it is open: the case checks too little, and ADR-0003 has no input for it**
      (UJ9.7-i (d); DJ3:39; DF R7.3j; the ADR-0003 row). Also carries the text of R6-m6, N6-m2, PLAN-9 and 6N2, which edit
      the same case, and answers ARCH6-3's ADR half. **fix (F174 — testability; F197 for the post-lock item):**
  - **UJ9.7-i** — the whole row becomes (Given, When, Assert, Rows):

    > | UJ9.7-i | The seeded file; R4.7, R6.1 and R6.2 built; Studio Markers searched zx-0; for run (b), a copy of the
    > seeded file holding a third collection, Spare, kept in another folder; for run (d), another folder | Delete ZX-010
    > through "Delete swatch" and the Data Foundation PRD's E8 delete action; clear Family on ZX-001 and ZX-002 through
    > "Set a field"; in run (d), then set ZX-002's Swatch Alternate Name, empty in the seeded file, to Leaf and press
    > Return, a write that removes no text; declare the app's permission to the file revoked (R8.10a); set ZX-003's
    > Swatch Name to Magenta and press Return; fire the Data Foundation PRD's E34 choose-the-file action and, in
    > separate runs: (a) pick the same file, declare the permission restored, then fire that PRD's E34 try-again action;
    > then fire "Undo change" twice; (b) pick the copy; (c) as (a) up to its try-again, with a bulk session on Gouache
    > Set brought in flight after the clear; (d) before the pick, rename the file and move it to that folder outside the
    > app, then pick it there under its new name, go on as (a) up to its try-again, then quit; in (a) and (b), read the
    > app's own storage while its E34 is up and again after quitting. The delete comes before the clear because a
    > delete is a write the user commits, which ends R4.7's history | (a) the Data Foundation PRD's E34 stays up until
    > the try-again, the file then holds Magenta for ZX-003, "Undo change" — and E10's "Undo" where R1.7 is built — is
    > still offered and the search field holds zx-0, and after the first "Undo change" ZX-003 reads Neon Magenta and
    > after the second ZX-001 holds Family Blue and ZX-002 Green; (b) its E34 stays up, the collection list names
    > Gouache Set and Studio Markers and no Spare, each file's bytes read before the pick and after it are identical,
    > and "Undo change" — and E10's "Undo" where R1.7 is built — is still offered; (c) as (a) up to the try-again, and
    > the Data Foundation PRD's E22 does not render; (d) as (a) up to the try-again, its E34 naming the file at its new
    > location until then, and, read with the app closed after the quit, the file under its new name holds Magenta for
    > ZX-003, Leaf for ZX-002, no ZX-010 and an empty Family for ZX-001 and ZX-002, and its bytes hold the text Warm
    > Grey 1 nowhere; in (a) and (b), neither read of the app's own storage holds the text Magenta (the Data Foundation
    > PRD's R1.1) | R8.8, R4.7 |

    6MN3's part: (d)'s read of the renamed file — no ZX-010, the empty Family, no Warm Grey 1. R6-m6's: a write that
    removes no text, committed after the last wipe, carried through the move (Leaf). N6-m2's: E34 naming the new
    location. PLAN-9's: the Given drops R1.7, E10's "Undo" asserted "where R1.7 is built", Rows drop R1.7. 6N2's: the
    Data Foundation PRD's R1.1 cited in the Assert rather than the Rows, which hold this PRD's IDs only (the interface
    review's Rows check).
  - **DJ3:39** (DF journeys), the whole line becomes:

    > | File open, a metadata change that removes no text committed to it, then the app's permission to it lost; the
    > file then renamed and moved to another folder outside the app (R7.2) | Change an item's metadata; choose Choose the
    > file again and pick the file under its new name and folder; then choose Try again; then quit | The file-selection
    > path restores access to that file; E34 stays up, naming the file at its new location, and no write is retried
    > until Try again, which lands the change whole in it; read with the app closed, the file under its new name holds
    > both changes (R7.3j, R1.10, F54, F58) | E34 |

  - **The ADR half** — F197-2's post-lock item under § ADR-0003. The ADR-0003 row itself gains no input (F197 puts it in
    post-lock). 0 body words.
- [ ] **6MN4 (Minor) — a re-read's checks now run beside user writes, and nothing orders the two** (DJ4:70; DF R7.3c;
      R8.5; R4.7). Also answers ARCH6-2. **fix (F191):** F191-1 to F191-4. With edits held while a re-read runs, the
      refresh cannot miss a write made during the checks, and R4.7's "a re-read ends that history" cuts the same at the
      re-read's start or end; R8.5 and R4.7 are unchanged. 0 Collection Mode body words.
- [ ] **6N1 (Nit) — DJ3's deferred-mark line leaves open which collection the item is in** (DJ3:45). Also carries the
      parts of R6-n4, IF6-N4 and ARCH6-N3 that edit this line. **fix (F182 — testability):** DJ3:45 becomes:

    > | No capture in flight; a [Collection Mode R8.1f/g write] held running until released ([Collection Mode R8.10a]);
    > an item outside the held write whose sample archive is unreadable and not yet marked (R5.5a, R7.2) | Open that
    > item's detail while the write runs; then release the write | The detail opens without waiting on the write
    > ([Collection Mode R8.1b]); a copy of the file read by SQL while the write runs holds no archive-unavailable mark
    > for the item, and within 5 s of the write landing, a functional timeout, the file holds it (R1.11, R5.5a, F57) |
    > E31 |

    6N1's part: "an item outside the held write". R6-n4's: the named copy read and the functional timeout. IF6-N4's:
    "a Collection Mode R8.1f/g write" for "a Collection Mode bulk write or delete", the label keeping its §8 target.
- [ ] **6N2 (Nit) — UJ9.7-i's new app-storage read cites the wrong owning row.** **fix (F53 — testability):** in
      6MN3's UJ9.7-i text — the Assert's closing "(the Data Foundation PRD's R1.1)"; the Rows stay this PRD's IDs.
- [ ] **6N3 (Nit) — UJ6.4-j and UJ6.4-k never apply the clear** (UJ6.4-j, UJ6.4-k; R6.2). **fix (testability — no
      new WHAT):** in both Whens, "…fire "Set a field", choose Family and leave the value empty;" → "…fire "Set a field",
      choose Family, leave the value empty and press Return;". Rows unchanged.
- [ ] **6N4 (Nit) — F102's F157 line still reads "wiped when that read ends, or when the file next opens"** (fences,
      the F157 line under F102). **fix (F173 — fence bookkeeping):** a dated line is immutable history, so a new line
      goes after F102's F175 line, rather than an edit to the F157 line: "**Clarified 2026-09-25 (owner decision D42,
      F173):** the F157 line above reads in the order F157's F173 line states — the wipe follows the read's end, on its
      own or at the first open after it — as the Data Foundation PRD's F56 Corrected line records." F102's Carried-by is
      unchanged.
- [ ] **carried: 5MN2 (PARTIAL) — the cases that crash straight after a removing write get no wait, and "reads once
      it ends" gives no grace** (the Harness; UJ1.3-f; UJ6.4-k). **covered by 6MN1.**

The review's twelve clarifying questions are answered: 1 by 6MJ1 (a deadline: the wipe may land while a later read
runs); 2 and 3 by F190; 4 by F173 (E35 stays until the erase happens); 5 by 6MN3's case and F197-2's ADR-0003 item; 6, 7
and 8 by F191 (no; nothing to reconcile; the cut is the same either way); 9 by 6MN1 (a); 10 by 6MN1 (b). Question 11 (how
the app knows at an open that a wipe is pending, and whether E35 may show when no text was removed) is ADR-0003's
mechanism — the architecture review calls the stored wipe-pending marker "an ADR-0003 detail" — and R6.2a shows E35
only for a removing write's wipe: reported, not changed. Question 12 (a removing edit on a network volume) is F183's:
the non-waiting save is unpromised there, and the help-docs item F183-2 made says so: reported. Its Deferred items are
carried by boxes above or below: architecture — 6MN3's mechanism (F197-2), the wipe as checkpoint or reset (6MJ1:
either meets the deadline), question 11 (reported); performance — non-blocking checkpoint retries (PERF6-1's caution);
test — 6MN1 and 6MJ1; product and marketing — E35's copy when only this app's read holds the wipe (F173's lifetime;
no copy change, reported) and E35 at a reopen saying "Your change is saved" about an earlier run (F194's "Your changes
are saved." and F190); privacy — removed text in a WAL left under the old name (F197-2).

## peer-test-reviewer (Claude route)

- [ ] **R6-M1 (Major) — the decoded storage read skips data values and dictionary keys, and replaces the raw read
      instead of running beside it** (the Harness's Byte checks; the map's app-storage line; UJ9.8-c; UJ3.3-f, UJ9.4-b,
      UJ6.4-k; R3.6, R4.7, R7.1, R7.2, R8.6, R8.10e). Also answers PRIV6-1 and carried R5-B1, and carries the text of
      R6-m1 and R6-m2's baseline sentence; log row 2. **fix (F35, F53, F102, F130 — testability; companion only, 0
      body words):**
  - **The Harness's Byte checks** — replace the sentences from "A read of the app's own storage for a text exempts
    nothing, and it decodes before it matches:" through "…beside the decoded read, never in place of it." with
    exactly:

    > A read of the app's own storage for a text exempts nothing, and it decodes before it matches: each property list
    > or keyed archive — the app's Preferences, Saved Application State's data.data decrypted with the key in
    > windows.plist beside it, and any other such file — is read as each string it holds, dictionary keys included, and
    > each number as its decimal text; each SQLite database among those paths is read as the SQL read reads the file,
    > every table, its internal tables and any full-text index's vocabulary included; each data value such a list or
    > archive holds, and each BLOB such a database holds, is read by these same rules as a file of its own; unified-log
    > entries are read decoded; and any other file is read as its raw contents. For a text that is not a number, every
    > file is also read as its raw contents, a match in either read counting. The read finds no match for the text, or
    > for any word of it of four or more letters, as a whole token — bounded on each side by the start or end of a
    > decoded value or of the raw contents, or by a character that is not a letter or digit — in any letter case or
    > normalised form, in either encoding. Where the text is a number, the read is compared with a baseline read of the
    > same storage taken before the case's When, and a match does not count only where the baseline holds the same
    > text in the same value, at the same key path, in the same file. A case may also relaunch with window restoration
    > on and read R8.10b, beside the decoded read, never in place of it.

    "UJ9.8-c is the control this check must catch, its SQL-read run and its storage runs included." follows unchanged.
    R6-M1's parts: dictionary keys, data values and BLOBs read recursively as files of their own, and the raw read for a
    text that is not a number. PRIV6-1's: every property list, keyed archive and SQLite database read raw as well, "a
    match in either read counting", which reaches a deleted cache row's free page. R6-m2's: the baseline match defined
    by value, key path and file. Whole tokens stay the rule, so "blue" in "CoreBluetooth" stays no match.
  - **UJ9.8-c** — the Given's storage part, after "…prefix-compressed so the copy's raw bytes hold warm nowhere; and, in
    the app's own storage," becomes: "(i) the word warm alone as a string value in a binary property list among its
    Preferences; (ii) the word warm as a dictionary key in that property list; (iii) the word warm alone as a string in
    a keyed archive held as a data value in that property list; (iv) the text warm grey 1 inside a JSON object held as
    a data value in that property list; (v) a row of three adjacent text columns holding ZX-010, warm and Grey in an
    SQLite database under its Caches; (vi) the word warm as a prefix-compressed full-text vocabulary term after war in
    an SQLite database of its own there, with no content table; and (vii) the word warm alone as the one column of a
    row in an SQLite database under its Caches, the row deleted before the read". The When is unchanged. The Assert
    becomes: "The check reports a match in every run: in the file's fourth run from the SQL read alone; in storage runs
    (i), (ii), (iii), (v) and (vi) from the decoded read alone, the raw bytes there holding no word of the text as a
    whole token; in run (iv) from both reads; and in run (vii) from the raw read alone". Rows unchanged (R8.10). (ii),
    (iii) and (iv) are R6-M1's controls; (vii) is PRIV6-1's; (v)'s "warm and Grey", which packs as `ZX-010warmGrey`, (vi)'s
    database of its own and the Assert's "no word of the text" are R6-m1's.
  - **The map's app-storage line** — "…for a text by the Harness's byte check, decoded and as whole tokens within each
    decoded value, exempting nothing, a number against a baseline read before the When, and a control seeded in it for
    that check" → "…for a text by the Harness's byte check, decoded — data values and BLOBs as files of their own — and,
    for a text that is not a number, raw as well, as whole tokens, exempting nothing, a number against a baseline at the
    same key path read before the When, and a control seeded in it for that check".
- [ ] **R6-M2 (Major) — DJ4's second-read line asserts that removed text stays, which no row states and a correct build
      fails** (DJ4:69; also DJ4:68). **covered by 6MJ1.** Its own parts: DJ4:69's "not exercised" clause and declared
      second read (in 6MJ1's text), and **DJ4:68's Initial state** — "A read made from outside the app still running" →
      "A read transaction held open from outside the app through a read-only connection at SQLITE_READER_FLOOR, having
      read a page ([Collection Mode R8.10a]), still running" (F157, F164 — testability). If the owner wants the text to
      stay, that is a new R6.2a rule; nothing here makes it.
- [ ] **R6-m1 (Minor) — UJ9.8-c's SQLite storage controls don't tell a working decoded read from a raw one.** **fix
      (F53 — testability):** in R6-M1's UJ9.8-c text — control (v) "ZX-010, warm and Grey", control (vi) in a database of
      its own with no content table, and the Assert's "no word of the text as a whole token".
- [ ] **R6-m2 (Minor) — UJ10.1-d has no usable text for the grid choice or the size, and the number baseline leaves
      match identity undefined** (UJ10.1-d; the Harness). Also answers PRIV6-4. **fix (F71 — testability):** the
      baseline sentence is in R6-M1's Harness text; **UJ10.1-d** becomes:

    > | UJ10.1-d | The seeded file; the swatch size the first "Grid" of a fresh launch shows, recorded before the Given;
    > Studio Markers shown as a grid at a swatch size above MIN_SWATCH_SIZE other than 48 pt and other than that
    > recorded size | Set the swatch size to 48 pt; close the file, read the app's own storage and the file, then reopen
    > the file and choose Studio Markers; in a second run, set the swatch size to 48 pt, or to 56 pt where the recorded
    > size is 48 pt, quit the app, relaunch it with window restoration on, open the file if the app has not, choose
    > Studio Markers and fire "Grid" | Neither read holds the swatch size's text 48, and after reopening Studio Markers
    > shows as the table; in the second run, after the relaunch Studio Markers shows as the table and, after "Grid", its
    > swatches show at the recorded size | R7.1, R7.2, R8.6 |

    "The grid choice" leaves the read, not being a text; the table after reopening and the relaunch run are its
    oracles. The relaunch run is the one the Harness permits ("beside the decoded read"). PLAN-10's unnamed default is
    recorded, not assumed.
- [ ] **R6-m3 (Minor) — R3.6's "none is kept once the file closes" has no case for filters or for the All items view.**
      **fix (F20 — testability):**
  - **UJ3.3-f** — When "In Studio Markers type zx-01 and fire the L* header; choose Gouache Set; choose Studio Markers;
    then close the file, reopen it and choose Studio Markers; …" → "In Studio Markers type zx-01 and fire the L* header;
    choose Gouache Set; choose Studio Markers; then choose the mark filter value simulated, close the file, reopen it
    and choose Studio Markers; …"; Assert "…after reopening, the search field is empty and the table lists 13 items in
    queue order; …" → "…after reopening, the search field is empty, no filter is active and the table lists 13 items in
    queue order; …".
  - **A new case after UJ7.1-s:**

    > | UJ7.1-t | The seeded file | Fire "All items", type zx-01, choose the mark filter value simulated and fire the L*
    > header; close the file, reopen it and fire "All items" | After reopening, the search field is empty, no filter is
    > active, and the table lists the 15 items in R1.9's order, Gouache Set's ZX-001 and GS-002 first | R3.6, R1.9 |

    The UJ 7 preamble needs no change (UJ7.1-t runs where R1.9 and R1.10 land). F20's Carried-by and map line add
    UJ7.1-t (SIB-7).
- [ ] **R6-m4 (Minor) — UJ9.5-g's fourth sub-run reads "the original file … with the app closed", but the app stays
      open.** **fix (F176 — testability):** "…and the original file read with the app closed holds every Scale item
      (the Data Foundation PRD's R1.11)" → "…and the original file, copied and read by the Harness's SQL read while the
      app stays open, holds every Scale item (the Data Foundation PRD's R1.11)".
- [ ] **R6-m5 (Minor) — "after reopening" has no time bound in UJ9.7-h's second run or in DJ4's first line.** **covered
      by 6MN1** for UJ9.7-h. Its own part, **DJ4:66**: "…within 5 s of the read's end, a functional timeout, or after
      reopening, the removed text…" → "…within 5 s of the read's end, a functional timeout, or within 5 s of reopening,
      the removed text…" (testability).
- [ ] **R6-m6 (Minor) — UJ9.7-i (d) checks only the write that was retried.** **fix (F174 — testability):** in 6MN3's
      UJ9.7-i text — run (d)'s Leaf edit and its read-back. The review's column hide is not used: hiding a column needs
      R2.10 (P2), and made before the delete it is copied in by the delete's wipe, so it could not meet the stranded log;
      a text-adding edit after the clear, in run (d) only, is a P0 write the later wipes do not force out, and it leaves
      (a)'s two undos as they are.
- [ ] **R6-n1 (Nit) — UJ9.5-g's new timing check can pass without checking anything.** **fix (testability):** its
      Assert "…at the first keystroke frame showing the collection delete's progress, Scale Two's "Rename collection"
      shows disabled." → "…shows disabled, a run showing no progress reporting this check not exercised, never passed."
- [ ] **R6-n2 (Nit) — UJ9.5-d's overlap guard samples at the wrong moment.** **fix (F139, F158 — testability):** lands
      in PERF6-2's UJ9.5-d text — whether an export was running is recorded when each edit lands, and the export
      assertion reads "for an item edited while it ran".
- [ ] **R6-n3 (Nit) — UJ6.4-q takes its precondition read after the export.** **fix (F163 — testability):** its When
      "Set ZX-001's Swatch Name to Harbour and press Return; fire "Export collection" on Studio Markers and complete the
      Data Export PRD's canonical export to a declared folder outside the case's folder, then list the actions offered;
      read a copy of the file by the Harness's SQL read; open ZX-017, …" → "Read a copy of the file by the Harness's SQL
      read; set ZX-001's Swatch Name to Harbour and press Return; fire "Export collection" on Studio Markers and complete
      the Data Export PRD's canonical export to a declared folder outside the case's folder, then list the actions
      offered and read a copy of the file by the Harness's SQL read again; open ZX-017, …"; its Assert "The copy read
      before ZX-017 opens holds no archive-unavailable mark for ZX-017, or the case reports not exercised, never passed;"
      → "The copy read before the export holds no archive-unavailable mark for ZX-017, or the case reports not
      exercised, never passed, and the copy read after it holds none either;". Rows unchanged (R4.7). (DF's Data
      Export line: "export never writes the source".)
- [ ] **R6-n4 (Nit) — DJ3's new mark line gives no time bound and no named read.** **covered by 6N1**, whose text
      carries both.
- [ ] **carried: R5-B1 (PARTIAL) — values nested in data values, dictionary keys, weak SQLite controls.** **covered by
      R6-M1** (with R6-m1).

The review's Over-tested note asks for nothing.

## peer-interface-reviewer (Claude route)

- [ ] **IF6-1 (Minor) — the seam half of F187 is on neither obligation line, and DF's inbound Collection Mode "Rows
      here" omits R6.3** (DF:280; PRD:625). **fix (F187 — editorial: the Rows cell names the row that carries F187's
      deferral):** DF's inbound Collection Mode line's Rows add "[R6.3](#6-deletion-and-privacy)" after "[R6.2]" (+1 DF);
      recorded in SIB-1. Its optional part — F187-3's drafted clause on both sides (+7 each) — stays not added under the
      round-5 ruling: DF's budget cannot carry +7, and the Rows cell now lets the seam check see the pair.
- [ ] **IF6-2 (Minor) — DJ4:68–69 declare "A read made from outside the app" without this round's exact input.**
      **covered by 6MJ1** (DJ4:69) and **R6-M2** (DJ4:68).
- [ ] **IF6-N1 (Nit) — the phase marks in the Surfaces table are now uneven after L2-1** (PRD:373). Also answers
      PLAN-5 and P5-m2. **fix (editorial — the index follows R2.9's P1; no new WHAT):** the collection surface's Shows
      cell "…"Delete selected" [phase: action-absent], reordering, "Use as scan order"…" → "…"Delete selected" [phase:
      action-absent], reordering [phase: action-absent], "Use as scan order"…" (+2). The copy file gains a Phase-marks
      sentence naming this unlabelled capability (P5-n6's text), so check 2 has a counterpart; if its parser pairs only
      quoted labels, the lock record disposes this mark as the reviewer's own fallback allows. Its other two
      observations — range and toggle selection dropped from the index, and the collection-list row's descriptive
      "to the All items view" — carry no fix in the review: R6.1 and E2's actions govern, and the index stays as it is.
- [ ] **IF6-N2 (Nit) — the Harness names Data Foundation actions without their document.** **fix (editorial):** in
      6MN1's Harness text — "the Data Foundation PRD's Save a copy (its R7.3h)" and "that PRD's re-read checks (its
      R7.3c)".
- [ ] **IF6-N3 (Nit) — the Outbound Capture Mode row does not list Capture F75's R8.14 move to P0** (PRD:626). **fix
      (F186 — editorial: the row lists a change made in this change):** with F189's R9.9 half (F189-12), the row's
      Obligation adds, before "; and its FIND_BUDGET closes…", "; its R8.14 and R9.9's Flag entry at P0" and its Rows add
      R4.9. IF6-N3's part is "its R8.14 at P0" (+4).
- [ ] **IF6-N4 (Nit) — sibling wording.** **fix (editorial — labels; no rule changes):**
  - **Import PRD:146** — "([Collection Mode R1.3](…), [R3.1](…), [R4.4](…), F68)" → "([Collection Mode R1.3, R3.1 and
    R4.4](…#1-collections-and-the-collection-list), F68)", one label naming its document, so R3.1 no longer reads as
    Import's own; recorded in SIB-4.
  - **Capture PRD:386** — "this document's states never hiding its entry points" → "this document's states never hiding
    the surface's entry points", as Capture F43 states it ("never hides that surface's entry points") and as this PRD's
    inbound line summarises it; the reviewer's "Collection Mode's" is narrower than F43, so it is offered on the
    orchestrator's ruling. Recorded in SIB-3.
  - **DF DJ3:42 and DJ3:45, Import UJ 2.1 lines 55 and 57** — "a [Collection Mode bulk write or delete]" → "a [Collection
    Mode R8.1f/g write]", each label keeping its §8 target, as DF R1.11 names it (DJ3:45 in 6N1's text). Recorded in
    SIB-1 and SIB-4.
- [ ] **carried: IF-16 (UNRESOLVED; deferred by design) — `docs/product/README.md` line 18 still reads "queued".**
      **fix (F56 — editorial), at the bookkeeping close**, as rounds 2–5 hold; nothing in this pass.

The review's Missing list is carried: F187's obligation text (IF6-1), IF-16 (above), and the lock-time items (the last
box of Checks). Its over-engineered note (UJ9.5-g's many runs) was settled by the round-5 ruling not to split it.

## peer-privacy-reviewer (Claude route)

- [ ] **PRIV6-1 (Medium) — the storage read no longer reads plists or SQLite caches raw, so data values and deleted
      rows' free pages are read by nothing.** **covered by R6-M1.** Its own parts are in R6-M1's text: every property
      list, keyed archive and SQLite database also read raw ("a match in either read counting"), and UJ9.8-c's control
      (vii), the deleted one-column row.
- [ ] **PRIV6-2 (Low) — R8.8's "an earlier read" is narrower than DF R6.2a's "a read begun before its wipe"** (R8.8).
      **fix (F184 — editorial), on the orchestrator's ruling:** "…unless an earlier read defers its wipe (…)" → "…unless a
      read begun before its wipe defers it (…)" (+2). R8.8 reads pre-alignment. F184's Carried-by and map line add R8.8
      (SIB-7). If declined: R8.8 reads needs-discussion, and the product review's note (R8.8 cites R6.2a, which governs)
      is the recorded reason.
- [ ] **PRIV6-3 (Low) — no delete case checks that a deleted collection's name, or a deleted item's imported value, has
      left the bytes** (UJ1.3-c, -e, -f; UJ6.3-b). **fix (F35, F102 — testability):** UJ1.3-c's Assert "…and its bytes
      hold the text Teal nowhere" → "…and its bytes hold the texts Teal, Purple and Studio Markers nowhere"; UJ1.3-e's and
      UJ1.3-f's "The bytes hold the text Teal nowhere" → "The bytes hold the texts Teal, Purple and Studio Markers
      nowhere"; UJ6.3-b's "…its bytes hold the texts Teal and Plum nowhere" → "…its bytes hold the texts Teal, Plum and
      Purple nowhere". The exemption stays sound: after either delete no remaining seeded value contains Purple, Studio
      or Markers (Gouache Set's values are Cerulean, Cadmium Red and the codes), and after UJ6.3-b none contains Plum.
- [ ] **PRIV6-4 (Info) — the baseline rule for numbers does not say what makes a match "the same".** **covered by
      R6-m2** (the baseline sentence in R6-M1's Harness text).
- [ ] *unrated* **Privacy Missing — an optional post-lock help-docs line saying that an export, Save a copy or a
      re-read's checks running at a delete keeps the removed text until it finishes** (PRIV5-5 (c)). **needs owner:** the
      review marks it "the owner's call since it is unfenced"; F175 settles that no notice is shown and states no
      help-docs line, and F197 names three post-lock items, not this one. Nothing is made.

## peer-product-marketing-manager-reviewer (Claude route)

- [ ] **N6-m1 (Minor) — after F176, DF E15 and E34 render over a cancelled quit, close or switch, and "the last thing
      you did wasn't saved" points at the quit** (DF E15, E34). **fix (F193):** F193-1.
- [ ] **N6-m2 (Minor) — after F174's pick under a new name, nothing says whether E34's ⟨path⟩ follows the file** (DF
      E34; UJ9.7-i (d); DJ3:39). **fix (F174 — testability; DF's Placeholders "⟨path⟩ a file location", the open file
      being the one "wherever it now is"):** in 6MN3's UJ9.7-i text ("its E34 naming the file at its new location until
      then") and DJ3:39's text ("E34 stays up, naming the file at its new location"). No copy text changes.
- [ ] **N6-n1 (Nit) — DF E35's "Your change is saved." is singular.** **fix (F194):** F194-1.
- [ ] **N6-n2 (Nit) — the DF copy header's P1 rule no longer covers E33's own marked sentences once E33 is marked
      ‹P1›** (DF copy header). **fix (L1b-2's phase bookkeeping — editorial, DF copy only):** "…— and a marked action or
      sentence inside an unmarked state is withheld until then —…" → "…— and a marked action or sentence is withheld
      until then —…". Recorded in SIB-1.
- [ ] **N6-n3 (Nit) — E6 "elsewhere"'s "changes to a whole collection" can be read to include renames** (copy E6;
      R8.3). **covered by F179:** the owner's approved wording, which the reviewer recommends leaving; no change, a
      dogfood observation.

The review's two pointers: E35's OK across a reopen is PM6-2 (F190); what prompts the user to quit again after a
cancelled quit asks for no change (F176 accepts the user re-initiating) and is left to dogfood.

## peer-architecture-reviewer (Claude route)

- [ ] **ARCH6-1 (Major) — DF DJ4's F184-3 line asserts a retention that no SQLite build can show.** **covered by
      6MJ1**, including its Corrected line under DF F57 (SIB-2).
- [ ] **ARCH6-2 (Minor) — what a re-read shows for a write made during its checks is undefined.** **covered by 6MN4**
      (F191).
- [ ] **ARCH6-3 (Minor) — a file renamed or moved outside the app while open strands its log.** **covered by 6MN3**
      for the case and F197-2's ADR-0003 item. Its own part — a post-lock help-docs line, "move the file from inside the
      app, or with it closed" — **needs owner:** F197 approves three post-lock items and this is not among them; nothing
      is made.
- [ ] **ARCH6-N1 (Nit) — DJ4:68 and :69 say "A read made from outside the app" without DJ4:66's definition.** **covered
      by R6-M2** (DJ4:68) and **6MJ1** (DJ4:69).
- [ ] **ARCH6-N2 (Nit) — is the deferred wipe one of R1.11's app-made writes that wait?** **needs owner:** the reviewer
      asks for a DF F57 clause settling which reading holds; no fence decides whether the wipe is a write R1.11 defers
      (F182's example is R5.5a's mark). Recommendation to carry: the second reading, the wipe running beside a held
      write, which R6.2a's deadline (no hold exception) and the ADR input's "never holding a write" already favour.
      Nothing is made.
- [ ] **ARCH6-N3 (Nit) — DJ3's fixtures are loose.** **covered by 6N1** (DJ3:45's item outside the write and its
      timeout). Its own part, **DJ3:44** — "…the volume declared full before its release |" → "…the volume declared
      full before its release, for a move the destination's |" (testability: a same-volume move is a rename that needs
      no room).
- [ ] *unrated* **Architecture Missing — R8.11 is not scoped to R1.10's local volume, as the ADR-0003 input now is.**
      **fix (F197):** F197-3.

## peer-performance-reviewer (Claude route)

- [ ] **PERF6-1 (Major) — DF DJ4's second-read line asserts a state a correct build does not keep.** **covered by
      6MJ1.** Its caution is kept: no build is asked to hold off checkpoints while any reader exists.
- [ ] **PERF6-2 (Minor) — UJ9.5-d still does not make a capture save land while a wipe is deferred, and its guard
      cannot report that** (UJ9.5-d; R8.11). Also answers carried PERF5-2, PERF4-3 and PERF3-7, and carries the text of
      R6-n2 and PLAN-7. **fix (F157, F183 — testability; its (a), no row change):**
  - **UJ9.5-d** — the whole row becomes:

    > | UJ9.5-d | Scale as UJ9.5-a declares it, searched, filtered and view-sorted; a bulk session in flight on Scale on
    > the Demo Device at real pacing (the capture PRD's R11.7); a Release build on the Mac OQ 1's interim names; for the
    > second run, a Release-configuration test build and a read transaction held open on the file from outside the app
    > through a read-only connection at SQLITE_READER_FLOOR, having read a page (R8.10a), begun before the first set and
    > released after the 20th | While capture saves 20 sets, at each set's last Demo sample (the capture PRD's R11.7)
    > type a search keystroke, fire a header and set one Scale item's Family; in a second run, in the phase that lands
    > R1.9, the same keystroke and header fire at each set's last Demo sample and, at each set's first Demo sample, fire
    > "All items" so that its first rows load while the edit lands, start an export of another of UJ9.5-b's collections
    > through "Export collection" and the Data Export PRD's E1 where none runs, then set the Family of one item of the
    > exported collection and fire the Data Foundation PRD's E35 action, recording whether an export was running when
    > each edit landed; then release the outside read | Every trigger acknowledgement lands within TRIGGER_ACK_WINDOW and
    > every row confirmation within ROW_CONFIRM_BUDGET, as the capture PRD's R11.7 reads them, in the second run while
    > the outside read is held — ROW_CONFIRM_BUDGET being the engineering plan's declared value (F120), and a run before
    > that plan declares it reporting the row-confirmation half not exercised, never passed; in the second run each
    > export's row for an item edited while it ran holds that item's Family from before the edit (the Data Export PRD's
    > R1.1), a run in which no edit landed while an export ran reporting that half not exercised, never passed, and
    > within 5 s of the read's release, a functional timeout, the file's bytes hold none of the Family values the edits
    > replaced | R8.11, R1.8, R8.8 |

    PERF6-2's parts: the outside read held across the run, so every save lands behind a deferred wipe (the case F157
    and F183 were written for); the edit moved to the first sample with E35's action after it; the bytes read after
    the release; and the old guard ("a run in which no export was running at any set's last sample …"), which could
    never fire, removed. R6-n2's: the recording at each edit's landing and "an item edited while it ran" — a guard that
    can fire. PLAN-7's: the ROW_CONFIRM_BUDGET clause. The byte read keeps the Harness's exemption for a value another
    item still holds.
  - **The Harness's Phases and builds** — "…except that a timing case reading R8.10b or R8.10c runs a
    Release-configuration test build (R8.10)." → "…except that a timing case reading R8.10b or R8.10c, or holding
    R8.10a's outside read, runs a Release-configuration test build (R8.10)." R8.10a scopes the outside read to a test
    build (UJ9.7-h's Given says so too), which the review's "no build change" did not account for.
  - **The map's sessions line** — "…at real pacing and timed to a set's last sample where a case says so" → "…at real
    pacing and timed to a set's first or last sample where a case says so".
- [ ] **PERF6-3 (Minor) — DF DJ4's Save-a-copy and re-read line may never be exercised by a correct, fast build**
      (DJ4:70). **fix (F175 — testability; F191 removes its re-read run):** DJ4:70 becomes:

    > | Save a copy (R7.3h) running on R7.7's ROWS_CEILING corpus | Delete (or clear) while it runs | The delete shows
    > done without waiting; E35 does not render; within 5 s of the later of the delete landing and the copy's end, a
    > functional timeout, the removed text is in the active file's bytes nowhere (R6.2a, F57, F58) | none |

    A short-read build passes by the ordinary rule and a long-read build by the deferral; the not-exercised clause goes.
    The re-read run moves to DJ3 as F191-3's held line.
- [ ] **PERF6-4 (Minor) — F175's list of the app's long reads leaves out the open-file check.** **fix (F195):** F195-1.
- [ ] **PERF6-5 (Nit) — byte reads taken "once it ends" or "after reopening" have no tolerance.** **covered by 6MN1**
      (the Harness, EJ1, UJ9.7-h) and **R6-m5** (DJ4:66).
- [ ] **carried: PERF5-2 (PARTIAL) — its UJ9.5-d half.** **covered by PERF6-2.** The round-5 ruling's held export for
      UJ9.5-d (reported as a fork) is superseded: the outside read now makes the overlap, and EJ1's held export already
      proves the snapshot.
- [ ] **carried: PERF4-3 (PARTIAL) — UJ9.5-d's export finishes before the edit.** **covered by PERF6-2.**
- [ ] **carried: PERF3-7 (PARTIAL) — its overlap half.** **covered by PERF6-2.**
- [ ] *unrated* **Performance Missing (out of lens) — Export R4.3 still reads every value, mark and note back
      "identical", which its hold and EJ1's mid-export edit contradict** (Export R4.3). **fix (F158 — editorial: the same
      contradiction F33 removed from R1.1; no new WHAT):** R4.3's "…it reads every value, mark and note back identical at
      SQLITE_READER_FLOOR ([R1.1])" → "…it reads back at SQLITE_READER_FLOOR that the export itself changed no value, mark
      or note ([R1.1])" (+4 Export); its Commit PR cell "…amended 2026-09-25 (F33), alignment kept" → "…(F33, F34),
      alignment kept" (+1). Meaning check against F158 ("an edit made while it runs saves at once and is not in it") and
      R1.1's clause. Recorded in SIB-5.

The review's other risks are reported, not made: the cold All items open on the 8 GB Mac (est. 0.9–2.4 s against 1 s)
and a ceiling delete's free-space need are OQ 1's to measure; the unbudgeted open-file check on a FILE_ITEMS_CEILING file
has no fence (F119 leaves it out of the cold open), and F195 only places it among the long reads; log growth under a
long outside read is pre-existing and unbounded by any fence. Its cautions stand: no new latency constant, and UJ9.5-d's
overlap no longer depends on how fast the UI is driven.

## peer-plan-reviewer (Claude route, pre-lock lens)

- [ ] **PLAN-1 (Major) — Build dependencies: ADR-0005 is missing from the stops** (PRD:309–312). Log row 3. **fix
      (F192):** F192-1. Its second bullet, "Interim: the device PRD's OQ 21" for R8.10a's OS seams and the macOS test
      lane: the seam-placement half is **covered by F192** (the seams wait for ADR-0005, a stop, so no interim is
      needed); the macOS CI lane that M2's and M3's "every PR" runs presume — **needs owner:** naming a sibling's open
      question as governing this PRD's metric cadence is an interim no fence states. Nothing is made for it.
- [ ] **PLAN-2 (Major) — the first build phase never says it needs the sibling PRDs built** (the Harness's Phases and
      builds; the Named defaults' Build phase). Also answers P5-m1 and the Phase 5 Missing item on sibling rows pulled
      forward; log row 4. **fix (testability/plan — no new WHAT; companion only):** the Harness's Phases and builds
      paragraph gains, after its last sentence (as PERF6-2 leaves it):

    > This PRD's first build phase runs on the sibling rows and states its first-phase cases drive, each built in its
    > own PRD's phase: the capture PRD's collection creation (its R1.1–R1.3), declared sessions (its R11.6) with their
    > R11.7 timings and R11.11 readback, its session start, settings and set-aside list, and its E1, E2, E23, E25 and
    > E26; the device PRD's Demo Device saves, its R6.9, R6.12 and R6.17 and its E22; the Data Foundation PRD's R7.2 and
    > R7.4 declared states and its E1, E4, E8, E9, E10, E11, E14, E15, E26, E31, E34 and E35; the Data Export PRD's E1;
    > and the import PRD's preview and commit. A case driving a sibling row of a later phase than that PRD's first runs
    > once that row lands, as UJ4.5-b's Given states; and R1.1 and R3.2 build with them the code comparison the capture
    > PRD's R6.9 states, nothing else of that row applying to a view sort.

    The sibling rows it names are P0 in their PRDs (checked: Capture R1.1–R1.3, R1.5, R1.10, R3.5, R3.11, R8.16, R11.6,
    R11.7 and R11.11; DF R7.2 and R7.4; device R6.9, R6.12 and R6.17); a state whose owning sibling row is later runs
    under the paragraph's second sentence. The last clause is P5-m1's: it restates what
    R1.1 and R3.2 (P0) already require — Capture R6.9 keeps its P1 Pri — so check 1b's re-run disposes it as a
    restatement, no sibling Pri moving. No Build dependencies row is added (the optional row would cost body words the
    Harness does not need).
- [ ] **PLAN-3 (Major) — UJ9.5-c: a first-phase case fires a P1 bulk action** (UJ9.5-c; the UJ 9 preamble). Also answers
      P5-n2; log row 5. **fix (testability):** UJ9.5-c's When "Search, filter, sort, open an item, set a field on every
      item, delete one item, read both long values back, and fire "Show history" on the third item" → "Search, filter,
      sort, open an item, delete one item, read both long values back and fire "Show history" on the third item; in the
      phase that lands R6.1 and R6.2, also set a field on every item through "Select all" and "Set a field""; Assert and
      Rows unchanged (R8.1, R5.1 — the case stays first-phase). The UJ 9 preamble's "…and UJ9.5-a's P1 and P2 inputs in
      the phases that land theirs" → "…and UJ9.5-a's and UJ9.5-c's P1 and P2 inputs in the phases that land theirs" (the
      preamble's other edit is F189-8's).
- [ ] **PLAN-4 (Minor) — bulk delete doesn't name its dependency on multi-row selection** (Build dependencies row 5; the
      UJ 6 preamble; UJ9.5-g). **fix (editorial/testability — R6.3 fires "with two or more items selected"):** row 5's
      Available contract "R6.3; …" → "R6.1, R6.3; …" (+1); the UJ 6 preamble "…scenario 3 in the phase that lands R6.3,
      …" → "…scenario 3 in the phase that lands R6.1 and R6.3, …"; UJ9.5-g's "In separate timing runs, each in the phase
      that lands its row:" → "In separate timing runs, each in the phase that lands its rows, the selection delete's
      R6.1 and R6.3:".
- [ ] **PLAN-5 (Minor) — "reordering" has no phase mark in Surfaces.** **covered by IF6-N1.**
- [ ] **PLAN-6 (Minor) — Build dependencies cells don't match their rows.** **fix (editorial — the table is an index;
      no new WHAT), the review's cheaper option:** the paragraph under the table ends "…and is re-checked when it
      closes." → "…and is re-checked when it closes. The Interim stated list and each row's cites complete these cells."
      (+11); and the Data Foundation PRD's E14 moves from row 2's Available contract to row 1's, where R1.4 sits (row 1
      "…E10, E15, E34 and E35" → "…E10, E14, E15, E34 and E35", +1; row 2 "…E4, E8, E11, E14, E26 and E31" → "…E4, E8,
      E11, E26 and E31", in F189-2's text). The omitted OQs (row 1's OQ 2 and OQ 5; row 3's OQ 1, OQ 5 and OQ 12; rows 4,
      8 and 10's OQ 1) and sibling states (Export E1, DF E1 and E9, Capture E25) are then read from the Interim stated
      list and the rows' own cites.
- [ ] **PLAN-7 (Minor) — R8.11's interim points at a document that doesn't exist.** Also answers P5-m7. **fix (F120 —
      testability: F120 settles where the value comes from, and the case waits for it):** in PERF6-2's UJ9.5-d text —
      "ROW_CONFIRM_BUDGET being the engineering plan's declared value (F120), and a run before that plan declares it
      reporting the row-confirmation half not exercised, never passed". The declaration is left to the plan's author,
      not the builder. 0 body words.
- [ ] **PLAN-8 (Minor) — ADR-0003 ordering can quietly defer R1.7.** Also answers the review's spec-coverage gap on
      F57's release trigger. **fix (F196):** F196-1, F196-2, F196-3.
- [ ] **PLAN-9 (Minor) — the only case for undo surviving E34 needs R1.7, which may be deferred.** **fix
      (testability):** in 6MN3's UJ9.7-i text — the Given's "R4.7, R6.1 and R6.2 built", E10's "Undo" asserted "where
      R1.7 is built" in (a) and (b), and Rows "R8.8, R4.7". The UJ 9 preamble keeps UJ9.7-i among the cases run in the
      phase that lands their rows.
- [ ] **PLAN-10 (Nit) — R7.2 leaves two swatch sizes unnamed.** **needs owner:** naming a default and a largest size is
      a new constant, and "a default and range the build chooses" a new rule; no fence states either (F20 and F71 set the
      floor and what persists). Recommendation to carry: no change — R7.2's floor is its only bound, the row is P2, and
      R6-m2's UJ10.1-d now records the default rather than assuming one. Nothing is made.
- [ ] **PLAN-11 (Nit) — sRGB integer rounding is unstated** (the Named defaults' ZX-001 line). **fix (F105 —
      testability: the fixture already declares 91 for a red of 90.51–90.55):** "…sRGB (91, 157, 211) and HSL (207°, 58%,
      59%)…" → "…sRGB (91, 157, 211), each channel rounded, and HSL (207°, 58%, 59%)…". R2.11 is unchanged.
- [ ] **PLAN-12 (Nit) — E8's undo promise relies on grouping the table doesn't enforce; row 9 puts P2's R5.8 in a P1
      unit.** **fix (editorial — copy honesty, process rule 5, makes E8's "Undo change" sentence true when it renders;
      no new WHAT):** row 4's Work "Selection of several rows, bulk set or clear, and undo" → "…, and undo, landing
      together" (+2); on the orchestrator's ruling, R5.8 moves from row 9 to row 10: row 9 "Restore, distance from
      current, compare | R5.4, R5.5, R5.8; …" → "Restore and distance from current | R5.4, R5.5; …"; row 10 "Swatch grid and
      column visibility | R2.10, R7.1, R7.2; … | Stop: ADR-0003; Interim: OQ 6" → "Swatch grid, column visibility and
      compare | R2.10, R5.8, R7.1, R7.2; … | Stop: ADR-0003; Interim: OQ 6; Interim: OQ 11" (+4).
- [ ] *unrated* **Plan spec-coverage gap — this PRD's own follow-ons are not in post-lock.md** (AGENTS.md §2: a new
      PRD's lock appends them, grouped by trigger). **fix (editorial — AGENTS.md §2; each item restates an OQ's Closer,
      a metric or a fence, no new WHAT), in this pass or at the lock pass on the orchestrator's ruling.** The other two
      gaps are covered: F57's release trigger by PLAN-8 (F196-3), OQ 7's closer by F197-1. Drafted, each ending
      "(Collection Mode, its round-6 plan review; added in this change's PR, number pending)":
  - § First build PR: "- [ ] **Collection Mode** — OQ 11: engineering checks every quoted CIEDE2000 pair against the
    published table before the first "Find similar" or distance case runs."
  - § v1 release (F196-3's section): "- [ ] **Collection Mode** — OQ 1: engineering times Release builds at ROWS_CEILING
    on the M1 MacBook Air with 8 GB and one current Mac; the owner ratifies the OQ 1 constants and estimates
    HISTORY_READINGS_CEILING."
  - A new "## Dogfood" section after "## ADR-0003", its one-line intro "Readings the owner takes while dogfooding a
    build.": OQ 2 (the owner's estimate of collections per file, checked by UJ9.5-b); OQ 3 (a "Find similar" pass over a
    real collection of at least 200 items, counting what each query lists; the owner); OQ 4 (dogfood bulk edits, then
    the owner); OQ 5 (a hue sort of a collection holding a grey series; the owner); OQ 6 (the owner at the P2 build, on
    their display at working distance); OQ 12 (the widest inventory the owner dogfoods); M4 (each session's unanswered
    re-scans read a week later through "Answer re-scans"); and the E12 reading beside M4 (F82, F134: on a Display P3
    display with an outside-sRGB swatch listed, the owner says what each mark tells them). One item each, "**Collection
    Mode** — ⟨question⟩: ⟨closer⟩".

The review's risks are carried: build order across PRDs (PLAN-2), module placement (PLAN-1), delete-undo by default
(PLAN-8), the two unscheduled oracle inputs (PLAN-7; the M1 Air is OQ 1's interim machine, per-PR timing only a tripwire
under F41 — reported), and the budget (this list's ledger).

## Phase 5 priority pass — peer-product-manager-reviewer (focused review)

- [ ] **P5-M1 (Major) — the P0 build has no non-destructive way to correct a captured item's colour value** (R4.9, R5.5,
      R4.6; the §1, §4 and §5 trace lines). Log row 6. **fix (F189):** F189-1 to F189-14. The trace lines need no change:
      with the Flag at P0 they are true.
- [ ] **P5-m1 (Minor) — R1.1 and R3.2 (P0) depend on the capture PRD's R6.9 (P1).** **covered by PLAN-2** (the Harness
      sentence's last clause).
- [ ] **P5-m2 (Minor) — "reordering" carries no mark.** **covered by IF6-N1.**
- [ ] **P5-m3 (Minor) — the capture PRD's Flag and re-scan entry appear on the item detail with no phase mark.** Its
      Flag half is **covered by F189** (F189-5: at P0 the Flag takes no mark). Its re-scan half: **fix (editorial — the
      index follows R4.6's "once that PRD's re-scan rows land"; no new WHAT):** the item detail's Shows cell "…"Change
      code" [phase: action-absent] and "Undo change" [phase: action-absent] among them" → "…"Change code" [phase:
      action-absent], "Undo change" [phase: action-absent] and the capture PRD's re-scan entry [phase: action-absent]
      among them" (+7). The copy file's counterpart is P5-n6's sentence (check 2, as IF6-N1 notes). R8.10b's "actions
      offered" at P0 can then be read from the marks.
- [ ] **P5-m4 (Minor) — the UJ 4 preamble puts UJ4.5-b in the first phase, but it needs the capture PRD's P1 re-scan
      rows.** **fix (testability):** in F189-6's preamble text ("UJ4.5-b, which runs in the phase that lands the capture
      PRD's re-scan rows").
- [ ] **P5-m5 (Minor) — E4's "all items" variant has no `[phase: variant-absent]` mark.** **fix (editorial — the F97
      and F111 pattern; phase bookkeeping):** the copy file's E4 "all items" Variant line ends "…Capitals and extra
      spaces don't count as a difference. [phase: variant-absent]". E4 keeps its alignment (a mark, not wording, as
      L1b-2 ruled for DF E33).
- [ ] **P5-m6 (Minor) — the Legend says every row ships in this release, but R1.7 is conditionally deferred.** **fix
      (F57, F187 — a restatement of R1.7's own deferral):** the priority bullet "Every P0/P1/P2 row ships in this
      release;" → "Every P0/P1/P2 row but a deferred R1.7 ships in this release;" (+4). Check 19's pattern (the bullet's
      bold lead) is unchanged. A Legend priority change is not editorial: it goes to the round-7 delta.
- [ ] **P5-m7 (Minor) — R8.11's row-confirmation half has no number.** **covered by PLAN-7.** Its "numeric interim"
      option would be a new interim; not taken.
- [ ] **P5-n1 (Nit) — the phase rule says "rows are P1", but P2 actions carry the same marks.** **fix (editorial — the
      marks already cover P2's "Columns", "Grid", "Table" and "Compare"):** "A P0 state offering an action whose rows are
      P1 shows without it…" → "…whose rows are of a later priority shows without it…" (+3).
- [ ] **P5-n2 (Nit) — R8.1f's and R8.1g's inputs are P1 (but "Delete collection").** **covered by PLAN-3**: the UJ 9
      preamble's phase clause, which already times UJ9.5-a's P1 and P2 inputs in the phases that land them; no R8.1
      text changes, so the R8.1 family's status path is not reopened.
- [ ] **P5-n3 (Nit) — R5.7, R2.4i and R4.2h are P0 though only the capture PRD's P1 re-scan produces their state.**
      **covered by F1 and F37** (the honesty marks complete at P0); the reviewer asks to keep them. M4's first-build read
      is reported with the review's risks.
- [ ] **P5-n4 (Nit) — telemetry and update checks declared "off" through device R2.13 and R2.14, both P1.** **fix
      (testability — companions only; R8.10a's row text on the orchestrator's ruling):** the Named defaults' Network
      "Reachable, with telemetry off and update checks off (the device PRD's R2.13 and R2.14)" → "Reachable, with
      telemetry off and update checks off where the device PRD's R2.13 and R2.14 are built"; UJ9.4-c's Given "…telemetry
      and update checks off" → "…telemetry and update checks off where built".
- [ ] **P5-n5 (Nit) — R2.2 renders the capture PRD's E2 empty variant, whose body offers the P1 "Add a swatch".**
      **covered by the capture PRD's copy convention** (its E2 body "stands as guidance"); the reviewer places it there,
      not here. No change.
- [ ] **P5-n6 (Nit) — no mark kind covers a whole state absent on a surface that exists.** **fix (editorial — the
      Legend's phase rule restated; copy file only):** after the Phase marks paragraph ending "…and the state is complete
      without it.", add: "Two capabilities the PRD's Surfaces table marks `[phase: action-absent]` have no label in this
      file: the collection surface's drag reorder (R2.9) and the item detail's entry to the capture PRD's re-scan
      (R4.6); each is absent until its rows land. A state whose owning rows are all of a later priority carries no mark:
      it arrives with those rows, as E10 does with R1.7."

The review's table rows are each carried above (R4.9 RAISE: P5-M1; R1.1 and R3.2: P5-m1; R2.9: P5-m2; R4.6: P5-m3 and
P5-m4; R3.5: P5-m5; R1.7: P5-m6; R8.1f/g: P5-n2; R8.11: P5-m7; R5.7: P5-n3; R8.10a: P5-n4; R2.2: P5-n5). Its risks are
carried: the correction gap (F189); gates beyond priority — ADR-0003, ADR-0005 (F192), ADR-0006, ADR-0004 and Capture
OQ 8 — and the 8 unaligned P0 lead rows (the status bookkeeping above); comparator drift (P5-m1).

## Owner decisions F189–F197 — the edits each requires

Next free IDs, verified by reading the files at `71618a0`: DF fences end at F57 (no "F58" appears in any DF file), so
the round-6 Data Foundation fence is **DF F58**, under a new "## Collection Mode round-6 amendment (2026-09-25)" heading
as F57 has, and ARCH6-1's Corrected line goes under **DF F57** — its (1) holds the phrase "a read begun after the
delete deferring it too" (DF fences:598). Capture fences end at F75, so **Capture F76**. Export fences end at F33, so
**Export F34**. Import fences end at F68; its round-6 edits are editorial labels, recorded as a dated line under
**Import F68** (SIB-4), or as **Import F69** on the orchestrator's ruling. This PRD's UJ 7 scenario 1 ends at UJ7.1-s, so
the new case is **UJ7.1-t**. No new requirement row or copy state is needed in any document: R4.9, R8.8, DF R1.11,
R6.2a, R7.6b, OQ 20, E15, E34, E35, Capture R9.9 and Export R4.3 are amended in place.

### F189 — The collection-side Flag is P0

Every place the Flag's P1 status is stated or assumed, found by reading the PRD, the copy file, the journeys and the
capture PRD:

- [ ] **F189-1 — R4.9's Pri cell** (§4). "| R4.9 | v1 | P1 |" → "| R4.9 | v1 | P0 |"; Status "aligned" → "pre-alignment".
      The row's text is unchanged.
- [ ] **F189-2 — the Build dependencies rows holding R4.9** (with PLAN-6's E14 move). Row 2 "| Item detail, editing and
      history | R4.1–R4.3, R4.5, R4.6, R5.1–R5.3, R5.6, R5.7; copy E14, E17; the Data Foundation PRD's E4, E8, E11, E14,
      E26 and E31; the capture PRD's R8.18 |" → "| Item detail, editing and history | R4.1–R4.3, R4.5, R4.6, R4.9,
      R5.1–R5.3, R5.6, R5.7; copy E14, E17, E18; the Data Foundation PRD's E4, E8, E11, E26 and E31; the capture PRD's
      R5.6, R8.18 and R9.9 |" (+4); row 7 "| Code change, column rename and Flag | R4.4, R4.8, R4.9; copy E6, E11, E15,
      E16, E18, E19; the Data Foundation PRD's R1.2, the import PRD's R2.6, and the capture PRD's R5.6 and R9.9 |" → "|
      Code change and column rename | R4.4, R4.8; copy E6, E11, E15, E16, E19; the Data Foundation PRD's R1.2 and the
      import PRD's R2.6 |" (−9). Both keep "Stop: ADR-0003". Rows 1 and 2 then cover every P0 row, as the Phase 5 review
      found them to.
- [ ] **F189-3 — the Legend's P0 meaning** (the Pri table), on the orchestrator's ruling. "…a collection cannot be
      browsed, searched, read honestly, edited, deleted, or its history reached without it." → "…edited, deleted, its bad
      scans flagged, or its history reached without it." (+4). Meaning check against F189's Authority ("At P0 you can
      Flag a captured swatch from its detail, then review and re-scan it through capture's own P0 rows. History is
      kept."). Without it, R4.9 is the one P0 row the P0 meaning does not name.
- [ ] **F189-4 — E6's base body** (copy file). "Deleting a swatch or the collection and bringing back an earlier reading
      aren't available until you end that session" → "Deleting a swatch or the collection, flagging a swatch and bringing
      back an earlier reading aren't available until you end that session". E6 stays pre-alignment (rewritten). Copy
      honesty: in the first phase R8.3 refuses, on the collection, "Delete swatch", "Delete collection", the capture PRD's
      Flag and the Data Foundation PRD's E4 restore — the four the body now names. The "full" variant, which already
      names flagging, is unchanged; its condition ("every P1 action that variant names") still gates it.
- [ ] **F189-5 — E18, E14 and the Surfaces item-detail row: verified, no edit.** E18's base (Phase none) renders from the
      first phase with R4.9; its "restore" variant keeps `[phase: variant-absent]` (R5.5 is P1); E18 stays aligned.
      E14's Actions list this PRD's labels only — the capture PRD's Flag is named by document (the Labels rule) and never
      carried a mark there. The Surfaces item-detail row names no Flag and no mark, and a P0 action takes none; its
      re-scan-entry mark is P5-m3's.
- [ ] **F189-6 — the UJ 4 preamble** (with P5-m4). The whole preamble becomes: "Scenarios 1, 2, 4, 5 and 7 run in the
      first build phase, except UJ4.4-e, UJ4.4-g and UJ4.4-j, which run in the phase that lands R1.7, UJ4.5-b, which runs
      in the phase that lands the capture PRD's re-scan rows, and UJ4.7-f, which runs in the phase that lands R5.5;
      scenario 2 runs under continuity, each of its cases against the state the previous one left, UJ4.7-d runs against
      the state UJ4.7-a left, and the others each case from the harness state or its own Given. Scenario 3 runs in the
      phase that lands R4.4 and scenario 6 in the phase that lands R4.8, each case from the harness state or its own
      Given, except that UJ4.6-e and UJ4.6-f each run against the state UJ4.6-a's When leaves." UJ4.7-f alone needs a P1
      row (R5.5).
- [ ] **F189-7 — the "R4.9 built" Givens** (journeys). T14 "Studio Markers item ZX-001 present and captured; R4.9
      built" → "Studio Markers item ZX-001 present and captured"; UJ4.7-a, UJ4.7-b and UJ4.7-e "The seeded file; R4.9
      built" → "The seeded file"; UJ4.7-c "The seeded file; R4.9 built; a bulk session…" → "The seeded file; a bulk
      session…"; UJ4.7-d "The state UJ4.7-a left; R4.9 built" → "The state UJ4.7-a left"; UJ4.7-f "The seeded file; R4.9
      and R5.5 built" → "The seeded file; R5.5 built"; UJ9.1-f "The seeded file; R4.9 built; Studio Markers…" → "The
      seeded file; Studio Markers…". Asserts and Rows unchanged.
- [ ] **F189-8 — the UJ 9 preamble** (with PLAN-3). "…except that UJ9.1-f, UJ9.1-g, UJ9.1-p, …" → "…except that UJ9.1-g,
      UJ9.1-p, …": UJ9.1-f's rows (R3.9, R6.4, R4.9) are all P0 now.
- [ ] **F189-9 — UJ9.2-a** (read-only). Its Assert's list "…"Delete swatch", "Answer re-scans", field editing and, in
      later phases, every P1 and P2 write action…" → "…"Delete swatch", "Answer re-scans", the capture PRD's Flag
      action, field editing and, in later phases, every P1 and P2 write action…": the Flag is a first-phase write action
      R8.4 disables.
- [ ] **F189-10 — UJ9.8-b** (M3). "Run the Whens of UJ4.2-a and UJ5.2-c, and, in the phases that land their rows, of
      UJ5.3-c, UJ4.3-a, UJ6.2-c, UJ8.1-a, UJ4.6-a and UJ4.7-a;" → "Run the Whens of UJ4.2-a, UJ5.2-c and UJ4.7-a, and, in
      the phases that land their rows, of UJ5.3-c, UJ4.3-a, UJ6.2-c, UJ8.1-a and UJ4.6-a;".
- [ ] **F189-11 — this PRD's inbound Capture R9.9 line** (Inherited obligations › Inbound). "Its R9.9: a Flag from the
      collection on a captured item, doing what its R5.6 Flag does" → "Its R9.9: a Flag from the collection on a captured
      item, at P0, doing what its R5.6 Flag does" (+2), so both summaries of the seam say P0.
- [ ] **F189-12 — this PRD's Outbound Capture Mode row** (with IF6-N3). Its Obligation "…words each cause R4.2b shows;
      and its FIND_BUDGET closes…" → "…words each cause R4.2b shows; its R8.14 and R9.9's Flag entry at P0; and its
      FIND_BUDGET closes…", its Rows "R4.2b, R4.5, R8.1f, R8.3" → "R4.2b, R4.5, R4.9, R8.1f, R8.3" (+9: F189's part +5,
      IF6-N3's +4).
- [ ] **F189-13 — the capture PRD's R9.9, the row carrying the collection-side Flag entry** (Capture §9). "…a "Flag"
      from the collection — the Collection Mode PRD's entry point on a captured item ([its R4.9])…" → "…the Collection
      Mode PRD's P0 entry point on a captured item ([its R4.9])…"; Pri stays P1 (its one-row-session rule is P1); Commit
      PR "amended 2026-09-24 (F70), alignment kept" → "…; amended 2026-09-25 (F76), alignment kept". Still two sentences
      (check 10). P0 for that entry only, as F189 decides.
- [ ] **F189-14 — the capture PRD's Collection Mode obligation line** (Capture PRD:386). "…the collection-side "Flag"
      entry point on a captured item, doing what R5.6's Flag does;…" → "…the collection-side "Flag" entry point on a
      captured item, at P0, doing what R5.6's Flag does;…". Recorded, with F189-13, in SIB-3 (Capture F76). What a Flag
      does stays Capture R5.6's, already P0; Capture R8.16 and R8.3, which the flagged item's review and re-scan use,
      are P0 already. The Data Foundation PRD needs nothing: its R2.9 (P0) already names the Flag.

### F190 — E35 goes with its file

- [ ] **F190-1 — DF R6.2a** (DF Deletion lifecycle). "…one [E35] naming another app's until the wipe lands, OK hiding
      it until another such write)…" → "…one [E35] naming another app's until the wipe lands or the file closes, OK
      hiding it until another such write or the next open)…" (+8 DF; the tight form "…or open)" is +6, on the
      orchestrator's ruling). Meaning check against F190's Authority ("E35 closes with the file and never shows over
      another file. At the next open, if the other app is still reading, it shows again, even if you'd pressed OK before.
      Nothing is remembered outside the file."): closing ends it, a switch closes the file first, the OK's hide ends at
      the next open, and nothing new is kept (DF R1.1 already keeps nothing about a collection outside the file).
      Alignment kept; R7.6q unchanged.
- [ ] **F190-2 — DF DJ4** (DF journeys). DJ4:67 becomes:

  > | That read still running across a quit and a reopen | Delete (or clear); quit and reopen; then end the read — and in
  > a second run fire E35's OK before the quit | At the reopen, in both runs, the removed text is still in the file's
  > bytes and E35 is up; within 5 s of the read's end it is in the file's bytes nowhere and E35 is not up (R6.2a, F57,
  > F58) | E35 |

  and a new line follows it:

  > | That read still running; a second file kept elsewhere (R1.3) | Delete (or clear); with E35 up, close the file —
  > and separately open the second file; then reopen the first with the read still running | Once the file closes, or
  > the second file opens, E35 is not up, and it never shows over the second file; at the reopen E35 is up again
  > (R6.2a, F58) | E35 |

  The second run of DJ4:67 is PM6-2's; the new line is 6MN2's.

### F191 — Edits wait while a re-read runs

- [ ] **F191-1 — DF R1.11** (DF §1). "…an [Import R3.2] commit or an R1.9 move runs, no capture starts…" → "…an [Import
      R3.2] commit, an R1.9 move or a re-read runs, no capture starts…" (+2 DF). The row then holds, for a re-read, every
      write and session start, shown disabled, app-made writes deferred, and the waiting close, switch or quit — "like the
      one-writer rule", as F191's chosen option says. ⌛️ Ready for Alignment, unchanged. Meaning check against F191's
      Authority ("While a re-read runs, edits and other writes show unavailable, like the one-writer rule, with progress
      shown.").
- [ ] **F191-2 — DF R7.6b** (DF Surfaces, "What the test lists"). "…an upgrade's progress,…" → "…an upgrade's or a
      re-read's progress,…" (+3 DF). DF R7.6 makes the Surfaces table the rule, so this states the progress F191
      decides and makes it listable (PERF5-3's precedent for a move's progress). R7.3c needs no text. Alignment kept.
- [ ] **F191-3 — DF DJ3** (DF journeys), a new line after DJ3:42:

  > | No capture in flight; Read it again's checks (R7.3c) running on R7.7's ROWS_CEILING corpus | While they run, change
  > an item's metadata, try a capture start and choose the file-location move; then again once they end | While they
  > run the re-read's progress shows (R7.6b), each shows disabled and nothing is written, started or moved; once they
  > end each is offered; a run whose checks ended before the first try reports not exercised, never passed (R1.11, F58)
  > | E9 / re-read progress (R7.6b) |

- [ ] **F191-4 — DF DJ4:70** — its re-read run goes: in PERF6-3's text.
- [ ] **F191-5 — the Harness and the ADR-0003 input** — a re-read's checks leave the long reads that may defer a wipe,
      ending before the file takes a write: in 6MN1's Harness text and F195-1's ADR text. This PRD's R8.5 and R4.7 are
      unchanged; no Collection Mode case adds a write during a re-read (DJ3 carries it, R8.1f citing DF R1.11).

### F192 — ADR-0005 is a stop on every row

- [ ] **F192-1 — the Build dependencies paragraph.** "…that work waits for the ADR or question named, ADR-0003 and
      ADR-0006 stopping every row —…" → "…ADR-0003, ADR-0005 and ADR-0006 stopping every row —…" (+1). Meaning check
      against F192's Authority ("Every row waits for ADR-0003, ADR-0005 and ADR-0006."). No row's Stop cell changes (F124's
      pattern: the paragraph carries the every-row stops). The ADR queue's 0005 row is unchanged ("depends on 0001").

### F193 — DF E15 and E34 name the last change

- [ ] **F193-1 — DF E15 and E34** (DF copy). In both bodies, "…so the last thing you did wasn't saved." → "…so your last
      change wasn't saved." E15 keeps its alignment (owner-approved wording, as F170's change to E15 did); E34 stays ⌛️
      Ready for Alignment. R7.6f ("What it names as unwritten") and R7.6p ("What it names as unsaved") are unchanged.
      Copy honesty: during a hold no other write starts, so the held write is the last change.

### F194 — DF E35 is plural

- [ ] **F194-1 — DF E35** (DF copy). "Your change is saved." → "Your changes are saved." Status ⌛️ unchanged; R7.6q's
      test prose ("That the change is saved") stays, as the reviewer notes.

### F195 — The open-file check beside the long reads

- [ ] **F195-1 — the ADR-0003 row** (`docs/decisions/README.md`; with 6MJ1 and F191). Two edits, still eight inputs:
  - the removing-write input "…a text-removing write is saved without waiting on any read, only its wipe following the
    end of every read begun before that wipe, never holding a write, and every read this app makes but an export, Save a
    copy and a re-read's checks runs in transactions short enough that the wipe follows within Collection Mode's
    BROWSE_RESPONSE_BUDGET, so capture saves never wait on a removing edit;" → "…a text-removing write is saved without
    waiting on any read, its wipe waiting at most until every read begun before that wipe has ended, never holding a
    write, and every read this app makes but an export and Save a copy runs in transactions short enough that the wipe
    follows within Collection Mode's BROWSE_RESPONSE_BUDGET, a re-read's checks and the open-file check (Data Foundation
    R1.11, R5.4) ending before the file takes a write, so capture saves never wait on a removing edit;";
  - the fence list "[its F35, F50, F85, F102, F103, F127, F139, F146, F157, F158, F171, F175, F183 and F184]; [Data
    Foundation F52, F53, F54, F56 and F57]" → "[its F35, F50, F85, F102, F103, F127, F139, F146, F157, F158, F171, F175,
    F183, F184, F191 and F195]; [Data Foundation F52, F53, F54, F56, F57 and F58]".

  Meaning check against F195's Authority ("names the open-file check … beside the export and Save a copy: it ends
  before the file takes a write"), F175's F191 line ("an export and Save a copy remain the app's own long reads that may
  defer a wipe, beside the open-file check … which ends before the file takes a write") and F184 read as a deadline. The
  0007 row is unchanged.
- [ ] **F195-2 — the Harness's Byte checks** — in 6MN1's text ("that PRD's … open-file check (its R5.4) ending before the
      file takes a write").

### F196 — ADR-0003 need not wait for delete-undo

- [ ] **F196-1 — Build dependencies row 6.** "| Undo of a delete | R1.7; copy E10 | Stop: ADR-0003; Stop: OQ 10 — the
      Data Foundation PRD's OQ 20 |" → "| Undo of a delete | R1.7; copy E10 | Stop: ADR-0003 or a later
      undo-representation ADR; Stop: OQ 10 — the Data Foundation PRD's OQ 20 |" (+5; the clearer "Stop: ADR-0003, or a
      later ADR fixing its undo representation" is +8). Meaning check against F196's Authority ("its undo representation
      may come in a later ADR, so ADR-0003 need not wait for the Data Foundation PRD's OQ 20"). R1.7 unchanged.
- [ ] **F196-2 — DF OQ 20's Closer** (the DF half, process rule 12: DF's closer named ADR-0003 as where the undo
      representation lands). "Owner visibility/conflict decision, then ADR-0003 undo representation." → "Owner
      visibility/conflict decision, then an ADR's undo representation." (+1 DF). OQ 20 stays open; recorded in DF F58 (6).
- [ ] **F196-3 — the post-lock release item** (`docs/product/post-lock.md`, a new "## v1 release" section after "##
      Documentation", its intro "Decisions taken when v1 is cut."): "- [ ] **Collection Mode / DF** — at v1 release the
      owner decides delete-undo's fate: if the Data Foundation PRD's OQ 20 is still open, Collection Mode R1.7 and DF R6.3
      are marked deferred and v1 ships final deletes behind the counted confirmation and export first — a decision, not a
      default (Collection Mode F57, F187 and F196, its round-6 plan review's PLAN-8; added in this change's PR, number
      pending)." Placement on the orchestrator's ruling: post-lock.md has no release trigger yet.

### F197 — Three post-lock items

Each in post-lock.md's style, "added in this change's PR, number pending"; placement on the orchestrator's ruling.

- [ ] **F197-1 — OQ 7's display spike** (§ First build PR, the trigger for engineering checks before the build relies on
      an interim): "- [ ] **Collection Mode** — OQ 7: an engineering spike on one sRGB and one Display P3 display with the
      harness's seeded file, then the owner, settles the display check R2.5 and M2 run on under OQ 7's interim
      (Collection Mode F197, its round-6 plan review; added in this change's PR, number pending)." The hardware spike
      brief does not carry it, as the plan review says it should not.
- [ ] **F197-2 — a file renamed or moved while open** (§ ADR-0003): "- [ ] **DF** — a file renamed or moved outside the
      app while open keeps every committed write and survives a crash, R1.10's promise holding for the file under its
      new name (Collection Mode F174 and F197, its round-6 engineering and architecture reviews' 6MN3 and ARCH6-3; added
      in this change's PR, number pending)." ARCH6-3's mechanism ("notices the move, copies in and truncates its log")
      is left to ADR-0003, not written into the item.
- [ ] **F197-3 — R8.11 on a local volume** (a new "### Collection Mode" subsection under "## Next pass over a locked
      PRD", after "### Inventory Import"; R8.11's own text is where it lands next): "- [ ] **Collection Mode** — R8.11's
      timing is promised on a local volume only, Data Foundation R1.10's scope, as ADR-0003's non-waiting save is
      (Collection Mode F183 and F197, its round-6 architecture review; added in this change's PR, number pending)."

### Sibling fences, status lines, the ADR queue and the fence map

- [ ] **SIB-1 — DF F58** (DF fences; new, under "## Collection Mode round-6 amendment (2026-09-25)"). Source paragraph:
      "Source: the owner's round-6 (pre-lock) decisions in the Collection Mode PRD's adjudication of 2026-09-25, recorded
      there as [its F190 and F191], and its approved round-6 recommendations 1–4, recorded as [its F193, F194, F195 and
      F196], with the editorial and testability halves its round-6 fix pass carries under its F7, F157, F164, F173,
      F174, F176, F182, F184 and F187. This fence carries only the Data Foundation halves of those decisions; peer review
      pending." Then:
  - **Title:** "F58 — E35 goes with its file, a re-read holds writes, the last-change and plural wording, the open-file
    check beside the long reads, and a later ADR for the undo representation (2026-09-25)".
  - **Authority:** [the Collection Mode PRD's F190, F191, F193, F194, F195 and F196] — owner decisions D48 and D49 of its
    round-6 (pre-lock) adjudication, 2026-09-25 (F190, F191), and its approved round-6 recommendations 1–4 the same day
    (F193–F196).
  - **Decision:** (1) Under the Collection Mode PRD's F190, R6.2a's E35 goes when the file closes and never shows over
    another file; at the next open, while the other app still reads, it shows again, an earlier OK notwithstanding;
    nothing about it is kept outside the file (R1.1); DJ4 asserts the close, the switch and a reopen after OK. (2) Under
    its F191, R1.11 names a re-read among the operations that hold writes, and R7.6b lists a re-read's progress; a
    re-read's checks therefore never overlap an edit, which supersedes F57 (3)'s clause that R7.3c's checks defer a wipe
    and DJ4's re-read run; DJ3 asserts the hold. (3) Under its F193, E15 and E34 read "so your last change wasn't
    saved". (4) Under its F194, E35 reads "Your changes are saved." (5) Under its F195, ADR-0003's input bounds every read
    this app makes but an export and R7.3h's copy, R7.3c's checks and R5.4's open-file check ending before the file
    takes a write. (6) Under its F196, OQ 20's closer names an ADR's undo representation, so ADR-0003 need not wait for
    OQ 20. (7) Editorial and testability, no owner decision: DJ4's second-read line asserts R6.2a's deadline, the deferral
    by a read begun after the delete permitted and never required (the Corrected line under F57); DJ4's outside reads are
    declared as its first line's; DJ3's lines name Collection Mode R8.1f/g writes, the destination's volume for a move,
    an item outside the held write, a named copy read, a functional timeout, and a write committed before a rename kept
    under the new name; the copy header withholds a marked action or sentence whatever its state; the inbound
    Collection Mode line's Rows add R6.3 (the Collection Mode PRD's F187) and its fence range reads F52–F58; the status
    line's range reads F50–F54 and F56–F58. After the round-6 fix pass the body stands at ⟨count⟩ of 8,400 (F55), no
    rule trimmed. R1.11, R7.6q, E34 and E35 stay ready for alignment; R6.2a, R7.6b and E15 keep their alignment; OQ 20
    stays open.
  - **Why:** each is the Data Foundation half of a Collection Mode round-6 decision or fix about the file, a state or
    surface this PRD owns, its copy, an input it hands ADR-0003, or the seam.
  - **Rows:** R1.11, R6.2a, R7.6b, E15, E34, E35, OQ 20, DJ3, DJ4, the copy header, the Collection Mode inbound line, the
    Corrected F57 line.
  - DF's map line: "| F58 | R1.11, R6.2a, R7.6b, E15, E34, E35, OQ 20; DJ3, DJ4; the copy header; the Collection Mode
    inbound line in [Inherited obligations]; the Corrected F57 line. |". DF's inbound Collection Mode line "…inputs
    F52–F57 list" → "…inputs F52–F58 list" and its Rows add [R6.3] (IF6-1); the status line "…under F50–F54 and F56–F57
    (…)" → "…under F50–F54 and F56–F58 (…)" (word-neutral). Also carries N6-n2 (the copy header), IF6-N4's DJ3:42 label,
    ARCH6-N3's DJ3:44 and 6N1's DJ3:45.
- [ ] **SIB-2 — a Corrected line under DF F57** (DF fences, after F57's Rows line; 6MJ1): "**Corrected 2026-09-25 ([the
      Collection Mode PRD's F184], read as the deadline R6.2a states):** (1)'s "a read begun after the delete deferring it
      too" reads "a read begun after the delete permitted to defer it": R6.2a sets when the removed text must be gone at
      the latest and never requires it to stay, and once a checkpoint copies the delete past the earlier read no build
      can keep it, so DJ4's second-read line asserts the deadline, E35 up whenever the text is still in the file's bytes,
      and no longer that the text stays while the later read runs. F58 (2) supersedes (3)'s re-read clause. Peer review
      pending."
- [ ] **SIB-3 — Capture F76** (Capture fences; new, after F75):
  - "### F76 — Mirror Collection Mode's round-6 decision: the collection-side Flag entry is P0 (2026-09-25)"
  - **Authority:** [the Collection Mode PRD's F189] — owner decision D47, its round-6 (pre-lock) adjudication
    2026-09-25; peer review pending.
  - **Decision:** (1) Under the Collection Mode PRD's F189, that PRD's R4.9 — the collection-side "Flag" entry point on a
    captured item — moves to P0; R9.9 names the entry point as P0 there, the rest of R9.9 staying P1, and keeps its
    alignment; the Collection Mode obligation line says the entry point is P0. What a Flag does stays R5.6's, P0
    already, and R8.3 and R8.16, which the flagged item's re-scan and review use, are P0 already. (2) Editorial, no
    owner decision: the Collection Mode obligation line's "this document's states never hiding its entry points" reads
    "…the surface's entry points", as F43 states it. Traceability's range reads F1–F76.
  - **Carried by:** R9.9, Collection Mode obligation line, Traceability.
  - The map line: "- **F76** Collection Mode round-6 mirror — R9.9, Collection Mode obligation line, Traceability.";
    Traceability "Owner decisions F1–F75" → "F1–F76"; the status clause in the Checks.
- [ ] **SIB-4 — Import, a dated line under F68** (Import fences; or F69 on the orchestrator's ruling): "**Clarified
      2026-09-25 ([the Collection Mode PRD's F188], its round-6 fix pass, editorial):** the Collection Mode obligation
      line's cite reads "Collection Mode R1.3, R3.1 and R4.4" as one label, so R3.1 no longer reads as this document's;
      UJ 2.1's held-write lines name a Collection Mode R8.1f/g write, as Data Foundation R1.11 does. No rule changes; peer
      review pending." F68's map line already names the obligation line and UJ 2.1; unchanged. The status line is
      unchanged (its F68 clause covers the line).
- [ ] **SIB-5 — Export F34** (Export fences; new, after F33): "### F34 — R4.3's no-change clause and EJ1's timeout
      (2026-09-25)". **Decision:** "R4.3's read-back reads 'it reads back at SQLITE_READER_FLOOR that the export itself
      changed no value, mark or note', as R1.1's clause does since F33, so its test-build hold and EJ1's mid-export edit
      no longer contradict it; R4.3 keeps its alignment. EJ1's mid-export line reads the file's bytes within 5 s of the
      export's end, a functional timeout." **Why:** "the Collection Mode round-6 reviews found R4.3 still said
      'identical' beside a held export an edit changes, the contradiction F33 removed from R1.1, and EJ1's byte read
      without a tolerance. Source: [the Collection Mode PRD's F158] and its round-6 fix pass, editorial and testability;
      peer review pending." The map line "| F34 | R4.3; EJ1 |"; R4.3's Commit PR "(F33, F34)"; the status clause.
- [ ] **SIB-6 — the ADR-0003 row** — in F195-1 (with 6MJ1's wording). The 0005 and 0007 rows are unchanged.
- [ ] **SIB-7 — this PRD's fence map** (editorial). F20's Carried-by and map line add UJ7.1-t (R6-m3); F184's add R8.8
      (PRIV6-2, if taken); F102 gains 6N4's dated line, its Carried-by unchanged.

## Checks

- [ ] **Testability pairing (process rule 3).** Every row, state and metric changed above has its acceptance case and
      its seam line in the same pass: R4.9 → T14, UJ4.7-a–f, UJ9.1-f, UJ9.2-a, UJ9.8-b (F189-7 to F189-10; its seam R8.10b
      and the map's item-detail line, unchanged); R8.8 → UJ9.7-h (6MN1), UJ9.7-i (6MN3); E6 → UJ1.2-a, UJ4.7-c and every
      "E6 with no variant" case, first-phase base (unchanged cases, the body read by state and variant); the Legend and
      Build dependencies edits → no case (index). Sibling pairs: DF R6.2a → DJ4:66, :67, the new close line, :68, :69
      (F190-2, R6-m5, R6-M2, 6MJ1); DF R1.11 → DJ3's new re-read line (F191-3); DF R7.6b → DJ3's re-read line's state
      cell; DF E15, E34, E35 → DJ3 and DJ4 by state (wording only); DF OQ 20 → none (a closer); Capture R9.9 → Capture
      UJ4-f and this PRD's UJ4.7 cases (unchanged); Export R4.3 → EJ1 (SIB-5). New case: UJ7.1-t; new DJ3 and DJ4 lines.
- [ ] **Word counts** by rule 14's method. Collection Mode: start 11,951, end at or under 11,990 (≥10 left for the lock
      status line and the lock pass); each Result note that adds body words names its headroom or trim; report the final
      count, the trims taken and their meaning checks; flag any fix that cannot be paid for. Data Foundation: start
      8,381, budget 8,400 (F177); report the final count, and write it into F58 (7)'s ⟨count⟩; stop and report under the
      header's stop rule if it would pass 8,400. Export: start 3,783, budget 4,000. Report the companions' counts,
      unbudgeted.
- [ ] **Meaning check on every rewrite and trim.** Before an edit lands, read it against the fence Authority text — the
      owner's own words — not this list's drafts: R4.9's Pri and the Build dependencies rows (F189); the Legend's P0
      meaning (F189); E6's base body (F189, copy honesty against R8.3's first-phase refusals); Capture R9.9 and its
      obligation line (F189: "for that entry only"); DF R6.2a (F190); DF R1.11 and R7.6b (F191); the Build dependencies
      paragraph (F192); DF E15, E34 and E35 (F193, F194); the ADR-0003 row (F195, F175's F191 line, F184); Build
      dependencies row 6 and DF OQ 20 (F196); the post-lock items (F196, F197); R8.8 (F184); the Legend's priority bullet
      (F57, F187); Export R4.3 (F158). Each trim against the row or fence its candidate names. Name each in the Result.
- [ ] **Fences and Carried-by.** Fill the `_(filled by the round-6 fix pass)_` Carried-by lines and map lines of
      F189–F197 in the Carried-by grammar. As drafted:
  - F189 — R4.9, E6, E18, T14, UJ4.7-a, UJ4.7-b, UJ4.7-c, UJ4.7-d, UJ4.7-e, UJ9.1-f, UJ9.2-a, UJ9.8-b, the capture PRD
    R9.9, the capture PRD F76
  - F190 — the Data Foundation PRD R6.2a, the Data Foundation PRD DJ4, the Data Foundation PRD F58
  - F191 — the Data Foundation PRD R1.11, the Data Foundation PRD R7.6b, the Data Foundation PRD DJ3, the Data
    Foundation PRD DJ4, the Data Foundation PRD F58
  - F192 — governs no rows (it sets the Build dependencies paragraph, which carries no ID, as F124 does)
  - F193 — the Data Foundation PRD E15, the Data Foundation PRD E34, the Data Foundation PRD F58
  - F194 — the Data Foundation PRD E35, the Data Foundation PRD F58
  - F195 — the Data Foundation PRD F58 (the ADR-0003 row and the Harness carry no ID)
  - F196 — the Data Foundation PRD F58 (Build dependencies row 6 and OQ 20 carry no ID the grammar parses)
  - F197 — governs no rows (post-lock items only)

  Also: F20 adds UJ7.1-t; F184 adds R8.8 if PRIV6-2 is taken (SIB-7). No placeholder is left. Check 2 then reads 197
  fences, 197 map lines, the map equal to the Carried-by lines; check 8 resolves the siblings' new DF F58, Capture F76
  and Export F34.
- [ ] **Sibling status lines, each in its sibling's style, peer review pending.**
  - DF, its compact fence-citing form: "…under F50–F54 and F56–F57 (…)" → "…under F50–F54 and F56–F58 (…)"
    (word-neutral).
  - Capture: "; Collection Mode round-6 amendment 2026-09-25 under F76 (R9.9 amended with alignment kept; peer review
    pending)".
  - Export: "; Collection Mode round-6 mirror 2026-09-25 under F34 (R4.3 amended with alignment kept; peer review
    pending)" (+15, in the Export ledger).
  - Import: unchanged under SIB-4's dated line; "; Collection Mode round-6 mirror under F69 dated 2026-09-25 (labels;
    peer review pending)" if F69 is ruled in.

  This PRD's own status line stays "Status: draft".
- [ ] **Label check** (check 13, both directions, and the capture PRD's standing label check for its amendment). Re-run
      after the case edits: the new or moved quoted strings — "Select all" and "Set a field" (UJ9.5-c), "All items" (UJ7.1-t
      and UJ9.5-d), "Export collection" (UJ9.5-d) and "Grid" (UJ10.1-d) — are copy labels; sibling actions stay named by document and state, unquoted ("the capture PRD's Flag
      action", "the Data Foundation PRD's E35 action", "the capture PRD's re-scan entry", Import's "Collection Mode's
      Rename collection"); E6's new "flagging a swatch" and DF E15's, E34's and E35's new copy carry no action label.
      Direction 2 prints nothing; direction 1 lists only the disposed sites.
- [ ] **Traceability** "F1–F188" → "F1–F197" in this PRD (word-neutral); the capture PRD's "F1–F76". New case IDs take
      each journey's next free letter (UJ7.1-t); nothing is renumbered.
- [ ] **Seam check (the Inherited-obligations standing check; the lock run's check 1a).** Each seam's two summaries state
      the same set: this PRD's Outbound Data Foundation row ("as its F52–F58 record") against DF's inbound Collection Mode
      line ("F52–F58 list", Rows now naming R6.3 — IF6-1); this PRD's inbound Capture R9.9 line ("at P0") against the
      capture PRD's obligation line ("at P0") and R9.9 ("P0 entry point") — check 1b's pair for F189; this PRD's
      Outbound Capture Mode row (R8.14 and R9.9's entry at P0) against Capture R8.14 (P0, F75) and R9.9 (F76); the inbound
      Import R2.3 line against Import's line (one label now); the inbound Data Export line and the Outbound Data Export
      row against Export's two lines (unchanged); the inbound Capture R6.9 line against DF's outbound line (unchanged),
      with PLAN-2's comparator clause disposed under check 1b as a restatement of R1.1 and R3.2. Report each pair.
- [ ] **ADR queue and post-lock.** `docs/decisions/README.md`'s 0003 row is edited as F195-1 states (still eight
      inputs); the 0005 and 0007 rows are unchanged. `docs/product/post-lock.md` gains F196-3's release item, F197-1 to
      F197-3, and the PLAN follow-ons if ruled into this pass, each in its style and attributed to this PR; the new
      sections ("## v1 release", "## Dogfood", "### Collection Mode" under "## Next pass over a locked PRD") are placed as
      ruled. The round-5 items stay unticked ("number pending"), and the item "**DF** — whether a Collection Mode rename
      moves an imported column's stored name" (line 99) is still **NOT ticked**: it is ticked when the PR exists, at the
      bookkeeping close.
- [ ] **Constants and OQs.** No constant, candidate or interim changes in this PRD. The Interim-stated list is
      unchanged and the no-interim list stays None, derived from the Open questions table; T-a changes OQ 10's Decision
      cell only. DF OQ 20's Closer changes (F196-2); its status stays open.
- [ ] **Lock-time items — deferred to the lock pass, not this one** (carried from round 5). The status line (`Status:
      draft` → `Status: locked (<date>)`, about +2 body words); deleting the guidance comments and the conditional
      template comment (checks 15 and 16); rewriting "peer review pending" on the sibling status surfaces this change
      touches (Capture PRD:3 and its fences preamble, DF PRD:3, the device PRD:3 and its fences preamble, Export PRD:3,
      Import PRD:3) at the bookkeeping close; the product README's "queued" (IF-16); the PR number in post-lock.md's
      "number pending" items (check 14); the fence preamble's first-lock "Resolved baselines" sentence; the
      Open-questions table's statuses at lock; and the full mechanical-check roster re-run against the state that locks.
      None is done here.

## Out of scope for this pass

- The lock-time items the last box lists — at lock or the bookkeeping close.
- A Data Foundation budget beyond 8,400, or a split, if its body would pass it — the owner's, reported under the stop
  rule.
- Any change a fence above does not authorize. Marked **needs owner**, nothing made: PLAN-1's device-OQ-21 interim for
  the macOS CI lane; PLAN-10's swatch-size default and range; ARCH6-3's help-docs line on moving the file; ARCH6-N2's
  reading of the wipe against R1.11's deferral; the privacy review's help-docs line on the app's own long reads.
  Reported, not made: E35's copy while only this app's read holds the wipe (F173's lifetime stands); the cold All items
  open, a ceiling delete's free space and the unbudgeted open-file check at FILE_ITEMS_CEILING (OQ 1's to measure);
  WAL growth under a long outside read; the engineering review's question 11 (ADR-0003's wipe-pending marker); the
  capture PRD's E2 empty-variant guidance (P5-n5); and whichever of the orchestrator's rulings above are declined.
