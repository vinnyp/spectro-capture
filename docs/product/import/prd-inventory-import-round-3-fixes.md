# Nix Toolkit import amendment — round 3 fixes

Resume point for round 3 (delta verification of 7332d69; review log Round 3). One box per finding group, naming every round-3 finding and every earlier finding a lens left PARTIAL. Lens prefixes: PM-R3 (product manager), SSE3 (staff engineer), TR3 (test), IF3 (interface; its IF2-m1 partial too), AR3 (architecture), PRIV3 (privacy), PMM3 (product marketing), PL3 (plan; its review numbers them R3-*), DB3 (database). No new owner decision was needed; the dated round-3 Clarified lines under Import F71, F73 and F76, DF F64, Collection Mode F221, Export F35 and Device F33 record the fixes.

## Majors

- [x] **A Toolkit export not in UTF-8** — PM-R3-M1, IF2-m1 partial, AR3-m4, PL3 round-2 m4 partial, SSE3-m1, TR3-m2. R6.1 recognises the first record under whatever encoding reads it; a Toolkit export not decoded as UTF-8 throughout reaches E5's new Toolkit encoding variant, which offers no read control. UJ 3 case with a Windows-1252 byte. The round-2 fix file's contrary claim is corrected in place.
- [x] **E5's quote remedy loops** — PMM3-M1, PM-R3-m2. E5's Toolkit quotes variant names the record and asks for the quote mark to be removed in the Toolkit before exporting the collection again.
- [x] **E11 before the file-level checks** — IF3-M1, PM-R3-m1, AR3-m3, PL3-m2, SSE3-m3, TR3-m1, DB3-n4. R6.4: at read, E7, E9, E10 and R6.7's exclusions run first; E11 applies once a target is chosen and never reopens R6.4's or R6.11's check.
- [x] **Build order for importer-made fixtures** — PL3-M1, PL3 round-1 G1 partial, SSE3-m6, PL3-m5, TR3-m9, IF3-n4, TR3-n3, PL3-n2. Post-lock's build-order item: R7.7o, DJ6 and Export's goldens after Import §6; DJ6's preamble says it is R7.7o's journey and runs in §6's phase; R6.10 names DJ6; UJ 3's Collection Mode clauses wait for those rows; Device UJ3-c declares its reading directly.
- [x] **Fixture T's own values** — PL3-M2, TR3-M2, SSE3-M1, PL3 round-2 M3 partial, TR2-m1 partial. R6.10 and UJ 3 name an independent implementation of CIE 15's calculation with ASTM E308's 10 nm tables, cross-checked by DF R7.5's reference once named; DF R7.5 checks the build over Fixture T's spectra, so DF M2's bound covers them; R6.6's offset cases set the file's Lab from the build's, testing the comparison alone.
- [x] **The outcome-change case** — TR3-M1, PL3-m4. UJ 3's case uses a Demo Device scan, moving TK-2 from R6.8b to R6.8g, and asserts the reason after Import.
- [x] **Importer-side saved-device check** — TR3-M3. UJ 3 asserts no saved-device record exists after import and adds a case with a saved live-kind Nix Spectro 2 record left unchanged and unoccupied; Import's Device obligation line names it.

## Minors and Nits

- [x] **Commit recheck scope, order and hold** — SSE3-m2, PL3-m1, TR3-n4, DB3-m2, TR3-m5, AR3-n1, DB3-n3. R3.8k: every eligible record's match and outcome and every count or list E43's Toolkit lines showed, within the commit's write hold, in the order E13, E40, E46, then E13's collection variant; UJ 3 case adding a swatch coded TK-3 mid-preview.
- [x] **Salvage replays in record order** — IF3-m3, AR3-m1, PL3-m6, DB3-m1, SSE3-m5, TR3-m4. DF R5.5d; DJ6's salvage case covers six pairings with reasons and predecessors.
- [x] **NULL, not empty** — IF3-n2, AR3-m2, PL3-m3, DB3-m3, SSE3-m4, TR3-n1, DB2-n1 partial. UJ 3, Collection Mode UJ2.1-r and the ADR-0003 row say absent (NULL), the model included; post-lock points at the ADR row instead of paraphrasing it.
- [x] **ADR-0003 row** — AR3-n2, DB3-n2, DB3 Missing (unique code backstop). The predecessor cites R2.3h; the imported reading's own-values reference is named; the uniqueness constraint is a backstop that R6.8f's check still counts a restore against; Import R3.3's one item per code is a candidate backstop. Engine settings (`foreign_keys`, journal mode, `busy_timeout`) predate this amendment and stay ADR-0003's own work.
- [x] **Toolkit variants named in R6.1; blank codes case** — PMM3-m1, IF3-n5. R6.1 names E5, E7, E9, E10 and E42's Toolkit variants; UJ 3 case with every code blank (E9, then E42).
- [x] **E46 lists colours per mode** — PMM3-m2. The mixed-modes variant lists the colours under each mode; OQ 3 asks whether a colour can move between Toolkit collections.
- [x] **Export E1 wording** — PMM3-m3. "SpectroCapture takes no … from one".
- [x] **Copy nits** — PMM3-n1, PMM3-n2, PMM3-n3, SSE3 nit (E47 singular). "Export the collection again" in E5 and E47; "unknown or not recorded"; E48 "where it can be" and "new ones are listed"; E47 "each column"; E43's not-checked reason adds "unreadable".
- [x] **Journey labels** — PMM3-n4, PM-R3-n1, IF3-n1, TR3-n2. "Now a swatch's colour" and "kept in history" throughout UJ 3.
- [x] **Stale fix-file lines** — PMM3-n5, PM-R3-n4. Round 1's Toolkit-remedy line marked superseded; round 2's anchor count corrected to three.
- [x] **Creation-form label mirrored** — PMM3-n6, IF3-n6. Import's Capture obligation line names "Measurement condition: ⟨mode⟩, set by this Nix Toolkit export".
- [x] **M2's causes** — PM-R3-n2, PL3-n1, SSE3 nit. M2: no record excluded by E7, E9, E10 or E47; no E5, E46 mixed-modes or E49 refusal.
- [x] **Pre-fill record** — PM-R3-n3, SSE3 nit. R6.3: the first record left after R6.4's exclusions.
- [x] **Counts in the repo** — PRIV3-1. M2 and OQ 3: tallies summed over two or more files; from one file, only format facts and whether it imported cleanly. AGENTS.md: "record counts — format facts and Import M2's cross-file tallies aside".
- [x] **Serial column question** — PRIV3-2. OQ 3 asks whether a serial or firmware column appears, and revises R6.2 and E43 from the answer.
- [x] **Set-aside listing for an unreadable item** — IF3-m1. R6.8b lists an unreadable set-aside item; UJ 3's quarantined case asserts the listing and exactly one current reading (DB3-n1).
- [x] **Quarantined live current** — TR3-m3. R6.8 routes a match with no readable current value to R6.8b before the kind test; UJ 3 case.
- [x] **Grammar cases** — IF3-m2, TR3-m7. UJ 3 case with two failing columns on one record, a basic-form time, and a sub-millisecond digit truncated.
- [x] **R2.3-equal collection names** — TR3-m6. UJ 3 case.
- [x] **Blank illuminant and observer** — TR3-m8. UJ 3's blank-device case also blanks the pair, stored absent and listed not checked.
- [x] **Restore then re-import in P0** — TR3-m10. The case declares the restore under DF R7.2.
- [x] **Export assertions** — TR3-m11, IF3-n7. EJ1 asserts R7.7o's quarantined imported current and its reading kept behind as `superseded`; R1.1t's model cell "or empty".
- [x] **Comma read of Fixture T** — TR3-m12. UJ 3's missing-column case asserts only "not a Toolkit export: no E48".
- [x] **Collection Mode build row** — PL3-m7. The imported rows' row covers R8.9's imported shape and M2's ZX-022 and governs overlaps; UJ2.1-r adds ZX-026 with no model (TR3-m9).
- [x] **Import's Device line and status line** — IF3-n3, IF3-n8. "if any"; the status line names every Toolkit and collection variant.

## Recorded without change

- [x] **PRIV3-3** — the review log's coarse facts about the real export are not data values; later rounds report matches by category only.
- [x] **Mapping the file's illuminant text to an offered pair** — SSE3 nit. Pinned by the fixture's forms until Capture OQ 21 widens the set; OQ 3's corpus records the forms seen.
