# Data Foundation PRD — owner decisions (fences)

Owner decisions on this PRD. A fence is settled: reviewers do not re-litigate it, and a row that names one does so for provenance only. Owner-locked rows: none yet. Review log: `../../agent-reviews/<date-of-round-1>-prd-data-foundation-peer-reviews.md` (recorded at round 1).

## Phase 0 — research inventory (2026-09-09)

Ingested before any round: every brief under `docs/briefs/` on `main` at `dd43c13`; the browsing-a-collection-at-scale research results v2 (brought into `docs/briefs/` from the foundry in this PR; supersedes the first-pass results on any point where they conflict); the X-Rite SDK and open-source prior-art findings (brought in the same way; CxF and export shapes). The three locked PRDs and their obligations tables. Deferred by the owner: the Spectro 2 software competitive brief (positioning, not data). No project-supplied prior-art query command.

### F1 — Data Foundation is a slim data contract, not a schema document (2026-09-09)

**Decision:** This PRD states what the user-owned file guarantees: what is kept and never destroyed, what "canonical value" and version history mean to someone reading the file, which derived spaces are present and what the gamut-clipped flag asserts, what the CSV export contains, what a schema migration does to a file a user already has, what the user may delete, and the obligations the capture-mode, import, and device-management PRDs already impose on Data Foundation, gathered here by citation. The storage schema, blob layout, library choice, and migration mechanism are out of scope: they belong to ADR-0003 and a technical spec that follows this document. Word budget for the PRD body: 5,000 words (raised from 4,000 in Phase 3, 2026-09-09: the first fill landed at 5,030 after four compaction passes, and the remainder is rules, not prose — export and deletion are U5 and U6 promises this fence put in scope). Shape: the capture PRD's three-file shape plus this fence file and an OQ results file; no author line; row IDs `R<section>.<n>`, `E<n>`, `M<n>`; every requirement row at most two sentences.

**Why:** U5 and U6 are promises to users about the data itself, and the data-consumer persona has no other PRD; the rest is engineering, and reviewing schema through product lenses produces the "solution smuggled into a requirement" findings the gate exists to reject. Everything this PRD can inherit it cites rather than restates.

### F2 — One file, one open at a time (2026-09-09, Phase 3)

**Decision:** A user's data lives in one store the user owns, and one store is open at a time; the matching rule and every cross-collection view span that one file. Closes OQ 1.

**Why:** The browsing research favours one store over many; the capture PRD's OQ 19 already leans this way; every "the file" guarantee is stated once.

### F3 — A re-scan asks correction-or-re-measurement once, outside the loop (2026-09-09, Phase 3)

**Decision:** The file records whether a new version is a correction (the old value was wrong) or a re-measurement (the item changed). A re-scan made during capture defaults to correction and is never asked mid-loop; the question is asked once, outside the heads-down loop, and the answer lands on the version. Inherited obligation for the capture PRD: its E29 state gains the correction default as a post-lock amendment. Closes OQ 3.

**Why:** The research is unambiguous that the distinction cannot be inferred; not asking means fabricating it, and asking mid-loop breaks heads-down capture.

### F4 — The gamut-clipped flag compares against a fixed stored reference (2026-09-09, Phase 3)

**Decision:** GAMUT_REFERENCE_SPACE is sRGB and GAMUT_RENDERING_INTENT is relative colorimetric, stored with the datum, so the flag means the same thing to every reader of the file. A live display-dependent check is Collection Mode's, not this file's. Closes OQ 7.

### F5 — Export is canonical-only by default with version history as a v1 option (2026-09-09, Phase 3)

**Decision:** The default CSV export is one row per item carrying its canonical value; an explicit option exports every version. The canonical export is P0; the history option is P1. Closes OQ 9 and settles the export rows' priority.

### F6 — Version history is kept for good (2026-09-09, Phase 3)

**Decision:** HISTORY_RETENTION means nothing is aged out; no retention window exists. Closes OQ 4.

**Why:** Corrections never destroy data (AGENTS.md §4), and the research names retrofitting a policy onto a store that cannot enumerate its history as the trap.

### F7 — A delete is undoable until the app quits (2026-09-09, Phase 3)

**Decision:** DELETE_UNDO_WINDOW is the running session: a deleted item and its history can be restored until the app quits, and the deletion is final after. Closes OQ 10.

### F8 — The vendor SDK's analytics disclosure belongs to the Telemetry PRD and the help docs (2026-09-09, Phase 3)

**Decision:** This PRD hands the disclosure over as an inherited obligation; it carries no disclosure row of its own. Closes OQ 12's ownership half; whether the analytics can be disabled stays open there.

### F9 — Journey DJ5, querying the file without the app, stays (2026-09-09, Phase 3)

**Decision:** The journeys file keeps DJ5; it is non-normative and states what a reader of the file can rely on, which is U6's headline job.

## Fence → row map

Filled by the Phase 3 fix pass (2026-09-09). A row "carries" a fence when the fence's decision is what the row now states; the fence file, not the row, holds the rationale.

| Fence | Rows that carry it |
| :--- | :--- |
| F1 | Every row in the PRD body, plus its Background scope statement, its shape, and its 5,000-word budget. |
| F2 | R1.1, R1.3; OQ 1. |
| F3 | R2.4; the Capture Mode line in [Inherited obligations](prd-data-foundation.md#inherited-obligations) → "what this PRD imposes on others"; OQ 3. |
| F4 | R3.4; OQ 7. |
| F5 | R4.1 (canonical default, P0), R4.4 (history option, P1); the P0/P1 split in the Legend, and the Pri cells of R4.2, R4.3, R4.5; OQ 9. |
| F6 | R2.6; OQ 4. |
| F7 | R6.3; OQ 10. |
| F8 | R6.5; the Telemetry and help-docs line in [Inherited obligations](prd-data-foundation.md#inherited-obligations) → "what this PRD imposes on others"; OQ 12's ownership half. |
| F9 | No requirement row. It keeps [DJ5](prd-data-foundation-journeys.md#dj5-query-the-file-without-the-app) in the journeys companion, which the [User Journeys](prd-data-foundation.md#user-journeys) index lists as J5 against R1.1–R1.3 and R3.2. |

## Rejected findings

(none yet)
