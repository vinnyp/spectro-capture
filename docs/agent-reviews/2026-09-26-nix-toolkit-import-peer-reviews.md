# Peer reviews — Nix Toolkit export import amendment (2026-09-26)

The review log for the amendment that brings a Nix Toolkit (vendor mobile app) collection export into v1 with its readings. It amends the Inventory Import PRD (its §6, fences F69–F83) and mirrors into Data Foundation, Device Management, Collection Mode, Data Export, Capture Mode and the vision. Every later round appends a `## Round N` section here.

## Round 0 — owner adjudication (2026-09-26)

The owner decided the amendment's fifteen WHAT points before any drafting, from a research report on a real Toolkit export (the export itself stays out of the repository, decision N3) and an inventory of the locked rows it touches. The decisions are recorded verbatim below; each fence cites its decision by number. Two values quoted from the real export in the owner's questions are redacted in brackets under N3.

Taken in the orchestrator session via AskUserQuestion; recommended option listed first in each question.

### Earlier (while PR #21 was in review)
- **N1 Scope.** Q: "When should SpectroCapture import a Nix Toolkit collection export with its readings?" Chosen: **v1, readings included** — "Pull J7 forward for this one format: each row becomes a measured item. Needs an Inventory Import + Data Foundation amendment before build. CxF stays v2." Not chosen: v2, with CxF; v1 inventory, readings v2.
- **N2 Reading.** Q: "When a row's reading is imported, what does it become?" Chosen: **Canonical, marked imported** — "It becomes the item's canonical value, with its provenance (Toolkit, device model, date, D50/2°, M2); derived values are recomputed from its spectrum. A later scan supersedes it into history like any re-scan." Not chosen: Canonical, flagged to re-verify; Reference only, still to scan.
- **N3 Fixture.** Q: "May this file go into the public repo as a test fixture?" Chosen: **Keep it out of the repo** — "Used only locally for research; fixtures stay synthetic." Not chosen: Commit as is; Commit with dates shifted.

### Batch 1
- **N4 Home.** Q: "Where should the Toolkit-import behaviour live?" Chosen: **Extend Inventory Import** — "Same import flow the user already knows (pick file, map, preview, commit); Import gains a Toolkit section with room to spare (3,121 words, no cap). Siblings (DF, Device, Collection Mode, Export, Capture, vision) get small dated mirror amendments." Not chosen: A new PRD for vendor-app import.
- **N5 Detect.** Q: "How should the app recognise a Toolkit export?" Chosen: **Auto-detect, then confirm** — "The header signature (Nix Device, Illuminant, Measurement Mode, R400–R700 nm …) identifies it; the preview says 'Nix Toolkit export — [the file's reading count] readings' and the user confirms before commit. Semicolons, the duplicate L header and 'undefined' notes handled automatically." Not chosen: User picks the format.
- **N6 Conflicts.** Q: "When an item in the target collection already has a reading scanned in SpectroCapture, what happens to the Toolkit reading for it?" Chosen: **Scan wins; import to history** — "The SpectroCapture scan stays canonical; the Toolkit reading is kept in the item's version history as an earlier reading. Metadata still updates as a normal re-import." Not chosen: Newest reading wins; Skip the reading.
- **N7 Mark.** Q: "What does 'marked imported' look like to the user?" Chosen: **A table mark, like simulated** — "A tenth honesty mark on the row/chip ('imported from Nix Toolkit'), with the device and date in the item detail and history; it clears when the item is re-scanned, the imported reading staying in history with its mark." Not chosen: Detail and history only.

### Batch 2
- **N8 Scan mode.** Q: "The export records one measurement mode per file (yours is M2, UV-excluded); a collection has one chosen scan mode (the Spectro 2 defaults to M1). How should they meet?" Chosen: **Match or refuse** — "A new collection adopts the file's mode; an existing collection must already use it (or have no readings yet, and then switches). Otherwise the preview refuses with the reason, since readings under different modes aren't comparable." Not chosen: Import anyway.
- **N9 Provenance.** Q: "The export lacks things a SpectroCapture reading always has: sample count, device serial and firmware, and the raw payload. What gets stored?" Chosen: **Record them as unknown** — "Samples 'not recorded', device 'Nix Spectro 2, serial unknown', raw payload 'none supplied' (a state distinct from a damaged archive). Nothing is invented; the spectrum is kept and every derived value recomputed from it." Not chosen: Assume a single sample.
- **N10 Timestamp.** Q: "Which time does an imported reading carry?" Chosen: **Date Saved is when measured** — "The reading's measurement time is the Toolkit's Date Saved; its record time is the import commit. History sorts it by when it was actually measured." Not chosen: Import time only.
- **N11 Export & QC.** Q: "How do export and QC comparisons treat an imported reading?" Chosen: **Mark in export; QC as normal** — "CSV export gains an imported-provenance column beside the simulated one; a QC ΔE comparison against an imported canonical value works like any other, with the imported mark showing." Not chosen: Mark in export; QC needs a scan.

### Batch 3
- **N12 Budgets.** Q: "Two locked PRDs are out of word budget: Data Foundation (8,473 of 8,480) and Collection Mode (12,399 of 12,400). The amendment needs roughly 100 words in DF (provenance, R2.2's exception, scope) and about 60 in Collection Mode (the imported mark and its history line). How should they fit?" Chosen: **Raise DF to 8,600, CM to 12,500** — "Dated fences like F214/F220; the new rules land in plain words and no aligned row reopens for compaction." Not chosen: Compact to fit.
- **N13 File values.** Q: "The export also carries the Toolkit's own computed values (Lab, LCh, XYZ, sRGB, HEX) and CMYK densities. What happens to them?" Chosen: **Recompute and check** — "SpectroCapture recomputes every value from the spectrum; the file's Lab is compared, and a row differing by more than the derivation tolerance is flagged in the preview. Densities (no v1 feature) are kept as imported metadata columns." Not chosen: Keep the file's values.
- **N14 Re-import.** Q: "Re-importing a Toolkit export (or a newer one) for items whose current reading was itself imported: what happens?" Chosen: **Newest imported wins** — "An identical reading (same date and spectrum) changes nothing; a newer Toolkit reading becomes canonical and the older imported one goes to history. A SpectroCapture scan still always wins." Not chosen: First import stands.
- **N15 Name.** Q: "The export names its Toolkit collection ('[the file's collection name]'). What does the importer do with it?" Chosen: **Pre-fill a new collection** — "When importing into a new collection, its name is pre-filled from the file (editable); when importing into an existing collection, the file's name is ignored." Not chosen: Ignore it.


## Round 1 — nine lenses (2026-09-26)

Subject: commit 417fdac, rewritten as 2a54fce before any push (see below), branch `docs/nix-toolkit-import` off `main` 6374538; scope `git diff 6374538 <subject> -- docs`. Briefs were built with `review-gate brief --mode requirements`, each carrying its lens's retargeting line, the owner decisions N1–N15 and every PRD's fence file as sources; the local format report on the real export was a source too, with the instruction to quote format facts only.

### Lenses and why

The four standing lenses (product manager, staff software engineer, test, interface), plus: privacy, because the amendment governs a user's own collection data and a real export sits at the repository boundary; product marketing, for the new user-facing copy (E43's Toolkit variant, E46, E47, the imported mark, Export E1); architecture, for the device-kind boundary and the schema one-way doors; database, for what the SQLite file holds for an imported reading; and plan, because an unattended build runs from these amended PRDs. No cross-model pass ran: sending the briefs out would carry the format report's real values to a third-party provider (PRIV-7), so any later one gets a format-only extract.

### Orchestrator verification

Every Blocker and Major was checked against the branch by grep before it was accepted.
- **Real export values.** PRIV-1, SSE1-B3 and PL1-M4 reproduced by count-only greps against the real export: Fixture T's three Date Saved values and TK-3's name each occurred once in it. PRIV-2's two quoted values sat in Round 0. The values were replaced with invented ones and Round 0 redacted, and the unpushed commit 417fdac was rewritten as 2a54fce, so no pushed commit ever held them. A local-only check, never committed, now finds no real-export value in the diff. The reviews below were run through the same check and five values redacted in brackets.
- **Everything else.** All other Blockers and Majors reproduced as stated:
  - the same-reading gap across R6.8b–d and DF R2.3j;
  - the E43 count contradiction between UJ 3's two cases;
  - the history-order conflict with DF R2.1 and Collection Mode R5.3;
  - DF's unamended reading model;
  - the missing fixture and golden for `sc_imported`;
  - the mode adoption outside the commit;
  - E14's promise;
  - E12's missing meaning;
  - E46's shared headline;
  - `AGENTS.md` §3 and §8.
- **Not reproduced.** None.

### Owner adjudication (2026-09-26)

Taken in the orchestrator session via AskUserQuestion; recommended option listed first in each.

- **N16 History order.** Q: "You decided (N10) that an imported reading's history 'sorts by when it was actually measured'. But Data Foundation R2.1 and Collection Mode's history list order by when a reading was recorded, and history's Measured view already orders by measurement date. Which does N10 mean?" Chosen: **Measured view only** — "The over-time views and history's Measured view place an imported reading by its Date Saved. The default recorded-order list keeps it where it was recorded (the import). No locked row changes." Not chosen: Recorded list too.
- **N17 Reason.** Q: "When a newer Toolkit reading replaces an older imported one as current (your N14), which reason does the older one get? Data Foundation R2.4 says the app asks instead of guessing. 'Re-measurement' keeps the old reading as a valid earlier point; 'correction' marks it never true." Chosen: **Re-measurement, no question** — "Recorded as a re-measurement automatically, with an explicit exception written into R2.4. A reading kept behind another is recorded as 'initial'." Not chosen: Ask, like a re-scan.
- **N18 Demo reading.** Q: "An item's current reading came from the Demo Device (simulated), and a real Toolkit reading for it is imported. Your N6 says a SpectroCapture scan stays current. Does that include a simulated one?" Chosen: **Toolkit reading wins** — "A simulated reading isn't a measurement: the real Toolkit reading becomes current and the simulated one goes to history. Only a live scan stays current over a Toolkit reading." Not chosen: Any SpectroCapture reading stays.
- **N19 Tie.** Q: "A Toolkit record for an item whose current reading was imported has the same Date Saved but a different spectrum (neither newer nor older). What happens?" Chosen: **Keep in history, list it** — "The current reading stays; the incoming one is kept in history and the preview lists it as a same-date reading that differs." Not chosen: It becomes current.
- **N20 Set aside.** Q: "A Toolkit record matches an item that is set aside with no current reading (skipped, or flagged as missing or damaged in a session). What happens to it?" Chosen: **Becomes captured, listed** — "The Toolkit reading becomes its current value and the item is captured; the preview lists these items by name so you see the set-aside decision being resolved before you commit." Not chosen: Stays set aside.
- **N21 Many sets.** Q: "A Toolkit file holds records from more than one Toolkit collection (different Custom Collection Names). How should the import treat it?" Chosen: **Refuse; one collection per file** — "Refused with the reason, like mixed modes: export each Toolkit collection on its own and import each. Keeps codes from different Toolkit collections from colliding." Not chosen: Merge into one collection.
- **N22 Unchecked.** Q: "A record's spectrum is fine but the file's own Lab can't be read, or its illuminant/observer is one the app can't work under yet (the app offers D50/2° until a later decision). What happens to that record?" Chosen: **Import it, listed unchecked** — "The reading still imports (its values are worked out from the spectrum under the collection's reference); the preview lists it as not checked against the Toolkit's own values." Not chosen: Exclude it.
- **N23 Budgets.** Q: "The fixes land mostly in Data Foundation (its reading definition, the predecessor rule, history order, a fixture row, the ADR-0003 inputs). It is at 8,592 of 8,600 words; Collection Mode is at 12,497 of 12,500. How should they fit?" Chosen: **Raise DF to 8,900, CM to 12,550** — "Dated budget fences like F65/F222; the fixes land in plain words and no aligned row reopens for compaction." Not chosen: Compact to fit.

### What landed

The fix file [`prd-inventory-import-round-1-fixes.md`](../product/import/prd-inventory-import-round-1-fixes.md) lists every finding by ID and where it landed. It covers:
- the same-reading rule;
- the reading counts;
- the value grammar and the fixture format;
- the mode adoption inside the commit;
- every Toolkit column's fate;
- the `undefined` Note;
- recognition under any separator;
- E14's and E43's Toolkit variants, E46, E48 and E49;
- Data Foundation's reading model, predecessor rule and fixture;
- Collection Mode's E12, copy lines and cases;
- Export's `sc_imported` assertions;
- the obligation lines both ways;
- the ADR-0003 inputs;
- `AGENTS.md`;
- OQ 3 and M2.

Owner decisions N16–N23 are fences Import F84–F91, DF F66–F70 and Collection Mode F223, with dated Clarified lines under Import F71, F73, F76 and F82, DF F64, Collection Mode F221, Device F33, Export F35 and Capture F77.

### product-manager

#### Verdict
Sound after fixing Blockers. The amendment builds the right thing for a Cataloger who already measured in the Nix Toolkit, and its first-import happy path is complete. But one self-contradictory re-import rule would put duplicate readings into history that can never be removed. Several Majors also leave the realistic unhappy paths either unwritten or silently lossy, and the format contract rests on a single export.

#### User & problem context (brief)
- **User and job.** The user is the Cataloger (the owner is also the first user) who has already scanned some or all of a physical collection in the Nix Toolkit mobile app. The job: get those readings into a SpectroCapture collection without re-scanning, with honest provenance, then keep scanning, correcting and QC-ing on top of them (vision J7 pulled into v1).
- **Validated.** One real export: the owner's, one device (Spectro 2), one mode (M2), one illuminant/observer (D50/2°), one collection, one locale. Its spectrum reproduces its own Lab within 0.01 ΔE2000.
- **Assumed.** That every other Toolkit export has the same shape: delimiter and spacing, number and date formats, codes always filled, one collection per file, one device per file.
- Paths below are relative to `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/`.

#### Findings

**[BLOCKER] PM1 — Re-importing puts duplicate readings into history, which can never be removed.**
- **Where:** `import/prd-inventory-import.md:180` (R6.8c), `:181` (R6.8d), `:171` (R6.9), `:78` (R3.3), and `data-foundation/prd-data-foundation.md:137` (R2.3j).
- **Defect:**
  - R6.8c says that behind a SpectroCapture scan "the reading is kept in version history". It never checks whether that reading is already there.
  - R6.8d and DF R2.3j compare an incoming reading only with the *current* one. So "an earlier one goes to history" also re-adds a reading that is already in history.
  - This contradicts R6.9 and R3.3's promise that "repeating a committed export with no intervening edits changes nothing … readings included". Under R6.8c even a plain repeat adds a copy.
- **Scenario:** The Cataloger imports their Toolkit export and later scans 10 of the items. Months later they add colours in the Toolkit and re-import the whole grown export, which is the re-import case N14 exists for.
  - Every scanned item gains a second, identical Toolkit reading in history.
  - E43 reports them as "Kept in history: 10", which looks like new data.
  - Because DF R2.3 says only deleting the item removes a reading, the duplicates are permanent. They also appear in the P1 history export.
  - Re-importing an older export after a newer one does the same.
- **Mutation that survives today:** a build that appends the Toolkit reading on every R6.8c/R6.8d-earlier match, exactly as the rows read, passes every UJ 3 and DJ6 case. The only re-import case (`import/prd-inventory-import-journeys.md:119`) starts from a state where all three items are imported-current, so R6.8c is never re-exercised.
- **Fix:**
  - Add to R6.8 and to DF R2.3j: "a reading equal to one the item already holds, current or in history (same Date Saved and every reflectance), changes nothing and counts as unchanged".
  - Add a UJ 3 case: TK-1 is scanned, commit Fixture T, import Fixture T again. E43 must show TK-1 among the unchanged readings and 0 kept in history, and TK-1's history must hold exactly one imported reading.
  - Add the same case for re-importing an older export.

**[MAJOR] PM2 — A Demo Device reading outranks a real Toolkit reading.**
- **Where:** `import/prd-inventory-import.md:180` (R6.8c "scanned in SpectroCapture") and `data-foundation/prd-data-foundation.md:137` (R2.3j "else kept behind A, which stays current").
- **Defect:** The natural reading of R2.3j keeps *any* non-imported current reading current, simulated ones included. F74's reason for letting a scan win — "a measurement taken in this app, under its own agreement check" — does not describe a Demo Device reading.
- **Scenario:** The Cataloger tries the app with the Demo Device on their real collection, then imports their real Toolkit export. The fake colours stay canonical and the real measurements go to history, one "Use this reading" (P1) at a time to undo.
- **Status:** Neither validated nor decided; N6 speaks only of "a reading scanned in SpectroCapture".
- **Fix:** Name the snapshot kind in R6.8c ("current value of the live kind"). Add R6.8f for a simulated current value, with the owner deciding whether the imported reading becomes current. Add a UJ 3 case and mirror it in R2.3j.

**[MAJOR] PM3 — Importing silently changes an existing collection's scan mode.**
- **Where:** `import/prd-inventory-import.md:166` (R6.4 "hold no reading and then adopt it"), `:171` (R6.9), `import/prd-inventory-import-copy.md:29` (E43 Toolkit variant), `import/prd-inventory-import-journeys.md:117`.
- **Defect:** The E43 preview names the file's mode but never says the target's own chosen scan mode will change.
- **Scenario:** This is the natural inventory-first flow.
  - The Cataloger creates "Copic Sketch" at M1, imports the maker's full CSV (all rows pending), then imports the Toolkit export of the 40 they already measured at M2.
  - The collection flips to M2 with no mention in the preview.
  - The next session scans the remaining 300 at M2 and runs the agreement check at M2, not the mode they chose.
- **Fix:** R6.9 and E43's Toolkit variant add "⟨collection⟩'s scan mode changes from ⟨old⟩ to ⟨file mode⟩" when R6.4's adopt branch applies. State that the switch lands only at commit (Cancel leaves the mode unchanged). Assert both in UJ 3's case at `:117`.

**[MAJOR] PM4 — The "Colour marks" legend never explains the new imported mark.**
- **Where:** `collection-mode/prd-collection-mode-copy.md:173` (E12 body), against `collection-mode/prd-collection-mode.md:309` (R2.8: E12 shows each R2.4 mark "and what it means") and `:302` (R2.4j).
- **Defect:**
  - E12's "Where the reading came from" group explains Simulated, No spectral data and Samples disagreed, but has no "Imported reading:" sentence.
  - F221 says the mark shows "in … E12".
  - UJ2.1-k asserts E12 lists twelve marks *by identifier*, so a build with no meaning text passes.
- **Scenario:** After importing, the Cataloger sees "Imported reading" on [the file's reading count] chips and opens "Colour marks" to learn what they can trust. There is no answer, or one the builder improvised.
- **Fix:** Add the sentence to E12's body, for example: "Imported reading: the reading came from a Nix Toolkit export. Its colour is worked out from the spectrum the file holds; the instrument's serial and firmware, and how many samples were averaged, are unknown." Add the case to the owner's E12 dogfood reading.

**[MAJOR] PM5 — A re-saved or slightly different Toolkit export silently loses its readings.**
- **Where:** `import/prd-inventory-import.md:163` (R6.1 "split on semicolons … Any other source follows §1–§3 unchanged").
- **Defect:** A Toolkit export that has passed through Numbers or Excel (re-saved with commas), or a slightly different Toolkit version, does not match the signature. It falls to the generic path with no signal.
  - **Why this is common:** Import's own M1 assumes users work in exactly those spreadsheet apps.
  - **Result:** [the file's reading count] *pending* items, an E41 notice about the two L columns, and about 50 metadata columns (R400 nm … R700 nm, L, a, b, …). R3.6 says added columns persist in the collection's column list, so the only clean undo is deleting the collection. The readings are lost.
- **Status:** N5 decided "the header signature identifies it". Tying detection to the semicolon was a drafting choice.
- **Fix:** Match the header signature under any offered delimiter. When the headers match but the file is not in the Toolkit's form, show a near-miss state, for example "This looks like a Nix Toolkit export that's been re-saved. Its readings come in only from the file the Toolkit exported — export it again, or import it as an inventory without readings." Add a UJ 3 case with Fixture T re-saved with commas.

**[MAJOR] PM6 — Every "fix the file" remedy is a dead end for a Toolkit export.**
- **Where:** `import/prd-inventory-import.md:164` (R6.2 "fixed … never chosen"), `:165` (R6.3), and `import/prd-inventory-import-copy.md:19` (E9), `:20` (E10), `:28` (E42), `:33` (E47).
- **Defect:** For a Toolkit file, the remedies "fill the codes in and try again", "fix the file and try again" and "fix the export" can't be followed. The mapping can't be changed, and editing the file re-saves it out of the Toolkit form (PM5).
- **Scenario A:** The Toolkit collection was saved without Color Codes. That is unknown for Toolkit users generally; the one export studied had all codes filled because the owner catalogs markers. Every row hits E9, then E42. The user cannot choose Color Name as the code.
- **Scenario B:** A file holding several Toolkit collections, which R6.3 itself anticipates and UJ 3 at `:121` exercises.
  - Everything merges into one SpectroCapture collection.
  - Codes that repeat across the Toolkit collections are all excluded by E10.
  - The grouping is lost, because R6.2 never says whether Custom Collection Name is stored.
- **Fix:** Owner decides three things:
  - a fallback when Color Code is blank (Color Name as the code, or offer a choice of code column only when Color Code is blank);
  - whether a multi-collection file is refused like mixed modes ("export each collection separately") or merged with Custom Collection Name kept as metadata;
  - Toolkit variants of the E9, E10, E42 and E47 remedy text that point back to the Toolkit.

**[MAJOR] PM7 — The format contract is generalized from one export, and nothing tracks whether it holds.**
- **Where:** `import/prd-inventory-import.md:184-190` (only M1, which is for spreadsheet corpora), `:194-197` (OQ 1 and OQ 2 only), and `post-lock.md` Dogfood (no item added).
- **Defect:** R6.1–R6.7 fix delimiter, quoting, mode values, the Observer form ("2"), and the date and number forms from one file. No open question, metric or dogfood item checks them against a second export.
- **Scenario:** A user whose locale uses a decimal comma — which is why semicolon CSVs exist — may export `8,78740000e+01` or localized dates. That puts every row in E47, then E42: the primary job fails outright.
  - A Spectro L export, a newer Toolkit version, or a mixed-device collection (colorimeter rows with no spectrum show E47 "can't read", which is misleading) could fail the same way.
  - Nothing in the PRD would notice.
- **Fix:**
  - Add Import OQ 3, "Toolkit format variance", with the interim contract R6.1–R6.7 as written. Its closer is at least N real exports across locale, device and app version, with outcomes recorded.
  - Add M2: "share of real Toolkit exports imported with no E46, E47 or R6.6 flag", with an owner-proposal target.
  - Add a post-lock Dogfood line.
  - Scale: a small corpus is enough; no funnel or instrumentation is needed.

**[MAJOR] PM8 — The format facts R6.10's fixtures depend on are not in the repo.**
- **Where:** `import/prd-inventory-import.md:172` (R6.10 "built to the format the research records"), `import/prd-inventory-import-fences.md:169` (F71), `import/prd-inventory-import-journeys.md:104`.
- **Defect:** The research lives outside the repo, so the only format statement a builder has is UJ 3's prose.
  - **Numbers.** The prose never says Lab, LCh, XYZ and the reflectances are written in fixed scientific notation (`d.dddddddde±d`), that XYZ is on a 0–1 scale, or that densities have two decimals and can be negative.
  - **The one number example is in the wrong form.** TK-3's "one reflectance `1.047`" is plain decimal, and "stored as given" doesn't say whether the text or the number is kept.
  - **Separators.** The prose writes the header as "Index; Custom Collection Name; …". Whether a space follows each semicolon decides whether R1.5c ("double quotes delimit quoted fields … invalid quoting is E5") reads a data field like ` "[a code]"` at all.
- **Scenario:** A build tested on a plain-decimal, space-free Fixture T passes every case. If the real rows differ, it could reject a real export wholesale (E47 or E5), or store codes with literal quotes.
- **Status:** Assumed; not checkable from the repo.
- **Fix:** Put a format appendix in the repo — format facts only, no data values, consistent with N3. It should cover numeric serialization per column group, date form, HEX case, the exact separator bytes, and the header-versus-data quoting rule. Write TK-3's value as `1.04700000e+00`. Check the separator spacing against the real export locally before alignment.

**[MAJOR] PM9 — Illuminant and observer handling is unspecified, and the fixture can't catch a wrong build.**
- **Where:** `import/prd-inventory-import.md:167-169` (R6.5–R6.7), `import/prd-inventory-import-journeys.md:104`, `:111`.
- **Three gaps:**
  - **Unreadable values have no outcome.** R6.7 excludes unreadable Date Saved, Measurement Mode and reflectance values, but says nothing about an unreadable or unsupported Illuminant or Observer, or a blank Nix Device. Yet R6.6 needs the illuminant and observer.
  - **No case has a non-default reference.** Fixture T is D50/2°, the same as a new collection's default. A build that runs R6.6's check under the *collection's* reference instead of "the file's own" passes UJ 3. On a D65/10° export that build would flag every record as differing.
  - **The Toolkit-versus-SpectroCapture difference is unexplained.** A new target keeps D50/2° (OQ 21's interim single pair), so a Cataloger who used D65/10° in the Toolkit sees different Lab values in SpectroCapture. Nothing in E43 says why.
- **Fix:** Add Illuminant, Observer and Nix Device to R6.7's exclusions. Add a UJ 3 case with a D65/10° fixture into a D50/2° collection: 0 flagged, stored Lab under D50/2°. Add one E43 sentence when the file's reference differs from the collection's.

**Minor findings (one line each)**
- [MINOR] **PM-m1** `import/prd-inventory-import.md:164` — R6.2 dispositions every column except Custom Collection Name (a metadata column by R2.2's default, or dropped per R6.3's "ignores that column"?); UJ 3 at `:112` never checks it. State it.
- [MINOR] **PM-m2** `:164`/`:168` — R6.2 says LCh, XYZ, sRGB and HEX are "read for R6.6's check", but R6.6 uses only the first L, a, b. No outcome is given when the file's L, a, b are blank or unreadable (skip the check, flag, or exclude).
- [MINOR] **PM-m3** `:83` — R3.8 still says "apply R3.8a–o" though R3.8p now exists. UJ 2's every-state row (`import/prd-inventory-import-journeys.md:36`) still lists "E4–E14, E38, E40–E45", omitting E46 and E47's Cancel and Pick-again actions.
- [MINOR] **PM-m4** `import/prd-inventory-import-copy.md:29`/R3.8n — E43 offers "Choose an encoding"/"Choose a separator" on a detected Toolkit export. Does a manual choice override R6.1 (dropping to §1–§3), or is it withheld? Unstated.
- [MINOR] **PM-m5** `:178-182` — R6.8 leaves three cases open: a match whose current reading is quarantined (Collection Mode R2.4h treats that as distinct from "no current value"); an equal Date Saved with different reflectances; and a set-aside row the user deliberately left (Capture R8.5) quietly becoming captured under R6.8b.
- [MINOR] **PM-m6** `:166` — R6.4's "hold no reading" is ambiguous. Does a flagged reading in history, a quarantined reading, or a QC record count?
- [MINOR] **PM-m7** `import/prd-inventory-import-journeys.md:109` — when a Toolkit import creates the target, is the creation form's scan-mode field locked to the file's mode, or editable and then refused?
- [MINOR] **PM-m8** `import/prd-inventory-import-copy.md:24` — E14 promises "Your measurements aren't touched either way" in a Toolkit re-import where E43 reports readings becoming current or going to history. Add a Toolkit variant: "your choice here doesn't change which readings come in".
- [MINOR] **PM-m9** `data-foundation/prd-data-foundation.md:112` — DF R2.1 (and AGENTS §8) still define the canonical value as "the stored mean … averaging basis". An imported one has neither; F64 amends R2.3j but not R2.1.
- [MINOR] **PM-m10** `data-foundation/prd-data-foundation-journeys.md:114` and DF F64 — "history … place B … by its measurement time" contradicts DF R2.1 (history ordered by record time) and Collection Mode R5.3 (`collection-mode/prd-collection-mode.md:385`, newest-recorded first). In that default order the Toolkit reading lists *above* the current scan. Say that DJ6 asserts the measured order.
- [MINOR] **PM-m11** `collection-mode/prd-collection-mode.md:348` — R4.2d names serial, firmware, samples, basis, spread and verdict as unknown for an imported reading; the copy line (`collection-mode/prd-collection-mode-copy.md:295`) renders only serial, firmware and samples.
- [MINOR] **PM-m12** `import/prd-inventory-import-copy.md:29`, `:32` — Import says "measurement mode" while Collection Mode and Export copy say "measurement condition" for the same thing. Use one term the Cataloger sees everywhere.
- [MINOR] **PM-m13** `import/prd-inventory-import-copy.md:32` — Two problems with E46:
  - Its mixed-modes remedy, "Export each mode from the Toolkit separately", assumes a Toolkit feature the evidence doesn't show; the likely remedy is "each collection".
  - Its collection variant doesn't mention that Capture R1.10 (`capture-mode/prd-capture-mode.md:147`) lets the user switch this collection's scan mode. Switching away later blanks every imported chip (DF R3.3d) without warning.
- [MINOR] **PM-m14** `vision.md:160` against Capture R8.7–R8.10 — Toolkit import is P0 but re-scan is P1. In the first build the imported mark's "clears when re-scanned" has no path except Flag followed by the set-aside review. DJ6's re-scan row (`data-foundation/prd-data-foundation-journeys.md:115`) exercises a P1 row.
- [MINOR] **PM-m15** `post-lock.md` — only the QC line was added. Missing: an ADR-0003 input for the three new stored states (imported snapshot kind, never-supplied payload, samples not recorded), a Documentation item on how to export from the Toolkit, and a Dogfood item (PM7).
- [MINOR] **PM-m16** `capture-mode/prd-capture-mode.md:420` — Capture M8 measures "import commit → first row-success". A Toolkit-imported collection is captured at commit and may never hold a session; exclude such imports from M8's population.
- [MINOR] **PM-m17** `import/prd-inventory-import.md:5`, `:21` — Line 5 and the "Ready to capture" vocabulary say only new items change state. R6.8b also turns matched pending and set-aside items captured.
- [NIT] **PM-n1** `vision.md:140`, `:144` — J7's heading still says "v2 candidate", and "ships with CxF support (v2), not v1" still stands unstruck above the update.
- [NIT] **PM-n2** `import/prd-inventory-import-copy.md:29` — E43 shows two "Unchanged:" lines (rows and readings) in one preview. Label the second "Readings unchanged".

#### Biggest risks   (what builds the wrong thing or fails the user)
1. **Permanent history pollution on re-import (PM1).** Re-import is the realistic ongoing use, since Toolkit collections grow. Immutability means the duplicates can never be cleaned up, and the preview presents them as new history.
2. **The format rests on one export, and the repo can't hold a builder to it (PM7, PM8).** A real export that differs from the one studied — locale, device, app version, separator spacing — could fail wholesale (E47 → E42, or E5) while every synthetic case passes, and no metric or open question would notice.
3. **Silent, lossy fallbacks (PM5, PM6).** The Cataloger's reflex to open a CSV in a spreadsheet, or to "fix the file", turns a readings import into [the file's reading count] pending items plus about 50 permanent junk columns, with no explanation.
4. **User settings and trust changed silently (PM3, PM2, PM4).** A collection's scan mode flips without notice; Demo Device readings can outrank real ones; a new honesty mark has no explanation in the legend.

#### Genuinely solid   (incl. where simplicity is right that a product-zealot would over-spec)
- **The core decisions fit the job.** Readings arrive usable. Derived values are recomputed from the spectrum and checked against the file's own Lab, and a record that differs still imports rather than blocking the user. Provenance is honest: "not recorded" and "never supplied" are kept distinct from damage, and the snapshot is a third device kind that never occupies a device record.
- **The re-import precedence reads cleanly.** A SpectroCapture scan wins, and among imported readings the newest wins.
- **Extending the existing import flow (N4) was the right call.** §6 "states only where it differs", so the Cataloger meets the flow they already know. There is no new PRD, no per-import toggle, and no bulk "re-verify" workflow; declining one (N2) is appropriately lean for v1.
- **The mode-mismatch refusal is well built.** E46 names both modes, and "Choose another collection" keeps the file already read (R3.8p), so the refusal is not a dead end.
- **The mirrors are thorough and consistent:**
  - Device R1.21/R1.22 add the imported snapshot kind;
  - DF R2.3j, DJ6 and the R2.2 exception cover storage and history;
  - Collection Mode covers the chip, filter, VoiceOver, detail, history and no-spread display;
  - Export adds `sc_imported`, R1.1t, which counts an imported reading as neither unavailable nor damaged, and an E1 count.
- **Densities are kept as plain metadata, not a feature.** That matches the vision's v2 "density data" line.
- **The fixture discipline (N3, F71) protects the owner's private data**, and UJ 3's SQL-read cases are concrete and falsifiable.
- **No persona deck, funnel or OKR ceremony is warranted.** PM7 asks only for a small real-export corpus and one metric row, not instrumentation.

#### Missing / over-specified
**Missing:**
- a rule that de-duplicates readings already in history (PM1);
- how R6.8 treats a simulated current value (PM2);
- disclosure of the scan-mode change (PM3);
- E12's imported-mark meaning (PM4);
- a near-miss state for re-saved Toolkit exports (PM5);
- Toolkit-specific remedies, a blank-code fallback, and multi-collection file semantics (PM6);
- a format-variance open question, metric and dogfood corpus (PM7);
- an in-repo format appendix (PM8);
- illuminant, observer and Nix Device exclusions, plus a non-default-reference case (PM9);
- a help-doc line on getting an export out of the Toolkit, and ADR-0003 inputs (PM-m15).

**Over-specified (lightly):**
- R6.2's list of file columns "read for R6.6's check" when R6.6 uses only L, a, b; "ignored" is enough (PM-m2).
- The two "Unchanged" counts in E43 (PM-n2).
- Otherwise the amendment is proportionate: the budget raises (N12) avoided reopening aligned rows, and nothing here needs more ceremony.

| Row ID | disposition |
| :--- | :--- |
| Import-R1.5 | OBJECT (PM-m4) |
| Import-R3.3 | OBJECT (PM1) |
| Import-R3.8 | OBJECT (PM-m3, PM6) |
| Import-R6.1 | OBJECT (PM5, PM8) |
| Import-R6.2 | OBJECT (PM6, PM-m1, PM-m2) |
| Import-R6.3 | OBJECT (PM6) |
| Import-R6.4 | OBJECT (PM3, PM-m6, PM-m7, PM-m13) |
| Import-R6.5 | OBJECT (PM9) |
| Import-R6.6 | OBJECT (PM9, PM-m2) |
| Import-R6.7 | OBJECT (PM8, PM9) |
| Import-R6.8 | OBJECT (PM1, PM2, PM-m5) |
| Import-R6.9 | OBJECT (PM1, PM3, PM-n2) |
| Import-R6.10 | OBJECT (PM7, PM8) |
| DF-R1.6 | ALIGN |
| DF-R2.2 | ALIGN |
| DF-R2.3 (with R2.3j) | OBJECT (PM1, PM2, PM-m10) |
| Device-R1.21 | ALIGN |
| Device-R1.22 | ALIGN |
| CM-R2.4 (with R2.4j) | OBJECT (PM4) |
| CM-R2.7 | ALIGN |
| CM-R4.2 (with R4.2d) | OBJECT (PM-m11) |
| CM-R5.2 (with R5.2b/e) | ALIGN |
| CM-R8.9 | ALIGN |
| Export-R1.1 (with R1.1p/t) | ALIGN |
| Export-R1.2 | ALIGN |
| Export-E1 | ALIGN |
| Capture-R1.10 | ALIGN |

### staff-software-engineer

#### Verdict
Proceed after addressing Blockers. The design is sound: the snapshot kind is the single source of truth, every stored value is recomputed from the spectrum, and a mode that doesn't match is refused rather than mixed. The problems are at the edges. Re-import is not idempotent once a reading sits in history. The Toolkit number format is not written down anywhere in the repo. The fixture described as "synthetic" carries real export values. Eight Majors (row-level edits and missing cases) should land in the same pass.

#### What I reviewed
- **Type:** requirements-mode amendment, subject commit 417fdac on branch docs/nix-toolkit-import. Base is 6374538. I read `git diff 6374538 417fdac -- docs` in full.
- **Files.** All live under `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/`. Abbreviations used below:
  - `IMP` = `docs/product/import/prd-inventory-import.md`
  - `IMP-J` = `…/import/prd-inventory-import-journeys.md`
  - `IMP-C` = `…/import/prd-inventory-import-copy.md`
  - `IMP-F` = `…/import/prd-inventory-import-fences.md`
  - `DF` = `docs/product/data-foundation/prd-data-foundation.md`
  - `DF-J` = `…/data-foundation/prd-data-foundation-journeys.md`
  - `DF-F` = `…/data-foundation/prd-data-foundation-fences.md`
  - `CM` = `docs/product/collection-mode/prd-collection-mode.md`
  - `CM-C` = `…/collection-mode/prd-collection-mode-copy.md`
  - `EXP` = `docs/product/export/prd-data-export.md`
  - `CAP` = `docs/product/capture-mode/prd-capture-mode.md`
  - `DEV` = `docs/product/device-management/prd-device-management.md`
  - `LOG` = `docs/agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md`
- **Upstream:** owner decisions N1–N15 (LOG Round 0), and the format report at `/private/tmp/claude-501/-Users-vinnypasceri-Projects-spectro-capture/d4b2a2e5-4696-48c6-9516-d4cbad888739/scratchpad/nix-toolkit/report.md`.
- **Also read:**
  - every sibling fence file
  - the vision, post-lock and the PRD README
  - `docs/decisions/README.md`
  - the unchanged sibling rows the amendment leans on: DF R2.1/R2.4/R7.1/R7.2/R7.7, CM R5.3, CAP R1.5/R5.6/R8.18/OQ 1/OQ 21, EXP R2.3/R4.2.
- **"Codebase":** there is no code yet (AGENTS.md §7), so "fits the system" means fitting the locked sibling PRDs and the ADR queue.
- **Format facts checked, values never printed:** I ran count-only pattern checks against the local export the report names (not in the repo). Confirmed:
  - bare `;` separators with no padding
  - an unquoted header
  - LF endings only
  - every Lab/LCh/XYZ and reflectance cell in the form `-?\d\.\d{8}e[+-]\d` (one exponent digit)
  - every Date Saved in the form `YYYY-MM-DDTHH:MM:SS.sssZ`
- **Could not verify:**
  - the rule-14 word counts (the method can't be reproduced here)
  - how the Toolkit escapes an embedded `"`, `;` or newline
  - whether other Toolkit settings (illuminant/observer, other devices, app versions) change the column set or the number format
  - the Spectro 2 SDK's wavelength grid (Export OQ 4)
  - whether real values leaked anywhere beyond the strings I tested

#### Findings

**[BLOCKER] B1 — Import R6.8c/R6.8d, DF R2.3j, R6.9/R3.3 — re-import creates duplicate readings in permanent history.**
- **Where:** `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import.md:180-181`, and `…/docs/product/data-foundation/prd-data-foundation.md:137`.
- **The gap.** Only R6.8d has a "same reading changes nothing" rule, and it compares only against the *current* value. R6.8c ("the reading is kept in version history") and R6.8d's "an earlier one goes to history" have no such check. DF R2.3j goes further: it *creates* B in every case ("Create B … else kept behind A").
- **Scenario 1.** A Cataloger imports a Toolkit export, re-scans some items in SpectroCapture, then re-imports an updated export to pick up new colours. Each re-scanned item gets another copy of its Toolkit reading in history, on every re-import.
- **Scenario 2.** The same happens after a newer export followed by the older one again.
- **Scenario 3.** An imported current reading is Flagged from the collection (CAP R5.6), which moves it to history. A re-import then hits R6.8b ("no current value") and resurrects the flagged reading as current.
- **Why it can't be cleaned up.** History is never deleted except by deleting the item (DF R2.3), so the duplicates stay forever. They inflate reading counts (DF R6.2), E17 and export history.
- **Contradiction.** R6.9 and R3.3 claim "idempotent, readings included", so an engineer has to pick one side.
- **Mutation that passes today:** implement R6.8c literally, appending on every import. Every UJ 3 case still passes, because the only re-import case (IMP-J:119) covers items whose current reading is imported.
- **Fix:**
  - Add a lead rule to R6.8: "A record whose reading equals (same Date Saved and reflectances, R6.8d) a reading the item already holds, current or in history, changes nothing and counts as unchanged. R6.8b–d apply only otherwise."
  - Mirror it into DF R2.3j.
  - Add three UJ 3 cases: re-import after IMP-J:118's state; re-import after Flagging an imported current reading; re-import of the original fixture after IMP-J:120.

**[BLOCKER] B2 — Import R6.10 / UJ 3 fixture — the only in-repo format contract omits the export's number format.**
- **Where:** `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import.md:172` and `…/docs/product/import/prd-inventory-import-journeys.md:104`.
- **R6.10 can't be built as written.** It (and F71) says to build fixtures "to the format the research records". That research is not in the repo, and no brief in `docs/briefs` records the Toolkit format.
- **What the fixture text leaves out.** UJ 3's fixture is therefore the only format spec. Every number cell in the real export is scientific notation with a one-digit exponent (e.g. the form `d.dddddddde+d`). The fixture never says so, and its only literal reflectance, `1.047` (IMP-J:104, :111), is a plain decimal.
- **Mutation that passes today:** a reflectance/Lab parser that accepts only `-?\d+(\.\d+)?` passes every UJ 3 case. On a real export every record then fails R6.7 and the import ends at E42, with nothing imported.
- **The prose is misleading.** It lists the header as "Index; Custom Collection Name; …", with padding. The real format has bare `;`. A padded data field ` "x"` would be invalid quoting under R1.5c, so the whole file would go to E5.
- **Fix:**
  - Point R6.10 at "UJ 3's fixture format".
  - Write the format facts into UJ 3's fixture paragraph:
    - Lab/LCh/XYZ/reflectance cells in the one-integer-digit, eight-fraction-digit, one-exponent-digit scientific form
    - XYZ scaled 0–1
    - sRGB as integers
    - HEX as `#` plus six lowercase hex digits
    - Density as two-place decimals, possibly negative
    - bare `;`, unquoted header, the four quoted string columns, Date Saved as `YYYY-MM-DDTHH:MM:SS.sssZ`
  - Write TK-3's out-of-range value in that scientific form.

**[BLOCKER] B3 — UJ 3 Fixture T, DF DJ6, review log — the "synthetic" fixture carries real export values (contradicts F71/N3).**
- **Where:** `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import-journeys.md:104,111`; `…/docs/product/data-foundation/prd-data-foundation-journeys.md:113-115`; `…/docs/agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md:18,32`.
- **What I found.** Count-only checks against the owner's local export show:
  - each of Fixture T's three millisecond-precision Date Saved strings appears once in the real export
  - one of Fixture T's three colour names also appears in it
  - the review log's N5 and N15 quote the real collection's row count and its name
- **Why it matters.** F71 says the tests are synthetic. The owner explicitly rejected even "Commit with dates shifted" (N3).
- **Timing.** The branch is 1 commit ahead of origin/main with no remote branch, so it is not pushed yet. Once it is pushed to this public repo, removing the values means rewriting history.
- **The check that fails today:** a count-only `grep -F` of the fixture's three Date Saved strings against the export returns 1 each. It must return 0.
- **Fix:**
  - Replace the three timestamps (in IMP-J and DF-J) and the real colour name with invented values.
  - Replace the review log's quoted name and count with placeholders.
  - Do this before the first push.

**[MAJOR] M1 — Import R6.9 / UJ 3 — the E43 count oracles contradict each other, so a correct build fails one case.**
- IMP-J:110 counts three new items (R6.8a) as "3 becoming current".
- IMP-J:118 counts TK-3, also new under R6.8a, as "1 new" and reports "1 becoming current (TK-2)".
- **Fix:** state in R6.9 whether R6.8a readings count as "becoming current". I recommend yes, and then change IMP-J:118 to "2 becoming current (TK-2, TK-3), 1 new".

**[MAJOR] M2 — Import R6.8d (IMP:181), DF R2.3j (DF:137) — the equality and ordering rules are incomplete.**
- **Undefined case.** Equal Date Saved with different reflectances matches none of R6.8d's three branches (same / later / earlier). A plausible trigger: a future Toolkit version writes fewer digits.
- **Equality rule unstated.** "Every reflectance equal" doesn't say whether values are compared as text (`1.047` vs `1.04700000e+0`) or as numbers, nor whether Date Saved is compared as an instant or a string.
- **"Older" is ambiguous.** DF R2.3j's "over an older imported A" could mean older measurement time or older record time. Imported A's record time is always the older one, so reading it as record time makes every re-import current and contradicts R6.8d.
- **Fix:**
  - Define "same reading" as the same Date Saved instant (to the millisecond) and every reflectance equal as a parsed finite number.
  - State the equal-date, different-spectrum outcome. DF R2.3j implicitly says history; make it explicit and list it in E43.
  - Write "older by measurement time (Date Saved)" in R2.3j.

**[MAJOR] M3 — Import R6.7/R6.6/R6.5 (IMP:167-169) — the grammar for "a number" and "a time" is left to the builder.**
- **Scenario 1.** The Rust core's `f64` parse (ADR-0001) accepts `NaN` and `inf`. SQLite stores a NaN REAL as NULL, so a `NaN` reflectance would become a hole in the stored spectrum rather than an E47 exclusion.
- **Scenario 2.** A locale-aware parser accepts a comma decimal or rejects exponents.
- **Scenario 3.** Negative reflectances are neither allowed nor refused. The export's own Density cells go negative, so noise below 0 is plausible.
- **Scenario 4.** A Date Saved with no `Z` or offset parses in some builds and not others.
- **Scenario 5.** If the file's own L, a or b can't be read, R6.6 has nothing to compare against, and R6.7 doesn't cover those columns.
- **Fix:** add R6.7a:
  - A number is an optional sign, digits, an optional `.` fraction and an optional `e`/`E` exponent of one or more digits.
  - Only finite values count; `NaN`, `Infinity`, empty, `undefined` and comma-decimal are unreadable.
  - The owner decides whether negative reflectances are readable.
  - A time is ISO 8601 with `Z` or ±hh:mm, fractional seconds optional, stored to the millisecond.
  - The owner decides what an unreadable file L/a/b does: exclude via E47 naming the column, or import unchecked and list it.
  - Add UJ 3 cases for `NaN`, a plain decimal, and a time with no zone.

**[MAJOR] M4 — Import R6.4/R6.6/R6.7, Capture R1.10 — the valid values for Measurement Mode, Illuminant and Observer aren't stated, and the order of the checks is undefined.**
- **Illuminant/Observer.** R6.6 must work out Lab "under the file's own illuminant and observer". R6.7 never validates those two columns. The app's interim reference set is D50/2° only (CAP OQ 21, CAP:446), and CAP R1.5 says the list is one pair. A Toolkit export saved at D65/10° therefore has no defined outcome: compute anyway, skip the check, exclude, or refuse.
- **Mode.** "A mode" is undefined, while CAP R1.10 (CAP:147) allows only M0/M1/M2. An all-`M3` file would set a scan mode Capture forbids.
- **Order.** A record with an empty mode among M2 records could be excluded by R6.7 (E47) or refuse the whole file under R6.4 (E46 mixed).
- **Fix:**
  - R6.7 names the readable sets: modes exactly M0, M1, M2; illuminant/observer from the app's reference set under CAP OQ 21, or an owner-chosen interim for out-of-set pairs.
  - State that R6.7's exclusions run before R6.4's sameness check.
  - Add UJ 3 cases for D65/10 and for an empty mode on one record.

**[MAJOR] M5 — Import R6.4 / R3.8j, Capture R1.10 — when an existing empty target adopts the file's mode is unstated.**
- **Where:** "hold no reading and then adopt it" (IMP:166).
- **The gap.** It doesn't say whether the mode changes at target selection or inside the commit.
- **Scenario.** A user picks an empty M1 target, previews, then presses Cancel (or the commit fails with E44). If the switch happened at selection, their collection is silently left at M2. That contradicts R3.8j's "exit without imported changes" and R3.2's whole-or-nothing commit.
- **Related gap.** It is also unstated whether the Capture §1 creation form for a Toolkit-created target lets the user change the scan mode; CAP R1.10 says it arrives pre-filled at M1.
- **Fix:**
  - R6.4: "adopts it inside the commit (R3.2); Cancel, E44 or a failed commit leaves its mode unchanged."
  - CAP R1.10: the creation form shows the file's mode, fixed.
  - Add a UJ 3 case: empty M1 target, preview, Cancel, mode still M1.

**[MAJOR] M6 — DF F64 / DJ6 / N10 vs DF R2.1 and CM R5.3, and Import R6.8c — history ordering contradicts settled rows.**
- **The contradiction.** DF-F:744 and DF-J:114 say "history and over-time views place it by its measurement time", matching owner decision N10: "History sorts it by when it was actually measured". But DF R2.1 (DF:112, Aligned, unamended) says "history orders by record time", and CM R5.3 (CM:385) lists E17 newest-recorded first, with measured order as a separate variant.
- **The oracle can't tell them apart.** DJ6's case passes under either reading, because the imported B is both newest-recorded and earliest-measured.
- **They diverge once a later scan C lands.** Recorded order is C, B, A; "history by measurement time" gives C, A, B.
- **A wrong label.** R6.8c/F74 call the kept reading "an earlier reading", but its Date Saved can be later than the scan's.
- **Fix:**
  - Say that N10's "history" means CM R5.3's measured order and that recorded order stays by record time. Otherwise amend DF R2.1 and CM R5.3 by owner decision.
  - Say "kept in history, not current" in R6.8c.
  - Make DJ6:114 discriminating by adding a later scan C.

**[MAJOR] M7 — DF Vocabulary "Reading" (DF:37), R2.1 (DF:112), R1.2/R7.1, ADR-0003's queue entry — the definitions still say a reading is a mean of 1–5 samples.**
- **The conflict.** The vocabulary says a reading is "one saved set of 1–5 samples", and R2.1 makes the canonical value "the stored mean with … averaging basis". AGENTS.md §8 and ADR-0003's queue title ("the stored mean canonical") say the same. R2.3j creates a zero-sample reading with no mean and no basis, and none of those definitions is amended.
- **Scenario.** A builder enforces the vocabulary as a constraint (1–5 samples per reading, value = mean of samples) and every Toolkit reading is rejected.
- **The new states aren't listed.** The imported-kind snapshot, samples-not-recorded and never-supplied payload are missing from R1.2's and R7.1's lists of SQL-readable states. DJ6:117 asserts them anyway.
- **Why now.** This is the schema's one-way door: the schema ships inside users' own files.
- **Fix:**
  - Amend "Reading" and R2.1 with "…or an imported Toolkit reading: the file's spectrum as given, no samples, averaging basis not recorded (R2.3j)".
  - Add the three states to R1.2 and R7.1.
  - Add post-lock items for AGENTS.md §8 and ADR-0003's scope.

**[MAJOR] M8 — DF R7.7 fixture matrix (DF:248), R7.2 (DF:240), Export R4.2 (EXP:145) — no fixture holds an imported reading.**
- **The gap.** R7.7 is the "sole R7.7/Data Export inventory" and has no row with an imported reading. Export R4.2 enumerates only R7.7a–n and asserts "all three `sc_simulated` values", not `sc_imported`. R7.2 exercises R2.3a–i, not R2.3j.
- **Consequence.** F35's claim that `sc_imported` "lands in v1's golden" has no fixture to land on. The golden never shows `sc_imported` true, and EJ1's new row and CM UJ2.1-r have no inventory entry to draw from.
- **Fix:**
  - Add R7.7o: an imported reading that is current; one kept behind a scanned current; one superseded by a re-scan. Consumers: DJ6, Export R1.1t/R1.2/R4.2, CM UJ2.1-r.
  - Extend R7.2 to R2.3a–j.
  - Export R4.2: "all three `sc_simulated` and `sc_imported` values".

**Minor**
- **[MINOR] m1 — R6.2/R6.3 (IMP:164-165):** Custom Collection Name isn't in R6.2's list, so R2.2's default makes it a metadata column. R6.3's "an existing target ignores that column" then reads as "not stored". State its fate for both new and existing targets. Also say whether "the same one" means R2.3 equality or exact text.
- **[MINOR] m2 — R6.1 + R3.8n:** E43 still offers "Choose an encoding / Choose a separator" on a detected Toolkit export. R6.1 forces UTF-8 with `;` and runs detection regardless of the active delimiter, so the controls either do nothing or silently drop Toolkit handling. State whether they are offered, and what a change does.
- **[MINOR] m3 — R6.6 (IMP:168):** DERIVATION_TOLERANCE links to DF's Legend, which doesn't define it. Name DF OQ 6, its candidate ΔE2000 0.1 and its release gate, and add DF OQ 6 to Import's Build dependencies (IMP:30).
- **[MINOR] m4 — Vocabulary:** existing rows use "toolkit" for the vendor SDK (CAP R1.5/OQ 21, DF R3.1, Device obligations); the amendment uses "Toolkit" for the mobile app. Add an Import vocabulary entry to separate them. Also, "Ready to capture" (IMP:21) still says "new rows pending".
- **[MINOR] m5 — DF R2.4 (DF:116):** R2.4 says "the app asks rather than guessing", but R2.3j assigns initial or re-measurement without asking. Add a carve-out naming R2.3j.
- **[MINOR] m6 — CM R4.2d/R5.2e vs copy:**
  - R4.2d (CM:348) shows basis, spread and verdict as unknown / not recorded; the copy line CM-C:295 omits all three.
  - R5.2e says "neither was recorded"; its copy shows only "Samples not recorded".
  - Align each rule with its copy.
- **[MINOR] m7 — E43 Toolkit variant (IMP-C:29):** the new placeholders (⟨readings⟩, ⟨mode⟩, ⟨current⟩, ⟨history⟩, ⟨unchanged readings⟩, ⟨flagged⟩) are missing from the placeholder paragraph at IMP-C:35. R6.9 never defines the ⟨readings⟩ total: all records, eligible records, or the sum of the three counts?
- **[MINOR] m8 — Wavelength grid:** the fixed 400–700 nm / 10 nm set is assumed to equal WAVELENGTH_GRID, which Export OQ 4 leaves open until the hardware spike. If they differ, Export R2.3's "unreachable" out-of-grid branch becomes reachable and imported spectra export as empty cells. Extra bands (e.g. R380) silently become metadata under R2.2. State the assumption and the fallback.
- **[MINOR] m9 — R6.8b/c edge cases:**
  - A Demo Device (simulated) current value fits none of R6.8b–d, since "scanned in SpectroCapture" is unclear for simulated readings.
  - An item set aside by a Flag, or left set aside with a note, becomes captured through R6.8b, overriding the operator's decision.
  - A current value that is a restore of an imported reading has no stated row.
  - Key R6.8c/d on the current reading's snapshot kind, and get the owner's answers (questions below).
- **[MINOR] m10 — R6.5 "its illuminant, observer":** on a spectral reading this should be provenance only (the reference the file's own values used), not a fixed reference that could trigger DF R3.5/R3.3e mismatch logic. Say so. Also state whether a Toolkit-created collection takes the file's illuminant/observer or CAP R1.5's D50/2° pre-fill.
- **[MINOR] m11 — Performance budget:** a Toolkit import adds a spectral derivation per record at preview and a reading plus six derived sets per record at commit. State that Capture OQ 13's IMPORT_BUDGET covers this at ROWS_TARGET, and include a ROWS_TARGET Toolkit file in the dogfood budget run.
- **[MINOR] m12 — R6.2:** Index, LCh, XYZ, sRGB and HEX are "read for R6.6's check", but R6.6 uses only L, a and b. Say "ignored", so a malformed HEX raises no question.
- **[MINOR] m13 — Quoting and placeholders:** it is unverified how the Toolkit writes a name containing `"`. Under R1.5c a single such name sends the whole file to E5. Add a question and a pinned UJ 3 case. Also ask whether a literal `undefined` in Color Name, Color Code or Custom Collection Name gets the Note's treatment.

**Nit**
- **[NIT] n1:** Import's outbound Data Export obligation line (IMP:148) doesn't mention `sc_imported`; Export's inbound line does.
- **[NIT] n2:** the E43 Toolkit variant has two "Unchanged:" labels, one for rows and one for readings.
- **[NIT] n3:** DF R2.3's lead sentence "A later reading supersedes it" now has an R2.3j exception; add "save R2.3j".
- **[NIT] n4:** fold "bare `;`, no padding" into B2's fixture paragraph.

#### Clarifying questions for the author
1. Does a record whose reading equals any reading the item already holds, current or in history, change nothing?
2. After an imported current reading is Flagged, should a re-import leave the Flag standing?
3. With an equal Date Saved and different reflectances, does the incoming reading go to history, become current, or get flagged?
4. Are Date Saved and reflectances compared as parsed instants and numbers, not as text?
5. Do readings on new items (R6.8a) count as "becoming current" in E43?
6. Which number forms are readable: the exponent form, plain decimals, negatives? And are `NaN`/`Infinity` unreadable?
7. Is a Date Saved with no `Z`/offset, or with no milliseconds, a readable time?
8. Are the readable modes exactly M0/M1/M2, and do R6.7's exclusions run before R6.4's sameness check?
9. What happens to a record whose illuminant/observer is outside the app's reference set (interim D50/2°): excluded, imported with the check skipped, or refused?
10. If the file's L, a or b can't be read, is the record excluded, or imported unchecked and listed?
11. Does an existing empty target adopt the mode only inside the commit, so Cancel leaves it unchanged?
12. Is the scan mode on the creation form fixed to the file's mode for a Toolkit-created target?
13. Does N10's "history sorts by when measured" mean CM R5.3's measured order only, with recorded order staying by record time?
14. Is Custom Collection Name stored as a metadata column, and does the answer differ for new and existing targets?
15. On a detected Toolkit export, are "Choose an encoding"/"Choose a separator" offered, and does using them drop Toolkit handling?
16. Should a Demo Device (simulated) current value outrank a real Toolkit reading?
17. Should R6.8b promote a Toolkit reading on an item set aside by a Flag, or on one deliberately left set aside?
18. Is the Toolkit's 400–700/10 nm grid assumed equal to WAVELENGTH_GRID, and what happens if the spike finds otherwise?
19. Does a Toolkit-created collection take the file's illuminant/observer as its display default?
20. Two records with the same Color Code in one export: exclude both (R2.4), or let the newest Date Saved win?

#### Claimed properties
- **"Repeating a committed export … changes nothing"** (R6.9; R3.3's "idempotent, readings included"): **does not hold.** It holds only when every matched item's current reading is imported. It fails for items under R6.8c and for readings already in history (B1).
- **"Fixture T is synthetic, never a real user's export"** (UJ 3, DJ6, F71): **does not hold.** The three timestamps and one colour name are real (B3).
- **"Semicolons, the duplicate L header and 'undefined' notes handled automatically"** (N5/F73): **holds** for recognition, the second L (no E41) and Note. The number format isn't recorded anywhere in the repo, so real files can fail (B2).
- **"`sc_imported` lands in v1's golden before any export format version ships, so R2.5 is not engaged"** (EXP F35): **the versioning half holds**, since nothing has shipped. **The golden half does not**, because no fixture holds an imported reading (M8).
- **"An imported snapshot is never a device record and occupies none"** (DEV R1.22): **holds.** Nothing I read conflicts with it.
- **"No aligned row reopens for compaction"** (N12/F80): **unverified.** Rule 14's counting method isn't reproducible here; `wc -w` includes markup and isn't comparable.
- **"Every derived value worked out from the spectrum lands within DERIVATION_TOLERANCE"**: **holds on the one researched export** (well under 0.1 ΔE2000). **Unverified** for other illuminant/observer settings (M4).

#### Genuinely sound
- **One source of truth for provenance.** The imported mark (CM R2.4j), `sc_imported` (EXP R1.2) and DEV R1.21 all key on the snapshot kind. A restore carries it correctly, because DF R2.3f copies H's snapshot.
- **Spectrum first.** All six spaces are recomputed from the spectrum and the file's own derived values are never stored, so there is one derivation and no second truth. The Lab check is a cheap, honest cross-check.
- **Conservative recognition.** The header-signature subset match under R2.3 falls through to §1–§3 unchanged. I confirmed that the real format (bare `;`, unquoted header, LF only) satisfies R6.1's split and R1.5c's quoting, so no special-case parser is needed.
- **Measurement mode is match-or-refuse.** It fits DF R3.3c/d, where another condition's value is never substituted, and adoption only when the target holds no reading prevents mixed-mode collections.
- **Reflectances above 1.0 are kept as given**, and "no payload, never supplied" is not dressed up as a payload. This fits DF's existing rule that "an archive the instrument never supplied is not corruption".
- **Right-sized scope.** Adding §6 to the existing import flow, rather than writing a new PRD, is correct. No separate operating envelope is needed: derivation cost is trivial and Capture OQ 13 already owns import budgets (m11 only asks to say so). R6.8's one-row-per-state table is the right shape; the gaps are its missing edge cases, not its structure.

#### Deferred
- **peer-privacy-reviewer:** B3 in depth — sweep every amendment line for other real values, check the review log's verbatim quotes, and consider a guard against fixture leakage before push.
- **peer-product-manager-reviewer:** the WHAT behind m9 and questions 16, 17 and 20 (Demo vs real readings, overriding a Flag, duplicate codes). Also:
  - Capture only starts a session over pending or set-aside rows, so a Toolkit-created collection can't be re-verified in bulk.
  - "Scanned" counts (CM R2.2) now include imported items.
  - There is no success metric for Toolkit imports; Import M1's corpus is Numbers/Excel/Sheets only.
  - A near-miss export (one header renamed) silently becomes a plain CSV with no readings.
  - What happens to D65/10 exports (the product half of M4).
- **peer-product-marketing-manager-reviewer:** E43's wording and the double "Unchanged"; the imported mark's label; the missing copy for naming a Toolkit export on the target and mapping steps.
- **peer-architecture-reviewer (ADR-0003):** a reading with no samples, a per-reading "never supplied" state, and the key used to spot a duplicate reading (M7, B1) — schema one-way doors.
- **peer-database-reviewer:** how reflectances and Date Saved are stored so the numeric-equality rule is exact (M2): REAL vs TEXT for "as given", and NaN becoming NULL.

| Row ID | disposition |
| :--- | :--- |
| Import-R1.5 | ALIGN |
| Import-R3.3 | OBJECT (B1) |
| Import-R3.8 | OBJECT (M5, m2) |
| Import-R6.1 | OBJECT (m2, m13) |
| Import-R6.2 | OBJECT (m1, m12) |
| Import-R6.3 | OBJECT (m1) |
| Import-R6.4 | OBJECT (M4, M5) |
| Import-R6.5 | OBJECT (M3, M7, m10) |
| Import-R6.6 | OBJECT (M3, M4, m3) |
| Import-R6.7 | OBJECT (B2, M3, M4) |
| Import-R6.8 | OBJECT (B1, M2, M6, m9) |
| Import-R6.9 | OBJECT (B1, M1, m7) |
| Import-R6.10 | OBJECT (B2, B3) |
| DF-R1.6 | ALIGN |
| DF-R2.2 | ALIGN |
| DF-R2.3 (with R2.3j) | OBJECT (B1, M2, M6, M7, M8, m5) |
| Device-R1.21 | ALIGN |
| Device-R1.22 | ALIGN |
| CM-R2.4 (with R2.4j) | ALIGN |
| CM-R2.7 | ALIGN |
| CM-R4.2 (with R4.2d) | OBJECT (m6) |
| CM-R5.2 (with R5.2b/e) | OBJECT (m6) |
| CM-R8.9 | ALIGN |
| Export-R1.1 (with R1.1p/t) | OBJECT (m8) |
| Export-R1.2 | OBJECT (M8) |
| Export-E1 | ALIGN |
| Capture-R1.10 | OBJECT (M4, M5) |

### test

#### Verdict
Tests don't prove the behavior. As written, UJ 3 cannot go green on any build (B1). Its fixture can go green on a build that misreads every real export (B3). The owner's re-import rule is contradicted and untested on scanned items (B2). Several R6 branches, including N6's discriminating case, have no case that fails a wrong build.

#### Coverage map (brief)
Paths used below (all under `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/`): IMP=`product/import/prd-inventory-import.md`, IJ=`product/import/prd-inventory-import-journeys.md`, IC=`product/import/prd-inventory-import-copy.md`, IF=`product/import/prd-inventory-import-fences.md`, DF=`product/data-foundation/prd-data-foundation.md`, DJ=`product/data-foundation/prd-data-foundation-journeys.md`, DFF=`product/data-foundation/prd-data-foundation-fences.md`, CM=`product/collection-mode/prd-collection-mode.md`, CMJ=`product/collection-mode/prd-collection-mode-journeys.md`, CMC=`product/collection-mode/prd-collection-mode-copy.md`, EX=`product/export/prd-data-export.md`, EXJ=`product/export/prd-data-export-journeys.md`, CAP=`product/capture-mode/prd-capture-mode.md`. There is no code or suite yet. This review judges the specified oracles; every "mutation" below is a build variant run against those oracles.

What is covered: R6.1 positive detection; R6.2 fixed mapping, dropped columns and `undefined` Note; R6.3 uniform/non-uniform pre-fill; R6.4 new, mixed, mismatch and adopt; R6.5 provenance fields; R6.6 over-tolerance; R6.7 bad reflectance only; R6.8a; R6.8b (pending); R6.8c (scan older than Toolkit only); R6.8d same, later and earlier; repeat import (R6.8a state only); DJ6 current and behind-scan; UJ3-c; UJ2.1-r; EJ1.

Load-bearing gaps: idempotency on R6.8c state; a Toolkit reading newer than a scan; R6.8d same-date/different-spectrum; the gamut flag on imported readings; R6.7 Date Saved/Mode/Illuminant/Observer; R6.1 negatives; `sc_imported` in the golden inventory.

#### Findings
[BLOCKER] IJ:110 vs IJ:118 (E43 copy IC:29, placeholder note IC:35) — the two cases cannot both pass. In row 110 all three records are new (R6.8a) and it asserts "3 becoming current". In row 118 TK-3 is also new (R6.8a) but the row asserts "1 becoming current (TK-2) … 1 new (TK-3)". A build that counts new items' readings in ⟨current⟩ fails row 118; a build that leaves them out fails row 110. Fix: define ⟨current⟩, ⟨history⟩, ⟨unchanged readings⟩ and ⟨readings⟩ in IC:35 (e.g. "readings becoming current, new items included"). Then make row 118 read "2 becoming current (TK-2, TK-3), 1 kept in history (TK-1), 1 new (TK-3)".

[BLOCKER] IMP:180 (R6.8c), IMP:181 (R6.8d "an earlier one goes to history") vs IMP:171 (R6.9), IMP:78 (R3.3), IF:233 (F82 "Re-importing an identical Toolkit reading changes nothing") — re-importing onto scanned items is uncovered and contradicts its fence. As written, R6.8c appends the Toolkit reading to history on every import. The rule "identical reading changes nothing" exists only for an imported current value (R6.8d). The repeat case IJ:119 runs only on the all-R6.8a state. Scenario: the Cataloger re-exports from the Toolkit after scanning some items in SpectroCapture; each scanned item gains another identical imported reading in history, which can never be deleted (DF R2.3). The same happens when an older export is re-imported after a newer one. Mutation that passes every UJ 3 and DJ6 row: follow R6.8c literally and append the reading on every import. Fix: add to R6.8c and R6.8d "a reading equal (Date Saved and every reflectance) to one the item already holds, current or in history, changes nothing and counts as unchanged". Add a case: from row 118's end state, import Fixture T again → E43 shows 3 unchanged readings and TK-1's reading count is unchanged.

[BLOCKER] IJ:104 (Fixture T), IMP:172 (R6.10 "built to the format the research records") — the synthetic fixture is not pinned to the real format, so it can go false-green. The research that R6.10 cites is outside the repo, so IJ:104 is the only format spec a builder has. It pins no number form, and its one literal, `1.047`, is plain decimal. The researched export writes every L/a/b/L/c/h/X/Y/Z and R-cell as 9-significant-digit exponent notation with a one-digit signed exponent (`d.dddddddde±d`); I confirmed this format fact, and the next one, with a counts-only probe. The fixture also renders the header as "Index; Custom Collection Name; …". The real file uses a bare `;`, and under R1.5c a space before an opening quote is E5. Mutation that passes every UJ 3 row: a numeric reader that rejects exponent notation (a typed CSV deserializer or locale formatter; Rust's `f64::from_str` itself would accept it). On a real export that reader sends every record to E47, then E42. Fix: pin in IJ:104 exponent notation for those columns, XYZ scaled 0–1, sRGB as integers, HEX as `#` plus six lowercase digits, Densities as 2-place decimals that may be negative, and no space after `;`. Write TK-3's value as `1.04700000e+0`. Make R6.10 cite IJ:104, not the research.

[MAJOR] DJ:114 and DFF:741 (F64 "history and over-time views place by its measurement time") vs DF:112 (R2.1 "history orders by record time", Aligned, unchanged), DF:115 (R2.3 "A later reading supersedes it", status flipped but text unchanged), CM:385 (R5.3 "newest-recorded first"), EX:80 (R1.1g "history by per-item sequence ascending") — a test writer cannot write one SQL history-order oracle. For B kept behind a scan, record time and sequence put it after A. DJ6 says history puts it before A "by its measurement time". The per-item sequence of a reading kept behind another is undefined. Fix: amend R2.1 and R2.3 to name this exception and define B's sequence, and mirror that into CM R5.3 and EX R1.1g. Otherwise restate DJ6 row 2 so that history follows record time and only over-time views follow measurement time.

[MAJOR] IJ:118 (no scan date given), DJ:114 (scan 2026-09-20, which is newer than TK-1) — N6/F74's rule that the scan stays current is never tested when the Toolkit reading is newer. Mutation that passes: "newest measurement wins", the option N6 rejected. Fix: add a case where the scan was measured 2026-06-01 and TK-1's Date Saved is [its former Date Saved, a real date since replaced]. The scan stays current, TK-1 is kept, and the over-time view places TK-1 after the scan.

[MAJOR] IMP:181 (R6.8d), DF:137 (R2.3j reason column) — two branches have no rule. (a) An imported current value and an incoming record with the same Date Saved but a different reflectance is neither "same", "later" nor "earlier". (b) The supersession reason for an earlier imported reading that goes to history behind a newer imported current value is undefined; "re-measurement over an imported A" can be read either way, and no DJ6 row covers it. Fix: state both rules, and add a UJ 3 case and a DJ6 row.

[MAJOR] IMP:167 (R6.5 "the gamut-clipped flag included"), IJ:104 and IJ:111, DJ:113, and the DF:27 index claiming R3.4 — the flag on imported readings has no oracle. Fixture T declares no out-of-gamut record, and no row asserts the flag's value. The research found gamut clipping common in a real Toolkit export. Mutation that passes: never compute the flag for imported readings. Fix: declare one Fixture T record whose spectrum falls outside sRGB and one inside. Assert the flag by SQL, CM's outside-sRGB mark, and EX's `sc_sRGB_gamut_clipped`.

[MAJOR] IMP:169 (R6.7), IJ:114 — R6.7 is only one-third tested and has holes.
- Only the bad-reflectance branch has a case.
- Mutation that passes: store an unparseable Date Saved as empty or as commit time. That silently misorders R6.8d and the over-time views.
- No rule covers an unreadable or absent Illuminant, Observer, Nix Device, or the file's own L/a/b. R6.5 and R6.6 depend on all of them.
- No rule covers a readable illuminant the app cannot derive under.
- The order is undefined when a record is both unreadable (E47) and in another mode (E46 mixed).

Fix:
- Add cases: Date Saved `not-a-date` → E47 naming `Date Saved`; Measurement Mode blank → E47.
- Extend R6.7 or R6.6 to those columns.
- State that E47 exclusion runs before the mixed-mode check.

[MAJOR] EXJ:3 ("DF R7.7a–n is the sole fixture inventory"), EXJ:14, DF:250–267, EX:145 (R4.2 lists "all three `sc_simulated` values" only) — `sc_imported` never reaches the golden, which is the release contract (R4.1). The new EJ1 row has no fixture in the inventory that its own rule says is the only one. Mutation that passes every R4.2-enumerated golden: always emit `sc_imported` false. Fix: add DF R7.7o (an imported Toolkit reading that is current, and one kept behind a scan). Amend R4.2 to assert true, false and empty `sc_imported` values.

[MAJOR] IMP:164 (R6.2: `undefined` → stored empty) with IMP:101 (R3.6c "an empty string clears") and R3.5's default — every Toolkit export writes `undefined`, so each re-import proposes clearing any Note the user typed in SpectroCapture. On a captured item the E14 default is "Take the new details". On a pending or set-aside item the Note clears with no prompt, and the text is unrecoverable (DF R2.3). This misreads `undefined` (absent) as a present empty value. Fix: have the owner say whether `undefined` behaves as an omitted field (R3.6a) or as empty. Add a case: a stored Note `Keep me` on a captured item and on a pending item, then re-import Fixture T.

[MINOR] IJ:108 — R6.1 has no negative or superset case. Add Fixture T minus `R550 nm` (generic path), and Fixture T plus an extra trailing column (still a Toolkit export).
[MINOR] IMP:164, IJ:112 — Custom Collection Name has no disposition in R6.2, whose fixed mapping is meant to cover every column; R2.2's fallback would store it. Name it, and assert it in row 112.
[MINOR] IJ:112 — a Note other than `undefined` is untested, so an "always blank Note" build passes. Add one.
[MINOR] IJ:108 — assert explicitly that the two L headers raise no E41 or E12.
[MINOR] IJ:109, IJ:121 — untested: "existing target ignores the name", and a pre-filled name that collides with an existing collection (Capture R1.2 uniqueness).
[MINOR] IJ:117, IMP:124 — gaps around R6.4's mode switch:
  - E43 does not tell the user that an existing target's scan mode will switch.
  - Cancel-before-commit leaving the mode unchanged is untested.
  - "Hold no reading" is undefined for a collection with history only.
  - R3.8k's pre-commit recheck omits R6.4.
[MINOR] IJ:110 — no below-tolerance, nonzero case, e.g. file Lab 0.05 ΔE2000 off → not flagged. "0 flagged" is only as strong as the fixture author's derivation method.
[MINOR] IJ:120 — cites R6.9 but asserts no E43 counts. It is unclear whether ⟨history⟩ counts a displaced current reading, and whether ⟨readings⟩ counts E47-excluded records.
[MINOR] IMP:83, IJ:36 — R3.8 still says "apply R3.8a–o", though R3.8p now exists. The exhaustive action row omits E46/E47, so E46/E47 Cancel and E47 "Pick the file again" are untested.
[MINOR] IMP:48 (R1.5) and IMP:163 — undefined whether E43's "Choose a separator"/"Choose an encoding" override Toolkit detection or are overridden by it.
[MINOR] IMP:179–180 — R6.8b on a set-aside, flagged or quarantined-current item, and R6.8c with a Demo Device (simulated) current value, are untested.
[MINOR] DF:37 (Reading = "1–5 samples"), DF:112 (averaging basis), IJ:111 — the vocabulary contradicts "samples not recorded" and pulls a builder toward the one-sample pseudo-set that N9 rejected. The oracle should assert no sample record and a not-recorded count, not 1.
[MINOR] DF:99 (R1.2), DF:239 (R7.1) — the lists of what an outside reader can see omit snapshot kind, not-recorded samples and never-supplied payload, all of which DJ:117 asserts are readable.
[MINOR] IMP:167 vs CAP:180 (R4.6: illuminant and observer belong with the value, not the reading) — R6.5 puts them on the reading, and no oracle reads the reading-level pair.
[MINOR] CMC:295 vs CM:348 — R4.2d requires basis, spread and verdict shown as unknown or not recorded; the copy line drops all three.
[MINOR] CMJ:236 — UJ2.1-r does not state the imported reading's mode. Studio Markers is M1, so a seeded M2 reading (as in Fixture T) would carry value-absent and fail "no other mark". State "measured in M1".
[MINOR] CM:574 — M2's population (the 15-item seeded file) contains no imported item, so the imported mark is never measured.
[MINOR] EX:88–90, EXJ:14 — R1.1h, R1.1i and R1.1s name only `sc_simulated`. EJ1 does not assert the model cell `Nix Spectro 2`, measured-at equal to Date Saved, or `sc_imported` empty on a never-scanned row, and its ‹P1› history half asserts nothing.
[MINOR] EX:120 — R2.3's claim that out-of-grid readings are unreachable now depends on OQ 4 closing at 400–700 nm / 10 nm, the grid R6.1 hard-codes. Record that in OQ 4.
[MINOR] CAP:147, IJ:109 — unstated whether the Toolkit-set mode is editable at creation. If the user picks M1, R6.4's adopt rule silently overrides it.
[NIT] IJ:110 asserts "0 kept in history, 0 flagged" although zero-count sentences are omitted; phrase these as count identities.
[NIT] IMP:133 — §4 names only UJ 2–2.2 as running without hardware.
[NIT] DF:27 — DJ6's index lists R3.3, but no DJ6 row exercises regeneration.

#### Biggest risks   (what could ship broken behind a green suite)
- A reader that handles plain-decimal Fixture T but not the real export's exponent notation. The suite is green and every real import ends at E42.
- Duplicate imported readings piling up in the history of every scanned item on each Toolkit re-export. History can never be deleted.
- "Newest measurement wins" shipping in place of the owner's rule that a scan always stays current.
- The gamut flag missing on imported readings, a non-negotiable honesty mark, with nothing to catch it.
- User-typed Notes silently cleared on re-import of pending items.
- `sc_imported` absent or wrong in the golden that is the export contract.

#### Genuinely solid   (incl. where minimal scoping is correct that a coverage-zealot would wrongly flag)
- IJ:108's "No E45" is a sharp negative: a build without detection hits the one-column guard under the comma default.
- R6.6 compares only Lab. That sidesteps the file's 0–1 XYZ scale, and "first L" is harmless because both L columns carry the same value.
- IJ:113 offsets by 2 ΔE2000, well clear of wherever DERIVATION_TOLERANCE closes.
- IJ:114's record 4 matches R1.4's numbering.
- IJ:120 covers the later and earlier branches in one fixture.
- DJ:113 ("never supplied, not archive-unavailable") and EXJ:14 ("counts no unavailable archive") catch a build that conflates an imported reading with R5.5a damage.
- DJ:114 ("A stays current") and DJ:117 kill a build that infers the current reading from the latest record.
- UJ3-c kills builds that create or link a saved "imported" device.
- UJ2.1-r's ZX-023 and "filter lists ZX-022 alone" kill a mark that sticks to the item instead of the current reading.
- Correctly minimal, and no new cases needed:
  - Capture R1.10 needs no Capture journey; IJ:109 and IJ:117 exercise it.
  - R6.8e's absent-item branch is covered by R3.3d.
  - E7/E9/E10 on Toolkit files follow §1–§3 under §6's preamble.
  - The file's LCh/XYZ/sRGB/HEX correctness is not stored, so it needs no oracle.

#### Missing / over-tested
Missing:
- Repeat import from the R6.8c state.
- Toolkit reading newer than a scan.
- R6.8d with the same date and a different spectrum, and the reason for an earlier imported reading.
- Gamut-clipped and in-gamut Fixture T records.
- R6.7 Date Saved and Mode cases, and the Illuminant/Observer/file-Lab rules.
- R6.1 negative and superset cases.
- DF R7.7o plus `sc_imported` in R4.2.
- A re-import that preserves a Note.
- A real Note value.
- Existing-target name ignored.
- A below-tolerance negative for R6.6.
- E46/E47 Cancel and re-pick.

Over-tested: none. Row 112's list of excluded columns is long but earns its place.

| Row ID | disposition |
| :--- | :--- |
| Import-R1.5 | OBJECT (MINOR IMP:48 separator/encoding override) |
| Import-R3.3 | OBJECT (BLOCKER R6.8c re-import) |
| Import-R3.8 | OBJECT (MINOR IMP:83/IJ:36) |
| Import-R6.1 | OBJECT (BLOCKER fixture fidelity; MINOR IJ:108 negatives) |
| Import-R6.2 | OBJECT (MAJOR `undefined` Note; MINOR Custom Collection Name; MINOR real Note; MINOR no-E41) |
| Import-R6.3 | OBJECT (MINOR IJ:109/121) |
| Import-R6.4 | OBJECT (MINOR IJ:117/IMP:124 mode switch) |
| Import-R6.5 | OBJECT (MAJOR gamut flag; MINOR samples-not-recorded oracle; MINOR illuminant/observer placement) |
| Import-R6.6 | OBJECT (MAJOR R6.7 file-Lab/illuminant; MINOR below-tolerance) |
| Import-R6.7 | OBJECT (MAJOR R6.7) |
| Import-R6.8 | OBJECT (BLOCKER R6.8c re-import; MAJOR N6 newer-Toolkit; MAJOR R6.8d branches; MINOR R6.8b/c variants) |
| Import-R6.9 | OBJECT (BLOCKER E43 count; MINOR IJ:120 counts) |
| Import-R6.10 | OBJECT (BLOCKER fixture fidelity) |
| DF-R1.6 | ALIGN |
| DF-R2.2 | ALIGN |
| DF-R2.3 (with R2.3j) | OBJECT (MAJOR history order; MAJOR R6.8d reason; MINOR DF:37 vocabulary) |
| Device-R1.21 | ALIGN |
| Device-R1.22 | ALIGN |
| CM-R2.4 (with R2.4j) | OBJECT (MINOR CMJ:236 mode; MINOR CM:574 M2 population) |
| CM-R2.7 | ALIGN |
| CM-R4.2 (with R4.2d) | OBJECT (MINOR CMC:295 copy/row mismatch) |
| CM-R5.2 (with R5.2b/e) | ALIGN |
| CM-R8.9 | ALIGN |
| Export-R1.1 (with R1.1p/t) | OBJECT (MAJOR golden inventory; MINOR EX:88–90/EXJ:14; MINOR EX:120 grid) |
| Export-R1.2 | OBJECT (MAJOR golden inventory) |
| Export-E1 | ALIGN |
| Capture-R1.10 | OBJECT (MINOR CAP:147 creation-time mode) |

### interface

#### Verdict
Sound after fixing Blockers. The six PRDs are mostly wired in both directions, and `sc_imported` lands before any export format version ships, so no existing consumer breaks. But two self-contradictions (re-import idempotency and the E43 reading counts) and a cluster of copy, obligation and coverage gaps would leave a builder guessing on the Toolkit path.

#### Surface & consumers (brief)
- **Surface.** The contract the six locked PRDs publish to each other and to the builder:
  - row IDs and the R3.8 action transitions;
  - the copy states: E43's Toolkit variant, E46, E47, E14, Collection Mode's Mark labels, detail and history lines, and Export E1;
  - the inherited-obligation tables;
  - the export CSV's app columns.
- **Consumers.** Build agents, who implement and test by row and state ID. Sibling PRDs, through the obligation lines. The Cataloger, through the copy. Scripts parsing the export CSV, which read `sc_` columns by name.
- **What changed** (`git diff 6374538 417fdac -- docs`, worktree `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import`):
  - Inventory Import gains §6 (R6.1–R6.10, R6.8a–e), R3.8p, E46, E47, E43's Toolkit variant and UJ 3.
  - Mirrors: Data Foundation R2.3j and DJ6; Device's imported kind; Collection Mode's R2.4j mark and copy lines; Export `sc_imported` and R1.1t; Capture R1.10.
  - The only change a consumer parses is the new `sc_imported` column. Export F35 correctly lands it before any release.

#### Findings

[BLOCKER] IF1-B1 — Import R6.8c/R6.8d: no reading-identity rule, so the published idempotent re-import breaks
- **Location:** `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import.md:180-181`, against R3.3 (:78, "idempotent, readings included") and R6.9 (:171, "changes nothing").
- **The gap:**
  - R6.8c has no same-reading test. It keeps the record's reading "in version history as an earlier reading" on every import where the match's current value is a SpectroCapture scan.
  - R6.8d tests identity only against the *current* imported reading.
- **Scenarios:**
  - (a) Re-import Fixture T over the state of the UJ 3 case at `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import-journeys.md:118` (TK-1 current value is a scan). A second copy of TK-1's Toolkit reading enters history, and E43 says "Kept in history: 1" every time.
  - (b) Import v1, then v2 (newer; v1's reading goes to history), then v1 again. R6.8d's "an earlier one goes to history" duplicates v1's reading. This narrows N14's "an identical reading (same date and spectrum) changes nothing" to the current reading only.
  - (c) Same Date Saved with different reflectances is neither "the same", "later" nor "earlier", so it has no outcome.
- **Why it matters:** history readings can never be deleted on their own (DF R2.3, §6), so every duplicate is permanent.
- **Mutation:** a build that appends on every R6.8c match and compares only against the current value passes every UJ 3 case. The only repeat case (journeys :119) starts from an all-imported-current state.
- **Fix:**
  - Define reading identity once: Date Saved equal to the millisecond, and every reflectance numerically equal.
  - Put a rule ahead of R6.8b–d: a record whose reading is identical to any reading the item already holds with an imported-kind snapshot changes nothing and counts as unchanged.
  - Send the same-date/different-spectrum outcome to the owner (it is a WHAT) and state it.
  - Add UJ 3 cases for a repeat over the :118 state and for v1→v2→v1.

[BLOCKER] IF1-B2 — UJ 3's E43 counts contradict each other
- **Location:** `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import-journeys.md:110` vs `:118`.
- **The contradiction:**
  - :110 — a new target where TK-1–TK-3 are all R6.8a new items expects "3 becoming current".
  - :118 — TK-3 is an R6.8a new item, yet the case expects "1 becoming current (TK-2) … and 1 new (TK-3)", so TK-3's reading is not counted.
  - R6.8a ("canonical value is the record's reading") and R6.9 ("readings becoming current") say new items do count.
- **Mutation:** count R6.8a readings in ⟨current⟩ and :110 passes but :118 fails (2≠1). Exclude them and :118 passes but :110 fails (0≠3). No build passes both.
- **Fix:** change :118 to "2 becoming current (TK-2, TK-3), 1 kept in history (TK-1); New: 1 (TK-3)", and state in R6.9 that ⟨current⟩ includes new items' readings.

[MAJOR] IF1-M1 — E43 Toolkit variant: "Choose an encoding" and "Choose a separator" have no defined effect
- **Location:** `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import-copy.md:29`; R6.1 at `.../import/prd-inventory-import.md:163`; R1.5b at `:56`; R3.8n.
- **The problem:** R6.1 splits record 1 on semicolons whatever the active delimiter, then requires "UTF-8 with the semicolon delimiter".
  - "Choose a separator" re-reads (R3.8n) into an identical preview. The action silently does nothing.
  - "Choose an encoding" (for example UTF-16LE) breaks the signature test. The file silently drops into the generic §1–§3 flow and loses its readings.
- **Coverage:** UJ 2's every-action case (journeys :36) has no destination for either.
- **Fix:** either withdraw both actions from the Toolkit variant, keeping the settings displayed per R1.2, or say in R1.5b/R6.1 which takes precedence and name the fallback. Add a case.

[MAJOR] IF1-M2 — Measurement-mode fit is not part of the commit contract
- **Location:** R3.8k at `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import.md:124`; R6.4 at `:166`; R3.2 at `:77`; Capture R1.10 at `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/capture-mode/prd-capture-mode.md:147`.
- **The gap:** R6.4 checks mode fit "before mapping", and R3.8p checks it again on a newly chosen target. R3.8k rechecks only "source and session gate" at commit.
- **How the target changes under an open preview:**
  - Its scan mode can be changed whenever no write is held (Capture R1.10). Collection Mode UJ9.5-g already changes target settings while a CSV preview is open.
  - A session can start, save and end on the target between preview and Import.
- **Consequence:** the commit lands M2 readings into an M1 collection that holds readings — exactly what F76 forbids — with no state to route to.
- **Also unstated:**
  - When adoption happens. R3.2's whole-or-nothing still names only R3.3a–d, so it is unclear whether an E44 failure or Cancel rolls the adoption back.
  - Whether a Toolkit-created target's scan-mode field is locked. Capture R1.10 says "pre-filled … or taking"; a user edit to M1 would create a target that doesn't fit.
- **Fix:**
  - R3.8k adds "mode fit (R6.4) → E46".
  - R3.2 names R6.8a–e and R6.4's adoption inside the atomic commit.
  - R6.4 and Capture R1.10 say the field is set to the file's mode and can't be changed in that creation.
  - Add cases for a mode change between preview and Import, and for E44 rolling back an adoption.

[MAJOR] IF1-M3 — E14's copy promises something the Toolkit path breaks
- **Location:** `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import-copy.md:24`; R3.5 at `.../import/prd-inventory-import.md:80`; R6.8 at `:170`.
- **The problem:** E14 says "Your measurements aren't touched either way", and R3.5 says "neither path touches measurements/history". Both still render for a Toolkit import.
  - R6.8c adds the Toolkit reading to the captured item's history.
  - R6.8d can make a new reading current.
  - UJ 3 (journeys :118) shows E14 on exactly such a row (TK-1).
  - The copy header claims every promise is backed by a row; this one is contradicted.
- **Fix:** add a Toolkit variant of E14, for example "Readings you scanned here stay current; readings from this file are added to history or become current as the preview shows", and carve "save R6.8" into R3.5.

[MAJOR] IF1-M4 — Collection Mode's E12 has no meaning for the imported mark
- **Location:** `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/collection-mode/prd-collection-mode-copy.md:173`; R2.8 at `.../collection-mode/prd-collection-mode.md:309`; UJ2.1-k at `.../collection-mode/prd-collection-mode-journeys.md:229`.
- **The gap:**
  - R2.8 renders each mark "with its shape, its chip label and what it means", UJ2.1-k now expects twelve marks, and F221's Decision names E12.
  - The meanings live only in E12's body. Its "Where the reading came from" group covers Simulated, No spectral data and Samples disagreed, and has nothing for Imported.
  - The builder has to invent user-facing text. UJ2.1-k reads by identifier, so an empty meaning passes.
- **Fix:** add a sentence to E12 after Samples disagreed, for example "Imported reading: the reading came from a Nix Toolkit export; its instrument's serial and firmware and how many samples it averaged aren't known." Add E12 and R2.8 to F221's Carried by.

[MAJOR] IF1-M5 — Collection Mode R4.2d and R5.2e don't match their copy lines
- **Location:** rows at `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/collection-mode/prd-collection-mode.md:348` and `:381`; copy lines at `.../collection-mode/prd-collection-mode-copy.md:295` and `:310`.
- **The mismatch:**
  - R4.2d (and F221) require serial, firmware, samples, averaging basis, spread and verdict shown "as unknown or not recorded". The imported copy line renders only "serial unknown, firmware unknown · samples not recorded", with no basis, spread or verdict.
  - R5.2e says "neither [samples nor spread] was recorded". The copy renders only "Samples not recorded".
  - A build that follows the rows invents strings; one that follows the copy omits what the fence says to show.
- **Fix:** either add the missing basis, spread and agreement strings to both copy lines, or narrow R4.2d, R5.2e and F221 to what the copy shows. That choice is the owner's.

[MAJOR] IF1-M6 — The new placeholders have no owner, and one count has no row
- **Location:** Capture §12 at `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/capture-mode/prd-capture-mode.md:406`; `.../import/prd-inventory-import-copy.md:35`; R6.9 at `.../import/prd-inventory-import.md:171`.
- **The problem:**
  - Import's copy hands placeholder ownership to Capture §12. That registry ("Import also uses ⟨existing⟩ …") was not extended with ⟨readings⟩, ⟨mode⟩, ⟨current⟩, ⟨history⟩, ⟨unchanged readings⟩, ⟨flagged⟩, ⟨file mode⟩, ⟨collection mode⟩ or ⟨modes⟩.
  - Import's own note still says "Other count placeholders name the corresponding R3.6 count". None of the new ones are R3.6 counts.
  - ⟨readings⟩ is missing from R6.9's list even though F73 requires "its reading count". Once E47 or E10 exclude any records, it is a guess whether ⟨readings⟩ counts all records or eligible ones.
- **Fix:** register the nine placeholders (or move ownership), define each against an R6.9 or R6.4 count, and add "the eligible reading count" to R6.9.

[MAJOR] IF1-M7 — E46's shared headline can't render its mixed-modes variant, and its remedies mislead
- **Location:** `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import-copy.md:32`.
- **The problems:**
  - The headline "This file was measured in ⟨file mode⟩" has no single ⟨file mode⟩ when the file mixes modes.
  - The collection remedy leaves out R6.4's third option, a collection with no readings yet.
  - The mixed-modes remedy ("Export each mode … then import each file") doesn't say "into separate collections", so the second file into the same collection hits E46 again.
  - It also claims the Toolkit can export by mode, which the research does not show.
- **Fix:**
  - Give each variant its own headline; the mixed one could read "This file mixes measurement modes".
  - Collection variant: "…or one that uses ⟨file mode⟩ or has no readings yet".
  - Mixed-modes variant: "…import each into its own collection", and drop the unverified export-by-mode claim.

[MAJOR] IF1-M8 — `sc_imported` has an inbound obligation but no outbound one
- **Location:** Import's Data Export line at `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import.md:148`, vs Export's Inventory Import line at `.../export/prd-data-export.md:173` and Export R1.2.
- **The asymmetry:**
  - Export cites "its R6.5 … F35" as the source of `sc_imported`.
  - Import's Data Export line still covers only passthrough columns (Rows R2.2, R2.6), and Import's fence map says F79 "governs no row here".
  - It is the one obligation among the six PRDs wired on one side only. A later change to R6.5's snapshot kind won't trace to the export column.
- **Fix:** add "a Toolkit reading exports marked `sc_imported` (Export R1.2/R1.1t)" to Import's Data Export line, with R6.5 in its Rows, and name that line in F79's map.

[MAJOR] IF1-M9 — Import's Capture obligation line states a rule §6 now breaks
- **Location:** `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import.md:150`.
- **The problem:**
  - The line still says "New items append pending and matched items retain state/position".
  - R6.8a appends new items captured, and R6.8b turns a matched pending or set-aside item (one with no current value) into captured.
  - Capture's own Inventory import line does carve "save a Nix Toolkit export's records, which arrive captured … (R6.4/R6.8)"; Import's side does not.
- **Fix:** add the same "save R6.8" carve-out and R6.8 in the Rows. Say whether a matched set-aside item leaves set-aside, since that is a Capture state change.

[MAJOR] IF1-M10 — `sc_imported`'s true and empty values are never checked against a golden
- **Location:** Export R1.1h/s/i at `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/export/prd-data-export.md:88-90` and R4.2 at `:145`; DF R7.7 at `.../data-foundation/prd-data-foundation.md:245` and R7.7e at `:258`; post-lock.md:131 ("sole fixture inventory").
- **The gap:**
  - R1.2 says `sc_imported` "follows the same rule" as `sc_simulated`. But the missing-data rows spell out only "snapshot/`sc_simulated`" for never-scanned, quarantined-current and quarantined-history rows.
  - R4.2 asserts "all three `sc_simulated` values" and nothing for `sc_imported`.
  - DF R7.7a–n, from which the goldens are cut, has no imported-reading fixture.
  - EJ1's new row checks true and false, but not empty.
- **Mutation:** emit `sc_imported=false` for a never-scanned item. Every case and golden passes, and a consumer reads "known, not imported" where the contract says "unknown".
- **Fix:** make R1.1h/s/i read "snapshot/`sc_simulated`/`sc_imported`"; have R4.2 assert all three values of both columns; add a Toolkit-imported fixture row to DF R7.7.

[MAJOR] IF1-M11 — F64/DJ6's history ordering contradicts DF R2.1, Collection Mode R5.3 and Export R1.1g
- **Location:** DJ6 at `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/data-foundation/prd-data-foundation-journeys.md:114`; DF R2.1 at `.../data-foundation/prd-data-foundation.md:112`; Collection Mode R5.3 at `.../collection-mode/prd-collection-mode.md:385`.
- **The contradiction:**
  - DJ6, a stored-data oracle, asserts that "over-time views and history place B before A by its measurement time".
  - DF R2.1 (aligned) says "history orders by record time".
  - Collection Mode R5.3 (aligned) lists E17 newest-recorded first.
  - Export R1.1g orders history rows by per-item sequence.
- **Why no build satisfies all of them:** the only SQL-visible history order is the sequence. Giving B (recorded after A) a sequence before A's means renumbering an immutable reading. Giving it one after A makes DJ6's "by measurement time" false for history.
- **Fix (keeps every aligned row):** read F64's "history" as E17's measured-order (over-time) view. DJ6 then asserts B precedes A in measured order and follows A in record/sequence order. Otherwise amend R2.1, R5.3 and R1.1g to say how an imported reading is sequenced.

[MAJOR] IF1-M12 — R6.2 doesn't close the stored column set
- **Location:** `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import.md:164`; UJ 3 at `.../import/prd-inventory-import-journeys.md:112`.
- **What the fixed mapping leaves open:**
  - What happens to Custom Collection Name. R6.3 uses it, but R6.2 never says whether it is stored.
  - Whether the reading columns are also kept as passthrough metadata: Nix Device, Date Saved, Illuminant, Observer, Measurement Mode and the 31 reflectances.
  - What an unlisted extra column does. §6's "every rule there … still applies" pulls in R2.2's "Import remaining columns as metadata".
- **Why it matters:**
  - Passthrough columns are permanent: Collection Mode's E19 copy says "A column can't be removed once it's added".
  - Every passthrough column is emitted in the export (Export R2.3), so two conforming builds produce different stored and exported column sets.
  - UJ 3's metadata case asserts only that Index and the file's L…HEX are absent, so a build storing 37 extra columns passes.
  - R6.6 needs L, a and b, but R6.1's signature doesn't require them.
- **Fix:** list the disposition of every Toolkit column (stored metadata = Note plus the five Density columns, nothing else); state the rule for unlisted columns; either require L, a, b in R6.1 or say R6.6 is skipped without them; make UJ 3 assert the complete stored column list.

[MAJOR] IF1-M13 — Illuminant, Observer and device values have no failure path
- **Location:** `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import.md:167-169` (R6.5–R6.7).
- **The gap:**
  - R6.6 derives Lab "under the file's own illuminant and observer", and R6.5 stores them. But R6.7 excludes only unreadable Date Saved, Measurement Mode and reflectances.
  - Nothing covers an illuminant/observer pair the app can't derive under. While OQ 21 is open the offered set is one pair (Capture R1.5, F56).
  - Nothing covers records within one file whose illuminant or observer differ. Mode is checked per file; these are not, though the format carries them per record.
  - "A mode" in R6.7 has no defined value set (M0–M2 per Capture R1.10, and M3 is unaddressed).
  - A blank Nix Device produces a snapshot with no model.
- **Fix:** add Illuminant, Observer and Nix Device to R6.7 with their accepted value sets. State the outcome for an unsupported pair (exclude through E47, or skip R6.6 and store) and for mixed pairs. Add cases.

- [MINOR] IF1-m1 — R3.8 (`.../import/prd-inventory-import.md:83`) still says "apply R3.8a–o", though R3.8p exists. UJ 2's every-state case (journeys :36) stops at E45, so E46/E47 Cancel and E47's "Pick the file again" have no case.
- [MINOR] IF1-m2 — DF R2.1 (:112, "the stored mean with … averaging basis") and R2.3's lead (:115, "A later reading supersedes it") have no "save R2.3j" carve-out. An imported canonical value has no samples or basis, and a kept-behind imported reading is recorded later but supersedes nothing. AGENTS.md's Canonical value vocabulary has the same gap.
- [MINOR] IF1-m3 — R6.8b–d key on prose ("scanned in SpectroCapture", "was imported") rather than the current reading's snapshot kind, as CM R2.4j does. Demo Device, restored-from-imported, flagged and quarantined matches become guesses. Key on kind: live or simulated → R6.8c; imported → R6.8d.
- [MINOR] IF1-m4 — R6.1 detects only a semicolon-split header. A Toolkit export re-saved as comma CSV silently becomes a plain import, with readings dropped and about 57 permanent passthrough columns added. Test the signature under the active delimiter too, or show a notice.
- [MINOR] IF1-m5 — Imported items count as "scanned" in the shared counts (Capture E22/E25; Collection Mode UJ2.1-j wording), though none was scanned in the app.
- [MINOR] IF1-m6 — E43's Toolkit variant uses the label "Unchanged:" twice, once for rows and once for readings. Rename the second "Readings unchanged:".
- [MINOR] IF1-m7 — Other one-sided obligation lines:
  - Device's DF line (device PRD :263) and DF's Device inbound line (DF :278) lack R1.22's new "imported snapshot is never a device record".
  - Device's Export line (:265) and Export's Device inbound line (:172) still cover `sc_simulated` only.
  - DF's Import inbound Rows (:281) omit the amended R1.6.
  - Import's Collection Mode line names only the mark, while Collection Mode's side lists R2.7, R4.2d, R5.2b and R5.2e.
- [MINOR] IF1-m8 — Collection Mode R5.2b's "The device" has no rendering for an unknown serial; Device R1.18's "⟨model⟩ (⟨serial⟩)" form breaks.
- [MINOR] IF1-m9 — The imported snapshot records no source, yet the labels hard-code "Nix Toolkit" and `sc_imported` is a plain boolean. CxF import in v2 will need a file migration to tell sources apart; consider a source field now.
- [MINOR] IF1-m10 — UJ 3's repeat case (:119) asserts reading counts only. Add "rows Unchanged: 3, no E14" so a build that compares the raw `undefined` against the stored empty Note (R6.2) can't pass while showing E14 on every re-import.
- [MINOR] IF1-m11 — R2.3j gives every kept-behind imported reading the reason "initial", so an item can show two "First reading" history lines.
- [MINOR] IF1-m12 — R6.1's "every later step names it a Toolkit export" has copy only in E43; the target and mapping steps have no string.
- [MINOR] IF1-m13 — Import's Vocabulary "Ready to capture … new rows pending, existing rows preserved" and the README's "Import ends at ready-to-capture" now read false for a Toolkit import.
- [MINOR] IF1-m14 — post-lock has no ADR-0003 item for this amendment's schema consequences (third snapshot kind, samples-not-recorded, a never-supplied payload on a reading with no samples, imported-reading sequencing). The ADR-0003 queue entry still reads "the stored mean canonical with the raw payload archived beside it".
- [NIT] IF1-n1 — R6.3's "carries the same one": say whether R2.3 equality or exact text.
- [NIT] IF1-n2 — R1.5 (:48) "without numeric or date coercion": add "save §6's reading columns".
- [NIT] IF1-n3 — Export R1.2 could state that `sc_simulated` and `sc_imported` are never both true.
- [NIT] IF1-n4 — R6.2 says Index is "read for R6.6's check", but R6.6 doesn't use it.

#### Biggest risks   (what existing consumers/scripts/agents break)
- **Permanent duplicate readings (B1).** Re-importing the same Toolkit export into a collection with re-scanned items duplicates Toolkit readings in history for good. That breaks the Import PRD's core promise of an idempotent re-import.
- **Build gate stalls (B2).** UJ 3's two count cases can't both pass, so the gate stalls or an agent "fixes" R6.9 to match the wrong case.
- **Export CSV consumers.**
  - `sc_imported` empty vs false is unasserted (M10).
  - The passthrough column set differs between conforming builds, and those columns are permanent (M12).
- **Silent mode mixing (M2).** A setting change or a session between preview and Import mixes modes, the exact outcome N8 forbids.
- **Misleading or missing user copy.** E14's promise is false (M3). E12's imported meaning is missing (M4). R4.2d/R5.2e disagree with their copy (M5). E46's mixed-modes headline can't render (M7).

#### Genuinely well-designed   (incl. where a deliberate inconsistency is correct that a style-checker would wrongly flag)
- **Export F35 is correct.** `sc_imported` joins v1's first golden before any export format version ships, and R2.5 already says to parse by name. No version bump is needed; adding one would be wrong.
- **Four of five sibling obligations are wired both ways.** Import R6.5/R6.8 ↔ DF R2.3j; Import R6.5 ↔ Device R1.21/R1.22; Import R6.5 ↔ Collection Mode R2.4j; Import R6.4 ↔ Capture R1.10. Only Export is one-sided (M8).
- **Action labels resolve both ways.**
  - E46's "Pick the file again" and "Choose another collection", and E47's "Continue without them" and "Pick the file again", match R3.8b/c/p exactly.
  - Every new copy action resolves back to a transition row.
  - E47 reuses the E7/E9/E10 exclusion idiom and one-based record numbers ("record 4"), so it is predictable.
- **Collection Mode's mark work is consistent.**
  - The imported mark's chip, filter and VoiceOver labels match UJ2.1-r and R5.2b exactly.
  - The mark counts move 9→10 and 11→12 consistently across R2.4, R8.9, UJ2.1-k and UJ9.6-a.
  - R2.4j parallels R2.4c by keying on snapshot kind.
  - The new seeded IDs ZX-022 and ZX-023 collide with nothing.
- **Keeping "never supplied" distinct from "archive unavailable"** (DF R1.6/R2.3j, Export R1.1t) is right: it stops E1 counting imported readings as damage.
- **Fixture T** reproduces the real quirks (header/data quoting mismatch, the duplicate L, `undefined` notes, reflectance above 1.0) with no real data values.
- **Anchors hold.** The new anchors resolve, and vision J7's anchor stays stable while its body updates — correctly not renamed.

#### Missing / over-engineered
- **Missing from the rows:**
  - a reading-identity rule (B1);
  - a commit-time mode recheck and adoption atomicity (M2);
  - a complete Toolkit column disposition (M12);
  - a failure path for illuminant and observer (M13).
- **Missing from the copy and fixtures:**
  - a Toolkit variant of E14 (M3);
  - E12's imported meaning (M4);
  - registered placeholders (M6);
  - an imported-reading fixture in DF R7.7 (M10);
  - an ADR-0003 post-lock item (m14).
- **Missing UJ 3 cases:**
  - a repeat over a scanned-current item;
  - v1→v2→v1;
  - same date with a different spectrum;
  - E46/E47 Cancel;
  - a mode change between preview and Import;
  - E44 rolling back a mode adoption;
  - an existing target ignoring the file's collection name (R6.3).
- **Not over-engineered:** two booleans instead of a device-kind enum is the owner's N11 decision and fine for v1. m9 is only a forward-compatibility note.

| Row ID | disposition |
| :--- | :--- |
| Import-R1.5 | OBJECT (IF1-M1, IF1-n2) |
| Import-R3.3 | OBJECT (IF1-B1) |
| Import-R3.8 | OBJECT (IF1-M2, IF1-m1) |
| Import-R6.1 | OBJECT (IF1-M1, IF1-m4, IF1-m12) |
| Import-R6.2 | OBJECT (IF1-M12) |
| Import-R6.3 | ALIGN |
| Import-R6.4 | OBJECT (IF1-M2, IF1-M7) |
| Import-R6.5 | ALIGN |
| Import-R6.6 | OBJECT (IF1-M13, IF1-M12) |
| Import-R6.7 | OBJECT (IF1-M13) |
| Import-R6.8 | OBJECT (IF1-B1, IF1-M3, IF1-m3) |
| Import-R6.9 | OBJECT (IF1-B2, IF1-M6) |
| Import-R6.10 | OBJECT (IF1-B2, IF1-m1) |
| DF-R1.6 | ALIGN |
| DF-R2.2 | ALIGN |
| DF-R2.3 (with R2.3j) | OBJECT (IF1-M11, IF1-m2, IF1-m11) |
| Device-R1.21 | ALIGN |
| Device-R1.22 | ALIGN |
| CM-R2.4 (with R2.4j) | OBJECT (IF1-M4) |
| CM-R2.7 | ALIGN |
| CM-R4.2 (with R4.2d) | OBJECT (IF1-M5) |
| CM-R5.2 (with R5.2b/e) | OBJECT (IF1-M5, IF1-m8) |
| CM-R8.9 | ALIGN |
| Export-R1.1 (with R1.1p/t) | OBJECT (IF1-M10) |
| Export-R1.2 | ALIGN |
| Export-E1 | ALIGN |
| Capture-R1.10 | OBJECT (IF1-M2) |

### architecture

#### Verdict
Build after addressing Blockers. The overall shape is sound: the Toolkit import runs through the one existing import flow, every stored value is recomputed from the spectrum, the current reading is selected explicitly, and the imported mark is worked out from the snapshot kind. Two things block building: re-importing an export adds duplicate readings to history, which can never be deleted, and Data Foundation's reading model and its rule for which reading is current are only half amended, so the ADR-0003 author would have to guess.

#### Architecture in brief
- **Import §6** detects the file, fixes the mapping, checks each record and decides each record's outcome (R6.8a–e).
- **Data Foundation** owns what is stored: the reading, its snapshot, its derived sets, which reading is current, and version history (R2.3j).
- **Device Management** owns the snapshot-kind vocabulary. It adds a third kind, "imported".
- **Collection Mode** works out the imported mark from the current reading's snapshot kind.
- **Data Export** writes `sc_imported` and R1.1t's empty cells.
- **Capture Mode** owns the collection's chosen scan mode, which an import may now set.

The key tradeoff: imported values are usable as canonical values right away. The cost is that three Data Foundation invariants that used to hold everywhere now have exceptions:
- a reading has 1–5 samples;
- the current reading is the latest-recorded one;
- the app asks for every supersession reason rather than assigning one.

The amendment states those exceptions only in the Import rows and in R2.3j. The Data Foundation rows that ADR-0003 will build the schema from are left unamended.

#### Findings

Root for every path below: `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import`

**[BLOCKER] B1 — Import R6.8 / Data Foundation R2.3j: re-importing adds duplicate readings to history, which cannot be deleted**

- **Where:**
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import.md:180` — R6.8c: "the reading is kept in version history", with no check against readings the item already holds.
  - `:181` — R6.8d: "the same reading" is compared only with the current value; "an earlier one goes to history".
  - `:179` — R6.8b makes the reading current again after an operator's Flag.
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/data-foundation/prd-data-foundation.md:137` — R2.3j: "else kept behind A", with no equality rule at all.
  - These contradict Import R3.3 (`:78`, "idempotent, readings included") and R6.9 (`:171`, "changes nothing").
- **Scenario:** The Cataloger's normal loop: import the Toolkit export, re-scan some items in SpectroCapture, keep using the Toolkit, re-export the whole collection, re-import.
  - Each re-scanned item (R6.8c) gets another copy of its unchanged Toolkit reading in history on every re-import.
  - An item whose older Toolkit reading already sits behind a newer imported one (UJ 3's TK-1, journeys `:120`) gets another copy too.
  - A Flagged imported reading becomes current again.
  - Even with no intervening edit, repeating the import in journeys `:118` adds a second TK-1 reading, which R6.9 forbids.
  - Data Foundation R2.3 and R6.1 make a reading removable only by deleting the whole item, so the duplicates are permanent. They inflate the reading counts on the delete confirmations E8/E14/E33, duplicate points in over-time views, and duplicate rows in the history export.
- **Mutation that passes today:** build R6.8c and R2.3j literally, appending the incoming reading behind the current one without comparing it with the item's other readings. Every UJ 3 and DJ6 case still passes. The only re-import case (journeys `:119`) targets the all-imported-current state, which R6.8d's current-value comparison already covers.
- **Cases that catch it:**
  - "State left by journeys `:118` | import the same file again | TK-1 holds exactly 2 readings; E43 counts 3 unchanged readings and 0 kept in history; the store is unchanged."
  - "State left by journeys `:120` | import that variant again | nothing changes."
  - The literal build fails both; a build that checks for duplicates passes both.
- **Fix:** put one rule in both R2.3j and R6.8, applied before R6.8b–d: "A record whose reading equals one the item already holds, current or in history (same Date Saved, measurement mode and every reflectance), changes nothing and counts as unchanged." F74 and F82 stay intact; N14 already says an identical reading changes nothing.

**[MAJOR] M1 — Data Foundation R2.3j: the current-reading rule names no time axis, and R2.3's opening sentence still applies to every reading**

- **Where:** Data Foundation `:137` says B becomes current "over an older imported A". Data Foundation owns two time axes, measurement time and record time (R2.1, `:112`).
- **Scenario:** measured by record time, every existing A is older than the reading being imported. A store built from Data Foundation alone would therefore make every imported B current over an imported A. That contradicts Import R6.8d (`:181`, which orders by Date Saved) and journeys `:120`.
  - R2.3's opening sentence (`:115`), "A later reading supersedes it", is unamended, yet R2.3j now records later readings that supersede nothing.
  - R6.8d does not cover a record with the same Date Saved but different reflectances: it is neither the same, later nor earlier.
- **Fix:**
  - R2.3j: "over an imported A whose measurement time is earlier than B's".
  - R2.3's opening sentence: add "save as R2.3j states".
  - R6.8d: name the outcome for an equal Date Saved with a different spectrum, for example kept in history and listed in E43.

**[MAJOR] M2 — History order contradicts Data Foundation R2.1 and Collection Mode R5.3**

- **Where:**
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/data-foundation/prd-data-foundation-fences.md:744` — F64: "which history and over-time views place by its measurement time".
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/data-foundation/prd-data-foundation-journeys.md:114` — DJ6: "over-time views and history place B before A by its measurement time".
  - These contradict Data Foundation R2.1 (`:112`, aligned and unamended: "history orders by record time") and Collection Mode R5.3 (`/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/collection-mode/prd-collection-mode.md:385`, aligned: history lists "newest-recorded first", and its Measured order is the over-time view).
- **Scenario:** in Collection Mode's Recorded order, TK-1's imported B (recorded at the commit) is listed above the current A, which is not measurement-time order.
  - A build faithful to R2.1 and R5.3 fails DJ6 `:114` as written.
  - A build faithful to DJ6 breaks R5.3. If it also sorts every reading by measurement time, it misplaces restores, because R2.3f gives a restore its source's measurement time. That is why R2.1 orders history by record time.
- **Fix:** Collection Mode's Measured-order view already delivers N10 ("history sorts it by when it was actually measured").
  - Narrow F64 and DJ6 to: "over-time views place B by its measurement time; recorded-order history places it at its record time, not current".
  - Add a Collection Mode R5.3 case with TK-1.
  - If the owner wants the recorded view itself to place imported readings by measurement time, amend R2.1 and R5.3 explicitly with a mixed-key rule and its tie-break, not through a journey.

**[MAJOR] M3 — Data Foundation R2.3j assigns a supersession reason that R2.4 says the app must ask for**

- **Where:** R2.3j (Data Foundation `:137`) records "re-measurement over an imported A" with no correction question. R2.4 (`:116`, aligned and unamended) says every reading records its reason and "the app asks rather than guessing". Owner decision N14 decides only which reading is current, not the reason.
- **Why it matters:** under R2.5, a re-measurement keeps its predecessor as a legitimate earlier point, while a correction marks it never-true and removes it from QC references.
- **Scenario:**
  - A user who re-measured in the Toolkit because the first reading was bad finds that bad reading kept as a valid point.
  - The reason for a B kept behind a *newer* imported A is also ambiguous: is B "over an imported A" when it lands behind it?
  - DJ6 `:115` builds "re-measurement" into its expected result.
- **Fix:** route to the owner. Either:
  - (a) add an explicit exception to R2.4: "save a Toolkit reading over an imported one, recorded as re-measurement (R2.3j)"; or
  - (b) record the reading as correction-unconfirmed and add it to E11/E26's after-session set, which R2.8 already makes answerable in one pass.

  In both cases, state the reason for a reading kept behind A explicitly (initial).

**[MAJOR] M4 — Data Foundation's reading model is not amended for a reading with no samples**

- **Where:**
  - Data Foundation Vocabulary (`:37`): "Reading — one saved set of 1–5 samples".
  - R2.1 (`:112`, aligned and unamended): "Keep every sample…"; the canonical value carries an "averaging basis".
  - R2.3a (`:128`): "from 1–5 samples".
  - R2.3f (`:133`): a restore copies its source's samples, verdict and spread.
  - R2.3j creates a reading whose samples are not recorded, but never states its averaging basis, agreement verdict or spread, or what restoring it does.
  - Collection Mode R4.2d (`:348`) and Export R1.1t (`/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/export/prd-data-export.md:94`) already say those values are unknown or empty. Data Foundation, which owns the canonical value, is the only document that says nothing.
- **Scenario:** ADR-0003 is drafted from Data Foundation. A sample-count constraint of 1–5, a required averaging basis, or a mean computed from sample rows would each make an imported reading impossible to store. That lands in the user's file format, the sharpest one-way door in the ADR queue.
- **Fix:**
  - Amend "Reading" and R2.1: "an imported reading holds its given spectrum as its stored mean, no samples, and no averaging basis, agreement verdict or spread — each recorded as not recorded".
  - State, in R2.3f or R2.3j, that restoring an imported reading copies that state and its imported-kind snapshot.

**[MAJOR] M5 — Scan-mode adoption: when it lands, whether the preview shows it, and whether commit rechecks it**

- **Where:**
  - Import R6.4 (`:166`, "hold no reading and then adopt it") and Capture R1.10 (`/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/capture-mode/prd-capture-mode.md:147`) never place the adoption inside R3.2's whole-or-nothing commit (`:77`).
  - Capture R1.10 still says the mode changes "never while DF R1.11 holds writes", and the import's own commit is such a write.
  - E43 (`/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import-copy.md:29`) never says the target's mode will change.
  - R3.8k (`:124`) rechecks the source and the session gate at commit, but not R6.4's mode check.
- **Scenarios:**
  - (a) An existing M1 collection holding only pending rows: the user selects it, previews, then cancels. If adoption happened at target selection, where R6.4's check runs, the collection is left at M2 with nothing imported, which breaks R3.8j.
  - (b) An E44 rollback after adoption.
  - (c) On a successful commit, the user's chosen M1 silently becomes M2. Every later scan's agreement check and working set move with it (Capture R1.10), and nothing in the preview said so.
- **Fix:**
  - R6.4: "the adoption lands with the commit, whole or not at all (R3.2)".
  - E43's Toolkit variant: add the line "⟨collection⟩'s scan mode changes from ⟨old⟩ to ⟨mode⟩".
  - R3.8k: recheck R6.4 at commit.
  - Capture R1.10: add "save as the import's commit (Import R6.4)".
  - UJ 3: add cancel and E44 cases after a preview that adopts the mode.

**[MAJOR] M6 — The two UJ 3 preview cases count "becoming current" differently**

- **Where:** journeys `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import-journeys.md:110` counts each new item's reading as becoming current (3 new items, 3 becoming current). Journeys `:118` counts "1 becoming current (TK-2) … 1 new (TK-3)", although R6.8a makes TK-3's reading its canonical value. R6.9 (`:171`) never says whether new items' readings count.
- **Scenario:** one of the two cases fails for every build, however it is written.
- **Fix:** R6.9 should read "readings becoming current, new items' included", and journeys `:118` should expect 2 becoming current. The opposite convention also works if it is applied consistently in both cases and in E43's copy.

**[MAJOR] M7 — No imported reading in Data Foundation's checked-in fixture list**

- **Where:** Data Foundation R7.7 is the sole fixture list for Data Foundation and Data Export (`:250`), and R7.7e (`:258`) lists only simulated and live-kind snapshots. Export R4.2 (`/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/export/prd-data-export.md:145`) checks exports against saved expected outputs (goldens) for R7.7a–n, including "all three sc_simulated values", but never `sc_imported`. EJ1's new row and DJ6 use Import's source CSV, not a checked-in store fixture.
- **Scenario:**
  - A build that writes `sc_imported` as false everywhere, or fills serial or basis with placeholders, passes every golden.
  - The upgrade tests behind R5.2/M5 never see a reading with no samples, an unknown serial and no payload. Those empty and unknown values are the fields a future schema migration is most likely to get wrong.
- **Fix:**
  - R7.7e: add "an imported-kind reading as Import R6.5 stores it, current and kept behind a scan".
  - Export R4.2: check both `sc_imported` values and R1.1t's empty cells.

**Minors**

- [MINOR] m1 — Data Foundation R5.5d (`:187`), the rule for recovering a file with two current readings, keeps "the later-recorded" one current. That contradicts F74 and R2.3j: it would make an imported B current over the scan it was kept behind. Change to "the one R2.3j would make current, else the later-recorded".
- [MINOR] m2 — Data Foundation R1.6 (`:100`) and DJ6 (`:113`, `:117`) say the missing payload is "recorded as never supplied" and "readable without the app". But a payload belongs to a sample and an imported reading has none, so the state already follows from the missing samples. R1.2 (`:99`) and R7.1 (`:239`) never list the snapshot kind, "samples not recorded" or this state among what an outside reader can read, so DJ6 adds a rule a journey may not add. Fix: add them to R1.2 and R7.1, and have DJ6 say what a reader must be able to tell rather than how it is recorded.
- [MINOR] m3 — Device R1.21 (`/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/device-management/prd-device-management.md:120`) and F33 treat "imported" as a kind of device, but it describes where the reading came from. A single three-valued kind cannot describe a future import whose device is known, such as a CxF file with a serial, or a SpectroCapture export of Demo Device readings, which must keep the simulated mark. Keep v1's rule as is, but record as an ADR-0003 input that the schema keep a reading's origin separate from its device kind.
- [MINOR] m4 — Import R6.5 (`:167`) stores the file's illuminant and observer on a spectral reading without saying they are provenance only. A builder could apply Data Foundation R3.5/R3.3e's fixed-reference handling for readings without spectral data. R6.6 (`:168`) is undefined when the file's illuminant or observer is outside the pairs the app supports (Capture F67). R6.7 (`:169`) excludes records for bad values in only three fields (Date Saved, mode, reflectances), not Illuminant, Observer or a blank Nix Device.
- [MINOR] m5 — R6.7's "cannot be read as a mode" and R6.4's mode comparison name no list of valid modes (Capture R1.10's M0/M1/M2?) and no comparison rule. R6.4's "hold no reading" does not say whether readings that are only in history, or quarantined, count.
- [MINOR] m6 — R6.8c/d (`:180–181`) tell "scanned in SpectroCapture" from "imported" only in prose. Tie the test to the current reading's snapshot kind, as Collection Mode R2.4j (`:302`) does, so a restored imported reading (which keeps its snapshot) and a Demo Device scan are each handled one defined way.
- [MINOR] m7 — R6.1 (`:163`) assumes the Toolkit's 400–700 nm grid at 10 nm steps matches v1's wavelength grid (Export OQ 4, still open) and the grid live scans report. If the hardware spike reads a different grid, one collection holds two grids and the export's fixed column set cannot carry both. Add a post-lock item for the spike.
- [MINOR] m8 — R6.2 (`:164`) stores a Note of `undefined` as an empty value. Re-importing over a Note the user has edited then clears it by default (R3.5's "Take the new details", `:80`), and Data Foundation R6.2d makes that clear unrecoverable. Treat `undefined` as not supplied, so the stored value is kept (R3.6a).
- [MINOR] m9 — R6.1 overrides only R1.5b's default delimiter. It does not say whether E43's "Choose an encoding / Choose a separator" (R3.8n, `:127`) are offered for a Toolkit export, or whether choosing a separator other than semicolon drops Toolkit recognition.
- [MINOR] m10 — R6.2 (`:164`) says what happens to every column except Custom Collection Name, which by default falls through to R2.2 as a metadata column. Say so, or exclude it.
- [MINOR] m11 — The amendment introduces facts that shape the schema: a reading with no samples, averaging basis, verdict or spread; a third snapshot kind with unknown serial and firmware; and a current reading that need not be the latest recorded. None is added to ADR-0003's inputs in `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/decisions/README.md` or to post-lock's ADR-0003 group, as the Collection Mode amendment did for its own inputs.
- [MINOR] m12 — R2.4's rule excluding every record in a duplicate-code group (`:68`, E10) applies to Toolkit files unchanged. If the Toolkit lets one Color Code be saved twice (a re-measurement), neither reading imports, although N14 would make the newer one current. Confirm the Toolkit keeps codes unique, or give §6 its own rule.

**Nits**

- [NIT] n1 — Capture's count copy "⟨captured⟩ scanned" (`/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/capture-mode/prd-capture-mode-copy.md:22`, `:25`) now counts imported items as scanned.
- [NIT] n2 — E43's Toolkit variant (`:29`) adds a second "Unchanged:" label beside the row count's; use "Unchanged readings".
- [NIT] n3 — Collection Mode now uses "imported" both for the " (imported)" column tag (copy `:268–269`) and for the new "Imported reading" mark (copy `:260`).

#### Biggest risks
1. **Permanent history pollution (B1).** Every routine Toolkit re-import after re-scanning adds readings to a history that only deleting the item can clean. No listed test catches it.
2. **A one-way door in the file format (M4, M1, m11).** ADR-0003 will be drafted from Data Foundation rows that still describe the old reading: 1–5 samples, an averaging basis, a current reading that is always the latest recorded, and a reason the app always asks for. Getting this wrong puts a migration into every user's file.
3. **Builds and tests that disagree (M2, M3, M6), and a hidden settings change (M5).** Builds will split on history order, on the supersession reason and on the preview counts. The collection's scan mode can change without the preview saying so, and the change is not tied to the commit.

#### Genuinely sound
- **No new vendor-import subsystem.** Extending the existing import flow (N4) reuses its all-or-nothing commit, Data Foundation R1.11's rule that only one write runs at a time, and the existing preview. No new component boundary.
- **One derivation behind every stored colour.** Every value is recomputed from the spectrum and the Toolkit's own values are not stored (N13).
  - The check compares against the file's Lab under the file's own illuminant and observer, while stored values use the collection's reference. Each comparison is like with like.
  - DERIVATION_VERSION and R3.3's regeneration rules apply to imported readings exactly as to scanned ones.
- **Explicit current selection already covers the new rule.** A dogmatic reviewer might say "current is no longer the latest recorded, so the schema breaks." It does not: R1.2 and R2.2 already require the current reading to be stored explicitly and readable without the app, and the history export already writes an explicit current flag. Only the two-current recovery tie-break relied on record order (m1).
- **No fake "never supplied" payload mark.** A reading with no samples has no payload to mark. R5.5a's per-sample mark and Export R1.1p's unavailable-archive count are untouched, and R1.1t counts an imported reading as neither.
- **Imported snapshots never become device records (R1.22).** The device identity (kind, model, serial) cannot collide on an unknown serial, and removing a saved device cannot touch these readings (UJ3-c).
- **The imported mark is worked out, not stored (Collection Mode R2.4j).** A re-scan clears it, restoring an imported reading shows it again, and history keeps it, with no second copy to drift out of step.
- **Match-or-refuse on scan mode.** Adopting a mode only when the target holds no reading keeps readings from different measurement modes out of one collection's colour values. It also keeps R3.3c's regeneration (OQ 17 is still open) out of the import commit.
- **At most one reading per item per commit.** R2.4 excludes every record in a duplicate-code group, so each item's reading sequence stays strictly ordered.
- **No conflict with ADR-0001.** The import never touches the `SpectroDevice` seam, as it should not; it is a CSV path in the core that CI can test without hardware. A third enum variant in the Rust core is caught by exhaustive `match` wherever it is used.
- **`sc_imported` as its own column.** Keeping it separate from `sc_simulated` preserves that column's meaning for existing consumers, and adding it before v1's export format ships needs no format-version bump.
- **Other correct calls:** reflectances above 1.0 are stored as given; samples are "not recorded" rather than assumed to be one; the test fixtures are synthetic.

#### Missing / over-engineered
**Missing:**
- The rule that an incoming reading equal to any reading the item already holds changes nothing (B1).
- Data Foundation's exception for readings with no samples, and what restoring an imported reading does (M4).
- The time axis for the current-reading rule (M1).
- An E43 line for the scan-mode change, with the change tied to the commit (M5).
- An imported reading in R7.7's fixture list (M7).
- ADR-0003 inputs from this amendment (m11).
- UJ 3 cases:
  - re-import after R6.8c's case;
  - re-import of an earlier reading;
  - Flag, then re-import;
  - restore, then re-import;
  - cancel, and E44, after a preview that adopts the mode.

**Over-engineered:** nothing material. Keeping densities as metadata columns is cheap and reversible, and E46's refusal of mixed modes is simpler than supporting a mode per record. The only avoidable complexity is carrying the reading's origin inside the three-valued device kind (m3), and that costs little in v1.

| Row ID | disposition |
| :--- | :--- |
| Import-R1.5 | ALIGN |
| Import-R3.3 | OBJECT (B1) |
| Import-R3.8 | OBJECT (M5) |
| Import-R6.1 | OBJECT (m7, m9) |
| Import-R6.2 | OBJECT (m8, m10) |
| Import-R6.3 | ALIGN |
| Import-R6.4 | OBJECT (M5, m5) |
| Import-R6.5 | OBJECT (M4, m2, m4) |
| Import-R6.6 | OBJECT (m4) |
| Import-R6.7 | OBJECT (m4, m5) |
| Import-R6.8 | OBJECT (B1, M1, m6, m12) |
| Import-R6.9 | OBJECT (B1, M6) |
| Import-R6.10 | ALIGN |
| DF-R1.6 | OBJECT (m2) |
| DF-R2.2 | ALIGN |
| DF-R2.3 (with R2.3j) | OBJECT (B1, M1, M2, M3, M4, m1) |
| Device-R1.21 | OBJECT (m3) |
| Device-R1.22 | ALIGN |
| CM-R2.4 (with R2.4j) | ALIGN |
| CM-R2.7 | ALIGN |
| CM-R4.2 (with R4.2d) | ALIGN |
| CM-R5.2 (with R5.2b/e) | ALIGN |
| CM-R8.9 | ALIGN |
| Export-R1.1 (with R1.1p/t) | OBJECT (M7) |
| Export-R1.2 | OBJECT (M7) |
| Export-E1 | ALIGN |
| Capture-R1.10 | OBJECT (M5) |

### privacy

#### Verdict
Ship after fixing Critical+High. The rows are privacy-sound: processing is local only, nothing missing from the file is invented, and provenance is marked on every surface. But the "synthetic" Toolkit fixture carries real values from the owner's export, and the review log quotes two more, both against N3 and F71. Commit 417fdac is not on the remote yet (`git ls-remote` shows no `docs/nix-toolkit-import` branch), so the values can still be kept out of public history.

#### Data-flow & PII map (brief)
- **Data subject.** The Cataloger (here, the owner). The design touches no third party. The repository's public readers are the recipients in the fixture and log leak.
- **Personal fields in a Toolkit record.** Date Saved (an activity timestamp, to the millisecond, UTC), Note (free text), Custom Collection Name (a user label), and Nix Device (a model name; whether a user can rename a device is unverified). Color Name and Color Code identify products, not people. Spectra, densities, illuminant, observer and mode are not personal data.
- **Product flow.** The user picks a CSV and it is read locally (R6.1). It lands only in the user's SQLite file, and DF R1.4 lets nothing leave.
  - The reading: Date Saved becomes the measurement time; the model becomes an imported-kind snapshot with serial and firmware unknown.
  - Metadata columns: Note (`undefined` stored as empty) and the five Density columns. What happens to Custom Collection Name is not stated (PRIV-3).
  - It then shows on local Collection Mode surfaces, and leaves only in an export the user starts: measured-at, model, `sc_imported` and the passthrough columns, with serial, firmware, basis and payload cells empty.
  - The amendment adds no network, telemetry, sync or third-party flow.
- **Process flow.** Values from the real export reached the drafting agent and then committed text (PRIV-1, PRIV-2). The research report, a contract source in this brief, would reach any cross-model reviewer (PRIV-7).
- This review quotes no value from the real export. Where the diff quotes one, I cite its location only, so the review can be filed in the public log without repeating the leak. I checked against the real export with count-only greps and printed none of its lines.

#### Findings

**[HIGH] PRIV-1 — Fixture T and DJ6 carry real values from the owner's export; the "synthetic" control is illusory**

Where:
- `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import-journeys.md:104` — Fixture T: the Date Saved values of TK-1, TK-2 and TK-3, and TK-3's Color Name.
- `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import-journeys.md:111` — TK-1's Date Saved as the SQL oracle.
- `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/data-foundation/prd-data-foundation-journeys.md:113`, `:114`, `:115` — the same three Date Saved values as DJ6's oracles.
- Inherited through Fixture T by Device UJ3-c (`/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/device-management/prd-device-management-journeys.md:80`), and by any Data Foundation R7.7 fixture or Export golden later built from Fixture T.

Evidence:
- Each of the three Date Saved values occurs exactly once in the real export, identical to the millisecond.
- TK-3's Color Name occurs once, on the same real record as TK-3's Date Saved, so TK-3 reproduces two fields of one real record.
- The invented values really are invented: TK-1's and TK-2's names, `Fixture Set` and TK-3's `1.047` reflectance do not occur in the file.
- None of the leaked values is in the research report, which gives only the calendar days of the two sessions. They were copied from the export itself, which was therefore in the drafting agent's context.

Harm:
- The owner's scanning timestamps are activity data about an identifiable person (the repo's author). They show a two-session pattern and times of day, and they would sit in a public open-source repo.
- N3 chose "Keep it out of the repo — used only locally for research" and rejected even "Commit with dates shifted". These dates are not shifted.
- R6.10, F71, UJ 3's heading and DJ6's preamble all state "synthetic … never a real export" over real values.
- Once Fixture T becomes a checked-in CSV, the values copy into the Data Foundation fixture and the export golden's measured-at column.
- The data is not very sensitive in itself. The severity comes from the owner's explicit refusal, the false "synthetic" claim, and the fact that it cannot be undone after a push: a PR's commits stay visible on GitHub even after a squash-merge.

Check: a local pre-push script reads the real export and fails if any non-constant field value (dates, names, codes, notes, collection name, numeric cells) appears in `git diff 6374538..HEAD`. It would fail on the five lines above today and pass after the fix. It must never be committed, because it reads the real file.

Remediation:
1. Replace the three Date Saved values and TK-3's name with plainly invented ones. Keep the orderings that UJ 3's re-import case (`:120`) and DJ6 rows 2–3 rely on, or change those values together.
2. Rewrite 417fdac (amend or fixup) before the first push. A follow-up commit is not enough, because it leaves the values in history.
3. Run the check above locally.
4. Future drafting dispatches get a format-only report, never the export.

(GDPR Art. 5(1)(b) and (c) purpose limitation and minimisation, Art. 25 privacy by design; the owner's N3.)

**[MEDIUM] PRIV-2 — The review log quotes the real export's collection name and reading count**

Where: `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md:18` (N5's count) and `:32` (N15's quoted collection name).

Harm:
- Round 0 is labelled "verbatim", so the owner's questions carry data values from the file into a public log with no need. The brief's rule forbids exactly this ("never its data values — codes, names, dates, counts").
- Together the two values show which product line and set size the owner owns. That is low harm, but it is personal data in a public artifact.
- N8 at `:23` ("yours is M2") names a measurement setting that the synthetic fixture uses anyway. It can stay.

Remediation: redact in place with bracketed placeholders ("[the file's collection name]", "[the file's reading count]"), and add one sentence to Round 0 saying values from the real export are redacted under N3. That keeps the record honest. Land it in the same pre-push rewrite as PRIV-1.

**[MEDIUM] PRIV-3 — R6.2 never says what happens to Custom Collection Name, so it can land on every item and every export row**

Where: `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import.md:164` (R6.2).

The path:
1. R6.2 disposes of every other Toolkit column, but not Custom Collection Name.
2. §6's preamble (`:159`) keeps every §1–§3 rule that §6 does not replace, so R2.2 (`:66`, "Import remaining columns as metadata") stores the name as an imported column on every item.
3. That includes an existing target, where R6.3 (`:165`), F83 and N15 say the name is ignored.
4. The Data Export obligation (`:148`) then carries the column into every exported row.

Testability gap: UJ 3's metadata case (`/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import-journeys.md:112`) lists the columns that must not be stored but leaves Custom Collection Name out. A build that stores it and a build that does not both pass every case, so a builder guesses.

Harm: the user's own collection label, which is sometimes personal, gets copied per item and exported, beyond its one decided purpose (pre-filling the name).

Remediation:
- R6.2 adds "Custom Collection Name is read for R6.3 and not stored".
- UJ 3 `:112` adds it to the list of columns no stored column may hold.
- Optionally, R6.2 says a column it does not name (from a later Toolkit version) imports as metadata under R2.2 and is listed in E43's added columns, so any new personal field is visible before the commit.

(Art. 5(1)(b) and (c).)

**[MEDIUM] PRIV-4 — R6.10's "synthetic" is undefined, points at research outside the repo, and does not cover the derived fixture and golden**

Where: `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import.md:172` (R6.10), carrying F71 (`/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import-fences.md:169`).

Three problems:
- **(a) The format reference points outside the repo.** "Built to the format the research records" refers to a session-scratchpad report. A builder or test reviewer cannot open it. A contributor who restores the reference by committing the report would publish the real export's local path (which includes the owner's username), its collection name, codes, dates and three rows of measured values.
- **(b) "Synthetic" is not defined.** That is how PRIV-1 passed. The Collection Mode Harness already defines the word (`/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/collection-mode/prd-collection-mode-journeys.md:24`: "invented collections, codes, names, serials and dates").
- **(c) The scope stops at Toolkit exports.** It does not reach the checked-in SQLite fixture and export golden an imported reading needs. Data Foundation R7.7 is the sole fixture inventory (`/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/data-foundation/prd-data-foundation.md:250`), Export R4.1 checks goldens in, and EJ1's new row (`/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/export/prd-data-export-journeys.md:14`) needs one.

Remediation:
- R6.10 reads: "…on Toolkit fixtures built to UJ 3's stated format whose every value is invented — collection names, codes, names, notes and dates — as the Collection Mode Harness defines synthetic. Every checked-in fixture or golden holding an imported reading derives from one. Neither a real export nor a report on one enters the repository."
- UJ 3's fixture paragraph (`:104`) says "invented" explicitly.
- This makes F71 buildable within its own decision; it does not re-open it.

**[LOW] PRIV-5 — A Note of `undefined` stored as empty lets a later import clear a note the user typed**

Where: `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import.md:164` (R6.2) with `:170` (R6.8: metadata follows R3.5/R3.6 unchanged).

The path:
1. R6.2 stores a Note of exactly `undefined` as an empty value that counts as present.
2. The user later types a note into that column.
3. On the next Toolkit import, R3.6c (`:101`) replaces the typed note with empty: at once for a pending match, and under E14's default "Take the new details" (R3.5, `:80`) for a captured one.
4. Data Foundation R2.3's edit rule then leaves no prior text, so the note cannot be recovered.

The file never supplied a note, yet the app manufactures a value that erases the user's own.

Remediation: a Note of exactly `undefined` supplies no value. A new item's Note is empty; a matched item keeps its stored Note, as R3.6a does for an omitted column. Add a UJ 3 case: edit TK-1's Note, re-import, and the note is kept. (Art. 5(1)(d) accuracy; user control.) Not blocking.

**[LOW] PRIV-6 — E43's Toolkit variant does not say that imported readings are permanent**

Where: `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import-copy.md:29` (E43 Toolkit variant) and R6.9 (`prd-inventory-import.md:171`).

Harm:
- This is the first import that writes measurements that cannot later be changed, in bulk.
- Readings kept in history or made current can never be removed short of deleting the swatch (Data Foundation R6.1 at `:217` and R2.3 at `:115`; N6 and N14 are settled).
- A wrong file therefore permanently embeds another export's readings and Date Saved times in the user's item histories.
- The preview gives counts but no warning.

Remediation: E43's Toolkit variant appends "Imported readings stay in each swatch's history; only deleting the swatch removes them." Not blocking.

**[LOW] PRIV-7 — A cross-model review would send the research report to a third-party model provider (conditional)**

This brief names the scratchpad research report as a contract source. The report carries the real export's local path, its collection name and count, its colour codes, its date range and three rows of measured values. If this round's brief also goes to an agy or codex runtime, those values cross to a third-party model provider.

Remediation: send cross-model reviewers a format-only extract, or first record the owner's consent in the review log, as the Collection Mode log did.

**[INFO] PRIV-8 — Nix Device is stored as the model, but whether a user can rename it is unverified**

Where: `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import.md:167` (R6.5).

Nix Device becomes the snapshot's model and goes out on every export row. The research saw one constant value. If the Toolkit lets a user rename a device and exports that name here, a personal label would travel as "model". Confirm on the hardware spike. If the name can be user-assigned, store the model only when it is a v1-family model name, and otherwise record the model as unknown.

**[INFO] PRIV-9 — Two gaps outside my lens, for the owning reviewers**

- **(a) History order contradicts DJ6.** Data Foundation R2.1 (`/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/data-foundation/prd-data-foundation.md:112`) orders history by record time, and Collection Mode R5.3 (`/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/collection-mode/prd-collection-mode.md:385`) lists E17 newest-recorded first. But DJ6 row 2 (`prd-data-foundation-journeys.md:114`) and F64 (`prd-data-foundation-fences.md:741`) place an imported reading in history by its measurement time. A correct R2.1 build fails DJ6 row 2. N10 is settled, so R2.1 and R5.3 need the mirror, or DJ6 should say "over-time views" only.
- **(b) No imported-kind fixture.** Data Foundation R7.7's matrix gains no fixture holding an imported-kind snapshot, though EJ1's new row and Export R4.1's goldens need one; R7.7e covers only the simulated and live kinds.

Nit: `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/README.md:146` adds the imported kind but omits Device R1.22's "an imported snapshot is never a device record and occupies none".

#### Biggest privacy risks
1. **PRIV-1:** real Date Saved values and one real record's name in a fixture labelled synthetic, feeding DJ6 and every derived fixture or golden. Fix it by rewriting the commit before the first push.
2. **PRIV-2:** the real collection name and reading count in a public review log. Same rewrite.
3. **PRIV-3:** the collection label possibly copied onto every item and every export row, against N15's decision to ignore it.

#### Genuinely privacy-respecting
- **The real export and the report stay out of the repo.** The diff touches only Markdown: no CSV, no local path, no brand or username string. The report's three sample rows and its colour codes appear nowhere in `docs/`.
- **Processing is local only.** No network, telemetry or sync is added. DF R1.4 and CM R8.6 still bind: no outbound attempt carries an item, reading or payload, and no collection content goes to system search.
- **Nothing is invented.** Serial and firmware are unknown, samples are not recorded, and there is no payload, recorded as never supplied rather than as damage (R6.5, DF R2.3j and R1.6, Export R1.1t, and E1's disclosure). A shallow checklist might flag "model stored, serial empty" as incomplete; it is the honest, minimal choice.
- **No false linkage.** Under Device R1.22 an imported snapshot never occupies a device record, so imported readings are never attributed to the user's own paired instrument serial, a persistent identifier. UJ3-c tests this.
- **Minimisation.** The file's own Lab, LCh, XYZ, sRGB and HEX values and its Index are read for the check and not stored (R6.2).
- **Purpose limitation for the name.** Custom Collection Name only pre-fills an editable name and is ignored for an existing target (R6.3), subject to PRIV-3.
- **Transparency.** The imported mark shows on the chip, in Filters, to VoiceOver, in the detail and in history (CM R2.4j, R4.2d, R5.2b and e). Export carries `sc_imported` and E1's count.
- **Idempotent re-import.** Importing the same file again adds nothing (R3.3, R6.8d, R6.9), so readings do not pile up.
- **The privacy inventory stays closed.** DF R6.5 gains no new category of personal data: Date Saved is a measurement time like any reading's, and Note and the densities are imported columns.
- **Densities are not over-collection.** They are kept with no v1 use, but they are not personal data, and N13 settled them.
- **Exports are not disclosures.** Date Saved and Note leave only in an export the user starts, to a place the user chooses.

#### Missing controls / over-collection
- There is no repeatable guard keeping real-export values out of committed text: no local pre-push value check, and no rule that drafting agents get format-only input (PRIV-1).
- R6.10 does not define "synthetic", points outside the repo, and does not cover the derived fixture and golden (PRIV-4).
- Custom Collection Name has no stated disposition, and neither does a column R6.2 does not name (PRIV-3).
- A `undefined` Note is treated as a supplied value (PRIV-5).
- E43 does not disclose that imported readings are permanent (PRIV-6).
- No owner consent is recorded for sending the report to a cross-model reviewer (PRIV-7).

| Row ID | disposition |
|---|---|
| Import-R1.5 | ABSTAIN |
| Import-R3.3 | ALIGN |
| Import-R3.8 | ABSTAIN |
| Import-R6.1 | ABSTAIN |
| Import-R6.2 | OBJECT (PRIV-3, PRIV-5) |
| Import-R6.3 | ALIGN |
| Import-R6.4 | ABSTAIN |
| Import-R6.5 | ALIGN |
| Import-R6.6 | ABSTAIN |
| Import-R6.7 | ABSTAIN |
| Import-R6.8 | ALIGN |
| Import-R6.9 | ALIGN |
| Import-R6.10 | OBJECT (PRIV-1, PRIV-4) |
| DF-R1.6 | ALIGN |
| DF-R2.2 | ABSTAIN |
| DF-R2.3 (with R2.3j) | OBJECT (PRIV-1; DJ6's values only, the row text is sound) |
| Device-R1.21 | ALIGN |
| Device-R1.22 | ALIGN |
| CM-R2.4 (with R2.4j) | ALIGN |
| CM-R2.7 | ABSTAIN |
| CM-R4.2 (with R4.2d) | ALIGN |
| CM-R5.2 (with R5.2b/e) | ALIGN |
| CM-R8.9 | ABSTAIN |
| Export-R1.1 (with R1.1p/t) | ALIGN |
| Export-R1.2 | ALIGN |
| Export-E1 | ALIGN |
| Capture-R1.10 | ABSTAIN |

### product-marketing

#### Verdict
Lands after fixing Blockers. The voice is honest and fits the product: nothing is invented, unknowns are named as unknown, and the vendor is treated neutrally. But E14 now makes one shipping promise that a Toolkit import breaks, and several copy states the amendment needs are missing (E12's explanation of the new mark, the fixed-mapping step, the scan-mode change).

#### Audience & message context (brief)
- **Reader:** the Cataloger. Most often this is a Nix Toolkit phone-app user moving an existing collection into SpectroCapture for the first time. The other readers are the Data consumer (who reads Export E1) and the vision's readers (contributors, and the future README and launch copy).
- **Intended takeaway:** "My Toolkit colours come across already measured, clearly marked as imported, and nothing about them is made up."
- **Surfaces:**
  - Import E43 (Toolkit variant), E46 and E47.
  - Collection Mode's imported mark (chip, filter and VoiceOver labels), the R4.2d imported line, and R5.2b/R5.2e.
  - Export E1's imported line.
  - Vision J7 and the feature list.
- **Checked against:** the diff 6374538..417fdac, fences F69–F83 and their mirrors, owner decisions N1–N15, and the format report. I quote only format facts from the report.

#### Findings

**[BLOCKER] PMM1-B1 — Import E14 body, `docs/product/import/prd-inventory-import-copy.md:24`: "Your measurements aren't touched either way."**
- **Reader reaction:** the Cataloger reads this as "this import won't change my readings." Under a Toolkit import that is false, and it contradicts E43's "Becoming current: ⟨n⟩" on the same screen.
- **Why it now renders:** R6.8 (`prd-inventory-import.md:170`) routes metadata "through R3.5/R3.6 unchanged", so E14 appears in Toolkit imports. UJ 3 already does this at journeys:118 ("keep E14's default").
- **Where it is false:**
  - R6.8d (`:181`): a later Toolkit reading replaces the item's current value.
  - R6.8c (`:180`): a new reading is added to the item's history.
- **Failing scenario (mutation):**
  1. Take UJ 3 with an existing M2 target whose TK-3 current reading was imported.
  2. Give TK-3 a later Date Saved, different reflectances and a changed Color Name.
  3. E14 renders with its promise, then the commit makes TK-3's new reading current (DF R2.3j).
  4. A test that checks E14's promise against the store after commit fails.
- **Why this copy file:** its own rule (line 8) says "Every promise below is backed by a requirement row." R3.5 backs only "the choice doesn't touch measurements"; R6.8 now does.
- **Rewrite:** add an E14 Toolkit variant that replaces the last sentence with: "Either way, this choice changes only details. This file's readings are counted under Nix Toolkit export below; a swatch you scanned in SpectroCapture keeps that scan as its colour." Have R6.8 name that variant.

**[MAJOR] PMM1-M1 — Collection Mode E12 body, `prd-collection-mode-copy.md:173`, has no explanation of the imported mark.**
- R2.8 (`prd-collection-mode.md:309`) says E12 shows each mark's "chip label and what it means". UJ2.1-k (journeys:229) now expects twelve marks. The amendment added the Mark labels table row (copy:260) but not E12's explanation sentence.
- **Reader reaction:** "Colour marks" is the one place a Cataloger learns what a mark means, and "Imported reading" gets no explanation there. A builder would have to write the sentence and pick its framing, including whether it implies the colour is less trustworthy.
- **Rewrite** (insert after "Samples disagreed: …"): "Imported reading: the reading came from a Nix Toolkit export, not a scan in SpectroCapture. Its colour is worked out from the spectrum in that export, as a scan's is; the instrument's serial and firmware and how many samples it took weren't recorded, so Spread shows nothing. Scanning the swatch here replaces it, and the imported reading stays in its history."

**[MAJOR] PMM1-M2 — The imported detail and history lines drop fields that R4.2d, R5.2e and F221 require.**
- **Mismatch:**
  - R4.2d (`prd-collection-mode.md:348`) and F221 say to show "serial, firmware, samples, basis, spread and verdict as unknown or not recorded".
  - The R4.2d imported copy line (copy:295) shows only serial unknown, firmware unknown and samples not recorded. Basis, spread and verdict are silently missing.
  - UJ2.1-r (journeys:236) requires the Reading line to be the copy line exactly.
  - Result: a build that follows the row fails the case, and a build that follows the copy breaks the row.
- R5.2e (`:381`, "neither was recorded") vs copy:310 ("Samples not recorded") has the same gap.
- **Rewrite, R4.2d imported:** "Measured, then the date · Nix Toolkit export, model, serial unknown, firmware unknown · samples, averaging, spread and agreement not recorded".
- **Rewrite, R5.2e:** "the chip, and Samples and spread not recorded".

**[MAJOR] PMM1-M3 — E46 has one headline for both variants, and it is wrong for the mixed-modes variant (`prd-inventory-import-copy.md:32`).**
- The headline is "This file was measured in ⟨file mode⟩". In the mixed-modes variant (UJ 3 journeys:115, M1 and M2) there is no single ⟨file mode⟩. The builder either leaves it undefined or shows one mode above a body saying "This file mixes measurement modes (M1, M2)", which contradicts it.
- The existing headline also states a fact instead of the problem, unlike E5, E7 and E40.
- **Rewrite** (a headline per variant):
  - Collection: "⟨collection⟩ uses a different measurement mode"
  - Mixed modes: "This file mixes measurement modes"

**[MAJOR] PMM1-M4 — The E43 Toolkit variant uses "Unchanged:" twice and puts the Toolkit news last (copy:29).**
- The base body already says "Unchanged: ⟨unchanged⟩" (rows). The variant adds a second "Unchanged: ⟨unchanged readings⟩".
- **Reader reaction:** a re-export with renamed colours shows "Updated: 3 … Unchanged: 3". For a 3-row file that reads as 6 rows, or as a contradiction.
- The fact this reader cares about most, "Nix Toolkit export — N readings", comes after every row count and list. N5 intended the preview to lead with that confirmation.
- **Rewrite:** render the Toolkit line first after Target, then:
  - "Used as the swatch's colour: ⟨current⟩."
  - "Kept as earlier readings: ⟨history⟩ — a swatch you scanned here keeps that scan."
  - "Readings already here: ⟨unchanged readings⟩."
- Say how it orders against the above-ceiling append.

**[MAJOR] PMM1-M5 — The scan-mode change on an existing collection is never shown to the user (R6.4 `:166`; UJ 3 journeys:117).**
- An M1 collection holding only pending items silently becomes M2. Every later scan's agreement check and working colour then change mode.
- Neither E43 nor R6.9 (`:171`) discloses it. The Cataloger confirms an import without being told their collection's setting will change.
- **Rewrite:** add to E43's Toolkit variant: "⟨collection⟩ will use ⟨mode⟩ from now on, to match these readings; it used ⟨collection mode⟩." Add that disclosure to R6.9's list.

**[MAJOR] PMM1-M6 — The fixed-mapping step has no copy, and nothing says how each later step names the Toolkit export (R6.1 `:163`, R6.2 `:164`; UJ 3 journeys:108).**
- R6.1 says "every later step names it a Toolkit export". R6.2 says the mapping is "fixed and shown", with the file's L, a, b, …, HEX "not stored". No copy exists for either.
- **Reader reaction:** a locked mapping screen with no reason given, apparently discarding the Toolkit's Lab and HEX. A migrating user will think data is being lost. The builder writes that reassurance, or leaves it out.
- **Rewrite:** add a state, for example E48 "This is a Nix Toolkit export":
  - Body: "The columns are set for you: Color Code is each swatch's code and Color Name its name, and each row's spectrum, date and measurement settings become its reading. Note and the Density columns come in as details. The file's own Lab, LCh, XYZ, sRGB and HEX are checked, not kept — SpectroCapture works every colour out again from the spectrum."
  - Actions: "Continue"; "Cancel".

**[MAJOR] PMM1-M7 — The two UJ 3 cases disagree on what "Becoming current" counts (journeys:110 vs :118).**
- Case :110 counts new swatches' readings (3 new → "3 becoming current").
- Case :118 does not: TK-3 is new (R6.8a), but the case says "1 becoming current (TK-2), 1 kept in history (TK-1) and 1 new (TK-3)".
- **Reader reaction:** "3 readings … Becoming current: 1. Kept in history: 1." leaves the third reading unaccounted for. A builder must pick one rule, and whichever it picks fails one case.
- **Fix:**
  - Define ⟨current⟩ as every reading that becomes a swatch's colour, new swatches included.
  - Change :118 to "2 becoming current (TK-2, TK-3)".
  - Add to the E43 footnote: "⟨current⟩ + ⟨history⟩ + ⟨unchanged readings⟩ = ⟨readings⟩".

**[MAJOR] PMM1-M8 — The vision's Problems bullet (`vision.md:13`) now contradicts the J7 update (`:145`).**
- `:13` says "partial, lossy export … Owners can't get full-fidelity spectral data into their own hands as a queryable, portable dataset."
- `:145` says the Toolkit exports each colour's spectrum as CSV. The report found that spectrum reproduces the file's Lab within about 0.01 ΔE2000, and it is portable.
- **Reader reaction:** a contributor or prospective user sees the founding problem overclaimed by the vision's own evidence.
- **Rewrite what is still true:** "Measurements live inside vendor, account-bound apps; the phone app's CSV carries each colour's spectrum but not the instrument's serial, firmware or sample count, and the open colour tools people trust can't connect to the device. Owners have no queryable, portable home for a whole collection and its history."
- If spectral export turns out to be a paid Toolkit tier, say that instead; it would make the claim sharper, not weaker.

**[MINOR] PMM1-m1 — The chip label "Imported reading" (CM copy:260) leaves out the source.**
- N7's own words and the filter and VoiceOver labels all say "imported from Nix Toolkit".
- "Imported" also collides with the existing " (imported)" column tag and "the details you imported", which mean CSV details, not readings.
- Suggest the chip label "From Nix Toolkit".

**[MINOR] PMM1-m2 — "Measurement mode" (E46, E43) vs "measurement condition" (CM E9, E12, R5.4; Export E1).**
- These are two names for the same thing, and "M2" is never explained.
- Pick one product-wide term. If Import keeps the Toolkit's word, bridge it once, for example: "M2 (the Toolkit's Measurement Mode — the measurement condition this collection is set to)".
- E46's "can't be compared" should read "aren't directly comparable".

**[MINOR] PMM1-m3 — E46's mixed-modes advice, "Export each mode from the Toolkit separately", claims a Toolkit feature nobody has verified.**
- The research saw one file with one constant mode. Verify this on the Toolkit, or give recovery advice that doesn't depend on it.

**[MINOR] PMM1-m4 — E47's "fix the export and try again" (copy:33) invites hand-editing.**
- A spreadsheet re-save typically changes the delimiter and quoting. The file then no longer passes R6.1's check and imports as a plain CSV with no readings.
- Suggest "or export it again from the Toolkit and try again."

**[MINOR] PMM1-m5 — Export E1's imported line (`prd-data-export-copy.md:16`) leaves out the empty averaging basis.**
- R1.1t empties the averaging basis too. A data consumer seeing empty basis cells may read them as damage.
- Rewrite: "⟨imported⟩ readings came from a Nix Toolkit export; their instrument serial, firmware and sample count weren't recorded, so those cells stay empty."

**[MINOR] PMM1-m6 — J7 contradicts itself right above its update.**
- The J7 risk point (`vision.md:144`, "ships with CxF support (v2), not v1") and heading ("v2 candidate") sit directly above the v1 update.
- The PRD index's v2 backlog (`docs/product/README.md:174`) still lists "the vendor-app migration path (J7)" as v2 in full.
- Add "(in part superseded — see the update)" and scope the backlog line to CxF and the other vendor apps.

**[MINOR] PMM1-m7 — The vision's feature note (`:160`) and J7 update describe how it works, not what the Cataloger gains, and generalise from one export.**
- Suggest: "Bring a Toolkit collection over without re-scanning it; each colour arrives measured and marked imported."
- Scope the claim: "a Toolkit export that carries spectra, as the one studied does."

**[MINOR] PMM1-m8 — The root `README.md` will be wrong once this ships.**
- Its "How it will work" section (line 28 onward) never mentions the Toolkit import.
- "Everything — the raw instrument payload…" (line 30) is untrue for imported readings, which have none.
- Add a post-lock documentation item.

**[MINOR] PMM1-m9 — New placeholders are not registered anywhere.**
- ⟨readings⟩, ⟨mode⟩, ⟨current⟩, ⟨history⟩, ⟨unchanged readings⟩, ⟨flagged⟩, ⟨file mode⟩, ⟨collection mode⟩ and ⟨modes⟩ are missing from Capture §12's Placeholders list and from the import copy footnote.
- ⟨unchanged readings⟩ contains a space, which breaks the token convention.

**[MINOR] PMM1-m10 — The "synthetic" fixture claim may not hold (outside my lens; for the privacy reviewer to verify).**
- Fixture T's dates (journeys:106) fall on the same two dates as the real export's two sessions, with times to the millisecond.
- The Round 0 log also quotes the real collection name and row count.
- If any of those values were copied from the real file, R6.10's "synthetic" claim overstates. Shift them.

**[NIT] PMM1-n1** — "save" meaning "except" (line 5, R1.5b, R3.3, DF R2.2, DF/Export out-of-scope, the Capture obligation) collides with the product's verb "save". Use "except".

**[NIT] PMM1-n2** — E43's "⟨flagged⟩ readings differ from the Toolkit's own values": it is the worked-out values that differ. Suggest "⟨flagged⟩ readings' colour values differ from the ones the Toolkit saved".

**[NIT] PMM1-n3** — E40's target reason, "new swatches can't join its queue mid-run", doesn't fit Toolkit rows, which arrive already measured.

**[NIT] PMM1-n4** — "Becoming current" is internal wording for a first-time migrator (covered by the M4 rewrite).

#### Biggest risks
- **E14's false reassurance (B1).** It is the one place the copy tells the Cataloger something untrue about their measurements, on the same screen that shows the opposite.
- **Silent surprises for the migrator:**
  - the collection's scan mode changes without notice (M5);
  - a locked mapping appears to throw away the Toolkit's Lab and HEX (M6);
  - reading counts that don't add up (M4, M7).
- **The "Imported reading" mark has no explanation (M1).** Left to the builder, it could be framed as a warning, which would undersell a reading the research shows is measurement-grade.
- **The positioning contradiction (M8).** The founding "data is stranded" problem now clashes with the vision's own finding that the Toolkit exports full spectra. That will spread into README and launch copy.

#### Genuinely strong
- **Honesty discipline, applied to provenance.** "Serial unknown, firmware unknown, samples not recorded", with "never supplied" kept distinct from damage. Nothing is invented. The product's "no silent closest colour" value extends naturally to "no invented details about where a reading came from".
- **Recompute-and-check stays neutral toward the vendor.** Flagged rows "differ from the Toolkit's own values" with no suggestion that the Toolkit is wrong. "Nix Toolkit" and "Nix Spectro 2" are used only to identify the source, which is proper use of the names and does not disparage the vendor.
- **E46 and E47 reuse established patterns.** They follow the "Bring in the rest, or fix … and try again" and "Pick the file again" shapes and labels; R3.8b, c and p quote them exactly.
- **Export E1's imported line mirrors the simulated line** and registers ⟨imported⟩ properly.
- **The vision update is appropriately plain and dated.** It keeps CxF in v2 and doesn't hype. The migration story underneath it is strong positioning: it lowers the cost of switching from the incumbent phone app and makes "never locked to a vendor account" demonstrable.

#### Missing / over-hyped
- **Missing:**
  - E12's explanation sentence (M1).
  - A copy state for the fixed mapping, and the "Nix Toolkit export" tag the other steps should show (M6).
  - The mode-change disclosure (M5).
  - A near-miss state: a file that looks like a Toolkit export but lacks the R400–R700 columns currently falls through R6.1 silently and imports with no readings. The copy should say so.
  - A one-line E43 note on what the file doesn't carry (serial, firmware, sample count).
  - A post-lock documentation item for the README story and the scoped "raw payload" claim (m8).
  - Registration of the new placeholders (m9).
  - Behaviour of "Choose a separator" on a recognised Toolkit export is unclear (interface lens).
- **Over-hyped:** nothing is hyped in tone. The only overreach is claim scope:
  - the vision generalises "the Nix Toolkit exports … each color's spectrum" from one file (m7);
  - the Problems bullet overclaims, as M8 describes.

| Row ID | disposition |
|---|---|
| Import-R1.5 | ABSTAIN (out of lens) |
| Import-R3.3 | ALIGN |
| Import-R3.8 | ALIGN |
| Import-R6.1 | OBJECT (PMM1-M6) |
| Import-R6.2 | OBJECT (PMM1-M6) |
| Import-R6.3 | ALIGN |
| Import-R6.4 | OBJECT (PMM1-M3, PMM1-M5) |
| Import-R6.5 | ABSTAIN (out of lens) |
| Import-R6.6 | ALIGN |
| Import-R6.7 | ALIGN |
| Import-R6.8 | OBJECT (PMM1-B1) |
| Import-R6.9 | OBJECT (PMM1-M4, PMM1-M5, PMM1-M7) |
| Import-R6.10 | ABSTAIN (out of lens) |
| DF-R1.6 | ABSTAIN (out of lens) |
| DF-R2.2 | ABSTAIN (out of lens) |
| DF-R2.3 (with R2.3j) | ABSTAIN (out of lens) |
| Device-R1.21 | ABSTAIN (out of lens) |
| Device-R1.22 | ABSTAIN (out of lens) |
| CM-R2.4 (with R2.4j) | OBJECT (PMM1-M1) |
| CM-R2.7 | ALIGN |
| CM-R4.2 (with R4.2d) | OBJECT (PMM1-M2) |
| CM-R5.2 (with R5.2b/e) | OBJECT (PMM1-M2) |
| CM-R8.9 | ALIGN |
| Export-R1.1 (with R1.1p/t) | ALIGN |
| Export-R1.2 | ALIGN |
| Export-E1 | ALIGN |
| Capture-R1.10 | ALIGN |

Minor and Nit findings are listed above but not raised as objections: m1 (CM-R2.4), m2/m3 (Import-R6.4), m4 (Import-R6.7), m5 (Export-E1), m9 (Import-R6.9), m10 (Import-R6.10), n1 (Import-R1.5, R3.3). M8, m6, m7 and m8 fall on the vision and README, which have no rows.

### plan

#### Verdict
Execute after fixing Blockers. One acceptance case contradicts another, so an unattended build stops at UJ 3 outright. The nine Majors belong in the same fix pass: each one either lets a wrong build pass every case or leaves the builder guessing.

#### Findings

[BLOCKER] B1: Import UJ 3, R6.9 counts, `docs/product/import/prd-inventory-import-journeys.md:118` vs `:110`
- **Problem:** The two cases count the same thing two different ways.
  - `:110`: the three R6.8a new items count as "3 becoming current".
  - `:118`: new item TK-3 counts only as "1 new", and "1 becoming current (TK-2)" is the whole current tally.
  - R6.8a makes a new item's reading its canonical value, so by `:110`'s rule `:118` must say 2 becoming current (TK-2 and TK-3).
- **Mutation:** A build that counts R6.8a readings in ⟨current⟩ passes `:110` and fails `:118`. A build that leaves them out passes `:118` and fails `:110`. No correct build passes both, so an unattended agent either stalls or bends behaviour to one case.
- **Fix:**
  - Change `:118` to: "E43 counts 2 becoming current (TK-2, TK-3), 1 kept in history (TK-1); New 1 (TK-3), Updated 1 (TK-1)".
  - Add one clause to R6.9 (`prd-inventory-import.md:171`): a new item's reading counts as becoming current.

[MAJOR] M1: Import R6.8c/d, R3.3, R6.9, reading identity on re-import (`prd-inventory-import.md:180–181`, `:78`, `:171`)
- **Problem:** R6.8d's "same reading" test compares only against the *current* value, and R6.8c has no "already held" clause at all. Three scenarios break:
  - (a) Re-importing a file after an R6.8c commit: read literally, R6.8c appends the same reading to history again. R6.9's "changes nothing" says otherwise, but which E43 tally it falls in (kept in history or unchanged) is undefined.
  - (b) Import July file A, then September file B (A goes to history), then re-import A. A's Date Saved is earlier than current B, so "an earlier one goes to history" duplicates A. This is not "repeating a committed export with no intervening edits", so R6.9 does not catch it.
  - (c) Same Date Saved but different reflectances is neither same, later nor earlier, so it is undefined.
- **Why it matters:** DF R2.3 makes a reading removable only by deleting its item, so a duplicate is permanent. No UJ 3 or DJ6 case covers (a)–(c), so a duplicating build passes every case.
- **Fix:**
  - Define "the same reading" as equal in Date Saved and every reflectance (numeric equality) to *any* reading the item holds, current or history. Such a record changes nothing and counts as unchanged.
  - Get an owner decision on (c).
  - Add UJ 3 cases for (a) and (b), each asserting the item's reading count does not change.

[MAJOR] M2: Import R6.7, R6.4, R6.10 and Fixture T, accepted value forms (`prd-inventory-import.md:169`, `:166`; `prd-inventory-import-journeys.md:104`)
- **Problem:** R6.7 says "cannot be read as a time, a mode or a number" but never states the accepted forms. The research records:
  - every Lab/LCh/XYZ/reflectance cell in fixed scientific notation (`d.dddddddde±d`);
  - Date Saved as ISO 8601 UTC with milliseconds and `Z`.
- **Why a wrong build passes:** Fixture T declares only "a 31-value reflectance list" plus a plain-decimal literal `1.047`. A parser that rejects `e` notation passes every UJ 3 case, then excludes every record of a real export (E47, then E42).
- **Also undefined:**
  - Which mode strings count as "a mode". Capture R1.10 (`prd-capture-mode.md:147`) names only M0, M1 and M2; M3 exists in the M-condition family; `m2` raises whether R2.3 comparison applies.
  - Whether R6.7's exclusion runs before R6.4's mixed-modes refusal.
- **Fix:**
  - R6.7 states the forms: decimal or scientific notation with `.`; RFC 3339 UTC with optional fractional seconds; mode ∈ {M0, M1, M2} (exact or under R2.3), and what M3 does.
  - R6.7 runs before R6.4's mixed check, which then applies to the remaining records.
  - Fixture T writes its numeric cells in the research's scientific form (TK-3's as `1.04700000e+00`).

[MAJOR] M3: Fixture T's file values are an oracle the build can generate for itself (`prd-inventory-import-journeys.md:104`, `:110`, `:111`; R6.6 at `prd-inventory-import.md:168`)
- **Problem:** "File Lab, LCh, XYZ, sRGB and HEX worked out from that list under D50/2°" does not say worked out *by what*.
  - If they come from the app's own derivation, "0 flagged" and "Lab within DERIVATION_TOLERANCE of the file's" pass for any derivation.
  - That includes a wrong white point or a missing Bradford adaptation, which the research shows would then flag every real record.
- **Precedent:** Collection Mode's harness already bans this ("Fixture values are declared, never copied from the implementation"). UJ 3 has no such rule.
- **Fix:** In UJ 3's preamble, make Fixture T's file values checked-in constants from an independent reference — DF R7.5's published source once it is named, or an independent colour library at a stated version — never from the app's derivation.

[MAJOR] M4: Fixture T timestamps and the Round 0 record appear to carry real-export data into a public repo (`prd-inventory-import-journeys.md:104`; `docs/agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md:18`, `:32`)
- **Evidence:**
  - The research report dates the real export's two sessions to [two real session dates].
  - Fixture T's Date Saved values fall on exactly those dates, with millisecond times ([three real timestamps]) that look sampled rather than invented.
- **What it conflicts with:** N3/F71, the brief's "never its data values (… dates …)", and Collection Mode's harness rule ("invented … dates").
- **Related:** Round 0 also quotes the real collection's name ('[the file's collection name]') and its reading count ([the file's reading count]).
- **Fix:**
  - Check the fixture against the real file (I did not open it).
  - Replace the timestamps with obviously invented values that keep the orderings the cases rely on: TK-1 before DJ6's 2026-09-20 scan, and the `:120` variants.
  - Owner call on redacting '[the file's collection name]' and [the file's reading count] from the verbatim quotes.

[MAJOR] M5: Import R6.2/R6.3, Custom Collection Name has no disposition (`prd-inventory-import.md:164–165`; journeys `:112`)
- **Problem:** R6.2 classifies every Toolkit column except Custom Collection Name, so under §6's preamble R2.2's "remaining columns as metadata" applies and every item would get that column.
  - Against that, R6.3's "an existing target ignores that column" and N15 read as *not stored*.
  - `:112`'s list of unstored columns omits it.
- **Impact:** The builder guesses. The table (CM R2.1) and the export's passthrough columns (Export R2.3) differ depending on the guess.
- **Fix:**
  - R6.2: "Custom Collection Name is read for R6.3's pre-fill and not stored".
  - Add it to `:112`'s no-column list.

[MAJOR] M6: Import R6.4, R3.8k and Capture R1.10, when the mode is adopted and rechecked (`prd-inventory-import.md:166`, `:124`; `prd-capture-mode.md:147`)
- **Problem:**
  - (a) R6.4 does not say an existing no-reading target adopts the mode *at commit*, inside R3.2's whole-or-nothing scope. A build that switches it on target selection passes `:117`, yet leaves the mode changed after Cancel, against R3.8j.
  - (b) R3.8k rechecks "source and session gate" but not the mode. UJ9.5-g shows capture settings stay offered while a preview is open, so the target can be switched to M1, or gain a reading, before commit. M2 readings then land in an incompatible collection, bypassing N8.
  - (c) The Capture §1 creation form shows a scan mode. Whether it is locked to the file's mode, or editable and then silently overwritten, is unstated.
- **Fix:**
  - R6.4: "adopts it at commit within R3.2's scope; Cancel or failure leaves it unchanged".
  - R3.8k: "recheck source, session gate and R6.4's fit (E46)".
  - State that the creation form shows the file's mode fixed.
  - Add a Cancel case to UJ 3.

[MAJOR] M7: Import R6.5/R6.6 and DF R2.3j vs Capture R4.6 — the file's illuminant and observer (`prd-inventory-import.md:167–168`; `prd-data-foundation.md:137`; `prd-data-foundation-journeys.md:113` vs `prd-capture-mode.md:180`)
- **Problem:**
  - R6.5 stores "its illuminant, observer and measurement mode" with the reading. Capture R4.6, a settled row, says illuminant and observer are recorded "with that value rather than with the reading".
  - UJ 3 `:111` says the derived values carry the *collection's* illuminant and observer.
  - Nothing covers a file whose pair differs from the collection's reference (the Toolkit lets users choose; Capture OQ 21 lists eighteen reference whites).
  - Nothing covers a pair the app cannot derive under. R6.6 then has no check to run, and R6.7 never excludes on Illuminant or Observer.
- **Fix:**
  - R6.5 stores the file's pair as the reference of the file's own (unstored) values — provenance for R6.6 — not as the reading's condition. Derived sets follow DF R3.1's collection reference.
  - R6.6/R6.7 state the outcome for an unsupported or unreadable pair (owner call: import unchecked and list it, or exclude via E47).
  - Add a Fixture T variant at D65/10°.

[MAJOR] M8: DF F64 and DJ6 vs DF R2.1 and CM R5.3, history order (`prd-data-foundation-fences.md:744`; `prd-data-foundation-journeys.md:114` vs `prd-data-foundation.md:112`; `prd-collection-mode.md:385`)
- **Problem:**
  - F64 and DJ6 row 2 say history *and* over-time views place the imported reading "by its measurement time".
  - R2.1 says history orders by record time. CM R5.3's Recorded order lists newest-recorded first.
  - A builder following F64 sorts history by measurement time and breaks R5.3. DJ6's "B before A" holds under both orders, so it does not settle the question.
  - R6.8c's "as an earlier reading" is false when Date Saved is after the scan.
- **Fix:**
  - F64/DJ6: "over-time views and the Measured order place B by measurement time; the Recorded order places it by record time, the commit".
  - DJ6 row 2 asserts both orders explicitly.
  - R6.8c: "kept in version history, not current".

[MAJOR] M9: `AGENTS.md:50`, `:108` — the repo's rule file still forbids this build
- **Problem:**
  - AGENTS §3 lists "Mobile-app data export / CxF migration path" under Open: "don't assume one and don't build as if one exists".
  - §8 defines the canonical value as "the stored mean of a saved set of samples … the basis the mean was taken on", which an imported value cannot meet.
  - Every agent reads AGENTS.md first, and the amendment (scoped to `docs/`) leaves it untouched. An unattended builder meets a direct instruction not to build §6.
- **Fix:**
  - Move the Toolkit CSV out of the Open list; CxF and other vendor apps stay open.
  - Add the imported exception to the Canonical value and Raw payload entries, in this PR or as a post-lock Documentation item.

[MINOR] m1: Import R6.8c/d (`:180–181`) — "scanned in SpectroCapture" vs "imported" is not tied to snapshot kind. A Demo Device current value, and a restore of an imported reading (R2.3f carries its imported snapshot), are unclassified. Fix: classify by the current reading's snapshot kind — live or simulated → R6.8c, imported → R6.8d.

[MINOR] m2: DF R2.3j (`prd-data-foundation.md:137`) has three gaps.
- The reason for an earlier imported reading kept behind an imported current value (R6.8d, UJ 3 `:120`) is unstated.
- The automatic "re-measurement" contradicts R2.4's "asks rather than guessing" (`:116`), and R2.4 names no exception.
- R2.3j marks only samples as not recorded, while CM F221 and Export R1.1t also treat basis, spread and verdict as unrecorded. DF owns the SQL shape yet does not state them, and DJ6 row 5 does not read them.
- Fix: name all three in R2.3j and R2.4.

[MINOR] m3: CM copy lines "R4.2d imported" and "R5.2e Value" (`prd-collection-mode-copy.md:295`, `:310`) show only serial, firmware and samples. CM R4.2d/R5.2e (`prd-collection-mode.md:348`, `:381`) also require basis, spread and verdict shown as unknown or not recorded. UJ2.1-r asserts the copy line, so a build following the row fails it. Fix: align copy and row.

[MINOR] m4: Import copy E43 (`prd-inventory-import-copy.md:29`) — the Toolkit variant adds a second "Unchanged:" label beside the row-count one. An item whose reading becomes current is also counted as an Unchanged row (R3.6g). Fix: label it "Readings unchanged:".

[MINOR] m5: Import R6.1 vs R3.8n (`:163`, `:127`) — E43 still offers "Choose an encoding" and "Choose a separator" on a detected export. R6.1 overrides only the *default*, so a manual comma or Windows-1252 choice leaves its Toolkit status undefined. Fix: show both controls fixed for a Toolkit export, or state that a manual change reads it as a plain CSV.

[MINOR] m6: Import R6.1/R6.6 — the signature does not require the L, a, b columns R6.6 compares against. Nothing covers a missing or non-numeric file Lab, a blank Nix Device, or R-nm columns outside 400–700. Fix: "where the first L, a, b are present and numeric; otherwise listed as unchecked".

[MINOR] m7: Fixture T (journeys `:104`)
- The header "Index; Custom Collection Name; …" reads as semicolon-plus-space. The research's column map shows no space, and with a space R1.5c makes the quoted fields E5.
- Color Code, Index and Density values are not declared.
- Fix: say "no space after the delimiter"; declare Color Code `TK-1`–`TK-3`, Index 1–3, and the densities.

[MINOR] m8: R6.10 (`:172`) — "each R6.8 outcome" has no UJ 3 case for R6.8e's absent item, for R6.3's existing target ignoring the name, or for re-import after R6.8c (see M1).

[MINOR] m9: CM M2 (`prd-collection-mode.md:574`) — the seeded file's 15 items include no imported item, so the tenth mark never enters the honesty-mark agreement metric.

[MINOR] m10: Export R2.3 (`prd-data-export.md:120`) says the out-of-grid branch is "unreachable for v1's instrument-family grid". A Toolkit spectrum is fixed at 400–700/10 nm, while WAVELENGTH_GRID (OQ 4) is still open, so the branch becomes reachable if the grids differ. Say so in R1.1t or OQ 4.

[MINOR] m11: CM R2.2 counts use the capture PRD's "scanned" tally (UJ2.1-j), which will include imported items never scanned in the app. Owner call; at minimum, name it.

[MINOR] m12: Import OQ 1 and M1 (`:196`, `:188`) — the format rests on one export, and Toolkit exports are in neither OQ 1's corpus nor M1. Format drift (app versions, locale decimals, M0/M1 or D65/10° files) has nowhere to be gathered.

[NIT] n1: R6.2 (`:164`) says Index is "read for R6.6's check", but R6.6 never uses it.

[NIT] n2: R3.8b (`:115`) lists E46 generally, but only E46's mixed-modes variant offers "Pick the file again". The E46 collection copy (`copy:32`) omits "or one holding no reading".

[NIT] n3: Import Vocabulary "Ready to capture" (`:21`) and §4 (`:133`, "UJ 2–2.2") are not extended to Toolkit exports and UJ 3.

[NIT] n4: R6.3 (`:165`) — "every record carries the same one" does not say whether that means exact text or R2.3 equality.

#### Biggest risks (if executed as-is)
1. **The build halts, or ships the wrong count.** B1 means no correct build passes both UJ 3 count cases, so an unattended agent will either stop or bend E43's tallies to fit one.
2. **Permanent, invisible duplicates.** Re-import can duplicate readings in history (M1). DF's immutability makes each duplicate unremovable, and no case catches it.
3. **Every real export can fail while CI stays green.** A decimal-only parser (M2) combined with a self-generated fixture oracle (M3) passes CI and then excludes every record of a real Toolkit export.
4. **Private research data may be public.** The Fixture T dates look copied from the real export, and '[the file's collection name]' and [the file's reading count] sit in the public review log (M4), against N3.
5. **The builder is told not to build.** AGENTS.md still says don't build this (M9), and it is the first file every agent reads.
6. **There is no word budget left for the fixes.** By rule 14's method (verified): DF is at 8,592 of 8,600 words and CM at 12,497 of 12,500. The M7/M8/m2 fixes land in DF and will need either compaction or another owner raise.

#### Plan strengths
- **N1–N15 are fully covered.** Each decision has a fence, a carrying row and a fence→row map entry in all six PRDs; the "Carried by" lists match the rows.
- **The research's format facts are applied correctly:**
  - the duplicate L is compared by position ("first L");
  - reflectances above 1.0 are stored as given, with the gamut flag recomputed;
  - the `undefined` Note is stored empty;
  - quoting in data rows only is left to R1.5c;
  - E47's "record 4" follows R1.4's numbering.
- **"No raw payload, never supplied"** is kept distinct from R5.5a's archive-unavailable mark in DF R1.6, DJ6, Export R1.1t/R1.1p and E1.
- **The imported mark is keyed to the current reading's snapshot kind** (CM R2.4j), so re-scan, restore and history behave without special cases. The mark counts are consistent (ten R2.4 marks + two = twelve in R8.9, UJ2.1-k and UJ9.6-a).
- **False-positives I checked and ruled out:**
  - Changing a collection's scan mode after import is already defined (DF R3.3d: value absent, never substituted).
  - Derived sets under the collection reference agree with DF R3.1.
  - Toolkit detection skipping E45 does not break UJ 2's plain-semicolon E45 case.
  - All new anchors resolve.
  - Both word budgets fit.

#### Spec coverage gaps (requirements with no task)
- **G1 — build order and phase are named nowhere.** The brief's ordering (Device R1.21's imported kind and DF R2.3j before Import R6.5; CM R2.4j after both) appears in none of:
  - Import's Build dependencies (`prd-inventory-import.md:30`);
  - CM's Build dependencies row 1 (`prd-collection-mode.md:205`);
  - post-lock's First build PR.

  Nor is it stated that §6 needs DF's in-app spectrum-to-six-spaces derivation (R3.1–R3.4, DF OQ 15's interim) running without an instrument or licence. Nor that R6.6's tolerance is DF OQ 6's provisional constant, which gates release.
- **G2 — no fixture-inventory row declares an imported-kind reading.** DF R7.7 (`prd-data-foundation.md:245`), "the sole fixture inventory", declares simulated snapshots via Device R6.9 and hand-authored live-kind snapshots only. So:
  - Export R4.2's goldens never assert `sc_imported` true;
  - CM UJ2.1-r's seeded state, Device UJ3-c, DJ6 rows 2–3 and CM M2 have no declared way to seed an imported reading short of running §6 — which G1 has not ordered.

| Row ID | disposition |
|---|---|
| Import-R1.5 | ALIGN |
| Import-R3.3 | OBJECT (M1) |
| Import-R3.8 | OBJECT (M6, n2) |
| Import-R6.1 | OBJECT (m5, m6) |
| Import-R6.2 | OBJECT (M5, n1) |
| Import-R6.3 | OBJECT (M5, n4) |
| Import-R6.4 | OBJECT (M6, M2) |
| Import-R6.5 | OBJECT (M7) |
| Import-R6.6 | OBJECT (M3, M7, m6) |
| Import-R6.7 | OBJECT (M2) |
| Import-R6.8 | OBJECT (M1, m1) |
| Import-R6.9 | OBJECT (B1, m4) |
| Import-R6.10 | OBJECT (M3, M4, m7, m8) |
| DF-R1.6 | ALIGN |
| DF-R2.2 | ALIGN |
| DF-R2.3 (with R2.3j) | OBJECT (M8, M7, m2) |
| Device-R1.21 | ALIGN |
| Device-R1.22 | ALIGN |
| CM-R2.4 (with R2.4j) | OBJECT (G1, G2, m9) |
| CM-R2.7 | ALIGN |
| CM-R4.2 (with R4.2d) | OBJECT (m3) |
| CM-R5.2 (with R5.2b/e) | OBJECT (m3) |
| CM-R8.9 | ALIGN |
| Export-R1.1 (with R1.1p/t) | OBJECT (G2, m10) |
| Export-R1.2 | ALIGN |
| Export-E1 | ALIGN |
| Capture-R1.10 | OBJECT (M6, M2) |

### database

#### Verdict
Sound after fixing the Blocker and the seven Majors. The data model underneath is right: an explicit stored current selection, a device snapshot copied into each reading, derived values recomputed from the spectrum, and the two time axes kept apart. The problems are in the rows' definitions, and one missing identity rule lets duplicate readings into history that can never be deleted.

#### Schema & engine (brief)
- **No DDL to read.** There is no code in the repo, and ADR-0003 (the storage schema) is still queued in `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/decisions/README.md:22`. Data Foundation (DF) puts the schema out of scope. So the requirement rows are the data contract, and I traced each against the logical entities they imply:
  - item: Swatch Code unique per collection under R2.3
  - reading: per-item sequence, measurement time, record time, supersession reason, explicit current selection, and a copied snapshot (kind ∈ {live, simulated, imported}, model, serial, firmware)
  - sample tier: 1–5 per scanned reading, none for an imported one
  - derived sets keyed reading × condition
  - the collection's chosen scan mode
  - imported metadata as decoded text
- **Engine.** SQLite is decided; GRDB is recommended; reads happen at SQLITE_READER_FLOOR (OQ 2 open). WAL is implied by DJ3's WAL-frame and journal-or-log cases. `PRAGMA foreign_keys`, the journal mode and `busy_timeout` are decided nowhere; they are ADR-0003's.
- **Sources read.** I read the diff 6374538..417fdac, all 14 target files, and all 12 contract sources, including the local format report.

#### Findings

[BLOCKER] Import R6.8b–d and R6.9, DF R2.3j — imported readings are deduplicated only against the current reading, so identical readings pile up in history that can never be deleted.
- **Where.**
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import.md:179-181`, `:171`, `:78`
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/data-foundation/prd-data-foundation.md:137`
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import-journeys.md:118-120`
- **The defect.** The only sameness test is in R6.8d: "Date Saved and every reflectance equal — changes nothing". It applies only when the item's *current* reading was imported.
  - R6.8c ("the reading is kept in version history") has no sameness test at all.
  - R6.8d's "an earlier one goes to history" has none against readings already in history.
  - R6.8b ("no current value") has none against history either.
  - Readings cannot be deleted one at a time (DF R6.1, CM R5.6). So a duplicate is permanent: E17 lists it twice, over-time views plot it twice, and the history export emits it twice.
- **Scenario 1.** From the state `:118` leaves (TK-1: scan current, imported reading in history), import Fixture T again. R6.8c adds a second identical TK-1 reading. This contradicts R6.9 and R3.3 ("repeating … changes nothing"), but the effect row mandates it.
- **Scenario 2.** After `:120`'s variant, TK-3's current reading is the 2026-09-25 one and the original is in history. Re-import the original Fixture T: its TK-3 reading is "earlier", so R6.8d *requires* a second copy. R6.9 does not protect here, because a newer import intervened.
- **Scenario 3.** An imported reading was Flagged away (CM R4.9), so the item has no current value. Re-importing the same file sends it to R6.8b, which copies the reading back and makes it current, silently undoing the Flag.
- **Scenario 4.** The user restored an older imported reading. Re-importing sends the newer one, which sits in history, down R6.8d's "later" branch: it is copied again and made current, undoing the restore.
- **Mutation.** A build that appends unconditionally under R6.8b, R6.8c and R6.8d passes every UJ 3 and DJ6 case. The only repeat case (`:119`) starts from the all-imported-current state.
- **Fix.**
  - Add a first Toolkit outcome row, applied before R6.8b–d: "The item already holds a reading imported from an equal record — R6.8d's sameness — current or in history, a restore's copy aside → changes nothing; counted unchanged."
  - R6.8d then compares against the current reading only to order a new one.
  - Add cases: repeat Fixture T over `:118`'s state (TK-1 holds exactly 2 readings); re-import the original Fixture T after `:120` (TK-3 holds exactly 2, E43 counts it unchanged); Flag TK-2, then re-import (TK-2 stays set aside, no new reading).
  - For ADR-0003: a UNIQUE index here must exclude restores, because R2.3f legitimately copies measurement time and spectrum. Use a partial index on imported kind AND reason ≠ restore, or enforce the check in the commit.

[MAJOR] Import R6.8d and R6.5 — "equal" and "as given" are undefined, so reformatted text can pass for a new reading or be misread by SQL.
- **Where.** `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import.md:181`, `:167`, `:48`; journeys `:111`.
- **(a) Equal as text?** R1.5 keeps decoded text without numeric or date coercion, R6.5 says "as given", and R6.7 parses the fields as a time and numbers. If sameness is textual, a Toolkit version that writes `0.512345678` for `5.12345678e-01`, or `+00:00` for `Z`, turns one measurement into a "new" reading.
- **(b) Stored as TEXT?** If reflectances are stored as TEXT to honour "as given", SQLite compares them with a numeric literal as text. An outside reader's `WHERE r600 > 1.0` then matches `'9.87654321e-01'`. That silently breaks DF R1.2's and M4's promise that the file can be queried directly.
- **(c) The tie is unassigned.** An equal Date Saved with a different spectrum is neither the same, later nor earlier.
- **(d) Precision.** The oracles assert milliseconds (`…42.543Z`). A store at second precision, such as integer Unix seconds, breaks both the SQL oracle and sameness.
- **Fix.**
  - R6.5: "its reflectances as the finite numbers given, neither clamped nor rounded; Date Saved read as an instant with its offset, kept to the millisecond".
  - R6.8d: "the same reading — the same instant to the millisecond and every reflectance the same number".
  - Assign the tie: an equal Date Saved with a different spectrum is kept in history, the current reading unchanged, and listed in E43.
  - Case: re-import Fixture T rewritten with plain decimals and `+00:00` offsets → 3 unchanged.

[MAJOR] DF F64 and DJ6 row 2 against DF R2.1 and CM R5.3 — history order for an older-measured reading that is recorded later is contradictory.
- **Where.**
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/data-foundation/prd-data-foundation-fences.md:744`
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/data-foundation/prd-data-foundation-journeys.md:114`
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/data-foundation/prd-data-foundation.md:112`
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/collection-mode/prd-collection-mode.md:385`
- **The conflict.** R2.1 (aligned, not amended) says "history orders by record time, over-time views by measurement time". CM R5.3's default E17 lists readings newest-recorded first. F64 (from N10) instead says history *and* over-time views place an imported reading kept behind a scan by its measurement time, and DJ6 row 2 asserts that.
- **What a builder does.** Either (a) follow R2.1 and R5.3, or (b) sort imported readings by measurement time inside the recorded list. Option (b) is a mixed sort key: a restore of an imported reading carries its source's measurement time and imported kind (R2.3f), so the current restore would drop down to July, below older-recorded readings.
- **The fixture cannot tell them apart.** TK-1's imported reading is both older-measured and newer-recorded than scan A, so it lists before A in both orders.
- **Fix.**
  - Have the owner clarify N10 against R2.1 and R5.3. Recommended reading: N10's "history" means over-time views, which E17's Measured order already is.
  - Reword F64 and DJ6 row 2: "over-time views, E17's measured order among them, place B by its measurement time; the recorded order and the per-item sequence place it by its record time (R2.1)".
  - Add a discriminating case: scan A measured 2026-09-20; a Toolkit record dated 2026-09-25 imported later → A current, measured order A then B, recorded order B then A.
  - If the owner means the recorded order too, amend R2.1 and CM R5.3 and state where a restore sits.

[MAJOR] DF R2.3h–j and CM R2.4i — the supersession relation can no longer be recovered from sequence order, so the wrong reading can be marked never true.
- **Where.**
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/data-foundation/prd-data-foundation.md:124`, `:135-137`, `:115`
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/collection-mode/prd-collection-mode.md:301`
  - DJ6 `:116`
- **What changed.** The legend (`:124`) defines A as the old current reading at the time of the operation, but nothing requires that relation to be stored. R2.3i ("A → B → C") and CM R2.4i ("its successor") recover it later. Until this amendment every sequenced reading became current when it landed, so the previous sequence number always reproduced the relation. R2.3j breaks that: an imported reading can take the next sequence number and the latest record time without becoming current.
- **Scenario.** TK-1: scan A is current (sequence 1). Imported B is kept behind it (sequence 2, reason initial). The operator re-scans, and C (sequence 3) lands correction-unconfirmed over A.
  - A model that links by adjacent sequence numbers puts the awaiting-answer mark on B.
  - On "The old reading was wrong" it marks B never true. That permanently excludes a legitimate Toolkit reading from over-time views and QC references (R2.3d), while the actually wrong scan A stays a legitimate point.
  - DJ6 row 4 re-scans TK-2, whose imported reading is current, so no case catches this.
- **Also unassigned.** The reason given to a reading kept behind an *imported* current (R6.8d's "earlier" branch, journeys `:120`). R2.3j's reason cell covers only "initial" and "re-measurement over an imported A".
- **Fix.**
  - R2.3h: "a reading's predecessor is the reading current when it landed, never the previous sequence number; a reading kept behind (R2.3j) has none and is no reading's predecessor".
  - R2.3j reason cell: "initial when current over none or kept behind any A; re-measurement when current over an imported A".
  - R2.3's lead: "A reading that becomes current supersedes the current one".
  - DJ6 case: TK-1 as row 2, re-scan C, answer "old reading was wrong" → A never true, B unmarked and still an over-time point.

[MAJOR] Import R6.4 and R3.8k — the mode-fit check is not re-run at commit, and the mode adoption is not placed inside the transaction.
- **Where.** `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import.md:166`, `:124`, `:129`.
- **The gap.** Mode fit is checked "before mapping" and again on R3.8p's target change. R3.8k's pre-commit recheck covers only the source and the session gate.
- **How it bites.** While a preview is open:
  - the target's scan mode can still change (Capture R1.10 allows it between sessions, and a preview is not an R1.11 write);
  - the target can gain a reading, through Add a swatch or a session started and ended.

  CM UJ9.5-g drives exactly these actions with an import previewed. The commit then lands M2 readings in a collection now at M1, or adopts M2 over a collection that now holds a reading — both of which N8/F76 forbid.
- **Also unstated.** That "then adopt it" is written by the commit and rolled back with it (R3.2). A build that writes the adoption at target selection leaves an existing collection's mode changed after Cancel or E44.
- **Fix.**
  - R3.8k: "recheck source, session gate and R6.4's mode fit inside the commit's R1.11 hold; an unfit target → E46".
  - R6.4: "the adoption is written by the commit and rolled back with it".
  - Case: preview into an M2 target, switch it to M1 in Capture settings, Import → E46, nothing written.

[MAJOR] Import R6.5–R6.7 — `NaN` and `Infinity` count as readable numbers, and SQLite stores NaN as NULL; unreadable Lab, Illuminant and Observer have no rule.
- **Where.** `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import.md:167-169`.
- **Why it's likely.** The exporter is evidently JavaScript (Note is the literal `undefined`), and JavaScript prints `NaN` and `Infinity`. Rust's f64 parse and Swift's `Double(String)` both accept those strings.
- **Effect on stored data.** SQLite stores a bound NaN as NULL and +Inf as a REAL infinity.
  - The spectrum stored "as given" silently gains a NULL cell.
  - Derived values computed from it become NaN, then NULL. The result is a current reading with no working-set value and no absent mark — the state R3.5 forbids and R7.1 enumerates.
- **Effect on the check.** In R6.6, a NaN or empty file L/a/b makes ΔE2000 NaN. `NaN > tolerance` is false, so the record silently reads as matching.
- **Missing fields.** R6.7 also omits Illuminant and Observer, which the R6.6 check and R6.5's stored reference both need. An unrecognised illuminant has no rule.
- **Fix.**
  - R6.7: "…a time with its offset, one of M0/M1/M2, or a finite decimal number — NaN, Infinity or an empty cell cannot be read". Keep negative values as given, like values above 1.0.
  - R6.6: "a record whose file L, a, b are not three finite numbers, or whose Illuminant/Observer the derivation does not offer, is listed as not checked, never as matching, and still imports".
  - Cases: R550 `NaN` → E47; an empty file L → listed as not checked.

[MAJOR] Import R6.2 — the `undefined` placeholder is stored as an empty string, which clears the user's notes by default on re-import.
- **Where.** `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import.md:164`; journeys `:112`.
- **The mechanism.** `undefined` is the exporter's stand-in for "no note", but R6.2 stores it as a present empty string. Under R3.6c an empty incoming value replaces a present one. For captured items — which every Toolkit item is — E14 defaults to "Take the new details".
- **Scenario (the re-import workflow N14 is designed for).**
  1. Import Fixture T.
  2. Add a note to TK-1 in Collection Mode.
  3. Import a newer Toolkit export.

  E14 proposes changing the note to empty, the default clears it, and DF R2.3 puts cleared text beyond recovery. This is null-versus-empty confusion, landing as data loss by default.
- **Fix.**
  - R6.2: "a Note holding exactly `undefined` is not supplied for that record: a stored Note is kept and never offered for replacement; an absent one stays absent" (R3.6a applied per record).
  - Update `:112`'s oracle.
  - Case: edit TK-1's note, re-import → note kept, no E14 row for it.

[MAJOR] Import R6.9 and E43 — the reading counts are undefined, and two UJ 3 oracles contradict each other.
- **Where.**
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import-journeys.md:110`, `:118`, `:120`
  - `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import-copy.md:29`, `:35`
  - `prd-inventory-import.md:171`
- **The contradiction.** `:110` counts three new items' readings as "3 becoming current". `:118` counts only TK-2 as "1 becoming current", although TK-3 is also new and its reading also becomes current. A correct build fails one of the two cases.
- **The missing definitions.** The copy's placeholder note (`:35`) maps count placeholders to R3.6 counts, but ⟨readings⟩, ⟨current⟩, ⟨history⟩ and ⟨unchanged readings⟩ have no R3.6 count. It is also unstated whether a stored reading displaced by a newer import counts in ⟨history⟩; `:120` asserts no counts at all.
- **Fix.**
  - Define in R6.9 (this partitions ⟨readings⟩; displaced stored readings are not counted):

    | Placeholder | Counts |
    | :--- | :--- |
    | ⟨readings⟩ | eligible records |
    | ⟨current⟩ | R6.8a, R6.8b and R6.8d-later |
    | ⟨history⟩ | R6.8c, R6.8d-earlier and the equal-date tie |
    | ⟨unchanged readings⟩ | readings the new sameness row (Blocker fix) matches |

  - Correct `:118` to "2 becoming current (TK-2, TK-3), 1 kept in history".
  - Assert `:120`'s counts.

**Minors**

[MINOR] Illuminant and observer stored on the reading.
- **Where:** Import R6.5 and DF R2.3j.
- **Defect:** both store the file's illuminant/observer with the reading, but Capture R4.6 (`/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/capture-mode/prd-capture-mode.md:180`) records them with each value. The derived sets use the collection's reference (journeys `:111`), so an outside reader can join the wrong reference.
- **Fix:** keep the file's illuminant/observer as provenance for R6.6 only, never as a stored set's reference.

[MINOR] "Hold no reading" is undefined.
- **Where:** Import R6.4 and Capture R1.10.
- **Defect:** it is unclear for items holding only history (Flagged or quarantined) and for QC records.
- **Fix:** "no item holds any reading, current or in history; QC records (DF R2.3g) aside".

[MINOR] R6.8c/d don't name how "scanned in SpectroCapture" is told from "imported".
- **Where:** Import R6.8c and R6.8d.
- **Fix:** name the test — the current reading's snapshot kind, with a restore carrying its source's kind (R2.3f). Demo Device readings then fall under R6.8c and restores of an imported reading under R6.8d.

[MINOR] DF's vocabulary and floor-readable lists aren't amended for imported readings.
- **Where:** DF vocabulary "Reading — one saved set of 1–5 samples" (`/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/data-foundation/prd-data-foundation.md:37`), R2.1's "keep every sample", and the R1.2 and R7.1 floor-readable lists.
- **Defect:** none of them covers a reading with zero samples, or a payload never supplied at the reading level (R7.7i's mark is per sample, and an imported reading has no sample row to carry it).
- **Fix:** amend them, or state both facts are derivable from the imported kind, so no count or restore that joins through samples drops imported readings (E8/R6.2 counts, R2.6 cost, an R2.3f restore of an imported reading).

[MINOR] Salvage can promote an imported reading over a scan.
- **Where:** DF R5.5d (`:187`).
- **Defect:** salvage keeps "the later-recorded" of two current readings. After R6.8c, that can be an imported reading kept behind a scan, so salvage would make it current against F74.
- **Fix:** prefer the non-imported reading, or hand this tie to ADR-0003 by name.

[MINOR] DF R2.4 isn't carved out for R2.3j.
- **Where:** DF R2.4 (`:116`).
- **Defect:** it says "the app asks rather than guessing", but R2.3j records re-measurement without asking.
- **Fix:** add "save R2.3j".

[MINOR] Import R3.2's all-or-nothing rule names only R3.3a–d.
- **Where:** Import R3.2 (`:77`).
- **Fix:** also name R6.8a–e with their derived sets and R6.4's mode adoption. Otherwise a build may compute derived sets after the commit, leaving a window where a current reading has no working set.

[MINOR] R6.2 doesn't say what happens to unlisted columns.
- **Where:** Import R6.2.
- **Defect:** it is silent on Custom Collection Name (stored as a metadata column by R2.2's default, or not?) and on any column outside its list, such as a later Toolkit column.
- **Fix:** decide, and assert the result at journeys `:112`.

[MINOR] No fixture yields `sc_imported` true.
- **Where:** DF R7.7's sole fixture inventory (R7.7a–n).
- **Defect:** it gains no imported-kind fixture, so Export R4.1/R4.2's golden never holds `sc_imported` true. EJ1's new row uses a fixture outside the inventory.
- **Fix:** add R7.7o: an imported current reading, and an imported reading kept behind a scan.

[MINOR] The out-of-grid branch now depends on Export OQ 4.
- **Where:** Export R2.3.
- **Defect:** it calls the out-of-grid branch unreachable for v1's grid. Toolkit spectra are fixed at 400–700 nm in 10 nm steps (R6.1), so that is true only if Export OQ 4 closes on that grid.
- **Fix:** record the dependency against OQ 4.

**Nits**

[NIT] Import R6.2 says Index is "read for R6.6's check", but R6.6 uses only L, a, b. Index is simply not stored.

[NIT] "Serial and firmware unknown" should read back as NULL, never a sentinel string that an outside reader or the export could take for a value (R1.1t emits empty).

[NIT] Capture R1.10 says the mode is changeable "never while R1.11 holds writes", yet the adoption lands inside the import commit, which is itself an R1.11 write. Add "save as that commit's own write".

#### Biggest risks
1. **Permanent duplicate readings (the Blocker).** They grow with every re-import of an older or overlapping export, and they can only be removed by deleting the item.
2. **The wrong reading marked never true (predecessor Major).** This starts on the first re-scan of an item whose imported reading sits behind a scan. It corrupts over-time views and QC references without any sign.
3. **Default-on loss of user notes (the `undefined` Major).** It happens on the designed re-import path.
4. **Silent NULLs in the spectrum and derived sets (the NaN Major).** One malformed Toolkit value is enough.
5. **Mixed-mode collections (the mode-fit Major).** They can arise through an open preview window.

#### Genuinely sound
- **Current selection is explicit.** DF R2.2 requires the current reading to be told from the file. That is essential here: after R6.8c, no heuristic — highest sequence, latest record time, latest measurement time — always picks the right reading. The PRD already rules heuristics out.
- **At most one reading per item per commit.** Duplicate codes within a file are excluded wholesale (R2.4/E10), so a single commit can never produce two current readings on an item.
- **The snapshot is copied into the reading (Device R1.21/R1.22).** It is not a foreign key to a device record, and that is the right call for immutability. "Occupies none" follows naturally. A textbook normaliser would wrongly move it to a devices table.
- **One source of truth for colour values.** Derived values are recomputed from the spectrum, and the file's L/a/b/XYZ/sRGB/HEX are not stored, so two Lab values cannot drift apart. The research shows agreement within 0.0094 ΔE2000.
- **The two time axes are used correctly.** Date Saved is the measurement time and the commit is the record time.
- **Existing rules already handle a later mode change.** If the collection's mode changes after import, R3.3d's value-absent mark covers it; no new rule is needed.
- **Density and Note stay decoded text.** For metadata with no v1 feature, text is right — it keeps values like `[a negative value]` in their original form. A textbook DBA would want REAL columns.
- **Honest storage choices.** Values above 1.0 are kept unclamped. A payload never supplied is distinct from an unavailable archive, and Export R1.1t does not count it as unavailable. `sc_imported` is a separate boolean rather than making `sc_simulated` three-valued, so existing consumers' parsing is unchanged.

#### Missing / over-engineered
- **ADR-0003's input list gains nothing from this amendment.** It should gain:
  - the imported-kind snapshot with NULL serial and firmware;
  - readings with zero samples;
  - a payload never supplied, recorded at the reading level;
  - a partial unique index on current readings (`ON reading(item_id) WHERE is_current`);
  - an explicit predecessor link (the predecessor Major);
  - the imported-reading sameness rule, whose UNIQUE index must exclude restores (the Blocker).
- **Missing cases:**
  - a repeat import over R6.8c;
  - an older export imported after a newer one;
  - a restore or a Flag, then a re-import;
  - a newer-measured imported reading kept behind a scan;
  - a re-scan, then "the old reading was wrong", on an item holding an interleaved imported reading.
- **Nothing over-engineered.** The third snapshot kind and one boolean export column are the minimum.

| Row ID | disposition |
| :--- | :--- |
| Import-R1.5 | ALIGN |
| Import-R3.3 | OBJECT (Blocker: imported readings deduplicated only against the current reading) |
| Import-R3.8 | OBJECT (Major: mode-fit check not re-run at commit) |
| Import-R6.1 | ALIGN |
| Import-R6.2 | OBJECT (Major: `undefined` stored as an empty string clears notes) |
| Import-R6.3 | ALIGN |
| Import-R6.4 | OBJECT (Major: mode-fit check not re-run at commit) |
| Import-R6.5 | OBJECT (Majors: "equal" and "as given" undefined; NaN/Infinity accepted as numbers) |
| Import-R6.6 | OBJECT (Major: NaN/Infinity accepted as numbers) |
| Import-R6.7 | OBJECT (Major: NaN/Infinity accepted as numbers) |
| Import-R6.8 | OBJECT (Blocker: imported readings deduplicated only against the current reading; Majors: "equal" and "as given" undefined; supersession relation not recoverable from sequence) |
| Import-R6.9 | OBJECT (Blocker: imported readings deduplicated only against the current reading; Major: reading counts undefined and oracles contradict) |
| Import-R6.10 | ALIGN |
| DF-R1.6 | ALIGN |
| DF-R2.2 | ALIGN |
| DF-R2.3 (with R2.3j) | OBJECT (Blocker: imported readings deduplicated only against the current reading; Majors: history order contradictory; supersession relation not recoverable from sequence) |
| Device-R1.21 | ALIGN |
| Device-R1.22 | ALIGN |
| CM-R2.4 (with R2.4j) | ALIGN |
| CM-R2.7 | ALIGN |
| CM-R4.2 (with R4.2d) | ALIGN |
| CM-R5.2 (with R5.2b/e) | ALIGN |
| CM-R8.9 | ABSTAIN (out of lens) |
| Export-R1.1 (with R1.1p/t) | ALIGN |
| Export-R1.2 | ALIGN |
| Export-E1 | ALIGN |
| Capture-R1.10 | ALIGN |
