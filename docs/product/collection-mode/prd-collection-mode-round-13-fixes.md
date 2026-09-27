# Collection Mode PRD — round 13 fixes (the delta-verify of round 12, 2026-09-26)

This is the resume point for the fix pass after review round 13 (subject commit `7c12387`).

- **The round.** The same nine lenses ran. Their reviews are in the orchestrator's scratchpad, `round13/review-<persona>.md`.
- **No owner decision this round.** Every Major is a case, wording or cite fix that an existing fence settles.
- **Rules.** The round-10 to round-12 rules carry over.
- **Budgets.** Collection Mode ≤ 12,400 words, now 12,397. Data Foundation ≤ 8,450, now 8,431. **Do not add a word to either PRD body.**

**Already landed by the orchestrator.** Each was verified against F207, F213, F215, F216 and F61. Do not redo any of them.

- **R5.4 and R5.8.** The ΔE2000 clause now needs readable readings:
  - R5.4: "when it is readable and both were worked out…";
  - R5.8: "when both are readable and have values…".

  This fixes PM13-2, IF13-1, R13-M2, SSE13-1, TR13-2, PM13-3 and R13-m1. F207's "kept derived values never compared" now holds in both rows.
- **R2.1.** A semicolon replaces the comma splice, an editorial fix.
- **R8.8's cite.** Now "its R1.10 and F59–F61", an editorial fix.
- **DF R6.2a.** Now "any index or other file the app keeps beside it, a journal or log aside" (PRIV13-1, R13-m5, SSE13-3, ARCH13-3).
- **The copy's History lines table:**
  - the heading cites R5.8;
  - the R5.4 not-compared trigger reads "where either reading's value was worked out under another illuminant or observer";
  - the Compare note reads "R5.8's Compare slot shows whichever of these lines R5.8 gives, with no label, between the two chips (F216)" (PMM12-1 residual, IF nits).
- **DF DJ3's journal-or-log line, rewritten in full:**
  - (a) scopes "E35 is up" to "while read 1 runs" and says "list the state" (TR13-3, R13-m4, the database Nit).
  - (b), (e) and (f) use "(a)'s one-way E35 check".
  - (d) says "a sub-run whose own seeding byte read…" (TR m6).
  - (e) sets aside (b)'s read-2-end clause and starts from (a)'s main-file deadline (DB13-MAJOR-1, R13-M3, PRIV13-2).
  - (f) holds read 2 at least 10 s after the edit and bounds the edit at 1 s, with read 2 still running. It ends with the read-2-end clearing (DB13-MAJOR-2, TR13-4, SSE12-2 residual, ARCH13-4, PRIV13-2).
  - (g) fails the held write by a full volume, with room then restored and E15 rendering (R13-M4, SSE13-2, TR13-5, PM13-7).
  - New (h) holds a read across the crash (ARCH13-2, TR m7).
- **The review log.** The heading "### Orchestrator verification" that round 12 fused onto a table row is split back onto its own line.

## A. The record

- [x] **A1 — Review log: "## Round 13 — delta-verify of round 12 (2026-09-26)".** Append it after Round 12, leaving everything above byte-identical apart from the orchestrator's split heading. In order:
      Result: Appended verbatim (1,217 inserted lines, confirmed by `wc -l` against the pre-pass 21,899 and post-pass 23,116 line counts, with no line above the new heading touched). Order: an intro line naming subject `7c12387` and the nine lenses over the amendment `3cf3b55..7c12387`; the nine reviews verbatim under "### <persona>" in round 12's order (product-manager, staff-software-engineer, test, interface, architecture, privacy, product-marketing, plan, database), each followed by a blank line before the next heading (verified: `grep -n "^### "` shows every boundary clean, no fused heading); "### Orchestrator verification" carrying box C1's bullets verbatim (diffed byte-for-byte against C1 below, apart from C1's own checklist-wrapper indent); "### Dispositions" carrying box E3's per-row tally, built from the nine reviews' own final disposition tables; a closing line naming this file. No owner-adjudication subsection landed, matching this round's "no owner decision" note.
  - An intro line.
  - The nine reviews, verbatim, under "### <persona>", in round 12's order.
    - End each pasted review with a blank line, so the next heading renders. The round-12 paste fused one heading.
  - "### Orchestrator verification", holding C1 verbatim.
  - "### Dispositions", holding E3's tally.
  - A closing line naming this file.
  - No owner-adjudication subsection: no owner decision was taken this round.

## B. Fences (Clarified lines only; no new fence)

- [x] **B1 — Clarified lines**, each dated 2026-09-26:
      Result: Three Clarified lines landed, each carrying the Authority "round-13 orchestrator bookkeeping; no owner decision; the fence it points to governs": under DF F60 (data-foundation fences:659, IF13-m3, pointing to F61 and restating F61's principle, mirroring F211's own "replaced by" Clarified line style); under Collection Mode F200 (collection-mode fences:1711, pointing to F215) and under DF F58 (data-foundation fences:634, pointing to F61), both for PRIV13-4 and the architecture Nit. All three are new lines appended below each fence's existing text; no existing fence line was edited (confirmed by `git diff` showing only `+` insertions in both fences files, no `-` lines under any `### F` heading).
  - **Under DF F60:** F61 replaces F60's list of what holds the journal or log copy (IF13-m3).
  - **Under Collection Mode F200 and DF F58**, whose Clarified lines still name "the latest when a write running at the wipe lands": each gets a pointer to F215 and F61 respectively (PRIV13-4, an architecture Nit).
  - **Authority for all three:** "round-13 orchestrator bookkeeping; no owner decision; the fence it points to governs".

## C. Verification notes (for A1)

- [x] **C1 — Orchestrator verification**, verbatim:
      Result: This bulleted text landed verbatim (one indent level shallower, as the top-level content of the review log's new "### Orchestrator verification" section rather than nested under a "C1 —" bullet, exactly as round 12's C1 was pasted) — confirmed by `diff` against the review log's new section, byte-identical apart from that de-nesting.
  - **Confirmed by grep at `7c12387`:**
    - PM13-1, PMM13-1, IF13-2, R13-M1, TR13-1 and DB13-MAJOR-3: UJ2.3-d's "Columns" list includes Code, and R2.10 (PRD:310) and UJ2.2-a exclude it.
    - PM13-2, IF13-1, R13-M2, SSE13-1 and TR13-2: R5.8 put ΔE2000 before the unreadable line, and DF R3.3g keeps a quarantined reading's sets. This was the orchestrator's own round-12 rewrite.
    - DB13-MAJOR-1 and R13-M3: DJ3 (e) inherited (b)'s read-2-end clause.
    - DB13-MAJOR-2, TR13-4 and the SSE12-2 residual: (f) set no floor on how long read 2 lasts after the edit, and its 5 s bound passed a busy-wait clearing. The database lens's probe shows busy timeouts of 2,000 ms and 4,500 ms passing.
    - R13-M4, SSE13-2 and TR13-5: (g) named no fault.
    - TR13-3: (a)'s "E35 is up" lost its scope.
    - PMM12-1 (residual): the copy's not-compared trigger named R5.8 while its Label reads "From current".
  - **All reproduced.** None was rejected.
  - **No owner question.** Each fix is settled by F207, F213, F215, F216, R2.10 or R8.10a.
  - **Disposed to post-lock:**
    - the meaning of "a read uses the journal or log" in SQLite's terms (ARCH13-1, a database Minor). Both lenses rate it Minor, and no case fails a correct build.
    - an outside app's write (ARCH13-6). It predates the amendment and needs the owner.

## D. Fixes (Collection Mode journeys, post-lock, statuses)

- [x] **D1 — UJ2.3-a.**
      Result: Journeys:238 rewritten. The header-accessible-names read now cites "the Test-controls map's accessibility row" in place of R8.10b. The re-import commit cite now reads "then its E43 Import action (its R3.8k)" in place of "then its Import action (its R3.8g)" — verified against the import PRD: R3.8g is E14's "Take the new details" / "Keep what I have" (import PRD:120), and R3.8k is E43's "Import" commit (import PRD:124), matching IF13-m1's finding exactly. R8.9 dropped from the Rows cell, which now reads "R2.1, R4.3, R1.8".
  - "then its Import action (its R3.8g)" becomes "then its E43 Import action (its R3.8k)"; the import PRD's R3.8g is E14's choice, and R3.8k is E43's commit (IF13-m1).
  - Read the header accessible names "through the Test-controls map's accessibility row", not R8.10b.
  - Drop R8.9 from the Rows cell if it is there, since R2.1 carries the VoiceOver label (IF13-m5, PM13-5).
- [x] **D2 — UJ2.3-d.**
      Result: Journeys:241 rewritten. The Given's expected "Columns" list is removed. The Assert's "Columns" clause now reads "'Columns' offers exactly the columns Swatch Name, row state, L*, C*, h°, Spread, Swatch Alternate Code, Swatch Alternate Name and the two imported columns, the imported ones labelled State (imported) and Spread (imported), and neither the chip nor Swatch Code" — verified against R2.10 (PRD:310, "except the chip and Swatch Code") and UJ2.2-a (journeys:236, "neither the chip nor Swatch Code"), and it names built-in entries by column as UJ2.2-a does, quoting only the tagged labels. After hiding, the Assert now also states "Spread (imported) no longer shows" (TR m1). The SQL-read clause now reads "an SQL read, against one taken just before 'Set a field', shows only the column stored State changed" (IF13-m4, the database Minor).
  - **"Columns" (PM13-1, PMM13-1, IF13-2, R13-M1, TR13-1, DB13-MAJOR-3).** Remove the expected list from the Given. The Assert reads: "'Columns' offers exactly the columns Swatch Name, row state, L*, C*, h°, Spread, Swatch Alternate Code, Swatch Alternate Name and the two imported columns, the imported ones labelled State (imported) and Spread (imported), and neither the chip nor Swatch Code". That names built-in entries by column, as UJ2.2-a does, and quotes only the tagged labels.
  - **After hiding.** Also assert that "Spread (imported) no longer shows" (TR m1).
  - **The SQL read.** Take it just before "Set a field" and name the stored column: "an SQL read, against one taken just before 'Set a field', shows only the column stored State changed" (IF13-m4, a database Minor).
- [x] **D3 — UJ2.3-f and UJ2.3-g.**
      Result: UJ2.3-f's When (journeys:243) now ends "...type Nova, press Return, then fire E8's 'Apply to ⟨n⟩ swatches'" (PM13-4); its Assert reads "an SQL read shows only the column stored State changed" in place of the display-label form, matching D2's fix. Both UJ2.3-f and UJ2.3-g's fixtures now declare their items "captured, one live spectral reading" (TR Nit).
  - UJ2.3-f's When ends "type Nova, press Return, then fire E8's 'Apply to ⟨n⟩ swatches'" (PM13-4). Its Assert reads "only the column stored State changed".
  - Each item in both cases is declared "captured, one live spectral reading" (TR Nit).
- [x] **D4 — History and Compare cases.**
      Result: UJ5.3-r's current reading (journeys:394) now reads "an M1 value ... worked out at D65/10°, L* 50, C* 20, h° 60"; new CX4 added — "quarantined, retaining an M1 value L* 53, C* 23, h° 63 at D50/2° (the Data Foundation PRD's R3.3g)" — asserting "CX4 shows the copy file's R5.4 unreadable line" (TR m3, PM13-6, SSE13-4, R13-m2, TR m2). UJ5.4-d's non-spectral reading now reads "with an M1 value worked out at D65/10°", and it runs both selection orders (TR m4). UJ5.4-i's both readings now read "with M1 values worked out at D65/10°" (PM13-6, SSE13-4, R13-m2). UJ5.4-c, -e and -h's Asserts each gained ", with no label, read through the Test-controls map's accessibility row"; UJ5.4-f already carried "with no label" and was left untouched, per this box's own note. UJ5.3-b's Assert now reads "the copy file's R5.4 no-value line" in place of "the copy file's no-value distance line" (TR Nit).
  - **UJ5.3-r.** Its current reading is "an M1 value worked out at D65/10°". Add CX4: quarantined, retaining an M1 value at D50/2°, asserting "R5.4 unreadable" (TR m3, PM13-6, SSE13-4, R13-m2, TR m2).
  - **UJ5.4-d.**
    - Its non-spectral reading is "an M1 value worked out at D65/10°".
    - Run it in both selection orders (TR m4).
  - **UJ5.4-i.** Both readings are "M1 values worked out at D65/10°".
  - **UJ5.4-c, UJ5.4-e and UJ5.4-h.** Each Assert adds "with no label, read through the Test-controls map's accessibility row"; UJ5.4-f already carries "with no label" (PMM12-1 residual).
  - **UJ5.3-b.** Its Assert uses the identifier form, "the copy file's R5.4 no-value line" (TR Nit).
- [x] **D5 — The Test-controls map's accessibility row.**
      Result: Journeys:591's accessibility row now also reads "and the Compare slot's accessible text" (TR12-9, R13-m10, PMM12-1). Journeys only, no PRD-body word spent. It adds "each column header's accessible name and the Compare slot's accessible text" to what it reads (TR12-9, R13-m10, PMM12-1). This is journeys only.
- [x] **D6 — UJ9.5-d.**
      Result: Journeys:539's When now releases the outside read "then, after the 10th set, release the outside read" in place of the unscoped "then release the outside read" at the end of the sentence, matching its Given's "released after the 10th" (R13-m6). Its Assert's capture-timing clause now reads "...as the capture PRD's R11.7 reads them, in every set of both runs — ROW_CONFIRM_BUDGET being..." in place of "in the second run while the outside read is held" (SSE13-5, TR m8, ARCH13-5).
  - Its When releases the outside read after the 10th set, matching its Given (R13-m6).
  - Its Assert's capture-timing clause reads "in every set of both runs" (SSE13-5, TR m8, ARCH13-5).
- [x] **D7 — post-lock.md**, adding items under their triggers, in the file's style:
      Result: The ADR-0003 paragraph (post-lock.md, the item that carries F200/F202/DF F58/F59 forward) gained a fourth sentence, "Round 13 adds four more: ...", naming ARCH13-1/the database Minor 1 (defining "a read uses the journal or log" in SQLite's terms), the `temp_store=MEMORY` Nit, DJ3 (a)'s seeding-recipe Nit, and PRIV13-5's wording-alignment note. Under "### Data Foundation" two new items landed: a needs-owner item for ARCH13-6 (no named state for a write refused on another app's write lock), and a next-pass item for R13-m8 (a DJ3 run where the app's own Save a copy is read 2). Under "### Collection Mode" two new next-pass items landed: R13-m9 (the Vocabulary's "working set" against F217 and R2.3) and IF13-m6 (the rows' "can't-be-read line" against the copy identifier "R5.4 unreadable"). Under "## Dogfood" the existing E35-headline watch item gained "or a write of its own begun later (F215, DJ3 (e))" (PMM13-3); a new item on Compare's unlabelled Not compared lines landed beside it (PMM13-4).
  - **ADR-0003:**
    - "a read uses the journal or log" to be defined in SQLite's terms: any read begun before the file took in everything that journal or log holds (ARCH13-1, the round-13 database Minor 1);
    - `temp_store=MEMORY` or equivalent, so statement journals and sort spills never hold removed text on disk (an architecture Nit);
    - DJ3 (a)'s seeding recipe: hold a read on the log across the seeding write (an architecture Nit);
    - PRIV13-5's wording alignment.
  - **Next Data Foundation pass (needs owner):** a save refused because another app holds SQLite's write lock has no named state (ARCH13-6).
  - **Next Data Foundation pass:** a DJ3 run where the app's own Save a copy is read 2 (R13-m8).
  - **Next Collection Mode pass:**
    - the Vocabulary's "working set" (PRD:103) against F217's value-at-another-light, and R2.3's value-absent chip (R13-m9);
    - the rows' "can't-be-read line" against the copy identifier "R5.4 unreadable" (IF13-m6).
  - **Dogfood:**
    - E35's watch adds "or a write of its own begun later (F215, DJ3 (e))" (PMM13-3);
    - Compare's unlabelled Not compared lines, and whether "this reading" and "different light" read clearly (PMM13-4).
- [x] **D8 — Statuses (bookkeeping for round 13's dispositions).**
      Result: R8.1 flipped to "aligned" (PRD:424) — every non-abstaining lens (PM, SSE, TR, IF, ARCH, PMM, R13/plan, DB) chose ALIGN this round, only PRIV abstained, and the row's text is unchanged since `7c12387` (confirmed: no diff to R8.1's Requirement cell this pass). R2.1 and R8.8 stay needs-discussion — several lenses objected (R2.1: PM, TR, IF, PMM, R13, DB; R8.8: TR only), but only case/cite fixes landed this pass, no row-text change. R5.4 and R5.8 stay pre-alignment, rewritten by the orchestrator before this pass and still objected to by several lenses. DF R6.2/R6.2a stay ⌛️ Ready for Alignment, objected to by SSE, TR, R13 and DB (DJ3's cases), the rows' own text unchanged this pass. All changes listed in the review log's new Dispositions table (see A1).
  - **Flip to aligned:** R8.1. Every non-abstaining lens chose ALIGN in round 13, and the row is unchanged since `7c12387`.
  - **Keep at needs-discussion:** R2.1 and R8.8. Objected, but their rows changed only editorially: R2.1 a semicolon, R8.8 its cite.
  - **Keep at pre-alignment:** R5.4 and R5.8, rewritten by the orchestrator.
  - **Keep at ⌛️ Ready for Alignment:** DF R6.2 and R6.2a, since R6.2a was rewritten.
  - List every change.

## E. Checks

- [x] **E1 — Word counts.** Collection Mode must be ≤ 12,400 and Data Foundation ≤ 8,450, both unchanged by this pass.
      Result: measured by rule 14's method (`perl -0pe 's/<!--.*?-->//gs; s/```.*?```//gs; s/\]\([^)]*\)/]/g; s/^\|[-: |]+\|\s*$//mg; s/\|/ /g' FILE | wc -w`) after every fix above. Collection Mode: 12,397 of 12,400 — PASS, unchanged from the pre-pass figure (this pass's only PRD-body edit was R8.1's status-cell swap, which rule 1's exception exempts from the count). Data Foundation: 8,431 of 8,450 — PASS, unchanged (no Data Foundation PRD-body edit landed this pass).
- [x] **E2 — Mechanical checks.** Run checks 1, 2, 6, 7, 10, 11, 12, 13, 15, 16, 17 and 18, read-only, and report PASS or MISS.
  - Check 1c must find no bare sibling ID in a changed case.
  - Check 10 must count R2.1 at two sentences.
  - Check 13 must find no quoted label missing from the copy.
      Result:
      - **1 (cross-PRD, both directions):** PASS. Check 1c: grepped every ID token out of each changed case's row (`grep -oE '(^|[^`[:alnum:]./-])(R[0-9]+\.[0-9]+[a-z]?|E[0-9]+|M[0-9]+|F[0-9]+)\b'` over UJ2.3-a, -d, -f, -g, UJ5.3-b, -r, UJ5.4-c/d/e/h/i, UJ9.5-d) — every sibling ID is qualified, either by name in the same clause ("the import PRD's E14 ... (its R3.5), then its E43 Import action (its R3.8k)"; "the Data Foundation PRD's R3.3e/R3.3g") or by an antecedent already established in the same sentence, no bare sibling ID found. DF's F52–F61 range still agrees both directions (DF PRD:280, CM PRD:478, unchanged this pass).
      - **2 (index sync):** PASS. No Carried-by line was touched this pass — confirmed by `git diff` on both fences files showing only new lines appended, no `-` line under any `### F` heading — so the fence → row map is unchanged from round 12's confirmed 0-mismatch state.
      - **6 (case refs/copy coverage):** PASS. Every Rows cell touched this pass (UJ2.3-a "R2.1, R4.3, R1.8"; UJ2.3-d/f "R2.1, R2.10, R6.2"/"R2.1, R6.2"; UJ2.3-g "R2.1"; UJ5.3-b/r "R5.4"; UJ5.4-c/d/e/h/i "R5.8"; UJ9.5-d "R8.11, R1.8, R8.8") resolves to a live row ID; no copy state was added or removed this pass.
      - **7 (constants named in owning row):** PASS, unchanged — no constant touched this pass.
      - **10 (two-sentence scan):** PASS, 0 hits, re-run in full over every requirement cell with rule 14's exact perl pattern. R2.1's cell counts at exactly 2 sentences (verified directly: `perl -ne 'my $n = () = /[.!?]["\x27)\]]*(?=\s+["\x27(\[]*[A-Z]|\s*$)/g'` on R2.1's cell prints `2`) — the round-12 comma→semicolon fix resolved the round-12-reported 3-sentence residue.
      - **11 (companion paths/links):** PASS. No new Markdown link was added this pass; the Companions lines and every existing link are untouched.
      - **12 (word count):** see E1. Rule-migration scan: no new companion content section this pass beyond the Clarified-line additions in each fences file and the post-lock.md edits, and each carries its own fence or finding cite (F215/F61/IF13-m3/PRIV13-4/ARCH13-1/ARCH13-6/R13-m8/R13-m9/IF13-m6/PMM13-3/PMM13-4).
      - **13 (label check):** PASS. Every quoted string added or changed this pass ("Apply to ⟨n⟩ swatches", "Columns", "Set a field", "Compare", "Export collection", "Show history", "All items" — collected via `git diff | grep '^+' | grep -oE '"[^"]*"'`) is an established label already used unchanged elsewhere in the document; no new label was introduced, so none is missing from the copy file.
      - **15/16 (unresolved fill / guidance comments):** PASS, 0 hits, swept across the PRD, its four companions and post-lock.md.
      - **17 (banned adjectives in asserts):** PASS. No new hit; the one regex match (UJ4.7-b's "correction answer") is the pre-existing false positive rounds 11–12 already recorded, not "correct" as an adjective.
      - **18 (test-controls map / asserted values traced):** PASS by inspection for every case touched this pass. Every asserted identifier in UJ2.3-d (TT-001, TT-002) and every UJ5.3/UJ5.4 fixture ID (CX1–CX4, Q1, N1, O1) traces to its own case's Given (verified with the datum-tracing script against just these rows: no MISS). A pre-existing gap in the UJ3.4 "as X declares it" fixture-reuse idiom (28 lines, scenario 4, untouched by this pass) is unrelated to this pass — confirmed identical before and after this pass's edits by re-running the same script against the pre-pass `7c12387` text (28 MISS lines both times).
- [x] **E3 — The disposition tally for A1.**
      Result: Tally table built from the nine reviews' own final disposition tables and landed verbatim in the review log's new "### Dispositions" section (see A1). R8.1 is the only row that flips (unanimous non-abstaining ALIGN); every other row's OBJECT is accounted for in D8's status list, and nothing flips without a unanimous non-abstaining ALIGN.
