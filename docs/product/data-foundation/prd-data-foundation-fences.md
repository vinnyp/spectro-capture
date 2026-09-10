# Data Foundation PRD — owner decisions (fences)

Owner decisions on this PRD. A fence is settled: reviewers do not re-litigate it, and a row that names one does so for provenance only. Owner-locked rows: none yet. Review log: `../../agent-reviews/2026-09-09-prd-data-foundation-peer-reviews.md`.

## Phase 0 — research inventory (2026-09-09)

Ingested before any round: every brief under `docs/briefs/` on `main` at `dd43c13`; the browsing-a-collection-at-scale research results v2 (brought into `docs/briefs/` from the foundry in this PR; supersedes the first-pass results on any point where they conflict); the X-Rite SDK and open-source prior-art findings (brought in the same way; CxF and export shapes). The three locked PRDs and their obligations tables. Deferred by the owner: the Spectro 2 software competitive brief (positioning, not data). No project-supplied prior-art query command.

### F1 — Data Foundation is a slim data contract, not a schema document (2026-09-09)

**Decision:** This PRD states what the user-owned file guarantees: what is kept and never destroyed, what "canonical value" and version history mean to someone reading the file, which derived spaces are present and what the gamut-clipped flag asserts, what the CSV export contains, what a schema migration does to a file a user already has, what the user may delete, and the obligations the capture-mode, import, and device-management PRDs already impose on Data Foundation, gathered here by citation. The storage schema, blob layout, library choice, and migration mechanism are out of scope: they belong to ADR-0003 and a technical spec that follows this document. Word budget for the PRD body: 7,000 words (raised from 4,000 to 5,000 in Phase 3 when the first fill landed at 5,030 after four compaction passes, and from 5,000 to 7,000 after round 1, 2026-09-09: nine lenses found some fifteen missing guarantees — the sample tier, time axes, atomic writes, the sync posture, the file's identity and floor, the CSV contract, fixtures — which are rules, not prose). Shape: the capture PRD's three-file shape plus this fence file and an OQ results file; no author line; row IDs `R<section>.<n>`, `E<n>`, `M<n>`; every requirement row at most two sentences.

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

**Decision:** The default CSV export is one row per item carrying its canonical value; an explicit option exports every version. The canonical export is P0; the history option is P1. Closes OQ 9 and settles the export rows' priority.

**Clarification (1), 2026-09-09:** R7.6, the test row that asserts the export's column set, is P0 with the rows it verifies (process rule 3).

### F6 — Version history is kept for good (2026-09-09, Phase 3)

**Decision:** HISTORY_RETENTION means nothing is aged out; no retention window exists. Closes OQ 4.

**Why:** Corrections never destroy data (AGENTS.md §4), and the research names retrofitting a policy onto a store that cannot enumerate its history as the trap.

### F7 — A delete is undoable until the app quits (2026-09-09, Phase 3)

**Decision:** DELETE_UNDO_WINDOW is the running session: a deleted item and its history can be restored until the app quits, and the deletion is final after. Closes OQ 10.

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

**Decision:** One opaque column per row, documented as the vendor's round-trip string, so the export carries the file's fidelity promise and M3 reads as written; a reading with no payload leaves it empty and marked.

### F19 — v1 does not ship without the vendor-analytics disclosure (2026-09-09, round 1)

**Decision:** A §6 row gates release on a disclosure that names the recipient, the events (connect, scan, calibration), that it fires per event and cannot be switched off, and that it is separate from the app's own opt-in telemetry. Authorship stays with the Telemetry PRD and the help docs (F8 unchanged).

### F20 — The gamut-clipped export column is `sRGB_gamut_clipped` (2026-09-09, round 1)

**Decision:** Closes OQ 8 by owner decision; revisited only if ISO 17972-4's schema becomes readable.

### F21 — Word budget 7,000 (2026-09-09, round 1)

**Decision:** Recorded in F1's amended text above.

### F22 — Export column names, the second-file outcome, and two tokens (2026-09-09, after the round-1 fix pass)

**Decision:** (a) Every column the app emits in the CSV carries the prefix `sc_`; an imported column that would collide is emitted as `import_<name>` and the export surface says so (R4.8). (b) Opening a second file is refused while a capture session is running; otherwise the file in hand closes first (R1.3). (c) An absent derived value exports as an empty field, never a zero (R3.5); no COMPATIBILITY_FLOOR constant exists until a release raises the floor above the first file version (R5.7).

### F23 — The `sc_` prefix applies to every app column (2026-09-09, round 2)

**Decision:** F22(a) subsumes F20 and the device PRD's `simulated` column: the export columns are `sc_sRGB_gamut_clipped`, `sc_sRGB_source_space`, `sc_sRGB_rendering_intent`, `sc_simulated`, and `sc_nm_<wavelength>` for the spectral columns. F20's answer and OQ 8's results section are restated under the prefix; the device PRD's "emits a `simulated` column" obligation is read as `sc_simulated`, carried as a post-lock amendment line under "what this PRD imposes on others". No exemption list exists.

**Why:** One rule the golden header can be written against; a closed exemption list would re-open the collision F22 exists to close.

### F24 — The export format version fixes the wavelength column set (2026-09-09, round 2)

**Decision:** The wavelength column set — start, end, interval — is part of the export format version; v1's set is the v1 instrument family's reported grid, carried as OQ 16 with a per-SDK-docs candidate closed on hardware. A reading whose wavelengths fall outside the set exports those columns empty and marked; any change to the set bumps the export format version (R4.9).

**Why:** R4.7's "column order is fixed" and R7.7's golden are undefined without a fixed set; a header that follows the data is unusable to a consumer allocating columns before reading.

### F25 — Samples are readable at the floor (2026-09-09, round 2)

**Decision:** Each sample's decoded spectrum or colour values are stored in plain SQLite types beside its reading, inside R1.2's readability floor, so an outside reader can check the stored mean against its samples; each sample's vendor payload is an archived artifact per F11 and may be compressed. OQ 5's budget is re-derived on this population.

**Why:** R1.2 promises everything a reader needs; the evidence behind every canonical value is part of that, and R7.1 cannot verify a mean whose samples it cannot see.

### F26 — The CSV carries one payload column per sample slot (2026-09-09, round 2; amends F18)

**Decision:** The canonical export carries `sc_sample_1_payload` … `sc_sample_5_payload`, one column per sample slot up to the capture PRD's maximum of five, empty where a slot is unused or a sample has no payload. F18's "one opaque column" becomes up to five; the header stays fixed.

**Why:** A reading of N samples has N payloads; a fixed slot set keeps R7.7's golden well-defined and gives M3 every sample it reconstructs from.

### F27 — Derived values are kept per reading, history included (2026-09-09, round 2)

**Decision:** Every reading — superseded ones too — carries its six derived spaces; a derivation bump regenerates every reading's set and marks the old sets superseded. An outside reader plots an item's history from plain columns. OQ 5's budget is re-derived on this population.

**Why:** R2.5's over-time view, R4.4's history export, and R7.1's read-back all read derived values off superseded readings; keeping them only on the canonical value would make history a value only the app can compute, which R1.2 forbids.

### F28 — The collection's chosen condition is the current derived set; the canonical export emits it (2026-09-09, round 2)

**Decision:** Derived sets are keyed per reading and measurement condition; the set for the collection's chosen scan mode ([the capture PRD's R1.10](../capture-mode/prd-capture-mode.md#1-collections)) is always present and is the current one, and another condition's set is present when the user asks for it. The canonical export emits one row per item on the chosen condition, the condition column saying which; the header stays fixed.

**Why:** The capture PRD keeps every mode a reading arrives with and works colour values out from the chosen mode; one row per item (F5) and a fixed header (F24) survive only if the export names one condition per row.

### F29 — Every item gets an export row (2026-09-09, round 2)

**Decision:** Every item in the collection gets a row in the canonical export; an item with no canonical value carries its identity and import columns with its colour, spectral, and payload columns empty, and a column states whether it is never-scanned or quarantined. Both cases join M3's population.

**Why:** A half-scanned collection is the ordinary state between sittings, and the export must reconcile against the spreadsheet the inventory came from.

### F30 — The export contract is its own PRD (2026-09-09, after the round-2 fix pass)

**Decision:** The CSV export contract — §4 (R4.1–R4.9), the golden-file rule (R7.9), the export half of R7.8, the export surface (R7.6a), M3, OQ 8, OQ 9, OQ 11, OQ 16, the export states (E6, E7, E17, E18), and DJ1 — moves to a new PRD, Data Export, at `docs/product/export/` in the five-file shape, with its own row-ID families, fences, and a 4,000-word budget. Moved IDs are retired in this document's Legend naming their destination and recorded in the destination's Traceability naming their origin; the decisions the moved rows carry (F5, F18, F20, F22, F23, F24, F26, F28, F29) are transcribed into the export PRD's fence file with their provenance, and this file keeps them as history. The two PRDs share this review log; round 3 onward reads both. Chosen over raising the budget again (4,000 → 5,000 → 7,000) after the round-2 fix pass landed at 7,690 with nothing left to cut but rules.

**Why:** The export is a consumer contract with its own audience (U6's downstream tools) and its own fresh-lens set; Data Foundation is the file's promise to its owner. A document that cannot fit because it has too many rows is two PRDs (writing-prds, Phase 2).

## Fence → row map

Filled by the Phase 3 fix pass and extended by the round-1 and round-2 fix passes (2026-09-09). A row "carries" a fence when the fence's decision is what the row now states; the fence file, not the row, holds the rationale.

| Fence | Rows that carry it |
| :--- | :--- |
| F1 | Every row in the PRD body, plus its Background scope statement, its shape, and its 7,000-word budget (F21). |
| F2 | R1.1, R1.3; OQ 1. |
| F3 | R2.4; the Capture Mode line in [Inherited obligations](prd-data-foundation.md#inherited-obligations) → "what this PRD imposes on others"; OQ 3. |
| F4 | R3.4; OQ 7. |
| F5 | R4.1 (canonical default, P0), R4.4 (history option, P1); the P0/P1 split in the Legend, and the Pri cells of R4.2, R4.3, R4.5; OQ 9. |
| F6 | R2.6; OQ 4. |
| F7 | R6.3; OQ 10. |
| F8 | R6.6 (authorship handed over; the gate itself is F19's); the Telemetry and help-docs line in [Inherited obligations](prd-data-foundation.md#inherited-obligations) → "what this PRD imposes on others"; OQ 12's ownership half. |
| F9 | No requirement row. It keeps [DJ5](prd-data-foundation-journeys.md#dj5-query-the-file-without-the-app) in the journeys companion, which the [User Journeys](prd-data-foundation.md#user-journeys) index lists as J5. |
| F10 | R2.1; the Vocabulary entries for sample, reading, and canonical value; R6.2's counted-in-readings clause and [E8](prd-data-foundation-copy.md#error--state-copy); M6's population; the state diagram's sample → reading → history path. |
| F11 | R1.2 (the readability half), R1.6 (the archived-payload half); OQ 5's constraint, OQ 2's derivation. |
| F12 | R2.4, R2.5, R2.8; R7.1's read-back set and M4's third question; [E11](prd-data-foundation-copy.md#error--state-copy)'s "Ask me later"; the Capture Mode E29 line in [Inherited obligations](prd-data-foundation.md#inherited-obligations) → "what this PRD imposes on others"; OQ 3's amendment. |
| F13 | R1.7; R1.4's second clause; R7.4's volume-class scoping and M1's population. |
| F14 | R1.8, R1.9; [E13](prd-data-foundation-copy.md#error--state-copy); the Capture Mode OQ 19 line in [Inherited obligations](prd-data-foundation.md#inherited-obligations) → "what this PRD imposes on others". |
| F15 | R6.1, R6.2, R6.3; [E14](prd-data-foundation-copy.md#error--state-copy); the Collection Mode line in [Inherited obligations](prd-data-foundation.md#inherited-obligations) → "what this PRD imposes on others". |
| F16 | The Pri cells of R6.1, R6.2, R6.5 (P0) and R6.3 (P1), and the Legend's P0/P1 split; R6.5's positive inventory and its two-surface credential test. |
| F17 | R2.3's second sentence. |
| F18 | R4.1's raw-payload column; M3's statistic. Amended by F26: one column per sample slot. |
| F19 | R6.6; the Telemetry and help-docs line in [Inherited obligations](prd-data-foundation.md#inherited-obligations) → "what this PRD imposes on others"; OQ 12's gate half. |
| F20 | R4.2; OQ 8. |
| F21 | F1's amended text; no requirement row. |
| F22 | R4.8 (a); R1.3 (b); R3.5 and R5.7 (c). |
| F23 | R4.2, R4.3, R4.7, R4.8; OQ 8's results section; the Device Management export line both ways in [Inherited obligations](prd-data-foundation.md#inherited-obligations). |
| F24 | R4.7, R4.9, R7.7, R7.9; OQ 16. |
| F25 | R1.2, R1.6, R2.1, R7.1; M6's population; OQ 5's population. |
| F26 | R4.1, R7.7, R7.9; M3. |
| F27 | R3.1, R3.3, R2.5, R4.4, R7.1, R7.5; M6's population; OQ 5's population. |
| F28 | R3.1, R4.1, R4.2, R4.7; the Capture Mode line in [Inherited obligations](prd-data-foundation.md#inherited-obligations) (its R1.10, R4.5). |
| F29 | R4.1, R4.2; M3's population; [E6](prd-data-foundation-copy.md#error--state-copy). |
| F30 | The Legend's retired-ID list; the Background scope statement; the obligations tables both ways; no requirement row. |

## Rejected findings

- **agy staff, round 1, "derivation regeneration storage leak" (R3.3):** asked to drop the requirement that values on an older derivation stamp stay findable. Rejected: reproducibility of a value a user has already exported or quoted is the point of the stamp; the architecture lens's rule that exactly one set is current and the rest are retained and marked superseded is adopted instead.
- **agy privacy, round 1, Low (R6.1):** asked for deletion of a single reading out of an item's history. Rejected under the non-negotiable that corrections never destroy data and fence F6; the erasure need it points at is met by F17 (notes correctable in place) and F15 (item and collection delete).
