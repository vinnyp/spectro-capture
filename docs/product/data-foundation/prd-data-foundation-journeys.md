# Data Foundation PRD — user journeys

Companion to [prd-data-foundation.md](prd-data-foundation.md): what the user does and sees, journey by journey.
The requirement rows in that file are the rules; nothing here adds one, and the shipping copy is in [prd-data-foundation-copy.md](prd-data-foundation-copy.md).

States are named in plain language; each one's shipping copy is in the copy file, and the vocabulary is defined once in the [PRD](prd-data-foundation.md#vocabulary). Persona is named per journey. Citation shorthand: [browsing v2](../../briefs/browsing-a-collection-at-scale-research-results-v2.md) is the collection-browsing research (v2 supersedes the first pass), [acq v2](../../briefs/acquisition-experience-research-results-v2.md) the acquisition-experience research, [SDK audit](../../briefs/nix-universal-sdk-audit-findings.md) the vendor SDK audit.

## DJ1. Export the collection

**Data consumer.** Serves [U6](../vision.md#use-cases) and [vision J4](../vision.md#j4-data-out-data-consumer). Exercises [R3.4](prd-data-foundation.md#3-derived-values-and-gamut-honesty), [R4.1](prd-data-foundation.md#4-export)–[R4.5](prd-data-foundation.md#4-export).

1. I've finished digitizing a marker set and I want the numbers in my own colour tooling.
2. From the collection I choose to export it. The app tells me what's about to come out: how many swatches, that every column I imported is there, that the full wavelength readings are there, and that all six colour spaces are there.
3. It also tells me each colour space value will say which illuminant, observer, and measurement condition produced it — so a year from now I can still tell what a number means.
4. I choose current readings rather than full history, and where to put the file.
5. The file is written. Nothing in my collection changed.
6. I open it. The sRGB columns sit next to columns naming the space they were converted into, the rendering intent used, and whether that value fell outside the gamut. The swatches my monitor can't show honestly are marked in the file, not just in the app.
7. There's a `simulated` column too, so the readings I took against the Demo Device while testing are obvious rather than mixed in silently.

**If there's nowhere to write it.** No room, no permission, or the folder is gone: nothing is written, no half-finished file is left, and the app says which of the three it was and offers somewhere else. I never end up with a truncated CSV I might mistake for a complete one.

**If I ask for full history.** I get every version of every swatch instead of just the current one. The export surface always says which of the two it is doing before it runs, so I am never guessing.

## DJ2. Fix a bad scan by measuring it again

**Cataloger.** Serves [U5](../vision.md#use-cases). Exercises [R2.2](prd-data-foundation.md#2-canonical-value-and-version-history)–[R2.5](prd-data-foundation.md#2-canonical-value-and-version-history).

1. Looking through the collection I notice a swatch whose colour looks wrong — I think the instrument wasn't flat on it.
2. I scan it again. The app takes a full set of samples exactly as capture does, and doesn't show me the old value while I'm doing it, so the old reading can't lead me.
3. It asks me one question: has the swatch itself changed, or was the old reading wrong?
4. I say the old reading was wrong. That is a correction: the new reading becomes the swatch's value and the old one is kept but marked as never having been true.
5. Later, when I look at how that swatch has changed over time, the bad reading isn't in the chart. A reading I know is wrong plotted alongside good ones would be a lie told with real numbers ([browsing v2 §7](../../briefs/browsing-a-collection-at-scale-research-results-v2.md#correction-vs-re-measurement--bitemporality-is-the-exact-standardised-answer)).
6. Nothing was deleted. The swatch's history still holds every reading, every attempt, and every decision.

**When the swatch really has changed.** An old marker has faded and I re-measure it. I answer "the swatch has changed" and both readings stay in its timeline as legitimate points, because both were true — at different times.

**When I change my mind.** I restore the earlier reading. That writes a new current value equal to the old one rather than deleting anything, so I can undo the undo.

**Why the app asks rather than working it out.** Two scans a week apart look identical whether the marker faded or the first scan was botched — nothing in the data separates them, so a silent default would invent an answer. It asks me once, after the heads-down run rather than in the middle of it — a re-scan I take during capture is filed as a correction until I answer.

## DJ3. Open a file made by an older or a newer app

**Cataloger.** Serves [U5](../vision.md#use-cases) and [U6](../vision.md#use-cases). Exercises [R5.1](prd-data-foundation.md#5-migration-and-compatibility)–[R5.5](prd-data-foundation.md#5-migration-and-compatibility).

1. I update SpectroCapture and open the file I've been scanning into for a year.
2. The app says it needs to get the file ready for this version, and that it is saving a copy of the file exactly as it is now first, and where that copy is.
3. It finishes. Everything is where I left it — my collections, my counts, my history.

**If the upgrade stops partway.** The file is exactly as it was. The app names the version it found and the version it expected, and points at the copy it took before it started. Nothing was half-changed and nothing is lost.

**If an upgrade would lose a reading.** It doesn't happen. The app refuses rather than performing it.

**If I open the file on a machine running an older app.** I get a plain statement that this file was made by a newer version, the version numbers of both, and a read-only view. I am never shown a partial read of a file the app doesn't fully understand, which would look like data loss.

**If one reading can't be read.** That single swatch is set aside and named, and the rest of the collection opens and works normally. A catalogue of 1,200 swatches must not become unopenable because one measurement is damaged — and because the raw reading is the real data, a damaged one is something I need to see rather than something to hide ([browsing v2 §10](../../briefs/browsing-a-collection-at-scale-research-results-v2.md#10-empty-loading-and-error-states)).

**If the whole file is damaged.** It opens read-only, and the app offers to write everything it can still read into a fresh file while leaving the original untouched. It never repairs my file behind my back.

**Keeping my own copy.** I ask the app for a copy rather than copying the file in Finder while the app is open, because copying a database that is in use is one of the documented ways to end up with a broken copy — and "the file is the sync strategy" means people will put it in a folder that syncs itself.

## DJ4. Delete a swatch

**Cataloger.** Serves [U5](../vision.md#use-cases). Exercises [R6.1](prd-data-foundation.md#6-deletion-and-privacy)–[R6.4](prd-data-foundation.md#6-deletion-and-privacy).

1. A swatch got into the collection twice under two codes and I want one of them gone.
2. I delete it. Deleting isn't the button my hand lands on by accident — it is never the default action on the surface.
3. The app tells me exactly what goes: the reading it has now, the earlier readings behind it, and everything recorded about scanning it, each counted. It offers to export first.
4. I go ahead. That is the one place in this app where data is genuinely thrown away, and it happened because I asked for it by name.
5. I realise immediately it was the wrong one and undo. The swatch comes back with its history intact — not as an empty row with the right code on it.

**What I can't do.** I can't delete a single reading out of a swatch's history. Corrections never destroy data; if the earlier reading was wrong, that is a correction, not a deletion ([DJ2](#dj2-fix-a-bad-scan-by-measuring-it-again)).

**What a delete never reaches.** Removing a saved instrument from the app doesn't touch any measurement: the instrument's details are recorded on each reading at the moment it was taken and outlive the device record entirely.

**How long the undo lasts.** Until I quit the app. After that the delete is final, and the app says so before I confirm it.

## DJ5. Query the file without the app

**Data consumer.** Serves [U6](../vision.md#use-cases) — the promise that makes the file mine rather than the app's. Exercises [R1.1](prd-data-foundation.md#1-the-file-the-user-owns)–[R1.3](prd-data-foundation.md#1-the-file-the-user-owns), [R3.2](prd-data-foundation.md#3-derived-values-and-gamut-honesty).

1. SpectroCapture is closed. I open my file in the `sqlite3` that came with my Mac.
2. I can read an item's identity, its current colour values, and the illuminant, observer, and measurement condition each of those came from — with nothing installed and no function the app had to register.
3. I can tell which of a swatch's readings is the current one without asking the app.
4. I can find which values are marked as outside the display gamut, and take that honesty into whatever I build next.
5. I move the file to a different Mac and it still opens. I keep a copy on a drive and it still opens.

**What isn't safe.** Writing to the file while the app has it open. The app can't see another program's write and will keep showing me what it last read — so it offers me a re-read, and the help docs say plainly which direction is safe. No SQLite library detects an out-of-process write; this is a property of the format, not a gap in the app ([browsing v2 §9](../../briefs/browsing-a-collection-at-scale-research-results-v2.md#the-finding-that-should-shape-the-architecture)).

**What version of sqlite3 I need.** SQLITE_READER_FLOOR, candidate 3.31.0 — [OQ 2](prd-data-foundation.md#open-questions) — stated in the help docs, so I know before I try rather than after.
