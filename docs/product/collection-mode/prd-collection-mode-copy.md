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
restated here: the capture PRD's E1, E2, E3, E23, E24, E25, E26, E28, E33 and E34; the device PRD's
E22; the Data Foundation PRD's E4, E8, E9, E10, E11, E14, E15, E26, E31 and E33 and its read-only
file states; and the Data Export PRD's E1.

### E1 — No collections yet

- Status: aligned
- Phase: none
- Variants enumerated by: none
- Headline: No collections yet
- Body: A collection holds the swatches you import from a spreadsheet or scan. Start one, then bring your swatch list into it.
- Actions: "New collection"

### E2 — Collections

- Status: aligned
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
- Body: ⟨n⟩ swatches. ⟨unlike⟩ swatches whose colour was worked out for a different light or viewing angle come after the rest in this order.
- Actions: "Filters", "Clear search", "Clear filters", "Colour marks", "Columns" [phase: action-absent], "Rename collection", "Delete collection", "Export collection", "Delete swatch", "Undo change" [phase: action-absent], "Select all" [phase: action-absent], "Set a field" [phase: action-absent], "Delete selected" [phase: action-absent], "Use as scan order" [phase: action-absent], "Rename column" [phase: action-absent], "Grid" [phase: action-absent], "Table" [phase: action-absent]
- Variant: "narrowed" — a search or a filter is active. ⟨shown⟩ of ⟨n⟩ swatches match. ⟨unlike⟩ swatches whose colour was worked out for a different light or viewing angle come after the rest in this order.

### E4 — No search matches

- Status: pre-alignment
- Phase: none
- Variants enumerated by: R3.5
- Headline: Nothing matches ⟨text⟩
- Body: The search looks at the start of each swatch's code and alternate code, and anywhere in its name, its alternate name and the details you imported. Capitals and extra spaces don't count as a difference.
- Actions: "Clear search"
- Variant: "all items" — the search is in the All items view. The search looks at the start of each swatch's code and alternate code, and anywhere in its name, its alternate name, the details you imported and its collection's name. Capitals and extra spaces don't count as a difference.

### E5 — Nothing passes the filters

- Status: needs-discussion
- Phase: none
- Variants enumerated by: none
- Headline: No swatch matches these filters
- Body: ⟨n⟩ swatches are hidden by the filters you've chosen. Clearing the filters keeps your search.
- Actions: "Clear filters"

### E6 — Waiting for a session to end

- Status: pre-alignment
- Phase: none
- Variants enumerated by: R8.3
- Headline: ⟨collection⟩ has a session that hasn't ended
- Body: Scanning in ⟨collection⟩ is active, paused or halted. Deleting a swatch or the collection and bringing back an earlier reading aren't available until you end that session, so nothing it's working on moves under it. Nothing has been changed — do it again once the session has ended.
- Actions: "Go to the session", "Cancel"
- Variant: "interrupted" — the collection holds an interrupted session and the action is deleting the collection. A session in ⟨collection⟩ was interrupted and hasn't been resumed or ended. Resume it or end it before deleting the collection. Nothing has been changed.
- Variant: "full" — every P1 action R8.3 names is built, and the session is on the collection the action is on. Scanning in ⟨collection⟩ is active, paused or halted. Deleting swatches or the collection, changing a field on many swatches, changing a swatch's code or undoing that change, flagging a swatch, bringing back an earlier reading and undoing a delete aren't available until you end that session, so nothing it's working on moves under it. Nothing has been changed — do it again once the session has ended. [phase: variant-absent]
- Variant: "elsewhere" — the session is on a collection other than the one the action is on, ⟨collection⟩ naming the collection with the session. Scanning in ⟨collection⟩ is active, paused or halted. While any session is running, a change that touches many swatches at once waits until it ends, so the session's saves aren't held up. Nothing has been changed — do it again once the session has ended.

### E7 — Collection name needed

- Status: aligned
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
- Body: Every selected swatch in ⟨collection⟩ gets ⟨value⟩ in ⟨column⟩, replacing what each has there now. Nothing else changes, and their readings stay as they are. What was there isn't kept in your file. Undo change puts it back until the file closes, you read the file again, or you change anything other than swatch details, codes or names — scanning doesn't count.
- Actions: "Apply to ⟨n⟩ swatches", "Cancel"
- Variant: "clear" — the change empties the field. Every selected swatch in ⟨collection⟩ has ⟨column⟩ emptied. Nothing else changes, and their readings stay as they are. What was there isn't kept in your file. Undo change puts it back until the file closes, you read the file again, or you change anything other than swatch details, codes or names — scanning doesn't count.

### E9 — Colours close to a swatch

- Status: pre-alignment
- Phase: none
- Variants enumerated by: R3.8
- Headline: Colours close to ⟨code⟩ (⟨collection⟩)
- Body: ⟨n⟩ swatches in ⟨scope⟩ are within ΔE2000 ⟨distance⟩ of ⟨code⟩, nearest first; open one to see it. ⟨excluded⟩ swatches weren't compared: they have no colour in their own collection's measurement condition, or theirs was worked out under a different light or measurement condition. Only your own swatches are compared — never a named colour library.
- Actions: "Close"
- Variant: "none" — no other swatch is at or within the distance. No other swatch in ⟨scope⟩ is within ΔE2000 ⟨distance⟩ of ⟨code⟩. ⟨excluded⟩ swatches weren't compared: they have no colour in their own collection's measurement condition, or theirs was worked out under a different light or measurement condition.

### E10 — Deleted, undo available

E10 does not render until R1.7 is built, which OQ 10 holds back (F57); it carries no phase mark of its own.

- Status: pre-alignment
- Phase: none
- Variants enumerated by: R1.7
- Headline: Deleted
- Body: ⟨code⟩ is deleted from ⟨collection⟩. You can undo it while this file stays open; closing the file, quitting, or an app crash makes it final.
- Actions: "Undo"
- Variant: "swatches" — a selection was deleted. ⟨n⟩ swatches are deleted from ⟨collection⟩. You can undo it while this file stays open; closing the file, quitting, or an app crash makes it final.
- Variant: "collection" — a collection was deleted. ⟨collection⟩ and its ⟨n⟩ swatches are deleted. You can undo it while this file stays open; closing the file, quitting, or an app crash makes it final.

### E11 — Column name not accepted

- Status: needs-discussion
- Phase: none
- Variants enumerated by: R4.8
- Headline: That column name can't be used
- Body: A column needs a name, so ⟨column⟩ keeps its name.
- Actions: "Change the name"
- Variant: "duplicate" — the name is already another column's or a swatch field's. ⟨text⟩ is already the name of a column or a swatch field here — capitals and extra spaces don't count as a difference — so ⟨column⟩ keeps its name.

### E12 — What the marks mean

Spread's definition comes first; then each mark shows its shape (R8.9) before its name, and the marks
are grouped under the Mark labels table's group headings, in that table's order.

- Status: pre-alignment
- Phase: none
- Variants enumerated by: none
- Headline: What the marks mean
- Body: Spread: the largest difference, in ΔE2000, between any one of a reading's samples and their average. Colour beyond a limit. Can't show: this display can't render the colour, so the chip shows the nearest colour it can, not the colour itself. Outside sRGB: the colour is more saturated than standard sRGB can hold, so its sRGB and HSL values — here, in your file and in exports — are the nearest sRGB colour; its Lab, XYZ and spectral values are unaffected. Without Can't show beside it, the chip is still the true colour. Where the reading came from. Simulated: the reading came from the Demo Device, not an instrument. No spectral data: the reading has colour values but not the curve behind them, so it can't be worked out again under another light. Samples disagreed: its samples came out further apart than expected and you accepted their average. A swatch you set aside instead has no colour to show, and its State line gives samples disagreed as the reason. No colour to show. Not in this condition: it has a reading, but none under the measurement condition this collection is set to. No current value: the swatch hasn't been scanned, or it's set aside for a reason other than an unreadable reading. Unreadable: the reading saved in your file is damaged and can't be read, so the swatch is set aside to scan again or to go back to an earlier reading. Waiting on you. Re-scan unanswered: it was scanned again and you haven't said whether the swatch changed or the old reading was wrong; answer it from its swatch detail, or with Answer re-scans. Earlier readings. Never right: you said this earlier reading was wrong, so it's left out of anything showing change over time. Awaiting answer: a later re-scan is waiting for your answer, so whether this earlier reading still stands isn't known yet.
- Actions: "Close"

### E13 — All items shown

- Status: pre-alignment
- Phase: [phase: surface-absent]
- Variants enumerated by: R3.4
- Headline: All items
- Body: Every swatch in your ⟨collections⟩ collections, in one list. ⟨n⟩ swatches in all. ⟨unlike⟩ swatches whose colour was worked out for a different light or viewing angle come after the rest in this order.
- Actions: "Filters", "Clear search", "Clear filters", "Colour marks"
- Variant: "narrowed" — a search or a filter is active. ⟨shown⟩ of ⟨n⟩ swatches across ⟨collections⟩ collections match. ⟨unlike⟩ swatches whose colour was worked out for a different light or viewing angle come after the rest in this order.

### E14 — Swatch detail

- Status: aligned
- Phase: none
- Variants enumerated by: R8.4
- Headline: ⟨code⟩ ⟨name⟩
- Body: A change saves when you press Return or leave the field; Escape puts back what was there before you started typing.
- Actions: "Show history", "Colour marks", "Export swatch", "Delete swatch", "Find similar" [phase: action-absent], "Change code" [phase: action-absent], "Undo change" [phase: action-absent]
- Variant: "read-only" — the file is open read-only. Your file is open read-only, so nothing here can be changed.

### E15 — Code not accepted

- Status: aligned
- Phase: none
- Variants enumerated by: R4.4
- Headline: That code can't be used
- Body: A swatch needs a code — it's how you and your imports find it — so ⟨code⟩ keeps its code.
- Actions: "Try another code"
- Variant: "duplicate" — the code is already another swatch's in the collection. ⟨text⟩ is already the code of another swatch in ⟨collection⟩ — capitals and extra spaces don't count as a difference — so ⟨code⟩ keeps its code.

### E16 — Change a swatch's code

- Status: aligned
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
- Body: Every reading this swatch has had, the most recently recorded first. None can be removed on its own: deleting the swatch or its collection is the only thing that throws a reading away.
- Actions: "Recorded order", "Measured order", "Colour marks", "Use this reading" [phase: action-absent], "Compare" [phase: action-absent]
- Variant: "restore" — R5.5 is built and readings are in recorded order. Every reading this swatch has had, the most recently recorded first. None can be removed on its own: deleting the swatch or its collection is the only thing that throws a reading away. Use this reading, where it's offered, makes an earlier reading the current one again, and the reading it replaces stays in the history. [phase: variant-absent]
- Variant: "measured" — readings are in measurement-time order. The readings in the order they were measured, oldest first. ⟨left⟩ readings marked never right are left out of this order, and a reading still awaiting an answer is shown and marked.

### E18 — Set a swatch aside to scan again

- Status: aligned
- Phase: none
- Variants enumerated by: R4.9
- Headline: Set ⟨code⟩ aside to scan again?
- Body: Its current reading moves to its history, and the swatch has no colour until it's scanned again. Nothing is deleted.
- Actions: "Set aside to scan again", "Cancel"
- Variant: "restore" — R5.5 is built, so Use this reading will be offered on the reading being set aside. Its current reading moves to its history, and the swatch has no colour until it's scanned again or you bring that reading back with Use this reading. Nothing is deleted. [phase: variant-absent]

### E19 — Rename a column

- Status: pre-alignment
- Phase: none
- Variants enumerated by: none
- Headline: Rename ⟨column⟩ to ⟨text⟩?
- Body: Imports match columns by name, so a spreadsheet that still has a ⟨column⟩ column will add it as a new column instead of filling this one. A column can't be removed once it's added. Every value in this column stays as it is.
- Actions: "Rename the column", "Keep this name"

## Display labels

The fixed text the rows show outside the states above, each table citing the row it serves. Tests
read these by the identifier in the first column, never by the words.

**Mark labels** (R2.4, R3.4, R8.9; F38). A mark's filter label is what "Filters" offers; never-true
and awaiting-answer mark earlier readings and are not filter values (R3.4 filters by R2.4's marks).

| Mark | Group in E12 | Chip label | Filter label | VoiceOver name |
|---|---|---|---|---|
| cannot-show | Colour beyond a limit | Can't show | Can't show on this screen | can't show on this screen |
| outside-sRGB | Colour beyond a limit | Outside sRGB | Outside sRGB | outside sRGB |
| simulated | Where the reading came from | Simulated | Simulated | simulated reading |
| non-spectral | Where the reading came from | No spectral data | No spectral data | no spectral data |
| samples-disagreed | Where the reading came from | Samples disagreed | Samples disagreed, average accepted | samples disagreed |
| value-absent | No colour to show | Not in this condition | Not in this condition | not in this condition |
| no-value | No colour to show | No current value | No current value | no current value |
| unreadable | No colour to show | Unreadable | Unreadable | unreadable |
| re-scan-unanswered | Waiting on you | Re-scan unanswered | Re-scan unanswered | re-scan unanswered |
| never-true | Earlier readings | Never right | — | never right |
| awaiting-answer | Earlier readings | Awaiting answer | — | awaiting answer |

**Column headers** (R2.1, R1.9). The chip column has no header text; an imported column is headed
by the name the file stores for it.

| Column | Header |
|---|---|
| Swatch Code | Code |
| Swatch Name | Name |
| row state | State |
| L*, C*, h° | L*, C*, h° |
| Spread | Spread |
| Swatch Alternate Code | Alt. code |
| Swatch Alternate Name | Alt. name |
| Collection | Collection |

**Detail lines** (R4.2; F37, F96). Row states use the capture PRD's words (its R10.3), and a
set-aside cause the label its copy file's Set-aside cause labels table gives it.

| Line | Label | What follows it |
|---|---|---|
| R4.2a Identity | Swatch | Code, name, alt. code, alt. name, collection, then each imported column under its stored name |
| R4.2b State | State | The row state; for a set-aside swatch, its cause and then set aside for good, or set aside, still to deal with |
| R4.2c Current value | Colour | The chip, then each space's values with its light, observer, condition and version; Not in this condition where the collection's condition has no value; or No current value |
| R4.2d The current reading | Reading | Measured, then the date · instrument, model, serial and firmware · samples kept · averaged over spectral curves or colour values · spread · samples agreed, or samples disagreed, average accepted |
| R4.2e Marks | Marks | Each mark's chip label; where a non-spectral reading's reference differs from the collection's: Worked out under a different light from this collection's, because this reading has no spectral data |
| R4.2f History | History | The number of readings, as 1 reading or 2 readings and so on |
| R4.2h Re-scans awaiting an answer | Waiting on you | The Data Foundation PRD's E11 |

**History lines** (R5.2, R5.4). A supersession reason is shown in these words: initial — First
reading; re-measurement — Re-measured; correction — Correction; correction-unconfirmed — Re-scan
awaiting your answer; restore — Earlier reading used again.

| Line | Label | What follows it |
|---|---|---|
| R5.2a Times | Measured, Recorded | The date after each |
| R5.2b Device | Device | The device, with the simulated chip label where it applies |
| R5.2c Reason | Why | The reason's words above, and Current on the current reading |
| R5.2d Standing | Marks | The chip labels of never-true, awaiting-answer and unreadable where they apply |
| R5.2e Value | Colour | The chip, samples kept and spread |
| R5.4 distance | From current | ΔE2000 and the distance; or, where the two were worked out under different light, observer or condition: Not compared — worked out under a different light or measurement condition |
