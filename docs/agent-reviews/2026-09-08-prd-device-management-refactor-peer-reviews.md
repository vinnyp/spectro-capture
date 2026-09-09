# Peer-review gate — prd-device-management-refactor (2026-09-08)

**Mode:** requirements. **Reviewers:** peer-product-manager-reviewer, peer-staff-software-engineer-reviewer, peer-test-reviewer, peer-interface-reviewer. **persona-version:** cache/1.4.0.

## Findings

### peer-product-manager-reviewer

See the round sections below: each lens's findings are consolidated in the verify-the-reviewer table and the "Minors accepted" paragraph of the round in which they were raised; the reviewers' full outputs are kept outside the repo.

### peer-staff-software-engineer-reviewer

See the round sections below: each lens's findings are consolidated in the verify-the-reviewer table and the "Minors accepted" paragraph of the round in which they were raised; the reviewers' full outputs are kept outside the repo.

### peer-test-reviewer

See the round sections below: each lens's findings are consolidated in the verify-the-reviewer table and the "Minors accepted" paragraph of the round in which they were raised; the reviewers' full outputs are kept outside the repo.

### peer-interface-reviewer

See the round sections below: each lens's findings are consolidated in the verify-the-reviewer table and the "Minors accepted" paragraph of the round in which they were raised; the reviewers' full outputs are kept outside the repo.

## Verify-the-reviewer dispositions

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| 1 |  |  |  |  |

## Round 1 — verification of the F9 refactor (2026-09-08)

**Subject:** commit `4b0f474` — the locked device-management PRD refactored under fence F9 (row IDs, two-sentence rows, journeys/copy/oq-results companions, an obligations table both ways, `&amp;` → `&`), verified against the pre-refactor source at `fd21ab7`. **Lenses:** product manager, staff engineer, test (retargeted), interface, on Claude/Opus. Each lens independently diffed the 83 source rows against the 100 new rows, the 32 copy states, and the journeys: no rule, constant, named state, citation, or owner decision was lost or weakened by compaction; the copy states and journeys moved byte-identically. **Tier rationale:** the PRD tier's always-on lenses plus interface, because the refactor changes the ID and cross-document contract two locked PRDs cite.

| lens | verdict | new findings | rows OBJECT |
|---|---|---|---|
| product manager | builds the right thing; no loss found | 3 Minor, 2 Nit | R1.8, R2.6, R3.5, R4.3, R5.1, E7, E23 |
| staff engineer | proceed after three Majors | 3 Major, 5 Minor, 4 Nit | R1.8, R2.11, R2.15, R3.5, R6.22, R6.27 |
| test (retargeted) | trustworthy; a genuine compaction | 1 Major, 6 Minor, 4 Nit | R1.8, R2.14, R3.5 |
| interface | contract sound; loss-free | 1 Major, 4 Minor, 4 Nit | R1.8, R2.14, R3.5, R4.1, R6.22, E11, E13, E15, E18, E32 |

### Verify-the-reviewer dispositions

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| D1-F1 | The pairing and calibration retry counts are provisional constants with no open question, so under F7 R1.8 and R3.5 can never ship | all four (Major) | Legend `:63`, `:116`, `:193` confirmed; source rows carried the same bare "TBD-on-spike". | accept — owner: one OQ 28 for both (F9 clarification (1a)). |
| D1-F2 | The Legend's rule "every provisional constant carries its candidate value" is contradicted by six constants with none, and the closers need halt-log data that cannot exist until the thresholds hold values; no dogfood carve-out like the capture PRD's F23 | Staff (Major) | `:53`–`:63` confirmed; capture `:66` carries F23. | accept — owner: F7 clarification (1), the dogfood carve-out; the rule reads "where one exists". |
| D1-F3 | The Capture Mode obligations line says R6.27 "moves into the first build phase" while its Pri cell says P1; the sibling line for R5.18 uses "must move with the amendment" | Staff (Major) | `:305` vs `:279`, `:304` confirmed. | accept — the line takes the R5.18 form. |
| D1-F4 | R4.1's muted-audio advisory has no Demo Device seam: R6.9 cannot declare audio muted | Test (Minor) | `:203`, `:226`, `:261` confirmed; pre-existing. | accept — owner: R6.9 gains the settable audio state (F9 clarification (1b)). |
| D1-F5 | R2.14 gained "forks ship their own provider ID", not in the source row | Test (Minor), Interface (Nit) | fd21ab7 row 32 confirmed; F6 text. | accept as recorded — owner keeps it (F9 clarification (1c)). |

Minors accepted (all editorial): R2.6, R4.3, E7 carry a pointer to the pending spectral-data amendment (the capture PRD's OQ 16) (PM); R5.1 cites E23 and R4.4 cites E19/E20 (PM); the inbound half of the Inherited-obligations preamble says a cited row is the one the obligation attaches to, and the naming PRD's row is authoritative (Test); the capture PRD's persistence cite retargets from §5 to the obligations table (Test); the two "device PRD §7" labels read "the device PRD's copy file" (all); R4.1 cites OQ 3 (Interface); OQ 22 regains "the pinned enumeration is R6.22's source of truth for the full error set" and R6.22 cites OQ 22 (Interface, Staff); the copy file gains a Status cell per row and ‹P1› marks on E11, E13, E15, E18's "Extend offline use", E32 (Interface; F9 clarification (1d)); R2.15 regains the approximate-date fallback clause (Staff); the Legend's bullet 3 drops the backoff ladder, which R2.11 settles (Staff); the fence → row map lists the rows that name each fence (Staff); §7 gains a Placeholders paragraph naming ⟨model⟩, ⟨serial⟩, ⟨name⟩, ⟨date⟩ and R1.7's provisional-form substitution (Staff); the results file gains sections for OQ 4, 19, 25 (Test); the Legend gains the "For the engineering plan" roll-up (Staff). Nits accepted: §2's Traces adds UJ1.1; Legend bullet 5 drops R4.6; `:295` drops the R5.19 cite; the journeys header uses the capture journeys wording. Post-lock: E18's "you can do it right now" against R2.16's rule; the Surfaces table's E22 placement; OQ 5/9 Closer derivation.

### Fix pass (resume point)

- [x] **DF-1** — OQ 28 "Pairing and calibration retry counts": Decision so far "None"; Interim rule "None — the consecutive-failure counts stay unset"; Closer "Owner decision with dogfood data — owner"; Feeds R1.8, R3.5; Status open. R1.8, R3.5, and the Legend's bullet cite it.
  - _Result:_ Done: OQ 28 added at `:373` as the table's 30th question; R1.8 `:123`, R3.5 `:200` and the Legend's bullet `:63` cite it, and its Feeds cell cites both rows back.
- [x] **DF-2** — Legend `:53`: "carrying its candidate value where one exists, and its Open Questions id … no provisional constant ships in a release without its OQ resolved (fence F7); a dogfood build is not a release for that rule, so the first sessions run on candidate values and produce the data those questions need (F7, clarification (1))".
  - _Result:_ Done: the Legend's provisional-constants paragraph `:53` now reads "where one exists" and carries the dogfood carve-out, F7 clarification (1) — the only rule the six candidate-less constants were failing.
- [x] **DF-3** — Obligations `:305`: "the configurable-latency row ([R6.27]), P1 here, must move into the first build phase with the amendment carrying these notes, pacing at that PRD's DEMO_SCAN_CYCLE".
  - _Result:_ Done: the Capture Mode §6 obligations line now takes the R5.18 form — "[R6.27], P1 here, must move into the first build phase with the amendment carrying these notes, pacing at that PRD's DEMO_SCAN_CYCLE".
- [x] **DF-4** — R6.9's settable list gains "system audio output muted / not muted ([R4.1])"; R4.1's advisory cites R6.9. Two sentences max.
  - _Result:_ Done: R6.9's settable list gains "settable system audio output muted / not muted ([R4.1])" and R4.1's advisory cites R6.9 back; both rows still two sentences, R rows still 100.
- [x] **DF-5** — The editorial Minors and Nits listed above, each the smallest edit; the two capture-file label changes and the one capture-file link retarget are the only edits outside the device files.
  - _Result:_ Done: all fourteen Minors and four Nits, each the smallest edit — R2.6/R4.3/E7 amendment pointers, R5.1→E23, R4.4→E19/E20, the inbound-obligations preamble line, R4.1→OQ 3, OQ 22↔R6.22, R2.15's approximate-date clause, Legend bullets 3 and 5, the fence → row map, §7's Placeholders, the OQ 4/19/25 results sections, the engineering-plan roll-up, §2's UJ1.1, the R5.19 cite dropped, the journeys header. Outside the device files: the two "device PRD's copy file" labels and the persistence cite retargeted to `#inherited-obligations`.
- [x] **DF-6** — Copy file: a Status column ("🤝 Aligned" on every row); ‹P1› marks per clarification (1d) with one header sentence as the capture copy header's; the Placeholders paragraph (in §7, cited from the copy header).
  - _Result:_ Done: the copy file takes a Status column (🤝 Aligned on all 32 rows), ‹P1› on E11, E13, E15, E32 and on E18's "Extend offline use" action, a header sentence matching the capture copy header's, and a cite to §7's new Placeholders paragraph; §7's whole-table status sentence becomes per-row.
- [x] **DF-7** — Checks as RP-8, plus: OQ 28 resolves from both rows; `grep -c '⌛️'` is 0 in every device file; R rows still 100 (DF-4 edits a row, adds none); counts and word counts.
  - _Result:_ Done: 1,233 links and anchors resolve across the device files and every inbound file (the only 4 misses stay the pre-existing quoted excerpts in the 2026-09-06 capture review log); `&amp;` survives only where the refactor log and F9 name the string itself; 0 bare cross-PRD row labels; every requirement row two sentences or fewer (R6.1's third "sentence" is the period inside its quoted helper text); all 11 mermaid blocks render; R 100 (R1 22, R2 19, R3 5, R4 6, R5 20, R6 28), all 🤝 Aligned; E 32, each with a 🤝 Aligned Status cell; M 5, all 🤝; OQ 30; `⌛️` appears once per PRD, in the Legend's value list, on no row.

Rows edited by the pass and re-verified in round 2: R1.8, R2.6, R2.11, R2.15, R3.5, R4.1, R4.3, R4.4, R5.1, R6.9, R6.22, R6.27, E7, E11, E13, E15, E18, E23, E32; every other row is consented by all four lenses and stays 🤝.

## Round 2 — delta on the round-1 fixes (2026-09-08)

**Subject:** commit `212b616`. **Lenses:** product manager, staff engineer, test (retargeted), interface, on Claude/Opus over the nineteen edited rows, OQ 28, and the edited paragraphs. All round-1 findings verified RESOLVED except D1-F3 (PARTIAL: the conditional form is wrong for R6.27) and D1-F4 (PARTIAL: the seam is binary and filed inside the device mock).

| lens | verdict | new findings | rows OBJECT |
|---|---|---|---|
| product manager | builds the right thing | 2 Major, 6 Minor, 2 Nit | R1.8, R3.5, R4.1, R4.4, R6.9, E13, E15, E18, OQ 28 |
| staff engineer | not ready — one Blocker | 1 Blocker, 1 Major, 4 Minor, 3 Nit | R4.1, R6.9, R6.27, E18, OQ 28, the roll-up |
| test (retargeted) | trustworthy | 5 Minor, 3 Nit | R4.1, R6.9, OQ 28, the roll-up, the results file |
| interface | contract sound | 5 Minor, 3 Nit | E3, E7, E18 |

### Verify-the-reviewer dispositions

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| D2-F1 | R6.27 is P1 with a conditional obligations line, while the capture PRD's locked P0 R11.3 places the latency row in the first phase unconditionally; the device PRD's own inbound preamble makes the naming PRD's row authoritative | Staff (Blocker) | capture `:387` P0 unconditional; device `:286` P1. | accept — owner: R6.27 → P0 (F8 clarification (1)); the obligations line drops the conditional. |
| D2-F2 | The engineering-plan roll-up lists OQ 3, 5, 28 as runnable now (OQ 3 waits on hardware-gated OQ 2; 5 and 28 need dogfood data) and lists OQ 3 under "no interim rule" though it has one; "Six constants" counts open questions | PM, Staff (Major), Test, Interface (Minor) | `:70`, `:72`, `:346`, `:349`, `:373` confirmed. | accept — bullets 2 and 4 corrected; R1.14 (OQ 24) added; "Six open questions hold every constant that has no candidate value". |
| D2-F3 | OQ 28 has no candidate value, so the escape hatches cannot build in dogfood | PM (Major) | `:373` "None". | accept — owner: candidate 3 (F9 clarification (2a)). |
| D2-F4 | E12 is produced by no requirement row | PM (Minor), Staff, Test (nit) | grep: Surfaces table only. | accept — owner: new R1.23, P0 (F9 clarification (2b)). |
| D2-F5 | E18's body promises an action the P0 build withholds | PM, Staff, Interface (Minor) | copy `:27`. | accept — owner: drop the clause (F9 clarification (2c)). |
| D2-F6 | R6.9's audio seam is two-valued where R4.1's "verified muted" is three-branched, and is filed inside the device mock rather than the shell-side seam list | Staff, Test, PM (Minor) | `:210`, `:268`, `:289`. | accept — three-valued; §6's seam note names system audio output (F9 clarification (2d)). |

Minors accepted: E3's "Leave setup for now" ‹P1› (Interface; clarification (2e)); R4.4 reads "a one-tap way forward — re-check after charging or connecting power (E19), recalibrate (§3), extend offline use or reconnect (§2), re-check after freeing disk space (E20)" (PM); the F7 carve-out gains "where a constant has no candidate, the engineering plan sets a dogfood value, which the OQ's closer then replaces" (Interface); the fence-map preamble reads "A row not listed here cites no fence; F7, F8 and F9 bind every row by inheritance rather than by citation" and the F9 cell is titled Scope (Interface); OQ 4, 19, 25 gain results cites and the closing rule says eleven sections (Test); the results header says "three of the questions it bears on" (Test); the copy header says a marked state whose only exit is from an unmarked loop leaves the loop's own state carrying the exit as guidance (PM). Nits accepted: R4.3's pointer reads "the amendment's readiness half, already absorbed"; the `〈date〉` glyph; R5.5 "shows the advisory" for "warns"; OQ 19's results section marks the Apple-reference clause as the table's claim, not the audit's; E7's and E12's "Not copy" notes move to a footnote under the copy table. Post-lock: the journeys' nine "copy, §7" labels; OQ 5's "dogfood data" closer with no producing row.

### Fix pass (resume point)

- [x] **DG-1** — R6.27 Pri → P0; the Capture Mode §6 obligations line reads "the configurable-latency row ([R6.27]) is in the first build phase, pacing at that PRD's DEMO_SCAN_CYCLE".
  - _Result:_ Done: R6.27's Pri is P0 (`:287`) and the Capture Mode §6 obligations line drops the conditional — "the configurable-latency row ([R6.27]) is in the first build phase, pacing at that PRD's DEMO_SCAN_CYCLE".
- [x] **DG-2** — Roll-up: bullet 2 drops OQ 3 from R4.1 and adds "R1.14 (OQ 24)"; bullet 4 reads "the questions an owner or a checked-in artifact closes without hardware (OQ 12, 13, 14, 19, 21, 22, 24) run now; OQ 3 waits on OQ 2's hardware probe, and OQ 5 and OQ 28 wait on dogfood data from the first build"; bullet 1 opens "Six open questions hold every constant that has no candidate value:".
  - _Result:_ Done: bullet 2 drops OQ 3 from R4.1 and adds [R1.14] (OQ 24) in document order; bullet 4 splits the parallel track three ways; bullet 1 counts open questions and reads "Five", because DG-3's candidate takes OQ 28 out of the no-candidate set in the same pass — the one departure from the checklist's literal wording, flagged for the owner.
- [x] **DG-3** — OQ 28: Decision so far "None; candidate 3 consecutive failures."; R1.8, R3.5, and the Legend bullet name the candidate.
  - _Result:_ Done: OQ 28 `:374` reads "None; candidate 3 consecutive failures.", its Interim rule carries the candidate rather than "stay unset" (leaving "None" there would contradict the new candidate), and R1.8 `:124`, R3.5 `:201` and the Legend bullet `:63` name it.
- [x] **DG-4** — R1.23 (v1, P0): "A pairing attempt that fails shows the pairing-failed state ([E12]) with a retry action." Status 🤝 Aligned (owner-authorized, verified in round 3). Traceability's §1 count and the Surfaces table unchanged.
  - _Result:_ Done: R1.23 (v1, P0, 🤝 Aligned) added at `:123`, ahead of R1.8 in the pairing table — the capture PRD's precedent of placing an added row topically and letting its ID sit out of numeric order; one sentence; Traceability and the Surfaces table untouched. R rows now 101.
- [x] **DG-5** — E18's body drops "— you're online, so you can do it right now". E3's "Leave setup for now" gains ‹P1›. Copy header gains the loop-exit sentence. E7's and E12's "Not copy" notes become a footnote under the table.
  - _Result:_ Done: E18's body reads "Extend offline use to keep scanning."; E3's "Leave setup for now" carries ‹P1›; the copy header gains the loop-exit sentence; E7's and E12's "Not copy" notes are one footnote under the table.
- [x] **DG-6** — R6.9: "settable system audio output state (muted / not muted / not determinable) ([R4.1])"; §6's shell-side seam note adds "system audio output". R4.4's wording; R4.3's pointer; R5.5's verb.
  - _Result:_ Done: R6.9's audio state is three-valued (muted / not muted / not determinable) and §6's shell-side seam note names system audio output; R4.4's way-forward wording, R4.3's "already absorbed" pointer, R5.5's "shows the advisory". R4.1 is unedited — its "verified muted" reading already matches the three-valued seam.
- [x] **DG-7** — Legend `:53` carve-out clause; fence-map preamble and the F9 cell title; OQ 4/19/25 results cites and the closing rule's count; results header wording; OQ 19's source note; the `〈date〉` glyph.
  - _Result:_ Done: the Legend carve-out `:53` gains the dogfood-value clause; the fence-map preamble and the F9 cell's "Scope:" title; OQ 4, 19, 25 gain results cites and the closing rule says eleven sections; the results header says "three of the questions it bears on"; OQ 19's results section marks the Apple-reference clause as the table's claim; the ⟨date⟩ glyph at `:30`.
- [x] **DG-8** — Checks as DF-7; R rows now 101; every row 🤝; counts and word counts.
  - _Result:_ Done: 1,305 links and anchors resolve across the device files and every inbound file (the only 4 misses stay the pre-existing quoted excerpts in the 2026-09-06 capture review log); `&amp;` survives only in the fence file and the refactor log, which name the string itself; 0 bare cross-PRD row labels; every requirement row two sentences or fewer (R6.1's third "sentence" is the period inside its quoted helper text); all 11 mermaid blocks render; R 101 (R1 23, R2 19, R3 5, R4 6, R5 20, R6 28), all 🤝 Aligned; E 32, all 🤝; M 5, all 🤝; OQ 30; `⌛️` appears once, in the Legend's value list, on no row. Words: PRD 11,729, copy 1,787, journeys 1,814, oq-results 1,032, fences 1,005.

Rows edited by the pass and re-verified in round 3: R1.8, R1.23 (new), R3.5, R4.1, R4.3, R4.4, R5.5, R6.9, R6.27, E3, E7, E12, E18, OQ 28, and the Legend, obligations, fence-map, and results-file paragraphs.

## Round 3 — delta on the round-2 fixes (2026-09-08)

**Subject:** commit `a5bb7e9`. **Lenses:** product manager, staff engineer, test (retargeted), interface, on Claude/Opus over the fourteen edited rows, OQ 28, and the edited paragraphs. Every round-2 finding verified RESOLVED except the F7 clause (Legend only, not in the fence) and the copy header's loop-exit rule (no state realizes it).

| lens | verdict | new findings | rows OBJECT |
|---|---|---|---|
| product manager | builds the right thing | 1 Major, 4 Minor, 1 Nit | R6.27, E12, E18, the Legend paragraphs |
| staff engineer | proceed after one Major | 1 Major, 4 Minor | R4.1, R6.9, E12, E14, R1.20, the Legend paragraphs |
| test (retargeted) | trustworthy after one Major | 1 Major, 3 Minor, 1 Nit | E12, OQ 28, the Legend paragraphs |
| interface | contract sound | 1 Major, 4 Minor, 1 Nit | R1.23, R6.27, E3, E12, E14, the obligations line |

### Verify-the-reviewer dispositions

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| D3-F1 | The copy header's loop-exit rule governs E12 and E14, and neither body carries the exit; a literal P0 build ships the retry loop with no door | Staff, Test, Interface (Major), PM (Minor) | copy `:4`, `:21`, `:23`; E13/E15 ‹P1›. | accept — owner: both bodies close with E13's exit sentence (F9 clarification (3a)). |
| D3-F2 | E18 has no P0 remedy: both renewal rows are P1 and R2.12's silent renewal applies only while E17 is shown | PM (Major) | `:178`–`:183` confirmed. | accept — owner: R2.12 renews while E17 or E18 is shown (F9 clarification (3b)). |
| D3-F3 | E3's ‹P1› exit action is produced by no row in any phase | Interface (Minor) | R1.8 and R3.5 scope the exit to their loops. | accept — owner: R1.8's exit generalizes to any first-run setup state (F9 clarification (3c)). |
| D3-F4 | OQ 28's closer names dogfood data no row produces | Test (Minor), PM (Nit) | R5.12 records halts only. | accept as post-lock — owner: revisited with OQ 5 (F9 clarification (3e)). |
| D3-F5 | Roll-up: R6.27 (OQ 10) missing from bullet 2; OQ 4b, 7, 20b are labelled hardware but close on halt-log data from the first sessions | PM, Staff, Test, Interface (Minor) | `:70`, `:72`, `:349`, `:352`, `:366`. | accept — R6.27 added to bullet 2; 4b, 7, 20b join the dogfood clause of bullet 4 and the closing rule's exception. |
| D3-F6 | The Legend attributes the dogfood-value clause to F7 clarification (1), which did not carry it; R4.4's rewording has no fence provenance | PM, Staff, Test (Minor) | fences `:33`. | accept — clause appended to F7 (1); R4.4 recorded under F9 (3d). |
| D3-F7 | The audio seam is asserted in the device mock (R6.9, R4.1) and in the shell-side seam list | Staff (Minor) | `:211`, `:269`, `:290`. | accept — R6.9's clause reads "and, shell-side as §6's seam note says, a settable system audio output state (…)"; R4.1 cites the seam note. |

Minors accepted: R1.20 cites "R1.1–R1.8 and R1.23"; the Capture Mode §6 obligations line is marked "already absorbed" like R4.3. Post-lock list: the journeys' nine "copy, §7" labels; the capture PRD's two "moves from P1 into the first build phase" sentences (R11.3 and its obligations table), stale now that R6.27 is P0, ride that PRD's OQ 16 amendment; E12's "Try again" versus the state diagram's return to discovery; OQ 5 and OQ 28's closers.

### Fix pass (resume point)

- [x] **DH-1** — E12 and E14 bodies close with "You can come back to this any time from the device panel."; the copy header's loop-exit sentence names that sentence as the guidance; the footnote at the table's end says E12/E14 carry it until E13/E15 land.
  - _Result:_ Done: E12 and E14 close with "You can come back to this any time from the device panel."; the copy header's loop-exit sentence now names that sentence; the footnote adds "E12 and E14: each closes with E13's and E15's exit sentence, carrying it as guidance until those states land."
- [x] **DH-2** — R2.12: "…while either that state or the offline-use-ended state ([E18]) is shown the app listens for connectivity and renews silently the moment any returns". Two sentences max. E18's row cites R2.12 in the footnote or its body stays as is.
  - _Result:_ Done: R2.12 renews while E17 or E18 is shown, two sentences; its second sentence now seams on the withheld "Extend offline use" action ([R2.10], with [R2.11]'s pre-emptive renewal) rather than on renewal-on-reconnect, which F9 (3b) moves into P0 — flagged for the owner. E18's row stays as is.
- [x] **DH-3** — R1.8's second sentence: "Any first-run setup state offers 'Leave setup for now', landing on the device panel, so setup is never an inescapable loop." (or the closest two-sentence form that keeps R1.8's counter rule). E3's action keeps ‹P1›.
  - _Result:_ Done: R1.8's second sentence keeps the counter rule and generalizes the exit — "…and any first-run setup state offers the same 'Leave setup for now' exit, landing on the device panel — including the no-internet-to-authorize state ([E3])." Two sentences; E3's action keeps ‹P1›.
- [x] **DH-4** — Roll-up bullet 2 adds [R6.27] (OQ 10); bullet 4's dogfood clause reads "and OQ 4b, 5, 7, 20b and 28 wait on halt-log and dogfood data from the first build"; the closing rule's exception matches.
  - _Result:_ Done: roll-up bullet 2 ends "[R6.27] (OQ 10)" in document order; bullet 4 reads "…and OQ 4b, 5, 7, 20b and 28 wait on halt-log and dogfood data from the first build"; the closing rule's exception adds "OQ 4b, 7 and 20b are labelled hardware but close on the same first-build halt logs".
- [x] **DH-5** — R6.9's audio clause and R4.1's cite per D3-F7; R1.20's range cite; the §6 obligations line "already absorbed".
  - _Result:_ Done: R6.9's audio clause reads "and, shell-side as [§6]'s seam note says, a settable system audio output state (muted / not muted / not determinable) ([R4.1])"; R4.1 cites the seam note; R1.20 cites "[R1.1]–[R1.8] and [R1.23]"; the Capture Mode §6 obligations line ends "pacing at that PRD's DEMO_SCAN_CYCLE — already absorbed".
- [x] **DH-6** — Checks as DG-8; R 101; every row 🤝; counts.
  - _Result:_ Done: 1,252 relative link instances (2,370 file+anchor checks) resolve across the device files and every inbound file, +7 on the round-2 baseline and exactly the seven links this pass added; the only 4 misses stay the pre-existing quoted excerpts in the 2026-09-06 capture review log. 0 bare cross-PRD row labels; every requirement row two sentences or fewer (R6.1's third "sentence" is the period inside its quoted helper text); all 11 mermaid blocks render; R 101 (R1 23, R2 19, R3 5, R4 6, R5 20, R6 28), all 🤝 Aligned; E 32, all 🤝; M 5, all 🤝; OQ 30 (22 open, 8 residual); `&amp;` survives only in the fence file and the refactor log, which name the string itself; `⌛️` appears once, in the Legend's value list, on no row. Words: PRD 11,812, copy 1,845, journeys 1,814, oq-results 1,032, fences 1,160.

Rows edited and re-verified in round 4 (staff, test, product manager): R1.8, R1.20, R2.12, R4.1, R6.9, E12, E14, plus the roll-up and closing rule.

## Round 4 — delta on the round-3 fixes (2026-09-08)

**Subject:** commit `7253d4f`. **Lenses:** product manager, staff engineer, test (retargeted), on Claude/Opus over R1.8, R1.20, R2.12, R4.1, R6.9, E12, E14 and the edited paragraphs. Round-3 items D3-F1, D3-F5, D3-F6, D3-F7 and the Minors verified RESOLVED; D3-F2 and D3-F3 PARTIAL.

| lens | verdict | new findings | rows OBJECT |
|---|---|---|---|
| product manager | builds the right thing; one Major | 1 Major, 3 Minor, 2 Nit | R1.8, R2.12, E3, E5, E7, E8, E18 |
| staff engineer | proceed after three Majors | 3 Major, 3 Minor, 1 Nit | R1.8, R2.12, R4.1, E3, E12, E14, E18 |
| test (retargeted) | not yet safe | 1 Blocker, 1 Major, 1 Minor, 2 Nit | R1.8, R2.12, E3, E18 |

### Verify-the-reviewer dispositions

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| D4-F1 | R2.12 renews "the moment any [connectivity] returns", a transition E18 has already passed, and E18 has no unmarked action, so an online user with a lapsed window is blocked with no control in P0 — narrower than F9 (3b) | Test (Blocker), PM, Staff (Major) | `:180`, copy `:27`, fences (3b). | accept — owner: "whenever connectivity is present, including the moment it returns"; E17 offline / E18 online named; E18 gains "Check again" (F9 clarification (4c)). |
| D4-F2 | R1.8's "any first-run setup state" is an open set reaching the license states whose action cells say None, and the row gives two destinations | PM, Test, Staff (Major) | `:124` vs `:201`, `:106`, copy `:14`–`:17`. | accept — owner: the blocking states only (E3, E12/E13, E14/E15); on first run the exit lands on the device panel, mirroring R3.5 (F9 clarification (4b)). |
| D4-F3 | The exit guidance in E12/E14 is true only if first-run setup states are non-modal, which the document never states | Staff (Major) | E16 the only non-modal state named; R3.1 "guided flow". | accept — owner: non-modal; one Surfaces sentence (F9 clarification (4a)). |
| D4-F4 | E3 got the ‹P1› action's producing row but no P0 guidance sentence | PM, Test (Minor) | copy `:12`. | accept — E3's body closes with the exit sentence (F9 clarification (4d)). |

Minors accepted: R4.1 "TBD pending their open questions (OQ 3, OQ 4b, OQ 5)" (Staff); bullet 4 opens "The hardware spike's scope is every OQ whose closer is 'hardware' except OQ 4b, 7 and 20b, which close on first-build halt logs" (Staff); R2.12's second sentence narrows to E18 (Test, Staff); the "already absorbed" mark moves to the latency clause's end (PM). Post-lock: the workflow diagram's missing leave-setup edge from Calibration; E12/E14's guidance says leaving is possible without saying how; R5.2's forward reference to ADR-0004.

### Fix pass (resume point)

- [x] **DI-1** — R2.12, sentence 1: "If the pre-authorization window has expired, the pre-flight check blocks an acquisition session start — with the reconnect-once state ([E17]) while offline, or the offline-use-ended state ([E18]) while online — and while either is shown the app renews silently whenever connectivity is present, including the moment it returns; expiry never surfaces as a mystery mid-queue failure." Sentence 2: "Under the P1-seam convention ([R4.4]) the P0 phase renders E18 without the "Extend offline use" action, which arrives with [R2.10] alongside [R2.11]'s pre-emptive renewal." E18's actions: "Check again; Extend offline use ‹P1›".
  - _Result:_ Done: R2.12 carries the checklist's two sentences verbatim with E17, E18, R4.4, R2.10 and R2.11 linked in the neighbouring rows' form; E18's actions read "Check again; Extend offline use ‹P1›", so an online user with a lapsed window has an unmarked P0 control.
- [x] **DI-2** — R1.8, sentence 2: "The consecutive-failure counter resets on a successful attempt and on leaving setup, and the states that block first-run progress — device authorization ([E3]), pairing, calibration — offer the same exit, which on first run lands on the device panel." Sentence 1 unchanged.
  - _Result:_ Done: R1.8's sentence 2 is the checklist's, with E3 linked; the exit is now the three blocking first-run states only, one destination, and sentence 1 is untouched — the row stays two sentences.
- [x] **DI-3** — Surfaces paragraph gains: "First-run setup states are non-modal: they render on the device panel, which stays reachable, so leaving one never needs an action." E3's body closes with "You can come back to this any time from the device panel."; the footnote names E3 with E12 and E14.
  - _Result:_ Done: the Surfaces paragraph gains the non-modal sentence as its third; E3's body closes with "You can come back to this any time from the device panel."; the copy file's "Not copy." note now reads "E3, E12 and E14", naming E3's own ‹P1› action as what its sentence stands in for.
- [x] **DI-4** — R4.1's clause; bullet 4's opening clause; the "already absorbed" placement.
  - _Result:_ Done: R4.1 ends "TBD pending their open questions (OQ 3, OQ 4b, OQ 5)"; bullet 4 opens "…whose closer is "hardware" except OQ 4b, 7 and 20b, which close on first-build halt logs"; the §6 obligations line's mark is now ", already absorbed" — R4.3's comma form, closing the latency clause instead of reading off DEMO_SCAN_CYCLE.
- [x] **DI-5** — Checks as DH-6; R 101; every row 🤝; counts.
  - _Result:_ Done: 1,279 relative link instances (2,418 file+anchor checks) resolve across the device files and every inbound file, unchanged in count by this pass (375 in the PRD, 6 in the copy file, both before and after); the only 4 misses stay the pre-existing quoted excerpts in the 2026-09-06 capture review log. 0 bare cross-PRD row labels; every requirement row two sentences or fewer (R6.1's third "sentence" is still the period inside its quoted helper text); all 11 mermaid blocks re-extracted from source and rendering; R 101 (R1 23, R2 19, R3 5, R4 6, R5 20, R6 28), all 🤝 Aligned; E 32, all 🤝; M 5, all 🤝; OQ 30 (22 open, 8 residual); `&amp;` survives only in the fence file and the refactor log, which name the string itself; `⌛️` appears once, in the Legend's value list, on no row. Words: PRD 11,845, copy 1,872, journeys 1,814, oq-results 1,032, fences 1,283.

Rows edited and re-verified in round 5 (staff, test): R1.8, R2.12, R4.1, E3, E18, the Surfaces paragraph, bullet 4.

## Round 5 — delta on the round-4 fixes (2026-09-08)

**Subject:** commit `1d8db64`. **Lenses:** staff engineer and test (retargeted) on Claude/Opus over R1.8, R2.12, R4.1, E3, E18, the Surfaces sentence, bullet 4, the obligations mark, and the copy footnote. D4-F1 and D4-F4 RESOLVED; D4-F2 and D4-F3 PARTIAL (the exit set not yet in E12/E14's cells; non-modality stated in prose no row owns).

| lens | verdict | new findings |
|---|---|---|
| staff engineer | ready to re-lock after two Majors | 2 Major, 3 Minor, 2 Nit |
| test (retargeted) | trustworthy after two Majors | 2 Major, 2 Minor, 1 Nit |

### Verify-the-reviewer dispositions

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| D5-F1 | E12 and E14 carry no "Leave setup for now ‹P1›" although F9 (4b) names them | Test (Major) | copy `:21`, `:23`. | accept — realization of (4b); clarification (5b). |
| D5-F2 | The non-modal sentence is prose with no row, status, or seam; a modal P0 sheet passes every row | Test (Major) | `:97`; Traceability `:87`. | accept — R1.23 carries it; §6 gains the observation; clarification (5a). |
| D5-F3 | R4.4 says "guidance but no action" where the Legend, §7 and the copy header say "without that action"; R2.12 cites R4.4, so E18's Check again could be read out again | Staff (Major) | `:214` vs `:51`, `:325`, copy `:4`. | accept — three words; clarification (5e). |
| D5-F4 | "Leaving setup" has no defining event now that states are non-modal; the action's visible effect on first run is undefined; "first run" is undefined | Staff (Major, Minor) | `:124`, `:201`, `:97`. | accept — clarification (5c), (5d), recorded by the orchestrator as precision, owner may overrule. |
| D5-F5 | Non-modality does not span the device picker (E1); E18's body names the withheld control and lacks E17's coverage hedge; R4.1's "every check" overreaches by one | Staff, Test (Minor) | `:97`, `:106`, copy `:27`, `:211`. | accept — clarification (5a), (5f); R4.1 names the three checks and cites R2.12 for the fourth. |

Post-lock list: E17's "Check again" while still offline; a producing row for "Check again"; R2.12's "reconnect-once" shorthand against §5's device-reconnect vocabulary; the copy header's loop-exit rule written for a marked state where E3's case is a marked action; the workflow diagram's missing leave-setup edges and single AuthBlocked state; the journeys' nine "copy, §7" labels; OQ 5's and OQ 28's closers; the capture PRD's two stale "moves from P1" sentences (that PRD's OQ 16 amendment); R5.2's forward reference to ADR-0004; E12's "Try again" versus the diagram.

### Lock pass (editorial and precision edits under clarification (5); verified by orchestrator diff and one bounded test-lens check)

Five rounds have each surfaced a new Minor-to-Major in the same first-run seam after the previous round's fix, the pattern the capture-mode arc's retro named. The items above realize decisions already fenced; they are applied as a lock pass, the diff is verified by the orchestrator, and the test lens runs one bounded check over the four rule-touching rows (R1.8, R1.23, R3.5, R4.4) where only a Blocker stops the lock.

- [x] **LK-1** — R1.23: "A pairing attempt that fails shows the pairing-failed state ([E12]) with a retry action. First-run setup states render non-modally on the device panel or in the device picker, both of which stay reachable, so leaving one never needs an action." The Surfaces sentence becomes a pointer to R1.23. §6: R6.9's list or R6.17's observability gains "and whether the device panel's other affordances stay operable while a setup state shows ([R1.23])" — whichever row keeps to two sentences.
  - _Done:_ R1.23 carries the sentence verbatim (two sentences); the Surfaces paragraph now reads "First-run setup states are non-modal ([R1.23])."; the clause went to R6.17, which observes rather than sets, appended to its second sentence and still two.
- [x] **LK-2** — E12: "Try again; Leave setup for now ‹P1›"; E14: "Recalibrate; Leave setup for now ‹P1›"; the footnote's "until its own exit lands" now reads true for all three.
  - _Done:_ both action cells set as written; the footnote's trailing clause, which named E13's and E15's states as E12's and E14's exit, now reads "carrying it as guidance until its own ‹P1› "Leave setup for now" action lands." for all three.
- [x] **LK-3** — R1.8 sentence 2: "The counter resets on a successful attempt and on leaving setup — navigating away from a setup state by that action or otherwise — and the states that block first-run progress (device authorization ([E3]), pairing, calibration) offer the same exit, which dismisses the state and on first run returns the device panel to its normal content." R3.5 cites R1.8 for the leaving rule rather than restating it. Legend gains: "First run: the app has no saved device yet."
  - _Done:_ R1.8's sentence 2 replaced verbatim, [E3] in the neighbours' link form; R3.5's sentence 2 is now "The counter reset and the exit's effect follow [R1.8]'s leaving rule."; the Legend's terminology paragraph gains ""First run" means the app has no saved device yet." — prose form, matching its two neighbouring definitions.
- [x] **LK-4** — R4.4: "with guidance and without that action". R4.1: "the pass/warn/block boundaries for the battery, calibration and storage checks are TBD pending their open questions (OQ 3, OQ 4b, OQ 5); the authorization check's block boundary is [R2.12]'s."
  - _Done:_ both replaced verbatim; R4.4's sentence 2 now matches the Legend, §7 and the copy header's "without that action", and R4.1 stays two sentences.
- [x] **LK-5** — E18's body: "Extend offline use to keep scanning. Reconnecting refreshes this automatically — or check again now. If this doesn't clear, the device may not be covered by your license." R2.16 cites [E17] and [E18].
  - _Done:_ E18's body replaced verbatim; R2.16 now reads "the offline-use-expired copy ([E17], [E18]) must not promise that reconnecting will fix it" and stays one sentence; E18's action cell was already "Check again; Extend offline use ‹P1›" from round 4 and is unchanged.
- [x] **LK-6** — Checks as DI-5; R 101; every row 🤝; counts.
  - _Result:_ Done: 1,258 relative link instances (2,382 file+anchor checks) resolve across the device files and every inbound file, up 6 from 1,252 by this pass (381 in the PRD, was 375; 6 in the copy file, unchanged); the only 4 misses stay the pre-existing quoted excerpts in the 2026-09-06 capture review log. 0 bare cross-PRD row labels in the normative device files and in every inbound file (the fences file's round-1 "the capture PRD's R4.26" is prose in a decision record, untouched); every requirement row two sentences or fewer (R6.1's third "sentence" is still the period inside its quoted helper text); all 11 mermaid blocks re-extracted from source and rendering; R 101 (R1 23, R2 19, R3 5, R4 6, R5 20, R6 28), all 🤝 Aligned; E 32, all 🤝, ids contiguous E1–E32; M 5, all 🤝; OQ 30 (22 open, 8 residual); `&amp;` survives only in the fence file and the refactor log, which name the string itself; `⌛️` appears once, in the Legend's value list, on no row. Words: PRD 11,898, copy 1,896, journeys 1,814, oq-results 1,032, fences 1,461 (unedited; the round-4 count of 1,283 predates clarification (5)).

Rows edited in the lock pass: R1.8, R1.23, R2.16, R3.5, R4.1, R4.4, R6.17, the Surfaces paragraph, the Legend's terminology paragraph; copy E12, E14, E18 and the footnote.

## Lock record (2026-09-08)

- **Bounded pre-lock check** (test lens over R1.8, R1.23, R3.5, R4.4, R6.17, E12, E14, E18): LOCK; every round-5 finding RESOLVED; no rule in scope contradicts another; no P0 first-run state lacks both an escape and a control.
- **Every row aligned:** 101 R rows, 32 E rows, 5 M rows, all 🤝; no owner overrule stands. Every moved row traced to the pre-refactor source at `fd21ab7`; every rule change since carries a fence clarification (F7 (1); F8 (1); F9 (1)–(5)).
- **Verification rounds:** five full or delta rounds plus the bounded check, on Claude/Opus (product manager, staff engineer, test, interface). The tier rationale: the PRD tier's always-on lenses plus interface, because the refactor changed the ID and cross-document contract two locked PRDs cite.
- **OQ contract:** 30 questions (22 open, 8 residual); eleven results sections, one for each question carrying evidence; OQ 28 added for the retry counts.
- **Zero placeholders; template comments deleted; mechanical checks:** links and anchors resolve across the device files and every inbound file (the four known misses are quoted excerpts inside the capture review log); no bare cross-PRD row label; every requirement row at most two sentences; eleven mermaid blocks render; `&amp;` gone from `docs/product` except where quoted as the fixed string.
- **Process file** `prd-device-management-refactor-pass.md` (the F9 spec and compaction map, every box ticked) is deleted at lock; it is recoverable from the branch history, and this log holds every finding and the compaction summary.
- **Header** reads `Status: locked (2026-09-08)`; the product README row reads Locked.

### Post-lock list (consolidated)

Rows and copy: R1.8's two landings (collection vs device panel on first run) read as complementary but not as one destination; under non-modality the counter also resets on incidental navigation; R4.4's "satisfied from P1" over-generalizes; R1.23's "or in the device picker" clause binds nothing while Surfaces assigns first-run states to the panel, so E1's non-modality is unstated; R6.17's affordance clause names no seam; E18's body leads with the withheld control; E17's "Check again" while still offline has no stated behaviour and no row produces "Check again"; R2.12's "reconnect-once" shorthand collides with §5's device-reconnect vocabulary; the copy header's loop-exit rule is written for a marked state where E3's case is a marked action.

Indexes and journeys: the journeys' nine "copy, §7" labels; the workflow diagram's single AuthBlocked state and missing leave-setup edges from Pairing and Calibration; E12's "Try again" versus the diagram's return to discovery; R5.2's forward reference to ADR-0004.

Cross-document: the capture PRD's two "moves from P1 into the first build phase" sentences (R11.3 and its obligations table) are stale now that R6.27 is P0 and ride that PRD's OQ 16 amendment; OQ 5's and OQ 28's closers name dogfood data no row records.
