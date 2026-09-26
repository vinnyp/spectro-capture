# Collection Mode PRD — round 11 fixes (the amendment's delta and pre-lock round, 2026-09-26)

This is the resume point for the fix pass after review round 11, subject commit `cee1fb2`.

- **The round.** Nine lenses read the amendment `04dfb97..cee1fb2`: product-manager, staff-software-engineer, test, interface, architecture, privacy, product-marketing, plan and database. The database lens is the re-lock's unseen lens.
  - The reviews are saved in the orchestrator's scratchpad as `round11/review-<persona>.md`, and box A1 copies them into the review log.
- **Authority.** The fence file, with F1–F210 as they stand and F211–F214 from the owner's answers D65–D68 (box B1), and the Data Foundation PRD's F59 and F60.
  - Those answers, and the orchestrator's SQLite reproductions of the database lens's MAJOR-1 and MAJOR-2, are recorded verbatim in `round11/round11-adjudication.md`.
- **Carried over from round 10.** The meaning check, the rule against deciding what a fence leaves open (report a Fork instead), and the status rules all hold.
- **Budgets.**
  - Collection Mode stays at or under 12,400 (F210).
  - Data Foundation stays at or under 8,450, the cap D68 raised to (F214 and DF F55 as clarified).
  - Count both with rule 14's method.
- **Ticking.** Tick each box with a one-line Result note.
- **IDs.** A finding ID below names the lens that raised it:

  | Prefix | Lens |
  |---|---|
  | PM | product-manager |
  | SSE11 | staff engineer |
  | TR | test |
  | IF | interface |
  | ARCH11 | architecture |
  | PRIV11 | privacy |
  | PMM11 | product-marketing |
  | R11 | plan |
  | DB | database (MAJOR-n, MINOR-n, NIT-n) |

## A. The record

- [x] **A1 — Review log, "## Round 11 — the amendment's delta and pre-lock round (2026-09-26)".** Append it after "## Round 10", leaving everything above byte-identical. In order:
      Result: Appended verbatim (1,624 inserted lines, 0 deletions, confirmed by `git diff --numstat`). Order: intro line naming the subject and lens set with the extra-lens reasons and the performance note; the nine reviews verbatim under "### <persona>" in the stated order; "### Orchestrator verification" carrying C1's text verbatim; "### Owner adjudication (2026-09-26)" carrying `round11-adjudication.md` verbatim; "### Dispositions" carrying box E3's per-row tally; a closing line naming this file as round 11's resume point. Anchor verified: `## Round 11 — the amendment's delta and pre-lock round (2026-09-26)` slugs to `round-11--the-amendments-delta-and-pre-lock-round-2026-09-26`, matching the link added in B3.
  1. **Intro.**
     - Subject: `cee1fb2`.
     - The lens set, and why each extra lens ran:
       - plan: pre-lock;
       - database: the unseen lens a re-lock requires;
       - architecture: storage;
       - privacy: the erase promise;
       - product-marketing: new copy.
     - Performance did not run. No budget value changed, and the one new timing effect, a log clearing holding the next write, was read by the database and architecture lenses.
  2. **The nine reviews, verbatim, each under "### <persona>".** Order: product-manager, staff-software-engineer, test, interface, architecture, privacy, product-marketing, plan, database.
  3. **"### Orchestrator verification"**, with C1's text verbatim.
  4. **"### Owner adjudication (2026-09-26)"**: paste `round11-adjudication.md` unchanged.
  5. **"### Dispositions"**: box E3's tally.
  6. **A closing line** naming this file as round 11's resume point.

## B. Fences

- [x] **B1 — F211–F214**, after F210, in F202's grammar. Each Authority quotes its D record verbatim.
      Result: F211–F214 landed after F210, each with Authority (quoting D65/D66/D67/D68 verbatim, dated 2026-09-26), Decision, and Carried by. Carried by: F211 and F212 both R8.8, the Data Foundation PRD R6.2a, the Data Foundation PRD DJ3; F213 R5.8, UJ5.4-c, UJ5.4-e, UJ5.4-f, UJ5.4-g; F214 governs no rows. Every Carried-by matches the fence → row map (B3).
  - **F211 — The log copy waits for every read still using it (2026-09-26)** (D65; qualifies F202).
    - **The rule.** A copy of removed text in a journal or log beside the file goes once two things are true: no read still uses that journal or log, a read begun after the wipe included; and the write running at the wipe has ended, landed or failed. After a crash, it goes at the next open.
    - **E35.** It appears only when another app's read defers a wipe. Once up, it stays until the text is in none of those bytes.
    - **The app's own reads.** Its own export, Save a copy or check holds the copy without any notice, as the app's earlier reads already do (F57).
  - **F212 — Clearing a log never holds a write while it waits on a read (2026-09-26)** (D66; qualifies F202).
    - **The rule.** Clearing a journal or log holds the next write only for its own copy and truncation, never while it waits on a read. A clearing that a read blocks gives way, and is retried once that read ends.
    - **Capture saves.** A capture save never waits on another app's read, and waits on a clearing only within R8.11's budgets.
  - **F213 — Compare depends only on the two selected readings (2026-09-26)** (D67; qualifies F207).
    - **The rule.** Compare's slot shows the two readings' ΔE2000 when both have values in the working set, worked out alike. Otherwise it shows the line for the reading that lacks one, the can't-be-read line first.
    - **Scope.** The history view's rule that an item with no current working-set value shows no From-current line stays with the history view.
  - **F214 — The Data Foundation PRD's word budget is 8,450 (2026-09-26)** (D68). It is recorded there by a dated line under its F55, following F177's precedent, which overrides the never-raise rule for that PRD.
- [x] **B2 — Clarified lines**, dated 2026-09-26, with no fence body touched.
  - **Under F202:** one line citing F211 and F212.
  - **Under F207:** one line citing F213.
  - **Under F120:** R8.11's interim is unchanged by F209, and is listed again under Interim stated (D2).
  - **Under F33:** OQ 1 closed under F209, and its answer carries the Mac this fence names (SSE11-14).
  - **In the Data Foundation fences:**
    - a line under F55 raising the budget to 8,450 (F214), and one under F21 in F21's existing style;
    - a line under F59 citing F60, which also records that its "(D1)" means the Collection Mode round-10 fix file's box D1 (IF m4);
    - new fence **F60**, the Data Foundation half of F211 and F212, written in F59's style. Its Rows are R6.2 and R6.2a, R1.11, DJ3, and the inbound Collection Mode line's range, which becomes F52–F60.
      Result: All six landed as dated lines with no fence body touched. Under F202: cites F211 and F212. Under F207: cites F213 and restates its Compare-only scope. Under F120: states R8.11's interim is unchanged by F209 and listed again under Interim stated. Under F33: cites F209 and OQ 1's answer, naming the M1 MacBook Air. DF fences: a Clarified line under F55 (8,450) and a second under F21 in that fence's existing per-amendment style; a Clarified line under F59 disambiguating "(D1)"; new fence F60 (Authority citing CM F211/F212; Decision completing R6.2a's deadline and R1.11's exemption; Rows R6.2, R6.2a, R1.11, DJ3, fence range F52–F60) under a new "## Collection Mode round-11 amendment" heading, mirroring F59's section.
- [x] **B3 — Maps, Traceability, the Authority index and the status clauses.**
  - Add F211–F214 to the fence → row map.
  - Traceability reads F1–F214.
  - Add the round-11 adjudication, D65–D68, to the Authority index.
  - The amendment clause becomes F202–F214 wherever it appears: the PRD status line, the preamble's pending item and README row 6.
  - Update the Data Foundation fence map with F60.
      Result: Four map lines added (F211–F214), each byte-identical to its fence's own Carried by field. The PRD's Traceability line now reads "Owner decisions F1–F214". A new Authority-index row links owner decision D65–D68 to the review log's Round 11 section (anchor verified by GitHub slug rules). The round-4/5/6 Authority-index rows were also corrected (D8, below) since neither file carries a numbered list. The amendment clause reads F202–F214 in the PRD status line, the fence preamble's "Amendment pending" item, and README row 6. The Data Foundation fence map gained a new F60 line.

## C. Verification notes (for A1)

- [x] **C1 — Orchestrator verification**, verbatim:
      Result: This bulleted text landed verbatim in the review log's new "### Orchestrator verification" section under A1.
  - **Grepped against `cee1fb2` and confirmed:**
    - PMM11-1, PM-2, IF-1, SSE11-5 and R11-M7: copy:288 still reads "under its stored name".
    - PM-1, TR-2, R11-M5, IF-2, SSE11-4 and DB MINOR-2: UJ2.3-a's Given holds no imported column.
    - PM-3, SSE11-1, R11-M1 and TR-17: F120 requires R8.11 under Interim stated, and the list now holds only R1.7.
    - PRIV11-1, SSE11-2, R11-M2 and ARCH11-2: the ADR input's opening clause still gives journals and logs the read-end deadline.
    - IF-5, ARCH11-4, PRIV11-6, R11-m5, TR-19 and DB MINOR-5: DF PRD:280 and CM PRD:475 still read F52–F58.
    - TR-1: Studio Markers' queue order equals its code order (journeys:112).
    - TR-3, DB MAJOR-4 and R11-M6: Import R3.6c's default replaces the edited value on re-import.
  - **Reproduced on SQLite 3.53.4** (see the adjudication record): DB MAJOR-1 and ARCH11-1, a read begun after the wipe holding the WAL copy; and DB MAJOR-2, a TRUNCATE waiting on a reader holding the write lock.
  - **Every Major reproduces.** No finding was rejected as unreproduced.
  - **Forks the Majors raised went to the owner:** D65 (DB MAJOR-1, ARCH11-1, R11-m4), D66 (DB MAJOR-2, ARCH11-2) and D67 (IF-4, SSE11-3, TR-6, R11-M3). D68 answers the budget.
  - **Settled by fence text, not an owner question.** SSE11-3's question (a), a current non-spectral reading at another reference against an earlier value, is settled by D64's text: "the different-light line applies only when both readings have values but they were worked out differently", either reading.
  - **Disposed with no change:**
    - IF n1: the status line stays parseable, and the lock record is linked from the fence preamble and README row 6.
    - "Candidate / interim": the format fixes the constants table's column set.
    - PRIV11-8: E33 mirrors aligned E8 and E14, and post-lock's onboarding item discloses it.
    - PRIV11-11 and PM-12: post-lock (D10).
    - DB NIT-3: post-lock (D10).

## D. Fixes

A new case takes the next free ID in its journey. Its Rows cell holds live IDs, it runs in its rows' phase, and its Assert names values: an identifier, a list, a count or an SQL read.

- [x] **D1 — The erase: F211, F212 and DF F60.**
  - **R8.8.** Rewrite its byte clause so that "the file's bytes" keeps its Vocabulary meaning:
    - text it removes is nowhere in the file's bytes from the moment it lands, open or after a crash, unless a read begun before its wipe defers it;
    - a copy in a journal or log beside the file goes as the Data Foundation PRD's R6.2a states.
    - Drop "the file's own bytes" and "a held write" from the row. This answers IF-6, PRIV11-2, R11-m3, SSE11-13, TR-13, DB MINOR-1 and ARCH11-N1.
  - **DF R6.2a.** Its parenthetical carries F211 and F212 in two parseable clauses (IF m2, R11-m4): the file's bytes but a journal or log; then the journal or log copy, with its deadline, its crash case and the clearing's hold. Keep E35's clause, reading "until the text is in none of those bytes".
  - **DF R1.11.** "but a wipe" becomes "but a wipe (R6.2a)" (ARCH11-3, SSE11-12, IF m1). F60 carries R1.11.
  - **The ADR-0003 input (`docs/decisions/README.md` row 0003).** Rewrite the erase clauses as one coherent set (PRIV11-1, SSE11-2, R11-M2, ARCH11-2, IF m1, PM-10, TR-13):
    - the file's bytes but a journal or log keep the read-based deadline, this app's own reads included;
    - a journal or log copy goes as F211 states;
    - clearing it holds the next write as F212 states;
    - "so capture saves never wait on a removing edit" becomes "so capture saves never wait on a read a removing edit defers, and on a clearing only within Collection Mode R8.11's budgets".
    - Add F211 and F212, and DF F60, to the input's fence lists.
  - **DF DJ3's WAL line (DF journeys:48).** Rewrite it, adding a sub-run for each of the following (DB MAJOR-3, TR-8, DB NIT-1, ARCH11-1, PRIV11-4):
    1. **Seeding.** The removed text is in the main file and in a committed log frame.
    2. **E35 oracle.** E35 is up exactly while a byte read finds the text in a file beside the file, and E35 is read before the bytes at each look, as DJ4's agreement oracle does. Do not require the copy to survive.
    3. **A second outside read.** It is begun after the first ends, while the write is held, and is still open when the write lands. The beside-file copy goes within 5 s of that later read ending, and E35 stays up until then.
    4. **A crash.** The app crashes after the first read ends, while the write is held. Before the reopen, the file's own bytes are clean. Within 5 s of reopening, no file beside the file holds the text.
    5. **Not exercised.** A run that cannot place the text in a log frame reports "not exercised", never passed.
    - Add R6.2a and E35 to DJ3's Rows exercised at DF PRD:24 (TR-23, IF n5).
  - **Fence ranges.**
    - DF PRD:280 becomes "F52–F60 list".
    - CM PRD:475 becomes "its F52–F60 record", adding "R6.2a's journal-or-log copy" beside "its deferred wipe".
    - This answers IF-5, ARCH11-4, PRIV11-6, R11-m5, TR-19 and DB MINOR-5.
  - **`post-lock.md`.**
    - **The ADR-0003 item at :153 gains four notes:**
      - the read begun during the held write (ARCH11-1, DB MAJOR-1);
      - copying back without holding a write, then a clearing that does not wait on readers (ARCH11-2, DB MAJOR-2);
      - copied pages reaching stable storage before truncation, F_FULLFSYNC on macOS (DB risk 2);
      - the file at a move's new place meeting R6.2a's deadline (PRIV11-5).
    - **The E35 dogfood item at :167 gains two windows:** the one after the other app stops reading while a write or the app's own export still holds the copy (PMM11-2, PRIV11-3), and the E35 headline's accuracy there.
    - **New next-pass items:**
      - a wipe that is due while E34 or E15 is up (DB MINOR-7, PRIV11-9);
      - E34 after "Choose the file again" (PM-12);
      - how DF R6.2 counts a quarantined current reading (DB NIT-3).
      Result: R8.8 rewritten to say "the file's bytes" (Vocabulary meaning restored) and delegate the journal-or-log deadline to DF R6.2a, dropping "the file's own bytes" and "a held write". DF R6.2a rewritten with two clean clauses (file's bytes but a journal or log, on the read-based deadline; then the journal-or-log copy with F211's completed deadline and F212's never-wait-on-a-read clearing rule), E35's clause kept verbatim. DF R1.11's "but a wipe" now reads "but a wipe (R6.2a)". The ADR-0003 input (`docs/decisions/README.md` row 0003) rewritten as one coherent set: the general clause now excludes the journal/log (aside), the journal-or-log clause states F211's and F212's rules with their fence cites, and the closing clause reads "so capture saves never wait on a read a removing edit defers, and on a clearing only within Collection Mode R8.11's budgets"; F211, F212 and DF F60 added to the input's fence lists (still eight named inputs). DF DJ3's WAL line rewritten with the four sub-runs the box names plus a fifth "not exercised" run, and R6.2a and E35 added to DJ3's Rows exercised at DF PRD:24. Fence ranges: DF PRD:280 and CM PRD:475 now read F52–F60 both ways (check 1b/1d agreement). `post-lock.md`: the "needs owner" item stays ticked; its unticked ADR-0003 follow-on item gained the four new technical notes (the read begun during the hold, copy-back-then-clear, F_FULLFSYNC, a move's destination deadline); the E35 dogfood item gained the new stale-headline window; three new next-pass items added (a wipe due while E34/E15 is up; E34 after "Choose the file again"; how R6.2 counts a quarantined current reading).
- [x] **D2 — R8.11's interim (PM-3, SSE11-1, R11-M1, TR-17).**
  - Restore under Interim stated: "- R8.11 — the capture PRD's OQ 5 — F120."
  - Add "Interim: the capture PRD's OQ 5 — the engineering plan's ROW_CONFIRM_BUDGET (F120)" to the first Build dependencies row, in the REORDER_SCOPE entry's form.
  - Take the R8.11 clause out of OQ 1's Decision cell and the OQ 1 results section, and drop R8.11 from OQ 1's Feeds. Leave a pointer: "R8.11's row-confirmation half stays under F120's interim, the capture PRD's OQ 5".
  - README row 6: "R8.11's row-confirmation half builds against the engineering plan's declared ROW_CONFIRM_BUDGET until the capture PRD's OQ 5 closes (F120)".
      Result: All four landed verbatim as stated. Interim stated regained its R8.11 line; the first Build-dependencies row gained the Interim entry; OQ 1's Decision cell and the oq-results.md OQ 1 section both replaced the R8.11 clause with the stated pointer, and OQ 1's Feeds cell dropped R8.11 (now R2.5, R3.1, R8.1, R8.2, M1); README row 6 reworded as stated (and folded into the larger D9 row-6 rewrite).
- [x] **D3 — The imported-column tag (F205 and F206).**
  - **copy:288 (the Detail lines table, R4.2a).** "then each imported column under its R2.1 label". This answers PMM11-1, PM-2, IF-1, SSE11-5 and R11-M7.
  - **The Placeholders rule (PRD:548).** "⟨column⟩ a column's R2.1 label, except in E11 and E19, where it is its stored name" (IF-1, SSE11-5, PMM11-4, PRIV11-7, PM-5).
  - **R2.1 and the copy's Column headers note.** Where tagged labels still clash, the later of the clashing columns by stored position takes the import PRD's collision form (PM-4, IF-2, SSE11-4, R11's spec gap). The note says "equals, under the import PRD's R2.3 rule, a Swatch field's name or a header in this table, the L*, C* and h° row counting as three headers" (Nits).
  - **UJ2.3-a.** This answers TR-3, DB MAJOR-4, R11-M6, IF-3, SSE11-10, PM-11, ARCH11-5 and IF m9.
    - Name the import PRD's E14 action verbatim ("Take the new details"), and restate "mapping only Code" for the re-import (its R1.2).
    - Read the file after the Amber edit, the app quit, before the export. Then relaunch.
    - **Export Assert, by values:** the Swatch Name app column, under the sc_ name the Data Export PRD's golden fixes (its R2.4 and R4.1a), holds Marigold; a separate passthrough column headed literally Swatch Name holds Amber. Drop the `sc_swatch_name` literal.
    - **After the re-import:** Marigold is unchanged, the imported Swatch Name reads Sunflower, and no column is added.
    - **Also assert** that TG-001's detail labels the imported field "Swatch Name (imported)".
  - **UJ2.3-b.** This answers PM-1, TR-2, R11-M5, IF-2, SSE11-4 and DB MINOR-2.
    - Given: Tags holding imported columns in stored position order State, then State (imported).
    - Assert per stored column: State reads "State (imported)", and State (imported) reads "State (imported) (2)".
    - Add a reversed-order run as its own case. There, the literal "State (imported)" is first and keeps its label, and the tagged State, being later, reads "State (imported) (2)". Check this against F206 before writing it, and report a Fork if F206 does not settle it.
  - **A new tagged-surface case (TR-4, R11-m12, PM-5, IF-1)**, over Tags with imported columns State and Spread:
    - read, through R8.10b, the "Columns" entries, the detail's Identity labels, "Set a field"'s field list and each header's accessible name, each as "⟨name⟩ (imported)", in the phase of the rows each read needs;
    - hide Spread (imported), and the built-in Spread still shows (R2.10's phase);
    - fire the State (imported) header, and the rows order by the imported values;
    - in R6.2's phase, set State (imported) on two items: E8's ⟨column⟩ is "State (imported)", and an SQL read shows only that column changed.
  - **Normalisation (TR-18).** A case variant with a lower-case "spread" header, labelled "spread (imported)".
  - **Inherited obligations (IF m8).** The import PRD's line lists R2.1, its R2.5 order and its copy's Collision template.
      Result: copy:288 (R4.2a) now reads "then each imported column under its R2.1 label". The Placeholders rule's ⟨column⟩ now reads "a column's R2.1 label, except in E11 and E19, where it is its stored name". R2.1's clashing-label clause now reads "the later of the clashing columns by stored position takes that PRD's collision form" (matching F206), and the copy's Column headers note gained the same "the later of the clashing columns by stored position" wording, "under the import PRD's R2.3 rule", and "the L*, C* and h° row counting as three headers". UJ2.3-a rewritten: names E14's "Take the new details" verbatim, restates "mapping only Code to identity again (the import PRD's R1.2)" for the re-import, reads the file with the app closed right after the Amber edit and before the export (not "at SQLITE_READER_FLOOR" with no named moment), asserts the export by values (the Swatch Name app column under the sc_ name the Data Export PRD's golden fixes — R2.4, R4.1a — holding Marigold; a separate literal "Swatch Name" passthrough column holding Amber; the `sc_swatch_name` literal dropped), asserts the post-re-import end state (Marigold unchanged, imported column reads Sunflower, no column added), and asserts TG-001's detail labels the field "Swatch Name (imported)". UJ2.3-b rewritten with an explicit stored-position order (State, then State (imported)) and per-column asserts. Two new cases: UJ2.3-c, the reversed-order run (F206 settles it cleanly — no fork needed, since F206 gives the suffix to "the later column by position" regardless of which label is tagged). UJ2.3-d, the new tagged-surface case (a fresh collection Tags Two with imported columns State and Spread), reading "Columns", the detail's Identity labels, "Set a field"'s field list and header accessible names as "⟨name⟩ (imported)"; hides Spread (imported) with the built-in Spread still showing; sorts by State (imported); and, in R6.2's phase, sets State (imported) on two items with an SQL read proving only that column changed (values named, not "unchanged", per check 17). UJ2.3-e, the normalisation case (a lower-case "spread" header reading "spread (imported)"). The import PRD's inbound line gained a new row naming R2.1, its R2.5 order and its copy's Collision template.
- [x] **D4 — History distances (F207 and F213).**
  - **R5.4.** Where either reading's value was worked out under another illuminant or observer, it shows the not-compared line (SSE11-3a, DB MINOR-6). The can't-be-read line wins over the no-value line in the history view too (PMM11-3, PM-6), per F207's Unreadable bullet.
  - **R5.8.** "their distance in a single slot, worked out from the two selected readings alone" under F213. Precedence: can't-be-read, then no-value (either reading), then not-compared.
  - **Copy (the History lines table).**
    - Split the R5.4 distance cell into four rows, each with its own identifier: "R5.4 distance", "R5.4 no-value", "R5.4 unreadable" and "R5.4 not-compared". Keep each string verbatim (IF-7).
    - The no-value row reads "for a reading with no value…".
    - R8.10b and R8.10c read that identifier, and the cases assert identifiers.
  - **Cases.**
    - **UJ5.3-q**, history with a different basis: Mixed Ref as UJ5.4-d declares it, in scenario 3. The earlier non-spectral reading shows R5.4 not-compared and no number (TR-5, R11-M4, IF m10).
    - **UJ5.4-c** declares SL-001's current reading: readable, captured, with an M1 value. Run it in both selection orders (TR-6, PM-7, R11-M3, TR-11).
    - **A new Compare case**, a no-value reading against an ordinary one, shows R5.4 no-value (PM-7, TR-11).
    - **A new Compare case on ZX-006** (its current reading has no working-set value) with two earlier readings that have values, shows their ΔE2000 (F213).
    - **A new Compare case**, ZX-006's current reading against an earlier reading with a value, shows R5.4 no-value (SSE11-3b).
    - **UJ5.3-p and UJ5.4-c** declare the quarantined reading's retained working-set values, per DF R3.3g (PM-7, TR-10, R11-m6).
      Result: R5.4 rewritten: "an unreadable earlier reading shows its can't-be-read distance line, which wins over the no-value line, without its kept derived values ever being compared, and either reading's value worked out under another illuminant or observer shows the not-compared distance line" (the "either reading" wording is a reading of F207's own already-generic "a value worked out under another illuminant or observer" text, not a new decision — the fence already covers it). R5.8 rewritten: "their distance in a single slot, worked out from the two selected readings alone: an unreadable reading's line taking precedence, then the no-value line, then the not-compared line" (F213). Copy's History lines table split the single R5.4 distance cell into four identified rows — "R5.4 distance", "R5.4 no-value", "R5.4 unreadable", "R5.4 not-compared" — each string kept verbatim; the no-value row reads "for a reading with no value…". R8.10b now reads "R5.4's and R5.8's distance line by identifier". New case UJ5.3-q (Mixed Ref, scenario 3, R5.4's own phase): the earlier non-spectral reading shows the not-compared line and no number. UJ5.4-c rewritten to declare SL-001's current reading (readable, captured, M1 value) and Q1's retained working-set values (DF R3.3g), and to run in both selection orders. Three new Compare cases: UJ5.4-e (a no-value reading against an ordinary one → no-value line); UJ5.4-f (ZX-006, current value-absent, against two earlier readings with identical values → ΔE2000 0.00, a value chosen to need no external computation since identical Lab triples give ΔE2000 = 0 under any formula); UJ5.4-g (ZX-006's current reading against one earlier reading with a value → no-value line, per F213/D67's fork: Compare depends on the two selected readings alone). UJ5.3-p gained Q1's retained working-set values.
- [x] **D5 — "Clear sort" (F204).** This answers TR-1, SSE11-6, R11-m1, PM-8, IF m5, SSE11-7, R11-m2, TR-12, DB MINOR-3 and SSE11-8.
  - **UJ3.3-n and UJ3.3-o** use a collection whose queue order differs from code order (UJ3.3-a's Codes, or another declared fixture), with the full expected lists:
    - L*, then "Clear sort", then the table lists queue order;
    - "Clear sort" is then not offered;
    - the SQL read shows the file unchanged, with the full exclusion wording "SQLite's own statistics tables, and their rows in its schema table, aside".
    - Chain the drag in R2.9's phase as one case, or grant the continuity in the UJ 3 preamble as UJ5.3-i's is. After the drag, assert both the table order and the capture PRD's R11.11 order.
  - **UJ7.1-u** gets the same kind of fixture and lists every row.
  - **Two further cases:** "Clear sort" is not offered with no sort applied; and with a search and a filter active, "Clear sort" leaves both.
  - **R8.1a** names "Clear sort" among its view-sort inputs, under BROWSE_RESPONSE_BUDGET (SSE11-8).
      Result: UJ3.3-n and UJ3.3-o rewritten against a new fixture, Set Order (SO-1..SO-4, queue order SO-3, SO-1, SO-2, SO-4, differing from both code order and L*-sort order), with the full expected lists at each step; UJ3.3-n now also asserts "'Clear sort' is not offered" afterward. UJ3.3-o continuity granted in the UJ 3 preamble ("except UJ3.3-o, which runs against the state UJ3.3-n leaves, in the phase that lands R2.9"), and its Assert names both the table order and the capture PRD's R11.11 order. UJ7.1-u rewritten against a two-collection fixture (Zone One, queue order differing from code order; Zone Two) and lists every row. Two new cases: UJ3.3-p ("Clear sort" not offered with no sort applied) and UJ3.3-q (with a search and a filter active, "Clear sort" leaves both). R8.1a now reads "a view sort or 'Clear sort'".
- [x] **D6 — The E9 return cases (TR-9, DB MINOR-4, SSE11-15, a Nit).**
  - UJ3.4-m and UJ3.4-n declare their outside changes' end states through the Data Foundation PRD's R7.2: quarantined per R5.5b, and absent and marked as ZX-006 is.
  - Scope the Gamut-margins rule (journeys:94–96) to gamut marks, so these cases may read the unreadable and value-absent marks.
      Result: UJ3.4-m's When now reads "damage FS-000's current reading so it is retained and persistently quarantined (the Data Foundation PRD's R7.2, R5.5b)". UJ3.4-n's When now reads "clear FS-000's measurement in the chosen condition M1, declaring its working-set value absent and marked as ZX-006's is (the Data Foundation PRD's R7.2, R3.3d)". The Gamut-margins rule now reads "no case reads their gamut marks (cannot-show, outside-sRGB) — a case may still read a non-gamut mark, unreadable or value-absent, among them, on one of their items", scoping the old blanket "no case reads their marks".
- [x] **D7 — Ratification clean-ups.**
  - **OQ 5 and its results:** "F19's candidate, C* 3.0 (no source measured)" (PMM11-7).
  - **OQ 11 and its results:** "confirmed as v1's value", not "ratified" (Nits).
  - **oq-results** sections in ascending order: 1–12 as present (Nits).
  - **The OQ 1 Decision cell and results:** a full sentence (Nits).
  - **HISTORY_READINGS_CEILING's estimate** is the owner's: fix PRD:186 and post-lock:188 (PMM11-6, R11-m8, TR-16, SSE11-14, IF m7). Move post-lock:188's check out of a decisions section if it sits in one (R11-m8).
  - **Stale "candidate" wording:** journeys:133 and :521 (Nits).
  - **PRD:74–75:** point at OQ 1's answer and F209's post-lock checks (SSE11-9, IF m6).
  - **UJ2.1-g:** the row-state value becomes "pending", checked against the seeded table so that the three narrowings together still list nothing (TR-7). Drop "Choose Studio Markers" from the When (R11 Nit).
      Result: OQ 5's PRD row and its oq-results.md section both now read "C* 3.0 (no source measured)". OQ 11's PRD row and its oq-results.md section both now read "confirmed as v1's value" in place of "ratified". oq-results.md sections reordered to ascending 1–12 (moved OQ 8 and OQ 9 to precede OQ 11). OQ 1's Decision cell now a full sentence ("Each candidate is measured on...; it is read every release there..."). HISTORY_READINGS_CEILING: PRD:187's Closure evidence now reads "the owner's estimate"; post-lock.md's OQ 1 item now reads "the owner estimates HISTORY_READINGS_CEILING" and was moved out of the "## v1 release" decisions section into "## First build PR" (it is a check, not a v1-cut decision). Stale "candidate" wording fixed at journeys:134 ("Each at the value the PRD's constants table gives") and journeys:532 ("1,000 at its value"); "Candidate / interim" (PRD:179 header) and journeys:133/521 line numbers shifted post-edit but both fixed. PRD:74–75 (the User journeys pointer) now points at OQ 1's answer and F209's post-lock checks. UJ2.1-g fixed per box.
- [x] **D8 — The Authority index (R11-m10).**
  - Read the round-4, round-5 and round-6 fix files.
  - For each approved-recommendation row, name the section that actually lists the numbered recommendations, or say that the fence's own Authority quote is the committed record.
  - Every link must resolve.
      Result: Confirmed by grep that round-4, round-5 and round-6's fix files carry no "## Owner-needed" section or any other numbered-recommendations list (unlike rounds 1–3, which do); each fence's own Authority field states its recommendation's substance verbatim (spot-checked F161). The three Authority-index rows now read "The round-N fix file carries no numbered recommendations list; the fence's own Authority quote is the committed record", matching the D1–D64 rows' pattern. Links unchanged (still point at the round-N fix files, which exist).
- [x] **D9 — Status surfaces.**
  - **The Data Foundation status line:** "Collection Mode, F50–F54, F56–F60; peer review pending". Drop "Requirements aligned, not implemented." (PMM11-5, IF m3, SSE11-11, R11-m9, DB NIT-2).
  - **README row 6:** "first lock 2026-09-25 ([lock record]); amendment F202–F214 (2026-09-26), peer review pending (PR #21)", in place of "peer review closed 2026-09-25" beside "pending" (R11-m9). Keep the ready-to-build clause as D2 amends it.
  - **The round-10 fix file:** replace the absolute scratchpad path with "the orchestrator's scratchpad" (PRIV11-12).
      Result: All three landed verbatim as stated. The DF status line's fence range also corrected to F56–F60 (was F56–F59, stale after F60). README row 6's cross-document amendment clause also corrected from "Data Foundation (F50–F58)" to "(F50–F60)", and the ready-to-build clause kept D2's wording. The round-10 fix file's line 23 now reads "`scratchpad` below is the orchestrator's scratchpad."
- [x] **D10 — Statuses.**
  - **Flip to aligned**, as bookkeeping for round 11's unanimous non-abstaining ALIGN:
    - Collection Mode R2.6, R4.1, E3, E13, and OQ 2, 3, 4, 5, 6, 7, 11 and 12.
    - Data Foundation R1.11, R7.6o, R7.6p, R7.6q, E33, E34 and E35, each to 🤝 Aligned.
    - D7's OQ 5 and OQ 11 wording and D1's R1.11 cite are editorial. The orchestrator verifies them word by word, so those flips stand.
  - **Set to pre-alignment:** every row this pass rewrites (R2.1, R5.4, R5.8, R8.8, R8.1, OQ 1).
  - **Stay at ⌛️ Ready for Alignment:** DF R6.2 and R6.2a.
  - **Set to needs-discussion:** R3.2, objected but with its row unchanged.
  - List every status change.
      Result: Flipped to aligned: Collection Mode R2.6, R4.1, E3, E13, OQ 2, 3, 4, 5, 6, 7, 11, 12; Data Foundation R1.11, R7.6o, R7.6p, R7.6q (Surfaces table) and copy states E33, E34, E35. Set to pre-alignment: R2.1, R5.4, R5.8, R8.8, R8.1 (its sub-row R8.1a rewritten), OQ 1. Set to needs-discussion: R3.2 (TR-1 objected; row text unchanged this pass). Unchanged, stay at ⌛️ Ready for Alignment: DF R6.2 and R6.2a (ARCH11-1/DB MAJOR-1-3 objected). Full tally in the review log's new "### Dispositions" table (E3).

## E. Checks

- [x] **E1 — Word counts.** Collection Mode must be ≤ 12,400 and Data Foundation ≤ 8,450.
      Result: measured by rule 14's method after every fix above. Collection Mode: 12,348 of 12,400 — PASS, 52 words of headroom. Data Foundation: 8,412 of 8,450 — PASS, 38 words of headroom.
- [x] **E2 — Mechanical checks.** Run checks 1, 2, 6, 7, 11, 12, 13, 15, 16, 17 and 18 read-only, fix any MISS this pass caused, and report the verdicts.
      Result:
      - **1 (cross-PRD, both directions):** PASS after one fix. Four bare "DF R…" cites this pass introduced in UJ3.4-m, UJ3.4-n, UJ5.3-p and UJ5.4-c were caught by a targeted grep and requalified to "the Data Foundation PRD's R…" (rule 1c). CM's outbound Data Foundation line and DF's inbound Collection Mode line now agree both ways at F52–F60 (1a). Every other new cross-doc cite (the import PRD's R1.2/R2.5, the Data Export PRD's R2.4/R4.1a, DF's R7.2/R3.3d/R5.5b/R3.3g) already named its document.
      - **2 (index sync):** PASS. F211–F214's map lines are byte-identical to their fences' own Carried-by fields (verified by grep); F213 appears twice (F211/F212 share identical Carried-by) and matches. No existing fence's Carried-by or map line was touched, so F1–F210's index sync is unaffected.
      - **6 (case refs/copy coverage):** PASS. Every Rows cell this pass added or changed (R2.1, R2.9, R2.10, R3.2, R4.1, R4.3, R5.4, R5.8, R6.2, R8.5, R1.8, R1.9) resolves to a live row; no copy state lost its only case.
      - **7 (constants named in owning row):** not exercised — no constant was added, renamed or re-owned this pass; ROW_CONFIRM_BUDGET's citing row (R8.11) is unchanged.
      - **11 (companion paths/links):** PASS. The one new link this pass added (the Authority index's D65–D68 row, linking the review log's new Round 11 heading) resolves: computed by GitHub slug rules as `round-11--the-amendments-delta-and-pre-lock-round-2026-09-26` and confirmed against the actual heading text landed in A1.
      - **12 (word count):** see E1. Rule-migration scan: the new copy.md content (the four split History-lines rows, the Column headers note, R4.2a) each sits in a table row already scanned as content, and none introduces a modal sentence without an owning-row cite beyond the pre-existing convention.
      - **13 (label check):** PASS. "Take the new details" is quoted and matches the import PRD's copy verbatim. The dynamic tag strings this pass writes (State (imported), Spread (imported), spread (imported), State (imported) (2)) are written unquoted/descriptively, following the round-10 convention that dynamic column-header text is not a copy-provided Actions/Variant label.
      - **15/16 (unresolved fill / guidance comments):** PASS, 0 hits, re-swept across all ten files (both PRDs and their four companions each).
      - **17 (banned adjectives in asserts):** PASS after one fix. Two "unchanged" hits this pass introduced (UJ2.3-a, UJ2.3-d) were caught and reworded to name the actual values (TG-001's true Swatch Name still reads Marigold; TT-001/TT-002 keep row state captured and Spread 0.20/0.30 respectively). The one remaining regex hit ("correction answer" in UJ4.7-b) is a pre-existing false positive, not "correct" as an adjective.
      - **18 (test-controls map / asserted values traced):** PASS by inspection. Every new fixture (Set Order, Zone One/Two, Tags Three, Tags Two, Casks, Slots' O1) is fully self-declared in its own case's Given; no case asserts a value, identifier or instant that only an external source could supply. All driven surfaces (Columns, Set a field, header sorts, item-detail Identity lines) are already declared in the Test-controls map.
- [x] **E3 — The disposition tally for A1.** Per row: which lenses ALIGN, OBJECT or ABSTAIN, and the resulting status.
      Result: Tally table built from the nine reviews' own per-row disposition tables and landed verbatim in the review log's new "### Dispositions" section (see A1). Nothing flips to aligned without unanimous non-abstaining ALIGN; every OBJECT is accounted for in D10's status list.

## Orchestrator edits after the pass (2026-09-26)

Verified word by word against F211–F213 before commit:

- **DF R6.2a.** The pass wrote "nothing still needing it, one begun after the wipe included" onto the main-file clause.
  - The text now keeps "nothing waiting on it" there, from F200 and F202.
  - The later-read clause moves to the journal-or-log copy: "once no read still uses it, one begun after the wipe included, and the write running at the wipe has ended". That is F211's scope, and matches the architecture probe, where a later read never delays the main-file copy.
- **R8.8's cite** reads "its R1.10, F59 and F60".
- **The ADR-0003 input.** The journal-or-log copy clause and the clearing clause move out of "on a local volume" and into the general erase clause, since F211 and F212 carry no volume scope (PRIV11-1). The wording "ended, landed or failed" is F211's.
- **UJ7.1-u.** The pass's names made a Swatch Name sort equal the collection-then-queue order, so a "Clear sort" that did nothing would pass.
  - Now ZO-1 is Alpha, ZO-2 Charlie and ZT-1 Bravo.
  - The Assert gives the sorted list (ZO-1, ZT-1, ZO-2), then the cleared one (ZO-2, ZO-1, ZT-1), with "Clear sort" not offered.
- **DF DJ3's WAL line, (a) and (b).** "E35 is still up … not required to survive" contradicted itself.
  - E35 is now up exactly when a byte read taken after it finds the text beside the file, as DB MAJOR-3's fix asks.
  - (b) ends with E35 not up within 5 s of read 2's end.
