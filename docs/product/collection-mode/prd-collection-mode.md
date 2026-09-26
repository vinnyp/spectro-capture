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
the collection surface's item table and — last — its swatch grid, and the All items view; every honesty mark where this PRD renders colour, whether the display in use can show it included; search, filters, view sorts, the lightness, chroma and hue sorts and "Find similar"; the item
detail and editing an item's metadata, its Swatch Code and an imported column's name; the
version-history view, restoring an earlier reading, and where the correction question is reached;
renaming and deleting a collection, deleting an item or a selection, and setting or clearing one
field across a selection; and the controls that reorder a collection's scan queue from the
collection. The Capture Mode PRD owns creating a collection and its capture settings, the queue
order and what a reorder does to it, sessions and the set-aside list, re-scanning, and every entry
point and state it places on the collection surface. The Data Foundation PRD owns the file and what
it keeps — readings, current values, supersession reasons and their marks, derived values and the
stored gamut-clipped flag, damage, re-reading the file, and what a restore and a delete do —
together with its confirmation, question and failure states. The
Inventory Import PRD owns the one matching rule and how imported columns arrive; the Data Export
PRD owns what an export contains once it is started; the Device Management PRD owns the
simulated-readings banner's copy (its E22) and simulated provenance; the QC & Comparison PRD owns
checking a swatch against its reading, and the Color Visualization PRD the 3D plot, neither yet
written. This PRD deliberately does not cover exporting only a selection or
answering the correction question for a selection, matching against any named
colour library, any level of organisation above collections, an app-owned notes field or a column added by hand, moving an item between collections, copying a value beyond the platform's text selection, deleting or merging a column, and multi-collection compare or printer gamut analysis.

Rows own behaviour; acceptance scenarios supply fixtures, actions, oracles and source rows; copy
owns displayed text; fences own decisions. Preserve row IDs, priorities, statuses and historical
decisions; an amendment is not an implementation completion. Whether capture is a full-window
takeover that hands off to the collection when a session ends, or a state of the live collection,
is open — the capture PRD's R10.6 and OQ 8, and ADR-0004 — and neither reading is selected here:
every row states behaviour on the collection surface without deciding how the user reached it or
where it sits relative to capture, and where the session summary sits stays the capture PRD's F20.

## Row transitions
<!-- guidance: one table, one row per route between the states of the entity this PRD governs,
     each naming the requirement rows that own the route. This section REPLACES the lifecycle
     diagram: a diagram is read, a table is checked.
     Enumerate the routes FROM THE REQUIREMENT ROWS, not from memory — walk every row that can
     change the entity's state and give its route a line here. A route any row can cause and this
     table omits is a defect (two reviewers independently caught one omitted route in the first
     conversion run of this format). -->

The `Starting state` and `Result` cells carry only terms the Vocabulary marks `(state)`. The entity
is an item or a collection as this PRD shows it; a reading's states are the Data Foundation PRD's
and a row's queue states the capture PRD's, and a route changing one names the row it acts through.

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
| present | The user hides or shows a table column | present | R2.10 |
| present | The user applies a bulk set or clear | present | R6.2 |
| present | The user fires "Undo change" on a committed metadata change R4.7 accepts | present | R4.7 |
| present | The user restores an earlier reading, a new current value resulting (the Data Foundation PRD's R2.3f) | present | R4.6, R5.5 |
| present | The user answers a re-scan question from the item detail or through "Answer re-scans" | present | R4.6, R5.7 |
| present | The user confirms E18 on a captured item, which the capture PRD's R5.6 sets aside | present | R4.9 |
| present | The user reorders a collection's queue from the collection surface | present | R2.9 |
| present | A re-read finds it gone, removed outside the app | removed | R4.1, R8.5 |

## User journeys
<!-- guidance: one paragraph, no table. The journeys themselves are acceptance scenarios and live
     in the journeys companion; this paragraph points at it and states the three things a reader
     needs to use it: the `### UJ n. <name>` headings are stable anchors (cite them, never
     renumber them), cases run per phase, and hardware- or environment-gated findings stay behind
     their open questions rather than being asserted here. -->

The user journeys are acceptance scenarios in
`docs/product/collection-mode/prd-collection-mode-journeys.md`, whose `### UJ n. <name>` headings
are stable anchors: cite them, never renumber or retitle them. A case for a row, state or variant a
phase mark or a P1 or P2 priority holds back runs in the phase that lands it, and a finding that
depends on an unmeasured environment (timings, display gamut) stays behind its open question.

## Requirements

### Vocabulary
<!-- guidance: every term the rows use, one line each — a term a row leans on and this list does
     not define is a latent decision. Include the product's measurement tiers where it has them
     (e.g. the raw, the stored and the computed form of whatever this product measures) and EVERY
     state name the row-transitions table uses. Not conditional. -->

A term naming a state of the entity this PRD governs is written `- **<term>** (state) — …`, and
the row-transitions table uses only terms marked that way. The Data Foundation PRD's reading, sample,
derived value, supersession reason, quarantined and chosen condition, and the capture PRD's
pending, captured, set aside, settled, session and remembered row, are used unchanged.

- **the file** — the one SQLite file holding all the user's collections (the Data Foundation PRD's
  R1.1).
- **the file's bytes** — the bytes of the file and of any journal, log, index or other file the app
  keeps beside it.
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
- **current value** — an item's canonical value, at most one; none when never scanned or set aside,
  a quarantined current reading setting the item aside as unreadable (the capture PRD's R8.18).
- **working set** — the derived values of the collection's chosen condition under its illuminant
  and observer (the Data Foundation PRD's R3.1).
- **collection list** — the surface naming every collection in the file.
- **collection surface** — the surface one collection is browsed on, carrying the capture PRD's
  entry points and states beside this PRD's browsing.
- **item table** — the collection surface's list of items, one row each.
- **All items view** — a view listing every item of every collection; it holds nothing of its own
  and is no level above collections.
- **item detail** — the surface showing one item's lines and actions.
- **version history view** — the surface listing every reading one item holds.
- **colour chip** — the patch in a row, the detail or the history that renders a value.
- **honesty mark** — one of R2.4's marks: a sign, perceivable without seeing colour, that a value
  is not what its colour alone suggests.
- **mark identifier** — a mark's name in R2.4 or R5.2d, by which a test reads it; the copy file's
  Mark labels table words it.
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
- **awaiting answer** — the mark on a reading whose successor's reason is correction-unconfirmed.
- **ΔE2000** — the CIEDE2000 colour difference (AGENTS.md §8).
- **named constant** — a product parameter in the Legend's constants table, named in its owning
  rows.
- **present** (state) — the item or collection is in the file and listed wherever its collection is
  browsed.
- **deleted-undoable** (state) — the item or collection has been deleted, is listed nowhere, and
  "Undo" can restore it until the Data Foundation PRD's DELETE_UNDO_WINDOW ends.
- **deleted** (state) — the item or collection is gone from the file for good, unrecoverable by an
  outside reader, anyone reading the file's bytes included (the Data Foundation PRD's R6.2a).
- **removed** (state) — gone from the file by a change outside the app, as a re-read finds; no byte
  guarantee applies.

### Legend
<!-- guidance: the vocabularies every later table depends on: priority semantics (exactly one of
     the two bullets survives to lock — delete the other), the phase rule, the status vocabulary,
     then the two tables and the two bullets below. Not conditional. -->

**Priority — the semantic the `Pri` column carries in this release:**

- **Build order within the release — nothing droppable.** Every P0/P1/P2 row but a deferred R1.7 ships in this
  release; priority only orders the sequence work happens in.

| Pri | Meaning |
|---|---|
| P0 | Blocks the first usable build: a collection cannot be browsed, searched, read honestly, edited, deleted, or its history reached without it. |
| P1 | Built after the P0 rows, inside v1. |
| P2 | Built last inside v1. |

**Release.** The `Release` column names the release a row ships in, in the project's own
vocabulary; priority orders work inside it, and an unassigned row leaves the cell empty.

**Phase rule.** A P0 state offering an action whose rows are of a later priority shows without it until those rows
land. A phase mark names one of three kinds of absence — `[phase: surface-absent]`, a surface not
yet built; `[phase: action-absent]`, an action absent from a surface that exists; and
`[phase: variant-absent]`, a body variant not yet produced — the complete set, here and in the copy
companion.

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
| BROWSE_RESPONSE_BUDGET | 100 ms at the 95th percentile; interim rule — build and test against 100 ms on OQ 1's interim Mac | R2.5, R3.1, R8.1, R8.2 | OQ 1 — its timing run |
| OPEN_COLLECTION_BUDGET | 1 s; interim rule — build and test against 1 s on the same Mac | R8.1d, R8.2 | OQ 1 — the same timing run |
| DROPPED_FRAME_SHARE | 1% of frames missed while paging through ROWS_CEILING rows; interim rule — the same 1% on the same Mac | R8.1e | OQ 1 — the same timing run |
| BULK_WRITE_BUDGET | 2 s; interim rule — build and test a bulk set, clear or reorder at ROWS_CEILING items against 2 s on the same Mac | R8.1f | OQ 1 — the same timing run |
| DELETE_WRITE_BUDGET | 10 s; interim rule — the same, for a selection or collection delete of ROWS_CEILING items and its undo | R8.1g | OQ 1 — the same timing run |
| HISTORY_READINGS_CEILING | 500 readings on one item; interim rule — history opens are built and tested at 500 | R8.1b | OQ 1 — the owner's estimate of the longest history a real item reaches, timed by UJ9.5-e |
| IMPORTED_COLUMNS_CEILING | 20 imported columns, each value up to 200 characters; interim rule — the budgets are built and tested at that width | R8.1, R8.2 | OQ 12 — the widest inventory the owner dogfoods |
| FILE_ITEMS_CEILING | 100,000 items; interim rule — design and test the file-wide views at 100,000 items across the whole file | R8.1, R8.2 | OQ 2 — the owner's estimate of collections per file, checked by UJ9.5-b |
| FIND_SIMILAR_DISTANCE | ΔE2000 3.0; interim rule — list items at or within 3.0 | R3.7 | OQ 3 — a dogfood "Find similar" pass over a real collection |
| BULK_CONFIRM_COUNT | 10 items; interim rule — a bulk set or clear across more than 10 items confirms first | R6.2 | OQ 4 — dogfood bulk edits, then the owner |
| NEUTRAL_CHROMA | C* 3.0; interim rule — a hue sort places items below C* 3.0 after the chromatic ones | R3.3 | OQ 5 — a dogfood hue sort of a collection holding a grey series |
| MIN_SWATCH_SIZE | 24 pt; interim rule — no swatch renders smaller than 24 pt on a side | R7.1, R7.2 | OQ 6 — the owner at the P2 build, on the owner's display at working distance |

This table is an index; each constant's owning row is its home and governs. ROWS_CEILING,
FIND_BUDGET, TRIGGER_ACK_WINDOW and ROW_CONFIRM_BUDGET are the capture PRD's, and DELETE_UNDO_WINDOW
and SQLITE_READER_FLOOR the Data Foundation PRD's, used here by citation with their closure gates
left there.

**Build dependencies**
<!-- guidance: what the first build can proceed on, and what must stay gated. One row per unit of
     work: the contract already available to build against, and what must remain open (an ADR, an
     open question, hardware, a dogfood run) rather than being guessed at by the builder. -->

| Work | Available contract | What must remain open |
|---|---|---|
| Collection list, collection table, honesty marks, search, filters, sorts and selecting one row | R1.1–R1.6, R1.8, R2.1–R2.8, R2.11, R3.1–R3.6, R3.9, R6.4, R8.1, R8.3–R8.11; copy E1–E7, E12; the capture PRD's E1, E2 and E26, the device PRD's E22 and the Data Foundation PRD's E10, E14, E15, E34 and E35 | Stop: ADR-0003; Interim: OQ 1; Interim: OQ 7; Interim: OQ 12 |
| Item detail, editing and history | R4.1–R4.3, R4.5, R4.6, R4.9, R5.1–R5.3, R5.6, R5.7; copy E14, E17, E18; the Data Foundation PRD's E4, E8, E11, E26 and E31; the capture PRD's R5.6, R8.18 and R9.9 | Stop: ADR-0003 |
| The All items view and "Find similar" | R1.9, R1.10, R3.7, R3.8, R8.2; copy E9, E13 | Stop: ADR-0003; Interim: OQ 2; Interim: OQ 3; Interim: OQ 11 |
| Multi-item selection, bulk set or clear, undo; R6.2 and R4.7 landing together | R6.1, R6.2, R4.7; copy E8 | Stop: ADR-0003; Interim: OQ 4 |
| Bulk delete | R6.1, R6.3; the Data Foundation PRD's R6.2 and E33 | Stop: ADR-0003; Interim: OQ 1 |
| Undo of a delete | R1.7; copy E10 | Stop: ADR-0003 or a later undo-representation ADR; Stop: OQ 10 — the Data Foundation PRD's OQ 20 |
| Code change and column rename | R4.4, R4.8; copy E6, E11, E15, E16, E19; the Data Foundation PRD's R1.2 and the import PRD's R2.6 | Stop: ADR-0003 |
| Reordering from the collection | R2.9; the capture PRD's R6.8–R6.12, E33 and E34 | Stop: ADR-0003; Interim: the capture PRD's OQ 7 — its REORDER_SCOPE interim |
| Restore and distance from current | R5.4, R5.5; the Data Foundation PRD's R2.3f and R2.9 and the capture PRD's R5.6 and R8.18 | Stop: ADR-0003; Interim: OQ 11 |
| Swatch grid, column visibility and compare | R2.10, R5.8, R7.1, R7.2; the Data Foundation PRD's R1.1 | Stop: ADR-0003; Interim: OQ 6; Interim: OQ 11 |
| Where capture sits relative to the collection | no row here | Stop: the capture PRD's OQ 8 and ADR-0004 |

A builder builds against the **Available contract** column only. In **What must remain open**, a
Stop: item is a stop — that work waits for the ADR or question named, ADR-0003, ADR-0005 and ADR-0006
stopping every row — and work under an Interim:
item starts on the named question's interim rule and is re-checked when it closes. The Interim stated list and each row's cites complete these cells.

**P0 rows that defer to an open question with no interim rule:**
<!-- guidance: these two lists stay separate and are NEVER merged into one — merging them hides
     the difference the body text below states. Derive both from the Open questions table, not
     from memory. -->

A row listed here cannot be started, its question having no interim rule; a row under **Interim
stated** starts on its interim and is re-checked when its question closes.

- None — every open question below carries an interim rule.

**Interim stated:**

- R2.5, R3.1, R8.1, R8.2, R8.11, M1 — OQ 1 — F20, F33, F41, F42, F66, F99, F118, F119, F120.
- R8.1, R8.2 — OQ 2 — F34, F129.
- R3.7 — OQ 3 — F16.
- R6.2 — OQ 4 — F7.
- R3.3 — OQ 5 — F19.
- R7.1, R7.2 — OQ 6 — F20.
- R2.5, M2 — OQ 7 — F40 and F64.
- R1.7 — OQ 10 — the Data Foundation PRD's OQ 20 interim, which that PRD's fences set, and F57.
- R3.7, R5.4, R5.8 — OQ 11 — F64.
- R8.1, R8.2 — OQ 12 — F43, F103.
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

- Requirement rows are `R<section>.<n>` (`R7.4`), `<section>` being their section's number; copy
  states are `E<n>` and success metrics `M<n>`.
- A lead row with a table of its own letters those sub-rows (`R8.1a`) — R2.4, R4.2, R5.2, R8.1 and
  R8.10 do — and a sub-row carries its lead row's release, priority and status.
- IDs are assigned once and never renumbered. **Retired IDs:** none.
- The **Commit PR** column names the PR that landed the row.
- Owner decisions F1–F201 are in the fence file; a row names one for provenance only.

### Surfaces
<!-- guidance: every user-facing surface this product area touches, what it shows, and the copy
     states in that surface's flow. Every copy state appears under exactly one surface unless the
     state is deliberately shared, in which case this preamble says so and names the surfaces.
     Phase marks (see the Legend's phase rule) go on P1-only surfaces, states and actions. The copy
     strings themselves are never written here — they live in the copy companion. Conditional: a
     product area with no user-facing surface deletes this subsection rather than leaving it
     empty. -->

Every copy state appears under exactly one surface unless this preamble names it as shared. E4 and
E5 are shared by the collection surface and the All items view; E9 by those two and the item
detail; E12 by those three and the version history view; E6 by those four and the collection list; and E10
by the collection list, the collection surface, the All items view and the item detail.

| Surface | Shows | The copy states in this surface's flow |
|---|---|---|
| Collection list | Every collection in the file with its item count, and the entries to create a collection, to the All items view [phase: action-absent] and to the re-scans awaiting an answer | E1, E2, E6, E10 |
| Collection surface | One collection's item table with search, filters and sorts, one-row selection, "Select all" [phase: action-absent], "Set a field" [phase: action-absent], "Delete selected" [phase: action-absent], reordering [phase: action-absent], "Use as scan order" [phase: action-absent], "Undo change" [phase: action-absent], "Columns" [phase: action-absent], "Rename column" [phase: action-absent], "Grid" [phase: action-absent] and "Table" [phase: action-absent], and the entries to rename, delete and export the collection, beside every entry point and state the capture PRD places there | E3, E4, E5, E6, E7, E8, E9, E10, E11, E12, E19 |
| All items view [phase: surface-absent] | Every item of every collection, one row each naming its collection, with search, filters and sorts | E13, E4, E5, E6, E9, E10, E12 |
| Item detail | R4.2's lines, its editable fields and its actions, "Find similar" [phase: action-absent], "Change code" [phase: action-absent], "Undo change" [phase: action-absent] and the capture PRD's re-scan entry [phase: action-absent] among them | E14, E15, E16, E18, E6, E9, E10, E12 |
| Version history view | Every reading one item holds, in recorded or measured order, with R5.2's lines, "Use this reading" [phase: action-absent] and "Compare" [phase: action-absent] | E17, E6, E12 |
| Swatch grid [phase: surface-absent] | R7.1's and R7.2's swatches | none of its own |

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
(U5) through the P0 feature *collections and version history*.

| ID | Release | Pri | Requirement | Status | Commit PR |
|---|---|---|---|---|---|
| R1.1 | v1 | P0 | The collection list renders E2, naming every collection in the file with its item count, ordered by name as the capture PRD's R6.9 orders codes; a file holding no collection renders E1 instead. | aligned | |
| R1.2 | v1 | P0 | "New collection" runs the capture PRD's collection creation (its R1.1–R1.3, R1.5 and R1.10), and choosing a collection in the collection list shows it on the collection surface. | aligned | |
| R1.3 | v1 | P0 | "Rename collection" stores the new name as entered unless it is blank, when E7 renders, or another collection's name equals it under the import PRD's R2.3 rule, when the capture PRD's E1 renders; either way the old name stays, and E7's "Change the name" and that PRD's E1 action each return to editing it. Escape abandons a rename and keeps the old name, a new name equal to the collection's own under that rule, differing only in spacing or letter case, is accepted, and a stored name leaves the one it replaces nowhere in the file's bytes (inherited obligation for the Data Foundation doc). | aligned | |
| R1.4 | v1 | P0 | "Delete collection" renders E6's "interrupted" variant and deletes nothing while the collection holds an interrupted session, R8.3 governing it in flight; otherwise it opens the Data Foundation PRD's E14. | aligned | |
| R1.5 | v1 | P0 | E6's "Go to the session" opens that session where the capture PRD returns to it — the session itself while in flight (its R3.5), its E25 resume offer while interrupted — and "Cancel" closes E6 having changed nothing. | aligned | |
| R1.6 | v1 | P0 | Every delete this PRD offers opens its confirmation before anything is deleted, and in each such confirmation the Return key fires the confirmation's cancel action. No surface this PRD renders makes a delete its default action. | aligned | |
| R1.7 | v1 | P1 | After a confirmed delete E10 renders — its body for one item, its "swatches" variant for a selection, its "collection" variant for a collection — and its "Undo" restores what was deleted as the Data Foundation PRD's R6.2b states, until that PRD's DELETE_UNDO_WINDOW ends, R8.3 governing it in flight. OQ 10's interim holds — this row is not built until that question closes — and if it is still open at v1 release this row is deferred and v1 ships final deletes behind the counted confirmation and export first. | aligned | |
| R1.8 | v1 | P0 | "Export collection" on the collection surface and "Export swatch" in an item's detail open the Data Export PRD's E1 at collection and at single-item scope respectively (its R1.1a). | aligned | |
| R1.9 | v1 | P1 | "All items" renders E13 and lists every item of every collection in the file, one row per item, with R2.1's columns other than imported columns and then a Collection column, by collection name and then queue order until a view sort is applied. Items sharing a Swatch Code in different collections are separate rows, each naming its own collection and showing its own collection's working-set values. | aligned | |
| R1.10 | v1 | P1 | The All items view offers search — matching anywhere in a collection's name too, and imported values as R3.1 does — filters, view sorts, the lightness, chroma and hue sorts, "Find similar" and each item's detail as the collection surface does, each item carrying its own simulated mark in place of the device PRD's E22. It offers no reordering, no multi-item selection or bulk operation, and no collection rename, delete or export. | aligned | |

### 2. The collection table and colour honesty

Traces UJ 2, UJ 8; serves the vision use case *see the collection honestly* (U7).

| ID | Release | Pri | Requirement | Status | Commit PR |
|---|---|---|---|---|---|
| R2.1 | v1 | P0 | The collection surface renders E3 and lists the collection's items in a table, one row per item, in queue order until a view sort is applied. Its columns are the colour chip, Swatch Code, Swatch Name, row state, L*, C*, h°, Spread, Swatch Alternate Code, Swatch Alternate Name, and every imported column in its stored position. | aligned | |
| R2.2 | v1 | P0 | Row states and the collection's counts use the capture PRD's words and numbers, identical to its capture surface's (its R10.3), and every entry point and state that PRD places on the collection surface stays reachable and unhidden beside this PRD's (its R7.19). A collection holding no item renders the capture PRD's E2 empty variant in place of the table. | aligned | |
| R2.3 | v1 | P0 | A chip renders the item's current value in the collection's working set colour-managed to the display showing it, never the Data Foundation PRD's stored sRGB value, and outside that display's gamut renders the display's clipped colour with the cannot-show mark. An item with no current value, or whose working-set value is absent, shows an empty chip, never a stand-in colour, and a chip carries every R2.4 mark that applies to its item, each observable on its own. | aligned | |
| R2.4 | v1 | P0 | The honesty marks are the nine below, each labelled on the chip, in "Filters" and to VoiceOver as the copy file's Mark labels table words it; each shows wherever this PRD renders the value it describes, and no colour this PRD renders is presented as faithful where a mark says otherwise. | aligned | |

**The honesty marks.** One lettered row per mark; every cell is a rule ([R2.4](#2-the-collection-table-and-colour-honesty)).

| ID | Mark | Shown when |
|---|---|---|
| R2.4a | cannot-show | The display showing the window cannot render the item's current value, as R2.5 works it out. |
| R2.4b | outside-sRGB | The gamut-clipped flag of the current reading's working-set value is set (the Data Foundation PRD's R3.4). |
| R2.4c | simulated | The current reading's acquiring-device snapshot is of the simulated kind (the device PRD's R1.21 and R6.5). |
| R2.4d | non-spectral | The current reading is marked non-spectral (the Data Foundation PRD's R3.5; the capture PRD's R4.24). |
| R2.4e | samples-disagreed | The current reading's recorded agreement verdict is that its samples disagreed and their average was accepted (the capture PRD's R4.9–R4.11). |
| R2.4f | value-absent | The current reading's working-set value is absent because its condition is missing (the Data Foundation PRD's R3.3d and R3.5). |
| R2.4g | no-value | The item has no current value and no quarantined current reading: it was never scanned, or it is set aside for a cause other than unreadable. |
| R2.4h | unreadable | The item's current reading is quarantined (the Data Foundation PRD's R5.5b), the item being set aside with the cause unreadable (the capture PRD's R8.18). |
| R2.4i | re-scan-unanswered | A reading on the item carries the awaiting-answer mark, its successor waiting for the answer the Data Foundation PRD's R2.4 and R2.8 ask for. |

| ID | Release | Pri | Requirement | Status | Commit PR |
|---|---|---|---|---|---|
| R2.5 | v1 | P0 | The cannot-show mark is worked out, as OQ 7's interim states, against the gamut of the display showing the window — for a window across two displays, the one macOS reports it is on — and on a display reporting sRGB or none it is set exactly when R2.4b's flag is; no cannot-show mark is stored (inherited obligation for the Data Foundation doc). It is worked out again within BROWSE_RESPONSE_BUDGET at the 95th percentile whenever that gamut changes — the window moving to another display, or the display's profile or reference mode changing (R8.10a). | aligned | |
| R2.6 | v1 | P0 | The collection surface renders the device PRD's E22 while any item in the collection has a simulated current reading, and that PRD's E22 action applies R3.4's simulated filter, leaving exactly those items listed. A reading's simulated mark stays on it in the version history view whatever the item's current reading is (the device PRD's R6.5). | aligned | |
| R2.7 | v1 | P0 | The Spread column shows the current reading's recorded sample spread (the capture PRD's R4.9 and R4.11), and shows none for a reading of one sample. | aligned | |
| R2.8 | v1 | P0 | "Colour marks", offered on every surface E12's index row names, renders E12, showing each mark R2.4 and R5.2d list with its shape (R8.9), its chip label and what it means, grouped as E12 groups them, and what Spread shows. E12's "Close" closes it. | aligned | |
| R2.9 | v1 | P1 | With no session in flight on the collection and no search or filter active, the user drags a pending row to a new queue position while the table is in queue order, and fires "Use as scan order" while the table is view-sorted on Swatch Code, Swatch Name, an alternate or an imported column, both under the capture PRD's R6.9–R6.12 with its E33 and E34. While a session on the collection is in flight, or a search or filter is active, neither is offered, reordering in flight being the capture PRD's queue list's (its R6.8). | aligned | |
| R2.10 | v1 | P2 | "Columns" hides or shows any table column except the chip and Swatch Code, and the choice is kept per collection, with that collection, in the user's file (the Data Foundation PRD's R1.1), nothing about it being kept elsewhere (inherited obligation for the Data Foundation doc). | aligned | |
| R2.11 | v1 | P0 | Wherever this PRD displays them, L*, a*, b*, u*, v*, C* and h° show to one decimal place, Spread, ΔE2000 and X, Y and Z on 0–100 to two, sRGB as 0–255 integers and HSL as whole degrees and percents, while every sort and every comparison with a distance uses the stored value. | aligned | |

### 3. Search, filter and sort

Traces UJ 3, UJ 7; serves the vision use case *see the collection honestly* (U7) and the P0 feature
*collections and version history*.

| ID | Release | Pri | Requirement | Status | Commit PR |
|---|---|---|---|---|---|
| R3.1 | v1 | P0 | Typing in the search field lists only the items whose Swatch Code or Swatch Alternate Code starts with the text, or whose Swatch Name, Swatch Alternate Name or imported value contains it, both sides compared after the import PRD's R2.3 normalisation, and text that normalises to nothing is no search and narrows nothing. The table lists the result within BROWSE_RESPONSE_BUDGET, at the 95th percentile, of the last keystroke. | aligned | |
| R3.2 | v1 | P0 | Firing a column header view-sorts the table by that column and firing it again reverses the order; text compares as the capture PRD's R6.9 compares codes, row state in the order pending, captured, set aside, and Spread by its number, items with no value in the column following every item with one in either direction and ties keeping their existing order. The chip column does not sort, and a view sort never changes queue order (the capture PRD's R6.7). | aligned | |
| R3.3 | v1 | P0 | L*, C* and h° sort by the item's current value in its own collection's working set, and in an h° sort, in either direction, items whose C* is below NEUTRAL_CHROMA follow the chromatic items in ascending L*. Items whose value was worked out under an illuminant and observer other than the like pair — the collection's own in its table, and in the All items view the most common among listed items with a value, a mismatched non-spectral reading not counting, a tie going to the pair of the collection first in the collection list — and non-spectral readings at a reference other than their collection's (the Data Foundation PRD's R3.3e) follow the rest in the same order, E3 or E13 stating how many. | aligned | |
| R3.4 | v1 | P0 | "Filters" narrows the table by row state — pending, captured, set aside — and by any R2.4 mark under its Mark labels filter label, the row-state values forming one filter and the mark values another: values within a filter combine with or, and the two filters with and. While a search or a filter is active E3 and E13 render their "narrowed" variant, offering "Clear search" while a search is active and "Clear filters" while a filter is, each removing only its own narrowing. | aligned | |
| R3.5 | v1 | P0 | When the search lists no item, E4 renders whatever filters are active — its "all items" variant in the All items view — and when the filters hide every item the search lists, E5 renders. | aligned | |
| R3.6 | v1 | P0 | The search, filters and view sort of each collection and of the All items view persist while the file stays open, independently of one another, and none is kept once the file closes. | aligned | |
| R3.7 | v1 | P1 | "Find similar" on an item with a working-set value renders E9, listing nearest first, ties by Swatch Code, then collection-list order, each with its ΔE2000 — and its collection, from the All items view — the items of that item's collection, or of every collection from the All items view, whose working-set value is at or within FIND_SIMILAR_DISTANCE of it, each opening its item detail (R4.1). It compares only working-set values worked out under the chosen item's illuminant, observer and measurement condition, counting every other item, one with none included, as not compared, and computes no derived value set (the Data Foundation PRD's R3.3f), writes nothing and matches no named colour library. | aligned | |
| R3.8 | v1 | P1 | E9 renders its "none" variant when no other item is at or within that distance, and its "Close" closes it; "Find similar" is not offered on an item without a working-set value. | aligned | |
| R3.9 | v1 | P0 | Whenever an item changes — a metadata change, a capture save, a restore, a Flag, a re-scan answer, an undo or a re-read — or the display gamut changes under the cannot-show filter, the table lists it, unlists it or moves it as the active search, filters and view sort place it; R4.1, R8.3 and R8.5 keep those settings, not the listing. | aligned | |

### 4. Item detail and editing

Traces UJ 4; serves the vision use case *fix a bad scan without losing history* (U5) and journey
J3's closing step.

| ID | Release | Pri | Requirement | Status | Commit PR |
|---|---|---|---|---|---|
| R4.1 | v1 | P0 | Opening an item renders E14 with R4.2's lines, and closing it returns to E9, worked out again, when opened from E9 while E9's item remains, otherwise to the table with its search, filters, view sort, selection and scroll position as they were before it opened, R3.9 governing what is listed. If the item is deleted, or a re-read finds it gone, while its detail or history view is open, both close as closing does, with E10 over it where R1.7 is built. | aligned | |
| R4.2 | v1 | P0 | The item detail shows the lines below, the Data Foundation PRD's R7.6g among them, each labelled as the copy file's Detail lines table words it. | aligned | |

**What the item detail shows.** One lettered row per line; every cell is a rule ([R4.2](#4-item-detail-and-editing)).

| ID | Line | What it shows |
|---|---|---|
| R4.2a | Identity | Swatch Code, Swatch Name, both alternates, the collection's name, and every imported column with its value. |
| R4.2b | State | The row state, and for a set-aside item its cause as the capture PRD's copy file labels it — unreadable where its current reading is quarantined — and whether it has been deliberately left or is still to deal with (that PRD's R8.2, R8.5 and R8.18). |
| R4.2c | Current value | The chip with its marks and the current value in each of the six derived spaces, each with its illuminant, observer, measurement condition and derivation version (the Data Foundation PRD's R3.1 and R3.2); the value-absent mark where its working-set value is absent; or, for an item with none, that it has no current value. |
| R4.2d | The current reading | Its measurement time; its acquiring device's kind, model, serial and firmware version (the device PRD's R1.21); its samples kept, averaging basis, recorded spread and agreement verdict. |
| R4.2e | Marks | Every R2.4 mark that applies, and the non-spectral reference mismatch where the Data Foundation PRD's R3.3e sets one. |
| R4.2f | History | How many readings the item holds, and "Show history", which is not offered on an item holding none. |
| R4.2g | Damage | The Data Foundation PRD's E4 in its current or history variant, or its E31, wherever that PRD's R5.5a–c applies. |
| R4.2h | Re-scans awaiting an answer | The Data Foundation PRD's E11 for each reading on the item carrying the awaiting-answer mark. |

| ID | Release | Pri | Requirement | Status | Commit PR |
|---|---|---|---|---|---|
| R4.3 | v1 | P0 | Swatch Name, Swatch Alternate Code, Swatch Alternate Name and each imported column's value can be set or cleared in the item detail, and neither such an edit nor a code change under R4.4 changes a reading, the queue order or the row state. An edit lands in the file when the user presses Return or leaves the field, leaving the text it replaces nowhere in the file's bytes (the Data Foundation PRD's R2.3; inherited obligation for the Data Foundation doc), and Escape before then discards it. | aligned | |
| R4.4 | v1 | P1 | A Swatch Code entered through "Change code" that is blank renders E15, and one equal under the import PRD's R2.3 rule to another item's code in the collection renders E15's "duplicate" variant, E15's "Try another code" returning to editing it; either way, and when Escape abandons the change, the old code stays. One equal to the item's own code under that rule is stored as entered; any other renders E16, whose "Change the code" stores it on the same item, readings and history kept (inherited obligation for the Data Foundation doc), and "Keep this code" keeps the old one, R8.3 governing "Change code" in flight. | aligned | |
| R4.5 | v1 | P0 | "Delete swatch", in the item detail or on the one selected row (R6.4), opens the Data Foundation PRD's E8 for that item, naming its collection (inherited obligation for the Data Foundation and Capture Mode docs); while a session on the collection is in flight R8.3 governs it. | aligned | |
| R4.6 | v1 | P0 | The item detail offers the capture PRD's re-scan entry for the item (its R8.7) once that PRD's re-scan rows land, and the actions of the Data Foundation PRD's E4 and E11 shown there act as that PRD's R7.3k and R2.3c–e state, R8.3 governing that PRD's E4 restore in flight. | aligned | |
| R4.7 | v1 | P1 | "Undo change", offered only where its latest change's collection is shown, never in the All items view, reverses committed metadata changes — a field edit, a code change, a column or collection rename, a bulk set or clear — most recent first while the file stays open, until any other write the user commits, a column hidden or shown and "New collection" included, or a re-read ends that history, no write a capture session makes counting; there is no redo. Each undo re-checks the rule its change first passed (R1.3, R4.4, R4.8 and R8.3), rendering that rule's state, changing nothing and keeping its entry when refused, and a replaced value is held only in the running app's memory, never in the file or anywhere outside it (the Data Foundation PRD's R6.2d and R1.1; F53). | aligned | |
| R4.8 | v1 | P1 | "Rename column" on an imported column renders E11 for a blank name, E11's "duplicate" variant for a name equal under the import PRD's R2.3 rule to another of the collection's columns, a Swatch field's name or a header the copy file's Column headers table shows — the old name staying, E11's "Change the name" returning to editing it — and E19 for any other except one equal to the column's own name under that rule, stored as entered, Escape abandoning the rename. E19's "Rename the column" changes the name the file stores, so the table, an export and an outside reader show it and a later import matches on it (inherited obligation for the Data Foundation and Inventory Import docs), and "Keep this name" keeps the old one. | aligned | |
| R4.9 | v1 | P0 | On a captured item carrying no re-scan-unanswered mark (R2.4i), the item detail offers the capture PRD's Flag action (its §12 Labels), which renders E18, its "restore" variant once R5.5 lands; E18's "Set aside to scan again" sets the item aside as that PRD's R5.6 and R9.9 state, its reading kept as history and not marked never true, and its "Cancel" changes nothing. The action is not offered on an item that is not captured or that carries that mark, R8.3 governing it in flight. | aligned | |

### 5. Version history and corrections

Traces UJ 5; serves the vision use case *fix a bad scan without losing history* (U5).

| ID | Release | Pri | Requirement | Status | Commit PR |
|---|---|---|---|---|---|
| R5.1 | v1 | P0 | "Show history" in the item detail renders E17, listing every reading the item holds — current, superseded, never true, awaiting an answer or quarantined — each with R5.2's lines, without closing the item detail or changing what the table lists. | aligned | |
| R5.2 | v1 | P0 | Each reading in the version history view shows the lines below, each labelled as the copy file's History lines table words it. | aligned | |

**What each reading shows.** One lettered row per line; every cell is a rule ([R5.2](#5-version-history-and-corrections)).

| ID | Line | What it shows |
|---|---|---|
| R5.2a | Times | Its measurement time and its record time. |
| R5.2b | Device | Its acquiring device, with the simulated mark where that snapshot is of the simulated kind. |
| R5.2c | Reason | Its supersession reason (the Data Foundation PRD's R2.4), and whether it is the current reading. |
| R5.2d | Standing | The never-true mark, the awaiting-answer mark, and the unreadable mark where it is quarantined (the Data Foundation PRD's R2.5 and R5.5c). |
| R5.2e | Value | Its chip, rendered as R2.3 renders a chip, with the R2.4 marks that apply to its own value, its samples kept and its recorded spread. |

| ID | Release | Pri | Requirement | Status | Commit PR |
|---|---|---|---|---|---|
| R5.3 | v1 | P0 | E17's body — its "restore" variant once R5.5 lands — lists readings newest-recorded first, and its "measured" variant lists them by measurement time, oldest first, ties broken by record time so a restore follows its source, as an over-time view that leaves out never-true readings and states how many, keeps readings superseded by a re-measurement as ordinary points, and shows readings awaiting an answer with their mark (the Data Foundation PRD's R2.5). "Recorded order" and "Measured order" switch between the two. | aligned | |
| R5.4 | v1 | P1 | Each earlier reading of an item with a current value shows its ΔE2000 from it when both were worked out under the same illuminant, observer and measurement condition, and otherwise shows the copy file's not-compared distance line in its place, never a distance computed across the two. | aligned | |
| R5.5 | v1 | P1 | "Use this reading" on a readable earlier reading, a never-true one included and keeping its mark, makes a new current value equal to it as the Data Foundation PRD's R2.3f states, and is offered on an item with a current value, one whose current reading is quarantined (that PRD's R5.5b), or one set aside by a Flag (the capture PRD's R5.6), the flagged reading included, either of the last two becoming captured again (inherited obligation for the Capture Mode and Data Foundation docs). It is never offered on a quarantined reading, nor on an item with no current value for any other reason, R8.3 governing it in flight. | aligned | |
| R5.6 | v1 | P0 | No surface this PRD renders offers an action that removes one reading from an item's history (the Data Foundation PRD's R6.1). | aligned | |
| R5.7 | v1 | P0 | While the file holds any reading awaiting an answer, E2 offers "Answer re-scans", which opens the Data Foundation PRD's E26 (its R2.8); while it holds none, the action is not offered. | aligned | |
| R5.8 | v1 | P2 | "Compare" on two readings selected in E17 shows their two chips side by side with their marks, and their ΔE2000 under R5.4's rule. | aligned | |

### 6. Selection and bulk operations

Traces UJ 4, UJ 6; serves the P0 feature *collections and version history*.

| ID | Release | Pri | Requirement | Status | Commit PR |
|---|---|---|---|---|---|
| R6.1 | v1 | P1 | The table also selects several items, by range and by adding or removing single rows, shows how many are selected, and "Select all" selects every item the current search and filters list, whether or not its row is on screen. | aligned | |
| R6.2 | v1 | P1 | "Set a field" with two or more items selected sets one editable field to the value entered — an empty value clearing it — on exactly the selected items, the selected count staying in view while the field and value are chosen, and leaving the values it replaces nowhere in the file's bytes (inherited obligation for the Data Foundation doc). Return applies the value — above BULK_CONFIRM_COUNT selected items first rendering E8, its "clear" variant for a clear, nothing changing until "Apply to ⟨n⟩ swatches" — and Escape, or E8's "Cancel", which Return fires there, changes nothing. | aligned | |
| R6.3 | v1 | P1 | "Delete selected" with two or more items selected opens the Data Foundation PRD's E33, which deletes every selected item or none, R8.3 governing it in flight (inherited obligation for the Data Foundation doc). | aligned | |
| R6.4 | v1 | P0 | The user selects one row of the table, which deselects any other, and a search or filter change or any R3.9 change that stops listing a selected item deselects it. | aligned | |

### 7. The swatch grid

Traces UJ 10; serves the vision use case *see the collection honestly* (U7) through the P2 feature
*gamut-aware swatch grid*.

| ID | Release | Pri | Requirement | Status | Commit PR |
|---|---|---|---|---|---|
| R7.1 | v1 | P2 | "Grid" shows the collection surface's items as swatches no smaller than MIN_SWATCH_SIZE, each labelled with its Swatch Code and carrying every R2.4 mark on the swatch itself, and "Table" returns to the table. Switching either way keeps the search, filters, view sort, selection and the item in view — the selected item if on screen, else the first on screen — and the "Grid" or "Table" choice, kept per collection, lasts while the file stays open and is written nowhere (R8.6). | needs-discussion | |
| R7.2 | v1 | P2 | The user sets each collection's swatch size, its default and range the build's choice, never below MIN_SWATCH_SIZE, a size that lasts while the file stays open and is written nowhere (R8.6), and an item with no current value shows R2.3's empty chip as its swatch. | pre-alignment | |

### 8. Operating envelope and quality attributes
<!-- guidance: mandatory, and always the LAST numbered requirement section. Its heading takes the
     number after the project's own requirement sections — assigned at scaffold time, before any
     ID exists, and never changed afterwards — so its rows are `R<that number>.<n>` and are
     ordinary requirement rows in every respect: same columns, same priorities, same statuses,
     same acceptance cases in the journeys companion, same named constants in the Legend's
     constants table. This is the class of requirement a builder most often has to go back to
     product for, so the format scaffolds it rather than leaving it to be remembered. -->

Traces UJ 9; serves the vision use case *see the collection honestly* (U7) and AGENTS.md §4's
local-first, offline, user-owned commitments.

The rows here hold the operating envelope — latency and throughput, data volumes, concurrency,
offline and partial failure, platform compatibility, durability, and security and privacy — each
class answered by a row, or by a row saying why this area has none; a class left out is a latent
decision.

| ID | Release | Pri | Requirement | Status | Commit PR |
|---|---|---|---|---|---|
| R8.1 | v1 | P0 | Each input below meets its budget at ROWS_CEILING items in the collection with up to IMPORTED_COLUMNS_CEILING imported columns, in a file of up to FILE_ITEMS_CEILING items, timed from the input, or a capture save from when it lands, to the first frame showing its result, BROWSE_RESPONSE_BUDGET at the 95th percentile, on the machine OQ 1's interim names. Above any ceiling every row still works and nothing is refused or truncated, but no budget is promised. | aligned | |

**The budgets.** One lettered row per class of input; every cell is a rule ([R8.1](#8-operating-envelope-and-quality-attributes)).

| ID | Input | Result shown within |
|---|---|---|
| R8.1a | A search keystroke, a filter change or a view sort, in the table or the grid | BROWSE_RESPONSE_BUDGET |
| R8.1b | Selecting one row, a range or every listed item, or toggling one row; opening or closing an item detail, or a version history view of up to HISTORY_READINGS_CEILING readings; switching between "Table" and "Grid"; changing the swatch size | BROWSE_RESPONSE_BUDGET |
| R8.1c | A single-item write anywhere in the app — a reading capture saves, an edit, a rename, a restore, a Flag, a re-scan answer, a single delete, a drag, an undo of one, or a column hidden or shown — reaching the table, grid, item detail or version history view while shown, or its next showing within that showing's budget | BROWSE_RESPONSE_BUDGET |
| R8.1d | Choosing a collection | OPEN_COLLECTION_BUDGET to its first rows — the first frame showing a screenful of rows with their chips and marks — every other budget holding from that frame, nothing left loading that a search waits on |
| R8.1e | Paging through the table or grid | no more than DROPPED_FRAME_SHARE of frames missed |
| R8.1f | A bulk set or clear, a "Use as scan order", or an undo of a set or clear | progress shown unless the write is done within BROWSE_RESPONSE_BUDGET, the surface meeting R8.1a–b throughout, the Data Foundation PRD's R1.11 applying, the write done within BULK_WRITE_BUDGET |
| R8.1g | A "Delete selected" or "Delete collection", or E10's "Undo" of either | as R8.1f, the write done within DELETE_WRITE_BUDGET |

| ID | Release | Pri | Requirement | Status | Commit PR |
|---|---|---|---|---|---|
| R8.2 | v1 | P1 | The All items view's R8.1a–c inputs and firing "Find similar" to E9 meet BROWSE_RESPONSE_BUDGET at FILE_ITEMS_CEILING items, every collection holding IMPORTED_COLUMNS_CEILING imported columns, the view showing its first rows within OPEN_COLLECTION_BUDGET as R8.1d does, and an item's version history view lists every reading it holds, however many. | aligned | |
| R8.3 | v1 | P0 | While a session on a collection is in flight, "Delete swatch", "Change code", an "Undo change" reversing a code change, E10's "Undo", the capture PRD's Flag action, "Use this reading" and the Data Foundation PRD's E4 restore action on that collection, and "Set a field", "Delete selected", "Delete collection", "Use as scan order", E10's "Undo" of a selection or collection delete and an "Undo change" of a bulk set or clear on any collection, render E6 — its "elsewhere" variant for another collection's session and its "full" variant once every P1 action that variant names lands, never R1.4's "interrupted" — and change nothing (inherited obligation for the Capture Mode doc), while every other metadata edit and that PRD's E11 answers stay available and move no queue position, row state, remembered row or current row. A reading saved by any session shows in the table without resetting its search, filters, view sort, selection or scroll position, R3.9 governing what is listed. | aligned | |
| R8.4 | v1 | P0 | With the file open read-only, the collection list, table, item detail and version history view show every item, current value and mark of a format this build reads, a newer-format file (the Data Foundation PRD's E1) showing that PRD's R5.3 minimum with the rest left to its OQ 18, and every action this PRD offers that would write the file shows disabled, E14 rendering its "read-only" variant. An item whose readings cannot be read never stops the rest of its collection being browsed, searched, sorted or edited. | aligned | |
| R8.5 | v1 | P0 | After the Data Foundation PRD's re-read (its R1.5 and R7.3c) the table, item detail and version history view show the file as re-read, keeping the search, filters and view sort and deselecting any item the file no longer holds. Noticing a change made outside the app is that PRD's OQ 14, and no row here does it. | aligned | |
| R8.6 | v1 | P0 | Every row here works with no network connection, no action this PRD adds makes a network request, nothing it reads or writes leaves the machine (the Data Foundation PRD's R1.4), and no event about its actions carries typed text, a code, a name or a value (inherited obligation for the Telemetry doc). Search text, filters, view sorts, the grid choices and the undo history are never written to the file or anywhere outside it, preferences and saved window state included, column visibility (R2.10) being the one view choice the file keeps, and the file gets no system-kept versions and no collection content goes to system search or Handoff. | needs-discussion | |
| R8.7 | v1 | P0 | No row here sets a minimum macOS version or a display requirement: ADR-0006 selects the floor, and a row the selected floor cannot deliver goes back to the owner rather than being dropped. R2.5's mark is worked out on every display the window can be on, built-in or external. | aligned | |
| R8.8 | v1 | P0 | Each committed edit, rename, reorder, restore, column-visibility change and bulk operation lands in the file whole or not at all before any surface shows it done, so a crash or power loss leaves it complete or absent, and text it removes is nowhere in the file's bytes from the moment it lands, open or after a crash, unless a read begun before its wipe defers it (the Data Foundation PRD's R1.10 and R6.2a; inherited obligation for the Data Foundation doc). A write refused because the volume is full renders that PRD's E15, because another copy of the app holds the file its E10, because permission to the file was lost its E34, and because the volume is gone the capture PRD's E26, each changing nothing. | pre-alignment | |
| R8.9 | v1 | P0 | Every mark the copy file's Mark labels table lists — R2.4's nine and R5.2d's never-true and awaiting-answer — has a shape, distinct from every other mark's, that does not depend on seeing colour, and the VoiceOver name that table gives it, read with the row's Swatch Code, Swatch Name and row state. Every action this PRD offers, the drag reorder and choosing two readings for "Compare" included, can be reached and fired from the keyboard by a route the build chooses. | aligned | |
| R8.10 | v1 | P0 | A test can declare the inputs and read the results below without matching wording, and can read the file with the app closed at SQLITE_READER_FLOOR (the Data Foundation PRD's R7.1). R8.10b and R8.10c exist only in test builds, and no build opens a listening socket or cross-process service for one. | aligned | |

**The verification seam.** One lettered row per kind of input or readback; every cell is a rule ([R8.10](#8-operating-envelope-and-quality-attributes)).

| ID | Seam | What a test can declare or read |
|---|---|---|
| R8.10a | Declared inputs | The file's contents (the Data Foundation PRD's R7.2) and the app's permission to it, a collection's session state (the capture PRD's R11.6), any gamut a display reports and which display macOS reports the window on, the clock, the user's language, and the network's reachability and the telemetry and update-check settings (the device PRD's R6.12, R2.13 and R2.14); and in a test build a write the Data Foundation PRD's R1.11 names held running until released, and a read held open on the file from outside the app |
| R8.10b | Surfaces | For the collection list, table, All items view, item detail, version history view, E9 and the grid: rows or swatches in order with their columns and collection, the first on screen and how many show, the swatch size, the selection and its count, the search text, the active filters, the actions offered or shown disabled — for "Set a field", its fields — each R4.2 and R5.2 line's content, a set-aside cause by the capture PRD's Cause column, whether a write's progress shows, and the copy state and variant up, with each token's value |
| R8.10c | Chips | Each chip's, E12's and each history line's marks by identifier, each mark's symbol apart from its tint, and its accessible description, whether it is empty or filled, and for a filled chip the working-set value it renders with its illuminant, observer, condition and derivation version, the display-space triplet it sends, and whether that triplet was clipped |
| R8.10d | The file's bytes | The file's bytes, read with the app closed, while it is open, and after a crash before reopening |
| R8.10e | The app's own storage | Everything the app keeps outside the file wherever the build puts it — its unified-log entries, crash reports, system-kept file versions and system search or Handoff donations included — never the file's bytes |
| R8.10f | Timing | The time from each R8.1 input or R2.5 gamut change to the first frame showing its result, with the build configuration and machine |

| ID | Release | Pri | Requirement | Status | Commit PR |
|---|---|---|---|---|---|
| R8.11 | v1 | P0 | No write or refresh this PRD makes while a session is in flight delays that session's trigger acknowledgement or row confirmation past the capture PRD's TRIGGER_ACK_WINDOW or ROW_CONFIRM_BUDGET (its R4.8 and R4.13), and the edits R8.3 leaves available stay available. | aligned | |

## Inherited obligations
<!-- guidance: outbound first — every behaviour this PRD's rows require another document's product
     to implement. The inbound table is conditional on siblings existing: when they do, it carries
     the same three columns for what those documents require of this one.
     Standing check, run on every amendment: the obligation summary on each side of a seam says
     the same thing; a change on one side lands on the other IN THE SAME PR, with a dated
     clarification on the fence it touches. The most common missing thing in an amendment is the
     sibling half of a mirror. -->

Every cross-document cite in this PRD and its companions names the owning document in its visible
label — the device PRD's `R6.27`, never a bare `R6.27` — because ID families, fence numbers
included, repeat across PRDs; an ID written bare is this document's own.

**Outbound** — what this PRD requires of other documents:

| Target PRD | Obligation | Rows |
|---|---|---|
| Data Foundation | A selection-scale delete confirmation beside its E8 and E14 — its E33 — stating the selected item count and their current and earlier readings, with cancel as E8 offers it and an export first that opens, and says it saves, the whole collection holding the selection, tested by its R7.6o; made in this change | R6.3 |
| Capture Mode, Data Foundation | Restoring a readable earlier reading of an item set aside by a Flag — the flagged reading included — makes it captured again, and the operator's Flag is named beside damage as a way an item's current value is removed; a restore carries its source's device snapshot, agreement verdict and spread; made in this change in the capture PRD's R5.6 and the Data Foundation PRD's R2.9 and R2.3f | R5.5 |
| Data Foundation, Inventory Import | A renamed imported column keeps its position and values under the new name the file stores, and a later import matches that name, a source header equal to the old name arriving as a new column; made in this change in the Data Foundation PRD's R1.2 and the import PRD's R2.6 | R4.8 |
| Data Foundation | Made in this change, as its F52–F58 record: its E8, E11, E15, E26, E34 and E35 wording, E34's actions as its R7.3j states; its R1.2's identity under a new code and column visibility, R2.3's and R6.2a's byte rule and its deferred wipe, R3.4's own flag test and R1.5's help docs; its R1.11 one-writer rule; its OQ 20 sizing an undoable ceiling delete; and the ADR-0003 and ADR-0007 inputs those fences list | R1.3, R1.7, R2.4b, R2.5, R2.10, R4.2h, R4.3, R4.4, R4.5, R5.7, R6.2, R8.1f, R8.2, R8.8, R8.11 |
| Capture Mode | Made in this change: its R5.6 and R8.18 refuse a restore while a session is in flight; its writes and session starts follow the Data Foundation PRD's R1.11; its R8.18 counts an unreadable row in collection tallies only, follows the Data Foundation PRD's quarantine mark, and on a read-only file shows what that PRD reports; its R3.7 resumes at the first pending row when the remembered row was deleted; its R11.15g points at E3 and Surfaces; its copy file's Set-aside cause labels table words each cause R4.2b shows; its R8.14 and R9.9's Flag entry at P0; and its FIND_BUDGET closes at or below BROWSE_RESPONSE_BUDGET (its OQ 13) | R2.4c, R2.4d, R2.6, R2.7, R4.2b, R4.5, R4.9, R8.1f, R8.3 |
| Inventory Import | Made in this change: its R3.2 refuses a commit while any session is in flight and follows the Data Foundation PRD's R1.11 | R8.1f |
| Data Export | Made in this change: its R1.1 reads one snapshot, the file as it stood when the export started | R1.8, R8.8 |
| Telemetry, not yet written | No event about a Collection Mode action carries typed text, a code, a name or a value | R8.6 |

**Inbound** — what other documents require of this one:
<!-- conditional: include only when sibling PRDs exist; delete the heading and table otherwise. -->

| Target PRD | Obligation | Rows |
|---|---|---|
| Product README (§6, Coverage) | §6's scope, the UI half of U5 and the swatch-grid half of U7 | R1.1, R2.1, R3.1–R3.9, R4.1–R4.9, R5.1–R5.8, R6.1–R6.4, R7.1, R7.2 |
| Capture Mode | Its R1.8: rename and delete a collection under the one naming rule, a delete guarded against an active or interrupted session, warned with export first and never the default; its journeys' UJ1.2-a–c, carried in UJ 1 | the capture PRD's R1.2, R1.8 → R1.3, R1.4, R1.5, R1.6, R8.3 |
| Capture Mode | Its R4.11: show the spread recorded on a reading | the capture PRD's R4.11 → R2.4e, R2.7, R4.2d, R5.2e |
| Capture Mode | Its R9.9: a Flag from the collection on a captured item, at P0, doing what its R5.6 Flag does | the capture PRD's R5.6, R9.9 → R4.9, R8.3 |
| Capture Mode | Its obligations line: restores of set-aside rows, and bulk writes, refused in flight | the capture PRD's R4.13, R5.6, R8.18 → R8.3 |
| Capture Mode | Its R8.18: an item whose current reading is quarantined is set aside with the cause unreadable and counted as set aside | the capture PRD's R8.18 → R2.4g, R2.4h, R4.2b |
| Capture Mode | Its R8.14: the simulated-readings banner, the recorded spread and the non-spectral mark surfaced where the collection is browsed | the capture PRD's R8.14 → R2.4c, R2.4d, R2.6, R2.7 |
| Capture Mode | Its R6.8: the sort and drag controls the operator orders the queue with, before a session, from the collection | the capture PRD's R6.8 → R2.9 |
| Capture Mode | Its R4.8 and R4.13: an in-flight session's trigger acknowledgement and row confirmation within TRIGGER_ACK_WINDOW and ROW_CONFIRM_BUDGET | the capture PRD's R4.8, R4.13 → R8.11 |
| Capture Mode, through Data Foundation | Its R6.9: codes compare the same way wherever they are ordered | the capture PRD's R6.9 → R1.1, R3.2 |
| Capture Mode, Data Foundation | Marking a colour outside what a display can show — this PRD the showing, the Data Foundation PRD what the reading keeps | the capture PRD's obligations line → R2.4a, R2.5 |
| Capture Mode | The shared collection surface: identical row states, vocabulary and counts, its states never hiding the surface's entry points (its F43), its review, summary and resume offers there, view sorts leaving queue order alone, and re-scan from an item | the capture PRD's R6.7, R7.8, R7.15, R7.19, R8.7, R8.16, R10.3 → R2.1, R2.2, R3.2, R4.6 |
| Device Management | Its R6.5 and E22, and its journeys' UJ1.2-c: the banner while any item's current reading is simulated, none in the All items view, the per-item mark following the current reading, the flag kept in history | the device PRD's R6.5 → R1.10, R2.4c, R2.6, R5.2b |
| Data Foundation | Its R2.5: the three over-time display rules | the Data Foundation PRD's R2.5 → R5.2d, R5.3 |
| Data Foundation | Its R3.4: the gamut-clipped flag belongs to the reading, and any display-relative check is this PRD's | the Data Foundation PRD's R3.4 → R2.4a, R2.4b, R2.5 |
| Data Foundation | Its R3.5: the non-spectral mark and the absent-value mark | the Data Foundation PRD's R3.5 → R2.4d, R2.4f, R4.2e |
| Data Foundation | Its R6.1: no single reading is deletable out of history, and delete is never a default action | the Data Foundation PRD's R6.1 → R1.6, R5.6 |
| Data Foundation | Its obligations line: history reachable from an item without costing the primary view | the Data Foundation PRD's R2.3 → R4.1, R5.1 |
| Data Foundation | Its R6.2, R6.3, E8, E14 and E33: the counted delete confirmations, export first, and the P1 undo | the Data Foundation PRD's R6.2, R6.3 → R1.4, R1.7, R4.5, R6.3 |
| Data Foundation | Its R2.3, R2.4, R2.8, E11 and E26: restore, the correction question and the unanswered set reachable from the app and from each item | the Data Foundation PRD's R2.3, R2.4, R2.8 → R4.2h, R4.6, R5.5, R5.7 |
| Data Foundation | Its R5.5, E4 and E31: the damage states, the rest of the collection still usable, and a restore when the current reading is quarantined | the Data Foundation PRD's R5.5 → R2.4h, R4.2g, R5.5, R8.4 |
| Data Foundation | Its R2.3, R6.2a and R6.2d: metadata edited and cleared in place and content deleted, no prior text recoverable from the file's bytes | the Data Foundation PRD's R2.3, R6.2 → R1.4, R4.3, R4.5, R4.7, R6.2, R6.3 |
| Data Foundation | Its R7.6g: the item detail's data lines | the Data Foundation PRD's R7.6g → R4.2 |
| Data Foundation | Its R5.3: the read-only browsing floor | the Data Foundation PRD's R5.3 → R8.4 |
| Data Foundation | Its R1.5, R1.4 and E9: re-reading the file, and no outbound traffic | the Data Foundation PRD's R1.4, R1.5 → R8.5, R8.6 |
| Data Foundation | Its R1.1: nothing about a collection is kept outside the file | the Data Foundation PRD's R1.1 → R2.10 |
| Data Foundation | Its R1.11: the one-writer rule | the Data Foundation PRD's R1.11 → R8.1c, R8.1f |
| Inventory Import | Its R2.6 and obligations line: renaming imported columns, a rename changing the name the file stores and a later import matches | the import PRD's R2.6 → R4.8 |
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
| E1 | No collections yet | Collection list | R1.1 | aligned |
| E2 | Collections | Collection list | R1.1, R5.7 | aligned |
| E3 | Collection shown | Collection surface | R2.1, R3.3, R3.4 | aligned |
| E4 | No search matches | Collection surface; All items view | R3.5 | aligned |
| E5 | Nothing passes the filters | Collection surface; All items view | R3.5 | aligned |
| E6 | Waiting for a session to end | Collection list; collection surface; All items view; item detail; version history view | R1.4, R1.5, R4.9, R8.3 | aligned |
| E7 | Collection name needed | Collection surface | R1.3 | aligned |
| E8 | Change a field on many swatches | Collection surface | R6.2 | aligned |
| E9 | Colours close to a swatch | Collection surface; All items view; item detail | R3.7, R3.8 | aligned |
| E10 | Deleted, undo available | Collection list; collection surface; All items view; item detail | R1.7 | aligned |
| E11 | Column name not accepted | Collection surface | R4.8 | aligned |
| E12 | What the marks mean | Collection surface; All items view; item detail; version history view | R2.8 | aligned |
| E13 | All items shown | All items view | R1.9, R3.3, R3.4 | aligned |
| E14 | Swatch detail | Item detail | R4.1, R8.4 | aligned |
| E15 | Code not accepted | Item detail | R4.4 | aligned |
| E16 | Change a swatch's code | Item detail | R4.4 | aligned |
| E17 | History | Version history view | R5.1, R5.3 | aligned |
| E18 | Set a swatch aside to scan again | Item detail | R4.9 | aligned |
| E19 | Rename a column | Collection surface | R4.8 | aligned |

**Labels rule.** `docs/product/collection-mode/prd-collection-mode-copy.md` is the one place a user-facing label is
written; every action a row names is quoted from it. A sibling's action that a row here relies on is
named by its document and state, never quoted here.

**Placeholders rule.** The tokens in use: ⟨collection⟩ a collection's name — the named item's
where a state names an item; ⟨code⟩ the Swatch Code of the item the state is about — the item opened (E14,
E16, E17), the item "Find similar" started from (E9), the item deleted (E10), the item whose code was
refused (E15) or the item being set aside (E18); ⟨name⟩ that item's Swatch Name; ⟨text⟩ what the
user typed; ⟨column⟩ a column's name; ⟨value⟩ a value being set; ⟨n⟩ a count whose basis each state
sets — the collections in the file (E2), the collection's swatches (E3) or every collection's
(E13), those the filters hide among what the search lists or, with no search, among the collection's
(E5), the selected swatches (E8), the deleted swatches (E10), the swatches "Find similar" lists (E9),
and the item's readings (E17); ⟨shown⟩ the swatches a search or filter lists; ⟨collections⟩ the
collections in the file; ⟨excluded⟩ the swatches "Find similar" did not compare; ⟨unlike⟩ the listed
swatches an L*, C* or h° sort places after the rest (R3.3), zero under any other sort; ⟨left⟩ the
readings an over-time view leaves out; ⟨scope⟩ the collection's name, or all your collections; and
⟨distance⟩ FIND_SIMILAR_DISTANCE's current number, to two places. A sentence whose count would be zero is left out
rather than showing 0, with the punctuation that joined it; a sentence carrying a count agrees with
it — 1 swatch, ⟨n⟩ swatches; and a named constant renders as the number it currently holds.

**Variant enumeration rule.** A copy state with variants names, in its copy-companion entry, the
requirement row that enumerates its variant set, and that row lists the variant names verbatim.

## Success metrics
<!-- guidance: precise enough that two people computing the same metric from the same data get the
     same number. The Method column names the OBSERVABLE a test or a dogfood runbook reads — not
     "measure adoption" but the event, surface or file it is read from; evidence artifacts are
     named files. Not conditional. -->

| ID | Metric | Definition (start event, end event, statistic, population) | Candidate target | Method | Status |
|---|---|---|---|---|---|
| M1 | Browse response time | Start: an R8.1a–c input or R2.5 gamut change, a capture save timed from when it lands; end: the first frame showing its result; statistic: the nearest-rank 95th percentile per input kind; population: each kind as UJ9.5-a delivers it, in a Release build on OQ 1's interim Mac, warm. | ≤ BROWSE_RESPONSE_BUDGET | R8.10f's timing readback; UJ9.5-a is its worked oracle. | aligned |
| M2 | Honesty-mark agreement | Start: a seeded file whose every item's R2.4 conditions are declared, and a declared display gamut; end: the marks R8.10 lists for each chip; statistic: the share of item-and-mark pairs where the listed mark agrees with the declared condition; population: every item of the harness's seeded file — 15, Gouache Set's two included — under each of sRGB, Display P3 and no reported gamut, every PR. | 100% | R8.10c's listing of each chip's marks against R2.4's conditions; UJ9.8-a is its worked oracle. | aligned |
| M3 | Readings lost to a Collection Mode action | Start: the file before a field edit, a code change, a column rename, a bulk set or clear, a reorder, a restore, a re-scan answer or a Flag; end: the file read with the app closed at SQLITE_READER_FLOOR after it; statistic: the count of readings present before that are not readable after; population: every such action the journeys run, every PR. | 0 | R8.10's read of the file at SQLITE_READER_FLOOR; UJ9.8-b is its worked oracle. | aligned |
| M4 | Re-scans left unanswered | Start: the end of a dogfood session on a real collection; end: the same file a week later; statistic: the count of re-scans still awaiting an answer; population: each dogfood session ending with at least one re-scan awaiting an answer, that count recorded at the start; a session with none reads not measured, never 0. | 0 | The Data Foundation PRD's E26 count, read through R5.7's "Answer re-scans" a week on. | aligned |

Beside M4's dogfood session, one check that is not a metric: the owner reads E12 once, on a Display P3
display with an outside-sRGB swatch listed, and says what each mark tells them they can and cannot
trust (F82, F134).

## Open questions
<!-- guidance: every question this PRD cannot yet answer that blocks a row. Closer names who or
     what closes it (the evidence, the run, the owner call), Feeds names the row IDs it blocks; an
     OQ with no row it feeds is scope creep, not a blocker. Reserved numbers stay reserved: a
     withdrawn OQ keeps its number rather than freeing it. Not conditional. -->

| # | Question | Decision so far | Interim rule | Closer | Feeds (row IDs) | Status |
|---|---|---|---|---|---|---|
| 1 | The constants table's OQ 1 budgets and ceiling: how fast must browsing answer at scale, and on which machines? | F20, F42, F66, F99 and F118's candidates. | Each candidate, on an M1 MacBook Air with 8 GB, internal disk, the first open cold as the journeys' timing workload defines it and browsing warm; read every release there, per-PR timing only a tripwire; R8.11 against the engineering plan's ROW_CONFIRM_BUDGET while the capture PRD's OQ 5 is open. | Engineering times Release builds at ROWS_CEILING on that Mac and one current Mac; the owner ratifies, and estimates HISTORY_READINGS_CEILING. | R2.5, R3.1, R8.1, R8.2, R8.11, M1 | pre-alignment |
| 2 | FILE_ITEMS_CEILING: how many items must the file-wide views hold up under? | F34's candidate. | 100,000 items across the whole file. | The owner's estimate of collections per file, checked by UJ9.5-b. | R8.1, R8.2 | pre-alignment |
| 3 | FIND_SIMILAR_DISTANCE | F16's candidate. | ΔE2000 3.0, at or within. | A dogfood "Find similar" pass over a real collection of at least 200 items, recording how many items each query lists; the owner. | R3.7 | pre-alignment |
| 4 | BULK_CONFIRM_COUNT | F7's candidate. | Confirm a bulk set or clear across more than 10 items. | Dogfood bulk edits, then the owner. | R6.2 | pre-alignment |
| 5 | NEUTRAL_CHROMA: below what chroma is an item treated as having no meaningful hue? | F19's candidate; no source measured. | C* 3.0. | A dogfood hue sort of a collection holding a grey series; the owner. | R3.3 | pre-alignment |
| 6 | MIN_SWATCH_SIZE | F20's candidate. | 24 pt. | The owner at the P2 build, on the owner's display at working distance. | R7.1, R7.2 | pre-alignment |
| 7 | What decides that the display cannot show a colour: which gamut, which rendering intent, and a display reporting none? | The Data Foundation PRD's R3.4 states the stored flag's own test (F127, F146); the display check is this question's. | Adapt the current value to the display's white by Bradford and test it against the display's reported gamut under relative colorimetric at zero tolerance, treating a display reporting none as sRGB. | An engineering spike on one sRGB and one Display P3 display with the harness's seeded file; the owner. | R2.5, M2 | pre-alignment |
| 8 | Does renaming an imported column change the name the file stores? | Answered by F9 (R4.8): yes. | | Closed — the owner (F9); the results file carries the answer. | R4.8 | aligned |
| 9 | How is deleting a selection confirmed? | Answered by F7 (R6.3): by the Data Foundation PRD's E33. | | Closed — the owner (F7); the results file carries the answer. | R6.3 | aligned |
| 10 | When may undo of a delete be built? | The Data Foundation PRD's OQ 20 gates its R6.3; if it keeps pending content in the file, the crash route and the deleted state are restated. | R1.7 is not built and E10 does not appear. | The Data Foundation PRD's OQ 20. | R1.7 | pre-alignment |
| 11 | Which published ΔE2000 values do the "Find similar" and distance cases use? | F64's interim: CIEDE2000 test pairs 1–5 from Sharma, Wu and Dalal (2005). | Use the pairs as the journeys quote them. | Engineering checks every quoted pair against the published table before the first such case runs. | R3.7, R5.4, R5.8 | pre-alignment |
| 12 | IMPORTED_COLUMNS_CEILING: how wide may a collection's imported data be while the budgets hold? | F43's candidate. | 20 columns of 200 characters. | The widest inventory the owner dogfoods; the owner. | R8.1, R8.2 | pre-alignment |

Every open question carries an interim rule, or its P0 rows are in the Legend's no-interim list;
there is no third option.

Results file: `docs/product/collection-mode/prd-collection-mode-oq-results.md`, one `## OQ <id>` section per
answer; an OQ's Status changes only when its section exists, and every confirm-on-verification
marker carries its OQ id.

---

Upstream: this PRD traces to the product vision at `docs/product/vision.md` and the current
product strategy at `STRATEGY.md`. Where it narrows or overrides either, the
build contract says so.
<!-- guidance: docs/product/vision.md is the durable WHY this PRD serves; STRATEGY.md
     is the active bets and roadmap it sits inside. Both are project-supplied paths, substituted at
     scaffold. A divergence from either is stated in the build contract, never left implicit. -->
