# Collection Mode PRD — round 19 fixes (the delta-verify of round 18, 2026-09-26)

This file is the resume point for the fix pass after review round 19, whose subject commit is `591044a`.

- **Who reviewed.** Nine lenses. Their reviews are in the orchestrator's scratchpad as `round19/review-<persona>.md`.
- **Owner decision this round (2026-09-26).** The owner chose "Fix + micro re-check" for SSE19-1. The fix is applied, and only the objecting lenses re-check it: staff-engineer, test and database. This follows round 9's micro re-check before lock.
- **Budgets.** Neither body changed, nor did the ADR-0003 input. Collection Mode stands at 12,397 of 12,400 and Data Foundation at 8,448 of 8,450.

**Already landed by the orchestrator.** Each item was verified against F215, F218, F61, F62 and D72.

- **The Major: DJ3 (i), for SSE19-1 and DB19-MAJOR-1.** Round 18 rescoped (i)'s early-clearing guard. That let (i) pass a build that unlinks the log on a full volume and recovers once room returns. The build's file does not open after a crash in that window, and outside reads fail throughout it.
  - (i)'s When now reads: "once E15 renders for it, take an SQL read at SQLITE_READER_FLOOR through an outside read-only connection and end it; restore room no sooner than 10 s after read 1's end and once that read has ended".
  - (i)'s Assert gains "that SQL read, taken while the volume is full, opens the file and shows the landed write's change".
  - (d)'s (i) clause is keyed to that same read, both for the early-clearing guard and for "whose edit lands".
  - The fix also covers the "taken and ended before room is restored" Nits from IF19-n4, ARCH19-5, TR19, SSE and DB.
- **Post-lock only.** No row or case moved, beyond the Major above.
  - **E35 owner item.**
    - Its sample body reads "stays with your file", following the main-file-wipe item.
    - The body carries PL:215's conditions.
    - (b) gains a draft, and each option gains a line on what it fixes and what it costs.
    - "Saved —" is at odds with E15 only once E15 shows.
    - This answers R19-1, R19-2, PMM19-5, PRIV19-3 and IF19-n6.
  - **Positive case.** It declares (i)'s state with space left in the log, never (j)'s (ARCH19-2).
  - **The DJ3 runs item gains:**
    - R19-3's run: space freed while the file is closed;
    - the move run's old path (R19-5, PRIV19-4);
    - R18-2's run done under the hook, or asserting the failure of the open (ARCH19-1);
    - a write-side twin of the room-return run (a plan spec gap);
    - its "E35 not up" follows the owner item (a plan Nit).
  - **The harness item gains:**
    - the map declarations and reads with the app open (IF19-m2, TR19-m2, R19-6 and a privacy Nit);
    - (g)'s folder scope and scratch recipe, taken after the fill (IF19-m1, SSE19-m2, and plan and database Minors);
    - a free-space guard for (g), (i) and (j) (TR19-m1);
    - read-backs for the other early-vanish guards (SSE19-m3);
    - (c)'s post-crash look (plan and SSE19-m1 Minors);
    - "rolled back if it took the lock" and "once per held write" (IF19-n1 and n2, ARCH19-3);
    - (g) runs under the hook (ARCH19-4);
    - (j)'s scope and its "either or both" (IF19-n3, and test and plan Nits).
  - **First build PR.** A pointer to the two DJ3 items (IF19-m4).
  - **Main-file-wipe item.** A refused log sync copies nothing (a database Minor).
  - **ADR-0003 items gain:**
    - the preallocation evidence and order (ARCH19-7, a database Nit);
    - the no-room opening state (ARCH19-1);
    - an allocation of any kind failing under the hook (a plan Minor).
  - **ADR-0005 group.** A note for AGENTS.md and the queue (ARCH19-6).
  - **Help-docs line.**
    - It reads "Keep the files beside yours with it…".
    - Its "in your file" half is dropped only if the answer shows the wipe always reaches disk.
    - Under "only disclosed" it adds the power-loss clause.
    - This answers PMM19-1, PMM19-2, PMM19-4 and PRIV19-1.
  - **The v1 release item** recommends the help-docs line in any case. Option (c) alone ships no true disclosure (PMM19-3, R19-4, PRIV19-2).
- **Editorial.**
  - DF F62's give-way line cites "its Clarified line from the round-16 product-manager review's PM16-2" (IF19-m3).
  - F218's and DF F62's round-17 bookkeeping lines add "(d)'s guards for (g), (i) and (j) carry it with them".
  - F218's map line reads "DJ3 (d), (g), (i) and (j)" (IF19-n5).

**Recorded with no change.**
- **"The Test-controls map's accessibility row" in UJ5.4-a to -e (R19-7).** The file uses this short name ten times for the "accessibility and keyboard" row, which it names unambiguously.
- **CMF:1884, and the "no write runs" wording at PL:49 and PL:215 (a privacy Nit).** R6.2a states all three conditions, and PL:216 carries the write half.

## A. The record

- [ ] **A1 — Review log.** Append "## Round 19 — delta-verify of round 18 (2026-09-26)" after Round 18's closing line in `docs/agent-reviews/2026-09-24-prd-collection-mode-peer-reviews.md`. In order:
  - an intro: subject `591044a`, nine lenses, and the owner decision;
  - the nine reviews verbatim under "### <persona>", in round 18's order, each followed by a blank line;
  - "### Orchestrator verification", with C1 verbatim;
  - "### Editorial edits", listing the three editorial items above;
  - "### Dispositions", with E3's tally;
  - a closing line naming this file as the resume point and saying a micro round 20 follows.

## C. Verification notes (for A1)

- [ ] **C1 — Orchestrator verification**, verbatim:
  - **Confirmed at `591044a`.** SSE19-1 and DB19-MAJOR-1 are the same defect, and grep confirms it.
    - DJ3 (i)'s Assert read the landed write back only "no sooner than 5 s after room is restored".
    - (d)'s rescoped (i) guard fired only "while the landed write's change still reads back".
    - So a run whose text leaves the log early while outside reads fail goes on to be judged, and it passes if the app recovers once room returns.
    - The database lens's `probe_i19.py` and `probe_i19u.py` show it on HFS+ and APFS: CANTOPEN while the volume is full, 400 rows read back after room returns, and SQLITE_CORRUPT after a crash in the window.
  - **Owner decision.** "Fix + micro re-check" (2026-09-26).
  - **Nothing was rejected.** Two items are recorded with no change, each with its reason.

## D. Statuses

- [ ] **D1 — Statuses (bookkeeping for round 19).**
  - **R8.8 stays needs-discussion.** Staff-engineer objected on DJ3 (i).
  - **DF R6.2 stays ⌛️ Ready for Alignment.** Staff-engineer and database objected on DJ3 (i).
  - **Neither row's text changed.** Both flip at micro round 20 if staff-engineer, test and database align. The other six lenses' round-19 ALIGNs stand, because the only change since is (i)'s Assert and guard, which none of them objected to.

## E. Checks

- [ ] **E1 — Word counts.** Collection Mode must be ≤ 12,400 and Data Foundation ≤ 8,450.
- [ ] **E2 — Mechanical checks.** Run checks 1, 2, 6, 7, 10, 11, 12, 13, 15, 16, 17 and 18 read-only, and report PASS or MISS for each. Check 18 covers DJ3 (i) and (d) as changed. Check 2 reads each fence's Carried-by with its Clarified lines.
- [ ] **E3 — The disposition tally for A1.**

  | Row | PM | SSE | TR | IF | ARCH | PRIV | PMM | R19 | DB | Resulting status |
  |---|---|---|---|---|---|---|---|---|---|---|
  | R8.8 | ALIGN | OBJECT | ALIGN | ALIGN | ALIGN | ALIGN | ALIGN | ALIGN | ALIGN | needs-discussion |
  | DF-R6.2 (with R6.2a) | ALIGN | OBJECT | ALIGN | ALIGN | ALIGN | ALIGN | ALIGN | ALIGN | OBJECT | ⌛️ Ready for Alignment |
