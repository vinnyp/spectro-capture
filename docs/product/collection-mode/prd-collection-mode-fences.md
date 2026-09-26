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

**Clarified 2026-09-25 (owner decision D24, F100):** while a session is in flight on any collection, bulk writes are refused with E6; single edits stay available as this fence states.

**Clarified 2026-09-25 (owner decision D29, F136):** while a session is in flight on any collection, E10's "Undo" of a multi-item delete and "Undo change" of a bulk set or clear are refused with E6 as well; single edits stay available.

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

**Clarified 2026-09-25 (approved round-2 recommendation 16, F114):** Find similar compares working-set values only, and is not offered on an item without one; such an item's detail shows the value-absent mark in its Current value line.

### F17 — What search matches (2026-09-24)

- **Authority:** approved recommendation 6: "Search uses Capture's Find rule (codes match from the
  start, names anywhere) plus imported values anywhere."
- **Decision:** As recommendation 6 states.
- **Carried by:** R3.1, E4

**Clarified 2026-09-25 (approved round-2 recommendation 11, F109):** in the All items view a collection name matches anywhere in it, as names do, and E4 there has an "all items" variant saying collection names are searched.

### F18 — Filters and the empty chip (2026-09-24)

- **Authority:** approved recommendation 7: "Filters work by row state and by each honesty mark.
  There are no filters on imported columns in v1, and an item with no value shows an empty chip."
- **Decision:** As recommendation 7 states.
- **Carried by:** R2.3, R3.4, R3.5, E5

**Clarified 2026-09-25 (approved round-2 recommendation 12, F110):** E4 renders whenever the search lists no item, whatever filters are active; E5 renders only when the filters hide every item the search lists.

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

**Clarified 2026-09-25 (approved round-2 recommendation 6, F104):** the actions ending "Undo change" history are closed — any committed write other than a metadata change, a column hidden or shown and "New collection" included, ends it, never a capture save or a session starting, each undo re-checking R8.3; a refused undo keeps its entry; and the action is offered only where its latest change's collection is shown. R4.7 already ended the history at a re-read, which stays.

**Clarified 2026-09-25 (owner decision D28 and approved round-3 recommendation 11, F135 and F145):** no write a capture session makes ends the history, and E8 names what does — closing or re-reading the file, importing, hiding or showing a column, or any action but editing swatches' details, codes or names; "Undo change" is never offered in the All items view, an edit made there being undone in the item detail or on its collection's surface.

### F30 — Row-moving actions are refused while a session is in flight (2026-09-24)

- **Authority:** owner decision D16, round-1 adjudication 2026-09-24. Question: "While a capture session is running on a collection, what about actions that move rows under it: restoring a reading (DF E4's and 'Use this reading'), the delete 'Undo' (E10), and answering correction questions (DF E11)?" Chosen: **Block row-movers** — "Restores and the delete Undo are refused with E6 while a session is in flight — same as Flag and delete. Correction answers stay available (they move no row). Mirrored on Capture R5.6/R8.18." Not chosen: blocking correction answers too; allowing all with Capture defining the session's behaviour.
- **Decision:** As the chosen option states; it extends F14's list.
- **Carried by:** R1.7, R4.6, R5.5, R8.3, E6, UJ4.4-g, UJ4.5-d, UJ5.2-d, UJ5.3-h, the capture PRD R5.6, the capture PRD R8.18, the capture PRD F71

**Clarified 2026-09-25 (owner decision D29, F136):** E10's "Undo" of a multi-item delete is refused while a session is in flight on any collection, not only on its own; the "Undo" of one item is refused on its own collection, as before.

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

**Clarified 2026-09-25 (owner decision D26, F102):** "gone" holds from the moment the write lands, in the file and any journal, log or index beside it, while the file is open and after a crash.

**Clarified 2026-09-25 (approved round-3 recommendation 5, F139):** the moment F102 fixes comes once every earlier read of the file has ended; what waits for it is F139's.

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

**Clarified 2026-09-25 (approved round-2 recommendation 29, F127):** the Data Foundation PRD's R3.4 flag is worked out by OQ 7's interim test at sRGB, so the stored flag and the cannot-show check are one computation, and ADR-0003 takes that as an input; UJ2.1-q is the one declared edge case exempt from the Harness's margin rule.

**Clarified 2026-09-25 (approved round-3 recommendation 12, F146):** the stored flag's Bradford-and-zero-tolerance test is now the Data Foundation PRD's R3.4's own, under its F54; OQ 7's interim still governs the display check.

### F41 — Approved recommendation 6: what the timing budgets cover and how they are measured (2026-09-24)

- **Authority:** approved recommendation 6: "They also cover selection, Select all, opening and closing the detail and history, scrolling (a dropped-frame statistic) and the grid. The clock starts when a save lands and ends at the first frame that shows the result. Measure on the internal disk, with a cold first open and warm browsing. M1 is read every release on the named Mac; per-PR timing is only a tripwire."
- **Decision:** As recommendation 6 states; the dropped-frame statistic is a new named constant under OQ 1.
- **Carried by:** R8.1, R8.1a, R8.1b, R8.1c, R8.1d, R8.1e, R8.1f, R8.2, R8.10f, M1, UJ9.5-a, UJ9.5-b

**Clarified 2026-09-25 (approved round-2 recommendation 21, F119):** a cold first open means the OS file cache purged and the Data Foundation PRD's open-file check already finished, as the journeys' timing workload declares.

### F42 — Approved recommendation 7: bulk writes and BULK_WRITE_BUDGET (2026-09-24)

- **Authority:** approved recommendation 7: "Bulk writes at ROWS_CEILING show progress, keep the surface responsive, and finish within a new constant, BULK_WRITE_BUDGET (candidate 2 s)."
- **Decision:** As recommendation 7 states, the constant a candidate under OQ 1.
- **Carried by:** R8.1f, UJ9.5-a

**Clarified 2026-09-25 (owner decision D23, F99):** deletes have their own budget, DELETE_WRITE_BUDGET; BULK_WRITE_BUDGET stays for set, clear, reorder and their undo.

**Clarified 2026-09-25 (approved round-2 recommendation 30, F128):** a bulk write done within BROWSE_RESPONSE_BUDGET needs no progress frame.

### F43 — Approved recommendation 8: IMPORTED_COLUMNS_CEILING and behaviour above the ceilings (2026-09-24)

- **Authority:** approved recommendation 8: "New constant IMPORTED_COLUMNS_CEILING: budgets hold up to 20 imported columns of up to 200 characters. Above that, or above ROWS_CEILING, everything still works and nothing is refused, but the budgets aren't promised."
- **Decision:** As recommendation 8 states, the constant a candidate under an open question.
- **Carried by:** R8.1, UJ9.5-a, UJ9.5-c

**Clarified 2026-09-25 (approved round-2 recommendations 20 and 31, F118 and F129):** the budgets hold up to HISTORY_READINGS_CEILING, candidate 500 readings on one item, and in a file of up to FILE_ITEMS_CEILING items; above any ceiling everything still works and nothing is refused, but no budget is promised.

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

**Clarified 2026-09-25 (approved round-2 recommendations 9 and 25, F107 and F123):** closing a detail opened from E9 returns to E9 with its list; in the All items view a tie on both distance and Swatch Code falls to collection-list order.

**Clarified 2026-09-25 (approved round-3 recommendation 10, F144):** E9 is worked out again whenever a detail opened from it closes, and deleting that result returns to it.

### F47 — Approved recommendation 12: displayed precision (2026-09-24)

- **Authority:** approved recommendation 12: "Displayed precision: L*, C* and h° to one decimal; Spread and ΔE to two. Sorting uses the stored values."
- **Decision:** As recommendation 12 states.
- **Carried by:** R2.11, UJ2.1-i, UJ3.4-a, UJ3.4-d, UJ4.1-a, UJ5.3-a, UJ5.3-b, UJ5.4-a

**Clarified 2026-09-25 (approved round-2 recommendation 7, F105):** a*, b*, u* and v* show to one decimal as L* does, X, Y and Z to two on a 0–100 scale, sRGB as 0–255 integers, and HSL as whole degrees and percents.

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

**Clarified 2026-09-25 (owner decision D22, F91):** the Build dependencies split is carried by labelling each item in the format's "What must remain open" column 'Stop:' or 'Interim:', not by a fourth column.

### F51 — Approved recommendation 16: metric M4 (2026-09-24)

- **Authority:** approved recommendation 16: "New metric M4: re-scans still unanswered a week after a dogfood session, target 0."
- **Decision:** As recommendation 16 states.
- **Carried by:** M4

**Clarified 2026-09-25 (approved round-2 recommendation 8, F106):** M4's population is the dogfood sessions ending with at least one re-scan awaiting an answer, that count recorded at the start; a session with none reads not measured, never 0.

### F52 — Approved recommendation 17: the Telemetry PRD's content limit (2026-09-24)

- **Authority:** approved recommendation 17: "Obligation handed to the future Telemetry PRD: no event about a Collection Mode action carries typed text, codes, names or values."
- **Decision:** As recommendation 17 states, as an outbound obligation to the queued Telemetry PRD.
- **Carried by:** R8.6

### F53 — Approved recommendation 18: nothing kept outside the file (2026-09-24)

- **Authority:** approved recommendation 18: "Nothing about search, view state or undo is written outside the file, including preferences and saved window state, and a test can read the app's own storage to check."
- **Decision:** As recommendation 18 states.
- **Carried by:** R8.6, R8.10e, UJ3.3-f, UJ6.4-j, UJ6.4-k, UJ7.1-r, UJ9.4-b, UJ10.1-d

**Clarified 2026-09-25 (approved round-2 recommendation 32, F130):** the file gets no system-kept versions, and no collection content goes to system search or Handoff.

### F54 — Approved recommendation 19: L*, C* and h° in the All items view (2026-09-24)

- **Authority:** approved recommendation 19: "L*, C* and h° in the All items view: each item shows its own collection's values; sorting orders items that share the most common illuminant/observer, and the others follow, counted."
- **Decision:** As recommendation 19 states.
- **Carried by:** R1.9, R3.3, E3, E13, UJ7.1-i

**Clarified 2026-09-25 (approved round-2 recommendation 15, F113):** the like pair is, in a collection's table, the collection's own reference and, in the All items view, the most common among listed items with a value, ties per F94; a non-spectral reading at a reference other than its collection's is never like.

**Clarified 2026-09-25 (approved round-3 recommendation 8, F142):** a mismatched non-spectral reading does not count toward the most common illuminant and observer.

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

**Clarified 2026-09-25 (approved round-5 recommendation 10, F187):** the Data Foundation PRD's R6.3 and its OQ 20 carry the same deferral, so if that OQ is still open at v1 release, delete-undo is deferred there too and v1's deletes are final.

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

**Clarified 2026-09-25 (approved round-2 recommendation 15, F113):** in a collection's table the like reference is always the collection's own, however many mismatched readings it lists.

**Clarified 2026-09-25 (approved round-3 recommendation 8, F142):** such a reading counts toward no pair in the All items view's majority either.

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

**Clarified 2026-09-25 (owner decision D25, F101):** the Data Foundation PRD gains the permission-lost state in this change rather than owing it.

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

**Clarified 2026-09-25 (approved round-2 recommendation 28, F126):** the deferral is tracked by a Device line on docs/product/post-lock.md, "simulated badge" becoming "mark" at Device's next amendment.

### F82 — Approved round-1 recommendation 25 (PMM unrated) (2026-09-25)

- **Authority:** approved round-1 recommendation 25, answering the round-1 fix file's box PMM unrated: A dogfood reading of E12? Recommended and approved: a one-line dogfood check beside M4, no metric.
- **Decision:** As recommendation 25 states.
- **Carried by:** M4

**Clarified 2026-09-25 (approved round-2 recommendation 36, F134):** the reading runs on a Display P3 display with an outside-sRGB swatch listed.

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

**Clarified 2026-09-25 (owner decision D27, F103):** the All items view's budgets hold at the full per-collection imported width across the file.

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

**Clarified 2026-09-25 (owner decision D24 and approved round-2 recommendation 22, F100 and F120):** with bulk writes refused while any session is in flight, UJ9.5-d drives the searches, sorts and single edits R8.3 leaves available, timed to each set's last sample; while the capture PRD's OQ 5 leaves ROW_CONFIRM_BUDGET TBD, R8.11 runs against the engineering plan's declared value.

### F90 — Approved round-1 recommendation 33 (PERF-13) (2026-09-25)

- **Authority:** approved round-1 recommendation 33, answering the round-1 fix file's box PERF-13: FIND_BUDGET ≤ BROWSE_RESPONSE_BUDGET? Recommended and approved: yes — state it in OQ 1 and hand Capture a line at its OQ 13.
- **Decision:** As recommendation 33 states.
- **Carried by:** the capture PRD F71

Fences F91–F98 were decided by the owner on 2026-09-25 over the forks the round-1 fix pass surfaced. F91 answers one question, quoted with its chosen option. F92–F98 are the owner's approval, as a set, of seven lettered fixes (a)–(g) put in the question "Approve these small consistency fixes as a set?", answered **Approve all (a)–(g)**; each fence's Authority is its item, quoted, and settles what it states and nothing more.

### F91 — The Build dependencies split is labelled within the format's columns (2026-09-25)

- **Authority:** owner decision D22, 2026-09-25. Question: "My recommendation 15 (fence F50) split the Build dependencies table into 'stops' and 'proceeds under interim' columns — but the agent-prd v1 format fixes that table's three columns (only requirement tables may gain a column). How should the split land?" Chosen: **Label within the column** — "Keep the format's three columns; each item under 'What must remain open' is prefixed 'Stop:' or 'Interim:'. Same information, stays conformant. A dated note under F50 records it." Not chosen: keeping a fourth column and forking the format.
- **Decision:** As the chosen option states; it refines F50's table clause.
- **Carried by:** governs no rows
- **Why:** it sets the Legend's Build dependencies table, which carries no ID.

### F92 — Approved fix (a) Escape abandons a code change (2026-09-25)

- **Authority:** approved fix (a): "Escape also abandons a Swatch Code change, as it does a rename."
- **Decision:** As the item states.
- **Carried by:** R4.4, UJ4.3-h

### F93 — Approved fix (b) A column renamed to itself (2026-09-25)

- **Authority:** approved fix (b): "Renaming a column to its own name with only case/spacing changed is stored as typed, with no E19 import warning (mirrors codes)."
- **Decision:** As the item states.
- **Carried by:** R4.8, UJ4.6-i

### F94 — Approved fix (c) The All items tie on the most common illuminant/observer (2026-09-25)

- **Authority:** approved fix (c): "All items L*/C*/h° sort: when two illuminant/observer pairs tie for most common, the pair of the collection first in the collection list wins."
- **Decision:** As the item states.
- **Carried by:** R3.3, UJ7.1-l

### F95 — Approved fix (d) Capture E30's spacing phrase (2026-09-25)

- **Authority:** approved fix (d): "Capture E30 carries the same 'spacing doesn't count' overclaim as E1 — fix it the same way."
- **Decision:** As the item states.
- **Carried by:** the capture PRD E30, the capture PRD F71

### F96 — Approved fix (e) A copy home for set-aside cause names (2026-09-25)

- **Authority:** approved fix (e): "Set-aside cause names get a copy home in Capture's copy file (Capture owns the causes), cited by this PRD's item detail."
- **Decision:** As the item states.
- **Carried by:** R4.2b, UJ4.1-c, UJ4.7-a, the capture PRD F71

### F97 — Approved fix (f) E18's phase mark (2026-09-25)

- **Authority:** approved fix (f): "E18's mention of 'Use this reading' is phase-marked until R5.5 lands."
- **Decision:** As the item states.
- **Carried by:** R4.9, E18, UJ4.7-a, UJ4.7-f

**Clarified 2026-09-25 (approved round-2 recommendation 13, F111):** the same pattern marks E6's and E17's sentences naming P1 actions — E6's "full" and E17's "restore" variants, each listed by its enumerating row.

**Clarified 2026-09-25 (approved round-3 recommendation 7, F141):** E18's "restore" variant is gated as E6's "full" is — it renders in a build where every P1 action it names has landed.

### F98 — Approved fix (g) The decision-queue and post-lock halves (2026-09-25)

- **Authority:** approved fix (g): "The out-of-scope halves land in this PR: the ADR-0003 row in docs/decisions/README.md gains the five inputs, and post-lock.md gains the flagged-reading item (F77) and DF's owed permission-lost state."
- **Decision:** As the item states.
- **Carried by:** governs no rows

Fences F99–F134 were decided by the owner on 2026-09-25 over the findings review round 2 accepted. F99–F103 each answer one question, quoted with its chosen option. F104–F134 are the owner's approval, as a set, of round-2 recommendations 6–36 as they stand in the round-2 fix file's Owner-needed list (recommendation n is fence F(98+n)), stated to the owner in full before the question "Approve round-2 recommendations 6–36 above as a set?", answered **Approve all 6–36**.

### F99 — Deletes get their own time budget (2026-09-25)

- **Authority:** owner decision D23, round-2 adjudication 2026-09-25. Question: "Deleting a full collection (10,000 items) while erasing the old bytes, as you required (F35), measured 3.05 s on the fastest Mac — over the 2 s bulk-write budget (F42), and 'Delete collection' has no budget at all today. How should delete time be bounded?" Chosen: **Deletes get their own budget** — "A delete of up to a full collection shows progress and finishes within a new DELETE_WRITE_BUDGET (candidate 10 s on the M1 Air, an open question), the app staying responsive. The 2 s budget stays for set/clear/reorder. F35's byte rule unchanged." Not chosen: keeping 2 s for deletes via ADR-0003; scrubbing freed bytes in the background.
- **Decision:** As the chosen option states; it refines F42 for deletes.
- **Carried by:** R8.1, R8.1f, R8.1g, UJ9.5-g

### F100 — No bulk writes while any session is in flight (2026-09-25)

- **Authority:** owner decision D24, round-2 adjudication 2026-09-25. Question: "The file has one writer. A 2 s bulk write (F42) while a capture session is running would stall capture's saves, which R8.11 forbids. Which rule?" Chosen: **No bulk writes in a session** — "While any capture session is in flight, bulk set/clear, 'Delete selected', 'Delete collection' and 'Use as scan order' are refused with E6; single edits stay available (each ~10 ms). Simple and testable; narrows F14's 'other edits stay available' for bulk only." Not chosen: bulk writes that yield or queue behind capture, engineered under ADR-0005.
- **Decision:** As the chosen option states; it narrows F14 for bulk writes only and applies to a session on any collection.
- **Carried by:** R1.4, R6.3, R8.3, E6, UJ1.2-f, UJ6.2-i, UJ6.2-j, UJ6.3-f, UJ8.1-j, UJ9.5-d, the capture PRD F72

**Clarified 2026-09-25 (owner decisions D29–D30, F136, F137):** the refusal also covers E10's "Undo" of a multi-item delete and "Undo change" of a bulk set or clear; and while a bulk write or delete runs, no session starts or resumes and no other write this PRD offers starts.

### F101 — The Data Foundation PRD gains its permission-lost state in this change (2026-09-25)

- **Authority:** owner decision D25, round-2 adjudication 2026-09-25. Question: "R8.8 must show a state when a save is refused because the app lost permission to the file — Data Foundation owes that state and hasn't written it… What now?" Chosen: **Add the DF state now** — "Data Foundation gains a 'can't save to your file' state in this PR (wording goes through the same review); R8.8 renders it, with a case. Closes the post-lock item." Not chosen: a Stop until the Data Foundation PRD writes it.
- **Decision:** As the chosen option states; it refines F73.
- **Carried by:** R8.8, R8.10a, UJ9.7-g, the Data Foundation PRD R1.10, the Data Foundation PRD R7.3j, the Data Foundation PRD R7.6p, the Data Foundation PRD E34, the Data Foundation PRD F53

**Clarified 2026-09-25 (owner decision D31, F138):** the state's recovery copy is cause-neutral and offers choosing the file again.

### F102 — Removed text is gone from the moment the write lands, side files included (2026-09-25)

- **Authority:** owner decision D26, round-2 adjudication 2026-09-25. Question: "Your F35 says deleted, cleared or replaced text is gone from the file's bytes — but not WHEN, or whether the database's side files (journal/WAL/index) count… Which?" Chosen: **From the moment it saves** — "Gone from the file and any journal, log or index beside it as soon as the write lands — while the file is open and after a crash. Matches the cases already written; handed to ADR-0003 and DF R6.2a in full." Not chosen: by the next checkpoint or close.
- **Decision:** As the chosen option states; it refines F35.
- **Carried by:** R1.3, R8.8, R8.10d, T1, T5, T6, UJ1.3-c, UJ4.2-a, UJ4.2-c, UJ4.4-b, UJ6.3-b, UJ9.8-c, the Data Foundation PRD R2.3, the Data Foundation PRD R6.2a, the Data Foundation PRD F53

**Clarified 2026-09-25 (approved round-3 recommendation 5, F139):** capture saves never wait on a removing edit: ADR-0003 takes as an input that a text-removing write lands once every earlier read has ended, this app's own reads (an export, All items' cold load) running in transactions no longer than BROWSE_RESPONSE_BUDGET; an outside reader's hold leaves the edit shown not yet saved; the Data Foundation PRD's R1.5 help-docs line says reading the file elsewhere during capture may delay saves; and R8.1c times a user's own write from its input.

**Clarified 2026-09-25 (owner decision D38, F157):** removed text is gone from the file's bytes from the moment the write lands, unless a read begun before the write is still running — another app's, or an export this app is making (F158). The text is then wiped when that read ends, or when the file next opens. While another app's read holds it, the Data Foundation PRD's notice says so.

**Clarified 2026-09-25 (owner decision D44 and approved round-5 recommendation 7, F175 and F184):** the read that defers a wipe is any read begun before that wipe, another app's or this app's own; of this app's reads only an export, Save a copy and a re-read's checks may run longer than BROWSE_RESPONSE_BUDGET.

### F103 — All items search holds its budget at full imported width (2026-09-25)

- **Authority:** owner decision D27, round-2 adjudication 2026-09-25. Question: "All items search matches imported values (your F86). At the file-wide ceiling that's up to 100,000 items × 20 columns × 200 characters searched per keystroke within 100 ms on the M1 Air — likely needing a search index in the file (which must then obey your byte-erasure rule). What width must All items search hold its budget at?" Chosen: **Full width** — "Budgets hold at the full per-collection imported width across the whole file; the storage-schema decision (ADR-0003) takes the index-or-scan choice as an input, under the byte rule. Keeps F86 as approved." Not chosen: imported values unbudgeted; no imported values in All items.
- **Decision:** As the chosen option states.
- **Carried by:** R8.2, UJ9.5-b

### F104 — Approved round-2 recommendation 6: Which actions end "Undo change" history, and where it is offered (2026-09-25)

- **Authority:** approved round-2 recommendation 6: any committed write but a metadata change ends it (column hide/show and "New collection" too), a capture save or session start not (each undo re-checks R8.3); a refused undo keeps its entry; offered only where its collection is shown.
- **Decision:** As recommendation 6 states.
- **Carried by:** R4.7, E8, UJ6.4-i, UJ6.4-l, UJ6.4-m, UJ6.4-n

**Clarified 2026-09-25 (owner decision D28, F135):** nothing a capture session writes ends the history, not only a save or a session start.

**Clarified 2026-09-25 (approved round-3 recommendation 11, F145):** "where its latest change's collection is shown" never includes the All items view; the item detail and the collection's surface offer it.

### F105 — Approved round-2 recommendation 7: Displayed precision of the other derived values (2026-09-25)

- **Authority:** approved round-2 recommendation 7: a*, b*, u*, v* to one decimal as L*; X, Y, Z to two on 0–100; sRGB as 0–255 integers; HSL as whole degrees and percents.
- **Decision:** As recommendation 7 states.
- **Carried by:** R2.11, UJ4.1-a

### F106 — Approved round-2 recommendation 8: M4's population (2026-09-25)

- **Authority:** approved round-2 recommendation 8: dogfood sessions ending with at least one re-scan awaiting an answer, the start count recorded; a session with none reads "not measured", never 0.
- **Decision:** As recommendation 8 states.
- **Carried by:** M4

### F107 — Approved round-2 recommendation 9: Where closing a detail opened from Find similar returns (2026-09-25)

- **Authority:** approved round-2 recommendation 9: to E9 with its list, a case opening two results in turn.
- **Decision:** As recommendation 9 states.
- **Carried by:** R4.1, UJ3.4-e

**Clarified 2026-09-25 (approved round-3 recommendation 10, F144):** on return E9 is worked out again for the same source — a deleted result gone, a changed one at its new distance or no longer listed — and deleting a result opened from E9 returns to E9, the table showing only if the source itself is gone.

### F108 — Approved round-2 recommendation 10: R4.8's duplicate set against the headers users see (2026-09-25)

- **Authority:** approved round-2 recommendation 10: it covers every header the copy file's Column headers table shows as well as the collection's columns; E11's "duplicate" variant unchanged.
- **Decision:** As recommendation 10 states.
- **Carried by:** R4.8, UJ4.6-j

### F109 — Approved round-2 recommendation 11: How a collection name matches in All items search, and E4 there (2026-09-25)

- **Authority:** approved round-2 recommendation 11: anywhere in the name, as names match (F17); E4 in the All items view says collection names are searched, a variant R3.5 enumerates.
- **Decision:** As recommendation 11 states.
- **Carried by:** R1.10, R3.5, E4, UJ7.1-j, UJ7.1-m

### F110 — Approved round-2 recommendation 12: E4 or E5 when the search alone lists nothing (2026-09-25)

- **Authority:** approved round-2 recommendation 12: E4 whenever the search lists no item, whatever filters are active; E5 only when the search lists items and the filters hide them all; UJ3.2-d follows.
- **Decision:** As recommendation 12 states.
- **Carried by:** R3.5, UJ3.2-d, UJ3.2-e

### F111 — Approved round-2 recommendation 13: Phase-marking E6's and E17's sentences that name P1 actions (2026-09-25)

- **Authority:** approved round-2 recommendation 13: yes, F97's pattern — each moves to a `[phase: variant-absent]` variant its enumerating row lists.
- **Decision:** As recommendation 13 states.
- **Carried by:** R5.3, R8.3, E6, E17, UJ5.3-n

**Clarified 2026-09-25 (approved round-3 recommendations 6 and 7, F140 and F141):** E6's "full" variant no longer names undoing a delete and renders once every P1 action it names lands, so R1.7's deferral never holds it back; one gate, "every P1 action that variant names", reads the same in R8.3, the copy and the Harness; and while a session is in flight E6 renders its body, "full" or "elsewhere", "interrupted" rendering only while none is.

### F112 — Approved round-2 recommendation 14: An open item detail and history view when their item changes (2026-09-25)

- **Authority:** approved round-2 recommendation 14: they show the change as the table does (R3.9), within R8.1c's budget.
- **Decision:** As recommendation 14 states.
- **Carried by:** R8.1c, UJ9.1-l

### F113 — Approved round-2 recommendation 15: R3.3's like set (2026-09-25)

- **Authority:** approved round-2 recommendation 15: in a collection's table, the collection's own reference; in All items, the most common pair among listed items with a value (ties per F94); a mismatched non-spectral reading never.
- **Decision:** As recommendation 15 states.
- **Carried by:** R3.3, UJ3.3-m, UJ7.1-n

**Clarified 2026-09-25 (approved round-3 recommendation 8, F142):** a mismatched non-spectral reading does not count toward the All items view's most common pair.

### F114 — Approved round-2 recommendation 16: Find similar's compared values, and an item whose working-set value is absent (2026-09-25)

- **Authority:** approved round-2 recommendation 16: compare working-set values only; not offered when the chosen item's is absent; R4.2c shows value-absent for such an item.
- **Decision:** As recommendation 16 states.
- **Carried by:** R3.7, R3.8, R4.2c, E9, UJ3.4-g, UJ3.4-h

### F115 — Approved round-2 recommendation 17: The distance line on an item with no current value (2026-09-25)

- **Authority:** approved round-2 recommendation 17: no distance line shows.
- **Decision:** As recommendation 17 states.
- **Carried by:** R5.4, UJ5.3-l

### F116 — Approved round-2 recommendation 18: "Use this reading" on a never-true reading (2026-09-25)

- **Authority:** approved round-2 recommendation 18: offered, as R5.5 reads — the way back from a mistaken "old reading was wrong" answer, the original keeping its never-true mark; add a case.
- **Decision:** As recommendation 18 states.
- **Carried by:** R5.5, UJ5.3-m

### F117 — Approved round-2 recommendation 19: How "Set a field" commits and cancels (2026-09-25)

- **Authority:** approved round-2 recommendation 19: as R4.3 — Return applies (through E8 above BULK_CONFIRM_COUNT) and Escape cancels; no new label.
- **Decision:** As recommendation 19 states.
- **Carried by:** R6.2, UJ6.2-k

**Clarified 2026-09-25 (approved round-3 recommendation 9, F143):** in E8, Return fires "Cancel", as in every delete confirmation (R1.6).

### F118 — Approved round-2 recommendation 20: A bound on readings for R8.1b's history-open budget (2026-09-25)

- **Authority:** approved round-2 recommendation 20: the budget holds up to 500 readings per item (UJ9.5-e timed), a candidate under OQ 1; above it the view works and no budget is promised.
- **Decision:** As recommendation 20 states.
- **Carried by:** R8.1, R8.1b, UJ9.5-e

**Clarified 2026-09-25 (approved round-3 recommendation 14, F148):** HISTORY_READINGS_CEILING closes by the owner's estimate of the longest history a real item reaches, timed by UJ9.5-e.

### F119 — Approved round-2 recommendation 21: What "cold" means for the first open (2026-09-25)

- **Authority:** approved round-2 recommendation 21: the OS file cache purged and the Data Foundation PRD's open-file check already finished.
- **Decision:** As recommendation 21 states.
- **Carried by:** UJ9.5-a, UJ9.5-b

### F120 — Approved round-2 recommendation 22: R8.11's interim while Capture's ROW_CONFIRM_BUDGET is TBD (2026-09-25)

- **Authority:** approved round-2 recommendation 22: the engineering plan's declared value (Capture's OQ 5 leaves it there), R8.11 listed under Interim stated.
- **Decision:** As recommendation 22 states.
- **Carried by:** R8.11, UJ9.5-d

### F121 — Approved round-2 recommendation 23: DF E11 and E26's "not yet settled" (2026-09-25)

- **Authority:** approved round-2 recommendation 23: "marked as awaiting your answer", under a dated DF fence (DF F53), both keeping their alignment.
- **Decision:** As recommendation 23 states.
- **Carried by:** the Data Foundation PRD E11, the Data Foundation PRD E26, the Data Foundation PRD F53

### F122 — Approved round-2 recommendation 24: Capture R5.8 and R6.6's cause wording (2026-09-25)

- **Authority:** approved round-2 recommendation 24: fix now — both quote "flagged as missing or damaged", R8.2's words, in a dated line under Capture F71.
- **Decision:** As recommendation 24 states.
- **Carried by:** the capture PRD R5.8, the capture PRD R6.6, the capture PRD F71

### F123 — Approved round-2 recommendation 25: An All-items Find similar tie on distance and code (2026-09-25)

- **Authority:** approved round-2 recommendation 25: then by collection, in collection-list order, as F94 breaks ties.
- **Decision:** As recommendation 25 states.
- **Carried by:** R3.7, UJ3.4-i

### F124 — Approved round-2 recommendation 26: ADR-0006 as a stop for every row (2026-09-25)

- **Authority:** approved round-2 recommendation 26: yes — the paragraph under Build dependencies says so beside ADR-0003 (F65), and row 1's item goes, word-neutral.
- **Decision:** As recommendation 26 states.
- **Why:** it sets the Build dependencies paragraph and removes an item from its first row's cells, which carry no ID.
- **Carried by:** governs no rows

### F125 — Approved round-2 recommendation 27: "Samples disagreed" as a cause and a mark (2026-09-25)

- **Authority:** approved round-2 recommendation 27: the filter label becomes "Samples disagreed, average accepted" (R4.2d's words), and E12 says a set-aside swatch shows it only as its State cause.
- **Decision:** As recommendation 27 states.
- **Carried by:** E12

### F126 — Approved round-2 recommendation 28: A post-lock Device line for F81's deferral (2026-09-25)

- **Authority:** approved round-2 recommendation 28: yes — "simulated badge" to "mark" at Device's next amendment, citing F81.
- **Decision:** As recommendation 28 states.
- **Why:** it adds a Device line to docs/product/post-lock.md, which carries no ID.
- **Carried by:** governs no rows

### F127 — Approved round-2 recommendation 29: How the stored gamut-clipped flag is computed (2026-09-25)

- **Authority:** approved round-2 recommendation 29: a DF clarification and ADR-0003 input that DF R3.4's flag uses OQ 7's interim test at sRGB, plus one declared edge case exempt from the margin rule.
- **Decision:** As recommendation 29 states.
- **Carried by:** UJ2.1-q, the Data Foundation PRD R3.4, the Data Foundation PRD F53

**Clarified 2026-09-25 (approved round-3 recommendation 12, F146):** the Data Foundation PRD's R3.4 now states the flag's test itself — Bradford to the display white, relative colorimetric, zero tolerance, the adaptation its sRGB derivation uses — under its F54, so closing OQ 7 changes it only through a Data Foundation fence and a new derivation version; ADR-0003's input follows.

### F128 — Approved round-2 recommendation 30: Progress for a bulk write faster than BROWSE_RESPONSE_BUDGET (2026-09-25)

- **Authority:** approved round-2 recommendation 30: no progress frame needed.
- **Decision:** As recommendation 30 states.
- **Carried by:** R8.1f, R8.1g, UJ9.5-g

### F129 — Approved round-2 recommendation 31: File-level limits (2026-09-25)

- **Authority:** approved round-2 recommendation 31: R8.1's budgets hold in a file of up to FILE_ITEMS_CEILING items, and above FILE_ITEMS_CEILING everything works, nothing refused, no budget, a function-only case at twice it.
- **Decision:** As recommendation 31 states.
- **Carried by:** R8.1, UJ9.5-a, UJ9.5-f

### F130 — Approved round-2 recommendation 32: System-kept document versions and system donations (2026-09-25)

- **Authority:** approved round-2 recommendation 32: yes — R8.6 states the file gets no system-kept versions and no collection content goes to system search or Handoff (DF R1.1, F53 imply it).
- **Decision:** As recommendation 32 states.
- **Carried by:** R8.6, R8.10e, UJ9.4-d

**Clarified 2026-09-25 (approved round-4 recommendation 12, F172):** "Handoff" in R8.6 means the system handing an activity to another device. It does not mean the user's own copy and paste, Universal Clipboard included.

### F131 — Approved round-2 recommendation 33: Help-docs transparency about backups and snapshots (2026-09-25)

- **Authority:** approved round-2 recommendation 33: no change here; the help docs (DF R1.5's) are outside this PRD — note it for them.
- **Decision:** As recommendation 33 states.
- **Why:** it notes the help-docs item on docs/product/post-lock.md, which carries no ID; no row here changes.
- **Carried by:** governs no rows

### F132 — Approved round-2 recommendation 34: The verification seam in shipped builds (2026-09-25)

- **Authority:** approved round-2 recommendation 34: R8.10's readbacks other than R8.10f exist only in test builds, and no build opens a listening socket or cross-process service for them.
- **Decision:** As recommendation 34 states.
- **Carried by:** R8.10, UJ9.4-e

**Clarified 2026-09-25 (approved round-4 recommendation 2, F162):** R8.10d and R8.10e are reads made from outside the app, not in-app readbacks. So a case may make them against a Release build, and the readbacks kept to test builds are all but R8.10d–f.

### F133 — Approved round-2 recommendation 35: E19 saying a column is permanent (2026-09-25)

- **Authority:** approved round-2 recommendation 35: yes — "A column can't be removed once it's added" (true under F63); copy file only.
- **Decision:** As recommendation 35 states.
- **Carried by:** E19

### F134 — Approved round-2 recommendation 36: F82's reading of E12 on a Display P3 screen (2026-09-25)

- **Authority:** approved round-2 recommendation 36: yes — the check runs on a P3 display with an outside-sRGB swatch listed.
- **Decision:** As recommendation 36 states.
- **Carried by:** M4

Fences F135–F150 were decided by the owner on 2026-09-25 over the findings review round 3 accepted. F135–F138 each answer one question, quoted with its chosen option. F139–F150 are the owner's approval, as a set, of round-3 recommendations 5–16 (recommendation n is fence F(134+n)), each stated to the owner before the question "Approve round-3 recommendations 5–16 above as a set?", answered **Approve all 5–16**. Recommendations 5–14 and 16 are as they stand in the round-3 fix file's Owner-needed list; recommendation 15 was stated to the owner as "an import commit also waits while any capture session is running. This is an Import PRD amendment in this PR, for consistency with the one-writer rule", which governs over the fix file's text for item 15.

### F135 — No write a capture session makes ends the undo history (2026-09-25)

- **Authority:** owner decision D28, round-3 adjudication 2026-09-25. Question: "Metadata 'Undo change' (F29/F104): today a capture save or session start doesn't end its history, but a Skip, Flag, pause or end-session during capture DOES — while E8 tells the user 'scanning doesn't count'. Which?" Chosen: **No capture write ends it** — "Nothing a capture session writes ends the undo history (each undo still re-checks the session guard). E8's text is rewritten to name exactly what does end it: closing or re-reading the file, importing, hiding/showing a column, or any action other than editing swatch details, codes or names." Not chosen: exempting only saves and starts.
- **Decision:** As the chosen option states; it refines F29 and F104.
- **Carried by:** R4.7, E8, UJ6.4-m, UJ6.4-o

**Clarified 2026-09-25 (approved round-4 recommendation 3, F163):** a write the app makes on its own never ends the history either, for example the Data Foundation PRD's damage mark saved when the file is read. Only the user's own writes do.

### F136 — Bulk undos are refused while any session is in flight (2026-09-25)

- **Authority:** owner decision D29, round-3 adjudication 2026-09-25. Question: "Your F100 refuses bulk writes while any session runs, but two big writes slip through: the delete 'Undo' (E10) of a selection/collection delete (up to 10 s), and 'Undo change' of a bulk set/clear. Refuse them too?" Chosen: **Refuse both** — "While any capture session is in flight, E10's 'Undo' of a multi-item delete and 'Undo change' of a bulk set or clear are refused with E6's 'elsewhere' variant, like the bulk writes themselves. Mirrored in Capture." Not chosen: refusing only the delete undo.
- **Decision:** As the chosen option states; it extends F100.
- **Carried by:** R8.3, E6, UJ1.3-g, UJ4.4-j, UJ6.3-g, UJ6.4-p, the capture PRD F73

**Clarified 2026-09-25 (approved round-4 recommendation 1, F161):** "a multi-item delete" means a selection or collection delete, whatever its item count.

### F137 — A session waits, shown, while a bulk write or delete runs (2026-09-25)

- **Authority:** owner decision D30, round-3 adjudication 2026-09-25. Question: "F100 works one way only: nothing stops a capture session STARTING while a bulk write or delete (up to 10 s) already holds the file's single writer. Which?" Chosen: **Session waits, shown** — "While a bulk write or delete runs, no session starts or resumes and no other write this PRD offers starts — each is shown unavailable until the write lands (its progress is showing). A Capture half lands in this PR." Not chosen: the session starting and the write yielding under ADR-0005.
- **Decision:** As the chosen option states; it is F100's reverse ordering.
- **Carried by:** R8.1f, R8.1g, UJ9.5-g, the capture PRD R3.5, the capture PRD T7, the capture PRD F73

**Clarified 2026-09-25 (owner decision D33, F152):** while such a write runs, no other write anywhere in the app starts — an import commit, the Data Foundation PRD's re-read or move, and the capture PRD's collection creation included — each shown unavailable until it lands.

**Clarified 2026-09-25 (owner decision D36, F155):** the rule's home moves to the Data Foundation PRD's one-writer rule. R8.1f and the capture PRD's rows cite that rule instead of restating it.

### F138 — The Data Foundation PRD's E34 recovery copy is cause-neutral and offers choosing the file again (2026-09-25)

- **Authority:** owner decision D31, round-3 adjudication 2026-09-25. Question: "Data Foundation's new E34 ('Can't save to your file') tells users to check Finder's Get Info → Sharing & Permissions. That fixes nothing when access was lost through macOS privacy settings or a sandbox (ADR-0007 is still open). Reword?" Chosen: **Neutral + choose file** — "'SpectroCapture can no longer change your file at ⟨path⟩, so the last thing you did wasn't saved… Check that the file isn't locked and that SpectroCapture is still allowed to change it, then try again — or choose the file again.' Adds a 'Choose the file again' action; ADR-0007 gets an input to name the exact place once it decides." Not chosen: neutral wording without the new action.
- **Decision:** As the chosen option states; it refines F101.
- **Carried by:** the Data Foundation PRD E34, the Data Foundation PRD R7.3j, the Data Foundation PRD R7.6p, the Data Foundation PRD F54

**Clarified 2026-09-25 (owner decisions D32, D34; F151, F153):** "Choose the file again" restores access only, the user then pressing "Try again"; E34 also says OK leaves the change unsaved.

### F139 — Approved round-3 recommendation 5: Removing writes against open readers (2026-09-25)

- **Authority:** approved round-3 recommendation 5: capture saves never wait on a removing edit — an ADR-0003 input that a text-removing write lands once every earlier read has ended, this app's own reads (an export, All items' cold load) running in transactions no longer than BROWSE_RESPONSE_BUDGET; an outside reader's hold leaves the edit shown not yet saved, and DF R1.5's help-docs line says reading the file elsewhere during capture may delay saves; R8.1c times a user's own write from the input (+8 body words); the UJ9.5-d run PERF3-7 names.
- **Decision:** As recommendation 5 states.
- **Carried by:** R8.1, R8.1c, M1, UJ9.5-a, UJ9.5-d, the Data Foundation PRD R1.5, the Data Foundation PRD F54

**Clarified 2026-09-25 (owner decisions D38 and D39, F157 and F158):** an outside reader's hold no longer leaves the edit shown not yet saved. The edit shows saved, and only the wipe of the text it removed waits (F157). An export reads one snapshot (F158), so "an export" leaves the ADR-0003 input's short-transaction reads.

### F140 — Approved round-3 recommendation 6: Which E6 variant renders when two apply (2026-09-25)

- **Authority:** approved round-3 recommendation 6: an in-flight session wins — the body, "full" or "elsewhere" — and "interrupted" renders only while no session is in flight, because resuming the interrupted session is refused while another is active; the copy's "interrupted" condition adds "and no session is in flight" (no body words), R8.3's "beside" reworded word-neutrally, and a UJ1.2 case for each collision.
- **Decision:** As recommendation 6 states.
- **Carried by:** R8.3, E6, UJ1.2-g, UJ1.2-h

### F141 — Approved round-3 recommendation 7: E6's "full" gate if R1.7 is deferred (2026-09-25)

- **Authority:** approved round-3 recommendation 7: "full" renders once every P1 action it names lands, E10's "Undo" aside, and drops "and undoing a delete" (the headline covers a delete made before the session); one gate, "every P1 action that variant names", in R8.3 (+3 words), the copy and the Harness; a dated line under F111.
- **Decision:** As recommendation 7 states.
- **Carried by:** R8.3, E6

### F142 — Approved round-3 recommendation 8: How a mismatched non-spectral reading counts toward the All items like pair (2026-09-25)

- **Authority:** approved round-3 recommendation 8: it doesn't count — F113 makes it never like, so the majority counts only readings that can be like ("…the most common among listed items with a value, a mismatched non-spectral reading not counting", about +6 words); R3-m10's inks case plus a second fixture, because the first cannot tell "not counting" from "counting under its collection's pair".
- **Decision:** As recommendation 8 states.
- **Carried by:** R3.3, UJ7.1-p, UJ7.1-q

### F143 — Approved round-3 recommendation 9: What Return fires in E8 (2026-09-25)

- **Authority:** approved round-3 recommendation 9: "Cancel", as in every delete confirmation (R1.6) — E8 exists to stop an accidental change to many swatches, and a second press of Return would otherwise apply it unread; R6.2 gains "in E8 Return fires "Cancel"" (about +6 words) and a UJ6.2 case.
- **Decision:** As recommendation 9 states.
- **Carried by:** R6.2, UJ6.2-l

### F144 — Approved round-3 recommendation 10: E9 after a result opened from it is deleted, flagged, restored or answered (2026-09-25)

- **Authority:** approved round-3 recommendation 10: on return E9 is worked out again for the same source — a deleted result gone, a changed one at its new distance or no longer listed — and deleting a result opened from E9 returns to E9, the table showing only if the source itself is gone (about +8 words in R4.1); a case deletes FS-002 from E9 and one restores a result.
- **Decision:** As recommendation 10 states.
- **Carried by:** R4.1, UJ3.4-j, UJ3.4-k

### F145 — Approved round-3 recommendation 11: "Undo change" and the All items view (2026-09-25)

- **Authority:** approved round-3 recommendation 11: the All items view doesn't offer it, E13 unchanged; an edit made from it is undone in the item detail or on its collection's surface, R4.7's "offered only where its latest change's collection is shown" adding "never in the All items view" (+5 body words).
- **Decision:** As recommendation 11 states.
- **Carried by:** R4.7, UJ7.1-o

### F146 — Approved round-3 recommendation 12: Who owns the stored gamut flag's test parameters (2026-09-25)

- **Authority:** approved round-3 recommendation 12: the Data Foundation PRD — its R3.4 states the test itself (Bradford to the display white, relative colorimetric, zero tolerance, the adaptation its sRGB derivation uses) under DF F54, closing this PRD's OQ 7 changing DF R3.4 only through a DF fence and a new derivation version; OQ 7's Decision cell swaps its F40/F64 provenance sentence for that (word-neutral) and cites DF for the sRGB case; the ADR-0003 input follows.
- **Decision:** As recommendation 12 states.
- **Carried by:** the Data Foundation PRD R3.4, the Data Foundation PRD F54

### F147 — Approved round-3 recommendation 13: What an undoable ceiling delete holds (2026-09-25)

- **Authority:** approved round-3 recommendation 13: yes — the Data Foundation PRD's OQ 20 question adds "and what an undoable delete of ROWS_CEILING items holds in memory, or writes when the window ends, on OQ 1's Mac", in a dated line under DF F50 as IF-10's was; R1.7 stays gated and no row changes.
- **Decision:** As recommendation 13 states.
- **Carried by:** the Data Foundation PRD F50, the Data Foundation PRD F54

### F148 — Approved round-3 recommendation 14: HISTORY_READINGS_CEILING's closure evidence (2026-09-25)

- **Authority:** approved round-3 recommendation 14: yes — "OQ 1 — the owner's estimate of the longest history a real item reaches, timed by UJ9.5-e", as FILE_ITEMS_CEILING closes by estimate (+6 body words in the constants table); a dated line under F118.
- **Decision:** As recommendation 14 states.
- **Carried by:** R8.1b, UJ9.5-e

### F149 — Approved round-3 recommendation 15: An import commit while a session is in flight (2026-09-25)

- **Authority:** approved round-3 recommendation 15: an import commit also waits while any capture session is running — an Import PRD amendment in this change, for consistency with the one-writer rule (F100, F137).
- **Decision:** As recommendation 15 states.
- **Carried by:** the import PRD R3.2, the import PRD E40, the import PRD F66

### F150 — Approved round-3 recommendation 16: E34 in an owner reading once ADR-0007 lands (2026-09-25)

- **Authority:** approved round-3 recommendation 16: yes, as an input on the ADR-0007 row — revoke access the way the chosen model allows, follow E34 and confirm it recovers — decided with 4 (D); no row changes.
- **Decision:** As recommendation 16 states.
- **Carried by:** the Data Foundation PRD F54

Fences F151–F153 were decided by the owner on 2026-09-25 over the forks the round-3 fix pass surfaced; each answers one question, quoted with its chosen option.

### F151 — Choosing the file again restores access only (2026-09-25)

- **Authority:** owner decision D32, 2026-09-25. Question: "Data Foundation E34's new 'Choose the file again' action (F138): after the user re-chooses the file, does the app retry the change that wasn't saved?" Chosen: **No — then Try again** — "Choosing the file only restores access; E34 stays up and the user presses 'Try again' to redo the save. Nothing happens without the user's say-so." Not chosen: retrying automatically.
- **Decision:** As the chosen option states; it refines F138.
- **Carried by:** the Data Foundation PRD E34, the Data Foundation PRD R7.3j, the Data Foundation PRD R7.6p, the Data Foundation PRD DJ3, the Data Foundation PRD F54

**Clarified 2026-09-25 (owner decision D40, F159):** a different file chosen in that picker opens nothing, switches nothing and retries nothing. E34 stays up and the open file stays open.

### F152 — While a Collection Mode bulk write or delete runs, no other write anywhere in the app starts (2026-09-25)

- **Authority:** owner decision D33, 2026-09-25. Question: "F137 stops sessions and this PRD's other writes while a bulk write/delete runs. Other writers exist: an import commit, Data Foundation's re-read/move, Capture's 'New collection'. Do they also wait?" Chosen: **Everything waits, shown** — "While a Collection Mode bulk write or delete runs, no other write anywhere in the app starts — each is shown unavailable until it lands. One-line mirrors land in Import, Data Foundation and Capture in this PR." Not chosen: only what F137 names.
- **Decision:** As the chosen option states; it widens F137.
- **Carried by:** R8.1f, R8.1g, UJ9.5-g, the import PRD R3.2, the import PRD F66, the Data Foundation PRD R1.5, the Data Foundation PRD R1.9, the Data Foundation PRD DJ3, the Data Foundation PRD F54, the capture PRD R1.1, the capture PRD T7, the capture PRD F73

**Clarified 2026-09-25 (owner decisions D36 and D37, F155 and F156):** the rule becomes one Data Foundation rule. It also holds while an import commit or a file move runs, and closing, switching files and quitting wait for the write to land. Every sibling cites that rule instead of restating it.

### F153 — E34 says OK leaves the change unsaved (2026-09-25)

- **Authority:** owner decision D34, 2026-09-25. Question: "E34's 'OK' dismisses the message and leaves the change unsaved. Should the copy say so ('OK leaves it unsaved')?" Chosen: **Yes, say it** — "Add 'OK leaves it unsaved.' — the one DF 'OK' that discards a change says so." Not chosen: keeping F138's text exactly.
- **Decision:** As the chosen option states; it refines F138.
- **Carried by:** the Data Foundation PRD E34, the Data Foundation PRD R7.6p, the Data Foundation PRD F54

### F154 — The Data Foundation PRD's word budget is 8,200 (2026-09-25)

- **Authority:** owner decision D35, 2026-09-25. Question: "Data Foundation's body is 8,218 words against its 8,000 budget (its F21) after an editorial compaction removed ~700 words. The remaining excess is rule text you ratified in this PR (identity across code changes, the byte rule, E34, bulk-write holds). A further ~48 words can come from shortening sibling-cite labels. How do we close the rest?" Chosen: **Raise DF to 8,200** — "Shorten the cite labels (−48 → ~8,170) and raise DF's budget to 8,200 under a dated DF fence recording that the growth is owner-ratified rule text from this PR. Precedent: you raised DF's budget twice before (7,000→7,500→8,000). Note: the agent-PRD format's own rule says budgets are never raised — this is you overriding it for DF on the record." Not chosen: splitting the Data Foundation PRD; moving the Collection-Mode-driven rules into Collection Mode rows.
- **Decision:** As the chosen option states: the Data Foundation PRD's budget is 8,200, recorded there as its F55; this PRD's own 12,000-word budget is unchanged.
- **Why:** it sets a sibling document's budget, which carries no ID here.
- **Carried by:** governs no rows

**Clarified 2026-09-25 (owner decision D41, F160):** raised to 8,300.

Fences F155–F160 were decided by the owner on 2026-09-25 over the round-4 review's findings; each answers one question, quoted with its chosen option. F161–F172 record the owner's approval of round-4 recommendations 1–12. Recommendation 13's two declines are in Rejected findings.

### F155 — The one-writer rule is one Data Foundation rule (2026-09-25)

- **Authority:** owner decision D36, round-4 adjudication 2026-09-25. Question: "Your F152 says nothing else writes while a bulk write runs, but that rule is copied row by row into each PRD and misses writers: Capture's settings (\"editable at any time\"), \"Add a swatch\", ending an interrupted session, and a session starting while an import commits. How should it be fixed?" Chosen: **One Data Foundation rule** — "Data Foundation states it once: while a Collection Mode bulk write or delete, an import commit or a file move runs, no session starts or resumes and no other write to the file starts, each shown unavailable. Collection Mode, Import and Capture cite that rule instead of restating it. Where regeneration fits is added to Data Foundation's OQ 17." Not chosen: patching each PRD's copy of the rule.
- **Decision:** As the chosen option states. It widens F137 and F152: an import commit and a file move now also hold every other write to the file, and a session's start or resume. It moves their home to the Data Foundation PRD.
- **Carried by:** R8.1f, R8.1g, R8.10a, UJ9.5-g, the Data Foundation PRD R1.11, the Data Foundation PRD R1.3, the Data Foundation PRD R1.5, the Data Foundation PRD R1.9, the Data Foundation PRD R7.3j, the Data Foundation PRD DJ3, the Data Foundation PRD F56, the import PRD R3.2, the import PRD R3.8i, the import PRD F67, the capture PRD R1.1, the capture PRD R1.3, the capture PRD R1.5, the capture PRD R1.10, the capture PRD R3.5, the capture PRD R7.13, the capture PRD R8.5, the capture PRD R9.3, the capture PRD T7, the capture PRD F74

### F156 — Closing, switching files or quitting waits for a long write (2026-09-25)

- **Authority:** owner decision D37, round-4 adjudication 2026-09-25. Question: "While one of those long writes runs (a Collection Mode write lasts at most 10 s), what happens if the user closes the file, switches files or quits?" Chosen: **Wait, shown** — "Closing, switching or quitting waits for the write to finish, with its progress showing, then goes ahead. Nothing is lost and a confirmed delete stays confirmed." Not chosen: making them unavailable until the write is done; stopping the write and undoing it.
- **Decision:** As the chosen option states; it is part of F155's rule.
- **Carried by:** UJ9.5-g, the Data Foundation PRD R1.11, the Data Foundation PRD R1.3, the Data Foundation PRD DJ3, the Data Foundation PRD F56

**Clarified 2026-09-25 (owner decision D45, F176):** if the write fails while they wait, the close, switch or quit is cancelled and the failure state stays up.

### F157 — An edit shows saved; a wipe an outside read holds up waits, said (2026-09-25)

- **Authority:** owner decision D38, round-4 adjudication 2026-09-25. Question: "Another app reading your file (a SQL browser or a script) can stop SpectroCapture from wiping an edit's old text out of the file's bytes. Capture saves never wait, so the edit itself is saved. Only the wipe waits. F139 said \"shown not yet saved\", but nothing carries that, and quitting is undecided. What should the user see?" Chosen: **Saved, wipe pending** — "The change shows saved. A Data Foundation notice says the removed text stays in the file until the other app stops reading it, and the app then wipes it on its own, or when the file next opens. Closing and quitting never wait, and no other write waits. This amends F139 and narrows F102 to 'once nothing else is reading'." Not chosen: showing "not yet saved" as F139 said, with closing and quitting waiting for it.
- **Decision:** As the chosen option states. It amends F139 and narrows F102.
- **Carried by:** R8.8, R8.10a, UJ9.7-h, the Data Foundation PRD R6.2a, the Data Foundation PRD R2.3, the Data Foundation PRD R1.5, the Data Foundation PRD R7.6q, the Data Foundation PRD E35, the Data Foundation PRD DJ4, the Data Foundation PRD F56, the Data Foundation PRD F57

**Clarified 2026-09-25 (owner decisions D42 and D44, F173 and F175):** the wipe follows the end of the read, on its own or at the first open after it. E35 is one notice while any wipe waits on another app's read; OK hides it, a later removing write during that read shows it again, and it goes on its own once the wipe lands. Save a copy and a re-read's checks defer a wipe as an export does, with no notice.

### F158 — An export is one snapshot (2026-09-25)

- **Authority:** owner decision D39, round-4 adjudication 2026-09-25. Question: "Should an export be one snapshot? Right now the design has exports read the file in short slices so an edit's wipe isn't held up. The Export PRD never agreed to that." Chosen: **One snapshot** — "An export is the file exactly as it stood when you started it. An edit made while it runs saves at once, and its removed text is wiped when the export finishes. A matching line lands in the Export PRD." Not chosen: reading in short slices, each item whole.
- **Decision:** As the chosen option states. It amends F139's ADR-0003 input, which drops "an export".
- **Carried by:** UJ9.5-d, the Data Export PRD R1.1, the Data Export PRD EJ1, the Data Export PRD F32, the Data Foundation PRD R6.2a, the Data Foundation PRD F56

**Clarified 2026-09-25 (owner decision D44, F175):** Save a copy and a re-read's checks join the export as the app's own long reads that may defer a wipe until they end. Every other read the app makes stays within BROWSE_RESPONSE_BUDGET.

### F159 — Choosing another file in E34's picker changes nothing (2026-09-25)

- **Authority:** owner decision D40, round-4 adjudication 2026-09-25. Question: "Data Foundation E34's \"Choose the file again\" opens a file picker. If the user picks a different file there (say a copy), what happens?" Chosen: **Nothing changes** — "Only the same file restores access. Picking another file opens nothing, switches nothing and retries nothing. E34 stays up and the current file stays open, so the delete undo and \"Undo change\" survive." Not chosen: switching to it and dropping the unsaved change.
- **Decision:** As the chosen option states. It refines F151, whose "restores access only" the Data Foundation compaction dropped from that PRD's R7.3j; the fix pass restores it.
- **Carried by:** UJ9.7-i, the Data Foundation PRD R7.3j, the Data Foundation PRD R7.6p, the Data Foundation PRD DJ3, the Data Foundation PRD F54

**Clarified 2026-09-25 (owner decision D43, F174):** "the same file" is the open file wherever it now is, renamed or moved included. A copy or any other file changes nothing.

### F160 — The Data Foundation PRD's word budget is 8,300 (2026-09-25)

- **Authority:** owner decision D41, round-4 adjudication 2026-09-25. Question: "These decisions add about 100 words to the Data Foundation body, which has 36 left under the 8,200 you set (D35). The last trim deleted a rule by accident. Which way?" Chosen: **Raise DF to 8,300** — "Record a second budget override as a dated fence. No further trimming, so no risk of changing meaning." Not chosen: keeping 8,200 and trimming again.
- **Decision:** As the chosen option states: the Data Foundation PRD's budget is 8,300, recorded there by a dated line under its F55. This PRD's own 12,000-word budget is unchanged. The agent-PRD format's rule that a budget is never raised is overridden for that PRD, on the record, by the owner.
- **Why:** it sets a sibling document's budget, which carries no ID here.
- **Carried by:** governs no rows

**Clarified 2026-09-25 (owner decision D46, F177):** raised to 8,400.

### F161 — Approved round-4 recommendation 1: Which delete's undo R8.3 refuses in flight (2026-09-25)

- **Authority:** approved round-4 recommendation 1: R8.3's "E10's 'Undo' of a multi-item delete" becomes "of a selection or collection delete", as F136's question put it. So the undo of a delete of a one-item or empty collection is refused too. The capture PRD's obligations line follows it, and a case undoes a one-item collection's delete while a session is in flight, asserting E6's "elsewhere" variant.
- **Decision:** As recommendation 1 states; it clarifies F136.
- **Carried by:** R8.3, UJ1.3-h, the capture PRD F74

**Clarified 2026-09-25 (approved round-5 recommendation 2, F179):** E6's "elsewhere" variant names changes to a whole collection or to many swatches at once, so it fits an empty or one-item collection's undo.

### F162 — Approved round-4 recommendation 2: R8.10d and R8.10e are outside reads (2026-09-25)

- **Authority:** approved round-4 recommendation 2: the readbacks R8.10 keeps to test builds are all but R8.10d–f. R8.10d and R8.10e are reads made from outside the app, which UJ9.4-b and UJ9.4-d may make against a Release build.
- **Decision:** As recommendation 2 states; it clarifies F132.
- **Carried by:** R8.10

### F163 — Approved round-4 recommendation 3: A write the app makes by itself never ends "Undo change" (2026-09-25)

- **Authority:** approved round-4 recommendation 3: in R4.7, a write the app makes on its own never ends the undo history. An example is the Data Foundation PRD's damage mark, saved when the file is read. Only the user's own writes end it, as E8 says.
- **Decision:** As recommendation 3 states; it clarifies F135.
- **Carried by:** R4.7, UJ6.4-q

### F164 — Approved round-4 recommendation 4: Two test-build inputs (2026-09-25)

- **Authority:** approved round-4 recommendation 4: R8.10a gains two test-build inputs:
  - a bulk write or delete held running until released;
  - a read held open on the file from outside the app.

  UJ9.5-g's hold checks, and the sibling cases whose Givens declare R8.1f's hold, use the first. The F157 case uses the second.
- **Decision:** As recommendation 4 states.
- **Carried by:** R8.10a, UJ9.5-g, UJ9.7-h, the capture PRD T7, the Data Foundation PRD DJ3

**Clarified 2026-09-25 (approved round-5 recommendation 8, F185):** the held-write input stays in R8.10a for now; a post-lock item moves the grant into the Data Foundation PRD's R7.2 when that PRD has room.

### F165 — Approved round-4 recommendation 5: The Data Foundation PRD's R3.4 and R6.2a wording (2026-09-25)

- **Authority:** approved round-4 recommendation 5:
  - The Data Foundation PRD's R3.4 names "the adaptation its sRGB derivation uses", restoring what the compaction lost.
  - Its R6.2a leaves R5.8's recovery copies aside from the files the app keeps beside the file.
- **Decision:** As recommendation 5 states; the Data Foundation PRD records it under its F54 with a dated line.
- **Carried by:** the Data Foundation PRD R3.4, the Data Foundation PRD R6.2a, the Data Foundation PRD F54

### F166 — Approved round-4 recommendation 6: The seam lines match (2026-09-25)

- **Authority:** approved round-4 recommendation 6:
  - This PRD's Outbound Data Foundation row takes the Data Foundation PRD's compact form: E34's actions by its R7.3j, and the ADR inputs by the fences that list them.
  - The Data Foundation PRD's inbound Collection Mode line mends its "R2.9's Flag removal" phrase.
  - This PRD's Outbound Inventory Import row names F149's refusal of a commit while any session is in flight.
- **Decision:** As recommendation 6 states.
- **Carried by:** the Data Foundation PRD F56

### F167 — Approved round-4 recommendation 7: Where E10 and E6 render (2026-09-25)

- **Authority:** approved round-4 recommendation 7: E10's index row and Surfaces entry add the collection list; E6's add the collection list and the All items view.
- **Decision:** As recommendation 7 states.
- **Carried by:** E6, E10, UJ1.3-g, UJ1.3-h, UJ7.1-s

### F168 — Approved round-4 recommendation 8: Returning to E9 whose item is gone (2026-09-25)

- **Authority:** approved round-4 recommendation 8: in R4.1, closing a result's detail returns to E9, worked out again, only while E9's own item remains; otherwise the table shows. A case removes E9's item by reading the file again, then closes the result's detail.
- **Decision:** As recommendation 8 states; it clarifies F144.
- **Carried by:** R4.1, UJ3.4-l

### F169 — Approved round-4 recommendation 9: The import PRD's E40 another-collection variant (2026-09-25)

- **Authority:** approved round-4 recommendation 9: the import PRD's E40 another-collection variant drops "waits" and reads: "A session in ⟨collection⟩ is active, paused or halted. Until it ends, imports aren't available in any collection, so the session's saves aren't held up." Its shared append asks the user to start the import again. E40 moves to that PRD's ready-for-alignment status, under a dated Import fence.
- **Decision:** As recommendation 9 states.
- **Carried by:** the import PRD E40, the import PRD F67

### F170 — Approved round-4 recommendation 10: Copy wording (2026-09-25)

- **Authority:** approved round-4 recommendation 10:
  - E8's closing clause reads "— including importing, or hiding or showing a column; scanning doesn't count."
  - The Data Foundation PRD's E15 and E34 read "as it was before that".
  - E6's "full" condition names "Use this reading" among the actions it names.
- **Decision:** As recommendation 10 states.
- **Carried by:** E6, E8, the Data Foundation PRD E15, the Data Foundation PRD E34, the Data Foundation PRD F56

### F171 — Approved round-4 recommendation 11: ADR-0003's search input (2026-09-25)

- **Authority:** approved round-4 recommendation 11: the ADR-0003 search input adds R8.1c's budget to the budgets it names, and names the Mac its evidence was measured on (an M5 Max).
- **Decision:** As recommendation 11 states.
- **Carried by:** the Data Foundation PRD F53, the Data Foundation PRD F56

### F172 — Approved round-4 recommendation 12: What "Handoff" means in R8.6 (2026-09-25)

- **Authority:** approved round-4 recommendation 12: "Handoff" in R8.6 means the system handing an activity to another device. It does not mean the user's own copy and paste, Universal Clipboard included.
- **Decision:** As recommendation 12 states; it clarifies F130.
- **Carried by:** UJ9.4-d

Fences F173–F177 were decided by the owner on 2026-09-25 over the round-5 review's findings and the first lock-check run; each answers one question, quoted with its chosen option. F178–F188 record the owner's approval of round-5 recommendations 1–11.

### F173 — DF E35 is one notice that goes by itself (2026-09-25)

- **Authority:** owner decision D42, round-5 adjudication 2026-09-25. Question: "When another app's read holds up erasing text, E35 says so. How long does it stay up? If you keep a SQL browser open and rename 10 swatches, what should you see?" Chosen: **One notice, goes by itself** — "One E35 while any erase is waiting. OK hides it. A later edit while the other app still reads shows it again. It goes away on its own once the erase happens." Not chosen: staying until OK; one notice per edit.
- **Decision:** As the chosen option states; it refines F157.
- **Carried by:** UJ9.7-h, the Data Foundation PRD R6.2a, the Data Foundation PRD E35, the Data Foundation PRD DJ4, the Data Foundation PRD F57

### F174 — "The same file" in E34's picker is the open file, wherever it now is (2026-09-25)

- **Authority:** owner decision D43, round-5 adjudication 2026-09-25. Question: "In E34's \"Choose the file again\" picker (only the same file restores access), what counts as \"the same file\" if you renamed or moved it in Finder while it was open?" Chosen: **The open file, wherever it is** — "Picking your file under its new name or folder restores access. A copy or any other file still changes nothing." Not chosen: the same path only.
- **Decision:** As the chosen option states; it refines F159.
- **Carried by:** UJ9.7-i, the Data Foundation PRD R7.3j, the Data Foundation PRD DJ3, the Data Foundation PRD F54

### F175 — The app's own long reads defer a wipe as an export does (2026-09-25)

- **Authority:** owner decision D44, round-5 adjudication 2026-09-25. Question: "Other than an export, the app has reads that can run for seconds: \"Save a copy\" and the checks a re-read runs. An edit made while one is running can't have its old text erased until that read finishes. What's the rule?" Chosen: **Like an export** — "Old text is erased as soon as the copy or check finishes. No notice is shown, because the app started the read itself. Every other read the app makes stays short (under 100 ms)." Not chosen: every read but an export's always short.
- **Decision:** As the chosen option states; it refines F157 and F158, and restores the bound on every other read the app makes that the round-4 rewrite of the ADR-0003 input had narrowed to cold loads.
- **Carried by:** the Data Foundation PRD DJ4, the Data Foundation PRD F56, the Data Foundation PRD F57

### F176 — A held write that fails cancels the close, switch or quit (2026-09-25)

- **Authority:** owner decision D45, round-5 adjudication 2026-09-25. Question: "Closing, switching files or quitting waits for a long write (your F156). What if that write fails while they wait, say the disk fills during an import commit?" Chosen: **Stop and show it** — "The close, switch or quit is cancelled and the failure message stays on screen. You see what didn't save before you leave." Not chosen: going ahead anyway.
- **Decision:** As the chosen option states; it refines F156.
- **Carried by:** UJ9.5-g, the Data Foundation PRD R1.11, the Data Foundation PRD DJ3, the Data Foundation PRD F57

### F177 — The Data Foundation PRD's word budget is 8,400 (2026-09-25)

- **Authority:** owner decision D46, round-5 adjudication 2026-09-25. Question: "Data Foundation needs about 50 more words: your round-5 answers plus the seam lines the lock checks require. It has 8 left under 8,300. Which way?" Chosen: **Raise DF to 8,400** — "A third recorded override. It covers this round and gives the pre-lock round some headroom. No trimming, so no risk of changing meaning." Not chosen: trimming the Data Foundation PRD instead.
- **Decision:** As the chosen option states: the Data Foundation PRD's budget is 8,400, recorded there by a dated line under its F55. This PRD's 12,000-word budget is unchanged. The owner overrides the agent-PRD format's never-raise rule for that PRD, on the record.
- **Why:** it sets a sibling document's budget, which carries no ID here.
- **Carried by:** governs no rows

### F178 — Approved round-5 recommendation 1: E8's wording on what the file keeps (2026-09-25)

- **Authority:** approved round-5 recommendation 1: E8's "What was there isn't kept in your file." becomes "Your file won't keep what was there.", in its body and its "clear" variant, so it stays true while a wipe is deferred (F157) and agrees with the Data Foundation PRD's E35.
- **Decision:** As recommendation 1 states.
- **Carried by:** E8

### F179 — Approved round-5 recommendation 2: E6's "elsewhere" names whole-collection changes (2026-09-25)

- **Authority:** approved round-5 recommendation 2: E6's "elsewhere" variant's "changes that touch many swatches at once" becomes "changes to a whole collection or to many swatches at once".
- **Decision:** As recommendation 2 states; it clarifies F161.
- **Carried by:** E6

### F180 — Approved round-5 recommendation 3: DF E35's wording (2026-09-25)

- **Authority:** approved round-5 recommendation 3: the Data Foundation PRD's E35 "What it removed" becomes "What was there before", since a rename replaces text rather than removing it.
- **Decision:** As recommendation 3 states.
- **Carried by:** the Data Foundation PRD E35, the Data Foundation PRD F57

### F181 — Approved round-5 recommendation 4: E10's "collection" variant at zero swatches (2026-09-25)

- **Authority:** approved round-5 recommendation 4: E10's "collection" variant, for a collection holding no swatches, reads "⟨collection⟩ is deleted." in place of the sentence the zero rule drops.
- **Decision:** As recommendation 4 states.
- **Carried by:** E10, UJ1.3-h

### F182 — Approved round-5 recommendation 5: An app-made write during a hold is deferred (2026-09-25)

- **Authority:** approved round-5 recommendation 5: the Data Foundation PRD's R1.11 defers a write the app makes by itself during a hold, for example R5.5a's archive-unavailable mark; it waits until the hold ends and never contends for the file.
- **Decision:** As recommendation 5 states; it refines F155.
- **Carried by:** the Data Foundation PRD R1.11, the Data Foundation PRD DJ3, the Data Foundation PRD F57

### F183 — Approved round-5 recommendation 6: ADR-0003's non-waiting save is scoped to a local volume (2026-09-25)

- **Authority:** approved round-5 recommendation 6: the ADR-0003 input's "a text-removing write is saved without waiting on any read" is scoped to the Data Foundation PRD's R1.10 local-volume scope; the help-docs line for a network volume becomes a post-lock item.
- **Decision:** As recommendation 6 states; it refines F157.
- **Carried by:** the Data Foundation PRD F57

### F184 — Approved round-5 recommendation 7: A read that starts before a wipe also defers it (2026-09-25)

- **Authority:** approved round-5 recommendation 7: the Data Foundation PRD's R6.2a defers the wipe while any read begun before the wipe runs, not only a read begun before the write, as F157's "until the other app stops reading it" states.
- **Decision:** As recommendation 7 states; it clarifies F157.
- **Carried by:** the Data Foundation PRD R6.2a, the Data Foundation PRD DJ4, the Data Foundation PRD F57

### F185 — Approved round-5 recommendation 8: R8.10a's held-write grant stays here for now (2026-09-25)

- **Authority:** approved round-5 recommendation 8: R8.10a keeps its held-write input; a post-lock item moves the grant into the Data Foundation PRD's R7.2 when that PRD has room.
- **Decision:** As recommendation 8 states; it clarifies F164.
- **Carried by:** R8.10a

### F186 — Approved round-5 recommendation 9: Capture R8.14 moves to P0 (2026-09-25)

- **Authority:** approved round-5 recommendation 9: the capture PRD's R8.14 (the simulated, spread and non-spectral marks surfaced where the collection is browsed) moves from P1 to P0, because this PRD builds them at P0 under F1; lock check 1b.
- **Decision:** As recommendation 9 states; it follows F1.
- **Carried by:** the capture PRD R8.14, the capture PRD F75

### F187 — Approved round-5 recommendation 10: The Data Foundation PRD mirrors F57 (2026-09-25)

- **Authority:** approved round-5 recommendation 10: the Data Foundation PRD's R6.3 and its OQ 20 carry F57's deferral: if OQ 20 is still open at v1 release, delete-undo is deferred there too and v1's deletes are final; lock check 1b.
- **Decision:** As recommendation 10 states; it mirrors F57.
- **Carried by:** the Data Foundation PRD R6.3, the Data Foundation PRD F57

### F188 — Approved round-5 recommendation 11: The seam lines on both sides (2026-09-25)

- **Authority:** approved round-5 recommendation 11: the capture and import PRDs, whose format has no inbound-obligations table, carry what this PRD imposes on them by their rows' cites and dated fences, accepted in place of adding tables to two locked PRDs. The Data Foundation, capture, import and Data Export PRDs' outbound lines to Collection Mode name every row this PRD's inbound lines cite, the Data Export PRD gaining that line; lock check 1a.
- **Decision:** As recommendation 11 states.
- **Carried by:** the Data Foundation PRD F57, the capture PRD F75, the import PRD F68, the Data Export PRD F33

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
- **F53** — R8.6, R8.10e, UJ3.3-f, UJ6.4-j, UJ6.4-k, UJ7.1-r, UJ9.4-b, UJ10.1-d
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
- **F91** — governs no rows
- **F92** — R4.4, UJ4.3-h
- **F93** — R4.8, UJ4.6-i
- **F94** — R3.3, UJ7.1-l
- **F95** — the capture PRD E30, the capture PRD F71
- **F96** — R4.2b, UJ4.1-c, UJ4.7-a, the capture PRD F71
- **F97** — R4.9, E18, UJ4.7-a, UJ4.7-f
- **F98** — governs no rows
- **F99** — R8.1, R8.1f, R8.1g, UJ9.5-g
- **F100** — R1.4, R6.3, R8.3, E6, UJ1.2-f, UJ6.2-i, UJ6.2-j, UJ6.3-f, UJ8.1-j, UJ9.5-d, the capture PRD F72
- **F101** — R8.8, R8.10a, UJ9.7-g, the Data Foundation PRD R1.10, the Data Foundation PRD R7.3j, the Data Foundation PRD R7.6p, the Data Foundation PRD E34, the Data Foundation PRD F53
- **F102** — R1.3, R8.8, R8.10d, T1, T5, T6, UJ1.3-c, UJ4.2-a, UJ4.2-c, UJ4.4-b, UJ6.3-b, UJ9.8-c, the Data Foundation PRD R2.3, the Data Foundation PRD R6.2a, the Data Foundation PRD F53
- **F103** — R8.2, UJ9.5-b
- **F104** — R4.7, E8, UJ6.4-i, UJ6.4-l, UJ6.4-m, UJ6.4-n
- **F105** — R2.11, UJ4.1-a
- **F106** — M4
- **F107** — R4.1, UJ3.4-e
- **F108** — R4.8, UJ4.6-j
- **F109** — R1.10, R3.5, E4, UJ7.1-j, UJ7.1-m
- **F110** — R3.5, UJ3.2-d, UJ3.2-e
- **F111** — R5.3, R8.3, E6, E17, UJ5.3-n
- **F112** — R8.1c, UJ9.1-l
- **F113** — R3.3, UJ3.3-m, UJ7.1-n
- **F114** — R3.7, R3.8, R4.2c, E9, UJ3.4-g, UJ3.4-h
- **F115** — R5.4, UJ5.3-l
- **F116** — R5.5, UJ5.3-m
- **F117** — R6.2, UJ6.2-k
- **F118** — R8.1, R8.1b, UJ9.5-e
- **F119** — UJ9.5-a, UJ9.5-b
- **F120** — R8.11, UJ9.5-d
- **F121** — the Data Foundation PRD E11, the Data Foundation PRD E26, the Data Foundation PRD F53
- **F122** — the capture PRD R5.8, the capture PRD R6.6, the capture PRD F71
- **F123** — R3.7, UJ3.4-i
- **F124** — governs no rows
- **F125** — E12
- **F126** — governs no rows
- **F127** — UJ2.1-q, the Data Foundation PRD R3.4, the Data Foundation PRD F53
- **F128** — R8.1f, R8.1g, UJ9.5-g
- **F129** — R8.1, UJ9.5-a, UJ9.5-f
- **F130** — R8.6, R8.10e, UJ9.4-d
- **F131** — governs no rows
- **F132** — R8.10, UJ9.4-e
- **F133** — E19
- **F134** — M4
- **F135** — R4.7, E8, UJ6.4-m, UJ6.4-o
- **F136** — R8.3, E6, UJ1.3-g, UJ4.4-j, UJ6.3-g, UJ6.4-p, the capture PRD F73
- **F137** — R8.1f, R8.1g, UJ9.5-g, the capture PRD R3.5, the capture PRD T7, the capture PRD F73
- **F138** — the Data Foundation PRD E34, the Data Foundation PRD R7.3j, the Data Foundation PRD R7.6p, the Data Foundation PRD F54
- **F139** — R8.1, R8.1c, M1, UJ9.5-a, UJ9.5-d, the Data Foundation PRD R1.5, the Data Foundation PRD F54
- **F140** — R8.3, E6, UJ1.2-g, UJ1.2-h
- **F141** — R8.3, E6
- **F142** — R3.3, UJ7.1-p, UJ7.1-q
- **F143** — R6.2, UJ6.2-l
- **F144** — R4.1, UJ3.4-j, UJ3.4-k
- **F145** — R4.7, UJ7.1-o
- **F146** — the Data Foundation PRD R3.4, the Data Foundation PRD F54
- **F147** — the Data Foundation PRD F50, the Data Foundation PRD F54
- **F148** — R8.1b, UJ9.5-e
- **F149** — the import PRD R3.2, the import PRD E40, the import PRD F66
- **F150** — the Data Foundation PRD F54
- **F151** — the Data Foundation PRD E34, the Data Foundation PRD R7.3j, the Data Foundation PRD R7.6p, the Data Foundation PRD DJ3, the Data Foundation PRD F54
- **F152** — R8.1f, R8.1g, UJ9.5-g, the import PRD R3.2, the import PRD F66, the Data Foundation PRD R1.5, the Data Foundation PRD R1.9, the Data Foundation PRD DJ3, the Data Foundation PRD F54, the capture PRD R1.1, the capture PRD T7, the capture PRD F73
- **F153** — the Data Foundation PRD E34, the Data Foundation PRD R7.6p, the Data Foundation PRD F54
- **F154** — governs no rows
- **F155** — R8.1f, R8.1g, R8.10a, UJ9.5-g, the Data Foundation PRD R1.11, the Data Foundation PRD R1.3, the Data Foundation PRD R1.5, the Data Foundation PRD R1.9, the Data Foundation PRD R7.3j, the Data Foundation PRD DJ3, the Data Foundation PRD F56, the import PRD R3.2, the import PRD R3.8i, the import PRD F67, the capture PRD R1.1, the capture PRD R1.3, the capture PRD R1.5, the capture PRD R1.10, the capture PRD R3.5, the capture PRD R7.13, the capture PRD R8.5, the capture PRD R9.3, the capture PRD T7, the capture PRD F74
- **F156** — UJ9.5-g, the Data Foundation PRD R1.11, the Data Foundation PRD R1.3, the Data Foundation PRD DJ3, the Data Foundation PRD F56
- **F157** — R8.8, R8.10a, UJ9.7-h, the Data Foundation PRD R6.2a, the Data Foundation PRD R2.3, the Data Foundation PRD R1.5, the Data Foundation PRD R7.6q, the Data Foundation PRD E35, the Data Foundation PRD DJ4, the Data Foundation PRD F56, the Data Foundation PRD F57
- **F158** — UJ9.5-d, the Data Export PRD R1.1, the Data Export PRD EJ1, the Data Export PRD F32, the Data Foundation PRD R6.2a, the Data Foundation PRD F56
- **F159** — UJ9.7-i, the Data Foundation PRD R7.3j, the Data Foundation PRD R7.6p, the Data Foundation PRD DJ3, the Data Foundation PRD F54
- **F160** — governs no rows
- **F161** — R8.3, UJ1.3-h, the capture PRD F74
- **F162** — R8.10
- **F163** — R4.7, UJ6.4-q
- **F164** — R8.10a, UJ9.5-g, UJ9.7-h, the capture PRD T7, the Data Foundation PRD DJ3
- **F165** — the Data Foundation PRD R3.4, the Data Foundation PRD R6.2a, the Data Foundation PRD F54
- **F166** — the Data Foundation PRD F56
- **F167** — E6, E10, UJ1.3-g, UJ1.3-h, UJ7.1-s
- **F168** — R4.1, UJ3.4-l
- **F169** — the import PRD E40, the import PRD F67
- **F170** — E6, E8, the Data Foundation PRD E15, the Data Foundation PRD E34, the Data Foundation PRD F56
- **F171** — the Data Foundation PRD F53, the Data Foundation PRD F56
- **F172** — UJ9.4-d
- **F173** — UJ9.7-h, the Data Foundation PRD R6.2a, the Data Foundation PRD E35, the Data Foundation PRD DJ4, the Data Foundation PRD F57
- **F174** — UJ9.7-i, the Data Foundation PRD R7.3j, the Data Foundation PRD DJ3, the Data Foundation PRD F54
- **F175** — the Data Foundation PRD DJ4, the Data Foundation PRD F56, the Data Foundation PRD F57
- **F176** — UJ9.5-g, the Data Foundation PRD R1.11, the Data Foundation PRD DJ3, the Data Foundation PRD F57
- **F177** — governs no rows
- **F178** — E8
- **F179** — E6
- **F180** — the Data Foundation PRD E35, the Data Foundation PRD F57
- **F181** — E10, UJ1.3-h
- **F182** — the Data Foundation PRD R1.11, the Data Foundation PRD DJ3, the Data Foundation PRD F57
- **F183** — the Data Foundation PRD F57
- **F184** — the Data Foundation PRD R6.2a, the Data Foundation PRD DJ4, the Data Foundation PRD F57
- **F185** — R8.10a
- **F186** — the capture PRD R8.14, the capture PRD F75
- **F187** — the Data Foundation PRD R6.3, the Data Foundation PRD F57
- **F188** — the Data Foundation PRD F57, the capture PRD F75, the import PRD F68, the Data Export PRD F33

## Rejected findings
<!-- guidance: every reviewer finding the owner rejected, with the same authority-by-link
     discipline as a fence. A rejection recorded here is settled: it is carried into every later
     brief so the finding is never resurfaced round after round. Not conditional — a file with no
     rejections keeps the heading and says "none". -->

| Finding | Raised by | Disposition | Authority |
|---|---|---|---|
| Round 1 Finding-2 (Blocker): "Set a field" could set Swatch Code on a selection and create duplicate codes (R6.2) | peer-staff-software-engineer-reviewer, cross-model on agy | Not reproduced: the Vocabulary's editable field excludes Swatch Code, which only R4.4 changes (F7); not fixed | The review log's Round 1 disposition row 5 |
| Round 4 N4-n2 (Nit): a trailing clause on E6 for when an interrupted session is also held | peer-product-marketing-manager-reviewer | Declined: the case is rare, and E6 stays short; not fixed | Approved round-4 recommendation 13 |
| Round 4 PERF4-7 (Nit): hold other writes only while the bulk write's progress shows | peer-performance-reviewer | Declined: it would let a session start in a bulk write's first BROWSE_RESPONSE_BUDGET, the contention F137 stops; not fixed | Approved round-4 recommendation 13 |
