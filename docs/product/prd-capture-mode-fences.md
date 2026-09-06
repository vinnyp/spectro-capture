# Capture Mode PRD — fences

Owner decisions and owner-rejected findings for `prd-capture-mode.md`. Every entry is settled: it is carried into every review brief and editing dispatch, and is not re-litigated. Format per `operator-agents:writing-prds`.

Review log: docs/agent-reviews/2026-09-06-prd-capture-mode-peer-reviews.md (created round 1, 2026-09-06; later rounds append)
Owner-locked row IDs: (none — no requirement rows yet)

## Fences

### F1 — No Library in v1 (2026-09-06)

**Decision:** The Library > Collection hierarchy is dropped from v1. The organizational model is flat collections, matching the vision and AGENTS.md vocabulary. "Move a collection to another library" and "rename/delete a library" journeys are removed. One SQLite file holds the user's collections; whether a user can have several files is a Data Foundation question, not a capture one.

**Why:** Library was introduced by the journey bootstrap, not the vision. The collection-browsing research leans toward one flat store with shallow collections (calibre: virtual libraries are "superior to splitting up your library into multiple smaller libraries"), and no source endorses Library > Collection containment. Dropping it removes a data-model decision that was arriving through journey steps.

**Applies to:** UJ1, UJ1.1, UJ1.2, UJ2 (target choice), UJ3 step 2, UJ4 step 2, the surfaced-questions list, the assumptions list.

### F2 — Measurement settings scope (2026-09-06)

**Decision:** Illuminant and observer are optional collection-level display defaults (D50/2°), never required at creation, editable at any time without re-scanning. Samples-per-row (1–5) is the acquisition setting: required at collection creation, changeable between sessions. Which ISO 13655 scan modes (M0/M1/M2) a capture records remains an open question against the SDK. Whether samples-per-row can change mid-session remains an open question.

**Why:** The raw payload is canonical; colour values are derived from it (AGENTS.md §8; SDK audit: colour data is XYZ-backed and freely convertible). "Observer (2°, M1)" in the bootstrap conflated the CIE observer with the measurement condition, which is a scan mode of the instrument.

### F3 — Rename and delete belong to Collection Mode (2026-09-06)

**Decision:** Create-collection stays in this PRD because it is on the capture critical path. Rename-collection and delete-collection are handed to the Collection Mode PRD. The owner's steps are preserved under UJ1.2 with a scope-boundary note so they land in exactly one document; they generate no requirements here.

**Why:** The product README assigns editing surfaces, selection, and bulk operations to Collection Mode.

### F4 — Import target is chosen in the app (2026-09-06)

**Decision:** The target collection is picked or created in the app before columns are mapped. Collection name (and, per F1, library name) is not a CSV mapping target in v1. One CSV feeds one collection. Import ends at "collection ready to capture"; starting the session is a separate step. The bootstrap's UJ2 / UJ2.1 / UJ2.2 shape becomes UJ2 (new collection) plus UJ2.1 (idempotent re-import into an existing collection), with UJ2.2 as the shared column-mapping journey.

**Why:** A CSV is normally one swatch book. A collection-name column forces every file to carry a constant column and implies a multi-collection file that is not the primary case. Import-today-scan-tomorrow requires the queue to survive between import and session.

### F5 — Queue navigation: jump by code plus manual reordering (2026-09-06)

**Decision:** The operator can find any pending row by code or name without leaving capture (UJ3.7), and can also reorder the queue manually, before or during a session, by dragging rows or by sorting on a metadata column. Reordering is original design: the research supports identifier-as-entry-point and gives no precedent for reordering. The reordering journey states the user-visible guarantees (captured rows are never re-scanned by a reorder; a reorder mid-session does not lose the current item's samples) and names its specifics as open questions.

**Why:** Physical order rarely matches CSV order for a whole run (a marker set sorted by hue). Jump-by-code alone means a 200-item hue-sorted set is navigated one search at a time.

## Rejected findings

(none yet)
