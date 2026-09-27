# Nix Toolkit import amendment — round 7 fixes

Resume point for round 7, the pre-lock re-check of 3ae25c1 by all nine lenses (review log, Round 7). One box per finding group, naming every round-7 finding. Lens prefixes: PM-R7, SSE7, TR7, IF7, AR7, PRIV7, PMM7, PL7 (plan; its review numbers them R7-*) and DB7. Seven lenses aligned on every row; two Minors held one row each. No new owner decision was needed. Dated round-7 Clarified lines under Import F67 and F76, DF F64 and Export F35 record the fixes.

## Minors

- [x] **R7.7o's re-scan is live** — SSE7-m1, AR7-n2, IF7-n3, PL7 R7-n5, SSE7 question 1. R7.7o now reads "one a live re-scan superseded", as EJ1 needs.
- [x] **The column-rename case isolates the added-columns check** — TR7-m1.
  - `Finish` holds a value only on ZZ-1, which the file lacks, so TK-1–TK-3 hold nothing in it.
  - The case expects E43's Swatch details counts unchanged, so only the added columns can raise E13.
  - The rename confirms E19's "Rename the column" (PL7 R7-n6).
- [x] **The choice-only case asserts the commit** — TR7-m2. It now expects "No E13: the commit proceeds". TK-1 keeps its name, with its imported reading kept in history, and TK-2 and TK-3 are added, captured.

## Nits

- [x] **R3.8k's wording** — DB7-n1, DB7-n2, IF7-n1, SSE7-n1, PL7 R7-n1, TR7-n3, SSE7 question 2.
  - The recheck names each record's matched item, and each R3.6 field change "with the stored value it replaces".
  - It compares against "the preview E43 and E14 last showed, its per-record plan and current choices included".
  - A Toolkit commit writes what that recheck worked out.
- [x] **E13's collection body** — PMM7-n1. "Its measurement condition, its columns or the swatches this file matches have changed since the preview. The app has worked it out again from the collection as it is now: each swatch keeps its choice between taking the new details and keeping what you have, and a swatch newly listed with different details starts at taking the new details. Check it before importing."
- [x] **UJ 2.1's kept-choice cases** — PM-R7-n1, IF7-n2, PL7 R7-n3, SSE7-n2, TR7-n1, SSE7 questions 3 and 4.
  - The E44 and E40 cases declare "a captured match whose name the file changes, set to 'Keep what I have'".
  - E40 is met at commit, and a confirmed ending is followed by Import.
  - The source-change case changes another record and reaches the fresh preview through "Review again".
- [x] **The Demo Device E13 case** — PL7 R7-n2. It now reads "Import, then 'Review again', then Import".
- [x] **The Note case** — PL7 R7-n6. TK-1's Note is declared, not set through Collection Mode, so no gate is needed.
- [x] **TK-3's setup in the rename case** — TR7-n2. It adds "and no imported columns".
- [x] **DJ6's restore sources** — AR7-n1, PL7 R7-n4. Each restore's source is held in TK-2's history, so "no reading dropped" covers it.
- [x] **ADR-0003's restore exemption** — AR7-n1. Restores "must stay told apart even where DF R5.5d's salvage re-records a restore's reason".
- [x] **DJ6's index row** — IF7-n4. It names R5.5d and R7.2. R2.3j, which R2.1–R2.5 already covers, is dropped, keeping Data Foundation at 8,900 words.
- [x] **EJ1 on the no-model row** — TR7-n4. "Each imported row has `sc_imported` true."
- [x] **Real-value wording in the review log** — PRIV7-1.
  - Round 6's verification note now names categories only.
  - The one location in privacy's round-5 section that pointed at a real value is redacted, and the redaction is marked there.

## Recorded without change

- [x] **Synthetic-pattern names** — PRIV7 optional hardening. The local check runs before every commit, and it caught round 6's coincidence. Later rounds favour invented names that are not plain colour words.
- [x] **A serial column found through OQ 3** — PRIV7 watch item. OQ 3's revise list already names the device PRD's R1.21. How a serial is held is decided when that revision is made.
