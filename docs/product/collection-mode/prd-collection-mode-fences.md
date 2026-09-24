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

## Rejected findings
<!-- guidance: every reviewer finding the owner rejected, with the same authority-by-link
     discipline as a fence. A rejection recorded here is settled: it is carried into every later
     brief so the finding is never resurfaced round after round. Not conditional — a file with no
     rejections keeps the heading and says "none". -->

None — no review round has run yet.
