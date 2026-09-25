# Peer-review gate — prd-collection-mode (2026-09-24)

**Mode:** requirements. **Subject:** `docs/product/collection-mode/prd-collection-mode.md` and its four companions (`-journeys`, `-copy`, `-fences`, `-oq-results`), plus the sibling-PRD halves of its seams landed in the same change (Capture Mode, Data Foundation, Inventory Import, Data Export, Device Management). **Reviewers (round 1):** peer-product-manager-reviewer, peer-staff-software-engineer-reviewer, peer-test-reviewer, peer-interface-reviewer, peer-privacy-reviewer, peer-product-marketing-manager-reviewer, peer-architecture-reviewer, peer-performance-reviewer; cross-model: peer-staff-software-engineer-reviewer on agy. **persona-version:** cache/1.4.0. **Skill:** operator-agents:writing-agent-prds 1.7.0 via agent-dispatch:running-the-peer-review-gate 1.7.0.

**tier-rationale:** the four standing lenses of writing-agent-prds (product-manager, staff-software-engineer, test, interface) are the floor. Added by document shape: privacy — the rows display, edit and delete user-entered values and device snapshots (serials) and set what the user's file keeps; product-marketing-manager — the copy companion is end-user-visible text; architecture — the rows sit on the open capture→collection seam (ADR-0004), the queued schema (ADR-0003) and ADR-0001's Rust-core data layer; performance (Tier 3, data volume) — the operating envelope sets browse/search/sort/Find-similar budgets at scale. Not added: security (no auth, network or trust boundary — local-only, R8.6), database (no schema or SQL; ADR-0003 owns it), reliability (no long-lived service; durability is Data Foundation R1.10's), standards, release, devops, retrieval (no such shape).

Log of rounds: one `## Round N` section per round, appended; `log-new` is never re-run for this PRD.

## Round 0 — owner adjudication before round 1 (2026-09-24)

No lens ran. The owner decided F1–F4 before the fill, F5–F20 after it, and F21–F28 after the round-0 fix pass; each is a dated fence in `docs/product/collection-mode/prd-collection-mode-fences.md`. The round-0 fix list is `docs/product/collection-mode/prd-collection-mode-round-0-fixes.md` (sections Round 0 and Round 0b, every box ticked but the post-lock tick that waits for the PR number).

## Round 1 (2026-09-24)

**Lenses:** the eight above on the Claude Agent-tool route, in parallel; then one cross-model pass — peer-staff-software-engineer-reviewer on agy (Google Gemini via the Antigravity CLI), serialized. **Cross-model consent:** the owner chose "Yes — agy (Google Gemini)" when told the pass sends the brief and the files it names (this PRD, its fence file, copy strings and journey fixtures, and the sibling PRD diffs) to the runtime's external model provider (2026-09-24). **Subject commit:** recorded below with the results.

(findings, per-row disposition tables and verify-the-reviewer dispositions follow)
