# Peer-review gate — prd-collection-mode (2026-09-24)

**Mode:** requirements. **Subject:** `docs/product/collection-mode/prd-collection-mode.md` and its four companions (`-journeys`, `-copy`, `-fences`, `-oq-results`), plus the sibling-PRD halves of its seams landed in the same change (Capture Mode, Data Foundation, Inventory Import, Data Export, Device Management). **Reviewers (round 1):** peer-product-manager-reviewer, peer-staff-software-engineer-reviewer, peer-test-reviewer, peer-interface-reviewer, peer-privacy-reviewer, peer-product-marketing-manager-reviewer, peer-architecture-reviewer, peer-performance-reviewer; cross-model: peer-staff-software-engineer-reviewer on agy. **persona-version:** cache/1.4.0. **Skill:** operator-agents:writing-agent-prds 1.7.0 via agent-dispatch:running-the-peer-review-gate 1.7.0.

**tier-rationale:** the four standing lenses of writing-agent-prds (product-manager, staff-software-engineer, test, interface) are the floor. Added by document shape: privacy — the rows display, edit and delete user-entered values and device snapshots (serials) and set what the user's file keeps; product-marketing-manager — the copy companion is end-user-visible text; architecture — the rows sit on the open capture→collection seam (ADR-0004), the queued schema (ADR-0003) and ADR-0001's Rust-core data layer; performance (Tier 3, data volume) — the operating envelope sets browse/search/sort/Find-similar budgets at scale. Not added: security (no auth, network or trust boundary — local-only, R8.6), database (no schema or SQL; ADR-0003 owns it), reliability (no long-lived service; durability is Data Foundation R1.10's), standards, release, devops, retrieval (no such shape).

Log of rounds: one `## Round N` section per round, appended; `log-new` is never re-run for this PRD.

## Round 0 — owner adjudication before round 1 (2026-09-24)

No lens ran. The owner decided F1–F4 before the fill, F5–F20 after it, and F21–F28 after the round-0 fix pass; each is a dated fence in `docs/product/collection-mode/prd-collection-mode-fences.md`. The round-0 fix list is `docs/product/collection-mode/prd-collection-mode-round-0-fixes.md` (sections Round 0 and Round 0b, every box ticked but the post-lock tick that waits for the PR number).

## Round 1 (2026-09-24)

**Lenses:** the eight above on the Claude Agent-tool route, in parallel; then one cross-model pass — peer-staff-software-engineer-reviewer on agy (Google Gemini via the Antigravity CLI), serialized. **Cross-model consent:** the owner chose "Yes — agy (Google Gemini)" when told the pass sends the brief and the files it names (this PRD, its fence file, copy strings and journey fixtures, and the sibling PRD diffs) to the runtime's external model provider (2026-09-24). **Subject commit:** `9acd883` (every lens and the cross-model pass reviewed this commit; sibling halves read as `git diff origin/main` at merge-base `dc1b747`).


### Per-lens reviews (verbatim; each ends with its per-row disposition table)

#### peer-product-manager-reviewer (Claude route)

## Verdict
Builds the right thing for the user. The product is the post-acquisition half of the Cataloger's job, and it sits squarely on vision U5 and U7. There are no Blockers, but two Major gaps should close before lock: the undo model, and deleting a swatch whose code also exists in another collection.

## User & problem context (brief)
- **User.** The primary user is the Cataloger. Secondary users are the Data consumer and the QC re-checker (vision.md Personas).
- **Job.** Once a heads-down capture is done, the user wants to see the whole collection honestly, find a swatch, fix a bad scan without losing history, and keep metadata tidy (U5, U7, J2 step 6).
- **Validated.** The owner decided F1–F28 in dated adjudications.
- **Assumed.** Everything else. There is no user research beyond the owner's own dogfooding, which is right-sized for a solo open-source tool. The performance budgets and the display-gamut behaviour are unmeasured and held open by OQ 1 and OQ 7.
- **Files read.** All four target files, the fence file, the round-0 fix list and the review log (it holds only the round-1 header, so no earlier findings exist). I also read the full sibling diff against origin/main and the sibling rows and copy cited below.

## Findings
[MAJOR] /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode.md:501 (R4.7), with R1.7 at :412 and E10 at prd-collection-mode-copy.md:190 — **the undo model breaks down as soon as a user mixes actions.**
- **What R4.7 says.** "Undo" reverses only metadata changes, most recent first.
- **What it leaves out.** Several other committed actions happen on the same surfaces, and none of them is on that history. No row says whether any of them ends it:
  - a delete (R1.4, R4.5, R6.3), whose own undo is E10's "Undo"
  - a Flag (R4.9)
  - a restore (R5.5)
  - a reorder (R2.9)
  - a re-scan answer (R4.6)
  - a collection rename (R1.3), which is not in R4.7's list either
- **The scenario.** A user sets ZX-001's name to "Sky Blue Light". Then they mistakenly Flag ZX-002, or delete ZX-010, and press ⌘Z, the macOS reflex. R4.7 reverses the name edit from before; the Flag or delete stands. There is no redo row, so the edit is lost without the user noticing.
- **A second problem.** "Undo" labels two different actions: E10's, and R4.7's, which has no home in the copy companion (this breaks the Labels rule). T10 and UJ6.2-f "Fire 'Undo'" without saying which one.
- **Fix (needs an owner decision; F13's scope stays as is).**
  - State that any committed action outside R4.7's list ends the metadata undo history, or that Undo is offered only while the most recent committed change is a metadata change.
  - Decide whether a collection rename is on the list.
  - Give R4.7's action a copy entry, and say which undo fires while E10 is up.

[MAJOR] prd-collection-mode.md:415 (R1.10) with **the Data Foundation PRD's E8** (/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/data-foundation/prd-data-foundation-copy.md:25) and E10 (prd-collection-mode-copy.md:189, 191–192) — **deleting a swatch from the All items view never names its collection.**
- **The scenario.** A cataloger has "Copic Sketch" and "Copic Ciao" collections. The two product lines share codes and names, which is realistic, and the harness's own ZX-001 fixture is this case. In the All items view, two rows "B12 Ice Mint" differ only in the Collection column.
- **What happens.** F5 and R1.10 allow a single-item delete through the item detail. The user opens one row and fires "Delete swatch". The Data Foundation PRD's E8 reads "Delete B12? This removes the swatch…" and names no collection. E10 then says "⟨code⟩ is deleted." E14's headline, "⟨code⟩ ⟨name⟩", is identical for both items.
- **Why it matters.** At P0 the delete is final, because R1.7 is gated on OQ 10. F2 listed "presenting the same Swatch Code in two collections" as still to specify. F5 settled the rows but not the confirmations.
- **What already works.** The detail body does name the collection (R4.2a). The gap is at the confirmation, which is the last check before the user loses data.
- **Fix.** Add ⟨collection⟩ to the Data Foundation PRD's E8 as a sibling-half amendment in this change (owner decision), and to E10's body and variants. Withholding delete from All items would reopen F5, so I don't recommend that route.

[MINOR] prd-collection-mode.md:479 (R4.1) and :580 (R8.3) — **no row says whether search, filters and view sort re-apply when an item changes.**
- R4.1 restores the search, filters, sort, selection and scroll "as they were before it opened". R8.3 shows a saved reading "without resetting" them.
- Neither row says what happens to an item whose edit or capture means it no longer passes the active search or filter. Nor do they say what happens when it moves under the sort.
- **Scenario.** A cataloger keeps the collection surface filtered to "pending" during a session to watch what's left. Each captured row either leaves the list, or stays as "captured" inside a "pending" filter, which makes the filter wrong. The builder has to guess.
- **Fix.** Add one row: whenever an item changes, the search, filters and view sort are re-applied to it. An item that stops passing leaves the list, and the selection per R6.1's rule; R4.1 and R8.3 preserve the settings, not which items are listed.

[MINOR] prd-collection-mode.md:499 (R4.5) — **single-row selection at P0 is carried by no row.**
- R4.5 deletes "on the one selected row", R4.1 restores selection, and UJ4.4-c and UJ4.1-b run in the first build phase.
- Yet the only selection row is R6.1, which is P1. A P0 builder has no stated way to select one row.
- **Fix.** Add single-row selection at P0, either to R4.1 or as a new row, and leave range and toggle selection at P1.

[MINOR] prd-collection-mode.md:503 (R4.9) — **the collection-side Flag removes a colour with nothing telling the user what it does.**
- **Context.** On macOS, "Flag" usually means "mark for attention" (Mail, Reminders). In capture, Flag fires seconds after a bad scan, so its meaning is obvious.
- **In the item detail it's different.** It is a standalone button that empties the chip, drops the captured count and sets the item aside. It has no confirmation, no copy explaining the consequence, and it is not on R4.7's Undo.
- **Recovery.** The only way back is R5.5's restore, reached through Show history, and it leaves a restore reading in history.
- **Fix (owner call).** F10 settles the label and the entry point; this fix adds to them. Add a one-line explanatory or confirmation state in this PRD's copy ("Set ⟨code⟩ aside to scan again? Its reading stays in history."), and name restore as the way back.

[MINOR] prd-collection-mode.md:467 (R3.7); prd-collection-mode-copy.md:180 (E9 Actions) — **Find similar results can't be acted on.**
- E9 lists codes and their ΔE2000 but offers only "Done".
- **Scenario.** In a cataloguing tool, the likeliest reason to run Find similar is hunting duplicates or substitutes. To open, compare or delete a match, the user must close E9 and retype the code into search. From the All items view they must also switch collection.
- **Fix.** Make each listed item open its detail (R4.1), or add an action that narrows the table to the result set.

[MINOR] prd-collection-mode.md:425 (R2.1) and :488 (R4.2c) — **no displayed precision is stated anywhere in the product docs** (checked by grep).
- **Scenario.** Two near-identical greys both show "L* 75" at integer rounding, while R3.3 sorts them by the stored value. The user sees an order they can't explain. A data consumer comparing against the vendor app sees different digits.
- The journeys imply integers for L*/C*/h°, one decimal for Spread, and four for E9's ΔE ("1.0000").
- **Fix.** Add a row (or copy-companion rule) giving the displayed precision for each quantity: L*, C*, h°, Spread and ΔE2000. The stored value stays unrounded and sorting uses it.

[MINOR] prd-collection-mode.md:408 (R1.3) and :502 (R4.8); copy E7 (:154–161), E11 (:194–202), E15 (:232–240) — **no rename has a stated way to cancel.**
- E7 and E11 offer only "Change the name", which returns to editing. No row says how to abandon a rename without entering a valid name.
- E15's "OK" closes instead, so the three rejection states follow two different patterns.
- **Scenario.** A user clears a collection's name by accident, gets E7, goes back to editing, and wants the old name back. No exit is specified, so a builder could loop the user in the dialog.
- **Fix.** State in R1.3 and R4.8 that Escape abandons a rename and keeps the old name, as R4.3 already does for fields. Make E7, E11 and E15 consistent.

[MINOR] prd-collection-mode.md:502 (R4.8) — **after a column rename, one re-import can split the data into two columns for good.**
- **Scenario.** The user renames "Family" to "Hue Family". Months later, re-importing the vendor's updated list (still headed "Family") appends a second Family column (Import R2.6, UJ4.6-f). New items' values land there; old ones stay in Hue Family.
- No row in any PRD deletes or merges a column (checked by grep), so the split is permanent. Hiding the column (R2.10, P2) is the only mitigation.
- E16 warns about exactly this class of problem for codes; column rename has no equivalent.
- **Fix.** Either add an E16-style note to the rename ("A spreadsheet that still says ⟨column⟩ will add it as a new column"), or name column delete/merge as a v1 non-goal in the Build contract so the owner accepts the split knowingly.

[MINOR] prd-collection-mode.md:487 (R4.2b) and :489 (R4.2d); copy E14 (:223–230); **Capture R8.2** (/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/capture-mode/prd-capture-mode.md, the row amended to add "unreadable") — **the item detail's line labels and set-aside causes have no copy.**
- E14 carries only a headline, a body and actions. The R4.2 line labels have no strings, and neither do the set-aside cause names.
- The cause names exist only as prose in Capture R8.2. This change adds "unreadable" there without adding a string to the capture copy file.
- **Scenario.** A builder renders internal vocabulary to the Cataloger, e.g. "Averaging basis: spectral curves · Agreement verdict: … · Derivation version: 3".
- **Fix.** Add a line-label and cause-name table to the copy companion, written in the user's words.

[MINOR] prd-collection-mode-copy.md:210 (E12) — **the "Outside sRGB" legend text misdescribes the file.**
- It says sRGB is "the reference your exports and your file use". In fact the file stores spectra and Lab/XYZ; sRGB is one derived space, and it is the one that gets clipped (the Data Foundation PRD's R3.4).
- **Scenario.** A data consumer reads this as "my file is limited to sRGB", which is the opposite of the measurement-grade thesis.
- **Fix.** "…outside standard sRGB, so its sRGB values in your file and exports are clipped to fit; its measured values are unaffected." The product-marketing lens co-owns the final wording.

[MINOR] prd-collection-mode.md:412 (R1.7) against the Legend at :237–238 — **it's unclear whether v1 ships without delete-undo.**
- The Legend says every row ships in v1 ("nothing droppable", F12).
- R1.7 is "not built until [OQ 10] closes", and OQ 10 is closed by the Data Foundation PRD's OQ 20, which could still be open at release.
- The PRD doesn't say whether R1.7 would then be `deferred` or block the release. That decides whether v1 users get final deletes.
- **Fix.** One sentence, as an owner call: if OQ 10 is open at release, R1.7 becomes deferred, or v1 waits.

[MINOR] prd-collection-mode.md:705–707 (M1–M3) — **all three metrics are per-PR guardrails on synthetic data.**
- Each one is sound: M1 is timing, M2 is mark agreement on the seeded fixture, M3 is readings lost.
- But none reads real use, so they don't show whether the Cataloger can find a swatch or fix a scan.
- **Fix, right-sized.** Add one dogfood signal next to the OQ 3/4/5 dogfood passes. For example, the number of re-scans still unanswered a week after a dogfood session (tests whether R5.7 and R4.2h are actually reachable), or the time to open a named swatch in a collection of ROWS_TARGET size. A funnel or OKRs would be over-engineering.

[MINOR] **Capture R11.15g** (/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/capture-mode/prd-capture-mode.md:376) — **the new cross-reference points at the wrong row.**
- It says Collection Mode's own actions on the collection surface "are listed under its R8.10".
- R8.10 is the test-seam row and lists no actions. The actions live in E3's Actions line (copy :123) and the Surfaces table.
- **Fix.** Repoint the reference to E3 or the Surfaces row.

[NIT] prd-collection-mode.md:472–473 (§4 Traces line) — **the claim that §4 serves "journey J3's closing step" is too broad.**
- F8 rules out a notes field, so a collection that imported no free-text column can't record "new marker". The only route is re-importing a CSV that has a notes column, and nothing tells the user that.
- **Fix.** Narrow the trace claim, or add a help/copy line explaining the route.

[NIT] prd-collection-mode.md:706 (M2) against journeys :344 (UJ9.8-a) — **the population doesn't match its oracle.** M2 says "every item of the harness's seeded file", which is 15 items; UJ9.8-a asserts over 13. Pick one.

[NIT] prd-collection-mode.md:464 (R3.4); copy E3/E13 — **the "narrowed" variants offer no clear actions.** "Clear filters" and "Clear search" appear only in E4 and E5. After E22's "Show simulated readings" narrows the table to 1 of 13, the way back is an unstated deselect inside "Filters".

[NIT] copy E13 (:219) and E17 (:256) — **the zero-count rule blanks two states.** It empties E13's body when the file holds collections but no items, and E17's headline for a never-scanned item. No row says whether "Show history" is offered on an item with no readings.

[NIT] journeys :370 (Test-controls map) against prd-collection-mode.md:377–378 (Surfaces) — **rename and delete sit on two different surfaces.** The map puts "Rename collection" and "Delete collection" on the collection list; the Surfaces table and E3 put them on the collection surface.

[NIT] **The Data Foundation PRD's E33** (prd-data-foundation-copy.md:27) — **"Export first" doesn't say what it exports.** It opens the whole-collection export (F24), but the copy says "an export carries each swatch's current reading", and a user who selected 5 swatches will expect only those.

[NIT] **Capture R1.1** (prd-capture-mode.md:138) — **the new wording names one creation route.** The dash clause says creating a collection is "the Collection Mode PRD's 'New collection' (its E1/E2)". Import R2.1 and R3.7 (prd-inventory-import.md:65, :82) also create a target collection mid-import. Write "for example", or name both entry points.

## Biggest risks   (what builds the wrong thing or fails the user)
1. **Undo.** Two undo mechanisms under one label, with no rule for actions that aren't metadata. ⌘Z will silently revert the wrong thing.
2. **Same-code delete.** Deleting from All items when a code exists in two collections, with confirmation copy that can't tell them apart. The delete is permanent at P0.
3. **Live changes against filters.** Whether an item leaves a search or filter when it changes is left to the builder. This is the collection surface's core promise while a session writes into it, and the capture→collection seam is still open.
4. **Delete-undo may not ship.** R1.7 can quietly miss v1 because its release fate if OQ 10 stays open is unstated.

## Genuinely solid
- **Behaviour, not solutions.**
  - The rows state what the product does, not how to build it.
  - Three rows name a mechanism rather than a behaviour, and all three are right:
    - R2.9's drag and R3.2's header-fire are the interaction the Capture PRD's R6.8 obligation already names.
    - R8.9's VoiceOver follows from macOS being decided.
  - R8.1's timing readback and R8.10 are test seams the agent-PRD format requires. They are not smuggled solutions.
- **Copy traces fully.** Every one of the 17 copy states comes from a row that puts the user there, and every variant is listed verbatim by its row.
- **Honesty marks.**
  - Keeping "cannot show on this screen" separate from "outside sRGB" (F6) is exactly right for P3-display Macs.
  - The OQ 7 interim, which treats a display that reports no gamut as sRGB, errs toward the honest side.
  - An empty chip is never a stand-in colour.
- **Destructive paths.** Delete is never the default, Return cancels, E6 guards a session in flight, E6's "interrupted" variant sends the user to the E25 resume offer, and export first returns to the confirmation. Few PRDs cover these unhappy paths this thoroughly.
- **Honest copy.** Empty results split into E4 (search) and E5 (filters), and E5 says "Clearing the filters keeps your search". E16 warns honestly that imports match on codes.
- **Fixtures check out.** I verified the fixture arithmetic, so a test reviewer can trust the oracles:
  - the sort orders in UJ3.3-b, -c, -d and -e and UJ7.1-e
  - 16 and 18 readings in the seeded file
  - the 10 / 2 / 1 tallies
  - the 11-item range
  - Find similar uses published CIEDE2000 pairs as its oracle.
- **Right-sized.** "Priority = build order, nothing droppable" is honest for a solo builder. There is no persona deck or funnel, and none is needed. The dogfood-closed OQs are the right feedback loop.
- **Sibling halves agree.**
  - Capture R5.6, R8.18 and R9.9 match the Data Foundation PRD's R2.9 ("only damage or the operator's Flag").
  - Import R2.6 matches R4.8 and UJ4.6-f.

## Missing / over-specified
- **Missing:**
  - column delete or merge (or an explicit non-goal)
  - moving an item between collections (non-goal?)
  - copying a value to the clipboard (in or out?)
  - displayed precision
  - single-row selection at P0
  - one dogfood outcome signal
  - owner ratification of the three agent-drafted interims (OQ 2, 7 and 11) before lock. OQ 7 is especially important because P0 R2.5 builds on it.
- **Over-specified:** nothing material. The fixture density is high but earns its keep. R8.7 is closer to a non-requirement statement than a behaviour, which is harmless.

| Row ID | disposition |
|---|---|
| R1.1 | ALIGN |
| R1.2 | ALIGN |
| R1.3 | OBJECT (rename cancel path) |
| R1.4 | ALIGN |
| R1.5 | ALIGN |
| R1.6 | ALIGN |
| R1.7 | OBJECT (release fate; undo model) |
| R1.8 | ALIGN |
| R1.9 | ALIGN |
| R1.10 | OBJECT (same-code delete) |
| R2.1 | OBJECT (display precision) |
| R2.2 | ALIGN |
| R2.3 | ALIGN |
| R2.4 | ALIGN |
| R2.4a | ALIGN |
| R2.4b | ALIGN |
| R2.4c | ALIGN |
| R2.4d | ALIGN |
| R2.4e | ALIGN |
| R2.4f | ALIGN |
| R2.4g | ALIGN |
| R2.4h | ALIGN |
| R2.4i | ALIGN |
| R2.5 | ALIGN |
| R2.6 | ALIGN |
| R2.7 | ALIGN |
| R2.8 | ALIGN |
| R2.9 | ALIGN |
| R2.10 | ALIGN |
| R3.1 | ALIGN |
| R3.2 | ALIGN |
| R3.3 | ALIGN |
| R3.4 | OBJECT (narrowed-state clear actions) |
| R3.5 | ALIGN |
| R3.6 | ALIGN |
| R3.7 | OBJECT (Find similar results) |
| R3.8 | ALIGN |
| R4.1 | OBJECT (live filter re-apply) |
| R4.2 | ALIGN |
| R4.2a | ALIGN |
| R4.2b | OBJECT (labels and causes without copy) |
| R4.2c | OBJECT (display precision) |
| R4.2d | OBJECT (labels and causes without copy) |
| R4.2e | ALIGN |
| R4.2f | ALIGN |
| R4.2g | ALIGN |
| R4.2h | ALIGN |
| R4.3 | ALIGN |
| R4.4 | ALIGN |
| R4.5 | OBJECT (single-row selection at P0) |
| R4.6 | ALIGN |
| R4.7 | OBJECT (undo model) |
| R4.8 | OBJECT (rename cancel path; column split) |
| R4.9 | OBJECT (collection-side Flag) |
| R5.1 | ALIGN |
| R5.2 | ALIGN |
| R5.2a | ALIGN |
| R5.2b | ALIGN |
| R5.2c | ALIGN |
| R5.2d | ALIGN |
| R5.2e | ALIGN |
| R5.3 | ALIGN |
| R5.4 | ALIGN |
| R5.5 | ALIGN |
| R5.6 | ALIGN |
| R5.7 | ALIGN |
| R5.8 | ALIGN |
| R6.1 | ALIGN |
| R6.2 | ALIGN |
| R6.3 | ALIGN |
| R7.1 | ALIGN |
| R7.2 | ALIGN |
| R8.1 | ALIGN |
| R8.2 | ALIGN |
| R8.3 | OBJECT (live filter re-apply) |
| R8.4 | ALIGN |
| R8.5 | ALIGN |
| R8.6 | ALIGN |
| R8.7 | ABSTAIN (platform floor; out of lens) |
| R8.8 | ALIGN |
| R8.9 | ALIGN |
| R8.10 | ABSTAIN (test seam; test/interface lens) |
| E1 | ALIGN |
| E2 | ALIGN |
| E3 | OBJECT (narrowed-state clear actions) |
| E4 | ALIGN |
| E5 | ALIGN |
| E6 | ALIGN |
| E7 | OBJECT (rename cancel path) |
| E8 | ALIGN |
| E9 | OBJECT (Find similar results) |
| E10 | OBJECT (same-code delete; undo model) |
| E11 | OBJECT (rename cancel path) |
| E12 | OBJECT ("Outside sRGB" legend) |
| E13 | OBJECT (zero-count blanks) |
| E14 | OBJECT (labels and causes without copy) |
| E15 | ALIGN |
| E16 | ALIGN |
| E17 | OBJECT (zero-count blanks) |
| M1 | ALIGN |
| M2 | OBJECT (population mismatch) |
| M3 | ALIGN |

#### peer-staff-software-engineer-reviewer (Claude route)

## Verdict
Proceed after addressing Blockers. The rows are unusually tight, and no row contradicts its fence. But one P0 acceptance fixture is colorimetrically false, so a correct build cannot pass it. Eight MAJOR gaps would each leave a builder guessing: what the chip renders, the "unsettled" vocabulary, stop-versus-interim gating, undo, selection, restore during a session, the budget's reference machine, and envelope bounds.

## What I reviewed
- **The PRD (requirements mode), all read in full:** `docs/product/collection-mode/prd-collection-mode.md` with `-journeys.md`, `-copy.md`, `-oq-results.md`.
- **Contract sources, all read:**
  - `-fences.md` (F1–F28)
  - `-round-0-fixes.md`
  - the review log `docs/agent-reviews/2026-09-24-prd-collection-mode-peer-reviews.md`
  - the sibling halves via `git diff origin/main -- docs/product ':!docs/product/collection-mode'` (merge-base `dc1b747`): Capture, Data Foundation (DF), Import, Export, Device
  - the relevant rows of the current Capture, DF, Import, Export and Device PRDs and copy files
  - `docs/product/vision.md`, `docs/product/README.md`, `docs/decisions/README.md` (ADR queue)
- **No product code exists** (AGENTS.md §7). The "real system" is therefore the sibling contracts and ADR-0001.
- **Checks I computed** (python3 one-liners, no files touched):
  - every harness fixture's sRGB and Display P3 membership, from its L*C*h° under D50/2°, with the ICC Bradford matrices, cross-checked with CAT02, von Kries and XYZ scaling
  - the five quoted CIEDE2000 pairs, with a reference implementation
- **Could NOT verify:**
  - the Sharma 2005 table itself (I checked arithmetic only)
  - real ColorSync or NSScreen gamut behaviour
  - any timing
  - OQ closures
  - anything needing a build

## Findings

**[BLOCKER] B1 — Harness fixture ZX-013 / R2.5 / R2.4a / M2 — a correct cannot-show test fails the P0 gate.**
- **The defect.** `prd-collection-mode-journeys.md:67` declares ZX-013 "L* 60, C* 40, h° 190; inside sRGB". Under D50/2° that colour is outside sRGB under every standard adaptation. Linear sRGB R is −0.0090 (Bradford), −0.0094 (CAT02), −0.0128 (von Kries) and −0.0208 (XYZ scaling). It is inside Display P3 (R 0.058).
- **The cases it breaks.** UJ2.1-b (`journeys:164`) lists ZX-013's marks as only re-scan-unanswered on an sRGB display. UJ9.8-a (`journeys:344`) with M2 = 100% (`prd:706`) require the same under sRGB and under "none reported" (treated as sRGB by OQ 7's interim, `prd:723`).
- **Mutation.** Implement R2.5 (`prd:446`) exactly per OQ 7's interim, with a standard CMM and zero tolerance. ZX-013 then gets cannot-show on the sRGB display, and both cases go red.
- **Why it blocks.** The only way to go green is an undocumented tolerance of at least 0.009 linear, or a non-standard conversion. An unattended builder would be trained to weaken the honesty test, which AGENTS.md §4 makes non-negotiable.
- **Root cause.** Neither R2.5 nor OQ 7 names the chromatic adaptation or a boundary tolerance, so this is an unnamed constant. Nothing requires the live mark to agree with DF R3.4's stored flag on an sRGB display.
- **Fix:**
  - Set ZX-013 to C* 30 (checked: linear R +0.057, inside), or re-declare it outside and update UJ2.1-b.
  - Add to OQ 7's interim: "adapt the working set's illuminant to the display white by Bradford; zero tolerance on encoded values", or name GAMUT_TEST_TOLERANCE with a candidate value.
  - Add to R2.5: "on a display reporting sRGB or none, cannot-show holds exactly when the stored gamut-clipped flag is set".
  - Give the near-boundary fixtures margin: Gouache ZX-001 has sRGB R +0.010; ZX-002 has P3 R +0.017.

**[MAJOR] MJ1 — R2.3 / R5.2e / R8.10 — what the chip renders is guessable, and never read back.**
- R2.3 (`prd:427`) says the chip renders "the item's current value in the collection's working set as the display showing it renders that value". The working set includes DF's stored sRGB derivation, which is clipped (DF R3.4, `data-foundation.md:145`). One engineer hands that sRGB triple to SwiftUI; another colour-manages Lab or XYZ into the display's space.
- On a P3 display, ZX-002 (inside P3) then renders clipped with no cannot-show mark — the silent "closest colour" the vision names as the problem (`vision.md:14`).
- R8.10 (`prd:587`) lists each chip's marks and accessible description but not its rendered colour. M2 checks marks only.
- **Mutation:** draw every chip from the stored clipped sRGB, or a constant grey. Every case and M2 stay green.
- **Fix:**
  - State in R2.3 that the chip renders the working-set colorimetry colour-managed into the display's space, never the stored sRGB derivation. Say what renders outside the display gamut (for example, the relative-colorimetric clip under OQ 7).
  - Grant in R8.10 a readback of each chip's rendered colour in a declared space.
  - Add a case: ZX-002 on Display P3 renders unclipped.

**[MAJOR] MJ2 — Vocabulary / R4.2b / R2.4i / R5.2d / R5.7 / E12 — "settled" and "unsettled" mean two unrelated things.**
- The Vocabulary imports the capture PRD's "settled" unchanged (`prd:175–176`). There, "unsettled" means a set-aside row not deliberately left.
- It then defines "unsettled" as a reading mark: the predecessor of a correction-unconfirmed reading (`prd:219`).
- R4.2b (`prd:487`) uses the capture sense; UJ4.1-c (`journeys:219`) asserts ZX-012 "not settled". R2.4i, R5.2d and R5.7 (`prd:442,523,532`) use the reading sense. E12's legend (`copy:210`) explains "Not settled: a later re-scan is waiting for your answer".
- A user sees "not settled" on ZX-012, which has no re-scan waiting. A builder can wire R5.7's "Answer re-scans" to unsettled set-aside rows.
- **Fix:** rename the reading mark in this PRD's Vocabulary, rows and E12 (for example "awaiting answer"). Keep capture's settled/unsettled for rows only.

**[MAJOR] MJ3 — Legend › Build dependencies — the "stop" rule contradicts its own cells.**
- The rule at `prd:310–312` says anything under "What must remain open" is a stop and the work waits.
- Yet row 1 (`prd:298`) and row 3 (`prd:300`) say the OQ 1, OQ 2, OQ 3 and OQ 7 "interims hold". ADR-0003's schema is listed on only two rows.
- "Code change, column rename and Flag — nothing" (`prd:304`) is false: R4.8 changes the stored column name, which is ADR-0003 territory.
- A builder cannot tell whether any P0 row may start.
- **Fix:** split the column into "Stops (wait)" and "Proceeds under interim". State once whether ADR-0003 stops every row, which AGENTS.md §7 implies.

**[MAJOR] MJ4 — R4.7 (with R1.7 / E10) — undo has no label, no shared-stack rule and no failure behaviour.**
- **No label.** The "Undo" quoted in R4.7 (`prd:501`) and in T10 (`journeys:97`) has no copy entry; the only "Undo" label is E10's delete undo. Nothing says whether ⌘Z undoes a pending delete or a metadata change, or in what order.
- **Undo can break uniqueness.** UJ4.6-f (`journeys:245`) leaves both "Hue Family" and a new "Family" column. Undoing the rename then yields two columns named Family. The same happens when undoing a code change after an import re-added the old code.
- **Undo can slip past the session block.** Undoing a code change mid-session is not in R8.3's refused list (`prd:580`), yet F14 blocks code changes during a session.
- **Stale targets are unhandled:** undo on a now-deleted item, and undo after R8.5's re-read.
- **Redo is unstated.**
- **Fix:** add a copy label and say how it shares ⌘Z with E10. Refuse (or name the state for) any undo that would violate uniqueness, target a missing item, or fall under R8.3 or R8.4. State whether Redo exists.

**[MAJOR] MJ5 — R6.1 × R8.3 / R4.1 — a selected item can drop out of the list and stay selected.**
- R6.1 (`prd:543`) deselects items only when a search or filter change unlists them.
- R8.3 (`prd:580`) keeps selection across capture saves, and R4.1 restores selection after closing the detail. Edits, bulk sets, restores and Flags can all unlist a selected item.
- Examples: filter "pending", select ZX-010, capture saves it. Or search "sky", select ZX-001, rename it.
- "Delete swatch" on the one selected row, "Set a field" at 10 or fewer items (no E8 confirmation), or "Delete selected" then act on rows the user cannot see.
- **Fix:** extend R6.1's deselect rule to any change that stops listing a selected item. Add a case.

**[MAJOR] MJ6 — R8.3 / R4.6 / R5.5 — restore and correction answers during an in-flight session are unspecified.**
- R8.3 refuses deletes, Change code and Flag. It says other *metadata* edits move no row state.
- Two restores are in neither list, and both move row state:
  - DF E4's "Use a previous reading" is P0 via R4.6 (`prd:500`), and DF E4 has no in-flight gate (`data-foundation-copy.md:23`).
  - "Use this reading" is P1 via R5.5 (`prd:530`).
- E11 answers are also unaddressed.
- F14 (`fences:306–312`) decided only deletes and code changes, so an owner call is needed, not a guess.
- **Failure scenario:** ZX-012 (set aside, unreadable) is the session's review row. It is restored from the collection surface, becomes captured, and the next trigger saves a set over a current value. Under DF R2.3b that raises an unintended correction question.
- **Fix:** ask the owner, record a fence, then list restore and answers either in R8.3's refused set (with E6) or as explicitly allowed with capture-side behaviour.

**[MAJOR] MJ7 — R8.1 / M1 / OQ 1 — the interim budget has no reference machine and no defined end event.**
- "Build and test against 100 ms" (`prd:278,717`) names no machine until OQ 1 closes. On the builder's M5-class machine it is not a gate.
- M1 (`prd:705`) and R8.1 (`prd:578`) end at "the table listing the result". That could be the model update or the first rendered frame, which differ materially for a 10,000-row table.
- **Fix:** have the interim name its gating machine (for example, the slowest Mac the owner has to hand, recorded in the readback). Define the end event as the first rendered frame showing the result.

**[MAJOR] MJ8 — §8 operating envelope — bounds the build must hit are missing.**
- The §8 preamble (`prd:569–574`) claims every class of envelope requirement is answered. These are not:
  - **Bulk operations:** no latency or progress bound for "Set a field" or "Delete selected" over a "Select all" at ROWS_CEILING (R6.1–R6.3).
  - **Detail and history:** no bound on opening an item detail or its version history, even though R8.2 says history is "however many".
  - **Scrolling:** no bound on scrolling or rendering the chip table at ROWS_CEILING.
  - **Width:** no bound on imported-column count.
- FILE_ITEMS_CEILING's interim equals ROWS_CEILING (`prd:280,718`). Any file with one full collection plus one more item exceeds it, while the research measured 100,000 rows.
- **Fix:** add one row per class, or state "no requirement, because…". Set the FILE_ITEMS_CEILING interim to a multiple of ROWS_CEILING.

**[MINOR] MN1 — R3.4 (`prd:464`).** "Values within one filter … separate filters with and" never defines what counts as one filter. That the nine marks form one OR-group and row state another is inferable only from UJ3.2-b and UJ3.2-c. Say it in the row.

**[MINOR] MN2 — R3.2 (`prd:462`).**
- The sort order of the row-state column, of Spread, and whether the chip column sorts at all are unstated.
- Capture R6.9 (`capture:236`) makes text comparison language-aware, but the harness named defaults (`journeys:51–78`) declare no locale.

**[MINOR] MN3 — R3.3 / R1.9 (`prd:463,414`).** L*, C* and h° sorts compare values taken at different references: a non-spectral reference mismatch (FS-005), and in All items, collections with different illuminants. This contradicts the like-with-like stance of R3.7, R5.4 and F27. Say where such items go, for example after the valued items, as R3.2 does for empty cells.

**[MINOR] MN4 — R3.7 / R5.3 (`prd:467,528`).**
- Find similar's tie order is unstated; UJ3.4-d contains a 1.0000 tie.
- "Measured order" direction (ascending) is given only by UJ5.1-c.
- A restore always shares its source reading's measurement time (T11), so it always ties with it. The tie-break, and how a restore appears in an over-time view, are unstated.

**[MINOR] MN5 — R4.4 / E16 (`prd:498`, `copy:248`).** Entering a case- or spacing-variant of the item's own code (or the same code) reaches E16. E16's warning that "a spreadsheet that still lists ⟨code⟩ will add it as a new swatch" is then false, because import matches under R2.3. Mirror R1.3's self-equal clause.

**[MINOR] MN6 — R7.1 / R7.2 (`prd:554–555`).** "The item in view" is undefined when many items are in view. Whether swatch size and the Grid/Table choice persist is unstated. If they persist, F21's principle means they belong in the file.

**[MINOR] MN7 — R2.5 (`prd:446`).**
- Cannot-show is re-evaluated only when the window moves. A display-profile change or an XDR reference-mode switch is not covered, and neither is which display governs a window straddling two.
- Custom calibrated ICC profiles are likely for this persona but untested; only sRGB, Display P3 and "none" are declarable.

**[MINOR] MN8 — R8.4 (`prd:581`).** "Not available" does not say whether read-only write actions are hidden or shown disabled; DF uses visible-but-disabled.

**[MINOR] MN9 — R8.8 (`prd:585`).** Only the full-volume failure is named. Volume disappearance (Capture E26, DF R7.3), permission loss, and an outside reader holding a lock are not cited for an edit.

**[MINOR] MN10 — R4.1 / R8.5 / E10.** What an open item detail or history view shows once its item is deleted (from the detail itself, or removed by a re-read) is unstated. E10 is also listed on the item detail surface.

**[MINOR] MN11 — R8.9 × R2.9 / R5.8 (`prd:586,450,533`).** "Every action … from the keyboard" requires a keyboard equivalent of the drag reorder and of selecting two readings for Compare. Neither is stated.

**[MINOR] MN12 — Journeys, cross-document cite rule.** Bare "E8's delete action" and "E14's delete action" at `journeys:89,148,149,150,233,235,236` mean DF's E8 and E14. Under this PRD's own rule a bare ID is this PRD's, and this PRD's E8 and E14 are the bulk-set confirmation and the Swatch detail. Qualify them.

**[MINOR] MN13 — R8.10 / Capture R11.15g.**
- Cases assert which actions are offered, whether a row is first on screen (UJ4.1-b), how many rows the window shows (UJ6.1-b) and swatch size (UJ10.1-b).
- R8.10 grants none of these readbacks; only the test-controls map names "which actions are offered".
- Capture R11.15g (`capture:376`) claims Collection Mode's actions "are listed under its R8.10", which does not hold. Add these readbacks to R8.10.

**[MINOR] MN14 — Capture R8.18 (`capture:297`).** "Counts as set aside in every tally" contradicts Capture UJ3.3-h (`capture-journeys:111`): "no session's own tallies count it deferred". Narrow it to the collection-state tallies.

**[MINOR] MN15 — R2.10 / Outbound obligations (`prd:451,604–608`).** There is no outbound obligation telling the DF PRD that the file keeps per-collection column visibility. It needs to sit within DF R6.5's inventory and R1.2/R7.1's readable-at-floor contract, since UJ2.2-a/b read it back.

**[MINOR] MN16 — Capture R3.7 (`capture:161`).** F14 allows deleting items under an interrupted session, including its remembered row. R3.7 falls back to "the next pending row in queue order" anchored on a row that no longer exists, while E25 says "You'll start again at ⟨code⟩". Name the fallback anchor.

**[MINOR] MN17 — R2.1 / R2.7 / R4.2c.** Displayed precision for L*, C*, h°, Spread and the six spaces is unstated, as is whether sorts use stored or displayed precision. UJ2.1-i asserts "0.4" and "3.1".

**[MINOR] MN18 — R2.9 (`prd:450`).** With a search or filter active: is drag offered, and where does a dragged row land relative to hidden rows? Does "Use as scan order" order all pending rows or only the listed ones? Each reading changes the persistent queue differently.

**[NIT] N1 — R1.9.** The Collection column's position (last) comes only from UJ7.1-a.

**[NIT] N2 — R1.3.** What the capture PRD's E1 "Change the name" does on a rename is unstated; E7's equivalent is.

**[NIT] N3 — UJ9.8-a (`journeys:344`).** It says "13 items" while M2's population is every item of the seeded file (15).

## Clarifying questions for the author
1. Does the chip render the working-set colorimetry colour-managed to the display, never DF's stored clipped sRGB?
2. Outside the display's gamut, does the chip show the relative-colorimetric clip?
3. Which chromatic adaptation (Bradford?) and which boundary tolerance (zero on encoded values?) does the cannot-show test use?
4. Must cannot-show equal the stored gamut-clipped flag on an sRGB or unreported display?
5. Should ZX-013 move to C* 30, or be declared outside sRGB with UJ2.1-b updated?
6. What word names the reading mark, so that "settled/unsettled" keeps the capture PRD's meaning?
7. Is ADR-0003 a stop for every row in this PRD?
8. While a session is in flight, are DF E4's "Use a previous reading", R5.5's "Use this reading" and E11 answers offered, or refused with E6?
9. Does ⌘Z undo a pending delete (E10), a metadata change, or both through one stack?
10. When an undo would recreate a taken name or code, or target a missing item, is it refused, and with which state?
11. Is there Redo?
12. Does a data change that unlists a selected item deselect it?
13. Which machine gates the interim 100 ms and 1 s budgets before OQ 1 closes?
14. Is M1's end event the first rendered frame showing the result?
15. What bound applies to bulk operations after a Select-all at ROWS_CEILING, to opening a detail or history, and to scrolling at ROWS_CEILING?
16. Should FILE_ITEMS_CEILING's interim be a multiple of ROWS_CEILING (for example 100,000)?
17. Are row states and marks exactly two filters: OR within each, AND between?
18. How do the row-state and Spread columns sort, and is the chip column sortable?
19. Where do reference-mismatched values go in L*, C* and h° sorts?
20. How are Find similar ties ordered, and how is a restore ordered against its source in "Measured order"?
21. Is a case-only change to the item's own code stored silently, confirmed with E16, or refused?
22. What is "the item in view", and are swatch size and the Grid/Table choice remembered (in the file)?
23. Does a display-profile change re-evaluate cannot-show, and which display governs a straddling window?
24. Are read-only write actions hidden or disabled?
25. Which state renders when an edit fails because the volume disappeared or the file is locked?
26. What does an open detail or history view show once its item is gone?
27. What is the keyboard equivalent of a drag reorder, and of selecting two readings for Compare?
28. How many decimals does each displayed value carry?
29. With a search active, does "Use as scan order" order every pending row or only the listed ones?
30. Will OQ 2's, OQ 7's and OQ 11's agent-drafted interims get owner fences before lock?

## Claimed properties
- **"Every open question carries an interim rule" — holds** (OQ 1–7, 10, 11; OQ 8 and 9 closed). OQ 2, 7 and 11's interims are unratified (Q30).
- **"Every constant … is also named in its owning row" — holds.** I checked all seven.
- **"No row chooses ADR-0003 or ADR-0006", and "neither seam reading is selected" — hold.** Under the takeover reading, R8.3 becomes vacuous rather than contradictory.
- **"Anything under What must remain open is a stop" — does not hold as written** (MJ3).
- **"Every copy state appears under exactly one surface unless shared" — holds.**
- **Harness gamut declarations — do not hold for ZX-013** (B1). Every other fixture checks out under the ICC Bradford path.
- **Round 0: "Export reads stored names; no Export change" — holds.** Export R2.4 and R2.4c use stored names.
- **"Sibling halves land in the same change" — holds for F7, F9, F10/F11, F22, F23, F24 and F25.** Verified in the diff: DF R1.2, R2.9, R6.2, R7.6o, E33, DJ2, DJ4, DJ5; Import R2.6 plus a case; the Capture rows and cases; Export §5 and F31; Device F32.
- **Capture R11.15g's pointer to CM R8.10 — does not hold** (MN13).
- **Capture R8.18's "every tally" — does not hold against UJ3.3-h** (MN14).
- **OQ 11's quoted pairs — hold arithmetically.** FS-001 1.0000, FS-002 2.0425, FS-003 2.8615, FS-004 3.4412, FS-101 1.0000, with kL = kC = kH = 1. Comparison against the published table itself is unverified.
- **M3 = 0 lost readings — structurally sound; unverified** without a build.
- **No row contradicts its fence (F1–F28) — holds.**

## Genuinely sound
- **R1.7 is withheld** behind OQ 10 (DF OQ 20). That correctly absorbs the identity-reuse hazards (renaming or re-coding into a deleted-but-undoable name). A zealot would demand those rules here; they belong to DF OQ 20.
- **The R4.8 rename seam** is complete on both sides. UJ4.6-e/f and Import's new case cover the old and new header paths, and Export already reads stored names.
- **Search reuses** Import R2.3's normalisation and Capture R6.1's find rule, with no parallel matcher. Sorts reuse Capture R6.9.
- **Fixture arithmetic is right everywhere I checked** except ZX-013:
  - every sort oracle: UJ3.3-a–e, UJ4.1-b, UJ7.1-e
  - the counts: 16 and 18 readings; 10/2/1 row states; an 11-item range; E33's 2 current and 3 earlier readings
  - every ΔE2000 value
- **Leaving out** notes fields, imported-column filters, library matching and selection export is right-sized, not a gap.
- **R8.6 reuses** the Device PRD's R6.12/R6.17 seams instead of inventing one.
- **The body is within its word budget**, and each seeded item exercises exactly one mark, which keeps the harness efficient.

## Deferred
- **peer-product-manager-reviewer:**
  - A flagged reading ("that one was wrong") becomes a legitimate over-time point after a re-scan (reason initial), not never-true.
  - A Flag on an item with a pending correction question can end with both readings "wrong".
  - E9 offers no way to open a listed item.
  - E33's copy doesn't say that Export first exports the whole collection. E12's "the reference your exports and your file use" is also an over-claim (a PMM wording point).
- **peer-architecture-reviewer:**
  - where colour management and gamut testing live across ADR-0001's Rust core / SwiftUI boundary (MJ1, B1)
  - where language-aware collation lives (MN2)
  - who owns the in-memory undo stack (MJ4)
- **peer-performance-reviewer:** whether a SwiftUI Table meets 100 ms at 10,000 rows. MJ7 and MJ8 only cover stating the bounds.
- **peer-test-reviewer:** the readback grants (MN13), the bare sibling IDs (MN12), UJ9.8-a's count (N3), and whether UJ3.4-d's Given replaces or adds to the default file.

| Row ID | disposition |
|---|---|
| R1.1 | ALIGN |
| R1.2 | ALIGN |
| R1.3 | OBJECT (N2) |
| R1.4 | ALIGN |
| R1.5 | ALIGN |
| R1.6 | ALIGN |
| R1.7 | OBJECT (MJ4) |
| R1.8 | ALIGN |
| R1.9 | OBJECT (MN3, N1) |
| R1.10 | ALIGN |
| R2.1 | OBJECT (MN17) |
| R2.2 | ALIGN |
| R2.3 | OBJECT (MJ1) |
| R2.4 | ALIGN |
| R2.4a | OBJECT (B1) |
| R2.4b | ALIGN |
| R2.4c | ALIGN |
| R2.4d | ALIGN |
| R2.4e | ALIGN |
| R2.4f | ALIGN |
| R2.4g | ALIGN |
| R2.4h | ALIGN |
| R2.4i | OBJECT (MJ2) |
| R2.5 | OBJECT (B1, MN7) |
| R2.6 | ALIGN |
| R2.7 | OBJECT (MN17) |
| R2.8 | ALIGN |
| R2.9 | OBJECT (MN18, MN11) |
| R2.10 | OBJECT (MN15) |
| R3.1 | OBJECT (MJ7) |
| R3.2 | OBJECT (MN2) |
| R3.3 | OBJECT (MN3) |
| R3.4 | OBJECT (MN1) |
| R3.5 | ALIGN |
| R3.6 | ALIGN |
| R3.7 | OBJECT (MN4) |
| R3.8 | ALIGN |
| R4.1 | OBJECT (MJ5, MN10) |
| R4.2 | ALIGN |
| R4.2a | ALIGN |
| R4.2b | OBJECT (MJ2) |
| R4.2c | OBJECT (MN17) |
| R4.2d | ALIGN |
| R4.2e | ALIGN |
| R4.2f | ALIGN |
| R4.2g | ALIGN |
| R4.2h | ALIGN |
| R4.3 | ALIGN |
| R4.4 | OBJECT (MN5) |
| R4.5 | OBJECT (MJ5) |
| R4.6 | OBJECT (MJ6) |
| R4.7 | OBJECT (MJ4) |
| R4.8 | ALIGN |
| R4.9 | ALIGN |
| R5.1 | ALIGN |
| R5.2 | ALIGN |
| R5.2a | ALIGN |
| R5.2b | ALIGN |
| R5.2c | ALIGN |
| R5.2d | OBJECT (MJ2) |
| R5.2e | OBJECT (MJ1) |
| R5.3 | OBJECT (MN4) |
| R5.4 | ALIGN |
| R5.5 | OBJECT (MJ6) |
| R5.6 | ALIGN |
| R5.7 | OBJECT (MJ2) |
| R5.8 | OBJECT (MN11) |
| R6.1 | OBJECT (MJ5) |
| R6.2 | OBJECT (MJ8) |
| R6.3 | OBJECT (MJ5, MJ8) |
| R7.1 | OBJECT (MN6) |
| R7.2 | OBJECT (MN6) |
| R8.1 | OBJECT (MJ7, MJ8) |
| R8.2 | OBJECT (MJ8) |
| R8.3 | OBJECT (MJ5, MJ6) |
| R8.4 | OBJECT (MN8) |
| R8.5 | OBJECT (MN10) |
| R8.6 | ALIGN |
| R8.7 | ALIGN |
| R8.8 | OBJECT (MN9) |
| R8.9 | OBJECT (MN11) |
| R8.10 | OBJECT (MJ1, MN13) |
| E1 | ALIGN |
| E2 | ALIGN |
| E3 | ALIGN |
| E4 | ALIGN |
| E5 | ALIGN |
| E6 | ALIGN |
| E7 | ALIGN |
| E8 | ALIGN |
| E9 | ALIGN |
| E10 | OBJECT (MJ4) |
| E11 | ALIGN |
| E12 | OBJECT (MJ2) |
| E13 | ALIGN |
| E14 | ALIGN |
| E15 | ALIGN |
| E16 | OBJECT (MN5) |
| E17 | ALIGN |
| M1 | OBJECT (MJ7) |
| M2 | OBJECT (B1) |
| M3 | ALIGN |

MJ3 targets the Legend's Build dependencies table, which carries no row ID.

#### peer-test-reviewer (Claude route)

## Verdict
Trustworthy after fixing Blockers. Almost every oracle I recomputed is correct, but two defects sit on the gamut-honesty core. One fixture declares a colour inside sRGB that is outside it, so a correct app fails and a dishonest one passes. And the chip's rendered colour has no readback at all. Eight Majors should also land before lock.

## Coverage map (brief)
- **Well covered:** 14 row-transition routes, with 14 matching T-cases. Every copy state and variant has a case. Search, filter and sort all have value oracles. Rename, delete, restore, re-scan answers and Flag have cases. Both halves of the rename and E33 seams are tested.
- **Load-bearing gaps:**
  - What a chip actually renders cannot be observed.
  - R8.10 grants no readback for copy-state token values, E9's contents or which actions a surface offers.
  - The in-flight session guard is tested in only two of its four sub-states.
  - Restore and undo during an in-flight session are neither specified nor tested.
  - R4.7's rule that undo never leaves prior values in the file has no case.
  - The envelope cases (read-only, network, keyboard) run only in the P0 phase, so every P1/P2 action escapes them.
  - "Export collection" is never fired.
  - Find similar's like-with-like exclusion has no case that tells it apart from other exclusions.

## Findings
Paths: PRD = `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode.md`; J = `…/collection-mode/prd-collection-mode-journeys.md`; C = `…/collection-mode/prd-collection-mode-copy.md`; CAP = `…/capture-mode/prd-capture-mode.md`; CAPJ = `…/capture-mode/prd-capture-mode-journeys.md`; DFC = `…/data-foundation/prd-data-foundation-copy.md`; DFJ = `…/data-foundation/prd-data-foundation-journeys.md`.

**[BLOCKER] B1 — PRD:427 (R2.3), PRD:587 (R8.10), J:164 (UJ2.1-b cites R2.3) — chip colour has no readback.**
- **What's missing:** R8.10 lets a test list "each chip's marks and accessible description". It does not grant what the chip renders:
  - the working-set value it was handed;
  - the display-space coordinates sent to the display;
  - even whether the chip is empty or filled, which cases assert ("empty chip", "shows a colour") with no granted observable.
- **Mutation that stays green:** render every chip from Data Foundation's stored derived sRGB value, which is clipped (its R3.4), instead of the working-set value colour-managed to the window's display. On Display P3, ZX-002 then renders clipped while correctly carrying no cannot-show mark. UJ2.1-b/c/d/e, UJ9.8-a (M2) and UJ10.1-a all stay green.
- **Other mutations that also pass:** a chip that renders the oldest reading instead of the current one, or renders under D65 instead of D50.
- **Why it matters:** this is exactly the dishonesty F6 created two separate marks to prevent (a colour outside sRGB can be shown faithfully on P3).
- **Fix:**
  - R8.10 grants a per-chip readback: empty or filled; the working-set value rendered (Lab plus illuminant, observer, condition and version); and the display-space triplet, with a clipped/unclipped flag.
  - Add a case: on Display P3, ZX-002's chip renders the unclipped P3 triplet of L*70 C*90 h°150; on sRGB it renders a clipped triplet and carries cannot-show.

**[BLOCKER] B2 — J:67 (ZX-013 fixture), asserted at J:164, J:333, J:344 (UJ9.8-a / M2), J:354 — ZX-013 is declared "inside sRGB" but is outside it.**
- **What I computed:** ZX-013's current value is L*60 C*40 h°190 under D50/2°. Its linear sRGB red channel comes out negative under every adaptation I tried:

  | Adaptation | Linear R |
  |---|---|
  | Bradford | −0.009 |
  | CAT02 | −0.009 |
  | von Kries | −0.013 |
  | XYZ scaling | −0.021 |
  | Lab read as D65 | −0.021 |

  Encoded, that is about −30/255, so it is not a rounding artefact. The colour is inside P3 (R = 0.058).
- **Consequence:** on the harness's default sRGB display, a correct R2.5 marks ZX-013 cannot-show. A derived gamut-clipped flag would also add outside-sRGB. But UJ2.1-b lists only re-scan-unanswered, and M2 reads below 100%.
- **Mutation the suite rewards:** "cannot-show = the stored clipped flag when the display reports sRGB or none; live test otherwise". It passes UJ2.1-b/c/d/e and UJ9.8-a, while the correct implementation fails them. In an unattended build gated on this suite, that pushes the builder toward the dishonest version.
- **Fix:**
  - Either move ZX-013 inside sRGB with margin (at L*60 h°190, C*≤32 gives R ≥ 0.044), or keep it and assert both gamut marks.
  - Derive every inside/outside declaration from Data Foundation's R7.5 checked-in reference values, not prose.

**[MAJOR] MJ1 — PRD:587 (R8.10), J:371–375 (test-controls map), CAP:376 (Capture R11.15g) — readback for copy-state contents and offered actions isn't granted.**
- **What R8.10 grants:** only "which copy state and variant is up, without matching wording".
- **What it doesn't grant:**
  - rendered token values (⟨n⟩, ⟨collection⟩, ⟨code⟩, ⟨shown⟩, ⟨left⟩, ⟨excluded⟩);
  - E9's listed swatches, with their ΔE2000 and collection;
  - the actions (or, for Set a field, the fields) a surface offers. The test-controls map names this, but the row doesn't — and Capture R11.15g now says Collection Mode's actions "are listed under its R8.10".
- **What depends on it:** about 40 asserts. Examples: E2 "2 collections", E3 "1 of 13", E8 "Family… 11 swatches", E17 "1 reading left out", all of UJ3.4-a/d's output, UJ7.1-c, UJ8.1-e, UJ9.2-a and UJ6.2-e.
- **Risk:** a harness built to R8.10 cannot run these without matching wording, which R8.10 forbids. The mutation "E8 counts rows the search hides" then goes unseen.
- **Fix:** R8.10 adds token values, E9's list and not-compared count, and offered actions and fields.

**[MAJOR] MJ2 — PRD:501 (R4.7) — undo's privacy clause is never tested.**
- **The clause:** "never kept where an outside reader… can recover it" (Data Foundation R6.2d / F17; E8's "clear" copy makes the same promise).
- **The gap:** no case covers it in the phase that lands undo. UJ4.2-c checks it only in the P0 phase, before any undo stack exists.
- **Mutation:** keep the undo stack in a table in the file → T10, UJ6.2-f and everything else stay green.
- **Also untested:** most-recent-first order, undo of a field edit, a code change or a column rename, and undo ending when the file closes.
- **Fix:**
  - After UJ6.2-c's When, in the R4.7 phase, assert the text "Pink" is nowhere in the file (it is ZX-003's Family and unique in the fixture), and that Undo still restores it.
  - Add a two-step undo-order case, and one case per undoable kind.

**[MAJOR] MJ3 — PRD:213 (in-flight vocabulary), PRD:580 (R8.3), J:141 — in-flight guard tested in only two of four states.**
- **The gap:** "in flight" means active, paused by the operator, paused by the guard, or held by a device halt. Cases cover only active and operator-paused.
- **Mutation:** guard on `status ∈ {active, operatorPaused}`. All five guarded actions (the three deletes, Change code and Flag) then run under a guard-paused or halted session, and no case fails.
- **Fix:** parametrise UJ1.2-a, UJ4.4-c and UJ4.7-c over all four sub-states. Capture R11.6 and Device R6.9 can already inject them.

**[MAJOR] MJ4 — PRD:530 (R5.5), PRD:500 (R4.6), PRD:501 (R4.7), PRD:412 (R1.7) — state-changing actions during an in-flight session are unspecified.**
- **The actions:**
  - "Use this reading", and Data Foundation E4's restore, turn a flagged or unreadable set-aside row back to captured under a live session.
  - Undo of an earlier code change re-changes a code R8.3 forbids changing in flight.
  - E10 Undo re-inserts rows into a live queue.
- **Why it's open:** R8.3 governs only metadata edits, and F14 covers only deletes, code changes and metadata edits. So nothing says these are refused or allowed. This is not re-litigating a fence.
- **Fix:** owner decides whether they join R8.3's list (E6). Then add one case per action, reading back row state, queue and current/remembered row through Capture R11.11.

**[MAJOR] MJ5 — J:326–327 (UJ9 preamble), J:333, J:336, J:341; PRD:581 (R8.4), PRD:451 (R2.10) — cross-cutting cases run only in the P0 phase, so their later-phase claims are vacuous.**
- **UJ9.2-a:** asserts that no P1/P2 write action is offered on a read-only file, but it runs only when none exist yet. Mutation: Rename column, Set a field, Delete selected, Use this reading, drag or Use as scan order, Flag or E10 Undo stay enabled read-only → green.
- **The same pattern elsewhere:**
  - R8.6: no P1/P2 action is checked for outbound network attempts.
  - R8.9: no keyboard case for E8, E9, E10, E15, E16, selection or Grid. No row names a keyboard route for R2.9's drag.
  - R2.10: what hiding a column does on a read-only file is unspecified.
- **Fix:** re-run these cases in every phase with that phase's action list; state R2.10's read-only behaviour; name the drag's keyboard route.

**[MAJOR] MJ6 — PRD:413 (R1.8) — "Export collection" is never fired.**
- **The gap:** UJ1.3-b and UJ6.3-e reach Export E1 through Data Foundation's E14/E33 "Export first", not through this row's entry point.
- **Mutation:** the entry opens Export E1 at single-item scope, or does nothing → green.
- **Fix:** add a collection-surface case asserting Export E1 at collection scope, plus a variant with a search active that still exports the whole collection (F3 excludes exporting a selection).

**[MAJOR] MJ7 — J:337 (UJ9.4-b) — the "no zx-01 in the file" oracle fails a correct implementation.**
- **Why:** under Import R2.3, the normalised codes of ZX-010 to ZX-015 are "zx-010"… "zx-015", each of which contains "zx-01". Storing that normalised form is the natural way to meet Data Foundation R1.3's per-collection uniqueness, and R2.3's "preserve entered text separately" implies it.
- **Risk:** a correct app fails the case, and the builder's "fix" is to drop a legitimate index.
- **Fix:** search a string that cannot occur in any stored form (e.g. "qzqz") and assert that string is nowhere in the file.

**[MAJOR] MJ8 — PRD:467 (R3.7), J:201, J:204 — Find similar's like-with-like exclusion (F16) has no case that isolates it.**
- **Undeclared inputs:** Blues Two's condition and illuminant/observer (UJ3.4-d needs them to be M1 D50/2°), and FS-005's values. FS-005 may be excluded because it has no working-set value, not because of a condition mismatch.
- **Mutation:** compare across illuminants and conditions → green.
- **Fix:**
  - Declare Blues Two as M1 D50/2°.
  - Add a collection at D65/10° (or M2) holding an item whose own working-set value is within 1.0 of FS-000, and assert it is not listed from All items and is counted as not compared.
  - Give FS-005 a declared value within 3.0.

**[MINOR] m1 — PRD:463 (R3.3), J:197–198 — sort coverage holes.**
- No case fires the C* header.
- Hue is sorted only ascending, so "in either direction" (greys stay after the colours on a descending hue sort) is unasserted. Mutation "reverse the whole list" passes.
- No item sits near NEUTRAL_CHROMA (fixture greys are C* 1.5 and 2.5; the nearest colour is C* 36). Fix: add items at C* 2.9 and 3.1, and fire h° twice.

**[MINOR] m2 — PRD:544 (R6.2), PRD:467 (R3.7) — constant boundaries untested.**
- BULK_CONFIRM_COUNT is tested only at 2 and 11 selected. Add 10.
- FIND_SIMILAR_DISTANCE: 2.8615 is in and 3.4412 out, so a cutoff anywhere between would pass. The pair (48.5, 0, 0)/(51.5, 0, 0) computes to exactly 3.0000 and can pin "at or within".

**[MINOR] m3 — J:57–72, J:336, J:338 — undeclared seam inputs that change oracles.**
- The alternate codes and names of 12 Studio Markers items and both Gouache Set items (every search case depends on them).
- The locale behind Capture R6.9 sorts ("respect the user's language").
- Telemetry and update-check settings for UJ9.4-a. Device R6.17 records attempts Data Foundation R1.4 permits, so "no outbound attempt" can fail a correct app.
- UJ9.5-a and M1: the search texts, filters, sort columns, and cold vs warm opens are all undeclared.
- Fix: add each to the named defaults.

**[MINOR] m4 — J:172 (UJ2.1-j) — Given is unreachable.** Capture's E24 "Session complete" can't be up when Studio Markers still has a pending row and two unsettled set-aside rows (Capture R8.6), and "1 to go" contradicts it. Use E23, the end-early summary.

**[MINOR] m5 — PRD:723 (OQ 7), J:58, J:71 — adaptation transform not pinned.**
- OQ 7's interim fixes gamut and rendering intent, but not the D50-to-display adaptation.
- ZX-002 is inside P3 by only 0.017 under Bradford, and outside (−0.002) under von Kries.
- Gouache Set's ZX-001 is inside sRGB by only 0.010.
- Fix: pin Bradford in the interim and keep every fixture at least 0.03 from a gamut boundary.

**[MINOR] m6 — PRD:706–707, J:344–345 — metric definitions don't match their cases.**
- M2's population is the whole seeded file (15 items); UJ9.8-a counts 13.
- M3 and UJ9.8-b leave out the Flag (R4.9), the one action here that demotes a reading.
- UJ9.8-b runs UJ5.3-c (a P1 restore) in its P0 group.

**[MINOR] m7 — PRD:727 (OQ 11)** — Feeds lists only R3.7, but R5.4 and R5.8 use the same published pairs.

**[MINOR] m8 — J:335 (UJ9.3-a) — weak re-read case.** The only selected item is the one removed, so "deselect everything on re-read" passes; no sort or filter is set, so keeping them is untested. Fix: select ZX-011 and ZX-013, sort by L*, filter to captured, and assert ZX-013 stays selected with sort and filter kept.

**[MINOR] m9 — J:343 (UJ9.7-b), PRD:585 (R8.8) — crash-atomicity gaps.**
- "Crash during the write" doesn't pin when; a crash before the write starts trivially satisfies all-or-none. Data Foundation R7.4 allows any moment, so pin it between the first and the 11th item update.
- Reorder and restore, both multi-row writes, have no crash case.

**[MINOR] m10 — PRD:586 (R8.9), J:340 — accessibility wording undefined.**
- UJ9.6-a asserts accessible wording ("can't show on this screen") that no copy entry defines.
- R8.9's distinct-shape rule does not cover the never-true and unsettled marks.

**[MINOR] m11 — cases or copy introduce rules the rows don't state.**
- E5's ⟨n⟩ is undefined when a search is also active (UJ3.2-d).
- E9's ⟨excluded⟩ counts items with no value, beyond R3.7's "the rest" of values.
- UJ7.1-a places the Collection column last, which R1.9 doesn't specify.

**[MINOR] m12 — PRD:216–217, J:65–66, C:210 — "unsettled" means two things.**
- It means Capture's un-decided set-aside row (the ZX-011/ZX-012 fixtures, R4.2b) and also Data Foundation's correction-unconfirmed reading mark (R2.4i, E12's "Not settled").
- A builder could seed ZX-011 as correction-unconfirmed. Fix: qualify or rename one of them.

**[MINOR] m13 — PRD:450 (R2.9) — reorder edge cases.** Dragging or "Use as scan order" while a search or filter narrows the table is unspecified. Dragging a set-aside row (Capture E33's set-aside variant) has no case.

**[MINOR] m14 — small clauses with no case.**
- **Actions never fired:** "Change the name" on E7 and E11, "Done" on E9 and E12, "OK" on E15, E13's "Filters".
- **Row clauses:**
  - R4.2e's reference-mismatch line.
  - R3.6: the All items view's independent state, and filters persisting.
  - R4.3: a code change leaving queue order and row state alone.
  - R5.5: that the restored value equals the earlier reading's value (only reason and time are asserted).
  - R1.10: delete or export from the All items view (Gouache Set's ZX-001 vs Studio Markers' ZX-001).
  - R8.1: a saved reading appearing in time at ROWS_CEILING.
  - R8.2: a very long history.
  - R8.3: keeping the scroll position (UJ9.1-a).
- **Undefined term:** R7.1's "the item in view".

**[MINOR] m15 — PRD:412 (R1.7)** — two deletes inside one undo window, and ⌘Z after a delete versus R4.7's metadata stack, are unspecified. Raise under OQ 10.

**[MINOR] m16 — sibling halves.**
- **Data Foundation E33 (DFC:27):** zero-count rendering for a selection of only never-scanned items is untested. UJ6.3-d confirms exactly that selection (ZX-010, ZX-011) without asserting E33, and DJ4's new row (DFJ:50) uses "some with history".
- **Capture UJ3.3-h (CAPJ:111):** asserts M3 "not reopened" but names no observable to read it from.
- **Capture R8.18 (CAP:297):** the N_CONSEC_HARD clause has no case. If it can't be reached in flight, say so.

**[NIT] n1** — Ties under a reversed sort are exercised only for items with no value. A whitespace-only search (active or not?) and a window straddling two displays are unspecified.

## Biggest risks   (what could ship broken behind a green suite)
- A chip that shows a clipped or wrong colour while its honesty marks look right (B1). The ZX-013 fixture actively pushes the build toward that result (B2).
- Write actions enabled on a read-only file for every P1/P2 action (MJ5).
- Deletes, code changes or Flag going through while a guard-paused or device-halted session is live, and restores or undos changing rows under a running session (MJ3, MJ4).
- Undo storing prior metadata in the file, breaking Data Foundation's F17 privacy rule (MJ2).
- Wrong counts in confirmation dialogs going unseen for lack of token readback (MJ1).

## Genuinely solid   (incl. where minimal scoping is correct that a coverage-zealot would wrongly flag)
- **Oracles verified by recomputation:**
  - Sort orders: UJ3.3-b/c/d/e/f, UJ4.1-b, UJ7.1-e, UJ9.2-b, UJ10.1-a.
  - Counts: 16 Studio Markers readings, 18 in the file, E33's 2/2/3, the 11-item range, the 10/2/1 tallies.
  - Every ΔE2000 value checks out against my own CIEDE2000 implementation: 0.9999989, 2.0424597, 2.8615102, 3.4411906 and 1.0000047.
  - UJ3.4-d correctly doesn't assert the FS-001/FS-101 order (they are 1.0000 apart only at the 5th decimal).
  - UJ8.1-d is correct under Capture's pending-only reorder interim.
- **Structure:** the T-cases mirror the 14 row-transition routes one-for-one.
- **Undo-window care:** T2–T4, T10, UJ6.2-f and UJ1.3-d avoid file reads with the app closed, which would end the undo window mid-case.
- **Good discriminator:** UJ3.1-d (typing "12") separates start-of-code matching from anywhere-in-text matching across code and alternate code.
- **Seams:** the rename seam is tested from both sides (UJ4.6-e/f and Import's new UJ2.1 row).
- **Atomicity:** UJ9.7-b's all-or-none oracle is the right shape.
- **Correctly minimal:**
  - Quit, crash and file-switch endings of the delete-undo window are left to Data Foundation's DJ4.
  - Data Foundation's own state wording isn't re-tested here.
  - Dismiss buttons don't need more than a smoke case.

## Missing / over-tested
- **Missing:** the B1 chip readback; the MJ1 token, E9 and offered-action readback; four-state in-flight cases; in-flight restore and undo rules; per-phase re-runs of the read-only, network and keyboard cases; an undo case that reads the file; a direct "Export collection" case; a Find-similar case across illuminants; the boundary cases in m1 and m2; and the named defaults in m3.
- **Over-tested:** nothing material. The T-cases overlap UJ cases (T1≈UJ4.4-b, T6≈UJ4.2-a, T8≈UJ4.6-a, T11≈UJ5.3-c), but the format requires both.

| Row ID | disposition |
|---|---|
| R1.1 | ALIGN |
| R1.2 | ALIGN |
| R1.3 | OBJECT (m14) |
| R1.4 | OBJECT (MJ3) |
| R1.5 | ALIGN |
| R1.6 | ALIGN |
| R1.7 | OBJECT (m15, MJ4) |
| R1.8 | OBJECT (MJ6) |
| R1.9 | OBJECT (m11) |
| R1.10 | OBJECT (m14) |
| R2.1 | ALIGN |
| R2.2 | OBJECT (m4) |
| R2.3 | OBJECT (B1) |
| R2.4 | OBJECT (B2) |
| R2.4a | OBJECT (B2) |
| R2.4b | OBJECT (B2) |
| R2.4c | ALIGN |
| R2.4d | ALIGN |
| R2.4e | ALIGN |
| R2.4f | ALIGN |
| R2.4g | ALIGN |
| R2.4h | ALIGN |
| R2.4i | OBJECT (m12) |
| R2.5 | OBJECT (B2, m5) |
| R2.6 | ALIGN |
| R2.7 | ALIGN |
| R2.8 | ALIGN |
| R2.9 | OBJECT (m13, MJ5) |
| R2.10 | OBJECT (MJ5) |
| R3.1 | OBJECT (m3) |
| R3.2 | OBJECT (m3) |
| R3.3 | OBJECT (m1) |
| R3.4 | ALIGN |
| R3.5 | OBJECT (m11) |
| R3.6 | OBJECT (m14) |
| R3.7 | OBJECT (MJ8, m2, m11) |
| R3.8 | ALIGN |
| R4.1 | ALIGN |
| R4.2 | ALIGN |
| R4.2a | ALIGN |
| R4.2b | ALIGN |
| R4.2c | ALIGN |
| R4.2d | ALIGN |
| R4.2e | OBJECT (m14) |
| R4.2f | ALIGN |
| R4.2g | ALIGN |
| R4.2h | ALIGN |
| R4.3 | OBJECT (m14) |
| R4.4 | ALIGN |
| R4.5 | ALIGN |
| R4.6 | OBJECT (MJ4) |
| R4.7 | OBJECT (MJ2, MJ4) |
| R4.8 | ALIGN |
| R4.9 | ALIGN |
| R5.1 | ALIGN |
| R5.2 | ALIGN |
| R5.2a | ALIGN |
| R5.2b | ALIGN |
| R5.2c | ALIGN |
| R5.2d | ALIGN |
| R5.2e | ALIGN |
| R5.3 | ALIGN |
| R5.4 | OBJECT (m7) |
| R5.5 | OBJECT (MJ4, m14) |
| R5.6 | ALIGN |
| R5.7 | ALIGN |
| R5.8 | OBJECT (m7) |
| R6.1 | ALIGN |
| R6.2 | OBJECT (m2) |
| R6.3 | ALIGN |
| R7.1 | OBJECT (m14) |
| R7.2 | ALIGN |
| R8.1 | OBJECT (m3, m14) |
| R8.2 | OBJECT (m14) |
| R8.3 | OBJECT (MJ3, MJ4) |
| R8.4 | OBJECT (MJ5) |
| R8.5 | OBJECT (m8) |
| R8.6 | OBJECT (MJ7, MJ5, m3) |
| R8.7 | ALIGN |
| R8.8 | OBJECT (m9) |
| R8.9 | OBJECT (m10, MJ5) |
| R8.10 | OBJECT (B1, MJ1) |
| E1 | ALIGN |
| E2 | ALIGN |
| E3 | ALIGN |
| E4 | ALIGN |
| E5 | OBJECT (m11) |
| E6 | ALIGN |
| E7 | OBJECT (m14) |
| E8 | ALIGN |
| E9 | OBJECT (MJ1, MJ8) |
| E10 | ALIGN |
| E11 | ALIGN |
| E12 | OBJECT (m12) |
| E13 | ALIGN |
| E14 | ALIGN |
| E15 | ALIGN |
| E16 | ALIGN |
| E17 | ALIGN |
| M1 | OBJECT (m3) |
| M2 | OBJECT (B2, m6) |
| M3 | OBJECT (m6) |

(MJ1's missing token readback affects the asserts on E2, E3, E6, E8, E10, E13, E15, E16 and E17. I've carried it once, on R8.10, where the one-clause fix lands.)

#### peer-interface-reviewer (Claude route)

## Verdict
Sound after fixing the Majors. There are no Blockers: every sibling row this change amends is gated by a dated fence and a "peer review pending" clause on its status line. But six Majors leave labels, action placement and the test readback contract ambiguous for an agent builder: a "Done" label collision with Capture's locked Labels rule, an "Undo" with no copy home, a contradiction over where rename and delete live, mark and filter text that no copy entry owns, an incomplete R8.10 readback, and a restore that no row gates during a live session.

## Surface & consumers (brief)
- **The surface.** The Collection Mode PRD's row IDs, named constants, inherited-obligations tables and copy states. Files:
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode.md` (PRD below)
  - `…/prd-collection-mode-copy.md` (copy below)
  - `…/prd-collection-mode-journeys.md` (journeys below)
  - `…/prd-collection-mode-oq-results.md`
  - The sibling halves from `git diff origin/main -- docs/product ':!docs/product/collection-mode'`, in the Capture, Data Foundation, Import, Export and Device PRDs.
- **Who depends on it.** Agent builders reading rows, copy and the Test-controls map. Test harnesses that key on copy-state and variant IDs and on action labels. Sibling PRDs that cite these rows.
- **What changed in the siblings.** Capture gains R8.18, and R1.1, R5.6, R8.2, R9.9, R11.15g, M3 and M4 are amended (F70). Data Foundation gains E33 and R7.6o, and R1.2, R2.9 and R6.2 are amended (F50, F51). Import R2.6 is narrowed from "first-seen spelling" to "stored spelling" (F65). Export §5 and its copy header now name DF E33 (F31). Device's obligation line is amended (F32). Each carries a dated fence and a status-line clause. I found no un-gated break to a locked sibling contract.

## Findings
[MAJOR] IF-1 "Done" on the collection surface: a label collision with Capture's locked Labels rule.
- **Where.** Copy E9 Actions (copy:180) and E12 Actions (copy:211) both render "Done". Both are indexed to the collection surface. R2.8 (PRD:449) and R3.8 (PRD:468) name them.
- **The conflict.** Capture's §12 Labels (`docs/product/capture-mode/prd-capture-mode.md:402`) fixes "Done" as the label "which dismisses a summary shown on the collection surface and nothing else" (E24).
- **Why it matters.** This change adds two more meanings to that label on that same surface without mirroring anything into Capture: F70's editorial clause (3) does not touch §12. In UJ2.1-j, E24 is up and "Colour marks" is one click away, so two "Done" controls with different effects coexist. A harness or builder applying Capture's rule dismisses the wrong thing.
- **Fix.** Either give E9 and E12 a distinct dismiss label (e.g. "Close"), or append a dated F70 clarification narrowing Capture's "Done" rule to "the summary's own Done". The owner or copy decides which.

[MAJOR] IF-2 R4.7 "Undo" (PRD:501): no copy home, a collision with E10's "Undo", and no refusal model.
- **No copy home.** R4.7 quotes "Undo", but the only "Undo" in the copy is E10's action (copy:190), which is a different state (undoing a delete, R1.7, gated behind OQ 10).
  - The Test-controls map declares "Undo" only through "the actions of E4–E12" (journeys:371). The item detail row (journeys:373) has none.
  - T10 (journeys:97) and UJ6.2-f (journeys:289) therefore fire a control that no declared seam exposes.
- **Two stacks, one label.** If a user edits a field and then deletes an item, the plain Undo is ambiguous: E10 is up, and R4.7 says "most recent first" over a list that excludes deletes.
- **Undo can break R4.8's rule.** After UJ4.6-a (Family renamed to Hue Family) and UJ4.6-f (an import appends a new "Family" column), R4.7's Undo reverses the rename and leaves two columns named Family. That violates R4.8's duplicate rule, and Import R2.6 matching can no longer resolve the name.
- **Undo can break R4.4's rule.** Change ZX-001 to ZX-100 (E16's own warning scenario), import a CSV listing ZX-001, then Undo: the result is two ZX-001s.
- **Undo can bypass the session guard.** Undo of a code change while a session is in flight is a code change, which R8.3 and F14 refuse. R4.7 does not route it through R8.3.
- **No failure path.** There is no copy state for a refused undo.
- **Fix.**
  - R4.7 should state that an undo re-applies R4.4's, R4.8's and R8.3's acceptance, rendering E15 or E11 "duplicate", or E6, and changing nothing.
  - State whether an import, a re-read (R8.5) or a delete clears the undo history.
  - Give metadata undo its own copy label, distinct from E10's, or state explicitly that both share one stack.
  - Add the control to the Test-controls map.

[MAJOR] IF-3 "Rename collection" and "Delete collection" are placed on two different surfaces.
- **Collection surface.** The Surfaces table (PRD:377–378) and copy E3's Actions (copy:123) put both actions on the collection surface. E2's Actions (copy:114) are exactly "New collection", "All items" and "Answer re-scans". E7 is indexed to the collection surface.
- **Collection list.** The Test-controls map puts both actions on the collection list (journeys:370). UJ1.1-a–d and UJ1.3-a/c assert E2 immediately after the action (journeys:137–148). E3's own "Rename collection" and "Delete collection" are not among the collection surface's declared inputs (journeys:371, which lists "the actions of E4–E12", not E3's).
- **Effect.** A builder following the copy and a harness following the map disagree on where the control is.
- **Fix.** Move both actions to the collection-surface line of the map and change the UJ1 Asserts to read the collection surface. Or, if the owner wants them on the list too, add them to E2's Actions.

[MAJOR] IF-4 Displayed text on the P0 honesty path has no copy home.
- **Contract.** The document's contract is that "copy owns displayed text". The copy file writes no strings for the following P0 text:
  - the nine R2.4 mark names as they render on chips (PRD:428–442);
  - the filter values offered by "Filters" (R3.4, PRD:464);
  - each mark's VoiceOver accessible name (R8.9, PRD:586);
  - the set-aside cause R4.2b displays ("unreadable", "flagged after capture"). Capture's copy file has no cause strings either.
- **Only source is prose.** The one place the display names exist is E12's prose body (copy:210), in the form "Can't show on this screen:".
- **A case matches on wording.** UJ9.6-a (journeys:340) asserts the accessible description "names … can't show on this screen and outside sRGB". That matches on wording, contradicting R8.10's "without matching wording".
- **Lower-stakes instances.** R2.1's column headers and the line labels of R4.2 and R5.2.
- **Fix.** Add a copy entry (a label table) mapping each mark identifier (cannot-show, outside-sRGB, …) to its chip label, filter label and accessible name. Have R2.4, R3.4 and R8.9 cite it. Rewrite UJ9.6-a to assert mark identifiers.

[MAJOR] IF-5 R8.10's test readback (PRD:587) cannot read what about 20 cases assert.
- **What R8.10 grants.** Listing, for the collection list, table, item detail and version history view: rows in order, columns, each chip's marks and accessible description, and which copy state and variant is up. The journeys Harness restates only that (journeys:35–37).
- **Which actions are offered.** Not granted. UJ3.4-c, UJ4.7-b, UJ5.1-d, UJ5.3-d/f, UJ7.1-c, UJ8.1-e/f and UJ9.2-a assert it, and the guard rows R8.3, R8.4 and R1.10 depend on it.
- **Other state.** Also not granted:
  - the selection and the selected count (UJ6.1-*);
  - the search field's text (UJ3.2-d, UJ3.3-f);
  - a filter shown as active (UJ2.1-g);
  - the first row on screen (UJ4.1-b);
  - All items rows with their collection (UJ7.1-*);
  - E9's listed items and distances (UJ3.4-a/d);
  - grid swatches and their size (UJ10.1-a/b).
- **The journeys' own rule fails.** The Test-controls map lists these as observables, but no row grants them. That breaks the journeys' rule that "a case that asserts a value this document gives no way to read is a defect".
- **A sibling cite points at the gap.** Capture R11.15g (`prd-capture-mode.md:376`) now says Collection Mode's own collection-surface actions "are listed under its R8.10". R8.10 lists none.
- **Fix.** Extend R8.10's list with those observables and add the All items view, the E9 list and the grid to its surface list. Retarget Capture R11.15g to copy E3's Actions, or to the extended R8.10 clause.

[MAJOR] IF-6 A restore during an in-flight session is ungated on every side of the seam.
- **Where restore exists.** R5.5 "Use this reading" (PRD:530) and R4.6's route to DF E4 "Use a previous reading" (PRD:500) both move a row from set aside to captured. That is Capture R5.6 and R8.18 as amended.
- **R8.3 doesn't cover it.** R8.3 (PRD:580) blocks only "Delete collection", "Delete swatch", "Delete selected", "Change code" and Flag. Its "every other metadata edit … moves no … row state" clause does not reach a restore, which is not a metadata edit.
- **Siblings don't gate it.** DF R7.3k gates "Scan again" with "normal session gates" but not restore. Capture R5.6, Capture R8.18, and cases UJ3.3-i/j and UJ5.3-g all assume there is no session.
- **Scenario.** A bulk session is in flight on Studio Markers. The operator flags a row, which enters that session's set-aside list. They then open it in the item detail and fire "Use this reading". The row leaves the live session's set-aside list, and the session's tallies change under it. That is exactly the "nothing it's working on moves under it" that E6's copy (copy:150) promises.
- **Not a fence breach.** F14's "other edits stay available" doesn't clearly cover it either way, so this is an unstated WHAT rather than a fence contradiction.
- **Fix.** Put the fork to the owner: add both restore entry points to R8.3's E6 list, or state what the session does when a row is restored under it. Mirror the answer on Capture R8.18 and R5.6 and add a case.

[MINOR] IF-7 Inbound-table fence cites name no owning document, and collide with this PRD's own fences.
- **Where.** PRD:616–639: "(with R1.2 and F3)", "(and F10)", "(and F22)", "F43", "(and F70)", "F4", "F15", "F17", "F39", "(and F65)".
- **Collision.** Under this PRD's own rule ("An ID written bare is this document's own", PRD:599–600), these resolve to Collection Mode's F3, F4, F10, F15, F17 and F22. Those are different decisions. They should resolve to Capture's F3, F10, F22 and F43, and DF's F4, F15, F17 and F39.
- **Impact.** Provenance only, but a mechanical fence map built on the bare-ID rule will misresolve them.
- **Fix.** Write "its F10" (and so on) in each cite.

[MINOR] IF-8 E10 carries "Phase: none" (copy:186), but OQ 10's interim says "R1.7 is not built and E10 does not appear" (PRD:726).
- **Why it matters.** E10's trigger is a P0 delete (R1.4, R4.5), so a first-phase builder reading the copy would render it.
- **Root cause.** The three-kind phase vocabulary (PRD:254, copy:75–77) cannot express "a state that doesn't exist yet on an existing surface".
- **Fix.** Add a state-absent kind, or reuse variant-absent with a sentence saying so.

[MINOR] IF-9 R3.4 (PRD:464) reads as if "Clear filters" and "Clear search" are available whenever the table is narrowed.
- **Conflict.** E3's "narrowed" variant carries neither action (copy:123–124). They exist only in E4 and E5.
- **Fix.** Say they are E4's and E5's actions, or add them to E3.

[MINOR] IF-10 The selection-scale undo was not mirrored into Data Foundation.
- **What CM relies on.** R1.7's "swatches" variant (PRD:412) relies on DF R6.2b. DF E33 promises "‹P1› You can undo it while this file stays open."
- **What DF grants.** DF R6.3 (`prd-data-foundation.md:218`) and R6.2a keep the item/collection scope. F50's decision says the undo window is unchanged.
- **Fix.** Add the selection scope to R6.3 in the same change, or note it under OQ 20.

[MINOR] IF-11 The ⟨n⟩ token is overloaded, and its per-state meaning is undefined (Placeholders rule, PRD:681–690).
- **The overload.** The same token means collections in E2, total swatches in E3, hidden swatches in E5, selected swatches in E8, and readings in E17.
- **A case where E5 misleads.** In UJ3.2-d (search qqq plus a filter), E5 renders "hidden by the filters" (copy:141) when the search, not the filters, emptied the table.
- **Fix.** Define ⟨n⟩ per state, and state E5's count basis.

[MINOR] IF-12 The Device seam names rows on one side only.
- **Mismatch.** Device's line (`prd-device-management.md:266`) names Collection Mode R1.10 and F5/F25 for the All items no-banner rule. Collection Mode's inbound Device line (PRD:625) maps only to R2.4c, R2.6 and R5.2b.
- **Fix.** Add R1.10 to that line.

[MINOR] IF-13 Some labels have no owning row, and some are never exercised.
- **No owning row.** E8's "Cancel" is not named by R6.2 or any other row.
- **Never fired by a case.** "Change the name" (E7, E11), "OK" (E15) and "Done" (E9, E12) are not fired by any case, so their "closes / returns to editing" behavior is untested.
- **Fix.** Name "Cancel" in R6.2 and add one case per action.

[NIT] IF-14 Two quoted strings in the PRD body are not labels: "no requirement" (PRD:574) and "the device PRD's `R6.27`" (PRD:599). Both sit outside comments, so the tree-wide label check will flag them. Unquote them or reword.

[NIT] IF-15 R2.10 writes the file (column visibility, F21), but neither the Row transitions table nor the T-index has a present→present route for it.

[NIT] IF-16 Stale indexes.
- Capture `prd-capture-mode.md:91` still says "Collection Mode remains unwritten."
- `docs/product/README.md:18` still lists Collection Mode as "queued".

[NIT] IF-17 R1.3 does not say what Capture E1's "Change the name" does when E1 is reached from a rename rather than from creation. It says it only for E7's.

[NIT] IF-18 E9 is fired from the item detail ("Find similar" is in E14), but E9 is indexed only to the collection surface and the All items view. Its ⟨code⟩ headline is also ambiguous in the All items view when the same code appears in two collections.

## Biggest risks   (what existing consumers/scripts/agents break)
- **Harnesses keyed on labels.** They break on "Done" (IF-1) and "Undo" (IF-2), which are ambiguous across states and across PRDs on the shared collection surface.
- **Agent-built harnesses.** A harness built from R8.10 and the Harness paragraph cannot read offered actions, the selection or the search text (IF-5). The P0 guard rows (R8.3, R8.4) and about 20 cases then fail as unexecutable, or pass without being checked.
- **Where the controls are.** Builder and harness diverge on where rename and delete live (IF-3).
- **Data integrity through Undo.** Undo can create duplicate codes or columns, or bypass the session guard (IF-2). The file then holds states that R4.4, R4.8 and Import R2.6 matching define as impossible.
- **Live-session consistency.** A restore under a live session can move a row out of that session's set-aside list (IF-6).

## Genuinely well-designed
- **Labels match both ways.** Every quoted action and variant in the rows matches a copy label character for character, and every copy label maps back to a row. The only exceptions are R4.7's "Undo" and E8's "Cancel".
- **Variants.** Every variant is enumerated verbatim by the row the copy names (R1.4, R1.7, R3.4, R3.8, R4.4, R4.8, R5.3, R6.2).
- **Sibling actions.** Sibling actions are named by document and state and never quoted. Examples: "the Data Foundation PRD's E11 swatch-has-changed action", and Capture's "Flag" via its §12.
- **Outbound obligations.** All three name rows and fences on both sides, and each sibling half landed in the same change as a dated, gated fence with "alignment kept" and "peer review pending". This is the right migration path. Import R2.6's narrowing is the only real semantic change to a locked matching contract, and it has its own fence (F65) and a new journey case.
- **Correct reuse that a style checker would wrongly flag:**
  - "Change the name" in Capture E1 and Collection Mode E7/E11 has one meaning;
  - "Cancel" is scoped per state;
  - the UJ1.2-a/b/c IDs repeat across documents but are resolved by owning-document naming and a mapping from scenario to inherited case;
  - the All items view omits the E22 banner, which is justified by F5 and mirrored in Device.
- **Tests read identifiers, not wording.** The kebab-case mark identifiers used by cases avoid wording-matching, apart from UJ9.6-a (IF-4). The identifiers only need a copy map.

## Missing / over-engineered
- **Missing:**
  - a copy table of mark, filter and accessible labels (IF-4);
  - a label and refusal model for metadata undo (IF-2);
  - action availability and selection/search/scroll observables in R8.10 (IF-5);
  - a session-gating rule for restore (IF-6).
- **Over-engineered:** nothing material. The cross-seam bookkeeping (fence maps, dated clarifications) is heavy but proportionate for agent builders.

| Row ID | disposition |
|---|---|
| R1.1 | ALIGN |
| R1.2 | ALIGN |
| R1.3 | OBJECT (IF-3) |
| R1.4 | OBJECT (IF-3) |
| R1.5 | ALIGN |
| R1.6 | ALIGN |
| R1.7 | OBJECT (IF-2, IF-10) |
| R1.8 | ALIGN |
| R1.9 | ALIGN |
| R1.10 | OBJECT (IF-12) |
| R2.1 | ALIGN |
| R2.2 | ALIGN |
| R2.3 | ALIGN |
| R2.4 | OBJECT (IF-4) |
| R2.4a | ALIGN |
| R2.4b | ALIGN |
| R2.4c | ALIGN |
| R2.4d | ALIGN |
| R2.4e | ALIGN |
| R2.4f | ALIGN |
| R2.4g | ALIGN |
| R2.4h | ALIGN |
| R2.4i | ALIGN |
| R2.5 | ALIGN |
| R2.6 | ALIGN |
| R2.7 | ALIGN |
| R2.8 | OBJECT (IF-1) |
| R2.9 | ALIGN |
| R2.10 | ALIGN |
| R3.1 | ALIGN |
| R3.2 | ALIGN |
| R3.3 | ALIGN |
| R3.4 | OBJECT (IF-4, IF-9) |
| R3.5 | ALIGN |
| R3.6 | ALIGN |
| R3.7 | ALIGN |
| R3.8 | OBJECT (IF-1) |
| R4.1 | ALIGN |
| R4.2 | ALIGN |
| R4.2a | ALIGN |
| R4.2b | ALIGN |
| R4.2c | ALIGN |
| R4.2d | ALIGN |
| R4.2e | ALIGN |
| R4.2f | ALIGN |
| R4.2g | ALIGN |
| R4.2h | ALIGN |
| R4.3 | ALIGN |
| R4.4 | ALIGN |
| R4.5 | ALIGN |
| R4.6 | OBJECT (IF-6) |
| R4.7 | OBJECT (IF-2) |
| R4.8 | ALIGN |
| R4.9 | ALIGN |
| R5.1 | ALIGN |
| R5.2 | ALIGN |
| R5.2a | ALIGN |
| R5.2b | ALIGN |
| R5.2c | ALIGN |
| R5.2d | ALIGN |
| R5.2e | ALIGN |
| R5.3 | ALIGN |
| R5.4 | ALIGN |
| R5.5 | OBJECT (IF-6) |
| R5.6 | ALIGN |
| R5.7 | ALIGN |
| R5.8 | ALIGN |
| R6.1 | ALIGN |
| R6.2 | ALIGN |
| R6.3 | ALIGN |
| R7.1 | ALIGN |
| R7.2 | ALIGN |
| R8.1 | ABSTAIN |
| R8.2 | ABSTAIN |
| R8.3 | OBJECT (IF-6) |
| R8.4 | ALIGN |
| R8.5 | ALIGN |
| R8.6 | ALIGN |
| R8.7 | ALIGN |
| R8.8 | ALIGN |
| R8.9 | OBJECT (IF-4) |
| R8.10 | OBJECT (IF-5) |
| E1 | ALIGN |
| E2 | ALIGN |
| E3 | ALIGN |
| E4 | ALIGN |
| E5 | OBJECT (IF-11) |
| E6 | ALIGN |
| E7 | ALIGN |
| E8 | OBJECT (IF-13) |
| E9 | OBJECT (IF-1) |
| E10 | OBJECT (IF-2, IF-8) |
| E11 | ALIGN |
| E12 | OBJECT (IF-1, IF-4) |
| E13 | ALIGN |
| E14 | ALIGN |
| E15 | ALIGN |
| E16 | ALIGN |
| E17 | ALIGN |
| M1 | ABSTAIN |
| M2 | ALIGN |
| M3 | ALIGN |

#### peer-privacy-reviewer (Claude route)

## Verdict
**Privacy-sound to ship.** No finding is Critical or High. Every row keeps personal data inside the user's own file, and none adds a network path. Three Medium gaps should still close before lock: the promises that deleted or cleared text is gone, that the metadata undo stack stays out of the file, and that nothing leaves the machine each lack a case that would catch a real violation.

## Data-flow & PII map (brief)
- **Who the data is about.** The user (the operator). Also anyone the user names in an imported column: F8 routes free text such as a "Client" or "Notes" column through CSV import, so third parties can become data subjects.
- **Personal fields these surfaces touch:**
  - the acquiring device's serial and firmware (Device R1.21 snapshot; shown by R4.2d and R5.2b);
  - measurement and record times (R5.2a);
  - free-text Swatch Name, alternates and imported values, plus collection and column names: edited by R1.3, R4.3, R4.4, R4.8 and R6.2, deleted by R1.4, R4.5 and R6.3;
  - search text (R3.1, R3.6);
  - per-collection column visibility (R2.10).
  - Nothing is special-category, and colour values are not personal data.
- **Where it is stored.** Everything lives in the user's one SQLite file (DF R1.1). The only other state is short-lived:
  - search, filters and sorts stay in memory while the file is open (R3.6, R8.6);
  - the undo stacks for metadata edits (R4.7) and deletes (R1.7) live for the open-file lifetime.
- **How data leaves.** Only through an export the user starts: R1.8 opens Export E1, whose copy says each reading carries its instrument serial. DF E33's "Export first" exports the whole collection (F24). This PRD adds no network path (R8.6).
- **The user's rights.** Erasure works only per item, selection or collection (R5.6; DF R6.1, a settled non-negotiable). Correction happens in place (R4.3). History is kept for good (R8.2; DF R2.6).

## Findings

**[MEDIUM] PRIV-1 — Deleted, cleared or replaced text can survive in the file, and no case would notice**
- **Where:**
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode.md:587` (R8.10)
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode-copy.md:171` (E8 "clear")
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode-journeys.md` lines 88/233 (T1, UJ4.4-b), 148 (UJ1.3-c), 292 (UJ6.3-b), 93/223 (T6, UJ4.2-a), 240 (UJ4.6-a), 286 (UJ6.2-c)
- **The promise.** This PRD adds new ways to remove or replace user-entered text: bulk set and clear (R6.2), selection delete (R6.3), code change (R4.4), column rename (R4.8), and the P0 edit (R4.3). The old text is supposed to be gone:
  - DF R2.3 (`docs/product/data-foundation/prd-data-foundation.md:115`) says an edit leaves "no prior text";
  - DF R6.2a and R6.2d (:229, :232) say deleted or cleared content is "unrecoverable … by an outside reader";
  - this PRD's own E8 "clear" copy tells the user "what was there is no longer kept in your file".
- **Why no case can catch a violation.**
  - Only UJ4.2-c checks that old text is absent, and it covers one single-field clear in the P0 phase.
  - The delete cases (T1, UJ4.4-b, UJ1.3-c, UJ6.3-b) only check that the row is gone ("holds no item ZX-010").
  - Several edit cases pick fixtures where the new value contains the old one: Sky Blue → Sky Blue Light (T6, UJ4.2-a) and Family → Hue Family (UJ4.6-a). No leftover check is possible.
  - UJ6.2-c, the case behind E8's "clear" promise, asserts only "an empty Family".
  - R8.10 grants one readback: a SQL read at SQLITE_READER_FLOOR. A SQL reader never sees freed pages or leftover journal/WAL frames.
- **Exposure.** An implementation that leaves deleted or cleared rows in freed pages passes every case. Running `strings` over a copy of the file then recovers them. The file is designed to be copied and shared (AGENTS.md §3: "the SQLite file is the sync strategy"; DF R1.1).
- **Who is affected.** The user, and any third party named in an imported column.
- **Remediation:**
  - (a) R8.10 grants a readback of the file's bytes, and of anything the app leaves beside it, with the app closed.
  - (b) Residue asserts use fixtures where the old value is not part of the new one:
    - UJ6.2-c: "Pink" and "Yellow" appear nowhere;
    - UJ6.3-b: "Teal" and "Plum" appear nowhere;
    - T1 / UJ4.4-b: "Warm Grey 1" appears nowhere;
    - T6 / UJ4.2-a: change the new name so it does not contain "Sky Blue", then assert "Sky Blue" appears nowhere.
  - (c) Outbound obligation to Data Foundation: define R6.2a's "outside reader" as either a SQL client or anyone holding the file's bytes. E8's copy promises the stronger meaning.
- **Fences.** No fence decides leftover data, so this re-litigates nothing. (GDPR Art. 17; Art. 5(1)(e); Art. 25)

**[MEDIUM] PRIV-2 — The metadata undo stack can leak what the user cleared**
- **Where:**
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode.md:501` (R4.7), `:587` (R8.10)
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode-journeys.md:276-277` (the UJ 6 phase includes R4.7), `:289` (UJ6.2-f), `:97` (T10)
- **Gap 1: the wording is narrower than the user is told.** R4.7 only forbids keeping replaced values "where an outside reader of the file can recover it". That is weaker than DF R1.1 ("nothing about a collection … is kept elsewhere") and than E8's "no longer kept in your file". Read alone, R4.7 would allow saving the undo stack to disk outside the file for crash recovery.
- **Gap 2: no case can detect an undo log kept inside the file.**
  - UJ6.2-f and T10 run with R4.7 built, but R8.10 reads the file only after the app closes. A clean close is exactly when such a log would be purged.
  - While the app is open, an outside reader can see the log. DF R1.5 (`docs/product/data-foundation/prd-data-foundation.md:105`) tells users in the help docs that reading the file elsewhere is safe.
  - After a crash, the log stays in the file until the next open.
- **Exposure.** A user clears a "Client" column, is told it is no longer kept, the app crashes, and the user shares the file. The cleared values go with it.
- **Remediation:**
  - R4.7 says replaced values are held only in the running app's memory and are discarded when the undo window ends: file close, quit, crash or switching files, the same lifetime as DELETE_UNDO_WINDOW. They are never written to the file or anywhere else.
  - Add a case in R4.7's phase: select ZX-003, ZX-011 and ZX-012 and clear Family. Then:
    - (i) with undo still available, a second reader of the file finds "Pink" and "Yellow" nowhere;
    - (ii) crash the app; before reopening, the file's bytes and anything beside it hold "Pink" and "Yellow" nowhere.
  - R8.10 grants the open-app outside read and the read after a crash, before reopening.
- (Art. 5(1)(c)/(e); Art. 25)

**[MEDIUM] PRIV-3 — The no-network check runs only with the network off**
- **Where:**
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode.md:583` (R8.6)
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode-journeys.md:336` (UJ9.4-a), `:78` (named default: Network Reachable)
- **The promise.** R8.6 says "no action this PRD adds makes a network request, and nothing it reads or writes leaves the machine".
- **The gap.**
  - Its only case, UJ9.4-a, marks the network unreachable through Device R6.12 (a simulated network-status setting, `docs/product/device-management/prd-device-management.md:233`). It then asserts that R6.17's record of outbound attempts (:238) is empty.
  - Code that checks connectivity first (a network monitor, or a "send when online" queue) never tries to send while offline. It passes UJ9.4-a, then sends under the harness's own default: network reachable.
  - UJ9.4-a also drives only search, one edit and "Show history". Find similar in the All items view, rename, bulk set and clear, delete, the export entry points and column visibility are never checked for outbound traffic.
  - DF R1.4's general egress test does not drive these actions.
- **What could leave.** User-entered metadata, search text, collection names and column names.
- **Remediation:**
  - Add a case with the network reachable and telemetry declared off (Device R2.14's default).
  - Drive every action this PRD adds.
  - Assert that R6.17 records no attempt beyond DF R1.4's permitted set for that configuration, and none whose payload carries typed text.
  - Keep UJ9.4-a as the offline-works case.
- (Art. 5(1)(b); Art. 25)

**[LOW] PRIV-4 — After a crash, a pending delete may not be unrecoverable, as the PRD claims**
- **Where:**
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode.md:136` (row transition: crash → deleted), `:227` (definition of "deleted"), `:412` (R1.7)
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode-journeys.md:91` (T4), `:150` (UJ1.3-e)
  - DF `docs/product/data-foundation/prd-data-foundation.md:230` (R6.2b), `:339` (OQ 20)
- **The claim.** The Row-transitions table says a crash turns deleted-undoable into deleted, which the Vocabulary defines as "unrecoverable by an outside reader".
- **Why it may not hold.** Where content waiting on undo is kept is exactly what DF OQ 20 leaves open. If OQ 20 keeps it inside the file, a crash leaves the "deleted" collection readable until the next open, and sharing the file in between discloses it.
- **Why no case catches it.** T4 reads the file only after reopening, so an implementation that purges on open passes. UJ1.3-e never reads the file. No case covers the crash route.
- **Why Low.** R1.7 is gated behind OQ 10 and cannot be built yet.
- **Remediation.** When OQ 10 / DF OQ 20 closes:
  - T4 and UJ1.3-e read the file with the app closed, before reopening;
  - add a case that crashes the app while an item is deleted-undoable and asserts the same;
  - if OQ 20 keeps pending content in the file, restate the crash route's Result and the "deleted" definition instead of claiming something a crash cannot deliver.
- (Art. 17)

**[LOW] PRIV-5 — "Not kept once the file closes" is checked only against the UI and the file**
- **Where:**
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode.md:466` (R3.6), `:451` (R2.10, "nothing … kept elsewhere")
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode-journeys.md:200` (UJ3.3-f), `:337` (UJ9.4-b)
- **The gap.** R3.6 and DF R1.1 forbid keeping search text or view state anywhere after the file closes. The cases check only:
  - that the search field is empty after reopening (UJ3.3-f);
  - that the file does not contain the search text (UJ9.4-b).
- **Missed paths.** Common macOS persistence paths sit outside both checks:
  - the search field's saved recent searches in the app's preferences;
  - window-state restoration;
  - scene storage.
- **Exposure.** A searched imported value, such as a client's name, would then survive outside the user's file, even after the collection is deleted.
- **A second problem with UJ9.4-b.** Its search string "zx-01" is the start of the stored codes ZX-010 to ZX-015. If ADR-0003 stores a lower-cased copy of each code for Import R2.3 matching (not confirmed), a byte scan cannot tell leftover search text from stored data.
- **Remediation:**
  - R8.10 grants a readback of what the app writes outside the file (its preferences, container and saved state).
  - UJ9.4-b asserts the search text appears nowhere there after close, and switches to a string found nowhere in the fixture ("qqq", which UJ3.1-f already uses).
- (Art. 5(1)(e); DF R1.1)

**[LOW] PRIV-6 — No content limit is handed to the future Telemetry PRD**
- **Where:**
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode.md:583` (R8.6), `:602-608` (outbound obligations)
  - Device `docs/product/device-management/prd-device-management.md:149` (R2.14)
  - DF `docs/product/data-foundation/prd-data-foundation.md:103` (R1.4)
- **The gap.**
  - Opt-in telemetry is P1 and gets its own future PRD.
  - DF R1.4's content limit ("no item, reading or payload") does not clearly cover search text, Find-similar targets, collection names or column names.
  - R8.6 keeps all of these on the machine, but no outbound obligation line hands that rule to the Telemetry PRD.
- **Risk.** A future "search used" or "column renamed" event could carry the typed text without contradicting anything the Telemetry author is told to read.
- **Remediation.** Add an outbound obligation to the Telemetry PRD: any event about a Collection Mode action carries no typed text, code, name, imported value, collection name or column name; at most, counts.
- (Art. 5(1)(b)/(c))

**[INFO] PRIV-7 — "Export first" on a selection delete exports the whole collection.** F24 settled this. Export E1's copy names the collection and swatch count and says serials are included, all before anything is written, so the user can see the scope. Optionally, DF E33's "Export first" line could say "all of ⟨collection⟩".

**[INFO] PRIV-8 — Hiding a column (R2.10) is a view choice, not a redaction.** Export R2.3 still emits every imported column, and the file keeps it. No copy says otherwise, so this is not a finding. The help docs could say it.

**[INFO] PRIV-9 — Serials and measurement times can be erased only by deleting the item.** This follows from the history non-negotiable (R5.6, R8.2; DF R6.1 and R2.6; AGENTS.md §4). It is settled and honestly disclosed in E17's copy. Noted, not raised.

## Biggest privacy risks
1. Deletion and clearing can pass every test without actually removing the text (PRIV-1). The file is meant to be shared, so this is the realistic disclosure path.
2. A crash can leave the undo stack in the file, holding exactly the values the user was told were gone (PRIV-2).
3. Traffic that waits for a connection is invisible to a check that runs only offline (PRIV-3).

## Genuinely privacy-respecting
- **Search stays out of the file.** R8.6's second sentence, with UJ9.4-b, never writes search text, filters or sorts to the file. Not keeping a search history is correct minimization, not a missing convenience.
- **Column visibility is stored in the user's file (R2.10, F21).** A checklist might call "UI state in the data file" over-collection. It isn't: the value is not personal, it travels and is deleted with its collection, and nothing about a collection lives outside the user's control.
- **Showing the device serial locally (R4.2d, R5.2b) is not a new flow.** DF R6.5 already lists it in the file's privacy inventory, and Export E1 discloses it.
- **The All items view shows no imported columns (R1.9, F5)**, which keeps free text off one more surface.
- **Find similar (R3.7, E9) compares only the user's own swatches** and never calls an outside colour library or lookup service.
- **Deletes are guarded and explained.** R1.6 makes Return fire cancel and never makes delete the default. DF E33's copy lists exactly what is removed, including "everything recorded about scanning them". E17's copy states honestly that only a whole swatch or collection can be erased.
- **Renaming a column keeps no shadow copy (R4.8, Import F65).** A header with the old name arrives as a new column.
- **Bulk clear is all-or-nothing (R8.8)**, so it never leaves a half-cleared state.
- **Every journey fixture is synthetic, serials included.** The bundle sent to the cross-model reviewer (with the owner's consent, per the review log) contains no real personal data.

## Missing controls / over-collection
- A readback of the file's bytes, and of anything the app writes outside the file (PRIV-1, PRIV-5).
- In R8.10: a read of the file while the app is open, and a read after a crash but before reopening (PRIV-2, PRIV-4).
- A no-network case with the network reachable (PRIV-3).
- An outbound obligation giving the Telemetry PRD its content limit (PRIV-6).
- A definition of DF R6.2a's "outside reader" (PRIV-1c).
- No over-collection found: this PRD adds only per-collection view state, and nothing personal.

| Row ID | disposition |
|---|---|
| R1.1 | ABSTAIN (out of lens) |
| R1.2 | ABSTAIN (out of lens) |
| R1.3 | ALIGN |
| R1.4 | ALIGN |
| R1.5 | ABSTAIN (out of lens) |
| R1.6 | ALIGN |
| R1.7 | OBJECT (PRIV-4) |
| R1.8 | ALIGN |
| R1.9 | ALIGN |
| R1.10 | ALIGN |
| R2.1 | ABSTAIN (out of lens) |
| R2.2 | ABSTAIN (out of lens) |
| R2.3 | ABSTAIN (out of lens) |
| R2.4 | ABSTAIN (out of lens) |
| R2.4a | ABSTAIN (out of lens) |
| R2.4b | ABSTAIN (out of lens) |
| R2.4c | ABSTAIN (out of lens) |
| R2.4d | ABSTAIN (out of lens) |
| R2.4e | ABSTAIN (out of lens) |
| R2.4f | ABSTAIN (out of lens) |
| R2.4g | ABSTAIN (out of lens) |
| R2.4h | ABSTAIN (out of lens) |
| R2.4i | ABSTAIN (out of lens) |
| R2.5 | ABSTAIN (out of lens) |
| R2.6 | ABSTAIN (out of lens) |
| R2.7 | ABSTAIN (out of lens) |
| R2.8 | ABSTAIN (out of lens) |
| R2.9 | ABSTAIN (out of lens) |
| R2.10 | ALIGN |
| R3.1 | ALIGN |
| R3.2 | ABSTAIN (out of lens) |
| R3.3 | ABSTAIN (out of lens) |
| R3.4 | ABSTAIN (out of lens) |
| R3.5 | ABSTAIN (out of lens) |
| R3.6 | OBJECT (PRIV-5) |
| R3.7 | ALIGN |
| R3.8 | ABSTAIN (out of lens) |
| R4.1 | ABSTAIN (out of lens) |
| R4.2 | ALIGN |
| R4.2a | ALIGN |
| R4.2b | ABSTAIN (out of lens) |
| R4.2c | ABSTAIN (out of lens) |
| R4.2d | ALIGN |
| R4.2e | ABSTAIN (out of lens) |
| R4.2f | ABSTAIN (out of lens) |
| R4.2g | ABSTAIN (out of lens) |
| R4.2h | ABSTAIN (out of lens) |
| R4.3 | OBJECT (PRIV-1) |
| R4.4 | ALIGN |
| R4.5 | ALIGN |
| R4.6 | ABSTAIN (out of lens) |
| R4.7 | OBJECT (PRIV-2) |
| R4.8 | ALIGN |
| R4.9 | ABSTAIN (out of lens) |
| R5.1 | ALIGN |
| R5.2 | ABSTAIN (out of lens) |
| R5.2a | ABSTAIN (out of lens) |
| R5.2b | ALIGN |
| R5.2c | ABSTAIN (out of lens) |
| R5.2d | ABSTAIN (out of lens) |
| R5.2e | ABSTAIN (out of lens) |
| R5.3 | ABSTAIN (out of lens) |
| R5.4 | ABSTAIN (out of lens) |
| R5.5 | ABSTAIN (out of lens) |
| R5.6 | ALIGN |
| R5.7 | ABSTAIN (out of lens) |
| R5.8 | ABSTAIN (out of lens) |
| R6.1 | ABSTAIN (out of lens) |
| R6.2 | OBJECT (PRIV-1) |
| R6.3 | OBJECT (PRIV-1) |
| R7.1 | ABSTAIN (out of lens) |
| R7.2 | ABSTAIN (out of lens) |
| R8.1 | ABSTAIN (out of lens) |
| R8.2 | ALIGN |
| R8.3 | ABSTAIN (out of lens) |
| R8.4 | ALIGN |
| R8.5 | ALIGN |
| R8.6 | OBJECT (PRIV-3, PRIV-6) |
| R8.7 | ABSTAIN (out of lens) |
| R8.8 | ALIGN |
| R8.9 | ABSTAIN (out of lens) |
| R8.10 | OBJECT (PRIV-1, PRIV-2) |
| E1 | ABSTAIN (out of lens) |
| E2 | ABSTAIN (out of lens) |
| E3 | ABSTAIN (out of lens) |
| E4 | ABSTAIN (out of lens) |
| E5 | ABSTAIN (out of lens) |
| E6 | ABSTAIN (out of lens) |
| E7 | ABSTAIN (out of lens) |
| E8 | OBJECT (PRIV-1) |
| E9 | ALIGN |
| E10 | ALIGN |
| E11 | ABSTAIN (out of lens) |
| E12 | ABSTAIN (out of lens) |
| E13 | ABSTAIN (out of lens) |
| E14 | ABSTAIN (out of lens) |
| E15 | ABSTAIN (out of lens) |
| E16 | ABSTAIN (out of lens) |
| E17 | ALIGN |
| M1 | ABSTAIN (out of lens) |
| M2 | ABSTAIN (out of lens) |
| M3 | ABSTAIN (out of lens) |

#### peer-product-marketing-manager-reviewer (Claude route)

## Verdict
Lands after fixing Blockers. The copy is plain and calm, it reuses sibling wording, and most of its claims hold up. One definition in the honesty legend gives a false picture of what the user's file holds, and several mark names and actions set the Cataloger up to misread what they can trust or to do the wrong thing.

## Audience & message context (brief)
The reader is the Cataloger: a non-expert who owns a physical collection (markers, paints, a swatch book), has scanned it, and is now browsing, searching and fixing it. After reading, they should be able to say "I know which colours on screen are true, which numbers I can trust, and what each action will do to my data." The surface is in-app UI copy (E1–E17), plus the sibling state this change adds (Data Foundation E33) and the sibling states shown on these surfaces. Plain, honest UI copy is the right register here, not a launch narrative. I read all four target files, the fence file, the round-0 fixes, the review log, the sibling copy for every state shown on these surfaces, and `git diff origin/main` of the sibling PRDs.

## Findings

[BLOCKER] E12 "Outside sRGB" definition, /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode-copy.md:210
- **The problem:** it reads "the colour falls outside standard sRGB, the reference your exports and your file use". A Cataloger takes this to mean their file and exports are sRGB, so the colour was not really stored. That is false.
  - The export carries "wavelength readings and six colour spaces — XYZ, Lab, LCh, Luv, sRGB and HSL" (/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/export/prd-data-export-copy.md:16).
  - The flag concerns only "this value's sRGB derivation" (Data Foundation R3.4, /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/data-foundation/prd-data-foundation.md:145).
  - Only the sRGB and HSL columns carry the gamut qualifiers (Export R2.1, /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/export/prd-data-export.md:117).
- **What the reader misses:** the definition leaves out the one thing they cannot trust: the sRGB and HSL numbers they would paste into a design tool. It also casts doubt on the product's core promise, measurement-grade fidelity, in the one place meant to build trust.
- **Mutation:** trace each claim in E12 to a row under copy-honesty rule 5 ("a string promises only what a row delivers").
  - With this clause in place the trace fails: no row says sRGB is the reference the file or the export uses, and Export E1 contradicts it.
  - With the rewrite below the trace passes (Data Foundation R3.4 plus Export R2.1).
  - Reader-level check: take UJ2.1-c's ZX-002 on Display P3, which shows Outside sRGB only, and ask "which of its values are approximate?" Today the legend answers "the file's"; the true answer is "its sRGB and HSL".
- **Rewrite:** "Outside sRGB: this colour is more saturated than standard sRGB can hold, so its sRGB and HSL values — here, in your file and in exports — are the nearest sRGB colour, not the colour itself. Its Lab, XYZ and spectral values are unaffected."
- This keeps F6's two separate marks; it changes only the wording.

[MAJOR] E12 / R2.4f vs R2.4g: "Value missing" and "No current value" (copy:210; /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode.md:439-440)
- **The problem:** the two labels are near-synonyms. They sit side by side in the legend, in the Filters list (R3.4 filters by every mark) and in VoiceOver names. Both show an empty chip.
- **Reader reaction:** a Cataloger filtering for "swatches with no colour" picks "Value missing" and gets only ZX-006, which was scanned but has no reading under this collection's condition. They miss ZX-010 and ZX-011, which have never been scanned.
- "No current value … or it's set aside" also overlaps "Unreadable … the swatch is set aside".
- **Rewrite:**
  - Rename "Value missing" to "Not in this condition: it has a reading, but none under the measurement condition this collection is set to."
  - Change the other definition to "No current value: the swatch hasn't been scanned, or it's set aside for a reason other than an unreadable reading."

[MAJOR] E12 / R2.4i: the "Re-scan to answer" label (copy:210; prd:442)
- **The problem:** on a chip, in a filter, or read out by VoiceOver ("ZX-013, Teal, captured, re-scan to answer"), the label sounds like an instruction to re-scan.
- **Reader reaction:** the reader re-scans. That adds another reading and, after the session, another unanswered question (capture E29), which is the opposite of what the mark is for.
- **Rewrite:**
  - Label: "Re-scan unanswered" (it pairs with E2's "Answer re-scans").
  - Definition: "it was scanned again and you haven't said whether the swatch changed or the old reading was wrong. Answer it from the swatch, or with Answer re-scans."

[MAJOR] R4.2b: the item detail's "settled / not settled" state line (prd:487; UJ4.1-c, /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode-journeys.md:219) clashes with E12's "Not settled" (copy:210)
- **The problem:** "not settled" means two different things on neighbouring screens.
  - Every user-facing sibling uses "not yet settled" for a reading waiting on the correction question: Data Foundation E11 and E26 (/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/data-foundation/prd-data-foundation-copy.md:37-38) and capture E29.
  - R4.2b shows the capture PRD's internal row concept ("set aside, cause unreadable, not settled").
- **Reader reaction:** on ZX-012, the reader opens the legend and is given the wrong meaning.
  - Set-aside rows are common (every failed scan), so this happens often.
- **Rewrite:** have the state line use capture's user-facing words for a deliberately-left row, "set aside for good" (capture E2/E24), against "set aside, still to deal with". Keep "Not settled" for the reading-level mark only.
- F22 settles the idea of an unsettled row, not the words shown for it.

[MAJOR] R2.8 / E12: the legend has no shapes and is not reachable where the marks appear (prd:449, :670; copy:204-211)
- **The problem:** R2.8 renders names and meanings only. The reader opens "Colour marks" to decode the glyph on a chip, and a text-only list cannot be matched to glyphs.
- E12 lists only the collection surface and the All items view. The item detail and the version history, where "Never right", "Not settled" and the reference mismatch appear, have no way to reach it.
- **Rewrite R2.8 as:** '"Colour marks" renders E12, showing each mark's shape (R8.9) beside its name and meaning, and is offered in the item detail and the version history view as well'.
- Lay out E12 as a grouped list rather than one run-on paragraph. Suggested groups:
  - The chip isn't the true colour
  - Where the reading came from
  - No colour to show
  - Waiting on you
  - Earlier readings

[MAJOR] R4.9: the collection-side "Flag" gives no word on what it does (prd:503; E14, copy:230)
- **The problem:** in a browsing app, "Flag" usually means "mark for attention". Here it empties the chip, removes the current value, sets the swatch aside and makes the collection unfinished. Metadata Undo (R4.7) does not reverse it; the only way back is P1 "Use this reading".
- **Reader reaction:** the Cataloger flags a swatch as a bookmark and loses its current value without warning.
- The label itself is settled (Capture §12 Labels, F10), so don't rename it. Add copy about the consequence instead, which needs a new row and state. Suggested confirmation:
  - Headline: "Set ⟨code⟩ aside to scan again?"
  - Body: "Its current reading moves to its history, and it has no colour until you scan it again or use that reading again."
  - Actions: "Set it aside", "Cancel".

[MAJOR] R4.8 / E11: "Rename column" gives no warning about imports (prd:502; copy:194-202)
- **The problem:** E16 warns about exactly this for codes ("Imports find swatches by their code…"). A renamed column gets no such warning.
- **Reader reaction:** the Cataloger renames Family to Hue Family, then re-imports their master spreadsheet. The import adds a second "Family" column, and nothing in v1 removes a column (at P2 it can only be hidden).
- F9 settles the behaviour, not whether the user is warned.
- **Rewrite:** a confirmation that mirrors E16:
  - Headline: "Rename ⟨column⟩ to ⟨text⟩?"
  - Body: "Imports match columns by name, so a spreadsheet that still has a ⟨column⟩ column will add it as a new column instead of filling this one. Every value in it stays as it is."
  - Actions: "Rename column", "Keep this name".

[MINOR] E6 body (copy:150)
- **The problem:** "…and flagging a swatch wait until you end that session" reads as if the action is queued. R8.3 refuses and changes nothing. For a Flag, the reader believes the swatch will be re-queued, and it silently isn't.
- "Under way, paused or held" also differs from Data Foundation E9 and E15's "active, paused or halted".
- **Rewrite:** "Scanning in ⟨collection⟩ is active, paused or halted. Deleting swatches or the collection, changing a swatch's code and flagging a swatch aren't available until you end that session. Nothing has been changed — do it again once the session has ended."

[MINOR] "Spacing and capitals don't count as a difference" (E4, copy:132; E11, copy:202; E15, copy:240)
- **The problem:** Import R2.3 trims spaces at the ends and collapses runs of spaces; it does not remove them (/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/import/prd-inventory-import.md:67). So "skyblue" does not match "Sky Blue".
- E4 appears exactly when a search has failed, so a reader who typed "skyblue" is told spacing doesn't matter.
- **Rewrite:** "Capitals and extra spaces don't count as a difference."
- Capture E1, which is aligned, uses the same phrase; carry the fix there at its next amendment.

[MINOR] E5's ⟨n⟩ (copy:141)
- **The problem:** it is undefined whether ⟨n⟩ counts the whole collection or what the search lists. With a search also active (UJ3.2-d), "13 swatches are hidden by the filters" is false, because the search had already hidden them.
- **Fix:** define ⟨n⟩ for E5 in the Placeholders rule (prd:681) as "the swatches the search lists, or the collection's when no search is active". The zero-count rule then drops the sentence cleanly.

[MINOR] E8 (copy:169, :171)
- **The problem:** neither variant tells the reader that Undo exists (R4.7 lands in the same phase). Only the "clear" variant says replaced values aren't kept in the file, although "set" replaces values in the same way. So clearing sounds more dangerous than setting, when both behave the same.
- **Fix:** end both variants with "What was there isn't kept in your file; you can undo this while the file stays open."

[MINOR] E14 body (copy:229)
- **The problem:** "A change saves when you press Return…" is false when the file is open read-only (R8.4, prd:581; UJ9.2-a). The reader tries to type.
- **Fix:** add a read-only variant, listed by R8.4: "Your file is open read-only, so nothing here can be changed."

[MINOR] Displayed text that no copy entry writes
- **The problem:** the builder will make these up, or the reader won't understand them.
  - The "Spread" column (R2.7, prd:448) shows 0.4 or 3.1 with no unit or meaning, and E12 doesn't explain it.
  - R4.2e's reference-mismatch line (prd:490) has no wording.
  - R5.4's "absent and marked" distance (prd:529) has no wording.
- **Fix:**
  - Add to E12: "Spread: the largest difference, in ΔE2000, between one of its samples and their average" (capture R4.9).
  - Add copy entries for the mismatch line and the "not compared" distance.

[MINOR] E12 "Unreadable" definition (copy:210)
- **The problem:** "its current reading can't be read" doesn't say it is the reading saved in the file that is damaged. Next to capture's set-aside causes (light leak, temperature, drift; capture R8.2), the reader takes it for a scanning failure and blames the swatch, not the file.
- The label is settled by F11; the definition is not.
- **Rewrite:** "Unreadable: the reading saved in your file is damaged and can't be read, so the swatch is set aside to scan again or to go back to an earlier reading."

[MINOR] Data Foundation E33, a sibling state (/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/data-foundation/prd-data-foundation-copy.md:27)
- **The problem:** "Export first" opens the export of the whole collection (F24), but the text doesn't say so. Deleting 3 of 500 swatches, the reader lands on "Export Studio Markers … 500 swatches" and wonders what they are about to export or delete.
- **Fix:** in E33's version of the export sentence, name the scope: "Export first saves all of ⟨collection⟩, these swatches included — …". The behaviour is settled; the wording is not.

[MINOR] E17 (copy:257-259)
- **The problem:** nothing tells the reader what P1 "Use this reading" does, or that nothing is lost. A reader worried about their current reading hesitates, or assumes it will be overwritten.
- **Fix:** add a line tied to the action's phase: "Use this reading makes an earlier reading the current one again; the reading it replaces stays in the history."

[NIT] E15's action "OK" (copy:239) behaves differently from E7 and E11's "Change the name", which return the reader to editing; the three parallel refusals should recover the same way. Use "Try another code" (not "Change the code", which is already E16's label).

[NIT] E5's headline says "passes" while E3 and E4 say "match". Use "No swatch matches these filters".

[NIT] E17's body says "the most recently saved first" while its action is "Recorded order". Use "most recently recorded first".

[NIT] E9 says "no current colour" while E12 says "No current value". Use one term.

[NIT] The Device PRD's obligation line (/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/device-management/prd-device-management.md:266) says "simulated badge", while this PRD and E12 say "mark". The wording already exists in Device R6.5; align it at the next change.

## Biggest risks   (what misleads, confuses, or loses the reader)
- **The honesty legend:** it is the product's main tool for honesty and it currently:
  - misstates what the file and exports hold (the BLOCKER);
  - has two indistinguishable "no value" labels;
  - has one label that reads as an order to re-scan;
  - uses "Not settled" for something the item detail uses the same words for, with a different meaning;
  - shows no shapes to decode.

  A Cataloger who can't decode the marks gets no benefit from them.
- **Two actions that change data with no warning:**
  - "Flag" removes the swatch's current value, where the reader expected a bookmark.
  - "Rename column" leads to a duplicate column that can't be removed.

  Both surprise the reader after it is too late to undo.
- **E6's wording** ("wait until") can leave a reader believing a Flag or delete is queued.

## Genuinely strong   (incl. where plain-and-honest is right that a marketing-zealot would over-hype)
- E4 explains the search rule at the moment a search fails, which turns a dead end into something the reader learns from.
- E16 is a model confirmation: it states the consequence in the user's terms (imports match by code), reassures them ("keeps its readings and history"), and offers two plain choices.
- E17 turns a rule that can't be broken ("deleting the swatch or its collection is the only thing that throws a reading away") into one sentence a user can read.
- E10 states the undo window honestly, including a crash, and matches Data Foundation E8's wording.
- E9 limits its own claim: it counts the swatches it didn't compare and says why ("under a different light or measurement condition"). It also states the no-colour-library limit without naming any competitor.
- E12 reuses sibling wording, so a mark means the same thing wherever the reader first met it:
  - "came out further apart than expected" (capture E18)
  - "not the curve behind them … under another light" (capture E43)
  - "Demo Device, not an instrument" (device E22)
- "Can't show on this screen" is an exact, plain statement about the display.
- R2.3's "never a stand-in colour" puts honesty into the product's behaviour, not just its words.
- The voice is calm throughout: no speed claims and no hype. Performance budgets sit in the rows, not in the copy. For in-app UI copy that is the right choice, and nothing here needs more marketing.

## Missing / over-hyped
- **Over-hyped:** nothing. No claim overreaches in tone.
- **Missing:**
  - warning copy for Flag (R4.9) and for Rename column (R4.8/E11);
  - shapes in the legend, and a way to reach it from the item detail and the version history;
  - copy for Spread, the reference mismatch and the "not compared" distance;
  - a read-only variant of E14;
  - a statement that Undo exists, in E8.
- **Optional and small:** M2 checks that marks are *correct*, not that they are *understood*. A one-time dogfood reading of E12 would catch the next naming collision. The owner plus one or two Catalogers would each say what they can and cannot trust about ZX-002, ZX-004 and ZX-012. That is right-sized for a local tool; no funnel metric is needed.

| Row ID | disposition |
|---|---|
| R1.1 | ALIGN |
| R1.2 | ALIGN |
| R1.3 | ALIGN |
| R1.4 | ALIGN |
| R1.5 | ALIGN |
| R1.6 | ABSTAIN |
| R1.7 | ALIGN |
| R1.8 | ALIGN |
| R1.9 | ALIGN |
| R1.10 | ALIGN |
| R2.1 | ALIGN |
| R2.2 | ALIGN |
| R2.3 | ALIGN |
| R2.4 | ALIGN |
| R2.4a | ALIGN |
| R2.4b | ALIGN |
| R2.4c | ALIGN |
| R2.4d | ALIGN |
| R2.4e | ALIGN |
| R2.4f | ALIGN |
| R2.4g | ALIGN |
| R2.4h | ALIGN |
| R2.4i | ALIGN |
| R2.5 | ABSTAIN |
| R2.6 | ALIGN |
| R2.7 | OBJECT (MINOR: text with no copy home — Spread) |
| R2.8 | OBJECT (MAJOR: legend without shapes / reachability) |
| R2.9 | ALIGN |
| R2.10 | ABSTAIN |
| R3.1 | ALIGN |
| R3.2 | ABSTAIN |
| R3.3 | ABSTAIN |
| R3.4 | ALIGN |
| R3.5 | ALIGN |
| R3.6 | ABSTAIN |
| R3.7 | ALIGN |
| R3.8 | ALIGN |
| R4.1 | ABSTAIN |
| R4.2 | ALIGN |
| R4.2a | ALIGN |
| R4.2b | OBJECT (MAJOR: "not settled" collision) |
| R4.2c | ALIGN |
| R4.2d | ALIGN |
| R4.2e | OBJECT (MINOR: text with no copy home — reference mismatch) |
| R4.2f | ALIGN |
| R4.2g | ALIGN |
| R4.2h | ALIGN |
| R4.3 | ALIGN |
| R4.4 | ALIGN |
| R4.5 | ALIGN |
| R4.6 | ALIGN |
| R4.7 | ALIGN |
| R4.8 | OBJECT (MAJOR: Rename column gives no import-consequence warning) |
| R4.9 | OBJECT (MAJOR: Flag consequence unstated in the browsing context) |
| R5.1 | ALIGN |
| R5.2 | ALIGN |
| R5.2a | ALIGN |
| R5.2b | ALIGN |
| R5.2c | ALIGN |
| R5.2d | ALIGN |
| R5.2e | ALIGN |
| R5.3 | ALIGN |
| R5.4 | OBJECT (MINOR: text with no copy home — absent distance) |
| R5.5 | ALIGN |
| R5.6 | ALIGN |
| R5.7 | ALIGN |
| R5.8 | ABSTAIN |
| R6.1 | ABSTAIN |
| R6.2 | ALIGN |
| R6.3 | ALIGN |
| R7.1 | ABSTAIN |
| R7.2 | ABSTAIN |
| R8.1 | ABSTAIN |
| R8.2 | ABSTAIN |
| R8.3 | ALIGN |
| R8.4 | ALIGN |
| R8.5 | ABSTAIN |
| R8.6 | ABSTAIN |
| R8.7 | ABSTAIN |
| R8.8 | ABSTAIN |
| R8.9 | ALIGN |
| R8.10 | ABSTAIN |
| E1 | ALIGN |
| E2 | ALIGN |
| E3 | ALIGN |
| E4 | OBJECT (MINOR: spacing overclaim) |
| E5 | OBJECT (MINOR: ⟨n⟩ undefined with a search active) |
| E6 | OBJECT (MINOR: "wait until" implies a queue; "held" vs "halted") |
| E7 | ALIGN |
| E8 | OBJECT (MINOR: no Undo sentence; set/clear asymmetry) |
| E9 | ALIGN |
| E10 | ALIGN |
| E11 | OBJECT (MAJOR: no rename-import warning; MINOR: spacing overclaim) |
| E12 | OBJECT (BLOCKER: Outside sRGB claim; MAJOR: Value missing vs No current value; MAJOR: "Re-scan to answer"; MAJOR: no shapes; MINOR: Unreadable definition; MINOR: Spread gloss) |
| E13 | ALIGN |
| E14 | OBJECT (MINOR: body false when read-only) |
| E15 | OBJECT (MINOR: spacing overclaim; NIT: "OK" vs "Change the name") |
| E16 | ALIGN |
| E17 | OBJECT (MINOR: "Use this reading" consequence unstated) |
| M1 | ABSTAIN |
| M2 | ABSTAIN |
| M3 | ABSTAIN |

#### peer-architecture-reviewer (Claude route)

## Verdict
Sound, build it. There is no Blocker and nothing needs redesigning, but close the five Majors before lock. Each one is a rule missing at a seam between documents: an undo, an identity change, a colour crossing from the Rust core to the Swift shell, a mid-session restore, and the read-only floor.

## Architecture in brief
- **What this PRD owns:** Collection Mode is the browsing and editing layer over Data Foundation's single-writer SQLite file.
  - The collection list, the table, the item detail and the version history view.
  - Search, filter and sort state, held in memory for the open file and never written to it (R3.6, R8.6).
  - The one environment-dependent honesty mark, cannot-show (R2.5).
  - Metadata edits, and entry points into actions its siblings own.
- **What the siblings own:**
  - Data Foundation owns the persisted facts: readings, supersession reasons, derived sets, the stored gamut-clipped flag, quarantine, and deletion semantics.
  - Capture owns row state, queue order and session state.
  - Import owns the identifier comparator (its R2.3); ADR-0003 owns how that comparator's data version is governed.
  - ADR-0004 owns the navigation fork. The Build contract (prd-collection-mode.md:109-115) explicitly leaves it unselected.
- **The key tradeoff:** every mark is read from persisted reading facts except cannot-show, which is computed live from the display. Data Foundation R1.5 refuses a second running copy of the app, so capture writing while the user edits is a matter of ordering writes inside one core, not of reconciling separate processes.
- **Verdict on the brief's question:** no row outright pre-decides ADR-0003, ADR-0004 or ADR-0006. Several rows impose constraints on ADR-0003 (R4.4, R2.10) or on Data Foundation's open questions (R8.4) without a sibling half or an ADR input.

## Findings
Paths are relative to /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/.

- **[MAJOR] A1. Undo (R4.7) over a file that other features also write: undo can break uniqueness.** docs/product/collection-mode/prd-collection-mode.md:501.
  - R4.7 keeps a most-recent-first stack "while the file stays open". It never says what invalidates an entry, or that reversing an entry must pass the forward action's acceptance rule.
  - Scenario 1, through a code change:
    1. Change ZX-001 to ZX-100 (R4.4).
    2. Re-import the spreadsheet. As E16 warns (copy.md:248), it creates a new pending ZX-001.
    3. Undo. The code reverts, and the collection now holds two ZX-001 items.
  - Scenario 2, through a column rename: after UJ4.6-f's state (journeys.md:245: rename Family to Hue Family, then an import appends a new Family column), Undo renames Hue Family back to Family, giving two columns named Family.
  - Both results break the uniqueness that R4.4, R4.8 and Import R2.3/R2.6 enforce, and make later import matching ambiguous. The stack can also overwrite values an import or a re-read (R8.5) changed since, or target an item that has been deleted.
  - Mutation that passes today's cases: T10 and UJ6.2-f undo only in isolation, so a build that never clears or re-validates its stack passes both.
  - Simplest fix: clear the stack on any import commit into the collection, on Data Foundation's re-read, and on a delete of an entry's target. A code or column-name reversal re-checks R4.4's and R4.8's rules and renders E15 or E11 when refused. Add the two scenarios above as cases.

- **[MAJOR] A2. R4.4 makes the item's identity field changeable, with no Data Foundation or ADR-0003 counterpart.** prd-collection-mode.md:498, and the build-dependency row at :304, whose "What must remain open" cell says "nothing".
  - T7 and UJ4.3-a require the same item to persist under ZX-100 with its readings. That only holds if item identity is independent of Swatch Code in the file.
  - No sibling says so:
    - Data Foundation's rows (R1.2, R7.1 "item identity") never make the code changeable.
    - The Outbound table (:602-608) has no R4.4 line.
    - ADR-0003's queue entry (docs/decisions/README.md:22) does not list it.
  - R4.4 also runs while an interrupted session holds a remembered row (only an in-flight session blocks it), so Capture's references must be by identity too.
  - Change scenario: ADR-0003 is ratified keyed on the code, since import matches by code. When R4.4 lands at P1 it then forces a key migration of every reading and history row inside users' files. That is the sharpest one-way door the project has.
  - Fix:
    - Add an Outbound line to Data Foundation: an item's identity is independent of and unchanged by a Swatch Code change, and an outside reader at SQLITE_READER_FLOOR sees the same item under its new code.
    - Record that line as a Data Foundation fence and as an ADR-0003 input.
    - Gate the build-dependency row on ADR-0003.

- **[MAJOR] A3. R2.3 does not say which value the chip renders across the core-to-shell boundary, and no test reads the rendered colour.** prd-collection-mode.md:427, with R8.10 at :587.
  - "Renders the item's current value … as the display showing it renders that value" is ambiguous.
  - Data Foundation R3.4 (data-foundation/prd-data-foundation.md:145) keeps the stored sRGB derived value mapped to sRGB under relative colorimetric, with the gamut-clipped flag.
  - A shell that renders that stored triple shows ZX-002 clipped on a Display P3 display. It also shows no cannot-show mark, because UJ2.1-c (journeys.md:165) expects none there. The chip then presents a substituted colour as faithful, which is the non-negotiable this app exists to hold.
  - Mutation that passes today's suite: render from the stored sRGB value. M2 still reads 100%, because R8.10 lists marks and accessible descriptions but never the colour a chip was asked to render.
  - Fix:
    - R2.3: on a display whose gamut contains the value, the chip renders the working-set colorimetric value itself, whatever the stored flag says.
    - R8.10: read back each chip's requested colour in the display's space.
    - Add a case: ZX-002 on Display P3 renders coordinates outside sRGB.

- **[MAJOR] A4. Restoring during an in-flight session changes Capture's row state with no guard.** prd-collection-mode.md:530 (R5.5), :500 (R4.6), :580 (R8.3).
  - R8.3 refuses Flag while a session is in flight, which E6 says is "so nothing it's working on moves under it". The inverse moves are not refused:
    - R5.5's "Use this reading" on a flagged or unreadable set-aside item.
    - R4.6's Data Foundation E4 "Use a previous reading", which is P0.
  - Capture's transitions (capture-mode/prd-capture-mode.md:22-23) accept set aside → captured on restore with no session condition.
  - Capture R10.4 (:336) keeps the collection surface reachable without ending a session.
  - Scenario:
    1. A bulk session on Studio Markers is paused.
    2. The user opens ZX-012 and fires E4's restore.
    3. ZX-012 becomes captured mid-session. Neither Capture R8.1's state-by-exit table, nor the session's own tallies, nor the set-aside list defines a row leaving by that route.
  - Fix (owner call): either add both restore routes to R8.3's E6 list, which matches Flag and is the simpler option, or give Capture a row stating what a live session does when a set-aside row is restored under it. Add cases either way.

- **[MAJOR] A5. R8.4 widens Data Foundation's read-only floor and pre-empts that PRD's open question 18.** prd-collection-mode.md:581.
  - R8.4 promises that a read-only file "still show[s] every item, current value and mark", and forbids only write actions.
  - Data Foundation R5.3 (data-foundation/prd-data-foundation.md:173) guarantees only the item list, canonical values and history marks. For a newer-format file (E1) it also forbids "partially interpreted as complete".
  - Data Foundation's open question 18 (:337: "additional … search capabilities remain open. Owner, then ADR-0003") owns anything beyond that floor.
  - For E1, "every mark" requires the older app to decode a newer schema. That is a forward-compatibility constraint on ADR-0003 that nobody has decided.
  - Fix:
    - For E1, restate Data Foundation R5.3's floor. Leave search, filter, sort, Find similar, and marks this build cannot read to Data Foundation's open question 18.
    - Keep "every mark" only for formats this build reads (E16/E23/E24).
    - Split UJ9.2-a (journeys.md:333) accordingly.

- **[MINOR] A6. R8.1's write-to-table clause and UJ9.1-a are observable only under reading B of the open navigation fork.** R8.1 :578; UJ9.1-a journeys.md:331.
  - Capture R10.6 (:338) defines reading A as capture as a full-window takeover that hands off at session end, and reading B as capture as a state of the live collection.
  - "A reading … saved anywhere … show[s] in the table within BROWSE_RESPONSE_BUDGET" while a session is in flight describes a table on screen during capture.
  - Under reading A, the takeover holds the window and capture input is inert without focus (Capture R10.4). The table is off screen while readings save.
  - The row therefore charges reading A the cost of keeping a hidden table live-synced, which tilts ADR-0004's comparison.
  - Fix: "while the table is shown, and whenever it is next shown". Assert UJ9.1-a at the next showing so it holds under both readings.

- **[MINOR] A7. R2.10 stores a new kind of record in the user's file, but no Data Foundation or ADR-0003 counterpart exists.** :451.
  - Fence F21 is settled, and this finding does not question it. The gap is ownership: Data Foundation "owns the file and what it keeps" (Build contract), yet no Outbound line (:602-608), no Data Foundation row, R7.1 readback or R7.7 fixture, and no ADR-0003 scope item names per-collection view state.
  - R8.8's atomic-write list also omits it, and read-only mode (R8.4) silently removes hide/show.
  - Fix:
    - Add an Outbound line and a Data Foundation fence.
    - Record it as an ADR-0003 input.
    - Add it to R8.8's list.
    - State what hide/show does on a read-only file.

- **[MINOR] A8. The All items view compares values across different working sets.** R1.10 :415, R3.3 :463, R3.7 :467.
  - R1.10 carries the L*, C* and h° columns and sorts into a view spanning collections whose working sets (illuminant, observer and condition) may differ. It then orders values the rest of the PRD (fences F16 and F27) refuses to compare.
  - R3.7's "values worked out under the chosen item's …" does not say whether an existing non-working-set derived set may be used. It also does not say that Find similar never triggers Data Foundation R3.3f's compute-and-persist (prd-data-foundation.md:159). That would make a browse action write the file, and fail on a read-only file.
  - Every harness collection is M1 at D50/2° (journeys.md:56, :70), so no case would expose any of this.
  - Fix:
    - State what those cells show and how sorts treat items whose working set differs, and that Find similar never computes a set.
    - Add a fixture collection with a different working set.

- **[MINOR] A9. The locale is an undeclared input to ordering.** R1.1 :406, R3.2 :462, R8.10 :587.
  - Both rows inherit Capture R6.9 ("Sorts respect the user's language", capture-mode/prd-capture-mode.md:236).
  - Neither R8.10 nor the harness's Named defaults declares the language, although UJ1.4-b and UJ3.3-a/b/e depend on it.
  - Ordering is a function of an OS input, and possibly of which collation library runs (the core's or Foundation's). That undermines ADR-0001's deterministic headless core tests.
  - Fix: make the user's language a declarable R8.10 input with a named default.

- **[MINOR] A10. The display gamut crosses into the test seam and into filtering.** R2.5 :446, R3.4 :464, R6.1 :543.
  - The test seam names three gamuts (sRGB, Display P3, none). That invites a three-value enum at the core-to-shell interface, which would mis-mark wide-gamut displays that are not P3.
  - With the cannot-show filter active, moving the window between displays changes which rows are listed. R6.1 deselects only on a search or filter change, so items still selected but hidden stay in "Delete selected".
  - Fix:
    - R2.5: the test declares any reported gamut, with those three as fixtures.
    - R6.1: any change in what is listed deselects the items it stops listing.

- **[MINOR] A11. The interim file-wide ceiling sits below what one collection can reach.** Constants table :280, OQ 2 :718.
  - The interim FILE_ITEMS_CEILING (= ROWS_CEILING across the whole file, 10,000; capture :70) is no larger than a single maximal collection.
  - The file-wide views are therefore designed and tested with zero headroom.
  - Fix: set the interim to a multiple of ROWS_CEILING. 100,000 is the scale the browsing research measured.

- **[MINOR] A12. R8.1's write-to-visible budget is never timed at scale.** R8.1 :578, M1 :705, UJ9.5-a journeys.md:338.
  - Only single-reading writes are timed (UJ9.1-a).
  - A bulk set, clear or delete over ROWS_CEILING rows under an active sort and search is untimed. That is the mixed-row burst the ADR-0001 spike flagged as needing a keyed coalescer (docs/briefs/rust-boundary-spike-results.md:41).
  - The budget is feasible at 10,000 rows; this is a verifiability gap.
  - Fix: add a UJ9.5 case timing both bulk operations.

- **[MINOR] A13. Nothing proves a restore keeps its provenance.** R5.5 :530.
  - Data Foundation R2.3f (prd-data-foundation.md:133) enumerates copied samples, decoded values and measurement time. It does not name the device snapshot, the agreement verdict or the spread.
  - The simulated mark (R2.4c), the samples-disagreed mark (R2.4e), Spread (R2.7) and E22 all read these from the current reading.
  - No case restores a simulated or disagreed reading.
  - Fix: add a case restoring ZX-015's B1 (journeys.md:69), which must keep the simulated mark and put E22 up. Name the snapshot, verdict and spread in the Data Foundation obligation.

- **[MINOR] A14. Capture R8.18 needs one source of truth for "unreadable".** capture-mode/prd-capture-mode.md:297.
  - The set-aside standing and Data Foundation's quarantine mark are two statements of one fact.
  - Fix: state that the row's unreadable standing follows Data Foundation's persisted quarantine mark and never disagrees with it, including at SQLITE_READER_FLOOR. Also state what the counts show when damage is found on a read-only file.

- **[NIT] A15. Capture R11.15g points at the wrong row.** capture-mode/prd-capture-mode.md:376.
  - It says Collection Mode's own actions "are listed under its R8.10". R8.10 lists readbacks; the actions are in copy E3/E14 and the journeys' Test-controls map.

## Biggest risks
- **Identity integrity (A1 + A2).** Undo can reintroduce duplicate codes or column names. Separately, the fact that the Swatch Code can change is not handed to ADR-0003 before that schema ratifies inside users' files.
- **The one honesty failure no test sees (A3).** A clipped chip on a wide-gamut display passes M2 at 100%.
- **Mid-session mutation (A4).** A P0 restore route changes Capture's session state underneath a paused session.
- **Promising to read the future (A5).** R8.4 commits older builds to full-fidelity reading of newer files.

## Genuinely sound
- **The seam fork is held open properly.** The Build contract and the "Where capture sits" stop row keep ADR-0004 open. Capture R10.1 ("readings land in the real collection … true under either reading") makes live updates common to both readings, so the collection-side hot path is not itself a lean.
- **No schema choice is smuggled in.** "The name the file stores" and "an outside reader" work equally for per-row name records or real SQL columns. R8.7 defers the macOS floor to ADR-0006 and returns anything it cannot meet to the owner.
- **The two gamut marks are split correctly.** A persisted fact about the reading (Data Foundation R3.4) is separate from a live environment check (R2.5). Not storing cannot-show in the file is right, and a dogmatic review that asks to persist it would be wrong.
- **The delete undo (R1.7) is correctly withheld** behind Data Foundation's open question 20. Pending-delete visibility against R6.2a's "unrecoverable by an outside reader" is a real schema problem, and not guessing at it is the right call.
- **No over-engineering at this scale.** With one writer in one process, R8.3's capture plus editing needs no cross-process conflict handling or merge logic. At 10,000 rows, the spike (p95 0.019 ms write-to-visible) and the research (100,000 rows: 0.49–10.66 ms for full-text search, 9.5 ms for an exhaustive ΔE2000 pass) leave wide headroom. Exhaustive scans are right; asking for full-text or trigram indexes, or a spatial index for Find similar, would be over-engineering.
- **The sibling halves landed together and are fenced on both sides:** E33, the rename mirror (Data Foundation R1.2 and Import R2.6), the Flag (Capture R9.9/R5.6 and Data Foundation R2.9), the unreadable cause (Capture R8.18/M3/M4), the E22 action (Device F32), and export-first (Export F31). Data Foundation F51 also fixes a real pre-existing contradiction between Data Foundation R2.9 and Capture R5.6.
- **R8.10 is neutral on where state lives.** Core-computed order and marks can be asserted headlessly, consistent with ADR-0001's test gates.

## Missing / over-engineered
- **Missing:**
  - A list of the constraints this PRD places on ADR-0003: identity independent of Swatch Code (A2), per-collection view state (A7), no stored cannot-show.
  - The rule for what invalidates the undo stack (A1).
  - A Capture R3.7 fallback for a remembered row deleted while a session is interrupted. Fence F14 now permits that delete (UJ4.4-d).
  - Whether All items search matches imported values. R1.9 hides those columns, while R1.10 says search works "as the collection surface does".
  - R2.10's route in the Row transitions table.
- **Over-engineered:** nothing structural.

| Row ID | disposition |
|---|---|
| R1.1 | ALIGN |
| R1.2 | ALIGN |
| R1.3 | ALIGN |
| R1.4 | ALIGN |
| R1.5 | ALIGN |
| R1.6 | ALIGN |
| R1.7 | ALIGN |
| R1.8 | ALIGN |
| R1.9 | ALIGN |
| R1.10 | OBJECT (A8) |
| R2.1 | ALIGN |
| R2.2 | ALIGN |
| R2.3 | OBJECT (A3) |
| R2.4 | ALIGN |
| R2.4a | ALIGN |
| R2.4b | ALIGN |
| R2.4c | ALIGN |
| R2.4d | ALIGN |
| R2.4e | ALIGN |
| R2.4f | ALIGN |
| R2.4g | ALIGN |
| R2.4h | ALIGN |
| R2.4i | ALIGN |
| R2.5 | OBJECT (A10) |
| R2.6 | ALIGN |
| R2.7 | ALIGN |
| R2.8 | ALIGN |
| R2.9 | ALIGN |
| R2.10 | OBJECT (A7) |
| R3.1 | ALIGN |
| R3.2 | ALIGN |
| R3.3 | OBJECT (A8) |
| R3.4 | OBJECT (A10) |
| R3.5 | ALIGN |
| R3.6 | ALIGN |
| R3.7 | OBJECT (A8) |
| R3.8 | ALIGN |
| R4.1 | ALIGN |
| R4.2 | ALIGN |
| R4.2a | ALIGN |
| R4.2b | ALIGN |
| R4.2c | ALIGN |
| R4.2d | ALIGN |
| R4.2e | ALIGN |
| R4.2f | ALIGN |
| R4.2g | ALIGN |
| R4.2h | ALIGN |
| R4.3 | ALIGN |
| R4.4 | OBJECT (A2) |
| R4.5 | ALIGN |
| R4.6 | OBJECT (A4) |
| R4.7 | OBJECT (A1) |
| R4.8 | ALIGN |
| R4.9 | ALIGN |
| R5.1 | ALIGN |
| R5.2 | ALIGN |
| R5.2a | ALIGN |
| R5.2b | ALIGN |
| R5.2c | ALIGN |
| R5.2d | ALIGN |
| R5.2e | ALIGN |
| R5.3 | ALIGN |
| R5.4 | ALIGN |
| R5.5 | OBJECT (A4, A13) |
| R5.6 | ALIGN |
| R5.7 | ALIGN |
| R5.8 | ALIGN |
| R6.1 | OBJECT (A10) |
| R6.2 | ALIGN |
| R6.3 | ALIGN |
| R7.1 | ALIGN |
| R7.2 | ALIGN |
| R8.1 | OBJECT (A6, A12) |
| R8.2 | OBJECT (A11) |
| R8.3 | OBJECT (A4) |
| R8.4 | OBJECT (A5) |
| R8.5 | ALIGN |
| R8.6 | ALIGN |
| R8.7 | ALIGN |
| R8.8 | ALIGN |
| R8.9 | ALIGN |
| R8.10 | OBJECT (A3, A9) |
| E1 | ALIGN |
| E2 | ALIGN |
| E3 | ALIGN |
| E4 | ALIGN |
| E5 | ALIGN |
| E6 | ALIGN |
| E7 | ALIGN |
| E8 | ALIGN |
| E9 | ALIGN |
| E10 | ALIGN |
| E11 | ALIGN |
| E12 | ALIGN |
| E13 | ALIGN |
| E14 | ALIGN |
| E15 | ALIGN |
| E16 | ALIGN |
| E17 | ALIGN |
| M1 | ALIGN |
| M2 | ALIGN |
| M3 | ALIGN |

#### peer-performance-reviewer (Claude route)

## Verdict
Meets budget after fixing Blockers. At the stated scale, what the rows ask for is cheap: I measured it here and it matches the research. But R8.1 and M1 leave the interaction that fails first without a budget: selecting a row or updating the data in the SwiftUI table at about 1–5k rows. The timing conditions and workload are also not declared, so two runs of M1 won't give the same number. The four Majors need to land with the Blocker fix.

## Workload & budget (brief)
- **Paths.** Root is `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode`. Short names used below:
  - `prd` = docs/product/collection-mode/prd-collection-mode.md
  - `jny` = …/prd-collection-mode-journeys.md
  - `capture` = docs/product/capture-mode/prd-capture-mode.md
  - `df` = docs/product/data-foundation/prd-data-foundation.md
  - `import` = docs/product/import/prd-inventory-import.md
  - `v2` = docs/briefs/browsing-a-collection-at-scale-research-results-v2.md
- **Scale.**
  - Each collection has a working range of 200–1,200 items (ROWS_TARGET) and a ceiling of 10,000 (ROWS_CEILING), both from capture:70. Import still allows a collection past the ceiling (import:76).
  - FILE_ITEMS_CEILING has an interim equal to ROWS_CEILING across the whole file.
  - The DF R7.7 corpus at the ceiling is about 1.2 GB raw (df:328).
- **Budgets.** BROWSE_RESPONSE_BUDGET is 100 ms at p95; OPEN_COLLECTION_BUDGET is 1 s. Both are F20 candidates.
- **Hot paths.** The table's search, filter and sort; selection; a reading saved during capture showing up in the table; the All items view; Find similar.
- **What I measured.** Apple M5 Max, one warm run, Swift `-O`, SQLite 3.54.0. This is the fastest-class machine, so a floor Mac will be slower (I estimate 2–3×, not measured).
  - Locale-aware natural sort: 10k ≈ 24 ms; 100k ≈ 300–315 ms.
  - R2.3-style normalising of 5 fields per item, on each keystroke: 10k ≈ 18.5 ms.
  - Swift contains-scan over already-normalised text: 10k ≈ 17 ms; 100k ≈ 180 ms.
  - SQLite with WAL, `synchronous=FULL`, `fullfsync=ON`, `secure_delete=ON`: single-row commit 0.5–1.5 ms; 10k-row UPDATE 6.7–8.4 ms.

## Findings
**[BLOCKER] PERF-1 — Selection, scrolling and opening an item have no budget**
- **Where:** prd:578 (R8.1), prd:705 (M1), jny:338 (UJ9.5-a).
- **The gap:** R8.1 budgets keystroke, filter change, view sort and saved change. M1 and UJ9.5-a time only the first three. None of these carries a budget at any scale:
  - selecting a row
  - extending a selection by range
  - "Select all" over every listed item (R6.1, prd:543)
  - scrolling
  - opening and closing the item detail with scroll and selection restored (R4.1, prd:479)
- **Why it matters:** the project's own research ranks "hang on selection, not scroll" as the #1 macOS symptom (v2:183). It puts SwiftUI `Table`'s cliff at 1,000–1,219 rows: 13+ s to select, later about 3.5 s (v2:136, :181). It also reports a hang "at any size" on macOS 15.5/26 (v2:141). That is the top of ROWS_TARGET, a tenth of ROWS_CEILING.
- **Mutation that passes every stated case:** build R2.1's table as a SwiftUI `Table` whose rows are `AnyView`-erased (the documented cause of the 1,219-row hang, v2:171).
  - UJ9.5-a still passes, because it times only search, filter and header.
  - UJ6.1-a–c (jny:281–283) and UJ4.1-b (jny:218) still pass on 13 rows.
  - Every click at 1,200 rows then stalls for seconds.
- **Fix:**
  - Add select, range-extend, Select all, and open/close detail to R8.1's inputs and to M1's population, at BROWSE_RESPONSE_BUDGET p95.
  - Add a scroll statistic as a new owner constant under OQ 1 — for example, the share of frames that miss their deadline while paging through ROWS_CEILING rows.
  - UJ9.5-a delivers 200 of each.

**[MAJOR] PERF-2 — The saved-change budget is never measured at scale, and its scope is undefined**
- **Where:** prd:578 (R8.1, "a reading or edit saved anywhere"), jny:331 (UJ9.1-a).
- **Not measured:** this is the research's #2 symptom — a hang on data update after a fine first load, seen with `Table` at 5,000 rows (v2:139, :183). R8.1 budgets it at ROWS_CEILING. The only case is UJ9.1-a on the 13-item file; M1 and UJ9.5-a leave it out.
- **Start event unstated:** does the clock start at the user's Return or at the commit? R8.8 puts the durable save inside the Return-based reading.
- **"Edit" undefined:** a builder can't tell whether these have to show within 100 ms:
  - a bulk set or clear over up to ROWS_CEILING selected items (R6.2)
  - "Use as scan order" rewriting up to ROWS_CEILING positions (R2.9)
  - Undo of a bulk change (R4.7)
  - a restore (R5.5)
  - a column rename (R4.8)
- **Where the cost really is:** the SQL is not the risk; a 10k-row UPDATE measured 7–8 ms. The risk is the refresh that follows at 10k: re-query, re-sort and the container's diff.
- **Fix:**
  - Define the start event as the commit landing.
  - List which writes the budget covers. Give bulk writes their own "shown done within X, surface stays responsive" rule.
  - Add to UJ9.5-a: the Demo Device saves 200 sets into Scale while it is searched, filtered and view-sorted, at p95; plus one bulk clear across ROWS_CEILING items.

**[MAJOR] PERF-3 — The timing environment is not declared**
- **Where:** prd:578 ("a declared machine"), prd:705 ("each machine OQ 1 names"), prd:717 (OQ 1's interim).
- **Gaps:**
  - **Machine:** no interim machine is named, so any machine can be declared — including this M5 Max.
  - **Volume class:** R8.8 makes an edit wait for its durable save. Capture R4.13 (capture:189) says save cost differs by volume class and measures internal, USB and network volumes separately. DF R1.7 (df:104) allows network and sync volumes. A flat 100 ms can't be met on a network volume by construction.
  - **Cache state:** the corpus is about 1.2 GB raw (df:328). UJ9.5-a's "every choice" is decided by the cold choice, but cold versus warm isn't declared. Nor is whether DF's open-file check has finished.
  - **End event:** R8.10's model-level listing, or the first frame on screen?
  - **Cadence, warm-up and estimator:** per PR on shared CI runners, or per release (DF times its check every release, df:245)? Are warm-up samples discarded? Is p95 by nearest rank?
- **Fix:**
  - The owner names an interim floor machine.
  - Measure R8.1 on the internal volume; USB and network are either excluded or budgeted as the budget plus the measured save.
  - The first OPEN sample is cold, browse samples are warm.
  - End event = the first displayed frame containing the result.
  - M1 is read every release on named hardware; the per-PR run is a tripwire only.

**[MAJOR] PERF-4 — M1's workload isn't declared, and the imported-columns dimension has no bound**
- **Where:** prd:705 (M1), jny:338 (UJ9.5-a), prd:461 (R3.1), prd:425 (R2.1).
- **Why the p95 can't be reproduced**, against the template's own "same number" rule (prd:698):
  - **Queries:** which ones? A prefix that lists all 10k rows costs very differently from a no-match full scan.
  - **Pacing:** coalescing leaves the results of intermediate keystrokes undefined.
  - **Filters:** a row-state filter or the live cannot-show mark?
  - **Headers:** L* under 1 ms, versus natural-order locale-aware text per Capture R6.9 at about 24 ms per 10k on the fastest Mac.
- **The unbounded second axis:** R3.1 searches every imported value and R2.1 shows every imported column, but nothing bounds how many imported columns there are or how long their values get. Import sets no limit. DF R7.7's generated corpus is sized for file size (M6/M8), not for text. At 10k items, 5 fields per item already takes about 17–18 ms per keystroke on this machine; 20 imported columns would take most of the 100 ms.
- **Fix — declare in M1 and UJ9.5-a:**
  - the corpus text: K imported columns and a value-length distribution, with a named COLUMNS candidate for the owner
  - the query list: all-match prefix, selective prefix, contains-in-imported-value, no-match
  - the keystroke cadence
  - filters round-robin over row states and every R2.4 mark
  - headers round-robin over every column, in both directions

**[MAJOR] PERF-5 — FILE_ITEMS_CEILING's interim is too low, and its closer can't close it**
- **Where:** prd:280, prd:718 (OQ 2).
- **The interim is exceeded on day one:** "ROWS_CEILING across the whole file" is exceeded by any legal file with one ceiling collection plus another. Import lets a single collection exceed ROWS_CEILING outright (import:76).
- **The closer can't close it:** it rests on Capture OQ 13, which sizes one collection — OQ 2 itself says so.
- **Why it matters:** the All items view is the first surface to leave budget as files grow. At 10^5, the research's "architectural ceiling" (v2:279), text sort took about 300 ms and a contains-scan about 180 ms on this machine.
- **Fix:** set the interim to 100,000 (the research's measured corpus), or ROWS_CEILING × a stated number of collections per file. The closer becomes the owner's estimate of collections per file, checked by UJ9.5-b.

**[MINOR] PERF-6 — R8.2 and UJ9.5-b are underspecified**
- **Where:** prd:579 (R8.2), jny:339 (UJ9.5-b).
- **Gaps:**
  - R8.2 doesn't list R8.1's inputs.
  - UJ9.5-b times only keystrokes and Find similar.
  - Opening the All items view has no budget.
  - Undeclared: how items split across collections, their chosen conditions (which drive the "not compared" count), and whether Find similar is fired from the All items view or from one collection.
- **Fix:** name the inputs and an open budget in R8.2, and declare the distribution and scope in UJ9.5-b.

**[MINOR] PERF-7 — A p95 budget used as a single-shot deadline**
- **Where:** prd:278 defines the budget as p95. R3.1 (prd:461) and R2.5 (prd:446) state it as a per-event deadline. UJ2.1-d (jny:166) and UJ9.1-a (jny:331) assert one observation.
- **Why it matters:** a single sample can't pass or fail a p95, so these cases either flake or can't fail. R2.5 also names no scale.
- **Fix:** the rows cite p95. Single-shot cases use a functional timeout and leave timing to M1. Add a display-move timing at ROWS_CEILING, with a cannot-show filter active, to UJ9.5-a.

**[MINOR] PERF-8 — Nothing is stated above ROWS_CEILING**
- **Where:** prd:578, with Import R3.1 (import:76) allowing it.
- **Why it matters:** above the ceiling there is no rule at all, at a size import accepts.
- **Fix:** "Above ROWS_CEILING every row works, nothing is refused or truncated, and the budgets are not promised." Add a function-only smoke case at 2× ROWS_CEILING.

**[MINOR] PERF-9 — "First rows" in OPEN_COLLECTION_BUDGET is undefined**
- **Where:** prd:578.
- **Gaps:** how many rows, whether chips and marks are included, and when the full set becomes searchable. A lazy load passes the 1 s budget while a search typed at 1.5 s waits on the rest.
- **Fix:** first rows = the visible rows with their chips and marks. R8.1's budgets hold from that moment.

**[MINOR] PERF-10 — No precedence for capture over this PRD's writes and refreshes**
- **Where:** prd:580 (R8.3), against Capture R4.8 and R4.13 (capture:182, :189).
- **The overlap:** F14 keeps bulk set/clear over up to ROWS_CEILING items, rename and undo available during a session, and R8.1 refreshes the table on every saved reading. Both share one single-writer file with capture — and, because the seam is open (ADR-0004), possibly one main thread with TRIGGER_ACK_WINDOW (100 ms) and ROW_CONFIRM_BUDGET.
- **What's missing:** no row gives capture precedence or bounds how long this PRD holds the file or the thread.
- **Where it bites:** the SQL is small on internal SSD, so it bites on USB and network volumes, during search-index upkeep, and in a main-thread refresh at ROWS_CEILING.
- **Fix, without reopening F14:** add a row that no Collection Mode write or refresh delays an in-flight session's trigger acknowledgement or row confirmation beyond Capture's budgets. Add a case: the Demo Device at real pacing during a bulk clear over ROWS_CEILING items, reading Capture M1.

**[MINOR] PERF-11 — The grid has no budget**
- **Where:** prd:554–555 (R7.1, R7.2).
- **Gaps:** R8.1 covers only "the table". Unbudgeted for the grid: search and sort, the Table↔Grid switch that keeps the item in view, and a swatch-size change re-laying out up to ROWS_CEILING swatches. The research names grid frame drops as a symptom (v2:183).
- **Fix:** extend R8.1's inputs to the grid, and add switch and resize to UJ9.5-a in R7's phase.

**[MINOR] PERF-12 — R8.2 missing from OQ 1's closure gate**
- **Where:** prd:278 lists R8.2 as an owner of BROWSE_RESPONSE_BUDGET, but OQ 1's Feeds (prd:717) and the Interim-stated line (prd:327) leave it out.
- **Why it matters:** closing OQ 1 would not re-check R8.2.
- **Fix:** add R8.2 to both.

**[MINOR] PERF-13 — Two budgets for one search rule (sibling seam)**
- **Where:** Capture R3.12 and R6.1, FIND_BUDGET, still TBD (capture:71, :167, :228).
- **The mismatch:** under F17, this PRD's search is Capture's Find rule plus imported values, over the same collection. The two budgets close independently, so a looser FIND_BUDGET would give the narrower search a slower budget than the broader one.
- **Fix:** state FIND_BUDGET ≤ BROWSE_RESPONSE_BUDGET in OQ 1 and in Capture OQ 13's closer.

**[NIT] PERF-14 — In-memory undo grows while the file is open**
- **Where:** prd:501 (R4.7).
- **Why it's fine:** F13 settles that every change stays undoable while the file is open, so earlier values build up in memory — kilobytes to low megabytes at real edit rates.
- **Suggestion:** add one case that undoes a bulk clear over ROWS_CEILING items.

## Biggest risks   (what degrades first as data/traffic grows)
1. **Selection and data-update hangs in the table container**, at roughly 1–5k rows — inside ROWS_TARGET (PERF-1, PERF-2).
2. **A main-thread table refresh on every saved reading at ROWS_CEILING**, running alongside capture's 100 ms trigger acknowledgement (PERF-10).
3. **File-wide text sort and search** once a file approaches 10^5 items (PERF-5).
4. **Imported-column width.** Search cost grows linearly with the total imported text (PERF-4).
5. **A search index against the no-prior-text rule** (DF F17, UJ4.2-c).
   - An FTS5 external-content index keeps deleted tokens around until its segments merge.
   - Honouring UJ4.2-c needs FTS5 secure-delete (SQLite 3.42 or later, which raises SQLITE_READER_FLOOR) or a contentless-delete design, plus zeroing freed pages — extra writes on every edit.
   - UJ4.2-c will catch a naive index, which is good.
6. **Undoing the delete of a ceiling collection** (R1.7, gated by DF OQ 20) — about 1.2 GB raw has to be held somewhere.

## Genuinely efficient   (incl. where simple-and-fast-enough is right that a perf-zealot would wrongly flag)
- **Find similar with no index is the right call.** Exhaustive ΔE2000 measured 1.9 ms at 10k and 9.5 ms at 100k (v2:263). An R-tree prefilter can miss matches under ΔE2000 (v2:277), and the PRD rightly mandates no index.
- **L*/C*/h°, the marks, and cannot-show read stored values.** They come from the working set; cannot-show is a cheap per-item gamut test, even when the window moves displays.
- **R3.1 times from the last keystroke.** That rules out a debounce longer than the budget — precise.
- **R4.3 saves on Return or leaving the field**, not per keystroke. A durable commit measured 0.5–1.5 ms.
- **R3.6 and R8.6 keep search, filter and sort out of the file and off the network**, so the browse hot path does no writes.
- **History "however many, never aged out" needs no pagination or budget.** DF's M6 corpus has about 10 readings per item.
- **The collection-list counts, the "Answer re-scans" check and the E22 banner check** are each one aggregate query.
- **OQ 1 cites the research faithfully** (v2:5, :221–223, :263) and flags that this app is unmeasured.
- **The sibling halves add no new performance exposure.** This covers Capture R5.6/R8.18/R9.9/M3/M4, DF R1.2/R2.9/R6.2/R7.6o/E33, Import R2.6, Export F31 and Device F32. E33's counts are one aggregate query, and a selection delete can't be bigger than the collection delete R1.4 already allows.

## Missing / over-engineered   (premature optimization)
- **Missing:**
  - a budget for selection, scrolling and opening an item (PERF-1)
  - a saved-change case at scale (PERF-2)
  - an interim machine and a volume class (PERF-3)
  - a declared workload and a bound on imported columns (PERF-4)
  - file-wide ceiling evidence (PERF-5)
  - behaviour above the ceiling (PERF-8)
  - capture precedence (PERF-10)
- **Not over-engineered:** no index, cache or pagination is mandated; 200 samples per kind is proportionate; and 100 ms / 1 s (F20) fits a local app.
- **One caution:** don't gate every PR on a 100 ms p95 on shared CI runners.

| Row ID | disposition |
|---|---|
| R1.1 | ALIGN |
| R1.2 | ABSTAIN |
| R1.3 | ABSTAIN |
| R1.4 | ABSTAIN |
| R1.5 | ABSTAIN |
| R1.6 | ABSTAIN |
| R1.7 | ABSTAIN |
| R1.8 | ABSTAIN |
| R1.9 | ALIGN |
| R1.10 | ALIGN |
| R2.1 | ALIGN |
| R2.2 | ABSTAIN |
| R2.3 | ALIGN |
| R2.4 | ALIGN |
| R2.4a | ALIGN |
| R2.4b | ABSTAIN |
| R2.4c | ABSTAIN |
| R2.4d | ABSTAIN |
| R2.4e | ABSTAIN |
| R2.4f | ABSTAIN |
| R2.4g | ABSTAIN |
| R2.4h | ABSTAIN |
| R2.4i | ABSTAIN |
| R2.5 | OBJECT (PERF-7) |
| R2.6 | ALIGN |
| R2.7 | ABSTAIN |
| R2.8 | ABSTAIN |
| R2.9 | ALIGN |
| R2.10 | ABSTAIN |
| R3.1 | OBJECT (PERF-4, PERF-7, PERF-13) |
| R3.2 | ALIGN |
| R3.3 | ALIGN |
| R3.4 | ALIGN |
| R3.5 | ABSTAIN |
| R3.6 | ALIGN |
| R3.7 | ALIGN |
| R3.8 | ABSTAIN |
| R4.1 | ALIGN |
| R4.2 | ABSTAIN |
| R4.2a | ABSTAIN |
| R4.2b | ABSTAIN |
| R4.2c | ABSTAIN |
| R4.2d | ABSTAIN |
| R4.2e | ABSTAIN |
| R4.2f | ABSTAIN |
| R4.2g | ABSTAIN |
| R4.2h | ABSTAIN |
| R4.3 | ALIGN |
| R4.4 | ABSTAIN |
| R4.5 | ABSTAIN |
| R4.6 | ABSTAIN |
| R4.7 | ALIGN |
| R4.8 | ABSTAIN |
| R4.9 | ABSTAIN |
| R5.1 | ALIGN |
| R5.2 | ABSTAIN |
| R5.2a | ABSTAIN |
| R5.2b | ABSTAIN |
| R5.2c | ABSTAIN |
| R5.2d | ABSTAIN |
| R5.2e | ABSTAIN |
| R5.3 | ABSTAIN |
| R5.4 | ALIGN |
| R5.5 | ABSTAIN |
| R5.6 | ABSTAIN |
| R5.7 | ALIGN |
| R5.8 | ABSTAIN |
| R6.1 | OBJECT (PERF-1) |
| R6.2 | OBJECT (PERF-2, PERF-10) |
| R6.3 | ABSTAIN |
| R7.1 | OBJECT (PERF-11) |
| R7.2 | OBJECT (PERF-11) |
| R8.1 | OBJECT (PERF-1, PERF-2, PERF-3, PERF-4, PERF-8, PERF-9) |
| R8.2 | OBJECT (PERF-5, PERF-6, PERF-12) |
| R8.3 | OBJECT (PERF-10) |
| R8.4 | ALIGN |
| R8.5 | ALIGN |
| R8.6 | ABSTAIN |
| R8.7 | ABSTAIN |
| R8.8 | ALIGN |
| R8.9 | ABSTAIN |
| R8.10 | ALIGN |
| E1 | ABSTAIN |
| E2 | ABSTAIN |
| E3 | ABSTAIN |
| E4 | ABSTAIN |
| E5 | ABSTAIN |
| E6 | ABSTAIN |
| E7 | ABSTAIN |
| E8 | ABSTAIN |
| E9 | ABSTAIN |
| E10 | ABSTAIN |
| E11 | ABSTAIN |
| E12 | ABSTAIN |
| E13 | ABSTAIN |
| E14 | ABSTAIN |
| E15 | ABSTAIN |
| E16 | ABSTAIN |
| E17 | ABSTAIN |
| M1 | OBJECT (PERF-1, PERF-3, PERF-4) |
| M2 | ABSTAIN |
| M3 | ABSTAIN |

#### peer-staff-software-engineer-reviewer (cross-model, agy)

## Verdict
Not ready — needs rework — One Blocker and two ambiguities in bulk operations and search tracing require clarification before engineering begins.

## What I reviewed
- docs/product/collection-mode/prd-collection-mode.md and its companions (journeys, copy, oq-results).
- The upstream it must satisfy: N/A (this is the PRD).
- The codebase/system I read: The capture mode and data foundation PRDs in the same workspace.
- What I could NOT verify: N/A.

## Findings
BLOCKER R6.2 — Bulk editing Swatch Code creating duplicates — The requirement "sets one editable field" does not explicitly exclude Swatch Code, but bulk-setting Swatch Code to a single value would violate R4.4's uniqueness requirement and fail. Mutation: in a test, select two items, bulk-set their Swatch Code to "duplicate", and assert that the file successfully saves both with the same Swatch Code (violating R4.4 uniqueness) or throws an unhandled constraint error; cite `docs/product/collection-mode/prd-collection-mode.md:544` — Explicitly exclude Swatch Code from the fields that can be set in a bulk operation.

MINOR R3.1 — All items view search omitting Collection name — In the All items view, a user typing a collection name will not find its items because search only checks Swatch fields and imported values, not the Collection name; cite `docs/product/collection-mode/prd-collection-mode.md:461` — Add the Collection name to the list of fields searched in the All items view, or explicitly declare it excluded.

NIT R3.7 — Find similar tie-breaking rule unspecified — "listing nearest first" does not specify the tie-breaker for items with the exact same ΔE2000 distance; cite `docs/product/collection-mode/prd-collection-mode.md:467` — State the tie-breaker (e.g. queue order or Swatch Code).

## Clarifying questions for the author
1. In R6.2, should Swatch Code be explicitly excluded from bulk edits to prevent creating duplicate identifiers?
2. In R3.1, when searching in the "All items" view, should the search text also match against the Collection name?
3. In R3.7, if two items have the exact same ΔE2000 distance in the "Find similar" list, what is their secondary sort order?

## Claimed properties
- "Rename column changes the name the file stores" — Holds. Checked against OQ 8 and the Data Foundation/Inventory Import PRD amendments.
- "A captured row whose current reading is quarantined is set aside" — Holds. Capture Mode F70 and R8.18 fully mirror the unreadable cause and restoration paths.
- "Use this reading on a flagged reading restores it" — Holds. Capture Mode R5.6 explicitly allows Collection Mode R5.5 to restore flagged readings, making them captured again.

## Genuinely sound
- R1.3 explicitly handles the edge case of renaming a collection to its own name differing only in case or spacing, passing the uniqueness check correctly.
- R8.4 provides a clean, blanket rule for read-only mode by disabling any action that would write the file, rather than enumerating them one by one.
- R2.9 explicitly bounds reordering in a view-sorted table to the Capture PRD's rules, avoiding conflicts with the display order.

## Deferred
- `peer-product-manager-reviewer`: Whether excluding the Collection name from "All items" search genuinely hurts the user flow.
- `peer-architecture-reviewer`: Assessing if computing ΔE2000 for `FILE_ITEMS_CEILING` rows within 100ms scales structurally on the target mobile/desktop database engine.

| Row ID | disposition |
|---|---|
| R1.1 | ALIGN |
| R1.2 | ALIGN |
| R1.3 | ALIGN |
| R1.4 | ALIGN |
| R1.5 | ALIGN |
| R1.6 | ALIGN |
| R1.7 | ALIGN |
| R1.8 | ALIGN |
| R1.9 | ALIGN |
| R1.10 | ALIGN |
| R2.1 | ALIGN |
| R2.2 | ALIGN |
| R2.3 | ALIGN |
| R2.4 | ALIGN |
| R2.4a | ALIGN |
| R2.4b | ALIGN |
| R2.4c | ALIGN |
| R2.4d | ALIGN |
| R2.4e | ALIGN |
| R2.4f | ALIGN |
| R2.4g | ALIGN |
| R2.4h | ALIGN |
| R2.4i | ALIGN |
| R2.5 | ALIGN |
| R2.6 | ALIGN |
| R2.7 | ALIGN |
| R2.8 | ALIGN |
| R2.9 | ALIGN |
| R2.10 | ALIGN |
| R3.1 | OBJECT (Finding-1) |
| R3.2 | ALIGN |
| R3.3 | ALIGN |
| R3.4 | ALIGN |
| R3.5 | ALIGN |
| R3.6 | ALIGN |
| R3.7 | OBJECT (Finding-3) |
| R3.8 | ALIGN |
| R4.1 | ALIGN |
| R4.2 | ALIGN |
| R4.2a | ALIGN |
| R4.2b | ALIGN |
| R4.2c | ALIGN |
| R4.2d | ALIGN |
| R4.2e | ALIGN |
| R4.2f | ALIGN |
| R4.2g | ALIGN |
| R4.2h | ALIGN |
| R4.3 | ALIGN |
| R4.4 | ALIGN |
| R4.5 | ALIGN |
| R4.6 | ALIGN |
| R4.7 | ALIGN |
| R4.8 | ALIGN |
| R4.9 | ALIGN |
| R5.1 | ALIGN |
| R5.2 | ALIGN |
| R5.2a | ALIGN |
| R5.2b | ALIGN |
| R5.2c | ALIGN |
| R5.2d | ALIGN |
| R5.2e | ALIGN |
| R5.3 | ALIGN |
| R5.4 | ALIGN |
| R5.5 | ALIGN |
| R5.6 | ALIGN |
| R5.7 | ALIGN |
| R5.8 | ALIGN |
| R6.1 | ALIGN |
| R6.2 | OBJECT (Finding-2) |
| R6.3 | ALIGN |
| R7.1 | ALIGN |
| R7.2 | ALIGN |
| R8.1 | ALIGN |
| R8.2 | ALIGN |
| R8.3 | ALIGN |
| R8.4 | ALIGN |
| R8.5 | ALIGN |
| R8.6 | ALIGN |
| R8.7 | ALIGN |
| R8.8 | ALIGN |
| R8.9 | ALIGN |
| R8.10 | ALIGN |
| E1 | ALIGN |
| E2 | ALIGN |
| E3 | ALIGN |
| E4 | ALIGN |
| E5 | ALIGN |
| E6 | ALIGN |
| E7 | ALIGN |
| E8 | ALIGN |
| E9 | ALIGN |
| E10 | ALIGN |
| E11 | ALIGN |
| E12 | ALIGN |
| E13 | ALIGN |
| E14 | ALIGN |
| E15 | ALIGN |
| E16 | ALIGN |
| E17 | ALIGN |
| M1 | ALIGN |
| M2 | ALIGN |
| M3 | ALIGN |


### Round 1 — verify-the-reviewer dispositions (Blocker/Major; Critical→Blocker, High→Major)

Every Blocker and Major was checked against commit `9acd883` before being accepted. Findings raised by more than one lens are merged into one row and name every lens. The privacy lens raised no Critical or High; its three Medium findings (PRIV-1 byte-level residue, PRIV-2 undo stack kept out of the file, PRIV-3 no-network check with the network reachable) are advisory, noted here and carried into the round-1 fix list where the owner accepts them.

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| 1 | E12's "Outside sRGB" says sRGB is "the reference your exports and your file use"; the file and exports carry six spaces and spectra, and only the sRGB/HSL derivation is clipped (DF R3.4, Export R2.1) | product-marketing-manager (Blocker); product-manager (Minor) | reproduced — copy line 210 as quoted | accept — owner decision on wording |
| 2 | No budget for selecting, range-extending, Select all, scrolling, or opening/closing the item detail; the research ranks a selection hang as the #1 macOS symptom at ~1,000 rows (v2:136, :183) | performance (Blocker); staff-software-engineer MJ8 | reproduced — R8.1, M1 and UJ9.5-a time only search, filter and header | accept |
| 3 | What the chip renders is guessable (stored clipped sRGB vs working-set colour managed to the display) and no readback observes it, so a clipped chip passes M2 at 100% | test (Blocker B1); architecture (A3); staff-software-engineer (MJ1) | reproduced — R2.3 wording, R8.10's readback list | accept — owner decision on the rule |
| 4 | Fixture ZX-013 (L*60 C*40 h°190, D50/2°) is declared inside sRGB but is outside it; a correct cannot-show test fails the P0 gate | test (Blocker B2); staff-software-engineer (Blocker B1) | reproduced — orchestrator recomputation: linear sRGB R = −0.009 (Bradford) | accept |
| 5 | Bulk "Set a field" could set Swatch Code to one value and create duplicates | peer-staff-software-engineer-reviewer on agy (Blocker) | not reproduced — the Vocabulary defines "editable field" as Swatch Name, both alternates and imported columns, "Swatch Code is changed only through R4.4" (prd line 188) | reject — not reproduced; not fixed |
| 6 | Metadata undo: no copy label (collides with E10's "Undo"), no rule for interleaved non-metadata actions, can recreate duplicate codes/columns, can bypass R8.3, stale targets, no redo rule; its no-residue promise is untested | product-manager; interface (IF-2); architecture (A1); staff-software-engineer (MJ4); test (MJ2); privacy PRIV-2 (Medium) | reproduced — R4.7 text; only E10 carries "Undo"; UJ4.6-f + undo yields two "Family" columns | accept — owner decision on the undo model |
| 7 | Deleting from the All items view: the DF PRD's E8 names no collection, so two same-code items are indistinguishable at a final delete | product-manager | reproduced — DF copy E8 headline "Delete ⟨code⟩?" | accept — owner decision (sibling amendment) |
| 8 | "Done" on E9/E12 collides with the capture PRD's Labels rule reserving "Done" on the collection surface for its summary | interface (IF-1) | reproduced — capture PRD line 402 | accept — owner decision on the label |
| 9 | Rename/Delete collection sit on the collection surface in Surfaces/E3 but on the collection list in the test-controls map and UJ1 asserts | interface (IF-3) | reproduced — journeys line 370 vs copy line 123 | accept — correct the map and cases to the collection surface |
| 10 | Mark chip labels, filter labels and accessible names have no copy home; UJ9.6-a matches wording | interface (IF-4); test m10 | reproduced — no label table in the copy file | accept |
| 11 | R8.10's readback grants neither offered actions, selection, search text, token values, E9's list, All items rows nor grid state that ~40 asserts read; capture R11.15g points at R8.10 for actions it does not list | interface (IF-5); test (MJ1); staff-software-engineer MN13 | reproduced — R8.10 text | accept |
| 12 | Restores (DF E4's and "Use this reading"), E10's Undo and correction answers during an in-flight session are ungated and move rows under a live session | interface (IF-6); architecture (A4); staff-software-engineer (MJ6); test (MJ4) | reproduced — R8.3's refused list | accept — owner decision |
| 13 | R4.4 requires item identity independent of Swatch Code in the file, with no DF or ADR-0003 counterpart | architecture (A2) | reproduced — no Outbound line for R4.4; ADR queue entry silent | accept — owner decision (sibling obligation) |
| 14 | R8.4 promises every mark on a read-only newer-format file, wider than DF R5.3's floor and pre-empting DF OQ 18 | architecture (A5) | reproduced — DF lines 173, 337 | accept |
| 15 | "Value missing" and "No current value" are near-synonyms in legend, filters and VoiceOver | product-marketing-manager | reproduced — copy line 210 | accept — owner decision on labels |
| 16 | "Re-scan to answer" reads as an instruction to re-scan | product-marketing-manager | reproduced — copy line 210 | accept — owner decision on label |
| 17 | "settled/unsettled" means both the capture PRD's row decision and this PRD's reading mark | product-marketing-manager; staff-software-engineer (MJ2); test m12 | reproduced — Vocabulary imports capture's "settled" and defines "unsettled" as a reading mark | accept — owner decision on the name |
| 18 | The marks legend shows no shapes and is unreachable from the item detail and version history | product-marketing-manager | reproduced — R2.8 | accept |
| 19 | The collection-side "Flag" empties the chip and sets the item aside with no word of its consequence | product-marketing-manager; product-manager (Minor) | reproduced — R4.9, E14 | accept — owner decision |
| 20 | "Rename column" gives no warning that a later import will add the old header as a new column | product-marketing-manager; product-manager (Minor) | reproduced — R4.8, E11; E16's code warning has no column counterpart | accept — owner decision |
| 21 | The saved-change budget's scope and start event are undefined and never timed at scale; bulk writes have no bound | performance (PERF-2); architecture A12; staff-software-engineer MJ8 | reproduced — R8.1, UJ9.1-a on 13 items | accept |
| 22 | The timing environment is undeclared: no interim machine, end event, volume class, cold/warm, cadence | performance (PERF-3); staff-software-engineer (MJ7) | reproduced — R8.1 "a declared machine", OQ 1 interim | accept — owner names the machine |
| 23 | M1's workload is undeclared and imported-column width is unbounded | performance (PERF-4); staff-software-engineer MJ8 | reproduced — no such constant | accept |
| 24 | FILE_ITEMS_CEILING's interim (= ROWS_CEILING) is exceeded by any file with one full collection plus one item | performance (PERF-5); architecture A11; staff-software-engineer MJ8 | reproduced — constants table | accept — owner decision on the interim |
| 25 | The Build dependencies "stop" rule contradicts cells that say interims hold; ADR-0003 stops are partial | staff-software-engineer (MJ3) | reproduced — Legend rows 1 and 3 | accept |
| 26 | A selected item unlisted by a data change stays selected | staff-software-engineer (MJ5); architecture A10 | reproduced — R6.1 deselects only on search/filter change | accept |
| 27 | The in-flight guard is tested in two of its four sub-states | test (MJ3) | reproduced — no case names a guard-paused or halted session | accept |
| 28 | Cross-cutting envelope cases (read-only, network, keyboard) run only in the first phase, so P1/P2 actions escape them | test (MJ5) | reproduced — UJ9 preamble | accept |
| 29 | "Export collection" is never fired by a case | test (MJ6) | reproduced — only listed, never fired | accept |
| 30 | UJ9.4-b's "zx-01 nowhere in the file" fails a correct build that stores normalised codes | test (MJ7) | reproduced — UJ9.4-b | accept |
| 31 | Find similar's like-with-like exclusion has no isolating case; Blues Two's conditions are undeclared | test (MJ8) | reproduced — UJ3.4-d Given | accept |

**Per-row dispositions (flip rule: a row is flip-eligible only when every non-abstaining lens ALIGNs and at least one opined).** Flip-eligible after round 1 (25): E1, E2, R1.1, R1.2, R1.5, R1.6, R2.4c, R2.4d, R2.4e, R2.4f, R2.4g, R2.4h, R2.6, R4.2, R4.2a, R4.2f, R4.2g, R4.2h, R5.1, R5.2, R5.2a, R5.2b, R5.2c, R5.6, R8.7. Each flips to aligned in the round-1 fix pass unless that pass changes its meaning, in which case it is re-reviewed in round 2. Every other row (77) carries at least one OBJECT and moves to needs-discussion.

**Flip-list correction (2026-09-25, editorial).** The PRD's Traceability makes a lettered sub-row carry its lead row's status, so a sub-row cannot flip apart from its lead. Of the 25 IDs above, the lead rows R2.4 (objected), R4.2 and R5.2 (sub-rows rewritten in the round-1 fix pass) hold their sub-rows; the standalone rows that flip are E1, E2, R1.1, R1.2, R1.5, R1.6, R2.6, R5.1, R5.6 and R8.7, each unless the round-1 fix pass changes its meaning.

**Round-1 adjudication.** The owner decided F29–F35 and approved recommendations 1–22 (F36–F57) on 2026-09-24, and recommendations 1–33 over the remaining findings (F58–F90) on 2026-09-25. The fix list is `docs/product/collection-mode/prd-collection-mode-round-1-fixes.md` — one box per finding from all nine reviews (158 rated findings plus 11 unrated items from the reviewers' Missing lists), verified item-for-item against each review's count before dispatch.

## Round 2 (2026-09-25) — delta verification

**Lenses:** the eight Claude-route lenses of round 1, re-dispatched in parallel with the round-1 log and fix file as sources, each asked per round-1 finding RESOLVED / UNRESOLVED / PARTIAL with file:line, then any new defect the round-1 and round-1b fixes introduced (sibling halves included), then a per-row table over the current row IDs. **Subject commit:** `7861221`. **Cross-model findings:** the agy pass's three round-1 findings were delta-verified by the Claude staff-software-engineer lens (the same persona), so no brief went to an external provider this round; the owner's cross-model consent covered round 1 only. **tier-rationale:** unchanged from round 1.

**Delta summary.** Round-1 findings resolved per lens: product-manager 24 of 25 (1 partial); staff-software-engineer 30 of 34 (4 partial) and the agy pass's 3 of 3, the agy R6.2 rejection confirmed as not reproducible; test 22 of 27 (5 partial, the chip-colour Blocker among them); interface 16 of 18 (2 partial); privacy 9 of 9; product-marketing-manager 19 of 21 (2 partial) and 1 deferred by F81; architecture 14 of 15 plus both unrated (1 partial); performance 10 of 14 (3 partial) with the round-1 Blocker resolved.

### Per-lens reviews (verbatim)

#### peer-product-manager-reviewer (Claude route)

## Verdict
Builds the right thing for the user. Every product-manager finding from round 1 is closed except one, which is partly closed. The fix passes add one new Major: E8 promises an undo window that R4.7 does not give. Fix it before lock. The rest are Minors and Nits.

## User & problem context (brief)
- **User.** The primary user is the Cataloger. Secondary users are the Data consumer and the QC re-checker.
- **Job.** Once capture is done, the user wants to see the whole collection honestly, find a swatch, fix a bad scan without losing history, and keep metadata tidy (vision U5 and U7).
- **Validated.** The owner has decided F1–F98.
- **Assumed.** Everything else. Owner dogfooding is the only feedback loop, which is right-sized for a solo open-source tool.
- **Files read.** All four target files at 7861221, the fence file (F1–F98), the round-1 fix file (including Round 1b) and the review log's Round 1. I also read both sibling diffs: `git diff origin/main` and `9acd883..7861221` over docs, excluding collection-mode and agent-reviews.
- **Path shorthand.** Paths are under the root /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/:
  - PRD = docs/product/collection-mode/prd-collection-mode.md
  - COPY = docs/product/collection-mode/prd-collection-mode-copy.md
  - JRN = docs/product/collection-mode/prd-collection-mode-journeys.md
  - DFC = docs/product/data-foundation/prd-data-foundation-copy.md
  - CAP = docs/product/capture-mode/prd-capture-mode.md
  - CAPC = docs/product/capture-mode/prd-capture-mode-copy.md

## Findings

### Round-1 delta verification (my lens's round-1 findings, named as the round-1 fix file boxes them)

| Round-1 finding | Result | Evidence |
|---|---|---|
| PM Major 1 — the undo model | RESOLVED | PRD:500 (R4.7: its own label, ended by any other committed action, re-checks, memory only, no redo; collection rename included per F58); COPY:124 and :236 ("Undo change", separate from E10's "Undo"); JRN:353–363 (UJ6.4-a–k). A delete ends the metadata history, so only E10's Undo is live while E10 is up. The E8 copy it touched brings a new defect (PM2-1). |
| PM Major 2 — a delete from All items never names the collection | RESOLVED | DFC:25 (E8 now reads "Delete ⟨code⟩ from ⟨collection⟩?"); COPY:192–195 (E10 names ⟨collection⟩); JRN:270 (UJ4.4-a), :378 (UJ7.1-g) |
| PM Minor 1 — changed items vs the active search, filters and sort | RESOLVED | PRD:466 (R3.9); PRD:543 (R6.4 deselects); JRN:413–414 (UJ9.1-c/d) |
| PM Minor 2 — single-row selection at P0 | RESOLVED | PRD:543 (R6.4, P0); PRD:498 (R4.5 cites it) |
| PM Minor 3 — the collection-side Flag is unexplained | RESOLVED | PRD:502 (R4.9); COPY:268–276 (E18 and its "restore" variant); JRN:291, :295–296 |
| PM Minor 4 — Find similar results can't be acted on | RESOLVED | PRD:464 (R3.7: each result opens its detail); COPY:180; JRN:237 (UJ3.4-e). The return path is a new gap (PM2-6). |
| PM Minor 5 — no displayed precision | PARTIAL | PRD:448 (R2.11) covers L*, C*, h°, Spread and ΔE2000. R4.2c (PRD:487) shows six derived spaces, and a*, b*, XYZ, u*v*, sRGB and HSL still have no displayed precision or form (PM2-4). |
| PM Minor 6 — no way to cancel a rename | RESOLVED | PRD:405 (R1.3), :497 (R4.4, F92), :501 (R4.8); COPY:246 ("Try another code"); JRN:159, :269, :288 |
| PM Minor 7 — a re-import can split a renamed column | RESOLVED | COPY:278–285 (E19); PRD:501; PRD:102 (F63 non-goal) |
| PM Minor 8 — detail line labels and set-aside causes have no copy | RESOLVED | COPY:287–347 (Display labels); CAPC:45 (Set-aside cause labels table); PRD:486 (R4.2b cites it) |
| PM Minor 9 — E12's "Outside sRGB" misdescribes the file | RESOLVED | COPY:216 |
| PM Minor 10 — R1.7's release fate | RESOLVED | PRD:409; PRD:764 (OQ 10) |
| PM Minor 11 — no real-use metric | RESOLVED | PRD:742 (M4). M4 can pass vacuously, a new validity gap (PM2-5). |
| PM Minor 12 — Capture R11.15g points at the wrong row | RESOLVED | CAP:376 |
| PM Nit 1 — §4's Traces claim | RESOLVED | PRD:470–472 |
| PM Nit 2 — M2's population vs UJ9.8-a | RESOLVED | PRD:740; JRN:436 |
| PM Nit 3 — "narrowed" variants offer no clear actions | RESOLVED | PRD:461 (R3.4); COPY:124–125, :226–227 |
| PM Nit 4 — the zero-count rule blanks E13 and E17 | RESOLVED | COPY:225; PRD:490 (R4.2f withholds "Show history"); JRN:310, :379 |
| PM Nit 5 — rename and delete on two surfaces | RESOLVED | JRN:465; JRN:155–159 |
| PM Nit 6 — DF E33's "Export first" scope | RESOLVED | DFC:27 |
| PM Nit 7 — Capture R1.1 names one creation route | RESOLVED | CAP:138 |
| PM unrated 1 — moving an item between collections | RESOLVED | PRD:102 (F61; its wording is PM2-12) |
| PM unrated 2 — copying to the clipboard | RESOLVED | PRD:102 (F62) |
| PM unrated 3 — column delete or merge | RESOLVED | PRD:102 (F63) |
| PM unrated 4 — owner ratification of the drafted interims | RESOLVED | PRD:756, :761, :765; F34, F40, F64 |

### New defects introduced by the round-1 and round-1b fixes

[MAJOR] **PM2-1** — COPY:170 (E8 body) and COPY:172 (the "clear" variant), against PRD:500 (R4.7). **E8 promises an undo that R4.7 does not give.**
- **What conflicts.** Both E8 texts end: "What was there isn't kept in your file; you can undo this while the file stays open." R4.7 ends the metadata undo history at "any other committed action (a delete, a Flag, a restore, a reorder, a re-scan answer, an import or a re-read)". UJ6.4-f (JRN:358) asserts that "Undo change" disappears after each of those.
- **Scenario.** A cataloger clears Family on 40 selected swatches. E8 renders and promises undo while the file is open. The user then flags one swatch, or answers a re-scan, and a minute later sees the clear was a mistake. "Undo change" is gone. By design the old values exist only in memory, so they are lost for good. The user acted on the copy's promise.
- **Why it is Major.** This is a copy-honesty break on a bulk, destructive, unrecoverable action. It came in with the PMM Minor 4 fix, which reused E10's delete-undo phrasing for a different undo model. M3 measures lost readings, not lost metadata, so nothing instruments this loss.
- **Fix.** Reword both E8 endings to match R4.7, for example: "What was there isn't kept in your file. Undo change brings it back until you do something other than edit — delete, flag, restore, reorder, import or answer a re-scan." Add a case asserting that E8's wording and UJ6.4-f agree.

[MINOR] **PM2-2** — PRD:500 (R4.7); COPY:124 and :236; JRN:358 and :361. **Which actions end the undo history is open-ended, and what the undo acts on is invisible.**
- **(a) The ending list is illustrative, not closed.** Three cases are not stated:
  - A capture save by an in-flight session. R8.3 keeps edits available in flight, and R8.1c classes a capture save as a write. If a save ends the history, undo is effectively gone during a session; if it doesn't, a builder must know that.
  - A column hide or show. R8.8 lists it as a committed write, so by R4.7's wording it ends the history.
  - Whether a refused undo (E6 in flight, UJ6.4-i) keeps its entry for after the session.
- **(b) Scope and feedback.** The history covers the whole file, and E3 offers "Undo change" on every collection surface. From Gouache Set, the button reverses an invisible Swatch Name edit made in Studio Markers. No copy says what was reversed, and E13 offers no "Undo change" after an All items edit.
- **Fix (owner call where it is a WHAT; F29's label and rule stand).**
  - R4.7 states whether a capture save, a column-visibility change, and starting a session end the history (UJ6.4-i already implies starting does not). It also states whether a refused undo keeps its entry.
  - Decide either that "Undo change" is offered only where its change is visible, or that the item it changed is selected and scrolled into view.
  - Add a case for a capture save between an edit and its undo.

[MINOR] **PM2-3** — PRD:595 (R8.8, P0); PRD:297 (Build dependencies row 1); docs/product/post-lock.md (the DF permission-lost item). **A P0 row renders a state that does not exist, with no interim.**
- **What is missing.** R8.8 says a write refused "because permission was lost [renders] the state that PRD owes". DF has no such state; I grepped the DF PRD and its copy file. It sits on the post-lock list for DF's next pass.
- **No stop or interim.** The first Build-dependencies row lists R8.3–R8.11 against DF E10 and E15 only. It names no "Stop:" or "Interim:" for the owed state. JRN has no permission-lost case (UJ9.7-e/f cover the vanished volume and the held file).
- **Scenario.** The file sits on an external or network volume, or macOS revokes folder access. The user edits a name and presses Return. The data holds ("changing nothing"), but what the user sees is left to the builder — possibly a silent revert.
- **Fix.** Give the clause an interim, for example the capture PRD's E26 or DF E10 until DF's state lands. List it as "Interim:" in the Build-dependencies row, and add a UJ9.7 case.

[MINOR] **PM2-4** — PRD:448 (R2.11) against PRD:487 (R4.2c) and :521 (R5.2e). **Displayed precision covers only LCh, Spread and ΔE.**
- **What is missing.** The item detail shows the current value in all six derived spaces. No row sets the precision or form of a*, b*, X, Y, Z, u*, v*, sRGB or HSL. For sRGB, a builder can't tell whether to show 0–255 integers, 0–1 decimals or hex.
- **Scenario.** F62 makes platform text selection in the detail the only way to copy a value. So the displayed form is exactly what a Data consumer pastes into their tool.
- **Fix.** Extend R2.11 to every value R4.2c shows, naming the sRGB and HSL form. Assert one of them in UJ4.1-a.

[MINOR] **PM2-5** — PRD:742 (M4). **M4 can pass vacuously.**
- **The gap.** The population is "each dogfood session's file", and the target is 0 re-scans still awaiting an answer. A dogfood session that produced no corrections scores 0 and passes, yet says nothing about whether R5.7 and R4.2h are reachable.
- **Fix.** Limit the population to sessions that ended with at least one re-scan awaiting an answer. Record the starting count, and report a session with none as "not measured", never 0.

[MINOR] **PM2-6** — PRD:464 (R3.7), PRD:478 (R4.1), JRN:237 (UJ3.4-e). **Opening a Find similar result loses the result list.**
- **The gap.** R4.1 returns a closed detail "to the table", so E9's list is gone.
- **Scenario.** This breaks duplicate-hunting, the likeliest use of the feature: open match 1, close it, then go back to the source item and fire Find similar again for match 2. From All items, the source may be in another collection.
- **Fix.** Closing a detail opened from E9 returns to E9 with its list, or E9 stays up beside the detail. Add a case opening two results in turn.

[MINOR] **PM2-7** — PRD:501 (R4.8) against COPY:309–321 (Column headers, added in round 1). **The duplicate-name check compares against names the user never sees.**
- **The gap.** R4.8 refuses a name equal to "another of the collection's columns or a Swatch field's name". UJ4.6-c treats "Swatch Name" as the field name. The new header table shows the user "Code", "Name", "Alt. code", "Alt. name", "State" and "Spread" instead.
- **Scenario.** Renaming Family to "Name" or "Spread" passes, and the table shows two identically headed columns. The duplicate check misses the collision the user can see and guards one they can't.
- **Fix.** The check covers every header the Column headers table shows, plus imported column names (or the table reverts to full field names). Add a case renaming Family to Name.

[MINOR] **PM2-8** — PRD:412 (R1.10); COPY:133 (E4, shared by the All items view). **The collection-name search rule is unstated.**
- **The gap.** R1.10's "matching collection names too" doesn't say whether a name matches from its start, like codes, or anywhere, like names. UJ7.1-j's "gouache" passes either reading.
- **Scenario.** In All items, typing "set" either floods the table with every Gouache Set item or not, depending on the builder. E4's body in that view also omits collection names from what "the search looks at".
- **Fix.** State the rule in R1.10, and add collection names to E4's body, or give E4 an All items variant.

[MINOR] **PM2-9** — PRD:462 (R3.5); COPY:141 (E5 headline); JRN:221 (UJ3.2-d, added in round 1). **E5 blames the filters when the search alone lists nothing.**
- **The gap.** With a filter active and a search matching nothing, R3.5 renders E5, "No swatch matches these filters". The user clears the filters as told and only then gets E4, "Nothing matches qqq". UJ3.2-d now asserts this two-step misattribution.
- **Fix.** When the search alone lists no item, E4 renders whatever filters are active. E5 renders only when the search lists items and the filters hide them all. Update UJ3.2-d to match.

[NIT] **PM2-10** — PRD:502 (R4.9) against PRD:216 (Vocabulary). R4.9 withholds Flag when the item's "current reading awaits … [the] correction answer". The Vocabulary puts the awaiting-answer mark on the predecessor, not on the current reading, so a literal build offers Flag on ZX-013, contrary to UJ4.7-b. **Fix:** "on a captured item carrying no re-scan-unanswered mark (R2.4i)".

[NIT] **PM2-11** — COPY:264–265 (E17). The body's new sentence describing "Use this reading" is not phase-marked, while the action itself is [phase: action-absent] at P0. F97 moved E18's same mention into a phase-marked variant. **Fix:** use the same pattern for E17.

[NIT] **PM2-12** — PRD:102 (Build contract). "moving an item between collections, which a re-import does (F61)" overstates the route. A re-import adds a new pending item; the readings stay behind. A help writer relaying "re-import moves it" leads users to re-import and then delete the original, losing its history. **Fix:** "…which a re-import approximates, as a new pending item (F61)". F61's non-goal stands.

[NIT] **PM2-15** — PRD:466 (R3.9), :582 (R8.1c), :590 (R8.3). The new live-update rules reach "the table or grid" only. No row says an open item detail or version history view shows a capture save on its item. A stale detail would still read "no current value" and withhold Flag. **Fix:** extend R3.9 or R4.1 to the open detail and history view.

## Biggest risks   (what builds the wrong thing or fails the user)
1. **PM2-1.** E8 tells a user a bulk clear is undoable while the file is open, but R4.7 ends undo at the next non-metadata action. The replaced values are memory-only, so the loss is permanent.
2. **PM2-2.** "Undo change" covers the whole file and gives no feedback, and whether a capture save ends it is undecided. During a session this decides whether undo exists at all.
3. **PM2-3.** A P0 row depends on a sibling state that isn't written, with no interim, on the editing path.
4. **Word budget.** The body is at 11,985 of 12,000 words (fix file, Round 1b). Every round-2 fix must be paid for by trimming, which pushes toward terser rows exactly where ambiguity is the defect class.

## Genuinely solid   (incl. where simplicity is right that a product-zealot would over-spec)
- **The round-1 answers are shaped well.**
  - Undo has its own label, a single "any other action ends it" rule, memory only and no redo. That is the right v1 simplicity.
  - E18 and E19 mirror E16's consequence-first pattern.
  - Escape now abandons all four name and code entries consistently.
  - R3.9 and R6.4 settle live changes against filters in one rule.
  - The non-goals (F61–F63) stop a builder guessing.
- **Every copy state traces.** Each E1–E19 has an owning row that puts the user there, including the new E18 ← R4.9 and E19 ← R4.8, E3/E13 "narrowed" ← R3.4, and E14 "read-only" ← R8.4.
- **New rows state behaviour, not mechanism.** R2.11, R3.9, R6.4 and R8.11 are behaviours. R2.3's "colour-managed" names the observable result, and R8.9 leaves the keyboard route to the build.
- **Sibling halves agree.**
  - DF E8 and E33, DF R2.3, R2.3f and R6.2a, and the capture PRD's R3.7, R5.6, R8.18 and R11.15g match their fences.
  - The new capture cause-label table matches the capture PRD's R8.2 word for word.
  - The ADR-0003 queue row carries the five inputs.
- **Right-sized.** One dogfood metric (M4) plus a one-line E12 read-through, and no funnel or OKRs, is correct for a solo open-source tool. Blocking in-flight restores and delete-undo while leaving correction answers open (F30) is the right line.

## Missing / over-specified
- **Missing:**
  - displayed precision for the other derived spaces (PM2-4)
  - an interim for the permission-lost write (PM2-3)
  - E9's return path (PM2-6)
  - a live update for an open detail or history view (PM2-15)
  - whether a capture save ends the undo history (PM2-2)
- **Over-specified:** nothing new of substance. E18's "restore" variant keyed to build phase is odd but fence-settled (F97). The density is at the word ceiling (see Biggest risks), so round 2 should cut rule-free prose rather than add.

| Row ID | disposition |
|---|---|
| R1.1 | ALIGN |
| R1.2 | ALIGN |
| R1.3 | ALIGN |
| R1.4 | ALIGN |
| R1.5 | ALIGN |
| R1.6 | ALIGN |
| R1.7 | ALIGN |
| R1.8 | ALIGN |
| R1.9 | ALIGN |
| R1.10 | OBJECT (PM2-8) |
| R2.1 | ALIGN |
| R2.2 | ALIGN |
| R2.3 | ALIGN |
| R2.4 | ALIGN |
| R2.4a | ALIGN |
| R2.4b | ALIGN |
| R2.4c | ALIGN |
| R2.4d | ALIGN |
| R2.4e | ALIGN |
| R2.4f | ALIGN |
| R2.4g | ALIGN |
| R2.4h | ALIGN |
| R2.4i | ALIGN |
| R2.5 | ALIGN |
| R2.6 | ALIGN |
| R2.7 | ALIGN |
| R2.8 | ALIGN |
| R2.9 | ALIGN |
| R2.10 | ALIGN |
| R2.11 | OBJECT (PM2-4) |
| R3.1 | ALIGN |
| R3.2 | ALIGN |
| R3.3 | ALIGN |
| R3.4 | ALIGN |
| R3.5 | OBJECT (PM2-9) |
| R3.6 | ALIGN |
| R3.7 | OBJECT (PM2-6) |
| R3.8 | ALIGN |
| R3.9 | ALIGN |
| R4.1 | OBJECT (PM2-15) |
| R4.2 | ALIGN |
| R4.2a | ALIGN |
| R4.2b | ALIGN |
| R4.2c | OBJECT (PM2-4) |
| R4.2d | ALIGN |
| R4.2e | ALIGN |
| R4.2f | ALIGN |
| R4.2g | ALIGN |
| R4.2h | ALIGN |
| R4.3 | ALIGN |
| R4.4 | ALIGN |
| R4.5 | ALIGN |
| R4.6 | ALIGN |
| R4.7 | OBJECT (PM2-1, PM2-2) |
| R4.8 | OBJECT (PM2-7) |
| R4.9 | OBJECT (PM2-10) |
| R5.1 | ALIGN |
| R5.2 | ALIGN |
| R5.2a | ALIGN |
| R5.2b | ALIGN |
| R5.2c | ALIGN |
| R5.2d | ALIGN |
| R5.2e | ALIGN |
| R5.3 | ALIGN |
| R5.4 | ALIGN |
| R5.5 | ALIGN |
| R5.6 | ALIGN |
| R5.7 | ALIGN |
| R5.8 | ALIGN |
| R6.1 | ALIGN |
| R6.2 | ALIGN |
| R6.3 | ALIGN |
| R6.4 | ALIGN |
| R7.1 | ALIGN |
| R7.2 | ALIGN |
| R8.1 | ALIGN |
| R8.1a | ALIGN |
| R8.1b | ALIGN |
| R8.1c | ALIGN |
| R8.1d | ALIGN |
| R8.1e | ALIGN |
| R8.1f | ALIGN |
| R8.2 | ALIGN |
| R8.3 | ALIGN |
| R8.4 | ALIGN |
| R8.5 | ALIGN |
| R8.6 | ALIGN |
| R8.7 | ABSTAIN (platform floor; out of lens) |
| R8.8 | OBJECT (PM2-3) |
| R8.9 | ALIGN |
| R8.10 | ABSTAIN (test seam; test/interface lens) |
| R8.10a | ABSTAIN (test seam; test/interface lens) |
| R8.10b | ABSTAIN (test seam; test/interface lens) |
| R8.10c | ABSTAIN (test seam; test/interface lens) |
| R8.10d | ABSTAIN (test seam; test/interface lens) |
| R8.10e | ABSTAIN (test seam; test/interface lens) |
| R8.10f | ABSTAIN (test seam; test/interface lens) |
| R8.11 | ALIGN |
| E1 | ALIGN |
| E2 | ALIGN |
| E3 | ALIGN |
| E4 | OBJECT (PM2-8) |
| E5 | OBJECT (PM2-9) |
| E6 | ALIGN |
| E7 | ALIGN |
| E8 | OBJECT (PM2-1) |
| E9 | ALIGN |
| E10 | ALIGN |
| E11 | ALIGN |
| E12 | ALIGN |
| E13 | ALIGN |
| E14 | ALIGN |
| E15 | ALIGN |
| E16 | ALIGN |
| E17 | OBJECT (PM2-11) |
| E18 | ALIGN |
| E19 | ALIGN |
| M1 | ALIGN |
| M2 | ALIGN |
| M3 | ALIGN |
| M4 | OBJECT (PM2-5) |

#### peer-staff-software-engineer-reviewer (Claude route)

## Verdict
Ready — proceed. No Blocker is left, but settle the two new MAJORs before lock. The round-1 and round-1b fixes landed cleanly: 30 of my 34 Claude-route items and all three agy items are resolved, and I found no row that contradicts F1–F98. What remains is a set of gaps the fixes opened: the envelope doesn't say how wide the file-wide data is, and the storage constraints the rows now imply never reach ADR-0003.

## What I reviewed
- **Subject.** Requirements mode, at commit `7861221` (the worktree HEAD). Read in full:
  - `docs/product/collection-mode/prd-collection-mode.md`
  - `docs/product/collection-mode/prd-collection-mode-journeys.md`
  - `docs/product/collection-mode/prd-collection-mode-copy.md`
  - `docs/product/collection-mode/prd-collection-mode-oq-results.md`
- **Contract sources.** Read in full:
  - `-fences.md` (F1–F98, the fence → row map, Rejected findings)
  - `-round-1-fixes.md`, including Round 1b
  - `docs/agent-reviews/2026-09-24-prd-collection-mode-peer-reviews.md`: both of my lens's round-1 sections (Claude route and agy) and the disposition table
  - The rows I relied on in the Capture Mode, Data Foundation (DF) and Import PRDs and their copy files; `docs/decisions/README.md`; `docs/product/post-lock.md`; ADR-0001.
- **Sibling halves.** `git diff origin/main -- docs ':!docs/product/collection-mode' ':!docs/agent-reviews'` at merge-base `dc1b747`. For what the fixes changed specifically, `git diff 9acd883 7861221` over the same paths: Capture F71 (R1.1, R3.7, R5.6, R8.18, R11.15g, E1, E30, the cause-label table, UJ3.3-h/k), DF F52 (R1.2, R2.3, R2.3f, R6.2a, R7.2, R7.6k, E8, E33, DJ rows), the ADR-0003 row in the decisions README, and two new post-lock items.
- **Checks I computed** in memory with python; no file was touched:
  - Linear sRGB and Display P3 membership of every seeded L\*C\*h°, D50/2° with Bradford to D65.
  - The body word count by rule 14's method.
  - By hand: every sort oracle, every count, and the ΔE2000 boundary in UJ3.4-f.
- **Could NOT verify:**
  - Real ColorSync or NSScreen behaviour.
  - Any timing.
  - SQLite journal and checkpoint behaviour under ADR-0003. 2MJ2 is reasoning, not a run.
  - The Sharma et al. table itself.
  - M3 and anything else that needs a build.

## Findings

### Round-1 delta verification (my lens: Claude route and agy)

| Round-1 finding | Status | Evidence |
|---|---|---|
| B1 ZX-013 outside sRGB | RESOLVED | journeys:81 (C\* 30; I computed linear sRGB R +0.057), journeys:51–57 (margin rule), prd:442 (R2.5's sRGB rule), prd:761 (OQ 7: Bradford, zero tolerance) |
| MJ1 what the chip renders | RESOLVED | prd:423 (R2.3), prd:605 (R8.10c), journeys:186–187 |
| MJ2 settled/unsettled | PARTIAL | This PRD is fixed: prd:216–217, 438, 486; copy:216, 307. DF's E11 and E26, which render on this PRD's surfaces, still say "marked as not yet settled" (DF copy:37–38) → 2MN18 |
| MJ3 stop rule vs cells | RESOLVED | prd:295–312 (Stop:/Interim:), fences:707–711 (F65) |
| MJ4 undo model | RESOLVED | prd:500, copy:124/236, journeys:353–363. The edge of "committed action" is still loose → 2MN5 |
| MJ5 unlisted item stays selected | RESOLVED | prd:543 (R6.4), journeys:339, 413–414 |
| MJ6 restores and answers in flight | RESOLVED | prd:590 (R8.3), copy:151, journeys:276, 281, 314, 322 |
| MJ7 reference machine and end event | RESOLVED | prd:274, 574, 739, 755. "Cold" and "paging" are undefined → 2MN11 |
| MJ8 envelope bounds | PARTIAL | prd:580–585 (R8.1a–f) and prd:277–279 are fixed. R8.2 (prd:589) states no width and UJ9.5-b (journeys:424) declares no imported columns → 2MJ1. No budget for deleting a collection → 2MN12. No bound on history length → 2MN13 |
| MN1 what one filter is | RESOLVED | prd:461 |
| MN2 sort orders and locale | RESOLVED | prd:459, 603; journeys:93, 230–231 |
| MN3 sorts across references | RESOLVED | prd:460; journeys:232, 380, 383. The wording's scope is still ambiguous → 2MN3 |
| MN4 ties and measured order | RESOLVED | prd:464, 525; journeys:238, 323 |
| MN5 code equal to its own | RESOLVED | prd:497; journeys:268 |
| MN6 the item in view, persistence | RESOLVED | prd:552–553; journeys:449–450. Per collection or global is unstated → 2MN10 |
| MN7 display changes, straddling window | RESOLVED | prd:442, 603; journeys:196–198 |
| MN8 read-only actions hidden or disabled | RESOLVED | prd:591; journeys:415 |
| MN9 other write failures | PARTIAL | prd:595. The permission-lost state is owed by DF and has no interim → 2MN14 |
| MN10 open detail whose item is gone | RESOLVED | prd:478; journeys:277, 419 |
| MN11 keyboard routes | RESOLVED | prd:596; journeys:327, 398 |
| MN12 bare sibling IDs | RESOLVED | journeys:105–106, 165–170, 270–277 |
| MN13 readbacks | RESOLVED | prd:597–608 (R8.10a–f). A new gap (shapes and causes) → 2MN16 |
| MN14 Capture R8.18 "every tally" | RESOLVED | Capture prd:297 |
| MN15 DF obligation for column visibility | RESOLVED | prd:634; DF prd:100 |
| MN16 Capture R3.7 anchor | RESOLVED | Capture prd:161; journeys:275 |
| MN17 displayed precision | PARTIAL | prd:448 (R2.11). The six spaces in R4.2c (prd:487) are still open → 2MN17 |
| MN18 reordering while narrowed | RESOLVED | prd:446; journeys:399 |
| N1 / N2 / N3 | RESOLVED | prd:411; prd:405; journeys:436 with prd:740 |
| unrated 1–4 | RESOLVED | prd:502 and journeys:291–292 (F77, F78); journeys:236; prd:756, 761, 765 (F34, F40, F64) |
| agy Finding-1: collection name in All items search | RESOLVED | prd:412 (R1.10), journeys:381. How the name matches is unstated → 2MN1 |
| agy Finding-2: bulk-setting Swatch Code | RESOLVED — the rejection is **confirmed**; I could not reproduce the defect | prd:184–185 (Swatch Code is not an editable field), prd:541, fences:224 (F7), journeys:344 (UJ6.2-e), fences:1032 |
| agy Finding-3: Find similar ties | RESOLVED | prd:464, journeys:238 |

### New findings (introduced by the round-1 and round-1b fixes, unless marked pre-existing)

**[MAJOR] 2MJ1 — R8.2 and UJ9.5-b: the file-wide budgets state no data width.**
- **Locations.** prd:589, prd:574, prd:412, fences:573 (F43), journeys:424.
- **The gap.**
  - F43 applied IMPORTED_COLUMNS_CEILING to R8.1 only.
  - R8.2 promises R8.1a–c's budgets at FILE_ITEMS_CEILING without saying how wide the data is. R8.1d's "nothing left loading that a search waits on" is not carried into R8.2 either.
  - F86 makes All items search match imported values.
  - At both ceilings that is 100,000 × 20 × 200 = 4×10⁸ characters, contains-matched within 100 ms p95 on an 8 GB M1. No memory ceiling is stated anywhere.
- **Why it matters.** One engineer builds a normalised in-memory corpus or index of about 0.4–0.8 GB. Another assumes no width.
- **Mutation.** Drop imported values from the All-items search. UJ9.5-b declares no columns and no UJ7.1 case searches an imported value, so everything stays green.
- **Fix.**
  - R8.2 names the width its budgets hold at: per collection up to IMPORTED_COLUMNS_CEILING, or a stated narrower file-wide width with its own interim under OQ 2 or OQ 12.
  - R8.2 says whether R8.1d's no-lazy-tail clause applies.
  - UJ9.5-b declares its columns.
  - Add an All-items imported-value search case.
  - Feasibility → performance lens.

**[MAJOR] 2MJ2 — R4.3, R6.2, R8.1f and R8.11: the storage constraints the envelope now implies are not handed to ADR-0003.**
- **Locations.** prd:496, 541, 585, 595, 607, 612; journeys:362, 426; decisions README:22; Capture prd:69 and 189.
- **(i) When removed text must be gone.**
  - UJ6.4-j reads the bytes of the file and of what sits beside it *while "Undo change" is still offered*.
  - The ADR-0003 input says only "gone from the file's bytes": no timing, and nothing about journal or WAL sidecar files.
  - WAL is the natural fit for R8.3's and R8.11's "browse while capture writes". Under WAL with default checkpointing, the pre-image pages holding "Pink" stay in the main file, and earlier frames stay in the -wal file, until a checkpoint. UJ6.4-j then fails, after the one-way door has closed.
- **(ii) Bulk writes against in-flight capture.**
  - R8.8 makes a bulk write atomic, and R8.1f allows it 2 s.
  - SQLite has a single writer. A 1.5 s atomic clear of 10,000 items delays an in-flight capture save by about 1.4 s, past any plausible ROW_CONFIRM_BUDGET, so UJ9.5-d fails.
  - No row tells the builder that in flight the bound is ROW_CONFIRM_BUDGET minus one save, not BULK_WRITE_BUDGET.
- **Fix.**
  - R4.3 and R6.2 say "from the moment the write lands, while the file is open, including any journal or log beside it". Alternatively, the owner picks a later moment. That is a question of *when*, not a re-opening of F35.
  - R8.11 says an atomic Collection Mode write never holds the writer longer than ROW_CONFIRM_BUDGET minus one save while a session is in flight, or else queues behind capture.
  - Add both to the ADR-0003 row and to DF F52 item (7).
  - This must be paid for from the 15 words of budget headroom.

**MINOR**
- **2MN1 — R1.10 and E4 (prd:412, copy:133).**
  - Whether a collection name matches from its start, as codes do, or anywhere, as names do, is unstated. Typing "set" lists Gouache Set's items under one reading and nothing under the other. UJ7.1-j ("gouache") passes both.
  - E4's body, which the All items view shares, doesn't mention collection names.
  - No case searches an imported value from All items.
  - Fix: name the match mode, extend E4, and add a case.
- **2MN2 — R2.4b and R2.5 (prd:431, 442; DF prd:142, 145). Pre-existing.**
  - DF keeps a gamut-clipped flag per derived value, so one reading has one flag per condition set. "The current reading's gamut-clipped flag" doesn't say which.
  - Fix: "the working-set value's flag".
- **2MN3 — R3.3 (prd:460; fix file:720).**
  - The dashed clause leaves open whether the most-common rule also applies inside one collection's table. The round-1b note itself says an in-collection tie is "reported, not decided".
  - It also leaves open whether a mismatched non-spectral reading counts toward the majority, and whether "listed" (so search and filters) moves the like set.
  - Fix: in a collection's table, like means the collection's own reference. In All items, like means the most common pair among the listed items that have values, a tie going to the first collection. A mismatched non-spectral reading is never like.
- **2MN4 — R3.7, R3.8 and R4.2c (prd:464–465, 487).**
  - Find similar is offered "on an item with a current value". For ZX-006, whose working-set value is absent, it is offered with nothing to compare. R4.2c doesn't say which condition's six spaces ZX-006's detail shows. This part is pre-existing.
  - "Values already worked out" does not say whether candidates' persisted non-working sets (DF R3.1) count, so the list would depend on what was computed on request earlier.
  - Fix: compare working-set values only, and state the case where the working-set value is absent.
- **2MN5 — R4.7 (prd:500, 590, 595).**
  - "Any other committed action" leaves open whether a capture save by a session in flight, a column hide or show (which R8.8 calls a committed change), or "New collection" ends the undo history.
  - That decides whether the uniqueness-refusal branch is reachable at all: undoing a rename after a new collection has taken the old name.
  - Fix: list the set as closed.
- **2MN6 — R5.4 and the History lines copy (prd:526, copy:347).**
  - For an item with no current value, such as flagged ZX-018 or quarantined ZX-012, every earlier reading gets "Not compared — measured under a different light or condition". That reason is false.
  - Fix: show no distance line, or a no-current-value line.
- **2MN7 — R5.5 (prd:527; DF prd:131, 133). Pre-existing.**
  - "Use this reading" on a *never-true* earlier reading is not excluded. Restoring it puts back a value the user said was wrong, and it re-enters over-time views through the restored copy.
  - Fix: state whether it is offered.
- **2MN8 — R6.2 (prd:541). Pre-existing.**
  - At or below BULK_CONFIRM_COUNT, nothing says what commits the "Set a field" value or cancels the form: Return, Escape, a button? There is no copy label either.
  - Fix: mirror R4.3's Return and Escape, or name the action.
- **2MN9 — E8 copy against R4.7 (copy:170, prd:500).**
  - "You can undo this while the file stays open" over-promises. R4.7 ends the history at the next committed action of any other kind.
- **2MN10 — R7.1 and R7.2 (prd:552–553).**
  - Whether the Grid/Table choice and the swatch size are per collection or one per open file is unstated. F71's "as R3.6 does" implies per collection.
  - Fix: say so.
- **2MN11 — R8.1, R8.1d, R8.1e, M1 and the Timing workload (prd:582–584, 739, 755; journeys:95, 423).**
  - "Cold" is undefined: app launch only, or the OS page cache purged too.
  - The input and rate for "paging" under DROPPED_FRAME_SHARE are undeclared: Page Down or trackpad momentum, and at what speed.
  - Two runs will get different numbers.
- **2MN12 — R8.1 (prd:580–585).**
  - Deleting a collection of ROWS_CEILING items, and undoing that delete, falls in no class. It is neither "single-item" (R8.1c) nor in R8.1f's list. With F35's byte scrub over every reading, it is probably the slowest write in the app.
  - Fix: add it to R8.1f.
- **2MN13 — R8.1b and R8.2 (prd:581, 589; journeys:427).**
  - The history-open budget has no bound on readings per item. R8.2 says "however many", and UJ9.5-e's 500-reading history is not timed.
- **2MN14 — R8.8 (prd:595; post-lock:41).**
  - The permission-lost branch renders "the state that PRD owes", which doesn't exist yet and has no interim. The Legend says no P0 row lacks an interim (prd:322).
  - Fix: give an interim, or list R8.8 under no-interim.
- **2MN15 — R8.11 (prd:612; Capture prd:69, 435).**
  - ROW_CONFIRM_BUDGET is TBD with no interim; the capture PRD says "None for the two TBDs". So P0 R8.11 and UJ9.5-d have no number to test against, and the Legend's "None" doesn't hold.
  - Fix: list it as no-interim, or give a relative interim (confirmation no later than the same save with no Collection Mode write running, plus N ms).
- **2MN16 — R8.10 (prd:597, 604–605; journeys:195, 254, 291, 428; copy:290; Capture copy set-aside intro).**
  - No readback identifies a mark's *shape*, yet UJ2.1-k and UJ9.6-a assert shapes.
  - UJ4.1-c and UJ4.7-a read the set-aside cause "by its label". That contradicts the Display labels rule (tests read by identifier, never by the words) and the capture PRD's "never by its label".
  - Fix: grant shape and cause identifiers in R8.10b/c.
- **2MN17 — R4.2c (prd:487). MN17 residual.**
  - The displayed precision and encoding of a\*, b\*, XYZ, Luv, sRGB (0–1 or 0–255) and HSL are unstated. F62 makes displayed text the copy route, so this matters to the user.
- **2MN18 — R4.2h and R5.7, through DF E11 and E26 (DF copy:37–38). MJ2 residual.**
  - Both sibling states say "marked as not yet settled", beside this PRD's "Awaiting answer". That contradicts F37's own intent that settled/unsettled now describe rows only.
  - Fix: reword the DF copy under a dated DF fence.
- **2MN19 — sibling Capture, cited by R4.2b.**
  - F96's new label table (Capture copy:56) is "the one place" labels are written and says "flagged as missing or damaged".
  - Capture R5.8 and R6.6 (Capture prd:215, 233) quote "flagged: missing or damaged". The fix pass reported this but did not fix it.
  - Fix: align them editorially under F71.

**NIT**
- **2N1 — Row transitions (prd:128–144).** No line covers an item that a re-read finds gone and removes from present (R8.5, R4.1, UJ9.3-b).
- **2N2 — E10's surfaces (prd:368, 697).** E10 is listed on the item detail. But R4.1 closes the detail and shows E10 over "the table", which from an All-items detail is the All items view, where E10 isn't listed.
- **2N3 — R3.7.** In All items, a tie on both distance and Swatch Code (the same code in two collections) has no final sort key.
- **2N4 — E9's ⟨distance⟩ (prd:724).** It could render "3.0" (the constant's number) or "3.00" (R2.11's two places).
- **2N5 — Harness (journeys:57, 236, 380, 383).**
  - "Well inside sRGB" isn't quantified: BI-1 sits 0.016 linear from black. That is harmless, because no case reads its marks.
  - Two different fixtures are both named "Daylight".
- **2N6 — Build dependencies (prd:297).** "Stop: ADR-0006" sits on row 1 only. Every row needs the deployment target. This is moot while ADR-0003 stops every row.

## Clarifying questions for the author
1. Do R8.2's budgets hold with up to IMPORTED_COLUMNS_CEILING imported columns per collection (up to 4×10⁸ characters searched), or at a stated narrower width?
2. Does R8.1d's "nothing left loading that a search waits on" apply to the All items view?
3. Must replaced or deleted text be gone from the file *and* any journal or log beside it from the moment the write lands while the file is open, as UJ6.4-j reads it, or only by the next close or checkpoint?
4. While a session is in flight, may an atomic Collection Mode write hold the writer for up to BULK_WRITE_BUDGET, or must it yield within ROW_CONFIRM_BUDGET minus one save?
5. Until the capture PRD's OQ 5 closes, what does UJ9.5-d test R8.11 against?
6. Until DF names it, which state renders when permission to the file is lost?
7. Does an All-items collection-name match start at the beginning of the name, or anywhere in it?
8. Inside one collection's table, is "like" the collection's own reference or the most common pair, and does a mismatched non-spectral reading ever count toward the majority?
9. Does a capture save in flight, a column hide or show, or "New collection" end "Undo change" history?
10. Which readback identifies a mark's shape, and a set-aside cause, without matching wording?
11. What does an earlier reading's distance line show when the item has no current value?
12. Should E8 say undo lasts until the next change of any other kind, rather than "while the file stays open"?
13. Which derived set's gamut-clipped flag drives the outside-sRGB mark and R2.5's sRGB-display rule?
14. Is Find similar offered when the chosen item's working-set value is absent (ZX-006)? Does it ever compare a candidate's persisted non-working set? What do R4.2c's lines show for such an item?
15. What does "cold" mean for the first open, and what input and rate count as "paging"?
16. What budget and progress rule apply to deleting, and undoing the delete of, a ROWS_CEILING collection?
17. Does R8.1b's history-open budget hold at any number of readings?
18. What precision and encoding do the detail's a\*, b\*, XYZ, Luv, sRGB and HSL values show?
19. Will DF's E11 and E26 drop "not yet settled" in favour of "awaiting your answer"?
20. How is a "Set a field" value committed and cancelled at or below BULK_CONFIRM_COUNT?
21. Is "Use this reading" offered on a never-true reading?
22. Are the Grid/Table choice and the swatch size per collection?
23. Which cause string is canonical in the capture PRD: "flagged as missing or damaged" or "flagged: missing or damaged"?

## Claimed properties
- **Every box in the round-1 fix file ticked, for my lens — holds.** See the delta table; four items are PARTIAL on substance.
- **Body at 11,985 words, within 12,000 — holds.** I recomputed it. There are 15 words of headroom, so every fix above must be paid for by trimming.
- **Every own constant has a candidate, an interim and an OQ — holds** for all ten.
- **"No P0 row lacks an interim" — does not hold in substance.** R8.11 leans on the capture PRD's TBD ROW_CONFIRM_BUDGET, and R8.8 on DF's owed permission-lost state (2MN14, 2MN15).
- **Gamut margins of at least 0.03 — hold for all 11 declared memberships** under Bradford:
  - The smallest margins are GS-002 at +0.038 and ZX-014 at +0.044.
  - ZX-002 is outside sRGB by 0.061 and inside Display P3 by 0.052.
  - ZX-003 is outside Display P3 by 0.058.
- **Sort and count oracles hold:** UJ3.3-a–k, UJ4.1-b, UJ7.1-e/i/l, UJ9.3-a; 16 and 18 readings; the 11- and 10-item ranges; E33's 2 + 3. UJ3.4-f's ΔE2000 of 3.0000 is analytically exact: an achromatic pair with S_L = 1 at an average L\* of 50.
- **No row contradicts F1–F98 — holds.**
- **Sibling halves landed — holds:** Capture F71, DF F52, the ADR-0003 row, post-lock. The one exception is the sibling inconsistency in 2MN19.
- **Capture R11.15g's pointer and R8.18's tallies — now hold.**
- **agy Finding-2 "not reproduced" — holds.**
- **M3 = 0, the timings, and CMM agreement at the boundary — unverified.**

## Genuinely sound
- **F29's "any other committed action ends the history".** "Undo change" and E10's "Undo" can never both be live, and an import or delete ends the history before an undo could recreate a duplicate. A zealot would ask for an undo-conflict matrix; none is needed.
- **R2.5 mirrors the stored flag on an sRGB or unreported display.** It doesn't re-test, which avoids a disagreement between DF's flag and a live test using a different chromatic adaptation.
- **The clip method is left to the build.** Which in-gamut colour a clipped chip shows is a build choice, and that is right: the cannot-show mark carries the honesty.
- **The fixtures are well built.** The 0.03 margins under a pinned Bradford adaptation keep M2 independent of the colour engine. UJ3.4-f's boundary is exact. UJ7.1-l tells F94's rule apart from both plausible wrong rules.
- **The Build dependencies table (F91).** Stop:/Interim: labels sit inside the fixed three columns, and ADR-0003 honestly stops every row.
- **R1.7 is withheld whole** behind DF OQ 20 rather than half-specified.
- **F77 is logged to post-lock** rather than forcing a DF change now.

## Deferred
- **peer-architecture-reviewer and a database lens:** the mechanism behind 2MJ2 — journal mode, checkpoint or secure delete, and how long an atomic bulk write holds the lock against capture saves — and where colour management lives across ADR-0001's core/shell boundary.
- **peer-performance-reviewer:** whether 2MJ1 is feasible (100 ms contains-search over up to 4×10⁸ characters and the memory it takes on an 8 GB M1), and the cost of a collection delete with the byte scrub.
- **peer-product-manager-reviewer:** M4's population (are re-scans raised after the session counted?), and the product intent behind 2MN7.
- **peer-product-marketing-manager-reviewer:** the wording of E8 (2MN9), of E4 in All items (2MN1), and of DF E11/E26 (2MN18).
- **peer-test-reviewer:** 2MN16, the reproducibility in 2MN11, 2N5, and the back-references written "That E8" (journeys:273, 378).
- **peer-interface-reviewer:**
  - No copy label for hiding or showing a column (R2.10), or for the "Set a field" chooser and its commit.
  - R4.8's "a Swatch field's name" means "Swatch Name", while the column header reads "Name", so an imported column renamed "Name" duplicates a visible header.

| Row ID | disposition |
|---|---|
| R1.1 | ALIGN |
| R1.2 | ALIGN |
| R1.3 | ALIGN |
| R1.4 | ALIGN |
| R1.5 | ALIGN |
| R1.6 | ALIGN |
| R1.7 | ALIGN |
| R1.8 | ALIGN |
| R1.9 | ALIGN |
| R1.10 | OBJECT (2MN1) |
| R2.1 | ALIGN |
| R2.2 | ALIGN |
| R2.3 | ALIGN |
| R2.4 | ALIGN |
| R2.4a | ALIGN |
| R2.4b | OBJECT (2MN2) |
| R2.4c | ALIGN |
| R2.4d | ALIGN |
| R2.4e | ALIGN |
| R2.4f | ALIGN |
| R2.4g | ALIGN |
| R2.4h | ALIGN |
| R2.4i | ALIGN |
| R2.5 | OBJECT (2MN2) |
| R2.6 | ALIGN |
| R2.7 | ALIGN |
| R2.8 | ALIGN |
| R2.9 | ALIGN |
| R2.10 | ALIGN |
| R2.11 | ALIGN |
| R3.1 | ALIGN |
| R3.2 | ALIGN |
| R3.3 | OBJECT (2MN3) |
| R3.4 | ALIGN |
| R3.5 | ALIGN |
| R3.6 | ALIGN |
| R3.7 | OBJECT (2MN4, 2N3) |
| R3.8 | OBJECT (2MN4) |
| R3.9 | ALIGN |
| R4.1 | ALIGN |
| R4.2 | ALIGN |
| R4.2a | ALIGN |
| R4.2b | OBJECT (2MN16, 2MN19) |
| R4.2c | OBJECT (2MN4, 2MN17) |
| R4.2d | ALIGN |
| R4.2e | ALIGN |
| R4.2f | ALIGN |
| R4.2g | ALIGN |
| R4.2h | OBJECT (2MN18) |
| R4.3 | OBJECT (2MJ2) |
| R4.4 | ALIGN |
| R4.5 | ALIGN |
| R4.6 | ALIGN |
| R4.7 | OBJECT (2MN5) |
| R4.8 | ALIGN |
| R4.9 | ALIGN |
| R5.1 | ALIGN |
| R5.2 | ALIGN |
| R5.2a | ALIGN |
| R5.2b | ALIGN |
| R5.2c | ALIGN |
| R5.2d | ALIGN |
| R5.2e | ALIGN |
| R5.3 | ALIGN |
| R5.4 | OBJECT (2MN6) |
| R5.5 | OBJECT (2MN7) |
| R5.6 | ALIGN |
| R5.7 | OBJECT (2MN18) |
| R5.8 | ALIGN |
| R6.1 | ALIGN |
| R6.2 | OBJECT (2MJ2, 2MN8) |
| R6.3 | ALIGN |
| R6.4 | ALIGN |
| R7.1 | OBJECT (2MN10) |
| R7.2 | OBJECT (2MN10) |
| R8.1 | OBJECT (2MN11, 2MN12) |
| R8.1a | ALIGN |
| R8.1b | OBJECT (2MN13) |
| R8.1c | ALIGN |
| R8.1d | OBJECT (2MN11) |
| R8.1e | OBJECT (2MN11) |
| R8.1f | OBJECT (2MJ2) |
| R8.2 | OBJECT (2MJ1, 2MN13) |
| R8.3 | ALIGN |
| R8.4 | ALIGN |
| R8.5 | ALIGN |
| R8.6 | ALIGN |
| R8.7 | ALIGN |
| R8.8 | OBJECT (2MN14) |
| R8.9 | ALIGN |
| R8.10 | OBJECT (2MN16) |
| R8.10a | ALIGN |
| R8.10b | OBJECT (2MN16) |
| R8.10c | OBJECT (2MN16) |
| R8.10d | ALIGN |
| R8.10e | ALIGN |
| R8.10f | ALIGN |
| R8.11 | OBJECT (2MJ2, 2MN15) |
| E1 | ALIGN |
| E2 | ALIGN |
| E3 | ALIGN |
| E4 | OBJECT (2MN1) |
| E5 | ALIGN |
| E6 | ALIGN |
| E7 | ALIGN |
| E8 | OBJECT (2MN9) |
| E9 | OBJECT (2N4) |
| E10 | OBJECT (2N2) |
| E11 | ALIGN |
| E12 | ALIGN |
| E13 | ALIGN |
| E14 | ALIGN |
| E15 | ALIGN |
| E16 | ALIGN |
| E17 | ALIGN |
| E18 | ALIGN |
| E19 | ALIGN |
| M1 | OBJECT (2MN11) |
| M2 | ALIGN |
| M3 | ALIGN |
| M4 | ABSTAIN (out of lens) |

2N1, 2N5 and 2N6 target the Row transitions table, the Harness and the Build dependencies table, none of which carries a row ID.

#### peer-test-reviewer (Claude route)

## Verdict
Trustworthy after fixing Blockers. The round-1 fixes closed 22 of my 27 findings, and every fixture number I recomputed now holds. One Blocker is only partly fixed: no case asserts the value of the colour triplet the chip sends to the display, so a chip rendered from the stored sRGB value still passes on Display P3. The fixes also added three Majors: byte checks that miss lower-cased or tokenised leftovers, change triggers only half covered, and a mark-shape rule with no way to read the shapes.

## Coverage map (brief)
- **Now well covered:**
  - Token, action and E9 readback (R8.10b).
  - The in-flight guard in all four states.
  - F30's refused restores and undo.
  - Undo order, each undoable kind, and its privacy check, including after a crash.
  - Export collection, including with a search active.
  - Find similar's like-with-like exclusion, now isolated (DL-101 sits at ΔE 0 and FS-005 at about 0.3, so both are excluded only by reference).
  - The sort and constant boundaries.
  - The crash moment for a bulk edit and a reorder.
- **Load-bearing gaps:**
  - The chip's rendered colour value (B1).
  - Byte checks against lower-cased or tokenised forms of removed text (R2-M1).
  - Four of R3.9's seven change triggers, its "moves it" clause, and four of R6.4's seven deselect triggers (R2-M2).
  - Any way to read a mark's shape (R2-M3).
  - Lost file permission has no injection path.
  - R8.11 is P0 but is tested only by a P1 bulk clear.

## Findings
Paths: PRD = `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode.md`; J = `…/collection-mode/prd-collection-mode-journeys.md`; C = `…/collection-mode/prd-collection-mode-copy.md`; CAP = `…/capture-mode/prd-capture-mode.md`; CAPJ = `…/capture-mode/prd-capture-mode-journeys.md`; CAPC = `…/capture-mode/prd-capture-mode-copy.md`; DF = `…/data-foundation/prd-data-foundation.md`.

**Round-1 delta verification (my round-1 findings, against subject commit 7861221)**

| Round-1 ID | Status | Evidence |
|---|---|---|
| B1 | PARTIAL | PRD:605 (R8.10c) grants fill, value, triplet and a clipped flag; J:186–187 assert fill and "clipped / not clipped" only. No case asserts the triplet's value or any chip value on an item with several readings. See the B1 finding below. |
| B2 | RESOLVED | J:81: ZX-013 is now C*30. Recomputed with Bradford: inside sRGB by +0.057. Every declared membership clears 0.03; the thinnest is GS-002 at +0.038 (J:51–57). |
| MJ1 | RESOLVED | PRD:604 (R8.10b): token values, actions offered or disabled, fields, E9's list |
| MJ2 | RESOLVED | J:362–363 (UJ6.4-j/k), J:354 (UJ6.4-b), J:353–357 |
| MJ3 | RESOLVED | J:46–49 (in-flight runs); J:160, 267, 272, 293, 350, 397 |
| MJ4 | RESOLVED | PRD:590 (R8.3); J:276, 281, 322, 361 |
| MJ5 | PARTIAL | J:404–407 re-run the read-only and network cases; J:398 and J:327 add keyboard routes for drag and Compare. But J:429 (UJ9.6-b) still covers only E2, E3, E14 and E17 (R2-m4). |
| MJ6 | RESOLVED | J:174–175 |
| MJ7 | RESOLVED | J:421 (qqq) |
| MJ8 | RESOLVED | J:233, J:236 |
| m1 | RESOLVED | J:228–229. The Near Greys items at C* 2.9 and 3.1 catch a threshold at either 2.8 or 3.2. |
| m2 | RESOLVED | J:347 (exactly 10); J:238. GA/GB is exactly 3.0 in floating point: SL = 1, ΔC = ΔH = 0. |
| m3 | RESOLVED | J:87, J:93–95 |
| m4 | RESOLVED | J:194 (E23; 10/2/1 recounted) |
| m5 | RESOLVED | PRD:761 (Bradford), J:51–54 |
| m6 | RESOLVED | PRD:740–741, J:436–437 |
| m7 | RESOLVED | PRD:765 |
| m8 | PARTIAL | J:418 now keeps ZX-013 selected, but a selected item that the re-read removes is now asserted nowhere (R2-m2). |
| m9 | PARTIAL | J:431–432 pinned; J:433 (UJ9.7-d) still says "crash during the write" (R2-m3). |
| m10 | RESOLVED | C:292–307, PRD:596, J:428. The shape readback is a new gap (R2-M3). |
| m11 | RESOLVED | PRD:717–719, PRD:464, PRD:411 |
| m12 | RESOLVED | PRD:216–217 |
| m13 | RESOLVED | J:400, J:399, PRD:446 |
| m14 | RESOLVED | J:157, 195, 234, 265, 283, 373, 378, 382, 258, 262, 324, 427, 411 |
| m15 | RESOLVED | PRD:764 |
| m16 | RESOLVED | J:351; CAPJ:111, CAPJ:114 |
| n1 | RESOLVED | J:230, J:217, J:197 |

**[BLOCKER] B1 (carried, PARTIAL) — J:187 (UJ2.1-c), PRD:423 (R2.3), PRD:605 (R8.10c) — no case asserts the colour the chip actually sends to the display.**
- **What R2.3 promises:** the working-set value colour-managed to the display, "never the Data Foundation PRD's stored sRGB value".
- **Mutation that stays green:** build the chip's triplet by converting DF's stored, already-clipped sRGB value into the display's space.
  - On Display P3 that gives ZX-002 = (90.2, 197.1, 104.0)/255, reported "not clipped", because converting sRGB to P3 clips nothing.
  - The true triplet is (64.7, 196.8, 103.6)/255 (recomputed: D50→D65 Bradford, then P3).
  - The chip still self-reports the working-set value L*70 C*80 h°150, so UJ2.1-c, UJ2.1-b, UJ9.8-a and M2 (at 100%) all pass. This is exactly the dishonesty F40 was written to prevent.
- **Also untested:** "renders the oldest reading". No case reads the chip value of an item with several readings: ZX-002 has one, and UJ5.3-j reads after the restore, when the current and oldest values are equal.
- **Fix:**
  - UJ2.1-c asserts ZX-002's P3 triplet equals the declared (64.7, 196.8, 103.6)/255 within ±1/255. The harness works out the declared value by OQ 7's interim, as it already does for margins.
  - Add a case reading ZX-013's chip value, T3's (60, 30, 190), before any restore.

**[MAJOR] R2-M1 — PRD:606 (R8.10d), J:105, 108, 110, 169–170, 259, 261, 342, 349, 362–363; DF:243 — the byte checks miss removed text that survives in lower-cased, normalised or tokenised form.**
- **What the checks do:** assert "bytes hold the text X nowhere" for mixed-case, often multi-word strings ("Warm Grey 1", "Sky Blue", "Pink").
- **Mutation that stays green:** an FTS5 search index — the browsing research's measured option — with its default tokeniser and no secure-delete.
  - That tokeniser lower-cases words and stores each one separately, compressed against the words beside it, and it keeps deleted terms in older index segments until they are merged.
  - A lower-cased normalised shadow column (e.g. "warm grey 1") also survives an exact-case search.
  - If ADR-0003 picks UTF-16 text encoding, a UTF-8 byte search finds nothing at all.
  - Either way T1, T4, T6, UJ4.2-a/c, UJ6.2-c, UJ6.3-b, UJ1.3-e/f and UJ6.4-j/k all pass while F35/DF R6.2a is broken.
- **Fix:** have the Harness define "holds text T nowhere" as all of the following:
  - T, and each word unique to T in the seeded file (e.g. "warm", "azure"), in any letter case and in Import R2.3's normalised form;
  - searched in UTF-8 and UTF-16LE/BE, across the file and its -wal, -shm and -journal files;
  - plus no match at SQLITE_READER_FLOOR over every table, shadow tables included.
  - Add one seeded-residue control case that the check must catch.

**[MAJOR] R2-M2 — PRD:466 (R3.9), PRD:543 (R6.4); J:339, 413–414, 418 — the new change-trigger rows are only half covered.**
- **R3.9:** 3 of its 7 triggers have cases (edit, capture save, re-read).
  - No case covers a restore (P0 through DF E4), a Flag, an undo, or a display-gamut change under the cannot-show filter. That last one is timed in UJ9.5-a but its listing is never checked.
  - The "moves it" clause has no case at all: no change ever lands under a value sort.
  - Mutation that stays green: re-filter but don't re-sort on change. A capture save on ZX-010 under an L* sort leaves it in the empty-value tail.
- **R6.4:** the deselect triggers for search change, restore, Flag, re-read and display change have no case.
- **Fix:** add one case per trigger:
  - the no-value filter, then restore ZX-012 through DF E4 → it is unlisted and deselected;
  - the cannot-show filter on sRGB, ZX-002 selected, then move to P3 → ZX-002 is unlisted and deselected;
  - an L* sort, then a capture save on ZX-010 → it moves to its L* position (ZX-010's L* declared);
  - a search change with a selection → deselected.

**[MAJOR] R2-M3 — PRD:596 (R8.9), PRD:605 (R8.10c), J:428 (UJ9.6-a), J:195 (UJ2.1-k), J:475, C:295 — a mark's shape can't be read.**
- **What depends on it:** UJ9.6-a asserts "the eleven shapes… are pairwise distinct", and UJ2.1-k asserts each mark in E12 "with its shape".
- **The gap:**
  - R8.10c grants marks by identifier and an accessible description, but no shape.
  - The Mark labels table has no shape column.
  - The test-controls map cites R8.9, a behaviour row, as what grants the observable.
- **Mutation that stays green:** cannot-show and outside-sRGB drawn as the same symbol in two tints — exactly the colour dependence R8.9 forbids.
- **Fix:**
  - R8.10c reads each mark's rendered symbol identifier and its tint separately.
  - UJ9.6-a asserts the 11 identifiers are pairwise distinct with tint ignored.

**[MINOR] R2-m1 — J:275 (UJ4.4-f), CAP:161 (Capture R3.7) — the fallback case can't tell "first pending" from "next pending".**
- After ZX-010 is deleted, ZX-020 is the only pending row, so both rules pick it.
- Fix: declare a pending row ahead of ZX-010 in the queue (e.g. ZX-000 at position 1) and assert the session resumes at ZX-000.

**[MINOR] R2-m2 — J:418–419, PRD:592 (R8.5), PRD:543 — a re-read no longer has to deselect a removed item.**
- The m8 fix swapped one assertion for the other: the survivor now stays selected, but "deselecting any item the file no longer holds" is asserted nowhere.
- Fix: add a run in which the selected ZX-011 is removed outside the app, and assert nothing is selected afterwards.

**[MINOR] R2-m3 — J:433 (UJ9.7-d) — the restore crash moment isn't pinned.**
- A crash before the write starts satisfies "never part of a restore" on its own.
- Fix: crash after the new reading is written and before its six derived sets are.

**[MINOR] R2-m4 — J:429 (UJ9.6-b), PRD:596 — the keyboard re-runs still skip most states.**
- They cover only E2, E3, E14 and E17.
- Not covered: the actions of E8–E13, E15, E16, E18 and E19, the All items view, and range or toggle selection.
- Fix: cover "every state this PRD owns plus selection" in each phase.

**[MINOR] R2-m5 — J:412 (UJ9.1-b), PRD:590 — R8.3's "edits stay available" half runs in one, undeclared session sub-state.**
- Mutation that stays green: refuse edits while the session is guard-paused or halted.
- Fix: run UJ9.1-b "in each in-flight state".

**[MINOR] R2-m6 — PRD:448 (R2.11), J:238 — "sorts and comparisons use the stored value" is never told apart from using the displayed value.**
- No fixture has two values that display the same but are stored differently.
- Fix: add an item whose stored ΔE2000 from GA is about 3.0035 (GB at L* 48.4965). It displays 3.00 and must not be listed. Add an L* pair (50.04, 50.01) in reversed code order.

**[MINOR] R2-m7 — J:325 (UJ5.3-k) — F83's spread and verdict carry isn't tested apart from the device snapshot.**
- B2's spread and verdict are undeclared, and B1 is declared "agreed".
- Fix: declare B2 as spread 0.30, agreed, and B1 as disagreed with average accepted and spread 1.20. Then assert samples-disagreed and 1.20 after the restore.

**[MINOR] R2-m8 — J:425 (UJ9.5-c), PRD:278 — the above-ceiling case never tests value length.**
- It goes one column over IMPORTED_COLUMNS_CEILING, but value length is undeclared, so a 200-character truncation is never exercised.
- Fix: add values of 201 and 10,000 characters and read them back unchanged.

**[MINOR] R2-m9 — PRD:595 (R8.8), PRD:603 (R8.10a), J:471 — lost file permission has no injection input and no case.**
- Fix: add "file permission revoked" to R8.10a and the map. Add a case asserting the file is unchanged, and the state once the Data Foundation PRD names it.

**[MINOR] R2-m10 — PRD:612 (R8.11), PRD:582/585 (R8.1c/f), PRD:739 (M1), J:423, J:426 — timing coverage gaps.**
- R8.11 is P0, but its only case (UJ9.5-d) needs P1 bulk operations.
- R8.1f's bulk delete and "Use as scan order", and R8.1c's kinds other than capture saves, are never timed. M1's "per input kind" therefore quietly covers only the kinds UJ9.5-a delivers.
- Fix:
  - Add a P0 variant of UJ9.5-d: searches, sorts and single edits during 20 capture saves.
  - Time a bulk delete and a "Use as scan order" at ROWS_CEILING.

**[MINOR] R2-m11 — J:254, J:291, J:294 vs CAPC:47; C:289–290 vs PRD:604 — the round-1b cause-label change (F96) contradicts itself, and detail lines have no granted readback.**
- The cases read the set-aside cause "by its label in the capture PRD's copy file".
- CAPC:47 says "a test reads a row's cause through R11.11, never by its label", and R11.11 doesn't observe what the detail shows.
- C:289 claims detail and history lines are "read by the identifier", but R8.10b grants no readback keyed by line.
- Fix: R8.10b reads each R4.2 and R5.2 sub-row's fields, the cause by its capture-table identifier; reword the three cases.

**[MINOR] R2-m12 — J:51–54, DF:246, DF:329 — the gamut-margin rule depends on a reference that doesn't exist yet.**
- The margins must be "checked against the Data Foundation PRD's R7.5 reference values before a case relies on it".
- DF's R7.5 reference is unnamed (its OQ 6, spike-gated) and covers the stored sRGB flag, not P3 display membership. Read strictly, the P0 gamut cases and M2 wait on the hardware spike.
- Fix: check margins by a checked-in computation (CIE 15, Bradford, published primaries), cross-checked against R7.5 once it lands.

**[MINOR] R2-m13 — J:227 (UJ3.3-f), PRD:607 (R8.10e) — the app-storage read of "zx-01" can falsely fail.**
- If the file sits inside the app's container (sandboxing is open), its stored normalised codes ("zx-010") contain "zx-01", so a correct app fails — MJ7's collision again.
- Fix: declare the file outside the container as a named default, or leave the file out of R8.10e reads.

**[MINOR] R2-m14 — PRD:442 (R2.5), PRD:593 (R8.6), J:421 — no "file unchanged" check for actions that shouldn't write.**
- "No cannot-show mark is stored" has no case: persisting the mark stays green.
- UJ9.4-b's "no record of the filter or the sort" is not objective.
- Fix: after UJ2.1-d's display move and after UJ9.4-b's When, assert the file's bytes match the bytes before.

**[MINOR] R2-m15 — PRD:460 (R3.3) — F94's clause makes the row parse two ways.**
- In the All items view, it is unclear whether an item with the Data Foundation PRD's R3.3e reference mismatch, whose own illuminant/observer pair is the most common, follows the rest or sorts with that pair. No case covers it.
- Fix: split the sentence, and add an All-items case with such an item.

**[MINOR] R2-m16 — sibling halves: CAPJ:114 (Capture UJ3.3-k), CAP:297 (Capture R8.18).**
- UJ3.3-k declares D quarantined mid-session through DF R7.2, which grants seeded states only, and DF disables "Read it again" in flight. No path exists for the app to discover the damage.
- F84's read-only clause ("whatever quarantine DF reports… nothing written") has no case.

**[NIT] R2-n1 — J:55–57, J:380, J:383 — fixture naming and margins.**
- DL-001 and BI-1 (L*20 C*10 h°90) sit inside sRGB by only +0.016, against the Harness's "well inside" wording. No mark is read, so nothing breaks.
- "Daylight" names two different fixtures: UJ3.4-d's is outside sRGB, UJ7.1-i's inside. Rename one.

**[NIT] R2-n2 — J:331–333, J:358, J:362–363 — undeclared build-phase dependencies.**
- UJ6.4-f's Flag and drag runs, and UJ6.4-j/k's toggle-select plus "Set a field", depend on R4.9, R2.9, R6.1 and R6.2. Their Rows cells omit those, and the preamble decides phases by the Rows named.

## Biggest risks   (what could ship broken behind a green suite)
- **A chip that isn't the true colour on Display P3 (B1).** It renders from DF's stored clipped sRGB value while every mark and M2 read correct, on the default display of every recent Mac.
- **Deleted or cleared text surviving in the file (R2-M1).** A case-folded search index or normalised column keeps it, against F35, while every byte check passes.
- **Stale tables (R2-M2).** After a restore, Flag, undo or display move, or when a capture save lands under a value sort, items stay listed or selected.
- **Honesty marks told apart only by tint (R2-M3).**
- **Timing blind spots (R2-m10).** R8.11's capture budget is unguarded in the P0 build, and bulk deletes at scale are never timed.

## Genuinely solid   (incl. where minimal scoping is correct that a coverage-zealot would wrongly flag)
- **Oracles I recomputed, all correct:**
  - Every declared gamut membership, with Bradford: 23 values. The thinnest declared margin is GS-002 at +0.038.
  - UJ3.3-g's second h° order (the greys stay ascending under a descending sort), UJ3.3-h's 3.0 boundary, UJ3.3-i/j's ties and empty values under a reversed sort, UJ3.3-k.
  - UJ7.1-l: catches a tie broken by pair name or by the first sorted item. UJ7.1-i.
  - UJ3.4-f: exactly 3.0; queue order GB-2 then GB-1 isolates the tie by code.
  - UJ1.4-e (13, not 6), UJ6.2-h (exactly 10, no confirmation).
- **UJ2.1-n correctly doesn't assert ZX-003.** It is outside Rec. 2020 by only 0.003, and asserting it would be a flaky oracle.
- **The fixes' new cases discriminate well.**
  - UJ6.4-g/h turn A1's uniqueness scenarios into "history ended".
  - UJ6.4-j/k read bytes and app storage both while undo is live and after a crash.
  - UJ4.7-b isolates F78 with ZX-013.
  - UJ4.7-a and UJ4.7-f split E18's variant by phase.
  - Import's new rename case tells a reused column apart from an appended one.
  - UJ8.1-h runs narrowing crossed with sort mode.
- **The Harness's in-flight-runs rule** turns four sub-states into one declarative clause instead of copy-pasted cases.
- **Correctly minimal:** R4.7's R1.3, R4.4 and R4.8 undo refusals can't be reached — undo runs most-recent-first and any other committed action ends the history — so only R8.3's refusal needs a case (UJ6.4-i). Don't invent dead tests for the other three.

## Missing / over-tested
- **Missing:**
  - The P3 triplet value and a multi-reading chip value (B1).
  - A case-, normalisation- and encoding-aware byte check (R2-M1).
  - Per-trigger R3.9 and R6.4 cases, and a "moves under sort" case (R2-M2).
  - A mark-shape readback (R2-M3).
  - A discriminating fallback fixture (R2-m1).
  - A removed-selected re-read case (R2-m2).
  - A pinned restore crash moment (R2-m3).
  - A permission-loss injection path (R2-m9).
  - A P0 case for R8.11 (R2-m10).
  - "File unchanged" checks for actions that shouldn't write (R2-m14).
- **Over-tested:** nothing material. UJ9.5-a/b/d are release-cadence timing, with per-PR runs a tripwire, which is the right scope.

| Row ID | disposition |
|---|---|
| R1.1 | ALIGN |
| R1.2 | ALIGN |
| R1.3 | ALIGN |
| R1.4 | ALIGN |
| R1.5 | ALIGN |
| R1.6 | ALIGN |
| R1.7 | ALIGN |
| R1.8 | ALIGN |
| R1.9 | ALIGN |
| R1.10 | ALIGN |
| R2.1 | ALIGN |
| R2.2 | ALIGN |
| R2.3 | OBJECT (B1) |
| R2.4 | ALIGN |
| R2.4a | ALIGN |
| R2.4b | ALIGN |
| R2.4c | ALIGN |
| R2.4d | ALIGN |
| R2.4e | ALIGN |
| R2.4f | ALIGN |
| R2.4g | ALIGN |
| R2.4h | ALIGN |
| R2.4i | ALIGN |
| R2.5 | OBJECT (R2-m12, R2-m14) |
| R2.6 | ALIGN |
| R2.7 | ALIGN |
| R2.8 | OBJECT (R2-M3) |
| R2.9 | ALIGN |
| R2.10 | ALIGN |
| R2.11 | OBJECT (R2-m6) |
| R3.1 | ALIGN |
| R3.2 | ALIGN |
| R3.3 | OBJECT (R2-m15) |
| R3.4 | ALIGN |
| R3.5 | ALIGN |
| R3.6 | ALIGN |
| R3.7 | ALIGN |
| R3.8 | ALIGN |
| R3.9 | OBJECT (R2-M2) |
| R4.1 | ALIGN |
| R4.2 | OBJECT (R2-m11) |
| R4.2a | ALIGN |
| R4.2b | OBJECT (R2-m11) |
| R4.2c | ALIGN |
| R4.2d | ALIGN |
| R4.2e | ALIGN |
| R4.2f | ALIGN |
| R4.2g | ALIGN |
| R4.2h | ALIGN |
| R4.3 | ALIGN |
| R4.4 | ALIGN |
| R4.5 | OBJECT (R2-m1) |
| R4.6 | ALIGN |
| R4.7 | ALIGN |
| R4.8 | ALIGN |
| R4.9 | ALIGN |
| R5.1 | ALIGN |
| R5.2 | ALIGN |
| R5.2a | ALIGN |
| R5.2b | ALIGN |
| R5.2c | ALIGN |
| R5.2d | ALIGN |
| R5.2e | ALIGN |
| R5.3 | ALIGN |
| R5.4 | ALIGN |
| R5.5 | OBJECT (R2-m7) |
| R5.6 | ALIGN |
| R5.7 | ALIGN |
| R5.8 | ALIGN |
| R6.1 | ALIGN |
| R6.2 | ALIGN |
| R6.3 | ALIGN |
| R6.4 | OBJECT (R2-M2, R2-m2) |
| R7.1 | ALIGN |
| R7.2 | ALIGN |
| R8.1 | OBJECT (R2-m8, R2-m10) |
| R8.1a | ALIGN |
| R8.1b | ALIGN |
| R8.1c | OBJECT (R2-m10) |
| R8.1d | ALIGN |
| R8.1e | ALIGN |
| R8.1f | OBJECT (R2-m10) |
| R8.2 | ALIGN |
| R8.3 | OBJECT (R2-m5) |
| R8.4 | ALIGN |
| R8.5 | OBJECT (R2-m2) |
| R8.6 | OBJECT (R2-m13, R2-m14) |
| R8.7 | ALIGN |
| R8.8 | OBJECT (R2-m3, R2-m9) |
| R8.9 | OBJECT (R2-M3, R2-m4) |
| R8.10 | OBJECT (R2-M1, R2-M3, R2-m9, R2-m11) |
| R8.10a | OBJECT (R2-m9) |
| R8.10b | OBJECT (R2-m11) |
| R8.10c | OBJECT (R2-M3) |
| R8.10d | OBJECT (R2-M1) |
| R8.10e | OBJECT (R2-m13) |
| R8.10f | ALIGN |
| R8.11 | OBJECT (R2-m10) |
| E1 | ALIGN |
| E2 | ALIGN |
| E3 | ALIGN |
| E4 | ALIGN |
| E5 | ALIGN |
| E6 | ALIGN |
| E7 | ALIGN |
| E8 | ALIGN |
| E9 | ALIGN |
| E10 | ALIGN |
| E11 | ALIGN |
| E12 | OBJECT (R2-M3) |
| E13 | ALIGN |
| E14 | ALIGN |
| E15 | ALIGN |
| E16 | ALIGN |
| E17 | ALIGN |
| E18 | ALIGN |
| E19 | ALIGN |
| M1 | OBJECT (R2-m10) |
| M2 | OBJECT (B1, R2-m12) |
| M3 | ALIGN |
| M4 | ALIGN |

(R2-M1 is carried once, on R8.10 and R8.10d, where the Harness definition lands. The byte checks of R1.7, R4.3, R4.5, R4.7, R6.2 and R6.3 all depend on it. R2-m16 is sibling-only and objects to no Collection Mode row.)

#### peer-interface-reviewer (Claude route)

## Verdict
Contract sound: no Blocker, and every round-1 interface finding is resolved or knowingly deferred. The round-1 and round-1b fixes left one Major: R8.8 renders a permission-lost state that Data Foundation still owes, and no gate says so. They also left eleven Minors and two Nits, mostly about how tests read the display and seam bookkeeping.

## Surface & consumers (brief)
- **What I reviewed.** Subject commit 7861221: row IDs, named constants, the inherited-obligations tables and the copy states of `docs/product/collection-mode/prd-collection-mode.md`, together with its journeys, copy and oq-results companions.
- **Consumers.** Agent builders read rows and copy. Test harnesses key on copy-state and variant IDs, mark identifiers and action labels. The sibling PRDs cite these rows.
- **Delta.** I ran `git diff origin/main -- docs ':!docs/product/collection-mode' ':!docs/agent-reviews'`. It shows:
  - Capture F71: R1.1, R3.7, R5.6, R8.18, R11.15g, E1 and E30 amended, and a new Set-aside cause labels table in `prd-capture-mode-copy.md:45–57`.
  - Data Foundation F52: E8, E33, R1.2, R2.3, R2.3f, R6.2a, R7.2 and R7.6k.
  - Import F65, Export F31 and Device F32.
  - The ADR-0003 row in `docs/decisions/README.md` and two items in `post-lock.md`.
- **Gating.** Every sibling change carries a dated fence and a "peer review pending" status clause. I found no ungated break to a locked sibling contract.
- **Label check.** I ran it mechanically. Every double-quoted string in the PRD and the journeys resolves character for character to a copy label, and every copy label is quoted by the PRD and by at least one case.

**Round-1 delta verification**

| Round-1 finding | Status | Evidence |
|---|---|---|
| IF-1 | RESOLVED | copy:181, copy:217 ("Close"); PRD:445 (R2.8), PRD:465 (R3.8); journeys:195 (UJ2.1-k), journeys:234 (UJ3.4-b) |
| IF-2 | RESOLVED | PRD:500 (R4.7 own label, re-check and refusal, memory only, no redo); copy:124, copy:236 ("Undo change"); journeys:465, 467 (map); PRD:590 (R8.3); journeys:353–363. Residual ambiguity in R2-IF-5. |
| IF-3 | RESOLVED | journeys:465 (collection-surface line carries both actions); journeys:155–159, 165–167 fire them from the collection surface |
| IF-4 | RESOLVED | copy:292–307 (Mark labels); PRD:424, 461, 596 cite it; journeys:428 (identifiers); copy:309–347 (headers and line labels); capture copy:45–57 (causes). Residuals in R2-IF-2 and R2-IF-3. |
| IF-5 | RESOLVED | PRD:603–608 (R8.10a–f: offered and disabled actions, selection, search, filters, first on screen, E9, grid, token values); journeys:35–41; capture PRD:376 (R11.15g repointed) |
| IF-6 | RESOLVED | PRD:590 (R8.3 refuses both restores and E10's "Undo"); PRD:499, 527; capture PRD:213 (R5.6), :297 (R8.18); journeys:276, 281, 322 |
| IF-7 | RESOLVED (as scoped to Inbound) | PRD:644–669, every fence cite reads "its F…". The Outbound table has the same defect, in R2-IF-10. |
| IF-8 | RESOLVED | copy:186 |
| IF-9 | RESOLVED | PRD:461 (R3.4, F59); copy:124, copy:226 |
| IF-10 | PARTIAL | PRD:764 (OQ 10 records the selection scope). The Data Foundation side is unchanged: DF:218 (R6.3 "an item or collection delete") and DF:339 (OQ 20), while its own E33 promises ‹P1› undo. |
| IF-11 | RESOLVED | PRD:717–720 (⟨n⟩ defined per state, E5's basis included) |
| IF-12 | RESOLVED | PRD:654 |
| IF-13 | RESOLVED | PRD:541 (E8's "Cancel"); journeys:157, 265, 283 fire "Change the name" and "Try another code" |
| IF-14 | RESOLVED | PRD:569–570, PRD:623–624 (unquoted) |
| IF-15 | RESOLVED | PRD:138; journeys:119 (T15) |
| IF-16 | PARTIAL | capture PRD:91 fixed; `docs/product/README.md:18` still reads "queued", deferred to the bookkeeping close by the fix file's Out-of-scope list |
| IF-17 | RESOLVED | PRD:405 |
| IF-18 | RESOLVED | PRD:696 (E9 indexed to the item detail); copy:179 (headline names ⟨collection⟩) |

## Findings
[MAJOR] R2-IF-1 R8.8 (PRD:595): the lost-permission branch of a P0 error row names a state that does not exist, and nothing gates it.
- **The rule.** R8.8 says a write refused "because permission was lost [renders] the state that PRD owes".
- **The state does not exist.** Data Foundation's obligations line (DF:285) records it as "still owed", and `post-lock.md:41` sends its wording and actions back to the owner.
- **Nothing gates it.** Build dependencies row 1 (PRD:297) lists R8.8 under **Available contract**, with DF E10 and E15 only. Nothing under "What must remain open" is a Stop: or Interim: for this state.
- **No case.** UJ9.7-e and UJ9.7-f cover a vanished volume and a held file; nothing covers lost permission.
- **Effect.** A P0 builder must invent a state or render nothing. Rendering nothing is a silent failure: the field reverts and the user is not told the write was refused. This is also the "third option" that PRD:768–769 forbids.
- **Fix.** This does not reopen F73. Either:
  - add "Stop: the Data Foundation PRD's permission-lost state (its F52), for R8.8's lost-permission clause only" to row 1, or
  - have the owner name an interim state until Data Foundation supplies one.

  Add the case when the state exists.

[MINOR] R2-IF-2 UJ4.1-c (journeys:254), UJ4.7-a (journeys:291), R8.10b (PRD:604): the set-aside cause is asserted by its wording.
- **The cases.** Both cases assert the State line shows the cause "by its label in the capture PRD's copy file" (UJ4.1-c also asserts the words "still to deal with").
- **Two rules contradict them.** The new Capture table says "a test reads a row's cause through R11.11, never by its label" (capture copy:47). This PRD's Display labels say "Tests read these by the identifier … never by the words" (copy:289–290).
- **Neither route works.** R11.11 reads Capture's row, not what E14 displays. R8.10b grants no readback of a detail line's value by identifier.
- **Effect.** A harness either matches on wording, against R8.10, or never verifies what E14 shows.
- **Fix.**
  - Have R8.10b read each item-detail and history line's value by identifier: the row state, the cause by the Capture table's Cause column, and whether the item was deliberately left.
  - Reword both cases to "the cause Unreadable" and "the cause Flagged after capture", identified by that Cause column.

[MINOR] R2-IF-3 R2.8 (PRD:445) against E12 (copy:216): the two disagree on how E12 names the cannot-show mark.
- R2.8 says E12 shows each mark with "its chip label". For cannot-show that label is "Can't show" (copy:297).
- E12's Body instead writes "Can't show on this screen", which is the filter label. Every other mark's name in E12 matches its chip label.
- **Fix.** Either E12 reads "Can't show:", or R2.8 says E12 names each mark by its filter label. Copy or owner decides.

[MINOR] R2-IF-5 R4.7 (PRD:500): the set of actions that ends the undo history is not closed.
- **The rule.** R4.7 says "until any other committed action (a delete, a Flag, a restore, a reorder, a re-scan answer, an import or a re-read) ends that history".
- **Unclassified actions.** Four are neither listed nor excluded:
  - a session starting;
  - a capture save, on this collection or another;
  - a column hidden or shown, which R8.8 treats as a committed write;
  - "New collection".
- **Rows and cases that depend on the answer.**
  - R8.3's in-flight clause (PRD:590) for an "Undo change" that reverses a code change, and UJ6.4-i (journeys:361), both work only if a session start does not end the history.
  - E6's promise "do it again once the session has ended" (copy:151) holds only if capture saves do not end it either.
- **Effect.** Two defensible builds diverge. UJ6.4-f tests only the listed actions.
- **Fix.** Make the list exhaustive and state each of these four explicitly. Add a case for a column hide and for a capture save, each followed by listing the offered actions.

[MINOR] R2-IF-6 R3.9 (PRD:466) and R6.4 (PRD:543): the two rows list what changes an item differently.
- R3.9 lists "an edit, a capture save, a restore, a Flag, an undo or a re-read". R6.4 omits the undo.
- Both omit a re-scan answer. An answer clears re-scan-unanswered (T12), so under that filter the item must unlist and deselect.
- **Fix.** Write one list that both rows cite, adding the re-scan answer. Add a case: the re-scan-unanswered filter on, ZX-013 selected, the answer given from its detail.

[MINOR] R2-IF-7 E10's index (PRD:697), the Surfaces preamble (PRD:368) and the item-detail row (PRD:377) against R4.1 (PRD:478, F74): E10 is indexed to the wrong surfaces.
- R4.1 closes the detail on a delete and puts E10 "over" the table, so E10 never renders in the item detail.
- A delete from an item opened in the All items view (UJ7.1-g) puts E10 over the All items table. That surface is not indexed.
- **Fix.** Index E10 to the collection surface and the All items view, and update the preamble and both Surfaces rows.

[MINOR] R2-IF-8 E6 Body (copy:151): the text names P1 actions in a P0 state, with none of the phase marking F97 gave E18.
- In the first build E6 renders (R1.4, R4.5 and R4.6 are P0), yet it says "changing a swatch's code, flagging a swatch … and undoing a delete aren't available". Those are R4.4, R4.9 and R1.7, all P1.
- Under F57, "undoing a delete" may never ship in v1.
- **Fix.** Move those clauses into a `[phase: variant-absent]` variant enumerated by R8.3, as F97 did for E18, or word the body without naming the P1 actions.

[MINOR] R2-IF-9 Column headers (copy:309–321) against R4.8 (PRD:501) and the E11 "duplicate" variant (copy:205): the headers users see are not the names the duplicate rule checks.
- The new headers are "Code", "Name", "State", "Spread", "Alt. code", "Alt. name" and "Collection". R4.8 refuses only a name equal to another column's or "a Swatch field's name".
- So renaming Family to "Name" or "State" is accepted, and the table shows two columns with the same header. Import can also create such a column, because Import R2.6 appends passthrough headers.
- Meanwhile "Swatch Name" is refused, and E11 then points at a field the user sees as "Name".
- **Fix (owner).** Extend R4.8's duplicate set to the fixed headers in the Column headers table, or head the Swatch fields by their full names.

[MINOR] R2-IF-10 Outbound rows 2–3 (PRD:632–633): several fence cites do not name their document.
- The rows cite "their F70, F51 and F52" and "their F50 and F65". Capture also has F51 and F52, and Import has F50, so under the PRD's own rule (PRD:623–625) these misresolve. Adding F52 in round 1 widened the ambiguity.
- Row 4 (PRD:634) names no Data Foundation row for identity under a new code or for column visibility. Both live in DF R1.2 (DF:100).
- **Fix.** Write "the capture PRD's F70, the Data Foundation PRD's F51 and F52", "the Data Foundation PRD's F50, the import PRD's F65", and add DF R1.2 to row 4.

[MINOR] R2-IF-11 Inherited obligations: three one-sided mirrors remain.
- **Outbound Capture row (PRD:635).** It omits F96's Capture half. R4.2b now depends on the Capture copy file's Set-aside cause labels table.
- **Inbound, restore refusal.** Capture's obligations line (capture PRD:386) requires Collection Mode to refuse "a restore of a set-aside row while a session … is in flight (its R8.3, F71)". No Inbound line maps that obligation to R8.3.
- **Inbound R8.18 line (PRD:647).** It cites only "its F70", though R8.18 was amended under Capture F71: collection tallies only, the quarantine source, and the read-only case.
- **Fix.** Add the F96 clause with R4.2b to the Outbound Capture row, add the restore-refusal Inbound line to R8.3, and cite "its F70 and F71".

[MINOR] R2-IF-12 Capture R5.8 (capture PRD:215) and R6.6 (capture PRD:233), a sibling half: these rows quote a cause label differently from the new table.
- Both quote the cause as "flagged: missing or damaged". The new Capture table, which calls itself "the one place those labels are written", says "flagged as missing or damaged", which is R8.2's wording.
- The fix pass reported this but did not fix it.
- **Fix.** Align R5.8 and R6.6 under a dated line on Capture F71 (editorial).

[NIT] R2-IF-13 Capture Traceability (capture PRD:116) still reads "Owner decisions F1–F70", though Capture F71 now exists. Change it to F1–F71.

[NIT] R2-IF-14 The Row transitions restore line (PRD:141) names only R5.5. Data Foundation E4's restore through R4.6 (UJ4.5-c) is the same route, so add R4.6 to that line.

## Biggest risks   (what existing consumers/scripts/agents break)
- **A P0 builder hits R8.8's lost-permission branch** with no state and no Stop, and fills the gap by guessing or by failing silently (R2-IF-1).
- **Harness authors split on how to read a set-aside cause.** Some follow these cases and match wording; others follow Capture's copy rule and never check what E14 shows (R2-IF-2).
- **Metadata-undo availability can differ between two correct builds**, depending on whether a session start or a capture save ends the history. UJ6.4-i can then fail on a defensible build (R2-IF-5).
- **R1.7's "swatches" variant rests on a Data Foundation undo that does not grant selection scope.** DF R6.3 and OQ 20 still read item or collection only, while DF E33 promises ‹P1› undo (IF-10, partial).

## Genuinely well-designed   (incl. where a deliberate inconsistency is correct that a style-checker would wrongly flag)
- **Labels are tight.** Checked mechanically, there are zero unresolved quoted strings in the PRD and journeys and zero unused copy labels.
  - "Close" removes the collision with Capture's "Done" cleanly.
  - "Undo change" and E10's "Undo" are now distinct, each with its own refusal model.
- **R8.10 as a lettered seam (a–f)** now grants what the cases assert: offered and disabled actions, the selection, search text, filters, first on screen, E9, the grid, token values, the file's bytes and the app's own storage.
- **Mark identifiers first, wording in one table.** The Mark labels table keeps the P0 honesty path free of wording matches (UJ9.6-a).
- **The set-aside causes got one home, in the document that owns them** (Capture). That is the right ownership; only the reading method conflicts (R2-IF-2).
- **Two justified deviations a style checker would flag.**
  - E18's "restore" variant carries `[phase: variant-absent]`: the variant mechanism is used for a build phase. That is exactly what the phase rule's third kind exists for.
  - E10's prose note declines to invent a fourth phase kind. That is correct restraint.
- **Sibling halves.** Each landed with a dated fence and a status clause (Capture F71, DF F52, Import F65, Export F31, Device F32). The ADR-0003 row takes the five inputs with the right fences: F85, not F73, for no stored cannot-show mark.
- **Build dependencies.** The table is back to the format's three columns, with Stop: and Interim: prefixes (F91).

## Missing / over-engineered
- **Missing:**
  - a Stop or interim for R8.8's owed state;
  - a closed list of the actions that end R4.7's history;
  - readback by identifier for item-detail and history line values;
  - the Data Foundation note that its R6.3 and OQ 20 must cover E33's selection scope (IF-10);
  - the one-sided mirror lines in R2-IF-11.
- **Over-engineered:** nothing material. The bookkeeping (fence maps, dated clarifications, Stop:/Interim: labels) is heavy but proportionate for agent builders.

| Row ID | disposition |
|---|---|
| R1.1 | ALIGN |
| R1.2 | ALIGN |
| R1.3 | ALIGN |
| R1.4 | ALIGN |
| R1.5 | ALIGN |
| R1.6 | ALIGN |
| R1.7 | ALIGN |
| R1.8 | ALIGN |
| R1.9 | ALIGN |
| R1.10 | ALIGN |
| R2.1 | ALIGN |
| R2.2 | ALIGN |
| R2.3 | ALIGN |
| R2.4 | ALIGN |
| R2.4a | ALIGN |
| R2.4b | ALIGN |
| R2.4c | ALIGN |
| R2.4d | ALIGN |
| R2.4e | ALIGN |
| R2.4f | ALIGN |
| R2.4g | ALIGN |
| R2.4h | ALIGN |
| R2.4i | ALIGN |
| R2.5 | ALIGN |
| R2.6 | ALIGN |
| R2.7 | ALIGN |
| R2.8 | OBJECT (R2-IF-3) |
| R2.9 | ALIGN |
| R2.10 | ALIGN |
| R2.11 | ALIGN |
| R3.1 | ALIGN |
| R3.2 | ALIGN |
| R3.3 | ALIGN |
| R3.4 | ALIGN |
| R3.5 | ALIGN |
| R3.6 | ALIGN |
| R3.7 | ALIGN |
| R3.8 | ALIGN |
| R3.9 | OBJECT (R2-IF-6) |
| R4.1 | ALIGN |
| R4.2 | OBJECT (R2-IF-2) |
| R4.2a | ALIGN |
| R4.2b | OBJECT (R2-IF-2) |
| R4.2c | ALIGN |
| R4.2d | ALIGN |
| R4.2e | ALIGN |
| R4.2f | ALIGN |
| R4.2g | ALIGN |
| R4.2h | ALIGN |
| R4.3 | ALIGN |
| R4.4 | ALIGN |
| R4.5 | ALIGN |
| R4.6 | ALIGN |
| R4.7 | OBJECT (R2-IF-5) |
| R4.8 | OBJECT (R2-IF-9) |
| R4.9 | ALIGN |
| R5.1 | ALIGN |
| R5.2 | ALIGN |
| R5.2a | ALIGN |
| R5.2b | ALIGN |
| R5.2c | ALIGN |
| R5.2d | ALIGN |
| R5.2e | ALIGN |
| R5.3 | ALIGN |
| R5.4 | ALIGN |
| R5.5 | ALIGN |
| R5.6 | ALIGN |
| R5.7 | ALIGN |
| R5.8 | ALIGN |
| R6.1 | ALIGN |
| R6.2 | ALIGN |
| R6.3 | ALIGN |
| R6.4 | OBJECT (R2-IF-6) |
| R7.1 | ALIGN |
| R7.2 | ALIGN |
| R8.1 | ABSTAIN |
| R8.1a | ABSTAIN |
| R8.1b | ABSTAIN |
| R8.1c | ABSTAIN |
| R8.1d | ABSTAIN |
| R8.1e | ABSTAIN |
| R8.1f | ABSTAIN |
| R8.2 | ABSTAIN |
| R8.3 | ALIGN |
| R8.4 | ALIGN |
| R8.5 | ALIGN |
| R8.6 | ALIGN |
| R8.7 | ALIGN |
| R8.8 | OBJECT (R2-IF-1) |
| R8.9 | ALIGN |
| R8.10 | OBJECT (R2-IF-2) |
| R8.10a | ALIGN |
| R8.10b | OBJECT (R2-IF-2) |
| R8.10c | ALIGN |
| R8.10d | ALIGN |
| R8.10e | ALIGN |
| R8.10f | ALIGN |
| R8.11 | ABSTAIN |
| E1 | ALIGN |
| E2 | ALIGN |
| E3 | ALIGN |
| E4 | ALIGN |
| E5 | ALIGN |
| E6 | OBJECT (R2-IF-8) |
| E7 | ALIGN |
| E8 | ALIGN |
| E9 | ALIGN |
| E10 | OBJECT (R2-IF-7) |
| E11 | OBJECT (R2-IF-9) |
| E12 | OBJECT (R2-IF-3) |
| E13 | ALIGN |
| E14 | ALIGN |
| E15 | ALIGN |
| E16 | ALIGN |
| E17 | ALIGN |
| E18 | ALIGN |
| E19 | ALIGN |
| M1 | ABSTAIN |
| M2 | ALIGN |
| M3 | ALIGN |
| M4 | ALIGN |

R2-IF-10 and R2-IF-11 are defects in the inherited-obligations tables, R2-IF-14 is in the Row transitions table, and R2-IF-12 and R2-IF-13 sit in the Capture PRD. None of these five objects to a Collection Mode row.

#### peer-privacy-reviewer (Claude route)

## Verdict
**Privacy-sound to ship.** No finding is Critical or High. The round-1 fixes closed all nine round-1 privacy findings as they were specified. No new or amended row, in this PRD or its sibling documents, adds a network path or moves personal data out of the user's file. One Medium gap should still close before lock: the P0 edit and delete cases check for leftover text only after a clean close.

## Data-flow & PII map (brief)
- **Who the data is about.** The user (the operator). Also anyone the user names in an imported free-text column (F8) or in a collection name.
- **Personal fields these surfaces touch:**
  - the device serial and firmware (R4.2d, R5.2b). A restore now copies its source reading's device snapshot (R5.5; the Data Foundation PRD's R2.3f under its F52).
  - measurement and record times (R5.2a).
  - Swatch Name, the alternates, imported values, and collection and column names. They are edited by R1.3, R4.3, R4.8 and R6.2, and deleted by R1.4, R4.5 and R6.3.
  - search text (R3.1). In the All items view, R1.10 now also matches collection names and imported values.
  - per-collection column visibility (R2.10).
  - Nothing is special-category.
- **Where it is stored.** Everything is in the user's one SQLite file (the Data Foundation PRD's R1.1). Column visibility is the one view choice the file keeps. Search, filters, sorts, the grid choices (R7.1, R7.2) and the metadata undo history (R4.7) live only in memory (R8.6).
- **How data leaves.** Only through an export the user starts (R1.8). The Data Foundation PRD's E33 now says its "Export first" saves the whole collection. There is no network path. The unwritten Telemetry PRD is handed a content limit (Outbound table, F52).
- **The user's rights.** Deleted, cleared or replaced text is to be gone from the file's bytes (F35; the Data Foundation PRD's R6.2a). Correction happens in place. History is kept for good (settled).

## Findings

**Round-1 delta (commit 7861221)**

| Round-1 ID | Status | Evidence |
|---|---|---|
| PRIV-1 | RESOLVED, as remediated | R8.10d reads the bytes of the file and of what sits beside it: /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode.md:606. The Data Foundation PRD's R6.2a and R2.3 now cover the bytes: /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/data-foundation/prd-data-foundation.md:229. Residue asserts: /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode-journeys.md:105 (T1), :110 (T6, renamed Harbour), :271 (UJ4.4-b), :342 (UJ6.2-c), :349 (UJ6.3-b). A gap my round-1 remediation left open is raised as PRIV-10. |
| PRIV-2 | RESOLVED | R4.7 holds a replaced value "only in the running app's memory, never in the file or anywhere outside it": /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode.md:500. /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode-journeys.md:362 (UJ6.4-j reads while undo is still offered) and :363 (UJ6.4-k reads after a crash, before reopening). |
| PRIV-3 | RESOLVED | The named default is now network reachable: /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode-journeys.md:94. UJ9.4-c drives every action (:422), and UJ 9's preamble re-runs it in every phase (:406-407). A detail of the new case is raised as PRIV-13. |
| PRIV-4 | RESOLVED | /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode-journeys.md:108 (T4 reads the bytes before reopening), :169 (UJ1.3-e), :170 (UJ1.3-f, the crash). OQ 10's restatement clause: /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode.md:764. |
| PRIV-5 | RESOLVED | R8.10e: /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode.md:607. /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode-journeys.md:227 (UJ3.3-f), :421 (UJ9.4-b now searches qqq), :449 (UJ10.1-d). |
| PRIV-6 | RESOLVED | /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode.md:593 (R8.6's telemetry clause) and :636 (the Outbound Telemetry line, F52). |
| PRIV-7 | RESOLVED | The Data Foundation PRD's E33 says "Export first saves all of ⟨collection⟩": /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/data-foundation/prd-data-foundation-copy.md:27. |
| PRIV-8 | RESOLVED (no change needed) | UJ2.2-b asserts the file still holds the hidden Family column and its values, and no copy calls hiding a redaction: /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode-journeys.md:201. |
| PRIV-9 | RESOLVED (settled) | E17 discloses it: /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode-copy.md:264. The Data Foundation PRD's R6.1: /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/data-foundation/prd-data-foundation.md:216. |

**New findings**

**[MEDIUM] PRIV-10 — The P0 edit and delete cases check for leftover text only after a clean close**
- **Where:**
  - R4.3, R4.5, R1.4 and R6.3: /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode.md:496, :498, :406, :542
  - the cases: /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode-journeys.md:105 (T1), :110 (T6), :167 (UJ1.3-c), :259 (UJ4.2-a), :261 (UJ4.2-c), :271 (UJ4.4-b), :349 (UJ6.3-b)
- **The promise.** R4.3 promises that an edit leaves "the text it replaces nowhere in the file's bytes". There is no time limit on that promise, and F13 (fences:305-312) says replaced values are "never kept where an outside reader could recover them". The Data Foundation PRD's R1.5 (/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/data-foundation/prd-data-foundation.md:105) tells users that reading the file elsewhere while the app is open is safe.
- **What the cases check.** R8.10d (prd:606) now allows the bytes to be read at three moments: app closed, app open, and after a crash before reopening.
  - Only the P1 cases use the open and after-crash moments: UJ6.4-j/k, UJ1.3-f and T4.
  - T1 and UJ6.3-b read with the app closed.
  - T6, UJ4.2-a/c and UJ4.4-b name no moment.
  - UJ1.3-c, the P0 collection delete, reads no bytes at all. If F57 defers R1.7, UJ1.3-e/f never run in v1, so no case here ever reads the bytes after a collection delete. The Data Foundation PRD's DJ4 reads them at an unnamed moment.
  - All of these cases read "its bytes", the main file only, never what sits beside it.
- **Mutation that passes every P0 case.** Build with SQLite in WAL mode at the default auto-checkpoint, with no checkpoint after a write that replaces or deletes text.
  - A clean close checkpoints and removes the -wal file, so T6, UJ4.2-a/c, T1, UJ4.4-b and UJ1.3-c all pass.
  - Meanwhile the main file keeps the old page holding "Sky Blue" or "Warm Grey 1" for the rest of the open session. After a crash it keeps that page until the next open.
  - A persistent -journal or -wal file beside the main file escapes too, because the P0 asserts read only the main file.
- **Who is affected.** The user, and any third party named in an imported column. A copy of the file taken mid-session or after a crash, or picked up by a sync folder, still discloses the text the user was told was gone.
- **Remediation (testability only; R4.3 already states the rule).**
  - Mirror UJ6.4-j/k for the single-field edit and clear (UJ4.2-a, UJ4.2-c) and for the item delete (T1, UJ4.4-b): with the app open after the write lands, and again after a crash before reopening, read the bytes of the file and of what sits beside it (R8.10d). "Sky Blue", "Azure" and "Warm Grey 1" appear nowhere.
  - Give UJ1.3-c and UJ6.3-b the same three reads, with "Teal" and "Plum" appearing nowhere.
  - Optionally, R4.3 cites R8.10d's three moments.
- (GDPR Art. 17; Art. 5(1)(e); Art. 25; this PRD's F13 and F35)

**[LOW] PRIV-11 — Renaming a collection has no byte rule and no case**
- **Where:**
  - R1.3: /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode.md:405
  - R4.8: prd:501
  - the Outbound Data Foundation line: prd:634
  - F35's Carried by: /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode-fences.md:527
  - T5 and UJ1.1-a: /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode-journeys.md:109, :155
- **What changed.** The fix pass wrote F35's byte clause into R4.3 and R6.2 only.
- **The gap.**
  - F35's title covers all "replaced text", but a rename (R1.3) is not in its Carried-by list.
  - T5 and UJ1.1-a assert only the new name.
  - A collection name can name a person (a client's order). The old name could survive in freed pages, in un-checkpointed pages, or in a denormalised copy. For example, an activity record might carry the collection name from session time; that record is in the Data Foundation PRD's R6.5 privacy inventory. Whether ADR-0003 denormalises the name is unverified.
- **Remediation:**
  - R1.3 carries R4.3's clause: the name it replaces is left nowhere in the file's bytes.
  - T5 asserts that the bytes hold the text "Gouache Set" nowhere. It is not a substring of "Gouache Travel Set", so a correct build passes.
  - Column names are rarely personal. R4.8 can take the same clause optionally.
- (Art. 5(1)(e); F35)

**[LOW] PRIV-12 — The readback of the app's own storage assumes a sandbox and misses copies the system keeps**
- **Where:**
  - R8.10e: /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode.md:607 ("its preferences, its container and its saved window state")
  - R8.6: prd:593 ("never written to the file or anywhere outside it")
  - the Test-controls map: /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode-journeys.md:472
- **Gap 1: "container" is undefined.** A container exists only under the sandboxed reading, and sandboxing is an open decision (AGENTS.md §3). Unsandboxed, the app writes to Application Support, Caches, Preferences, Saved Application State and the per-user temporary directory, and a test author has to guess which of these to read.
- **Gap 2: copies the system keeps on the app's behalf are read nowhere:**
  - unified-log entries (NSLog messages are not redacted; neither is os_log `.public` interpolation);
  - document versions under /.DocumentRevisions-V100, if the shell adopts NSDocument/DocumentGroup autosave-in-place. That is SwiftUI's idiomatic document shape, and no ADR has ruled it out;
  - crash-report messages;
  - CoreSpotlight or Handoff (NSUserActivity) donations.
  - Search text, names or cleared values in any of these pass UJ3.3-f, UJ9.4-b, UJ6.4-j/k and UJ10.1-d.
- **Uncertainty.** None of these is known to be planned. By default os_log redacts dynamic strings. The exposure is local to the same account, or reaches others through a sysdiagnose the user sends.
- **Remediation:**
  - Define the app's own storage by location for both sandbox readings.
  - Add the run's unified-log entries for the app, and any system-kept versions of the file, to R8.10e.
  - Optionally, R8.6 states what the Data Foundation PRD's R1.1 and F53 already imply: no system-kept document versions of the file, and no donation of collection content to system search or Handoff.
- (Art. 5(1)(c)/(e); the Data Foundation PRD's R1.1)

**[LOW] PRIV-13 — UJ9.4-c's content check can't be read, and its configuration is undeclared**
- **Where:**
  - UJ9.4-c: /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode-journeys.md:422
  - the device PRD's R6.17: /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/device-management/prd-device-management.md:238
  - the Data Foundation PRD's R1.4: /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/data-foundation/prd-data-foundation.md:103
- **Problem 1: the content check can't be read.** UJ9.4-c asserts that no attempt carries "text a case typed". R6.17 records only a request's destination, time and "payload shape", so a test cannot read payload content.
- **Problem 2: the check is narrower than R8.6.** R8.6 also forbids codes, names and values. Seeded values that the case never types are not checked.
- **Problem 3: the configuration is undeclared.** The Given does not say whether the Demo or the live configuration runs, and R1.4's permitted set depends on it: live adds vendor authorization and SDK analytics.
- **Remediation:**
  - Declare the Demo configuration. With telemetry and update checks off, R1.4's permitted set is then empty, so assert that R6.17 records no outbound attempt at all.
  - If a live run is kept, assert that every attempt's destination is in the permitted set and that its payload shape has no field holding a seeded code, name, imported value, collection name or column name.

**[INFO] PRIV-14 — R4.7's cite covers only half its claim.** R4.7 (prd:500) cites the Data Foundation PRD's R6.2d for "never in the file or anywhere outside it". R6.2d (/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/data-foundation/prd-data-foundation.md:232) covers only the active file. The outside-the-file half comes from that PRD's R1.1 (:97) and this PRD's F53. Add both cites.

**[INFO] PRIV-15 — Transparency: the byte rule is honestly limited to the active file.** The Data Foundation PRD says at :234 that copies and pre-upgrade snapshots keep what they held. The help docs, which that PRD's R1.5 already points to, could also say that backups, Time Machine or APFS local snapshots, and a sync provider's version history made before a delete or clear keep what was there.

**[INFO] PRIV-16 — Keep the verification seam out of shipped builds, or local to the test runner.** R8.10b/c let a test read search text, the selection and rendered values, and M1 and UJ9.5-a read R8.10f in a Release build (prd:739; journeys:423). R8.6 covers outbound requests, not an inbound channel. Say that the readbacks other than timing exist only in test builds, or that no build opens a listening socket or cross-process service for them.

## Biggest privacy risks
1. **Leftover text in the open or crashed file (PRIV-10).** For P0 edits and deletes, text the user was told is gone stays in the main file bytes for the whole open session and after a crash, and every P0 case still passes. The file is meant to be read and shared while open.
2. **Copies outside the checked locations (PRIV-12).** Personal text could sit in the unified log or in document versions the system keeps, outside anything the storage readback checks. R8.6 promises "anywhere outside it".
3. **Renamed collection names (PRIV-11).** A collection's old name has no byte rule and no case.

## Genuinely privacy-respecting
- **The byte rule is built into the design.** F35, R4.3 and R6.2 set it, and it is handed to ADR-0003 as a schema input: /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/decisions/README.md:22. A checklist might say secure deletion on APFS cannot be promised. The promise is limited to the active file's bytes (the Data Foundation PRD's R6.2a and :234; E8 says "your file"), so it does not overclaim.
- **Undo keeps nothing on disk.** R4.7 undoes metadata from memory only and offers no redo. UJ6.4-j reads the file while undo is live, and UJ6.4-k reads it after a crash. That makes both halves of E8's "What was there isn't kept in your file; you can undo this while the file stays open" (copy:170, :172) true.
- **Grid choices are not remembered (R7.1, R7.2, F71).** Grid/Table and swatch size are "written nowhere". That is correct minimisation, not a missing convenience.
- **A restore keeps its source's device snapshot (R5.5; the Data Foundation PRD's R2.3f).** A checklist might call copying a serial over-collection. It is provenance accuracy (Art. 5(1)(d)): stamping the current device instead would fabricate the record. The copy lives and dies with its item, so erasure is unchanged.
- **Search across the whole file stays in memory (R1.10).** All items search over imported values and collection names runs over the user's own file, and no index or history is persisted (R8.6).
- **Tests now run online.** Network reachable is the named default, so every case runs in the realistic state, and UJ9.4-a stays as the offline case.
- **F57 favours privacy.** If the Data Foundation PRD's OQ 20 is still open at v1 release, deletes ship final and no pending deleted content is kept in the file.
- **The sibling halves add no disclosure.** None of them adds a flow:
  - the import PRD's F65 keeps no alias of a column's old name (no shadow copy);
  - the capture PRD's cause labels, the export header notes and the device PRD's E22 note add no data flow;
  - the ADR-0003 row gains the byte-level input.
- **Every new fixture is synthetic**, serials included.

## Missing controls / over-collection
- Reads with the app open and after a crash, including what sits beside the file, on the P0 edit, clear and delete cases and on UJ6.3-b (PRIV-10).
- A byte clause on R1.3 and a residue assert on T5 (PRIV-11).
- A definition of the app's own storage by location, plus the unified log and system-kept document versions (PRIV-12).
- A declared configuration and a zero-attempt assert for UJ9.4-c (PRIV-13).
- **No over-collection.** The new rows (R2.11, R3.9, R6.4, R8.1a–f, R8.10a–f, R8.11, E18, E19, M4) add no personal field. M4 reads a count only. Column visibility is still the one view choice the file keeps.

| Row ID | disposition |
|---|---|
| R1.1 | ABSTAIN (out of lens) |
| R1.2 | ABSTAIN (out of lens) |
| R1.3 | OBJECT (PRIV-11) |
| R1.4 | OBJECT (PRIV-10) |
| R1.5 | ABSTAIN (out of lens) |
| R1.6 | ALIGN |
| R1.7 | ALIGN |
| R1.8 | ALIGN |
| R1.9 | ALIGN |
| R1.10 | ALIGN |
| R2.1 | ABSTAIN (out of lens) |
| R2.2 | ABSTAIN (out of lens) |
| R2.3 | ABSTAIN (out of lens) |
| R2.4 | ABSTAIN (out of lens) |
| R2.4a | ABSTAIN (out of lens) |
| R2.4b | ABSTAIN (out of lens) |
| R2.4c | ABSTAIN (out of lens) |
| R2.4d | ABSTAIN (out of lens) |
| R2.4e | ABSTAIN (out of lens) |
| R2.4f | ABSTAIN (out of lens) |
| R2.4g | ABSTAIN (out of lens) |
| R2.4h | ABSTAIN (out of lens) |
| R2.4i | ABSTAIN (out of lens) |
| R2.5 | ABSTAIN (out of lens) |
| R2.6 | ABSTAIN (out of lens) |
| R2.7 | ABSTAIN (out of lens) |
| R2.8 | ABSTAIN (out of lens) |
| R2.9 | ABSTAIN (out of lens) |
| R2.10 | ALIGN |
| R2.11 | ABSTAIN (out of lens) |
| R3.1 | ALIGN |
| R3.2 | ABSTAIN (out of lens) |
| R3.3 | ABSTAIN (out of lens) |
| R3.4 | ABSTAIN (out of lens) |
| R3.5 | ABSTAIN (out of lens) |
| R3.6 | ALIGN |
| R3.7 | ALIGN |
| R3.8 | ABSTAIN (out of lens) |
| R3.9 | ABSTAIN (out of lens) |
| R4.1 | ABSTAIN (out of lens) |
| R4.2 | ALIGN |
| R4.2a | ALIGN |
| R4.2b | ABSTAIN (out of lens) |
| R4.2c | ABSTAIN (out of lens) |
| R4.2d | ALIGN |
| R4.2e | ABSTAIN (out of lens) |
| R4.2f | ABSTAIN (out of lens) |
| R4.2g | ABSTAIN (out of lens) |
| R4.2h | ABSTAIN (out of lens) |
| R4.3 | OBJECT (PRIV-10) |
| R4.4 | ALIGN |
| R4.5 | OBJECT (PRIV-10) |
| R4.6 | ABSTAIN (out of lens) |
| R4.7 | ALIGN |
| R4.8 | ALIGN |
| R4.9 | ABSTAIN (out of lens) |
| R5.1 | ABSTAIN (out of lens) |
| R5.2 | ABSTAIN (out of lens) |
| R5.2a | ABSTAIN (out of lens) |
| R5.2b | ALIGN |
| R5.2c | ABSTAIN (out of lens) |
| R5.2d | ABSTAIN (out of lens) |
| R5.2e | ABSTAIN (out of lens) |
| R5.3 | ABSTAIN (out of lens) |
| R5.4 | ABSTAIN (out of lens) |
| R5.5 | ALIGN |
| R5.6 | ALIGN |
| R5.7 | ABSTAIN (out of lens) |
| R5.8 | ABSTAIN (out of lens) |
| R6.1 | ABSTAIN (out of lens) |
| R6.2 | ALIGN |
| R6.3 | OBJECT (PRIV-10) |
| R6.4 | ABSTAIN (out of lens) |
| R7.1 | ALIGN |
| R7.2 | ALIGN |
| R8.1 | ABSTAIN (out of lens) |
| R8.1a | ABSTAIN (out of lens) |
| R8.1b | ABSTAIN (out of lens) |
| R8.1c | ABSTAIN (out of lens) |
| R8.1d | ABSTAIN (out of lens) |
| R8.1e | ABSTAIN (out of lens) |
| R8.1f | ABSTAIN (out of lens) |
| R8.2 | ALIGN |
| R8.3 | ABSTAIN (out of lens) |
| R8.4 | ALIGN |
| R8.5 | ALIGN |
| R8.6 | OBJECT (PRIV-12, PRIV-13) |
| R8.7 | ABSTAIN (out of lens) |
| R8.8 | ALIGN |
| R8.9 | ABSTAIN (out of lens) |
| R8.10 | OBJECT (PRIV-12) |
| R8.10a | ALIGN |
| R8.10b | ALIGN |
| R8.10c | ABSTAIN (out of lens) |
| R8.10d | ALIGN |
| R8.10e | OBJECT (PRIV-12) |
| R8.10f | ABSTAIN (out of lens) |
| R8.11 | ABSTAIN (out of lens) |
| E1 | ABSTAIN (out of lens) |
| E2 | ABSTAIN (out of lens) |
| E3 | ABSTAIN (out of lens) |
| E4 | ABSTAIN (out of lens) |
| E5 | ABSTAIN (out of lens) |
| E6 | ABSTAIN (out of lens) |
| E7 | ABSTAIN (out of lens) |
| E8 | ALIGN |
| E9 | ALIGN |
| E10 | ALIGN |
| E11 | ABSTAIN (out of lens) |
| E12 | ABSTAIN (out of lens) |
| E13 | ABSTAIN (out of lens) |
| E14 | ABSTAIN (out of lens) |
| E15 | ABSTAIN (out of lens) |
| E16 | ABSTAIN (out of lens) |
| E17 | ALIGN |
| E18 | ABSTAIN (out of lens) |
| E19 | ABSTAIN (out of lens) |
| M1 | ABSTAIN (out of lens) |
| M2 | ABSTAIN (out of lens) |
| M3 | ABSTAIN (out of lens) |
| M4 | ALIGN |

#### peer-product-marketing-manager-reviewer (Claude route)

## Verdict
Lands after fixing Blockers. Every round-1 finding in my lens is resolved or partly resolved, but the round-1 fixes added two new false statements: the honesty legend says a correctly rendered chip "isn't the true colour", and E8 promises an undo that R4.7 no longer delivers. Each is a one-line wording fix.

## Audience & message context (brief)
The reader is the Cataloger, a non-expert who has scanned a physical collection and is now browsing and fixing it in the app. After reading the copy they should be able to say which colours on screen are true, which numbers they can trust, and what each action will do to their data. The surface is in-app UI copy: E1–E19, the Display labels tables, and the sibling states this change adds or amends (Data Foundation E8 and E33, Capture E1 and E30, and Capture's new Set-aside cause labels table). Plain, calm UI copy is the right register; no launch narrative is needed.

I read all four target files, the fence file (F1–F98), the round-1 review log, the round-1 and round-1b fix file, the journeys cases that exercise the copy, and `git diff origin/main` of the sibling halves.

## Findings

**Path key.** 
- copy = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode-copy.md
- prd = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode.md
- journeys = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode-journeys.md
- DF copy = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/data-foundation/prd-data-foundation-copy.md
- Capture copy = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/capture-mode/prd-capture-mode-copy.md

**Round-1 delta** (IDs as the round-1 fix file numbers them)

| Round-1 finding | Result | Evidence |
|---|---|---|
| PMM Blocker — "Outside sRGB" misstated what the file and exports hold | RESOLVED | copy:216. The definition is now true: sRGB and HSL are nearest-sRGB; Lab, XYZ and spectral values are unaffected. See N-B1 for a new defect in its group heading. |
| PMM Major 1 — "Value missing" vs "No current value" | RESOLVED | copy:216, copy:302-303; prd:436-437 |
| PMM Major 2 — "Re-scan to answer" read as an order | RESOLVED | copy:216, copy:305; prd:438 |
| PMM Major 3 — "not settled" meant two things on neighbouring screens | RESOLVED in this PRD | prd:486; copy:307, copy:329. See N-m1 for the sibling half. |
| PMM Major 4 — legend had no shapes and couldn't be reached from detail or history | RESOLVED | prd:445, prd:699; copy:209-210, copy:236, copy:265; journeys:195, journeys:199 |
| PMM Major 5 — Flag gave no word of its consequence | RESOLVED | copy:268-276 (E18); prd:502 |
| PMM Major 6 — Rename column gave no import warning | RESOLVED | copy:278-285 (E19); prd:501 |
| PMM Minor 1 — E6 read as a queue; "held" | RESOLVED | copy:151 |
| PMM Minor 2 — "Spacing and capitals" overclaimed | RESOLVED | copy:133, copy:205, copy:247; Capture copy:12 (E1), Capture copy:30 (E30). Checked against Import R2.3: trimming, collapsing spaces and case-folding, so the claim is now true. |
| PMM Minor 3 — E5's ⟨n⟩ with a search active | RESOLVED | prd:718-719 |
| PMM Minor 4 — E8 never mentioned undo | PARTIAL | copy:170, copy:172. The sentence was added, but it now promises more than F29's R4.7 delivers (N-B2). |
| PMM Minor 5 — E14 false on a read-only file | RESOLVED | copy:237; prd:591 |
| PMM Minor 6 — displayed text with no copy (Spread, mismatch line, not-compared distance) | RESOLVED | copy:216, copy:332, copy:347 |
| PMM Minor 7 — "Unreadable" definition | RESOLVED | copy:216 |
| PMM Minor 8 — DF E33 didn't state the export scope | PARTIAL | DF copy:27. The scope clause sits in a ‹until P1› sentence, which DF copy:4 withdraws once the full-history sentence appears (N-m6). |
| PMM Minor 9 — E17 never said what "Use this reading" does | PARTIAL | copy:264. The sentence renders at P0 while its action is `[phase: action-absent]` (copy:265), unlike E18's F97 treatment (N-m5). |
| PMM Nit 1 — E15's "OK" | RESOLVED | copy:246 |
| PMM Nit 2 — E5's "passes" | RESOLVED | copy:141 |
| PMM Nit 3 — E17's "most recently saved" | RESOLVED | copy:264 |
| PMM Nit 4 — "no current colour" | RESOLVED | copy:180 |
| PMM Nit 5 — Device PRD says "simulated badge" | UNRESOLVED (deferred by F81) | /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/device-management/prd-device-management.md:266 is unchanged, and no post-lock line carries the deferral (N-n5). |
| PMM unrated — a dogfood reading of E12 | RESOLVED | prd:744-745 (F82) |

**New findings**

[BLOCKER] **N-B1 — E12 files Outside sRGB under "The chip isn't the true colour".** copy:216 (the Body opens "The chip isn't the true colour.") and copy:298 (outside-sRGB's "Group in E12" cell).
- **Reader reaction:** since F40, R2.3 (prd:423) renders the chip colour-managed to the display and clips it only outside the display's gamut. On a Display P3 screen, a colour inside P3 but outside sRGB renders true and carries only Outside sRGB; UJ2.1-c asserts exactly this (journeys:187: ZX-002, "not cannot-show… a display triplet that is not clipped"). The Cataloger opens "Colour marks" and is told this chip isn't the true colour. That is false, and it teaches them to distrust the colours the product shows correctly.
- **How often:** Apple's recent built-in displays, including the M1 MacBook Air that OQ 1 names, are Display P3, and saturated marker sets are the core collection. This will be a common reading.
- **Where it came from:** the group name was my round-1 suggestion. It was true while the chip's rendering was undecided; F40 then made it false for Outside sRGB alone. Because E12's Body is written as prose, the heading also reads as a blanket first sentence of the legend.
- **Trace / mutation:** Given UJ2.1-c (Display P3), R8.10c reads ZX-002 unclipped with marks {outside-sRGB}. Fire "Colour marks": E12 shows outside-sRGB under "The chip isn't the true colour". Traced against R2.3 under copy-honesty rule 5, this fails. With the rewrite below it passes on sRGB (UJ2.1-b: both marks, heading true) and on P3 (UJ2.1-c: one mark, chip true).
- **Rewrite:**
  - Rename the group to a neutral heading, e.g. "Colour beyond a limit", in copy:297-298 and at the start of E12's Body.
  - End Outside sRGB's definition with: "…its Lab, XYZ and spectral values are unaffected. Without Can't show beside it, the chip is still the true colour."
  - This is wording only: F6's two marks, F36's definition and F38's table are untouched.

[BLOCKER] **N-B2 — E8's undo promise outlives R4.7.** copy:170 (Body) and copy:172 ("clear" variant): "you can undo this while the file stays open."
- **The claim vs the row:** F29's R4.7 (prd:500) ends the undo history at "any other committed action (a delete, a Flag, a restore, a reorder, a re-scan answer, an import or a re-read)". UJ6.4-f asserts "Undo change" is gone after each of them (journeys:358).
- **Reader reaction:** the Cataloger clears Family on 200 swatches. They trust the promise because the same sentence has just told them "What was there isn't kept in your file". They delete one swatch, then decide the clear was wrong. Undo is gone and 200 values are permanently lost, with the file still open.
- **Secondary risk:** R6.2 and R4.7 are separate P1 rows. If E8 ships before R4.7, it promises an undo that doesn't exist.
- **Trace / mutation:** Given UJ6.2-c's 11-item clear (E8 "clear" rendered), When a delete fires through the DF E8 delete action (UJ6.4-f's step), Assert "Undo change" is offered. This fails under R4.7 while the file is still open, so E8's sentence fails copy-honesty rule 5. With the rewrite, the stated window matches R4.7 and the trace passes.
- **Rewrite (both variants):** "What was there isn't kept in your file. You can put it back with Undo change until the file closes or you do anything other than another edit."

[MINOR] **N-m1 — the reading mark now has two names; the sibling half wasn't carried.** DF copy:37 (E11) and DF copy:38 (E26) say the earlier reading is "marked as not yet settled". F37 renamed that mark "Awaiting answer" (copy:216, copy:307) on the premise that "settled/unsettled then means rows only".
- **Where the reader meets it:** this PRD renders DF E11 inside the item detail (R4.2h, prd:492) and opens DF E26 through "Answer re-scans" (R5.7, prd:529).
- **Reader reaction:** they read "marked as not yet settled", open "Colour marks", and find no such mark. The legend loses its job of naming every mark the app mentions.
- **Rewrite:** in DF E11 and E26, "marked as awaiting your answer". This needs a dated DF fence as a sibling half; failing that, add a post-lock DF line.

[MINOR] **N-m2 — "samples disagreed" is both a cause and a mark, meaning opposite things.** Capture copy:54 (the new F96 cause label: the row was set aside and the average was not accepted) vs copy:301 (the chip and filter label "Samples disagreed": the average was accepted).
- **Reader reaction:** the Cataloger picks "Samples disagreed" in Filters to find every swatch whose samples disagreed. The ones they set aside (Capture R4.10 "Set it aside") are not listed, yet each one's State line reads "samples disagreed".
- **Rewrite:**
  - Filter label: "Samples disagreed, average accepted", matching R4.2d's own detail wording at copy:331.
  - E12 definition, add: "A swatch you set aside instead has no colour to show, and its State line gives samples disagreed as the reason."

[MINOR] **N-m3 — E9's new headline misstates the scope from All items.** copy:179, "Colours close to ⟨code⟩ in ⟨collection⟩". From All items, "in Blues" reads as the search scope, but the list spans every collection (UJ3.4-d, journeys:236, lists Blues Two's FS-101), and the body says "in all your collections".
- **Rewrite:** "Colours close to ⟨code⟩ (⟨collection⟩)". The collection then identifies the swatch rather than the scope.

[MINOR] **N-m4 — "observer" is jargon in a main-surface body.** copy:123, copy:125, copy:225, copy:227: "⟨unlike⟩ swatches worked out under a different light or observer…". This sentence shows on the collection surface under any L*, C* or h° sort, and a non-expert can't tell what "observer" means.
- **Rewrite:** "⟨unlike⟩ swatches whose colour was worked out for a different light or viewing angle come after the rest in this order."

[MINOR] **N-m5 — E17 describes an action that isn't there at P0 (PARTIAL of Minor 9).** copy:264 renders "Use this reading, where it's offered, makes an earlier reading the current one again…" while copy:265 marks the action `[phase: action-absent]`.
- **Reader reaction:** at P0 they look for an action that exists nowhere.
- **Inconsistency:** F97 phase-marked the identical pattern in E18.
- **Rewrite:** treat E17 as F97 treated E18 (a phase-marked variant enumerated by R5.3), or get an owner call accepting the hedged sentence at P0.

[MINOR] **N-m6 — DF E33 loses its scope statement at P1 (PARTIAL of Minor 8).** DF copy:27 puts "Export first saves all of ⟨collection⟩, these swatches included" in a ‹until P1› sentence. DF copy:4 withdraws that sentence when the full-history ‹P1› sentence appears, and the replacement says nothing about scope.
- **Consequences:** the round-1 reader problem returns (deleting 3 of 500 and landing on a 500-swatch export). The Outbound obligation at prd:631 ("says it saves, the whole collection") also stops being met.
- **Rewrite:** carry the scope into the ‹P1› sentence: "‹P1› Export first saves all of ⟨collection⟩, these swatches included; exporting with full history carries every earlier reading as well as the current one, still not the scanning record." This needs a DF fence.

[NIT] **N-n1 — Spread lands under "Earlier readings".** copy:216: the Spread definition ends the Body after the "Earlier readings" group and has no group of its own in the Mark labels table, so it reads as an earlier-readings mark. Give it its own heading (e.g. "In the table") or place it before the groups.

[NIT] **N-n2 — "from the swatch" is ambiguous.** copy:216, Re-scan unanswered: "answer it from the swatch" can be read as the physical swatch. Use "from its swatch detail".

[NIT] **N-n3 — the Demo Device is labelled "Instrument".** copy:343, History line R5.2b: a Demo Device reading shows "Instrument: Demo Device" beside E12's "the Demo Device, not an instrument". Use "Device", the line's own name at prd:519.

[NIT] **N-n4 — the not-compared line says "measured".** copy:347, R5.4: "measured under a different light or condition". E9 says "worked out under a different light or measurement condition", and the light is a calculation choice, not something measured under. Use E9's words.

[NIT] **N-n5 — two reported-but-deferred wording drifts aren't tracked.** F81 defers the Device PRD's "simulated badge" to Device's next amendment. Capture R5.8/R6.6 quote "flagged: missing or damaged" against the new label "flagged as missing or damaged" (Capture copy:56). Neither is in /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/post-lock.md; the diff adds only two DF items. Add a Device line and a Capture line so neither is lost.

## Biggest risks   (what misleads, confuses, or loses the reader)
- **The honesty legend now under-trusts true colours.** On a P3 Mac, the legend tells the Cataloger that correct chips are wrong (N-B1). This damages the product's core promise, measurement-grade fidelity, in the one place meant to build trust.
- **A recoverability promise the product won't keep, on a destructive bulk action.** E8's undo window is wider than R4.7's (N-B2), which leads straight to permanent loss of values.
- **Vocabulary drift across the seam.** "not yet settled" vs "Awaiting answer" (N-m1), and "samples disagreed" meaning opposite things as a cause and as a mark (N-m2). The legend works only if every word the app shows maps to one entry in it.

## Genuinely strong   (incl. where plain-and-honest is right that a marketing-zealot would over-hype)
- **E18 and E19 are model confirmations.** Each states the consequence in the reader's terms ("has no colour until it's scanned again", "will add it as a new column") and reassures ("Nothing is deleted", "Every value in this column stays as it is"). The restore sentence is phase-marked instead of promising early.
- **The Outside sRGB definition is now exactly true.** It names what to distrust (sRGB, HSL) and what to trust (Lab, XYZ, spectral) wherever the numbers go.
- **E6 now says plainly that nothing happened and what to do next.** "aren't available… Nothing has been changed — do it again once the session has ended". It uses the siblings' "active, paused or halted".
- **The Mark labels table fixes chip, filter and VoiceOver words in one place**, and the capture cause labels now have a single home rather than two copies.
- **"Capitals and extra spaces" is now true against Import R2.3**, and it was carried into Capture E1 and E30 rather than left to drift.
- **The voice stays calm.** There are no speed or quality claims, and E9 still limits itself ("never a named colour library") without naming a competitor. For in-app copy, plain and honest is right.

## Missing / over-hyped
- **Over-hyped:** nothing.
- **Missing (optional):**
  - E19 could say the consequence is permanent in v1 (F63: no column delete or merge), since that would change the reader's choice. Something like: "A column can't be removed once it's added."
  - F82's dogfood reading of E12 would catch N-B1 only if done on a P3 display with an outside-sRGB swatch; name that condition in the check.

| Row ID | disposition |
|---|---|
| R1.1 | ALIGN |
| R1.2 | ALIGN |
| R1.3 | ALIGN |
| R1.4 | ALIGN |
| R1.5 | ALIGN |
| R1.6 | ALIGN |
| R1.7 | ALIGN |
| R1.8 | ALIGN |
| R1.9 | ALIGN |
| R1.10 | ALIGN |
| R2.1 | ALIGN |
| R2.2 | ALIGN |
| R2.3 | ALIGN |
| R2.4 | ALIGN |
| R2.4a | ALIGN |
| R2.4b | ALIGN |
| R2.4c | ALIGN |
| R2.4d | ALIGN |
| R2.4e | ALIGN |
| R2.4f | ALIGN |
| R2.4g | ALIGN |
| R2.4h | ALIGN |
| R2.4i | ALIGN |
| R2.5 | ABSTAIN |
| R2.6 | ALIGN |
| R2.7 | ALIGN |
| R2.8 | ALIGN |
| R2.9 | ALIGN |
| R2.10 | ABSTAIN |
| R2.11 | ALIGN |
| R3.1 | ALIGN |
| R3.2 | ABSTAIN |
| R3.3 | ALIGN |
| R3.4 | ALIGN |
| R3.5 | ALIGN |
| R3.6 | ABSTAIN |
| R3.7 | ALIGN |
| R3.8 | ALIGN |
| R3.9 | ABSTAIN |
| R4.1 | ABSTAIN |
| R4.2 | ALIGN |
| R4.2a | ALIGN |
| R4.2b | ALIGN |
| R4.2c | ALIGN |
| R4.2d | ALIGN |
| R4.2e | ALIGN |
| R4.2f | ALIGN |
| R4.2g | ALIGN |
| R4.2h | OBJECT (N-m1) |
| R4.3 | ALIGN |
| R4.4 | ALIGN |
| R4.5 | ALIGN |
| R4.6 | ALIGN |
| R4.7 | ALIGN |
| R4.8 | ALIGN |
| R4.9 | ALIGN |
| R5.1 | ALIGN |
| R5.2 | ALIGN |
| R5.2a | ALIGN |
| R5.2b | OBJECT (N-n3) |
| R5.2c | ALIGN |
| R5.2d | ALIGN |
| R5.2e | ALIGN |
| R5.3 | ALIGN |
| R5.4 | OBJECT (N-n4) |
| R5.5 | ALIGN |
| R5.6 | ALIGN |
| R5.7 | OBJECT (N-m1) |
| R5.8 | ABSTAIN |
| R6.1 | ABSTAIN |
| R6.2 | ALIGN |
| R6.3 | OBJECT (N-m6) |
| R6.4 | ABSTAIN |
| R7.1 | ABSTAIN |
| R7.2 | ABSTAIN |
| R8.1 | ABSTAIN |
| R8.1a | ABSTAIN |
| R8.1b | ABSTAIN |
| R8.1c | ABSTAIN |
| R8.1d | ABSTAIN |
| R8.1e | ABSTAIN |
| R8.1f | ABSTAIN |
| R8.2 | ABSTAIN |
| R8.3 | ALIGN |
| R8.4 | ALIGN |
| R8.5 | ABSTAIN |
| R8.6 | ABSTAIN |
| R8.7 | ABSTAIN |
| R8.8 | ABSTAIN |
| R8.9 | ALIGN |
| R8.10 | ABSTAIN |
| R8.10a | ABSTAIN |
| R8.10b | ABSTAIN |
| R8.10c | ABSTAIN |
| R8.10d | ABSTAIN |
| R8.10e | ABSTAIN |
| R8.10f | ABSTAIN |
| R8.11 | ABSTAIN |
| E1 | ALIGN |
| E2 | ALIGN |
| E3 | OBJECT (N-m4) |
| E4 | ALIGN |
| E5 | ALIGN |
| E6 | ALIGN |
| E7 | ALIGN |
| E8 | OBJECT (N-B2) |
| E9 | OBJECT (N-m3) |
| E10 | ALIGN |
| E11 | ALIGN |
| E12 | OBJECT (N-B1; N-m2; N-n1; N-n2) |
| E13 | OBJECT (N-m4) |
| E14 | ALIGN |
| E15 | ALIGN |
| E16 | ALIGN |
| E17 | OBJECT (N-m5) |
| E18 | ALIGN |
| E19 | ALIGN |
| M1 | ABSTAIN |
| M2 | ABSTAIN |
| M3 | ABSTAIN |
| M4 | ALIGN |

#### peer-architecture-reviewer (Claude route)

## Verdict
Sound, build it. None of my round-1 findings needs redesign: 14 are resolved, 1 is partial (A12), and the fix pass adds no Blocker. It does add one Major hand-off gap. The rule that deleted, cleared or replaced text is gone from the file's bytes is stricter in this PRD's rows and cases than in the input handed to ADR-0003. Close that before lock.

## Architecture in brief
- **Ownership.** Collection Mode is the browsing and editing layer over Data Foundation's single-writer SQLite file. It owns:
  - the in-memory view state: search, filters, sorts and the metadata undo history;
  - the one live-computed mark, cannot-show;
  - the entry points into actions its siblings own.
- **Sibling boundaries.** The round-1 fixes moved every boundary finding to the right owner:
  - item identity across a code change is Data Foundation's R1.2 and an ADR-0003 input;
  - the restore guard is this PRD's R8.3, mirrored in Capture R5.6 and R8.18;
  - the read-only floor defers to Data Foundation R5.3 and its OQ 18;
  - the chip value is R2.3, with the R8.10c readback.
- **The key tradeoff.** Honesty marks are read from persisted reading facts, and only cannot-show is computed live. The fix pass pinned it to the stored flag on sRGB displays (F40).
- **Seams still open.** The remaining risks sit on two seams: the byte-level erasure contract with ADR-0003, and write scheduling between capture and editing, which ADR-0005 owns.

## Findings
Paths are relative to /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/:
- prd = docs/product/collection-mode/prd-collection-mode.md
- jny = docs/product/collection-mode/prd-collection-mode-journeys.md
- DF = docs/product/data-foundation/prd-data-foundation.md
- CAP = docs/product/capture-mode/prd-capture-mode.md
- ADRQ = docs/decisions/README.md

### (1) Round-1 delta: my findings

| Round-1 ID | Result | Evidence |
|---|---|---|
| A1 undo breaks uniqueness | RESOLVED | prd:500 — R4.7's history ends on any other committed action, an import and a re-read included, and each undo re-checks R1.3, R4.4, R4.8 and R8.3. My two scenarios are cases: jny:359 (UJ6.4-g) and jny:360 (UJ6.4-h). |
| A2 identity independent of the code | RESOLVED | DF:100 (R1.2 identity sentence); prd:303 (Stop: ADR-0003 with that input); prd:634 (Outbound); ADRQ:22. |
| A3 which value the chip renders | RESOLVED | prd:423 (R2.3: the working-set value, never the stored sRGB); prd:605 (R8.10c reads back the triplet and whether it clipped); jny:186-187. A residual edge case is new finding A17. |
| A4 restoring during an in-flight session | RESOLVED | prd:590 (R8.3 refuses both restores and E10's Undo); CAP:213, CAP:297 (mirrored); jny:281 (UJ4.5-d); jny:322 (UJ5.3-h). |
| A5 R8.4 widens the read-only floor | RESOLVED | prd:591 (a newer-format file gets DF R5.3's minimum, the rest left to DF OQ 18); jny:415 and jny:417 (UJ9.2-a and UJ9.2-c split). |
| A6 R8.1 assumes one navigation reading | RESOLVED | prd:582 (R8.1c: "while shown, or at its next showing"); jny:411. |
| A7 column visibility has no counterpart | RESOLVED | prd:595 (R8.8's atomic list); prd:591 and jny:415 (disabled on read-only); DF:100; ADRQ:22. |
| A8 All items compares across working sets | RESOLVED | prd:460 (R3.3 under F54, F68 and F94); prd:464 (R3.7 computes no derived set and writes nothing); jny:236, jny:380, jny:383. A residual is NIT A24. |
| A9 locale undeclared | RESOLVED | prd:603 (R8.10a); jny:93 (named default: English). |
| A10 display gamut in the seam and in filtering | RESOLVED | prd:603 (any reported gamut); prd:543 (R6.4 deselects on a display change); jny:198 (UJ2.1-n, Rec. 2020). |
| A11 file-wide ceiling too low | RESOLVED | prd:279 (100,000). |
| A12 bulk writes never timed | PARTIAL | prd:585 (R8.1f covers bulk delete and "Use as scan order"), but jny:423 (UJ9.5-a) times only the clear and its undo. |
| A13 restore keeps its provenance | RESOLVED | prd:527; DF:133 (R2.3f); jny:325 (UJ5.3-k). |
| A14 one source for "unreadable" | RESOLVED | CAP:297 (R8.18 follows DF's quarantine mark, read-only included). |
| A15 R11.15g points at the wrong row | RESOLVED | CAP:376. |
| ARCH unrated 1 (no stored cannot-show as an ADR-0003 input) | RESOLVED | prd:442; prd:297; prd:634; ADRQ:22. |
| ARCH unrated 2 (All items search matches imported values) | RESOLVED | prd:412 (R1.10). |
| Missing: Capture R3.7 fallback for a deleted remembered row | RESOLVED | CAP:161; jny:275 (UJ4.4-f). |
| Missing: R2.10's route in Row transitions | RESOLVED | prd:138. |

**[MINOR] A12 (residual) — R8.1f bulk writes at scale.**
- **Scenario:** the costliest write this PRD makes has no timing case. That is a bulk delete at ROWS_CEILING, which removes readings, samples and raw payloads under the byte-level erasure rule (F35). "Use as scan order" at ROWS_CEILING is untimed too. Both are named in prd:585 and bounded by BULK_WRITE_BUDGET (prd:277).
- **Fix:** add both to UJ9.5-a (jny:423), each with progress shown and completion within BULK_WRITE_BUDGET, the bulk delete run under an active search and sort.

### (2) New defects introduced by the round-1 and 1b fixes

**[MAJOR] A16 — Byte-level erasure, the seam with ADR-0003 and Data Foundation: the input handed over is weaker than the rows and cases.**
- **What this PRD requires:**
  - R4.7 (prd:500) keeps a replaced value "only in the running app's memory, never in the file or anywhere outside it".
  - R8.10d (prd:606) reads "the bytes of the file and of anything the app keeps beside it … while it is open, and after a crash before reopening".
  - UJ6.4-j (jny:362) reads the file and what sits beside it while "Undo change" is still offered.
  - UJ6.4-k (jny:363) crashes, then reads those bytes before reopening.
- **What the hand-offs say:** only that the text is "gone from the file's bytes". This wording is at prd:298 (the Stop cell), prd:634 (Outbound), DF:229 (R6.2a), DF:232 (R6.2d: "the active file") and ADRQ:22 (the ADR-0003 inputs). None of them names the journal or WAL sidecar files, commit time, or the crash window.
- **Scenario:** ADR-0003 is drafted from its queue row and picks WAL mode with passive checkpoints, a standard choice that meets "gone from the file's bytes" once the file is checkpointed at close.
  - Under WAL, a committed change sits in the -wal file while the main file keeps the superseded page until a checkpoint.
  - So UJ6.4-j finds "Pink" in the main file while it is open, and UJ6.4-k finds it after a crash.
  - An FTS5 search index fails in the same way: it keeps deleted tokens in its segments unless its secure-delete option is on.
- **Consequence:** the gap surfaces only at build time, as a storage-configuration rework against the project's sharpest one-way door. If the tests were ever relaxed, the crash path would break F35's privacy promise.
- **Not re-litigating F35 or F29:** this PRD's rows are coherent. The defect is the weaker hand-off.
- **Fix:** an editorial change under F29, F35 and F53, landing together in prd:298, prd:634, DF R6.2a/F52(2) and ADRQ:22. Extend the input to: "gone from the file, and from any journal, WAL, index or other file the app keeps beside it, from the moment the write lands and after a crash". ADR-0003 still picks the mechanism: secure_delete, a checkpoint-and-reset per text-replacing commit, or rollback-journal mode.

**[MINOR] A17 — R2.5 and R2.3: the edge-of-gamut judgement on an sRGB display comes from two computations.**
- **The two paths:**
  - On a display reporting sRGB or none, cannot-show "is set exactly when the stored gamut-clipped flag is" (prd:442, F40).
  - The chip renders "the display's clipped colour" when the value is outside that display's gamut, as the live conversion finds it (prd:423). R8.10c exposes whether the triplet was clipped.
- **Where they can disagree:** Data Foundation R3.4 (DF:145) fixes only "sRGB and relative colorimetric" for the stored flag. It names no chromatic adaptation and no tolerance. OQ 7's live interim is Bradford adaptation at zero tolerance (prd:761). A value within about 1e-3 of the sRGB boundary can therefore be clipped on the chip while carrying no cannot-show mark. That is the non-negotiable direction of the honesty failure.
- **Hidden by the fixtures:** the harness's 0.03 gamut-margin rule (jny:51-57) guarantees no case exercises the edge.
- **Fix, preserving F40:** hand Data Foundation and ADR-0003 the input that the stored flag is computed by OQ 7's interim test at sRGB, so the two paths are one function and change together when OQ 7 closes. Add one edge case where a value inside sRGB by less than the margin has an unclipped triplet and no cannot-show mark.

**[MINOR] A18 — R8.11 with R8.8 and R8.1f: capture takes precedence over a single-writer file, but ADR-0005 has not been told.**
- **The rows together:**
  - R8.3 (prd:590) keeps bulk set and clear available during a session.
  - R8.8 (prd:595) makes each bulk write atomic.
  - R8.1f (prd:585) allows it BULK_WRITE_BUDGET, which is 2 s (prd:277).
  - R8.11 (prd:612) forbids delaying capture's row confirmation past ROW_CONFIRM_BUDGET. That budget is still TBD and is measured on internal, USB and network volumes (CAP:189).
- **What they imply:** on one SQLite writer, an in-flight bulk transaction must hold the writer for less than the time a capture save can wait. That bound is well under 2 s and is stated nowhere.
- **What the core needs:** a write path that gives capture priority, keeps non-capture transactions bounded, and keeps UI refreshes off the path of capture's cues. ADR-0005 owns that shape (executor and runtime ownership, ADRQ:24), and its queue row carries no inputs.
- **Test gap:** UJ9.5-d (jny:426) runs on internal disk only.
- **Fix:**
  - Record "capture saves and cues take precedence over any Collection Mode write or refresh (R8.11)" as an ADR-0005 input, as F98 did for ADR-0003.
  - State in R8.11 that R8.11 governs R8.1f during a session.
  - Or run UJ9.5-d on the slowest volume class ROW_CONFIRM_BUDGET is measured on.

**[MINOR] A19 — R8.2: the All items search workload is not sized, and the size decides whether an index is needed.**
- **The rows:** R1.10 (prd:412; F86) makes All items search match imported values, anywhere in the text. R8.2 (prd:589) requires R8.1a–c's budgets "at FILE_ITEMS_CEILING items", searchable from the first frame with nothing loading behind it (F88, prd:583). It does not say whether R8.1's per-collection IMPORTED_COLUMNS_CEILING (20 × 200) applies.
- **The numbers:** read that way, the search covers about 400 M characters at 100,000 items (40 M per full collection). A per-keystroke in-memory scan at that size probably misses 100 ms on an 8 GB M1 MacBook Air. Meeting it takes an index, which is a schema question for ADR-0003 and falls under A16's erasure rule.
- **Test gap:** UJ9.5-b (jny:424) declares no imported columns, so it tests only the easy reading.
- **Fix:** have R8.2 state the imported width its budgets hold at, have UJ9.5-b declare it, and hand ADR-0003 the choice between an index and a scan if that width is the full ceiling.

**[MINOR] A20 — R8.8: a P0 failure path depends on a Data Foundation state that does not exist yet, with no Stop and no interim.**
- **The gap:** R8.8 (prd:595) renders "the state that PRD owes" when a write is refused because permission was lost. Data Foundation F52 leaves that state's form undecided. It is logged at docs/product/post-lock.md:41.
- **Why it matters:** the first Build dependencies row (prd:297) names no stop for it, and the Legend's no-interim list (prd:322) says "None". A builder working only from the "Available contract" column has no surface to render. The "changing nothing" behaviour itself is specified.
- **Fix:** add either "Stop: the Data Foundation PRD's owed permission-lost state" or an Interim to prd:297. A suitable interim is to render Data Foundation's E10 as the write-refused fallback, with nothing changed.

**[NIT] A21 — R8.10e (prd:607): "its container" assumes a sandboxed build, and ADR-0007 is open.**
- An unsandboxed build keeps its state under Application Support and Caches instead.
- Fix: word it as "any container, Application Support or cache data, preferences or saved window state it keeps".

**[NIT] A22 — R4.7 (prd:500): the rule does not say whether a capture-side commit ends the undo history.**
- A capture save, or a session starting or ending, is not in the list of actions that end the history.
- The re-check keeps both readings correct, but the choice is visible to the user. Say which.

**[NIT] A23 — Row transitions (prd:128-144): no route for an item the re-read finds removed.**
- R4.1 and R8.5 handle an item that a re-read finds removed outside the app.
- That item is neither "present" nor "deleted", because "deleted" carries the byte guarantee (prd:225).

**[NIT] A24 — R3.7 (prd:464): Find similar's results can depend on history.**
- "Values already worked out" means the set of compared items depends on which non-working-set derived sets Data Foundation R3.3f happened to persist, for example after an export under another illuminant.
- Acknowledge this, or restrict the comparison to working sets.

## Biggest risks
- **A16:** the byte-erasure promise is written at three strengths across three documents. ADR-0003, the one-way door, reads the weakest one.
- **A18:** capture's precedence over this PRD's writes depends on a write-scheduling design that no ADR has been asked for.
- **A19:** the file-wide search envelope is unsized. That is the difference between scanning in memory and keeping an index in the file.
- **A17:** a residual honesty split at the sRGB gamut edge, deliberately hidden by the fixture margins.

## Genuinely sound
- **Every round-1 Major landed at its owner and on both sides of its seam, with dated sibling fences:**
  - identity → Data Foundation R1.2 and ADR-0003's queue row;
  - the restore guard → R8.3, Capture R5.6 and R8.18, and Capture's Row transitions;
  - the read-only floor → deferred to Data Foundation R5.3 and OQ 18;
  - the chip value → R2.3 with a readback that actually observes the rendered triplet.
- **The ADR-0003 queue row now lists Collection Mode's inputs (F98)**, and the Build dependencies table keeps the format's three columns with Stop:/Interim: labels (F91). That is the right place for both, with no fork of the format.
- **Capture R8.18 derives the unreadable standing from Data Foundation's quarantine mark**, on read-only files too. This is the correct single-source design, even though it makes row state partly derived.
- **The navigation fork is still not selected.** R8.1c and R8.3's last sentence work under either reading. UJ9.5-d (capture saving while the user bulk-clears) is also neutral: the instrument trigger is not a focus-gated capture shortcut (CAP:178, CAP:336). A reviewer might wrongly flag it as assuming reading B.
- **R8.3's split is right for one process with one writer.** Row-moving actions are refused during a session, while metadata edits and correction answers stay available. Blocking every write during capture would be over-cautious.
- **R4.7's undo is the simplest correct model.** It is memory-only, ended by any other action, re-checked on undo, with no redo. A persisted command log or a conflict-resolving undo would be over-engineering, and would also fight F35.
- **Browse actions stay read-only.** Because R3.7 computes no derived set, Find similar works on a read-only file.
- **No premature indexes.** Exhaustive scans at 10,000 rows are fine, and R8.2's "history never aged out" is fine too.

## Missing / over-engineered
- **Missing:**
  - the full-strength byte-erasure input for ADR-0003 (A16);
  - an ADR-0005 input for capture-priority writes (A18);
  - R8.2's imported-column width (A19);
  - a stop or interim for the permission-lost state (A20);
  - bulk-delete and "Use as scan order" timing at ROWS_CEILING (A12).
- **Over-engineered:** nothing structural. The verification seam R8.10a–f is broad, but every line serves ADR-0001's headless test gates.

| Row ID | disposition |
|---|---|
| R1.1 | ALIGN |
| R1.2 | ALIGN |
| R1.3 | ALIGN |
| R1.4 | ALIGN |
| R1.5 | ALIGN |
| R1.6 | ALIGN |
| R1.7 | ALIGN |
| R1.8 | ALIGN |
| R1.9 | ALIGN |
| R1.10 | ALIGN |
| R2.1 | ALIGN |
| R2.2 | ALIGN |
| R2.3 | ALIGN |
| R2.4 | ALIGN |
| R2.4a | ALIGN |
| R2.4b | ALIGN |
| R2.4c | ALIGN |
| R2.4d | ALIGN |
| R2.4e | ALIGN |
| R2.4f | ALIGN |
| R2.4g | ALIGN |
| R2.4h | ALIGN |
| R2.4i | ALIGN |
| R2.5 | OBJECT (A17) |
| R2.6 | ALIGN |
| R2.7 | ALIGN |
| R2.8 | ALIGN |
| R2.9 | ALIGN |
| R2.10 | ALIGN |
| R2.11 | ALIGN |
| R3.1 | ALIGN |
| R3.2 | ALIGN |
| R3.3 | ALIGN |
| R3.4 | ALIGN |
| R3.5 | ALIGN |
| R3.6 | ALIGN |
| R3.7 | ALIGN |
| R3.8 | ALIGN |
| R3.9 | ALIGN |
| R4.1 | ALIGN |
| R4.2 | ALIGN |
| R4.2a | ALIGN |
| R4.2b | ALIGN |
| R4.2c | ALIGN |
| R4.2d | ALIGN |
| R4.2e | ALIGN |
| R4.2f | ALIGN |
| R4.2g | ALIGN |
| R4.2h | ALIGN |
| R4.3 | ALIGN |
| R4.4 | ALIGN |
| R4.5 | ALIGN |
| R4.6 | ALIGN |
| R4.7 | OBJECT (A16) |
| R4.8 | ALIGN |
| R4.9 | ALIGN |
| R5.1 | ALIGN |
| R5.2 | ALIGN |
| R5.2a | ALIGN |
| R5.2b | ALIGN |
| R5.2c | ALIGN |
| R5.2d | ALIGN |
| R5.2e | ALIGN |
| R5.3 | ALIGN |
| R5.4 | ALIGN |
| R5.5 | ALIGN |
| R5.6 | ALIGN |
| R5.7 | ALIGN |
| R5.8 | ALIGN |
| R6.1 | ALIGN |
| R6.2 | ALIGN |
| R6.3 | ALIGN |
| R6.4 | ALIGN |
| R7.1 | ALIGN |
| R7.2 | ALIGN |
| R8.1 | OBJECT (A12, A18) |
| R8.1a | ALIGN |
| R8.1b | ALIGN |
| R8.1c | ALIGN |
| R8.1d | ALIGN |
| R8.1e | ALIGN |
| R8.1f | OBJECT (A12, A18) |
| R8.2 | OBJECT (A19) |
| R8.3 | ALIGN |
| R8.4 | ALIGN |
| R8.5 | ALIGN |
| R8.6 | ALIGN |
| R8.7 | ALIGN |
| R8.8 | OBJECT (A20) |
| R8.9 | ALIGN |
| R8.10 | OBJECT (A16) |
| R8.10a | ALIGN |
| R8.10b | ALIGN |
| R8.10c | ALIGN |
| R8.10d | OBJECT (A16) |
| R8.10e | ALIGN |
| R8.10f | ALIGN |
| R8.11 | OBJECT (A18) |
| E1 | ALIGN |
| E2 | ALIGN |
| E3 | ALIGN |
| E4 | ALIGN |
| E5 | ALIGN |
| E6 | ALIGN |
| E7 | ALIGN |
| E8 | ALIGN |
| E9 | ALIGN |
| E10 | ALIGN |
| E11 | ALIGN |
| E12 | ALIGN |
| E13 | ALIGN |
| E14 | ALIGN |
| E15 | ALIGN |
| E16 | ALIGN |
| E17 | ALIGN |
| E18 | ALIGN |
| E19 | ALIGN |
| M1 | ALIGN |
| M2 | ALIGN |
| M3 | ALIGN |
| M4 | ALIGN |

#### peer-performance-reviewer (Claude route)

## Verdict
Meets budget. Round 1's Blocker (PERF-1) and all four Majors are resolved, and no Blocker remains. Two new Majors should land before the envelope rows flip. First, a bulk delete under the new rule that deleted content be gone from the file's bytes measured 3.05 s on the fastest Mac against a 2 s budget, and no case times it. Second, UJ9.5-d, the only case behind R8.11, almost never makes capture and a bulk write actually compete for the file.

## Workload & budget (brief)
- **Paths.** Root is `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode`. Short names used below:
  - `prd` = docs/product/collection-mode/prd-collection-mode.md
  - `jny` = …/prd-collection-mode-journeys.md
  - `fix` = …/prd-collection-mode-round-1-fixes.md
  - `capture` = docs/product/capture-mode/prd-capture-mode.md
  - `df` = docs/product/data-foundation/prd-data-foundation.md
  - `v2` = docs/briefs/browsing-a-collection-at-scale-research-results-v2.md
- **Scale.**
  - ROWS_CEILING is 10,000 items per collection. IMPORTED_COLUMNS_CEILING is 20 columns × 200 characters. FILE_ITEMS_CEILING is 100,000 items (prd:274–279).
  - A collection at ROWS_CEILING is about 1.2 GB raw, 620 MB compressed (df:328).
- **Budgets and gating machine.** 100 ms p95 to browse, 1 s to open, 1% dropped frames, 2 s for a bulk write. All are gated on an M1 MacBook Air 8 GB, internal disk. Capture's TRIGGER_ACK_WINDOW is 100 ms; its ROW_CONFIRM_BUDGET is still TBD (capture:68–69).
- **Hot paths.** The table and grid at ROWS_CEILING. All items and Find similar at 100k. Bulk writes at ROWS_CEILING while a session is in flight.
- **Measured this round.** Script: `/private/tmp/claude-501/-Users-vinnypasceri-Projects-spectro-capture/d4b2a2e5-4696-48c6-9516-d4cbad888739/scratchpad/perf/bulk_delete_probe.py`.
  - Setup: Apple M5 Max, SQLite 3.53.4, WAL, `synchronous=FULL`, `fullfsync` and `checkpoint_fullfsync` on.
  - Stand-in schema: 10k items × 20 × 200-character columns, 100k readings, 300k samples of 2 KB, 1.2M derived rows. The file is 1,307 MB, close to DF OQ 5's estimate. ADR-0003 is still open, so the schema is mine.
  - Results over two runs:

| Operation | Time | WAL |
|---|---|---|
| Bulk clear of one column over 10k items, `secure_delete` on | 125–128 ms | 46 MB |
| One-field edit | 7.7–8.1 ms commit + 3.1–3.2 ms TRUNCATE checkpoint | — |
| Delete every item in one transaction, `secure_delete` off | 845–855 ms | — |
| Same delete, `secure_delete` on | **3,042–3,077 ms** | **1,310 MB** |
| Same pair on a 486 MB file | 1,156 ms off / 1,556 ms on | — |

  - The M1 Air is not measured and will be slower.

## Round-1 delta (my findings)

| Round-1 ID | Status | Evidence |
|---|---|---|
| PERF-1 (Blocker) | RESOLVED | R8.1b adds selecting one row, a range and all, plus opening and closing the detail and history (prd:581). R8.1e and DROPPED_FRAME_SHARE cover paging (prd:584, prd:276). UJ9.5-a delivers 200 of each and pages end to end (jny:423). Two gaps remain: toggling a row is untimed, and the paging workload is undeclared (PERF2-7, PERF2-6). |
| PERF-2 (Major) | PARTIAL | R8.1c and R8.1f class the writes (prd:582, :585). UJ9.5-a adds 200 Demo Device saves and a bulk clear with undo at ROWS_CEILING (jny:423). But "Delete collection" (R1.4) and E10's undo of it are in neither class, although fix:545 says "none left unclassed". This is the largest write the PRD triggers (PERF2-1). |
| PERF-3 (Major) | RESOLVED | M1 Air, internal disk, cold first open and warm browsing, read every release (prd:274, :739, :755). Clock runs from when the save lands to the first frame (prd:574). Residual conditions: PERF2-6. |
| PERF-4 (Major) | RESOLVED | The named timing workload sets queries, cadence and round-robin (jny:95). IMPORTED_COLUMNS_CEILING added (prd:278). UJ9.5-a runs at that width (jny:423). |
| PERF-5 (Major) | RESOLVED | FILE_ITEMS_CEILING is 100,000 (prd:279). OQ 2's closer is now the owner's estimate of collections per file (prd:756). |
| PERF-6 | RESOLVED | R8.2 names R8.1a–c and gives All items an open budget (prd:589). UJ9.5-b declares its split and scope (jny:424). Residual: PERF2-5. |
| PERF-7 | RESOLVED | R2.5 and R3.1 now cite p95 (prd:442, :458). UJ2.1-d and UJ9.1-a use a 5 s functional timeout (jny:188, :411). Display moves are timed (jny:423). |
| PERF-8 | RESOLVED | R8.1's lead row covers everything above the ceilings (prd:574). UJ9.5-c runs at 2× ROWS_CEILING (jny:425). No equivalent rule for FILE_ITEMS_CEILING: PERF2-8. |
| PERF-9 | RESOLVED | R8.1d defines first rows and says there is no lazily loaded tail (prd:583). No case tests the no-tail rule: PERF2-5. |
| PERF-10 | PARTIAL | R8.11 (prd:612) and UJ9.5-d (jny:426) exist. But UJ9.5-d cannot fail as written (PERF2-2), and its ROW_CONFIRM_BUDGET oracle is TBD with no interim (PERF2-3). |
| PERF-11 | PARTIAL | R8.1a and R8.1b reach the grid (prd:580–581). UJ9.5-a times only the Table/Grid switch and the swatch size (jny:423). Nothing times search, filter or paging in the grid. R8.1e says "Paging through the table" only (prd:584), yet grid frame drops are a symptom the research names (v2:183). |
| PERF-12 | RESOLVED | prd:326, prd:755. |
| PERF-13 | RESOLVED | prd:755; capture:441. |
| PERF-14 | RESOLVED | UJ9.5-a undoes a bulk clear over ROWS_CEILING items (jny:423). |

## Findings

**[MAJOR] PERF2-1 — Bulk delete and collection delete at ROWS_CEILING are the most expensive bulk write, untimed, and likely over budget**
- **Where:** prd:585 (R8.1f), prd:277 (BULK_WRITE_BUDGET), prd:612 (R8.11), prd:595 (R8.8), jny:423 (UJ9.5-a), df:229 (R6.2a), docs/decisions/README.md:22.
- **How the cost arises:**
  - F35 and DF R6.2a require deleted item content to be gone from the file's bytes.
  - R8.8 makes a delete atomic, so it is one transaction.
  - The obvious way to meet both is SQLite `secure_delete`. It zero-fills every freed page through the WAL.
- **Measured:** deleting every item of a 1.3 GB ceiling collection took 3.05 s on an M5 Max, with a 1.31 GB WAL. Without erasure it took 0.85 s. On the gating M1 Air 8 GB it will be slower still.
  - That exceeds BULK_WRITE_BUDGET, which the constants table says to "build and test … at ROWS_CEILING items".
  - It also holds SQLite's single write lock that whole time.
- **Collision with R8.11:** R8.3 refuses deletes only "on that collection" (prd:590). So deleting a different ceiling collection, or all of its items, is allowed while a session is in flight. Capture's durable row save then waits seconds.
- **Why nothing catches it:**
  - UJ9.5-a times only the cheap bulk op (a Family clear, about 130 ms measured).
  - R8.1c and R8.1f don't class "Delete collection" at all.
  - The ADR-0003 input line (README:22) hands over "gone from the bytes" with no cost bound.
- **Scenario:** a build using `secure_delete` passes every case. On the M1 Air, a whole-collection delete takes several seconds and stalls another collection's capture saves for as long.
- **Fix, without reopening F35, F42 or F89:**
  - Add to UJ9.5-a a "Delete selected" over every Scale item.
  - Add to UJ9.5-d a "Delete collection" of a second ceiling collection while Scale's session runs.
  - Class the collection delete and its E10 undo in R8.1f.
  - Add an ADR-0003 input, on the DF F52 line and README:22: erasing a ceiling collection fits BULK_WRITE_BUDGET and does not hold the write lock past capture's budgets. Per-item-key crypto-erasure is one mechanism that meets both, since only keys are rewritten; the choice stays with the ADR.
  - Have OQ 1's timing run report bulk delete separately.
- **Why not a Blocker:** no row is wrong, a mechanism that meets the budget exists, and 2 s is an OQ 1 candidate. The defect is that nothing measures the case that decides it.

**[MAJOR] PERF2-2 — UJ9.5-d, the only oracle for R8.11, almost never makes capture and the bulk write compete**
- **Where:** jny:426, prd:612.
- **The gap:** the case runs one Family clear on Scale, about 130 ms measured, while 20 sets are captured at DEMO_SCAN_CYCLE.
  - Capture OQ 14 calls this "a roughly three-second loop", so a trigger press comes about every 3 s and a row confirmation about every 9 s.
  - The chance that any trigger press or confirmation falls inside a 0.13–0.4 s write window is about 4–13%.
  - So roughly nine runs in ten never make the two compete. A build that blocks capture's save, or the main thread, for the whole write passes.
- **Also missing:**
  - The Given declares no search, filter or sort, so each save's table refresh takes the cheap path.
  - The heaviest write allowed during a session (PERF2-1's delete on another collection) is absent.
- **Fix:**
  - Start the clear when the Demo Device's last sample of a set arrives, as its test clock sets (capture R11.7).
  - Alternate clear and "Undo change" across all 20 sets.
  - Add PERF2-1's other-collection delete.
  - Declare Scale searched, filtered and view-sorted.
  - Also assert against the same run without the writes (p95 not worse by more than a stated margin), so the case can be read before ROW_CONFIRM_BUDGET closes.

**[MINOR] PERF2-3 — R8.11 (P0) rests on a sibling constant that is TBD with no interim, and nothing lists that dependency**
- **Where:** prd:612, prd:297, prd:322, prd:326–335; capture:69, capture:435.
- **The gap:** ROW_CONFIRM_BUDGET is TBD, and capture's OQ 5 interim says "None for the two TBDs — they are the engineering plan's".
- **What the PRD says instead:**
  - Its no-interim list says "None".
  - The Build dependencies row carrying R8.11 names only OQ 1, OQ 7 and OQ 12.
  - Interim stated leaves R8.11 out.
  - So a builder cannot see that UJ9.5-d's second oracle has no value.
- **Fix:** list R8.11 under the capture PRD's OQ 5, and name the engineering plan's declared ROW_CONFIRM_BUDGET as its interim. Or list R8.11 as the one P0 row without an interim.

**[MINOR] PERF2-4 — R8.1f's "progress shown" and "responsive throughout" can't be observed, so a bulk write that blocks the main thread passes**
- **Where:** prd:585, prd:604 (R8.10b), jny:423.
- **The gaps:**
  - UJ9.5-a asserts "each show progress", but R8.10b gives no readback for progress, and the copy file has no progress state. The case asserts something no seam can read.
  - UJ9.5-a sends no input during the clear or its undo. So "the surface meeting R8.1a–c throughout" is never tested, and a write that freezes the main thread for up to 2 s passes.
- **Fix:**
  - During the clear and its undo (and PERF2-1's delete), send a search keystroke and a selection every 100 ms and time each.
  - Add bulk-write progress to R8.10b.
  - Within F42: let the oracle accept a write that finishes inside BROWSE_RESPONSE_BUDGET with no progress frame (the measured clear takes about 130 ms). Otherwise a fast build fails, or has to flash a progress indicator.

**[MINOR] PERF2-5 — The no-lazy-tail rule (F88) has no case, and R8.2 doesn't clearly carry it to All items**
- **Where:** prd:583 (R8.1d), prd:589 (R8.2), jny:423–424.
- **What the cases time:** UJ9.5-a and UJ9.5-b time the open ("choose Scale 20 times", "fire All items 20 times") separately from the searches.
- **The gap:** a build that shows first rows at 0.9 s but makes a search typed then wait until 3 s passes both. The lazy tail is most tempting in the All items view at 100k.
- **R8.2's wording:** it says the view shows "its first rows within OPEN_COLLECTION_BUDGET". It does not say whether R8.1d's "every other budget holding from that frame" applies.
- **UJ9.5-b's file is under-declared:** readings per item, raw payloads and imported columns are not given, unlike UJ9.5-a's "R7.7 corpus". Its cold open could be a 50 MB file or a 12 GB one.
- **Fix:**
  - At the first-rows frame of the cold open in UJ9.5-a and UJ9.5-b, type the workload's all-match prefix and time it against BROWSE_RESPONSE_BUDGET.
  - R8.2 cites R8.1d's first-rows rule in full.
  - UJ9.5-b generates each collection as the R7.7 corpus.

**[MINOR] PERF2-6 — The new statistics' measurement conditions are not reproducible**
- **Where:** prd:584, prd:608 (R8.10f), prd:739 (M1), prd:755, jny:88, jny:95, jny:423.
- **The gaps:**
  - **Paging:** "page through the table end to end" names no input (Page Down, scroll wheel, trackpad momentum) and no rate.
  - **Refresh rate:** frames are missed against the display's refresh rate. The M1 Air's built-in panel runs at 60 Hz (16.7 ms per frame); OQ 1's "one current Mac" may run at 120 Hz (8.3 ms). The same 1% is a different bar on each, and the named default Display declares gamut only.
  - **"Cold":** "Cold" is defined only as "after launch". Relaunching doesn't purge the OS file cache, so on an 8 GB Mac a 1.3 GB file may still be resident. Whether DF's open-file check (DF M8) is still running is not declared either.
  - **OS version:** R8.10f records build and machine but not the macOS version, and the research reports `Table` hangs specific to OS versions (v2:141). ADR-0006 leaves the floor open, so two release reads on the same Mac aren't comparable across an OS update.
  - **Estimator:** the kinds sampled 20 times (switch, swatch size, window moves) name no p95 estimator. By nearest rank, p95 of 20 samples is the 19th value.
- **Fix:**
  - Declare paging as N Page Down presses at a fixed cadence, or a scripted scroll at a fixed speed.
  - Read DROPPED_FRAME_SHARE at the display's refresh rate, and record that rate.
  - Define "cold" as after purging the file cache, with DF's check finished.
  - Add macOS version and refresh rate to R8.10f.
  - Name nearest-rank p95.

**[MINOR] PERF2-7 — R8.1 inputs with no timed sample**
- **Where:** prd:580–581, :584–585; jny:423.
- **Untimed inputs:**
  - Search, filter and sort in the grid (R8.1a names the grid).
  - Paging through the grid (R8.1e says table only; PERF-11's residual).
  - Toggling one row in or out of a large selection, such as after "Select all" (R6.1). This is the research's #1 symptom: a hang on selection, caused by the container's diff (v2:183).
  - "Use as scan order" at ROWS_CEILING (R8.1f). Low risk: about 10k position writes.
- **Fix:** in UJ9.5-a, add 200 toggles after "Select all". In R7's phase, send the timing workload in the grid and page it. Extend R8.1e to the grid.

**[MINOR] PERF2-8 — File-level limits aren't stated**
- **Where:** prd:574, prd:589, jny:423.
- **The gaps:**
  - R8.1's budgets hold "at ROWS_CEILING items in the collection". They say nothing about a file of up to FILE_ITEMS_CEILING items, and UJ9.5-a doesn't declare whether Scale's file holds other collections. A build that loads the whole file into memory passes on a file holding only Scale.
  - R8.2 has no rule for a file above FILE_ITEMS_CEILING, the counterpart of R8.1's above-the-ceiling sentence.
- **Fix:**
  - Change R8.1 to read "…in a file of up to FILE_ITEMS_CEILING items".
  - Run UJ9.5-a's collection inputs on UJ9.5-b's file too.
  - Give R8.2 the same above-ceiling sentence, with a function-only case at 2× FILE_ITEMS_CEILING.

**[NIT] PERF2-9 — Editorial**
- R8.1c's "or at its next showing" doesn't fit its "from when the write lands" clock (prd:582). Say that the next showing meets its own budget, already reflecting the write.
- R8.2 says Find similar meets "R8.1a–c's budgets", but firing Find similar isn't an R8.1a–c input kind (prd:589).
- The display-gamut change R2.5 budgets, and UJ9.5-a times, is not an R8.1 input kind, so M1's population leaves it out.

## Biggest risks   (what degrades first as data/traffic grows)
1. **Bulk delete or collection delete of a ceiling collection under byte erasure** (PERF2-1). Measured 3.05 s on an M5 Max, holding the only write lock. It breaches BULK_WRITE_BUDGET and R8.11 first.
2. **All items at 100k on the 8 GB M1 Air.** Round 1 measured a text sort at about 300 ms and a contains-scan at about 180 ms at 100k on the M5 Max. Only precomputed collation keys or an index will meet 100 ms. The lazy-tail shortcut is untested (PERF2-5).
3. **SwiftUI selection diff on large selections** (PERF2-7). The failure mode is inside ROWS_TARGET, and it depends on the OS version (v2:136, :141).
4. **"Gone from the bytes while open" (UJ6.4-j)** forces a TRUNCATE checkpoint after each commit, measured at about 3 ms. A long-lived reader, such as an All items read at 100k or an export, blocks it, so text lingers or the writer stalls. This is mostly the database and reliability lenses' concern.
5. **Scrolling the grid at 10k swatches.** Untimed; the research's symptom #4.

## Genuinely efficient   (incl. where simple-and-fast-enough is right that a perf-zealot would wrongly flag)
- **Bulk clear with byte erasure** takes about 130 ms at 10k items with 20 × 200-character columns on the M5 Max, well inside 2 s even allowing 3× for the M1 Air.
- **A one-field edit** costs about 8 ms to commit with full fsync, plus about 3 ms for a TRUNCATE checkpoint. At human edit rates, the byte rule costs nothing a user would see.
- **The 150 ms keystroke cadence** is longer than the budget, so every keystroke is a "last keystroke" that can be timed. It is well chosen.
- **"Every choice within OPEN_COLLECTION_BUDGET"** uses the maximum, not p95. That is correct for 20 samples.
- **UJ9.5-c** is function-only above the ceilings, with no budget promised. That is the right level.
- **Other settled points:**
  - Exhaustive Find similar at 100k (about 9.5 ms on the M5 Max) needs no index.
  - F90 keeps capture's FIND_BUDGET at or below BROWSE_RESPONSE_BUDGET, which is consistent.
  - A 500-reading history (UJ9.5-e) needs no paging.
  - R3.6 and R8.6 keep the browse path free of writes.
- **Sibling halves:** they add no new performance exposure except DF R2.3 and R6.2a's byte rule, whose cost is PERF2-1. Capture R3.7's fallback to the first pending row is a single linear pass.

## Missing / over-engineered   (premature optimization)
- **Missing:**
  - timing for bulk delete and collection delete at ROWS_CEILING, and a cost bound on ADR-0003's erase input (PERF2-1)
  - a case that actually makes capture and a bulk write compete (PERF2-2)
  - a readback for progress and for responsiveness during a bulk write (PERF2-4)
  - a case for the no-lazy-tail rule (PERF2-5)
  - declared paging input, refresh rate and OS version (PERF2-6)
- **Not over-engineered:** 1% dropped frames, 200 samples per kind, a 2 s bulk budget, and no mandated index or cache are all proportionate.
- **One caution:** don't make a bulk write that finishes inside BROWSE_RESPONSE_BUDGET show a progress frame (PERF2-4).

| Row ID | disposition |
|---|---|
| R1.1 | ALIGN |
| R1.2 | ABSTAIN |
| R1.3 | ABSTAIN |
| R1.4 | ABSTAIN |
| R1.5 | ABSTAIN |
| R1.6 | ABSTAIN |
| R1.7 | ABSTAIN |
| R1.8 | ABSTAIN |
| R1.9 | ALIGN |
| R1.10 | ALIGN |
| R2.1 | ALIGN |
| R2.2 | ABSTAIN |
| R2.3 | ALIGN |
| R2.4 | ALIGN |
| R2.4a | ALIGN |
| R2.4b | ABSTAIN |
| R2.4c | ABSTAIN |
| R2.4d | ABSTAIN |
| R2.4e | ABSTAIN |
| R2.4f | ABSTAIN |
| R2.4g | ABSTAIN |
| R2.4h | ABSTAIN |
| R2.4i | ABSTAIN |
| R2.5 | ALIGN |
| R2.6 | ALIGN |
| R2.7 | ABSTAIN |
| R2.8 | ABSTAIN |
| R2.9 | ALIGN |
| R2.10 | ABSTAIN |
| R2.11 | ALIGN |
| R3.1 | ALIGN |
| R3.2 | ALIGN |
| R3.3 | ALIGN |
| R3.4 | ALIGN |
| R3.5 | ABSTAIN |
| R3.6 | ALIGN |
| R3.7 | ALIGN |
| R3.8 | ABSTAIN |
| R3.9 | ALIGN |
| R4.1 | ALIGN |
| R4.2 | ABSTAIN |
| R4.2a | ABSTAIN |
| R4.2b | ABSTAIN |
| R4.2c | ABSTAIN |
| R4.2d | ABSTAIN |
| R4.2e | ABSTAIN |
| R4.2f | ABSTAIN |
| R4.2g | ABSTAIN |
| R4.2h | ABSTAIN |
| R4.3 | ALIGN |
| R4.4 | ABSTAIN |
| R4.5 | ABSTAIN |
| R4.6 | ABSTAIN |
| R4.7 | ALIGN |
| R4.8 | ABSTAIN |
| R4.9 | ABSTAIN |
| R5.1 | ALIGN |
| R5.2 | ABSTAIN |
| R5.2a | ABSTAIN |
| R5.2b | ABSTAIN |
| R5.2c | ABSTAIN |
| R5.2d | ABSTAIN |
| R5.2e | ABSTAIN |
| R5.3 | ABSTAIN |
| R5.4 | ALIGN |
| R5.5 | ABSTAIN |
| R5.6 | ABSTAIN |
| R5.7 | ALIGN |
| R5.8 | ABSTAIN |
| R6.1 | ALIGN |
| R6.2 | ALIGN |
| R6.3 | ABSTAIN |
| R6.4 | ALIGN |
| R7.1 | ALIGN |
| R7.2 | ALIGN |
| R8.1 | OBJECT (PERF2-1, PERF2-4, PERF2-5, PERF2-6, PERF2-7, PERF2-8) |
| R8.1a | OBJECT (PERF2-7) |
| R8.1b | OBJECT (PERF2-7) |
| R8.1c | OBJECT (PERF2-1) |
| R8.1d | OBJECT (PERF2-5, PERF2-6) |
| R8.1e | OBJECT (PERF2-6, PERF2-7) |
| R8.1f | OBJECT (PERF2-1, PERF2-4) |
| R8.2 | OBJECT (PERF2-5, PERF2-8) |
| R8.3 | ALIGN |
| R8.4 | ALIGN |
| R8.5 | ALIGN |
| R8.6 | ALIGN |
| R8.7 | ABSTAIN |
| R8.8 | ALIGN |
| R8.9 | ABSTAIN |
| R8.10 | OBJECT (PERF2-4, PERF2-6) |
| R8.10a | ALIGN |
| R8.10b | OBJECT (PERF2-4) |
| R8.10c | ALIGN |
| R8.10d | ALIGN |
| R8.10e | ABSTAIN |
| R8.10f | OBJECT (PERF2-6) |
| R8.11 | OBJECT (PERF2-1, PERF2-2, PERF2-3) |
| E1 | ABSTAIN |
| E2 | ABSTAIN |
| E3 | ABSTAIN |
| E4 | ABSTAIN |
| E5 | ABSTAIN |
| E6 | ABSTAIN |
| E7 | ABSTAIN |
| E8 | ABSTAIN |
| E9 | ABSTAIN |
| E10 | ABSTAIN |
| E11 | ABSTAIN |
| E12 | ABSTAIN |
| E13 | ABSTAIN |
| E14 | ABSTAIN |
| E15 | ABSTAIN |
| E16 | ABSTAIN |
| E17 | ABSTAIN |
| E18 | ABSTAIN |
| E19 | ABSTAIN |
| M1 | OBJECT (PERF2-6) |
| M2 | ABSTAIN |
| M3 | ABSTAIN |
| M4 | ABSTAIN |

### Round 2 — verify-the-reviewer dispositions (Blocker/Major; Critical→Blocker, High→Major)

Every Blocker and Major was checked against commit `7861221`. Privacy raised no Critical or High; its one Medium (PRIV-10: the P0 edit and delete cases read the bytes only after a clean close) is advisory and carried into the round-2 fix list.

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| 1 | E12 files Outside sRGB under "The chip isn't the true colour", false on a P3 display since F40 | product-marketing-manager (Blocker N-B1) | reproduced — copy lines 216, 298 | accept |
| 2 | E8's "you can undo this while the file stays open" outlives R4.7, whose history ends at any other committed action | product-marketing-manager (Blocker N-B2); product-manager (Major PM2-1); staff-software-engineer 2MN9 | reproduced — copy lines 170, 172 vs R4.7 | accept |
| 3 | No case asserts the value of the triplet the chip sends to the display; a chip converted from the stored clipped sRGB value passes on Display P3 | test (Blocker B1, carried PARTIAL) | reproduced — UJ2.1-c asserts fill and "not clipped" only | accept |
| 4 | A bulk delete or collection delete at ROWS_CEILING under byte erasure is untimed, "Delete collection" is in no R8.1 class, and the reviewer measured 3.05 s against the 2 s BULK_WRITE_BUDGET on its own stand-in schema | performance (PERF2-1); staff-software-engineer 2MN12; architecture A12 | reproduced — R8.1c/R8.1f leave "Delete collection" unclassed; UJ9.5-a times the clear only; the timing is the reviewer's measurement, not re-run | accept — owner decision |
| 5 | UJ9.5-d, R8.11's only case, rarely makes a capture save and the bulk write overlap, so a blocking build passes | performance (PERF2-2) | reproduced by reasoning — one ~130 ms clear against a ~3 s capture cycle | accept |
| 6 | R8.8's lost-permission branch renders a state the Data Foundation PRD owes and has not written; no Stop, no interim, no case | interface (R2-IF-1); product-manager PM2-3; architecture A20; staff-software-engineer 2MN14 | reproduced — post-lock.md item; Build dependencies row 1; the DF copy file has no such state; DF E10 and Capture E26 each say something false for this cause | accept — owner decision |
| 7 | The byte-erasure input handed to ADR-0003 ("gone from the file's bytes") is weaker than this PRD's rows and cases, which read sidecar files, while the file is open and after a crash | architecture (A16); staff-software-engineer 2MJ2(i) | reproduced — docs/decisions/README.md ADR-0003 row; DF R6.2a | accept — owner decision on the moment |
| 8 | On a single-writer file, R8.1f allows an atomic bulk write 2 s while R8.11 forbids delaying capture's saves; no row bounds the hold, and ADR-0005 has no input | staff-software-engineer 2MJ2(ii); architecture A18; performance PERF2-2 | reproduced — R8.1f vs R8.11 | accept — owner decision |
| 9 | R8.2's file-wide budgets state no imported-column width, though All items search matches imported values (F86) | staff-software-engineer 2MJ1; architecture A19 | reproduced — R8.2; UJ9.5-b declares no imported columns | accept — owner decision |
| 10 | The byte checks miss removed text surviving lower-cased, normalised, tokenised or in another encoding or a sidecar file | test (R2-M1) | reproduced — the Harness defines "holds text T nowhere" as a literal search | accept |
| 11 | Four of R3.9's seven change triggers, its "moves it" clause, and five of R6.4's deselect triggers have no case | test (R2-M2) | reproduced — cases cover edit, capture save and re-read only | accept |
| 12 | No readback exposes a mark's shape, yet UJ2.1-k and UJ9.6-a assert shapes | test (R2-M3); staff-software-engineer 2MN16 | reproduced — R8.10c grants identifiers and accessible descriptions only | accept |

**Per-row dispositions after round 2.** Flip-eligible (every non-abstaining lens ALIGNs, at least one opined): E1, E2, E7, E14, E15, E16, E18, E19, M3, R1.1, R1.2, R1.5, R1.6, R1.7, R1.8, R1.9, R2.1, R2.2, R2.4, R2.4a, R2.4c, R2.4d, R2.4e, R2.4f, R2.4g, R2.4h, R2.4i, R2.6, R2.7, R2.9, R2.10, R3.1, R3.2, R3.4, R3.6, R4.2a, R4.2d, R4.2e, R4.2f, R4.2g, R4.4, R4.6, R5.1, R5.2, R5.2a, R5.2c, R5.2d, R5.2e, R5.3, R5.6, R5.8, R6.1, R8.4, R8.7. A lettered sub-row flips only with its lead (the PRD's Traceability rule), so a sub-row whose lead or sibling sub-row is objected holds. Every other row carries at least one OBJECT and moves to needs-discussion until the round-2 fix pass lands.

## Round 3 (2026-09-25) — delta verification

**Lenses:** the eight Claude-route lenses, re-dispatched in parallel with the round-2 log and fix file as sources, asked per round-2 finding RESOLVED / UNRESOLVED / PARTIAL with file:line, then any new defect the round-2 fixes introduced (`git diff 7861221 HEAD`), then a per-row table; Blocker/Major reserved for a builder guessing or a correct build failing a case. **Subject commit:** `c4ec992`. No cross-model pass. **tier-rationale:** unchanged.

**Delta summary.** No Blocker was raised. Round-2 findings resolved per lens: product-manager 12 of 14 (2 partial); staff-software-engineer 30 of 33 (3 partial); test 20 of 25 (4 partial, 1 superseded by a new Major); interface 14 of 15 (IF-16 deferred to the bookkeeping close by design); privacy 5 of 7 (2 partial); product-marketing-manager 19 of 19; architecture 8 of 10 (2 partial); performance 11 of 12 (1 partial).

### Per-lens reviews (verbatim)

#### peer-product-manager-reviewer (Claude route)

## Verdict
Builds the right thing for the user. Of my 14 round-2 items (13 findings plus one carried), 12 are resolved and 2 are partly resolved. The fixes add one new Major, PM3-1: E8's rewritten undo sentence still promises more than R4.7 keeps, and R4.7's exception for capture can be built two ways. Fix it before lock. Everything else is Minor or Nit.

## User & problem context (brief)
- **User.** The primary user is the Cataloger. Secondary users are the Data consumer and the QC re-checker.
- **Job.** After capture, the user wants to see the whole collection honestly, find a swatch, fix a bad scan without losing its history, and keep metadata tidy (vision U5 and U7).
- **Validated.** The owner has decided F1–F134.
- **Assumed.** Everything else. The owner's own dogfooding is the only feedback loop, which is the right size for a solo open-source tool.
- **What I read.** At c4ec992: all four target files; fences F99–F134; the round-2 fix file; my round-2 section of the review log; and `git diff 7861221 HEAD -- docs ':!docs/agent-reviews'`. From the siblings: the changed Data Foundation lines (PRD, copy, journeys), the Capture Mode PRD and journeys, decisions/README.md and post-lock.md. I also read the capture PRD's R1.9 and R3.6 and its cause-label table.
- **Not re-read in full.** The Import, Export and Device PRDs. Round 2 did not change them.
- **Path shorthand.** All paths are under /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/:
  - PRD = docs/product/collection-mode/prd-collection-mode.md
  - COPY = docs/product/collection-mode/prd-collection-mode-copy.md
  - JRN = docs/product/collection-mode/prd-collection-mode-journeys.md
  - DFC = docs/product/data-foundation/prd-data-foundation-copy.md
  - CAP = docs/product/capture-mode/prd-capture-mode.md

## Findings

### Round-2 delta verification

| Round-2 finding | Result | Evidence |
|---|---|---|
| PM2-1 (Major) — E8 promises an undo R4.7 does not give | PARTIAL | COPY:173 and :175 now list closing the file, re-reading it, and "anything other than swatch details, codes or names". That still disagrees with R4.7 (PRD:496) about imports, column hide/show and capture-session writes. Carried forward as PM3-1. |
| PM2-2 (Minor) — the actions that end undo are open-ended, and what an undo reverses is invisible | PARTIAL | PRD:496 now closes the list: capture saves and session starts are exempt, a refused undo keeps its entry, and undo is offered only where the change's collection is shown. Cases: JRN:392, :395–:397. Two gaps remain. Capture writes other than a save are unclassified (PM3-1). The All items view is ambiguous (PM3-5). |
| PM2-3 (Minor) — the permission-lost state has no interim | RESOLVED | PRD:590 (R8.8 renders DF E34); PRD:300 (Build dependencies row 1); JRN:486 (UJ9.7-g); DFC:29; post-lock item ticked |
| PM2-4 (Minor) — displayed precision covers only some values | RESOLVED | PRD:448 (R2.11 covers every space R4.2c shows, including the sRGB and HSL forms); JRN:275 (UJ4.1-a) |
| PM2-5 (Minor) — M4 can pass vacuously | RESOLVED | PRD:738 (a session with nothing to answer reads "not measured") |
| PM2-6 (Minor) — Find similar loses its list | RESOLVED | PRD:474 (R4.1 returns to E9); JRN:257 (UJ3.4-e opens two results in turn). A new gap on delete is PM3-3. |
| PM2-7 (Minor) — the duplicate check ignores visible headers | RESOLVED | PRD:497 (R4.8 checks the Column headers table); JRN:314 (UJ4.6-j) |
| PM2-8 (Minor) — the collection-name search rule is unstated | RESOLVED | PRD:413 (matches anywhere in the name); COPY:135 (E4 "all items" variant); JRN:415, :418 |
| PM2-9 (Minor) — E5 blames the filters for an empty search | RESOLVED | PRD:461; JRN:238–239 (UJ3.2-d, UJ3.2-e) |
| PM2-10 (Nit) — R4.9 keyed to the wrong reading | RESOLVED | PRD:498 (keyed to the R2.4i mark in both clauses) |
| PM2-11 (Nit) — E17 names a P1 action at P0 | RESOLVED | COPY:267–269 (P0 body trimmed; "restore" variant is phase-marked); PRD:521 lists it; JRN:352 |
| PM2-12 (Nit) — "a re-import moves an item" | RESOLVED | PRD:101 now says "re-importing approximates"; the overstatement is gone |
| PM2-15 (Nit) — an open detail or history view doesn't update | RESOLVED | PRD:576 (R8.1c now reaches the item detail and version history view); JRN:459 (UJ9.1-l) |
| carried PM Minor 5 — precision for R4.2c's six spaces | RESOLVED | PRD:448 |

### New defects from the round-2 fixes

[MAJOR] **PM3-1** — COPY:173 and :175 (E8's new endings) against PRD:496 (R4.7) and CAP:160 (capture R3.6). **E8's undo promise still does not match R4.7, and R4.7's capture exception can be read two ways.**
- **(a) The copy promises more than the row keeps.** E8 now says Undo change "puts it back until the file closes, you read the file again, or you change anything other than swatch details, codes or names — scanning doesn't count." Three things in R4.7 contradict that:
  - An import that updates swatch details ends the history. R4.7 and UJ6.4-f (JRN:389) both say so, but E8's wording tells the user it does not.
  - Hiding or showing a column ends the history (F104). A user won't read "change anything other than swatch details" as covering a view toggle.
  - "Scanning doesn't count" covers only what R4.7 exempts: a capture save and a session starting.
- **(b) The capture exception is open to two builds.** The capture PRD writes the remembered row to the file on every queue advance, jump, insert or review selection (CAP:146, CAP:160). "Skip", "Flag", "Add a swatch", "End session" and the end-of-run review are all committed writes. A builder has to decide which of them count as "a capture save".
- **Scenario.** A cataloger mistakenly clears Family on 40 swatches. They start a session, skip one missing marker and end the session. Then they notice the mistake. Under a literal R4.7, the skip already ended the undo history. The old values existed only in memory, so they are gone for good, although E8 said scanning doesn't count.
- **Mutation.** End the history on the remembered-row write that a Skip makes. UJ6.4-m (JRN:396) still passes, because it only runs a save and a session start, yet E8's promise is broken after one Skip.
- **Fix (owner call; this clarifies F104 rather than reopening it).**
  - Either replace R4.7's "a capture save or a session starting not" with "no write a capture session makes". That is 2 words fewer and keeps E8's "scanning doesn't count" true.
  - Or keep R4.7 as it is and have E8 name the session actions that end the history.
  - Either way, key E8's ending to actions, not to what changed. Example: "…until the file closes or is read again, or you do anything but edit swatches' details, codes or names or rename a column or collection — importing and hiding a column included." This is a copy-file change, so it costs no body words.
  - Add a UJ6.4 case: edit, then Skip, Flag and End session, then check whether Undo change is still offered.

[MINOR] **PM3-2** — PRD:585 (R8.3) and COPY:155 (E6 "full"), against PRD:410 (R1.7) and JRN:49–51. **E6's "full" variant only appears once every P1 action R8.3 names is built, and one of those is E10's "Undo".**
- **The gap.** R1.7 may be deferred past v1 if OQ 10 is still open (F57). In that build "full" never renders. On the in-flight collection, "Change code", Flag, "Set a field", "Delete selected" and an undo of a code change then get the base E6 body. That body names only deleting and restoring. The harness also asserts the base body in that build.
- **Scenario.** A user fires "Set a field" during a session and is told that "deleting a swatch or the collection and bringing back an earlier reading aren't available". Nothing tells them the edit they just tried is blocked.
- **Fix.** Show "full" once every P1 action R8.3 names except E10's "Undo" is built. Move "undoing a delete" into its own clause or variant tied to R1.7. The body cost is 2 words in R8.3. Pay for it by cutting the Build contract's last sentence (PRD:111–112, 20 words). The Build-dependencies paragraph (PRD:312–316) and R8.7 already say the same.

[MINOR] **PM3-3** — PRD:474 (R4.1), JRN:257; PRD:576 (R8.1c). **The F107 return to E9 covers closing a result, not deleting or changing one.**
- **The gap.** R4.1's delete clause still sends the user to the table, so E9's list is lost. Deleting the duplicate is the most likely action when hunting duplicates. A Flag, restore or re-scan answer on a result brings the user back to an E9 that lists its old distance. E9 is not among the surfaces R8.1c updates, so a result that now has no current value still shows ΔE 2.04.
- **Scenario.** A cataloger finds three near-duplicates of FS-000 and deletes FS-001. They land on the table and have to reopen FS-000 and run Find similar again for each remaining match.
- **Fix (owner call).** Deleting a result opened from E9 returns to E9, and on return E9 is re-run for the same source. Add a case that deletes FS-002 from E9. The body cost is about 8 words in R4.1. Pay for it by cutting PRD:765–766 ("Every open question carries an interim rule… there is no third option.", 22 words). It repeats the Legend's two-list preamble at PRD:323–324.

[MINOR] **PM3-4** — PRD:585 (R8.3) against PRD:580 (R8.1g), PRD:607 (R8.11) and JRN:299 (UJ4.4-g). **E10's "Undo" of a large delete is not refused while a session runs elsewhere.**
- **The gap.** F100 refuses "Delete selected" and "Delete collection" on any collection while any session is in flight. R8.1g gives their E10 "Undo" the same 10 s DELETE_WRITE_BUDGET. But R8.3 refuses E10's "Undo" only "on that collection". A deleted collection can never have its own session in flight, so its undo is never refused.
- **What the builder must guess.** When R1.7 is built, R8.3 allows a write of up to 10 s during another collection's session, and R8.11 forbids its effect. The builder has to choose.
- **Scenario.** A user deletes Scale by mistake, starts scanning Studio Markers, then fires "Undo". Under a literal R8.3 the undo runs and holds up capture's saves.
- **Fix.** Move E10's "Undo" into R8.3's "on any collection" list (0 words), extending F100. Add a case alongside UJ4.4-g. Severity is Minor only because OQ 10 stops R1.7 from being built yet.

[NIT] **PM3-5** — PRD:496 (R4.7) against COPY:229 (E13's actions) and JRN:409 (UJ7.1-d). The All items view shows every collection's items, but E13 offers no "Undo change". So "offered only where its latest change's collection is shown" is ambiguous there. An edit made from All items can only be undone inside the item detail or from the collection surface. **Fix:** say that All items does not offer it, or add it to E13's actions. Both are copy-file or wording-only changes.

[NIT] **PM3-6** — JRN:395 (UJ6.4-l, first run). After "New collection" the user is on Inks, not Studio Markers. "Undo change" isn't offered there whether or not creating the collection ended the history, so the case can't catch a wrong build. **Fix:** choose Studio Markers before listing the actions. The test lens owns this.

[NIT] **PM3-7** — COPY:156 (E6 "elsewhere"). The copy says a bulk change "waits until it ends", which sounds like the change is queued. Queuing is the option F100 rejected, and the next sentence says nothing changed. **Fix:** "…can't be made until it ends…".

[NIT] **PM3-8** — DFC:29 (DF E34, new in this change). E34 offers only "Try again" and "OK". If permission can't be restored, each later edit shows E34 again with no way forward from the state. By contrast, E15 offers "Move my file" and E20/E29 offer "Choose somewhere else". **Fix:** offer the move route from DF's R1.9, as DF R7.3j does for E15.

## Biggest risks   (what builds the wrong thing or fails the user)
1. **PM3-1.** The one undo for a bulk clear lives only in memory. E8's wording and R4.7 still disagree about imports, hiding columns and scanning, and two correct-looking builds decide the session case differently.
2. **PM3-4.** When R1.7 is eventually built, a 10 s undo can run during another collection's session. That is exactly the stall F100 was written to prevent.
3. **Word budget.** The body is at 12,000 of 12,000. PM3-2 and PM3-3 need small additions, paid for by cutting PRD:111–112 and PRD:765–766. Both only repeat rules stated elsewhere; check each for rules before cutting.

## Genuinely solid   (incl. where simplicity is right that a product-zealot would over-spec)
- **The owner answers closed my round-2 items cleanly.**
  - E4 versus E5 now tells the user the true cause of an empty list.
  - DF E34 closes the permission-lost path, with a case.
  - M4 can no longer pass without measuring anything.
  - Displayed precision is complete, including the sRGB and HSL forms that Data consumers paste.
  - A refused undo keeps its entry, and undo is scoped to the collection on screen.
  - The E6, E17 and E18 phase variants follow one pattern.
- **Every copy state still traces to a row that puts the user there.** E4 "all items" comes from R3.5; E6 "elsewhere" and "full" from R8.3; E17 "restore" from R5.3; E18 "restore" from R4.9. Every variant name is listed word for word by the row that enumerates it.
- **Sibling halves agree.**
  - DF E11 and E26 now say "awaiting your answer", matching E12's mark.
  - Capture R5.8 and R6.6 match its cause-label table.
  - Capture's obligations line matches F100 word for word.
  - The ADR-0003 row takes the seven inputs.
  - post-lock.md ticks the DF item and adds the Device and help-docs lines.
- **Right-sized.** Refusing all bulk writes while any session runs is simple and testable (F100). Scoping undo to a collection, with no redo, is the right v1 choice. One dogfood metric plus one E12 read-through on a P3 display is enough for a single-owner tool.

## Missing / over-specified
- **Missing:**
  - which capture-session writes end the undo history (PM3-1)
  - E9's behaviour on return after a result is deleted or changed (PM3-3)
  - E10's "Undo" while a session runs on another collection (PM3-4)
  - whether the All items view offers "Undo change" (PM3-5)
- **Over-specified:**
  - E6 "full" is tied to R1.7, which may never ship in v1 (PM3-2).
  - Nothing else new. The fence discipline is carrying the load well.

| Row ID | disposition |
|---|---|
| R1.1 | ALIGN |
| R1.2 | ALIGN |
| R1.3 | ALIGN |
| R1.4 | ALIGN |
| R1.5 | ALIGN |
| R1.6 | ALIGN |
| R1.7 | ALIGN |
| R1.8 | ALIGN |
| R1.9 | ALIGN |
| R1.10 | ALIGN |
| R2.1 | ALIGN |
| R2.2 | ALIGN |
| R2.3 | ALIGN |
| R2.4 | ALIGN |
| R2.4a | ALIGN |
| R2.4b | ALIGN |
| R2.4c | ALIGN |
| R2.4d | ALIGN |
| R2.4e | ALIGN |
| R2.4f | ALIGN |
| R2.4g | ALIGN |
| R2.4h | ALIGN |
| R2.4i | ALIGN |
| R2.5 | ALIGN |
| R2.6 | ALIGN |
| R2.7 | ALIGN |
| R2.8 | ALIGN |
| R2.9 | ALIGN |
| R2.10 | ALIGN |
| R2.11 | ALIGN |
| R3.1 | ALIGN |
| R3.2 | ALIGN |
| R3.3 | ALIGN |
| R3.4 | ALIGN |
| R3.5 | ALIGN |
| R3.6 | ALIGN |
| R3.7 | ALIGN |
| R3.8 | ALIGN |
| R3.9 | ALIGN |
| R4.1 | OBJECT (PM3-3) |
| R4.2 | ALIGN |
| R4.2a | ALIGN |
| R4.2b | ALIGN |
| R4.2c | ALIGN |
| R4.2d | ALIGN |
| R4.2e | ALIGN |
| R4.2f | ALIGN |
| R4.2g | ALIGN |
| R4.2h | ALIGN |
| R4.3 | ALIGN |
| R4.4 | ALIGN |
| R4.5 | ALIGN |
| R4.6 | ALIGN |
| R4.7 | OBJECT (PM3-1, PM3-5, PM3-6) |
| R4.8 | ALIGN |
| R4.9 | ALIGN |
| R5.1 | ALIGN |
| R5.2 | ALIGN |
| R5.2a | ALIGN |
| R5.2b | ALIGN |
| R5.2c | ALIGN |
| R5.2d | ALIGN |
| R5.2e | ALIGN |
| R5.3 | ALIGN |
| R5.4 | ALIGN |
| R5.5 | ALIGN |
| R5.6 | ALIGN |
| R5.7 | ALIGN |
| R5.8 | ALIGN |
| R6.1 | ALIGN |
| R6.2 | ALIGN |
| R6.3 | ALIGN |
| R6.4 | ALIGN |
| R7.1 | ALIGN |
| R7.2 | ALIGN |
| R8.1 | ALIGN |
| R8.1a | ALIGN |
| R8.1b | ALIGN |
| R8.1c | ALIGN |
| R8.1d | ALIGN |
| R8.1e | ALIGN |
| R8.1f | ALIGN |
| R8.1g | ALIGN |
| R8.2 | ALIGN |
| R8.3 | OBJECT (PM3-2, PM3-4) |
| R8.4 | ALIGN |
| R8.5 | ALIGN |
| R8.6 | ALIGN |
| R8.7 | ABSTAIN (platform floor; out of lens) |
| R8.8 | ALIGN |
| R8.9 | ALIGN |
| R8.10 | ABSTAIN (test seam; test/interface lens) |
| R8.10a | ABSTAIN (test seam; test/interface lens) |
| R8.10b | ABSTAIN (test seam; test/interface lens) |
| R8.10c | ABSTAIN (test seam; test/interface lens) |
| R8.10d | ABSTAIN (test seam; test/interface lens) |
| R8.10e | ABSTAIN (test seam; test/interface lens) |
| R8.10f | ABSTAIN (test seam; test/interface lens) |
| R8.11 | ALIGN |
| E1 | ALIGN |
| E2 | ALIGN |
| E3 | ALIGN |
| E4 | ALIGN |
| E5 | ALIGN |
| E6 | OBJECT (PM3-2, PM3-7) |
| E7 | ALIGN |
| E8 | OBJECT (PM3-1) |
| E9 | ALIGN |
| E10 | ALIGN |
| E11 | ALIGN |
| E12 | ALIGN |
| E13 | ALIGN |
| E14 | ALIGN |
| E15 | ALIGN |
| E16 | ALIGN |
| E17 | ALIGN |
| E18 | ALIGN |
| E19 | ALIGN |
| M1 | ALIGN |
| M2 | ALIGN |
| M3 | ALIGN |
| M4 | ALIGN |

#### peer-staff-software-engineer-reviewer (Claude route)

## Verdict
Ready to proceed, but settle the two new MAJORs before lock. There are no Blockers. Of my lens's 33 round-2 items, 30 are resolved and 3 are partly resolved. F99–F134 and their sibling halves landed as recorded, and I found no row that re-argues a fence. What the fixes opened:
- **R8.1's lead row applies F118's reading bound to every budget.** F118 meant it for the history view only.
- **F100 closes only one ordering of the single-writer race.** A session can still start while a bulk write or delete of up to 10 s is running.

## What I reviewed
- **Paths.** All paths below are relative to `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/`.
- **Subject.** Requirements mode at `c4ec992` (HEAD). I read in full:
  - `docs/product/collection-mode/prd-collection-mode.md`
  - `docs/product/collection-mode/prd-collection-mode-journeys.md`
  - `docs/product/collection-mode/prd-collection-mode-copy.md`
  - `docs/product/collection-mode/prd-collection-mode-oq-results.md`
- **Contract sources.**
  - Fences F99–F134 in `-fences.md`.
  - `-round-2-fixes.md`, in full.
  - My Round 2 section of `docs/agent-reviews/2026-09-24-prd-collection-mode-peer-reviews.md`.
  - The sibling diffs from `git diff 7861221 HEAD -- docs ':!docs/agent-reviews'`: DF F53 (R1.10, R2.3, R3.4, R6.2a, R7.3j, R7.6p, E11, E26, E33, E34, OQ 20); Capture F72 and the F71 clarification (R5.8, R6.6, UJ3.3-k, UJ3.3-l); the ADR-0003 row; `post-lock.md`.
  - Capture R3.2, R3.5, R9.7, OQ 5, OQ 13; DF R7.7; the capture copy file's Set-aside cause labels table.
- **Checks I computed** in memory with python; no file was touched:
  - Linear sRGB and Display P3 membership of every seeded or case-declared L\*C\*h°, D50 to D65 by Bradford.
  - ZX-019's margin under four matrix and white-point variants.
  - UJ2.1-c's P3 triplet.
  - The sort, count and ΔE oracles of the new cases.
  - The word-count delta between `7861221` and HEAD.
- **Could not verify:**
  - Any timing.
  - Real ColorSync behaviour.
  - SQLite secure-delete and WAL/checkpoint behaviour under F102.
  - How large and how random the seeded file really is (3MN8).
  - M3 and anything else that needs a build.

## Findings

### Round-2 delta verification (my lens)

| Round-2 finding | Status | Evidence |
|---|---|---|
| 2MJ1 file-wide budgets state no width | RESOLVED | prd:584 (R8.2 at IMPORTED_COLUMNS_CEILING, first rows "as R8.1d does"); journeys:472 (UJ9.5-b declares the width), 418 (UJ7.1-m: an imported-value search from All items), 108 (the workload's imported-value text) |
| 2MJ2 storage constraints not handed to ADR-0003 | PARTIAL | (i) resolved: prd:174–175 (the file's bytes include side files), prd:590 (R8.8 "from the moment it lands, open or after a crash"); DF prd:116, 230; decisions README:22. (ii) resolved for a bulk write started while a session is in flight (prd:585, F100). Still open: a session starting, or another write landing, while a bulk write or delete runs → **3MJ2** |
| 2MN1 collection-name match mode, E4 | RESOLVED | prd:413; copy:135; journeys:415, 418 |
| 2MN2 which derived set's clipped flag | RESOLVED | prd:431 (R2.4b), prd:442 (R2.5) |
| 2MN3 R3.3's like set | PARTIAL | In-collection, "listed" and never-like are resolved: prd:459; journeys:252, 419. Whether a mismatched non-spectral reading counts toward the majority is still open → **3MN5** |
| 2MN4 Find similar with the working-set value absent | RESOLVED | prd:463–464, 483; journeys:259–260 |
| 2MN5 what ends the undo history | PARTIAL | prd:496 closes the set for Collection Mode actions. Capture's writes other than a save or a session start are still unclassed → **3MN4** |
| 2MN6 false not-compared reason | RESOLVED | prd:522; journeys:350 |
| 2MN7 "Use this reading" on a never-true reading | RESOLVED | prd:523; journeys:351 |
| 2MN8 how "Set a field" commits and cancels | RESOLVED | prd:535; journeys:377. E8's own Return default is still unstated → 3N2 |
| 2MN9 E8's undo over-promise | RESOLVED | copy:173, 175. A capture-write residual → 3MN4 |
| 2MN10 per-collection grid and swatch size | RESOLVED | prd:546–547; journeys:503 |
| 2MN11 "cold" and paging | RESOLVED | journeys:108; prd:752 |
| 2MN12 deleting a ROWS_CEILING collection | RESOLVED | prd:279, 580 (R8.1g); journeys:477 |
| 2MN13 bound on readings for history open | RESOLVED in substance | prd:280. The bound is placed in R8.1's lead row → **3MJ1** |
| 2MN14 no interim for lost permission | RESOLVED | prd:590; DF copy:29 (E34); journeys:486 |
| 2MN15 R8.11's TBD with no interim | RESOLVED | prd:330, 752 (F120) |
| 2MN16 no readback for shape or cause | RESOLVED | prd:599–600; journeys:277, 315, 478 |
| 2MN17 precision of R4.2c's six spaces | RESOLVED | prd:448; journeys:275 |
| 2MN18 DF E11/E26 "not yet settled" | RESOLVED | DF copy:38–39 |
| 2MN19 Capture R5.8/R6.6 cause wording | RESOLVED | Capture prd:215, 233 |
| 2N1 no route for an item removed outside the app | RESOLVED | prd:144, 226–227; journeys:133 (T16) |
| 2N2 E10's surfaces | RESOLVED | prd:372, 378–379, 693 |
| 2N3 All-items Find similar tie | RESOLVED | prd:463; journeys:261 (UJ3.4-i) |
| 2N4 ⟨distance⟩ places | RESOLVED | prd:720 |
| 2N5 fixture margins, two "Daylight"s | RESOLVED | journeys:61–70, 414 |
| 2N6 ADR-0006 stops every row | RESOLVED | prd:312–316 |
| carried MJ2 / MJ8 / MN9 / MN17 | RESOLVED | covered by 2MN18 / 2MJ1, 2MN12 and 2MN13 / PM2-3 / PM2-4, as above |
| unrated: "Columns" label | RESOLVED | prd:447; copy:124; journeys:217 |
| unrated: "That E8" back-references | RESOLVED | journeys:296, 412 |

### New defects introduced by the round-2 fixes

**[MAJOR] 3MJ1 — R8.1's lead row applies HISTORY_READINGS_CEILING to every budget, not to history opens.**
- **Locations.** prd:568; prd:280; fences:1076–1080 (F118); the round-2 fix file:150.
- **The gap.**
  - R8.1 says every input below meets its budget "at ROWS_CEILING items in the collection with up to IMPORTED_COLUMNS_CEILING imported columns and HISTORY_READINGS_CEILING readings an item".
  - The constants table (prd:280) and F118 ("A bound on readings for R8.1b's history-open budget") scope the bound to history opens only. Because the Legend makes the owning row govern (prd:288), the lead row's wider scope wins.
- **The two ways it fails.**
  - *Too much is promised.* R8.1g would need a byte-scrubbed "Delete collection" of 10,000 × 500 = 5×10⁶ readings within 10 s on an 8 GB M1 Air. The performance lens measured 3.05 s at about one reading per item. A builder taking the row at its word is pushed toward a storage decision (per-collection encryption keys, or a scrub deferred against F102) that only a misplaced constant justifies.
  - *Too little is promised.* One item with 501 readings takes its whole collection "above any ceiling", so search, sort and open lose their budgets.
- **Mutation.** A build that meets R8.1a–g only at one reading per item stays green. UJ9.5-a and UJ9.5-g seed DF R7.7's corpus, and only UJ9.5-e (journeys:475) holds 500 readings, on a single item.
- **Fix.**
  - Delete "and HISTORY_READINGS_CEILING readings an item" from R8.1's lead (−5 words).
  - In R8.1b, write "opening or closing an item detail, or a version history view of up to HISTORY_READINGS_CEILING readings" (+7 words).
  - Pay for it by trimming R8.2's ", history never being aged out", which restates "every reading it holds, however many" (−5 words). Net −3 words.

**[MAJOR] 3MJ2 — Nothing orders other writes, or a session's start, against a bulk write or delete already running (up to DELETE_WRITE_BUDGET = 10 s). This is the 2MJ2 (ii) residual.**
- **Locations.**
  - prd:579–580: R8.1f/g say "the surface meeting R8.1a–c throughout", and R8.1c includes single-item writes.
  - prd:422: R2.2 keeps every capture entry point "reachable and unhidden".
  - prd:585: R8.3's refusal is checked only when the bulk action is fired.
  - prd:607: R8.11.
  - Capture F72: "Nothing in this PRD's rows changes."
  - journeys:474: UJ9.5-d starts with the session already in flight.
  - journeys:477: UJ9.5-g runs with "no session in flight".
- **Scenario A.** Fire "Delete collection" on Scale, confirm it, and during the 10 s delete choose another collection and start a session. On a single-writer file the first row confirmation waits for the delete to commit, which breaks ROW_CONFIRM_BUDGET and fails R8.11.
- **Scenario B.** Start a session on Scale itself, or edit or rename it, during its own deletion. The delete then lands under an in-flight session, which R1.4 and F100 exist to forbid, and the queued edit targets a row that no longer exists.
- **A second gap in UJ9.5-g.** Its "Delete collection" run types a search keystroke into a surface no row names. The collection list has no search field.
- **Mutation.** A build that lets a session start mid-delete passes UJ9.5-d, UJ9.5-g and UJ1.2-a/f.
- **Fix (owner decision; F100 is the precedent).**
  - Add to R8.3: "While a bulk write or delete this PRD makes runs, no session starts or resumes and no other write this PRD offers starts, each shown disabled until it lands (inherited obligation for the Capture Mode doc)." That is about 30 words.
  - Pay for it by trimming §8's preamble paragraph (prd:561–564, about 47 words). It restates the section's guidance comment and gives the builder no rule, if agent-prd v1 doesn't fix that paragraph.
  - Add a dated Capture line under its F72, and put the same question to Import (whether an import commit waits too).
  - UJ9.5-g should name the surface shown during a collection delete and try a session start mid-write.

**[MINOR] 3MN1 — E10's "Undo" of a selection or collection delete is refused only on "that collection" (prd:585).**
- R8.1g (prd:580) puts that undo in the same DELETE_WRITE_BUDGET class as the deletes that F100 refuses on any collection. A deleted collection can hold no session, so the refusal never fires for a collection undo, and a restore lasting seconds runs against another collection's session, against R8.11.
- An undo of a bulk set or clear in flight is refused only by combining R4.7's re-check with R8.3, and no case covers it.
- **Fix.** Move E10's "Undo" into R8.3's "on any collection" group; the reorder is word-neutral. Add a UJ6.4 case: undo a 10-item clear while a session is in flight on Gouache Set, expecting E6 "elsewhere".

**[MINOR] 3MN2 — Which E6 variant wins is unstated.**
- **Locations.** prd:407 (R1.4), prd:585; copy:154, 156; Capture prd:156 (R3.2), 159 (R3.5), R9.7.
- **Collision 1.** Collection A can hold an interrupted bulk session while B's session is in flight. "Delete collection" on A then meets both "interrupted" and "elsewhere".
- **Collision 2.** A one-row session can be in flight on A beside A's interrupted session. That pits "interrupted" against the body or "full".
- The variants' "Go to the session" leads to different sessions, and resuming A is refused while B is active.
- The round-2 trim of R1.4 to "R8.3 governing it in flight" removed the only hint.
- **Fix (companion only).** Give each variant condition an order, for example "elsewhere" and in-flight before "interrupted", and add a UJ1.2 case for each collision.

**[MINOR] 3MN3 — E6's "full" variant waits on an action OQ 10 may defer past v1.**
- **Locations.** prd:585; copy:155; prd:410.
- "full" renders once every P1 action R8.3 names lands, and one of them is E10's "Undo".
- If R1.7 is deferred at v1, "full" never renders. A refused "Set a field", "Change code", Flag or "Use this reading" then shows the P0 body, which names only deletes and restores. The "full" text would also claim that undoing a delete exists.
- **Fix.** R8.3: "once every P1 action it names lands or is deferred" (+3 words, paid from the §8 paragraph). The "full" body names undoing a delete only where R1.7 is built.

**[MINOR] 3MN4 — R4.7 exempts only "a capture save or a session starting" (prd:496).**
- A session's other writes all end the undo history under the literal row: a skip, a set-aside by the guard or by capture's Flag, a pause, an end or resume, and a queue-list reorder.
- E8 promises "scanning doesn't count" (copy:173, 175).
- UJ6.4-i (journeys:392) expects "Undo change" still offered after the session ends.
- **Fix (owner, refining F104).** Replace the clause with "nothing a session writes doing so" (−2 words). Add a case that skips a row mid-session and then undoes.

**[MINOR] 3MN5 — R3.3's majority count is still undecided (2MN3 residual; prd:459).**
- "The most common among listed items with a value" doesn't say whether a mismatched non-spectral reading votes under its value's pair, votes under its collection's pair, or doesn't vote.
- UJ7.1-n (journeys:419) gives the same order under all three readings.
- Two collections of 2 items each, plus one D65/10° non-spectral item in the D50/2° collection, give a different like pair under each reading.
- **Fix.** Add that case. Once the owner reads F113's "never", add "each item voting under its value's pair" or the chosen alternative (+6 words, paid from the §8 paragraph).

**[MINOR] 3MN6 — Release-build timing cases read a readback that exists only in test builds.**
- UJ9.5-e and UJ9.5-g run "a Release build" (journeys:475, 477) but assert through R8.10b: E17's list, and whether a write's progress shows.
- F132 makes R8.10b exist only in test builds (prd:592).
- UJ9.4-e (journeys:470) treats "Release build" and "test build" as different builds.
- **Fix (Harness only).** Timing cases run a Release-configuration test build, and UJ9.4-e's Release build is the shipped one.

**[MINOR] 3MN7 — Two E17 cases contradict each other once R5.5 lands.**
- UJ5.1-a and UJ5.1-b assert E17's body with no variant (journeys:330–331).
- UJ5.3-n uses the same Given and When and asserts the "restore" variant (journeys:352), which F111 made the recorded-order text once R5.5 is built.
- The Harness gives E6 a substitution rule (journeys:49–51); E17 has none. UJ4.7-a handles E18 inline.
- **Fix (companion only).** Add "where R5.5 is built, a case asserting E17's body asserts its 'restore' variant".

**[MINOR] 3MN8 — The byte check's per-word rule can match raw binary by chance (journeys:53–59).**
- "Sky" is the unique word of "Sky Blue" in T6 and UJ4.2-a. Its 8 letter-case variants match any given byte offset with probability of about 4.8×10⁻⁷.
- Across the file, the WAL and the high-entropy raw-payload and float blobs, at three moments per case, a correct build fails T6 and UJ4.2-a at an estimated few percent per run. The magnitude is unverified.
- **Fix.** Apply the per-word rule to SQLITE_READER_FLOOR's text values, and to raw bytes only for words of four or more letters. Hand this to the test lens.

**[NIT] 3N1 — R8.1f's "or an undo of one of them" (prd:579) includes "Use as scan order", which R4.7 never undoes.** Pre-existing. Fix: "or undoing a set or clear" (word-neutral).

**[NIT] 3N2 — E8's default key is unstated.** R6.2 now says Return applies (prd:535), but no row says what Return fires inside E8 (copy:174). R1.6's cancel-by-default covers deletes only, so two presses of Return clear ⟨n⟩ values; undo can still restore them. Fix: state it, as R1.6 does for deletes.

**[NIT] 3N3 — R4.1 returns to "E9 with its list" (prd:474).** A restore made in the opened detail changes that item's value, so the list and ΔE shown on return are stale. R3.9 governs only the table. Fix: say "E9 recomputed", or add a case.

## Clarifying questions for the author
1. Does HISTORY_READINGS_CEILING bound only R8.1b's history open, as F118 and the constants table say, or every R8.1 budget?
2. While a bulk write or delete runs, may a session start or resume, on that collection or another? If so, does the write yield, or does the start wait?
3. While a bulk write or delete runs, are this PRD's other writes offered, queued or refused? What happens to an edit to an item that the running delete removes?
4. What surface shows while "Delete collection" runs?
5. Is E10's "Undo" of a selection or collection delete refused while any session is in flight, as the deletes are?
6. When collection A holds an interrupted session and a session is in flight on B, or a one-row session is in flight on A, which E6 variant does "Delete collection" on A render?
7. If R1.7 is deferred at v1, does E6's "full" render once every other P1 action it names lands?
8. Do a session's writes other than a save (skip, set aside, pause, end, resume, queue-list reorder) end "Undo change" history?
9. In the All-items majority, does a mismatched non-spectral reading vote under its value's pair, under its collection's pair, or not at all?
10. Do timing cases run a Release-configuration test build that carries R8.10b?
11. Does every case keep running in later build phases?
12. In E8, which action does Return fire?
13. Is E9's list recomputed when a detail opened from it closes after a restore?
14. For the Import PRD: is an import commit refused while a session is in flight on another collection, as F100 refuses this PRD's bulk writes?

## Claimed properties
- **Every box for my lens in the round-2 fix file is ticked — holds.** Three items are PARTIAL on substance (see the table).
- **Body at 12,000 words by rule 14's method — holds within method.** My raw-token recount moves by +15 from `7861221`, matching 11,985 → 12,000. There is no headroom, so every body fix above names its trim.
- **Every P0 row has an interim; the no-interim list is "None" — holds.** R8.11 sits under OQ 1's interim (F120), and R8.8 renders DF E34, which now exists (DF copy:29).
- **Every own constant has a candidate, an interim and an OQ — holds** for all 12, DELETE_WRITE_BUDGET and HISTORY_READINGS_CEILING included. The latter's owning row over-scopes it (3MJ1).
- **Gamut margins — hold.**
  - Every other declared value is at least 0.03 inside or outside. The smallest are GS-002 at +0.038 and ZX-014 at +0.044. The exemptions are DL-001 and BI-1, each at about +0.016.
  - ZX-019 is inside sRGB by +0.00056 to +0.00059 under the primaries-derived matrix, the IEC 61966-2-1 rounded matrix and the ICC D50-adapted matrix, with either D50 white. UJ2.1-q's "not clipped" survives any of them.
- **UJ2.1-c's triplet (64.7, 196.8, 103.6)/255 — holds** exactly under OQ 7's interim.
- **The new oracles hold:**
  - UJ3.4-f: GB-3 at 3.0035, analytically, with S_L ≈ 1.
  - UJ3.2-e: 7 hidden.
  - UJ3.3-l and UJ3.3-m.
  - UJ3.4-h and UJ3.4-i.
  - UJ7.1-n.
  - UJ9.1-e/f/h/i/k.
  - UJ4.4-f.
- **The ADR-0003 row "takes seven inputs" — holds.** I counted seven distinct inputs.
- **Sibling halves landed — holds:** DF F53 and its rows, Capture F72 and the F71 clarification, post-lock.
- **F100 removes capture/bulk-write contention — does not hold.** It fails for a session that starts or resumes mid-write (3MJ2).
- **No row contradicts F1–F134 — holds, except** that R8.1's lead row reaches past F118's scope (3MJ1).
- **Unverified:** all timings, ColorSync, SQLite secure-delete and checkpoint behaviour under F102, 3MN8's rate, M3.

## Genuinely sound
- **F100 is the right simplification.** Refusing bulk writes in flight is testable, and UJ9.5-d is now a P0 case against the edits that stay available. It beats an engineered yield or queue under ADR-0005; 3MJ2 asks only for the reverse ordering.
- **F102 is defined once, in the Vocabulary's "the file's bytes".** R1.3, R4.3, R6.2 and R8.8 inherit it, and UJ9.8-c is a real control the check must catch.
- **DELETE_WRITE_BUDGET is a separate constant**, rather than 2 s being stretched or F35's scrub being deferred.
- **F127 makes the stored flag and the sRGB display check one computation**, and UJ2.1-q is an edge that genuinely survives implementation variance.
- **The verification seam exists only in test builds and opens no socket (F132), with UJ9.4-e.** That is right-sized for a single-user local app.
- **F110's E4/E5 split and E4's "all items" variant** are deterministic and cheap.
- **The round's trims kept the body at 12,000 words.** They removed research and fence-provenance sentences, not rules.

## Deferred
- **peer-architecture-reviewer:**
  - 3MJ2's mechanism: single-writer arbitration, and whether a bulk write can yield without breaking R8.8's atomicity.
  - F102 under WAL: a checkpoint after each write against capture's concurrent reads.
  - An import commit while a session is in flight (question 14).
- **peer-performance-reviewer:**
  - That 3MJ1's literal envelope is infeasible at 500 readings per item.
  - Whether R8.1g's 10 s holds with the F102 scrub at ROWS_CEILING.
- **peer-test-reviewer:**
  - 3MN6, 3MN7 and 3MN8.
  - UJ9.5-f has no functional timeout.
- **peer-product-marketing-manager-reviewer:**
  - E6's "elsewhere" copy says a change "waits until it ends", which reads as a queued change while R8.3 changes nothing.
  - E8's "scanning doesn't count" against R4.7 (3MN4).
- **peer-interface-reviewer:** E8's Return default (3N2).

| Row ID | disposition |
|---|---|
| R1.1 | ALIGN |
| R1.2 | ALIGN |
| R1.3 | ALIGN |
| R1.4 | OBJECT (3MN2) |
| R1.5 | ALIGN |
| R1.6 | ALIGN |
| R1.7 | ALIGN |
| R1.8 | ALIGN |
| R1.9 | ALIGN |
| R1.10 | ALIGN |
| R2.1 | ALIGN |
| R2.2 | ALIGN |
| R2.3 | ALIGN |
| R2.4 | ALIGN |
| R2.4a | ALIGN |
| R2.4b | ALIGN |
| R2.4c | ALIGN |
| R2.4d | ALIGN |
| R2.4e | ALIGN |
| R2.4f | ALIGN |
| R2.4g | ALIGN |
| R2.4h | ALIGN |
| R2.4i | ALIGN |
| R2.5 | ALIGN |
| R2.6 | ALIGN |
| R2.7 | ALIGN |
| R2.8 | ALIGN |
| R2.9 | ALIGN |
| R2.10 | ALIGN |
| R2.11 | ALIGN |
| R3.1 | ALIGN |
| R3.2 | ALIGN |
| R3.3 | OBJECT (3MN5) |
| R3.4 | ALIGN |
| R3.5 | ALIGN |
| R3.6 | ALIGN |
| R3.7 | ALIGN |
| R3.8 | ALIGN |
| R3.9 | ALIGN |
| R4.1 | OBJECT (3N3) |
| R4.2 | ALIGN |
| R4.2a | ALIGN |
| R4.2b | ALIGN |
| R4.2c | ALIGN |
| R4.2d | ALIGN |
| R4.2e | ALIGN |
| R4.2f | ALIGN |
| R4.2g | ALIGN |
| R4.2h | ALIGN |
| R4.3 | ALIGN |
| R4.4 | ALIGN |
| R4.5 | ALIGN |
| R4.6 | ALIGN |
| R4.7 | OBJECT (3MN4) |
| R4.8 | ALIGN |
| R4.9 | ALIGN |
| R5.1 | ALIGN |
| R5.2 | ALIGN |
| R5.2a | ALIGN |
| R5.2b | ALIGN |
| R5.2c | ALIGN |
| R5.2d | ALIGN |
| R5.2e | ALIGN |
| R5.3 | ALIGN |
| R5.4 | ALIGN |
| R5.5 | ALIGN |
| R5.6 | ALIGN |
| R5.7 | ALIGN |
| R5.8 | ALIGN |
| R6.1 | ALIGN |
| R6.2 | OBJECT (3N2) |
| R6.3 | ALIGN |
| R6.4 | ALIGN |
| R7.1 | ALIGN |
| R7.2 | ALIGN |
| R8.1 | OBJECT (3MJ1) |
| R8.1a | ALIGN |
| R8.1b | OBJECT (3MJ1) |
| R8.1c | ALIGN |
| R8.1d | ALIGN |
| R8.1e | ALIGN |
| R8.1f | OBJECT (3MJ2, 3N1) |
| R8.1g | OBJECT (3MJ2) |
| R8.2 | ALIGN |
| R8.3 | OBJECT (3MJ2, 3MN1, 3MN2, 3MN3) |
| R8.4 | ALIGN |
| R8.5 | ALIGN |
| R8.6 | ALIGN |
| R8.7 | ALIGN |
| R8.8 | ALIGN |
| R8.9 | ALIGN |
| R8.10 | OBJECT (3MN6) |
| R8.10a | ALIGN |
| R8.10b | OBJECT (3MN6) |
| R8.10c | ALIGN |
| R8.10d | ALIGN |
| R8.10e | ALIGN |
| R8.10f | ALIGN |
| R8.11 | OBJECT (3MJ2) |
| E1 | ALIGN |
| E2 | ALIGN |
| E3 | ALIGN |
| E4 | ALIGN |
| E5 | ALIGN |
| E6 | OBJECT (3MN2, 3MN3) |
| E7 | ALIGN |
| E8 | OBJECT (3MN4, 3N2) |
| E9 | ALIGN |
| E10 | ALIGN |
| E11 | ALIGN |
| E12 | ALIGN |
| E13 | ALIGN |
| E14 | ALIGN |
| E15 | ALIGN |
| E16 | ALIGN |
| E17 | OBJECT (3MN7) |
| E18 | ALIGN |
| E19 | ALIGN |
| M1 | ALIGN |
| M2 | ALIGN |
| M3 | ALIGN |
| M4 | ABSTAIN (out of lens) |

3MN8 targets the Harness's byte-check rule, which carries no row ID. Question 14 targets the Import PRD.

#### peer-test-reviewer (Claude route)

## Verdict
Trustworthy once three new Majors are fixed; no Blocker remains. Of my four round-2 Blocker and Major findings, three are resolved and one is partly resolved, and every new fixture number I recomputed holds. The round-2 fixes did add three Majors:
- **Undos of bulk writes stay available while a session is in flight**, against F100 and R8.11.
- **"File's bytes equal" checks fail correct builds.** A correct SQLite build fails them. My own round-2 fix text caused this.
- **UJ5.1-a fails a correct build** from the phase that lands R5.5.

## Coverage map (brief)
- **Now covered:**
  - Every R3.9 trigger, and its "moves it" clause.
  - The Display P3 colour values the chip sends, and a chip on an item with several readings.
  - A byte check that handles letter case, encoding and side files, with a control case.
  - Lost file permission.
  - R8.11 in the first build phase.
  - Bulk and collection deletes timed at the ceiling.
  - Each of F100's four refusals, with an "elsewhere" case.
- **Load-bearing gaps:**
  - Undos of bulk writes while a session is in flight (R3-M1).
  - Oracles that fail correct builds (R3-M2, R3-M3).
  - The symbols of marks in E12 and on history lines can't be read (R3-m1).
  - Nothing tests R3.9's "lists it" (R3-m2).
  - R8.1c's writes other than capture saves are untimed, and nothing tests history above its ceiling (R3-m7, R3-m8).

## Findings
Paths, all under `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/`:
- PRD = `collection-mode/prd-collection-mode.md`
- J = `collection-mode/prd-collection-mode-journeys.md`
- C = `collection-mode/prd-collection-mode-copy.md`
- FEN = `collection-mode/prd-collection-mode-fences.md`
- CAPJ = `capture-mode/prd-capture-mode-journeys.md`
- CAPC = `capture-mode/prd-capture-mode-copy.md`
- DF = `data-foundation/prd-data-foundation.md`

**Round-2 delta verification (subject c4ec992)**

| Round-2 ID | Status | Evidence |
|---|---|---|
| B1 | RESOLVED | J:202 asserts ZX-002's P3 triplet (64.7, 196.8, 103.6)/255 ±1/255. I recomputed (64.69, 196.82, 103.58); it holds across D50/D65 white-point and matrix variants. J:215 (UJ2.1-p) reads ZX-013's chip as T3's value, with T1 and T2 declared at other values (J:94). |
| R2-M1 | RESOLVED | J:53–59 checks the text and its unique words, in any case and in the import PRD's R2.3 form, as UTF-8 and both UTF-16 byte orders. It reads every file beside the file and every table, internal tables included, at F102's moments. J:489 is the control. Remaining calibration issues are R3-m5. |
| R2-M2 | RESOLVED | J:452–458: a restore (e), a Flag (f), an undo (g), a display move (h), a capture save under an L* sort (i), a search change (j) and a re-scan answer (k). In (i), ZX-010 at L* 50 lands between ZX-005 (48) and ZX-003 (55); I recomputed this. Only the unlist direction is tested: R3-m2. |
| R2-M3 | PARTIAL | PRD:600 now reads a chip mark's symbol apart from its tint, and J:478 asserts the 11 symbols distinct. E12 (J:210) and the marks on R5.2d's Standing line still have no symbol readback: R3-m1. |
| R2-m1 | RESOLVED | J:298: ZX-000 at queue position 1; the case asserts ZX-000, not ZX-020. |
| R2-m2 | RESOLVED | J:465 (UJ9.3-c). |
| R2-m3 | RESOLVED | J:483: the crash comes after the new reading is written and before its six derived sets are. |
| R2-m4 | RESOLVED | J:479: every action of every state in the phase, range and toggle selection included. A Nit on its surface list is R3-n3. |
| R2-m5 | RESOLVED | J:449 runs in each in-flight state. |
| R2-m6 | RESOLVED | J:258: GB-3 at ΔE2000 3.0035 (its lightness weighting differs from 1 by about 10⁻⁸) displays as 3.00 and is excluded. J:251: CP-2 (50.01) sorts before CP-1 (50.04), both showing 50.0. |
| R2-m7 | RESOLVED | J:349. |
| R2-m8 | RESOLVED | J:473. |
| R2-m9 | RESOLVED | PRD:598 declares the permission; PRD:590 names E34; J:486 (UJ9.7-g); J:524. DF R7.3 induces DF's new File actions case for it. |
| R2-m10 | PARTIAL | J:474 is now P0 (a keystroke, a header fire and a single edit at each set's last sample). J:477 times both deletes and "Use as scan order". R8.1c's kinds other than capture saves are still untimed, and M1's per-kind claim doesn't match its workload: R3-m7. |
| R2-m11 | RESOLVED | PRD:599 reads each R4.2 and R5.2 line, and the cause by its key; J:277, J:315. UJ4.7-d still reads the cause by its label: R3-n2. |
| R2-m12 | RESOLVED | J:61–65. |
| R2-m13 | PARTIAL | PRD:602's "never the user's file itself" still leaves the file's side files readable as app storage: R3-m6. |
| R2-m14 | PARTIAL | J:203 and J:467 landed as raw byte equality, which correct builds fail. My round-2 fix text was wrong: R3-M2. |
| R2-m15 | RESOLVED | PRD:459 is split. J:419 (UJ7.1-n): GI-1 is unlike though its own pair is the like pair; ⟨unlike⟩ 3, recomputed. The counting rule is R3-m10. |
| R2-m16 | RESOLVED | CAPJ:114 seeds the quarantine before the session; CAPJ:115 is the F84 read-only case. Its byte oracle falls under R3-M2. |
| R2-n1 | RESOLVED | J:68–70 names DL-001 and BI-1 (inside sRGB by 0.016; I recomputed 0.0157). J:414 is now Soft Light. |
| R2-n2 | RESOLVED | J:389, J:393–394. New instances are R3-n1. |
| carried MJ5 | RESOLVED | Via R2-m4, J:479. |
| carried m8 | RESOLVED | Via R2-m2, J:465. |
| carried m9 | RESOLVED | Via R2-m3, J:483. |

**New defects introduced by the round-2 fixes**

**[MAJOR] R3-M1 — PRD:585 (R8.3), PRD:607 (R8.11), PRD:579–580 (R8.1f, R8.1g), C:156 (E6 "elsewhere"), FEN:968–972 (F100) — two bulk writes are still allowed while a session is in flight, and no case covers them.**
- **What F100 changed:** four bulk actions are now refused while a session on any collection is in flight.
- **What it left out:**
  - an "Undo change" reversing a bulk set or clear — R8.3 refuses only the undo of a code change, and says "every other metadata edit … stay available";
  - E10's "Undo" of a selection or collection delete, which R8.3 refuses only "on that collection".
- **Both are reachable:**
  - Clear Family on 10,000 Scale items with no session running, then start a session; F104 says starting a session doesn't end the undo history, and UJ6.4-m fires Undo in flight. "Undo change" is then a BULK_WRITE_BUDGET-class write during capture.
  - Or delete Gouache Set, start a session on Studio Markers, and fire E10's "Undo": a DELETE_WRITE_BUDGET (10 s) write during capture.
- **A builder has to guess:**
  - R8.3 allows both writes.
  - R8.11 says no write may delay a row confirmation.
  - E6 "elsewhere" tells the user "a change that touches many swatches at once waits".
  - F100's title says no bulk writes in flight.
  - Meeting R8.11 while allowing both needs the yield-or-queue design F100 rejected.
- **Mutation that stays green:** allow both undos in flight, holding the single writer for 2–10 s. No case fires either in flight, and UJ9.5-d (J:474) fires only a keystroke, a header and a single edit.
- **Fix (within F100's "no bulk writes"):**
  - In R8.3, move "E10's "Undo"" from the "on that collection" list to the "on any collection" list (no words added).
  - Add "or an "Undo change" of a bulk set or clear" after "Use as scan order" (+9 words).
  - Pay for it by deleting the Build contract's rule-free provenance parentheticals "(the vision's non-goals)" and "(the vision's v2 candidates)" (PRD:101–102, −7), plus 2 words of §8's preamble (see R3-m1).
  - Mirror the change in the capture PRD's F72 Outbound line.
  - Add cases:
    - UJ6.2-l: clear Family on 11 items, bring a session on Gouache Set in flight in each in-flight state, fire "Undo change" → E6 "elsewhere", and the 11 stay empty.
    - UJ4.4-i (R1.7): delete Gouache Set, bring Studio Markers' session in flight, fire E10's "Undo" → E6 "elsewhere".
  - If the owner reads F100 as covering the four actions only, R8.11 needs an owner decision instead.

**[MAJOR] R3-M2 — J:203 (UJ2.1-d), J:467 (UJ9.4-b), CAPJ:115 (capture UJ3.3-l); PRD:174–175, PRD:442 (R2.5), PRD:588 (R8.6) — "the file's bytes equal those before" fails correct builds.**
- By the Vocabulary, the file's bytes include every journal, log, index or file beside the file. Raw byte identity is an implementation detail, not a behaviour.
- **Correct builds that fail:**
  - SQLite's recommended `PRAGMA optimize` at close writes sqlite_stat1.
  - An ownership hold (DF R1.5 refuses a second copy "by name") may be written at open and cleared at close.
  - A WAL checkpoint, on a timer or at close, rewrites main-file pages with the same logical content.
  - The -wal and -shm files exist only while the app is open. UJ9.4-b's undated "before the case" baseline, compared with a read after the app closes, can differ by whole files.
- **Fix (companion only):**
  - Replace each equality with "read with the app closed at SQLITE_READER_FLOOR, every table holds the same rows as the seeded file read before launch, SQLite's own statistics tables aside".
  - Keep UJ9.4-b's byte check for qqq.
  - For capture UJ3.3-l, assert the main file unchanged plus the same row-level read.

**[MAJOR] R3-M3 — J:330 (UJ5.1-a), PRD:521 (R5.3), C:269 (E17 "restore"), J:49–51 — UJ5.1-a fails a correct build once R5.5 lands.**
- F111 moved E17's Use-this-reading sentence into a "restore" variant. By R5.3, that variant renders in recorded order once R5.5 lands.
- UJ5.1-a asserts "E17 renders with no variant", and nothing substitutes the new variant:
  - the Harness's substitution rule covers only E6 ("full");
  - UJ4.7-a conditions E18 itself ("no variant while R5.5 is not built", J:315); UJ5.1-a has no such condition.
- The E6 rule shows the suite re-runs earlier cases in later builds, so a correct build fails UJ5.1-a from R5.5's phase on.
- **Fix (companion only):** "Where a case asserts E6 or E17 with no variant, it asserts E6's "full" variant once every P1 action it names has landed, or E17's "restore" variant once R5.5 has (R8.3, R5.3)."

**[MINOR] R3-m1 (R2-M3 carried) — J:210 (UJ2.1-k), PRD:599–600, C:210–220, J:478, J:528 — the marks in E12 and on history lines have no symbol readback.**
- R8.10b grants E12 nothing beyond "the copy state and variant up, with each token's value", and E12 has no tokens. So UJ2.1-k's "naming eleven marks … each with its shape" can only be read by its wording.
- Never-true and awaiting-answer render on R5.2d's Standing line, not on a chip. So UJ9.6-a can't read two of its eleven symbols.
- **Mutation that stays green:** E12 draws cannot-show with a different symbol from the chip's.
- **Fix:**
  - R8.10c "Each chip's marks by identifier" → "Each chip's, E12's and each history line's marks by identifier" (+5 words).
  - Pay by trimming §8's preamble sentence at PRD:561–564 ("The rows here hold the operating envelope … a class left out is a latent decision.", 48 words). It is an authoring instruction that binds no build behaviour; trim it only if the format's checks don't require it.
  - UJ2.1-k asserts E12's eleven identifiers, each symbol equal to the chip's.
  - The map cites R8.10c, not R8.9.

**[MINOR] R3-m2 — PRD:465 (R3.9), PRD:537 (R6.4); J:450, J:457, J:366 — only the unlist direction is tested.**
- No case makes a change newly list an item. Mutation that stays green: filter maintenance that only drops items that stop matching.
- R6.4's keep branch on a search change is also unasserted. "Any keystroke clears the selection" passes UJ9.1-j; UJ6.1-c covers the keep branch for filters only, and only at P1.
- **Fix:**
  - UJ9.1-m: row-state filter captured; a capture save on ZX-010 → ZX-010 is listed.
  - UJ9.1-n: ZX-013 selected; type zx-01 → ZX-013 is still selected.

**[MINOR] R3-m3 — PRD:585 — nothing tests R8.3's per-collection list with a session on another collection.**
- Every refusal case for the "on that collection" list runs with the session on the same collection (J:290, 295, 304, 317, 346, 299).
- **Mutation that stays green:** refuse every R8.3 action whenever any session is in flight.
- **Fix:** with a session on Gouache Set in flight, "Delete swatch" on Studio Markers' ZX-010 opens DF E8, not E6.

**[MINOR] R3-m4 — PRD:496 (R4.7), J:389–391, J:395–396 — the Undo-history cases list actions on an undeclared surface.**
- F104 offers "Undo change" only where its latest change's collection is shown.
- UJ6.4-l lists actions after "New collection", which ends on Inks or the collection list, where Undo is never offered. So "not offered" passes even if "New collection" doesn't end the history — a false green.
- UJ6.4-f's import and re-read runs, UJ6.4-g and UJ6.4-h leave the surface to the import flow.
- UJ6.4-m reads Undo in flight without the "show Studio Markers' collection surface" step that UJ9.1-a, -c and -i use. Under the full-window takeover reading of the open seam (ADR-0004), it fails.
- **Fix:** end each run with "choose Studio Markers and list the actions offered", and add the show step to UJ6.4-m.

**[MINOR] R3-m5 — J:53–59, J:489; J:123, 245, 282, 393–394, 467, 501 — the byte check needs calibration.**
- **(a) Chance matches on a short word.** "Sky" (T6, UJ4.2-a) is 3 letters: about 4.8×10⁻⁷ per byte position case-insensitively, or roughly 0.5 chance hits per MB of high-entropy bytes. Such bytes include compressed raw payloads (DF R1.6), float arrays and random WAL salts. Every other checked word has four or more letters, at about 3.7×10⁻⁹ per position.
- **(b) App-storage reads have no matching rule.** The reads in UJ3.3-f, UJ9.4-b, UJ6.4-j/k and UJ10.1-d define no case or encoding handling. A UTF-16 plist or undo store passes an exact-case UTF-8 search. Unified-log entries are compressed on disk.
- **(c) The control covers two branches only.** UJ9.8-c checks letter case and a side file, not UTF-16, word-level or table-level matching.
- **(d) Search-index terms can evade a byte search.** A full-text index stores terms prefix-compressed: "plum" after "pink" is stored as "lum".
- **Fix (companion only):**
  - Check words of four or more letters only.
  - Apply the same rule to R8.10e reads, with log entries read decoded.
  - Add control runs: Warm as UTF-16LE in a side file, and warm in an internal table.
  - Read any full-text index's vocabulary at SQLITE_READER_FLOOR.

**[MINOR] R3-m6 (R2-m13 carried) — PRD:602 vs PRD:174; J:245 — a side file can hold "zx-010".**
- A -wal, a persistent WAL, or an index beside the file can hold normalised codes such as "zx-010" and counts as app storage under R8.10e. UJ3.3-f can then fail a correct build.
- **Fix:** R8.10e "never the user's file itself" → "never the file's bytes" (−1 word).

**[MINOR] R3-m7 (R2-m10 carried) — PRD:576 (R8.1c), PRD:735 (M1), J:471, J:459 — timing gaps remain.**
- Of R8.1c's kinds, only capture saves are timed. F112's version-history half has no functional case either.
- M1 says "200 of each kind", but UJ9.5-a gives 20 display moves (now an M1 kind), 20 Table/Grid switches and 20 swatch-size changes.
- **Fix (companion only):**
  - Time 200 of each R8.1c kind, and 200 display moves and grid switches.
  - Add a case: answer ZX-013's re-scan with its history open → E17 shows T3's reason as re-measurement.

**[MINOR] R3-m8 — PRD:568, PRD:584; J:473, J:475 — nothing tests history above HISTORY_READINGS_CEILING.**
- UJ9.5-e holds exactly 500 readings, so a view capped at 500 passes, against R8.2's "however many".
- **Fix:** UJ9.5-c adds an item holding 1,000 readings; E17's ⟨n⟩ and its list both read 1,000.

**[MINOR] R3-m9 — J:275 (UJ4.1-a), PRD:448 — F105's precisions are asserted as formats, not values.**
- u*, v*, X, Y, Z, sRGB and HSL are checked for format only, so values swapped between those slots still pass.
- **Fix:** declare ZX-001's six derived sets and assert the values. I computed u* −32.0, v* −44.7, X 26.79, Y 30.40, Z 49.87, sRGB (91, 157, 211), HSL (207°, 58%, 59%); none sits at a rounding edge.

**[MINOR] R3-m10 — PRD:459 (R3.3), J:419 — the like-pair count is ambiguous and no case tells the readings apart.**
- "The most common among listed items with a value" doesn't say whether a mismatched non-spectral item counts toward the pair its value was worked out under. UJ7.1-n gives D65/10° under all three readings.
- **Fix:** a case that tells them apart:
  - Alpha Inks at D65/10°: AI-1 and AI-2.
  - Beta Inks at D50/2°: BI-1 to BI-3.
  - Gamma Inks at D50/2°: GI-1 and GI-2, non-spectral, worked out at D65/10°.
  - The literal reading gives like pair D65/10° and ⟨unlike⟩ 5. If the owner meant otherwise, R3.3 needs about 8 words, paid from §8's preamble.

**[MINOR] R3-m11 — PRD:592, J:470 — nothing tests "exists only in test builds".**
- **Fix:** UJ9.4-e also tries each R8.10b–e readback against the Release build, and none answers.

**[NIT] R3-n1 — J:453–454 — UJ9.1-f and UJ9.1-g's Rows omit R4.9 and R4.7**, which their Givens need. This is R2-n2's pattern again.

**[NIT] R3-n2 — J:318 vs CAPC:47 — UJ4.7-d reads the set-aside cause by its label.** Read it by its Cause key through R11.11.

**[NIT] R3-n3 — J:479, J:477 — two cases leave a surface undeclared.**
- UJ9.6-b's surface list omits the collection list.
- UJ9.5-g doesn't name the surface its inputs go to while Scale is being deleted.

**[NIT] R3-n4 — PRD:407, PRD:585 — "interrupted" and "elsewhere" can both apply.** This happens when the collection holds an interrupted session and another collection's session is in flight. One case would pin which renders.

## Biggest risks   (what could ship broken behind a green suite)
- **A 2–10 s bulk undo during capture (R3-M1).** It holds up the capture session's saves, while E6 tells the user such changes wait.
- **Correct builds failing byte-equality and UJ5.1-a (R3-M2, R3-M3).** This pushes teams to weaken good oracles.
- **An E12 legend whose symbols differ from the chips' (R3-m1).**
- **Filtered tables that never gain newly matching items (R3-m2).**

## Genuinely solid   (incl. where minimal scoping is correct that a coverage-zealot would wrongly flag)
- **Oracles I recomputed, all correct:**
  - ZX-002's P3 triplet.
  - ZX-019 inside sRGB by +0.00055 to +0.00060 across nine white-point choices, the IEC matrix and a fixed-point (s15.16) ICC matrix. UJ2.1-q's edge case can't flip on a correct build.
  - GB-3; the CP pair; UJ9.1-i's position and UJ9.1-f's list; UJ3.2-e's 7.
  - UJ3.3-m's ⟨unlike⟩ 2, which catches a "most common" rule applied inside a collection (3).
  - UJ3.4-h: FS-007's M2 set at ΔE 0 catches comparing any persisted derived set.
  - UJ3.4-i's tie broken by collection-list order.
- **The byte-check moments catch two classic SQLite leaks.** Reading while open and after a crash finds the old page image the main file keeps until a WAL checkpoint, and PERSIST-mode journals.
- **The E6 "full" substitution** is the right single declarative clause; it only needs E17 added (R3-M3).
- **Lost permission is injectable** through R8.10a together with DF R7.3.
- **Correctly minimal:**
  - UJ2.1-b doesn't assert the clipped sRGB triplet's value; the clipping method belongs to OQ 7.
  - UJ9.5-d needs no baseline run without the browsing inputs; the absolute budget is the contract.
  - Timing R8.1c's kinds at release cadence, not every PR, is enough.

## Missing / over-tested
- **Missing:**
  - Cases for bulk undos in flight (R3-M1).
  - Row-level replacements for the byte-equality oracles (R3-M2).
  - An E17 variant rule in the Harness (R3-M3).
  - Symbol readbacks for E12 and the history lines (R3-m1).
  - "Lists it" and search-keep cases (R3-m2).
  - A cross-collection availability case (R3-m3).
  - Declared surfaces for the Undo cases (R3-m4).
  - Byte-check calibration and more controls (R3-m5).
  - R8.1c timing (R3-m7).
  - A case above the history ceiling (R3-m8).
  - Derived values in UJ4.1-a (R3-m9).
  - A case that pins the like-pair count (R3-m10).
  - A Release-build readback probe (R3-m11).
- **Over-tested:** nothing material.

| Row ID | disposition |
|---|---|
| R1.1 | ALIGN |
| R1.2 | ALIGN |
| R1.3 | ALIGN |
| R1.4 | ALIGN |
| R1.5 | ALIGN |
| R1.6 | ALIGN |
| R1.7 | ALIGN |
| R1.8 | ALIGN |
| R1.9 | ALIGN |
| R1.10 | ALIGN |
| R2.1 | ALIGN |
| R2.2 | ALIGN |
| R2.3 | ALIGN |
| R2.4 | ALIGN |
| R2.4a | ALIGN |
| R2.4b | ALIGN |
| R2.4c | ALIGN |
| R2.4d | ALIGN |
| R2.4e | ALIGN |
| R2.4f | ALIGN |
| R2.4g | ALIGN |
| R2.4h | ALIGN |
| R2.4i | ALIGN |
| R2.5 | OBJECT (R3-M2) |
| R2.6 | ALIGN |
| R2.7 | ALIGN |
| R2.8 | OBJECT (R3-m1) |
| R2.9 | ALIGN |
| R2.10 | ALIGN |
| R2.11 | OBJECT (R3-m9) |
| R3.1 | ALIGN |
| R3.2 | ALIGN |
| R3.3 | OBJECT (R3-m10) |
| R3.4 | ALIGN |
| R3.5 | ALIGN |
| R3.6 | ALIGN |
| R3.7 | ALIGN |
| R3.8 | ALIGN |
| R3.9 | OBJECT (R3-m2) |
| R4.1 | ALIGN |
| R4.2 | OBJECT (R3-m9) |
| R4.2a | ALIGN |
| R4.2b | ALIGN |
| R4.2c | OBJECT (R3-m9) |
| R4.2d | ALIGN |
| R4.2e | ALIGN |
| R4.2f | ALIGN |
| R4.2g | ALIGN |
| R4.2h | ALIGN |
| R4.3 | ALIGN |
| R4.4 | ALIGN |
| R4.5 | ALIGN |
| R4.6 | ALIGN |
| R4.7 | OBJECT (R3-m4) |
| R4.8 | ALIGN |
| R4.9 | ALIGN |
| R5.1 | OBJECT (R3-M3) |
| R5.2 | ALIGN |
| R5.2a | ALIGN |
| R5.2b | ALIGN |
| R5.2c | ALIGN |
| R5.2d | ALIGN |
| R5.2e | ALIGN |
| R5.3 | OBJECT (R3-M3) |
| R5.4 | ALIGN |
| R5.5 | ALIGN |
| R5.6 | ALIGN |
| R5.7 | ALIGN |
| R5.8 | ALIGN |
| R6.1 | ALIGN |
| R6.2 | ALIGN |
| R6.3 | ALIGN |
| R6.4 | OBJECT (R3-m2) |
| R7.1 | ALIGN |
| R7.2 | ALIGN |
| R8.1 | OBJECT (R3-m7, R3-m8) |
| R8.1a | ALIGN |
| R8.1b | ALIGN |
| R8.1c | OBJECT (R3-m7) |
| R8.1d | ALIGN |
| R8.1e | ALIGN |
| R8.1f | ALIGN |
| R8.1g | ALIGN |
| R8.2 | OBJECT (R3-m8) |
| R8.3 | OBJECT (R3-M1, R3-m3) |
| R8.4 | ALIGN |
| R8.5 | ALIGN |
| R8.6 | OBJECT (R3-M2) |
| R8.7 | ALIGN |
| R8.8 | ALIGN |
| R8.9 | OBJECT (R3-m1) |
| R8.10 | OBJECT (R3-m5, R3-m11) |
| R8.10a | ALIGN |
| R8.10b | ALIGN |
| R8.10c | OBJECT (R3-m1) |
| R8.10d | OBJECT (R3-m5) |
| R8.10e | OBJECT (R3-m5, R3-m6) |
| R8.10f | ALIGN |
| R8.11 | OBJECT (R3-M1) |
| E1 | ALIGN |
| E2 | ALIGN |
| E3 | ALIGN |
| E4 | ALIGN |
| E5 | ALIGN |
| E6 | OBJECT (R3-M1) |
| E7 | ALIGN |
| E8 | ALIGN |
| E9 | ALIGN |
| E10 | ALIGN |
| E11 | ALIGN |
| E12 | OBJECT (R3-m1) |
| E13 | ALIGN |
| E14 | ALIGN |
| E15 | ALIGN |
| E16 | ALIGN |
| E17 | OBJECT (R3-M3) |
| E18 | ALIGN |
| E19 | ALIGN |
| M1 | OBJECT (R3-m7) |
| M2 | ALIGN |
| M3 | ALIGN |
| M4 | ALIGN |

#### peer-interface-reviewer (Claude route)

## Verdict
Contract sound. Every round-2 interface finding is resolved except IF-16 (the product README line), which the fix file deferred on purpose. The round-2 fixes introduced no Blocker or Major. They did leave seven Minors and four Nits, mostly about how E6's variants are chosen and cite hygiene.

## Surface & consumers (brief)
- **What I reviewed:** commit c4ec992. That covers the row IDs, named constants, both obligations tables and the copy index of `docs/product/collection-mode/prd-collection-mode.md`, plus its journeys, copy and oq-results companions. I read every named source.
- **Who depends on it:**
  - agent builders, who read rows and copy;
  - test harnesses, which key on copy-state and variant IDs, mark identifiers and action labels;
  - the Capture, Data Foundation, Import, Export and Device PRDs, which cite these rows.
- **What changed (`git diff 7861221 HEAD`):**
  - Capture F72: its obligations line and Traceability, and R5.8/R6.6 wording.
  - Data Foundation F53: new E34, R1.10, R7.3j and R7.6p; R2.3, R3.4 and R6.2a clarified; E11, E26 and E33 reworded; OQ 20 widened.
  - ADR-0003 row: now 7 inputs.
  - post-lock.md: one item ticked and two lines added.
- **Gating:** every sibling change carries a dated fence and a "peer review pending" status clause. I found no ungated break.
- **Mechanical label check (re-run):** the copy file has 53 labels. Every quoted string in the PRD and the journeys resolves to one of them, and every label is quoted in both files. Zero unresolved, zero unused.

## Findings

**Round-2 delta verification**

| Round-2 finding | Status | Evidence |
|---|---|---|
| R2-IF-1 | RESOLVED | PRD:590 (R8.8 renders DF E34); DF copy:29 (E34); DF PRD:107 (R1.10), :208 (R7.3j), :88 (R7.6p); journeys:486 (UJ9.7-g); PRD:300 (row 1 lists DF E34); post-lock.md:41 ticked |
| R2-IF-2 | RESOLVED | PRD:599 (R8.10b reads each R4.2/R5.2 line, and a cause by key); journeys:277 (UJ4.1-c), :315 (UJ4.7-a). A residual in UJ4.7-d is R3-IF-10. |
| R2-IF-3 | RESOLVED | copy:219 now reads "Can't show:", matching the chip label at copy:301 |
| R2-IF-5 | RESOLVED | PRD:496 (column hide/show and "New collection" end the history; a capture save and a session start do not); journeys:392, :395, :396 (UJ6.4-i, -l, -m) |
| R2-IF-6 | RESOLVED | PRD:465 (one list, re-scan answer and undo added); PRD:537 (R6.4 cites it); journeys:458 (UJ9.1-k). A gap in the list itself is R3-IF-6. |
| R2-IF-7 | RESOLVED | PRD:372 (preamble), :377–378 (Surfaces rows), :693 (index row) |
| R2-IF-8 | RESOLVED | copy:152 (the P0 body names only P0 actions); copy:155 ("full" [phase: variant-absent]); PRD:585 (R8.3 enumerates it). A new edge is R3-IF-3. |
| R2-IF-9 | RESOLVED | PRD:497 (R4.8 adds the Column headers table); journeys:314 (UJ4.6-j) |
| R2-IF-10 | RESOLVED | PRD:627–628 (fence cites name their documents); PRD:629 (its R1.2) |
| R2-IF-11 | RESOLVED | PRD:630 (cause-labels table, R4.2b); PRD:642 (new Inbound line → R8.3); PRD:643 ("its F70 and F71") |
| R2-IF-12 | RESOLVED | capture PRD:215, :233 now read "flagged as missing or damaged", matching capture copy:56 |
| R2-IF-13 | RESOLVED | capture PRD:116 reads F1–F72 (F72 at capture fences:640) |
| R2-IF-14 | RESOLVED | PRD:140 (R4.6 and R5.5) |
| IF-10 (carried) | RESOLVED | DF PRD:340 (OQ 20 names E33's selection and two deletes in one window); PRD:761 |
| IF-16 (carried) | UNRESOLVED (deferred on purpose) | `docs/product/README.md:18` still reads "queued"; the fix file's Out-of-scope list defers it to the bookkeeping close |

**New defects**

[MINOR] R3-IF-1 — Harness in-flight rule (journeys:49–51) and E17: F111 added E17's "restore" variant, but only E6 got a rule for builds where its phase variant has landed.
- **The contradiction:** UJ5.1-a (journeys:330) and UJ5.3-n (:352) have the same Given ("the seeded file") and the same When (open ZX-013, fire "Show history").
  - UJ5.1-a asserts "E17 renders with no variant".
  - UJ5.3-n asserts its "restore" variant.
- UJ5.1-b (:331) also asserts "the body" in recorded order.
- **When it bites:** the E6 rule only makes sense if first-phase cases such as UJ1.2-a run again in later builds. On that reading, UJ5.1-a and UJ5.1-b fail every correct build once R5.5 lands.
- **The other reading isn't granted either:** "Build phase" is listed as a seam input (journeys:103), but R8.10a (PRD:598) does not grant it.
- **Fix (journeys only, no body words):** "Where a case asserts E6, E17 or E18 with no variant, it asserts that state's phase variant ("full", "restore", "restore") in a build where every P1 action that variant names has landed."

[MINOR] R3-IF-2 — R1.4 (PRD:407), R8.3 (PRD:585) and E6 (copy:154, :156): no rule says which of E6's variants wins when two apply.
- **The state is reachable:** under the capture PRD's R3.2 (capture PRD:156) and R3.5 (:159), Studio Markers can hold an interrupted session while Gouache Set's session is in flight.
- **Both variants apply:** "Delete collection" on Studio Markers then meets R1.4's "interrupted" condition and R8.3's "elsewhere" condition, which is new this round (R1.4 dropped "there").
- **Nothing settles it:** no case covers it, because UJ1.2-b's Given (journeys:175) rules out a session in flight. R8.10b reads the variant by ID, so two correct builds diverge.
- **Wording nit folded in:** R8.3's "its 'full' variant, beside R1.4's 'interrupted', once…" reads as if both variants render together.
- **Fix (owner picks the order):** add "and no session is in flight" to the copy's "interrupted" condition (companion only), or have R1.4 say "R8.3 governing it first in flight" (+1 word). Add a UJ1.2-g case for the combined state.

[MINOR] R3-IF-3 — E6's "full" variant is gated three different ways, and all three wait on R1.7.
- **The three conditions:**
  - copy:155: "every P1 action R8.3 names is built";
  - the Harness (journeys:50): "every P1 action that variant names";
  - R8.3 (PRD:585): "every P1 action it names".
- **They split on "Use as scan order" (R2.9).** R8.3 names it, but it is never refused on the session's own collection, so "full" does not name it. In a build where R2.9 lands last, the copy says the body renders and the Harness says "full" — for UJ1.2-a, UJ4.3-f, UJ6.2-j and others.
- **All three include E10's "Undo" (R1.7).** If OQ 10 is still open at release, F57 defers R1.7 (PRD:410). Then "full" never ships in v1, and refused "Change code", Flag, "Set a field" and "Delete selected" all show a P0 body that names none of them.
- **Fix:**
  - In the copy, gate "full" on "every P1 action this variant names is built", and drop "and undoing a delete" from its body.
  - In R8.3, change "it names" to "that variant names" (+1 word, paid by dropping "now" in R3.9's "view sort now place it").

[MINOR] R3-IF-4 — R8.3 against R8.1g, F100 and R8.11 (PRD:580, :585, :607): an undo of a large delete is not refused while another collection is scanning.
- **The mismatch:** R8.1g puts E10's "Undo" of a selection or collection delete in the delete class (DELETE_WRITE_BUDGET, 10 s). R8.3 refuses it only "on that collection", and a deleted collection can't hold a session.
- **Effect:** with Gouache Set in flight, undoing Studio Markers' "Delete collection" is offered and runs a 10 s write on the single writer. That is exactly the case F100 refused for the deletes themselves. R8.11 then requires engineering F100 turned down (writes that yield or queue behind capture).
- **Why only this undo:** "Undo change" of a bulk set is already covered, because R4.7 re-checks R8.3.
- **Mitigation:** R1.7 is Stop-gated behind OQ 10 (PRD:305).
- **Fix (owner confirms it counts as a bulk write under F100):** move E10's "Undo" of a selection or collection into R8.3's any-collection group. That costs about 6 words, paid by R1.7's "— this row is not built until that question closes —", which repeats OQ 10's interim (PRD:761) and the Stop.

[MINOR] R3-IF-5 — E8's body and "clear" variant (copy:173, :175) against R4.7 (PRD:496): E8 promises an undo that R4.7 withdraws after an import.
- **What E8 says:** the undo lasts "until the file closes, you read the file again, or you change anything other than swatch details, codes or names".
- **The mismatch:** an import that only updates swatch details is, in E8's own words, "changing swatch details". But R4.7 ends the history at any other committed write, and UJ6.4-f (journeys:389) asserts "Undo change" is gone after an import.
- **Fix (copy only):** "…you read the file again or import into it, or you change anything other than swatch details, codes or names by hand — scanning doesn't count."

[MINOR] R3-IF-6 — R3.9 (PRD:465), which R6.4 (PRD:537) now cites as the one list: the list leaves out a code change. This is pre-existing, but the R2-IF-6 fix made the list authoritative.
- **The list:** "an edit, a capture save, a restore, a Flag, a re-scan answer, an undo or a re-read".
- **Why it misses code changes:** this PRD keeps "an edit" apart from a code change (R4.3 PRD:492; R4.7 lists "a field edit, a code change … a bulk set or clear").
- **Effect:** changing ZX-001 to ZX-100 under a Code sort or a "zx-00" search neither re-lists nor deselects by any row. A bulk set is ambiguous the same way.
- **Fix:** "an edit" → "a metadata change" (R4.7's term), word-neutral with the trim above. Add a case in R4.4's phase: search zx-00, ZX-001 selected, change it to ZX-100; it unlists and is deselected.

[MINOR] R3-IF-7 — R8.8 (PRD:590): "that PRD's E34" points at the wrong document by its nearest antecedent.
- **The sentence:** "…the capture PRD's E26, because another copy of the app holds the file that PRD's E10, and because permission to the file was lost that PRD's E34".
- **The collision:** read in order, "that PRD" is the capture PRD. The capture PRD has its own E34 ("Sort over a manual order", capture copy:34), and it renders on this PRD's surfaces (R2.9).
- **Rule broken:** the PRD's own cite rule (PRD:618–620).
- **Mitigation:** row 1 (PRD:300) and UJ9.7-g name the Data Foundation PRD's E34.
- **Fix (word-neutral):** reorder the list so the capture clause comes last: "…renders that PRD's E15, because another copy of the app holds the file its E10, because permission to the file was lost its E34, and because the volume is gone the capture PRD's E26".

[NIT] R3-IF-8 — copy:94–97: the sibling-owned states list leaves out the Data Foundation PRD's E34, which R8.8 renders here. The Test-controls map (journeys:522) has it. Add it.

[NIT] R3-IF-9 — sibling IDs written bare, against the cite rule (all pre-existing):
- UJ2.1-g "fire E22's action" (journeys:206), UJ2.1-h (:207) and UJ2.1-j "E23 is up" (:209);
- UJ8.1-c and UJ8.1-d "E34 renders" (:430–431) — E34 now exists in both Capture and the Data Foundation PRD;
- R4.9's "(E11 in the same detail)" (PRD:498), where this PRD's own E11 is the column-name error.

Fix: name the document in each case. In R4.9, drop the parenthetical (−4 words; R2.4i already defines the mark).

[NIT] R3-IF-10 — reading a set-aside cause:
- R8.10b (PRD:599) says "the capture PRD's cause key", but the capture table's column is "Cause" (capture copy:49).
- UJ4.7-d (journeys:318) still asserts the set-aside list shows "the cause flagged after capture" (the label), against capture copy:47 ("through R11.11, never by its label"). This is left over from R2-IF-2's class of defect.

Fix: say "by its Cause column", and have UJ4.7-d read "the capture PRD's R11.11 reads ZX-001's cause Flagged after capture".

[NIT] R3-IF-11 — E6 "elsewhere" (copy:156): "waits until it ends" reads as if the change were queued. R8.3 says the action changes nothing, and the variant's own last sentence says "do it again". Use "can't be made until it ends".

## Biggest risks   (what existing consumers/scripts/agents break)
- **A regression suite that re-runs first-phase cases goes red on a correct build once R5.5 lands** (UJ5.1-a against UJ5.3-n). An agent builder could "fix" it by suppressing E17's "restore" variant (R3-IF-1).
- **Harnesses split on E6:**
  - which variant shows when another collection is in flight and this one is interrupted (R3-IF-2);
  - when "full" applies, which diverges if R2.9 lands last (R3-IF-3).
- **If OQ 10 defers R1.7, v1's refusals of P1 actions show a message that names none of them** (R3-IF-3).
- **Once R1.7 lands, undoing a collection delete runs a 10 s write during another collection's session,** and no case tests that against R8.11 (R3-IF-4).

## Genuinely well-designed   (incl. where a deliberate inconsistency is correct that a style-checker would wrongly flag)
- **R2-IF-1 was closed the right way.** The Data Foundation PRD now owns E34, with R1.10, R7.3j, R7.6p and DJ3 all mirroring it. This PRD's R8.8, row 1, R8.10a, UJ9.7-g and the Test-controls map name it, and the post-lock item is ticked.
- **The F100 mirror is complete on both sides:** Capture F72 plus its obligations line (capture PRD:386) matches this PRD's Inbound line (PRD:642).
- **The ADR-0003 row's seven inputs match the Outbound DF row one for one.**
- **R4.7's ender set is now closed, and every class that was ambiguous has its own case** (UJ6.4-i, -l, -m, -n).
- **"Undo change" re-checks R8.3,** so undoing a bulk set on another collection is refused with "elsewhere" without R8.3 having to list it.
- **The "elsewhere" variant has a case for every refused bulk action:** UJ1.2-f, UJ6.2-i, UJ6.3-f and UJ8.1-j.
- **Status lines reconcile:** the copy index and the copy file's Status lines agree on all 19 states.
- **"Columns" closes the staff engineer's deferred label gap.**
- **Deliberate choices a style checker would wrongly flag:**
  - E6 "elsewhere" binds ⟨collection⟩ to the session's collection, not the one acted on. That is right: the headline is about the session.
  - "restore" is reused as a variant name on E17 and E18. That is fine, because variants are read per state.
  - E10 still carries no phase mark, because it is held back by the OQ 10 Stop rather than by phase.

## Missing / over-engineered
- **Missing:**
  - a phase-variant mapping in the Harness for E17 and E18, not just E6;
  - a precedence rule between E6's variants;
  - one gating condition for "full", used in all three places;
  - the Data Foundation PRD's E34 in the copy file's sibling-owned list;
  - IF-16's README line.
- **Over-engineered:** nothing material.

| Row ID | disposition |
|---|---|
| R1.1 | ALIGN |
| R1.2 | ALIGN |
| R1.3 | ALIGN |
| R1.4 | OBJECT (R3-IF-2) |
| R1.5 | ALIGN |
| R1.6 | ALIGN |
| R1.7 | ALIGN |
| R1.8 | ALIGN |
| R1.9 | ALIGN |
| R1.10 | ALIGN |
| R2.1 | ALIGN |
| R2.2 | ALIGN |
| R2.3 | ALIGN |
| R2.4 | ALIGN |
| R2.4a | ALIGN |
| R2.4b | ALIGN |
| R2.4c | ALIGN |
| R2.4d | ALIGN |
| R2.4e | ALIGN |
| R2.4f | ALIGN |
| R2.4g | ALIGN |
| R2.4h | ALIGN |
| R2.4i | ALIGN |
| R2.5 | ALIGN |
| R2.6 | ALIGN |
| R2.7 | ALIGN |
| R2.8 | ALIGN |
| R2.9 | ALIGN |
| R2.10 | ALIGN |
| R2.11 | ALIGN |
| R3.1 | ALIGN |
| R3.2 | ALIGN |
| R3.3 | ALIGN |
| R3.4 | ALIGN |
| R3.5 | ALIGN |
| R3.6 | ALIGN |
| R3.7 | ALIGN |
| R3.8 | ALIGN |
| R3.9 | OBJECT (R3-IF-6) |
| R4.1 | ALIGN |
| R4.2 | ALIGN |
| R4.2a | ALIGN |
| R4.2b | ALIGN |
| R4.2c | ALIGN |
| R4.2d | ALIGN |
| R4.2e | ALIGN |
| R4.2f | ALIGN |
| R4.2g | ALIGN |
| R4.2h | ALIGN |
| R4.3 | ALIGN |
| R4.4 | ALIGN |
| R4.5 | ALIGN |
| R4.6 | ALIGN |
| R4.7 | ALIGN |
| R4.8 | ALIGN |
| R4.9 | ALIGN |
| R5.1 | ALIGN |
| R5.2 | ALIGN |
| R5.2a | ALIGN |
| R5.2b | ALIGN |
| R5.2c | ALIGN |
| R5.2d | ALIGN |
| R5.2e | ALIGN |
| R5.3 | ALIGN |
| R5.4 | ALIGN |
| R5.5 | ALIGN |
| R5.6 | ALIGN |
| R5.7 | ALIGN |
| R5.8 | ALIGN |
| R6.1 | ALIGN |
| R6.2 | ALIGN |
| R6.3 | ALIGN |
| R6.4 | OBJECT (R3-IF-6) |
| R7.1 | ALIGN |
| R7.2 | ALIGN |
| R8.1 | ABSTAIN |
| R8.1a | ABSTAIN |
| R8.1b | ABSTAIN |
| R8.1c | ABSTAIN |
| R8.1d | ABSTAIN |
| R8.1e | ABSTAIN |
| R8.1f | ABSTAIN |
| R8.1g | ABSTAIN |
| R8.2 | ABSTAIN |
| R8.3 | OBJECT (R3-IF-2, R3-IF-3, R3-IF-4) |
| R8.4 | ALIGN |
| R8.5 | ALIGN |
| R8.6 | ALIGN |
| R8.7 | ALIGN |
| R8.8 | OBJECT (R3-IF-7) |
| R8.9 | ALIGN |
| R8.10 | ALIGN |
| R8.10a | ALIGN |
| R8.10b | ALIGN |
| R8.10c | ALIGN |
| R8.10d | ALIGN |
| R8.10e | ALIGN |
| R8.10f | ALIGN |
| R8.11 | ABSTAIN |
| E1 | ALIGN |
| E2 | ALIGN |
| E3 | ALIGN |
| E4 | ALIGN |
| E5 | ALIGN |
| E6 | OBJECT (R3-IF-2, R3-IF-3) |
| E7 | ALIGN |
| E8 | OBJECT (R3-IF-5) |
| E9 | ALIGN |
| E10 | ALIGN |
| E11 | ALIGN |
| E12 | ALIGN |
| E13 | ALIGN |
| E14 | ALIGN |
| E15 | ALIGN |
| E16 | ALIGN |
| E17 | OBJECT (R3-IF-1) |
| E18 | ALIGN |
| E19 | ALIGN |
| M1 | ABSTAIN |
| M2 | ALIGN |
| M3 | ALIGN |
| M4 | ALIGN |

The table covers 122 IDs, R8.1g included.
- **Abstentions:** R8.1, R8.1a–g, R8.2, R8.11 and M1 are performance-lens rows.
- **Nits that object no row:** R3-IF-8 through R3-IF-11. The R4.9 and R8.10b nits fold into R3-IF-9 and R3-IF-10.
- **R3-IF-4:** the fix belongs in R8.3, so R1.7 is left aligned.

Paths: `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode.md`, `…/prd-collection-mode-copy.md`, `…/prd-collection-mode-journeys.md`, `…/docs/product/capture-mode/prd-capture-mode.md`, `…/docs/product/capture-mode/prd-capture-mode-copy.md`, `…/docs/product/data-foundation/prd-data-foundation-copy.md`, `…/docs/product/README.md`.

#### peer-privacy-reviewer (Claude route)

## Verdict
**Privacy-sound to ship.** No finding is Critical or High. The round-2 fixes closed five of my seven round-2 findings and part of the other two. No new or amended row adds a network path or moves personal data off the machine. One Medium should close before lock: nothing places or checks copies of item content kept outside the user's file. The All items search budget (F103) now points builders toward keeping such a copy, as a search index.

## Data-flow & PII map (brief)
- **Who the data is about.** The user, and anyone named in an imported free-text column (F8) or in a collection name. Nothing is special-category.
- **Personal fields.**
  - The device serial and firmware (R4.2d, R5.2b), and measurement and record times.
  - Swatch Name, the alternates, imported values, and collection and column names. R1.3, R4.3, R4.8 and R6.2 edit them; R1.4, R4.5 and R6.3 delete them.
  - Search text. In the All items view it now matches names and imported values across the whole file (F109, F103).
  - Column visibility.
- **Where it is stored.** The one SQLite file, plus the files the app keeps beside it. Search, filters, sorts, the grid choices and undo stay in memory (R8.6). R8.6 now also rules out system-kept versions of the file and Spotlight/Handoff donations (F130). Where a search index for F103 would live is unstated (PRIV3-2).
- **How data leaves.** Only through an export the user starts. There is no network path: in the Demo configuration, with telemetry and update checks off, zero outbound attempts are allowed (UJ9.4-c). The test readbacks exist only in test builds, and no build opens a listening socket for them (F132). E34 shows the file's path only to its owner, on screen.
- **The user's rights.** Removed text is gone from the file's bytes from the moment its write lands, open or after a crash (F102). Corrections happen in place, and history is kept for good. Backups and snapshots are left to the help docs (F131).

## Findings
Paths used below:
- PRD = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode.md
- J = …/collection-mode/prd-collection-mode-journeys.md
- F = …/collection-mode/prd-collection-mode-fences.md
- FIX = …/collection-mode/prd-collection-mode-round-2-fixes.md
- DF = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/data-foundation/prd-data-foundation.md
- ADRQ = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/decisions/README.md
- PL = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/post-lock.md

**Round-2 delta (subject c4ec992)**

| Round-2 ID | Status | Evidence |
|---|---|---|
| PRIV-10 (Medium) | PARTIAL | The Harness's byte check now reads the file and every file beside it at three moments: open after the write, after a crash, and closed (J:53-59). UJ1.3-c now reads the bytes for Teal (J:182). But the carve-out "unless the case names its moment" (J:55) lets four cases be read as closed-only. Their Assert also says "the file read with the app closed" or "at SQLITE_READER_FLOOR": T1 (J:118), UJ1.3-c (J:182), UJ4.2-c (J:284) and UJ6.3-b (J:379). Carried as PRIV3-1. |
| PRIV-11 (Low) | RESOLVED | R1.3 carries the byte clause (PRD:406). T5 asserts Gouache Set nowhere (J:122). A correct build passes: the whole text is not a substring of Gouache Travel Set, and the Harness exempts its words (J:56-57). A bookkeeping gap is noted in PRIV3-8. |
| PRIV-12 (Low) | PARTIAL | **Gap 2 is closed.** R8.6 rules out system-kept versions and donations (PRD:588). R8.10e and the map add unified-log entries, versions and donations (PRD:602, J:525), and UJ9.4-d tests them (J:469). **Gap 1 is not.** It is answered with "wherever the build puts it", which still leaves the list of locations to the build under test. FIX:353-356 had planned "by location under either sandbox reading". **Crash reports** are still read nowhere. |
| PRIV-13 (Low) | RESOLVED | UJ9.4-c declares the Demo configuration with telemetry and update checks off, and asserts no outbound attempt at all (J:468). The Data Foundation PRD's R1.4 leaves that configuration's permitted set empty (DF:104). |
| PRIV-14 (Info) | RESOLVED | R4.7 cites the Data Foundation PRD's R6.2d and R1.1 and this PRD's F53 (PRD:496). |
| PRIV-15 (Info) | RESOLVED | F131 (F:1156-1161); PL:137 carries the help-docs item. |
| PRIV-16 (Info) | RESOLVED | R8.10 (PRD:592), UJ9.4-e (J:470), F132 (F:1163-1167). The new case has defects of its own: PRIV3-5 and PRIV3-6. |

**New findings**

[MEDIUM] **PRIV3-2 — Copies of item content kept outside the user's file: nothing says where they may live, and no case looks**
- **Where:** PRD:584 (R8.2, F103); ADRQ:22 (the ADR-0003 row); J:53-59 (byte checks); J:393-394 (UJ6.4-j/k); DF:98 (the Data Foundation PRD's R1.1).
- **Why an index is now likely.** F103 holds All items search to BROWSE_RESPONSE_BUDGET at 100,000 items with 20 imported columns each. The ADR-0003 row answers with "an index or a scan, either one under that byte rule".
- **The rule doesn't reach it.** The byte rule covers only the file and the files beside it. An index in ~/Library/Caches or Application Support is outside it.
- **Only one rule forbids it.** The Data Foundation PRD's R1.1 says "Nothing about a collection … is kept elsewhere". No row here restates that for item content; R8.6 lists only view state and undo.
- **What the cases read.**
  - Every byte check reads only the file and what sits beside it (J:54).
  - No delete case reads the app's own storage: T1, UJ4.4-b, UJ1.3-c and UJ6.3-b.
  - The storage reads that do exist (UJ6.4-j/k, UJ3.3-f, UJ9.4-b) have no defined matching. The Harness's any-case, normal-form and UTF-16 rules are written for the file's bytes only.
- **Mutation that passes every case.** Keep a full-text index of names, codes and imported values in ~/Library/Caches/<bundle id>/, stored as lower-case tokens. Update it on edits and clears; never purge it on deletes.
  - T1, UJ4.4-b, UJ1.3-c and UJ6.3-b pass: they never read the cache.
  - UJ6.4-j/k, read literally, look for "Pink" and "Yellow" and miss "pink" and "yellow".
  - UJ3.3-f passes because tokenising splits ZX-010 into "zx" and "010", so "zx-01" never appears.
  - Meanwhile the cache holds every current Swatch Name and imported value, and still holds Warm Grey 1, Teal and Plum after their deletes.
- **Who is affected.** The user, and third parties named in imported free text. The copy survives deletes, and survives the user moving or deleting the file. No surface shows it exists.
- **Remediation (no PRD body words).**
  - (a) The Harness's byte check also reads the app's own storage (R8.10e), and every read of that storage uses the byte check's matching.
  - (b) Add one P1 case: after UJ7.1-m's All items search, quit and read the app's own storage. Assert it holds none of Sky Blue, Cerulean or Purple.
  - (c) On the orchestrator's say-so, as F98 allows for the ADR queue, the ADR-0003 row reads "an index in the file, or a scan" (DF R1.1).
- (GDPR Art. 5(1)(e), Art. 17, Art. 25(2); the Data Foundation PRD's R1.1)

[LOW] **PRIV3-1 — The moment carve-out can turn four byte cases into closed-only checks**
- **Where:** J:55; T1 (J:118), UJ1.3-c (J:182), UJ4.2-c (J:284), UJ6.3-b (J:379).
- **The rule.** Every case that deliberately names its moment does so in its When: T4, UJ1.3-e/f, UJ6.4-j/k and UJ9.4-b. These four instead say "the file read with the app closed" (or "at SQLITE_READER_FLOOR", which R8.10 reads closed) in the Assert, beside "its bytes".
- **The risk.** A literal builder can take that phrase as the case's moment. Under that reading, three writes are never checked with the file open or after a crash: the only P0 collection delete (UJ1.3-c), the only single-field clear (UJ4.2-c) and the only selection delete (UJ6.3-b).
- **Mutation.** WAL mode with no checkpoint after a write that removes text. All four cases pass, while the open file holds Teal, Plum and Azure for the whole session.
- **Remediation (journals).** Reword the carve-out to "unless the case's When names the moment at which it reads the bytes".
- (F102)

[LOW] **PRIV3-3 — The after-crash byte read can erase its own evidence**
- **Where:** J:53-59; PRD:601 (R8.10d).
- **The gap.** At each moment the check scans the raw bytes and also reads every table at SQLITE_READER_FLOOR. It orders neither step, and says nothing about how the SQLite reader opens the file. After a crash, a read-write connection (the sqlite3 CLI's default) runs WAL recovery. When that last connection closes, it checkpoints and deletes the -wal file.
- **Mutation.** A build that never checkpoints after a removing write leaves Teal in the main file's old pages and in the -wal, after UJ1.3-f's or UJ6.4-k's crash. If the test does its SQL read first, it rewrites or unlinks those bytes before the raw scan, and the case passes.
- **Remediation (journals).** At each moment, copy the file and every file beside it byte for byte before any SQLite connection opens them. Raw-scan that copy; run the SQL read against a second copy, or read-only.

[LOW] **PRIV3-5 — UJ9.4-e is stricter than its row, and doesn't say which listing to check**
- **Where:** J:470 (UJ9.4-e); PRD:592 (R8.10).
- **The mismatch.** R8.10 (F132) forbids a listening socket or cross-process service opened for a readback. UJ9.4-e asserts that none of any kind is listed, in both a Release and a test build, and never says which operating-system listing counts as "cross-process services it offers".
- **The risk.** A tester who lists launchd's per-process services would also see an XPC service embedded in the app bundle. One example is Sparkle 2's installer under the sandboxed reading: AGENTS.md §3 recommends Sparkle, and sandboxing is still open. A correct build then fails the case, or the tester guesses.
- **Remediation (journals).** Assert no listening socket at all (lsof over the process's TCP, UDP and Unix sockets), and no service the app registers or offers that returns an R8.10b–e value. F132 stays as written.

[INFO] **PRIV3-4 — "Beside it" has three scopes**
- This PRD's Vocabulary (PRD:174) says "other file the app keeps beside it".
- The Data Foundation PRD's R2.3 and R6.2a (DF:116, :230) and the ADR-0003 row (ADRQ:22) say "any … other file beside it". Read literally, that includes a user's own CSV or export saved in the same folder.
- The Harness (J:54) says "every file beside it".
- No case puts such a file there, and user-facing copy promises only "your file", so nothing overclaims to the user.
- Fix: the Data Foundation half says "the app keeps", in a dated line under its F53. The Harness says a case's folder holds only the file, the files the app keeps beside it, and any control file the case declares.

[INFO] **PRIV3-6 — Release builds get no byte or storage check.** F132's "every readback below but R8.10f exists only in test builds" (PRD:592) also covers R8.10d and R8.10e, which are reads made from outside the app. As a result, no byte or storage case runs against a Release build; only UJ9.4-e does. Without re-opening F132: run UJ9.4-b and UJ9.4-d "in turn a Release build and a test build", as UJ9.4-e already does (journals only).

[INFO] **PRIV3-7 — Edge cases in the byte check's matching rule (J:56-58)**
- The word exemption is worked out from "values the file holds after the action". If that means the build's own file, a build that wrongly keeps the old text exempts its own words. It should mean the case's declared values after the action.
- The whole text gets no exemption, and the check ignores letter case. So R1.3's and R4.3's byte clause can't be satisfied on a change of letter case only (UJ1.1-d, UJ4.3-g, UJ4.6-i). Nor can it where another kept value holds the same text, such as a second swatch named Sky Blue or a capture review note (the capture PRD's R8.13).
- No current case asserts bytes in these situations, so no correct build fails today. Fix: exempt the whole text where a declared post-action value contains it.

[INFO] **PRIV3-8 — Bookkeeping**
- R1.3's new byte clause appears in neither F35's nor F102's Carried by (F:1226, F:1293); F102 lists T5 only. Add R1.3 to F102's Carried by and its map line.
- The map's app-storage line (J:525) allows reads "with the app closed or after a crash", but UJ6.4-j (J:393) reads the storage while the app is open. Add "open" to J:525.

**Word budget.** Every remediation above lands in the journeys, the fences or the ADR queue, so none adds PRD body words. One optional addition does: "crash reports" in R8.10e (+2 words, for PRIV-12's remainder). It can be paid for by cutting R8.10's rule-free purpose clause "to assert what each action left there" (PRD:592).

## Biggest privacy risks
1. **Copies of names and imported values in the app's own storage, outside the file (PRIV3-2).** They survive deletes and survive the file being removed. The new index-or-scan input makes this likely, and no case looks.
2. **Byte checks that a residue build can pass.** Under one reading, four cases never read the file open or after a crash (PRIV3-1). After a crash, the SQL read can destroy the evidence before the raw scan runs (PRIV3-3).
3. **An app-storage readback whose locations the build under test lists itself, with crash reports left out (PRIV-12's remainder).**

## Genuinely privacy-respecting
- **F102 lands consistently.** It appears in R8.8, the Data Foundation PRD's R2.3 and R6.2a, the ADR-0003 row and the Harness. That closes the WAL and side-file residue gap at the rule level.
- **R8.8 does not miss deletes.** Its list leaves out single and collection deletes, which a shallow read would flag. But deletes go through the Data Foundation PRD's E8, E14 and E33 to its R6.2a, which now carries the moment clause, and the Vocabulary's deleted state cites it.
- **UJ9.4-c asserts zero outbound attempts.** That is the strongest oracle available, and it runs with the network reachable.
- **F130 is correct minimisation.** No autosave versions and no Spotlight or Handoff donation. A checklist might call the missing autosave versions a lost convenience, but DF R1.1 says the one file is everything.
- **The test seam stays out of shipped builds.** F132 keeps the in-app readbacks to test builds, with no listening socket (UJ9.4-e).
- **F99 favours privacy.** It gives deletes 10 s instead of scrubbing freed bytes in the background, so there is no window in which freed text lingers.
- **The copy stays honest.** E8's new ending ("What was there isn't kept in your file. Undo change puts it back until …") is still true, and UJ6.4-j/k prove both halves. E4's "all items" variant tells the user that collection names and imported values are searched.
- **The removed state is honest.** It carries "no byte guarantee" for a change made outside the app.
- **Browsing writes nothing.** UJ9.4-b's "file's bytes equal those before the case" shows the file keeps no browse history.
- **E34 is not a disclosure.** It shows the file's path only to its owner, on screen.
- **The sibling halves add no data flow.** Capture F72, the Data Foundation PRD's E11 and E26 wording, and Capture R5.8 and R6.6 change no flow of personal data.
- **Every new fixture is synthetic:** ZX-019, LH-001, CP-1/2, RT-1–3, AI/BI/GI and ZX-000/020.

## Missing controls / over-collection
- A declared place, in the file, for any search index. Delete and edit cases that also read the app's own storage, using the byte check's matching (PRIV3-2).
- Reads with the file open and after a crash for UJ1.3-c, UJ4.2-c and UJ6.3-b (PRIV3-1). A raw read before any SQLite open (PRIV3-3).
- The app's storage listed by location for both sandbox readings, plus the app's crash reports (PRIV-12's remainder).
- One storage and byte case run on a Release build (PRIV3-6).
- **No over-collection.** The new rows and states add no personal field: R8.1g, R8.10's new clause, E34, E4 "all items", E6 "full" and "elsewhere", and E17 "restore". HISTORY_READINGS_CEILING and DELETE_WRITE_BUDGET are constants. M4 still reads a count only.

| Row ID | disposition |
|---|---|
| R1.1 | ABSTAIN (out of lens) |
| R1.2 | ABSTAIN (out of lens) |
| R1.3 | OBJECT (PRIV3-2) |
| R1.4 | OBJECT (PRIV3-1, PRIV3-2) |
| R1.5 | ABSTAIN (out of lens) |
| R1.6 | ALIGN |
| R1.7 | ALIGN |
| R1.8 | ALIGN |
| R1.9 | ALIGN |
| R1.10 | ALIGN |
| R2.1 | ABSTAIN (out of lens) |
| R2.2 | ABSTAIN (out of lens) |
| R2.3 | ABSTAIN (out of lens) |
| R2.4 | ABSTAIN (out of lens) |
| R2.4a | ABSTAIN (out of lens) |
| R2.4b | ABSTAIN (out of lens) |
| R2.4c | ABSTAIN (out of lens) |
| R2.4d | ABSTAIN (out of lens) |
| R2.4e | ABSTAIN (out of lens) |
| R2.4f | ABSTAIN (out of lens) |
| R2.4g | ABSTAIN (out of lens) |
| R2.4h | ABSTAIN (out of lens) |
| R2.4i | ABSTAIN (out of lens) |
| R2.5 | ABSTAIN (out of lens) |
| R2.6 | ABSTAIN (out of lens) |
| R2.7 | ABSTAIN (out of lens) |
| R2.8 | ABSTAIN (out of lens) |
| R2.9 | ABSTAIN (out of lens) |
| R2.10 | ALIGN |
| R2.11 | ABSTAIN (out of lens) |
| R3.1 | ALIGN |
| R3.2 | ABSTAIN (out of lens) |
| R3.3 | ABSTAIN (out of lens) |
| R3.4 | ABSTAIN (out of lens) |
| R3.5 | ABSTAIN (out of lens) |
| R3.6 | ALIGN |
| R3.7 | ALIGN |
| R3.8 | ABSTAIN (out of lens) |
| R3.9 | ABSTAIN (out of lens) |
| R4.1 | ABSTAIN (out of lens) |
| R4.2 | ALIGN |
| R4.2a | ALIGN |
| R4.2b | ABSTAIN (out of lens) |
| R4.2c | ABSTAIN (out of lens) |
| R4.2d | ALIGN |
| R4.2e | ABSTAIN (out of lens) |
| R4.2f | ABSTAIN (out of lens) |
| R4.2g | ABSTAIN (out of lens) |
| R4.2h | ABSTAIN (out of lens) |
| R4.3 | OBJECT (PRIV3-1, PRIV3-2) |
| R4.4 | ALIGN |
| R4.5 | OBJECT (PRIV3-2) |
| R4.6 | ABSTAIN (out of lens) |
| R4.7 | ALIGN |
| R4.8 | ALIGN |
| R4.9 | ABSTAIN (out of lens) |
| R5.1 | ABSTAIN (out of lens) |
| R5.2 | ABSTAIN (out of lens) |
| R5.2a | ABSTAIN (out of lens) |
| R5.2b | ALIGN |
| R5.2c | ABSTAIN (out of lens) |
| R5.2d | ABSTAIN (out of lens) |
| R5.2e | ABSTAIN (out of lens) |
| R5.3 | ABSTAIN (out of lens) |
| R5.4 | ABSTAIN (out of lens) |
| R5.5 | ALIGN |
| R5.6 | ALIGN |
| R5.7 | ABSTAIN (out of lens) |
| R5.8 | ABSTAIN (out of lens) |
| R6.1 | ABSTAIN (out of lens) |
| R6.2 | OBJECT (PRIV3-2) |
| R6.3 | OBJECT (PRIV3-1, PRIV3-2) |
| R6.4 | ABSTAIN (out of lens) |
| R7.1 | ALIGN |
| R7.2 | ALIGN |
| R8.1 | ABSTAIN (out of lens) |
| R8.1a | ABSTAIN (out of lens) |
| R8.1b | ABSTAIN (out of lens) |
| R8.1c | ABSTAIN (out of lens) |
| R8.1d | ABSTAIN (out of lens) |
| R8.1e | ABSTAIN (out of lens) |
| R8.1f | ABSTAIN (out of lens) |
| R8.1g | ALIGN |
| R8.2 | OBJECT (PRIV3-2) |
| R8.3 | ABSTAIN (out of lens) |
| R8.4 | ALIGN |
| R8.5 | ALIGN |
| R8.6 | ALIGN |
| R8.7 | ABSTAIN (out of lens) |
| R8.8 | ALIGN |
| R8.9 | ABSTAIN (out of lens) |
| R8.10 | OBJECT (PRIV3-5) |
| R8.10a | ALIGN |
| R8.10b | ALIGN |
| R8.10c | ABSTAIN (out of lens) |
| R8.10d | OBJECT (PRIV3-1, PRIV3-3) |
| R8.10e | OBJECT (PRIV-12) |
| R8.10f | ABSTAIN (out of lens) |
| R8.11 | ABSTAIN (out of lens) |
| E1 | ABSTAIN (out of lens) |
| E2 | ABSTAIN (out of lens) |
| E3 | ABSTAIN (out of lens) |
| E4 | ALIGN |
| E5 | ABSTAIN (out of lens) |
| E6 | ABSTAIN (out of lens) |
| E7 | ABSTAIN (out of lens) |
| E8 | ALIGN |
| E9 | ALIGN |
| E10 | ALIGN |
| E11 | ABSTAIN (out of lens) |
| E12 | ABSTAIN (out of lens) |
| E13 | ABSTAIN (out of lens) |
| E14 | ABSTAIN (out of lens) |
| E15 | ABSTAIN (out of lens) |
| E16 | ABSTAIN (out of lens) |
| E17 | ALIGN |
| E18 | ABSTAIN (out of lens) |
| E19 | ABSTAIN (out of lens) |
| M1 | ABSTAIN (out of lens) |
| M2 | ABSTAIN (out of lens) |
| M3 | ABSTAIN (out of lens) |
| M4 | ALIGN |

#### peer-product-marketing-manager-reviewer (Claude route)

## Verdict
Lands & honest once one Major is fixed. All 19 of my round-2 items (the Blockers, the carried PARTIALs and the unrated items) are resolved in the text. The round-2 fixes added one misleading recovery instruction in the new DF E34, which assumes the answer to the still-open sandbox decision (ADR-0007). They also added four smaller wording defects.

## Audience & message context (brief)
- **Reader.** The Cataloger: a non-expert who has scanned a physical colour collection and is now browsing and fixing it.
- **What they should come away with.** Which colours on screen are true, which numbers they can trust, what each action will do to their data, and how to get unstuck when the app refuses something.
- **Surface.** In-app UI copy: E1–E19, the Display labels tables, and the sibling states this round touched (DF E11, E26, E33 and the new E34; Capture R5.8 and R6.6 wording; the post-lock lines).
- **Register.** Calm, plain and precise. No launch narrative is needed.
- **What I read.** All four target files, the fence file through F134, both review-log rounds, the round-2 fix file, and `git diff 7861221 HEAD -- docs ':!docs/agent-reviews'`.

## Findings

**Path key**
- copy = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode-copy.md
- prd = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode.md
- journeys = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode-journeys.md
- fences = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/collection-mode/prd-collection-mode-fences.md
- DF copy = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/data-foundation/prd-data-foundation-copy.md
- DF prd = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/data-foundation/prd-data-foundation.md
- Capture prd = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/capture-mode/prd-capture-mode.md
- Capture copy = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/capture-mode/prd-capture-mode-copy.md
- post-lock = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/product/post-lock.md
- ADR queue = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/docs/decisions/README.md

### (1) Round-2 delta: my lens's findings

| Round-2 finding | Result | Evidence |
|---|---|---|
| N-B1 (Blocker): E12 filed Outside sRGB under "The chip isn't the true colour" | RESOLVED | copy:219 ("Colour beyond a limit." … "Without Can't show beside it, the chip is still the true colour."); copy:301–302. Agrees with UJ2.1-c at journeys:202 (ZX-002 on P3: outside-sRGB, no cannot-show, unclipped triplet). |
| N-B2 (Blocker): E8's undo promise outlived R4.7 | RESOLVED | copy:173 and copy:175 against prd:496 (R4.7, F104). journeys:389, :395 and :396 assert the actions that end undo and the ones that don't. A delete after a clear is now a change the copy names. A narrower leftover case is N3-m3. |
| N-m1: DF E11/E26 "not yet settled" | RESOLVED | DF copy:38 and :39 now say "marked as awaiting your answer", matching the legend at copy:219 and :311 |
| N-m2: "samples disagreed" meant a cause and a mark | RESOLVED | copy:305 (filter label "Samples disagreed, average accepted"); copy:219 (set-aside sentence). Checked true against Capture copy:54. |
| N-m3: E9 headline read as the search scope | RESOLVED | copy:182 |
| N-m4: "observer" jargon on the main surface | RESOLVED | copy:123, :125, :228, :230 |
| N-m5: E17 described an action absent at P0 | RESOLVED | copy:267 (P0 body); copy:269 ("restore" variant, `[phase: variant-absent]`); prd:521; journeys:352 |
| N-m6: DF E33 dropped its scope sentence at P1 | RESOLVED | DF copy:27 (the ‹P1› sentence opens "Export first saves all of ⟨collection⟩, these swatches included") |
| N-n1: Spread read as an earlier-readings mark | RESOLVED | copy:212, copy:219 |
| N-n2: "from the swatch" | RESOLVED | copy:219 ("from its swatch detail") |
| N-n3: R5.2b labelled "Instrument" | RESOLVED | copy:347 |
| N-n4: the not-compared line said "measured under" | RESOLVED as worded. The wording I proposed leaves out the observer; see N3-m4. | copy:351 |
| N-n5: two deferred wording drifts untracked | RESOLVED | post-lock:66 (the Device line, F126); Capture prd:215 (R5.8) and :233 (R6.6) now read "flagged as missing or damaged" |
| carried PMM Minor 4: E8 over-promised undo | RESOLVED | as N-B2 |
| carried PMM Minor 8: DF E33 scope at P1 | RESOLVED | DF copy:27 |
| carried PMM Minor 9: E17 sentence rendered at P0 | RESOLVED | copy:267–269 |
| carried PMM Nit 5: Device's "simulated badge" | RESOLVED as a tracked deferral | post-lock:66. The Device PRD's line 266 is unchanged, as F81 and F126 intend. |
| unrated: E19 should say a column is permanent | RESOLVED | copy:288 |
| unrated: the F82 check should run on a P3 display | RESOLVED | prd:740–742 (F134) |

### (2) New defects introduced by the round-2 fixes

[MAJOR] **N3-M1: DF E34's recovery sentence assumes one permission model while ADR-0007 is still open.** DF copy:29: "Make sure you can still change that file — its Sharing & Permissions in Finder's Get Info — then try again."
- **Reader reaction.** A Mac app loses write access to an open file in at least three ways:
  1. The file's own permissions or ACL changed. Get Info fixes this.
  2. The file is in a privacy-protected location (Documents, Desktop, Downloads, iCloud Drive, or a removable or network volume), and the app's Files and Folders access was turned off in System Settings › Privacy & Security. Get Info still shows Read & Write and fixes nothing.
  3. In a sandboxed build, the access the user granted by choosing the file is lost. Get Info can't fix this either.

  There is also a fourth case the pointer gets wrong: a Locked file is set in Get Info's General section, not Sharing & Permissions.

  In cases 2 and 3 the Cataloger follows the instruction and finds nothing wrong. They press Try again and get E34 again. The only other exit is OK, which discards what they did.
- **Why it is against the brief.** Whether case 3 exists at all is ADR-0007, which is still queued (ADR queue line 25). DF's own copy rule says every promise is "backed by a requirement row" (DF copy:12), and no row backs this pointer. DF R7.6p (DF prd:88) tests only what E34 names as unsaved, that nothing changed, and its actions.
- **Why Major at the buildability bar.** R8.8 (prd:590) sends "permission to the file was lost" to E34, and R8.10a (prd:598) makes "the app's permission to it" a declared test input. Neither says which kinds of denial count. E34's pointer is the only text that narrows it, and it narrows it to file permissions. So a builder must guess:
  - If a privacy-folder or sandbox denial counts as lost permission, E34 renders with instructions that don't work.
  - If it doesn't count, no state is named for it, which breaks DF R1.10's rule that a refused write is named rather than silently dropped (DF prd:107).
  - DF's case at DF journeys:37 ("restore permission and choose Try again") has a route the user can reach only for case 1.
- **Trace.** Take an unsandboxed build with the file in ~/Documents, and revoke access through Files and Folders. E34 renders and UJ9.7-g passes. Yet the only recovery E34 offers doesn't restore access, and Try again brings E34 back.
- **Rewrite** (DF copy only; no PRD body words):
  - E34 body: "SpectroCapture is no longer allowed to change your file at ⟨path⟩, so the last thing you did wasn't saved and nothing in your file changed. Check that the file isn't locked and that SpectroCapture is still allowed to change it, then try again."
  - Add an input to the ADR-0007 row: E34's recovery sentence names where access is restored (Finder's Get Info, Privacy & Security's Files and Folders, or choosing the file again) once 0007 picks the model.
  - Record it in a dated line under DF F53.

[MINOR] **N3-m1: E6's "elsewhere" variant describes the queue the owner turned down.** copy:156: "While any session is running, a change that touches many swatches at once waits until it ends… Nothing has been changed — do it again once the session has ended."
- **The conflict.** F100 (fences:970) chose to refuse these writes and explicitly did not choose "bulk writes that yield or queue behind capture". "Waits until it ends" says the change is queued; the next sentence says it wasn't made.
- **Reader reaction.** A Cataloger sets Family on 200 Studio Markers swatches during a Gouache Set session. They read the first sentence, expect the change to land when scanning ends, and walk away. It never lands. This is the same queue reading that round-1 Minor 1 removed from E6's body.
- **Second problem.** "Running" contradicts the "active, paused or halted" in the sentence before it. A Cataloger whose session is paused reads that the rule doesn't apply to them.
- **Rewrite** (copy only; the four cases that assert this variant check it by name):
  - "Scanning in ⟨collection⟩ is active, paused or halted. Until that session ends, changes that touch many swatches at once aren't available in any collection, so the session's saves aren't held up. Nothing has been changed — do it again once the session has ended."

[MINOR] **N3-m2: if R1.7 is deferred, E6 never names the action that was refused.**
- **The gate.** The "full" variant (copy:155) and R8.3 (prd:585) wait until "every P1 action R8.3 names" is built. R8.3 names E10's "Undo".
- **Why it may never open.** R1.7 (prd:410) and OQ 10 (prd:761) defer R1.7 if OQ 10 is still open at v1 release. In that case "full" never renders.
- **What v1 shows instead.** For a refused Flag, "Change code", undo of a code change, or "Set a field" on the session's collection, the user gets the P0 body (copy:152). It lists only "Deleting a swatch or the collection and bringing back an earlier reading", and that list reads as complete. The Cataloger who just pressed Flag is told other things are blocked and not why their own action was refused.
- **Why only Minor.** The headline and "Nothing has been changed — do it again…" still get the main message across.
- **Fix** (stays inside F111's pattern):
  - R8.3: "once every P1 action it names lands" becomes "once every P1 action it names but E10's 'Undo' lands" (+3 words).
  - Pay for it by removing the rule-free status clause "nothing of this app is measured" from OQ 1's Decision cell (prd:752, 6 words).
  - The "full" variant drops "and undoing a delete", and its condition line (copy:155) follows R8.3.
  - The only way E10's Undo can be refused in flight is a delete made before the session started, and the headline covers that.
  - The journeys preamble (journeys:50) already keys on what the variant names, so it follows without change.

[MINOR] **N3-m3: E8's undo window has a case the reader can't see coming.** copy:173 and :175: "…or you change anything other than swatch details, codes or names."
- **The gap.** R4.7 (prd:496) also ends the undo history when a column is hidden or shown, or on "New collection" (UJ6.4-l, journeys:395). Nobody thinks of hiding a column as changing something, and no copy says that choice is saved in the file (R2.10).
- **Scenario.** A Cataloger clears Family on 200 swatches, hides Spread to see better, then realises the clear was wrong. Undo is gone, and E8 has already told them "What was there isn't kept in your file". This is the N-B2 loss on a narrower path.
- **Rewrite** (under-promises, so it is safe in every phase):
  - "…Undo change puts it back until the file closes, you read the file again, or you do anything other than edit swatch details, codes or names, however small — scanning doesn't count."

[MINOR] **N3-m4: the "not compared" reasons leave out the viewing angle, and E9 makes unscanned swatches sound like a condition problem.**
- **Where.** E9 (copy:183, :185) says "worked out under a different light or measurement condition". The R5.4 line (copy:351) uses the same words, because I asked for them in N-n4.
- **The mismatch.** R3.7 (prd:463) and R5.4 (prd:522) exclude on illuminant, observer and condition, and E3/E13 now call the observer the "viewing angle" (copy:123).
- **Reader reaction.** An All-items Find similar from a D50/2° swatch excludes swatches from a D50/10° collection; UJ9.5-b declares exactly that mix. E9 blames the light or the condition. The Cataloger checks both, finds them equal, and stops trusting the explanation.
- **Second problem.** The round-2 phrase "no colour in their own collection's measurement condition" also covers swatches that were simply never scanned, so a Cataloger with 10 unscanned swatches reads it as a condition fault.
- **Rewrite** (copy only):
  - E9: "⟨excluded⟩ swatches weren't compared: they have no colour, or none under their collection's measurement condition, or theirs was worked out for a different light, viewing angle or measurement condition."
  - R5.4 line: "Not compared — worked out for a different light, viewing angle or measurement condition".

[NIT] **N3-n1: E34's "OK" silently discards the unsaved change, and its reassurance is not hedged.** DF copy:29.
- Every other "OK" in DF copy (E1, E16, E4, E31) only dismisses information. This one throws away the user's change. Add "…then try again; OK leaves it unsaved."
- E15 hedges its reassurance with "On a local disk…" (DF copy:28), and DF R1.7 (DF prd:105) doesn't promise atomic writes on network volumes. E34's "nothing in your file changed" should carry the same hedge unless DF can rule out a permission refusal partway through a write.

[NIT] **N3-n2: the copy file's list of sibling-owned states omits DF E34.** copy:94–97 lists DF's "E4, E8, E9, E10, E11, E14, E15, E26, E31 and E33". E34 renders on this product's surfaces (prd:590; Build dependencies at prd:300; journeys:522), so add it.

## Biggest risks   (what misleads, confuses, or loses the reader)
- **A recovery instruction that dead-ends.** For a common unsandboxed cause (a privacy-protected folder), and for the whole sandboxed option, E34 sends the Cataloger to a pane that can't help. That turns Try again into a loop (N3-M1). It is also the only copy that commits to an answer on ADR-0007.
- **Refusal copy that sounds like a queue.** E6's "elsewhere" variant says the change "waits", which is the design the owner rejected. The user may believe a 200-swatch change is still pending (N3-m1).
- **Explanations that don't match the cause.** An E6 whose list never includes the refused action (N3-m2), and "not compared" reasons that leave out the viewing angle (N3-m4). Each teaches the Cataloger that the app's explanations can't be trusted, which is the opposite of what the honesty legend is for.

## Genuinely strong   (incl. where plain-and-honest is right that a marketing-zealot would over-hype)
- **The legend now says when to trust the chip.** "Without Can't show beside it, the chip is still the true colour" is a positive trust statement, rarer and more useful than another warning. It also matches UJ2.1-c on P3.
- **E8's undo promise is now tested.** It matches R4.7, and UJ6.4-f, -l and -m assert every action that ends it and every one that doesn't.
- **The phase discipline holds.** E17's P0 body promises nothing that isn't there, and the restore sentence waits in a marked variant.
- **E19 states the consequence before the choice.** "A column can't be removed once it's added" is true under F63.
- **Vocabulary now matches across the seam.** "Awaiting your answer" (DF E11, E26 and the legend), "flagged as missing or damaged" (Capture's rows and its table), and the filter label "Samples disagreed, average accepted" each give one word one meaning.
- **The register stays calm.** There are no speed, quality or competitor claims. E9 still limits itself to "never a named colour library". For in-app copy, plain and honest is correct.

## Missing / over-hyped
- **Over-hyped:** nothing.
- **Missing (out of my lens; a pointer only):**
  - R8.3 (prd:585) refuses E10's "Undo" only on the session's own collection. Yet R8.1g (prd:580) classes E10's undo of a selection or collection delete as a 10-second DELETE_WRITE_BUDGET write.
  - So undoing a collection delete in collection B while a session on A is in flight is allowed. That is the kind of write F100 was meant to keep away from capture's saves (R8.11).
  - It also makes the general statement in E6's "elsewhere" variant slightly broader than the rows deliver.
  - Worth a performance and architecture check against F100's scope. I have not graded it.
- **Optional:** once ADR-0007 lands, add E34 to the F134-style owner reading: revoke access the way the chosen model allows, follow the copy, and confirm it gets the user back.

Disposition note: I OBJECT only on Major and Minor findings; rows touched only by a Nit are ALIGN.

| Row ID | disposition |
|---|---|
| R1.1 | ALIGN |
| R1.2 | ALIGN |
| R1.3 | ALIGN |
| R1.4 | ALIGN |
| R1.5 | ALIGN |
| R1.6 | ALIGN |
| R1.7 | ALIGN |
| R1.8 | ALIGN |
| R1.9 | ALIGN |
| R1.10 | ALIGN |
| R2.1 | ALIGN |
| R2.2 | ALIGN |
| R2.3 | ALIGN |
| R2.4 | ALIGN |
| R2.4a | ALIGN |
| R2.4b | ALIGN |
| R2.4c | ALIGN |
| R2.4d | ALIGN |
| R2.4e | ALIGN |
| R2.4f | ALIGN |
| R2.4g | ALIGN |
| R2.4h | ALIGN |
| R2.4i | ALIGN |
| R2.5 | ABSTAIN |
| R2.6 | ALIGN |
| R2.7 | ALIGN |
| R2.8 | ALIGN |
| R2.9 | ALIGN |
| R2.10 | ALIGN |
| R2.11 | ALIGN |
| R3.1 | ALIGN |
| R3.2 | ABSTAIN |
| R3.3 | ALIGN |
| R3.4 | ALIGN |
| R3.5 | ALIGN |
| R3.6 | ABSTAIN |
| R3.7 | ALIGN |
| R3.8 | ALIGN |
| R3.9 | ABSTAIN |
| R4.1 | ABSTAIN |
| R4.2 | ALIGN |
| R4.2a | ALIGN |
| R4.2b | ALIGN |
| R4.2c | ALIGN |
| R4.2d | ALIGN |
| R4.2e | ALIGN |
| R4.2f | ALIGN |
| R4.2g | ALIGN |
| R4.2h | ALIGN |
| R4.3 | ALIGN |
| R4.4 | ALIGN |
| R4.5 | ALIGN |
| R4.6 | ALIGN |
| R4.7 | ALIGN |
| R4.8 | ALIGN |
| R4.9 | ALIGN |
| R5.1 | ALIGN |
| R5.2 | ALIGN |
| R5.2a | ALIGN |
| R5.2b | ALIGN |
| R5.2c | ALIGN |
| R5.2d | ALIGN |
| R5.2e | ALIGN |
| R5.3 | ALIGN |
| R5.4 | OBJECT (N3-m4) |
| R5.5 | ALIGN |
| R5.6 | ALIGN |
| R5.7 | ALIGN |
| R5.8 | ABSTAIN |
| R6.1 | ABSTAIN |
| R6.2 | ALIGN |
| R6.3 | ALIGN |
| R6.4 | ABSTAIN |
| R7.1 | ABSTAIN |
| R7.2 | ABSTAIN |
| R8.1 | ABSTAIN |
| R8.1a | ABSTAIN |
| R8.1b | ABSTAIN |
| R8.1c | ABSTAIN |
| R8.1d | ABSTAIN |
| R8.1e | ABSTAIN |
| R8.1f | ABSTAIN |
| R8.1g | ABSTAIN |
| R8.2 | ABSTAIN |
| R8.3 | OBJECT (N3-m2) |
| R8.4 | ALIGN |
| R8.5 | ABSTAIN |
| R8.6 | ABSTAIN |
| R8.7 | ABSTAIN |
| R8.8 | OBJECT (N3-M1) |
| R8.9 | ALIGN |
| R8.10 | ABSTAIN |
| R8.10a | ABSTAIN |
| R8.10b | ABSTAIN |
| R8.10c | ABSTAIN |
| R8.10d | ABSTAIN |
| R8.10e | ABSTAIN |
| R8.10f | ABSTAIN |
| R8.11 | ABSTAIN |
| E1 | ALIGN |
| E2 | ALIGN |
| E3 | ALIGN |
| E4 | ALIGN |
| E5 | ALIGN |
| E6 | OBJECT (N3-m1; N3-m2) |
| E7 | ALIGN |
| E8 | OBJECT (N3-m3) |
| E9 | OBJECT (N3-m4) |
| E10 | ALIGN |
| E11 | ALIGN |
| E12 | ALIGN |
| E13 | ALIGN |
| E14 | ALIGN |
| E15 | ALIGN |
| E16 | ALIGN |
| E17 | ALIGN |
| E18 | ALIGN |
| E19 | ALIGN |
| M1 | ABSTAIN |
| M2 | ABSTAIN |
| M3 | ABSTAIN |
| M4 | ALIGN |

#### peer-architecture-reviewer (Claude route)

## Verdict
Sound, build it. Of my 10 round-2 findings, 8 are resolved and 2 are partial (A18 and A22). The fix pass adds no Blocker. It adds one Major: E10's "Undo" of a multi-item delete on another collection is still allowed while a session is in flight. That is the largest write this PRD makes, and F100 meant to take bulk writes off capture's path. R1.7 is P1 and waits on OQ 10, so this does not block the P0 build. It should still close before lock.

## Architecture in brief
- **What Collection Mode owns.** It is the browsing and editing layer over Data Foundation's single-writer SQLite file. It owns in-memory view state (search, filters, sorts, the metadata undo history), the one live-computed mark (cannot-show), and entry points into actions its sibling PRDs own.
- **How round 2 settled the four storage seams.** Each was settled by an owner fence and mirrored in the sibling PRDs:
  - **Contention with capture** — refused rather than scheduled. F100 means no bulk writes while any session is in flight, and no ADR-0005 input was added.
  - **Byte erasure** — made strict. F102: removed text is gone from the moment the write lands, side files included.
  - **File-wide search** — sized at full imported width. F103 hands the index-or-scan choice to ADR-0003.
  - **Stored gamut flag** — made one computation with the display check. F127 defines DF R3.4 through OQ 7's interim.
- **The key tradeoff.** Simple, testable refusals and strict privacy rules are bought at the price of new synchronisation points in the storage layer: erasure against readers of the file, and search against cold-open time. ADR-0003 must now carry those points.

## Findings
Paths are relative to /Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode/:
- prd = docs/product/collection-mode/prd-collection-mode.md
- jny = …/prd-collection-mode-journeys.md
- copy = …/prd-collection-mode-copy.md
- r2fix = …/prd-collection-mode-round-2-fixes.md
- DF = docs/product/data-foundation/prd-data-foundation.md
- DFcopy = …/prd-data-foundation-copy.md
- CAP = docs/product/capture-mode/prd-capture-mode.md
- ADRQ = docs/decisions/README.md

### (1) Round-2 delta: my findings (the boxes are at r2fix:429-455)

| Round-2 ID | Result | Evidence |
|---|---|---|
| A12 (Minor): bulk delete and "Use as scan order" untimed | RESOLVED | prd:279 (DELETE_WRITE_BUDGET); prd:580 (R8.1g); jny:477 (UJ9.5-g times the selection delete, the collection delete, their E10 undo, and "Use as scan order" at ROWS_CEILING) |
| A16 (Major): byte-erasure hand-off weaker than the rows | RESOLVED | prd:174-175 (Vocabulary: "the file's bytes" includes every side file); prd:590 (R8.8: "from the moment it lands, open or after a crash"); prd:629 (Outbound DF row); ADRQ:22; DF:116 (R2.3); DF:230 (R6.2a). The residual wording is NIT-1. |
| A17 (Minor): gamut edge judged by two computations | RESOLVED | DF:146 (R3.4 is tested by OQ 7's interim at sRGB); prd:758 (OQ 7); ADRQ:22; jny:216 (UJ2.1-q); jny:69-70 (margin exemption). A new coupling issue is ARCH3-5. |
| A18 (Minor): capture precedence on a single-writer file | PARTIAL | prd:585 (R8.3 refuses the four bulk actions on any collection); jny:474 (UJ9.5-d is now P0 and can fail); CAP:386 (the F72 mirror). The gap left is E10's "Undo" of a multi-item delete elsewhere (ARCH3-1). The refresh-scheduling half now has a P0 case, and I do not re-open F100's decision not to engineer it under ADR-0005. |
| A19 (Minor): All items search workload unsized | RESOLVED | prd:584 (R8.2 at full width, "as R8.1d does"); jny:472 (UJ9.5-b declares the width); ADRQ:22. The hand-off is incomplete; see ARCH3-3. |
| A20 (Minor): R8.8's permission-lost path had no stop or interim | RESOLVED | prd:590 (R8.8 renders DF E34); prd:300 (Build dependencies row 1); DF:107 (R1.10); DFcopy:29 (E34); jny:486 (UJ9.7-g). A new ADR-0007 issue in E34's copy is ARCH3-6. |
| A21 (Nit): "its container" assumed a sandbox | RESOLVED | prd:602 (R8.10e: "wherever the build puts it") |
| A22 (Nit): does a capture-side commit end the undo history? | PARTIAL | prd:496 exempts only "a capture save or a session starting". Every other session write (a jump, a Flag, a set-aside, pause or end, a reorder in the queue list) still ends the history, while E8 (copy:173, 175) says "scanning doesn't count". See ARCH3-2. |
| A23 (Nit): no route for an item a re-read finds removed | RESOLVED | prd:144; prd:226-227 (the new state, removed) |
| A24 (Nit): Find similar depends on which derived sets happen to be persisted | RESOLVED | prd:463 (R3.7 compares working-set values only); prd:464 (R3.8) |

### (2) New defects introduced by the round-2 fixes

**[MAJOR] ARCH3-1 — R8.3 and F100: E10's "Undo" of a selection or collection delete on another collection is the one bulk write still allowed while a session is in flight.**
- **Where it sits.** R8.3 (prd:585) puts E10's "Undo" in its "on that collection" list, so only the in-flight collection's undo is refused. The four bulk actions are refused "on any collection".
- **Scenario.**
  1. With R1.7 built, delete Gouache Set.
  2. Bring a bulk session in flight on Studio Markers.
  3. Fire "Undo" on E10.
  4. Nothing refuses it. It restores up to ROWS_CEILING items with their readings. R8.1g (prd:580) allows that DELETE_WRITE_BUDGET, 10 s, five times the 2 s write F100 was chosen to remove. R8.8 (prd:590) makes it atomic on the single writer.
  5. R8.11 (prd:607) forbids delaying the session's row confirmation.
- **Contradiction in the sibling.** The Capture mirror states the property as holding: "so none holds up a session's saves" (CAP:386, Capture F72). It does not hold on this path.
- **No case covers it.** UJ4.4-g (jny:299) tests only the same-collection undo, and UJ9.5-g (jny:477) runs with no session in flight.
- **The builder's guess.** Whoever builds R1.7 must choose between refusing it (unstated), yielding it (the ADR-0005 path F100 declined), or chunking it (which breaks R8.8).
- **Fix (word-neutral).**
  - In R8.3, move "E10's 'Undo'" from the "on that collection" group to the "on any collection" group ("…and E10's 'Undo', 'Set a field', 'Delete selected', 'Delete collection' and 'Use as scan order' on any collection").
  - Add a dated Clarified line under F100, whose title and "applies to a session on any collection" cover this. Get owner confirmation, because F100's quoted option lists four actions.
  - Mirror it in Capture F72 and CAP:386.
  - Add a case: Gouache Set deleted; a session in flight on Studio Markers in each in-flight state; "Undo" on E10 renders E6 "elsewhere", and E2 does not list Gouache Set.
  - If the owner wants single-item undo elsewhere to stay available, use "E10's 'Undo' on that collection, or of more than one swatch on any" instead. That adds about 7 words; pay for them from the fence-provenance parentheses "(F65)" and "(F124)" at prd:314-315.

**[MINOR] ARCH3-2 — R4.7 against the capture PRD's own writes: the undo-history exemption depends on how capture's writes are grouped into transactions, and E8 promises more.**
- **What R4.7 says.** It ends the history on "any other committed write … a capture save or a session starting not" (prd:496).
- **What else capture commits.**
  - A jump makes the target row the remembered row (Capture R6.2, and R3.6 at CAP:160).
  - Capture's own Flag (its R5.8), a set-aside by the guard, a pause or session end, and a reorder in the queue list (its R6.8) are all committed writes. None of them is a save or a session start.
- **Scenario.** Set ZX-001's name to Harbour, start a session, and have the operator jump rows. "Undo change" vanishes. E8 (copy:173, 175) told the user "scanning doesn't count".
- **Why a correct build can fail.** Whether a save's own advance of the remembered row counts as part of "a capture save" is a transaction-layout choice (ADR-0003 or the builder). A build that commits the advance separately fails UJ6.4-m (jny:396).
- **Fix: owner, as a refinement of F104.**
  - Replace "a capture save or a session starting not" with "no write a capture session makes counting". That is word-neutral, independent of storage layout, and matches E8 and F104's rationale that each undo re-checks R8.3.
  - Or keep R4.7 as it is, narrow E8 to "saving a scan doesn't count", and add a case showing that a jump ends the history.

**[MINOR] ARCH3-3 — F103's ADR-0003 input leaves out the two conditions that actually decide index or scan.**
- **What the hand-off carries.** ADRQ:22, prd:629 and DF F53(5) hand over the width and the byte rule only.
- **What the rows require.**
  - R3.1 (prd:457) matches imported values anywhere in the text from the first keystroke.
  - R8.2 (prd:584) holds R8.1a from the first rows of the view with "nothing left loading that a search waits on" (R8.1d, prd:577).
  - UJ9.5-b (jny:472) fills 100,000 items × 20 columns × 200 characters, about 400 M characters. The Timing workload (jny:108) types "text nothing matches" at one keystroke per 150 ms.
- **Why neither obvious choice fits alone.**
  - A trigram full-text index, the usual "index" for anywhere-in-text search (SQLite FTS5 trigram), cannot answer a 1–2-character substring and falls back to a table scan.
  - An in-memory scan must load and normalise about 400 MB before the first rows show. That is also a memory cost on the 8 GB interim Mac and a runtime question for ADR-0005.
  - A persisted index also needs FTS5's secure-delete option to meet F102. That option arrived in SQLite 3.42 and bears on DF's SQLITE_READER_FLOOR (its OQ 2).
- **Consequence.** An ADR-0003 that picks "an index" from the row as written can fail UJ9.5-b. This is A16's pattern again: the hand-off is weaker than the rows.
- **Fix (outside the body budget).** Extend the ADRQ:22 input with "answering every keystroke from the first character within BROWSE_RESPONSE_BUDGET, from the first rows of a cold open (R8.2, R8.1d)". Add the same sentence to DF F53(5). The Outbound DF row keeps its words.

**[MINOR] ARCH3-4 — F102 makes the erasure moment a synchronisation point with outside readers, and no row says who waits.**
- **The three rules in play.**
  - DF R1.5 (DF:106): the help docs say reading the file elsewhere while the app is open is safe.
  - F102, with R8.8 (prd:590) and DF R2.3 (DF:116): removed text is gone "from the moment the write lands".
  - R8.11 (prd:607): no Collection Mode write delays capture's row confirmation.
- **Rollback-journal mode with secure_delete meets F102, but** any outside reader's shared lock delays every commit, capture saves included.
- **WAL mode meets F102 only with a truncating checkpoint on each text-removing commit.** That checkpoint waits for readers on older snapshots and blocks new writers while it waits. A passive checkpoint instead leaves the old page in the main file, which breaks F102.
- **Scenario.** An outside reader holds a long read during a session. The user makes a single edit, which R8.3 allows in flight. Either capture's saves stall, or the edit cannot land, or the erasure moment is broken.
- **Before round 2.** The option F102 did not choose, "by the next checkpoint or close", was the escape here.
- **Fix: owner, then an ADR-0003 input.** Name what waits when an outside reader holds a snapshot. Candidates:
  - The text-removing edit shows not-yet-saved until the reader lets go, and capture saves are never queued behind it.
  - Or DF R1.5's help docs say reading the file elsewhere during capture may delay saves.
- **Word budget.** This lands in ADRQ:22 and DF R1.5's help-docs line, both outside the body budget.

**[MINOR] ARCH3-5 — F127: the stored, exported gamut flag now depends on an open question in a downstream PRD.**
- **The dependency.** DF R3.4 (DF:146) defines a fact stored in users' files and carried into export (the export PRD's R2.1) by pointing at CM OQ 7's interim (prd:758). OQ 7 closes through this PRD's results file (prd:768-770).
- **Scenario.** OQ 7 closes after v1 ships its interim with a tolerance or a different adaptation. DF's stored flag then changes meaning with no DF fence and no derivation-version bump named. Every flag already in a user's file, and in any past export, is quietly defined by the old test.
- **A second gap.** DF names no adaptation for the stored sRGB derivation itself (DF:42, DF:143-146). The exported sRGB value and its own flag can disagree near the boundary if the derivation adapts by anything other than Bradford.
- **Fix.**
  - DF R3.4 or DF F53 states the test's parameters itself (Bradford, relative colorimetric, zero tolerance, the same adaptation its sRGB derivation uses), and CM OQ 7 cites DF for the sRGB case.
  - In OQ 7's Decision cell, replace "F40 adds Bradford adaptation at zero tolerance and F64 ratifies the rest of the interim" (15 words of provenance that the Interim cell already restates) with "closing it changes that PRD's R3.4 only through its fence and a new derivation version" (about 15 words).

**[MINOR] ARCH3-6 — DF E34, the sibling half added in this change: its recovery advice assumes a POSIX-permission cause, and ADR-0007 (sandboxing) is still open (ADRQ:26).**
- **What E34 says.** DFcopy:29: "its Sharing & Permissions in Finder's Get Info — then try again", with the actions Try again and OK.
- **Why that advice fails in either build.**
  - A sandboxed build loses access when its security-scoped bookmark goes stale. Get Info cannot fix that, and Try again cannot re-acquire it.
  - An unsandboxed build loses access when Files and Folders or removable-volume consent is withdrawn in System Settings. Get Info shows nothing wrong.
- **The only recovery that works under both readings** is choosing the file again. Yet Build dependencies row 1 (prd:300) lists DF E34 as an available contract with no ADR-0007 gate.
- **Fix (DF copy, outside the CM budget).** Make the body cause-neutral ("check you can still change it, or choose it again, then try again") and add a choose-the-file action through DF R7.3j. Alternatively, gate E34's recovery text on ADR-0007.

**[MINOR] ARCH3-7 — UJ9.4-e's oracle is broader than R8.10 and pre-empts ADR-0007 and ADR-0008.**
- **The mismatch.** UJ9.4-e (jny:470) asserts that no listening socket or cross-process service "is listed" at all. R8.10 (prd:592) and F132 forbid only one opened for a readback.
- **Scenario.** A sandboxed build that updates through Sparkle 2 (AGENTS.md §3's recommendation; ADR-0007 and ADR-0008 are queued) must bundle Sparkle's installer XPC service, and nothing stops ADR-0005 from choosing a helper process. Either fails the case while R8.10 holds.
- **Fix (journeys, outside the budget).** Change the oracle to "no listening socket, and no cross-process service beyond those an Accepted ADR names".

**[NIT] NIT-1 — DF R6.2 (DF:218) and R6.2d (DF:233) still say "unrecoverable from the active file".** They inherit F102 through R2.3 and R6.2a but read narrower on their own. Write "as R6.2a states" (DF, outside the CM budget).

**[NIT] NIT-2 — the Harness's byte check reads a wider set of files than the Vocabulary defines.** The check (jny:53-54) reads "every file beside it", while the Vocabulary (prd:174-175) scopes to files "the app keeps" there. A case whose import CSV sits in the file's folder (UJ6.4-f, UJ6.4-g) would false-match. State that fixture sources sit outside the file's folder, keeping UJ9.8-c's control file as the one deliberate exception.

## Biggest risks
- **ARCH3-1:** the 10 s restore is the one path where capture can still be starved on the single writer, and the Capture mirror (F72) says it cannot happen.
- **ARCH3-4:** F102 removes WAL's easy way out. Outside readers, which DF calls safe, now collide either with erasure or with capture timing, and nobody has decided which gives way.
- **ARCH3-3:** file-wide search at about 400 MB is the scale-limit risk. ADR-0003's input leaves out the conditions (first-character substrings, a cold open with nothing loading) that make the choice between index and scan.
- **ARCH3-5:** a fact that is stored and exported now changes when a downstream PRD closes an open question.

## Genuinely sound
- **F100's refusal is the simplest correct answer to one writer and a hardware-paced loop.** It removes transactions of 2 s and more from capture's path without designing a scheduler.
- **R4.7's re-check already refuses "Undo change" of a bulk set in flight.** The undo re-checks R8.3, which refuses "Set a field" on any collection. A shallow review would flag this as a hole; it is covered.
- **F102 can be met.** Rollback journal with secure_delete works, and so does WAL with a synchronous truncating checkpoint per text-removing commit. The Harness's internal-tables clause (jny:58) catches sqlite_stat4 samples and FTS5 segments, the two classic places deleted text survives.
- **F127's single computation is the right design** for the stored flag and the display check. Only the direction of the dependency is wrong (ARCH3-5).
- **F101 puts the permission-lost state in Data Foundation.** That PRD owns the file and its failure states, so ownership is correct.
- **The new state, removed, keeps the byte guarantee for the app's own deletes.** The app cannot promise erasure for a change it did not make.
- **Test-build-only readbacks with no test server (F132) fit ADR-0001's in-process UniFFI core.**
- **The navigation fork (ADR-0004) is still not selected.**
- **UJ9.5-d is now P0 and can fail.** That is the right empirical gate for keeping UI refreshes from interfering with capture, and cheaper than an ADR-0005 input.

## Missing / over-engineered
- **Missing:**
  - E10's "Undo" in R8.3's "on any collection" group (ARCH3-1);
  - one exemption from undo-history endings covering every session write (ARCH3-2);
  - the first-character and cold-open conditions on the ADR-0003 search input (ARCH3-3);
  - who waits when an outside reader holds a snapshot (ARCH3-4);
  - Data Foundation owning the sRGB test's parameters (ARCH3-5);
  - E34 recovery that works either way ADR-0007 goes (ARCH3-6);
  - UJ9.4-e's oracle scoped to what R8.10 actually forbids (ARCH3-7).
- **Over-engineered:** nothing structural. The byte check's UTF-16, case-folding and per-word breadth is justified by F35's promise.

| Row ID | disposition |
|---|---|
| R1.1 | ALIGN |
| R1.2 | ALIGN |
| R1.3 | ALIGN |
| R1.4 | ALIGN |
| R1.5 | ALIGN |
| R1.6 | ALIGN |
| R1.7 | ALIGN |
| R1.8 | ALIGN |
| R1.9 | ALIGN |
| R1.10 | ALIGN |
| R2.1 | ALIGN |
| R2.2 | ALIGN |
| R2.3 | ALIGN |
| R2.4 | ALIGN |
| R2.4a | ALIGN |
| R2.4b | ALIGN |
| R2.4c | ALIGN |
| R2.4d | ALIGN |
| R2.4e | ALIGN |
| R2.4f | ALIGN |
| R2.4g | ALIGN |
| R2.4h | ALIGN |
| R2.4i | ALIGN |
| R2.5 | OBJECT (ARCH3-5) |
| R2.6 | ALIGN |
| R2.7 | ALIGN |
| R2.8 | ALIGN |
| R2.9 | ALIGN |
| R2.10 | ALIGN |
| R2.11 | ALIGN |
| R3.1 | ALIGN |
| R3.2 | ALIGN |
| R3.3 | ALIGN |
| R3.4 | ALIGN |
| R3.5 | ALIGN |
| R3.6 | ALIGN |
| R3.7 | ALIGN |
| R3.8 | ALIGN |
| R3.9 | ALIGN |
| R4.1 | ALIGN |
| R4.2 | ALIGN |
| R4.2a | ALIGN |
| R4.2b | ALIGN |
| R4.2c | ALIGN |
| R4.2d | ALIGN |
| R4.2e | ALIGN |
| R4.2f | ALIGN |
| R4.2g | ALIGN |
| R4.2h | ALIGN |
| R4.3 | ALIGN |
| R4.4 | ALIGN |
| R4.5 | ALIGN |
| R4.6 | ALIGN |
| R4.7 | OBJECT (ARCH3-2) |
| R4.8 | ALIGN |
| R4.9 | ALIGN |
| R5.1 | ALIGN |
| R5.2 | ALIGN |
| R5.2a | ALIGN |
| R5.2b | ALIGN |
| R5.2c | ALIGN |
| R5.2d | ALIGN |
| R5.2e | ALIGN |
| R5.3 | ALIGN |
| R5.4 | ALIGN |
| R5.5 | ALIGN |
| R5.6 | ALIGN |
| R5.7 | ALIGN |
| R5.8 | ALIGN |
| R6.1 | ALIGN |
| R6.2 | ALIGN |
| R6.3 | ALIGN |
| R6.4 | ALIGN |
| R7.1 | ALIGN |
| R7.2 | ALIGN |
| R8.1 | ALIGN |
| R8.1a | ALIGN |
| R8.1b | ALIGN |
| R8.1c | ALIGN |
| R8.1d | ALIGN |
| R8.1e | ALIGN |
| R8.1f | ALIGN |
| R8.1g | ALIGN |
| R8.2 | OBJECT (ARCH3-3) |
| R8.3 | OBJECT (ARCH3-1) |
| R8.4 | ALIGN |
| R8.5 | ALIGN |
| R8.6 | ALIGN |
| R8.7 | ALIGN |
| R8.8 | ALIGN |
| R8.9 | ALIGN |
| R8.10 | OBJECT (ARCH3-7) |
| R8.10a | ALIGN |
| R8.10b | ALIGN |
| R8.10c | ALIGN |
| R8.10d | ALIGN |
| R8.10e | ALIGN |
| R8.10f | ALIGN |
| R8.11 | OBJECT (ARCH3-4) |
| E1 | ALIGN |
| E2 | ALIGN |
| E3 | ALIGN |
| E4 | ALIGN |
| E5 | ALIGN |
| E6 | ALIGN |
| E7 | ALIGN |
| E8 | OBJECT (ARCH3-2) |
| E9 | ALIGN |
| E10 | ALIGN |
| E11 | ALIGN |
| E12 | ALIGN |
| E13 | ALIGN |
| E14 | ALIGN |
| E15 | ALIGN |
| E16 | ALIGN |
| E17 | ALIGN |
| E18 | ALIGN |
| E19 | ALIGN |
| M1 | ALIGN |
| M2 | ALIGN |
| M3 | ALIGN |
| M4 | ALIGN |

#### peer-performance-reviewer (Claude route)

## Verdict
**Meets budget.** No Blocker remains, and 11 of my 12 round-2 findings are resolved (PERF2-5 is partial). Two new Majors should land before R8.2, R8.3 and R8.11 flip:
- **ADR-0003's search input is too loose.** It lets file-wide search use "an index or a scan, either one". Under F102's byte rule, SQLite's stock full-text index measured 39 s per 1,000 deleted items on the fastest Mac. That is about 200 times over the budgets it would have to meet.
- **F100 only works one way.** It refuses bulk writes once a session is in flight. Nothing covers a session started while a bulk write already holds the file's only writer, and that hold can now last up to 10 s (DELETE_WRITE_BUDGET).

## Workload & budget (brief)
- **Paths.** Root: `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-collection-mode`. Short names:
  - `prd` = docs/product/collection-mode/prd-collection-mode.md
  - `jny` = …/prd-collection-mode-journeys.md
  - `fen` = …/prd-collection-mode-fences.md
  - `capture` = docs/product/capture-mode/prd-capture-mode.md
  - `capfen` = …/prd-capture-mode-fences.md
  - `df` = docs/product/data-foundation/prd-data-foundation.md
  - `adr` = docs/decisions/README.md
- **Scale.**
  - ROWS_CEILING is 10k items; IMPORTED_COLUMNS_CEILING is 20 columns × 200 characters; FILE_ITEMS_CEILING is 100k items.
  - Under F103, UJ9.5-b's file holds about 4×10⁸ imported characters across ten collections of about 1.2 GB each (DF OQ 5).
  - HISTORY_READINGS_CEILING is 500 readings.
- **Budgets, gated on the M1 MacBook Air 8 GB:**
  - 100 ms p95 for browsing;
  - 1 s to first rows, cold, with no lazy tail;
  - 1% dropped frames;
  - 2 s for a bulk set, clear or reorder;
  - 10 s for a delete (new, F99);
  - R8.11 against TRIGGER_ACK_WINDOW (100 ms) and the engineering plan's ROW_CONFIRM_BUDGET (F120).
- **Hot paths now:**
  - an All items keystroke over 4×10⁸ characters;
  - deletes at the ceiling under F102;
  - single edits during a session.
- **Measured this round.**
  - Setup: Apple M5 Max; SQLite 3.53.4 and the bundled copy in rusqlite 0.37; WAL, `synchronous=FULL`, `fullfsync` on.
  - Probes are in `/private/tmp/claude-501/-Users-vinnypasceri-Projects-spectro-capture/d4b2a2e5-4696-48c6-9516-d4cbad888739/scratchpad/perf/`: `bulk_delete_probe.py`, `r3probe/` (Rust in-memory scan and SQLite load, plus `fts_probe.py`) and `reader_ckpt_probe.py`.
  - The M1 Air was not measured. Figures marked "est." are extrapolations: about 1.8× slower CPU and about 2 GB/s SSD writes.

| Probe | Result |
|---|---|
| Delete a ceiling collection (10k items, 1.3 GB file), page `secure_delete` on, checkpoint included | 3,120 ms (3,108 ms commit + 12 ms TRUNCATE); 887 ms with it off. Est. 6–8 s on the M1 Air |
| Bulk clear of one 200-character column over 10k items | 129 ms |
| One-field edit | 7.8 ms commit + 4.1 ms checkpoint |
| All items in-memory scan: 407 MB of normalised text, 100k items | Single thread: 7.3–8.6 ms for rare-byte needles, 27.9 ms worst (a common-byte phrase). 8 threads: 0.9–4.0 ms |
| Reading the 461 MB item table from SQLite, warm | 169–188 ms, plus 184–186 ms for an ASCII lower-case pass (a floor for the import PRD's R2.3 normalisation) |
| FTS5 trigram index over the same 100k items | 1,930 MB file (index about 1.47 GB, 3.2× the text); 35 s to build. Query "ght pig" (13,206 hits): 57 ms. A 2-character contains on one of 22 columns falls back to a scan: 696 ms |
| Same index with page and FTS5 secure-delete on, as F102 requires | Delete 1,000 items: 39.0 s. Clear one column on 1,000 items: 39.4 s. One-field edit: 122 ms. With both off: 241 ms, 925 ms and 35 ms |
| TRUNCATE checkpoint after an edit, with one reader open | Returned busy after the 2 s busy timeout, and the old text was still in the main file. After the reader ended: 0.5 ms, and the text was gone |

## Findings

### Round-2 delta (my findings, as the round-2 fix file boxes them)

| Round-2 ID | Status | Evidence |
|---|---|---|
| PERF2-1 (Major) | RESOLVED | prd:279 adds DELETE_WRITE_BUDGET (10 s). prd:580 (R8.1g) classes both deletes and E10's undo. jny:477 (UJ9.5-g) times them at ROWS_CEILING. prd:585 (R8.3, F100) refuses them in flight. Re-measured at 3.12 s end to end, est. 6–8 s on the M1 Air, inside 10 s. The reverse race is left over: PERF3-2 |
| PERF2-2 (Major) | RESOLVED | jny:474 sends a keystroke, a header fire and a single edit at each set's last sample, with Scale searched, filtered and sorted, so every set overlaps a write. F120's interim gives the oracle a value (prd:752) |
| PERF2-3 (Minor) | RESOLVED | prd:330 lists R8.11 under Interim stated; prd:752 gives OQ 1's interim |
| PERF2-4 (Minor) | RESOLVED | prd:579 (R8.1f): no progress frame inside BROWSE_RESPONSE_BUDGET (F128). prd:599 (R8.10b) reads "whether a write's progress shows". jny:477 sends inputs every 100 ms during each write; jny:529. Nit: PERF3-9 |
| PERF2-5 (Minor) | PARTIAL | prd:584: R8.2's first rows follow R8.1d. jny:471–472 type the prefix at each first-rows frame. jny:472 declares UJ9.5-b at full width. But the probe text can't see a lazily loaded imported-value corpus: PERF3-3 |
| PERF2-6 (Minor) | RESOLVED | jny:108: Page Down every 100 ms, cold per F119, refresh rate and macOS version recorded, nearest-rank p95. prd:735. Page Down against F41's "scrolling": PERF3-4 |
| PERF2-7 (Minor) | RESOLVED | prd:575 adds toggling one row. prd:574 and prd:578 add the grid. jny:471 adds toggles after "Select all" and the grid workload and paging. jny:477 times "Use as scan order" |
| PERF2-8 (Minor) | RESOLVED | prd:568: a file of up to FILE_ITEMS_CEILING items. jny:471: UJ9.5-a runs in UJ9.5-b's file. jny:476: UJ9.5-f at twice the ceiling. fen:1144. M1 hasn't followed: PERF3-5 |
| PERF2-9 (Nit) | RESOLVED | prd:576 (R8.1c's later showing), prd:584 (firing Find similar to E9), prd:735 (R2.5 in M1's start event). The M1 addition sharpened PERF3-5 |
| carried PERF-2 | RESOLVED | prd:580 |
| carried PERF-10 | RESOLVED | jny:474; prd:752 |
| carried PERF-11 | RESOLVED | prd:574, prd:578; jny:471 |

### New findings

**[MAJOR] PERF3-1 — ADR-0003 is told file-wide search may use "an index or a scan, either one". Under F102, the stock index misses the ceiling writes by about 200× and can't answer 1–2 character searches**
- **Where:**
  - adr:22 (ADR-0003's seventh input) and prd:629 (the Outbound Data Foundation row);
  - prd:584 (R8.2), prd:579–580 (R8.1f, R8.1g);
  - jny:108, jny:472, jny:477; fen:986 (F103).
- **Cost of the index branch:**
  - With the erasure F102 requires, deleting or clearing 1,000 items took about 39 s. Extrapolated linearly to UJ9.5-g's 10k items, that is about 6.5 minutes per write on the M5 Max, against 10 s and 2 s.
  - Any stored text index must shed a removed value's fragments the moment the write lands, so its cost grows with the text removed.
  - Re-indexing a whole item on one edit took 122 ms (est. about 250 ms on the M1 Air). That weakens F100's premise that in-flight single edits are "~10 ms" (fen:970), and the edit holds the writer that long (R8.11).
- **Search limits of the index:**
  - A trigram index can't answer a 1–2 character contains search. Every keystroke of the workload's imported-value text is timed (jny:108), and R3.1 requires those searches.
  - The scan fallback took 696 ms on one of 22 columns.
  - A moderately common phrase took 57 ms on the M5 Max, est. about 115 ms on the M1 Air.
- **The scan branch fits:**
  - Worst measured was 27.9 ms single-threaded on 407 MB (est. 60 ms or less on the M1 Air; 10 ms or less on its four fast cores), with nothing to erase.
  - Its cost moves to memory: about 0.4 GB resident on the 8 GB Mac.
  - It also moves to the cold open. R8.2, through R8.1d, needs the whole corpus searchable at first rows within 1 s.
  - Reading the corpus took 169–188 ms warm, plus about 185 ms for the lower-case floor. With the full R2.3 normalisation on one thread, est. 0.9–2.4 s on the M1 Air.
  - So the build must store the normalised text or normalise in parallel.
- **Scenario:** ADR-0003 reads the input as written and picks SQLite's full-text index, the usual choice. UJ9.5-g's clear and deletes then miss by two orders of magnitude, and nobody finds out until the P1 timing run.
- **Fix, without reopening F102 or F103, outside the body:**
  - Change adr:22 from "…uses an index or a scan, either one under that byte rule" to "…either one under that byte rule and within R8.1f's, R8.1g's and R8.2's budgets on OQ 1's Mac, one- and two-character searches and the cold first rows included".
  - Cite this probe's figures as input evidence for ADR-0003.
  - prd:629 can stay a pointer, so the body doesn't change.
- **Why not a Blocker:** no row is wrong, and a design that meets every row exists. The defect is an input that invites a design that doesn't.

**[MAJOR] PERF3-2 — F100 only refuses one way: nothing stops a session starting while a bulk write, now up to 10 s, holds the only writer**
- **Where:**
  - prd:585 (R8.3), prd:607 (R8.11), prd:580 (R8.1g), prd:279;
  - capfen:644 (Capture F72: "so no bulk write holds the file's writer while this PRD's saves need it");
  - capture:155 (R3.1 durably records a session), capture:164 (R3.10, the first capture action), capture:159 (R3.5 refuses a start only when another session holds the instrument).
- **How it arises:**
  - R8.1f and R8.1g keep the surface responsive through a delete. For a ceiling collection that is est. 6–8 s on the M1 Air.
  - So the user can choose another collection and start capture.
  - The session's start record and its first set's save then wait for SQLite's writer. R8.11 forbids that delay, and F72 states the opposite of what happens.
- **The builder guesses between three options:**
  - refuse the start, a state no copy covers;
  - queue it, an unbudgeted wait;
  - let SQLITE_BUSY surface. The obvious mapping for that renders the Data Foundation PRD's E10, "Another copy of SpectroCapture has this file", which is false.
- **No case covers it.** UJ9.5-g declares no session in flight, and UJ9.5-d sends no bulk write.
- **Fix — needs owner:** this is the mirror F100 left open.
  - Recommended rule: "a session does not start or resume while one of R8.3's bulk writes is under way; its start waits for the write to land, its progress showing".
  - It would land as a capture PRD row, with a dated line under Capture F72, and in this PRD's Inbound line (prd:642). That adds 6 body words, paid for by PERF3-5's trim.
  - Add a UJ9.5-g run: during the collection delete, fire the capture PRD's start on another of UJ9.5-b's collections. Assert the first row confirmation lands within ROW_CONFIRM_BUDGET of its last sample.

**[MINOR] PERF3-3 — the first-rows probe types a prefix that codes alone answer, so a lazily loaded imported-value corpus passes**
- **Where:** jny:471, jny:472, jny:108; prd:577 (R8.1d), prd:584; prd:457 (R3.1).
- **The gap:**
  - R3.1 matches text as a prefix only against Swatch Code and Swatch Alternate Code. So "a prefix every item matches" is answered from the codes.
  - The imported-value corpus is never consulted. At FILE_ITEMS_CEILING it is 0.4 GB, the part most tempting to defer.
  - A build that shows first rows and loads imported values afterwards passes both UJ9.5-a and UJ9.5-b.
  - This wording came from my own round-2 fix.
- **Fix, in the journeys only:** at each first-rows frame, type the workload's imported-value text as well as the prefix. Assert that it lists exactly the items holding that text within BROWSE_RESPONSE_BUDGET.

**[MINOR] PERF3-4 — Page Down paging never exercises the scrolling F41 decided to budget**
- **Where:** jny:108 ("paging as Page Down pressed every 100 ms"), prd:578 (R8.1e), prd:277; fen:575 (F41: "scrolling (a dropped-frame statistic)"), fen:747 (F66: "paging").
- **The gap:**
  - Page Down jumps a screen at a time: one new screen every six frames at 60 Hz, with no continuous motion.
  - The stutter the research names happens during continuous trackpad or momentum scrolling, grid swatches included.
  - A build that renders each page in one frame but stutters on a continuous scroll stays under 1%.
- **Fix, in the journeys only, keeping the row's and F66's "paging":**
  - Add to the timing workload: "and again as a continuous scroll at one screenful per 100 ms".
  - Have UJ9.5-a do both, in the table and in the grid.

**[MINOR] PERF3-5 — M1's population disagrees with its own oracle and with R8.1's file**
- **Where:** prd:735; jny:471; prd:568.
- **The gap:**
  - M1 says "200 of each kind" on "one collection of ROWS_CEILING items".
  - UJ9.5-a, M1's worked oracle, sends only 20 each of:
    - gamut changes, which have been in M1's start event since the PERF2-9 fix;
    - Table/Grid switches;
    - swatch-size changes.
  - UJ9.5-a also runs in UJ9.5-b's 100k-item file, which R8.1 now binds.
  - So two people computing M1 get different populations.
- **Fix, saving 14 body words:** "Population: each kind as UJ9.5-a delivers it, in a Release build on OQ 1's interim Mac, warm."

**[MINOR] PERF3-6 — R8.1g budgets E10's undo of a ceiling delete, but nothing sizes what the undo holds while F102 keeps the bytes clean**
- **Where:** prd:580, prd:761 (OQ 10), prd:222–225 (the deleted-undoable and deleted states), prd:132 (a crash ends the window), jny:121 (T4); df:231 (R6.2b), df:340 (DF OQ 20).
- **The gap:**
  - A crash moves an item to "deleted", which is gone from the bytes even before reopening.
  - So pending content can't sit in the file's bytes. It has to be held in memory, about 1.2 GB raw per ceiling collection (DF OQ 5), or kept under a key held only in memory.
  - DF OQ 20 now allows two deletes in one window: about 2.4 GB on an 8 GB Mac.
  - The alternative, erasing when the window ends, puts an est. 6–8 s write on closing the file or quitting. No budget covers it.
- **Fix, outside the body, with a dated line under DF F53:** DF OQ 20's question adds "and what an undoable delete of ROWS_CEILING items holds in memory, or writes when the window ends, on OQ 1's Mac". R1.7 stays gated.

**[MINOR] PERF3-7 — under F102, a write that removes text can't land while any older read is open, and no budget covers how long a user's edit takes to land**
- **Where:** prd:590 (R8.8), prd:576 (R8.1c, "from when the write lands"), prd:607 (R8.11); fen:970 (F100's "each ~10 ms"); fen:982 (F102).
- **Measured:**
  - With one reader open, the TRUNCATE checkpoint an edit needs to clear the old text returned busy after the 2 s timeout. The old text was still in the main file.
  - Once the reader ended, the checkpoint took 0.5 ms and the text was gone.
  - SQLite's FULL, RESTART and TRUNCATE checkpoints also block new writers while they wait. A rollback journal doesn't avoid this: there a writer waits for every reader anyway.
- **Scenario:**
  - An export, or All items' cold load (up to 1 s), is reading while the user edits a name during a session.
  - The edit's landing holds the writer until the read ends, and capture's row save waits behind it (R8.11).
  - Outside a session, the edit shows as unsaved for as long as the read lasts.
  - R8.1c's clock starts when the write lands, so no case sees either delay.
- **Fix:**
  - (a) Outside the body: an ADR-0003 input that a removing write lands only after every earlier read has ended, so no read transaction may outlast BROWSE_RESPONSE_BUDGET. Long reads, exports included, run in short transactions.
  - (b) Needs owner: R8.1c times a user's own write from the input, and capture's save from when it lands. That adds 8 body words, paid for below.
  - (c) Add a UJ9.5-d run with All items cold-loading during the edits.
- **Severity:** this becomes Major if the export PRD lets an export run while a session is in flight. It says nothing either way.

**[NIT] PERF3-8 — HISTORY_READINGS_CEILING closes by a timing run, which shows 500 readings are met, not that 500 is enough**
- **Where:** prd:280, beside prd:282, where FILE_ITEMS_CEILING closes by the owner's estimate.
- **Fix:** "OQ 1 — the owner's estimate of the longest history, timed by UJ9.5-e". That adds 6 body words.

**[NIT] PERF3-9 — UJ9.5-g's collection-delete run doesn't say where its keystrokes and selections land while Scale is being deleted**
- **Where:** jny:477; prd:579 ("the surface").
- **Fix:** "during the collection delete, on another of UJ9.5-b's collections".

**Body word accounting.** PERF3-2 adds 6 words, PERF3-7(b) adds 8 and PERF3-8 adds 6, a total of 20. They are paid for by:
- PERF3-5, which saves 14 words;
- the §8 preamble's enumerated class list (prd:561–562, 18 words). It is an authoring checklist restating the format's guidance comment, with no rule for a builder.

Net: 12 words fewer.

## Biggest risks   (what degrades first as data/traffic grows)
1. **The storage choice for file-wide search under F102 (PERF3-1).** A stored text index makes every bulk clear or delete at the ceiling take minutes.
2. **F102's coupling between reads and writes (PERF3-7).** Every write that removes text waits for every earlier read, and capture's saves stall behind it.
3. **The cold All items open at full width on the 8 GB Mac.** The corpus is 0.4 GB, and normalising it on one thread is est. 0.9–2.4 s against a 1 s budget.
4. **A session started during a 6–8 s delete (PERF3-2).**
5. **Thin delete margins.** A ceiling delete is est. 6–8 s against 10 s on the M1 Air, and the undo may hold gigabytes in memory (PERF3-6).

## Genuinely efficient   (incl. where simple-and-fast-enough is right that a perf-zealot would wrongly flag)
- **All items search without an index.** An in-memory contains-scan searched 407 MB in 28 ms or less on one thread. So the PRD is right not to mandate an index, and the evidence argues against one.
- **DELETE_WRITE_BUDGET as its own constant.** Erasure at the page level took 3.12 s end to end (est. 6–8 s on the M1 Air), which fits 10 s. The 2 s budget still easily covers a clear (129 ms). A separate constant was the right call rather than stretching BULK_WRITE_BUDGET.
- **F100's refusal of bulk writes in flight.** It is simpler and more testable than writers that yield or queue under ADR-0005.
- **F128.** A write that finishes within 100 ms flashes no progress indicator.
- **UJ9.5-d** now overlaps a write with every set.
- **Measurement conditions.** F119's definition of "cold", nearest-rank p95 and the recorded refresh rate and macOS version make runs reproducible.
- **Other points that need nothing:**
  - HISTORY_READINGS_CEILING at 500 needs no paging.
  - Find similar at 100k (9.5 ms in round 1) needs no index.
  - UJ9.5-f's function-only check at twice the ceiling is the right level.
- **Sibling halves.**
  - The Data Foundation PRD's R2.3 and R6.2a wording, and Capture F72, cost nothing beyond PERF3-2 and PERF3-7.
  - Working out the Data Foundation PRD's R3.4 flag by OQ 7's interim test at sRGB is one matrix operation per value at derivation, which is negligible.

## Missing / over-engineered   (premature optimization)
- **Missing:**
  - the budgets in ADR-0003's search input (PERF3-1);
  - precedence when a session starts during a bulk write (PERF3-2);
  - an imported-value probe at the first-rows frame (PERF3-3);
  - a continuous scroll in the timing workload (PERF3-4);
  - a bound on read transactions under F102 (PERF3-7);
  - the memory an undoable delete holds (PERF3-6).
- **Not over-engineered:** a separate delete budget, the 500-reading ceiling, 200 samples per input kind, and the function-only check above the ceilings.
- **Caution:** don't answer PERF3-1 by having the PRD mandate either an index or a scan. The budgets decide it, and ADR-0003 owns the mechanism.

| Row ID | disposition |
|---|---|
| R1.1 | ALIGN |
| R1.2 | ABSTAIN |
| R1.3 | ABSTAIN |
| R1.4 | ABSTAIN |
| R1.5 | ABSTAIN |
| R1.6 | ABSTAIN |
| R1.7 | ABSTAIN |
| R1.8 | ABSTAIN |
| R1.9 | ALIGN |
| R1.10 | ALIGN |
| R2.1 | ALIGN |
| R2.2 | ABSTAIN |
| R2.3 | ALIGN |
| R2.4 | ALIGN |
| R2.4a | ALIGN |
| R2.4b | ABSTAIN |
| R2.4c | ABSTAIN |
| R2.4d | ABSTAIN |
| R2.4e | ABSTAIN |
| R2.4f | ABSTAIN |
| R2.4g | ABSTAIN |
| R2.4h | ABSTAIN |
| R2.4i | ABSTAIN |
| R2.5 | ALIGN |
| R2.6 | ALIGN |
| R2.7 | ABSTAIN |
| R2.8 | ABSTAIN |
| R2.9 | ALIGN |
| R2.10 | ABSTAIN |
| R2.11 | ALIGN |
| R3.1 | ALIGN |
| R3.2 | ALIGN |
| R3.3 | ALIGN |
| R3.4 | ALIGN |
| R3.5 | ABSTAIN |
| R3.6 | ALIGN |
| R3.7 | ALIGN |
| R3.8 | ABSTAIN |
| R3.9 | ALIGN |
| R4.1 | ALIGN |
| R4.2 | ABSTAIN |
| R4.2a | ABSTAIN |
| R4.2b | ABSTAIN |
| R4.2c | ABSTAIN |
| R4.2d | ABSTAIN |
| R4.2e | ABSTAIN |
| R4.2f | ABSTAIN |
| R4.2g | ABSTAIN |
| R4.2h | ABSTAIN |
| R4.3 | ALIGN |
| R4.4 | ABSTAIN |
| R4.5 | ABSTAIN |
| R4.6 | ABSTAIN |
| R4.7 | ALIGN |
| R4.8 | ABSTAIN |
| R4.9 | ABSTAIN |
| R5.1 | ALIGN |
| R5.2 | ABSTAIN |
| R5.2a | ABSTAIN |
| R5.2b | ABSTAIN |
| R5.2c | ABSTAIN |
| R5.2d | ABSTAIN |
| R5.2e | ABSTAIN |
| R5.3 | ABSTAIN |
| R5.4 | ALIGN |
| R5.5 | ABSTAIN |
| R5.6 | ABSTAIN |
| R5.7 | ALIGN |
| R5.8 | ABSTAIN |
| R6.1 | ALIGN |
| R6.2 | ALIGN |
| R6.3 | ALIGN |
| R6.4 | ALIGN |
| R7.1 | ALIGN |
| R7.2 | ALIGN |
| R8.1 | OBJECT (PERF3-3, PERF3-4, PERF3-6, PERF3-7) |
| R8.1a | ALIGN |
| R8.1b | ALIGN |
| R8.1c | OBJECT (PERF3-7) |
| R8.1d | OBJECT (PERF3-3) |
| R8.1e | OBJECT (PERF3-4) |
| R8.1f | ALIGN |
| R8.1g | OBJECT (PERF3-2, PERF3-6) |
| R8.2 | OBJECT (PERF3-1, PERF3-3) |
| R8.3 | OBJECT (PERF3-2) |
| R8.4 | ALIGN |
| R8.5 | ALIGN |
| R8.6 | ALIGN |
| R8.7 | ABSTAIN |
| R8.8 | OBJECT (PERF3-7) |
| R8.9 | ABSTAIN |
| R8.10 | ALIGN |
| R8.10a | ALIGN |
| R8.10b | ALIGN |
| R8.10c | ALIGN |
| R8.10d | ALIGN |
| R8.10e | ABSTAIN |
| R8.10f | ALIGN |
| R8.11 | OBJECT (PERF3-2, PERF3-7) |
| E1 | ABSTAIN |
| E2 | ABSTAIN |
| E3 | ABSTAIN |
| E4 | ABSTAIN |
| E5 | ABSTAIN |
| E6 | ABSTAIN |
| E7 | ABSTAIN |
| E8 | ABSTAIN |
| E9 | ABSTAIN |
| E10 | ABSTAIN |
| E11 | ABSTAIN |
| E12 | ABSTAIN |
| E13 | ABSTAIN |
| E14 | ABSTAIN |
| E15 | ABSTAIN |
| E16 | ABSTAIN |
| E17 | ABSTAIN |
| E18 | ABSTAIN |
| E19 | ABSTAIN |
| M1 | OBJECT (PERF3-5) |
| M2 | ABSTAIN |
| M3 | ABSTAIN |
| M4 | ABSTAIN |

### Round 3 — verify-the-reviewer dispositions (Blocker/Major; Critical→Blocker, High→Major)

Every Major was checked against commit `c4ec992`. Privacy's one Medium (PRIV3-2: nothing places a search index's copy of item content inside the file) is advisory and carried into the round-3 fix list.

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| 1 | R4.7 exempts only "a capture save or a session starting" from ending the undo history, while E8 promises "scanning doesn't count" and E8 omits import and column hide/show | product-manager PM3-1; architecture ARCH3-2; staff-software-engineer 3MN4; product-marketing-manager N3-m3; interface R3-IF-5 | reproduced — R4.7 vs copy E8 | accept — owner decision |
| 2 | The Data Foundation PRD's new E34 sends the user to Finder's Get Info, which fixes neither a privacy-setting nor a sandbox loss; ADR-0007 is open | product-marketing-manager N3-M1; architecture ARCH3-6; product-manager PM3-8 | reproduced — DF copy E34 | accept — owner decision on wording |
| 3 | E10's "Undo" of a selection or collection delete (a DELETE_WRITE_BUDGET write) and "Undo change" of a bulk set or clear are not refused while a session on another collection is in flight | architecture ARCH3-1; test R3-M1; product-manager PM3-4; interface R3-IF-4; staff-software-engineer 3MN1 | reproduced — R8.3 lists E10's "Undo" under "on that collection" | accept — owner decision (extends F100) |
| 4 | R8.1's lead row applies HISTORY_READINGS_CEILING to every budget; F118 scoped it to opening the history view | staff-software-engineer 3MJ1 | reproduced — R8.1 lead row vs F118 and the constants table | accept — fix to the fence, no new decision |
| 5 | Nothing stops a session starting or resuming while a bulk write or delete (up to 10 s) holds the single writer — F100's reverse ordering | staff-software-engineer 3MJ2; performance PERF3-2 | reproduced — R8.3 checks refusal only when the bulk action fires; UJ9.5-g runs with no session | accept — owner decision |
| 6 | "The file's bytes equal those before" oracles fail correct SQLite builds (statistics tables, checkpoints, side files existing only while open) | test R3-M2 | reproduced by reasoning — UJ2.1-d, UJ9.4-b, the capture PRD's UJ3.3-l | accept — testability fix |
| 7 | UJ5.1-a asserts E17 with no variant, which a correct build fails once R5.5 lands and E17's "restore" variant renders | test R3-M3; interface R3-IF-1; staff-software-engineer 3MN7 | reproduced — UJ5.1-a vs UJ5.3-n; the Harness substitutes only E6 | accept — testability fix |
| 8 | ADR-0003's search input reads "an index or a scan, either one"; the stock full-text index measured ~39 s per 1,000 deleted items under F102 and cannot answer 1–2-character searches; the input omits the budgets that decide the choice | performance PERF3-1; architecture ARCH3-3; privacy PRIV3-2 (Medium: the index's location) | reproduced — docs/decisions/README.md ADR-0003 row; the timings are the reviewer's measurements | accept — hand-off fix under F103 and the Data Foundation PRD's R1.1 |

**Per-row dispositions after round 3.** Flip-eligible (every non-abstaining lens ALIGNs): E1, E2, E3, E4, E5, E7, E10, E11, E13, E14, E15, E16, E18, E19, M2, M3, M4, R1.1, R1.2, R1.5, R1.6, R1.7, R1.8, R1.9, R1.10, R2.1, R2.2, R2.3, R2.4, R2.4a, R2.4b, R2.4c, R2.4d, R2.4e, R2.4f, R2.4g, R2.4h, R2.4i, R2.6, R2.7, R2.9, R2.10, R3.1, R3.2, R3.4, R3.5, R3.6, R3.7, R3.8, R4.2a, R4.2b, R4.2d, R4.2e, R4.2f, R4.2g, R4.2h, R4.4, R4.6, R4.8, R4.9, R5.2, R5.2a, R5.2b, R5.2c, R5.2d, R5.2e, R5.5, R5.6, R5.7, R5.8, R6.1, R7.1, R7.2, R8.1a, R8.4, R8.5, R8.7, R8.10a, R8.10f. A lettered sub-row flips only with its lead and all its siblings.
