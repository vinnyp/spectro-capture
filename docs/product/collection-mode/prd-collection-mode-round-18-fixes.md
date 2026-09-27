# Collection Mode PRD — round 18 fixes (the delta-verify of round 17, 2026-09-26)

This file is the resume point for the fix pass after review round 18, subject commit `6c78a9c`.

- **Who reviewed.** Nine lenses. Their reviews are in the orchestrator's scratchpad as `round18/review-<persona>.md`.
- **No owner decision this round.**
- **Budgets.** Collection Mode stands at 12,397 of 12,400 and Data Foundation at 8,448 of 8,450. Neither body, nor the ADR-0003 input, changed.

**Already landed by the orchestrator.** Every item was verified against F211, F212, F215, F216, F218, F61, F62 and D72. Do not redo any of them.

- **DJ3's hold check (the Major).** This answers TR18-1, IF18-m1, M1 (plan), ARCH18-1, ARCH18-2, SSE18-m1 and the database lens's connection Minor.
  - The hold clause now reads: "held only once its transaction holds the file's write lock (a BEGIN IMMEDIATE tried once while it is held, with no busy timeout, through an outside read-write connection of the harness's own, rolled back and closed at once, returns busy), and before read 1 ends".
    - It names the connection and its lifetime.
    - It drops the unobservable "has begun writing" (a database Nit).
  - (e)'s second write is "checked the same way" (an SSE Nit).
  - (d) gains "a run of (c) whose byte read after the crash, before the reopen, finds it beside the file nowhere".
  - The Collection Mode Test-controls map's "the file" row declares the outside read-write BEGIN IMMEDIATE.
- **DJ3 (d)'s other guards.**
  - (a) joins the before-release guard (TR18-m1).
  - A run of (g) in which a declared-full truncation of a scratch file beside the file fails (the database lens's (g) Minor: stock APFS).
  - (i)'s early-clearing guard applies only "while the landed write's change still reads back". A run that loses the change goes on to fail (i) (TR18-m2).
  - "Whose edit lands" is observed "as an SQL read at SQLITE_READER_FLOOR taken before room is restored shows" (TR18-m3).
- **DJ3's declared-full states.**
  - (g) and (i) read "the volume reports no free space to the app and a write that needs new space fails, while every other operation on the file or on any file the app keeps beside it, a truncation, an in-place overwrite or a sync, succeeds". This answers TR18-m4, IF N1, R18-7, ARCH18-4, ARCH18-5, SSE18-m2, M2 and a database Nit.
  - (j) reads "the volume reports no free space to the app, a write that needs new space fails, and a truncation of any file the app keeps beside the file, or a sync of the file or of any file beside it, also fails until room returns".
- **DJ3 (i).**
  - "As the log truncated to zero before the seeding gives" names the recipe for the pin (test and SSE Nits).
  - The read-back is taken "no sooner than 5 s after room is restored" (SSE18-m3, a test Nit).
- **DJ3 (g) and (j).** "E15 renders for the failed write (R1.10)" (IF N2).
- **Collection Mode journeys.**
  - UJ5.4-a and UJ5.4-b read "…with no label, read through the Test-controls map's accessibility row" (IF N3).
  - The map row reads "which of each one's actions" (IF N4).
- **Fences.** Clarified lines only.
  - DF F62's E35 mirror now cites F218's Clarified line "from the round-16 privacy review's PRIV16-2 and PRIV16-3" rather than "its third" (IF18-m2, PMM18-6, PRIV18-2, ARCH18-3, R18-4, and SSE and database Nits).
  - DF F62's room gloss names the moment in full: "the first moment after room returns that the file is open, no read uses the log and no write runs" (an SSE Nit, a plan Nit).
  - The round-17 (j) bookkeeping lines under F218 and DF F62 read "refuses a truncation or a sync" (a database Minor: real APFS refuses only the truncation).
- **`post-lock.md`.**
  - **The E35 owner item** is posed over headline and body, with three options: (a) a no-room sibling, (b) cause-neutral, (c) as is. It names the rows (R7.6q, R6.2a) and the word cost, and gives the reopen all-clear effect. Its sample body is PMM18-1's, and the APFS evidence is reworded. This answers R18-1, R18-2, R18-5, PMM18-1, PMM18-2, PMM18-7, PRIV18-1 and PRIV18-4.
  - **The positive DJ3 case** lands before the first build PR that runs DJ3 (i), and its edit removes no text or asserts its wipe (M4, SSE18 question 5, PRIV18-3).
  - **The DJ3 runs item** also lands before the first build PR that runs DJ3. It adds:
    - M5's no-outside-read run;
    - R18-2's close-and-reopen run;
    - R18-3's move run;
    - the plan lens's time-bound Nit;
    - the privacy lens's (j) zero-in-place Nit.
  - **:49** borrows the help-docs wording (a privacy Nit).
  - **The main-file wipe owner item** is widened to any main-file wipe whose sync a full volume refuses. It is decided before ADR-0003 is accepted, and it names the help-docs line and E35's body as following it (PRIV18-3, M3, M4, PMM18-4).
  - **The ADR-0003 items gain:**
    - F_PREALLOCATE before a growth retry (an architecture Missing item);
    - (i)'s page-size note (a database Nit);
    - the round-11 note's "and then lands" (ARCH18-6);
    - the hook's (j) state reading "a truncation of any file beside the file, or a sync" (a database Minor).
  - **Window 3** reads "a write that failed for want of room" (PMM18-8).
  - **The help-docs line** reads "If there's no room left where your file is … until you free up space …". It adds "Leave the file beside yours where it is: it can also hold changes not yet in your file." Its "in your file" half follows the owner item (PMM18-3, PMM18-4, PMM18-5, IF N5, R18-6, M3).
  - **A new v1 release item:** what tells the user about full-disk retention (PRIV18-1).
  - **A new "## ADR-0005" group** carries the poll timer and the activity assertion (ARCH17-6).

**Recorded with no change.**

- **SSE18-m1's claim** that a read-only BEGIN IMMEDIATE returns SQLITE_READONLY does not reproduce. It returns OK (the orchestrator's probe, and the test, architecture, database and plan lenses' probes). The fix is the same either way.
- **The database lens's view** that only (g) needs the no-free-space clause. The rows follow the majority and PL's hook note instead, so there is one declared-full vocabulary. The database lens itself says no verdict changes.
- **F218's round-16 Clarified line (CMF:1884)** names "the file open and no read using the log" but not "no write runs". R6.2a (DF:228) states all three conditions, and the DF mirror now spells them out.

## A. The record

- [x] **A1 — Review log.** Append "## Round 18 — delta-verify of round 17 (2026-09-26)" after Round 17's closing line in `docs/agent-reviews/2026-09-24-prd-collection-mode-peer-reviews.md`, in order:
  - an intro: subject `6c78a9c`, the same nine lenses, no extra lens;
  - the nine reviews verbatim under "### <persona>", in round 17's order, each followed by a blank line;
  - "### Orchestrator verification", with C1 verbatim;
  - "### Editorial edits", listing each editorial item:
    - the R1.10 cites;
    - the ordinal-to-source cite;
    - "which of each one's actions";
    - the "refuses a truncation or a sync" gloss;
  - "### Dispositions", with E3's tally;
  - a closing line naming this file as the resume point.
  - Result: appended after line 27295 (Round 17's closing line) of `docs/agent-reviews/2026-09-24-prd-collection-mode-peer-reviews.md`; the intro (subject `6c78a9c`, amendment `f3ca93c..6c78a9c`, same nine lenses as round 17, no extra lens); all nine `round18/review-peer-<persona>-reviewer.md` files pasted verbatim, byte-for-byte diffed against source (product-manager, staff-software-engineer, test, interface, architecture, privacy, product-marketing, plan, database), each followed by a blank line; then `### Orchestrator verification` (C1's nested bullets verbatim), `### Editorial edits` (the R1.10 cites added to DJ3 (g) and (j)'s Assert cells, DF F62's ordinal-to-source cite at `prd-data-foundation-fences.md:701`, the Collection Mode Test-controls map's "which of each one's actions" wording at `prd-collection-mode-journeys.md:585`, and the "refuses a truncation or a sync" gloss under F218 and DF F62, each named editorial), `### Dispositions` (E3's table plus the two "stays" bullets from D1), and the closing resume-point line. File now runs to line 28360. No PII found in any of the nine source reviews (checked by regex for emails/phone numbers before pasting).

## C. Verification notes (for A1)

- [x] **C1 — Orchestrator verification**, verbatim:
  - **Confirmed at `6c78a9c`:**
    - DJ3's hold check names no connection (grep: the parenthetical in (a) and each restated fixture). The only outside connection that DJ3 and the Test-controls map declare is read-only.
    - The orchestrator's probe (SQLite 3.53.4) shows a read-only connection's BEGIN IMMEDIATE returning OK while the app holds the write lock; a read-write connection's returns "database is locked".
    - The test lens's `probe_close18.py` and the database lens's `probe_checkconn18b.py` show a read-write check connection left open across (c)'s crash deleting the log as the last connection, which lets a build that never clears after a crash pass (c) (TR18-1, with IF18-m1, M1, ARCH18-1/2, SSE18-m1 and a database Minor).
  - **No owner decision.** Every fix follows F215, F218 and its Clarified lines, D72 and R8.10a's test seam.
  - **Nothing was rejected.** Three items are recorded with no change, each with its reason.
  - Result: pasted verbatim into the review log's `### Orchestrator verification` subsection, word for word as boxed above.

## D. Statuses

- [x] **D1 — Statuses (bookkeeping for round 18).**
  - **R8.8 stays needs-discussion.** The test lens objected on DJ3's hold check only, and the row's text did not change.
  - **DF R6.2 stays ⌛️ Ready for Alignment,** for the same reason. R6.2a's text did not change.
  - List every change, and confirm that neither row's status cell changed.
  - Result: `git diff --stat` against the worktree shows six files touched: the review log itself (`docs/agent-reviews/2026-09-24-prd-collection-mode-peer-reviews.md`, +1065, this round's A1 append) plus five product docs — `prd-collection-mode-fences.md` (+1/-1), `prd-collection-mode-journeys.md` (+4/-4), `prd-data-foundation-fences.md` (+3/-3), `prd-data-foundation-journeys.md` (+10/-10), `post-lock.md` (+16/-9) — no PRD body among them. `git diff --name-only` lists exactly those six paths; `git diff --name-only | grep -E "prd-collection-mode\.md|prd-data-foundation\.md"` returns nothing, so neither PRD body file is in the diff. R8.8's status cell (`prd-collection-mode.md:446`, "needs-discussion") and DF R6.2's status cell (`prd-data-foundation.md:216`, "⌛️ Ready for Alignment") were read directly in the untouched body files and are unchanged from round 17. Every change this round lands in the "Already landed" section above: the hold clause and its guards in DJ3 (a)–(j) at `prd-data-foundation-journeys.md:48–57`; the Collection Mode journeys' UJ5.4-a/b accessibility-row cite and the Test-controls map's "which of each one's actions" wording at `prd-collection-mode-journeys.md:395–396, :585`; the Clarified-line-only edits under CM F218 and DF F62 at `prd-collection-mode-fences.md:1882` and `prd-data-foundation-fences.md:699–703` (fence bodies untouched); and the post-lock.md items (the E35 owner item, the positive DJ3 case, the DJ3 runs item, the help-docs line, the main-file wipe owner item, the ADR-0003 items, window 3's wording, the v1 release item and the new ADR-0005 group) listed under "Already landed" above. Neither PRD body, nor the ADR-0003 input (`docs/decisions/README.md`), changed, matching the fix file's front-matter budget line.

## E. Checks

- [x] **E1 — Word counts.** Collection Mode must be ≤ 12,400 and Data Foundation ≤ 8,450, by process rule 14's command.
  - Result: ran the specified `perl -0pe 's/<!--.*?-->//gs; s/```.*?```//gs; s/\]\([^)]*\)/]/g; s/^\|[-: |]+\|\s*$//mg; s/\|/ /g' FILE | wc -w` against the current worktree body files. Collection Mode: 12,397 of 12,400 — PASS. Data Foundation: 8,448 of 8,450 — PASS. Both match the counts the fix file's front matter states exactly; no word was added to either body this round.
- [x] **E2 — Mechanical checks.** Run checks 1, 2, 6, 7, 10, 11, 12, 13, 15, 16, 17 and 18 read-only, and report PASS or MISS for each.
  - **Check 18** runs against every Given, When and Assert cell this round changed: DJ3 (a)–(j), UJ5.4-a and UJ5.4-b, and the map rows. It confirms the outside read-write BEGIN IMMEDIATE is now on the map's "the file" row.
  - **Check 2** reads each fence's Carried-by together with its Clarified lines, per round 17's adjudication.
  - Result (run against both PRDs' bodies/companions in the worktree as it now stands, HEAD `6c78a9c` plus the uncommitted round-18 edits):
    - **Check 1 (cross-PRD consistency)** — PASS. Every cross-PRD markdown link added by this round's diff (`git diff | grep '^+' | grep -oE '\[[^]]*\]\([^)]*\.md[^)]*\)'` over the touched fences/journeys files) names its owning document in the visible label — "Collection Mode R8.10a", "Collection Mode R8.1c", "the Collection Mode PRD's F215 and F218" — and each anchor (`#8-operating-envelope-and-quality-attributes`) resolves to a real CM PRD heading (`prd-collection-mode.md:412`).
    - **Check 2 (index sync)** — unchanged from round 17's read. F218's own **Carried by:** line (`prd-collection-mode-fences.md:1876`) still reads "(g) and (i)" — this round touched no Carried-by or map line, only the Clarified lines' wording (the ordinal fix and the "or"/"and" fix). The **Fence → row map** line (`:2109`) still reads "(g), (i) and (j)", matching DF F62's own carriers (`prd-data-foundation-fences.md:699`). Per round 17's own adjudication (carried forward, no new edit needed): the map is read together with Carried-by's Clarified lines, so this is **PASS on re-read**, not a fresh MISS — the literal single-line parse still reports the same gap round 17 named and resolved, and nothing this round reopens it.
    - **Check 6 (case refs/copy-state coverage)** — PASS. DJ3's lettered (a)–(j) sub-fixtures are prose inside one cell rather than the template's `Case | Given | When | Assert` rows, so the check's literal ID extraction has nothing to range over there, as round 17 noted; traced by hand instead against each lettered fixture's own guard in (d) — no dangling reference found, and no `T`/`UJ` case this round cites a retired or nonexistent row.
    - **Check 7 (Legend constants named in an owning row)** — PASS (N/A this round). The Legend/constants table is untouched; no CM constant changed.
    - **Check 10 (two-sentence scan)** — PASS (N/A this round). No PRD-body requirement cell changed; the scan's subject is unaffected by a fences/journeys/post-lock-only round.
    - **Check 11 (companion paths/link resolution)** — PASS. Both PRDs' Companions lines are unchanged (`prd-collection-mode.md:5`, `prd-data-foundation.md:5`) and all companions resolve; `git diff | grep '^+' | grep -oE '\[[^]]*\]\([^)]*\)'` over the touched fences/journeys files returns no new markdown link this round.
    - **Check 12 (word count)** — PASS, see E1: CM 12,397/12,400, DF 8,448/8,450.
    - **Check 13 (label check)** — PASS. `git diff | grep '^+' | grep -oE '"[^"]*"'` over the touched fences/journeys files (excluding post-lock.md, which sits outside this check's five-file scope) surfaces only pre-existing labels already homed in the copy companion ("Compare", "Set a field", "Show history") or self-citations of a fence's own Decision text ("E35 stays up", "room to clear it"); no new user-facing copy string was introduced this round.
    - **Check 15 (unresolved fill)** — PASS. 0 hits for `{{`, `_(…)_` or `<prd-slug>` across both PRDs and all eight companions (correctly scoped to the ten product-doc files, excluding the round-fix bookkeeping files which legitimately quote these tokens as history).
    - **Check 16 (guidance comments)** — PASS. 0 `<!-- guidance` hits across both PRDs and all eight companions.
    - **Check 17 (banned adjectives in asserts)** — PASS. DF journeys: DJ3 carries no `T`/`UJ`-format case rows, so the check's literal extraction is empty there (as in round 17); CM journeys: one naive grep hit on "correction" inside a Test-controls-map cross-reference ("ZX-013's current reading awaiting its correction answer") is a substring false positive of "correct", the same false-positive class round 17 noted; a word-boundary re-run (`\bcorrect\b` etc.) is clean.
    - **Check 18 (test-controls map/asserted-value tracing)** — PASS over the round's touched cells. Direction 1 (surfaces): the new outside read-write BEGIN IMMEDIATE that DJ3's hold clause and each restated fixture add (`prd-data-foundation-journeys.md:48–57`) is now declared on the CM Test-controls map's "the file" row (`prd-collection-mode-journeys.md:587`, confirmed by diff: "…and a BEGIN IMMEDIATE tried on it once, with no busy timeout, through an outside read-write connection rolled back and closed at once, which checks a held write's lock (the Data Foundation PRD's DJ3)"), closing the gap the round-17 SSE18-m1/TR18-1/ARCH18-1-2 findings named. The map's "sibling states opened here" row's edited text, "which of each one's actions", is what the surfaces DJ3 already declares resolve through. Direction 2 (datum tracing): re-running the check's `$datum` pattern (`[a-z][a-z0-9+.-]*://…|…T[0-9:]+Z|[A-Za-z]+-[0-9]{3,}|[a-z]+[0-9]{3,}`) against DJ3 (a)–(j)'s changed Assert cells (`prd-data-foundation-journeys.md:48–57`) finds nothing — only quantities (1 s/5 s/10 s/1 MB), which the check deliberately excludes — so the direction is vacuously clean there; against UJ5.4-a/b's changed Assert cells it finds `HX-001`, which the same case's own When cell supplies ("Open HX-001, fire 'Show history'…"), so it traces cleanly, not a MISS.
    - **No MISS is attributable to an orchestrator uncommitted edit.** Check 2's re-read is a carry-forward of round 17's own adjudication, not a fresh finding, and no other check reports a MISS this round.
- [x] **E3 — The disposition tally for A1.**

  | Row | PM | SSE | TR | IF | ARCH | PRIV | PMM | R18 | DB | Resulting status |
  |---|---|---|---|---|---|---|---|---|---|---|
  | R8.8 | ALIGN | ALIGN | OBJECT | ALIGN | ALIGN | ALIGN | ALIGN | ALIGN | ALIGN | needs-discussion |
  | DF-R6.2 (with R6.2a) | ALIGN | ALIGN | OBJECT | ALIGN | ALIGN | ALIGN | ALIGN | ALIGN | ALIGN | ⌛️ Ready for Alignment |
  - Result: this table is already in the file (above) and was copied verbatim into the review log's `### Dispositions` subsection, alongside the two "stays" bullets restating D1's conclusions. Already ticked; no further action.
