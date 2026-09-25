<!-- guidance: this is the decisions companion to `PRD: Collection Mode`. It is the fourth of the
     format's four homes: fences own decisions. Every owner decision and every owner-rejected
     finding lands here as its own dated fence, and this whole file travels as a `--source` in
     every review brief and every editing dispatch thereafter.
     Two fill families are placeholders: `{{...}}`, substituted at scaffold time, and `_(prompt)_`,
     filled during authoring; an unresolved instance of EITHER is a lock-blocking defect. Every
     guidance comment in this file is deleted at lock, so a rule a BUILDER needs after lock lives
     in body text and never in a comment. -->

# Decisions: Collection Mode
<!-- guidance: Collection Mode matches the PRD's title exactly; the file itself is
     `prd-collection-mode-fences.md` beside the PRD in docs/product/collection-mode.
     docs/product/collection-mode: the project's product-docs directory, where the PRD and its four
     companions live. -->

## Preamble
<!-- guidance: five things, in this order, and nothing else — the fourth and fifth only while
     an amendment is open.
     1. The settlement statement: what a fence is and what it costs to reopen one.
     2. The historical baseline: which fences predate the current lock, and the statement that
        they stand as recorded — an amendment preserves historical decisions, it does not
        re-decide them.
     3. The review-log path: the durable log this PRD's rounds append to, recorded once, at
        round 1, and never re-created.
     4. The amendment pending mark: the PRD status line's amendment clause, repeated here while
        that amendment's review rounds run and cleared by the bookkeeping close — absent from a
        document with no amendment open, which is why the rule below, and not a scaffolded line,
        is what this template carries.
     5. The resolved baselines: the two commit SHAs the amendment's diff checks compare against,
        both recorded once at round 1 — present only where an amendment or a re-lock is open, for
        the same reason as item 4. The PRESERVATION baseline is the commit at which the document
        was most recently locked; the CHANGE baseline is the merge-base of this amendment's branch
        with the trunk it targets. One cannot serve both: where the trunk moved after the lock,
        diffing from the lock reports changes this amendment did not make, and asking it to fence
        them is asking it to answer for work already authorized elsewhere. A diff check whose
        baseline is not recorded here is NOT-RUN, because two runs that resolved a baseline
        differently are not comparable. Every run records the `(baseline, head)` pair it used
        beside its result, and names which baseline it read; a branch name resolves differently on
        two days and a SHA does not, which is why these lines carry SHAs rather than branch names.
        `references/mechanical-checks.md` carries the per-check mapping.
     Fence language is not settlement: a fence records a decision the OWNER made, by link. An
     agent-authored amendment may proceed under one blanket approval, but every WHAT choice inside
     it is still listed for the owner and recorded as its own dated fence before re-lock. -->

Every fence below is **settled**: it is carried into every review brief and every editing dispatch
for this PRD, and it is never re-litigated. A reviewer finding that a fence already settles is not
re-raised.

**Historical baseline:** none yet — this PRD has not been locked, so no fence predates a lock. From
the first lock on, the fences that predate it stand as recorded; an amendment preserves them and
does not re-decide them.

**Review log:** `{{review-log-path}}`.
<!-- guidance: {{review-log-path}} is the path of the durable peer-review log for this PRD,
     created once at round 1 and appended to by every later round — recorded here so every later
     brief can find it without re-deriving it. -->

**Amendment pending mark.** While an amendment's review rounds run, and only then, this preamble
carries a fourth item: the same amendment clause the PRD's status line carries, naming the fence
range that authorises that amendment. Its presence says that the fences in that range are not yet
closed and the rows they touch are not yet re-ratified. The bookkeeping close clears it here and in
every other place the document carries it, in the one push that closes the amendment.

**Resolved baselines.** While that amendment is open, the preamble also records the two commits the
diff checks compare against, each resolved once at round 1 and written here as a SHA: the
**preservation baseline**, the commit at which this document was most recently locked, and the
**change baseline**, the merge-base of this amendment's branch with the trunk it targets. A check
that asks what has been preserved since the lock reads the first; a check that asks what this
amendment changed reads the second.

## Fences
<!-- guidance: one `### F<n> — <title> (<date>)` per decision, in ascending order, numbered per
     PRD. Fence numbers are assigned once and never reused.
     Each fence's four fields mean:
     - **Authority** — the owner comment or decision that made it, BY LINK. A fence with no
       authority is an agent's preference wearing a fence's clothes.
     - **Decision** — what was decided, in the owner's terms, scoped to what the owner actually
       decided. The most common agent over-reach is a fence that decides a phase or a numerator
       the owner did not.
     - **Why** — optional; include it only where the reason changes how the decision is applied.
     - **Carried by** — where the decision now lives. A decision with nothing carrying it has not
       landed. -->

**Fence grammar.** Every fence is a `### F<n> — <title> (<date>)` section carrying **Authority**
(the owner decision that made it, by link), **Decision** (what the owner decided, in the owner's
terms), **Why** (optional) and **Carried by**, in that order.

<!-- guidance: how a **Carried by** line is written, because the mechanical checks build the
     fence → row map from it rather than trusting the map section: a comma-separated list of IDs
     and nothing else — no prose, no ranges, no "see above" — each item a bare ID optionally
     preceded by the owning document's name, so the ID is the last whitespace-separated token of
     the item. Rationale belongs in **Why**, never here. -->

**Carried by grammar.** A **Carried by** value is a comma-separated list of the IDs the decision
now lives in — row IDs, case IDs, copy-state IDs, metric IDs and sibling fence numbers. A
cross-document reference is preceded by the name of the document that owns it ("the device PRD
R6.27"); an ID written bare is this PRD's own.

**A fence's original text is never rewritten.** A later clarification, narrowing, demotion or close
is appended under the fence as its own dated line, `**Clarified <date> (<authority>):** …`; where
an appended line and the original differ, the latest dated line governs. A fence cited from another
PRD is qualified by document ("the device PRD's F12"), exactly as a row ID is, because fence
numbering repeats across PRDs.

Fences F1–F4 were decided by the owner in the pre-fill adjudication of 2026-09-24, before any row
was drafted, because they set which rows exist at all. Each one's **Authority** quotes the question
put to the owner and the option the owner chose, verbatim; the question's other options are listed
so the scope of the choice is legible.

### F1 — The P0 browsing surface is a table with colour chips; the swatch grid stays P2 (2026-09-24)

- **Authority:** owner decision D1, pre-fill adjudication 2026-09-24. Question: "Which browsing
  surface is the P0 (first usable build) surface? The vision puts 'Collections + version history'
  at P0 but the gamut-aware swatch grid at P2." Chosen: **Table with chips** — "P0 = a sortable
  table, one row per item with a colour chip carrying the gamut, simulated and non-spectral marks
  (honesty marks are P0 wherever colour renders). The dense swatch-grid view mode stays P2 as the
  vision says." Not chosen: grid and table both P0; grid P0 with the table at P1.
- **Decision:** The first usable build browses a collection as a sortable table, one row per item,
  whose colour chip carries the gamut, simulated and non-spectral marks. Those honesty marks are P0
  on every surface that renders colour. The dense swatch-grid view is P2, as
  [the vision](../vision.md) sets it.
- **Carried by:** R2.1, R2.3, R2.4, R2.5, R4.2, R5.2, R7.1, R7.2, R8.9, E12

### F2 — v1 browses one collection at a time and also offers an All items view (2026-09-24)

- **Authority:** owner decision D2, pre-fill adjudication 2026-09-24. Question: "Does v1 browsing,
  search and sort work within one collection at a time, or across every collection in the file?"
  Chosen: **Also an 'All items' view** — "Add a library-wide view spanning every collection for
  search and sort (Data Foundation's single-file model supports it). More to specify: presenting
  the same Swatch Code in two collections." Not chosen: one collection at a time, with
  cross-collection search deferred to v2.
- **Decision:** v1 browses, searches and sorts within one chosen collection, and also offers an
  All items view spanning every collection in the file for search and sort. The owner did not
  decide the All items view's priority, which actions it offers beyond search and sort, or how the
  same Swatch Code in two collections is presented; each goes back to the owner.
- **Carried by:** R1.2, R1.9, R1.10, R3.6, R3.7, R8.2, E13

### F3 — v1 bulk operations are delete selected items and set a field on the selection (2026-09-24)

- **Authority:** owner decision D3, pre-fill adjudication 2026-09-24. Question: "Which bulk
  operations over a multi-item selection are in v1?" Chosen, of four: **Delete selected items** —
  "Count-labelled confirmation (Data Foundation's never-default and export-first rules), undoable
  while the file stays open." — and **Set a field on selection** — "Bulk metadata edit — set or
  clear one field across the selected items, with an explicit scope and a confirmation above a
  count." Not chosen: export only the selection; answer the correction question in bulk.
- **Decision:** The v1 bulk operations over a multi-item selection are deleting the selected items
  and setting or clearing one field across them. Exporting only a selection and answering the
  correction question for a selection are not bulk operations of this PRD; this fence does not
  touch how the Data Foundation PRD's R2.8 offers the unconfirmed set. The owner did not decide the
  operations' priority or the count above which a bulk edit confirms.
- **Carried by:** R6.1, R6.2, R6.3, E8

### F4 — v1 sorts by lightness, chroma and hue, and finds similar colours by ΔE2000 (2026-09-24)

- **Authority:** owner decision D4, pre-fill adjudication 2026-09-24. Question: "How much
  colour-aware navigation is in v1?" Chosen: **L/C/h sort + find similar** — "Sort by lightness,
  chroma, hue; 'Find similar' lists items within a ΔE2000 distance of a chosen item — inside the
  user's own collection only, never a named-library match." Not chosen: L/C/h sort only; text and
  field sort only.
- **Decision:** v1 sorts items by lightness, chroma and hue, and offers Find similar: the items
  within a ΔE2000 distance of a chosen item, drawn only from the user's own data and never matched
  against a named colour library. The owner did not decide the distance, the priority, or whether
  Find similar spans every collection when started from the All items view (F2); each goes back to
  the owner.
- **Carried by:** R3.3, R3.7, R3.8, E9

Fences F5–F20 were decided by the owner in the post-fill adjudication of 2026-09-24, over the forks
the Phase 2 fill surfaced. F5–F11 each answer one question put to the owner, quoted with its chosen
option. F12–F20 are the owner's approval, as a set, of nine numbered recommendations stated to the
owner in full before the question "Approve the numbered recommendations 1–9 listed above, as a set
(including the one change: column visibility kept in app preferences, not the file)?", answered
**Approve all 1–9**; each fence's Authority is its recommendation, cited by number, and settles the
choices that recommendation states and nothing more.

### F5 — The All items view is P1 and offers browsing and each item's detail (2026-09-24)

- **Authority:** owner decision D5, post-fill adjudication 2026-09-24. Question: "How should the
  All items view (your F2) be shaped?" Chosen: **P1, browse + detail** — "P1. Search, filters,
  sorts, L/C/h sort, Find similar, and each item's detail (so single-item edit/delete through the
  detail). No reorder, multi-select, bulk, or collection rename/delete/export. The same code in two
  collections = two rows, each naming its collection. No imported columns (they differ per
  collection); per-item simulated mark instead of the banner." Not chosen: the same view at P0; a
  P1 read-only list that opens an item in its own collection.
- **Decision:** Closes F2's open points. The All items view is P1. It offers search, filters, view
  sorts, the lightness, chroma and hue sorts, Find similar and each item's detail, and through that
  detail the single-item actions the detail offers; it offers no reordering, multi-item selection,
  bulk operation, or collection rename, delete or export. An item sharing a Swatch Code with an
  item in another collection is its own row naming its own collection; the view shows no imported
  columns and no simulated-readings banner, each item carrying its own simulated mark.
- **Carried by:** R1.9, R1.10, R3.7, R8.2, E13

### F6 — Two gamut marks: what this display cannot show, and the stored outside-sRGB flag (2026-09-24)

- **Authority:** owner decision D6, post-fill adjudication 2026-09-24. Question: "Gamut honesty on
  the chip: Data Foundation stores an 'outside sRGB' flag on each reading and leaves 'can this
  display show it' to this PRD. What does v1 mark?" Chosen: **Both, two marks** — "'Cannot show on
  this display' worked out live for the display the window is on, plus 'outside sRGB' from the
  stored flag (DF R3.4 says that flag travels to every view). On a P3 display a colour can be
  outside sRGB yet shown faithfully — the two marks say different true things." Not chosen: the
  display check only; the stored flag only.
- **Decision:** v1 marks, as two distinct marks, a colour the display showing the window cannot
  render — worked out live for that display — and a reading whose stored gamut-clipped flag is set.
  The owner did not decide the display test's rendering intent or how a display reporting no gamut
  is treated; those stay with the open question that carries them.
- **Carried by:** R2.4a, R2.4b, R2.5, R8.7, E12, M2

### F7 — Both bulk operations are P1, and a selection-scale delete confirmation lands in the Data Foundation PRD in this change (2026-09-24)

- **Authority:** owner decision D7, post-fill adjudication 2026-09-24. Question: "Bulk delete and
  bulk set-a-field (your F3): priority, and how is deleting a selection confirmed? DF today has only
  item-scale (E8) and collection-scale (E14) delete confirmations." Chosen: **P1; amend DF in this
  PR** — "Both P1. Add a selection-scale delete confirmation to Data Foundation beside E8/E14 in
  this same PR, so 'Delete selected' is buildable. Bulk set confirms above 10 items
  (BULK_CONFIRM_COUNT, open question); an empty value clears; Swatch Code can't be bulk-set; not
  offered in All items." Not chosen: P1 with "Delete selected" withheld until a later amendment;
  both at P0.
- **Decision:** Closes F3's open points. Deleting a selection and setting or clearing one field
  across a selection are both P1. Deleting a selection is confirmed by a selection-scale delete
  confirmation that the Data Foundation PRD gains, beside its item-scale and collection-scale ones,
  in the same change as this PRD, so "Delete selected" is offered rather than withheld. A bulk set
  or clear above BULK_CONFIRM_COUNT selected items confirms first, with 10 as the candidate held
  open by its question; an empty value clears the field; Swatch Code cannot be set in bulk; and
  neither bulk operation is offered in the All items view.
- **Carried by:** R6.1, R6.2, R6.3, E8, UJ6.3-a, UJ6.3-b, UJ6.3-c, UJ6.3-e, the Data Foundation PRD E33, the Data Foundation PRD R6.2, the Data Foundation PRD R7.6o, the Data Foundation PRD F50

### F8 — Editable item metadata, and no app-owned notes field in v1 (2026-09-24)

- **Authority:** owner decision D8, post-fill adjudication 2026-09-24. Question: "What item
  metadata can the user edit in v1? DF says 'metadata and notes are editable', but no PRD defines
  a notes field; free metadata today is imported columns, and a re-import can append new columns
  (Import R2.2)." Chosen: **As drafted, no notes** — "P0: edit Swatch Name, both alternates and
  imported values in place. P1: 'Change code' with a warning that later imports match the new code,
  refused during a session. No app-owned notes field in v1 — a Notes column can arrive through a
  CSV import." Not chosen: a per-item Notes field; adding a column by hand.
- **Decision:** Swatch Name, both alternates and each imported column's value are edited in place
  at P0. Changing a Swatch Code is P1, goes through a confirmation warning that a later import
  matches the new code, and is refused while a session on the collection is running. v1 has no
  app-owned notes field and no column added by hand; free-text metadata arrives as an imported
  column.
- **Carried by:** R4.3, R4.4, E15, E16

### F9 — Renaming an imported column changes the name the file stores; the Data Foundation and Inventory Import PRDs are amended in this change (2026-09-24)

- **Authority:** owner decision D9, post-fill adjudication 2026-09-24. Question: "Renaming an
  imported column (Import R2.6 hands it to this PRD; post-lock.md:94 leaves open whether the stored
  name moves). What does a rename do?" Chosen: **Stored name moves; amend now** — "P1. The file
  stores the new name, so the table, exports and outside readers show it, and a later import matches
  the new name (a CSV still carrying the old header then adds it as a new column). Amend Data
  Foundation R1.2 and Import R2.6 in this PR, closing the post-lock item." Not chosen: the same rule
  with the action withheld; a display-only name.
- **Decision:** "Rename column" is P1 and changes the name the file stores for that imported
  column, so the table, an export and an outside reader show the new name and a later import matches
  on it — a source still carrying the old header adding it as a new column. The Data Foundation
  PRD's R1.2 and the Inventory Import PRD's R2.6 are amended in the same change, closing the
  post-lock item that asked the question.
- **Carried by:** R4.8, E11, T8, UJ4.6-a, UJ4.6-e, UJ4.6-f, the Data Foundation PRD R1.2, the Inventory Import PRD R2.6, the Data Foundation PRD F50, the Inventory Import PRD F65

### F10 — A collection-side "Flag" on a captured item, P1 (2026-09-24)

- **Authority:** owner decision D10, post-fill adjudication 2026-09-24. Question: "Capture R9.9
  (locked) says 'that one was wrong' on a captured item is 'a re-scan or a \"Flag\" from the
  collection', but no row defines a collection-side Flag. Capture's Flag moves a captured item to
  set aside, its reading to history, until re-scanned." Chosen: **Add 'Flag' here, P1** — "The item
  detail offers Capture's 'Flag' on a captured item: it becomes set aside under Capture's rules (its
  reading kept as history) so the next session picks it up. Capture owns what Flag does; this PRD
  owns the entry point. Refused while a session is running." Not chosen: amending the capture PRD's
  R9.9 to drop the collection-side Flag.
- **Decision:** At P1 the item detail offers the capture PRD's "Flag" on a captured item; the item
  becomes set aside under the capture PRD's rules, its reading kept as history, so a later session
  picks it up. The capture PRD owns what a Flag does to the item; this PRD owns the entry point,
  which is refused while a session on the collection is running.
- **Carried by:** R4.9, R8.3, E6, T14, UJ4.7-a, UJ4.7-b, UJ4.7-c, UJ4.7-d, the capture PRD R9.9, the capture PRD F70

### F11 — An item whose current reading is quarantined is set aside, its cause "unreadable" (2026-09-24)

- **Authority:** owner decision D11, post-fill adjudication 2026-09-24. Question: "An item whose
  current reading is quarantined (unreadable, DF R5.5b) has no current value, so it fits none of
  Capture's row states (pending / captured = 'has a canonical value' / set aside). What row state is
  it?" Chosen: **Set aside** — "It becomes set aside (cause: unreadable) so the next session
  re-scans it. Changes counts and Capture's set-aside rules." Not chosen: captured with the
  unreadable mark (the orchestrator's recommendation); a new fourth row state.
- **Decision:** An item whose current reading is quarantined has the row state set aside, with the
  cause "unreadable", and is counted as set aside, so a later session re-scans it as it re-scans
  other set-aside items. The capture PRD's set-aside rules and counts are amended in the same change
  to carry this cause; the Data Foundation PRD's R5.5b quarantine and its offer of a re-scan or a
  restore of a readable earlier reading are unchanged.
- **Carried by:** R2.4g, R2.4h, R4.2b, E12, UJ2.1-j, UJ3.2-a, UJ4.1-c, UJ4.5-c, the capture PRD R8.2, the capture PRD R8.18, the capture PRD F70

### F12 — Priority is build order within v1 (2026-09-24)

- **Authority:** approved recommendation 1: "Priority means build order within v1; nothing is
  droppable."
- **Decision:** The Legend's priority semantic is build order within the release; every P0, P1 and
  P2 row ships in v1.
- **Why:** it sets the Legend's priority bullet, which carries no ID, so no row carries it.
- **Carried by:** governs no rows

### F13 — Metadata edits save per field, with undo while the file is open (2026-09-24)

- **Authority:** approved recommendation 2: "A metadata edit saves when you press Return or leave the
  field, and Escape discards it (P0). Undo of metadata edits works while the file is open (P1), and
  replaced values are never kept where an outside reader could recover them."
- **Decision:** As recommendation 2 states.
- **Carried by:** R4.3, R4.7

### F14 — Editing while a session is running (2026-09-24)

- **Authority:** approved recommendation 3: "While a session is running on a collection: deletes and
  code changes are blocked; other edits stay available; the reorder controls are hidden; an
  interrupted session blocks only deleting the collection."
- **Decision:** As recommendation 3 states.
- **Carried by:** R1.4, R2.9, R4.4, R4.5, R6.3, R8.3, E6

### F15 — The version history view's shape (2026-09-24)

- **Authority:** approved recommendation 4: "Version history — P0: the reading list, in recorded
  order and in measured (over-time) order, following DF R2.5. P1: each earlier reading's ΔE2000
  from the current value (never computed across different illuminants or observers), and restore
  via 'Use this reading'. P2: compare two readings. The correction question shows inline in the
  item detail, and 'Answer re-scans' on the collection list opens DF's E26."
- **Decision:** As recommendation 4 states. Where restore is offered stays with the Data Foundation
  PRD's R5.5b, which offers a restore of a readable earlier reading when the current reading is
  quarantined.
- **Carried by:** R4.2h, R5.1, R5.3, R5.4, R5.5, R5.7, R5.8, E17

**Clarified 2026-09-24 (owner decision D14, F23):** where restore is offered is extended by F23 —
"Use this reading" is also offered on an item set aside by a Flag, the flagged reading included.
The rest of this fence stands.

### F16 — Find similar: P1, its candidate distance, and its scope (2026-09-24)

- **Authority:** approved recommendation 5: "Find similar is P1. The candidate distance is ΔE2000
  3.0, held open as an OQ. Its scope is wherever it was started from, and items measured under a
  different illuminant, observer or condition are counted as not compared."
- **Decision:** Closes F4's open points: as recommendation 5 states, the distance staying a
  candidate under its open question.
- **Carried by:** R3.7, R3.8, E9

### F17 — What search matches (2026-09-24)

- **Authority:** approved recommendation 6: "Search uses Capture's Find rule (codes match from the
  start, names anywhere) plus imported values anywhere."
- **Decision:** As recommendation 6 states.
- **Carried by:** R3.1, E4

### F18 — Filters and the empty chip (2026-09-24)

- **Authority:** approved recommendation 7: "Filters work by row state and by each honesty mark.
  There are no filters on imported columns in v1, and an item with no value shows an empty chip."
- **Decision:** As recommendation 7 states.
- **Carried by:** R2.3, R3.4, R3.5, E5

### F19 — The hue sort's greys, the marks legend, the delete-confirmation key, the create label and the banner's action (2026-09-24)

- **Authority:** approved recommendation 8: "The hue sort puts near-greys (below C* 3.0, held open as
  an OQ) after the chromatic items, ordered by lightness. There's a 'Colour marks' legend, and
  Return fires Cancel in every delete confirmation. The collection list's create action is labelled
  'New collection', and the device PRD's E22 'Show simulated readings' action applies the simulated
  filter."
- **Decision:** As recommendation 8 states, NEUTRAL_CHROMA staying a candidate under its open
  question.
- **Carried by:** R1.2, R1.6, R2.6, R2.8, R3.3, E12

### F20 — Candidate budgets, what persists, and where column visibility is kept (2026-09-24)

- **Authority:** approved recommendation 9: "Candidate budgets, all held open as OQs: 100 ms at p95
  for search, filter and sort; 1 s to open a collection; swatches no smaller than 24 pt. Search,
  filters and sorts last while the file is open and are never written to it. One change from the
  draft: P2 column visibility persists across launches in app preferences, not in your data file.
  It's UI state, and the file should hold data."
- **Decision:** As recommendation 9 states: the three budgets stay candidates under their open
  questions; search, filters and view sorts last while the file is open and are never written to
  it; and the P2 column-visibility choice persists across launches outside the user's file.
- **Carried by:** R2.10, R3.6, R8.1, R8.6, R7.1, R7.2, R8.10, UJ2.2-a, UJ2.2-b

**Clarified 2026-09-24 (owner decision D12, F21):** the column-visibility clause of this fence is
superseded by F21 — the choice is kept with the collection in the file. The rest of this fence
stands.

Fences F21–F28 were decided by the owner in a second post-fill adjudication of 2026-09-24, over the
questions the round-0 fix pass surfaced. F21–F23 each answer one question, quoted with its chosen
option. F24–F28 are the owner's approval, as a set, of the five lettered loose ends (i)–(v) put in
the question "Approve these loose ends as a set?", answered **Approve all**; each fence's Authority
is its item, and settles what that item states and nothing more.

### F21 — Column visibility is kept with the collection in the file (2026-09-24)

- **Authority:** owner decision D12, second post-fill adjudication 2026-09-24. Question: "My F20
  change (P2 column visibility kept in app preferences, not the file) conflicts with locked Data
  Foundation R1.1: 'Nothing about a collection or session is kept elsewhere' — the file is the
  portable whole. How should column visibility persist?" Chosen: **In the file, per collection** —
  "Revert to the draft: the choice is kept with the collection in the file, so it travels with the
  file and honours DF R1.1. Supersedes that part of F20." Not chosen: not remembered at all; app
  preferences with an amendment to the Data Foundation PRD's R1.1.
- **Decision:** The P2 column-visibility choice is kept per collection, with that collection, in
  the user's file; nothing about it is kept elsewhere.
- **Carried by:** R2.10, R8.10, UJ2.2-a, UJ2.2-b

### F22 — The consequences of an item set aside as unreadable (2026-09-24)

- **Authority:** owner decision D13, second post-fill adjudication 2026-09-24. Question: "F11
  consequences, drafted as Capture R8.18 at P0: an item set aside because its current reading
  became unreadable is UNSETTLED (the review reopens and the collection is no longer 'finished'
  until it's re-scanned, restored or deliberately left); it neither counts toward nor breaks the
  consecutive-failure guard; restoring a readable earlier reading makes it captured again; it's
  excluded from Capture M4's per-session deferred rate and doesn't reopen M3 once recorded.
  Approve?" Chosen: **Approve as drafted.** Not chosen: arriving settled.
- **Decision:** As the question states, the capture PRD's R8.18 carrying it at P0.
- **Carried by:** the capture PRD R8.18, the capture PRD M3, the capture PRD M4, the capture PRD UJ3.3-h, the capture PRD F70, R4.2b, UJ4.1-c, UJ4.5-c

### F23 — Restore is offered on an item set aside by a Flag, and the Data Foundation PRD's R2.9 names the Flag (2026-09-24)

- **Authority:** owner decision D14, second post-fill adjudication 2026-09-24. Question: "An item
  set aside by the collection-side Flag (F10): can 'Use this reading' restore a readable earlier
  reading, including the flagged one? Related: DF R2.9 (locked) says 'only damage to the current
  reading removes the item's current value', which already contradicts Capture's Flag demotion
  (R5.6)." Chosen: **Offer restore; fix DF R2.9** — "Restore is offered on a flagged item, making
  it captured again — an out for a mistaken Flag. Amend DF R2.9 in this PR to name the operator's
  Flag as the other way a current value is removed." Not chosen: no restore on a flagged item.
- **Decision:** "Use this reading" is offered on a readable earlier reading — the flagged reading
  included — of an item set aside by a Flag, and makes the item captured again. The Data
  Foundation PRD's R2.9 is amended in this change to name the operator's Flag (the capture PRD's
  R5.6) as the other way an item's current value is removed.
- **Carried by:** R5.5, UJ5.3-e, UJ5.3-g, the capture PRD R5.6, the capture PRD UJ3.3-j, the capture PRD F9, the capture PRD F70, the Data Foundation PRD R2.9, the Data Foundation PRD DJ2, the Data Foundation PRD F33, the Data Foundation PRD F51

### F24 — Export first from a selection delete opens the whole-collection export (2026-09-24)

- **Authority:** approved loose end (i): "E33's 'Export first' opens the whole-collection export
  (Export has no selection scope; F3 excludes selection export), and Export's list of confirmations
  offering export-first gains E33."
- **Decision:** As item (i) states, the Data Export PRD's list amended in this change.
- **Carried by:** UJ6.3-e, the Data Foundation PRD R6.2, the Data Foundation PRD R7.6o, the Data Foundation PRD DJ4, the Data Foundation PRD F50, the Data Export PRD F31

### F25 — The All items view shows no simulated-readings banner, recorded on the device side (2026-09-24)

- **Authority:** approved loose end (ii): "Device gets a dated note, citing F5, that the All items
  view shows no simulated banner."
- **Decision:** As item (ii) states; F5 already decides the behaviour, and this fence authorises the
  device PRD's half of that seam.
- **Carried by:** R1.10, the device PRD F32

### F26 — "Answer re-scans" is not offered in the All items view (2026-09-24)

- **Authority:** approved loose end (iii): "'Answer re-scans' is removed from the All items view (F5
  didn't list it; it stays on the collection list)."
- **Decision:** As item (iii) states.
- **Carried by:** R5.7, E13

### F27 — Distance from the current value compares only like with like (2026-09-24)

- **Authority:** approved loose end (iv): "R5.4's distance compares only readings under the same
  illuminant, observer AND measurement condition (matches F16)."
- **Decision:** As item (iv) states.
- **Carried by:** R5.4

### F28 — An answered open question carries the status aligned (2026-09-24)

- **Authority:** approved loose end (v): "Answered OQ 8/9 carry status 'aligned'."
- **Decision:** As item (v) states, for OQ 8 and OQ 9.
- **Why:** it sets the Open questions table's Status cells, which carry no requirement ID.
- **Carried by:** governs no rows

## Fence → row map
<!-- guidance: one line per fence. This is the index the mechanical checks reconcile against the
     fence bodies: no map entry may point at deleted text, and every changed row must appear in
     some fence's Carried-by.
     Each line is written `- **F<n>** — <IDs>`, with `<IDs>` following the **Carried by** grammar
     above. No map line reads "see the lists above". -->

Each line of this map names the IDs one fence governs, by the **Carried by** grammar above — or
the single clause `governs no rows`, where a blanket amendment fence authorises a rewrite rather
than deciding a WHAT.

- **F1** — R2.1, R2.3, R2.4, R2.5, R4.2, R5.2, R7.1, R7.2, R8.9, E12
- **F2** — R1.2, R1.9, R1.10, R3.6, R3.7, R8.2, E13
- **F3** — R6.1, R6.2, R6.3, E8
- **F4** — R3.3, R3.7, R3.8, E9
- **F5** — R1.9, R1.10, R3.7, R8.2, E13
- **F6** — R2.4a, R2.4b, R2.5, R8.7, E12, M2
- **F7** — R6.1, R6.2, R6.3, E8, UJ6.3-a, UJ6.3-b, UJ6.3-c, UJ6.3-e, the Data Foundation PRD E33, the Data Foundation PRD R6.2, the Data Foundation PRD R7.6o, the Data Foundation PRD F50
- **F8** — R4.3, R4.4, E15, E16
- **F9** — R4.8, E11, T8, UJ4.6-a, UJ4.6-e, UJ4.6-f, the Data Foundation PRD R1.2, the Inventory Import PRD R2.6, the Data Foundation PRD F50, the Inventory Import PRD F65
- **F10** — R4.9, R8.3, E6, T14, UJ4.7-a, UJ4.7-b, UJ4.7-c, UJ4.7-d, the capture PRD R9.9, the capture PRD F70
- **F11** — R2.4g, R2.4h, R4.2b, E12, UJ2.1-j, UJ3.2-a, UJ4.1-c, UJ4.5-c, the capture PRD R8.2, the capture PRD R8.18, the capture PRD F70
- **F12** — governs no rows
- **F13** — R4.3, R4.7
- **F14** — R1.4, R2.9, R4.4, R4.5, R6.3, R8.3, E6
- **F15** — R4.2h, R5.1, R5.3, R5.4, R5.5, R5.7, R5.8, E17
- **F16** — R3.7, R3.8, E9
- **F17** — R3.1, E4
- **F18** — R2.3, R3.4, R3.5, E5
- **F19** — R1.2, R1.6, R2.6, R2.8, R3.3, E12
- **F20** — R2.10, R3.6, R8.1, R8.6, R7.1, R7.2, R8.10, UJ2.2-a, UJ2.2-b
- **F21** — R2.10, R8.10, UJ2.2-a, UJ2.2-b
- **F22** — the capture PRD R8.18, the capture PRD M3, the capture PRD M4, the capture PRD UJ3.3-h, the capture PRD F70, R4.2b, UJ4.1-c, UJ4.5-c
- **F23** — R5.5, UJ5.3-e, UJ5.3-g, the capture PRD R5.6, the capture PRD UJ3.3-j, the capture PRD F9, the capture PRD F70, the Data Foundation PRD R2.9, the Data Foundation PRD DJ2, the Data Foundation PRD F33, the Data Foundation PRD F51
- **F24** — UJ6.3-e, the Data Foundation PRD R6.2, the Data Foundation PRD R7.6o, the Data Foundation PRD DJ4, the Data Foundation PRD F50, the Data Export PRD F31
- **F25** — R1.10, the device PRD F32
- **F26** — R5.7, E13
- **F27** — R5.4
- **F28** — governs no rows

## Rejected findings
<!-- guidance: every reviewer finding the owner rejected, with the same authority-by-link
     discipline as a fence. A rejection recorded here is settled: it is carried into every later
     brief so the finding is never resurfaced round after round. Not conditional — a file with no
     rejections keeps the heading and says "none". -->

None — no review round has run yet.
