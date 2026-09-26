# Collection Mode PRD — round 17 fixes (the delta-verify of round 16, 2026-09-26)

This file is the resume point for the fix pass that follows review round 17, whose subject commit is `f3ca93c`.

- **Who reviewed.** Nine lenses. Their reviews sit in the orchestrator's scratchpad as `round17/review-<persona>.md`.
- **No owner decision this round.**
- **Budgets.**
  - Collection Mode stands at 12,397 of 12,400.
  - Data Foundation stands at 8,448 of 8,450.
  - No word may be added to either body. Neither body, nor the ADR-0003 input, changed this round.

**Already landed by the orchestrator.** Each item below was verified against F211, F212, F215, F216, F218, F61, F62 and D72. Do not redo any of them.

- **DJ3 (i) — log length and edit guard** (the Major: SSE17-1, TR17-1, DB17-MAJOR-1, ARCH17-5 and the plan lens's (i) Minor).
  - (i)'s Given now reads "…and the log holding no space past its last frame once that write lands, so that a later edit's frame needs new space".
  - (d) now reports "a run of (i) … whose edit lands rather than failing" as not exercised.
  - The reverse gap that TR17-1 names, a build that refuses every edit without trying, goes to post-lock as a positive DJ3 case, next to the item already carried there.
- **DJ3 (i) — the rest.**
  - The declared-full state is stated inline (an interface Nit).
  - The edit is made "once a look finds the file's own bytes clean, and no sooner than 5 s after read 1's end" (SSE17-m2).
  - "E15 renders within 1 s (R1.10)" (IF17-m4).
  - The state cell reads "E35 / E15" (IF17-m4, PMM17-4).
  - "The edit never shown disabled when it is made" (a test Nit).
  - "After room is restored the landed write's change reads back by an SQL read at SQLITE_READER_FLOOR" (a database Minor).
- **DJ3's hold clause, in (a) and each restated fixture.** It now reads "held only once its transaction has begun writing, holding the file's write lock (a BEGIN IMMEDIATE tried once from outside while it is held returns busy), and before read 1 ends".
  - (d) reports a run "in which a BEGIN IMMEDIATE tried from outside while a write is held does not return busy" as not exercised.
  - This answers ARCH17-1, IF17-m5, SSE17-m3, test m3 and the plan lens's hold Minor.
- **DJ3 (e).** Its second write is "held likewise only once its transaction holds the file's write lock". (d) reports "a run of (e) whose first byte read after read 2 ends finds the removed text beside the file nowhere" as not exercised (test m4).
- **DJ3 (g).**
  - The declared-full state reads "the volume reports no free space to the app and a write that needs new space fails while a truncation, an in-place overwrite or a sync of the file succeeds" (R17-3, test m9, the plan lens's free-space Minor, and a database Minor).
  - Its Result gains the one-way E35 check between E15 and the clearing (an interface Nit).
- **DJ3 (j).** The declared-full state fails "a truncation of any file the app keeps beside the file, or a sync of the file" (SSE17-m1, test m1, a database Minor).
- **DJ3 (d)'s other guards.**
  - A run of (c) or (g) whose last byte read before the crash or the release finds the removed text beside the file nowhere (the plan lens's guard Minor).
  - A run of (g) or (j) whose released write lands rather than failing (test m2, a database Minor).
- **DJ3's looks.** Each look "lists every state up" (test m6). The Collection Mode map row reads "Which sibling states are up, what each names" (test m6).
- **DJ3 (h).** "Once the file's opening state (R7.6b) is up" (an interface Nit carried from round 16).
- **Collection Mode journeys.**
  - UJ2.3-d's SQL read covers "Swatch field, imported and measurement values" (test m7, an interface Nit).
  - UJ5.4-a and UJ5.4-b read "…ΔE2000 2.04, with no label" (an interface Nit carried from round 16; F216).
- **Fences.** Clarified lines only; no fence body was touched.
  - A round-17 bookkeeping line under Collection Mode F218 and under DF F62 says DJ3 (j) also carries the fence. (g) is now the full volume that lets the clearing run, and (i) and (j) are the clearings a full volume stops. This answers IF17-m3, PMM17-6, SSE and plan Nits, test and architecture Nits (ARCH17-8), and an interface Nit on DF F62's Decision.
  - F218's fence → row map line reads "DJ3 (g), (i) and (j)".
  - Under DF F62, a mirror of F218's third Clarified line (PRIV17-2, ARCH17-9, an interface Nit) and a mirror of the "room enough for that clearing to succeed" gloss (ARCH17-9, an interface Nit).
  - DF F61's two round-16 lines move above the round-15 amendment heading (R17-5, IF17-m1, PMM17-5, ARCH17-7, test, database and plan Nits).
- **`post-lock.md`**
  - **:46.** Reads "E35 staying up if it was … (DJ3 (i), (j); (g) is a full volume that lets the clearing run)" (PMM17-1, IF17-m2, plan and SSE Minors, ARCH17-10).
  - **Dogfood E35 item.**
    - The third window now reads (g) at most 5 s, and until room returns in (i) after an edit and in (j) (PMM17-2, SSE, plan and privacy Nits).
    - The fifth window moves to a needs-owner copy-choice item under Data Foundation (R17-6). The move adds window 5's "until the user's next change" (PMM17-2, a privacy Nit), the ‹no room› body suggestion (PMM17-2), ARCH17-4's APFS evidence, and "gating neither the build nor the re-lock" (a plan Nit).
  - **The help-docs line** reads "…once there's room and no other app is reading it, while the file is open or the next time you open it" (R17-2, PMM17-3, SSE Nit, ARCH17-10, privacy and database Nits).
  - **Needs owner.** A main-file wipe on clone-shared APFS blocks (PRIV17-1, the plan lens's ARCH16-4 Minor).
  - **ADR-0003 items:**
    - the IF17-m6 readings;
    - DB IOERR_FSYNC-at-commit and F_FULLFSYNC-after-a-failed-growth-checkpoint (with ARCH17-3);
    - ARCH17-2's PASSIVE-then-TRUNCATE retry and no statfs gate, plus ARCH17-4's probe evidence;
    - ARCH17-6's ADR-0005 cross-listing;
    - the round-15 hook note now names both declared-full states and the free-space query (SSE17-m4, and the plan lens's PL:170 Minor);
    - the round-11 note is qualified by "once that write holds SQLite's write lock" (a test Nit);
    - the round-12 rollback-journal note is moot if ADR-0003 records that WAL is forced (an architecture over-engineering note).
  - **Next pass.**
    - The positive DJ3 case (SSE17-1 and TR17-1's reverse gap), with the round-16 Nits SSE carried.
    - A new DJ3 item: room returning while a read still uses the log (test m5), a guard for the R1.11 full-volume row (test m8 and the plan lens's DFJ:44 Minor), and "lift the declared-full state" (a test Nit).
    - A copy-pass item for E8 (PRIV17-3).
    - The Collection Mode test-controls map's "the file" row (a test Nit).
  - A stray blank line inside the Collection Mode list is removed.

**Recorded with no change.**

- **Database Nit: (i)'s one-way clause.** "Within 5 s of a byte read first finding it beside the file nowhere, E35 is not up" cannot be reached before room is restored now that (d) guards that run. It is kept because every DJ3 row uses the same one-way form.
- **Architecture Nit: the stacked pointer lines.** R8.8 already delegates to DF R6.2a, which, with the ADR-0003 input, is the one consolidated statement. The fences are the decision record, not the build statement.
- **SSE Nit on round 16's check-18 statement.** The Nit is acknowledged. Round 17's E2 below states check 18 against the new Given, When and Assert cells.

## A. The record

- [x] **A1 — Review log.** Append "## Round 17 — delta-verify of round 16 (2026-09-26)" after Round 16's closing line in `docs/agent-reviews/2026-09-24-prd-collection-mode-peer-reviews.md`. Add, in order:
  - an intro: subject `f3ca93c`, nine lenses, and no extra lens beyond round 16's set, since the tier triggers have not changed;
  - the nine reviews verbatim under "### <persona>", in this order: product-manager, staff-software-engineer, test, interface, architecture, privacy, product-marketing, plan, database. Leave a blank line after each;
  - "### Orchestrator verification", holding C1 verbatim;
  - "### Editorial edits", listing the map line, the R7.6b cite, the DF F61 line move, the stray blank line and the post-lock wording, each as editorial;
  - "### Dispositions", holding E3's tally;
  - a closing line naming this file as the resume point.
  - Result: appended after line 26192 (Round 16's closing line) of `docs/agent-reviews/2026-09-24-prd-collection-mode-peer-reviews.md`; the intro (subject `f3ca93c`, nine lenses, same set as round 16); all nine `round17/review-<persona>.md` files pasted verbatim, byte-for-byte diffed against source (product-manager, staff-software-engineer, test, interface, architecture, privacy, product-marketing, plan, database), each followed by a blank line; then `### Orchestrator verification` (C1's nested bullets verbatim), `### Editorial edits` (the F218 map line, the DJ3 (h) R7.6b cite, the DF F61 line move, the stray post-lock blank line, and post-lock wording, each named editorial), `### Dispositions` (E3's table plus the two disposition bullets, matching round 16's own shape), and the closing resume-point line. File now runs to line 27295.

## C. Verification notes (for A1)

- [x] **C1 — Orchestrator verification**, verbatim:
  - **Confirmed at `f3ca93c`:**
    - DJ3 (i)'s new edit assertion, "for the edit E15 renders within 1 s", fails a correct build whenever the log keeps space past its last frame, and (d) had no guard for an edit that lands. Grep confirms (i)'s Given pinned no log length. Three probes reproduce it: the test lens's `probe_i17_slack.py` (the edit landed on a full volume) and the staff-engineer and database lenses' own probes. (SSE17-1, TR17-1, DB17-MAJOR-1.)
    - The hold clause "begun writing to the file" cannot be satisfied or observed in WAL mode. The architecture lens's `probe17_g.py` variant B shows a hold placed before SQLite's write lock restarting the log over the seeded frame. (ARCH17-1, IF17-m5, SSE17-m3, test m3.)
  - **No owner decision.** Every fix follows F215, F218 and its Clarified lines, D72's not-chosen "Also stop new writes", and R8.10a's test seam.
  - **Nothing was rejected.** Three Nits were recorded with no change, each with its reason.
  - Result: pasted verbatim into the review log's `### Orchestrator verification` subsection, word for word as boxed above.

## D. Statuses

- [x] **D1 — Statuses (bookkeeping for round 17).**
  - **R8.8 stays needs-discussion.** The test lens objected on DJ3 (i) only, and the row's text did not change.
  - **DF R6.2 stays ⌛️ Ready for Alignment.** The staff-engineer, test and database lenses objected on DJ3 (i). R6.2a's text did not change.
  - List every change, and confirm that neither row's status cell changed.
  - Result: `git diff --stat` against the worktree shows five files touched, all fences/journeys/post-lock, no PRD body: `prd-collection-mode-fences.md` (+4/-1), `prd-collection-mode-journeys.md` (+8/-8), `prd-data-foundation-fences.md` (+14/-4), `prd-data-foundation-journeys.md` (+20/-20), `post-lock.md` (+22/-7) — 42 insertions, 26 deletions, 5 files changed. `git diff --name-only` returns neither `prd-collection-mode.md` nor `prd-data-foundation.md`; both PRD body files are absent from the diff entirely, confirmed by `git diff --name-only | grep -E "prd-collection-mode\.md|prd-data-foundation\.md"` returning nothing. R8.8's status cell (CM PRD:446, "needs-discussion") and DF R6.2's status cell (DF PRD:216, "⌛️ Ready for Alignment") are therefore both unchanged this round — confirmed by reading each cell directly in the untouched body files. Every change this round lands in the "Already landed" section above: DF R6.2a's own body text (DF PRD:228, a separate row from R6.2) likewise did not change this round (last touched in round 16). All touched content is DJ3 (g)/(h)/(i)/(j) and their shared fixture in DF journeys; F218's Clarified lines, its fence → row map line, and DJ3 (h)'s R7.6b cite in CM fences/journeys is untouched apart from the one map-line edit; DF F61's two round-16 Clarified lines relocated and F62's new Clarified lines in DF fences; and the post-lock items listed under "Already landed" (:46, the dogfood E35 item, the help-docs line, the needs-owner item, the ADR-0003 items, the next-pass items, and the stray blank line) in post-lock.md.

## E. Checks

- [x] **E1 — Word counts.** Collection Mode must be ≤ 12,400 and Data Foundation ≤ 8,450, by process rule 14's command.
  - Result: ran the specified `perl -0pe 's/<!--.*?-->//gs; s/```.*?```//gs; s/\]\([^)]*\)/]/g; s/^\|[-: |]+\|\s*$//mg; s/\|/ /g' FILE | wc -w` against the current worktree body files. Collection Mode: 12,397 of 12,400 — PASS. Data Foundation: 8,448 of 8,450 — PASS. Both match the counts the fix file's front matter states; no word was added to either body this round.
- [x] **E2 — Mechanical checks.** Run checks 1, 2, 6, 7, 10, 11, 12, 13, 15, 16, 17 and 18 read-only, and report PASS or MISS for each. Check 18 runs against every Given, When and Assert cell this round changed:
  - DJ3 (a)–(j);
  - UJ2.3-d;
  - UJ5.4-a and UJ5.4-b;
  - the Collection Mode map row.

  Tracing its assert values (the 1 s, 5 s and 10 s functional timeouts, SQLITE_READER_FLOOR, BEGIN IMMEDIATE, ΔE2000 2.04) to their owners.
  - Result (run against both PRDs' bodies/companions in the worktree as it now stands, HEAD `f3ca93c` plus the uncommitted round-17 edits):
    - **Check 1 (cross-PRD consistency)** — PASS. CM F218 and DF F62 cite each other by document name in both directions and resolve; every DJ3 (a)–(j) cite of "Collection Mode R8.1f/g", "Collection Mode R8.10a" and "Collection Mode R8.1c" names its owning document; F218's and F62's Clarified lines that cite each other resolve to the right fence.
    - **Check 2 (index sync)** — **MISS.** `docs/product/collection-mode/prd-collection-mode-fences.md:1876`, F218's own **Carried by:** line, still reads "the Data Foundation PRD DJ3 (g) and (i)" — it was not touched this round. But the **Fence → row map** line for the same fence, `docs/product/collection-mode/prd-collection-mode-fences.md:2109`, was edited this round to read "the Data Foundation PRD DJ3 (g), (i) and (j)". The two disagree on whether DJ3 (j) carries F218. DF F62's own "Rows:" line (`prd-data-foundation-fences.md:693`) and its "Fence → row map" line (`:262`) both cite DJ3 as a whole (no per-letter breakdown), so DF's side of this fence has no equivalent gap. Not fixed here (D1/E2 are read-only); flagged for the next Collection Mode editing pass to add "(j)" to F218's Carried-by line.
    - **Orchestrator adjudication of check 2 — PASS on re-read, no edit.** F218's Carried-by line is fence text, which is never rewritten. The round-17 Clarified line under F218 (`prd-collection-mode-fences.md:1882`) extends it to DJ3 (j), and the fence → row map tracks Carried-by read with its Clarified lines. So the map line at :2109 and the fence agree, and the MISS is the check's literal read of the original field alone.
    - **Check 6 (case refs/copy-state coverage)** — PASS, with a documented caveat. CM's own T-/UJ-numbered cases resolve to live rows and every DF copy-companion `E<n>` appears in at least one DF case. DF's DJ3 table uses "Initial state / Action / Result oracle / State" columns rather than the agent-PRD template's `Case | Given | When | Assert | Rows` shape (DJ3's lettered (a)–(j) sub-fixtures are prose inside one cell, not separate case rows), so the check's literal `T[0-9]+|UJ[0-9]+\.[0-9]+-[a-z]` extraction finds nothing to range over there; traced by hand instead against each lettered fixture and its own not-exercised guard in (d) — no dangling reference found.
    - **Check 7 (Legend constants named in an owning row)** — PASS. All 12 CM constants (`prd-collection-mode.md:183-194`) forward-resolve into a requirement row and each row's Owning-rows cell reverse-resolves; the constants table is untouched this round.
    - **Check 10 (two-sentence scan)** — PASS (N/A this round). No PRD-body requirement cell changed; the scan's subject (dispositionable table cells in the two PRD bodies) is unaffected by a fences/journeys/post-lock-only round.
    - **Check 11 (companion paths/link resolution)** — PASS. Both PRDs' Companions lines are unchanged and all four companions resolve; the one pre-existing cross-file link touched by this round's context, DJ3 (i)/(j)'s "[the Collection Mode PRD's F215 and F218](../collection-mode/prd-collection-mode-fences.md)", resolves to an existing file.
    - **Check 12 (word count)** — PASS, see E1: CM 12,397/12,400, DF 8,448/8,450.
    - **Check 13 (label check)** — PASS. `git diff | grep -E '^\+' | grep -oE '"[^"]*"'` over the touched fences/journeys files surfaces only pre-existing labels already homed in the copy companion ("Columns", "Compare", "Set a field", "Show history") or self-citations of a fence's own Decision text ("E35 stays up", "room to clear it"); no new user-facing copy string was introduced this round, matching the fix file's front matter.
    - **Check 15 (unresolved fill)** — PASS. 0 hits for `{{`, `_(…)_` or `<prd-slug>` across both PRDs and all eight companions.
    - **Check 16 (guidance comments)** — PASS. 0 `<!-- guidance` hits.
    - **Check 17 (banned adjectives in asserts)** — PASS. DF journeys: 0 hits. CM journeys: one naive grep hit on "correction" inside a Test-controls-map cross-reference ("ZX-013's current reading awaiting its correction answer") is a substring false positive of "correct", the same false-positive class round 16 noted; a word-boundary re-run is clean.
    - **Check 18 (test-controls map/asserted-value tracing)** — PASS over the round's touched cells. Direction 1 (surfaces): DJ3's driven surfaces (declaring the volume full, releasing a held write, an SQL read at SQLITE_READER_FLOOR, a single-item edit) all land on the CM Test-controls map's "the file" and "collection surface" rows, already declared; the "sibling states opened here" row's edited text, "Which sibling states are up, what each names" (`prd-collection-mode-journeys.md:589`), is what DJ3's E35/E15 cites resolve through. Direction 2 (datum tracing): DJ3 (a)–(j)'s Asserts carry no URL, identifier or instant token under the check's `$datum` pattern — only quantities (1 s/5 s/10 s/1 MB), which the check deliberately excludes — so there is nothing to trace and the direction is vacuously clean; UJ2.3-d's new "Swatch field, imported and measurement values" clause is supplied by its own Given (the two imported columns it declares); UJ5.4-a and UJ5.4-b's "with no label" traces to the Test-controls map's accessibility row, already declared and unchanged.
    - **No MISS is attributable to an orchestrator uncommitted edit** other than check 2's, which is itself an uncommitted-edit inconsistency (the map line was updated, the Carried-by line was not) and is reported above rather than fixed. The orchestrator's adjudication above resolves it.
- [x] **E3 — The disposition tally for A1.**

  | Row | PM | SSE | TR | IF | ARCH | PRIV | PMM | R17 | DB | Resulting status |
  |---|---|---|---|---|---|---|---|---|---|---|
  | R8.8 | ALIGN | ALIGN | OBJECT | ALIGN | ALIGN | ALIGN | ALIGN | ALIGN | ALIGN | needs-discussion |
  | DF-R6.2 (with R6.2a) | ALIGN | OBJECT | OBJECT | ALIGN | ALIGN | ALIGN | ALIGN | ALIGN | OBJECT | ⌛️ Ready for Alignment |
  - Result: this table is already in the file (above) and was copied verbatim into the review log's `### Dispositions` subsection, alongside the two "stays" bullets restating D1's conclusions. Already ticked; no further action.
