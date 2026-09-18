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

### F18 — The CSV carries the vendor's raw payload column (2026-09-09, round 1)

**Decision:** One opaque column per row, documented as the vendor's round-trip string, so the export carries the file's fidelity promise and M3 (now the export PRD's M1) reads as written; a reading with no payload leaves it empty and marked.

### F19 — v1 does not ship without the vendor-analytics disclosure (2026-09-09, round 1)

**Decision:** A §6 row gates release on a disclosure that names the recipient, the events (connect, scan, calibration), that it fires per event and cannot be switched off, and that it is separate from the app's own opt-in telemetry. Authorship stays with the Telemetry PRD and the help docs (F8 unchanged).

### F20 — The gamut-clipped export column is `sRGB_gamut_clipped` (2026-09-09, round 1)

**Decision:** Closes OQ 8 (now the export PRD's OQ 1) by owner decision; revisited only if ISO 17972-4's schema becomes readable.

### F21 — Word budget 7,000 (2026-09-09, round 1)

**Decision:** Recorded in F1's amended text above. **Amended 2026-09-10 (owner, before the pre-lock round):** 7,500, for the lock checks' traceability and vocabulary additions. **Amended 2026-09-14 (owner, after the pre-lock round):** 8,000, for the fresh lenses' rules.

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
| F1 | Every row in the PRD body, plus its Background scope statement, its shape, and its word budget (F21, 8,000 since 2026-09-14). |
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
