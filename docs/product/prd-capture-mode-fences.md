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

### F6 — A per-scan failure holds the row (2026-09-06, round 1, R1-F1)

**Decision:** When a sample fails for a per-scan reason (ambient light leakage, out-of-range temperature, set disagreement), the queue holds on the current row. The next trigger press is the retry. Moving on is a deliberate queue action (Skip, Flag). After K_FAILED_ATTEMPTS consecutive failed attempts on one row (provisional, candidate 3, OQ) the row auto-defers with a distinct "moved on" cue and the queue advances. Skip after a failure advances exactly once. The same rule governs the sample-disagreement branch.

**Why:** Three lenses found UJ3.1 step 3 self-contradictory (defer-and-advance and retry-in-place). A heads-down operator's reflexive re-press after a warning tone must never land on the next row. Holding is the only rule under which the next reading cannot be mis-attributed.

### F7 — Calibration drift and no-reading are device-PRD halts; no per-scan timeout (2026-09-06, round 1, R1-F2)

**Decision:** The per-scan failure set owned by this PRD is exactly: ambient light leakage, out-of-range temperature, set disagreement, and operator flag. Calibration drift (the scan-delta error) and a device that returns no reading are halts under the device PRD §5, handled in UJ3.6. SCAN_TIMEOUT is removed; there is no per-scan timer.

**Why:** The locked device PRD routes scan-delta drift under its halt taxonomy (§3) and halts a silent device after its liveness timeout (§5). One timer, one owner; no new injection is needed.

### F8 — N_CONSEC_DRIFT is dropped from v1 (2026-09-06, round 1, R1-F14)

**Decision:** The Westgard 10:x drift rule is removed from the consecutive-failure guard, the constants list, and the open questions. Drift detection relies on the device PRD's SDK-signalled calibration-due state and scan-delta halt, plus the within-item sample spread.

**Why:** 10:x presumes repeated measurement of one control; a bulk queue measures a different colour on every row, so there is no baseline and a hue-sorted set would pause a healthy session. Five lenses agreed.

### F9 — Flag row moves a captured row to Deferred with its reading kept as history (2026-09-06, round 1, R1-F5)

**Decision:** "Flag row" acts on the current pending row (no reading yet: deferred, cause "flagged: missing or damaged") or on the most recent captured row on the recents strip (Captured → Deferred; the flagged reading is demoted to version history; the row has no canonical value until re-scanned). The row-state diagram gains the Captured → Deferred edge. Flagged rows are resolved in the end-of-session review like any deferred row.

**Why:** Flag-row is a P0 feature that was undefined on the state model; one place resolves all problem rows, and a suspect value is never live in the collection.

### F10 — Accept average with the spread recorded (2026-09-06, round 1, R1-F8)

**Decision:** When the N samples of one item disagree beyond SAMPLE_TOLERANCE, the operator has three choices at the caution and in the review: re-take, defer, or "Accept average", which writes the canonical measurement set with the sample spread recorded on the reading so Collection Mode can show it (inherited obligation). SAMPLE_TOLERANCE stays one provisional constant, not a per-collection setting. N = 1 skips the agreement check.

**Why:** Textured paint, fabric, and metallic swatches disagree on every attempt; without this path a whole material class can never reach Captured.

### F11 — Swatch Code normalisation (2026-09-06, round 1, R1-F9)

**Decision:** Wherever a Swatch Code or a collection name is compared (uniqueness, re-import match, find, ad-hoc duplicate), the comparison trims leading and trailing whitespace, collapses internal whitespace runs to one space, and is case-insensitive. The display value is preserved as entered. Stated once in UJ2.2 and cited everywhere else.

**Why:** A re-export from Numbers or an Excel autocorrect must not double the queue on an idempotent re-import. The owner chose the forgiving rule over the research default (case-sensitive) because spreadsheet drift is the common case for this persona.

### F12 — The session is a named entity; device binding is released at quit (2026-09-06, round 1, R1-F4, R1-F11)

**Decision:** A capture session is an entity owned by a collection: the bound device identity, started and ended times, status (active, interrupted, ended, complete), elapsed capture time, and a persisted cursor (the current row's identity, updated on advance, jump, and insert). At most one active session per app and one interrupted session per collection; two collections may each hold an interrupted session. On resume, if the cursor row is still pending the session resumes there; otherwise at the next pending row in queue order. Quitting or crashing releases the device binding: "Resume capture" after a relaunch is a new session start against the persisted cursor, runs the full pre-flight gate including authorization, and binds whichever device is connected. An interrupted session can be ended from the collection. The device PRD §5 "current item" definition applies to the un-jumped case; this PRD records the cursor refinement as an inherited note for the device PRD.

**Why:** The inherited "first row with no durably written reading" definition breaks after a jump or reorder (fence F5), teleporting the operator on the common halt-resume path. The reference-white rationale for device binding is intra-session; across days it no longer holds, and releasing it at quit avoids immortal device-bound sessions.

**Amended 2026-09-06 (round-1 delta verification, fence F14):** "Resume capture after a relaunch is a new session start" stands, and is sharpened: the interrupted session is closed with status "interrupted — resumed by the next session" and the new session inherits its tallies and elapsed capture time. See F14.

### F13 — UJ4.1 (insert an unplanned item mid-session) stays in v1 (2026-09-06, round 1, R1-F24)

**Decision:** Mid-session insert with typed metadata remains a v1 journey. INSERT_POSITION stays an open question, with "append to the end and jump to it" recorded as the simpler alternative.

**Why:** Typing is the operator's choice, never demanded by the queue; the "Add item" control for a physically present item missing from the worklist is a shipped precedent. The cross-model product lens's concern (creeping UI on the capture surface) is recorded as input to the seam decision.

### F14 — Resume after a relaunch is a new session; the old one is closed as interrupted (2026-09-06, round-1 delta, NEW-J)

**Decision:** When the app relaunches after a quit or crash and the operator chooses "Resume capture", a new session starts against the remembered row: the full pre-flight gate runs, including authorization, and whichever device is connected is bound. The previous session is closed with status "interrupted — resumed by the next session" and never returns to active. The new session inherits the old session's tallies and elapsed capture time so the summary reads as one run. A day with two crashes produces three session records, linked. "At most one interrupted session per collection" therefore means at most one *unresumed* interrupted session.

**Why:** Reviewers found F12's "new session start" and "tallies carry forward" contradictory without a stated entity rule. The owner chose the chain of sessions over re-activating the old one; a per-session device identity stays simple and every session has exactly one instrument.

**Not a session interruption:** system sleep. Sleep mid-session is a device-PRD halt (device PRD §5: the disconnect halts, the authorization check is excluded for the in-flight session, "Resume scanning" is the way back). Only quit, crash, force-quit, and power loss make a session interrupted. This corrects the round-1 fix pass, not a new owner call.

### F15 — The collection remembers the last current row; every new session opens there (2026-09-06, round-1 delta, NEW-K)

**Decision:** The remembered current row belongs to the collection, not only to an interrupted session. After a session ends early, is interrupted, or completes, the next session on that collection opens at the last remembered row, falling back to the next pending row in queue order if that row is no longer pending. Jumps and reorders made in an earlier session are honoured on day two. Selecting a row in the deferred-row review also updates the remembered row.

**Why:** UJ3 step 2 ("a fresh session opens at the first pending row") and UJ3.4 step 4 ("the next session opens at the row the operator was on") contradicted each other after the round-1 fix. A hue-sorted run that stopped at row 150 must not restart at the first skipped row.

### F16 — Flag targets the row that just landed until the next trigger press (2026-09-06, round-1 delta, NEW-C)

**Decision:** After a row-success confirmation and before any trigger press on the new current row, "Flag row" demotes the row that just landed (Captured → Deferred, reading kept as history, per F9). Once the operator has pressed the trigger on the new row, Flag targets the current row (a pending row is deferred as "flagged: missing or damaged"; a row mid-set is deferred with its good samples kept). No timing constant; the boundary is the operator's own next action.

**Why:** The queue auto-advances on the row-success confirmation, so a reflexive Flag after "that one was wrong" would otherwise defer the next row as missing while the bad reading stayed live — the mis-attribution F6 exists to prevent.

### F17 — A quick re-scan or ad-hoc add never wakes an interrupted bulk session (2026-09-06, round-2 delta, R2-D1)

**Decision:** When a collection holds an unresumed interrupted bulk session, a re-scan or an ad-hoc capture runs as a one-row session of its own and ends itself; the interrupted session is untouched and still waits for a deliberate "Resume capture". This replaces round-2 attach rule (c) ("resuming comes first … the bulk session stays active"). The other two attach rules stand: blocked while another collection's session is active or paused; attaches to a paused bulk session on this collection for that one row. An interrupted session is not in flight, so a one-row session beside it does not break "one in-flight session per collection". A one-row session that is itself interrupted (crash mid-scan) is simply closed on relaunch: its row stays as it was, and there is no resume ceremony for it.

**Why:** Both cross-model lenses objected: a Cataloger doing a one-off scan was forced through the full bulk-session resume and left inside the live queue. The owner chose the simplest rule.

## Rejected findings

- **R1-F22** (agy product-manager, Blocker, round 1): "Immediate undo journey is missing; UJ3.2 mentioned but missing from the detailed text." Rejected: UJ3.2 exists with "Re-take sample" and "Restart item"; the reviewer's cited line numbers do not correspond to the document.
- **R1-F23** (agy product-manager, Major, round 1): "Cut mid-session drag reordering." Rejected as re-litigating fence F5 ("before or during a session"); the one-handed drag ergonomics concern is already a named open question.
- **R1-F25** (agy test lens, round 1): the whole review cites journeys and text that do not exist in the document (UJ5.2, UJ5.4, a 15-second timeout, a manifest). Non-conforming; not counted as the lens having run on that route. The Claude test lens stands.
