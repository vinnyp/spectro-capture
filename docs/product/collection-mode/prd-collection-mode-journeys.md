<!-- guidance: this is the acceptance-scenarios companion to `PRD: Collection Mode`. It is the
     second of the format's four homes: rows own behaviour, and the cases here supply the
     fixtures, actions, oracles and source rows that make a row executable. A case never
     introduces a rule — if a case needs a rule this PRD's rows do not state, the row is missing.
     Two fill families are placeholders: `{{...}}`, substituted at scaffold time, and `_(prompt)_`,
     filled during authoring; an unresolved instance of EITHER is a lock-blocking defect. Every
     guidance comment in this file is deleted at lock, so a rule a BUILDER needs after lock lives
     in body text and never in a comment. -->

# Acceptance scenarios: Collection Mode
<!-- guidance: Collection Mode matches the PRD's title exactly, so the pair reads as one
     document; the file itself is `prd-collection-mode-journeys.md` beside the PRD in
     docs/product/collection-mode.
     docs/product/collection-mode: the project's product-docs directory, where the PRD and its four
     companions live. -->

Rows own behaviour. The cases in this file supply fixtures, actions, oracles and the rows each
case sources; they restate and cite rules, never introduce them. Every case in this file resolves
to at least one row of `docs/product/collection-mode/prd-collection-mode.md`.

## Harness
<!-- guidance: the harness statement. Name the simulated seam a case drives (what stands in for
     the real world), and the readback controls a case asserts through (what the test can
     observe). Then, verbatim: "Declare every exercised seam input; only named defaults are
     exempt." Fixture values are declared here or in the case's Given — never copied out of the
     implementation, because a fixture copied from the code asserts the code against itself.
     The fixture-continuity rule below is BODY text, not guidance: it is the semantics under which
     a builder reads every case table in this file, and it has to survive lock.
     Not conditional: a companion with no harness statement cannot be executed unattended. -->

The cases drive the app against a seeded file whose contents a case declares rather than walks to
(the Data Foundation PRD's R7.2; R8.10a), a declared display gamut, and a declared session state on
a collection (the capture PRD's R11.6); the device PRD's Demo Device supplies a reading only where a
case needs one that capture saves while the case runs, and nothing is measured by an instrument. A
case asserts through what R8.10 lets a test read — the surfaces' rows or swatches in order with
their columns, the selection, search text, active filters, offered and disabled actions, copy state,
variant and token values (R8.10b), each chip's marks, fill, rendered value and display triplet
(R8.10c), the bytes of the file and of what sits beside it (R8.10d), and the app's own storage
(R8.10e) — through the file read with the app closed at SQLITE_READER_FLOOR (the Data Foundation
PRD's R7.1), through the capture PRD's R11.11 readback of queue order, row states and a session's
current and remembered row, through the device PRD's R6.17 record of outbound attempts, and through
R8.10f's timing readback. Every fixture here is synthetic: invented collections, codes, names,
serials and dates. Declare every exercised seam input; only named defaults are exempt. Fixture
values are declared, never copied from the implementation.

**In-flight runs.** A Given that declares a bulk session in flight on a collection *in each
in-flight state* runs four times — the session active, paused by the operator, paused by the guard,
and held by a device halt — each declared through the capture PRD's R11.6 and the device PRD's R6.9,
whether at the start of the case or at the step that says so.

**Gamut margins.** Every seeded value declared inside or outside sRGB or Display P3 sits at least
0.03 from each boundary of that gamut in linear RGB, worked out from its L*, C*, h° under D50/2°
with Bradford adaptation to the display white (OQ 7's interim) and checked against the Data
Foundation PRD's R7.5 reference values before a case relies on it. The Find similar and distance
collections whose values sit outside sRGB (Blues, Blues Two, Pair, Hist and Daylight) declare no
gamut membership, their gamut-clipped flags set as their values place them, and no case reads their
marks; every other value a case declares sits well inside sRGB.

**Fixture continuity.** Every case runs against a fixture reset to this harness's declared state,
unless the journey's preamble line declares continuity across that journey's cases; where it does,
each case in that journey runs against the state the previous case left. Continuity is declared in
the preamble line or it does not hold — a case never carries state no preamble granted it, and a
builder reading a case table needs no other source to know which of the two applies.

**Named defaults** (the seam inputs a case may leave undeclared, each with its default value):

| Seam input | Default | Rows |
|---|---|---|
| The file | One file holding exactly two collections, Studio Markers and Gouache Set, as the lines below declare them, with every table column of each shown, changed by nothing outside the app | R2.10, R8.10 |
| Studio Markers | Chosen condition M1, illuminant and observer D50/2°, 3 samples per row, one imported column named Family; 13 items holding 16 readings, in the queue order ZX-001, ZX-002, ZX-003, ZX-004, ZX-005, ZX-006, ZX-007, ZX-010, ZX-011, ZX-012, ZX-013, ZX-014, ZX-015, set by import with no manual reorder | R2.1, R8.10 |
| Studio Markers item ZX-001 | Swatch Name Sky Blue; alternate code B-12; alternate name Azure; Family Blue; captured; one spectral reading measured 2026-02-03 by a live Spectro 2, serial SN-100200, firmware 2.1.0, with 3 samples, averaging basis spectral curves, recorded spread ΔE2000 0.4 and agreement verdict agreed; current value L* 62, C* 36, h° 254; inside sRGB and inside Display P3 | R8.10 |
| Studio Markers item ZX-002 | Signal Green; Family Green; captured; one live spectral reading; L* 70, C* 80, h° 150; gamut-clipped flag set; inside Display P3 | R8.10 |
| Studio Markers item ZX-003 | Neon Magenta; Family Pink; captured; one live spectral reading; L* 55, C* 120, h° 340; gamut-clipped flag set; outside Display P3 | R8.10 |
| Studio Markers item ZX-004 | Cool Grey 3; Family Grey; captured; one live reading marked non-spectral, worked out at the collection's reference; L* 75, C* 1.5, h° 250; inside sRGB | R8.10 |
| Studio Markers item ZX-005 | Demo Red; Family Red; captured; one reading whose acquiring snapshot is the Demo Device's, of the simulated kind, with 3 samples, recorded spread ΔE2000 3.1 and agreement verdict disagreed, average accepted; L* 48, C* 60, h° 30; inside sRGB | R8.10 |
| Studio Markers item ZX-006 | Deep Navy; Family Blue; captured; one live spectral reading with no measurement in the chosen condition, so its working-set value is absent and marked | R8.10 |
| Studio Markers item ZX-007 | Chalk White; Family White; captured; one live spectral reading of 1 sample; L* 95, C* 2.5, h° 90; inside sRGB | R8.10 |
| Studio Markers item ZX-010 | Warm Grey 1; Family Grey; pending; never scanned | R8.10 |
| Studio Markers item ZX-011 | Lemon; Family Yellow; set aside, not yet deliberately left, cause light leak; no reading | R8.10 |
| Studio Markers item ZX-012 | Ochre; Family Yellow; set aside, not yet deliberately left, cause unreadable; its current reading quarantined; one readable earlier reading measured 2026-02-01 | R2.4h, R8.10 |
| Studio Markers item ZX-013 | Teal; Family Green; captured; three live spectral readings recorded in this order: T1, reason initial, measured 2026-01-10; T2, reason re-measurement, measured 2026-03-02; T3, reason correction-unconfirmed, measured 2026-05-20, current; current value L* 60, C* 30, h° 190; inside sRGB | R8.10 |
| Studio Markers item ZX-014 | Plum; Family Purple; captured; two live spectral readings: P1, reason initial, measured 2026-01-12, marked never true; P2, reason correction, measured 2026-04-04, current; L* 35, C* 45, h° 320; inside sRGB | R8.10 |
| Studio Markers item ZX-015 | Brick; Family Red; captured; two spectral readings: B1, reason initial, measured 2026-02-10, from the Demo Device, of the simulated kind; B2, reason re-measurement, measured 2026-06-15, live, current; L* 45, C* 50, h° 35; inside sRGB | R8.10 |
| Gouache Set | Chosen condition M1, D50/2°, no imported column; 2 items holding 2 readings, in the queue order ZX-001, GS-002 | R8.10 |
| Gouache Set item ZX-001 | Cerulean; captured; one live spectral reading; L* 58, C* 32, h° 240; inside sRGB | R8.10 |
| Gouache Set item GS-002 | Cadmium Red; captured; one live spectral reading; L* 50, C* 70, h° 35; inside sRGB | R8.10 |
| Alternates | No item but Studio Markers' ZX-001 holds an alternate code or an alternate name | R3.1, R8.10 |
| Display | One display, reporting the sRGB gamut, showing the window | R2.5, R8.10 |
| Sessions | No session on any collection | R8.3, R8.10 |
| Build phase | The first build phase — P0 rows only — unless a journey's preamble or a case's Given names a later one | R8.10 |
| Named constants | Each at the candidate the PRD's constants table gives | R3.3, R3.7, R6.2, R7.1 |
| Clock | 2026-09-01T10:00:00Z, advanced only by a case step that says so | R8.10 |
| Language | English, the user's language every text comparison and sort follows | R1.1, R3.2, R8.10 |
| Network | Reachable, with telemetry off and update checks off (the device PRD's R2.13 and R2.14) | R8.6 |
| Timing workload | Search texts in turn — a prefix every item matches, a prefix one item in a hundred matches, text only an imported value holds, and text nothing matches — each typed one keystroke per 150 ms; filters in turn over every row-state value and every R2.4 mark; headers in turn over every sortable column, each fired twice; the first choice of a collection after launch timed cold, every later input warm | R8.1, M1 |

## Transition index
<!-- guidance: `T1..Tn`, one case per route through the entity lifecycle — the executable
     counterpart of the PRD's row-transitions table. Enumerate these from that table and from the
     rows, not from memory: a route the PRD's transition table names and this index omits is a
     defect, and so is the reverse. -->

| Case | Given | When | Assert | Rows |
|---|---|---|---|---|
| T1 | Studio Markers item ZX-010 present; first build phase | Fire "Delete swatch" on ZX-010, then the Data Foundation PRD's E8 delete action | ZX-010 is deleted: the table lists 12 items without it, the file read with the app closed holds no item ZX-010, and its bytes hold the text Warm Grey 1 nowhere | R4.5 |
| T2 | ZX-010 present; R1.7 built, OQ 10 closed | Fire "Delete swatch" on ZX-010, then the Data Foundation PRD's E8 delete action | ZX-010 is deleted-undoable: E10 renders with no variant naming ZX-010 and Studio Markers, and the table lists 12 items without it | R1.7 |
| T3 | ZX-010 deleted-undoable, as T2's When leaves it; R1.7 built | Fire "Undo" on E10 | ZX-010 is present: the table lists 13 items, with ZX-010 pending at queue position 8 | R1.7 |
| T4 | ZX-010 deleted-undoable, as T2's When leaves it; R1.7 built | Close the file; with the app closed read its bytes; reopen it | The bytes hold the text Warm Grey 1 nowhere; after reopening ZX-010 is deleted: the table lists 12 items, E10 is not up, and the file holds no item ZX-010 | R1.7 |
| T5 | Gouache Set present | Choose Gouache Set, fire "Rename collection" and enter Gouache Travel Set | Gouache Travel Set is present with its 2 items, and the file names it Gouache Travel Set | R1.3 |
| T6 | Studio Markers item ZX-001 present | In ZX-001's detail set Swatch Name to Harbour and press Return | ZX-001 is present, the file holds Swatch Name Harbour for it and 1 reading, and its bytes hold the text Sky Blue nowhere | R4.3 |
| T7 | Studio Markers item ZX-001 present; R4.4 built | Fire "Change code" on ZX-001, enter ZX-100, then fire "Change the code" | The item is present as ZX-100 with its 1 reading, and Studio Markers holds no item ZX-001 | R4.4 |
| T8 | Studio Markers' Family column present; R4.8 built | Fire "Rename column" on Family, enter Hue Family, then fire E19's "Rename the column" | The file stores the column as Hue Family in the position Family had, with ZX-001's value Blue | R4.8 |
| T9 | Studio Markers items ZX-001 and ZX-002 present; R6.1 and R6.2 built | Select both, fire "Set a field", choose Family and enter Brights | Both are present with Family Brights in the file, and ZX-003's Family reads Pink | R6.2 |
| T10 | ZX-001 and ZX-002 holding Family Brights after the bulk change T9's When makes; R4.7 built | Fire "Undo change" | The file holds Family Blue for ZX-001 and Family Green for ZX-002 | R4.7 |
| T11 | Studio Markers item ZX-013 present; R5.5 built | Fire "Show history" in ZX-013's detail, then "Use this reading" on T1 | ZX-013 is present with 4 readings: a new current one whose reason is restore and whose measurement time is 2026-01-10, then T3, T2 and T1 | R5.5 |
| T12 | Studio Markers item ZX-013 present, T2 awaiting an answer | In ZX-013's detail fire the Data Foundation PRD's E11 swatch-has-changed action | ZX-013 is present, T3's reason reads re-measurement, T2 carries no awaiting-answer mark, E11 is not up, and E2 no longer offers "Answer re-scans" | R4.6, R5.7 |
| T13 | Studio Markers item ZX-010 present and pending; R2.9 built; table in queue order | Drag ZX-010 above ZX-001 | ZX-010 is present, and the queue order the capture PRD's R11.11 reads begins ZX-010, ZX-001, ZX-002 | R2.9 |
| T14 | Studio Markers item ZX-001 present and captured; R4.9 built | In ZX-001's detail fire the capture PRD's Flag action, then E18's "Set aside to scan again" | ZX-001 is present with its 1 reading, which is no longer current, and the capture PRD's R11.11 reads its row state set aside | R4.9 |
| T15 | Studio Markers present; R2.10 built | Hide the Spread column | Studio Markers is present with its 13 items, and the file read with the app closed records Spread hidden for it | R2.10 |

## Journeys
<!-- guidance: one `### UJ n. <name>` section per journey, in the PRD's order. Under each heading:
     one line of phase/preamble (which build phase the journey's cases run in, and any continuity
     that holds across them, per the Harness's fixture-continuity rule), then the five-column case
     table.
     Rules for every case table in this file:
     - Every Assert names values or observable outcomes. "Correct", "unchanged", "appropriate" and
       "as expected" are not oracles; a case that asserts a value this document gives no way to
       read is a defect.
     - A case whose Given declares a fixture no row grants is a defect: either the row is missing
       or the fixture is invented.
     - Every copy state and every variant in the copy companion has a case here.
     - Per-cause failures get one case each — one case covering "any failure" hides the causes
       that behave differently. -->

**Case IDs.** A case ID is `UJ<journey>.<scenario>-<letter>` (`UJ3.1-a`, `UJ3.1-b`, `UJ3.2-a`). A
**scenario** is one continuous run through the journey: a journey that is walked once has a single
scenario, numbered 1, and a second scenario is a second run through the same journey from a
different starting state. Cases within a scenario are lettered in execution order. The
`### UJ n. <name>` headings are the stable anchors the PRD, the fences and sibling documents cite:
a journey is never renumbered or retitled once cited, and case IDs are assigned once and never
renumbered.

### UJ 1. Manage collections

Scenario 1 runs in the first build phase under continuity — each case runs against the state the
previous case left — and carries the capture PRD's inherited case UJ1.2-a. Scenario 2 runs in the
first build phase, each case from the harness state, and carries that PRD's UJ1.2-b. Scenario 3
runs each case from the harness state, in the first build phase except UJ1.3-d, UJ1.3-e and
UJ1.3-f, which run in the phase that lands R1.7 once OQ 10 closes; it carries that PRD's UJ1.2-c.
Scenario 4 runs in the first build phase, each case from the harness state or its own Given.

| Case | Given | When | Assert | Rows |
|---|---|---|---|---|
| UJ1.1-a | The seeded file | Choose Gouache Set, fire "Rename collection" and enter Gouache Travel Set | E3's ⟨collection⟩ reads Gouache Travel Set, E2 lists Gouache Travel Set then Studio Markers, and the file read with the app closed names the collection Gouache Travel Set | R1.3 |
| UJ1.1-b | The state UJ1.1-a left | On Gouache Travel Set fire "Rename collection" and enter a space, studio in lower case, three spaces, MARKERS in upper case, and a space; then fire the capture PRD's E1 action and press Escape | The capture PRD's E1 renders; after its action and Escape it is not up, and the file names the collection Gouache Travel Set | R1.3 |
| UJ1.1-c | The state UJ1.1-b left | On Gouache Travel Set fire "Rename collection" and enter three spaces; then fire E7's "Change the name" and press Escape | E7 renders naming Gouache Travel Set; after "Change the name" and Escape it is not up, and the file names the collection Gouache Travel Set | R1.3 |
| UJ1.1-d | The state UJ1.1-c left | On Gouache Travel Set fire "Rename collection" and enter gouache travel set, all in lower case | E3's ⟨collection⟩ reads gouache travel set, and the file stores the name gouache travel set as entered | R1.3 |
| UJ1.1-e | The state UJ1.1-d left | Fire "Rename collection", type Studio Inks, then press Escape | E3's ⟨collection⟩ reads gouache travel set, and the file names the collection gouache travel set | R1.3 |
| UJ1.2-a | Studio Markers holds a bulk session in flight, in each in-flight state | Choose Studio Markers and fire "Delete collection" | E6 renders with no variant naming Studio Markers, and the file holds Studio Markers with 13 items and 16 readings | R1.4, R8.3 |
| UJ1.2-b | Studio Markers holds an interrupted session and no session in flight | Choose Studio Markers and fire "Delete collection" | E6 renders its "interrupted" variant naming Studio Markers, and the file holds Studio Markers with 13 items and 16 readings | R1.4 |
| UJ1.2-c | As UJ1.2-a, the session paused by the operator, with E6 up | Fire "Go to the session" | The capture surface of Studio Markers' paused session is the surface shown, and the file holds Studio Markers with 13 items | R1.5 |
| UJ1.2-d | As UJ1.2-b, with E6 up | Fire "Go to the session" | The capture PRD's E25 renders for Studio Markers | R1.5 |
| UJ1.2-e | As UJ1.2-a, the session paused by the operator, with E6 up | Fire "Cancel" | E6 is not up, and the file holds Studio Markers with 13 items and 16 readings | R1.5 |
| UJ1.3-a | The seeded file | Choose Studio Markers, fire "Delete collection", then press Return | The Data Foundation PRD's E14 renders naming Studio Markers, 13 swatches and 16 readings; after Return it is not up and E3 states 13 swatches | R1.4, R1.6 |
| UJ1.3-b | The seeded file | Choose Studio Markers, fire "Delete collection", then the Data Foundation PRD's E14 export-first action, then close the export | The Data Export PRD's E1 renders for Studio Markers at collection scope; after it closes the Data Foundation PRD's E14 is up again and the file holds Studio Markers with 13 items | R1.4 |
| UJ1.3-c | The seeded file | Choose Studio Markers, fire "Delete collection", then the Data Foundation PRD's E14 delete action | The collection list lists only Gouache Set, and the file read with the app closed holds no item and no reading of Studio Markers | R1.4 |
| UJ1.3-d | The seeded file; R1.7 built | Choose Studio Markers, fire "Delete collection", then the Data Foundation PRD's E14 delete action, then "Undo" on E10 | E10 renders its "collection" variant naming Studio Markers and 13 swatches; after Undo, E2 lists Studio Markers with 13 swatches and ZX-013's history lists 3 readings | R1.7 |
| UJ1.3-e | The seeded file; R1.7 built | Choose Studio Markers, fire "Delete collection", then the Data Foundation PRD's E14 delete action; close the file, read its bytes with the app closed, and reopen it | The bytes hold the text Teal nowhere; after reopening E2 lists only Gouache Set, and E10 is not up | R1.7 |
| UJ1.3-f | The seeded file; R1.7 built | Choose Studio Markers, fire "Delete collection", then the Data Foundation PRD's E14 delete action; crash the app, read the file's bytes before reopening, and reopen it | The bytes hold the text Teal nowhere; after reopening E2 lists only Gouache Set, and E10 is not up | R1.7 |
| UJ1.4-a | A file holding no collection | Open the file | E1 renders and offers "New collection" | R1.1 |
| UJ1.4-b | The seeded file | Show the collection list | E2 renders stating 2 collections and lists Gouache Set with 2 swatches, then Studio Markers with 13 swatches | R1.1 |
| UJ1.4-c | The seeded file | Fire "New collection", complete the capture PRD's creation naming the collection Inks, then choose Inks | E2 lists Gouache Set, Inks and Studio Markers in that order, and the collection surface shows Inks with the capture PRD's E2 empty variant where the table would be | R1.2, R2.2 |
| UJ1.4-d | The seeded file | Choose Studio Markers and fire "Export collection" | The Data Export PRD's E1 renders for Studio Markers at collection scope, naming 13 swatches | R1.8 |
| UJ1.4-e | The seeded file | Choose Studio Markers, type zx-01 in the search field, then fire "Export collection" | The Data Export PRD's E1 renders for Studio Markers at collection scope, naming 13 swatches, not the 6 the search lists | R1.8 |

### UJ 2. Browse a collection honestly

Scenario 1 runs in the first build phase, each case from the harness state or its own Given;
UJ2.1-h carries the collection half of the device PRD's UJ1.2-c. Scenario 2 runs in the phase that
lands R2.10, each case from the harness state.

| Case | Given | When | Assert | Rows |
|---|---|---|---|---|
| UJ2.1-a | The seeded file | Choose Studio Markers | E3 renders with no variant stating 13 swatches; the table lists ZX-001, ZX-002, ZX-003, ZX-004, ZX-005, ZX-006, ZX-007, ZX-010, ZX-011, ZX-012, ZX-013, ZX-014, ZX-015 in that order, under the columns chip, Swatch Code, Swatch Name, row state, L*, C*, h°, Spread, Swatch Alternate Code, Swatch Alternate Name and Family | R2.1 |
| UJ2.1-b | The seeded file; display gamut sRGB | Choose Studio Markers and list each chip's marks and fill | ZX-002 and ZX-003 each carry cannot-show and outside-sRGB, ZX-002's chip filled with the working-set value L* 70, C* 80, h° 150 at D50/2°, M1, and a clipped display triplet; ZX-004 non-spectral; ZX-005 simulated and samples-disagreed; ZX-006 value-absent, with an empty chip; ZX-010 and ZX-011 no-value, each with an empty chip; ZX-012 unreadable, with an empty chip; ZX-013 re-scan-unanswered; ZX-001, ZX-007, ZX-014 and ZX-015 carry no mark | R2.3, R2.4 |
| UJ2.1-c | The seeded file; display gamut Display P3 | Choose Studio Markers and list each chip's marks and fill | ZX-002 carries outside-sRGB and not cannot-show, its chip filled with L* 70, C* 80, h° 150 and a display triplet that is not clipped; ZX-003 carries both | R2.3, R2.4, R2.5 |
| UJ2.1-d | The seeded file; the window on a display reporting Display P3, beside a second display reporting sRGB | Move the window to the sRGB display | Within 5 s, a functional timeout (timing is UJ9.5-a's), ZX-002 carries cannot-show | R2.5 |
| UJ2.1-e | The seeded file; the only display reports no gamut | Choose Studio Markers and list each chip's marks | ZX-002 and ZX-003 each carry cannot-show, and ZX-001 does not | R2.5 |
| UJ2.1-f | The seeded file | Choose Studio Markers, then Gouache Set | The device PRD's E22 is up on Studio Markers and not up on Gouache Set | R2.6 |
| UJ2.1-g | The seeded file | Choose Studio Markers and fire E22's action, then "Clear filters" | The table lists only ZX-005, the simulated filter shows as active, and E3 renders its "narrowed" variant stating 1 of 13, offering "Clear filters" and not "Clear search"; after "Clear filters" the table lists 13 items and E3 renders with no variant | R2.6, R3.4 |
| UJ2.1-h | The seeded file, with ZX-005's current reading's snapshot of the live kind | Choose Studio Markers, open ZX-015 and fire "Show history" | E22 is not up; ZX-015's chip carries no simulated mark; E17 lists B2 without the simulated mark and B1 with it | R2.6, R5.2 |
| UJ2.1-i | The seeded file | Choose Studio Markers and read the L*, C*, h° and Spread columns | ZX-001 shows 62.0, 36.0, 254.0 and 0.40, ZX-005 shows spread 3.10, and ZX-007 shows no spread | R2.7, R2.11 |
| UJ2.1-j | The seeded file; Studio Markers shows the capture PRD's E23 end-early summary | Choose Studio Markers and list the collection surface | The table lists the 13 items; E23 is up; the entry points the capture PRD's R11.15g lists are listed beside it; the counts read 10 scanned, 2 set aside and 1 to go | R2.2 |
| UJ2.1-k | The seeded file | Choose Studio Markers and fire "Colour marks", then E12's "Close" | E12 renders naming eleven marks: the nine R2.4 lists, never-true and awaiting-answer, each with its shape; after "Close" it is not up | R2.8 |
| UJ2.1-l | The seeded file; the window on a display reporting Display P3 | That display's reported gamut changes to sRGB, as a profile change does | ZX-002 carries cannot-show | R2.5 |
| UJ2.1-m | The seeded file; the window across a display reporting Display P3 and one reporting sRGB, macOS reporting it on the Display P3 one | Choose Studio Markers and list each chip's marks | ZX-002 carries outside-sRGB and not cannot-show | R2.5 |
| UJ2.1-n | The seeded file; the only display reports Rec. 2020, a gamut none of the three fixtures names | Choose Studio Markers and list ZX-002's and ZX-001's marks | ZX-002 carries outside-sRGB and not cannot-show, and ZX-001 carries no mark | R2.5 |
| UJ2.1-o | The seeded file | Open ZX-014 and fire "Colour marks", then E12's "Close"; fire "Show history" and "Colour marks" there | E12 renders from the item detail, and again from the version history view | R2.8 |
| UJ2.2-a | The seeded file; R2.10 built | Choose Studio Markers and list the columns the user can hide; hide Spread, choose Gouache Set, then quit the app and read the file | The columns offered are Swatch Name, row state, L*, C*, h°, Spread, Swatch Alternate Code, Swatch Alternate Name and Family, and neither the chip nor Swatch Code; Gouache Set still shows Spread; the file read with the app closed records Spread hidden for Studio Markers and no column hidden for Gouache Set | R2.10 |
| UJ2.2-b | The seeded file; R2.10 built | In Studio Markers hide Family and Spread; quit the app, relaunch it and choose Studio Markers; then quit it and read the file | After relaunch the columns are the chip, Swatch Code, Swatch Name, row state, L*, C*, h°, Swatch Alternate Code and Swatch Alternate Name; the file read with the app closed records Family and Spread hidden for Studio Markers and still holds the Family column with ZX-001's value Blue | R2.10 |

### UJ 3. Find a swatch

Scenarios 1–3 run in the first build phase, each case from the harness state or its own Given.
Scenario 4 runs in the phase that lands R3.7 and R3.8, each case from its own Given; its distances
are the published CIEDE2000 test values OQ 11 names, shown to two places under R2.11.

| Case | Given | When | Assert | Rows |
|---|---|---|---|---|
| UJ3.1-a | The seeded file; Studio Markers chosen | Type zx-01 in the search field | The table lists ZX-010, ZX-011, ZX-012, ZX-013, ZX-014, ZX-015, and E3 renders its "narrowed" variant stating 6 of 13, offering "Clear search" and not "Clear filters" | R3.1, R3.4 |
| UJ3.1-b | The seeded file; Studio Markers chosen | Type purple | The table lists only ZX-014, whose Family is Purple | R3.1 |
| UJ3.1-c | The seeded file; Studio Markers chosen | Type b-1; then empty the field and type zure | The table lists only ZX-001 both times | R3.1 |
| UJ3.1-d | The seeded file; Studio Markers chosen | Type 12 | E4 renders naming 12, and the table lists no item | R3.1, R3.5 |
| UJ3.1-e | The seeded file; Studio Markers chosen | Type a space, SKY in upper case, three spaces, blue, and a space | The table lists only ZX-001 | R3.1 |
| UJ3.1-f | The seeded file; Studio Markers chosen | Type qqq, then fire "Clear search" | E4 renders offering only "Clear search"; after it, E3 renders with no variant and the table lists 13 items | R3.5 |
| UJ3.1-g | The seeded file; Studio Markers chosen | Type three spaces | The table lists 13 items, and E3 renders with no variant, offering neither "Clear search" nor "Clear filters" | R3.1, R3.4 |
| UJ3.2-a | The seeded file; Studio Markers chosen | Choose the row-state filter value set aside | The table lists ZX-011 and ZX-012 | R3.4 |
| UJ3.2-b | The seeded file; Studio Markers chosen | Choose the mark filter values outside-sRGB and simulated | The table lists ZX-002, ZX-003, ZX-005 | R3.4 |
| UJ3.2-c | The seeded file; Studio Markers chosen | Choose the row-state value pending and the mark value outside-sRGB, then fire "Clear filters" | E5 renders stating 13 swatches hidden; after it, the table lists 13 items | R3.4, R3.5 |
| UJ3.2-d | The seeded file; Studio Markers chosen | Type qqq, choose the mark value simulated, then fire "Clear filters" | E5 renders with its count sentence left out, the search having listed none; after "Clear filters" the search field holds qqq and E4 renders | R3.4, R3.5 |
| UJ3.3-a | A collection named Codes holding items A10, A2, A and B1 in that queue order | Fire the Swatch Code header, then fire it again | First the table lists A, A2, A10, B1; then B1, A10, A2, A | R3.2 |
| UJ3.3-b | The seeded file; Studio Markers chosen | Fire the Family header | The table lists ZX-001, ZX-006, ZX-002, ZX-013, ZX-004, ZX-010, ZX-003, ZX-014, ZX-005, ZX-015, ZX-007, ZX-011, ZX-012 | R3.2 |
| UJ3.3-c | The seeded file; Studio Markers chosen | Fire the L* header, then fire it again | First ZX-014, ZX-015, ZX-005, ZX-003, ZX-013, ZX-001, ZX-002, ZX-004, ZX-007, ZX-006, ZX-010, ZX-011, ZX-012; then ZX-007, ZX-004, ZX-002, ZX-001, ZX-013, ZX-003, ZX-005, ZX-015, ZX-014, ZX-006, ZX-010, ZX-011, ZX-012 | R3.2, R3.3 |
| UJ3.3-d | The seeded file; Studio Markers chosen | Fire the h° header | ZX-005, ZX-015, ZX-002, ZX-013, ZX-001, ZX-014, ZX-003, ZX-004, ZX-007, ZX-006, ZX-010, ZX-011, ZX-012 | R3.3 |
| UJ3.3-e | The seeded file; Studio Markers chosen | Fire the Swatch Name header, then read the queue order through the capture PRD's R11.11 | The table lists ZX-015, ZX-007, ZX-004, ZX-006, ZX-005, ZX-011, ZX-003, ZX-012, ZX-014, ZX-002, ZX-001, ZX-013, ZX-010; the queue order reads ZX-001, ZX-002, ZX-003, ZX-004, ZX-005, ZX-006, ZX-007, ZX-010, ZX-011, ZX-012, ZX-013, ZX-014, ZX-015 | R3.2 |
| UJ3.3-f | The seeded file | In Studio Markers type zx-01 and fire the L* header; choose Gouache Set; choose Studio Markers; then close the file, reopen it and choose Studio Markers; then quit the app and read its own storage | On returning, the search field holds zx-01 and the table lists ZX-014, ZX-015, ZX-013, ZX-010, ZX-011, ZX-012; after reopening, the search field is empty and the table lists 13 items in queue order; the app's own storage holds the text zx-01 nowhere | R3.6, R8.6 |
| UJ3.3-g | The seeded file; Studio Markers chosen | Fire the C* header; then fire the h° header twice | C* lists ZX-004, ZX-007, ZX-013, ZX-001, ZX-014, ZX-015, ZX-005, ZX-002, ZX-003, ZX-006, ZX-010, ZX-011, ZX-012; the second h° fire lists ZX-003, ZX-014, ZX-001, ZX-013, ZX-002, ZX-015, ZX-005, ZX-004, ZX-007, ZX-006, ZX-010, ZX-011, ZX-012 | R3.3 |
| UJ3.3-h | A collection Near Greys, chosen condition M1, D50/2°, holding NG-1 at L* 40, C* 3.1, h° 90, NG-2 at L* 30, C* 2.9, h° 80 and NG-3 at L* 50, C* 20, h° 100, in that queue order | Fire the h° header, then fire it again | First NG-1, NG-3, NG-2; then NG-3, NG-1, NG-2 | R3.3 |
| UJ3.3-i | The seeded file; Studio Markers chosen | Fire the row-state header, then fire it again, then try to sort by the chip column | First ZX-010, ZX-001, ZX-002, ZX-003, ZX-004, ZX-005, ZX-006, ZX-007, ZX-013, ZX-014, ZX-015, ZX-011, ZX-012; then ZX-011, ZX-012, ZX-001, ZX-002, ZX-003, ZX-004, ZX-005, ZX-006, ZX-007, ZX-013, ZX-014, ZX-015, ZX-010; the chip column offers no sort and the order stays | R3.2 |
| UJ3.3-j | A collection Spreads, chosen condition M1, D50/2°, holding SP-1, captured from 3 samples with spread 0.80, SP-2, captured from 3 samples with spread 0.25, SP-3, pending, and SP-4, captured from 1 sample, in that queue order, each captured value L* 50, C* 10, h° 90 | Fire the Spread header, then fire it again | First SP-2, SP-1, SP-3, SP-4; then SP-1, SP-2, SP-3, SP-4 | R3.2 |
| UJ3.3-k | A collection Refs, chosen condition M1, D50/2°, holding RF-1 at L* 40, C* 20, h° 60, RF-2 at L* 60, C* 20, h° 60, and RF-3, non-spectral, at L* 50, C* 20, h° 60 worked out at D65/10° (the Data Foundation PRD's R3.3e), in that queue order | Fire the L* header | The table lists RF-1, RF-2, RF-3, and E3's ⟨unlike⟩ reads 1 | R3.3 |
| UJ3.4-a | A collection Blues, chosen condition M1, D50/2°, holding FS-000 at L* 50, a* 0, b* −82.7485; FS-001 at 50, −1.3802, −84.2814; FS-002 at 50, 2.6772, −79.7751; FS-003 at 50, 3.1571, −77.2803; FS-004 at 50, 2.8361, −74.0200; FS-005, non-spectral, at 50, 0, −81.5000 worked out at D65/10°, a reference other than the collection's; FS-006, pending; published ΔE2000 from FS-000 of 1.0000, 2.0425, 2.8615 and 3.4412 for FS-001 to FS-004 | Open FS-000 and fire "Find similar" | E9 renders with no variant, listing FS-001 at 1.00, FS-002 at 2.04 and FS-003 at 2.86 in that order, and stating 2 swatches not compared | R3.7, R2.11 |
| UJ3.4-b | A collection Pair holding FS-000 and FS-004 at the values UJ3.4-a declares, published ΔE2000 3.4412 between them | Open FS-000 and fire "Find similar", then E9's "Close" | E9 renders its "none" variant; after "Close" it is not up | R3.8 |
| UJ3.4-c | Blues as UJ3.4-a declares it | Open FS-006 | E14 does not offer "Find similar" | R3.8 |
| UJ3.4-d | A file holding only Blues as UJ3.4-a declares it; Blues Two, chosen condition M1, D50/2°, holding FS-101 at L* 50, a* −1.1848, b* −84.8006, published ΔE2000 1.0000 from FS-000; and Daylight, chosen condition M1, D65/10°, holding DL-101, its working-set value L* 50, a* 0, b* −82.7485 — FS-000's numbers under another illuminant and observer; R1.9 and R1.10 built | From the All items view open Blues' FS-000 and fire "Find similar"; then from Blues open FS-000 and fire it again | From All items, E9 lists FS-001 then FS-101, each at 1.00 and naming its collection, then FS-002 at 2.04 and FS-003 at 2.86, lists no DL-101, and states 3 swatches not compared; from Blues, E9 lists FS-001, FS-002 and FS-003, no FS-101, and states 2 not compared | R3.7, R1.10 |
| UJ3.4-e | Blues as UJ3.4-a declares it | Open FS-000, fire "Find similar", then choose FS-002 in E9 | E14 renders for FS-002 | R3.7 |
| UJ3.4-f | A collection Greys, chosen condition M1, D50/2°, holding GA at L* 51.5, a* 0, b* 0, and GB-2 and GB-1 each at L* 48.5, a* 0, b* 0, each ΔE2000 3.0000 from GA | Open GA and fire "Find similar" | E9 lists GB-1 then GB-2, each at 3.00 | R3.7 |

### UJ 4. Look at and edit a swatch

Scenarios 1, 2, 4 and 5 run in the first build phase, except UJ4.4-e and UJ4.4-g, which run in the
phase that lands R1.7; scenario 2 runs under continuity, each of its cases against the state the
previous one left, and the others each case from the harness state or its own Given. Scenario 3
runs in the phase that lands R4.4, scenario 6 in the phase that lands R4.8 and scenario 7 in the
phase that lands R4.9, each case from the harness state or its own Given, except that UJ4.6-e and
UJ4.6-f each run against the state UJ4.6-a's When leaves and UJ4.7-d runs against the state UJ4.7-a
left.

| Case | Given | When | Assert | Rows |
|---|---|---|---|---|
| UJ4.1-a | The seeded file | Open Studio Markers' ZX-001 | E14 renders; its lines read Swatch Code ZX-001, Swatch Name Sky Blue, alternate code B-12, alternate name Azure, collection Studio Markers, Family Blue, row state captured, six spaces each stamped D50, 2°, M1 and a derivation version, measured 2026-02-03, device live Spectro 2 SN-100200 firmware 2.1.0, 3 samples, basis spectral curves, spread 0.40, verdict agreed, no mark, 1 reading | R4.1, R4.2 |
| UJ4.1-b | The seeded file; Studio Markers searched zx-01, view-sorted by Swatch Name, ZX-013 selected, its row the first on screen | Open ZX-013, then close it | The table lists ZX-015, ZX-011, ZX-012, ZX-014, ZX-013, ZX-010, ZX-013 is selected, and ZX-013's row is the first on screen | R4.1, R6.4 |
| UJ4.1-c | The seeded file | Open ZX-012 | E14 shows the Data Foundation PRD's E4 in its current variant, states that ZX-012 has no current value, shows the unreadable mark, and its State line reads set aside, cause unreadable, still to deal with | R4.2 |
| UJ4.1-d | The seeded file plus Studio Markers item ZX-016, Moss: captured, its current reading readable, one earlier reading quarantined | Open ZX-016 | E14 shows the Data Foundation PRD's E4 in its history variant, and ZX-016's current value | R4.2 |
| UJ4.1-e | The seeded file plus Studio Markers item ZX-017, Sand: captured, one sample's archived payload unavailable, its values intact | Open ZX-017 | E14 shows the Data Foundation PRD's E31 and ZX-017's current value | R4.2 |
| UJ4.1-f | The seeded file | Open ZX-013 | E14 shows the Data Foundation PRD's E11 naming ZX-013 and 2026-05-20 | R4.2 |
| UJ4.1-g | Refs as UJ3.3-k declares it | Open RF-3 | E14's Marks line shows the non-spectral mark and the reference-mismatch line | R4.2 |
| UJ4.2-a | The seeded file | Open ZX-001, set Swatch Name to Harbour and press Return | The file holds Swatch Name Harbour for ZX-001, 1 reading, queue position 1 and row state captured, and its bytes hold the text Sky Blue nowhere | R4.3 |
| UJ4.2-b | The state UJ4.2-a left | In ZX-001's Family field type Cyan, then press Escape | The file holds Family Blue for ZX-001 | R4.3 |
| UJ4.2-c | The state UJ4.2-b left | Empty ZX-001's Swatch Alternate Name and leave the field | The file read at SQLITE_READER_FLOOR holds an empty alternate name for ZX-001, and its bytes hold the text Azure nowhere | R4.3 |
| UJ4.3-a | The seeded file; R4.4 built | Open ZX-001, fire "Change code", enter ZX-100, then fire "Change the code" | E16 renders naming ZX-001 and ZX-100; afterwards the file holds Studio Markers item ZX-100 with 1 reading, queue position 1 and row state captured, and no Studio Markers item ZX-001, and Gouache Set's ZX-001 reads Cerulean | R4.4, R4.3 |
| UJ4.3-b | The seeded file; R4.4 built | Open ZX-001, fire "Change code", enter ZX-100, then fire "Keep this code" | The file holds Studio Markers item ZX-001 and no ZX-100 | R4.4 |
| UJ4.3-c | The seeded file; R4.4 built | Open ZX-001, fire "Change code" and enter zx-002 | E15 renders its "duplicate" variant naming ZX-001, and the file holds ZX-001 | R4.4 |
| UJ4.3-d | The seeded file; R4.4 built | Open ZX-001, fire "Change code" and enter two spaces; then fire E15's "Try another code", enter ZX-101 and fire "Change the code" | E15 renders with no variant naming ZX-001; after "Try another code" E16 renders naming ZX-001 and ZX-101, and afterwards the file holds ZX-101 and no ZX-001 | R4.4 |
| UJ4.3-e | The seeded file; R4.4 built | Open Studio Markers' ZX-007, fire "Change code", enter GS-002, then fire "Change the code" | E16 renders; afterwards Studio Markers holds GS-002 with Swatch Name Chalk White, and Gouache Set's GS-002 reads Cadmium Red | R4.4 |
| UJ4.3-f | The seeded file; R4.4 built; a bulk session in flight on Studio Markers, in each in-flight state | Open ZX-001 and fire "Change code" | E6 renders with no variant, and the file holds ZX-001 | R4.4, R8.3 |
| UJ4.3-g | The seeded file; R4.4 built | Open ZX-001, fire "Change code" and enter zx-001 | Neither E15 nor E16 renders, and the file holds the same item, with its 1 reading, under the code zx-001 | R4.4 |
| UJ4.4-a | The seeded file | Open ZX-010, fire "Delete swatch", then press Return | The Data Foundation PRD's E8 renders naming ZX-010 and Studio Markers; after Return it is not up and the table lists 13 items | R4.5, R1.6 |
| UJ4.4-b | The seeded file | Open ZX-010, fire "Delete swatch", then the Data Foundation PRD's E8 delete action | E3 states 12 swatches, the table lists no ZX-010, the file holds no item ZX-010, and its bytes hold the text Warm Grey 1 nowhere | R4.5 |
| UJ4.4-c | The seeded file; a bulk session in flight on Studio Markers, in each in-flight state | Select ZX-010's row and fire "Delete swatch" | E6 renders with no variant, and the table lists 13 items | R4.5, R6.4, R8.3 |
| UJ4.4-d | The seeded file; Studio Markers holds an interrupted session and no session in flight | Open ZX-010, fire "Delete swatch", then the Data Foundation PRD's E8 delete action | That E8 renders, not E6; afterwards the table lists 12 items and no ZX-010 | R4.5, R8.3 |
| UJ4.4-e | The seeded file; R1.7 built | Open ZX-010, fire "Delete swatch", the Data Foundation PRD's E8 delete action, then "Undo" on E10 | E10 renders with no variant naming ZX-010 and Studio Markers; after Undo the table lists ZX-010, pending, at queue position 8 | R1.7 |
| UJ4.4-f | The seeded file plus Studio Markers item ZX-020, Amber, pending, appended after ZX-015; Studio Markers holds an interrupted bulk session whose remembered row is ZX-010 | Open ZX-010, fire "Delete swatch" and the Data Foundation PRD's E8 delete action; then fire the capture PRD's E25 resume action | The capture PRD's R11.11 reads the resumed session's current row ZX-020 | R4.5 |
| UJ4.4-g | The seeded file; R1.7 built | Open ZX-010, fire "Delete swatch" and the Data Foundation PRD's E8 delete action; bring a bulk session on Studio Markers in flight, in each in-flight state; fire "Undo" on E10 | E6 renders with no variant, and the table lists 12 items and no ZX-010 | R1.7, R8.3 |
| UJ4.4-h | The seeded file | Open ZX-001 and fire "Show history"; then, in ZX-001's detail, fire "Delete swatch" and the Data Foundation PRD's E8 delete action | Neither E14 nor E17 is up, and the table lists 12 items | R4.1 |
| UJ4.5-a | The seeded file | Open ZX-001 and fire "Export swatch" | The Data Export PRD's E1 renders at single-item scope naming ZX-001 | R1.8 |
| UJ4.5-b | The seeded file; the capture PRD's re-scan rows built | Open ZX-001 and fire the capture PRD's re-scan entry | The capture PRD's E28 renders naming ZX-001 | R4.6 |
| UJ4.5-c | The seeded file | Open ZX-012 and fire the Data Foundation PRD's E4 use-a-previous-reading action | ZX-012's chip shows a colour and no unreadable mark, its history lists 3 readings, the newest current with reason restore and measurement time 2026-02-01, and the capture PRD's R11.11 reads its row state captured | R4.6 |
| UJ4.5-d | The seeded file; a bulk session in flight on Studio Markers, in each in-flight state | Open ZX-012 and fire the Data Foundation PRD's E4 use-a-previous-reading action | E6 renders with no variant, and the capture PRD's R11.11 reads ZX-012 set aside with no current value | R4.6, R8.3 |
| UJ4.6-a | The seeded file; R4.8 built | Fire "Rename column" on Family, enter Hue Family, then fire E19's "Rename the column" | E19 renders naming Family and Hue Family; afterwards the table's header reads Hue Family, and the file stores the column as Hue Family in the position Family had, with ZX-001's value Blue | R4.8 |
| UJ4.6-b | The seeded file; R4.8 built | Fire "Rename column" on Family and enter two spaces; then fire E11's "Change the name" and press Escape | E11 renders with no variant naming Family; after "Change the name" and Escape it is not up, and the file stores the column as Family | R4.8 |
| UJ4.6-c | The seeded file; R4.8 built | Fire "Rename column" on Family and enter swatch  NAME, with two spaces between the words | E11 renders its "duplicate" variant, and the file stores the column as Family | R4.8 |
| UJ4.6-d | The seeded file plus a second Studio Markers imported column named Maker; R4.8 built | Fire "Rename column" on Family and enter MAKER | E11 renders its "duplicate" variant, and the file stores the column as Family | R4.8 |
| UJ4.6-e | The state UJ4.6-a's When leaves; R4.8 built | Import into Studio Markers a CSV whose code column holds ZX-001 and whose column named Hue Family holds Teal Blue, keeping the new details | The file holds Hue Family Teal Blue for ZX-001, and Studio Markers holds one column named Hue Family and none named Family | R4.8 |
| UJ4.6-f | The state UJ4.6-a's When leaves; R4.8 built | Import into Studio Markers a CSV whose code column holds ZX-001 and whose column named Family holds Sea Blue | Studio Markers holds the column Hue Family, ZX-001's value there still Blue, and after its existing columns a new column named Family, ZX-001's value there Sea Blue | R4.8 |
| UJ4.6-g | The seeded file; R4.8 built | Fire "Rename column" on Family, type Tint, then press Escape | E19 does not render, and the file stores the column as Family | R4.8 |
| UJ4.6-h | The seeded file; R4.8 built | Fire "Rename column" on Family, enter Hue Family, then fire E19's "Keep this name" | The file stores the column as Family and holds no column Hue Family | R4.8 |
| UJ4.7-a | The seeded file; R4.9 built | Open ZX-001, fire the capture PRD's Flag action, then E18's "Set aside to scan again" | E18 renders naming ZX-001; afterwards ZX-001's chip is empty and carries the no-value mark; E14's State line reads set aside, cause flagged after capture, still to deal with; its history lists 1 reading, not current, reason initial, with no never-true mark; the capture PRD's R11.11 reads its row state set aside | R4.9, R4.2 |
| UJ4.7-b | The seeded file; R4.9 built | Open ZX-010, ZX-011, ZX-012 and ZX-013 in turn and list each detail's actions | None of the four offers the capture PRD's Flag action, ZX-013's current reading awaiting its correction answer | R4.9 |
| UJ4.7-c | The seeded file; R4.9 built; a bulk session in flight on Studio Markers, in each in-flight state | Open ZX-001 and fire the capture PRD's Flag action | E6 renders with no variant, and the capture PRD's R11.11 reads ZX-001 captured with its 1 reading current | R4.9, R8.3 |
| UJ4.7-d | The state UJ4.7-a left; R4.9 built | Open the capture PRD's set-aside list from the collection surface | It lists ZX-001 with the cause flagged after capture, beside ZX-011 and ZX-012 | R4.9 |
| UJ4.7-e | The seeded file; R4.9 built | Open ZX-001, fire the capture PRD's Flag action, then E18's "Cancel" | E18 is not up, and the capture PRD's R11.11 reads ZX-001 captured with its 1 reading current | R4.9 |

### UJ 5. Read a swatch's history and settle its re-scans

Scenarios 1 and 2 run in the first build phase, scenario 3 in the phase that lands R5.4 and R5.5,
and scenario 4 in the phase that lands R5.8, each case from the harness state or its own Given,
except UJ5.3-i, which runs against the state UJ5.3-c leaves.

| Case | Given | When | Assert | Rows |
|---|---|---|---|---|
| UJ5.1-a | The seeded file; Studio Markers chosen | Open ZX-013 and fire "Show history" | E17 renders with no variant stating 3 readings, listing T3 — current, measured 2026-05-20 — then T2 with reason re-measurement, measured 2026-03-02, and the awaiting-answer mark, then T1 with reason initial, measured 2026-01-10; E14 is still up and the table lists its 13 rows | R5.1, R5.2 |
| UJ5.1-b | The seeded file | Open ZX-014, fire "Show history", then "Measured order", then "Recorded order" | The "measured" variant lists only P2 and states 1 reading left out; the body then lists P2, then P1 carrying the never-true mark | R5.3 |
| UJ5.1-c | The seeded file | Open ZX-013, fire "Show history", then "Measured order" | The "measured" variant lists T1, T2, T3 in that order, T2 carrying the awaiting-answer mark, with no left-out sentence | R5.3 |
| UJ5.1-d | The seeded file | For each reading of ZX-012, ZX-013 and ZX-014 in E17, list the actions offered | No action offered removes a reading | R5.6 |
| UJ5.1-e | The seeded file | Open ZX-010, then ZX-011, and list each detail's actions | Neither offers "Show history" | R4.2 |
| UJ5.2-a | The seeded file | Show the collection list and fire "Answer re-scans" | E2 offers "Answer re-scans", and the Data Foundation PRD's E26 renders stating 1 re-scan | R5.7 |
| UJ5.2-b | The seeded file, with T3's reason declared re-measurement | Show the collection list | E2 does not offer "Answer re-scans" | R5.7 |
| UJ5.2-c | The seeded file | Open ZX-013 and fire the Data Foundation PRD's E11 old-reading-was-wrong action, then "Show history" and "Measured order" | T2 carries the never-true mark, and the "measured" variant lists T1 and T3 and states 1 reading left out | R4.6, R5.3 |
| UJ5.2-d | The seeded file; a bulk session in flight on Studio Markers, in each in-flight state | Open ZX-013 and fire the Data Foundation PRD's E11 swatch-has-changed action | E6 does not render; T3's reason reads re-measurement and T2 carries no awaiting-answer mark; the capture PRD's R11.11 reads the seeded queue order and row states | R4.6, R8.3 |
| UJ5.3-a | A collection Hist, chosen condition M1, D50/2°, holding HX-001 with spectral readings H1 (reason initial, L* 50, a* 2.6772, b* −79.7751), H2 (reason re-measurement, 50, −1.3802, −84.2814) and H3 (reason re-measurement, current, 50, 0, −82.7485); published ΔE2000 from H3 of 2.0425 for H1 and 1.0000 for H2 | Open HX-001 and fire "Show history" | H2 shows ΔE2000 1.00 and H1 shows 2.04 | R5.4, R2.11 |
| UJ5.3-b | Hist as UJ5.3-a declares it, except that H1 holds no measurement in the chosen condition M1 | Open HX-001 and fire "Show history" | H1's distance shows the not-compared distance line, and H2 shows 1.00 | R5.4 |
| UJ5.3-c | The seeded file | Open ZX-013, fire "Show history", then "Use this reading" on T1 | E17 lists 4 readings: a new current one with reason restore and measurement time 2026-01-10, then T3, T2, T1; the file holds all 4 | R5.5 |
| UJ5.3-d | The seeded file plus ZX-016 as UJ4.1-d declares it | Open ZX-016 and fire "Show history" | Its quarantined earlier reading offers no "Use this reading" | R5.5 |
| UJ5.3-e | The seeded file plus Studio Markers item ZX-018, Rust: set aside by a flag, its one reading, measured 2026-03-15, in history, no current value | Open ZX-018 and fire "Show history" | Its one reading, the flagged one, offers "Use this reading" | R5.5 |
| UJ5.3-f | The seeded file | Open ZX-012, fire "Show history" and list the actions each reading offers, then fire "Use this reading" on its readable earlier reading | Its quarantined reading offers no "Use this reading" and its earlier reading does; afterwards E17 lists 3 readings, the newest current with reason restore and measurement time 2026-02-01, and ZX-012 carries no unreadable mark | R5.5 |
| UJ5.3-g | The seeded file plus ZX-018 as UJ5.3-e declares it | Open ZX-018, fire "Show history", then "Use this reading" on its one reading | E17 lists 2 readings, the newest current with reason restore and measurement time 2026-03-15; ZX-018's chip shows a colour and carries no no-value mark, and the capture PRD's R11.11 reads its row state captured | R5.5 |
| UJ5.3-h | The seeded file plus ZX-018 as UJ5.3-e declares it; a bulk session in flight on Studio Markers, in each in-flight state | Open ZX-018, fire "Show history", then "Use this reading" on its one reading | E6 renders with no variant; E17 lists 1 reading, and the capture PRD's R11.11 reads ZX-018 set aside | R5.5, R8.3 |
| UJ5.3-i | The state UJ5.3-c leaves | Fire "Measured order" | The "measured" variant lists T1, the restore, T2 and T3 in that order | R5.3 |
| UJ5.3-j | Hist as UJ5.3-a declares it | Open HX-001, fire "Show history", then "Use this reading" on H1 | HX-001's chip renders the working-set value L* 50, a* 2.6772, b* −79.7751 | R5.5 |
| UJ5.3-k | The seeded file, with ZX-005's current reading's snapshot of the live kind and ZX-015's B1 declared of 3 samples with recorded spread 1.2 and verdict agreed | Open ZX-015, fire "Show history", then "Use this reading" on B1 | ZX-015's chip carries the simulated mark and no samples-disagreed mark, its Spread shows 1.20, and the device PRD's E22 is up on Studio Markers | R5.5, R2.6 |
| UJ5.4-a | Hist as UJ5.3-a declares it | Open HX-001, fire "Show history", select H1 and H3, fire "Compare" | Two chips show side by side with their marks, and the distance reads ΔE2000 2.04 | R5.8 |
| UJ5.4-b | Hist as UJ5.3-a declares it | From the keyboard alone, open HX-001's history, choose H1 and H3 and fire "Compare" | Two chips show side by side, and the distance reads ΔE2000 2.04 | R5.8, R8.9 |

### UJ 6. Work on many swatches at once

Scenarios 1 and 2 run in the phase that lands R6.1, R6.2 and R4.7, scenario 3 in the phase that
lands R6.3, and scenario 4 in the phase that lands R4.7, each of its cases that names another row
in the phase that lands that row too; every case runs from the harness state or its own Given.

| Case | Given | When | Assert | Rows |
|---|---|---|---|---|
| UJ6.1-a | The seeded file; Studio Markers chosen | Select ZX-001, then extend the selection by range to ZX-003 | 3 items are selected: ZX-001, ZX-002, ZX-003 | R6.1 |
| UJ6.1-b | The seeded file; Studio Markers chosen; the window shows 5 rows | Type zx-01 and fire "Select all" | 6 items are selected, ZX-015 among them though its row is off screen | R6.1 |
| UJ6.1-c | The seeded file; Studio Markers chosen | Select ZX-001, add ZX-011, then choose the row-state filter value captured | 1 item is selected, ZX-001 | R6.1, R6.4 |
| UJ6.2-a | The seeded file; Studio Markers chosen | Select ZX-001 and ZX-002, fire "Set a field", choose Family and enter Brights | E8 does not render; the file holds Family Brights for ZX-001 and ZX-002 and Pink for ZX-003 | R6.2 |
| UJ6.2-b | The seeded file; Studio Markers chosen | Select ZX-001 through ZX-013 by range, fire "Set a field", choose Family and leave the value empty, then fire "Cancel" | E8 renders its "clear" variant naming Family and 11 swatches; afterwards the file holds Family Blue for ZX-001 and Green for ZX-013 | R6.2 |
| UJ6.2-c | The seeded file; Studio Markers chosen | Select ZX-001 through ZX-013 by range, fire "Set a field", choose Family and leave the value empty, then fire "Apply to ⟨n⟩ swatches" with ⟨n⟩ rendering 11 | The file holds an empty Family for those 11 items, Purple for ZX-014 and Red for ZX-015, and 16 Studio Markers readings, and its bytes hold the texts Pink and Yellow nowhere | R6.2 |
| UJ6.2-d | The seeded file; Studio Markers chosen | Select ZX-001 through ZX-013 by range, fire "Set a field", choose Family and enter Set A | E8 renders with no variant naming Family, Set A and 11 swatches | R6.2 |
| UJ6.2-e | The seeded file; Studio Markers chosen | Select ZX-001 and ZX-002 and fire "Set a field" | The fields offered are Swatch Name, Swatch Alternate Code, Swatch Alternate Name and Family, and Swatch Code is not among them | R6.2 |
| UJ6.2-f | The seeded file after the 11-item clear UJ6.2-c's When makes | Fire "Undo change" | The file holds Family Blue for ZX-001, Green for ZX-002 and Yellow for ZX-012 | R4.7 |
| UJ6.2-g | The seeded file; Studio Markers chosen; the volume holding the file declared full | Select ZX-001 and ZX-002, fire "Set a field", choose Family and enter Brights | The Data Foundation PRD's E15 renders, and the file holds Family Blue for ZX-001 and Green for ZX-002 | R8.8 |
| UJ6.2-h | The seeded file; Studio Markers chosen | Select ZX-001 through ZX-012 by range, fire "Set a field", choose Family and enter Set B | E8 does not render; the file holds Set B for those 10 items and Green for ZX-013 | R6.2 |
| UJ6.3-a | The seeded file; Studio Markers chosen; R6.3 built | Select ZX-013 and ZX-014, fire "Delete selected", then press Return | The Data Foundation PRD's E33 renders naming 2 swatches; after Return it is not up, the table lists 13 items, and the file holds ZX-013's 3 readings and ZX-014's 2 | R6.3, R1.6 |
| UJ6.3-b | The seeded file; Studio Markers chosen; R6.3 built | Select ZX-013 and ZX-014, fire "Delete selected", then the Data Foundation PRD's E33 delete action | That E33 names 2 swatches, 2 current readings and 3 earlier readings; afterwards E3 states 11 swatches, the file read with the app closed holds neither item and 11 Studio Markers readings, and its bytes hold the texts Teal and Plum nowhere | R6.3 |
| UJ6.3-c | The seeded file; R6.3 built; a bulk session in flight on Studio Markers, in each in-flight state | Select ZX-010 and ZX-011 and fire "Delete selected" | E6 renders with no variant, and the table lists 13 items | R6.3, R8.3 |
| UJ6.3-d | The seeded file; R6.3 and R1.7 built | Select ZX-010 and ZX-011, fire "Delete selected", then the Data Foundation PRD's E33 delete action, then "Undo" on E10 | That E33 names 2 swatches and no reading; E10 renders its "swatches" variant stating 2 and naming Studio Markers; after Undo the table lists 13 items, ZX-010 and ZX-011 among them | R1.7, R6.3 |
| UJ6.3-e | The seeded file; Studio Markers chosen; R6.3 built | Select ZX-013 and ZX-014, fire "Delete selected", then the Data Foundation PRD's E33 export-first action, then close the export | The Data Export PRD's E1 renders for Studio Markers at collection scope; after it closes that E33 is up again naming 2 swatches, and the file holds ZX-013 and ZX-014 with their 5 readings | R6.3 |
| UJ6.4-a | The seeded file | Open ZX-001, set Swatch Name to Harbour and press Return, then fire "Undo change" | The file holds Sky Blue for ZX-001 | R4.7 |
| UJ6.4-b | The seeded file | Open ZX-001, set Family to Teal Blue and press Return, then Swatch Name to Harbour and press Return; fire "Undo change", then fire it again | After the first undo the file holds Sky Blue and Teal Blue for ZX-001; after the second, Sky Blue and Blue | R4.7 |
| UJ6.4-c | The seeded file; R4.4 built | Open ZX-001, change its code to ZX-100 through E16's "Change the code", then fire "Undo change" | Studio Markers holds ZX-001 with its 1 reading and no ZX-100 | R4.7, R4.4 |
| UJ6.4-d | The seeded file; R4.8 built | Rename Family to Hue Family through E19's "Rename the column", then fire "Undo change" | The file stores the column as Family, ZX-001's value Blue | R4.7, R4.8 |
| UJ6.4-e | The seeded file | Choose Gouache Set, rename it Gouache Travel Set, then fire "Undo change" | The file names the collection Gouache Set | R4.7, R1.3 |
| UJ6.4-f | The seeded file, in separate runs | Set ZX-001's Swatch Name to Harbour and press Return, then, one per run: delete ZX-010 through the Data Foundation PRD's E8 delete action; Flag ZX-002 through E18's "Set aside to scan again"; restore ZX-012 through the Data Foundation PRD's E4 action; drag ZX-010 above ZX-001; answer ZX-013's re-scan through the Data Foundation PRD's E11 action; import into Studio Markers a CSV holding ZX-001; read the file again through the Data Foundation PRD's E9 action; then list the actions offered | "Undo change" is not offered after any of them, and the file holds Harbour for ZX-001 | R4.7 |
| UJ6.4-g | The seeded file; R4.4 built | Change ZX-001's code to ZX-100 through E16, then import into Studio Markers a CSV whose code column holds ZX-001, keeping the new details; list the actions offered | "Undo change" is not offered, and Studio Markers holds ZX-100 and a pending ZX-001 | R4.7 |
| UJ6.4-h | The state UJ4.6-f leaves; R4.8 built | List the actions offered | "Undo change" is not offered, and Studio Markers holds one column named Hue Family and one named Family | R4.7 |
| UJ6.4-i | The seeded file; R4.4 built | Change ZX-001's code to ZX-100 through E16; bring a bulk session on Studio Markers in flight, in each in-flight state; fire "Undo change" | E6 renders with no variant, and Studio Markers holds ZX-100 and no ZX-001 | R4.7, R8.3 |
| UJ6.4-j | The seeded file | Select ZX-003, ZX-011 and ZX-012, fire "Set a field", choose Family and leave the value empty; with "Undo change" still offered, read the bytes of the file and of what sits beside it, and the app's own storage; then fire "Undo change" | Neither read holds the text Pink or Yellow; afterwards the file holds Pink for ZX-003 and Yellow for ZX-011 and ZX-012 | R4.7, R8.10 |
| UJ6.4-k | The seeded file | Select ZX-003, ZX-011 and ZX-012, fire "Set a field", choose Family and leave the value empty; crash the app; before reopening, read the bytes of the file and of what sits beside it, and the app's own storage; reopen and list the actions offered | Neither read holds the text Pink or Yellow; after reopening "Undo change" is not offered and the three hold an empty Family | R4.7, R8.10 |

### UJ 7. Browse every collection at once

Scenario 1 runs in the phase that lands R1.9 and R1.10, each case from the harness state or its own
Given.

| Case | Given | When | Assert | Rows |
|---|---|---|---|---|
| UJ7.1-a | The seeded file | Fire "All items" | E13 renders with no variant stating 15 swatches across 2 collections; the table lists Gouache Set's ZX-001 and GS-002, then Studio Markers' 13 items in queue order, under the columns chip, Swatch Code, Swatch Name, row state, L*, C*, h°, Spread, Swatch Alternate Code, Swatch Alternate Name and Collection; ZX-001 appears twice, once as Cerulean in Gouache Set and once as Sky Blue in Studio Markers | R1.9 |
| UJ7.1-b | The seeded file | Fire "All items" and type zx-001; then fire "Filters" and choose the row-state value captured | The table lists two rows, Gouache Set's ZX-001 and Studio Markers' ZX-001, and E13 renders its "narrowed" variant stating 2 of 15; after the filter it still lists both, E13 offering "Clear search" and "Clear filters" | R1.9, R3.4 |
| UJ7.1-c | The seeded file | Fire "All items" and list the actions offered | "Filters" and "Colour marks" are offered; "Answer re-scans", "Select all", "Set a field", "Delete selected", "Use as scan order", "Rename collection", "Delete collection" and "Export collection" are not, and no row can be dragged | R1.10, R5.7 |
| UJ7.1-d | The seeded file | Fire "All items", open Gouache Set's ZX-001, set Swatch Name to Cerulean Deep and press Return | The file holds Cerulean Deep for Gouache Set's ZX-001 and Sky Blue for Studio Markers' ZX-001 | R1.10, R4.3 |
| UJ7.1-e | The seeded file | Fire "All items", then the L* header | The table lists ZX-014, ZX-015, ZX-005, GS-002, ZX-003, Gouache Set's ZX-001, ZX-013, Studio Markers' ZX-001, ZX-002, ZX-004, ZX-007, ZX-006, ZX-010, ZX-011, ZX-012 | R1.9, R3.2 |
| UJ7.1-f | The seeded file | Fire "All items" | The device PRD's E22 is not up, and ZX-005's chip carries the simulated mark | R1.10 |
| UJ7.1-g | The seeded file | Fire "All items", open Gouache Set's ZX-001, fire "Delete swatch", then the Data Foundation PRD's E8 delete action | That E8 names ZX-001 and Gouache Set; afterwards the file holds no Gouache Set item ZX-001, and Studio Markers' ZX-001 with its 1 reading | R1.10, R4.5 |
| UJ7.1-h | A file holding two collections, Empty One and Empty Two, and no item | Fire "All items" | E13 renders with no variant, its ⟨collections⟩ reading 2 and its ⟨n⟩ 0, and the table lists no item | R1.9 |
| UJ7.1-i | The seeded file plus a collection Daylight, chosen condition M1, D65/10°, holding DL-001, captured, its working-set value L* 20, C* 10, h° 90 | Fire "All items", then the L* header | The table lists UJ7.1-e's valued items in its order, then DL-001, then ZX-006, ZX-010, ZX-011, ZX-012, and E13's ⟨unlike⟩ reads 1 | R1.9, R3.3 |
| UJ7.1-j | The seeded file | Fire "All items" and type gouache | The table lists Gouache Set's ZX-001 and GS-002 | R1.10 |
| UJ7.1-k | The seeded file | In Studio Markers type zx-01 and choose the mark filter value simulated; fire "All items" and type gs; choose Studio Markers; then fire "All items" again | Back in Studio Markers the search field holds zx-01 with the simulated filter active; back in All items its search field holds gs and no filter is active | R3.6 |

### UJ 8. Reorder the scan queue from the collection

Scenario 1 runs in the phase that lands R2.9, each case from the harness state or its own Given,
under the capture PRD's REORDER_SCOPE interim.

| Case | Given | When | Assert | Rows |
|---|---|---|---|---|
| UJ8.1-a | The seeded file; Studio Markers chosen, the table in queue order | Drag ZX-010 above ZX-001 | The queue order the capture PRD's R11.11 reads begins ZX-010, ZX-001, ZX-002 | R2.9 |
| UJ8.1-b | The seeded file; Studio Markers chosen, the table in queue order | Drag ZX-001 below ZX-010 | The capture PRD's E33 renders its scanned variant, and the queue order begins ZX-001, ZX-002 | R2.9 |
| UJ8.1-c | The seeded file, with Studio Markers' queue order manually set by a drag to begin ZX-010, ZX-001 | Fire the Swatch Name header, fire "Use as scan order", then the capture PRD's E34 keep-my-order action | E34 renders; afterwards the queue order begins ZX-010, ZX-001 | R2.9 |
| UJ8.1-d | The seeded file plus two pending Studio Markers items appended after ZX-015 by import, ZX-020 Amber and ZX-021 Aqua | Fire the Swatch Name header, then "Use as scan order" | E34 does not render; the queue order reads ZX-001, ZX-002, ZX-003, ZX-004, ZX-005, ZX-006, ZX-007, ZX-020, ZX-011, ZX-012, ZX-013, ZX-014, ZX-015, ZX-021, ZX-010 | R2.9 |
| UJ8.1-e | The seeded file; Studio Markers view-sorted by L* | List the actions offered and try to drag ZX-010 | "Use as scan order" is not offered and no row can be dragged | R2.9 |
| UJ8.1-f | The seeded file; a bulk session in flight on Studio Markers, in each in-flight state, the table in queue order | List the actions offered and try to drag ZX-010 | "Use as scan order" is not offered and no row can be dragged | R2.9, R8.3 |
| UJ8.1-g | The seeded file; Studio Markers chosen, the table in queue order | From the keyboard alone, move ZX-010 above ZX-001 | The queue order the capture PRD's R11.11 reads begins ZX-010, ZX-001, ZX-002 | R2.9, R8.9 |
| UJ8.1-h | The seeded file; Studio Markers chosen; in separate runs the search zx-01 typed or the row-state filter value pending chosen, each run once in queue order and once view-sorted by Swatch Name | List the actions offered and try to drag ZX-010 | In no run is "Use as scan order" offered or any row draggable | R2.9 |
| UJ8.1-i | The seeded file; Studio Markers chosen, the table in queue order | Drag ZX-011 above ZX-001 | The capture PRD's E33 renders its set-aside variant, and the queue order the capture PRD's R11.11 reads is the seeded one | R2.9 |

### UJ 9. Browse while the app does other things

Scenario 1 through scenario 8 run in the first build phase, each case from the harness state or its
own Given, except that UJ9.5-b, UJ9.5-e and UJ9.7-b to UJ9.7-d run in the phase that lands the rows
they name, and UJ9.5-a's grid inputs in R7's phase. UJ9.2-a, UJ9.2-c, UJ9.4-a, UJ9.4-c and UJ9.6-b
run again in every later build phase, over the actions that phase adds.

| Case | Given | When | Assert | Rows |
|---|---|---|---|---|
| UJ9.1-a | The seeded file; a bulk session in flight on Studio Markers with ZX-010 current, the Demo Device connected; the table searched zx-01, view-sorted by Swatch Name, ZX-013 selected, its row first on screen | Capture saves a 3-sample Demo Device set on ZX-010; then show Studio Markers' collection surface | Within 5 s, a functional timeout (timing is UJ9.5-a's), ZX-010 lists as captured with a chip carrying the simulated mark; the search field holds zx-01, the sort is by Swatch Name, ZX-013 is selected and its row is first on screen | R8.1, R8.3 |
| UJ9.1-b | The seeded file; a bulk session in flight on Studio Markers with ZX-010 current and remembered | Open ZX-001, set Swatch Name to Sky and press Return | The file holds Sky for ZX-001; the capture PRD's R11.11 reads the current row ZX-010, the remembered row ZX-010, and the seeded queue order | R8.3 |
| UJ9.1-c | The seeded file; a bulk session in flight on Studio Markers with ZX-010 current, the Demo Device connected; the row-state filter value pending chosen and ZX-010 selected | Capture saves a 3-sample Demo Device set on ZX-010; then show Studio Markers' collection surface | The table lists no item and E5 renders; the pending filter stays chosen, and no item is selected | R3.9, R6.4 |
| UJ9.1-d | The seeded file; Studio Markers searched sky, ZX-001 selected | Open ZX-001, set Swatch Name to Harbour, press Return and close the detail | The table lists no item and E4 renders naming sky; no item is selected | R3.9, R6.4 |
| UJ9.2-a | The seeded file, in a format this build reads, declared open read-only (the Data Foundation PRD's R7.2) | List the collection list, Studio Markers' table and marks, ZX-001's detail, ZX-013's history, and every action offered | 13 items list with the marks UJ2.1-b lists; E14 renders its "read-only" variant with its lines, and the history shows its lines; every action that writes the file — "New collection", "Rename collection", "Delete collection", "Delete swatch", "Answer re-scans", field editing and, in later phases, every P1 and P2 write action, hiding a column included — shows disabled, none absent | R8.4 |
| UJ9.2-b | The seeded file | In Studio Markers type zx-01, fire the L* header, then open ZX-013 and set Swatch Name to Deep Teal | The table lists ZX-014, ZX-015, ZX-013, ZX-010, ZX-011, ZX-012, and the file holds Deep Teal for ZX-013 | R8.4 |
| UJ9.2-c | A file of a newer format, opened read-only in the Data Foundation PRD's E1 state | List the collection list, a collection's table, one item's detail and its history, and every action offered | The item list, current values and history marks show, as the Data Foundation PRD's R5.3 guarantees, and every write action shows disabled | R8.4 |
| UJ9.3-a | The seeded file; Studio Markers searched zx-01, the row-state filter values captured and set aside chosen, view-sorted by L*, ZX-013 selected; then, outside the app, ZX-011 removed from the file and ZX-013 renamed Deep Teal | Fire the Data Foundation PRD's E9 read-again action | The table lists ZX-014, ZX-015, ZX-013, ZX-012 with ZX-013 named Deep Teal; the search, both filter values and the L* sort are kept, and ZX-013 is selected | R8.5 |
| UJ9.3-b | The seeded file; ZX-013's detail and history open; then, outside the app, ZX-013 removed from the file | Fire the Data Foundation PRD's E9 read-again action | Neither E14 nor E17 is up, and the table lists 12 items | R4.1, R8.5 |
| UJ9.4-a | The seeded file; network unreachable through the device PRD's R6.12 | Choose Studio Markers, type zx-01, open ZX-001, set Swatch Name to Sky and press Return, then fire "Show history" on ZX-013 | The table lists 6 items, the file holds Sky for ZX-001, E17 lists 3 readings, and the device PRD's R6.17 record holds no outbound attempt | R8.6 |
| UJ9.4-b | The seeded file | In Studio Markers type qqq, choose the mark value simulated and fire the L* header; close the app, then read the file's bytes and the app's own storage | Neither holds the text qqq, and the file holds no record of the filter or the sort | R8.6 |
| UJ9.4-c | The seeded file; network reachable, telemetry and update checks off | Deliver every action this PRD offers in the phase — searches, filters, sorts, Find similar in the All items view, renames, edits, bulk set and clear, deletes, the export entries and column visibility | The device PRD's R6.17 records no attempt beyond the Data Foundation PRD's R1.4 permitted set for that configuration, and none carrying text a case typed | R8.6 |
| UJ9.5-a | A file whose collection Scale holds ROWS_CEILING items generated as the Data Foundation PRD's R7.7 corpus, with IMPORTED_COLUMNS_CEILING imported columns of 200-character values, the first named Family; a Release build on the Mac OQ 1's interim names, internal disk | Deliver the named timing workload's 200 search keystrokes, 200 filter changes and 200 header fires; 200 each of selecting one row, extending a range and "Select all", and of opening and closing an item detail and a version history view; page through the table end to end; choose Scale 20 times from the collection list, the first after launch; have the Demo Device save 200 sets into Scale while it is searched, filtered and view-sorted; move the window between an sRGB and a Display P3 display 20 times with the cannot-show filter on; clear Family across every Scale item, then fire "Undo change"; in R7's phase, switch "Table" and "Grid" 20 times and change the swatch size 20 times | The 95th percentile of each input kind is at or below BROWSE_RESPONSE_BUDGET, saves and moves timed from when they land; no more than DROPPED_FRAME_SHARE of frames are missed while paging; every choice of Scale shows its first rows within OPEN_COLLECTION_BUDGET; the clear and its undo each show progress, the surface meeting BROWSE_RESPONSE_BUDGET throughout, and each is done within BULK_WRITE_BUDGET | R8.1, M1 |
| UJ9.5-b | A file of FILE_ITEMS_CEILING items split evenly across ten collections — nine at M1, D50/2°, one at M1, D65/10°; a Release build on the Mac OQ 1's interim names; R1.9, R3.7 and R8.2 built | Fire "All items" 20 times, the first after launch; deliver the named workload's search keystrokes, filter changes and header fires in the All items view, 200 of each; fire "Find similar" 50 times from the All items view on items of the D50/2° collections | Every "All items" shows its first rows within OPEN_COLLECTION_BUDGET, and the 95th percentile of each other input is at or below BROWSE_RESPONSE_BUDGET | R8.2 |
| UJ9.5-c | A file whose collection Double holds twice ROWS_CEILING items, with one more than IMPORTED_COLUMNS_CEILING imported columns | Search, filter, sort, open an item, set a field on every item, and delete one item | Each completes, and no item, column or value is refused or truncated | R8.1 |
| UJ9.5-d | Scale as UJ9.5-a declares it; a bulk session in flight on Scale on the Demo Device at real pacing (the capture PRD's R11.7); a Release build on the Mac OQ 1's interim names | While capture saves 20 sets, select every Scale item and clear Family | Every trigger acknowledgement lands within TRIGGER_ACK_WINDOW and every row confirmation within ROW_CONFIRM_BUDGET, as the capture PRD's R11.7 reads them | R8.11 |
| UJ9.5-e | The seeded file plus Studio Markers item LH-001, Long, captured, holding 500 readings; R8.2 built | Open LH-001 and fire "Show history" | E17's ⟨n⟩ reads 500 and it lists 500 readings | R8.2 |
| UJ9.6-a | The seeded file | Read each mark's shape and VoiceOver name, and the accessible description of ZX-003's row | The eleven shapes the Mark labels table lists are pairwise distinct; ZX-003's description names ZX-003, Neon Magenta and captured, and the marks cannot-show and outside-sRGB by identifier | R8.9 |
| UJ9.6-b | The seeded file | From the keyboard alone, deliver every action E2, E3, E14 and E17 offer in the build phase | Each action fires | R8.9 |
| UJ9.7-a | The seeded file | Open ZX-001, set Swatch Name to Sky, press Return, then lose everything not yet safely written through the Data Foundation PRD's R7.4 and reopen | The file holds Sky for ZX-001 | R8.8 |
| UJ9.7-b | The seeded file; R6.1 and R6.2 built | Select ZX-001 through ZX-013, fire "Set a field", choose Family and leave the value empty, fire "Apply to ⟨n⟩ swatches", and crash the app after the first item's update and before the 11th's (the Data Foundation PRD's R7.4); reopen | Either all 11 items hold an empty Family, or all 11 hold their seeded values; no item of the 11 differs from the rest | R8.8 |
| UJ9.7-c | The seeded file plus ZX-020 and ZX-021 as UJ8.1-d declares them; R2.9 built | Fire the Swatch Name header and "Use as scan order", and crash the app after the first position is rewritten and before the last; reopen | The queue order the capture PRD's R11.11 reads is either the seeded one or UJ8.1-d's, never a mix | R8.8 |
| UJ9.7-d | The seeded file; R5.5 built | Fire "Use this reading" on ZX-013's T1, crash the app during the write, and reopen | ZX-013 holds either 3 readings with T3 current or 4 with the restore current, never part of a restore | R8.8 |
| UJ9.7-e | The seeded file; the volume holding it declared gone | Open ZX-001, set Swatch Name to Harbour and press Return | The capture PRD's E26 renders, and once the volume is back the file holds Sky Blue for ZX-001 | R8.8 |
| UJ9.7-f | The seeded file; another copy of the app declared holding the file | Open ZX-001, set Swatch Name to Harbour and press Return | The Data Foundation PRD's E10 renders, and the file holds Sky Blue for ZX-001 | R8.8 |
| UJ9.8-a | The seeded file under each of the display gamuts sRGB, Display P3 and none reported | List every chip's marks | Every listed mark agrees with the conditions the harness declares, for 15 items under each gamut: M2 reads 100% | R2.4, R8.10, M2 |
| UJ9.8-b | The seeded file, its 18 readings read at SQLITE_READER_FLOOR before any action | Run the Whens of UJ4.2-a and UJ5.2-c, and, in the phases that land their rows, of UJ5.3-c, UJ4.3-a, UJ6.2-c, UJ8.1-a, UJ4.6-a and UJ4.7-a; read the file after each | Each of the 18 seeded readings reads back after every action, so M3 counts 0 | R8.10, M3 |

### UJ 10. See the collection as a swatch grid

Scenario 1 runs in the phase that lands R7.1 and R7.2, each case from the harness state or its own
Given.

| Case | Given | When | Assert | Rows |
|---|---|---|---|---|
| UJ10.1-a | The seeded file; Studio Markers searched zx-01, view-sorted by L*, ZX-013 selected | Fire "Grid", then "Table" | The grid lists ZX-014, ZX-015, ZX-013, ZX-010, ZX-011, ZX-012 as swatches no smaller than MIN_SWATCH_SIZE, each labelled with its code and carrying the marks UJ2.1-b lists, with ZX-013 selected; back in the table the same six rows list in that order with ZX-013 selected | R7.1 |
| UJ10.1-b | The seeded file; Studio Markers shown as a grid | Set the swatch size as small as it goes | Every swatch measures MIN_SWATCH_SIZE on a side | R7.2 |
| UJ10.1-c | The seeded file; Studio Markers shown as a grid | List ZX-010's swatch | It is an empty chip carrying the no-value mark | R7.2 |
| UJ10.1-d | The seeded file; Studio Markers shown as a grid at a swatch size above MIN_SWATCH_SIZE | Close the file, read the app's own storage and the file, then reopen the file and choose Studio Markers | Neither read holds the grid choice or the swatch size, and after reopening Studio Markers shows as the table | R7.1, R7.2, R8.6 |
| UJ10.1-e | The seeded file; Studio Markers in queue order, no item selected, ZX-005's row first on screen | Fire "Grid", then "Table" | In the grid ZX-005's swatch is the first on screen, and back in the table ZX-005's row is first on screen | R7.1 |

## Test-controls map
<!-- guidance: the last section, always. One line per surface: the input a test controls to drive
     that surface, the result it can observe, and the rows that grant both. Not conditional.
     This map is also what makes the harness statement's "declare every exercised seam input"
     checkable: the mechanical checks reconcile every case's `When` cell against it. -->

Every surface a case in this file drives appears in this map, or in the Harness's **Named
defaults** table. A case that drives a surface named in neither is asserting through a seam nobody
declared.

| Surface | Controlled input | Observable result | Rows |
|---|---|---|---|
| collection list | Showing it, choosing a collection, "New collection", "All items", "Answer re-scans" | E1 or E2, the collections listed with their counts in order, which actions are offered | R1.1, R1.2, R5.7 |
| collection surface | The search field's text, filter values, header fires, drags and their keyboard route, one-row, range and toggle selection, hiding and showing a column, "Rename collection" and "Delete collection" with the names a case enters, Escape during a rename, "Select all", "Set a field", "Delete selected", "Delete swatch", "Use as scan order", "Rename column", "Export collection", "Colour marks", "Undo change", "Clear search", "Clear filters", "Grid", "Table", the swatch size, and the actions of E4–E12 and E19 | The rows or swatches in order, their columns and displayed values, each chip's marks, fill and triplet, accessible descriptions, which copy state and variant is up with its token values, the selection and its count, the first row on screen, which actions are offered or disabled | R1.3–R1.8, R2.1–R2.11, R3.1–R3.9, R4.7, R4.8, R6.1–R6.4, R7.1, R7.2, R8.9 |
| All items view | "All items", its search field, filters, header fires, "Clear search", "Clear filters", "Colour marks", opening an item, "Find similar" and choosing an item E9 lists | E13 and its variant with its token values, the rows listed with their collections, E9's list, which actions are offered, whether the device PRD's E22 is up | R1.9, R1.10, R3.6, R3.7 |
| item detail | Opening and closing an item, field edits confirmed by Return or leaving the field or discarded by Escape, "Change code" with the code a case enters, "Change the code", "Keep this code", "Try another code", "Delete swatch", "Export swatch", "Find similar", "Show history", "Colour marks", "Undo change", the capture PRD's Flag action, E18's "Set aside to scan again" and "Cancel", and the actions of the sibling states it shows | E14's lines and variant, E15, E16 or E18 and their variants, the sibling state shown, which actions are offered or disabled | R2.8, R4.1–R4.9 |
| version history view | "Show history", "Recorded order", "Measured order", "Use this reading", "Colour marks", choosing two readings, by pointer or keyboard, "Compare" | The readings listed in order with their lines, marks and distances, E17 and its variant, which actions each reading offers | R2.8, R5.1–R5.8 |
| sibling states opened here | The Data Foundation PRD's E4, E8, E9, E11, E14, E15, E26 and E33 actions, the capture PRD's E1, E23, E25, E28, E33 and E34 actions and its start, creation, re-scan entry and set-aside list, the device PRD's E22 action, the Data Export PRD's E1, and an import into a collection | Which sibling state is up, what it names, and what the file and the capture PRD's R11.11 read afterwards | R1.2–R1.5, R1.8, R2.6, R2.9, R4.5, R4.6, R4.9, R5.7, R6.3, R8.5 |
| display | The declared gamut of each display, any gamut a display reports, a change to it, moving the window from one display to another, and a window across two displays with the one macOS reports it on | Which chips carry the cannot-show mark, and when they change | R2.5, R8.7 |
| the file | Its seeded contents, a declared outside change, a read-only open, a full or vanished volume, another copy of the app holding it, an induced loss of unwritten data or a crash at a declared moment, closing and reopening it, quitting and relaunching the app | Its contents read with the app closed at SQLITE_READER_FLOOR, and the bytes of it and of what sits beside it, read closed, open or after a crash | R2.10, R4.3, R4.7, R8.4, R8.5, R8.8, R8.10 |
| the app's own storage | Reading it with the app closed or after a crash | What the app keeps outside the file — preferences, container, saved window state | R4.7, R7.1, R7.2, R8.6, R8.10 |
| sessions | A declared session state on a collection, at the start of a case or at a step that says so, in each in-flight state, and a set capture saves on the Demo Device while a case runs, at real pacing where a case says so | The capture PRD's R11.11 readback of queue order, row states, current and remembered row, and its R11.7 timings | R8.3, R8.10, R8.11 |
| network | Reachability declared through the device PRD's R6.12, and the telemetry and update-check settings | The device PRD's R6.17 record of outbound attempts | R8.6 |
| accessibility and keyboard | Actions delivered from the keyboard alone, the drag and the choice of two readings included; reading each mark's shape and VoiceOver name and each row's accessible description | Whether each action fired; the shapes, names and descriptions | R8.9 |
| timing | The named timing workload delivered against a declared build configuration on the Mac OQ 1's interim names | The time from each input to the first frame showing its result, and the frames missed while paging | R8.1, R8.2, R8.11 |
