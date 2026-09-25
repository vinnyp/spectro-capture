# Collection Mode PRD — round 0 fixes (post-fill owner adjudication, 2026-09-24)

The resume point for the fix pass that applies the owner's post-fill decisions (fences F5–F20 in
`prd-collection-mode-fences.md`) before review round 1. Each box is ticked as its fix lands. The
fence file is the written authorization for every change below: a change a fence does not name is
not authorized, and goes back to the orchestrator as an unratified WHAT rather than being made.

Sibling IDs are written with their document ("DF" = the Data Foundation PRD, "Capture" = the
Capture Mode PRD, "Import" = the Inventory Import PRD, "Export" = the Data Export PRD, "Device" =
the Device Management PRD).

## This PRD

- [x] **F7 — Delete selected is offered.** R6.3 opens DF's new selection-scale delete confirmation
      (cite its new ID) and no longer carries OQ 9's withholding interim. Update the Build
      dependencies "Bulk delete" row, the Outbound table line (now made, not proposed), the
      Interim-stated list, and the OQ table: OQ 9 answered by F7 — write its `## OQ 9` section in
      the OQ results file and set its status accordingly (reserved number stays). Cases for
      "Delete selected" (confirm, export first, cancel, session in flight) exist and assert values.
- [x] **F9 — Rename column is offered.** R4.8 no longer carries OQ 8's withholding interim; its
      inherited-obligation marker and the Outbound line point at the DF and Import rows as amended
      in this change (made, not proposed). OQ 8 answered by F9 — `## OQ 8` section written. Cases
      for rename: accepted, blank, duplicate, a later import of a source still carrying the old
      header adding it as a new column.
- [x] **F10 — Collection-side "Flag".** Add a P1 row (item detail): on a captured item, fires the
      capture PRD's "Flag" (the label is Capture's — cite it by document and state per the Labels
      rule, never re-quote it as this PRD's own), the item becoming set aside under Capture's rules
      with its reading kept as history; not offered on an item that is not captured; refused with
      E6 while a session on the collection is in flight (add it to R8.3's list and E6's owning
      rows). Add the route to Row transitions (naming the capture row it acts through), a T-case,
      and journey cases. Fill F10's Carried by and map line.
- [x] **F11 — Quarantined current reading ⇒ set aside, cause "unreadable".** Update the Vocabulary
      "current value" line, R2.4g (no-value) and R2.4h (unreadable) so the two marks and the row
      state agree with F11, R4.2b (the State line names the cause), R3.4's filter set if needed,
      and every fixture/count in the journeys that assumed "captured as recorded" (e.g. UJ2.1-j).
      Fill F11's Carried by and map line.
- [x] **R5.5 vs DF R5.5b (conflict found in the orchestrator's read).** DF R5.5b offers "re-scan or
      restore of a readable earlier reading" when an item's current reading is quarantined. R5.5
      must offer "Use this reading" on a readable earlier reading of such an item, and never on the
      quarantined reading itself. Whether restore is offered on an item set aside by a Flag is NOT
      settled by any fence — if DF's R2.3f does not settle it, leave it as drafted and report it as
      a fork; do not decide it. Update R5.5's cases.
- [x] **F20 — column visibility.** R2.10: the choice persists across launches and is never written
      to the user's file (it no longer says "kept with the collection in the file"). Update its case.
- [x] **F8 — build contract.** The non-goal "an app-owned notes field or a column added by hand
      (decided nowhere yet)" now cites F8 instead.
- [x] **F5, F6, F12–F19 — verify.** Read each fence against the rows it carries and fix any drift
      between the drafted row and the fence's decision (e.g. R1.9/R1.10 against F5; R2.4a/b and
      R2.5 against F6; R4.3/R4.7 against F13; R8.3 against F14; R5.1–R5.8 against F15). Report
      "no drift" per fence where none.
- [x] **Carried-by placeholders.** Replace every `_(…)_` in F7, F9, F10 and F11's Carried by lines
      and in the fence → row map with the actual IDs (this PRD's bare; siblings' with their
      document name first, per the Carried-by grammar).
- [x] **Testability pairing (process rule 3).** Every row changed above has its acceptance case and
      its verification-seam row (R8.10 / the test-controls map) updated in the same pass.
- [x] **Word count** of the PRD body by rule 14's method, reported at the end (budget 12,000).
      Result: 9,681 words.

## Sibling documents — both sides of every seam (process rule 12)

Each sibling edit lands in this same change, carries a new dated fence in that sibling's own fence
file (next free number there, in that file's existing style) whose Authority cites this PRD's fence
and the owner decision (D7, D9, D10 or D11, 2026-09-24), and — where it touches an aligned row —
keeps the row's alignment and says so inline, per process rule 6. Append the sibling's status-line
amendment clause in that document's existing style, marked peer review pending. Keep each sibling's
own ID families, format and voice; never renumber.

- [x] **DF — selection-scale delete confirmation (F7).** Add a copy state beside DF's E8 (item) and
      E14 (collection) for deleting a selection: states the selected item count and their current
      and earlier readings, offers export first and cancel as E8 does, delete never the default.
      Amend DF R6.2 (and its deletion-lifecycle sub-rows only where needed) to name it, add or
      extend the DF verifiability row that tests E8 (R7.6k per the fill's report — verify) to cover
      it, and update DF's obligations line to Collection Mode.
- [x] **DF R1.2 + Import R2.6 (and Import's obligations line) — renamed column's stored name (F9).**
      The stored name of an imported column changes on a Collection Mode rename; its position and
      values are kept; a later import matches the stored (new) name, a source header equal to the
      old name arriving as a new column under Import's existing rules. Check whether Export mirrors
      "first-seen resolved names" and, if so, add the mirror there too (with its own dated fence).
      Result: Export reads stored names (its R2.3, R2.4a–c), never "first-seen"; no Export change.
- [x] **Capture R9.9 + its obligations line to Collection Mode (F10).** Dated clarification that
      the collection-side "Flag" is carried by this PRD's new row; Capture's Flag semantics
      (R5.6's demotion to set aside, reading to history) are what that entry point fires.
- [x] **Capture — set-aside cause "unreadable" (F11).** Amend Capture's set-aside causes and the
      rows that enumerate them, its row-state vocabulary ("captured — a row with a canonical value"
      stays true; say where an item whose current reading is quarantined sits), its counts, and
      how a later session treats such an item (as it treats other set-aside items). Cite DF R5.5b.
      If this needs a Capture copy variant for the cause, add it in Capture's copy file.
- [x] **Capture — editorial mirrors the fill reported** (only where they change no meaning; any
      that change meaning go back to the orchestrator as unratified): Capture's Surfaces table
      noting its E1, E33 and E34 also render on the collection surface this PRD browses; Capture
      citing this PRD's "New collection" label (F19) where it names creating a collection; the
      scope of Capture R11.15g's list of entry points on the collection surface.
- [x] **Device — editorial mirror** noting that this PRD's R2.6 carries E22's action and that the
      All items view shows no banner (R1.10) — only if it changes no meaning.
      Result: the E22-action pointer landed (Device F32); the All-items no-banner note was declined
      as meaning-changing against Device R6.5 and goes back to the orchestrator.
- [ ] **Post-lock list** — do NOT tick `docs/product/post-lock.md:94` yet; it is ticked, attributed
      to this PR's number, when the PR exists.

## Out of scope for this pass

- `docs/product/README.md` (index status, §6 paragraph) — updated at the bookkeeping close.
- Deleting guidance comments — at lock.
- Any change a fence above does not authorize.

## Round 0b — second post-fill adjudication (fences F21–F28, 2026-09-24)

The same rules as above: the fence file authorizes, a change no fence names is reported rather than
made, and each box is ticked as its fix lands.

- [ ] **F21 — column visibility in the file.** R2.10: the choice is kept per collection, with the
      collection, in the user's file (the Data Foundation PRD's R1.1). Remove the app-preferences
      seam from R8.10, the Named defaults and the test-controls map if nothing else uses it; update
      UJ2.2-a/b to read the choice back from the file. Fill F21's Carried by and map line.
- [ ] **F22 — Capture R8.18.** Its status moves from ✋ Needs Discussion to the owner-authorized
      aligned value in Capture's vocabulary (🤝 Aligned), with the owner authorization noted inline
      per Capture's convention; its text states each F22 consequence (unsettled; neither counts
      toward nor breaks a N_CONSEC_HARD run; restore makes it captured again; excluded from M4's
      per-session deferred rate and does not reopen M3 once recorded — amend M3/M4's definitions or
      a note citing R8.18, whichever that PRD's shape uses). Capture F70 gets a dated clarification
      recording F22. Remove the "drafted, undecided clauses" wording wherever it now misstates.
- [ ] **F23 — restore on a flagged item.** R5.5 offers "Use this reading" on a readable earlier
      reading — the flagged one included — of an item set aside by a Flag, making it captured again;
      UJ5.3-e's assert flips accordingly, and add a case restoring the flagged reading itself. Amend
      the Data Foundation PRD's R2.9 to name the operator's Flag (the capture PRD's R5.6) as the
      other way a current value is removed; add a dated clarification to DF F50 (or a new DF fence,
      if F50's scope cannot carry it) citing this PRD's F23 / D14. If Capture's Vocabulary or R8.18
      transition lines say only a restore after damage makes a row captured again, extend them to
      the Flag case.
- [ ] **F24 — export first from E33.** The Data Foundation PRD's E33 "Export first" opens the
      whole-collection export (the Data Export PRD's collection scope). The Data Export PRD's list
      of delete confirmations offering export first gains E33, with a new dated fence in its fence
      file citing this PRD's F24. Update UJ6.3-e's assert to the collection scope.
- [ ] **F25 — Device note.** Add to the device PRD (and its F32, as a dated clarification) that the
      All items view shows no E22 banner, each item carrying its own simulated mark, citing this
      PRD's F5/F25.
- [ ] **F26 — "Answer re-scans" off the All items view.** R5.7 names E2 only; E13's copy loses the
      action; drop any case asserting it on E13 and keep one asserting its absence there.
- [ ] **F27 — R5.4.** Confirm its text says same illuminant, observer and measurement condition.
- [ ] **F28 — OQ statuses.** Confirm OQ 8 and OQ 9 carry aligned.
- [ ] **Editorial.** UJ1.3-b's bare "E14" names the Data Foundation PRD (its E14), per the
      cross-document cite rule.
- [ ] **Carried-by placeholders** for F21–F25 and their map lines filled with actual IDs.
- [ ] **Testability pairing** for every row changed in this section; word count reported.
