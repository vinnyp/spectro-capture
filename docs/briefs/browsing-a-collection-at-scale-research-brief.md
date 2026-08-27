# Research Brief: Browsing a Collection at Scale in SwiftUI on macOS

## Context

**SpectroCapture** is an open-source macOS app for **bulk color acquisition**: the user imports an inventory of physical items, scans heads-down through it with a spectrophotometer, and every measurement lands in a local SQLite database the user owns. This brief covers the **second half** of that app — everything after the scanning stops. Once a collection exists, the user browses it, searches it, corrects it, and looks at it. A companion brief covers the capture mode and the seam between the two; this brief does not revisit either.

The scale to design for: a realistic v1 collection is on the order of **10³ items** (a marker collection of 1,200 is the motivating case), and the architecture should not fall over at **10⁴–10⁵** — a cataloguing use case in a heritage or textile context, which our competitive research identified as a plausible future persona, reaches that range. Every item carries a raw instrument payload, a full spectral curve (wavelength/reflectance arrays), several derived color spaces, arbitrary user metadata from the imported inventory, and a version history of prior measurements. That is a wide row with a heavy blob, rendered as a *visual* object — a color swatch — which makes this a harder rendering problem than a text list of the same length.

Two requirements make this collection view unusual, and both are product commitments rather than nice-to-haves. First, **visual honesty**: monitors silently render out-of-gamut colors as the "closest color," which hides from the user which of their collection colors are faithfully renderable and which are not. Our vision spec commits to marking renderable versus non-renderable honestly rather than displaying a comfortable lie, and to flagging derived sRGB values as gamut-clipped so downstream consumers inherit the caveat and not just the number. Second, **non-destructive correction**: re-scanning an item updates its canonical value but must never destroy the prior one — corrections layer history rather than overwriting it.

**Already decided — do not research or re-litigate these.** Platform is macOS-only; **SwiftUI** is the UI framework; iOS, Windows, and Linux are explicit non-goals. Persistence is a local **SQLite** file, portable and directly queryable by the user outside the app, with the raw instrument payload as the canonical record. The app is offline-first with no cloud sync — the SQLite file *is* the sync strategy. Export is CSV plus direct SQLite access. Color-library matching (PANTONE/RAL/NCS) is out of scope, as is multi-instrument support and any print-QC depth beyond a simple delta-E comparison. The licensing and instrument decisions are settled in our own ADR-0001 and ADR-0002.

**The gap this closes.** The project has a product vision, a competitive analysis, and two decision records, but **no app-architecture decision record** — and the collection view is where the architectural choices bite hardest, because the UI layer, the data layer, and the color-management layer all have to agree. This brief feeds that ADR. The questions are about how to build a browsing and editing surface that stays responsive at scale in SwiftUI, what the data layer underneath it should look like, and how to render color honestly on a display that would rather not. Answers should be grounded in shipped applications, Apple's documentation, and measured evidence — not in what ought to work.

---

## Questions

### 1. Collection Model and Information Architecture

- In shipped applications that manage large item collections (digital-asset managers, photo libraries, music and ebook libraries, specimen and inventory catalogues), what are the dominant organizational models — flat with metadata filtering, hierarchical folders, tag-based, or smart/saved queries — and what evidence exists on which scales best for a single user?
- What is documented practice for supporting **multiple collections** in a single-file local database, and where do applications draw the line between a collection, a filter, and a saved view?
- How do shipped applications handle an item that belongs to **more than one collection**, and what are the documented trade-offs between copy semantics and reference semantics?
- What navigation structures are recommended in Apple's macOS Human Interface Guidelines for a library-style app, and which specific SwiftUI container (`NavigationSplitView` and its column configurations) implements each — from which macOS version?
- What patterns exist for **sidebar organization** at scale, when the number of collections itself grows large enough to need its own scrolling and grouping?

### 2. View Modes — Grid, List, and Table

- For a collection whose items are primarily *visual*, what evidence exists on when a grid outperforms a list or a table for findability and scanning speed?
- What are the documented capabilities and limitations of SwiftUI's `Table` on macOS — sorting, column resizing and reordering, multi-column selection, custom cell content — and which features arrived in which macOS version?
- How do shipped macOS applications implement **view-mode switching** (grid/list/table) over the same underlying data without losing selection, scroll position, or sort state?
- What is documented practice for **adjustable item density** (a thumbnail-size slider or zoom control) in grid views, and how do applications persist that preference?
- What guidance exists on the minimum visual size at which a color swatch remains reliably distinguishable, and are there published perceptual thresholds relevant to sizing a swatch grid?

### 3. Virtualization and Rendering Performance at Scale

- What are the documented performance characteristics of SwiftUI's `List`, `LazyVStack`/`LazyVGrid` inside a `ScrollView`, and `Table` on macOS at 10³, 10⁴, and 10⁵ rows — and are there published benchmarks with actual numbers?
- Which of these SwiftUI containers actually **virtualize** (recycle views outside the viewport) on macOS versus merely lazily initialize, and what documented evidence establishes the difference?
- What are the known SwiftUI performance pitfalls specific to large collections — identity churn from unstable `id`s, expensive `body` recomputation, over-broad `@Observable` invalidation, `AnyView` erasure — and what are the documented remedies?
- At what collection size do practitioners report needing to drop to AppKit (`NSTableView`, `NSCollectionView` via `NSViewRepresentable`) for acceptable macOS performance, and what specific symptoms trigger that decision?
- What tooling and methodology are recommended for **measuring** SwiftUI rendering cost on macOS — Instruments templates, `Self._printChanges()`, the SwiftUI performance instrument — and what metrics indicate a problem?
- How should **thumbnail or swatch rendering** be handled at scale — computed per-frame, cached in memory, or precomputed and stored — and what are the documented caching strategies for a scrolling grid?

### 4. Search, Filter, Facet, and Sort

- What are the established interaction patterns for **incremental/live search** over a large local collection, and what latency budget do sources give for keystroke-to-results?
- How do shipped applications combine free-text search with **structured filters** (numeric ranges, categorical facets, date ranges) in one interface without overwhelming the user?
- What is documented practice for implementing full-text search over SQLite for this workload — FTS5 configuration, tokenizers, index maintenance cost, and how it interacts with a `LIKE`-based fallback?
- What patterns exist for **saved searches / smart collections**, and how do applications represent a stored query so it survives schema changes?
- How do applications support **searching within numeric or perceptual ranges** — for example, finding all items near a given color — and what indexing strategies make that tractable in SQLite?
- What guidance exists on **sort stability and multi-key sorting** in a large table view, and how is sort state communicated and persisted?

### 5. Selection and Bulk Operations

- What are the documented selection models for large collections on macOS — click, shift-range, command-toggle, marquee, select-all-matching-filter — and which does SwiftUI support natively on `List`, `Table`, and `LazyVGrid`, from which macOS version?
- How do shipped applications handle **select-all across a filtered set** that exceeds what is loaded in memory, and what is the documented pattern for representing "everything matching this query" as a selection?
- What interface patterns exist for **bulk edit** of a metadata field across a large selection, including how mixed values are displayed and how partial application is communicated?
- What is documented practice for showing **progress and cancellation** for a bulk operation over thousands of items, and how do applications keep the UI responsive during it?
- How do applications make bulk operations **reversible**, and what are the documented approaches to undo for an operation too large to hold in a naive undo stack?

### 6. Editing Surfaces — Inline, Inspector, and Modal

- What evidence or guidance exists on choosing between **inline editing**, a **persistent inspector panel**, and a **modal sheet** for item metadata in a library-style macOS app?
- What is Apple's current documented guidance and SwiftUI API for an **inspector** on macOS (`inspector(isPresented:)`), from which macOS version, and how does it interact with `NavigationSplitView`?
- What patterns exist for **keyboard-driven editing** in a table — tab-to-next-field, commit-on-return, escape-to-cancel — and how well does SwiftUI's `Table` support them natively?
- How do shipped applications handle **autosave versus explicit commit** for item edits, and what evidence exists on which produces fewer user errors in a data-management context?
- What is documented practice for **validating an edit** against constraints (uniqueness, format, referential integrity) and surfacing the failure without losing the user's input?

### 7. Non-Destructive Editing and Version History

- What are the dominant data models for **per-item version history** in local-first applications — append-only event log, snapshot-per-version, or current-plus-diff — and what are the documented trade-offs in storage and query complexity?
- How do shipped applications expose version history in the interface without cluttering the primary view, and what patterns exist for comparing or reverting to a prior version?
- What is documented practice for representing a **canonical/current value** alongside its history in a relational schema, and how do applications keep queries against "current" fast as history grows?
- How do applications handle **history for a heavy payload** (a large blob or array per version) — full copies, deduplication, or compression — and what evidence exists on storage growth in practice?
- What patterns exist for distinguishing, in the data model and the UI, between a **correction** (the earlier value was wrong) and a **re-measurement** (the item itself changed over time)?

### 8. Color Rendering and Visual Honesty

- What is the documented, correct pipeline on macOS for rendering a measured color (from CIE XYZ or L\*a\*b\*) to screen with color management applied — which specific `CGColorSpace`, `NSColor`, and ColorSync APIs are involved, and where are the common mistakes?
- How does an application determine whether a given color is **inside or outside the gamut** of the user's actual display, and what documented APIs expose the display's color profile on macOS?
- What are the established interface patterns for **marking out-of-gamut colors** — the soft-proofing conventions in professional color tools, out-of-gamut warnings, hatching or badge overlays — and what evidence exists on which users understand?
- How should an application render a **spectral curve** efficiently for many items, and what are the documented approaches for sparkline-scale versus full-detail plotting in SwiftUI?
- What is documented practice for handling **wide-gamut displays** (Display P3) versus sRGB in the same application, and how do applications avoid double-converting or silently clamping?
- How do shipped color applications communicate that an exported sRGB value is a **lossy derivation** rather than the measured truth, and are there established conventions for flagging it in exports?

### 9. Data Layer Behind a Responsive UI

- For a SwiftUI macOS app over an existing SQLite file, what are the documented trade-offs between **GRDB**, **SQLite.swift**, raw C API usage, and **SwiftData** — specifically regarding control over the schema, observation of changes, and whether the file stays portable and externally queryable?
- Which of these options support **change observation** that drives SwiftUI updates efficiently at scale, and what are the documented mechanisms (GRDB's `ValueObservation`, SwiftData's `@Query`) and their invalidation granularity?
- What is documented practice for **pagination or windowed fetching** behind a scrolling collection view, and how do applications reconcile that with a virtualized list that may jump to an arbitrary scroll position?
- What indexing strategy is documented for this access pattern — filtering and sorting on several metadata columns while a large blob sits in the same row — and does column ordering or a separate blob table matter in SQLite?
- How should **derived values** (color spaces computed from a raw payload) be handled — computed on read, stored as columns, or materialized in a view — and what are the documented trade-offs when the derivation logic changes?
- What is documented practice for keeping the main thread free while querying, in a SwiftUI app using structured concurrency, and what are the known pitfalls of database access from an actor context?

### 10. Empty, Loading, and Error States; Perceived Performance

- What are the documented patterns for **skeleton/placeholder content** versus spinners versus progressive rendering in a collection view, and what evidence exists on which feels faster?
- What guidance exists on designing a useful **empty state** for a collection app — first-run, no-results-for-this-filter, and everything-filtered-out are three different states with different correct responses?
- What is documented practice for **preserving and restoring scroll position and selection** across view-mode changes, filter changes, and app relaunch on macOS, and which SwiftUI APIs support it?
- How do applications communicate a **partially-loaded or still-indexing** collection without blocking interaction, and what patterns exist for progressive availability of search?
- What are the documented approaches to surfacing a **data-integrity problem** (a corrupt row, an unreadable payload, a failed migration) in a local-first app where the user owns the file and could have modified it externally?

---

## Deliverable Requested

For each question, provide the answer with:
- Source links, version numbers, and package names where applicable
- Side-by-side comparison tables where the question spans multiple options
- A clear statement of which position is most commonly held in recent (2024–2026) sources when community opinion is divided

Flag any answer that is inferred from indirect evidence rather than explicit documentation. State the version of any SDK, framework, or specification the answer applies to — for SwiftUI and macOS answers specifically, state the minimum macOS version each API requires, and note where an API's behavior changed between macOS versions.

**Citation requirements — mandatory throughout your response:**
Every factual claim in your answers that originates from a source must be cited inline using APA author-date style: (Author, Year) or (Organization, Year). Do not cite general knowledge. Do not use footnotes or numbered references inline — use author-date only. At the end of your response, include a References section with a full APA reference list, alphabetical by first author or organization, covering every source cited inline. The response is considered incomplete without inline citations and the References section.

**Closing recommendation — mandatory:**
At the end of your response and before the References section, provide an opinionated closing recommendation. It must be a decision, not a list of options: state the collection-browsing architecture you would build — the view container and whether you would use SwiftUI or drop to AppKit at this scale, the database library, the change-observation mechanism, the search implementation, the version-history data model, and the color-rendering pipeline — and say plainly what you would *not* build. Where performance evidence is thin, say which choice you would make provisionally and what measurement would settle it. Cite the key sources that informed the recommendation inline.

**The following two sections are also mandatory. The response is considered incomplete without all four of: answers with inline citations, closing recommendation, Question Status, Unanswered Questions Summary, and References.**

**Question Status** — reproduce every question from this brief, grouped under its original concern area heading. Mark each [x] if answered or [ ] if not. For every unanswered question, provide: (1) the reason it was not answered, citing the specific gap in available documentation or sources, and (2) a concrete, specific follow-up action the reader can take to close the gap. "Needs more research" is not an acceptable follow-up. "Build a 50,000-row `LazyVGrid` harness on macOS 15, scroll it under the SwiftUI instrument in Instruments, and record view-body invocation counts and frame drops" is.

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
