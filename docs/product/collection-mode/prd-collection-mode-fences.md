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

**Review log:** `docs/agent-reviews/2026-09-24-prd-collection-mode-peer-reviews.md` — created at round 1, 2026-09-24; every later round appends a `## Round N` section to it.
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

**Clarified 2026-09-25 (F40, F64, F72):** the points this fence left open are now decided, the two marks standing as decided here: the chip renders the working-set colour managed to the display and, on a display reporting sRGB or none, the cannot-show mark is set exactly when the stored flag is (F40); OQ 7's interim — Bradford adaptation at zero tolerance under relative colorimetric, a display reporting no gamut treated as sRGB — is ratified (F40, F64); and a window across two displays follows the one macOS reports it on (F72).

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

**Clarified 2026-09-25 (F48, F70):** a code equal to the item's own under the import PRD's R2.3 rule is stored as entered without the confirmation (F70), and a refused code returns to editing through "Try another code" (F48). The rest of this fence stands.

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

**Clarified 2026-09-25 (owner decision D17, F31):** "Rename column" first renders a confirmation stating the import consequence (E19); what a rename does to the stored name is unchanged.

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

**Clarified 2026-09-25 (owner decision D17, F31; F78):** the collection-side Flag first renders a confirmation stating its consequence (E18), and is not offered while the item's current reading awaits the correction answer (F78); the capture PRD's "Flag" label and what a Flag does are unchanged.

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

**Clarified 2026-09-24 (owner decision D15, F29):** the undo clause of this fence is refined by F29 — its own label, ended by any non-metadata action, re-checked against the forward rule, held in memory only, no redo.

### F14 — Editing while a session is running (2026-09-24)

- **Authority:** approved recommendation 3: "While a session is running on a collection: deletes and
  code changes are blocked; other edits stay available; the reorder controls are hidden; an
  interrupted session blocks only deleting the collection."
- **Decision:** As recommendation 3 states.
- **Carried by:** R1.4, R2.9, R4.4, R4.5, R6.3, R8.3, E6

**Clarified 2026-09-25 (owner decision D16, F30):** F30 extends this fence's blocked list — restoring a reading (the Data Foundation PRD's E4 restore and "Use this reading") and E10's "Undo" are refused with E6 while a session is in flight — and correction answers stay available. The rest of this fence stands.

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

**Clarified 2026-09-24 (owner decisions D19–D20, F33, F34, F41–F43):** the budgets are gated on an M1 MacBook Air with 8 GB until OQ 1 closes (F33); what they cover and how they are measured is F41's; F42 and F43 add BULK_WRITE_BUDGET and IMPORTED_COLUMNS_CEILING.

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

Fences F29–F57 were decided by the owner in the round-1 adjudication of 2026-09-24, over the
findings review round 1 accepted (the review log's Round 1 disposition table). F29–F35 each answer
one question, quoted with its chosen option. F36–F57 are the owner's approval, as a set, of 22
numbered recommendations stated to the owner in full before the question "Approve the numbered
recommendations 1–21 listed above as a set — plus 22: if DF's OQ 20 is still open at v1 release,
delete-undo (R1.7) is marked deferred and v1 ships final deletes behind the counted confirmation and
export-first?", answered **Approve all 1–22**; each fence's Authority is its recommendation, cited by
number and quoted, and settles what it states and nothing more. Carried-by lines marked for the
round-1 fix pass are filled by it.

### F29 — Metadata undo: one simple rule (2026-09-24)

- **Authority:** owner decision D15, round-1 adjudication 2026-09-24. Question: "Metadata undo (your F13, P1) — five lenses found it underspecified… Which model?" Chosen: **One simple rule** — "Its own label ('Undo change'), offered only while the most recent committed change is a metadata change — any other action (delete, Flag, restore, reorder, re-scan answer, import, re-read) ends the history. Each undo re-checks the forward rule (unique codes/names, the session guard) and is refused with that rule's state. Replaced values live only in the running app's memory. No Redo in v1." Not chosen: dropping metadata undo from v1; undo with redo.
- **Decision:** As the chosen option states; it refines F13's undo clause, which stays P1.
- **Carried by:** R4.7, R8.3, E3, E14, T10, UJ6.2-f, UJ6.4-a, UJ6.4-b, UJ6.4-c, UJ6.4-d, UJ6.4-f, UJ6.4-g, UJ6.4-h, UJ6.4-i, UJ6.4-j, UJ6.4-k

### F30 — Row-moving actions are refused while a session is in flight (2026-09-24)

- **Authority:** owner decision D16, round-1 adjudication 2026-09-24. Question: "While a capture session is running on a collection, what about actions that move rows under it: restoring a reading (DF E4's and 'Use this reading'), the delete 'Undo' (E10), and answering correction questions (DF E11)?" Chosen: **Block row-movers** — "Restores and the delete Undo are refused with E6 while a session is in flight — same as Flag and delete. Correction answers stay available (they move no row). Mirrored on Capture R5.6/R8.18." Not chosen: blocking correction answers too; allowing all with Capture defining the session's behaviour.
- **Decision:** As the chosen option states; it extends F14's list.
- **Carried by:** R1.7, R4.6, R5.5, R8.3, E6, UJ4.4-g, UJ4.5-d, UJ5.2-d, UJ5.3-h, the capture PRD R5.6, the capture PRD R8.18, the capture PRD F71

### F31 — "Flag" and "Rename column" each confirm first (2026-09-24)

- **Authority:** owner decision D17, round-1 adjudication 2026-09-24. Question: "Two actions change data with no warning: the collection-side 'Flag' (empties the chip, sets the item aside) and 'Rename column' (a later import of the old header adds a second column). Add confirmations?" Chosen: **Both confirm** — "Flag: 'Set ⟨code⟩ aside to scan again? Its current reading moves to its history…' Rename column: mirrors E16's code warning — imports match columns by name, so a spreadsheet with the old header adds it as a new column." Not chosen: Flag only; neither.
- **Decision:** Both actions render a confirmation stating their consequence before anything changes, worded in this PRD's copy file; the capture PRD's "Flag" label is unchanged (F10).
- **Carried by:** R4.8, R4.9, E18, E19, T8, T14, UJ4.6-a, UJ4.6-g, UJ4.6-h, UJ4.7-a, UJ4.7-e

### F32 — The item delete confirmation names the collection (2026-09-24)

- **Authority:** owner decision D18, round-1 adjudication 2026-09-24. Question: "Deleting from the All items view: DF's E8 confirmation reads 'Delete ⟨code⟩?' — two same-code items in different collections look identical at a final (P0) delete. Fix?" Chosen: **Name the collection** — "Amend DF E8 (and this PRD's E10) in this PR so the confirmation always names the collection: 'Delete ⟨code⟩ from ⟨collection⟩?'" Not chosen: naming it only from All items; no change.
- **Decision:** As the chosen option states, the Data Foundation PRD's E8 amended in this change.
- **Carried by:** R4.5, E10, T2, UJ4.4-a, UJ4.4-e, UJ6.3-d, UJ7.1-g, the Data Foundation PRD E8, the Data Foundation PRD F52

### F33 — The interim timing budgets are gated on an M1 MacBook Air, 8 GB (2026-09-24)

- **Authority:** owner decision D19, round-1 adjudication 2026-09-24. Question: "Which Mac gates the interim timing budgets (100 ms browse, 1 s open) until OQ 1 closes?" Chosen: **M1 MacBook Air, 8 GB** — "The oldest, lowest-spec Apple-silicon class — a realistic floor for a Cataloger's machine. Measured on its internal disk." Not chosen: the owner's current dev Mac; both.
- **Decision:** Until OQ 1 closes, BROWSE_RESPONSE_BUDGET, OPEN_COLLECTION_BUDGET and any budget F42 adds are measured on an M1 MacBook Air with 8 GB, on its internal disk.
- **Carried by:** R8.1, M1, UJ9.5-a, UJ9.5-b, UJ9.5-d

### F34 — FILE_ITEMS_CEILING's candidate is 100,000 items (2026-09-24)

- **Authority:** owner decision D20, round-1 adjudication 2026-09-24. Question: "FILE_ITEMS_CEILING — the scale the All items view and file-wide Find similar must hold budget at…" Chosen: **100,000** — "The scale the browsing research measured; ~10 full collections. Stays a candidate under OQ 2." Not chosen: 50,000; 30,000.
- **Decision:** As the chosen option states.
- **Carried by:** R8.2, UJ9.5-b

### F35 — Deleted, cleared or replaced text is gone from the file's bytes (2026-09-24)

- **Authority:** owner decision D21, round-1 adjudication 2026-09-24. Question: "When a user deletes a swatch or clears a field, how 'gone' must the old text be? DF R6.2a says 'unrecoverable from the active file by an outside reader' — ambiguous between a SQL client and anyone holding the file's bytes…" Chosen: **Gone from the bytes** — "Unrecoverable even by someone scanning a copy of the file's bytes. Amend DF R6.2a/R2.3 wording in this PR and hand ADR-0003 the requirement (it picks the mechanism, e.g. secure delete); tests read the file's bytes." Not chosen: SQL-level only with softened copy.
- **Decision:** As the chosen option states: the Data Foundation PRD's R6.2a and R2.3 are amended in this change so "unrecoverable by an outside reader" includes anyone reading the file's bytes; ADR-0003 chooses the mechanism; tests read the file's bytes.
- **Carried by:** R4.3, R4.7, R6.2, R8.10d, T1, T4, T6, UJ1.3-e, UJ1.3-f, UJ4.2-a, UJ4.2-c, UJ4.4-b, UJ6.2-c, UJ6.3-b, UJ6.4-j, UJ6.4-k, the Data Foundation PRD R2.3, the Data Foundation PRD R6.2a, the Data Foundation PRD R7.2, the Data Foundation PRD F52

### F36 — Approved recommendation 1: E12's "Outside sRGB" wording (2026-09-24)

- **Authority:** approved recommendation 1: "E12's 'Outside sRGB' wording becomes: its sRGB and HSL values, here, in your file and in exports, are the nearest sRGB colour, and its Lab, XYZ and spectral values are unaffected."
- **Decision:** As recommendation 1 states.
- **Carried by:** E12

### F37 — Approved recommendation 2: mark labels (2026-09-24)

- **Authority:** approved recommendation 2: "'Value missing' becomes 'Not in this condition'. 'Re-scan to answer' becomes 'Re-scan unanswered'. The reading mark 'unsettled / Not settled' becomes 'Awaiting answer'. Capture's settled/unsettled then means rows only, and the item detail uses Capture's user-facing words ('set aside for good' / 'set aside, still to deal with'). 'Unreadable' says the reading saved in the file is damaged."
- **Decision:** As recommendation 2 states.
- **Carried by:** R2.4i, R4.2b, R4.2h, R5.1, R5.2d, R5.3, R5.7, E12, E17, UJ4.1-c, UJ4.7-a, UJ5.1-a, UJ5.1-c

### F38 — Approved recommendation 3: the label table and the legend (2026-09-24)

- **Authority:** approved recommendation 3: "The copy file gains a label table: each mark's chip label, filter label and VoiceOver name. The Colour marks legend shows each mark's shape, is reachable from the item detail and history too, and explains Spread."
- **Decision:** As recommendation 3 states.
- **Carried by:** R2.4, R2.8, R8.9, E12, E14, E17, UJ2.1-k, UJ2.1-o, UJ9.6-a

### F39 — Approved recommendation 4: "Close" dismisses E9 and E12 (2026-09-24)

- **Authority:** approved recommendation 4: "E9 and E12's dismiss button becomes 'Close', because Capture reserves 'Done' on this surface."
- **Decision:** As recommendation 4 states.
- **Carried by:** R2.8, R3.8, E9, E12, UJ2.1-k, UJ3.4-b

### F40 — Approved recommendation 5: the chip shows the true colour (2026-09-24)

- **Authority:** approved recommendation 5: "The chip shows the true colour, colour-managed to the display, never the stored sRGB value. Outside the display's gamut it shows the clipped colour plus the cannot-show mark. On an sRGB display, cannot-show equals the stored flag. The gamut test adapts to the display white with Bradford (a standard chromatic-adaptation method) at zero tolerance. ZX-013 moves inside sRGB, and every fixture keeps a margin from the gamut edge."
- **Decision:** As recommendation 5 states; the Bradford-and-zero-tolerance clause joins OQ 7's interim.
- **Carried by:** R2.3, R2.5, R5.2e, R8.10c, M2, UJ2.1-b, UJ2.1-c, UJ9.8-a

### F41 — Approved recommendation 6: what the timing budgets cover and how they are measured (2026-09-24)

- **Authority:** approved recommendation 6: "They also cover selection, Select all, opening and closing the detail and history, scrolling (a dropped-frame statistic) and the grid. The clock starts when a save lands and ends at the first frame that shows the result. Measure on the internal disk, with a cold first open and warm browsing. M1 is read every release on the named Mac; per-PR timing is only a tripwire."
- **Decision:** As recommendation 6 states; the dropped-frame statistic is a new named constant under OQ 1.
- **Carried by:** R8.1, R8.1a, R8.1b, R8.1c, R8.1d, R8.1e, R8.1f, R8.2, R8.10f, M1, UJ9.5-a, UJ9.5-b

### F42 — Approved recommendation 7: bulk writes and BULK_WRITE_BUDGET (2026-09-24)

- **Authority:** approved recommendation 7: "Bulk writes at ROWS_CEILING show progress, keep the surface responsive, and finish within a new constant, BULK_WRITE_BUDGET (candidate 2 s)."
- **Decision:** As recommendation 7 states, the constant a candidate under OQ 1.
- **Carried by:** R8.1f, UJ9.5-a

### F43 — Approved recommendation 8: IMPORTED_COLUMNS_CEILING and behaviour above the ceilings (2026-09-24)

- **Authority:** approved recommendation 8: "New constant IMPORTED_COLUMNS_CEILING: budgets hold up to 20 imported columns of up to 200 characters. Above that, or above ROWS_CEILING, everything still works and nothing is refused, but the budgets aren't promised."
- **Decision:** As recommendation 8 states, the constant a candidate under an open question.
- **Carried by:** R8.1, UJ9.5-a, UJ9.5-c

### F44 — Approved recommendation 9: selection and live changes (2026-09-24)

- **Authority:** approved recommendation 9: "Any change that stops listing a selected item deselects it, and an item that changes is re-checked against the active search, filters and sort."
- **Decision:** As recommendation 9 states.
- **Carried by:** R3.9, R6.4, UJ6.1-c, UJ9.1-c, UJ9.1-d

### F45 — Approved recommendation 10: single-row selection is P0 (2026-09-24)

- **Authority:** approved recommendation 10: "Single-row selection is P0. Range, toggle and Select all stay P1."
- **Decision:** As recommendation 10 states.
- **Carried by:** R6.4, R4.5, UJ4.1-b, UJ4.4-c

### F46 — Approved recommendation 11: Find similar results, ties, and All items search (2026-09-24)

- **Authority:** approved recommendation 11: "Find similar: each result opens its item's detail; ties are ordered by code; it never computes a new derived value set. Search in the All items view also matches collection names."
- **Decision:** As recommendation 11 states.
- **Carried by:** R1.10, R3.7, E9, UJ3.4-d, UJ3.4-e, UJ3.4-f, UJ7.1-j

### F47 — Approved recommendation 12: displayed precision (2026-09-24)

- **Authority:** approved recommendation 12: "Displayed precision: L*, C* and h° to one decimal; Spread and ΔE to two. Sorting uses the stored values."
- **Decision:** As recommendation 12 states.
- **Carried by:** R2.11, UJ2.1-i, UJ3.4-a, UJ3.4-d, UJ4.1-a, UJ5.3-a, UJ5.3-b, UJ5.4-a

### F48 — Approved recommendation 13: abandoning a rename, and E15's action (2026-09-24)

- **Authority:** approved recommendation 13: "Renames: Escape abandons any rename and keeps the old name. E15's 'OK' becomes 'Try another code'."
- **Decision:** As recommendation 13 states.
- **Carried by:** R1.3, R4.4, R4.8, E15, UJ1.1-e, UJ4.3-d, UJ4.6-g

### F49 — Approved recommendation 14: read-only files (2026-09-24)

- **Authority:** approved recommendation 14: "Read-only files: a newer-format file gets Data Foundation R5.3's guaranteed minimum, with the rest left to its OQ 18. Write actions are shown disabled."
- **Decision:** As recommendation 14 states.
- **Carried by:** R8.4, E14, UJ9.2-a, UJ9.2-c

### F50 — Approved recommendation 15: item identity across a code change, and ADR-0003 inputs (2026-09-24)

- **Authority:** approved recommendation 15: "Changing a Swatch Code keeps the same item. This becomes a Data Foundation obligation and an input to the storage-schema decision (ADR-0003), along with per-collection column visibility and renamed column names. The Build dependencies table splits into 'stops' and 'proceeds under interim'."
- **Decision:** As recommendation 15 states, the Data Foundation half landing in this change.
- **Carried by:** R2.10, R4.4, R4.8, the Data Foundation PRD R1.2, the Data Foundation PRD F52

### F51 — Approved recommendation 16: metric M4 (2026-09-24)

- **Authority:** approved recommendation 16: "New metric M4: re-scans still unanswered a week after a dogfood session, target 0."
- **Decision:** As recommendation 16 states.
- **Carried by:** M4

### F52 — Approved recommendation 17: the Telemetry PRD's content limit (2026-09-24)

- **Authority:** approved recommendation 17: "Obligation handed to the future Telemetry PRD: no event about a Collection Mode action carries typed text, codes, names or values."
- **Decision:** As recommendation 17 states, as an outbound obligation to the queued Telemetry PRD.
- **Carried by:** R8.6

### F53 — Approved recommendation 18: nothing kept outside the file (2026-09-24)

- **Authority:** approved recommendation 18: "Nothing about search, view state or undo is written outside the file, including preferences and saved window state, and a test can read the app's own storage to check."
- **Decision:** As recommendation 18 states.
- **Carried by:** R8.6, R8.10e, UJ3.3-f, UJ6.4-j, UJ6.4-k, UJ9.4-b, UJ10.1-d

### F54 — Approved recommendation 19: L*, C* and h° in the All items view (2026-09-24)

- **Authority:** approved recommendation 19: "L*, C* and h° in the All items view: each item shows its own collection's values; sorting orders items that share the most common illuminant/observer, and the others follow, counted."
- **Decision:** As recommendation 19 states.
- **Carried by:** R1.9, R3.3, E3, E13, UJ7.1-i

### F55 — Approved recommendation 20: capture PRD R8.18 clarifications (2026-09-24)

- **Authority:** approved recommendation 20: "Capture R8.18 clarifications: 'counts as set aside' applies to collection tallies, not a session's; the unreadable standing always follows Data Foundation's quarantine mark; if an interrupted session's remembered row is deleted, the session resumes at the first pending row."
- **Decision:** As recommendation 20 states, the capture PRD's half landing in this change.
- **Carried by:** UJ4.4-f, the capture PRD R3.7, the capture PRD R8.18, the capture PRD UJ3.3-h, the capture PRD F71

### F56 — Approved recommendation 21: sibling pointer and wording fixes (2026-09-24)

- **Authority:** approved recommendation 21: "Pointer and wording fixes in sibling PRDs: Capture R11.15g and Capture's stale 'Collection Mode remains unwritten'; Data Foundation E33 says 'Export first saves all of ⟨collection⟩'."
- **Decision:** As recommendation 21 states.
- **Carried by:** the capture PRD R11.15g, the Data Foundation PRD E33, the capture PRD F71, the Data Foundation PRD F52

### F57 — Approved recommendation 22: delete-undo's release fate (2026-09-24)

- **Authority:** approved recommendation 22: "if DF's OQ 20 is still open at v1 release, delete-undo (R1.7) is marked deferred and v1 ships final deletes behind the counted confirmation and export-first."
- **Decision:** As recommendation 22 states.
- **Carried by:** R1.7

Fences F58–F90 are the owner's approval, on 2026-09-25, as a set, of 33 numbered recommendations over the round-1 findings no earlier fence settled — each stated to the owner in full before the question "Approve the 33 round-1 recommendations listed above as a set?", answered **Approve all 1–33**. Recommendation n is fence F(57+n); its Authority quotes the recommendation as it stands in the round-1 fix file's Owner-needed list (the box named in its title), and it settles what that recommendation states and nothing more.

### F58 — Approved round-1 recommendation 1 (PM Major 1) (2026-09-25)

- **Authority:** approved round-1 recommendation 1, answering the round-1 fix file's box PM Major 1: Is a collection rename undoable by "Undo change"? Recommended and approved: yes — it is name text like a column rename, re-checked against R1.3 on undo.
- **Decision:** As recommendation 1 states.
- **Carried by:** R4.7, UJ6.4-e

### F59 — Approved round-1 recommendation 2 (PM Nit 3 / IF-9) (2026-09-25)

- **Authority:** approved round-1 recommendation 2, answering the round-1 fix file's box PM Nit 3 / IF-9: Where are "Clear filters" and "Clear search" offered? Recommended and approved: on E3's and E13's "narrowed" variants too, each only while its own narrowing is active, as R3.4 already reads.
- **Decision:** As recommendation 2 states.
- **Carried by:** R3.4, E3, E13, UJ2.1-g, UJ3.1-a, UJ7.1-b

### F60 — Approved round-1 recommendation 3 (PM Nit 4) (2026-09-25)

- **Authority:** approved round-1 recommendation 3, answering the round-1 fix file's box PM Nit 4: Is "Show history" offered on an item with no readings? Recommended and approved: no, as "Find similar" is withheld without a current value; R4.2f says the item has no readings yet.
- **Decision:** As recommendation 3 states.
- **Carried by:** R4.2f, UJ5.1-e

### F61 — Approved round-1 recommendation 4 (PM unrated 1) (2026-09-25)

- **Authority:** approved round-1 recommendation 4, answering the round-1 fix file's box PM unrated 1: Moving an item between collections? Recommended and approved: a stated v1 non-goal in the Build contract; a re-import is the route.
- **Decision:** As recommendation 4 states.
- **Carried by:** governs no rows

### F62 — Approved round-1 recommendation 5 (PM unrated 2) (2026-09-25)

- **Authority:** approved round-1 recommendation 5, answering the round-1 fix file's box PM unrated 2: Copying a value to the clipboard? Recommended and approved: no v1 row beyond platform text selection; export and the file are the data routes.
- **Decision:** As recommendation 5 states.
- **Carried by:** governs no rows

### F63 — Approved round-1 recommendation 6 (PM unrated 3) (2026-09-25)

- **Authority:** approved round-1 recommendation 6, answering the round-1 fix file's box PM unrated 3: Column delete or merge? Recommended and approved: a stated v1 non-goal; F31's warning makes the split a knowing choice and hiding (R2.10) the mitigation.
- **Decision:** As recommendation 6 states.
- **Carried by:** governs no rows

### F64 — Approved round-1 recommendation 7 (PM unrated 4 / SSE unrated 4) (2026-09-25)

- **Authority:** approved round-1 recommendation 7, answering the round-1 fix file's box PM unrated 4 / SSE unrated 4: OQ 7's other interim clauses and OQ 11's interim? Recommended and approved: ratify both as drafted.
- **Decision:** As recommendation 7 states.
- **Carried by:** R2.5, R3.7, R5.4, R5.8, M2

### F65 — Approved round-1 recommendation 8 (SSE MJ3) (2026-09-25)

- **Authority:** approved round-1 recommendation 8, answering the round-1 fix file's box SSE MJ3: Is ADR-0003 a stop for every row? Recommended and approved: yes — every row reads or writes the file whose schema it fixes; the OQ interims proceed alongside.
- **Decision:** As recommendation 8 states.
- **Carried by:** governs no rows

### F66 — Approved round-1 recommendation 9 (SSE MJ8) (2026-09-25)

- **Authority:** approved round-1 recommendation 9, answering the round-1 fix file's box SSE MJ8: The dropped-frame constant's candidate (F41)? Recommended and approved: no more than 1% of frames missed while paging through ROWS_CEILING rows on F33's Mac, the interim equal to it.
- **Decision:** As recommendation 9 states.
- **Carried by:** R8.1e, UJ9.5-a

### F67 — Approved round-1 recommendation 10 (SSE MN2) (2026-09-25)

- **Authority:** approved round-1 recommendation 10, answering the round-1 fix file's box SSE MN2: How do the row-state and Spread columns sort, and does the chip column sort? Recommended and approved: row state in lifecycle order (pending, captured, set aside), Spread by number, and the chip column does not sort.
- **Decision:** As recommendation 10 states.
- **Carried by:** R3.2, UJ3.3-i, UJ3.3-j

### F68 — Approved round-1 recommendation 11 (SSE MN3) (2026-09-25)

- **Authority:** approved round-1 recommendation 11, answering the round-1 fix file's box SSE MN3: Where does a within-collection item with DF R3.3e's reference mismatch go in L*, C*, h° sorts? Recommended and approved: F54's rule — after the like-referenced items, counted.
- **Decision:** As recommendation 11 states.
- **Carried by:** R3.3, E3, UJ3.3-k

### F69 — Approved round-1 recommendation 12 (SSE MN4) (2026-09-25)

- **Authority:** approved round-1 recommendation 12, answering the round-1 fix file's box SSE MN4: How does a restore order against its source in "Measured order"? Recommended and approved: ties on measurement time break by record time, the restore after its source.
- **Decision:** As recommendation 12 states.
- **Carried by:** R5.3, UJ5.3-i

### F70 — Approved round-1 recommendation 13 (SSE MN5) (2026-09-25)

- **Authority:** approved round-1 recommendation 13, answering the round-1 fix file's box SSE MN5: A code equal to the item's own but for case or spacing? Recommended and approved: mirror R1.3 — stored as entered, no E16.
- **Decision:** As recommendation 13 states.
- **Carried by:** R4.4, UJ4.3-g

### F71 — Approved round-1 recommendation 14 (SSE MN6) (2026-09-25)

- **Authority:** approved round-1 recommendation 14, answering the round-1 fix file's box SSE MN6: "The item in view", and do swatch size and Grid/Table persist? Recommended and approved: the selected item if on screen, else the first on screen; both last while the file is open and are written nowhere, as R3.6 does.
- **Decision:** As recommendation 14 states.
- **Carried by:** R7.1, R7.2, R8.6, UJ10.1-d, UJ10.1-e

### F72 — Approved round-1 recommendation 15 (SSE MN7) (2026-09-25)

- **Authority:** approved round-1 recommendation 15, answering the round-1 fix file's box SSE MN7: Which display governs a straddling window? Recommended and approved: the one macOS reports the window is on (holding most of it).
- **Decision:** As recommendation 15 states.
- **Carried by:** R2.5, UJ2.1-m

### F73 — Approved round-1 recommendation 16 (SSE MN9) (2026-09-25)

- **Authority:** approved round-1 recommendation 16, answering the round-1 fix file's box SSE MN9: Which states render for an edit failing on a vanished volume, lost permission or a held file? Recommended and approved: cite Capture E26 and DF E10 where they apply; hand DF an obligation for a permission-lost state.
- **Decision:** As recommendation 16 states.
- **Carried by:** R8.8, UJ9.7-e, UJ9.7-f, the Data Foundation PRD F52

### F74 — Approved round-1 recommendation 17 (SSE MN10) (2026-09-25)

- **Authority:** approved round-1 recommendation 17, answering the round-1 fix file's box SSE MN10: What shows once an open detail's item is deleted? Recommended and approved: the detail and history close, returning to the table (E10 over it where R1.7 is built); the same after a re-read.
- **Decision:** As recommendation 17 states.
- **Carried by:** R4.1, UJ4.4-h, UJ9.3-b

### F75 — Approved round-1 recommendation 18 (SSE MN11) (2026-09-25)

- **Authority:** approved round-1 recommendation 18, answering the round-1 fix file's box SSE MN11: Keyboard routes for drag and Compare? Recommended and approved: leave the mechanism to the build under R8.9 and add a keyboard case for each.
- **Decision:** As recommendation 18 states.
- **Carried by:** R8.9, UJ5.4-b, UJ8.1-g

### F76 — Approved round-1 recommendation 19 (SSE MN18) (2026-09-25)

- **Authority:** approved round-1 recommendation 19, answering the round-1 fix file's box SSE MN18: Reordering while a search or filter narrows the table? Recommended and approved: neither drag nor "Use as scan order" is offered while narrowed.
- **Decision:** As recommendation 19 states.
- **Carried by:** R2.9, UJ8.1-h

### F77 — Approved round-1 recommendation 20 (SSE unrated 1) (2026-09-25)

- **Authority:** approved round-1 recommendation 20, answering the round-1 fix file's box SSE unrated 1: A flagged reading re-entering over-time views after a re-scan? Recommended and approved: accept for v1 (a Flag means scan again, not never true); log it for the DF owner on the post-lock list.
- **Decision:** As recommendation 20 states.
- **Carried by:** R4.9, UJ4.7-a

### F78 — Approved round-1 recommendation 21 (SSE unrated 2) (2026-09-25)

- **Authority:** approved round-1 recommendation 21, answering the round-1 fix file's box SSE unrated 2: Flag while the current reading awaits the correction answer? Recommended and approved: not offered until that question is answered (E11 sits in the same detail).
- **Decision:** As recommendation 21 states.
- **Carried by:** R4.9, UJ4.7-b

### F79 — Approved round-1 recommendation 22 (TEST n1) (2026-09-25)

- **Authority:** approved round-1 recommendation 22, answering the round-1 fix file's box TEST n1: Is a search that normalises to nothing an active search? Recommended and approved: no — no narrowing and no "narrowed" variant.
- **Decision:** As recommendation 22 states.
- **Carried by:** R3.1, UJ3.1-g

### F80 — Approved round-1 recommendation 23 (PMM Minor 2) (2026-09-25)

- **Authority:** approved round-1 recommendation 23, answering the round-1 fix file's box PMM Minor 2: The same spacing phrase in the capture PRD's aligned E1? Recommended and approved: fix it in this change under a dated Capture fence, since Capture is already being amended.
- **Decision:** As recommendation 23 states.
- **Carried by:** the capture PRD E1, the capture PRD F71

### F81 — Approved round-1 recommendation 24 (PMM Nit 5) (2026-09-25)

- **Authority:** approved round-1 recommendation 24, answering the round-1 fix file's box PMM Nit 5: Device's "simulated badge" wording? Recommended and approved: leave it for Device's next amendment; its R6.5 owns the word.
- **Decision:** As recommendation 24 states.
- **Carried by:** governs no rows

### F82 — Approved round-1 recommendation 25 (PMM unrated) (2026-09-25)

- **Authority:** approved round-1 recommendation 25, answering the round-1 fix file's box PMM unrated: A dogfood reading of E12? Recommended and approved: a one-line dogfood check beside M4, no metric.
- **Decision:** As recommendation 25 states.
- **Carried by:** M4

### F83 — Approved round-1 recommendation 26 (A13) (2026-09-25)

- **Authority:** approved round-1 recommendation 26, answering the round-1 fix file's box A13: Does a restore carry its source's device snapshot, agreement verdict and spread? Recommended and approved: yes — a DF clarification of R2.3f's "equal to H" and the ZX-015 B1 case.
- **Decision:** As recommendation 26 states.
- **Carried by:** R5.5, UJ5.3-k, the Data Foundation PRD R2.3f, the Data Foundation PRD F52

### F84 — Approved round-1 recommendation 27 (A14) (2026-09-25)

- **Authority:** approved round-1 recommendation 27, answering the round-1 fix file's box A14: What do the counts show when damage is found on a read-only file? Recommended and approved: whatever quarantine DF reports for the open file, persisted or not; nothing is written.
- **Decision:** As recommendation 27 states.
- **Carried by:** the capture PRD R8.18, the capture PRD F71

### F85 — Approved round-1 recommendation 28 (ARCH unrated 1) (2026-09-25)

- **Authority:** approved round-1 recommendation 28, answering the round-1 fix file's box ARCH unrated 1: "No stored cannot-show" as an ADR-0003 input? Recommended and approved: yes; F6 already keeps the mark live-only.
- **Decision:** As recommendation 28 states.
- **Carried by:** R2.5, the Data Foundation PRD F52

### F86 — Approved round-1 recommendation 29 (ARCH unrated 2) (2026-09-25)

- **Authority:** approved round-1 recommendation 29, answering the round-1 fix file's box ARCH unrated 2: Does All items search match imported values? Recommended and approved: yes, as R1.10 reads; say so in R1.10.
- **Decision:** As recommendation 29 states.
- **Carried by:** R1.10

### F87 — Approved round-1 recommendation 30 (PERF-6) (2026-09-25)

- **Authority:** approved round-1 recommendation 30, answering the round-1 fix file's box PERF-6: A budget for opening the All items view? Recommended and approved: OPEN_COLLECTION_BUDGET for its first rows at FILE_ITEMS_CEILING.
- **Decision:** As recommendation 30 states.
- **Carried by:** R8.2, UJ9.5-b

### F88 — Approved round-1 recommendation 31 (PERF-9) (2026-09-25)

- **Authority:** approved round-1 recommendation 31, answering the round-1 fix file's box PERF-9: Do the browse budgets hold from the first-rows moment? Recommended and approved: yes — no lazily loaded tail that a search waits on.
- **Decision:** As recommendation 31 states.
- **Carried by:** R8.1d

### F89 — Approved round-1 recommendation 32 (PERF-10) (2026-09-25)

- **Authority:** approved round-1 recommendation 32, answering the round-1 fix file's box PERF-10: Capture precedence over this PRD's writes and refreshes? Recommended and approved: yes — add the row and a Demo Device case during a bulk clear; F14's in-session edits stay available.
- **Decision:** As recommendation 32 states.
- **Carried by:** R8.11, UJ9.5-d

### F90 — Approved round-1 recommendation 33 (PERF-13) (2026-09-25)

- **Authority:** approved round-1 recommendation 33, answering the round-1 fix file's box PERF-13: FIND_BUDGET ≤ BROWSE_RESPONSE_BUDGET? Recommended and approved: yes — state it in OQ 1 and hand Capture a line at its OQ 13.
- **Decision:** As recommendation 33 states.
- **Carried by:** the capture PRD F71

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
- **F29** — R4.7, R8.3, E3, E14, T10, UJ6.2-f, UJ6.4-a, UJ6.4-b, UJ6.4-c, UJ6.4-d, UJ6.4-f, UJ6.4-g, UJ6.4-h, UJ6.4-i, UJ6.4-j, UJ6.4-k
- **F30** — R1.7, R4.6, R5.5, R8.3, E6, UJ4.4-g, UJ4.5-d, UJ5.2-d, UJ5.3-h, the capture PRD R5.6, the capture PRD R8.18, the capture PRD F71
- **F31** — R4.8, R4.9, E18, E19, T8, T14, UJ4.6-a, UJ4.6-g, UJ4.6-h, UJ4.7-a, UJ4.7-e
- **F32** — R4.5, E10, T2, UJ4.4-a, UJ4.4-e, UJ6.3-d, UJ7.1-g, the Data Foundation PRD E8, the Data Foundation PRD F52
- **F33** — R8.1, M1, UJ9.5-a, UJ9.5-b, UJ9.5-d
- **F34** — R8.2, UJ9.5-b
- **F35** — R4.3, R4.7, R6.2, R8.10d, T1, T4, T6, UJ1.3-e, UJ1.3-f, UJ4.2-a, UJ4.2-c, UJ4.4-b, UJ6.2-c, UJ6.3-b, UJ6.4-j, UJ6.4-k, the Data Foundation PRD R2.3, the Data Foundation PRD R6.2a, the Data Foundation PRD R7.2, the Data Foundation PRD F52
- **F36** — E12
- **F37** — R2.4i, R4.2b, R4.2h, R5.1, R5.2d, R5.3, R5.7, E12, E17, UJ4.1-c, UJ4.7-a, UJ5.1-a, UJ5.1-c
- **F38** — R2.4, R2.8, R8.9, E12, E14, E17, UJ2.1-k, UJ2.1-o, UJ9.6-a
- **F39** — R2.8, R3.8, E9, E12, UJ2.1-k, UJ3.4-b
- **F40** — R2.3, R2.5, R5.2e, R8.10c, M2, UJ2.1-b, UJ2.1-c, UJ9.8-a
- **F41** — R8.1, R8.1a, R8.1b, R8.1c, R8.1d, R8.1e, R8.1f, R8.2, R8.10f, M1, UJ9.5-a, UJ9.5-b
- **F42** — R8.1f, UJ9.5-a
- **F43** — R8.1, UJ9.5-a, UJ9.5-c
- **F44** — R3.9, R6.4, UJ6.1-c, UJ9.1-c, UJ9.1-d
- **F45** — R6.4, R4.5, UJ4.1-b, UJ4.4-c
- **F46** — R1.10, R3.7, E9, UJ3.4-d, UJ3.4-e, UJ3.4-f, UJ7.1-j
- **F47** — R2.11, UJ2.1-i, UJ3.4-a, UJ3.4-d, UJ4.1-a, UJ5.3-a, UJ5.3-b, UJ5.4-a
- **F48** — R1.3, R4.4, R4.8, E15, UJ1.1-e, UJ4.3-d, UJ4.6-g
- **F49** — R8.4, E14, UJ9.2-a, UJ9.2-c
- **F50** — R2.10, R4.4, R4.8, the Data Foundation PRD R1.2, the Data Foundation PRD F52
- **F51** — M4
- **F52** — R8.6
- **F53** — R8.6, R8.10e, UJ3.3-f, UJ6.4-j, UJ6.4-k, UJ9.4-b, UJ10.1-d
- **F54** — R1.9, R3.3, E3, E13, UJ7.1-i
- **F55** — UJ4.4-f, the capture PRD R3.7, the capture PRD R8.18, the capture PRD UJ3.3-h, the capture PRD F71
- **F56** — the capture PRD R11.15g, the Data Foundation PRD E33, the capture PRD F71, the Data Foundation PRD F52
- **F57** — R1.7
- **F58** — R4.7, UJ6.4-e
- **F59** — R3.4, E3, E13, UJ2.1-g, UJ3.1-a, UJ7.1-b
- **F60** — R4.2f, UJ5.1-e
- **F61** — governs no rows
- **F62** — governs no rows
- **F63** — governs no rows
- **F64** — R2.5, R3.7, R5.4, R5.8, M2
- **F65** — governs no rows
- **F66** — R8.1e, UJ9.5-a
- **F67** — R3.2, UJ3.3-i, UJ3.3-j
- **F68** — R3.3, E3, UJ3.3-k
- **F69** — R5.3, UJ5.3-i
- **F70** — R4.4, UJ4.3-g
- **F71** — R7.1, R7.2, R8.6, UJ10.1-d, UJ10.1-e
- **F72** — R2.5, UJ2.1-m
- **F73** — R8.8, UJ9.7-e, UJ9.7-f, the Data Foundation PRD F52
- **F74** — R4.1, UJ4.4-h, UJ9.3-b
- **F75** — R8.9, UJ5.4-b, UJ8.1-g
- **F76** — R2.9, UJ8.1-h
- **F77** — R4.9, UJ4.7-a
- **F78** — R4.9, UJ4.7-b
- **F79** — R3.1, UJ3.1-g
- **F80** — the capture PRD E1, the capture PRD F71
- **F81** — governs no rows
- **F82** — M4
- **F83** — R5.5, UJ5.3-k, the Data Foundation PRD R2.3f, the Data Foundation PRD F52
- **F84** — the capture PRD R8.18, the capture PRD F71
- **F85** — R2.5, the Data Foundation PRD F52
- **F86** — R1.10
- **F87** — R8.2, UJ9.5-b
- **F88** — R8.1d
- **F89** — R8.11, UJ9.5-d
- **F90** — the capture PRD F71

## Rejected findings
<!-- guidance: every reviewer finding the owner rejected, with the same authority-by-link
     discipline as a fence. A rejection recorded here is settled: it is carried into every later
     brief so the finding is never resurfaced round after round. Not conditional — a file with no
     rejections keeps the heading and says "none". -->

| Finding | Raised by | Disposition | Authority |
|---|---|---|---|
| Round 1 Finding-2 (Blocker): "Set a field" could set Swatch Code on a selection and create duplicate codes (R6.2) | peer-staff-software-engineer-reviewer, cross-model on agy | Not reproduced: the Vocabulary's editable field excludes Swatch Code, which only R4.4 changes (F7); not fixed | The review log's Round 1 disposition row 5 |
