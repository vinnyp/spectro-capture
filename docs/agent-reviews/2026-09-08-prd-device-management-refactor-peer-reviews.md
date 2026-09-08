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
