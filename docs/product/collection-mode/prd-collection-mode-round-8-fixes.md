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

- [x] **A1 — UJ10.1-d's file check becomes row equality.** Answers test R8-M1, architecture ARCH8-1 and plan PLAN8-1; covers staff-engineer 8MN1 and interface IF8-m2.
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
      Result: Landed in UJ10.1-d only, Rows unchanged. The S-choice clause is gone: S is "another size S the build offers". The Given records "the window's frame at a fresh launch" with the default size, holds "every window the case shows … at that frame, the reopened and the relaunched one included", and ends "the Harness's SQL read of the file taken"; the When closes the file, reads the app's own storage and takes that SQL read again. The Assert reads "The app's own storage holds S's number as text nowhere, compared with its baseline read taken before the When; the file holds the same rows, table for table, as the Harness's SQL read of it taken before the When, SQLite's own statistics tables aside", and both size clauses read "at the size recorded before the Given" (the window's is a "frame", so "size" has one referent). The third Assert line above is the reason the oracle is exact, not something a test observes, so it stays out of the cell, as in UJ9.4-b. Meaning check against F199 (default and range the build's choice, at or above 24 pt): the case needs two offered sizes, takes the default from the build, assumes no size's digits, and compares a width-dependent default at one frame; F71 ("last while the file is open and are written nowhere") and F53 (the storage read) hold. The Data Foundation PRD states no write at close for this When: no delete, no removed text, no pending wipe.

## B. Minors and Nits fixed now (companion only)

- [x] **B1 — UJ9.5-d's "All items" half.** Answers PLAN8-3 and staff-engineer 8MN3.
  - At each set's first Demo sample, choose the exported collection before starting its export.
  - The not-exercised clause reads "a run in which, in every set, "All items" showed its first rows before that set's last Demo sample, reports that half not exercised, never passed".
  - The Given declares Scale's samples per row as 3.
      Result: Landed in UJ9.5-d, Rows unchanged. The Given reads "Scale as UJ9.5-a declares it, with 3 samples per row"; the second run, "at each set's first Demo sample, choose the exported collection, another of UJ9.5-b's collections, and start its export through "Export collection" …", which in R1.9's phase also brings the run back from "All items"; the not-exercised clause is the text above verbatim. Meaning check against F89 and F120 (R8.11's case and ROW_CONFIRM_BUDGET) and F184 (the later-of deadline): unchanged.
- [x] **B2 — UJ4.7-d names its states.** "no correction question appears" becomes "neither the Data Foundation PRD's E11 nor its E26 renders" (IF8-n1). Check that both are the right sibling states for a correction question. If only E11 applies, name E11 alone and report it.
      Result: Both apply, but not with "renders", so this landed in a scoped form (Rows unchanged; reported as a fork). E11 is the correction question itself; E26, "⟨n⟩ re-scans still need an answer", is its unanswered set; the Data Foundation journeys word R2.9's recovery as "never raises E11 or enters E26" (DF journeys:29). "Neither … renders" would fail a correct build here: the state UJ4.7-a left keeps ZX-013's T3 correction-unconfirmed (UJ4.7-b), the capture PRD's R8.3 look-through is a one-row session, and the Data Foundation PRD's R2.8 offers the unanswered set whole after a session (E26 at ⟨n⟩ 1) while its R2.4 asks once outside the loop (an E11 naming ZX-013). The Assert now reads "the new one current with reason initial, neither raising the Data Foundation PRD's E11 nor entering its E26 (its R2.9)": both states named, in that PRD's own words, and ZX-013 cannot trip it.
- [x] **B3 — B8's cite.** Answers IF8-n2 and staff-engineer 8N2. UJ1.3-c, UJ1.3-e, UJ1.3-f and UJ6.3-b close their storage clause with "(the Data Foundation PRD's R1.1)", as UJ9.7-i does. R8.6 stays in their Rows.
      Result: Landed in all four: each "…bytes hold the texts … nowhere" clause, which the Harness's byte check also runs over the app's own storage, now closes with "(the Data Foundation PRD's R1.1)"; Rows unchanged. Meaning check against that PRD's R1.1 ("Nothing about a collection or session is kept elsewhere"): holds.
- [x] **B4 — the Harness's baseline sentence.** It reads "…in the same file, or, in a file read as its raw contents, where…", so the two conditions are alternatives (R8-n2).
      Result: Landed verbatim. One exemption per read kind; a new write still raises a key path or a count, so it counts (R8.6, F53).
- [x] **B5 — the Test-controls map's app-storage line.** Add "or, in a file read raw, by count" beside "a baseline at the same key path". Answers R8-n1, IF8-m1 and PLAN8-6.
      Result: Landed as "a number against a baseline read before the When, at the same key path or, in a file read raw, by count", "read before the When" moved ahead so it governs both; it mirrors the Harness and adds no rule.
- [x] **B7 — the Harness's first-phase sibling list** names the Data Export PRD's R1.1 beside its E1, since UJ9.5-d's second run is first-phase and reads exports' rows (ARCH8-N1).
      Result: Landed as "the Data Export PRD's R1.1 and its E1"; Export R1.1 is P0 in that PRD. Index only.
- [x] **B6 — DF DJ3's re-read line.** "as its UJ9.5-b declares it" becomes "the file the Collection Mode PRD's UJ9.5-b declares, nothing else of its Given", so it inherits no R1.9, R3.7 or R8.2 gating (8N1). DF journeys only.
      Result: Landed in the Data Foundation journeys only: "…FILE_ITEMS_CEILING items, the file the Collection Mode PRD's UJ9.5-b declares, nothing else of its Given, the OS file cache purged…", the link on the new text, its target unchanged. Meaning check against F191 and that PRD's R1.11 (P0): the line takes UJ9.5-b's file alone.

## C. Post-lock items

- [x] **C1.** The ADR-0003 two-mechanism wipe item gains the prefix "Needs owner (qualifies F200):" (PLAN8-4).
      Result: Landed as "needs owner (qualifies F200): the wipe is two mechanisms…", lower case as the list's other needs-owner item writes it. No fence edited. ARCH8-4 (the log's "reset" should say truncation) is not in this list and was not applied.
- [x] **C2 — § Dogfood, OQ 6.** The owner's 24 pt check at the P2 build needs MIN_SWATCH_SIZE among the offered sizes. The P2 build offers it for that check, or the owner judges at the smallest size offered (PLAN8-5).
      Result: Appended to the existing OQ 6 item: "The owner's 24 pt check needs MIN_SWATCH_SIZE among the sizes the build offers: the P2 build offers it for that check, or the owner judges at the smallest size it offers (Collection Mode F199, its round-8 plan review's PLAN8-5; …)". Both options stay open, as F199's "You can tune them at the P2 build" leaves them.
- [x] **C3 — next Data Foundation pass.** DF's inbound Collection Mode line lists R7.6b, which F191 changed (IF7-N4, carried).
      Result: Added under Next pass → Data Foundation: the inbound line's Rows cell names R7.6b beside R1.11, one body word, putting that PRD at 8,400, its cap (F177).
- [x] **C4 — the next Collection Mode pass, or the P2 build.** Three test gaps:
  - a per-collection swatch-size case: Gouache Set's grid does not show S (R8-m3, 8N3);
  - UJ10.1-b measures each swatch's on-screen frame, not the reported size (R8-m2);
  - raw-read storage files the When grows are told apart from a real write by a rerun with another size S′ (R8-m1).
      Result: Added as three items under Next pass → Collection Mode, each "at the next Collection Mode pass or the P2 build", citing R8-m3 and 8N3; R8-m2 and IF8-n4; and R8-m1, ARCH8-3 and 8MN2.

## Checks

- [x] **Word counts.** Collection Mode, Data Foundation and Export are unchanged.
      Result: Collection Mode 11,986, Data Foundation 8,399 and Export 3,803, each equal to HEAD by check 12's method; the three bodies, the fences and the copy are byte-identical to HEAD.
- [x] **Mechanical scripts, read-only.** Re-run checks 1, 2, 6, 11, 12, 13, 17 and 18, and report the verdicts.
      Result: Each run against a pre-pass baseline. 2 and 6 PASS. 11a and 11b print the same MISS lines as before, none new (11a reads the Companions comment's backticked suffixes as paths, the four real companions resolving; 11b's five sit in the round-6 fix file). 12: 11,986 of 12,000, BUDGET PASS, the rule-migration scan unchanged. 13: no quoted string lacks a copy state. 17: one hit, inside "correction", down from two, no banned adjective. 18: the same six (1b) lines, 0 unsourced values. 1 (an enumeration): post-lock's lines naming Collection Mode with a row cite go from 13 to 16, this pass's items.
- [x] **Statuses unchanged by this pass.** R7.1, R7.2 and R8.6 flip after the test, architecture and plan lenses re-check A1.
      Result: R7.1 needs-discussion, R7.2 pre-alignment, R8.6 needs-discussion, R8.8 pre-alignment, as at HEAD. Round 8 has every lens ALIGN on R8.8, so its flip is due at the same bookkeeping.
