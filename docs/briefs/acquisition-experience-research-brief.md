# Research Brief: Acquisition Experience Design Patterns

## Context

**SpectroCapture** is an open-source macOS app for **bulk color acquisition**: the user imports an inventory of physical items, then scans heads-down through it with a spectrophotometer, with no per-item metadata entry between scans. Every measurement lands in a local SQLite database the user owns. The motivating case is digitizing a personal marker collection, but the pattern generalizes: this is an app with a *capture mode* (fast, repetitive, hardware-driven, one item after another) bolted to a *collection mode* (browse, search, edit what you captured). This brief covers **only the capture mode**; a companion brief covers browsing and editing a collection at scale.

The competitive position that makes this worth researching: every existing desktop tool for this hardware class is **verification software** — measure a sample, compare it to a standard, report the delta. None is built for acquisition. The workflow inversion is the entire product thesis, which means the capture experience is not a feature, it is the thing. A capture loop that is 3 seconds slower per item costs an hour on a 1,200-item collection. The design target from our vision spec is that the session is paced by the instrument's physical scan cycle, not by the UI — the user should never be waiting on software.

**Already decided — do not research or re-litigate these.** Platform is macOS-only (iOS, Windows, and Linux are explicit non-goals for v1). UI framework is **SwiftUI**. Persistence is a local **SQLite** file, with the instrument's raw measurement payload stored as the canonical record and derived color spaces computed from it. The app is offline-first. The v1 instrument family is fixed (one vendor SDK, connected over BLE or USB, one device at a time, one scan in flight at a time — the SDK is architecturally single-device and single-session). Inventory import is CSV in v1. Multi-instrument support, cloud sync, and color-library matching (PANTONE/RAL/NCS) are all out of scope. The licensing and instrument-selection decisions are settled in our own ADR-0001 and ADR-0002 and are not in scope here.

**The gap this closes.** We have a product vision, a competitive analysis, and two decision records — but **no app-architecture decision record**. This brief feeds one. The open questions are all about *how a capture-mode app should be built*: what the queue is, how a person drives it without looking at the screen, what happens when a scan fails at item 400 of 1,200, how state survives a crash mid-run, and how a just-captured item hands off to the collection. These are questions other people have solved in adjacent domains — warehouse and retail scanning, field data collection, tethered studio photography, library and museum cataloguing, lab sample intake — and we would rather inherit their patterns than rediscover them. Answers should be grounded in shipped products and documented practice, not invented.

---

## Questions

### 1. Queue Model and Task Framing

- In shipped bulk-capture applications, what are the dominant models for structuring a capture session — a pre-loaded worklist the user advances through, an append-only capture log classified afterward, or a hybrid — and what evidence exists about which produces fewer errors and higher throughput?
- Where a pre-loaded worklist is used, how do real applications handle items scanned **out of the queue's order**, and items physically present but absent from the imported inventory?
- What is documented practice for **multi-sample averaging** at a single queue position (taking N readings of one item and reducing them), and how is the reading count chosen or surfaced to the user?
- How do bulk-capture applications represent a queue row's lifecycle states (pending, in progress, captured, flagged, skipped), and which state models appear in shipped products versus proposed in the literature?
- What patterns exist for letting a user **re-open and correct** an already-captured queue row without leaving the capture flow?

### 2. Heads-Down Input and Interaction Modality

- What input modalities are documented for heads-down capture workflows on desktop — keyboard-only, foot pedal, barcode-scanner-as-keyboard-wedge, hardware button on the instrument itself, voice — and what evidence compares them on throughput or error rate?
- In applications where the capture device itself has a physical trigger, how is the division of labor decided between the device button and the host application's controls?
- What are the documented accessibility implications of a heads-down, timing-sensitive capture mode on macOS, and how do shipped apps reconcile a fast keyboard loop with VoiceOver and Full Keyboard Access?
- What non-visual feedback channels (audio cues, haptics, spoken confirmation) are used in shipped capture applications to confirm a successful reading without requiring the user to look at the screen, and is there published evidence on their effectiveness?
- What keyboard-shortcut conventions exist specifically for advance/retry/skip/flag operations in a linear task queue, and do any macOS Human Interface Guidelines or established apps set a precedent worth following?

### 3. Pacing, Latency, and Feedback Against a Hardware-Bound Cycle

- When an application's throughput is bounded by a hardware cycle it does not control, what interface patterns are documented for keeping the user productively occupied rather than idle-waiting?
- What are the published latency thresholds at which users perceive a UI as instantaneous, responsive, or lagging, and which specific sources establish those numbers?
- How do shipped applications communicate "the device is working, not the app" so a hardware delay is not misread as software slowness — and is there evidence this distinction changes user tolerance?
- What is documented practice for **optimistic UI** in capture flows — advancing the queue before the write is confirmed — and what are the documented failure modes when the write subsequently fails?
- In applications where the user can physically outpace the device, what patterns prevent input loss (buffering, queueing, or explicit lockout) and which is preferred?

### 4. Mid-Run Error Handling and Recovery

- What are the dominant patterns for handling a **recoverable per-item failure** during a long automated run without stopping the run — inline retry, silent requeue, flag-and-continue — and what evidence exists on which preserves flow best?
- How do shipped bulk-capture applications distinguish, in the interface, between errors the user can fix immediately (reposition the device, replace a battery) and errors requiring the session to end?
- What patterns exist for a **deferred-error queue** — collecting flagged items during a run for resolution at the end — and how do applications surface that backlog without interrupting the run?
- What is documented practice for error-message design in a heads-down context, where the user is not reading the screen at the moment the error occurs?
- How do applications handle a **run of consecutive failures** (suggesting a systemic problem rather than a bad item), and are there documented heuristics for when to halt automatically?

### 5. Pre-Flight and Session Readiness

- What patterns exist for **pre-flight checks** before a long capture run — device health, calibration currency, storage headroom, power state — and how do shipped applications decide what blocks a session versus what merely warns?
- How do applications handle a readiness condition that expires *during* a long session (a calibration coming due, a battery draining, an authorization window lapsing) — prompt immediately, defer to a natural boundary, or pre-empt before the session starts?
- What is documented practice for **guided calibration flows** in measurement applications, and what completion and verification steps do shipped products include?
- How do applications communicate a **time-boxed readiness window** ("valid until ⟨date⟩") so a user can act before going into a session, and are there established patterns from other domains (certificates, licenses, offline tokens) worth borrowing?
- What evidence exists on whether front-loading setup friction before a session improves or harms overall completion rates compared with resolving issues as they arise?

### 6. Durability, Crash Safety, and Resumability

- What write strategies do shipped capture applications use to ensure a reading is durable before the UI advances — write-through, write-ahead, batched commit — and what are the documented trade-offs at capture speed?
- What are the specific, documented practices for SQLite durability under rapid sequential inserts on macOS — journal mode, synchronous setting, transaction batching — and what are the published trade-offs between throughput and crash safety for this access pattern?
- How do applications implement **session resumability** after a crash or quit mid-run, and what state must be persisted beyond the readings themselves to restore a session faithfully?
- What patterns exist for storing a **raw, opaque instrument payload** as the canonical record alongside derived values, and how do applications handle recomputation when the derivation logic later changes?
- What is documented practice for protecting an in-progress session from accidental termination (window close, quit, sleep) on macOS, and which APIs are involved?

### 7. Inventory Import and Column Mapping

- What are the established interaction patterns for **CSV column mapping** in shipped desktop applications — auto-detection with confirmation, manual drag-mapping, saved mapping profiles — and which produce fewer downstream errors?
- How do import flows handle malformed, ambiguous, or partially-valid input files, and what is documented practice for a preview-and-validate step before committing an import?
- What patterns exist for **identifier selection** during import (choosing which column is the stable key) and for handling duplicate or missing identifiers?
- What is documented practice for encoding and delimiter detection in CSV import on macOS, and which libraries or system APIs are commonly used in Swift applications?
- How do applications support **re-importing** an updated inventory against an existing collection — merge, replace, or reconcile — and what conflict-resolution interfaces are used?

### 8. Progress, Orientation, and Session Completion

- What progress-indicator patterns are documented for long, user-paced (rather than machine-paced) tasks, and how do they differ from patterns for automated progress bars?
- How do shipped applications answer "where am I and how much is left" in a linear queue without pulling the user's attention away from the physical task?
- What patterns exist for **session summary and completion** screens in bulk-capture workflows, and what information do shipped products surface (counts, flagged items, elapsed time, throughput)?
- How do applications handle a **partially-completed session** — explicit pause and resume, implicit save, or session abandonment — and how is an incomplete session represented afterward?
- What evidence exists on the motivational or error-reducing effect of visible progress in long repetitive data-entry tasks?

### 9. SwiftUI Implementation of a Focus-Mode Capture Surface

- What are the current documented approaches in SwiftUI on macOS for building a **keyboard-driven modal surface** that captures key events reliably — and which specific APIs (`focusable`, `focusedValue`, `onKeyPress`, `KeyboardShortcut`, `commands`) apply, from which macOS/SwiftUI version?
- What are the documented limitations of SwiftUI focus management on macOS that drive teams to drop to AppKit (`NSViewRepresentable`, `NSEvent` monitors) for capture-style interfaces, and at what point is that escape hatch considered necessary?
- What is current documented guidance for **state ownership** in a SwiftUI capture flow — `@Observable` versus `ObservableObject`, and where session state should live relative to the view hierarchy — and which macOS version gates each option?
- How should a SwiftUI application bridge an **asynchronous, delegate-based hardware SDK** into structured concurrency, and what are the documented patterns and pitfalls (actor isolation, `AsyncStream`, continuation misuse) for a sequential command interface?
- What patterns are documented for **preventing accidental navigation** away from an in-progress capture surface in SwiftUI on macOS, and what is the recommended way to intercept window close and app termination?
- What published guidance or measurement exists on SwiftUI view-update cost in a surface that re-renders on every capture event, and what profiling tools are recommended?

### 10. Measuring Acquisition Throughput

- What metrics do practitioners use to evaluate a bulk data-capture interface — items per hour, time per item, error rate, rework rate, time-to-first-capture — and which are documented as most predictive of real-world efficiency?
- What documented methods exist for **instrumenting a capture app** to separate time spent waiting on hardware from time spent waiting on the user or the UI?
- Are there published benchmarks or case studies reporting real throughput figures for comparable bulk-capture workflows (cataloguing, sample intake, inventory scanning) that could serve as a target?
- What is documented practice for detecting **user fatigue or degradation** over a long capture session, and do any shipped applications act on it?
- What analytics or telemetry approaches are appropriate for a **local-first, offline, open-source** application where transmitting usage data is not acceptable by default?

### 11. The Seam — Handoff from Capture to Collection

- In applications with a distinct capture mode and a distinct browse/edit mode, what navigation models are documented for moving between them — separate windows, a modal takeover, a tab or mode switch — and what evidence favors one?
- Where does a **just-captured item** go, and what patterns exist for making it findable immediately without disrupting the capture flow (a recents strip, an inline confirmation, deferred insertion at session end)?
- How is **shared state ownership** structured between a capture session and a persistent collection — does the capture session own a draft buffer that is committed at the end, or does it write directly to the collection store — and what are the documented trade-offs?
- What patterns exist for **entering capture mode from within a collection** (re-scanning an existing item, capturing into a specific subset) as opposed to starting a fresh session?
- How do shipped applications avoid the "two apps bolted together" failure mode, and are there documented critiques or post-mortems of applications that fell into it?
- What is documented practice for keeping a browse/edit view **consistent with an in-progress capture session** when both are visible or reachable simultaneously?

---

## Deliverable Requested

For each question, provide the answer with:
- Source links, version numbers, and package names where applicable
- Side-by-side comparison tables where the question spans multiple options
- A clear statement of which position is most commonly held in recent (2024–2026) sources when community opinion is divided

Flag any answer that is inferred from indirect evidence rather than explicit documentation. State the version of any SDK, framework, or specification the answer applies to — for SwiftUI and macOS answers specifically, state the minimum macOS version each API requires.

**Citation requirements — mandatory throughout your response:**
Every factual claim in your answers that originates from a source must be cited inline using APA author-date style: (Author, Year) or (Organization, Year). Do not cite general knowledge. Do not use footnotes or numbered references inline — use author-date only. At the end of your response, include a References section with a full APA reference list, alphabetical by first author or organization, covering every source cited inline. The response is considered incomplete without inline citations and the References section.

**Closing recommendation — mandatory:**
At the end of your response and before the References section, provide an opinionated closing recommendation. It must be a decision, not a list of options: state the capture-mode architecture you would build for this application — the queue model, the input modality, the write/durability strategy, the error-handling posture, and the capture-to-collection seam — and say plainly what you would *not* build. Where you are recommending against a pattern that is common in the field, say so explicitly and give the reason. Cite the key sources that informed the recommendation inline.

**The following two sections are also mandatory. The response is considered incomplete without all four of: answers with inline citations, closing recommendation, Question Status, Unanswered Questions Summary, and References.**

**Question Status** — reproduce every question from this brief, grouped under its original concern area heading. Mark each [x] if answered or [ ] if not. For every unanswered question, provide: (1) the reason it was not answered, citing the specific gap in available documentation or sources, and (2) a concrete, specific follow-up action the reader can take to close the gap. "Needs more research" is not an acceptable follow-up. "Build a 200-row SwiftUI `List` harness on macOS 15 and profile view-body invocations in Instruments while driving synthetic capture events" is.

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

_Note on this brief's own sources: the Context block above is synthesized from internal project documents (the SpectroCapture vision spec, competitive brief, SDK audit, and ADR-0001/ADR-0002) and cites no external published sources, so no author-side reference list is included. This does not relax the citation requirement on your answers._