# ADR-0001: Rust core + SwiftUI shell

**Status:** Proposed — pending the boundary spike below and ratification by the project owner.
**Date:** 2026-09-01 (revised same day per peer-architecture review)
**Evidence:** [`docs/briefs/rust-primary-language-research-results.md`](../briefs/rust-primary-language-research-results.md) (53-question research pass); prior baseline in [`docs/briefs/macos-swiftui-app-architecture-research-results.md`](../briefs/macos-swiftui-app-architecture-research-results.md).

## Context

The product vision listed SwiftUI as decided and the architecture research recommended an all-Swift stack. Two premises, stated by the project owner and outside that research's original scope, re-opened the question:

1. **The goal is bug-surface reduction through compiler enforcement** — not performance. The project explicitly rejects build-fast-then-fix: near-zero defects at check-in, minimal tech debt.
2. **Development is agent-only.** Vinny is the product manager (WHAT/WHY); AI coding agents are the sole builders (HOW). A coding agent's plausible-but-wrong change should fail to compile, not fail at runtime. Rust's non-optional enforcement (ownership, exhaustive `match`, `Result`-typed errors, `Send`/`Sync`) rejects a class of agent error that Swift permits with discipline; and a pure-Rust core verifies headlessly in fast, Linux-runnable, hardware-free CI — a materially tighter feedback loop for an agent than macOS-runner Xcode builds (an all-Swift split with a platform-scoped core SPM target recovers some of this benefit, at macOS-runner cost).

Three shapes were evaluated. **Full-Rust UI (shape B) is disqualified** by the product's own thesis: Slint carries a documented sRGB-blending correctness bug, gpui's VoiceOver is broken per its own maintainers, webview P3 support does not cover canvas-drawn content, and wgpu's macOS wide-gamut pipeline is reported incomplete (that last finding is inferred, not primary-sourced; the Slint and gpui findings carry the disqualification on their own). Shipped teams have retreated from Rust UI to native shells. An app whose non-negotiable is gamut honesty cannot risk silently unmanaged sRGB. **All-Swift (shape C)** remains credible — Swift 6 strict concurrency, optionals, overflow trapping, and exhaustive `switch` occupy much of the safety ground — but its enforcement is partly opt-out, its diagnostics degrade on SwiftUI-adjacent code, and its verify loop requires macOS runners.

**This decision overrides the research's own default, and that must be recorded.** On the evidence alone, the research favors the all-Swift incumbent: §1.5 places this project below the two-language payoff line; §9.5 finds the strongest Rust defect-rate evidence does not transfer to an ARC-safe, IO-bound, non-adversarial desktop app ("stateful business logic… is exactly where Rust's case weakens"); §10.5 sets the bar at a concrete, *current* pain point all-Swift cannot solve — which does not exist. The two owner premises above are what clear that bar: they change who writes the code and what an error costs. If the premises fall, so does this decision (see flip conditions).

The findings that argued against a Rust core for a *human* team split two ways under agent labor. The **owner's build loop** genuinely cheapens: mechanical binding upkeep and single-builder onboarding are agent-cheap. The **open-source contributor tax does not** — this repo is public and open source from day one (a Decided constraint), so a human drive-by contributor whose PR crosses the boundary needs both toolchains plus binding regeneration; that is an *accepted cost* of this decision, not a shrunk one. A second accepted risk: agents have deep, stable knowledge of Swift/SwiftUI and thinner, faster-staling knowledge of pre-1.0 UniFFI — agent error is likeliest to concentrate at the one surface neither compiler checks, which the contract-test gate below exists to contain.

## Decision

Adopt **shape A**: business logic, data layer, and color math in Rust; the UI shell and the vendor-SDK device layer in Swift/SwiftUI; bridged with UniFFI (0.32.x line), with the scan pipeline crossing the boundary as an async stream (the Ferrostar callback-to-`AsyncStream` pattern).

**This ADR may be ratified only after a time-boxed boundary spike (1–2 weeks) retires or prices its four known design costs.** Spike code is decision evidence: it lives in a scratch worktree and is never merged.

Spike exit criteria — all four answered with working evidence; the fallback triggers if **any** fails or prices the boundary above its worth:

1. **Streaming + payloads, both directions.** Rust→Swift: a mock-device Rust crate emits scan events consumed in Swift as an `AsyncStream`. Swift→Rust: a Swift-side hardware-free fake implements the same UniFFI foreign trait the live device layer will implement, feeding raw-payload byte buffers of realistic size into the Rust core — the live topology's actual ingestion direction, and the path CI can never exercise with real hardware. Copy behavior measured both ways (UniFFI's zero-copy path is documented for Kotlin, unverified for Swift).
2. **Cancellation.** Mid-scan cancellation built from UniFFI's drop-callback hook, demonstrated from the Swift side against the mock device (UniFFI has no built-in cancellation).
3. **Concurrency seam.** The generated bindings' actual Swift 6 strict-concurrency posture: enumerate every `@unchecked Sendable` surface the binding forces, and show the audit boundary is small and stable.
4. **Live queries at scale.** Collection-view updates over a 10k-row store via a hand-built change-notification path (no Rust SQLite crate offers a GRDB `ValueObservation` equivalent): single-item write updates visible rows without a full requery, at interactive latency. This is the collection-mode hot path — and the capture hot path too if ADR-0004 lands on the v2 research's direct-write recommendation.

Additionally the spike **prices** (does not gate on) the boundary edit-compile-run tax: the three-way measurement the research's Question Status already scripts — a Rust-only change, a Swift-only change, and a `#[uniffi::export]` signature change — timed through to a runnable app.

**Fallback:** if the spike fails, this ADR is rewritten to all-Swift with a mandatory enforcement regime — strict concurrency from first commit, typed throws at module boundaries, exhaustive `switch` in state machines, schema-enforced version history, property tests against reference fixtures, and a platform-scoped core SPM target isolated from the UI for maximal headless testing — and the failure evidence is recorded here.

### Enforcement gates (part of this decision)

- Rust core: `#![forbid(unsafe_code)]` in logic crates; `clippy -D warnings`; `cargo nextest` + property tests (color math validated against Munsell renotation fixtures) in hardware-free CI on every PR.
- Swift shell and device layer: Swift 6 strict concurrency on from the first commit; Swift Testing for shell unit tests; XCTest for UI tests.
- **Cross-boundary contract test, permanent:** the spike's Swift-side fake (criterion 1) is kept as a CI test that exercises the *actual generated bindings* in both directions on every PR. UniFFI's generation-from-one-source and checksums close structural drift; this test is what catches semantic drift (threading, ordering, cancellation timing) that checksums cannot.
- The `SpectroDevice` seam is also the FFI boundary: the mock implementation is pure Rust so agents and CI exercise the full core without hardware or a license key. Device-touching PRs still require human hardware verification — the one builder-side step agents cannot perform.

## Consequences

**Positive.** Agent check-ins are compiler-gated where the bugs would live; the core's verify loop is fast, deterministic, and macOS-free; the memory-safety and data-race classes are closed by construction rather than convention; a future cross-platform move (today a non-goal) would inherit the core.

**Negative / accepted costs.** The FFI boundary is an unchecked surface neither compiler sees across — bounded by keeping it thin, generated from a single source of truth, and contract-tested per the gate above; Swift's data-race checking is suspended at the seam (`@unchecked Sendable`), accepted only at the size the spike demonstrates; live queries are hand-built rather than free; the human open-source contributor tax is accepted as stated in Context; Xcode drives packaging regardless (no shape-A bundler exists); a whole-app tokio runtime is avoided (Zed precedent) — the core runs a deliberately owned executor arrangement decided in ADR-0005.

**Migration ownership.** Superseding GRDB forfeits `DatabaseMigrator`. The migration story becomes an assembled capability — `rusqlite_migration`/`refinery` plus the version-tagged blob-decode convention (research results §5.4) — for a user-owned, portable file where schema is the project's sharpest one-way door. **ADR-0003 owns the migration mechanism**; it may not be left implicit.

**Re-scoping of prior recommendations.** The architecture research's Swift-stack recommendations (MV, swift-dependencies, Swift Testing, GRDB) are re-scoped to the shell or superseded: the data layer moves to the Rust core (rusqlite family — exact crate deferred), and the shell's internal pattern becomes a deferred two-way door.

**Flip conditions.** (1) Spike failure → all-Swift fallback above. (2) The builder mix changes — agents stop being the effectively sole authors of core code (beyond drive-by OSS contributions, whose cost is already accepted) → the two-language-tax findings reactivate at full weight; re-argue via a superseding ADR. (3) During early build, if defect escapes concentrate at the FFI seam rather than inside either language domain, the premise that enforcement nets out positive is failing empirically — re-argue. (4) Windows/Linux entering scope only strengthens this decision.
