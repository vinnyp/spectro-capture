# Nix Toolkit import amendment — round 6 fixes

Resume point for round 6 (a re-check of d72b0b0 by eight lenses; review log Round 6). One box per finding group, naming every round-6 finding and every earlier finding a lens left PARTIAL. Lens prefixes: PM-R6, SSE6, TR6, IF6, AR6, PMM6, PL6 (plan; its review numbers them R6-*), DB6. Privacy did not run this round: it objected to no row in rounds 4 and 5, and round 5 changed nothing that is stored or sent. No new owner decision was needed. Dated round-6 Clarified lines under Import F67 and F76, DF F64 and Export F35 record the fixes.

## Minors and Nits

- [x] **Restores in salvage** — IF6-m1, DB6-m2, AR6-n1, PL6 R6-m3, SSE6-m3, TR6-m3, PM-R6-n1, SSE6 question 3.
  - DJ3 now reads "a pair whose later reading is imported, and not a restore, ends as DJ6 states", and DJ6 reads "Where the later is imported and not a restore".
  - DJ6 adds two pairings, so it now has nine: a live reading then a restore of an imported reading, and an imported reading then a restore of an imported reading measured earlier. In each, the restore stays current with reason correction-unconfirmed, and the other reading is its predecessor, shown unsettled.
- [x] **What R3.8k's Toolkit recheck compares** — DB6-m1, AR6-n2, SSE6-m2, IF6-m2 (Toolkit half), PL6 R6-m2(b), TR6-m1, SSE6 question 2.
  - The recheck now compares what the commit would write — every eligible record's match, R6.8 outcome and R3.6 field changes, and E43's Toolkit lines, Swatch details counts, added columns and rows gaining details — against what E43 and E14 show. It routes any difference to E13's collection variant.
  - A choice changed in E14 alone is shown, so it raises no E13.
  - Four new UJ 3 cases:
    - a choice change alone, with no E13;
    - a mid-preview rename that adds an Updated row, which also pins R3.8f's newly offered row taking E14's default and a per-row "Keep what I have";
    - a rename that leaves every count equal, with E13 anyway;
    - a column rename, with the added columns rechecked.
- [x] **E13's collection body** — PMM6-n1, with the recheck above. It reads: "Its measurement condition, its columns, or the swatches this file matches, changed. The app has worked out the preview again, keeping the choice you made for each swatch; a swatch newly listed with different details starts at taking the new details. Check it before importing."
- [x] **Kept choices on E40 and E44** — PM-R6-m1, IF6-m3, PL6 R6-m2(a), TR6-m2.
  - UJ 2.1's E44 case asserts that a match set to "Keep what I have" keeps that choice on the fresh preview and keeps its name after the retry.
  - A new case changes the source before "Try again" and expects E13 with the default restored.
  - UJ 2.1's E40 case asserts the kept choice on a confirmed ending.
  - F67 gains a Clarified line saying the rule applies to every import, and its map names R3.8l and E44.
- [x] **The encoding variant is checked first** — PL6 R6-m1, SSE6-m1, TR6-n1, TR5-n3 partial, SSE6 question 1. The case now puts the stray quote in TK-1 and the Windows-1252 byte in TK-2, so the encoding variant wins even though the quote comes first in the file.
- [x] **Second Import in the E13 cases** — IF6-n2, PL6 R6-n1, SSE6-n1, TR6-n2, SSE6 question 4. The Add a swatch case and the delete-TK-3 case read "Import, then 'Review again', then Import", with "nothing is written until Import is chosen again".
- [x] **Gates on Collection Mode actions** — PL6 R6-n2, SSE6-n2. Each new UJ 3 case that drives a Collection Mode action runs once that action lands: R4.5's Delete swatch, R4.3's item-detail edit or R4.8's Rename column. This follows the Add a swatch case's gate on the capture PRD's R9.3.
- [x] **EJ1's live rows and the re-scanned item** — TR6-m4, IF6-n1.
  - EJ1 declares the re-scan live and asserts both flags false on every row whose current reading is live.
  - In the ‹P1› history export, the reading the re-scan superseded exports `superseded` with `sc_imported` true, the re-scan's row carries a true current flag and `sc_imported` false, and E1 states 5 imported readings.
- [x] **The absent count for an excluded matched record** — TR6-n3. The case adds "and not among the rows not in this file".
- [x] **Plain-CSV recheck scope** — IF6-m2 (plain half), SSE6-m2 (plain half), SSE6 question 5. The post-lock item now also covers a stored column renamed mid-preview. It asks whether R3.8k's recheck of what the commit would write extends to every import, and what any commit does when its target collection was deleted mid-preview.
- [x] **Provenance maps** — IF6-n3.
  - Import's map gives F71 and F73 "as clarified five times", F76 "six times" (adding R3.8f, R3.8i and R3.8l), F82 "three times", and F83 and F88 "as clarified".
  - Data Foundation's F64 map adds DJ3.

## Recorded without change

- [x] **Stale Existing items and absent lines at commit** — SSE6 deferral to the product lens. These are left out of the recheck on purpose, because they do not change what the commit writes. The product lens aligned on it (PM-R6, false positives considered).
- [x] **Privacy** — not run. The reason is given above.
