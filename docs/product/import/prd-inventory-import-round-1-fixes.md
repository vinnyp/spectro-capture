# Nix Toolkit import amendment — round 1 fixes

Resume point for round 1 of the Nix Toolkit export amendment (review log: `docs/agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md`, Round 1). One box per finding group; each box names every finding it answers, from all nine lenses. Lens prefixes: PM (product manager), SSE1 (staff engineer), TR1 (test), IF1 (interface), AR1 (architecture), PRIV (privacy), PMM1 (product marketing), PL1 (plan), DB1 (database). Owner decisions N16–N23 (Round 1 adjudication) authorise the status-bearing choices; fences Import F84–F91, DF F66–F70, Collection Mode F223 and the dated Clarified lines record them.

## Landed before the fix pass

- [x] **Real export values in the fixture and log** — PRIV-1, PRIV-2, SSE1-B3, PL1-M4, PMM1-m10. Fixture T's three Date Saved values, TK-3's name and the re-import variant's dates replaced with invented ones; Round 0's quoted collection name and reading count redacted; commit 417fdac rewritten as 2a54fce before any push. A local-only check (never committed) confirms no real-export value remains in the diff or in the review texts filed below.

## Owner decisions (N16–N23)

- [x] **N16 history order** — AR1-M2, TR1-M1, PL1-M8, DB1-M2, SSE1-M6, IF1-M11, PM-m10, PRIV-9a. Measured view only: DF R2.1 names the measured order as an over-time view; F64 Clarified; DJ6 row 2 asserts both orders; R6.8c says "not current"; Collection Mode UJ2.1-s discriminates.
- [x] **N17 reasons** — AR1-M3, DB1-M3 (reason half), SSE1-m5, DB1-m (R2.4), TR1-M4b, IF1-m11, PL1-m2. Re-measurement over a simulated or imported A, else initial; DF R2.4 carves out R2.3j; DF R2.3h fixes the predecessor as the reading current when B landed (DB1-M3 predecessor half); DJ6 case. IF1-m11 (two "First reading" lines on one item) is the owner's N17 outcome and stands.
- [x] **N18 simulated current** — PM2, PL1-m1, IF1-m3, SSE1-m9, TR1-m (R6.8c Demo), DB1-m (kind). R6.8 keys on the current reading's snapshot kind; new R6.8g; DF R2.3j; UJ 3 case.
- [x] **N19 same date, different spectrum** — DB1-M1c, SSE1-M2, AR1-M1c, PL1-M1c, TR1-M4a, IF1-B1 (tie half), PM-m5 (tie). Kept in history, not current, listed in E43 (R6.8d, R6.9); UJ 3 case.
- [x] **N20 set-aside items** — PM-m5 (set-aside half), SSE1-m9, PMM (Missing list). R6.8b captures them and E43 lists them; a quarantined current reading counts as no current value; UJ 3 case.
- [x] **N21 one Toolkit collection per file** — PM6 (scenario B). New R6.11 and E49; R6.3's pre-fill relies on it; UJ 3 case.
- [x] **N22 unchecked records** — PM9, PL1-M7, SSE1-M3/M4 (file Lab and pair halves), DB1-M5 (R6.6 half), IF1-M13 (pair half), AR1-m4, PM-m2. R6.6 lists records it cannot check, never as matching; UJ 3 case at D65/10.
- [x] **N23 budgets** — DF 8,900 (DF F70), Collection Mode 12,550 (Collection Mode F223).

## Row and case fixes (no new owner decision; each within a recorded fence)

- [x] **Same-reading rule (Blocker)** — PM1, TR1-B2, AR1-B1, PL1-M1a/b, DB1-B1, SSE1-B1, IF1-B1. New R6.8f ahead of R6.8b–d; Vocabulary defines "same reading"; DF R2.3j "creates none"; F82 Clarified from N14's own words ("An identical reading (same date and spectrum) changes nothing"); UJ 3 cases for a repeat over the scan-current state, newer-then-older, and a Flag then re-import; DJ6 case.
- [x] **Equality defined** — DB1-M1a/b/d, SSE1-M2, TR1-M4. Same Date Saved instant to the millisecond and every reflectance the same number; UJ 3 case re-importing Fixture T rewritten with plain decimals and a +00:00 offset.
- [x] **Reading counts** — TR1-B1, PL1-B1, AR1-M6, IF1-B2, PMM1-M7, DB1-M7, SSE1-M1, SSE1-m7, IF1-M6. R6.9 partitions the eligible readings; UJ 3 :118 reads 2 becoming current; :120 asserts its counts.
- [x] **Placeholders registered** — IF1-M6, PMM1-m9. Single-word tokens in Capture §12's placeholder list and Import copy's note; ⟨unchanged readings⟩ becomes ⟨same⟩.
- [x] **Fixture format in the repo** — TR1-B3, SSE1-B2, PM8, PL1-M2 (fixture half), PL1-m7, SSE1-n4. UJ 3's paragraph states the format facts (bare `;`, unquoted header, the four quoted columns, scientific notation, XYZ 0–1, sRGB integers, lowercase HEX, two-place densities, Date Saved form); TK-3 written `1.04700000e+00`; codes, Index and densities declared.
- [x] **Synthetic defined; independent oracle** — PRIV-4, PL1-M3, TR1-m (IJ:110 derivation). R6.10 defines synthetic, cites UJ 3's format, covers derived fixtures and goldens, bars reports; the file's own values come from an independent reference.
- [x] **Gamut flag oracle** — TR1-M5. Fixture T declares TK-2 outside sRGB and TK-1 inside; UJ 3 asserts the flag.
- [x] **Value grammar** — PL1-M2, SSE1-M3, DB1-M5, TR1-M6, IF1-M13 (mode/device half), AR1-m5, SSE1-M4 (mode, order). R6.7 states readable times, modes (M0–M2), numbers (finite; NaN, Infinity, empty and comma decimals unreadable) and runs before R6.4; negative reflectances are kept as given like values above 1.0 (N9, "nothing invented"); a blank Nix Device leaves the model unknown; UJ 3 cases for NaN, a time with no zone and an empty mode.
- [x] **Mode adoption tied to the commit** — PM3, AR1-M5, PL1-M6, DB1-M4, SSE1-M5, IF1-M2, PMM1-M5, TR1-m (IJ:117), PM-m6, PM-m7, DB1-m (R3.2), DB1-n3. R6.4 (written by the commit, "hold no reading" defined, rechecked at commit), R3.2, R3.8k, R6.3 (creation form's mode fixed), Capture R1.10, E43's mode line; UJ 3 cases for Cancel, E44 and a change between preview and Import.
- [x] **Illuminant and observer as provenance** — PL1-M7, AR1-m4, DB1-m (illuminant), TR1-m (IMP:167 vs Capture R4.6), SSE1-m10. R6.5 keeps the file's pair as the reference its own values used, never a derived set's; derived values under the collection's reference.
- [x] **Every Toolkit column disposed** — PRIV-3, PM-m1, TR1-m (Custom Collection Name), AR1-m10, PL1-M5, SSE1-m1, SSE1-m12, IF1-M12, DB1-m (unlisted columns), PL1-n1, IF1-n4, DB1-n1. R6.2 names each column's fate, Custom Collection Name read for R6.3/R6.11 and not stored, unlisted columns as metadata listed in E43; UJ 3 asserts the stored column list.
- [x] **`undefined` Note supplies no value** — PRIV-5, TR1-M8, AR1-m8, DB1-M6, IF1-m10, TR1-m (real Note value). R6.2 under F73's "handled without prompts"; UJ 3 cases keep a typed note and store a real one.
- [x] **Recognition and read controls** — PM5, IF1-m4, IF1-M1, SSE1-m2, PL1-m5, AR1-m9, PM-m4, TR1-m (R1.5 override), TR1-m (R6.1 negatives). R6.1 tries semicolon, comma and tab; encoding and separator shown fixed, E43 offering neither control; UJ 3 cases for a comma re-save, a missing signature column and an extra column.
- [x] **Fixed-mapping notice** — PMM1-M6, IF1-m12. New E48 on the mapping step.
- [x] **Toolkit remedies** — PM6 (scenario A), PMM1-m4. The copy's Toolkit rule: E7, E9, E10, E42 and E47 end "Correct it in the Nix Toolkit and export it again"; whether a Toolkit code can be blank goes to OQ 3.
- [x] **E14 Toolkit variant** — PMM1-B1, IF1-M3, PM-m8. E14 variant; R3.5 carve-out.
- [x] **E43 Toolkit variant** — PMM1-M4, PMM1-n2, PMM1-n4, IF1-m6, PL1-m4, AR1-n2, SSE1-n2, PM-n2, PRIV-6, PMM (Missing: what the file lacks). The Toolkit line leads, distinct labels, the mode-change, set-aside, differing and unchecked lines, what the file doesn't carry, and that imported readings stay in history.
- [x] **E46 headlines and remedies** — PMM1-M3, IF1-M7, PMM1-m3, PM-m13, PL1-n2, PMM1-m2, PM-m12. One headline per variant; the no-readings option named; the unverified export-by-mode claim dropped; "measurement condition" bridged to the Toolkit's "Measurement Mode".
- [x] **Collection Mode E12 meaning** — PMM1-M1, PM4, IF1-M4. E12 gains the imported sentence; F221 Clarified names E12 and R2.8.
- [x] **Collection Mode detail and history copy** — PMM1-M2, IF1-M5, TR1-m (CMC:295), PL1-m3, SSE1-m6, PM-m11, IF1-m8. R4.2d and R5.2e copy lines show every unrecorded field; R5.2b's device line for an unknown serial.
- [x] **Imported chip label** — PMM1-m1, AR1-n3. Chip label "From Nix Toolkit", clear of the " (imported)" column tag.
- [x] **Collection Mode cases and metric** — TR1-m (CMJ:236), TR1-m (CM:574), PL1-m9. UJ2.1-r measured in M1; M2's population includes an imported reading.
- [x] **DF reading model** — AR1-M4, SSE1-M7, TR1-m (DF:37), PM-m9, IF1-m2, DB1-m (vocabulary), TR1-m (DF:99/239), AR1-m2, DB1-n2, PL1-m2 (fields half). Vocabulary, R2.1, R1.2, R7.1 and R2.3j name a reading with no samples, basis, verdict or spread; unknowns read back empty, never a sentinel; restoring an imported reading copies that state (R2.3j).
- [x] **Imported fixture and golden** — AR1-M7, TR1-M7, SSE1-M8, IF1-M10, PL1-G2, DB1-m (fixture), PRIV-9b. DF R7.7o; R7.7 and R7.2 extended; Export R1.1h/s/i and R4.2 assert `sc_imported`.
- [x] **Salvage keeps a scan current** — AR1-m1, DB1-m (salvage). DF R5.5d.
- [x] **Obligation lines both ways** — IF1-M8, IF1-M9, IF1-m7, SSE1-n1, PM-m17, PL1-n3, IF1-m13, PRIV-9 nit. Import's Data Export, Capture and Collection Mode lines; Device ↔ DF and Device ↔ Export lines; DF's Import Rows; line 5 and Ready to capture; the PRD README's device line.
- [x] **ADR-0003 inputs** — AR1-m11, AR1-m3, DB1 (Missing list), IF1-m14, IF1-m9, SSE1-M7 (ADR half). docs/decisions/README.md and post-lock gain the schema inputs: a reading with no samples, the imported kind and a reading's origin, explicit current selection and predecessor, the same-reading key excluding restores.
- [x] **AGENTS.md** — PL1-M9, IF1-m2 (AGENTS half). §3 Open list and §8's Canonical value and Raw payload entries.
- [x] **Format variance tracked** — PM7, PL1-m12, SSE1-m13, SSE1 q20, AR1-m12. Import OQ 3 and M2; post-lock dogfood item.
- [x] **Build order named** — PL1-G1, SSE1-m3. Import's Build dependencies; R6.6 cites DF OQ 6.
- [x] **Wavelength grid assumption** — AR1-m7, SSE1-m8, TR1-m (EX:120), PL1-m10, DB1-m (grid). Export OQ 4's decision-so-far names the Toolkit grid; post-lock spike item.
- [x] **Capture counts** — PM-m16. Capture M8 leaves out Toolkit imports.
- [x] **Export surface** — PMM1-m5, IF1-n3, TR1-m (EX:88–90/EXJ:14). E1's line names the empty cells; R1.2 says the two columns are never both true; R1.1h/s/i; EJ1 asserts model, measured-at, never-scanned empty and the P1 history half.
- [x] **Vision and index** — PMM1-M8, PMM1-m6, PMM1-m7, PM-n1. Problems bullet; J7 risk marked superseded in part; feature note; the index's v2 line.
- [x] **Import editorial** — PM-m3, TR1-m (IMP:83/IJ:36), TR1-n (IMP:133), SSE1-m4, PMM1-n1, IF1-n1, IF1-n2, PL1-n4, PL1-m8, TR1-m (IJ:109/121, IJ:120, IJ:110 below tolerance), TR1-n (IJ:110), TR1-n (DF:27). R3.8a–p; every-state row covers E46–E49; §4 names UJ 3; Nix Toolkit vocabulary; "except" in amended lines; R1.5 carve-out; R6.11 compares under R2.3; cases for an existing target ignoring the name, a pre-filled name that collides, an absent item and a below-tolerance record; DJ6's index.

## Deferred to post-lock, each with its reason

- [x] **Near-miss Toolkit state** (PMM Missing, SSE1 deferral) — a file with some but not all signature headers; post-lock next pass on Import, since OQ 3's corpus shows which near-misses occur.
- [x] **"Scanned" counts include imported items** — AR1-n1, IF1-m5, PL1-m11. A Capture copy question for its next pass; the counts stay correct as captured tallies.
- [x] **Re-scan is P1** — PM-m14. In the first build phase the imported mark clears through Flag and set-aside review; recorded in post-lock.
- [x] **Root README and help** — PMM1-m8, PM-m15. Documentation items.
- [x] **Nix Device a user label?** — PRIV-8. Hardware spike item.
- [x] **Cross-model egress** — PRIV-7. No cross-model pass ran this round; any later one gets a format-only extract, never the research report.
- [x] **Import performance** — SSE1-m11. Dogfood budget run includes a ROWS_TARGET Toolkit file.

## Rejected

- [x] **PMM1-n3** — E40's target reason ("new swatches can't join its queue mid-run") still states why the import waits; a Toolkit import's items also join the collection a session is using. No change.
- [x] **PL1-M4 owner call on redaction** — the owner's N3 ("Keep it out of the repo") already covers it; redacted without a new question.
