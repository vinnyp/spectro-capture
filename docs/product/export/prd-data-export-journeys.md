# Data Export PRD — user journeys

Companion to [prd-data-export.md](prd-data-export.md): what the user does and sees, journey by journey.
The requirement rows in that file are the rules; nothing here adds one, and the shipping copy is in [prd-data-export-copy.md](prd-data-export-copy.md).

States are named in plain language; each one's shipping copy is in the copy file, and the vocabulary is defined once in the [PRD](prd-data-export.md#vocabulary) and in the [Data Foundation PRD](../data-foundation/prd-data-foundation.md#vocabulary). Persona is named per journey. Citation shorthand: [browsing v2](../../briefs/browsing-a-collection-at-scale-research-results-v2.md) is the collection-browsing research (v2 supersedes the first pass), [SDK audit](../../briefs/nix-universal-sdk-audit-findings.md) the vendor SDK audit. This journey was the [Data Foundation PRD's DJ1](../data-foundation/prd-data-foundation.md), moved here under that document's fence F30 with no step changed.

## J1. Export the collection

**Data consumer.** Serves [U6](../vision.md#use-cases) and [vision J4](../vision.md#j4-data-out-data-consumer). Exercises [R1.1](prd-data-export.md#1-what-the-export-contains)–[R1.3](prd-data-export.md#1-what-the-export-contains), [R2.1](prd-data-export.md#2-columns-names-dialect-and-the-version)–[R2.5](prd-data-export.md#2-columns-names-dialect-and-the-version), [R3.1](prd-data-export.md#3-when-an-export-cannot-finish), and [the Data Foundation PRD's R3.4 and R3.5](../data-foundation/prd-data-foundation.md#3-derived-values-and-gamut-honesty).

1. I've finished digitizing a marker set and I want the numbers in my own colour tooling.
2. From the collection I choose to export it. The app tells me what's about to come out: how many swatches, that every column I imported is there, that the wavelength readings are there, and that all six colour spaces — XYZ, Lab, LCh, Luv, sRGB, HSL — are there.
3. It also tells me each colour space value will say which illuminant, observer, and measurement condition produced it, and which version of the working-out it came from — so a year from now I can still tell what a number means. It tells me which measurement condition the rows come out on, the one this collection is set to; a set worked out for another condition stays in my file rather than coming out here. It tells me every row will also carry when that swatch was last measured.
4. I choose current readings rather than full history, and where to put the file.
5. The file is written. Nothing in my collection changed.
6. I open it. It's plain UTF-8 CSV with comma separators and `.` decimals — nothing to configure. One column per wavelength, each named `sc_nm_` and the wavelength. The rows come out in the collection's own order, so exporting the same unchanged collection twice gives me the same file twice.
7. The sRGB columns sit next to columns naming the space they were converted into, the rendering intent used, and whether that value fell outside the gamut. Colours that fall outside standard sRGB are marked in the file, not just in the app — most screens can't show them accurately.
8. There's an `sc_simulated` column too, so the readings I took against the Demo Device while testing are obvious rather than mixed in silently, and a mark on any reading taken without wavelength data along with how its average was worked out.
9. Where a value genuinely couldn't be worked out, the cell is empty. It is never a zero or a rounded-off guess I might mistake for a measurement.
10. There's one opaque column per sample slot carrying the instrument's own round-trip string, so what I export is as complete as what the app holds.
11. Every swatch in the collection gets a row, including the ones I haven't scanned yet and the ones whose reading can't be read — those come out with their details and their colour and wavelength columns empty, and a column saying which of the two it is, so my export reconciles against the spreadsheet I imported.
12. Every row says which version of the export format it is and which version of the app wrote it, and the help docs tell me what a version bump means: a column removed, renamed, moved, or changed in meaning, or the set of wavelength columns changing. A column appended at the end doesn't bump it, and I read columns by name rather than by position, so my parser keeps working.

**If a column I imported clashes with one the app writes.** Every column the app writes carries the reserved `sc_` prefix, with no exceptions — the gamut mark, the simulated mark, the wavelength columns, all of them — so my column comes out under a slightly different name rather than being dropped or quietly overwritten, and the export surface tells me before it runs.

**If there's nowhere to write it.** No room, no permission, or the folder is gone: nothing is written, no half-finished file is left, and the app says which of the three it was and offers somewhere else. I never end up with a truncated CSV I might mistake for a complete one.

**If I ask for full history.** I get every version of every swatch instead of just the current one. The export surface always says which of the two it is doing before it runs, so I am never guessing.
