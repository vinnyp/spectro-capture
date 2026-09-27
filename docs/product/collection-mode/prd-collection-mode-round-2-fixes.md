# Collection Mode PRD — round 2 fixes (review round 2, 2026-09-25)

The resume point for the fix pass over the findings of review round 2, before round 3. The fence file
`prd-collection-mode-fences.md` (F1–F98) is the written authorization for every change below: a **fix** names the fence
that authorizes it, or reads "editorial/testability — no new WHAT" where it only adds a case, a readback grant, a
fixture, a cite or a wording fix that makes copy match an existing row. A change no fence names is not made: it is
reported to the orchestrator as an unratified WHAT. Every **needs owner** box waits for the owner's answer and its own
dated fence (F99 on). Each box is ticked, with a Result note, as its fix lands.

**Word budget.** The PRD body's budget is 12,000 words by rule 14's method, and it stands at 11,985 (the round-1b
count), 15 words of headroom. Every word a fix adds to the body is paid for in the same pass by trimming rule-free
prose, and the Result note names the trim. A fix that cannot be paid for is not squeezed in: it is flagged to the
orchestrator. The companions (journeys, copy, fences) sit outside the budget, so a fix that can land in a companion
lands there.

The findings are the eight round-2 delta reviews in `docs/agent-reviews/2026-09-24-prd-collection-mode-peer-reviews.md`
(Round 2 › Per-lens reviews), in log order; that log's Round 2 verify-the-reviewer table ("log row n" below) records
the accepted Blockers and Majors, and its five "owner decision" rows (4, 6, 7, 8, 9) are Owner-needed items 1–5. Each
box carries the reviewer's own ID and severity. A round-1 finding that a review's delta table marks PARTIAL or
UNRESOLVED gets its own box, labelled "carried: <round-1 ID>"; B1 and A12 are carried and re-graded, one box each. One
box carries each fix, the first in log order, and every other box raising the same defect says **covered by** it. An
item a review raised only in its Missing or Deferred list, carried by no rated finding, gets a box marked *unrated*.
Sibling IDs name their document as in round 1 ("DF", "Capture", "Import", "Device"); a bare ID is this PRD's own.

Dispositions: **fix**, **covered by**, and **needs owner** — each needs-owner box names its item in the Owner-needed
list below, where the five items the orchestrator is putting to the owner separately are marked *(asked separately)*.

## Status bookkeeping

- [x] **Flip to aligned** (the log's round-2 flip record, less the holds below) — 35 rows and states, in the PRD's
      tables and the copy file's Status lines: E1, E2, E7, E14, E15, E16, E18, E19, M3, R1.1, R1.2, R1.5, R1.6, R1.7,
      R1.8, R1.9, R2.1, R2.2, R2.6, R2.7, R2.9, R2.10, R3.1, R3.2, R3.4, R3.6, R4.4, R4.6, R5.1, R5.3, R5.6, R5.8, R6.1,
      R8.4, R8.7. No fix box changes their meaning; R2.10 only quotes a label (SSE *unrated*), wording only. R5.3
      (Owner-needed 13) and E19 (35) land at pre-alignment instead if those answers change them.
      Result: 33 flipped in the PRD tables and the copy file's Status lines — E1, E2, E7, E14, E15, E16, E18, M3, R1.1, R1.2, R1.5, R1.6, R1.7, R1.8, R1.9, R2.1, R2.2, R2.6, R2.7, R2.9, R2.10 (its label now quoted, wording only), R3.1, R3.2, R3.4, R3.6, R4.4, R4.6, R5.1, R5.6, R5.8, R6.1, R8.4, R8.7; R5.3 (F111 lists its "restore" variant) and E19 (F133) changed and land at pre-alignment.
- [x] **Sub-rows hold with their lead — flag to the orchestrator.** The record also lists R2.4, R2.4a, R2.4c–i, R4.2a,
      R4.2d–g, R5.2, R5.2a and R5.2c–e (19), but a lettered sub-row carries its lead's status and flips only with its
      lead and every sibling: R2.4b (2MN2), R4.2 itself with R4.2b, c and h, and R5.2b (N-n3) are objected, so all
      three families hold.
      Result: flagged in the pass report; the R2.4 and R4.2 families were rewritten this pass (2MN2, F114) and sit at pre-alignment with their leads; the R5.2 family holds at needs-discussion.
- [x] **Objected rows this pass rewrites → pre-alignment:** R1.3 (PRIV-11); R2.4 and R2.4a–i (2MN2 on R2.4b); R2.5
      (2MN2); R3.9 and R6.4 (R2-IF-6); R4.7 (PRIV-14); R4.9 (PM2-10); R7.1 and R7.2 (2MN10); R8.1 and R8.1a–f (PERF2-7,
      PERF2-9); R8.2 (2MJ1, PERF2-9); R8.10 and R8.10a–f (2MN16, R2-m9, R2-m13, PRIV-12, PERF2-4, PERF2-6); E3 and E13
      (N-m4); E8 (PM2-1); E9 (N-m3); E10 (2N2); E12 (N-B1, R2-IF-3, N-n1, N-n2); M1 (PERF2-6, PERF2-9).
      Result: every listed row, family, state and metric reads pre-alignment; new sub-row R8.1g carries R8.1's.
- [x] **Objected rows this pass leaves → needs-discussion** (answered in a companion only, or waiting on the owner):
      R1.4, R1.10, R2.3, R2.8, R2.11, R3.3, R3.5, R3.7, R3.8, R4.1, R4.2 and R4.2a–h, R4.3, R4.5, R4.8, R5.2 and
      R5.2a–e, R5.4, R5.5, R5.7, R6.2, R6.3, R8.3, R8.5, R8.6, R8.8, R8.9, R8.11; E4, E5, E6, E11, E17; M2, M4. A row an
      owner answer rewrites in this pass lands at pre-alignment instead.
      Result: owner answers rewrote R1.4, R1.10, R2.11, R3.3, R3.5, R3.7, R3.8, R4.1, R4.2 and R4.2a–h, R4.8, R5.4, R5.5, R6.2, R6.3, R8.3, R8.6, R8.8, E4, E6, E17 and M4, and a trim touched R4.5, so those land at pre-alignment; R2.3, R2.8, R4.3, R5.2 and R5.2a–e, R5.7, R8.5, R8.9, R8.11, E5, E11 and M2 read needs-discussion.
- [x] **After the pass:** report the final lists — the record's 54 flip-eligible IDs and the 67 objected reconcile to
      the PRD's 121; new rows, states, variants or metrics an owner answer adds enter at pre-alignment.
      Result: the 121 reconcile — 33 aligned; 72 pre-alignment (43 rows, states and metrics plus the 29 sub-rows of R2.4, R4.2, R8.1 and R8.10); 16 needs-discussion (11 plus R5.2a–e). New at pre-alignment: R8.1g, and the variants E4 "all items", E6 "full" and "elsewhere", E17 "restore".

## peer-product-manager-reviewer (Claude route)

- [x] **PM2-1 (Major) — E8 promises an undo R4.7 does not give** (E8 body and "clear" variant; UJ6.4-f). **fix (log
      row 2; F29 — copy honesty to R4.7):** both endings say "Undo change" brings back what was there until a change of
      another kind is made, worded to Owner-needed 6; a case asserts E8's words and UJ6.4-f agree.
      Result: E8's body and "clear" variant say Undo change puts it back until the file closes, the file is read again, or anything but swatch details, codes or names changes, scanning not counting (F104's set); UJ6.4-f, UJ6.4-l and UJ6.4-m assert that set.
- [x] **PM2-2 (Minor) — the actions ending the undo history are open-ended, and what an undo acts on is invisible**
      (R4.7, E3, E14, UJ6.4-f, UJ6.4-i). **needs owner (6).**
      Result: F104 landed in R4.7 — any other committed write, column hide or show and "New collection" included, or a re-read ends the history, never a capture save or a session starting; a refused undo keeps its entry; offered only where its latest change's collection is shown; UJ6.4-i, -l, -m, -n; dated line under F29. R4.7's existing re-read ender is kept, F104 being silent on it (reported).
- [x] **PM2-3 (Minor) — R8.8 renders a permission-lost state DF has not written, with no Stop or interim** (R8.8, Build
      dependencies row 1, UJ9.7). **needs owner (3 — C, asked separately).**
      Result: F101 landed — the Data Foundation PRD gains E34 (its R1.10, R7.3j, R7.6p, DJ3 and F53); R8.8 renders it; R8.10a declares lost permission; UJ9.7-g; Build dependencies row 1 lists DF E34; the post-lock item is ticked, attributed to this change's pending PR.
- [x] **PM2-4 (Minor) — displayed precision covers only L*, C*, h°, Spread and ΔE2000** (R2.11, R4.2c, R5.2e,
      UJ4.1-a). **needs owner (7).**
      Result: F105 landed — R2.11 adds a*, b*, u*, v* at one decimal, X, Y, Z at two on 0–100, sRGB as 0–255 integers, HSL as whole degrees and percents; UJ4.1-a asserts them; dated line under F47.
- [x] **PM2-5 (Minor) — M4 passes vacuously on a dogfood session with no re-scans** (M4). **needs owner (8).**
      Result: F106 landed — M4's population is sessions ending with a re-scan awaiting an answer, the start count recorded, a session with none reading not measured; dated line under F51.
- [x] **PM2-6 (Minor) — closing a detail opened from Find similar loses E9's list** (R3.7, R4.1, UJ3.4-e). **needs
      owner (9).**
      Result: F107 landed — R4.1 returns to E9 with its list when the detail was opened from E9; UJ3.4-e opens two results in turn; dated line under F46.
- [x] **PM2-7 (Minor) — R4.8's duplicate check compares against names the user never sees** (R4.8, E11, the copy
      file's Column headers, UJ4.6-c). **needs owner (10).**
      Result: F108 landed — R4.8's duplicate set adds the copy file's Column headers; E11 unchanged; UJ4.6-j.
- [x] **PM2-8 (Minor) — whether a collection name matches from its start or anywhere is unstated, and E4 omits it**
      (R1.10, E4, UJ7.1-j). **needs owner (11).**
      Result: F109 landed — R1.10 matches anywhere in a collection's name; R3.5 enumerates E4's new "all items" variant, which names collection names; UJ7.1-j types set, UJ7.1-m renders the variant; dated line under F17.
- [x] **PM2-9 (Minor) — E5 blames the filters when the search alone lists nothing** (R3.5, E4, E5, UJ3.2-d). **needs
      owner (12).**
      Result: F110 landed — R3.5 renders E4 whenever the search lists nothing, E5 only when the filters hide everything it lists; UJ3.2-d rewritten, UJ3.2-e added; dated line under F18.
- [x] **PM2-10 (Nit) — R4.9 withholds Flag when the "current reading" awaits the answer; the Vocabulary marks its
      predecessor** (R4.9). **fix (F78 — editorial):** "on a captured item carrying no re-scan-unanswered mark (R2.4i)".
      Result: R4.9 keys the Flag on the re-scan-unanswered mark (R2.4i) in both clauses.
- [x] **PM2-11 (Nit) — E17's "Use this reading" sentence renders at P0, unmarked, unlike E18's under F97** (E17, R5.3).
      **needs owner (13).**
      Result: F111 landed — E17's P0 body drops the Use-this-reading sentence, now a [phase: variant-absent] "restore" variant R5.3 lists; UJ5.3-n; E6 likewise under R2-IF-8; dated line under F97.
- [x] **PM2-12 (Nit) — the Build contract says a re-import moves an item; it adds a new pending item and the readings
      stay** (Build contract). **fix (F61 — editorial; the non-goal stands):** "…which a re-import approximates, as a new
      pending item (F61)".
      Result: the Build contract says re-importing approximates moving an item; word-neutral.
- [x] **PM2-15 (Nit) — no row says an open item detail or history view shows a change to its item** (R3.9, R4.1,
      R8.1c). **needs owner (14).**
      Result: F112 landed in R8.1c, whose single-item writes now reach the item detail and version history view while shown within BROWSE_RESPONSE_BUDGET; R4.1 was left unchanged for the budget, R8.1c carrying the rule; UJ9.1-l.
- [x] **carried: PM Minor 5 (PARTIAL) — no displayed precision for R4.2c's six spaces** (R2.11, R4.2c). **covered by
      PM2-4.**
      Result: covered by PM2-4.

## peer-staff-software-engineer-reviewer (Claude route)

- [x] **2MJ1 (Major) — R8.2's file-wide budgets state no imported-column width** (R8.2, UJ9.5-b, UJ7.1). The width:
      **needs owner (5 — E, asked separately)**; **fix (F87, F88):** R8.2 carries R8.1d's no-lazy-tail clause; **fix
      (F86 — testability):** a UJ7.1 case searches an imported value from the All items view.
      Result: F103 landed — R8.2 holds at IMPORTED_COLUMNS_CEILING in every collection across FILE_ITEMS_CEILING, its first rows "as R8.1d does", which carries the no-lazy-tail clause; UJ9.5-b declares the width; UJ7.1-m searches an imported value from All items; the ADR-0003 row gains the index-or-scan input; dated F86 line already present.
- [x] **2MJ2 (Major) — the storage constraints the rows imply never reach ADR-0003 or ADR-0005** (R4.3, R6.2, R8.1f,
      R8.11; the ADR-0003 row; DF F52 (7)). (i) when removed text must be gone: **needs owner (4 — D, asked
      separately)**; (ii) an atomic bulk write against in-flight capture saves: **needs owner (2 — B, asked separately).**
      Result: (i) F102 landed — the Vocabulary's "the file's bytes" takes in any journal, log, index or file beside it and R8.8 holds removed text gone from the moment its write lands, open or after a crash; the Data Foundation PRD's R2.3 and R6.2a follow (its F53) and so does the ADR-0003 row. (ii) F100 landed in R8.3 — no bulk writes while any session is in flight; no ADR-0005 input, F100 naming none.
- [x] **2MN1 (Minor) — a collection name's match mode, E4 in All items, and no All-items imported-value case** (R1.10,
      E4). **covered by PM2-8** (match mode, E4) **and 2MJ1** (the case).
      Result: covered by PM2-8 and 2MJ1.
- [x] **2MN2 (Minor; pre-existing) — "the current reading's gamut-clipped flag" doesn't say which derived set's**
      (R2.4b, R2.5). **fix (F6, F40 — editorial):** "the working-set value's flag", the only one F40's sRGB-display
      equality can mean.
      Result: R2.4b reads the flag of the current reading's working-set value; R2.5 cites R2.4b's flag.
- [x] **2MN3 (Minor) — R3.3's like set inside one collection, for a mismatched non-spectral reading, and under search**
      (R3.3, E3, UJ3.3-k). **needs owner (15).**
      Result: F113 landed — R3.3's like pair is the collection's own in its table and, in All items, the most common among listed items with a value (ties per F94), a mismatched non-spectral reading never like; UJ3.3-m, UJ7.1-n; dated lines under F54 and F68.
- [x] **2MN4 (Minor) — Find similar on an item whose working-set value is absent (ZX-006); persisted non-working sets;
      what R4.2c shows then** (R3.7, R3.8, R4.2c). **needs owner (16).**
      Result: F114 landed — R3.7 compares working-set values only, R3.8 withholds Find similar without one, R4.2c shows value-absent; E9's excluded sentence follows; UJ3.4-g, UJ3.4-h; dated line under F16.
- [x] **2MN5 (Minor) — "any other committed action" leaves capture saves, column hide/show and "New collection"
      unclassed** (R4.7). **covered by PM2-2.**
      Result: covered by PM2-2.
- [x] **2MN6 (Minor) — the not-compared line gives a false reason on an item with no current value** (R5.4, the copy
      file's History lines). **needs owner (17).**
      Result: F115 landed — R5.4 shows distances only on an item with a current value; UJ5.3-l.
- [x] **2MN7 (Minor; pre-existing) — "Use this reading" on a never-true reading is neither offered nor excluded**
      (R5.5). **needs owner (18).**
      Result: F116 landed — R5.5 names a never-true reading as offered, its mark kept; UJ5.3-m.
- [x] **2MN8 (Minor; pre-existing) — nothing commits or cancels "Set a field" at or below BULK_CONFIRM_COUNT** (R6.2).
      **needs owner (19).**
      Result: F117 landed — in R6.2 Return applies (through E8 above BULK_CONFIRM_COUNT) and Escape or E8's Cancel changes nothing; no new label; UJ6.2-k.
- [x] **2MN9 (Minor) — E8's undo window over-promises** (E8). **covered by PM2-1.**
      Result: covered by PM2-1.
- [x] **2MN10 (Minor) — the Grid/Table choice and swatch size: per collection or per file** (R7.1, R7.2). **fix (F71 —
      "as R3.6 does"):** both per collection; a UJ10.1 case shows Gouache Set as the table while Studio Markers is a grid.
      Result: R7.1 and R7.2 keep the Grid or Table choice and the swatch size per collection; UJ10.1-f.
- [x] **2MN11 (Minor) — "cold" and the paging input and rate are undefined** (R8.1d, R8.1e, M1, OQ 1, the Timing
      workload). **fix (F66 — testability):** the Timing workload declares paging as a fixed count of Page Down presses
      at a fixed cadence; what "cold" means: **needs owner (21).**
      Result: the Timing workload declares paging as Page Down every 100 ms, and cold as F119 defines it (the OS file cache purged, the Data Foundation PRD's open-file check finished), OQ 1's interim citing it; dated line under F41.
- [x] **2MN12 (Minor) — deleting, and undoing the delete of, a ROWS_CEILING collection falls in no R8.1 class** (R8.1c,
      R8.1f). **needs owner (1 — A, asked separately).**
      Result: F99 landed — DELETE_WRITE_BUDGET (10 s, under OQ 1) joins the constants table, and new R8.1g holds "Delete selected", "Delete collection" and E10's undo of either to it, BULK_WRITE_BUDGET keeping set, clear, reorder and their undo in R8.1f; UJ9.5-g times both deletes at ROWS_CEILING.
- [x] **2MN13 (Minor) — R8.1b's history-open budget has no bound on readings per item** (R8.1b, R8.2, UJ9.5-e).
      **needs owner (20).**
      Result: F118 landed — HISTORY_READINGS_CEILING (500, under OQ 1) joins R8.1's lead and the constants table; UJ9.5-e is timed; dated line under F43.
- [x] **2MN14 (Minor) — R8.8's permission-lost branch has no interim, against the Legend's "None"** (R8.8). **covered by
      PM2-3.**
      Result: covered by PM2-3.
- [x] **2MN15 (Minor) — R8.11 rests on Capture's TBD ROW_CONFIRM_BUDGET with no interim; the Legend's "None" fails**
      (R8.11, Legend, UJ9.5-d; Capture OQ 5). **needs owner (22).**
      Result: F120 landed — OQ 1's interim runs R8.11 against the engineering plan's declared ROW_CONFIRM_BUDGET while the capture PRD's OQ 5 is open, and R8.11 joins OQ 1's Interim-stated line; dated line under F89.
- [x] **2MN16 (Minor) — no readback gives a mark's shape or a cause's identifier** (R8.10b, R8.10c, UJ2.1-k, UJ4.1-c,
      UJ4.7-a, UJ9.6-a). **fix (log row 12; F38, F96 — testability):** R8.10c reads each mark's symbol apart from its tint,
      R8.10b each R4.2/R5.2 line and a cause by Capture's table key; UJ9.6-a: 11 distinct symbols; UJ4.1-c, UJ4.7-a by key.
      Result: R8.10c reads each mark's symbol apart from its tint; R8.10b reads each R4.2 and R5.2 line's content and a set-aside cause by the capture PRD's cause key; UJ4.1-c and UJ4.7-a read the key; UJ9.6-a asserts the eleven symbols distinct, tint aside.
- [x] **2MN17 (Minor) — precision and encoding of R4.2c's a*, b*, XYZ, Luv, sRGB and HSL** (R4.2c). **covered by
      PM2-4.**
      Result: covered by PM2-4.
- [x] **2MN18 (Minor) — DF E11 and E26 still say "marked as not yet settled"** (R4.2h, R5.7; DF E11, DF E26). **needs
      owner (23).**
      Result: F121 landed — the Data Foundation PRD's E11 and E26 say "marked as awaiting your answer", alignment kept, under its F53.
- [x] **2MN19 (Minor) — Capture R5.8 and R6.6 quote "flagged: missing or damaged" against the cause table's "flagged as
      missing or damaged"** (R4.2b; Capture R5.8, Capture R6.6). **needs owner (24).**
      Result: F122 landed — Capture R5.8 and R6.6 quote "flagged as missing or damaged", alignment kept, in a dated line under Capture F71.
- [x] **2N1 (Nit) — Row transitions have no route for an item a re-read finds gone** (Row transitions, Vocabulary; R4.1,
      R8.5). **fix (F74 — editorial; the table carries every route a row causes):** a route from present to a new (state)
      term for an item gone from the file by a change outside the app, "deleted" keeping its byte guarantee; T16.
      Result: a new (state) term, removed, and a Row transitions route present → removed for an item a re-read finds gone, deleted keeping its byte guarantee; T16.
- [x] **2N2 (Nit) — E10 is indexed to the item detail, where it never renders, and not to the All items view** (E10's
      index row; the Surfaces preamble and rows). **fix (F74 — editorial):** E10 indexed to the collection surface and the
      All items view, the preamble and both Surfaces rows following.
      Result: E10 is indexed to the collection surface and the All items view in the copy index, the Surfaces preamble and both Surfaces rows.
- [x] **2N3 (Nit) — an All-items Find similar tie on both distance and code has no final key** (R3.7). **needs owner
      (25).**
      Result: F123 landed — R3.7's ties go by Swatch Code, then collection-list order; UJ3.4-i; dated line under F46.
- [x] **2N4 (Nit) — E9's ⟨distance⟩ could render 3.0 or 3.00** (the Placeholders rule, E9). **fix (F47 — editorial):**
      ⟨distance⟩ renders to two places, as R2.11 shows ΔE2000.
      Result: the Placeholders rule renders ⟨distance⟩ to two places.
- [x] **2N5 (Nit) — "well inside sRGB" is unquantified (BI-1 at +0.016), and two fixtures are named Daylight** (Harness
      gamut margins; UJ3.4-d, UJ7.1-i, UJ7.1-l). **fix (testability):** the Harness names the fixtures inside sRGB by
      less than 0.03 whose marks no case reads; UJ7.1-i's collection is renamed.
      Result: the Harness names DL-001 and BI-1 (inside sRGB by 0.016, marks never read) and ZX-019 (F127) as its exceptions; UJ7.1-i's collection is renamed Soft Light.
- [x] **2N6 (Nit) — "Stop: ADR-0006" sits on Build dependencies row 1 only** (Build dependencies). **needs owner (26).**
      Result: F124 landed — the Build dependencies paragraph makes ADR-0006 a stop for every row beside ADR-0003, and row 1's ADR-0006 item goes.
- [x] **carried: SSE MJ2 (PARTIAL) — DF E11 and E26 keep "not yet settled"** (DF E11, DF E26). **covered by 2MN18.**
      Result: covered by 2MN18.
- [x] **carried: SSE MJ8 (PARTIAL) — envelope bounds: file-wide width, collection delete, history length** (R8.1, R8.2).
      **covered by 2MJ1, 2MN12 and 2MN13.**
      Result: covered by 2MJ1, 2MN12 and 2MN13.
- [x] **carried: SSE MN9 (PARTIAL) — the permission-lost state has no interim** (R8.8). **covered by PM2-3.**
      Result: covered by PM2-3.
- [x] **carried: SSE MN17 (PARTIAL) — displayed precision of R4.2c's six spaces** (R4.2c). **covered by PM2-4.**
      Result: covered by PM2-4.
- [x] *unrated* **SSE Deferred (to interface) — hiding or showing a column has no copy label** (R2.10, E3, the map).
      **fix (editorial — the Labels rule):** the copy file gains the label, on E3 [phase: action-absent], quoted by R2.10.
      Result: "Columns" joins E3's Actions [phase: action-absent]; R2.10 quotes it; UJ2.2-a, UJ6.4-l and the map use it.
- [x] *unrated* **SSE Deferred (to test) — "That E8" back-references** (UJ4.4-d, UJ7.1-g). **fix (editorial — the
      cross-document cite rule, as round 1's SSE MN12):** "the Data Foundation PRD's E8".
      Result: UJ4.4-d and UJ7.1-g cite the Data Foundation PRD's E8, and UJ6.3-b, -d and -e its E33, in place of "That".

## peer-test-reviewer (Claude route)

- [x] **B1 (Blocker; carried: TEST B1, PARTIAL) — no case asserts the display triplet the chip sends** (R2.3, R8.10c,
      UJ2.1-c, M2). **fix (log row 3; F40 — testability):** UJ2.1-c asserts ZX-002's P3 triplet (64.7, 196.8, 103.6)/255
      ±1/255 by OQ 7's interim; a case reads ZX-013's chip as T3's value, T1 and T2 declared at other values.
      Result: UJ2.1-c asserts ZX-002's unclipped Display P3 triplet (64.7, 196.8, 103.6)/255 ±1/255 by OQ 7's interim, recomputed; UJ2.1-p reads ZX-013's chip as T3's value, the Harness declaring T1 and T2 at other values.
- [x] **R2-M1 (Major) — byte checks miss removed text case-folded, tokenised or re-encoded** (Harness, R8.10d; T1, T4,
      T6, UJ1.3-e/f, UJ4.2-a/c, UJ6.2-c, UJ6.3-b, UJ6.4-j/k). **fix (log row 10; F35 — testability):** "holds T nowhere"
      covers T's unique words, any case, normal form, UTF-8/16, every table at SQLITE_READER_FLOOR; a control; sidecars per D.
      Result: the Harness's byte check covers the text and each of its unique words in any case and the import PRD's R2.3 normal form, as UTF-8 and UTF-16, in the file and every file beside it and every table at SQLITE_READER_FLOOR, at F102's moments; UJ9.8-c is the control.
- [x] **R2-M2 (Major) — four of R3.9's seven triggers, its "moves it" clause and five of R6.4's deselects have no case**
      (R3.9, R6.4, UJ9.1). **fix (log row 11; F44 — testability):** a case each for a restore, a Flag, an undo and a
      display move under their filters, a capture save on ZX-010 (L* declared) under an L* sort, and a search change.
      Result: UJ9.1-e (a restore under the unreadable filter), UJ9.1-f (a Flag), UJ9.1-g (an undo), UJ9.1-h (a display move under cannot-show), UJ9.1-i (a capture save on ZX-010 at a declared L* 50 under an L* sort) and UJ9.1-j (a search change).
- [x] **R2-M3 (Major) — a mark's shape can't be read** (R8.9, R8.10c, UJ2.1-k, UJ9.6-a, the Mark labels table, the map).
      **covered by 2MN16.**
      Result: covered by 2MN16.
- [x] **R2-m1 (Minor) — UJ4.4-f can't tell "first pending" from "next pending"** (UJ4.4-f; Capture R3.7). **fix (F55 —
      testability):** a pending ZX-000 declared at queue position 1; assert the session resumes at ZX-000.
      Result: UJ4.4-f declares pending ZX-000 at queue position 1 and asserts the session resumes there, not at ZX-020.
- [x] **R2-m2 (Minor) — no case asserts a re-read deselects an item it removes** (R8.5, R6.4, UJ9.3-a). **fix
      (testability):** a run removing the selected ZX-011 outside the app, nothing selected afterwards.
      Result: UJ9.3-c removes the selected ZX-011 outside the app; nothing is selected after the re-read.
- [x] **R2-m3 (Minor) — UJ9.7-d's restore crash moment is unpinned** (R8.8, UJ9.7-d). **fix (testability):** crash
      after the new reading is written and before its six derived sets are (DF R7.4).
      Result: UJ9.7-d crashes after the new reading is written and before its six derived sets are.
- [x] **R2-m4 (Minor) — the keyboard re-runs still skip most states** (R8.9, UJ9.6-b). **fix (F75 — testability):**
      UJ9.6-b fires every action of every state this PRD owns in the phase, the All items view and range and toggle
      selection included.
      Result: UJ9.6-b fires every action of every state this PRD owns in the phase, on all four surfaces, range and toggle selection included.
- [x] **R2-m5 (Minor) — R8.3's "edits stay available" runs in one undeclared sub-state** (R8.3, UJ9.1-b). **fix (F14,
      F30 — testability):** UJ9.1-b runs in each in-flight state.
      Result: UJ9.1-b runs in each in-flight state.
- [x] **R2-m6 (Minor) — sorting and comparing on stored values is never told apart from the displayed ones** (R2.11,
      UJ3.3, UJ3.4-f). **fix (F47 — testability):** a Greys item at L* 48.4965 (ΔE2000 3.0035, shown 3.00) is not
      listed; an L* pair 50.04 and 50.01 in reversed code order sorts by stored value.
      Result: UJ3.4-f adds GB-3 at ΔE2000 3.0035, unlisted though it would display 3.00; UJ3.3-l sorts CP-2 (50.01) before CP-1 (50.04), both showing 50.0.
- [x] **R2-m7 (Minor) — F83's spread and verdict carry isn't told apart from the snapshot** (R5.5, UJ5.3-k). **fix (F83
      — testability):** B2 declared spread 0.30, agreed; B1 disagreed, average accepted, spread 1.20; samples-disagreed
      and 1.20 asserted after the restore.
      Result: UJ5.3-k declares B2 agreed at 0.30 and B1 disagreed, average accepted, at 1.20, and asserts samples-disagreed and 1.20 after the restore.
- [x] **R2-m8 (Minor) — the above-ceiling case never tests value length** (R8.1, UJ9.5-c). **fix (F43 — testability):**
      values of 201 and 10,000 characters, read back unchanged.
      Result: UJ9.5-c declares values of 201 and 10,000 characters and reads both back unchanged.
- [x] **R2-m9 (Minor) — lost file permission has no injection input and no case** (R8.8, R8.10a, the map). **fix (F73 —
      testability):** "file permission revoked" joins R8.10a and the map's file line; a case asserts the file unchanged,
      the state rendered per Owner-needed 3 (C).
      Result: R8.10a declares the app's permission to the file, the map's file line lists it, and UJ9.7-g asserts E34 with the file unchanged.
- [x] **R2-m10 (Minor) — R8.11 has no P0 case, and bulk delete and "Use as scan order" are untimed** (R8.11, R8.1f,
      M1, UJ9.5-a, UJ9.5-d). **fix (F42, F89 — testability):** a P0 UJ9.5-d variant (searches, sorts, single edits over
      20 saves); UJ9.5-a times both at ROWS_CEILING, the delete's oracle per Owner-needed 1 (A).
      Result: UJ9.5-d is now P0 — a search keystroke, a header fire and a single edit at each of 20 sets' last sample — F100 refusing bulk writes in flight; UJ9.5-g times a selection delete, a collection delete and "Use as scan order" at ROWS_CEILING against DELETE_WRITE_BUDGET and BULK_WRITE_BUDGET.
- [x] **R2-m11 (Minor) — cases read a set-aside cause by its label, and detail lines have no granted readback** (R4.2,
      R4.2b, R8.10b, UJ4.1-c, UJ4.7-a). **covered by 2MN16.**
      Result: covered by 2MN16.
- [x] **R2-m12 (Minor) — the gamut-margin rule waits on DF R7.5's unnamed reference** (Harness gamut margins, M2; DF
      R7.5). **fix (F40 — testability):** margins checked by a checked-in computation (CIE 15, Bradford, published
      primaries), cross-checked against DF R7.5 once it lands.
      Result: the Harness checks margins by a checked-in computation from CIE 15, Bradford and each gamut's published primaries, cross-checked against the Data Foundation PRD's R7.5 once it lands.
- [x] **R2-m13 (Minor) — UJ3.3-f's app-storage read of zx-01 fails a correct build if the file sits in the container**
      (R8.10e, UJ3.3-f). **fix (F53 — testability):** R8.10e's reads leave out the user's file wherever it sits, as
      "outside the file" already says.
      Result: R8.10e never reads the user's file, wherever it sits.
- [x] **R2-m14 (Minor) — no "file unchanged" check for actions that shouldn't write** (R2.5, R8.6; UJ2.1-d, UJ9.4-b).
      **fix (F6, F20 — testability):** after UJ2.1-d's move and UJ9.4-b's When, the file's bytes equal those before.
      Result: UJ2.1-d and UJ9.4-b assert the file's bytes equal those before.
- [x] **R2-m15 (Minor) — R3.3 parses two ways for an All-items item whose own mismatched pair is the most common**
      (R3.3). **covered by 2MN3.**
      Result: covered by 2MN3.
- [x] **R2-m16 (Minor) — sibling halves: Capture UJ3.3-k's mid-session quarantine has no path; F84's read-only clause
      has no case** (Capture UJ3.3-k, Capture R8.18). **fix (F22, F84 — testability; a dated line under Capture F71):**
      UJ3.3-k's quarantine is seeded before the session, or Capture says the clause can't arise in flight; an F84 case.
      Result: Capture UJ3.3-k declares the quarantine before the session, and new Capture UJ3.3-l asserts F84's read-only clause, in a dated line under Capture F71.
- [x] **R2-n1 (Nit) — thin fixture margins, and two fixtures named Daylight** (Harness; UJ3.4-d, UJ7.1-i). **covered
      by 2N5.**
      Result: covered by 2N5.
- [x] **R2-n2 (Nit) — UJ6.4-f/j/k depend on R4.9, R2.9, R6.1 and R6.2, which their Rows cells omit** (UJ6.4-f,
      UJ6.4-j, UJ6.4-k). **fix (editorial — the preamble phases cases by their Rows):** add those rows.
      Result: UJ6.4-f's Rows add R4.9 and R2.9, UJ6.4-j's and UJ6.4-k's R6.1 and R6.2.
- [x] **carried: TEST MJ5 (PARTIAL) — UJ9.6-b still covers four states** (R8.9, UJ9.6-b). **covered by R2-m4.**
      Result: covered by R2-m4.
- [x] **carried: TEST m8 (PARTIAL) — a re-read's deselect of a removed item is asserted nowhere** (R8.5). **covered by
      R2-m2.**
      Result: covered by R2-m2.
- [x] **carried: TEST m9 (PARTIAL) — UJ9.7-d still says "crash during the write"** (UJ9.7-d). **covered by R2-m3.**
      Result: covered by R2-m3.

## peer-interface-reviewer (Claude route)

- [x] **R2-IF-1 (Major) — R8.8's lost-permission branch names a state that doesn't exist, and nothing gates it** (R8.8,
      Build dependencies row 1). **covered by PM2-3.**
      Result: covered by PM2-3.
- [x] **R2-IF-2 (Minor) — the set-aside cause is asserted by its wording** (R4.2, R4.2b, R8.10b, UJ4.1-c, UJ4.7-a).
      **covered by 2MN16.**
      Result: covered by 2MN16.
- [x] **R2-IF-3 (Minor) — E12 names cannot-show by its filter label where R2.8 says its chip label** (R2.8, E12). **fix
      (F38 — editorial; copy matches R2.8):** E12 reads "Can't show:".
      Result: E12 reads "Can't show:".
- [x] **R2-IF-5 (Minor) — the actions ending the undo history are not a closed set** (R4.7, R8.3, E6, UJ6.4-f,
      UJ6.4-i). **covered by PM2-2.**
      Result: covered by PM2-2.
- [x] **R2-IF-6 (Minor) — R3.9 and R6.4 list the changes differently, and both omit a re-scan answer** (R3.9, R6.4).
      **fix (F44 — its "any change"):** one list, in R3.9 and cited by R6.4, with the undo and a re-scan answer in it; a
      case answers ZX-013 from its detail under the re-scan-unanswered filter, ZX-013 selected.
      Result: R3.9 carries the one list, a re-scan answer and an undo in it, and R6.4 cites it; UJ9.1-k answers ZX-013 under the re-scan-unanswered filter, ZX-013 selected.
- [x] **R2-IF-7 (Minor) — E10 is indexed to the wrong surfaces** (E10's index row, Surfaces). **covered by 2N2.**
      Result: covered by 2N2.
- [x] **R2-IF-8 (Minor) — E6's body names P1 actions in a P0 state, unmarked** (E6, R1.4, R8.3). **needs owner (13).**
      Result: F111 landed — E6's P0 body names only P0 actions, the P1 names moving to a [phase: variant-absent] "full" variant; R8.3 is now E6's enumerating row, listing "full", "elsewhere" and R1.4's "interrupted"; the Harness reads a case's "no variant" as "full" once every P1 action lands.
- [x] **R2-IF-9 (Minor) — the headers users see aren't the names R4.8's duplicate rule checks** (R4.8, E11, the Column
      headers). **covered by PM2-7.**
      Result: covered by PM2-7.
- [x] **R2-IF-10 (Minor) — Outbound rows 2–3 cite fences without their document, and row 4 omits DF R1.2** (Outbound
      table). **fix (editorial — the PRD's cross-document cite rule):** "the capture PRD's F70, the Data Foundation PRD's
      F51 and F52", "the Data Foundation PRD's F50, the import PRD's F65"; row 4 adds DF R1.2.
      Result: Outbound rows 2 and 3 cite "the capture PRD's F70, the Data Foundation PRD's F51 and F52" and "the Data Foundation PRD's F50, the import PRD's F65"; row 4 names its R1.2.
- [x] **R2-IF-11 (Minor) — three one-sided mirrors** (Outbound Capture row, Inbound table). **fix (F30, F96 —
      editorial):** the Outbound Capture row adds F96's cause-label table (R4.2b); an Inbound line maps Capture's restore
      refusal to R8.3; the R8.18 Inbound line cites "its F70 and F71".
      Result: the Outbound Capture row adds its Set-aside cause labels table (R4.2b); a new Inbound line maps Capture's restore refusal, with F100's bulk-write refusal, to R8.3; the R8.18 line cites its F70 and F71.
- [x] **R2-IF-12 (Minor) — Capture R5.8 and R6.6 quote the cause differently from Capture's table** (Capture R5.8,
      Capture R6.6). **covered by 2MN19.**
      Result: covered by 2MN19.
- [x] **R2-IF-13 (Nit) — Capture's Traceability reads "Owner decisions F1–F70"** (Capture Traceability). **fix (Capture
      F71 — bookkeeping of its own range):** "F1–F71", recorded in a dated line under Capture F71.
      Result: Capture's Traceability reads F1–F72, F72 being this pass's new Capture fence, which records it.
- [x] **R2-IF-14 (Nit) — the Row transitions restore line names R5.5 only** (Row transitions). **fix (editorial):** add
      R4.6, DF E4's restore.
      Result: the restore route's Rows add R4.6.
- [x] **carried: IF-10 (PARTIAL) — DF R6.3 and OQ 20 don't cover E33's selection-scale undo** (R1.7, OQ 10; DF R6.3, DF
      OQ 20, DF E33). **fix (F3, F7 — editorial; a dated line under DF F50):** DF OQ 20's question names E33's selection
      scope and two deletes in one window, as OQ 10 does; no DF row changes.
      Result: the Data Foundation PRD's OQ 20 question names an item, a collection or E33's selection and two deletes in one window; dated line under its F50; no DF row changes.
- [ ] **carried: IF-16 (PARTIAL) — `docs/product/README.md` line 18 still reads "queued"** (the product README). **fix
      (F56 — editorial), at the bookkeeping close** as round 1's Out-of-scope list holds; nothing in this pass.
      Result: not landed in this pass, as the box states — the product README's line waits for the bookkeeping close.

## peer-privacy-reviewer (Claude route)

- [x] **PRIV-10 (Medium) — the P0 edit and delete cases read leftover text only after a clean close, main file only**
      (R1.4, R4.3, R4.5, R6.3; T1, T6, UJ1.3-c, UJ4.2-a/c, UJ4.4-b, UJ6.3-b). **fix (F35 — testability):** each reads the
      file and what sits beside it (R8.10d) open after the write and after a crash, UJ1.3-c included; moments per D.
      Result: the Harness's byte check reads the file and every file beside it open after the write, after a crash and closed for every byte case (T1, T6, UJ4.2-a/c, UJ4.4-b, UJ6.3-b); UJ1.3-c now reads the bytes for Teal.
- [x] **PRIV-11 (Low) — a collection rename has no byte rule and no case** (R1.3, T5, UJ1.1-a). **fix (F35 — replaced
      text; DF R2.3's metadata):** R1.3 leaves the name it replaces nowhere in the file's bytes; T5 asserts the whole text
      Gouache Set nowhere, its words living on in the new name; R4.8's optional clause not made — reported.
      Result: R1.3 leaves a replaced name nowhere in the file's bytes (inherited obligation for the Data Foundation doc); T5 asserts Gouache Set nowhere; R4.8's optional clause not made — reported.
- [x] **PRIV-12 (Low) — the app-storage readback assumes a sandbox and misses copies the system keeps** (R8.10e, R8.6,
      the map). **fix (F53 — testability):** R8.10e names the app's storage by location under either sandbox reading, plus
      the run's unified-log entries and any system-kept versions of the file; R8.6's clause: **needs owner (32).**
      Result: R8.10e reads the app's storage wherever the build puts it, its unified-log entries, system-kept file versions and system search or Handoff donations, never the user's file; F130 landed in R8.6; UJ9.4-d; dated line under F53.
- [x] **PRIV-13 (Low) — UJ9.4-c's content check can't be read, and its configuration is undeclared** (R8.6, UJ9.4-c).
      **fix (F52 — testability):** the Demo configuration, telemetry and update checks off, and the device PRD's R6.17
      records no outbound attempt at all.
      Result: UJ9.4-c declares the Demo configuration and asserts no outbound attempt at all.
- [x] **PRIV-14 (Info) — R4.7's cite covers only the file half of "never in the file or anywhere outside it"** (R4.7).
      **fix (editorial):** add the Data Foundation PRD's R1.1 and F53.
      Result: R4.7 cites the Data Foundation PRD's R6.2d and R1.1 and this PRD's F53.
- [x] **PRIV-15 (Info) — the help docs could say backups and snapshots keep what they held** (DF R1.5's help docs).
      **needs owner (33).**
      Result: F131 landed — no change here; a Data Foundation Documentation item on post-lock.md notes it for the help docs.
- [x] **PRIV-16 (Info) — keep the verification seam out of shipped builds** (R8.10, R8.6). **needs owner (34).**
      Result: F132 landed — R8.10 says every readback but R8.10f exists only in test builds and no build opens a listening socket or cross-process service for one; UJ9.4-e.

## peer-product-marketing-manager-reviewer (Claude route)

- [x] **N-B1 (Blocker) — E12 files Outside sRGB under "The chip isn't the true colour", false on Display P3** (E12, the
      Mark labels table's group column). **fix (log row 1; F40 — copy honesty to R2.3):** a neutral group heading in the
      table and at E12's start; Outside sRGB adds that without Can't show beside it the chip is true, F36's words kept.
      Result: the Mark labels table and E12 head that group "Colour beyond a limit"; Outside sRGB adds "Without Can't show beside it, the chip is still the true colour", F36's words kept.
- [x] **N-B2 (Blocker) — E8's undo promise outlives R4.7** (E8). **covered by PM2-1**; its ship-order risk needs
      nothing, R6.2 and R4.7 sharing one Build dependencies row.
      Result: covered by PM2-1.
- [x] **N-m1 (Minor) — DF E11 and E26 give the reading mark a second name** (R4.2h, R5.7; DF E11, DF E26). **covered by
      2MN18.**
      Result: covered by 2MN18.
- [x] **N-m2 (Minor) — "samples disagreed" is both a cause and a mark, meaning opposite things** (the Mark labels table,
      E12; Capture's cause labels). **needs owner (27).**
      Result: F125 landed — samples-disagreed's filter label is "Samples disagreed, average accepted"; E12 says a set-aside swatch shows it only as its State cause.
- [x] **N-m3 (Minor) — E9's headline "in ⟨collection⟩" reads as the scope from All items** (E9). **fix (F5, F16 — copy
      honesty to R3.7):** "Colours close to ⟨code⟩ (⟨collection⟩)".
      Result: E9's headline reads "Colours close to ⟨code⟩ (⟨collection⟩)".
- [x] **N-m4 (Minor) — "observer" is jargon on the main surface** (E3, E13 and their "narrowed" variants). **fix
      (editorial — no new WHAT):** "⟨unlike⟩ swatches whose colour was worked out for a different light or viewing angle
      come after the rest in this order".
      Result: E3's and E13's bodies and "narrowed" variants say "whose colour was worked out for a different light or viewing angle".
- [x] **N-m5 (Minor) — E17 describes an action absent at P0** (E17). **covered by PM2-11.**
      Result: covered by PM2-11.
- [x] **N-m6 (Minor) — DF E33 drops its scope sentence at P1** (R6.3, the Outbound DF row; DF E33). **fix (F56 — a
      dated line under DF F52):** E33's ‹P1› sentence opens "Export first saves all of ⟨collection⟩, these swatches
      included".
      Result: the Data Foundation PRD's E33 ‹P1› sentence opens with the export-first scope clause; dated line under its F52.
- [x] **N-n1 (Nit) — Spread reads as an earlier-readings mark in E12** (E12). **fix (F38 — editorial):** Spread's
      definition moves before the groups.
      Result: Spread's definition opens E12, before the groups, and E12's preamble says so.
- [x] **N-n2 (Nit) — "answer it from the swatch" can mean the physical swatch** (E12). **fix (editorial):** "from its
      swatch detail".
      Result: E12 says "answer it from its swatch detail".
- [x] **N-n3 (Nit) — History line R5.2b is labelled "Instrument", beside a Demo Device reading** (the copy file's
      History lines). **fix (editorial — R5.2b's own name):** "Device".
      Result: History line R5.2b is labelled Device.
- [x] **N-n4 (Nit) — the not-compared line says "measured under"** (the copy file's History lines, R5.4). **fix
      (editorial — E9's words):** "worked out under a different light or measurement condition".
      Result: the not-compared line reads "worked out under a different light or measurement condition".
- [x] **N-n5 (Nit) — two deferred sibling drifts aren't tracked** (post-lock.md; Device's Collection Mode line; Capture
      R5.8, R6.6). The Device line: **needs owner (28)**; the Capture drift: **covered by 2MN19.**
      Result: F126 landed as a Device line on post-lock.md ("simulated badge" to "mark", F81); the Capture drift is fixed by 2MN19.
- [x] **carried: PMM Minor 4 (PARTIAL) — E8's undo sentence over-promises** (E8). **covered by PM2-1.**
      Result: covered by PM2-1.
- [x] **carried: PMM Minor 8 (PARTIAL) — DF E33's scope sentence is withdrawn at P1** (DF E33). **covered by N-m6.**
      Result: covered by N-m6.
- [x] **carried: PMM Minor 9 (PARTIAL) — E17's sentence renders at P0** (E17). **covered by PM2-11.**
      Result: covered by PM2-11.
- [x] **carried: PMM Nit 5 (UNRESOLVED; deferred by F81) — Device's "simulated badge"** (Device). **covered by N-n5.**
      Result: covered by N-n5.
- [x] *unrated* **PMM Missing — E19 could say a column can't be removed once added (F63)** (E19). **needs owner (35).**
      Result: F133 landed — E19 says "A column can't be removed once it's added"; E19 lands at pre-alignment.
- [x] *unrated* **PMM Missing — F82's reading of E12 should run on a P3 display with an outside-sRGB swatch** (the
      dogfood line beside M4). **needs owner (36).**
      Result: F134 landed — the check beside M4 runs on a Display P3 display with an outside-sRGB swatch listed; dated line under F82.

## peer-architecture-reviewer (Claude route)

- [x] **A12 (Minor; carried: A12, PARTIAL) — bulk delete and "Use as scan order" at ROWS_CEILING are untimed** (R8.1f,
      UJ9.5-a). **covered by R2-m10** (the cases); the delete's budget: **needs owner (1 — A, asked separately).**
      Result: covered by R2-m10; the delete budget is F99's (2MN12).
- [x] **A16 (Major) — the byte-erasure input handed to ADR-0003 and DF is weaker than the rows and cases** (Build
      dependencies row 2, the Outbound DF row; DF R6.2a, DF R6.2d, DF F52; the ADR-0003 row). **needs owner (4 — D,
      asked separately).**
      Result: covered by 2MJ2 (i) — the ADR-0003 row, the Data Foundation PRD's R2.3 and R6.2a, and the Outbound DF row carry F102's moment and side files.
- [x] **A17 (Minor) — on an sRGB display the gamut edge is judged by two computations** (R2.3, R2.5, the Harness
      margins; DF R3.4). **needs owner (29).**
      Result: F127 landed — the Data Foundation PRD's R3.4 flag is tested by OQ 7's interim at sRGB (its F53 and a dated line under its F4), an input on the ADR-0003 row; UJ2.1-q is the one exempt edge case; dated line under F40.
- [x] **A18 (Minor) — capture's precedence on a single-writer file is no ADR-0005 input** (R8.1f, R8.8, R8.11, UJ9.5-d;
      the ADR-0005 row). **needs owner (2 — B, asked separately).**
      Result: F100 refuses bulk writes in flight instead; no ADR-0005 input, F100 naming none (reported).
- [x] **A19 (Minor) — the All items search workload is unsized** (R1.10, R8.2, UJ9.5-b). **needs owner (5 — E, asked
      separately).**
      Result: covered by 2MJ1.
- [x] **A20 (Minor) — R8.8's P0 failure path has no Stop and no interim** (R8.8). **covered by PM2-3.**
      Result: covered by PM2-3.
- [x] **A21 (Nit) — R8.10e's "its container" assumes a sandboxed build** (R8.10e). **covered by PRIV-12.**
      Result: covered by PRIV-12.
- [x] **A22 (Nit) — whether a capture-side commit ends the undo history is unsaid** (R4.7). **covered by PM2-2.**
      Result: covered by PM2-2.
- [x] **A23 (Nit) — no route for an item a re-read finds removed** (Row transitions). **covered by 2N1.**
      Result: covered by 2N1.
- [x] **A24 (Nit) — Find similar's results depend on which non-working sets DF happened to persist** (R3.7). **covered
      by 2MN4.**
      Result: covered by 2MN4.

## peer-performance-reviewer (Claude route)

- [x] **PERF2-1 (Major) — bulk and collection delete at ROWS_CEILING under byte erasure: unclassed, untimed, measured at
      3.05 s against 2 s** (R8.1c, R8.1f, R8.11, UJ9.5-a, UJ9.5-d; the ADR-0003 row, DF F52). **needs owner (1 — A,
      asked separately)**; its "Delete selected" timing is R2-m10's.
      Result: covered by 2MN12 (F99's DELETE_WRITE_BUDGET, R8.1g) and R2-m10 (UJ9.5-g); F100 removes the in-flight collision.
- [x] **PERF2-2 (Major) — UJ9.5-d almost never makes capture and the bulk write compete** (R8.11, UJ9.5-d). **fix (log
      row 5; F89 — testability):** the clear starts at a set's last Demo sample (Capture R11.7), alternating with "Undo
      change" over 20 sets, Scale narrowed and sorted; R8.11's side: **needs owner (2 — B, asked separately).**
      Result: with F100 refusing bulk writes in flight, UJ9.5-d pits capture against what R8.3 leaves available — a search keystroke, a header fire and a single edit at each of 20 sets' last Demo sample, Scale searched, filtered and sorted; the alternating clear and undo is moot (reported).
- [x] **PERF2-3 (Minor) — R8.11 rests on a TBD sibling constant that nothing lists** (R8.11, Legend). **covered by
      2MN15.**
      Result: covered by 2MN15.
- [x] **PERF2-4 (Minor) — "progress shown" and "responsive throughout" can't be observed** (R8.1f, R8.10b, UJ9.5-a).
      **fix (F42 — testability):** R8.10b reads bulk-write progress; a keystroke and a selection every 100 ms during each
      bulk write, each timed; no progress frame for a write inside BROWSE_RESPONSE_BUDGET: **needs owner (30).**
      Result: F128 landed — R8.10b reads whether a write's progress shows; R8.1f needs no progress frame for a write done within BROWSE_RESPONSE_BUDGET, R8.1g as R8.1f; UJ9.5-g sends a keystroke and a selection every 100 ms during each bulk write and times them.
- [x] **PERF2-5 (Minor) — the no-lazy-tail rule has no case, and R8.2 doesn't clearly carry it** (R8.1d, R8.2,
      UJ9.5-a, UJ9.5-b). R8.2's clause: **covered by 2MJ1**; **fix (F88 — testability):** at each cold open's first-rows
      frame, type the all-match prefix and time it; UJ9.5-b's per-collection data per Owner-needed 5 (E).
      Result: UJ9.5-a and UJ9.5-b type the all-match prefix at each cold open's first-rows frame and time it; UJ9.5-b's collections are R7.7 corpora at IMPORTED_COLUMNS_CEILING.
- [x] **PERF2-6 (Minor) — the measurement conditions aren't reproducible** (R8.1e, R8.10f, M1, OQ 1, the Timing
      workload). Paging: **covered by 2MN11**; "cold": **needs owner (21)**; **fix (F41, F66 — testability):** frames
      read at the display's refresh rate, recorded with the macOS version by R8.10f; p95 by nearest rank.
      Result: frames read at the display's refresh rate, each run recording the macOS version and that rate, and p95 by nearest rank — in the Harness's Timing workload and M1's statistic; R8.10f is unchanged, the rig recording the OS version and rate, not the app (reported).
- [x] **PERF2-7 (Minor) — R8.1 inputs with no timed sample** (R8.1a, R8.1b, R8.1e, UJ9.5-a). **fix (F41 — "selection …
      and the grid"):** R8.1b names toggling a row, R8.1e the grid; UJ9.5-a adds 200 toggles after "Select all" and, in
      R7's phase, the workload and paging in the grid; "Use as scan order" is R2-m10's.
      Result: R8.1b names toggling one row and R8.1e the grid; UJ9.5-a adds toggles after "Select all" and, in R7's phase, the workload and paging in the grid; "Use as scan order" is timed in UJ9.5-g.
- [x] **PERF2-8 (Minor) — file-level limits aren't stated** (R8.1, R8.2, UJ9.5-a). **needs owner (31).**
      Result: F129 landed — R8.1's budgets hold in a file of up to FILE_ITEMS_CEILING items, nothing refused and no budget above any ceiling; UJ9.5-a runs in UJ9.5-b's file; UJ9.5-f is the function-only case at twice it; dated line under F43.
- [x] **PERF2-9 (Nit) — three editorial slips** (R8.1c, R8.2, M1). **fix (F41 — editorial):** R8.1c's next showing
      meets its own budget, already reflecting the write; R8.2 names firing Find similar to E9 as its input; M1's start
      event includes R2.5's gamut change.
      Result: R8.1c's later showing meets that showing's own budget; R8.2 names firing Find similar to E9; M1's start event includes R2.5's gamut change.
- [x] **carried: PERF-2 (PARTIAL) — "Delete collection" and its undo are unclassed** (R8.1c, R8.1f). **covered by
      2MN12** (Owner-needed 1 — A).
      Result: covered by 2MN12.
- [x] **carried: PERF-10 (PARTIAL) — UJ9.5-d can't fail, and its oracle is TBD** (R8.11, UJ9.5-d). **covered by
      PERF2-2 and 2MN15.**
      Result: covered by PERF2-2 and 2MN15.
- [x] **carried: PERF-11 (PARTIAL) — the grid's search, filter and paging are untimed** (R8.1a, R8.1e). **covered by
      PERF2-7.**
      Result: covered by PERF2-7.

**Owner decisions, 2026-09-25.** Items 1–5 were answered as fences F99–F103; for items 1–4 the owner's answers differ from the recommendations written below — the fence governs, never the recommendation (1 → F99 DELETE_WRITE_BUDGET; 2 → F100 no bulk writes while any session is in flight; 3 → F101 the Data Foundation state added in this change; 4 → F102 from the moment the write lands, side files included; 5 → F103 full width). Items 6–36 are fences F104–F134 (item n is F(98+n)), approved as recommended.

## Owner-needed

Each item: the question, the boxes it answers, and a one-line recommended answer. Items 1–5 are the log's five
"owner decision" rows, which the orchestrator is putting to the owner separately. An answer becomes its own dated
fence (F99 on), with a dated Clarified line under any earlier fence it refines.

1. **(A) Bulk and collection delete under byte erasure** *(asked separately)* — PERF2-1, 2MN12, A12; carried PERF-2.
   *Recommend:* class "Delete collection" and its E10 undo in R8.1f under BULK_WRITE_BUDGET, time both at ROWS_CEILING,
   and hand ADR-0003 that erasing a ceiling collection fits that budget without holding the writer past capture's.
2. **(B) Capture precedence against single-writer bulk writes** *(asked separately)* — 2MJ2 (ii), A18, PERF2-2's R8.11
   side. *Recommend:* in flight R8.11 governs R8.1f — no write holds the writer past what lets a save meet
   ROW_CONFIRM_BUDGET (it yields or queues) — as an ADR-0005 input, UJ9.5-d also comparing a run without the writes.
3. **(C) The permission-lost state** *(asked separately)* — PM2-3, 2MN14, R2-IF-1, A20; carried SSE MN9; R2-m9's
   state. *Recommend:* "Stop: the Data Foundation PRD's owed permission-lost state, for R8.8's clause only" in Build
   dependencies row 1, no interim (DF E10 and Capture E26 would each say something false), its case landing with it.
4. **(D) When removed text must be gone, and whether sidecar files count** *(asked separately)* — A16, 2MJ2 (i); PRIV-10
   and R2-M1 follow it. *Recommend:* from the moment the write lands, while open and after a crash, the file and any
   journal, WAL, index or file beside it included — in Build dependencies row 2, the Outbound DF row, DF R6.2a, ADR-0003.
5. **(E) The All items search width** *(asked separately)* — 2MJ1, A19; PERF2-5's UJ9.5-b data follows it.
   *Recommend:* R8.2's budgets hold at IMPORTED_COLUMNS_CEILING per collection across FILE_ITEMS_CEILING, UJ9.5-b
   declaring that width, and ADR-0003 takes the index-or-scan choice as an input under D's byte rule.
6. **Which actions end "Undo change" history, and where it is offered** — PM2-2, 2MN5, R2-IF-5, A22. *Recommend:* any
   committed write but a metadata change ends it (column hide/show and "New collection" too), a capture save or session
   start not (each undo re-checks R8.3); a refused undo keeps its entry; offered only where its collection is shown.
7. **Displayed precision of the other derived values** — PM2-4, 2MN17; carried PM Minor 5, SSE MN17. *Recommend:* a*,
   b*, u*, v* to one decimal as L*; X, Y, Z to two on 0–100; sRGB as 0–255 integers; HSL as whole degrees and percents.
8. **M4's population** — PM2-5. *Recommend:* dogfood sessions ending with at least one re-scan awaiting an answer, the
   start count recorded; a session with none reads "not measured", never 0.
9. **Where closing a detail opened from Find similar returns** — PM2-6. *Recommend:* to E9 with its list, a case
   opening two results in turn.
10. **R4.8's duplicate set against the headers users see** — PM2-7, R2-IF-9. *Recommend:* it covers every header the
    copy file's Column headers table shows as well as the collection's columns; E11's "duplicate" variant unchanged.
11. **How a collection name matches in All items search, and E4 there** — PM2-8, 2MN1. *Recommend:* anywhere in the
    name, as names match (F17); E4 in the All items view says collection names are searched, a variant R3.5 enumerates.
12. **E4 or E5 when the search alone lists nothing** — PM2-9. *Recommend:* E4 whenever the search lists no item,
    whatever filters are active; E5 only when the search lists items and the filters hide them all; UJ3.2-d follows.
13. **Phase-marking E6's and E17's sentences that name P1 actions** — PM2-11, R2-IF-8, N-m5; carried PMM Minor 9.
    *Recommend:* yes, F97's pattern — each moves to a `[phase: variant-absent]` variant its enumerating row lists.
14. **An open item detail and history view when their item changes** — PM2-15. *Recommend:* they show the change as
    the table does (R3.9), within R8.1c's budget.
15. **R3.3's like set** — 2MN3, R2-m15. *Recommend:* in a collection's table, the collection's own reference; in All
    items, the most common pair among listed items with a value (ties per F94); a mismatched non-spectral reading never.
16. **Find similar's compared values, and an item whose working-set value is absent** — 2MN4, A24. *Recommend:* compare
    working-set values only; not offered when the chosen item's is absent; R4.2c shows value-absent for such an item.
17. **The distance line on an item with no current value** — 2MN6. *Recommend:* no distance line shows.
18. **"Use this reading" on a never-true reading** — 2MN7. *Recommend:* offered, as R5.5 reads — the way back from a
    mistaken "old reading was wrong" answer, the original keeping its never-true mark; add a case.
19. **How "Set a field" commits and cancels** — 2MN8. *Recommend:* as R4.3 — Return applies (through E8 above
    BULK_CONFIRM_COUNT) and Escape cancels; no new label.
20. **A bound on readings for R8.1b's history-open budget** — 2MN13. *Recommend:* the budget holds up to 500 readings
    per item (UJ9.5-e timed), a candidate under OQ 1; above it the view works and no budget is promised.
21. **What "cold" means for the first open** — 2MN11, PERF2-6. *Recommend:* the OS file cache purged and the Data
    Foundation PRD's open-file check already finished.
22. **R8.11's interim while Capture's ROW_CONFIRM_BUDGET is TBD** — 2MN15, PERF2-3; carried PERF-10. *Recommend:* the
    engineering plan's declared value (Capture's OQ 5 leaves it there), R8.11 listed under Interim stated.
23. **DF E11 and E26's "not yet settled"** — 2MN18, N-m1; carried SSE MJ2. *Recommend:* "marked as awaiting your
    answer", under a dated DF fence (DF F53), both keeping their alignment.
24. **Capture R5.8 and R6.6's cause wording** — 2MN19, R2-IF-12, N-n5 (Capture). *Recommend:* fix now — both quote
    "flagged as missing or damaged", R8.2's words, in a dated line under Capture F71.
25. **An All-items Find similar tie on distance and code** — 2N3. *Recommend:* then by collection, in collection-list
    order, as F94 breaks ties.
26. **ADR-0006 as a stop for every row** — 2N6. *Recommend:* yes — the paragraph under Build dependencies says so beside
    ADR-0003 (F65), and row 1's item goes, word-neutral.
27. **"Samples disagreed" as a cause and a mark** — N-m2. *Recommend:* the filter label becomes "Samples disagreed,
    average accepted" (R4.2d's words), and E12 says a set-aside swatch shows it only as its State cause.
28. **A post-lock Device line for F81's deferral** — N-n5 (Device); carried PMM Nit 5. *Recommend:* yes — "simulated
    badge" to "mark" at Device's next amendment, citing F81.
29. **How the stored gamut-clipped flag is computed** — A17. *Recommend:* a DF clarification and ADR-0003 input that
    DF R3.4's flag uses OQ 7's interim test at sRGB, plus one declared edge case exempt from the margin rule.
30. **Progress for a bulk write faster than BROWSE_RESPONSE_BUDGET** — PERF2-4. *Recommend:* no progress frame needed.
31. **File-level limits** — PERF2-8. *Recommend:* R8.1's budgets hold in a file of up to FILE_ITEMS_CEILING items, and
    above FILE_ITEMS_CEILING everything works, nothing refused, no budget, a function-only case at twice it.
32. **System-kept document versions and system donations** — PRIV-12. *Recommend:* yes — R8.6 states the file gets no
    system-kept versions and no collection content goes to system search or Handoff (DF R1.1, F53 imply it).
33. **Help-docs transparency about backups and snapshots** — PRIV-15. *Recommend:* no change here; the help docs (DF
    R1.5's) are outside this PRD — note it for them.
34. **The verification seam in shipped builds** — PRIV-16. *Recommend:* R8.10's readbacks other than R8.10f exist only
    in test builds, and no build opens a listening socket or cross-process service for them.
35. **E19 saying a column is permanent** — PMM *unrated*. *Recommend:* yes — "A column can't be removed once it's
    added" (true under F63); copy file only.
36. **F82's reading of E12 on a Display P3 screen** — PMM *unrated*. *Recommend:* yes — the check runs on a P3 display
    with an outside-sRGB swatch listed.

## Checks

- [x] **Testability pairing (process rule 3).** Every row, state and metric changed above has its acceptance case and
      its verification-seam line (R8.10, the Harness, the named defaults, the Test-controls map) updated in the same
      pass, and every owner answer that lands brings its case.
      Result: every changed row, state and metric has its case and seam line, per box above; new cases T16, UJ1.2-f, UJ2.1-p/q, UJ3.2-e, UJ3.3-l/m, UJ3.4-g/h/i, UJ4.6-j, UJ5.3-l/m/n, UJ6.2-i/j/k, UJ6.3-f, UJ6.4-l/m/n, UJ7.1-m/n, UJ8.1-j, UJ9.1-e to UJ9.1-l, UJ9.3-c, UJ9.4-d/e, UJ9.5-f/g, UJ9.7-g, UJ9.8-c, UJ10.1-f.
- [x] **Word count** of the PRD body by rule 14's method (budget 12,000; start 11,985): each Result note that adds body
      words names the rule-free prose it trims — candidates, each checked rule-free before it goes, are the research
      sentences in OQ 1's, OQ 6's, OQ 7's and OQ 11's Decision cells and any clause a row restates from a row it cites.
      Report the final count; flag any fix that cannot be paid for.
      Result: 12,000 words by rule 14's method (from 11,985), at the budget. Paid for by trimming rule-free prose: the research sentences in OQ 1, 2, 6, 7 and 11; the §1–§4, §6 and §7 preamble fence-provenance and hand-off sentences the Inbound table carries; the User journeys paragraph's inherited-case sentence; the Build contract's non-goal fence citations and DF state list; fence provenance in Outbound and Inbound cells; OQ 10's and OQ 12's clauses restating the Data Foundation PRD's OQ 20 and R8.1; clauses R1.4, R4.5, R5.5 and R6.3 restated from rows they cite; the Build dependencies' ADR-0003 input clauses, now listed once in the Outbound DF row; the "cold" definition moved to the Harness's Timing workload. No box was held back.
- [x] **Fences and Carried-by.** Each owner answer becomes a dated fence from F99, in the fence file's grammar, its
      Carried by and fence → row map line filled with actual IDs; an answer refining an earlier fence adds a dated
      Clarified line under it (e.g. F29 by 6, F35 by 4, F42 by 30, F47 by 7, F51 by 8, F73 by 3, F89 by 2 and 22, F97 by
      13). Traceability's "F1–F98" follows. No placeholder left.
      Result: F99–F134 Carried by and map lines filled with IDs, F124, F126 and F131 reading governs no rows with a Why; dated Clarified lines under F16, F17, F18, F29, F40, F41, F42, F43, F46, F47, F51, F53, F54, F68, F81, F82, F89 and F97 (F14, F35, F73 and F86 already carried theirs); Traceability reads F1–F134; no placeholder left.
- [x] **Sibling halves (process rule 12).** Next free sibling fences, verified at 94e7fc8: DF F53, Capture F72, Import
      F66, Export F32, Device F33. A half inside an existing sibling fence's decision lands as a dated Clarified line
      under it, as round 1b did; a new decision takes the next free number, its Authority citing this PRD's fence; each
      status-line clause is extended, peer review pending. This pass: DF — N-m6 (under DF F52), IF-10 (under DF F50);
      Capture — R2-IF-13, R2-m16 (under Capture F71). Owner-dependent: DF (1, 4, 23, 29), Capture (24).
      Result: DF F53 (new, next free verified) with dated lines under DF F4, F17, F50 and F52 and its status-line clause; Capture F72 (new, next free verified) with a dated line under Capture F71 and its status-line clause. Import, Export and Device PRDs unchanged; Device's item is a post-lock line.
- [x] **ADR queue and post-lock.** `docs/decisions/README.md`'s 0003 row (answers 1, 4, 5, 29) and 0005 row (answer 2),
      and `docs/product/post-lock.md` (answer 28), are edited only on the orchestrator's say-so, as F98 authorized in
      round 1b — report.
      Result: on the orchestrator's say-so in this dispatch: the ADR-0003 row now takes seven inputs, adding F102's moment and side files, F103's index or scan and F127's flag test; no ADR-0005 input, no fence naming one; post-lock ticks the DF permission-lost item, attributed to this change's pending PR, and adds the Device (F126) and DF help-docs (F131) lines; the rename item is left unticked.
- [x] **Constants and OQs.** Any constant, interim or measurement condition an answer sets (5, 20, 21, 22, 31) lands in
      the constants table, its owning rows and an OQ with its Interim-stated line; the no-interim list stays derived from
      the Open questions table.
      Result: DELETE_WRITE_BUDGET (10 s) and HISTORY_READINGS_CEILING (500) join the constants table and OQ 1; cold (F119) lands in the Harness's Timing workload, which OQ 1's interim cites; R8.11's interim (F120) sits in OQ 1 and its Interim-stated line; F129 adds R8.1 to FILE_ITEMS_CEILING, OQ 2 and its line; the no-interim list stays None.
- [x] **Traceability.** New IDs (T16, and any case, state or variant an answer adds) are assigned once; nothing is
      renumbered.
      Result: new IDs assigned once — R8.1g, T16 and the cases under Testability pairing; new variants E4 "all items", E6 "full" and "elsewhere", E17 "restore"; nothing renumbered.
- [x] **Standing label check** (check 13) re-run after the copy changes: E8's endings, E9's headline, E12's group
      heading, "Can't show:", Spread's place and "from its swatch detail", E3's and E13's bodies, the History lines'
      "Device" and not-compared line, the column hide/show label, and any label an owner answer adds (27, 11).
      Result: re-run after the copy changes: both directions pass (53 labels; no quoted string in a row or case lacks a copy home); the new labels "Columns", "all items", "full", "elsewhere" and "restore" are written once in the copy file and only quoted elsewhere.

## Out of scope for this pass

- `docs/product/README.md` (IF-16's index status) — at the bookkeeping close.
- Deleting guidance comments — at lock.
- The post-lock tick for this PRD's item — when the PR exists.
- Any change a fence above does not authorize, and every **needs owner** item until the owner answers it.
