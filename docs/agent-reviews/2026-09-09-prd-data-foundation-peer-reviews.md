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
