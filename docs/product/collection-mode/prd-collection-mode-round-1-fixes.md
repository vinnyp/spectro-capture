# Collection Mode PRD — round 1 fixes (owner adjudication of review round 1, 2026-09-24)

The resume point for the fix pass that applies the owner's round-1 decisions (fences F29–F57 in
`prd-collection-mode-fences.md`) to the findings of review round 1, before round 2. The fence file is the written
authorization for every change below: an earlier fence authorizes where it is cited, and "editorial/testability — no
new WHAT" marks a change that only adds a case, a readback, a cite, a fixture correction or a wording fix and changes no
rule. A change no fence names is not made: it goes back to the orchestrator as an unratified WHAT, and every **needs
owner** item waits for the owner's answer and its own dated fence (F58 on). Each box is ticked as its fix lands.

The findings are the nine round-1 reviews in `docs/agent-reviews/2026-09-24-prd-collection-mode-peer-reviews.md`
(Round 1 › Per-lens reviews), in log order; that log's verify-the-reviewer table records which Blockers and Majors were
accepted, merged or not reproduced. Each box carries the reviewer's own finding ID. The product-manager and
product-marketing lenses number none, so their boxes use severity and order in that review ("Major 2", "Minor 7"); the
agy pass's boxes use the Finding numbers of its disposition table. One box carries each fix — normally the first in log
order — and every other review raising the same defect gets its own box saying **covered by** that one. An item a review
raised only in its Missing, Deferred or clarifying-question lists gets a box, marked *unrated*, only where no rated
finding carries it. Sibling IDs name their document as in round 0 ("DF", "Capture", "Import", "Export", "Device"); an ID
written bare is this PRD's own.

Dispositions: **fix** (with the fence that authorizes it), **covered by**, **not reproduced**, **needs owner** (collected
under Owner-needed), and — for two Info items their reviewer marks as no finding — **no change**.

## Status bookkeeping

- [x] **Flip to aligned** (the log's flip record, less the holds below): E1, E2, R1.1, R1.2, R1.5, R1.6, R2.6, R5.1,
      R5.6, R8.7 — in the PRD's tables and the copy file's Status lines. R5.1 flips with F37's rename of the reading mark
      applied as wording only (SSE MJ2); R8.7 stays needs-discussion, and is listed, if SSE MN7's straddling answer lands in it.
      Result: all ten flipped — E1, E2, R1.1, R1.2, R1.5, R1.6, R2.6, R5.6 and R8.7 unchanged, R5.1 changed by F37's wording only, in the PRD tables and the copy file's Status lines; SSE MN7's straddling answer landed in R2.5, not R8.7.
- [x] **Sub-rows hold with their lead — flag to the orchestrator.** The log also lists R2.4c–h, R4.2, R4.2a/f/g/h, R5.2
      and R5.2a–c, but Traceability makes a lettered sub-row carry its lead's status (sub-row tables have no Status cell). R2.4
      is objected and R4.2b–e and R5.2d–e are rewritten below, so R2.4, R4.2 and R5.2 hold; these 15 are logged round-1-aligned.
      Result: flagged to the orchestrator in the pass report; R2.4, R4.2 and R5.2 were rewritten this pass, so they and their lettered sub-rows sit at pre-alignment.
- [x] **Objected → needs-discussion now** (the log's 77): R1.3, R1.4, R1.7–R1.10; R2.1–R2.5 (with R2.4a, R2.4b, R2.4i),
      R2.7–R2.10; R3.1–R3.8; R4.1, R4.2b–e, R4.3–R4.9; R5.2d, R5.2e, R5.3–R5.5, R5.7, R5.8; R6.1–R6.3; R7.1, R7.2;
      R8.1–R8.6, R8.8–R8.10; E3–E17; M1–M3.
      Result: the objected rows and states this pass leaves unchanged read needs-discussion — R1.4, R1.8, R2.1, R2.2, R2.7, R3.5, R3.6, R5.8, R6.3, R8.5, E7, E16; every other objected row, state and metric was rewritten.
- [x] **After the pass:** every row, state or metric the pass rewrites goes back to pre-alignment for round 2; one whose
      only open box is **needs owner** stays needs-discussion; new rows, states and metrics (the P0 selection row, the Flag
      and Rename-column confirmations, M4, any new OQ) enter at pre-alignment.
      Result: every rewritten row, state and metric reads pre-alignment; new R2.11, R3.9, R6.4, R8.11, R8.1a–f, R8.10a–f, E18, E19, M4 and OQ 12 enter at pre-alignment; no box is left waiting on the owner (F58–F90).

## peer-product-manager-reviewer (Claude route)

- [x] **PM Major 1 — undo model** (R4.7, R1.7, E10, T10, UJ6.2-f). **fix (F29):** R4.7 takes F29 — its own label "Undo
      change" in the copy file and Test-controls map, ended by any non-metadata action, re-checking R4.4/R4.8/R8.3 (refused
      with E15, E11 or E6), memory only, no Redo; add ARCH A1's cases. Is a collection rename (R1.3) undoable: **needs owner**.
      Result: R4.7 rewritten to F29 (its own label, ended by any other committed action, re-checks R1.3/R4.4/R4.8/R8.3, memory only, no redo) with F58's collection rename; "Undo change" on E3 and E14 and in the map; T10, UJ6.2-f and UJ6.4-a–k.
- [x] **PM Major 2 — a delete from All items never names the collection** (R1.10, R4.5, E10; DF E8). **fix (F32):** DF
      E8 reads "Delete ⟨code⟩ from ⟨collection⟩?" (DF half, dated DF fence); E10's body and "swatches" variant name
      ⟨collection⟩; UJ4.4-a asserts it; add an All-items delete of Gouache Set's ZX-001 (TEST m14).
      Result: the Data Foundation PRD's E8 headline names the collection (its F52); E10's body and "swatches" variant name ⟨collection⟩; UJ4.4-a, UJ4.4-e, UJ7.1-g.
- [x] **PM Minor 1 — changed items vs the active search, filters and sort** (R4.1, R8.3). **fix (F44):** a row re-checks
      an item that changes against the active search, filters and view sort, R4.1 and R8.3 keeping the settings, not the
      listing; add a case (a capture save leaving a "pending" filter).
      Result: new P0 R3.9; UJ9.1-c (a capture save under the pending filter) and UJ9.1-d.
- [x] **PM Minor 2 — single-row selection at P0** (R4.5, R4.1, R6.1). **fix (F45):** a P0 row (next free in §6, R6.4)
      selects one row; range, toggle and "Select all" stay P1 in R6.1; UJ4.4-c and UJ4.1-b cite the new row.
      Result: new P0 R6.4 selects one row and deselects on unlisting; R6.1 keeps range, toggle and "Select all"; UJ4.4-c and UJ4.1-b cite R6.4.
- [x] **PM Minor 3 — the collection-side Flag is unexplained** (R4.9, E14, T14, UJ4.7-a/c/d). **fix (F31):** a new
      confirmation state (next free E ID) owned by R4.9, worded per F31 and naming "Use this reading" as the way back (F23);
      the capture PRD's "Flag" label unchanged (F10); the cases and the Row-transitions Flag line pass through it.
      Result: new E18 owned by R4.9, naming Use this reading as the way back; the Row transitions Flag line and T14 pass through it; UJ4.7-a, UJ4.7-e.
- [x] **PM Minor 4 — Find similar results can't be acted on** (R3.7, E9). **fix (F46):** each listed item opens its item
      detail (R4.1); add a case opening a result from E9.
      Result: R3.7 opens each listed item's detail and E9's body says so; UJ3.4-e.
- [x] **PM Minor 5 — no displayed precision** (R2.1, R2.7, R4.2c, R5.4, E9). **fix (F47):** L*, C*, h° to one decimal,
      Spread and ΔE2000 to two, sorts on stored values; displayed values in UJ2.1-i, UJ3.4-a/d, UJ4.1-a, UJ5.3-a/b and UJ5.4-a
      follow (e.g. 0.40, 3.10, 1.00, 2.04, 2.86), the published four-decimal oracles kept in the Givens.
      Result: new P0 R2.11; displayed values in UJ2.1-i, UJ3.4-a/d, UJ4.1-a, UJ5.3-a/b and UJ5.4-a/b, the published four-decimal oracles kept in the Givens.
- [x] **PM Minor 6 — no way to cancel a rename** (R1.3, R4.8, R4.4; E7, E11, E15). **fix (F48):** R1.3 and R4.8 state
      Escape abandons a rename, keeping the old name; E15's "OK" becomes "Try another code", which R4.4 states returns to
      editing as E7's and E11's "Change the name" do; a case each.
      Result: R1.3 and R4.8 state Escape; E15's action is "Try another code", which R4.4 returns to editing; UJ1.1-b/c/e, UJ4.3-d, UJ4.6-b/g.
- [x] **PM Minor 7 — a re-import can split a renamed column** (R4.8, E11, T8, UJ4.6-a/e/f). **fix (F31):** a new
      confirmation state owned by R4.8, mirroring E16, warns that imports match columns by name; the cases pass through it.
      Column delete or merge as a non-goal: PM unrated 3.
      Result: new E19 owned by R4.8 ("Rename the column", "Keep this name"); T8 and UJ4.6-a/e/f pass through it; UJ4.6-h.
- [x] **PM Minor 8 — detail line labels and set-aside causes have no copy** (R2.1, R4.2b, R4.2d, R5.2, E14). **fix
      (editorial — no new WHAT; F37 for the settled words):** the copy file gains R2.1's headers and R4.2/R5.2's line labels;
      cause names cite the capture PRD's R8.2 words (Capture's copy holds no cause strings — reported, not added).
      Result: the copy file's Display labels section adds the Column headers, Detail lines and History lines tables; cause names cite the capture PRD's R8.2 words, and that Capture's copy holds no cause strings is reported, not changed.
- [x] **PM Minor 9 — E12's "Outside sRGB" misdescribes the file** (E12). **fix (F36):** F36's wording — the sRGB and HSL
      values here, in the file and in exports are the nearest sRGB colour; Lab, XYZ and spectral values are unaffected.
      Result: E12's Outside sRGB definition reads as F36 words it.
- [x] **PM Minor 10 — R1.7's release fate** (R1.7, OQ 10, Interim-stated). **fix (F57):** R1.7 and OQ 10 state that if the
      DF PRD's OQ 20 is open at v1 release R1.7 is deferred and v1 ships final deletes behind the counted confirmation and
      export first; the Interim-stated line cites F57.
      Result: R1.7 and OQ 10 carry F57's release fate; the Interim-stated line cites F57.
- [x] **PM Minor 11 — every metric is a synthetic per-PR guardrail** (M1–M3). **fix (F51):** new M4 — re-scans still
      unanswered a week after a dogfood session, target 0 — its observable the DF PRD's E26 count read a week on.
      Result: new M4, its observable the Data Foundation PRD's E26 count read a week on.
- [x] **PM Minor 12 — Capture R11.15g points at R8.10** (Capture R11.15g). **fix (F56):** repoint it to this PRD's copy
      E3 Actions line and Surfaces row (Capture half, dated Capture fence).
      Result: the capture PRD's R11.15g points at E3's Actions line and the Surfaces row (its F71).
- [x] **PM Nit 1 — §4's Traces claim on J3's closing step** (§4 Traces line). **fix (editorial):** narrow the claim to what
      F8 leaves — a note reaches an item only as an imported column.
      Result: §4's Traces line narrowed to a collection carrying an imported notes column (F8).
- [x] **PM Nit 2 — M2's population vs UJ9.8-a** (M2, UJ9.8-a). **fix (editorial/testability):** UJ9.8-a counts every item
      of the seeded file (15, Gouache Set's two included), as M2 defines it.
      Result: UJ9.8-a counts 15 items; M2's population names Gouache Set's two.
- [x] **PM Nit 3 — "narrowed" variants offer no clear actions** (R3.4, E3, E13). **needs owner:** R3.4 reads "Clear
      filters" and "Clear search" as available whenever narrowed, the copy offers them only on E4/E5 — add them to E3's and
      E13's "narrowed" variants, or narrow R3.4 to E4/E5?
      Result: (F59) E3 and E13 offer "Clear search" and "Clear filters", each while its own narrowing is active (R3.4); UJ2.1-g, UJ3.1-a, UJ7.1-b.
- [x] **PM Nit 4 — the zero-count rule blanks E13 and E17** (E13, E17, R4.2f). **fix (editorial):** reword E13's body so a
      file with collections but no items keeps a sentence. Is "Show history" offered on an item with no readings (it decides
      E17's zero headline): **needs owner**.
      Result: E13's body keeps a sentence with no items (UJ7.1-h); (F60) R4.2f withholds "Show history" with no readings, UJ5.1-e, so E17's headline never counts zero.
- [x] **PM Nit 5 — rename and delete on two surfaces** (Test-controls map, UJ1.1-a–d, UJ1.3-a/c; E3). **fix (log row 9 —
      editorial/testability):** the map's collection-surface line carries "Rename collection" and "Delete collection", and
      the UJ1 asserts read the collection surface.
      Result: the map's collection-surface line carries "Rename collection" and "Delete collection"; UJ1.1 and UJ1.3 fire them from the collection surface and read E3.
- [x] **PM Nit 6 — DF E33's "Export first" scope unstated** (DF E33). **fix (F56):** it reads "Export first saves all of
      ⟨collection⟩" (DF half, dated DF fence).
      Result: the Data Foundation PRD's E33 says Export first saves all of ⟨collection⟩ (its F52).
- [x] **PM Nit 7 — Capture R1.1 names one creation route** (Capture R1.1). **fix (editorial — the round-0 mirror, no new
      WHAT):** "for example" before this PRD's "New collection", Import R2.1/R3.7 also creating one (dated Capture fence).
      Result: the capture PRD's R1.1 reads for example, beside an import creating its target collection (its F71).
- [x] **PM unrated 1 (Missing) — moving an item between collections** (Build contract). **needs owner:** in v1, or a
      stated non-goal?
      Result: (F61) a Build contract non-goal, a re-import being the route.
- [x] **PM unrated 2 (Missing) — copying a value to the clipboard** (R4.2c). **needs owner:** in or out of v1?
      Result: (F62) a Build contract non-goal beyond the platform's text selection.
- [x] **PM unrated 3 (Missing) — column delete or merge** (R4.8, Build contract). **needs owner:** F31 warns of the split
      but no fence says whether removing or merging a column is out of v1 — state it as a non-goal?
      Result: (F63) a Build contract non-goal.
- [x] **PM unrated 4 (Missing) — owner ratification of the drafted interims** (OQ 2, OQ 7, OQ 11, Interim-stated). **fix
      (F34, F40)** for OQ 2's candidate and OQ 7's Bradford clause, the Interim-stated lines citing them; OQ 7's other
      clauses (relative colorimetric, "none reported" as sRGB) and OQ 11's interim: **needs owner**.
      Result: OQ 2 (F34), OQ 7 (F40 and F64) and OQ 11 (F64) and their Interim-stated lines cite the fences.

## peer-staff-software-engineer-reviewer (Claude route)

- [x] **SSE B1 — fixture ZX-013 is outside sRGB** (ZX-013, UJ2.1-b, UJ9.8-a, R2.4a, R2.5, M2, OQ 7). **fix (F40):** move
      ZX-013 inside sRGB (e.g. C* 30); every fixture keeps a declared margin from each gamut edge (ZX-001s, ZX-002; TEST m5),
      derived from DF R7.5's reference values (TEST B2); OQ 7's interim adds Bradford at zero tolerance; R2.5 adds F40's sRGB rule.
      Result: ZX-013 at C* 30 (sRGB margin 0.057 under Bradford); ZX-002 to C* 80, ZX-003 to C* 120 and Gouache Set's ZX-001 to C* 32 so every declared membership clears 0.03; the Harness states the margin rule; OQ 7's interim adds Bradford at zero tolerance; R2.5 adds F40's sRGB rule.
- [x] **SSE MJ1 — what the chip renders is guessable and unread** (R2.3, R5.2e, R8.10, UJ2.1-c). **fix (F40):** R2.3 — the
      working-set value colour-managed to the display, never DF's stored sRGB, clipped plus cannot-show outside its gamut;
      R8.10 reads back each chip's fill, value and display triplet with a clipped flag (TEST B1); ZX-002 cases on P3 and sRGB.
      Result: R2.3 renders the working-set value colour-managed, never the stored sRGB; R8.10c reads fill, value, triplet and clipped flag; UJ2.1-b/c assert ZX-002 clipped on sRGB and unclipped on Display P3.
- [x] **SSE MJ2 — "settled/unsettled" means two things** (Vocabulary, R2.4i, R4.2b, R4.2h, R5.1, R5.2d, R5.3, R5.7, E12,
      E17, T12, UJ4.1-c, UJ4.7-a, UJ5.1-a/c). **fix (F37):** the reading mark becomes "Awaiting answer" in the Vocabulary,
      rows, copy and cases; R4.2b uses Capture's "set aside for good" / "set aside, still to deal with"; settled stays Capture's.
      Result: awaiting answer in the Vocabulary, rows, copy and cases; R4.2b uses deliberately left or still to deal with, worded set aside for good / set aside, still to deal with in the copy file.
- [x] **SSE MJ3 — the Build dependencies stop rule contradicts its cells** (Legend › Build dependencies). **fix (F50):**
      split the last column into "Stops" and "Proceeds under interim"; the code-change, column-rename and column-visibility
      work stops on ADR-0003 (F50's inputs). Is ADR-0003 a stop for every row: **needs owner**.
      Result: Build dependencies keeps What must remain open as the stops column and adds Proceeds under interim; (F65) ADR-0003 stops every row, the cells naming F35's, F50's and F85's inputs — the column-set note is in the report.
- [x] **SSE MJ4 — undo: no label, no shared-stack rule, no failure path** (R4.7, R1.7, E10). **covered by PM Major 1**
      (F29, Redo none).
      Result: covered by PM Major 1.
- [x] **SSE MJ5 — an unlisted item stays selected** (R6.1, R8.3, R4.1, R4.5, R6.3). **fix (F44):** R6.1 — any change that
      stops listing a selected item (edit, capture save, restore, Flag, display move) deselects it; add the "sky" rename and
      the "pending"-filter capture cases.
      Result: R6.4 deselects on any change that unlists; UJ9.1-c, UJ9.1-d.
- [x] **SSE MJ6 — restore and answers during an in-flight session** (R8.3, R4.6, R5.5, R1.7, E6, Surfaces; Capture R5.6,
      R8.18). **fix (F30):** R8.3 refuses DF E4's restore, "Use this reading" and E10's "Undo" with E6 (E6 also on the version
      history view); correction answers stay; Capture R5.6/R8.18 mirror it (dated Capture fence); a case each via Capture R11.11.
      Result: R8.3 refuses both restores and E10's "Undo" with E6, E11 answers staying; E6 on the history view; the capture PRD's R5.6 and R8.18 mirror it (its F71); UJ4.4-g, UJ4.5-d, UJ5.2-d, UJ5.3-h.
- [x] **SSE MJ7 — no reference machine, no end event** (R8.1, M1, OQ 1, constants table, UJ9.5-a). **fix (F33, F41):**
      interim budgets gated on an M1 MacBook Air 8 GB, internal disk; the clock starts when a save lands, ends at the first
      frame showing the result; cold first open, warm browsing; M1 read every release on that Mac, per-PR a tripwire.
      Result: OQ 1's interim names the M1 MacBook Air, internal disk, cold first open, warm browsing and the release cadence; R8.1 starts a save when it lands and ends at the first frame; M1 rewritten.
- [x] **SSE MJ8 — envelope bounds missing** (R8.1, R8.2, R6.2, R6.3, M1, OQ 1, OQ 2, UJ9.5-a). **fix (F41–F43, F34):**
      R8.1 and UJ9.5-a add selection (single, range, all), detail/history open-close and scrolling (PERF-1); BULK_WRITE_BUDGET
      2 s; IMPORTED_COLUMNS_CEILING 20 × 200 chars; FILE_ITEMS_CEILING 100,000. The new dropped-frame constant: **needs owner**.
      Result: R8.1 becomes a lead with R8.1a–f (selection, detail and history, grid, saves, first rows, paging, bulk); BULK_WRITE_BUDGET 2 s, IMPORTED_COLUMNS_CEILING 20 × 200, FILE_ITEMS_CEILING 100,000 and (F66) DROPPED_FRAME_SHARE 1%; UJ9.5-a.
- [x] **SSE MN1 — what "one filter" is** (R3.4). **fix (editorial — UJ3.2-b/c already assert it):** R3.4 states that row
      states form one or-group and marks another, and-ed together.
      Result: R3.4 names the row-state filter and the mark filter.
- [x] **SSE MN2 — row-state, Spread and chip sorts; locale** (R3.2, R8.10, named defaults). **fix (testability):** the
      user's language becomes a declared R8.10 input with a named default (ARCH A9, TEST m3). How the row-state and Spread
      columns sort, and whether the chip column sorts: **needs owner**.
      Result: the user's language is an R8.10a input, named default English; (F67) R3.2; UJ3.3-i, UJ3.3-j.
- [x] **SSE MN3 — L*, C*, h° sorts across references** (R3.3, R1.9, R1.10). **fix (F54):** in the All items view each item
      shows its own collection's values; a sort orders items sharing the most common illuminant/observer, the rest following,
      counted (a copy line). A within-collection item carrying DF R3.3e's reference mismatch in these sorts: **needs owner**.
      Result: R1.9 and R3.3 carry F54 and (F68) the within-collection mismatch, E3 and E13 counting ⟨unlike⟩; UJ3.3-k, UJ7.1-i.
- [x] **SSE MN4 — ties and "Measured order"** (R3.7, R5.3). **fix (F46; editorial):** Find similar ties order by Swatch
      Code; R5.3 states "Measured order" runs oldest first, as UJ5.1-c asserts. How a restore orders against its source reading,
      which shares its measurement time: **needs owner**.
      Result: R3.7 ties by Swatch Code; R5.3 oldest first and (F69) ties by record time; UJ3.4-f, UJ5.3-i.
- [x] **SSE MN5 — re-entering the item's own code** (R4.4, E16). **needs owner:** is a code equal to the item's own under
      Import R2.3 (case or spacing only) stored without E16, as R1.3 does for names, confirmed, or refused?
      Result: (F70) R4.4 stores a self-equal code without E16; UJ4.3-g.
- [x] **SSE MN6 — "the item in view"; grid persistence** (R7.1, R7.2). **needs owner:** what "the item in view" is, and
      whether swatch size and the Grid/Table choice persist (in the file if so — F21, F53).
      Result: (F71) R7.1 defines the item in view; R7.1 and R7.2 last while the file is open and are written nowhere; UJ10.1-d, UJ10.1-e.
- [x] **SSE MN7 — display changes, custom profiles, straddling** (R2.5, R8.7, R8.10). **fix (F6 — worked out live):**
      cannot-show is re-worked whenever the showing display's gamut changes (a move, a profile or reference-mode change); the
      seam declares any reported gamut, three as fixtures (ARCH A10). Which display governs a straddling window: **needs owner**.
      Result: R2.5 re-works on any gamut change and (F72) follows the display macOS reports for a straddling window; R8.10a declares any gamut; UJ2.1-l, UJ2.1-m, UJ2.1-n.
- [x] **SSE MN8 — read-only actions hidden or disabled** (R8.4, UJ9.2-a). **fix (F49):** write actions show disabled on a
      read-only file and UJ9.2-a asserts disabled, not absent; F49 reaches read-only only — R2.9, R3.8, R4.9, R5.7 keep "not offered".
      Result: R8.4 shows write actions disabled; UJ9.2-a asserts disabled, not absent.
- [x] **SSE MN9 — other write failures** (R8.8). **needs owner:** name the states for an edit failing because the volume
      vanished, permission was lost, or another reader holds the file.
      Result: (F73) R8.8 names the capture PRD's E26 and the Data Foundation PRD's E10, and a permission-lost state that PRD owes (Outbound; its F52); UJ9.7-e, UJ9.7-f.
- [x] **SSE MN10 — an open detail whose item is gone** (R4.1, R8.5, E10). **needs owner:** what an open item detail or
      history view shows once its item is deleted (from the detail, or by a re-read).
      Result: (F74) R4.1 closes the detail and history; UJ4.4-h, UJ9.3-b.
- [x] **SSE MN11 — keyboard routes for drag and Compare** (R8.9, R2.9, R5.8). **needs owner:** name the keyboard equivalent
      of the drag reorder and of picking two readings for "Compare", or leave it to the build under R8.9 with a case each?
      Result: (F75) R8.9 names both routes, left to the build; UJ8.1-g, UJ5.4-b.
- [x] **SSE MN12 — bare sibling IDs in the journeys** (T2, UJ1.3-c/d/e, UJ4.4-b/d/e). **fix (editorial):** "E8's delete
      action", "E14's delete action" and UJ4.4-d's "E8" name the Data Foundation PRD.
      Result: every bare E8, E14 and E33 action in the journeys names the Data Foundation PRD.
- [x] **SSE MN13 — R8.10 can't read what cases assert** (R8.10, Harness, Test-controls map). **fix (log row 11 —
      testability):** R8.10 grants offered actions and fields, the selection and count, search text, active filters, first
      row and rows shown, token values, E9's list and not-compared count, All items rows, grid swatches and size (TEST MJ1, IF-5).
      Result: R8.10 becomes a lead with R8.10a–f granting actions offered and disabled, fields, selection and count, search text, filters, first row and rows shown, token values, E9's list, All items rows and the grid.
- [x] **SSE MN14 — Capture R8.18's "every tally"** (Capture R8.18, Capture UJ3.3-h). **fix (F55):** "counts as set aside"
      means collection tallies, not a session's (dated Capture fence).
      Result: the capture PRD's R8.18 counts the row in collection tallies only (its F71).
- [x] **SSE MN15 — no DF obligation for column visibility** (R2.10, Outbound table). **fix (F50):** an Outbound line to DF
      naming per-collection column visibility and renamed column names as things the file keeps and ADR-0003 inputs (DF half).
      Result: the Outbound Data Foundation line names per-collection column visibility and renamed column names as ADR-0003 inputs; R2.10 carries the marker.
- [x] **SSE MN16 — Capture R3.7's anchor for a deleted remembered row** (Capture R3.7, Capture E25). **fix (F55):** the
      session resumes at the first pending row (dated Capture fence); add a case after UJ4.4-d's delete.
      Result: the capture PRD's R3.7 falls back to the first pending row (its F71); UJ4.4-f.
- [x] **SSE MN17 — displayed precision** (R2.1, R2.7, R4.2c). **covered by PM Minor 5.**
      Result: covered by PM Minor 5.
- [x] **SSE MN18 — reorder while a search or filter is active** (R2.9). **needs owner:** is drag or "Use as scan order"
      offered while narrowed, and does "Use as scan order" order every pending row or only those listed?
      Result: (F76) R2.9 offers neither while narrowed; UJ8.1-h.
- [x] **SSE N1 — the Collection column's place** (R1.9, UJ7.1-a). **fix (editorial — the case already asserts it):** R1.9
      states the Collection column comes last.
      Result: R1.9 places the Collection column last.
- [x] **SSE N2 — Capture E1's "Change the name" on a rename** (R1.3). **fix (editorial — the label's one meaning):** R1.3
      states it returns to editing the name, as E7's does.
      Result: R1.3 says the capture PRD's E1 action returns to editing.
- [x] **SSE N3 — UJ9.8-a's 13 vs M2's 15** (UJ9.8-a, M2). **covered by PM Nit 2.**
      Result: covered by PM Nit 2.
- [x] **SSE unrated 1 (Deferred) — a flagged reading re-enters over-time views** (R5.3; DF R2.4, R2.5; Capture R5.6).
      **needs owner:** after a re-scan a flagged reading keeps reason initial and plots over time, not as never true — accept?
      Result: (F77) accepted — R4.9 says the flagged reading is not marked never true and UJ4.7-a asserts it; the post-lock entry for the Data Foundation owner is reported, not written (post-lock.md is outside this pass).
- [x] **SSE unrated 2 (Deferred) — Flag on an item with a pending correction question** (R4.9; DF R2.3c–e). **needs
      owner:** flagging while the current reading awaits the correction answer can leave both readings "wrong" — what rules?
      Result: (F78) R4.9 withholds the Flag while the current reading awaits the answer; UJ4.7-b adds ZX-013.
- [x] **SSE unrated 3 (Deferred) — does UJ3.4-d's Given replace or add to the default file** (UJ3.4-d). **covered by TEST MJ8.**
      Result: covered by TEST MJ8.
- [x] **SSE unrated 4 (Q30) — owner fences for OQ 2's, OQ 7's and OQ 11's interims** (OQ 2, 7, 11). **covered by PM unrated 4.**
      Result: covered by PM unrated 4.

## peer-test-reviewer (Claude route)

- [x] **TEST B1 — chip colour has no readback** (R2.3, R8.10, UJ2.1-b). **covered by SSE MJ1** (its readback also catches
      TEST B1's oldest-reading and D65 mutations).
      Result: covered by SSE MJ1.
- [x] **TEST B2 — ZX-013 declared inside sRGB** (ZX-013, UJ2.1-b, UJ9.8-a, M2). **covered by SSE B1.**
      Result: covered by SSE B1.
- [x] **TEST MJ1 — tokens, E9's list and offered actions unreadable** (R8.10, Test-controls map; Capture R11.15g).
      **covered by SSE MN13** and PM Minor 12.
      Result: covered by SSE MN13 and PM Minor 12.
- [x] **TEST MJ2 — undo's privacy clause is never tested** (R4.7, R8.10, T10, UJ6.2-c/f). **fix (F29, F35, F53 —
      testability):** in R4.7's phase, with undo still available and again after a crash before reopening, "Pink" is in neither
      the file's bytes nor the app's own storage (R8.10 grants both reads, PRIV-2); add an undo-order case and one per kind.
      Result: UJ6.4-j and UJ6.4-k read the bytes and the app's own storage with undo live and after a crash; UJ6.4-b orders two undos; UJ6.4-a, -c, -d, -e and UJ6.2-f cover each kind.
- [x] **TEST MJ3 — the in-flight guard is tested in two of four states** (R8.3, UJ1.2-a, UJ4.3-f, UJ4.4-c, UJ4.7-c,
      UJ6.3-c). **fix (log row 27 — testability):** run each guarded case, F30's new ones too, under active, operator-paused,
      guard-paused and device-halted sessions (Capture R11.6, Device R6.9).
      Result: the Harness's in-flight runs rule; UJ1.2-a, UJ4.3-f, UJ4.4-c, UJ4.7-c, UJ6.3-c, UJ8.1-f and F30's new cases run in each in-flight state.
- [x] **TEST MJ4 — state-changing actions in flight** (R5.5, R4.6, R4.7, R1.7). **covered by SSE MJ6** (F30) and PM Major 1
      (F29's session re-check on an undo).
      Result: covered by SSE MJ6 and PM Major 1.
- [x] **TEST MJ5 — cross-cutting cases run only in P0** (UJ9 preamble, UJ9.2-a, UJ9.4-a, UJ9.6-b, R8.4, R2.10). **fix (log
      row 28 — testability; F49):** re-run the read-only, network and keyboard cases in every phase with its actions; hiding a
      column on a read-only file is a disabled write (F49). The drag's keyboard route: covered by SSE MN11.
      Result: UJ 9's preamble re-runs UJ9.2-a/c, UJ9.4-a/c and UJ9.6-b in every later phase; UJ9.2-a lists hiding a column as a disabled write (F49).
- [x] **TEST MJ6 — "Export collection" is never fired** (R1.8). **fix (log row 29 — testability):** a case asserts Export E1
      at collection scope, and one with a search active still exporting the whole collection (F3).
      Result: UJ1.4-d and UJ1.4-e, the second with a search active exporting the whole collection.
- [x] **TEST MJ7 — UJ9.4-b's "zx-01 nowhere" fails a correct build** (UJ9.4-b). **fix (log row 30 — testability):** search a
      string no stored form holds (e.g. qqq) and assert it nowhere.
      Result: UJ9.4-b searches qqq and reads the file's bytes and the app's own storage.
- [x] **TEST MJ8 — the like-with-like exclusion is unisolated** (R3.7, UJ3.4-a, UJ3.4-d). **fix (log row 31 — testability):**
      declare Blues Two M1 D50/2° and whether UJ3.4-d's Given replaces the default file; add a D65/10° collection with an item
      within 1.0 of FS-000, unlisted from All items and counted not compared (ARCH A8); give FS-005 a value within 3.0.
      Result: UJ3.4-d's Given replaces the file and declares Blues Two M1 D50/2° and Daylight's DL-101 (D65/10°, FS-000's numbers), counted not compared; FS-005 declared within 3.0.
- [x] **TEST m1 — sort coverage holes** (R3.3, UJ3.3-c/d). **fix (testability):** fire the C* header; fire h° twice (greys
      stay last descending); items at C* 2.9 and 3.1 in a case's own Given.
      Result: UJ3.3-g fires C* and h° twice; UJ3.3-h holds C* 2.9 and 3.1.
- [x] **TEST m2 — constant boundaries untested** (R6.2, R3.7). **fix (testability):** BULK_CONFIRM_COUNT at exactly 10
      selected; FIND_SIMILAR_DISTANCE on the (48.5, 0, 0)/(51.5, 0, 0) pair at exactly 3.00.
      Result: UJ6.2-h at exactly 10 selected; UJ3.4-f at exactly 3.00.
- [x] **TEST m3 — undeclared seam inputs** (named defaults; UJ3.1-*, UJ9.4-a, UJ9.5-a, M1). **fix (testability):** declare
      every item's alternates, telemetry and update-check settings (Device R2.14), and M1's workload — query list, keystroke
      cadence, filters and headers round-robin (PERF-4) — as named defaults. The locale: covered by SSE MN2.
      Result: named defaults for alternates, language, network and update settings, and the timing workload.
- [x] **TEST m4 — UJ2.1-j's Given is unreachable** (UJ2.1-j). **fix (testability):** show the capture PRD's E23 end-early
      summary in place of E24.
      Result: UJ2.1-j shows the capture PRD's E23.
- [x] **TEST m5 — adaptation unpinned, thin margins** (OQ 7; ZX-001, ZX-002). **covered by SSE B1** (F40).
      Result: covered by SSE B1.
- [x] **TEST m6 — metric definitions vs their cases** (M2, M3, UJ9.8-a, UJ9.8-b). **fix (editorial/testability):** M3 and
      UJ9.8-b add the Flag (R4.9); UJ9.8-b runs UJ5.3-c in its own phase. M2's count: covered by PM Nit 2.
      Result: M3 and UJ9.8-b add the Flag (UJ4.7-a); UJ5.3-c runs in its own phase.
- [x] **TEST m7 — OQ 11 feeds more rows** (OQ 11). **fix (editorial):** its Feeds gain R5.4 and R5.8.
      Result: OQ 11 feeds R3.7, R5.4 and R5.8.
- [x] **TEST m8 — UJ9.3-a is weak** (R8.5, UJ9.3-a). **fix (testability):** two items selected with a sort and a filter set,
      one removed outside the app; assert the other stays selected and the sort and filter are kept.
      Result: UJ9.3-a removes ZX-011 and keeps ZX-013 selected, the search, both filter values and the L* sort.
- [x] **TEST m9 — crash-atomicity gaps** (R8.8, UJ9.7-b). **fix (testability):** pin the crash between the first and the
      11th item update (DF R7.4); add crash cases for a reorder and a restore.
      Result: UJ9.7-b crashes between the first and the 11th update; UJ9.7-c (reorder) and UJ9.7-d (restore).
- [x] **TEST m10 — accessible wording has no copy; R8.9 skips two marks** (R8.9, R2.4, R3.4, UJ9.6-a). **fix (F38):** a
      label table maps each mark identifier to its chip label, filter label and VoiceOver name, cited by R2.4, R3.4 and R8.9;
      UJ9.6-a asserts identifiers; R8.9's distinct shapes cover never-true and awaiting-answer too.
      Result: (F38) the copy file's Mark labels table, cited by R2.4, R3.4 and R8.9; R8.9 covers never-true and awaiting-answer; UJ9.6-a asserts identifiers.
- [x] **TEST m11 — cases or copy state rules the rows don't** (E5, R3.5, E9, R3.7, R1.9). **fix (editorial — copy honesty):**
      E5's ⟨n⟩ counts what the filters hide among what the search lists; R3.7 counts items with no current value as not
      compared, as E9 and UJ3.4-a do. The Collection column: covered by SSE N1.
      Result: the Placeholders rule sets E5's ⟨n⟩ basis; R3.7 counts an item with no current value as not compared.
- [x] **TEST m12 — "unsettled" means two things** (Vocabulary, E12, ZX-011/ZX-012). **covered by SSE MJ2.**
      Result: covered by SSE MJ2.
- [x] **TEST m13 — reorder edge cases** (R2.9). **fix (testability):** a case dragging a set-aside row (Capture E33's
      set-aside variant). Reordering while narrowed: covered by SSE MN18.
      Result: UJ8.1-i drags a set-aside row.
- [x] **TEST m14 — clauses and actions no case fires** (E7, E9, E11–E13, E15; R1.10, R3.6, R4.2e, R4.3, R5.5, R8.2, R8.3).
      **fix (testability):** a case per unfired action and per named clause (All-items delete, R3.6's independent state, the
      mismatch line, R4.3's code change, the restored value, a long history, R8.3's scroll). R8.1 at scale: PERF-2; R7.1: SSE MN6.
      Result: UJ1.1-c, UJ2.1-k, UJ3.4-b, UJ4.3-d, UJ4.6-b and UJ7.1-b fire the unfired actions; UJ7.1-g, UJ7.1-k, UJ4.1-g, UJ4.3-a, UJ5.3-j, UJ9.5-e and UJ9.1-a carry the named clauses.
- [x] **TEST m15 — two deletes in one window; ⌘Z vs the metadata history** (R1.7, OQ 10). **fix (editorial):** OQ 10 names
      two deletes in one window as part of DF OQ 20's question. ⌘Z: covered by PM Major 1 (a delete ends F29's history).
      Result: OQ 10 names two deletes in one window.
- [x] **TEST m16 — sibling halves untested** (DF E33, UJ6.3-d; Capture UJ3.3-h, Capture R8.18). **fix (testability, F7,
      F22):** UJ6.3-d asserts DF E33 for two never-scanned items; Capture UJ3.3-h names M3's observable; Capture R8.18's
      N_CONSEC_HARD clause gets a case or a line that it can't arise in flight (dated Capture fence).
      Result: UJ6.3-d asserts the Data Foundation PRD's E33 with no reading; the capture PRD's UJ3.3-h names M3's readback and its new UJ3.3-k tests R8.18's guard clause (its F71).
- [x] **TEST n1 — reversed-sort ties, whitespace search, straddling** (R3.2, R3.1, R2.5). **fix (testability):** a case of
      valued ties under a reversed sort. Is a search that normalises to nothing active: **needs owner**. Straddling: covered by
      SSE MN7.
      Result: UJ3.3-i reverses valued ties; (F79) R3.1 and UJ3.1-g.

## peer-interface-reviewer (Claude route)

- [x] **IF-1 — "Done" collides with Capture's Labels rule** (E9, E12, R2.8, R3.8). **fix (F39):** E9's and E12's dismiss
      is "Close", quoted by R2.8 and R3.8; a case fires it.
      Result: (F39) E9 and E12 dismiss with "Close", quoted by R2.8 and R3.8; UJ2.1-k, UJ3.4-b fire it.
- [x] **IF-2 — "Undo" has no copy home, collides with E10, can't refuse** (R4.7, E10, T10, UJ6.2-f). **covered by PM Major 1.**
      Result: covered by PM Major 1.
- [x] **IF-3 — rename and delete placed on two surfaces** (Test-controls map, UJ1.1-a–d, UJ1.3-a/c). **covered by PM Nit 5.**
      Result: covered by PM Nit 5.
- [x] **IF-4 — P0 honesty text has no copy home** (R2.4, R3.4, R8.9, E12, UJ9.6-a). **covered by TEST m10** (the label
      table) and PM Minor 8 (causes, line labels, headers).
      Result: covered by TEST m10 and PM Minor 8.
- [x] **IF-5 — R8.10 can't read what ~20 cases assert** (R8.10, Harness). **covered by SSE MN13** (the All items view, E9's
      list and the grid join its surface list) and PM Minor 12 (Capture R11.15g).
      Result: covered by SSE MN13 and PM Minor 12.
- [x] **IF-6 — restore ungated during a live session** (R5.5, R4.6, R8.3; Capture R5.6, R8.18). **covered by SSE MJ6.**
      Result: covered by SSE MJ6.
- [x] **IF-7 — Inbound fence cites name no document** (Inbound table). **fix (editorial):** "its F10", "its F4" and so on
      in every Inbound cite.
      Result: every Inbound fence cite reads as its sibling's, as do the §1, §4 and §5 Traces lines.
- [x] **IF-8 — E10's "Phase: none" vs OQ 10's interim** (E10, OQ 10). **fix (editorial):** E10's entry says it does not
      render until R1.7 is built (OQ 10, F57), with no new phase kind.
      Result: E10's entry says it does not render until R1.7 is built, with no new phase kind.
- [x] **IF-9 — R3.4's clear actions vs E3** (R3.4, E3). **covered by PM Nit 3** (needs owner).
      Result: covered by PM Nit 3 (F59).
- [x] **IF-10 — the selection-scale undo is not mirrored in DF** (R1.7; DF R6.3, DF E33). **fix (editorial — F3/F7 decide
      it):** OQ 10 records that DF OQ 20 must cover the selection scope DF E33 promises; no DF row changes (report if a DF note
      is wanted).
      Result: OQ 10 records the selection scope; no Data Foundation row changed — reported.
- [x] **IF-11 — ⟨n⟩ is overloaded** (Placeholders rule; E2, E3, E5, E8, E17). **fix (editorial):** the Placeholders rule
      defines ⟨n⟩ per state. E5's basis: covered by TEST m11.
      Result: the Placeholders rule defines ⟨n⟩ per state.
- [x] **IF-12 — the Device seam names R1.10 on one side** (Inbound Device line). **fix (editorial):** add R1.10.
      Result: the Device inbound line maps to R1.10.
- [x] **IF-13 — E8's "Cancel" unowned; actions never fired** (R6.2, E8). **fix (editorial):** R6.2 names E8's "Cancel".
      The unfired actions: covered by TEST m14.
      Result: R6.2 names E8's "Cancel".
- [x] **IF-14 — two quoted strings are not labels** (§8 preamble; Inherited obligations preamble). **fix (editorial):**
      unquote or reword "no requirement" and "the device PRD's `R6.27`".
      Result: both strings unquoted.
- [x] **IF-15 — R2.10 has no transition route** (Row transitions, Transition index, R2.10). **fix (editorial/testability):**
      a present → present route for hiding or showing a column, and its T-case.
      Result: a present → present route and T15.
- [x] **IF-16 — stale indexes** (Capture line 91; `docs/product/README.md` line 18). **fix (F56):** Capture's "Collection
      Mode remains unwritten" is corrected (dated Capture fence); the README row waits for the bookkeeping close.
      Result: the capture PRD's Legend note corrected (its F71); the README row waits for the bookkeeping close.
- [x] **IF-17 — Capture E1's "Change the name" on a rename** (R1.3). **covered by SSE N2.**
      Result: covered by SSE N2.
- [x] **IF-18 — E9 from the detail; an ambiguous headline** (E9, Surfaces, copy index). **fix (editorial; F5):** E9 is
      indexed to the item detail too, and from All items its headline names the collection, as F5 has each row do.
      Result: E9 indexed to the item detail too, its headline naming ⟨collection⟩.

## peer-privacy-reviewer (Claude route)

- [x] **PRIV-1 — removed text can survive in the file's bytes** (R4.3, R4.4, R4.8, R6.2, R6.3, R8.10, E8, "deleted"; DF
      R6.2a, R2.3). **fix (F35):** DF R6.2a/R2.3 cover anyone reading the bytes (DF half; ADR-0003 input); R8.10 reads the bytes
      and what sits beside the file; residue asserts on UJ6.2-c, UJ6.3-b, T1/UJ4.4-b, and T6/UJ4.2-a with a non-"Sky Blue" name.
      Result: (F35) the Data Foundation PRD's R2.3 and R6.2a cover the bytes (its F52); R8.10d; residue asserts on UJ6.2-c, UJ6.3-b, T1, UJ4.4-b, T6 and UJ4.2-a (renamed Harbour).
- [x] **PRIV-2 — the undo stack can leak** (R4.7, R8.10). **covered by TEST MJ2** (R4.7's memory-only wording: PM Major 1).
      Result: covered by TEST MJ2.
- [x] **PRIV-3 — the no-network check runs only offline** (R8.6, UJ9.4-a). **fix (testability — R8.6 unchanged):** with the
      network reachable and telemetry off (Device R2.14), drive every action this PRD adds; Device R6.17 records nothing beyond
      DF R1.4's permitted set and nothing carrying typed text; UJ9.4-a stays the offline case.
      Result: UJ9.4-c, network reachable and telemetry off, every action in each phase.
- [x] **PRIV-4 — a crash may not finalize a pending delete** (R1.7, Row transitions, T4, UJ1.3-e). **fix (F35 — testability,
      runs when R1.7 lands):** T4 and UJ1.3-e read the bytes before reopening; add a crash-while-undoable case; OQ 10 notes the
      crash route is restated if DF OQ 20 keeps pending content in the file.
      Result: T4 and UJ1.3-e read the bytes before reopening; UJ1.3-f crashes; OQ 10 notes the restatement.
- [x] **PRIV-5 — "not kept" checked only in the UI and the file** (R3.6, R2.10, R8.6, R8.10, UJ3.3-f, UJ9.4-b). **fix
      (F53):** R8.10 reads the app's own storage (preferences, container, saved window state); UJ3.3-f and UJ9.4-b assert the
      search text is nowhere there. UJ9.4-b's string: covered by TEST MJ7.
      Result: (F53) R8.10e; UJ3.3-f and UJ9.4-b read the app's own storage.
- [x] **PRIV-6 — no content limit for the Telemetry PRD** (R8.6, Outbound table). **fix (F52):** an Outbound line to the
      unwritten Telemetry PRD — no event about a Collection Mode action carries typed text, codes, names or values.
      Result: (F52) Outbound Telemetry line; R8.6 carries the marker.
- [x] **PRIV-7 (Info) — Export first exports the whole collection** (DF E33). **covered by PM Nit 6** (F56).
      Result: covered by PM Nit 6.
- [x] **PRIV-8 (Info) — hiding a column is not redaction** (R2.10). **no change:** the reviewer marks it not a finding (help
      docs sit outside this PRD).
      Result: no change.
- [x] **PRIV-9 (Info) — serials erasable only with the item** (R5.6, R8.2). **no change:** settled by AGENTS.md §4 and DF
      R6.1; the reviewer notes it and does not raise it.
      Result: no change.

## peer-product-marketing-manager-reviewer (Claude route)

- [x] **PMM Blocker — E12's "Outside sRGB" definition** (E12). **covered by PM Minor 9** (F36).
      Result: covered by PM Minor 9.
- [x] **PMM Major 1 — "Value missing" vs "No current value"** (E12, R2.4f, R2.4g, R3.4). **fix (F37; editorial):** "Value
      missing" becomes "Not in this condition"; "No current value" reads never scanned or set aside for a cause other than
      unreadable, as R2.4g states.
      Result: Not in this condition, and No current value's definition as R2.4g states it.
- [x] **PMM Major 2 — "Re-scan to answer" reads as an order** (E12, R2.4i). **fix (F37):** the label becomes "Re-scan
      unanswered", its definition pointing at answering from the swatch or with "Answer re-scans".
      Result: Re-scan unanswered, pointing at answering from the swatch or with Answer re-scans.
- [x] **PMM Major 3 — "not settled" on neighbouring screens** (R4.2b, E12, UJ4.1-c). **covered by SSE MJ2** (F37).
      Result: covered by SSE MJ2.
- [x] **PMM Major 4 — the legend shows no shapes and can't be reached from detail or history** (R2.8, E12, E14, E17,
      Surfaces). **fix (F38):** E12 shows each mark's shape beside its name, grouped (editorial), and explains Spread; "Colour
      marks" is offered in the item detail and the version history view, and E12's Surfaces cell lists both.
      Result: E12 grouped, shapes before names, Spread explained; "Colour marks" on E14 and E17; E12 indexed to both; UJ2.1-o.
- [x] **PMM Major 5 — Flag gives no word of its consequence** (R4.9, E14). **covered by PM Minor 3** (F31).
      Result: covered by PM Minor 3.
- [x] **PMM Major 6 — "Rename column" gives no import warning** (R4.8, E11). **covered by PM Minor 7** (F31).
      Result: covered by PM Minor 7.
- [x] **PMM Minor 1 — E6 reads as a queue; "held"** (E6). **fix (F30; editorial — copy honesty to R8.3):** E6 says the
      actions aren't available until the session ends and nothing changed, says "active, paused or halted" as DF E9/E15 do, and
      lists F30's restores and delete undo.
      Result: E6 reworded: not available until the session ends, active, paused or halted, F30's restores and delete undo listed.
- [x] **PMM Minor 2 — "Spacing and capitals don't count" overclaims** (E4, E11, E15). **fix (editorial — copy honesty to
      Import R2.3):** "Capitals and extra spaces don't count as a difference". The same phrase in the capture PRD's aligned E1:
      **needs owner**.
      Result: E4, E11 and E15 reworded; (F80) the capture PRD's E1 too (its F71).
- [x] **PMM Minor 3 — E5's ⟨n⟩ with a search active** (E5). **covered by TEST m11.**
      Result: covered by TEST m11.
- [x] **PMM Minor 4 — E8 never mentions undo; set vs clear** (E8). **fix (F29, F35 — copy honesty):** both variants end by
      saying what was there isn't kept in the file and can be undone while the file stays open.
      Result: both E8 variants end with the not-kept and undo sentence.
- [x] **PMM Minor 5 — E14's body is false on a read-only file** (E14, R8.4). **fix (F49):** a read-only variant of E14,
      enumerated by R8.4, saying nothing there can be changed.
      Result: (F49) E14's "read-only" variant, enumerated by R8.4.
- [x] **PMM Minor 6 — displayed text no copy writes** (R2.7, R4.2e, R5.4, E12). **fix (F38 for Spread; editorial):** E12
      explains Spread; copy entries for R4.2e's reference-mismatch line and R5.4's absent-and-marked distance.
      Result: E12 explains Spread; the Detail lines and History lines tables word the mismatch line and the not-compared distance.
- [x] **PMM Minor 7 — the "Unreadable" definition** (E12, R2.4h). **fix (F37):** it says the reading saved in the file is
      damaged.
      Result: Unreadable reworded.
- [x] **PMM Minor 8 — DF E33's export scope** (DF E33). **covered by PM Nit 6** (F56).
      Result: covered by PM Nit 6.
- [x] **PMM Minor 9 — E17 never says what "Use this reading" does** (E17, R5.5). **fix (editorial — copy honesty to R5.5):**
      a P1-phase sentence: it makes an earlier reading current again, and the reading it replaces stays in the history.
      Result: E17's body says what Use this reading does, where it's offered — the entry grammar has no sentence-level phase mark.
- [x] **PMM Nit 1 — E15's "OK"** (E15). **covered by PM Minor 6** (F48).
      Result: covered by PM Minor 6.
- [x] **PMM Nit 2 — E5's "passes"** (E5). **fix (editorial):** "No swatch matches these filters".
      Result: E5's headline reads No swatch matches these filters.
- [x] **PMM Nit 3 — E17's "most recently saved"** (E17). **fix (editorial):** "most recently recorded first".
      Result: E17 reads most recently recorded first.
- [x] **PMM Nit 4 — "no current colour" vs "No current value"** (E9, E12). **fix (editorial):** one term in both.
      Result: E9 says no current value.
- [x] **PMM Nit 5 — Device's "simulated badge"** (Device's Collection Mode obligation line; Device R6.5). **needs owner:**
      align it to "mark" in this change, or at Device's next amendment as the reviewer suggests?
      Result: (F81) no change here; left for the device PRD's next amendment.
- [x] **PMM unrated (Missing) — a dogfood reading of E12** (E12, M2). **needs owner:** add a one-time Cataloger read of the
      legend beside M4's dogfood session?
      Result: (F82) a one-line dogfood check beside M4.

## peer-architecture-reviewer (Claude route)

- [x] **A1 — undo can break uniqueness** (R4.7, T10, UJ6.2-f). **covered by PM Major 1** (F29; its two scenarios as cases).
      Result: covered by PM Major 1 — under F29 an import ends the history, so UJ6.4-g and UJ6.4-h are its two scenarios.
- [x] **A2 — a code change needs identity independent of the code** (R4.4, T7, UJ4.3-a, Outbound table, Build dependencies).
      **fix (F50):** an Outbound line and DF half — the item keeps its identity under a new code, an outside reader at
      SQLITE_READER_FLOOR seeing the same item; an ADR-0003 input; the row stops on ADR-0003. Report if Capture keys a row on the code.
      Result: (F50) the Outbound Data Foundation line and its R1.2 (its F52); R4.4 carries the marker; the Build dependencies row stops on ADR-0003; no capture row found keying a row on its code.
- [x] **A3 — which value the chip renders across the core–shell boundary** (R2.3, R8.10). **covered by SSE MJ1.**
      Result: covered by SSE MJ1.
- [x] **A4 — restoring during an in-flight session** (R5.5, R4.6, R8.3). **covered by SSE MJ6.**
      Result: covered by SSE MJ6.
- [x] **A5 — R8.4 widens DF's read-only floor** (R8.4, UJ9.2-a). **fix (F49):** a newer-format file (DF E1) gets DF R5.3's
      minimum, the rest left to DF OQ 18; "every mark" holds only for formats this build reads; UJ9.2-a splits accordingly.
      Result: (F49) R8.4 splits the read-only floors; UJ9.2-a (a format this build reads) and UJ9.2-c (a newer format).
- [x] **A6 — R8.1 presumes one navigation reading** (R8.1, UJ9.1-a). **fix (editorial — the Build contract's
      no-reading-selected rule, AGENTS.md §3):** a saved reading shows while the table is shown and whenever it is next
      shown; UJ9.1-a asserts at the next showing.
      Result: R8.1c times a save while the table is shown or at its next showing; UJ9.1-a asserts at the next showing.
- [x] **A7 — column visibility: no DF or ADR-0003 counterpart, R8.8, read-only** (R2.10, R8.8, R8.4). **fix (editorial;
      F49):** R8.8's atomic list gains column visibility; hide/show on a read-only file is a disabled write. The DF line and
      ADR-0003 input: covered by SSE MN15.
      Result: R8.8 lists column visibility; hiding on a read-only file is a disabled write (R8.4, UJ9.2-a).
- [x] **A8 — All items compares across working sets; Find similar computing a set** (R1.10, R3.3, R3.7). **fix (F46):** R3.7
      says Find similar never computes a new derived value set (DF R3.3f), so it writes nothing. The sort rule: covered by SSE
      MN3; the fixture: covered by TEST MJ8.
      Result: (F46) R3.7 computes no set and writes nothing.
- [x] **A9 — the locale is undeclared** (R1.1, R3.2, R8.10). **covered by SSE MN2.**
      Result: covered by SSE MN2.
- [x] **A10 — display gamut in the seam and in filtering** (R2.5, R3.4, R6.1). **covered by SSE MN7** (any reported gamut)
      and SSE MJ5 (F44).
      Result: covered by SSE MN7 and SSE MJ5.
- [x] **A11 — the interim file-wide ceiling is too low** (constants table, OQ 2). **covered by SSE MJ8** (F34).
      Result: covered by SSE MJ8.
- [x] **A12 — bulk writes are never timed** (R8.1, M1, UJ9.5-a). **covered by SSE MJ8** (F42) and PERF-2 (the UJ9.5-a bulk
      clear).
      Result: covered by SSE MJ8 and PERF-2.
- [x] **A13 — nothing proves a restore keeps its provenance** (R5.5; DF R2.3f). **needs owner:** does a restore carry its
      source's device snapshot, agreement verdict and spread (DF R2.3f's "equal to H")? If so, a DF clarification and a
      ZX-015 B1 restore case.
      Result: (F83) R5.5 and the Data Foundation PRD's R2.3f carry the provenance (its F52); UJ5.3-k restores ZX-015's B1.
- [x] **A14 — Capture R8.18 needs one source of truth** (Capture R8.18). **fix (F55):** the unreadable standing always
      follows DF's quarantine mark (dated Capture fence). What the counts show when damage is found on a read-only file:
      **needs owner**.
      Result: (F55, F84) the capture PRD's R8.18 follows the quarantine mark and shows the reported quarantine on a read-only file (its F71).
- [x] **A15 — Capture R11.15g points at the wrong row** (Capture R11.15g). **covered by PM Minor 12.**
      Result: covered by PM Minor 12.
- [x] **ARCH unrated 1 (Missing) — "no stored cannot-show" as an ADR-0003 input** (R2.5, Build dependencies). **needs
      owner:** add it to F50's ADR-0003 inputs?
      Result: (F85) R2.5's marker, the Outbound Data Foundation line, the first Build dependencies row and its F52.
- [x] **ARCH unrated 2 (Missing) — does All items search match imported values** (R1.9, R1.10, R3.1). **needs owner:** yes
      as R1.10's "as the collection surface does" reads, though the view shows no imported column — or no?
      Result: (F86) R1.10 says so.

## peer-performance-reviewer (Claude route)

- [x] **PERF-1 — selection, scrolling and opening an item have no budget** (R8.1, M1, UJ9.5-a). **covered by SSE MJ8** (F41,
      with PERF-1's UJ9.5-a samples).
      Result: covered by SSE MJ8.
- [x] **PERF-2 — saved-change budget: scope, start, scale** (R8.1, R2.9, R4.7, R4.8, R5.5, R6.2, UJ9.5-a). **fix (F42 —
      testability):** R8.1 says which writes each budget covers (report any unclassed); UJ9.5-a adds 200 Demo Device saves into
      a searched, filtered, sorted Scale and one ROWS_CEILING bulk clear. The start event: covered by SSE MJ7.
      Result: R8.1c and R8.1f class every write this PRD makes, none left unclassed; UJ9.5-a adds 200 Demo Device saves and a ROWS_CEILING bulk clear.
- [x] **PERF-3 — the timing environment is undeclared** (R8.1, M1, OQ 1). **covered by SSE MJ7** (F33, F41).
      Result: covered by SSE MJ7.
- [x] **PERF-4 — M1's workload; imported-column width** (M1, UJ9.5-a, R3.1, R2.1). **covered by TEST m3** (the workload) and
      SSE MJ8 (F43).
      Result: covered by TEST m3 and SSE MJ8.
- [x] **PERF-5 — FILE_ITEMS_CEILING's interim and closer** (constants table, OQ 2, UJ9.5-b). **covered by SSE MJ8** (F34).
      **fix (editorial):** OQ 2's closer becomes the owner's estimate of collections per file, checked by UJ9.5-b.
      Result: covered by SSE MJ8; OQ 2's closer is the owner's estimate of collections per file, checked by UJ9.5-b.
- [x] **PERF-6 — R8.2 and UJ9.5-b are underspecified** (R8.2, UJ9.5-b). **fix (F41; testability):** R8.2 names R8.1's inputs;
      UJ9.5-b declares the split across collections, their conditions and where Find similar fires. A budget for opening the
      All items view: **needs owner**.
      Result: R8.2 names R8.1a–c; UJ9.5-b declares ten collections, one at D65/10°, Find similar from All items; (F87) the open budget in R8.2.
- [x] **PERF-7 — a p95 budget used as a one-shot deadline** (R2.5, R3.1, UJ2.1-d, UJ9.1-a, UJ9.5-a). **fix
      (editorial/testability):** rows cite the 95th percentile; one-shot cases use a functional timeout, timing left to M1;
      UJ9.5-a adds display moves at ROWS_CEILING with the cannot-show filter on.
      Result: R2.5 and R3.1 cite the 95th percentile; UJ2.1-d and UJ9.1-a use a 5 s functional timeout; UJ9.5-a adds display moves under the cannot-show filter.
- [x] **PERF-8 — nothing stated above ROWS_CEILING** (R8.1, R8.2). **fix (F43):** above ROWS_CEILING or
      IMPORTED_COLUMNS_CEILING everything works and nothing is refused or truncated, the budgets not promised; a function-only
      smoke case at 2× ROWS_CEILING.
      Result: (F43) R8.1's lead; UJ9.5-c at twice ROWS_CEILING.
- [x] **PERF-9 — "first rows" is undefined** (R8.1). **fix (F41):** first rows are the first frame showing a screenful of
      rows with their chips and marks. Whether R8.1's other budgets hold from that moment (no lazy tail): **needs owner**.
      Result: R8.1d defines first rows; (F88) the other budgets hold from that frame.
- [x] **PERF-10 — no precedence for capture** (R8.3, R8.1; Capture R4.8, R4.13). **needs owner:** a row that no Collection
      Mode write or refresh delays an in-flight session's trigger acknowledgement or row confirmation past Capture's budgets?
      Result: (F89) R8.11; UJ9.5-d.
- [x] **PERF-11 — the grid has no budget** (R7.1, R7.2, R8.1, UJ9.5-a). **fix (F41):** R8.1's inputs reach the grid, and
      UJ9.5-a in R7's phase times the Table↔Grid switch and a swatch-size change.
      Result: R8.1a/b reach the grid; UJ9.5-a times Table↔Grid and swatch size in R7's phase.
- [x] **PERF-12 — R8.2 is missing from OQ 1's gate** (OQ 1, Interim-stated). **fix (editorial):** add R8.2 to OQ 1's Feeds
      and its Interim-stated line.
      Result: OQ 1's Feeds and its Interim-stated line include R8.2.
- [x] **PERF-13 — two budgets for one search rule** (OQ 1; Capture FIND_BUDGET, Capture OQ 13). **needs owner:** state
      FIND_BUDGET ≤ BROWSE_RESPONSE_BUDGET here and in Capture OQ 13's closer?
      Result: (F90) OQ 1 states it; the capture PRD's OQ 13 closer carries it (its F71).
- [x] **PERF-14 — in-memory undo grows** (R4.7). **fix (testability; F29, F42):** a case undoing a bulk clear over
      ROWS_CEILING items.
      Result: UJ9.5-a undoes a ROWS_CEILING bulk clear.

## peer-staff-software-engineer-reviewer (cross-model, agy)

- [x] **Finding-2 (Blocker) — bulk-setting Swatch Code** (R6.2). **not reproduced** (log row 5): the Vocabulary's editable
      field excludes Swatch Code (F7); no change. The fence file's Rejected findings holds owner rejections only — its "no
      review round has run yet" line is updated (editorial).
      Result: added to the fence file's Rejected findings as not reproduced, the stale line removed.
- [x] **Finding-1 (Minor) — All items search omits the collection name** (R3.1, R1.10). **fix (F46):** in the All items view
      search also matches collection names; add a case.
      Result: (F46) R1.10; UJ7.1-j.
- [x] **Finding-3 (Nit) — Find similar's tie-break** (R3.7). **covered by SSE MN4** (F46: ties by code).
      Result: covered by SSE MN4.

**Owner decisions, 2026-09-25.** Every box marked *needs owner* is authorized by the fence numbered 57 plus its position in the Owner-needed list below (F58–F90, approved as a set); apply each recommendation as that fence states it, and fill that fence's Carried by.

## Owner-needed

Each line: the box, the question, and a recommended answer.

- **PM Major 1** — Is a collection rename undoable by "Undo change"? *Recommend:* yes — it is name text like a column
  rename, re-checked against R1.3 on undo.
- **PM Nit 3 / IF-9** — Where are "Clear filters" and "Clear search" offered? *Recommend:* on E3's and E13's "narrowed"
  variants too, each only while its own narrowing is active, as R3.4 already reads.
- **PM Nit 4** — Is "Show history" offered on an item with no readings? *Recommend:* no, as "Find similar" is withheld
  without a current value; R4.2f says the item has no readings yet.
- **PM unrated 1** — Moving an item between collections? *Recommend:* a stated v1 non-goal in the Build contract; a
  re-import is the route.
- **PM unrated 2** — Copying a value to the clipboard? *Recommend:* no v1 row beyond platform text selection; export and the
  file are the data routes.
- **PM unrated 3** — Column delete or merge? *Recommend:* a stated v1 non-goal; F31's warning makes the split a knowing
  choice and hiding (R2.10) the mitigation.
- **PM unrated 4 / SSE unrated 4** — OQ 7's other interim clauses and OQ 11's interim? *Recommend:* ratify both as drafted.
- **SSE MJ3** — Is ADR-0003 a stop for every row? *Recommend:* yes — every row reads or writes the file whose schema it
  fixes; the OQ interims proceed alongside.
- **SSE MJ8** — The dropped-frame constant's candidate (F41)? *Recommend:* no more than 1% of frames missed while paging
  through ROWS_CEILING rows on F33's Mac, the interim equal to it.
- **SSE MN2** — How do the row-state and Spread columns sort, and does the chip column sort? *Recommend:* row state in
  lifecycle order (pending, captured, set aside), Spread by number, and the chip column does not sort.
- **SSE MN3** — Where does a within-collection item with DF R3.3e's reference mismatch go in L*, C*, h° sorts? *Recommend:*
  F54's rule — after the like-referenced items, counted.
- **SSE MN4** — How does a restore order against its source in "Measured order"? *Recommend:* ties on measurement time
  break by record time, the restore after its source.
- **SSE MN5** — A code equal to the item's own but for case or spacing? *Recommend:* mirror R1.3 — stored as entered, no E16.
- **SSE MN6** — "The item in view", and do swatch size and Grid/Table persist? *Recommend:* the selected item if on screen,
  else the first on screen; both last while the file is open and are written nowhere, as R3.6 does.
- **SSE MN7** — Which display governs a straddling window? *Recommend:* the one macOS reports the window is on (holding
  most of it).
- **SSE MN9** — Which states render for an edit failing on a vanished volume, lost permission or a held file?
  *Recommend:* cite Capture E26 and DF E10 where they apply; hand DF an obligation for a permission-lost state.
- **SSE MN10** — What shows once an open detail's item is deleted? *Recommend:* the detail and history close, returning to
  the table (E10 over it where R1.7 is built); the same after a re-read.
- **SSE MN11** — Keyboard routes for drag and Compare? *Recommend:* leave the mechanism to the build under R8.9 and add a
  keyboard case for each.
- **SSE MN18** — Reordering while a search or filter narrows the table? *Recommend:* neither drag nor "Use as scan order"
  is offered while narrowed.
- **SSE unrated 1** — A flagged reading re-entering over-time views after a re-scan? *Recommend:* accept for v1 (a Flag
  means scan again, not never true); log it for the DF owner on the post-lock list.
- **SSE unrated 2** — Flag while the current reading awaits the correction answer? *Recommend:* not offered until that
  question is answered (E11 sits in the same detail).
- **TEST n1** — Is a search that normalises to nothing an active search? *Recommend:* no — no narrowing and no "narrowed"
  variant.
- **PMM Minor 2** — The same spacing phrase in the capture PRD's aligned E1? *Recommend:* fix it in this change under a
  dated Capture fence, since Capture is already being amended.
- **PMM Nit 5** — Device's "simulated badge" wording? *Recommend:* leave it for Device's next amendment; its R6.5 owns the word.
- **PMM unrated** — A dogfood reading of E12? *Recommend:* a one-line dogfood check beside M4, no metric.
- **A13** — Does a restore carry its source's device snapshot, agreement verdict and spread? *Recommend:* yes — a DF
  clarification of R2.3f's "equal to H" and the ZX-015 B1 case.
- **A14** — What do the counts show when damage is found on a read-only file? *Recommend:* whatever quarantine DF reports
  for the open file, persisted or not; nothing is written.
- **ARCH unrated 1** — "No stored cannot-show" as an ADR-0003 input? *Recommend:* yes; F6 already keeps the mark live-only.
- **ARCH unrated 2** — Does All items search match imported values? *Recommend:* yes, as R1.10 reads; say so in R1.10.
- **PERF-6** — A budget for opening the All items view? *Recommend:* OPEN_COLLECTION_BUDGET for its first rows at
  FILE_ITEMS_CEILING.
- **PERF-9** — Do the browse budgets hold from the first-rows moment? *Recommend:* yes — no lazily loaded tail that a search
  waits on.
- **PERF-10** — Capture precedence over this PRD's writes and refreshes? *Recommend:* yes — add the row and a Demo Device
  case during a bulk clear; F14's in-session edits stay available.
- **PERF-13** — FIND_BUDGET ≤ BROWSE_RESPONSE_BUDGET? *Recommend:* yes — state it in OQ 1 and hand Capture a line at its OQ 13.

## Checks

- [x] **Testability pairing (process rule 3).** Every row, state and metric changed above has its acceptance case and its
      verification-seam line (R8.10, the Harness paragraph, the named defaults, the Test-controls map) updated in the same
      pass; each new state (the two confirmations) has a case per action, and M4 has its method.
      Result: every changed row, state and metric has its case and seam line; E18 and E19 have a case per action; M4 names its method.
- [x] **Word count** of the PRD body by rule 14's method (budget 12,000): strip HTML comments, link targets, code fences
      and table pipes (and table separator rows, as round 0b did); report the result (round 0b: 9,862).
      Result: 11,972 words by rule 14's method, table separator rows stripped (budget 12,000).
- [x] **Carried-by lines.** Replace every `_(filled by the round-1 fix pass)_` in F29–F35 and F37–F57, and the matching
      fence → row map lines, with actual IDs (this PRD's bare, siblings' after their document name); check F36's "E12" still
      covers what F36 touched. No placeholder left.
      Result: every placeholder in F29–F90 and the map filled; F36's E12 still covers it.
- [x] **Clarified lines.** F13 (F29) and F20 (F33, F34, F41–F43) carry theirs; add a dated line to F14 (F30 extends its
      list) and to any other earlier fence the pass finds now misstated (e.g. F6 by F40, F10 by F31), none re-deciding it.
      Result: F6, F8, F9, F10 and F14 carry 2026-09-25 lines; F20's existing line moved under F20.
- [x] **Sibling halves (process rule 12).** Each lands in this change with a new dated fence in its own file (next free:
      DF F52, Capture F71), Authority citing this PRD's fence and decision, alignment kept inline on aligned rows, and the
      status-line clause extended, peer review pending:
      - DF — F32 (E8 names the collection), F35 (R6.2a and R2.3 cover the file's bytes), F50 (identity across a code change;
        column visibility and renamed column names as ADR-0003 inputs), F56 (E33's "Export first" wording);
      - Capture — F30 (R5.6 and R8.18 refuse a restore while a session is in flight), F55 (R8.18's tallies and quarantine
        source; R3.7's fallback), F56 (R11.15g's pointer; line 91), with PM Nit 7's and TEST m16's editorial and testability items;
      - Telemetry — F52 lands in this PRD's Outbound table only; that PRD is unwritten.
      Result: DF F52 and Capture F71 landed with status-line clauses and dated clarifications (DF F17, F40, F50; Capture F9, F11, F15, F70); Telemetry through the Outbound table only.
- [x] **ADR-0003 inputs (F35, F50)** are carried in the Build dependencies "Stops" column and the DF half;
      `docs/decisions/README.md`'s 0003 row is not edited without the orchestrator's say-so — report.
      Result: carried in the Build dependencies stops cells and DF F52; docs/decisions/README.md not edited — reported.
- [x] **Constants and OQs.** The constants table gains BULK_WRITE_BUDGET (F42), the dropped-frame constant (F41),
      IMPORTED_COLUMNS_CEILING (F43) and FILE_ITEMS_CEILING's 100,000 (F34); OQ 1 (F33, F41, R8.2), OQ 2 (F34), OQ 7 (F40),
      OQ 10 (F57) and OQ 11 are updated; the Interim-stated list cites each fence; a new OQ takes number 12.
      Result: constants table and OQ 1, 2, 7, 10 and 11 updated; OQ 12 added for IMPORTED_COLUMNS_CEILING.
- [x] **Traceability.** "Owner decisions F1–F28" reads F1–F57; new IDs (the P0 selection row, the two confirmation states,
      M4, the column-visibility T-case) are assigned once, and nothing is renumbered.
      Result: F1–F90; new IDs R2.11, R3.9, R6.4, R8.11, R8.1a–f, R8.10a–f, E18, E19, M4, T15 and OQ 12, nothing renumbered.
- [x] **Standing label check** (check 13) re-run after the copy changes: "Close", "Try another code", "Undo change", F37's
      labels and the two confirmations' actions.
      Result: run — every quoted string in the PRD and journeys resolves to a copy label; E18's confirm is "Set aside to scan again", clear of the capture PRD's "Set it aside".

## Out of scope for this pass

- `docs/product/README.md` (index status, §6 paragraph, "Telemetry inherits") — at the bookkeeping close.
- Deleting guidance comments — at lock.
- The post-lock tick — round 0's box, when the PR exists.
- Any change a fence above does not authorize, and every **needs owner** item until the owner answers it.

## Round 1b — forks from the round-1 fix pass (fences F91–F98, 2026-09-25)

The same rules as above. Each box is ticked, with a Result note, as its fix lands.

- [x] **F91 — Build dependencies.** Remove the trailing "Proceeds under interim" column; keep the format's three columns (Work · Available contract · What must remain open) and prefix each item in the last column "Stop:" or "Interim:", carrying the same content. Confirm the table's header matches the format exactly.
      Result: the table has the format's three columns, its header matching the template exactly (Work · Available contract · What must remain open); every item in the last column is prefixed Stop: or Interim:, carrying the same content row for row, and the paragraph under it names the two labels.
- [x] **F92 — Escape abandons a code change.** R4.4 (and E15/E16 where they name the way back) states Escape abandons a Swatch Code change and keeps the old code; add a case.
      Result: R4.4 — Escape abandons a code change, the old code staying; UJ4.3-h (Escape while typing, and after E15's "Try another code"); the Test-controls map's item-detail line gains Escape during a code change. E15's and E16's copy name no way back, so neither changes.
- [x] **F93 — column renamed to itself.** R4.8 mirrors R1.3/F70: a new name equal to the column's own under the import PRD's R2.3 rule, differing only in case or spacing, is stored as typed with no E19; add a case.
      Result: R4.8 — a name equal to the column's own under the import PRD's R2.3 rule is stored as entered, with no E19; the Row transitions R4.8 route drops "and confirms E19", matching R4.4's line; UJ4.6-i.
- [x] **F94 — All items tie.** The row carrying F54 states the tie-break (the pair of the collection first in the collection list); add or extend a case.
      Result: R3.3 — in the All items view a tie goes to the tied pair of the collection first in the collection list; UJ7.1-l, whose fixture tells that rule apart from ordering by pair name or by the first sorted item. Scoped to All items, as F94 is; a tie inside one collection's table is reported, not decided.
- [x] **F95 — Capture E30.** Fix E30's spacing phrase as E1's was fixed (Capture F71's wording), recorded as a dated line under Capture F71.
      Result: Capture E30 reads "capitals and extra spaces don't count as a difference"; a dated line under Capture F71 closes its Not-decided E30 item, E30 keeping its alignment.
- [x] **F96 — cause names.** Add the set-aside cause names as labels in the capture PRD's copy file (one home, in that file's style), recorded under Capture F71; this PRD's R4.2b and item-detail copy cite them by document, never re-quote them as this PRD's own.
      Result: the capture copy file gains a Set-aside cause labels table — R8.2's eight causes, each label R8.2's words; R4.2b and this copy file's Detail lines cite it and the re-quoted list is gone; UJ4.1-c and UJ4.7-a read the cause by that label; dated line under Capture F71. Capture R5.8 and R6.6 quote the cause as "flagged: missing or damaged" — reported, not changed.
- [x] **F97 — E18 phase mark.** E18's "Use this reading" sentence carries `[phase: variant-absent]` (or the equivalent variant) until R5.5 lands, with the owning row enumerating it.
      Result: E18's Body drops the Use this reading clause and a new "restore" variant carries it, marked [phase: variant-absent]; Variants enumerated by R4.9, which names "restore"; UJ4.7-a asserts no variant while R5.5 is unbuilt, new UJ4.7-f the variant.
- [x] **F98 — decision queue and post-lock.** `docs/decisions/README.md`'s ADR-0003 row gains the five Collection Mode inputs (item identity kept across a Swatch Code change; per-collection column visibility kept in the file; renamed imported column names; deleted, cleared or replaced text gone from the file's bytes; no stored cannot-show mark), citing this PRD's F35/F50/F73 and DF F52. `docs/product/post-lock.md` gains, in its existing grouping and style, (i) the flagged-reading-over-time item for the Data Foundation owner (F77) and (ii) Data Foundation's owed permission-lost state (DF F52). Do not tick post-lock.md:94 yet — it is ticked with the PR number when the PR exists.
      Result: the ADR-0003 row gains the five inputs, citing F35, F50 and F85 and DF F52 — F85, not F73, carries the no-stored-cannot-show input (F73 is the permission-lost obligation, which went to post-lock) — reported; post-lock.md gains the two DF items under Next pass › Data Foundation; the rename item, now post-lock.md:97, is not ticked.
- [x] **Carried by** for F92–F98 and their map lines filled.
      Result: F92–F97 filled with their rows, cases and Capture IDs; F98 governs no rows, its halves landing in files that carry no IDs; the map lines match; Traceability reads F1–F98.
- [x] **Testability pairing** for every changed row; **word count** by rule 14 reported (budget 12,000 — the body stands at 11,972, so any addition must be paid for by trimming rule-free prose).
      Result: every changed row has its case and seam — R3.3 UJ7.1-l; R4.2b UJ4.1-c, UJ4.7-a; R4.4 UJ4.3-h and the map line; R4.8 UJ4.6-i; R4.9 and E18 UJ4.7-a, UJ4.7-f. 11,985 words by rule 14's method, separator rows stripped, paid for by cutting the §2 and §5 preamble hand-off sentences, both carried in full by the Inbound table.
