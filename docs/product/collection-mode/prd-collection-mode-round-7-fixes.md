# Collection Mode PRD — round 7 fixes (final delta, 2026-09-25)

This is the resume point for the fix pass over review round 7's findings (subject commit `a720dae`). A targeted re-check and the lock follow it. The fence file (F1–F201) is the written authorization. Every box below is either:

- **editorial/testability — no new WHAT**: a case, a harness sentence, a Rows cell, a cite or a word-neutral wording fix that makes a case match its row and fence; or
- **post-lock**: an item added to `docs/product/post-lock.md` in that file's style, under the trigger named.

No box changes a requirement row's rule, and no box needs the owner. Anything the pass meets that no fence states is reported, not made. Tick each box with a one-line Result note.

**Budgets.** Count by rule 14's method.

- **Collection Mode body:** 11,982 of 12,000. The only body edit is B12 (+4). The pass lands at or under 11,988, which leaves room for the lock status line.
- **Data Foundation body:** 8,399 of 8,400. The only DF body edit is B11, which is word-neutral.
- **Meaning check:** do one on every rewrite, against the fence Authority text it carries.

The findings are the nine round-7 reviews in `docs/agent-reviews/2026-09-24-prd-collection-mode-peer-reviews.md` (Round 7). The log's verify-the-reviewer table records the three Majors.

## A. The three Majors (testability, companion only)

- [x] **A1 — the swatch-grid cases follow F199.** Answers PM7-1, IF7-2, PLAN7-1, ARCH7-1, 7MJ1, R7-M1 and R7-m1. Journeys only:
  - **UJ10.1-b.** The Assert reads "Every swatch measures the smallest size the build offers, read through R8.10b, and none less than MIN_SWATCH_SIZE on a side".
  - **UJ10.1-d.** Replace 48 pt and 56 pt with sizes the build offers:
    - The Given reads "…the swatch size the first "Grid" of a fresh launch shows on Studio Markers, recorded before the Given".
    - The When reads "set the swatch size to another size S the build offers, S recorded" in place of 48 pt; the second run uses S in place of its 48/56 pt branch.
    - Choose S so that no seeded value's text holds S's number.
    - The storage read looks for S's number.
    - After reopening, the first run fires "Grid" and asserts the swatches show at the size recorded before the Given (R7-m1: R7.2's "lasts while the file stays open").
  - **UJ10.1-f.** "set the swatch size to a size S the build offers other than the one shown", and "its swatches S on a side".
  - **Named defaults.** Say, if it helps, that S is declared per case.
  - **Rows** are unchanged.
      Result: Landed in the journeys only, Rows unchanged. UJ10.1-b asserts "the smallest size the build offers, read through R8.10b, and none less than MIN_SWATCH_SIZE on a side". UJ10.1-d's Given records the first "Grid" size on Studio Markers and shows the grid at that size; its When sets "another size S the build offers, S recorded, choosing an S whose number no seeded value's text holds", the second run uses S, and after reopening the first run fires "Grid" and asserts the recorded size (R7-m1); the Assert reads "Neither read holds S's number as text". UJ10.1-f sets "a size S the build offers other than the one shown", swatches "S on a side". No Named-defaults line: each case declares its S in its own When. Meaning check against F199's Authority ("the default and the range are the build's choice, at or above 24 pt"): the cases now need only two offered sizes and a floor at or above MIN_SWATCH_SIZE; the reopen assertion reads R7.2's own "lasts while the file stays open".
- [x] **A2 — UJ9.5-d's byte read waits for the app's own export too.** Answers IF7-1, PRIV7-1, ARCH7-4, 7MN2 and R7-m2, plus PLAN7-3's and PERF7-2's ordering. In the second run:
  - The Assert's byte clause reads "within 5 s of the later of the outside read's release and the end of the last export started, a functional timeout, …".
  - The export step writes "to a declared folder outside the case's folder" (PRIV7-1 (b)).
  - The second run runs in the first phase, save for its "All items" half, which runs in R1.9's phase (PLAN7-3).
  - In that half, fire "All items" just before each set's last Demo sample, then type the keystroke and fire the header in it. Keep the export and the edit at the set's first sample, and drop "so that its first rows load while the edit lands". A run whose first rows showed before the last sample arrived reports that half not exercised, never passed (PERF7-2).
  - Rows are unchanged.
      Result: Landed in UJ9.5-d, Rows unchanged. The second run now runs in the first phase; its export writes "to a declared folder outside the case's folder"; its "All items" half runs in R1.9's phase, firing "All items" just before each set's last Demo sample and typing the keystroke and firing the header in it, the "so that its first rows load while the edit lands" clause dropped, and the Assert adds that a run in which "All items" showed its first rows before each set's last sample arrived reports that half not exercised, never passed. The byte clause reads "within 5 s of the later of the outside read's release and the end of the last export started, a functional timeout". With B5 the edited value is the exported item's first imported column, so the snapshot and byte clauses read "that item's value in that column" and "the values the edits replaced". Meaning check against F184's and F175's Authority (a read begun before the wipe defers it; old text goes as soon as the app's own long read finishes): the deadline is now the later of the two reads' ends.
- [x] **A3 — the number baseline for a raw-read file.** Answers R7-M2. The Harness's baseline sentence (journeys:106–108) appends: "…in the same file; and, in a file read as its raw contents, where the baseline read of that file holds the text as many times or more."
      Result: Landed verbatim in the Harness's Byte checks. Meaning check against R8.6 (nothing written outside the file): a new write of the number into a raw-read file raises its count over the baseline and still counts; the Test-controls map's summary line is unchanged.

## B. Minors and Nits fixed now (companion only unless noted)

- [x] **B1 — the Harness's first-phase sibling list names the capture PRD's Flag.** Answers IF7-3, PLAN7-2, 7MN3 and R7-n1. After "set-aside list", add ", its Flag (its R5.6) with the collection-side entry its R9.9 makes P0 (F189)".
      Result: Landed after "set-aside list". I also named "with its one-row re-scan (its R8.3)" beside the set-aside list, because B9 adds a first-phase case that drives the capture PRD's R8.3 (P0 there); this is an index entry only. Meaning check against F189's Authority (the capture row "mirrors P0 for that entry only"): holds.
- [x] **B2 — UJ9.4-b's search text.** "qqq" becomes the six-letter nonce "qzxqvj" in its When and Assert, and anywhere else the same case's text recurs (R7-m3).
      Result: UJ9.4-b's When and Assert read qzxqvj; the case's text holds no other qqq. UJ3.1-f, UJ3.2-d and UJ7.1-m keep qqq: they are other cases, none reads the bytes.
- [x] **B3 — DF DJ3's re-read line becomes exercisable.** Answers R7-m4, PERF7-1 and ARCH7-N1. In the DF journeys only:
  - The line runs on a file of FILE_ITEMS_CEILING items (UJ9.5-b's), with the OS file cache purged before "Read it again".
  - Each try (a capture start, a move, an edit) is its own run. A try made after the checks end reports not exercised, never passed.
  - Add a close, a switch and a quit run, each waiting with the re-read's progress showing (R1.11).
  - DJ3's following lines' "each of those writes" becomes "each of the writes the line two above names", or the re-read line moves after them, whichever reads cleaner.
      Result: Landed in the DF journeys only. The re-read line now runs on "a file of Collection Mode's FILE_ITEMS_CEILING items, as its UJ9.5-b declares it, the OS file cache purged before Read it again". Each try is its own run, with close, switch and quit runs that each wait with the re-read's progress showing and go ahead only once the checks end, and a try made after the checks end reports that try not exercised, never passed (R1.11, R1.3, F58). The line moved below the two lines that say "each of those writes", which now follow the writes line directly again. Meaning check against F191's Authority ("edits and other writes show unavailable … with progress shown") and DF R1.11's "closing the file, switching and quitting wait for it": holds. Whether they should wait at all is C3's owner question.
- [x] **B4 — UJ9.8-c control (vii).** The Given says the row was "deleted with secure_delete off, its bytes left in the page's free space" (R7-n2).
      Result: Control (vii) reads "the row deleted with secure_delete off before the read, its bytes left in the page's free space".
- [x] **B5 — UJ9.5 wording.** Answers R7-n3.
  - UJ9.5-d edits "its first imported column" of an item in another collection, in place of "the Family".
  - UJ9.5-c's bulk set fires "Apply to ⟨n⟩ swatches" after E8.
      Result: UJ9.5-d, landed with A2: "then, on one item of the exported collection, set its first imported column", and its Assert follows. UJ9.5-c: "…through "Select all" and "Set a field", then fire "Apply to ⟨n⟩ swatches"" (E8 above BULK_CONFIRM_COUNT, R6.2).
- [x] **B6 — UJ9.5-a's grid workload runs at the smallest size the build offers.** Its 200 size changes alternate between the smallest and largest sizes the build offers (PERF7-3).
      Result: UJ9.5-a's R7 phase reads: "…both pagings in the grid at the smallest swatch size the build offers, and switch "Table" and "Grid" 200 times and change the swatch size 200 times, alternating between the smallest and largest sizes the build offers". Meaning check against F199: sizes are the build's, and no size is named.
- [x] **B7 — DF DJ4's second-read line.** Answers PERF7-5 and ARCH7-N3.
  - The Assert reads E35 before it reads the bytes.
  - Drop the not-exercised clause, which a correct build can never exercise on this fixture.
  - The end-state assertion stays.
      Result: DJ4's line reads "E35 is up whenever the removed text is still in the file's bytes, E35 read before the bytes at each look"; the not-exercised clause is gone and the end-state assertion stays. Meaning check against F173 (E35 "goes away on its own once the erase happens"): E35 down and then clean bytes is the only order a correct build shows.
- [x] **B8 — Rows for the storage half of delete cases.** Answers PRIV7-2. UJ1.3-c, UJ1.3-e, UJ1.3-f and UJ6.3-b add R8.6 to their Rows, since their byte checks also read the app's own storage for the collection name.
      Result: Rows now read UJ1.3-c "R1.4, R8.6", UJ1.3-e and UJ1.3-f "R1.7, R8.6", UJ6.3-b "R6.3, R8.6". UJ6.3-b's texts are item values, not the collection name. R8.6 still covers its storage half, because the undo history and view state are never written outside the file.
- [x] **B9 — the P0 correction loop end to end.** Answers PM7-2. UJ4.7-d's When adds "select ZX-001 and complete a Demo Device set through the capture PRD's R8.3". The Assert adds:
  - ZX-001 is captured;
  - its history holds 2 readings, the new one current with reason initial and the flagged one carrying no never-true mark;
  - no correction question appears (the Data Foundation PRD's R2.9).

  Check that the case stays in the first phase.
      Result: UJ4.7-d's Given adds "the Demo Device connected" (the declared seam input); its When adds "then select ZX-001 and complete a Demo Device set through the capture PRD's R8.3"; its Assert adds that the capture PRD's R11.11 reads ZX-001 captured, its history holds 2 readings (the new one current with reason initial, the flagged one with no never-true mark), and no correction question appears (the Data Foundation PRD's R2.9). Rows stay R4.9. It stays in the first phase: scenario 7 runs there (UJ 4 preamble), and capture R8.3's look-through re-scan and R8.16 are P0 there; the Harness list now names R8.3 (B1). Meaning check against F189's Authority ("review and re-scan it through capture's own P0 rows. History is kept."): holds.
- [x] **B10 — the open-file check's cite.** Answers IF7-N1, ARCH7-N2 and 7N2. The Harness's "(its R1.11)" for the open-file check "ending before the file takes a write" reads "(its R1.11 for a re-read; its R5.4 and F195 for the open-file check)". The ADR-0003 row takes the same correction if it carries the same cite.
      Result: The Harness reads "(its R1.11 for a re-read; its R5.4 and F195 for the open-file check)". The ADR-0003 row is left unchanged: its cite is "(Data Foundation R1.11, R5.4)", which already pairs the re-read with R1.11 and the open-file check with R5.4, and F195 is in the row's own list of inputs, so it is not the same cite. Meaning check against F195: holds.
- [x] **B11 — DF R7.6q.** "That the change is saved" becomes "That the changes are saved" (IF7-N2). Word-neutral, in the DF body; alignment kept.
      Result: DF R7.6q reads "That the changes are saved"; DF body 8,399, word-neutral; its status stays ⌛️ Ready for Alignment. Meaning check against F194 ("Your changes are saved."): holds.
- [x] **B12 — the Outbound Capture Mode row.** Its Rows cell adds R2.4c, R2.4d, R2.6 and R2.7, the rows that need its R8.14 at P0 (IF7-N3; +4 body words).
      Result: The Outbound Capture Mode row's Rows read "R2.4c, R2.4d, R2.6, R2.7, R4.2b, R4.5, R4.9, R8.1f, R8.3" (+4, body 11,986). They now pair with the inbound R8.14 line's rows. Meaning check against F186: holds.
- [x] **B13 — Build dependencies row 4.** "…landing together" reads "R6.2 and R4.7 landing together" (PLAN7-5; word-neutral).
      Result: The literal form ("…and undo, R6.2 and R4.7 landing together") is +3 words, not word-neutral, and would put the body at 11,989, over the 11,988 cap. It landed word-neutral instead: "Multi-item selection, bulk set or clear, undo; R6.2 and R4.7 landing together" (12 words for 12). "Multi-item selection" is R1.10's own term. Meaning check against PLAN-12's intent (E8 lands with R4.7): "together" now binds R6.2 and R4.7 only, and R6.1 stays in the contract cell.
- [x] **B14 — UJ9.8-b.** "each When from the seeded file" (PLAN7-6).
      Result: UJ9.8-b reads "…UJ8.1-a and UJ4.6-a, each When from the seeded file; read the file after each".

## C. Post-lock items (added to `docs/product/post-lock.md`, in its style)

- [x] **C1 — § ADR-0003.** Answers ARCH7-2, 7MN1 and PERF7-4. The item reads:

  > The wipe is two mechanisms. Copying into the main file waits only on reads begun before it (F200's exact deadline covers main-file text). A copy of replaced text in a log frame goes once no read begun before the wipe and no write in progress needs that log — at the latest when a held write lands — and the log's reset may briefly hold a write. E35 follows the bytes, not the copy-back. Log frames are never overwritten in place. A move's copy tolerates a wipe beside it.

  Result: Added under § ADR-0003 as **DF**, verbatim, attributed to Collection Mode F200 and DF F58 (7) and to ARCH7-2, 7MN1 and PERF7-4. It qualifies F200's "the deadline stays exact" for text held in a log frame, and both source reviews asked for owner confirmation; I report it as a fork, and no fence is edited.
- [x] **C2 — § ADR-0003, the moved-file item.** Append "…and no journal or log the app kept beside the file under its old name is left holding its text" (ARCH7-5).
      Result: Appended to the moved-file item after "under its new name", with ARCH7-5 added to its attribution.
- [x] **C3 — next Data Foundation pass, needs owner.** Should closing, switching and quitting end a running re-read rather than wait for it? And what is a re-read "failure" (the re-read could not read the file)? Answers ARCH7-3 and 7N1.
      Result: Added under § Next pass › Data Foundation as **DF**, marked "needs owner", attributed to F191, ARCH7-3 and 7N1.
- [x] **C4 — the help-docs item for F201 (2), needs owner at the help-docs pass.** Answers N7-m1, IF7-4, PRIV7-3 and PERF7-6. As written it contradicts F195, and it covers removal only. Suggested wording: "While an export or Save a copy runs, text you remove or replace stays in the file until it finishes." Or, if the reopen is meant: "Text you removed while another app was reading your file is wiped when you next open it, once SpectroCapture has checked the file."
      Result: Appended to the existing F201 (2) item under § Documentation: "Needs owner at the help-docs pass", why the wording fails (it contradicts F195 and covers removal only), both suggested wordings, and attribution to N7-m1, IF7-4, PRIV7-3 and PERF7-6. The owner's F201 wording itself is unchanged.
- [x] **C5 — § v1 release.** The OQ 1 timing item adds "and the open-file check on UJ9.5-b's file, cold" (PERF7-6).
      Result: The OQ 1 item under § v1 release reads "…and one current Mac, and the open-file check on UJ9.5-b's file, cold; …", with PERF7-6 added to its attribution.
- [x] **C6 — First build PR.** Answers PLAN7-4. The Data Foundation amendment rows this PRD's first phase builds against (R1.11, R7.6p, R7.6q, E34, E35) are aligned in that PRD's own review before the first build PR.
      Result: Added under § First build PR as **Collection Mode / DF**, attributed to PLAN7-4.
- [x] **C7 — First build PR.** The engineering plan declares ROW_CONFIRM_BUDGET, closing UJ9.5-d's row-confirmation half (the plan lens's coverage gap; F120).
      Result: Added under § First build PR as **Collection Mode**, attributed to F120 and the plan review's coverage gap.
- [x] **C8 — § Dogfood.** Note whether:
  - E35 at a reopen ("Saved — …" before the user has acted) confuses (PM7-3);
  - E15's and E34's "your last change" reads well when the failed write was a scan (N7-n1).
      Result: Two items added under § Dogfood as **DF**: E35 at a reopen (F190, F194, PM7-3), and "your last change" after a failed scan write (F193, N7-n1).
- [x] **C9 — next Data Foundation pass.** A storage check that nothing about E35's state is kept outside the file (F190; PRIV7-4).
      Result: Added under § Next pass › Data Foundation as **DF**, attributed to F190 and PRIV7-4.

## Status bookkeeping and checks

- [x] **Flip to aligned.** Take the round-7 flip record's 118 IDs, less any sub-row whose lead is still objected, in the PRD and the copy file.
      Result: Flipped all 118 flip-record IDs. In the PRD tables that took seven Status cells: R3.6, R4.7, R4.9, R8.1 (R8.1a–g with it), R8.10 (R8.10a–f with it), R8.11 and E6. The copy file's E6 Status line reads aligned. Every other ID in the record was already aligned. No lettered sub-row's lead is still objected.
- [x] **Left for the targeted re-check.** R7.1, R7.2, R8.6 and R8.8 stay where they are: R7.2 and R8.8 at pre-alignment; R7.1 and R8.6 at needs-discussion. Report the tally.
      Result: Held: R7.2 and R8.8 pre-alignment; R7.1 and R8.6 needs-discussion. Tally over the 122 IDs, sub-rows counted with their leads: 118 aligned, 2 pre-alignment, 2 needs-discussion. Copy Status equals the index for E1–E19.
- [x] **Word counts.** Collection Mode, Data Foundation and Export.
      Result: Rule 14's method: Collection Mode 11,986 of 12,000 (B12 +4; B13 word-neutral), within the 11,988 cap. Data Foundation 8,399 of 8,400 (B11 word-neutral). Export 3,803, unchanged.
- [x] **Placeholders.** No `_(…)_` left.
      Result: None: `{{…}}` and `_(…)_` occur only inside guidance comments in the five files.
- [x] **Read-only mechanical checks.** Re-run the scripts in `…/scratchpad/mech1-r5/` for checks 1, 2, 5, 6, 8, 11, 12, 13, 17, 18 and 20, and report each verdict.
      Result: Re-run and diffed against a pre-pass run of the same scripts. Check 1: 1a MISS, the same five as rounds 5 and 6 (capture and import carry no inbound table under F188; the R6.9 route); 1c's 34 candidates and the 3 collide lines are unchanged. Check 2 PASS. Check 5 PASS. Check 6 PASS (434 bare Rows IDs, +4 from B8). Check 8: direction 1 PASS, direction 2 NOT-RUN (first lock). Check 11: 11a PASS with comments stripped (the unstripped run's MISSes are the guidance comment's suffix examples, as before); 11b's MISSes are the round-6 fix file's five, as before, and DF journeys' two new links resolve. Check 12 PASS (11,986). Check 13: direction 2 prints nothing, and 53 quoted strings, all labels. Check 17: 2 hits, both "correction", the new one UJ4.7-d's "correction question"; no word-bounded hit. Check 18: the same 6 (1b) When cells as before, no new unsourced datum. Check 20: M1–M4 each resolve. No MISS is new.
- [x] **The column-rename post-lock item stays unticked.** Carried IF-16 is deferred to the bookkeeping close.
      Result: Unticked: post-lock.md's "**DF** — whether a Collection Mode rename moves an imported column's stored name" stays `- [ ]`.
