# Collection Mode PRD — round 15 fixes (the delta-verify of round 14, 2026-09-26)

This is the resume point for the fix pass after review round 15, whose subject commit is `17de231`.

- **Lenses.** Eight ran: the round-14 set without product-marketing, which aligned every row in round 14. Their reviews are in the orchestrator's scratchpad as `round15/review-<persona>.md`.
- **Owner decision.** D72 is recorded verbatim in `round15/round15-adjudication.md`.
- **Word budgets.**
  - Collection Mode is at 12,397 of 12,400. Add nothing to its body.
  - Data Foundation is at 8,444 of 8,450, after D72's clause. Add nothing further to its body.

## Already landed by the orchestrator

Each edit was verified against F58 (1), F211, F212, F215, F61 and D72. Do not redo them.

- **DJ3's rows (a)–(h) were regenerated, and row (i) was added.** Nine self-contained rows in total. The changes:
  - **Looks.** Looks start "from once the delete shows done" (R15-M1).
  - **Row (a).** Its main-file clause again reads "while the write still runs" (SSE15-m1, ARCH15-5, test m1).
  - **Read 2 in (b), (e), (f) and (h).** Read 2 now begins only once a look finds the file's own bytes clean, with the write still held (R15-m5, ARCH15-9). In (h) this also gates the crash on clean bytes (SSE15-1, TR15-1, ARCH15-1).
  - **Row (h).**
    - It now asserts that "the landed write's change reads back from the file" after the reopen (DB round-15 Minor, ARCH15-7).
    - Its post-reopen E35 check starts "once the file's opening state is up" (IF15-m5, SSE15-m5, test m2, ARCH15-10).
    - It now cites F212 (a test Nit).
  - **Row (g), under D72.**
    - From E15 until room is restored, the one-way E35 check holds.
    - Within 5 s of room being restored, the log copy is gone and E35 is down. Room returns no sooner than 10 s after E15 renders.
    - The full volume is declared through R7.3's induced store-full.
    - This answers DB15-MAJOR-1, PRIV15-1, ARCH15-2 and the test and interface Minors on (g).
  - **Row (d).** It names "(a)–(c) or (e)–(i)" (Nits from several lenses). It also adds "a run of (g) whose released write lands rather than failing" (DB15-MAJOR-2).
  - **New row (i), the D72 growth case.** A held write of at least 1 MB lands while read 1 runs, the volume is declared full, and read 1 ends.
    - The main file is clean within 5 s of read 1's end.
    - The one-way E35 check holds until room is restored.
    - Within 5 s of room being restored, the log copy is gone (ARCH15-2, PRIV15-1, DB15-MAJOR-1).
- **DF R6.2a (D72).** After "no write runs" it now reads ", or, where a full volume stops that clearing, within 5 s of room returning,".
- **The ADR-0003 input.** It carries the same clause and cites F218 and DF F62.

## A. The record

- [x] **A1 — Review log.** Append "## Round 15 — delta-verify of round 14 (2026-09-26)" after Round 14. In order:
      Result: Appended verbatim (963 inserted lines, confirmed by `wc -l` against the pre-pass 24,173 and post-pass 25,136 line counts, and by `diff` on the first 24,173 lines showing byte-identical against the pre-pass file at `17de231` — no line above the new heading touched). Order: an intro naming subject `17de231` and the eight lenses over the amendment `7328002..17de231` (product-manager, staff-software-engineer, test, interface, architecture, privacy, plan, database — round 14's nine minus product-marketing, which aligned every row in round 14); the eight reviews verbatim under "### <persona>", each followed by a blank line (verified: `grep -n "^### "` over the new section shows every boundary clean, one blank line each side); "### Orchestrator verification" carrying box C1's bullets verbatim, one indent level shallower; "### Owner adjudication (2026-09-26)" carrying `round15-adjudication.md` from its second line on; "### Dispositions" carrying box E3's per-row tally; a closing line naming this file. Anchor verified by GitHub slug rules (computed programmatically): `## Round 15 — delta-verify of round 14 (2026-09-26)` slugs to `round-15--delta-verify-of-round-14-2026-09-26`, matching the Authority-index link added in B1.
  - an intro naming the eight lenses and why product-marketing did not run;
  - the eight reviews, verbatim, under "### <persona>", each followed by a blank line;
  - "### Orchestrator verification", with C1 verbatim;
  - "### Owner adjudication (2026-09-26)": paste `round15-adjudication.md` from its second line on, since its first line is that heading;
  - "### Dispositions", with E3's tally;
  - a closing line.

## B. Fences

- [x] **B1 — F218** goes after F217, in F215's grammar. Its Authority quotes D72 verbatim.
      Result: Landed at `prd-collection-mode-fences.md` after F217, in F215's Authority/Decision/Why/Carried-by grammar. Authority quotes D72 verbatim from `round15-adjudication.md`. Carried by matches this box's text exactly: R8.8, the Data Foundation PRD R6.2a, the Data Foundation PRD DJ3 (g) and (i), and the Data Foundation PRD F62. Added to the fence → row map and the Authority index (D72, linking to the new Round 15 section).
  - **Title:** "A full volume defers the log copy until room returns (2026-09-26)" (D72; qualifies F215).
  - **Decision:**
    - Where a full volume stops the clearing of a journal or log, the copy stays and E35 stays up.
    - Within 5 s of room being restored, the app clears it on its own, with no user action.
    - F215's "first moment" reads "…and the volume has room to clear it".
    - Text in the main file is still wiped at its normal deadline.
  - **Why:** the database lens's APFS probe (SQLITE_IOERR_TRUNCATE in 8 of 46 runs), and the growth case the privacy, database and architecture lenses reproduced on HFS+ and APFS.
  - **Carried by:** R8.8, the Data Foundation PRD R6.2a, the Data Foundation PRD DJ3 (g) and (i), and the Data Foundation PRD F62.
- [x] **B2 — Data Foundation F62**, the Data Foundation half of F218, written in F61's style.
      Result: Landed at `prd-data-foundation-fences.md` as F62, in F61's Authority/Decision/Why/Rows grammar, under a new "## Collection Mode round-15 amendment (2026-09-26)" heading (mirroring F61's own section) and added to the fence → row map.
  - **Rows:** R6.2 and R6.2a, and DJ3.
      Result: F62's Rows field reads "R6.2, R6.2a, DJ3, and the inbound Collection Mode line's fence range, which becomes F52–F62."
  - **Ranges:** the inbound Collection Mode line's range becomes F52–F62, and CM PRD:478 reads "F52–F62 record". Both are editorial range numbers, and word-neutral.
      Result: DF PRD:280 (the inbound Collection Mode line) now reads "F52–F62 list"; CM PRD:478 now reads "F52–F62 record". Both are digit-only range swaps — confirmed word-count-neutral by the before/after measurement in E1.
  - **Status line:** the Data Foundation status line's range reads F56–F62.
      Result: DF PRD:3 now reads "PR #21 (Collection Mode, F50–F54, F56–F62; peer review pending)."
- [x] **B3 — Clarified lines,** each dated 2026-09-26.
      Result: All landed as described below.
  - **Under F215:** "F218 qualifies the first moment for a full volume". Authority: D72.
      Result: `prd-collection-mode-fences.md`, under F215: "**Clarified 2026-09-26 (owner decision D72; F218):** F218 qualifies the first moment for a full volume."
  - **Under the round-14 pointer lines at Collection Mode F202 and DF F59**, one line each saying the pointer supersedes only "this fence's journal-or-log deadline"; the main-file clause and E35's end condition stand (IF15-m2, PRIV15-2, SSE15-m2, ARCH15-4).
    - Authority: "round-15 orchestrator bookkeeping; no owner decision".
    - Also note that "after a crash, at the first open at which that holds" means the first such moment after reopening, as the ADR-0003 input reads (IF15-m3, SSE15-m2, R15-m4, and a PM Nit).
      Result: One new Clarified line landed under each: CM fences (under F202, after the round-14 pointer) and DF fences (under F59, after the round-14 pointer). Each reads: "**Clarified 2026-09-26 (round-15 orchestrator bookkeeping; no owner decision; IF15-m2, PRIV15-2, SSE15-m2, ARCH15-4):** the round-14 line above supersedes only this fence's journal-or-log deadline; the main-file clause and E35's end condition stand. \"After a crash, at the first open at which that holds\" means the first such moment after reopening, as the ADR-0003 input reads (IF15-m3, SSE15-m2, R15-m4)." Neither existing fence line was edited (confirmed by `git diff` showing only `+` insertions under both `### F202` and `### F59`, no `-` lines).
  - **Maps and indexes:**
    - the fence → row map gains F218;
    - Traceability reads F1–F218;
    - the Authority index adds D72;
    - the amendment clause reads F202–F218 everywhere it appears: the PRD status line, the preamble and README row 6;
    - the DF status line's clause reads through F62.
      Result: Fence → row map gained an F218 line (CM fences) and an F62 line (DF fences), each consistent with that fence's own Carried-by/Rows field, matching the map's established per-file convention. Traceability (CM PRD:242) now reads "Owner decisions F1–F218". Authority index gained a D72 row linking to the review log's Round 15 section (anchor verified programmatically: the heading slugs to `round-15--delta-verify-of-round-14-2026-09-26`). The amendment clause now reads F202–F218 in the PRD status line (CM PRD:3), the fence preamble's "Amendment pending" item (CM fences:16), and README row 6 (docs/product/README.md:18, which also gained the R8.8/DF-R6.2-R6.2a not-yet-aligned note per D1). The DF status line (DF PRD:3) now reads through F56–F62.

## C. Verification notes (for A1)

- [x] **C1 — Orchestrator verification**, verbatim:
      Result: This bulleted text landed verbatim (one indent level shallower, as the top-level content of the review log's new "### Orchestrator verification" section rather than nested under a "C1 —" bullet, exactly as round 14's C1 was pasted) — confirmed by comparing the appended section against this box's own text, byte-identical apart from that de-nesting.
  - **Confirmed at `17de231`:**
    - DJ3 (h) crashed without waiting for clean main-file bytes (SSE15-1, TR15-1, ARCH15-1). This was the orchestrator's own round-14 rewrite. The test lens's probe_h4 reproduced it, and probe_h5 showed the gated version works.
    - DJ3's looks had no start point (R15-M1).
    - (g)'s "from the failure" was not reliably deliverable on APFS (DB15-MAJOR-1), and (g)'s write could land (DB15-MAJOR-2).
    - PL:46's note overstated that clearing needs no room (ARCH15-2, PRIV15-1).
  - **Owner decision.** The full-volume case went to the owner as D72. The rest are case fixes settled by F58 (1), F211, F212 and F215.
  - Nothing was rejected.

## D. Fixes

- [x] **D1 — post-lock.md.**
  - **:46.** Rewrite the E15 half as "settled by Collection Mode F218 / DF F62 (D72): the log copy stays, E35 up, until room returns, then clears within 5 s (DJ3 (g), (i))". Keep the E34 half open. Fix the missing full stop and the bare "F215" (IF15-m4, PRIV15-1, SSE15-m3, ARCH15-2).
      Result: post-lock.md:46 rewritten exactly as specified; the E34 half ("what stays open is a wipe or clearing that falls due while E34 is up") is kept unchanged; the missing full stop after "added in PR #21)" is added; the bare "F215" is replaced by the qualified "Collection Mode F218 / DF F62 (D72)".
  - **:169, the ADR-0003 note.** In the "uses the log" definition, add "and no write has committed since it began" (ARCH15-3, R15-m2). Add these notes:
    - ARCH14-4's pending-wipe marker is load-bearing for DJ3 (h); decide it before the first build PR that runs (h), and keep it in the file itself (DF F58 (1)) (ARCH15-6, PRIV15-3).
    - The declared-full test hook must behave like a real full volume: writes that allocate fail, while truncation and in-place overwrites succeed (ARCH15-8).
    - The growth-case evidence: the database lens's probe_room.py and the architecture lens's probe15_growth.py (a DB Minor).
      Result: post-lock.md:169's "uses the log" definition now reads "...had taken in every committed frame and no write has committed since it began, so...". A new "Round 15 adds three more:" sentence appended at the end of the paragraph, naming ARCH14-4's load-bearing pending-wipe marker for DJ3 (h) with its DF F58 (1) in-file-only constraint (ARCH15-6, PRIV15-3), the declared-full test hook's real-full-volume behavior (ARCH15-8), and the growth-case evidence, probe_room.py and probe15_growth.py (a DB Minor), in the file's established "Round N adds M more" style.
  - **:183.** Re-point the Dogfood item "a crash that discards a held write" to DJ3 (c), or to dogfood only (N3, SSE15-m4, the database and privacy Nits).
      Result: Re-pointed to DJ3 (c) — the database lens's specific finding: (c) crashes with the R8.1f/g write still held and unreleased (never committed), which is the actual "held write discarded by a crash" scenario now that (h) lands its write before crashing.
  - **:18, README row 6 (R15-m1, R13-m7, R14-m3).** Add: "R8.8 and the Data Foundation PRD's R6.2/R6.2a are not yet aligned". Or state that the amendment's re-lock precedes building, which it does.
      Result: Added verbatim to docs/product/README.md row 6, immediately after the "ready to build once ADR-0003, ADR-0005 and ADR-0006 are accepted" clause (in the same edit that updated the amendment range to F202–F218).
- [x] **D2 — Collection Mode journeys.**
  - **UJ2.3-d.** Point its "Columns" entries and detail Identity labels at the collection-surface and item-detail rows of the Test-controls map, not the accessibility row (N4 and Nits).
      Result: journeys:241 rewritten. "Read, through the Test-controls map's accessibility row" now reads "through the Test-controls map's collection-surface and item-detail rows" — the collection-surface row's "columns and displayed values" observable covers "Columns"' entries, and the item-detail row's "each line's content" covers the detail Identity labels; the accessibility row's own inputs (header accessible names and the Compare slot) covered neither.
  - **Scope its SQL read** to "among TT-001's and TT-002's stored values" (a DB Nit).
      Result: the Result cell now reads "an SQL read shows only the column stored State changed, among TT-001's and TT-002's stored values, to Nova for both items...".
  - **Move UJ2.3-d's baseline SQL read into the When** (a PM Nit).
      Result: "take a baseline SQL read" moved into the Action (Controlled input) cell, immediately before "fire \"Set a field\""; the Result cell no longer carries "against one taken just before \"Set a field\"".
- [x] **D3 — Statuses (bookkeeping for round 15).**
  - **Flip to aligned:** R5.4. Every non-abstaining lens chose ALIGN in round 15, and the row is unchanged since `17de231`.
      Result: R5.4 (CM PRD:385) flipped needs-discussion → aligned.
  - **Keep at needs-discussion:** R8.8. The test lens objected on DJ3 (h) only, and the row is unchanged.
      Result: R8.8 (CM PRD:446) left at needs-discussion — no edit (test lens's TR15-1 objection on DJ3 (h)).
  - **Set DF R6.2 to ⌛️ Ready for Alignment.** R6.2a's text changed under D72.
      Result: DF R6.2 (DF PRD:216) flipped ✋ Needs Discussion → ⌛️ Ready for Alignment. DF R6.2a carries no Status cell of its own — its Deletion-lifecycle table has no Status column — so its bookkeeping is recorded here and in the review log's Dispositions table only, no cell to edit.
  - List every change.
      Result: R5.4 needs-discussion→aligned; R8.8 unchanged at needs-discussion; DF R6.2 ✋ Needs Discussion→⌛️ Ready for Alignment; DF R6.2a bookkeeping-only (no PRD cell), now tracked at ⌛️ Ready for Alignment alongside R6.2. This flip moves R6.2/R6.2a to a status *ahead* of most individual lens dispositions this round (several OBJECTed) because D72 settles the substantive database/privacy finding (DB15-MAJOR-1, PRIV15-1) that was R6.2a's own text, while the remaining objections (SSE15-1, TR15-1, ARCH15-1/ARCH15-2, R15-M1, DB15-MAJOR-2) are against DJ3 as it stood before the orchestrator's already-landed rewrite of rows (a)–(i) at `17de231`; see the Dispositions table's note.

## E. Checks

- [x] **E1 — Word counts.** Both bodies must stay within budget: Collection Mode ≤ 12,400 and Data Foundation ≤ 8,450.
      Result: measured by rule 14's method after every fix above. Collection Mode: 12,397 of 12,400 — PASS, unchanged from the pre-pass figure (this pass's only Collection Mode PRD-body edits were the R5.4 status swap and three digit-only range swaps — F202–F217→F202–F218, F1–F217→F1–F218, F52–F61→F52–F62 — each token-count-neutral). Data Foundation: 8,445 of 8,450 — PASS, up one word from the pre-pass 8,444 (the two range swaps, F56–F61→F56–F62 and F52–F61→F52–F62, are token-neutral; the exempted DF R6.2 status swap, "✋ Needs Discussion" (3 tokens) → "⌛️ Ready for Alignment" (4 tokens), adds the one word — this box's framing that the status-cell swaps "cost no words" does not hold exactly for this particular swap, reported here rather than silently absorbed; the result is still within budget).
- [x] **E2 — Mechanical checks.** Run checks 1, 2, 6, 7, 10, 11, 12, 13, 15, 16, 17 and 18, read-only, and report PASS or MISS.
      Result:
      - **1 (cross-PRD, both directions):** PASS. Every cross-document ID this pass touched or added is qualified by its owning document, either inline ("the Data Foundation PRD R6.2a", "the Collection Mode PRD's F218") or by the row/table context that already names the document (CM PRD:478's and DF PRD:280's "F52–F62" cites sit inside rows already headed "Data Foundation" / "Collection Mode" respectively, matching the pre-existing F52–F61 convention). No bare cross-document ID found in this pass's added or changed lines.
      - **2 (index sync):** PASS. The CM fences → row map's new F218 line is byte-identical to F218's own Carried-by field, matching that file's convention (every existing map line there is byte-identical to its fence's Carried by). The DF fences → row map's new F62 line follows that file's own paraphrased-but-consistent convention (semicolons, an added link), matching F59/F60/F61's map lines exactly in form, each built from that fence's own Rows field.
      - **6 (case refs/copy coverage):** PASS. UJ2.3-d's Rows cell ("R2.1, R2.10, R6.2") is unchanged and resolves to three live Collection Mode rows. Its reassignment to "the collection-surface and item-detail rows" resolves to two live Test-controls map surfaces (PRD:581, PRD:583) whose Observable columns cover what UJ2.3-d now asserts through them.
      - **7 (constants named in owning row):** PASS, unchanged — no constant touched this pass.
      - **10 (two-sentence scan):** PASS. Re-run over this pass's changed requirement/journeys cells: 0 hits over two sentences (the R5.4 and DF R6.2 edits were status-cell-only; UJ2.3-d's cells were re-run and are unchanged in sentence count from before this pass). Fence Decision text (F218, F62) is exempt under process rule 7 (rationale/fence text lives outside the dispositionable row).
      - **11 (companion paths/links):** PASS. The one new link this pass added — the Authority index's D72 row (`round-15--delta-verify-of-round-14-2026-09-26`, computed by GitHub slug rules and confirmed against the actual heading landed in A1) — resolves. No other new Markdown link was added.
      - **12 (word count):** see E1. Rule-migration scan: no new companion content section this pass touched (only the two PRD bodies, both fences files, post-lock.md, README.md and the review log were edited; post-lock.md and README.md are outside the four companions this check scans).
      - **13 (label check):** PASS. Every quoted string added or changed this pass is either an existing, already-established copy or UI label quoted again ("Columns", "Set a field"), a citation of existing copy body text ("another app is reading your file", from DF copy's E35 row), or fence-deliberation quoting (Authority Q&A, F215's own "first moment" wording being requoted under F218) — no new user-facing label was invented that the copy file does not already provide.
      - **15/16 (unresolved fill / guidance comments):** PASS, 0 hits, swept across the full amendment diff (`7328002..HEAD`) over docs excluding agent-reviews.
      - **17 (banned adjectives in asserts):** PASS. 0 hits in this pass's added lines, and 0 hits in the journeys files' full amendment diff since round 14 (`7328002..HEAD`), including the already-landed DJ3 rewrite.
      - **18 (test-controls map / asserted values traced):** PASS. UJ2.3-d's re-pointed surfaces (collection-surface, item-detail) declare every observable the case now asserts through them ("Columns"' entries, displayed values, detail line content); no new datum-class value was introduced by this pass's edits.
- [x] **E3 — Disposition tally for A1.**
      Result: Tally table built from the eight reviews' own final disposition tables and landed verbatim in the review log's new "### Dispositions" section (see A1). R5.4 flips to aligned (unanimous non-abstaining ALIGN — privacy abstained — per D3). R8.8 stays needs-discussion (objected by the test lens only, on DJ3 (h), per D3). DF-R6.2 (with R6.2a) moves to ⌛️ Ready for Alignment (objected by six of eight lenses on DJ3's cases, but D72 settles the objections that bore on R6.2a's own text, per D3's reasoning, carried into the table as an explanatory note). Every row in the tally is accounted for in D3's change list; nothing flips without either a unanimous non-abstaining ALIGN or the owner's recorded authorization (D72).
