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

- [ ] **A1 — the swatch-grid cases follow F199.** Answers PM7-1, IF7-2, PLAN7-1, ARCH7-1, 7MJ1, R7-M1 and R7-m1. Journeys only:
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
- [ ] **A2 — UJ9.5-d's byte read waits for the app's own export too.** Answers IF7-1, PRIV7-1, ARCH7-4, 7MN2 and R7-m2, plus PLAN7-3's and PERF7-2's ordering. In the second run:
  - The Assert's byte clause reads "within 5 s of the later of the outside read's release and the end of the last export started, a functional timeout, …".
  - The export step writes "to a declared folder outside the case's folder" (PRIV7-1 (b)).
  - The second run runs in the first phase, save for its "All items" half, which runs in R1.9's phase (PLAN7-3).
  - In that half, fire "All items" just before each set's last Demo sample, then type the keystroke and fire the header in it. Keep the export and the edit at the set's first sample, and drop "so that its first rows load while the edit lands". A run whose first rows showed before the last sample arrived reports that half not exercised, never passed (PERF7-2).
  - Rows are unchanged.
- [ ] **A3 — the number baseline for a raw-read file.** Answers R7-M2. The Harness's baseline sentence (journeys:106–108) appends: "…in the same file; and, in a file read as its raw contents, where the baseline read of that file holds the text as many times or more."

## B. Minors and Nits fixed now (companion only unless noted)

- [ ] **B1 — the Harness's first-phase sibling list names the capture PRD's Flag.** Answers IF7-3, PLAN7-2, 7MN3 and R7-n1. After "set-aside list", add ", its Flag (its R5.6) with the collection-side entry its R9.9 makes P0 (F189)".
- [ ] **B2 — UJ9.4-b's search text.** "qqq" becomes the six-letter nonce "qzxqvj" in its When and Assert, and anywhere else the same case's text recurs (R7-m3).
- [ ] **B3 — DF DJ3's re-read line becomes exercisable.** Answers R7-m4, PERF7-1 and ARCH7-N1. In the DF journeys only:
  - The line runs on a file of FILE_ITEMS_CEILING items (UJ9.5-b's), with the OS file cache purged before "Read it again".
  - Each try (a capture start, a move, an edit) is its own run. A try made after the checks end reports not exercised, never passed.
  - Add a close, a switch and a quit run, each waiting with the re-read's progress showing (R1.11).
  - DJ3's following lines' "each of those writes" becomes "each of the writes the line two above names", or the re-read line moves after them, whichever reads cleaner.
- [ ] **B4 — UJ9.8-c control (vii).** The Given says the row was "deleted with secure_delete off, its bytes left in the page's free space" (R7-n2).
- [ ] **B5 — UJ9.5 wording.** Answers R7-n3.
  - UJ9.5-d edits "its first imported column" of an item in another collection, in place of "the Family".
  - UJ9.5-c's bulk set fires "Apply to ⟨n⟩ swatches" after E8.
- [ ] **B6 — UJ9.5-a's grid workload runs at the smallest size the build offers.** Its 200 size changes alternate between the smallest and largest sizes the build offers (PERF7-3).
- [ ] **B7 — DF DJ4's second-read line.** Answers PERF7-5 and ARCH7-N3.
  - The Assert reads E35 before it reads the bytes.
  - Drop the not-exercised clause, which a correct build can never exercise on this fixture.
  - The end-state assertion stays.
- [ ] **B8 — Rows for the storage half of delete cases.** Answers PRIV7-2. UJ1.3-c, UJ1.3-e, UJ1.3-f and UJ6.3-b add R8.6 to their Rows, since their byte checks also read the app's own storage for the collection name.
- [ ] **B9 — the P0 correction loop end to end.** Answers PM7-2. UJ4.7-d's When adds "select ZX-001 and complete a Demo Device set through the capture PRD's R8.3". The Assert adds:
  - ZX-001 is captured;
  - its history holds 2 readings, the new one current with reason initial and the flagged one carrying no never-true mark;
  - no correction question appears (the Data Foundation PRD's R2.9).

  Check that the case stays in the first phase.
- [ ] **B10 — the open-file check's cite.** Answers IF7-N1, ARCH7-N2 and 7N2. The Harness's "(its R1.11)" for the open-file check "ending before the file takes a write" reads "(its R1.11 for a re-read; its R5.4 and F195 for the open-file check)". The ADR-0003 row takes the same correction if it carries the same cite.
- [ ] **B11 — DF R7.6q.** "That the change is saved" becomes "That the changes are saved" (IF7-N2). Word-neutral, in the DF body; alignment kept.
- [ ] **B12 — the Outbound Capture Mode row.** Its Rows cell adds R2.4c, R2.4d, R2.6 and R2.7, the rows that need its R8.14 at P0 (IF7-N3; +4 body words).
- [ ] **B13 — Build dependencies row 4.** "…landing together" reads "R6.2 and R4.7 landing together" (PLAN7-5; word-neutral).
- [ ] **B14 — UJ9.8-b.** "each When from the seeded file" (PLAN7-6).

## C. Post-lock items (added to `docs/product/post-lock.md`, in its style)

- [ ] **C1 — § ADR-0003.** Answers ARCH7-2, 7MN1 and PERF7-4. The item reads:

  > The wipe is two mechanisms. Copying into the main file waits only on reads begun before it (F200's exact deadline covers main-file text). A copy of replaced text in a log frame goes once no read begun before the wipe and no write in progress needs that log — at the latest when a held write lands — and the log's reset may briefly hold a write. E35 follows the bytes, not the copy-back. Log frames are never overwritten in place. A move's copy tolerates a wipe beside it.
- [ ] **C2 — § ADR-0003, the moved-file item.** Append "…and no journal or log the app kept beside the file under its old name is left holding its text" (ARCH7-5).
- [ ] **C3 — next Data Foundation pass, needs owner.** Should closing, switching and quitting end a running re-read rather than wait for it? And what is a re-read "failure" (the re-read could not read the file)? Answers ARCH7-3 and 7N1.
- [ ] **C4 — the help-docs item for F201 (2), needs owner at the help-docs pass.** Answers N7-m1, IF7-4, PRIV7-3 and PERF7-6. As written it contradicts F195, and it covers removal only. Suggested wording: "While an export or Save a copy runs, text you remove or replace stays in the file until it finishes." Or, if the reopen is meant: "Text you removed while another app was reading your file is wiped when you next open it, once SpectroCapture has checked the file."
- [ ] **C5 — § v1 release.** The OQ 1 timing item adds "and the open-file check on UJ9.5-b's file, cold" (PERF7-6).
- [ ] **C6 — First build PR.** Answers PLAN7-4. The Data Foundation amendment rows this PRD's first phase builds against (R1.11, R7.6p, R7.6q, E34, E35) are aligned in that PRD's own review before the first build PR.
- [ ] **C7 — First build PR.** The engineering plan declares ROW_CONFIRM_BUDGET, closing UJ9.5-d's row-confirmation half (the plan lens's coverage gap; F120).
- [ ] **C8 — § Dogfood.** Note whether:
  - E35 at a reopen ("Saved — …" before the user has acted) confuses (PM7-3);
  - E15's and E34's "your last change" reads well when the failed write was a scan (N7-n1).
- [ ] **C9 — next Data Foundation pass.** A storage check that nothing about E35's state is kept outside the file (F190; PRIV7-4).

## Status bookkeeping and checks

- [ ] **Flip to aligned.** Take the round-7 flip record's 118 IDs, less any sub-row whose lead is still objected, in the PRD and the copy file.
- [ ] **Left for the targeted re-check.** R7.1, R7.2, R8.6 and R8.8 stay where they are: R7.2 and R8.8 at pre-alignment; R7.1 and R8.6 at needs-discussion. Report the tally.
- [ ] **Word counts.** Collection Mode, Data Foundation and Export.
- [ ] **Placeholders.** No `_(…)_` left.
- [ ] **Read-only mechanical checks.** Re-run the scripts in `…/scratchpad/mech1-r5/` for checks 1, 2, 5, 6, 8, 11, 12, 13, 17, 18 and 20, and report each verdict.
- [ ] **The column-rename post-lock item stays unticked.** Carried IF-16 is deferred to the bookkeeping close.
