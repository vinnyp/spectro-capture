# Data Foundation PRD — user journeys

Companion to [prd-data-foundation.md](prd-data-foundation.md): what the user does and sees, journey by journey.
The requirement rows in that file are the rules; nothing here adds one, and the shipping copy is in [prd-data-foundation-copy.md](prd-data-foundation-copy.md).

DJ1, exporting the collection, moved to [the export PRD's EJ1](../export/prd-data-export-journeys.md#ej1-export-the-collection) under fence F30 and its number is retired here; DJ2–DJ5 keep the numbers they have.

States are named in plain language; each one's shipping copy is in the copy file, and the vocabulary is defined once in the [PRD](prd-data-foundation.md#vocabulary). Persona is named per journey. Citation shorthand: [browsing v2](../../briefs/browsing-a-collection-at-scale-research-results-v2.md) is the collection-browsing research (v2 supersedes the first pass), [acq v2](../../briefs/acquisition-experience-research-results-v2.md) the acquisition-experience research, [SDK audit](../../briefs/nix-universal-sdk-audit-findings.md) the vendor SDK audit.

## DJ2. Fix a bad scan by measuring it again

**Cataloger.** Serves [U5](../vision.md#use-cases). Exercises [R2.2](prd-data-foundation.md#2-canonical-value-and-version-history)–[R2.5](prd-data-foundation.md#2-canonical-value-and-version-history), [R2.8](prd-data-foundation.md#2-canonical-value-and-version-history).

1. Looking through the collection I notice a swatch whose colour looks wrong — I think the instrument wasn't flat on it.
2. I scan it again. The app takes a full set of samples exactly as capture does, keeps every one of them, and doesn't show me the old value while I'm doing it, so the old reading can't lead me.
3. It asks me one question: has the swatch itself changed, or was the old reading wrong?
4. I say the old reading was wrong. That is a correction: the new reading becomes the swatch's value and the old one is kept but marked as never having been true.
5. Later, when I look at how that swatch has changed over time, the bad reading isn't in the chart. A reading I know is wrong plotted alongside good ones would be a lie told with real numbers ([browsing v2 §7](../../briefs/browsing-a-collection-at-scale-research-results-v2.md#correction-vs-re-measurement--bitemporality-is-the-exact-standardised-answer)).
6. Nothing was deleted. The swatch's history still holds every reading, every attempt, and every decision.

**When the swatch really has changed.** An old marker has faded and I re-measure it. I answer "the swatch has changed" and both readings stay in its timeline as legitimate points, because both were true — at different times. The timeline puts them where they were measured, not where they happened to be filed.

**When I change my mind.** I restore the earlier reading. That writes a new current value equal to the old one rather than deleting anything, and it carries the date that reading was actually measured, so I can undo the undo without the timeline shifting.

**Why the app asks rather than working it out.** Two scans a week apart look identical whether the marker faded or the first scan was botched — nothing in the data separates them, so a silent default would invent an answer. It asks me once, after the heads-down run rather than in the middle of it.

**When I don't answer yet.** A re-scan I take during capture sits as an unconfirmed correction: the new reading is the swatch's value, and the old one is kept and marked as not yet settled — shown in the timeline rather than dropped out of it, because the app hasn't been told it was wrong. I can say "Ask me later" as often as I like and nothing ever quietly decides for me.

**Answering them all at once.** After a session the app offers me every unconfirmed re-scan together, each with the date it was taken, so I work through the whole run in one pass instead of hunting swatch by swatch.

## DJ3. Open a file made by an older or a newer app

**Cataloger.** Serves [U5](../vision.md#use-cases) and [U6](../vision.md#use-cases). Exercises [R2.9](prd-data-foundation.md#2-canonical-value-and-version-history), [R5.1](prd-data-foundation.md#5-migration-and-compatibility)–[R5.8](prd-data-foundation.md#5-migration-and-compatibility).

1. I update SpectroCapture and open the file I've been scanning into for a year.
2. The app says it needs to get the file ready for this version, and that it is saving a copy of the file exactly as it is now first, and where that copy is. It shows me how far along it is rather than leaving me guessing.
3. It finishes. Everything is where I left it — my collections, my counts, my history.
4. That copy is still there afterwards. The app can show me where it is whenever I ask, and it stays until I remove it, because it is my way back to the version I was running before.

**If there isn't room for the copy.** The upgrade is never started. I'm told to free up space, and my file is untouched.

**If the upgrade stops partway.** The file is exactly as it was. The app names the version it found and the version it expected, and points at the copy it took before it started. Nothing was half-changed and nothing is lost.

**If an upgrade would lose a reading.** It doesn't happen. The app refuses rather than performing it, and my file comes out of the attempt byte for byte as it went in.

**If I open the file on a machine running an older app.** I get a plain statement that this file was made by a newer version, the version numbers of both, and a read-only view. I am never shown a partial read of a file the app doesn't fully understand, which would look like data loss.

**If a future release stops upgrading files this old.** The app says so instead of trying: it names my file's version, its own, and the last app version that can still change my file, and opens it read-only. Today there is no such file — v1 writes the first version there is.

**If one reading can't be read.** That single swatch is set aside and named, and the rest of the collection opens and works normally. The swatch has no value until I scan it again or go back to one of its earlier readings, which are all still there — nothing is quietly promoted into the gap. A catalogue of 1,200 swatches must not become unopenable because one measurement is damaged, and a damaged reading is something I need to see rather than something to hide ([browsing v2 §10](../../briefs/browsing-a-collection-at-scale-research-results-v2.md#10-empty-loading-and-error-states)).

**If the whole file is damaged.** It opens read-only, and the app offers to write everything it can still read into a fresh file while leaving the original untouched. It never repairs my file behind my back.

**If the disk fills up under me.** The app says so and my file is exactly as it was before the write it couldn't finish. There is never a half-written reading, and never a file that won't open next time.

**Keeping my own copy.** I ask the app for a copy rather than copying the file in Finder while the app is open, because copying a database that is in use is one of the documented ways to end up with a broken copy — and "the file is the sync strategy" means people will put it in a folder that syncs itself. If mine is in one, the app told me the risk when I chose it.

## DJ4. Delete a swatch

**Cataloger.** Serves [U5](../vision.md#use-cases). Exercises [R6.1](prd-data-foundation.md#6-deletion-and-privacy)–[R6.4](prd-data-foundation.md#6-deletion-and-privacy).

1. A swatch got into the collection twice under two codes and I want one of them gone.
2. I delete it. Deleting isn't the button my hand lands on by accident — it is never the default action on the surface.
3. The app tells me exactly what goes: the reading it has now, the earlier readings behind it, and everything recorded about scanning it, each counted. The counts are of readings — the samples inside a reading aren't counted at me as though they were separate scans. It offers to export first.
4. I go ahead. Deleting is the only thing that throws away a reading, and it happened because I asked for it by name.
5. I realise immediately it was the wrong one and undo. The swatch comes back with its history intact — not as an empty row with the right code on it.

**If I take the export first.** Choosing it runs the export and does not delete anything: I come back to the same confirmation with the swatch still there, and delete once I have the file in hand.

**Throwing away a whole collection.** Same thing at collection scale: I'm told how many swatches and how many readings go with it, and offered an export first. It's the same undo, and the same finality after.

**What I can't do.** I can't delete a single reading out of a swatch's history. Corrections never destroy data; if the earlier reading was wrong, that is a correction, not a deletion ([DJ2](#dj2-fix-a-bad-scan-by-measuring-it-again)).

**Throwing away the whole file.** That isn't something the app does — it's my file, and I delete it in Finder like any other file of mine. The help docs say so plainly rather than leaving me hunting for a menu item.

**What a delete never reaches.** Removing a saved instrument from the app doesn't touch any measurement: the instrument's details are recorded on each reading at the moment it was taken and outlive the device record entirely.

**How long the undo lasts.** Until I quit the app. After that the delete is final, and the app says so before I confirm it. If the app crashes with an undo still pending, the delete stands — a crash isn't a quit I chose, but it isn't a rescue either, and the app never pretends otherwise. Once that window has closed, nobody reading my file in another tool can dig the deleted swatch back out of it.

## DJ5. Query the file without the app

**Data consumer.** Serves [U6](../vision.md#use-cases) — the promise that makes the file mine rather than the app's. Exercises [R1.1](prd-data-foundation.md#1-the-file-the-user-owns)–[R1.3](prd-data-foundation.md#1-the-file-the-user-owns), [R1.6](prd-data-foundation.md#1-the-file-the-user-owns), [R3.2](prd-data-foundation.md#3-derived-values-and-gamut-honesty), [R5.6](prd-data-foundation.md#5-migration-and-compatibility).

1. SpectroCapture is closed. I open my file in the `sqlite3` that came with my Mac.
2. The first thing I can read is what version of the file format it is — in one place, in one form, and in the same place it will be in every future version. I know what I'm looking at before I interpret anything else in it.
3. I can read an item's identity, its current colour values, and the illuminant, observer, and measurement condition each of those came from — with nothing installed and no function the app had to register.
4. I can tell which of a swatch's readings is the current one without asking the app, why each of the others was superseded, and which are marked as never having been true.
5. I can find which values are marked as outside standard sRGB, and take that honesty into whatever I build next.
6. I move the file to a different Mac and it still opens. I keep a copy on a drive and it still opens.

**The one thing I can't read on my own.** There are archived columns holding the instrument's own opaque string, one for each sample behind a reading. They're there so nothing is lost, and they may be compressed — but nothing I need is locked inside them. Every value, condition, mark, and reason above, and every sample's own numbers, is stored in plain columns beside them.

**What isn't safe.** Writing to the file while the app has it open. The app can't see another program's write and will keep showing me what it last read — so a re-read is always available in the app, and the help docs say plainly which direction is safe. No SQLite library detects an out-of-process write; this is a property of the format, not a gap in the app ([browsing v2 §9](../../briefs/browsing-a-collection-at-scale-research-results-v2.md#the-finding-that-should-shape-the-architecture)).

**What version of sqlite3 I need.** SQLITE_READER_FLOOR — [OQ 2](prd-data-foundation.md#open-questions), whose candidate is the `sqlite3` shipping with the app's minimum macOS version once that floor lands — worked out from what the file actually uses and stated in the help docs, so I know before I try rather than after.
