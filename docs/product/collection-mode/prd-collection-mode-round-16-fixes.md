# Collection Mode PRD — round 16 fixes (the delta-verify of round 15, 2026-09-26)

This is the resume point for the fix pass after review round 16, subject commit `74f058b`.

- **Who reviewed.** Nine lenses. Their reviews are in the orchestrator's scratchpad as `round16/review-<persona>.md`.
- **No owner decision this round.**
- **Budgets.**
  - Collection Mode stands at 12,397 of 12,400.
  - Data Foundation stands at 8,448 of 8,450.
  - Add no word to either body.

**Already landed by the orchestrator.** Every item below was verified against F211, F212, F215, F218, F61, F62 and D72. Do not redo them.

- **DF R6.2a and the ADR-0003 input: room is now part of the first moment.** They read "…that no read uses that journal or log, no write runs and the volume has room to clear it (within 5 s of room returning, when room comes last)…", which is F218's third bullet (DB16-MAJOR-1, SSE16-m1, R16-m3). The ADR input's garden path and its inline cite are fixed too, and now name F218 and F62.
- **DJ3 (g) clears at the failure again.**
  - Its held write needs at least 1 MB of new log space, so it fails.
  - The declared-full state is pinned: writes that need new space fail, while truncation and in-place overwrites succeed.
  - Its Result is "within 5 s of E15 rendering, the volume still full, no file beside the file holds the removed text, and E35 is not up".
  - This answers IF16-1, R16-M1, R16-M2, SSE16-1, PRIV16-1 and DB m4.
- **DJ3 (i).**
  - Its fixture reads "such as a 'Set a field' of a long value across enough items, whose committed frames would grow the file at least 1 MB past its own end".
  - It runs in (g)'s declared-full state.
  - It asserts that E15 does not render before a single-item edit made while the volume is full. For that edit, E15 renders within 1 s, the edit is never shown disabled and no progress shows (SSE16-m6, D72's not-chosen option).
  - This answers IF16-m5, ARCH16-3, SSE16-m2/m3/m4 and TR16 Minors.
- **New DJ3 (j).** A declared-full state in which truncation and sync also fail until room returns. The one-way E35 check holds until room is restored, then the copy is gone within 5 s (DB m1, SSE16-m5).
- **DJ3's shared fixture.** The held write is "held only once it has begun writing to the file and before read 1 ends", stated in (a) and in each restated fixture (TR16-1).
- **DJ3 (d)** guards:
  - (b), (e) or (f) with nothing beside the file after the held write lands (TR16-1);
  - (i) with nothing beside the file before room is restored (PM16-3, PRIV16-4, R16-m6, ARCH16-2, DB m3);
  - (j) with nothing beside the file at E15.
- **DJ3 (h)** names its read-back channel: "by an SQL read at SQLITE_READER_FLOOR" (a plan Nit).
- **Fences** (Clarified lines only; no fence body touched):
  - Under F218:
    - the give-way line (PM16-2, R16-m4);
    - the scope line: the deferral applies only where a full volume actually stops the clearing (IF16-1, R16-M1, SSE16-1);
    - the E35 and "file open" line (PRIV16-2, PRIV16-3, DB m6).
  - Under DF F62: the give-way and scope lines.
  - Under F215 and DF F61: the crash-clause gloss (SSE16-m8, IF16-m4).
  - Under DF F61: the pointer to F62 (an architecture Nit).
- **Collection Mode.**
  - R8.8 cites "F59–F62" (PMM16-3, IF16-m1, R16-m2); this is editorial.
  - UJ5.4-g asserts "with no label" (PMM16-2).
  - UJ2.3-d's SQL read is scoped to "imported and measurement values" (a database Nit).
- **README rows 4 and 6** read F50–F62.
- **`post-lock.md`:**
  - :46 is conditional: "Where a full volume stops the clearing…".
  - :169 adds two ADR-0003 items: the round-16 architecture notes ARCH16-4/5/6/7, and the database and staff-engineer notes DB m1/m2/m5, SSE16-m5 and UJ6.2-g's log slack.
  - :183 gains a fifth E35 window, needs owner (PMM16-1), and its PM14-4 window is dogfood only.
  - A full-disk help-docs line is added (PRIV16-2).
  - The carried round-15 Minors are added under the next Collection Mode pass.
  - "a full volume that lets truncation run" replaces "a real full volume" in the hook note.

## A. The record

- [x] **A1 — Review log.** Append "## Round 16 — delta-verify of round 15 (2026-09-26)" after Round 15. In order:
  - an intro;
  - the nine reviews verbatim under "### <persona>", each followed by a blank line;
  - "### Orchestrator verification" with C1 verbatim;
  - "### Dispositions" with E3's tally;
  - a closing line.
  - Result: appended after line 25136 of `docs/agent-reviews/2026-09-24-prd-collection-mode-peer-reviews.md`; all nine `round16/review-<persona>.md` files pasted verbatim under `### <persona>` headers (product-manager, staff-software-engineer, test, interface, architecture, privacy, product-marketing, plan, database), each followed by a blank line, then `### Orchestrator verification` (C1 verbatim), `### Dispositions` (E3's table) and the closing resume-point line.

## C. Verification notes (for A1)

- [x] **C1 — Orchestrator verification**, verbatim:
  - **Confirmed at `74f058b`:**
    - (g)'s room-timed deadline passed a build that waits whenever the volume is full (IF16-1, R16-M1, SSE16-1, PRIV16-1); the orchestrator's own round-15 wording.
    - (g)'s write was unsized (R16-M2).
    - R6.2a's D72 clause read as a deadline of its own, and the database lens's probe_reader_at_room.py shows a read spanning room's return keeps the copy (DB16-MAJOR-1).
    - The read-2 gate let a hold placed before SQLite's write lock clear or restart the log early, and the test lens's probe_h16*.py show it (TR16-1).
  - **No owner decision.** Every fix follows D72's own "while a full volume stops the clearing", F215, F212 and R8.10a's test seam.
  - Nothing was rejected.
  - Result: pasted verbatim into the review log's `### Orchestrator verification` subsection, word for word as boxed above.

## D. Statuses

- [x] **D1 — Statuses (bookkeeping for round 16).**
  - **R8.8 stays needs-discussion.** The test lens objected on DJ3 only, and the row changed only editorially: its cite.
  - **DF R6.2 stays ⌛️ Ready for Alignment.** R6.2a's text changed this round.
  - List every change.
  - Result: no status flips this round. R8.8 (CM PRD:446) stays needs-discussion; its only change is the editorial cite fix from "F59–F61" to "F59–F62" (already in the worktree). DF R6.2/R6.2a stays ⌛️ Ready for Alignment; R6.2a's body (DF PRD:228) changed to fold "room" into the first-moment conjunction ("…no write runs and the volume has room to clear it (within 5 s of room returning, when room comes last)…"), fixing DB16-MAJOR-1, and DJ3 (g)/(h)/(i)/(j), F218's/F62's Clarified lines, and post-lock:46/169/183 changed as the checklist's "Already landed" section records; none of this flips either row's resulting status.

## E. Checks

- [x] **E1 — Word counts.** Collection Mode must be ≤ 12,400 and Data Foundation ≤ 8,450.
  - Result: Collection Mode 12,397 of 12,400 — PASS. Data Foundation 8,448 of 8,450 — PASS. Both measured with the specified perl strip-and-count against the current worktree body files; no word was added to either body this pass.
- [x] **E2 — Mechanical checks.** Run checks 1, 2, 6, 7, 10, 11, 12, 13, 15, 16, 17 and 18 read-only, and report PASS or MISS for each.
  - Result (run against both PRDs' bodies/companions in the worktree at HEAD `74f058b` plus uncommitted edits): check 1 (cross-PRD consistency) — PASS; check 2 (index sync, F211/F212/F215/F218 Carried-by vs. Fence → row map) — PASS; check 6 (case refs/copy-state coverage) — PASS; check 7 (Legend constants named in an owning row, CM's 12 constants) — PASS; check 10 (two-sentence scan, incl. DF R6.2a's cell) — PASS (0 hits over threshold in touched cells; pre-existing >2-sentence candidates elsewhere in DF are unrelated to this round and unchanged); check 11 (companion paths/links) — PASS; check 12 (word count) — PASS, see E1; check 13 (label check, both directions) — PASS, no new copy string this round; check 15 (unresolved fill) — PASS; check 16 (guidance comments) — PASS; check 17 (banned adjectives in asserts) — PASS (one naive grep hit on "correction" inside CM journeys UJ4.7-b was a substring false-positive of "correct", not a banned-adjective assert; a word-boundary re-run is clean); check 18 (test-controls map/asserted-value tracing) — PASS, guards satisfied and no new Given/When/Assert cell landed this round. No MISS found, so none is attributable to an orchestrator uncommitted edit.
- [x] **E3 — The disposition tally for A1.**
  - Result:

    | Row | PM | SSE | TR | IF | ARCH | PRIV | PMM | R16 | DB | Resulting status |
    |---|---|---|---|---|---|---|---|---|---|---|
    | R8.8 | ALIGN | ALIGN | OBJECT | ALIGN | ALIGN | ALIGN | ALIGN | ALIGN | ALIGN | needs-discussion |
    | DF-R6.2 (with R6.2a) | ALIGN | OBJECT | OBJECT | OBJECT | ALIGN | ALIGN | ALIGN | OBJECT | OBJECT | ⌛️ Ready for Alignment |
