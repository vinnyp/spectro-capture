# Research Results: macOS SwiftUI App Architecture and Scaffolding

## 1. Architectural Pattern Selection

### Apple's position is not what either camp claims

Apple's normative data-flow guidance **never uses the words "view model" or "MVVM."** *Managing model data in your app* (macOS **14.0**) frames the subject entirely as a *data model* observed directly by views. Apple's flagship sample — *Landmarks: Building an app with Liquid Glass* (macOS **26.0**) — contains **37 Swift files in exactly two groups, `Model/` and `Views/`, and zero files named `ViewModel`**. Its root store is `@Observable @MainActor class ModelData`. *(Verified by the agent extracting the published ZIP and inflating three files.)*

**But Apple is not doctrinally anti-view-model, and this is what the community debate misses.** In WWDC25 session 306, Apple's presenter hits the over-broad-dependency trap *in this same sample* and fixes it by **introducing view models**: "instead of each view being dependent on the full array of favorites, each view only depends directly on its own landmark's view model."

**Apple uses a view model as a per-item invalidation-granularity device, not as a per-screen architectural layer.** That reframing is the most useful thing this question yields: **the real axis is not "layer or no layer" but "what is the unit of observation."**

**MV camp anchors:** Ricouard (2025) — "It's 2025… you don't need ViewModels in SwiftUI"; and his *MV Patterns Reference*, the most operationally useful artefact in the debate, a checklist naming five conditions where a view model is unnecessary indirection: it "mirror[s] local view state, wrap[s] values already available through `@Environment`, duplicate[s] `@Query`, `@State`, or `Binding`-based data flow, exist[s] only because the view body is too long, [or] hold[s] one-off async loading logic that can live in `.task` plus local view state." Azam (2022, 2026) — "I was the biggest advocate of the MVVM design pattern… If you are a developer and you jumped on the MVVM bandwagon then I was the driver," and on the boilerplate: "For each screen, I created a separate view model… `MovieListViewModel`, `AddMovieViewModel`, and `MovieDetailViewModel`."

**MVVM camp:** Hudson (2024) teaches it while conceding "This is a terrifically bad name." van der Lee (2024) — "it's still a commonly used pattern in SwiftUI," then concedes the failure mode: "how it's being used isn't always consistent and results more in a View-ViewModel pattern."

**Which position dominates (INFERRED, with caveats stated).** Among sources that *argue about architecture as their subject*, the MV position dominates 2024–2026, and it is what Apple's docs and sample embody. MVVM persists mainly as **tutorial and book scaffolding**. Two honest caveats: the sample is self-selecting (people content with MV don't write "why I still use MV" posts), and the MV camp is concentrated in two authors — thinner evidence than the post volume suggests.

**Version gates for the machinery either camp needs** (from `.swiftinterface`): `@Observable`/Observation **14.0**; `@Bindable` **14.0**; `.environment(_ object:)` for Observable **14.0**; `@State`/`@Binding`/`@Environment` **10.15**; `@StateObject` **11.0**; `ObservableObject`/`@Published` **10.15**.

### TCA — recommend against, on its maintainers' own criteria

**Current version 1.26.1** (2026-07-21), 14,883 stars. **Minimum macOS 13.0** — higher than `swift-dependencies` (10.15). Complete Swift 6 language-mode support since **1.15.0** (2024-09-12).

**When the maintainers say not to use it**, verbatim from their FAQ: "We do not recommend people use TCA when they are first learning Swift or SwiftUI." And: "**We also don't think TCA really shines when building simple 'reader' apps that mostly load JSON from the network and display it.** Such apps don't tend to have much in the way of nuanced logic or complex side effects." And: "it can be fine to start a project with vanilla SwiftUI… and then transition to TCA later if there is a need."

**When it pays:** value-type observation ("TCA apps are allowed to use Swift's observation tools with value types, whereas vanilla SwiftUI is limited to only reference types"); exhaustive testing including effect feedback; previewability from controlled dependencies.

**SpectroCapture is close to the shape they name as a poor fit** — import an inventory, scan, write rows. Its genuinely nuanced part (the device state machine) is one component, not the whole app. **Recommend against.**

*Gap:* **TCA's own build/compile-time cost is not documented by its maintainers** — `Performance.md` covers runtime only, and there is no `CHANGELOG.md`. **Anyone citing a TCA compile-time number is citing something not findable at source.** **(c)** — follow-up harness specified in the agent report.

### Evidence from shipped apps — a genuine (a)

**UNANSWERED — (a) no literature exists.** There is no published study, longitudinal report, or controlled comparison of architectural patterns in shipped macOS SwiftUI applications. **Not "I couldn't find one" — the genre does not exist.** What is published is single-author before/after narratives and repository *structure*, which reveals module shape, not pattern durability.

**The observational evidence that does exist** (all verified via `gh api`, 2026-08-24):

| App | ★ | Structural shape |
|---|---|---|
| exelban/stats | 41,407 | `Kit/` + `Modules/<subsystem>/` — one independently-built unit per monitored subsystem, each with its own `main/popup/widget/settings.swift` + `Info.plist` |
| jordanbaird/Ice | 29,376 | Single app target, **no `Package.swift` at all**, folder-per-feature |
| p0deje/Maccy | 21,319 | Single target; `Views/` + `Observables/` alongside `AppDelegate.swift` and a `FloatingPanel` NSPanel subclass — a genuine hybrid |
| MrKai77/Loop | 11,434 | 3 targets: app + Dock-tile plugin + updater helper |
| TheBoredTeam/boring.notch | 10,455 | App + XPC helper, private-API adapter isolated |
| insidegui/WWDC | 8,760 | Local SPM `Packages/ConfCore` + `Packages/Transcripts` via `.package(path:)` |
| **insidegui/VirtualBuddy** | 8,512 | **7 own frameworks** — the closest analogue to SpectroCapture |
| buresdv/Cork | 4,643 | Tuist-generated modules, macOS 14.0 target, **FactoryKit** for DI |

**What this supports:** shipped macOS SwiftUI apps of comparable scope span *one flat target* to *seven frameworks*, and **the split is driven by process/bundle boundaries — helper tools, XPC services, plugins, Dock tiles — far more often than by feature count.** Every multi-target app above has a non-architectural reason for at least one target. **It does not support any claim about which pattern survives growth.**

**Disqualified, do not cite:** Whisky (archived), NetNewsWire (AppKit), gao-sun/eul (stale), wulkano/Kap (Electron), `Cindori/AppWizard` (404). **And an adjacent second-hand item:** Azam cites an NSSpain talk on rewriting SoundCloud in SwiftUI as a real-app postmortem; the agent could not locate it. **Do not cite it as a source.**

### Swift 6 interaction

**The view layer is isolated for free, in every pattern.** From `SwiftUICore.swiftinterface`: `View`, `ViewModifier`, `App`, and `Scene` are all `@preconcurrency @MainActor` at the protocol level. No pattern changes this and none earns credit for it.

**`@Observable` model types are NOT isolated** — `Observation.swiftinterface` declares a bare `public protocol Observable {}` with no actor annotation. **The isolation decision on your model classes is yours, and it is the same decision in MV, MVVM, and TCA.** Apple's own answer in its sample: `@Observable @MainActor class ModelData`.

**The documented SwiftUI-specific `Sendable` trap is on `Binding`:** "A binding conforms to `Sendable` only if its wrapped value type also conforms… **SwiftUI will issue a warning at runtime if it detects a binding being used in a way that may compromise data safety.**"

**Is any pattern materially harder? No, and the question is mis-aimed.** The `Sendable` cost is paid at the boundary where mutable reference-type domain state leaves the main actor. **MVVM's extra layer is the cheapest thing in the system to make clean** — one annotation. For SpectroCapture the expensive boundaries are the vendor SDK's callback thread and the SQLite writer, both of which exist regardless of pattern.

### UIKit-era habits — documented failure modes

| Habit | Documented consequence | Symptom |
|---|---|---|
| One fat VM per screen exposing helper methods views call | "Observation tracks changes to any observable property that appears in the **execution scope** of a view's `body`" — a row calling a method that internally reads a whole array makes **every row** depend on the array | Cause & Effect Graph shows one `@Observable` node fanning out to N body updates |
| `ObservableObject` + `@Published` per screen | "a view updates when **any** published property changes, even if the view doesn't read the property that changes" | Whole-screen redraw on an unrelated field change |
| Delegate plumbing translated to stored closures on views | "**Avoid storing closures in views.**… If the closure captures `self`… SwiftUI recalculates the closure's result whenever any of the view's properties changes" | Views update on properties they visibly don't read |
| Business logic executed during layout | "avoid performing complex, long-running tasks in your `View` initializer and these methods: `body`, `onAppear`, `onChanged`" | Long View Body Updates lane shows orange/red |
| Wrapping a list in a VM that filters at render time | "the inline filter here is linear over the collection… **It's better to move it out to the model**" | List cost slows superlinearly with row count |
| `AnyView` / conditional rows | "the number of views is now completely unknown… **All rows must be created**" | Scroll and load cost proportional to *total* rows, not visible rows |

**The VIPER half is (a) — no literature exists.** Searched Apple's docs tree, the TCA/`swift-dependencies` corpora, and five author sites: **no source documents VIPER-specific failure modes in SwiftUI.** The 2024–2026 debate has consolidated to MV vs MVVM vs unidirectional-store. **Any claim about "VIPER in SwiftUI symptoms" in a research deliverable is being invented.**

---

## 2. Project Structure and Module Boundaries

**Apple documents exactly one of the three approaches.** *Organizing your code with local packages* — note `metadata.platforms` is **`null`**, so there is no macOS version and anyone giving one is inventing it. Apple's stated benefits, verbatim: "**Simplify maintenance, promote modularity, and encourage reuse.**" Its one heuristic: "identify code that's a good candidate for modularization; for example, networking logic, source files that contain utilities." **That is the entirety of Apple's guidance — nothing about boundaries, dependency direction, feature modules, or costs.** Apple does not document the single-target or multi-project-workspace approaches at all.

**A workspace of multiple `.xcodeproj` projects has no current macOS SwiftUI open-source exemplar.** The apps needing more than one target use targets or local packages, not sibling projects. **State that plainly rather than presenting the three as equally attested.**

**The payoff point that is *documented* is not a screen count — it is a structural need:** a boundary you want the compiler to enforce, or a unit you want to test/build without the app.

**Documented costs, verified at source:**

| Cost | Evidence | Status |
|---|---|---|
| `Bundle.module` across a package boundary **crashes SwiftUI Previews** (`unable to find bundle named Theme_Theme`) | Swift Forums 41736 | Verified; **Xcode 12-era (2020)** |
| Same failure in a **test target** (`unable to find bundle named UILibrary_UILibrary`) | Swift Forums 43974 | Verified; **Xcode 12-era (2021)** |
| Same target reused across multiple products **breaks Previews** | SwiftPM #7323 | Verified, closed `not_planned` |
| Resource bundle not found for `.dynamic` library products | SwiftPM #6048 | Verified, **open** |
| `Bundle.module` **fails on macOS when launched via a symlink** (e.g. a Homebrew binary); works on Linux | SwiftPM #8510 | Verified, **open** (2025) — the most macOS-specific of the set |
| Module test targets don't surface as schemes in the app workspace | manu.show (2025) | — |

**No fetchable source quantifies the build-time delta.** The widely-repeated "Airbnb: p90 build time under 1 minute with 1000+ modules" figure surfaced **only in search-engine AI summaries, never in a primary page. Do not cite it.**

**Xcode indexing degradation is (a) — no literature exists.** Searched `forums.swift.org` and the SwiftPM tracker; no on-point thread. **This is folklore in this corpus. Say "not documented," not "known issue."**

**Feature-module boundaries — the negative finding matters more than the positive.** **There is no established Swift convention.** Apple documents none. What exists is three enforcement mechanisms of very different maturity: **SwiftPM's own model** (a target can only import a declared dependency; cycles are rejected — every local-package project gets this free); **Tuist's graph validation** (prevents cycles, does *not* stop a `Shared` module accreting); and **one layer-import linter**, `solid-like-a-rock`, at **29 stars and v0.9.0** — promising, but **do not represent it as established practice.**

**The shipped proof of a one-directional graph:** kickstarter/ios-oss — `Library/Package.swift` declares `.package(path: "../KsApi")`, `../KDS`, `../Experimentation`, `../ServerDrivenUI`, while `KsApi/Package.swift` depends on `../GraphAPI` and four external packages **and on nothing above it.** Verifiable in thirty seconds; the strongest convention evidence in this area.

**How a single developer prevents a `Core` dumping ground: don't create one.** A `Core`/`Common`/`Shared` module has no cohesion criterion, so nothing can be argued out of it. **Name modules after what they own** (`SpectroDevice`, `MeasurementStore`) so "does this belong here?" has an answer. Cork does exactly this — and notably **still has a `Shared`**, which is the empirical fact rather than the ideal.

### The device-abstraction module — the highest-value finding in these four areas

**LoopKit is the documented reference implementation.** Open-source framework for closed-loop insulin delivery: the "device" is a physical insulin pump or CGM, the app is useless without one, and contributors mostly don't have one. **That is SpectroCapture's problem, shipped.**

```swift
.target(name: "LoopKit",        dependencies: [],                                         path: "LoopKit"),
.target(name: "LoopKitUI",      dependencies: ["LoopKit", "SwiftCharts"],                 path: "LoopKitUI"),
.target(name: "LoopTestingKit", dependencies: ["LoopKit"],                                path: "LoopTestingKit"),
.target(name: "MockKit",        dependencies: ["LoopKit","LoopKitUI","LoopTestingKit"],   path: "MockKit"),
.target(name: "MockKitUI",      dependencies: ["MockKit","LoopKit","LoopKitUI"],          path: "MockKitUI"),
```

**Three tiers, and the middle one is the insight.** `LoopKit` holds the real contract; `LoopTestingKit` holds a **contract extension for simulation only** (`TestingDeviceManager` with `acceptDefaultsAndSkipOnboarding()` and `trigger(action:)`; `TestingPumpManager` with `injectPumpEvents(_:)`); `MockKit` holds the fakes; `MockKitUI` gives the simulator its own settings UI.

**The mock is presented to the user as a first-class selectable device, not a debug flag** — its `localizedTitle` is *"Pump Simulator."*

**Two caveats verified and reported, because they are exactly what gets glossed:** LoopKit's `Package.swift` declares `platforms: [.iOS("15.0")]` — **no macOS at all**; and it carries a maintainer banner, `// *************** Not complete yet, do not expect this to work! ***********************`, because the shipping build uses the Xcode project. **Cite LoopKit for its protocol/target *shape*, not as a working macOS SPM package.**

**The generic recipe** comes from `swift-dependencies`: "It is common for the interface of a dependency to be super lightweight and compile quickly… but for the 'live' implementation to be heavyweight… In such cases it is recommended to put the interface and live implementation in separate modules."

**A third example without the "Mock" naming:** PureSwift/GATT declares `public protocol CentralManager` with two implementations side by side — `GATTCentral` (pure-Swift, hardware-free) and `DarwinGATT`'s `DarwinCentral` (real CoreBluetooth). **No target is named "Mock."**

**The architecture that follows:**

```
SpectroDevice        — protocol + value types + errors + testValue/previewValue.  NO vendor SDK.
SpectroDeviceMock    — a shipped, user-selectable "Simulated Spectrophotometer."  NO vendor SDK.
SpectroDeviceLive    — the only module that imports the vendor SDK package.
SpectroCapture (app) — depends on Device + Mock; links Live at the composition root only.
```

**Because SwiftPM enforces that a target can only import a declared dependency, it becomes a compile error for a feature to reach the vendor SDK.** That is a stronger guarantee than a convention, and **it is the strongest form the open-source constraint can take.** A contributor without the SDK builds Device + Mock + the app and gets a running application.

**Resources — the cheapest correct posture is to keep all resources in the app target and the local packages resource-free.** That sidesteps every row of the `Bundle.module` table above at essentially zero cost for a single-developer, English-only, macOS-only app. *(Normative mechanics: SE-0271 establishes `Bundle.module`; SE-0278 adds `defaultLocalization`, which "becomes mandatory when localized resources are present.")*

**Currency caveat:** the two Preview/test failures date from **Xcode 12 (2020–2021)**. Whether they reproduce under Xcode 26.6 is **(c)** — a two-package harness settles it.

---

## 3. State Management and Data Flow

### Version gates, read from MacOSX26.5.sdk

| API | Module | Min macOS |
|---|---|---|
| `Observable`, `@Observable`, `@ObservationIgnored`, `withObservationTracking` | Observation | **14.0** |
| **`Observations<Element, Failure>`** (Observation as `AsyncSequence`) | Observation | **26.0** |
| `ObservableObject`, `@Published` | Combine | **10.15** |
| `@ObservedObject`, `@EnvironmentObject` | SwiftUICore | **10.15** |
| `@StateObject` | SwiftUICore | **11.0** |
| `@Bindable`, `.environment(_ object:)` | SwiftUICore | **14.0** |

***Orchestrator-verified:* the DocC JSON for `observation` returns `[('macOS','14.0'), ('iOS','17.0'), ('visionOS','1.0')]`.*

**`Observations` at macOS 26.0 is a finding worth flagging.** SE-0475 gives an `AsyncSequence` of model changes "starting transactions at the first `willSet` and then emitting a value upon that transaction end at the first point of consistency." **If SpectroCapture targets macOS 14 or 15, this is unavailable** — directly relevant, because the capture brief already settled on `AsyncStream` for the hardware bridge and `Observations` would be the symmetric tool for the *model* side.

**Documented behavioural differences.** Apple lists three benefits, and **the first is the least-cited and often the most consequential: "Tracking optionals and collections of objects, which isn't possible when using `ObservableObject`."** For a measurement app whose central model is a growing collection, **that is decisive on its own.**

**Is `ObservableObject` deprecated? No — and the agent checked rather than assumed.** It grepped `Combine.swiftinterface` and `SwiftUICore.swiftinterface` for deprecation attributes on `ObservableObject`, `Published`, `StateObject`, `ObservedObject`, `EnvironmentObject`. **None carries `@available(*, deprecated)`.** Apple actively supports mixing: "Your app can mix data model types that use different observation systems."

**It is deprecated *in practice*** — no documented reason to reach for it in a new macOS-14+ app. **Do not write that Apple deprecated it; that is false and checkable.**

### Where application state should live

**Apple documents a root store, explicitly and by example:** `@State private var library = Library()` on the `App` struct, injected with `.environment(library)`.

**But Apple also documents the root store's failure mode at scale, in the same app** (WWDC25 306): "Because each view accessed the favorites array, even though it was indirectly… all of the views are marked as outdated, and their bodies run again. But that's not ideal, because the only view I actually changed was view number three."

**So the documented answer is not one of the three options — it is a division of labour:**

| Concern | Where it lives |
|---|---|
| **Ownership / lifetime** | one root `@Observable` in `@State` on the `App` or `Scene` |
| **Invalidation granularity** | per-item / per-feature `@Observable` objects *stored inside* the root store |
| **Reach** | `.environment(object)` + `@Environment(Type.self)` |
| **Services** (device, database) | separate `Environment` values, **not fields on the store** (INFERRED) |

**Two documented costs of the `Environment` route:**

1. **Absence is a crash, not a nil.** "If a view attempts to retrieve an object using its type and that object isn't in the environment, **SwiftUI throws exception**." The escape is the optional form. **This bites hardest in previews and tests**, where the injecting ancestor is often absent.
2. **Every reader pays on every environment change, even when its body doesn't run.** "even in cases where a view's body doesn't need to run… there is still a cost associated with checking for updates… The time spent can add up quickly if your app has a lot of views reading from the environment."

**Pass-through is free, which is what makes the root store viable:** "If a view doesn't have any dependencies, SwiftUI doesn't update the view when data changes. This approach allows an observable model data object to pass through multiple layers of a view hierarchy without each intermediate view forming a dependency."

**For SpectroCapture:** one `@Observable @MainActor` app model in `@State` on the `App`, in the environment, owning identity (current inventory, selection, session). **A per-measurement-row `@Observable`** the moment the collection view is long — that is exactly the WWDC25 306 defect shape. **The device and the database as separate `Environment` values, not fields on the app model** — merging them makes the store a god object and forces every preview and test to construct one. **Nothing frequently-changing in the environment.**

### Property wrappers — semantics and pitfalls

| Wrapper | Min macOS | The pitfall that matters |
|---|---|---|
| `@State` | 10.15 | "**Declare state as private** to prevent setting it in a memberwise initializer, which can conflict with the storage management that SwiftUI provides." Scope: "local to a view and its subviews" |
| `@Binding` | 10.15 | `Sendable` only if the wrapped value is; runtime warning if misused across domains. For `Observable` types use `@Bindable`, not `@Binding` |
| `@Environment` | 10.15 (type-key form **14.0**) | Missing type-keyed object **throws**; every reader pays a check on every change |
| `@Bindable` | 14.0 | **Not a storage wrapper** — creates no source of truth. Reaching for it where `@State` is meant is the misuse |
| `@AppStorage` | 11.0 | Apple's reference page has **no Overview section** — semantics are thinner than the wrapper's popularity implies |
| `@SceneStorage` | 11.0 | **Three documented, all easy to violate:** no guarantee when/how often it persists; keep it lightweight; and **"If the `Scene` is explicitly destroyed (e.g. the window is closed on macOS), the data is also destroyed. Do not use `SceneStorage` with sensitive data."** |

**Most commonly misused (INFERRED, each anchored to a documented rule):** `@SceneStorage` on macOS is the most dangerous — **the destroy-on-window-close rule is stated once, in a sentence, and contradicts most developers' mental model of "restoration."** `@Environment` for services is the most common over-reach, because crash-on-absent makes it a poor DI mechanism. `@State` without `private`. And the `@ObservedObject` reflex on `@Observable` types.

### Invalidation granularity — beyond the settled baseline

**The governing rule:** "Observation tracks changes to any observable property that appears in the **execution scope** of a view's `body`." *Execution scope, not lexical text* — that single word is why the over-broad-dependency trap exists.

**Computed properties are transparent to observation, and it works** — "Observation also supports tracking of computed properties when the computed property makes use of an observable property." **A computed property is not a granularity boundary — it is the opposite.** Deriving a value in a computed property does not protect you; it exposes you.

**Collections — more precise than commonly stated.** "When a view forms a dependency on a collection of objects, **of any collection type**, the view tracks changes made to the collection itself" — insert, delete, move, replace. **And the crucial second half:** "instead of `LibraryView` depending on a book's `title`, each `Text` item of the list depends on `title`. **Any changes to a `title` updates only the individual `Text`… and not the others**" — because a `List`'s content closure is `@escaping`.

**So: container mutation invalidates the container view; element-property mutation invalidates only the element's view** — provided the element property is read inside the row closure, not the parent's `body`.

**Four further documented rules:** storage without reading is **not** free ("a view that stores a reference… updates if the reference changes… **not because the object is observable**" — a `View` value change, a different mechanism); pass-through views form no dependency; dependencies **chain through nested objects** (`book.author.name`); and tracking does not require storage — a global read in `body` is tracked identically.

**Techniques against over-broad invalidation:** read the narrowest property; extract subviews (**with Apple's caveat: "just be careful with very large structs. Not every dependency deserves to be scoped like this"**); per-item `@Observable` view models; `@ObservationIgnored`; cache derived values in the model; keep frequently-changing values out of `Environment`; don't store closures in views; constant view count per `ForEach` element.

**Two macOS-specific notes:** "In macOS Sonoma and iOS 17, SwiftUI has a number of optimizations under the hood for cases like filtering and scrolling" — **so measurements taken on macOS 13 do not generalize forward.** And `Table` row identity **changed in macOS Sonoma / iOS 17** — previously by `TableRow` value, now by the `ForEach` element's ID.

### Separating ephemeral UI state from domain state — and an uncomfortable finding

**The documented tiering:** `@State` for "transient UI state locally within a view"; `@SceneStorage` for window-scoped; `@AppStorage` for preferences; your SQLite store for domain. Apple's normative rule: "**Move business logic and other non-UI work out of views to model types** because SwiftUI recreates views, and recalculates view bodies, frequently."

**How a shipped SQLite app does it** — GRDB's demo documents a four-piece separation: `AppDatabase` ("grants database access… `AppDatabase` is **tested**"); `Persistence.swift` ("one database on disk for the application, and **in-memory databases for SwiftUI previews**"); `PlayerListModel` (an `@Observable` that observes the database, also tested); and the App feeding the database through the environment. **With a caveat worth honouring: "This demo app is not a project template. Do not copy it as a starting point."**

> **The uncomfortable finding: Apple's own reference sample entangles them.** `ModelData` holds domain state (`landmarks`, `favoritesCollection`, `userCollections`, `landmarksById`) *side by side with* pure UI state — `selectedLandmark`, `isLandmarkInspectorPresented`, `searchString`, `path: NavigationPath` **with a `didSet` that dismisses the inspector**, and `windowSize: CGSize`.
>
> **So the honest answer to "how do shipped apps keep the two from entangling" is: frequently, they don't** — and the mechanism is visible in that `didSet`: navigation and presentation state coupled inside a class whose other half is domain data.

**Discipline for SpectroCapture, as testable rules:** a type decoded from SQLite carries **no UI flags** (no `isSelected`, no `isExpanded` on a `Measurement`; selection is a `Set<Measurement.ID>` in `@State`); UI state lives in the view that owns it, promoted only on demand; `@SceneStorage` only for what must survive relaunch but is **not** the user's data; **the database-access type is not the app model** (GRDB's `AppDatabase` shape is what makes the preview/in-memory swap a one-line change); and **the test for entanglement is mechanical — can you construct the persistent type in a test with no SwiftUI import?**

### Profiling — the inherited gap, closed

**The SwiftUI instrument requires Xcode 26 on the host and macOS 26 on the profiled machine.** A hard gate: if SpectroCapture deploys to macOS 14/15, you can still profile it, but **only on a macOS 26 machine.**

Lanes: **Update Groups** ("If CPU usage is spiking during a time when this lane is empty, you'll know that your problem likely lies somewhere outside of SwiftUI"), **Long View Body Updates**, **Long Representable Updates**, **Other Long Updates**, coloured orange/red. Plus the **Cause & Effect Graph** — blue nodes are your code, gray are the system, and clicking a state-change node shows "a backtrace of where the value was updated."

**Why breakpoints don't work here:** "**Because SwiftUI is declarative, you can't use the backtrace to understand why your view is updating.**" A SwiftUI backtrace is "several recursive updates to stuff inside SwiftUI, separated by frames inside something called AttributeGraph. None of this tells me why my view specifically needs to update." **The single most useful thing to know before spending an afternoon in the debugger.**

**`Self._printChanges()` versions, from the SDK:** `View._printChanges()` **12.0**; `View._logChanges()` **14.1**. Apple's characterization: "a debugging-only facility that gives a **best-effort** explanation," and the warning: "never guaranteed to always exist and may even be removed… **never submit a call to this method to the app store**… has a runtime performance impact." Two usage facts: callable from LLDB via `expression` without editing code, and **`@Self` in the output means the view's own value changed**, not a dynamic property.

**Apple publishes no numeric budget for a view body.** The criterion is **the frame deadline**, and Instruments' orange/red mapping is explicitly conditional: "Whether these updates actually result in any hangs or hitches **can depend on device conditions**." **Any specific millisecond threshold for SpectroCapture is (c).**

**And the Release-build caveat, restated:** Command-I "compiles the app in Release mode." Sibling research found a case where a catastrophic Debug baseline vanished in Release. **Any measurement taken in Debug is not evidence.**

---

## 4. Dependency Injection and Testability Seams

| Approach | Version / min macOS | Strengths | Costs |
|---|---|---|---|
| **`Environment`** | `@Environment` **10.15**; `@Entry` **10.15**; type-keyed **14.0** | No dependency; distributed registration; automatic subtree scoping | Missing type-keyed object **throws**; per-reader cost on every change; **only works inside the view hierarchy** — `@Dependency` "behaves much like SwiftUI's `@Environment`… **but it works outside of views**" |
| **Initializer injection** | — | No machinery; the compiler is the check | Threading through N intermediate views. Apple: "convenient when you have a **shallow** view hierarchy… However, you usually don't know if a view needs to pass the object to subviews" |
| **`swift-dependencies`** | **1.17.0** (2026-08-24), **macOS 10.15** | `liveValue`/`previewValue`/`testValue` resolve by context. **Fails the test if a live dependency is touched from a test.** Documented interface/implementation split | `@Dependency` in an `@Observable` needs `@ObservationIgnored`. The `.dependencies` preview trait requires **macOS 15** |
| **Factory** | **3.3.2** (2026-07-15), **macOS 10.15** | "compile-time safe; a factory for a given type must exist or the code simply will not compile." Contexts for preview/test/UITest. Under 1,000 lines. **In production in a macOS SwiftUI app (Cork)** | A documented Swift 6.2 limitation in the maintainer's own words about global MainActors with nonisolated service classes |
| **Swinject** | **2.10.0** (2025-09-01), 6,707★ | — | **Not evaluated.** Version and currency verified; no documentation fetched, **so no characterization is made.** **(b)** — the docs exist and simply were not read |

**Recommendation:** `Environment` + initializer injection covers the whole application, **with one exception — the device seam**, where you need *automatic* selection across app/preview/test and where getting it wrong means a contributor's Xcode tries to talk to hardware that isn't there. `swift-dependencies` is the smallest thing that solves exactly that, at macOS 10.15, and it is what the interface/implementation split is built around. **A general-purpose DI container for the whole app is over-engineering at this scale** — and the FAQ authors of the most complex option in the table say the same about their own library.

### Defining the abstraction

`swift-dependencies` documents two styles and does not insist. **Protocol style, with *three* conformances, not two:**

```swift
struct LiveAudioPlayer: AudioPlayer { let audioEngine: AVAudioEngine }
struct MockAudioPlayer: AudioPlayer { }
struct UnimplementedAudioPlayer: AudioPlayer {
  func loop(url: URL) async throws { reportIssue("AudioPlayer.loop is unimplemented") }
}
private enum AudioPlayerKey: DependencyKey {
  static let liveValue:    any AudioPlayer = LiveAudioPlayer()
  static let previewValue: any AudioPlayer = MockAudioPlayer()
  static let testValue:    any AudioPlayer = UnimplementedAudioPlayer()
}
```

**The third conformance is the one usually omitted and the one that does the most work** — an `Unimplemented` variant that fails loudly on any endpoint a test did not deliberately stub. **A mock that answers a question the test never meant to ask is dishonest in a way contract tests will not catch.**

**Struct-of-closures style** trades the protocol for per-endpoint override. For a device with a small stable surface (connect, calibrate, measure, disconnect), the protocol style is clearer; for stubbing one method at a time, the struct wins.

**Selection.** `swift-dependencies` resolves automatically per context, **and fails hard in tests**: "🛑 A dependency has no test implementation, but was accessed from a test context… Dependencies registered with the library are **not allowed to use their default, live implementations when run from tests**." Opting in is explicit.

**LoopKit's selection model is the one to copy:** `MockPumpManager` declares `pluginIdentifier = "MockPumpManager"` and `localizedTitle = "Pump Simulator"` — **a shipped, user-selectable device in the same picker as real hardware, not a `#if DEBUG` branch.** Three consequences: a contributor without hardware gets a fully functional app; CI exercises the same code path a user does; **and the mock is continuously exercised through the real UI, which is the honesty mechanism.**

**What Apple's own sample does: nothing.** Its `#Preview` constructs the real `ModelData`. **That is the honest baseline for small-scale Apple code, and it works precisely because `ModelData` has no device or network dependency. It would not work for SpectroCapture** — and knowing exactly why is the argument for the seam.

### Keeping the mock honest

**The named property and mechanism come from the general software-engineering literature.** *Software Engineering at Google*, ch. 13:

> "**Fidelity** refers to how closely the behavior of a test double resembles the behavior of the real implementation… But perfect fidelity might not be feasible… **Unit tests that use test doubles often need to be supplemented by larger-scope tests that exercise the real implementation.**"
> "A fake without tests might initially provide realistic behavior, but **without tests, this behavior can diverge over time as the real implementation evolves**. One approach… involves writing tests against the API's public interface and **running those tests against both the real implementation and the fake (these are known as contract tests)**. The tests that run against the real implementation will likely be slower, but **their downside is minimized because they need to be run only by the owners of the fake**."

**That last clause is the answer to the CI constraint verbatim: one suite, two subjects, different cadences.** CI runs it against the mock on every PR; the maintainer with the spectrophotometer runs the identical suite against hardware.

**Fowler (2011/2018) supplies operational details unusually well-matched to expensive hardware runs:** run contract tests on "**the rhythm of changes to the external service**… Often running just once a day is plenty"; "**A failure in a contract test shouldn't necessarily break the build in the same way that a normal test failure would.** It should, however, trigger a task to get things consistent again"; and "the contract test needs to check that the **format** is the same, even if the actual data has changed."

**Google's own retrospective on the alternative:** "though these tests were easy to write, we suffered greatly given that they required constant effort to maintain while rarely finding bugs. **The pendulum at Google has now begun swinging in the other direction, with many engineers avoiding mocking frameworks in favor of writing more realistic tests.**" **A hand-written stateful fake beats a per-test mocking framework here** — and LoopKit's `MockPumpManager` + `MockPumpManagerState` is exactly that.

**Four honesty mechanisms for a spectrophotometer, in descending value per unit effort:**

1. **A single conformance suite, two subjects** — write it against the `SpectroDevice` protocol, parameterize the subject. CI runs mock-only; a maintainer-only scheme runs hardware.
2. **Replay fixtures captured from real hardware** — record real responses including calibration sequences and malformed frames, and have the mock replay them. **This is Klipper's model** (CI replays recorded `test/klippy/*.test` fixtures rather than driving printers) and **the only mechanism that gives the mock *real* data without real hardware.**
3. **Error-case parity as an enumerated obligation** — make the error type an `enum` in the interface module so `switch` exhaustiveness makes an unhandled new case a **compile error** in the mock.
4. **Timing parity as an envelope, not a constant** — a mock that returns instantly hides every race in the capture UI. Give it a configurable latency defaulting to the real device's measured envelope.

**The strongest structural mechanism remains the selection model: if the mock is a shipped device that you and every contributor use daily, drift shows up as a bug report rather than a silent green suite.**

### Previews and cross-module injection

**Apple's native mechanisms:** `#Preview` **10.15**; `@Previewable` **14.0**; and **`PreviewModifier` — macOS 15.0**, the one that natively answers this: "Conforming types can define **shared contexts that will be cached by the preview system, then reused across participating previews**." **Its macOS 15 floor is the catch.**

**`swift-dependencies`' `previewValue` is used automatically in previews — zero code at any call site.** Its rationale is precisely SpectroCapture's: "many of Apple's frameworks do not work in previews, such as Core Location, and so it will be hard to interact with your feature in previews if it touches those frameworks."

**Two documented constraints, and the second couples directly to the module split:** the `.dependencies` preview trait requires macOS 15 (below that use `prepareDependencies`); and **"The `previewValue` implementation must be defined in the same module as the `TestDependencyKey` conformance… it must be defined [in] the interface module, not the implementation module."**

**GRDB's demo offers the non-DI-library answer:** in-memory databases for previews. **For a SQLite app, an in-memory store is often a better preview dependency than a mock, because it is the *real* code with a different file.**

**Cross-module injection without cycles or a god container.** The direction is `Feature → Interface ← Implementation`; nothing points back at a feature, and **SwiftPM enforces this structurally.** And **registration is distributed, not centralized** — each interface module registers its own key, so nothing is enumerated in one app-owned file that must import everything. **That is the property distinguishing it from a classic container.**

**The one non-obvious constraint, restated because it bites:** if you put your simulated spectrophotometer's `previewValue` next to the live SDK-linking implementation, **previews in feature modules that don't link the implementation will fall back to `liveValue`** — which for SpectroCapture means Xcode tries to talk to hardware.

---

## 5. App Lifecycle, Windowing, and Scenes

**Menu-bar limitations are worse than the API surface suggests.** Reichelt documents two the API doesn't reveal:

> "**The problem comes when you want to communicate back to the SwiftUI views from the menubar.** How can you direct your menubar commands to the correct destination? **AppKit uses the responder chain**, so it effectively broadcasts any menubar message until something handles it… **SwiftUI doesn't work like this.**"
> "**Unfortunately, you can't disable an entire `CommandMenu`.**"
> "My next attempt was to step into AppKit and have `NSApp` send a selector through the responder chain. This looked like it should work, but **I could never get it to, and it looked clunky.**"

Also: "**I was surprised to find that `Link` does not work in a menu.**" And on removing default items: replacing with `EmptyView()` "**is probably not a great idea as regards the Human Interface Guidelines** but it is an option."

**Practical consequence: the "menu command → active window" routing problem is the real work in a macOS menu bar.** `@FocusedValue`/`focusedSceneValue` is what you use **precisely because the responder chain isn't available to you** — and Reichelt's account is evidence the AppKit route is a dead end, not a fallback. **Budget for `@FocusedValue` plumbing per command; don't plan on `NSApp.sendAction`.**

*Gap:* **no practitioner article specifically on macOS termination interception, dock menus, or reopen handling in SwiftUI** was found across five author sites. The "what still requires `NSApplicationDelegateAdaptor`" list therefore rests on Apple-documented facts plus a SwiftUI symbol-index enumeration — the strongest available grounding, **but no community corroboration is claimed for it.**

---

## 6. Navigation Architecture and State Restoration

**Four macOS-only `NavigationSplitView` defects, dated and reproduced** *(orchestrator-verified: "out of row range" and "15.1" appear in the source)*:

> "there is a **long-standing bug where a hidden sidebar cannot be dragged back into view**."
> "**the sidebar is often hidden on app launch.** I tried giving `NavigationSplitView` its optional `columnVisibility` parameter, but no setting made it appear consistently."
> "there is a bug where **you cannot unwrap a conditional and show a view based on that.** One workaround is to wrap the entire detail in a `ZStack`."

> ### ⚠️ The macOS 15.1 crash matches SpectroCapture's exact interaction
>
> "**Warning: there's a bug right now that crashes the app in macOS 15.1.** If you scroll down the list in the sidebar and then **select a status that is more than 8 or 9 rows below the previous selection**, the app crashes with `Row index -1 out of row range (numberOfRows: 60)`. I've tested this with Xcode 16.0 beta 6 and Xcode 16.1 and they both act the same. But on a computer running macOS 15.0 beta 7… it works perfectly. **So this is a bug in macOS 15.1.**"
>
> **That is a `List` selection crash on a long-jump selection change in a sidebar — precisely the interaction of scanning heads-down through an inventory with the selection jumping ahead.**

**And the first defect is a hard argument for keeping `SidebarCommands()` and the default sidebar toggle rather than removing them via `toolbar(removing:)`** — because a hidden sidebar cannot be recovered by dragging.

**Two concrete state-restoration defects:**

> "The `@SceneStorage` property wrapper restores the state of my search scope but it's **not triggering a correctly initialized fetch request**… **The `onChange` action is not triggered when the `SceneStorage` property wrapper initializes.**" — fix: pass the restored value through an initializer rather than relying on `onChange`.

> **The multi-window `@State` bug:** "In the root view, dynamic data created through `@State` or similar mechanisms are **entirely the same in every new window**… **All windows are using the same instance of the observable object**, which is unacceptable for applications that need to provide an independent state container for each window." **Fixed in macOS 14.5.** *(Orchestrator-verified: "macOS 14.5" and "same instance" appear in the source.)*

**Consequences.** Apple's WWDC22 restoration recipe wires `@SceneStorage` through a `.task { }` that seeds the model on appear and then iterates changes — **precisely the shape that sidesteps the `onChange`-doesn't-fire-on-init bug. Follow the WWDC recipe literally; do not substitute `onChange(of: sceneStorageValue)`.** And **macOS 14.5 is the practical floor for correct per-window `@State`** in a multi-window app.

**Routing state — comparative evidence absent, but the positions are now citable.** No instrumented comparison exists **(a)**. But the weight of recent practice is **"routing state should be data owned by the feature, not a separate Router/Coordinator object."**

- **Point-Free** put routing state *inside* each feature's state via optionals and enums: "when the value is non-nil the feature is presented, and when the value is nil it is dismissed. We like to call this style **'tree-based'**." An explicit argument that the router should be data, not a class.
- **Jabrayilov's own position moved with the framework.** 2019, on UIKit: "The one huge problem which I have with Coordinator pattern is **keeping it in sync with ViewController hierarchy**." 2022, once `NavigationStack` made navigation state-driven: "I love to keep my feature's navigation flow in a single place… the **Navigator pattern**." **What changed was not the pattern's value — it was that the framework started exposing navigation as data.**
- **Fatbobman** on imperative navigation calls: `dismiss()` and friends "**reduce a view's testability. They increase the difficulty of maintaining the project later on.**"

**Recommendation:** a per-scene `NavigationModel` (selection + path, `Codable` on IDs, no SwiftUI imports) *is* the router. **A separate `Router` class is over-engineering here.**

*Unresolved:* whether split-view column widths persist across launches — **(c)**, no source addressed it.

---

## 7. Concurrency Architecture under Swift 6

> ### ⚠️ The agent revised its own filed position here
>
> It first treated `SWIFT_DEFAULT_ACTOR_ISOLATION = MainActor` as consensus **because Apple's own Xcode 26.6 App template enables it** — then found a genuine community split and withdrew the claim. **That is Apple's position, not the community's.**

**Massicotte (2025), against** *(orchestrator-verified: both quotes appear at source)*:

> "Well I have made up my mind, at least for now. **We should not. I'm not sure we should even have the ability to do so.**"
> "when you finally encounter concurrency, and you almost certainly will, **a default of `MainActor` can make those encounters much more difficult to understand and address.**"
> "understanding *why* `MainActor` is needed is far simpler than understanding why or how to *remove* it."
> "You will never be able to work with one mode exclusively, because at the minimum, **Apple will not adopt this for their own libraries.**"

**Wals, for**, on grounds that also bear on background work:

> "while blocking the main thread is bad **we shouldn't be afraid to run code on the main thread.**"
> "It's often cheaper for a quick operation that started on the Main Actor to stay there than it is for that operation to be performed on a background thread and handing the result back to the Main Actor."

**Revised position for SpectroCapture:** enable it **on the app target only**, and leave it `nonisolated` on the device and persistence packages — which is what Apple's own guidance says ("Use this primarily for your main app module and any modules that are focused on UI interactions") and what Massicotte's objection argues for structurally. **His "you will never work with one mode exclusively" is the operative warning: the mixed-mode boundary is where the confusion lands, so put that boundary at a module edge you chose deliberately.**

*Note on sourcing:* the agent could **not** verify the literal build-setting key `SWIFT_DEFAULT_ACTOR_ISOLATION` from any online source — only the Xcode UI label "Default Actor Isolation." **Its on-disk `Swift.xcspec` retrieval is the stronger citation**, including the verbatim `Values = (nonisolated, MainActor)` mapping. Separately, SE-0466 *Control default actor isolation inference* is **Implemented in Swift 6.2**, and `swiftc -h` on this machine reports `-default-isolation MainActor|nonisolated`.

**Migration cost — practitioner reports:**

> "First I changed language version to 6… **I was shown 50 or so errors at first and I thought ok not bad. I sprinkled mainactor on all the classes to make the red errors go away. Then came 50 more errors… half a week later of repeating this process they finally built.**"
> "**Swift 6 mode enables runtime concurrency assertions which will crash any code that isn't completely correct from the concurrency runtime's perspective. This includes Apple's frameworks like Combine.**"
> "The main issue is the extensive use of `@MainActor`—since UIKit components are inherently MainActor isolated, **this causes a cascading effect where protocols and dependencies up the chain need to be marked as `@MainActor` as well.**"
> "**Sendable seems to propagate everywhere**… now all this services seems to be Sendable, and I've got some inheritance on the services. **Which makes Sendable impossible.**"
> "**Migrating to Swift 6, for a lot of apps, is going to be a very slow and lengthy process**… Not all of Apple's code is necessarily Swift 6 compatible or Swift 6 friendly."

**This reinforces rather than changes the conclusion: every one of these is a *migration* cost report. SpectroCapture is greenfield and pays none of it.** The `Sendable`-propagation-through-class-inheritance failure is exactly what GRDB's "replace record classes with structs" guidance prevents up front.

**`@concurrent` granularity — annotate the *smallest* unit:** "We made the **smallest unit of work possible** `@concurrent` to avoid introducing loads of concurrency where we don't need it… Only `decode` will run on the global executor, ensuring we're not blocking the main actor during our JSON decoding."

*Citation correction the agent made to itself:* `@concurrent`'s proposal is **SE-0461** *Run nonisolated async functions on the caller's actor by default* (Implemented, Swift 6.2; upcoming-feature flag `NonisolatedNonsendingByDefault`) — **not SE-0472**, which is a different, narrower proposal.

**On `Task.detached`** — a direct statement, though dated (2022, pre-Swift 6): "I have interpreted the documentation and previous WWDC videos as **discouraging the use of `Task.detached`**… The main downside… is **there's no API to manipulate them as a group.**"

**Testing async and actor-isolated code — an important limit on `confirmation` the agent corrected in itself:**

> "**We must call the `confirm` object the expected number of times before our closure returns.** This means that **it's not usable when you want to test code that's fully completion handler based.**"
> "Instead of a confirmation, we can have our test wait for a **continuation**… **When you're testing completion handler based code, I usually find that I will reach for this instead.**"

**Revised guidance:** use `confirmation(expectedCount:)` to assert **how many readings an `AsyncStream` bridge yields** during an operation you can `await`; use `withCheckedContinuation` for a single "did the SDK call back exactly once with this result" test. **Both in a `@Suite(.serialized)` if they touch a shared database file or the one physical instrument.**

---

## 8. Error Handling, Logging, and Diagnostics

**Typed throws ships in Swift 6.0** (SE-0413), syntax `func f() throws(MyError) -> T`. **A caveat the proposal itself records:** the closure/`do..catch` type-inference portion (`FullTypedThrows`) **did not ship in 6.0** and moved to Future Directions — so typed throws is narrower than the accepted proposal.

**Swift's own guidance is to mostly *not* use it**, and it is unusually blunt: "errors are usually propagated or rendered, but not exhaustively handled, so even with the addition of typed throws to Swift, **untyped `throws` is better for most scenarios**." It gives the exact anti-pattern for a hardware app: `public func loadBytes(from file: String) async throws(FileSystemError) -> [UInt8]` **"should use untyped throws,"** because pinning the error type "may hamper further evolution of this API."

**Recommendation:** untyped `throws` at every public/module boundary; **typed `throws` only inside the device package** where the whole error space is genuinely closed. **Do not build a central `AppError` god-enum** — every module would depend on it, inverting the dependency direction.

**Logging — redaction is the default, which is the right default for you.** `Logger`/`OSLogPrivacy` are **macOS 11.0+**. "When you include an interpolated string or custom object in your message, **the system redacts the value of that string or object by default.**"

> **The trap: numbers and Booleans are *not* redacted by default — only strings and objects are.** A measurement value or a serial number interpolated as an `Int` appears in the log **in the clear** unless marked.

| Level | Persisted to disk |
|---|---|
| Debug | **No** — memory only |
| Info | Only when collected with the `log` tool |
| Notice (default) / Error / Fault | Yes, up to a storage limit |

> ### ⚠️ An in-app "Export Diagnostics" button will not work on macOS
>
> `OSLogStore.local()`: "**The caller must be run by an admin account and have the `com.apple.logging.local-store` entitlement.**" **That entitlement is not claimable by a third-party Developer ID app.** The documented retrieval path is instructing the user to run `log collect --last 1h --output ~/Desktop/spectrocapture.logarchive`.
>
> **Corollary: because private data cannot be reliably un-redacted on a user's machine, anything you will actually need must be logged `.public` deliberately** (device model, transport, error code, timing) and anything user-identifying must stay `.private`/`.sensitive`. **Design the log line for the redacted reading.** `private(mask: .hash)` lets you correlate a redacted value across lines without disclosing it.

*Also:* the widely-cited `log config --mode "private_data:on"` recipe is **not backed by the current man page** — on macOS 26.6 the documented `--mode` values are only `level:` and `persist:`. **Treat blog posts asserting it as stale.**

**Surfacing errors vs logging silently** — the HIG states the boundary as a matching rule: "The most effective feedback tends to match the significance of the information to the way it's delivered." And is stricter still on alerts: "**Use alerts sparingly.**… **Avoid using an alert merely to provide information.** People don't appreciate an interruption from an alert that's informative, but not actionable." Plus: "**Avoid showing an alert when your app starts.**" And on wording: "**Avoid writing a title that doesn't convey useful information — like 'Error' or 'Error 329347 occurred'.**"

**Applied:** an instrument disconnect mid-scan is **status**, not an alert — a persistent in-context indicator. A failed migration or an about-to-overwrite action **is** an alert. Everything else goes to `OSLog` at `.error`/`.notice` and nowhere else. macOS alerts may add "a suppression checkbox and a Help button" — **directly relevant to a heads-down workflow where a recoverable hiccup must not modal-block on every occurrence.**

**Unrecoverable errors.** SQLite ships a documented recovery path — the `.recover` command **and a recovery API you can build into an application**. Detection via `quick_check`/`integrity_check`/`foreign_key_check`. **GRDB's README contains no mention of `corrupt`, `integrity`, or `recover`** — corruption handling is **not** a documented GRDB feature and you would be driving SQLite's pragmas yourself. *(Flagged as a documented absence, verified by grep of the retrieved README.)*

**Design:** `quick_check` on open and before any migration; a `VACUUM INTO` snapshot immediately before migrating; on `SQLITE_CORRUPT`, **refuse to write**, alert with a recovery offer, and drive `.recover` into a **new sidecar file rather than mutating the user's file in place.**

*Gap:* **no primary source establishes an open-source convention for a macOS support bundle** — only the mechanisms are documented. **(c)** — follow-up: write one `Logger` line per privacy option from a signed sandboxed test app, run `log collect` then `log show --style ndjson`, and record which values survive in the clear. **Repeat with `OSLogStore(scope: .currentProcessIdentifier)` to settle whether the in-process scope evades the `local()` entitlement gate** — Apple's page for that symbol carries **no descriptive text at all.**

---

## 9. Testing Strategy and CI Without Hardware

**Swift Testing is bundled with Swift 6.0 / Xcode 16.0** — "**You do not need to add it as a package dependency.**" It versions with the toolchain (`swift-6.3.2-RELEASE`). Supports `@Test`/`@Suite`, `#expect`/`#require`, parameterized tests, traits (`.enabled(if:)`, `.disabled()`, `.timeLimit(_:)`, `.serialized`, `.tags()`, `.bug()`), in-process parallelization by default, `Attachment.record(...)`, and `withKnownIssue()`.

**Xcode 26-era additions worth knowing:** `try Test.cancel("…")` for ending a *running* test early; and a **cross-library interoperability** feature making `XCTAssert*` failures inside shared helpers actually fail a Swift Testing test — **Apple's own example shows a helper whose `XCTAssertEqual` failure was previously silently swallowed. A false-green class worth knowing about if you mix.**

**Documented gaps, verified by omission:** no UI-testing topic on the landing page; the migration article never mentions `XCUIApplication`; `continueAfterFailure` has no equivalent (use `#require`); and — **a direct practical hit for a spectrophotometry app** — "The testing library doesn't provide an equivalent of `XCTAssertEqual(_:_:accuracy:)`. To compare two numeric values within a specified accuracy, use `isApproximatelyEqual()` from swift-numerics." **Plan on a swift-numerics dependency for comparing float spectra.**

**Recommended: yes, with XCTest retained for UI tests.**

### Testing SwiftUI views — the honest answer is unflattering to all three

**There is no first-party Apple API for this** — SwiftUI's documentation landing page has no Testing section, and `documentation/swiftui/testing.json` **404s.**

| | Genuinely proves | Main false-confidence mode |
|---|---|---|
| **ViewInspector 0.10.3** | The struct tree `body` produces contains element X; a button's action closure mutates state Y when invoked programmatically. **No render pass, no layout, no window server, no pixels** | Reading a green suite as "it renders correctly." **Compounded by documented blind spots** — its own tracking file lists as unsupported: `WindowGroup`, `Window`, `Settings`, `MenuBarExtra`, `DocumentGroup`, `Table`, `Chart`, `@StateObject`, `@AppStorage`, `@FocusState`, `@SceneStorage`, `@NSApplicationDelegateAdaptor`, `windowResizability`, `contextMenu`, `keyboardShortcut`, `searchable`, `focused`. **Essentially the entire macOS windowing and menu surface.** So the *test file* looks like coverage the *codebase* doesn't have |
| **swift-snapshot-testing 1.19.4** | This exact bitmap, on this OS/Xcode/font stack, matches an accepted reference. **The only one of the three that touches real rendering** | Its own doc-comment warns: "**Snapshots must be compared on the same OS as the device that originally took the reference.**" A green run locally says nothing about another macOS version; a red run on CI usually means "different OS," not "regression." **And record-mode will happily bless an already-wrong baseline on first run** |
| **XCUITest** | A synthetic Accessibility-driven user against the real compiled running app with a real window server can complete this sequence. **Strongest end-to-end claim** | Slow and timing-flaky, which breeds retry-tolerance that erodes trust in real regressions |

**Also worth knowing:** ViewInspector **requires production-code changes** for `@State`/`@Environment` views (an `internal var didAppear` or an `Inspection<Self>` helper copied into your *build* target), and while its README says it uses "the official Swift reflection API," its own architecture doc requires a `typePrefix` matching "SwiftUI's internal type name" — **so it avoids private *functions* while being tightly coupled to private *type names*.** And swift-snapshot-testing's macOS text strategy calls a **private AppKit selector**, `_subtreeDescription`.

*Unverified sub-item, flagged:* **whether macOS XCUITest requires Accessibility permission and a GUI session, and whether hosted GitHub macOS runners can run it, is not stated** in the Apple doc surface probed nor in `actions/runner-images`' macOS README. **(c)** — push a minimal UI test to a public repo and record whether it passes, hangs, or fails with a TCC error.

### CI without hardware

**There is no Apple-provided "requires hardware" semantic** — the meaning is entirely a project convention. The mechanics: XCTest's `XCTSkip`/`XCTSkipIf`/`XCTSkipUnless`; Swift Testing's `ConditionTrait` surfaced as `.enabled(if:)`/`.disabled(if:)`, plus `Tag` with reverse-DNS members and suite→test inheritance; and selection via `swift test --filter/--skip` or `xcodebuild -only-testing/-skip-testing`.

**Real macOS/Swift precedents:**

- **Caldis/Mos** (Logitech HID++ over USB/Bluetooth) uses exactly the env-var convention: `static var hasDevice: Bool { ProcessInfo.processInfo.environment["LOGI_REAL_DEVICE"] == "1" }` with `try XCTSkipUnless(Self.hasDevice, "requires LOGI_REAL_DEVICE=1")`, plus a runtime fallback when the var is set but no session establishes.
- **wozniakpawel/PairPods** defines `ConditionTrait.enabled(if: BlackHoleHelper.isAvailable, …)` — **and critically, its CI installs BlackHole via Homebrew on the runner before `xcodebuild test`, so the "hardware" tests actually run in CI, because BlackHole is a virtual driver.**

**The dominant *architectural* answer across embedded OSS: run the logic on the host, not the target.** QMK's tests "are always compiled with the native compiler of your platform, so they are also run like any other program on your computer," with CI on `ubuntu-latest`. Karabiner-Elements has 30+ test targets testing the logic layer with synthetic events; **none requires a physical keyboard**, and CI runs on a plain `macos-15` runner.

**Two stronger precedents worth stealing:** **Zephyr BabbleSim** — Bluetooth LE tests run against a *physical-layer simulator* plus software SoC models, producing a plain Linux executable with no radio hardware. And **libusb/hidapi** runs its device-I/O test against a *real virtual* USB HID device using USB Raw Gadget inside a VM, gated to a `ci-virtual-device` label.

**Applied:** the hard constraint is only survivable if **the device boundary is narrow and everything above it is host-testable** — the QMK/Karabiner pattern. `ConditionTrait.enabled(if: … "SPECTRO_REAL_DEVICE" == "1")` is the precedented marking convention. **A recorded-transcript fake — replaying a captured byte stream from a real instrument through the same transport code — is the SpectroCapture analogue of BabbleSim and the single highest-leverage thing you can build for contributors without hardware.**

**macOS CI, current as of 2026-08-24:** `macos-latest` → **macOS 26, arm64, 3-core, 7 GB RAM**, defaulting to **Xcode 26.6**. macOS 14 is **deprecated**.

**Cost — the decisive fact for an open-source project: public repos get standard GitHub-hosted runners free and unlimited.** SpectroCapture's macOS CI costs **nothing**. (Larger runners are always charged, even on public repos. Rates: macOS 3/4-core $0.062/min vs Linux 2-core $0.006/min — **~10.3×**, not a documented "10x".) Concurrency: max concurrent **macOS** jobs is **5** on Free/Pro/Team.

**GitHub's own documented Swift example uses `swift build`/`swift test` and never mentions `xcodebuild`** — for a SwiftPM-heavy project that is the path of least resistance and **needs no signing workaround.**

**A correction on code signing in CI:** `CODE_SIGNING_ALLOWED` is **not** in Apple's public Build Settings Reference (verified by grepping the retrieved JSON) — **but it is a genuine build setting**, appearing in Xcode 26.6's shipped product-type specs. *Orchestrator-verified: 9 xcspec files contain it.* **Cite it as "real, verified in the shipped toolchain, not publicly documented," not as community folklore.**

### The `needs-hardware-verify` workflow

**The strongest documented example is PX4-Autopilot**, whose `CONTRIBUTING.md` codifies a two-tier gate: "**Hardware-dependent changes that cannot be tested in SITL should include bench test or flight test evidence.**… **Reviewers will verify that tests or test evidence exist before approving a pull request.**" Backed by a test-type table and flight logs uploaded to Flight Review so a reviewer can inspect them.

**The Linux kernel's `Tested-by:` trailer** is the ancestor convention — a credit/audit-trail mechanism, **not** an enforced gate.

**The contrast case is Klipper, which explicitly declines the maintainer-verify model:** "Submitters are expected to test their changes prior to submission. **The reviewers look for errors, but they don't, in general, test submissions.**"

**Honest negative results, stated rather than glossed:** **no repo among those inspected uses a label literally named `needs-hardware-verify` as a documented merge condition.** `home-assistant/core` has a `waiting-for-test-hardware` label — **with no description, and both the issues API and Search API returned zero issues or PRs carrying it, ever. Label exists; workflow unobserved.** And **no example was found of a machine-enforced two-tier gate.**

**Applied:** adopt the PX4 shape, scaled down — a `CONTRIBUTING.md` clause requiring hardware-touching PRs to attach evidence (a captured device transcript, a photo of the reading, an exported measurement row), a `needs-hardware-verify` label you **create yourself since no convention exists to inherit**, and a `Tested-by:` trailer for third-party verification. **Do not build a bot-enforced status check** — no OSS project of comparable size was found doing so.

**Secret scanning.** GitHub's push protection is **free on public repos** and enabled by default on new ones (2024-03-11 changelog, scoped to new public repos owned by personal accounts). Tool currency, verified live:

| Tool | Latest | Published | Status |
|---|---|---|---|
| **gitleaks** | v8.30.1 | 2026-03-21 | Active |
| **trufflehog** | v3.97.1 | 2026-08-24 | Very active |
| **pre-commit** | v4.6.2 | 2026-08-10 | Active |
| detect-secrets | v1.5.0 | 2024-05-06 | Maintained but slow |
| **git-secrets** | tag 1.3.0, **no Release object** | tag commit **2019-02-10** | **Legacy — do not recommend** |

**The specific risk here is a *vendor licence key*, a custom pattern GitHub's partner-pattern scanning will not know.** That argues for a `gitleaks` config with a project-specific rule, run **both** as a pre-commit hook and a CI job. **Layering all three is proportionate here precisely *because* the key format is bespoke;** it would be over-engineering for a project whose only secrets were AWS keys.

---

## 10. Distribution

### `altool` is dead, not deprecated

"Starting November 1, 2023, the Apple notary service **no longer accepts uploads from `altool` or Xcode 13 or earlier**." ***Orchestrator-verified on this machine: `xcrun altool` → "unable to find utility 'altool', not a developer tool or in PATH."*** **Any blog post giving an `altool --notarize-app` recipe is dead information.**

**The prerequisite is not free.** "You can only notarize apps that you sign with a **Developer ID** certificate." Apple's membership comparison lists Developer ID as an **Apple Developer Program** benefit, at **99 USD/year**, not available on a free account. **For an open-source hobby project this is the single hardest cost line in the whole distribution question.**

**Apple's stated notarization requirements:** valid signatures on all executables; a Developer ID certificate; **Hardened Runtime enabled**; a **secure timestamp**; **no `com.apple.security.get-task-allow`**; macOS 10.9+ SDK; ASCII-encoded entitlements.

**The command sequence:**

```bash
security find-identity -p codesigning -v          # confirm the identity exists

xcodebuild -scheme SpectroCapture archive -archivePath build/SC.xcarchive
xcodebuild -exportArchive -archivePath build/SC.xcarchive \
           -exportOptionsPlist ExportOptions.plist -exportPath build/Export

# Sign INSIDE-OUT: "if component A depends on component B, sign B before you sign A"
codesign -s "Developer ID Application: <TeamID>" -f --timestamp \
         "build/Export/SpectroCapture.app/Contents/Frameworks/VendorSDK.framework"
codesign -s "Developer ID Application: <TeamID>" -f --timestamp -o runtime \
         --entitlements SpectroCapture.entitlements "build/Export/SpectroCapture.app"

hdiutil create -srcFolder build/Export -o build/SpectroCapture.dmg
codesign -s "Developer ID Application: <TeamID>" --timestamp \
         -i com.example.spectrocapture.dmg build/SpectroCapture.dmg

xcrun notarytool store-credentials "SC-notary" \
      --key ~/.private_keys/AuthKey_XXXXXXXXXX.p8 --key-id XXXXXXXXXX --issuer <uuid>
xcrun notarytool submit build/SpectroCapture.dmg -p "SC-notary" --wait
xcrun notarytool log <submission-id> -p "SC-notary" developer_log.json   # ALWAYS
xcrun stapler staple build/SpectroCapture.dmg
xcrun stapler validate build/SpectroCapture.dmg
```

**Apple is explicit that reading the log is not optional:** "**Always check the log file, even if notarization succeeds**, because it might contain warnings." And on stapling: "While you can notarize a ZIP archive, **you can't staple to it directly**" — **the practical argument for shipping a DMG.**

**Documented failure modes:**

| Log message | Cause | Diagnostic |
|---|---|---|
| `The signature of the binary is invalid.` | Unsigned or modified after signing | `codesign -vvv --deep --strict <path>` |
| `The binary is not signed with a valid Developer ID certificate.` | Wrong cert type | `spctl -vvv --assess --type exec <path>` |
| `The signature does not include a secure timestamp.` | Custom export path skipping archive/export | `codesign -dvv` — a `Timestamp` value is good, **`Signed Time` means no secure timestamp** |
| `com.apple.security.get-task-allow` present | `CODE_SIGN_INJECT_BASE_ENTITLEMENTS=YES`, **Xcode's default for new macOS projects** | Strip it from distribution entitlements |

**Performance notes:** notarization "completes for most software within 5 minutes, and for 98 percent within 15 minutes"; "**Limit notarizations to 75 per day**"; `notarytool submit` accepts **only** UDIF disk images, signed flat installer packages, and ZIP archives.

### MAS vs Developer ID

| | Mac App Store | Direct (Developer ID) |
|---|---|---|
| App Sandbox | **Mandatory** | **Optional** |
| Notarization | Not required | **Required** |
| Hardened Runtime | Not a stated prerequisite | **Required** by notarization |
| Container | `.pkg` submission | `.dmg` / `.pkg` signable; **`.zip` not signable** |

**One trap if you ever ship both:** "The default DR for a Mac App Store app and a Developer ID-signed app are **not mutually compatible**… those variants don't share access to privacy-protected resources" — **a user switching builds would be re-prompted for Bluetooth permission.**

**MAS is the wrong channel regardless of the sandbox question** — the app depends on a user-supplied vendor licence key and user-supplied hardware, squarely outside App Store review's model.

### Entitlements and usage strings

| Need | Entitlement | Info.plist |
|---|---|---|
| App Sandbox | `com.apple.security.app-sandbox` (10.7+) | — |
| Bluetooth | `com.apple.security.device.bluetooth` (10.7+) | **`NSBluetoothAlwaysUsageDescription` — required** (macOS 11.0) |
| USB | `com.apple.security.device.usb` (10.7+) | **None on macOS** |
| Serial | `com.apple.security.device.serial` — **not in Apple's current reference (404)**; documented only in the archived Entitlement Key Reference | **None** |
| Outbound network | `com.apple.security.network.client` (10.7+) | **None** |
| User's SQLite file | `com.apple.security.files.user-selected.read-write` (10.7+) | — |
| Persisting that access | `com.apple.security.files.bookmarks.app-scope` — **also 404 in the current reference** | — |

**The serial entitlement is the sharp edge, verified three ways.** Apple's current App Sandbox reference lists a Hardware topic with camera, microphone, usb, print, bluetooth — **no serial.** Its JSON 404s. And a recursive grep of Xcode found **no capability UI for it — you must hand-edit the `.entitlements` plist.**

**But it works.** *Orchestrator-verified in `/System/Library/Sandbox/Profiles/application.sb` on macOS 26.6:*

```scheme
(when (entitlement "com.apple.security.device.serial")
      (allow file* (require-all (vnode-type TTY)
        (require-not (require-any … (regex "^/dev/tty[^\\.]") (regex "^/dev/pty") …)))))
```

**The deny regex is `^/dev/tty[^\.]` — it excludes `/dev/tty0`, `/dev/ttys001`, but `/dev/tty.usbserial-*`, `/dev/tty.usbmodem*` and all of `/dev/cu.*` are permitted, because the dot is excluded from the deny pattern.** Exactly the device-node shape a USB-serial spectrophotometer presents.

The same file confirms the others, including — **worth knowing** — that without `network.client`, **DNS resolution itself is denied**, not just the connection.

**What happens when one is missing.** **Missing sandbox entitlement → the operation *fails*, it does not crash:** "If your app attempts to use capabilities that you didn't request, it'll **fail to complete those operations even if you use the APIs correctly**." Diagnose via `Sandbox is preventing this process from…` on stderr, or Console filtered on `subsystem:com.apple.sandbox.reporting`, `category:violation`.

> **A correction to the brief's own framing.** The brief said a missing Bluetooth usage string causes a "hard crash." **Apple's actual wording is: "attempts to access the resource fail, and *might* cause your app to crash."** Directionally correct and Apple-documented, but **overstated as certainty.** No Apple document specifies, per-framework, whether CoreBluetooth on macOS 26 terminates the process or returns `CBManagerAuthorizationDenied`. **(c)** — 20-minute experiment: build a sandboxed app with the entitlement and **no** usage string, instantiate `CBCentralManager`, and record whether the process is terminated (check `~/Library/Logs/DiagnosticReports/`) or merely reports `.unauthorized`. Repeat with the key present-but-empty as a separate case.

*Also:* `NSBluetoothPeripheralUsageDescription` is **iOS-only** — **do not add it to a macOS app.** And forward-looking: `com.apple.developer.accessory-access.usb` is a **new, next-OS USB access model** with platform metadata **macOS 27.0** — worth watching, not actionable for a macOS 26 target.

### Sandbox and a user-chosen SQLite file

**The baseline flow:** an open/save panel extends the sandbox "**as if you called `startAccessingSecurityScopedResource()`**"; to persist across launches, create bookmark data with `.withSecurityScope` — but "**Unlike the bookmarks with implicit security scope**… when you resolve a security-scoped bookmark that you retrieve from a stored representation, **the system doesn't automatically extend your app's sandbox**." On each launch: resolve, recreate if `bookmarkDataIsStale`, start access, use, **stop access**.

**The leak that bites long-running apps:** "**If you fail to relinquish your access to file-system resources when you no longer need them, your app leaks kernel resources. If sufficient kernel resources leak, your app loses its ability to add file-system locations to its sandbox… until relaunched.**" **For a bulk-capture app that may hold a database open for hours, get the balance right.**

> **The SQLite-specific refinement — pick the *folder*, not the file.** WAL mode maintains "an additional quasi-persistent `-wal` file and `-shm` shared memory file," and "**It is not possible to open read-only WAL databases.** The opening process must have write privileges for the `-shm`… or else write access on the directory." **A security-scoped grant on a single `.sqlite` file does not obviously cover `.sqlite-wal`, `.sqlite-shm`, nor the directory write needed to create them.**
>
> **Apple documents the fix directly:** "**When the URL your app receives… represents a folder, the operating system extends your app's sandbox to items within that folder, and recursively in nested folders.**" So: have the user choose a **folder**, bookmark that, and place the `.sqlite` inside it. **The sidecars are then covered by construction.**

Alternatives if a file-only pick is required: **related file access** (`NSIsRelatedItemType: YES` + `NSFilePresenter` + `NSFileCoordinator`) or **document-relative bookmarks**.

**And sandbox denial is not the only failure mode** — POSIX permissions and ACLs (`EACCES`), SIP (`EPERM`), and data protection all produce failures. **Handle these distinctly, or your error messages will mislead.**

**A defensible option:** since Developer ID does not *require* the sandbox, shipping **unsandboxed with Hardened Runtime** sidesteps this entire class of problem while still satisfying notarization. **Sandboxing is the more principled choice, but it should be a deliberate decision, not an assumed requirement.**

### Distribution and updates — a time-critical policy change

> ### ⚠️ Homebrew requires signing and notarization for casks from **2026-09-01**
>
> From Homebrew's own discussion #6482: casks are now Gatekeeper-audited, the deprecation message reads "**It will be disabled on 2026-09-01**," and **387 of 7,624 casks (~5%) are currently deprecated** for failing the check — including alacritty and qutebrowser. Maintainers state this is a Homebrew policy choice, not an Apple mandate, motivated by reducing unfixable bug reports.
>
> ***Orchestrator-verified: 2026-09-01 is 8 days from today.***
>
> **Practical consequence: an unsigned SpectroCapture build almost certainly cannot enter the official `homebrew/homebrew-cask` tap.** A personal tap remains viable.

**And the Gatekeeper bypass got materially worse in macOS 15.** Apple's current "Safely open apps on your Mac" describes **only** the System Settings flow — "Click Privacy & Security, scroll down, and click the Open Anyway button." **No control-click override appears anywhere in current Apple documentation.** So an unsigned build now costs your users a multi-step System Settings detour **after a failed first launch** — a real adoption tax for a niche scientific tool.

**What comparable macOS OSS projects actually do**, verified from live cask definitions:

| Project | Cask | Artifact | Updater |
|---|---|---|---|
| exelban/stats | `stats` | `.dmg` from GH Releases | **Hand-rolled** `Kit/plugins/Updater.swift` polling the Releases API |
| rxhanson/Rectangle | `rectangle` | `.dmg` | Sparkle |
| iina/iina | `iina` | `.dmg` | Sparkle |
| p0deje/Maccy | `maccy` | `.app.zip` | Sparkle via SPM |
| MonitorControl | `monitorcontrol` | `.dmg` | Sparkle |
| BlackHole | `blackhole-2ch` | `.pkg` | **None** — reinstall to update |

**All seven ship signed and notarized pre-built binaries through the official tap; none currently distributes an unsigned build via Homebrew.** All set `auto_updates true`, deferring to the app's own updater.

**Sparkle 2.9.6** (2026-08-16) is the dominant choice; **minimum macOS 12.0**, verified from its own `ConfigCommon.xcconfig`. Signing: EdDSA (ed25519), private key in the keychain, public key in `Info.plist` as `SUPublicEDKey`.

**The sandbox interaction matters given the above:** **`Installer.xpc` is mandatory** for sandboxed apps; `Downloader.xpc` is optional and Sparkle **recommends against it**, advising `com.apple.security.network.client` instead — **which you already need, so it is free.** A temporary-exception entitlement (`com.apple.security.temporary-exception.mach-lookup.global-name` with `-spks`/`-spki`) is required, plus `SUEnableInstallerLauncherService`.

**If you ship unsandboxed, Sparkle gets substantially simpler: no `Installer.xpc`, no temporary-exception entitlement. That is a genuine, non-trivial argument in favour of not sandboxing.**

**Recommendation:** for SpectroCapture the licence-key-and-hardware requirement changes nothing about distribution mechanics — the key is user-supplied at runtime, not baked into the artifact. **The decision is binary: pay the $99/yr and notarize**, which unlocks the cask, a clean first-launch, and Sparkle; **or** ship a source build plus unsigned artifact and accept both the Settings detour and exclusion from `homebrew-cask` after 2026-09-01. **Given that your users are lab and print professionals who already bought a spectrophotometer, the $99 is the right trade.**

---

## Residual gaps

**(a) No literature exists** — comparative evidence on which architectural pattern survives growth in shipped macOS apps; VIPER-in-SwiftUI failure modes; Xcode indexing degradation from modularization; a `needs-hardware-verify` label convention to inherit; an open-source macOS support-bundle convention; controlled routing-architecture comparison.

**(b) Exists but unreachable** — Swinject's documentation (simply not fetched); Stack Overflow (Cloudflare wall for both agents; **no Stack Overflow content appears anywhere in these results**).

**(c) Inherently empirical** — TCA's compile-time cost; SPM build-time quantification; whether the 2020–2021 `Bundle.module` Preview/test failures reproduce under Xcode 26.6; a numeric per-view-body budget; whether macOS XCUITest runs on hosted runners; whether a missing Bluetooth usage string crashes or returns `.unauthorized`; whether `OSLogStore(scope: .currentProcessIdentifier)` evades the `local()` entitlement gate; whether split-view column widths persist across launches.

**Every (c) has a specified experiment in its section.** The two highest-value are the Bluetooth usage-string test (20 minutes, settles a crash-vs-fail question the brief got wrong) and the OSLog privacy probe (settles what an unprivileged support bundle can actually contain).

## Sources deliberately not used

- Hacking with Swift pieces on delegate adaptors are about **`UIApplicationDelegateAdaptor` (iOS)**, not the macOS equivalent — same API family, wrong platform.
- The **NSSpain SoundCloud talk** could not be located and exists here only as one author's characterization.
- The **Airbnb build-time figure** has no primary source.
- Any claimed **Point-Free statement on MV vs MVVM** — a grep of the entire TCA docs corpus returns zero matches. **Such a citation would be fabricated.**
