# Data Foundation PRD — owner decisions (fences)

Owner decisions on this PRD. A fence is settled: reviewers do not re-litigate it, and a row that names one does so for provenance only. Owner-locked rows: none yet. Review log: `../../agent-reviews/2026-09-09-prd-data-foundation-peer-reviews.md` — shared with [the export PRD](../export/prd-data-export.md) since fence F30, one log and one fresh-lens ledger across both documents.

## Phase 0 — research inventory (2026-09-09)

Ingested before any round: every brief under `docs/briefs/` on `main` at `dd43c13`; the browsing-a-collection-at-scale research results v2 (brought into `docs/briefs/` from the foundry in this PR; supersedes the first-pass results on any point where they conflict); the X-Rite SDK and open-source prior-art findings (brought in the same way; CxF and export shapes). The three locked PRDs and their obligations tables. Deferred by the owner: the Spectro 2 software competitive brief (positioning, not data). No project-supplied prior-art query command.

### F1 — Data Foundation is a slim data contract, not a schema document (2026-09-09)

**Decision:** This PRD states what the user-owned file guarantees: what is kept and never destroyed, what "canonical value" and version history mean to someone reading the file, which derived spaces are present and what the gamut-clipped flag asserts, what the CSV export contains, what a schema migration does to a file a user already has, what the user may delete, and the obligations the capture-mode, import, and device-management PRDs already impose on Data Foundation, gathered here by citation. The storage schema, blob layout, library choice, and migration mechanism are out of scope: they belong to ADR-0003 and a technical spec that follows this document. Word budget for the PRD body: 8,000 words (raised from 4,000 to 5,000 in Phase 3 when the first fill landed at 5,030 after four compaction passes; from 5,000 to 7,000 after round 1, 2026-09-09; and from 7,000 to 7,500 on 2026-09-10 before the pre-lock round, when the mechanical lock checks required a 21-row obligations map and four Vocabulary entries — bookkeeping, not rules — and the body landed at 7,281 with nothing left to cut but rules; and from 7,500 to 8,000 on 2026-09-14 after the pre-lock round, whose two fresh lenses — reliability and standards — named rules the file and export contracts lacked (a route back from a read-only state, a failure contract on the app's own copies, a dead-process hold test, a value-encoding rule): nine lenses found some fifteen missing guarantees — the sample tier, time axes, atomic writes, the sync posture, the file's identity and floor, the CSV contract, fixtures — which are rules, not prose). Shape: the capture PRD's three-file shape plus this fence file and an OQ results file; no author line; row IDs `R<section>.<n>`, `E<n>`, `M<n>`; every requirement row at most two sentences.

**Why:** U5 and U6 are promises to users about the data itself, and the data-consumer persona has no other PRD; the rest is engineering, and reviewing schema through product lenses produces the "solution smuggled into a requirement" findings the gate exists to reject. Everything this PRD can inherit it cites rather than restates.

### F2 — One file, one open at a time (2026-09-09, Phase 3)

**Decision:** A user's data lives in one store the user owns, and one store is open at a time; the matching rule and every cross-collection view span that one file. Closes OQ 1.

**Why:** The browsing research favours one store over many; the capture PRD's OQ 19 already leans this way; every "the file" guarantee is stated once.

### F3 — A re-scan asks correction-or-re-measurement once, outside the loop (2026-09-09, Phase 3)

**Decision:** The file records whether a new version is a correction (the old value was wrong) or a re-measurement (the item changed). A re-scan made during capture defaults to correction and is never asked mid-loop; the question is asked once, outside the heads-down loop, and the answer lands on the version. Inherited obligation for the capture PRD: its E29 state gains the correction default as a post-lock amendment. Closes OQ 3.

**Why:** The research is unambiguous that the distinction cannot be inferred; not asking means fabricating it, and asking mid-loop breaks heads-down capture.

**Clarification (1), 2026-09-09:** A capture-time re-scan is a correction provisionally; the app asks once after the session, or on that item's next review, and the answer replaces the provisional value.

### F4 — The gamut-clipped flag compares against a fixed stored reference (2026-09-09, Phase 3)

**Decision:** GAMUT_REFERENCE_SPACE is sRGB and GAMUT_RENDERING_INTENT is relative colorimetric, stored with the datum, so the flag means the same thing to every reader of the file. A live display-dependent check is Collection Mode's, not this file's. Closes OQ 7.

**Clarified 2026-09-25 (F53, under [the Collection Mode PRD's F127](../collection-mode/prd-collection-mode-fences.md)):** the stored flag is tested as the Collection Mode PRD's OQ 7 interim tests a display's gamut — Bradford adaptation, relative colorimetric, zero tolerance — at sRGB, so the stored flag and the display check are one computation; R3.4 says so and keeps its alignment, and ADR-0003 takes it as an input. Peer review pending.

### F5 — Export is canonical-only by default with version history as a v1 option (2026-09-09, Phase 3)

**Decision:** The default CSV export is one row per item carrying its canonical value; an explicit option exports every version. The canonical export is P0; the history option is P1. Closes OQ 9 (now the export PRD's OQ 2) and settles the export rows' priority.

**Clarification (1), 2026-09-09:** The test row that asserts the export's column set — now the export PRD's R4.4 (this document's R7.6 before F30) — is P0 with the rows it verifies (process rule 3).

### F6 — Version history is kept for good (2026-09-09, Phase 3)

**Decision:** HISTORY_RETENTION means nothing is aged out; no retention window exists. Closes OQ 4.

**Why:** Corrections never destroy data (AGENTS.md §4), and the research names retrofitting a policy onto a store that cannot enumerate its history as the trap.

### F7 — A delete is undoable until the app quits (2026-09-09, Phase 3)

**Decision:** DELETE_UNDO_WINDOW is the running session: a deleted item and its history can be restored until the app quits, and the deletion is final after. Closes OQ 10.

**Clarified 2026-09-17 (F35):** The original until-quit decision remains above; R6.3 also names crash and file close as ending undo. The copy and OQ 10 now enumerate all three, defining the open-file lifetime rather than a capture session.

### F8 — The vendor SDK's analytics disclosure belongs to the Telemetry PRD and the help docs (2026-09-09, Phase 3)

**Decision:** This PRD hands the disclosure over as an inherited obligation; it carries no disclosure row of its own. Closes OQ 12's ownership half; whether the analytics can be disabled stays open there.

### F9 — Journey DJ5, querying the file without the app, stays (2026-09-09, Phase 3)

**Decision:** The journeys file keeps DJ5; it is non-normative and states what a reader of the file can rely on, which is U6's headline job.

### F10 — The canonical value is a stored mean; samples are a third tier (2026-09-09, round 1)

**Decision:** Every sample of a saved set is kept exactly as the instrument produced it. An item's canonical value is the set's mean, stored rather than computed on read, readable without the app, and recording the basis it was taken on and the version of the working-out. Samples are a tier beneath a reading: never version-history entries, never counted as "earlier readings". The canonical value is defined by what it must contain — the spectrum or colour values, the conditions, the device snapshot, the basis, the derivation version — and a vendor's raw payload is an archived artifact beside it where one exists.

**Why:** The capture PRD's locked R4.12 averages N samples; a "raw payload as the instrument produced it" cannot be that mean. Both the architecture and staff lenses called this the round's one-way door, and the research names retrofitting a tier as the trap.

**Clarified 2026-09-18 (Capture agent-build amendment):** Capture F52 aligns its R4.12 and vocabulary with this stored-mean decision: sample-to-mean agreement uses D50/2°, spectral display derivations follow collection reference, and non-spectral restrictions remain R3.5. Capture F33’s raw-payload wording survives only as historical rationale.

**Clarified 2026-09-18 ([owner decisions](https://github.com/vinnyp/spectro-capture/pull/20#issuecomment-5736642147)):** D1 / Capture F56 individually ratifies the F52 reference split already mirrored here; non-spectral restrictions and the canonical stored-mean decision remain unchanged.

### F11 — Readability is split from the archived payload (2026-09-09, round 1)

**Decision:** Everything a reader needs — the decoded spectrum or colour values, the conditions, the derived values, the current-value marker, the supersession reason — is stored in plain SQLite types, readable at SQLITE_READER_FLOOR with no extension and no app function. The vendor's opaque payload is an archived artifact and may be compressed. STORE_SIZE_BUDGET is re-derived on the real population, samples counted; OQ 5 records this constraint; OQ 2's floor is derived from the features actually kept, not from a rejected one.

### F12 — An unanswered re-scan is recorded as unconfirmed and stays visible (2026-09-09, round 1)

**Decision:** The supersession reasons are initial, re-measurement, correction, correction-unconfirmed, and restore; a QC reading is a record of its own, not a supersession. Only a confirmed correction is marked never-true and excluded from over-time views; an unconfirmed one is shown and marked. Every unconfirmed reading is enumerable and answerable in bulk after a session, and the question offers "Ask me later". The capture PRD's E29 amendment (F3) uses this document's axis, "supersession reason", never that state's own word "correct".

### F13 — Synced and network volumes: warn, and scope durability to local volumes (2026-09-09, round 1)

**Decision:** The app names the risk when the file sits in a known sync-managed or network location; the crash-durability guarantee is stated for local volumes, with other classes marked needs-hardware-verify; the help docs carry the one safe practice. A hold left by a process that is gone never blocks the owner from opening their file. What the app sends is distinguished from what the user's chosen location does.

### F14 — The first launch asks where to keep the file (2026-09-09, round 1)

**Decision:** Before anything else, the first launch asks where the file lives, offering a default; the app shows where the file is and lets the user move it or open another. Closes the capture PRD's OQ 19 half that F2 left (that PRD's OQ 19 closes on F2 and F14, a post-lock amendment there). Owner's call over the recommendation to default silently.

### F15 — The app deletes items and collections, never the file (2026-09-09, round 1)

**Decision:** Whole-file deletion is Finder's job and the help docs say so. A collection delete has its own counted confirmation state; the undo window covers items and collections; once the window closes, deleted content is not recoverable from the file by an outside reader, and a crash is not a quit. The guarantees that a single reading is never deletable out of history and that delete is never a default action are imposed on Collection Mode.

### F16 — R6.1, R6.2, and R6.5 are P0; R6.3's undo stays P1 (2026-09-09, round 1)

**Decision:** The delete-scope rules, the counted confirmation, and the credential guard ship with the deletes and the export they govern; the credential test asserts against the store file as well as an export. R6.5 states a positive inventory of what the file holds about the user and their instrument rather than a negative blanket.

### F17 — Notes and metadata are correctable in place (2026-09-09, round 1)

**Decision:** Version history covers measurements. Item metadata and notes are editable and clearable; an edit supersedes the old text without touching any reading.

**Clarified 2026-09-25 (F52, under [the Collection Mode PRD's F35](../collection-mode/prd-collection-mode-fences.md)):** the superseded or cleared text is beyond the recovery of anyone reading the file's bytes, not only a SQL reader, as R2.3 and R6.2a now say; ADR-0003 chooses how. Peer review pending.

**Clarified 2026-09-25 (F53, under [the Collection Mode PRD's F102](../collection-mode/prd-collection-mode-fences.md)):** that text is gone from the moment the write lands, from the file and from any journal, log, index or other file beside it, while the file is open and after a crash; R2.3 says so and keeps its alignment. Peer review pending.

### F18 — The CSV carries the vendor's raw payload column (2026-09-09, round 1)

**Decision:** One opaque column per row, documented as the vendor's round-trip string, so the export carries the file's fidelity promise and M3 (now the export PRD's M1) reads as written; a reading with no payload leaves it empty and marked.

### F19 — v1 does not ship without the vendor-analytics disclosure (2026-09-09, round 1)

**Decision:** A §6 row gates release on a disclosure that names the recipient, the events (connect, scan, calibration), that it fires per event and cannot be switched off, and that it is separate from the app's own opt-in telemetry. Authorship stays with the Telemetry PRD and the help docs (F8 unchanged).

### F20 — The gamut-clipped export column is `sRGB_gamut_clipped` (2026-09-09, round 1)

**Decision:** Closes OQ 8 (now the export PRD's OQ 1) by owner decision; revisited only if ISO 17972-4's schema becomes readable.

### F21 — Word budget 7,000 (2026-09-09, round 1)

**Decision:** Recorded in F1's amended text above. **Amended 2026-09-10 (owner, before the pre-lock round):** 7,500, for the lock checks' traceability and vocabulary additions. **Amended 2026-09-14 (owner, after the pre-lock round):** 8,000, for the fresh lenses' rules.

**Clarified 2026-09-25 ([the Collection Mode PRD's F154](../collection-mode/prd-collection-mode-fences.md), owner decision D35):** the budget is 8,200 words; see F55.

**Clarified 2026-09-25 ([the Collection Mode PRD's F160](../collection-mode/prd-collection-mode-fences.md), owner decision D41):** the budget is 8,300 words; see F55.

**Clarified 2026-09-25 ([the Collection Mode PRD's F177](../collection-mode/prd-collection-mode-fences.md), owner decision D46):** the budget is 8,400 words; see F55.

**Clarified 2026-09-26 ([the Collection Mode PRD's F214](../collection-mode/prd-collection-mode-fences.md), owner decision D68):** the budget is 8,450 words; see F55.

**Clarified 2026-09-26 ([the Collection Mode PRD's F220](../collection-mode/prd-collection-mode-fences.md), owner decision D74):** the budget is 8,480 words; see F55.

**Clarified 2026-09-26 ([the Inventory Import PRD's F80](../import/prd-inventory-import-fences.md), owner decision N12):** the budget is 8,600 words; see F55.

**Clarified 2026-09-26 ([the Inventory Import PRD's F91](../import/prd-inventory-import-fences.md), owner decision N23):** the budget is 8,900 words; see F55.

### F22 — Export column names, the second-file outcome, and two tokens (2026-09-09, after the round-1 fix pass)

**Decision:** (a) Every column the app emits in the CSV carries the prefix `sc_`; an imported column that would collide is emitted as `import_<name>` and the export surface says so (R4.8, now the export PRD's R2.4). (b) Opening a second file is refused while a capture session is running; otherwise the file in hand closes first (R1.3). (c) An absent derived value exports as an empty field, never a zero (R3.5); no COMPATIBILITY_FLOOR constant exists until a release raises the floor above the first file version (R5.7).

### F23 — The `sc_` prefix applies to every app column (2026-09-09, round 2)

**Decision:** F22(a) subsumes F20 and the device PRD's `simulated` column: the export columns are `sc_sRGB_gamut_clipped`, `sc_sRGB_source_space`, `sc_sRGB_rendering_intent`, `sc_simulated`, and `sc_nm_<wavelength>` for the spectral columns. F20's answer and OQ 8's (now the export PRD's OQ 1) results section are restated under the prefix; the device PRD's "emits a `simulated` column" obligation is read as `sc_simulated`, carried as a post-lock amendment line under "what this PRD imposes on others". No exemption list exists.

**Why:** One rule the golden header can be written against; a closed exemption list would re-open the collision F22 exists to close.

### F24 — The export format version fixes the wavelength column set (2026-09-09, round 2)

**Decision:** The wavelength column set — start, end, interval — is part of the export format version; v1's set is the v1 instrument family's reported grid, carried as OQ 16 (now the export PRD's OQ 4) with a per-SDK-docs candidate closed on hardware. A reading whose wavelengths fall outside the set exports those columns empty and marked; any change to the set bumps the export format version (R4.9, now the export PRD's R2.5).

**Why:** R4.7's "column order is fixed" (now the export PRD's R2.3) and R7.7's golden are undefined without a fixed set; a header that follows the data is unusable to a consumer allocating columns before reading.

### F25 — Samples are readable at the floor (2026-09-09, round 2)

**Decision:** Each sample's decoded spectrum or colour values are stored in plain SQLite types beside its reading, inside R1.2's readability floor, so an outside reader can check the stored mean against its samples; each sample's vendor payload is an archived artifact per F11 and may be compressed. OQ 5's budget is re-derived on this population.

**Why:** R1.2 promises everything a reader needs; the evidence behind every canonical value is part of that, and R7.1 cannot verify a mean whose samples it cannot see.

### F26 — The CSV carries one payload column per sample slot (2026-09-09, round 2; amends F18)

**Decision:** The canonical export carries `sc_sample_1_payload` … `sc_sample_5_payload`, one column per sample slot up to the capture PRD's maximum of five, empty where a slot is unused or a sample has no payload. F18's "one opaque column" becomes up to five; the header stays fixed.

**Why:** A reading of N samples has N payloads; a fixed slot set keeps R7.7's golden well-defined and gives M3 (now the export PRD's M1) every sample it reconstructs from.

### F27 — Derived values are kept per reading, history included (2026-09-09, round 2)

**Decision:** Every reading — superseded ones too — carries its six derived spaces; a derivation bump regenerates every reading's set and marks the old sets superseded. An outside reader plots an item's history from plain columns. OQ 5's budget is re-derived on this population.

**Why:** R2.5's over-time view, R4.4's history export (now the export PRD's R1.3), and R7.1's read-back all read derived values off superseded readings; keeping them only on the canonical value would make history a value only the app can compute, which R1.2 forbids.

### F28 — The collection's chosen condition is the current derived set; the canonical export emits it (2026-09-09, round 2)

**Decision:** Derived sets are keyed per reading and measurement condition; the set for the collection's chosen scan mode ([the capture PRD's R1.10](../capture-mode/prd-capture-mode.md#1-collections)) is always present and is the current one, and another condition's set is present when the user asks for it. The canonical export emits one row per item on the chosen condition, the condition column saying which; the header stays fixed. **Amended 2026-09-10 (round 3, FX3-6):** the chosen condition's set is absent where the reading carries no measurement in that condition, which R3.5 marks.

**Why:** The capture PRD keeps every mode a reading arrives with and works colour values out from the chosen mode; one row per item (F5) and a fixed header (F24) survive only if the export names one condition per row.

### F29 — Every item gets an export row (2026-09-09, round 2)

**Decision:** Every item in the collection gets a row in the canonical export; an item with no canonical value carries its identity and import columns with its colour, spectral, and payload columns empty, and a column states whether it is never-scanned or quarantined. Both cases join M3's (now the export PRD's M1) population.

**Why:** A half-scanned collection is the ordinary state between sittings, and the export must reconcile against the spreadsheet the inventory came from.

**Clarification (2026-09-18, PR #18):** F29’s historical export payload clause is read under Export F14/DF F37: quarantine alone does not suppress an available archive. [Owner decisions 2 and 4](https://github.com/vinnyp/spectro-capture/pull/18#issuecomment-5732380642) confirm the boundary; Export F21 specifies canonical quarantine provenance.

### F30 — The export contract is its own PRD (2026-09-09, after the round-2 fix pass)

**Decision:** The CSV export contract — §4 (R4.1–R4.9), the golden-file rule (R7.9), the export half of R7.8, the export surface (R7.6a), M3, OQ 8, OQ 9, OQ 11, OQ 16, the export states (E6, E7, E17, E18), and DJ1 — moves to a new PRD, Data Export, at `docs/product/export/` in the five-file shape, with its own row-ID families, fences, and a 4,000-word budget. Moved IDs are retired in this document's Legend naming their destination and recorded in the destination's Traceability naming their origin; the decisions the moved rows carry (F5, F18, F20, F22, F23, F24, F26, F28, F29) are transcribed into the export PRD's fence file with their provenance, and this file keeps them as history. The two PRDs share this review log; round 3 onward reads both. Chosen over raising the budget again (4,000 → 5,000 → 7,000) after the round-2 fix pass landed at 7,690 with nothing left to cut but rules.

**Why:** The export is a consumer contract with its own audience (U6's downstream tools) and its own fresh-lens set; Data Foundation is the file's promise to its owner. A document that cannot fit because it has too many rows is two PRDs (writing-prds, Phase 2).

**Clarified 2026-09-17 (F35):** The complete retired-ID map moved from the main Legend to this file’s [Retired ID map](#retired-id-map); the Legend links to it. IDs and destinations did not change.

### F31 — A non-chosen condition's derived set is kept once asked for (2026-09-10, round 3)

**Decision:** A derived set for a condition other than the collection's chosen scan mode is worked out when the user asks for it and is then kept in the file like any other set — regenerated with the rest and readable at the floor. OQ 5 gains a third multiplier, bounded by the instrument's condition count. **Amended 2026-09-10 (round 4, owner):** the CSV stays one condition per row on the collection's chosen scan mode in both exports; the extra sets are readable in the file and not carried by the history export, and the export preview says which condition the rows come out on.

**Why:** R1.2 promises that nothing a reader needs is a value only the app can compute; a set computed for the moment and discarded would be exactly that. The export stays one condition per row so the header stays fixed (F28, the export PRD's F9).

### F32 — R6.5's inventory names the capture PRD's interaction record, measuring builds only (2026-09-10, before the pre-lock round)

**Decision:** The privacy inventory in R6.5 gains the per-session shortcut and control interaction record [the capture PRD's R11.16](../capture-mode/prd-capture-mode.md#11-demo-device-and-verifiability) keeps, present in a measuring build only and never in a release build, so the inventory stays closed and true.

**Why:** The lock checks' obligations map found R11.16 carried by no Data Foundation row; a closed inventory that omits a record the file can hold is false, and excluding the record from the file would amend a locked PRD.

## Fence → row map

Filled by the Phase 3 fix pass and extended by the round-1 and round-2 fix passes (2026-09-09). A row "carries" a fence when the fence's decision is what the row now states; the fence file, not the row, holds the rationale.

| Fence | Rows that carry it |
| :--- | :--- |
| F1 | Every row in the PRD body, plus its Background scope statement, its shape, and its word budget (F21 and F55, 8,400 since 2026-09-25). |
| F2 | R1.1, R1.3; the Capture Mode OQ 19 line in [Sibling amendment map](#sibling-amendment-map); OQ 1. |
| F3 | R2.4; the Capture Mode line in [Sibling amendment map](#sibling-amendment-map); OQ 3. |
| F4 | R3.4; OQ 7. |
| F5 | No row here any more: moved to the export PRD's R1.1 (canonical default, P0) and R1.3 (history option, P1), the P0/P1 split in [its Legend](../export/prd-data-export.md#legend) and the Pri cells of its R1.2, R2.1, R3.1, and its OQ 2 — transcribed there as [its F2](../export/prd-data-export-fences.md). This document's Legend keeps only the delete undo as P1. |
| F6 | R2.6; OQ 4. |
| F7 | R6.3; OQ 10. |
| F8 | R6.6 (authorship handed over; the gate itself is F19's); the Telemetry and help-docs line in [Inherited obligations](prd-data-foundation.md#inherited-obligations) → "what this PRD imposes on others"; §8's Error & State Copy paragraph; OQ 12's ownership half. |
| F9 | No requirement row. It keeps [DJ5](prd-data-foundation-journeys.md#dj5-query-the-file-without-the-app) in the journeys companion, which the [User Journeys](prd-data-foundation.md#user-journeys) index lists as DJ5. |
| F10 | R2.1; the Vocabulary entries for sample, reading, and canonical value; R6.2's counted-in-readings clause and [E8](prd-data-foundation-copy.md#error--state-copy); M6's population; the measurement-operations table; the Vision line in [Sibling amendment map](#sibling-amendment-map). |
| F11 | R1.2 (the readability half), R1.6 (the archived-payload half); OQ 5's constraint, OQ 2's derivation. |
| F12 | R2.4, R2.5, R2.8; R7.1's read-back set and M4's third question; [E11](prd-data-foundation-copy.md#error--state-copy)'s "Ask me later"; the Capture Mode E29 line in [Sibling amendment map](#sibling-amendment-map); OQ 3's amendment. |
| F13 | R1.7; the dead-process hold in R1.5 and R7.3; R1.4's second clause; R7.4's volume-class scoping and M1's population. |
| F14 | R1.8, R1.9; [E13](prd-data-foundation-copy.md#error--state-copy); the Capture Mode OQ 19 line in [Sibling amendment map](#sibling-amendment-map); the Device Management line (UJ1) there. |
| F15 | R6.1, R6.2, R6.3; [E14](prd-data-foundation-copy.md#error--state-copy); the Collection Mode line in [Inherited obligations](prd-data-foundation.md#inherited-obligations) → "what this PRD imposes on others". |
| F16 | The Pri cells of R6.1, R6.2, R6.5 (P0) and R6.3 (P1), and the Legend's P0/P1 split; R6.5's positive inventory and its two-surface credential test. |
| F17 | R2.3's second sentence. |
| F18 | No row here any more: moved to the export PRD's R1.1 (the raw-payload column) and its M1's statistic — [its F3](../export/prd-data-export-fences.md). Amended by F26: one column per sample slot. |
| F19 | R6.6; the Telemetry and help-docs line in [Inherited obligations](prd-data-foundation.md#inherited-obligations) → "what this PRD imposes on others"; OQ 12's gate half. |
| F20 | No row here any more: moved to the export PRD's R2.1 and its OQ 1 — [its F4](../export/prd-data-export-fences.md). |
| F21 | F1's amended text; no requirement row. |
| F22 | [R1.3](prd-data-foundation.md#1-the-file-the-user-owns) (b); [R5.7](prd-data-foundation.md#5-migration-and-compatibility) (c, the COMPATIBILITY_FLOOR half); [R3.5](prd-data-foundation.md#3-derived-values-and-gamut-honesty) states the absent value the export writes empty. (a) and the export half of (c) moved to the export PRD's R2.4 and R2.1 — [its F5](../export/prd-data-export-fences.md). |
| F23 | No row here any more: moved to the export PRD's R1.2, R2.1, R2.3 and R2.4, its OQ 1's results section, and the Device Management export line both ways in [its obligations tables](../export/prd-data-export.md#inherited-obligations) — [its F6](../export/prd-data-export-fences.md). |
| F24 | No row here any more: moved to the export PRD's R2.3, R2.5, R4.1 and R4.2, and its OQ 4 — [its F7](../export/prd-data-export-fences.md). [R7.7](prd-data-foundation.md#7-verifiability) keeps the fixtures those rows run on. |
| F25 | R1.2, R1.6, R2.1, R7.1; M6's population; OQ 5's population; the outbound Data Export line in [Inherited obligations](prd-data-foundation.md#inherited-obligations). |
| F26 | No row here any more: moved to the export PRD's R1.1, R4.1, R4.2 and its M1 — [its F8](../export/prd-data-export-fences.md). [R7.7](prd-data-foundation.md#7-verifiability) keeps the fixtures. |
| F27 | [R3.1](prd-data-foundation.md#3-derived-values-and-gamut-honesty), [R3.3](prd-data-foundation.md#3-derived-values-and-gamut-honesty), [R2.5](prd-data-foundation.md#2-canonical-value-and-version-history), [R7.1](prd-data-foundation.md#7-verifiability), [R7.5](prd-data-foundation.md#7-verifiability); M6's population; OQ 5's population; the outbound Data Export line in [Inherited obligations](prd-data-foundation.md#inherited-obligations). R4.4's history export moved to the export PRD's R1.3, which cites this fence from there. |
| F28 | [R3.1](prd-data-foundation.md#3-derived-values-and-gamut-honesty); the Capture Mode line in [Inherited obligations](prd-data-foundation.md#inherited-obligations) (its R1.10, R4.5); the outbound Data Export line there. The export half moved to the export PRD's R1.1, R2.1 and R2.3 — [its F9](../export/prd-data-export-fences.md). |
| F29 | No row here any more: moved to the export PRD's R1.1 and R2.1, its M1's population, and its E1 — [its F10](../export/prd-data-export-fences.md). |
| F30 | The [retired-ID map](#retired-id-map); the Open Questions' retirement note; the Background scope statement; [R7.7](prd-data-foundation.md#7-verifiability), [R7.8](prd-data-foundation.md#7-verifiability) and [R7.6](prd-data-foundation.md#7-verifiability), each having handed one half over; the obligations tables both ways; and [the export PRD's F1](../export/prd-data-export-fences.md), which transcribes it. No requirement row of its own. |
| F31 | [R3.1](prd-data-foundation.md#3-derived-values-and-gamut-honesty); OQ 5's decision-so-far; [M6](prd-data-foundation.md#success-metrics)'s population and [R7.7](prd-data-foundation.md#7-verifiability)'s fixture; the outbound Data Export line in [Inherited obligations](prd-data-foundation.md#inherited-obligations); the export PRD's R1.1, R1.3 and E1 (one condition per row, the amendment). |
| F32 | [R6.5](prd-data-foundation.md#6-deletion-and-privacy); the Capture Mode OQ 24 line in [Sibling amendment map](#sibling-amendment-map). |
| F33 | R2.9; R5.5a–c/f (F47 defines derived-set regeneration); R7.6g/h; R7.7i; E4/E31; DJ3; OQ 21, with interim export behavior settled by F37. |
| F34 | R1.3; R7.3a/b; E22; DJ3. |
| F35 | Legend/Traceability; R2.3a–i; R3.3a–g; R7.3a–k (R7.3l retired by F47); R6.2a–d/R6.3; R7.7a–m (PR #18 extends the inventory with R7.7n under Export F20, and a/e/i under Export F23/F19/F26); E8/E14; OQ 10/14/17–21; DJ2–DJ5. |
| F36 | R5.5d, R7.3; post-lock ADR-0003 item. |
| F37 | R5.5a–c, outbound Data Export obligation, R7.7i, OQ 21; DE R1.1/R1.3 and F14. |
| F38 | R1.5/R1.9, R7.3c/j, OQ 19, E9/E15/E25/E27, DJ3. |
| F39 | R5.3, R7.3f, OQ 18, E1/E23 and copy variants, DJ3. |
| F40 | R2.3f, R7.7g, DJ2. |
| F41 | R7.7, R7.7e; post-lock live-kind golden. |
| F42 | R3.3g, R7.1/R7.5, R7.7i, DJ5. |
| F43 | R7.7/R7.7m, R5.1/R5.2, R7.3, E2/E3/E23/E24, M5. |
| F44 | R1.3, R7.3b, DJ3; amends F34’s close-first ordering. |
| F45 | F34 dated confirmation; R1.3, R7.3a, E22. |
| F46 | Legend, Traceability, primary requirement tables. |
| F47 | R3.3a, R5.4/R5.5f, R7.6g/h, R7.7i, §8, DJ3; retired E32/R7.3l. |
| F48 | E14 and copy Variants; R6.1/R6.2. |
| F49 | post-lock Data Foundation list; future M10, no metric row yet. |
| F50 | R1.2, R6.2, R7.6o, E33; the Collection Mode line in [Inherited obligations](prd-data-foundation.md#inherited-obligations) → "what other PRDs impose on this one"; the copy header and Variants note; DJ4, DJ5. Clarified 2026-09-24 (E33's Export first at collection scope): R6.2, R7.6o, DJ4. |
| F51 | R2.9; DJ2; the Collection Mode line in [Inherited obligations](prd-data-foundation.md#inherited-obligations) → "what other PRDs impose on this one"; the dated F33 clarification. |
| F52 | R1.2, R2.3, R2.3f, R6.2a, R7.2, R7.6k, E8, E33; DJ2, DJ4, DJ5; the Collection Mode line in [Inherited obligations](prd-data-foundation.md#inherited-obligations) → "what other PRDs impose on this one"; the dated F17, F40 and F50 clarifications. |
| F53 | R1.10, R2.3, R3.4, R6.2a, R7.3j, R7.6p, E11, E26, E34, OQ 20; DJ3; the Collection Mode line in [Inherited obligations](prd-data-foundation.md#inherited-obligations) → "what other PRDs impose on this one"; the dated F4, F17, F50 and F52 clarifications. Clarified 2026-09-25 (the Collection Mode PRD's F171): the ADR-0003 search input in (5). |
| F54 | R1.5, R2.3, R3.4, R6.2, R6.2a, R6.2d, R7.3j, R7.6p, E34, OQ 20; DJ3; the Collection Mode line in [Inherited obligations](prd-data-foundation.md#inherited-obligations) → "what other PRDs impose on this one"; the dated F50 and F53 clarifications. Clarified 2026-09-25 (the Collection Mode PRD's F151–F153): R1.5, R1.9, R7.3j, R7.6p, E34; DJ3; the Collection Mode line. Corrected and clarified 2026-09-25 (the Collection Mode PRD's F159 and F165): R3.4, R6.2a, R7.3j, R7.6p; DJ3. Clarified 2026-09-25 (the Collection Mode PRD's F174): R7.3j; DJ3. |
| F55 | F1's amended text (the word budget, 8,400 since 2026-09-25); no requirement row. |
| F56 | R1.3, R1.5, R1.9, R1.11, R2.3, R6.2a, R7.3j, R7.6q, E15, E34, E35, OQ 17; DJ3, DJ4; the Collection Mode inbound line and the Collection Mode, Capture Mode and Inventory Import outbound lines in [Inherited obligations](prd-data-foundation.md#inherited-obligations); F1's map line; the dated F53 clarification. |
| F57 | R1.11, R6.2a, R6.3, R7.6d, E33, E35, OQ 20; DJ3, DJ4; the copy header; the Collection Mode inbound and outbound lines in [Inherited obligations](prd-data-foundation.md#inherited-obligations); the dated F21, F54, F55 and F56 clarifications. |
| F58 | R1.11, R6.2a, R7.6b, E15, E34, E35, OQ 20; DJ3, DJ4; the copy header; the Collection Mode inbound line in [Inherited obligations](prd-data-foundation.md#inherited-obligations); the Corrected F57 line. |
| F59 | R6.2, R6.2a, E35; DJ3; the Collection Mode inbound line in [Inherited obligations](prd-data-foundation.md#inherited-obligations), its fence range now F52–F59. |
| F60 | R6.2, R6.2a, R1.11; DJ3; the Collection Mode inbound line in [Inherited obligations](prd-data-foundation.md#inherited-obligations), its fence range now F52–F60. |
| F61 | R6.2, R6.2a; DJ3; the Collection Mode inbound line in [Inherited obligations](prd-data-foundation.md#inherited-obligations), its fence range now F52–F61. |
| F62 | R6.2, R6.2a; DJ3; the Collection Mode inbound line in [Inherited obligations](prd-data-foundation.md#inherited-obligations), its fence range now F52–F62. |
| F63 | R6.2, R6.2a; DJ3 (d), (k), (l) and (m); the Collection Mode inbound line in [Inherited obligations](prd-data-foundation.md#inherited-obligations), its fence range now F52–F63. |
| F64 | Vocabulary; R1.2, R1.6, R2.1, R2.2, R2.3, R2.3h, R2.3j, R5.5d, R7.1, R7.2, R7.5, R7.7, R7.7o, M2; DJ3, DJ6; the out-of-scope line, the journeys index and the Inventory Import and Device Management lines in [Inherited obligations](prd-data-foundation.md#inherited-obligations). |
| F65 | Governs no rows. |
| F66 | R2.1; DJ6. |
| F67 | R2.3j, R2.4; DJ6. |
| F68 | R2.3j; DJ6. |
| F69 | R2.3j; DJ6. |
| F70 | Governs no rows. |

## Rejected findings

- **agy staff, round 1, "derivation regeneration storage leak" (R3.3):** asked to drop the requirement that values on an older derivation stamp stay findable. Rejected: reproducibility of a value a user has already exported or quoted is the point of the stamp; the architecture lens's rule that exactly one set is current and the rest are retained and marked superseded is adopted instead.
- **agy privacy, round 1, Low (R6.1):** asked for deletion of a single reading out of an item's history. Rejected under the non-negotiable that corrections never destroy data and fence F6; the erasure need it points at is met by F17 (notes correctable in place) and F15 (item and collection delete).
- **standards, round 7, SR7-4 (R5.5):** asked to scope the salvage promise to the invariants this document states. Rejected (owner, 2026-09-15): the salvage output resolves whatever its source broke and names each choice ([E5](prd-data-foundation-copy.md#error--state-copy)); the per-invariant rules are ADR-0003's, and enumerating them at WHAT level is the schema document F1 fences this PRD off from. The one rule stated — two current readings — stays as the worked example.
- **privacy, round 7, Medium (E27):** asked for a disclosure sentence on the network-volume state — that the whole collection sits on a host someone else administers. Rejected (owner, 2026-09-15): E27 names the two risks that class carries; R1.7's risk pair is scoped per class instead (round-7 fix FX7-3); a network share is the user's own choice of location, and F13's warning is about the damage and the copies the app's own behaviour causes.


## Agent-build amendment (2026-09-17)

The owner approved the audit's eight recommendations with “proceed with your recommendations” after the two behavior changes below were explicitly proposed. F33 and F34 record those choices individually; the remaining edits clarify existing guarantees or expose unanswered decisions, not new architecture choices.

### F33 — Isolate archive and history damage from current-value loss

**Decision:** Archive-only damage does not invalidate an intact stored mean or decoded measurements, and damage to a historical reading does not clear an intact current reading. Mark and retain the affected archive or reading; only authoritative damage to the current reading leaves an item without a current value, with no automatic historical promotion.

**Amends:** R2.9, R5.5, Vocabulary, E4; adds archive state E31 and R5.5a–c acceptance cases. F10/F11/F25 remain authoritative: archived vendor bytes are not the canonical value; export handling of an unreadable archive remains an explicit open decision (OQ 21) rather than silently substituting empty bytes.

**Clarified 2026-09-24 (F51, under [the Collection Mode PRD's F23](../collection-mode/prd-collection-mode-fences.md)):** "only authoritative damage to the current reading" ranks the damage kinds against one another; the operator's Flag ([the capture PRD's R5.6](../capture-mode/prd-capture-mode.md#5-per-scan-failure-and-the-consecutive-failure-guard)) is the other way an item's current value is removed, as R2.9 now says. The damage rules above are unchanged. Peer review pending.

### F34 — Pausing capture does not permit switching files

**Decision:** Refuse opening another file while a capture session is active, paused or halted, naming the session and preserving its state. With no in-flight capture, close the original file before opening the selected one; an interrupted session remains persisted in its own file for later resume.

**Amends:** F22(b), R1.3, E22, R7.3a/b and DJ3. The approved recommendation explicitly covered active and paused capture; halted capture retains the same in-flight session under Capture R3.5/R7.14, so the same gate applies. Move and re-read scheduling remain OQ 19.

**Owner confirmed 2026-09-17 (F45):** [Decision 10](https://github.com/vinnyp/spectro-capture/pull/17#issuecomment-5725041707) explicitly extends F34 to halted capture; End session is the halted route out. F44 subsequently changes target-check ordering; F38 settles the move/re-read interim policy.

### F35 — Agent-readable contract and existing-rule clarification

**Decision:** Preserve stable IDs, priorities, anchors and owner decisions; remove repeated persona narrative, local-machine governance dependencies and review disposition text from implementation cells; release and alignment status are stated once for the requirement tables. Use lettered subrows and acceptance tables for measurement operations, regeneration, recovery, deletion and fixtures; a subrow inherits its parent's priority, release and status.

**Clarifications:** R6.3 already ends undo on quit, crash and file close; E8/E14, OQ 10 and DJ4 now say the same. Correct the external-write impossibility claim using [SQLite's primary documentation](https://sqlite.org/pragma.html#pragma_data_version), without selecting polling, notifications or a library. OQ 17–21 expose previously unspecified product behavior and preserve ADR ownership; they are not settled by this approval. Implementation PR cells stay empty until code lands; earlier review provenance remains in git and the original review log.

**Amended 2026-09-17:** F36–F46 below settle the review’s owner choices; F46 restores per-row Status and Commit PR, F38/F39/F37 establish interim behavior for OQ 19/18/21. OQ 17 and OQ 20 remain open as before.

## Retired ID map

E32 and R7.3l are retired under F47 in PR #17, never reused; automatic R5.5f regeneration replaces their prompt.

**Retired row IDs.** Retired under fence F30, never reused, each now [the export PRD](../export/prd-data-export.md)'s at the ID named: R4.1 → [its R1.1](../export/prd-data-export.md#1-what-the-export-contains); R4.3 → [its R1.2](../export/prd-data-export.md#1-what-the-export-contains); R4.4 → [its R1.3](../export/prd-data-export.md#1-what-the-export-contains); R4.2 → [its R2.1](../export/prd-data-export.md#2-columns-names-dialect-and-the-version); R4.6 → [its R2.2](../export/prd-data-export.md#2-columns-names-dialect-and-the-version); R4.7 → [its R2.3](../export/prd-data-export.md#2-columns-names-dialect-and-the-version); R4.8 → [its R2.4](../export/prd-data-export.md#2-columns-names-dialect-and-the-version); R4.9 → [its R2.5](../export/prd-data-export.md#2-columns-names-dialect-and-the-version); R4.5 → [its R3.1](../export/prd-data-export.md#3-when-an-export-cannot-finish); R7.9 → [its R4.1](../export/prd-data-export.md#4-verifiability); R7.6a → [its R4.4a](../export/prd-data-export.md#surfaces); M3 → [its M1](../export/prd-data-export.md#success-metrics); OQ 8, 9, 11, 16 → [its OQ 1–4](../export/prd-data-export.md#open-questions); E6, E7, E17, E18 → [its E1–E4](../export/prd-data-export-copy.md#error--state-copy); DJ1 → [its EJ1](../export/prd-data-export-journeys.md#ej1-export-the-collection). §4's number goes with them; [R7.6](prd-data-foundation.md#7-verifiability), [R7.7](prd-data-foundation.md#7-verifiability) and [R7.8](prd-data-foundation.md#7-verifiability) stay ([its Traceability](../export/prd-data-export.md#traceability)).

## Sibling amendment map

Original hand-offs retained for traceability; landed amendments are tracked in [post-lock](../post-lock.md#landed-sibling-amendments), and each linked sibling row owns its current wording. The active inbound/outbound contract remains in the PRD's obligations tables.

| Target | Amendment recorded at lock | Rows here |
| :--- | :--- | :--- |
| Capture Mode | [Its E29](../capture-mode/prd-capture-mode-copy.md#error--state-copy) gains the correction default — a capture-time re-scan carries the supersession reason correction-unconfirmed, the question never asked mid-loop — post-lock there (fences F3, F12) | [R2.4](prd-data-foundation.md#2-canonical-value-and-version-history) |
| Capture Mode | [Its OQ 19](../capture-mode/prd-capture-mode.md#open-questions) closes on fences F2 and F14 — one file, one open at a time, its location asked for on first launch — post-lock there | [R1.1](prd-data-foundation.md#1-the-file-the-user-owns), [R1.3](prd-data-foundation.md#1-the-file-the-user-owns), [R1.8](prd-data-foundation.md#1-the-file-the-user-owns) |
| Capture Mode | Post-lock amendments there: [its Data Foundation obligation row](../capture-mode/prd-capture-mode.md#inherited-obligations) gains its R4.5 and its R1.10, and its R1.9 queue-order obligation names Data Export beside Data Foundation ([the export PRD's R2.3](../export/prd-data-export.md#2-columns-names-dialect-and-the-version)) | [R2.1](prd-data-foundation.md#2-canonical-value-and-version-history), [R3.1](prd-data-foundation.md#3-derived-values-and-gamut-honesty) |
| Capture Mode | Post-lock amendment there: [its OQ 24](../capture-mode/prd-capture-mode.md#open-questions) closing toward a shipped build re-opens [R6.5](prd-data-foundation.md#6-deletion-and-privacy)'s inventory (fence F32) | [R6.5](prd-data-foundation.md#6-deletion-and-privacy) |
| Device Management | Post-lock amendments there: [its UJ1](../device-management/prd-device-management-journeys.md#uj-1-first-run) and [its UJ1.2](../device-management/prd-device-management-journeys.md#uj-12-first-run-with-no-hardware-contributor) gain a first step — choosing where the file lives before license activation (fence F14) — which [its M2](../device-management/prd-device-management.md#success-metrics)'s start event spans, and [its R6.20](../device-management/prd-device-management.md#6-mock-device-layer)'s declared-state harness gains the file-location axis; the `simulated` column amendment is [the export PRD's](../export/prd-data-export.md#inherited-obligations) | [R1.8](prd-data-foundation.md#1-the-file-the-user-owns) |
| Device Management | Post-lock amendment there: [its hardware spike's scope](../device-management/prd-device-management.md#legend) gains reading the instrument's reported wavelength grid ([the export PRD's OQ 4](../export/prd-data-export.md#open-questions)), measuring, storing, reconstructing and comparing a raw payload and recording which spaces the toolkit supplies (OQ 15), the vendor analytics recipient and whether they can be switched off (OQ 12), and sourcing the published reference set (OQ 6) | [R1.6](prd-data-foundation.md#1-the-file-the-user-owns), [R3.1](prd-data-foundation.md#3-derived-values-and-gamut-honesty), [R6.6](prd-data-foundation.md#6-deletion-and-privacy), [R7.5](prd-data-foundation.md#7-verifiability) |
| Inventory Import | Post-lock amendment there: [its R2.2](../import/prd-inventory-import.md#2-target-mapping-and-the-matching-rule) gains that a later import into a collection keeps the existing columns' positions and appends its new ones after them | [R1.2](prd-data-foundation.md#1-the-file-the-user-owns) |
| Vision | Post-lock amendment there: [the v1 feature list](../vision.md#v1)'s "raw payload canonical" phrasing is the stored mean, the payload archived beside it (fence F10) | [R2.1](prd-data-foundation.md#2-canonical-value-and-version-history) |

## PR #17 review decisions (2026-09-17)

Source: [the owner’s eleven decisions](https://github.com/vinnyp/spectro-capture/pull/17#issuecomment-5725041707), one fence per numbered decision in order. These supersede conflicting clauses in the initial PR #17 amendment; architecture remains ADR-owned.

### F36 — Keep the general salvage promise (2026-09-17)

**Decision:** Salvage remains complete for readable data, opens read-write and names every resolution; two current readings remain the worked example. Per-invariant rules, including equal record times, belong to ADR-0003 without becoming a product precondition to salvage; the rejected SR7-4 finding stands.

**Why:** E5 must retain a usable recovery path; schema mechanics must not narrow the owner’s general guarantee.

**Rows:** R5.5d, R7.3; post-lock ADR-0003 item.

**Clarified 2026-09-26 ([the Nix Toolkit import review's round 5](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-5--delta-verification-2026-09-26); F64 and R5.5d):** The worked example's two current readings are ordered by R2.1's per-item sequence, so equal record times no longer decide it; the later stays current with reason correction-unconfirmed and the other as its predecessor, and a pair whose later reading is imported, and not a restore, ends as R2.3j leaves it. Other invariants, a repeated per-item sequence among them, stay ADR-0003's.

### F37 — Export unavailable archives and quarantined readings (2026-09-17)

**Decision:** The canonical export runs with an empty payload cell for each unavailable sample archive, with no new CSV column or state token. Any quarantined reading emits `quarantined`, including superseded history; OQ 21 stays open only for a distinct archive mark with an Export R2.5 version bump.

**Why:** Preserve export of intact canonical data while giving agents one interim byte-level contract and one state precedence rule.

**Rows:** R5.5a–c, outbound Data Export obligation, R7.7i, OQ 21; DE R1.1/R1.3 and F14.

### F38 — Disable move and re-read during in-flight capture (2026-09-17)

**Decision:** Move and explicit re-read are disabled during active, paused or halted capture; an interrupted session does not disable them. E9/E15/E25/E27 explain the disabled actions and point to ending the session; OQ 19 remains open only for a future mid-capture policy.

**Why:** An enabled action must have defined behavior; preserve in-flight scans until the existing End session path has run.

**Rows:** R1.5/R1.9, R7.3c/j, OQ 19, E9/E15/E25/E27, DJ3.

### F39 — Guarantee a read-only browsing floor (2026-09-17)

**Decision:** E1/E16/E23/E24 read-only opens show at least the item list, each item’s canonical value and history marks; E1 and E23 retain their browsing promises. OQ 18 concerns additional export, salvage and search capabilities, with existing export restrictions and requested salvage rules still applying.

**Why:** Read-only is a useful view of owned data, not merely an update dialog.

**Rows:** R5.3, R7.3f, OQ 18, E1/E23 and copy variants, DJ3.

### F40 — Restore copies samples and recomputes derived values (2026-09-17)

**Decision:** A restored reading has its own copies of the source samples and decoded values, and fresh six-space derivations at current DERIVATION_VERSION under the collection reference, subject to R3.5’s non-spectral/missing-condition rules. The source reading and its sets remain unchanged.

**Why:** A restore must be independently readable and must not revive stale derivations or mutate the source history.

**Rows:** R2.3f, R7.7g, DJ2.

**Clarified 2026-09-25 (F52, under [the Collection Mode PRD's F83](../collection-mode/prd-collection-mode-fences.md)):** the restored reading also carries its source's acquiring-device snapshot, agreement verdict and recorded spread, as R2.3f now says. Peer review pending.

### F41 — Hand-author the live-kind fixture (2026-09-17)

**Decision:** The non-simulated snapshot is a hand-authored live-kind snapshot checked into the fixture file; the simulated snapshot still uses Device R6.9’s mock fields. No Device Management change or mock kind override is introduced.

**Why:** Exercise `sc_simulated=false` without hardware while keeping simulated and live device identities separate.

**Rows:** R7.7, R7.7e; post-lock live-kind golden.

**Clarified 2026-09-18 ([owner decisions](https://github.com/vinnyp/spectro-capture/pull/19#issuecomment-5734032720)):** Device F20 / R6.30 adds a test-only live-kind flow double for authorization coverage; generated readings still carry simulated acquiring snapshots, so F41's hand-authored fixtures remain the sole producer of the sc_simulated=false fixture shape. R7.7 records that distinction; the prior no-override statement is historical for the original PR #17 scope.

**Clarified 2026-09-18 ([round-2 owner decisions](https://github.com/vinnyp/spectro-capture/pull/19#issuecomment-5734782540)):** Device F27 makes the R6.30 double simulated-kind in saved records, display, identity and provenance; UJ3-b seeds a separate live-kind saved record through Device R6.20. F41 still supplies hand-authored live measurement snapshots, not generated false-simulated readings.

### F42 — Exclude unreadable measurements from regeneration (2026-09-17)

**Decision:** Do not regenerate a reading whose authoritative measurement is unreadable; retain its sets and stamp. Completion enumerations exclude it and list it separately as excluded by damage.

**Why:** A retained quarantined reading cannot be re-derived and must not make completion permanently unreachable.

**Rows:** R3.3g, R7.1/R7.5, R7.7i, DJ5.

### F43 — Exercise upgrades from the first release (2026-09-17)

**Decision:** Check in a fixture at a fabricated format version below current and exercise a test-only migration path from v1. Cover snapshot-first, successful upgrade, mid-upgrade failure, lossy refusal and insufficient snapshot space; include it in M5’s population.

**Why:** v1 has no real older format, but its migration guarantees need non-vacuous tests before the first format bump.

**Rows:** R7.7/R7.7m, R5.1/R5.2, R7.3, E2/E3/E23/E24, M5.

### F44 — Precheck a switch target before closing the original (2026-09-17)

**Decision:** Run target version, integrity and ownership checks before closing the original file; a failed target leaves the original open and shows the target’s opening state. Only an accepted target proceeds to close the original and open the target; retain one active store and persisted interrupted sessions.

**Why:** Failed selection must not strand the app in an undefined no-file-open state.

**Rows:** R1.3, R7.3b, DJ3; amends F34’s close-first ordering.

### F45 — Ratify halted capture in F34 (2026-09-17)

**Decision:** F34 explicitly includes halted capture; the owner confirms this scope rather than leaving it as an agent inference. E22 names End session as the way out when scanning cannot continue.

**Why:** A halt retains an in-flight session, and an unavailable instrument must not trap the file-switch path behind finishing scans.

**Rows:** F34 dated confirmation; R1.3, R7.3a, E22.

### F46 — Restore per-row status tracking (2026-09-17)

**Decision:** Restore each primary requirement row’s Status column and the Commit PR column name. Keep lettered sub-tables and the shared v1 declaration.

**Why:** Match the other PRDs’ implementation tracking without undoing the useful contract tables.

**Rows:** Legend, Traceability, primary requirement tables.

## PR #17 round-2 decisions (2026-09-17)

Source: [the owner’s three round-2 decisions](https://github.com/vinnyp/spectro-capture/pull/17#issuecomment-5725692929), mapped in order below. F47 supersedes the requested-rebuild behavior introduced in round 1; that earlier review record remains history.

### F47 — Regenerate unreadable derived sets automatically (2026-09-17)

**Decision:** Mark an unreadable persisted derived set whose measurement is intact, and regenerate it automatically under R3.3a at current DERIVATION_VERSION without bumping it; retain the damaged set as superseded and leave canonical selection/reading marks unchanged. Retire E32 and R7.3l, removing their prompt, actions and surface entries; R7.7i asserts the regenerated set is readable and the damaged set retained and marked, with current selection unchanged.

**Why:** A prompt offers no useful alternative and creates inconsistent completion/export outcomes; automatic regeneration restores the existing six-space contract without an export exception or additional matrix row.

**Rows:** R3.3a, R5.4/R5.5f, R7.6g/h, R7.7i, §8, DJ3; retired E32/R7.3l.

### F48 — Restore the deletion statement on the collection confirmation (2026-09-17)

**Decision:** Restore ‘Deleting is the only thing that throws away a reading’ to E14 only; E8 stays unchanged. The copy Variants note explains the asymmetry for never-scanned items.

**Why:** Keep the non-negotiable visible in shipping copy without claiming a never-scanned item has a reading.

**Rows:** E14 and copy Variants; R6.1/R6.2.

### F49 — Track the salvage metric after lock (2026-09-17)

**Decision:** Record M10 (readable readings absent from salvage output, target 0, method R7.3) as a Data Foundation post-lock item. Do not add a metric row to this PRD now.

**Why:** Make the proposed preservation measurement discoverable for the next pass while keeping this amendment scoped to the remaining review fixes.

**Rows:** post-lock Data Foundation list; future M10, no metric row yet.

**Clarified 2026-09-18 ([owner decisions](https://github.com/vinnyp/spectro-capture/pull/19#issuecomment-5734032720)):** Under D9 / Device F23, R1.4's permitted recipients gain user-controlled software-update checks for both live and Demo (Device R2.13), observed through R6.12/R6.17; no item, reading or payload may leave. This records the network-set amendment here because no earlier fence owns that permitted set; F49's salvage decision is unchanged.

**Clarified 2026-09-18 ([round-2 owner decisions](https://github.com/vinnyp/spectro-capture/pull/19#issuecomment-5734782540)):** D13 / Device F27 adds R6.30’s double to R1.4’s live permitted set for authorization traffic, while its persisted/displayed kind and all provenance remain simulated; the software-update and no-file-content rules are unchanged.

**Clarified 2026-09-18 (Capture agent-build amendment):** The Capture obligation mirror now names F53’s own-session contributions versus cumulative resumed-chain display; R1.1/R7.1 carry persistence/readback, with schema left to ADR-0003. No record is duplicated by inheritance and no new privacy field or session-status enum is selected.

**Clarified 2026-09-18 ([owner decisions](https://github.com/vinnyp/spectro-capture/pull/20#issuecomment-5736642147)):** D2 / Capture F57 ratifies own-session contributions and cumulative summary figures; this mirror note is placed here because no earlier DF fence owns Capture’s session-accounting test obligation, not because salvage changes. The earlier “R1.1/R7.1 carry persistence/readback” overstates R7.1: schema remains ADR-0003 and readback stays on Capture R11.11; its R11.6/R11.11 declaration/readback of counters, partial sets, row evidence and labelled own/cumulative figures is inherited whole by the Capture obligation line, without enlarging R7.1’s list.

**Clarified 2026-09-18 ([round-2 owner decisions](https://github.com/vinnyp/spectro-capture/pull/20#issuecomment-5736942308)):** D11 / Capture F66 corrects summary tallies to collection state at completion/End; only elapsed time and the derived rate use the chain basis, with the numeric basis discriminator in Capture R11.11, not a copy variant. Both obligation cells now name F52/F56 and F53/F57/F66 plus the R11.6/R11.11 test contract; Capture R3.1 durably records the guard counter for reopen readback, while a successor starts at zero and never inherits that counter. Schema remains ADR-0003 and readback remains Capture R11.11.

**Clarified 2026-09-18 ([round-3 owner decision](https://github.com/vinnyp/spectro-capture/pull/20#issuecomment-5738693056)):** D14 / Capture F69 makes the summary rate readback use only the rows captured by that chain over its cumulative elapsed capture time; displayed collection-state tallies are not the numerator. Both Capture/DF obligation cells mirror this distinction under Capture R11.11; schema remains ADR-0003.

## Collection Mode seam amendment (2026-09-24)

Source: the owner's decisions D7 and D9 in the Collection Mode PRD's post-fill adjudication of 2026-09-24, recorded there as [its F7 and F9](../collection-mode/prd-collection-mode-fences.md). This fence carries only the Data Foundation halves of those two seams; peer review pending.

### F50 — A selection-scale delete confirmation, and a renamed column's stored name (2026-09-24)

**Authority:** [the Collection Mode PRD's F7 and F9](../collection-mode/prd-collection-mode-fences.md) — owner decisions D7 and D9, post-fill adjudication 2026-09-24.

**Decision:** (1) Under the Collection Mode PRD's F7, this PRD gains E33, a selection-scale delete confirmation beside E8 and E14 that Collection Mode's Delete selected opens: it states the selected swatch count and their current and earlier readings, offers export first and cancel as E8 does, and never makes delete the default; R6.2 names it and R7.6o tests it. (2) Under the Collection Mode PRD's F9, a Collection Mode rename changes an imported column's stored name, keeping its position and values, and a later import matches the stored name; R1.2 says so, its first-seen names now meaning first-seen until renamed. Nothing else in deletion, the undo window or the import rules changes, and R1.2 and R6.2 keep their alignment.

**Not decided:** which scope E33's Export first opens — the collection holding the selection, or the selection itself, which the export PRD's R1.1a does not offer — is neither Collection Mode's F7 nor this fence's, and goes back to the owner.

**Why:** Collection Mode's F7 makes its Delete selected buildable only if a selection-scale confirmation exists here, and its F9 closes the post-lock question of whether a rename moves the stored name.

**Rows:** R1.2, R6.2, R7.6o, E33, the Collection Mode inbound obligation line, the copy header and Variants note, DJ4, DJ5.

**Clarified 2026-09-24 ([the Collection Mode PRD's F24](../collection-mode/prd-collection-mode-fences.md), its approved loose end (i), second post-fill adjudication):** The "Not decided" point above is settled: E33's Export first opens the export of the whole collection holding the selection, at [the export PRD's R1.1a](../export/prd-data-export.md#row-selection-and-fields) collection scope — that PRD offers no selection scope, and Collection Mode's F3 excludes exporting a selection. R6.2 says so and keeps its alignment, R7.6o lists it and DJ4 asserts it; [the export PRD's F31](../export/prd-data-export-fences.md) adds E33 to its list of the confirmations offering export first. E33's copy is unchanged. Peer review pending.

**Clarified 2026-09-25 (F52, under [the Collection Mode PRD's F56](../collection-mode/prd-collection-mode-fences.md)):** E33's export-first sentence now says it saves all of ⟨collection⟩, these swatches included; what Export first opens is unchanged. Peer review pending.

**Clarified 2026-09-25 (F53, under [the Collection Mode PRD's F3 and F7](../collection-mode/prd-collection-mode-fences.md)):** OQ 20's question now names E33's selection scope and two deletes in one open-file lifetime, as the Collection Mode PRD's OQ 10 does; no row changes. Peer review pending.

**Clarified 2026-09-25 (F54, under [the Collection Mode PRD's F147](../collection-mode/prd-collection-mode-fences.md)):** OQ 20's question now also asks what an undoable delete of ROWS_CEILING items holds in memory, or writes when the window ends, on the Collection Mode PRD's OQ 1 Mac; R6.3 stays gated and no row changes. Peer review pending.

**Closed 2026-09-26 ([final review](https://github.com/vinnyp/spectro-capture/pull/21#pullrequestreview-5328170977)):** For F50–F63, peer review closed 2026-09-26 (PR #21), F50–F58 at the Collection Mode PRD's first lock on 2026-09-25 and F59–F63 at its re-locks of 2026-09-26; re-locked on merge.

## Collection Mode seam amendment, second pass (2026-09-24)

Source: the owner's decision D14 in the Collection Mode PRD's second post-fill adjudication of 2026-09-24, recorded there as [its F23](../collection-mode/prd-collection-mode-fences.md). This fence carries only the Data Foundation half of that seam; peer review pending.

### F51 — The operator's Flag is the other way an item's current value is removed (2026-09-24)

**Authority:** [the Collection Mode PRD's F23](../collection-mode/prd-collection-mode-fences.md) — owner decision D14, second post-fill adjudication 2026-09-24.

**Decision:** R2.9 names the operator's Flag on a captured item ([the capture PRD's R5.6](../capture-mode/prd-capture-mode.md#5-per-scan-failure-and-the-consecutive-failure-guard)) beside damage to the current reading as a way an item's current value is removed. The flagged reading stays readable in history, so restoring it or any other readable earlier reading follows R2.3f — Collection Mode's R5.5 offers that restore on a flagged item, making it captured again — and a fresh set follows R2.3a. Nothing else in R2.9, the damage classification or the measurement operations changes, and R2.9 keeps its alignment.

**Why:** R2.9's "only damage to the current reading removes the item's current value" already contradicted the capture PRD's R5.6 Flag demotion, which Collection Mode's F23 names as the authority to fix here.

**Rows:** R2.9, DJ2, the Collection Mode inbound obligation line, the dated F33 clarification.

## Collection Mode round-1 amendment (2026-09-25)

Source: the owner's round-1 decisions in the Collection Mode PRD's adjudications of 2026-09-24 and 2026-09-25, recorded there as [its F32, F35, F50, F56, F73, F83 and F85](../collection-mode/prd-collection-mode-fences.md). This fence carries only the Data Foundation halves of those decisions; peer review pending.

### F52 — The item delete names its collection, removed text leaves the bytes, identity survives a code change, a restore keeps its provenance (2026-09-25)

**Authority:** [the Collection Mode PRD's F32, F35, F50, F56, F73, F83 and F85](../collection-mode/prd-collection-mode-fences.md) — owner decisions D18 and D21 of its round-1 adjudication, 2026-09-24 (F32, F35); its approved recommendations 15 and 21, the same day (F50, F56); and its approved round-1 recommendations 16, 26 and 28, 2026-09-25 (F73, F83, F85).

**Decision:** (1) Under the Collection Mode PRD's F32, E8's headline names the item's collection, and R7.6k asserts it. (2) Under its F35, "unrecoverable by an outside reader" in R2.3 and R6.2a includes anyone reading the file's bytes, not only a SQL reader; ADR-0003 chooses the mechanism, and R7.2 and DJ4 read the bytes. (3) Under its F50, an item keeps its identity when Collection Mode R4.4 changes its Swatch Code, a reader at SQLITE_READER_FLOOR seeing the same item under the new code, and each collection's column-visibility choice (Collection Mode R2.10) is kept with it; R1.2 and DJ5 say so. (4) Under its F56, E33's export-first sentence says it saves all of ⟨collection⟩. (5) Under its F83, a restore (R2.3f) carries its source's acquiring-device snapshot, agreement verdict and recorded spread, and DJ2 asserts it. (6) Under its F73, this PRD owes Collection Mode a named state for a write refused because permission to the file was lost, recorded on the Collection Mode obligation line. (7) Under its F50 and F85, ADR-0003 takes as inputs item identity kept across a code change, per-collection column visibility, a renamed column's stored name, (2)'s byte-level rule, and that the file keeps no display-relative cannot-show mark, the gamut-clipped flag (R3.4) being the only gamut mark stored. R1.2, R2.3, R2.3f, R6.2a, R7.2 and R7.6k keep their alignment; E8 and E33 change wording only.

**Not decided:** the permission-lost state's wording and actions, and whether it is a new state or a variant of an existing one, are neither the Collection Mode PRD's F73 nor this fence's, and go back to the owner.

**Why:** each is the Data Foundation half of a Collection Mode decision about what the file keeps or how a confirmation reads; this PRD owns the file and those confirmations.

**Rows:** R1.2, R2.3, R2.3f, R6.2a, R7.2, R7.6k, E8, E33, DJ2, DJ4, DJ5, the Collection Mode inbound obligation line, the dated F17, F40 and F50 clarifications.

**Clarified 2026-09-25 (F53, under [the Collection Mode PRD's F56 and F101](../collection-mode/prd-collection-mode-fences.md)):** E33's ‹P1› sentence now also opens by saying Export first saves all of ⟨collection⟩, these swatches included, so the scope survives when the ‹until P1› sentences are withdrawn; and the **Not decided** point about (6) is closed — F53 supplies the permission-lost state as E34. Peer review pending.

## Collection Mode round-2 amendment (2026-09-25)

Source: the owner's round-2 decisions in the Collection Mode PRD's adjudication of 2026-09-25, recorded there as [its F101, F102, F103, F121 and F127](../collection-mode/prd-collection-mode-fences.md), with the editorial halves its round-2 fix pass carries under its F3, F7 and F56. This fence carries only the Data Foundation halves of those decisions; peer review pending.

### F53 — A permission-lost state, removed text gone from the moment the write lands, the awaiting-answer wording, and the stored flag's test (2026-09-25)

**Authority:** [the Collection Mode PRD's F101, F102, F103, F121 and F127](../collection-mode/prd-collection-mode-fences.md) — owner decisions D25, D26 and D27 of its round-2 adjudication, 2026-09-25 (F101, F102, F103), and its approved round-2 recommendations 23 and 29 the same day (F121, F127).

**Decision:** (1) Under the Collection Mode PRD's F101, this PRD gains E34, the state for a write refused because the app lost permission to the file: it says the file can't be saved to, that nothing in it changed, and how to recover, offering Try again, which R7.3j retries; R1.10 names it, R7.6p tests it and DJ3 exercises it. Its wording goes through the same peer review. (2) Under its F102, text R2.3 and R6.2a put beyond an outside reader is gone from the moment its write lands, from the file and from any journal, log, index or other file beside it, while the file is open and after a crash; ADR-0003 still chooses the mechanism. (3) Under its F121, E11 and E26 say an earlier reading is marked as awaiting your answer, the Collection Mode PRD's name for that mark, in place of "not yet settled". (4) Under its F127, R3.4's flag is tested as the Collection Mode PRD's OQ 7 interim tests a display's gamut, at sRGB, and ADR-0003 takes that as an input. (5) Under its F103, ADR-0003 takes as an input that file-wide search may use an index or a scan, either one under (2)'s rule. (6) Editorial, under its F3, F7 and F56: OQ 20's question names E33's selection scope and two deletes in one window, and E33's ‹P1› sentence keeps its export-first scope clause. R1.10, R2.3, R3.4, R6.2a and R7.3j keep their alignment; E11, E26 and E33 change wording only; E34 and R7.6p are ready for alignment.

**Why:** each is the Data Foundation half of a Collection Mode decision about a state this PRD owns, what the file keeps, or how its copy reads; this PRD owns the file, its failure states and its copy.

**Rows:** R1.10, R2.3, R3.4, R6.2a, R7.3j, R7.6p, E11, E26, E33, E34, OQ 20, DJ3, the Collection Mode inbound obligation line, the dated F4, F17, F50 and F52 clarifications.

**Clarified 2026-09-25 (F54, under [the Collection Mode PRD's F138](../collection-mode/prd-collection-mode-fences.md)):** (1)'s recovery is cause-neutral and E34 gains Choose the file again, as F54 records; "nothing in it changed" now holds on a local disk, as E15 says. Peer review pending.

**Clarified 2026-09-25 (F54, under [the Collection Mode PRD's F146](../collection-mode/prd-collection-mode-fences.md)):** (4) is superseded: R3.4 states the flag's test itself rather than pointing at the Collection Mode PRD's OQ 7 interim, as F54 records. Peer review pending.

**Clarified 2026-09-25 (F54, under [the Collection Mode PRD's F87, F88, F102 and F103](../collection-mode/prd-collection-mode-fences.md)):** (2)'s "any journal, log, index or other file beside it" is the files the app keeps beside it, in R2.3 and R6.2a; R6.2 and R6.2d now say unrecoverable "as R6.2a states"; and (5)'s search input reads: file-wide search uses an index kept in the file, or a scan, either one under (2)'s rule, answering every keystroke from the first character within the Collection Mode PRD's BROWSE_RESPONSE_BUDGET from the first rows of a cold open, and within its R8.1f's, R8.1g's and R8.2's budgets on its OQ 1 Mac, the Collection Mode review log's round-3 performance measurements being the evidence; it mandates neither. Peer review pending.

**Clarified 2026-09-25 ([the Collection Mode PRD's F171](../collection-mode/prd-collection-mode-fences.md), its approved round-4 recommendation 11):** (5)'s search input, as the line above restates it, answers within the Collection Mode PRD's R8.1c's, R8.1f's, R8.1g's and R8.2's budgets on its OQ 1 Mac, adding R8.1c's, and its evidence is the performance lens's round-3 measurements, on an M5 Max, in the Collection Mode review log; F56 (4) records it. Peer review pending.

## Collection Mode round-3 amendment (2026-09-25)

Source: the owner's round-3 decisions in the Collection Mode PRD's adjudication of 2026-09-25, recorded there as [its F138, F139, F146, F147 and F150](../collection-mode/prd-collection-mode-fences.md), with the editorial and hand-off halves its round-3 fix pass carries under its F87, F88, F102 and F103. This fence carries only the Data Foundation halves of those decisions; peer review pending.

### F54 — E34's cause-neutral recovery, R3.4's own flag test, what outside readers may delay, and the ADR hand-offs (2026-09-25)

**Authority:** [the Collection Mode PRD's F138, F139, F146, F147 and F150](../collection-mode/prd-collection-mode-fences.md) — owner decision D31 of its round-3 adjudication, 2026-09-25 (F138), and its approved round-3 recommendations 5, 12, 13 and 16 the same day (F139, F146, F147, F150).

**Decision:** (1) Under the Collection Mode PRD's F138, E34 is cause-neutral: SpectroCapture can no longer change the file at ⟨path⟩, so the last thing done wasn't saved; on a local disk the file is as it was; the user checks that the file isn't locked and that SpectroCapture is still allowed to change it, then tries again — or chooses the file again. E34 gains Choose the file again, which R7.3j runs as R7.3d's file-selection path for the file, and R7.6p and DJ3 test it; ADR-0007 takes as an input to name where access is restored once it picks the permission model. (2) Under its F150, ADR-0007 also takes as an input an owner reading once it lands: revoke access the way the chosen model allows, follow E34, and confirm it recovers. (3) Under its F146, R3.4 states the flag's test itself — zero tolerance after Bradford adaptation to sRGB's white, the adaptation the sRGB derivation uses, under relative colorimetric — and closing the Collection Mode PRD's OQ 7 changes it only through a fence here and a new derivation version; ADR-0003's flag input follows. (4) Under its F139, R1.5's help docs say reading the file elsewhere during capture may delay saves, and ADR-0003 takes as an input that a text-removing write lands once every earlier read has ended, this app's own reads — an export, the Collection Mode PRD's All items cold load — running in transactions no longer than its BROWSE_RESPONSE_BUDGET, so capture saves never wait on a removing edit. (5) Under its F147, OQ 20's question adds what an undoable delete of ROWS_CEILING items holds in memory, or writes when the window ends, on the Collection Mode PRD's OQ 1 Mac. (6) Editorial and hand-off only, under its F87, F88, F102 and F103: R2.3 and R6.2a name the files the app keeps beside the file; R6.2 and R6.2d say unrecoverable as R6.2a states; and F53 (5)'s search input gains the index's place and the budgets that decide it, as the dated line under F53 records. R1.5, R2.3, R3.4, R6.2, R6.2a, R6.2d and R7.3j keep their alignment; E34 is reworded and gains an action, ready for alignment with R7.6p.

**Not decided:** whether Choose the file again also retries the unsaved write or leaves that to Try again; the Collection Mode PRD's F138 adds the action without deciding it, so it goes back to the owner.

**Why:** each is the Data Foundation half of a Collection Mode decision about a state this PRD owns, what the file keeps or its help docs say, or an input this PRD hands the ADRs.

**Rows:** R1.5, R2.3, R3.4, R6.2, R6.2a, R6.2d, R7.3j, R7.6p, E34, OQ 20, DJ3, the Collection Mode inbound obligation line, the dated F50 and F53 clarifications.

**Clarified 2026-09-25 ([the Collection Mode PRD's F151](../collection-mode/prd-collection-mode-fences.md), owner decision D32):** the **Not decided** point above is settled: Choose the file again restores access only. R7.3j runs the file-selection path and retries no write, E34 staying up for Try again; E34's body says to try again after choosing the file, R7.6p lists that no write is retried, and DJ3 asserts it. R7.3j keeps its alignment. Peer review pending.

**Clarified 2026-09-25 ([the Collection Mode PRD's F152](../collection-mode/prd-collection-mode-fences.md), owner decision D33):** while a Collection Mode bulk write or delete runs ([its R8.1f and R8.1g](../collection-mode/prd-collection-mode.md#8-operating-envelope-and-quality-attributes)), no re-read or move starts: R1.5 and R1.9 show each disabled until the write lands, DJ3 asserts it, and the Collection Mode inbound obligation line records it. R1.5 and R1.9 keep their alignment. Peer review pending.

**Clarified 2026-09-25 ([the Collection Mode PRD's F153](../collection-mode/prd-collection-mode-fences.md), owner decision D34):** E34's body adds "OK leaves it unsaved.", so the one OK here that leaves a change unsaved says so; R7.6p lists it. Peer review pending.

**Editorial compaction 2026-09-25 (process rules 7, 11 and 14; no owner decision):** the PRD body went from 8,917 to 8,218 words by rule 14's method, removing only rule-free prose: fence and provenance cites inside row cells, preambles and obligation cells, with the Capture inbound cell's sibling-fence names kept as the dated 2026-09-18 clarifications record; text restating a rule that an ID row, a sibling row or a fence already states, the cite kept, in preambles, trailers, obligation cells and Open Questions decision cells; lists duplicated where one cites the other; and verbose phrasing of the F50–F54 additions, the status line and the Collection Mode inbound line among them. No rule, row scope, constant, ID, priority or status changed, and R1.2 is back to two sentences. The body is still 218 words over F21's 8,000 budget, and that goes back to the owner.

**Corrected 2026-09-25 ([the Collection Mode PRD's F159 and F165](../collection-mode/prd-collection-mode-fences.md)):** the compaction changed meaning in two rows despite the sentence above — R7.3j lost 'which restores access only' (F151), and R3.4's appositive came to name sRGB's white rather than the adaptation (F146, (3) above); both are restored, R7.3j under the F159 line below and R3.4 under the F165 line. Peer review pending.

**Clarified 2026-09-25 ([the Collection Mode PRD's F159](../collection-mode/prd-collection-mode-fences.md), owner decision D40):** Choose the file again restores access only, as the F151 line above settled and the compaction dropped from R7.3j; a different file chosen in its picker opens nothing, switches nothing and retries nothing, E34 staying up and the open file open. R7.3j says so, R7.6p lists it and DJ3 asserts it; R7.3j keeps its alignment. Peer review pending.

**Clarified 2026-09-25 ([the Collection Mode PRD's F165](../collection-mode/prd-collection-mode-fences.md), its approved round-4 recommendation 5):** R3.4 names the adaptation its sRGB derivation uses, as (3) above states and the compaction had blurred, and R6.2a leaves R5.8's copies aside from the files the app keeps beside the file. Both keep their alignment. Peer review pending.

**Clarified 2026-09-25 ([the Collection Mode PRD's F174](../collection-mode/prd-collection-mode-fences.md), owner decision D43):** "the file" in R7.3j's file-selection path is the open file wherever it now is, renamed or moved included; a copy or any other file changes nothing, as the F159 line above settled. R7.3j says so and DJ3 asserts it; R7.3j keeps its alignment. Peer review pending.

## Collection Mode budget decision (2026-09-25)

### F55 — Word budget 8,200 (2026-09-25)

**Authority:** [the Collection Mode PRD's F154](../collection-mode/prd-collection-mode-fences.md) — owner decision D35, 2026-09-25. Question: "Data Foundation's body is 8,218 words against its 8,000 budget (its F21) after an editorial compaction removed ~700 words. The remaining excess is rule text you ratified in this PR (identity across code changes, the byte rule, E34, bulk-write holds). A further ~48 words can come from shortening sibling-cite labels. How do we close the rest?" Chosen: **Raise DF to 8,200** — "Shorten the cite labels (−48 → ~8,170) and raise DF's budget to 8,200 under a dated DF fence recording that the growth is owner-ratified rule text from this PR. Precedent: you raised DF's budget twice before (7,000→7,500→8,000). Note: the agent-PRD format's own rule says budgets are never raised — this is you overriding it for DF on the record." Not chosen: splitting the Data Foundation PRD; moving the Collection-Mode-driven rules into Collection Mode rows.

**Decision:** This PRD's body budget is 8,200 words, counted by rule 14's method. The owner raised it, overriding the agent-PRD format's rule that a budget is never raised, because the growth over 8,000 is rule text the owner ratified in the Collection Mode change (F50–F54) after an editorial compaction had removed every rule-free word it could; the sibling-cite labels were shortened the same day ("the capture PRD's R1.9" → "Capture R1.9", likewise Device, Export and Import), no rule changing. The body stands at 8,164.

**Why:** splitting the document or moving file-owned rules into Collection Mode would cost more clarity than 164 words of budget.

**Rows:** F1's amended text; no requirement row.

**Clarified 2026-09-25 ([the Collection Mode PRD's F160](../collection-mode/prd-collection-mode-fences.md), owner decision D41):** the budget is 8,300 words — a second owner override of the agent-PRD format's never-raise rule, recorded here, so the round-4 rule text lands without further trimming; the body stood at 8,164 when it was raised. After the round-4 fix pass the body stands at 8,292, within the budget: every addition was tightened to its shortest form with the same meaning, and the status line's Collection Mode clauses — status bookkeeping, not rule text — became one clause citing F50–F54 and F56, whose Decision and Rows fields name every row, state and question the old clauses listed; no rule was trimmed.

**Clarified 2026-09-25 ([the Collection Mode PRD's F177](../collection-mode/prd-collection-mode-fences.md), owner decision D46):** the budget is 8,400 words — a third owner override of the agent-PRD format's never-raise rule, recorded here, so the round-5 rule text and the seam line the lock checks require land without trimming; the body stood at 8,292 when it was raised. After the round-5 fix pass the body stands at 8,381, within the budget: every addition landed at its drafted tightest form, and no rule was trimmed.

**Clarified 2026-09-26 ([the Collection Mode PRD's F214](../collection-mode/prd-collection-mode-fences.md), owner decision D68):** the budget is 8,450 words — a fourth owner override of the agent-PRD format's never-raise rule, recorded here, so R6.2a's F60 wording and the two fence-range cite fixes land without trimming an aligned row.

**Clarified 2026-09-26 ([the Collection Mode PRD's F220](../collection-mode/prd-collection-mode-fences.md), owner decision D74):** the budget is 8,480 words — a fifth owner override of the agent-PRD format's never-raise rule, recorded here, so R6.2a states the main-file wipe's full-volume retry (the Collection Mode PRD's F219).

**Clarified 2026-09-26 ([the Inventory Import PRD's F80](../import/prd-inventory-import-fences.md), owner decision N12):** the budget is 8,600 words — a sixth owner override of the agent-PRD format's never-raise rule, recorded here, so the Nix Toolkit mirror (F64) lands in plain words.

**Clarified 2026-09-26 ([the Inventory Import PRD's F91](../import/prd-inventory-import-fences.md), owner decision N23):** the budget is 8,900 words — a seventh owner override of the agent-PRD format's never-raise rule, recorded here, so round 1's fixes to the Nix Toolkit mirror (F66–F69) land in plain words.

## Collection Mode round-4 amendment (2026-09-25)

Source: the owner's round-4 decisions in the Collection Mode PRD's adjudication of 2026-09-25, recorded there as [its F155, F156, F157 and F158](../collection-mode/prd-collection-mode-fences.md), and its approved round-4 recommendations 6, 10 and 11, recorded as [its F166, F170 and F171](../collection-mode/prd-collection-mode-fences.md). Its F159 (R7.3j, R7.6p, DJ3) and F165 (R3.4, R6.2a) land as dated lines under F54 here, and its F160 under F55 and F21. This fence carries only the Data Foundation halves of those decisions; peer review pending.

### F56 — One one-writer rule, a deferred wipe and its notice, an export read as one snapshot, and the round-4 wording and hand-offs (2026-09-25)

**Authority:** [the Collection Mode PRD's F155–F158, F166, F170 and F171](../collection-mode/prd-collection-mode-fences.md) — owner decisions D36–D39 of its round-4 adjudication, 2026-09-25 (F155–F158), and its approved round-4 recommendations 6, 10 and 11 the same day (F166, F170, F171).

**Decision:** (1) Under the Collection Mode PRD's F155 and F156, R1.11 states the one-writer rule: while a [Collection Mode R8.1f or R8.1g](../collection-mode/prd-collection-mode.md#8-operating-envelope-and-quality-attributes) write, an Import R3.2 commit or an R1.9 move runs, no capture starts or resumes and no re-read or other write to the file starts, each shown disabled, and closing the file, switching and quitting wait for it with its progress showing. R1.3, R1.5, R1.9 and R7.3j's move clause cite it, the first three in place of the Collection Mode R8.1f holds F54's F152 line records, and OQ 17's question adds where regeneration fits in it. (2) Under its F157, R6.2a defers the wipe of removed text while a read begun before the write still runs — this app's own or another app's — until that read ends or the file next opens, nothing waiting on it; R2.3 cites the deferral, E35 says so for another app's read and R7.6q tests it, and R1.5's help docs say reading the file elsewhere may delay wiping removed text. This narrows F53 (2), and supersedes F54 (4)'s help-docs clause that reading during capture may delay saves. (3) Under its F158, an export reads one snapshot, the file as it stood when the export started, and is among the reads R6.2a names; [the export PRD's F32](../export/prd-data-export-fences.md) records it there. (4) ADR-0003's inputs change as the Collection Mode PRD's F157, F158 and F171 decide: a text-removing write is saved without waiting on any read, only its wipe following the end of every earlier read, this app's own cold-load reads running short enough that the wipe follows within that PRD's BROWSE_RESPONSE_BUDGET — replacing F54 (4)'s input, which named an export among this app's short reads; and F53 (5)'s search input adds that PRD's R8.1c budget and names the M5 Max its evidence was measured on, as the F171 line under F53 records. (5) Under its F166, the inbound Collection Mode obligation line mends its R2.9 phrase and names E15, E35, R1.11 and the deferred wipe. (6) Under its F170, E15 and E34 read "before that". (7) Editorial, no owner decision: F1's map line names the current budget and points at F55; E33's Status cell, checked, already reads ⌛️ Ready for Alignment, as the status line records, so it needed no edit; and the status line's Collection Mode clauses become one clause citing F50–F54 and F56, which name the rows, states and questions those clauses listed. R1.3, R1.5, R1.9, R2.3, R6.2a and R7.3j keep their alignment; R1.11, R7.6q and E35 are ready for alignment; E15 and E34 change wording only.

**Why:** each is the Data Foundation half of a Collection Mode round-4 decision about the file, a state or surface this PRD owns, its copy, or an input it hands ADR-0003.

**Rows:** R1.3, R1.5, R1.9, R1.11, R2.3, R6.2a, R7.3j, R7.6q, E15, E34, E35, OQ 17, DJ3, DJ4, the Collection Mode inbound obligation line, the Collection Mode, Capture Mode and Inventory Import outbound lines, F1's map line, the dated F53 clarification.

**Corrected 2026-09-25 ([the Collection Mode PRD's F157, as its F173 line clarifies it, and its F175](../collection-mode/prd-collection-mode-fences.md)):** (2)'s "until that read ends or the file next opens" read as either trigger, which no build can meet while the read still runs; the wipe follows the read's end, on its own or at the first open after, as F57 (1) records. (4)'s "this app's own cold-load reads" narrowed F54 (4)'s bound beyond the Collection Mode PRD's F158, which dropped only the export; every read this app makes but an export, R7.3h's copy and R7.3c's checks keeps that bound, as F57 (3) records. Peer review pending.

## Collection Mode round-5 amendment (2026-09-25)

Source: the owner's round-5 decisions in the Collection Mode PRD's adjudication of 2026-09-25, recorded there as [its F173, F175 and F176](../collection-mode/prd-collection-mode-fences.md), and its approved round-5 recommendations 3, 5, 6, 7, 10 and 11, recorded as [its F180, F182, F183, F184, F187 and F188](../collection-mode/prd-collection-mode-fences.md), with the editorial and testability halves its round-5 fix pass carries under its F7, F156, F157 and F166 and its first lock-check run. Its F174 (R7.3j, DJ3) lands as a dated line under F54 here, and its F177 under F55 and F21. This fence carries only the Data Foundation halves of those decisions; peer review pending.

### F57 — E35's lifetime, the wipe after the read, the app's own long reads, a failing held write, app-made writes deferred, the Collection Mode PRD's F57 mirrored, and the seam line (2026-09-25)

**Authority:** [the Collection Mode PRD's F173, F175, F176, F180, F182, F183, F184, F187 and F188](../collection-mode/prd-collection-mode-fences.md) — owner decisions D42, D44 and D45 of its round-5 adjudication, 2026-09-25 (F173, F175, F176), and its approved round-5 recommendations 3, 5, 6, 7, 10 and 11 the same day (F180, F182, F183, F184, F187, F188).

**Decision:** (1) Under the Collection Mode PRD's F157, as its F173 line clarifies it, and its F184: R6.2a's deferral follows F157's order — while a read begun before the wipe runs, the removed text stays until that read has ended and the file is open, the app wiping it on its own or at the first open after, which corrects F56 (2)'s "until that read ends or the file next opens"; E35 is one notice while any wipe waits on another app's read, OK hiding it until another such write, and it goes on its own once the wipe lands; DJ4 asserts each, its quit-and-reopen run ending the read first, a reopen during the read keeping the text and E35, and a read begun after the delete deferring it too. (2) Under its F180, E35 reads "What was there before". (3) Under its F175, R7.3h's copy and R7.3c's checks defer a wipe as an export does, with no notice, and ADR-0003's input bounds every other read the app makes by that PRD's BROWSE_RESPONSE_BUDGET, restoring F54 (4)'s bound that F56 (4) narrowed to cold loads; DJ4 asserts the copy's and the checks' deferral. (4) Under its F176, a held write that fails cancels the close, switch or quit waiting on it, its failure state staying up; under its F182, a write the app makes by itself during a hold, R5.5a's mark among them, is deferred until the hold ends; R1.11 says both, and DJ3 asserts both. (5) Under its F183, ADR-0003's non-waiting save is scoped to R1.10's local-volume scope; the help-docs line for a network volume is a post-lock item. (6) Under its F187, R6.3 carries the Collection Mode PRD's F57, and OQ 20 carries it through its "As R6.2a–c and R6.3 state": if OQ 20 is still open at v1 release, R6.3 is deferred and v1's deletes are final. (7) Under its F188, the outbound Collection Mode line names every row that PRD's inbound lines cite. (8) Editorial and testability, no owner decision: E33 is marked ‹P1›, appearing when Collection Mode R6.3 lands (its F7), the copy header naming that row; R7.6d lists a move's progress (the Collection Mode PRD's F156 made it shown), and DJ3's held-close line names where each held write's progress is listed; the inbound Collection Mode line's Rows add E15 (its F166) and its fence range reads F52–F57; the status line's range reads F50–F54 and F56–F57. R1.11, R7.6q and E35 stay ready for alignment; R6.2a, R6.3, R7.3j and R7.6d keep their alignment; E33 stays ready for alignment with its mark; E35 changes wording only.

**Why:** each is the Data Foundation half of a Collection Mode round-5 decision or lock-check fix about the file, a state or surface this PRD owns, its copy, an input it hands ADR-0003, or the seam.

**Rows:** R1.11, R6.2a, R6.3, R7.6d, E33, E35, OQ 20, DJ3, DJ4, the copy header, the Collection Mode inbound and outbound lines, the dated F21, F54, F55 and F56 clarifications.

**Corrected 2026-09-25 ([the Collection Mode PRD's F184](../collection-mode/prd-collection-mode-fences.md), read as the deadline R6.2a states):** (1)'s "a read begun after the delete deferring it too" reads "a read begun after the delete permitted to defer it": R6.2a sets when the removed text must be gone at the latest and never requires it to stay, and once a checkpoint copies the delete past the earlier read no build can keep it, so DJ4's second-read line asserts the deadline, E35 up whenever the text is still in the file's bytes, and no longer that the text stays while the later read runs. F58 (2) supersedes (3)'s re-read clause. Peer review pending.

## Collection Mode round-6 amendment (2026-09-25)

Source: the owner's round-6 (pre-lock) decisions in the Collection Mode PRD's adjudication of 2026-09-25, recorded there as [its F190, F191 and F200](../collection-mode/prd-collection-mode-fences.md), and its approved round-6 recommendations 1–4, recorded as [its F193, F194, F195 and F196](../collection-mode/prd-collection-mode-fences.md), with the editorial and testability halves its round-6 fix pass carries under its F7, F157, F164, F173, F174, F176, F182, F184 and F187. This fence carries only the Data Foundation halves of those decisions; peer review pending.

### F58 — E35 goes with its file, a re-read holds writes, a wipe runs beside a held write, the last-change and plural wording, the open-file check beside the long reads, and a later ADR for the undo representation (2026-09-25)

**Authority:** [the Collection Mode PRD's F190, F191, F193, F194, F195, F196 and F200](../collection-mode/prd-collection-mode-fences.md) — owner decisions D48, D49 and D53 of its round-6 (pre-lock) adjudication, 2026-09-25 (F190, F191, F200), and its approved round-6 recommendations 1–4 the same day (F193–F196).

**Decision:** (1) Under the Collection Mode PRD's F190, R6.2a's E35 goes when the file closes and never shows over another file; at the next open, while the other app still reads, it shows again, an earlier OK notwithstanding; nothing about it is kept outside the file (R1.1); DJ4 asserts the close, the switch and a reopen after OK. (2) Under its F191, R1.11 names a re-read among the operations that hold writes, and R7.6b lists a re-read's progress; a re-read's checks therefore never overlap an edit, which supersedes F57 (3)'s clause that R7.3c's checks defer a wipe and DJ4's re-read run; DJ3 asserts the hold. (3) Under its F193, E15 and E34 read "so your last change wasn't saved". (4) Under its F194, E35 reads "Your changes are saved." (5) Under its F195, ADR-0003's input bounds every read this app makes but an export and R7.3h's copy, R7.3c's checks and R5.4's open-file check ending before the file takes a write. (6) Under its F196, OQ 20's closer names an ADR's undo representation, so ADR-0003 need not wait for OQ 20. (7) Under its F200, R1.11's deferral of a write the app makes by itself leaves out the wipe, which runs beside a held write, needing no write lock, so R6.2a's deadline holds while a write is held; DJ3 asserts it on a checkpointed file whose removed text is in no WAL frame, leaving the WAL's reset to ADR-0003. (8) Editorial and testability, no owner decision: DJ4's second-read line asserts R6.2a's deadline, the deferral by a read begun after the delete permitted and never required (the Corrected line under F57); DJ4's outside reads are declared as its first line's, its first line's reopen read taking a 5 s functional timeout, and its Save-a-copy line reads within 5 s of the later of the delete landing and the copy's end; DJ3's lines name Collection Mode R8.1f/g writes, the destination's volume for a move, an item outside the held write, a named copy read, a functional timeout, and a write committed before a rename kept under the new name; the copy header withholds a marked action or sentence whatever its state; the inbound Collection Mode line's Rows add R6.3 (the Collection Mode PRD's F187) and its fence range reads F52–F58; the status line's range reads F50–F54 and F56–F58. After the round-6 fix pass the body stands at 8,399 of 8,400 (F55), no rule trimmed. R1.11, R7.6q, E34 and E35 stay ready for alignment; R6.2a, R7.6b and E15 keep their alignment; OQ 20 stays open.

**Why:** each is the Data Foundation half of a Collection Mode round-6 decision or fix about the file, a state or surface this PRD owns, its copy, an input it hands ADR-0003, or the seam.

**Rows:** R1.11, R6.2a, R7.6b, E15, E34, E35, OQ 20, DJ3 (its wipe-beside-a-held-write line among them), DJ4, the copy header, the Collection Mode inbound line, the Corrected F57 line.

**Clarified 2026-09-26 ([the Collection Mode PRD's F202](../collection-mode/prd-collection-mode-fences.md#f202--the-log-copy-of-removed-text-waits-for-a-running-write-2026-09-26), owner decision D55):** (7)'s "needing no write lock, so R6.2a's deadline holds while a write is held" is qualified: that holds for text in the file's own bytes; a copy of it in a journal or log the app keeps beside the file goes at the latest when a held write lands, as F59 states. Peer review pending.

**Clarified 2026-09-26 (round-13 orchestrator bookkeeping; no owner decision; the fence it points to governs):** the journal-or-log deadline (7) and the Clarified line above state is superseded by F61's principle; F61 governs.

## Collection Mode round-10 amendment (2026-09-26)

Source: the owner's round-10 decisions in the Collection Mode PRD's adjudication of 2026-09-26, recorded there as [its F202](../collection-mode/prd-collection-mode-fences.md#f202--the-log-copy-of-removed-text-waits-for-a-running-write-2026-09-26) (owner decision D55, over the PR #21 review's finding T3). This fence carries only the Data Foundation half of that decision; peer review pending.

### F59 — The log copy of removed text waits for a running write (2026-09-26)

- **Authority:** [the Collection Mode PRD's F202](../collection-mode/prd-collection-mode-fences.md#f202--the-log-copy-of-removed-text-waits-for-a-running-write-2026-09-26) — owner decision D55, its round-10 adjudication, 2026-09-26.
- **Decision:** R6.2a's deadline is qualified as F202 states: text in the file's own bytes is gone at the read-based deadline R6.2a already states, whether or not a write is held; a copy of that text in a journal or log the app keeps beside the file goes once nothing still needs it — a read begun before the wipe, and a write running when the wipe runs, both counting — at the latest when that write lands, clearing it able to hold the next write while it runs. E35 stays until the wipe lands, meaning the text is in none of R6.2a's bytes. DJ3 gains the log-copy line (D1).
- **Why:** the Data Foundation half of a Collection Mode decision about what the file's bytes guarantee; this PRD owns the file and its deletion lifecycle.
- **Rows:** R6.2, R6.2a, E35, DJ3, and the inbound Collection Mode line's fence range, which becomes F52–F59.

**Clarified 2026-09-26 (F60):** its "(D1)" above means the Collection Mode round-10 fix file's box D1, not an owner decision numbered D1; F60 carries the round-11 completion of this same deadline.

**Clarified 2026-09-26 (round-14 orchestrator bookkeeping; no owner decision; the fence it points to governs; IF14-m2):** this fence's Decision is superseded by F61's principle — the copy of removed text in a journal or log beside the file goes at the first moment, with the file open, that no read uses that journal or log and no write runs; after a crash, at the first open at which that holds. F61 governs.

**Clarified 2026-09-26 (round-15 orchestrator bookkeeping; no owner decision; IF15-m2, PRIV15-2, SSE15-m2, ARCH15-4):** the round-14 line above supersedes only this fence's journal-or-log deadline; the main-file clause and E35's end condition stand. "After a crash, at the first open at which that holds" means the first such moment after reopening, as the ADR-0003 input reads (IF15-m3, SSE15-m2, R15-m4).

## Collection Mode round-11 amendment (2026-09-26)

Source: the owner's round-11 decisions in the Collection Mode PRD's adjudication of 2026-09-26, recorded there as [its F211 and F212](../collection-mode/prd-collection-mode-fences.md) (owner decisions D65 and D66, over round 11's database-lens MAJOR-1 and MAJOR-2). This fence carries only the Data Foundation half of those decisions; peer review pending.

### F60 — The log copy waits for every read still using it, and clearing never holds a write on a read (2026-09-26)

- **Authority:** [the Collection Mode PRD's F211 and F212](../collection-mode/prd-collection-mode-fences.md) — owner decisions D65 and D66, its round-11 adjudication, 2026-09-26.
- **Decision:** R6.2a's journal-or-log deadline is completed as F211 and F212 state. The copy goes once no read still using it remains — one begun after the wipe included — and the write running at the wipe has ended or failed, at the next open after a crash. Clearing it holds the next write only for its own copy and truncation, never while it waits on a read; a clearing a read blocks gives way and is retried once that read ends. R1.11's "but a wipe (R6.2a)" exemption covers this clearing the same way. DJ3 gains sub-runs for a second outside read begun during the held write, and for a crash in that window.
- **Why:** the Data Foundation half of a Collection Mode decision about what the file's bytes guarantee; this PRD owns the file and its deletion lifecycle.
- **Rows:** R6.2, R6.2a, R1.11, DJ3, and the inbound Collection Mode line's fence range, which becomes F52–F60.

**Clarified 2026-09-26 (F61; ARCH12-1):** this fence's Decision reads "R1.11's 'but a wipe (R6.2a)' exemption covers this clearing the same way"; that is corrected. R1.11's exemption covers the wipe of the file's own bytes, which needs no write lock. The clearing needs the write lock and runs once no write runs, as F61 states.

**Clarified 2026-09-26 (round-13 orchestrator bookkeeping; no owner decision; the fence it points to governs; IF13-m3):** this fence's list of what can hold the journal or log copy is replaced by F61's principle — the copy goes at the first moment, with the file open, that no read uses that journal or log and no write runs, after a crash at the first open at which that holds — which covers every case this fence's list did. F61 governs.

## Collection Mode round-12 amendment (2026-09-26)

Source: the owner's round-12 decisions in the Collection Mode PRD's adjudication of 2026-09-26, recorded there as [its F215](../collection-mode/prd-collection-mode-fences.md) (owner decision D69, over round 12's privacy and architecture findings PRIV12-1 and ARCH12-1). This fence carries only the Data Foundation half of that decision; peer review pending.

### F61 — The log copy goes at the first moment nothing uses the log (2026-09-26)

- **Authority:** [the Collection Mode PRD's F215](../collection-mode/prd-collection-mode-fences.md) — owner decision D69, its round-12 adjudication, 2026-09-26.
- **Decision:** R6.2a's journal-or-log deadline is restated as a principle, replacing F60's case-by-case list: the copy of removed text in a journal or log beside the file goes at the first moment, with the file open, that no read uses that journal or log and no write runs; after a crash, at the first open at which that holds. Clearing it holds the next write only for its own copy and truncation, never while it waits on a read, running before the next write starts (F60). DJ3 gains sub-runs for a second write started while a clearing waits on a read, a write made while a clearing is blocked, and the held write failing instead of landing.
- **Why:** the Data Foundation half of a Collection Mode decision about what the file's bytes guarantee; this PRD owns the file and its deletion lifecycle.
- **Rows:** R6.2, R6.2a, DJ3, and the inbound Collection Mode line's fence range, which becomes F52–F61.

**Clarified 2026-09-26 (round-16 orchestrator bookkeeping; no owner decision; the fence it points to governs):** F62 qualifies this fence's first moment for a full volume.

**Clarified 2026-09-26 (round-16 orchestrator bookkeeping; no owner decision):** "after a crash, at the first open at which that holds" means the first such moment after reopening, as the ADR-0003 input and DJ3 (h) read it.

## Collection Mode round-15 amendment (2026-09-26)

Source: the owner's round-15 decision in the Collection Mode PRD's adjudication of 2026-09-26, recorded there as [its F218](../collection-mode/prd-collection-mode-fences.md) (owner decision D72, over round 15's database-lens DB15-MAJOR-1 and privacy-lens PRIV15-1). This fence carries only the Data Foundation half of that decision; peer review pending.

### F62 — A full volume defers the log copy until room returns (2026-09-26)

- **Authority:** [the Collection Mode PRD's F218](../collection-mode/prd-collection-mode-fences.md) — owner decision D72, its round-15 adjudication, 2026-09-26.
- **Decision:** Where a full volume stops the clearing of a journal or log, the copy stays and E35 stays up. Within 5 s of room being restored, the app clears it on its own, with no user action. F61's "first moment" reads "…and the volume has room to clear it". Text in the main file is still wiped at its normal deadline. DJ3 gains sub-runs (g)'s full-volume timing and (i)'s growth case, where committed frames not yet copied in would make the file bigger.
- **Why:** the Data Foundation half of a Collection Mode decision about what the file's bytes guarantee; this PRD owns the file and its deletion lifecycle.
- **Rows:** R6.2, R6.2a, DJ3, and the inbound Collection Mode line's fence range, which becomes F52–F62.

**Clarified 2026-09-26 ([the Collection Mode PRD's F218](../collection-mode/prd-collection-mode-fences.md) and its Clarified line from the round-16 product-manager review's PM16-2, owner decision D72):** a clearing a full volume stops gives way, holding no write of the app's own, and is retried once room returns.

**Clarified 2026-09-26 ([the Collection Mode PRD's F218](../collection-mode/prd-collection-mode-fences.md) and its Clarified lines, owner decision D72):** the deferral applies only where a full volume actually stops the clearing; where the clearing can run on the full volume, F61's first moment governs (DJ3 (g) and (i)).

**Clarified 2026-09-26 (round-17 orchestrator bookkeeping; no owner decision):** DJ3 (j), a full volume that refuses a truncation or a sync until room returns, also carries this fence, beside (g) and (i); (g) is now the full volume that lets the clearing run, and (i) and (j) the clearings a full volume stops; (d)'s guards for (g), (i) and (j) carry it with them.

**Clarified 2026-09-26 ([the Collection Mode PRD's F218](../collection-mode/prd-collection-mode-fences.md) and its Clarified line from the round-16 privacy review's PRIV16-2 and PRIV16-3, owner decision D72 read with F211):** "E35 stays up" keeps an E35 already up and raises none where F211 shows none; the clearing once room returns still needs the file open and no read using the log, and no marker of it is kept outside the file (F58 (1)).

**Clarified 2026-09-26 (round-17 orchestrator bookkeeping; no owner decision; [the Collection Mode PRD's F218](../collection-mode/prd-collection-mode-fences.md) and its Clarified lines govern):** "room to clear it" means room enough for that clearing to succeed, and the clearing once room returns runs at the first moment after room returns that the file is open, no read uses the log and no write runs.

**Clarified 2026-09-26 ([the Collection Mode PRD's F219](../collection-mode/prd-collection-mode-fences.md), owner decision D73):** the Decision's "Text in the main file is still wiped at its normal deadline" holds where the volume lets that wipe and its sync complete; where a full volume refuses either, F63 governs.

## Collection Mode PR-review follow-up amendment (2026-09-26)

Source: the owner's decision D73 in the Collection Mode PRD's PR #21 follow-up adjudication of 2026-09-26, recorded there as [its F219](../collection-mode/prd-collection-mode-fences.md), over the follow-up review's reopened erasure finding. This fence carries only the Data Foundation half of that decision; peer review pending.

### F63 — A full volume defers the main-file wipe until room returns (2026-09-26)

- **Authority:** [the Collection Mode PRD's F219](../collection-mode/prd-collection-mode-fences.md) — owner decision D73, its PR #21 follow-up adjudication, 2026-09-26.
- **Decision:** Where a full volume refuses a main-file wipe or the sync that makes it last, the wipe is retried, and the removed text is gone from the file's own bytes, durably, within 5 s of the file being open with room: room returning while it is open, or the first open with room after a close, a crash or a power loss. E35 stays up if it was and is raised nowhere F60 shows none, as under F62. F62's "Text in the main file is still wiped at its normal deadline" holds where the volume lets the wipe and its sync complete. R6.2a states it; DJ3 gains (k), a sync refused before the wipe, and (l), its crash and power-loss runs.
- **Why:** the Data Foundation half of a Collection Mode decision about what the file's bytes guarantee; this PRD owns the file and its deletion lifecycle.
- **Rows:** R6.2, R6.2a, DJ3, and the inbound Collection Mode line's fence range, which becomes F52–F63.

**Clarified 2026-09-26 ([the Collection Mode PRD's F219](../collection-mode/prd-collection-mode-fences.md) and its round-22 Clarified line, owner decision D73):** "durably" covers the text-removing write itself, which reaches disk before any surface shows it done, so a power loss never undoes it. A refused main-file wipe or sync is the app's own write and renders no refusal state (E15, E34), which stay for a user's change. "With room" means room enough for the wipe and its sync to succeed. A read begun after the wipe was refused does not carry its retry past the 5 s. E35 goes as R6.2a's byte test finds the text gone.

**Clarified 2026-09-26 (round-22 orchestrator bookkeeping; no owner decision; interface review's IF22-m1 and IF22-n5):** "raised nowhere F60 shows none" reads "raised nowhere [the Collection Mode PRD's F211](../collection-mode/prd-collection-mode-fences.md) shows none", F60 carrying no E35 rule; the Rows are R6.2, R6.2a, DJ3 (d), (k), (l) and (m), (m) being a delete with no outside read on a volume that refuses only the file's own sync.

**Clarified 2026-09-26 (round-23 orchestrator bookkeeping; no owner decision; product-marketing review's PMM23-1 and PMM23-4):** "refusal state" in the round-22 Clarified line above means any of the Collection Mode PRD's R8.8's four: E15, E10 and E34, and the capture PRD's E26; and, as the Collection Mode line reads, durability is what DJ3's induced-loss reads assert.

## Nix Toolkit export amendment (2026-09-26)

Source: the owner's decisions N1–N15 in [the Nix Toolkit import review log's Round 0](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-0--owner-adjudication-2026-09-26) and N16–N23 in its Round 1, recorded as [the Inventory Import PRD's F69–F91](../import/prd-inventory-import-fences.md). These fences carry only the Data Foundation half; peer review pending.

### F64 — A Nix Toolkit export's reading can be an item's canonical value (2026-09-26)

- **Authority:** [the Inventory Import PRD's F70, F74, F77, F78, F81 and F82](../import/prd-inventory-import-fences.md) — owner decisions N2, N6, N9, N10, N13 and N14, 2026-09-26.
- **Decision:** A reading imported from a Nix Toolkit export is stored as R2.3j states and can be an item's canonical value, which R2.2's "an imported row has none until scanned" now excepts. Its spectrum is kept as given and every derived value worked out from it; its samples are not recorded; its snapshot is of the imported kind with serial and firmware unknown; it has no raw payload, one never supplied, distinct from R5.5a's archive-unavailable mark; its measurement time is the file's Date Saved and its record time the commit. It is current with no current value or over an older imported one; behind a reading scanned in SpectroCapture it is kept as an earlier reading, which history and over-time views place by its measurement time. The out-of-scope line names these exports as v1.
- **Why:** the Data Foundation half of the Inventory Import PRD's decision to bring Toolkit readings in; this PRD owns the canonical value and version history.
- **Rows:** R1.6, R2.2, R2.3, R2.3j, the out-of-scope line, the journeys index, DJ6, and the Inventory Import obligation line.

**Clarified 2026-09-26 ([round 1](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-1--nine-lenses-2026-09-26); owner decision N16, F66):** "which history and over-time views place by its measurement time" means the over-time views, the history's measured order among them; the recorded order keeps the imported reading at its record time (R2.1). Round 1 also carries this decision into the Vocabulary, R1.2, R2.1, R2.3's lead, R2.3h, R7.1, R7.2, R7.7 and R7.7o: a reading with no samples, basis, verdict or spread; unknowns read back empty; a same reading creating none; the predecessor fixed as the reading current when B landed; restoring an imported reading; and its fixture.

**Clarified 2026-09-26 ([round 2](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-2--delta-verification-2026-09-26); F66–F69):** R5.5d's salvage of two current readings keeps the one R2.3j would leave current, the other kept behind with no question, and otherwise the later-recorded; an imported snapshot's unknowns are absent (NULL), never a placeholder; the Data Export line names R7.7o. R5.5d, R2.3j, DJ6 and that line carry it.

**Clarified 2026-09-26 ([round 3](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-3--delta-verification-2026-09-26)):** R5.5d replays two current readings in record order, ending a pair whose later-recorded reading is imported as R2.3j leaves it with that reading as B, reasons included; R7.5 checks the build over Import UJ 3's Fixture T spectra too; R7.7o adds a quarantined imported current. R5.5d, R7.5, R7.7o and DJ6 carry it.

**Clarified 2026-09-26 ([round 4](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-4--delta-verification-2026-09-26)):** R5.5d replays by per-item sequence and never drops a reading, a same reading kept behind, and otherwise gives the later reading correction-unconfirmed with the other as its predecessor; R7.5's Fixture T check lands with Import §6 under the tabulation the build uses, OQ 6's reference governing once named; M2's population names Fixture T's spectra; R2.3j stores a blank reference absent (NULL) and speaks of a readable A; R7.7o adds a reading with no model. R2.3j, R5.5d, R7.5, R7.7o, M2 and DJ6 carry it.

**Clarified 2026-09-26 ([round 5](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-5--delta-verification-2026-09-26)):** DJ3's salvage case follows R5.5d; R5.5d's R2.3j branch excludes a restore; R2.3j's reason column gives re-measurement only when B is current over a readable A, matching Import R6.8b and R6.8d; M2's Fixture T population waits for Import §6; R7.7o no longer names Collection Mode, whose cases seed directly; the Import line names its R6.10. DJ3 and F36's Clarified line carry it too.

**Clarified 2026-09-26 ([round 6](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#round-6--delta-verification-2026-09-26)):** DJ3 and DJ6 carry R5.5d's "and not a restore", and DJ6 adds two restore pairings: a restore of an imported reading stays current as correction-unconfirmed over a live reading and over an imported reading measured later, the other its predecessor. DJ3 and DJ6 carry it.

### F65 — Word budget 8,600 (2026-09-26)

- **Authority:** [the Inventory Import PRD's F80](../import/prd-inventory-import-fences.md) — owner decision N12, 2026-09-26.
- **Decision:** This PRD's word budget is 8,600 words, counted by rule 14's method, recorded by the dated lines under F55 and beside F214's.
- **Rows:** governs no rows.

### F66 — History order for an imported reading (2026-09-26)

- **Authority:** [the Inventory Import PRD's F84](../import/prd-inventory-import-fences.md) — owner decision N16 in [the Nix Toolkit import review log's Round 1](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#owner-adjudication-2026-09-26), 2026-09-26.
- **Decision:** R2.1's history orders by record time and its over-time views, the history's measured order among them, by measurement time; an imported reading kept behind the current one takes its place in each by those rules. No other order is introduced. Peer review pending.
- **Rows:** R2.1; DJ6.

### F67 — The reasons R2.3j records (2026-09-26)

- **Authority:** [the Inventory Import PRD's F85](../import/prd-inventory-import-fences.md) — owner decision N17 in [the Nix Toolkit import review log's Round 1](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#owner-adjudication-2026-09-26), 2026-09-26.
- **Decision:** A Toolkit reading current over an imported or simulated A is recorded as a re-measurement, one current over none or kept behind A as initial, without asking; R2.4 names the exception. Peer review pending.
- **Rows:** R2.3j, R2.4; DJ6.

### F68 — A Toolkit reading replaces a simulated current one (2026-09-26)

- **Authority:** [the Inventory Import PRD's F86](../import/prd-inventory-import-fences.md) — owner decision N18 in [the Nix Toolkit import review log's Round 1](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#owner-adjudication-2026-09-26), 2026-09-26.
- **Decision:** B is current over a simulated A, which stays in history; only a live A stays current over B. Peer review pending.
- **Rows:** R2.3j; DJ6.

### F69 — A same-dated, different reading is kept behind (2026-09-26)

- **Authority:** [the Inventory Import PRD's F87](../import/prd-inventory-import-fences.md) — owner decision N19 in [the Nix Toolkit import review log's Round 1](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#owner-adjudication-2026-09-26), 2026-09-26.
- **Decision:** B with the same measurement time as an imported current A but a different spectrum is kept behind A; only an earlier-measured imported A yields. Peer review pending.
- **Rows:** R2.3j; DJ6.

### F70 — Word budget 8,900 (2026-09-26)

- **Authority:** [the Inventory Import PRD's F91](../import/prd-inventory-import-fences.md) — owner decision N23 in [the Nix Toolkit import review log's Round 1](../../agent-reviews/2026-09-26-nix-toolkit-import-peer-reviews.md#owner-adjudication-2026-09-26), 2026-09-26.
- **Decision:** This PRD's word budget is 8,900 words, counted by rule 14's method, recorded by the dated lines under F55. Peer review pending.
- **Rows:** governs no rows.
