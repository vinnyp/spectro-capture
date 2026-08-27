# Research Brief: macOS SwiftUI App Architecture and Scaffolding

## Context

**SpectroCapture** is an open-source macOS app for bulk color acquisition: a user imports an inventory of physical items, scans heads-down through it with a spectrophotometer, and every measurement lands in a local SQLite database they own. This brief is about the **chassis** — the app skeleton that everything else sits inside. Not the capture screen, not the collection view, but the decisions a team makes once, early, and lives with: how the code is organized into modules, how state flows, how dependencies are injected, how the app windows and navigates, how concurrency is structured, how it is tested without the hardware, and how it is signed and shipped.

**Two companion briefs already cover the feature surfaces, and this brief must not duplicate them.** The capture-mode brief covers the heads-down capture surface, its keyboard and focus handling, and bridging the asynchronous hardware SDK into structured concurrency — and its answers are already in, so treat `AsyncStream` for the hardware bridge and the `onKeyPress`-versus-`NSEvent`-monitor question as **settled and out of scope**. The collection-mode brief covers list and grid virtualization and rendering performance at scale, the database library choice and change observation, and colour management — all **out of scope here**. So is the seam between capture and browse modes, which the capture brief answered. Where this brief touches state management or performance, it does so at the *application* level (how state is owned and scoped across the whole app) rather than at the level of one screen.

**Two questions are deliberately inherited.** The capture-mode research came back with 36 of 57 questions answered, and two of the unanswered ones were chassis questions that had landed in the wrong brief: current guidance on `@Observable` versus `ObservableObject` and where state should live relative to the view hierarchy, and what published guidance or measurement exists on SwiftUI view-update cost and how to profile it. Both are re-asked here, at application scope, and the reported gap was that the available material covered event-handling snippets rather than state-ownership or profiling practice — so answering them will require sources that go beyond API reference documentation.

**Already decided — do not research or re-litigate these.** Platform is macOS-only; iOS, Windows, and Linux are explicit non-goals. **SwiftUI** is the UI framework — the question is how to structure a SwiftUI app well, never whether to use AppKit instead wholesale (targeted `NSViewRepresentable` escape hatches are fair game where SwiftUI genuinely cannot do the job). Persistence is a local **SQLite** file that stays portable and externally queryable. The app is offline-first with no cloud sync and no server component. It is **open source from day one**, and the vendor SDK is pulled as a package dependency rather than vendored — no SDK binaries, headers, or licence keys in the repo — which means **CI can build the project but cannot exercise any device code path**, and contributors without the hardware must still be able to run the app and its tests. That constraint is a hard architectural input, not a preference.

**The gap this closes.** The project has a product vision, a competitive analysis, and two decision records, but **no app-architecture decision record**. This brief, together with the two feature briefs, feeds it. The questions below are the ones whose answers are expensive to change later — module boundaries, the state-ownership model, the concurrency posture under Swift 6, and the testability seams that determine whether a contributor without a spectrophotometer can do useful work. Answers should be grounded in shipped applications, Apple's documentation with version numbers, and the current state of community practice — including where that practice is actively contested.

---

## Questions

### 1. Architectural Pattern Selection for SwiftUI on macOS

- What is the current state of the debate between the **"MV" / plain-SwiftUI** position (views observing model objects directly, no view-model layer) and **MVVM** for SwiftUI, and which specific arguments and sources anchor each side?
- Under what documented conditions does **The Composable Architecture (TCA)** pay for its complexity, what is its current major version, and what do its own maintainers say about when not to use it?
- What evidence exists from shipped macOS applications about which architectural pattern holds up as an app grows past a handful of screens, as opposed to which reads best in a tutorial?
- How does the choice of architectural pattern interact with **Swift 6 strict concurrency** — are any of these patterns materially harder to make `Sendable`-clean, and is that documented?
- What are the documented failure modes of applying **UIKit-era MVVM or VIPER habits** to SwiftUI, and which specific symptoms indicate a codebase has done so?

### 2. Project Structure and Module Boundaries

- What are the documented approaches to structuring a medium-sized macOS app — a single app target, local **Swift Package Manager** packages, or a workspace of multiple projects — and what specific benefits do teams report from each?
- At what point do practitioners report that splitting into local SPM packages pays off, and what are the documented costs (build times, Xcode indexing, previews breaking across module boundaries)?
- What are the established conventions for **feature-module boundaries** in a SwiftUI app, and how do shipped projects prevent a shared "Core" or "Common" module from becoming a dumping ground?
- How should a **hardware or device-abstraction layer** be isolated as its own module so that the rest of the app compiles and runs without it, and what are documented examples of this pattern?
- What is documented practice for organizing **resources, localization, and assets** across multiple SPM modules on macOS, and what are the known pitfalls with `Bundle.module`?
- What open-source macOS SwiftUI applications of comparable scope are worth reading as structural references, and what does each demonstrate?

### 3. State Management and Data Flow

- What is the current documented guidance on **`@Observable` (the Observation framework) versus `ObservableObject`** — which macOS version gates each, what are the documented behavioural and performance differences, and is `ObservableObject` considered deprecated in practice?
- Where should application state live relative to the view hierarchy — a single root store, per-feature stores, or `Environment`-injected services — and what evidence or guidance supports each at application scale?
- What are the documented semantics and pitfalls of SwiftUI's state property wrappers on macOS (`@State`, `@Binding`, `@Environment`, `@Bindable`, `@AppStorage`, `@SceneStorage`), and which are commonly misused?
- How does `@Observable` **invalidation granularity** actually work — what triggers a view update, and what documented techniques exist to avoid over-broad invalidation in a large app?
- What is documented practice for separating **ephemeral UI state** from **persistent domain state**, and how do shipped apps keep the two from entangling?
- What published guidance or measurement exists on **SwiftUI view-update cost**, and what specific profiling tools and techniques (the SwiftUI instrument in Instruments, `Self._printChanges()`, view-body counting) are recommended for diagnosing it?

### 4. Dependency Injection and Testability Seams

- What are the dominant dependency-injection approaches in SwiftUI applications — `Environment`-based injection, initializer injection, a dedicated container, or the `swift-dependencies` package — and what are the documented trade-offs?
- What is documented practice for defining a **protocol-based hardware abstraction** with a real implementation and a mock, and how do shipped projects select between them at launch, in tests, and in SwiftUI previews?
- How do applications keep a mock implementation **honest** — that is, prevent it from drifting into something that no longer resembles the real device's behaviour, error cases, and timing?
- What patterns exist for making SwiftUI **previews work** against injected dependencies without special-casing preview code throughout the app?
- What is documented practice for injecting dependencies across **SPM module boundaries** without creating circular dependencies or a god-object container?

### 5. App Lifecycle, Windowing, and Scenes on macOS

- What are the documented capabilities and limitations of the SwiftUI **`App` and `Scene`** lifecycle on macOS compared with an `NSApplicationDelegate`, and what specifically still requires `NSApplicationDelegateAdaptor`, as of which macOS version?
- Which SwiftUI scene types are available on macOS (`WindowGroup`, `Window`, `Settings`, `MenuBarExtra`, `DocumentGroup`, `Utility`/inspector windows), what does each do, and from which macOS version?
- What is documented practice for a **single-window macOS app** that also needs auxiliary windows (a settings window, a device panel, an inspector), and how is window identity and reopening managed?
- How do applications correctly handle **app termination, window close, and sleep** — including intercepting termination when work is in progress — and which APIs are involved on macOS?
- What is documented practice for building the **macOS menu bar** in SwiftUI (`commands`, `CommandGroup`), and what are the known limitations that force a drop to AppKit?
- How should an app handle **`Settings` / preferences** in SwiftUI on macOS, and what does the HIG currently recommend for preference organization?

### 6. Navigation Architecture and State Restoration

- What is current documented guidance for `NavigationSplitView` on macOS — its column configurations, sidebar behaviour, and how it interacts with toolbars and inspectors — and what changed across recent macOS versions?
- What are the documented approaches to **programmatic / state-driven navigation** in SwiftUI (`NavigationPath`, typed navigation destinations, enum-based routing), and which hold up in a multi-window macOS app?
- What is documented practice for **state restoration** on macOS — restoring window position, size, selection, and navigation state across launches — and which SwiftUI APIs (`@SceneStorage`) versus AppKit mechanisms apply?
- How do applications structure navigation so it remains **testable and deep-linkable**, and what evidence exists that a routing layer is worth its cost in a SwiftUI app?
- What are the documented pitfalls of SwiftUI navigation on macOS specifically — as opposed to iOS — that teams report hitting?

### 7. Concurrency Architecture under Swift 6

- What does enabling **Swift 6 language mode / strict concurrency** actually require of an existing SwiftUI app, and what is the documented migration path and its typical cost?
- What is the current documented guidance on **`@MainActor` placement** — annotating views, view models, or whole modules — and what are the documented consequences of over-annotating?
- How should **long-running background work** (database queries, file import, export) be structured relative to the main actor, and what are the documented patterns for keeping the UI responsive?
- What are the documented pitfalls of `Sendable` conformance for **model types crossing actor boundaries**, and what techniques (value types, `@unchecked Sendable`, actor isolation) are recommended and when?
- What is documented practice for **cancellation** of in-flight async work when a user navigates away or closes a window, and how does SwiftUI's `task` modifier participate?
- How do applications **test** actor-isolated and async code on macOS, and what does Swift Testing (versus XCTest) offer here, from which toolchain version?

### 8. Error Handling, Logging, and Diagnostics

- What are the documented approaches to **error-type design** in a Swift application — typed throws (and from which Swift version), a single app-error enum, or per-module error types — and what are the trade-offs?
- What is current documented practice for **logging on macOS** using `OSLog` / the unified logging system, including privacy annotations, log levels, and how to retrieve logs from a user's machine?
- How should an app **surface errors to the user** versus log them silently, and what guidance exists on the boundary in the macOS HIG?
- What is documented practice for building a **user-facing diagnostics or support bundle** in an open-source app, and how do projects avoid capturing sensitive data in it?
- How do applications handle and report **unrecoverable errors** (a corrupt database, a failed migration) in a local-first app where the user owns the data file?

### 9. Testing Strategy and CI Without Hardware

- What is the current documented state of **Swift Testing versus XCTest** — what does Swift Testing support, from which Xcode/toolchain version, and is it recommended for new projects?
- What are the documented approaches to **testing SwiftUI views** on macOS — ViewInspector, snapshot testing, XCUITest — and what does each actually verify versus claim to?
- How do open-source projects structure CI so that **contributors without the hardware** can run the full test suite, and what conventions exist for marking tests that require a physical device?
- What is documented practice for running **macOS CI** — GitHub Actions macOS runners, `xcodebuild` invocation, code-signing in CI for unsigned test builds — and what are the known cost and availability constraints?
- What patterns exist for a **`needs-hardware-verify` review workflow**, where some pull requests can only be validated by a maintainer with the device, and are there documented examples in other hardware-adjacent open-source projects?
- What is documented practice for guarding a repository against **committed secrets or licence keys** (pre-commit hooks, CI scanning), and which tools are current?

### 10. Distribution — Sandbox, Entitlements, Signing, and Notarization

- What is the current documented process for **signing and notarizing** a macOS app, which tool replaced `altool`, and what are the specific steps and common failure modes?
- What are the documented differences between distributing **outside the Mac App Store** (Developer ID plus notarization) versus through it, and what does each imply for entitlements and sandboxing?
- Which specific **entitlements and `Info.plist` usage strings** does an app need for Bluetooth, USB, serial-device, and outbound-network access on macOS, and what happens at runtime when one is missing?
- How does the **App Sandbox** constrain an app that must read and write a user-chosen SQLite file, and what is documented practice for security-scoped bookmarks in that scenario?
- What is documented practice for **distributing an open-source macOS app** whose users must supply their own licence key and hardware — release artifacts, Homebrew casks, unsigned builds from source — and what do comparable projects do?
- What are the documented approaches to **automatic updates** for a non-App-Store macOS app (for example Sparkle), and what is the current recommended version and its signing requirements?

---

## Deliverable Requested

For each question, provide the answer with:
- Source links, version numbers, and package names where applicable
- Side-by-side comparison tables where the question spans multiple options
- A clear statement of which position is most commonly held in recent (2024–2026) sources when community opinion is divided

Flag any answer that is inferred from indirect evidence rather than explicit documentation. State the version of any SDK, framework, or specification the answer applies to — for SwiftUI, Swift, Xcode, and macOS answers specifically, state the minimum version each API or capability requires, and note where behaviour changed between versions.

**Citation requirements — mandatory throughout your response:**
Every factual claim in your answers that originates from a source must be cited inline using APA author-date style: (Author, Year) or (Organization, Year). Do not cite general knowledge. Do not use footnotes or numbered references inline — use author-date only. At the end of your response, include a References section with a full APA reference list, alphabetical by first author or organization, covering every source cited inline. The response is considered incomplete without inline citations and the References section.

**Closing recommendation — mandatory:**
At the end of your response and before the References section, provide an opinionated closing recommendation. It must be a decision, not a list of options: state the app architecture you would build — the architectural pattern, the module structure, the state-ownership model, the dependency-injection approach, the concurrency posture, the testing strategy, and the distribution path — and say plainly what you would *not* build. This project is a single-developer open-source app, not an enterprise team codebase; where a pattern is popular but would be over-engineering at this scale, say so explicitly and recommend against it. Cite the key sources that informed the recommendation inline.

**The following two sections are also mandatory. The response is considered incomplete without all four of: answers with inline citations, closing recommendation, Question Status, Unanswered Questions Summary, and References.**

**Question Status** — reproduce every question from this brief, grouped under its original concern area heading. Mark each [x] if answered or [ ] if not. For every unanswered question, provide: (1) the reason it was not answered, citing the specific gap in available documentation or sources, and (2) a concrete, specific follow-up action the reader can take to close the gap. "Needs more research" is not an acceptable follow-up. "Create an empty SwiftUI macOS app, enable Swift 6 language mode in build settings, and record the exact diagnostics emitted" is.

**Unanswered Questions — Summary** — a consolidated flat list of all unanswered questions extracted from the Question Status section. Each entry must include a one-line reason and one-line follow-up. If all questions were answered, write "All questions answered." explicitly.

---

## Question Status

[This section is mandatory. Reproduce every question from this brief, grouped under its original concern area heading, marked [x] answered or [ ] unanswered. For every unanswered question give a specific documentation-gap reason and a concrete follow-up action. Do not omit any question.]

---

## Unanswered Questions — Summary

[A consolidated flat list of only the unanswered questions, extracted from the Question Status section above, so gaps can be scanned without reading the full status list. Each entry: question — one-line reason → one-line follow-up action. If all questions were answered, write "All questions answered." and omit the list.]

---

## References

[Full APA reference list, alphabetical by first author or organization, covering every source cited inline in your answers. Mandatory if any answers contain inline citations, which they must.]

_Note on this brief's own sources: the Context block above is synthesized from internal project documents (the SpectroCapture vision spec, ADR-0001/ADR-0002, and the returned results of the companion capture-mode research brief) and cites no external published sources, so no author-side reference list is included. This does not relax the citation requirement on your answers._
