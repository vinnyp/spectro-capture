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
