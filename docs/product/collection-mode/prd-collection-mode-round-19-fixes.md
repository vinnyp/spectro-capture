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

- [x] **A1 — Review log.** Append "## Round 19 — delta-verify of round 18 (2026-09-26)" after Round 18's closing line in `docs/agent-reviews/2026-09-24-prd-collection-mode-peer-reviews.md`. In order:
  - an intro: subject `591044a`, nine lenses, and the owner decision;
  - the nine reviews verbatim under "### <persona>", in round 18's order, each followed by a blank line;
  - "### Orchestrator verification", with C1 verbatim;
  - "### Editorial edits", listing the three editorial items above;
  - "### Dispositions", with E3's tally;
  - a closing line naming this file as the resume point and saying a micro round 20 follows.
  - Result: appended after line 28360 (Round 18's closing line, `Resume point: ...round-18-fixes.md`.) of `docs/agent-reviews/2026-09-24-prd-collection-mode-peer-reviews.md`; the new section now runs lines 28362–29468. The intro (subject `591044a`, amendment `6c78a9c..591044a`, same nine lenses as round 18, no extra lens, plus the owner decision "Fix + micro re-check" for SSE19-1) precedes all nine `round19/review-peer-<persona>-reviewer.md` files pasted verbatim by script and verified byte-for-byte identical to source before and after paste (product-manager, staff-software-engineer, test, interface, architecture, privacy, product-marketing, plan, database — round 18's order), each followed by a blank line (lines 28370–29436). Then `### Orchestrator verification` (C1's nested bullets verbatim, lines 29437–29446), `### Editorial edits` (the three items above, each marked editorial, lines 29447–29452), `### Dispositions` (E3's table plus D1's three bullets, lines 29453–29466), and the closing resume-point line naming this file and the micro round 20 (line 29468). No PII found in any of the nine source reviews (checked by regex for emails/phone numbers before pasting; none present).

## C. Verification notes (for A1)

- [x] **C1 — Orchestrator verification**, verbatim:
  - **Confirmed at `591044a`.** SSE19-1 and DB19-MAJOR-1 are the same defect, and grep confirms it.
    - DJ3 (i)'s Assert read the landed write back only "no sooner than 5 s after room is restored".
    - (d)'s rescoped (i) guard fired only "while the landed write's change still reads back".
    - So a run whose text leaves the log early while outside reads fail goes on to be judged, and it passes if the app recovers once room returns.
    - The database lens's `probe_i19.py` and `probe_i19u.py` show it on HFS+ and APFS: CANTOPEN while the volume is full, 400 rows read back after room returns, and SQLITE_CORRUPT after a crash in the window.
  - **Owner decision.** "Fix + micro re-check" (2026-09-26).
  - **Nothing was rejected.** Two items are recorded with no change, each with its reason.
  - Result: pasted verbatim into the review log's `### Orchestrator verification` subsection (`docs/agent-reviews/2026-09-24-prd-collection-mode-peer-reviews.md:29437–29446`), word for word as boxed above.

## D. Statuses

- [x] **D1 — Statuses (bookkeeping for round 19).**
  - **R8.8 stays needs-discussion.** Staff-engineer objected on DJ3 (i).
  - **DF R6.2 stays ⌛️ Ready for Alignment.** Staff-engineer and database objected on DJ3 (i).
  - **Neither row's text changed.** Both flip at micro round 20 if staff-engineer, test and database align. The other six lenses' round-19 ALIGNs stand, because the only change since is (i)'s Assert and guard, which none of them objected to.
  - Result: `git diff --stat 591044a 1aed29a` shows five files touched (excluding this fix file's own front matter, already part of `1aed29a`): `docs/product/collection-mode/prd-collection-mode-fences.md` (+2/-2), `docs/product/data-foundation/prd-data-foundation-fences.md` (+2/-2), `docs/product/data-foundation/prd-data-foundation-journeys.md` (+2/-2, DJ3 (d) and (i) rows), `docs/product/post-lock.md` (+10/-9), and `docs/product/collection-mode/prd-collection-mode-round-19-fixes.md` itself (98 insertions, this round's front matter) — 5 files changed, 114 insertions(+), 15 deletions(-) total, no PRD body among them. `git diff --name-only 591044a 1aed29a | grep -E "prd-collection-mode\.md|prd-data-foundation\.md"` returns nothing, so neither PRD body file is in the diff. R8.8's status cell (`prd-collection-mode.md:446`, "needs-discussion") and DF R6.2's status cell (`prd-data-foundation.md:216`, "⌛️ Ready for Alignment") were read directly in the untouched body files and are unchanged. Every change this round lands in the "Already landed" section above: the DJ3 (i) Major (its When, Assert and (d)'s guard clause at `prd-data-foundation-journeys.md:51,56`); the two Editorial edits under CM F218 and DF F62 (`prd-collection-mode-fences.md:1882,2109`, `prd-data-foundation-fences.md:695,699`); and the post-lock.md items (the E35 owner item, the DJ3 runs item, the harness item, the first-build-PR pointer, the main-file-wipe item, the ADR-0003 items, the ADR-0005 note, and the help-docs and v1-release items) listed under "Already landed" above.

## E. Checks

- [x] **E1 — Word counts.** Collection Mode must be ≤ 12,400 and Data Foundation ≤ 8,450.
  - Result: ran the specified `perl -0pe 's/<!--.*?-->//gs; s/```.*?```//gs; s/\]\([^)]*\)/]/g; s/^\|[-: |]+\|\s*$//mg; s/\|/ /g' FILE | wc -w` against both body files at HEAD `1aed29a`. Collection Mode: 12,397 of 12,400 — PASS. Data Foundation: 8,448 of 8,450 — PASS. Both match the counts stated in this file's front-matter budget line exactly; no word was added to either body this round.
- [x] **E2 — Mechanical checks.** Run checks 1, 2, 6, 7, 10, 11, 12, 13, 15, 16, 17 and 18 read-only, and report PASS or MISS for each. Check 18 covers DJ3 (i) and (d) as changed. Check 2 reads each fence's Carried-by with its Clarified lines.
  - Result (run against both PRDs' bodies/companions at HEAD `1aed29a`, tree clean apart from this fix file and the review-log append):
    - **Check 1 (cross-PRD consistency)** — PASS. `git diff 591044a 1aed29a -- docs | grep '^+' | grep -oE '\[[^]]*\]\([^)]*\.md[^)]*\)'` returns four distinct links touched this round ("Collection Mode R8.1c", "Collection Mode R8.1f/g write", "the Collection Mode PRD's F215 and F218", "the Collection Mode PRD's F218"), each naming its owning document in the visible label, and each anchor (`#8-operating-envelope-and-quality-attributes`) resolves to a real CM PRD heading.
    - **Check 2 (index sync)** — PASS on re-read, carried forward from round 17/18's own adjudication. F218's **Carried by** line (`prd-collection-mode-fences.md:1876`) still reads "(g) and (i)"; this round's new Clarified line at `:1882` extends "(d)'s guards for (g), (i) and (j) carry it with them", and the **Fence → row map** line (`:2109`) now reads "(d), (g), (i) and (j)". Per the standing round-17/18 adjudication (the map is read together with Carried-by's Clarified lines), this is a re-read PASS, not a fresh MISS. DF F62's **Rows:** cell (`prd-data-foundation-fences.md:693`) cites DJ3 as a whole (no per-letter list), so no analogous gap there.
    - **Check 6 (case refs/copy-state coverage)** — PASS (N/A this round). DJ3's lettered (a)–(j) sub-fixtures are prose inside one cell rather than `Case | Given | When | Assert` rows, so the check's literal ID extraction has nothing to range over there, as rounds 17–18 noted; no CM journeys `T`/`UJ` row changed this round, so no dangling reference was introduced.
    - **Check 7 (Legend constants named in an owning row)** — PASS (N/A this round). The Legend/constants table is untouched; no CM constant changed.
    - **Check 10 (two-sentence scan)** — PASS (N/A this round). No PRD-body requirement cell changed; this round's diff is fences/journeys/post-lock only.
    - **Check 11 (companion paths/link resolution)** — PASS. Both PRDs' Companions lines are unchanged (`prd-collection-mode.md:5`, `prd-data-foundation.md:5`); all four links found by check 1's grep resolve to the same already-passing targets as before.
    - **Check 12 (word count)** — PASS, see E1: CM 12,397/12,400, DF 8,448/8,450.
    - **Check 13 (label check)** — PASS. `git diff 591044a 1aed29a -- docs/product/collection-mode/prd-collection-mode-fences.md docs/product/data-foundation/prd-data-foundation-fences.md docs/product/data-foundation/prd-data-foundation-journeys.md | grep '^+' | grep -oE '"[^"]*"'` returns only `"Set a field"`, a pre-existing action label already homed in the copy companion (as round 18 also found); no new user-facing copy string was introduced this round.
    - **Check 15 (unresolved fill)** — PASS. 0 hits for `{{`, `_(…)_` or `<prd-slug>` across both PRDs and all eight companions.
    - **Check 16 (guidance comments)** — PASS. 0 `<!-- guidance` hits in the PRDs/companions; the only tree hits are literal quotes of this check's own name inside prior round-fix bookkeeping files (rounds 10/17/18), not guidance comments themselves.
    - **Check 17 (banned adjectives in asserts)** — PASS (N/A this round for DJ3; PASS for CM journeys). DF journeys: DJ3 carries no `T`/`UJ`-format case rows, so the check's literal extraction is empty there, as in rounds 17–18. CM journeys: no `UJ` row changed this round, so nothing new to re-check; round 18's clean word-boundary result stands.
    - **Check 18 (test-controls map/asserted-value tracing)** — PASS over DJ3 (i) and (d), this round's changed cells. Direction 1 (surfaces): (i)'s new When clause ("take an SQL read at SQLITE_READER_FLOOR through an outside read-only connection") drives a surface already declared on the CM Test-controls map's "the file" row ("a read transaction held open on it from outside the app through a read-only connection, having read a page (R8.10a)", `prd-collection-mode-journeys.md:587`) — nothing new to add. Direction 2 (datum tracing): re-running the check's `$datum` pattern against (d)'s and (i)'s changed Assert cells (`prd-data-foundation-journeys.md:51,56`) finds nothing — only quantities (1 s/5 s/10 s/1 MB), which the check deliberately excludes — so the direction is vacuously clean over both changed rows.
    - **No MISS is attributable to an orchestrator uncommitted edit.** Check 2's re-read is a carry-forward of round 17/18's own adjudication, not a fresh finding, and no other check reports a MISS this round.
- [x] **E3 — The disposition tally for A1.**

  | Row | PM | SSE | TR | IF | ARCH | PRIV | PMM | R19 | DB | Resulting status |
  |---|---|---|---|---|---|---|---|---|---|---|
  | R8.8 | ALIGN | OBJECT | ALIGN | ALIGN | ALIGN | ALIGN | ALIGN | ALIGN | ALIGN | needs-discussion |
  | DF-R6.2 (with R6.2a) | ALIGN | OBJECT | ALIGN | ALIGN | ALIGN | ALIGN | ALIGN | ALIGN | OBJECT | ⌛️ Ready for Alignment |
  - Result: this table was copied verbatim into the review log's `### Dispositions` subsection, alongside D1's three bullets restating the "stays" conclusions (`docs/agent-reviews/2026-09-24-prd-collection-mode-peer-reviews.md:29453–29466`). Already ticked; no further action.
