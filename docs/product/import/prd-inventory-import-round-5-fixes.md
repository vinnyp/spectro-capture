# Nix Toolkit import amendment — round 5 fixes

Resume point for round 5 (delta verification of cdaba46; review log Round 5). One box per finding group, naming every round-5 finding and every earlier finding a lens left PARTIAL or UNRESOLVED. Lens prefixes: PM-R5, SSE5, TR5, IF5, AR5, PRIV5, PMM5, PL5 (plan; its review numbers them R5-*), DB5. No new owner decision was needed. Dated round-5 Clarified lines under Import F71, F73 and F76, DF F36 and F64, Collection Mode F221, Export F35 and Capture F77 record the fixes.

## Major

- [x] **DJ3's salvage case follows R5.5d** — TR5-M1, AR5-m2, IF5-m2. DJ3's two-current case now says the pair is replayed in per-item sequence: the later reading stays current with reason correction-unconfirmed, the other is its predecessor and is shown unsettled, a pair whose later reading is imported ends as DJ6 states, and no reading is dropped. F36 gains a Clarified line. F64's round-5 line names DJ3. Post-lock's judgment call reads "the later reading in per-item sequence stays current, except where R2.3j keeps an imported reading behind".

## Minors and Nits

- [x] **R3.8k's scope and recheck list** — PM-R5-m1, SSE5-m1, IF5-m1, PL5 R5-m1, AR5-n3, PMM5-n1, SSE5 question 1. The collection-variant route applies only to a Toolkit export.
  - The recheck list names what the route compares: the mode fit, every eligible record's match and R6.8 outcome, and E43's Toolkit lines and Swatch details counts.
  - The trigger no longer covers the Existing items, absent and Added columns lines, so E13's collection body stays true as written (PMM5-n1's rewrite is not needed).
  - A new UJ 3 case deletes an unmatched swatch mid-preview and expects no E13.
- [x] **A P0 case for the match recheck** — DB5-m2, DB4-n4 partial. A new UJ 3 case deletes a matched swatch (TK-3) mid-preview with Collection Mode R4.5's Delete swatch. It expects E13's collection variant, a fresh E43 counting TK-3 as new, one TK-3 captured after Import, and every stored reading's item present.
- [x] **E14 choices on the other re-preview paths** — PM-R5-m2, IF5-n7.
  - R3.8i keeps each still-offered row's E14 choice. R3.8l keeps it too, unless the source changed.
  - R3.8f gives a newly offered row E14's default.
  - E13's collection variant reads "keeping any choices you made".
- [x] **ADR-0003's uniqueness exemption** — AR5-m1, DB5-m1, SSE5-m2, TR5-m1, DB4-n2 partial, SSE5 question 2. The ADR row also exempts "a same reading DF R5.5d's salvage keeps behind". How the schema tells that reading apart (DB5-m1's marker) is left to ADR-0003. So is AR5-m1's option of dropping the backstop altogether.
- [x] **R2.3j's reason column** — AR5-n1, DB5-n2, SSE5 nit, AR4-n3 partial. It now reads "B = re-measurement when current over a readable A, else initial". That matches Import R6.8b (a quarantined current gives initial) and R6.8d (an earlier imported reading kept behind gives initial).
- [x] **R5.5d and restores** — DB5-n3, TR5-n5. R2.3j's branch applies only where the later reading "is imported and not a restore". A restore of an imported reading takes the general branch: it stays current with reason correction-unconfirmed, and the other reading is its predecessor.
- [x] **DF's other R5.5d mirrors** — DB5-n1, AR5-m2. DF's closing salvage line names "equal record times or a repeated per-item sequence" as ADR-0003's.
- [x] **DF M2's Fixture T population** — AR5-n2, PL5 R5-n1. It is included "once Import §6 lands".
- [x] **R7.7o's consumer cell** — PL5 R4-n5 (unresolved from round 4). The cell no longer names Collection Mode R2.4j, whose cases declare their imported readings directly (Import R6.10).
- [x] **DF's inbound Import line** — IF4-m5 partial. It names Import R6.10: Fixture T joins R7.5, and R7.7o comes from Import's importer.
- [x] **R6.10's tabulation and regeneration** — PL5 R5-n2, TR5-n6. The row names the ASTM E308 weighting table, as UJ 3 and F71 do. Fixture T is regenerated "by an independent implementation of its method, never from the build".
- [x] **UJ 3's sRGB and HEX sentence** — IF5-n3, SSE5 nit. It is now a sentence of its own: "Its sRGB and HEX, which R6.2 ignores, may be any well-formed values."
- [x] **E5's encoding variant states the rule** — PMM5-m1. It reads "SpectroCapture reads a Nix Toolkit export only as UTF-8 text, and this one isn't — often because it was saved again from a spreadsheet". That holds both directly and after a manual encoding choice.
- [x] **E13's choices case** — PM-R5-n1, IF5-n6, TR5-n1, PL5 R5-n3, SSE5 nit, SSE5 question 3. The action reads "Import, then 'Review again', then Import", and the check follows the second Import.
- [x] **E7's wrong-width record** — PL5 R5-n4. The case also asserts "E47 does not list it".
- [x] **Collection Mode M2's population** — IF5-n1, PM-R5-n2, SSE5 nit, SSE5 question 4. M2 and the imported rows' Build-dependencies row name ZX-026 beside ZX-022.
- [x] **EJ1's wording** — IF5-n2, TR5-n4. EJ1 names the Nix Spectro 2 row and reads "each imported row's measured-at is its Date Saved". It lists R7.7o's item whose imported reading a re-scan superseded, and its count of 3 imported readings stands.
- [x] **OQ 3** — IF5-n4, SSE5 nit, PM-R4-n3 partial. The revise list adds the device PRD's R1.21, and the Feeds column matches the revise list.
- [x] **Capture's cite for the creation-form label** — IF5-n5. It is now "the import PRD's R6.3/R6.4/R6.8".
- [x] **The creation-form label has a case** — TR5-m2. UJ 3's create-a-target case asserts the scan mode "is labelled as set by this Nix Toolkit export".
- [x] **Append order** — TR5-m3. UJ 3's case with a partly held target asserts that TK-3 is "appended after `Peach` and `Deep Coral` in queue order".
- [x] **An excluded matched record** — TR5-m4. A new UJ 3 case starts from the state the first Import case leaves, with TK-3's `R550 nm` `abc` and its Color Name `Changed`. After "Continue without them" and Import, TK-3 is unchanged and E43 lists it as left out.
- [x] **The stray quote's position** — TR5-n2. The quotes case uses Color Name `Deep "Coral`, a quote mid-value.
- [x] **The encoding variant is checked first** — TR5-n3. A new UJ 3 case puts a Windows-1252 byte in TK-1 and a stray quote in TK-2, and expects the encoding variant.
- [x] **Plain-CSV recheck scope** — DB5-m3. The post-lock item now covers "a matched item added, deleted or edited between preview and commit". It rests on R3.3 and the preview's classification alone.
- [x] **Round-4 fix-file omissions** — PL5 (its note on R4-n1, R4-n4 and R4-n5). Round 4's plan findings R4-n1 and R4-n4 were fixed under other lenses' IDs: the EJ1 link and the journey labels. R4-n5 is fixed here.

## Recorded without change

- [x] **PRIV5** — no new finding at any severity. The real-value check found no real value in the tree, the commit message or the added lines.
- [x] **OQ 6's closer** — SSE5 optional nit. DF R7.5 carries "the named reference governs; the build is never loosened" normatively, so the closer does not repeat it.
