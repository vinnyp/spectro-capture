# Collection Mode PRD — round 8 fixes (targeted re-check, 2026-09-25)

This is the resume point for the last fix pass before lock. Review round 8 (subject commit `d248158`) was a targeted re-check by the six lenses that objected in round 7.

- **Scope:** no box changes a requirement row's rule or needs the owner.
- **Authority:** the fence file (F1–F201) authorizes every change.
- **Kinds of change:** each box is either:
  - **testability**: a case, harness or map edit that makes an oracle match its row and fence, at 0 body words; or
  - **post-lock**: an item added to `docs/product/post-lock.md` in that file's style.
- **Ticking:** tick each box with a one-line Result note.
- **Budget:** the Collection Mode body stays at 11,986, and the Data Foundation body at 8,399.

The findings are the six round-8 reviews in the review log, section "## Round 8".

## A. The one Major (companion only)

- [ ] **A1 — UJ10.1-d's file check becomes row equality.** Answers test R8-M1, architecture ARCH8-1 and plan PLAN8-1; covers staff-engineer 8MN1 and interface IF8-m2.
  - **Why the round-7 rule fails.** "Choosing an S whose number no seeded value's text holds" removed the only exemption:
    - the file's substring reads are now live over undeclared REALs and bytes, so a correct build fails;
    - a build offering, say, {24, 32, 48} pt has no legal S at all.
  - **Why not a byte count.** A count against the file's bytes also fails, because a checkpoint at close moves pages from the log into the main file (ARCH8-1).
  - **When:**
    - Drop ", choosing an S whose number no seeded value's text holds"; S is any size the build offers other than the recorded one.
    - Take the Harness's SQL read of the file (a copy, opened read-only, at SQLITE_READER_FLOOR) before the When.
    - After the close, read the app's own storage and read the file the same way.
  - **Assert:**
    - The app's own storage holds S's number as text nowhere (its number baseline applies).
    - The file holds the same rows, table for table, as the read taken before the When, SQLite's own statistics tables aside, in UJ9.4-b's form.
    - Setting a swatch size and closing the file writes nothing, so any changed row is the build's.
  - **Also in this box:**
    - Both "at the recorded size" clauses read "at the size recorded before the Given" (product-manager Minor).
    - The Given pins the window's size (PLAN8-2, ARCH8-2), so a width-dependent default cannot drift across the reopen or the relaunch.
  - **Rows:** unchanged.

## B. Minors and Nits fixed now (companion only)

- [ ] **B1 — UJ9.5-d's "All items" half.** Answers PLAN8-3 and staff-engineer 8MN3.
  - At each set's first Demo sample, choose the exported collection before starting its export.
  - The not-exercised clause reads "a run in which, in every set, "All items" showed its first rows before that set's last Demo sample, reports that half not exercised, never passed".
  - The Given declares Scale's samples per row as 3.
- [ ] **B2 — UJ4.7-d names its states.** "no correction question appears" becomes "neither the Data Foundation PRD's E11 nor its E26 renders" (IF8-n1). Check that both are the right sibling states for a correction question. If only E11 applies, name E11 alone and report it.
- [ ] **B3 — B8's cite.** Answers IF8-n2 and staff-engineer 8N2. UJ1.3-c, UJ1.3-e, UJ1.3-f and UJ6.3-b close their storage clause with "(the Data Foundation PRD's R1.1)", as UJ9.7-i does. R8.6 stays in their Rows.
- [ ] **B4 — the Harness's baseline sentence.** It reads "…in the same file, or, in a file read as its raw contents, where…", so the two conditions are alternatives (R8-n2).
- [ ] **B5 — the Test-controls map's app-storage line.** Add "or, in a file read raw, by count" beside "a baseline at the same key path". Answers R8-n1, IF8-m1 and PLAN8-6.
- [ ] **B7 — the Harness's first-phase sibling list** names the Data Export PRD's R1.1 beside its E1, since UJ9.5-d's second run is first-phase and reads exports' rows (ARCH8-N1).
- [ ] **B6 — DF DJ3's re-read line.** "as its UJ9.5-b declares it" becomes "the file the Collection Mode PRD's UJ9.5-b declares, nothing else of its Given", so it inherits no R1.9, R3.7 or R8.2 gating (8N1). DF journeys only.

## C. Post-lock items

- [ ] **C1.** The ADR-0003 two-mechanism wipe item gains the prefix "Needs owner (qualifies F200):" (PLAN8-4).
- [ ] **C2 — § Dogfood, OQ 6.** The owner's 24 pt check at the P2 build needs MIN_SWATCH_SIZE among the offered sizes. The P2 build offers it for that check, or the owner judges at the smallest size offered (PLAN8-5).
- [ ] **C3 — next Data Foundation pass.** DF's inbound Collection Mode line lists R7.6b, which F191 changed (IF7-N4, carried).
- [ ] **C4 — the next Collection Mode pass, or the P2 build.** Three test gaps:
  - a per-collection swatch-size case: Gouache Set's grid does not show S (R8-m3, 8N3);
  - UJ10.1-b measures each swatch's on-screen frame, not the reported size (R8-m2);
  - raw-read storage files the When grows are told apart from a real write by a rerun with another size S′ (R8-m1).

## Checks

- [ ] **Word counts.** Collection Mode, Data Foundation and Export are unchanged.
- [ ] **Mechanical scripts, read-only.** Re-run checks 1, 2, 6, 11, 12, 13, 17 and 18, and report the verdicts.
- [ ] **Statuses unchanged by this pass.** R7.1, R7.2 and R8.6 flip after the test, architecture and plan lenses re-check A1.
