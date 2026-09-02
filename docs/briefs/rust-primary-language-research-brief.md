---
canonical-id: 6669392d-21f6-4ff4-86f5-96cf49edeaff
---

# Research Brief: Rust as the Primary Language for SpectroCapture (macOS)

_2026-09-01. To be passed to a deep research agent._

---

## Context

SpectroCapture is an open-source macOS desktop app for bulk color acquisition: a user imports an inventory of physical items (markers, swatches, products), scans heads-down through it with a Nix Spectro 2 / Spectro L spectrophotometer (BLE or USB), and every measurement lands in a local SQLite file the user owns. The vendor SDK is a license-keyed binary framework consumed as a package dependency — never vendored, architecturally single-device and single-session — and CI can build the project but cannot exercise any device code path. The instrument's raw measurement payload is stored as the canonical record; Lab/XYZ/LCh/Luv/sRGB/HSL are derived from it, with out-of-gamut sRGB values explicitly flagged rather than silently substituted. See `docs/product/vision.md` and `docs/briefs/` in the repository for the full product and research record.

Prior architecture research (`docs/briefs/macos-swiftui-app-architecture-research-results.md`) assumed Swift end-to-end and recommended — without an ADR yet ratifying any of it — MV over MVVM/TCA, an SPM module split around a `SpectroDevice` protocol with mock and live implementations, GRDB for SQLite, swift-dependencies for the device seam, Swift 6 strict concurrency, and Swift Testing. The product vision lists SwiftUI as decided. **This brief formally re-opens the language and UI-stack question for the purposes of the project's first architecture ADR.** The proposal on the table: Rust as the primary implementation language.

The research must evaluate three candidate shapes and recommend one: **(A) Rust core + SwiftUI shell** — business logic, data layer, and color math in Rust beneath a native Swift/SwiftUI UI, bridged over FFI; **(B) full-Rust app** — Rust owns the UI as well, via a Rust-native UI stack; **(C) all-Swift** — the incumbent baseline from the prior research. The following are fixed inputs, not up for re-litigation: macOS-only (no iOS); local-first and offline-first with no server or cloud sync; the user-owned, portable, directly-queryable SQLite file; one device at a time; no per-item metadata entry during capture; corrections never destroy data; gamut honesty; and the vendor SDK as-is (the research covers driving it *from* the chosen architecture, never replacing or reimplementing it).

The output feeds a decision-grade ADR in `docs/decisions/`. Precision matters more than breadth: version numbers, shipped-app precedent, and measured evidence beat general Rust-vs-Swift advocacy, which is abundant and mostly useless here.

---

## Questions

### 1. Architecture Shapes & Shipped Precedent

- Which shipped, non-trivial macOS desktop apps use a Rust core beneath a native Swift/AppKit/SwiftUI shell (1Password 8, Signal Desktop's libsignal, Firefox's Rust components, others)? For each: what lives on which side of the boundary, and what has the team publicly reported about the boundary's maintenance cost?
- Which shipped macOS apps are full-Rust including the UI (Zed/gpui, Warp, others), and what UI stack does each actually use — a general-purpose framework or a bespoke one that doesn't transfer?
- What do engineering retrospectives from 2023–2026 identify as the single biggest win and the single biggest regret of the Rust-core-plus-native-shell architecture?
- Is there any shipped precedent for a hardware-peripheral-centric macOS app (BLE/USB instrument, one device, event-streaming) built with a Rust core? What did the device layer look like?
- At what codebase size or team size do practitioners report the two-language overhead of shape A paying for itself versus dragging — and is a solo-to-small-team open-source project above or below that line?

### 2. The FFI Boundary (Shape A)

- UniFFI vs. swift-bridge vs. cbindgen/cxx vs. a hand-rolled C ABI: current version, maintenance status, and backing organization of each as of 2026, and which is the dominant recommendation for Swift↔Rust specifically?
- Can a Rust `async fn` surface as a Swift `async/await` call today, and through which tool at which version? What executor/runtime bridging does it require?
- How are continuous event streams best modeled across the boundary — the app's core pattern is a hardware scan stream currently specced as an `AsyncStream` over the vendor SDK? Callbacks, polling, channels: concrete patterns with crate names and versions.
- What are the current type-mapping limits: enums with associated values, `Result`/`throws`, struct graphs, and zero-copy transfer of raw byte buffers (measurement payloads)?
- What is the canonical build integration for a Rust static library inside an SPM package or Xcode project (XCFramework? SPM build plugin?), including universal arm64 + x86_64 output — and what breaks in incremental builds?
- What is the debugging story across the boundary: lldb over mixed Swift/Rust frames, and symbolication of Rust frames in macOS crash reports?

### 3. Vendor SDK Integration (Fixed Constraint)

- The vendor SDK is a Swift/Objective-C binary framework. What are the viable patterns for a Rust core to drive a Swift-only SDK — and if the device layer must stay in Swift, what does that do to the "Rust as primary language" claim in practice?
- What is the 2026 maturity of calling Swift/ObjC frameworks *from* Rust (objc2 family, swift-bridge's reverse direction): versions, limits, and real usage?
- If the device layer stays Swift in shape A: what is the cleanest ownership split so the `SpectroDevice` seam (protocol + mock/live) still lets CI and contributors exercise the Rust core with no hardware and no license key?
- Is direct Rust BLE (btleplug + CoreBluetooth) even relevant given the vendor SDK owns the transport — or does it only matter for a hypothetical future multi-instrument expansion?
- Does a Rust core introduce any new constraints on runtime license-key activation and on keeping the key out of the binary and repo?

### 4. Rust-Native UI Options (Shape B)

- For each of egui, Slint, Tauri, Dioxus, gpui, and any credible newcomer: current version, license, macOS-specific maturity, and at least one shipped macOS app using it.
- Which of them support VoiceOver accessibility, the native menu bar, standard shortcuts, and multi-window on macOS today — at what fidelity?
- Wide-gamut color: which frameworks can render Display-P3 correctly and integrate with ColorSync? For an app whose core promise is color fidelity with explicit gamut-clipped flags, can any non-native stack guarantee it is not silently rendering unmanaged sRGB?
- Which frameworks have a proven virtualized list/table story at 10,000+ rows with per-cell custom drawing (color swatch grids for collection browsing)?
- What are the text-input, IME, and localization limitations of each on macOS?
- What is the honest 2025–2026 community verdict on production-readiness of Rust-native desktop UI on macOS — ship it, or wait?

### 5. Color Science & Data Layer in Rust

- Which crates cover spectral→XYZ→Lab/LCh/Luv/sRGB/HSL conversion and ΔE2000 (palette, kolor, others): versions, correctness validation, and how they compare against established references (ICC, the Python colour-science library)?
- ICC profile and ColorSync interop from Rust — reading display profiles, converting through lcms2 bindings: what exists and how mature is it?
- rusqlite vs. sqlx vs. diesel for a local, user-owned, portable SQLite file: which best supports careful schema migration, WAL mode, and typed blob handling — and how does the best of them compare feature-for-feature with GRDB, the incumbent Swift recommendation?
- Are there Rust-specific patterns or pitfalls for the canonical-raw-payload-plus-recomputed-derived-values schema (versioned derivation pipelines over blobs)?
- CSV inventory import: which crate handles real-world CSV (encodings, quoting, malformed rows) best, and how does it compare to Swift's options?

### 6. Concurrency & the Hardware-Paced Pipeline

- What replaces the settled `AsyncStream`-bridge pattern in each shape: tokio (which version is the 2026 default), smol, or std-only — and what do desktop apps (not servers) actually use?
- What are the concrete idioms for enforcing one-scan-in-flight against a single-device, single-session SDK — actor-style serialization in Rust vs. Swift actors?
- In shape A, who owns the event loops? Patterns and pitfalls for running a tokio runtime inside a macOS app process alongside the main RunLoop (thread QoS, App Nap, energy impact)?
- How do UniFFI/swift-bridge propagate cancellation and timeouts across the boundary mid-operation — e.g., a scan cancelled from the UI while the SDK call is in flight?
- Do the generated Swift bindings satisfy Swift 6 strict-concurrency checking (Sendable conformance), or does the FFI boundary become a permanent `@unchecked` escape hatch?

### 7. Testing Without Hardware

- In each shape, where should the `SpectroDevice` mock seam live so that CI — which has no hardware and no license key — exercises the maximum amount of logic?
- What is the recommended Rust testing stack for this kind of app (cargo test, cargo-nextest, proptest/quickcheck for color math): versions and conventions?
- How do teams contract-test an FFI boundary so the generated bindings can't drift from either side? Real project examples.
- What is the UI-testing story per shape: XCUITest against a SwiftUI shell vs. what, exactly, for egui/Slint/Tauri?
- Are there reference datasets (Munsell, RIT, CIE fixtures) and crates commonly used to property-test color conversions against ground truth?

### 8. Build, Signing, Distribution

- What is the canonical pipeline for building, Developer-ID-signing, and notarizing a macOS `.app` whose binary is Rust or mixed Rust+Swift — cargo-bundle, cargo-dist, Tauri's bundler, or Xcode driving everything? What do teams that actually ship use, at which versions?
- What is the exact mechanism for producing a universal (arm64 + x86_64) binary from a mixed cargo/SPM build, and what does it cost in CI time?
- Does Sparkle integrate cleanly with a non-Xcode or full-Rust app? What are the alternatives if not?
- Does Rust introduce friction with the App Sandbox and hardened runtime (entitlements, library validation)? The sandbox decision is still open for this project — does a Rust core or Rust UI bias it either way?
- What does GitHub Actions macOS CI look like for a mixed Rust+Swift toolchain versus pure Swift: cache strategy, cold and warm build minutes, known flakes?

### 9. Contributor Ecosystem & DX

- For an open-source color/hardware tool, what evidence exists on contributor pools — do Rust projects of comparable size attract more or fewer drive-by contributors than Swift projects (Octoverse data, language surveys, contributor counts of comparable apps)?
- What are realistic cold and warm build times for an app-sized Rust codebase vs. a comparable Swift one, and how much does an FFI boundary slow the edit-compile-run loop in shape A?
- What is day-to-day IDE reality in a mixed repo — rust-analyzer plus Xcode side by side vs. pure Xcode: what breaks (jump-to-definition across the boundary, refactoring, debugging)?
- What do teams report about onboarding contributors onto a two-language codebase — does shape A effectively require every core contributor to know both?
- What is the honest evidence (2024–2026) that Rust reduces defect rates for this class of app — desktop, IO-bound, GC-irrelevant, not memory-pressured — as opposed to systems software where the safety argument originated?

### 10. Decision Framework & Risks

- What decision criteria have engineering organizations published for native-vs-Rust-core choices, and which of those criteria actually apply at this project's scale (solo-to-small open-source team, no existing code)?
- For this app's real hot paths — SQLite reads over ~10k rows, color-space math per scan, BLE event handling at hardware pace — is Rust's performance advantage material or noise? Cite measurements, not folklore.
- What documented cases exist of teams abandoning a Rust-core-plus-native-shell or Rust-UI approach, and what was the stated cause?
- Given no code exists yet: if shape A were chosen and later reversed, what is the migration cost back to all-Swift — and vice versa? Which choice preserves the most optionality?
- All-Swift is the incumbent recommendation of the prior architecture research. State, as a testable bar, what evidence threshold an ADR should demand before overturning it.
- Is there shipped precedent for sequencing strategies — start all-Swift and extract a Rust core later, or build the Rust core first and wrap UI iterations around it — and what did each cost the teams that tried?

---

## Deliverable Requested

For each question, provide the answer with:

- Source links, version numbers, and package names where applicable
- Side-by-side comparison tables where the question spans multiple options
- A clear statement of which position is most commonly held in recent (2024–2026) sources when community opinion is divided

Flag any answer that is inferred from indirect evidence rather than explicit documentation. State the version of any SDK, framework, or specification the answer applies to.

**Citation requirements — mandatory throughout your response:**
Every factual claim in your answers that originates from a source must be cited inline using APA author-date style: (Author, Year) or (Organization, Year). Do not cite general knowledge. Do not use footnotes or numbered references inline — use author-date only. At the end of your response, include a References section with a full APA reference list, alphabetical by first author or organization, covering every source cited inline. The response is considered incomplete without inline citations and the References section.

**Closing recommendation — mandatory:**
At the end of your response and before the References section, provide an opinionated closing recommendation: which of the three shapes this project should adopt, and under what conditions that verdict would flip. It must be a decision, not a list of options. Cite the key sources that informed the recommendation inline.

**The following two sections are also mandatory. The response is considered incomplete without all four of: answers with inline citations, closing recommendation, Question Status, Unanswered Questions Summary, and References.**

**Question Status** — reproduce every question from this brief, grouped under its original concern area heading. Mark each `[x]` if answered or `[ ]` if not. For every unanswered question, provide: (1) the reason it was not answered, citing the specific gap in available documentation or sources, and (2) a concrete, specific follow-up action the reader can take to close the gap. "Needs more research" is not an acceptable follow-up. "Prototype the UniFFI async stream binding against a mock device crate and measure per-event overhead" is.

**Unanswered Questions — Summary** — a consolidated flat list of all unanswered questions extracted from the Question Status section. Each entry must include a one-line reason and one-line follow-up. If all questions were answered, write "All questions answered." explicitly.
