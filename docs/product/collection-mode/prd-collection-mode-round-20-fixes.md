# Collection Mode PRD — round 20 fixes (micro re-check of the round-19 DJ3 (i) fix, 2026-09-26)

This is the resume point after micro round 20, whose subject commit is `1aed29a`.

- **Who reviewed.** Following the owner's "Fix + micro re-check" decision, three lenses reviewed: staff-engineer, test and database. Their reviews are in the orchestrator's scratchpad as `round20/review-<persona>.md`.
- **Budgets.** Neither body changed, nor did the ADR-0003 input. Collection Mode stands at 12,397 of 12,400, and Data Foundation at 8,448 of 8,450.

**Already landed by the orchestrator.**

- **SSE20-1 (Major), also raised as the Minors TR20-m1, DB20-m1, DB20-m2 and SSE20-m1.** Round 19's wording, "once E15 renders for it, take an SQL read", left (d)'s "whose edit lands" guard with no read in the case it exists for, the failed log pin. In that case a correct build's edit lands, no E15 renders and no read is taken. The run failed on E15, and the room restore could never happen. Read that late, the check also left a window before room returned in which a dropped log was reported not exercised rather than failed.
  - (i)'s When now reads: "no sooner than 1 s after the edit and 10 s after read 1's end, take an SQL read at SQLITE_READER_FLOOR through an outside read-only connection and end it; then, as the next step, restore room".
  - The read exists on both branches. It is the last step before room returns, and the Assert and (d) stay keyed to it.
  - All three lenses proposed this wording.
- **Post-lock.** The harness item names "(i)'s SQL read while full and its read-back" (a test Nit).

**Confirmed resolved in round 20.** SSE19-1 and DB19-MAJOR-1. The test lens verified them by mutation and the database lens by probe, both on APFS and HFS+.
- A correct build passes: the read returns (400, 'other2') while the volume is full.
- The build that unlinks the log fails with CANTOPEN.
- A build that truncates or zero-fills the log in place fails with NOTADB or SHORT_READ.

## A. The record

- [x] **A1 — Review log.** Append "## Round 20 — micro re-check of the round-19 DJ3 (i) fix (2026-09-26)" after Round 19's closing line. It holds:
  - the three reviews verbatim under "### <persona>";
  - "### Orchestrator verification", with C1;
  - "### Dispositions", with D1's tally;
  - a closing line naming this file and the round-21 nano re-check.

## C. Verification

- [x] **C1 — Orchestrator verification**, verbatim:
  - **Confirmed at `1aed29a`.** DJ3 (i)'s When took the SQL read only "once E15 renders for it", and (d)'s edit-lands clause keys to "that same SQL read". With the log pin failed, a correct build's edit lands, so the read is never taken and the run fails. The test lens's mutation (a log restarted rather than truncated before the seeding) reproduces it.
  - **The fix** is the wording all three lenses gave.
  - **Nothing was rejected.**

## D. Statuses

- [x] **D1 — Tally.**

  | Row | SSE | TR | DB | Resulting status |
  |---|---|---|---|---|
  | R8.8 | OBJECT | ALIGN | ALIGN | needs-discussion |
  | DF-R6.2 (with R6.2a) | OBJECT | ALIGN | ALIGN | ⌛️ Ready for Alignment |

  The staff-engineer lens objected on SSE20-1 alone, and both rows flip on its ALIGN at nano round 21. Test and database aligned in round 20. The other six lenses' round-19 ALIGNs stand, because the only change since is (i)'s read timing.
