# Collection Mode PRD — round 14 fixes (the delta-verify of round 13, 2026-09-26)

This is the resume point for the fix pass after review round 14, subject commit `7328002`.

- **Who reviewed.** The same nine lenses as round 13. Their reviews are in the orchestrator's scratchpad, `round14/review-<persona>.md`.
- **No owner decision this round.** Round-10 to round-13 rules carry over.
- **Word budgets.** Collection Mode must stay ≤ 12,400 and is at 12,397. Data Foundation must stay ≤ 8,450 and is at 8,431. **Add no word to either PRD body.**

**Already landed by the orchestrator.** These were verified against F211, F215, F216, F58 (1) and F61. Do not redo them.

- **DF DJ3's journal-or-log line is now eight self-contained rows, (a) to (h), one per sub-run.** This answers the interface lens's structural suggestion and removes the "As (x)" inheritance that produced a Major in each of rounds 12–14.
  - Rows (b) to (h) take "the journal-or-log fixture (a)'s row declares".
  - Each row states its own actions, look cadence and full Result.
  - (d) is the not-exercised guard for every row, and for an (h) run whose first byte read after the reopen finds nothing beside the file.
- **(g)** times the log copy from the failure, with the volume still full. Room is restored no sooner than 10 s later. This answers R14-M1, PRIV14-1, PM14-1, IF14-2, ARCH14-2, and Minors m5 and m1.
  - The privacy, test and database lenses each ran a full-volume probe on SQLite 3.53.4, on HFS+ and APFS images. TRUNCATE returned (0,0,0) with the volume still full.
- **(h)** begins read 2 while the write is held, releases the write, and crashes once it has landed with read 2 still running. It ends read 2 no sooner than 10 s after the reopen. After the reopen, E35 is up at each look that finds the text beside the file (F58 (1)).
  - This answers R14-M2, TR14-2, IF14-1, SSE14-1, DB14-MAJOR-1, ARCH14-1 and ARCH14-3.
  - The architecture lens's probe (variant C) shows a landed write keeps the copy across the crash until read 2 ends.
- **UJ5.3-r** declares "four earlier spectral readings", with CX1's M1 value "at D50/2°" (TR14-1).
- **UJ5.4-d** declares its current reading "with an M1 value at D50/2°" (R14-m1, a database Minor).
- **UJ5.4-d, UJ5.4-f and UJ5.4-i** assert "with no label, read through the Test-controls map's accessibility row" (PM14-5, PMM14-3, R14-m2, TR m6).
- **The Test-controls map's accessibility row** reads "each header's accessible name and the Compare slot's text" in its Observable column. Its Rows cell adds R2.1 and R5.8.
- **The copy's R5.4 not-compared trigger** reads "where, both having values in this collection's measurement condition, either was worked out under another illuminant or observer" (PM14-3, a TR Nit).
- **post-lock.md:46** is annotated: F215 and DF F61 settle the E15 half of the log copy, and the E34 half stays open.
- **The ADR-0003 input's crash clause** reads "after a crash at the first such moment after reopening" (ARCH14-6).
- **R5.4's pronoun ("when it is readable")** was left as round 14 aligned it. All lenses rate it Minor or Nit, and F207's "never compared" and UJ5.3-p pin it, so it goes to post-lock (D3).

## A. The record

- [x] **A1 — Review log.** Append "## Round 14 — delta-verify of round 13 (2026-09-26)" after Round 13, leaving everything above it byte-identical. In order:
      Result: Appended verbatim (1,057 inserted lines, confirmed by `wc -l` against the pre-pass 23,116 and post-pass 24,173 line counts, and by `diff` on the first 23,116 lines showing byte-identical against the pre-pass file at `7328002` — no line above the new heading touched). Order: an intro line naming subject `7328002` and the nine lenses over the amendment `7c12387..7328002`; the nine reviews verbatim under "### <persona>" in round 13's order (product-manager, staff-software-engineer, test, interface, architecture, privacy, product-marketing, plan, database), each followed by a blank line before the next heading (verified: `grep -n "^### "` over the new section shows every boundary clean, no fused heading); "### Orchestrator verification" carrying box C1's bullets verbatim, one indent level shallower, exactly as round 13's own C1 paste was de-nested; "### Dispositions" carrying box E3's per-row tally, built from the nine reviews' own final disposition tables; a closing line naming this file. No owner-adjudication subsection landed, matching this round's "no owner decision" note.
  - an intro;
  - the nine reviews verbatim under "### <persona>", in round 13's order, each followed by a blank line;
  - "### Orchestrator verification", with C1 verbatim;
  - "### Dispositions", with E3's tally;
  - a closing line.

## B. Fences

- [x] **B1 — Clarified pointer lines**, dated 2026-09-26. Their authority is "round-14 orchestrator bookkeeping; no owner decision; the fence it points to governs".
      Result: Two Clarified lines landed, each carrying that Authority: under Collection Mode F202 (collection-mode fences:1731, IF14-m2, ARCH14-N1), pointing to F215; under DF F59 (data-foundation fences:649, IF14-m2), pointing to F61. Both are new lines appended below each fence's existing Clarified line; no existing fence line was edited (confirmed by `git diff` showing only `+` insertions under both `### F202` and `### F59`, no `-` lines).
  - Under Collection Mode F202: a pointer to F215 (IF14-m2, ARCH14-N1).
  - Under DF F59: a pointer to F61 (IF14-m2).

## C. Verification notes (for A1)

- [x] **C1 — Orchestrator verification**, verbatim:
      Result: This bulleted text landed verbatim (one indent level shallower, as the top-level content of the review log's new "### Orchestrator verification" section rather than nested under a "C1 —" bullet, exactly as round 13's C1 was pasted) — confirmed by comparing the appended section against this box's own text, byte-identical apart from that de-nesting.
  - **Confirmed at `7328002`:**
    - DJ3 (h) said "As (c)" with nothing set aside, so it inherited (c)'s reopen clause (R14-M2, TR14-2, IF14-1, SSE14-1, DB14-MAJOR-1, ARCH14-1).
    - DJ3 (g) timed the deadline from "room being restored" (R14-M1, PRIV14-1, PM14-1, IF14-2, ARCH14-2). Both are the orchestrator's own round-13 wording.
    - UJ5.3-r's CX1 had lost "spectral" (TR14-1).
  - **The fixes.** The orchestrator rewrote DJ3 as eight self-contained rows, so no sub-run inherits a clause. It anchored (g) at the failure, following F211 and F215, backed by three lenses' full-volume probes, and rebased (h) on a landed write.
  - **Nothing rejected, no owner question.**

## D. Fixes

- [x] **D1 — UJ2.3-d, two Nits.**
      Result: Journeys:241 rewritten. The header-accessible-names read now cites "the Test-controls map's accessibility row" in place of R8.10b (SSE and TR Nits). The Set-a-field close now reads "choose State (imported), type Nova and press Return" in place of "choose State (imported) and enter Nova" (IF Nit).
  - "choose State (imported) and enter Nova" becomes "choose State (imported), type Nova and press Return" (IF Nit).
  - Its "through R8.10b" reads for "Columns"' entries and the detail labels become "through the Test-controls map's accessibility row" (SSE and TR Nits).
- [x] **D2 — The Test-controls map.** If any other case still reads accessible names "through R8.10b", point it at the accessibility row. Report the case IDs.
      Result: No other case reads accessible names "through R8.10b". Every `R8.10b` hit in the journeys file (journeys:15, 41, 87, 241, 535, 542, 552, 566, 583) was read; apart from UJ2.3-d (fixed by D1), every remaining hit uses R8.10b for a progress state, a decoded/token value, a cross-process socket readback or a swatch-size readout, never an accessible name. No further case IDs to report.
- [x] **D3 — post-lock.md.** Add these items in the file's style, each citing its finding:
      Result: Nine items landed, each citing its finding, in the file's style. Under "### ADR-0003" (post-lock.md:166): the ARCH13-1 definition's "everything that journal or log now holds" corrected to "every committed frame", and a "Round 14 adds two more" sentence appended for ARCH14-4 and ARCH14-5/the database lens's round-14 Minor/TR m3. Under "### Collection Mode" (post-lock.md:113): a new item for R5.4's pronoun (PM14-2, IF14-m1, and the SSE, TR and architecture Nits); and PL:108 widened to also cover "Columns"' built-in-entry labels, not only E8's ⟨column⟩ (PMM14-4). Under "### Data Foundation" (post-lock.md:51): two new items — DF R2.3 to mirror R6.2a's "a journal or log aside" (a database Nit), and F61's "(F60)" credit belonging to D69 via F215 (an architecture Nit). Help-docs (post-lock.md:194): a "Needs fix at the help-docs pass" sentence added naming "…and move the files beside it with it" (PRIV14-2). Needs owner (post-lock.md:49, extending ARCH13-6): a sentence added on another app's write also holding the log copy while E35 says "reading" (PRIV14-3). Dogfood (post-lock.md:180): a "Round 14 adds a third and fourth window" sentence added for the held-write-under-E15 case (PMM14-1) and the crash-discards-a-held-write case (PM14-4).
  - **ADR-0003:**
    - How a pending wipe survives a close or crash, so E35 can show at a reopen while another app's read holds removed text (ARCH14-4).
    - The clearing runs with no busy handler, since DJ3 (f)'s 1 s bound passes a sub-second wait (ARCH14-5, the database lens's round-14 (f) Minor, TR m3).
    - In PL:166's ARCH13-1 definition, "everything that journal or log now holds" becomes "every committed frame" (an architecture Nit).
  - **Next Collection Mode pass:** R5.4's "when it is readable" to read "when both are readable" (PM14-2, IF14-m1, and the SSE, TR and architecture Nits).
  - **Next Data Foundation pass:**
    - DF R2.3 to mirror R6.2a's "a journal or log aside" (a database Nit).
    - F61's "(F60)" credit (an architecture Nit).
  - **Help-docs (PRIV14-2).** F201's move line adds "…and move the files beside it with it".
  - **Needs owner, extending PL:49 (PRIV14-3).** Another app's write also holds the log copy, while E35 says "reading".
  - **Dogfood:**
    - E35 beside E15 after a failed write (PMM14-1).
    - E35 at a reopen after a crash that lost a bulk edit (PM14-4).
  - **Widen PL:108** to cover "E8 and 'Columns'" labels for built-in columns (PMM14-4).
- [x] **D4 — Statuses (bookkeeping for round 14's dispositions).**
      Result: R2.1 (CM PRD:284) flipped needs-discussion → aligned; R5.8 (CM PRD:389) flipped pre-alignment → aligned. R5.4 (CM PRD:385) flipped pre-alignment → needs-discussion. R8.8 (CM PRD:446) stays needs-discussion — no change (every non-abstaining lens ALIGNED, but the test lens's OBJECT on DJ3 (h) means this box's own instruction is to hold it at needs-discussion, not flip it). DF R6.2 (DF PRD:216) flipped ⌛️ Ready for Alignment → ✋ Needs Discussion. DF R6.2a carries no Status cell of its own — its Deletion-lifecycle table (DF PRD's "#### Deletion lifecycle") has no Status column — so its bookkeeping is recorded here and in the review log's Dispositions table only, no cell to edit. All changes: R2.1 needs-discussion→aligned; R5.8 pre-alignment→aligned; R5.4 pre-alignment→needs-discussion; R8.8 unchanged at needs-discussion; DF R6.2 ⌛️ Ready for Alignment→✋ Needs Discussion; DF R6.2a bookkeeping-only (no PRD cell), now tracked at ✋ Needs Discussion alongside R6.2. Note: post-lock.md:148 ("the Data Foundation PRD's R6.2 and R6.2a... are still ⌛️ Ready for Alignment") was not in this box's scope and was left unedited; it is now stale against this flip and is flagged in the report as a cross-reference the next pass should update.
  - **Flip to aligned:** R2.1 and R5.8. Every non-abstaining lens chose ALIGN in round 14, and neither row changed after `7328002`.
  - **Set to needs-discussion:** R5.4, objected by the test lens on UJ5.3-r's fixture only, with the row unchanged; and R8.8, objected by the test lens on DJ3 (h) only, with the row unchanged.
  - **Set DF R6.2 and its sub-row R6.2a to ✋ Needs Discussion.** They were objected on DJ3's cases only, and R6.2a's text is unchanged since `7328002`.
  - List every change.

## E. Checks

- [x] **E1 — Word counts.** Both budgets must hold.
      Result: measured by rule 14's method after every fix above. Collection Mode: 12,397 of 12,400 — PASS, unchanged from the pre-pass figure (this pass's only Collection Mode PRD-body edits were the three status-cell swaps rule 1 exempts — needs-discussion↔aligned↔pre-alignment are each one token, so the count held exactly). Data Foundation: 8,430 of 8,450 — PASS, down one word from the pre-pass 8,431 (the exempted DF R6.2 status swap: "⌛️ Ready for Alignment", 4 space-separated tokens, → "✋ Needs Discussion", 3 tokens).
- [x] **E2 — Mechanical checks.** Run checks 1, 2, 6, 7, 10, 11, 12, 13, 15, 16, 17 and 18, read-only. Report PASS or MISS.
      Result:
      - **1 (cross-PRD, both directions):** PASS. Check 1c: extracted every sibling ID token from this round's changed lines (`git diff -U0 HEAD -- docs ':!docs/agent-reviews' | grep -E '^[+-]'`) across UJ2.3-d, UJ5.3-r, UJ5.4-d/f/i, the Test-controls map's accessibility row, DJ3 (a)–(h), the ADR-0003 input line, and the two new fence Clarified lines — every cross-document ID is qualified, either inline ("the Collection Mode PRD's F215", "the Data Foundation PRD's R3.3e/R3.3g") or by the surrounding bracketed group naming its owning document; every same-document ID (a PRD's own rows/fences cited in its own journeys/fences file) needs no qualifier and carries none extra. No bare cross-document ID found.
      - **2 (index sync):** PASS, unchanged. No `Carried by` line was touched this pass — confirmed by `git diff` on both fences files showing only new Clarified-line insertions, no `-` line under any `### F` heading — so the fence → row map is unchanged from round 13's confirmed state.
      - **6 (case refs/copy coverage):** PASS. The accessibility row's Rows cell, now "R2.1, R5.8, R8.9, R8.10", resolves to four live Collection Mode row IDs (verified: R2.1 PRD:284, R5.8 PRD:389, R8.9 PRD:447, R8.10 PRD:448). DJ3's own rightmost column, "E35" and "E35 / E15", both resolve to live Data Foundation copy states (DF copy:28 E15, DF copy:30 E35). No Rows cell I touched (UJ2.3-d, UJ5.3-r, UJ5.4-d/f/i) had its own Rows cell edited this pass.
      - **7 (constants named in owning row):** PASS, unchanged — no constant touched this pass.
      - **10 (two-sentence scan):** PASS. Re-run in full over every Collection Mode and Data Foundation requirement cell with rule 14's exact perl pattern: 0 Collection Mode hits over 2 sentences; the five Data Foundation hits (DF R2.3, R2.4-region, R3.4, and one more) are pre-existing rows this pass did not touch, unchanged from before this round. R2.1's cell counts at exactly 2 sentences (verified directly), as this box requires.
      - **11 (companion paths/links):** PASS. No new Markdown link was added this pass; the Companions lines and every existing link are untouched.
      - **12 (word count):** see E1. Rule-migration scan: no new companion content section beyond the Clarified-line additions in each fences file (each carrying its own dated Authority) and the post-lock.md edits (each carrying its own finding cite); post-lock.md is outside the four companions this check scans.
      - **13 (label check):** PASS. Every quoted string added or changed this pass (`git diff -U0 HEAD -- docs ':!docs/agent-reviews' | grep '^+' | grep -oE '"[^"]*"'`) is either a pre-existing, already-established copy label quoted again in new prose ("Columns", "Set a field", "Compare", "Show history"), a citation of existing copy body text ("another app is reading your file", "Your changes are saved", both from DF copy's E35 row), or a citation of existing fence/row prose in post-lock.md commentary (outside this check's PRD/journeys/copy scope) — no new label was invented that the copy file does not already provide.
      - **15/16 (unresolved fill / guidance comments):** PASS, 0 hits, swept across both PRDs, their companions and post-lock.md.
      - **17 (banned adjectives in asserts):** PASS. No new hit in this pass's added lines (checked both Collection Mode and Data Foundation journeys' Assert-shaped cells); the one regex match (UJ4.7-b's "correction answer") is the pre-existing false positive rounds 11–13 already recorded, not "correct" as an adjective.
      - **18 (test-controls map / asserted values traced):** PASS by inspection for every case touched this pass. The accessibility row's surface is now driven by UJ2.3-a, UJ2.3-d and UJ5.4-c/d/e/f/h/i, all declared. No new datum-class value (a URL, an identifier or an instant) was introduced by this pass's edits — "Nova" in UJ2.3-d's rewritten When is the same value the case already asserted before this pass, unaffected by the wording change, and every DJ3 sub-run's asserted state ("the file's own bytes hold the removed text nowhere", "E35 is not up") is a stated condition, not a datum this check's pattern would flag.
- [x] **E3 — The disposition tally for A1.**
      Result: Tally table built from the nine reviews' own final disposition tables and landed verbatim in the review log's new "### Dispositions" section (see A1). R2.1 and R5.8 flip to aligned (unanimous non-abstaining ALIGN, per D4). R5.4 and R8.8 are set to needs-discussion (each objected by the test lens only, on UJ5.3-r's fixture and DJ3 (h) respectively, per D4). DF-R6.2 (with R6.2a) is set to ✋ Needs Discussion (objected by eight of nine lenses on DJ3's (g) and (h) cases; only product-marketing ALIGNED). Every row in the tally is accounted for in D4's change list; nothing flips without a unanimous non-abstaining ALIGN.
