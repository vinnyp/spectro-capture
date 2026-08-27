# Research Results: Browsing a Collection at Scale

## 1. Collection Model and Information Architecture

**The dominant shipped pattern is a hybrid**: one flat item store plus user-created reference collections, tags, and saved queries, with search/filter as the primary retrieval path. Zotero: "Collections allow hierarchical organization of items into groups and subgroups," "Tags allow for detailed characterization," and "Saved Searches are like smart collections. They automatically update." calibre: a flat library plus saved-query "Virtual libraries."

**On what scales best for a single user**, the strongest evidence favours navigable structure over tags. Bergman, Gradovitch, Bar-Ilan &amp; Beyth-Marom (2013), two studies where both mechanisms were available:


| Measure                                                      | Result                                                                                       |
| ------------------------------------------------------------ | -------------------------------------------------------------------------------------------- |
| Gmail: labelled messages folder- vs tag-labelled             | 64% (SD 42) vs 33% (SD 41); t(59)=2.97, p&lt;.01                                             |
| Windows 7: files given a specific folder, free-choice period | 96%                                                                                          |
| Windows 7: participants creating any tag                     | 6 of 23 (26%)                                                                                |
| Estimated retrievals by folder navigation vs tags            | 61% (SD 7) vs 5% (SD 15)                                                                     |
| **Controlled retrieval task, mean time**                     | **21.21 s (SD 20.9) using tags vs 16.44 s (SD 12.59) not using tags**; t(118)=2.22, p&lt;.05 |
| Failures                                                     | 6 (tags) vs 1 (no tags); W=1.89, p=.059                                                      |


**Scope limit, stated honestly:** these are *personal document/email* collections where the item is text and the retrieval cue is a remembered name. A colour catalogue is retrieved by *attribute* (hue, ΔE, supplier, batch), which Bergman did not test. Treat this as evidence that **a navigable, stable, single-home structure earns its keep** — not as evidence against faceted filtering.

calibre states a directly transferable position: "Using Virtual libraries is the preferred way of partitioning your large book collection into smaller sub collections. **It is superior to splitting up your library into multiple smaller libraries.**"

**Collection vs filter vs saved view — calibre's distinction is the sharpest design input:** *the difference is not persistence, it is scope of effect.* A filter "will only restrict the list of books shown"; a Virtual library also "restrict[s] the entries shown in the Tag browser" — it narrows the whole app's idea of what exists.

**Multiple collections in one file:** Zotero is the cleanest precedent — `zotero.sqlite` holds "item metadata, notes, tags," while binary attachments live *outside* in a `storage` folder of 8-character subdirectories. Directly relevant to your heavy raw-payload blobs. Zotero also warns: "Before you copy, delete, or move any of these files, be sure that Zotero is closed."

**Item in multiple collections — reference semantics, with two documented costs.** Zotero: "Adding an item to multiple collections does not duplicate the item… collections are more like music playlists than folders." The costs: (1) **remove ≠ delete** — "Deleting a collection does not delete the items in the collection," so you must ship **two distinct destructive verbs**, visually distinguishable, or users lose data; (2) **membership does not travel** — "Tags are portable, but collections are not. Copying items between Zotero libraries will transfer their tags, but not their collection placements." Collection membership is an edge in a join table, meaningless outside its own database — **it is the thing that does not survive export**, whereas tags do.

**HIG navigation, mapped to API with versions** *(all from `metadata.platforms`)*:


| Structure                      | SwiftUI API                     | Min macOS |
| ------------------------------ | ------------------------------- | --------- |
| Two/three-column library shell | `NavigationSplitView`           | **13.0**  |
| Programmatic column show/hide  | `NavigationSplitViewVisibility` | **13.0**  |
| Targeting a specific column    | `NavigationSplitViewColumn`     | **14.0**  |
| Sidebar / content list         | `List`                          | **10.15** |
| Multi-column detail table      | `Table`                         | **12.0**  |
| Swatch grid                    | `LazyVGrid` in `ScrollView`     | **11.0**  |
| Inspector column               | `.inspector(isPresented:)`      | **14.0**  |


HIG: "It's common to use a split view to display a sidebar for navigation, where the leading pane lists the top-level items or collections." And the depth rule: "**In general, show no more than two levels of hierarchy in a sidebar.** When a data hierarchy is deeper than two levels, consider using a split view interface that includes a content list between the sidebar items and detail view."

**Sidebar at scale** — five HIG rules: disclosure groups are the named remedy for volume; cap depth at two; succinct group labels; let people customize contents; and **"Avoid putting critical information or actions at the bottom of a sidebar. People often relocate a window in a way that hides its bottom edge"** — which bites exactly in the many-collections case, because an add-collection button at the sidebar's foot is what a designer reaches for. Apple gives **no numeric guidance** on how many items is too many; its answer to "too many" is structural, not numeric.

---

## 2. View Modes — Grid, List, and Table

**When a grid beats a list.** Baymard's benchmark draws the line on the **spec-driven vs visually-driven** axis and treats grid/list/**table** as three answers. Grid for visually-driven items; list for spec-driven, because "If a 'Grid View' is used… list item info has less room because significant space is dedicated to thumbnails" (23% of sites using a grid for spec-driven products is classified as a defect). **Table wins for spec-heavy items on desktop** — tables "allow users to view even more product attributes than is possible with a traditional 'List View'" and "more efficiently and effectively compare… across multiple rows." Participant quote: "It's slower to scroll through… I do like the table option a little bit more because you can really scan."

NN/g reaches a compatible conclusion on a different axis — **discriminability**. Harley (2014): image grids justify their cost only "where the differences between options become more nuanced," and are wasteful at broad category levels; a Sports Authority image grid required ~8 scroll gestures before a selection vs 1 on a text menu.

**For a colour catalogue this cuts both ways, which is the finding:** a swatch is the highest-discriminability cue available (grid's best case) *and* the row is spec-heavy — Lab, ΔE, supplier, batch (table's best case). **That is the strongest available argument that this app needs both a swatch grid and a table**, not a grid with a list bolted on.

**Evidence-quality caveat:** Baymard and NN/g are moderated qualitative research plus benchmarking, not controlled experiments with task times and CIs. **No study measures findability in a grid of colour swatches.**

**SwiftUI `Table` — capabilities and version history:**


| Feature                                                                       | Min macOS |
| ----------------------------------------------------------------------------- | --------- |
| `Table`, `TableColumn`, `TableRow`, selection, sortable columns, `TableStyle` | **12.0**  |
| **User-customizable columns** (`TableColumnCustomization`)                    | **14.0**  |
| Hide column headers (`tableColumnHeaders(_:)`)                                | **14.0**  |
| Hierarchical/outline table                                                    | **14.0**  |
| **Dynamic data-driven columns** (`TableColumnForEach`)                        | **14.4**  |


**Documented limitations, and these are load-bearing:**

1. `**Table` does not sort. You do.** "When the table sort descriptors update, re-sort the data collection that underlies the table; **the table itself doesn't perform a sort operation**." Sorting is offered only on columns declared with a key path, which "prevent[s] sorting on integer and boolean columns when using Swift value types."
2. **Compact size class silently amputates the table** — "the table automatically hides headers and all columns after the first when it detects this condition."
3. **Fixed row height on macOS** — "Regardless of the actual height requirements of the content in the cell, Table will always maintain the system's default row height." This constrains swatch size inside a table row.
4. **DSL ceilings** — roughly ten columns before the result builder gives out; "poor autocomplete and cryptic compilation errors due to complex generics."
5. **No documented cell editing, no documented row reordering.**

**Version note worth flagging:** Apple's current "What's new in SwiftUI" describes "the 2027 releases," confirmed from symbol metadata as **macOS 27.0 (beta)**. Nothing there is shippable. The practical floor is macOS 13 for `NavigationSplitView`, 14 for `.inspector` / `@Observable` / Table column customization, 15 for identity-stable `ScrollPosition`.

**View-mode switching.** Finder's model is that **view state is a property of the container, not the app** — per-folder via "Always open in," or app-wide per view type via "Use as Defaults" (unavailable for Column view). A documented decision worth copying or consciously rejecting: switching folders can change your view mode.

The SwiftUI mechanisms, with versions: sort order = `[KeyPathComparator]` you hold (12.0); selection = a `Set<ID>` (10.15); **scroll anchor by item identity = `scrollPosition(id:anchor:)` (14.0)** or `**ScrollPosition` (15.0)**; per-window restoration = `@SceneStorage` (11.0).

`**ScrollPosition` (macOS 15) is the API that actually answers this**, and Apple documents the guarantee: "For view identity positions, SwiftUI will attempt to keep the view with the identity specified in the provided binding visible when events occur that might cause it to be scrolled out of view" — naming among those events "**The data backing the content of a scroll view is re-ordered**" and "**The size of the scroll view changes, like when a window is resized on macOS**." And the contrast: "For a point, SwiftUI won't attempt to keep that exact offset scrolled when the content size changes."

**The architectural consequence:** hoist selection, sort, and scroll anchor **above the container** and express them in terms of *item identity*, not indices or offsets — `Set<Item.ID>`, `Item.ID?`, `[KeyPathComparator<Item>]`. Swapping `Table` for `LazyVGrid` then destroys the container but not the state. **An offset is meaningless after a re-sort; an item ID is still meaningful.**

*Gap:* **no source documents how any shipped macOS app internally preserves selection and scroll across a view-mode switch.** Follow-up (15 minutes): in Finder, select a non-contiguous set mid-way down a large folder, cycle Icon→List→Gallery→Icon, and record whether selection survives, whether the anchor lands on the same *item* or the same *offset*, and whether sort survives. Repeat in Photos and Music. That produces a behavioural spec you can hold your own implementation to.

**Adjustable item density.** Finder's icon-size slider sits in a per-view-type options panel (⌘J), not the toolbar and not app Settings, and persists at **two scopes simultaneously**. **Recommendation:** swatch size is a preference about the user's eyes and monitor, not a property of a collection — use `@AppStorage` app-wide, the *opposite* of Finder's per-folder model, whose scoping is arguably its most-complained-about behaviour and exists for a reason you don't share. Store the edge length as a `Double` so it can be a continuous slider.

**Minimum swatch size — there is a perceptual floor, not just an aesthetic one.** Three published thresholds:

1. **Foveal tritanopia — a hard floor around 20 arcmin.** Davies &amp; Morland (2003), *BJO*: "The extent of the foveal tritanopic region is around 20–25 minutes of arc" (median 18′ in controls). **Below this, an observer fixating the stimulus loses short-wavelength discrimination** — blue/yellow differences become invisible while red/green survive. *Orchestrator-verified: "tritanopic" appears 30× in the source.* **For a colour catalogue this is not an edge case** — a swatch below ~20 arcmin makes two samples differing mainly in b look identical.
2. **The CIE observer boundary at ~4°.** The 1931 2° observer is intended for fields under about 4°; the 1964 10° observer is for larger. A UI swatch is always under 4°, so **2° colorimetry is the correct basis** for any ΔE the app displays about what the user is looking at.
3. **The colour size effect** — Xiao, Luo &amp; Li (2012) built models "capable of transforming the colour appearance of a stimulus having a viewing field of 2° to that associated with a range of viewing fields." Consequence: **a small on-screen swatch does not look like the physical sample even when the colorimetry is exactly right** — larger stimuli appear lighter and more colourful. **The honest claim is "this is the measured colour rendered correctly," never "this looks like your sample."**

**Converting to points** (INFERRED arithmetic, at 600 mm viewing distance):


| Angular size                 | Physical | ≈ points    |
| ---------------------------- | -------- | ----------- |
| 20 arcmin (tritanopia floor) | 3.5 mm   | ≈ **10 pt** |
| 1°                           | 10.5 mm  | ≈ 30 pt     |
| 2° (CIE standard observer)   | 20.9 mm  | ≈ 59 pt     |
| 4° (2°/10° boundary)         | 41.9 mm  | ≈ 119 pt    |


**A swatch below roughly 10 pt is perceptually broken for blue-yellow discrimination**; 60–120 pt is where a user's judgement corresponds to standard colorimetry. A density slider ranging 44–120 pt with a hard floor near 24 pt sits above the cliff with margin.

*Gap — **(b)**:* ASTM D1729's normative specimen-size requirement is paywalled; every public summary covers illumination and observer qualification but not specimen size. Follow-up: purchase ASTM D1729-22 and read the specimen-requirements clause.

---

## 3. Virtualization and Rendering Performance at Scale

**Read this first: almost every published, methodologically described SwiftUI scrolling benchmark is iOS, not macOS.** The macOS evidence at 10⁴–10⁵ is overwhelmingly Apple Developer Forums reports — real, dated, sometimes with Feedback IDs, but not controlled benchmarks. Every figure below is labelled with its platform.

**Published iOS benchmarks (STRV, iPhone 15 Pro / iOS 17.5.1, 1,000 rows, rebuilt before each run):**


| Metric                   | `List`       | `LazyVStack` |
| ------------------------ | ------------ | ------------ |
| Memory at launch         | 114.4 MB     | 90.2 MB      |
| After scrolling down     | 128.9 MB     | 149.0 MB     |
| After scrolling back up  | **118.2 MB** | **151.8 MB** |
| Time to scroll to bottom | **5.53 s**   | **52.3 s**   |
| Hang count               | 4.6          | **78**       |


*Orchestrator-verified: 52.3, 5.53, and 151.8 all appear in the source.* **A 9.5× scroll-time and 17× hang-count difference at only 1,000 rows.**

**macOS evidence — forum-grade but specific and dated:**


| Scale            | Container                      | Reported behaviour                                                                                                                                                                                                                            |
| ---------------- | ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ~100+ rows       | `List`                         | "unacceptably slow"; an Apple Frameworks Engineer replied "Using a ScrollView with a LazyVStack will help eliminate some of the overhead of List" (2020)                                                                                      |
| 1,000–1,219 rows | `Table`                        | **13+ s hang to select an item**; later ~3.5 s with ~1,000 items (macOS 14, M2 Max, 2024)                                                                                                                                                     |
| **10,000 rows**  | `List` in a split-view sidebar | **CPU pinned at 100% for over 50 s before the list appeared.** With `Equatable` + `.equatable()`: init &lt; 0.1 s, CPU 25–30%, smooth. **The catastrophic baseline was debug-build-specific**; release matched the `.equatable()` debug build |
| ~17,000 rows     | `List`                         | 1–2 s hang **on selection, not scrolling**; persisted after replacing row view and destination with plain `Text`                                                                                                                              |
| 5,000 rows       | `Table`                        | Loads fine; **hangs indefinitely on data update**; not reproducible on iOS; Case ID 9474931                                                                                                                                                   |
| ~150,000         | `List` + Core Data             | "junky scrolling, slow reloads"; attributed to `List`'s undisableable diffing, which also defeated `fetchBatchSize`                                                                                                                           |
| unspecified      | `Table`                        | **Regression: smooth on macOS 15.4.1, hangs on 15.5, worse on macOS 26**; unresolved as of July 2025, no Apple response                                                                                                                       |


**Answer to "are there published benchmarks": yes for iOS at 10³; no for macOS at any scale.** Nobody has published macOS numbers at 10⁵.

### The most consequential finding: no SwiftUI container documents recycling

**Apple's own wording establishes lazy *creation* and never claims recycling.** `LazyVStack` is "A view that arranges its children in a line that grows vertically, **creating items only as needed**." `LazyVGrid` likewise, contrasted with the eager `Grid`. `**List`'s documentation page says nothing at all about laziness or recycling.** Nowhere in Apple's SwiftUI documentation is there a recycling contract analogous to AppKit's.

**AppKit has an explicit API-level reuse contract and SwiftUI has none.** `NSTableView.makeView(withIdentifier:owner:)` "returns a reused view with the same identifier that is no longer on screen." Because the pool tracks the visible area rather than the collection size, **memory stays flat as the collection grows**.

**The measured evidence that lazy-create ≠ recycle** is STRV's memory trace above: `List` gave memory back across a scroll round-trip; `LazyVStack` kept climbing.

**But it is a moving target.** Bartlett (2025) reports that against the iOS 18 SDK, `LazyVStack` "has evidently been upgraded under the hood since iOS 16, to become lazy in both directions." A macOS practitioner reports `LazyVGrid` "release[s] backing views that scrolled far out of the visible area, but… later than a fixed AppKit reuse pool would have."

**The macOS-specific complication, and it is bad.** A minimal 101-row test found that on macOS "the `List` calls the `init` &amp; `body` of **every** row, even if those rows are not on screen (and might never be shown)" — verified with print statements — while `ScrollView` + `LazyVStack` invoked them only for visible rows. **That thread received zero replies and no Apple response.**

**And regardless of body-laziness, identities are always gathered eagerly:** "List and Table use identifiers to know what changes occurred… For consistency, **all the IDs of List and Table are gathered eagerly**." This is why identifier generation must be cheap and why `List`/`Table` cost scales with N even when rendering does not.


| Container                        | Lazily initializes?                                                                        | Recycles / bounds memory?                                                       |
| -------------------------------- | ------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------- |
| `VStack`/`Grid` in `ScrollView`  | No — eager                                                                                 | No                                                                              |
| `LazyVStack`/`LazyVGrid`         | Yes (Apple's own wording)                                                                  | **Not a documented contract.** Releases on modern OSes, later than a reuse pool |
| `List`                           | iOS yes; **macOS contested** — one reproducible report of eager `init`+`body` for all rows | Behaves as if it does (memory returns) — no documented contract                 |
| `Table`                          | Undocumented; IDs eager                                                                    | Undocumented                                                                    |
| `NSTableView`/`NSCollectionView` | Yes                                                                                        | **Yes — explicit API contract**                                                 |


**One correction to a claim you will meet everywhere:** "List uses UICollectionView under the hood, including all the cell recycling" is an **iOS** statement. No source establishes what macOS `List` is backed by, and the eager-body report is evidence it does not behave like `NSTableView`. **Do not carry the UIKit-backing claim across to macOS.**

### Known pitfalls and documented remedies

1. **Non-constant view count per `ForEach` element** — because IDs are gathered eagerly, "the view count per element **must be constant**, or SwiftUI must build all views to identify rows." The named anti-patterns are a conditional row (`if dog.likesBall { … }`) and `AnyView`. `**AnyView` inside a `ForEach` in a `List`/`Table` is called out by name** — erasure doesn't merely cost type-checking, it defeats row identification. Not theoretical: the 13-second hang at 1,219 rows traced to exactly this, with ~200 ms attributable to re-running the `ForEach` for every row. Apple: "It's better to move it out to the model."
2. `**.id()` on `ForEach` children destroys `List` laziness** — measured: with 40,000 Core Data records, adding `.id()` caused **all 40,000 row views to be initialized eagerly** and a &gt;1 s stall; without it, only ~10–20. For scale: the fetch of 40,000 records took ~11 ms and a single body evaluation ~0.0005 s — **the time is not where you'd guess.**
3. **Over-broad `@Observable` invalidation** — the WWDC25 headline; see [[2026-08-24-acquisition-experience-research-results-v2-gap-closure|the acquisition v2 doc]] §9.
4. `**@Environment` is a per-view dependency on the whole `EnvironmentValues`** — cost accrues even when the body doesn't run.
5. **Work inside `body`** — expensive dynamic-property instantiation, string interpolation, filtering, synchronous heap allocations.
6. **Row-level `Equatable` — the highest-leverage macOS-specific remedy found.** `.equatable()` took a 10,000-row macOS `List` from 50+ s at 100% CPU to sub-0.1 s. Available from **macOS 10.15**. **Caveat: the catastrophic baseline was a debug-build artefact. Measure in Release, or you will optimise a phantom.**
7. `**Table` row identity changed between OS generations** — iOS 16 identified rows by `TableRow`'s value; from iOS 17 / macOS 14 by the `ForEach` elements themselves. A silent identity-churn source across an OS upgrade.

### When practitioners drop to AppKit

**The reported threshold is bimodal, split by container:** `Table` fails at **~1,000–1,200 rows** (multi-second selection hang) or **at any size post-15.5**; `List` at **10,000–30,000**, though the 10,000 case was fixed in SwiftUI with `.equatable()`; `LazyVGrid` at **~100,000** for a grid.

**The symptoms, ranked by frequency:** (1) **hang on selection, not scroll** — the most common macOS trigger and the most surprising, persisting after reducing row and destination views to plain `Text`, so it is not a rendering cost but the container's diff/identity work; (2) hang on data *update* after a fine initial load; (3) CPU pinned before first paint; (4) frame drops in fast grid scrolling, attributed to per-cell `NSView` allocation rather than pool reuse; (5) `NSTableView` requiring predetermined cell sizes for accurate scroll-indicator positioning.

**One important qualification against a hasty AppKit decision:** two of the loudest cases had SwiftUI-level fixes — `.equatable()` plus a Release build, and a conditional inside `ForEach`. Both are pitfalls, not framework ceilings. **The genuinely irreducible cases are the 150,000-row `List` and the macOS 15.5 `Table` regression** — the latter being an argument for AppKit on *volatility* grounds rather than scale, which is a different and arguably stronger argument.

### Measurement tooling

The **SwiftUI instrument (Instruments 26)**, requiring Xcode 26 and current OS. Metrics that indicate a problem: red/orange updates; the frame deadline (16.67 ms at 60 Hz, 8.33 ms at 120 Hz); **body invocation *count*, not just duration** — the metric Apple's own optimisation story turns on; and resident memory across a down-and-back scroll as the recycling test.

**Two methodology rules that matter more than the tool choice:** profile **Release**, not Debug; and rebuild between runs with controlled device state.

### Swatch and sparkline rendering

**The documented pattern: precompute outside `body`, cache keyed by item ID, invalidate on the event that changes the input — never derive in `body`.** Apple's WWDC25 example is exactly this shape: expensive per-row derivation moved into an `@Observable` holding a `[Landmark.ID: String]` cache, populated from the location-update callback rather than from any view.

**For a swatch specifically:** the rendering cost is trivial; **the *derivation* cost is not** — raw payload → spectral reflectance → CIE XYZ → display triple is real arithmetic. **Precompute the display triple and store it as columns at measurement time**, alongside a gamut-clipped flag; `body` then reads three floats and fills a `Rectangle`. Storage cost is a handful of bytes per item, recomputation is a one-pass migration, **and it makes the swatch sortable and filterable in SQL**, which a computed-on-read value is not.

**The spectral curve is where the real risk lives.** Swift Charts degrades on macOS: ~20,000 points "chart setup takes time, window resizing is laggy"; 100,000 "can barely resize"; 500,000 unusable; and — the alarming one — **the vectorized `PointPlot` API "still exhibits 50–150 ms hangs even with 500–2,000 data points."** A 380–730 nm sweep at 10 nm is 36 points; at 1 nm, 351. **One curve is fine. The problem is *n* curves**: 1,200 sparklines × 36 points = 43,200 marks. Documented workaround: decimation off the main thread (LTTB — largest-triangle-three-buckets, by Sveinn Steinarsson; *the thesis itself is behind an hCaptcha wall, so cite the author's reference implementation, not a page number*).

**Recommended layering:**


| Scale                             | Approach                                                                                                                                                                                                                     |
| --------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Swatch in a grid/table cell       | Precomputed sRGB/P3 triple stored as columns; fill a shape. No caching layer                                                                                                                                                 |
| Sparkline per row                 | **Not a `Chart` per row.** Decimate to ~20–40 points at write time, store the decimated series, draw with a single `Canvas` or `Path` — or rasterise once via `ImageRenderer` (macOS 13+) into an `NSCache` keyed by item ID |
| Full-detail plot in the inspector | One `Chart` — a single instance, far below every reported threshold                                                                                                                                                          |


*Gap:* **no published measurement of many small `Chart` instances in a scrolling macOS container** — the Swift Charts evidence is all single-chart-many-points. Follow-up (~2 hours): a 5,000-row `List` with three row-curve variants (`Chart` with 40 marks, `Canvas` with a 40-point `Path`, cached `ImageRenderer` bitmap), profiled in Release under the SwiftUI instrument. **That settles the highest-uncertainty rendering decision in this app.**

---

## 4. Search, Filter, Facet, and Sort

**Live search — the latency budget is 0.1 s.** Nielsen's three limits: 0.1 s "about the limit for having the user feel that the system is reacting instantaneously, meaning that no special feedback is necessary"; 1.0 s for uninterrupted flow of thought; 10 s for holding attention. **Below 0.1 s no spinner or placeholder is warranted at all.**

**MEASURED against the 100,000-row corpus with an FTS5 external-content index:**


| Query                                                | Median       | Max      |
| ---------------------------------------------------- | ------------ | -------- |
| FTS5 prefix `MATCH 'phth*'` LIMIT 200                | **0.49 ms**  | 0.52 ms  |
| FTS5 two-term + `ORDER BY rank` LIMIT 50 (bm25)      | **7.37 ms**  | 7.73 ms  |
| Unindexed `LIKE '%phthalo%'` over all 33,384 matches | **10.66 ms** | 10.96 ms |


**At 10³–10⁵ the query is two orders of magnitude inside budget.** Debouncing is not required to *meet* it — only to avoid wasted work. **The realistic budget violator at 10⁵ is the SwiftUI re-render and any main-thread database access, not the SQL.** Instrument the render, not the query.

**API surface, with versions:** `searchable(text:placement:prompt:)` **13.0**; `searchScopes(_:scopes:)` **13.0**; `searchSuggestions(_:)` **13.0**; `searchable(text:editableTokens:…)` **14.0**. HIG prescribes: start search immediately on typing; placeholder text communicating scope; suggested terms; prioritised and categorised results; a **scope bar** ("Default to a broader scope and let people refine it") and **tokens** ("a visual representation of a search term that someone can select and edit").

**Combining free-text with structured filters.** NN/g defines the filter/facet distinction and warns explicitly: faceted navigation is "significantly more expensive to create and maintain" and adds "interaction cost by presenting users with more options to comprehend and manipulate," whereas "a simple filter can often be easier to understand and faster to use." Adopt facets only for very large sets across multiple dimensions, "only after confirming users genuinely need it."

*A figure the agent checked and declined to cite:* a "25–50% faster task completion with faceted navigation" claim attributed to NN/g could not be retrieved from any primary page. **Unverified.**

**macOS's native answer folds the structured filter *inside* the search field** — scope bars for coarse categories, tokens for term-level filters. For a full rule builder the shipped control is `NSPredicateEditor` (**macOS 10.5+**), the machinery behind Finder's Find window and Mail's smart mailboxes — **it has no SwiftUI equivalent** and needs `NSViewRepresentable`.

**Recommended three-tier layout:** one always-visible free-text field over FTS5; a small fixed rail of 3–5 low-cardinality facets (brand, medium, collection, gamut-renderable flag) mapping to indexed columns — the ones NN/g's cost warning permits; and a separately-invoked advanced rule builder for numeric/date ranges, kept out of the default view. **Tokens are the seam**: promoting an advanced rule into a token puts it back in the search field so both systems share one visible state.

**FTS5 — the best-documented question in the set.** In the amalgamation since **3.9.0 (2015-10-14)**; contentless-delete requires **3.43.0**; `secure-delete` requires **3.42.0**. *Orchestrator-verified on this machine: macOS 26.6's SQLite 3.51.0 reports `ENABLE_FTS5` and `ENABLE_RTREE`, and a trigram FTS5 substring match works.*

**Tokenizers:** `unicode61` (default, case-insensitive, **removes diacritics from Latin characters by default**, options `remove_diacritics`/`categories`/`tokenchars`/`separators`); `ascii`; `porter`; `trigram` (every contiguous 3-character sequence). **For pigment names like "Cadmium Yellow Deep," `unicode61` with a widened `tokenchars` (to keep hyphens and codes like `YR-04` intact) is the configuration that matters. `porter` is a liability** — stemming "Deep"/"Deeper" together is not obviously desirable for product names.

**Trigram and the `LIKE` seam:** a trigram index optimises `LIKE`/`GLOB` **only if `remove_diacritics` is not set**; with `case_sensitive=1` only `GLOB` is optimised; not at all with an `ESCAPE` clause; substrings under three characters fall back to a linear scan. Cost: "a larger index."

**External content** (`content='item', content_rowid='id'`) stores index entries only and **you must keep it in sync yourself** via `AFTER INSERT/DELETE/UPDATE` triggers using the `'delete'` command form with the *old* values.

**Index size — SQLite's own real-world figure:** 1636 MiB of email → 743 MiB index at `detail=full`, 340 MiB at `detail=column` (≈54% smaller), 134 MiB at `detail=none` (≈82% smaller), at the cost of NEAR/phrase and then column-filter queries. **MEASURED here:** a three-column external-content index over 100,000 rows built in **0.11 s** and occupied **7.37 MB** against a 29.3 MB base table — **~25% overhead at `detail=full`.**

**Maintenance:** `automerge` default 4, `crisismerge` default 16; `optimize` "can be slow"; `merge N` does bounded incremental work, and **the documented way to tell whether a merge did anything is to compare `sqlite3_total_changes()` before and after — a difference under 2 means no-op.**

**The `LIKE` fallback decision:** without a trigram index, `LIKE '%x%'` is a full scan (MEASURED 10.66 ms over 100k). Two coherent designs — (a) one `unicode61` index plus an unindexed `LIKE` scan for the rare true-substring case, or (b) a second, much larger trigram index. **At 10³–10⁵ option (a) already fits inside the 100 ms budget with room to spare, so (b) buys nothing but index size and write cost. Build (a).**

**Saved searches.** Two shipped representations at opposite ends: **an opaque predicate string** (macOS Smart Folders store a `RawQuery` key in a plist; "it doesn't even list the files 'contained' by this 'folder'" — hence "just a few KB" and dynamic contents) and **a structured criteria document** (Navidrome's `.nsp` JSON with `all`/`any` arrays of operator objects — `is`, `gt`, `lt`, `contains`, `inTheRange`, `before`, `after`, `inTheLast`, `inPlaylist`, `isMissing`, `isPresent`). **The common property: neither stores a result set**, which is why adding rows never invalidates a saved search.

*Honest gap:* **neither source documents what happens when a referenced field disappears**, and Navidrome's format carries **no version field**. So the "survives schema changes" half has no documented answer. **The design that closes it:** structured JSON keyed on stable field *identifiers* rather than column names, with an explicit `schema_version`, compiled to SQL at query time, validated on load against a field registry, marking a saved search "needs attention" rather than silently returning zero rows. **Storing raw SQL is the one representation that provably cannot survive a column rename**, because nothing in the store can detect the break.

### Colour-proximity search: the counter-intuitive answer

**MEASURED — exhaustive exact ΔE00.** CIEDE2000 written in C, **validated against Sharma, Wu &amp; Dalal's (2005) test pair 1 (computed 2.0425, expected 2.0425)**, compiled `-O2`, single-threaded:


| Corpus            | Full exhaustive ΔE00 scan |
| ----------------- | ------------------------- |
| 1,000 Lab triples | **0.277 ms**              |
| 10,000            | **1.933 ms**              |
| 100,000           | **9.500 ms**              |


**MEASURED — SQL-side alternatives on the same table:**


| Strategy                                                       | Median                                |
| -------------------------------------------------------------- | ------------------------------------- |
| `ORDER BY` squared Euclidean `LIMIT 20` (full scan)            | 12.64 ms                              |
| Lab bounding-box `BETWEEN`, no index                           | 6.28 ms                               |
| R-tree box query                                               | 0.01 ms                               |
| **R-tree prefilter (±10 box → 162 candidates) + exact rerank** | **0.06 ms** (~250× the SQL full scan) |
| R-tree build over 100,000 points                               | 327 ms one-off; DB 29.3 → 46.6 MB     |


**SQLite's RTree supports 1–5 dimensions** (3-D Lab needs 7 columns) and its documentation is explicit that it is a prefilter: "An RTree index does not normally provide the exact answer but merely reduces the set of potential answers from millions to dozens." It also warns that simultaneous reads and writes to the same R-tree can fail with `SQLITE_LOCKED`.

**The correctness trap most write-ups miss.** A Lab bounding box is a *sound* prefilter for ΔE76. **It is not provably sound for ΔE00**, because the S_L, S_C, S_H and R_T weighting terms can make a pair with larger Euclidean separation have smaller ΔE00 — and Sharma et al. document that the formula has **discontinuities** and does not reliably satisfy the triangle inequality. Any R-tree prefilter for ΔE00 needs a generously oversized box and an accepted approximate-recall bound.

**Recommendation: build brute force, in application code, with no index.** At the motivating 10³ scale exact exhaustive ΔE00 costs **0.28 ms**; at the architectural ceiling of 10⁵ it costs **9.5 ms** — inside Nielsen's instantaneity limit with 10× headroom. Store L, a, b as three plain `REAL` columns; that alone keeps every other option open. **Add the R-tree only if you are at 10⁵ *and* have measured brute force as the actual bottleneck *and* have accepted the recall approximation. Building it first is the textbook case of solving a problem you do not have.** (`sqlite-vec` is strictly overkill for a 3-dimensional problem.)

**Interpretive thresholds** for the UI, from an instrument vendor: ΔE &lt; 1 "imperceptible to the human eye"; 1–2 "barely noticeable under close inspection"; ≥3 "clearly visible difference, often unacceptable depending on industry."

### Sort stability — a sharp edge

**SQLite's `ORDER BY` is explicitly NOT stable.** "Rows are first sorted based on the results of evaluating the left-most expression… **The order in which two rows for which all ORDER BY expressions evaluate to equal values are returned is undefined.**" NULLs sort smaller than any other value; `ASC NULLS LAST` / `DESC NULLS FIRST` is available (*verified working on 3.51.0*).

**Consequence — load-bearing:** **you must append a unique tiebreaker (the primary key) to every `ORDER BY`.** Without it, two evaluations of the same query can order equal-key rows differently, surfacing as rows visibly swapping under the user during incremental refresh and as broken scroll and selection restoration. **Not theoretical in a colour catalogue**, where sorting by `brand` or by L rounded to an integer produces large tie groups.

**By contrast, Swift's in-memory sort IS stable** — both `sorted(by:)` and `sort()` guarantee it, so repeated stable sorts compose into multi-key ordering for free.

**Persisting sort state:** `TableColumnCustomization` (**macOS 14.0**) is `Codable` and bindable to app or scene storage — but **it persists order and visibility, not the sort-comparator array**, which is yours. And "**If a table column does not have a customization identifier, it will not be customizable.**"

---

## 5. Selection and Bulk Operations

**A documented absence that materially constrains the design:**


| Container       | Native selection                                          | Introduced            |
| --------------- | --------------------------------------------------------- | --------------------- |
| `List`          | `SelectionValue?` single / `Set<SelectionValue>` multi    | **macOS 10.15**       |
| `Table`         | row value (single) / `Set<Value.ID>` (multi)              | **macOS 12.0**        |
| `**LazyVGrid**` | **none — no `selection` parameter exists at any version** | container itself 11.0 |


`LazyVGrid`'s initialisers accept only `columns`, `alignment`, `spacing`, `pinnedViews`, `content`. Community reports corroborate, including for multi-item drag.

**What each gesture costs:** click, ⇧-click range, ⌘-click toggle and ⌘A come free with `List`/`Table` because they are AppKit-backed. **Marquee (rubber-band) selection is provided by neither** — it is an `NSCollectionView`/`NSTableView` behaviour. "Select-all-matching-filter" is not a framework concept in any of the three.

**The design consequence:** the visual-first browse mode a colour catalogue most wants — a grid of swatches — is **precisely the container SwiftUI gives no selection for.** Three honest options: (1) hand-build selection over `LazyVGrid` (no marquee, no keyboard arrow navigation); (2) use `List`/`Table` with a wide row carrying a large swatch, keeping native selection, keyboard navigation and ⌘A; (3) drop to `NSCollectionView` via `NSViewRepresentable`. **Option (2) is the only one that is free.**

Related modifiers: `selectionDisabled(_:)` **14.0**; `contextMenu(forSelectionType:menu:primaryAction:)` **13.0**; `DisclosureTableRow` **14.0**.

**Select-all across a filtered set.** No framework supports it; one shipped pattern dominates — **Gmail's two-stage promotion**: "All 50 conversations on this page are selected. **Select all 2,000 conversations in Inbox.**" Two properties carry the whole pattern: the page-scoped and query-scoped selections are **visually distinct states with an explicit promotion step** (you cannot arrive at the 2,000-item selection by accident), and the promoted state is **labelled with its exact count**, so the blast radius is stated before the action.

**The representation:** not a set of IDs but *the predicate plus a count*:

```
.explicit(Set<ID>)
.allMatching(query: Query, exclusions: Set<ID>)
```

The `exclusions` set is what lets a user deselect three items out of 40,000 without materialising 39,997 IDs. **Execution follows: an `.allMatching` selection compiles to a set-based `UPDATE … WHERE <predicate> AND id NOT IN (…)` inside one transaction, never a loop over IDs.** The only representation that is O(1) in memory and statement count.

**Constraint:** `List`/`Table` accept only `Set<…>` as their selection binding, so **the promoted state must live beside the view's binding and the view must render "all selected" itself.**

**Bulk edit and mixed values.** `NSControl.StateValue.mixed` exists for exactly this — "if a checkbox displays the state of more than one selected row… and the items have different states, then the checkbox's state is mixed." **SwiftUI's `Toggle` has no mixed state** — it binds a `Bool` — so a tri-state bulk checkbox must be hand-built or bridged.

**Lightroom's convention is the shipped one:** `<mixed>` in a field when selections differ, and — the more interesting half — **scope is an explicit persistent mode**, not a per-edit prompt. A Targeted Photo / Selected Photos switch appears only with multiple selections and governs both display and editing. **The two coherent designs for communicating partial application** are (a) an explicit persistent scope switch, visible before the user types, or (b) a per-commit confirmation naming the count ("Apply 'Cadmium Yellow' to 412 items?"). **Mixing them is what produces the "I didn't mean to change all of them" error class**, because the user learns to dismiss the prompt and then the switch silently governs.

**Progress and cancellation.** HIG is unusually specific: prefer determinate; "Be as accurate as possible… **Showing 90 percent completion in five seconds and the last 10 percent in 5 minutes can make people wonder if your app is still working and can even feel deceptive**"; "**People tend to associate a stationary indicator with a stalled process**"; switch indeterminate→determinate when possible but **"Don't switch from the circular style to the bar style"**; include Cancel when interruption is safe, **and a Pause alongside it when interruption has negative side effects**; alert with a confirm/resume option when cancelling loses progress; **"Avoid vague terms like *loading* or *authenticating* because they seldom add value."** `ProgressView` is **macOS 11.0**; `Task.checkCancellation()` **10.15**.

**The part the HIG cannot tell you, and the part that decides responsiveness:** for a local-SQLite bulk write this is a **transaction-shape** decision, not a UI one. A single transaction over 40,000 rows holds a write lock for its duration, produces no intermediate progress, and **cannot be partially cancelled** — all or nothing. Chunked transactions (e.g. 500 rows, checking `Task.isCancelled` and yielding between chunks) give a real progress fraction and a real cancel point, **at the cost of leaving the operation partially applied if cancelled** — exactly the "negative side effect" the HIG says to warn about. **The two shapes are not interchangeable; choose deliberately per operation.**

**Reversibility — SQLite documents a technique that fits.** Its own trigger-based undo/redo: a `TEMP TABLE undolog(seq, sql)`; three TEMP triggers per table writing the **inverse** SQL (an `AFTER INSERT` writes a `DELETE`, an `AFTER UPDATE` writes an `UPDATE` restoring old values, a `BEFORE DELETE` writes an `INSERT`), all escaped through `quote()`; application-held `undostack`/`redostack` of `[begin, end]` ranges with a `barrier` call so **many row changes between barriers form one undo step** — that is the mechanism making a 40,000-row bulk edit a single ⌘Z. Undo replays in reverse `seq` order inside a transaction.

**Why this answers "too large for a naive undo stack":** the undo state is *rows in the database*, not objects on a heap. A 40,000-row undo step costs 40,000 log rows on disk and **O(1) memory in the app process**.

`UndoManager` reaches SwiftUI via `@Environment(\.undoManager)` (**10.15**), documented as `nil` "when the environment represents a context that doesn't support undo." HIG: name the result in the menu item ("Undo Set Brand for 412 Items"); **"it's crucial to highlight the result of each undo and redo to keep people from thinking that the action had no effect, which can lead them to perform it repeatedly"**; allow multiple undos; support ⌘Z and ⇧⌘Z.

**A scope reduction specific to SpectroCapture:** the commitment that re-scanning never destroys the prior value means **the measurement path needs no undo at all — history *is* the undo.** Undo is required only for metadata and organisational operations: bulk field edits, collection membership, deletes. **A far smaller surface than it first appears**, and the trigger-based undolog covers it exactly.

---

## 6. Editing Surfaces

**Evidence quality, stated plainly: what exists is normative platform guidance, not controlled experiment.** No published study compares error rates or task times across inline / inspector / modal metadata editing.

**The HIG's definition of an inspector is the decisive criterion:** "An inspector displays the details of the **currently selected item, automatically updating its contents when the item changes or when people select a new item.** In contrast, if you need to present an Info window — which always maintains the same contents — use a regular window." **Browsing is a continuous stream of selection changes**, so the selection-following definition settles it. A modal sheet breaks that loop by construction; the HIG reserves modality for a "distinct, narrowly scoped task" and warns "Take care to avoid creating a modal experience that feels like an app within your app."

**So: inline for a single low-risk field edited in place; inspector for the item's whole metadata record (the default in a catalogue); modal only for tasks that must complete or abort atomically** (an import column-mapping step, a destructive confirmation). The HIG's caution against typing-heavy controls in a *floating panel* does not extend to a split-view inspector pane, which the HIG itself offers as the alternative.

`**.inspector(isPresented:content:)` is macOS 14.0+**, iOS/iPadOS/Mac Catalyst 17.0+, not on tvOS/watchOS. "Trailing column inspectors have their presentation state restored by the framework." `inspectorColumnWidth(min:ideal:max:)` is also **14.0**.

**Composition with `NavigationSplitView` — placement changes the result materially.** On macOS the inspector presents as a **full-height trailing sidebar and never as a sheet**. Applied to the *detail content*, it nests as a column within the detail, **underneath** the navigation structure's toolbar. Applied to the **whole split view**, it becomes part of the window-level layout: full height, with its own toolbar section. On width: `ideal` sets the first-launch default, user resizing persists across launches, and **the inspector is not resizable at all unless you apply the modifier.**

**Keyboard-driven table editing — barely supported, and the gap is confirmed by an unanswered Apple forum question.** `Table`'s documentation covers construction, scrolling, selection, sorting, styles and size-class adaptation, and **says nothing about a cell-editing lifecycle**. The pieces you must assemble yourself: `FocusState` **12.0**, `onSubmit(of:_:)` **12.0**, `onExitCommand(perform:)` **10.15** (delivers the event but **does not restore the prior value**), `onKeyPress(_:action:)` **14.0**.

And the default macOS behaviour actively fights inline editing. An Apple Developer Forums question, May 2025: "if I embed a `TextField` then the text field is presented as non-editable. If the user clicks on the text and waits a short period of time, the text field will become editable… **is there a way in SwiftUI to suppress this behaviour such that the TextField is always presented as being editable?**" — **zero replies, including from Apple.**

**Consequence:** spreadsheet-grade keyboard editing in SwiftUI `Table` on macOS is a hand-built system **with no supported API for the always-editable cell state it requires**. `NSTableView` provides these natively. **A real decision point: if heads-down keyboard metadata entry across a table is a core workflow, that argues for AppKit; if editing happens item-at-a-time in an inspector, SwiftUI's `onSubmit` + `FocusState` + `onExitCommand` is sufficient.**

**Autosave vs explicit commit.** **No controlled study exists** measuring error rates in a data-management context — anyone claiming a measured answer should be asked for the study. What exists is guidance, and **GitHub's Primer draws the sharpest line — by *control type*, not risk level:** explicit saving is the default ("Start here for forms," "Never mix explicit and autosave in one form," "Preserve user data if errors occur"); automatic saving applies "**only for imperative controls: ToggleSwitches, SegmentedControls, single-select dropdowns**," and is to be avoided for "declarative controls (text inputs, checkboxes, radio buttons, multi-select)" — because text inputs "risk unintended submission of sensitive data" and checkbox/radio groups can be clicked accidentally while navigating.

**macOS's opposite default has a precondition:** NSDocument apps autosave in place, and `browseVersions(_:)` (**10.8+**) is the Browse Saved Versions action. **Version history is not decoration here — it is what makes autosave recoverable.** The two positions reconcile: **autosave is defensible exactly to the extent that every autosaved state is recoverable.**

**For SpectroCapture:** the app already commits to non-destructive history for measurements. Extend that versioning to metadata edits and autosave becomes defensible for metadata too. **The exception is bulk edits over a large selection** — those are the "destructive action" Primer says requires confirmation, because they are hard to notice and expensive to reverse by hand.

**Validation — three constraint classes need three different mechanisms.** HIG: "**Dynamically validate field values.**… For numeric data in particular, consider using a number formatter," and prevention over correction: "**When possible, offer choices instead of requiring text entry**" — a picker cannot produce an invalid value at all.

- **Format** — validate per keystroke in the view layer, show inline.
- **Uniqueness** — cannot be checked reliably against a possibly-stale snapshot; **enforce in the schema.** SQLite supports partial `UNIQUE` indexes since **3.8.0 (2013-08-26)**. *Orchestrator-verified: a partial unique index correctly rejected a second current row with `UNIQUE constraint failed`.*
- **Referential integrity** — *orchestrator-verified: `PRAGMA foreign_keys` returns **0** on macOS 26.6's system SQLite.* **Enforcement is off unless you turn it on per connection**, so a missing `PRAGMA foreign_keys = ON` silently disables every `REFERENCES` clause in the schema.

**The SwiftUI trap (INFERRED from the API's shape, not documented):** the formatter-bound `TextField(value:format:)` family enforces format by **reverting the field** when parsing fails — the opposite of preserving input. Keep a `String` draft in view state, validate the draft, and write through to the model only on a successful parse. Verify against your own build before relying on it.

---

## 7. Non-Destructive Editing and Version History

**Snapshot-per-version is standardised, precisely, as SQL:2011 system versioning.** The mechanics: INSERT sets `Sys_start` to the transaction timestamp and `Sys_end` to the type's highest value; "UPDATE and DELETE on system-versioned tables **only operate on current system rows**. Users are not allowed to update or delete historical system rows"; both "result in the **automatic insertion of a historical system row**"; a DELETE "does not actually delete the qualifying rows; instead it changes the system-time period end time." **Query complexity is documented as low** — a query specifying none of the temporal options is "assumed to specify `FOR SYSTEM_TIME AS OF CURRENT_TIMESTAMP`" and returns only current rows. **Constraint complexity likewise:** "constraints on system-versioned tables need only be enforced on the current system rows. **Historical system rows form immutable snapshots of the past.**"


| Model                               | Write cost          | "Current" query                        | History query            | Storage per change        |
| ----------------------------------- | ------------------- | -------------------------------------- | ------------------------ | ------------------------- |
| Append-only event log               | 1 insert            | Replay, or a projection                | Natural                  | Smallest for small deltas |
| **Snapshot-per-version (SQL:2011)** | 1 insert + 1 update | One predicate, default in the standard | One range predicate      | One full row per version  |
| Current + diff                      | 1 insert + 1 update | Trivial                                | Reconstruct, O(versions) | Smallest                  |


*(Row 2 is DOCUMENTED; rows 1 and 3 are INFERRED characterisations.)*

***Orchestrator-verified: SQLite implements none of it** — `PERIOD FOR`, `WITH SYSTEM VERSIONING`, and `FOR SYSTEM_TIME AS OF` all fail with syntax errors on 3.51.0.* **SQL:2011 temporal syntax is a design template here, not a feature you can switch on.**

**The choice for this app:** a measurement is not a small diff — it is a whole new reading with its own raw payload and spectral curve, and **there is no meaningful "diff" between two independent instrument readings**, which eliminates current-plus-diff. And **the "event" *is* the snapshot**, collapsing append-only-log and snapshot-per-version into the same thing. **Snapshot-per-version, following the SQL:2011 shape, has no real competitor for this workload.**

**Exposing history without clutter — two shipped answers.** macOS Versions: "Using the File/Revert To/Browse All Versions… command displays all saved versions using an interface similar to that of the Time Machine app." Two properties worth stealing: **the entry point is a menu command, not persistent chrome** (history costs zero pixels until asked for), and **reverting is itself non-destructive** — "older versions are still retained in the version database, so you can revert back to any of those versions later." Google Docs adds the answer to the *other* clutter problem: **named versions** (up to 40 per Doc) with an "Only show named versions" toggle — let the user promote the meaningful ones and filter to those, because most versions are noise.

*Not documented:* whether restoring destroys later versions, or the retention window. Checked; absent.

**For SpectroCapture:** one affordance in the inspector — "3 measurements" — opening a history list; nothing else in the primary view. **Annotate each entry with its date, instrument/conditions, and its ΔE00 from the current canonical value** — the domain-native diff, far more informative than a timestamp. "Compare" renders two swatches plus two overlaid spectral curves. **"Revert" should write a *new* version whose value equals the old one, never delete rows** — matching both Apple's retain-on-revert behaviour and the app's own commitment.

**Keeping "current" fast as history grows.** SQLite **partial indexes** (3.8.0+): "only some subset of the rows have corresponding index entries," with smaller file, faster traversal, and cheaper writes. So:

```sql
CREATE UNIQUE INDEX one_current ON measurement(item_id) WHERE valid_to IS NULL;
```

keeps the index at exactly one entry per item **regardless of how deep history grows**, and additionally **enforces** the invariant that exactly one current row exists per item.

**The planner constraint that will silently defeat you:** SQLite matches partial-index `WHERE` terms **exactly** — "SQLite performs no algebraic transformation (e.g. 'b=6' won't match 'b-6=0')." **Every "current" query must literally spell `WHERE valid_to IS NULL`.** A query written `WHERE valid_to > date('now')` or `WHERE is_current = 1` against that index will **silently full-scan**. **Precisely the class of defect a green test suite will not catch, because the results are correct — only the plan is wrong.**

**Heavy payloads.** Two shipped strategies: **copy-on-write clones** (macOS stores versions in `.DocumentRevisions-V100`; "on APFS volumes at least, that no file data is actually copied, but that the database contains… clone blocks for that version") — with a real cautionary note, "**Old versions are retained indefinitely… There's also no direct way to empty the version database**"; and **content-addressing plus delta packing** (Git stores the *newest* version intact and the older as a delta, "because you're most likely to need faster access to the most recent version" — a 22,044-byte file and its one-line successor packed to a 7K packfile with the older stored as a **9-byte** delta).

**SQLite's blob-siting arithmetic:** for BLOBs **smaller than about 100 KB, storing them inside the database is faster** than separate files; above that, files win. Optimal page sizes for large-BLOB I/O are 8192 or 16384 bytes.

**MEASURED — and the result contradicts the intuitive answer.** Three databases, 1,000 items × 10 versions = 10,000 rows, each with a 144-byte spectral curve plus a ~3.2 KB raw payload:


| Strategy                                | Size                                            |
| --------------------------------------- | ----------------------------------------------- |
| Full snapshot per version, raw blobs    | **41.06 MB**                                    |
| Full snapshot per version, zlib level 6 | **20.53 MB** (50% reduction)                    |
| Content-addressed (SHA-256) + zlib      | **22.20 MB** — **worse than plain compression** |


**Content addressing came out worse because every payload is unique** — it bought zero deduplication while costing 32 bytes of hash per reference plus a b-tree. Extrapolating: 10⁵ items × 10 versions ≈ **4.1 GB raw / 2.05 GB compressed**.

**Conclusion:** at 10³ this is ~4 MB and the question doesn't arise. At 10⁵ the compression decision is worth ~2 GB and **the deduplication decision is worth approximately nothing**, because independent readings will not repeat byte-for-byte. **Full snapshot per version; compress the raw payload; do not build a content-addressed store; leave the 144-byte spectral curve uncompressed** (compression is pure overhead at that size, and it is the field you'll read in bulk for charting). **And take Apple's unbounded-growth experience as a warning: decide the retention policy up front**, because retrofitting one onto a database with no way to enumerate or empty its history is the corner macOS painted itself into.

### Correction vs re-measurement — bitemporality is the exact standardised answer

**The two time axes**, from SQL:2011: "**valid time**, the time period during which a row is regarded as correctly reflecting reality by the user of the database [and] **transaction time**, the time period during which a row is committed to or recorded in the database."

**The standard's own worked example is exactly this distinction:** "an employee may change names. Typically the name changes legally at a specific time (for example, a marriage) but the name is not changed in the database concurrently… **the system-time period automatically records when a particular name is known to the database, and the application-time period records when the name was legally effective.**"

**The mapping:**

- A **re-measurement** — the marker faded; the sample genuinely differs now — is a change in **valid time**. A new application-time period opens; the prior closes. ***Both values were true*, at different times.**
- A **correction** — the first scan was taken with a dirty aperture; that value was never true — is a change in **transaction time only**. The old row's system-time period closes, a new row opens, and **the application-time period is unchanged**.

A `measurement` row therefore wants four fields: `measured_at` (valid time), `recorded_at` (transaction time), `superseded_by` (nullable FK), and `reason ∈ {initial, remeasurement, correction}`.

**The `reason` cannot be inferred from the data** — two scans a week apart look identical whether the marker faded or the first was botched. **That is the whole point, and it means the UI must *ask* when a new scan lands on an item that already has one. A silent default would fabricate the distinction.**

**The UI consequence, which is where this meets the app's stated ethics:** a re-measurement **extends a timeline** — both readings are legitimate data points. A correction **supersedes**: the earlier value renders struck-through or greyed, labelled "superseded," and **must be excluded from any colour-over-time chart.** Plotting a reading known to be wrong is the same species of visual dishonesty the vision spec forbids in the gamut-marking requirement. **Getting this wrong produces a chart that lies with entirely real numbers.**

---

## 8. Colour Rendering and Visual Honesty

**The correct macOS pipeline: tag the measured value with a device-independent space → hand it to a colour-managed framework → let ColorSync match it to the display profile.**

- **Build a space for the measurement.** `CGColorSpace.init?(labWhitePoint:blackPoint:range:)` — "Creates a device-independent color space… according to the CIE Lab standard," taking the diffuse white point as "3 numbers that specify the tristimulus value, in the CIE 1931 XYZ-space" — available from **macOS 10.0**. **You supply your own white point**, so a D50- or D65-referred measurement is represented honestly. Named spaces: `genericXYZ` **10.11**, `genericLab` **10.13** — but the named constants give no white-point control.
- **Make a colour**: `CGColor(colorSpace:components:)`, or `NSColorSpace.init(cgColorSpace:)` then `NSColor.init(colorSpace:components:count:)`.
- **Convert explicitly, only when you need the numbers**: `CGColor.converted(to:intent:options:)` (**10.11**), whose `intent` is "The mechanism to use to match the color when the color is outside the gamut of the new color space." Its AppKit twin `NSColor.usingColorSpace(_:)` "**Returns `nil` if conversion is not possible**" — **one of the few places AppKit admits a conversion failed rather than silently approximating. Treat `nil` as a finding, not an error to swallow.**
- **Or do nothing.** TN2313: on macOS colour management is *active* — "For each pixel a color match is performed from the source content's profile space to the destination space," so "properly tagged content requires no code to display properly."

**Where the mistakes are**, named in TN2313: untagged pixel buffers ("Pixel data that is not tagged is considered undefined"); under-specified graphics contexts; **device RGB** ("RGB device values without any color space information really tell you nothing… these spaces are actually the worst choice for faithful color reproduction"); **not noticing the window moved to another display** (`[myWindow setDisplaysWhenScreenProfileChanges:YES]` — for a multi-monitor colour app this is load-bearing, since a swatch in gamut on the laptop panel is not necessarily in gamut on the external); and **the lossiness itself** — "Color conversions are non-linear. **Conversion to a smaller gamut can cause irreversible damage.**"

**Rendering-intent default:** "If you do not explicitly set the rendering intent… the graphics context uses the **relative colorimetric** rendering intent, except when drawing sampled images." For a swatch grid you almost certainly want relative colorimetric **deliberately** rather than by accident.

### Gamut containment — the honesty badge

**Getting the display's profile — four routes:** `CGDisplayCopyColorSpace(_:)` **10.5** ("returns a display-dependent ICC-based color space… to produce color-matched output for that display"); `NSScreen.colorSpace` **10.6**; `ColorSyncProfileCreateWithDisplayID()` **10.4**; `NSScreen.canRepresent(_:)` **10.12**.

**A trap:** `NSScreen.canRepresent(_:)` looks like a gamut test and is not — it takes an `NSDisplayGamut` **enum** (sRGB/P3) and answers "is this screen at least P3-class?", not "can this screen show *this* colour?" Likewise `CGColorSpace.isWideGamutRGB` only "Returns whether the RGB color space covers a significant portion of the NTSC color gamut." **Building the honesty badge on either would be exactly a check that reports a verdict without evaluating its subject.**

**The actual containment test — two mechanisms:**

**(a) ColorSync's gamut-check transform.** `kColorSyncTransformGamutCheck` is "A `kColorSyncTransformTag` value that checks whether colors fall outside the destination gamut," one of the legal values passed into `ColorSyncTransformCreate(_:_:)`.

**The crucial detail is visible only in the SDK header, not the web docs: the output format.** `ColorSyncTransform.h` declares `kColorSync1BitGamut = 1` as the first `ColorSyncDataDepth` value — **one bit per sample, in-gamut or not.** *Orchestrator-verified on this machine at `ColorSyncTransform.h:72`.* The executable recipe: build a transform from `[Lab-or-XYZ profile, display profile]` with the gamut-check tag, call `ColorSyncTransformConvert` with `dstDepth = kColorSync1BitGamut`, read the bit. **For a 1,200-item collection you can batch every swatch through a single `width × 1` conversion.**

**Version discrepancy — report both, believe the header.** Apple's web docs stamp `kColorSyncTransformGamutCheck` and `ColorSyncTransformCreate` as **macOS 10.13+**; the macOS 26.5 SDK header annotates both `CS_AVAILABLE_STARTING(10.4, 16.0)`. *Orchestrator-verified at `ColorSyncTransform.h:158`.* **The header is what the compiler enforces.**

**(b) The ICC `gamt` tag.** `kColorSyncSigGamutTag` is declared `/* 0x67616D74L => CFSTR("gamt") */` (*verified at `ColorSyncProfile.h:80`*), extractable via `ColorSyncProfileContainsTag`/`ColorSyncProfileCopyTag`. **Real caveat:** the `gamt` tag is a cLUT and is *optional* — matrix/TRC display profiles, which most calibrated monitor profiles are, generally do not carry one. `ColorSyncProfileIsMatrixBased()` (**11.0+**) detects that case. **A bonus path, not the primary one.**

**(c) The pragmatic third method (INFERRED, but every component documented):** for a matrix-based profile, convert the measurement to the display's space with relative colorimetric intent **and without clipping**, and test whether any component falls outside [0, 1]. `extendedSRGB` "has the same colorimetry as sRGB, but you can encode component values below `0.0` and above `1.0`," and ColorSync exposes `kColorSyncConvertUseExtendedRange` (**11.0+**) to "allow float data to exceed [0.0 .. 1.0] range." **Without that flag the conversion clamps and the evidence of out-of-gamut-ness is destroyed — which is exactly the silent lie the product exists to expose.**

**The named gap:** Apple documents *that* the gamut check exists and *what it is for*, and the header reveals the 1-bit depth, but **publishes no worked example and no statement of which rendering intent the check is evaluated against.** Argyll's discussion is a reminder this is not trivial: gamut mapping lives in the BtoA tables and differs per intent, so **"in gamut" is intent-relative, not absolute.** **Follow-up (half a day):** a 30-line CLI harness building a transform from `genericLab` → `/System/Library/ColorSync/Profiles/sRGB Profile.icc` with the gamut-check tag, fed a Lab sweep along the sRGB boundary (primaries converted to Lab, scaled 0.9×/1.0×/1.1× in chroma), printing the returned bit — repeated across all four `kColorSyncRenderingIntent*` values. **Do not ship the honesty badge without running it.**

**Marking out-of-gamut colours — the dominant convention is colour substitution, not annotation.** Krita's manual: the warning "allows you to see which colors are being clipped, by replacing the resulting color with the set alarm color." Three properties to import: **it is destructive-by-display, on purpose** — the pixel is *replaced*, not overlaid, so the user cannot mistake the rendered colour for the true one; **the alarm colour is user-configurable and persisted with the document**, because any fixed warning colour is invisible against an image containing that colour, and **a colour-sample catalogue is the worst possible case for a fixed alarm colour**; and it layers on top of soft-proofing rather than replacing it.

Krita also documents two failure modes directly relevant: "**Soft Proofing doesn't work properly in floating-point spaces**, and attempting to force it will cause incorrect gamut alarms," and "**Gamut Warnings sometimes give odd warnings for linear profiles in the shadows. This is a bug in LittleCMS.**" **Since SpectroCapture works with float Lab/XYZ in a linear-ish pipeline, both conditions apply — strong evidence for building on ColorSync rather than importing LittleCMS.**

**A known weakness of colour substitution:** by the time the user sees the canvas the colours have *already* been brought into gamut by the rendering intent. **A gamut warning marks "this was moved," not "this is what you will get."** That argues for marking swatches with a **persistent badge tied to the item**, not a transient view mode — because renderability is a property of the datum, not of a display mode the user toggles.

**The evidence half — an honest void.** **No usability or psychophysical evidence exists on whether users correctly interpret out-of-gamut indicators.** The closest peer-reviewed work, Henry &amp; Westland (2020), *Coloration Technology* 136(3), is genuinely adjacent — it names "the *gamut issue*" as a barrier and reports "in the art and design community there is often a level of dissatisfaction and deep cynicism about colour management" — but its measured outcomes concern **colour pickers and subtractive-vs-additive mixing prediction, not indicator comprehension.** Citing it as evidence for comprehension would be a misattribution. **The gap is not retrieval; professional colour tools ship these affordances as craft convention and vendors do not publish usability data.**

**Follow-up:** a within-subjects comprehension test, 12–15 participants from the actual persona. Same 20-swatch grid rendered four ways — alarm-colour substitution, corner badge, diagonal hatch, unmarked control — with two scoring questions per condition: "point to every swatch your monitor cannot show accurately" (accuracy) and "**for the swatch you just pointed at, is the colour you see too saturated, not saturated enough, or you can't tell?**" (mental model). **The second question is the one that matters — it separates "noticed the marker" from "understood what it means," which the vendor-convention literature never tests.**

**Wide gamut.** Apple offers two representations: **Display P3** ("DCI P3 primaries, a D65 white point, and the sRGB transfer function," **10.11.2**) and **Extended Range sRGB** (same primaries, "**Negative values and values greater than 1**," **10.12**) — with the identity shown directly: Display P3 `{1.0, 0.0, 0.0}` = Extended sRGB `{1.358, -0.074, -0.012}`.

**Why extended sRGB is the right internal representation here.** SwiftUI's `Color` initialiser documents the mechanism: "A standard sRGB color space clamps each color component… but **SwiftUI colors use an extended sRGB color space, so you can use component values outside that range**" (**10.15+**). **The inference: because out-of-range components survive, the out-of-gamut condition is still legible as a number after conversion.** Convert to plain sRGB and the clamp erases the evidence. **For an app whose premise is not hiding the clamp, that is decisive.**

**Avoiding double conversion.** The failure mode is not "forgot to convert" — it is "converted manually *and then* let ColorSync convert again." **Convert exactly once, and only for measurement, never for drawing.** Tag the swatch with its true colour space and hand it to the view; run a separate explicitly-intent-ed conversion against the display profile purely to compute the verdict, and discard the result.

**Communicating that an exported sRGB value is lossy.** There is a strong ISO-backed convention, **and it is not a flag — it is a hierarchy.** X-Rite's CxF 3.0 (which became ISO 17972) structures every colour as an `Object` carrying possibly *many* `ColorValue`s, each of which **must reference a `ColorSpecification`** containing "information about the ColorValue including its source (measurement specifications), illuminant/observer calculation method… and physical attributes." The normative best-practice statement: "**colorimetric colorvalues should include illuminant/observer enumerations, and spectral colorvalues should include WavelengthRange and device information.**"

**The convention: you do not flag the derived value as lossy; you make the derivation unreproducible-without-the-caveat by requiring every derived value to carry the conditions that produced it.** A bare `ColorSRGB` triple with no `ColorSpecification` is malformed. **Stronger than a boolean, because a boolean can be dropped by a downstream consumer while a required specification reference cannot be silently ignored.** The same mapping holds across CGATS.17, the ASCII lineage of the field.

Apple's guidance points the same way from the other end: "**Consider encoding 'compatible' sRGB color alongside new wide gamut colors.**" That is exactly the export shape: ship the canonical value **and** the sRGB derivation, side by side, each labelled — never the sRGB alone.

**Recommended CSV export:** emit `sRGB_R/G/B` **never without** `sRGB_source_space`, `sRGB_rendering_intent`, and `sRGB_gamut_clipped`, plus the illuminant/observer columns CxF requires. If a consumer takes only the triple, that is their choice; **the file itself never presented the number as unqualified.**

*Named gap:* **neither CxF/CGATS nor Apple defines a standard field name for "this derived value was gamut-clipped."** The standards require recording the *conditions*, from which clipping is derivable, but there is no interoperable clipping flag — **so SpectroCapture would be inventing that column, not adopting it.** Follow-up: check ISO 17972-4:2018's `ColorSpecification` schema before minting a proprietary name.

---

## 9. Data Layer Behind a Responsive UI

**Library comparison** *(versions verified live via the GitHub Releases API 2026-08-24)*:


|                                      | **GRDB 7.11.1**                                                        | **SQLite.swift 0.16.0** | **Raw C**    | **SwiftData**                      |
| ------------------------------------ | ---------------------------------------------------------------------- | ----------------------- | ------------ | ---------------------------------- |
| Schema control                       | Full                                                                   | Full                    | Full         | **None** — generated from `@Model` |
| File portable / externally queryable | **Yes**                                                                | **Yes**                 | **Yes**      | **Effectively no**                 |
| Change observation                   | `ValueObservation`, `DatabaseRegionObservation`, `TransactionObserver` | Not provided            | Not provided | `@Query`                           |
| Min macOS                            | 10.15                                                                  | Apple + Linux + Android | any          | **14.0**                           |
| Fetch 200k rows, column indexes      | **0.06 s**                                                             | 0.28 s                  | 0.04 s       | n/a                                |
| Insert 50k rows                      | **0.06 s**                                                             | 0.14 s                  | 0.02 s       | n/a                                |


**Bias disclosure:** the benchmark is published by GRDB's own author (2025-01-26, MacBook Pro 18,1, Xcode 16.2, default settings). **Treat the ordering as credible and the precise ratios as unaudited.**

**The SwiftData verdict, and why it is decisive.** SwiftData is built on Core Data, whose SQLite store is an implementation detail Apple does not specify — entity tables named `Z` + uppercased entity name, with `Z_PK`/`Z_ENT`/`Z_OPT` columns and system tables `Z_PRIMARYKEY`/`Z_METADATA`/`Z_MODELCACHE`, documented only by reverse-engineering, with that documentation carrying its own warning: "**Apple may change its underlying implementation at any time.**"

**Against the commitment that the file stays directly queryable outside the app, that is disqualifying, and not marginally.** A user opening the file expecting `SELECT * FROM samples WHERE l_star > 70` would find `ZSAMPLE` with `ZLSTAR`, on a schema Apple reserves the right to change in a point release. Secondary defect for a blob-heavy schema: SwiftData's external-storage attribute writes payloads to a sidecar directory and **those attributes cannot be used in predicates.** **SwiftData is out on the product commitment, before any performance argument.**

**A correction to a widely-repeated claim:** search results assert "SQLite.swift is now a simple wrapper around GRDB." **That is false** — its `Package.swift` declares `SQLiteSwiftCSQLite` and optionally `SQLCipher`, **no GRDB dependency of any kind.** Do not carry it into the ADR.

**Recommendation: GRDB.** The only option satisfying all three of full schema control, a plain externally-queryable file, and first-class change observation.

### The finding that should shape the architecture

**GRDB's granularity is a region, not a value:** "`ValueObservation` tracks changes in a `DatabaseRegion`, not changes in values… if you track the maximum score of players, all transactions that impact the `score` column… trigger the observation, **even if the maximum score itself is not changed**." Sharpening is available — `tracking(region:_:fetch:)` "lets you entirely separate the **observed region(s)** from the **fetched value**." **For SpectroCapture that is the lever keeping a raw-payload blob column out of the observed region while still observing the metadata columns that drive the grid.**

**GRDB's own scaling guidance:** "**Keep your number of observations bounded.** In particular, **do not observe independently all elements in a list. Instead, observe the whole list in a single observation**" — at 10⁴–10⁵ swatches, one observation per row is the obvious wrong turn and GRDB names it. Also: observations "can create database contention"; use `.map(_:)` for post-processing "without blocking database accesses, and without blocking the main thread"; share with `shared(in:scheduling:extent:)`.

> ### ⚠️ GRDB cannot see external edits
>
> GRDB documents exactly four classes of undetected change: "**Changes performed by external database connections.** Changes performed by SQLite statements that are not a `DELETE`, `INSERT` or `UPDATE` statement compiled and executed by GRDB. Changes to the database schema… Changes to `WITHOUT ROWID` tables."
>
> **The first collides head-on with the product's central commitment.** The whole point is that the user can open the file in `sqlite3` or Datasette — **and if they *write* to it, the running app will not notice.** GRDB's documented remediation (`Database/notifyChanges(in:)`) only works from inside the app's own write transaction, which is no help when the writer is a Terminal window.
>
> **This is not a reason to abandon GRDB** — no Swift SQLite library detects out-of-process writes, because SQLite itself offers no cross-process change notification. But it means "**the user can query the file externally**" is safe while "**the user can *edit* the file externally**" needs an explicit reload affordance. **That should be an ADR decision, not an implementation detail.** Practical mitigation: a `DispatchSource` file-system watch on the `-wal` and main files, or an explicit "Reload from disk" command, plus a `WITHOUT ROWID` prohibition in the schema.

**SwiftData's `@Query` is `@MainActor @preconcurrency`** — main-actor-bound by construction, the opposite of what a 10⁵-row grid wants. Apple publishes no statement of its invalidation granularity; **that is a documentation gap on Apple's side, not a retrieval failure.**

**Pagination.** Keyset (seek) pagination, not `LIMIT/OFFSET` — `OFFSET` degrades linearly because SQLite still walks the skipped rows. Reported figures on 1,000,000 rows: 0.28 ms at offset 0 vs **138 ms at offset 999,990**. *[Verified via engineering write-ups, not a primary sqlite.org document — treat the mechanism as well-established and the millisecond figures as secondary.]*

**The reconciliation problem is genuinely unsolved in the sources.** Keyset is *sequential by construction* — it needs the previous page's last key. A virtualised grid where the user drags the scrollbar to 60% has no previous key. Three approaches, none documented as a SwiftUI pattern:

1. **Fetch IDs eagerly, rows lazily.** `SELECT id FROM samples ORDER BY …` returns a compact array — at 10⁵ an `Int64` array is 800 KB, trivial. The `ForEach` iterates IDs; each cell fetches its own row on appear. **Arbitrary scroll positions become free (index into the array) while the heavy payload stays off the scroll path.** This is the recommendation, and it is INFERRED.
2. `OFFSET` for jumps, keyset for sequential scroll.
3. A precomputed indexed `rank` column — but every sort order needs its own, and every insert invalidates ranks.

**And a macOS hazard that breaks the naive pattern:** if `List` on macOS builds every row eagerly (the 101-row report), then "windowed fetching behind a `List`" does not work as designed — **every row's body runs, so every row fetches.** An argument for `ScrollView` + `LazyVGrid` plus the ID-array strategy.

**Indexing with a blob in the row — and SQLite's author says exactly how.** D. Richard Hipp, SQLite forum, 2020-08-24:

> "If the K-th column is the right-most column in a row that you are reading, then SQLite will only read in as much as needed to cover the first K columns."
> "if you have a bunch of small columns up front followed by a big blob, and you read from the row but do not read the big blob, then **the big blob is not loaded into memory**."
> "**But if you have one small column *after* the big blob and you need to read that one small column, then SQLite will probably read in the big blob too.**"

**Three consequences, in priority order:**

1. **If the blob stays in the row, it must be the last column — with nothing after it, ever.** The caveat is the killer: **a schema that evolves by `ALTER TABLE ADD COLUMN` appends the new column *after* the blob, silently converting every grid query into a full-blob read.** A latent, invisible performance regression triggered by a routine migration. **This alone argues for a separate blob table, because it removes a class of regression no code review would catch.**
2. **A separate table is the safer design** — `sample(id, name, l_star, a_star, b_star, …)` + `sample_payload(sample_id PRIMARY KEY REFERENCES sample(id), raw_payload BLOB, spectrum BLOB)`. The grid query never touches the payload; the join is a single rowid lookup when the inspector opens. **It also composes cleanly with version history** — history rows attach to the payload table, not the metadata row.
3. **Covering indexes** over `(sort_key, filter_col, id)` let SQLite answer grid queries from the index without touching the table at all, making the blob's location moot for that path.

**Derived values.** SQLite's generated columns (3.31.0+): "The value of a VIRTUAL column is computed when read, whereas the value of a STORED column is computed when the row is written." Constraints: **"One cannot add new STORED columns using ALTER TABLE ADD COLUMN"** — a severe migration constraint; expressions limited to "constants, scalar deterministic functions, and same-row column references"; and **"Earlier versions cannot read databases containing this feature"** — a hard SQLite-3.31.0 requirement on any external tool, which is a portability cost that should be conscious.

**The decisive constraint:** the derivation from a raw spectral payload to XYZ/Lab **is not expressible as a SQLite scalar expression** — it requires integrating reflectance against a CIE standard observer and an illuminant. Generated columns are only available via a custom application-defined function, **and the moment you register one, the derived value is no longer computable by an external tool** — `sqlite3` at the command line will not have your function. **The generated column would break the very portability the product commits to.**

**Recommendation: ordinary columns written by the app, with an explicit `derivation_version`.** The grid needs indexed Lab columns; the derivation is expensive and shouldn't rerun on read; an external session sees plain numbers. `**derivation_version` is what makes a derivation change tractable** — bump the constant and find every stale row with `WHERE derivation_version < :current`. **Without it, a derivation change is unobservable and you have a silently mixed-vintage dataset — a correctness bug, not a performance one.** A SQL `VIEW` is the one thing not to do: views can't be indexed, so every grid sort becomes a full scan plus per-row derivation, and the derivation isn't expressible in SQL anyway.

**Concurrency — GRDB's two mandatory rules.** **Rule 1: connect to any database file only once** — "Open one single `DatabaseQueue` or `DatabasePool` per database file, for the whole duration of your use." Breaking it costs the observation features and produces `SQLITE_BUSY`. **Rule 2: mind your transactions** — "**You are responsible**, in your Swift code, for delimiting transactions… **These bugs corrupt user data, and are very difficult to fix.**"

`**DatabasePool` is required** for a responsive grid over 10⁵ rows, because a `DatabaseQueue` serialises the grid's reads behind any in-flight write.

**Three scheduling pitfalls:**

1. **The default is main-actor, asynchronous.**
2. `**.immediate` blocks the main thread on purpose** — "**Take care that the user interface is not responsive during the fetch of the first value, so only use the `immediate` scheduling for very fast database requests!**" Genuinely useful for avoiding a loading flash, but a 10⁵-row initial fetch would hang.
3. **The non-obvious one: writes on the main thread trigger fetches on the main thread.** "**In particular, modifying the database on the main thread triggers a fetch on the main thread as well.**" So an inline metadata edit committed from a SwiftUI action can drag a full grid re-fetch onto the main thread. The remedy needs a `DatabasePool` and `tracking(regions:fetch:)` — with GRDB's own warning: "**Make sure you read the documentation of those methods, or you might write an observation that misses some database changes.**"

**The actor pitfall (INFERRED):** GRDB's model is built on **serial dispatch queues, not Swift actors.** Wrapping a pool inside your own `actor` **does not add safety** — GRDB already serialises — and **does add a hop that can reorder notifications and defeat the non-reentrancy guarantee.** Hold the pool as a plain shared value and let GRDB own the serialisation.

---

## 10. Empty, Loading, and Error States

**Apple's guidance favours placeholders over spinners:** "**Show something as soon as possible.** If you make people wait for loading to complete before displaying anything, they can interpret the lack of content as a problem with your app… Instead, consider showing placeholder text, graphics, or animations as content loads."

**But the empirical evidence contradicts the industry folklore.** Faulkner &amp; Olvera (2017), 136 participants across three conditions:


| Metric                | Skeleton (n=39) | Spinner (n=39) | Blank (n=58) |
| --------------------- | --------------- | -------------- | ------------ |
| Agreed load was quick | 59%             | **74%**        | 66%          |
| Disagreed             | 36%             | **10%**        | 26%          |
| Avg perceived wait    | 2.82 s          | **2.41 s**     | 2.29 s       |
| Post-load task time   | 10.54 s         | **9.49 s**     | 9.50 s       |


**The skeleton screen performed worst on every measure.** Their conclusion: "Skeleton screens aren't a silver bullet for increasing perceived performance."

**Held honestly:** the evidence is genuinely divided — secondary sources cite mobile studies where skeletons won, which the agent did not retrieve and declined to cite figures from. **What is defensible: the widely-repeated "30% faster" claim has no retrievable controlled study behind it, and the one controlled study that was retrieved found the opposite.**

**For this app the argument for skeletons is weak *and unnecessary*.** GRDB's `.immediate` scheduling notifies the initial value synchronously — "You don't have to implement any empty or loading screen, or to prevent some undesired initial animation." **At 10³ items the first page is fast enough to show real content with no loading state at all**, which beats every condition in the study. Reserve a determinate progress bar for genuinely long operations (bulk re-derivation, FTS rebuild). `redacted(reason:)` (**11.0+**) is available if you want skeletons anyway.

**Empty states — the first run's claim that no guidance exists is wrong.** NN/g's three guidelines: **communicate system status** ("There are no records to display for the selected date range" — this disambiguates *empty* from *still working*, without which a blank container reads as a malfunction); **provide learning cues** ("Star your favorites to list them here"); **offer direct pathways** (put the actual action in the empty state).

`**ContentUnavailableView` is macOS 14.0+**, and Apple's own framing maps onto the three states.


| State                           | Correct response                                                       | Why it differs                                                                                          |
| ------------------------------- | ---------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| **First run — genuinely empty** | `ContentUnavailableView` with an action that starts capture or imports | The pathway *is* the point; nothing to undo                                                             |
| **Filter matches nothing**      | Name the filter *and* offer "Clear filter"                             | State *why* it is empty. **Offering "Add item" here would be wrong** — the items exist, they are hidden |
| **Search returns nothing**      | `ContentUnavailableView.search`                                        | System-provided, localised, consistent with macOS                                                       |
| **Still loading / indexing**    | **Not an empty state.** Progress indicator                             | NN/g explicitly flags the premature "No records" message as a defect                                    |


**The distinction most implementations get wrong: "no items" and "no matching items" require opposite calls to action.** One says *create*; the other says *stop filtering*.

**Progressive search availability.** FTS5's segment-merge architecture has explicit knobs: `automerge` (default 4, max 16) — "Setting it to a small value can speed up queries… but can also slow down writing"; `crisismerge` (default 16) — **the one that causes a visible stall**, since "an INSERT, UPDATE or DELETE that triggers a crisis-merge may take a long time to complete"; and `merge N` as the incremental escape hatch. External content tables carry the warning: "**It is the responsibility of the user to ensure that the contents of the full-text index are consistent with the named database object. If they are not, query results may be unpredictable.**"

**The design consequence: FTS5 is *incrementally* searchable** — each insert creates a segment that is immediately queryable. **There is no window where search returns nothing; there is a window where it is slower, and a window where a write stalls.** That reframes the UI problem: you are not communicating "search unavailable," you are communicating "still consolidating." **Run bounded `merge` calls on a background task with a determinate indicator; never gate search behind index completion.**

*Honest gap:* **no authoritative design guidance for "partially indexed collection."** macOS's own Spotlight "Indexing…" precedent could only be sourced from user forums and troubleshooting articles, which the agent declined to cite as a design convention. **Follow-up — competitive teardown, not more literature search:** install DEVONthink 3, Eagle, and Adobe Bridge, import a 20,000-item folder into each, and screen-record the first 120 seconds. Record: is search enabled during indexing; is the indicator determinate; does the app say results may be incomplete; is indexing cancellable. **Three concrete precedents in an afternoon — more than the published literature contains.**

**Data-integrity problems.** SQLite's *How To Corrupt An SQLite Database File* is the governing document, and its first point *is* the product's own premise: "SQLite database files are ordinary disk files. **That means that any process can open the file and overwrite it with garbage. There is nothing that the SQLite library can do to defend against this.**"

Documented hazards that follow directly from "the user owns the file": **backup or restore while a transaction is active** ("Systems that run automatic backups in the background might try to make a backup copy… The backup copy then might contain some old and some new content, and thus be corrupt") with named safe alternatives `sqlite3_rsync` and `VACUUM INTO` — **a first-class product concern, because "the SQLite file is the sync strategy" means users will put it in Dropbox or iCloud**; **unlinking or renaming while in use**; **two processes using different locking protocols**; and **detached journal/WAL files** — users who "back up the database" by copying only the `.sqlite` and not the `-wal` do exactly this.

**Detection APIs:** `PRAGMA integrity_check` (O(N log N), "will return at most *N* errors before the analysis quits, with N defaulting to 100"); `PRAGMA quick_check` ("does most of the checking… but runs much faster," O(N), skipping UNIQUE verification and index-to-table matching); `PRAGMA foreign_key_check` — **required separately, because `integrity_check` does not find FK errors**; and `PRAGMA cell_size_check`, **off by default**, which detects corruption "earlier and is less likely to 'spread'."

**The GRDB-specific integrity problem is not corruption but is just as user-visible:** the app showing stale data while `sqlite3` shows current data, **with no error anywhere. Arguably worse than corruption, because it is silent.**

**Recommended strategy:** run `quick_check` on open, reserving the full `integrity_check` + `foreign_key_check` for user request or a `quick_check` failure — at 10⁵ rows with blobs, an O(N log N) check every launch is not acceptable. Enable `cell_size_check` at connection setup. **Distinguish three failure classes, because they need different user actions:**

- **File-level corruption** — read-only mode, offer `VACUUM INTO` to salvage what is readable. **Never auto-repair a file the user owns.**
- **Row-level unreadable payload** — quarantine the single row, badge it in the grid, keep the rest usable. **A catalogue app must not become unopenable because one of 1,200 payloads is malformed** — and since the raw payload is canonical, a bad payload is a data-loss event the user needs to **see**, not one to hide.
- **Failed migration** — refuse to open, name the schema version found vs expected, point at the pre-migration snapshot. **Note the generated-columns interaction:** a file written with 3.31.0+ generated columns cannot be read by older SQLite at all, which presents to the user as a corrupt file if not explained.

**Snapshot before any migration using a documented-safe method** (`VACUUM INTO` or the backup API), **never `cp`** — a `cp` of a live WAL database is one of SQLite's named corruption paths.

---

## Where this run differs from the first


|                                     | First run                    | This run                                                                                                                |
| ----------------------------------- | ---------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| Answered                            | 28 / 54                      | **54 / 54**, with gaps stated *inside* answered questions                                                               |
| Gap reasons citing retrieval limits | **14 of 26**                 | 0                                                                                                                       |
| Original measurement                | none                         | SQLite + FTS5 + R-tree + CIEDE2000 benchmarked on this hardware; SDK headers and the live sandbox profile read directly |
| Area 10                             | **no answer section at all** | fully answered                                                                                                          |


**Verified independently by the orchestrator:** SQLite 3.51.0 with FTS5 and RTREE; `PRAGMA foreign_keys` = 0; SQL:2011 syntax absent; partial unique index enforcement; trigram substring match; `kColorSync1BitGamut` at `ColorSyncTransform.h:72`; `kColorSyncTransformGamutCheck CS_AVAILABLE_STARTING(10.4, 16.0)` at line 158; `kColorSyncSigGamutTag` at `ColorSyncProfile.h:80`; the Davies &amp; Morland, STRV, Powell, Tulig and NN/g sources.

## Residual gaps

**(a) No literature exists** — findability in a grid of colour swatches; how shipped macOS apps internally preserve selection/scroll across a view-mode switch; comprehension of out-of-gamut indicators; controlled inline-vs-inspector-vs-modal comparison; autosave-vs-explicit error rates; design guidance for partially-indexed search.

**(b) Exists but unreachable** — ASTM D1729-22's specimen-size clause; ISO 17972-4:2018's `ColorSpecification` schema; Steinarsson's LTTB thesis (hCaptcha wall — cite the reference implementation instead); Adobe's Lightroom collections page (timeout).

**(c) Inherently empirical** — macOS container benchmarks at 10³/10⁴/10⁵ in Release; many small `Chart` instances in a scrolling container; the gamut-check transform's rendering-intent semantics; the ID-array pagination strategy under a scrollbar drag.

**Every (c) has a specified harness in its section.** The highest-value two are the four-variant container benchmark (§3) and the gamut-check intent harness (§8) — **the second because an unvalidated gamut check is precisely a check that reports a verdict without evaluating its subject.**