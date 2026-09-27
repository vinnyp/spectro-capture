# Inventory Import PRD — fences

Owner decisions for `prd-inventory-import.md`. Existing owner decisions remain binding except where the later individual decisions F51–F91 explicitly amend them; F50 records structural authorization.

**Nix Toolkit amendment review log:** [`docs/agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md`](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md) — its Round 0 records the owner decisions N1–N15 that F69–F83 carry, and its Round 1 the decisions N16–N23 that F84–F91 carry.

**Amendment lock record:** [the Nix Toolkit import review log's lock record](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#lock-record-2026-09-26) — amendment F69–F91, peer review closed 2026-09-26 after eight rounds; checks 1–20 at the validated revision it names.

F4 and F11 were copied under F49; their canonical text and original dates remain in the capture fence file. Import-local decisions start at F50; IDs are scoped to their document.

F49 itself — the split that made this document, and its clarification (1) — stays in the [capture fence file](../capture-mode/prd-capture-mode-fences.md) and is cited from here rather than restated. So do F24, the priority split, and F45, the two-sentence rule, both of which bind every row in this PRD.

## Fences

### F4 — Import target is chosen in the app (2026-09-06)

**Decision:** The target collection is picked or created in the app before columns are mapped. Collection name (and, per F1, library name) is not a CSV mapping target in v1. One CSV feeds one collection. Import ends at "collection ready to capture"; starting the session is a separate step. The bootstrap's UJ2 / UJ2.1 / UJ2.2 shape becomes UJ2 (new collection) plus UJ2.1 (idempotent re-import into an existing collection), with UJ2.2 as the shared column-mapping journey.

**Why:** A CSV is normally one swatch book. A collection-name column forces every file to carry a constant column and implies a multi-collection file that is not the primary case. Import-today-scan-tomorrow requires the queue to survive between import and session.

### F11 — Swatch Code normalisation (2026-09-06, round 1, R1-F9)

**Decision:** Wherever a Swatch Code or a collection name is compared (uniqueness, re-import match, find, ad-hoc duplicate), the comparison trims leading and trailing whitespace, collapses internal whitespace runs to one space, and is case-insensitive. The display value is preserved as entered. The normative rule now lives in Inventory Import R2.3 (moved under F49); all consumers cite it.

**Why:** A re-export from Numbers or an Excel autocorrect must not double the queue on an idempotent re-import. The owner chose the forgiving rule over the research default (case-sensitive) because spreadsheet drift is the common case for this persona.

### F50 — Agent-build amendment (2026-09-17)

**Authorization:** The owner approved the eight-item Inventory Import audit proposal in this session (“proceed with your recommended changes”). The first revision included agent-selected behavior; the review identified choices needing individual ratification, now recorded in F51–F61 from the [owner’s 2026-09-17 decisions](https://github.com/vinnyp/spectro-capture/pull/16#issuecomment-5723537817).

**Decision:** Keep stable IDs and the PRD and four companions; move split history here, state v1/P0 once, retain implementation status, and express journeys as acceptance tables. Requirement rows remain normative; no architecture, module layout, or completed dogfood evidence is claimed. F51–F61 supersede the first revision’s behavioral proposals, including exact-name column reuse, blocking duplicate headers, and the Unicode data-version pin.

**Why:** Builders need deterministic requirements and testable outcomes; repeated narrative and process history obscure that contract.

### F51 — Blank-to-blank is unchanged (2026-09-17)

**Decision:** A supplied blank over a stored blank counts as unchanged; this replaces the locked R3.6 wording.

**Why:** A no-op must not inflate the preview’s updated count.

### F52 — Preview confirms the target (2026-09-17)

**Decision:** Selecting an existing collection adds no confirmation dialog; the preview names the target collection and existing item count as a first-class line.

**Why:** The final review identifies the destination where the user commits, without an extra dialog.

### F53 — Header drift reuses the stored column (2026-09-17)

**Decision:** A passthrough header equal under R2.3 reuses the stored column, keeping its first-seen spelling and position; append only unmatched columns.

**Why:** Spreadsheet case and spacing drift must not multiply stored columns.

### F54 — Keep preserves conflicts only (2026-09-17)

**Decision:** A captured row choosing “Keep what I have” retains previously present fields, including blanks, but receives incoming values for previously absent fields. Column addition alone never counts a row as updated; list added columns separately.

**Why:** Keep protects existing choices while allowing new metadata to arrive; row counts must not conflate a column addition with an existing-value change.

### F55 — Duplicate headers are a notice (2026-09-17)

**Decision:** Disambiguate later named-header collisions under R2.3 with the same suffix mechanism used for blank headers; show the results in E12/E41 with “Continue with the listed names” and “Pick the file again”. Keep literal generated-name templates beside the copy.

**Why:** The user can import all columns without editing a spreadsheet first, while seeing exactly which names will be stored.

### F56 — Full default folding without locale tailoring (2026-09-17)

**Decision:** Retain canonical caseless matching with full default case folding and no locale tailoring: Straße equals STRASSE, while İ differs from i and ı differs from I; the nine-pair journey table specifies the observable cases.

**Why:** One locale-independent rule keeps import, uniqueness, find and duplicate checks consistent across machines.

### F57 — Comparison data version belongs to ADR-0003 (2026-09-17)

**Decision:** Remove the Unicode version pin from R2.3; ADR-0003 chooses comparison data/version governance, and a table upgrade requires re-checking existing identifiers. Add this to the decision queue, including implementation evidence against the macOS floor when ADR-0006 selects it.

**Why:** Product behavior must stay stable without pretending the PRD has selected an unverified dependency or OS floor.

### F58 — Guard single-column reads (2026-09-17)

**Decision:** Always show encoding and delimiter in preview; require explicit confirmation and offer the delimiter control for a one-column parse before mapping is saved or reused. The four encodings and three delimiters are provisional under OQ 1, the guard remains until that question closes, and E5 offers a UTF-8 re-save path when all encodings fail.

**Why:** A valid one-column parse can still be the wrong interpretation of a spreadsheet export; expose that ambiguity before remembering the mapping.

### F59 — Mirror storage obligations (2026-09-17)

**Decision:** Decoded values stay directly queryable as a Data Foundation obligation; mirror field/column preservation and re-import measurement preservation into its inbound table, with Rows columns on both sides. Record the cross-document amendment in the post-lock list.

**Why:** Agents implementing the store must receive the same preservation contract as agents implementing import.

### F60 — Changed source resets choices (2026-09-17)

**Decision:** On E13 re-read, reset overwrite choices and require a new preview, revalidating mapping first.

**Why:** Earlier overwrite approvals describe the old file and cannot authorize changed input.

### F61 — Accept remaining flow and open-question changes (2026-09-17)

**Decision:** Keep OQ 1 (detection) separate from OQ 2 (named templates); offer Cancel on every import state and use “Continue without them” at E7/E9/E10. All-unchanged imports may commit, all-excluded imports may not; source record numbering includes headers/preambles, starts at data record 1 for headerless input, and counts a quoted multiline record once.

**Why:** Open questions need separate evidence and owners; action labels must distinguish continuing from committing, and source numbering must remain usable when locating an issue.

### F62 — Preview absent-field fills separately (2026-09-17)

**Decision:** A matched row gaining a value in a field it never had remains unchanged in the row counts unless a previously present value changes, whether the column is new or already exists. E43 names the number of rows gaining details on a first-class line; split acceptance cases assert counts, the line, and stored values for both column cases.

**Why:** The preview must disclose these writes without changing F54’s separate treatment of row updates and column additions. Source: [round-2 owner decisions](https://github.com/vinnyp/spectro-capture/pull/16#issuecomment-5723905420).

### F63 — Preview follows shared zero-count copy rules (2026-09-17)

**Decision:** E43 omits zero-count sentences and empty list lines rather than displaying zero or “None”; Capture §12 owns the rule. Extend its placeholder index with Import’s preview and generated-name tokens, including the filled-row count, and record that editorial extension beside Capture F11.

**Why:** Import and its siblings must render counts consistently from one copy contract. Source: [round-2 owner decisions](https://github.com/vinnyp/spectro-capture/pull/16#issuecomment-5723905420).

### F64 — Separately created target persists (2026-09-17)

**Decision:** A collection created through Capture §1 persists empty when its import is canceled or fails under R3.2’s storage guarantee; creation is a separate action. R3.7 owns this lifecycle, with import eligibility separated into R3.9.

**Why:** Canceling import does not undo a separately completed collection creation. Accepted explicitly in the [round-2 owner decisions](https://github.com/vinnyp/spectro-capture/pull/16#issuecomment-5723905420).

### F65 — A Collection Mode rename moves the stored column name (2026-09-24)

**Decision:** Mirroring [the Collection Mode PRD's F9](../collection-mode/prd-collection-mode-fences.md) (owner decision D9, post-fill adjudication 2026-09-24): renaming an imported column in Collection Mode changes its stored name, keeping its position and values, and a later import matches the stored name; a source header equal only to the old name appends as a new column under R2.6's existing rule. Narrows F53's "first-seen spelling" to the stored spelling, first-seen unless renamed. No other import rule changes; R2.6 keeps its alignment.

**Why:** Collection Mode's F9 closes the post-lock question of whether a rename moves the stored name, and both sides of that seam land in the same change. Source: the Collection Mode PRD's F9; peer review pending.

**Closed 2026-09-26 ([final review](https://github.com/vinnyp/spectro-capture/pull/21#pullrequestreview-5328170977)):** For F65–F68, peer review closed 2026-09-25 (PR #21), with the 2026-09-26 editorial Clarified line under F68; re-locked on merge.

### F66 — An import commit waits while any capture session is in flight (2026-09-25)

**Decision:** Mirroring [the Collection Mode PRD's F149](../collection-mode/prd-collection-mode-fences.md) (its approved round-3 recommendation 15, 2026-09-25): an import commit also waits while any capture session is running — active, paused or halted — on any collection, not only the target. R3.2 refuses the commit with E40, whose another-collection variant names the collection holding the session and offers the same actions; an interrupted session on another collection holds no writer and blocks nothing. R3.2 keeps its alignment; E40 gains the variant.

**Not decided:** whether an import commit also waits while a Collection Mode bulk write or delete runs; the Collection Mode PRD's F137 holds only sessions and that PRD's own writes, so it goes back to the owner.

**Why:** the file has one writer, and a commit landing during a session would hold up its saves, as the Collection Mode PRD's F100 and F137 rule for that PRD's bulk writes. Source: the Collection Mode PRD's F149; peer review pending.

**Clarified 2026-09-25 ([the Collection Mode PRD's F152](../collection-mode/prd-collection-mode-fences.md), owner decision D33):** the **Not decided** point above is settled: while a Collection Mode bulk write or delete runs, no other write anywhere in the app starts, so R3.2's commit shows disabled until that write lands, citing Collection Mode's R8.1f, and a UJ 2.1 case asserts it. R3.2 keeps its alignment. Peer review pending.

**Clarified 2026-09-25 ([the Collection Mode PRD's F155](../collection-mode/prd-collection-mode-fences.md), owner decision D36, F67):** the **Not decided** point's settlement now cites [Data Foundation R1.11](../data-foundation/prd-data-foundation.md#1-the-file-the-user-owns), the one-writer rule, in place of Collection Mode's R8.1f. Peer review pending.

### F67 — The import commit follows Data Foundation's one-writer rule both ways; E40's another-collection variant reworded (2026-09-25)

**Decision:** Mirroring [the Collection Mode PRD's F155, F156 and F169](../collection-mode/prd-collection-mode-fences.md) (owner decisions D36 and D37 and its approved round-4 recommendation 9, round-4 adjudication 2026-09-25): R3.2's commit follows [Data Foundation R1.11](../data-foundation/prd-data-foundation.md#1-the-file-the-user-owns)'s one-writer rule both ways — shown disabled while another write it names runs, and, while it commits, holding every other write to the file and a session's start or resume — and R3.8i's "End that session" is shown disabled while an R1.11 write runs. E40's another-collection variant drops "waits": a session in ⟨collection⟩ is active, paused or halted, and until it ends imports aren't available in any collection; its shared append asks the user to start the import again. E40 moves to ⌛️ Ready for Alignment; R3.2 and R3.8i keep their alignment, and R3.2 stays at two sentences. UJ 2.1 asserts the commit held behind a Collection Mode bulk write or delete or a Data Foundation move, and a capture start, Collection Mode's Set a field and the Data Foundation re-read held behind this import's commit, each held through Collection Mode R8.10a's held-write input.

**Why:** the file has one writer, and the owner moved that rule's home to Data Foundation so each writer cites it rather than restating it. Source: the Collection Mode PRD's F155, F156 and F169; peer review pending.

**Clarified 2026-09-26 ([the Nix Toolkit import review's round 6](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-6--delta-verification-2026-09-26); Import F76's round-5 line):** R3.8i's and R3.8l's kept E14 choices apply to every import, plain CSV included; UJ 2.1's E40 and E44 cases assert them, and a source changed before "Try again" resets them through E13.

**Clarified 2026-09-26 ([round 7](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-7--delta-verification-2026-09-26); editorial within this fence):** UJ 2.1's E40 and E44 kept-choice cases declare a captured match whose name the file changes and meet E40 at commit, and the source-change case reaches the fresh preview through "Review again".

### F68 — The Collection Mode line names the matching rule, a commit's progress is listed, and UJ 2.1 holds a P0 write (2026-09-25)

**Decision:** Mirroring [the Collection Mode PRD's F188](../collection-mode/prd-collection-mode-fences.md) (its approved round-5 recommendation 11, round-5 adjudication 2026-09-25), with the testability halves its round-5 fix pass carries under its F155, F156 and F164: the outbound Collection Mode line names R2.3's one matching rule, which Collection Mode's rename, search and code change apply, beside R2.6; this document, whose format has no inbound-obligations table, carries what Collection Mode imposes on it by its rows' cites and the dated fences F65–F68, no table added. R4.1 lists a commit's progress, which Data Foundation R1.11 makes show while a close, switch or quit waits on it. UJ 2.1 holds Collection Mode's Rename collection, a P0 write, behind this import's commit in place of its P1 Set a field, and asserts R3.8i's End that session shown disabled while a Data Foundation R1.11 write runs. R4.1 keeps its alignment.

**Why:** the lock checks pair each seam both ways, and a first-phase build offers no Set a field. Source: the Collection Mode PRD's F188; peer review pending.

**Clarified 2026-09-25 ([the Collection Mode PRD's F188](../collection-mode/prd-collection-mode-fences.md), its round-6 fix pass, editorial):** the Collection Mode obligation line's cite reads "Collection Mode R1.3, R3.1 and R4.4" as one label, so R3.1 no longer reads as this document's; UJ 2.1's held-write lines name a Collection Mode R8.1f/g write, as Data Foundation R1.11 does. No rule changes; peer review pending.

**Clarified 2026-09-26 ([the Collection Mode PRD's F206](../collection-mode/prd-collection-mode-fences.md), owner decision D63, its re-lock checks, editorial):** the Collection Mode obligation line also names R2.5's collision order and the copy file's Collision template, which Collection Mode's R2.1 tagged-label collision form takes, so the seam its inbound line names (Collection Mode PRD, Inherited obligations) is stated on both sides. No rule of this document changes.

### F69 — v1 imports a Nix Toolkit export with its readings (2026-09-26)

**Decision:** A Nix Toolkit collection export — the vendor mobile app's CSV — imports in v1 with its readings, pulling [vision J7](../vision.md#j7-migrating-in-from-the-vendor-apps-cataloger-v2-candidate) forward for this one format. CxF import, and migration from any other vendor app, stay v2. §6 carries it; line 5 names the exception to "only new items become pending".

**Why:** the research on a real export showed the mobile app does export, with a full spectrum that reproduces its own Lab to well under the derivation tolerance. Source: [owner decision N1](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-0--owner-adjudication-2026-09-26); peer review pending.

### F70 — A Toolkit reading becomes the item's canonical value, marked imported (2026-09-26)

**Decision:** Each Toolkit record's reading becomes its item's canonical value, marked imported, with its provenance — the Toolkit, the device model, its date, its illuminant and observer, its measurement mode — and every derived value worked out from its spectrum. A later SpectroCapture scan supersedes it into version history as any re-scan does. R6.5 and R6.8 carry it.

**Why:** an imported collection is only worth importing if its colours arrive usable. Source: [owner decision N2](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-0--owner-adjudication-2026-09-26); peer review pending.

### F71 — Toolkit test fixtures are synthetic (2026-09-26)

**Decision:** The real export the research used stays out of the repository; every Toolkit test runs on a synthetic export built to the format the research records. R6.10 and UJ 3's fixture carry it.

**Why:** the owner's own collection is private research input, not a public fixture. Source: [owner decision N3](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-0--owner-adjudication-2026-09-26); peer review pending.


**Clarified 2026-09-26 ([round 1](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-1--nine-lenses-2026-09-26); PRIV-1, PRIV-4):** Synthetic means every collection name, code, name, note, date and value is invented, none taken from a real export, as the Collection Mode harness uses the word. The format a fixture follows is UJ 3's stated one, the file's own values come from an independent reference, and every checked-in fixture or golden holding an imported reading derives from such a fixture; no report on a real export enters the repository either. R6.10 carries it.

**Clarified 2026-09-26 ([round 2](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-2--delta-verification-2026-09-26); PRIV2-1, plan R2-M1, test TR2-m1):** "A report on one" means any data value from a real export; M2 and OQ 3 record only format facts and tallies by state across files. Another PRD's case may declare an imported reading directly, only Import's cases and the Data Foundation PRD's R7.7o running the importer. The file's own values come from the reference the Data Foundation PRD's R7.5 checks the build against, and UJ 3's offsets are multiples of DERIVATION_TOLERANCE. R6.10, M2 and OQ 3 carry it.

**Clarified 2026-09-26 ([round 3](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-3--delta-verification-2026-09-26)):** Fixture T's own values come from an independent implementation of CIE 15's calculation with ASTM E308's 10 nm tables, named in its provenance, and the Data Foundation PRD's R7.5 holds Fixture T's spectra so its bound covers them; R6.6's offset cases set the file's Lab from the build's; DJ6 is R7.7o's journey and runs the importer; from a single real file only format facts and whether it imported cleanly are recorded. R6.10, M2 and OQ 3 carry it.

**Clarified 2026-09-26 ([round 4](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-4--delta-verification-2026-09-26)):** Fixture T's own values use the CIE 15 tabulation the build's derivation uses — its ASTM E308 table and its 400–700 nm handling, named in the engineering plan — and the Data Foundation PRD's R7.5 reference, once named, governs, Fixture T regenerated to agree with it and the build never loosened. R6.10 and UJ 3 carry it.

**Clarified 2026-09-26 ([round 5](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-5--delta-verification-2026-09-26)):** R6.10 names the ASTM E308 weighting table, as UJ 3 does; Fixture T is regenerated by an independent implementation of the named reference's method, never from the build; UJ 3's sRGB and HEX sentence stands alone. OQ 3's revise list adds the device PRD's R1.21 and its Feeds column matches that list. R6.10, OQ 3 and UJ 3 carry it.

### F72 — The Toolkit import lives in this PRD (2026-09-26)

**Decision:** The Toolkit import extends this PRD's flow — pick the file, map, preview, commit — as its §6, and each sibling PRD it touches (Data Foundation, Device Management, Collection Mode, Data Export, Capture Mode, the vision) carries a dated mirror.

**Why:** the user meets it as the import they already know. Source: [owner decision N4](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-0--owner-adjudication-2026-09-26); peer review pending.

### F73 — A Toolkit export is recognised by its headers and confirmed in the preview (2026-09-26)

**Decision:** A Toolkit export is recognised by its header signature, read with its semicolon delimiter, its duplicate L header and `undefined` notes handled without prompts, and the preview names it and its reading count before the user commits. R6.1, R6.2, R6.7, R6.9 and R1.5b carry it.

**Why:** the Cataloger should not have to know the format's quirks. Source: [owner decision N5](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-0--owner-adjudication-2026-09-26); peer review pending.


**Clarified 2026-09-26 ([round 1](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-1--nine-lenses-2026-09-26); N5's "the header signature identifies it" and "'undefined' notes handled automatically"):** The signature is sought under the semicolon, then the comma, then the tab, so a Toolkit export re-saved with another separator is still recognised; its encoding and separator are shown fixed. A Note of `undefined` supplies no value, so it never clears a note the user typed. Every Toolkit column's fate is stated, Custom Collection Name read and not stored; a time, a mode and a number have stated forms, and a value that is not a finite number cannot be read. R6.1, R6.2, R6.7 and E48 carry it.

**Clarified 2026-09-26 ([round 2](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-2--delta-verification-2026-09-26)):** A recognised Toolkit export whose records fail R1.5c's quoting reaches E5's Toolkit variant, which offers no read control; E5, E7, E9, E10 and E42 carry written-out Toolkit variants; a time is an RFC 3339 date-time truncated to the millisecond; §1–§3's exclusions and R6.7's run before R6.4's and R6.11's checks; E48 says only the Lab is checked. R6.1, R6.4, R6.7, R6.11; E5, E7, E9, E10, E42 and E48 carry it.

**Clarified 2026-09-26 ([round 3](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-3--delta-verification-2026-09-26)):** Recognition reads the first record under whatever encoding the file is read with, and a Toolkit export not decoded as UTF-8 throughout reaches E5's Toolkit encoding variant; E5's quoting variant names the record and asks for the quote mark to be removed in the Toolkit; the mixed-mode and one-collection checks run at read after E7, E9, E10 and E47, with E11 applying once a target is chosen. R6.1, R6.4 and E5 carry it.

**Clarified 2026-09-26 ([round 4](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-4--delta-verification-2026-09-26)):** E5's two Toolkit variants are named the encoding and the quotes variant, the encoding one checked first, the quotes one naming the row by ⟨record⟩; E46's mixed-modes variant lists the colours under each mode. R6.1, R6.4, E5 and E46 carry it.

**Clarified 2026-09-26 ([round 5](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-5--delta-verification-2026-09-26)):** E5's Toolkit encoding variant states the rule, that SpectroCapture reads a Nix Toolkit export only as UTF-8 text, so it holds after a manual encoding choice too; UJ 3 pins the quotes variant's stray quote mid-value and adds a case where the encoding variant wins over the quotes one. E5 and UJ 3 carry it.

### F74 — A SpectroCapture scan stays current over a Toolkit reading (2026-09-26)

**Decision:** Where the matched item's current value was scanned in SpectroCapture, the scan stays current and the Toolkit reading is kept in version history as an earlier reading; metadata updates as a normal re-import. R6.8c carries it.

**Why:** a measurement taken in this app, under its own agreement check, is the better canonical value. Source: [owner decision N6](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-0--owner-adjudication-2026-09-26); peer review pending.

### F75 — A Toolkit reading carries an imported mark (2026-09-26)

**Decision:** A Toolkit reading carries an imported honesty mark in the collection table, like the simulated one, with the device and date in the item detail and history; it clears when the item is re-scanned, the imported reading keeping its mark in history. The Collection Mode PRD's R2.4j carries the mark; this PRD's obligation line names it.

**Why:** the user should always see which colours came from somewhere else. Source: [owner decision N7](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-0--owner-adjudication-2026-09-26); peer review pending.

### F76 — The file's measurement mode must fit the target (2026-09-26)

**Decision:** A new target adopts the file's measurement mode as its chosen scan mode; an existing target must already use it, or hold no reading and then switch; otherwise the import is refused with the reason. A file mixing modes is refused. R6.4, E46 and R3.8p carry it.

**Why:** readings taken in different measurement modes are not comparable. Source: [owner decision N8](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-0--owner-adjudication-2026-09-26); peer review pending.


**Clarified 2026-09-26 ([round 1](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-1--nine-lenses-2026-09-26); N8's "A new collection adopts the file's mode; an existing collection must already use it (or have no readings yet, and then switches)"):** The switch is written by the import's commit and undone with it, so Cancel or a failed commit leaves the target's mode unchanged; E43 names it; the fit is checked again at commit; "no readings" means no item holds any reading, current or in history, QC records aside; a Toolkit-created target's mode is fixed in that creation; R6.7's exclusions run before the mixed-mode check. R3.2, R3.8k, R6.3, R6.4 and the Capture Mode obligation line carry it.

**Clarified 2026-09-26 ([round 2](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-2--delta-verification-2026-09-26); F76's "E43 names it"):** At commit, a target whose mode or matched outcomes changed since the preview returns to a fresh preview through E13's collection variant, nothing written, and one that no longer fits refuses with E46. A Toolkit-created target's form labels its mode as set by the export. R3.8k, R6.3 and E13 carry it.

**Clarified 2026-09-26 ([round 3](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-3--delta-verification-2026-09-26)):** The commit recheck runs within the commit's write hold, covers every eligible record's match and outcome and every count and list E43's Toolkit lines showed, and routes in order: E13 for a changed source, E40, E46, then E13's collection variant. R3.8k carries it; the Capture Mode obligation line names the creation form's label.

**Clarified 2026-09-26 ([round 4](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-4--delta-verification-2026-09-26); this fence's round-2 line):** R3.8k reads the source before the write hold and routes any changed match or R6.8 outcome, as well as a changed mode, count or list, to E13's collection variant, which keeps each still-offered row's E14 choice (R3.8f). R3.8f and R3.8k carry it.

**Clarified 2026-09-26 ([round 5](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-5--delta-verification-2026-09-26); this fence's round-2 line):** R3.8k's collection-variant route applies to a Toolkit export only, as F76 does and as the post-lock plain-CSV item leaves open; its recheck names what it compares — the mode fit, every eligible record's match and R6.8 outcome, and E43's Toolkit lines and Swatch details counts. A newly offered row takes E14's default (R3.8f), and R3.8i and R3.8l keep each still-offered row's choice as R3.8f does, R3.8l unless the source changed; E13's collection variant reads "keeping any choices you made". UJ 3 adds P0 cases deleting an unmatched and a matched swatch mid-preview. R3.8f, R3.8i, R3.8k, R3.8l, E13 and UJ 3 carry it.

**Clarified 2026-09-26 ([round 6](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-6--delta-verification-2026-09-26); this fence's round-2 line):** R3.8k's Toolkit recheck compares what the commit would write — every eligible record's match, R6.8 outcome and R3.6 field changes, and E43's Toolkit lines, Swatch details counts, added columns and rows gaining details — against what E43 and E14 show, so a choice changed in E14 alone raises no E13. E13's collection body names the collection's columns and says a swatch newly listed with different details starts at taking the new details. UJ 3 adds cases for a choice change alone, a mid-preview rename that changes the counts, one that leaves them equal, and a column rename. R3.8k, E13 and UJ 3 carry it.

**Clarified 2026-09-26 ([round 7](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-7--delta-verification-2026-09-26); this fence's round-2 line):** R3.8k's recheck names each record's matched item and the stored value each R3.6 field change replaces, compares against the preview E43 and E14 last showed, its per-record plan and current choices included, and has a Toolkit commit write what that recheck worked out. E13's collection body is reworded to say each swatch keeps its choice. UJ 3's choice-only case asserts the commit, and its column-rename case changes only the added columns. R3.8k, E13 and UJ 3 carry it.

### F77 — What a Toolkit export lacks is recorded as unknown (2026-09-26)

**Decision:** An imported reading's sample count is not recorded, its device snapshot names the model with serial and firmware unknown, and it has no raw payload, one never supplied, a state distinct from a damaged archive; nothing is invented. The spectrum is kept and every derived value worked out from it. R6.5 carries it.

**Why:** a value the file never gave must not appear to have been measured. Source: [owner decision N9](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-0--owner-adjudication-2026-09-26); peer review pending.

### F78 — Date Saved is the reading's measurement time (2026-09-26)

**Decision:** An imported reading's measurement time is the file's Date Saved and its record time the import commit. R6.5 carries it.

**Why:** history and over-time views should place it when it was measured. Source: [owner decision N10](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-0--owner-adjudication-2026-09-26); peer review pending.

### F79 — Export marks an imported reading; QC treats it as any other (2026-09-26)

**Decision:** The CSV export gains an imported-provenance column beside the simulated one, and a QC ΔE comparison against an imported canonical value works as against any other, the imported mark showing. The Data Export PRD's mirror carries the column; the QC & Comparison PRD, not yet written, inherits the rest through post-lock.

**Why:** provenance should survive export, and an imported value is a legitimate baseline. Source: [owner decision N11](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-0--owner-adjudication-2026-09-26); peer review pending.

### F80 — Word budgets for the mirrors (2026-09-26)

**Decision:** The Data Foundation PRD's word budget rises to 8,600 and the Collection Mode PRD's to 12,500, so the mirrors land in plain words. It governs no row here; those PRDs' own fences carry it.

**Why:** no aligned row should reopen for compaction. Source: [owner decision N12](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-0--owner-adjudication-2026-09-26); peer review pending.

### F81 — Toolkit values are worked out from the spectrum and checked (2026-09-26)

**Decision:** SpectroCapture works out every stored value from a record's spectrum; the file's own Lab is compared, and a record differing by more than the derivation tolerance is flagged in the preview and imports with the worked-out values. The file's densities, with no v1 feature to use them, import as metadata columns. R6.2 and R6.6 carry it.

**Why:** one derivation, this app's, stands behind every stored colour. Source: [owner decision N13](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-0--owner-adjudication-2026-09-26); peer review pending.

### F82 — Among imported readings, the newest is current (2026-09-26)

**Decision:** Re-importing an identical Toolkit reading changes nothing; a newer one becomes current, the older imported reading going to history; an earlier one goes to history. A SpectroCapture scan still always stays current (F74). R6.8d and R6.9 carry it.

**Why:** a later Toolkit measurement is the better imported value, and re-importing must be safe. Source: [owner decision N14](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-0--owner-adjudication-2026-09-26); peer review pending.


**Clarified 2026-09-26 ([round 1](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-1--nine-lenses-2026-09-26); N14's own words, "An identical reading (same date and spectrum) changes nothing"):** Identical is judged against every reading the item holds, current or in history, not the current one alone, so a re-import never adds a reading the item already has and never undoes a Flag, a restore or a set-aside decision. The same reading is the same Date Saved instant to the millisecond with every reflectance the same number. R6.8f and the Vocabulary carry it.

**Clarified 2026-09-26 ([round 2](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-2--delta-verification-2026-09-26)):** A quarantined reading is never the same reading, so re-importing recovers an item whose imported current reading is quarantined (R6.8b); an R6.8f record still updates metadata under R3.5/R3.6. R6.8f carries it.

**Clarified 2026-09-26 ([rounds 3 and 4](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-4--delta-verification-2026-09-26)):** R6.8 routes R6.8f first, then R6.8b for a match with no readable current value, then by the current reading's snapshot kind. R6.8 carries it.

### F83 — A new target's name is pre-filled from the file (2026-09-26)

**Decision:** Creating a target for a Toolkit export pre-fills its name from the file's collection name, editable; an existing target ignores that name. R6.3 carries it.

**Why:** the Cataloger's Toolkit collection name is the natural default. Source: [owner decision N15](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-0--owner-adjudication-2026-09-26); peer review pending.

**Clarified 2026-09-26 ([rounds 3 and 4](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-4--delta-verification-2026-09-26)):** The pre-fill takes the Custom Collection Name of the first record left after R6.4's exclusions. R6.3 carries it.

### F84 — An imported reading's history order is the measured view's (2026-09-26)

**Decision:** N10's "history sorts it by when it was actually measured" means the over-time views, the history's measured order among them: they place an imported reading by its Date Saved. The default history list keeps every reading by record time, an imported one where its import recorded it. The Data Foundation PRD's F66 and R2.1 carry it; R6.8c says "not current" rather than "earlier".

**Why:** round 1's lenses found the drafted rows left this open or contradicted a settled row. Source: [owner decision N16](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#owner-adjudication-2026-09-26); peer review pending.

### F85 — The reason each Toolkit outcome records (2026-09-26)

**Decision:** A Toolkit reading that becomes current over an imported or simulated reading is recorded as a re-measurement without asking; one kept behind the current reading, or becoming current over none, is recorded as initial. The Data Foundation PRD's R2.3j and R2.4 carry it, R2.4 naming the exception; R6.8b, R6.8c, R6.8d and R6.8g state it.

**Why:** round 1's lenses found the drafted rows left this open or contradicted a settled row. Source: [owner decision N17](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#owner-adjudication-2026-09-26); peer review pending.

### F86 — A real Toolkit reading replaces a simulated current one (2026-09-26)

**Decision:** Where the matched item's current reading is of the simulated kind, the Toolkit reading becomes current and the simulated one goes to history; only a live scan stays current over a Toolkit reading (F74). R6.8 tells the cases apart by the current reading's snapshot kind. R6.8 and R6.8g carry it.

**Why:** round 1's lenses found the drafted rows left this open or contradicted a settled row. Source: [owner decision N18](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#owner-adjudication-2026-09-26); peer review pending.

### F87 — A same-dated, different reading is kept in history (2026-09-26)

**Decision:** A Toolkit record whose Date Saved equals the imported current reading's but whose spectrum differs is kept in history, not current, and E43 lists it. R6.8d and R6.9 carry it.

**Why:** round 1's lenses found the drafted rows left this open or contradicted a settled row. Source: [owner decision N19](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#owner-adjudication-2026-09-26); peer review pending.

### F88 — A set-aside item takes its Toolkit reading (2026-09-26)

**Decision:** A matched item with no current value — pending, set aside for any cause, or its current reading quarantined — takes the Toolkit reading as its current value and becomes captured, and E43 lists each set-aside item it captures. R6.8b and R6.9 carry it.

**Why:** round 1's lenses found the drafted rows left this open or contradicted a settled row. Source: [owner decision N20](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#owner-adjudication-2026-09-26); peer review pending.

**Clarified 2026-09-26 ([rounds 3 and 4](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-4--delta-verification-2026-09-26)):** A set-aside item E43 lists includes one set aside as unreadable, its current reading quarantined, whatever its kind. R6.8b carries it.

### F89 — One Toolkit collection per file (2026-09-26)

**Decision:** A file whose records name more than one Custom Collection Name is refused with the reason (E49); each Toolkit collection is exported and imported on its own. R6.11, R6.3 and E49 carry it.

**Why:** round 1's lenses found the drafted rows left this open or contradicted a settled row. Source: [owner decision N21](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#owner-adjudication-2026-09-26); peer review pending.

### F90 — A record that can't be checked still imports (2026-09-26)

**Decision:** A record whose file L, a and b can't be read, or whose illuminant and observer the app can't yet work under, imports with its values worked out from its spectrum under the collection's reference, and E43 lists it as not checked. R6.6 and R6.9 carry it.

**Why:** round 1's lenses found the drafted rows left this open or contradicted a settled row. Source: [owner decision N22](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#owner-adjudication-2026-09-26); peer review pending.

### F91 — Round-1 word budgets (2026-09-26)

**Decision:** The Data Foundation PRD's word budget rises to 8,900 and the Collection Mode PRD's to 12,550, so round 1's fixes land in plain words. It governs no row here; the Data Foundation PRD's F70 and the Collection Mode PRD's F223 carry it.

**Why:** round 1's lenses found the drafted rows left this open or contradicted a settled row. Source: [owner decision N23](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#owner-adjudication-2026-09-26); peer review pending.

**Closed 2026-09-26 ([the Nix Toolkit import review's round 8](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-8--confirmation-2026-09-26)):** For F69–F91, peer review closed 2026-09-26 after eight rounds, every amendment row aligned by all nine lenses; re-locked on merge.

## Fence → row map

Which rows in [`prd-inventory-import.md`](prd-inventory-import.md) carry each fence. Where a row names a fence it is for provenance only and no row re-argues one; this map is the link.

- **F4** the import target is chosen in the app — [R2.1](prd-inventory-import.md#2-target-mapping-and-the-matching-rule). **F11** Swatch Code normalisation — [R2.3](prd-inventory-import.md#2-target-mapping-and-the-matching-rule), [R2.4](prd-inventory-import.md#2-target-mapping-and-the-matching-rule). F11 also carries the capture PRD's R1.2, R6.1, and R9.2, which cite [R2.3](prd-inventory-import.md#2-target-mapping-and-the-matching-rule) across.
- In the [capture fence file](../capture-mode/prd-capture-mode-fences.md): **F24** the priority split — the [Legend](prd-inventory-import.md#legend). **F45** every requirement row is at most two sentences — every row in [§1](prd-inventory-import.md#1-reading-the-file) through [§4](prd-inventory-import.md#4-demo-device-and-verifiability). **F49** the split — this document, its four companions, and the historical ID map below.
- **F50** structural amendment — Legend, Traceability, acceptance-table shape and stable IDs. New base requirement IDs: R1.5, R1.6, R2.5, R2.6, R3.7, R3.8, R3.9; new copy IDs: E41–E45; existing IDs retained.
- **F51** blank-to-blank is unchanged — R3.6b/g.
- **F52** preview confirms the target — R2.1, R3.1; E43.
- **F53** header drift reuses the stored column — R2.2, R2.6.
- **F54** keep preserves conflicts only — R3.5, R3.6d–g; E14, E43.
- **F55** duplicate headers are a notice — R2.5; E12, E41.
- **F56** full default folding without locale tailoring — R2.3; UJ 2.2.
- **F57** comparison data version belongs to adr-0003 — R2.3; build dependencies; ADR-0003 queue.
- **F58** guard single-column reads — R1.2, R1.5, R1.6; E5, E45; OQ 1.
- **F59** mirror storage obligations — R2.2, R2.5, R2.6, R3.3; inherited obligations; DF R1.2/R2.3.
- **F60** changed source resets choices — R3.1, R3.8f; E13.
- **F61** accept remaining flow and open-question changes — R1.4, R3.8, R3.9; E7/E9/E10/E42; OQ 1/2.
- **F62** absent-field fills — R3.1, R3.6g/j, R3.9; E43; UJ 2.1.
- **F63** zero-count copy — R3.1; E43; Capture §12 placeholder index.
- **F64** target lifecycle — R3.7, R3.8j; UJ 2.
- **F65** Collection Mode rename mirror — R2.6; Collection Mode inherited-obligation line; UJ 2.1.
- **F66** Collection Mode one-writer mirror, as clarified twice — R3.2; E40; UJ 2.1.
- **F67** Collection Mode round-4 mirror, as clarified twice — R3.2, R3.8i, R3.8l; E40, E44; UJ 2.1.
- **F68** Collection Mode round-5 mirror — R4.1; Collection Mode inherited-obligation line; UJ 2.1.
- **F69** Nix Toolkit scope — §6; line 5; R1.5b; UJ 3.
- **F70** Toolkit reading made canonical — R6.5, R6.8; Data Foundation and Device inherited-obligation lines; UJ 3.
- **F71** Synthetic Toolkit fixtures, as clarified five times — R6.10, M2, OQ 3; UJ 3.
- **F72** Toolkit import home — §6; the inherited-obligation lines.
- **F73** Toolkit recognition, as clarified five times — R1.5, R1.5b, R6.1, R6.2, R6.4, R6.7, R6.9, R6.11; E5, E7, E9, E10, E42, E43, E47, E48; UJ 3.
- **F74** Scan stays current — R6.8c; UJ 3.
- **F75** Imported mark — R6.5; Collection Mode inherited-obligation line.
- **F76** Measurement-mode fit, as clarified seven times — R3.2, R3.8f, R3.8i, R3.8k, R3.8l, R3.8p, R6.3, R6.4; E13, E43, E46; Capture inherited-obligation line; UJ 3.
- **F77** Unknown provenance recorded — R6.5; UJ 3.
- **F78** Date Saved as measurement time — R6.5; UJ 3.
- **F79** Export and QC — the Data Export inherited-obligation line.
- **F80** Mirror word budgets — governs no row here.
- **F81** Worked-out values and the check — R6.2, R6.6; E43; UJ 3.
- **F82** Newest imported reading current, as clarified three times — Vocabulary; R3.3, R6.8, R6.8d, R6.8f, R6.9; UJ 3.
- **F83** Name pre-fill, as clarified — R6.3; UJ 3.
- **F84** History order — R6.8c; Data Foundation inherited-obligation line.
- **F85** Reasons — R6.8b, R6.8c, R6.8d, R6.8g; UJ 3.
- **F86** Simulated current replaced — R6.8, R6.8g; UJ 3.
- **F87** Same-dated reading kept — R6.8d, R6.9; E43; UJ 3.
- **F88** Set-aside items captured, as clarified — R6.8b, R6.9; E43; UJ 3.
- **F89** One Toolkit collection per file — R6.3, R6.11; E49; UJ 3.
- **F90** Unchecked records import — R6.6, R6.9; E43; UJ 3.
- **F91** Round-1 word budgets — governs no row here.
- Round-1 fixes under the fences above also reach R3.5 and E14 (F70, F74), R3.8b and R3.8q (F73, F89), M2 and OQ 3 (F71, F73), Build dependencies (F72), and the Data Export, Collection Mode and Capture Mode inherited-obligation lines (F72).

## Historical ID map

**Where these rows came from.** The rows in this map moved from the capture PRD under fence F49; the left-hand IDs are retired there and never reused. The copy states E4–E14, E38, and E40 moved with their numbers unchanged, and so did journeys UJ 2, UJ 2.1, and UJ 2.2.

| In the capture PRD | Here |
| :--- | :--- |
| R2.1 | [R1.1](prd-inventory-import.md#1-reading-the-file) |
| R2.2 | [R1.2](prd-inventory-import.md#1-reading-the-file) |
| R2.3 | [R1.3](prd-inventory-import.md#1-reading-the-file) |
| R2.4 | [R1.4](prd-inventory-import.md#1-reading-the-file) |
| R2.5 | [R2.1](prd-inventory-import.md#2-target-mapping-and-the-matching-rule) |
| R2.6 | [R2.2](prd-inventory-import.md#2-target-mapping-and-the-matching-rule) |
| R2.7 | [R2.3](prd-inventory-import.md#2-target-mapping-and-the-matching-rule) |
| R2.8 | [R2.4](prd-inventory-import.md#2-target-mapping-and-the-matching-rule) |
| R2.9 | [R3.1](prd-inventory-import.md#3-preview-and-commit) |
| R2.10 | [R3.2](prd-inventory-import.md#3-preview-and-commit) |
| R2.11 | [R3.3](prd-inventory-import.md#3-preview-and-commit) |
| R2.12 | [R3.4](prd-inventory-import.md#3-preview-and-commit) |
| R2.13 | [R3.5](prd-inventory-import.md#3-preview-and-commit) |
| R2.14 | [R3.6](prd-inventory-import.md#3-preview-and-commit) |
| R11.15h | [R4.1](prd-inventory-import.md#4-demo-device-and-verifiability) |
| M7 | [M1](prd-inventory-import.md#success-metrics) |
| OQ 12 | [OQ 1](prd-inventory-import.md#open-questions) |

[R4.2](prd-inventory-import.md#4-demo-device-and-verifiability) is not in the map. It carries over these states the obligation the [capture PRD's R11.12](../capture-mode/prd-capture-mode.md#11-demo-device-and-verifiability) held before the split, and that row stays live there.
