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


## Round 2 — delta verification (2026-09-26)

Subject: commit f7cbde5 (round 1's fixes over 2a54fce). The same nine lenses, each given its own round-1 section above, the round-1 fix file and the fences as sources, and asked to mark every round-1 finding RESOLVED, PARTIAL or UNRESOLVED, report new defects the fixes introduced, and give a disposition over every row and state at pre-alignment. No brief cited the research report; no cross-model pass ran.

### Orchestrator verification

No lens reported an open Blocker, and every round-1 Blocker was marked RESOLVED by the lens that raised it. Every new Major was checked against the branch by grep and reproduced:
- the commit-recheck case contradicting R6.4, since a pending-only target switched to M1 still fits;
- DF R5.5d's tie-break reversing R2.3j for simulated and imported pairs;
- R7.7a–n left at four sites;
- E48 claiming checks R6.2 ignores;
- the E7/E9/E10/E42 splice rule;
- UJ2.1-s naming a P1 variant and not telling the orders apart;
- R6.6's undefined "cannot work under";
- R6.10 against the Collection Mode harness's declared fixtures;
- the fixed 0.05 offset;
- M2 and OQ 3 leaving what may be recorded unbounded.

None failed to reproduce. The lens reviews below passed the same local-only real-value check; the one hit, a fence ID, was a false positive. No owner decision was needed: each fix sits inside a recorded fence and is recorded by a dated round-2 Clarified line.

### What landed

The fix file [`prd-inventory-import-round-2-fixes.md`](../product/import/prd-inventory-import-round-2-fixes.md) lists every finding by ID. It covers:
- the commit recheck, with E13's new collection variant;
- the salvage rule;
- the fixture range;
- E48, and written-out Toolkit variants for E5, E7, E9, E10 and E42;
- E43 and E14 wording;
- the history-order cases (UJ2.1-s, UJ2.1-t);
- offered pairs;
- direct seeding of imported fixtures, with a Collection Mode build row and a post-lock build-order item;
- tolerance-relative offsets;
- the bound on dogfood records;
- the quarantined-reading exemption;
- the time and number grammar;
- the new UJ 3 cases;
- the ADR-0003 inputs;
- AGENTS.md §5.

### product-manager

#### Verdict
Builds the right thing for the user. Round 1's Blocker and seven of its eight Majors are fully resolved; PM9 is partly resolved, and what remains of it is only a missing sentence of copy. The fixes add no new Blocker or Major. What remains is copy precision and cross-references: one preview line calls a newer Toolkit reading "earlier", E46's mixed-modes refusal is now a dead end, and a salvage tie-break contradicts F86.

#### User & problem context (brief)
- **User and job:** the Cataloger (also the owner and first user) has already measured part or all of a physical collection in the Nix Toolkit mobile app. They want those readings in a SpectroCapture collection without re-scanning, with honest provenance. They then want to keep importing, scanning and correcting on top of them (vision J7, pulled into v1).
- **What is validated:** one real export (Spectro 2, M2, D50/2°, one collection, codes filled).
- **What is assumed:** every other Toolkit export has the same shape. That assumption is now named and tracked by Import OQ 3, M2 and a post-lock dogfood item, which is the right size for a one-user tool.

Path abbreviations below. Every path is absolute under `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/`:
- **IMP** `docs/product/import/prd-inventory-import.md`
- **IJ** `docs/product/import/prd-inventory-import-journeys.md`
- **IC** `docs/product/import/prd-inventory-import-copy.md`
- **FIX** `docs/product/import/prd-inventory-import-round-1-fixes.md`
- **DF** `docs/product/data-foundation/prd-data-foundation.md`
- **DJ** `docs/product/data-foundation/prd-data-foundation-journeys.md`
- **CM** `docs/product/collection-mode/prd-collection-mode.md`
- **CMC** `docs/product/collection-mode/prd-collection-mode-copy.md`
- **CMJ** `docs/product/collection-mode/prd-collection-mode-journeys.md`
- **EX** `docs/product/export/prd-data-export.md`
- **CAP** `docs/product/capture-mode/prd-capture-mode.md`
- **VIS** `docs/product/vision.md`
- **PL** `docs/product/post-lock.md`
- **AG** `AGENTS.md`

#### Findings

##### Round-1 findings: status

**Blocker and Majors**
- **PM1: RESOLVED.** New R6.8f runs first and matches against every reading the item holds, current or in history (IMP:182, Vocabulary IMP:23). DF R2.3j adds "none when the item already holds the same reading" (DF:137). The cases I asked for are all there: repeat over a scan-current state (IJ:134), newer-then-older (IJ:136–137), Flag then re-import (IJ:139), and DJ6's no-new-reading row (DJ:120).
- **PM2: RESOLVED.** R6.8 keys on the current reading's snapshot kind (IMP:173). R6.8g makes a Toolkit reading current over a simulated one (IMP:186), mirrored in DF R2.3j (DF:137), with cases at IJ:140 and DJ:118.
- **PM3: RESOLVED.**
  - R6.4 says the commit writes the mode adoption, the commit's rollback undoes it, and E43 names it (IMP:169).
  - R3.8k re-checks the fit at commit (IMP:126).
  - E43 has the mode-change sentence (IC:29).
  - Cases cover Cancel, E44, and a mode change between preview and commit (IJ:130–132).
- **PM4: RESOLVED.** E12 has a "From Nix Toolkit" meaning sentence (CMC:173).
- **PM5: RESOLVED.** R6.1 looks for the signature under semicolon, comma and tab (IMP:166), with a comma re-save case (IJ:111). The near-miss state is deferred at PL:101 with a sound reason: OQ 3's corpus will show which near-misses actually occur.
- **PM6: RESOLVED.**
  - Scenario B (several Toolkit collections in one file) is now R6.11 and E49 (IMP:176, IC:35, IJ:128).
  - Scenario A (blank codes) gets the Toolkit remedy rule (IC:39). Whether a code can be blank goes to OQ 3 (IMP:205). That deferral is sound: the remedy can be followed, and nobody yet knows if the Toolkit allows blank codes.
- **PM7: RESOLVED.** OQ 3 at IMP:205, M2 at IMP:195, dogfood item at PL:202. M2's scope has its own new Minor, PM-R2-m5.
- **PM8: RESOLVED.** UJ 3's paragraph states the format facts (IJ:104), and TK-3 is written in scientific notation (IJ:106). Nit: IJ:106/118 write `1.04700000e+0`, which matches IJ:104's one-digit exponent, but FIX:26 says `e+00`. The fix file is the stale one.
- **PM9: PARTIAL.**
  - Resolved:
    - R6.6 lists as not checked any record whose file L, a, b can't be read or whose illuminant/observer the app can't work under yet (IMP:171).
    - A blank Nix Device leaves the model unknown (IMP:170).
    - The D65/10° case at IJ:123 would catch a build that checks under the collection's reference.
  - Still open: E43's not-checked line (IC:29) gives no reason. A D65/10° Toolkit user sees every record listed as unchecked, and Lab values that differ from the Toolkit's, with no explanation. This is Minor now.

**Minors and Nits**
- **PM-m1: RESOLVED.** Custom Collection Name is read for R6.3 and R6.11 and never stored (IMP:167). The stored column list is asserted (IJ:119).
- **PM-m2: RESOLVED.** Only the first L, a, b are read, and every other colour column is ignored (IMP:167); unreadable L, a, b means not checked (IMP:171). E48's copy now overclaims this; see PM-R2-m4.
- **PM-m3: RESOLVED.** IMP:85 now says R3.8a–q, and the every-state row covers E40–E49 (IJ:36).
- **PM-m4: RESOLVED.** E43 offers neither read control for a Toolkit export (IMP:166, IC:29), with carve-outs at IMP:50 and IMP:58. E5 still has an unspecified corner; see PM-R2-m3.
- **PM-m5: RESOLVED.**
  - A quarantined current reading falls under R6.8b (IMP:184).
  - A same-dated reading with a different spectrum goes to history and is listed (IMP:187).
  - A set-aside item is captured and listed (IMP:184, IC:29).
- **PM-m6: RESOLVED.** "Hold no reading" is defined as current or in history, QC records aside (IMP:169).
- **PM-m7: RESOLVED.** A Toolkit-created target's mode can't be changed in that creation (IMP:168, IJ:114).
- **PM-m8: RESOLVED.** E14 has a Toolkit variant (IC:24). Its new sentence has a precision issue; see PM-R2-m1.
- **PM-m9: RESOLVED.** Reading and canonical value are defined for an imported reading (DF:37, DF:112, AG:108–109).
- **PM-m10: RESOLVED.** R2.1 names the history's measured order as one of the over-time views (DF:112). DJ6 asserts both orders (DJ:115–116), and CMJ:237 (UJ2.1-s) tells them apart.
- **PM-m11: RESOLVED.** The R4.2d copy line lists samples, averaging, spread and agreement as not recorded (CMC:295).
- **PM-m13: PARTIAL.**
  - IC:32 dropped the unverified "export each mode" remedy, but put nothing in its place (see PM-R2-m2).
  - The second half was not answered and is not recorded in FIX:39: switching the collection's scan mode away later (CAP:147, DF R3.3d) blanks every imported chip with no warning.
- **PM-m12: RESOLVED.** "Measurement condition" is linked to the Toolkit's "Measurement Mode" (IC:29, IC:32).
- **PM-m14: RESOLVED (deferred).** PL:103 records it with a revisit trigger (FIX:62). The deferral is sound because re-scan is an owner-set P1 and the Flag path exists at P0.
- **PM-m15: RESOLVED.** ADR-0003 inputs at PL:169, help docs at PL:217, dogfood at PL:202.
- **PM-m16: RESOLVED.** M8 leaves out imports whose items arrive captured (CAP:420).
- **PM-m17: RESOLVED.** Fixed at IMP:5 and IMP:21.
- **PM-n1: PARTIAL.** The heading at VIS:138 still reads "(cataloger; v2 candidate)"; only the risk bullet at VIS:144 is marked superseded. If the old heading is kept for F69's anchor, record that. Otherwise rename it and update the anchor.
- **PM-n2: RESOLVED.** The reading counts now have their own labels (IC:29).

##### New findings (round 2)

**[MINOR] PM-R2-m1: two E43 and E14 lines mislead on exactly the outcomes users find counterintuitive.**
- **Where:** IC:29 (E43 Toolkit variant: "Kept as earlier readings: ⟨history⟩ — a swatch you scanned here keeps that scan"; "…are kept as earlier readings") and IC:24 (E14 Toolkit variant: "a swatch you scanned here keeps that scan as its colour").
- **Defect 1, "earlier":** F84 changed R6.8c (IMP:185) to say "not current" instead of "earlier", because a reading kept behind a scan can have a *later* Date Saved than the scan (DJ:116). The copy still says "earlier".
- **Defect 2, "a swatch you scanned here":** this is false for a Demo Device scan, which F86/N18 lets a Toolkit reading replace (IMP:186).
- **Scenario:** a Cataloger scanned 10 swatches in February, re-measured them in the Toolkit in March, and re-imports. The preview tells them their March readings were kept as "earlier readings". History's measured view then lists them *after* the scan. The owner's N6 choice (a scan stays current) is fine, but the copy misstates it.
- **Fix:**
  - Rewrite the E43 line as "Kept in history, not used as the colour: ⟨history⟩ — a swatch you scanned here with an instrument keeps that scan, even where the Toolkit reading is newer."
  - Use "kept in history, not used as the colour" in the clashes line.
  - Rewrite E14's clause as "…a swatch scanned here with an instrument keeps that scan as its colour."

**[MINOR] PM-R2-m2: E46's mixed-modes variant is now a dead end.**
- **Where:** IC:32. After the unverified remedy was dropped, the variant explains why the file is refused but offers only "Pick the file again" and "Cancel".
- **Scenario:** a user who changed the Toolkit's Measurement Mode partway through a collection can't import any of it, and the app gives no path forward. They can't fall back to importing the file as an inventory without readings either, because recognition is automatic (R6.1).
- **Fix:** keep N8/F76's refusal as it is. Add one honest pointer, for example "Keep each measurement condition's colours in their own Nix Toolkit collection and export each." Mark it provisional under OQ 3 until the corpus shows whether the Toolkit can do this.
- **Also open:** the missing warning when a collection's mode is later switched away from its imported readings (PM-m13, second half).

**[MINOR] PM-R2-m3: E5 on a recognised Toolkit export is unspecified.**
- **Where:** IMP:166 says the encoding and separator are "both shown fixed". E5 (IC:15) still offers "Choose an encoding" and "Choose a separator" and says "save the file again from your spreadsheet". R3.8a (IMP:116) says "where offered". The copy's Toolkit remedy rule (IC:39) covers E7, E9, E10 and E42 but not E5.
- **Scenario:** OQ 3 itself asks how the Toolkit writes a quote inside a name. A colour named with an unescaped `"` sends a recognised Toolkit export to E5's syntax variant. If the build offers "Choose a separator" and the user picks comma, the file drops to §1–§3. The result is pending items plus about 50 junk metadata columns: the lossy fallback PM5 was meant to close.
- **Fix:** say whether E5 offers the read controls for a Toolkit export (recommend withholding both), and add E5 to IC:39's rule: "Correct it in the Nix Toolkit and export it again".

**[MINOR] PM-R2-m4: E48 claims values are checked that R6.2 ignores.**
- **Where:** E48 (IC:34) says "The file's own Lab, LCh, XYZ, sRGB and HEX are checked, not kept". R6.2 (IMP:167) ignores LCh, XYZ, sRGB and HEX, and R6.6 (IMP:171) compares Lab only.
- **Fix:** "The file's own Lab is checked against SpectroCapture's; its Lab, LCh, XYZ, sRGB and HEX aren't kept — SpectroCapture works every colour out again from the spectrum."

**[MINOR] PM-R2-m5: M2 counts some lossy imports as clean and some clean ones as failures.**
- **Where:** M2 (IMP:195) counts an import as clean if it had no E46, E47, E49 or R6.6 flag.
- **Not counted but should be:** E5, E7, E9 and E10. These are exactly the variances OQ 3 asks about (IMP:205): blank or repeated codes, and a quote or delimiter inside a name. A Toolkit export that loses rows to E9 or E10 and then commits counts as "clean".
- **Counted but shouldn't be:**
  - D65/10° records that are "not checked" by design (N22, an interim scope limit, not format variance);
  - E46's collection variant, which is the owner's choice of target, not a fault in the file.
- **Fix:** define clean as "no record excluded for any reason (E5, E7, E9–E11, E47), no E46 mixed-modes or E49 refusal, and no R6.6 differing record". Report not-checked records separately under OQ 21.

**[MINOR] PM-R2-m6: DF R5.5d's salvage tie-break contradicts F86 and R2.3j.**
- **Where:** DF:187: "the later-recorded stays current unless it is imported and the other is not".
- **Defect:**
  - When the other current reading is simulated, this rule makes the Demo reading current over the real imported one. F86 and R2.3j (DF:137) say the opposite.
  - When the imported reading loses to a live one, it is "retained as correction-unconfirmed". That puts a correction question on a reading R2.3j keeps behind the scan with reason initial and "no correction question".
- **Scenario:** rare. It needs a damaged file carrying two current readings, and the user would see and could answer the question. It is still a one-line contradiction of a settled fence.
- **Fix:** "…unless it is imported and the other is of the live kind, or it is simulated and the other imported; an imported reading so retained keeps reason initial."

**[MINOR] PM-R2-m7: stale fixture range.**
- **Where:** EX:145 (Export R4.2), EX:180 (Export's Data Foundation obligation line) and DF:281 (DF's Data Export obligation line) still say "R7.7a–n". R4.2 now asserts "all three … `sc_imported` values", and only R7.7o (DF:268) holds a true one.
- **Why only Minor:** EX journey line 14 names R7.7o, so a journey-driven build still catches it. This belongs to the test and interface lenses.
- **Fix:** change all three to "R7.7a–o".

**[NIT] PM-R2-n1: "Unchanged" reads as "nothing happened".** In the Toolkit variant of E43 (IC:29), the rows "Unchanged: ⟨unchanged⟩" line counts pending items that just became captured with colours (IJ:130 inventory-first flow). Label it "Details unchanged" in the Toolkit variant.

#### Biggest risks   (what builds the wrong thing or fails the user)
1. **Copy that misstates the two counterintuitive outcomes (PM-R2-m1).** A newer Toolkit reading doesn't replace a scan, and a Demo reading doesn't count as a scan. These are exactly the moments a Cataloger reads the preview closely, and today it tells them the wrong story.
2. **Format variance is still one export deep.** It is now tracked honestly by OQ 3, M2 and the dogfood item. But M2's definition misses the very row losses OQ 3 asks about (PM-R2-m5), and E5 on a recognised export can still drop a user into the lossy generic path (PM-R2-m3).
3. **Refusals without a path.** E46's mixed-modes variant (PM-R2-m2) blocks an entire collection with no stated way out. Frequency is unknown until OQ 3's corpus exists.

#### Genuinely solid   (incl. where simplicity is right that a product-zealot would over-spec)
- **Re-import is now safe.** R6.8f checks against every reading the item holds, which makes re-import idempotent. The cases that matter are covered: a repeat over a scan, newer-then-older, Flag then re-import, and a restore carrying its source's kind. This was the Blocker, and it is closed properly, not patched.
- **The reading counts partition cleanly** (current, kept, already here) and sum to the reading total. E43 lists every decision the user might not expect: a mode change, set-aside items captured, same-dated readings that differ, differing and unchecked records.
- **Mode adoption is honest.** It happens inside the commit, is undone by Cancel or E44, is re-checked at commit, and is named in the preview.
- **The format facts are in the repo with no data values**, the synthetic definition is strict, and the fixture's own values come from an independent reference. This protects the owner's data and gives the builder a falsifiable contract.
- **Right-sized for a one-user tool.** OQ 3 and M2 track format variance with a small owner corpus and no instrumentation or funnel. Deferring the near-miss state until the corpus shows which near-misses happen is the right call; designing it now would be speculative.
- **The history-order resolution (N16) changed no locked row** and gave DJ6 and UJ2.1-s cases that tell the two orders apart.
- **The mirrors are consistent across siblings:**
  - DF's reading and canonical-value definitions and its predecessor rule (R2.3h);
  - Collection Mode's E12, detail and history lines;
  - Export's `sc_imported` in R1.1h, i, s and t, with EJ asserting model and measured-at;
  - Capture M8's exclusion.

#### Missing / over-specified
**Missing:**
- An honest remedy on E46 mixed-modes (PM-R2-m2).
- Why E43 shows readings as not checked (PM9 residual).
- A warning when a collection's mode is switched away from its imported readings (PM-m13 residual).
- E5's behaviour for a Toolkit export (PM-R2-m3).
- A corrected salvage tie-break (PM-R2-m6).
- The R7.7a–o range at three sites (PM-R2-m7).

**Over-specified:** nothing material. R6.7's value grammar and R6.1's delimiter order look mechanism-heavy, but both are the build contract for a single-source format, so they earn their place. E43's Toolkit variant is long in full, but Capture §12's zero-count rule means the common first import shows only two or three lines.

| Row ID | disposition |
| :--- | :--- |
| Import-R1.5 | ALIGN |
| Import-R3.2 | ALIGN |
| Import-R3.3 | ALIGN |
| Import-R3.5 | ALIGN |
| Import-R3.8 | ALIGN |
| Import-R6.1 | OBJECT (PM-R2-m3) |
| Import-R6.2 | ALIGN |
| Import-R6.3 | ALIGN |
| Import-R6.4 | ALIGN |
| Import-R6.5 | ALIGN |
| Import-R6.6 | ALIGN |
| Import-R6.7 | ALIGN |
| Import-R6.8 | ALIGN |
| Import-R6.9 | ALIGN |
| Import-R6.10 | ALIGN |
| Import-R6.11 | ALIGN |
| Import-M2 | OBJECT (PM-R2-m5) |
| Import-E7 | ALIGN |
| Import-E9 | ALIGN |
| Import-E10 | ALIGN |
| Import-E14 | OBJECT (PM-R2-m1) |
| Import-E42 | ALIGN |
| Import-E43 (Toolkit variant) | OBJECT (PM-R2-m1, PM9 residual, PM-R2-n1) |
| Import-E46 | OBJECT (PM-R2-m2, PM-m13) |
| Import-E47 | ALIGN |
| Import-E48 | OBJECT (PM-R2-m4) |
| Import-E49 | ALIGN |
| DF-R1.2 | ALIGN |
| DF-R1.6 | ALIGN |
| DF-R2.1 | ALIGN |
| DF-R2.2 | ALIGN |
| DF-R2.3 (with R2.3h, R2.3j) | ALIGN |
| DF-R2.4 | ALIGN |
| DF-R5.5 (with R5.5d) | OBJECT (PM-R2-m6) |
| DF-R7.1 | ALIGN |
| DF-R7.2 | ALIGN |
| DF-R7.7 (with R7.7o) | ALIGN |
| Device-R1.21 | ALIGN |
| Device-R1.22 | ALIGN |
| CM-R2.4 (with R2.4j) | ALIGN |
| CM-R2.7 | ALIGN |
| CM-R4.2 (with R4.2d) | ALIGN |
| CM-R5.2 (with R5.2b/e) | ALIGN |
| CM-R8.9 | ALIGN |
| CM-M2 | ALIGN |
| CM-E12 | ALIGN |
| Export-R1.1 (with R1.1h/i/p/s/t) | ALIGN |
| Export-R1.2 | ALIGN |
| Export-R4.2 | OBJECT (PM-R2-m7) |
| Export-E1 | ALIGN |
| Capture-R1.10 | ALIGN |
| Capture-M8 | ALIGN |

### staff-software-engineer

#### Verdict
Ready to proceed once the one new Major is fixed. The Major is UJ 3 :132, which contradicts R6.4 and R3.8k. Every round-1 Blocker and Major is resolved except M8, which is partly resolved and leaves a Minor-sized residual. The fix pass added one Major and twelve Minors. Most of the Minors are one-clause edits.

#### What I reviewed
- **Artifact:** a requirements-mode delta. Round 2 of the Nix Toolkit import amendment: subject f7cbde5, round 1 at 2a54fce, base 6374538. I read `git diff 2a54fce f7cbde5 -- docs AGENTS.md` and then the changed rows in full.
- **Files.** All paths are under `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/`.
  - Abbreviations: `IMP` = `docs/product/import/prd-inventory-import.md`, `IMP-J` = its `-journeys.md`, `IMP-C` = its `-copy.md`, `IMP-F` = its `-fences.md`; `DF` = `docs/product/data-foundation/prd-data-foundation.md`, `DF-J` = its `-journeys.md`, `DF-F` = its `-fences.md`; `CM` = `docs/product/collection-mode/prd-collection-mode.md`, `CM-C` = its `-copy.md`; `EXP` = `docs/product/export/prd-data-export.md`.
  - Read in full: IMP, IMP-C, IMP-J UJ 3, IMP-F F69–F91, DF, DF-J DJ6, DF-F F64–F70, and the round-1 fix file `docs/product/import/prd-inventory-import-round-1-fixes.md`.
  - Also read: my round-1 section and the owner decisions N16–N23 in `docs/agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md`.
  - Read the changed rows and copy lines in CM (R2.4, R2.7, R4.2, R5.2, R5.3, R8.9, M2, the copy lines, UJ2.1-r/s, F221–F223), EXP (R1.1h–t, R1.2, R4.2, OQ 4, E1, EJ1), Capture R1.10, M8 and §12, and Device R1.21/R1.22.
  - Read the diffs to AGENTS.md, docs/decisions/README.md, post-lock, the PRD index and the vision.
- **Count-only leak check (B3), no value printed.**
  - Every Fixture T timestamp and name, and the DJ6 and variant timestamps, returned 0 hits against the local export the format report names (outside the repo).
  - No distinct Custom Collection Name, Color Name, Color Code or Date Saved value from that export (six characters or longer) appears in any added line of `git diff 6374538 f7cbde5 -- docs AGENTS.md`.
- **Could not verify:**
  - the rule-14 word counts
  - how the Toolkit escapes an embedded `"`, `;` or newline (OQ 3)
  - whether other Toolkit versions or settings change the column set, the grid or the number forms
  - the Spectro 2 grid (Export OQ 4)
  - how far the app's derivation sits from any independent colour library, which R2-m7 turns on

#### Findings

##### Round-1 findings: status
- **B1 re-import duplicates: RESOLVED.** IMP:182 (R6.8f, "current or in history … A Flag, a restore or a set-aside decision stands"), IMP:23, DF:137 ("none when the item already holds the same reading"). My three scenarios are IMP-J:134, :137 and :139, plus DF-J:120. R3.3 (IMP:80) and R6.9 (IMP:174) now agree.
- **B2 number format not in repo: RESOLVED.** IMP-J:104 states the format: bare `;`, the 59-column unquoted header (28 named columns plus 31 bands, which checks out), the four quoted columns, the `d.dddddddde±d` form, XYZ 0–1, sRGB integers, lowercase HEX, two-place densities, and the Date Saved form. R6.10 (IMP:175) cites it. TK-3 is `1.04700000e+0` (IMP-J:106), and IMP:172 accepts exponents.
- **B3 real values in the fixture: RESOLVED.** Fixture T's values are invented (IMP-J:106); synthetic is defined at IMP:175 and IMP-F:174; the count-only check above found no hits.
- **M1 E43 count contradiction: RESOLVED.** IMP:174 partitions the counts; IMP-J:117 and IMP-J:133 now read "2 used as colour (TK-2, TK-3)".
- **M2 equality and ordering: RESOLVED.** IMP:23; IMP:187 (same date, different spectrum → history); DF:137 ("imported A measured earlier"); IMP-J:138, :141.
- **M3 value grammar: RESOLVED.** IMP:172; IMP:170 ("above 1.0 or below 0 included"); IMP:171 (unreadable file L, a, b → not checked); IMP-J:125. The residual precision gap is R2-m6.
- **M4 mode/pair sets and check order: RESOLVED.** IMP:172 (exactly M0, M1 or M2); IMP:169 ("R6.7's exclusions run first"); IMP:171 (a pair the app can't work under → not checked, N22); IMP-J:123, :126. Residuals are R2-m3 and R2-m11.
- **M5 when the mode is adopted: RESOLVED.** IMP:169 (written by the commit, undone with it); IMP:168 (mode fixed in creation); Capture R1.10; IMP-J:131. The commit-time recheck case added alongside it created R2-M1.
- **M6 history order: RESOLVED.** DF:112; DF F66; IMP:185 ("not current"); DF-J:115–116 now tell the two orders apart; CM UJ2.1-s.
- **M7 DF reading model: RESOLVED.** DF:37, :99, :112, :137, :239. AGENTS.md §8. The ADR-0003 row input and its post-lock item.
- **M8 no imported fixture: PARTIAL.** DF:268 (R7.7o), DF:245 ("R7.7a–o"), DF:240 ("R2.3a–j") and EXP:145 (`sc_imported` values) all landed. But EXP:145 still enumerates "DF R7.7a–n" and DF:281 still says "goldens on R7.7a–n". See R2-m9.
- **m1 Custom Collection Name: RESOLVED.** IMP:167, IMP:176, IMP-J:119.
- **m2 read controls on E43: RESOLVED.** IMP:166; IMP-C:29 lists the Toolkit actions as "Import; Cancel". E5 is a separate gap, R2-m2.
- **m3 tolerance cite: RESOLVED.** IMP:171, IMP:32.
- **m4 vocabulary: RESOLVED.** IMP:21–22.
- **m5 R2.4 carve-out: RESOLVED.** DF:116.
- **m6 CM rule vs copy: RESOLVED.** CM-C:295 and :310 against CM:348.
- **m7 placeholders: RESOLVED.** IMP-C:37 and Capture §12.
- **m8 wavelength grid: RESOLVED, deferral sound.** EXP:205; the post-lock spike item; OQ 3 (IMP:205) covers Toolkit grid variance; extra bands are listed as added columns (IMP:167), not silent.
- **m9 R6.8 edge cases: RESOLVED.** IMP:173 keys on the snapshot kind, with a restore carrying its source's; IMP:184 and IMP:186; IMP-J:140.
- **m10 illuminant/observer as provenance: RESOLVED.** IMP:170. The creation form's pair is Capture §1's, by IMP:162's "every rule … not replace[d] still applies".
- **m11 performance: RESOLVED, deferral sound.** The post-lock dogfood item puts a ROWS_TARGET Toolkit file in the budget run; IMP:32 routes budgets to Capture OQ 13.
- **m12 ignored columns: RESOLVED.** IMP:167.
- **m13 quoting and `undefined`: PARTIAL.** Quoting is deferred to OQ 3 (IMP:205), which is sound. The question of `undefined` in Color Name, Color Code or Custom Collection Name is recorded nowhere. R6.2's Note-only wording makes the build deterministic (stored literally), so it only needs adding to OQ 3's closer list.
- **n1 Data Export obligation line: RESOLVED.** IMP:151.
- **n2 double "Unchanged": RESOLVED.** IMP-C:29, "Already here".
- **n3 R2.3 lead sentence: RESOLVED.** DF:115.
- **n4 bare `;`: RESOLVED.** IMP-J:104.

##### New in f7cbde5

**[MAJOR] R2-M1 — Import R3.8k / R6.4 against UJ 3 :132: changing an empty target's mode between preview and Import.**
- **Where:** IMP-J:132, new in this commit; R6.4 at IMP:169; R3.8k at IMP:126; F76 Clarified at IMP-F:208.
- **The contradiction.**
  - :132 starts from "an existing M2 target holding only pending items", sets the target to M1 after the preview, then expects E46 at Import.
  - At commit that target uses M1 and holds no reading. That is exactly IMP-J:130's state, which adopts M2.
  - Under R6.4 the target still fits, so R3.8k's "a target that no longer fits → E46" never fires, and a literal build adopts M2.
  - That adoption was never named in E43, which R6.4 ("named in E43") and F76 Clarified ("E43 names it") require.
  - E46's own body would then tell the user to import into "one with no readings yet", which this target already is.
- **Mutation.** A build that implements R6.4 and R3.8k as written fails :132. A build that refuses on any mode change since the preview passes :132, but it:
  - fires an E46 whose remedy the target already meets;
  - has no row to justify it.
- **Fix:**
  - In R3.8k: "a target whose scan mode differs from the one E43 showed returns to a fresh preview with nothing written: E46 if it now holds a reading, else E43 naming the adoption."
  - Change :132's expected result to that fresh preview naming M1 → M2.
  - Add a sibling case whose target holds a captured item, where the same change produces E46.

**New Minors**
- **R2-m1 — E48 overclaims (IMP-C:34).** It says the file's "Lab, LCh, XYZ, sRGB and HEX are checked, not kept". But R6.2 (IMP:167) ignores all of these except L, a and b, and R6.6 (IMP:171) compares only Lab. Rewrite as "its Lab is checked against the spectrum and its other colour values set aside".
- **R2-m2 — E5 on a recognised Toolkit export.**
  - R6.1 (IMP:166) fixes UTF-8 and the delimiter, but removes the read controls from E43 only.
  - E5's text and syntax variants (IMP-C:15) still offer "Choose an encoding / Choose a separator" and advise saving again from a spreadsheet. The Toolkit remedy rule (IMP-C:39) warns against that, but it covers only E7, E9, E10 and E42.
  - The likeliest trigger is OQ 3's unverified embedded quote.
  - Fix: state E5's Toolkit behaviour (controls not offered, the Toolkit remedy), and that recognition runs on the decoded header before any data record is parsed.
- **R2-m3 — Exclusion order before the sameness checks.**
  - R6.4 (IMP:169) runs only R6.7's exclusions before the mixed-mode check, and R6.11 (IMP:176) says "every record".
  - Whether records excluded by E7, E9 or E10 count toward E46's mixed modes or E49 is unstated.
  - So one build refuses a file whose only M1 record has a blank code, while another imports the rest.
  - Fix: "R6.4's and R6.11's checks run over the records left after E7, E9, E10 and E47."
- **R2-m4 — DF R5.5d (DF:187) ranks salvaged readings against R2.3j.**
  - The rule reads "the later-recorded stays current unless it is imported and the other is not". That keeps a simulated reading current over an imported one, the reverse of R2.3j (DF:137) and DF F68 (DF-F:777, "only a live A stays current over B").
  - Between two imported readings it picks by record time, where R2.3j picks by measurement time.
  - Fix: "unless it is imported and the other is live, or both are imported and the other was measured later". Only salvage is affected.
- **R2-m5 — No rendering for an unknown model.** R6.5 (IMP:170) now leaves the model unknown when the Nix Device cell is blank. CM R4.2d (CM:348) lists every other unknown but not the model, and CM-C:295 and :307 render "model" with no unknown form. Add "model unknown where the file gave none" to R4.2d and both copy lines.
- **R2-m6 — Time and number grammar (IMP:172).**
  - "ISO 8601" includes basic and week/ordinal forms that parsers disagree on.
  - How more than three fraction digits are reduced to R6.5's millisecond (truncate or round) is unstated, and that reduction feeds R6.8f's same-instant test.
  - Fix: pin "RFC 3339 date-time, as EXP R2.2 writes, extra fraction digits truncated to the millisecond", and say the exponent takes an optional sign, which IMP-J:104's form needs.
- **R2-m7 — IMP-J:122's "0.05 ΔE2000" is hard-coded against a provisional DERIVATION_TOLERANCE** (candidate 0.1, DF OQ 6).
  - A build whose derivation is within DF M2's bound of the independent reference can still land TK-1 above 0.1 and fail the case.
  - If OQ 6 closes at or below 0.05, the case inverts.
  - Fix: state :121's and :122's offsets as multiples of DERIVATION_TOLERANCE, and tie UJ 3's reference to the one DF R7.5 checks the build against.
- **R2-m8 — E14's Toolkit variant (IMP-C:24) overpromises.** It says "a swatch you scanned here keeps that scan as its colour", but R6.8g (IMP:186) replaces a Demo Device reading, which was also scanned here. Say "scanned here with an instrument"; E43's kept-readings clause (IMP-C:29) can take the same wording.
- **R2-m9 — M8 residual.** EXP:145 enumerates "DF R7.7a–n" while requiring all three `sc_imported` values, but the true value exists only in R7.7o. DF:281 also says "R7.7a–n". Change both to a–o.
- **R2-m10 — Where E48 sits in the flow.** R2.1 (IMP:67) puts the target before mapping, and R3.8q (IMP:132) sends E48 straight to preview. Yet IMP-J:110, :111 and :113 reach E48 from "Pick the file" with no target, and :114 creates the target after the pick. Either add the target step to those actions, or say that E48 precedes target selection and fix R3.8q.
- **R2-m11 — The file's Illuminant and Observer.**
  - R6.5 (IMP:170) stores them, R6.7 never validates them, and R6.6 routes a pair the app "cannot yet work under" to not checked.
  - Two things are unstated:
    - what is stored for a blank or unrecognised cell;
    - which file strings map to the app's D50/2° (`2` versus `2°`). Only UJ 3's fixture pins `D50` and `2`.
  - Fix: "stored as the text given, empty where blank; workable when it names a pair Capture OQ 21 offers".
- **R2-m12 — Quarantined readings and the same-reading check.** R6.8f (IMP:182) and DF R2.3j (DF:137) compare against every reading the item holds, but a quarantined reading's spectrum can't be read. Whether a re-import matches it (the item stays set aside as unreadable) or recovers the item through R6.8b is unstated. Fix: "a quarantined reading is never the same reading".

**Nits**
- **R6.3 (IMP:168):** when R6.11's names differ only in case or spacing, say which record's text pre-fills the name (the first eligible record's).
- **E47 (IMP:172):** a record that fails two columns should list each failing column.
- **IMP-J:133:** TK-2's pending item's Swatch Name is unstated, so "Updated 1" rests on an unpinned value.
- **Fix file line 26:** it writes `1.04700000e+00`, but the PRD's `e+0` (IMP-J:106) is the form that matches the stated one-digit exponent.

#### Clarifying questions for the author
1. At Import, when the target's mode changed after the preview and the target holds no reading, should the app adopt the file's mode, refuse with E46, or return to a fresh preview that names the adoption?
2. Do records excluded by E7, E9 or E10 count toward the mixed-mode check (R6.4) and the one-collection check (R6.11)?
3. When a recognised Toolkit export hits E5, does E5 offer the encoding and separator controls, and does it end with the Toolkit remedy?
4. Is a Date Saved with more than three fraction digits truncated or rounded, and is ISO 8601's basic form readable?
5. In salvage, should an imported current reading outrank a simulated one, as R2.3j does?
6. Can a quarantined reading ever count as the same reading under R6.8f?
7. What is stored for a blank or unrecognised Illuminant or Observer cell?
8. How do the item detail and history show an imported reading whose Nix Device cell was blank?
9. Should a literal `undefined` in Color Name, Color Code or Custom Collection Name be treated like the Note's (add to OQ 3)?
10. Does E48 appear before target selection or after it?

#### Claimed properties
- **"Repeating a committed export changes nothing"** (R3.3, R6.9): **holds.** R6.8f checks every reading the item holds; UJ 3 :134, :135, :137, :139 and :141 cover it.
- **"Fixture T is synthetic"** (R6.10, F71): **holds.** The count-only check found zero hits.
- **"The number format is in the repo"**: **holds** (IMP-J:104).
- **"R6.9's counts partition the eligible readings"**: **holds.** Outcomes a, b, c, d (later, earlier or same-dated), f and g cover every eligible record, and excluded records and absent items stay outside the counts.
- **"R6.7's exclusions run before R6.4"**: **holds for E47.** It is unstated for E7, E9 and E10 (R2-m3).
- **"Adoption is written by the commit and undone with it"**: **holds for Cancel and E44** (IMP-J:131). The commit-time recheck contradicts R6.4 at IMP-J:132 (R2-M1).
- **"`sc_imported` lands in v1's golden"**: **holds in substance** through R7.7o. The enumeration ranges at EXP:145 and DF:281 lag (R2-m9).
- **"Salvage keeps a scan current"** (fix file): **holds for a live reading. It does not hold for a simulated one:** R5.5d keeps the simulated reading current over an imported one (R2-m4).
- **"No aligned row reopens for compaction"**: **unverified.** The rule-14 counting method can't be reproduced here.

#### Genuinely sound
- **R6.8f leads the table and checks "current or in history".** Re-import can't duplicate a reading, and it can't undo a Flag, a restore or a set-aside decision.
- **R6.8 keys on the current reading's snapshot kind, and a restore carries its source's kind.** That gives one source of truth, shared with the imported mark, `sc_imported` and Device R1.21.
- **R6.1's signature leaves out Index and L, a and b.** Index is the column a spreadsheet re-save would prefix with a byte-order mark, and L is the duplicated header, so neither affects recognition. Recognising under `;`, then `,`, then tab is correctly conservative.
- **R6.10's independent-reference rule** removes the circular oracle.
- **DF R2.3h** fixes the predecessor as "the reading current when B landed". The ADR-0003 input correctly names explicit current selection as the schema's one-way door.
- **No operating envelope is needed for §6.** Derivation from 31 bands is trivial next to IMPORT_BUDGET, and the post-lock budget run covers it. A process-zealot asking for a separate §6 envelope would be over-specifying.
- **The deferrals are sound:** the near-miss state, the grid, a user label in Nix Device, performance, and quoting (OQ 3).

#### Deferred
- **peer-product-marketing-manager-reviewer:** the wording for R2-m1 (E48), R2-m2 (E5's Toolkit remedy) and R2-m8 (E14 and E43 for Demo readings).
- **peer-test-reviewer lens:** R2-m7's tolerance-relative offsets, and the unpinned TK-2 name at IMP-J:133.
- **peer-database-reviewer:** how the provenance pair in R2-m11 is typed, and whether "the same number" stays exact in REAL storage.
- **peer-architecture-reviewer (ADR-0003):** the same-reading key "excluding restores", and whether quarantine enters that key (R2-m12).
- **peer-product-manager-reviewer:** whether `undefined` in names and codes should count as absent (m13 residual).

| Row ID | disposition |
| :--- | :--- |
| Import-R1.5 | ALIGN |
| Import-R3.2 | ALIGN |
| Import-R3.3 | ALIGN |
| Import-R3.5 | ALIGN |
| Import-R3.8 | OBJECT (R2-M1) |
| Import-R6.1 | OBJECT (R2-m2) |
| Import-R6.2 | ALIGN |
| Import-R6.3 | ALIGN |
| Import-R6.4 | OBJECT (R2-M1, R2-m3) |
| Import-R6.5 | OBJECT (R2-m11) |
| Import-R6.6 | OBJECT (R2-m7) |
| Import-R6.7 | OBJECT (R2-m6) |
| Import-R6.8 | OBJECT (R2-m12) |
| Import-R6.9 | ALIGN |
| Import-R6.10 | ALIGN |
| Import-R6.11 | OBJECT (R2-m3) |
| Import-M2 | ALIGN |
| Import-E7 | ALIGN |
| Import-E9 | ALIGN |
| Import-E10 | ALIGN |
| Import-E14 | OBJECT (R2-m8) |
| Import-E42 | ALIGN |
| Import-E43 (Toolkit variant) | ALIGN |
| Import-E46 | ALIGN |
| Import-E47 | ALIGN |
| Import-E48 | OBJECT (R2-m1, R2-m10) |
| Import-E49 | ALIGN |
| DF-R1.2 | ALIGN |
| DF-R1.6 | ALIGN |
| DF-R2.1 | ALIGN |
| DF-R2.2 | ALIGN |
| DF-R2.3 (with R2.3h, R2.3j) | OBJECT (R2-m12) |
| DF-R2.4 | ALIGN |
| DF-R5.5 (with R5.5d) | OBJECT (R2-m4) |
| DF-R7.1 | ALIGN |
| DF-R7.2 | ALIGN |
| DF-R7.7 (with R7.7o) | ALIGN |
| Device-R1.21 | ALIGN |
| Device-R1.22 | ALIGN |
| CM-R2.4 (with R2.4j) | ALIGN |
| CM-R2.7 | ALIGN |
| CM-R4.2 (with R4.2d) | OBJECT (R2-m5) |
| CM-R5.2 (with R5.2b/e) | OBJECT (R2-m5) |
| CM-R8.9 | ALIGN |
| CM-M2 | ALIGN |
| CM-E12 | ALIGN |
| Export-R1.1 (with R1.1h/i/p/s/t) | ALIGN |
| Export-R1.2 | ALIGN |
| Export-R4.2 | OBJECT (R2-m9) |
| Export-E1 | ALIGN |
| Capture-R1.10 | ALIGN |
| Capture-M8 | ALIGN |

### test

#### Verdict
Trustworthy once four Majors are fixed. No Blocker remains. All three round-1 Blockers are resolved, and the R6.8a–g cases (R6.8f included) now reject the wrong builds they target. Four oracles are still weak: a commit-recheck case whose expected result contradicts R6.4, an unchecked-record case that depends on how Capture OQ 21 closes, an Export golden range that leaves out R7.7o, and a Collection Mode history case whose data cannot tell the two sort orders apart.

#### Coverage map (brief)
Paths below are under `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/`. IMP = `product/import/prd-inventory-import.md`, IJ = `…-journeys.md`, IC = `…-copy.md`, IF = `…-fences.md`; DF, DJ = `product/data-foundation/prd-data-foundation{,-journeys}.md`; DMJ = `product/device-management/prd-device-management-journeys.md`; CM, CMJ, CMC = `product/collection-mode/prd-collection-mode{,-journeys,-copy}.md`; EX, EXJ, EXF = `product/export/prd-data-export{,-journeys,-fences}.md`; CAP = `product/capture-mode/prd-capture-mode.md`; FIX = `product/import/prd-inventory-import-round-1-fixes.md`.

The fixture format in IJ:104 was checked against the format section of the local research report only, plus my round-1 probe for the bare `;`. No data value was read into this review or quoted in it.

Covered, with cases that fail a wrong build:
- **R6.1:** `;` (IJ:110), comma re-save (IJ:111), missing signature column (IJ:112), extra column (IJ:113).
- **R6.2:** every column's fate (IJ:119); `undefined` treated as omitted, plus a real Note (IJ:120).
- **R6.3:** IJ:114–116.
- **R6.4:** mixed modes (IJ:127), E47 before E46 (IJ:126), refuse (IJ:129), adopt (IJ:130), Cancel and E44 (IJ:131).
- **R6.5:** IJ:118, including both gamut flags.
- **R6.6:** over tolerance (IJ:121), under tolerance (IJ:122), not checked (IJ:123).
- **R6.7:** `abc`, NaN, a time with no zone, an empty mode; plain decimals and a `+00:00` offset read correctly (IJ:124–126, IJ:141).
- **R6.8a–g:** a (IJ:117, :133), b (:133, :140), c (:133, with the Toolkit reading newer than the scan), d later, earlier and tie (:136, :138), e (:142), f (:134, :135, :137, :139, :141), g (:140).
- **R6.9:** counts at IJ:117, :133, :134, :136. **R6.11:** IJ:128.
- **DJ6:** 11 rows, DJ:113–123. Also DMJ:80, CMJ:236–237, EXJ:14.

Load-bearing gaps:
- The commit recheck (IJ:132 asserts the wrong outcome).
- The unchecked-pair oracle (IJ:123 depends on OQ 21).
- `sc_imported` in the golden range (EX:145 still says a–n).
- History order as Collection Mode renders it (CMJ:237 cannot tell the orders apart).
- No case for: a target holding a reading only in history; R6.8b's quarantined arm; a negative reflectance, an empty reflectance cell, or a blank Nix Device; the file's own illuminant and observer kept as provenance; R5.5d's imported exception.

#### Findings

**Round-1 findings — status**

Blockers:
1. B1, E43 count contradiction — RESOLVED. IMP:174 partitions the eligible records; IC:37 defines the counts so they sum to ⟨readings⟩; IJ:117 ("3 used as colour") and IJ:133 ("2 … and 1 kept") agree.
2. B2, re-import onto scanned items — RESOLVED. R6.8f runs first, "current or in history" (IMP:182, IMP:23, IF:246). Cases: IJ:134 (TK-1 still holds exactly two readings), IJ:137, IJ:139, DJ:120.
3. B3, fixture not pinned to the real format — RESOLVED. IJ:104 pins the format and IJ:106 writes TK-3 as `1.04700000e+0`; IMP:175 cites UJ 3's format. IJ:125 and IJ:141 also make plain decimals readable, so neither a scientific-only nor a decimal-only reader passes.

Majors:
4. History-order oracle — PARTIAL. The Data Foundation side is fixed: DF:112, and DJ:115–116 test both directions. The Collection Mode case CMJ:237 cannot tell the orders apart (TR2-M4).
5. Scan stays current when the Toolkit reading is newer (N6) — RESOLVED at IJ:133 (scan 2026-02-01, TK-1 later) and DJ:116. FIX does not cite this finding by ID.
6. R6.8d's tie and the reason it records — RESOLVED: IMP:187, DF:137, IJ:136, IJ:138, DJ:121.
7. Gamut-flag oracle — RESOLVED: IJ:106 declares TK-1 inside sRGB and TK-2 outside; IJ:118 and DJ:113 assert the flags. Margin caveat in TR2-m2.
8. R6.7 only one-third tested — RESOLVED: IMP:172's value grammar, IMP:169's ordering, IJ:125, IJ:126; unreadable file Lab and pair go to R6.6 and IJ:123 (see TR2-M2). Remaining gaps in TR2-m5.
9. `sc_imported` missing from the golden — PARTIAL. DF:268 adds R7.7o, EX:145 names the values and EXJ:14 asserts them, but EX:145 still enumerates only R7.7a–n (TR2-M3).
10. `undefined` Note clearing a typed Note — RESOLVED: IMP:167, IF:188, IJ:120.

Minors:
11. R6.1 negative and superset cases — RESOLVED: IJ:112, IJ:113.
12. Custom Collection Name's fate — RESOLVED: IMP:167, IJ:119.
13. No real Note tested — RESOLVED: IJ:120 (`Glossy`).
14. No E41 or E12 for the two L headers — RESOLVED: IJ:110. FIX does not cite it by ID.
15. Existing target keeps its name; pre-filled name collides — RESOLVED: IJ:115, IJ:116.
16. Gaps around R6.4's mode switch — PARTIAL. E43's mode line (IC:29, IJ:130), Cancel and E44 (IJ:131), the "hold no reading" definition (IMP:169) and R3.8k's recheck (IMP:126) all landed, but the recheck case IJ:132 asserts the wrong outcome (TR2-M1).
17. Below-tolerance case — RESOLVED: IJ:122 (see TR2-m1).
18. What ⟨history⟩ and ⟨readings⟩ count — RESOLVED: IMP:174, IC:37, IJ:136.
19. R3.8a–q and E46/E47 actions — RESOLVED: IMP:85, IJ:36.
20. Whether the read controls override detection — RESOLVED for E43 (IMP:166). E5 remains open (TR2-m10).
21. R6.8b and R6.8c variants — PARTIAL. Set aside (IJ:140), flagged (IJ:139) and simulated (IJ:140) are covered; a quarantined current reading is not (TR2-m7).
22. DF:37 vocabulary — RESOLVED: DF:37, IJ:118, DJ:113.
23. DF R1.2 and R7.1 lists — RESOLVED: DF:99, DF:239, DJ:114.
24. Illuminant and observer placement vs Capture R4.6 — PARTIAL. IMP:170 reconciles the placement, but no oracle tells the file's pair apart from the collection's (TR2-m3).
25. CMC:295 copy line — RESOLVED: CMC:295.
26. CMJ:236 mode — RESOLVED: "measured in M1".
27. Collection Mode M2's population — RESOLVED: CM:574.
28. Export R1.1h/i/s and EJ1's thin assertions — RESOLVED: EX:88–90, EX:94, EXJ:14.
29. Wavelength-grid assumption — RESOLVED: EX:205 (OQ 4) and the post-lock item.
30. Creation-time mode — RESOLVED: CAP:147, IMP:168, IJ:114. FIX does not cite it by ID.

Nits:
31. Zero-count phrasing — RESOLVED: IJ:117.
32. §4 naming UJ 3 — RESOLVED: IMP:136.
33. DJ6 index listing R3.3 — RESOLVED: DF:27.

**New findings**

[MAJOR] TR2-M1, IJ:132 vs IMP:169 (R6.4) and IMP:126 (R3.8k) — the commit-recheck case expects a refusal that R6.4 forbids.
- Given: an M2 target holding only pending items; after preview its mode is set to M1; then Import. The case expects E46.
- Under R6.4, a target that "hold[s] no reading" fits by adopting the file's mode. So at commit the target still fits, and R3.8k's "a target that no longer fits → E46" does not fire.
- A build that follows R6.4 adopts M2 and commits. It fails the case, and it also commits an adoption E43 never named, which R6.4 requires.
- A build that checks against the previewed mode passes the case but breaks R6.4. The builder has to guess.
- Fix: change the Given to "an existing M2 target holding a captured item". A target with a reading that is switched to M1 no longer fits, so E46 is unambiguous. Separately, say in R3.8k that a target needing an adoption E43 did not name returns to a fresh preview (E13).

[MAJOR] TR2-M2, IMP:171 (R6.6 "cannot yet work under") and IJ:123 — the not-checked case depends on a state nobody declares, and on how an open question closes.
- Only Capture OQ 21's interim list of one offered pair makes D65/10 unworkable (CAP:446).
- CAP:142 requires derivation to be reference-parameterised "even while OQ 21's offered list is one pair". R6.6 does not say whether "can't work under" means "not an offered pair" or "no derivation tables for it".
- A build that ships D65/10 tables checks TK-2, finds a match, and lists it as neither differing nor not checked, so it fails IJ:123. When OQ 21 closes with the toolkit's reference whites, every build fails IJ:123.
- An injection path already exists: Capture R11.6 (CAP:352) lets a test declare the offered pair set.
- Fix: R6.6 should read "…whose illuminant and observer are not a pair the app offers (Capture R1.5, OQ 21)…". IJ:123's Given should add "offered pair set declared as D50/2° alone (Capture R11.6)". Optionally add a twin case with D65/10 offered, where TK-2 is checked and not listed.

[MAJOR] TR2-M3, EX:145 (R4.2 "Enumerate DF R7.7a–n plus generated corpus") vs EXF:225 (F35 Clarified: "a golden cut from … R7.7o") and DF:245 ("R7.7a–o own the inventory") — the amended R4.2 contradicts its own fence.
- EX:180, EXJ:3 ("R7.7a–n is the sole fixture inventory") and DF:281 (Data Foundation's Data Export obligation line) still say a–n.
- R7.7o is the only fixture holding an imported reading. A harness that enumerates R4.2's range cuts no golden with `sc_imported` true.
- Mutation: a build that always writes `sc_imported` false or empty stays byte-identical to every a–n golden. Only EXJ:14 would catch it, and EXJ:3 tells that case's author it may compose only a–n fixtures.
- Fix: change a–n to a–o at EX:145, EX:180, EXJ:3 and DF:281.

[MAJOR] TR2-M4, CMJ:237 (UJ2.1-s) — the case that FIX:11 says carries N16 into Collection Mode cannot fail the build N16 rules out.
- L1 is measured 2026-02-01; I2 is measured 2026-03-14 and recorded after L1. Record order and measurement order therefore agree.
- Build W1 sorts the default (restore-variant) list newest-measured first, the misreading F78's "Why" line still invites (IF:219). It lists I2 then L1, the expected result.
- Build W2 sorts the measured variant by record time, oldest first. It lists L1 then I2, also the expected result. Both pass.
- DJ:115–116 do discriminate, but only in SQL; nothing checks the rendered E17 view. The case binds Collection Mode R5.3, which is aligned and outside the table below.
- Fix: add a mirror row with L1 measured 2026-09-20 and I2 measured 2026-03-14, recorded after L1. Both variants must then list I2 then L1, which fails W1 and W2, as the DJ:115/116 pair does.

New Minors:
- **TR2-m1** — IJ:122's 0.05 ΔE2000 offset uses half the candidate DERIVATION_TOLERANCE, while DF M2 lets the app sit a full tolerance from the reference, so a conforming build can list TK-1. State the offset as a fraction of the tolerance.
- **TR2-m2** — IJ:106 says TK-1 is inside sRGB and TK-2 outside, with no margin, so the flags asserted at IJ:118 and DJ:113 can flip on derivation noise. State a margin, as UJ2.1-q's harness does.
- **TR2-m3** — IMP:170 keeps the file's illuminant and observer as provenance, but no case shows them apart from the collection's (Fixture T's D50/2° equals it). At IJ:123, assert that TK-2's reading records D65/10.
- **TR2-m4** — IMP:170: a negative reflectance "kept as given" and a blank Nix Device ("model unknown") have no case, so clamp-at-zero and default-model builds pass. Give one record a negative reflectance, and add a variant with Nix Device blank.
- **TR2-m5** — IMP:172: an empty reflectance cell, `Infinity` and a comma decimal have no case. A reader that treats empty as 0 passes every current R6.7 case.
- **TR2-m6** — IMP:169's "current or in history" is untested. A target whose only item is flagged with a reading in history must refuse, but a check on current readings only passes IJ:129–130.
- **TR2-m7** — IMP:184: R6.8b's quarantined-current arm has no case. A build that treats "has a current row" as having a current value routes it by snapshot kind instead.
- **TR2-m8** — IJ:133 asserts Updated 1 but never declares TK-2's pending Swatch Name or the target's columns. Declare them (e.g. `Deep Coral`, no imported columns).
- **TR2-m9** — IMP:169 and IMP:176 order only R6.7's exclusions before the mixed-mode check. Whether E7/E9/E10-excluded records count toward E46, and whether E47-excluded records count toward E49 ("every record's"), is unstated.
- **TR2-m10** — E5 (IC:15) still offers "Choose an encoding" and "Choose a separator" for a Toolkit export whose settings R6.1 fixes (IMP:166). State whether they apply, and which E5 variant a failed UTF-8 decode shows.
- **TR2-m11** — the Toolkit endings for E7, E9, E10 and E42 (IC:39) have no case that renders them. Add a Fixture T variant where TK-2's code equals TK-1's, giving E10 on records 2 and 3.
- **TR2-m12** — DF:187: R5.5d's imported exception has no case (DJ:34 is unchanged, so "always the later-recorded" passes). Its "the other retained as correction-unconfirmed" also raises a correction question over an imported reading, which DF:137 says never gets one.
- **TR2-m14** — R6.1's tab branch and a header that is only R2.3-equal (e.g. `color code`) are untested.

Nits:
- **TR2-n1** — DMJ:80 says "serial and firmware unknown", while DF:137 and IJ:118 say "empty". A build that stores the string "unknown" passes UJ3-c alone; say "empty".
- **TR2-n2** — IJ:126's "E46 … then" names no action. State that "Continue without them" leads to E46.
- **TR2-n3** — FIX:26 says TK-3 is written `1.04700000e+00`, which is off-format. The landed IJ:106 value `1.04700000e+0` is correct; fix FIX.
- **TR2-n4** — FIX does not cite round-1 items 5, 14 and 30 by ID, though all three landed.

#### Biggest risks   (what could ship broken behind a green suite)
- Collection Mode's default history list ordering imported readings by Date Saved, against N16, with UJ2.1-s still green (TR2-M4).
- `sc_imported` never reaching the release golden, because R4.2's range excludes R7.7o (TR2-M3).
- The commit recheck either committing a mode adoption the user was never shown, or refusing a target that fits — whichever way the builder guesses (TR2-M1).
- R6.6's not-checked list being wrong on any build with full derivation tables, and on every build once OQ 21 closes (TR2-M2).
- An empty reflectance stored as 0, or a negative clamped, as a canonical value with no mark (TR2-m4, TR2-m5).

#### Genuinely solid   (incl. where minimal scoping is correct that a coverage-zealot would wrongly flag)
- **R6.8f runs first against current or history,** and IJ:134/137/139/141 kill four wrong builds:
  - one that appends a reading on every re-import;
  - one that compares against the current reading only (newer-then-older, IJ:137);
  - one that runs R6.8b before R6.8f (the Flag stands, IJ:139);
  - one that compares Date Saved as a string (the `+00:00` and plain-decimal rewrite, IJ:141).
- **IJ:133** makes the Toolkit reading newer than the scan, so "newest measurement wins" fails.
- **DJ:117** makes a "predecessor = previous sequence" build mark B never-true.
- **DJ:115/116** pin both history keys against each other.
- **IJ:120** tests `undefined` as omitted and a real Note in one run.
- **IJ:126** tests the ordering of E47 before E46.
- **EXJ:14** asserts all four `sc_simulated`/`sc_imported` combinations, including empty on a never-scanned row.
- **UJ2.1-r's ZX-023** proves the imported mark follows the current reading.
- **The IJ:104 format** now matches the studied export's format facts.
- **Correctly minimal:**
  - R6.8e rests on IJ:142 plus R3.3d.
  - Capture M8 needs no case.
  - DJ6's re-scan rows follow Data Foundation's existing unmarked re-scan convention, so this amendment adds no phase debt.
  - E12's new sentence and E14's Toolkit sentence need only R4.2's shipping-string checks.
  - E13, E38 and E40 need no Toolkit-specific case.

#### Missing / over-tested
Missing:
- A corrected IJ:132 Given.
- The offered-pair declaration at IJ:123.
- a–o at EX:145, EX:180, EXJ:3 and DF:281.
- UJ2.1-s's mirror row.
- A target holding a reading only in history.
- R6.8b with a quarantined current reading.
- A negative reflectance, an empty cell and a blank Nix Device.
- The file's own pair asserted at IJ:123.
- An E10 case on a Toolkit file.
- R5.5d's imported salvage case.
- The tab and R2.3-variant detection cases.

Over-tested: none. The 33 UJ 3 rows each fail a distinct wrong build.

| Row ID | disposition |
| :--- | :--- |
| Import-R1.5 | ALIGN |
| Import-R3.2 | ALIGN |
| Import-R3.3 | ALIGN |
| Import-R3.5 | ALIGN |
| Import-R3.8 | OBJECT (TR2-M1) |
| Import-R6.1 | OBJECT (TR2-m10, TR2-m14) |
| Import-R6.2 | ALIGN |
| Import-R6.3 | ALIGN |
| Import-R6.4 | OBJECT (TR2-M1, TR2-m6, TR2-m9) |
| Import-R6.5 | OBJECT (TR2-m3, TR2-m4) |
| Import-R6.6 | OBJECT (TR2-M2, TR2-m1) |
| Import-R6.7 | OBJECT (TR2-m5) |
| Import-R6.8 | OBJECT (TR2-m7, TR2-m8) |
| Import-R6.9 | ALIGN |
| Import-R6.10 | OBJECT (TR2-m2) |
| Import-R6.11 | OBJECT (TR2-m9) |
| Import-M2 | ALIGN |
| Import-E7 | OBJECT (TR2-m11) |
| Import-E9 | OBJECT (TR2-m11) |
| Import-E10 | OBJECT (TR2-m11) |
| Import-E14 | ALIGN |
| Import-E42 | OBJECT (TR2-m11) |
| Import-E43 (Toolkit variant) | ALIGN |
| Import-E46 | OBJECT (TR2-M1) |
| Import-E47 | ALIGN |
| Import-E48 | ALIGN |
| Import-E49 | ALIGN |
| DF-R1.2 | ALIGN |
| DF-R1.6 | ALIGN |
| DF-R2.1 | ALIGN |
| DF-R2.2 | ALIGN |
| DF-R2.3 (with R2.3h, R2.3j) | ALIGN |
| DF-R2.4 | ALIGN |
| DF-R5.5 (with R5.5d) | OBJECT (TR2-m12) |
| DF-R7.1 | ALIGN |
| DF-R7.2 | ALIGN |
| DF-R7.7 (with R7.7o) | OBJECT (TR2-M3, at DF:281) |
| Device-R1.21 | ALIGN |
| Device-R1.22 | ALIGN |
| CM-R2.4 (with R2.4j) | ALIGN |
| CM-R2.7 | ALIGN |
| CM-R4.2 (with R4.2d) | ALIGN |
| CM-R5.2 (with R5.2b/e) | ALIGN |
| CM-R8.9 | ALIGN |
| CM-M2 | ALIGN |
| CM-E12 | ALIGN |
| Export-R1.1 (with R1.1h/i/p/s/t) | ALIGN |
| Export-R1.2 | ALIGN |
| Export-R4.2 | OBJECT (TR2-M3) |
| Export-E1 | ALIGN |
| Capture-R1.10 | ALIGN |
| Capture-M8 | ALIGN |

TR2-M4 binds Collection Mode R5.3 through UJ2.1-s; R5.3 is aligned and outside this table. DF-R2.1 is ALIGN because its SQL oracle (DJ:115–116) is real.

### interface

#### Verdict
Contract sound. No Blocker remains, and every round-1 Blocker is fixed. The fixes add two Majors: Export's fixture range is out of date on both sides of the Export and Data Foundation link, and Data Foundation R5.5d's new salvage exception reverses N18. There are also seven copy and journey Minors to fix before alignment.

#### Surface & consumers (brief)
- **Surface:** the published contract across the six PRDs: row IDs and R3.8 transitions, E-state copy and its placeholders, the inherited-obligation lines, and the export's `sc_` app columns.
- **Consumers:** build agents, who build and test by row and state ID; the sibling PRDs, through the obligation lines; the Cataloger, through the copy; scripts that read `sc_imported` by name.
- **What changed** (`git diff 2a54fce f7cbde5 -- docs AGENTS.md`):
  - Import: R6.8f/R6.8g, R6.11, E48 and E49, and Toolkit variants of E14 and E43.
  - Data Foundation: its reading model, R2.3h, R2.3j, R5.5d and R7.7o.
  - Collection Mode: E12, the copy lines and UJ2.1-s.
  - Export: `sc_imported` in R1.1h/i/s, R1.2 and R4.2.
  - Capture: R1.10, M8 and the §12 placeholders.
  - The obligation lines, in both directions.
- **No consumer-facing break:** `sc_imported` still lands before any export format version ships.

File aliases used below: IMP = `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import.md`; IMPC = `…/import/prd-inventory-import-copy.md`; IMPJ = `…/import/prd-inventory-import-journeys.md`; IMPF = `…/import/prd-inventory-import-fences.md`; DF = `…/data-foundation/prd-data-foundation.md`; EXP = `…/export/prd-data-export.md`; EXPJ = `…/export/prd-data-export-journeys.md`; EXPF = `…/export/prd-data-export-fences.md`; CM = `…/collection-mode/prd-collection-mode.md`; CMC = `…/collection-mode/prd-collection-mode-copy.md`; CMJ = `…/collection-mode/prd-collection-mode-journeys.md`; DEV = `…/device-management/prd-device-management.md` (all under `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/`).

#### Findings

##### Round-1 verification (IF1-*)

**Blockers**
- **IF1-B1 — RESOLVED.**
  - R6.8f runs first and checks against any reading the item holds, current or in history (IMP:182).
  - "Same reading" is defined at IMP:23, and DF R2.3j says a same reading "creates none" (DF:137).
  - Covered by cases: repeat over a scan-current item (IMPJ:134), v1→v2→v1 (IMPJ:137), the tie (IMPJ:138), a Flag then re-import (IMPJ:139), and plain-decimal numbers with a `+00:00` offset (IMPJ:141).
- **IF1-B2 — RESOLVED.** IMPJ:133 now reads "2 used as colour (TK-2, TK-3)", and R6.9 partitions the eligible records (IMP:174).

**Majors**
- **IF1-M1 — PARTIAL.** E43's Toolkit variant now offers neither control (IMPC:29; IMP:166). E5 still offers both controls for a source with the Toolkit signature (IMPC:15); see IF2-m1.
- **IF1-M2 — RESOLVED.**
  - Mode adoption is now part of the atomic commit (R3.2, IMP:79), and R3.8k rechecks the mode fit, routing to E46 (IMP:126).
  - The creation form's mode is fixed (R6.3, IMP:168), and R6.4 is written into the commit (IMP:169).
  - Capture R1.10 is updated to match.
  - Cases: IMPJ:131 and IMPJ:132.
- **IF1-M3 — RESOLVED.** E14 has a Toolkit variant (IMPC:24) and R3.5 has the carve-out (IMP:82). A wording residue remains; see IF2-m3.
- **IF1-M4 — RESOLVED.** E12 gains the "From Nix Toolkit" meaning (CMC:173), and F221 is carried by E12 and R2.8.
- **IF1-M5 — RESOLVED.** CMC:295 and CMC:310 now show every unrecorded field, and CMC:307 gives R5.2b a form for an unknown serial.
- **IF1-M6 — RESOLVED.**
  - The Toolkit tokens are registered in Capture §12's placeholder list.
  - IMPC:37 defines each token, and says ⟨current⟩, ⟨history⟩ and ⟨same⟩ sum to ⟨readings⟩.
  - R6.9 (IMP:174) defines ⟨readings⟩ as the eligible records.
- **IF1-M7 — RESOLVED.**
  - Each E46 variant has its own headline (IMPC:32).
  - The "no readings yet" option is named.
  - The unverified export-by-mode claim is gone.
- **IF1-M8 — PARTIAL.** Import's Data Export line now carries `sc_imported`, with R6.5 in its Rows (IMP:151). But F79's map still says "governs no row here" (IMPF:336), and the line is credited to F72 (IMPF:349); see IF2-n3.
- **IF1-M9 — RESOLVED.** Import's Capture line has the carve-out, R6.8 in its Rows, and states that set-aside items become captured (IMP:153).
- **IF1-M10 — PARTIAL.**
  - Landed: R1.1h/s/i name `sc_imported` (EXP:88–90), R4.2 asserts all three `sc_imported` values (EXP:145), R7.7o exists (DF:268), and EJ1 checks the never-scanned empty (EXPJ:14).
  - Still missing: R4.2 still lists "R7.7a–n"; see IF2-M1.
- **IF1-M11 — RESOLVED by N16.**
  - DF R2.1 names the measured order as an over-time view (DF:112).
  - DJ6 asserts both orders.
  - CMJ:237 (UJ2.1-s) checks both orders.
- **IF1-M12 — RESOLVED.** R6.2 states every column's fate (IMP:167). IMPJ:119 asserts the exact stored column list, and IMPJ:113 covers an extra column.
- **IF1-M13 — RESOLVED.**
  - R6.7 now defines readable times, modes (M0–M2) and numbers (IMP:172).
  - R6.5 records a blank Nix Device as an unknown model.
  - Under N22, R6.6 lists an unsupported illuminant/observer pair as not checked.
  - Cases: IMPJ:123 and IMPJ:125–126.

**Minors**
- **IF1-m1 — RESOLVED.** R3.8 now reads "R3.8a–q" (IMP:85), and the every-state case covers E40–E49 (IMPJ:36).
- **IF1-m2 — RESOLVED.** DF R2.1 (DF:112) and R2.3's opening clause (DF:115) have the carve-outs, and AGENTS.md §8's Canonical value and Raw payload entries are amended.
- **IF1-m3 — RESOLVED.** R6.8 keys on the current reading's snapshot kind, and a restore carries its source's kind (IMP:173, 185–187).
- **IF1-m4 — RESOLVED.** R6.1 tries semicolon, comma, then tab (IMP:166); IMPJ:111 covers a comma re-save.
- **IF1-m5 — RESOLVED (deferred).** The post-lock Capture "scanned" item is recorded; the reason is sound.
- **IF1-m6 — RESOLVED.** E43's Toolkit variant uses distinct labels (IMPC:29).
- **IF1-m7 — RESOLVED.** The lines are now two-sided: DEV:263 and DEV:265, DF:279 and DF:282, EXP:172, and IMP:150. A residue remains; see IF2-n4.
- **IF1-m8 — RESOLVED.** CMC:307.
- **IF1-m9 — RESOLVED (deferred to ADR-0003).** The decisions README's ADR-0003 row records a reading's origin sitting apart from its device kind.
- **IF1-m10 — RESOLVED.** IMPJ:135 asserts rows Unchanged 3 and no E14.
- **IF1-m11 — RESOLVED.** Two "First reading" lines are the owner's N17 outcome, recorded at fix-file line 12.
- **IF1-m12 — PARTIAL.** E48 names the Toolkit export at mapping (IMPC:34), but the target step still has no string; see IF2-m5.
- **IF1-m13 — RESOLVED.** The Vocabulary's Ready to capture (IMP:21), line 5, and the PRD index README (line 39) are amended.
- **IF1-m14 — RESOLVED.** The ADR-0003 row in `docs/decisions/README.md` and a post-lock Import/DF item now carry the schema inputs.

**Nits**
- **IF1-n1 — RESOLVED.** R6.11 compares under R2.3 (IMP:176).
- **IF1-n2 — RESOLVED.** R1.5 carves out §6's reading columns (IMP:50).
- **IF1-n3 — RESOLVED.** Export R1.2 says the two columns are never both true (EXP:66).
- **IF1-n4 — RESOLVED.** R6.2 now ignores Index (IMP:167).

##### New in round 2

[MAJOR] IF2-M1 — Export R4.2 and both sides of the Export/Data Foundation link still say the fixture matrix is R7.7a–n, which leaves out the imported fixture
- **Location:**
  - EXP:145 — R4.2: "Enumerate DF R7.7a–n plus generated corpus … including all three `sc_simulated` and `sc_imported` values".
  - EXP:180 — Export's outbound Data Foundation line: "Its R7.7a–n fixture matrix … is the sole inventory".
  - DF:281 — Data Foundation's inbound Data Export line: "assert goldens on R7.7a–n".
- **What it contradicts:**
  - DF R7.7 at DF:245 ("R7.7a–o own the inventory").
  - DF R7.7o at DF:268, which names Export R4.2.
  - Export's own Clarified line at EXPF:225 ("R4.2 asserts … against a golden cut from … R7.7o").
- **Why it matters:** R7.7a–n contain no imported-kind reading, so R4.2's "all three `sc_imported` values" cannot be met from the range R4.2 itself lists. R4.2 was amended in this commit and now contradicts its own fence line.
- **Mutation:**
  - Build the golden suite from R4.2's a–n list and emit `sc_imported=false` for imported readings.
  - Every R4.2 golden stays green; only EJ1 (EXPJ:14) fails.
  - A build that also skips EJ1's R7.7o fixture ships the wrong value.
- **Fix:** read "R7.7a–o" at EXP:145, EXP:180 and DF:281. DF:281 should read "including empty R7.7n and imported R7.7o".

[MAJOR] IF2-M2 — DF R5.5d's new imported exception picks the wrong current reading in two of three imported pairings
- **Location:** DF:187 — "the later-recorded stays current unless it is imported and the other is not (R2.3j)".
- **What it contradicts:**
  - DF R2.3j at DF:137.
  - Import R6.8g and R6.8d at IMP:186–187.
  - Import F86 and F87.
  - DF F68 and F69.
- **Scenario A:**
  - A simulated A is current. A Toolkit B is imported, becomes current under R6.8g, and is recorded later.
  - Damage then leaves both current, and salvage runs.
  - B is imported and A is not, so the exception fires: A, the simulated reading, stays current.
  - B is kept as correction-unconfirmed. That reverses N18 and gives an imported reading the correction question F85 says it never gets.
- **Scenario B:**
  - An imported A (Date Saved in March) is current. An imported B (Date Saved in February) is kept behind it under R6.8d and recorded later.
  - Both end up current. The exception doesn't fire because both are imported, so B, the later-recorded one, stays current.
  - R2.3j would keep A.
- **Mutation:** implement R5.5d as written, then salvage R7.2's declared two-current state for scenario A. The simulated reading ends up current. No DJ or R7.7 case exercises this pairing, so the build ships.
- **Fix:** "the later-recorded stays current unless R2.3j keeps it behind the other". Add an R7.2-declared two-current case with an imported reading.
- **Scope:** the path is rare, and every resolution is named to the user, but the rule is a published product rule.

**New Minors**
- [MINOR] IF2-m1 — IMP:166 / IMPC:15 / IMP:116: for a source with the Toolkit signature that reaches E5, E5 still offers "Choose an encoding" / "Choose a separator".
  - E5 can come from bytes that are invalid as UTF-8, or from a quote the Toolkit wrote unescaped, which OQ 3 leaves open.
  - A chosen separator is ignored by R6.1's fixed probe order (semicolon, comma, tab).
  - A chosen non-UTF-8 encoding contradicts "read as UTF-8".
  - This is IF1-M1's silent no-op moved to E5. Fix: state E5's Toolkit behaviour (offer only "Pick the file again" / "Cancel", ending with the re-export remedy, or keep recognition under a manual encoding), and add a UJ 3 case.
- [MINOR] IF2-m2 — IMPC:39: the Toolkit ending rule for E7, E9, E10 and E42 replaces "their fix-the-file or fill-the-codes sentence".
  - E7's sentence also carries "You can bring in the rest", and E42's also carries "nothing has been imported", so two builds ship different strings.
  - "Correct it" is singular for plural rows.
  - Fix: write each Toolkit ending out in full as E14's variant does, for example "Bring in the rest, or correct them in the Nix Toolkit and export it again."
- [MINOR] IF2-m3 — IMPC:29 and IMPC:24: "Kept as earlier readings: ⟨history⟩ — a swatch you scanned here keeps that scan" has three problems.
  - The count includes R6.8d's earlier and same-dated readings, where no scan is involved.
  - It calls a Toolkit reading measured after the scan "earlier": at IMPJ:133, TK-1 was measured 2026-03-14 and sits behind a 2026-02-01 scan. F84 removed exactly that word from R6.8c.
  - In E14 and E43, "keeps that scan" is false for a Demo Device reading, which R6.8g replaces.
  - Fix: "Kept in history, not used as the colour: ⟨history⟩", and scope the clause to readings scanned with the instrument.
- [MINOR] IF2-m4 — IMPC:34: E48 says "The file's own Lab, LCh, XYZ, sRGB and HEX are checked, not kept".
  - R6.6 checks only the first L, a and b. R6.2 ignores LCh, XYZ, sRGB and HEX, and N13 compares only Lab.
  - This breaks the copy header's promise that every sentence is backed by a row.
  - Fix: "The file's own Lab is checked against the spectrum; none of its colour values is kept."
- [MINOR] IF2-m5 — IMPJ:110–111 reach E48 from "Pick the file" with "no target yet".
  - The Surfaces flow (IMP:40) and R2.1 (IMP:67) put the target step before mapping, and R3.8q (IMP:132) sends E48 straight to preview.
  - A test harness that asserts state order either fails a correct build or has to add a target step the case doesn't state.
  - R6.1's "every later step names it a Toolkit export" still has no string at the target step.
  - Fix: add creating the target to those cases' Action, and either give the target step a Toolkit line or narrow R6.1 to mapping and preview.
- [MINOR] IF2-m6 — IMPC:37 says E43 shows "encoding/delimiter controls always visible (R3.8n)", with no Toolkit carve-out. R6.1 and E43's Toolkit variant offer neither control. Fix: "shown fixed, without controls, for a Toolkit export (R6.1)".
- [MINOR] IF2-m7 — CMJ:237 (UJ2.1-s) and F221's Clarified line read E17's "restore" variant.
  - That variant is `[phase: variant-absent]` until R5.5 lands (CMC:223; R5.5 is P1, CM:387).
  - In the first build phase the case names a variant the build must not render.
  - Fix: "E17 in recorded order (its restore variant once R5.5 lands)".

**Nits**
- [NIT] IF2-n1 — IMP:182: R6.8f's "Changes nothing" reads as the whole record, yet IMPJ:120 updates TK-2's Note on an R6.8f record. Say "adds no reading and changes no reading or state; metadata follows R3.5/R3.6".
- [NIT] IF2-n2 — DEV:120: R1.21's "naming the model its file gives" doesn't carry R6.5's "unknown where the cell is blank".
- [NIT] IF2-n3 — IMPF:336: F79 still "governs no row here", but its `sc_imported` now sits on the Data Export line (IMP:151). List that line under F79.
- [NIT] IF2-n4 — EXP:181: Export's outbound Device line still names only `sc_simulated`. EXPJ:13's quarantined-current row also asserts only `sc_simulated`, though R1.1s now names `sc_imported`.
- [NIT] IF2-n5 — `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/export/prd-data-export-copy.md:16`: E1 says "how many samples they averaged weren't recorded, so those cells stay empty", but the export has no sample-count column. The empty cells are the basis and payload columns (R1.1t).
- [NIT] IF2-n6 — Capture M8 and IMP:153 exclude "an import whose items arrive captured". A Toolkit import that captures no item (every match current on a live scan, no new record) is neither excluded nor clearly in scope. Say "a Toolkit export's import".
- [NIT] IF2-n7 — R6.7 (IMP:172): a Date Saved with sub-millisecond digits has no rule for truncating or rounding, though R6.5 and "same reading" compare to the millisecond.

#### Biggest risks   (what existing consumers/scripts/agents break)
- **Export goldens miss the imported fixture (IF2-M1).** A golden suite built from R4.2's own list never includes R7.7o. Only EJ1 then guards `sc_imported=true`, which is the one value scripts rely on to separate Toolkit readings.
- **Salvage keeps the wrong reading (IF2-M2).** Salvage of a damaged file can leave a Demo reading current over a real Toolkit reading, and put a correction question on an imported reading.
- **Toolkit exports that need a re-read (IF2-m1).** A Toolkit export that needs an encoding or separator choice is stuck: the offered actions have no defined effect.
- **Copy that says more than the rows (IF2-m3, IF2-m4).** The preview calls a later-measured reading "earlier", and E48 claims four checks the build doesn't run.

#### Genuinely well-designed   (incl. where a deliberate inconsistency is correct that a style-checker would wrongly flag)
- **R6.8f and the reading counts.** R6.8f checks against any reading held, and restores and Flags stand. Together with R6.9's partition, whose three counts sum to ⟨readings⟩, the idempotency promise is testable.
- **Keying R6.8 on snapshot kind.** A restore carries its source's kind, which removes all the prose guessing from round 1.
- **The E14 and E46 fixes.** E14's Toolkit variant, E46's split headlines and the registered placeholders close every round-1 copy/contract gap in my lens.
- **The chip label differs from the filter label, on purpose.** The chip label "From Nix Toolkit" differs from its filter label "Imported from Nix Toolkit" deliberately: it keeps the chip clear of the " (imported)" column tag. A style checker would wrongly flag this as inconsistent.
- **`sc_imported` needs no version bump, correctly.** It lands in the first golden, R2.5 says to parse by name, and R1.2 now says the two provenance columns are never both true.
- **The predecessor rule in R2.3h.** "The reading current when B landed, never the previous sequence number" is the right contract, and it aligns Data Foundation, Collection Mode UJ2.1-s and Export's sequence order.
- **The obligation lines.** They are now two-sided across all six PRDs, apart from the residues noted in IF2-n3 and IF2-n4.

#### Missing / over-engineered
- **Missing:**
  - an R7.7o golden in R4.2's list (IF2-M1);
  - a two-current salvage case with an imported reading (IF2-M2);
  - E5's Toolkit behaviour (IF2-m1);
  - a target step in UJ 3 before E48 (IF2-m5).
- **Not over-engineered:** E43's Toolkit variant is dense, but every sentence traces to R6.4, R6.6, R6.8 or R6.9, and zero-count sentences drop out under Capture §12.

| Row ID | disposition |
| :--- | :--- |
| Import-R1.5 | ALIGN |
| Import-R3.2 | ALIGN |
| Import-R3.3 | ALIGN |
| Import-R3.5 | ALIGN |
| Import-R3.8 | ALIGN |
| Import-R6.1 | OBJECT (IF2-m1, IF2-m5) |
| Import-R6.2 | ALIGN |
| Import-R6.3 | ALIGN |
| Import-R6.4 | ALIGN |
| Import-R6.5 | ALIGN |
| Import-R6.6 | ALIGN |
| Import-R6.7 | ALIGN |
| Import-R6.8 | ALIGN |
| Import-R6.9 | ALIGN |
| Import-R6.10 | ALIGN |
| Import-R6.11 | ALIGN |
| Import-M2 | ALIGN |
| Import-E7 | OBJECT (IF2-m2) |
| Import-E9 | OBJECT (IF2-m2) |
| Import-E10 | OBJECT (IF2-m2) |
| Import-E14 | OBJECT (IF2-m3) |
| Import-E42 | OBJECT (IF2-m2) |
| Import-E43 (Toolkit variant) | OBJECT (IF2-m3, IF2-m6) |
| Import-E46 | ALIGN |
| Import-E47 | ALIGN |
| Import-E48 | OBJECT (IF2-m4, IF2-m5) |
| Import-E49 | ALIGN |
| DF-R1.2 | ALIGN |
| DF-R1.6 | ALIGN |
| DF-R2.1 | ALIGN |
| DF-R2.2 | ALIGN |
| DF-R2.3 (with R2.3h, R2.3j) | ALIGN |
| DF-R2.4 | ALIGN |
| DF-R5.5 (with R5.5d) | OBJECT (IF2-M2) |
| DF-R7.1 | ALIGN |
| DF-R7.2 | ALIGN |
| DF-R7.7 (with R7.7o) | OBJECT (IF2-M1) |
| Device-R1.21 | ALIGN |
| Device-R1.22 | ALIGN |
| CM-R2.4 (with R2.4j) | ALIGN |
| CM-R2.7 | ALIGN |
| CM-R4.2 (with R4.2d) | ALIGN |
| CM-R5.2 (with R5.2b/e) | ALIGN |
| CM-R8.9 | ALIGN |
| CM-M2 | ALIGN |
| CM-E12 | ALIGN |
| Export-R1.1 (with R1.1h/i/p/s/t) | ALIGN |
| Export-R1.2 | ALIGN |
| Export-R4.2 | OBJECT (IF2-M1) |
| Export-E1 | ALIGN |
| Capture-R1.10 | ALIGN |
| Capture-M8 | ALIGN |

### architecture

#### Verdict
Sound. Build it once two new Majors are fixed; each needs about one clause. Round 1's Blocker and five of its seven Majors are resolved, and the imported snapshot kind, the never-supplied payload state and R2.3j's current-reading rule now hold together. No row decides ADR-0003's schema or the blob schema, and nothing contradicts ADR-0001. Two fixes introduced contradictions that split builds: a UJ 3 case refuses an import that R6.4 allows, and the salvage tie-break in R5.5d now contradicts R2.3j.

#### Architecture in brief
- **Import §6** checks each record, applies the same-reading check first (R6.8f), then chooses the record's outcome by the snapshot kind of the matched item's current reading (R6.8a–g).
- **Data Foundation** owns the stored reading and its rules. An imported reading has no samples, basis, verdict or spread. The current reading is stored explicitly, not taken as the latest. The predecessor is the reading that was current when the new one landed (R2.3h).
- **Device** owns the three snapshot kinds. Collection Mode's mark and Export's `sc_imported`/`sc_simulated` columns are all worked out from that one field.
- **Mode adoption** is part of the import's commit.
- **The key tradeoff:** a reading can be current without being the latest one recorded. The fixes now state that in DF's own rows and pass it to ADR-0003 as an input.

#### Findings

Path legend (all absolute):
- IMP = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import.md
- IJ = …/docs/product/import/prd-inventory-import-journeys.md
- IC = …/docs/product/import/prd-inventory-import-copy.md
- DF = …/docs/product/data-foundation/prd-data-foundation.md
- DFJ = …/docs/product/data-foundation/prd-data-foundation-journeys.md
- DFF = …/docs/product/data-foundation/prd-data-foundation-fences.md
- EX = …/docs/product/export/prd-data-export.md
- EXJ = …/docs/product/export/prd-data-export-journeys.md
- CMC = …/docs/product/collection-mode/prd-collection-mode-copy.md
- CMJ = …/docs/product/collection-mode/prd-collection-mode-journeys.md
- CAP = …/docs/product/capture-mode/prd-capture-mode.md
- ADR = …/docs/decisions/README.md
- PL = …/docs/product/post-lock.md

Every `…` stands for `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import`.

##### Round-1 findings: resolution

- **B1: RESOLVED.** The same-reading check now runs before every other outcome: IMP:173 (R6.8f first), IMP:182 (R6.8f), IMP:23 (Vocabulary). DF:137 (R2.3j) says "none when the item already holds the same reading". The cases are IJ:134, :135, :137, :139 (a Flag stands) and :141 (other written forms), plus DFJ:120.
- **M1: RESOLVED.** The time axis is named: DF:137 says "over an imported A measured earlier". R2.3's lead gains its exception (DF:115). A same-dated reading with a different spectrum has an outcome (IMP:187, DFF:783 F69).
- **M2: RESOLVED.** DF:112 (R2.1) keeps history in record order and puts the measured order among the over-time views. F66 (DFF:765) and F64's Clarified line (DFF:757) record it. DFJ:115–116 and CMJ:237 (UJ2.1-s) test it.
- **M3: RESOLVED.** Owner decision N17 is carried by the R2.4 exception (DF:116), the reasons column (DF:137) and the predecessor rule (DF:135, R2.3h). DFJ:117 proves a kept-behind reading gets no never-true mark.
- **M4: RESOLVED.** DF:37 (Vocabulary) and DF:112 (R2.1) define a reading with no samples. DF:137 says a restore copies that state. DFJ:123 tests it.
- **M5: PARTIAL.** The adoption now lands with the commit (IMP:169, IMP:79, IMP:126, CAP:147), E43 names it (IC:29), and IJ:130–131 test it. But the new case IJ:132 contradicts R6.4 (AR2-M1). Through that path the chosen mode can still change without the preview saying so, which was the hazard in M5(c).
- **M6: RESOLVED.** R6.9 splits the reading counts into three sets with no overlap (IMP:174). IJ:117, :133 and :136 agree with it.
- **M7: PARTIAL.** R7.7o was added (DF:268) and R7.7 now covers a–o (DF:245). EX:145 (R4.2) names `sc_imported`, but it still enumerates "DF R7.7a–n", and so do EX:180, EXJ:3 and DF:281 (AR2-m1).
- **m1: PARTIAL, now a Major.** DF:187 (R5.5d) covers live against imported, but it inverts decision N18 for simulated against imported and misorders two imported readings (AR2-M2).
- **m2: RESOLVED.** R1.2 (DF:99) and R7.1 (DF:239) now list snapshot kinds and readings with no samples. DFJ:114 says what a reader must be able to tell. A wording residue remains at DFJ:113 (AR2-n2).
- **m3: RESOLVED, deferred soundly** as an ADR-0003 input: ADR:22 (last clause) and PL:169.
- **m4: RESOLVED.** The file's illuminant and observer are stored as provenance only, and a blank Nix Device leaves the model unknown (IMP:170). R6.6 lists unsupported pairs as not checked (IMP:171). IJ:123 tests it.
- **m5: RESOLVED.** The mode grammar is M0, M1 or M2 (IMP:172). "Hold no reading" is defined as current or in history, QC records aside (IMP:169).
- **m6: RESOLVED.** R6.8 keys on the current reading's snapshot kind, and a restore carries its source's kind (IMP:173, :185–187).
- **m7: RESOLVED, deferred.** Export OQ 4 records the Toolkit's grid (EX:205), and the hardware spike checks it (PL:12).
- **m8: RESOLVED.** A Note of `undefined` supplies no value (IMP:167); IJ:120 tests it.
- **m9: RESOLVED.** R6.1 tries the semicolon, the comma and the tab, and shows encoding and separator fixed (IMP:166). IJ:110–112 test it, and E43 offers neither control (IC:29).
- **m10: RESOLVED.** Custom Collection Name is read for R6.3 and R6.11 and not stored (IMP:167).
- **m11: RESOLVED.** The ADR-0003 inputs are at ADR:22 and PL:169 (wording nit: AR2-n1).
- **m12: RESOLVED, deferred.** OQ 3 asks whether a code can repeat (IMP:205). Meanwhile R2.4 excludes every record in a duplicate-code group, which fails safe.
- **n1: RESOLVED, deferred** to Capture's next pass (PL:102).
- **n2: RESOLVED.** E43's Toolkit labels are now "Already here" and "Kept as earlier readings", distinct from "Unchanged:" (IC:29).
- **n3: RESOLVED.** The chip label is now "From Nix Toolkit" (CMC:260).

##### New findings

**[MAJOR] AR2-M1: Import R3.8k and UJ 3: a case refuses an import that R6.4 allows**

- **Where:**
  - IJ:132 expects E46 when the target's mode is changed from M2 to M1 between the preview and Import, on a target holding only pending items.
  - R6.4 (IMP:169) says that such a target "hold[s] no reading … and then adopt[s]" the file's mode. IJ:130 is exactly that valid case.
  - E46's own copy (IC:32) tells the user to "Import into … one with no readings yet". That is what they chose.
  - R3.8k (IMP:126) rechecks only whether the target still fits. It does not check whether the target's mode changed since the preview.
- **Scenario and mutation:**
  - A build that follows R6.4 and R3.8k literally sees that the target still fits, commits and switches the collection from M1 to M2. It fails IJ:132. Worse, the E43 the user confirmed never said the mode would change, which is round 1's M5(c) hazard.
  - A build that refuses on any mode change passes IJ:132, but shows E46 copy that contradicts itself.
- **Fix:**
  - R3.8k: "a target whose scan mode changed since the preview but still fits → E43 again with fresh counts and its mode line, Import required again (as R3.8f); one that no longer fits → E46".
  - Rewrite IJ:132 to expect that fresh E43, naming the change from M1 to M2, with nothing written.
  - Add a case where the target gains a reading in M1 between the preview and Import; that one gets E46.

**[MAJOR] AR2-M2: DF R5.5d: the tie-break for two current readings now contradicts R2.3j and fence F68**

- **Where:**
  - DF:187: "the later-recorded stays current unless it is imported and the other is not (R2.3j), the other retained as correction-unconfirmed".
  - This contradicts DF:137 (R2.3j) and F68 (DFF:777) on "over a simulated A". The two-current case in DFJ:34 (DJ3) exercises only the default rule.
- **Mutation:** declare an item with two current readings and salvage it (R7.2 already lets tests declare that state).
  - (a) A simulated reading recorded first and an imported one recorded later: the literal rule keeps the simulated reading current. R2.3j and N18 make the imported one current.
  - (b) Two imported readings, where the later-recorded one was measured earlier: the literal rule keeps the later-recorded one. R2.3j keeps the one measured later.
  - (c) A live reading and a later-recorded imported one: the live reading stays current, which is right. But the imported reading is "retained as correction-unconfirmed", which puts a question into E11/E26 that R2.3j (reason initial) and R2.3h ("no reading's predecessor") say a kept-behind reading never carries.
  - No case catches any of the three.
- **Fix:**
  - Reword R5.5d: "the later-recorded stays current unless R2.3j would have kept it behind the other; a reading so kept behind is retained with reason initial, otherwise the other is retained as correction-unconfirmed".
  - Add one salvage case with an imported reading to DJ3 or DJ6.
- **Reach:** low. Only a corrupted file gets here, and the salvage output keeps every reading. It still meets the brief's test: a line changed in this pass contradicts its own fence.

**Minors**

- **[MINOR] AR2-m1: stale fixture range.** Export R4.2 (EX:145) asserts all three `sc_imported` values but enumerates "DF R7.7a–n". The only imported fixture is R7.7o, so the row contradicts itself. EX:180, EXJ:3 and DF:281 carry the same stale range. Change a–n to a–o in all four.
- **[MINOR] AR2-m2: a quarantined reading and the same-reading check.** R6.8b (IMP:184) handles an item whose current reading is quarantined. But R6.8f (IMP:182, :23) checks "same reading" before R6.8b, and a quarantined reading's spectrum cannot be read. Builds would split: one lets R6.8b recover the item, the other reports "already held" and leaves it without a value. State "a quarantined reading is never the same reading" in R6.8f.
- **[MINOR] AR2-m3: ADR-0003 input on spectral precision.** R6.5 stores reflectances "neither clamped nor rounded" (IMP:170), and R6.8f compares them exactly. STORE_SIZE_BUDGET pushes towards a compact spectral encoding, and a single-precision or quantized one would break re-import idempotency. IJ:135 and :141 would catch it, but only after the file format has shipped. Add to ADR:22's Toolkit input: "an imported spectrum reads back as the numbers given, compared exactly by Import R6.8f".
- **[MINOR] AR2-m4: export state label.** Missed in round 1, and it predates this pass. Export's closed State column (EX:32, R1.1j at EX:91, R1.1t at EX:94) labels a reading R2.3j kept behind as `superseded`, though it was never current. Either define `superseded` as "not current, including a reading kept behind", or add a value before v1's format ships. After that, adding one is a format change under Export R2.5.

**Nits**

- **[NIT] AR2-n1: ADR-0003 input wording (ADR:22).**
  - A sentence break is missing: "…the cold open Input from the Nix Toolkit import".
  - "Stored explicitly" and "a check whose key must exclude restores" assume how the store will do it. Prefer "determinable by plain SQL at the floor (R1.2), never inferred from sequence" and "if enforced as a uniqueness constraint, restores are exempt".
- **[NIT] AR2-n2: DJ6 row 1 wording (DFJ:113).** It still says "recorded as never supplied", which implies a stored state that R1.2 and R7.1 don't list. "Never supplied" already follows from a reading having no samples. Say "no payload, and no archive-unavailable mark".
- **[NIT] AR2-n3: E14 Toolkit variant (IC:24).** "A swatch you scanned here keeps that scan as its colour" is false where the current reading came from the Demo Device (R6.8g, N18). This is for the copy lenses.

#### Biggest risks
1. **Two contradictions that split builds, both from this fix pass.** One is IJ:132 against R6.4 (AR2-M1). The other is DF R5.5d against R2.3j (AR2-M2). Each lets a wrong build pass or makes a correct build fail.
2. **The file format.** Idempotent re-import now rests on exact spectral equality, and the ADR-0003 inputs don't yet say so (AR2-m3). A space-driven encoding choice could break it after the format has shipped.
3. **Fixture coverage.** The Export golden range excludes R7.7o (AR2-m1), so `sc_imported` coverage exists only in the fence text, not in the row that enumerates the fixtures.

#### Genuinely sound
- **The same-reading check runs first (R6.8f).** This is what keeps DF R2.9 intact: without it, re-importing after a Flag would bring back the flagged value. IJ:139 proves it.
- **One field decides everything.** The snapshot kind drives R6.8, Collection Mode's R2.4j mark and both export columns, and a restore copies it. There is no second store to drift out of step.
- **The predecessor is decoupled from sequence (R2.3h).** Together with "a kept-behind reading is no reading's predecessor", this keeps E11/E26 free of questions that would name the wrong reading. A dogmatic reviewer might call explicit predecessors over-engineering; they are required once the current reading need not be the latest.
- **No mixed-key history sort was introduced (N16).** R2.1 and Collection Mode R5.3 keep their settled orders, and the measured view already delivers N10.
- **No new stored "never supplied" state is needed.** The state follows from a reading having no samples, and export reuses its existing unused-slot semantics.
- **Mode adoption is atomic with the commit and rolls back with it.** A Toolkit-created target's mode persisting after Cancel is consistent with R3.7, not a bug.
- **The ADR boundaries hold.** No row decides ADR-0003's schema or the blob layout; the new material is framed as inputs. ADR-0001 is untouched: the import stays off the device seam and runs in CI on synthetic fixtures.

#### Missing / over-engineered
**Missing:**
- R3.8k's path for a target whose mode changed since the preview (AR2-M1).
- An R5.5d rule and case that follow R2.3j (AR2-M2).
- The quarantined-reading exclusion from the same-reading check (AR2-m2).
- The spectral-precision ADR input (AR2-m3).
- A ‹P1› UJ 3 case for re-importing after a restore of an imported reading. R6.8f promises that "a restore stands", but only DFJ:123 covers restore, and not followed by a re-import.

**Over-engineered:** nothing material.

| Row ID | disposition |
| :--- | :--- |
| Import-R1.5 | ALIGN |
| Import-R3.2 | ALIGN |
| Import-R3.3 | ALIGN |
| Import-R3.5 | ALIGN |
| Import-R3.8 | OBJECT (AR2-M1) |
| Import-R6.1 | ALIGN |
| Import-R6.2 | ALIGN |
| Import-R6.3 | ALIGN |
| Import-R6.4 | OBJECT (AR2-M1) |
| Import-R6.5 | ALIGN |
| Import-R6.6 | ALIGN |
| Import-R6.7 | ALIGN |
| Import-R6.8 | OBJECT (AR2-m2) |
| Import-R6.9 | ALIGN |
| Import-R6.10 | ALIGN |
| Import-R6.11 | ALIGN |
| Import-M2 | ABSTAIN (out of lens) |
| Import-E7 | ABSTAIN (out of lens) |
| Import-E9 | ABSTAIN (out of lens) |
| Import-E10 | ABSTAIN (out of lens) |
| Import-E14 | OBJECT (AR2-n3) |
| Import-E42 | ABSTAIN (out of lens) |
| Import-E43 (Toolkit variant) | ALIGN |
| Import-E46 | ALIGN |
| Import-E47 | ABSTAIN (out of lens) |
| Import-E48 | ALIGN |
| Import-E49 | ALIGN |
| DF-R1.2 | ALIGN |
| DF-R1.6 | ALIGN |
| DF-R2.1 | ALIGN |
| DF-R2.2 | ALIGN |
| DF-R2.3 (with R2.3h, R2.3j) | ALIGN |
| DF-R2.4 | ALIGN |
| DF-R5.5 (with R5.5d) | OBJECT (AR2-M2) |
| DF-R7.1 | ALIGN |
| DF-R7.2 | ALIGN |
| DF-R7.7 (with R7.7o) | ALIGN |
| Device-R1.21 | ALIGN |
| Device-R1.22 | ALIGN |
| CM-R2.4 (with R2.4j) | ALIGN |
| CM-R2.7 | ALIGN |
| CM-R4.2 (with R4.2d) | ALIGN |
| CM-R5.2 (with R5.2b/e) | ALIGN |
| CM-R8.9 | ALIGN |
| CM-M2 | ABSTAIN (out of lens) |
| CM-E12 | ABSTAIN (out of lens) |
| Export-R1.1 (with R1.1h/i/p/s/t) | OBJECT (AR2-m4) |
| Export-R1.2 | ALIGN |
| Export-R4.2 | OBJECT (AR2-m1) |
| Export-E1 | ALIGN |
| Capture-R1.10 | ALIGN |
| Capture-M8 | ABSTAIN (out of lens) |

### privacy

#### Verdict
Privacy-sound to ship. All nine round-1 privacy findings are resolved or soundly deferred. My own count-only check against the real export found none of its values anywhere in the branch. One new Medium remains, and it is worth fixing before lock: the rows never say what the owner may write into the repo when M2 and OQ 3's dogfood runs are recorded.

#### Data-flow & PII map (brief)
- **Data subject.** The Cataloger (the owner). The only outside recipient is anyone reading the public repository.
- **Personal fields in a Toolkit record.** Date Saved (a millisecond activity timestamp), Note (free text), Custom Collection Name (a user label), Nix Device (a model name; whether a user can rename it is unverified), and any column R6.2 does not name.
- **What an import stores.**
  - The reading, with Date Saved as its measurement time and the model in an imported-kind snapshot (serial and firmware empty).
  - Metadata: Note (a Note of `undefined` supplies nothing), the five Density columns, and any other column, which E43 lists before commit.
  - Custom Collection Name is read to pre-fill a new target's name and for R6.11's one-collection check. It is never stored as a column (R6.2 at `prd-inventory-import.md:167`, asserted by UJ 3 at `prd-inventory-import-journeys.md:119`).
- **What an export carries.** Only on the user's own export: the model, measured-at (Date Saved), `sc_imported`, and the imported columns. Serial, firmware, basis and payload cells stay empty (Export R1.1t and E1). Nothing goes to the network, telemetry or system search (DF R1.4, CM R8.6).
- **Repository boundary.**
  - UJ 3 records format facts only. Fixture T and DJ6 values are invented.
  - The review log and fixtures hold no real-export value (checked below).
  - Recording OQ 3's future results in the repo is the one open path (PRIV2-1).

#### Findings

**Round-1 findings, re-checked against f7cbde5** (paths are under `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/`)

- **PRIV-1 (HIGH) — RESOLVED.**
  - Fixture T now uses invented dates, names and codes (`docs/product/import/prd-inventory-import-journeys.md:106`, oracle `:118`), and DJ6 inherits them (`docs/product/data-foundation/prd-data-foundation-journeys.md:109`, `:113`, `:115`, `:119`).
  - I re-checked by reading the real export locally and printing only booleans and masked counts. No Date Saved value (full, to the minute, or time of day), collection name, colour name, code, note, or written or rounded numeric value appears in the added lines of 6374538..f7cbde5 or in their commit messages.
  - The only raw matches were coincidences: line-number citations, one ordinary English word used in derivation prose, and the documents' own September fence dates.
  - None of Fixture T's or DJ6's days is a real session day.
  - 417fdac is reachable from no ref, and the branch is not on the remote.
- **PRIV-2 (MEDIUM) — RESOLVED.** Round 0 is redacted at `docs/agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md:18` and `:32`, and `:7` explains the redaction under N3.
- **PRIV-3 (MEDIUM) — RESOLVED.**
  - `docs/product/import/prd-inventory-import.md:167` (R6.2): Custom Collection Name is "read for R6.3 and R6.11 … none of these is stored as a column", and a column R6.2 does not name is listed in E43.
  - `prd-inventory-import-journeys.md:119` asserts it; `:116` covers an existing target ignoring the name.
- **PRIV-4 (MEDIUM) — RESOLVED.**
  - R6.10 at `prd-inventory-import.md:175` defines synthetic and covers derived fixtures and goldens; the F71 Clarified line is at `prd-inventory-import-fences.md:174`.
  - The format is stated in the repo at `prd-inventory-import-journeys.md:104`.
  - DF R7.7 (`prd-data-foundation.md:245`) and R7.7o (`:268`) source imported fixtures from a synthetic export.
- **PRIV-5 (LOW) — RESOLVED.** R6.2 (`prd-inventory-import.md:167`): a Note of `undefined` supplies no value, so a stored Note is kept. Covered by F73 Clarified (`prd-inventory-import-fences.md:188`) and UJ 3 `:120`, where a typed note survives re-import.
- **PRIV-6 (LOW) — RESOLVED.** E43's Toolkit variant (`prd-inventory-import-copy.md:29`) now ends "Imported readings stay in each swatch's history; only deleting the swatch removes them."
- **PRIV-7 (LOW) — RESOLVED.**
  - The rule is recorded at review log `:41` and fix file `:65`.
  - No round-2 brief in the dispatch folder cites the research report, and the review log it now carries holds no real value.
  - Nit: the fix file files it under "Deferred to post-lock", but `docs/product/post-lock.md` has no such line. The review log is an adequate home for a review-process rule.
- **PRIV-8 (INFO) — RESOLVED as a deferral.** `docs/product/post-lock.md:13` (spike group) says to record the model only when it is a v1-family name, if the Nix Device column can hold a user label.
- **PRIV-9a (INFO) — RESOLVED.** DF R2.1 (`prd-data-foundation.md:112`), F66 (`prd-data-foundation-fences.md:765`), and DJ6 `:115`–`:116` now assert both orders.
- **PRIV-9b (INFO) — RESOLVED.** DF R7.7o at `prd-data-foundation.md:268`.
- **PRIV-9 nit — RESOLVED.** `docs/product/README.md:146` says an imported snapshot is never a device record.

**New findings**

**[MEDIUM] PRIV2-1 — `prd-inventory-import.md:195` (M2) and `:205` (OQ 3), against `:175` (R6.10), `prd-inventory-import-fences.md:174` (F71 Clarified) and `docs/product/post-lock.md:202` — the repo is told to record real-export "outcomes" without a bound on what they may contain**

The conflict:
- R6.10 and F71 say "neither a real export nor a report on one enters the repository".
- M2 and OQ 3 require the owner's real exports to be run and their "outcomes and format facts recorded … never the files". `prd-inventory-import-oq-results.md:3` says an OQ closes only when its section "records the evidence".
- UJ 3 `:104` already holds format facts from a real export. So "report" in R6.10 cannot mean every record, and nothing says where the line sits.

Scenario:
1. After dogfood, the owner or an agent closes OQ 3 or measures M2.
2. They paste each file's preview outcomes: E49's ⟨collections⟩ (Toolkit collection names), E43's per-file counts, R6.6's differing and unchecked lists and E47's rows (swatch names, codes, record numbers).
3. These are the same classes of value PRIV-1 and PRIV-2 leaked (collection name, reading count, names), now in a public file.
4. M2's wording ("never the files") permits it, and R6.10's "report" does not clearly forbid it. A strict reader cannot close OQ 3 in the repo at all; a loose reader leaks.

Harm: low-sensitivity personal data about the owner (collection names, set sizes, item names, and dates if a clash is described), against N3's intent. It is irreversible once pushed.

Fix (one clause, no fence re-opened):
- M2's Method and OQ 3's Closer say what may be recorded: format facts (separator, quoting, number and time forms, grid, header set, which modes and illuminants occurred) and outcome tallies by state across files. Never a collection name, code, name, note, date, measured value, file name or path, or a per-file reading count.
- R6.10's "a report on one" reads "any data value from one", so the two rows agree.
- Mirror the bound on `post-lock.md:202`.

(GDPR Art. 5(1)(c) minimisation, Art. 25 privacy by design; the owner's N3.)

**[LOW] PRIV2-2 — `AGENTS.md:63`–`67` (§5, the public-repo boundary) — no standing repo rule keeps real vendor-app exports and their values out of commits**
- R6.10 binds Toolkit tests only.
- Round 1's value check was one-off and must stay uncommitted, because it reads the real file.
- Future work that reads AGENTS.md rather than R6.10 has no guard against the PRIV-1 path. That work includes OQ 3's "revise R6.1–R6.7 and UJ 3 from them", the help docs on exporting from the Toolkit (`post-lock.md:217`), and examples in bug reports or ADRs.
- Fix: one §5 line beside the SDK and license rule: "A real vendor-app export — anyone's collection — and any value from it (names, codes, notes, collection names, dates, counts, measured values, screenshots) never enter the repo; fixtures are synthetic (Import R6.10)."

**[INFO] PRIV2-3 — `prd-inventory-import-copy.md:34` (E48)** lists what comes in as details (Note and the Density columns) but not R6.2's "any other column". A column added by a later Toolkit version, possibly personal, is disclosed only by E43's Added columns line. That is adequate, but E48 could add "and any other column, listed in the preview."

#### Biggest privacy risks
1. **PRIV2-1:** the dogfood record for M2 and OQ 3 is the next likely place a real collection name, codes or counts land in the public repo. The rows license "outcomes" without bounding them.
2. **PRIV2-2:** the rule that kept this amendment clean lives in one Import row and a one-off local check, not in the rules every agent reads first.
3. **Residual (settled, disclosed):** imported readings are permanent short of deleting the swatch. That is N6, N14 and DF R2.3, and E43 now says so.

#### Genuinely privacy-respecting
- **Nothing from the real export is in the branch.** Checked with count-only matching against the real file: fixtures, DJ6, the Collection Mode cases, the review log (the verbatim round-1 reviews included) and commit messages hold no real value. The rewritten 417fdac is on no ref and the branch is unpushed.
- **Absolute `/Users/...` paths in the review log are not a new disclosure.** The same form is in four earlier merged review logs, and the author name is already public in every commit. The one scratchpad path cited names only a session folder and the report's filename, not the collection.
- **Minimisation of Custom Collection Name.** It is read, never stored as a column, and ignored for an existing target. The file's own Lab, LCh, XYZ, sRGB, HEX and Index values are checked and discarded (R6.2).
- **Accuracy over fabrication.** Unknown serial, firmware, samples, basis, verdict and spread read back empty, never as a sentinel (DF R2.3j). There is no invented payload. A shallow checklist would flag "model kept, serial empty"; it is the honest, minimal choice.
- **User control over their own notes.** A Note of `undefined` never overwrites a typed note (R6.2, UJ 3 `:120`).
- **No false linkage.** An imported snapshot never occupies a device record (Device R1.22, UJ3-c at `prd-device-management-journeys.md:80`), so imported readings are never tied to the user's paired serial.
- **No data pile-up.** A same reading, current or in history, creates nothing (R6.8f, DF R2.3j), so a repeated import cannot accumulate duplicates.
- **Transparency at every surface.** The imported mark shows on the chip, in the E12 legend, to VoiceOver, in the detail and in history (CM R2.4j, R4.2d, R5.2b and e). E43 lists added columns and states what the file lacks and that imported readings are permanent. Export carries `sc_imported` and E1's empty-cell line.
- **Exports are not disclosures.** Date Saved, Note, densities and any extra column leave only in an export the user starts, to a place the user chooses. No network, telemetry or system-search flow is added.

#### Missing controls / over-collection
- No bound on what the owner's real-export dogfood results (OQ 3, M2, post-lock `:202`) may record in the repo (PRIV2-1).
- No AGENTS.md-level rule, nor any repeatable local check, keeping real vendor-app export values out of commits (PRIV2-2).
- There is no over-collection. Densities are not personal data, and unnamed extra columns are shown in E43 before commit, the same treatment the base CSV import gives any unmapped column.

| Row ID | disposition |
|---|---|
| Import-R1.5 | ABSTAIN |
| Import-R3.2 | ABSTAIN |
| Import-R3.3 | ALIGN |
| Import-R3.5 | ABSTAIN |
| Import-R3.8 | ABSTAIN |
| Import-R6.1 | ABSTAIN |
| Import-R6.2 | ALIGN |
| Import-R6.3 | ALIGN |
| Import-R6.4 | ABSTAIN |
| Import-R6.5 | ALIGN |
| Import-R6.6 | ABSTAIN |
| Import-R6.7 | ABSTAIN |
| Import-R6.8 | ALIGN |
| Import-R6.9 | ALIGN |
| Import-R6.10 | ALIGN |
| Import-R6.11 | ALIGN |
| Import-M2 | OBJECT (PRIV2-1) |
| Import-E7 | ABSTAIN |
| Import-E9 | ABSTAIN |
| Import-E10 | ABSTAIN |
| Import-E14 | ABSTAIN |
| Import-E42 | ABSTAIN |
| Import-E43 (Toolkit variant) | ALIGN |
| Import-E46 | ABSTAIN |
| Import-E47 | ABSTAIN |
| Import-E48 | ALIGN |
| Import-E49 | ABSTAIN |
| DF-R1.2 | ALIGN |
| DF-R1.6 | ALIGN |
| DF-R2.1 | ALIGN |
| DF-R2.2 | ABSTAIN |
| DF-R2.3 (with R2.3h, R2.3j) | ALIGN |
| DF-R2.4 | ABSTAIN |
| DF-R5.5 (with R5.5d) | ABSTAIN |
| DF-R7.1 | ABSTAIN |
| DF-R7.2 | ABSTAIN |
| DF-R7.7 (with R7.7o) | ALIGN |
| Device-R1.21 | ALIGN |
| Device-R1.22 | ALIGN |
| CM-R2.4 (with R2.4j) | ALIGN |
| CM-R2.7 | ABSTAIN |
| CM-R4.2 (with R4.2d) | ALIGN |
| CM-R5.2 (with R5.2b/e) | ALIGN |
| CM-R8.9 | ABSTAIN |
| CM-M2 | ABSTAIN |
| CM-E12 | ALIGN |
| Export-R1.1 (with R1.1h/i/p/s/t) | ALIGN |
| Export-R1.2 | ALIGN |
| Export-R4.2 | ALIGN |
| Export-E1 | ALIGN |
| Capture-R1.10 | ABSTAIN |
| Capture-M8 | ABSTAIN |

### product-marketing

#### Verdict
Lands after two Major fixes (there are no Blockers). Every round-1 Blocker and Major landed well. Two things still stand between this copy and shipping. First, E48 now claims the app checks values it actually ignores; that sentence was my own round-1 rewrite, adopted word for word. Second, the Toolkit remedy for E7, E9, E10 and E42 is an unwritten splice rule, so a builder has to guess four shipping strings.

#### Audience & message context (brief)
- **Reader:** the Cataloger, usually a Nix Toolkit phone-app user moving a collection over. Export E1 is read by the Data consumer, and the vision by contributors and future README or launch copy.
- **Intended takeaway:** "My Toolkit colours arrive already measured, marked as imported, and nothing about them is invented."
- **Surfaces:** Import E14/E43 Toolkit variants, E46–E49 and the Toolkit remedy rule; Collection Mode E12, the Mark labels row, and the R4.2d, R5.2b and R5.2e lines; Export E1's imported line; the vision's Problems bullet, J7 and feature note; the PRD index's v2 line.
- **Checked against:** `git diff 2a54fce f7cbde5 -- docs AGENTS.md`, the fix file, fences F84–F91, DF F66–F70 and CM F223, owner decisions N16–N23, and the rows each string relies on. No value from the research report or a real export is quoted here.

#### Findings

##### Round-1 findings — status
- **PMM1-B1 — RESOLVED.** E14 now has a Toolkit variant (`prd-inventory-import-copy.md:24`), R3.5 carves it out (`prd-inventory-import.md:82`), R6.8 names it (`:173`), and UJ 3 exercises it (`prd-inventory-import-journeys.md:133`). A residual Demo Device edge case is PMM2-m2.
- **PMM1-M1 — RESOLVED.** E12 explains "From Nix Toolkit" (`prd-collection-mode-copy.md:173`) and frames it as measurement-grade, not as a warning.
- **PMM1-M2 — RESOLVED.** The copy now shows every unrecorded field:
  - R4.2d imported (`prd-collection-mode-copy.md:295`): "samples, averaging, spread and agreement not recorded".
  - R5.2e (`:310`): "Samples and spread not recorded".
  - UJ2.1-r matches.
- **PMM1-M3 — RESOLVED.** Each variant has its own headline stating the problem (`prd-inventory-import-copy.md:32`).
- **PMM1-M4 — RESOLVED.** The Toolkit lines come first after Target, the labels are distinct ("Used as…", "Kept as earlier…", "Already here"), and the order against the above-ceiling append is stated (`copy:29`).
- **PMM1-M5 — RESOLVED.** The mode-change sentence is at `copy:29` and `copy:37`. R6.4 writes the change with the commit (`prd-inventory-import.md:169`), R6.9 names it (`:174`), and UJ 3 covers it (`journeys:130–131`).
- **PMM1-M6 — RESOLVED.** E48 is added (`copy:34`), R6.2 names it (`:167`), R3.8q adds its Continue action (`:132`), and UJ 3 reaches it (`journeys:110`). E48's wording carries a new problem (PMM2-M1).
- **PMM1-M7 — RESOLVED.** R6.9 splits the eligible readings into three counts (`:174`); UJ 3 reads "2 used as colour (TK-2, TK-3)" (`journeys:133`); and the note at `copy:37` says the three counts add up to the reading total.
- **PMM1-M8 — RESOLVED.** The Problems bullet now states only what is true, scoped "as studied" (`vision.md:13`).
- **PMM1-m1 — RESOLVED.** The chip label is "From Nix Toolkit", no longer confusable with the " (imported)" column tag (`prd-collection-mode-copy.md:260`).
- **PMM1-m2 — RESOLVED.** "Measurement condition" is bridged to the Toolkit's "Measurement Mode", and the text says "aren't directly comparable" (`copy:29`, `copy:32`).
- **PMM1-m3 — RESOLVED.** The unverified "export each mode" claim is gone (`copy:32`). The mixed-modes variant now offers no next step (PMM2-n4).
- **PMM1-m4 — RESOLVED.** E47 now says to "export the file again from the Nix Toolkit" (`copy:33`).
- **PMM1-m5 — PARTIAL.** E1 (`prd-data-export-copy.md:16`) now names "how many samples they averaged". The export has no sample-count cell. The averaging-basis cell, which R1.1t empties (`prd-data-export.md:94`), is still not named, so a consumer seeing an empty basis cell still gets no explanation. Rewrite: "⟨imported⟩ readings came from a Nix Toolkit export, which records no instrument serial, firmware, averaging basis or archived instrument readings, so those cells stay empty."
- **PMM1-m6 — RESOLVED.** J7 is marked "(In part superseded — see the update.)" (`vision.md:144`), and the index's v2 line is scoped (`docs/product/README.md:174`).
- **PMM1-m7 — RESOLVED.** The feature note leads with the benefit (`vision.md:160`), and the update is scoped to "the Nix Toolkit export studied" (`:145`).
- **PMM1-m8 — RESOLVED.** Deferred with a sound record: the post-lock documentation item (`post-lock.md:217`) and the fix file (`prd-inventory-import-round-1-fixes.md:63`).
- **PMM1-m9 — RESOLVED.** The Toolkit tokens are registered in Capture §12 (`prd-capture-mode.md:406`) and in the import copy note (`copy:37`); ⟨unchanged readings⟩ became ⟨same⟩.
- **PMM1-m10 — RESOLVED.** The fixture values are invented (`journeys:106`) and the unpushed commit was rewritten (fix file `:7`).
- **PMM1-n1 — RESOLVED.** "except" now replaces "save" at `prd-inventory-import.md:5`, `:50`, `:58` and `:80`, in Export's out-of-scope line (`prd-data-export.md:13`) and in Capture's obligation line (`prd-capture-mode.md:388`). No leftover remains in the amendment.
- **PMM1-n2 — RESOLVED.** "readings' colour values differ from the ones the Toolkit saved" (`copy:29`).
- **PMM1-n3 — RESOLVED (rejected, and the rejection is sound).** E40's reason still explains why the import waits (fix file `:70`).
- **PMM1-n4 — RESOLVED.** "Becoming current" is replaced by "Used as the swatch's colour" (`copy:29`).
- **Round-1 Missing list — RESOLVED:**
  - What the file doesn't carry is now a sentence in E43 (`copy:29`).
  - The separator control is gone from the Toolkit variant, whose actions are "Import; Cancel" (`copy:29`).
  - The near-miss state is deferred with a reason (`post-lock.md:101`).

##### New findings

**[MAJOR] PMM2-M1 — E48's body claims a check the app does not make (`prd-inventory-import-copy.md:34`).**
- **The claim:** "The file's own Lab, LCh, XYZ, sRGB and HEX are checked, not kept."
- **What the rows say:**
  - R6.6 compares only the file's first L, a and b (`prd-inventory-import.md:171`).
  - R6.2 says the file's other L, c, h, X, Y, Z, sRGB R/G/B and HEX are **ignored** (`:167`).
- **Why it matters:** the copy file's own rule (`copy:8`) is "Every promise below is backed by a requirement row", and this one isn't.
- **Reader reaction:** a migrator who used the Toolkit's HEX or sRGB in design work sees their values reported as verified. If SpectroCapture's recomputed sRGB differs from the Toolkit's, E43 flags nothing, because only Lab is compared, and the user concludes the values matched.
- **Mutation:** a build that follows R6.2 and R6.6 exactly ships a screen that is false. A build that makes the copy true would have to check HEX and sRGB, which contradicts R6.2's "ignored".
- **Severity:** Major, not Blocker. The false part concerns values that are neither stored nor shown, and the Lab check that does run covers the colour itself, so no measurement is misrepresented.
- **Origin:** this was my own round-1 PMM1-M6 rewrite, adopted verbatim.
- **Rewrite:** "The file's own Lab is checked against its spectrum. Its Lab, LCh, XYZ, sRGB and HEX aren't kept — SpectroCapture works every colour out again from the spectrum."

**[MAJOR] PMM2-M2 — The Toolkit remedy for E7, E9, E10 and E42 is a splice rule, not written strings (`prd-inventory-import-copy.md:39`, applied to `:17`, `:19`, `:20`, `:28`).**
- **The rule:** these states end "Correct it in the Nix Toolkit and export it again" "in place of their fix-the-file or fill-the-codes sentence".
- **Two readings are possible:**
  - Replace the whole sentence. Then E7 loses "You can bring in the rest", and E42 loses "nothing has been imported", the only reassurance in a state where every row was refused.
  - Replace only the clause. That keeps both.
- **"it" has no clear antecedent:** E7, E9 and E10 talk about plural rows.
- **Why nothing catches it:** R4.2 keeps shipping-string checks separate from behaviour checks, so two builders ship two different strings and no test notices. Every other variant in this file (E14 included) is written out in full.
- **Fix file mismatch:** `prd-inventory-import-round-1-fixes.md:36` says E47 carries the same sentence, but E47 (`copy:33`) reads differently.
- **Rewrite:** write each as a "Toolkit variant replaces …" clause in its own row:
  - E7: "You can bring in the rest, or export the collection again from the Nix Toolkit and try again."
  - E9: "Bring in the rest, or give these colours a code in the Nix Toolkit and export the collection again."
  - E10: "Bring in the rest, or give each of these colours its own code in the Nix Toolkit and export the collection again."
  - E42: "Correct them in the Nix Toolkit and export the collection again; nothing has been imported."
  - Then delete line 39, or reduce it to a one-line pointer.

**[MINOR] PMM2-m1 — E43's history line gives the wrong reason in two cases (`copy:29`).**
- The line reads "Kept as earlier readings: ⟨history⟩ — a swatch you scanned here keeps that scan."
- R6.9 (`:174`) also counts R6.8d's earlier-dated and same-dated imported readings under ⟨history⟩. Re-importing an older export (UJ 3 `journeys:136`, TK-1) shows this reason although nothing was scanned.
- Rewrite: "Kept as earlier readings: ⟨history⟩ — each of those swatches keeps its current colour: your scan, or a later Toolkit reading."

**[MINOR] PMM2-m2 — "a swatch you scanned here keeps that scan" is false for a Demo Device scan (E14 `copy:24`, E43 `copy:29`).**
- R6.8g (`:186`, N18) replaces a simulated current reading with the Toolkit reading (UJ 3 `journeys:140`).
- The Capture copy tells users the Demo Device works "as it does with a real instrument", so a trial user reads their demo scans as "scanned here".
- Rewrite: "a swatch you scanned here with your instrument keeps that scan."

**[MINOR] PMM2-m3 — E43's permanence sentence is narrower than the rest of the product's copy (`copy:29`).**
- E43 says "only deleting the swatch removes them".
- Collection Mode E17 (`prd-collection-mode-copy.md:221`) says "deleting the swatch or its collection is the only thing that throws a reading away", and Data Foundation's E14 agrees.
- Use E17's wording.

**[MINOR] PMM2-m4 — "Toolkit exports don't record the instrument's serial or firmware…" (`copy:29`) is a universal claim about the vendor's product, based on one export.**
- OQ 3 is still open (`prd-inventory-import.md:205`).
- Under R6.2, "any other column imports as metadata", so a later export that carried a serial column would contradict this copy.
- Rewrite: "SpectroCapture doesn't take the instrument's serial, firmware or sample count from a Toolkit export, so they show as unknown."

**[MINOR] PMM2-m5 — E43's row counts are unqualified next to reading counts.**
- A re-import where TK-3's newer reading becomes its colour but its details are unchanged (UJ 3 `journeys:136`; compare `:135`, "rows Unchanged 3") shows "Unchanged: 3" beside "Used as the swatch's colour: 1".
- Rewrite: in the Toolkit variant, prefix the row counts with "Swatch details —".

**[MINOR] PMM2-m6 — E14's headline, "swatches you've already scanned", is wrong on a Toolkit re-import (`copy:24`).**
- It shows when a user renames colours in the Toolkit and re-exports, and those swatches were imported, not scanned.
- The post-lock "scanned" item (`post-lock.md:102`) covers only Capture's counts.
- Fix: give the Toolkit variant its own headline — "⟨n⟩ swatches that already have a colour have different details in this file" — or extend that post-lock item to E14.

**[MINOR] PMM2-m7 — The copy note at `copy:37` says E43 shows encoding and delimiter controls "always visible (R3.8n)", with no Toolkit carve-out.**
- R6.1 (`:166`) and E43's own Toolkit actions offer neither control.
- Add: "(shown fixed for a Toolkit export, R6.1)". UJ 3 `:117` catches a wrong build, hence Minor.

**[MINOR] PMM2-m8 — The new collection's scan mode is locked with no reason given.**
- R6.3 (`:168`) makes the mode not editable when a Toolkit import creates the collection, and R6.1 says every later step names the Toolkit export. No string tells the user why the control is locked.
- Add to the creation form: "Measurement condition: ⟨mode⟩, set by this Nix Toolkit export."

**[NIT] PMM2-n1** — "Used as the swatch's colour: ⟨current⟩" puts a count after a singular noun. Use "Now a swatch's colour: ⟨current⟩".

**[NIT] PMM2-n2** — "share a date with a different reading" can be read as the same day. These readings were saved at the same instant: "were saved at the same moment as a different reading already here".

**[NIT] PMM2-n3** — Two bridge phrasings: E43's "the measurement condition the Toolkit calls Measurement Mode" and E46's "— the Toolkit's Measurement Mode —". Pick one.

**[NIT] PMM2-n4** — E46's mixed-modes variant (`copy:32`) is now honest but names no next step. Acceptable while the case stays unverified; revisit under OQ 3.

**[NIT] PMM2-n5** — The set-aside line doesn't say those swatches leave set-aside, which N20's "see the set-aside decision being resolved" intends. Rewrite: "…give a colour, so they're no longer set aside: ⟨resolved⟩".

**[NIT] PMM2-n6** — The R4.2d imported line (`prd-collection-mode-copy.md:295`) has no wording for a blank Nix Device, which R6.5 records as unknown. Use "model unknown".

**[NIT] PMM2-n7** — `vision.md:13` is the vision's first use of "Toolkit"; the Nix Toolkit is only introduced at `:37`. "As studied" reads as an internal aside. The headline "stranded" is now stronger than the body supports; consider "The instrument's data has no home the owner controls."

#### Biggest risks
- **E48 overclaims (PMM2-M1).** The screen built to reassure a migrator that nothing is lost says their HEX, sRGB, LCh and XYZ were checked. They were ignored. It is the one remaining place where the copy states something untrue about what the app did with the user's data.
- **Four shipping strings are left to the builder (PMM2-M2).** The literal splice drops "nothing has been imported" from E42 and "bring in the rest" from E7, and leaves "Correct it" pointing at nothing.
- **"Scanned here" explanations fail at the edges (PMM2-m1, m2, m6).** A re-import of an older export, a trial user's Demo Device scans, and a re-import of renamed colours each show a reason or headline that doesn't match what happened.

#### Genuinely strong
- **E43's Toolkit variant now tells a story.** It says what arrived, what becomes each swatch's colour, what's kept and why, what changes about the collection, what couldn't be checked, what the file doesn't carry, and that nothing is thrown away. That is the confirmation owner decision N5 intended.
- **E12's "From Nix Toolkit" sentence is the right framing.** "Its colour is worked out from the spectrum in that export, as a scan's is" treats the imported reading as measurement-grade, names the unknowns plainly, and tells the user how to replace it. It avoids warning-label tone, which would have undersold a good reading.
- **E46 per variant.** Each headline states the problem, the bridge to the Toolkit's term is present, "one with no readings yet" gives a real third route, and every variant ends "Nothing has been imported".
- **The vision is honest.** The founding problem no longer contradicts the vision's own evidence, the claim is scoped to the one export studied, J7's superseded status is visible, and the feature note leads with the benefit ("without re-scanning").
- **Neutral and plain throughout.** "Nix Toolkit" and "Nix Spectro 2" are used only to identify the source. Nothing disparages the vendor app, and no hype was added. For a migration feature, calm, specific copy is the right register, and it was kept.
- **E49 gives a reason the user can accept:** "so codes from different ones can't collide".

#### Missing / over-hyped
- **Missing:**
  - Written-out Toolkit strings for E7, E9, E10 and E42 (PMM2-M2).
  - A reason line for the locked scan mode on the creation form (PMM2-m8).
  - A Toolkit headline for E14 (PMM2-m6).
  - E1 naming the empty averaging basis (PMM1-m5, partial).
  - A next step for the mixed-modes variant, once OQ 3 shows whether the case occurs.
- **Over-hyped:** nothing in tone. The two overclaims are both about scope:
  - E48's "checked" covers four value sets the app ignores (PMM2-M1).
  - E43's "Toolkit exports don't record…" states a vendor-wide fact from one file (PMM2-m4).

| Row ID | disposition |
|---|---|
| Import-R1.5 | ABSTAIN (out of lens) |
| Import-R3.2 | ABSTAIN (out of lens) |
| Import-R3.3 | ALIGN |
| Import-R3.5 | ALIGN |
| Import-R3.8 | ALIGN |
| Import-R6.1 | ALIGN |
| Import-R6.2 | ALIGN |
| Import-R6.3 | ALIGN |
| Import-R6.4 | ALIGN |
| Import-R6.5 | ABSTAIN (out of lens) |
| Import-R6.6 | ALIGN |
| Import-R6.7 | ALIGN |
| Import-R6.8 | ALIGN |
| Import-R6.9 | ALIGN |
| Import-R6.10 | ABSTAIN (out of lens) |
| Import-R6.11 | ALIGN |
| Import-M2 | ABSTAIN (out of lens) |
| Import-E7 | OBJECT (PMM2-M2) |
| Import-E9 | OBJECT (PMM2-M2) |
| Import-E10 | OBJECT (PMM2-M2) |
| Import-E14 | ALIGN |
| Import-E42 | OBJECT (PMM2-M2) |
| Import-E43 (Toolkit variant) | ALIGN |
| Import-E46 | ALIGN |
| Import-E47 | ALIGN |
| Import-E48 | OBJECT (PMM2-M1) |
| Import-E49 | ALIGN |
| DF-R1.2 | ABSTAIN (out of lens) |
| DF-R1.6 | ABSTAIN (out of lens) |
| DF-R2.1 | ABSTAIN (out of lens) |
| DF-R2.2 | ABSTAIN (out of lens) |
| DF-R2.3 (with R2.3h, R2.3j) | ABSTAIN (out of lens) |
| DF-R2.4 | ABSTAIN (out of lens) |
| DF-R5.5 (with R5.5d) | ABSTAIN (out of lens) |
| DF-R7.1 | ABSTAIN (out of lens) |
| DF-R7.2 | ABSTAIN (out of lens) |
| DF-R7.7 (with R7.7o) | ABSTAIN (out of lens) |
| Device-R1.21 | ABSTAIN (out of lens) |
| Device-R1.22 | ABSTAIN (out of lens) |
| CM-R2.4 (with R2.4j) | ALIGN |
| CM-R2.7 | ALIGN |
| CM-R4.2 (with R4.2d) | ALIGN |
| CM-R5.2 (with R5.2b/e) | ALIGN |
| CM-R8.9 | ALIGN |
| CM-M2 | ABSTAIN (out of lens) |
| CM-E12 | ALIGN |
| Export-R1.1 (with R1.1h/i/p/s/t) | ALIGN |
| Export-R1.2 | ALIGN |
| Export-R4.2 | ABSTAIN (out of lens) |
| Export-E1 | ALIGN |
| Capture-R1.10 | ALIGN |
| Capture-M8 | ABSTAIN (out of lens) |

As in round 1, Minors and Nits are listed but not raised as objections: PMM2-m1, m2, m4, m5, m7, n1, n2, n3 and n5 (Import-E43; m2 also Import-E14); m3 (Import-E43); m6 (Import-E14); m8 (Import-R6.3); PMM1-m5 PARTIAL (Export-E1); n4 (Import-E46); n6 (CM-R4.2). n7 falls on the vision, which has no rows.

### plan

#### Verdict
Execute after fixing Blockers. No Blockers remain: B1 is resolved, and eight of the nine round-1 Majors are too. The fixes did add five Majors, each of which either makes a correct build fail a new case or forces the builder to guess, and the downstream half of the build order is still unnamed. All of it is small and belongs in one fix pass before the build.

#### Findings

Paths are relative to /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import.
- IMP / IJ / IC = docs/product/import/prd-inventory-import{,-journeys,-copy}.md
- IFX = docs/product/import/prd-inventory-import-round-1-fixes.md
- DF / DFJ / DFF = docs/product/data-foundation/prd-data-foundation{,-journeys,-fences}.md
- CM / CMJ / CMC = docs/product/collection-mode/prd-collection-mode{,-journeys,-copy}.md
- EX = docs/product/export/prd-data-export.md
- DVJ = docs/product/device-management/prd-device-management-journeys.md
- CAP = docs/product/capture-mode/prd-capture-mode.md
- PL = docs/product/post-lock.md
- LOG = docs/agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md

##### Round-1 findings: status
- **B1 RESOLVED**:
  - R6.9 now partitions the counts, and R6.8a counts as becoming current (IMP:174).
  - IJ:133 reads "2 used as colour (TK-2, TK-3)"; IJ:117 reads "3 used".
- **M1 RESOLVED**:
  - R6.8f runs first and checks current and history (IMP:173, :182). The Vocabulary defines "same reading" (IMP:23), and DF R2.3j "creates none" (DF:137).
  - Cases: re-import after R6.8c (IJ:134); A/B/A (IJ:136–137); same date with a different spectrum under N19 (IJ:138); Flag (IJ:139); form independence (IJ:141).
- **M2 RESOLVED**:
  - The grammar is stated (IMP:172), and R6.7 runs before the mode check (IMP:169).
  - The fixture uses scientific notation (IJ:104; TK-3 `1.04700000e+0` at IJ:106).
  - "exactly M0, M1 or M2" excludes M3.
  - Cases at IJ:125–126.
- **M3 RESOLVED**: the file's values come from an independent reference (IJ:104; IMP:175). The new tolerance margin is a separate problem, R2-M3.
- **M4 RESOLVED**:
  - The fixture values are now invented (IJ:106), and Round 0 is redacted (LOG:18, :32).
  - Declining a new owner question is recorded and sound under N3 (IFX:71).
  - I did not re-run the local-only value check (LOG:46).
- **M5 RESOLVED**: IMP:167 (read for R6.3/R6.11, not stored); IJ:119.
- **M6 PARTIAL**:
  - The rows are fixed: the adoption is written by the commit (IMP:169), R3.8k rechecks the fit (IMP:126), the mode is fixed at creation (IMP:168; IJ:114), and there is a Cancel/E44 case (IJ:131).
  - But IJ:132, the case added for M6(b), contradicts R6.4 (R2-M4).
- **M7 RESOLVED**: IMP:170 (the file's pair kept as provenance); IMP:171 (unchecked records import, N22); IJ:123 (D65/10° case). A residual definition gap is R2-m2.
- **M8 RESOLVED**:
  - DF R2.1 (DF:112); DF F66 (DFF:765).
  - DJ6 asserts both orders (DFJ:115–116).
  - R6.8c now says "not current" (IMP:185).
- **M9 RESOLVED**: AGENTS.md:50 (§3); AGENTS.md:108–109 (§8).
- **m1 RESOLVED**: IMP:173 (keyed on snapshot kind; a restore takes its source's); IMP:186 (R6.8g).
- **m2 RESOLVED**: DF:137 (reasons and the unrecorded fields); DF:116 (R2.4 exception); DFJ:113.
- **m3 RESOLVED**: CMC:295, :307, :310; CMJ:236.
- **m4 RESOLVED**: IC:29 uses "Already here: ⟨same⟩"; the token is registered in Capture §12.
- **m5 RESOLVED**: IMP:166; IC:29 ("Toolkit variant: Import; Cancel"). A residual E5 edge is R2-m4.
- **m6 RESOLVED**: IMP:171 (unreadable file Lab is listed as not checked); IMP:170 (blank Nix Device); IMP:167 (any other column becomes metadata).
- **m7 RESOLVED**: IJ:104 ("bare `;` with no padding"); IJ:106 (codes, Index 1–3, densities, Density Status).
- **m8 RESOLVED**: IJ:142 (R6.8e); IJ:116 (name ignored); IJ:134 (re-import after R6.8c).
- **m9 RESOLVED**: CM:574 (ZX-022 is in M2's population).
- **m10 RESOLVED**: EX:205 (OQ 4); PL:12.
- **m11 RESOLVED (deferred)**: PL:102 and IFX:61. The deferral is sound: it is a copy question for Capture, and the tallies stay correct.
- **m12 RESOLVED**: IMP:205 (OQ 3); IMP:195 (M2); PL:202.
- **n1 RESOLVED**: IMP:167 (Index ignored).
- **n2 RESOLVED**: IC:32 gives actions per variant and adds "one with no readings yet".
- **n3 RESOLVED**: IMP:21; IMP:136.
- **n4 RESOLVED**: IMP:176 ("the same under R2.3").
- **G1 PARTIAL**: IMP:32 now names §6's upstream order (Device R1.21, DF R2.3j, derivation without an instrument, and DF OQ 6's tolerance gating release). The downstream half is still unnamed; see Spec coverage gaps.
- **G2 PARTIAL**: DF R7.7o (DF:268) and DF R7.7 (DF:245) now declare the imported fixture. But Export R4.2 still enumerates "R7.7a–n" (R2-M5).

##### New in round 2

[MAJOR] R2-M1 — Import R6.10 and DF R7.7 against Collection Mode's seeded file — how an imported-reading fixture is produced
- **What the rows say:**
  - IMP:175: "Every checked-in fixture or golden holding an imported reading derives from" a synthetic export.
  - DF:245: "imported readings come from Import R6.5 on a synthetic export". This parallels "simulated snapshots use Device R6.9", so it reads as: generated by running the importer.
  - CMJ:236 (UJ2.1-r) seeds ZX-022 "at ZX-001's working-set value", a declared derived value. CM's harness says "Fixture values are declared, never copied from the implementation" (CMJ:28).
  - Other consumers seed the same way: CM M2, every PR (CM:574); UJ2.1-s (CMJ:237); Device UJ3-c (DVJ:80).
- **Scenario:** the Collection Mode builder has two options, and each breaks a rule.
  - Run the importer on a synthetic export. The derived values then come from the app, against CMJ:28, and hitting a declared working-set Lab would need a spectral inversion.
  - Declare the imported reading directly. That goes against IMP:175 and DF:245.
  - Under the first reading, every one of these harnesses also waits on Import §6, which no build-dependency line says (see Spec coverage gaps).
- **Fix:** state which is meant in both IMP:175 and DF:245.
  - Recommended: another PRD's fixture may declare an imported reading directly, with an imported-kind snapshot and a spectrum invented under R6.10, its derived values declared (or taken from the independent reference where a case compares them). Only Import's own cases and DF R7.7o run the importer.

[MAJOR] R2-M2 — CM UJ2.1-s (CMJ:237) asserts a variant a first-phase build does not produce
- **Problem:**
  - The case reads "E17 in its restore variant".
  - That variant is marked `[phase: variant-absent]` until R5.5, which is P1, lands (CMC:223; R5.3 at CM:385).
  - The harness runs a case "in every build from the phase that lands its rows" (CMJ:36), and UJ2.1-s cites R2.4 and R5.3, both P0.
- **Scenario:** a correct first-phase build renders E17's base body and fails the variant assertion.
- **Fix:** either of these.
  - Read E17 in Recorded order, then Measured order, without naming a variant; CMJ:36's rule then asserts the restore variant only once R5.5 has landed.
  - Add "R5.5 built" to the Given, as UJ4.7-f does (CMJ:360).

[MAJOR] R2-M3 — Import UJ 3's below-tolerance case depends on a derivation method nothing states (IJ:122, IJ:118, IJ:104; R6.6 at IMP:171; DERIVATION_TOLERANCE candidate 0.1 at DF:328)
- **Problem:**
  - IJ:122 sets TK-1's file Lab 0.05 ΔE2000 from "its spectrum's" Lab, which is the independent reference's (IJ:104), and expects TK-1 not to be listed.
  - That leaves 0.05 ΔE2000 for any disagreement between the app's spectrum-to-XYZ method and the reference's.
  - Neither method is stated in any PRD: no weighting or tabulation, no interpolation, and no treatment of the 400–700 nm truncation.
- **Scenario:** the app and the reference differ in how they weight or fold the truncated ends. TK-1's result then turns on the author's choice of library method, not on the code, and a correct build can fail IJ:122 or IJ:118. The case that was added to answer round 1's "only as strong as the author's method" comment makes this dependence sharper.
- **Fix:** either of these.
  - UJ 3's preamble names the one tabulation that both the reference and the app's derivation of a 400–700 nm, 10 nm spectrum use, at D50/2° and D65/10°.
  - Or it states the maximum disagreement the case assumes and sizes the offset to fit.

[MAJOR] R2-M4 — Import UJ 3:132 contradicts R6.4 (IMP:169) and UJ 3:130
- **Problem:**
  - IJ:132's Given is an M2 target holding only pending items; the user previews, switches the target to M1, then imports. The case expects E46.
  - R6.4 says a target holding no reading fits and adopts the file's mode at commit. R3.8k (IMP:126) refuses only "a target that no longer fits".
  - At commit, the target is exactly IJ:130's Given, where the commit adopts M2.
- **Scenario:**
  - A build that follows the rows adopts M2 and fails IJ:132.
  - Passing IJ:132 means inventing a rule no row states: "mode changed since the preview → E46".
- **Fix:** either of these.
  - Change IJ:132's Given to an M2 target holding a captured item with an M2 reading. Switching it to M1 then really no longer fits, which is the recheck M6(b) asked for.
  - Or add to R3.8k: "a target whose scan mode changed since the preview → E46 (or a fresh preview, as E13)".

[MAJOR] R2-M5 — Export R4.2 (EX:145) contradicts its fence
- **Problem:**
  - This round changed R4.2 to assert "all three `sc_simulated` and `sc_imported` values".
  - It still enumerates only "DF R7.7a–n", none of which holds an imported reading.
  - Export's Data Foundation line (EX:180) and DF's Data Export line (DF:281) still call R7.7a–n the sole inventory.
  - DF R7.7 (DF:245) says a–o, and Export F35's Clarified line says R4.2 asserts against R7.7o.
- **Scenario:** goldens generated from R4.2's own list include none for R7.7o, so `sc_imported` true is never asserted.
- **Fix:** mechanical. Change "R7.7a–n" to "R7.7a–o" in all three places.

[MINOR] R2-m1 — DF R5.5d (DF:187)
- Its salvage rule keeps "the later-recorded [current reading] … unless it is imported and the other is not". That keeps a simulated reading over an imported one, against R2.3j and DF F68 (DFF:777, "only a live A stays current").
- Between two imported readings it keeps the later-recorded rather than the later-measured, against F69 (DFF:783).
- Fix: keep the one R2.3j would make current — live over imported, imported over simulated, later Date Saved between two imported; otherwise the later-recorded.

[MINOR] R2-m2 — Import R6.6 (IMP:171)
- "whose illuminant and observer the app cannot yet work under" is not defined.
- Capture R1.5 (CAP:142) requires the derivation to be reference-parameterised. A build that ships D65/10° tables could therefore check TK-2 and fail IJ:123.
- Fix: tie the phrase to the offered reference set (Capture OQ 21 at CAP:446, interim D50/2°, test-declarable under Capture R11.6).

[MINOR] R2-m3 — Import R6.4 and R6.11 (IMP:169, :176)
- Only R6.7's exclusions are ordered before the mode check. R6.11's place relative to E7, E9, E10 and R6.7 exclusions and to R6.4 is unstated, as is which of E46 and E49 wins when both apply.
- IJ:126 expects E46 "then" after E47, but its only Action is "Read the file", while E47 waits on "Continue without them" (IMP:118).
- Fix: state the order in one sentence, and add "Continue without them" to IJ:126's Action.

[MINOR] R2-m4 — Import R6.1 (IMP:166) against E5 (IC:15) and R3.8a (IMP:116)
- A recognised Toolkit export whose later records fail UTF-8 decoding reaches E5, which offers "Choose an encoding".
- Nothing says whether choosing Windows-1252 keeps the file a Toolkit export, which R6.1 says is "read as UTF-8, shown fixed".
- Fix: say one or the other; OQ 3 can revisit later.

[MINOR] R2-m5 — Import R6.7 (IMP:172), R6.5 (IMP:170) and the Vocabulary (IMP:23)
- R6.7 allows any number of fractional-second digits. The measurement time is stored, and the same-reading check compares, "to the millisecond".
- Whether extra digits are truncated or rounded is unstated, and it changes both the stored time and R6.8f's equality.
- Fix: state truncation or rounding.

[MINOR] R2-m6 — Import E14 and E43 copy (IC:24, IC:29)
- E14's Toolkit variant says "a swatch you scanned here keeps that scan as its colour". E43's "Kept as earlier readings" line repeats the scan clause.
- For an item captured with the Demo Device, R6.8g (IMP:186, F86) makes the Toolkit reading current, so the promise is false.
- ⟨history⟩ also counts R6.8d's kept imported readings, which have no scan behind them.
- Copy checks are kept separate from behaviour tests (R4.2), so no case catches this. The product-marketing lens owns the wording.

[NIT] R2-n1 — IJ:110 goes from "no target yet" straight to E48, skipping the target step that R2.1 and R6.3 place before mapping. Add "create a target" to the Action.

[NIT] R2-n2 — IJ:133 does not declare TK-2's pending item's Swatch Name or other fields, yet "Updated 1 (TK-1)" needs them to match Fixture T. Declare them.

[NIT] R2-n3 — IFX:26 records TK-3 as `1.04700000e+00`. IJ:106 and IJ:118 write `1.04700000e+0`, which follows the stated one-digit-exponent form (IJ:104). The journeys are right; the fix record is off.

#### Biggest risks (if executed as-is)
1. **Harness stall, or quiet rule-bending.** Collection Mode, Device and Export each have to guess how to seed an imported reading (R2-M1). With the downstream order unnamed, one of them may be built before the thing it needs.
2. **A first-phase Collection Mode build goes red on UJ2.1-s (R2-M2).** An agent may "fix" it by producing the P1 restore variant early.
3. **A flaky oracle.** Whether the import's below-tolerance case passes depends on which weighting method the fixture author picks, not on the code (R2-M3). The same unstated method decides whether real exports flag every record, which M2 would only reveal at dogfood.
4. **An invented stale-preview rule.** To pass IJ:132, the builder has to add a rule that contradicts R6.4 (R2-M4).
5. **`sc_imported` true is never asserted** if the goldens follow R4.2's a–n list (R2-M5).

#### Plan strengths
- **Round-1 fixes landed as rows with cases that tell right from wrong.**
  - The E43 counts are consistent.
  - The same-reading rule checks current and history, with A/B/A, Flag and form-independence cases.
  - The value grammar and fixture format are stated, and the oracle is independent.
  - Every Toolkit column's fate is stated.
  - The mode adoption happens inside the commit, with Cancel and E44 cases.
  - The illuminant and observer are kept as provenance.
  - History order is asserted both ways, in DJ6 and in UJ2.1-s.
  - AGENTS.md, the ADR-0003 inputs and the upstream build order are all in place.
- **Owner decisions N16–N23 are carried cleanly.** Each has a fence, the older fences carry dated Clarified lines, and the fence→row maps list them (IMP fences :341–349; DF F66–F70; CM F223).
- **R6.8's dispatch on the current reading's snapshot kind matches DF exactly.** R2.3j's reasons, R2.3h's predecessor rule (a reading kept behind is never a predecessor) and R2.4's exception all agree with it, and DJ6 exercises every branch.
- **Suspected problems I checked and ruled out:**
  - `e+0` in the journeys is the correct form; the fix file is the one that is off.
  - DJ6's measured order (DFJ:115–116) is oldest first, which matches CM R5.3.
  - IJ:120's TK-2 Note was previously absent, so R3.6d applies and no E14 is correct.
  - DJ6's re-scan rows use states DF declares under R7.2, so there is no phase conflict.
  - The E47 record numbers count the header as record 1, per R1.4.
  - R6.9's three counts add up to ⟨readings⟩ in IJ:117, :133, :134, :136 and :137.

#### Spec coverage gaps (requirements with no task)
- **The downstream half of G1 is still unnamed.** The brief requires "Collection Mode R2.4j after both" Device R1.21's imported kind and DF R2.3j, and nothing states it.
  - CM's Build dependencies row 1 (CM:205) lists R2.1–R2.8 with only ADR-0003 as a stop; row 2 (CM:206) does the same for R4.2 and R5.2.
  - CM's "Phases and builds" sibling-row list (CMJ:36–47) names neither Device R1.21's imported kind nor DF R2.3j.
  - DF R7.7o (DF:268), Export R4.2's `sc_imported` goldens (EX:145) and Device UJ3-c (DVJ:80) name no predecessor.
  - Post-lock's First build PR section (PL:141–166) has no Toolkit item.
  - If R2-M1 is settled as "generated by the importer", all of these also wait on Import §6.
  - **Fix:** add one CM Build-dependencies row covering R2.4j, the imported halves of R2.7, R4.2d and R5.2b/e, UJ2.1-r/s and M2's imported item: "after the device PRD's R1.21 imported kind and the Data Foundation PRD's R2.3j [and the import PRD's §6]". Add one post-lock First-build-PR item naming the chain.

| Row ID | disposition |
|---|---|
| Import-R1.5 | ALIGN |
| Import-R3.2 | ALIGN |
| Import-R3.3 | ALIGN |
| Import-R3.5 | ALIGN |
| Import-R3.8 | OBJECT (R2-M4, R2-m4) |
| Import-R6.1 | OBJECT (R2-m4) |
| Import-R6.2 | ALIGN |
| Import-R6.3 | ALIGN |
| Import-R6.4 | OBJECT (R2-M4, R2-m3) |
| Import-R6.5 | OBJECT (R2-m5) |
| Import-R6.6 | OBJECT (R2-M3, R2-m2) |
| Import-R6.7 | OBJECT (R2-m5) |
| Import-R6.8 | ALIGN |
| Import-R6.9 | ALIGN |
| Import-R6.10 | OBJECT (R2-M1, R2-M3) |
| Import-R6.11 | OBJECT (R2-m3) |
| Import-M2 | ALIGN |
| Import-E7 | ALIGN |
| Import-E9 | ALIGN |
| Import-E10 | ALIGN |
| Import-E14 | OBJECT (R2-m6) |
| Import-E42 | ALIGN |
| Import-E43 (Toolkit variant) | OBJECT (R2-m6) |
| Import-E46 | ALIGN |
| Import-E47 | ALIGN |
| Import-E48 | ALIGN |
| Import-E49 | ALIGN |
| DF-R1.2 | ALIGN |
| DF-R1.6 | ALIGN |
| DF-R2.1 | ALIGN |
| DF-R2.2 | ALIGN |
| DF-R2.3 (with R2.3h, R2.3j) | ALIGN |
| DF-R2.4 | ALIGN |
| DF-R5.5 (with R5.5d) | OBJECT (R2-m1) |
| DF-R7.1 | ALIGN |
| DF-R7.2 | ALIGN |
| DF-R7.7 (with R7.7o) | OBJECT (R2-M1, G1) |
| Device-R1.21 | ALIGN |
| Device-R1.22 | ALIGN |
| CM-R2.4 (with R2.4j) | OBJECT (R2-M1, R2-M2, G1) |
| CM-R2.7 | ALIGN |
| CM-R4.2 (with R4.2d) | ALIGN |
| CM-R5.2 (with R5.2b/e) | ALIGN |
| CM-R8.9 | ALIGN |
| CM-M2 | OBJECT (R2-M1) |
| CM-E12 | ALIGN |
| Export-R1.1 (with R1.1h/i/p/s/t) | ALIGN |
| Export-R1.2 | ALIGN |
| Export-R4.2 | OBJECT (R2-M5, G1) |
| Export-E1 | ALIGN |
| Capture-R1.10 | ALIGN |
| Capture-M8 | ALIGN |

### database

#### Verdict
Sound once one new Major is fixed. The round-1 Blocker and all seven round-1 Majors are resolved in the rows, the cases and the ADR-0003 inputs. The fix pass introduced one Major: DF R5.5d's salvage exception contradicts DF F67–F69 on three pairs of readings. It also introduced four Minors. Each is a one-line fix.

#### Schema & engine (brief)
- **No DDL exists.** There is no code yet, and ADR-0003 (the storage schema) is still queued (ADR:22). The requirement rows remain the data contract. I traced them against the logical entities they imply:
  - **Item:** its code is unique per collection under Import R2.3.
  - **Reading:** a per-item sequence, measurement and record times, a supersession reason, an explicit current selection, an explicit predecessor (new this round), and a copied snapshot of kind live, simulated or imported. An imported reading also carries the file's illuminant/observer pair as provenance.
  - **Samples:** an imported reading has zero.
  - **Derived sets:** one per reading × condition.
  - **Collection:** its chosen scan mode.
- **Engine.** SQLite; GRDB is recommended. Outside reads run at SQLITE_READER_FLOOR (DF OQ 2, still open). `PRAGMA foreign_keys`, the journal mode and `busy_timeout` are still decided nowhere; they are ADR-0003's.
- **What I read.** `git diff 2a54fce f7cbde5 -- docs AGENTS.md`; all 14 targets and the contract sources, in full or at every changed row; my round-1 section; N16–N23; and the fix file.

Path legend (all absolute; "X:n" means line n of that file):
- IMP = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import.md
- IMPJ = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import-journeys.md
- IMPC = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import-copy.md
- IMPF = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import-fences.md
- DF = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/data-foundation/prd-data-foundation.md
- DFJ = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/data-foundation/prd-data-foundation-journeys.md
- DFF = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/data-foundation/prd-data-foundation-fences.md
- EX = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/export/prd-data-export.md
- EXJ = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/export/prd-data-export-journeys.md
- CMJ = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/collection-mode/prd-collection-mode-journeys.md
- CAP = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/capture-mode/prd-capture-mode.md
- ADR = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/decisions/README.md
- PL = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/post-lock.md

#### Findings

##### Round-1 findings — status

**Blocker**
- **DB1-B1 (imported readings deduplicated only against the current reading) — RESOLVED.**
  - R6.8f now runs first, against every reading the item holds, and "a Flag, a restore or a set-aside decision stands" (IMP:173, IMP:182; Vocabulary IMP:23; F82 Clarified IMPF:246; DF R2.3j DF:137).
  - Cases: IMPJ:134, :137, :139, :141 and DFJ:120.
  - The ADR-0003 input says the same-reading key excludes restores (ADR:22).
  - One corner remains: DB2-m1.

**Majors**
- **DB1-M1 ("equal" and "as given" undefined) — RESOLVED.** Each part landed:
  - (a) Sameness is judged on parsed values: IMP:23 and IMPJ:141.
  - (b) Numbers are stored as numbers: IMP:170, with IMPJ:118's "stored as 1.047 … equal as numbers".
  - (c) The equal-date tie is assigned (N19): IMP:187.
  - (d) Millisecond precision: IMP:170.
  - A Nit remains: DB2-n2.
- **DB1-M2 (history order) — RESOLVED.**
  - N16 is recorded as F66 (DFF:765), and DF R2.1 is reworded (DF:112).
  - DFJ:115–116 and CMJ:237 (UJ2.1-s) discriminate the two orders.
  - CM R5.3 is unchanged and agrees: the measured order runs oldest first, with ties broken by record time.
- **DB1-M3 (predecessor not recoverable from sequence) — RESOLVED.**
  - DF R2.3h (DF:135) fixes the predecessor as the reading current when B landed.
  - The reason cell (DF:137, "else initial") also covers a reading kept behind an imported A.
  - DFJ:117 is the discriminating case; ADR:22 stores the predecessor explicitly.
- **DB1-M4 (mode fit not rechecked at commit) — RESOLVED.**
  - R3.8k (IMP:126) rechecks the fit at commit; R6.4 (IMP:169) says the adoption is "written by the commit and undone with it"; R3.2 (IMP:79).
  - Cases IMPJ:131 and :132; Capture R1.10 (CAP:147).
  - A related gap remains: DB2-m2.
- **DB1-M5 (NaN/Infinity readable; unreadable Lab and pair unruled) — RESOLVED.** R6.7 (IMP:172) requires finite numbers; R6.6 (IMP:171) lists such records "never as matching"; cases IMPJ:123, :125, :126.
- **DB1-M6 (`undefined` Note stored as an empty string) — RESOLVED.** R6.2 (IMP:167); cases IMPJ:119 and :120.
- **DB1-M7 (reading counts undefined, oracles contradictory) — RESOLVED.**
  - R6.9 (IMP:174) partitions the eligible readings, and IMPC:37 says the three counts sum to ⟨readings⟩.
  - IMPJ:133 now counts 2 used as colour (TK-2, TK-3), and IMPJ:136 asserts its counts.

**Minors**
- **Illuminant/observer stored as a set's reference — RESOLVED.** IMP:170 ("never a derived set's reference"), DF:137, IMPJ:123.
- **"Hold no reading" undefined — RESOLVED.** IMP:169 and IMPF:208.
- **How R6.8c/d tell a scan from an import — RESOLVED.** IMP:173 (the current reading's kind, a restore carrying its source's) and R6.8g (IMP:186).
- **DF vocabulary and floor-readable lists — RESOLVED.** DF:37, :99, :112, :239; DFJ:114. A restore copies the imported state (DF:137, DFJ:123).
- **Salvage can promote an imported reading over a scan — PARTIAL.** The exception landed (DF:187) but over-reaches on three pairs → DB2-M1.
- **DF R2.4 not carved out for R2.3j — RESOLVED.** DF:116.
- **R3.2 names only R3.3a–d — RESOLVED.** IMP:79.
- **R6.2 silent on unlisted columns — RESOLVED.** IMP:167; IMPJ:113 and :119.
- **No fixture yields `sc_imported` true — PARTIAL.**
  - Landed: DF R7.7o (DF:268), R7.7 (DF:245), EX:88–94 and EXJ:14.
  - Missing: Export R4.2 (EX:145) still enumerates only R7.7a–n → DB2-m3.
- **Out-of-grid branch depends on Export OQ 4 — RESOLVED.** EX:205 and PL:12.

**Nits**
- **Index "read for R6.6" — RESOLVED.** IMP:167.
- **Unknown serial/firmware should read back as NULL — PARTIAL.** The placeholder is barred (DF:137, ADR:22), but the rows say "empty", not NULL → DB2-n1.
- **Capture R1.10 vs R1.11 — RESOLVED.** CAP:147 ("other than that commit").

##### New findings

**[MAJOR] DB2-M1 — DF R5.5d (salvage of two current readings), DF:187**
- **What landed.** My round-1 fix text, "prefer the non-imported reading", landed as "the later-recorded stays current unless it is imported and the other is not (R2.3j)". N17–N19, decided afterwards, make that wrong on three pairs. Each contradicts a DF fence and the R2.3j it cites:
  - **Imported (later-recorded) against simulated (earlier).** This is the state R6.8g leaves if the old current flag failed to clear. R5.5d keeps the simulated reading current. F68 (DFF:777) and R2.3j say only a live A stays current over B.
  - **Two imported readings.** The later-recorded one stays current, but it can be a reading R2.3j kept behind (earlier or same Date Saved; F69, DFF:783). R2.3j keeps the later-measured one.
  - **Imported against live.** The right reading (the live one) stays current. But "the other retained as correction-unconfirmed" attaches a correction question to a pair that R2.3j and F67 (DFF:771) resolve as kept behind, reason initial, no question. Answering "the old reading was wrong" then marks the Toolkit reading never true.
- **Where it bites.** DF R7.2 declares two current readings and R7.3 induces them. A test that declares imported I (later) over simulated S (earlier) asserts S current per R5.5d, so a build that follows F68 fails it; a build that follows R5.5d contradicts F68. Swapping which reading is kept in that pair passes one reading and fails the other, and no case pins it. The damage is confined to salvage output, and nothing is lost: both readings are retained.
- **Fix.** Reword R5.5d's clause to: "of two current readings, the one R2.3j would leave current stays current — a live reading over an imported one, an imported over a simulated one, the later-measured of two imported — the other kept behind as R2.3j keeps it, reason initial, no question; any other pair, the later-recorded stays current, the other retained as correction-unconfirmed (R2.2, R2.4)". Alternatively, name the imported pairs as ADR-0003's (the row already defers per-invariant rules there). Either way, add one R7.2/R7.3 case per pair.

**[MINOR] DB2-m1 — Import R6.8f against R6.8b for a quarantined imported reading (IMP:182, IMP:184; F88 at IMPF:277; ADR:22)**
- **The conflict.** R6.8b and F88 name "its current reading quarantined" as a case where the Toolkit reading becomes current, so re-importing the same export is the natural recovery. But R6.8f runs first, and "the same reading" cannot be judged against a reading whose decoded spectrum is unreadable (DF R5.5b).
- **How it bites.** A build that keys sameness on stored columns — the ADR-0003 input's "same-reading key" — matches the quarantined reading and changes nothing. With a UNIQUE index the commit fails to E44 instead. Either way F88's recovery never happens, and no case covers it.
- **Fix.**
  - R6.8f: add "a quarantined reading is never the same reading".
  - The ADR-0003 input: the key excludes quarantined readings as well as restores.
  - Case 1: TK-2's imported current reading declared quarantined (DF R7.2); re-import Fixture T → TK-2 captured with a new current reading, reason initial, the quarantined one retained.
  - Case 2: a quarantined history reading gains one readable copy; a second re-import changes nothing.

**[MINOR] DB2-m2 — Import R3.8k (IMP:126) and the R6.8 table (IMP:178–188): the R6.8 outcomes and counts are not re-evaluated at commit**
- **The gap.** R3.8k now rechecks mode fit at commit, but not the matched items' reading state that each R6.8 outcome rests on.
- **Why it's reachable.** With a preview open, CM UJ9.5-g (CMJ:544) shows Add a swatch, settings changes and a session start and end are all possible. A Flag, a restore, or a session scan on a matched item changes its outcome (R6.8b→R6.8c, or R6.8f↔R6.8b).
- **How it bites.** A build that commits the previewed plan either makes a Toolkit reading current over a fresh live scan (against F74), or writes a second current reading. The second case trips DF R2.2, and the whole file then opens read-only (E5).
- **Why a builder would guess.** The table's "Effect at commit" implies evaluation at commit, but R3.2's "the previewed import" and E43's counts point the other way. The gap predates this round; R3.8k's new explicit recheck list makes the omission visible.
- **Fix.**
  - R3.8k: "recheck source, session gate, R6.4's mode fit and each matched item's R6.8 outcome inside the commit's transaction; a changed outcome returns to a fresh preview".
  - Case: preview Fixture T into a target where TK-2 is pending; scan TK-2 in a session and end it; Import → a fresh preview counting TK-2 as kept in history.

**[MINOR] DB2-m3 — Export R4.2 (EX:145) enumerates a fixture range that omits R7.7o**
- **The gap.** R4.2 still enumerates "DF R7.7a–n plus generated corpus" while requiring "all three `sc_simulated` and `sc_imported` values". An `sc_imported` of true exists only in R7.7o (DF:268), which DF R7.7 (DF:245) now adds to the inventory.
- **Same stale range elsewhere.** EX:180 and DF's Data Export inbound line (DF:281).
- **Effect.** A build that enumerates R7.7a–n has no golden with `sc_imported` true.
- **Fix.** "R7.7a–o" in all three places.

**Nits**
- **[NIT] DB2-n1 — "empty" should be NULL.** Unknown serial and firmware "read back empty" (DF:137, IMPJ:118, DFJ:113; ADR:22 "stored empty"). In this document family "empty" means a present empty string, distinct from absent (IMP:25), and SQL's unknown is NULL. Say "NULL (absent), never '' or a placeholder", as IMPJ:119's "no item holds a Note" already treats an absent field.
- **[NIT] DB2-n2 — sub-millisecond Date Saved.** R6.7 (IMP:172) allows any fractional seconds, while R6.5 (IMP:170) keeps milliseconds and sameness (IMP:23) compares at milliseconds. Say truncate, not round.
- **[NIT] DB2-n3 — E48 overstates the check.** E48 (IMPC:34) says the file's LCh, XYZ, sRGB and HEX are "checked". R6.2 (IMP:167) ignores them, and R6.6 checks only L, a and b.
- **[NIT] DB2-n4 — ADR-0003 row editorial.** At ADR:22 the Toolkit input runs into the previous sentence ("…the cold open Input from…"), and the row still says "Takes eight inputs from Collection Mode" before it.
- **[NIT] DB2-n5 — Toolkit copy misstates what is kept.**
  - E43's "Kept as earlier readings: ⟨history⟩ — a swatch you scanned here keeps that scan" and E14's variant (IMPC:24, IMPC:29) don't match what ⟨history⟩ counts:
    - It includes R6.8c readings measured after the scan; F84 dropped "earlier" from R6.8c for exactly this reason.
    - It includes R6.8d readings where no scan is involved.
    - A Demo Device reading is not kept current (R6.8g).
  - This is for the copy lenses.

#### Biggest risks
1. **DB2-M1.** Salvage of a two-current file picks the wrong reading for imported/simulated and imported/imported pairs, and puts a spurious correction question on imported/live pairs. The first answer of "the old reading was wrong" then permanently marks a real Toolkit reading never true.
2. **DB2-m2.** A plan-at-preview build writes a second current reading when the store changes under an open preview, and the whole file then opens read-only (E5). Schema enforcement (see Missing) turns this into a clean E44 instead.
3. **DB2-m1.** Recovering a damaged imported reading by re-import silently does nothing, or fails the commit.
4. **Toolkit format drift.** A different time precision or digit count in a later app version turns one measurement into a same-dated "different" reading kept in history. It is visible through E43's same-date list (N19), so not silent; OQ 3 tracks it.

#### Genuinely sound
- **R6.8f checks the whole held set, restores included.** The ADR input's uniqueness key excludes restores, because R2.3f copies the measurement time and spectrum. The "a Flag, restore or set-aside stands" clause is what stops a re-import from quietly re-promoting history, against DF R2.9.
- **Sameness is judged on parsed values,** the instant to the millisecond and each reflectance as a number, independent of written form. IMPJ:141 proves it.
- **IMPJ:118 pins numeric storage.** Under SQLite affinity, a TEXT-stored `'1.04700000e+0'` fails "equal … as numbers" and `typeof`. An f32-packed BLOB also fails, because it rounds, which agrees with DF R1.2's plain types. A bad store cannot pass the case.
- **Current selection and predecessor are explicit.** After R6.8c no heuristic works — highest sequence, latest record time or latest measurement time can each pick the wrong reading. The PRDs say so, and ADR-0003 carries it.
- **N16 is resolved without a mixed sort key.** The recorded order stays pure record time. The measured order is oldest first with a record-time tiebreak, so it is deterministic and a restore follows its source. DFJ:116 and UJ2.1-s tell the two orders apart.
- **Mode adoption sits inside the commit.** It is rechecked at commit and rolled back on Cancel or E44 (IMPJ:131–132).
- **R6.9's partition is total.** Duplicate codes are excluded wholesale (R2.4/E10) and a code matching several items is excluded (R3.4), so each item gets at most one reading per commit and there is no ordering question inside a commit.
- **Two SQLite/IEEE traps are closed.** R6.7's finiteness rule stops NaN being stored as NULL; R6.6's "never as matching" stops a NaN ΔE from reading as a match.
- **`undefined` becomes absent, not an empty string,** so the null-versus-empty distinction is handled correctly.
- **Right calls a textbook DBA would wrongly flag:**
  - the device snapshot is copied into each reading rather than foreign-keyed to a devices table;
  - the file's reference pair is kept as provenance only;
  - Density and Note stay decoded TEXT;
  - `sc_imported` is a separate boolean rather than a three-valued `sc_simulated`;
  - Toolkit readings are not forced unique across items.

#### Missing / over-engineered
- **One round-1 ADR-0003 input was not carried.** The inputs at ADR:22 and PL:169 cover the rest of the round-1 list, but not "at most one current reading per item, enforced by the schema" — for example a partial unique index `ON reading(item_id) WHERE is_current`, which SQLite has supported for a long time. With it, DB2-m2 and any salvage bug fail closed at E44 instead of writing the E5 state. R2.2's read-only path still covers a file damaged by an outside writer.
- **The same-reading key should also exclude quarantined readings** (DB2-m1).
- **One missing case:** restore an older imported reading, then re-import the export holding the newer one → nothing changes and the restore stays current. R6.8f's "a restore stands" has no case; the Flag half has IMPJ:139.
- **Nothing is over-engineered.**

| Row ID | disposition |
| :--- | :--- |
| Import-R1.5 | ALIGN |
| Import-R3.2 | ALIGN |
| Import-R3.3 | ALIGN |
| Import-R3.5 | ALIGN |
| Import-R3.8 | OBJECT (DB2-m2) |
| Import-R6.1 | ALIGN |
| Import-R6.2 | ALIGN |
| Import-R6.3 | ALIGN |
| Import-R6.4 | ALIGN |
| Import-R6.5 | ALIGN |
| Import-R6.6 | ALIGN |
| Import-R6.7 | ALIGN |
| Import-R6.8 | OBJECT (DB2-m1) |
| Import-R6.9 | ALIGN |
| Import-R6.10 | ALIGN |
| Import-R6.11 | ALIGN |
| Import-M2 | ABSTAIN |
| Import-E7 | ABSTAIN |
| Import-E9 | ABSTAIN |
| Import-E10 | ABSTAIN |
| Import-E14 | ALIGN |
| Import-E42 | ABSTAIN |
| Import-E43 (Toolkit variant) | ALIGN |
| Import-E46 | ALIGN |
| Import-E47 | ALIGN |
| Import-E48 | ALIGN |
| Import-E49 | ABSTAIN |
| DF-R1.2 | ALIGN |
| DF-R1.6 | ALIGN |
| DF-R2.1 | ALIGN |
| DF-R2.2 | ALIGN |
| DF-R2.3 (with R2.3h, R2.3j) | ALIGN |
| DF-R2.4 | ALIGN |
| DF-R5.5 (with R5.5d) | OBJECT (DB2-M1) |
| DF-R7.1 | ALIGN |
| DF-R7.2 | ALIGN |
| DF-R7.7 (with R7.7o) | ALIGN |
| Device-R1.21 | ALIGN |
| Device-R1.22 | ALIGN |
| CM-R2.4 (with R2.4j) | ALIGN |
| CM-R2.7 | ALIGN |
| CM-R4.2 (with R4.2d) | ALIGN |
| CM-R5.2 (with R5.2b/e) | ALIGN |
| CM-R8.9 | ABSTAIN |
| CM-M2 | ABSTAIN |
| CM-E12 | ALIGN |
| Export-R1.1 (with R1.1h/i/p/s/t) | ALIGN |
| Export-R1.2 | ALIGN |
| Export-R4.2 | OBJECT (DB2-m3) |
| Export-E1 | ALIGN |
| Capture-R1.10 | ALIGN |
| Capture-M8 | ABSTAIN |


## Round 3 — delta verification (2026-09-26)

Subject: commit 7332d69, which holds round 2's fixes over f7cbde5. The same nine lenses ran, each given its own round-2 section, the round-2 fix file and the fences as sources. Each marked every round-2 finding, and every earlier finding it had left PARTIAL. Each also reported new defects and gave a disposition over every row and state at pre-alignment.

### Orchestrator verification

No lens reported a Blocker. Privacy aligned on every row it reviewed. Every new Major was checked against the branch by grep and reproduced:
- **Toolkit exports not in UTF-8.** The round-2 fix file claimed "a file that fails UTF-8 decoding is never recognised", but no row said so. The claim was also wrong: a header that decodes is recognised before a later record fails.
- **E5's quote remedy.** It said re-export, which would reproduce the same file.
- **E11 in R6.4's list.** E11 appeared among R6.4's read-time exclusions, but it needs a target.
- **Build-order item.** The post-lock item put R7.7o's goldens and DJ6 beside the importer they need.
- **Fixture T's own values.** They were tied to a reference not yet named, and that reference may not be able to supply values for invented spectra.
- **"Live" scan case.** It asked for a live scan that CI cannot produce.
- **Device UJ3-c.** It became vacuous once its reading was declared directly.

None failed to reproduce. The round-3 reviews below passed the local-only real-value check with no hits. No owner decision was needed:
- A Toolkit export that isn't UTF-8 is recognised and then refused with its own E5 variant, not dropped to a plain import. This keeps the readings from being scattered into metadata columns, within F73.
- Salvage replays two current readings in record order, matching what normal operation produced.

### What landed

The fix file [`prd-inventory-import-round-3-fixes.md`](../product/import/prd-inventory-import-round-3-fixes.md) lists every finding by ID. It covers:
- E5's Toolkit encoding and quote variants;
- E11 moved to target selection;
- the build order, and DJ6 named as R7.7o's journey;
- a named CIE calculation for Fixture T's own values, with Data Foundation R7.5 holding its spectra and offsets taken from the build's own Lab;
- the Demo Device outcome case and the saved-device case;
- the commit recheck's scope, order and write hold;
- the salvage replay, with six pairings;
- "absent (NULL)" throughout;
- the ADR-0003 row;
- the grammar, naming and quarantine cases;
- the bound on single-file dogfood records.

### product-manager

#### Verdict
Builds the right thing for the user. All eight of my round-2 findings and my four round-1 leftovers are resolved. The fixes add no Blocker. They add one Major: the fix file says a rule about UTF-8 decoding landed, but no row states it, so a re-saved Toolkit export that isn't UTF-8 has no defined path.

#### User & problem context (brief)
- **User and job:** the Cataloger, who is also the owner and first user. They have measured part or all of a collection in the Nix Toolkit app and want those readings in a SpectroCapture collection without re-scanning. The provenance must be honest, and they must be able to keep importing, scanning and correcting afterwards (vision J7, pulled into v1).
- **Validated:** one real export (Spectro 2, M2, D50/2°, one collection).
- **Assumed:** every other export has the same shape. OQ 3, M2 and the dogfood item track this, which is the right size for a one-user tool.
- **The PRD also assumes spreadsheet re-saves happen.** R6.1 recognises the comma and tab separators, and the copy note at IC:39 says Toolkit variants exist "because editing a Toolkit export in a spreadsheet can change its form". Finding PM-R3-M1 follows from that assumption.

Every path below is under `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/`:
- **IMP** `docs/product/import/prd-inventory-import.md`
- **IC** `…-copy.md`
- **IJ** `…-journeys.md`
- **R1FIX** and **R2FIX** `…-round-1-fixes.md` and `…-round-2-fixes.md`
- **DF** and **DJ** `docs/product/data-foundation/prd-data-foundation{,-journeys}.md`
- **CMC** and **CMJ** `docs/product/collection-mode/prd-collection-mode{-copy,-journeys}.md`
- **EX** and **EXJ** `docs/product/export/prd-data-export{,-journeys}.md`
- **VIS** `docs/product/vision.md`
- **PL** `docs/product/post-lock.md`

#### Findings

##### Status of earlier findings

**Round 2**
- **PM-R2-m1: RESOLVED.**
  - E43's line is now "Kept in history, not used as the colour … a swatch scanned here with an instrument keeps that scan, even where the Toolkit reading is newer". The clashes line uses the same wording (IC:29).
  - E14's Toolkit variant says "a swatch scanned here with an instrument" (IC:24).
  - The journeys still use the old wording; see PM-R3-n1.
- **PM-R2-m2: RESOLVED.**
  - E46's mixed-modes variant now points the user to a way out: "Keep each measurement condition's colours in their own Nix Toolkit collection and export each" (IC:32).
  - It is not marked provisional under OQ 3. That is acceptable: at worst the user re-measures into a new Toolkit collection.
- **PM-R2-m3: RESOLVED for its quoting scenario.**
  - R6.1 sends a quoting failure to E5's Toolkit variant, which offers no read control (IMP:166, IC:15, IJ:147).
  - The encoding half that R2FIX:24 claims did not land; see PM-R3-M1.
- **PM-R2-m4: RESOLVED.** E48 now says only the Lab is checked (IC:34).
- **PM-R2-m5: RESOLVED.** M2 counts an import as clean only if no record was excluded, nothing was refused and no record differs; not-checked records are tallied separately (IMP:195). One nit remains; see PM-R3-n2.
- **PM-R2-m6: RESOLVED.** DF R5.5d keeps current the reading R2.3j would leave current, the other kept behind with no question (DF:187). DJ:124 covers all three pairings.
- **PM-R2-m7: RESOLVED.** "R7.7a–o" now appears at EX:145, EX:180, DF:281 and EXJ:3.
- **PM-R2-n1: RESOLVED.** The Toolkit variant's row counts are labelled "Swatch details —" (IC:29).

**Round-1 findings still open after round 2**
- **PM9: RESOLVED.** E43 now gives the reason a reading is not checked (IC:29). R6.6 ties "not checked" to a pair the app offers (IMP:171), and IJ:123 declares the offered set.
- **PM-m13: RESOLVED.**
  - First half: E46 now has a way out (IC:32).
  - Second half, deferred: PL:103 records the missing warning when a collection's mode is switched away from its imported readings. The deferral is sound: switching back restores the chips, no data is lost, and the decision is named against Capture R1.10.
- **PM-n1: RESOLVED (recorded without change).**
  - R2FIX:48 keeps J7's heading so its anchor stays stable.
  - VIS:144–145 mark the risk superseded in part and add the update.
  - The fix file miscounts the documents that link the anchor; see PM-R3-n4.
- **PM8 nit: RESOLVED.** R1FIX:26 now reads `1.04700000e+0`.

##### New findings (round 3)

**[MAJOR] PM-R3-M1: a recognised Toolkit export that doesn't decode as UTF-8 has no defined path, and the fix file says it does.**
- **Where:**
  - IMP:166 (R6.1): a file is recognised from its first record, "read as UTF-8 … both shown fixed". Only a quoting failure is routed to E5's Toolkit variant.
  - IMP:57 (R1.5a): decoding errors stop at E5. The all-encodings-failed variant needs manual encoding choices that a Toolkit export never offers.
  - IC:15 (E5): the Toolkit variant's copy blames quotes, and the text variant offers "Choose an encoding".
  - R2FIX:24 says "A file that fails UTF-8 decoding is never recognised, so it reaches E5's ordinary variants". No row, copy line or case carries that sentence.
- **Scenario:**
  - The Cataloger opens their export in Excel and saves it as plain "Comma Separated Values". That is IJ:111's comma re-save, done the common way.
  - One colour name has an accent, which is now a single Windows-1252 or Mac Roman byte.
  - The header is ASCII, so R6.1 recognises the file and requires UTF-8. Decoding then fails on a later record.
- **What a builder might do, none of it pinned by a case:**
  - Show E5's Toolkit variant, whose copy wrongly blames quotes.
  - Show E5's text variant with "Choose an encoding", contradicting "shown fixed". Choosing Windows-1252 re-reads the file, R6.1 recognises it again and requires UTF-8 again, and the user is back at E5: a loop.
  - Follow the fix file's intent. The file becomes "any other source", the user picks Windows-1252, and either the same loop happens or the file imports as a plain CSV: pending items, with the readings scattered into about 50 metadata columns. That is the lossy fallback round 1's PM5 closed.
- **Frequency:** low. But this is the re-save path the PRD itself plans for, and the fix record says it is closed.
- **Fix (within F73):**
  - Add one clause to R6.1: "A recognised source that does not decode as UTF-8 throughout reaches E5's Toolkit variant for its encoding, which offers no read control."
  - Add its copy to E5: "This Nix Toolkit export has been saved again in another text encoding. Export it again from the Nix Toolkit, or save it again as UTF-8 CSV, then pick it again." Actions: Pick the file again; Cancel.
  - Add a UJ 3 case: Fixture T with TK-1's Color Name holding `é` as a Windows-1252 byte → E5's Toolkit encoding variant, no read control, nothing imported.
  - Correct R2FIX:24 to match what lands.

**[MINOR] PM-R3-m1: R6.4 lists E11 among the exclusions that run before the mixed-modes check, but E11 needs a target.**
- **Where:** IMP:169. E11 (R3.4) needs the target's items. The journeys fire E46's mixed-modes variant and E49 on "Read the file", with no target chosen (IJ:126–128).
- **Scenario (rare):** TK-2 is the file's only M1 record, and its code matches two items in the target.
  - Read literally, R6.4 excludes TK-2 through E11 and imports the rest.
  - In the journeys' order, the file is refused.
- **Fix:** list only "E7, E9, E10 and R6.7's" exclusions as running first, leaving E11 at preview. Or state that the mixed-modes and one-collection checks run at read.

**[MINOR] PM-R3-m2: E5's Toolkit remedy may send the user in a circle.**
- **Where:** IC:15. The remedy is "Export the file again from the Nix Toolkit". IJ:147 tests an unescaped `"` in a Color Name, and OQ 3 (IMP:205) asks whether the Toolkit itself writes names that way.
- **Scenario:** if the Toolkit writes the stray quote itself, re-exporting produces the same file and the user hits E5 again.
- **Fix:** "…Export it again from the Nix Toolkit — if a colour's name holds a quote mark, take it out there first — then pick it again." Or mark the remedy provisional under OQ 3.

**[NIT] PM-R3-n1: the journeys still say "kept as earlier".** IJ:136, IJ:139 and IJ:141 name the count "kept as earlier" / "kept as earlier". Yet IJ:136's own TK-1 Toolkit reading (2026-03-14) is newer than the scan (2026-02-01). The copy dropped this wording for that reason. Rename the count "kept in history".

**[NIT] PM-R3-n2: M2 lists two causes that aren't record-level faults in the file.** M2 (IMP:195) counts E5 as a record exclusion, but E5 is a whole-file failure. It also counts E11, which is a duplicate in the user's target, not a fault in the file — the same reason E46's collection variant was left out.

**[NIT] PM-R3-n3: "first record" is ambiguous in R6.3.** R6.3 (IMP:168) pre-fills the name from "the first record's Custom Collection Name". R6.1 (IMP:166) uses "first record" for the header record. Say "the first eligible data record's".

**[NIT] PM-R3-n4: the fix file miscounts the documents that link J7's anchor.** R2FIX:48 says five documents link J7's anchor. Three do: PL, EX's OQ 3 line and the Import fences. The reason for keeping the heading still holds.

#### Biggest risks   (what builds the wrong thing or fails the user)
1. **A re-saved Toolkit export that isn't UTF-8 (PM-R3-M1).** It either loops through E5 or silently falls back to a lossy plain-CSV import. No case catches a wrong build, and the fix record wrongly says the gap is closed.
2. **Toolkit remedies that assume re-exporting fixes the file (PM-R3-m2).** Until OQ 3's corpus shows how the Toolkit writes names, one refusal may send the user around in a circle.
3. **The format still rests on one studied export.** OQ 3, M2 and the dogfood item are now tracked and bounded honestly. The risk is known, not hidden.

#### Genuinely solid   (incl. where simplicity is right that a product-zealot would over-spec)
- **Commit recheck (R3.8k, E13's collection variant).** It closes the gap between preview and commit. The cases tell the outcomes apart:
  - a target still holding a reading → E46 (IJ:132);
  - a pending-only target switched to M1 → a fresh preview naming the mode change (IJ:133);
  - an item scanned while the preview was open → a fresh preview counting it kept in history (IJ:134).
- **E43 now tells the counterintuitive outcomes truthfully:**
  - a newer Toolkit reading doesn't replace an instrument scan;
  - a Demo reading doesn't count as a scan;
  - same-moment clashes are named;
  - the reason a reading is not checked is given, with "Swatch details —" separating row counts from reading counts.
- **Salvage (DF R5.5d) now defers to R2.3j,** and DJ:124 covers all three pairings. One rule governs "which reading is current", with no special cases.
- **M2 measures format variance, not the owner's own choices.** It bounds what the dogfood run may record, never a data value, which suits a one-user tool with no instrumentation.
- **R6.10's direct seeding and the post-lock build order (PL:146)** keep sibling PRDs' cases independent of the importer. Collection Mode's UJ2.1-t (CMJ:238) pairs with UJ2.1-s, so neither history order can pass for the other.
- **Deferring the mode-switch warning (PL:103) and the near-miss state (PL:101) is right.** Both are reversible or rare, and designing them before OQ 3's corpus exists would be speculative.

#### Missing / over-specified
**Missing:**
- A row and a case for a recognised export that doesn't decode as UTF-8 (PM-R3-M1).
- A remedy on E5's Toolkit variant for when re-exporting doesn't help (PM-R3-m2).
- A correct exclusion list in R6.4 (PM-R3-m1).

**Over-specified:** nothing material. The written-out Toolkit variants for E5, E7, E9, E10 and E42 are longer than a splice rule, but they give copy and test authors one unambiguous string each, so they earn their length.

| Row ID | disposition |
| :--- | :--- |
| Import-R1.5 | ALIGN |
| Import-R3.2 | ALIGN |
| Import-R3.3 | ALIGN |
| Import-R3.5 | ALIGN |
| Import-R3.8 | ALIGN |
| Import-R6.1 | OBJECT (PM-R3-M1) |
| Import-R6.2 | ALIGN |
| Import-R6.3 | ALIGN |
| Import-R6.4 | OBJECT (PM-R3-m1) |
| Import-R6.5 | ALIGN |
| Import-R6.6 | ALIGN |
| Import-R6.7 | ALIGN |
| Import-R6.8 | ALIGN |
| Import-R6.9 | ALIGN |
| Import-R6.10 | ALIGN |
| Import-R6.11 | ALIGN |
| Import-M2 | ALIGN |
| Import-E5 | OBJECT (PM-R3-M1, PM-R3-m2) |
| Import-E7 | ALIGN |
| Import-E9 | ALIGN |
| Import-E10 | ALIGN |
| Import-E13 | ALIGN |
| Import-E14 | ALIGN |
| Import-E42 | ALIGN |
| Import-E43 (Toolkit variant) | ALIGN |
| Import-E46 | ALIGN |
| Import-E47 | ALIGN |
| Import-E48 | ALIGN |
| Import-E49 | ALIGN |
| DF-R1.2 | ALIGN |
| DF-R1.6 | ALIGN |
| DF-R2.1 | ALIGN |
| DF-R2.2 | ALIGN |
| DF-R2.3 (with R2.3h, R2.3j) | ALIGN |
| DF-R2.4 | ALIGN |
| DF-R5.5 (with R5.5d) | ALIGN |
| DF-R7.1 | ALIGN |
| DF-R7.2 | ALIGN |
| DF-R7.7 (with R7.7o) | ALIGN |
| Device-R1.21 | ALIGN |
| Device-R1.22 | ALIGN |
| CM-R2.4 (with R2.4j) | ALIGN |
| CM-R2.7 | ALIGN |
| CM-R4.2 (with R4.2d) | ALIGN |
| CM-R5.2 (with R5.2b/e) | ALIGN |
| CM-R8.9 | ALIGN |
| CM-M2 | ALIGN |
| CM-E12 | ALIGN |
| Export-R1.1 (with R1.1h/i/p/s/t) | ALIGN |
| Export-R1.2 | ALIGN |
| Export-R4.2 | ALIGN |
| Export-E1 | ALIGN |
| Capture-R1.10 | ALIGN |
| Capture-M8 | ALIGN |

### staff-software-engineer

#### Verdict
Proceed after fixing one new Major. There is no Blocker. Round 2's Major, R2-M1, is resolved, and two new cases tell its two outcomes apart. Of my twelve round-2 Minors, ten are resolved and two are only partly resolved (R2-m2's decoding half and R2-m4's choice of B). The new Major comes from a fix I proposed in round 2: UJ 3's fixture values are now tied to a reference that Data Foundation has not named yet, and nothing says what to use in the meantime.

#### What I reviewed
- **Artifact:** the requirements-mode delta for round 3 of the Nix Toolkit import amendment. Subject is 7332d69; round 2 reviewed f7cbde5; the base is 6374538. I read `git diff f7cbde5 7332d69 -- docs AGENTS.md`, then every changed row in full.
- **Paths.** All paths are under `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/`. Abbreviations:
  - IMP = `docs/product/import/prd-inventory-import.md`; IMP-J, IMP-C and IMP-F are its `-journeys`, `-copy` and `-fences` files.
  - DF = `docs/product/data-foundation/prd-data-foundation.md`; DF-J is its journeys file.
  - CM = `docs/product/collection-mode/prd-collection-mode.md`; CM-C is its copy file.
  - EXP = `docs/product/export/prd-data-export.md`; EXP-J is its journeys file.
  - FIX2 = `docs/product/import/prd-inventory-import-round-2-fixes.md`.
  - ADR = `docs/decisions/README.md`.
  - PL = `docs/product/post-lock.md`.
- **Read in full:** IMP §1–§6, M2 and OQ 3; the IMP-C error table; IMP-J UJ 3 (lines 102–152); IMP-F F69–F91 with their round-2 Clarified lines; FIX2; DF R2.3, R5.5, R7.5, R7.7o, the Data Export line, M2 and OQ 6; DF-J DJ6; DF fences F64–F69.
- **Also read:**
  - The changed lines in CM R4.2d, CM-C:295 and :307, UJ2.1-s and -t, and CM's Build-dependencies row.
  - EXP R1.1t, R4.2, the Data Foundation line, E1 and EXP-J:3.
  - Device R1.21 and UJ3-c; Capture R1.5, R1.10, R11.6, M8 and OQ 21.
  - The ADR-0003 row, PL, AGENTS.md §5, and §4 of the spike brief.
  - My own round-2 section in the review log.
- **Could not verify:**
  - Rule 14's word counts (DF 8,900, CM 12,550, Export's 3,992 of 4,000).
  - How the Toolkit writes an embedded quote, delimiter or newline (OQ 3).
  - Whether the hardware spike's reference will be a calculation method or a set of curves with known values; no spike results file exists yet.
  - I ran no count-only check for real-export values this round. The research report was not a source, and no value from it is quoted here.

#### Findings

##### Round-2 findings (and round-1 findings left partial): status
- **R2-M1 (commit recheck against R6.4): RESOLVED.**
  - R3.8k now rechecks the mode fit and each outcome (IMP:126), and E13 has a collection variant (IMP-C:23). F76's round-2 line records it (IMP-F:216).
  - :132 (a target holding a reading → E46) and :133 (a target holding only pending items → E13, then a fresh preview naming M1 → M2) are decided by the one variable that matters. :134 covers the race.
  - What is left is SSE3-m2.
- **R2-m1 (E48 overclaims): RESOLVED** (IMP-C:34).
- **R2-m2 (E5 on a Toolkit export): PARTIAL.**
  - The quoting half landed: IMP:166, IMP-C:15 and IMP-J:147.
  - The decoding half ("a file that fails UTF-8 decoding is never recognised") is only in FIX2:24, not in any PRD row. See SSE3-m1.
- **R2-m3 (which exclusions run before R6.4 and R6.11): RESOLVED** (IMP:169, IMP:176, IMP-J:126). E11's inclusion is a new Minor, SSE3-m3.
- **R2-m4 (the DF R5.5d tie-break): PARTIAL.** DF:187 now defers to R2.3j, and DF-J:124 covers three pairings. Which of the two readings plays R2.3j's B is still unstated (SSE3-m5).
- **R2-m5 (how an unknown model is shown): RESOLVED** (CM:349, CM-C:295, CM-C:307, IMP-J:150; Device R1.21 "if any").
- **R2-m6 (time and number grammar): RESOLVED** (IMP:172: RFC 3339, truncated to the millisecond, the exponent's optional sign).
- **R2-m7 (fixed offset against a provisional tolerance): RESOLVED for the offsets** (IMP-J:121–122 are multiples of DERIVATION_TOLERANCE). The other half of my proposed fix, tying the fixture to DF R7.5's reference, created SSE3-M1.
- **R2-m8 (E14's promise): RESOLVED** (IMP-C:24 and IMP-C:29 now say "with an instrument").
- **R2-m9 (the fixture range, R7.7a–n): RESOLVED.** EXP:145, EXP:180, EXP-J:3 and DF:281 all read R7.7a–o, and no R7.7a–n remains outside the review log.
- **R2-m10 (where E48 sits in the flow): RESOLVED.** IMP-J:110, :111, :113 and :146 create the target before E48, and IMP:166 names the Toolkit export only in E48 and E43.
- **R2-m11 (the file's illuminant and observer): RESOLVED.** IMP:170 stores the text, or nothing where the cell is blank. IMP:171 treats a pair the app does not offer as not checked. IMP-J:123 declares the offered set. The mapping from the file's text to an offered pair is a residual Nit, below.
- **R2-m12 (quarantine and the same-reading check): RESOLVED.** IMP:182 and IMP-J:151; F82's round-2 line (IMP-F:257); the ADR-0003 input exempts quarantined readings (ADR:22). DF R2.3j cites R6.8f (DF:137).
- **Nit, R6.3 pre-fill: PARTIAL.** IMP:168 says "first record's". It should be the first record left after R6.4's exclusions, since R6.11 checks only those (IMP:176).
- **Nit, E47 naming every failing column: RESOLVED in the row** (IMP:172). The copy is still singular (Nit below).
- **Nit, TK-2's pending item's name: RESOLVED** (IMP-J:136 declares both names).
- **Nit, `e+0` in the round-1 fix file: RESOLVED** (round-1 fixes:26).
- **Round-1 M8, partial (fixture range): RESOLVED** (EXP:145, DF:281).
- **Round-1 m13, partial (`undefined` in other columns): RESOLVED.** It is added to OQ 3's closer (IMP:205), and FIX2:49 records the interim rule (stored as given).

##### New in 7332d69

**[MAJOR] SSE3-M1 — Import R6.10 and UJ 3 :104: Fixture T's own colour values are tied to a reference that does not exist yet, with no interim rule.**
- **Where:**
  - IMP:175: "the file's own values checked in from the independent reference [DF R7.5] checks the build against".
  - IMP-J:104: the same, "worked out from each record's reflectances".
  - IMP-F:176: F71's round-2 Clarified line.
  - It is measured against DF:243 (R7.5): "a published source named by the hardware spike … until that reference lands, the other assertions still run".
  - DF:328 (OQ 6): "no reference source named yet".
  - The spike brief §4 scopes that source as "payloads or reflectance curves with independently known Lab/XYZ values".
  - `docs/briefs/` holds no hardware-spike results file.
- **Defect 1: no interim rule.**
  - Until OQ 6 closes, R6.10 allows no source for Fixture T's L, a, b, LCh, XYZ, sRGB and HEX cells.
  - Every case whose E43 must show no "differing" line needs those cells (:117, :136–:144), as do R6.6's own cases (:118, :121–:123).
  - IMP:32 gives DERIVATION_TOLERANCE an interim rule (provisional, gates release) but says nothing about the reference.
- **Defect 2: the reference may be the wrong kind of thing.**
  - If R7.5's source turns out to be a set of curves with known values, it only yields values for its own curves.
  - Fixture T's spectra are invented under constraints no published set will meet: a negative band, a band above 1, and stated gamut margins.
  - In that case the tie can never be satisfied.
- **Scenario (a wrong build passes):**
  - An unattended builder that reaches §6 before the spike reports has two choices: park the whole UJ 3 suite, or fill the cells from the app's own derivation, since no other derivation is sanctioned.
  - The second build passes :118 and :121–:122 by construction, so a derivation bug ships green.
  - That is the circular oracle R6.10 was written to forbid, now made the only available source.
- **Fix:**
  - Rewrite R6.10's clause as: "the file's own values come from an independent implementation of the CIE calculation, named in the engineering plan and never the app's code; once DF OQ 6 names R7.5's source, and where that source is a method, from that source; R6.6's cases stay provisional under DF OQ 6, as DERIVATION_TOLERANCE does."
  - Mirror it at IMP-J:104 and in F71's line.
  - Keep the tolerance multiples at :121–:122.

**[MINOR] SSE3-m1 — R6.1 and E5: whether recognition runs before or after the file is decoded (residual of R2-m2).**
- **Where:** IMP:166 sends only quoting failures to E5's Toolkit variant, and R1.5a (IMP:57) stops decoding errors at E5. The rule "never recognised if UTF-8 decoding fails" exists only in FIX2:24.
- **Scenario:** a Toolkit export re-saved from a spreadsheet as Windows-1252, with an accented colour name.
  - A build that decodes first shows E5's text variant. The user chooses Windows-1252, the ASCII header now matches R6.1, and R6.1 says to "read as UTF-8". That is a contradiction, so the builder guesses.
  - A build that recognises first fixes UTF-8 and then fails to decode. R6.1 does not say which E5 variant applies.
- **Fix:** add to R6.1: "Recognition runs only after the whole source decodes as UTF-8; a source that does not is never a Toolkit export and takes E5's ordinary variants." Add a UJ 3 case.

**[MINOR] SSE3-m2 — R3.8k: "matched outcomes changed" is undefined at its edges.**
- **Where:** IMP:126. E13's collection variant (IMP-C:23) says "a swatch this file matches changed", which is broader than the row.
- **Two readings:**
  - (a) compare the R6.8 letter (a–g) of each item that matched at preview;
  - (b) recompute E43's Toolkit counts and lists.
- **Where they diverge:**
  - A pending item is set aside in a session between preview and commit. It is R6.8b both times, but E43's set-aside list changes. N20 promises the user sees each set-aside decision being resolved before committing, and reading (a) breaks that.
  - An imported current reading is replaced by an older one: R6.8d's "later" becomes "earlier".
  - A record that matched nothing at preview gains a match when a code changes in Collection Mode. "Each matched item" never rechecks it.
- **Consequence:** a build using reading (a) passes :132–:134 and commits a set-aside resolution E43 never listed.
- **Also:** which of "no longer fits → E46" and "mode changed → E13" wins is given only by list order. :132 pins E46.
- **Fix:** "recompute every eligible record's R6.8 outcome; if the mode, any outcome, or any count or list E43's Toolkit lines show differs from the preview → E13's collection variant; a target that no longer fits → E46, ahead of E13."

**[MINOR] SSE3-m3 — R6.4 lists E11, which needs a target, before checks the cases run on reading the file.**
- **Where:**
  - IMP:169 lists "(E7, E9, E10, E11)".
  - E11 (R3.4, IMP:81) needs the target.
  - IMP-J:126–128 raise E46's mixed-modes variant and E49 at "Read the file", before any target is chosen.
  - R6.3 (IMP:168) needs "the file's" mode to fill the creation form.
- **Scenario:** a literal build waits to run the mixed-mode check until after target selection, so that E11 can run first. For a new target, the creation form's mode is then undefined for a mixed file, and :127 fails. E11 is a defensive state for a damaged store, so the harm is the ambiguity, not lost data.
- **Fix:** list "E7, E9, E10 and R6.7's" only, and say E11 applies at target selection, after R6.4's and R6.11's file-level checks.

**[MINOR] SSE3-m4 — the imported snapshot's serial and firmware: "empty" in three places, NULL everywhere else.**
- **Where:**
  - NULL is normative at DF:137 (R2.3j) and IMP:170 ("absent"), and is what DF-J:113 and Device UJ3-c expect.
  - "Empty" remains at IMP-J:118 ("serial and firmware empty"), the ADR-0003 input (ADR:22, "stored empty, never as a placeholder") and PL:172.
- **Scenario:** a test author writes :118 as `= ''`. A correct NULL build fails it, and a `''` build passes it while failing DJ6 and UJ3-c. ADR-0003's author reads "stored empty" as the schema input.
- **Fix:** "absent (NULL)" at all three places.

**[MINOR] SSE3-m5 — DF R5.5d: which of two current readings plays R2.3j's B in a salvage.**
- **Where:** DF:187. DF-J:124's three pairings always record the imported reading later.
- **Scenario:** the imported reading was recorded first and a simulated one later: a Demo re-scan over an imported reading, followed by file damage.
  - Taking the imported reading as B makes it current.
  - Taking the later-recorded reading as B means R2.3j does not apply. The simulated reading stays current, and the imported one is marked "correction-unconfirmed" with a correction question, which is what normal flow produced.
  - The row also says "kept behind" for the losing reading. Where B wins, A is B's predecessor (R2.3h), not a reading kept behind.
- **Fix:** add "taking the later-recorded as B", and add the imported-then-simulated pairing to DJ6. The owner may prefer the other choice.

**[MINOR] SSE3-m6 — the build order does not fit cases that need a later phase.**
- **Where:**
  - PL:146 and IMP:32 build DF R2.3j before Import §6, but every DJ6 action is "Import fixture T and commit" (DF-J:113–124).
  - IMP-J:117 asserts Collection Mode R2.4j's imported mark, and IMP-J:150 asserts R4.2d's detail line.
  - PL:146 lands §6 alongside Collection Mode's imported rows, not after them.
  - IMP-J:152 already shows the right pattern: "runs once R5.5 lands".
- **Fix:** say DJ6 runs in §6's phase, and gate :117's mark clause and :150's detail clause on "once Collection Mode's imported rows land". Hand-off to the plan lens.

**Nits**
- **R6.3 (IMP:168):** "first record's" should be "first record left after R6.4's exclusions", to match R6.11.
- **E47 copy (IMP-C:33):** "the column that couldn't be read" is singular; R6.7 (IMP:172) says "every failing column".
- **IMP-J:122:** a quarter of the tolerance leaves three quarters of headroom. That is tighter than DF M2's "≤ DERIVATION_TOLERANCE" (DF:311), so a build right at M2's limit can list TK-1 as differing. Use a zero offset or state the headroom.
- **M2 (IMP:195):** lists E5, a whole-file refusal, as a record exclusion.
- **R6.6 (IMP:171):** how the file's illuminant and observer text maps to an offered pair is pinned only by the fixture's two written forms. It needs a rule once Capture OQ 21 widens the set.
- **R3.8k:** say the recheck runs under the commit's DF R1.11 hold, so a Flag cannot land between the recheck and the write.

#### Clarifying questions for the author
1. Until DF OQ 6 names R7.5's source, what produces Fixture T's file L, a, b, LCh, XYZ, sRGB and HEX? A named independent implementation, or do those cases wait?
2. If R7.5's source turns out to be a set of curves with known values rather than a method, may UJ 3's values come from a separate independent calculation?
3. Does recognition run only after the whole source decodes as UTF-8, so that a non-UTF-8 re-save is never a Toolkit export?
4. Does the commit recheck compare every count and list in E43's Toolkit lines (including the set-aside and same-dated lists, and a record newly matched), or only the R6.8 letter?
5. When both apply at commit, does E46 take precedence over E13's collection variant?
6. Should E11 leave R6.4's pre-check list, since it needs a target?
7. In a salvage, which of two current readings plays R2.3j's B?
8. Is "empty" at IMP-J:118, ADR:22 and PL:172 meant as NULL?
9. Does DJ6 run in Import §6's build phase, and do :117's mark and :150's detail line wait for Collection Mode's imported rows?
10. Does R6.3's pre-fill take the first record left after R6.4's exclusions?

#### Claimed properties
- **"The commit recheck contradicts no row"** (FIX2:7): **holds** (IMP:126 against IMP-J:132–134). The definition gap is SSE3-m2.
- **"UJ 3's reference is the one DF R7.5 checks the build against"** (FIX2:15): **stated, but not buildable** until DF OQ 6 closes, and perhaps never if that source is a set of curves (SSE3-M1).
- **"Unknowns stored absent (NULL)"** (FIX2:30): **holds** at DF:137, IMP:170, DF-J:113 and Device UJ3-c. **Does not hold** at IMP-J:118, ADR:22 and PL:172.
- **"A file that fails UTF-8 decoding is never recognised"** (FIX2:24): **not in any PRD row** (SSE3-m1).
- **"Salvage keeps a scan current"** (FIX2:8): **holds** (DF:187, DF-J:124). The choice of B is unstated for other pairings (SSE3-m5).
- **"Repeating a committed export changes nothing"** (R3.3, R6.9): **holds, quarantine included** (IMP:182; IMP-J:137, :138, :140, :144, :151, :152).
- **"Exclusions run before R6.4 and R6.11"**: **holds** for E7, E9, E10 and E47 (IMP-J:126, :148). E11's wording is SSE3-m3.
- **"Export stays at 3,992 of 4,000 words" and "no aligned row reopens for compaction"**: **unverified**, because I cannot reproduce rule 14's counting method.

#### Genuinely sound
- **The fix for R2-M1 has the right shape.** Nothing is written, the user gets a fresh preview, and the cases differ in the one variable that decides the outcome (:132 against :133). :134 covers the outcome race.
- **R6.7 now pins RFC 3339 and truncation to the millisecond,** so R6.8f's same-instant test is deterministic. :144 proves the check ignores how a number or time is written.
- **R6.8f's quarantine exemption recovers the item without breaking repeatability:** the reading it recovers is the one R6.8f then matches (:151). The restore case (:152) is correctly gated on R5.5.
- **R6.11 runs after R6.4,** so a file failing both gets one refusal, not two.
- **E5's Toolkit variant keeps R1.5c's locked no-salvage rule.** Inventing a Toolkit-specific salvage before OQ 3's corpus exists would be speculative.
- **R6.10's "declare an imported reading directly" rule** removes Collection Mode's, Device's and Export's need to run the importer. It matches the Collection Mode harness.
- **NULL for unknowns in DF and Device** is right for a reader using plain SQL.
- **§6 still needs no operating envelope of its own.** Asking for one would be over-specifying.
- **M2 and OQ 3 now limit dogfood records** to format facts and tallies by state. That is the right size for a one-user tool.

#### Deferred
- **peer-product-manager-reviewer / peer-product-marketing-manager-reviewer:**
  - E5's Toolkit remedy, "Export the file again from the Nix Toolkit" (IMP-C:15), cannot fix an unescaped quote in a name, because the Toolkit will write the same file again. The remedy should be to rename the colour, marked provisional under OQ 3.
  - E43's not-checked line says "missing", but R6.6 also covers values that are present and unreadable.
- **peer-plan-reviewer:** SSE3-m6's build order.
- **peer-test-reviewer:** the headroom at :122. By my reading, UJ2.1-s and UJ2.1-t together do tell the two history orders apart.
- **peer-database-reviewer:** the NULL type in the ADR-0003 input (SSE3-m4), and exact read-back of spectra stored as REAL.
- **peer-architecture-reviewer:** what "predecessor" means for readings salvaged under R5.5d (the second half of SSE3-m5).
- **peer-privacy-reviewer:** I ran no count-only check for real-export values this round, because the report was not a source.

| Row ID | disposition |
| :--- | :--- |
| Import-R1.5 | ALIGN |
| Import-R3.2 | ALIGN |
| Import-R3.3 | ALIGN |
| Import-R3.5 | ALIGN |
| Import-R3.8 | OBJECT (SSE3-m2) |
| Import-R6.1 | OBJECT (SSE3-m1) |
| Import-R6.2 | ALIGN |
| Import-R6.3 | ALIGN |
| Import-R6.4 | OBJECT (SSE3-m3) |
| Import-R6.5 | OBJECT (SSE3-m4) |
| Import-R6.6 | ALIGN |
| Import-R6.7 | ALIGN |
| Import-R6.8 | ALIGN |
| Import-R6.9 | ALIGN |
| Import-R6.10 | OBJECT (SSE3-M1, SSE3-m6) |
| Import-R6.11 | ALIGN |
| Import-M2 | ALIGN |
| Import-E5 | OBJECT (SSE3-m1) |
| Import-E7 | ALIGN |
| Import-E9 | ALIGN |
| Import-E10 | ALIGN |
| Import-E13 | ALIGN |
| Import-E14 | ALIGN |
| Import-E42 | ALIGN |
| Import-E43 (Toolkit variant) | ALIGN |
| Import-E46 | ALIGN |
| Import-E47 | ALIGN |
| Import-E48 | ALIGN |
| Import-E49 | ALIGN |
| DF-R1.2 | ALIGN |
| DF-R1.6 | ALIGN |
| DF-R2.1 | ALIGN |
| DF-R2.2 | ALIGN |
| DF-R2.3 (with R2.3h, R2.3j) | ALIGN |
| DF-R2.4 | ALIGN |
| DF-R5.5 (with R5.5d) | OBJECT (SSE3-m5) |
| DF-R7.1 | ALIGN |
| DF-R7.2 | ALIGN |
| DF-R7.7 (with R7.7o) | ALIGN |
| Device-R1.21 | ALIGN |
| Device-R1.22 | ALIGN |
| CM-R2.4 (with R2.4j) | ALIGN |
| CM-R2.7 | ALIGN |
| CM-R4.2 (with R4.2d) | ALIGN |
| CM-R5.2 (with R5.2b/e) | ALIGN |
| CM-R8.9 | ALIGN |
| CM-M2 | ALIGN |
| CM-E12 | ALIGN |
| Export-R1.1 (with R1.1h/i/p/s/t) | ALIGN |
| Export-R1.2 | ALIGN |
| Export-R4.2 | ALIGN |
| Export-E1 | ALIGN |
| Capture-R1.10 | ALIGN |
| Capture-M8 | ALIGN |

### test

#### Verdict
Trustworthy once three new Majors are fixed. There is no Blocker, and every round-2 finding is resolved except TR2-m1, which is partial. The round-2 fixes introduced three Majors:
- the only case for "matched outcome changed" asks for a live scan that CI cannot produce;
- Fixture T's file values no longer have a usable reference source;
- Device's only check that the importer creates no saved-device record becomes vacuous when seeded.

#### Coverage map (brief)
Every path is under `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/`:
- **IMP / IJ / IC / IF** = `product/import/prd-inventory-import{,-journeys,-copy,-fences}.md`
- **FIX1 / FIX2** = `product/import/prd-inventory-import-round-{1,2}-fixes.md`
- **DF / DJ** = `product/data-foundation/prd-data-foundation{,-journeys}.md`
- **DM / DMJ** = `product/device-management/prd-device-management{,-journeys}.md`
- **CM / CMJ / CMC** = `product/collection-mode/prd-collection-mode{,-journeys,-copy}.md`
- **EX / EXJ** = `product/export/prd-data-export{,-journeys}.md`
- **CAP** = `product/capture-mode/prd-capture-mode.md`
- **PL** = `product/post-lock.md`

The repo has no code, so every mutation below was checked on paper against the PRD oracles; no suite exists to run.

**Branches covered, each killing a named wrong build:**
- **R6.1:** `;` IJ:110, `,` :111, tab plus an R2.3-variant header :146, a missing column :112, a superset :113.
- **R6.8a:** :117, :136.
- **R6.8b:** :136 pending, :143 set aside, :151 quarantined imported.
- **R6.8c:** :136, with the Toolkit reading newer than the scan.
- **R6.8d:** later and earlier :139, same Date Saved :141.
- **R6.8e:** :145.
- **R6.8f:** :137, :138, :140, :142, :144, :152.
- **R6.8g:** :143.
- **R6.4:** :126–:135. **R6.7:** :124–:126, :149. **R6.11:** :128.
- **Elsewhere:** DJ:113–124, DMJ:80, CMJ:236–238, EXJ:13–14.

**Load-bearing gaps:**
- R3.8k's outcome recheck (IJ:134 cannot run as written).
- The oracle for Fixture T's file values (IJ:104, IMP:175).
- An importer-side check that no saved-device record is created (DMJ:80 only).

#### Findings

**Round-2 findings: status**
- **TR2-M1 — RESOLVED.** IMP:126 (R3.8k), IC:23 (E13 collection variant). IJ:132 now has a target holding an M2 reading, giving E46. IJ:133 has a pending-only target, giving E13 and then a fresh E43 naming M1→M2.
- **TR2-M2 — RESOLVED.** IMP:171 ties "not checked" to "a pair the app offers". IJ:123 declares D50/2° alone through CAP:352 (Capture R11.6), so it stays green however OQ 21 closes.
- **TR2-M3 — RESOLVED.** EX:145, EX:180, EXJ:3 and DF:281 all read a–o. A grep finds no "R7.7a–n" left in `docs/product`.
- **TR2-M4 — RESOLVED.** CMJ:237 reads E17 in its recorded order, which CM:386 defines as E17's body. CMJ:238 (UJ2.1-t) lists I2 then L1 in both orders. That fails a newest-measured-first default and a measured view sorted by record time, both of which passed CMJ:237 alone.
- **TR2-m1 — PARTIAL.** IJ:122 is now a quarter of DERIVATION_TOLERANCE (T), per IJ:104. But DF M2 (DF:311) lets the build sit up to T from the reference, so a conforming build between 0.75 T and T still lists TK-1. Folded into TR3-M2.
- **TR2-m2 — RESOLVED.** IJ:106 sets margins of at least 0.03 in linear RGB on TK-1 and TK-2.
- **TR2-m3 — RESOLVED.** IJ:123 asserts that TK-2 records D65 and 10.
- **TR2-m4 — RESOLVED.** TK-1's R400 is `-1.20000000e-3` (IJ:106). IJ:118 and IJ:150 assert −0.0012 stored and the model absent.
- **TR2-m5 — RESOLVED.** IMP:172 and IJ:149.
- **TR2-m6 — RESOLVED.** IJ:135.
- **TR2-m7 — RESOLVED.** IMP:182, IMP:184 and IJ:151. Only the imported kind has a case; see TR3-m3.
- **TR2-m8 — RESOLVED.** IJ:136 declares `Peach`, `Deep Coral` and no imported columns.
- **TR2-m9 — RESOLVED.** IMP:169 and IMP:176. The E11 in that list is new, TR3-m1.
- **TR2-m10 — RESOLVED.** IMP:166, IC:15 and IJ:147. The decode path is TR3-m2.
- **TR2-m11 — RESOLVED.** IC:17, 19, 20 and 28 write out the Toolkit variants. IJ:148 renders E10's, and IJ:36's each-variant row reaches E7's, E9's and E42's.
- **TR2-m12 — RESOLVED.** DF:187 and DJ:124. The reversed pairings are TR3-m4.
- **TR2-m14 — RESOLVED.** IJ:146.
- **TR2-n1 — RESOLVED** at DMJ:80 ("absent (NULL)"). A residual remains as TR3-n1.
- **TR2-n2 — RESOLVED.** IJ:126.
- **TR2-n3 — RESOLVED.** FIX1:26.
- **TR2-n4 — RESOLVED.** FIX1:58.
- **Round-1 items I left PARTIAL:**
  - #4 via TR2-M4: RESOLVED.
  - #9 via TR2-M3: RESOLVED.
  - #16 via TR2-M1: RESOLVED.
  - #20 via TR2-m10: RESOLVED.
  - #21 via TR2-m7: RESOLVED.
  - #24 via TR2-m3: RESOLVED.

**New in 7332d69**

[MAJOR] TR3-M1, IJ:134 (R3.8k's "matched outcomes changed" arm, E13's collection variant) — no CI injection path for the action, so a correct build fails the case.
- **What the case does:** the action is "scan TK-2 live in a session and end it"; the case then expects TK-2 "kept in history, not used as the colour" (R6.8c).
- **Why CI can't do that:**
  - DM:251 (Device R6.30): the test-only instrument's "generated readings/snapshots remain simulated".
  - DF:245 (DF R7.7): "live-kind snapshots are hand-authored".
  - CAP:352 (Capture R11.6) and DF:240 (DF R7.2) declare starting or boundary states only, not a write landing mid-preview.
- **What happens in CI:** a session scan through the only available instrument lands a simulated reading, so a correct build's fresh E43 counts TK-2 through R6.8g as "now a swatch's colour". That fails the expected result.
- **Why it matters:** this is the only case for the arm that stops a stale R6.8b plan from displacing a real scan (N6).
- **Fix:**
  - Either: run the scan on the Demo Device and expect the R6.8g outcome in the fresh preview (TK-2 used as the colour; after Import, reason re-measurement with the simulated reading in history). This still fails both a build with no recheck and a build that silently re-plans without E13.
  - Or: name a seam that lands a declared live-kind reading on TK-2 between preview and Import.

[MAJOR] TR3-M2, IJ:104, IMP:175 (R6.10) and IF:176, against DF:243 (R7.5), DF:311 (M2) and DF:328 (OQ 6) — Fixture T's file values have no usable source, and a correct build can fail IJ:117, :118 and :122.
- **What changed:** round 2 allowed "a colour library at a stated version, or R7.5's published source once named". The fix narrowed this to "the independent reference R7.5 checks the build against".
- **Why that fails:**
  - That reference is "independently known values from a published source named by the hardware spike", and none is named yet (DF:328).
  - A published table of known values cannot supply Lab, XYZ or sRGB for invented spectra. Fixture T's spectra must be invented: TK-1 has a negative band, TK-3 a 1.047 band, and both carry gamut margins.
  - DF M2 bounds the build only "over the fixed reference set". Fixture T's spectra are not in that set, so an M2-conforming build can exceed T on TK-1 or TK-2. It then fails IJ:117 ("no … differing … sentence"), :118 ("Lab within DERIVATION_TOLERANCE") and :122.
- **Effect:** the builder has to guess a source for the file values, which breaks R6.10 as written.
- **Fix:**
  - Restore an interim: "an open colour library at a stated version until R7.5's source is named, then that source's method".
  - Then, either add Fixture T's three spectra to R7.5's reference set, so M2 bounds the build on them, or apply IJ:121/:122's offsets to the Lab the build itself derives. Those two cases test R6.6's comparison; :118 and R7.5 keep derivation accuracy.

[MAJOR] TR3-M3, DMJ:80 (UJ3-c) and DM:121 (Device R1.22), against IMP:175 (R6.10's new seeding permission) and PL:146 (build order) — the only case where the importer must create or occupy no saved-device record becomes vacuous.
- **Why it's vacuous:**
  - PL:146 builds Device R1.21 before Import §6, and R6.10 lets "another PRD's case … declare an imported reading directly". So UJ3-c's "imported from … fixture T" item has to be seeded at first, and nothing later makes it switch to the importer.
  - When seeded, "no device record was created for it and none is occupied" is trivially true.
  - No Import or DJ6 case reads the saved-device records (grep: none in `import/*.md` or DJ).
- **Mutation:** an importer that upserts a saved "Nix Spectro 2" record, or links its snapshots to an existing saved Nix Spectro 2 by model, passes IJ:110–152, DJ:113–124 and a seeded UJ3-c.
- **Fix:**
  - Add to IJ:118: "no saved-device record exists".
  - Add a variant whose Given holds a saved live-kind Nix Spectro 2 record, asserting that it is unchanged and occupied by no imported snapshot.
  - Or state that UJ3-c runs the importer once Import §6 lands.

**New Minors**
- **TR3-m1 — IMP:169, IMP:176:** R6.4's pre-check list now includes E11, which needs a target. But IJ:126–128 run the mixed-mode and E49 checks at "Read the file", before any target exists. Drop E11 from the list, or say the file-level checks run again after target selection.
- **TR3-m2 — IMP:166 vs IMP:57:** FIX2:24's "a file that fails UTF-8 decoding is never recognised" is in no row.
  - Scenario: a Toolkit export re-saved as Windows-1252 fails the default read and reaches E5. The user picks Windows-1252, the header matches R6.1, and R6.1 says "read as UTF-8", so it loops or lands in an undefined state.
  - Fix: say that recognition runs only on the default UTF-8 read, and add a case.
- **TR3-m3 — IMP:173 vs IMP:184:** R6.8 is "told by … snapshot kind", yet R6.8b claims a quarantined current reading. IJ:151 covers only the imported kind. A build keyed on kind sends a quarantined live current to R6.8c and passes every case. Add a quarantined-live TK-1 case.
- **TR3-m4 — DF:187, DJ:124:** pairing 1 is the one order where "R2.3j by kind" and "replay in record order" agree.
  - Untested: imported recorded first then a simulated reading (which one is current?), and imported first then a live reading (correction question or none?). R5.5d reads two ways for both.
  - Fix: add both reversed pairings and word R5.5d as "whichever was recorded first".
- **TR3-m5 — IMP:126:** "each matched item's R6.8 outcome" misses a record that was unmatched at preview but matches at commit. Example: an item coded TK-3 added mid-preview through Capture R9.3's Add a swatch; R6.8a would then create a second equal code, against R3.3. Say "each eligible record's outcome".
- **TR3-m6 — IMP:176, IMP:168:** no case has collection names that differ only by R2.3. An exact-compare E49 build passes IJ:128 and every Fixture T case. Add TK-2 with `fixture  set`, expecting no E49 and the target pre-filled `Fixture Set` (TK-1's text).
- **TR3-m7 — IMP:172:** the new rules have no case:
  - truncating versus rounding digits past the millisecond, which feeds R6.8f;
  - ISO 8601's basic form being unreadable;
  - E47 naming every failing column of one record.

  Suggested case: re-import TK-1 dated `2026-03-14T09:15:00.0009Z`, expecting 3 already here. A rounding build makes it later and current.
- **TR3-m8 — IMP:170:** a blank Illuminant or Observer is "absent where blank" but has no case, so a build that stores `''` passes. Add a blank-pair variant to IJ:123 or :150.
- **TR3-m9 — IMP:32, PL:146, CMC:295, CMC:307:** IJ:117's imported-mark clause and IJ:150's Reading-line clause assert Collection Mode rendering that builds after Import §6. Collection Mode also has no case of its own for the new model-unknown text in R4.2d/R5.2b.
  - Fix: mark those clauses "once Collection Mode's imported rows land", as IJ:152 does.
  - Fix: add a model-absent item to UJ2.1-r.
- **TR3-m10 — IJ:152, CM:388:** R6.8f's "a restore stands" is P0, but its case waits for Collection Mode R5.5, which is P1. Declare the restored state under DF R7.2 so it runs in P0.
- **TR3-m11 — EX:89, EX:94, EXJ:13–14:** two gaps:
  - No fixture exports a quarantined imported current. A build that writes `sc_imported` false for every quarantined row passes EXJ:13, because R7.7i's quarantined current is not imported.
  - R1.1t's new `superseded` label for a kept-behind imported reading is asserted only as "carry the same cells", and only in the P1 half.

  Assert both.
- **TR3-m12 — pre-existing, missed in round 2; IJ:112 vs IMP:59:** reading Fixture T with the default comma puts a `"` inside unquoted fields. R1.5c doesn't say whether that is literal or invalid quoting (E5). A strict parser shows E5 where the case expects E45. Either state the rule in R1.5c or assert only "not a Toolkit export (no E48)".

**Nits**
- **TR3-n1:** IJ:118 says "serial and firmware empty" and CMJ:236's Given says "unknown". R6.5 and DF:137 say "absent (NULL), never a placeholder".
- **TR3-n2:** IJ:136, :139 and :141 still say "kept as earlier", while IC:29 and IJ:134 say "kept in history, not used as the colour".
- **TR3-n3:** IMP:175 says only "this PRD's cases and DF R7.7o run the importer", but DJ:113–121 act with "Import fixture T and commit". Say that DJ6 is R7.7o's journey.
- **TR3-n4:** IMP:126 gives no precedence when a target both no longer fits and changed mode. IJ:132 pins E46, so say E46 comes first.

#### Biggest risks   (what could ship broken behind a green suite)
- **A phantom saved device.** The importer registers a "Nix Spectro 2" record, or links to the user's saved one by model, with every case green (TR3-M3).
- **The outcome recheck skipped or unverified.** R3.8k's outcome recheck ships either untested or tested against a harness that fakes a different scenario (TR3-M1). The stale-plan bug it guards against lets a Toolkit reading displace a real scan.
- **Guessed Fixture T values.** The file values come from whatever library the builder picks, so R6.6's differing and not-checked lists are right only by coincidence with the eventual R7.5 source (TR3-M2).
- **A quarantined live current routed by snapshot kind** to R6.8c, leaving the item with an unreadable current value after re-import (TR3-m3).

#### Genuinely solid   (incl. where minimal scoping is correct that a coverage-zealot would wrongly flag)
- **The R6.8a–g table is exercised end to end.** Every arm has a case, and R6.8f alone fails six distinct wrong builds:
  - append on every re-import;
  - compare against the current reading only;
  - Flag overridden;
  - string-compared Date Saved;
  - a quarantined reading counted as the same reading;
  - R6.8d run before R6.8f over a restore.
- **IJ:132/:133 now split "no longer fits" from "mode changed but still fits".** The injection path (Capture R1.10's between-session mode change during a preview) is real.
- **IJ:135** fails a build that checks only current readings, on "hold no reading".
- **IJ:136 declares both names**, so "Updated 1" rests on declared data.
- **CMJ:237/:238 are a textbook discriminating pair**, as DJ:115/116 are at the SQL level.
- **IJ:123's declared offered-pair set** makes the not-checked oracle independent of OQ 21.
- **DJ:124's pairings 2 and 3** fail the pre-amendment rule of always keeping the later-recorded reading.
- **R6.10's seeding permission is right for Collection Mode and Export rendering cases.** The subject there is rendering, not the importer. Only Device R1.22's importer-side invariant loses coverage.
- **Correctly minimal:** Capture M8 needs no case. E7, E9 and E42's Toolkit variants are reached by IJ:36's each-variant row with no dedicated case. Import M2 is a dogfood tally with no automated oracle.

#### Missing / over-tested
Missing:
- IJ:134 made runnable (TR3-M1).
- A source for Fixture T's file values, and M2's bound on its spectra (TR3-M2).
- An importer-side check for Device R1.22 (TR3-M3).
- A quarantined-live R6.8b case.
- DJ:124's reversed pairings.
- R2.3-equal collection names.
- Time truncation, the basic form, and multiple failing columns in one E47 record.
- A blank illuminant and observer.
- A Collection Mode case for a model-absent imported reading.
- A P0 restore case.
- Export of a quarantined imported reading, and the `superseded` label.
- Optional, once OQ 21 offers more than one pair: a case where the file's pair is offered but differs from the collection's, so R6.6's "under the file's own illuminant and observer" is told apart from "under the collection's reference".

Over-tested: none. Each of the 43 UJ 3 rows fails a distinct wrong build.

| Row ID | disposition |
| :--- | :--- |
| Import-R1.5 | ALIGN |
| Import-R3.2 | ALIGN |
| Import-R3.3 | ALIGN |
| Import-R3.5 | ALIGN |
| Import-R3.8 | OBJECT (TR3-M1, TR3-m5, TR3-n4) |
| Import-R6.1 | OBJECT (TR3-m2, TR3-m12) |
| Import-R6.2 | ALIGN |
| Import-R6.3 | ALIGN |
| Import-R6.4 | OBJECT (TR3-m1) |
| Import-R6.5 | OBJECT (TR3-M3, TR3-m8) |
| Import-R6.6 | OBJECT (TR3-M2, TR2-m1 residual) |
| Import-R6.7 | OBJECT (TR3-m7) |
| Import-R6.8 | OBJECT (TR3-m3, TR3-m10) |
| Import-R6.9 | ALIGN |
| Import-R6.10 | OBJECT (TR3-M2, TR3-M3) |
| Import-R6.11 | OBJECT (TR3-m1, TR3-m6) |
| Import-M2 | ALIGN |
| Import-E5 | ALIGN |
| Import-E7 | ALIGN |
| Import-E9 | ALIGN |
| Import-E10 | ALIGN |
| Import-E13 | OBJECT (TR3-M1) |
| Import-E14 | ALIGN |
| Import-E42 | ALIGN |
| Import-E43 (Toolkit variant) | ALIGN |
| Import-E46 | ALIGN |
| Import-E47 | ALIGN |
| Import-E48 | ALIGN |
| Import-E49 | ALIGN |
| DF-R1.2 | ALIGN |
| DF-R1.6 | ALIGN |
| DF-R2.1 | ALIGN |
| DF-R2.2 | ALIGN |
| DF-R2.3 (with R2.3h, R2.3j) | ALIGN |
| DF-R2.4 | ALIGN |
| DF-R5.5 (with R5.5d) | OBJECT (TR3-m4) |
| DF-R7.1 | ALIGN |
| DF-R7.2 | ALIGN |
| DF-R7.7 (with R7.7o) | ALIGN |
| Device-R1.21 | ALIGN |
| Device-R1.22 | OBJECT (TR3-M3) |
| CM-R2.4 (with R2.4j) | ALIGN |
| CM-R2.7 | ALIGN |
| CM-R4.2 (with R4.2d) | OBJECT (TR3-m9) |
| CM-R5.2 (with R5.2b/e) | OBJECT (TR3-m9) |
| CM-R8.9 | ALIGN |
| CM-M2 | ALIGN |
| CM-E12 | ALIGN |
| Export-R1.1 (with R1.1h/i/p/s/t) | OBJECT (TR3-m11) |
| Export-R1.2 | ALIGN |
| Export-R4.2 | ALIGN |
| Export-E1 | ALIGN |
| Capture-R1.10 | ALIGN |
| Capture-M8 | ALIGN |

TR3-M2 also binds DF R7.5 and DF M2, which are outside this table. TR3-M1 could be fixed through Capture R11.6 or DF R7.2 declaring a write mid-preview, and both of those rows are also outside the table.

### interface

#### Verdict
Contract sound. No Blocker is open, and every round-2 Major is fixed. Round 2's exclusion-order fix adds one Major: E11 now sits in R6.4's pre-check list, which makes the file-level refusals E46 (mixed modes) and E49 depend on the target, and three UJ 3 cases contradict that. One round-2 Minor is only partly fixed, and there are three new Minors.

#### Surface & consumers (brief)
- **Surface:** the published contract across the six PRDs: row IDs and R3.8 transitions, E-state variants and their placeholders, the inherited-obligation lines, and Export's `sc_` provenance columns.
- **Consumers:** build agents, who build and test by row and state ID; the sibling PRDs, through the obligation lines; the Cataloger, through the copy; scripts that parse `sc_imported`.
- **What changed** (`git diff f7cbde5 7332d69 -- docs AGENTS.md`):
  - Import: R3.8k with E13's new collection variant, R6.1, R6.3, R6.4, R6.6, R6.7, R6.8f, R6.10 and R6.11; written-out Toolkit variants for E5, E7, E9, E10 and E42; E14, E43 and E48 reworded.
  - Data Foundation: R2.3j (unknowns stored as NULL), R5.5d and a DJ6 salvage case.
  - Export: R7.7a–o at four sites, and R1.1t's `superseded`.
  - Collection Mode: R4.2d/R5.2b "model unknown", UJ2.1-t and a Build-dependencies row.
  - Device: R1.21 gains "if any". Capture: M8.
- **No consumer-facing break:** no export format version has shipped yet.

File aliases (all under `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/`):
- IMP = `import/prd-inventory-import.md`; IMPC = `import/prd-inventory-import-copy.md`; IMPJ = `import/prd-inventory-import-journeys.md`; IMPF = `import/prd-inventory-import-fences.md`; FIX2 = `import/prd-inventory-import-round-2-fixes.md`.
- DF = `data-foundation/prd-data-foundation.md`; DFJ = `data-foundation/prd-data-foundation-journeys.md`.
- EXP = `export/prd-data-export.md`; EXPJ = `export/prd-data-export-journeys.md`; EXPC = `export/prd-data-export-copy.md`.
- CM = `collection-mode/prd-collection-mode.md`; CMC = `collection-mode/prd-collection-mode-copy.md`; CMJ = `collection-mode/prd-collection-mode-journeys.md`.
- DEV = `device-management/prd-device-management.md`; DEVJ = `device-management/prd-device-management-journeys.md`; CAP = `capture-mode/prd-capture-mode.md`.

#### Findings

##### Checking my round-2 findings, and the round-1 findings they carried

**Majors**
- **IF2-M1 — RESOLVED.** The fixture range reads R7.7a–o at EXP:145 (R4.2), EXP:180, DF:281 ("including empty R7.7n and imported R7.7o") and the EXPJ:3 header. No stale "a–n" is left anywhere in the tree. R7.7o (DF:268) supplies all three `sc_imported` values.
- **IF2-M2 — RESOLVED.** DF:187 now keeps "the one R2.3j would leave current", the other kept behind with no question.
  - Scenario A (simulated then imported) now keeps the imported reading current.
  - Scenario B (two imported readings) now keeps the later-measured one current.
  - DFJ:124 asserts both, plus the live-then-imported pair. A residual ordering gap remains; see IF3-m3.

**Minors**
- **IF2-m1 — PARTIAL.**
  - The quote half landed: R6.1 (IMP:166), E5's Toolkit variant with only "Pick the file again; Cancel" (IMPC:15), and a case at IMPJ:147.
  - The encoding half did not. FIX2:24 says a file that fails UTF-8 decoding "is never recognised, so it reaches E5's ordinary variants", but no row, fence (IMPF:193) or case says so.
  - E5's ordinary Text variant offers "Choose an encoding". Once Windows-1252 decodes the file, R6.1 recognises the header, since it states no encoding condition, and then says "read as UTF-8".
  - A builder has to guess between three behaviours: recognise the file under Windows-1252; treat it as a plain import (readings dropped, about 57 permanent passthrough columns); or loop back to E5.
  - Fix: start R6.1 with "A source read as UTF-8 whose first record…", saying what a manual encoding then does, or "recognised under the encoding read and shown fixed at it". Add a UJ 3 case.
- **IF2-m2 — RESOLVED.** Each Toolkit remedy is written out in full, and each keeps its row's other clause: E7 at IMPC:17 ("You can bring in the rest"), E9 at :19, E10 at :20, and E42 at :28 ("nothing has been imported", plural "them"). The splice rule is gone (IMPC:39), and IMPJ:148 covers E10.
- **IF2-m3 — RESOLVED.** IMPC:29 reads "Kept in history, not used as the colour", limited to "a swatch scanned here with an instrument". E14's Toolkit variant (IMPC:24) has the same scope. A journey residue remains; see IF3-n1.
- **IF2-m4 — RESOLVED.** IMPC:34: "The file's own Lab is checked against its spectrum; its Lab, LCh, XYZ, sRGB and HEX aren't kept". This matches R6.2 and R6.6.
- **IF2-m5 — RESOLVED.** IMPJ:110, :111 and :146 create a target before E48. R6.1 now names only mapping (E48) and preview (E43) (IMP:166).
- **IF2-m6 — RESOLVED.** IMPC:37: for a Toolkit export, the settings are "shown fixed without controls (R6.1)".
- **IF2-m7 — RESOLVED.**
  - CMJ:237 reads E17 in its "recorded order". This is E17's body and its "Recorded order" action, not the P1 restore variant.
  - CMJ:238 (UJ2.1-t) pairs with it, so that neither order can pass for the other.

**Nits**
- **IF2-n1 — RESOLVED.** IMP:182: "Adds no reading and changes no reading or state … metadata follows R3.5/R3.6".
- **IF2-n2 — RESOLVED.** DEV:120 adds "if any". Import's own line still lacks it; see IF3-n3.
- **IF2-n3 — RESOLVED.** IMPF:348 maps F79 to the Data Export line.
- **IF2-n4 — RESOLVED.** EXP:181 names `sc_imported`, and so does EXPJ:13's quarantined row.
- **IF2-n5 — RESOLVED.** EXPC:16 names serial, firmware, averaging basis and the archived readings.
- **IF2-n6 — RESOLVED.** CAP:420 and IMP:153 now read "a Nix Toolkit export's import".
- **IF2-n7 — RESOLVED.** IMP:172: "fractional seconds optional and truncated to the millisecond".

**Round-1 findings my round-2 review left PARTIAL**
- **IF1-M1 — PARTIAL.** The E43 half was fixed in round 2; the E5 half is still open as IF2-m1 above.
- **IF1-M8 — RESOLVED.** IMPF:348, together with IMP:151 and EXP:173.
- **IF1-M10 — RESOLVED.** EXP:145 and DF:268.
- **IF1-m12 — RESOLVED.** IMP:166, and the cases at IMPJ:110–111.

##### New in round 3

[MAJOR] IF3-M1 — Import R6.4 and R6.11 put a target-dependent exclusion ahead of two file-level refusals

**Location:**
- IMP:169, R6.4: "§1–§3's exclusions (E7, E9, E10, E11) and R6.7's run first; then … E46's mixed-modes variant refuses the file."
- IMP:176, R6.11: "Every record left after R6.4's exclusions … checked after R6.4's mixed-mode check."
- Where it came from: the exclusion-order fix (FIX2:26; F73's round-2 Clarified line at IMPF:193). The finding it answered, SSE's round-2 R2-m3, asked only for "E7, E9, E10 and E47".

**What it contradicts:**
- E11 is target-dependent: it fires when a code "matches multiple existing items" (R3.4, IMP:81), and the target step comes after the read (Surfaces, IMP:40). As written, then, neither E46's mixed-modes check nor E49 can run until a target is chosen.
- IMPJ:126, :127 and :128 reach E46 (mixed modes) and E49 on "Read the file", with no target step.
- E46's mixed-modes actions (IMPC:32) are file-level only ("Pick the file again; Cancel"), even though the outcome now depends on which target was chosen.

**Scenario:** a user picks a file that mixes M1 and M2 records.
- **Build A follows R6.4.**
  - It defers both checks to target selection. The user creates a target first, and R3.7 persists it.
  - Only then does the refusal come, leaving an empty collection behind.
  - IMPJ:126–128 fail at "Read the file".
- **Build B checks at read time**, over E7, E9, E10 and E47 only.
  - It passes every case.
  - It refuses a file that R6.4 would import: one whose only M1 record E11 excludes against an existing target holding two items with that code.
  - No case exercises that.

**Mutation:** implement R6.4 literally. UJ 3's three mixed-modes and multi-collection cases go red, while the cases pass the build that breaks R6.4. E11 itself is rare, but the step split is visible on every mixed-mode or multi-collection file.

**Fix (smaller option):** make the list "E7, E9, E10 and R6.7's run first; a record E11 later excludes still counts" in R6.4, R6.11 and F73's round-2 Clarified line.

**Fix (alternative):** keep E11, and state that both checks run at target selection beside the collection check. Then add "create or choose a target" to IMPJ:126–128 and offer "Choose another collection" on E46's mixed-modes variant.

**New Minors**
- [MINOR] IF3-m1 — IMP:184 (R6.8b) lists "pending, set aside, or its current reading quarantined" as three cases, then says "E43 listing each set-aside item".
  - Capture R8.18 (CAP:297) sets an item whose current reading is quarantined aside, with the cause unreadable. Whether E43's ⟨resolved⟩ list names such an item is therefore a guess.
  - IMPJ:151 asserts only "counts it used as the colour".
  - Fix: write "each set-aside item, an unreadable one included" (or "excluded"), and assert the listing at IMPJ:151.
- [MINOR] IF3-m2 — R6.7's "E47 names every failing column of a record" (IMP:172) has no case.
  - IMPJ:124–126 and :149 each fail one column per record, so a build that stops at the first failing column passes.
  - E47's body (IMPC:33) says "with the column that couldn't be read", in the singular.
  - Fix: give one record in IMPJ:125 two failing columns (for example Date Saved and `R550 nm`), and pluralise the body.
- [MINOR] IF3-m3 — DF:187 R5.5d, "the one R2.3j would leave current": R2.3j (DF:137) is written for an imported reading that lands over an existing one.
  - The ambiguous case: an imported reading recorded before a simulated one, as when a Demo scan is taken over an imported current reading and damage then leaves both current.
    - Applying R2.3j by kind keeps the imported reading.
    - "Otherwise the later-recorded" keeps the simulated one, which matches what normal operation showed.
  - DFJ:124 covers only the simulated-first order. A same-dated imported pair likewise leaves open which reading is R2.3j's "A".
  - Fix: write "where the later-recorded reading is imported, the one R2.3j would leave current…; otherwise the later-recorded". Add the imported-first pairing to DFJ:124.

**Nits**
- [NIT] IF3-n1 — IMPJ:136, :139 and :141 still say "kept as earlier", the wording IF2-m3 retired from IMPC:29. Tests assert identity, not wording, but a test author gets the retired word.
- [NIT] IF3-n2 — IMPJ:118 says "serial and firmware empty". DF R2.3j (DF:137) and DJ6 (DFJ:112) now say "absent (NULL), never a placeholder", so a build that stores `''` passes IMPJ:118. Say "absent (NULL)".
- [NIT] IF3-n3 — Import's Device line at IMP:152 says "naming the model its file gives" without the "if any" that DEV:120 gained. This is IF2-n2's other side.
- [NIT] IF3-n4 — R6.10 (IMP:175) says "only this PRD's cases and Data Foundation R7.7o run the importer". But DJ6's rows run "Import fixture T and commit" (DFJ:112–122), and Device UJ3-c (DEVJ:80) reads "imported from … fixture T". Name DJ6 beside R7.7o, and say UJ3-c declares its reading directly.
- [NIT] IF3-n5 — IMPC:39 cites R6.1 for the Toolkit variants of E7, E9, E10 and E42, but R6.1 (IMP:166) names only E5's; only F73's map (IMPF:342) carries them. Add "E7, E9, E10 and E42 show their Toolkit variants" to R6.1.
- [NIT] IF3-n6 — IMPC:39 fixes a string on Capture §1's creation form: "Measurement condition: ⟨mode⟩, set by this Nix Toolkit export".
  - Neither Capture R1.10 (CAP:147) nor Import's Capture line (IMP:153) names that string.
  - Capture's copy has no ordinary scan-mode label for it to match.
  - Name the string in Import's Capture line.
- [NIT] IF3-n7 — EXP:94 (R1.1t): "the snapshot's model the file's" doesn't say the model cell is empty for a blank Nix Device (IMPJ:150). That follows from R1.1's empty rule, but write "the file's, or empty".
- [NIT] IF3-n8 — IMP:3's status line still reads "E14 and E43 gain Toolkit variants". Round 2 gave E5, E7, E9, E10 and E42 Toolkit variants and E13 a collection variant.

#### Biggest risks   (what existing consumers/scripts/agents break)
- **Mixed-modes and multi-collection files split the builds (IF3-M1).** The row and the cases put E46 (mixed modes) and E49 at different steps. The build that follows the row fails UJ 3, and leaves an empty collection behind for a file it then refuses.
- **A non-UTF-8 Toolkit export has no defined path (IF2-m1, partial).** A manual encoding choice after E5 can drop the file into the lossy plain import, which drops the readings and adds about 57 permanent passthrough columns. The fix file's stated rule appears in no row.
- **Rare-path gaps.** An unreadable set-aside item's E43 listing (IF3-m1), and salvage of an imported reading recorded before a simulated one (IF3-m3). Both are low-frequency, and both surface in text shown to the user.
- **`sc_imported` is now low-risk.** R4.2's range, both sides of the Export and Data Foundation link, R1.1t's `superseded` and EJ1's three values all agree.

#### Genuinely well-designed   (incl. where a deliberate inconsistency is correct that a style-checker would wrongly flag)
- **The `sc_imported` contract is closed end to end.**
  - Export R1.2 says the two provenance columns are never both true.
  - R1.1h/s/i cover the empty value and the quarantined cases.
  - R1.1t labels a reading kept behind `superseded`.
  - R7.7o is cut into the goldens.
  - It needs no version bump, correctly, because no format version has shipped.
- **Reusing E13 for a preview that went stale is right.** E13's collection variant reuses an existing state ID rather than minting a new one. A style checker would flag the mismatch with the state name "File changed on disk", but R4.2 tests variant identity, not the name, and "Review again" (R3.8f) is exactly the right transition.
- **The four written-out Toolkit remedies** (E7, E9, E10, E42) each keep their row's other clause, so there is no splice ambiguity.
- **The UJ2.1-s/t pair discriminates properly:** no single wrong ordering passes both cases.
- **Unknowns are stored as NULL, never a placeholder,** across DF R2.3j, DJ6 and Device UJ3-c. The labels that render them ("model unknown", "serial unknown") live only in Collection Mode's copy, which is the right split.
- **The chip and filter labels still differ on purpose** ("From Nix Toolkit" vs "Imported from Nix Toolkit", CMC:260), keeping the chip clear of the " (imported)" column tag.
- **Export's status line was trimmed on purpose** (EXP:3) to stay within its word budget. The two F35 Clarified lines keep the list of amended rows, so no traceability is lost.

#### Missing / over-engineered
- **Missing:**
  - an E11 carve-out, or a target step, for E46 (mixed modes) and E49 (IF3-M1);
  - a UJ 3 case for a non-UTF-8 Toolkit export (IF2-m1);
  - an E47 case with two failing columns (IF3-m2);
  - the listing assertion for an unreadable set-aside item (IF3-m1);
  - the imported-first salvage pairing (IF3-m3).
- **Not over-engineered:** R3.8k's recheck is lean. One recheck covers the mode fit and the matched-item outcomes, and it routes to two existing states, E46 and E13.

| Row ID | disposition |
| :--- | :--- |
| Import-R1.5 | ALIGN |
| Import-R3.2 | ALIGN |
| Import-R3.3 | ALIGN |
| Import-R3.5 | ALIGN |
| Import-R3.8 | ALIGN |
| Import-R6.1 | OBJECT (IF2-m1) |
| Import-R6.2 | ALIGN |
| Import-R6.3 | ALIGN |
| Import-R6.4 | OBJECT (IF3-M1) |
| Import-R6.5 | ALIGN |
| Import-R6.6 | ALIGN |
| Import-R6.7 | OBJECT (IF3-m2) |
| Import-R6.8 | OBJECT (IF3-m1) |
| Import-R6.9 | ALIGN |
| Import-R6.10 | ALIGN |
| Import-R6.11 | OBJECT (IF3-M1) |
| Import-M2 | ALIGN |
| Import-E5 | OBJECT (IF2-m1) |
| Import-E7 | ALIGN |
| Import-E9 | ALIGN |
| Import-E10 | ALIGN |
| Import-E13 | ALIGN |
| Import-E14 | ALIGN |
| Import-E42 | ALIGN |
| Import-E43 (Toolkit variant) | ALIGN |
| Import-E46 | OBJECT (IF3-M1) |
| Import-E47 | OBJECT (IF3-m2) |
| Import-E48 | ALIGN |
| Import-E49 | OBJECT (IF3-M1) |
| DF-R1.2 | ALIGN |
| DF-R1.6 | ALIGN |
| DF-R2.1 | ALIGN |
| DF-R2.2 | ALIGN |
| DF-R2.3 (with R2.3h, R2.3j) | ALIGN |
| DF-R2.4 | ALIGN |
| DF-R5.5 (with R5.5d) | OBJECT (IF3-m3) |
| DF-R7.1 | ALIGN |
| DF-R7.2 | ALIGN |
| DF-R7.7 (with R7.7o) | ALIGN |
| Device-R1.21 | ALIGN |
| Device-R1.22 | ALIGN |
| CM-R2.4 (with R2.4j) | ALIGN |
| CM-R2.7 | ALIGN |
| CM-R4.2 (with R4.2d) | ALIGN |
| CM-R5.2 (with R5.2b/e) | ALIGN |
| CM-R8.9 | ALIGN |
| CM-M2 | ALIGN |
| CM-E12 | ALIGN |
| Export-R1.1 (with R1.1h/i/p/s/t) | ALIGN |
| Export-R1.2 | ALIGN |
| Export-R4.2 | ALIGN |
| Export-E1 | ALIGN |
| Capture-R1.10 | ALIGN |
| Capture-M8 | ALIGN |

### architecture

#### Verdict
Sound, build it. Round 2's two Majors and all its Minors and Nits are resolved. The imported snapshot kind, the never-supplied payload state and R2.3j's rule for which reading is current now agree across all six PRDs. No row decides ADR-0003's schema or the blob schema, and nothing contradicts ADR-0001. What is left: four low-reach Minors (a salvage ordering gap, "empty" versus "absent (NULL)" drift in the ADR-0003 input, a check-phasing slip, and an encoding gap in Toolkit recognition) and two Nits.

#### Architecture in brief
- **Import §6** runs the file-level exclusions, then the mixed-mode and one-collection checks, then R6.8's outcome for each record. R6.8f (the same-reading check) runs first; after it, the matched item's current snapshot kind decides the outcome, a restore carrying its source's kind. R3.8k now works out the mode fit and every matched outcome again at commit. If either changed, it returns to a fresh preview through E13's collection variant; a target that no longer fits gets E46.
- **Data Foundation** owns the reading. An imported reading has no samples, basis, verdict, spread or payload, and its unknowns are NULL. DF also owns which reading is current (R2.3j), the predecessor (R2.3h) and salvage (R5.5d).
- **Device** owns the three snapshot kinds. Collection Mode's mark, Export's `sc_imported`/`sc_simulated` and R6.8 are all worked out from that one field.
- **Key tradeoff:** the current reading need not be the latest recorded. That is accepted and handed to ADR-0003 as an input. Other PRDs' tests now declare an imported reading directly instead of running the importer. That decouples build order; R7.7o is the one fixture the importer itself produces.

Path legend (all absolute):
- IMP = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import.md
- IJ = …/docs/product/import/prd-inventory-import-journeys.md
- IC = …/docs/product/import/prd-inventory-import-copy.md
- IFX = …/docs/product/import/prd-inventory-import-round-2-fixes.md
- DF = …/docs/product/data-foundation/prd-data-foundation.md
- DFJ = …/docs/product/data-foundation/prd-data-foundation-journeys.md
- EX = …/docs/product/export/prd-data-export.md
- EXJ = …/docs/product/export/prd-data-export-journeys.md
- CM = …/docs/product/collection-mode/prd-collection-mode.md
- CMJ = …/docs/product/collection-mode/prd-collection-mode-journeys.md
- DEVJ = …/docs/product/device-management/prd-device-management-journeys.md
- ADR = …/docs/decisions/README.md
- PL = …/docs/product/post-lock.md

Every `…` stands for `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import`.

#### Findings

**Round-2 findings and earlier partials: resolution**
- **AR2-M1: RESOLVED.**
  - IMP:126 (R3.8k): a target whose mode or matched outcomes changed goes to E13's collection variant and a fresh preview; one that no longer fits gets E46.
  - IC:23 adds E13's collection variant.
  - IJ:132 now sets up a target holding an M2 reading and expects E46. IJ:133 (a pending-only target switched to M1) expects E13 and a fresh E43 with the mode-change line. IJ:134 covers a matched item scanned while the preview is open.
- **AR2-M2: RESOLVED.** DF:187 keeps the reading R2.3j would leave current, with no question. DFJ:124 covers the three pairings. A residual ordering gap is AR3-m1.
- **AR2-m1: RESOLVED.** EX:145, EX:180, EXJ:3 and DF:281 all read R7.7a–o. A grep finds no remaining "R7.7a–n".
- **AR2-m2: RESOLVED.** IMP:182 (R6.8f) now says "a quarantined reading never counting". IMP:184 (R6.8b) recovers the item, IJ:151 tests it, and ADR:22 exempts quarantined readings.
- **AR2-m3: RESOLVED.** ADR:22: "compared exactly, so an imported spectrum reads back as the numbers given".
- **AR2-m4: RESOLVED.** EX:94 (R1.1t) labels a kept-behind reading `superseded`. EX:32's closed set is unchanged, so this is no format change.
- **AR2-n1: RESOLVED.** ADR:22 now has the sentence break, "determinable by plain SQL at the floor" and "if that is enforced as a uniqueness constraint". A residual citation nit is AR3-n2.
- **AR2-n2: RESOLVED.** DFJ:113: "no payload, and no archive-unavailable mark".
- **AR2-n3: RESOLVED.** IC:24 (and IC:29) now read "scanned here with an instrument", which excludes a Demo Device reading (R6.8g).
- **Round-2 Missing, the re-import-after-restore case: RESOLVED.** IJ:152, run once Collection Mode R5.5 lands.
- **Round-1 M5 (was PARTIAL): RESOLVED** by AR2-M1's fix (IMP:126, IJ:130–134).
- **Round-1 M7 (was PARTIAL): RESOLVED** by AR2-m1's fix.
- **Round-1 m1 (was PARTIAL, then AR2-M2): RESOLVED** at DF:187.

**New findings: no new Blocker or Major.**

- **[MINOR] AR3-m1 — DF R5.5d salvage: "the one R2.3j would leave current" loses record order.**
  - **Where:** DF:187 and DFJ:124.
  - **Problem:** R2.3j is written for an arriving reading B, but R5.5d applies it to a pair without saying which reading arrived. Take an imported reading recorded first and a Demo Device reading recorded later (a simulated re-scan over it, R2.3b).
    - A build that reads R2.3j symmetrically ("imported is current over simulated") keeps the imported reading current.
    - A build that replays the two in record order keeps the simulated re-scan current, with a correction-unconfirmed reason.
    - DFJ:124 only tests pairs where the imported reading was recorded later, so both builds pass.
  - **Also:** in pairing (a), R2.3j makes the simulated reading B's predecessor (reason re-measurement). R5.5d calls it "kept behind", and R2.3h says a kept-behind reading is no reading's predecessor.
  - **Fix:** reword to "replaying the two in record order, the one then current stays current (R2.3j where the later one is imported, else the later-recorded). A reading R2.3j keeps behind keeps reason initial and gets no question; otherwise the other is retained as correction-unconfirmed." Add the imported-then-simulated pairing to DFJ:124.
  - **Reach:** only corrupted files get here.
- **[MINOR] AR3-m2 — Unknown snapshot fields: "empty" versus "absent (NULL)" drift, including the ADR-0003 input.**
  - **Where:** DF:137 (R2.3j), DFJ:113 and DEVJ:80 store the unknowns as absent (NULL). Three places lag:
    - Import UJ 3's SQL oracle (IJ:118) still reads "serial and firmware empty".
    - ADR-0003's input (ADR:22) says "stored empty" and never says the model is absent when Nix Device is blank (IMP:170).
    - Post-lock's paraphrase of that input (PL:172) still says "stored empty" and lacks round 2's additions: exact read-back, at most one current reading per item, and the quarantined exemption.
  - **Why it matters:** a build that stores an empty-string placeholder passes IJ:118 and fails only DJ6. And the schema author, facing the sharpest one-way door, reads ADR:22 first.
  - **Fix:** say "absent (NULL)" at IJ:118 and ADR:22, model included. Have PL:172 point at ADR:22 instead of paraphrasing it.
- **[MINOR] AR3-m3 — Import R6.4: E11 is listed among the exclusions that run before the file-level checks, but E11 needs a target.**
  - **Where:** IMP:169 runs "§1–§3's exclusions (E7, E9, E10, E11)" before the mixed-mode check, and R6.11 (IMP:176) inherits that list. E11 (R3.4, IMP:81) excludes a code that matches several existing items, so it needs the target. UJ 3 runs E46's mixed-modes check and E49 at "Read the file", before any target exists (IJ:126–128).
  - **Problem:** a build that follows R6.4 literally cannot run those checks at read. A build that ignores E11 there passes every case.
  - **Fix:** in R6.4, the file-level exclusions (E7, E9, E10, E47) run first. E11 excludes after target selection and never reopens the mode or collection-name check. This one change also fixes R6.11.
- **[MINOR] AR3-m4 — Import R6.1: recognition after a manual encoding choice (outside this lens; the interface lens should own it).**
  - **Where:** IFX:24 says "a file that fails UTF-8 decoding is never recognised", but no row carries that.
  - **Problem:** after E5's text variant, "Choose an encoding" set to Windows-1252 or UTF-16 re-reads the file (R3.8a, IMP:116). R6.1's header test (IMP:166) does not depend on the encoding, so the re-read file matches, and R6.1 then says it "is read as UTF-8". A builder has to choose between three outcomes:
    - recognise it under the chosen encoding;
    - loop back to E5;
    - import it as a plain CSV. That files the reflectance and date columns as metadata with no readings, and they stay beside the real readings after a later correct import.
  - **Fix:** state it in R6.1, for example "a source whose UTF-8 read's first record …", and add a UJ 3 case with a Windows-1252 re-save.
- **[NIT] AR3-n1 — Import R3.8k: the recheck should run inside the commit's hold.**
  - **Where:** IMP:126 rechecks outcomes "then commit[s] once"; R3.2 (IMP:79) holds other writes only "while it commits".
  - **Problem:** a Collection Mode Flag, restore or delete landing between the recheck and the write would commit an outcome the preview never showed. That is AR2-M1's hazard through a window of milliseconds.
  - **Fix:** one clause: "the recheck runs under the commit's R1.11 hold". Reach is close to nil in a single-user app.
- **[NIT] AR3-n2 — ADR-0003 input: a wrong citation and a missing field.**
  - **Citation:** ADR:22 cites DF R1.2 for "each reading's predecessor determinable by plain SQL". R1.2's list (DF:99) names canonical selection and supersession reasons, not the predecessor; this was my own round-2 wording. Cite R2.3h alone for the predecessor.
  - **Missing field:** the input omits the imported reading's own-values reference, stored as the file's text (IMP:170). No other kind of reading carries that field.

#### Biggest risks
1. **Drift in the ADR-0003 input (AR3-m2, AR3-n2).** The ADR row is the schema author's entry point, and it now lags the DF row it summarises on how unknowns are stored. It is cheap to fix now and costly once the file format ships.
2. **Salvage ordering (AR3-m1).** Builds can split, but only on corrupted files, and salvage keeps every reading either way.
3. **Check order at file-read time (AR3-m3, AR3-m4).** A builder has to guess at read time. The worst outcome, a silent plain-CSV import of a re-encoded export, pollutes the collection's columns for good.

#### Genuinely sound
- **R3.8k's re-derivation at commit.** It compares mode fit and every R6.8 outcome and returns to a fresh preview instead of committing. That closes the hazard of an outcome the preview never showed, including a live scan landing under an open preview (IJ:134). A dogmatic review might ask for optimistic versioning or locking. A single-writer file under DF R1.11 does not need it.
- **E13's collection variant reuses an existing state instead of adding a new ID.** That is the right amount of mechanism.
- **"Absent (NULL), never a placeholder" does not decide ADR-0003's schema.** Any relational layout reads NULL at the floor: a nullable column, or a missing side-table row read through a LEFT JOIN. The rule only forbids sentinel values, which is a WHAT.
- **Exact spectral read-back does not decide the blob schema.** 64-bit floating point and decimal text both satisfy it. It rules out only lossy encodings (32-bit or quantized), which the same-reading key could not survive anyway, and the size cost is trivial.
- **Declaring imported readings directly (R6.10, CM:211, PL:146) points the dependency the right way.** Collection Mode, Device and Export depend on R2.3j's stored shape and R1.21's kind, not on Import §6. The importer-produced R7.7o still pins that shape for the Export goldens.
- **A quarantined reading never counts as the same reading.** Re-import becomes a recovery path (R6.8b) without promoting a readable reading from history, so R2.9 holds.
- **The file's illuminant and observer are stored as text, as provenance only.** That avoids a lossy mapping of the vendor's strings onto the app's reference set; R6.6 interprets them only during import.
- **The one-current-per-item input is permissive ("can enforce").** It stays consistent with R2.2 and R5.5d's read-only handling of a file written elsewhere.
- **ADR-0001 is untouched.** Nothing in this pass touches the `SpectroDevice` seam.

#### Missing / over-engineered
**Missing:**
- The fixes for AR3-m1 to m4.
- A salvage case for an imported reading followed by a simulated one (DFJ:124).
- A UJ 3 case for a Toolkit export re-saved in a non-UTF-8 encoding.

**Over-engineered:** nothing.

The Nits are optional and hold no row. Each OBJECT below is for a Minor.

| Row ID | disposition |
| :--- | :--- |
| Import-R1.5 | ALIGN |
| Import-R3.2 | ALIGN |
| Import-R3.3 | ALIGN |
| Import-R3.5 | ALIGN |
| Import-R3.8 | ALIGN |
| Import-R6.1 | OBJECT (AR3-m4) |
| Import-R6.2 | ALIGN |
| Import-R6.3 | ALIGN |
| Import-R6.4 | OBJECT (AR3-m3) |
| Import-R6.5 | OBJECT (AR3-m2) |
| Import-R6.6 | ALIGN |
| Import-R6.7 | ALIGN |
| Import-R6.8 | ALIGN |
| Import-R6.9 | ALIGN |
| Import-R6.10 | ALIGN |
| Import-R6.11 | ALIGN |
| Import-M2 | ABSTAIN (out of lens) |
| Import-E5 | ABSTAIN (out of lens) |
| Import-E7 | ABSTAIN (out of lens) |
| Import-E9 | ABSTAIN (out of lens) |
| Import-E10 | ABSTAIN (out of lens) |
| Import-E13 | ALIGN |
| Import-E14 | ALIGN |
| Import-E42 | ABSTAIN (out of lens) |
| Import-E43 (Toolkit variant) | ALIGN |
| Import-E46 | ALIGN |
| Import-E47 | ABSTAIN (out of lens) |
| Import-E48 | ALIGN |
| Import-E49 | ALIGN |
| DF-R1.2 | ALIGN |
| DF-R1.6 | ALIGN |
| DF-R2.1 | ALIGN |
| DF-R2.2 | ALIGN |
| DF-R2.3 (with R2.3h, R2.3j) | ALIGN |
| DF-R2.4 | ALIGN |
| DF-R5.5 (with R5.5d) | OBJECT (AR3-m1) |
| DF-R7.1 | ALIGN |
| DF-R7.2 | ALIGN |
| DF-R7.7 (with R7.7o) | ALIGN |
| Device-R1.21 | ALIGN |
| Device-R1.22 | ALIGN |
| CM-R2.4 (with R2.4j) | ALIGN |
| CM-R2.7 | ALIGN |
| CM-R4.2 (with R4.2d) | ALIGN |
| CM-R5.2 (with R5.2b/e) | ALIGN |
| CM-R8.9 | ALIGN |
| CM-M2 | ABSTAIN (out of lens) |
| CM-E12 | ABSTAIN (out of lens) |
| Export-R1.1 (with R1.1h/i/p/s/t) | ALIGN |
| Export-R1.2 | ALIGN |
| Export-R4.2 | ALIGN |
| Export-E1 | ALIGN |
| Capture-R1.10 | ALIGN |
| Capture-M8 | ABSTAIN (out of lens) |

### privacy

#### Verdict
Privacy-sound to ship. All four round-2 privacy items are resolved in 7332d69. A count-only check against the real export finds none of its data values in the round-2 fixes, the new Round 2 log section, or the commit message. The fixes add no Blocker or Major. One new Low and two Info findings remain; none blocks.

#### Data-flow & PII map (brief)
- **Data subject.** The Cataloger (the owner). The only outside recipient is anyone reading the public repository. The branch is still not on the remote (`git ls-remote` shows no head), and 417fdac is on no ref.
- **Personal fields in a Toolkit record.** Date Saved (an activity timestamp), Note (free text), Custom Collection Name (a user label) and Nix Device (a model name; whether a user can rename it is still deferred at `docs/product/post-lock.md:13`).
- **What an import stores.**
  - Date Saved becomes the measurement time.
  - The model goes into an imported-kind snapshot, or is absent where the cell is blank (`docs/product/import/prd-inventory-import.md:170`). Serial and firmware are absent (NULL), never a placeholder (`docs/product/data-foundation/prd-data-foundation.md:137`).
  - Note and densities are stored as metadata; a Note of `undefined` supplies nothing.
  - Custom Collection Name is read only, to pre-fill a new target's name and for the one-collection check (`:167`, `:168`, `:176`).
- **What an export carries.** Only on an export the user starts: model, measured-at, `sc_imported`, the imported columns, and now a `superseded` label for a reading kept behind. Serial, firmware, basis and payload cells stay empty (`docs/product/export/prd-data-export.md:94`).
- **Repository boundary.**
  - The dogfood records are now bounded (M2 `:195`, OQ 3 `:205`), and AGENTS.md carries a standing rule (`AGENTS.md:71`).
  - The fixtures, the new cases and the Round 2 log hold no real value.

#### Findings
Paths are relative to `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/`.

**Re-check of my round-2 findings (7332d69)**
- **PRIV2-1 (MEDIUM) — RESOLVED.**
  - Where it landed:
    - M2's Method now reads "recording only format facts and tallies by state across files — never a collection name, code, name, note, date, measured value, file name or per-file count" (`docs/product/import/prd-inventory-import.md:195`).
    - OQ 3 says "as M2 bounds them" (`:205`).
    - R6.10 says "any data value from one" (`:175`).
    - The fence is F71's round-2 Clarified line (`docs/product/import/prd-inventory-import-fences.md:176`), mirrored at `docs/product/post-lock.md:205`.
  - The "recording only" wording also rules out a file path, which the never-list itself leaves out.
  - One edge case remains; see PRIV3-1.
- **PRIV2-2 (LOW) — RESOLVED.**
  - `AGENTS.md:71` adds the §5 line beside the SDK and license rule, screenshots included.
  - Its anchor resolves to `docs/product/import/prd-inventory-import.md:160`.
  - Its word "counts" overlaps with M2; see PRIV3-1.
- **PRIV2-3 (INFO) — RESOLVED.** E48 now reads "Note, the Density columns and any other column come in as details, listed in the preview" (`docs/product/import/prd-inventory-import-copy.md:34`).
- **PRIV-7 nit — RESOLVED as recorded.**
  - `docs/product/import/prd-inventory-import-round-2-fixes.md:50` gives the reason: a review-process rule, kept in the review log.
  - The round-1 entry still sits under that file's "Deferred to post-lock" heading (`prd-inventory-import-round-1-fixes.md:67`). This is harmless.
- **Round-1 findings left PARTIAL or UNRESOLVED: none.** PRIV-1 to PRIV-9 were all RESOLVED in round 2 (review log `:3211`–`:3234`).
  - PRIV-1 is re-confirmed by the value check below.
  - PRIV-8's deferral stands at `docs/product/post-lock.md:13`. R6.5's new "absent where the cell is blank" wording (`prd-inventory-import.md:170`) is consistent with it.

**Real-value check (count-only; no value printed or quoted)**
- **What I compared.** I read the real export locally and compared each of these fields against the round-3 targets listed below:
  - every collection name, colour name and code;
  - every Note other than `undefined`;
  - each Date Saved in full, to the minute, by day and by time of day;
  - each HEX and each sRGB triplet;
  - every numeric cell as written, and Lab rounded to two places;
  - the file's name, and a phrase giving its reading count.
- **What I compared against.**
  - Every added line of `git diff f7cbde5 7332d69 -- docs AGENTS.md`, which includes the whole new Round 2 log section.
  - The commit message of 7332d69.
  - The whole tree at 7332d69, for the most specific categories.
- **Results.**
  - Zero hits anywhere in the tree for the collection name, the notes, full or to-the-minute timestamps, time of day, HEX, sRGB triplets, the file name or the reading-count phrase.
  - The commit message has zero hits in every category.
  - Every whole-word hit in the added lines was a coincidence: three-digit line-number citations, one common English word in the ADR-0003 row, generic two-place decimals used as offsets or margins (including the new sRGB margin at `docs/product/import/prd-inventory-import-journeys.md:106`), and calendar dates outside every fixture, case and oracle.
  - Fixture T's new values, the negative `R400 nm` included, are invented. So are Collection Mode's UJ2.1-t and DJ6's salvage case.

**New findings**
- **No new Blocker or Major.**

**[LOW] PRIV3-1 — `AGENTS.md:71` against `docs/product/import/prd-inventory-import.md:195` (M2) and `:205` (OQ 3), `prd-inventory-import-fences.md:176` and `docs/product/post-lock.md:205` — "counts" means two things, and one file makes a tally a per-file count**
- **Two readings of "counts".**
  - AGENTS.md bars "any value from it (… counts …)", while F71's Clarified line and M2 allow "tallies by state across files".
  - An agent that reads AGENTS.md first cannot record OQ 3's tallies. That is round 2's dead end, inverted.
  - Read literally, AGENTS.md's "counts" also covers UJ 3's stated column count.
- **The one-file case.** M2 bans a "per-file count", but while the corpus is still "one studied export" (`:205`), a tally across files is that file's own count. That includes the reading count Round 0 redacted (review log `:18`).
- **Harm.** Low-sensitivity facts: the size of the owner's collection and its differing and not-checked tallies. They cannot be taken back once pushed.
- **Fix (one clause each, non-blocking; M2 stays ALIGN).**
  - M2 and OQ 3: "tallies by state summed over two or more files; from a single file, only format facts and whether it imported cleanly".
  - AGENTS.md: "counts" becomes "record counts (format facts and Import M2's cross-file tallies aside)".
- Principle: GDPR Art. 5(1)(c) minimisation; a single-file tally is not an aggregate.

**[INFO] PRIV3-2 — E43's and E1's claims about serial and firmware against R6.2's "any other column"**
- E43 (`docs/product/import/prd-inventory-import-copy.md:29`) says "SpectroCapture doesn't take the instrument's serial, firmware or sample count from a Toolkit export". E1 (`docs/product/export/prd-data-export-copy.md:16`) says the export "records no instrument serial, firmware…".
- R6.2 (`prd-inventory-import.md:167`) imports any other column as metadata.
- If a later Toolkit version adds a serial or firmware column, that persistent device identifier would be stored and exported as a detail column while both copy lines say it isn't. It would still be listed under E43's Added columns.
- The data is the owner's own, it is disclosed, and the format is hypothetical, so this is Info.
- Fix: add "whether a serial or firmware column appears" to OQ 3's Closer (`:205`), and revise R6.2 and both copy lines from the answer.

**[INFO] PRIV3-3 — the review log keeps a few coarse facts about the owner's export**
- Earlier lenses' text, filed verbatim, states how many scanning sessions the export spans (`docs/agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md:1231`, `:1525`).
- My round-2 wording names the kind of date that coincidentally matched (`:3214`).
- None of this is a data value, and the harm is negligible. Now that `AGENTS.md:71` lists "dates, counts", though, later rounds should report matches by category only, without saying which dates coincided. This review does so.
- No redaction is needed.

#### Biggest privacy risks
1. **PRIV3-1.** Closing OQ 3 or M2 is the next time facts derived from a real export are written into the repo. With a one-file corpus, the tally allowance turns back into the per-file count M2 bans.
2. **The standing rule is text only.** It depends on agents reading `AGENTS.md:71`. No automated check can hold the real values, because that check would have to read the real file and so must stay uncommitted. The orchestrator's local check is the only control, which is acceptable for a one-owner repo.
3. **Residual (settled and disclosed).** Imported readings are permanent short of deleting the swatch or its collection (N6, N14, `docs/product/data-foundation/prd-data-foundation.md:217`). E43 now says so accurately.

#### Genuinely privacy-respecting
- **Accurate erasure disclosure.** E43 now names both ways to erase ("only deleting the swatch or its collection removes them", `prd-inventory-import-copy.md:29`). This matches DF R6.1 exactly (`prd-data-foundation.md:217`); round 1's wording named only the swatch.
- **No invented identifiers.** Unknowns are absent (NULL), never a placeholder (DF R2.3j `:137`; DJ6 `docs/product/data-foundation/prd-data-foundation-journeys.md:113`). A blank Nix Device leaves the model absent (`prd-inventory-import.md:170`), and it renders as "model unknown" (`docs/product/collection-mode/prd-collection-mode-copy.md:295`, `:307`). A checklist might flag "serial empty" as missing data; it is the accurate and minimal choice (Art. 5(1)(d)).
- **No path for real data into other harnesses.** Under R6.10's direct-seed path (`:175`), other PRDs may seed an imported reading only with an invented spectrum, and only Import's cases and DF R7.7o run the importer. So no Collection Mode, Device or Export harness ingests an export file. The new Build-dependencies row (`docs/product/collection-mode/prd-collection-mode.md:211`) seeds directly.
- **No pile-up from the quarantine exemption.** R6.8f's exemption (`prd-inventory-import.md:182`) adds at most one reading per re-import; after that the new copy counts as the same reading.
- **Allow-list form for dogfood records.** M2's and OQ 3's bound says what may be recorded ("recording only"), rather than listing what may not.
- **A broad standing rule.** The AGENTS.md rule covers screenshots and "anyone's collection", so it reaches bug reports, help docs and ADRs.

#### Missing controls / over-collection
- There is no bound for a one-file corpus, and "counts" is read two ways (PRIV3-1).
- OQ 3 does not ask whether a future export carries a serial or firmware column (PRIV3-2).
- The delta adds no over-collection. It stores no new field. The only additions are a "model unknown" rendering and Export's `superseded` label on a reading kept behind.

| Row ID | disposition |
|---|---|
| Import-R1.5 | ABSTAIN |
| Import-R3.2 | ABSTAIN |
| Import-R3.3 | ALIGN |
| Import-R3.5 | ABSTAIN |
| Import-R3.8 | ABSTAIN |
| Import-R6.1 | ABSTAIN |
| Import-R6.2 | ALIGN |
| Import-R6.3 | ALIGN |
| Import-R6.4 | ABSTAIN |
| Import-R6.5 | ALIGN |
| Import-R6.6 | ABSTAIN |
| Import-R6.7 | ABSTAIN |
| Import-R6.8 | ALIGN |
| Import-R6.9 | ALIGN |
| Import-R6.10 | ALIGN |
| Import-R6.11 | ALIGN |
| Import-M2 | ALIGN |
| Import-E5 | ABSTAIN |
| Import-E7 | ABSTAIN |
| Import-E9 | ABSTAIN |
| Import-E10 | ABSTAIN |
| Import-E13 | ABSTAIN |
| Import-E14 | ABSTAIN |
| Import-E42 | ABSTAIN |
| Import-E43 (Toolkit variant) | ALIGN |
| Import-E46 | ABSTAIN |
| Import-E47 | ABSTAIN |
| Import-E48 | ALIGN |
| Import-E49 | ABSTAIN |
| DF-R1.2 | ALIGN |
| DF-R1.6 | ALIGN |
| DF-R2.1 | ALIGN |
| DF-R2.2 | ABSTAIN |
| DF-R2.3 (with R2.3h, R2.3j) | ALIGN |
| DF-R2.4 | ABSTAIN |
| DF-R5.5 (with R5.5d) | ABSTAIN |
| DF-R7.1 | ABSTAIN |
| DF-R7.2 | ABSTAIN |
| DF-R7.7 (with R7.7o) | ALIGN |
| Device-R1.21 | ALIGN |
| Device-R1.22 | ALIGN |
| CM-R2.4 (with R2.4j) | ALIGN |
| CM-R2.7 | ABSTAIN |
| CM-R4.2 (with R4.2d) | ALIGN |
| CM-R5.2 (with R5.2b/e) | ALIGN |
| CM-R8.9 | ABSTAIN |
| CM-M2 | ABSTAIN |
| CM-E12 | ALIGN |
| Export-R1.1 (with R1.1h/i/p/s/t) | ALIGN |
| Export-R1.2 | ALIGN |
| Export-R4.2 | ALIGN |
| Export-E1 | ALIGN |
| Capture-R1.10 | ABSTAIN |
| Capture-M8 | ABSTAIN |

### product-marketing

#### Verdict
Lands after one Major fix; there are no Blockers. Every round-2 finding has been resolved, E48 no longer overclaims, and the Toolkit remedies are written out in full. One new Toolkit string does not work: E5's Toolkit variant tells the user to re-export, and in the case the PRD itself builds for it, re-exporting reproduces the same error.

#### Audience & message context (brief)
- **Reader:** the Cataloger, usually a Nix Toolkit phone-app user bringing a collection over. Export E1 is read by the Data consumer. The vision is read by contributors and by whoever writes the README and launch copy later.
- **Intended takeaway:** "My Toolkit colours arrive already measured and marked as imported. Nothing is invented. When something goes wrong, I'm told what to do next."
- **Checked against:** `git diff f7cbde5 7332d69 -- docs AGENTS.md`, the round-2 fix file, F73/F76/F82's round-2 Clarified lines, owner decisions N1–N23, and the rows each string depends on. No value from the research report or from a real export is quoted.
- **Path keys** (repo root `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import`):
  - **IC** = `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import-copy.md`
  - **IMP** = `…/docs/product/import/prd-inventory-import.md`
  - **IJ** = `…/docs/product/import/prd-inventory-import-journeys.md`
  - **CMC** = `…/docs/product/collection-mode/prd-collection-mode-copy.md`
  - **EXC** = `…/docs/product/export/prd-data-export-copy.md`
  - **VIS** = `…/docs/product/vision.md`
  - **CAP** = `…/docs/product/capture-mode/prd-capture-mode.md`
  - **R1F** = `…/docs/product/import/prd-inventory-import-round-1-fixes.md`

#### Findings

##### Round-2 findings — status
- **PMM2-M1 — RESOLVED.** IC:34 now reads "The file's own Lab is checked against its spectrum; its Lab, LCh, XYZ, sRGB and HEX aren't kept." That matches R6.6 (IMP:171) and R6.2 (IMP:167). The residual wording issue is PMM3-n3.
- **PMM2-M2 — RESOLVED.**
  - Written Toolkit variants are at IC:17, :19, :20 and :28. E7 keeps "bring in the rest" and E42 keeps "nothing has been imported".
  - IC:39 is now just a pointer.
  - UJ 3 has an E10 case (IJ:148).
  - Residuals: PMM3-m1 (test coverage) and PMM3-n5 (stale fix-file line).
- **PMM2-m1 — RESOLVED.** IC:29 adds "…and a swatch keeps a later Toolkit reading over an older one". Same-moment readings have their own line.
- **PMM2-m2 — RESOLVED.** IC:24 and IC:29 now say "scanned here with an instrument", which fits R6.8g (IMP:186).
- **PMM2-m3 — RESOLVED.** IC:29 reads "only deleting the swatch or its collection removes them", matching Collection Mode E17.
- **PMM2-m4 — RESOLVED.** IC:29 reads "SpectroCapture doesn't take … from a Toolkit export". Export E1 reintroduces the old claim (PMM3-m3).
- **PMM2-m5 — RESOLVED.** IC:29 reads "The row counts then read Swatch details — New…".
- **PMM2-m6 — RESOLVED.** IC:24 gives the Toolkit headline "⟨n⟩ swatches that already have a colour have different details in this file". It is true on both a first import and a re-import, because E14 fires only for captured items (R3.5, IMP:82).
- **PMM2-m7 — RESOLVED.** IC:37 adds "except for a Toolkit export, whose settings are shown fixed without controls (R6.1)".
- **PMM2-m8 — RESOLVED.** IC:39 and R6.3 (IMP:168) now say "labelled as set by the export". The Capture-side mirror is PMM3-n6.
- **PMM2-n1 — RESOLVED.** IC:29 reads "Now a swatch's colour". It also reads correctly for R6.8a's new swatches.
- **PMM2-n2 — RESOLVED.** IC:29 reads "were saved at the same moment as".
- **PMM2-n3 — RESOLVED.** One form, "— the Toolkit's Measurement Mode —", is used at IC:29 and in both E46 variants (IC:32).
- **PMM2-n4 — RESOLVED.** IC:32 adds a next step for mixed modes. Whether a user can act on it is PMM3-m2.
- **PMM2-n5 — RESOLVED.** IC:29 reads "so they're no longer set aside".
- **PMM2-n6 — RESOLVED.** CMC:295 reads "the model or model unknown" and CMC:307 reads "or 'model unknown'". IJ:150 asserts it.
- **PMM2-n7 — RESOLVED.** VIS:13 now has the headline "has no home the owner controls", and the first mention is "The mobile Nix Toolkit app's CSV export, in the one export studied".
- **PMM1-m5 (round-1, PARTIAL) — RESOLVED.** EXC:16 names the averaging basis and archived readings, matching R1.1t (`…/docs/product/export/prd-data-export.md:94`). The claim's scope is PMM3-m3.

##### New findings

**[MAJOR] PMM3-M1 — E5's Toolkit remedy doesn't fix the case the PRD builds for it.** Locations: IC:15; routed by R6.1 at IMP:166; exercised by UJ 3 at IJ:147.
- **The copy:** "A line of this Nix Toolkit export can't be read with its quotes as written. Export the file again from the Nix Toolkit, then pick it again."
- **The case:** IJ:147 reaches this state with a colour name holding an unescaped `"`. That is what the Toolkit itself writes if it doesn't double a quote, and OQ 3 (IMP:205) lists "how a quote … inside a name is written" as unknown.
  - The round-2 finding that asked for this variant (PM-R2-m3) used the same scenario. Its fix read "*Correct it* in the Nix Toolkit and export it again".
  - The text that landed dropped "Correct it".
  - This is an inference: the placeholder Note that R6.2 handles suggests a hand-built writer, which makes unescaped quotes more likely.
- **Reader reaction:**
  - The migrator re-exports, picks the file, and gets the same E5.
  - The state refuses the whole file. There is no "Continue without them", and R6.1 withholds the read controls, so the offered actions are just "Pick the file again" and "Cancel".
  - It names no line, so a user with 200 colours can't find the one to rename.
  - The first Toolkit import ends in a loop, which undercuts the amendment's promise ("without re-scanning", VIS:160).
- **Mutation:** a build that ships IC:15 word for word passes IJ:147 and still leaves the user in the loop. No case checks that the remedy clears the state.
- **Rewrite:** "A line of this Nix Toolkit export can't be read with its quotes as written — usually a colour's name, code or note holding a quote mark ("). Remove it in the Nix Toolkit, export the collection again, then pick the file again."
  - Better still, let R6.1 have this variant name the record it stopped at, "(record ⟨n⟩)", as E7 and E47 already list theirs.
- **Why Major, not Blocker:** nothing false is said about what happened to the user's data, the trigger is uncommon, and the fix is one sentence.

**[MINOR] PMM3-m1 — Only E10's Toolkit variant is tested, and R6.1 no longer names the later steps.**
- R6.1 (IMP:166) now says only that "mapping (E48) and preview (E43) name it a Toolkit export".
- The Toolkit variants of E7, E9 and E42 (IC:17, :19, :28) rest on the copy note and F73's round-2 Clarified line, and only E10 has a UJ 3 case (IJ:148).
- A build that shows the generic "fix the file and try again" on a Toolkit export's E7, E9 or E42 passes every case. That sends the migrator to the spreadsheet edit IC:39 warns against.
- **Fix:** add "E5, E7, E9, E10 and E42 show their Toolkit variants" to R6.1, and add a UJ 3 case: all three Color Codes blank gives E9's Toolkit variant, then "Continue without them" gives E42's.

**[MINOR] PMM3-m2 — The E46 mixed-modes next step (IC:32) can't be acted on as written.**
- "Keep each measurement condition's colours in their own Nix Toolkit collection" needs the user to know which colours are in which mode. The state lists none, so the user must open every colour in the Toolkit.
- Whether the Toolkit can move a saved colour between collections is also unverified.
- **Fix:** list the records under each mode, as E47 lists its records ("Measured in ⟨mode⟩: ⟨list⟩"). Add the move question to OQ 3's closer.

**[MINOR] PMM3-m3 — Export E1 (EXC:16) states as a fact about the vendor's file what E43 now scopes to SpectroCapture.**
- E1 says the readings "came from a Nix Toolkit export, which records no instrument serial, firmware or averaging basis and no archived instrument readings". This is the PMM2-m4 problem again, and it was my own round-2 rewrite.
- OQ 3 is still open. R6.2 imports any other column as metadata, so a later export with a serial column would make this sentence false while R1.1t still empties the cell.
- **Word-neutral rewrite:** "⟨imported⟩ readings came from a Nix Toolkit export; SpectroCapture takes no instrument serial, firmware, averaging basis or archived instrument readings from one, so those cells stay empty."

**[NIT] PMM3-n1 — Two phrasings for the same remedy.** E5 (IC:15) and E47 (IC:33) say "export the file again". E7, E9, E10 and E42 say "export the collection again". The Toolkit exports a collection, so use "the collection" everywhere.

**[NIT] PMM3-n2 — "Unknown" versus "not recorded".** E43 (IC:29) says the sample count "show[s] as unknown". Collection Mode shows it as "not recorded" (CMC:295, :310), and E12 says "weren't recorded". Use "so they show as unknown or not recorded".

**[NIT] PMM3-n3 — E48 (IC:34) is slightly broader than the rows behind it.**
- "The file's own Lab is checked against its spectrum" is unconditional, but R6.6 leaves a record unchecked when its Lab is missing or its illuminant/observer pair isn't offered. Add "where it can be".
- "Listed in the preview" holds only for new columns. Rewrite as "…come in as details; new ones are listed in the preview."

**[NIT] PMM3-n4 — UJ 3 still uses E43's retired labels.** It says "used as the swatch's colour" (IJ:117), "used as colour" and "kept as earlier" (IJ:136, :139), and "kept as earlier" (IJ:141). This is harmless under R4.2, but whoever writes the string checks reads these first.

**[NIT] PMM3-n5 — Stale line in the round-1 fix file.** R1F:36 still says E47 ends "Correct it in the Nix Toolkit and export it again". This was the leftover sub-point of PMM2-M2. Mark it superseded by round 2.

**[NIT] PMM3-n6 — The creation-form string lives only in Import's copy.** The string at IC:39 labels a control on Capture §1's form, and Capture §12 owns labels (IC:4). Capture R1.10 (CAP:147) says only "fixed", not labelled. Add "labelled as set by the export" to R1.10 or to the Capture obligation line (IMP:153).

**Checked and not flagged:**
- E43's history explanation shows both halves even when only one applies. Neither half is false.
- E13's collection variant says "while you were looking at the preview" even though the user may have been scanning. This repeats inherited E13 wording and is accurate enough.

#### Biggest risks
- **E5 loops (PMM3-M1).** A Toolkit export with a quote mark in one colour name refuses the whole file. The only remedy offered reproduces the error, and nothing says which colour to fix.
- **Untested remedy routing (PMM3-m1).** Three of the five written Toolkit remedies have no case. A build that shows the generic spreadsheet remedy instead ships green.
- **Two explanations of the same unknowns (PMM3-m3, n2).** Export blames the vendor's file while E43 describes SpectroCapture's choice, and sample count is "unknown" in one place and "not recorded" in another.

#### Genuinely strong
- **E48 now says exactly what the app does.** The one sentence that told a migrator their HEX and sRGB were verified is gone.
- **The written-out Toolkit variants read naturally.** They keep both the reassurance ("nothing has been imported") and the way forward ("bring in the rest").
- **E43's Toolkit variant is complete and true.** It explains why a reading is kept rather than used (an instrument scan wins even when older, and a later Toolkit reading wins over an earlier one). "Swatch details —" now separates row counts from reading counts, so a re-import no longer seems to contradict itself.
- **E14's Toolkit headline** fits a first import and a re-import of renamed colours alike.
- **The locked scan mode now has a reason:** "set by this Nix Toolkit export".
- **E13's collection variant is plain and blames no one.** It says what changed and what to do.
- **The vision is honest and approachable.** It introduces the Nix Toolkit app at first mention, and the headline no longer claims more than the body.
- **The register stays calm and neutral towards the vendor, with no hype added.** That is right for a migration feature.

#### Missing / over-hyped
- **Missing:**
  - A remedy that works for E5's Toolkit variant, and a pointer to the failing record (PMM3-M1).
  - A per-mode list in E46's mixed-modes variant (PMM3-m2).
  - Cases for the Toolkit variants of E7, E9 and E42 (PMM3-m1).
  - A Capture-side mirror of the creation-form label (PMM3-n6).
- **Over-hyped:** nothing in tone. The remaining overreach is about scope:
  - Export E1 states a fact about every Toolkit export from one studied file (PMM3-m3).
  - E48's "checked" is unconditional (PMM3-n3).

| Row ID | disposition |
|---|---|
| Import-R1.5 | ABSTAIN (out of lens) |
| Import-R3.2 | ABSTAIN (out of lens) |
| Import-R3.3 | ALIGN |
| Import-R3.5 | ALIGN |
| Import-R3.8 | ALIGN |
| Import-R6.1 | ALIGN |
| Import-R6.2 | ALIGN |
| Import-R6.3 | ALIGN |
| Import-R6.4 | ALIGN |
| Import-R6.5 | ABSTAIN (out of lens) |
| Import-R6.6 | ALIGN |
| Import-R6.7 | ALIGN |
| Import-R6.8 | ALIGN |
| Import-R6.9 | ALIGN |
| Import-R6.10 | ABSTAIN (out of lens) |
| Import-R6.11 | ALIGN |
| Import-M2 | ABSTAIN (out of lens) |
| Import-E5 | OBJECT (PMM3-M1) |
| Import-E7 | ALIGN |
| Import-E9 | ALIGN |
| Import-E10 | ALIGN |
| Import-E13 | ALIGN |
| Import-E14 | ALIGN |
| Import-E42 | ALIGN |
| Import-E43 (Toolkit variant) | ALIGN |
| Import-E46 | ALIGN |
| Import-E47 | ALIGN |
| Import-E48 | ALIGN |
| Import-E49 | ALIGN |
| DF-R1.2 | ABSTAIN (out of lens) |
| DF-R1.6 | ABSTAIN (out of lens) |
| DF-R2.1 | ABSTAIN (out of lens) |
| DF-R2.2 | ABSTAIN (out of lens) |
| DF-R2.3 (with R2.3h, R2.3j) | ABSTAIN (out of lens) |
| DF-R2.4 | ABSTAIN (out of lens) |
| DF-R5.5 (with R5.5d) | ABSTAIN (out of lens) |
| DF-R7.1 | ABSTAIN (out of lens) |
| DF-R7.2 | ABSTAIN (out of lens) |
| DF-R7.7 (with R7.7o) | ABSTAIN (out of lens) |
| Device-R1.21 | ABSTAIN (out of lens) |
| Device-R1.22 | ABSTAIN (out of lens) |
| CM-R2.4 (with R2.4j) | ALIGN |
| CM-R2.7 | ALIGN |
| CM-R4.2 (with R4.2d) | ALIGN |
| CM-R5.2 (with R5.2b/e) | ALIGN |
| CM-R8.9 | ALIGN |
| CM-M2 | ABSTAIN (out of lens) |
| CM-E12 | ALIGN |
| Export-R1.1 (with R1.1h/i/p/s/t) | ALIGN |
| Export-R1.2 | ALIGN |
| Export-R4.2 | ABSTAIN (out of lens) |
| Export-E1 | ALIGN |
| Capture-R1.10 | ALIGN |
| Capture-M8 | ABSTAIN (out of lens) |

As in rounds 1 and 2, Minors and Nits are listed but not raised as objections:
- PMM3-m1: Import-R6.1, E7, E9, E42
- PMM3-m2: Import-E46
- PMM3-m3: Export-E1
- PMM3-n1: Import-E5, E47
- PMM3-n2: Import-E43
- PMM3-n3: Import-E48
- PMM3-n6: Capture-R1.10, Import-R6.3
- PMM3-n4 falls on the journeys and PMM3-n5 on the round-1 fix file; neither has rows.

### plan

#### Verdict
Execute after fixing Blockers. No Blockers remain, and 12 of the 14 round-2 findings are resolved. However, the round-2 fixes added two Majors, and each one makes an unattended builder guess:
- the Toolkit build-order line contradicts R6.10 for the fixtures and goldens built on R7.7o;
- Fixture T's file values are pinned to a reference that the first build does not have.

Both need about one sentence each to fix.

#### Findings

Every path below is under `/Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import`. Abbreviations:
- IMP / IJ / IC = `docs/product/import/prd-inventory-import{,-journeys,-copy}.md`
- IFX2 = `docs/product/import/prd-inventory-import-round-2-fixes.md`
- DF / DFJ = `docs/product/data-foundation/prd-data-foundation{,-journeys}.md`
- CM / CMJ = `docs/product/collection-mode/prd-collection-mode{,-journeys}.md`
- EX / EXJ = `docs/product/export/prd-data-export{,-journeys}.md`
- DVJ = `docs/product/device-management/prd-device-management-journeys.md`
- CAP = `docs/product/capture-mode/prd-capture-mode.md`
- PL = `docs/product/post-lock.md`
- ADRQ = `docs/decisions/README.md`
- SPIKE = `docs/briefs/hardware-spike-brief.md`

##### Round-2 findings, plus the round-1 findings I left open: status

- **R2-M1 RESOLVED.** Another PRD's case may now declare an imported reading directly, and only Import's cases and DF R7.7o run the importer (IMP:175). DF:245 is consistent with that, since R7.7's matrix is built by the importer. CM:211 says "seeded directly". What is left is ordering, covered in R3-M1.
- **R2-M2 RESOLVED.** CMJ:237 now reads E17 in its "recorded order", which CM:386 (R5.3) gives E17's base body in P0. CMJ:238 (UJ2.1-t) separates the two orders.
- **R2-M3 PARTIAL.** The offsets at IJ:121–122 are now multiples of DERIVATION_TOLERANCE, and IJ:104 names the reference as R7.5's. Two problems remain:
  - That reference does not exist in the first build, and it is not a method (R3-M2).
  - R7.5 (DF:243) passes a build that sits up to 1× tolerance from the reference. TK-1's quarter-tolerance offset can therefore reach 1.25×, and a build that passes R7.5 can fail IJ:122. State the assumption the case makes (the app agrees with the reference on TK-1 within ¾ tolerance).
- **R2-M4 RESOLVED.**
  - IJ:132's target now holds a captured item with an M2 reading.
  - IJ:133 covers the switch on a target holding only pending items, through E13's collection variant (IC:23; R3.8k at IMP:126).
  - The precedence question that remains is R3-m1.
- **R2-M5 RESOLVED.** EX:145, EX:180, EXJ:3 and DF:281 all read R7.7a–o, and grep finds no "R7.7a–n" left.
- **R2-m1 RESOLVED.** DF:187 keeps "the one R2.3j would leave current", and DFJ:124 is the case for it. A reverse-order gap remains (R3-m6).
- **R2-m2 RESOLVED.** IMP:171 checks only "a pair the app offers (Capture R1.5 and OQ 21)", and IJ:123 declares the offered set under CAP:352 (R11.6).
- **R2-m3 RESOLVED.**
  - IMP:169 runs §1–§3's and R6.7's exclusions before the mode check.
  - IMP:176 runs E49 after it.
  - IJ:126 adds "Continue without them".
  - The E11 gap that remains is R3-m2.
- **R2-m4 PARTIAL.** The quoting half landed (IMP:166; IC:15; IJ:147). The decoding half ("a file that fails UTF-8 decoding is never recognised") appears only at IFX2:24, not in R6.1 or R1.5a (IMP:57). A recognised export with invalid UTF-8 in a later record still reaches E5's Text variant with "Choose an encoding". Whether a Windows-1252 re-read keeps it a Toolkit export is unstated.
- **R2-m5 RESOLVED.** IMP:172 says "truncated to the millisecond", and the exponent's sign is stated.
- **R2-m6 RESOLVED.** IC:24 and IC:29 limit the scan clause to "with an instrument". The later-Toolkit clause is added, and same-dated readings fall under the clashes sentence.
- **R2-n1 RESOLVED.** IJ:110 adds "and create a target", as do IJ:111 and IJ:146.
- **R2-n2 RESOLVED.** IJ:136 declares `Peach` and a pending `Deep Coral` with no imported columns, so "Updated 1 (TK-1)" follows from R3.6g.
- **R2-n3 RESOLVED.** IFX:26 now reads `1.04700000e+0`.
- **Round-1 M6 RESOLVED.** IMP:126, IJ:131–134.
- **Round-1 G1 PARTIAL.** The upstream order is at IMP:32, Collection Mode's row at CM:211, and the post-lock item at PL:146. The downstream wait on Import §6 is still unstated, and PL:146 contradicts it (R3-M1; see also Spec coverage gaps).
- **Round-1 G2 RESOLVED.** DF:245, DF:268, DF:281, EX:145, EX:180.

##### New in round 3

**[MAJOR] R3-M1 — PL:146 against IMP:175, DF:245, DF:268, DFJ:109–124 and EX:145 — sequencing of the fixtures the importer builds**
- **What the rows say:**
  - R6.10: only Import's cases and DF R7.7o run the importer.
  - DF R7.7: imported readings "come from Import R6.5 on a synthetic export".
  - R7.7o feeds DJ6 and Export R4.2's `sc_imported` goldens.
  - DJ6's actions read "Import fixture T and commit".
- **What PL:146 says:** Device R1.21 and DF R2.3j come "first; then Import §6, Collection Mode's imported mark … and Export's `sc_imported` goldens on R7.7o". That puts the goldens in the same tier as the importer they need. It then adds "Collection Mode, Device and Export cases may seed an imported reading directly".
- **Scenario:**
  - (a) An Export builder working before Import §6 lands follows PL:146 and seeds R7.7o directly. The goldens are then cut from declared values, against DF:245 and R6.10, and must be re-cut later.
  - (b) The DF PR that lands R2.3j "first" carries DJ6. DJ6 is P0, so it runs in the build that lands its rows, and it cannot run without the importer. That PR goes red or skips its own acceptance case.
- **Fix:** rewrite PL:146 as "…first; then Import §6 and Collection Mode's imported row; after Import §6, DF R7.7o, DJ6 and Export's `sc_imported` goldens". Drop "and Export" from the seeding sentence, since Export's only imported case is R7.7o's.

**[MAJOR] R3-M2 — IJ:104 and IMP:175 against DF:243 (R7.5), DF:328 (OQ 6), SPIKE:7 and SPIKE:53 — Fixture T's file values are pinned to a reference the first build lacks**
- **What the rows say:**
  - Fixture T's own Lab, LCh, XYZ, sRGB and HEX are "checked in from the independent reference [DF R7.5] checks the build against, worked out from each record's reflectances".
  - R7.5's reference is "independently known values from a published source named by the hardware spike".
  - DF OQ 6 says "no reference source named yet".
  - The spike "gates the first release, not the first build".
  - The spike's Q4 looks for a published set of curves with known values, which is a data set, not a method.
- **Scenario:**
  - In the first build there is no R7.5 source.
  - Even once one is named, a data set cannot supply values for invented spectra such as TK-1's negative reflectance or TK-3's reflectance above 1.
  - Most of UJ 3's E43 assertions depend on these cells (IJ:117's "no … differing … sentence", and IJ:121–123).
  - The builder therefore either picks an unstated library that is not "the" reference, or computes the values with the app (which R6.10 forbids), or stalls.
- **Fix:** name a method instead. For example: "worked out by a published colorimetric method (CIE 15 / ASTM E308 10 nm weights at D50/2° and D65/10°), implemented independently of the app's code and named in the fixture's provenance; R7.5's reference, once named, cross-checks it". Add that dependency to IMP:32. This also closes R2-M3's original request to name the tabulation.

**[MINOR] R3-m1 — IMP:126 (R3.8k) — precedence among the commit rechecks**
- IJ:132's target has both changed mode and stopped fitting, and the case expects E46. R3.8k lists "no longer fits → E46" and "mode … changed → E13's collection variant" without saying which wins.
- A changed source together with a changed mode names no variant.
- "Each matched item's R6.8 outcome" does not cover a record that was unmatched at preview but matches at commit (an item added with its code during the preview).
- Fix: state the order E40, then E46, then E13 (its collection variant where only the target changed), judged on matches as they stand at commit.

**[MINOR] R3-m2 — IMP:169 (R6.4) and IMP:176 (R6.11) against IJ:126–128 — E11 in the pre-check exclusions**
- E11 depends on the target (R3.4), but R6.4 lists it among the exclusions that run before the mixed-mode check.
- IJ:126–128 run E46 (mixed modes) and E49 at read, before any target exists.
- Scenario: a file whose only M1 record is E11-ambiguous in the chosen target. The cases refuse it; R6.4 imports it.
- Fix: E7, E9, E10 and R6.7's exclusions run first; the E46 mixed-mode and E49 checks run at read; E11 applies after target selection.

**[MINOR] R3-m3 — IJ:118 and ADRQ:22 against DF:137 and IMP:170 — NULL versus empty for unknowns**
- IJ:118 says "serial and firmware empty", and the ADR-0003 input says "stored empty".
- DF R2.3j says "absent (NULL), never a placeholder", and R6.5 says "absent".
- An Import test that asserts `''` fails a correct NULL build, and ADR-0003 could end up choosing `''`.
- Fix: "absent (NULL)" at both sites.

**[MINOR] R3-m4 — IJ:134 — a "live" scan cannot happen in a test with no hardware**
- The case says "scan TK-2 live in a session". The only seam for a scan in a session saves simulated snapshots (DF:245; Device R6.30).
- A simulated reading lands R6.8g ("used as the colour"), not the R6.8c "kept in history" the case expects.
- Fix: either declare a live-kind reading on TK-2 at that step, or keep a Demo Device scan and expect R6.8g's count. The latter is still an outcome change, so E13's collection variant still fires.

**[MINOR] R3-m5 — IJ:117 and IJ:150 — Import cases assert Collection Mode surfaces**
- IJ:117 asserts "each carrying the imported mark (Collection Mode R2.4j)".
- IJ:150 asserts "the item detail's Reading line shows model unknown".
- CM:211 and PL:146 put Collection Mode's imported row alongside Import §6, not before it, so an Import §6 PR that lands first cannot pass either case.
- Fix: assert the store condition instead (snapshot kind imported; model NULL), or mark those clauses "once Collection Mode's imported row lands", as IJ:152 does for R5.5.

**[MINOR] R3-m6 — DF:187 (R5.5d) and DFJ:124 — salvage when the imported reading was recorded before a simulated one**
- For an imported reading recorded before a simulated one, "the one R2.3j would leave current" reads two ways:
  - as a pairing rule it keeps the imported reading;
  - as R2.3j's arrival rule (R2.3j governs only an arriving imported reading) it falls to "otherwise the later-recorded" and keeps the simulated one, which R2.3b would also have made current.
- DJ6's salvage case covers only the order with the simulated reading first.
- Fix: state which applies, and add the reverse-order pair to DFJ:124.

**[MINOR] R3-m7 — CM:211 against CM:205, CM:449 (R8.9) and CM:575 (M2) — what the new Build-dependencies row leaves out**
- Row 211 gates R2.4j and the imported halves of R2.7, R4.2d, R5.2b and R5.2e, but not:
  - M2's population, which includes ZX-022 "every PR";
  - R8.9's shape for the imported mark.
- Row 205 still lists R2.1–R2.8 and R8.3–R8.11 whole, with ADR-0003 as the only stop.
- Fix: add M2's ZX-022 and R8.9's imported shape to row 211, and say that row 211's stops govern where the two rows overlap.

**[NIT] R3-n1 — IMP:195 (M2):** lists E5 among "record excluded", but E5 refuses the whole file. Move it beside E46 and E49.

**[NIT] R3-n2 — DVJ:80 (UJ3-c):** "imported from Import UJ 3's synthetic fixture T" reads as running the importer, which R6.10 now reserves for Import's cases and R7.7o. Say "declared as fixture T's reading (Import R6.10)".

#### Biggest risks (if executed as-is)
1. **Export goldens and DJ6 are cut from declared values, or go red because the importer isn't there yet** (R3-M1). The goldens are that release's contract, so a re-cut later is a contract change.
2. **Fixture T's file values come from an arbitrary library, or from the app itself** (R3-M2, R2-M3). R6.6's cases then either prove nothing or flake, depending on the method chosen.
3. **The commit-recheck order gets guessed** (R3-m1). A build that checks for a mode change first fails IJ:132.
4. **Import's and Data Foundation's cases disagree on NULL versus `''`** (R3-m3), and ADR-0003 inherits the ambiguity.

#### Plan strengths
- **R2-M4's fix is exact.** IJ:132 and IJ:133 separate a target that "no longer fits" from one that "changed but still fits", and E13's collection variant gives the second case a named state.
- **The history-order cases discriminate.** UJ2.1-s and UJ2.1-t together reject every wrong order I tried: the two swapped, measured order newest-first, and recorded order oldest-first.
- **Collection Mode is cleanly decoupled from Import §6.** R6.10's direct-seeding clause together with CM:211 does it.
- **Salvage now matches R2.3j.** R5.5d agrees with R2.3j for the three pairings DJ6 exercises.
- **Two new cases discriminate well.** The quarantine exemption with IJ:151, and IJ:152's restore-then-reimport case, which tells "R6.8f first" apart from R6.8d.
- **Suspected problems I checked and ruled out:**
  - IJ:136's counts under R3.6g;
  - IJ:139's partition;
  - IJ:144's plain-decimal equality, which holds under correctly rounded parsing;
  - UJ2.1-s's recorded order being P0 (CM:386);
  - all twelve Toolkit tokens being registered in Capture §12;
  - any "R7.7a–n" left anywhere (none).

#### Spec coverage gaps (requirements with no task)
- **G1 residual: the wait on Import §6 is stated nowhere.** No Data Foundation or Export row names Import §6 as a predecessor of R7.7o, DJ6 or R4.2's imported goldens. Neither PRD has a Build-dependencies table, and IMP:32 names only §6's upstream. The brief requires every dependency to be ordered; today the only statement of this one is PL:146, and PL:146 gets it wrong (R3-M1).

| Row ID | disposition |
|---|---|
| Import-R1.5 | ALIGN |
| Import-R3.2 | ALIGN |
| Import-R3.3 | ALIGN |
| Import-R3.5 | ALIGN |
| Import-R3.8 | OBJECT (R3-m1, R2-m4) |
| Import-R6.1 | OBJECT (R2-m4) |
| Import-R6.2 | ALIGN |
| Import-R6.3 | ALIGN |
| Import-R6.4 | OBJECT (R3-m1, R3-m2) |
| Import-R6.5 | OBJECT (R3-m3, R3-m5) |
| Import-R6.6 | OBJECT (R3-M2, R2-M3) |
| Import-R6.7 | ALIGN |
| Import-R6.8 | OBJECT (R3-m4, R3-m5) |
| Import-R6.9 | ALIGN |
| Import-R6.10 | OBJECT (R3-M1, R3-M2) |
| Import-R6.11 | OBJECT (R3-m2) |
| Import-M2 | ALIGN |
| Import-E5 | OBJECT (R2-m4) |
| Import-E7 | ALIGN |
| Import-E9 | ALIGN |
| Import-E10 | ALIGN |
| Import-E13 | OBJECT (R3-m1) |
| Import-E14 | ALIGN |
| Import-E42 | ALIGN |
| Import-E43 (Toolkit variant) | ALIGN |
| Import-E46 | OBJECT (R3-m1, R3-m2) |
| Import-E47 | ALIGN |
| Import-E48 | ALIGN |
| Import-E49 | OBJECT (R3-m2) |
| DF-R1.2 | ALIGN |
| DF-R1.6 | ALIGN |
| DF-R2.1 | ALIGN |
| DF-R2.2 | ALIGN |
| DF-R2.3 (with R2.3h, R2.3j) | OBJECT (R3-M1) |
| DF-R2.4 | ALIGN |
| DF-R5.5 (with R5.5d) | OBJECT (R3-m6) |
| DF-R7.1 | ALIGN |
| DF-R7.2 | ALIGN |
| DF-R7.7 (with R7.7o) | OBJECT (R3-M1) |
| Device-R1.21 | ALIGN |
| Device-R1.22 | ALIGN |
| CM-R2.4 (with R2.4j) | OBJECT (R3-m7) |
| CM-R2.7 | ALIGN |
| CM-R4.2 (with R4.2d) | ALIGN |
| CM-R5.2 (with R5.2b/e) | ALIGN |
| CM-R8.9 | OBJECT (R3-m7) |
| CM-M2 | OBJECT (R3-m7) |
| CM-E12 | ALIGN |
| Export-R1.1 (with R1.1h/i/p/s/t) | ALIGN |
| Export-R1.2 | ALIGN |
| Export-R4.2 | OBJECT (R3-M1) |
| Export-E1 | ALIGN |
| Capture-R1.10 | ALIGN |
| Capture-M8 | ALIGN |

### database

#### Verdict
The schema and queries are sound. No Blocker or Major is open. All round-2 findings are resolved except DB2-M1 and DB2-n1, which are partial. The fixes add three new Minors, each fixed by a wording change plus one assertion: DB3-m1 (salvage), DB3-m2 (the commit recheck) and DB3-m3 (NULL versus "empty").

#### Schema & engine (brief)
- **No DDL exists yet.** ADR-0003, the storage schema, is still queued (ADR:22), so the requirement rows are still the data contract. I traced them against the entities they imply:
  - **Item:** its code is unique per collection (Import R3.3).
  - **Reading:** a sequence, a measurement time and a record time; a reason; a current selection; a predecessor; a copied snapshot of kind live, simulated or imported, whose unknowns are NULL.
  - **Imported reading:** it also keeps the file's illuminant and observer, as text, as provenance.
  - **Samples, derived sets, collection:** samples (none for an imported reading); derived sets per reading × condition; the collection's scan mode.
- **Engine.** SQLite, with GRDB recommended; outside reads run at SQLITE_READER_FLOOR (DF OQ 2).
  - `PRAGMA foreign_keys`, the journal mode and `busy_timeout` are still decided nowhere.
  - New this round: ADR:22 says a schema constraint can enforce "at most one current reading per item".
- **What I read.** `git diff f7cbde5 7332d69 -- docs AGENTS.md` (read word by word), every changed row in full, UJ 3, DJ6, DF §2/§5/§7, the fences F64–F70 and F74–F88, my round-2 section, and the round-2 fix file.

Path legend (all absolute; "X:n" is line n of that file):
- IMP = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import.md
- IMPJ = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import-journeys.md
- IMPC = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import-copy.md
- IMPF = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/import/prd-inventory-import-fences.md
- DF = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/data-foundation/prd-data-foundation.md
- DFJ = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/data-foundation/prd-data-foundation-journeys.md
- DMJ = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/device-management/prd-device-management-journeys.md
- EX = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/export/prd-data-export.md
- EXJ = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/export/prd-data-export-journeys.md
- CM = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/collection-mode/prd-collection-mode.md
- CMJ = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/collection-mode/prd-collection-mode-journeys.md
- ADR = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/decisions/README.md
- PL = /Users/vinnypasceri/Projects/.worktrees/spectro-capture-nix-import/docs/product/post-lock.md

#### Findings

##### Round-2 findings — status
- **DB2-M1 (salvage of two current readings contradicts F67–F69) — PARTIAL.**
  - Landed: DF:187 now keeps "the one R2.3j would leave current", with the other kept behind and no question. DFJ:124 asserts the right current reading for all three pairs.
  - Missing: my fix's "reason initial" and a reason oracle. The row also never says which reading is B. → DB3-m1.
- **DB2-m1 (a quarantined reading blocks recovery by re-import) — RESOLVED.**
  - Where: IMP:182 ("a quarantined reading never counting"), IMPF:257, ADR:22 ("quarantined readings are exempt") and case 1 at IMPJ:151.
  - My case 2 (a second re-import) was not added. The rule is unambiguous without it, so that is acceptable. See DB3-n1.
- **DB2-m2 (R6.8 outcomes not rechecked at commit) — RESOLVED.**
  - Where: IMP:126 rechecks "each matched item's R6.8 outcome" and routes a change to E13's collection variant (IMPC:23). My case landed at IMPJ:134, and IMPJ:132–133 cover the mode side.
  - Residues: the recheck's scope → DB3-m2; the dropped "inside the transaction" → DB3-n3.
- **DB2-m3 (fixture range R7.7a–n) — RESOLVED.** R7.7a–o now appears at EX:145, EX:180, DF:281 and EXJ:3; a grep finds no remaining "R7.7a–n".
- **DB2-n1 (unknowns should be NULL) — PARTIAL.**
  - Fixed: DF:137, DFJ:113, IMP:170 and DMJ:80 now say absent (NULL).
  - Still "empty": IMPJ:118, ADR:22 and PL:172. → DB3-m3.
- **DB2-n2 (sub-millisecond Date Saved) — RESOLVED.** IMP:172: "truncated to the millisecond".
- **DB2-n3 (E48 overclaims the check) — RESOLVED.** IMPC:34: only the Lab is checked; the other values aren't kept.
- **DB2-n4 (ADR row editorial) — RESOLVED.** ADR:22 now reads "…the cold open. Input from the Nix Toolkit import…", so the Toolkit input stands apart from the eight Collection Mode inputs.
- **DB2-n5 (Toolkit copy misstates what is kept) — RESOLVED.** IMPC:29 reads "Kept in history, not used as the colour … with an instrument … a later Toolkit reading over an older one", and E14 matches (IMPC:24). Demo readings are no longer covered by "with an instrument".
- **Round-2 Missing: one current reading per item, enforced by the schema — RESOLVED at ADR:22.** PL:172 does not mirror it (DB3-m3).
- **Round-2 Missing: the same-reading key excludes quarantined readings — RESOLVED at ADR:22.** PL:172 is stale (DB3-m3).
- **Round-2 Missing: restore, then re-import — RESOLVED at IMPJ:152.** Deferring it until Collection Mode R5.5 lands is sound, because restore is that row's action.

##### Round-1 findings I left PARTIAL or UNRESOLVED
- **Salvage can promote an imported reading over a scan — RESOLVED.** DF:187; DFJ:124 keeps the live reading current with no question.
- **No fixture yields `sc_imported` true — RESOLVED** through DB2-m3 (EX:145).
- **Unknown serial/firmware should read back NULL — PARTIAL** through DB2-n1 → DB3-m3.
- **Residues:** DB1-B1's (DB2-m1) is RESOLVED, DB1-M1's (DB2-n2) is RESOLVED and DB1-M4's (DB2-m2) is RESOLVED.

##### New findings

[MINOR] DB3-m1 — DF R5.5d (DF:187) and DJ6's salvage case (DFJ:124): which reading plays B is unstated, and so are the reason and predecessor after salvage.
- **(a) Reversed pairs are unassigned.** All three pairs at DFJ:124 have the imported reading recorded later. Now take the reverse: an imported current reading, then a Demo or live scan recorded later that failed to clear it.
  - A build that reads R5.5d by kind — "imported beats simulated", "live beats imported" — keeps the imported reading current over the Demo scan. It also records no question over the live scan.
  - R2.3b, the operation that actually wrote the later reading (DFJ:122), leaves the scan current and correction-unconfirmed. A build that treats the later-recorded reading as B reaches R5.5d's "otherwise" branch and gets R2.3b's result.
  - The two builds pick different current readings, and both pass DFJ:124.
  - The same-dated imported pair also resolves only once B is fixed.
- **(b) "Kept behind" is misapplied to the simulated pair.** There the non-current reading is B's predecessor, not a reading R2.3j keeps behind.
  - Applying R2.3h's "a reading R2.3j keeps behind is no reading's predecessor" leaves the imported current reading's reason open: re-measurement per R2.3j and F67, or initial with no predecessor.
  - DFJ:124 asserts no reason, so either build passes. Yet the reason is floor-readable (DF R1.2) and exported (Export R1.3).
- **Fix.**
  - R5.5d: "of two current readings where the later-recorded is imported, the pair ends as R2.3j leaves it with that reading as B — current selection, reason and predecessor included, no question; otherwise the later-recorded stays current, the other retained as correction-unconfirmed".
  - DFJ:124 asserts the reasons: re-measurement with the simulated reading as predecessor; initial on each kept-behind imported reading.
  - DFJ:124 adds a reversed pair: imported recorded first, Demo recorded later → the Demo reading is current, correction-unconfirmed.

[MINOR] DB3-m2 — Import R3.8k (IMP:126): the recheck covers only items matched at preview.
- **The gap.** The row names "each matched item's R6.8 outcome", not each record's match.
- **Scenario.**
  1. Preview Fixture T into an existing M2 target that has no TK-3.
  2. Save a swatch TK-3 through Capture's Add a swatch, which is allowed with a preview open (CMJ:545).
  3. Choose Import.
- **How it bites.** TK-3 was R6.8a at preview and matched no item, so a literal recheck finds no changed outcome. A commit that follows the previewed plan then creates a second TK-3, against R3.3. That happens silently unless the schema enforces R3.3's unique code, which no ADR-0003 input requires.
- **Already covered elsewhere.** E13's collection body (IMPC:23, "a swatch this file matches, changed") already names this case. An item deleted mid-preview is also caught, because its own outcome changes.
- **Fix.**
  - R3.8k: "each record's match and R6.8 outcome".
  - Case: add TK-3 during the preview; Import → E13's collection variant; the fresh E43 counts TK-3 as matched, not New; the file holds one TK-3.

[MINOR] DB3-m3 — IMPJ:118, ADR:22 and PL:172: "empty" survives where the rows now say NULL.
- **Where.**
  - IMPJ:118 asserts "serial and firmware empty".
  - ADR:22 says "stored empty, never as a placeholder".
  - PL:172 says "stored empty". It also still lists round 1's inputs ("explicit current selection…", "the same-reading key excluding restores") and lacks round 2's one-current constraint, quarantined exemption and exact read-back.
- **The conflict.** IMP:170 says absent, and IMP:25 defines absent as distinct from an empty string. DF:137, DFJ:113 and DMJ:80 say NULL.
- **How it bites.** A test written from IMPJ:118 as `serial = ''` fails a correct build. A schema author working from ADR:22 may choose `NOT NULL DEFAULT ''`, which then fails DFJ:113 and DMJ:80. Either failure is loud, and the cited-row precedence rule points to NULL, so this is Minor.
- **Fix.**
  - IMPJ:118: "serial and firmware absent (NULL)".
  - ADR:22: "absent (NULL), never '' or a placeholder".
  - PL:172: repeat ADR:22's round-2 input list, or reduce the item to a pointer to that row.

[NIT] DB3-n1 — IMPJ:151: the case asserts the new current reading and "the quarantined one kept", but not that exactly one TK-2 reading is current. A build that leaves the quarantined reading marked current passes the case, and the file then opens read-only (DF R2.2). With ADR:22's constraint the commit fails to E44 instead. Add "exactly one TK-2 reading is current, the new one".

[NIT] DB3-n2 — ADR:22's uniqueness clause reads as if the constraint enforces R6.8f. It cannot: R6.8f's check counts a restore (IMPJ:152), and the constraint exempts restores.
- **Where it bites.** An item's only non-quarantined copy of a reading is a restore whose source was later quarantined (DF R5.5c). A build that relies on the constraint alone stores the reading again.
- **Fix.** Say the constraint is a backstop, and that R6.8f's check still counts a restore.

[NIT] DB3-n3 — IMP:126 dropped round 2's "inside the commit's transaction". DF R1.11 (DF:106) closes the check-then-write window only if the recheck is part of the commit's write hold. Say "within the commit's write hold (DF R1.11)". In SQLite, that means the recheck runs in the same `BEGIN IMMEDIATE` transaction as the writes.

[NIT] DB3-n4 — IMP:169 lists E11 among the exclusions that run before the mixed-mode check. But E11 needs a target (R3.4), and the mixed-mode refusal fires when the file is read (IMPJ:127). A record that matches several items and carries a different mode is excluded under the row's order but refused file-wide at read. Say E11 runs once a target is chosen, or drop it from that list.

#### Biggest risks
1. **DB3-m3.** An ADR-0003 author follows ADR:22's "stored empty", and an empty-string default ships in a one-way-door schema. Only DFJ:113 and DMJ:80, if they are built before the schema freezes, would catch it.
2. **DB3-m2, compounded by undecided FK enforcement.** A plan fixed at preview, run against a target that changed, writes a duplicate code. It does so silently, because nothing enforces unique codes and SQLite's default `foreign_keys = OFF` also lets orphaned writes through.
3. **DB3-m1.** Two builds salvage a reversed pair to different current readings, or store different reasons, and the salvage case passes both.
4. **Toolkit format drift (unchanged, tracked by OQ 3).** A different digit count turns one measurement into a same-dated "different" reading. E43 lists it, so it is not silent.

#### Genuinely sound
- **One current reading per item as a schema constraint (ADR:22).** It fails closed, as E44 rather than a read-only file.
  - Either shape works: a partial unique index on the current flag (SQLite ≥ 3.8.0, so the floor is barely affected) or a current-reading pointer on the item.
  - The two-current state DF R7.2 declares must then be written without the constraint, and R2.2's open-time check must query for the violation rather than rely on the constraint.
- **The quarantined exemption.** It is exactly what lets IMPJ:151's item hold the quarantined reading and its fresh copy under one key.
- **"Determinable by plain SQL at the floor" instead of "stored explicitly".** It states the WHAT without dictating a column, and still forbids inferring from sequence.
- **UJ2.1-t paired with UJ2.1-s (CMJ:237–238).** Together they catch a build that sorts the measured variant by record time ascending, which UJ2.1-s alone passes.
- **Direct seeding (IMP:175, CM:211, PL:146).**
  - DF R7.7o still runs the importer, so the stored shape the seeds copy is proven once.
  - EXJ:14's `sc_imported` golden sits on R7.7o.
- **Right calls a textbook DBA would wrongly flag:**
  - The export labels a kept-behind reading `superseded` (EX:94). The state column is a closed set (EX:32), so a new value would change its meaning, and the reason `initial` tells a kept-behind reading apart.
  - The file's illuminant and observer are stored "as their text" (IMP:170). This is provenance in the vendor's vocabulary, so a foreign key to the pairs the app offers would reject real files.
  - Truncating to the millisecond (IMP:172) matches millisecond sameness, and IMPJ:144's `+00:00` case pins comparison by instant.
  - IMPJ:150 pins a negative reflectance stored as a number.
  - An R6.8f record still takes metadata updates (IMP:182).

#### Missing / over-engineered
- **Still no ADR-0003 input for engine settings:** `PRAGMA foreign_keys = ON` on every connection, the journal mode and `busy_timeout`.
- **No ADR-0003 input asks for R3.3's unique code** (per collection, on the normalized R2.3 key) as a schema backstop. That backstop is what makes DB3-m2 fail closed.
- **DB2-m1's second case is not added.** That is acceptable, per the status line above.
- **Nothing is over-engineered.**

| Row ID | disposition |
| :--- | :--- |
| Import-R1.5 | ALIGN |
| Import-R3.2 | ALIGN |
| Import-R3.3 | ALIGN |
| Import-R3.5 | ALIGN |
| Import-R3.8 | OBJECT (DB3-m2) |
| Import-R6.1 | ALIGN |
| Import-R6.2 | ALIGN |
| Import-R6.3 | ALIGN |
| Import-R6.4 | ALIGN |
| Import-R6.5 | OBJECT (DB3-m3) |
| Import-R6.6 | ALIGN |
| Import-R6.7 | ALIGN |
| Import-R6.8 | ALIGN |
| Import-R6.9 | ALIGN |
| Import-R6.10 | ALIGN |
| Import-R6.11 | ALIGN |
| Import-M2 | ABSTAIN |
| Import-E5 | ABSTAIN |
| Import-E7 | ABSTAIN |
| Import-E9 | ABSTAIN |
| Import-E10 | ABSTAIN |
| Import-E13 | ALIGN |
| Import-E14 | ALIGN |
| Import-E42 | ABSTAIN |
| Import-E43 (Toolkit variant) | ALIGN |
| Import-E46 | ALIGN |
| Import-E47 | ALIGN |
| Import-E48 | ALIGN |
| Import-E49 | ABSTAIN |
| DF-R1.2 | ALIGN |
| DF-R1.6 | ALIGN |
| DF-R2.1 | ALIGN |
| DF-R2.2 | ALIGN |
| DF-R2.3 (with R2.3h, R2.3j) | ALIGN |
| DF-R2.4 | ALIGN |
| DF-R5.5 (with R5.5d) | OBJECT (DB3-m1) |
| DF-R7.1 | ALIGN |
| DF-R7.2 | ALIGN |
| DF-R7.7 (with R7.7o) | ALIGN |
| Device-R1.21 | ALIGN |
| Device-R1.22 | ALIGN |
| CM-R2.4 (with R2.4j) | ALIGN |
| CM-R2.7 | ALIGN |
| CM-R4.2 (with R4.2d) | ALIGN |
| CM-R5.2 (with R5.2b/e) | ALIGN |
| CM-R8.9 | ABSTAIN |
| CM-M2 | ABSTAIN |
| CM-E12 | ALIGN |
| Export-R1.1 (with R1.1h/i/p/s/t) | ALIGN |
| Export-R1.2 | ALIGN |
| Export-R4.2 | ALIGN |
| Export-E1 | ALIGN |
| Capture-R1.10 | ALIGN |
| Capture-M8 | ABSTAIN |
