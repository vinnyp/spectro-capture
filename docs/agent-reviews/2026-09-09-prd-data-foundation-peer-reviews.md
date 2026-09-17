# Peer-review gate — prd-data-foundation (2026-09-09)

**Mode:** requirements. **Reviewers:** peer-product-manager-reviewer, peer-staff-software-engineer-reviewer, peer-test-reviewer, peer-privacy-reviewer, peer-product-marketing-manager-reviewer, peer-architecture-reviewer, peer-interface-reviewer. **persona-version:** cache/1.4.0.

## Findings

### peer-product-manager-reviewer

See the round sections: each lens's findings are consolidated in the round's verify-the-reviewer table and its fix file; the reviewers' full outputs are kept outside the repo.

### peer-staff-software-engineer-reviewer

See the round sections: each lens's findings are consolidated in the round's verify-the-reviewer table and its fix file; the reviewers' full outputs are kept outside the repo.

### peer-test-reviewer

See the round sections: each lens's findings are consolidated in the round's verify-the-reviewer table and its fix file; the reviewers' full outputs are kept outside the repo.

### peer-privacy-reviewer

See the round sections: each lens's findings are consolidated in the round's verify-the-reviewer table and its fix file; the reviewers' full outputs are kept outside the repo.

### peer-product-marketing-manager-reviewer

See the round sections: each lens's findings are consolidated in the round's verify-the-reviewer table and its fix file; the reviewers' full outputs are kept outside the repo.

### peer-architecture-reviewer

See the round sections: each lens's findings are consolidated in the round's verify-the-reviewer table and its fix file; the reviewers' full outputs are kept outside the repo.

### peer-interface-reviewer

See the round sections: each lens's findings are consolidated in the round's verify-the-reviewer table and its fix file; the reviewers' full outputs are kept outside the repo.

## Verify-the-reviewer dispositions

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| 1 |  |  |  |  |

## Round 1 — first full round (2026-09-09)

**Subject:** commit `2855fe3` — the first fill after Phase 3 (38 R rows, E1–E12, M1–M6, 15 OQs; 4,985 words). **Lenses:** the PRD tier's three always-on lenses (product manager, staff engineer, test retargeted) plus, by document shape, privacy (personal data, deletion, egress), marketing (twelve copy states), architecture (data ownership and the version-history consistency model), and interface (the CSV export and direct-query contracts) — seven on Claude/Opus. **Cross-model (offered once, owner chose two lenses):** agy ran privacy and staff; both returned rc 0 with conforming per-row tables — the first conforming cross-model run in this project, after the agent-dispatch 1.6.1 exemption for requirements-mode verdict tables. **Tier rationale:** every Tier-2/3 trigger above fires on this document's surface; database and performance lenses do not, since the schema is out of scope by F1.

| lens | verdict | Blocker | Major | Minor/Nit | rows OBJECT |
|---|---|---|---|---|---|
| product manager | sound after Blockers | 1 | 10 | 3 | 22 |
| staff engineer | not ready | 2 | 10 | 8 | 27 |
| test (retargeted) | tests don't prove the behaviour | 4 | 16 | 9 | 41 |
| privacy | ship after Critical+High | 0 (1 High) | 6 Medium | 3 | 12 |
| marketing (copy) | lands after Blockers | 1 | 8 | 5 | E1–E11 |
| architecture | build after Blockers | 1 | 9 | 4 | 26 |
| interface | breaks consumers | 1 | 10 | 8 | 24 |
| agy privacy (advisory) | ship after High | 0 (1 High) | 0 | 1 | R6.1, R6.5 |
| agy staff (advisory) | needs revisions | 1 | 1 | 1 | R1.2, R2.6, R3.3 |

No row flips: every row drew at least one OBJECT from a non-abstaining lens.

### Verify-the-reviewer dispositions (Blockers and cross-lens Majors)

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R1-F1 | The data model has no place for a multi-sample set: R2.1's "raw payload as the instrument produced it" contradicts the capture PRD's locked R4.12 (a row's measurement is the mean of N samples' curves); R6.2's counts and M6's size population swing 5× | Arch (Blocker), Staff (Blocker) | `:131` vs capture `:185` confirmed. | accept — owner: stored mean, samples a third tier (F10). |
| R1-F2 | R3.5 marks a non-spectral reading's derived values absent; capture R4.24 (P0, locked) says the mean is taken across the colour values under the fixed reference and the reading marked non-spectral | Staff (Blocker), Interface (Blocker), Marketing, Test | `:152` vs capture `:186` confirmed. | accept — R3.5 restated to the real boundary; the export carries the mark and basis (FX1-13). |
| R1-F3 | R1.2's "no app-private encoding" is unreachable beside a 25 MB budget only compression meets; R1.2 also overstates what an outside reader gets from an opaque vendor payload | agy staff (Blocker), PM, Arch, Staff, Interface | `:117`, `:136`, OQ 5 confirmed. | accept — owner: split the promise (F11). |
| R1-F4 | An unanswered capture-time re-scan is recorded as a correction and R2.5 drops it from every over-time view: fabricated history | PM (Blocker), Interface, Test, Staff, Arch | `:134`–`:135` confirmed. | accept — owner: unconfirmed reason, shown and marked (F12). |
| R1-F5 | E6 tells the reader the gamut mark is about their screen; F4 stores it against sRGB | Marketing (Blocker) | copy `:19` vs `:151` confirmed. | accept — copy rewritten (FX1-20). |
| R1-F6 | R1.4's offline-only test passes an app that exfiltrates when online | Test (Blocker) | `:119` confirmed; device R6.17 exists. | accept — online closed-set assertion (FX1-19). |
| R1-F7 | Two current readings on an item has no stated outcome; R7.2 injects it and asserts nothing | Test (Blocker), Arch | `:132`, `:209` confirmed. | accept — read-only into E5, never auto-repaired (FX1-14). |
| R1-F8 | Nothing asserts the gamut flag's verdict, only its column | Test (Blocker) | `:151`, `:212`, `:213` confirmed. | accept — reference set with known verdicts (FX1-19). |
| R1-F9 | The pre-upgrade snapshot and the user's copy are never opened; a plain file copy passes | Test (Blocker) | `:178`, copy `:15`, `:25` confirmed. | accept — the snapshot is opened and read back under an active write (FX1-17). |
| R1-F10 | The vendor SDK analytics disclosure has no ship gate or content floor | Privacy (High), agy privacy (High) | `:197`, OQ 12 confirmed. | accept — owner: a §6 gate row (F19). |
| R1-F11 | No posture for sync-managed or network volumes despite the vision calling the file the sync strategy | PM, Privacy, Arch, Staff (Major) | `:119`, journeys DJ3 confirmed. | accept — owner: warn, durability scoped to local (F13). |
| R1-F12 | First-run file creation and switching have no rows; E10 unbacked | PM, Arch (Major) | `:116`, copy `:23` confirmed. | accept — owner: ask on first run (F14). |
| R1-F13 | Whole-file delete has no state or undo semantics; collection delete has no state of its own | PM, Marketing, Test, Arch (Major) | `:193`–`:195` confirmed. | accept — owner: Finder's job; collection state added (F15). |
| R1-F14 | §6 P1 behind capture's P0 R1.8; R6.5 P1 guards a P0 export | PM, Staff, Interface (Minor/Major) | Pri cells confirmed. | accept — owner: R6.1, R6.2, R6.5 to P0 (F16). |
| R1-F15 | Free text inherits a measurement's immutability by silence | Privacy (Medium ×2) | `:133`, capture R8.13 confirmed. | accept — owner: correctable in place (F17). |
| R1-F16 | M3 demands the canonical value from a CSV that does not carry it | PM, Arch, Test, Interface (Major) | `:251` vs `:163` confirmed. | accept — owner: raw payload column (F18). |
| R1-F17 | The CSV has no dialect, layout, naming, ordering, version-location, or bump-rule contract; passthrough columns can collide with app columns; the history export has no shape | Interface (Major ×4), Test, PM, Privacy, Staff | `:163`–`:167`, `:213` confirmed. | accept — the export contract rows (FX1-18). |
| R1-F18 | Missing invariants: two time axes; derived-value key and one current set; a general atomic-write row; the file's format identity and compatibility floor; snapshot lifecycle; stale-hold release | Arch, Staff, Interface, Privacy (Major) | each cited row confirmed absent. | accept (FX1-4, 15, 16, 17, 5). |

Rejected: agy staff's request to drop older-stamp derived values (R3.3) and agy privacy's request for single-reading deletion (R6.1) — recorded with reasons in the fence file. Every other Minor and Nit is accepted and mapped in the round-1 fix file; post-lock: a redacted bug-report artifact, the remembered-mapping lifecycle, the "colour" spelling house decision.

**Round-1 status:** 12 owner decisions recorded as fences F10–F21 (F1 amended to 7,000 words). Fix file `prd-data-foundation-round-1-fixes.md` (22 boxes) verified item-for-item against all nine reviews. Round 2 is a delta verification by the same seven Claude lenses; the cross-model pass is not repeated.

## Round 2 — delta verification (2026-09-09)

**Subject:** commit `2d15310` — after the round-1 fix pass and fence F22 (55 numbered R rows plus R7.6a–n, E1–E18, M1–M8, 15 OQs; 7,069 words by the budget's count — links, HTML comments, code fences and table pipes stripped — 8,247 raw). **Lenses:** the same seven on Claude/Opus, each with its own round-1 review and the round-1 fix file as `--source`, asked per-finding RESOLVED/PARTIAL/UNRESOLVED plus a fresh pass. **Cross-model:** not repeated (offered once, round 1). **Tier rationale:** unchanged from round 1.

| lens | verdict | Blocker | Major | Minor/Nit | rows OBJECT |
|---|---|---|---|---|---|
| product manager | builds the right thing; three Majors | 0 | 3 | 7 | 5 |
| staff engineer | ready once four Majors land | 0 | 4 | 9 | 18 |
| test (retargeted) | trustworthy after fixing Majors | 0 | 5 | 11 | 17 |
| privacy | privacy-sound once three Mediums close | 0 | 3 Medium | 3 Low, 1 Info | 4 |
| marketing (copy) | lands and honest; three Majors | 0 | 3 | 8 | 26 |
| architecture | build after closing four Majors | 0 | 4 | 5 | 18 |
| interface | sound after fixing Blockers | 1 | 2 | 10 | 11 |

**Round-1 dispositions, per lens.** PM: 12 of 14 resolved, F-10 partial (the delete's non-recoverability stayed P1), F-13 unresolved (no metric on the never-destroys promise). Staff: 16 resolved, 5 partial (MJ2 budget candidate, MJ4 QC reason, MN5 R1.4's set, NT2, NT3), MN1 unresolved (the 3.31.0 candidate; FX1-2 recorded it as landed and it did not). Test: all 4 Blockers resolved on the mutation; 3 partial (R1.3 and R5.2 refusals lack a state; the absent cell is named but unasserted), 1 nit unresolved (R7.1 cites R1.3). Privacy: the High resolved. Marketing: 13 of 14 resolved, F-5 partial (E2's progress landed in copy, not in a row). Architecture: 10 of 12 resolved, A4 partial (cardinality), one nit unresolved (M6's Method). Interface: 12 resolved, 6 partial (all on the export contract's literal strings and the golden), F1-20 unresolved (E1's redundant read-only action).

**Row flips.** Every non-abstaining lens ALIGNs on 35 rows: R1.1, R2.2, R3.2, R3.4, R3.5, R5.3, R5.4, R5.5, R6.1, R6.2, R6.4, R6.6, R7.2, R7.5, R7.6, R7.6a–c, R7.6f–h, R7.6j–n, R7.8, E1, E4, E5, E12, M2, M5, M7, M8. Eleven of them receive meaning changes in this round's fix pass — R1.1, R6.2, R7.2, R7.6, R7.6b, R7.6m, R7.8, E1, E12, M5, M8 — so they are held at ⌛️ and flip on round 3's delta; the other 24 flip to 🤝 Aligned in the fix pass as bookkeeping this disposition authorizes (R7.6d, R7.6e, R7.6i drew OBJECTs and stay).

### Verify-the-reviewer dispositions (the Blocker and every Major, plus the cross-lens Mediums)

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R2-F1 | R4.8's `sc_` prefix (F22a) contradicts F20's `sRGB_gamut_clipped`, R4.3's and the locked device R6.5's `simulated`, and R4.7's wavelength-named columns; R7.7's golden header cannot be written | Interface (Blocker), Staff (Major), Test | `:191` vs `:187`, `:188`, `:190`, device `:265`, OQ 8 results `:31` confirmed — two owner fences collide on a literal string. | accept — owner: prefix everywhere (F23). |
| R2-F2 | The wavelength column set is never pinned, so "column order is fixed" and the golden have nothing to be fixed against | Interface (Major), Staff (Minor) | `:190`, `:265` confirmed; SDK audit `:30` states arrays, no grid. | accept — owner: fixed by the export format version, OQ 16 (F24). |
| R2-F3 | R4.9's append-without-bump and R7.7's one-golden-per-version cannot both hold; the fixture↔golden pairing is unstated | Interface (Major), Test (Minor) | `:192` vs `:265` confirmed. | accept — the golden asserts its columns in order at the head, appended columns only after; an append updates the golden in place with the diff as the review artifact; a fixture's export is asserted against the current golden. |
| R2-F4 | The sample tier is stated but not placed: R1.2's floor, R1.6's archive, and R4.1's single payload column give three answers for one reading of N samples | Arch (Major) | `:152`, `:135`, `:136`, `:186` confirmed. | accept — owner: samples readable at the floor (F25); one payload column per sample slot (F26). |
| R2-F5 | R1.10 states durability unscoped while F13 scopes it to local volumes; only R7.4 carries the scope | Arch (Major) | `:141` vs fence F13 confirmed. | accept — R1.10 stated for local volumes, R1.7 names what is not promised elsewhere, M1's target per volume class. |
| R2-F6 | R3.1 guarantees derived values "for every canonical value" and "one current set per reading" in one row; R2.5, R4.4, R7.1 and R1.2 read them off superseded readings | Arch (Major) | `:171` confirmed self-contradictory. | accept — owner: per reading, history included; a regeneration restamps every reading (F27). |
| R2-F7 | A multi-condition reading (capture R4.5: one measurement per scan mode, all kept) has no cardinality in the derived set and no shape in the one-row-per-item export | Arch (Major) | `:171`, `:186`–`:190` vs capture `:177`–`:178` confirmed. | accept — owner: the chosen condition's set is current; the canonical export emits it, the condition column saying which (F28). |
| R2-F8 | R4.1 never says which items get a row; a half-scanned collection exports either short or with unspecified blanks | PM (Major) | `:186`, `:153`, `:154` confirmed. | accept — owner: every item gets a row, a column marking never-scanned or quarantined (F29). |
| R2-F9 | The in-app move (R1.9, and R1.7's way out) has no failure states and no atomicity rule; R1.10 reads as writes into the file | PM (Major) | `:134`, `:139`, `:141` confirmed. | accept — R1.9 gains the outcome rule (one location at all times; a failed move leaves the original untouched), three named states, R7.8 induces them, R7.6d asserts them. |
| R2-F10 | The P0 delete ships without the outside-reader non-recoverability guarantee (inside P1 R6.3) and E8/E14's finality sentence is ‹P1›-gated mid-sentence | PM (Major), Marketing (Minor) | `:225`, copy `:25`–`:26` confirmed. | accept — the guarantee moves to R6.2 with a "says whether it can be undone" clause; E8/E14 re-split so finality stands unmarked. |
| R2-F11 | R1.3's in-session second-file refusal and R5.2's lossy-upgrade refusal are induced by R7.3 but have no named state; E10 and E3 are different scenarios | Test (Major), Staff (Major), Marketing (Major) | `:137`, `:209`, `:241` vs copy `:18`, `:29` confirmed. | accept — two new states; R7.3 asserts the state per induced condition. |
| R2-F12 | No §7 facility declares the launch context (no file yet; a path that is local, sync-managed, or a network volume; a moved file), so R1.7, R1.8, R1.9 cannot be induced | Test (Major) | `:240`–`:241` confirmed. | accept — R7.2 declares the launch context; R7.6c/d/e assert against it. |
| R2-F13 | The file format version (R5.6) is in no read-back set; DJ5 step 2 rests on a claim no test makes | Test (Major) | `:239`, `:304` confirmed. | accept — first item in R7.1's set; M4's fourth question. |
| R2-F14 | R7.7 asserts the header only, and R4.7 cites it for byte-identity it cannot assert; dialect and the empty-cell rule are unasserted | Test (Major) | `:190`, `:265` confirmed. | accept — R7.7 asserts a golden file (header and rows) for a fixture collection, and a second export byte-identical. |
| R2-F15 | The fixture population never exercises the collision rule or the absent cell | Test (Major) | `:265`, `:303` confirmed. | accept — R7.7 names the required fixture content. |
| R2-F16 | Three P0 user-facing states have no copy: R1.7's sync/network warning, R2.8's bulk review, R1.3's refusal; R7.6e and R7.6i assert against nothing | Marketing (Major), PM (Minor), Privacy (Medium) | Surfaces `:109`, `:113`, `:108` confirmed; the history-cost and analytics "—" cells are deliberate (staff) and stay. | accept — three states added; §8 says why two cells stay empty. |
| R2-F17 | E2 promises progress no row requires | Marketing (Major) | copy `:17`; grep of the body confirmed. | accept — R5.1 clause; R7.6b lists it. |
| R2-F18 | E3 tells the reader the snapshot exists and may not exist, offering "Show me the copy" either way | Marketing (Major) | copy `:18` confirmed. | accept — split as R4.5's failures were. |
| R2-F19 | R1.8's "before anything else" inserts a step into the locked device PRD's first-run journey (UJ1, UJ1.2, M2's start) with no obligation line | Staff (Major) | `:133`, `:289`, device `:336` confirmed; F14 fixes the order, so no ordering fork. | accept — a Device Management line under "imposes on others". |
| R2-F20 | R1.4's "exactly the authorization service plus telemetry" is false on the Demo Device (device R6.3: activation never runs) and on hardware (R6.6's SDK analytics) | Staff (Major), Test (Minor) | `:138` vs device `:263`, `:228` confirmed. | accept — a per-configuration allowlist the observed set is a subset of; equality kept on request contents; R6.12 cited. |
| R2-F21 | R2.3's "clearable" must mean removed from the file; R6.5's inventory omits kind and model and carries an unverifiable "use of the app" negative; R1.7/E13 should say where the file lives is where the data goes | Privacy (Medium ×3), Staff (Minor) | `:155`, `:227` vs device `:149`, copy `:14` confirmed. | accept. |

**Judgment calls recorded for the owner to overrule:** a re-scan over a quarantined reading records the reason `initial` and is not asked E11 (Interface F2-10; keeps R2.4's five reasons); the canonical and history exports carry an export-kind column rather than separate version counters (Interface F2-04); imported passthrough columns follow the app columns in the import file's order, and a column mapped to identity is emitted once under its `sc_` name (Interface F2-06); a QC reading records no supersession reason and is not counted among an item's readings (Staff MN3); the never-destroys promise gains a metric (PM F-13); AGENTS.md §8's canonical-value and raw-payload entries are amended to F10 in this PR, the vision's line left as a post-lock item.

**Round-2 status:** 7 owner decisions recorded as fences F23–F29 (F18 amended by F26). Fix file `prd-data-foundation-round-2-fixes.md` to be verified item-for-item against all seven reviews before dispatch; the body is 69 words over budget before the fix lands, so the fix pass includes a compaction pass under process rules 7 and 8. Round 3 is a delta by the same seven lenses.

## Round 3 — delta verification of the round-2 fix pass and the split (2026-09-10)

**Subject:** commit `8c02390` — the round-2 fix pass (`f14054a`, 47 of 48 boxes) followed by the split of the Data Export PRD out of Data Foundation under fence F30. Two documents reviewed as one contract: Data Foundation (46 R rows plus R7.6b–n, E1–E5 and E8–E26, M1–M2 and M4–M9, 12 OQs; 6,971 words) and Data Export (13 R rows plus R4.4a, E1–E4, M1, 4 OQs; 2,822 words). **Lenses:** the same seven on Claude/Opus, each with its own round-2 report as a source, asked per-finding RESOLVED/PARTIAL/UNRESOLVED and a fresh pass over both documents. **Cross-model:** not repeated. **Tier rationale:** unchanged.

| lens | verdict | Blocker | Major | Minor/Nit | rows OBJECT |
|---|---|---|---|---|---|
| product manager | builds the right thing; one Major | 0 | 1 | 5 | 5 |
| staff engineer | proceed after one Blocker and two Majors | 1 | 2 | 9 | 7 |
| test (retargeted) | trustworthy after fixing Majors | 0 | 2 | 7 | 4 |
| privacy | privacy-sound to ship | 0 | 1 Medium | 2 Low, 2 Info | 2 |
| marketing (copy) | lands and honest; one Major | 0 | 1 | 7 | 5 |
| architecture | sound after two Majors | 0 | 2 | 5 | 3 |
| interface | contract sound | 0 | 0 | 10 | 4 |

**Round-2 dispositions, per lens.** PM: 10 of 11 resolved, the AGENTS.md/vision item partial (the vision's line is post-lock). Staff: 13 of 13 resolved. Test: 15 resolved, M8's timing harness partial. Privacy: 6 resolved, P2-6 partial (the export-first sentence now overclaims history), 3 deferred post-lock as recorded. Marketing: 10 resolved, F2-5 partial (E1's list landed as one sentence), one nit unresolved ("full"). Architecture: 7 resolved, 2 partial (B4's file half, M6's corpus). Interface: 13 of 13 resolved. Every lens confirmed the split clean in both directions; staff resolved every link in all ten files (0 unresolved of 690) and matched the retired-ID list to the origin table line for line.

**Row flips.** Data Foundation: every non-abstaining lens ALIGNs on every row except R1.2, R3.1, R3.3, R6.5, R7.3, E8, E14, E16, E25, M6. Of the flip-eligible rows, those this round's fix file changes in meaning are held at ⌛️ — R2.3, R5.7, R5.8, R6.2, R7.2, R7.7, R7.8, M8, M9, E12, E13, E22 — and the other 44 flip to 🤝 Aligned: R1.1, R1.3–R1.10, R2.1, R2.4–R2.9, R5.1, R5.2, R5.6, R6.3, R7.1, R7.4, R7.6, R7.6b, R7.6d, R7.6e, R7.6i, R7.6m, E1–E3, E9–E11, E15, E19–E21, E23, E24, E26, M1, M4, M5. Data Export: OBJECTs on R1.1, R1.3, R2.3, R2.4, R2.5, R4.1, R4.2, E1; R4.3 and M1 are held (edited this pass); R1.2, R2.1, R2.2, R3.1, R4.4, E2, E3, E4 flip to 🤝 Aligned. Aligned rows edited under this round's fix file (R3.5, R7.5) keep alignment under process rule 6, the fix file saying so.

### Verify-the-reviewer dispositions (the Blocker, every Major, and the cross-lens Mediums)

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R3-F1 | Data Export R2.3's passthrough order rests on "the import file's order", which no Data Foundation row promises the file keeps, and which has no referent after a second import (import R3.2 appends rows; the import PRD's header is a set "in any order") | Staff (Blocker) | DF `:131` (R1.2's enumeration has no imported columns), DE `:138`, import `:24`, `:138` confirmed. | accept — DF R1.2 gains the imported columns under name and position with a second-import rule (existing positions kept, new columns appended after them — a judgment call the owner may overrule); both obligations lines name it; DE R2.3 cites it. |
| R3-F2 | R4.1's "appended columns only after them" contradicts R2.3's app-block-then-passthrough order, so an append either violates R2.3 or forces the bump R2.5 forbids; and one golden per version cannot cover two export shapes | Staff (Major), Interface (Minor) | DE `:138`, `:140`, `:162` confirmed. | accept — an appended app column lands at the end of the app block, column position is not part of the contract (a consumer parses by name); a golden per export kind per version. |
| R3-F3 | DF R6.5 cites DE R4.2 for the no-credential-in-any-export assertion, and R4.2 asserts only a column set | Staff (Major), Test (Minor), Privacy (Info) | DF `:204` vs DE `:163` confirmed. | accept — R4.2 gains the clause; DE's inbound line names R4.2. |
| R3-F4 | "An export reads the file and never writes it" arrived with the split and has no assertion on either side, and its meaning is unstated (bytes vs content) | Test (Major), Staff (Minor) | DE `:123`, `:163`–`:164`, DF `:218` confirmed. | accept — the promise is content: every value, mark and note reads back identical at the floor before and after, and an export succeeds against a file open read-only; R4.3 asserts it across a success and each failure. Byte-identity is not promised (a reader connection may checkpoint) — a judgment call. |
| R3-F5 | R7.3 induces "a regeneration interrupted partway" and observes a named state, but no regeneration state exists; resumability is asserted nowhere | Test (Major) | DF `:169`, `:218`, `:220` confirmed. | accept — out of R7.3's list; R7.5 interrupts, restarts, and asserts completion with R7.1's older-stamp enumeration empty. |
| R3-F6 | R3.1's "always present" for the chosen condition's set is falsified by R3.5 and by capture R1.10/R4.5/OQ 1 (a collection's chosen mode can outrun what a reading carries) | Arch (Major) | DF `:167`, `:171`, capture `:139`, `:177`, `:467` confirmed. | accept — the exception clause; DE R2.1 says what an empty colour column on a current row means. |
| R3-F7 | M6 sizes the store at 1,000 items while R7.7's corpus and OQ 5's closer are at ROWS_CEILING; OQ 5's candidate is a tenth of the design scale | Arch (Major), Test, Interface, Staff (M1 Minors) | DF `:264`, `:222`, `:277` confirmed. | accept — M6 states both points with the budget read at the ceiling; OQ 5's candidate re-derived (about 1.2 GB raw / 620 MB compressed before F27's and F31's factors) — the owner sees the number at OQ 5's closure. DE M1's population is R7.7's fixtures with the statistic scoped. |
| R3-F8 | E8/E14's "an export carries the readings and their history" is unmarked while the history export is P1; the pre-delete mitigation overclaims at P0 | PM (Major), Privacy (Medium) | copy `:23`–`:24`, DE `:59`, `:125` confirmed; `git show f14054a^` shows the clause is new. | accept — re-split with the history clause under ‹P1›; R6.2 qualified. |
| R3-F9 | Data Export's E1 names unscanned rows but not quarantined ones, nor the app-version and state columns; R1.1's "the rest empty" contradicts its own state column | Marketing (Major), Interface, PM (Minors) | DE copy `:16`, DE `:123` confirmed. | accept. |
| R3-F10 | Whether a non-chosen condition's derived set is kept or transient is unstated — a 2–3× multiplier OQ 5 does not count | Arch (Minor, owner fork) | DF `:167`, `:277` confirmed. | accept — owner: kept once asked for (F31). |

Every Minor and Nit from all seven reviews is accepted and mapped in the round-3 fix file. **Judgment calls recorded for the owner to overrule:** the second-import column rule (R3-F1); "never writes" read as content-identical at the floor plus working read-only (R3-F4); columns the export PRD does not name literally follow `sc_` plus lower snake_case with `sc_sRGB_*` the fenced exception (Interface F3-04); the out-of-grid wavelength branch drops "marked" and is declared unreachable in v1 the way R5.7 declares its floor (Interface F3-02, Staff); retired-version goldens document what each release emitted and feed a future parse-an-old-export test (Interface); the single-item export uses the collection surface with counts scoped to one item (PM F3-3). Not adopted, with the lens's own agreement: no rule about an export mid-session (architecture: the app serialises it) and no overwrite rule at the destination (the macOS save panel's).

**Round-3 status:** one owner decision recorded as F31. Fix file `prd-data-foundation-round-3-fixes.md` (beside Data Foundation, covering both PRDs) to be verified item-for-item against all seven reviews before dispatch. Round 4 is a delta by the same seven lenses; if it converges, Phase 5 (priority pass) and Phase 6 (the pre-lock round with the retargeted plan reviewer and a fresh lens) follow for both documents together.

## Round 4 — delta verification of the round-3 fix pass (2026-09-10)

**Subject:** commit `c6933a0` — the round-3 fix pass (29 boxes, both documents; DF 6,997 words, DE 3,193; 76 rows Aligned). **Lenses:** the same seven on Claude/Opus, each with its round-3 report as a source, briefed to converge. **Cross-model:** not repeated. **Tier rationale:** unchanged.

| lens | verdict | Blocker | Major | Minor/Nit | rows OBJECT |
|---|---|---|---|---|---|
| product manager | builds the right thing; one Major F31 opened | 0 | 1 | 4 | 5 |
| staff engineer | proceed; one Major | 0 | 1 | 4 | 4 |
| test (retargeted) | trustworthy after fixing Majors | 0 | 3 | 3 | 4 |
| privacy | privacy-sound to ship | 0 | 0 | 2 Low, 3 Info | 0 |
| marketing (copy) | lands and honest | 0 | 0 | 5 | 4 |
| architecture | sound — build it | 0 | 0 | 4 | 0 |
| interface | contract sound | 0 | 0 | 5 | 2 |

**Round-3 dispositions, per lens.** PM: 4 of 4 resolved, the mid-session nit deliberate. Staff: 12 of 13 resolved, FX3-4's read-only clause partial (its Major). Test: 10 of 10 resolved. Privacy: 3 of 3 resolved. Marketing: 7 of 8 resolved, F3-3 partial (one journey line unswept). Architecture: 7 of 7 resolved. Interface: 11 of 11 resolved. Three lenses raised no OBJECT on any row; no Blocker anywhere.

**Row flips.** Of the 32 rows held after round 3, every non-abstaining lens ALIGNs on 19; 17 of those are untouched by this round's fix file and flip to 🤝 Aligned — DF R2.3, R3.1, R3.3, R5.8, R6.5, R7.3, R7.8, M8, M9, E12, E16, E22, E25 (13) and DE R2.3, R2.4, R2.5, R4.2 (4) — while E13 and DE M1 are held for a wording edit. Held with OBJECTs: DF R1.2, R5.7, R6.2, R7.2, R7.7, M6, E8, E14; DE R1.1, R1.3, R4.1, R4.3, E1. Aligned rows edited under process rule 6 this round: R7.1 (sample read-back), R5.5 (the salvage output's home), R1.9 and R7.6l (editorial cite and listing changes).

### Verify-the-reviewer dispositions (every Major and the cross-lens Minors)

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R4-F1 | Fence F31 is asserted by no fixture — R7.7's list has no non-chosen condition's set — and M6's population has no condition factor while OQ 5 says the corpus measures it | Test (Major), Arch (Minor) | DF `:222`, `:264`, `:277` confirmed. | accept — R7.7's fixtures gain a reading with a second condition's set; M6's population gains the conditions-kept factor. |
| R4-F2 | Each sample's decoded values (R1.2, F25) are read back by no test, though F25's map names R7.1 | Test (Major) | DF `:131`, `:216`, fences `:195` confirmed. | accept — R7.1's read-back set gains each sample's decoded values (process rule 6; R7.1 stays Aligned). |
| R4-F3 | R1.1's per-row app version cannot survive R4.1's byte-for-byte comparison against a golden keyed on export format version | Test (Major) | DE `:123`, `:162` confirmed. | accept — the app-version column is asserted present and well-formed and set aside before the comparison. |
| R4-F4 | "An export succeeds against a file the app holds read-only" cites R5.3 (a newer-app file the app must not interpret) and R7.2 (which declares no such state) | Staff (Major), Test (Minor) | DE `:123`, `:164`; DF `:187`, `:217` confirmed. | accept — R1.1 anchors the promise to the connection, a file the app cannot interpret (R5.3, R5.7) not being exportable; R7.2 declares a file held open read-only; DF's inbound line names R7.2. |
| R4-F5 | F31's "carried by the history export" has no export row: both exports emit the chosen condition, and a per-condition reading would break R2.3's ordering and R4.1's golden | PM (Major), Arch (Minor), Interface (note) | fence `:161`, DE `:123`, `:125` confirmed. | accept — owner: chosen condition only in both exports; F31 and the export PRD's F9 amended; E1 names the condition the rows come out on; both obligations lines reworded. |
| R4-F6 | The state column's enum has no value for a superseded reading in the history export | Interface (Minor), Staff (Minor) | DE `:123`, `:125` confirmed. | accept — the state is the row's; history rows read superseded, the current-reading flag saying which row is canonical. |
| R4-F7 | The golden is keyed on kind × version but asserted per R7.7 fixture, and the ceiling corpus sits inside the byte-for-byte scope | Interface (Minor) | DE `:162`, `:163`, DF `:222` confirmed. | accept — a golden per fixture per kind per version; the ceiling corpus asserted on header and dialect only. |
| R4-F8 | E8/E14's "Once it's done it's final" will ship beside the P1 undo sentence once R6.3 lands; the marker set has no way to withdraw a sentence | Marketing (Minor) | copy `:23`–`:24`, header `:4` confirmed. | accept — an ‹until P1› marker, defined in the copy header, for a sentence withdrawn when the P1 rows land. |
| R4-F9 | "The readings you have now" reads as history at collection scale in the P0 build | PM (Minor), Privacy (Low) | copy `:23`–`:24` confirmed. | accept — "each swatch's current reading, not its earlier ones and not the scanning record". |
| R4-F10 | "Export first" leaves the delete in an unstated state | PM (Minor) | DF `:201`, DJ4 confirmed. | accept — the export leaves the delete unperformed and returns to the confirmation; DJ4 gains the branch. |
| R4-F11 | The salvage output (R5.5, E5) is a complete copy with no stated destination or lifecycle | Privacy (Low) | DF `:189`, copy `:22` confirmed. | accept — written where the user chooses, kept until removed (rule 6 on R5.5); E5 names the place. |

Every remaining Minor and Nit is accepted and mapped in the round-4 fix file; the fence-file wording nits (F28, F31, the export PRD's F3, F7, F9) are applied by the orchestrator in this commit. **Judgment calls recorded for the owner to overrule:** the state enum gains `superseded` rather than making the state the item's (R4-F6); the app version is normalized out of the byte comparison rather than moved out of every row (R4-F3); a ‹until P1› marker rather than a reworded P0 sentence (R4-F8); the fixtures' device snapshots are the simulated device's settable ones (privacy Info, device R6.9).

**Round-4 status:** one owner decision recorded as the F31 amendment (the export PRD's F9 amended to match). Fix file `prd-data-foundation-round-4-fixes.md` (both PRDs) to be verified item-for-item before dispatch. Round 5 is a delta over the 15 held rows and the rule-6 edits by the same seven lenses; three lenses already converged with no OBJECT, so round 5's brief asks each lens to confine itself to the delta.

## Round 5 — narrow delta over the round-4 fix pass (2026-09-10)

**Subject:** commit `4c40749` — the round-4 fix pass (16 boxes; DF 6,981 words, DE 3,368; 93 rows Aligned). **Lenses:** the same seven on Claude/Opus, confined to the 15 held rows and the four rule-6 edits, each with its round-4 report as a source. **Cross-model:** not repeated.

| lens | verdict | Blocker | Major | Minor/Nit | rows OBJECT |
|---|---|---|---|---|---|
| product manager | builds the right thing; both PRDs should close | 0 | 0 | 2 | 0 |
| staff engineer | ready — proceed | 0 | 0 | 6 | 0 |
| test (retargeted) | trustworthy after one clause | 0 | 1 | 4 | 1 |
| privacy | privacy-sound to ship; closes | 0 | 0 | 1 Low, 3 Info | 0 |
| marketing (copy) | lands and honest, one half-clause | 0 | 0 | 5 | 1 |
| architecture | sound — build it | 0 | 0 | 3 | 0 |
| interface | contract sound | 0 | 0 | 6 | 0 |

**Round-4 dispositions, per lens.** Every round-4 finding from all seven lenses is resolved on the text; marketing's N4-2 partial (E13's repeated phrase) is the one residue. Five lenses raised no OBJECT on any row; the four rule-6 edits keep alignment on every lens.

**Row flips.** Of the 15 held rows, every non-abstaining lens ALIGNs on 13; those untouched by this round's fix file flip to 🤝 Aligned — DF R5.7, R6.2, R7.2, R7.7, E8, E14 (6) and DE M1 (1). Held with an OBJECT: DF R1.2 (test), DE E1 (marketing). Held for an edit this round: DF M6, E13; DE R1.1, R1.3, R4.1, R4.3. Aligned rows edited under process rule 6 this round: DF R7.1, R3.3, R7.6b; DE R2.1, R4.4a.

### Verify-the-reviewer dispositions (the Major and the cross-lens Minors)

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R5-F1 | R1.2 promises imported columns under name and position at the floor and names R7.1 as owner; R7.1's read-back set has no imported column, so a normalized layout with an app-private order passes every green assertion | Test (Major) | DF `:131`, `:216` confirmed. | accept — R7.1's read-back set gains the imported columns under name and position (rule 6). |
| R5-F2 | The canonical export carries no measurement time; R1.3 adds measured-at "beside every canonical-export column" | Interface (Minor), Privacy (reads the omission as minimization) | DE `:123`, `:125` confirmed. | accept — owner: measured-at on every canonical row (the export PRD's F11). |
| R5-F3 | R1.1's "not exportable" carve-out names two of DF's four read-only states and no row asserts it | Staff (Minor), Arch (Minor), Test (Minor) | DE `:123`, `:164`; DF `:149`, `:189` confirmed. | accept — the carve-out names R5.5 and R2.2 too; R4.3 induces an export against such a file and asserts it is refused. |
| R5-F4 | The golden's refresh rule covers an append only, not a DERIVATION_VERSION bump; "well-formed" is undefined; the ceiling corpus's golden scope is open | Staff (Minor), Interface (Minor, Nit), Test (Minor) | DE `:162`; DF `:169` confirmed. | accept — one clause each on R4.1. |
| R5-F5 | R1.1 and R1.3 disagree on whether a never-scanned item gets a history row | Interface (Minor) | DE `:123`, `:125` confirmed. | accept — it contributes its identity row as in the canonical export (judgment call). |
| R5-F6 | The merged blank-row line carries two counts and the zero-count rule as written drops the whole sentence when one is zero, the common case | Marketing (Minor) | DE copy `:8`, `:16` confirmed. | accept — the copy headers say a zero count's phrase is dropped and the rest of the sentence stands. |
| R5-F7 | ‹until P1› is tied to "the P1 rows" as a phase; an interim build landing the undo before the history export shows "final" beside "undo" | PM (Minor) | DF copy `:4` confirmed. | accept — the marker is withdrawn when the ‹P1› sentence it stands in for appears. |
| R5-F8 | M6's condition multiplier has no number; R3.3's regeneration triggers omit a change of the chosen scan mode | Staff (Minor), Arch (Minor) | DF `:264`, `:169` vs capture `:139` confirmed. | accept — M6 pins "the chosen condition plus one"; R3.3 names the scan-mode change (rule 6). |

Every remaining Nit is accepted and mapped in the round-5 fix file. **Judgment calls recorded for the owner to overrule:** a never-scanned item's identity row in the history export (R5-F5); "well-formed" read as the app's released version string; the two copy-header rules (R5-F6, R5-F7).

**Round-5 status:** one owner decision recorded as the export PRD's F11. Fix file `prd-data-foundation-round-5-fixes.md` (both PRDs, 13 boxes) to be verified item-for-item before dispatch. Round 6 is the Phase 6 pre-lock round: the seven lenses delta the rows this fix touches and answer Phase 5's question (does anything deferred break the first usable build?), joined by the retargeted `peer-plan-reviewer` and two fresh lenses — `peer-standards-reviewer` on the export contract and `peer-reliability-reviewer` on the file's durability, migration and recovery rows — with the three mechanical lock preconditions run before the round and again after its fix pass.

## Round 6 — the pre-lock round (2026-09-14)

**Subject:** commit `54583a5` — after the round-5 fix pass, the mechanical lock checks and their editorial pass, the F21 raise to 7,500, and fence F32 (DF 7,305 words, DE 3,584; 100 rows Aligned, 8 held). **Lenses:** the seven standing lenses on Claude/Opus, each with its round-5 report; the retargeted `peer-plan-reviewer` ("could the follow-on spike and first build phase execute from these documents unattended"); and two fresh lenses per the fresh-lens rule — `peer-standards-reviewer` on the export contract and `peer-reliability-reviewer` on the file's durability, migration and recovery rows. Every lens answered Phase 5's question (does anything deferred to P1 break the first usable build) and named what it would stand on at lock. The staff lens's first run died on a connection error and was relaunched with the same brief. **Cross-model:** not repeated.

| lens | verdict | Blocker | Major | Minor/Nit | rows OBJECT | Phase 5 | clears at lock |
|---|---|---|---|---|---|---|---|
| product manager | builds the right thing | 0 | 0 | 3 | 0 | no break | yes |
| staff engineer | ready — proceed | 0 | 0 | 7 | 0 | no break | yes |
| test (retargeted) | trustworthy after two clauses | 0 | 2 | 8 | 2 | no break | after F6-T1, F6-T2 |
| privacy | privacy-sound to ship | 0 | 1 Medium | 3 Low, 2 Info | 0 | no break | yes |
| marketing (copy) | lands after two Majors | 0 | 2 | 1 | 2 | no break | DE yes; DF after F6-1, F6-2 |
| architecture | sound; two clauses | 0 | 2 | 4 | 2 | no break | after A6-1, A6-2 |
| interface | contract sound | 0 | 0 | 6 | 0 | no break | yes |
| plan (retargeted) | execute after two clauses | 0 | 3 | 4 | 3 | no break | after P6-1 (+ P6-2 before the golden task) |
| standards (fresh) | publish with these changes | 0 | 2 | 8 | 2 | no break | after SR-1, SR-2 |
| reliability (fresh) | resilient after fixing the Blocker | 1 | 3 | 4 | 6 | no break | after RL-1–RL-4 |

**Round-5 dispositions.** Every round-5 finding from all seven standing lenses is resolved on the text; none partial, none unresolved.

**Row flips.** The 8 held rows: every non-abstaining lens ALIGNs on DF R1.2, M6, E13 and DE E1, and this round's fix file does not change their meaning — they flip to 🤝 Aligned. DE R1.1 (RL-2), R1.3 (A6-1, SR-2), R4.1 (P6-2) draw OBJECTs and R4.3 is edited (S6-MN4, test) — held. Aligned rows drawing an OBJECT this round, edited under process rule 6 and re-verified in round 7: DF R7.1, R3.3, R6.5, R2.2, R5.5, R5.8, R7.2, R7.3, M9, E5, E15, E25; DE R2.2.

### Verify-the-reviewer dispositions (the Blocker, every Major, the cross-lens Minors)

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R6-F1 | A file opened read-only for two current readings (R2.2 → E5) has no route back to a writable file: the salvage copies both readings and the fresh file breaks the same invariant; R5.4's "never repairs unasked" implies a repair no row defines | Reliability (Blocker) | DF `:153`, `:193`, copy `:22`, `:222` confirmed — nothing says the salvage output resolves the conflict or opens read-write. | accept — R5.5: the salvage output resolves an invariant its source breaks (the later-recorded reading stays current, the other kept as correction-unconfirmed, the choice named) and opens read-write; R7.3 asserts it; E5 says so. The choice of which reading stays current is a judgment call the owner may overrule. |
| R6-F2 | Save-a-copy, the pre-upgrade snapshot and the salvage output are the only destination writes with no failure contract; an unfinished copy is listed as a way back | Reliability (Major) | DF `:188`–`:193`, copy `:35` confirmed. | accept — the move's contract on R5.8 and R5.5 (three named failures, nothing partial, an unfinished copy never listed), three states, R7.3 induces, R7.6m lists. |
| R6-F3 | R1.5's "a hold left by a dead process never blocks the owner" is induced by no test | Reliability (Major) | DF `:140`, `:221`–`:222` confirmed. | accept — R7.3's list gains it. |
| R6-F4 | DE R1.1's not-exportable carve-out cites R5.5 whole, whose second half is the quarantined-payload case the export must still serve | Reliability (Major) | DE `:124`, DF `:193` confirmed. | accept — the carve-out names R5.5's file-level half. |
| R6-F5 | After F11 the history export names measured-at twice, with no scope rule; a row with no current reading has a required column with no content | Arch (Major), Interface (Minor), Standards (Minor) | DE `:124`, `:126` confirmed. | accept — R1.3 drops the duplicate and scopes the column to the row's reading; R1.1 says empty on a no-current-reading row. |
| R6-F6 | R7.1's older-stamp enumeration can never be empty under R3.1's retention, so R7.5's completion assertion is unsatisfiable | Arch (Major) | DF `:171`, `:220`, `:224` confirmed. | accept — "every live value". |
| R6-F7 | R6.5's "never a release build" is asserted by nothing; a release build writing the interaction record passes every green row | Test (Major) | DF `:208` confirmed. | accept — R6.5's assertion clause covers the record's absence from a release build's file. |
| R6-F8 | R3.3's settings-change trigger is induced by no test and does not say whether it bumps the stamp | Test (Major) | DF `:173`, `:224` confirmed. | accept — the settings change regenerates under the same stamp; R7.5 induces it on the second-condition fixture. |
| R6-F9 | E25 is written for the sync class only while R1.7 and R7.2 name a network-volume class with different risks | Marketing (Major) | DF `:139`, `:221`, copy `:32` confirmed. | accept — a network-volume state; R7.6e lists both. |
| R6-F10 | E15 restates R1.10's whole-or-nothing guarantee without R1.10's local-volume scope | Marketing (Major) | copy `:25` vs DF `:141`, `:139` confirmed. | accept — a scope clause. |
| R6-F11 | R7.2 and M9 (P0) assert the P1 undo with no landing qualifier, unlike the nine other seams | Plan (Major), Test (Minor) | DF `:221`, `:275` confirmed. | accept — "once R6.3 lands". |
| R6-F12 | OQ 4 has a closer but no usable interim, so the P0 golden cannot be authored | Plan (Major) | DE `:209`, `:139`, `:163`; SDK audit `:30` confirmed. | accept — R4.1 gates the wavelength block until OQ 4 closes; OQ 4's interim says so. |
| R6-F13 | The dialect stops at the field boundary: no value-encoding rule, no timestamp format, no column dictionary a consumer can reach | Standards (Major), Interface (Minor) | DE `:138` confirmed. | accept — R2.2 gains a value-form sentence (reflectance as a fraction of 1; times RFC 3339 with an explicit offset; booleans true/false; a fixed precision the golden records) and the help docs a per-version column dictionary. Reflectance's scale is a judgment call. |
| R6-F14 | A reading both quarantined and superseded has two state values; an item whose current reading is quarantined must carry no current-reading flag | Standards (Major), Interface (Minor) | DE `:54`, `:126`; DF `:154` confirmed. | accept — quarantined takes precedence; no current flag on such an item's rows. |
| R6-F15 | Three hardware closers name no vehicle; the reference set has no source | Plan (Major, Minor) | DF `:289`, `:292`, DE `:209` confirmed. | accept — an outbound Device Management line adds the grid, the payload round-trip and the reference-set sourcing to that PRD's spike scope; the closers name it. |
| R6-F16 | R6.5's "never a release build" outruns capture R11.16's open OQ 24, recorded nowhere; what a release build does with a measuring build's file is unsaid | PM (Minor), Privacy (Medium), Arch (Minor), Staff (Minor) | DF `:208` vs capture `:396`, `:485` confirmed. | accept — an outbound Capture Mode line; R6.5 names OQ 24 and says a release build leaves the record untouched. |
| R6-F17 | The 21-row map files R11.16 under read-back and R6.7/R6.9 under R1.3; R6.9's comparison rule is carried by no DF row | PM, Arch, Privacy, Staff, Test (Minors) | DF `:242` confirmed. | accept — R11.16 → R6.5; R6.7 → R1.1; R6.9 named as carried by no DF row and handed to the Collection Mode line. |

Every remaining Minor and Nit is accepted and mapped in the round-6 fix file. **Judgment calls recorded for the owner to overrule:** the salvage conflict resolution (R6-F1); reflectance as a fraction of 1 (R6-F13); E8/E14 gain an ‹until P1› sentence pointing at save-a-copy (staff, reliability, PM nit — PM would leave it); a release build leaves a measuring build's interaction record untouched (R6-F16); a delete does not reach the session-scoped interaction record (privacy Low). Recorded as deliberate omissions: no collection-identifier column in the export (standards nit).

**Round-6 status:** one owner decision — F21 raised to 8,000 for the fresh lenses' rules. Fix file `prd-data-foundation-round-6-fixes.md` (both PRDs). Round 7 is the closing delta: the six lenses that objected re-verify their rows, and the three fresh or retargeted lenses (plan, standards, reliability) confirm proceed, which Phase 6 requires; the three mechanical checks re-run after the round-7 fix against the state that locks.

## Round 7 — the closing delta (2026-09-15)

**Subject:** commit `cc16f86` — after the round-6 fix pass (DF 7,815 words, DE 3,913; 104 rows Aligned, 8 held: DE R1.1, R1.3, R4.1, R4.3 and DF E27–E30). **Lenses:** all ten of round 6 on Claude/Opus, each with its round-6 report and the round-6 fix file — the seven standing lenses, the retargeted `peer-plan-reviewer`, and the two fresh lenses (`peer-standards-reviewer`, `peer-reliability-reviewer`) confirming proceed as Phase 6 requires. Every lens answered per-finding RESOLVED/UNRESOLVED for round 6 and named what it would stand on at lock. **Cross-model:** not repeated.

| lens | verdict | Blocker | Major | Minor/Nit | rows OBJECT | clears at lock |
|---|---|---|---|---|---|---|
| product manager | DE clears; DF after one header clause | 0 | 1 | 3 | 1 (E8) | after F7-1 |
| staff engineer | proceed, both documents | 0 | 0 | 5 | 0 | yes |
| test (retargeted) | DE clears; DF after one clause | 0 | 1 | 9 | 1 (R6.2) | after F7-T1 |
| privacy | privacy-sound to ship | 0 | 1 Medium | 2 Low, 2 Info | 0 | yes |
| marketing (copy) | lands, both documents | 0 | 0 | 7 | 0 | yes |
| architecture | sound — lock it | 0 | 0 | 6 | 0 | yes |
| interface | contract sound | 0 | 0 | 6 | 0 | yes |
| plan (retargeted) | executes unattended — confirms proceed | 0 | 0 | 5 | 0 | yes |
| standards (fresh) | publish — confirms proceed | 0 | 0 | 7 | 0 | yes |
| reliability (fresh) | resilient — confirms proceed | 0 | 0 | 4 | 0 | yes |

**Round-6 dispositions.** Every round-6 finding from all ten lenses is resolved on the text, with two residues the raising lens owns: the test lens marks F6-T2 partial (the stamp is settled; only the scan-mode trigger is induced — F7-T2 below) and carries its round-6 Nit on R7.1's twice-imported order unresolved and still a Nit. The standards lens confirms a second implementer can now write a conforming parser from the PRD, the golden and the help-docs dictionary; the plan lens confirms the spike and the first build phase execute unattended, with one proceed condition outside these documents (PL7-4: the device PRD's spike scope amendment lands before the spike is dispatched); the reliability lens confirms every recovery path has a route back.

**Row flips.** Every non-abstaining lens ALIGNs on DE R1.1, R1.3, R4.3 and DF E27, and this round's fix file does not change their meaning — they flip to 🤝 Aligned. DE R4.1 draws no OBJECT but is edited this round (FX7-13, FX7-15, FX7-16) — held. DF E28–E30 draw no OBJECT but their wording is edited (FX7-18) — held. DF R6.2 (test F7-T1) and E8 (PM F7-1) draw OBJECTs — held. Aligned rows edited under process rule 6 and re-verified in round 8: DF R1.7, R5.1, R6.5, R7.1, R7.2, R7.3, R7.5, R7.7, R7.6b, R7.6d, R7.6e, R7.6m, E5, E14, E23, E25, both obligations tables; DE R2.2, R2.4, R2.5, R4.2, its outbound line.

### Verify-the-reviewer dispositions (both Majors, the cross-lens Minors, the owner forks)

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R7-F1 | R6.2's export-first offer — which leaves the delete unperformed — is asserted by nothing; wiring it as export-then-delete passes R7.2, R7.6k/l, M9 and DE R4.3 green | Test (Major); PM (Minor) and Interface (Nit) on R6.2's "or a complete copy" reading as a second action | "export first" occurs once in the DF body (`:205`); R7.2 `:221` asserts counts, post-delete read-back and the undo only; R7.6k/l `:115`–`:116` list actions. Confirmed. | accept — R7.2's delete sentence asserts that taking the export-first action leaves the item and its history present with the confirmation still open; R6.2 reads "the complete copy the state points at". |
| R7-F2 | The copy header's zero-count rule drops E8's single-count sentence for an item with one reading — the first delete any user performs never says the reading goes | PM (Major); Test (Nit) on the ‹P1› sentence dropping "not the scanning record" | copy `:6` (rule scoped to two-count sentences), `:23` confirmed; the owner ruled the principle at R5-F6. | accept — the rule is made phrase-level in both copy headers; E8/E14's ‹P1› sentence keeps "still not the scanning record". |
| R7-F3 | R1.7 enumerates the sync folder's risk pair for both volume classes; E27 rightly names different ones | Test, Standards, Architecture, Staff, Marketing, Privacy (Medium), PM (Nit), Interface (Nit: a path in both classes) | DF `:139`, copy `:33`, `:109` confirmed. | accept — R1.7 names that class's two risks, the provider copy scoped to the sync folder; a path in both classes shows the network state; R7.6e's cell names them. Privacy's disclosure line on E27 is owner-rejected (fence file). |
| R7-F4 | A same-stamp regeneration (illuminant, observer, scan mode) has no findability predicate and no interrupt assertion; the stamp-bump run has both | Architecture, Staff, Reliability, Test (Minors) | DF `:173`, `:220`, `:224` confirmed. | accept — R7.1's enumeration also names every live value carrying an illuminant, observer or condition other than its collection's; R7.5 changes the illuminant and the scan mode, interrupts and resumes, and asserts the absent mark. |
| R7-F5 | Value form is now contract (R2.2) but R2.5's bump list omits it and R4.1 refreshes a value-rendering change in place | Standards, Architecture (Minors) | DE `:138`, `:141`, `:163` confirmed. | accept — owner: a form change bumps the version (fence F12); R4.1's in-place list drops it. |
| R7-F6 | R2.2's value-form sentence is unscoped, so an imported `0007` or `1,234.5` reads as normalised on export | Interface (Minor) | DE `:138`; import PRD R2.2/R2.3 confirmed — no row pins a passthrough value. | accept — owner: app columns only; a passthrough value is emitted as stored (fence F13). |
| R7-F7 | The obligations tables no longer mirror at R7.3, and both name two of R1.1's four not-exportable states; the Capture inbound Rows cell trails the 21-row map | Staff, Architecture (×2), Standards, Test (Minors) | DF `:237`, `:239`, DE `:185` confirmed. | accept — both lines read "R1.1's not-exportable states, which R7.2 declares and R7.3 induces"; R7.3 and R3.2, R6.5, R7.1 join the Rows cells; DE's fixture list carries the ceiling corpus's second condition. |
| R7-F8 | E28–E30's "nothing new to go back to" is the backup reader's sentence on the salvage path; the states are listed on no file-opening surface; E5's settlement list and E12's version-back are on no surface cell | PM, Marketing, Reliability, Architecture, Standards, Test, Staff (Minors/Nits) | copy `:37`–`:39`, DF `:106`, `:117` confirmed. | accept — the clause is dropped, path-neutral; R7.6b lists E28–E30 and what the salvage had to settle; R7.6m lists the version each snapshot goes back to. |
| R7-F9 | R5.5's salvage promise is universally quantified with one rule given | Standards (Minor); Architecture reads the generality as correct | DF `:193` confirmed. | owner-rejected — the general promise stands, each choice named (E5); recorded in the fence file. |
| R7-F10 | R4.2 asserts a "full column set" R4.1's OQ-4 gate forbids the golden from carrying | Standards, Interface (Minor/Nit) | DE `:163`–`:164` confirmed. | accept — "the column set as its golden carries it". |
| R7-F11 | The help docs are a third published description of the columns and the errata clause ranks two | Interface (Minor) | DE `:138`, `:163` confirmed. | accept — "this document or the help docs". |
| R7-F12 | DJ3 still says "byte for byte" and states the disk-full promise without R1.10's local scope | Marketing (Minors) | journeys `:44`, `:54` confirmed. | accept — journeys sweep. |
| R7-F13 | R7.5's reference set is spike-gated with no interim clause; OQ 6's interim cell is circular; the reflectance precision is golden-fixed by a golden that omits it | Plan (Minors) | DF `:224`, `:289`; DE `:138`, `:163` confirmed. | accept — an interim clause on R7.5, OQ 6's interim names the gap, R4.1 fixes the reflectance precision when the golden is re-cut. |
| R7-F14 | The user-requested copy is not confirmed openable; E23's way back is a future release | Reliability (Minors) | DF `:188`, copy `:20` vs `:16` confirmed. | accept — R5.1 confirms both; E23 gains E16's clause. |
| R7-F15 | R6.5's release-build assertion declares no build axis; "a release build's file" invites a fixture that fails for behaving as required | Test (Minor), Staff (Nit) | DF `:208` confirmed. | accept — "a file a release build wrote, the run recording its build configuration". |

Every remaining Minor and Nit is accepted and mapped in the round-7 fix file, except these, declined with the reason: SR7-7 (a precedence half-clause on DE R1.3 — the Vocabulary is normative and the words are better spent); the test lens's twice-imported Nit on R7.1 (DE R2.3 plus the golden discriminate it); the interface lens's E25 whole-or-nothing sentence (R1.7 puts the safe practice in the help docs; the state names the two risks that matter at the moment of choice); the architecture lens's Nits (a) and (b) (no assertion depends on them; E11's question is right for a salvaged pair). **Post-lock list additions:** the dogfood build's participant-facing onboarding owes one line that the interaction record is kept and a delete does not reach it (privacy Low); the device PRD's spike scope amendment lands before the spike is dispatched (plan PL7-4); DE's 87-word headroom is the errata rule's working room (plan PL7-5). **Judgment calls recorded for the owner to overrule:** a path in both volume classes shows the network state (R7-F3); the reflectance precision is fixed when the golden is re-cut rather than named now (R7-F13); E28–E30 lose the "way back" clause rather than gaining a salvage variant (R7-F8).

**Round-7 status:** two owner decisions (DE fences F12, F13) and two owner rejections (DF fence file). Fix file `prd-data-foundation-round-7-fixes.md` (both PRDs). Round 8 is a narrow delta over the held and edited rows: the test lens on R6.2/R7.2, the PM lens on E8 and the header rule, the marketing lens on the copy and journeys edits, and each lens whose clause landed re-verifying its rows; the three mechanical checks re-run after the round-8 fix, if any, against the state that locks.

## Round 8 — narrow delta over the round-7 fix (2026-09-15)

**Subject:** commit `5f428e6` — after the round-7 fix pass (DF 7,922 words of 8,000, DE 3,942 of 4,000; held: DE R4.1, DF R6.2, E8, E28–E30). **Lenses:** the same ten on Claude/Opus, each briefed on the held rows and the rows edited under process rule 6 in round 7 only, answering RESOLVED/PARTIAL/UNRESOLVED on its own round-7 findings. **Cross-model:** not repeated.

| lens | verdict | Blocker | Major | Minor/Nit | rows OBJECT | stands on |
|---|---|---|---|---|---|---|
| product manager | DE clears; DF after one header clause | 0 | 1 | 1 | E8, headers | PM-F8-1 |
| staff engineer | proceed | 0 | 0 | 6 | R7.1, §8, EJ1 | SE8-1 |
| test (retargeted) | trustworthy after two clauses | 0 | 2 | 5 | R7.1, R7.7 | F8-T1, F8-T2 |
| privacy | privacy-sound to ship | 0 | 0 | 1 Low, 2 Info | 0 | nothing |
| marketing (copy) | lands after one header clause | 0 | 1 | 2 | E8, E14, R7.6b, headers | MK8-1 |
| architecture | sound — lock after two | 0 | 2 | 3 | R7.1, E8, headers | A8-1 |
| interface | sound after one Major | 0 | 1 | 6 | R2.4, R7.7, E8, R6.2, R1.7, E28 | R8-IF-1, R8-IF-2 |
| plan (retargeted) | execute after PL8-1; proceed holds | 0 | 1 | 5 | R7.1, R7.5, E8, E23, R2.2, headers | PL8-1, PL7-4 |
| standards (fresh) | publish with these changes | 0 | 1 | 6 | R7.1, R2.2, R2.4, §8, EJ1, headers | SR8-1, SR8-2, SR8-3 |
| reliability (fresh) | resilient — proceed | 0 | 0 | 5 | 0 | nothing |

**Round-7 dispositions.** Every round-7 finding is resolved on the text except three the raising lens marks partial, each carried below: the same-stamp findability predicate (test F7-T2, staff S7-MN3, architecture A7-2 → R8-F1), the passthrough value form (interface R7-IF-1 → R8-F2), and R6.2's substitution reading (interface R7-IF-4 → R8-F15). Plan corrects the log: DE's headroom is 58 words after the errata rule landed, not 87.

**Row flips.** DF E29 and E30: every non-abstaining lens ALIGNs and this round's fix file does not touch them — they flip to 🤝 Aligned. Held, with the OBJECT that holds each: DF R1.7 (R8-IF-6, a Nit the lens releases — held mechanically for round 9), R6.2 (R8-IF-5), R7.1 (F8-T1, A8-1, SE8-1, PL8-2, SR8-2), R7.5 (PL8-1), R7.7 (F8-T2, R8-IF-2), R7.6b (MK8-2), E8 (PM-F8-1, MK8-1, A8-2, R8-IF-4, PL8-4), E14 (MK8-1), E23 (PL8-5), E28 (R8-IF-7); DE R2.2 (PL8-3, SR8-4), R2.4 (R8-IF-1, SR8-1), R4.1 (edited FX8-10). Aligned rows edited under process rule 6 and re-verified in round 9: DF R5.7, R7.2, R7.3, R7.6d, §8's two-surface note, DJ3, both copy headers; DE's outbound Data Foundation line, EJ1.

### Verify-the-reviewer dispositions (every Major, the cross-lens Minors)

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R8-F1 | R7.1's new limb — "carrying an illuminant, observer or condition other than its collection's" — permanently enumerates every kept non-chosen condition's set (R3.1, F31) and every non-spectral reading (R3.3), so a same-stamp regeneration can never be certified complete; R6-F6's shape on the condition axis | Test, Architecture (Majors); Staff, Standards, Plan (Minors) | DF `:171`, `:173`, `:220`, `:226`; fence F31 confirmed. | accept — the limb becomes "and every spectral reading whose working set is on an illuminant, observer or condition other than its collection's", which is empty on a conforming file and non-empty on a half-finished scan-mode change. |
| R8-F2 | "a passthrough column's value emitted as stored" binds nothing: no row pins what is stored to what the import file gave (DF R1.2 pins name and position; DE R1.1 pins identity values only) | Interface, Standards (Majors) | DF `:135`, DE `:124`, `:140`; import PRD R2.2/R2.3 confirmed. | accept — "exactly as its import file gave it". |
| R8-F3 | Fence F13 has no fixture: R7.7 pins names and structure, never a value the dialect would reshape, so a normalising build passes every golden | Test (Major); Interface, Standards (Minors) | DF `:226`, DE `:185` confirmed. | accept — R7.7 and DE's mirror gain "an imported value a parser would reshape". |
| R8-F4 | The phrase-level zero-count rule does not say where a phrase ends, drops nothing that depends on it (E4's "Use the previous reading", E12's dangling clause, DE E1's bullets), and cannot reach an uncounted phrase naming what a never-scanned item lacks (E8's "the reading it has now") | PM, Marketing, Architecture (Majors); Interface, Standards, Plan (Minors); Staff (Nit) | copy `:6`, `:21`, `:23`, `:36`; DE copy `:8`, `:16` confirmed. | accept — one master clause in both headers: the phrase or clause a zero count or an absent thing governs is dropped with the conjunction or punctuation that joined it; a sentence, list line or action that exists only for it, or would not stand without it, is left out. |
| R8-F5 | R7.5's "interrupting and resuming that run the same way" points at a completion predicate (R7.1's enumeration empty) that R8-F1 made unsatisfiable; an unattended agent writes a test that can never go green | Plan (Major) | DF `:224` confirmed. | accept — the clause asserts completion explicitly against R7.1's mismatch enumeration, which R8-F1's fix makes empty on completion. |
| R8-F6 | R2.2's "every app column's value" sweeps in identity columns, which R1.1 says are written as entered | Plan (Minor) | DE `:124`, `:138`, `:139` confirmed. | accept — "every app column's value but identity's". |
| R8-F7 | "RFC 3339 with an explicit UTC offset" admits a local offset, so one history file can mix offsets and sort wrong | Standards (Minor) | DE `:138` confirmed. | accept — times in UTC, the offset written as `Z`. A judgment call the owner may overrule. |
| R8-F8 | E23's new "last version that can still change it" sentence is carried by R5.7, which scopes it to the below-floor state; R5.2 (E23's row) does not promise it | Test, Plan (Minors); Architecture (Nit) | DF `:187`, `:190`, copy `:20` confirmed. | accept — R5.7's second sentence names R5.2's refusal too. |
| R8-F9 | R7.2's export-first clause asserts nothing was destroyed but not that an export happened, and names the item only, not the collection scale | Test (Minor) | DF `:221` confirmed. | accept — "runs the export and leaves what the confirmation names present". |
| R8-F10 | R7.3 induces the three named copy/snapshot/salvage failures but not a crash mid-artifact, the one path that leaves a truncated snapshot for R7.6m's list | Reliability (Minor) | DF `:189`, `:222`, `:117` confirmed. | accept — the induced list gains a crash during a copy, a snapshot or a salvage. |
| R8-F11 | R4.1's in-place refresh on a DERIVATION_VERSION bump rewrites every colour cell, so a form change riding on it is camouflaged in the review diff | Reliability (Minor) | DE `:163` confirmed. | accept — the diff is confined to the derived-value and derivation-version columns. |
| R8-F12 | §8's "E4, E10 and E11 are each listed on two surfaces" is stale: E28–E30 now sit on R7.6b and R7.6m | Marketing, Test, Staff, Reliability, Architecture, Standards | DF `:263`, `:106`, `:117` confirmed. | accept — editorial. |
| R8-F13 | EJ1 step 12 narrates R2.5's bump list without the form trigger F12 added | Privacy, Test, Staff, Interface, Reliability, Standards | DE journeys `:23` confirmed. | accept — companion edit. |
| R8-F14 | E28's headline still names a copy on a state the salvage path also reaches | Staff, Interface, Reliability (Nits) | copy `:37` confirmed. | accept — "There isn't room to finish that". |
| R8-F15 | R6.2's "or, until its history option lands, the complete copy" reads as the copy replacing the canonical export before P1 | Interface (Minor) | DF `:205` vs copy `:23` confirmed. | accept — word-neutral: the state also points at the complete copy until the history option lands. |

Remaining Minors and Nits accepted and mapped in the round-8 fix file: R7.6d's "in E25's and E27's words" (test Nit — "in the vocabulary E25 and E27 use"); DJ3's second disk-full sentence unscoped (architecture Nit). Declined with the reason: R8-IF-6 (R1.7's second network risk sits in its next sentence and E27 — the lens releases it); PL8-6 (M2/M7's population cells — the Legend's provisional-constant rule and R7.5's interim clause cover it); the test lens's note that R6.5's "left untouched" clause is asserted by nothing (accepted by the lens as not worth the words); staff SE8-6 (the both-classes path — the owner's recorded judgment call). **Post-lock list additions:** the help-docs column dictionary carries a formula-injection note for spreadsheet consumers (privacy Low); nothing gates a release on that dictionary existing (reliability); the R7.7 fixture list and DE's mirror of it will drift on the next fixture added (architecture, standards). **Judgment calls recorded for the owner to overrule:** times in UTC with `Z` (R8-F7); E28's headline wording (R8-F14). Plan's proceed condition outside these documents stands: the device PRD's spike-scope amendment lands before the spike is dispatched (PL7-4).

**Round-8 status:** no owner decision needed; every fix is inside the fences (F12, F13 sharpened, not changed). Fix file `prd-data-foundation-round-8-fixes.md` (both PRDs). Round 9 is a narrow delta over the thirteen held rows and the rule-6 edits, by the lenses that objected; the three mechanical checks re-run after it against the state that locks.

## Round 9 — the closing delta after round 8 (2026-09-15)

**Subject:** commit `ff8743c` — after the round-8 fix pass and the second lock-check editorial pass (DF 7,999 words of 8,000, DE 3,997 of 4,000; held: DF R1.7, R6.2, R7.1, R7.5, R7.7, R7.6b, E8, E14, E23, E28; DE R2.2, R2.4, R4.1). **Lenses:** the same ten on Claude/Opus, briefed on the held rows and the round-8 rule-6 edits, and told this is the round Phase 6 locks on. **Cross-model:** not repeated.

| lens | verdict | Blocker | Major | Minor/Nit | rows OBJECT | stands on |
|---|---|---|---|---|---|---|
| product manager | builds the right thing — both clear | 0 | 0 | 6 | E8, E14, R7.6h, R2.4, DE E1, headers | nothing |
| staff engineer | proceed after one Major | 0 | 1 | 5 | R7.7, R7.1, R7.5, R5.7, E23, R4.1 | SE9-1 |
| test (retargeted) | trustworthy after one clause | 0 | 1 | 7 | R1.2, R7.1, R7.3, R7.6k/l, R2.4, R4.1 | F9-T1 |
| privacy | privacy-sound to ship — lock | 0 | 0 | 3 Info | 0 | nothing |
| marketing (copy) | lands and honest | 0 | 1 (README) | 3 | 0 | nothing |
| architecture | build after one Major | 0 | 1 | 4 | R7.7, E14, R2.4, R4.1, headers | A9-2 |
| interface | sound after two Majors | 0 | 2 | 5 | E23, R2.4, R4.1 | R9-IF-1, R9-IF-2 |
| plan (retargeted) | ready to lock after two clauses; proceed holds | 0 | 0 | 6 | R7.1, E23, R4.1 | PL9-2 (+ PL7-4 outside) |
| standards (fresh) | publish with these changes | 0 | 2 | 4 | R7.1, Vocabulary, E23, R4.1, headers | SR9-1, SR9-2 |
| reliability (fresh) | resilient — proceed | 0 | 0 | 3 | R5.7, E23, R4.1 | nothing |

**Round-8 dispositions.** Every round-8 finding is resolved on the text except four the raising lens marks partial, each carried below: the passthrough value's chain of custody (test F8-T2, interface R8-IF-1 → R9-F1), R7.1's predicate on the absent-set case (staff SE8-1, standards SR8-2 → R9-F3), R4.1's confinement reaching the append (reliability RL8-2 → R9-F2), E23's version triple (plan PL8-5 → R9-F6). The plan lens confirms proceed for the spike and the first build phase, with PL7-4 (the device PRD's spike-scope amendment before dispatch) as the one condition outside these documents.

**Row flips.** Every non-abstaining lens ALIGNs on DF R1.7, R6.2, R7.6b, E28 and DE R2.2, and this round's fix file does not change their meaning — they flip to 🤝 Aligned (E28's State-cell label edit is not copy). Held for round 10, with the OBJECT or edit that holds each: DF R1.2 (F9-T1, edited), R7.1 (edited), R7.5 (SE9-4), R7.7 (edited), R5.7 (RL9-2, SE9-3), R7.6h (PM-F9-4, edited), E8, E14, E23, E16 (edited copy); DE R2.4, R4.1 (edited), E1 (edited); both copy headers (edited); the Vocabulary's chosen-condition entry (edited).

### Verify-the-reviewer dispositions (every Major, the cross-lens Minors)

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R9-F1 | Fence F13's promise has no chain of custody: R2.4 now says "exactly as its import file gave it" but R1.2 and R7.1 pin an imported column's name and position only, so an importer that normalises `0007` passes every row and golden; and, the other way, "as its import file gave it" over-binds once the user edits the value (R2.3, F17) or re-imports under "keep" (import R3.5) | Test, Architecture (Majors); Interface (Major, the over-binding half); Standards, Plan, PM (Minors) | DF `:135`, `:220`; DE `:140`; fence F13 `:85` ("exactly as stored"); import PRD R2.2/R2.3/R3.5 confirmed. | accept, reconciled — R1.2 pins the stored value to the import file's until the user changes it (R2.3); R7.1 reads it back; R2.4 returns to the fence's own words, "exactly as stored", nothing reshaping it on the way out. Net word-negative. |
| R9-F2 | R4.1's confinement clause attaches to both in-place triggers, so a legitimate append's diff (a new column) violates it | Standards, Interface (Majors); Reliability, Test, Architecture, Staff, Plan (Minors) | DE `:163` confirmed. | accept — "a bump's diff confined to…". |
| R9-F3 | "working set" is load-bearing (R7.5 hangs its oracle on it) and defined nowhere; a spectral reading with no set in the new chosen condition is decidable only under one reading | Standards (Major); Test, Staff, Plan (Minors); Interface, Architecture (Nits) | DF `:76`, `:220`, `:224` confirmed. | accept — the Vocabulary binds "working set" to the chosen condition's set; R7.1 reads "carrying a working set on…", so a reading with none is plainly outside. |
| R9-F4 | R7.7 says every fixture is checked in; OQ 5 sizes the ROWS_CEILING corpus at about 1.2 GB raw in a public repo with no LFS | Staff (Major) | DF `:226`, `:288`; no `.gitattributes` confirmed. | accept — "a generated corpus". A judgment call the owner may overrule; the generator's determinism contract goes to ADR-0003 (post-lock). |
| R9-F5 | The repository README claims colours "outside your monitor's gamut" are marked; R3.4 fixes the flag to sRGB regardless of display | Marketing (Major) | `README.md:12` vs DF `:174` confirmed. | accept — the README line reads "outside standard sRGB", with the lock pass's README edits. |
| R9-F6 | R5.7 now promises the app's own version of E23 (and E16); neither state names it | Reliability, Staff, Standards, Plan, Interface (Minors) | DF `:187`, copy `:16`, `:20` confirmed. | accept — both states gain "and this is version ⟨version⟩" in E1's form; E23's three ways forward become one sentence (marketing's released Nit). |
| R9-F7 | For a never-scanned item the master clause drops E8's whole item sentence, so the confirmation names nothing; the header's absent-thing limb is item-scoped and misses collection- and export-scale phrases | PM (Minor, Nit); Marketing, Standards, Architecture, Interface (Nits/Minor) | copy `:6`, `:23`, `:24`; DE copy `:8`, `:16` confirmed. | accept — E8/E14 gain a clause that survives at zero ("the swatch and everything you imported with it"); both headers read "something that is not there"; DE E1's trailing clause reads "which it is". |
| R9-F8 | R7.3's crash induction has no oracle attached: the state assertion cannot see a crash and the nothing-partial clause is scoped to the three named failures | Test (Minor); Reliability, Architecture, Plan (Nits) | DF `:222` confirmed. | accept — the crash moves into the nothing-partial assertion. |
| R9-F9 | R7.6h's "both ways back" contradicts the header rule once a quarantined reading is an item's only one | PM (Nit), Interface (Minor, released) | DF `:112`, copy `:21` confirmed. | accept — "each way back". |
| R9-F10 | E28–E30's State labels still say "Copy didn't finish" on states the snapshot and salvage paths reach | Marketing, Standards, Interface, Reliability (Nits) | copy `:37`–`:39` confirmed. | accept — labels, not shipped copy. |
| R9-F11 | EJ1 says an appended column lands "at the end"; R2.5 puts it at the end of the app block | Plan, Interface (Nits) | DE journeys `:23` confirmed. | accept — companion edit. |
| R9-F12 | R7.6d's trailing "in the vocabulary the sync and network warnings use" adds nothing and reads as a wording match | Test (Nit) | DF `:108` confirmed. | accept — dropped; it funds the round's DF words. |

Declined with the reason, both released by the raising lens and neither fitting DF's headroom after the edits above: F9-T3 (R7.6k asserting the delete counts on an item with no earlier readings — R7.7's never-scanned fixture and the copy header's rule govern it; on the post-lock list as a build-review item); F9-T6 (R7.2's declared context listing "or both" for a path in two classes — R7.6e's clause names the case). Also declined: PM-F9-6's rewording of the ‹P1› sentence (taken: "as well as the current one" is the same fix — see the fix file). **Post-lock list additions:** a CSV → store → CSV round-trip case for the passthrough value (PM, Architecture, Test); the generated corpus's determinism contract (Staff → ADR-0003); E8's never-scanned rendering to be checked at build review (PM). **Judgment calls recorded for the owner to overrule:** the ceiling corpus is generated rather than checked in (R9-F4); R2.4 returns to "exactly as stored" with the storage obligation on R1.2 (R9-F1).

**Round-9 status:** one owner decision after the first fix pass (2026-09-16): the pass blocked five items on the word budget because status cells and rule-6 notes are counted text (275 DF words, 90 DE); the owner kept the budgets and the count method and chose to trim meaning-neutral prose to fit (fix-file item FX9-19), over excluding the bookkeeping columns or raising F21 and DE F1. Every fix stays inside the fences. Fix file `prd-data-foundation-round-9-fixes.md` (both PRDs). Round 10 is the narrow delta over the rows this pass edits and the held rows, by every lens that objected; the three mechanical checks re-run after it against the state that locks.

## Round 10 — the final narrow delta before lock (2026-09-16)

**Subject:** commit `894d085` — after the round-9 fix pass in two parts (DF 7,991 words of 8,000, DE 3,991 of 4,000; held: DF R1.2, R7.1, R7.5, R7.7, R5.7, E8, E14, E16, E23; DE R2.4, R4.1, E1; both copy headers; the Vocabulary's chosen-condition entry). **Lenses:** the same ten on Claude/Opus, briefed as the last round before lock over the held rows, the round-9 rule-6 edits and the nine trims. **Cross-model:** not repeated.

| lens | verdict | Blocker | Major | Minor/Nit | rows OBJECT | stands on |
|---|---|---|---|---|---|---|
| product manager | builds the right thing — both clear | 0 | 0 | 4 | 0 | nothing |
| staff engineer | proceed to lock | 0 | 0 | 8 | R1.2, R7.7, Vocabulary, OQ 2, R2.4, R4.1, EJ1 | nothing |
| test (retargeted) | trustworthy | 0 | 0 | 4 | R5.7, R7.6b | nothing |
| privacy | privacy-sound to ship — lock | 0 | 0 | 1 Low, 3 Info | R7.2 | nothing |
| marketing (copy) | lands and honest | 0 | 0 | 2 | 0 | nothing |
| architecture | sound — lock it | 0 | 0 | 3 | R7.1, R7.7 | nothing |
| interface | contract sound | 0 | 0 | 4 | R1.2, R4.1, EJ1 | nothing |
| plan (retargeted) | ready to lock; proceed holds | 0 | 0 | 3 | 0 | nothing (PL7-4 outside) |
| standards (fresh) | publish with these changes | 0 | 1 | 6 | R7.1, R7.5, Vocabulary, R2.4 | SR10-1 (word-negative) |
| reliability (fresh) | resilient — proceed | 0 | 1 | 0 | R7.1 | RL10-1, or post-lock with R7.5 as the interim |

**Round-9 dispositions.** Every round-9 finding from all ten lenses is resolved on the text; the three marked partial are carried below (staff SE9-1's corpus still inside the checked-in list → R10-F3; standards SR9-1's working-set definition → R10-F2; standards SR9-5's fence F13 rationale → R10-F6). Every lens says plainly it stands on nothing that would make either document unsafe to hand to engineering; the two Majors are one row, and the plan lens confirms proceed for the spike and the first build phase with PL7-4 as the standing condition outside these documents.

**Row flips.** Every non-abstaining lens ALIGNs on DF E8, E14, E16, E23, both copy headers, and DE E1 — they flip to 🤝 Aligned. Held for the closing delta (round 11), with the OBJECT or edit that holds each: DF R1.2 (SE10-2, IF10-1; edited), R7.1 (SR10-1, RL10-1, A10-2; edited), R7.5 (SR10-2; the fix lands on the Vocabulary), R7.7 (SE10-3, SE10-4, A10-1; edited), the Vocabulary's chosen-condition entry (SR10-2, SE10-6; edited); DE R2.4 (SE10-1, SR10-3; edited), R4.1 (IF10-2, SE10-5; edited), EJ1 (IF10-3, SE10-7; edited). Rows drawing an OBJECT the lens released as not standing, with the edit declined for budget and the item recorded post-lock, go to the owner for an overrule on the record at lock: DF R5.7 and R7.6b (test T10-1), R7.2 (privacy P10-1), OQ 2's Closer (staff SE10-8).

### Verify-the-reviewer dispositions (both Majors, the cross-lens Minors)

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R10-F1 | R7.1's read-back still says "the value its import file gave it" — the over-binding round 9 removed from R2.4 and qualified in R1.2 — so a store that keeps an imported value immutable is the only one under which the assertion holds unconditionally; and a reading with no set in the newly chosen condition and no absent mark is a straggler nothing enumerates on a real file | Standards (Major), Reliability (Major); Architecture, Interface, Plan, Test (Minors/Nits on the same limb) | DF `:135`, `:220`, `:224`, `:175` confirmed. | accept — R7.1 reads back "the value R1.2 keeps" and its mismatch limb becomes "carrying a working set stamped with an illuminant, observer or condition other than its collection's, or none and no absent mark". Net +4 words. |
| R10-F2 | The Vocabulary defines the working set as the chosen condition's set, which read literally makes the condition axis of R7.1's enumeration empty by definition | Standards (Minor), Architecture (Minor), Staff (Nit), Plan (Nit), Test (Nit) | DF `:76` confirmed. | accept — "a reading's working set is the set the app works it from". |
| R10-F3 | R7.7's generated corpus still sits grammatically inside "a fixture file is checked in … the set together holding", and the Vocabulary's Fixture entry says checked in | Staff (Minor), Architecture (Minor), Test (Nit) | DF `:77`, `:226`; OQ 5 `:288` confirmed. | accept — the corpus becomes its own clause after the list (word-negative); the Fixture entry is left as the per-version files it defines. |
| R10-F4 | R1.2's stored-value clause names the user's edit as the only change path; a re-import under the import PRD's R3.5 overwrites by default | Staff (Minor) | DF `:135`; import PRD R3.5 `:148` confirmed. | accept — the cite gains the import PRD's R3.5. |
| R10-F5 | R2.4's "nothing here reshaping it" read literally forbids RFC 4180 quoting of a passthrough field | Staff (Minor) | DE `:138`, `:140` confirmed. | accept — "nothing but R2.2's quoting reshaping it". |
| R10-F6 | Fence F13's Why still says "comes back out as it went in", the formulation round 9 reconciled away; R4.1's "a bump's confined" elides its noun; EJ1 step 11 keeps "which of the two it is" | Standards, Interface, Staff, Plan, Privacy (Minors/Nits) | DE fences `:87`; DE `:163`; DE journeys `:22` confirmed. | accept — all three, none inside a budget except the one-word "diff". |
| R10-F7 | The Inventory Import outbound line hands over positions only now that R1.2 also pins values | Plan (Nit) | DF `:258` confirmed. | accept — "positions and values". |
| R10-F8 | R5.7's version triple is asserted by no surface cell; R7.2's clear-beyond-recovery oracle names a note but not an imported value; no fixture carries `sc_simulated` false; a Collection Mode rename's effect on a stored column name is unsaid; OQ 2's ADR-0006 gloss was trimmed | Test (Minor), Privacy (Low), Staff (Minor), Interface (Minor), Staff/PM/Marketing/Architecture (Nits) | DF `:106`, `:221`, `:226`, `:135`, `:285`; `docs/decisions/README.md:25` confirmed (ADR-0006 is registered there). | declined for budget, each released by its lens; recorded on the post-lock list and put to the owner for an overrule on the record at lock. |

**Post-lock list additions:** R7.6b to list the versions each state names; R7.2's oracle to name a cleared imported value; a golden asserting `sc_simulated` false; whether a Collection Mode rename moves an imported column's stored name; the collision tie-break for a literal `import_`-prefixed passthrough (standards SR10-5); the README's gamut gloss (PM). **Judgment calls recorded for the owner to overrule:** trims applied again under the round-9 decision to fund the round-10 fixes (FX10-8).

**Round-10 status:** one owner decision after the round (2026-09-16): the four released objections R10-F8 lists — R7.6b listing the versions E16/E23 name (test T10-1), R7.2's oracle naming a cleared imported value (privacy P10-1), OQ 2's ADR-0006 gloss (staff SE10-8 and three others), and an `sc_simulated`-false fixture (staff SE10-4) — are overruled on the record and carried on the post-lock list; the owner's reason: each is a released Low or Minor with a build-review or post-lock home, and none makes either document unsafe to hand to engineering. The round-9 trim decision is applied once more (FX10-8, −16 words). Two orchestrator corrections after the pass: R7.1's duplicated R1.2 cite pruned; R1.2's own tail reads "positions and values" as the outbound Inventory Import line now does (DF 7,996 words, DE 3,998). Fix file `prd-data-foundation-round-10-fixes.md` (both PRDs). Round 11 is the closing delta over the eight edited rows and entries by the lenses that objected (standards, reliability, architecture, staff, interface, test, plan) plus privacy on R1.2; the three mechanical checks re-run after it against the state that locks.

## Round 11 — the closing delta after round 10 (2026-09-16)

**Subject:** commit `19a21ea` — after the round-10 fix pass and two orchestrator corrections (DF 7,996 words of 8,000, DE 3,998 of 4,000; held: DF R1.2, R7.1, R7.5, R7.7, R5.7; DE R2.4, R4.1). **Lenses:** the eight whose rows changed — standards, reliability, architecture, staff, interface, test, plan, privacy — on Claude/Opus over those rows, the Vocabulary entry, the outbound Inventory Import line, the FX10-8 trims, fence F13's Why and EJ1 step 11. **Cross-model:** not repeated.

| lens | verdict | Blocker | Major | Minor/Nit | rows OBJECT | stands on |
|---|---|---|---|---|---|---|
| staff engineer | not ready until one clause reverts | 1 | 0 | 3 | R1.2, outbound Import line | SE11-1 |
| test (retargeted) | trustworthy after one Blocker | 1 | 0 | 3 | R1.2, outbound Import line, R7.5, R4.1 | T11-1 |
| standards (fresh) | publish with these changes | 0 | 1 | 4 | R1.2, outbound Import line, fence F13 Why, §8, DE outbound line | SR11-1 |
| architecture | sound — lock after one fix | 0 | 1 | 1 | R1.2, outbound Import line | A11-1 |
| interface | contract sound after one Major | 0 | 1 | 2 | R1.2, outbound Import line, R4.1, DE outbound line | IF11-1 |
| privacy | privacy-sound to ship | 0 | 1 Medium | 4 Info | R1.2 | nothing (releases P11-1 post-lock) |
| reliability | resilient — proceed | 0 | 0 | 1 | R1.2, outbound Import line | nothing |
| plan (retargeted) | ready to execute; proceed holds | 0 | 0 | 4 | R1.2, outbound Import line | nothing (withdraws its own R10-F7) |

**Round-10 dispositions.** Every round-10 finding from the eight lenses is resolved on the text (standards SR10-1/2/3, reliability RL10-1, architecture A10-1/2, staff SE10-1/2/3/5/6/7, interface IF10-2/3, plan PL10-1/2, privacy's Info), with one withdrawn: plan's PL10-3 (the outbound line handing over "positions and values"), whose fix is what every lens now objects to. The plan lens confirms proceed for the spike and the first build phase; PL7-4 stands.

**Row flips.** Every non-abstaining lens ALIGNs on DF R7.1, R7.7, the Vocabulary's chosen-condition entry, the FX10-8 trims, DE R2.4, fence F13's Why sentence (as rationale) and EJ1 step 11 — R7.1, R7.7 and R2.4 flip to 🤝 Aligned. Held for the last delta (round 12): DE R4.1 (IF11-2, T11-3 — one word, edited). DF R1.2 and the outbound Inventory Import line take an editorial reversion to the text every lens reviewed in round 10 (R11-F1) and flip with it. DF R7.5 draws one released OBJECT (test T11-2) whose fix does not fit; it goes to the owner for an overrule on the record at lock. R5.7 stays held on the owner's existing round-10 overrule.

### Verify-the-reviewer dispositions

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R11-F1 | "keeps the existing columns' positions and values" — added to R1.2's tail as an orchestrator correction and to the outbound Inventory Import line by FX10-7 after round 10's review — says a re-import never changes an imported value, contradicting the import PRD's R3.5 default ("take the new details") that the same row cites two clauses earlier; R7.1's read-back oracle inherits the contradiction and the outbound line would write it into a locked PRD | Staff, Test (Blockers); Standards, Architecture, Interface (Majors); Privacy (Medium); Reliability, Plan (Minors) | DF `:135`, `:258`; import PRD R3.5 `:148` ("The default is to take the new details") confirmed. | accept — "and values" deleted at both places, restoring the round-10-reviewed column-order rule; the value pin and its two change paths stay in R1.2's first clause. Editorial reversion, net −4 words. |
| R11-F2 | R4.1's bump-diff confinement omits the gamut-mark column, which a derivation bump legitimately flips on a boundary value | Interface (Minor), Test (Minor) | DE `:163`; DF R3.4, M7 confirmed. | accept — "the derived-value, gamut-mark and derivation-version columns" (+1 word). |
| R11-F3 | Fence F13's Why now says "the app reshaping nothing on the way", the over-reach FX10-5 removed from R2.4 | Architecture (Nit), Standards (Minor) | DE fences `:87` confirmed. | accept — "reshaping nothing but its quoting on the way" (fence file, unbudgeted). |
| R11-F4 | DE's outbound mirror still lists the generated corpus among "R7.7's fixtures"; §8's trimmed "even" diverges from DE §5 and every sibling | Standards, Staff, Plan, Interface (Nits) | DE `:185`; DF `:263` vs DE `:192` confirmed. | accept — "beside a generated corpus" (word-neutral); "even" restored (+1). |
| R11-F5 | Both R7.1 enumerations are asserted empty and never non-empty, so an enumeration that cannot return a row passes | Test (Minor, released) | DF `:224` confirmed. | declined for budget, released by the lens; to the owner for an overrule on the record at lock, post-lock list. |

Declined, released by the lens: plan's "second enumeration" naming on R7.5; staff's device-snapshot sentence not covering the generated corpus (index; post-lock with the mirror drift item); standards' "working set" defined inside another term's entry; privacy's omitted-column clause (+4, the same rule import R3.6 already states). **Post-lock list additions:** R7.5 asserting a non-empty enumeration at the interrupt; the value pin handed over on one obligation line (staff SE11-2).

**Round-11 status:** one owner decision (2026-09-16): R11-F5 — R7.5 asserting a non-empty enumeration at the interrupt (test T11-2) — is overruled on the record and carried post-lock; the owner's reason: a released Minor whose sibling observations in R7.5 already catch a no-op regeneration, with the assertion added at build review. The fix pass's bookkeeping put DE two words over, so one four-word rule-free trim in its Background landed under the standing trim decision (DF 7,996 words, DE 3,998). Fix file `prd-data-foundation-round-11-fixes.md` (five boxes). Round 12 is the last delta: interface and test on DE R4.1 only; the three mechanical checks re-run after it against the state that locks.

## Round 12 — the last delta, one row (2026-09-16)

**Subject:** commit `3d62dac` — after the round-11 fix pass (DF 7,996 words of 8,000, DE 3,998 of 4,000; held: DE R4.1, plus DF R5.7 and R7.5 on the owner's overrules). **Lenses:** interface and test on Claude/Opus over DE R4.1 (the one edited row) and, for confirmation, the reverted DF R1.2 and outbound Inventory Import line. **Cross-model:** not repeated.

| lens | verdict | Blocker | Major | Minor/Nit | rows OBJECT | stands on |
|---|---|---|---|---|---|---|
| interface | contract sound — safe to hand to engineering | 0 | 0 | 1 Nit | 0 | nothing |
| test (retargeted) | trustworthy — safe to hand to engineering | 0 | 0 | 1 Nit | 0 | nothing |

**Round-11 dispositions.** IF11-1/T11-1 (the reversion) RESOLVED at DF `:135` and `:258` — an exact restoration of the round-10-reviewed text with no straggler; IF11-2/T11-3 (R4.1's gamut-mark column) RESOLVED at DE `:163`; IF11-3 (the DE mirror's "beside") RESOLVED at DE `:185`. Both lenses release one shared Nit to the post-lock list: "gamut-mark" could use the already-defined term "gamut-clipped" (word-neutral).

**Row flips.** DE R4.1: both lenses ALIGN and it is not edited — it flips to 🤝 Aligned. DF R5.7 and R7.5 flip to 🤝 Aligned by the owner's overrules recorded in rounds 10 and 11 (the standing objections — test T10-1 and T11-2 — and the owner's reasons are in those rounds' status lines). With that, every requirement, surface, metric and copy row in both documents is 🤝 Aligned; Phase 6's lock record follows the lock edits and the mechanical checks on the state that locks.

## Lock record — Data Foundation and Data Export (2026-09-16)

- **Every row aligned:** DF — 46 requirement rows, R7.6b–n's 13 surface rows, 8 metric rows, 26 copy states, all 🤝 Aligned; DE — 13 requirement rows plus R4.4a, 1 metric row, 4 copy states, all 🤝 Aligned. Four objections stand overruled by the owner on the record (round 10, 2026-09-16), each released by its lens and carried below: R7.6b listing the versions E16/E23 name (test); R7.2's clear-beyond-recovery oracle naming a cleared imported value (privacy); OQ 2's ADR-0006 gloss (staff and three others; `docs/decisions/README.md` registers the ADR); a golden asserting `sc_simulated` false (staff). Two findings stand owner-rejected in the DF fence file (round 7): the salvage promise's generality (standards SR7-4) and a disclosure line on E27 (privacy).
- **Mandatory pre-lock round:** round 6 ran the retargeted `peer-plan-reviewer` plus two fresh lenses (`peer-standards-reviewer` on the export contract, `peer-reliability-reviewer` on the file's durability and recovery rows); every lens answered Phase 5's question ("does anything deferred to P1 break the first usable build?") with no; rounds 7–11 were deltas on the edits those rounds produced, the plan lens confirming proceed at each.
- **OQ contract:** DF — 12 open questions each with an interim rule and a named closer, answered ones with a results section; DE — OQ 1–4 likewise. Five provisional constants (SQLITE_READER_FLOOR, STORE_SIZE_BUDGET, DERIVATION_TOLERANCE, INTEGRITY_CHECK_BUDGET, WAVELENGTH_GRID) stay open under the Legend's rule that none ships in a release with its OQ open.
- **Zero unresolved placeholders; template guidance comments deleted** (six: DF Background, §4, Legend; DE Background, §4, Legend).
- **Mechanical checks, run on the state that locks (commit `4e9826f`, after `1e8830a`'s sweep found three editorial misses and one post-lock item, all closed in that commit):** cross-PRD consistency over capture, import, device, DF and DE (obligations both ways, shared priorities, owning-document labels on every cross-PRD cite, retired and moved IDs, per-PRD fence numbering); index sync (fence-to-row maps, Surfaces against copy marks, journeys, OQ results, copy markers, word counts DF 7,991 of 8,000 and DE 4,000 of 4,000); latent-decision inventory (constants, copy states, priorities, terms, placeholders, statuses). Result: clean — zero editorial, zero post-lock, zero decision misses (report: `docs/agent-reviews/2026-09-16-data-foundation-lock-checks-final.md`); 862 links in the ten files and 2,197 across the product docs, none broken; fence maps 32/32 and 13/13; 26 of 26 copy states on a surface; every row 🤝 Aligned.
- **Fences:** DF F1–F32, DE F1–F13, with the fence → row maps in each fence file.
- **Post-lock list and the judgment calls that stood at lock:** moved to [the post-lock list](../product/post-lock.md) on 2026-09-17, the single source of truth; this record no longer carries them.
- **Next:** one PR from `docs/data-foundation` to `main`; the owner merges. The device PRD's spike-scope amendment lands before the spike is dispatched.
