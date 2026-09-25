<!-- guidance: this is the copy companion to `PRD: Collection Mode`. It is the third of the
     format's four homes: copy owns displayed text. Every user-facing string this product area
     renders is written here once; rows, acceptance cases and sibling documents quote it and never
     restate it, so a string never drifts between two homes.
     Two fill families are placeholders: `{{...}}`, substituted at scaffold time, and `_(prompt)_`,
     filled during authoring; an unresolved instance of EITHER is a lock-blocking defect. Every
     guidance comment in this file is deleted at lock, so a rule a BUILDER needs after lock lives
     in body text and never in a comment — the phase marks and the render-token rule below are
     body text for exactly that reason. The converse holds too: how an entry is WRITTEN and how it
     is CHECKED are an author's business, so they stay in these comments and out of the locked
     document. -->

# Copy: Collection Mode
<!-- guidance: Collection Mode matches the PRD's title exactly, so the set reads as one document;
     the file itself is `prd-collection-mode-copy.md` beside the PRD in docs/product/collection-mode.
     docs/product/collection-mode: the project's product-docs directory, where the PRD and its four
     companions live. -->

Copy owns displayed text. Every string this product area renders is written once below, under the
`E<n>` that owns it; `docs/product/collection-mode/prd-collection-mode.md` indexes these states, the surface each
appears in and the rows that cause them, and every action a row or an acceptance case names is
quoted from here character for character.

## Entry grammar
<!-- guidance: how an entry is written — authoring mechanics, deleted at lock. Each state's
     section is a list of fields, one field per line, each written as `- <Field>: <value>` with the
     field name unemphasized, so an extraction keys on the literal `<Field>: ` prefix. The fields,
     in this order:
     - `Status:` — one of the six status values the PRD's Legend defines. It must equal the
       `Status` cell this `E<n>` carries in the PRD's copy index; the two are reconciled at every
       lock.
     - `Phase:` — this state's phase mark, or `none`.
     - `Variants enumerated by:` — the ID of the requirement row that enumerates this state's
       variant set, or `none` where the state has no variants. A variant set no row enumerates is
       untestable without matching on wording.
     - `Headline:` — the headline as it renders.
     - `Body:` — the body as it renders when no variant applies.
     - `Actions:` — the action labels this state offers, in render order, each in double quotes
       and separated by commas, or `none`.
     - `Variant:` — one line per variant, and none where the state has none, written
       `- Variant: "<name>" — <the condition it renders under>. <the body it renders.>`
     Quote marks are load-bearing for the checks as well as for the reader: a double-quoted string
     in this file is a checkable label — an action label, or a variant's enumerated name — and
     nothing else in this file is quoted, so the mechanical checks' label search cannot collide
     with the prose around it. `Headline:`, `Body:` and the condition and body text on a
     `Variant:` line are prose and carry no quote marks. -->

One `### E<n> — <state>` section per copy state, in the PRD copy index's order. Those headings are
stable anchors — the PRD, the acceptance cases and the fences cite them — so a state is never
renumbered or retitled once cited, and a retired state keeps its number rather than freeing it.

Each state's `Headline:` and `Body:` render as written, the `Body:` being what renders when no
variant applies; its `Actions:` are the action labels the state offers, in render order; each
`Variant:` line gives the name of one variant, the condition it renders under, and the body it
renders instead. `Variants enumerated by:` names the requirement row that enumerates this state's
variant set. A label in double quotes renders without those quote marks — they mark it as a label
rather than prose — and a variant is identified by its quoted name, never by its wording.

**Render tokens are not placeholders.** A token the product substitutes at render time — a count, a
⟨code⟩ — is named in the PRD copy index's Placeholders rule, is written inside the string it
renders in, and survives lock. It never takes the form of either fill family — the doubled-brace
scaffold parameters or the underscore-parenthesis authoring prompts — so the unresolved-fill sweep
cannot confuse the two.
<!-- guidance: the two fill families are written out in this template's head comment, which is
     deleted at lock; the sentence above names them without reproducing them, because a locked
     document that spelled either one out would fail the unresolved-fill sweep on the very line
     explaining it. -->

## Phase marks

A phase mark says that something this file describes is not present in the build yet, and which
kind of absence it is. These are the three kinds the PRD's Legend phase rule defines, written out;
there are exactly three, and no other phase mark is legal:

- `[phase: surface-absent]` — the surface this state belongs to does not exist yet.
- `[phase: action-absent]` — the surface exists, and an action it will offer is not present yet.
- `[phase: variant-absent]` — the variant is not produced yet.

A mark on the `Phase:` field applies to the whole state; a mark written after an action's closing
quote applies to that action; a mark written at the end of a `Variant:` line applies to that
variant. A state, surface or action marked here carries the same mark in the PRD, and the two are
reconciled at every lock. A builder renders a state whose action is marked `[phase: action-absent]`
without that action, and the state is complete without it.

## States
<!-- guidance: one section per copy state, `### E<n> — <state>`, numbered per PRD. Every state here
     appears in the PRD's copy index, under exactly one surface unless the PRD's Surfaces preamble
     declares it shared, and has at least one acceptance case in the journeys companion. A state no
     requirement row causes is a latent decision, not copy: raise it as an open question rather
     than writing text for it. Copy honesty (process rule 5): a string promises only what a row
     delivers. Not conditional — a product area with no user-facing surface deletes the sections
     and says "none" here, rather than leaving the file empty. -->

Sibling-owned states that render on this product area's surfaces are their owners' and are not
restated here: the capture PRD's E1, E2, E3, E23, E24, E25, E28, E33 and E34; the device PRD's E22;
the Data Foundation PRD's E4, E8, E9, E11, E14, E15, E26, E31 and E33; and the Data Export PRD's E1.

### E1 — No collections yet

- Status: pre-alignment
- Phase: none
- Variants enumerated by: none
- Headline: No collections yet
- Body: A collection holds the swatches you import from a spreadsheet or scan. Start one, then bring your swatch list into it.
- Actions: "New collection"

### E2 — Collections

- Status: pre-alignment
- Phase: none
- Variants enumerated by: none
- Headline: Collections
- Body: ⟨n⟩ collections in this file.
- Actions: "New collection", "All items" [phase: action-absent], "Answer re-scans"

### E3 — Collection shown

- Status: pre-alignment
- Phase: none
- Variants enumerated by: R3.4
- Headline: ⟨collection⟩
- Body: ⟨n⟩ swatches.
- Actions: "Filters", "Colour marks", "Rename collection", "Delete collection", "Export collection", "Delete swatch", "Select all" [phase: action-absent], "Set a field" [phase: action-absent], "Delete selected" [phase: action-absent], "Use as scan order" [phase: action-absent], "Rename column" [phase: action-absent], "Grid" [phase: action-absent], "Table" [phase: action-absent]
- Variant: "narrowed" — a search or a filter is active. ⟨shown⟩ of ⟨n⟩ swatches match.

### E4 — No search matches

- Status: pre-alignment
- Phase: none
- Variants enumerated by: none
- Headline: Nothing matches ⟨text⟩
- Body: The search looks at the start of each swatch's code and alternate code, and anywhere in its name, its alternate name and the details you imported. Spacing and capitals don't count as a difference.
- Actions: "Clear search"

### E5 — Nothing passes the filters

- Status: pre-alignment
- Phase: none
- Variants enumerated by: none
- Headline: No swatch passes these filters
- Body: ⟨n⟩ swatches are hidden by the filters you've chosen. Clearing the filters keeps your search.
- Actions: "Clear filters"

### E6 — Waiting for a session to end

- Status: pre-alignment
- Phase: none
- Variants enumerated by: R1.4
- Headline: ⟨collection⟩ has a session that hasn't ended
- Body: Scanning in ⟨collection⟩ is under way, paused or held. Deleting swatches or the collection, changing a swatch's code, and flagging a swatch wait until you end that session, so nothing it's working on moves under it. Nothing has been changed.
- Actions: "Go to the session", "Cancel"
- Variant: "interrupted" — the collection holds an interrupted session and the action is deleting the collection. A session in ⟨collection⟩ was interrupted and hasn't been resumed or ended. Resume it or end it before deleting the collection. Nothing has been changed.

### E7 — Collection name needed

- Status: pre-alignment
- Phase: none
- Variants enumerated by: none
- Headline: A collection needs a name
- Body: The name can't be blank, so ⟨collection⟩ keeps its name.
- Actions: "Change the name"

### E8 — Change a field on many swatches

- Status: pre-alignment
- Phase: none
- Variants enumerated by: R6.2
- Headline: Change ⟨column⟩ on ⟨n⟩ swatches?
- Body: Every selected swatch in ⟨collection⟩ gets ⟨value⟩ in ⟨column⟩, replacing what each has there now. Nothing else changes, and their readings stay as they are.
- Actions: "Apply to ⟨n⟩ swatches", "Cancel"
- Variant: "clear" — the change empties the field. Every selected swatch in ⟨collection⟩ has ⟨column⟩ emptied, and what was there is no longer kept in your file. Nothing else changes, and their readings stay as they are.

### E9 — Colours close to a swatch

- Status: pre-alignment
- Phase: none
- Variants enumerated by: R3.8
- Headline: Colours close to ⟨code⟩
- Body: ⟨n⟩ swatches in ⟨scope⟩ are within ΔE2000 ⟨distance⟩ of ⟨code⟩, nearest first. ⟨excluded⟩ swatches weren't compared: they have no current colour, or theirs was worked out under a different light or measurement condition. Only your own swatches are compared — never a named colour library.
- Actions: "Done"
- Variant: "none" — no other swatch is at or within the distance. No other swatch in ⟨scope⟩ is within ΔE2000 ⟨distance⟩ of ⟨code⟩. ⟨excluded⟩ swatches weren't compared: they have no current colour, or theirs was worked out under a different light or measurement condition.

### E10 — Deleted, undo available

- Status: pre-alignment
- Phase: none
- Variants enumerated by: R1.7
- Headline: Deleted
- Body: ⟨code⟩ is deleted. You can undo it while this file stays open; closing the file, quitting, or an app crash makes it final.
- Actions: "Undo"
- Variant: "swatches" — a selection was deleted. ⟨n⟩ swatches are deleted. You can undo it while this file stays open; closing the file, quitting, or an app crash makes it final.
- Variant: "collection" — a collection was deleted. ⟨collection⟩ and its ⟨n⟩ swatches are deleted. You can undo it while this file stays open; closing the file, quitting, or an app crash makes it final.

### E11 — Column name not accepted

- Status: pre-alignment
- Phase: none
- Variants enumerated by: R4.8
- Headline: That column name can't be used
- Body: A column needs a name, so ⟨column⟩ keeps its name.
- Actions: "Change the name"
- Variant: "duplicate" — the name is already another column's or a swatch field's. ⟨text⟩ is already the name of a column or a swatch field here — spacing and capitals don't count as a difference — so ⟨column⟩ keeps its name.

### E12 — What the marks mean

- Status: pre-alignment
- Phase: none
- Variants enumerated by: none
- Headline: What the marks mean
- Body: Can't show on this screen: this display can't render the colour, so the chip shows what the screen can manage, not the colour itself. Outside sRGB: the colour falls outside standard sRGB, the reference your exports and your file use. Simulated: the reading came from the Demo Device, not an instrument. No spectral data: the reading has colour values but not the curve behind them, so it can't be worked out again under another light. Samples disagreed: its samples came out further apart than expected and you accepted their average. Value missing: there's no value for the measurement condition this collection is set to. No current value: the swatch hasn't been scanned, or it's set aside. Unreadable: its current reading can't be read, so the swatch is set aside. Re-scan to answer: it was scanned again and you haven't said whether the swatch changed or the old reading was wrong. Never right: you said this earlier reading was wrong, so it's left out of anything showing change over time. Not settled: a later re-scan is waiting for your answer, so this earlier reading's standing isn't settled yet.
- Actions: "Done"

### E13 — All items shown

- Status: pre-alignment
- Phase: [phase: surface-absent]
- Variants enumerated by: R3.4
- Headline: All items
- Body: ⟨n⟩ swatches across ⟨collections⟩ collections.
- Actions: "Filters", "Colour marks", "Answer re-scans"
- Variant: "narrowed" — a search or a filter is active. ⟨shown⟩ of ⟨n⟩ swatches across ⟨collections⟩ collections match.

### E14 — Swatch detail

- Status: pre-alignment
- Phase: none
- Variants enumerated by: none
- Headline: ⟨code⟩ ⟨name⟩
- Body: A change saves when you press Return or leave the field; Escape puts back what was there before you started typing.
- Actions: "Show history", "Export swatch", "Delete swatch", "Find similar" [phase: action-absent], "Change code" [phase: action-absent]

### E15 — Code not accepted

- Status: pre-alignment
- Phase: none
- Variants enumerated by: R4.4
- Headline: That code can't be used
- Body: A swatch needs a code — it's how you and your imports find it — so ⟨code⟩ keeps its code.
- Actions: "OK"
- Variant: "duplicate" — the code is already another swatch's in the collection. ⟨text⟩ is already the code of another swatch in ⟨collection⟩ — spacing and capitals don't count as a difference — so ⟨code⟩ keeps its code.

### E16 — Change a swatch's code

- Status: pre-alignment
- Phase: none
- Variants enumerated by: none
- Headline: Change ⟨code⟩ to ⟨text⟩?
- Body: Imports find swatches by their code, so a spreadsheet that still lists ⟨code⟩ will add it as a new swatch instead of updating this one. The swatch keeps its readings and history.
- Actions: "Change the code", "Keep this code"

### E17 — History

- Status: pre-alignment
- Phase: none
- Variants enumerated by: R5.3
- Headline: ⟨n⟩ readings of ⟨code⟩
- Body: Every reading this swatch has had, the most recently saved first. None can be removed on its own: deleting the swatch or its collection is the only thing that throws a reading away.
- Actions: "Recorded order", "Measured order", "Use this reading" [phase: action-absent], "Compare" [phase: action-absent]
- Variant: "measured" — readings are in measurement-time order. The readings in the order they were measured. ⟨left⟩ readings marked never right are left out of this order, and a reading still waiting on a re-scan answer is shown and marked.
