# Nix Toolkit import amendment — round 4 fixes

Resume point for round 4 (delta verification of 5538733; review log Round 4). One box per finding group, naming every round-4 finding and every earlier finding a lens left PARTIAL. Lens prefixes: PM-R4, SSE4, TR4, IF4, AR4, PRIV4, PMM4, PL4 (plan; its review numbers them R4-*), DB4. No new owner decision was needed; dated round-4 Clarified lines under Import F71, F73, F76, F82, F83 and F88, DF F64, Collection Mode F221, Export F35 and Capture F77 record the fixes.

## Majors

- [x] **R3.8k routes a changed match or outcome** — PL4-M1, DB4-M1, AR4-M1, IF4-M1, SSE4-M1, TR4-M1, DB3-m2 partial, SSE3-m2 partial. R3.8k sends any changed match or R6.8 outcome, as well as a changed mode, count or list, to E13's collection variant, as F76's round-2 line always said; it reads the source before the write hold (AR4-n2); UJ 3's Demo Device case names TK-2's pending item.
- [x] **UJ2.1-r's filter** — PM-R4-M1, IF4-M2, SSE4-M2, TR4-M2, TR3-m9 partial. The filter lists ZX-022 and ZX-026, ZX-026 carries imported, the action opens ZX-026 and reads its history, and each E17 line is named for its item.

## Minors and Nits

- [x] **E13's collection variant keeps choices** — PM-R4-m1, PMM4-n1. R3.8f and R3.8k keep each still-offered row's E14 choice on the collection variant; E13's copy says so and reads "the swatches this file matches"; UJ 3 case.
- [x] **E5's variants told apart** — PMM4-m1, IF4-m2, SSE4-m4, TR4-m1, IF4-n3, IF4-m3, PM-R4-n2, PMM4-n2. R6.1 names the encoding and quotes variants, the encoding one first; the quotes variant reads "Row ⟨record⟩" and names the collection's name among the places a quote mark sits; ⟨record⟩ is registered in the copy note and Capture §12; UJ 3's quote case expects the quotes variant naming row 3; an E7 Toolkit case.
- [x] **E5's encoding cause** — PMM4-m2. "isn't saved as UTF-8 text — often because it was saved again from a spreadsheet".
- [x] **Not-checked reason** — PMM4-m3. E43 names a file that doesn't say which light and viewing angle.
- [x] **Recognition under a chosen encoding** — TR4-m2. UJ 3 case for a UTF-16LE export read after E5's encoding choice.
- [x] **Exclusion scope of R6.3 and R6.11** — TR4-m3. UJ 3 case whose excluded first record holds another collection name.
- [x] **The mid-preview add-a-swatch case** — DB4-n4, AR4-n5, IF4-m4, SSE4-m2, TR4-m4. It names the capture PRD's Add a swatch and runs once that PRD's R9.3 lands; the Demo Device cases carry R3.8k in P0.
- [x] **Quarantined live case wording** — DB4-n1, PL4-n2, IF4-n7, SSE4-m3. TK-1's only reading, current, is a live one declared quarantined under DF R7.2.
- [x] **Fixture T's tabulation and which reference governs** — PL4-m1, PL4-m2, AR4-m2, SSE4-m1, TR4-m5, IF4-m5, TR4-n4. R6.10 and UJ 3 use the CIE 15 tabulation the build's derivation uses, its E308 table and 400–700 nm handling named in the engineering plan; DF R7.5's reference governs once named, Fixture T regenerated to agree with it and never the build loosened; R7.5's Fixture T check lands with Import §6; DF M2's population names Fixture T's spectra; the obligation lines and F64's map carry R6.10 ↔ DF R7.5; sRGB and HEX cells may be any well-formed values.
- [x] **NULL for a blank reference** — DB4-m2, AR4-m1. DF R2.3j and the ADR-0003 row store a blank illuminant and observer absent (NULL); the ADR row says the model is absent where the file gives none (PRIV4-2).
- [x] **Salvage** — DB4-n2, DB4-n3, SSE4-m5, TR4-m6, TR4-n3, SSE4 nit. DF R5.5d replays by per-item sequence, never drops a reading, keeps a same reading behind, and gives the later reading correction-unconfirmed with the other as its predecessor; DJ6 covers seven pairings.
- [x] **R2.3j's readable A** — AR4-n3. "no readable current value, over a readable simulated or imported A".
- [x] **EJ1's counts and link** — IF4-m1, TR4-m7, TR4-n5, IF4-n8, PM-R4-n1, PMM4-n4, AR4-n1, DB4-n5, SSE4 nit, PRIV4-3. One collection from R7.7o, a no-model row with an empty model cell, 3 imported readings with the quarantined one also counted as quarantined, the quarantined history row as R1.1i empties it, and the link repaired.
- [x] **IJ:118's device assertion** — TR4-n2. "the import adds no saved-device record".
- [x] **Import's Device line cites R1.22** — IF4-n2.
- [x] **E46 backed by R6.4** — IF4-n4. R6.4 says the mixed-modes variant lists the colours under each mode.
- [x] **Round-3 row changes get Clarified lines** — IF4-n5. F82 (R6.8 routing), F83 (pre-fill) and F88 (unreadable set-aside).
- [x] **Collection Mode build row wording** — IF4-n6, AR4-n4, SSE4 nit. "adds to the first row's stops".
- [x] **Creation-form label mirrored in Capture** — IF4-n9, PM-R4-n5. Capture R1.10 and its Inventory import line; Import's Capture line cites R6.3.
- [x] **OQ 3's revise list** — PMM4-n3, PM-R4-n3, PL4-n3, PRIV3-2 partial. R6.1–R6.7, R6.11, E5, E43, E46, E49 and the Data Export PRD's E1, plus an encoding and byte-order-mark question.
- [x] **Post-lock dogfood bound** — PRIV4-1. The single-file clause.
- [x] **Journey labels** — PM-R4-n4, IF4-n1, PMM3-n4 partial, SSE4 nit. UJ 3's first Import case and the repeat case use "now a swatch's colour" and "kept in history".
- [x] **Plain-CSV re-match at commit** — SSE4 question 9. Post-lock item; it predates this amendment.

## Recorded without change

- [x] **ΔE2000 against ΔE*ab** — TR4 optional. R6.6 names ΔE2000; a case telling it from ΔE*ab is declined as optional.
- [x] **Engine pragmas** — DB4 risk. Unchanged since round 3: ADR-0003's own work.
