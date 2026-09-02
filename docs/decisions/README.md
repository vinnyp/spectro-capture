# Architectural Decision Records

This directory is the only place a recommendation becomes a decision (see [AGENTS.md](../../AGENTS.md) §2). Research results can recommend with strong evidence; until an ADR here is **Accepted**, nothing is decided.

## Process

1. **Evidence** — a research brief/results pair in [`docs/briefs/`](../briefs), or a spike whose code is decision evidence and is never merged.
2. **Draft** — the builder (Claude) authors the ADR as `Status: Proposed`, Nygard format: Context / Decision / Consequences.
3. **Review** — peer-architecture review of the draft; product-level questions route back to the PRD.
4. **Ratification** — Vinny (PM/owner) flips `Status: Proposed` → `Accepted` in a PR. Only that flip decides anything.

One file per decision: `NNNN-title.md`. Statuses: `Proposed`, `Accepted`, `Deferred`, `Superseded by NNNN`. An Accepted ADR is revisited only by a new ADR that supersedes it.

## Decision queue

Ordered by dependency first, then irreversibility (numbering is pinned where existing docs already cite an ADR — the vision cites ADR-0002 for instrument scope — so number order is not strictly ratification order). Ratifying the queue before product code is the "architecture decided before code" plan.

| # | Decision | Status | Gate / blocker |
|---|---|---|---|
| [0001](0001-rust-core-swiftui-shell.md) | Language & UI shape: Rust core + SwiftUI shell | **Accepted** (2026-09-01) | Gate satisfied — [spike evidence](../briefs/rust-boundary-spike-results.md); fallback not triggered |
| 0002 | v1 instrument scope: Nix Spectro 2 / Spectro L only | queued — already cited by the vision, needs formalizing | none |
| 0003 | Storage schema: canonical raw payload, version history, derived-value recompute, and the schema-migration mechanism | queued | PRD for data/versioning; the sharpest one-way door — this schema ships inside users' own files. Scoped to the measurement/versioning core; session-adjacent tables (queue, dead-letter) wait for 0004's seam re-open |
| 0004 | The capture → collection seam | queued | PRD-level re-open first: the two capture-mode research passes conflict and v2 says re-open before the ADR |
| 0005 | Module layout, the `SpectroDevice` seam placement, and executor/runtime ownership (no whole-app tokio; see 0001) | queued | depends on 0001 |
| 0006 | Minimum macOS version | queued | research cites APIs spanning 13.0–15.0; decide against 0001's toolchain needs |
| 0007 | Sandboxed vs. unsandboxed distribution | queued | research calls it "a deliberate decision, not an assumed requirement" |
| 0008 | Distribution & updates: Developer ID + notarization pipeline, Sparkle (or alternative) update channel | queued | currently "recommended, no ADR" in AGENTS.md; decide with 0007 |

## Deliberately deferred (two-way doors)

Deferral is itself a decision — these are cheap to reverse and are decided at first use, not up front:

- Shell architecture pattern (MV vs. alternatives) — becomes low-stakes once business logic lives in the core (see 0001)
- Exact crate/package choices below the ADR level (e.g. which CSV crate)
- Code-style/lint configuration details beyond the enforcement gates named in 0001
