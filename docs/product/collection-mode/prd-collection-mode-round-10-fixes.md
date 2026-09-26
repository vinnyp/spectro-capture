# Collection Mode PRD — round 10 fixes (the PR #21 review, 2026-09-26)

This is the resume point for the amendment that answers the PR #21 review of `04dfb97`. That review was two reviews with eight inline threads (T1–T8), relayed as peer-agent feedback. Its text is in the review log's "## Round 10" section, which box A1 writes.

- **What this is.** An amendment to the PRD locked 2026-09-25, under fences F202–F210.
  - The preservation baseline is `04dfb97`, the revision the lock record validated.
  - The change baseline is `dc1b747`, the merge-base with `origin/main`, re-checked 2026-09-26.
- **Authority.**
  - The fence file authorizes every change: F1–F201 as they stand, plus F202–F210, which box B1 writes from the owner's answers D55–D64.
  - Those answers are recorded verbatim at `scratchpad/r10/round10-adjudication.md`. B1 copies them into the log and the fences.
  - A box authorizes a status change only where its fence records the decision.
- **Meaning check.** Check every rewritten row, case, copy line and sibling line against its fence's Decision text before ticking.
  - Fix-pass rewrites have changed meaning before (DF R7.3j, R6.2a, the ADR input's scope).
  - Where a fence does not settle a point a row needs, **do not decide it**. Leave the box open and report it as a fork.
- **Statuses.**
  - A row, case-bearing row, copy state or OQ row this pass rewrites goes to `pre-alignment`, or `⌛️ Ready for Alignment` in the Data Foundation PRD.
  - A row that only takes an editorial edit keeps its status: an index, map, link or label entry that keeps the same owning document.
- **Budget.** Under F210 the Collection Mode body may reach 12,400 words. The Data Foundation body stays at or under 8,400 (its F55 as clarified).
  - Count with rule 14's method: `perl -0pe 's/<!--.*?-->//gs; s/```.*?```//gs; s/\]\([^)]*\)/]/g; s/^\|[-: |]+\|\s*$//mg; s/\|/ /g' FILE | wc -w`
  - Report both counts when the pass ends.
- **Ticking.** Tick each box with a one-line Result note, as the round-8 fix file does.

`scratchpad` below is the orchestrator's scratchpad.

## A. The record

- [x] **A1 — Review log, "## Round 10 — the PR #21 review (2026-09-26)".** Append this section to `docs/agent-reviews/2026-09-24-prd-collection-mode-peer-reviews.md`, after the Lock section. Leave everything above it byte-identical. In order:
      Result: Appended verbatim (167 inserted lines, 0 deletions, confirmed by `git diff --stat`). Order: intro line; the two reviews and eight inline comments T1–T8 verbatim from `pr21-review-verbatim.md`; "### Orchestrator verification" carrying C1's text verbatim; "### Owner adjudication (2026-09-26)" carrying `round10-adjudication.md` verbatim, OQ 11's paragraph included; a closing line naming this fix file as the resume point.
  1. **One intro line.** The subject is `04dfb97`. There were two reviews and eight inline threads, relayed as peer-agent feedback. Round 10 is an out-of-band review, so no lens was dispatched this round.
  2. **The reviews, verbatim.** Paste `scratchpad/r10/pr21-review-verbatim.md` unchanged.
  3. **"### Orchestrator verification".** Write the eight notes and the gate dispositions in C1 below, verbatim.
  4. **"### Owner adjudication (2026-09-26)".** Paste `scratchpad/r10/round10-adjudication.md` unchanged, the OQ 11 paragraph included.
  5. **A closing line.** It names this fix file as round 10's resume point.

## B. Fences

- [x] **B1 — F202–F210** go after F201 in `prd-collection-mode-fences.md`, in the grammar F200 uses.
      Result: F202–F210 landed after F201, each with Authority (quoting the D record verbatim with the 2026-09-26 adjudication date), Decision, an optional Why, and Carried by. Carried by was set once D and E landed: F202 (R8.8, DF R6.2a, DF DJ3, DF F59), F203 (R2.6, UJ2.1-g), F204 (R3.2, E3, E13, UJ3.3-n, UJ3.3-o, UJ7.1-u), F205 (R2.1, UJ2.3-a, UJ2.3-b), F206 (R2.1, UJ2.3-b), F207 (R5.4, R5.8, UJ5.3-b, UJ5.3-o, UJ5.3-p, UJ5.4-c, UJ5.4-d), F208 (R4.1, UJ3.4-l/m/n), F209 (all eight OQs plus their Feeds row IDs), F210 (governs no rows). Every Carried-by matches the fence → row map (B3).
  - **Parts.** Authority quotes the D record verbatim, with the round-10 adjudication date. Then comes the Decision below, an optional Why, and Carried by, set once C–E land and matching the map.
  - **The fences.**
    - **F202 — The log copy of removed text waits for a running write (2026-09-26)** (D55; qualifies F200).
      - Removed text in the file's own bytes is wiped at F200's deadline: once every read begun before its wipe has ended, whether or not a write is held.
      - A copy of that text in a journal or log the app keeps beside the file goes once nothing still needs that journal or log. Two things can need it: a read begun before the wipe, and a write running when the wipe runs. The copy goes at the latest when that write lands. Clearing the journal or log may hold the next write while it runs.
      - E35 stays up until the text is in none of those bytes, and its wording is unchanged.
      - Why: the review's reproduction, repeated by the orchestrator on SQLite 3.53.4. A WAL log cannot be truncated while a write is in progress.
    - **F203 — The simulated banner's action replaces the narrowing (2026-09-26)** (D56).
      - The device PRD's E22 action clears the search and the row-state filter, and sets the mark filter to simulated alone.
      - The table then lists exactly the items with a simulated current reading, and "Clear filters" lists the whole collection again.
    - **F204 — "Clear sort" returns the table to queue order (2026-09-26)** (D57).
      - "Clear sort" is offered while a view sort is applied, and is reachable from the keyboard as every action is.
      - It returns a collection's table to queue order, and the All items view to R1.9's collection-then-queue order.
      - It writes nothing, and it sits beside "Clear search" and "Clear filters".
    - **F205 — An imported column named like a built-in one is tagged (2026-09-26)** (D58).
      - The rule covers an imported column whose stored name equals, under the import PRD's R2.3 rule, a Swatch field's name or a header the copy file's Column headers table shows.
      - Wherever the app names such a column, it is labelled with its stored name followed by the copy file's " (imported)" tag. That means its header, "Columns", a view sort, "Set a field", the item detail and VoiceOver.
      - Every action on it targets that column, never another with the same label.
      - The file keeps its stored name, so export and re-import are unchanged, and the Import PRD does not change.
    - **F206 — A tagged label that still clashes takes Import's collision form (2026-09-26)** (D63).
      - This applies where a label F205 gives still equals, under the import PRD's R2.3 rule, another column's label.
      - The later column by position takes that PRD's collision template, "⟨base⟩ (⟨suffix⟩)", with the suffix starting at 2 and rising until unique. Stored names are unchanged.
    - **F207 — History distances compare working-set values only (2026-09-26)** (D59 and D64).
      - **The rule.** R5.4 and R5.8 compare two readings' values in the collection's working set: its illuminant, observer and measurement condition.
      - **No current value.** Where the current reading has no such value, no reading shows a distance line, as for an item with no current value.
      - **No value in the condition.** An earlier reading with no such value shows the new line "Not compared — no value in this collection's measurement condition".
      - **Unreadable.** An unreadable (quarantined) reading shows the new line "Not compared — this reading can't be read", and its kept derived values are never compared.
      - **Different light or angle.** Today's not-compared line stays, unchanged, for a value worked out under another illuminant or observer.
      - **Compare.** Compare's single slot shows the same lines. The unreadable line wins over the no-value line (D64). The different-light line applies only when both readings have values that were worked out differently.
    - **F208 — Closing a detail returns to E9 only while its swatch has a usable colour (2026-09-26)** (D60).
      - R4.1 returns to E9 only while E9's item remains and has a working-set value.
      - Otherwise it returns to the table, as when that item is gone. No stale results remain, and there is no new copy.
    - **F209 — OQ 1–7 and 12's interim values are v1's contract (2026-09-26)** (D61).
      - Each of the eight interim values is ratified as v1's, and its OQ closes with a results section.
      - The OQ 1 timing, the OQ 7 display spike and the dogfood closers stay as post-lock checks. A failed check changes a value only through a new dated fence.
      - R8.11's ROW_CONFIRM_BUDGET stays the capture PRD's, under its OQ 5. F209 does not touch it.
    - **F210 — This PRD's word budget is 12,400 (2026-09-26)** (D62). The owner overrides the agent-PRD format's never-raise rule for this PRD, as F177 did for the Data Foundation PRD.
  - **Why lines.** Take each from the D record. Add nothing the owner did not decide.
- [x] **B2 — Clarified lines**, appended and dated 2026-09-26, with no fence body touched.
  - **Under F200:** `**Clarified 2026-09-26 (owner decision D55; F202):** the exact deadline covers text in the file's own bytes; a copy in a journal or log beside the file goes at the latest when a write running at the wipe lands (F202).`
  - **Under each fence that set an OQ 1–7 or 12 candidate:** OQ 1 names F20, F42, F66, F99 and F118; OQ 2 names F34; OQ 3, F16; OQ 4, F7; OQ 5, F19; OQ 6, F20; OQ 7, F127 and F146 (the display check only); OQ 12, F43. Each line reads: ratified as v1's value by F209 (D61).
    - Read each fence first, and clarify only the candidate the OQ names.
  - **Under F64:** OQ 11's closer ran 2026-09-26 and every quoted pair matched the published table. Its authority is "OQ 11's closer, run 2026-09-26". It is not an owner decision.
      Result: All landed, no fence body touched, each a new dated Clarified line. Under F200: the exact-deadline clarification (owner decision D55; F202). Under each OQ-candidate fence: F20 (one combined line, OQ 1's 100 ms/1 s and OQ 6's 24 pt), F42, F66, F99, F118 (OQ 1), F34 (OQ 2), F16 (OQ 3), F7 (OQ 4), F19 (OQ 5, plus a second line noting F203 supersedes its banner-action clause), F127 and F146 (OQ 7, display-check portion only, each line saying the stored-flag delegation to DF is unaffected), F43 (OQ 12). Under F64: OQ 11's closer line, dated and cited to the check itself, not an owner decision.
- [x] **B3 — Fence → row map and Traceability.**
  - Map entries for F202–F210 match their Carried by.
  - Traceability reads F1–F210.
  - The Historical baseline line stays as it is, because F1–F201 predate the first lock and F202–F210 are this amendment's.
  - The Rejected findings list is unchanged.
      Result: Nine map lines added (F202–F210), each byte-identical to its fence's own Carried by field (cross-checked after the UJ2.1-r/s → UJ2.3-a/b and R3.4 → R3.2 corrections). The PRD's Traceability line now reads "Owner decisions F1–F210". The Historical baseline line (fences:9) and the Rejected findings table were not touched.
- [x] **B4 — Data Foundation F59** is the Data Foundation half of F202, in the style of that PRD's F58.
  - Authority: the Collection Mode PRD's F202 (D55).
  - Decision: R6.2a's deadline is qualified as F202 states. E35 stays until the wipe lands, meaning the text is in none of R6.2a's bytes. DJ3 gains the log-copy line (D1).
  - Rows: R6.2a, E35, DJ3, and the inbound Collection Mode line's fence range, which becomes F52–F59.
  - Also add a Clarified line under F58 (7) that cites F59.
      Result: F59 landed in a new "Collection Mode round-10 amendment" section, styled after F58 (Authority/Decision/Why/Rows). Clarified line added under F58's Decision list, qualifying (7). The orchestrator landed R6.2a's row text and the DF status line directly; F59's Rows now read "R6.2, R6.2a, E35, DJ3, and the inbound Collection Mode line's fence range" and a new map line was added ("F59 | R6.2, R6.2a, E35; DJ3; the Collection Mode inbound line...") since DF's fence → row map had none for F59 yet.

## C. Record-keeping answers (no owner decision)

- [x] **C1 — The verification notes for A1**, verbatim:
  - **T1 — reproduced in part.** At `04dfb97` the log ended at round 9. The lock record then landed in `2aca21f` ("## Lock (2026-09-25; record written 2026-09-26)").
    - It records checks 1–20 against `04dfb97`: 0 MISS, and two NOT-RUNs.
    - Those two are check 8 direction 2 and check 9. The mechanical-checks reference's Applicability section exempts them in advance on a first lock, so they need no fence.
    - Still open: no status surface links that record, and the PR's test-plan box is unticked.
    - Fix: C2 links it, the PR body ticks the box, and this amendment gets its own lock record at close.
  - **T2 — reproduced.** The grammar (fences:29–31) asks for the owner decision "by link", but no Authority field carries one.
    - Owner-decision fences quote the question and every option verbatim, and no other committed record of those answers exists.
    - "Approved round-N recommendation M" fences need the round's adjudication record.
    - Fence text is immutable, so the fix is an Authority index in the preamble (C3), not 201 rewritten fields.
  - **T3 — reproduced.** The orchestrator ran the reviewer's sequence on SQLite 3.53.4 (WAL, secure_delete on, automatic checkpoints off).
    - PASSIVE returned (0,2,2), and TRUNCATE was busy at (1,2,2). The main file held no marker, and the WAL still held it.
    - After the writer committed, TRUNCATE returned (0,0,0) and the marker was gone.
    - F200, R8.8, DF R6.2a, DF F58 (7) and the ADR-0003 input's "never holding a write" each state the deadline for every byte. Only an unratified post-lock item qualified it.
    - Decided by D55.
  - **T4 — reproduced.**
    - R5.4 (PRD:391) gates on "an item with a current value", and ZX-006 (journeys:118) has a current reading with no M1 measurement.
    - The only reason line (copy:305) names light, observer or condition, and UJ5.3-b uses it for a missing condition.
    - No case covers an absent current working-set value or a quarantined operand, and DF R3.3g keeps a quarantined reading's derived sets.
    - Decided by D59 and D64.
  - **T5 — reproduced.** R4.1 (PRD:343) tests only that E9's item remains, while R3.7 and R3.8 need a working-set value. UJ3.4-l (journeys:280) covers removal only. Decided by D60.
  - **T6 — reproduced.** R2.6 (PRD:312) promises "exactly those items". R3.4 (PRD:329) ANDs a search with the filters and ORs mark values, and UJ2.1-g (journeys:222) starts unnarrowed. Decided by D56.
  - **T7 — reproduced.** R3.2 (PRD:327) only reverses a sort, and R3.6 keeps it while the file is open. No action or case returns the table to queue order. Decided by D57.
  - **T8 — reproduced.**
    - The import PRD's R2.5 and R2.6 resolve collisions only among input headers and stored columns.
    - The copy's Column headers table (copy:267–279) shows State, Spread and L*, and an imported column is headed by its stored name.
    - R4.8 blocks only renames into those names.
    - Decided by D58 and D63.
  - **Gate dispositions** (the second review body):
    - **ADR stops.** ADR-0003, ADR-0005 and ADR-0006 stop every row, and ADR-0004 stops the capture seam. This is true and already recorded, in the Build dependencies and the decision queue. They are architecture decisions (AGENTS.md §2) and cannot close in this PR. C4 makes "locked" and "ready to build" two stated states.
    - **Delete undo.** R1.7 already settles its release scope under its deferral rule (PRD:279): it is not built while OQ 10 is open, and it is deferred at v1 release if OQ 10 is still open. OQ 10's interim is "R1.7 is not built and E10 does not appear". No change.
    - **DF rows.** The Data Foundation's R1.11, R7.6o–q and E33–E35 sit at Ready for Alignment. They are in round 11's lens scope, so they can flip.
    - **R8.11.** ROW_CONFIRM_BUDGET is the capture PRD's constant, under its OQ 5. UJ9.5-d reports that half as not exercised, never passed. C4 lists it as a build gate. It is not decidable here.
    - **Open questions.** OQ 11 closes on its closer. OQ 1–7 and 12 are ratified by D61 (F209).
      Result: This bulleted text (T1–T8 and the Gate dispositions) landed verbatim in the review log's new "### Orchestrator verification" section under A1.
- [x] **C2 — T1: link the lock record.**
  - The fence preamble gains an item after "Review log": **Lock record**, linking the review log's "## Lock (2026-09-25; record written 2026-09-26)" section by its real anchor. Its text: validated revision `04dfb97`, checks 1–20, 0 MISS, two first-lock NOT-RUNs the reference exempts. A second link to this amendment's lock record is added at close.
  - README row 6 links the same section.
      Result: Landed as a new "**Lock record:**" bullet in the fence preamble, right after "Review log", with the anchor `#lock-2026-09-25-record-written-2026-09-26` (GitHub slug rules over the heading's rendered text, verified by hand against check 11's method) and the exact text the box states. README row 6 links the same anchor, inserted before the amendment clause.
- [x] **C3 — T2: the Authority index** goes in the fence preamble, after the Fence grammar paragraph. No fence's text changes.
  - **Intro sentence.** Each Authority field names its source, and this index links that source to its committed record.
  - **A table: source form → committed record → link.** Enumerate every distinct Authority source form across F1–F210 with `grep -n '^- \*\*Authority:\*\*'`, and give each form a row. The forms include at least:
    - "owner decision D<n>", by adjudication (pre-fill, and rounds 1–6 and 10);
    - "approved round-N recommendation M", for each N;
    - any editorial, testability or orchestrator-ruling form.
  - **Owner decisions D1–D64.** Designate the fence's own verbatim quote as the committed record, and link the review-log section where that adjudication is recorded.
  - **Approved recommendations.** Link the section that lists the numbered recommendations: the log's round-N adjudication paragraph, or the round-N fix file's list, whichever carries the numbers.
  - **Links.** Every link must resolve to a real heading anchor. Verify each with check 11's method.
  - **Scope of the edit.** This is editorial. The preamble is settled, and this adds navigation only.
      Result: Landed after the "Fence grammar" paragraph. Enumerated every distinct Authority form by `grep -n '^- \*\*Authority:\*\*'` over all 210 fences and classifying programmatically: "owner decision D<n>" (63), "approved round-N recommendation M" (104), "approved recommendation M" pre/post-fill (31), "approved loose end (i)–(v)" (5), "approved fix (a)–(g)" (7) — 210 total, no residue. D-number-to-round grouping was read off each fence's own Authority text (e.g. "round-2 adjudication"), giving D1–D14 Round 0, D15–D21 Round 1, D22–D27 Round 2, D28–D31 Round 3, D32–D41 Round 4, D42–D46 Round 5, D47–D54 Round 6, D55–D63 Round 10; D64 (OQ 11's closer) is separately rowed as not an owner decision. Nine rows link the review log's Round 0/1/2/3/4/5/6/10 section anchors (computed by GitHub slug rules from each heading's rendered text, e.g. the em-dash in "Round 2 (2026-09-25) — delta verification" drops to a double hyphen: `round-2-2026-09-25--delta-verification`); seven rows link the round-0 through round-6 fix files, which still exist in the tree; the OQ 11 row links this PRD's own results file. No fence text was touched. Not independently re-verified against GitHub's live renderer — computed by the slug algorithm mechanical-checks.md §11 states, which is the same method check 11 itself uses.
- [x] **C4 — "Locked" versus "ready to build".**
  - **README intro.** In `docs/product/README.md`, after the paragraph that begins "A PRD is not a decision about *how*", add one sentence. It says that a locked PRD settles what gets built, and that building starts only when every stop its Build dependencies name has cleared.
  - **README row 6.** Add "ready to build once ADR-0003, ADR-0005 and ADR-0006 are accepted (ADR-0004 for the capture seam); R1.7 waits on OQ 10; R8.11's row-confirmation half waits on the capture PRD's ROW_CONFIRM_BUDGET (its OQ 5)", plus the lock-record link from C2.
      Result: Both landed verbatim — the intro sentence appended to the existing paragraph, and row 6 gained the lock-record link (from C2) followed by the "ready to build once..." clause exactly as given.

## D. Row, copy and case edits (each fence's Carried by comes from here)

Every new case gets the next free ID in its journey. It carries a Rows cell of live IDs only, and runs in the phase its rows' priorities set, as the Harness's phase rule states. The Test-controls map and the Row-transition index take any line a new case or rule needs. A view sort writes nothing and is not an entity transition.

- [x] **D1 — F202 and DF F59 (the wipe).**
  - **R8.8:** its byte clause adds F202's journal-or-log qualification, citing the Data Foundation PRD's R6.2a.
  - **DF R6.2a:** its parenthetical deadline qualifies the same way, and E35's clause reads until the wipe lands, the text in none of those bytes.
  - **DF DJ3:** add one line covering text a journal or log beside the file still holds, which F58 (7)'s line left out.
    - Given: removed text written just before the delete, a read begun before the delete, and a Collection Mode R8.1f/g write held running (R8.10a) when that read ends.
    - Assert: within 5 s of the read ending, the file's own bytes hold the text nowhere. Within 5 s of the held write landing, no file the app keeps beside the file holds it (R6.2a, F59).
    - Write the fixture storage-neutrally, as the existing line does. Where the existing line names the WAL, match its wording.
  - **The ADR-0003 input (`docs/decisions/README.md`, row 0003):**
    - "its wipe waiting at most until every read begun before that wipe has ended, never holding a write" becomes the F202 form. Text in the file's own bytes keeps the read-based deadline. A journal or log copy goes at the latest when a write running at the wipe lands. Clearing it holds the next write only while it runs.
    - Add F202 to the input's Collection Mode fence list and F59 to its Data Foundation list. The input count stays at eight.
  - **Collection Mode journeys:** find every case that asserts the wipe deadline while a write is held (grep "held", "wipe", R8.10a) and align its Assert to F202. List them in the Result.
  - **`docs/product/post-lock.md`:**
    - Tick the "needs owner (qualifies F200)" item: "settled by Collection Mode F202 / DF F59 in PR #21".
    - Carry its still-open technical notes (truncation reaching the next writer's end or zero; committed frames never overwritten in place; a move's copy tolerating a wipe) into an unticked ADR-0003 item, with the same cites.
      Result: R8.8 landed with F202's journal-or-log qualification, citing the Data Foundation PRD's R6.2a and F59; Status → pre-alignment. Collection Mode journeys: grepped for "held" combined with "wipe"/"bytes hold"/"E35"/"nowhere" — no Collection Mode case asserts the wipe deadline while a write is held (the one case that does, DF's DJ3, lives in the Data Foundation journeys, not here), so none needed realigning. DF DJ3 gained the new line (the WAL-frame case, mirroring the existing checkpointed-file line's wording and structure), since revised to check E35's state at three points (up while the read runs after the delete lands; still up once the read ends while the write still holds the log copy; not up once the write lands and the beside-file check passes) rather than only the final one. The ADR-0003 input's quoted phrase is replaced with F202's form, F202 added to the Collection Mode fence list and F59 to the Data Foundation list, the input count staying at eight. post-lock.md's needs-owner item is ticked with the settled note, and its three still-open technical notes are carried into a new unticked item under ADR-0003, citing Collection Mode F200/F202 and DF F58(7)/F59. **The orchestrator landed the Data Foundation body edit directly** (R6.2a's parenthetical now carries F202's clause, R6.2 flips to ⌛️ Ready for Alignment since its sub-row changed, and the status line reads the new PR #21 form) — not redone here. DF's fence → row map and F59's own Rows now both list R6.2 and R6.2a (added this pass, since F59 had no map line yet). Measured word count: DF's body is 8,402 by rule 14's method (not the 8,400 the orchestrator intended) — reported, not touched; see E1.
- [x] **D2 — F203 (the banner).**
  - **R2.6:** the action clears the search and the row-state filter and sets the mark filter to simulated alone.
  - **UJ2.1-g is rewritten.**
    - Given: the seeded file with Studio Markers showing a search that lists no simulated item, a row-state filter, and another mark selected. Choose values from the seeded data that make each part discriminating, and verify them against the seeded table.
    - When: fire the device PRD's E22 action, then "Clear filters".
    - Assert: the table lists only ZX-005; the search field is empty; no row-state value is selected; the mark filter is simulated alone. E3 renders "narrowed", stating 1 of 13 and offering "Clear filters" but not "Clear search". After "Clear filters", 13 items and no variant.
  - **Device PRD:** check its rows and copy for any line that says what E22's action does. Mirror only if one contradicts F203, and report either way.
      Result: R2.6 landed as "...that PRD's E22 action clears the search and the row-state filter and sets the mark filter to simulated alone, leaving exactly those items listed..."; Status → pre-alignment. UJ2.1-g rewritten with a Given that discriminates each part (search "sky" lists only ZX-001; row-state "captured" lists 10 items; mark "outside-sRGB" lists ZX-002 and ZX-003; together 0 items), and an Assert naming all four post-action controls plus the render/count/offered-actions checks the box states, verbatim. Evidence for the seeded facts, quoted from the Harness's Named defaults table: "Studio Markers item ZX-001 | Swatch Name Sky Blue; ..." — "sky" matches nowhere else, so the search alone lists only ZX-001; the seeded queue order is captured for ZX-001, ZX-002, ZX-003, ZX-004, ZX-005, ZX-006, ZX-007, ZX-013, ZX-014, ZX-015 (ZX-010 "pending; never scanned", ZX-011 and ZX-012 "set aside") — 10 items, matching UJ2.1-j's "10 scanned" — so the row-state filter captured alone lists those 10; and UJ2.1-b's Assert already establishes "ZX-002 and ZX-003 each carry cannot-show and outside-sRGB" — the only two items with that mark. ZX-001 is captured but carries neither mark, so the three filters combined (search ∩ row-state ∩ mark) list 0 items before the action. Device PRD checked: its R6.5 already reads "what E22's action does on the collection surface is Collection Mode R2.6's (F32)" and its copy (E22) states no independent claim about the action's scope — no contradiction, so no Device mirror is needed.
- [x] **D3 — F204 ("Clear sort").**
  - **The row.** Add the action to R3.4's narrowing sentence or to R3.2. Choose the row whose rule it completes, and say which.
  - **Copy.** E3's and E13's Actions lists gain "Clear sort" beside "Clear filters". R3.2 and R3.4 are P0, so it takes no phase mark. Update any surface row that lists those actions.
  - **Case 1, P0.**
    - Given: the seeded file, with Studio Markers' queue order read by SQL before the When.
    - When: fire L*, then "Clear sort".
    - Assert: the table lists the queue order, the sort shows as none, and the file holds the same rows as the read before the When.
  - **Case 2, R2.9's phase.** From that state, drag a pending row. Queue order changes only by that drag.
  - **Case 3, R1.9's phase.** In the All items view, sort, then "Clear sort": the rows are back in collection-then-queue order.
      Result: Landed in R3.2, not R3.4 — "Clear sort" is the sort state's own inverse action (R3.4's "narrowed" variant and its Clear actions gate on search/filter, a different condition), so it completes R3.2's rule: "...and "Clear sort", offered while a view sort is applied and reachable from the keyboard as every action is, returns the table to queue order, the All items view to R1.9's collection-then-queue order, writing nothing." Kept at 2 sentences (rule 7). Status → pre-alignment. Copy: E3's and E13's Actions lists both gain "Clear sort" beside "Clear filters", no phase mark (R3.2 is P0); the Surfaces table's per-surface "Shows" prose doesn't enumerate "Clear search"/"Clear filters" either, so no surface row needed updating. Three new cases: UJ3.3-n (Case 1, P0) reads Studio Markers' queue order by SQL before firing L* then "Clear sort", asserting the full 13-item queue-order list, "the sort shows as none" and the pre-When SQL read matching; UJ3.3-o (Case 2, R2.9's phase) continues from UJ3.3-n's state and drags ZX-010 — pending per the Named defaults, which R2.9 needs — above ZX-001, asserting the full resulting 13-code queue order (ZX-010, ZX-001, ZX-002, ZX-003, ZX-004, ZX-005, ZX-006, ZX-007, ZX-011, ZX-012, ZX-013, ZX-014, ZX-015) rather than "changed only by that drag"; UJ7.1-u (Case 3, R1.9's phase) fires "All items", the Swatch Name header, then "Clear sort", asserting the table is back in R1.9's collection-then-queue order (Gouache Set's two items, then Studio Markers' 13 in queue order).
- [x] **D4 — F205 and F206 (the tag).**
  - **The rule** goes in R2.1, or wherever columns are labelled; state which. It cites the copy file for the tag and the import PRD's collision template for F206. Other rows that name a column (R2.10, R3.2, R4.2a, R6.x "Set a field") are covered by R2.1's rule, not restated.
  - **Copy.** The Column headers note gains the tag " (imported)" and F206's collision form.
  - **The import-to-browse-to-edit case.**
    - Given: a collection imported from `Code,Swatch Name,State,Spread` with only Code mapped, under the import PRD's R2.2, R2.5 and R2.6.
    - When: browse; open an item; set the imported Swatch Name's value; export; re-import the same source file.
    - Assert:
      - The headers show "Code", "Name", "State", …, "Swatch Name (imported)", "State (imported)" and "Spread (imported)".
      - The edit changes only the imported column's value in the file, and the Swatch Name field is unchanged (SQL read).
      - The export carries both fields under the Data Export PRD's R2.4 headers. Trace R2.4a–c for the exact header an imported "Swatch Name" gets, and quote it.
      - The re-import adds no column and matches each stored column (import PRD R2.6).
  - **An F206 case** with one column literally named "State (imported)".
  - **Inherited obligations.** If the Inventory Import line needs it, add a sentence: stored names unchanged, the label display-only.
      Result: Landed in R2.1 (rule) and the Column headers note (copy), citing this PRD's own tagging and Import's collision form for the rare re-clash; R2.10, R3.2, R4.2a and the "Set a field" rows are covered by R2.1's rule and were not restated. Status → pre-alignment for R2.1. Two new cases, both first-phase: UJ2.3-a (a new Scenario 3, import-to-browse-to-edit) now declares Tags directly (TG-001 Marigold, captured, one live spectral reading — the Harness's own fixture-declaration terms, matching how Studio Markers and Gouache Set are seeded) rather than claiming an import-with-only-Code-mapped could set the true Swatch Name and the captured state, which it cannot; the CSV import in the When adds the passthrough columns onto that already-seeded item. The Assert now quotes a value rather than describing a rule: the export's header row carries the true Swatch Name app column headed `sc_swatch_name` — derived from the Data Export PRD's R2.4 ("every app column uses sc_ plus lower snake_case") applied to the field name, since no literal string for it is written anywhere in that PRD's prose — and a separate passthrough column headed literally Swatch Name. The reasoning for why the passthrough isn't renamed (Export R2.3's identity-mapped-only rule reserves the sc_ name for the true field alone; R2.4a/R2.4b's rename never triggers because "Swatch Name" neither begins sc_ nor equals an allocated name) moved out of the Assert into this note. UJ2.3-b (the F206 case) adds a second column literally stored as "State (imported)", asserting the numbered collision form "State (imported) (2)" with both stored names unchanged. Item 7's header-naming check: UJ7.1-u already read "the Swatch Name header" (the field name, not its "Name" display label), matching every existing case that fires this header (UJ3.3-e, UJ8.1-c/d, UJ9.7-c) — left unchanged, already consistent. Inherited obligations: not landed — F205's own Decision text says "No change to the Import PRD," so nothing is imposed on Import (this is CM's display layer only); the Inventory Import outbound/inbound lines were left as they stand.
- [x] **D5 — F207 (history distances).**
  - **R5.4 and R5.8** state F207's rule.
  - **Copy's History lines table.** The R5.4 distance cell keeps today's line verbatim and adds the two new lines verbatim. It states Compare's precedence as the row does.
  - **Cases:**
    - UJ5.3-b's Assert becomes the no-value line.
    - A case where the current reading has no working-set value but a readable earlier reading exists (ZX-006 plus an earlier reading): no reading shows a distance line.
    - A quarantined earlier reading (UJ5.3-d's ZX-016, or the seeded ZX-012 as fits) shows the can't-be-read line.
    - A Compare of an unreadable reading with one that has no value shows the can't-be-read line.
    - A Compare of two readings with values in different references (a non-spectral reading, DF R3.3e) shows today's line.
      Result: R5.4 and R5.8 rewritten per F207/F209's rule (both to pre-alignment). Copy's R5.4 distance cell keeps today's line verbatim, adds both new lines verbatim, and states Compare's precedence (can't-be-read wins). UJ5.3-b's Assert now names the no-value line (descriptively, as the existing not-compared-line cases do, not by literal quote — a literal quote would be a copy-companion string with no Actions/Variant provenance, which check 13 direction 2 would flag as unmatched). Four new cases: UJ5.3-o (ZX-006 with an added earlier reading that has a value; current has none — no reading shows a distance line); UJ5.3-p (ZX-016, UJ4.1-d's quarantined-earlier-reading fixture — the can't-be-read line); UJ5.4-c (a new Slots collection, one quarantined and one no-value reading selected for Compare — can't-be-read wins); UJ5.4-d (a new Mixed Ref collection, a spectral current and a non-spectral earlier reading at another reference — today's line). All run in R5.4's or R5.8's phase.
- [x] **D6 — F208 (the E9 return).**
  - **R4.1** reads "while E9's item remains and has a working-set value".
  - **UJ3.4-l** gains two variants, as new cases:
    - FS-000 remains, but after the re-read its current reading is quarantined;
    - FS-000 remains, but after the re-read it has no value in the chosen condition.
  - In each, closing FS-002's detail shows the table with the list stated in full, and E9 is not up.
      Result: R4.1 landed verbatim as the box states; Status → pre-alignment. Two new cases, UJ3.4-m (quarantined) and UJ3.4-n (no value in the condition), each opening FS-000, choosing FS-002 in E9, inducing the outside change, re-reading and closing FS-002's detail; each Assert states the table's full 7-item list (FS-000 through FS-006) and that E9 is not up.
- [x] **D7 — F209 (ratification) and OQ 11.**
  - **The OQ table rows 1–7, 11 and 12** follow OQ 8 and 9's form. "Decision so far" names the ratifying fence and the value, the Interim cell is empty, and the Closer reads "Closed — the owner (F209); the results file carries the answer". OQ 11's reads "Closed — engineering's check, 2026-09-26; the results file carries the answer". Status goes to pre-alignment until round 11 aligns it.
  - **The constants table:** every "candidate" or "interim rule" wording for those constants reads as v1's ratified value (F209). Values are unchanged.
  - **`prd-collection-mode-oq-results.md`:** add `## OQ 1` … `## OQ 7`, `## OQ 11` and `## OQ 12`.
    - Each states the answer, its fence, and the post-lock check that stays.
    - OQ 1's section keeps R8.11's dependency on the capture PRD's OQ 5.
    - OQ 11's section carries the check's result from the adjudication record, source and method included.
  - **The Legend's no-interim bullet** and the sentence under the OQ table stay true. Re-read them after the edit.
  - **`post-lock.md`:** the Dogfood and engineering items for these OQs read as checks of F209's ratified values, where a failed check comes back as an amendment. Tick any OQ 11 check item as done 2026-09-26.
      Result: OQ table rows 1–7, 11, 12 rewritten in OQ 8/9's form (Decision so far names the ratifying fence, empty Interim cell, Closer "Closed — the owner (F209); the results file carries the answer", OQ 11's naming engineering's check instead); Status → pre-alignment on all eight (OQ 8–10 untouched). Constants table's twelve "interim rule" clauses reworded to "ratified as v1's value (F209)", values unchanged. oq-results.md gained `## OQ 1` through `## OQ 7`, `## OQ 11` and `## OQ 12`, each naming its fence, its rule now in force and the post-lock check that stays; OQ 11's section carries the closer's method and source verbatim.

      **Orchestrator-review correction pass:** diffed every ratified row (1–7, 11, 12) against `git show HEAD` and found OQ 1's old Interim cell content had no surviving copy anywhere — restored verbatim into both OQ 1's "Decision so far" cell and its results-file "Rule now in force" ("Each candidate, on an M1 MacBook Air with 8 GB, internal disk, the first open cold as the journeys' timing workload defines it and browsing warm; read every release there, per-PR timing only a tripwire; R8.11 against the engineering plan's ROW_CONFIRM_BUDGET while the capture PRD's OQ 5 is open."). OQ 11 was also short one clause ("Use the pairs as the journeys quote them.") — restored verbatim in both places. OQ 2–7 and 12 already carried every content word of their old Interim cells (checked word-for-word); no change needed there. Every dangling "OQ 1's/7's interim" reference in non-fence files was rewritten to "OQ 1's/7's answer" (PRD constants table, R2.5, R8.1, M1; journeys:92, :219, and the six "Mac OQ 1's interim names" instances at :519/520/522/523/525/575) — fences.md's own historical quotes were left untouched, and the capture PRD's own OQ 7 (REORDER_SCOPE) is unrelated and unchanged. The Build dependencies table's "Interim: OQ n" entries for the eight now-closed CM questions were removed (kept every Stop: and the capture PRD's OQ 7 REORDER_SCOPE interim); re-read the sentences under the table and the Legend's no-interim/Interim-stated bullets — all still true, since OQ 10 remains the sole open question. The Constants table's "Closure evidence" column now reads e.g. "F209 — OQ 1's timing run stays a post-lock check" for every ratified row, column header unchanged. post-lock.md's OQ 1–7, 12 dogfood/engineering items reworded to note they are now checks of a ratified value; OQ 11's item ticked done 2026-09-26.
- [x] **D8 — Status surfaces while the amendment runs** (the pending form).
  - **The PRD status line:** `Status: locked (2026-09-25); amendment F202–F210 (2026-09-26), peer review pending`.
  - **The fence preamble:**
    - the fourth item is that same clause;
    - the Resolved baselines are filled with `04dfb97` (preservation) and `dc1b747` (change), as full SHAs.
  - **README row 6:** the same clause.
  - **The Data Foundation status line:** add "Collection Mode amendment mirror 2026-09-26 under F59, peer review pending", in its existing style.
  - **Any other sibling** this pass edits gets the same treatment.
      Result: Landed verbatim in the PRD status line, a new "**Amendment pending:** F202–F210 (2026-09-26), peer review pending." bullet in the fence preamble, and a trailing sentence on the Resolved baselines paragraph naming both full SHAs (now full 40-character SHAs, per the orchestrator's correction). README row 6 gained the same clause, appended after the PR #21 clause. The Data Foundation status line: the orchestrator landed it directly ("Status: locked (2026-09-16); amendments PR #17 (F33–F49), PR #18 (Export mirror), PR #21 (Collection Mode, F50–F54, F56–F59; peer review pending). Requirements aligned, not implemented.") — not redone here. No other sibling PRD body was edited this round (Device Management was only read, not changed — see D2's Result).

## E. Checks at the end of the pass

- [x] **E1 — Word counts.** Collection Mode must be ≤ 12,400 and Data Foundation ≤ 8,400. Report Export's count too, if it was touched.
      Result (orchestrator-review correction pass, re-measured after every fix above): Collection Mode 12,277 of 12,400 (F210) — PASS, 123 words of headroom (the verbatim OQ 1/OQ 11 restorations, the "answer" rewordings and the Closure-evidence rewording added words; the Build-dependencies "Interim: OQ n" removals and the pruned "Interim stated" list took some back). Data Foundation 8,402 by rule 14's method — the orchestrator's own edit, not touched by me (per instruction 9); this is 2 words over the 8,400 the orchestrator intended, reported as measured, not corrected. Export not touched, not reported as a budget item.
- [x] **E2 — Mechanical scripts, read-only.** Re-run checks 1, 2, 6, 11, 12, 13, 15, 16 and 18 from `writing-agent-prds/references/mechanical-checks.md` (operator-agents 1.7.0) and report each verdict. Fix any MISS this pass caused. The full roster runs at close.
      Result:
      - **1 (cross-PRD, both directions):** PASS after one fix. Two bare cross-doc cites this pass introduced in UJ2.3-a ("R2.3's identity-mapped-only rule", "R2.4a/R2.4b's rename") were caught and requalified to "the Data Export PRD's R2.3" / "its R2.4a/R2.4b" (rule 1c). Every other new cross-doc cite (DF's R6.2a/DJ3/F59, the import PRD's R2.2/R2.3/R2.5/R2.6, the device PRD's E22/R6.5, the Data Export PRD's E1) already names its document.
      - **2 (index sync):** PASS on the fence→row map (derived 210 Carried-by fields against the map's 210 lines programmatically, 0 mismatches). One disposition on the Legend derivation: OQ 1–7, 11 and 12 now have an empty Interim-rule cell (matching OQ 8/9's closed form) and several carry P0 rows in Feeds (R2.5, R3.1, R8.1, R8.11, R6.2, R7.1, R7.2 among them); read literally, check 2's derivation would demand these appear in "P0 rows that defer to an open question with no interim rule." Disposed as OQ 8 and 9 already establish the precedent: the bullet is about questions still *open*, and these eight are *closed* (ratified, ⌛️/pre-alignment on their rows notwithstanding) — the same reading that already lets OQ 8/9 sit with an empty Interim cell without tripping the bullet. Only OQ 10 remains genuinely open, carries a non-empty interim, and is the "Interim stated" list's one entry (pruned from ten lines to one, since the other nine rows' questions no longer carry an interim rule to derive from).
      - **6 (case refs/copy coverage):** PASS. 66+ distinct Rows cites sampled, 0 unresolved; all 19 copy states (E1–E19) mentioned in journeys.
      - **11 (companion paths/links):** PASS. All 4 companions declared and exist (unchanged). Programmatically resolved all 20 links this pass added or changed (9 review-log anchors, 1 results-file anchor, 6 fix-file links, 1 DF-fences cross-link, 1 README link, 2 decisions/README links) — every file exists and every anchor is in that file's GitHub-slugged heading set.
      - **12 (word count):** see E1. Rule-migration scan: no uncited modal sentence in this pass's copy.md, journeys.md or fences.md additions (the two pre-existing uncited hits found — E17's "measured" variant in copy.md, and quoted owner questions inside other fences' Authority fields — predate this round and are outside its content).
      - **13 (label check):** PASS both directions after one fix. Direction 1: 54 labels extracted (including the new "Clear sort"), each written once in copy.md and only quoted elsewhere. Direction 2: 54 quoted strings in rows/cases, 0 unmatched — after removing quote marks I'd put around dynamic column-header text ("Swatch Name (imported)" etc.) and around the new history-distance lines in journeys' Asserts, neither of which is a copy-provided Actions/Variant label; both now read descriptively, matching the file's existing convention for column names and for the pre-existing not-compared line.
      - **15 (unresolved fill):** PASS. 0 `{{`, 0 `_(…)_`, 0 `<prd-slug>` across all five files.
      - **16 (guidance comments):** PASS. 0 `<!-- guidance` hits.
      - **18 (test-controls/asserted values):** PASS. All new cases drive surfaces already declared in the Test-controls map (collection surface, All items view); added "Clear sort" to the collection-surface and All-items-view Controlled-input prose for completeness. Every new case's asserted identifiers/values (Marigold, Amber, Sunflower, Bright, 0.10, ZX-006's added earlier reading's date, etc.) are declared in that case's own Given; quantities (the 13-item and 7-item lists) read and traced by hand against the Named defaults.

      **Re-run after the orchestrator's correction items, all nine checks again:** 1 PASS (UJ2.3-a's rewritten Given/Assert introduces no new bare cross-doc cite); 2 PASS (unaffected — no fence's Carried-by or the Legend derivation changed this pass, since OQ 1/11's verbatim restoration and the "answer" rewordings touch only prose, not Interim cells or Feeds); 6 PASS (Rows cells on UJ2.3-a, UJ3.3-o unchanged, still resolve); 11 PASS (no new links added this pass); 12 PASS — CM 12,277/12,400, DF 8,402 unchanged (see E1); 13 PASS (`sc_swatch_name` and the rewritten Assert text carry no new quoted, unmatched strings); 15/16 PASS (0 hits, re-swept); 18 PASS (UJ2.3-a's Given now supplies Marigold/captured directly, closing the invented-provenance gap; `sc_swatch_name` traces to the Data Export PRD's stated R2.4 naming rule, not an invented value).
- [x] **E3 — Status list.** List every row, E-state and OQ whose status this pass changed, and why. Nothing flips to aligned in this pass.
      Result: Nothing flips to aligned. `aligned` → `pre-alignment`:
      - **Rows (7):** R2.1 (F205/F206, the imported-column tag), R2.6 (F203, the banner's action), R3.2 (F204, "Clear sort"), R4.1 (F208, the E9 return), R5.4 (F207, history distances), R5.8 (F207, Compare's precedence), R8.8 (F202, the journal/log wipe qualification).
      - **E-states (2):** E3, E13 — both in copy.md and the PRD's copy index — each rewritten to add "Clear sort" to its Actions list (F204).
      - **Open Questions (9):** OQ 1, 2, 3, 4, 5, 6, 7 and 12 — each closed by F209 (owner decision D61); OQ 11 — closed by engineering's check, run 2026-09-26. All nine go to pre-alignment per the box's instruction, until round 11 aligns them.
      Unchanged: OQ 8, 9 stay aligned; OQ 10 stays pre-alignment (untouched, still genuinely open). The Data Foundation PRD's R6.2 flips to ⌛️ Ready for Alignment — landed by the orchestrator directly, since its sub-row R6.2a changed.
