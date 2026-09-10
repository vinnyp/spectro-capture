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
