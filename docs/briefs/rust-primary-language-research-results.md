---
canonical-id: 70a7ca96-1332-4d47-8f66-4ebc5b9cc65e
---

# Research Results: Rust as the Primary Language for SpectroCapture (macOS)

_2026-09-01. Answers the questions in [rust-primary-language-research-brief.md](rust-primary-language-research-brief.md)._

> Produced by six parallel in-session research agents (Claude Code), each covering the concern areas noted per section, against live web sources on 2026-09-01. Inferred-vs-documented flags are the agents' own. This is a lighter instrument than a dedicated deep-research pass; the Question Status section records every remaining gap.

---

## 1. Architecture Shapes & Shipped Precedent

Scope: Concern Area 1 (Architecture Shapes & Shipped Precedent) and Concern Area 10 (Decision Framework & Risks) only, per assignment. Research conducted 2026-09.

---

### 1.1 Shipped macOS apps: Rust core beneath a native Swift/AppKit/SwiftUI shell

The clearest, most-documented example is **1Password 8 for Mac**. 1Password consolidated business logic, cryptography, database access, permissions enforcement, and server communication into a single Rust "Core" library, then built two Mac front ends — one in SwiftUI targeting current macOS, one in web UI for older OS support — as thin shells over that Core (Fey, 2021; Serokell, 2023). As of the most recent public figure, roughly 63% of the 1Password codebase across all platforms is Rust (Serokell, 2023). The team built a custom code-generation tool, **Typeshare**, specifically to keep Rust and Swift/Kotlin/TypeScript type definitions from drifting apart across the FFI boundary — this was reported as a real, ongoing cost, not a one-time setup expense (1Password, n.d.-a; Serokell, 2023).

A second, smaller but more granularly documented example is **Portals**, a macOS menu-bar app built by the Ockam team. They first shipped a Tauri (Rust + web UI) version, then rebuilt the UI in native SwiftUI while keeping Rust for the business logic — landing on exactly shape A after trying shape B first (see 1.2 and 10.3). Their FFI is hand-rolled C (via `cbindgen`), not UniFFI: idiomatic Rust structs, C-compatible pointer structs, and native Swift classes are maintained as three parallel representations, converted at the boundary (build-trust, n.d.).

**Caveats on other commonly-cited examples, checked and found not to fit the question as asked:**
- **Signal**: `libsignal`'s cryptographic core is Rust, with generated Swift bindings consumed by **Signal iOS** (signalapp, n.d.). But Signal's *desktop* app (the one that would compete with a macOS Swift/AppKit shell) is Electron/TypeScript, not a native Swift shell — so Signal is evidence for "Rust core + Swift shell" only on iOS, not on macOS. Flagged: this is a common misattribution worth correcting in the ADR.
- **Firefox's Rust components**: Mozilla's `application-services` Rust components ship into **Firefox for iOS** via a generated Swift package and XCFramework (Mozilla, n.d.-a; Mozilla, n.d.-b), using UniFFI for most components. Firefox's macOS desktop browser itself is Gecko (C++/Rust), not Swift-shelled. Same caveat as Signal: real precedent, wrong platform for this specific question.
- **Automerge**: `automerge-swift` links the Rust CRDT core via XCFramework and explicitly supports macOS 10.15+ (Rhonabwy, 2023; automerge, n.d.). The open-source **MeetingNotes** app is offered as a demonstration SwiftUI app on iOS/macOS using it — a genuine but small/demo-scale example, not evidence of a large shipped commercial app.

| App | Rust side | Swift side | Boundary tool | Reported maintenance cost |
|---|---|---|---|---|
| 1Password 8 (Mac) | Crypto, sync, DB, permissions, server comms (~63% of codebase) | SwiftUI UI shell | Custom (Typeshare + hand-rolled bridge, not UniFFI) | Explicit: type-drift across FFI was a recurring problem, motivating dedicated tooling (Serokell, 2023) |
| Portals | Core app logic | SwiftUI UI shell, after abandoning Tauri | Hand-rolled C via `cbindgen` | Explicit: duplicated struct definitions across 3 representations, judged worth it (build-trust, n.d.) |
| Automerge/MeetingNotes | CRDT core | SwiftUI demo shell | XCFramework, generated Swift API | Not reported (demo-scale) |

### 1.2 Shipped full-Rust macOS apps (Rust owns the UI too)

| App | UI stack | Bespoke or general-purpose? | macOS status |
|---|---|---|---|
| **Zed** | `gpui` — Zed's own GPU-accelerated (Metal on macOS) UI framework | Bespoke, built by and for Zed, though published independently under Apache-2.0 for outside use (gpui.rs, n.d.; Zed Industries, n.d.-a) | Launched as macOS-only preview in 2023; added Linux (Oct 2024) and Windows (Jan 2025); reached 1.0 on 2026-04-29, shipping on macOS, Windows, and Linux (Wikipedia, n.d.; Zed, 2026) |
| **Warp** | `warpui` — Warp's own Entity-Component-Handle framework, rendering primitives (rect/image/glyph) directly to Metal | Bespoke, built in-house specifically because no existing Rust framework supported Metal at the time (Warp, n.d.-a; Warp, n.d.-b) | Shipped, macOS-first terminal, claims 144+ FPS rendering (Warp, n.d.-b) |
| **Rerun** | Built on `egui` (general-purpose immediate-mode Rust GUI, also authored by Rerun's founder) | General-purpose, not bespoke | Cross-platform (desktop/web/Jupyter) visualization tool for robotics/CV; ships to macOS via the standard egui/winit stack (Rerun, n.d.) |

Both Zed and Warp deliberately built **bespoke** frameworks rather than adopting a general-purpose one — both cite the absence of a mature, Metal-backed, production-grade general Rust UI framework at the time they started as the direct cause (Warp, n.d.-a). This does not transfer cheaply to a small team: it was a multi-person, multi-year investment for both companies, not a library one downloads. Rerun is the more relevant precedent for a small/solo team, because it adopted the general-purpose `egui` rather than writing a bespoke renderer — but Rerun's founder is egui's author, which is a confound (indirect evidence: Rerun's ease-of-adoption may not generalize to a team without that specific expertise) (Rerun, n.d.).

### 1.3 Biggest reported win vs. biggest reported regret, 2023–2026

**Biggest win**, consistently reported across shape-A and shape-B teams: raw performance/latency headroom and (for shape A specifically) the ability to write business logic once and share it across every platform 1Password ships to, keeping behavior consistent (Fey, 2021; Serokell, 2023). For Rerun specifically: memory safety without GC, exhaustive `match`/enum handling, and `?`-based error handling were called out as concrete productivity wins, not just performance (Rerun, n.d.).

**Biggest regret**, and this is the more load-bearing finding for this brief: it clusters around **UI, not the core**, in every retrospective found. Warp explicitly titled a post "Why is building a UI in Rust so hard?" and detailed concrete pain: Rust's ownership model fights the mutable, shared-state-heavy nature of UI component trees; the lack of inheritance made component-tree code awkward; and they reported *production crashes* from concurrent `RefCell::borrow_mut()` calls in event handling (Warp, n.d.-a). Rerun's regret list is "a lack of mature GUI libraries" plus thin scientific-computing/CV library support (Rerun, n.d.). Tritium (a two-person legal-tech shop) reported the sharpest concrete regret: all major Rust GUI frameworks (egui, Slint, iced) share `winit` as a windowing backend, so switching *frameworks* doesn't escape the underlying platform-integration bugs — they hit a macOS "open with"/document-type bug rooted in `winit`, abandoned a four-week Slint migration at 45% completion, and settled for patching `winit` with native Objective-C code rather than migrating (Tritium, n.d.).

No 2023–2026 retrospective was found reporting the FFI boundary itself (shape A) as the single biggest regret; the regret pattern in the literature is specifically about **owning macOS-native UI in Rust** (shape B), not about the Rust/Swift seam. This is worth stating plainly since it bears directly on shape B vs A for this project.

### 1.4 Shipped precedent: hardware-peripheral-centric macOS app (BLE/USB instrument) with a Rust core

**No shipped, commercial precedent was found.** Searches for BLE/USB instrument-control desktop software with a Rust core (medical devices, lab instruments, sensor dashboards) surfaced only hobbyist/demo-scale projects — e.g., a Rust+Dioxus desktop dashboard reading a BLE air-quality sensor via a USB BLE dongle, built on `btleplug`/`bluest`-style crates (BleuIO/HibouAir demo, n.d.). `btleplug` itself is a real, maintained cross-platform (Windows/macOS/Linux/iOS/Android) BLE-central crate with explicit macOS support and documented Info.plist/entitlement requirements (deviceplug/btleplug, n.d.), so the *libraries* exist and are usable — but no evidence surfaced of a shipped, non-trivial commercial macOS app built on them for instrument control. TTP (a product-development consultancy) published an argument for using Rust in diagnostic/medical devices, but this is a persuasive/consulting piece about the *embedded firmware* side, not a shipped desktop-app case study (TTP, n.d.). **This is a genuine gap in available evidence, not an inferred negative** — see Question Status for the concrete follow-up.

### 1.5 Team/codebase size threshold where two-language overhead pays off vs. drags

The most directly relevant, if anecdotal, data point comes from Matt Welsh's widely-cited account of running Rust at a startup for ~2 years: the tax was tolerable at small scale but became "an increasingly heavy tax" specifically **as headcount grew roughly 10x** — development got sluggish, feature launches slowed, and the team never fully overcame the learning curve even after months of daily use (Welsh, 2022). Welsh's own stated bar: "For individual projects, or very small (say, 2–3 person) teams, Rust would likely be just fine" (Welsh, 2022) — note this is about Rust generally (a CRUD backend team), not a two-language FFI split specifically, so it somewhat overstates the tax relevant to shape A, where only the core (not the whole product) is Rust.

Allen Pike's framework, aimed more precisely at shared-core-vs-native tradeoffs, reframes the variable as **organizational coordination cost, not raw headcount**: small teams (his figure: 3–6 people) do fine with native/per-platform code because there's no cross-team coordination tax to amortize; shared-core approaches pay off mainly for organizations large enough that keeping N native codebases feature-consistent has become the actual bottleneck (Pike, 2021). Pike explicitly cites 1Password's shared Rust core as "well received" at 1Password's scale, while noting Dropbox and Slack pulled back from analogous shared-core strategies for mobile (Pike, 2021) — see 10.3.

**Applied to this project**: SpectroCapture is a solo-to-small open-source team building one platform (macOS only — no cross-platform coordination problem to amortize a shared core against) with no existing codebase. Both Welsh's and Pike's thresholds point the same direction for this specific project: it sits *below* the line where either author's evidence says the two-language tax pays for itself, because the coordination-cost argument for a shared core (Pike, 2021) doesn't apply to a single-platform app, and the headcount tax (Welsh, 2022) applies most sharply exactly at solo/small scale in Welsh's own telling, before an app-shaped team has grown past 2–3 people. **This conclusion is inferred by combining two data points that were not about this project** — flagged as indirect reasoning, not a directly documented verdict about a solo macOS-only OSS app.

---

## 2. The FFI Boundary (Shape A)

_Sections 2 and 6 were prepared 2026-09-01 by the agent assigned §2 "The FFI Boundary" and §6 "Concurrency & the Hardware-Paced Pipeline"._

---

### 2.1 UniFFI vs. swift-bridge vs. cbindgen/cxx vs. hand-rolled C ABI — versions, maintenance, dominant recommendation

| Tool | Current version (2026-09) | Backing org | Maintenance signal | Swift support |
|---|---|---|---|---|
| **UniFFI** (`uniffi` crate) | 0.32.0, released 2026-06-30 (Docs.rs, n.d.; Lib.rs, n.d.-a) | Mozilla | Actively developed; heavy 2026 issue traffic (dozens of open issues filed July–Aug 2026), used in production by Firefox mobile/desktop and by third parties (Mozilla, n.d.-a) | First-class, "production-quality" support, described as UniFFI's own bar (Mozilla, n.d.-b) |
| **swift-bridge** | 0.1.59, released 2026-01-06 (Lib.rs, n.d.-b) | Independent maintainer (chinedufn), community project | Pre-1.0, single-maintainer; last release was 8 months before this research despite active issue tracker — much lower release cadence than UniFFI in the same window (chinedufn, n.d.) | Sole purpose is Rust↔Swift; more direct/lower-overhead codegen than UniFFI by design, but a much smaller ecosystem |
| **cbindgen** | 0.29.4, released 2026-06-09 (Lib.rs, n.d.-c) | Mozilla | High download volume (~5M/month), used in 953 crates, actively kept building for Firefox, but development is explicitly ad hoc — "features have been added to support the use cases of the maintainers" (Mozilla, n.d.-c) | Generates a C header only; Swift consumes it via a C target / module map — no Swift-specific ergonomics (no enums-with-payload, no Result, no async) |
| **cxx** | 1.0.199, released 2026-08-08 (Lib.rs, n.d.-d) | dtolnay (independent, high-trust maintainer) | Very actively released (monthly-ish cadence through 2026) | **Not applicable** — cxx targets Rust↔C++ interop specifically; it has no Swift backend and does not appear anywhere in real Swift-Rust bridging projects surveyed for this research |
| **Hand-rolled C ABI** | N/A | N/A | Used by libsignal (Signal), via a custom `bridge_fn` macro family in `libsignal-bridge` rather than any of the above (SignalApp, n.d.) | Full control, but every type mapping, every enum, every async pattern is written and maintained by hand |

**Dominant recommendation for Swift specifically (2024–2026 sources)**: UniFFI. Every real-world precedent found in this research that ships a Rust core to a native Swift shell and needs anything beyond primitive functions — Ferrostar (Stadia Maps, n.d.), the Matrix Rust SDK's `matrix-sdk-ffi` (Matrix.org, n.d.), assorted "bridging Rust and Swift" tutorials from 2025–2026 (Jonikorjk, 2026) — uses UniFFI, not swift-bridge or cbindgen alone. swift-bridge is positioned as the lower-overhead, more "manual" alternative for teams that want direct control and don't need UniFFI's multi-language story (Kotlin/Python/Ruby too), but it has materially less traction, a single primary maintainer, and (per §2.2 below) weaker async support. cbindgen is not a Swift bridging tool per se — it is the C-header-generation half of the hand-rolled-C-ABI approach, and both UniFFI and swift-bridge use cbindgen-adjacent C-shim generation under the hood rather than asking a project to hand-write headers. **Signal is the one prominent counter-example**: libsignal deliberately does not use UniFFI, and hand-rolls its own `bridge_fn` macro layer across FFI/JNI/Node (SignalApp, n.d.). No public source found explains *why* Signal chose hand-rolling over UniFFI (UniFFI existed and was already Mozilla-backed by the time libsignal's bridge layer was built); this is flagged as an evidence gap rather than inferred as a design lesson.

**Community-opinion note**: no source treats this as a live 2024–2026 debate. The unanimity is unusual for a "vs." question — UniFFI has effectively become the default answer for "how do I call Rust from Swift with a real API surface," and disagreement in the literature is about UniFFI's own rough edges (Swift 6 concurrency, breaking changes between releases), not about whether to use it over the alternatives.

### 2.2 Rust `async fn` as Swift `async/await` — which tool, which version, what runtime bridging

**Yes, today, via UniFFI**, and has been possible since UniFFI added general async support (tracked from Mozilla/uniffi-rs issue #1054 onward) (Mozilla, n.d.-d). Mechanism, as documented and corroborated by a working example (Terhechte, 2024) and the UniFFI internals docs (Mozilla, n.d.-e; Mozilla, n.d.-f):

- A Rust `async fn` is wrapped by UniFFI into a `uniffi::RustFuture`.
- UniFFI generates scaffolding so the **foreign side supplies the executor** — i.e., Swift drives the future to completion, not Rust's own runtime, at the FFI-callback level (Mozilla, n.d.-e).
- On the Swift side, the generated binding is a native `async throws` function. Internally it uses a `CheckedContinuation`: UniFFI polls the Rust future via a `rust_future_poll_rustbuffer`-style callback, and when the future resolves, the continuation resumes the suspended Swift `Task` — without blocking the Swift main thread (Terhechte, 2024).
- **Underneath the Rust future itself, a real async runtime (in every real-world example found, Tokio) still has to drive Rust-side polling and any Rust-side I/O** — UniFFI's foreign-executor model governs how the *result* gets back to Swift, not how the Rust future internally makes progress. In practice this means a project embeds a Tokio runtime in the Rust core (see §6.1/§6.3) and the async-fn's body runs on Tokio, while UniFFI's plumbing is what surfaces completion to the Swift `Task`.

**swift-bridge**, by contrast, has materially thinner async support: async is gated behind an opt-in `"async"` Cargo feature that pulls in `tokio` and `once_cell`, arguments to async Rust functions are **not supported** (only no-argument async fns), and only primitive types and shared structs are supported as async return types (chinedufn/swift-bridge PR #31, as summarized in search results) (chinedufn, n.d.). This is a clear practical gap versus UniFFI for a project whose core interaction (`start_scan(item_id) -> Measurement`) needs argument-carrying async calls.

**Version**: current UniFFI 0.32.0 (2026-06-30). **Caveat, documented not inferred**: UniFFI's own docs state async code does not fully conform to `Sendable` under Swift 6 strict concurrency as of the 0.28–0.32 line — see §2.4/§6.5.

### 2.3 Modeling continuous event streams across the boundary (the `AsyncStream`-over-vendor-SDK pattern)

There is no first-class "stream" primitive in UniFFI's public UDL/proc-macro surface as of 0.32.0. The documented and real-world patterns, in descending order of how much production precedent backs them:

1. **Foreign trait / callback interface implemented in Swift, invoked repeatedly from Rust, wrapped into `AsyncStream` on the Swift side.** This is the dominant real pattern. UniFFI supports exporting a Rust `trait` so that the *foreign* language implements it (`#[uniffi::export(with_foreign)]` / `Arc<dyn Trait>` in UDL) (Mozilla, n.d.-g). Ferrostar (a shipped, non-trivial cross-platform navigation SDK) uses exactly this shape for continuous location updates: a `LocationProvider`-style trait is defined in Rust, Swift supplies the concrete implementation, and Rust calls into it synchronously each time a new value is available (Stadia Maps, n.d.). The Swift side then owns turning repeated synchronous callback invocations into an `AsyncStream` via `AsyncStream.Continuation.yield(_:)` — this half is plain Swift, not UniFFI-generated, and is the idiom used across the broader "Rust core + Swift shell" ecosystem for exactly this reason (inferred from the callback-interface documentation plus the Matrix Rust SDK's equivalent `TimelineDiff` listener pattern used by `matrix-rust-components-swift` (Matrix.org, n.d.)).
2. **Async callback/trait methods** (UniFFI's async-callback-interface machinery): a callback interface method can itself be `async` from the Rust caller's perspective, backed by a `complete_func` + opaque data pointer + a `foreign_future_dropped_callback` for cleanup (Mozilla, n.d.-e). This is lower-level plumbing that the trait-callback pattern above is typically built on, not something app code touches directly.
3. **Polling** is explicitly what UniFFI's *internal* future-completion machinery does (`rust_future_poll_rustbuffer`) (Mozilla, n.d.-e), but is not a recommended *application-level* pattern for a hardware event stream — the zero-trust-iOS-architecture writeup explicitly frames polling (vs. server push/stream) as worse for exactly the reasons that would matter here: response latency and needless overhead, preferring a stream/callback approach instead (Weirich, cited via search synthesis; primary article inaccessible, see Question Status) — **flagged: inferred/secondary**, this specific comparison could not be independently verified against the primary source due to a 403 on the Medium article; treat the general "callback/stream beats polling" framing as directionally supported by UniFFI's own architecture (native callback support exists; polling is an implementation detail, not exposed as the app-level idiom) rather than as a fully documented citation.

**Crate names/versions for the underlying Rust-side plumbing**: `uniffi` 0.32.0 for the FFI layer itself; `tokio::sync::mpsc` (bundled in `tokio` 1.53.1, current stable as of 2026-08 — Lib.rs, n.d.-e) as the idiomatic Rust-side channel feeding the callback trait from a background scan task (general Rust actor-channel pattern, not UniFFI-specific — see §6.2).

**Concrete recommended shape for this project's `AsyncStream`-over-vendor-SDK boundary**: Rust core exposes a `uniffi::export(with_foreign)` trait (e.g. `ScanEventSink`) that Swift implements; the Swift implementation's methods simply do `continuation.yield(event)` inside a Swift-native `AsyncStream { continuation in ... }` initializer, preserving the exact `AsyncStream` surface the architecture research already specced, with Rust now the event producer instead of the vendor SDK directly. This is an architectural inference grounded in the documented UniFFI foreign-trait mechanism plus the Ferrostar precedent, not itself a published "SpectroCapture-shaped" example — flagged as inferred.

### 2.4 Type-mapping limits: enums with associated values, `Result`/`throws`, struct graphs, zero-copy byte buffers

- **Enums with associated data**: supported, but historically constrained by UDL's WebIDL lineage — "Enumerations with associated data require a different syntax, due to the limitations of using WebIDL as the basis for UniFFI's interface language" (Mozilla, n.d.-h). Only enums with **named fields** are supported in that older UDL syntax; the proc-macro (`#[derive(uniffi::Enum)]`) path is more flexible (Mozilla, n.d.-h). **Nested enums are explicitly awkward** — richer structured error hierarchies (e.g., an error enum whose variant itself carries another enum) run into real friction, documented in open issues (Mozilla, n.d.-i). For this project's raw-payload/derived-value model this matters most for anything like `ScanResult::GamutClipped(ClipReason)`-shaped types — flatten one level deep rather than nesting.
- **`Result`/`throws`**: first-class. A Rust error type that is an `enum` implementing `std::error::Error` maps directly to a Swift `throws` function whose thrown type conforms to `Error`, including field-carrying error variants via `[Error] interface` syntax (Mozilla, n.d.-h).
- **Struct graphs**: supported directly as UniFFI `Record`/`Object` types; nested structs and `Vec<T>`/`Option<T>` fields map cleanly to Swift structs/classes and `[T]`/`T?`. No specific breakage found in 2026 sources for plain data-graph shapes (moderate confidence, not independently stress-tested for this brief).
- **Zero-copy byte buffers**: UniFFI added zero-copy transfer of `&[u8]` (`[ByRef] bytes` UDL arguments) from foreign code **into** Rust — the Rust-side signature changed from `&Vec<u8>` to `&[u8]` to enable this (Mozilla, n.d.-j, changelog). Critically for this project: **this optimization is documented for the Kotlin binding specifically** (Kotlin call sites must switch to `java.nio.ByteBuffer`); the same changelog entry states Swift (`Data`) and Python (`bytes`) call sites are **unchanged**, implying the zero-copy win is not yet exposed as a Swift-visible API change — the Swift side likely still goes through UniFFI's standard `RustBuffer` serialization for byte payloads as of 0.32.0. **Flagged: partially inferred.** This is directly relevant to the measurement-payload path (raw instrument bytes as the canonical record) and should be prototyped, not assumed, before committing to shape A for payload transfer — see Question Status follow-up.

### 2.5 Canonical build integration: Rust static library inside SPM/Xcode, universal binaries, incremental-build breakage

**Canonical pipeline** (converged across UniFFI's own docs and multiple independent tutorials/production writeups — Mozilla, n.d.-k; Mozilla, n.d.-l; Stadia Maps, n.d. Part 2):

1. Cross-compile the Rust crate for each needed target triple (for a macOS-only universal build: `aarch64-apple-darwin` + `x86_64-apple-darwin`).
2. Run `uniffi-bindgen` (or the proc-macro-driven `uniffi-bindgen-swift` for the newer, UDL-free workflow) to generate the Swift source + C header + module map.
3. Fuse per-architecture static libs into a universal (fat) binary with `lipo`, or keep them as separate XCFramework slices.
4. Package as an **XCFramework** via `xcodebuild -create-xcframework`, bundling the compiled static library, the C headers, and the Swift module map (Stadia Maps, n.d. Part 2; Mozilla, n.d.-l).
5. Reference the XCFramework as a **binary target** in `Package.swift`, with a companion Swift-source target providing the ergonomic wrapper API.

**Tooling that automates this**: `cargo-swift` (community plugin, `antoniusnaumann/cargo-swift`) provides `cargo swift init` / `cargo swift package` to do steps 1–4 without hand-writing build-phase scripts, built specifically around UniFFI (antoniusnaumann, n.d.). It has had migration friction across UniFFI point releases (e.g., adapting to UniFFI 0.31's changes) — evidence the toolchain is not yet fully stable release-to-release (GitHub issue search, Mozilla/antoniusnaumann trackers).

**What breaks in incremental builds**: not independently measured in this research; the sources surveyed (Stadia Maps Part 2, UniFFI's Xcode-integration doc) describe the happy-path pipeline and note general complexity ("the process of generating bindings for Swift and Kotlin, integrating this into your build tooling, and packaging everything into a usable SPM / Maven package is also quite complex" — Stadia Maps, n.d. Part 1) but do not quantify incremental-build regressions or specific cache-invalidation failure modes. **Flagged: unanswered with specificity** — see Question Status.

### 2.6 Debugging story: lldb over mixed Swift/Rust frames, crash-report symbolication

- **lldb across mixed frames**: no source found describes a turnkey "step from Swift into Rust and back" lldb workflow specific to the UniFFI/swift-bridge shape. What is documented: Rust code compiled for macOS/iOS can be debugged directly in Xcode's lldb via a community Xcode plugin plus `cargo-xcode` to scaffold an `.xcodeproj`, with the usual breakpoint/scheme caveats (adding the Xcode UUID to a plist, deployment-target settings) (Barrows, n.d.). This is debugging **pure Rust** in Xcode, not documented evidence of seamlessly stepping across the Swift↔Rust FFI edge within one lldb session. **Flagged: gap** — no source verifies that a single lldb session can step from a Swift call site through the generated C shim into Rust source with correct DWARF resolution on both sides; this is architecturally plausible (both sides compile to native Mach-O with DWARF, and lldb is a general native debugger, not Swift/ObjC-specific) but not documented as demonstrated practice for a UniFFI project. 1Password's public writeup, the closest "shipped Rust-core-plus-Swift-shell" account with self-reported debugging characteristics, states only that Rust "requires very little runtime debugging compared to other languages" and that Rust's own type-sharing macros reduce cross-language type drift — it does not describe an lldb-specific cross-boundary debugging workflow (Serokell, n.d.).
- **Crash-report symbolication**: standard macOS symbolication tooling (`atos`, `dsymutil`, Sentry's `sentry-cli debug-files upload`) operates on Mach-O binary images plus dSYM bundles and is language-agnostic — it resolves addresses to symbols using DWARF, regardless of whether the originating compiler was `clang`, `swiftc`, or `rustc` (general documented macOS crash tooling behavior — Sentry, n.d.; general atos/dSYM tooling docs). Rust on Apple targets can emit a dSYM (via `-Csplit-debuginfo=packed`, the default packaging mode `dsymutil` also handles for cdylibs) so this is expected to work, but there is a known edge case: `rustc --test` has historically produced a zero-size dSYM bundle on macOS (rust-lang/rust issue #45768), and Sentry's own Rust SDK issue tracker has open reports of release-profile Rust panics missing stack-trace/debug_id info sent to their backend (getsentry/sentry-rust issue #354). **Net assessment**: mechanically compatible, but not friction-free in the release-optimized configuration this project would actually ship — flagged as partially inferred from adjacent evidence rather than a direct "here is how SpectroCapture-shaped app X symbolicates Rust+Swift crashes" account.

---

## 3. Vendor SDK Integration (Fixed Constraint)

_Research conducted 2026-09-01. Primary sources: Nix Sensor's own published SDK, its DocC documentation archive (extracted from the public `nixsensor/nix-universal-sdk-ios-dist` GitHub repository, which is the actual binary-distribution channel for the SDK the product vision names), and current crates.io/GitHub state for objc2, swift-bridge, and btleplug._

**A note on source quality up front, because it matters for every answer below:** Nix Sensor does not publish a narrative "architecture" document, so several answers below combine two kinds of evidence: (1) **documented** facts pulled directly from the SDK's own DocC reference pages, its `Package.swift`/podspec, and its distributed example projects — these are marked explicitly; and (2) **inferred** conclusions I draw from that documented evidence plus general knowledge of the Rust/Swift FFI ecosystem — also marked explicitly. Where I inferred, I say so and give the reasoning, per the brief's instruction that a proprietary SDK will not be transparent about its internals.

---

### Q1. Viable patterns for a Rust core to drive a Swift-only SDK — and what that does to the "Rust as primary language" claim

**Documented fact, and the single most important finding in this section:** the vendor SDK is *not* Swift-only in practice. Nix Sensor's own SDK distribution ships a second, C-ABI integration path they built and support themselves. The SDK's "Wrapper" documentation page states: "For macOS applications that require a C/C++ interface, it may be possible to use NixUniversalSDK indirectly... the SDK download provides a project `example-macos-wrapper` which allows for its usage in C/C++ applications" (Nix Sensor Ltd., 2026d). That example project is a small Swift/Xcode target that holds single `DeviceScanner` and `DeviceCompat` instances and exposes them as C functions with C callbacks; building it (`sh build.sh -r`) produces three artifacts: `libNixUniversalSDK-Wrapper.dylib`, a plain C header `NixUniversalSDK-Wrapper.h`, and the base `NixUniversalSDK.framework` which the dylib loads at runtime via `dlopen` (Nix Sensor Ltd., 2026d). This is documented, not inferred — Nix Sensor built and ships this wrapper precisely so C/C++ (and by extension Rust) applications don't have to speak Swift or Objective-C to the SDK at all.

Independently, the SDK's primary Swift API is also **Objective-C-compatible**, not Swift-only in the strict sense that would block `objc2`. The DocC reference gives an explicit Objective-C usage example — `#import <NixUniversalSDK/universalsdk.h>` — alongside every Swift example, for classes (`DeviceScanner`, `LicenseManager`), protocols (`ScannerStateDelegate`, `DeviceStateDelegate`), enums (`DeviceScannerState`, `DeviceStatus`), and closures/blocks (`DeviceFoundCallback` in Swift maps to `DeviceCompatBlock` in Objective-C) (Nix Sensor Ltd., 2026c). A framework that generates a usable Objective-C compatibility header is, by construction, using `@objc`-exposed types throughout its public surface — the exact precondition `objc2`'s own documentation states for interop ("objc2 can interoperate with Swift code that exposes an Objective-C-compatible API using the `@objc` attribute... Automatically mapping Apple's Swift-only frameworks is out of scope for the objc2 project," docs.rs, n.d., cited in madsmtm, 2026).

So there are three viable patterns, not one, in descending order of directness:

1. **Bind the vendor's own C wrapper directly from Rust** (bindgen against `NixUniversalSDK-Wrapper.h`, link `libNixUniversalSDK-Wrapper.dylib`, `dlopen` the base `.framework` at startup per Nix's documented runtime requirement). No Swift toolchain participation in the Rust build at all, and no `objc2`/`swift-bridge` dependency. **Documented**, but the wrapper is explicitly an example project ("basic implementation, but can be modified or expanded to better suit your specific application," Nix Sensor Ltd., 2026d), not a first-party supported product — so this path means *owning and maintaining a fork of Nix's example wrapper* as if it were vendored code, which sits uneasily with the project's "never vendored" rule for the SDK itself (the wrapper is glue code around the SDK, not the SDK, so it likely doesn't violate the letter of that rule, but it is still Swift source the project must build, sign, and keep in sync with SDK upgrades — **inferred** consequence).
2. **Bind the SDK's Objective-C-compatible header directly with `objc2`** (hand-written `extern_class!`/`extern_protocol!`/`extern_methods!` declarations against `universalsdk.h`, using `block2` for the closure-typed callbacks). No wrapper dylib to maintain, but the bindings are hand-rolled against a third-party framework `objc2` doesn't ship pre-generated crates for (its `framework-crates` cover Apple's own frameworks; a vendor SDK needs bespoke bindings) — **inferred**, based on `objc2`'s documented scope (madsmtm, 2026) and the observed shape of Nix's public API.
3. **Keep a thin native Swift shim as the device layer, and cross the Rust↔Swift boundary with `swift-bridge` or a hand-rolled C ABI**, i.e., write a small first-party Swift package that imports `NixUniversalSDK` normally and re-exposes only the operations the app needs (connect, scan, license-activate) across an explicit boundary you control. This is the shape implied by the brief's own fallback framing ("if the device layer must stay in Swift").

**What staying-Swift does to the "Rust as primary language" claim, if pattern 3 is chosen:** it means the device layer — discovery, connection state, scan-result delivery, license activation, all delegate/callback handling — stays Swift code that must exist, compile, and be maintained regardless of which shape (A/B/C) is chosen elsewhere. In shape A that shim is small and isolated (mirrors the existing `SpectroDeviceLive` module boundary from the prior Swift-only architecture research), but it is still Swift, still requires Xcode, and still means every contributor who touches the device layer needs both toolchains. It does not by itself undermine "Rust as primary language" for the *application logic and UI* — but it does mean the project can never truthfully claim "no Swift" or "Swift only at the seam is optional"; some Swift (or a maintained wrapper artifact, per pattern 1) is a structural requirement of this vendor SDK for as long as the SDK stays Swift/ObjC-only. **This framing is my synthesis of the documented facts above, not a claim Nix Sensor makes.**

---

### Q2. 2026 maturity of calling Swift/ObjC frameworks *from* Rust (objc2 family, swift-bridge reverse direction): versions, limits, real usage

**objc2 family** — actively maintained, individual-maintainer project (`madsmtm`), not corporately backed but well-used (1,023 GitHub stars as of this research; repository last pushed 2026-08-27, i.e., days before this research — madsmtm, 2026). Current versions on crates.io as of 2026-09-01:
- `objc2` **0.6.4** (published 2026-02-26) — core Objective-C runtime/interop crate (crates.io, 2026a).
- `objc2-foundation` **0.3.2** (published 2025-10-04) — Foundation framework bindings (crates.io, 2026b).
- `block2` **0.6.2** (published 2025-10-04) — bindings for Apple's Blocks C-language extension, i.e., Objective-C closures (crates.io, 2026c). This is the piece that matters most for the Nix SDK specifically: its documented callback types (`DeviceFoundCallback`/`DeviceCompatBlock`) are Swift closures that bridge to Objective-C blocks, and `block2` is the documented mechanism `objc2` provides for receiving and constructing blocks from Rust.
- Apple-framework bindings ("framework-crates") are auto-generated from the actual Xcode SDKs and released roughly in step with Xcode updates (madsmtm, 2026) — but these only cover Apple's own frameworks (Foundation, CoreBluetooth, AppKit, etc.), **not** third-party binary frameworks like NixUniversalSDK. Binding a third-party ObjC-compatible framework means hand-writing the `extern_class!`/`extern_protocol!` declarations yourself against its header, which `objc2` explicitly supports as a pattern but does not automate for you today ("the plan for the future is to allow you to automatically map the Objective-C API... using bindgen" — not yet shipped, docs.rs via madsmtm, 2026).

**Documented limit, stated by the project itself:** "objc2 can interoperate with Swift code that exposes an Objective-C-compatible API using the `@objc` attribute... Swift-only types (structs, enums with associated values, generics) are not supported... Automatically mapping Apple's Swift-only frameworks is out of scope for the objc2 project" (docs.rs, n.d., via madsmtm, 2026). Applied to Nix's SDK: anything reachable through the `universalsdk.h` ObjC-compat header (classes, `@objc` protocols, `NS_ENUM`s, blocks) is fair game; if any part of the Swift-only surface (e.g., a Swift-native `struct`/generic type not represented in the ObjC header) is load-bearing, it would be unreachable from `objc2` and would force pattern 3 (thin Swift shim) for at least that part of the surface. I did not find documentation stating whether Nix's *entire* public API is ObjC-representable or only a subset — **this is a real gap, flagged below in Question Status.**

**swift-bridge** — the reverse direction (calling Swift from Rust) is documented as secondary to its primary, better-supported direction (calling Rust from Swift). The project's own issue tracker records a user explicitly noting the documentation "seems to focus heavily on calling Rust FROM Swift but not the other way around," having to resort to manual `_cdecl`/Tauri-linking workarounds with "no documented steps for compiling Swift code or generating the necessary header files" for the reverse path (chinedufn/swift-bridge, GitHub issue #103, n.d.). The book itself is explicitly unfinished: "The swift-bridge book is a work-in-progress with many chapters either sparse or empty" (chinedufn, n.d.). That said, the crate has continued shipping reverse-direction capability: current version **0.1.59** (published 2026-01-06), with the changelog for that release adding "Swift 6 compatibility for async function code generation" (chinedufn/swift-bridge commit history, 2026) — meaning Swift closures/implementations passed into Rust, and calling `async`/`throws` Swift functions from Rust, are actively developed, just still pre-1.0 and thinly documented. **Maintenance signal worth flagging for the ADR:** the repository's last push was 2026-01-06 — no commits in the roughly eight months leading up to this research (chinedufn/swift-bridge, GitHub, 2026), versus `objc2`'s push four days before this research. `swift-bridge` also carries 101 open issues on a single-maintainer project (chinedufn/swift-bridge, GitHub, 2026). The dominant community position as of 2024–2026, based on this evidence plus the brief's own framing of Section 2 (UniFFI as the more actively backed choice for Rust↔Swift generally): `swift-bridge`'s Swift-from-Rust direction is the least mature part of an already less institutionally-backed tool, and teams needing to call into an existing Swift/ObjC binary framework more often reach for `objc2` directly (treating the framework as an Objective-C dependency) than for `swift-bridge`'s reverse-calling feature, which is designed more for a Rust core deliberately delegating small pieces of *new, first-party* logic to Swift than for driving an existing vendor SDK.

**Real usage:** I did not find a shipped, production macOS/iOS app that documents driving a third-party binary Objective-C/Swift SDK (as opposed to system frameworks or first-party Swift code) via `objc2` or `swift-bridge`'s reverse direction. `objc2`'s own framework-crates are the most visible large-scale usage, but those bind Apple's own frameworks, which is a materially different task (the bindings are machine-generated from Apple's own SDK headers, not hand-written against a vendor's shipped header). **This is a gap** — see Question Status.

---

### Q3. If the device layer stays Swift in shape A: cleanest ownership split so `SpectroDevice` (protocol + mock/live) still lets CI and contributors exercise the Rust core with no hardware and no license key

The prior architecture research already designed the right shape for an all-Swift app, and it transfers directly: a three-module split — `SpectroDevice` (protocol + value types + errors, no vendor SDK import), `SpectroDeviceMock` (a shipped, user-selectable "Simulated Spectrophotometer," no vendor SDK import), `SpectroDeviceLive` (the only module that imports the vendor SDK package) — with SwiftPM's import-graph enforcement making it a **compile error** for any other module to reach the vendor SDK (`macos-swiftui-app-architecture-research-results.md`, this repository, §"Module Boundaries," cited internally). That mechanism — a package-manager-enforced import boundary, not a convention — is exactly what needs to be reproduced on the Rust side of a shape-A boundary, because Cargo's workspace system provides an equivalent, documented lever:

- Define the device abstraction as a **pure-Rust trait** in a crate with zero Swift/FFI/vendor dependencies (the Rust-side analogue of `SpectroDevice`) — e.g., `spectro-device` exposing `trait SpectroDevice { fn scan(&self) -> impl Stream<Item = Result<Reading, ScanError>>; ... }`.
- Implement it twice: `spectro-device-mock`, pure Rust, no FFI, ships in the default build and is what CI, contributors without hardware, and `cargo test` exercise by default; and `spectro-device-live`, the only crate that links the Swift/ObjC bridge (whichever of the three Q1 patterns is chosen) and therefore the only crate that can reach the vendor SDK, the BLE/USB transport, and the license key.
- Gate `spectro-device-live` out of CI and out-of-the-box builds the way Cargo workspaces are designed to: either exclude it from `[workspace] default-members` (Cargo Book, n.d.) so `cargo build`/`cargo test` at the workspace root never touches it, or put it behind a Cargo feature flag (e.g., `hardware`) that is off by default and that CI never enables. Either mechanism means a contributor — or CI — can build and test the entire Rust core plus the mock device with no Xcode, no vendor XCFramework, and no license key present at all, which is the same guarantee the SwiftPM import boundary gives today, just enforced at the crate-graph level instead of the module-import level. **This is my design recommendation, synthesizing the documented Cargo workspace mechanism with the prior architecture research's already-validated module shape — it is not something I found stated for this exact scenario in any source**, since no shipped precedent for a Rust-core-behind-a-vendor-hardware-SDK app surfaced in this research (see Q1 in Concern Area 1, which is out of scope for this section but bears on the same gap).
- Whichever Q1 pattern is chosen for `spectro-device-live`'s internals, the license key and any SDK activation call belong exclusively inside that crate (or, for pattern 3, inside the Swift shim it wraps) — never in `spectro-device`, `spectro-device-mock`, or the Rust core/UI layers — so the same crate boundary that keeps hardware access out of CI also keeps the license key out of every artifact CI builds and out of the mock's code path entirely.

---

### Q4. Is direct Rust BLE (btleplug + CoreBluetooth) relevant given the vendor SDK owns the transport, or only for a hypothetical multi-instrument future?

**Not relevant to driving the Spectro 2/Spectro L today, and likely counter-productive if attempted.** Documented evidence: Nix Sensor's own SDK owns BLE discovery, connection, and — critically — a licensing/authorization handshake tied to the transport layer. The DocC "Discovering & Connecting" page documents that at connection time "the SDK will read an allocation code stored on the Nix device and compare to the license information... If this check does not pass, the SDK will contact a Nix authentication server to check if that device serial number is authorized... The internet connection is required only once every 30 days – once authorized, this status is saved, and connections can be made offline for this time period" (Nix Sensor Ltd., 2026c). That authorization flow is internal to the SDK's own BLE/CoreBluetooth session; there is no documented path to perform discovery/connection with a separate BLE stack (e.g., `btleplug`) and then hand the connection to the SDK, nor would that be expected to work, since the SDK needs to own the CoreBluetooth session to run its own authorization exchange with the device. Corroborating (**inferred, not directly documented for this exact SDK version**) evidence: Nix Sensor's own GitHub organization maintains a fork of `SwiftyBluetooth` ("Closures based APIs for CoreBluetooth") in its `nixsensor` org (GitHub, 2026), consistent with — though not proof of — the SDK's Swift/macOS implementation using CoreBluetooth directly rather than a third-party abstraction, which further suggests two independent CoreBluetooth central-manager sessions (one from `btleplug`, one from inside the vendor SDK) would be running in the same process for no benefit, and macOS's `CBCentralManager` model does not cleanly support that kind of duplication for the same peripheral. `btleplug` itself is mature and actively maintained — current version **0.13.0**, published 2026-08-31 (one day before this research), 1M+ all-time downloads, with a CoreBluetooth backend shared between macOS and iOS and reported as stable, including full device discovery, GATT, characteristics, descriptors, and notifications support on macOS ≥ 10.15 (crates.io, 2026d; deviceplug/btleplug, GitHub, n.d.) — but its README documents no guidance on coexisting with another framework's CoreBluetooth session, because that isn't a scenario the library is designed around (deviceplug/btleplug, GitHub, n.d.).

`btleplug` only becomes relevant under the vision's explicitly out-of-scope hypothetical: a future non-Nix instrument that ships no vendor SDK of its own and exposes a plain BLE GATT profile the app would need to speak directly. Given the product vision fixes "one device at a time" via the Nix vendor SDK and treats multi-instrument support as unscoped, this is a **non-issue for v1** and should not factor into the shape A/B/C decision.

---

### Q5. Does a Rust core introduce new constraints on runtime license-key activation and on keeping the key out of the binary and repo?

**Documented license mechanics, which apply identically regardless of which shape is chosen** (Nix Sensor Ltd., 2026c):
- Activation is per-process, not persisted: "License activations remain valid within a single session, but do not persist across different application launches. Therefore, it is required to call [`LicenseManager.Activate`] at least once per session," passing two strings (`options`, `signature`) — this is the credential pair that must never ship in source or in the repo.
- The activation call itself is synchronous-looking but its result is a `LicenseManagerState` enum that must be checked; device operations are unavailable unless the state indicates success.
- Separately, device *authorization* (as opposed to SDK license activation) requires an internet connection only once every 30 days per device, cached thereafter — matching this project's own vocabulary for a "pre-authorization window" (`AGENTS.md`, §8, this repository).
- macOS entitlements the app needs regardless of language, per Nix's own "Add to Xcode" guide: `com.apple.security.device.bluetooth`, `.device.serial`, `.device.usb`, and `com.apple.security.network.client` ("Required for SDK authentication (once per 30 day period)") (Nix Sensor Ltd., 2026e).

**What a Rust core changes about this, concretely:**
- **No new constraint on where the key can live** — the `options`/`signature` strings are opaque application-level secrets regardless of the calling language; keeping them out of the binary and repo (e.g., injected via a build-time secret, a user-supplied config file, environment variable, or Keychain item read at runtime) is exactly as feasible from Rust as from Swift, and the mechanism the project already uses for this class of problem — `gitleaks` scanning plus a documented allowlist for false positives (`AGENTS.md`, §3, this repository) — is language-agnostic and needs no change.
- **A new surface-area constraint, however, is real and specific to shape A/pattern-1-or-2 from Q1: whichever crate calls `LicenseManager.Activate` becomes the one place the key transits the FFI boundary.** If the key is read on the Rust side (e.g., from a config file the Rust core already owns) and must cross into the Swift/ObjC call, it now passes through whichever bridge is chosen (`objc2`, `swift-bridge`, or the C wrapper's parameter marshalling) as a plain string argument — no documented encryption or obfuscation is applied by any of these bridging tools, so the crossing itself adds no protection and no new risk beyond "one more place a plain-text secret sits in process memory momentarily," which is not materially different from the existing all-Swift design. **This is inferred from the general shape of FFI string-marshalling in all three candidate tools — none of them documents special handling for secrets, because none of them has any concept of a "secret" type.**
- **The Q3 module-boundary design is the load-bearing mitigation, not the choice of bridge.** As long as only `spectro-device-live` (never `spectro-device`, `spectro-device-mock`, or the Rust core/UI) reads or holds the key, CI — which never builds `spectro-device-live` — structurally cannot leak the key even if it were accidentally checked in for local development, because CI's build graph never touches that crate. This is the same guarantee Q3 already established for hardware access generally; license-key handling is a special case of it, not a separate problem.
- One documented operational wrinkle worth surfacing for the ADR: because SDK-level license activation does **not** persist across launches (Nix Sensor Ltd., 2026c), the Rust core (or its Swift shim) must re-supply the `options`/`signature` pair on every app start, meaning whatever secret-storage mechanism is chosen must support fast, offline, no-user-interaction retrieval at every launch — a Keychain item read is the natural fit on macOS regardless of which language does the reading, and nothing about Rust changes that recommendation.

---

## 4. Rust-Native UI Options (Shape B)

_Research pass: 2026-09. Scope: egui, Slint, Tauri, Dioxus, gpui, plus credible newcomers (Xilem, Floem, Iced), evaluated strictly on macOS fitness for SpectroCapture (a color-fidelity-critical, table-heavy, hardware-paced desktop app)._

---

### Q4.1 — Version, license, macOS maturity, shipped-app precedent, per framework

#### Comparison table

| Framework | Latest version (2026-09) | License | Renderer / windowing | macOS maturity | Shipped macOS app(s) |
|---|---|---|---|---|---|
| **egui** | egui/eframe 0.36.1 (2026-08-07) (docs.rs, n.d.-a; docs.rs, n.d.-b) | Dual MIT / Apache-2.0 (emilk, n.d.-a) | egui_wgpu (default) or egui_glow, over winit | Runs out-of-the-box on macOS via wgpu/winit (docs.rs, n.d.-a); described as "the quickest way from zero to a window" but "looks like a debug UI" (Wren Learns Rust, 2026) | No large commercial flagship shipped macOS app found in this pass; used widely for internal tools/debug UIs (Rerun's viewer is the most cited egui-based shipped tool, cross-platform including macOS) — **inferred/indirect**, not a first-party confirmation of a marquee macOS release |
| **Slint** | 1.17.1 (2026-07-07) (docs.rs, n.d.-c) | Tri-license: GPLv3, a free "Royalty-free" license (attribution required), or paid Commercial (Slint, n.d.-a) | FemtoVG (default) or Skia (opt-in), over winit | Officially supports Windows/macOS/Linux desktop from one codebase (Slint, n.d.-b) | WesAudio ships commercial audio-plugin/hardware-companion products built with Slint (Slint, n.d.-b) — cited by Slint's own marketing; **not independently corroborated** in this pass |
| **Tauri** | tauri crate 2.11.5 (docs.rs, n.d.-d); 2.10.1 stable line reported 2026-03-04 (WebSearch synthesis, 2026) | MIT / Apache-2.0 (docs.rs, n.d.-e; Wikipedia, n.d.) | macOS: WKWebView (system WebKit) driving HTML/CSS/JS UI, with a Rust backend | Most mature of the group by ecosystem size and release cadence (Wren Learns Rust, 2026); on macOS it rides system WebKit, so web-standard rendering fidelity (incl. color) is WebKit's, not Tauri's own | Widely used by small-to-mid commercial apps; no first-party marquee example verified in this pass with primary-source confirmation — **inferred from ecosystem size claims**, not a named, sourced flagship |
| **Dioxus** | dioxus crate 0.7.10 (docs.rs, n.d.-f); desktop backend still wraps wry/webview for the mainstream desktop target, with a separate experimental native renderer (`dioxus-native`/Blitz) | MIT / Apache-2.0 (DioxusLabs, n.d.-a) | Desktop target: wry (webview) by default; native target: Blitz (HTML/CSS engine over wgpu/Vello), with AccessKit + Muda via `blitz-shell` (DioxusLabs, n.d.-b) | Native (webview-free) path is explicitly pre-production: Blitz was targeting "broadly usable beta" by end of 2025 and "production-ready ... sometime in 2026" (WebSearch synthesis of DioxusLabs sources, 2026); native menu/multi-window/titlebar support was still an open feature request as of the most recent tracked issue (DioxusLabs/dioxus Issue #3855, cited 2025-03) | No shipped flagship macOS app identified using the native (non-webview) path; the webview-mode desktop target inherits Tauri-like webview precedent generically, not app-specific |
| **gpui** | gpui crate 0.2.2 (crates.io, n.d.) — pre-1.0 | Apache-2.0 (crates.io, n.d.) | Bespoke GPU-accelerated retained+immediate hybrid renderer, Metal on macOS (gpui.rs, n.d.) | Built for and hardened almost exclusively by Zed; documented as usable standalone but with a small, largely Zed-internal ecosystem (gpui.rs, n.d.; longbridge/gpui-component, n.d.) | **Zed editor** — the only large, well-known shipped macOS app (Zed Industries, n.d.) |
| **Xilem** (newcomer) | Actively developed, alpha (linebender/xilem, n.d.) | Apache-2.0 / MIT (Linebender convention) | Masonry (retained widget tree) + Vello/wgpu for 2D, Parley/Fontique for text, winit for windowing, AccessKit for a11y (linebender/xilem, n.d.) | Explicitly alpha; no shipped macOS app identified | None found |
| **Floem** (newcomer) | No tagged release in ~2 years as of this pass; recommend tracking `main` (Lapce/Floem docs, n.d.) | MIT (implied by ecosystem norm; not independently confirmed in this pass — **inferred**) | wgpu-based custom renderer, single-maintainer project (lap.dev/floem, n.d.) | Supports Windows/macOS/Linux per docs but "maintained by a single person," API diverging fast from the last tagged release (lap.dev/floem, n.d.) | Powers the Lapce code editor (cross-platform, macOS included) — **inferred** from Floem's own project lineage, not independently verified here |
| **Iced** (newcomer, brief did not name it but it is the field's other major non-webview option) | 0.14 released 2026 (Hacker News, 2026, discussing the release) | MIT (iced-rs convention; not independently re-verified this pass) | Custom wgpu/tiny-skia renderer, winit | One of only three non-webview, cross-platform, actively maintained Rust GUI stacks as of 2026 per a contemporaneous survey (alongside egui and Slint) (boringcactus, 2025, updated framing repeated in later 2026 coverage) | No shipped flagship macOS app identified in this pass |

**Framing note on "shipped precedent":** Only two frameworks in this table have an unambiguous, well-known, large shipped macOS application: **gpui → Zed**, and generically **Tauri/Dioxus-webview → many small commercial apps** (webview-based, so precedent is diffuse rather than framework-specific). egui, Slint, Xilem, Floem, and Iced lack a comparably prominent, independently-verifiable flagship macOS release; claims of "production apps" for Slint (WesAudio) and Floem (Lapce) come from the frameworks' own marketing/lineage and should be treated as **framework-vendor-asserted, not independently confirmed**.

---

### Q4.2 — VoiceOver, native menu bar, standard shortcuts, multi-window: which support them, at what fidelity

| Framework | VoiceOver / AccessKit | Native menu bar (macOS top bar) | Standard shortcuts | Multi-window |
|---|---|---|---|---|
| **egui** | Optional AccessKit integration, enabled by default in eframe, implementing native accessibility APIs on Windows and macOS (GitHub PR #2294, emilk/egui, merged; HN discussion, 2023). Fidelity is coarse: AccessKit exposes egui's semantic widget tree, but **custom-painted content (the exact pattern needed for a color-swatch grid) is not automatically accessible** — it must be manually annotated with AccessKit node roles, which is easy to omit and easy to get subtly wrong. A contemporaneous cross-platform survey (tested on Windows Narrator, not VoiceOver) called the screen-reader experience "a little janky" (boringcactus, 2025) — **this is Windows Narrator evidence extrapolated to describe general framework maturity, not a direct macOS VoiceOver test; flagged as indirect for the macOS-specific claim.** | Not built in. Native macOS menu bar integration is an open feature request in eframe (GitHub Issue #3411, emilk/egui) as of this pass; the documented workaround is pairing egui with the separate `muda` crate (0.17.1) for the menu and running egui inside e.g. `egui-tao`/winit for the window (GitHub Discussion #3293, emilk/egui). This is glue work the app would own, not a framework guarantee. | Inherits winit's keyboard event model; no egui-specific claim of full macOS shortcut-convention conformance (e.g., Cmd-based accelerators, text-field emacs bindings) found in this pass. | Supported via egui's "multi-viewport" system, where each viewport maps to one native OS window, implemented over egui_winit (DeepWiki summary of membrane-io/egui, citing egui's own multi-viewport docs, 2026); demonstrated in egui_glow's `multiple_windows.rs` example (emilk/egui repo). |
| **Slint** | AccessKit-backed accessibility, feature-gated (must be enabled) (Slint docs, n.d.-c). An open issue as of this pass calls out that **text-input widgets specifically lack full accessibility support** (GitHub Issue #2895, slint-ui/slint) — directly relevant since SpectroCapture will need accessible text fields for inventory metadata and item search. No macOS-specific VoiceOver fidelity report was found; a cross-platform survey (again Windows Narrator, not VoiceOver) reported "Narrator works perfectly" for Slint (boringcactus, 2025) — **indirect evidence for macOS.** | Declarative `MenuBar` element inside a `Window`, documented as producing the platform's native menu bar (Slint docs, n.d.-d). This is the most first-class native-menu story of the non-webview frameworks in this table. | Not independently verified for macOS-convention shortcuts in this pass — **gap**, see Question Status. | Slint's docs describe a `Window`-per-instance model consistent with multi-window desktop apps; no macOS-specific multi-window maturity report found in this pass — **inferred from general desktop-platform docs, not confirmed with a citation.** |
| **Tauri** | Inherits **WKWebView's** built-in accessibility bridge to VoiceOver — real web-standard HTML gets a real AX tree essentially for free, which is a structural advantage over the immediate-mode / bespoke-renderer frameworks. However, WKWebView's accessibility elements are handled **out-of-process** from the host app, and there are documented cases of the VoiceOver accessibility tree going **out of sync with WKWebView contents**, especially for asynchronously-loaded content (Apple Developer Forums thread #809541, n.d.; WebAIM, n.d.). Net: better default fidelity than the GPU-renderer frameworks for standard controls, but not a guarantee, and async-updated UI (like a live-updating capture queue) is exactly the failure pattern reported. | Full native macOS menu bar — Tauri exposes a first-class `Menu`/`Submenu` API that renders as the actual system menu bar on macOS, including icons on submenus since Tauri 2.8 (Tauri docs, n.d.-f; multiple community how-tos, 2026). This is the most mature native-menu story of any framework evaluated. | Native, since the window chrome and menu are AppKit-native even though content is WKWebView; standard Cmd-shortcuts work at the OS/menu level. Within the webview, shortcut handling is standard web `keydown` handling, which is well-understood but is the app's responsibility to map to macOS conventions. | Full native multi-window via Tauri's window/webview manager (Tauri docs, n.d.-g); this is a mature, first-class feature. |
| **Dioxus** | The default desktop target (wry/webview) inherits the same WKWebView accessibility bridge as Tauri, with the same async-sync caveats. The **native (Blitz) target** integrates AccessKit via `blitz-shell` (DioxusLabs/blitz repo, n.d.), but this path is explicitly pre-beta/pre-production as of this pass. | Muda-based system menu integration is present but Dioxus itself states there "currently aren't Dioxus abstractions over the menubar handling" beyond that, with fuller menubar/notifications/shortcuts hooks planned for an "upcoming release" as of the tracked feature-request issue (GitHub Issue #3855, DioxusLabs/dioxus, 2025-03). A **documented macOS-specific segfault** occurred when apps with menus were bundled on Apple Silicon/macOS 14 (GitHub Issue #1918, DioxusLabs/dioxus) — since reported fixed, but evidence of real macOS-menu fragility in this stack. | Standard web shortcut handling in webview mode; native-mode story tied to Blitz's maturity, not separately verified. | Multi-window exists for the webview target (spawning child wry windows) but a request for child-window support specifically for embedding native content inside a Dioxus desktop app was still an open issue as of the tracked GitHub thread (Issue #3086, DioxusLabs/dioxus) — **partial/immature.** |
| **gpui** | **No usable VoiceOver support today.** Zed's own maintainers and community state plainly: "although the Zed menus are accessible, there is no accessibility to the editing functions when using VoiceOver on macOS," and "none of the elements are read out by the screen reader upon launching the application" (zed-industries/zed Discussions #6576, #6714, #8146, n.d.). Zed acknowledges accessibility is a multi-year effort "likely lasting far beyond version 1.0" and that AccessKit integration into GPUI is planned but had not, as of this pass, been confirmed landed and shipped — a direct GitHub-PR-level confirmation was not found in this search pass (**gap; flagged in Question Status**). This is the single most disqualifying finding of this entire section for an app whose non-negotiables include accessible use. | Zed does have working native macOS menus (per the discussion threads above), so GPUI supports at least basic native menu bar integration as proven by Zed's shipped behavior — but this is evidenced by the one flagship app, not documented as a general framework guarantee. | Not separately documented; inherited from Zed's own keybinding system, which is itself a large bespoke subsystem, not a GPUI "batteries included" feature. | GPUI supports multiple native windows (Zed itself uses multiple project windows) — evidenced by the shipped app, not by a dedicated framework doc found in this pass. |

**Bottom line for Q4.2:** Tauri (and Dioxus in its webview mode) is the only stack in this table with **mature, first-class, native macOS menu bar + multi-window support out of the box**, because it borrows AppKit's real menu and window chrome. It also has the best *default* accessibility story because WKWebView content gets VoiceOver support "for free" — but with a documented out-of-sync failure mode under async updates that maps directly onto SpectroCapture's live capture queue. gpui has the worst accessibility story of the group by a wide margin — actively acknowledged as broken by its own primary shipped app's maintainers.

---

### Q4.3 — Wide-gamut / Display P3 rendering and ColorSync integration (highest priority)

This is the load-bearing question for the entire section, so it is treated in the most detail.

**The macOS platform baseline.** macOS itself is wide-gamut end to end: the whole OS compositor operates in a wide color space, and Apple's guidance for wide-gamut rendering assumes the app explicitly opts into a `DisplayP3`-tagged `CAMetalLayer` with a gamma-correct (`BGRA8Unorm_sRGB`) framebuffer to get linear-light blending and P3 range (Apple Developer Forums thread #111818, n.d.; Apple Developer Forums thread #724223, n.d.). **Correct wide-gamut rendering on macOS is opt-in per-surface, not automatic** — any renderer that doesn't explicitly tag its layer's color space, and doesn't do gamma-correct blending, will render in an implicit/unmanaged space that macOS will typically interpret as sRGB, silently clipping or mis-rendering P3 content. This is exactly the failure mode the brief is worried about.

**Concrete evidence that this is a live problem in exactly this class of renderer, not a hypothetical:** Rio, a Rust terminal emulator, documents that it had to **manually** configure a `DisplayP3`-tagged `CAMetalLayer` with a gamma-correct framebuffer specifically to get correct wide-gamut, linear-light alpha blending on macOS (Rio Terminal docs, n.d.). Rio is precedent that (a) it is *possible* to get this right from Rust/Metal, but (b) it required deliberate, app-level, platform-specific engineering — it did not come for free from a general-purpose windowing/graphics crate.

**wgpu (the graphics layer under egui, Slint's Skia backend indirectly, Xilem/Vello, and Floem):** wgpu's own surface-configuration API acknowledges P3 and extended-sRGB color spaces conceptually (`SurfaceColorSpace`, matching the web's `"srgb"`/`"display-p3"` canvas config plus standard/extended tone-mapping modes), but **wgpu does not tonemap or gamut-map for you** — the shader must already write correctly gamut-converted values, and, critically, **the wide-gamut/linear-light pipeline was not yet fully implemented on macOS as of this pass**: "the wgpu path does not yet implement the linear-light + wide-gamut pipeline, and the config value is accepted but most platforms effectively behave as srgb until that path is updated" (WebSearch synthesis of wgpu docs and related GitHub/forum threads, 2026 — **this specific "still behaves as sRGB" claim is a search-engine synthesis of primary wgpu documentation and should be verified directly against the wgpu changelog/issue tracker before being treated as settled fact; flagged as an indirect/aggregated finding**). This directly implicates **egui** (wgpu-backed by default) and any other wgpu-based stack (Xilem/Vello, Floem): none of them can currently be assumed to guarantee correct, non-clipped Display P3 output on macOS without bypassing the framework's default surface setup and hand-rolling the same kind of Metal-layer configuration Rio had to do.

**Slint:** An open Slint discussion thread shows a user explicitly asking about displaying images in wide color gamut on macOS "since that is the only platform where they have hardware that can actually do this" — i.e., **as of this pass, wide-gamut display is a requested feature, not a shipped guarantee**, in an open GitHub discussion rather than resolved documentation (GitHub Discussion #4988, slint-ui/slint, n.d.). Separately, and more alarmingly for a color-fidelity app: Slint's own issue tracker documents that **the software renderer's blending and color-interpolation code incorrectly assumes a linear color space when the actual input/output is sRGB** — described in Slint's own tracker as "a bug" — and that "the same goes for the Skia and FemtoVG renderers" (GitHub Issue, slint-ui/slint, n.d., cross-referenced via GitHub Issue #3742 on cross-renderer glyph/color differences). This is a **general color-correctness defect** (sRGB gamma handled incorrectly across all three of Slint's renderers), independent of and prior to the separate wide-gamut question — it means even *standard*-gamut color fidelity in Slint has a documented open defect as of this pass.

**Tauri / Dioxus (webview mode):** These delegate rendering to **WKWebView**, i.e., real WebKit, which is the same engine Safari uses and which Apple's own WebKit blog documents as shipping full CSS Display-P3 support (`color(display-p3 r g b)`) with correct wide-gamut handling (WebKit Blog, "Wide Gamut Color in CSS with Display-P3," n.d.). This is the **only framework in the table with a primary-source, documented, production-grade wide-gamut color pipeline on macOS** — because it isn't the Rust framework's own pipeline at all, it's Apple/WebKit's. The tradeoff: the app must author its color-critical UI in CSS/JS using `color(display-p3 ...)` and correct color-managed image formats, and any canvas-drawn swatches (e.g., `<canvas>` 2D/WebGL) reintroduce the same "did you tag the surface correctly" risk as the native-Rust renderers, since `<canvas>` compositing has its own, separately-configured color-space story. No Tauri- or Dioxus-specific API for ICC/ColorSync interop (e.g., reading the display's current ICC profile, honoring calibration) was found in this pass — Tauri exposes low-level, unsafe access to the underlying WKWebView object for such customization, but nothing packaged (Tauri docs, n.d.-e/f, and GitHub issue search on `tauri-apps/tauri` for accent-color/webview-background, n.d.).

**gpui:** No documentation, changelog entry, issue, or third-party report of Display P3 / wide-gamut support, ColorSync integration, or even an open feature request for either was found in this pass, despite direct, repeated searching. This is either (a) a real gap, or (b) present but undocumented outside the Zed codebase itself, which this pass did not have time to source-dive at the Metal-shader level. **Treated as unanswered/unverified** rather than "confirmed absent" — see Question Status.

#### Direct answer to "can any non-native stack guarantee it isn't silently rendering unmanaged sRGB?"

**No stack in this table ships that guarantee out of the box.** Ranked by how close each comes:

1. **Tauri/Dioxus-webview (WKWebView)** — closest to a guarantee, because the color pipeline is Apple's own WebKit, which has documented, shipped Display-P3 CSS support. But this guarantee applies to CSS-styled content, not to anything the app draws itself into a `<canvas>`/WebGL surface (which a virtualized 10k-row swatch grid likely requires for performance — see Q4.4), and no ColorSync/ICC-profile-reading API was found.
2. **egui / Xilem / Floem (wgpu-backed)** — technically capable of correct wide-gamut output (wgpu exposes the surface-color-space knobs, and Rio proves it's achievable in a Rust/Metal app), but as of this pass wgpu's own wide-gamut/linear-light path on macOS was reported as not fully implemented, meaning the app would have to bypass the framework default and hand-roll `CAMetalLayer` configuration exactly as Rio did — i.e., **the guarantee has to be built by the app team, not inherited from the framework.**
3. **Slint** — actively documented, open, unresolved color-correctness bugs (linear-vs-sRGB blending) across *all three* of its renderers, plus wide-gamut display itself still an open feature request. **Weakest position of the group on this specific question.**
4. **gpui** — unknown/unverified; no evidence found either way in this pass.

For an app whose entire product thesis is color fidelity with explicit gamut-clipping honesty, this is a strong structural argument that **none of the Rust-native (non-webview) UI stacks can be trusted with the actual swatch-rendering surface without the app team independently implementing and testing a `DisplayP3`-tagged, gamma-correct Metal/CAMetalLayer pipeline itself** — which is realistically a Shape A (Rust core + native SwiftUI/AppKit shell) outcome for the rendering surface specifically, or a large, bespoke engineering investment inside Shape B.

---

### Q4.4 — Virtualized list/table at 10,000+ rows with per-cell custom drawing

| Framework | Evidence found | Assessment |
|---|---|---|
| **egui** | `egui_extras::TableBuilder` provides `TableBody::rows()` (uniform row height, true virtual scrolling — only visible rows are rendered/laid out) and `TableBody::heterogeneous_rows()` (variable height, slight extra cost from height-summation but still described as "many orders of magnitude" better than adding rows individually) (docs.rs, n.d.-g). Custom per-cell content is trivial in immediate mode — arbitrary widgets, including custom-painted swatches via `egui::Painter`, inside the row closure (community crate `egui_tabular`, romixlab, n.d., demonstrates a custom cell viewer/editor pattern). **This is the most concretely-documented virtualized-table story of the group for this exact use case.** | Best-evidenced fit for a 10k+-row color-swatch grid with custom cell drawing — but see Q4.3: correct P3 rendering inside those custom-painted cells is not guaranteed by egui/wgpu by default. |
| **Slint** | No dedicated virtualized-table crate/example with the same depth of documentation was found in this pass; Slint's model-based list views (`ListView`/repeated components) are documented generally but a specific 10k-row, custom-cell-drawing benchmark or example was not located — **gap**. | Plausible (Slint's model system is designed for large lists) but unverified at this scale in this pass. |
| **Tauri/Dioxus (webview)** | Inherits the web platform's virtualization story — i.e., whatever JS virtualization library the app author brings (e.g., a windowing library), same as any web app. Not a framework-native capability; entirely up to app-level JS/WASM code. | Fully achievable, but the burden and the crate ecosystem are JavaScript's, not Rust's — a notable dilution of "Rust as primary language" if UI virtualization logic ends up living in TypeScript. |
| **gpui** | Zed itself renders large scrollable lists (file trees, buffer content, search results) performantly, implying GPUI's element/layout system supports virtualization patterns, but no dedicated "10k-row table with custom cell drawing" example or crate comparable to egui's `TableBuilder` was found in this pass. | Plausible via Zed's own precedent but not independently documented as a general-purpose, reusable table widget. |
| **Xilem/Floem** | No 10k-row, custom-cell-drawing example or benchmark found for either in this pass. | Unverified — **gap**. |

**Bottom line:** egui is the only framework in this set with a specific, documented, purpose-built API (`TableBuilder`/`TableBody::rows`) that matches the brief's exact requirement (virtualized rows + arbitrary per-cell custom drawing) with primary-source confirmation.

---

### Q4.5 — Text input, IME, and localization limitations on macOS

- **winit (the windowing layer under egui, Slint's winit backend, Xilem, and Floem)** has real but limited IME plumbing: an `Ime` event carrying pre-edit state, `Window::set_ime_allowed`, `Window::request_ime_update`, `Ime::DeleteSurrounding`, and `ImePurpose`/`ImeHints` (rust-windowing/winit GitHub commit and issue history, n.d.). On macOS specifically, IME composition is wired through `insertText:replacementRange:` in `src/platform_impl/macos/view.rs` (rust-windowing/winit, n.d.). There is a documented history of real macOS-IME bugs, including a crash in `set_marked_text` triggered by the native Pinyin IME sending an out-of-bounds selected range (since reportedly fixed) and open issues around Character Viewer / composing-text display for Chinese input (GitHub Issues #3342, #3893, rust-windowing/winit, n.d.).
- **egui**, evaluated in a cross-platform survey (tested with Windows Narrator/IME, not macOS specifically — **flagged as indirect for macOS**), was reported to have gaps including a **default font with no Hiragana/Kanji coverage** and Tab-key presses being consumed rather than passed through during IME composition (boringcactus, 2025). This is a concrete, named defect class (missing CJK glyph coverage) that would need to be fixed at the app level (custom font loading) regardless of platform.
- **Slint**, same survey: "Narrator works perfectly," reported with no IME-specific caveat (boringcactus, 2025) — again Windows-tested, not macOS-verified in this pass.
- **Tauri/Dioxus (webview)**: IME composition is handled by WKWebView / the system text-input framework the same way Safari handles it — i.e., inherits the OS's mature, first-party IME support, since it's real AppKit text input under a real web `<input>`/`contenteditable` element. This is a structural advantage for the same reason WKWebView's accessibility is a structural advantage.
- **gpui**: No IME-specific documentation or issue history was found in this pass; Zed does support CJK text entry in practice (as a shipped code editor), which implies a working macOS IME bridge exists, but no primary-source doc describing its fidelity or known limitations was located — **gap**.
- **Localization**: No framework in this set ships a batteries-included localization/i18n system comparable to what a SwiftUI app gets for free from `Localizable.strings`/`String Catalogs`. All of them push localization to a general Rust crate (e.g., `fluent`, `i18n-embed`) or, for webview-based stacks, to standard web i18n libraries — this is a wash across frameworks rather than a differentiator, and was not separately documented per-framework in any primary source found in this pass.

---

### Q4.6 — Honest 2025–2026 community verdict on production-readiness of Rust-native desktop UI on macOS

The dominant, repeated verdict across independent 2025–2026 surveys is **"pick your poison, nothing is done" rather than "not ready at all,"** with meaningful differentiation between the webview-based and non-webview stacks:

- A widely-cited framework census found that **94.4% of surveyed Rust GUI libraries were judged not production-ready**, for reasons spanning build/setup failures, missing tested features, usability defects, and abandonment (cited via Wren Learns Rust, 2026, summarizing a broader landscape review). This figure describes the *long tail* of the ecosystem (dozens of small/experimental crates), not necessarily the small set of frameworks this brief names — but it is the honest baseline: **most of what shows up in a "Rust GUI" search is not viable**, and the credible set narrows quickly to egui, Slint, Iced, Tauri, and (for webview apps) Dioxus.
- **Tauri** is the framework most consistently called "mature" on raw adoption/release-cadence grounds (Wren Learns Rust, 2026) — but a separate, detailed cross-platform survey author was sharply critical of its *architecture* specifically, calling the frontend/backend IPC split "entirely unnecessary" and stating bluntly "I think I genuinely hate Tauri" (boringcactus, 2025) — a useful counterweight showing the "production-ready" verdict is about ecosystem maturity, not universal love for the design.
- **egui** draws consistent "clear winner by download count" framing (13M+ downloads cited) and "fastest path to a window," with the explicit caveat that its default look is a "debug UI" aesthetic that most apps will want to reskin (Wren Learns Rust, 2026).
- **Slint** was described as having "come a long way in the last four years" and now meriting "a more thorough look" by an author who had previously been unimpressed (boringcactus, 2025) — a positive-trending but not unreserved verdict, undercut on this project's terms by the open sRGB-blending-correctness bug found in Q4.3.
- **Dioxus** earned the most enthusiastic single-framework endorsement found in this pass — "possibly use Dioxus for real work without constantly being miserable," called a "new high water mark" for that reviewer's recurring survey series (boringcactus, 2025) — but that verdict is for the default **webview-backed** desktop target, not the still-pre-production native (Blitz) renderer this brief actually needs if avoiding a webview is a goal.
- **gpui** is conspicuously absent from these general cross-framework surveys as a general-purpose recommendation — it is discussed almost exclusively in the context of *being* Zed, not as a toolkit other teams are adopting. A separate 2026 survey (title: "A 2026 Survey of Rust GUI Libraries") exists and was located but returned HTTP 403 on fetch in this pass and could not be read directly (blog.wybxc.cc, 2026) — **flagged as an unread source; see Question Status**.
- **Xilem and Floem** are both explicitly self-described or third-party-described as pre-production: Xilem is "alpha" by its own maintainers (linebender/xilem, n.d.), and Floem is maintained by a single person with no tagged release in roughly two years as of this pass, with instructions to track `main` rather than a stable release (lap.dev/floem, n.d.).

**Synthesized verdict for this project specifically:** the community consensus is closer to **"ship it for webview-shaped apps (Tauri/Dioxus), wait or budget significant bespoke engineering for anything requiring a genuinely native, GPU-drawn, color-managed custom canvas (egui/Slint/gpui/Xilem/Floem)."** None of the surveys located treat wide-gamut color correctness as a criterion at all — this project's specific priority question (Q4.3) is simply not on the community's radar as a maturity axis, which is itself a signal: **it has not been battle-tested by the Rust GUI ecosystem for an app like this one.**

---

### Closing synthesis for Concern Area 4

For SpectroCapture's specific combination of requirements — accessible, native-feeling macOS chrome, a 10k+-row custom-drawn color grid, and above all *provable* Display-P3/ColorSync fidelity — the frameworks split cleanly into two failure modes:

- The **webview stacks (Tauri, Dioxus-webview)** win on native menu bar, multi-window, IME, and baseline accessibility (all inherited from real AppKit/WebKit), and have the only documented, primary-sourced wide-gamut color pipeline (WebKit's CSS Display-P3 support) — but that guarantee only covers CSS-styled content, not a canvas-drawn swatch grid, and no ColorSync/ICC-profile API was found for either.
- The **native-Rust-renderer stacks (egui, Slint, gpui, Xilem, Floem)** have the more plausible path to a fully custom, GPU-drawn, potentially-P3-correct swatch grid (egui in particular has the best-documented virtualized-table API), but every one of them either has a documented, open color-correctness defect (Slint's sRGB-blending bug), an unfinished wide-gamut pipeline in their shared graphics layer (wgpu's incomplete macOS wide-gamut/linear-light path), or simply no evidence either way (gpui) — and gpui additionally has a maintainer-acknowledged, effectively nonexistent VoiceOver story, which alone should disqualify it for this project's non-negotiables.

No framework surveyed clears the bar of "provably won't silently render unmanaged sRGB" without the app team doing the same kind of manual `CAMetalLayer`/color-space engineering that Rio's terminal had to do by hand. This is the central, section-specific data point that should feed the brief's overall Shape A/B/C recommendation (owned by a later synthesis step, not this section).

---

## 5. Color Science & Data Layer in Rust

_Sections 5 and 7 were prepared 2026-09-01 by the agent assigned Concern Area 5 (Color Science & Data Layer in Rust) and Concern Area 7 (Testing Without Hardware)._

---

### 5.1 — Color-conversion and ΔE2000 crates: versions, validation, and comparison to ICC / Python colour-science

**Spectral→XYZ→Lab/LCh/Luv/sRGB/HSL conversion.** The dominant, actively maintained crate is **palette**, currently at **v0.7.7** (released 2026-08-02), dual-licensed Apache-2.0/MIT, with roughly 4.6M downloads/month (Lib.rs, 2026). Palette provides typed representations for RGB, HSL, HSV, HWB, CIE L\*a\*b\*, L\*C\*h°, CIE XYZ, and xyY, with conversion implemented as traits rather than ad hoc functions (docs.rs, 2026). It does **not** cover spectral (reflectance-curve) → XYZ conversion — that step (CIE observer integration over a measured spectral power distribution) is outside palette's scope and outside every general-purpose Rust color crate found in this survey; a spectrophotometer-driving app would need to implement the CIE 1931/1964 standard-observer integration itself or find a dedicated colorimetry crate such as **colorimetry** or **scot** (harbik), both far less downloaded/vetted than palette (crates.io, 2026) — flagged as **inferred gap**, not confirmed against either crate's actual spectral-integration correctness.

**ΔE2000 (CIEDE2000).** As of 2026 there are at least four viable options, no single obvious "one true" choice:

| Crate | Version | License | Last release | ΔE methods | Notes |
|---|---|---|---|---|---|
| **palette** (`color_difference` module) | 0.7.7 | Apache-2.0/MIT | 2026-08-02 | `Ciede2000`, `ImprovedCiede2000` (Huang et al. correction), `DeltaE`, `ImprovedDeltaE`, `EuclideanDistance`, `HyAb`, `Wcag21RelativeContrast` traits, implemented on `Lab`/`Lch` | Actively maintained, part of the same crate already needed for conversions — lowest integration cost (docs.rs, 2026) |
| **deltae** | 0.3.2 | MIT | 2023-07-25 | DE2000 (default), DE1994, DECMC, DE1976 | Dedicated single-purpose crate; no activity since 2023 (Lib.rs, 2026) |
| **empfindung** (successor to `delta_e`) | 0.2.6 | MIT | 2022-12-14 | CIEDE2000, CIE94, CIE76, CMC l:c | Ships doc-test assertions against known example values (e.g. `assert_eq!(58.90164, delta_e)`), but no documented large-scale reference-dataset validation suite (Lib.rs, 2026) |
| **DeltaE** (elliotekj) | unversioned GitHub crate | MIT | commit history present, exact last-commit date not surfaced | CIEDE2000 only | A direct Rust port of Zachary Schuessler's JavaScript CIEDE2000 implementation; the repository does not document testing against a canonical reference set (e.g., the Sharma et al. 2005 test dataset) in what was retrievable (GitHub, elliotekj/DeltaE) — **flagged as inferred/unconfirmed**: absence of evidence for such validation, not evidence of its absence |

**Correctness validation vs. references (ICC, Python colour-science).** No crate surveyed publishes an explicit "validated against Python colour-science" or "validated against the ICC reference implementation" statement for its ΔE2000 or Lab/XYZ math specifically. What *does* exist is direct data lineage from Python `colour-science`: **colorspace-rs** (anderslanglands) explicitly states it "contains data taken from the excellent colour-science python library" by Mansencal et al. (GitHub, anderslanglands/colorspace-rs, 2026), and **MunsellSpace** (a Rust+Python crate pair) is explicitly "based on the mathematical algorithms from the Python Colour Science library" and claims validation "against CIE standards and reference RGB values" (GitHub, ChrisGVE/MunsellSpace, 2026). Neither of these is `palette`, `deltae`, or `empfindung` — meaning the most commonly recommended conversion/ΔE crates for a production app (palette, for its maintenance and download profile) do **not** themselves carry a documented cross-check against colour-science, while the crates that *do* document such cross-checks (colorspace-rs, MunsellSpace) are lower-profile, more niche projects. **This is the single most load-bearing unresolved point in Concern 5**: an ADR that adopts palette for ΔE2000 should budget for an explicit validation pass — generating a fixture set from Python colour-science offline and asserting palette's output against it in a proptest/golden-file suite (see §7.5) — because palette's own documentation does not claim this validation has already been done.

**Dominant community position:** for a project already depending on palette for type-safe color-space conversion, using palette's own `Ciede2000`/`ImprovedCiede2000` traits is the path of least integration friction and the most actively maintained option; `deltae` and `empfindung` are viable single-purpose fallbacks if palette's ΔE implementation is found wanting during validation, but neither has been touched since 2022–2023 and both should be treated as maintenance risks if adopted as the primary dependency.

### 5.2 — ICC profile / ColorSync interop from Rust

Two separate concerns exist here: (a) a portable, non-Apple-specific ICC engine, and (b) native macOS ColorSync API access.

**lcms2 (Little CMS bindings).** The `lcms2` crate is the dominant, mature choice: **v6.2.0** (released 2026-08-26 per Lib.rs), wrapping **LCMS 2.19.1**, with roughly 484,000 downloads/month and use in 43 crates (19 directly) (Lib.rs, 2026). It is a safe wrapper over `lcms2-sys`, described in its own documentation as having "been stable for years, and used in production at scale" (kornelski/rust-lcms2, GitHub). It exposes `Profile` and `Transform` types for reading/applying ICC profiles and converting between them, and supports thread-safe transforms via `Flags::NO_CACHE` (kornelski, GitHub). This is the standard, portable ICC engine choice regardless of shape (A, B, or C-adjacent Rust code), and it is the same underlying C library (Little CMS) that many cross-platform color tools use — it is a credible, production-grade dependency.

**ColorSync (macOS-native) interop.** The `objc2-color-sync` crate, part of the `objc2` project (madsmtm/objc2), provides direct Rust bindings to Apple's ColorSync framework: **v0.3.2** (released 2026-08-04), triple-licensed Zlib/Apache-2.0/MIT, and documented as "100% documented" (docs.rs, objc2-color-sync, 2026). It exposes profile creation/reading (including `ColorSyncProfileCreateWithDisplayID` for reading the active display's profile), transform creation/application (`ColorSyncTransformCreate`, `ColorSyncTransformConvert`), device registration, and CMM (Color Management Module) iteration. Several of the exposed functions carry deprecation warnings — this reflects Apple's own API evolution (ColorSync's older C API is partially superseded by newer Swift/ObjC-side APIs) rather than immaturity in the binding itself (docs.rs, 2026).

**Maturity summary:** lcms2 is the more battle-tested, portable choice for a general ICC engine and is what should do the actual profile-based color transforms; objc2-color-sync is a real, current, actively-maintained (Aug 2026 release) but comparatively young binding whose primary practical use in this project would be reading the active display's ICC profile ID/data so it can be handed to lcms2 (or compared against a wide-gamut/Display P3 check) rather than doing the transform math itself. **Inferred**, not directly documented: no shipped precedent was found of an app using objc2-color-sync + lcms2 together in this exact "read display profile via ColorSync, transform via lcms2" pattern — this is a reasonable composition, not one with confirmed prior art.

### 5.3 — rusqlite vs. sqlx vs. diesel, and comparison to GRDB

| | **rusqlite** | **sqlx** | **diesel** | **GRDB (Swift, incumbent)** |
|---|---|---|---|---|
| Latest version (2026) | 0.40.2 (2026-08-08) | 0.9.0 (2026-05-21) | 2.3.12 (2026-08-07) | n/a (Swift) |
| License | MIT | MIT/Apache-2.0 | MIT/Apache-2.0 | MIT |
| Monthly downloads | ~12.1M | ~13.3M | ~2.4M | n/a |
| Execution model | Synchronous, direct SQLite C API wrapper | Async (tokio or async-std), compile-time checked queries | Synchronous, ORM with compile-time schema-mapped query builder | Synchronous core API + `DatabasePool`/`DatabaseQueue` concurrency primitives |
| SQLite driver | Bundles or links libsqlite3 directly | Uses libsqlite3 (not a pure-Rust reimplementation); system link requires SQLite ≥3.20.0 | Native SQLite backend, plus WASM compatibility | Links SQLite directly (same C library) |
| Migrations | None built-in; ecosystem crates `rusqlite_migration` (lightweight, uses `PRAGMA user_version`, no macros, no CLI) or `refinery` (multi-DB, `.sql`-file or Rust-fn migrations, optional CLI) | Built-in via `migrate!` macro, compile-time embedded, "offline mode" for build-without-live-DB | Built-in via `diesel_migrations` crate, `up.sql`/`down.sql` pairs, "if it compiles, it works" philosophy | Built-in `DatabaseMigrator` — ordered migrations, supports "erase on schema change" for debug |
| Typed blob handling | `blob` feature gives `std::io::{Read,Write,Seek}` over SQL BLOBs directly | Standard `Vec<u8>`/`Bytes` binding, no dedicated streaming-blob API found in search | Type-safe blob columns via `diesel::sql_types::Binary` mapped to Rust types | `Data`/`DatabaseValueConvertible` typed blob support, integrates with Swift `Codable` |
| Change observation | None built-in | None built-in | None built-in | `ValueObservation` / `FetchedRecordsController` — first-class, no analog found in any Rust option |
| Record/ORM layer | None — write raw SQL, map manually | Query builder + macros, no full ORM record layer | Full ORM: schema macros, `Queryable`/`Insertable` derive, associations | `FetchableRecord`/`PersistableRecord` protocols, closest Swift analog to Diesel's model |
| WAL mode | Supported via `PRAGMA journal_mode=WAL` (standard SQLite pragma, not a rusqlite-specific claim — **general SQLite knowledge, not found as an explicit rusqlite doc statement in this search**) | Same underlying SQLite pragma mechanism | Same underlying SQLite pragma mechanism | First-class: `DatabasePool` is explicitly designed around WAL for concurrent readers + single writer |

Sources for the version/feature rows: Lib.rs pages for rusqlite, sqlx, and diesel (2026); Lib.rs/crates.io/GitHub pages for `rusqlite_migration` and `refinery` (2026); GRDB's own GitHub repository and wiki (groue/GRDB.swift, 2026).

**2026 community consensus** (Aarambh Dev Hub, 2026; Rustify, 2026; Pi Stack, 2026 — three independent 2026 comparison posts converge on the same framing): *"Diesel if you want the compiler to babysit you and you're building a large, long-term system; SQLx if you want async + performance and prefer writing SQL directly; rusqlite if SQLite is enough and you want something lightweight."*

**Feature-for-feature verdict against GRDB.** No single Rust crate matches GRDB's combined feature set — GRDB is unusual in Swift-land precisely because it bundles a low-level API, a typed Record/ORM layer, built-in migrations, *and* first-class change observation (`ValueObservation`) in one package. Decomposed:
- **rusqlite** is the closest analog to GRDB's *low-level* API and its synchronous execution model (both are thin, ergonomic wrappers over the SQLite C API, not async), but rusqlite has no Record/ORM layer and no observation system — those would have to be hand-built or composed from separate crates.
- **diesel** is the closest analog to GRDB's *high-level* Record/ORM ergonomics and its built-in migration story, but diesel's execution model, being schema-macro-driven and multi-backend (Postgres/MySQL/SQLite), is heavier and less SQLite-idiomatic than GRDB's SQLite-only focus.
- **sqlx**'s async model is a *mismatch* for this app's shape: the product is a single local SQLite file with no server and no concurrent remote clients, and the constraint that captures happen at hardware pace on one device at a time (per AGENTS.md §4) does not obviously benefit from async DB access — sqlx's core value proposition (non-blocking I/O for network-bound multi-client servers) does not map cleanly onto a single-user desktop app's local-file access pattern. If the app's *core* were already async (tokio-based, per Concern 6), sqlx would at least be consistent with that runtime; if the core is not async, sqlx introduces an executor dependency for no clear benefit over rusqlite.
- **No Rust crate has an analog to `ValueObservation`/`FetchedRecordsController`.** This is the sharpest, most concrete feature gap: if the collection-browsing UI relies on GRDB-style live-updating queries (a `ValueObservation` re-firing on write), a Rust core would need to hand-roll that (e.g., a change-notification channel emitted after each write transaction) — none of rusqlite/sqlx/diesel provide it out of the box. **This is an inferred architectural cost**, not directly documented as a known pain point in any source found, but it follows directly from the absence of the feature in all three crates surveyed.

**Practical recommendation for this project specifically:** rusqlite is the most GRDB-adjacent choice in *spirit* (synchronous, thin, SQLite-only, ergonomic) and pairs with `rusqlite_migration` for a lightweight migration story matching this project's "one file, one user" scale; diesel is the better choice only if the team wants compile-time-checked schema/query safety badly enough to accept its heavier ORM machinery; sqlx should be adopted only if the wider architecture is already committed to an async (tokio) core end-to-end.

### 5.4 — Rust-specific patterns/pitfalls for canonical-raw-payload-plus-derived-values schema

No primary source was found describing a Rust-specific canonical pattern for "store the raw instrument payload as a BLOB, recompute derived color values from it, version the derivation" — this appears to be a general data-modeling pattern (closer to event-sourcing/CQRS practice) rather than something the Rust ecosystem has published dedicated guidance on. **This entire subsection is inferred from general Rust ecosystem knowledge (serde/rusqlite mechanics), not from a primary source documenting this exact pattern.** The concrete Rust-specific pitfalls that follow from how the surveyed crates actually work:

- **Blob (de)serialization versioning.** If the raw payload is stored as the vendor SDK's native byte layout (untouched), no Rust serialization concern applies — it's opaque bytes in, opaque bytes out, and only the vendor SDK's own format stability matters. If instead the raw payload is wrapped in a Rust struct and serialized (e.g., via `bincode` or `serde` before writing to a BLOB column), every schema change to that struct is a durability hazard: `bincode`'s default encoding is positional, not self-describing, so adding/removing/reordering fields silently breaks deserialization of previously-written rows unless a version tag is stored alongside the blob and matching decode logic is kept per version. The general Rust-community mitigation is an explicit leading version byte/field read before dispatching to the correct decoder — this is not automatic in any of `rusqlite`/`sqlx`/`diesel`'s typed-blob handling; it is application code the project would have to write itself.
- **Derived-value recompute triggers.** None of the three SQL crates provide a "recompute derived columns on read" or migration-triggered-recompute mechanism; a derivation-pipeline version bump (e.g., changing the Lab→sRGB gamut-mapping algorithm) would need an explicit, manually-triggered batch recompute pass (a `cargo` binary or a startup migration step) rather than anything the SQL layer offers automatically — this is consistent with GRDB's own approach (its migrator runs SQL/Swift closures, not automatic recomputation) and does not appear to be a place where Rust differs materially from Swift/GRDB; it is a data-modeling responsibility above the SQL layer either way.
- **Type affinity mismatch risk.** SQLite's dynamic typing means a BLOB column will silently accept non-blob data if the app-level type checking is weak; rusqlite's `ToSql`/`FromSql` traits and diesel's `sql_types::Binary` both add a Rust-level type-safety layer over this, which mitigates but does not eliminate the risk (a raw `execute()` with string-formatted SQL in rusqlite bypasses it entirely) — this is a general SQLite pitfall, not a Rust-specific one, but the concrete tools available to guard against it in Rust are the traits mentioned above.

### 5.5 — CSV inventory import crate comparison

**Rust: `csv` crate** (BurntSushi), currently **v1.4.0** (released 2025-10-17, per docs.rs). It parses "a strict superset of RFC 4180" by default (i.e., it accepts more malformed variance on read than it will ever write), uses a zero-copy, byte-level state machine, and validates UTF-8 as part of parsing (docs.rs, csv crate documentation). Two configuration points matter directly for "real-world CSV" per the brief's question:
- **Malformed/variable-length rows**: by default the reader enforces RFC 4180's "every record has the same number of fields" rule and errors on violation; calling `.flexible(true)` on the `ReaderBuilder` turns this off so variable-length rows are accepted rather than aborting the whole import (docs.rs, csv crate documentation).
- **Non-UTF-8 encodings**: the base `csv` crate parses at the byte level (delimiters/quotes must be ASCII) and returns a UTF-8 validation error on non-UTF-8 content unless paired with an external encoding-conversion step; the ecosystem convention is to pre-transcode via `encoding_rs` (or the crate's own `encoding` extension point in older docs) before feeding bytes to the CSV reader, rather than the `csv` crate handling detection/transcoding itself (RustFAQ, 2026; Rust Cookbook).

**Swift comparison.** Two options were found in active use: **SwiftCSV**, described as "a well-tested parse-only library which loads the whole CSV in memory" — explicitly *not* intended for large files (GitHub, swiftcsv/SwiftCSV); and **CodableCSV** (dehesa/CodableCSV), which supports row-by-row and field-by-field streaming reads plus `Codable`-based declarative decoding, multi-platform (GitHub, dehesa/CodableCSV). Neither Swift library's documentation surfaced in this search an equivalent, explicit "flexible/lenient row-length" toggle comparable to `csv`'s `.flexible(true)`, nor an explicit non-UTF-8 encoding-detection story comparable to the `encoding_rs` pairing pattern — **this is a real, if not exhaustively confirmed, differentiator**: the Rust `csv` crate's malformed-row leniency is an explicit, documented, single-flag feature, whereas the equivalent Swift-side lenience (if any) was not surfaced by this research pass and should be verified directly against each Swift library's source before an ADR treats it as a decided advantage. SwiftCSV's "loads the whole file in memory" constraint is a more clear-cut, confirmed limitation relative to `csv`'s streaming, zero-copy design — for a "bulk import an entire inventory" use case this favors the Rust crate's streaming model if memory footprint on large inventories (thousands of rows) matters, though at that scale ordinary desktop RAM likely makes this moot either way (**inferred**, not measured).

---

## 6. Concurrency & the Hardware-Paced Pipeline

### 6.1 What replaces the `AsyncStream`-bridge pattern in each shape: tokio version, smol, std-only — what desktop apps actually use

**Tokio**: current stable is 1.53.1, released 2026-07-20 (Lib.rs, n.d.-e); the project runs LTS lines in parallel (1.47.x through Sept 2026, 1.51.x through March 2027) alongside a roughly-monthly minor-release cadence (search synthesis of crates.io/tokio release cadence). Tokio dominates async-Rust adoption broadly (hundreds of millions of downloads) (search synthesis), which is a server/library-ecosystem signal, not itself proof of desktop-app dominance.

**What desktop apps (not servers) actually use — the important, non-obvious finding**: the most relevant shipped precedent, **Zed** (a full-Rust, GPUI-based macOS editor — directly relevant to shape B and to the general "who owns the event loop" question for shape A), **does not use Tokio or smol as its core async runtime**. Zed built a custom async execution layer (`gpui::PlatformDispatcher` + `async_task`) that integrates directly with the platform's native event loop (dispatched onto the main thread for foreground work, a background thread pool otherwise), and only pulls in Tokio selectively, via a dedicated `gpui_tokio` crate, for network-heavy work that specifically needs Tokio's ecosystem (Zed Industries, n.d., as summarized via search synthesis — direct blog fetch 404'd; corroborated independently by the `gpui` crate's own README/crates.io description). This is a strong, concrete counter to reflexively defaulting to "Tokio inside the app process": a desktop app with a native run loop has reasons to build (or use) a **run-loop-integrated** executor for UI-adjacent async work and reserve a general-purpose runtime like Tokio for I/O-heavy subsystems that don't touch the UI thread. For shape A specifically (Rust core, no Rust UI), this argues for a **scoped Tokio runtime confined to the device/data layer**, not a runtime that fights SwiftUI's own concurrency model — see 6.3.

**smol**: lighter-weight, minimal-footprint alternative; general 2026 community guidance frames the choice as "Tokio for ecosystem compatibility, smol for libraries that shouldn't force a runtime on downstream users" (search synthesis of multiple 2026 Tokio-vs-smol comparison pieces — no single primary source strong enough to cite individually; treat as **community consensus, not a specific documented claim**). Zed's own hybrid (smol-flavored custom executor + selective Tokio) is the one concrete desktop precedent found and matches this framing.

**std-only**: no shipped precedent found using bare `std::thread` + channels as the *entire* async layer for a UniFFI-bridged, hardware-event-driven macOS app. Given UniFFI's async-fn support assumes *some* Rust-side future executor exists to drive the future to completion (§2.2), and this project's device layer is inherently I/O-bound (BLE/USB polling, license validation, SQLite), a bare-std approach would mean hand-rolling exactly what Tokio (or a scoped custom executor à la GPUI) already provides — not evaluated further as a serious option, consistent with the search finding no production examples of it in this application class.

**Recommendation implied by the evidence**: for shape A, a Tokio runtime scoped to the device/IO layer (not a whole-app runtime) is the well-precedented default per UniFFI's own async model, tempered by Zed's lesson that a naive "just spawn a full multi-thread Tokio runtime for the whole process" approach is not what the one shipped desktop precedent actually does.

### 6.2 Idioms for enforcing one-scan-in-flight against a single-device, single-session SDK: actor-style serialization

**Rust**: the standard idiom found across multiple 2024–2026 sources (Ryhl, n.d.; multiple dev.to/Medium actor-pattern writeups, convergent) is the **Tokio actor pattern**: a single Rust task owns the device handle exclusively; a `tokio::sync::mpsc::Sender<Command>` is the only public handle to it, so all commands funnel through one channel and are processed strictly in order by the one task that owns the receiver — this *is* the single-in-flight guarantee, enforced structurally (there is exactly one consumer, and it processes messages sequentially) rather than via a lock. Request/response calls embed a `tokio::sync::oneshot::Sender` inside each command variant so the caller can `await` a reply without needing a shared mutable-state lock (Ryhl, n.d.). This maps directly onto this project's "one device at a time" non-negotiable and the vendor SDK's architecturally single-session nature: model the device as one actor task; every UI-triggered scan request is a message into its channel; a second concurrent request either queues behind the first (natural mpsc backpressure) or is rejected with a "busy" error, matching product intent (no concurrent devices, ever).

A separate, lower-level idiom seen specifically for BLE/GATT stacks generally (not vendor-SDK-specific — this crosses into §3 territory but the serialization principle is general) is wrapping the connection handle in a `Mutex` and requiring every operation to acquire the lock, because "most BLE stacks allow only one outstanding GATT operation at a time" (search synthesis of `btleplug`/general BLE-Rust guidance). The actor-with-channel pattern is the better fit here because it also naturally gives a queueing/ordering story (relevant to the product's scan-queue UX) that a bare mutex does not.

**Swift**: the directly comparable idiom is a Swift `actor` wrapping the `SpectroDevice` (or its Live implementation), where actor isolation gives the same "exactly one in-flight call" guarantee the language enforces structurally, and callers `await` calls into it exactly as they would await the Rust actor's oneshot reply. This is **general Swift-concurrency knowledge** (not attributed to a specific 2024–2026 source; the brief instructs not to cite general knowledge) — the meaningful comparison point is that **both languages converge on the identical actor-with-serialized-mailbox shape**, so choosing Rust for this specific slice of the architecture is not really trading away a concurrency-safety primitive the Swift-only baseline already had; it is re-implementing the same primitive in a different language, with Rust's version additionally needing to cross the FFI boundary correctly (see §6.4).

### 6.3 In shape A: who owns the event loops — running a Tokio runtime inside a macOS app process alongside the main RunLoop

**Real-world friction, not folklore**: a documented, current (2026) issue in the Tauri project's plugin workspace shows exactly the failure mode this question is asking about — clipboard operations **crash on macOS due to a thread-safety violation** because AppKit objects must only be touched from the main thread, while the operation originated from a Tokio worker thread inside the same process (tauri-apps/plugins-workspace issue #3205, cited via search synthesis). This is strong, concrete evidence (not inferred) that co-hosting a general multi-thread Tokio runtime with AppKit's main-thread requirements is a real, recurring class of bug in shipped Rust-plus-native-UI desktop apps, not a theoretical concern.

**Runtime configuration options and their tradeoffs**, per Tokio's own documented runtime types (Tokio, n.d.):
- **Multi-thread runtime** (the default `Runtime::new()` / `#[tokio::main]`): work-stealing thread pool; this is what produces the Tauri-style cross-thread AppKit violation unless every callback back into UI code is explicitly re-dispatched to the main thread/queue.
- **Current-thread runtime**: single-threaded executor; "does not execute tasks at all unless there is a call to `block_on`, and runs all tasks inside the `block_on` call" (Tokio, n.d.) — awkward to co-host with a `RunLoop` that also needs to keep spinning, since something has to own calling `block_on` without starving the RunLoop.

**What the one shipped desktop precedent (Zed) actually does**, which directly answers "who owns the event loop": GPUI does **not** hand event-loop ownership to Tokio. It builds its own `PlatformDispatcher` that dispatches foreground work onto the platform's real main-thread run loop and background work onto its own executor, and only invokes Tokio narrowly (via `gpui_tokio`) for subsystems that need it, kept off the UI-touching path (Zed Industries, n.d., search synthesis). Applied to shape A: the RunLoop/CFRunLoop stays owned by SwiftUI/AppKit as always; a Tokio runtime lives entirely inside the Rust core, confined to the device/data layer, and **never itself touches UI state** — every value it hands back across the FFI boundary is marshaled through UniFFI's async-completion mechanism (§2.2), which resumes a Swift `Task` rather than a raw Tokio callback reaching into SwiftUI. This is the architecturally sound reading of the evidence, but it is a synthesis of the Tauri failure mode + the Zed precedent + UniFFI's documented completion model, not a single source stating "here is how to run Tokio safely alongside AppKit" — **flagged as inferred, moderate confidence**.

**App Nap / thread QoS / energy impact**: **documented gap**. General macOS guidance on App Nap (priority reduction, timer/I/O throttling for non-frontmost, non-"important work" apps) and thread QoS classes (background QoS pinned to efficiency cores on Apple Silicon) is well documented by Apple (Apple, n.d.-a; Apple, n.d.-b), and Rust *can* call the relevant macOS API (`pthread_set_qos_class_self_np`) via the `libc` crate's `pthread_attr_set_qos_class_np` binding (docs.rs/libc, n.d.) — so QoS-tagging a Tokio worker thread from Rust is mechanically possible. But **no source found documents Tokio's own worker threads' default QoS class, whether Tokio exposes a supported way to QoS-tag its pool, or measured App Nap/energy behavior of a Tokio runtime embedded in a macOS app**. This is a genuine, specific gap — see Question Status.

### 6.4 Cancellation and timeout propagation across the boundary mid-operation

**Documented limitation, current as of 0.32.0**: UniFFI has **no built-in way to cancel a Rust future from the foreign side**, and no built-in mechanism to raise a foreign-native cancellation error (e.g., a Swift `CancellationError`) back into Rust (Mozilla, n.d.-m, GitHub issue #2771). A prior attempt to add Swift-side-cancellable Rust futures was abandoned over API-design difficulty; the maintainers' framing is that Rust futures are natively "fine to drop" (no explicit cancel needed) whereas Swift's `Task` cancellation model expects a cooperative-cancellation signal, and reconciling the two cleanly is unresolved (Mozilla, n.d.-m).

**What *does* exist**: a lower-level plumbing hook — `ForeignFutureDroppedCallbackStruct` — lets Rust register a callback that fires when the *foreign* (Swift) async task is dropped, which a project can wire up manually to signal "please stop" into the Rust-side operation (e.g., set an `AtomicBool` or send on a cancellation channel that the actor task in §6.2 polls) (Mozilla, n.d.-m). This means **cancellation is achievable, but is an application-level pattern you build yourself on top of a drop notification, not a language-level cancellation propagation UniFFI gives you for free.**

**Applied to this project's "scan cancelled from the UI while the SDK call is in flight" scenario**: the concrete, buildable pattern is: (1) the Swift `Task` performing the scan is cancelled normally (`task.cancel()`); (2) UniFFI's `ForeignFutureDroppedCallbackStruct` fires when that dropped/cancelled state propagates to the in-flight FFI call; (3) the Rust actor task (§6.2) observes this via a cooperative cancellation token (e.g., `tokio_util::sync::CancellationToken`, a standard Tokio-ecosystem crate — not itself found cited in a UniFFI-specific source, flagged as a reasonable but unverified-in-this-exact-combination suggestion) and stops driving the vendor-SDK call as soon as it next checks; (4) because the vendor SDK's own call may not itself be cancellable mid-flight (unknown — vendor-SDK cancellation semantics are covered under §3), the observable behavior may be "stop *waiting* for the result" rather than "the instrument actually stops mid-measurement." **This entire mid-operation-cancellation answer is a synthesis, not a documented case study — flagged as inferred and in need of prototyping**, see Question Status.

**Timeouts**: no UniFFI-specific timeout-propagation mechanism found; the standard idiom is Rust-side (`tokio::time::timeout(...)` wrapping the operation inside the actor task), which is orthogonal to the FFI boundary — a timeout firing Rust-side simply resolves the future (with an error) the same way any other completion does, which UniFFI already handles via its normal async-completion path (§2.2). This part is a straightforward, low-risk extrapolation from documented Tokio + UniFFI mechanics rather than a genuine gap.

### 6.5 Do UniFFI-generated Swift bindings satisfy Swift 6 strict concurrency, or is the FFI boundary a permanent `@unchecked` escape hatch?

**Documented, current answer: partial, with a specific known hole.** UniFFI's own Swift-bindings documentation states plainly: "UniFFI has partial support for Swift 6, and they welcome all help improving this... most generated code will conform to `Sendable`... at time of writing, it is known that async code will not conform [to `Sendable`]" (Mozilla, n.d.-b). This is corroborated by live, current issue activity:
- GitHub issue mozilla/uniffi-rs#2274: awaiting UniFFI-generated async functions in Swift 6 strict-concurrency mode produces real compiler errors — "Sending 'self'-isolated value of type 'any StoreHandlerProtocol' with later accesses to nonisolated context risks causing data races" (Mozilla, n.d.-n).
- GitHub issue mozilla/uniffi-rs#2818 (filed against Xcode 26 / Swift 6.2): when the consuming Swift module sets `SWIFT_DEFAULT_ACTOR_ISOLATION=MainActor`, generated UniFFI declarations inherit `@MainActor` isolation, which breaks because the generated code relies on raw pointers, `deinit`, and synchronous C interop — none of which can legally be actor-isolated (Mozilla, n.d.-o).
- A documented workaround pattern exists on the Swift side generally for exactly this class of problem (not UniFFI-specific): wrap the non-`Sendable` piece — here, the async completion closure/continuation — in a small `@unchecked Sendable`-conforming type, taking on manual responsibility for the safety UniFFI can't yet prove statically (general Swift-concurrency guidance; search synthesis).
- An experimental UniFFI configuration flag, `experimental_sendable_value_types = true`, is reported by users as reducing (not eliminating) `Sendable`-related warnings when upgrading a UniFFI-bridged project to Swift 6 (search synthesis of developer reports; not confirmed against UniFFI's own changelog in this pass — **flagged as secondary/unverified**).

**Net assessment for the ADR**: as of UniFFI 0.32.0 (2026-06-30) against Swift 6 / Xcode 26, the boundary is **not** a clean, fully-`Sendable`-checked seam — it is closer to "mostly clean, with async call sites specifically requiring either an `@unchecked Sendable` shim or accepting non-strict-concurrency checking at that call site." Given this project's `Recommended`-but-not-yet-ADR'd choice of Swift 6 strict concurrency for the app target, this is a directly load-bearing finding: **the FFI boundary would currently force at least one deliberate, documented `@unchecked Sendable` (or an equivalent local relaxation) at the async-call sites that cross it**, not a hypothetical one — this is documented, current behavior, not speculation.

---

## 7. Testing Without Hardware

### 7.1 — Where should the `SpectroDevice` mock seam live in each shape, to maximize what CI can exercise?

No shipped precedent was found for this exact question (a hardware-peripheral mock seam across a Rust/Swift FFI boundary specifically); the answer below composes documented UniFFI/objc2 mechanics with the project's own fixed constraints (vendor SDK is Swift/ObjC-only; CI has no hardware or license key) and should be read as **reasoned synthesis, not a confirmed pattern from a shipped app**.

- **Shape A (Rust core + SwiftUI shell).** Because the vendor SDK is a Swift/ObjC-only binary framework (a fixed constraint per the brief's Concern 3), the *live* device implementation cannot live in pure Rust — it must be Swift code that either implements a UniFFI "foreign trait"/callback interface generated from the Rust-defined `SpectroDevice` trait (Mozilla/uniffi-rs, "Foreign traits," 2026 docs), or sits above the Rust core entirely. The *mock* implementation, by contrast, can and should be written as a second, pure-Rust implementation of the same trait, living entirely inside the Rust core crate with zero FFI involvement. This means: CI can run `cargo test` / `cargo nextest run` against the Mock-backed core with no Xcode, no Swift toolchain, and no macOS runner at all if the core crate is kept platform-agnostic — the maximum possible fraction of business logic (queue state machine, error/dead-letter handling, derived-value computation, SQLite writes) becomes exercisable on any CI runner. Only the thin Swift-side glue that implements the *live* foreign-trait callback, plus the SwiftUI shell itself, remains untestable without hardware.
- **Shape B (full-Rust, Rust-native UI).** The seam is symmetric and simpler: since there is no Swift layer at all in the happy path, the vendor SDK access itself is the one piece that must cross an FFI boundary (Rust calling the Swift/ObjC vendor framework via `objc2`, per Concern 3) — but the `SpectroDevice` trait, its Mock, and the UI layer are all pure Rust. CI can therefore exercise not just the core logic but also a meaningful slice of the UI layer against the Mock, using the shape-specific UI test harness (egui_kittest, Slint's testing backend, or Tauri's WebDriver tooling — see §7.4), entirely without hardware.
- **Shape C (all-Swift, incumbent baseline).** Per the prior architecture research (already referenced in AGENTS.md §3), the mock lives as a Swift `MockSpectroDevice` conforming to a `SpectroDevice` protocol, injected via `swift-dependencies`. Included here only for contrast — this is not a Rust-specific finding.

**Net comparison:** Shape B maximizes the fraction of the *whole app* (core + UI interaction) that CI can exercise without hardware, because everything but the vendor-SDK call itself is pure Rust behind one trait. Shape A maximizes how much of the *core business logic* is testable in a minimal, fast, Xcode-free CI job, but leaves the SwiftUI shell itself dependent on XCUITest (which does not require hardware, only the Mock wired through the shell — see §7.4). Both shapes achieve the stated goal (device logic testable without hardware or license key) at least as well as the Swift-only baseline; neither was found to have a confirmed shipped-precedent example specific to a BLE/USB instrument.

### 7.2 — Recommended Rust testing stack: versions and conventions

| Tool | Version (2026) | Role | Convention |
|---|---|---|---|
| `cargo test` | bundled with rustc/cargo (no separate version) | Built-in test runner, only tool that runs doctests | Kept as a distinct CI step (`cargo test --doc`) specifically for doctests, since nextest cannot run them (JetBrains, 2026; Pi Stack, 2026) |
| `cargo-nextest` | ~0.9.14x line as of 2026 (e.g. 0.9.140+ builds published within the month of the search) | Faster, process-isolated test runner for everything except doctests | Each test in its own process (leak-safe), parallel binaries, retries, filtersets, JUnit output; the 2026 default for unit/integration tests at any nontrivial scale (JetBrains, 2026a; JetBrains, 2026b) |
| `proptest` | 1.11.0 (released 2026-03-24) | Property-based testing via composable `Strategy` value generation, Hypothesis-inspired shrinking | Preferred over `quickcheck` for new Rust projects in 2026 commentary, because generation/shrinking is defined per-value via `Strategy` objects rather than per-type, which composes better for structured inputs like a `Lab` color triple with physically valid ranges (docs.rs, 2026; LogRocket, "Property-based testing in Rust with Proptest") |
| `quickcheck` (BurntSushi) | actively maintained but the older/simpler of the two | Type-driven `Arbitrary`-based generation + shrinking | Still viable, and a `proptest-quickcheck-interop` crate exists for projects mixing both; dominant 2026 community position favors proptest for new work, with quickcheck acceptable where its simpler type-based model is sufficient (GitHub, BurntSushi/quickcheck; crates.io, proptest-quickcheck-interop) |
| `mockall` | supports Rust 1.77.0+, MSRV-gated | Trait/struct mocking via `#[automock]` or the `mock!` macro | Described in 2026 sources as "the default mocking library for Rust and the one to learn first," with over 100M cumulative downloads (docs.rs, 2026; QASkills, 2026) — directly applicable to mocking the `SpectroDevice` trait in Shape A/B for finer-grained unit tests than a hand-written Mock struct, if per-call assertion (call counts, argument matching) is needed beyond a simple fixture-replay mock |

**Convention for layout:** unit tests inline in `#[cfg(test)] mod tests` next to the code under test; integration tests in a top-level `tests/` directory; doctests kept as a separate, slower CI step since `cargo-nextest` cannot run them (JetBrains, 2026b).

### 7.3 — FFI contract-testing practices, with real project examples

No dedicated "contract-testing framework" for the Rust↔Swift FFI boundary was found; what exists in real shipped projects is a set of practices that prevent *hand-drift* by generating the boundary from a single source of truth, rather than a separate test suite that verifies two independently-written sides stay in sync:

- **UniFFI (Mozilla).** Bindings for every target language (Swift, Kotlin, Python, JS, etc.) are generated from one Rust-side interface definition (either a `.udl` file or, in modern UniFFI, proc-macro attributes directly on the Rust code) — because the Swift API is generated, not hand-written, there is structurally nothing to drift *unless* someone hand-edits generated output (which is explicitly discouraged practice). UniFFI further has a **checksum mechanism**: changing the interface changes an API checksum that generated bindings check against, so a binding built against a stale interface fails loudly rather than silently misbehaving (Mozilla/uniffi-rs, CHANGELOG and GitHub issues, 2026). A `uniffi_testing` crate exists specifically to support testing the generated bindings themselves inside a Rust CI pipeline (crates.io, 2026) — but a still-open GitHub issue on the project ("Figure out how to test generated UniFFI Desktop bindings," mozilla/uniffi-rs#272) indicates this remains a partially unsolved problem for desktop targets specifically, even for Mozilla's own tooling, as of the most recent state visible in this search. UniFFI is used in production at scale for exactly this shape of problem: it "is currently used extensively by Mozilla in Firefox mobile and desktop browsers," generating Kotlin bindings for Android and Swift bindings for iOS from one Rust source (Mozilla, uniffi-rs GitHub README, 2026), and application-services replaced its JS sync engine's tabs component with a UniFFI-bridged Rust implementation as a concrete production example (Mozilla, 2026).
- **libsignal (Signal).** Uses a `bridge_fn` macro family: a single, normal, strongly-typed Rust function is annotated once, and the macro generates the FFI, JNI, and Node.js glue for it automatically, with type mapping handled by the macro rather than hand-written per-language (signalapp/libsignal, GitHub, 2026). The explicit design goal stated in the project is to "generate safe interfaces to Rust APIs, on top of which idiomatic Swift, Java, and TypeScript APIs can be built" — i.e., the same one-source-of-truth principle as UniFFI, implemented with libsignal's own macro system rather than adopting UniFFI itself. The Swift package is built via a `build_ffi.sh` script with a `--generate-ffi` flag when new APIs are exposed (signalapp/libsignal/swift/README.md, 2026), and is shipped as both a CocoaPod and a Swift Package for the real Signal iOS client — a genuine, large-scale, shipped precedent for exactly the FFI shape this project would use.
- **Practical synthesis for SpectroCapture:** the real-world practice is (1) generate the boundary from one source (UniFFI proc-macros on the `SpectroDevice` trait and related types) so there is no second hand-maintained copy to drift; (2) rely on the generator's own checksum/versioning to fail the build on interface mismatch; and (3) add a thin XCTest (or Swift Testing) target in CI that calls every exposed Rust function once against the Mock device through the *actual generated bindings* (not a hand-mocked Swift stub), which is the closest thing to a "contract test" any of the surveyed real projects actually run — this is a **recommended synthesis**, not something directly documented as a named practice in any single source.

### 7.4 — UI-testing story per shape

| Shape / framework | Test tool | Maturity (2026) | Accessibility-tree based? | Screenshot/snapshot support |
|---|---|---|---|---|
| Shape A/C — SwiftUI shell | **XCUITest** | Mature, first-party, years in production use; the strongest story surveyed | Yes — queries the accessibility tree, requires explicit `.accessibilityIdentifier()` tags on SwiftUI views since the tree doesn't map 1:1 onto the view hierarchy (Medium/QASkills/swiftyplace, 2026 sources) | Yes, via `XCTAttachment` screenshots |
| Shape B — egui | **egui_kittest** (built on the general-purpose `kittest` crate, using **AccessKit** for the accessibility tree) | First-party, added in egui 0.30.0; actively developed as of 2026 (emilk/egui GitHub, 2026) | Yes — `kittest` explicitly targets "any Rust GUI," not just egui, via AccessKit | Yes — `Harness::snapshot`, with `snapshot`+`wgpu` features enabled, writes to `tests/snapshots` for regression testing |
| Shape B — Slint | **i-slint-backend-testing** | Explicitly labeled a **"preliminary API"** by its own documentation (docs.rs/crates.io, 2026); Slint's own docs describe integrating tests into CI and VS Code but the crate itself is the least mature of the three surveyed | Partial — supports locating/mutating/verifying element state, not confirmed as a full accessibility-tree query model | Not confirmed in this search |
| Shape B — Tauri | No first-party WebDriver for WKWebView on macOS as of Feb 2026 (Raffel, 2026). Options: **CrabNebula**'s commercial testing service; the community **`tauri-plugin-webdriver`**; or **Tauri-WebDriver**, an independent open-source project (Daniel Raffel, Feb 2026) — a debug-only Tauri plugin plus a `tauri-wd` CLI translating standard W3C WebDriver commands to the plugin's HTTP API, additionally wired into MCP for agent-driven testing | Least mature of the four — stitched together by individual/community effort rather than solved by Apple or Tauri itself; Apple ships no macOS equivalent to Linux's WebKitWebDriver or Windows' Edge WebDriver for WKWebView apps (Raffel, 2026) | DOM-based (standard WebDriver element queries), not accessibility-tree based | Yes, via WebDriver screenshot commands |

**Dominant 2026 verdict:** XCUITest remains the single most mature UI-testing story of any option surveyed, which is a real point in favor of any shape that keeps a genuine SwiftUI shell (A or C). Within Shape B, egui's testing story (egui_kittest, AccessKit-based, first-party, actively developed) is clearly ahead of Slint's (self-described "preliminary") and far ahead of Tauri's (no first-party solution on macOS, reliant on an individual contributor's 2026 project or a commercial service) — this tracks with egui also being the most-downloaded non-webview Rust GUI crate overall per the LogRocket 2026 "state of Rust GUI libraries" survey referenced in Concern 4's research.

### 7.5 — Reference color datasets/fixtures for property-testing conversions

- **Munsell Renotation data** (`all.dat`, `experimental.dat`, `real.dat`) — the standard ground-truth dataset for color-space conversion validation: six columns (Munsell hue, value, chroma, CIE x, y, Y), computed under Illuminant C and the CIE 1931 2° standard observer (search synthesis of colour-science/R `munsellinterpol` documentation, 2026). This is the dataset underlying most "validated against CIE standards" claims found in this research.
- **MunsellSpace** (ChrisGVE, GitHub, 2026) — a Rust crate (with a companion Python package) built directly on the 1943 Munsell renotation studies and later refinements, explicitly claiming validation "against CIE standards and reference RGB values." This is the clearest example found of a Rust crate treating Munsell data as its own ground-truth fixture set, and is the most directly reusable prior art for building a property-test fixture file.
- **colorspace-rs** (anderslanglands, GitHub) — explicitly states its data is "taken from the excellent colour-science python library" (Mansencal et al.), making it a second, independent Rust-side source of cross-validated reference values, though the crate's own maintenance/download profile is far smaller than palette's.
- **RIT Munsell Color Science Lab** (rit.edu) — an institutional source of Munsell/color-science educational and reference material; not packaged as a crate, so a project would need to derive/vendor fixture files from it rather than depend on it as a dependency (rit.edu, Munsell Color Science Lab Educational Resources, 2026).
- **No CIE-specific Rust "fixtures" crate** (e.g., a crate purpose-built to ship the official CIE observer/illuminant tables and known-good round-trip test vectors as `include!`-able Rust constants) was found in this search — this appears to be a gap: every crate found either computes standard-observer math itself (palette) or imports data from Python colour-science by hand (colorspace-rs, MunsellSpace) rather than depending on a shared, canonical Rust "CIE reference data" crate. **Flagged as a genuine ecosystem gap**, not merely an unsearched corner.

**Recommended testing composition (synthesis, not directly documented as a named pattern in any single source):** pair `proptest` for round-trip/invariant properties (e.g., "for any Lab value within the physically valid gamut, `XYZ→Lab→XYZ` is within ε of identity"; "ΔE2000(c, c) == 0 for all generated colors") with a small fixture file of golden values — either sourced from the Munsell renotation dataset directly, or exported once, offline, from Python `colour-science` and checked into the repo as static test data — asserted with exact/near-exact equality against palette's (or whichever crate's) output. This combination catches both structural bugs (round-trip properties, via proptest's shrinking) and absolute correctness against external ground truth (via golden fixtures), and directly closes the validation gap identified in §5.1.

---

### Closing Notes for These Two Sections (Areas 5 & 7)

Nothing in Concern 5 or Concern 7 surfaced a hard blocker to Rust as a viable data/color-math layer — the crate ecosystem (palette, lcms2, rusqlite, csv, proptest, cargo-nextest, mockall, UniFFI) is real, versioned, and in active 2026 use at meaningful scale. The two sharpest open risks this research surfaces, both flagged inline above, are: (1) **no surveyed color-math crate documents its own validation against Python colour-science or an ICC reference implementation** — this validation work would need to be done by the project itself, not assumed as already-done upstream; and (2) **no Rust SQL crate has an analog to GRDB's `ValueObservation`** — a live-updating collection-browsing UI would require hand-rolling change notification on top of rusqlite/diesel/sqlx, a real architectural cost not present in the Swift-only baseline.

---

## 8. Build, Signing, Distribution

**Scope of this document:** Concern Area 8 (Build, Signing, Distribution) and Concern Area 9 (Contributor Ecosystem & DX) only, per the research brief `docs/briefs/rust-primary-language-research-brief.md`. All other concern areas are out of scope and answered elsewhere.

**Research date:** 2026-09-01. Versions verified as of this date via web search; where a source could not confirm a version, this is flagged.

---

### 8.1 — Canonical pipeline for building, signing, and notarizing a Rust or mixed Rust+Swift `.app`: cargo-bundle, cargo-dist, Tauri's bundler, or Xcode driving everything. What do teams that actually ship use, at which versions?

There is no single "canonical" pipeline; the honest answer is that the choice bifurcates hard by architecture shape, and the tooling for shape A (Rust core + Swift shell) and shape B (full-Rust UI) are essentially disjoint ecosystems.

**For shape B (full-Rust UI, e.g. via Tauri):** the dominant, actually-shipping pipeline is **Tauri's own bundler** (`tauri-bundler`, distributed as part of the `tauri` CLI, current stable **Tauri v2.10.1**, released 2026-03-04 per the Tauri release history) (Tauri Contributors, 2026). `tauri-bundler` is itself a fork of `cargo-bundle` that Tauri turned into a library (Kruckenberg, n.d.; tauri-apps, 2026a). It has first-class, built-in Developer ID signing and Apple notarization support: `tauri.conf.json`'s `bundle.macOS` section accepts a `signingIdentity` and `providerShortName`, and `tauri build` signs and submits for notarization automatically when Apple credentials (App Store Connect API key or Apple ID app-specific password) are supplied as environment variables (Tauri Contributors, 2026b). Multiple independent 2026 how-to posts confirm teams use this in production, including full GitHub Actions release pipelines built around it (du, 2026a, 2026b; Hiyoyok, 2026). GitButler — a real, non-trivial, open-source Tauri/Rust/Svelte desktop app — ships this way and documents Xcode + cmake as macOS build prerequisites even though the UI is not native (GitButler, 2026).

**`cargo-bundle`** (the tool `tauri-bundler` forked from) still exists independently (`burtonageo/cargo-bundle`, latest release 0.10.0, 2026-04-18) but its own README states it is **"Very early alpha… no guarantee of stability"** and it documents no notarization support at all — only `.app` bundle creation (burtonageo, n.d.). It is not a shipping-grade signing/notarization solution on its own; teams that use raw `cargo-bundle` layer their own `codesign`/`notarytool` shell scripts on top (Kanoldt, n.d.), which is exactly the DIY pattern Tauri's bundler was built to avoid.

**`cargo-dist`** (axodotdev, current version **0.32.0**, released 2026-05-21) is a release-orchestration tool (builds, packages, and publishes GitHub Releases/installers across platforms) rather than a signing tool. It has received fixes so that macOS codesigning configuration is respected, but does not itself implement notarization logic; codesigning integration is an ongoing, incrementally-built feature area (issue #1121) (axodotdev, 2026a, 2026b). It is a better fit for CLI-tool distribution than a signed, notarized `.app` GUI pipeline.

**For shape A (Rust core + SwiftUI shell):** none of the above apply as the top-level driver. The shipping pattern is **Xcode driving everything** — the Rust core is built as a static library/XCFramework and consumed as a normal SPM/Xcode dependency, and Xcode's own `xcodebuild -exportArchive`/notarytool pipeline (or `fastlane`) handles signing and notarization exactly as it would for an all-Swift app. This is the pattern documented for shipping Rust-via-UniFFI as Swift Packages (Mozilla application-services team; Stadia Maps' Ferrostar SDK) (Mozilla, n.d.; Stadia Maps, n.d.) — Rust becomes an upstream build dependency, not the thing driving code signing.

**Comparison table:**

| Tool | Best fit | macOS notarization built in? | Maturity (2026-09) |
|---|---|---|---|
| Xcode / `xcodebuild` + `notarytool` | Shape A (Rust core, Swift shell) | Yes (native Apple toolchain) | Mature, unchanged by Rust presence |
| Tauri bundler (v2.10.1) | Shape B (Tauri UI) | Yes, first-class | Production-proven (GitButler and others) |
| `cargo-bundle` (0.10.0) | Neither — building block only | No | "Very early alpha" per its own README |
| `cargo-dist` (0.32.0) | CLI tool releases, not signed GUI apps | Partial/in progress | Actively developed, not GUI-signing-focused |

**Flag:** which pipeline "teams that actually ship use" for shape A specifically (Rust-core-under-SwiftUI, as distinct from Tauri) is **inferred** from the general Xcode-driven-build pattern documented for UniFFI/Swift-Package consumption, not from a named shipped-app's public CI config — no shipped macOS-app writeup was found that describes its exact Xcode+Rust signing/notarization pipeline end to end. Confidence: high for shape B (multiple concrete 2026 writeups), lower for shape A specifics (inferred from build-integration docs, not a signing-pipeline writeup).

---

### 8.2 — Exact mechanism for producing a universal (arm64 + x86_64) binary from a mixed cargo/SPM build, and CI time cost

**Mechanism:** Cargo has no native universal-binary support — this is a tracked, still-open upstream limitation (`rust-lang/cargo` issue #8875) (rust-lang, n.d.). The documented pattern, used consistently across sources, is:

1. Add both Apple targets to the toolchain: `rustup target add x86_64-apple-darwin aarch64-apple-darwin`.
2. Build each target separately: `cargo build --release --target x86_64-apple-darwin` and `cargo build --release --target aarch64-apple-darwin`.
3. Merge the two architecture-specific binaries (or static/dynamic libraries) into one fat binary with Apple's `lipo -create -output universal_binary x86_64_binary arm64_binary` (Israel, n.d.; RustFAQ, n.d.; Apple, n.d.).

For a static library consumed via SPM/XCFramework (the shape-A path), `lipo` is run on the two `.a`/`.dylib` outputs before `xcodebuild -create-xcframework` packages them, matching Apple's own documented universal-binary process for non-Swift dependencies (Apple, n.d.).

**cargo-dist does not automate this today.** `axodotdev/cargo-dist` issue #77, opened 2023-02-01, explicitly requests exactly this ("bundle both the x86_64 and the arm64 binary into a single file using `lipo`") and **remains open with no PR or resolution** as of this research pass (axodotdev, 2023). Teams using cargo-dist for macOS today either ship two separate architecture-specific artifacts or bolt on their own `lipo` step outside cargo-dist. Tauri's bundler, by contrast, does support building for `universal-apple-darwin` directly via its `--target universal-apple-darwin` build flag, compiling both architectures and invoking `lipo` internally as part of `tauri build` (tauri-apps, 2023, referenced via issue #3317 discussion) — this is a **documented capability, not independently re-verified against the live v2.10.1 CLI in this pass**, so it is flagged as inferred-from-issue-discussion rather than confirmed against current docs.

**CI time cost:** No source gives a precise "N extra minutes" figure for universal-binary Rust builds specifically. What is documented: (a) macOS GitHub Actions runners cost **$0.062/minute** as of 2026-01-01 (down from $0.080), roughly **10x the Linux per-minute rate** (GitSpider, 2026; cicdpipelinecost.com, 2026); (b) building two full architecture targets sequentially in CI roughly doubles the Rust-compile portion of the pipeline versus a single-arch build, since Cargo does not parallelize cross-target compiles within one `cargo build` invocation by default — each `--target` invocation is a separate full compile graph. This doubling is a **reasonable inference from how `cargo build --target` works**, not a documented benchmark; no source directly measured "single-arch Rust build vs. universal Rust build" CI minutes for a comparable app-sized crate. **Flag: inferred, not measured** — a concrete follow-up is given in the Question Status section below.

---

### 8.3 — Does Sparkle integrate cleanly with a non-Xcode or full-Rust app? What are the alternatives?

**Sparkle with Tauri:** yes, via a third-party community plugin, `tauri-plugin-sparkle-updater` (current version **0.2.2** per docs.rs), which vendors the Sparkle framework via a download script (not requiring Xcode), exposes a `SparkleUpdaterExt` trait to the Tauri Rust builder, and can be conditionally compiled to fall back to Tauri's own built-in updater on non-macOS platforms (ahonn, n.d.). This is a **third-party, single-maintainer plugin**, not an official Tauri or Sparkle-project artifact — its long-term maintenance status is unverified beyond its crates.io/docs.rs presence, and it should be treated as a smaller-community dependency than Sparkle-via-Xcode-integration would be.

**Sparkle from raw Rust (no Tauri):** Sparkle's own documentation describes an Xcode-centric setup flow (Swift Package Manager integration inside an Xcode project, code-signing keys generated via Xcode-adjacent tooling) (Sparkle Project, n.d.). Driving Sparkle's Objective-C/Swift API directly from a full-Rust binary (shape B without Tauri, or the device layer of shape A) would go through **objc2** — a mature, actively maintained Rust↔Objective-C interop crate family with dedicated `objc2-app-kit` and framework-specific binding crates for calling AppKit/Foundation directly (madsmtm, n.d.). No source documents someone having actually wired Sparkle to a raw (non-Tauri) Rust app via objc2 end to end; this combination appears technically plausible from the primitives available (objc2 can call arbitrary Objective-C frameworks, and Sparkle is a standard Objective-C framework) but is **not a documented, shipped pattern** — flagged as inferred/no direct precedent found.

**Alternatives to Sparkle:**
- Tauri's own built-in updater (cross-platform, JSON-manifest-driven, works without Sparkle at all) — the most-documented alternative for shape B (ahonn, n.d., implicitly; Tauri Contributors, 2026b).
- For shape A (Xcode-driven build), plain Sparkle-via-Xcode remains the default, unaffected by Rust's presence in the core — Rust is beneath the update mechanism, not part of it, as long as the update payload only replaces the `.app` bundle wholesale (which is Sparkle's normal model).

**Community position:** no divided opinion found — the small existing evidence base converges on "Sparkle is Xcode/Swift-native by default; bolt it on via a community plugin (Tauri) or objc2 (raw Rust) if you need it outside that context," with the Tauri-plugin path being the only one with a documented, versioned artifact.

---

### 8.4 — Does Rust introduce friction with the App Sandbox and hardened runtime (entitlements, library validation)? Does it bias the still-open sandbox decision either way?

**No Rust-specific friction was found with the Hardened Runtime itself.** Hardened Runtime is mandatory for notarization regardless of source language and regardless of sandboxing (Eclectic Light Co., 2021) — it restricts loading of unsigned/unvalidated code and writable+executable memory, which a normally-compiled, normally-signed Rust static library linked into the same signed `.app` binary satisfies exactly as a Swift/ObjC one would.

**Library Validation** is the one place a Rust core could plausibly matter, but only in the specific case of loading Rust code as a *separate, independently-built dynamic library at runtime* rather than statically linking it into the main signed binary or a single signed framework. Library Validation (a Hardened Runtime sub-restriction) requires every loaded library to be signed by the same Team ID as the main executable; disabling it (`com.apple.security.cs.disable-library-validation`) is described as widening attack surface and should only be requested for what is actually used (Medium/macoclock, n.d.). For shape A, if the Rust core is built as an XCFramework/static library and linked directly into the app binary (the standard UniFFI/Swift-Package pattern) (Mozilla, n.d.), it becomes part of the same signed executable and this restriction does not apply — **the friction is avoidable by build choice, not inherent to Rust.**

**On the open sandbox decision:** no evidence was found that a Rust core or Rust UI stack biases the App Sandbox decision either way. Sandboxing and Hardened Runtime are independent systems from code signing and from source language (Medium/macoclock, n.d.); a sandboxed app's entitlement needs (BLE/USB device access, file access for the SQLite store, network — none needed here) are determined by what the app does, not what language it's written in. The one place shape A could add sandbox friction is if the Rust core needs raw BLE access outside the vendor SDK's own entitlement story — but per the fixed constraint that the vendor SDK owns the BLE/USB transport, this does not arise. **This question's core claim ("Rust biases the sandbox decision") appears to be a non-issue** — flagged explicitly since the brief asks for an honest answer and the honest answer is "no material bias found."

---

### 8.5 — GitHub Actions macOS CI for a mixed Rust+Swift toolchain vs. pure Swift: cache strategy, cold/warm build minutes, known flakes

**Cost baseline:** GitHub-hosted macOS runners cost **$0.062/minute** as of 2026-01-01 (a 22.5% cut from the prior $0.080/minute), and are consistently reported as roughly **10x the price of Linux runners** — the single biggest lever for total CI cost on this project regardless of language choice (GitSpider, 2026; cicdpipelinecost.com, 2026; StackTrack, 2026).

**Cache strategy — pure Swift:** SwiftPM dependency resolution/build reported at 3–5 minutes cold, and a from-zero Xcode DerivedData build at 5–10 minutes; combined with CocoaPods/gem dependencies where present, cold CI runs of 15+ minutes are typical, with caching (DerivedData, SPM `.build`, Homebrew) potentially saving 10–12 minutes per run (No-Wham Dev, n.d.). Standard approach: `actions/cache` keyed on `Package.resolved`/`.xcodeproj` hashes for SPM artifacts and DerivedData.

**Cache strategy — Rust addition:** the standard tool is `actions/cache`-based (via the `Swatinem/rust-cache` action, commonly referenced as "Rust Cache" on the GitHub Marketplace) or `sccache` (Depot, n.d.; GitHub Marketplace, n.d.). A documented, real caveat: **GitHub Actions runners are ephemeral and don't share disk across jobs**, so Cargo's `target/` directory — which grows unboundedly and accumulates artifacts not actually needed for the next build — is a poor `actions/cache` candidate at scale; teams commonly report cache restores pulling down large tarballs of only partially-useful build artifacts (Infinyon, 2021; BuildPulse, n.d.). `sccache` (a distributed/local compilation cache) is the more commonly recommended fix for this specific problem (Depot, n.d.).

**Cold/warm numbers for a mixed toolchain specifically:** **no source was found that measured cold vs. warm build minutes for a genuinely mixed Rust+Swift macOS CI pipeline.** All available numbers are single-language (Swift-only build-time figures above; generic Rust cache-effectiveness discussion with no macOS-specific or Swift-adjacent numbers). This is a real, flagged gap — see Question Status. What can be said with confidence, by composition rather than direct measurement: a mixed pipeline's cold-build cost is *additive*, not shared — the Rust `cargo build` and the Swift/Xcode build are two independent compile graphs with no cache overlap, so a mixed-toolchain cold CI run should cost roughly the sum of a Rust-only cold build and a Swift-only cold build, plus universal-binary doubling (§8.2) if both architectures are built in the same job. **This additive-cost claim is inferred from how the two toolchains' build graphs are structured, not measured.**

**Known flakes (language-agnostic but load-bearing for signing, which both shapes need):** the dominant, well-documented failure mode in GitHub Actions macOS CI is **keychain-related hanging during codesigning** — importing a signing certificate into the default login keychain causes CI to hang indefinitely waiting for a password prompt with no visible UI; the standard fix is a dedicated, explicitly-unlocked temporary keychain with `security set-key-partition-list` and an extended lock timeout (`-lut 21600`, i.e. 6 hours) (cmsj.net, 2021; CodeJam, 2025; MSicc, n.d.). A second documented flake: importing **multiple certificates separately** into the same workflow run via common third-party import actions can fail due to how those actions manage keychain state; combining certificates into a single `.p12` before import is the documented workaround (MSicc, n.d.). A third, notarization-specific flake: Apple's notarization service is reported to usually take 2–5 minutes but has been observed to take **15–20 minutes or, in a documented Tauri GitHub Discussion, over an hour**, with builds appearing "stuck at notarizing" (Tauri Contributors, 2026a discussion #8630; dev.to, 2026c). None of these three flakes are Rust-specific — they apply identically to a pure-Swift Xcode pipeline — but they are the dominant *macOS CI* flakes this project will hit regardless of language shape chosen, and are worth budgeting for.

---

## 9. Contributor Ecosystem & DX

### 9.1 — Contributor-pool evidence: do Rust projects of comparable size attract more or fewer drive-by contributors than Swift projects (Octoverse, surveys, comparable-app contributor counts)?

**Octoverse itself is a weak instrument for this specific question.** GitHub's Octoverse 2025 headline report focuses its detailed rankings/growth tables on TypeScript, Python, JavaScript, Java, and C# — Rust and Swift are both acknowledged as part of the broader "widely adopted" language set but **neither appears in Octoverse 2025's detailed ranking tables or growth charts** (GitHub, 2025, verified by direct fetch of the report — confirmed absent). Any claim that Octoverse "shows" Rust beating or losing to Swift on contributor growth is not supportable from the 2025 report as published; this is a genuine evidence gap, not an oversight in this research pass.

**What the evidence does show, at the language-interest/salary level (a proxy for contributor supply, not a direct contributor-count comparison):**
- Rust was the **most admired language** in the 2025 Stack Overflow Developer Survey at **72%** admiration (developers who use it and want to continue), ahead of Gleam (70%), Elixir (66%), Zig (64%) (Stack Overflow, 2025, via multiple secondary confirmations — the "most admired" table for Swift specifically could not be retrieved directly from the source page in this pass, a flagged gap).
- Usage share (2025 SO Survey, all respondents): **Rust 14.8%**, **Swift 5.4%** — professional developers: Rust 14.5%, Swift 5.7% (Stack Overflow, 2025, direct fetch of survey.stackoverflow.co/2025/technology). Rust's usage share is roughly **2.7x** Swift's among all respondents on this survey, though this measures a global cross-platform developer population, not specifically people who would contribute to a macOS-only open-source app — Swift's usage is structurally capped by being Apple-platform-only while Rust is cross-platform, so this comparison is not apples-to-apples for a macOS-only project's *specific* contributor pool.
- Rust carries a documented **24% salary premium over the median developer** in 2026 per Octoverse-adjacent reporting (itsfoss.com, 2025/2026, summarizing Octoverse 2025) — relevant as an indicator of scarce/high-demand skill, not directly of open-source drive-by willingness.
- JetBrains' State of Rust Ecosystem 2025 survey (24,534 respondents, fielded April–June 2025) found **2.27 million developers used Rust in the past 12 months**, of whom 709,000 use it as their primary language; 65% run it on side/hobby projects and only 26% use it professionally — i.e., a large share of the Rust-using population is exactly the "hobbyist/side-project" demographic that plausibly maps to drive-by open-source contribution (JetBrains, 2026). No equivalent figure for Swift's hobbyist-vs-professional split was found in this pass.

**Comparable shipped-app contributor counts:** direct, apples-to-apples contributor-count comparisons between comparable-scope Rust and Swift open-source macOS/desktop apps were **not conclusively found**. Zed (Rust, GPUI, open-sourced 2024) is repeatedly cited as actively courting outside contributors (its engineering blog runs a public series specifically about hiring/crediting contributors sourced through GitHub) (Zed Industries, n.d.), but no source gave a specific total contributor count. Comparable Swift-only shipped macOS apps (IINA, and the broader `awesome-swift-macos-apps`/`awesome-native-macosx-apps` curated lists) were located, but again without a directly comparable contributor-count figure pulled in this pass (multiple curators, n.d.). **This is the weakest-evidenced sub-question in Section 9** — flagged as a real gap with a concrete follow-up below, not something this pass can respectably close by inference.

**Dominant position where evidence is thin but suggestive:** the closest thing to a "community consensus" position, weighing the SO2025 admiration/usage gap and the JetBrains hobbyist-share data together, is that Rust currently has a *larger and more enthusiastic pool of developers predisposed to want to contribute to Rust code* than Swift has of developers predisposed to want to contribute to Swift code outside employer contexts — but this is a language-preference argument, not a *demonstrated* drive-by-PR-rate comparison for comparable projects, and should be weighted accordingly in the ADR.

---

### 9.2 — Realistic cold/warm build times, app-sized Rust vs. comparable Swift, and FFI-boundary slowdown of the edit-compile-run loop in shape A

**No source gives directly comparable, controlled cold/warm build-time numbers for an "app-sized" Rust codebase vs. a comparable Swift one.** What is documented, non-comparably:
- Swift/Xcode: 3–5 min SwiftPM dependency resolution, 5–10 min from-zero DerivedData build, 15+ min combined cold CI with CocoaPods/Ruby deps present (No-Wham Dev, n.d.) — but this is a generic iOS-CI-caching post, not benchmarked against a specific app size.
- Rust: widely known (general knowledge, not from a specific 2024–2026 source in this search pass) that Rust's borrow-checker and monomorphization work make full/cold builds slower than comparable Swift builds at the same LOC, while Rust's incremental-compilation and `sccache`/cache-hit warm builds are frequently reported as fast once primed — but the only source surfaced that speaks to this directly (dakharlamov, n.d.) discusses *runtime* performance benchmarks (a Brainfuck-interpreter microbenchmark: Rust 5.5x faster, 9x less memory than Swift 6.1.2), **not compile time**, and should not be miscited as compile-time evidence. **This is flagged explicitly: no reliable 2024–2026 primary source quantifying cold/warm compile-time delta between Rust and Swift for an app-scale codebase was found.**

**FFI-boundary slowdown of edit-compile-run in shape A:** no source directly measures this either. The structurally-inferable claim (not measured) is that shape A's edit-compile-run loop is slower than either single-language shape whenever a change crosses the boundary, because: (a) UniFFI/`cargo swift`-style toolchains regenerate Swift bindings from a Rust build artifact as a discrete step (not a single unified incremental build graph) (antoniusnaumann, n.d.; Stadia Maps, n.d.), and (b) the Swift side must then pick up the regenerated binding/XCFramework and re-link, which is a second, separate build invocation Xcode has to notice and act on. A change confined entirely to the Rust side (no interface change) or entirely to the Swift side does not pay this cost — only signature/type changes crossing the boundary do. **This entire sub-answer is an inference from the documented two-step build mechanics, not a measured DX report** — flagged, with a concrete follow-up below.

---

### 9.3 — Day-to-day IDE reality in a mixed repo: rust-analyzer + Xcode side by side vs. pure Xcode. What breaks (jump-to-definition across the boundary, refactoring, debugging)?

**This is one of the thinner-evidenced questions in this brief.** Direct findings:
- rust-analyzer is documented as an LSP implementation with first-class support in VS Code, and community/CLI support in Emacs and Vim; **Xcode is not listed among rust-analyzer's supported/documented editors** (rust-analyzer.github.io, n.d.). Community discussion on the Rust users forum states plainly: "Xcode is just not set up for Rust development" (Rust Users Forum, n.d.).
- The one piece of concrete Xcode↔Rust IDE tooling found is `rust-xcode-plugin`, which some Rust-toolchain installers add automatically and which provides **syntax highlighting only** — not jump-to-definition, refactoring, or rust-analyzer-grade semantic features inside Xcode itself (Rust Users Forum, n.d.).
- The practical, repeatedly-implied pattern (not stated as a formal recommendation anywhere, but consistent across every FFI-tooling doc surveyed) is: **two separate editor contexts** — VS Code (or another rust-analyzer-capable editor) for the Rust crate, Xcode for the SwiftUI shell and for anything Interface-Builder/SwiftUI-preview-dependent — with no unified single-IDE experience spanning both languages. This matches the general macOS-Rust-development literature's framing of "expose a C FFI to the Swift code" as the standard shape-A pattern, implying two separate toolchains from the outset (Rust Users Forum, n.d.).
- **Debugging across the boundary, symbolication of Rust frames in crash reports, and lldb-over-mixed-frames** are covered under Concern Area 2, Question 6 — not re-answered here.

**What breaks, specifically, based on available evidence:** jump-to-definition, rename-refactor, and find-references all stop at the FFI boundary — there is no tool found in this research pass that provides unified semantic navigation from a Swift call site into the Rust function it's bound to, or vice versa (this is an **inference from the absence of any tool claiming this capability**, not a documented "it breaks" report from a team that hit it — no team's own postmortem or blog post narrating this specific pain point was located). **Flagged as inferred, not documented** — see Question Status for a concrete way to close this gap.

---

### 9.4 — What do teams report about onboarding contributors onto a two-language codebase? Does shape A effectively require every core contributor to know both languages?

**The strongest concrete evidence found is a negative case, not a positive onboarding report:** Automattic's WordPress-rs team began with Rust as the shared core driving native Kotlin (Android) and Swift (iOS) clients, and explicitly **pivoted away from that shape**, demoting Rust to a "helper library" rather than the central component. The stated reason (from the team's own PR discussion) was specifically an **FFI-callback/async problem on the Kotlin/JVM side** — "calling Kotlin from Rust is less than ideal, as JVM can drop the memory or the memory might be in a different thread" — which the team characterized as building "on a house of cards," and they chose the simpler helper-library shape specifically to eliminate the need for Rust-to-native callbacks and cross-boundary async (Automattic, 2024/2025, PR #8). **Caveat: this is an Android/Kotlin FFI problem, not a Swift-specific one** — Swift's UniFFI async story is generally reported as more mature than Kotlin's in the same ecosystem (a claim made elsewhere in cross-boundary FFI literature but not independently re-verified in this pass) — so this precedent transfers only partially to the Swift side of a shape-A SpectroCapture. It is nonetheless the clearest documented case in this research pass of a real team finding two-language-core complexity costly enough to walk back, and it directly supports the brief's implicit worry about shape A's overhead.

**On whether shape A requires every core contributor to know both languages:** no source directly answers this for a Rust-core/Swift-shell app. The structurally-inferable answer, drawn from how the ownership boundary is typically described in the FFI/build-integration literature surveyed (Mozilla application-services, Bitwarden SDK, Stadia Maps Ferrostar): **no** for *casual* contribution — a contributor can meaningfully work on either the Rust core (adding logic, fixing a bug in color math) or the SwiftUI shell (UI polish, a new screen) without touching the other language, as long as the FFI *interface* itself (the UniFFI-annotated types/functions) doesn't need to change. **Yes** for anyone whose work requires *changing the shape of what crosses the boundary* — adding a new async stream, a new struct field visible to Swift, a new error case — because that requires editing the Rust `#[uniffi::export]` surface, regenerating bindings, and then writing the Swift-side consumption code, which by construction touches both languages. Bitwarden's own SDK architecture documentation frames the Rust layer explicitly as "the single source of truth for business logic" with UniFFI-generated Kotlin/Swift bindings as a mechanical, generated consumption layer (Bitwarden, n.d.) — implying the *intended* contributor split is Rust-core contributors vs. platform-shell contributors as two largely separate populations, with only interface-boundary changes requiring both. **This is inferred from architecture-documentation framing across three real projects (Bitwarden, Mozilla application-services, Ferrostar), not a direct "here's what we found when onboarding contributors" report** — flagged, with a concrete follow-up below.

---

### 9.5 — Honest 2024–2026 evidence that Rust reduces defect rates for this class of app — desktop, IO-bound, GC-irrelevant, not memory-pressured — as opposed to systems software where the safety argument originated

This is the question this research pass can answer with the most confidence, precisely because the honest answer is that **the strongest 2024–2026 evidence base does not transfer cleanly to SpectroCapture's actual workload**, and saying so plainly is more useful than padding the section with systems-software statistics dressed up as desktop-app evidence.

**The headline 2024–2026 evidence, and why it's the wrong shape of evidence for this app:**
- Google reports Android's Rust-adoption program correlates with a roughly **1000x reduction in memory-safety vulnerability density** versus its C/C++ code, with memory-safety vulnerabilities falling from 223 (2019) to under 50 (2024), and dropping below 20% of total vulnerabilities for the first time in 2025 data; Rust code in this program also showed a **4x lower rollback rate** and **~20% fewer revisions** than C++ counterparts, with 25% less time in code review (Google, 2024/2025; The Hacker News, 2025; Slashdot, 2025).
- A cited 2026 IEEE-associated benchmark of the Linux kernel found **zero memory vulnerabilities in Rust code versus 12.3 per 1,000 lines in C++** for the kernel's Rust subsystems (cited via secondary reporting, primary IEEE source not independently verified in this pass — flagged).
- Academic work (Xu et al., cataloged via ACM TOSEM and follow-on arXiv benchmarking work) has systematically studied *all* published Rust CVEs and finds the overwhelming majority trace back to `unsafe` code blocks, not safe-Rust logic errors (Xu et al., 2021; follow-on 2024–2026 CVE-benchmarking work, arXiv, 2026).

**Why this doesn't transfer directly:** every one of these results comes from **memory-unsafe-language rewrite contexts** — Android's C/C++ codebase (untrusted-input parsers, media codecs, kernel drivers), and the Linux kernel itself — i.e., exactly the systems-software domain the brief asks to distinguish this app from. The mechanism by which Rust reduces defects in these studies is specifically **eliminating memory-corruption bugs (use-after-free, buffer overflows, data races) that arise from manual memory management and unsafe pointer arithmetic** — categories of bug that a memory-safe garbage-collected-or-ARC language (Swift, via ARC) is **already largely immune to for the same reason Rust is**, just via a different mechanism (automatic reference counting rather than ownership/borrowing). SpectroCapture's actual hot paths as described in the brief — SQLite reads, color-space math, BLE event handling at hardware pace via a vendor SDK — are IO-bound and logic-bound, not the class of buffer-parsing/pointer-arithmetic code where these studies' mechanism applies.

**The nuanced, honestly-divided community position:** general commentary (not a single authoritative source, but a consistent theme across the ACM Queue "Memory Safety for Skeptics" piece and multiple 2026 retrospective posts on Rust rewrites) holds that Rust's measured defect-rate wins are real and substantial **specifically for code that was previously written in C/C++ and specifically for memory-corruption-class bugs**; demands for large-scale rewrites into Rust are frequently criticized as "aesthetic criticisms of code that looks old" rather than "carefully reasoned arguments about trade-offs," and the more careful framing is that a rewrite is strongly justified when a project (a) is currently in a memory-unsafe language, (b) handles untrusted/adversarial input, (c) has real performance requirements, and (d) involves substantial concurrent code — with the explicit caveat that **outside those conditions, "rapid prototyping, stateful business logic, [and cases where] developer velocity matters more than memory safety" are exactly where Rust's case weakens** (JetBrains Rust blog, 2026; ACM Queue, 2026 [cacm.acm.org/practice/memory-safety-for-skeptics]). SpectroCapture — Swift already being memory-safe via ARC, single-user local file, no untrusted network input, no adversarial data beyond a malformed CSV import (a bounded, already-well-understood defect class regardless of language) — sits squarely in the "outside those conditions" bucket by this framing.

**Bottom line for the ADR:** there is no 2024–2026 study found in this pass that measures Rust-vs-Swift (or Rust-vs-any-ARC-language) defect rates for IO-bound, non-adversarial-input desktop application logic specifically. The closest available evidence (Android, Linux kernel) measures a different mechanism (Rust vs. manual C/C++ memory management) solving a different problem (memory corruption from unsafe pointer use) that Swift's ARC already mostly solves by a different route. **Citing the Android/kernel numbers as evidence for a Rust-core-over-Swift defect-rate win in this specific app would be a misapplication of the cited studies' actual scope**, and the ADR should not do so without this caveat attached.

---

### Closing Note for Sections 8–9 (feeds the brief's overall Closing Recommendation, not authored here)

Two load-bearing findings from this pass that should weigh heavily in whichever shape the final ADR recommends, regardless of which way it goes:

1. **Section 8's tooling maturity gap is asymmetric and real.** Shape B (full-Rust UI via Tauri) has a genuinely production-proven, versioned, actively-shipping signing/notarization/universal-binary pipeline (Tauri v2.10.1). Shape A (Rust core + SwiftUI shell) has *no* equivalent single tool — it falls back to Xcode's native pipeline treating the Rust core as "just another linked static library," which works but has no shipped-precedent writeup confirming the exact mechanics end-to-end were found in this pass. `cargo-bundle` and `cargo-dist`, the two Rust-native alternatives, are respectively "very early alpha" and missing native macOS universal-binary/notarization support as of 2026-09.

2. **Section 9's strongest piece of contributor-ecosystem evidence (WordPress-rs's retreat from a Rust-core shape) is a documented case of a real team finding two-language FFI complexity costly enough to reverse**, and Section 9.5's honest reading is that the defect-rate case for Rust — the argument most likely to be reached for to justify the two-language cost — does not cleanly apply to this app's actual (IO-bound, ARC-already-memory-safe, non-adversarial-input) workload. Both findings should be read as points *against* underweighting shape A's/B's real coordination costs, not as an argument for either specific shape — that tradeoff belongs in Concern Area 10's decision framework, which this document does not address.

---

## 10. Decision Framework & Risks

### 10.1 Published decision criteria for native-vs-Rust-core choices, and which apply at this project's scale

No single "Rust-core decision rubric" document was found; the applicable criteria have to be assembled from adjacent sources, and most published frameworks are implicitly about *larger* orgs than this project:

- **Coordination-cost / team-size criterion** (Pike, 2021): shared-core architectures pay off once the cost of keeping N platform-native codebases in sync exceeds the cost of the shared-core abstraction tax. **Does not apply**: SpectroCapture targets one platform, so there is no cross-platform consistency problem for a shared core to solve.
- **Language-fit / workload criterion** (Welsh, 2022): Rust's tradeoff (safety over developer velocity) pays off specifically for CPU/memory-pressured, concurrency-heavy, or safety-critical workloads; for a conventional CRUD-shaped app, "the problems Rust is designed to avoid can be solved other ways." **Partially applies**: the brief's own framing (§1, non-negotiables) explicitly describes this app's real hot paths as SQLite reads, per-scan color math, and hardware-paced BLE events — none of which resemble the GC-latency-under-load problem that drove Discord's Rust rewrite (Discord, n.d.) (see 10.2). Under Welsh's criterion, this cuts against a performance rationale for Rust here.
- **Hiring/fungibility criterion** (Welsh, 2022): a Rust-only or Rust-plus-X team is harder to hire into and creates a knowledge schism between contributors who know Rust and those who don't. **Applies directly to an open-source project** more than to Welsh's VC-funded startup context, since OSS contribution pools are drive-by and voluntary rather than hired and onboarded (this specific angle is Concern Area 9's territory, not evaluated in depth here).
- **Reversibility/optionality criterion**: not explicitly published as a named framework anywhere found, but implicit in both the Dropbox/Slack reversal (Guthmann, cited in InfoQ, 2019; Slack Engineering, 2019) and in Pike's framing (Pike, 2021) — see 10.4.

No engineering organization was found to have published a decision framework calibrated to a **solo-to-small open-source team with no existing code**, which is this project's actual situation; every criterion above is extrapolated from teams that were larger, funded, and/or already had a live codebase before the language question arose. Flagged as an evidentiary gap — see Question Status.

### 10.2 Is Rust's performance advantage material or noise for this app's real hot paths?

No direct measurement of this app's specific hot paths (SQLite reads over ~10k rows, per-scan color-space math, hardware-paced BLE events) was found or could be found without a working prototype — this answer is **necessarily inferred from adjacent measurements and workload analysis, not from a study of this app**:

- **SQLite reads at ~10k rows**: GRDB's own published benchmarks (run on a MacBook Pro, Xcode 16.2, GRDB 7.0.0) show 200,000-row fetches and 50,000-row inserts as the *baseline* workload class they benchmark against other Swift persistence layers (GRDB.swift wiki, n.d.). 10k rows is two orders of magnitude below that baseline. `rusqlite` is a thin wrapper that "performs on par with SQLite... with rusqlite abstractions introducing minor overhead" (w3resource, n.d.) — i.e., both languages are bottlenecked by SQLite itself at this scale, not by the host language. **Conclusion: noise.** No specific 10k-row Swift-vs-Rust SQLite comparison was found; this is inferred from the GRDB benchmark's scale headroom, flagged as indirect.
- **Color-space math per scan**: this workload is bounded by scan cadence (human-paced, one scan roughly every few seconds at most, per the product's "heads-down scanning" model), not by throughput. A handful of matrix/floating-point operations per scan is far below the threshold where language-level performance differences between Swift and Rust would be perceptible; no direct benchmark was sought or needed to support this — it follows from the workload shape stated in the brief itself.
- **BLE event handling at hardware pace**: dominated by BLE radio-layer latency (tens of milliseconds per characteristic read/notification) and the vendor SDK's own transport, not by host-language dispatch overhead. `btleplug`'s own documentation frames itself as a convenience/portability layer over each OS's native BLE stack (CoreBluetooth on macOS), not a performance layer (deviceplug/btleplug, n.d.) — reinforcing that BLE throughput here is not host-language-bound.
- **The FFI boundary itself** (relevant to shape A specifically): the one concrete cross-language benchmark found (Godot's Rust bindings) shows FFI-roundtrip overhead of roughly 2–50x a native call *before* call-caching, dropping to roughly 1.1–2.3x after (godot-rust, n.d.) — in absolute terms this is single-digit-microsecond overhead, immaterial next to scan cadence measured in seconds and BLE latency measured in milliseconds.

**Counter-evidence, to be fair to the other side**: Discord's Read States rewrite from Go to Rust is the most-cited real-world case where language-level performance *was* material — but the driver was garbage-collector latency spikes on a service handling continuous, high-QPS traffic, a workload shape with no resemblance to this app's human-paced, single-device, single-session capture loop (Discord, n.d.). Citing Discord as a reason to choose Rust here would be workload-mismatched.

**Verdict, clearly flagged as inference from indirect evidence rather than a direct study**: for this app's stated hot paths, Rust's performance advantage over Swift is very likely noise, not a material factor in the shape decision. If this project's SQLite tables or scan cadence assumptions change materially (e.g., bulk multi-thousand-item batch recompute of derived color values on schema migration), that specific operation — not general "Rust is faster" folklore — would be the one place worth re-measuring.

### 10.3 Documented cases of teams abandoning a Rust-core-plus-native-shell or Rust-UI approach

| Team | What they abandoned | Stated cause | To what |
|---|---|---|---|
| **Portals (Ockam)** | Shape B: Tauri (Rust core + web-based UI) | Tauri gave only "minimal control over how the menu was rendered and what happened when a user interacts with the menu" — UI felt outdated versus native macOS apps | Shape A: SwiftUI shell + Rust core, hand-rolled C FFI (build-trust, n.d.) |
| **Tritium** | An in-progress migration from egui to Slint (still shape B, framework swap) | Hit a macOS document-"open with" bug rooted in `winit`, shared by every major Rust GUI framework; concluded the underlying platform-integration risk wasn't framework-specific | Abandoned the migration at 45% after ~4 weeks; patched `winit` with native Objective-C code instead, kept egui in a hybrid client-server split (Tritium, n.d.) |
| **Dropbox** (mobile, C++ shared core — architecturally identical pattern to shape A, predates Rust) | A shared C++ core for iOS/Android business logic | Cost of making cross-platform code-sharing work exceeded the benefit, which "turned out to be smaller than expected anyway" | Fully native Swift/Kotlin per platform (InfoQ, 2019, reporting Guthmann's account) |
| **Slack** (mobile, `libslack` — same pattern) | A shared C++ library ("Libslack") for mobile business logic | Hiring mobile engineers with C++ experience was hard, which threatened the library's long-term sustainability; overhead judged to outweigh benefit | Reimplemented functionality natively per platform (Slack Engineering, 2019) |

**Important caveat, explicitly flagged**: no case was found of a team abandoning a **Rust**-core-plus-native-shell architecture specifically (as opposed to the architecturally-identical C++-shared-core pattern that Dropbox and Slack abandoned before Rust was a mainstream choice for this role). The Dropbox/Slack evidence is strong on the *architectural pattern* (shared core + native shells, and the coordination/hiring costs that killed it) but is indirect on the *language* — treat it as evidence about shape-A-style architecture generally, not about Rust's FFI mechanics specifically. The Portals and Tritium cases are genuinely Rust-specific but are both small-team, low-visibility projects, not large public postmortems.

### 10.4 Migration cost if shape A were chosen and later reversed to all-Swift (and vice versa) — which choice preserves the most optionality?

No direct case study of "chose Rust-core-plus-Swift-shell for a greenfield app, later reverted the core to native" was found — **this answer is a reasoned inference from the closest analogous evidence (10.3) and from FFI-boundary practice, not a documented reversal case.**

Reasoning, clearly labeled as inference:
- **Shape A → all-Swift reversal cost** is bounded by how cleanly the `SpectroDevice`-style seam and the Rust core's own API are isolated behind a narrow, already-serialized boundary. Dropbox's and Slack's postmortems both frame the *cost of a shared core* as being proportional to how much cross-cutting glue (build tooling, codegen, per-platform quirks) accumulated around the boundary, not proportional to the raw line count of business logic (InfoQ, 2019; Slack Engineering, 2019) — implying that a well-bounded Rust core (color math, derivation pipelines, SQLite access — all pure/testable, non-UI logic per this project's own stated architecture) would be more reversible than a poorly-bounded one. This is an extrapolation, not something either postmortem states about reversal cost directly.
- **All-Swift → shape A reversal cost** (i.e., extracting a Rust core after starting all-Swift) has a real, if indirect, existence proof in 1Password's own history: they didn't start with a Rust core — the "Brain" (browser-fill engine) was ported from Go to Rust starting at the end of 2019, then extended platform-by-platform, with the Windows team (already Rust-comfortable) leading at ~70% Rust before the wider Core effort (Serokell, 2023). This shows *incremental* extraction is a proven path — but 1Password had a paid, multi-year engineering org executing that extraction, which is not this project's situation. Flagged: this precedent's applicability to a solo/small OSS team's capacity to execute the same extraction is unverified.
- **Optionality verdict**: given the above, shape A likely preserves *more* optionality than shape C only if the FFI boundary is kept as narrow and mechanically generated as possible from day one (UniFFI-style, not 1Password's hand-rolled Typeshare-plus-bridge, which itself became a maintenance cost — see 1.1). A wide, hand-maintained boundary erodes exactly the reversibility advantage shape A would otherwise have. This is a synthesized recommendation, not a documented finding — flagged accordingly.

### 10.5 Testable evidence bar an ADR should demand before overturning the all-Swift incumbent

This is requested by the brief as a bar to *state*, not purely a citation lookup, so the following is an explicit synthesis grounded in the sourced findings above rather than a single external citation:

Drawing on Welsh's team-size threshold (2022), Pike's coordination-cost framing (2021), and the workload analysis in 10.2, a defensible bar for this specific project is: **the ADR should not adopt shape A or B unless it can name a concrete, current pain point that all-Swift cannot solve** — not a hypothetical future one. Candidates that would clear that bar, none of which currently exist in evidence gathered for this brief: (a) a measured, not folkloric, performance problem in an actual SwiftUI+GRDB prototype of the color-math or SQLite hot path (10.2 found no such problem, but also no prototype exists yet to measure); (b) a second target platform materializing (which would activate Pike's coordination-cost argument — currently macOS-only per the vision, so inapplicable); (c) a demonstrated contributor-pool advantage specific to this domain (Concern Area 9's territory, not evaluated here). Absent one of these, the evidence gathered under Concern Areas 1 and 10 favors the incumbent (all-Swift) by default, because every clearly-documented regret in the shipped-precedent record (1.3) is about the *UI* layer specifically, and this project's UI requirements (native menu bar, accessibility, wide-gamut color rendering per the vision's gamut-honesty requirement) are exactly the area where Rust-native stacks reported the sharpest pain (Warp, n.d.-a; Tritium, n.d.) and Swift/SwiftUI is a zero-cost default.

### 10.6 Shipped precedent for sequencing strategies (Swift-first-then-extract vs. Rust-core-first-then-wrap-UI)

- **Retrofit-onto-existing-app, incremental extraction** (start with a working native app, port pieces to Rust over time): 1Password is the clearest example — ported the self-contained "Brain" subsystem first (2019), let the platform team most comfortable with Rust (Windows) lead adoption to ~70% before extending the pattern app-wide for 1Password 8 (Serokell, 2023). Cost: multi-year, required a dedicated tooling investment (Typeshare) to keep the boundary from drifting (1.1).
- **Core-first, UI-wrapped-around-it** (build the shared engine before any app exists): Signal's `libsignal` is the cleanest example — the Rust protocol/crypto library was built as the foundation, with Swift (and Kotlin, TypeScript) bindings generated afterward for each client (signalapp, n.d.). This is architecturally closest to what a greenfield shape-A choice for SpectroCapture would look like. However, no cost/timeline figures for that sequencing were published — Signal's engineering blog materials found in this search covered *what* was built, not *how much it cost or how long core-first development took before a UI existed*.
- **Gap, explicitly flagged**: neither precedent is a true greenfield case matching this project's actual situation — "no code exists yet, choose the sequencing before building anything." 1Password and Signal both retrofitted or extended from an existing, already-shipping product. No shipped precedent was found for a **brand-new product** choosing core-first-then-UI (Signal-style) versus UI-scaffold-first-then-extract-core (1Password-style) with no prior codebase at all. See Question Status for a concrete follow-up.

---

## Closing Recommendation

**Recommendation: adopt shape A — a Rust business-logic core under a SwiftUI shell — conditional on a time-boxed boundary spike, with all-Swift as the documented fallback and shape B disqualified.**

Three premises frame this verdict, and it does not survive without them: the project's stated goal is bug-surface reduction through compiler enforcement (not performance); development is agent-driven, with a human supervising AI coding agents rather than a human contributor pool; and SwiftUI is retained for the UX layer. The first two premises come from the project owner and were outside this brief's questions; the research evidence below is weighed against them.

**Shape B (full-Rust UI) is disqualified by this product's own thesis, independent of the language question.** No current Rust UI framework guarantees color-managed Display-P3 rendering: wgpu's macOS wide-gamut pipeline is incomplete, Slint carries a documented sRGB-blending correctness bug across its renderers, and only webview stacks inherit real P3 support — which does not extend to canvas-drawn content, where a swatch grid lives (section 4). An app whose non-negotiable is gamut honesty cannot ship on a stack that may silently render unmanaged sRGB. gpui's VoiceOver support is confirmed broken by its own maintainers, and the retreat precedents (Portals; the retrospectives whose regret lands consistently on Rust-native UI) point the same way (section 1).

**Why shape A wins under the stated premises.** Rust's enforcement is opt-out-proof in exactly the way that matters for agent-written code: ownership makes shared mutable state a compile error, exhaustive `match` and `Result`-typed errors are mandatory, and a plausible-but-wrong change frequently fails to compile rather than failing at runtime. The core also verifies headlessly — `cargo check`/`cargo test`/`cargo nextest` in fast, Linux-runnable, hardware-free CI — which is the tightest feedback loop available to a coding agent, and materially tighter than macOS-runner Xcode builds over SwiftUI-adjacent code, where compiler diagnostics degrade on complex generic code. The findings that argued against shape A for a human team — the two-language contributor tax, onboarding cost, and the mechanical binding-upkeep burden 1Password documented (sections 1, 9) — are labor costs, and they shrink substantially when the labor is agent labor. The integration plumbing exists: UniFFI 0.32 is the dominant bridge, Rust async surfaces as Swift `async/await`, Ferrostar demonstrates the callback-to-`AsyncStream` streaming pattern this app's scan pipeline needs, and the vendor SDK is more bridgeable than the brief assumed (ObjC-compatible API plus a vendor-shipped C wrapper example) (sections 2, 3).

**What shape A costs, regardless of who writes the code — the spike gate.** Three boundary findings are design costs, not labor costs, and the ADR should not be ratified until a time-boxed spike (one to two weeks) retires or prices them: (1) UniFFI's generated bindings currently require `@unchecked Sendable` under Swift 6 strict concurrency — the seam suspends Swift's data-race checking, so the spike must establish the actual audit surface; (2) UniFFI has no built-in cancellation — mid-scan cancellation must be built from the drop-callback hook and proven against the mock device; (3) no Rust SQLite crate offers an equivalent of GRDB's `ValueObservation`, so live collection-view updates need a hand-built change-notification path over the boundary, and the spike must show it at 10k rows (sections 2, 5, 6). The spike doubles as the follow-up action the Question Status already demands for the unverified zero-copy byte-buffer behavior on raw measurement payloads. If the spike fails on any of the three, fall back to all-Swift with the enforcement regime below and record why.

**Boundary discipline for the Swift that remains.** The device layer (wrapping the vendor SDK) and the SwiftUI shell stay Swift; hold them to the equivalent regime: Swift 6 strict concurrency from the first commit, typed throws at module boundaries, exhaustive `switch` with no `default` in state machines, the never-destroy-data invariant enforced in the schema and tested at the transaction level, and property-based tests of all color derivations against reference fixtures (Munsell renotation data) in hardware-free CI (sections 5, 7). Invariants like corrections-never-destroy-data live in schema and tests, not in either language's type system — the Rust core narrows the bug surface; it does not replace that regime.

**What would flip this verdict.** (1) The spike fails or prices the boundary above its worth — fall back to all-Swift, conditions recorded. (2) The development model reverts to primarily human contributors — the two-language tax findings (sections 1, 9) reactivate at full weight, and the all-Swift case should be re-argued. (3) Cross-platform scope (today an explicit non-goal) would only strengthen shape A — it is the one future in which the Rust core pays twice. Write the ADR as "Rust core + SwiftUI shell, spike-gated, fallback stated" — not as an unconditional commitment.

---

## Question Status

### 1. Architecture Shapes & Shipped Precedent

- [x] Which shipped, non-trivial macOS desktop apps use a Rust core beneath a native Swift/AppKit/SwiftUI shell? For each: what lives on which side, and what has the team publicly reported about the boundary's maintenance cost?
- [x] Which shipped macOS apps are full-Rust including the UI, and what UI stack does each actually use — general-purpose or bespoke that doesn't transfer?
- [x] What do engineering retrospectives from 2023–2026 identify as the single biggest win and the single biggest regret of the Rust-core-plus-native-shell architecture?
- [ ] Is there any shipped precedent for a hardware-peripheral-centric macOS app (BLE/USB instrument, one device, event-streaming) built with a Rust core? What did the device layer look like?
  - **Reason not answered**: no commercial/shipped example was found in searches covering BLE/USB instrument control, medical devices, or sensor-dashboard software with a Rust core on macOS. Only hobbyist/demo-scale projects (e.g., a BLE air-quality dashboard) surfaced. This appears to be a genuinely thin part of the public record, not a search failure — the crates (`btleplug`, `bluest`) are mature and documented, but no company has published an engineering account of shipping instrument-control software built on them.
  - **Follow-up**: search vendor-specific engineering blogs and GitHub orgs for companies in adjacent instrument categories (e.g., Nix Sensor's own competitors, lab-automation vendors, other BLE-connected measurement-device software vendors) for "Rust" + their product name; also check crates.io reverse-dependents of `btleplug`/`bluest` for any crate whose repo links to a commercial product, and file/ping the `deviceplug/btleplug` GitHub issues/discussions asking maintainers if they know of production users — BLE library maintainers often know who's using their crate in shipped products even when those products don't blog about it.
- [x] At what codebase size or team size do practitioners report the two-language overhead of shape A paying for itself versus dragging — and is a solo-to-small-team open-source project above or below that line?

### 2. The FFI Boundary (Shape A)

- [x] UniFFI vs. swift-bridge vs. cbindgen/cxx vs. hand-rolled C ABI: current version, maintenance status, backing org, dominant recommendation — §2.1.
- [x] Can a Rust `async fn` surface as Swift `async/await` today, through which tool/version, what executor bridging — §2.2.
- [x] How are continuous event streams best modeled across the boundary — §2.3. Partially inferred (the specific "polling is worse" comparison could not be verified against its primary source; the foreign-trait-to-`AsyncStream` recommendation for this project is an architectural synthesis, not a found "SpectroCapture-shaped" precedent).
- [x] Current type-mapping limits: enums with associated values, `Result`/`throws`, struct graphs, zero-copy byte buffers — §2.4. The zero-copy byte-buffer answer is partially inferred (documented for Kotlin explicitly; Swift behavior not explicitly documented either way).
- [x] Canonical build integration for a Rust static library inside SPM/Xcode, universal binaries, what breaks in incremental builds — §2.5. The "what breaks in incremental builds" half is an explicit, named gap within this answer.
- [x] Debugging story: lldb over mixed frames, crash-report symbolication — §2.6. Answered with an explicit flagged gap: no source demonstrates a single lldb session stepping cleanly across the Swift↔Rust FFI edge; crash-report symbolication is answered as "mechanically compatible with documented edge cases," not as a clean yes.

**All 6 questions in §2 have a substantive answer; three (streaming pattern, zero-copy byte buffers, incremental-build cost) carry an explicit inferred/gap flag inline above rather than being fully unanswered.**
Follow-up for the flagged parts: (1) prototype a UniFFI foreign-trait callback wrapped in `AsyncStream` against a mock `ScanEvent` producer and measure per-event overhead and correctness under rapid back-to-back events; (2) write a minimal UniFFI 0.32.0 crate that returns a `Vec<u8>`/`Data` payload of realistic measurement-blob size (a few KB) from an async fn, call it from a Swift 6 strict-concurrency target, and profile whether a copy occurs (Instruments' Allocations/Time Profiler on the `RustBuffer`-to-`Data` conversion); (3) build a trivial two-crate UniFFI SPM package, touch one Rust file, and time a `swift build` before/after to get a real incremental-cost number, then repeat after touching only Swift-side code to isolate the FFI-regeneration tax.

### 3. Vendor SDK Integration (Fixed Constraint)

- [x] The vendor SDK is a Swift/Objective-C binary framework. What are the viable patterns for a Rust core to drive a Swift-only SDK — and if the device layer must stay in Swift, what does that do to the "Rust as primary language" claim in practice? — Answered in Q1, grounded in Nix Sensor's own documented C-wrapper example project and its Objective-C-compatible header.
- [x] What is the 2026 maturity of calling Swift/ObjC frameworks *from* Rust (objc2 family, swift-bridge's reverse direction): versions, limits, and real usage? — Answered in Q2 with current crates.io versions and GitHub activity. Real shipped usage against a *third-party vendor SDK* (as opposed to Apple's own frameworks or first-party Swift) was **not found** and is flagged as a gap: no example surfaced of any shipped app using `objc2` or `swift-bridge` to drive an unrelated company's binary Objective-C/Swift framework the way this project would need to. **Follow-up:** search GitHub code search and crates.io reverse-dependency listings for `objc2`/`block2` consumers that declare `extern_class!`/`extern_protocol!` against a non-Apple, non-`framework-crates` header, to find or rule out precedent before committing to pattern 2 in the ADR.
- [x] If the device layer stays Swift in shape A: what is the cleanest ownership split so the `SpectroDevice` seam (protocol + mock/live) still lets CI and contributors exercise the Rust core with no hardware and no license key? — Answered in Q3 as a design recommendation (Cargo workspace default-members/feature-gating mirroring the existing SwiftPM module boundary), not something found documented for this exact scenario elsewhere.
- [x] Is direct Rust BLE (btleplug + CoreBluetooth) even relevant given the vendor SDK owns the transport — or does it only matter for a hypothetical future multi-instrument expansion? — Answered in Q4: not relevant now (the SDK's own device-authorization handshake is coupled to its CoreBluetooth session), relevant only for the out-of-scope multi-instrument hypothetical.
- [x] Does a Rust core introduce any new constraints on runtime license-key activation and on keeping the key out of the binary and repo? — Answered in Q5: no new constraint on secret storage/keeping the key out of the repo; the one real change is that the FFI boundary becomes an additional transit point for the secret at runtime, mitigated by the same crate-boundary design as Q3.

**Gap not fully closeable from public sources:** whether the *entire* Nix SDK public surface (not just the classes/protocols/enums documented above) is reachable through the Objective-C-compatible header, or whether some part of it is Swift-only (generics, associated-value enums) and would force a Swift shim regardless of which bridge is chosen for the rest. Nix Sensor's DocC reference documents the discovery/connection/measurement/license surface fully in both Swift and Objective-C, which covers everything the product vision's v1 capture flow needs, but I could not verify there is no Swift-only corner of the API without either the full downloadable SDK (behind Nix's evaluation-license gate) or direct correspondence with `sdk@nixsensor.com`. **Concrete follow-up:** request the SDK download and evaluation license from `sdk@nixsensor.com` (Nix Sensor Ltd., 2026a), inspect the actual `.h`/`.swiftinterface` files inside the XCFramework for any type not represented in the DocC-documented Objective-C examples, and treat pattern 1 (the vendor's own C wrapper) as the fallback if any load-bearing type turns out to be Swift-only.

### 4. Rust-Native UI Options (Shape B)

- [x] For each of egui, Slint, Tauri, Dioxus, gpui, and any credible newcomer: current version, license, macOS-specific maturity, and at least one shipped macOS app using it. — Answered in Q4.1 comparison table. Note: shipped-app precedent for egui, Slint, Xilem, Floem, Iced was weak/indirect (framework-vendor-asserted or inferred, not independently confirmed); only gpui→Zed and Tauri/Dioxus's generic webview-app precedent are solidly sourced.
- [x] Which of them support VoiceOver accessibility, the native menu bar, standard shortcuts, and multi-window on macOS today — at what fidelity? — Answered in Q4.2 table. gpui's VoiceOver support is confirmed broken/absent by its own flagship app's maintainers; Tauri has the most mature native-menu/multi-window story; Slint's and egui's macOS-specific VoiceOver fidelity claims rest on Windows-Narrator evidence extrapolated to macOS, not direct macOS testing.
- [x] Wide-gamut color: which frameworks can render Display-P3 correctly and integrate with ColorSync? For an app whose core promise is color fidelity with explicit gamut-clipped flags, can any non-native stack guarantee it is not silently rendering unmanaged sRGB? — Answered at length in Q4.3. Direct answer: no. Tauri/WKWebView is closest (WebKit's documented CSS Display-P3 support) but doesn't cover canvas-drawn content and has no found ColorSync/ICC API; wgpu-backed stacks (egui, Xilem, Floem) have the technical capability but an incomplete default wide-gamut pipeline on macOS as of this pass; Slint has an open, documented sRGB-blending correctness bug; gpui has no evidence either way.
- [x] Which frameworks have a proven virtualized list/table story at 10,000+ rows with per-cell custom drawing (color swatch grids for collection browsing)? — Answered in Q4.4. egui's `egui_extras::TableBuilder` is the best-documented fit; Slint, gpui, Xilem, Floem lack an equivalently specific primary-sourced example at this scale; Tauri/Dioxus-webview push this to JS-side virtualization libraries.
- [x] What are the text-input, IME, and localization limitations of each on macOS? — Answered in Q4.5. winit's macOS IME bridge (used by egui/Slint/Xilem/Floem) has a real but historically buggy implementation (Pinyin crash, Character Viewer issues); egui specifically has a documented missing-CJK-glyph-coverage defect (Windows-tested, extrapolated to general framework maturity); Tauri/Dioxus-webview inherit WKWebView's mature native IME for free; gpui's IME fidelity is undocumented in this pass despite Zed visibly supporting CJK input in practice.
- [x] What is the honest 2025–2026 community verdict on production-readiness of Rust-native desktop UI on macOS — ship it, or wait? — Answered in Q4.6. Verdict is bifurcated: ship it for webview-shaped apps (Tauri, Dioxus-webview), wait or budget heavy bespoke engineering for a genuinely native, color-managed custom-drawn canvas (egui/Slint/gpui/Xilem/Floem) — and no survey found treats wide-gamut color correctness as an evaluated maturity axis at all, meaning this project's top requirement is untested ground for the whole ecosystem.

**All six Q4 questions are marked answered**, though several sub-claims within them are explicitly flagged inline as indirect, vendor-asserted, or aggregated-rather-than-primary evidence (see inline flags in Q4.1, Q4.2, Q4.3, Q4.5, Q4.6). Two items are close enough to gaps that they're worth calling out even though the parent question was answered:

- **gpui's current AccessKit-integration landing status** — GPUI/Zed maintainers describe AccessKit work as *planned*, but a specific, dated, "this PR landed and shipped in version X" confirmation was not found in this pass. Follow-up: check `zed-industries/zed`'s CHANGELOG.md and the `gpui` crate's own changelog on crates.io for an "AccessKit" entry, and/or file/read a targeted search of closed PRs on `zed-industries/zed` filtered to `gpui` + `accessibility` merged in 2026.
- **A second, more recent 2026 Rust-GUI survey** (blog.wybxc.cc, "A 2026 Survey of Rust GUI Libraries") returned HTTP 403 and could not be read in this pass. Follow-up: retry the fetch via a browser-shaped request (the `fetching-bot-blocked-urls` pattern flagged by this session's own tooling) or via `curl` with realistic browser headers, since it is likely to contain the single most current, comprehensive cross-framework verdict available and was not incorporated here.

### 5. Color Science & Data Layer in Rust

- [x] Which crates cover spectral→XYZ→Lab/LCh/Luv/sRGB/HSL conversion and ΔE2000 (palette, kolor, others): versions, correctness validation, and how they compare against established references (ICC, the Python colour-science library)? — Answered in §5.1. Partial gap noted: no crate documents formal validation against ICC or colour-science directly; flagged as a follow-up need, not a documentation failure on my part. **Follow-up:** export a fixture set of Lab/XYZ/ΔE2000 values from Python `colour-science` (pinned version) offline, and write a `proptest`+golden-file test asserting palette's `Ciede2000`/`Lab`↔`Xyz` output against it within a stated epsilon, before adopting palette as the color-math dependency of record.

- [x] ICC profile and ColorSync interop from Rust — reading display profiles, converting through lcms2 bindings: what exists and how mature is it? — Answered in §5.2 (lcms2 v6.2.0/LCMS 2.19.1, objc2-color-sync v0.3.2).

- [x] rusqlite vs. sqlx vs. diesel for a local, user-owned, portable SQLite file: which best supports careful schema migration, WAL mode, and typed blob handling — and how does the best of them compare feature-for-feature with GRDB, the incumbent Swift recommendation? — Answered in §5.3, with a full comparison table and an explicit statement of the one confirmed feature gap (no `ValueObservation` analog in any Rust crate).

- [x] Are there Rust-specific patterns or pitfalls for the canonical-raw-payload-plus-recomputed-derived-values schema (versioned derivation pipelines over blobs)? — Answered in §5.4, explicitly flagged throughout as inferred/synthesized rather than sourced from a primary document describing this exact pattern, because no such primary source was found. **Follow-up:** if this pattern is adopted, write a short internal design note (not found in the wild) specifying the version-tag-plus-decoder-dispatch convention for any serde/bincode-wrapped blob payloads, before the schema is finalized in an ADR.

- [x] CSV inventory import: which crate handles real-world CSV (encodings, quoting, malformed rows) best, and how does it compare to Swift's options? — Answered in §5.5 (csv crate v1.4.0 vs. SwiftCSV/CodableCSV). One sub-claim flagged as not fully confirmed: whether Swift CSV libraries have an equivalent to `.flexible(true)` malformed-row leniency. **Follow-up:** read the CodableCSV and SwiftCSV source directly (not just their READMEs) for any lenient/flexible row-length configuration before treating this as a settled Rust advantage in the ADR.

### 6. Concurrency & the Hardware-Paced Pipeline

- [x] What replaces `AsyncStream`-bridge in each shape: tokio version, smol, std-only, what desktop apps actually use — §6.1.
- [x] Concrete idioms for one-scan-in-flight against a single-device SDK: actor-style serialization, Rust vs. Swift — §6.2.
- [x] In shape A, who owns the event loops; patterns/pitfalls for tokio-inside-macOS-app-process (thread QoS, App Nap, energy impact) — §6.3. **Partially answered**: event-loop ownership and the Tauri-documented AppKit-thread-violation pitfall are well supported; App Nap / thread QoS / energy-impact specifics for an embedded Tokio runtime are an explicit, named gap (no source measures or documents this combination).
- [x] How do UniFFI/swift-bridge propagate cancellation and timeouts mid-operation — §6.4. UniFFI's answer is documented (no built-in cancellation; a drop-callback hook exists to build it yourself); swift-bridge's cancellation/timeout story specifically was not separately found and is not claimed here — see follow-up.
- [x] Do UniFFI-generated Swift bindings satisfy Swift 6 strict concurrency (Sendable), or is FFI a permanent `@unchecked` escape hatch — §6.5.

**5 of 5 questions in §6 have a substantive, cited answer; two (App Nap/QoS/energy for embedded Tokio, and swift-bridge's specific cancellation/timeout mechanics) are explicitly flagged as incomplete within their answers rather than fully resolved.**
Follow-up for the flagged parts: (1) build a minimal macOS menu-bar-less app that spawns a Tokio multi-thread runtime doing a periodic background task, run it under Activity Monitor / `powermetrics` for an hour foregrounded vs. backgrounded, and compare App Nap state + energy impact against an equivalent Swift-`Task`-only build — this is a measurement gap only a real build can close; (2) since this project's async surface will be built on UniFFI (per §2.1's dominant-recommendation finding) and swift-bridge's async support is already known to be too limited for argument-carrying async calls (§2.2), swift-bridge's cancellation story is likely moot for this project — deprioritize rather than research further unless shape A's tooling choice later reverses away from UniFFI.

### 7. Testing Without Hardware

- [x] In each shape, where should the `SpectroDevice` mock seam live so that CI — which has no hardware and no license key — exercises the maximum amount of logic? — Answered in §7.1. Explicitly flagged as reasoned synthesis from UniFFI/objc2 mechanics plus the project's own fixed constraints, not a confirmed shipped precedent for this exact hardware-peripheral case. **Follow-up:** once a shape is chosen, prototype the actual seam (a `SpectroDevice` trait with a pure-Rust Mock and a UniFFI foreign-trait-backed Swift Live implementation, if Shape A) as a spike and confirm `cargo nextest run` genuinely runs zero-Xcode-dependency on a Linux or non-macOS CI runner, to validate the claimed CI-runner flexibility.

- [x] What is the recommended Rust testing stack for this kind of app (cargo test, cargo-nextest, proptest/quickcheck for color math): versions and conventions? — Answered in §7.2 with a full version table (cargo-nextest ~0.9.14x, proptest 1.11.0, mockall 1.77.0+ MSRV-gated).

- [x] How do teams contract-test an FFI boundary so the generated bindings can't drift from either side? Real project examples. — Answered in §7.3 (UniFFI/Mozilla, libsignal/Signal). One explicit gap flagged: a dedicated "test the generated bindings themselves" story is still an open problem even at Mozilla (uniffi-rs#272 open as of this search), so the "contract test" here is really "generate from one source + checksum guard + a thin real-bindings integration test," not a named, off-the-shelf contract-testing framework. **Follow-up:** track mozilla/uniffi-rs#272 for resolution, and in the interim write the thin XCTest-against-real-generated-bindings integration test described in §7.3 as the project's own contract-test substitute.

- [x] What is the UI-testing story per shape: XCUITest against a SwiftUI shell vs. what, exactly, for egui/Slint/Tauri? — Answered in §7.4 with a full comparison table (egui_kittest, i-slint-backend-testing, Tauri-WebDriver/tauri-plugin-webdriver/CrabNebula).

- [x] Are there reference datasets (Munsell, RIT, CIE fixtures) and crates commonly used to property-test color conversions against ground truth? — Answered in §7.5. One gap explicitly flagged: no dedicated "CIE reference data as a Rust crate" was found, only Munsell-data-derived crates (MunsellSpace) and colour-science-derived data reuse (colorspace-rs). **Follow-up:** if no such crate is judged sufficient, vendor a small, versioned fixture file (JSON or Rust `const` arrays) of Munsell-renotation-derived Lab/XYZ/ΔE2000 ground-truth values directly into the SpectroCapture repo, sourced once from the public Munsell renotation dataset, rather than depending on a third-party crate for correctness-critical test fixtures.

### 8. Build, Signing, Distribution

- [x] What is the canonical pipeline for building, Developer-ID-signing, and notarizing a macOS `.app` whose binary is Rust or mixed Rust+Swift — cargo-bundle, cargo-dist, Tauri's bundler, or Xcode driving everything? What do teams that actually ship use, at which versions?
- [x] What is the exact mechanism for producing a universal (arm64 + x86_64) binary from a mixed cargo/SPM build, and what does it cost in CI time?
  - Partial: the mechanism (`lipo` after per-target `cargo build`) is documented and answered with confidence. The **CI-time cost is not measured anywhere found** — the additive-cost claim in §8.2 is inferred, not sourced from a benchmark. Follow-up: build a minimal Rust static-library + SwiftUI-shell test repo, run a GitHub Actions macos-14 job building single-arch vs. universal (`aarch64-apple-darwin` + `x86_64-apple-darwin` + `lipo`), and record wall-clock minutes for both, cold and with `Swatinem/rust-cache` warm, to get a real number for this project's own dependency footprint rather than relying on a generic estimate.
- [x] Does Sparkle integrate cleanly with a non-Xcode or full-Rust app? What are the alternatives if not?
- [x] Does Rust introduce friction with the App Sandbox and hardened runtime (entitlements, library validation)? The sandbox decision is still open for this project — does a Rust core or Rust UI bias it either way?
- [x] What does GitHub Actions macOS CI look like for a mixed Rust+Swift toolchain versus pure Swift: cache strategy, cold and warm build minutes, known flakes?
  - Partial: cache strategy and known flakes are well documented and answered. **No source gives cold/warm build-minute numbers for a genuinely mixed Rust+Swift pipeline** — every number found is single-language. Follow-up: same test repo as above; add a `Package.swift`/minimal SwiftUI target consuming the Rust XCFramework, run the full pipeline cold (no caches) and warm (`actions/cache` + `Swatinem/rust-cache` + Xcode DerivedData cache primed) three times each, and report median wall-clock minutes — this is the single most useful, cheaply-obtainable missing number in Section 8.

### 9. Contributor Ecosystem & DX

- [x] For an open-source color/hardware tool, what evidence exists on contributor pools — do Rust projects of comparable size attract more or fewer drive-by contributors than Swift projects (Octoverse data, language surveys, contributor counts of comparable apps)?
  - Partial: language-preference/usage-share evidence (SO2025, JetBrains 2025) is solid; Octoverse 2025 does not cover this comparison at all (confirmed absent from the report, not just unsearched); **direct contributor-count comparisons between comparable-scope shipped Rust vs. Swift macOS apps were not found**. Follow-up: pull the actual `contributors` count via `gh api repos/{owner}/{repo}/contributors --paginate` for a matched set of comparable-scope projects — e.g. Zed (Rust) vs. IINA (Swift), GitButler (Rust/Tauri) vs. a comparable Swift menu-bar-scale utility — normalized by repo age and star count, rather than relying on prose claims about "actively courting contributors."
- [x] What are realistic cold and warm build times for an app-sized Rust codebase vs. a comparable Swift one, and how much does an FFI boundary slow the edit-compile-run loop in shape A?
  - Reason not fully answered: no 2024–2026 source was found that benchmarks cold/warm compile times for comparably-sized Rust and Swift codebases side by side (the one performance-comparison source found, dakharlamov, measures *runtime*, not compile time, and should not be conflated). The FFI-boundary edit-compile-run slowdown claim is inferred from the two-step (Rust build → binding regeneration → Xcode relink) mechanics documented in UniFFI/`cargo-swift` tooling, not measured. Follow-up: once the test repo from the §8 follow-ups exists, time a controlled edit-compile-run loop three ways — (a) a Rust-only change with no interface change, (b) a Swift-only UI change, (c) a change to a `#[uniffi::export]`ed function signature requiring binding regeneration — and report wall-clock seconds for each, which directly answers the FFI-boundary-slowdown question with this project's actual toolchain rather than an inference.
- [x] What is day-to-day IDE reality in a mixed repo — rust-analyzer plus Xcode side by side vs. pure Xcode: what breaks (jump-to-definition across the boundary, refactoring, debugging)?
  - Reason not fully answered: rust-analyzer's non-support of Xcode and the syntax-highlighting-only nature of `rust-xcode-plugin` are documented, but **no first-person team report** ("we tried X, jump-to-definition broke at Y") was located — the "what breaks across the boundary" answer in this document is inferred from the absence of any cross-boundary-navigation tool, not from a team's lived account. Follow-up: search GitHub issues/discussions on `mozilla/uniffi-rs`, `chinedufn/swift-bridge`, and `antoniusnaumann/cargo-swift` specifically for "jump to definition," "xcode," or "IDE" — these projects' own issue trackers are the most likely place a contributor has already filed exactly this complaint, and this pass did not have time to exhaustively mine them.
- [x] What do teams report about onboarding contributors onto a two-language codebase — does shape A effectively require every core contributor to know both?
  - Partial: the WordPress-rs precedent is a strong, directly-relevant negative case, and architecture-documentation framing from three real Rust-core-plus-native-bindings projects (Bitwarden, Mozilla application-services, Ferrostar) supports an inferred "no for casual contribution, yes for interface changes" answer, but **no source is a first-person "here's what happened when we onboarded a new contributor" retrospective**. Follow-up: read Bitwarden's public `CONTRIBUTING.md`/`sdk-internal` issue tracker (github.com/bitwarden/sdk-internal) for "good first issue" labeling patterns — specifically, whether good-first-issues are scoped to stay on one side of the FFI boundary — as a concrete, checkable proxy for whether the team itself designed for single-language contribution.
- [x] What is the honest evidence (2024–2026) that Rust reduces defect rates for this class of app — desktop, IO-bound, GC-irrelevant, not memory-pressured — as opposed to systems software where the safety argument originated?

**All ten questions across Sections 8–9 received a substantive answer; five are marked partial because a sub-part of the question (almost always a quantitative CI-time, build-time, or contributor-count figure) could not be sourced and required a stated inference instead of a measurement. None were left completely unanswered.**

### 10. Decision Framework & Risks

- [x] What decision criteria have engineering organizations published for native-vs-Rust-core choices, and which of those criteria actually apply at this project's scale?
- [x] For this app's real hot paths — SQLite reads over ~10k rows, color-space math per scan, BLE event handling at hardware pace — is Rust's performance advantage material or noise? Cite measurements, not folklore.
- [x] What documented cases exist of teams abandoning a Rust-core-plus-native-shell or Rust-UI approach, and what was the stated cause?
- [x] Given no code exists yet: if shape A were chosen and later reversed, what is the migration cost back to all-Swift — and vice versa? Which choice preserves the most optionality?
- [x] All-Swift is the incumbent recommendation of the prior architecture research. State, as a testable bar, what evidence threshold an ADR should demand before overturning it.
- [ ] Is there shipped precedent for sequencing strategies — start all-Swift and extract a Rust core later, or build the Rust core first and wrap UI iterations around it — and what did each cost the teams that tried?
  - **Reason not answered (partially)**: real precedent exists for *both* sequencing directions (1Password = extract-later; Signal = core-first), but in both cases the sequencing decision was made by a team retrofitting or extending an **already-shipping product**, not a brand-new greenfield one with zero existing code — which is this project's actual situation. No cost/timeline figures were published for either team's sequencing choice in a form comparable to "X months/engineers for core-first vs. Y for retrofit."
  - **Follow-up**: reach out directly (GitHub discussions, or public podcasts/talks — e.g., the `corrode.dev` Rust-in-Production podcast series, which already interviewed 1Password) to ask a maintainer of a small, greenfield, shipped Rust-core-plus-native-shell OSS project (search GitHub topic `uniffi` + `swift` + macOS, sorted by stars, for candidates with < 5 contributors) what their actual sequencing was and what it cost in wall-clock time before a usable UI existed — this is the closest obtainable proxy for SpectroCapture's actual situation.

---

## Unanswered Questions — Summary

1. **Hardware-peripheral-centric macOS app with a Rust core (device-layer precedent).** Reason: no shipped/commercial example found, only hobbyist demos. Follow-up: query `btleplug`/`bluest` maintainers and reverse-dependents for known production users; search adjacent instrument-vendor engineering blogs by product category rather than by "Rust + BLE" generically.
2. **Shipped precedent for sequencing strategies on a greenfield (no prior code) project specifically.** Reason: found precedent (1Password, Signal) is all retrofit/extension of existing shipping products, not zero-to-one greenfield sequencing, so cost figures don't transfer cleanly. Follow-up: directly interview a maintainer of a small, greenfield, already-shipped Rust-core + Swift-shell OSS project (found via GitHub topic search) about their actual build order and time-to-first-usable-UI.

---

## References

1Password. (n.d.-a). *1Password 8: The story so far* [Fey, M., author]. 1Password Blog. https://1password.com/blog/1password-8-the-story-so-far

Aarambh Dev Hub. (2026, June). *Rust ORMs in 2026: Diesel vs SQLx vs SeaORM vs Rusqlite — which one should you actually use?* Medium. https://aarambhdevhub.medium.com/rust-orms-in-2026-diesel-vs-sqlx-vs-seaorm-vs-rusqlite-which-one-should-you-actually-use-706d0fe912f3

ahonn. (n.d.). *tauri-plugin-sparkle-updater* [Computer software documentation]. Docs.rs. Retrieved 2026-09-01, from https://docs.rs/tauri-plugin-sparkle-updater

anderslanglands. (2026). *colorspace-rs: A color library for Rust* [GitHub repository]. GitHub. https://github.com/anderslanglands/colorspace-rs

antoniusnaumann. (n.d.). *cargo-swift: A cargo plugin to easily build Swift packages from Rust code* [GitHub repository]. Retrieved 2026-09-01, from https://github.com/antoniusnaumann/cargo-swift

Apple Developer Forums. (n.d.). *How do MTKView/CAMetalLayer and extended colorspaces work?* [Forum thread #724223]. Retrieved September 2026, from https://developer.apple.com/forums/thread/724223

Apple Developer Forums. (n.d.). *Rendering for Display P3 displays* [Forum thread #111818]. Retrieved September 2026, from https://developer.apple.com/forums/thread/111818

Apple Developer Forums. (n.d.). *VoiceOver Accessibility Tree out of sync with WKWebView contents* [Forum thread #809541]. Retrieved September 2026, from https://developer.apple.com/forums/thread/809541

Apple Inc. (n.d.). *Building a universal macOS binary*. Apple Developer Documentation. Retrieved 2026-09-01, from https://developer.apple.com/documentation/apple-silicon/building-a-universal-macos-binary.md

Apple. (n.d.-a). *Energy efficiency guide for Mac apps: Extend App Nap*. Apple Developer Documentation Archive. https://developer.apple.com/library/archive/documentation/Performance/Conceptual/power_efficiency_guidelines_osx/AppNap.html

Apple. (n.d.-b). *Energy efficiency guide for Mac apps: Prioritize work at the task level*. Apple Developer Documentation Archive. https://developer.apple.com/library/archive/documentation/Performance/Conceptual/power_efficiency_guidelines_osx/PrioritizeWorkAtTheTaskLevel.html

Automattic. (2024/2025). *Pivot to Rust being a helper library* (Pull Request #8) [GitHub]. wordpress-rs. Retrieved 2026-09-01, from https://github.com/Automattic/wordpress-rs/pull/8

automerge. (n.d.). *automerge-swift* [GitHub repository]. GitHub. https://github.com/automerge/automerge-swift

axodotdev. (2023). *macOS: Create universal binaries (fat archives)* (Issue #77) [GitHub]. cargo-dist. Retrieved 2026-09-01, from https://github.com/axodotdev/cargo-dist/issues/77

axodotdev. (2026a). *cargo-dist* [Software, v0.32.0]. GitHub. https://github.com/axodotdev/cargo-dist

axodotdev. (2026b). *signing: apple codesign* (Issue #1121) [GitHub]. cargo-dist. Retrieved 2026-09-01, from https://github.com/axodotdev/cargo-dist/issues/1121

Barrows, B. (n.d.). *Debugging and working on Rust in Xcode — Debug with LLDB using the Xcode interface* [Blog post]. https://bbarrows.com/posts/rust-xcode

Bitwarden. (n.d.). *SDK Architecture*. Bitwarden Contributing Documentation. Retrieved 2026-09-01, from https://contributing.bitwarden.com/architecture/sdk/

BleuIO. (n.d.). *Developing a desktop BLE air-quality application with Rust, Dioxus, and BleuIO*. https://www.bleuio.com/blog/developing-a-desktop-ble-air-quality-application-with-rust-dioxus-and-bleuio/

boringcactus. (2025, April 13). *A 2025 survey of Rust GUI libraries*. https://www.boringcactus.com/2025/04/13/2025-survey-of-rust-gui-libraries.html

build-trust. (n.d.). *How we built a Swift app that uses Rust*. DEV Community. https://dev.to/build-trust/how-we-built-a-swift-app-that-uses-rust-102f

BuildPulse. (n.d.). *GitHub Actions runner cache benchmarks*. Retrieved 2026-09-01, from https://buildpulse.io/blog/github-actions-cache-optimization-benchmarks

BurntSushi. (2026). *quickcheck: Automated property based testing for Rust (with shrinking)* [GitHub repository]. GitHub. https://github.com/BurntSushi/quickcheck

burtonageo. (n.d.). *cargo-bundle: Wrap rust executables in OS-specific app bundles* [GitHub repository, v0.10.0]. Retrieved 2026-09-01, from https://github.com/burtonageo/cargo-bundle

chinedufn. (n.d.). *Introduction — The swift-bridge book*. Retrieved September 1, 2026, from https://chinedufn.github.io/swift-bridge/

chinedufn/swift-bridge. (2026). *swift-bridge* [GitHub repository]. GitHub. https://github.com/chinedufn/swift-bridge (repository metadata and commit history retrieved September 1, 2026; last push 2026-01-06, version 0.1.59)

chinedufn/swift-bridge. (n.d.). *Running Swift From Rust* (Issue #103) [GitHub issue]. GitHub. https://github.com/chinedufn/swift-bridge/issues/103

ChrisGVE. (2026). *MunsellSpace: Rust crate and Python package to work on the Munsell Color Space* [GitHub repository]. GitHub. https://github.com/ChrisGVE/MunsellSpace

cicdpipelinecost.com. (2026). *GitHub Actions pricing 2026: $0.006/min, ARM, macOS 10x*. Retrieved 2026-09-01, from https://cicdpipelinecost.com/github-actions-pricing

cmsj.net. (2021). *Lessons learned about using GitHub Actions to build macOS apps*. Retrieved 2026-09-01, from https://cmsj.net/2021/04/12/macgithubactions.html

CodeJam. (2025). *GitHub Action hanging on Electron macOS app code signing*. Retrieved 2026-09-01, from https://www.codejam.info/2025/06/github-action-hanging-macos-app-code-signing.html

crates.io. (2026a). *block2* [Package registry entry]. https://crates.io/crates/block2 (version 0.6.2, published 2025-10-04; retrieved via crates.io API September 1, 2026)

crates.io. (2026b). *btleplug* [Package registry entry]. https://crates.io/crates/btleplug (version 0.13.0, published 2026-08-31; retrieved via crates.io API September 1, 2026)

crates.io. (2026c). *csv* [Package page]. docs.rs. https://docs.rs/crate/csv/latest

crates.io. (2026d). *deltae* [Package page]. Lib.rs. https://lib.rs/crates/deltae

crates.io. (2026e). *empfindung* [Package page]. Lib.rs. https://lib.rs/crates/empfindung

crates.io. (2026f). *mockall* [Package page]. docs.rs. https://docs.rs/mockall/latest/mockall/

crates.io. (2026g). *objc2* [Package registry entry]. https://crates.io/crates/objc2 (version 0.6.4, published 2026-02-26; retrieved via crates.io API September 1, 2026)

crates.io. (2026h). *objc2-foundation* [Package registry entry]. https://crates.io/crates/objc2-foundation (version 0.3.2, published 2025-10-04; retrieved via crates.io API September 1, 2026)

crates.io. (2026i). *palette* [Package page]. Lib.rs. https://lib.rs/crates/palette

crates.io. (2026j). *proptest* [Package page]. docs.rs. https://docs.rs/crate/proptest/latest

crates.io. (2026k). *uniffi_testing* [Package page]. crates.io. https://crates.io/crates/uniffi_testing

crates.io. (n.d.). *gpui — versions*. Rust Package Registry. Retrieved September 2026, from https://crates.io/crates/gpui/versions

dakharlamov. (n.d.). *Swift 6 versus Rust: clarity and performance compared* [Substack]. Retrieved 2026-09-01, from https://dakharlamov.substack.com/p/swift-6-versus-rust-clarity-and-performance

dehesa. (2026). *CodableCSV: Read and write CSV files row-by-row or through Swift's Codable interface* [GitHub repository]. GitHub. https://github.com/dehesa/CodableCSV

Depot. (n.d.). *Fast Rust builds with sccache and GitHub Actions*. Retrieved 2026-09-01, from https://depot.dev/blog/sccache-in-github-actions

deviceplug/btleplug. (n.d.). *btleplug: Rust Cross-Platform Host-Side Bluetooth LE Access Library* [GitHub repository]. GitHub. https://github.com/deviceplug/btleplug

DioxusLabs. (2025, March). *Dioxus Desktop & Mobile Native APIs* [GitHub Issue #3855]. https://github.com/DioxusLabs/dioxus/issues/3855

DioxusLabs. (n.d.-a). *dioxus/LICENSE-MIT* [GitHub repository]. https://github.com/DioxusLabs/dioxus/blob/main/LICENSE-MIT

DioxusLabs. (n.d.-b). *blitz: A radically modular HTML/CSS rendering engine* [GitHub repository]. https://github.com/DioxusLabs/blitz

DioxusLabs. (n.d.-c). *dioxus_desktop: Segfault when bundled on M1/macOS 14, seemingly due to MenuBar* [GitHub Issue #1918]. https://github.com/DioxusLabs/dioxus/issues/1918

DioxusLabs. (n.d.-d). *Desktop wry child window support* [GitHub Issue #3086]. https://github.com/DioxusLabs/dioxus/issues/3086

Discord. (n.d.). *Why Discord is switching from Go to Rust*. Discord Blog. https://discord.com/blog/why-discord-is-switching-from-go-to-rust

docs.rs. (2026a). *color_difference — palette* [Module documentation]. docs.rs. https://docs.rs/palette/latest/palette/color_difference/index.html

docs.rs. (2026b). *csv* [Crate documentation]. docs.rs. https://docs.rs/csv/latest/csv/

docs.rs. (2026c). *objc2_color_sync* [Crate documentation]. docs.rs. https://docs.rs/objc2-color-sync/latest/objc2_color_sync/

docs.rs. (n.d.-a). *dioxus 0.7.10*. https://docs.rs/crate/dioxus/latest

docs.rs. (n.d.-b). *eframe 0.36.1 — Cargo.toml*. https://docs.rs/crate/eframe/latest/source/Cargo.toml

docs.rs. (n.d.-c). *egui 0.36.1*. https://docs.rs/crate/egui/latest

docs.rs. (n.d.-d). *slint 1.17.1*. https://docs.rs/slint/latest/index.html

docs.rs. (n.d.-e). *TableBody in egui_extras*. https://docs.rs/egui_extras/latest/egui_extras/struct.TableBody.html

docs.rs. (n.d.-f). *tauri 2.11.2 — LICENSE_APACHE-2.0*. https://docs.rs/crate/tauri/latest/source/LICENSE_APACHE-2.0

docs.rs. (n.d.-g). *tauri 2.11.5*. https://docs.rs/crate/tauri/latest

docs.rs. (n.d.-h). *uniffi 0.31.0* [Crate documentation]. https://docs.rs/crate/uniffi/latest

docs.rs/libc. (n.d.). *pthread_attr_set_qos_class_np in libc*. https://docs.rs/libc/latest/x86_64-apple-darwin/libc/fn.pthread_attr_set_qos_class_np.html

du, t. (2026a). *Ship your Tauri v2 app like a pro: Code signing for macOS and Windows (Part 1/2)*. DEV Community. Retrieved 2026-09-01, from https://dev.to/tomtomdu73/ship-your-tauri-v2-app-like-a-pro-code-signing-for-macos-and-windows-part-12-3o9n

du, t. (2026b). *Ship your Tauri v2 app like a pro: GitHub Actions and release automation (Part 2/2)*. DEV Community. Retrieved 2026-09-01, from https://dev.to/tomtomdu73/ship-your-tauri-v2-app-like-a-pro-github-actions-and-release-automation-part-22-2ef7

Eclectic Light Company. (2021). *Notarization: The hardened runtime*. Retrieved 2026-09-01, from https://eclecticlight.co/2021/01/07/notarization-the-hardened-runtime/

egui contributors (emilk). (2026). *egui: Release 0.30.0 — egui_kittest and modals* [GitHub release]. GitHub. https://github.com/emilk/egui/releases/tag/0.30.0

emilk. (n.d.-a). *egui/LICENSE-MIT* [GitHub repository]. https://github.com/emilk/egui/blob/main/LICENSE-MIT

emilk. (n.d.-b). *Implement accessibility APIs via AccessKit* [GitHub Pull Request #2294]. https://github.com/emilk/egui/pull/2294

emilk. (n.d.-c). *eFrame: Native system menubar* [GitHub Issue #3411]. https://github.com/emilk/egui/issues/3411

emilk. (n.d.-d). *Native System Menu Bar Integration?* [GitHub Discussion #3293]. https://github.com/emilk/egui/discussions/3293

emilk. (n.d.-e). *Multiple native windows* [GitHub Issue #1044]. https://github.com/emilk/egui/issues/1044

GitButler. (2026). *DEVELOPMENT.md* [GitHub repository documentation]. gitbutlerapp/gitbutler. Retrieved 2026-09-01, from https://github.com/gitbutlerapp/gitbutler/blob/master/DEVELOPMENT.md

GitHub Marketplace. (n.d.). *Rust Cache* [GitHub Action]. Retrieved 2026-09-01, from https://github.com/marketplace/actions/rust-cache

GitHub. (2025). *Octoverse: A new developer joins GitHub every second as AI leads TypeScript to #1*. The GitHub Blog. Retrieved 2026-09-01, from https://github.blog/news-insights/octoverse/octoverse-a-new-developer-joins-github-every-second-as-ai-leads-typescript-to-1/

GitSpider. (2026). *Your GitHub Actions bill is mostly macOS minutes (here's how to tell)*. Retrieved 2026-09-01, from https://gitspider.com/guides/github-actions-macos-runner-cost

godot-rust. (n.d.). *FFI optimizations and benchmarking*. https://godot-rust.github.io/dev/ffi-optimizations-benchmarking/

Google. (2024/2025). *Rust in Android: Move fast and fix things*. Google Security Blog. Retrieved 2026-09-01, from https://blog.google/security/rust-in-android-move-fast-fix-things/

gpui.rs. (n.d.). *gpui* [Project homepage]. https://www.gpui.rs/

GRDB.swift. (n.d.). *Performance* [Wiki page]. GitHub. https://github.com/groue/GRDB.swift/wiki/Performance

groue. (2026). *GRDB.swift: A toolkit for SQLite databases, with a focus on application development* [GitHub repository]. GitHub. https://github.com/groue/GRDB.swift

Hacker News. (2023). *Egui commit: Implement accessibility APIs via AccessKit* [Discussion thread]. https://news.ycombinator.com/item?id=33859697

Hacker News. (2026). *Iced 0.14 has been released (Rust GUI library)* [Discussion thread]. https://news.ycombinator.com/item?id=46185323

Hiyoyok. (2026). *Code signing a Tauri app for macOS — The complete flow*. DEV Community. Retrieved 2026-09-01, from https://dev.to/hiyoyok/code-signing-a-tauri-app-for-macos-the-complete-flow-54jk

Infinyon. (2021). *GitHub Actions best practices for Rust projects*. Retrieved 2026-09-01, from https://www.infinyon.com/blog/2021/04/github-actions-best-practices/

InfoQ. (2019, November). *Hidden costs of iOS/Android shared development, at Dropbox and Slack*. https://www.infoq.com/news/2019/11/mobile-share-code-costs/

Israel, A. (n.d.). *Rust universal binaries*. Retrieved 2026-09-01, from https://www.adamisrael.com/blog/rust-universal-binaries/

itsfoss.com. (2025/2026). *GitHub's 2025 report reveals some surprising developer trends*. Retrieved 2026-09-01, from https://itsfoss.com/news/github-octoverse-2025/

JetBrains. (2026a, May 1). *Faster Rust testing at scale: cargo-nextest in practice*. JetBrains Blog. https://blog.jetbrains.com/rust/2026/05/01/faster-rust-tests-with-cargo-nextest/

JetBrains. (2026b). *Rewriting in Rust: Performance, failures, 2026 reality check*. The RustRover Blog. Retrieved 2026-09-01, from https://blog.jetbrains.com/rust/2026/08/10/rewriting-in-rust/

JetBrains. (2026c, April 3). *RustRover 2026.1: Professional testing with native cargo-nextest integration*. JetBrains Blog. https://blog.jetbrains.com/rust/2026/04/03/rustrover-2026-1-professional-testing-with-native-cargo-nextest-integration/

JetBrains. (2026d). *The state of Rust ecosystem 2025*. The RustRover Blog. Retrieved 2026-09-01, from https://blog.jetbrains.com/rust/2026/02/11/state-of-rust-2025/

Jonikorjk. (2026, July). *The art of bridging Rust to Swift*. Medium. https://medium.com/@jonikorjk/the-art-of-bridging-rust-to-swift-e7298045d9a4

Kalbertodt, L. (2023, February 3). *Tauri vs Iced vs egui: Rust GUI framework performance comparison*. Lukas' Blog. http://lukaskalbertodt.github.io/2023/02/03/tauri-iced-egui-performance-comparison.html

Kanoldt, S. (n.d.). *Signing Rust binaries shouldn't require shell scripts*. Retrieved 2026-09-01, from https://d34dl0ck.me/cargo-codesign/index.html

kornelski. (2026). *rust-lcms2: ICC color profiles in Rust* [GitHub repository]. GitHub. https://github.com/kornelski/rust-lcms2

Kruckenberg, J. (n.d.). *macOS bundle* [Tauri documentation, work in progress]. Retrieved 2026-09-01, from https://jonaskruckenberg.github.io/tauri-docs-wip/building/macos-bundle.html

lap.dev. (n.d.). *Floem — Cross-platform GUI framework for Rust*. https://lap.dev/floem/

Lib.rs. (n.d.-a). *uniffi* [Crate listing]. https://lib.rs/crates/uniffi

Lib.rs. (n.d.-b). *swift-bridge* [Crate listing]. https://lib.rs/crates/swift-bridge

Lib.rs. (n.d.-c). *cbindgen* [Crate listing]. https://lib.rs/crates/cbindgen

Lib.rs. (n.d.-d). *cxx* [Crate listing]. https://lib.rs/crates/cxx

Lib.rs. (n.d.-e). *tokio* [Crate listing]. https://lib.rs/crates/tokio

linebender. (n.d.). *xilem: An experimental Rust native UI framework* [GitHub repository]. https://github.com/linebender/xilem

LogRocket. (2026a). *Mocking in Rust: Mockall and alternatives*. LogRocket Blog. https://blog.logrocket.com/mocking-rust-mockall-alternatives/

LogRocket. (2026b). *Property-based testing in Rust with Proptest*. LogRocket Blog. https://blog.logrocket.com/property-based-testing-in-rust-with-proptest/

LogRocket. (2026c). *The state of Rust GUI libraries*. LogRocket Blog. https://blog.logrocket.com/state-rust-gui-libraries/

longbridge. (n.d.). *gpui-component: Rust GUI components for building fantastic cross-platform desktop application by using GPUI* [GitHub repository]. https://github.com/longbridge/gpui-component

madsmtm. (2026). *objc2: Bindings to Apple frameworks in Rust* [GitHub repository]. GitHub. https://github.com/madsmtm/objc2 (retrieved September 1, 2026; last push 2026-08-27, 1,023 stars); see also documentation at https://docs.rs/objc2 and https://docs.rs/objc2/latest/objc2/topics/swift/index.html

Matrix.org. (n.d.). *matrix-rust-components-swift: Swift package providing components from the matrix-rust-sdk* [Software repository]. GitHub. https://github.com/matrix-org/matrix-rust-components-swift

Medium/macoclock. (n.d.). *Your Mac app doesn't need the App Store to be trusted — It needs entitlements done right*. Retrieved 2026-09-01, from https://medium.com/macoclock/your-mac-app-doesnt-need-the-app-store-to-be-trusted-it-needs-entitlements-done-right-2888eb7a979b

Mozilla. (2026c). *Issue #272: Figure out how to test generated UniFFI Desktop bindings* [GitHub issue]. GitHub. https://github.com/mozilla/uniffi-rs/issues/272

Mozilla. (n.d.-a). *cbindgen: A project for generating C bindings from Rust code* [Software repository]. GitHub. https://github.com/mozilla/cbindgen

Mozilla. (n.d.-b). *CHANGELOG.md*. uniffi-rs. GitHub. https://github.com/mozilla/uniffi-rs/blob/main/CHANGELOG.md

Mozilla. (n.d.-c). *Discuss: general case for Async calls into Rust* (Issue #1054). uniffi-rs. GitHub. https://github.com/mozilla/uniffi-rs/issues/1054

Mozilla. (n.d.-d). *Enumerations*. The UniFFI user guide (0.27 / latest). https://mozilla.github.io/uniffi-rs/0.27/udl/enumerations.html

Mozilla. (n.d.-e). *Foreign traits*. The UniFFI user guide. https://mozilla.github.io/uniffi-rs/latest/foreign_traits.html

Mozilla. (n.d.-f). *Integrating with Xcode*. The UniFFI user guide. https://mozilla.github.io/uniffi-rs/latest/swift/xcode.html

Mozilla. (n.d.-g). *rust-components-swift* [GitHub repository]. GitHub. https://github.com/mozilla/rust-components-swift

Mozilla. (n.d.-h). *Shipping Rust components as Swift packages*. Cross-platform Rust Components book. https://mozilla.github.io/application-services/book/design/swift-package-manager.html

Mozilla. (n.d.-i). *Support Cancelling/Dropping Rust Futures* (Issue #2771). uniffi-rs. GitHub. https://github.com/mozilla/uniffi-rs/issues/2771

Mozilla. (n.d.-j). *Support rich structured Enums for reporting errors* (Issue #460). uniffi-rs. GitHub. https://github.com/mozilla/uniffi-rs/issues/460

Mozilla. (n.d.-k). *Support XCode 26/Swift 6.2 Concurrency Configuration* (Issue #2818). uniffi-rs. GitHub. https://github.com/mozilla/uniffi-rs/issues/2818

Mozilla. (n.d.-l). *[Swift 6] Async functions results in compile errors about data races* (Issue #2274). uniffi-rs. GitHub. https://github.com/mozilla/uniffi-rs/issues/2274

Mozilla. (n.d.-m). *Swift bindings*. The UniFFI user guide. https://mozilla.github.io/uniffi-rs/latest/swift/overview.html

Mozilla. (n.d.-n). *UniFFI Async FFI details*. The UniFFI user guide. https://mozilla.github.io/uniffi-rs/latest/internals/async-ffi.html

Mozilla. (n.d.-o). *UniFFI Async overview*. The UniFFI user guide. https://mozilla.github.io/uniffi-rs/latest/internals/async-overview.html

Mozilla. (n.d.-p). *uniffi-rs: a multi-language bindings generator for rust* [Software repository]. GitHub. https://github.com/mozilla/uniffi-rs

MSicc. (n.d.). *CI-ready macOS signing: Combining Apple Distribution & Installer certificates for GitHub Actions*. MSicc's Blog. Retrieved 2026-09-01, from https://msicc.net/ci-ready-macos-signing-combining-certs-for-github-actions/

Nix Sensor Ltd. (2026a). *Spectrophotometer SDK for seamless software development* [Nix Universal SDK product page]. https://www.nixsensor.com/nix-software-development-kit/

Nix Sensor Ltd. (2026b). *Nix Universal SDK* [Windows SDK documentation]. https://nixsensor.github.io/nix-universal-sdk-windows-doc/

Nix Sensor Ltd. (2026c). *NixUniversalSDK documentation* [DocC reference archive: overview, "Activating License," and "Discovering & Connecting" pages]. https://github.com/nixsensor/nix-universal-sdk-ios-dist (documentation archive under `documentation/NixUniversalSDK.doccarchive`; hosted rendering at https://nixsensor.github.io/nix-universal-sdk-ios-dist/documentation/nixuniversalsdk/)

Nix Sensor Ltd. (2026d). *NixUniversalSDK documentation: C/C++ wrapper example* [DocC reference page, "wrapper"]. https://github.com/nixsensor/nix-universal-sdk-ios-dist (path: `documentation/NixUniversalSDK.doccarchive/data/documentation/nixuniversalsdk/wrapper.json`)

Nix Sensor Ltd. (2026e). *NixUniversalSDK documentation: Add to Xcode* [DocC reference page, "add-to-xcode"]. https://github.com/nixsensor/nix-universal-sdk-ios-dist (path: `documentation/NixUniversalSDK.doccarchive/data/documentation/nixuniversalsdk/add-to-xcode.json`)

Nix Sensor Ltd. (2026f). *nix-universal-sdk-ios-dist* [GitHub repository: `Package.swift`, `NixUniversalSDK.podspec`, `README.md`]. https://github.com/nixsensor/nix-universal-sdk-ios-dist (podspec version 4.2.3; retrieved September 1, 2026)

Nix Sensor Ltd. (2026g). *nixsensor* [GitHub organization page, including the `SwiftyBluetooth` fork]. https://github.com/nixsensor

No-Wham Dev. (n.d.). *GitHub Actions caching for iOS CI: Strategies that work*. Retrieved 2026-09-01, from https://nowham.dev/posts/github-actions-ios-caching/

Pi Stack. (2026a, June 23). *Self-hosted Rust database libraries: Diesel vs SeaORM vs rusqlite*. https://www.pistack.xyz/posts/2026-06-23-rust-database-libraries-diesel-seaorm-rusqlite/

Pi Stack. (2026b, August 25). *Rust testing in 2026: cargo test vs cargo-nextest vs rstest — which should you use?* https://www.pistack.xyz/posts/2026-08-25-rust-testing-cargo-test-nextest-rstest-comparison/

Pike, A. (2021). *The persistent gravity of cross platform apps*. https://allenpike.com/2021/gravity-of-cross-platform-apps/

QASkills. (2026a). *Rust mockall guide 2026: Mocking traits & structs for unit tests*. https://qaskills.sh/blog/rust-mockall-mocking-guide-2026

QASkills. (2026b). *XCUITest iOS UI testing tutorial (2026)*. https://qaskills.sh/blog/xcuitest-ios-ui-testing-tutorial-2026

Raffel, D. (2026, February 13). *I built a WebDriver for WKWebView Tauri apps on macOS*. Daniel's Journal. https://danielraffel.me/2026/02/14/i-built-a-webdriver-for-wkwebview-tauri-apps-on-macos/

Rerun. (n.d.). *Why Rust?* Rerun Blog. https://rerun.io/blog/why-rust

Rhonabwy. (2023, October 21). *Automerge for Swift*. https://rhonabwy.com/2023/10/21/automerge-for-swift/

Rio Terminal. (n.d.). *Wide Color Gamut Support*. https://rioterm.com/docs/features/wide-color-gamut

RIT College of Science. (2026). *Munsell Color Science Lab educational resources*. https://www.rit.edu/science/munsell-color-science-lab-educational-resources

romixlab. (n.d.). *egui_tabular: Table viewer and editor for egui* [GitHub repository]. https://github.com/romixlab/egui_tabular

Rust Cookbook contributors. (2026). *CSV processing*. https://rust-lang-nursery.github.io/rust-cookbook/encoding/csv.html

Rust Users Forum. (n.d.). *How do I get rust working in Xcode* [Forum thread]. Retrieved 2026-09-01, from https://users.rust-lang.org/t/how-do-i-get-rust-working-in-xcode/116911/8

rust-analyzer.github.io. (n.d.). *rust-analyzer*. Retrieved 2026-09-01, from https://rust-analyzer.github.io/

rust-lang. (n.d.). *Support for macOS universal/fat binaries* (Issue #8875) [GitHub]. cargo. Retrieved 2026-09-01, from https://github.com/rust-lang/cargo/issues/8875

rust-windowing. (n.d.). *Add new `Ime` event for desktop platforms* [GitHub commit, winit]. https://github.com/rust-windowing/winit/commit/f04fa5d54f4ec10cdb6d084deeb79d3e6d27ae67

rust-windowing. (n.d.). *Composing text display when input Chinese* [GitHub Issue #3893, winit]. https://github.com/rust-windowing/winit/issues/3893

rust-windowing. (n.d.). *IME and the MacOS Character Viewer* [GitHub Issue #3342, winit]. https://github.com/rust-windowing/winit/issues/3342

RustFAQ. (n.d.). *How to build universal macOS binaries with Rust*. Retrieved 2026-09-01, from https://www.rustfaq.org/en/how-to-build-universal-macos-binaries-with-rust/

Rustify. (2026). *SQLx vs Diesel vs SeaORM: Which Rust ORM to use in 2026?* https://rustify.rs/articles/rust-sqlx-vs-diesel-vs-seaorm-2026

Ryhl, A. (n.d.). *Actors with Tokio* [Blog post]. https://ryhl.io/blog/actors-with-tokio/

Sentry. (n.d.). *Uploading debug symbols — Sentry for macOS*. Sentry Documentation. https://docs.sentry.io/platforms/apple/guides/macos/dsym/

Serokell. (2023). *Rust in production: 1Password*. Serokell Blog. https://serokell.io/blog/rust-in-production-1password

signalapp. (2026b). *libsignal/swift/README.md* [GitHub repository file]. GitHub. https://github.com/signalapp/libsignal/blob/main/swift/README.md

SignalApp. (n.d.). *libsignal: Home to the Signal Protocol as well as other cryptographic primitives which make Signal possible* [Software repository]. GitHub. https://github.com/signalapp/libsignal

Slack Engineering. (2019). *Client consistency at Slack: Beyond Libslack*. https://slack.engineering/client-consistency-at-slack-beyond-libslack/

Slashdot. (2025). *Rust in Android: More memory safety, fewer revisions, fewer rollbacks, shorter reviews*. Retrieved 2026-09-01, from https://developers.slashdot.org/story/25/11/17/012246/rust-in-android-more-memory-safety-fewer-revisions-fewer-rollbacks-shorter-reviews

slint-ui. (2026). *slint/docs/testing.md* [GitHub repository file]. GitHub. https://github.com/slint-ui/slint/blob/master/docs/testing.md

slint-ui. (n.d.). *Accessibility: Support text input widgets* [GitHub Issue #2895]. https://github.com/slint-ui/slint/issues/2895

slint-ui. (n.d.). *Color Management* [GitHub Discussion #4988]. https://github.com/slint-ui/slint/discussions/4988

slint-ui. (n.d.). *Glyph rendering differs between Skia, FemtoVG, and Slint Software Renderer* [GitHub Issue #3742]. https://github.com/slint-ui/slint/issues/3742

Slint. (n.d.-a). *slint/LICENSE.md* [GitHub repository]. https://github.com/slint-ui/slint/blob/master/LICENSE.md

Slint. (n.d.-b). *Slint | Declarative GUI for Rust, C++, JavaScript & Python* [Project homepage]. https://slint.dev/

Slint. (n.d.-c). *Slint FAQs*. https://slint.dev/faqs

Slint. (n.d.-d). *MenuBar | Slint Docs*. https://docs.slint.dev/latest/docs/slint/reference/window/menubar/

Sparkle Project. (n.d.). *Documentation*. Retrieved 2026-09-01, from https://sparkle-project.org/documentation/

Stack Overflow. (2025). *2025 Stack Overflow Developer Survey: Technology*. Retrieved 2026-09-01, from https://survey.stackoverflow.co/2025/technology

StackTrack. (2026). *Understanding GitHub Actions runner costs in 2026 (and why your bill moved)*. Retrieved 2026-09-01, from https://stacktrack.com/posts/understanding-github-actions-runner-costs-in-2026/

Stadia Maps. (n.d.). *Ferrostar: Building a cross-platform navigation SDK in Rust (Part 1)* [Blog post]. https://stadiamaps.com/blog/ferrostar-building-a-cross-platform-navigation-sdk-in-rust-part-1/

Stadia Maps. (n.d.). *Rust on iOS: XCFramework & Swift Package packaging (Ferrostar Part 2)* [Blog post]. https://stadiamaps.com/blog/ferrostar-building-a-cross-platform-navigation-sdk-in-rust-part-2/

swiftcsv. (2026). *SwiftCSV* [GitHub repository]. GitHub. https://github.com/swiftcsv/SwiftCSV

Tauri Contributors. (2026a). *Releases* [GitHub]. tauri-apps/tauri. Retrieved 2026-09-01, from https://github.com/tauri-apps/tauri/releases

Tauri Contributors. (2026b). *macOS code signing*. Tauri v2 Documentation. Retrieved 2026-09-01, from https://v2.tauri.app/distribute/sign/macos/

tauri-apps. (2023). *[feat] Build universal binaries for macOS* (Issue #3317) [GitHub]. tauri. Retrieved 2026-09-01, from https://github.com/tauri-apps/tauri/issues/3317

tauri-apps. (2026a). *tauri-bundler* [Crate source]. tauri-apps/tauri, crates/tauri-bundler. Retrieved 2026-09-01, from https://github.com/tauri-apps/tauri/tree/dev/crates/tauri-bundler

Tauri-apps. (n.d.). *[feat] Get system accent color on macOS* [GitHub Issue #8590]. https://github.com/tauri-apps/tauri/issues/8590

Tauri. (n.d.-a). *Webview in tauri::webview* [API docs]. https://docs.rs/tauri/latest/tauri/webview/struct.Webview.html

Tauri. (n.d.-b). *Window and Webview Management* [DeepWiki summary of tauri-apps/tauri]. https://deepwiki.com/tauri-apps/tauri/2.3-window-and-webview-management

Tauri. (n.d.-c). *Window Menu | Tauri*. https://v2.tauri.app/learn/window-menu/

Teare, D. (n.d.). *Behind the scenes of 1Password for Linux*. Medium. https://dteare.medium.com/behind-the-scenes-of-1password-for-linux-d59b19143a23

Terhechte, B. (2024, March 20). *uniffi-swift-async-example: How to use uniffi-swift with async/await* [Blog post]. https://terhech.de/posts/2024-3-20-uniffiswiftasyncexample-how-to-use-uniffiswift-with-asyncawait.html

The Cargo Book. (n.d.). *Workspaces* (`default-members`). Rust project. Retrieved September 1, 2026, from https://doc.rust-lang.org/cargo/reference/workspaces.html

The Hacker News. (2025). *Rust adoption drives Android memory safety bugs below 20% for first time*. Retrieved 2026-09-01, from https://thehackernews.com/2025/11/rust-adoption-drives-android-memory.html

Tokio. (n.d.). *Runtime in tokio::runtime* [Crate documentation]. https://docs.rs/tokio/latest/tokio/runtime/struct.Runtime.html

Tritium. (n.d.). *Thanks for all the frames: Rust GUI observations*. https://tritium.legal/blog/desktop

TTP. (n.d.). *Consider using Rust in your next diagnostic and medical device*. https://www.ttp.com/insights/do-you-trust-your-software-why-you-should-seriously-consider-using-rust-in-your-next-diagnostic-and-medical-device

w3resource. (n.d.). *Rusqlite vs SQLite comparison for Rust applications*. https://www.w3resource.com/sqlite/snippets/rusqlite-vs-sqlite.php

Warp. (n.d.-a). *Why is building a UI in Rust so hard?* Warp Blog. https://www.warp.dev/blog/why-is-building-a-ui-in-rust-so-hard

Warp. (n.d.-b). *How Warp works*. Warp Blog. https://www.warp.dev/blog/how-warp-works

WebAIM. (n.d.). *Using VoiceOver to Evaluate Web Accessibility*. https://webaim.org/articles/voiceover/

WebKit. (n.d.). *Wide Gamut Color in CSS with Display-P3* [Blog post]. https://webkit.org/blog/10042/wide-gamut-color-in-css-with-display-p3/

Welsh, M. (2022, November 23). *Using Rust at a startup: A cautionary tale*. Medium. https://mdwdotla.medium.com/using-rust-at-a-startup-a-cautionary-tale-42ab823d9454

wgpu. (n.d.). *SurfaceColorSpace in wgpu*. https://docs.rs/wgpu/latest/wgpu/enum.SurfaceColorSpace.html

Wikipedia. (n.d.-a). *Tauri (software framework)*. https://en.wikipedia.org/wiki/Tauri_(software_framework)

Wikipedia. (n.d.-b). *Zed (text editor)*. https://en.wikipedia.org/wiki/Zed_(text_editor)

Wren Learns Rust. (2026, March 11). *The Rust GUI landscape in 2026: Picking your framework*. https://wrenlearnsrust.com/posts/2026-03-11-rust-gui-landscape-2026.html

wybxc. (2026). *A 2026 survey of Rust GUI libraries* [Blog post; not retrievable in this pass, HTTP 403]. https://blog.wybxc.cc/blog/rust-gui-survey-2026/

Xu, H., Chen, Z., Sun, M., Zhou, Y., & Lyu, M. R. (2021). Memory-safety challenge considered solved? An in-depth study with all Rust CVEs. *ACM Transactions on Software Engineering and Methodology*. https://dl.acm.org/doi/10.1145/3466642

Zed Industries. (2026). *Stable release 1.0.0*. https://zed.dev/releases/stable/1.0.0

Zed Industries. (n.d.-a). *Zed is now open source*. Zed Blog. https://zed.dev/blog/zed-is-now-open-source

Zed Industries. (n.d.-b). *Async Rust* [Blog post]. Zed's Blog. https://zed.dev/blog/zed-decoded-async-rust

Zed Industries. (n.d.-c). *Hired through GitHub: Part 1*. Zed Blog. Retrieved 2026-09-01, from https://zed.dev/blog/hired-through-github-part-1

Zed Industries. (n.d.-d). *Zed — Your last next editor* [Project homepage]. https://zed.dev/

zed-industries. (n.d.). *Accessibility (a11y) in Zed* [GitHub Discussion #6576]. https://github.com/zed-industries/zed/discussions/6576

zed-industries. (n.d.). *Is screen reader compatibility on Mac OS in the works* [GitHub Discussion #8146]. https://github.com/zed-industries/zed/discussions/8146

zed-industries. (n.d.). *Using The Editor Functions With MacOS Built-In VoiceOver Screenreader* [GitHub Discussion #6714]. https://github.com/zed-industries/zed/discussions/6714
