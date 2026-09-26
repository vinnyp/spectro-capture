# Collection Mode PRD — round 12 fixes (the delta-verify of round 11, 2026-09-26)

This is the resume point for the fix pass after review round 12, subject commit `3cf3b55`, in which the same nine lenses as round 11 ran.

**Where things are.**
- The reviews are in the orchestrator's scratchpad as `round12/review-<persona>.md`.
- The owner's answers D69–D71 and the orchestrator's re-run of the privacy probe are in `round12/round12-adjudication.md`.

**Rules.**
- The round-10 and round-11 rules carry over: the meaning check; no deciding what a fence leaves open, reported as a Fork instead; the status rules; and rule 14 word counts.
- Collection Mode may go up to 12,400 words and Data Foundation up to 8,450.
- The Collection Mode body is at 12,372 after the orchestrator's edits, leaving 28 words of headroom. Spend it only where a box needs it.

**Already landed by the orchestrator** (verified against D67, D69 and D71; do not redo):
- **Data Foundation R6.2a** is rewritten under D69.
  - The journal or log clause now sits outside the deferral parenthetical, as one principle: "at the first moment, the file open, that no read uses that journal or log and no write runs". The clearing then runs before the next write starts, holding that write only for its own copy and truncation and never while it waits on a read.
  - E35 stays "until the text is in none of the bytes this row names".
- **Collection Mode R5.4** is rewritten under D71.
  - "No current value" now means no value in the collection's measurement condition.
  - The precedence is can't-be-read, then no-value, then not-compared, the last applying only when both readings have values.
- **Collection Mode R5.8** is rewritten under F213. The slot shows ΔE2000 when both readings have values worked out under the collection's illuminant, observer and condition. Otherwise it shows the can't-be-read line, then no-value, then not-compared.
- **The ADR-0003 input** takes the same principle and cites F215 and DF F61. Its capture-save clause uses F212's words: "never wait on another app's read, and on a clearing only within … R8.11's budgets".

**Finding IDs** name their lens, as in round 11:

| Prefix | Lens |
|---|---|
| PMM12 | product-marketing |
| PRIV12 | privacy |
| PM12 | product manager |
| IF | interface, round 12 |
| DB12 | database |
| R12 | plan |
| TR12 | test |
| SSE12 | staff engineer |
| ARCH12 | architecture |

## A. The record

- [x] **A1 — Review log.** Append "## Round 12 — delta-verify of round 11 (2026-09-26)" after Round 11, with everything above it left byte-identical.
      Result: Appended verbatim (1,158 inserted lines, 2 pre-existing deletions from the orchestrator's duplicate-heading fix, confirmed by `git diff --numstat` against `3cf3b55`). Order: intro line naming subject `3cf3b55` and the nine lenses over the amendment `cee1fb2..3cf3b55`; the nine reviews verbatim under "### <persona>" in round 11's order; "### Orchestrator verification" carrying C1's bullets verbatim; "### Owner adjudication (2026-09-26)" carrying `round12-adjudication.md` from its second line onward; "### Dispositions" carrying box E3's per-row tally; a closing line naming this file. Anchor verified by GitHub slug rules: `## Round 12 — delta-verify of round 11 (2026-09-26)` slugs to `round-12--delta-verify-of-round-11-2026-09-26`, matching the Authority-index link added in B3.
  - An intro gives the subject `3cf3b55` and the same nine lenses.
  - Then come the nine reviews verbatim, one per "### <persona>" heading, in round 11's order.
  - Then "### Orchestrator verification", C1 verbatim.
  - Then "### Owner adjudication (2026-09-26)": paste `round12-adjudication.md` from its second line onward, since its first line is that same heading. Round 11's copy duplicated the heading, and the orchestrator removed the duplicate.
  - Then "### Dispositions", the tally from E3, and a closing line naming this file.

## B. Fences

- [x] **B1 — F215–F217**, after F214, in F211's grammar. Each Authority quotes its D record verbatim.
      Result: F215–F217 landed after F214 (fences:1786–1839), each with Authority quoting D69/D70/D71 verbatim (dated 2026-09-26), Decision and Carried by. Carried by: F215 R8.8, DF R6.2a, DF DJ3, DF F61; F216 R5.8, UJ5.4-f; F217 R5.4, UJ5.3-r. Every Carried-by matches the fence → row map (B3), verified programmatically (0 mismatches across all 217 fences).
  - **F215 — A log copy goes at the first moment nothing uses the log (2026-09-26)** (D69; qualifies F202 and F211).
    - **Rule.** A copy of removed text in a journal or log beside the file goes at the first moment, with the file open, that no read uses that journal or log and no write runs. After a crash, it goes at the first open at which that holds.
    - **Clearing.** The clearing then runs before the next write starts, holding that write only for its own copy and truncation (F212).
    - **Scope.** This replaces F211's list of what can hold the copy. F211's E35 and own-reads clauses stand.
  - **F216 — Compare's slot carries no label (2026-09-26)** (D70). The slot sits between the two chips and shows the distance or a Not compared line, with no label. No new copy.
  - **F217 — A current value at another light shows the not-compared lines (2026-09-26)** (D71; qualifies F207).
    - **Rule.** Where the current reading has a value in the collection's measurement condition but worked out under another illuminant or observer, each earlier reading follows R5.4's order: can't-be-read, then no-value, then not-compared.
    - **What "no current value" means.** In the history view's no-lines rule, it now means no value in the collection's measurement condition.
- [x] **B2 — Clarified lines and Data Foundation fences.**
      Result: All landed. Clarified lines added under F211 (cites F215), F207 (cites F217) and F213 (records "alike" per F207's working-set rule, dated to D71/R12-m5). New Data Foundation F61 landed under a new "## Collection Mode round-12 amendment" heading in F60's style, Authority citing CM F215/D69, Rows R6.2, R6.2a, DJ3, fence range F52–F61. A Clarified line under DF F60 corrects its R1.11 sentence per ARCH12-1: the R1.11 exemption covers the wipe of the file's own bytes; the clearing runs once no write runs (F61). Range updates to F52–F61 landed at DF PRD:280 ("F52–F61 list"), CM PRD:477 ("F52–F61 record"), and the DF status line (PR #17/#18/#21, now "F50–F54, F56–F61"); the ADR-0003 input already carried F215 and DF F61 in its fence lists before this pass, confirmed by grep.
  - **Clarified lines**, dated 2026-09-26:
    - under F211, citing F215;
    - under F207, citing F217;
    - under F213, recording that "alike" means worked out under the collection's illuminant, observer and condition, per F207's working-set rule, so two readings at the same foreign reference show the not-compared line.
  - **New Data Foundation F61**, the Data Foundation half of F215, in F60's style. Its Rows are R6.2 and R6.2a, DJ3 and the inbound line's range, which becomes F52–F61.
  - **A Clarified line under DF F60** correcting its R1.11 sentence (ARCH12-1): R1.11's exemption covers the wipe of the file's own bytes; the clearing, which needs the write lock, runs once no write runs (F61).
  - **Range updates to F52–F61:** DF PRD:280, CM PRD:477, the ADR-0003 input's lists (the orchestrator already added F215 and F61 there), and the DF status line's range.
- [x] **B3 — Maps, Traceability, Carried by, the Authority index and status clauses.**
      Result: F215–F217 added to the fence → row map, byte-identical to their own Carried-by fields (verified programmatically, 0 mismatches across 217 fences after fixing two collateral edits an over-eager `replace_all` introduced on F202's and F215's own lines, caught by the same script). Traceability now reads "Owner decisions F1–F217". F211's and F212's Carried by (and map lines) now add the Data Foundation PRD's F60 and F61 (IF n4). F204's Carried by gained UJ3.3-p and UJ3.3-q; F205's gained UJ2.3-c, UJ2.3-d, UJ2.3-e and UJ2.3-f (from D2); F206's gained UJ2.3-c (the clash case); F213's gained the two new Compare cases UJ5.4-h and UJ5.4-i. Authority index gained a D69–D71 row linking to the review log's Round 12 section (anchor verified by the same GitHub-slug derivation used for A1). The amendment clause now reads F202–F217 in the PRD status line, the fence preamble's "Amendment pending" item, and README row 6 (which also gained the DF cross-document range F50–F61). README row 4 (Data Foundation) now mentions "PR #21 Collection Mode amendment (F50–F61), peer review pending" (R12 Nit, IDX:16), left as plain text rather than a hyperlink since no PR #21 URL was already established in this row's style.
  - **Maps and Traceability.** Add F215–F217 to the fence → row map, and update Traceability to F1–F217.
  - **Carried by.** F211 and F212 name DF F60 and F61 (IF n4). F204, F205 and F206 add the cases round 11 created: UJ3.3-p, UJ3.3-q, UJ2.3-c, UJ2.3-d and UJ2.3-e, plus UJ2.3-f from D2 below.
  - **Authority index.** Add D69–D71.
  - **Status clauses.** The amendment clause becomes F202–F217 everywhere it appears.
  - **README row 4 (Data Foundation)** mentions the PR #21 amendment F50–F61, peer review pending (R12 Nit).

## C. Verification notes (for A1)

- [x] **C1 — Orchestrator verification**, verbatim:
      Result: This bulleted text landed verbatim in the review log's new "### Orchestrator verification" section under A1, and every grepped claim it makes was re-confirmed against the current tree during this pass (UJ2.3-d/UJ2.3-f's E8 split, R5.4's foreign-current-reading gap now closed by UJ5.3-r and F217, DJ3's two-way E35 check replaced by the one-way form, DJ3 (c)'s crash moment now bound to a look, DJ3 (a)'s seeding check added, DJ3's new write-while-blocked and second-write sub-runs (f)/(e), and R8.1a's "Clear sort" now in the Timing workload).
  - **Grepped at `3cf3b55` and confirmed:**
    - PMM12-1: copy:309–312 carry "From current" and R5.8 fills its slot from them.
    - PM12-1, TR12-1, SSE12-1: UJ2.3-d asserts E8 for two items, while R6.2 shows E8 only above BULK_CONFIRM_COUNT (10).
    - R12-M1, TR12-4: no case has a foreign current reading, and the Vocabulary's working set (PRD:103) is "under its illuminant and observer".
    - DB12-MAJOR-1, TR12-6, ARCH12-3: DJ3's "E35 is up exactly when…" is a two-way check.
    - DB12-MAJOR-2, R12-M2, TR12-3, PM12-7: DJ3 (c) leaves the crash moment unset.
    - ARCH12-2: DJ3 (a) never shows that the seeded log frame survives the delete.
    - TR12-2, SSE12-2: no case writes while a clearing is blocked.
    - TR12-5: R8.1a's "Clear sort" is in no timing workload.
  - **Reproduced on SQLite 3.53.4:** PRIV12-1 and ARCH12-1, a second write begun while read 2 holds the clearing back.
  - **Decided by the owner:** D69 (PRIV12-1, ARCH12-1, and the database lens's crash-spanning read), D70 (PMM12-1) and D71 (R12-M1, TR12-4).
  - **Every Major reproduces.** None was rejected.
  - **Disposed with no change:**
    - **The "(D2; …)" wording in F120's Clarified line.** It is fence text and immutable. It means the round-11 fix file's box D2, as this note records.
    - **DF:24's "E35" in Rows exercised.** It follows that table's existing practice.
    - **ARCH11-N2**, the round-10 record's 8,402. It is a historical record, and the round-11 E1 note corrects it.

## D. Fixes

A new case takes the next free ID, its Rows cell holds only live IDs, it runs in its rows' phase, and its Assert names values or identifiers.

- [x] **D1 — DJ3's journal-or-log line (DF journeys:48).**
      Result: DJ3's Action and Result columns rewritten in full. (a) gained the seeding check (a byte read after the delete, while read 1 runs, finds the text beside the file) and the one-way E35 form (E35 read first; a byte read finding the text beside the file finds E35 up; within 5 s of a byte read first finding it beside the file nowhere, E35 is not up). (b) now reads "As (a)... (a)'s post-landing clause aside" and uses the same one-way E35 form until read 2 ends. (c)'s crash is now bound to "once a look after read 1's end finds the file's own bytes clean, the write still held", with E35 not up asserted after reopening (PRIV12-6). (d) now ties its not-exercised guard to "(a)'s seeding byte read". New (e) (a second R8.1f/g write started while read 2 runs, F215/DF F61), (f) (a single-item edit made while the clearing is blocked by read 2, F212/DF F61) and (g) (the held write fails and rolls back, F215/DF F61) added. F215 is cited via "the Collection Mode PRD's F215" (linked on first use) and DF's own F61 cited bare throughout, per this file's own-document convention. This answers DB12-MAJOR-1, DB12-MAJOR-2, ARCH12-1, ARCH12-2, TR12-2, TR12-3, TR12-6, TR12-13, PRIV12-1, PRIV12-6, R12-M2, R12-m4 and SSE12-2.
  - **(a) The seeding check.** Add: "a byte read taken after the delete lands, while read 1 runs, finds the removed text in a file the app keeps beside the file".
  - **(a) and (b) The E35 check.** Replace the two-way wording with DJ4's one-way form: "at each look, E35 read first, a byte read finding the removed text beside the file finds E35 up at that look; within 5 s, a functional timeout, of a byte read first finding it beside the file nowhere, E35 is not up".
  - **(b)** Its "As (a)" sets aside (a)'s post-landing clause.
  - **(c) The crash moment.** Crash once a look after read 1's end finds the file's own bytes clean with the write still held.
    - Before the reopen, the file's own bytes hold the text nowhere.
    - Within 5 s of reopening, no file beside the file holds it, and E35 is not up (PRIV12-6).
  - **(d)** A run in which (a)'s seeding byte read does not find the text beside the file reports not exercised, never passed.
  - **(e) New — a second write (F215).** As (b), but after the held write lands and while read 2 runs, start a second Collection Mode R8.1f/g write held running.
    - End read 2, then release that write.
    - Assert: until it lands, the (a)/(b) E35 check holds and the file's own bytes hold the text nowhere.
    - Within 5 s of it landing, no file beside the file holds the text, and E35 is not up.
  - **(f) New — a write made while a clearing is blocked (F212).** As (b), but once the held write lands and while read 2 still runs, make a single-item edit (Collection Mode R8.1c).
    - It shows done within 5 s, a functional timeout.
    - No refusal state renders, and no write's progress shows.
    - The E35 check holds until read 2 ends.
  - **(g) New — the held write fails (F215).** As (a), but the held write fails and rolls back instead of landing. Within 5 s of the failure, no file beside the file holds the text, and E35 is not up.
  - Cite F215 and DF F61 on (a)–(g) where they apply.
- [x] **D2 — UJ2.3-d and the new UJ2.3-f.**
      Result: UJ2.3-a gained the header accessible-name read (moved from UJ2.3-d, P0) and its Assert now checks each tagged header's accessible name equals its text; its E14 cite rewritten to "the import PRD's E14 take-the-new-details action (its R3.5), then its Import action (its R3.8g)" — no quoted sibling label (IF m1, PMM12-3, R12-m2, DB Minor, SSE Nit). UJ2.3-d rewritten: Given now declares Tags Two's queue order, each reading's 3 samples and recorded spread (0.20/0.30), the imported Spread values (7/9), and the full expected "Columns" list; When reads "Set a field"'s fields only after selecting both items; Assert drops the E8 clause (asserts "E8 does not render for the two-item set", R6.2), asserts the full "Columns" list, asserts the shown Spread column holds the recorded 0.20/0.30, and keeps the SQL-only check that State (imported) alone changed. The scenario-3 preamble now names UJ2.3-d (R2.10+R6.2 phase) and UJ2.3-f (R6.2 phase) as its exceptions. New UJ2.3-f: 11 items, all imported-State; select all 11, "Set a field", State (imported), Nova; Assert E8 renders with ⟨column⟩ State (imported) and ⟨n⟩ 11, then, after "Apply to ⟨n⟩ swatches" with ⟨n⟩ rendering 11, an SQL read shows only State (imported) changed on all 11. Test-controls map's "accessibility and keyboard" row now also declares "each column header's accessible name". New UJ2.3-g (TR12-12): an imported column stored C* reads "C* (imported)". This answers PM12-1, TR12-1, SSE12-1, TR12-7, TR12-8, TR12-9, R12-m9, PM12-6 and a database-lens Minor.
  - **UJ2.3-d's Given** declares:
    - Tags Two's queue order (TT-001, TT-002);
    - each reading's samples and recorded spread, 3 samples each;
    - the imported Spread values, e.g. 7 and 9;
    - the full "Columns" list it expects.
  - **UJ2.3-d's When** reads "Set a field"'s fields only after selecting both items.
  - **UJ2.3-d's Assert:**
    - drops the E8 clause and asserts that E8 does not render at two items (R6.2);
    - asserts the full "Columns" list;
    - asserts that the Spread column still shown after hiding holds the recorded spreads 0.20 and 0.30;
    - checks by SQL that only State (imported) changed.
  - **Phase split.** Move the header accessible-name read into UJ2.3-a, which is P0. Keep UJ2.3-d in the phase its rows need and say so.
  - **New UJ2.3-f**, with 11 items, all with the imported State column.
    - Select all 11, fire "Set a field", choose State (imported), enter Nova and press Return.
    - Assert: E8 renders with ⟨column⟩ "State (imported)" and ⟨n⟩ 11.
    - After E8's apply action ("Apply to ⟨n⟩ swatches" at ⟨n⟩ 11, as the copy words it), an SQL read shows only State (imported) changed, to Nova on all 11.
  - **Test-controls map.** Add the header accessible names to its accessibility row.
  - **New case, UJ2.3-g: the L*, C*, h° row as three headers (TR12-12).** An imported column stored C* reads "C* (imported)".
- [x] **D3 — History and Compare (F216, F217, F213).**
      Result: Copy's History lines table: added "R5.8's Compare slot shows the same line with no label, between the two chips (F216)" as a trailing note; dropped ", which wins over the no-value line" from the R5.4 unreadable cell; the R5.4 not-compared cell's trigger now reads "where R5.4 or R5.8 gives the not-compared line". New case UJ5.3-r (scenario 3, R5.4's phase): a non-spectral current reading at D65/10° with three named earlier spectral readings — CX1 (M1 value) asserts R5.4 not-compared with no number, CX2 (no M1 measurement) asserts R5.4 no-value, CX3 (unreadable) asserts R5.4 unreadable. Identifiers: UJ5.3-p, UJ5.3-q, UJ5.4-c, UJ5.4-d, UJ5.4-e and UJ5.4-g now assert "the copy file's R5.4 …" identifiers in place of descriptive "can't-be-read/no-value/not-compared" phrasing. UJ5.4-f's Given rewritten to a published pair (H1/H3's values from UJ5.3-a, ΔE2000 2.0425) and its Assert now reads "2.04, with no label". New Compare case UJ5.4-h (Slots as UJ5.4-e declares it; Q1 vs O1) shows R5.4 unreadable, never the ΔE2000 Q1's retained values would give. New Compare case UJ5.4-i (F213's "alike", R12-m5): two non-spectral readings both at D65/10° in a D50/2° collection show R5.4 not-compared. F213's Carried by and the map gained UJ5.4-h/i. This answers PMM12-1, PMM12-4, PM12-3, TR12-4, TR12-10, TR12-11, R12-M1, R12-m11, IF m7, IF m8 and SSE12-6.
  - **Copy, the History lines table.**
    - Add one line: "R5.8's Compare slot shows the same line with no label, between the two chips (F216)".
    - Remove ", which wins over the no-value line" from the R5.4 unreadable cell (the rows carry the precedence).
    - The R5.4 not-compared cell's trigger reads "where R5.4 or R5.8 gives the not-compared line".
  - **New case, UJ5.3-r, in scenario 3 (D71).** An item's current reading is non-spectral, worked out at D65/10° (DF R3.3e), with declared values. It has earlier spectral readings: E1 with an M1 value, E2 with no M1 measurement, and E3 unreadable with no M1 value.
    - E1 shows R5.4 not-compared, with no number.
    - E2 shows R5.4 no-value.
    - E3 shows R5.4 unreadable.
  - **Identifiers.** UJ5.3-p, UJ5.3-q, UJ5.4-c, UJ5.4-d, UJ5.4-e and UJ5.4-g assert "R5.4 distance", "R5.4 no-value", "R5.4 unreadable" and "R5.4 not-compared". Use "the copy file's R5.4 unreadable line" and similar.
  - **UJ5.4-f** uses a published pair: the H1 and H3 values from UJ5.3-a on ZX-006's two earlier readings, giving ΔE2000 2.04.
    - Also assert that the slot carries no label.
  - **New Compare case: Q1 against O1** (UJ5.4-c's fixture). It shows R5.4 unreadable, never the ΔE2000 Q1's retained values would give.
  - **New Compare case, F213's "alike" (R12-m5).** Two readings, both non-spectral at D65/10°, in a D50/2° collection, with declared values. Compare shows R5.4 not-compared.
- [x] **D4 — Timing "Clear sort" (TR12-5, PM12-5, IF m3, SSE12-7, R12-m7).**
      Result: The Timing workload (journeys:138) now reads "each fired twice, then 'Clear sort'". UJ9.5-a's action now delivers "200 header fires and 100 'Clear sort' fires", counts consistent with M1's population (100 sortable columns, each fired twice, then cleared once); its R7 grid-phase clause already says "deliver the same workload", so it inherits the addition with no further edit. UJ9.5-b's All-items run now also delivers "100 'Clear sort' fires there". UJ9.5-d (R12-m8): the outside read is now "released after the 10th" set rather than the 20th, so a clearing overlaps capture saves; its not-exercised clause for ROW_CONFIRM_BUDGET (F120) is unchanged.
  - The Timing workload (journeys:138) reads "headers … each fired twice, then 'Clear sort'".
  - UJ9.5-a adds "Clear sort" to its inputs, its grid phase included. UJ9.5-b adds it to its All items run.
  - Keep the counts consistent with M1's population.
  - **UJ9.5-d (R12-m8).** Release the outside read after the 10th set rather than the 20th, so a clearing overlaps capture saves. Keep its not-exercised clause for ROW_CONFIRM_BUDGET.
- [x] **D5 — Cites and copy statuses.**
      Result: UJ2.3-a's cite fixed as part of D2 above (same edit answers both boxes' identical instruction). The copy file's E3 and E13 "Status:" lines both now read "aligned", matching the PRD copy index (which D10 already flipped in round 11).
  - **UJ2.3-a:** "the import PRD's E14 take-the-new-details action (its R3.5), then its "Import" (its R3.8g)", with no quoted sibling label. This answers IF m1, PMM12-3, R12-m2, a database-lens Minor and an SSE Nit.
  - **The copy file's E3 and E13 "Status:"** lines read "aligned", matching the PRD index (IF m2, PMM12-2).
- [x] **D6 — Wording Minors.**
      Result: PRD:74–76 now reads "...and a finding on a real display's reported gamut, not one R8.10a declares, stays behind F209's post-lock display spike." OQ 1's Decision cell (PRD) and oq-results.md's OQ 1 section both now read "Each value is to be timed on an M1 MacBook Air..." and "...R8.11's row-confirmation half stays under F120's interim while the capture PRD's OQ 5 is open." The Interim stated line now reads "R8.11 — the capture PRD's OQ 5 — the engineering plan's declared ROW_CONFIRM_BUDGET (F120)."
  - **PRD:74–76:** "a finding on a real display's reported gamut, not one R8.10a declares, stays behind F209's post-lock display spike" (R12-m10, SSE12-4, a PM Nit).
  - **OQ 1's Decision cell and oq-results OQ 1:**
    - "Each value is to be timed on an M1 MacBook Air…" (PMM12-6, a PM Nit).
    - "R8.11's row-confirmation half stays under F120's interim while the capture PRD's OQ 5 is open" (a plan Nit).
  - **The Interim stated line:** "R8.11 — the capture PRD's OQ 5 — the engineering plan's declared ROW_CONFIRM_BUDGET (F120)" (a PM Nit).
- [x] **D7 — post-lock.md.**
      Result: The first-build item (PL:139) ticked, attributed to "the round-11 fix pass, PR #21" (its five rows — DF R1.11, R7.6p, R7.6q, E34, E35 — are now aligned); a new open item follows it keeping DF R6.2 and R6.2a open until they align (R12-m3). Help-docs lines: the backups line (PRIV-15) gained "or before SpectroCapture has wiped it" (PRIV12-5); the export/Save-a-copy line (F201) gained a trailing sentence that a read begun shortly after a removal also holds the copy silently (PRIV12-4); a new line states the "(imported)" tag is display-only and a CSV export and the file keep the stored name (PMM, previously flagged "still missing"). The ADR-0003 technical-notes item gained a sixth note: a rollback-journal choice, if ADR-0003 picks one, needs its own DJ3 sub-run, since today's (d) only reports a WAL-seeding failure as not exercised (PRIV12-7). Next-pass items added under Collection Mode: fire E34's OK and exercise a held move's available actions (TR-14/TR-15, carried forward via TR12-15); the ⟨column⟩ wording for a built-in column in E8 (PMM12-5, IF n3); E19's headline for a tagged column (R12-m13); and a needs-owner item on E35 when the pre-wipe read was the app's own and a later outside read holds the copy (SSE12-8).
  - **The first-build item (PL:139):** tick the rows now aligned (DF R1.11, R7.6p, R7.6q, E34, E35). Keep an open item for DF R6.2 and R6.2a, until they align (R12-m3).
  - **Help-docs lines:**
    - an export or Save a copy begun after a removal also keeps it beside the file for its length (PRIV12-4);
    - "…or before SpectroCapture has wiped it" on backups (PRIV12-5);
    - "the (imported) tag is display-only; a CSV export and the file keep the stored name" (PMM).
  - **The ADR-0003 item** notes that a rollback-journal choice needs its own DJ3 line (PRIV12-7).
  - **Next-pass items:**
    - E34's OK, and actions under a held move (TR-15, carried from round 11);
    - the ⟨column⟩ wording for a built-in column in E8 (PMM12-5, IF n3);
    - E19's headline for a tagged column (R12-m13);
    - E35 when the pre-wipe read was the app's own and a later outside read holds the copy (SSE12-8), marked needs owner.
- [x] **D8 — Statuses (bookkeeping for round 12's dispositions).**
      Result: Flipped to aligned: R3.2 (unanimous ALIGN across all nine lenses) and OQ 1 (unanimous non-abstaining ALIGN; D6's wording is editorial and verified word for word). Set to needs-discussion: R2.1 (PM/SSE/TR objected, but only to UJ2.3-d's case, fixed by D2), R8.1 (TR objected to the missing "Clear sort" timing case, fixed by D4) and R8.8 (TR objected to DJ3's cases, fixed by D1; R8.8 itself only delegates to R6.2a). Stayed at pre-alignment: R5.4 and R5.8, the two rows the orchestrator rewrote under D71/F213 before this round's reviews ran; their row text was not touched this pass. Stayed at ⌛️ Ready for Alignment: DF R6.2 and R6.2a (SSE/TR/ARCH/PRIV/plan/DB all objected to DJ3's cases and R6.2a's deadline wording, both now fixed by D1, but the rows' own text and status are the Data Foundation PRD's, still short of that PRD's own review pass). Every change listed above; the full per-lens tally is in the review log's new "### Dispositions" section (A1).
  - **Flip to aligned**, since every non-abstaining lens chose ALIGN in round 12 and the rows are unchanged since `3cf3b55`: R3.2 and OQ 1. OQ 1's D6 wording is editorial and the orchestrator verifies it.
  - **Set to needs-discussion**, as rows objected to but left unchanged: R2.1 (D2 is cases only), R8.1 (D4 is cases only) and R8.8 (it delegates to R6.2a, and D1 is cases).
  - **Stay at pre-alignment:** R5.4 and R5.8, which the orchestrator rewrote.
  - **Stay at ⌛️ Ready for Alignment:** DF R6.2 and R6.2a.
  - List every change.

## E. Checks

- [x] **E1 — Word counts.** Collection Mode ≤ 12,400; Data Foundation ≤ 8,450.
      Result: measured by rule 14's method after every fix above. Collection Mode: 12,391 of 12,400 — PASS, 9 words of headroom (started at 12,372, so this pass spent 19 words net). Data Foundation: 8,426 of 8,450 — PASS, 24 words of headroom (unchanged from the pre-pass figure; the DJ3 rewrite and F61's dated line landed inside the existing budget with no net word growth in the counted body, since check 12 excludes fence-file and journeys-file prose from the PRD-body budget and the only DF PRD-body edits were the status line and the F52–F61 range word).
- [x] **E2 — Mechanical checks, read-only.** Run checks 1, 2, 6, 7, 10, 11, 12, 13, 15, 16, 17 and 18.
  - **Check 1c:** confirm no bare sibling ID is left in a new case (UJ2.3-a's E14 was missed last pass).
  - **Check 13:** confirm no quoted label is missing from the copy file.
  - Report each verdict. Fix any MISS this pass caused.
      Result:
      - **1 (cross-PRD, both directions):** PASS after one fix. UJ2.3-a's bare `E14` and quoted `"Import"` (this document's own Swatch detail carries no such action or label) were caught and rewritten to "the import PRD's E14 take-the-new-details action (its R3.5), then its Import action (its R3.8g)" — no quoted sibling label. DF PRD:280 and CM PRD:477 agree both ways at F52–F61 (1a). Every other cross-doc cite this pass added (DF's R3.3e in UJ5.3-r/UJ5.4-i, Collection Mode R8.1f/g and R8.1c in DJ3, the Collection Mode PRD's F215/F212 in DJ3 and DF fences) already names its owning document.
      - **2 (index sync):** PASS after one fix. Programmatically diffed every fence's own Carried-by field against the fence → row map's line for all 217 fences: an over-eager `replace_all` on F211/F212's Carried-by text had also matched a substring inside F202's and F215's own lines (both share the "R8.8, the Data Foundation PRD R6.2a, the Data Foundation PRD DJ3" prefix), duplicating DF F60/F61 there; caught by the script and corrected. Re-run: 0 mismatches.
      - **6 (case refs/copy coverage):** PASS. Every Rows cell this pass added or changed (R2.1, R2.10, R6.2, R5.4, R5.8, R8.9) resolves to a live row or metric ID; every copy state E<n> still resolves to at least one case.
      - **7 (constants named in owning row):** PASS, run in full (not "not exercised" — the subject parses and the applicable set is non-empty). All 12 constants are named in at least one requirement row; none was added, renamed or re-owned this pass, so the set is unchanged from the pre-pass state.
      - **10 (two-sentence scan):** run in full; one pre-existing 3-sentence cell (R2.1) surfaced, unchanged by this pass — a candidate for a future compaction pass, not a MISS this round introduced.
      - **11 (companion paths/links):** PASS. The new links this pass added — the Authority index's D69–D71 row (`round-12--delta-verify-of-round-11-2026-09-26`, computed by GitHub slug rules and confirmed against the actual heading landed in A1) and DF fences'/DF journeys' "the Collection Mode PRD's F215" links to `prd-collection-mode-fences.md` — all resolve.
      - **12 (word count):** see E1. Rule-migration scan: the new copy.md content (the R5.8-Compare-slot note, the two edited History-lines cells) sits beside the existing owning-row cites and introduces no uncited modal sentence.
      - **13 (label check):** PASS after one fix. `"Apply to 11 swatches"` in UJ2.3-f was a dynamic, non-literal rendering of the placeholder label; rewritten to the literal `"Apply to ⟨n⟩ swatches" with ⟨n⟩ rendering 11`, matching the established convention (UJ6.2-c, UJ6.4-p, UJ9.5-c, UJ9.7-b). Re-run: every quoted string in a requirement row or a case now resolves character-for-character to an Actions/Variant label in the copy file.
      - **15/16 (unresolved fill / guidance comments):** PASS, 0 hits, swept across the PRD and its four companions.
      - **17 (banned adjectives in asserts):** PASS. No new hit; the one regex match (`UJ4.7-b`'s "correction answer") is the pre-existing false positive round 11 already recorded, not "correct" as an adjective.
      - **18 (test-controls map / asserted values traced):** PASS by inspection. The new "accessibility and keyboard" map line now declares the header accessible-name read UJ2.3-a exercises. Every new fixture (Tags Four, Tags Six, Cross Ref, Off Basis) and every reused fixture (Slots via UJ5.4-e/UJ5.4-h) is fully self-declared in its own case's Given or the case it chains from; no case asserts a URL, identifier or instant that only an external source could supply.
- [x] **E3 — The disposition tally for A1.**
      Result: Tally table built from the nine reviews' own per-row disposition tables and landed verbatim in the review log's new "### Dispositions" section (see A1). Nothing flips to aligned without unanimous non-abstaining ALIGN; every OBJECT is accounted for in D8's status list.

## Orchestrator edits after the pass (2026-09-26)

- **R2.1, an editorial fix (punctuation only).**
  - E2 reported R2.1 as a "pre-existing" three-sentence cell. It is not pre-existing: round 10's F205 sentence made it three, against check 10's two-sentence limit, and the first lock recorded 0 cells over two.
  - The second and third sentences are now joined with a comma: "…in its stored position, an imported column whose stored name equals…". No word changed.
  - R2.1 stays needs-discussion.
- **The orchestrator's edits before this pass,** verified against D67, D69 and D71:
  - DF R6.2a, CM R5.4 and CM R5.8, as this file's header states.
  - The ADR-0003 input's erase clause and capture-save clause.
  - The review log's duplicated round-11 heading, removed.
