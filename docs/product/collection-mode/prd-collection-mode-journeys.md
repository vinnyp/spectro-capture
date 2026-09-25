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
(the Data Foundation PRD's R7.2, R8.10), a declared display gamut, and a declared session state on
a collection (the capture PRD's R11.6); the device PRD's Demo Device supplies a reading only where a
case needs one that capture saves while the case runs, and nothing is measured by an instrument. A
case asserts through what R8.10 lets a test list — the collection list's and the table's rows in
order, their columns, each chip's marks and accessible description, the item detail's lines, the
version history view's readings, and which copy state and variant is up, without matching wording —
through the file read with the app closed at SQLITE_READER_FLOOR (the Data Foundation PRD's R7.1),
through the capture PRD's R11.11 readback of queue order, row states and a session's current and
remembered row, through the device PRD's R6.17 record of outbound attempts, and through R8.1's
timing readback. Every fixture here is synthetic: invented collections, codes, names, serials and
dates. Declare every exercised seam input; only named defaults are exempt. Fixture values are
declared, never copied from the implementation.

**Fixture continuity.** Every case runs against a fixture reset to this harness's declared state,
unless the journey's preamble line declares continuity across that journey's cases; where it does,
each case in that journey runs against the state the previous case left. Continuity is declared in
the preamble line or it does not hold — a case never carries state no preamble granted it, and a
builder reading a case table needs no other source to know which of the two applies.

**Named defaults** (the seam inputs a case may leave undeclared, each with its default value):

| Seam input | Default | Rows |
|---|---|---|
| The file | One file holding exactly two collections, Studio Markers and Gouache Set, as the lines below declare them, changed by nothing outside the app | R8.10 |
| Studio Markers | Chosen condition M1, illuminant and observer D50/2°, 3 samples per row, one imported column named Family; 13 items holding 16 readings, in the queue order ZX-001, ZX-002, ZX-003, ZX-004, ZX-005, ZX-006, ZX-007, ZX-010, ZX-011, ZX-012, ZX-013, ZX-014, ZX-015, set by import with no manual reorder | R2.1, R8.10 |
| Studio Markers item ZX-001 | Swatch Name Sky Blue; alternate code B-12; alternate name Azure; Family Blue; captured; one spectral reading measured 2026-02-03 by a live Spectro 2, serial SN-100200, firmware 2.1.0, with 3 samples, averaging basis spectral curves, recorded spread ΔE2000 0.4 and agreement verdict agreed; current value L* 62, C* 36, h° 254; inside sRGB and inside Display P3 | R8.10 |
| Studio Markers item ZX-002 | Signal Green; Family Green; captured; one live spectral reading; L* 70, C* 90, h° 150; gamut-clipped flag set; inside Display P3 | R8.10 |
| Studio Markers item ZX-003 | Neon Magenta; Family Pink; captured; one live spectral reading; L* 55, C* 110, h° 340; gamut-clipped flag set; outside Display P3 | R8.10 |
| Studio Markers item ZX-004 | Cool Grey 3; Family Grey; captured; one live reading marked non-spectral, worked out at the collection's reference; L* 75, C* 1.5, h° 250; inside sRGB | R8.10 |
| Studio Markers item ZX-005 | Demo Red; Family Red; captured; one reading whose acquiring snapshot is the Demo Device's, of the simulated kind, with 3 samples, recorded spread ΔE2000 3.1 and agreement verdict disagreed, average accepted; L* 48, C* 60, h° 30; inside sRGB | R8.10 |
| Studio Markers item ZX-006 | Deep Navy; Family Blue; captured; one live spectral reading with no measurement in the chosen condition, so its working-set value is absent and marked | R8.10 |
| Studio Markers item ZX-007 | Chalk White; Family White; captured; one live spectral reading of 1 sample; L* 95, C* 2.5, h° 90; inside sRGB | R8.10 |
| Studio Markers item ZX-010 | Warm Grey 1; Family Grey; pending; never scanned | R8.10 |
| Studio Markers item ZX-011 | Lemon; Family Yellow; set aside, unsettled, cause light leak; no reading | R8.10 |
| Studio Markers item ZX-012 | Ochre; Family Yellow; set aside, unsettled, cause unreadable; its current reading quarantined; one readable earlier reading measured 2026-02-01 | R2.4h, R8.10 |
| Studio Markers item ZX-013 | Teal; Family Green; captured; three live spectral readings recorded in this order: T1, reason initial, measured 2026-01-10; T2, reason re-measurement, measured 2026-03-02; T3, reason correction-unconfirmed, measured 2026-05-20, current; current value L* 60, C* 40, h° 190; inside sRGB | R8.10 |
| Studio Markers item ZX-014 | Plum; Family Purple; captured; two live spectral readings: P1, reason initial, measured 2026-01-12, marked never true; P2, reason correction, measured 2026-04-04, current; L* 35, C* 45, h° 320; inside sRGB | R8.10 |
| Studio Markers item ZX-015 | Brick; Family Red; captured; two spectral readings: B1, reason initial, measured 2026-02-10, from the Demo Device, of the simulated kind; B2, reason re-measurement, measured 2026-06-15, live, current; L* 45, C* 50, h° 35; inside sRGB | R8.10 |
| Gouache Set | Chosen condition M1, D50/2°, no imported column; 2 items holding 2 readings, in the queue order ZX-001, GS-002 | R8.10 |
| Gouache Set item ZX-001 | Cerulean; captured; one live spectral reading; L* 58, C* 40, h° 240; inside sRGB | R8.10 |
| Gouache Set item GS-002 | Cadmium Red; captured; one live spectral reading; L* 50, C* 70, h° 35; inside sRGB | R8.10 |
| Display | One display, reporting the sRGB gamut, showing the window | R2.5, R8.10 |
| Sessions | No session on any collection | R8.3, R8.10 |
| App preferences | None set: every table column is shown | R2.10, R8.10 |
| Build phase | The first build phase — P0 rows only — unless a journey's preamble or a case's Given names a later one | R8.10 |
| Named constants | Each at the candidate the PRD's constants table gives | R3.3, R3.7, R6.2, R7.1 |
| Clock | 2026-09-01T10:00:00Z, advanced only by a case step that says so | R8.10 |
| Network | Reachable | R8.6 |

## Transition index
<!-- guidance: `T1..Tn`, one case per route through the entity lifecycle — the executable
     counterpart of the PRD's row-transitions table. Enumerate these from that table and from the
     rows, not from memory: a route the PRD's transition table names and this index omits is a
     defect, and so is the reverse. -->

| Case | Given | When | Assert | Rows |
|---|---|---|---|---|
| T1 | Studio Markers item ZX-010 present; first build phase | Fire "Delete swatch" on ZX-010, then the Data Foundation PRD's E8 delete action | ZX-010 is deleted: the table lists 12 items without it, and the file read with the app closed holds no item ZX-010 | R4.5 |
| T2 | ZX-010 present; R1.7 built, OQ 10 closed | Fire "Delete swatch" on ZX-010, then E8's delete action | ZX-010 is deleted-undoable: E10 renders with no variant naming ZX-010, and the table lists 12 items without it | R1.7 |
| T3 | ZX-010 deleted-undoable, as T2's When leaves it; R1.7 built | Fire "Undo" on E10 | ZX-010 is present: the table lists 13 items, with ZX-010 pending at queue position 8 | R1.7 |
| T4 | ZX-010 deleted-undoable, as T2's When leaves it; R1.7 built | Close the file and reopen it | ZX-010 is deleted: the table lists 12 items, E10 is not up, and the file holds no item ZX-010 | R1.7 |
| T5 | Gouache Set present | Fire "Rename collection" on Gouache Set and enter Gouache Travel Set | Gouache Travel Set is present with its 2 items, and the file names it Gouache Travel Set | R1.3 |
| T6 | Studio Markers item ZX-001 present | In ZX-001's detail set Swatch Name to Sky Blue Light and press Return | ZX-001 is present, the file holds Swatch Name Sky Blue Light for it, and it holds 1 reading | R4.3 |
| T7 | Studio Markers item ZX-001 present; R4.4 built | Fire "Change code" on ZX-001, enter ZX-100, then fire "Change the code" | The item is present as ZX-100 with its 1 reading, and Studio Markers holds no item ZX-001 | R4.4 |
| T8 | Studio Markers' Family column present; R4.8 built | Fire "Rename column" on Family and enter Hue Family | The file stores the column as Hue Family in the position Family had, with ZX-001's value Blue | R4.8 |
| T9 | Studio Markers items ZX-001 and ZX-002 present; R6.1 and R6.2 built | Select both, fire "Set a field", choose Family and enter Brights | Both are present with Family Brights in the file, and ZX-003's Family reads Pink | R6.2 |
| T10 | ZX-001 and ZX-002 holding Family Brights after the bulk change T9's When makes; R4.7 built | Fire "Undo" | The file holds Family Blue for ZX-001 and Family Green for ZX-002 | R4.7 |
| T11 | Studio Markers item ZX-013 present; R5.5 built | Fire "Show history" in ZX-013's detail, then "Use this reading" on T1 | ZX-013 is present with 4 readings: a new current one whose reason is restore and whose measurement time is 2026-01-10, then T3, T2 and T1 | R5.5 |
| T12 | Studio Markers item ZX-013 present, T2 unsettled | In ZX-013's detail fire the Data Foundation PRD's E11 swatch-has-changed action | ZX-013 is present, T3's reason reads re-measurement, T2 carries no unsettled mark, E11 is not up, and E2 no longer offers "Answer re-scans" | R4.6, R5.7 |
| T13 | Studio Markers item ZX-010 present and pending; R2.9 built; table in queue order | Drag ZX-010 above ZX-001 | ZX-010 is present, and the queue order the capture PRD's R11.11 reads begins ZX-010, ZX-001, ZX-002 | R2.9 |
| T14 | Studio Markers item ZX-001 present and captured; R4.9 built | In ZX-001's detail fire the capture PRD's Flag action | ZX-001 is present with its 1 reading, which is no longer current, and the capture PRD's R11.11 reads its row state set aside | R4.9 |

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
runs each case from the harness state, in the first build phase except UJ1.3-d and UJ1.3-e, which
run in the phase that lands R1.7 once OQ 10 closes; it carries that PRD's UJ1.2-c. Scenario 4 runs
in the first build phase, each case from the harness state.

| Case | Given | When | Assert | Rows |
|---|---|---|---|---|
| UJ1.1-a | The seeded file | Fire "Rename collection" on Gouache Set and enter Gouache Travel Set | E2 lists Gouache Travel Set then Studio Markers, and the file read with the app closed names the collection Gouache Travel Set | R1.3 |
| UJ1.1-b | The state UJ1.1-a left | Fire "Rename collection" on Gouache Travel Set and enter a space, studio in lower case, three spaces, MARKERS in upper case, and a space | The capture PRD's E1 renders, and the file names the collection Gouache Travel Set | R1.3 |
| UJ1.1-c | The state UJ1.1-b left | Fire "Rename collection" on Gouache Travel Set and enter three spaces | E7 renders naming Gouache Travel Set, and the file names the collection Gouache Travel Set | R1.3 |
| UJ1.1-d | The state UJ1.1-c left | Fire "Rename collection" on Gouache Travel Set and enter gouache travel set, all in lower case | E2 lists gouache travel set, and the file stores the name gouache travel set as entered | R1.3 |
| UJ1.2-a | Studio Markers holds a bulk session in flight, paused by the operator | Fire "Delete collection" on Studio Markers | E6 renders with no variant naming Studio Markers, and the file holds Studio Markers with 13 items and 16 readings | R1.4, R8.3 |
| UJ1.2-b | Studio Markers holds an interrupted session and no session in flight | Fire "Delete collection" on Studio Markers | E6 renders its "interrupted" variant naming Studio Markers, and the file holds Studio Markers with 13 items and 16 readings | R1.4 |
| UJ1.2-c | As UJ1.2-a, with E6 up | Fire "Go to the session" | The capture surface of Studio Markers' paused session is the surface shown, and the file holds Studio Markers with 13 items | R1.5 |
| UJ1.2-d | As UJ1.2-b, with E6 up | Fire "Go to the session" | The capture PRD's E25 renders for Studio Markers | R1.5 |
| UJ1.2-e | As UJ1.2-a, with E6 up | Fire "Cancel" | E6 is not up, and the file holds Studio Markers with 13 items and 16 readings | R1.5 |
| UJ1.3-a | The seeded file | Fire "Delete collection" on Studio Markers, then press Return | The Data Foundation PRD's E14 renders naming Studio Markers, 13 swatches and 16 readings; after Return it is not up and E2 lists Studio Markers with 13 swatches | R1.4, R1.6 |
| UJ1.3-b | The seeded file | Fire "Delete collection" on Studio Markers, then E14's export-first action, then close the export | The Data Export PRD's E1 renders for Studio Markers at collection scope; after it closes E14 is up again and the file holds Studio Markers with 13 items | R1.4 |
| UJ1.3-c | The seeded file | Fire "Delete collection" on Studio Markers, then E14's delete action | E2 lists only Gouache Set, and the file read with the app closed holds no item and no reading of Studio Markers | R1.4 |
| UJ1.3-d | The seeded file; R1.7 built | Fire "Delete collection" on Studio Markers, then E14's delete action, then "Undo" on E10 | E10 renders its "collection" variant naming Studio Markers and 13 swatches; after Undo, E2 lists Studio Markers with 13 swatches and ZX-013's history lists 3 readings | R1.7 |
| UJ1.3-e | The seeded file; R1.7 built | Fire "Delete collection" on Studio Markers, then E14's delete action, then close the file and reopen it | E2 lists only Gouache Set, and E10 is not up | R1.7 |
| UJ1.4-a | A file holding no collection | Open the file | E1 renders and offers "New collection" | R1.1 |
| UJ1.4-b | The seeded file | Show the collection list | E2 renders stating 2 collections and lists Gouache Set with 2 swatches, then Studio Markers with 13 swatches | R1.1 |
| UJ1.4-c | The seeded file | Fire "New collection", complete the capture PRD's creation naming the collection Inks, then choose Inks | E2 lists Gouache Set, Inks and Studio Markers in that order, and the collection surface shows Inks with the capture PRD's E2 empty variant where the table would be | R1.2, R2.2 |

### UJ 2. Browse a collection honestly

Scenario 1 runs in the first build phase, each case from the harness state; UJ2.1-h carries the
collection half of the device PRD's UJ1.2-c. Scenario 2 runs in the phase that lands R2.10, each case
from the harness state.

| Case | Given | When | Assert | Rows |
|---|---|---|---|---|
| UJ2.1-a | The seeded file | Choose Studio Markers | E3 renders with no variant stating 13 swatches; the table lists ZX-001, ZX-002, ZX-003, ZX-004, ZX-005, ZX-006, ZX-007, ZX-010, ZX-011, ZX-012, ZX-013, ZX-014, ZX-015 in that order, under the columns chip, Swatch Code, Swatch Name, row state, L*, C*, h°, Spread, Swatch Alternate Code, Swatch Alternate Name and Family | R2.1 |
| UJ2.1-b | The seeded file; display gamut sRGB | Choose Studio Markers and list each chip's marks | ZX-002 and ZX-003 each carry cannot-show and outside-sRGB; ZX-004 non-spectral; ZX-005 simulated and samples-disagreed; ZX-006 value-absent, with an empty chip; ZX-010 and ZX-011 no-value, each with an empty chip; ZX-012 unreadable, with an empty chip; ZX-013 re-scan-unanswered; ZX-001, ZX-007, ZX-014 and ZX-015 carry no mark | R2.3, R2.4 |
| UJ2.1-c | The seeded file; display gamut Display P3 | Choose Studio Markers and list each chip's marks | ZX-002 carries outside-sRGB and not cannot-show; ZX-003 carries both | R2.4, R2.5 |
| UJ2.1-d | The seeded file; the window on a display reporting Display P3, beside a second display reporting sRGB | Move the window to the sRGB display | Within BROWSE_RESPONSE_BUDGET of the move ZX-002 carries cannot-show | R2.5 |
| UJ2.1-e | The seeded file; the only display reports no gamut | Choose Studio Markers and list each chip's marks | ZX-002 and ZX-003 each carry cannot-show, and ZX-001 does not | R2.5 |
| UJ2.1-f | The seeded file | Choose Studio Markers, then Gouache Set | The device PRD's E22 is up on Studio Markers and not up on Gouache Set | R2.6 |
| UJ2.1-g | The seeded file | Choose Studio Markers and fire E22's action | The table lists only ZX-005, the simulated filter shows as active, and E3 renders its "narrowed" variant stating 1 of 13 | R2.6, R3.4 |
| UJ2.1-h | The seeded file, with ZX-005's current reading's snapshot of the live kind | Choose Studio Markers, open ZX-015 and fire "Show history" | E22 is not up; ZX-015's chip carries no simulated mark; E17 lists B2 without the simulated mark and B1 with it | R2.6, R5.2 |
| UJ2.1-i | The seeded file | Choose Studio Markers and read the Spread column | ZX-001 shows 0.4, ZX-005 shows 3.1, and ZX-007 shows none | R2.7 |
| UJ2.1-j | The seeded file; Studio Markers shows the capture PRD's E24 session summary | Choose Studio Markers and list the collection surface | The table lists the 13 items; E24 is up; the entry points the capture PRD's R11.15g lists are listed beside it; the counts read 10 scanned, 2 set aside and 1 to go | R2.2 |
| UJ2.1-k | The seeded file | Choose Studio Markers and fire "Colour marks" | E12 renders naming eleven marks: the nine R2.4 lists and never-true and unsettled | R2.8 |
| UJ2.2-a | The seeded file; R2.10 built | Choose Studio Markers and list the columns the user can hide | Swatch Name, row state, L*, C*, h°, Spread, Swatch Alternate Code, Swatch Alternate Name and Family, and neither the chip nor Swatch Code | R2.10 |
| UJ2.2-b | The seeded file; R2.10 built | In Studio Markers hide Family and Spread; quit the app, relaunch it and choose Studio Markers; then quit it and read the file | After relaunch the columns are the chip, Swatch Code, Swatch Name, row state, L*, C*, h°, Swatch Alternate Code and Swatch Alternate Name; the file holds no record of the hidden columns and still holds the Family column with ZX-001's value Blue | R2.10 |

### UJ 3. Find a swatch

Scenarios 1–3 run in the first build phase, each case from the harness state. Scenario 4 runs in the
phase that lands R3.7 and R3.8, each case from its own Given; its distances are the published
CIEDE2000 test values OQ 11 names.

| Case | Given | When | Assert | Rows |
|---|---|---|---|---|
| UJ3.1-a | The seeded file; Studio Markers chosen | Type zx-01 in the search field | The table lists ZX-010, ZX-011, ZX-012, ZX-013, ZX-014, ZX-015, and E3 renders its "narrowed" variant stating 6 of 13 | R3.1, R3.4 |
| UJ3.1-b | The seeded file; Studio Markers chosen | Type purple | The table lists only ZX-014, whose Family is Purple | R3.1 |
| UJ3.1-c | The seeded file; Studio Markers chosen | Type b-1; then empty the field and type zure | The table lists only ZX-001 both times | R3.1 |
| UJ3.1-d | The seeded file; Studio Markers chosen | Type 12 | E4 renders naming 12, and the table lists no item | R3.1, R3.5 |
| UJ3.1-e | The seeded file; Studio Markers chosen | Type a space, SKY in upper case, three spaces, blue, and a space | The table lists only ZX-001 | R3.1 |
| UJ3.1-f | The seeded file; Studio Markers chosen | Type qqq, then fire "Clear search" | E4 renders offering only "Clear search"; after it, E3 renders with no variant and the table lists 13 items | R3.5 |
| UJ3.2-a | The seeded file; Studio Markers chosen | Choose the row-state filter value set aside | The table lists ZX-011 and ZX-012 | R3.4 |
| UJ3.2-b | The seeded file; Studio Markers chosen | Choose the mark filter values outside-sRGB and simulated | The table lists ZX-002, ZX-003, ZX-005 | R3.4 |
| UJ3.2-c | The seeded file; Studio Markers chosen | Choose the row-state value pending and the mark value outside-sRGB, then fire "Clear filters" | E5 renders stating 13 swatches hidden; after it, the table lists 13 items | R3.4, R3.5 |
| UJ3.2-d | The seeded file; Studio Markers chosen | Type qqq, choose the mark value simulated, then fire "Clear filters" | E5 renders; after "Clear filters" the search field holds qqq and E4 renders | R3.4, R3.5 |
| UJ3.3-a | A collection named Codes holding items A10, A2, A and B1 in that queue order | Fire the Swatch Code header, then fire it again | First the table lists A, A2, A10, B1; then B1, A10, A2, A | R3.2 |
| UJ3.3-b | The seeded file; Studio Markers chosen | Fire the Family header | The table lists ZX-001, ZX-006, ZX-002, ZX-013, ZX-004, ZX-010, ZX-003, ZX-014, ZX-005, ZX-015, ZX-007, ZX-011, ZX-012 | R3.2 |
| UJ3.3-c | The seeded file; Studio Markers chosen | Fire the L* header, then fire it again | First ZX-014, ZX-015, ZX-005, ZX-003, ZX-013, ZX-001, ZX-002, ZX-004, ZX-007, ZX-006, ZX-010, ZX-011, ZX-012; then ZX-007, ZX-004, ZX-002, ZX-001, ZX-013, ZX-003, ZX-005, ZX-015, ZX-014, ZX-006, ZX-010, ZX-011, ZX-012 | R3.2, R3.3 |
| UJ3.3-d | The seeded file; Studio Markers chosen | Fire the h° header | ZX-005, ZX-015, ZX-002, ZX-013, ZX-001, ZX-014, ZX-003, ZX-004, ZX-007, ZX-006, ZX-010, ZX-011, ZX-012 | R3.3 |
| UJ3.3-e | The seeded file; Studio Markers chosen | Fire the Swatch Name header, then read the queue order through the capture PRD's R11.11 | The table lists ZX-015, ZX-007, ZX-004, ZX-006, ZX-005, ZX-011, ZX-003, ZX-012, ZX-014, ZX-002, ZX-001, ZX-013, ZX-010; the queue order reads ZX-001, ZX-002, ZX-003, ZX-004, ZX-005, ZX-006, ZX-007, ZX-010, ZX-011, ZX-012, ZX-013, ZX-014, ZX-015 | R3.2 |
| UJ3.3-f | The seeded file | In Studio Markers type zx-01 and fire the L* header; choose Gouache Set; choose Studio Markers; then close the file, reopen it and choose Studio Markers | On returning, the search field holds zx-01 and the table lists ZX-014, ZX-015, ZX-013, ZX-010, ZX-011, ZX-012; after reopening, the search field is empty and the table lists 13 items in queue order | R3.6 |
| UJ3.4-a | A collection Blues, chosen condition M1, D50/2°, holding FS-000 at L* 50, a* 0, b* −82.7485; FS-001 at 50, −1.3802, −84.2814; FS-002 at 50, 2.6772, −79.7751; FS-003 at 50, 3.1571, −77.2803; FS-004 at 50, 2.8361, −74.0200; FS-005, non-spectral, worked out at a reference other than the collection's; FS-006, pending; published ΔE2000 from FS-000 of 1.0000, 2.0425, 2.8615 and 3.4412 for FS-001 to FS-004 | Open FS-000 and fire "Find similar" | E9 renders with no variant, listing FS-001 at 1.0000, FS-002 at 2.0425 and FS-003 at 2.8615 in that order, and stating 2 swatches not compared | R3.7 |
| UJ3.4-b | A collection Pair holding FS-000 and FS-004 at the values UJ3.4-a declares, published ΔE2000 3.4412 between them | Open FS-000 and fire "Find similar" | E9 renders its "none" variant | R3.8 |
| UJ3.4-c | Blues as UJ3.4-a declares it | Open FS-006 | E14 does not offer "Find similar" | R3.8 |
| UJ3.4-d | Blues as UJ3.4-a declares it — FS-000, FS-001, FS-002, FS-003, FS-004, FS-005 and FS-006 — and a collection Blues Two holding FS-101 at L* 50, a* −1.1848, b* −84.8006, published ΔE2000 1.0000 from FS-000; R1.9 and R1.10 built | From the All items view open Blues' FS-000 and fire "Find similar"; then from Blues open FS-000 and fire it again | From All items, E9 lists FS-001 and FS-101 at 1.0000, each naming its collection, before FS-002 at 2.0425 and FS-003 at 2.8615; from Blues, E9 lists FS-001, FS-002 and FS-003 and no FS-101 | R3.7, R1.10 |

### UJ 4. Look at and edit a swatch

Scenarios 1, 2, 4 and 5 run in the first build phase; scenario 2 runs under continuity, each of its
cases against the state the previous one left, and the others each case from the harness state
or its own Given. Scenario 3 runs in the phase that lands R4.4, scenario 6 in the phase that lands
R4.8 and scenario 7 in the phase that lands R4.9, each case from the harness state or its own Given,
except that UJ4.6-e and UJ4.6-f each run against the state UJ4.6-a's When leaves and UJ4.7-d runs
against the state UJ4.7-a left.

| Case | Given | When | Assert | Rows |
|---|---|---|---|---|
| UJ4.1-a | The seeded file | Open Studio Markers' ZX-001 | E14 renders; its lines read Swatch Code ZX-001, Swatch Name Sky Blue, alternate code B-12, alternate name Azure, collection Studio Markers, Family Blue, row state captured, six spaces each stamped D50, 2°, M1 and a derivation version, measured 2026-02-03, device live Spectro 2 SN-100200 firmware 2.1.0, 3 samples, basis spectral curves, spread 0.4, verdict agreed, no mark, 1 reading | R4.1, R4.2 |
| UJ4.1-b | The seeded file; Studio Markers searched zx-01, view-sorted by Swatch Name, ZX-013 selected, its row the first on screen | Open ZX-013, then close it | The table lists ZX-015, ZX-011, ZX-012, ZX-014, ZX-013, ZX-010, ZX-013 is selected, and ZX-013's row is the first on screen | R4.1 |
| UJ4.1-c | The seeded file | Open ZX-012 | E14 shows the Data Foundation PRD's E4 in its current variant, states that ZX-012 has no current value, shows the unreadable mark, and its State line reads set aside, cause unreadable, not settled | R4.2 |
| UJ4.1-d | The seeded file plus Studio Markers item ZX-016, Moss: captured, its current reading readable, one earlier reading quarantined | Open ZX-016 | E14 shows the Data Foundation PRD's E4 in its history variant, and ZX-016's current value | R4.2 |
| UJ4.1-e | The seeded file plus Studio Markers item ZX-017, Sand: captured, one sample's archived payload unavailable, its values intact | Open ZX-017 | E14 shows the Data Foundation PRD's E31 and ZX-017's current value | R4.2 |
| UJ4.1-f | The seeded file | Open ZX-013 | E14 shows the Data Foundation PRD's E11 naming ZX-013 and 2026-05-20 | R4.2 |
| UJ4.2-a | The seeded file | Open ZX-001, set Swatch Name to Sky Blue Light and press Return | The file holds Swatch Name Sky Blue Light for ZX-001, 1 reading, queue position 1 and row state captured | R4.3 |
| UJ4.2-b | The state UJ4.2-a left | In ZX-001's Family field type Cyan, then press Escape | The file holds Family Blue for ZX-001 | R4.3 |
| UJ4.2-c | The state UJ4.2-b left | Empty ZX-001's Swatch Alternate Name and leave the field | The file read at SQLITE_READER_FLOOR holds an empty alternate name for ZX-001 and the text Azure nowhere | R4.3 |
| UJ4.3-a | The seeded file; R4.4 built | Open ZX-001, fire "Change code", enter ZX-100, then fire "Change the code" | E16 renders naming ZX-001 and ZX-100; afterwards the file holds Studio Markers item ZX-100 with 1 reading and no Studio Markers item ZX-001, and Gouache Set's ZX-001 reads Cerulean | R4.4 |
| UJ4.3-b | The seeded file; R4.4 built | Open ZX-001, fire "Change code", enter ZX-100, then fire "Keep this code" | The file holds Studio Markers item ZX-001 and no ZX-100 | R4.4 |
| UJ4.3-c | The seeded file; R4.4 built | Open ZX-001, fire "Change code" and enter zx-002 | E15 renders its "duplicate" variant naming ZX-001, and the file holds ZX-001 | R4.4 |
| UJ4.3-d | The seeded file; R4.4 built | Open ZX-001, fire "Change code" and enter two spaces | E15 renders with no variant naming ZX-001, and the file holds ZX-001 | R4.4 |
| UJ4.3-e | The seeded file; R4.4 built | Open Studio Markers' ZX-007, fire "Change code", enter GS-002, then fire "Change the code" | E16 renders; afterwards Studio Markers holds GS-002 with Swatch Name Chalk White, and Gouache Set's GS-002 reads Cadmium Red | R4.4 |
| UJ4.3-f | The seeded file; R4.4 built; a bulk session in flight on Studio Markers | Open ZX-001 and fire "Change code" | E6 renders with no variant, and the file holds ZX-001 | R4.4, R8.3 |
| UJ4.4-a | The seeded file | Open ZX-010, fire "Delete swatch", then press Return | The Data Foundation PRD's E8 renders naming ZX-010; after Return it is not up and the table lists 13 items | R4.5, R1.6 |
| UJ4.4-b | The seeded file | Open ZX-010, fire "Delete swatch", then E8's delete action | E3 states 12 swatches, the table lists no ZX-010, and the file holds no item ZX-010 | R4.5 |
| UJ4.4-c | The seeded file; a bulk session in flight on Studio Markers | Select ZX-010's row and fire "Delete swatch" | E6 renders with no variant, and the table lists 13 items | R4.5, R8.3 |
| UJ4.4-d | The seeded file; Studio Markers holds an interrupted session and no session in flight | Open ZX-010, fire "Delete swatch", then E8's delete action | E8 renders, not E6; afterwards the table lists 12 items and no ZX-010 | R4.5, R8.3 |
| UJ4.4-e | The seeded file; R1.7 built | Open ZX-010, fire "Delete swatch", E8's delete action, then "Undo" on E10 | E10 renders with no variant naming ZX-010; after Undo the table lists ZX-010, pending, at queue position 8 | R1.7 |
| UJ4.5-a | The seeded file | Open ZX-001 and fire "Export swatch" | The Data Export PRD's E1 renders at single-item scope naming ZX-001 | R1.8 |
| UJ4.5-b | The seeded file; the capture PRD's re-scan rows built | Open ZX-001 and fire the capture PRD's re-scan entry | The capture PRD's E28 renders naming ZX-001 | R4.6 |
| UJ4.5-c | The seeded file | Open ZX-012 and fire the Data Foundation PRD's E4 use-a-previous-reading action | ZX-012's chip shows a colour and no unreadable mark, its history lists 3 readings, the newest current with reason restore and measurement time 2026-02-01, and the capture PRD's R11.11 reads its row state captured | R4.6 |
| UJ4.6-a | The seeded file; R4.8 built | Fire "Rename column" on Family and enter Hue Family | The table's header reads Hue Family, and the file stores the column as Hue Family in the position Family had, with ZX-001's value Blue | R4.8 |
| UJ4.6-b | The seeded file; R4.8 built | Fire "Rename column" on Family and enter two spaces | E11 renders with no variant naming Family, and the file stores the column as Family | R4.8 |
| UJ4.6-c | The seeded file; R4.8 built | Fire "Rename column" on Family and enter swatch  NAME, with two spaces between the words | E11 renders its "duplicate" variant, and the file stores the column as Family | R4.8 |
| UJ4.6-d | The seeded file plus a second Studio Markers imported column named Maker; R4.8 built | Fire "Rename column" on Family and enter MAKER | E11 renders its "duplicate" variant, and the file stores the column as Family | R4.8 |
| UJ4.6-e | The state UJ4.6-a's When leaves; R4.8 built | Import into Studio Markers a CSV whose code column holds ZX-001 and whose column named Hue Family holds Teal Blue, keeping the new details | The file holds Hue Family Teal Blue for ZX-001, and Studio Markers holds one column named Hue Family and none named Family | R4.8 |
| UJ4.6-f | The state UJ4.6-a's When leaves; R4.8 built | Import into Studio Markers a CSV whose code column holds ZX-001 and whose column named Family holds Sea Blue | Studio Markers holds the column Hue Family, ZX-001's value there still Blue, and after its existing columns a new column named Family, ZX-001's value there Sea Blue | R4.8 |
| UJ4.7-a | The seeded file; R4.9 built | Open ZX-001 and fire the capture PRD's Flag action | ZX-001's chip is empty and carries the no-value mark; E14's State line reads set aside, cause flagged after capture, not settled; its history lists 1 reading, not current; the capture PRD's R11.11 reads its row state set aside | R4.9, R4.2 |
| UJ4.7-b | The seeded file; R4.9 built | Open ZX-010, ZX-011 and ZX-012 in turn and list each detail's actions | None of the three offers the capture PRD's Flag action | R4.9 |
| UJ4.7-c | The seeded file; R4.9 built; a bulk session in flight on Studio Markers | Open ZX-001 and fire the capture PRD's Flag action | E6 renders with no variant, and the capture PRD's R11.11 reads ZX-001 captured with its 1 reading current | R4.9, R8.3 |
| UJ4.7-d | The state UJ4.7-a left; R4.9 built | Open the capture PRD's set-aside list from the collection surface | It lists ZX-001 with the cause flagged after capture, beside ZX-011 and ZX-012 | R4.9 |

### UJ 5. Read a swatch's history and settle its re-scans

Scenarios 1 and 2 run in the first build phase, scenario 3 in the phase that lands R5.4 and R5.5,
and scenario 4 in the phase that lands R5.8, each case from the harness state or its own Given.

| Case | Given | When | Assert | Rows |
|---|---|---|---|---|
| UJ5.1-a | The seeded file; Studio Markers chosen | Open ZX-013 and fire "Show history" | E17 renders with no variant stating 3 readings, listing T3 — current, measured 2026-05-20 — then T2 with reason re-measurement, measured 2026-03-02, and the unsettled mark, then T1 with reason initial, measured 2026-01-10; E14 is still up and the table lists its 13 rows | R5.1, R5.2 |
| UJ5.1-b | The seeded file | Open ZX-014, fire "Show history", then "Measured order", then "Recorded order" | The "measured" variant lists only P2 and states 1 reading left out; the body then lists P2, then P1 carrying the never-true mark | R5.3 |
| UJ5.1-c | The seeded file | Open ZX-013, fire "Show history", then "Measured order" | The "measured" variant lists T1, T2, T3 in that order, T2 carrying the unsettled mark, with no left-out sentence | R5.3 |
| UJ5.1-d | The seeded file | For each reading of ZX-012, ZX-013 and ZX-014 in E17, list the actions offered | No action offered removes a reading | R5.6 |
| UJ5.2-a | The seeded file | Show the collection list and fire "Answer re-scans" | E2 offers "Answer re-scans", and the Data Foundation PRD's E26 renders stating 1 re-scan | R5.7 |
| UJ5.2-b | The seeded file, with T3's reason declared re-measurement | Show the collection list | E2 does not offer "Answer re-scans" | R5.7 |
| UJ5.2-c | The seeded file | Open ZX-013 and fire the Data Foundation PRD's E11 old-reading-was-wrong action, then "Show history" and "Measured order" | T2 carries the never-true mark, and the "measured" variant lists T1 and T3 and states 1 reading left out | R4.6, R5.3 |
| UJ5.3-a | A collection Hist, chosen condition M1, D50/2°, holding HX-001 with spectral readings H1 (reason initial, L* 50, a* 2.6772, b* −79.7751), H2 (reason re-measurement, 50, −1.3802, −84.2814) and H3 (reason re-measurement, current, 50, 0, −82.7485); published ΔE2000 from H3 of 2.0425 for H1 and 1.0000 for H2 | Open HX-001 and fire "Show history" | H2 shows ΔE2000 1.0000 and H1 shows 2.0425 | R5.4 |
| UJ5.3-b | Hist as UJ5.3-a declares it, except that H1 holds no measurement in the chosen condition M1 | Open HX-001 and fire "Show history" | H1's distance shows as absent with its mark, and H2 shows 1.0000 | R5.4 |
| UJ5.3-c | The seeded file | Open ZX-013, fire "Show history", then "Use this reading" on T1 | E17 lists 4 readings: a new current one with reason restore and measurement time 2026-01-10, then T3, T2, T1; the file holds all 4 | R5.5 |
| UJ5.3-d | The seeded file plus ZX-016 as UJ4.1-d declares it | Open ZX-016 and fire "Show history" | Its quarantined earlier reading offers no "Use this reading" | R5.5 |
| UJ5.3-e | The seeded file plus Studio Markers item ZX-018, Rust: set aside by a flag, its one reading in history, no current value | Open ZX-018 and fire "Show history" | No reading offers "Use this reading" | R5.5 |
| UJ5.3-f | The seeded file | Open ZX-012, fire "Show history" and list the actions each reading offers, then fire "Use this reading" on its readable earlier reading | Its quarantined reading offers no "Use this reading" and its earlier reading does; afterwards E17 lists 3 readings, the newest current with reason restore and measurement time 2026-02-01, and ZX-012 carries no unreadable mark | R5.5 |
| UJ5.4-a | Hist as UJ5.3-a declares it | Open HX-001, fire "Show history", select H1 and H3, fire "Compare" | Two chips show side by side with their marks, and the distance reads ΔE2000 2.0425 | R5.8 |

### UJ 6. Work on many swatches at once

Scenarios 1 and 2 run in the phase that lands R6.1, R6.2 and R4.7, and scenario 3 in the phase that
lands R6.3; every case runs from the harness state or its own Given.

| Case | Given | When | Assert | Rows |
|---|---|---|---|---|
| UJ6.1-a | The seeded file; Studio Markers chosen | Select ZX-001, then extend the selection by range to ZX-003 | 3 items are selected: ZX-001, ZX-002, ZX-003 | R6.1 |
| UJ6.1-b | The seeded file; Studio Markers chosen; the window shows 5 rows | Type zx-01 and fire "Select all" | 6 items are selected, ZX-015 among them though its row is off screen | R6.1 |
| UJ6.1-c | The seeded file; Studio Markers chosen | Select ZX-001, add ZX-011, then choose the row-state filter value captured | 1 item is selected, ZX-001 | R6.1 |
| UJ6.2-a | The seeded file; Studio Markers chosen | Select ZX-001 and ZX-002, fire "Set a field", choose Family and enter Brights | E8 does not render; the file holds Family Brights for ZX-001 and ZX-002 and Pink for ZX-003 | R6.2 |
| UJ6.2-b | The seeded file; Studio Markers chosen | Select ZX-001 through ZX-013 by range, fire "Set a field", choose Family and leave the value empty, then fire "Cancel" | E8 renders its "clear" variant naming Family and 11 swatches; afterwards the file holds Family Blue for ZX-001 and Green for ZX-013 | R6.2 |
| UJ6.2-c | The seeded file; Studio Markers chosen | Select ZX-001 through ZX-013 by range, fire "Set a field", choose Family and leave the value empty, then fire "Apply to ⟨n⟩ swatches" with ⟨n⟩ rendering 11 | The file holds an empty Family for those 11 items, Purple for ZX-014 and Red for ZX-015, and 16 Studio Markers readings | R6.2 |
| UJ6.2-d | The seeded file; Studio Markers chosen | Select ZX-001 through ZX-013 by range, fire "Set a field", choose Family and enter Set A | E8 renders with no variant naming Family, Set A and 11 swatches | R6.2 |
| UJ6.2-e | The seeded file; Studio Markers chosen | Select ZX-001 and ZX-002 and fire "Set a field" | The fields offered are Swatch Name, Swatch Alternate Code, Swatch Alternate Name and Family, and Swatch Code is not among them | R6.2 |
| UJ6.2-f | The seeded file after the 11-item clear UJ6.2-c's When makes | Fire "Undo" | The file holds Family Blue for ZX-001, Green for ZX-002 and Yellow for ZX-012 | R4.7 |
| UJ6.2-g | The seeded file; Studio Markers chosen; the volume holding the file declared full | Select ZX-001 and ZX-002, fire "Set a field", choose Family and enter Brights | The Data Foundation PRD's E15 renders, and the file holds Family Blue for ZX-001 and Green for ZX-002 | R8.8 |
| UJ6.3-a | The seeded file; Studio Markers chosen; R6.3 built | Select ZX-013 and ZX-014, fire "Delete selected", then press Return | The Data Foundation PRD's E33 renders naming 2 swatches; after Return it is not up, the table lists 13 items, and the file holds ZX-013's 3 readings and ZX-014's 2 | R6.3, R1.6 |
| UJ6.3-b | The seeded file; Studio Markers chosen; R6.3 built | Select ZX-013 and ZX-014, fire "Delete selected", then the Data Foundation PRD's E33 delete action | That E33 names 2 swatches, 2 current readings and 3 earlier readings; afterwards E3 states 11 swatches, and the file read with the app closed holds neither item and 11 Studio Markers readings | R6.3 |
| UJ6.3-c | The seeded file; R6.3 built; a bulk session in flight on Studio Markers | Select ZX-010 and ZX-011 and fire "Delete selected" | E6 renders with no variant, and the table lists 13 items | R6.3, R8.3 |
| UJ6.3-d | The seeded file; R6.3 and R1.7 built | Select ZX-010 and ZX-011, fire "Delete selected" and confirm, then fire "Undo" on E10 | E10 renders its "swatches" variant stating 2; after Undo the table lists 13 items, ZX-010 and ZX-011 among them | R1.7, R6.3 |
| UJ6.3-e | The seeded file; Studio Markers chosen; R6.3 built | Select ZX-013 and ZX-014, fire "Delete selected", then the Data Foundation PRD's E33 export-first action, then close the export | The Data Export PRD's E1 renders; after it closes that E33 is up again naming 2 swatches, and the file holds ZX-013 and ZX-014 with their 5 readings | R6.3 |

### UJ 7. Browse every collection at once

Scenario 1 runs in the phase that lands R1.9 and R1.10, each case from the harness state.

| Case | Given | When | Assert | Rows |
|---|---|---|---|---|
| UJ7.1-a | The seeded file | Fire "All items" | E13 renders with no variant stating 15 swatches across 2 collections; the table lists Gouache Set's ZX-001 and GS-002, then Studio Markers' 13 items in queue order, under the columns chip, Swatch Code, Swatch Name, row state, L*, C*, h°, Spread, Swatch Alternate Code, Swatch Alternate Name and Collection; ZX-001 appears twice, once as Cerulean in Gouache Set and once as Sky Blue in Studio Markers | R1.9 |
| UJ7.1-b | The seeded file | Fire "All items" and type zx-001 | The table lists two rows, Gouache Set's ZX-001 and Studio Markers' ZX-001, and E13 renders its "narrowed" variant stating 2 of 15 | R1.9, R3.4 |
| UJ7.1-c | The seeded file | Fire "All items" and list the actions offered | "Filters", "Colour marks" and "Answer re-scans" are offered; "Select all", "Set a field", "Delete selected", "Use as scan order", "Rename collection", "Delete collection" and "Export collection" are not, and no row can be dragged | R1.10 |
| UJ7.1-d | The seeded file | Fire "All items", open Gouache Set's ZX-001, set Swatch Name to Cerulean Deep and press Return | The file holds Cerulean Deep for Gouache Set's ZX-001 and Sky Blue for Studio Markers' ZX-001 | R1.10, R4.3 |
| UJ7.1-e | The seeded file | Fire "All items", then the L* header | The table lists ZX-014, ZX-015, ZX-005, GS-002, ZX-003, Gouache Set's ZX-001, ZX-013, Studio Markers' ZX-001, ZX-002, ZX-004, ZX-007, ZX-006, ZX-010, ZX-011, ZX-012 | R1.9, R3.2 |
| UJ7.1-f | The seeded file | Fire "All items" | The device PRD's E22 is not up, and ZX-005's chip carries the simulated mark | R1.10 |

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
| UJ8.1-f | The seeded file; a bulk session in flight on Studio Markers, the table in queue order | List the actions offered and try to drag ZX-010 | "Use as scan order" is not offered and no row can be dragged | R2.9, R8.3 |

### UJ 9. Browse while the app does other things

Scenario 1 through scenario 8 run in the first build phase, each case from the harness state or its
own Given, except UJ9.5-b and UJ9.7-b, which run in the phase that lands the P1 rows they name.

| Case | Given | When | Assert | Rows |
|---|---|---|---|---|
| UJ9.1-a | The seeded file; a bulk session in flight on Studio Markers with ZX-010 current, the Demo Device connected; the table searched zx-01, view-sorted by Swatch Name, ZX-013 selected | Capture saves a 3-sample Demo Device set on ZX-010 | Within BROWSE_RESPONSE_BUDGET ZX-010 lists as captured with a chip carrying the simulated mark; the search field holds zx-01, the sort is by Swatch Name and ZX-013 is selected | R8.1, R8.3 |
| UJ9.1-b | The seeded file; a bulk session in flight on Studio Markers with ZX-010 current and remembered | Open ZX-001, set Swatch Name to Sky and press Return | The file holds Sky for ZX-001; the capture PRD's R11.11 reads the current row ZX-010, the remembered row ZX-010, and the seeded queue order | R8.3 |
| UJ9.2-a | The seeded file, opened read-only in the Data Foundation PRD's E1 state | List the collection list, Studio Markers' table and marks, ZX-001's detail, ZX-013's history, and every action offered | 13 items list with the marks UJ2.1-b lists; the detail and history show their lines; no action offered writes the file — not "New collection", "Rename collection", "Delete collection", "Delete swatch", "Answer re-scans", field editing, or any P1 or P2 write action | R8.4 |
| UJ9.2-b | The seeded file | In Studio Markers type zx-01, fire the L* header, then open ZX-013 and set Swatch Name to Deep Teal | The table lists ZX-014, ZX-015, ZX-013, ZX-010, ZX-011, ZX-012, and the file holds Deep Teal for ZX-013 | R8.4 |
| UJ9.3-a | The seeded file; Studio Markers searched zx-01 with ZX-011 selected; then, outside the app, ZX-011 removed from the file and ZX-013 renamed Deep Teal | Fire the Data Foundation PRD's E9 read-again action | The table lists ZX-010, ZX-012, ZX-013, ZX-014, ZX-015 with ZX-013 named Deep Teal; the search field holds zx-01; no item is selected | R8.5 |
| UJ9.4-a | The seeded file; network unreachable through the device PRD's R6.12 | Choose Studio Markers, type zx-01, open ZX-001, set Swatch Name to Sky and press Return, then fire "Show history" on ZX-013 | The table lists 6 items, the file holds Sky for ZX-001, E17 lists 3 readings, and the device PRD's R6.17 record holds no outbound attempt | R8.6 |
| UJ9.4-b | The seeded file | In Studio Markers type zx-01, choose the mark value simulated and fire the L* header; close the app and read the file | The file holds the text zx-01 nowhere and no record of the filter or the sort | R8.6 |
| UJ9.5-a | A file whose collection Scale holds ROWS_CEILING items generated as the Data Foundation PRD's R7.7 corpus; a Release build on a declared machine | Deliver 200 search keystrokes, 200 filter changes and 200 header fires; choose Scale 20 times from the collection list | The 95th percentile of each input kind is at or below BROWSE_RESPONSE_BUDGET, and every choice lists Scale's first rows within OPEN_COLLECTION_BUDGET | R8.1, M1 |
| UJ9.5-b | A file whose collections hold FILE_ITEMS_CEILING items in all; a Release build on a declared machine; R1.9, R3.7 and R8.2 built | Deliver 200 search keystrokes in the All items view and fire "Find similar" 50 times | The 95th percentile of each is at or below BROWSE_RESPONSE_BUDGET | R8.2 |
| UJ9.6-a | The seeded file | Read each R2.4 mark's shape and accessible name, and the accessible description of ZX-003's row | The nine shapes are pairwise distinct; ZX-003's description names ZX-003, Neon Magenta, captured, can't show on this screen and outside sRGB | R8.9 |
| UJ9.6-b | The seeded file | From the keyboard alone, deliver every action E2, E3, E14 and E17 offer in the first build phase | Each action fires | R8.9 |
| UJ9.7-a | The seeded file | Open ZX-001, set Swatch Name to Sky, press Return, then lose everything not yet safely written through the Data Foundation PRD's R7.4 and reopen | The file holds Sky for ZX-001 | R8.8 |
| UJ9.7-b | The seeded file; R6.1 and R6.2 built | Select ZX-001 through ZX-013, fire "Set a field", choose Family and leave the value empty, fire "Apply to ⟨n⟩ swatches", and crash the app during the write; reopen | Either all 11 items hold an empty Family, or all 11 hold their seeded values; no item of the 11 differs from the rest | R8.8 |
| UJ9.8-a | The seeded file under each of the display gamuts sRGB, Display P3 and none reported | List every chip's marks | Every listed mark agrees with the conditions the harness declares, for 13 items under each gamut: M2 reads 100% | R2.4, R8.10, M2 |
| UJ9.8-b | The seeded file, its 18 readings read at SQLITE_READER_FLOOR before any action | Run the Whens of UJ4.2-a, UJ5.3-c and UJ5.2-c, and, in the phases that land their rows, of UJ4.3-a, UJ6.2-c, UJ8.1-a and UJ4.6-a; read the file after each | Each of the 18 seeded readings reads back after every action, so M3 counts 0 | R8.10, M3 |

### UJ 10. See the collection as a swatch grid

Scenario 1 runs in the phase that lands R7.1 and R7.2, each case from the harness state or its own
Given.

| Case | Given | When | Assert | Rows |
|---|---|---|---|---|
| UJ10.1-a | The seeded file; Studio Markers searched zx-01, view-sorted by L*, ZX-013 selected | Fire "Grid", then "Table" | The grid lists ZX-014, ZX-015, ZX-013, ZX-010, ZX-011, ZX-012 as swatches no smaller than MIN_SWATCH_SIZE, each labelled with its code and carrying the marks UJ2.1-b lists, with ZX-013 selected; back in the table the same six rows list in that order with ZX-013 selected | R7.1 |
| UJ10.1-b | The seeded file; Studio Markers shown as a grid | Set the swatch size as small as it goes | Every swatch measures MIN_SWATCH_SIZE on a side | R7.2 |
| UJ10.1-c | The seeded file; Studio Markers shown as a grid | List ZX-010's swatch | It is an empty chip carrying the no-value mark | R7.2 |

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
| collection list | Showing it, choosing a collection, "New collection", "All items", "Answer re-scans", "Rename collection" and "Delete collection" with the names a case enters | E1 or E2, the collections listed with their counts in order, which actions are offered | R1.1, R1.2, R1.3, R1.4, R5.7 |
| collection surface | The search field's text, filter values, header fires, drags, range and toggle selection, hiding and showing a column, "Select all", "Set a field", "Delete selected", "Delete swatch", "Use as scan order", "Rename column", "Export collection", "Colour marks", "Grid", "Table", and the actions of E4–E12 | The rows listed in order, their columns and Spread values, each chip's marks and accessible description, which copy state and variant is up, the selected count, which actions are offered | R1.4–R1.8, R2.1–R2.10, R3.1–R3.8, R6.1–R6.3, R7.1, R7.2, R8.9 |
| All items view | "All items", its search field, filters, header fires, opening an item, "Find similar" | E13 and its variant, the rows listed with their collections, which actions are offered, whether the device PRD's E22 is up | R1.9, R1.10, R3.7 |
| item detail | Opening and closing an item, field edits confirmed by Return or leaving the field or discarded by Escape, "Change code" with the code a case enters, "Change the code", "Keep this code", "Delete swatch", "Export swatch", "Find similar", "Show history", the capture PRD's Flag action, and the actions of the sibling states it shows | E14's lines, E15 or E16 and their variants, the sibling state shown, which actions are offered | R4.1–R4.9 |
| version history view | "Show history", "Recorded order", "Measured order", "Use this reading", selecting two readings, "Compare" | The readings listed in order with their lines, marks and distances, E17 and its variant, which actions each reading offers | R5.1–R5.8 |
| sibling states opened here | The Data Foundation PRD's E4, E8, E9, E11, E14, E15, E26 and E33 actions, the capture PRD's E25, E28, E33 and E34 actions, the device PRD's E22 action, the Data Export PRD's E1, and the capture PRD's creation, re-scan entry and set-aside list | Which sibling state is up, what it names, and what the file and the capture PRD's R11.11 read afterwards | R1.2, R1.4, R1.5, R1.8, R2.6, R2.9, R4.5, R4.6, R4.9, R5.7, R6.3, R8.5 |
| display | The declared gamut of each display, and moving the window from one display to another | Which chips carry the cannot-show mark, and when they change | R2.5, R8.7 |
| the file | Its seeded contents, a declared outside change, a read-only open, a full volume, an induced loss of unwritten data or a crash, an import into a collection, closing and reopening it, quitting and relaunching the app | Its contents read with the app closed at SQLITE_READER_FLOOR | R2.10, R8.4, R8.5, R8.8, R8.10 |
| sessions | A declared session state on a collection, and a set capture saves on the Demo Device while a case runs | The capture PRD's R11.11 readback of queue order, row states, current and remembered row | R8.3, R8.10 |
| network | Reachability declared through the device PRD's R6.12 | The device PRD's R6.17 record of outbound attempts | R8.6 |
| accessibility and keyboard | Actions delivered from the keyboard alone; reading each mark's shape and accessible name and each row's accessible description | Whether each action fired; the shapes, names and descriptions | R8.9 |
| timing | Inputs delivered against a declared build configuration and machine | The time from each input to the table listing its result | R8.1, R8.2 |
