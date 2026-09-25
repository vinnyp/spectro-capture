<!-- guidance (the principle this whole format exists to enforce — read before filling anything):
     Rows own behaviour; acceptance scenarios supply fixtures, actions, oracles and source rows;
     copy owns displayed text; fences own decisions. Anything a builder needs to know lives in
     exactly one of those four places and nowhere else. A rule restated in a preamble, a note, a
     journey or a README is a restatement that cites its owning row — it never introduces.
     Template shape is never the point; buildability by an agent is — but the shape is fixed
     rather than advisory: the Format contract below names what a project may change and what it
     may not, and the mechanical checks parse the sections it fixes by name.
     Two fill families are placeholders: `{{...}}`, substituted at scaffold time, and `_(prompt)_`,
     filled during authoring; an unresolved instance of EITHER is a lock-blocking defect. Every
     guidance comment in this file — this one included — is deleted at lock, so a rule a BUILDER
     needs after lock lives in body text and never in a comment; a comment carries only what an
     AUTHOR needs while filling the document. The word budget for this body (companions excluded)
     is 12000 words: count it with HTML comments, link targets, code fences and table
     pipes stripped; trim rule-free prose to fit; never raise the budget; a document that cannot
     fit is two PRDs.
     12000: the project-supplied word budget for this PRD body, default 12000. -->

# PRD: Collection Mode
<!-- guidance: Collection Mode is the product or feature area this PRD governs — a short name,
     substituted once at scaffold time. The PRD's file name and its `prd-collection-mode` are derived from
     it, and that slug names every state artifact below (journeys, copy, fences, OQ results). -->

Status: draft
<!-- guidance: the status line has exactly three forms, in this order over the document's life.
     At scaffold: `Status: draft`. At lock: `Status: locked (<date>)`. An amendment APPENDS to the
     locked form: `; agent-build amendment <date> under F<n>–F<m> (peer review pending)` — naming
     the fence range that authorises it. The bookkeeping close then REWRITES that appended clause
     to `; peer review closed <date> (PR #<n>); re-locked on merge`, and changes nothing else.
     There is no author line in this document: the owner is named in the fence file. -->

Companions: `docs/product/collection-mode/prd-collection-mode-journeys.md` (acceptance scenarios) ·
`docs/product/collection-mode/prd-collection-mode-copy.md` (copy) ·
`docs/product/collection-mode/prd-collection-mode-fences.md` (decisions) ·
`docs/product/collection-mode/prd-collection-mode-oq-results.md` (open-question results)
<!-- guidance: name all four companions by path, on one line, so an agent handed only this file
     can find every other place a rule of this product lives. Not conditional: all four exist from
     scaffold time, even while empty.
     The four SUFFIXES are a contract, not a convention: `-journeys.md`, `-copy.md`, `-fences.md`
     and `-oq-results.md`. The mechanical checks parse this line to resolve the companions and
     assert that all four are declared, so renaming one is a breaking format change that must be
     made here and in the checks together.
     docs/product/collection-mode: the project's product-docs directory, written REPO-ROOT-RELATIVE (e.g.
     `docs/product`), not as an absolute path and not relative to this file. The mechanical checks
     resolve companion paths against the repository root, so a path written any other way makes
     them resolve against the wrong base. Where this PRD and its four
     companions live — never `/tmp`, always in the project repo. -->

Format: agent-prd v1
<!-- guidance: the format version this document is authored to, and the version whose checks a
     re-lock runs. A change to the format that would make a conforming document non-conforming — a
     renamed section, a changed column set, an added lock condition — is a new major version;
     re-locking under a newer version is a deliberate migration, recorded as its own fence.
     What v1 fixes, and what an author may therefore change: every section heading in this
     document, and the column set of the row-transitions, constants-and-closure-gates,
     build-dependencies, requirement, obligations, copy-index, metrics and open-questions tables,
     is fixed — the mechanical checks parse them by name. A project may add a requirement section,
     add a trailing column to a requirement table, or delete a section this format marks
     conditional. Any other reshaping is a fork of this format, not an instance of it. This
     version line is the locked document's one citation of that contract; the contract itself is
     not restated in body text, because it is a rule for authoring and checking this document and
     not for building the product. -->

---

## Build contract
<!-- guidance: two paragraphs, no headings inside, and nothing else. This section REPLACES the
     background section, the problem statement, persona-story headings and every piece of inline
     narrative a human-audience PRD carries — a building agent needs ownership and rules, not
     motivation, and narrative is where behaviour hides.
     Paragraph 1 — ownership: what this PRD owns, and what each sibling document owns instead, one
     clause per sibling, each naming that sibling BY DOCUMENT NAME, then what this PRD
     deliberately does not cover. A reader who lands here must be able to route any question to
     the document that answers it — and a non-goal stated here is a question a builder stops
     asking, where an unstated one is a gap it fills by guessing.
     Paragraph 2 — the four-place rule (rows / acceptance scenarios / copy / fences), plus,
     verbatim: "Preserve row IDs, priorities, statuses and historical decisions; an amendment is
     not an implementation completion." If this PRD is gated on an open architectural fork, name
     it here with its open question and its ADR, and state that neither reading is selected here.
     {{sibling-document-name}}: one per sibling PRD this document shares a seam with — the
     document names used in this paragraph and in every cross-PRD cite in this file. -->

This PRD owns browsing and working with the collections in the user's file: the collection list,
the collection surface's item table and — last — its swatch grid, and the All items view; every
honesty mark shown where this PRD renders colour, including whether the display in use can show a
colour; search, filters, view sorts, the lightness, chroma and hue sorts and Find similar; the item
detail and editing an item's metadata, its Swatch Code and an imported column's name; the
version-history view, restoring an earlier reading, and where the correction question is reached;
renaming and deleting a collection, deleting an item or a selection, and setting or clearing one
field across a selection; and the controls that reorder a collection's scan queue from the
collection. The Capture Mode PRD owns creating a collection and its capture settings, the queue
order and what a reorder does to it, sessions and the set-aside list, re-scanning, and every entry
point and state it places on the collection surface. The Data Foundation PRD owns the file and what
it keeps — readings, current values, supersession reasons and their marks, derived values and the
stored gamut-clipped flag, damage, re-reading the file, and what a restore and a delete do —
together with its confirmation and question states (its E4, E8, E11, E14, E26, E31 and E33). The
Inventory Import PRD owns the one matching rule and how imported columns arrive; the Data Export
PRD owns what an export contains once it is started; the Device Management PRD owns the
simulated-readings banner's copy (its E22) and simulated provenance; the QC & Comparison PRD, not
yet written, owns checking a swatch against its reading; and the Color Visualization PRD, not yet
written, owns the 3D plot. This PRD deliberately does not cover exporting only a selection or
answering the correction question for a selection (settled out by F3), matching against any named
colour library (F4 and the vision's non-goals), any level of organisation above collections (the
capture PRD's F1), an app-owned notes field or a column added by hand (F8), and
multi-collection compare or printer gamut analysis (the vision's v2 candidates).

Rows own behaviour; acceptance scenarios supply fixtures, actions, oracles and source rows; copy
owns displayed text; fences own decisions. Preserve row IDs, priorities, statuses and historical
decisions; an amendment is not an implementation completion. Whether capture is a full-window
takeover that hands off to the collection when a session ends, or a state of the live collection,
is open — the capture PRD's R10.6 and OQ 8, and ADR-0004 — and neither reading is selected here:
every row states behaviour on the collection surface without deciding how the user reached it or
where it sits relative to capture, and where the session summary sits stays the capture PRD's F20.
The storage schema (ADR-0003) and the minimum macOS version (ADR-0006) are open too, and no row
here chooses either.

## Row transitions
<!-- guidance: one table, one row per route between the states of the entity this PRD governs,
     each naming the requirement rows that own the route. This section REPLACES the lifecycle
     diagram: a diagram is read, a table is checked.
     Enumerate the routes FROM THE REQUIREMENT ROWS, not from memory — walk every row that can
     change the entity's state and give its route a line here. A route any row can cause and this
     table omits is a defect (two reviewers independently caught one omitted route in the first
     conversion run of this format). -->

The `Starting state` and `Result` cells carry state names, never row IDs, and only terms the
Vocabulary marks `(state)`. The entity is an item or a collection as this PRD shows it; a reading's
own states and reasons are the Data Foundation PRD's and a row's queue states the capture PRD's, and
a route here that changes one of those names the owning row it acts through.

| Starting state | Event | Result | Rows |
|---|---|---|---|
| present | A delete of an item, a selection or a collection is confirmed in a build without R1.7 | deleted | R1.4, R4.5, R6.3 |
| present | A delete of an item, a selection or a collection is confirmed in a build with R1.7 | deleted-undoable | R1.7 |
| deleted-undoable | The user fires "Undo" on E10 | present | R1.7 |
| deleted-undoable | The Data Foundation PRD's DELETE_UNDO_WINDOW ends: the file closes, or the app quits or crashes | deleted | R1.7 |
| present | The user renames a collection to a name R1.3 accepts | present | R1.3 |
| present | The user commits an edit to an editable field | present | R4.3 |
| present | The user changes a Swatch Code to one R4.4 accepts | present | R4.4 |
| present | The user renames an imported column to a name R4.8 accepts | present | R4.8 |
| present | The user applies a bulk set or clear | present | R6.2 |
| present | The user fires "Undo" on a committed metadata change | present | R4.7 |
| present | The user restores an earlier reading, a new current value resulting (the Data Foundation PRD's R2.3f) | present | R5.5 |
| present | The user answers a re-scan question from the item detail or through "Answer re-scans" (the Data Foundation PRD's R2.3c–e) | present | R4.6, R5.7 |
| present | The user fires the capture PRD's Flag action on a captured item in the item detail, the item becoming set aside (the capture PRD's R5.6 and R9.9) | present | R4.9 |
| present | The user reorders a collection's queue from the collection surface (the capture PRD's R6.8) | present | R2.9 |

## User journeys
<!-- guidance: one paragraph, no table. The journeys themselves are acceptance scenarios and live
     in the journeys companion; this paragraph points at it and states the three things a reader
     needs to use it: the `### UJ n. <name>` headings are stable anchors (cite them, never
     renumber them), cases run per phase, and hardware- or environment-gated findings stay behind
     their open questions rather than being asserted here. -->

The user journeys for this product area are acceptance scenarios, kept in
`docs/product/collection-mode/prd-collection-mode-journeys.md`. The `### UJ n. <name>` headings
there are stable anchors: cite them, and never renumber or retitle them. Cases run per phase — a
case for a row, state or variant that a phase mark or a P1 or P2 priority holds back runs in the
phase that lands it — and a finding that depends on an environment this document has not measured
(timings, display gamut behaviour) stays behind its open question rather than being asserted here.
UJ 1 carries the capture PRD's inherited cases UJ1.2-a, UJ1.2-b and UJ1.2-c, and UJ 2 the
collection half of the device PRD's UJ1.2-c.

## Requirements

### Vocabulary
<!-- guidance: every term the rows use, one line each — a term a row leans on and this list does
     not define is a latent decision. Include the product's measurement tiers where it has them
     (e.g. the raw, the stored and the computed form of whatever this product measures) and EVERY
     state name the row-transitions table uses. Not conditional. -->

A term that names a state of the entity this PRD governs is written `- **<term>** (state) — …`, so
the set of state names is read off this list rather than inferred from the rows; the
row-transitions table uses only terms marked that way. The Data Foundation PRD's reading, sample,
derived value, supersession reason, quarantined and chosen condition, and the capture PRD's
pending, captured, set aside, settled, session and remembered row, are used unchanged.

- **the file** — the one SQLite file holding all the user's collections (the Data Foundation PRD's
  R1.1).
- **collection** — a named, flat set of items; no collection contains another (the capture PRD's
  R1.1).
- **item** — one swatch in one collection, identified within it by its Swatch Code; copy calls it a
  swatch.
- **Swatch Code, Swatch Name, Swatch Alternate Code, Swatch Alternate Name** — the import PRD's
  identity fields.
- **imported column** — a column an import added to a collection, holding one value per item (the
  import PRD's R2.2, R2.5, R2.6).
- **editable field** — Swatch Name, Swatch Alternate Code, Swatch Alternate Name and every imported
  column: R4.3's set. Swatch Code is changed only through R4.4.
- **current value** — an item's canonical value; an item has at most one, and has none when never
  scanned or set aside, an item whose current reading is quarantined being set aside with the
  cause unreadable (the capture PRD's R8.18).
- **working set** — the derived values of the collection's chosen condition under its illuminant
  and observer (the Data Foundation PRD's R3.1).
- **collection list** — the surface naming every collection in the file.
- **collection surface** — the surface one collection is browsed on; the capture PRD names it and
  places its own entry points and states on it, and this PRD owns its browsing.
- **item table** — the collection surface's list of items, one row each.
- **All items view** — a view listing every item of every collection; it holds nothing of its own
  and is no level above collections.
- **item detail** — the surface showing one item's lines and actions.
- **version history view** — the surface listing every reading one item holds.
- **colour chip** — the patch in a row, the detail or the history that renders a value.
- **honesty mark** — one of R2.4's marks: a sign, perceivable without seeing colour, that a value
  is not what its colour alone suggests.
- **display gamut** — the range of colours the display showing the window reports it can render.
- **queue order** — a collection's scan order, changed only by the capture PRD's reorder actions
  (its R6.7).
- **view sort** — an order chosen for browsing the table; it never changes queue order.
- **search** — the text typed in the search field; **filter** — a chosen row state or mark that
  narrows the table.
- **selection** — the items selected in the table.
- **in flight** — a session on a collection that is active, including paused by the operator or
  the guard and held by a device halt (the capture PRD's R3.2).
- **interrupted session** — the capture PRD's unresumed interrupted bulk session.
- **over-time view** — any ordering of one item's readings by measurement time (the Data
  Foundation PRD's R2.1, R2.5).
- **never true** — the Data Foundation PRD's mark on a reading a confirmed correction superseded.
- **unsettled** — the mark on a reading whose successor's reason is correction-unconfirmed.
- **ΔE2000** — the CIEDE2000 colour difference (AGENTS.md §8).
- **named constant** — a product parameter in the Legend's constants table, named in its owning
  rows.
- **present** (state) — the item or collection is in the file and listed wherever its collection is
  browsed.
- **deleted-undoable** (state) — the item or collection has been deleted, is listed nowhere, and
  "Undo" can restore it until the Data Foundation PRD's DELETE_UNDO_WINDOW ends.
- **deleted** (state) — the item or collection is gone from the file for good, unrecoverable by an
  outside reader (the Data Foundation PRD's R6.2a).

### Legend
<!-- guidance: the vocabularies every later table depends on: priority semantics (exactly one of
     the two bullets survives to lock — delete the other), the phase rule, the status vocabulary,
     then the two tables and the two bullets below. Not conditional. -->

**Priority — the semantic the `Pri` column carries in this release:**

- **Build order within the release — nothing droppable.** Every P0/P1/P2 row ships in this
  release; priority only orders the sequence work happens in.

| Pri | Meaning |
|---|---|
| P0 | Blocks the first usable build: a collection cannot be browsed, searched, read honestly, edited, deleted, or its history reached without it. |
| P1 | Built after the P0 rows, inside v1. |
| P2 | Built last inside v1. |

**Release.** The `Release` column on a requirement row names the release that row ships in, in the
project's own release vocabulary. Priority orders the work inside a release; this column says which
release. A row not yet assigned to one leaves the cell empty.

**Phase rule.** A P0 state that offers an action whose rows are P1 is shown without that action
until those rows land. Surfaces, states, actions and body variants carry phase marks as three
distinct kinds — a surface that does not exist yet, an action that is absent from a surface that
does, and a body variant that is not yet produced — and a phase mark says which kind it is. The
mark is written `[phase: surface-absent]`, `[phase: action-absent]` or `[phase: variant-absent]`;
those three are the complete set, here and in the copy companion.

**Status vocabulary** (every requirement, error/state and success-metric row uses exactly these
six values, in this order of progression):

| Status | Meaning |
|---|---|
| pre-alignment | Drafted, not yet reviewed. |
| needs-discussion | Reviewed; at least one open objection or fork. |
| aligned | Reviewed; unanimous non-abstaining disposition, or an owner overrule recorded with the standing objection and reason. |
| in-progress | Aligned and being built. |
| done | Built and verified. |
| deferred | Explicitly cut from this release, not abandoned. |

**Constants and closure gates**
<!-- guidance: every named provisional constant this product has, its candidate value or "TBD"
     plus the interim rule that holds until it closes, the rows that use it, and the open question
     plus the evidence that closes it. Names identify product parameters, not storage columns or
     API names. Every constant listed here is also named in its owning row — this table is an
     index, not the constant's home. -->

| Constant | Candidate / interim | Owning rows | Closure evidence |
|---|---|---|---|
| BROWSE_RESPONSE_BUDGET | 100 ms at the 95th percentile; interim rule — build and test against 100 ms | R2.5, R3.1, R8.1, R8.2 | OQ 1 — Release-build timings at ROWS_CEILING on the machines the owner names |
| OPEN_COLLECTION_BUDGET | 1 s; interim rule — build and test against 1 s | R8.1 | OQ 1 — the same timing run |
| FILE_ITEMS_CEILING | TBD; interim rule — design and test the file-wide views at ROWS_CEILING items across the whole file | R8.2 | OQ 2 — the owner, on the capture PRD's OQ 13 scale evidence |
| FIND_SIMILAR_DISTANCE | ΔE2000 3.0; interim rule — list items at or within 3.0 | R3.7 | OQ 3 — a dogfood Find similar pass over a real collection |
| BULK_CONFIRM_COUNT | 10 items; interim rule — a bulk set or clear across more than 10 items confirms first | R6.2 | OQ 4 — dogfood bulk edits, then the owner |
| NEUTRAL_CHROMA | C* 3.0; interim rule — a hue sort places items below C* 3.0 after the chromatic ones | R3.3 | OQ 5 — a dogfood hue sort of a collection holding a grey series |
| MIN_SWATCH_SIZE | 24 pt; interim rule — no swatch renders smaller than 24 pt on a side | R7.1, R7.2 | OQ 6 — the owner at the P2 build, on the owner's display at working distance |

This table is an index: every constant it names is also named in the row that owns it, and that row
is the constant's home. Where the two disagree, the row governs. ROWS_CEILING is the capture PRD's
(its OQ 13), and DELETE_UNDO_WINDOW and SQLITE_READER_FLOOR are the Data Foundation PRD's; this
document uses them by citation and carries none of their closure gates.

**Build dependencies**
<!-- guidance: what the first build can proceed on, and what must stay gated. One row per unit of
     work: the contract already available to build against, and what must remain open (an ADR, an
     open question, hardware, a dogfood run) rather than being guessed at by the builder. -->

| Work | Available contract | What must remain open |
|---|---|---|
| Collection list, collection table, honesty marks, search, filters and sorts | R1.1–R1.6, R1.8, R2.1–R2.8, R3.1–R3.6, R8.1, R8.3–R8.10; copy E1–E7, E12; the capture PRD's E1 and E2 and the device PRD's E22 | ADR-0003's schema; OQ 1's and OQ 7's closure evidence — the interim budgets and gamut test hold until measured; ADR-0006's macOS floor |
| Item detail, editing and history | R4.1–R4.3, R4.5, R4.6, R5.1–R5.3, R5.6, R5.7; copy E14, E17; the Data Foundation PRD's E4, E8, E11, E14, E26 and E31; the capture PRD's R8.18 | nothing beyond the row above |
| The All items view and Find similar | R1.9, R1.10, R3.7, R3.8, R8.2; copy E9, E13 | OQ 2's and OQ 3's closure evidence — their interims hold; OQ 11's check of the harness's reference pairs |
| Selection and bulk set or clear | R6.1, R6.2, R4.7; copy E8 | OQ 4's closure evidence — its interim holds |
| Bulk delete | R6.3; the Data Foundation PRD's R6.2 and E33 | nothing beyond the selection row above |
| Undo of a delete | R1.7; copy E10 | OQ 10 — the Data Foundation PRD's OQ 20 |
| Code change, column rename and Flag | R4.4, R4.8, R4.9; copy E6, E11, E15, E16; the Data Foundation PRD's R1.2, the import PRD's R2.6, and the capture PRD's R5.6 and R9.9 | nothing |
| Reordering from the collection | R2.9; the capture PRD's R6.8–R6.12, E33 and E34 | the capture PRD's OQ 7 — its REORDER_SCOPE interim holds |
| Restore, distance from current, compare | R5.4, R5.5, R5.8; the Data Foundation PRD's R2.3f and R2.9 and the capture PRD's R5.6 and R8.18 | nothing |
| Swatch grid and column visibility | R2.10, R7.1, R7.2; the Data Foundation PRD's R1.1 | ADR-0003's schema; OQ 6's closure evidence — its interim holds |
| Where capture sits relative to the collection | no row here | the capture PRD's OQ 8 and ADR-0004 — navigation and placement are never guessed |

A builder builds against the **Available contract** column only. Anything under **What must remain
open** is a stop rather than a guess: that work waits for the ADR, the open question, the hardware
or the dogfood run named there.

**P0 rows that defer to an open question with no interim rule:**
<!-- guidance: these two lists stay separate and are NEVER merged into one — merging them hides
     the difference the body text below states. Derive both from the Open questions table, not
     from memory. -->

A row in this first list cannot be started: its open question has no interim rule, so a builder
that begins it is guessing. A row in the **Interim stated** list below is started under the interim
rule named there, and re-checked when its open question closes.

- None — every open question below carries an interim rule.

**Interim stated:**

- R3.1, R8.1, R2.5, M1 — OQ 1 — F20.
- R8.2 — OQ 2 — no fence yet: an interim drafted for the owner.
- R3.7 — OQ 3 — F16.
- R6.2 — OQ 4 — F7.
- R3.3 — OQ 5 — F19.
- R7.1, R7.2 — OQ 6 — F20.
- R2.5, M2 — OQ 7 — no fence yet: an interim drafted for the owner (F6 leaves it open).
- R1.7 — OQ 10 — the Data Foundation PRD's OQ 20 interim, which that PRD's fences set.
- R3.7 — OQ 11 — no fence yet: an interim drafted for the owner.
<!-- guidance: any ID family may appear here, including an `M<n>` metric row, which carries no
     `Pri` column at all — this list is not priority-filtered, unlike the one above it. A row runs
     under the named interim whatever its priority, and is re-checked when the question closes. -->

### Traceability
<!-- guidance: the ID contract. Three families, one per dispositionable table; lettered sub-rows
     where a lead row carries a table of its own (`R8.1a`); assigned once at first draft and never
     renumbered, so a cut or deferred row keeps its ID rather than freeing it for reuse; retired
     IDs listed by ID so a reader who finds a cite can resolve it. Not conditional.
     Fill the **Commit PR** column with the pull (or merge) request that landed the row, never with
     a bare commit SHA: a SHA stops resolving the moment the branch is squashed on merge. -->

- Every requirement row gets an ID of the form `R<section>.<n>` (e.g. `R7.4`), where `<section>`
  is the number of its requirement section below.
- Every error/state row in the copy index gets an ID of the form `E<n>` (e.g. `E4`).
- Every success-metric row gets an ID of the form `M<n>` (e.g. `M2`).
- A lead row that carries a table of its own numbers those sub-rows with letters (`R8.1a`); three
  do: R2.4, R4.2 and R5.2. A lettered sub-row carries its lead row's release, priority and status.
- IDs are assigned once and never renumbered. **Retired IDs:** none — no ID in this document has
  been retired.
- The **Commit PR** column on a requirement row names the PR that landed it.
- Owner decisions F1–F28 are in the fence file; a row names one for provenance only.

### Surfaces
<!-- guidance: every user-facing surface this product area touches, what it shows, and the copy
     states in that surface's flow. Every copy state appears under exactly one surface unless the
     state is deliberately shared, in which case this preamble says so and names the surfaces.
     Phase marks (see the Legend's phase rule) go on P1-only surfaces, states and actions. The copy
     strings themselves are never written here — they live in the copy companion. Conditional: a
     product area with no user-facing surface deletes this subsection rather than leaving it
     empty. -->

Every copy state appears under exactly one surface unless this preamble names it as shared. E4,
E5, E9 and E12 are shared by the collection surface and the All items view; E6 and E10 by the
collection surface and the item detail. Sibling states also render on these surfaces and stay their
owners': the capture PRD's E1, E2, E3, E23, E24, E25, E28, E33 and E34, the device PRD's E22, the
Data Foundation PRD's E4, E8, E9, E11, E14, E15, E26, E31 and E33 and its read-only file states, and
the Data Export PRD's E1.

| Surface | Shows | The copy states in this surface's flow |
|---|---|---|
| Collection list | Every collection in the file with its item count, and the entries to create a collection, to the All items view [phase: action-absent] and to the re-scans awaiting an answer | E1, E2 |
| Collection surface | One collection's item table — or, last, its swatch grid — with search, filters and sorts, selection and bulk actions, reordering, and the entries to rename, delete and export the collection, beside every entry point and state the capture PRD places there | E3, E4, E5, E6, E7, E8, E9, E10, E11, E12 |
| All items view [phase: surface-absent] | Every item of every collection, one row each naming its collection, with search, filters and sorts | E13, E4, E5, E9, E12 |
| Item detail | One item's identity, state, current value, marks, current-reading facts, damage and re-scans awaiting an answer, its editable fields and its actions | E14, E15, E16, E6, E10 |
| Version history view | Every reading one item holds, in recorded or measured order, with each reading's facts and marks | E17 |
| Swatch grid [phase: surface-absent] | The collection surface's items as swatches at a chosen size, with their marks | none of its own |

### 1. Collections and the collection list
<!-- guidance: one subsection per group of requirements; its heading number is the `<section>` in
     `R<section>.<n>`, so section 1's rows are `R1.1`, `R1.2`, … Repeat this exact table header on
     every section. Open each section with a `Traces UJ…` line naming the journeys that exercise
     it and the vision use case it serves — a section no journey exercises is either unjustified
     or missing a journey. Each row states its rule in at most two sentences: rationale, evidence
     cites and fence cites live outside the cell. Every constant a row uses is NAMED IN THAT ROW,
     not only in the Legend's constants table.
     {{requirement-section-name}}: the name of this requirement section — one per group of rows
     this PRD's product area divides into.
     Three COLUMN NAMES are a contract, here and in the copy index and the metrics table: `ID`,
     `Pri` and `Status`. The mechanical checks locate them by header name rather than by position,
     precisely because the three tables put `Status` in three different columns — so adding a
     column or reordering one is safe, and RENAMING any of those three is a breaking format change
     that silently empties a check's field. Rename one only as a deliberate format version. -->

Traces UJ 1, UJ 4, UJ 6, UJ 7; serves the vision use case *fix a bad scan without losing history*
(U5) through the P0 feature *collections and version history*. F2 decides the All items view; the
capture PRD's R1.8 and F3 hand this PRD the rename and delete rows.

| ID | Release | Pri | Requirement | Status | Commit PR |
|---|---|---|---|---|---|
| R1.1 | v1 | P0 | The collection list renders E2, naming every collection in the file with its item count, ordered by name as the capture PRD's R6.9 orders codes; a file holding no collection renders E1 instead. | pre-alignment | |
| R1.2 | v1 | P0 | "New collection" runs the capture PRD's collection creation (its R1.1–R1.3, R1.5 and R1.10), and choosing a collection in the collection list shows it on the collection surface. | pre-alignment | |
| R1.3 | v1 | P0 | "Rename collection" stores the new name as entered unless another collection's name equals it under the import PRD's R2.3 rule, when the capture PRD's E1 renders and the old name stays, or the new name is blank, when E7 renders, the old name stays, and E7's "Change the name" returns to editing it. A new name equal to the collection's own under that rule, differing only in spacing or letter case, is accepted. | pre-alignment | |
| R1.4 | v1 | P0 | "Delete collection" renders E6's "interrupted" variant and deletes nothing while the collection holds an interrupted session, R8.3 governing it while a session there is in flight; otherwise it opens the Data Foundation PRD's E14, whose actions behave as that PRD's R6.2 states. | pre-alignment | |
| R1.5 | v1 | P0 | E6's "Go to the session" opens that session where the capture PRD returns to it — the session itself while in flight (its R3.5), its E25 resume offer while interrupted — and "Cancel" closes E6 having changed nothing. | pre-alignment | |
| R1.6 | v1 | P0 | Every delete this PRD offers opens its confirmation before anything is deleted, and in each such confirmation the Return key fires the confirmation's cancel action. No surface this PRD renders makes a delete its default action. | pre-alignment | |
| R1.7 | v1 | P1 | After a confirmed delete E10 renders — its body for one item, its "swatches" variant for a selection, its "collection" variant for a collection — and its "Undo" restores what was deleted as the Data Foundation PRD's R6.2b states, until that PRD's DELETE_UNDO_WINDOW ends. OQ 10's interim holds: this row is not built until that question closes. | pre-alignment | |
| R1.8 | v1 | P0 | "Export collection" on the collection surface and "Export swatch" in an item's detail open the Data Export PRD's E1 at collection and at single-item scope respectively (its R1.1a). | pre-alignment | |
| R1.9 | v1 | P1 | "All items" renders E13 and lists every item of every collection in the file, one row per item, with R2.1's columns other than imported columns plus a Collection column, by collection name and then queue order until a view sort is applied. Items sharing a Swatch Code in different collections are separate rows, each naming its own collection. | pre-alignment | |
| R1.10 | v1 | P1 | The All items view offers search, filters, view sorts, the lightness, chroma and hue sorts, Find similar and each item's detail as the collection surface does, each item carrying its own simulated mark in place of the device PRD's E22. It offers no reordering, no multi-item selection or bulk operation, and no collection rename, delete or export. | pre-alignment | |

### 2. The collection table and colour honesty

Traces UJ 2, UJ 8; serves the vision use case *see the collection honestly* (U7). F1 decides the
table with chips and the honesty marks at P0; the capture PRD's R4.11, R6.8 and R8.14, the device
PRD's R6.5, and the Data Foundation PRD's R3.4 and R3.5 hand this PRD what it shows.

| ID | Release | Pri | Requirement | Status | Commit PR |
|---|---|---|---|---|---|
| R2.1 | v1 | P0 | The collection surface renders E3 and lists the collection's items in a table, one row per item, in queue order until a view sort is applied. Its columns are the colour chip, Swatch Code, Swatch Name, row state, L*, C*, h°, Spread, Swatch Alternate Code, Swatch Alternate Name, and every imported column in its stored position. | pre-alignment | |
| R2.2 | v1 | P0 | Row states and the collection's counts use the capture PRD's words and numbers, identical to its capture surface's (its R10.3), and every entry point and state that PRD places on the collection surface stays reachable and unhidden beside this PRD's (its R7.19). A collection holding no item renders the capture PRD's E2 empty variant in place of the table. | pre-alignment | |
| R2.3 | v1 | P0 | A chip renders the item's current value in the collection's working set as the display showing it renders that value, and an item with no current value, or whose working-set value is absent, shows an empty chip, never a stand-in colour. A chip carries every R2.4 mark that applies to its item, each observable on its own. | pre-alignment | |
| R2.4 | v1 | P0 | The honesty marks are the nine below; each shows wherever this PRD renders the value it describes, and no colour this PRD renders is presented as faithful where a mark says otherwise. | pre-alignment | |

**The honesty marks.** One lettered row per mark; every cell is a rule ([R2.4](#2-the-collection-table-and-colour-honesty)).

| ID | Mark | Shown when |
|---|---|---|
| R2.4a | cannot-show | The display showing the window cannot render the item's current value, as R2.5 works it out. |
| R2.4b | outside-sRGB | The current reading's gamut-clipped flag is set (the Data Foundation PRD's R3.4). |
| R2.4c | simulated | The current reading's acquiring-device snapshot is of the simulated kind (the device PRD's R1.21 and R6.5). |
| R2.4d | non-spectral | The current reading is marked non-spectral (the Data Foundation PRD's R3.5; the capture PRD's R4.24). |
| R2.4e | samples-disagreed | The current reading's recorded agreement verdict is that its samples disagreed and their average was accepted (the capture PRD's R4.9–R4.11). |
| R2.4f | value-absent | The current reading's working-set value is absent because its condition is missing (the Data Foundation PRD's R3.3d and R3.5). |
| R2.4g | no-value | The item has no current value and no quarantined current reading: it was never scanned, or it is set aside for a cause other than unreadable. |
| R2.4h | unreadable | The item's current reading is quarantined (the Data Foundation PRD's R5.5b), the item being set aside with the cause unreadable (the capture PRD's R8.18). |
| R2.4i | re-scan-unanswered | A reading on the item is unsettled, awaiting the answer the Data Foundation PRD's R2.4 and R2.8 ask for. |

| ID | Release | Pri | Requirement | Status | Commit PR |
|---|---|---|---|---|---|
| R2.5 | v1 | P0 | The cannot-show mark is worked out against the display gamut of the display showing the window, as OQ 7's interim states, and worked out again within BROWSE_RESPONSE_BUDGET when the window moves to a display with a different gamut. A test can declare that gamut as sRGB, Display P3, or none reported. | pre-alignment | |
| R2.6 | v1 | P0 | The collection surface renders the device PRD's E22 while any item in the collection has a simulated current reading, and E22's action applies R3.4's simulated filter, leaving exactly those items listed. A reading's simulated mark stays on it in the version history view whatever the item's current reading is (the device PRD's R6.5). | pre-alignment | |
| R2.7 | v1 | P0 | The Spread column shows the current reading's recorded sample spread (the capture PRD's R4.9 and R4.11), and shows none for a reading of one sample. | pre-alignment | |
| R2.8 | v1 | P0 | "Colour marks" renders E12, naming each mark R2.4 and R5.2d list and what it means, and its "Done" closes it. | pre-alignment | |
| R2.9 | v1 | P1 | With no session in flight on the collection, the user drags a pending row to a new queue position while the table is in queue order, and fires "Use as scan order" while the table is view-sorted on Swatch Code, Swatch Name, an alternate or an imported column, both under the capture PRD's R6.9–R6.12 with its E33 and E34. While a session on the collection is in flight neither is offered, reordering then being the capture PRD's queue list's (its R6.8). | pre-alignment | |
| R2.10 | v1 | P2 | The user hides or shows any table column except the chip and Swatch Code, and the choice is kept per collection, with that collection, in the user's file (the Data Foundation PRD's R1.1), nothing about it being kept elsewhere. | pre-alignment | |

### 3. Search, filter and sort

Traces UJ 3, UJ 7; serves the vision use case *see the collection honestly* (U7) and the P0 feature
*collections and version history*. F4 decides the lightness, chroma and hue sorts and Find similar;
the Data Foundation PRD hands this PRD the capture PRD's R6.9 comparison.

| ID | Release | Pri | Requirement | Status | Commit PR |
|---|---|---|---|---|---|
| R3.1 | v1 | P0 | Typing in the search field lists only the items whose Swatch Code or Swatch Alternate Code starts with the text, or whose Swatch Name, Swatch Alternate Name or imported value contains it, both sides compared after the import PRD's R2.3 normalisation. The table lists the result within BROWSE_RESPONSE_BUDGET of the last keystroke. | pre-alignment | |
| R3.2 | v1 | P0 | Firing a column header view-sorts the table by that column and firing it again reverses the order; text compares as the capture PRD's R6.9 compares codes, items with no value in the column follow every item with one in either direction, and ties keep their existing order. A view sort never changes queue order (the capture PRD's R6.7). | pre-alignment | |
| R3.3 | v1 | P0 | L*, C* and h° sort by the item's current value in the collection's working set, and in an h° sort, in either direction, items whose C* is below NEUTRAL_CHROMA follow the chromatic items in ascending L*. | pre-alignment | |
| R3.4 | v1 | P0 | "Filters" narrows the table by row state — pending, captured, set aside — and by any R2.4 mark, values within one filter combining with or and separate filters with and. While a search or a filter is active E3 and E13 render their "narrowed" variant, and "Clear filters" and "Clear search" each remove only their own narrowing. | pre-alignment | |
| R3.5 | v1 | P0 | When a search with no filter active lists no item, E4 renders; when any filter is active and no item passes, E5 renders. | pre-alignment | |
| R3.6 | v1 | P0 | The search, filters and view sort of each collection and of the All items view persist while the file stays open, independently of one another, and none is kept once the file closes. | pre-alignment | |
| R3.7 | v1 | P1 | "Find similar" on an item with a current value renders E9, listing nearest first with each ΔE2000 the items in the same collection — or in every collection when fired from the All items view — whose current value is at or within FIND_SIMILAR_DISTANCE of it. Only values worked out under the chosen item's illuminant, observer and measurement condition are compared, the rest being counted as not compared, and no item is ever matched against a named colour library. | pre-alignment | |
| R3.8 | v1 | P1 | E9 renders its "none" variant when no other item is at or within that distance, and its "Done" closes it; "Find similar" is not offered on an item with no current value. | pre-alignment | |

### 4. Item detail and editing

Traces UJ 4; serves the vision use case *fix a bad scan without losing history* (U5) and journey
J3's closing step. The Data Foundation PRD's R2.3, R7.6g and F17 set what an edit may touch and what
the detail shows; the import PRD's R2.6 hands this PRD column renaming, and the capture PRD's R9.9 the
Flag entry point.

| ID | Release | Pri | Requirement | Status | Commit PR |
|---|---|---|---|---|---|
| R4.1 | v1 | P0 | Opening an item renders E14 with R4.2's lines, and closing it returns to the table with its search, filters, view sort, selection and scroll position as they were before it opened. | pre-alignment | |
| R4.2 | v1 | P0 | The item detail shows the lines below, the Data Foundation PRD's R7.6g among them. | pre-alignment | |

**What the item detail shows.** One lettered row per line; every cell is a rule ([R4.2](#4-item-detail-and-editing)).

| ID | Line | What it shows |
|---|---|---|
| R4.2a | Identity | Swatch Code, Swatch Name, both alternates, the collection's name, and every imported column with its value. |
| R4.2b | State | The row state, and for a set-aside item its cause — unreadable where its current reading is quarantined — and whether it is settled (the capture PRD's R8.2 and R8.18). |
| R4.2c | Current value | The chip with its marks and the current value in each of the six derived spaces, each with its illuminant, observer, measurement condition and derivation version (the Data Foundation PRD's R3.1 and R3.2); or, for an item with none, that it has no current value. |
| R4.2d | The current reading | Its measurement time; its acquiring device's kind, model, serial and firmware version (the device PRD's R1.21); its samples kept, averaging basis, recorded spread and agreement verdict. |
| R4.2e | Marks | Every R2.4 mark that applies, and the non-spectral reference mismatch where the Data Foundation PRD's R3.3e sets one. |
| R4.2f | History | How many readings the item holds, and "Show history". |
| R4.2g | Damage | The Data Foundation PRD's E4 in its current or history variant, or its E31, wherever that PRD's R5.5a–c applies. |
| R4.2h | Re-scans to answer | The Data Foundation PRD's E11 for each unsettled reading on the item. |

| ID | Release | Pri | Requirement | Status | Commit PR |
|---|---|---|---|---|---|
| R4.3 | v1 | P0 | Swatch Name, Swatch Alternate Code, Swatch Alternate Name and each imported column's value can be set or cleared in the item detail, and neither such an edit nor a code change under R4.4 changes a reading, the queue order or the row state. An edit lands in the file when the user presses Return or leaves the field, and Escape before then discards it. | pre-alignment | |
| R4.4 | v1 | P1 | A Swatch Code entered through "Change code" that is blank renders E15, and one equal under the import PRD's R2.3 rule to another item's code in the collection renders E15's "duplicate" variant, the old code staying in both cases, and E15's "OK" closes it; any other renders E16, whose "Change the code" stores it and "Keep this code" keeps the old one. While a session on the collection is in flight R8.3 governs "Change code". | pre-alignment | |
| R4.5 | v1 | P0 | "Delete swatch", in the item detail or on the one selected row, opens the Data Foundation PRD's E8 for that item, whose actions behave as that PRD's R6.2 states; while a session on the collection is in flight R8.3 governs it. | pre-alignment | |
| R4.6 | v1 | P0 | The item detail offers the capture PRD's re-scan entry for the item (its R8.7) once that PRD's re-scan rows land, and the actions of the Data Foundation PRD's E4 and E11 shown there act as that PRD's R7.3k and R2.3c–e state. | pre-alignment | |
| R4.7 | v1 | P1 | "Undo" reverses committed metadata changes — a field edit, a code change, a column rename, a bulk set or clear — most recent first while the file stays open. A replaced value held for that is never kept where an outside reader of the file can recover it (the Data Foundation PRD's R6.2d). | pre-alignment | |
| R4.8 | v1 | P1 | "Rename column" changes the name the file stores for one imported column, so the table, an export and an outside reader show the new name and a later import matches on it (inherited obligation for the Data Foundation and Inventory Import docs). A blank name renders E11, and a name equal under the import PRD's R2.3 rule to another column of the collection or to a Swatch field's name renders E11's "duplicate" variant, the old name staying in both cases and E11's "Change the name" returning to editing it. | pre-alignment | |
| R4.9 | v1 | P1 | On a captured item the item detail offers the capture PRD's Flag action (its §12 Labels), which sets the item aside as that PRD's R5.6 and R9.9 state, its reading kept as history; the action is not offered on an item that is not captured. While a session on the collection is in flight R8.3 governs it. | pre-alignment | |

### 5. Version history and corrections

Traces UJ 5; serves the vision use case *fix a bad scan without losing history* (U5). The Data
Foundation PRD's R2.5, R6.1 and F15 hand this PRD the over-time rules, the one-reading rule and
history's reachability; its R2.8 hands it the unanswered set.

| ID | Release | Pri | Requirement | Status | Commit PR |
|---|---|---|---|---|---|
| R5.1 | v1 | P0 | "Show history" in the item detail renders E17, listing every reading the item holds — current, superseded, never true, unsettled or quarantined — each with R5.2's lines, without closing the item detail or changing what the table lists. | pre-alignment | |
| R5.2 | v1 | P0 | Each reading in the version history view shows the lines below. | pre-alignment | |

**What each reading shows.** One lettered row per line; every cell is a rule ([R5.2](#5-version-history-and-corrections)).

| ID | Line | What it shows |
|---|---|---|
| R5.2a | Times | Its measurement time and its record time. |
| R5.2b | Device | Its acquiring device, with the simulated mark where that snapshot is of the simulated kind. |
| R5.2c | Reason | Its supersession reason (the Data Foundation PRD's R2.4), and whether it is the current reading. |
| R5.2d | Standing | The never-true mark, the unsettled mark, and the unreadable mark where it is quarantined (the Data Foundation PRD's R2.5 and R5.5c). |
| R5.2e | Value | Its chip with the R2.4 marks that apply to its own value, its samples kept and its recorded spread. |

| ID | Release | Pri | Requirement | Status | Commit PR |
|---|---|---|---|---|---|
| R5.3 | v1 | P0 | E17's body lists readings newest-recorded first, and its "measured" variant lists them by measurement time as an over-time view that leaves out never-true readings and states how many, keeps readings superseded by a re-measurement as ordinary points, and shows unsettled readings with their mark (the Data Foundation PRD's R2.5). "Recorded order" and "Measured order" switch between the two. | pre-alignment | |
| R5.4 | v1 | P1 | Each earlier reading shows its ΔE2000 from the current value when both were worked out under the same illuminant, observer and measurement condition, and otherwise shows that distance as absent and marked, never computed across the two. | pre-alignment | |
| R5.5 | v1 | P1 | "Use this reading" on a readable earlier reading makes a new current value equal to it, as the Data Foundation PRD's R2.3f states, and is offered on an item with a current value, one whose current reading is quarantined (that PRD's R5.5b), or one set aside by a Flag (the capture PRD's R5.6), the flagged reading included, either of the last two becoming captured again (inherited obligation for the Capture Mode and Data Foundation docs). It is never offered on a quarantined reading, nor on an item with no current value for any other reason. | pre-alignment | |
| R5.6 | v1 | P0 | No surface this PRD renders offers an action that removes one reading from an item's history (the Data Foundation PRD's R6.1). | pre-alignment | |
| R5.7 | v1 | P0 | While the file holds any unsettled reading, E2 offers "Answer re-scans", which opens the Data Foundation PRD's E26 (its R2.8); while it holds none, the action is not offered. | pre-alignment | |
| R5.8 | v1 | P2 | "Compare" on two readings selected in E17 shows their two chips side by side with their marks, and their ΔE2000 under R5.4's rule. | pre-alignment | |

### 6. Selection and bulk operations

Traces UJ 6; serves the P0 feature *collections and version history*. F3 decides which bulk
operations exist, and F7 their priority, BULK_CONFIRM_COUNT's candidate and the selection-scale
confirmation.

| ID | Release | Pri | Requirement | Status | Commit PR |
|---|---|---|---|---|---|
| R6.1 | v1 | P1 | The table selects several items by range and by adding or removing single rows, shows how many are selected, and "Select all" selects every item the current search and filters list, whether or not its row is on screen. A change to the search or filters deselects every selected item it stops listing. | pre-alignment | |
| R6.2 | v1 | P1 | "Set a field" with two or more items selected sets one editable field to the value entered — an empty value clearing it — on exactly the selected items, the selected count staying in view while the field and value are chosen. Above BULK_CONFIRM_COUNT selected items it first renders E8 — its "clear" variant for a clear — and nothing changes until "Apply to ⟨n⟩ swatches". | pre-alignment | |
| R6.3 | v1 | P1 | "Delete selected" with two or more items selected opens the Data Foundation PRD's E33, which deletes every selected item or none, its actions behaving as that PRD's R6.2 states and R8.3 governing it while a session on the collection is in flight (inherited obligation for the Data Foundation doc). | pre-alignment | |

### 7. The swatch grid

Traces UJ 10; serves the vision use case *see the collection honestly* (U7) through the P2 feature
*gamut-aware swatch grid*, which F1 keeps at P2.

| ID | Release | Pri | Requirement | Status | Commit PR |
|---|---|---|---|---|---|
| R7.1 | v1 | P2 | "Grid" shows the collection surface's items as swatches no smaller than MIN_SWATCH_SIZE, each labelled with its Swatch Code and carrying every R2.4 mark on the swatch itself, and "Table" returns to the table. Switching either way keeps the search, filters, view sort, selection and the item in view. | pre-alignment | |
| R7.2 | v1 | P2 | The user sets the swatch size, never below MIN_SWATCH_SIZE, and an item with no current value shows R2.3's empty chip as its swatch. | pre-alignment | |

### 8. Operating envelope and quality attributes
<!-- guidance: mandatory, and always the LAST numbered requirement section. Its heading takes the
     number after the project's own requirement sections — assigned at scaffold time, before any
     ID exists, and never changed afterwards — so its rows are `R<that number>.<n>` and are
     ordinary requirement rows in every respect: same columns, same priorities, same statuses,
     same acceptance cases in the journeys companion, same named constants in the Legend's
     constants table. This is the class of requirement a builder most often has to go back to
     product for, so the format scaffolds it rather than leaving it to be remembered. -->

Traces UJ 9; serves the vision use case *see the collection honestly* (U7) and the delivery
commitments the vision's Use cases table and AGENTS.md §4 set: local-first, offline, user-owned.

The rows here state the operating envelope this product area must hold: latency and throughput
budgets, data volumes and their growth, concurrency and retry semantics, offline and
partial-failure behaviour, platform and version compatibility, durability, and security and privacy
posture. Not conditional — each of those classes is answered by at least one row here, or by a row
stating that this product area has no requirement of that class and why. A class left out is a
latent decision, not a silent "no requirement".

| ID | Release | Pri | Requirement | Status | Commit PR |
|---|---|---|---|---|---|
| R8.1 | v1 | P0 | A search keystroke, a filter change, a view sort, and a reading or edit saved anywhere in the app show in the table within BROWSE_RESPONSE_BUDGET at ROWS_CEILING items in the collection, and a chosen collection lists its first rows within OPEN_COLLECTION_BUDGET. A test can read the time from each input to the table listing its result, in a declared build configuration on a declared machine. | pre-alignment | |
| R8.2 | v1 | P1 | The All items view and Find similar hold BROWSE_RESPONSE_BUDGET at FILE_ITEMS_CEILING items across the file, and an item's version history view lists every reading it holds, however many, history never being aged out. | pre-alignment | |
| R8.3 | v1 | P0 | While a session on a collection is in flight, "Delete collection", "Delete swatch", "Delete selected", "Change code" and the capture PRD's Flag action on that collection render E6 and change nothing, and every other metadata edit stays available and moves no queue position, row state, remembered row or current row. A reading saved by any session shows in the table without resetting its search, filters, view sort, selection or scroll position. | pre-alignment | |
| R8.4 | v1 | P0 | With the file open read-only (the Data Foundation PRD's R5.3), the collection list, table, item detail and version history view still show every item, current value and mark, and no action this PRD offers that would write the file is available. An item whose readings cannot be read never stops the rest of its collection being browsed, searched, sorted or edited. | pre-alignment | |
| R8.5 | v1 | P0 | After the Data Foundation PRD's re-read (its R1.5 and R7.3c) the table, item detail and version history view show the file as re-read, keeping the search, filters and view sort and deselecting any item the file no longer holds. Noticing a change made outside the app is that PRD's OQ 14, and no row here does it. | pre-alignment | |
| R8.6 | v1 | P0 | Every row here works with no network connection, no action this PRD adds makes a network request, and nothing it reads or writes leaves the machine (the Data Foundation PRD's R1.4). Search text, filters and view sorts are never written to the file. | pre-alignment | |
| R8.7 | v1 | P0 | No row here sets a minimum macOS version or a display requirement: ADR-0006 selects the floor, and a row the selected floor cannot deliver goes back to the owner rather than being dropped. R2.5's mark is worked out on every display the window can be on, built-in or external. | pre-alignment | |
| R8.8 | v1 | P0 | Each committed edit, rename, reorder, restore and bulk operation lands in the file whole or not at all before any surface shows it done, so a crash or power loss leaves it complete or absent (the Data Foundation PRD's R1.10). A write the full volume refuses renders the Data Foundation PRD's E15 and changes nothing. | pre-alignment | |
| R8.9 | v1 | P0 | Every R2.4 mark has a shape, distinct from every other mark's, that does not depend on seeing colour, and an accessible name VoiceOver reads with the row's Swatch Code, Swatch Name and row state. Every action this PRD offers can be reached and fired from the keyboard. | pre-alignment | |
| R8.10 | v1 | P0 | A test can declare the file's contents (the Data Foundation PRD's R7.2), a collection's session state (the capture PRD's R11.6), the display gamut and the clock, and can list what the collection list, table, item detail and version history view show — rows in order, columns, each chip's marks and accessible description — and which copy state and variant is up, without matching wording. It can read the file with the app closed at SQLITE_READER_FLOOR (the Data Foundation PRD's R7.1) to assert what each action left there. | pre-alignment | |

## Inherited obligations
<!-- guidance: outbound first — every behaviour this PRD's rows require another document's product
     to implement. The inbound table is conditional on siblings existing: when they do, it carries
     the same three columns for what those documents require of this one.
     Standing check, run on every amendment: the obligation summary on each side of a seam says
     the same thing; a change on one side lands on the other IN THE SAME PR, with a dated
     clarification on the fence it touches. The most common missing thing in an amendment is the
     sibling half of a mirror. -->

Every cross-document cite in this PRD and its companions names the owning document in its visible
label — "the device PRD's `R6.27`", never a bare `R6.27` — because the row-ID families repeat
across PRDs. An ID written bare is this document's own.

**Outbound** — what this PRD requires of other documents:

| Target PRD | Obligation | Rows |
|---|---|---|
| Data Foundation | A selection-scale delete confirmation beside its E8 and E14 — its E33 — stating the selected item count and their current and earlier readings, with cancel as E8 offers it and an export first that opens the export of the whole collection holding the selection (the Data Export PRD's collection scope, which lists E33 among its export-first confirmations under its F31), tested by its R7.6o; made in this change (F7 and F24, and that PRD's F50 as clarified) | R6.3 |
| Capture Mode, Data Foundation | Restoring a readable earlier reading of an item set aside by a Flag — the flagged reading included — makes it captured again, and the operator's Flag is named beside damage as a way an item's current value is removed; made in this change in the capture PRD's R5.6 and the Data Foundation PRD's R2.9 (F23, and their F70 and F51) | R5.5 |
| Data Foundation, Inventory Import | A renamed imported column keeps its position and values under the new name the file stores, and a later import matches that name, a source header equal to the old name arriving as a new column; made in this change in the Data Foundation PRD's R1.2 and the import PRD's R2.6 (F9, and their F50 and F65) | R4.8 |

**Inbound** — what other documents require of this one:
<!-- conditional: include only when sibling PRDs exist; delete the heading and table otherwise. -->

| Target PRD | Obligation | Rows |
|---|---|---|
| Product README (§6, Coverage) | Browse at scale; search, filter, facet and sort; editing surfaces; selection and bulk operations; the version-history UI; the gamut-aware swatch grid; the UI half of U5 and the swatch-grid half of U7 | R1.1, R2.1, R3.1–R3.8, R4.1–R4.8, R5.1–R5.8, R6.1–R6.3, R7.1, R7.2 |
| Capture Mode | Its R1.8 (with R1.2 and F3): rename and delete a collection, name uniqueness under the one rule, a delete guard ending an active or interrupted session first, a warning naming captured rows and history and offering export first, delete never the default; its journeys' UJ1.2-a, UJ1.2-b and UJ1.2-c, carried as UJ 1's scenarios | the capture PRD's R1.2, R1.8 → R1.3, R1.4, R1.5, R1.6, R8.3 |
| Capture Mode | Its R4.11 (and F10): show the spread recorded on a reading | the capture PRD's R4.11 → R2.4e, R2.7, R4.2d, R5.2e |
| Capture Mode | Its R9.9 (and F70): a Flag from the collection on a captured item, doing what its R5.6 Flag does | the capture PRD's R5.6, R9.9 → R4.9, R8.3 |
| Capture Mode | Its R8.18 (and F70): an item whose current reading is quarantined is set aside with the cause unreadable and counted as set aside | the capture PRD's R8.18 → R2.4g, R2.4h, R4.2b |
| Capture Mode | Its R4.22 and R8.14: the simulated-readings banner, the recorded spread and the non-spectral mark surfaced where the collection is browsed | the capture PRD's R4.22, R8.14 → R2.4c, R2.4d, R2.6, R2.7 |
| Capture Mode | Its R6.8 (and F22): the sort and drag controls the operator orders the queue with, before a session, from the collection | the capture PRD's R6.8 → R2.9 |
| Capture Mode, through Data Foundation | Its R6.9: codes compare the same way wherever they are ordered | the capture PRD's R6.9 → R1.1, R3.2 |
| Capture Mode, Data Foundation | Marking a colour outside what a display can show — this PRD the showing, the Data Foundation PRD what the reading keeps | the capture PRD's obligations line → R2.4a, R2.5 |
| Capture Mode | The collection surface it shares: identical row states, vocabulary and counts (its R10.3), its states never hiding the surface's entry points (its R7.19, F43), its review offer, summaries and resume offer there (its R8.16, R7.15, R7.8), view sorts never changing queue order (its R6.7), and re-scan from an item (its R8.7) | the capture PRD's R6.7, R7.8, R7.15, R7.19, R8.7, R8.16, R10.3 → R2.1, R2.2, R3.2, R4.6 |
| Device Management | Its R6.5 and E22, and its journeys' UJ1.2-c: the banner while any item's current reading is simulated, the per-item mark following the current reading, the flag kept in history | the device PRD's R6.5 → R2.4c, R2.6, R5.2b |
| Data Foundation | Its R2.5: the three over-time display rules | the Data Foundation PRD's R2.5 → R5.2d, R5.3 |
| Data Foundation | Its R3.4 and F4: the gamut-clipped flag belongs to the reading, and any display-relative check is this PRD's | the Data Foundation PRD's R3.4 → R2.4a, R2.4b, R2.5 |
| Data Foundation | Its R3.5: the non-spectral mark and the absent-value mark | the Data Foundation PRD's R3.5 → R2.4d, R2.4f, R4.2e |
| Data Foundation | Its R6.1 and F15: no single reading is deletable out of history, and delete is never a default action | the Data Foundation PRD's R6.1 → R1.6, R5.6 |
| Data Foundation | Its obligations line: history reachable from an item without costing the primary view | the Data Foundation PRD's R2.3 → R4.1, R5.1 |
| Data Foundation | Its R6.2, R6.3, E8, E14 and E33: the counted delete confirmations, export first, and the P1 undo | the Data Foundation PRD's R6.2, R6.3 → R1.4, R1.7, R4.5, R6.3 |
| Data Foundation | Its R2.3, R2.4, R2.8, E11 and E26: restore, the correction question and the unanswered set reachable from the app and from each item | the Data Foundation PRD's R2.3, R2.4, R2.8 → R4.2h, R4.6, R5.5, R5.7 |
| Data Foundation | Its R5.5, E4 and E31: the damage states an item can be in, the rest of the collection still usable, and a restore of a readable earlier reading when the current one is quarantined | the Data Foundation PRD's R5.5 → R2.4h, R4.2g, R5.5, R8.4 |
| Data Foundation | Its R2.3, R6.2d and F17: metadata edited and cleared in place, no prior text kept | the Data Foundation PRD's R2.3 → R4.3, R4.7, R6.2 |
| Data Foundation | Its R7.6g: the item detail's data lines | the Data Foundation PRD's R7.6g → R4.2 |
| Data Foundation | Its R5.3 and F39: the read-only browsing floor | the Data Foundation PRD's R5.3 → R8.4 |
| Data Foundation | Its R1.5, R1.4 and E9: re-reading the file, and no outbound traffic | the Data Foundation PRD's R1.4, R1.5 → R8.5, R8.6 |
| Data Foundation | Its R1.1 (and this PRD's F21): nothing about a collection is kept outside the file | the Data Foundation PRD's R1.1 → R2.10 |
| Inventory Import | Its R2.6 and obligations line (and F65): renaming imported columns, a rename changing the name the file stores and a later import matches | the import PRD's R2.6 → R4.8 |
| Inventory Import | Its R2.3: the one matching rule for codes and collection names | the import PRD's R2.3 → R1.3, R3.1, R4.4 |
| Data Export | Its R1.1: export of a collection or a single item | the export PRD's R1.1 → R1.8 |

## Error and state copy index
<!-- guidance: the index of the copy states, plus the three standing rules below. The copy text
     itself — headline, body, actions, variants — lives in the copy companion and nowhere else.
     Labels rule: the copy companion is the ONE place a user-facing label is written, and every
     action a row names is quoted from it.
     Placeholders rule: name the tokens in use, the zero-count rule (what a count token renders as
     at zero), and which row a ⟨code⟩ token names.
     Variant enumeration rule: the row that enumerates a state's variants lists the variant names
     verbatim.
     The Labels rule is enforced by a runnable search over the whole product-docs tree, run on
     every copy or row amendment. The command lives with the mechanical checks (check 13, "The
     standing label check, run") and is not carried in this document: it is how the document is
     checked, not something a builder reads. -->

| ID | State | Surface | Owning rows | Status |
|---|---|---|---|---|
| E1 | No collections yet | Collection list | R1.1 | pre-alignment |
| E2 | Collections | Collection list | R1.1, R5.7 | pre-alignment |
| E3 | Collection shown | Collection surface | R2.1, R3.4 | pre-alignment |
| E4 | No search matches | Collection surface; All items view | R3.5 | pre-alignment |
| E5 | Nothing passes the filters | Collection surface; All items view | R3.5 | pre-alignment |
| E6 | Waiting for a session to end | Collection surface; item detail | R1.4, R1.5, R4.9, R8.3 | pre-alignment |
| E7 | Collection name needed | Collection surface | R1.3 | pre-alignment |
| E8 | Change a field on many swatches | Collection surface | R6.2 | pre-alignment |
| E9 | Colours close to a swatch | Collection surface; All items view | R3.7, R3.8 | pre-alignment |
| E10 | Deleted, undo available | Collection surface; item detail | R1.7 | pre-alignment |
| E11 | Column name not accepted | Collection surface | R4.8 | pre-alignment |
| E12 | What the marks mean | Collection surface; All items view | R2.8 | pre-alignment |
| E13 | All items shown | All items view | R1.9, R3.4 | pre-alignment |
| E14 | Swatch detail | Item detail | R4.1 | pre-alignment |
| E15 | Code not accepted | Item detail | R4.4 | pre-alignment |
| E16 | Change a swatch's code | Item detail | R4.4 | pre-alignment |
| E17 | History | Version history view | R5.1, R5.3 | pre-alignment |

**Labels rule.** `docs/product/collection-mode/prd-collection-mode-copy.md` is the one place a user-facing label is
written; every action a row names is quoted from it. A sibling's action that a row here relies on is
named by its document and state, never quoted here.

**Placeholders rule.** The tokens in use: ⟨collection⟩ a collection's name; ⟨code⟩ the Swatch Code
of the item the state is about — the item opened (E14, E16, E17), the item Find similar started from
(E9), the item deleted (E10) or the item whose code was refused (E15); ⟨name⟩ that item's Swatch
Name; ⟨text⟩ what the user typed; ⟨column⟩ a column's name; ⟨value⟩ a value being set; ⟨n⟩ a count
of swatches, collections or readings; ⟨shown⟩ the swatches a search or filter lists; ⟨collections⟩
the collections in the file; ⟨excluded⟩ the swatches Find similar did not compare; ⟨left⟩ the
readings an over-time view leaves out; ⟨scope⟩ the collection's name, or all your collections; and
⟨distance⟩ FIND_SIMILAR_DISTANCE's current number. A sentence whose count would be zero is left out
rather than showing 0, with the punctuation that joined it; a sentence carrying a count agrees with
it — 1 swatch, ⟨n⟩ swatches; and a named constant renders as the number it currently holds.

**Variant enumeration rule.** A copy state that has variants names, in its copy-companion entry,
the requirement row that enumerates its variant set, and that row lists the variant names verbatim.
A variant no row enumerates is untestable without matching on wording; an enumerated variant with
no copy is text the builder would have to invent.

## Success metrics
<!-- guidance: precise enough that two people computing the same metric from the same data get the
     same number. The Method column names the OBSERVABLE a test or a dogfood runbook reads — not
     "measure adoption" but the event, surface or file it is read from; evidence artifacts are
     named files. Not conditional. -->

| ID | Metric | Definition (start event, end event, statistic, population) | Candidate target | Method | Status |
|---|---|---|---|---|---|
| M1 | Browse response time | Start: a search keystroke, a filter change or a header fired on the collection surface. End: the table listing the result. Statistic: the 95th percentile per input kind. Population: 200 inputs of each kind against one collection of ROWS_CEILING items, in a Release build, on each machine OQ 1 names. | ≤ BROWSE_RESPONSE_BUDGET | R8.1's timing readback; UJ9.5-a is its worked oracle. | pre-alignment |
| M2 | Honesty-mark agreement | Start: a seeded file whose every item's R2.4 conditions are declared, and a declared display gamut. End: the marks R8.10 lists for each chip. Statistic: the share of item-and-mark pairs where the listed mark agrees with the declared condition. Population: every item of the harness's seeded file under each of sRGB, Display P3 and no reported gamut, every PR. | 100% | R8.10's listing of each chip's marks against R2.4's conditions; UJ9.8-a is its worked oracle. | pre-alignment |
| M3 | Readings lost to a Collection Mode action | Start: the file before a field edit, a code change, a column rename, a bulk set or clear, a reorder, a restore or a re-scan answer. End: the file read with the app closed at SQLITE_READER_FLOOR after it. Statistic: the count of readings present before that are not readable after. Population: every such action the journeys run, every PR. | 0 | R8.10's read of the file at SQLITE_READER_FLOOR; UJ9.8-b is its worked oracle. | pre-alignment |

## Open questions
<!-- guidance: every question this PRD cannot yet answer that blocks a row. Closer names who or
     what closes it (the evidence, the run, the owner call), Feeds names the row IDs it blocks; an
     OQ with no row it feeds is scope creep, not a blocker. Reserved numbers stay reserved: a
     withdrawn OQ keeps its number rather than freeing it. Not conditional. -->

| # | Question | Decision so far | Interim rule | Closer | Feeds (row IDs) | Status |
|---|---|---|---|---|---|---|
| 1 | BROWSE_RESPONSE_BUDGET and OPEN_COLLECTION_BUDGET: how fast must browsing answer at scale, and on which machines? | F20 holds 100 ms and 1 s as candidates under this question. The browsing research measured full-text search at 0.49–10.66 ms and exhaustive ΔE2000 at 9.5 ms over 100,000 rows on one Apple M5 Max, against Nielsen's 0.1 s limit (per research, confirm on verification, OQ 1); nothing of this app is measured. | 100 ms at the 95th percentile for search, filters, view sorts, Find similar and a saved change appearing; 1 s for a chosen collection's first rows. | Engineering times Release builds at ROWS_CEILING on the lowest-specified Mac the owner names and one current Mac; the owner ratifies. | R2.5, R3.1, R8.1, M1 | pre-alignment |
| 2 | FILE_ITEMS_CEILING: how many items must the file-wide views hold up under? | None. The capture PRD's ROWS_CEILING sizes one collection (its OQ 13); nothing sizes a file of several. | ROWS_CEILING items across the whole file. | The owner, on the capture PRD's OQ 13 scale evidence. | R8.2 | pre-alignment |
| 3 | FIND_SIMILAR_DISTANCE | F16 holds ΔE2000 3.0 as the candidate under this question. | ΔE2000 3.0, at or within. | A dogfood Find similar pass over a real collection of at least 200 items, recording how many items each query lists; the owner. | R3.7 | pre-alignment |
| 4 | BULK_CONFIRM_COUNT | F7 holds 10 items as the candidate under this question. | Confirm a bulk set or clear across more than 10 items. | Dogfood bulk edits, then the owner. | R6.2 | pre-alignment |
| 5 | NEUTRAL_CHROMA: below what chroma is an item treated as having no meaningful hue? | F19 holds C* 3.0 as the candidate under this question; no source measured. | C* 3.0. | A dogfood hue sort of a collection holding a grey series; the owner. | R3.3 | pre-alignment |
| 6 | MIN_SWATCH_SIZE | F20 holds 24 pt as the candidate under this question. The browsing research places a perceptual floor near 10 pt at 600 mm and recommends a hard floor near 24 pt (per research, confirm on verification, OQ 6). | 24 pt. | The owner at the P2 build, on the owner's display at working distance. | R7.1, R7.2 | pre-alignment |
| 7 | What decides that the display cannot show a colour: which gamut, which rendering intent, and a display reporting none? | The Data Foundation PRD fixes the stored flag at sRGB and relative colorimetric and leaves the display check here (its F4); the research names the gamut-check's rendering-intent behaviour as unmeasured (per research, confirm on verification, OQ 7). | Test the current value against the display's reported gamut under relative colorimetric, and treat a display reporting none as sRGB. | An engineering spike on one sRGB and one Display P3 display with the harness's seeded file; the owner. | R2.5, M2 | pre-alignment |
| 8 | Does renaming an imported column change the name the file stores? | Answered by F9 (R4.8): yes — the file stores the new name and a later import matches it; the Data Foundation PRD's R1.2 and the import PRD's R2.6 are amended in this change. | | Closed — the owner (F9); the results file carries the answer. | R4.8 | aligned |
| 9 | How is deleting a selection confirmed? | Answered by F7 (R6.3): by the Data Foundation PRD's selection-scale confirmation, its E33, added in this change beside its E8 and E14. | | Closed — the owner (F7); the results file carries the answer. | R6.3 | aligned |
| 10 | When may undo of a delete be built? | The Data Foundation PRD's OQ 20 gates its R6.3 on pending-delete visibility and identity reuse. | R1.7 is not built and E10 does not appear. | The Data Foundation PRD's OQ 20. | R1.7 | pre-alignment |
| 11 | Which published ΔE2000 values do the Find similar and distance cases use? | The journeys use CIEDE2000 test pairs 1–5 from Sharma, Wu and Dalal (2005); the browsing research reproduced pair 1's 2.0425 (per research, confirm on verification, OQ 11). | Use the pairs as the journeys quote them. | Engineering checks every quoted pair against the published table before the first such case runs. | R3.7 | pre-alignment |

Every open question carries an interim rule, or its P0 rows are listed in the Legend's no-interim
bullet — there is no third option, and which of the two applies is what tells a builder whether it
may start the rows that question feeds.

Results file: `docs/product/collection-mode/prd-collection-mode-oq-results.md`, one `## OQ <id>` section per
answer; an OQ's Status may change only when its section exists. Every inline
confirm-on-verification marker in this document carries its OQ id; a marker without a matching OQ
is invalid.

---

Upstream: this PRD traces to the product vision at `docs/product/vision.md` and the current
product strategy at `STRATEGY.md`. Where it narrows or overrides either, the
build contract says so.
<!-- guidance: docs/product/vision.md is the durable WHY this PRD serves; STRATEGY.md
     is the active bets and roadmap it sits inside. Both are project-supplied paths, substituted at
     scaffold. A divergence from either is stated in the build contract, never left implicit. -->
