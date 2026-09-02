# Spike Results: The Rust↔Swift Boundary (ADR-0001 Gate)

_2026-09-01. Evidence for ratifying [ADR-0001](../decisions/0001-rust-core-swiftui-shell.md). Produced per the boundary-spike plan; spike code lives on the never-merged branch `spike/rust-boundary` (commits `abf20b0`…`1cf0ca2` in a scratch worktree) and is decision evidence, not product code. Executor: codex (codex-cli 0.149.1) via the agent-dispatch pipeline, one unit per dispatch, orchestrator-verified gates._

## Verdict table

Per ADR-0001's falsification semantics: only *does not work* counts against ratification; *works-with-workaround* findings are documented costs.

| Criterion | Verdict | Core evidence |
|---|---|---|
| 1. Streaming + payloads, both directions | **works-with-workaround** | Rust→Swift `AsyncStream` of 100 events in order; Swift→Rust foreign-trait ingestion of 10,000 × 2KB payloads with zero retained memory growth; 1 serialization-boundary allocation per event in each direction |
| 2. Cancellation | **works-with-workaround** | Drop-callback route confirmed unreachable from safe code; explicit cooperative token works — 25ms cancel-to-stop at a 20ms poll interval, 0.009ms pre-stream, idempotent double-cancel |
| 3. Swift 6 strict-concurrency seam | **works** | 6 unsafe-concurrency declarations, all in generated code; **0 handwritten**; byte-identical across regenerations; confined to the boundary module |
| 4. Live queries at 10k rows | **works-with-workaround** | 0 follow-up SELECTs across 100 single-row writes (delta patching, no requery); p95 write-to-visible 0.019ms (in-memory); burst coalescing works with a keyed-coalescer production caveat |

**No criterion failed. The all-Swift fallback is not triggered.**

## Environment

Swift 6.3.3 + cargo 1.97.1 under Command Line Tools as the active developer directory (Xcode 26.6 installed, not selected — CI parity posture held; no `xcode-select` switch was ever needed). Two environment findings: CLT ships neither `Testing` nor `XCTest`, so the shell pins the standalone `swift-testing` 0.12.0 package (workaround-class); CLT *does* expose `swift test --sanitize thread` (see mitigations).

## Criterion 1 — streaming and payloads (U2)

The `SpectroDevice` interface is a UniFFI foreign trait (`#[uniffi::export(rust, foreign)]`) — the legacy `callback_interface` form is foreign-only and cannot host the pure-Rust mock, so the modern trait form is required (workaround #1). Events wrap into a Swift `AsyncStream` via the continuation pattern. Measurements (named observable: allocations at the serialization boundary, identical method both directions):

- Rust→Swift: 100 events @ 8KB — 100 boundary allocations (one copy per event), 819,200 boundary bytes.
- Swift→Rust: 10,000 events @ 2KB — 10,000 boundary allocations; Swift retained-memory delta 0 across batches (bounded).
- Slow consumer: 0 events dropped — via **unbounded** `AsyncStream` buffering (workaround #2: this is not production backpressure).
- Observability limit: with `#![forbid(unsafe_code)]`, allocations inside UniFFI's generated lowering/lifting are not directly countable; the boundary counters + Swift malloc-zone deltas are the honest proxy (workaround #3).

## Criterion 2 — cancellation (U3)

The research flagged the drop-callback route as inferred-only; the spike settles it empirically: `ForeignFutureDroppedCallbackStruct` is generated plumbing for the opposite direction (Rust notified when a foreign future drops) and is **not a safe-code cancellation hook**. The working route is the documented-workaround shape from the plan's AE1: a Rust-exported cooperative token; Swift `withTaskCancellationHandler` + `Task.cancel()` signals it; the scan loop polls it. Measured: mid-stream cancel stops delivery in 25.0ms at a 20ms poll interval (latency is polling-bound); cancel-before-first-event 0.009ms with zero deliveries; double-cancel idempotent; both the Swift signal and the Rust observation asserted, not assumed. Production latency will track how often the device operation can check the token — or whether the vendor SDK exposes its own cancellable call.

## Criterion 3 — the strict-concurrency seam (U4)

Full enumeration (regeneration-stable, byte-identical across two regens): 4 `@unchecked Sendable` conformances + 2 `nonisolated(unsafe)` vtable pointers, all inside the generated `SpectroCore` target. Handwritten unsafe-concurrency annotations required: **zero** — the audit found U2's two handwritten `@unchecked Sendable` hedges were unnecessary and replaced them with compiler-checked `Sendable`, which strict concurrency accepts. Clean build/test output carries no concurrency diagnostics. The audit surface is small, stable, and structurally confined — the research's fear that `@unchecked` would leak into app-level call sites did not materialize in this scaffold.

## Criterion 4 — live queries (U5)

`spike/store` (rusqlite, `forbid(unsafe_code)`) seeds 10k rows and emits keyed deltas over the same event path as criterion 1 (the plan's shared-path design intent). The Swift consumer patches its visible window in memory. Evidence: 1 initial window SELECT, then **0 SELECTs across 100 single-item writes** — asserted by an explicit query counter, so "no full requery" is measured, not inferred. Latency: p50 0.016ms / p95 0.019ms write-to-visible (in-process, in-memory SQLite; not a disk-I/O prediction; the 100ms reference bar is cleared by orders of magnitude). Burst: 100 rapid same-row writes coalesced 98 events via `bufferingNewest(1)` without stalling the writer — with the caveat that newest-one buffering is not a keyed coalescer; mixed-row bursts need a per-row-newest buffer or consumer-side batching (workaround-class implementation cost; not a requery limitation).

## Failure loudness at the seam (U6, sabotage probes)

The ADR's premise is that compilers catch agent mistakes; the seam is where that premise is weakest. Four deliberate plausible-agent-mistake injections, each fully reverted:

| Probe | Result |
|---|---|
| Stale generated bindings after a `#[uniffi::export]` signature change | **LOUD** — UniFFI runtime halts immediately: `Fatal error: UniFFI API checksum mismatch: try cleaning and rebuilding your project`. The binding-drift class is machine-caught. |
| Dropped continuation (producer returns early, `finish()` never called) | **LOUD, poor diagnostic** — detected only by an external timeout; no continuation-specific message. |
| Callback invoked from a raw wrong thread, no executor hop | **SILENT** — gates pass; nothing flags the thread. |
| Unnecessary `@unchecked Sendable` hiding an unsynchronized counter raced by 8 tasks | **SILENT** — compiles and passes; observed 412,934 of 800,000 increments. The hedge suppresses exactly the guardrail the architecture is chosen for. |

Per the plan's AE3, the silent results are recorded findings, not verdict changes. **Mitigations, all cheap and concrete:** (1) TSan is available under CLT (`swift test --sanitize thread`) and would catch the race — add a TSan lane to the gates; (2) forbid handwritten `@unchecked Sendable` outright (criterion 3 proved it unnecessary; a grep gate enforces it); (3) wrap sink callbacks with a debug thread/executor assertion; (4) bound every stream test with a timeout so a dropped continuation reads as a failure, as U6 did.

## The iteration tax (priced, not gated)

Warm-cache `make gates` (cargo test + clippy `-D warnings` + swift test, bindings regenerated every run). The priced endpoint is green gates, not a runnable app:

| Change class | Run 1 | Run 2 |
|---|---|---|
| No-op baseline | 7.54s | 5.54s |
| Rust-only body edit | 6.49s | 5.54s |
| Swift-only body edit | 6.76s | 5.42s |
| Boundary signature change (regen + call-site update) | 7.01s | 5.55s |

The boundary adds effectively no steady-state wall time; its real tax is the coordination across Rust API, generated artifacts, and Swift call sites — which the checksum fatal (above) converts from a silent drift risk into a loud one.

## Agent-workflow friction (R12, distilled from the spike log)

Six code units, six codex dispatches, **zero retries, zero hard-blockers, every gate green on first dispatch** and again on independent orchestrator re-runs. Worker-reported friction was uniformly workaround-class: the foreign-trait form discovery (U2), a stale-bindgen Makefile ordering bug the worker found and fixed itself (U2), and the CLT test-module gap (U1). Orchestration-side friction: one lost dispatch output stream caused by the orchestrator double-backgrounding a dispatch (recovered by PID watch; tooling-class, not boundary-related), and the dispatch tooling's completion marker was synthesized from the diff on every run (marker protocol not followed by the worker — cosmetic). Against the owner's flip condition — "extremely difficult for the agents building it" — the observed difficulty was low: no unit required a second dispatch.

## Ratification notes

- **Recommendation carried by this evidence: ratify ADR-0001** (Rust core + SwiftUI shell). All four criteria hold; every workaround is bounded and named; the silent-failure findings come with cheap mitigations that belong in the enforcement gates at ratification.
- **R14 amendment note:** at ratification, amend ADR-0001's fallback clause to match the falsification semantics — the trigger is a criterion that *does not work*; drop "prices the boundary above its worth" as a trigger. The owner applies this edit as part of the ratification pass.
- **Suggested gate additions at ratification** (from the sabotage evidence): a TSan test lane; a zero-handwritten-`@unchecked Sendable` check; debug thread assertions in callback sinks.
- Spike branch `spike/rust-boundary` and its worktree are flagged for deletion after ratification; nothing from it merges.
