# Capture Mode PRD — fences

Owner decisions and owner-rejected findings for `prd-capture-mode.md`. Every entry is settled: it is carried into every review brief and editing dispatch, and is not re-litigated. Format per `operator-agents:writing-prds`.

Review log: docs/agent-reviews/2026-09-06-prd-capture-mode-peer-reviews.md (created round 1, 2026-09-06; later rounds append)
Owner-locked row IDs: (none yet — rows R1.1–R11.10, E1–E37, M1–M8 exist at ⌛️ Ready for Alignment as of commit 6e0ec45; none locked)

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

### F18 — The consecutive-failure guard counts only instrument-caused deferrals (2026-09-06, round-3 closing check)

**Decision:** N_CONSEC_FLAGGED counts consecutive rows the instrument's own failed readings set aside (a row deferred after K_FAILED_ATTEMPTS, or by Skip after a failed attempt, or by a set that disagreed). Rows the operator deferred by choice — Flag as missing or damaged, Skip mid-set with no failure, Flag on a row that just landed — never count. The guard watches the instrument, not the operator's decisions. The constant's trigger clause reads "deferred by the instrument".

**Why:** Flagging five missing markers in a row must not pause the session and prompt recalibration; the guard exists to catch a systemic instrument problem.

### F19 — Entering the deferred-row review mid-session discards the current item's partial set (2026-09-06, requirements fill, fork 1)

**Decision:** The one partial-set rule has no look-only exception: opening the review puts a different item under the instrument, so the samples taken so far on the current item are discarded and the row returns to "sample 0 of N". Closes OQ 18.

**Why:** The rule's value is that it has no exceptions; a look-only entry is a later refinement if dogfood asks for it.

### F20 — The review's surface and the session summary's form follow the seam decision (2026-09-06, requirements fill, forks 2 and 3)

**Decision:** Where the deferred-row review lives relative to the recents strip (same surface, adjacent, separate) and whether the end-of-session summary is a state or a dialog are settled together with the seam (OQ 8, ADR-0004), by the Demo Device prototype of each reading. Until then the summary is specified as a state on the collection surface so counts and items-per-hour stay readable, and the review rows stay gated on OQ 8. OQ 10 and OQ 17 are folded into OQ 8's closer.

**Why:** Both are the same surface question the seam decides; answering them first would pre-decide part of ADR-0004.

### F21 — Find matches code, name, and both alternates (2026-09-06, requirements fill, fork 4)

**Decision:** Find on the capture surface matches Swatch Code (from the start), Swatch Name (any part), and the alternate code and alternate name the same way, all under the one matching rule (F11); when a hit matched an alternate, the result says which field matched. Closes OQ 15.

**Why:** A swatch is known by more than one name; that is why the alternates exist.

### F22 — Capture owns the queue order; Collection Mode inherits the pre-session sort and drag controls (2026-09-06, requirements fill, fork 5)

**Decision:** This PRD defines what a reorder does to the queue (F5, §6). The sort and drag controls the operator uses on the collection view before a session are an inherited obligation for the Collection Mode PRD; R6.8 carries the marker. The capture surface's queue list remains this PRD's.

**Why:** Keeps ownership clean between the two PRDs.

### F23 — Guard mode: record-only in dogfood, enabled at v1; a dogfood build is not a release (2026-09-06, requirements fill, fork 6)

**Decision:** The consecutive-failure guard ships record-only during the dogfood phase (it counts and records, never pauses) so the constants can be tuned from real sessions, and ships enabled at v1 once OQ 3's data exists. A dogfood build does not count as a release for the Legend's rule that no provisional constant ships with its OQ unresolved. Closes the default half of OQ 6.

**Why:** The guard's numbers are clinical-analyser numbers until tuned; pausing dogfood sessions on untuned constants would poison the tuning data.

### F24 — Priority split: bulk critical path and version history P0; re-scan, ad-hoc capture, and reordering P1 (2026-09-06, requirements fill, fork 7)

**Decision:** Collections, import, the session, the scan loop, per-scan failure and the guard, queue navigation by find and skip, pause/end/interruption/resume, the deferred review, the Demo Device rows, and the version-history record shape (R8.13) are P0. The correction path (re-scan rows in §8), ad-hoc capture and one-row sessions (§9), and reordering (§6 reorder rows) are P1. Priority is build order within v1, not a cut line.

**Why:** Matches the vision's feature list; Data Foundation needs the record shape in the first build even though the re-scan journey that fills it comes second.

### F25 — The seam rows are P0 and the ADR-0004 prototype is first-build work (2026-09-06, requirements fill, fork 8)

**Decision:** R10.1–R10.7 stay P0, gated on OQ 8. A Demo Device prototype of each reading (A: modal takeover; B: capture as a state of the live collection) is part of the first build phase and is what closes OQ 8 and ADR-0004's gate. The research default (Reading B) is recorded as the default to confirm or overrule, not adopted.

**Why:** The first build needs the seam answered; deciding it without a prototype would forfeit the evidence the two research passes disagree on.

### F26 — A cue at the moment held samples are discarded; no confirm (2026-09-06, round 5, R5-F13)

**Decision:** When an action discards the samples taken so far on the current item (the operator's Pause, a jump, Add item, entering the review, a mid-session re-scan by find), a distinct two-sense cue fires at the moment of discard and the resulting state's copy says how many samples were dropped. No confirmation dialog: the heads-down loop stays free of prompts, and the operator always knows. R8.13's promise is bounded to readings that reached a saved set or a deferred row's record.

**Why:** Three lenses found the silent discard at odds with "corrections never destroy data" in spirit; a confirm would cost a keypress on every navigation.

### F27 — The agreement check is record-only through dogfood and prompts at v1 (2026-09-06, round 5, R5-F15)

**Decision:** Like the guard (F23), the sample-agreement check has an explicit mode: record-only (every set is accepted, its spread recorded, no prompt) through the dogfood phase, and enabled (the three-way choice of F10) at v1 once OQ 3's data has tuned SAMPLE_TOLERANCE. A dogfood build is not a release for the no-unresolved-constant rule.

**Why:** SAMPLE_TOLERANCE is a placeholder with no evidence behind it; prompting on it during dogfood would shape the data meant to tune it and break the heads-down path.

### F28 — The agreement check uses fixed D50/2°, recorded on the reading; the display default is D50/2° (2026-09-06, round 5, R5-F5)

**Decision:** The agreement check always computes ΔE2000 under D50/2° regardless of the collection's display illuminant and observer, and that basis is recorded on the reading with the spread. Display defaults stay display-only (F2). The collection's display defaults arrive pre-filled at D50/2°, as F2 named and the row had dropped.

**Why:** An acquisition gate must not key off a viewing preference; changing display defaults would otherwise change future capture outcomes and make tuning data incomparable.

### F29 — A lost-unflushed-writes stand-in makes the power-loss promise assertable (2026-09-06, round 5, R5-F8)

**Decision:** This PRD adds an obligation on the simulated layer (implemented by Data Foundation's store): a test can induce the loss of everything not durably flushed at an arbitrary point, and assert that every row the operator was told landed is present afterwards; drive detachment is the same stand-in. The row-success confirmation's cost is floored by the durable-commit time on the user's volume, so ROW_CONFIRM_BUDGET is measured on real volume classes, not only on the Demo Device.

**Why:** A force-quit test passes on a build that flushes lazily; without a stand-in the promise in R4.13 is unverifiable in CI.

### F30 — Session completion is measured per collection over session chains (2026-09-06, round 5, R5-F14)

**Decision:** M3 becomes the share of collections fully adjudicated plus days-to-adjudication; a resumed chain counts once. Session endings are split into "ended deliberately" and "abandoned mid-queue after a failure", and the second is its own metric. M2 and M5 likewise count over chains, with a re-scan attributed to the session it occurred in against a cumulative captured-row denominator.

**Why:** The metric as written scored the two-sitting behaviour §7 is built for, and any crash chain, as failure.

### F31 — The review offers "Leave all set aside" with one optional note (2026-09-06, round 5, R5-F39)

**Decision:** The deferred-row review offers a single action that marks every remaining set-aside row as deliberately left, recording one note for all; per-row "Leave it set aside" with its own note remains. Session completion after "Flag remaining as missing" is one keypress.

**Why:** A hundred rows flagged missing must not need a hundred decisions.

### F32 — The trigger lockout lasts until the in-flight reading returns or fails (2026-09-06, round 5, R5-F3)

**Decision:** A trigger press is rejected, with the rejection cue, until the in-flight measurement completes or fails; OQ 4 narrows to whether any additional dead time follows. A measurement already in flight when any queue action fires belongs to the row that was current at its accepted trigger; if that row no longer accepts samples when the reading arrives, the reading is discarded and recorded as an attempt in that row's history, never applied to another row. The simulated layer gains a per-measurement delay/hang injection so this is assertable, and its measurement record carries the row current at the accepted trigger and the row the reading was saved to.

**Why:** A fixed dead time shorter than the scan cycle would accept a second press mid-measurement; an in-flight reading with no owner is the mis-attribution F6 and F16 exist to prevent.

### F33 — The average is taken across the spectral curves (2026-09-06, round 5, R5-F4)

**Decision:** The mean of a set is the average of the samples' reflectance curves; colour values, including the ones the agreement check compares, are derived from that mean under the fixed reference (F28). The basis and a derivation version are recorded on the reading so the average can be recomputed. Individual readings are never discarded in favour of the average.

**Why:** Two builds would otherwise give different agreement verdicts and different canonical averages; the spectral mean matches raw-payload-as-canonical.

### F34 — Each collection carries a chosen scan mode, default M1 (2026-09-06, round-5 fix pass, fork 2)

**Decision:** A collection carries a chosen scan mode (M0, M1, or M2 as the instrument's firmware offers), default M1 on Spectro 2, set at creation beside samples-per-row and editable between sessions. The agreement check runs on that mode and the derived colour values shown by default come from it; every recorded mode is kept with the reading. Whether one measurement returns every supported mode, or the app must choose, stays OQ 1 (per SDK docs; confirm on hardware).

**Why:** The round-5 fix pass introduced "the collection's chosen scan mode" without a home; F2 covered only samples-per-row and display defaults.

### F35 — The seam prototype's scripted protocol runs with the owner only (2026-09-06, round-5 fix pass, fork 5)

**Decision:** PROTOTYPE_PARTICIPANTS = 1: the owner runs the scripted task on both readings of the seam. The tie-break and observables in R10.7 stand; the count is the owner's, not a placeholder.

**Why:** Fastest path to closing OQ 8; outside participants are hard to recruit for an unreleased tool.

### F36 — Without the spectral entitlement, capture proceeds on the colour-value mean (2026-09-07, round 6, R6-F2)

**Decision:** When the license lacks the spectral-data entitlement, capture still runs: the set's mean is taken across the instrument's colour values under the fixed D50/2° reference (F28), that basis is recorded on the reading, and the reading is marked non-spectral. With the entitlement present the spectral mean of F33 applies. A collection may hold both kinds; the mark says which.

**Why:** The locked device PRD lets a valid license without spectral data reach capture; a Cataloger with a basic license must still be able to digitize a collection.

### F37 — The guard counts consecutive rows the instrument failed on, not consecutive presses (2026-09-07, round 6, R6-F6)

**Decision:** N_CONSEC_HARD counts consecutive rows that each ended in an instrument-caused deferral (auto-deferred after K_FAILED_ATTEMPTS, or skipped after a failed attempt); repeated failures on one swatch are that swatch's problem and auto-defer it without pausing the session. N_CONSEC_FLAGGED keeps its meaning (F18) and the two counters may merge if tuning shows them redundant (OQ 3). The N_CONSEC_HARD < K_FAILED_ATTEMPTS invariant is withdrawn.

**Why:** Counting presses meant two light-leaks on one awkward swatch paused a heads-down run before the row could auto-defer.

### F38 — The seam tie-break is symmetric and repeated (2026-09-07, round 6, R6-F7)

**Decision:** The owner (F35) runs the same scripted task on each reading PROTOTYPE_RUNS times (candidate 3, OQ 8), alternating order; the reading with fewer mode slips across all runs wins, and a tie goes to fewer steps-to-the-just-captured-row. No single slip decides ADR-0004. The pre-commitment stands.

**Why:** The zero-tolerance rule applied only to Reading B and would have decided the document's costliest one-way door on one observation.

### F31 — clarification (2026-09-07, round 6, R6-F4)

**Recorded by the orchestrator as the plain consequence of F31, flagged for the owner:** a row the operator deliberately left set aside (singly or with "Leave them all set aside") is settled. Routing and completion treat settled rows as adjudicated: a collection whose set-aside rows are all settled is finished (E2's finished variant, "Every swatch here is scanned or set aside for good"), and starting capture on it does not reopen the review. A settled row can still be re-scanned from the collection or the set-aside list by choice.

### F6 — amendment (2026-09-07, round 7, R7-F5)

**Recorded by the orchestrator as the consequence of F37, flagged for the owner:** K_FAILED_ATTEMPTS counts failed attempts on one row across any accepted samples that fall between them ("consecutive" in F6's original wording is withdrawn); an accepted sample neither resets nor counts. The guard's counters (F37) count rows, so a row with interleaved failures contributes at most one to them.

### F39 — The set-aside review is offered while any set-aside row exists (2026-09-07, round 8, R8-F2)

**Decision:** "Review the set-aside swatches" is offered whenever the collection holds any set-aside row, settled or not, and is not offered when none is. The end-early summary (R7.5, E23) uses the same condition. "Unsettled" governs only completion and automatic entry: the queue running out or a session starting on a collection with only settled set-aside rows completes or shows the finished variant rather than opening the review (R8.1 routes 1 and 3, R8.6, R3.9).

**Why:** F31 as clarified promises that a row the operator left for good can still be re-scanned from the set-aside list. An offer gated on unsettled rows would leave no door into that list once every row was settled, stranding the promise in R3.9, R8.5, R8.15, and E33.

### F40 — N_CONSEC_HARD has a floor of 2 (2026-09-07, round 8, R8-F3)

**Decision:** Whatever OQ 3 tunes, N_CONSEC_HARD is never below 2. Stated once in R5.9 and in OQ 3; [§11](prd-capture-mode.md#11-demo-device-and-verifiability)'s preamble script cites the floor rather than claiming that no tuning can invalidate its interleaved-failures step.

**Why:** A guard that pauses on a single row's auto-deferral is not watching for a run of instrument failures; it would also make the auto-defer-without-pause path of R5.9 unreachable and falsify that step.

### F41 — Cancelling a mid-session "Add a swatch" keeps the partial set (2026-09-07, round 8, R8-F5)

**Decision:** Any offer the operator cancels leaves the samples taken so far in place: cancelling the mid-session re-scan offer (R6.3, E28) and cancelling "Add a swatch" (R9.10, E32) both return the operator to the row they were on with its samples intact. Only an add the operator sees through, or a confirmed re-scan, discards them, with the discard cue and E42's line.

**Why:** Neither cancel puts a different item under the instrument, which is the one reason R7.1 gives for a discard. One rule is easier to hold than two that differ by which dialog was open.

### F42 — Opening the set-aside list from the collection is a browse; the leave actions are offered only on unsettled rows (2026-09-07, round 9, R9-F2)

**Decision:** The set-aside list shows every set-aside row, and where a row was settled it shows the decision and its note. "Leave it set aside" and "Leave them all set aside" are offered only on rows not yet settled. Choosing "Review the set-aside swatches" from the collection surface opens the list as a browse, not a session: no pre-flight gate runs and no session starts until the operator scans a row from it, which runs as a one-row session (R3.11) with the gate. A session that opens into the review on its own (R3.9) is unchanged.

**Why:** F39 keeps the door open on a finished collection so a row left for good can still be re-scanned from the list; that door must not offer decisions about rows already decided, and looking at the list must not cost a session.

### F43 — A state shown on the collection surface never hides that surface's entry points; E24 says set-aside rows stay reachable (2026-09-07, round 10, R10-F6)

**Decision:** The completion summary (E24) gains one sentence saying the rows left set aside are still there, still marked, and can be scanned again any time; it carries no review action of its own. The document states once that a state rendered on the collection surface never hides that surface's entry points, so the door fence F39 puts on the collection surface is there while E24 shows.

**Why:** The reversal is said everywhere else (E2, E23, E27) and E24 is where a Cataloger most plausibly reads "for good" as final; a second copy of the offer inside E24 would give the same door two homes.

### F44 — Leaving a review entered with rows still pending returns to the queue (2026-09-07, round 13, R13-F3)

**Decision:** A set-aside review entered while rows are still pending is a detour: leaving it returns to the queue at the remembered row and the session goes on. "Leaving the review before every row is captured or left ends the session early" (R8.6, UJ3.3) applies only to the end-of-queue review — one entered with the queue exhausted, or taken from the end-early summary's "now".

**Why:** Under the live-collection reading the review can be reached mid-run from the collection surface; ending the session because the operator glanced at the set-aside rows would cost the heads-down loop a resume for nothing.

### F45 — Requirement rows are at most two sentences, and concise over prose (2026-09-07, round 15, owner)

**Decision:** Every §7 requirement row is at most two sentences, and the whole table gets shorter, not longer. Each row states one rule plainly; a second sentence is allowed only where the observable a test reads back is not obvious from the rule. Rationale, precedent, fence citations, and restatements of other rows' rules leave the rows — fences to the Traceability paragraph, research to the briefs. A row is split into new rows only where it carries rules that genuinely differ and a builder needs each on its own; nuance that can be dropped is dropped, and the row count must not grow beyond what those splits strictly require. The set-aside seam (R8.1, R8.6, R8.15) becomes one compact state-by-exit table rather than prose. The §12 copy table and the §9 metrics are exempt. Applied as one compaction pass after the round-15 fix, then verified by a full round over every row with the before-and-after diff in hand.

**Why:** The PRD is read by agents building plans, not by humans. Long prose rows carrying several rules were restated in sibling rows and diverged every round; ambiguity is reduced by saying less, precisely, not by saying more. Compaction is not licence to expand the requirements.

### F46 — The PRD is restructured for agent readers: journeys and copy move to companion files; trims and row cuts apply; buildability gaps go to the engineering plan (2026-09-07, round 16, owner)

**Decision:** (1) The user journeys and four of the five diagrams move verbatim to `prd-capture-mode-journeys.md`, with only UJ3 kept inline as a short numbered list; the §12 copy table moves verbatim to `prd-capture-mode-copy.md`, its two contract paragraphs (Labels, Placeholders) staying in the PRD; the row-state diagram, the Surfaces table, and every requirement row stay. (2) Each Open Question's Details cell shrinks to decision, interim rule, closer, and rows; the inherited-obligations paragraph becomes a three-column table (target PRD, obligation, rows); the fence-to-row map moves to this file; the copied use-case tables and the battery note go. (3) The plan reviewer's row cut list applies: rows that duplicate another are removed (their IDs retired, never reused), pure hand-off rows move into the obligations table, R4.25 merges back into R4.21, restated definitions go, and R11.9 and R11.15 become a preamble script and a two-column table; every remaining rationale clause a lens named is cut. (4) The buildability gaps the plan reviewer raised — the five constants at "candidate TBD", the four P0 rows deferring to an open question without an interim rule, the phase-0 predecessors, the hardware spike's scope, the dogfood entry point, and §10's build order — are left for the engineering plan, recorded in the PRD as one short hand-off list, not answered here.

**Why:** The document is read by agents building plans. At 33k words with 54% outside the rows it was twice what a builder needs; the moves lose no rule, and the E-ID contract keeps copy and rows in sync.

### F47 — The first build phases its offers; fence F24 stands (2026-09-07, round 18, priority pass)

**Decision:** A P0 row's state that offers an action whose rows are P1 — "Re-scan" (R8.7–R8.10, R8.12), "Add a swatch" (R9.1–R9.11), mid-session reorder (R6.8, R6.11) — is shown in the first build without that action; the offer arrives with the P1 rows. R11.15's surface lists and R7.19's entry points are asserted per phase. R10.7's reorder observable is recorded once reordering lands; R10.8's tie-break (mode slips, then steps to the just-captured row) is unaffected, so the seam prototype still closes OQ 8 in the first build. M5 is measured from the P1 build onward.

**Why:** Six lenses found the P0 set offering doors with nothing behind them; phasing the offers keeps the owner-confirmed P0/P1 split (F24) and costs one Legend sentence rather than twenty rows.

### F48 — One guard counter (2026-09-07, round 18, R18-F2)

**Decision:** The consecutive-failure guard has one counter, N_CONSEC_HARD (candidate 2, floor 2 — OQ 3), counting consecutive rows the instrument set aside by any route: auto-deferred after K_FAILED_ATTEMPTS, "Skip" after a failed attempt, or a set that disagreed. N_CONSEC_FLAGGED is retired; F18, F37, and F40 read as one counter; OQ 3 tunes one number.

**Why:** The two counters had identical event sets — a disagreeing set is a failed attempt and its only set-aside route is a Skip after it — so the second could never fire first and no test could tell it from its absence. OQ 3 already said they might merge.

### F36 — clarification (2026-09-07, round 7, R7-F2)

**Recorded by the orchestrator as the consequence of F36, flagged for the owner:** the non-spectral capture path is handed to the device PRD as an inherited note — its "License missing spectral data" state becomes a capability notice with a forward action (capture continues, readings marked non-spectral, colour shown under D50/2°) and its §2 "core payload" wording is softened — and this PRD adds a capture-surface indicator while a session runs non-spectral, with the mark's surfacing in Collection Mode handed to that PRD.

### F38 — clarification (2026-09-07, round 7, R7-F20)

**Recorded by the orchestrator, flagged for the owner:** PROTOTYPE_RUNS = 3 is the owner's fixed number, like PROTOTYPE_PARTICIPANTS (F35), not a provisional constant closed by OQ 8 — the prototype cannot both consume and close it.

### F40 — clarification (2026-09-07, round 9, R9-T1)

**Owner decision, on the orchestrator's question:** the floor of 2 is a property of the guard, not of one counter. One auto-deferred row counts toward both N_CONSEC_HARD and N_CONSEC_FLAGGED, so no guard counter is ever below 2, whatever OQ 3 tunes. R5.9, OQ 3, and [§11](prd-capture-mode.md#11-demo-device-and-verifiability)'s preamble script state it that way.

### F42 — clarification (2026-09-07, round 10, R10-F1, R10-F2)

**Owner decision, on the orchestrator's questions:** (1) A scan from the browse-opened set-aside list is a re-scan by choice and never moves the collection's remembered row (R3.11 holds); the review-selected update of R3.6 and R8.3 applies only to a row selected in a review entered inside a session. (2) Where the collection's bulk session is paused, a scan from the browse-opened list is a row inside that session (R9.6), mirroring R8.7; with no session open it is a one-row session. The browse itself has no partial set to discard.

### F42 — clarification (3) (2026-09-07, round 11, R11-F1)

**Recorded by the orchestrator as the plain consequence of F19, F42, and UJ3.3/UJ3.4, flagged for the owner:** browse versus in-session review is decided by whether a session is capturing on the collection, not by which surface the offer was taken from. With no session open, with an unresumed interrupted session (a one-row session beside it, F17), or with the bulk session paused (its partial set already let go at Pause, R7.3), the list opens as a browse: no gate, no session of its own, no change to the remembered row, nothing to discard. While the bulk session is capturing — whether the offer is taken from the collection surface under Reading B or from the end-early summary (E23, "now") — it opens the in-session review of UJ3.3: the partial set is discarded with the cue and the header line (F19), a selected row becomes the remembered row marked review-selected (R3.6, R8.3), and the session goes on to complete (R8.6) or end early (R7.5).

### F42 — clarification (4) (2026-09-07, round 12, R12-F1)

**Recorded by the orchestrator, flagged for the owner:** the discriminator in clarification (3) is a *live bulk session* on the collection — a bulk session that is active and neither paused nor interrupted, its open set-aside review included — and never the capture-time accounting of R3.1, which excludes time in the set-aside list from elapsed capture time for a different reason. A one-row session that is itself under way (an ad-hoc add, a re-scan, or a scan started from the look-through list) is not a live bulk session: with one under way the list is still a look-through, the remembered row does not move, and selecting another row ends or abandons that one-row session (R3.11) with its own partial set let go under R7.1. The term is defined once, in the Vocabulary, and R3.6, R3.11, R7.1, R8.1, R8.3 use it.

### F31 — clarification (2) (2026-09-07, round 13, R13-F1)

**Recorded by the orchestrator as the plain consequence of F31 and R7.11, flagged for the owner:** an unresumed interrupted bulk session on a collection that has nothing pending and no set-aside row unsettled is closed as complete whenever that becomes true — on relaunch, as R7.11 already says, or at the moment a look-through decision settles the last row — and its "Resume capture" offer is withdrawn, so the collection reads finished with no stale session beside it.

### F42 — clarification (5) (2026-09-07, round 13, R13-F2)

**Recorded by the orchestrator, flagged for the owner:** a bulk session held by the guard's pause (R5.11) or by a device halt is not a live bulk session, and the set-aside list opens to look through under both. A pause lifted for a single row (R9.6) is still a pause for this purpose: whether a live bulk session is on the collection is decided when the list opens and holds until it closes, so a scan started from the look-through list never turns the list into the in-session review part-way.

### F44 — clarification (2026-09-07, round 14, R14-F1, R14-F2)

**Recorded by the orchestrator as the plain consequence of F44's own words, flagged for the owner:** (1) the end-early summary's "now" is the end-of-run review whatever is still pending, because the operator has already asked to stop; F44's detour rule covers only a review the operator opened from the collection surface while the bulk session was still capturing. (2) A detour review moves nothing: it does not update the collection's remembered row or mark a row review-selected — those belong to the end-of-queue review (R3.6, R8.3, R3.8) — so leaving it returns the operator to the row they were on when they entered, as UJ3.3 promises.

### F44 — clarification (2) (2026-09-07, round 15, R15-F2)

**Recorded by the orchestrator as the plain consequence of F44 and F15, flagged for the owner:** the end-of-run review has three ways in — the queue exhausted, the end-early summary's "now", and a resume that opens into the review at a review-selected row (R3.8) with the queue already exhausted. A resume that opens into the review at a review-selected row while rows are still pending is the detour: the operator pressed "Resume capture" rather than asking to stop, so leaving that review returns them to the queue at the row they were on and the session goes on.

### F31 — clarification (3) (2026-09-07, round 15, R15-F5)

**Recorded by the orchestrator as the plain consequence of F19 and F31, flagged for the owner:** "Leave them all set aside" settles every set-aside row still outstanding, the one under the instrument included. Where that row holds a part-finished set — a review-selected row in the end-of-run or detour review, or the row of a one-row session started from the look-through list — those samples are let go under the one partial-set rule, with the discard cue and E42's set-aside line, and the one-row session ends. R7.1 owns that discard; R8.15 cites it.

## Fence → row map

Which rows in [`prd-capture-mode.md`](prd-capture-mode.md) carry each fence. The rows do not cite fences and no row re-argues one; this map is the link. Letters `R8.1a`–`R8.1k` are the rows of the state-by-exit table in that document's [§8](prd-capture-mode.md#8-deferred-row-review-and-corrections), in the order they appear.

- **F1** no Library — R1.1. **F2** measurement-settings scope — R1.3, R1.5, R1.10, R4.6. **F3** rename and delete are Collection Mode's — R1.8. **F4** the import target is chosen in the app — R2.5. **F5** jump by code plus manual reordering — R6.7, R6.8, R6.11.
- **F6** a per-scan failure holds the row, as amended — R5.3, R5.4, R5.13, R8.4. **F7** drift and no-reading are device halts, no per-scan timeout — R5.1, R5.2. **F8** N_CONSEC_DRIFT dropped — R5.10. **F9** "Flag" moves a captured row to set aside — R5.6, R5.8, R9.9. **F10** accept the average with the spread recorded — R1.4, R4.10, R4.11, R8.10.
- **F11** Swatch Code normalisation — R1.2, R2.7, R2.8, R6.1, R9.2. **F12** the session is a named entity, the binding released at quit — R3.1, R3.2, R3.3, R3.11, R3.13, R7.12. **F13** the mid-session insert stays in v1 — R9.10, R9.11. **F14** a resume is a new session and the old one is closed — R3.2, R3.4, R7.12, R7.18. **F15** the collection remembers the last current row — R3.1, R3.6, R3.7, R3.8, R7.5.
- **F16** "Flag" targets the row that just landed — R4.21, R5.6, R5.7, R5.8. **F17** a quick re-scan never wakes an interrupted session — R3.11, R3.13, R7.13, R9.7, R8.1b, R8.1e. **F18** the guard's one counter counts only instrument-caused deferrals, as amended by F48 — R5.9, R5.10, OQ 3, and [§11](prd-capture-mode.md#11-demo-device-and-verifiability)'s preamble script. **F19** entering the review discards the part-finished set — R7.1, R8.1f, R8.1g, R8.1j. **F20** the review's surface and the summary's form follow the seam — R4.15, R7.15, R8.1.
- **F21** find matches code, name, and both alternates — R6.1. **F22** capture owns the queue order — R6.7, R6.8. **F23** the guard is record-only in dogfood — R5.12. **F24** the priority split — the Pri column throughout, and the [Legend](prd-capture-mode.md#legend). **F25** the seam rows are P0 and the prototype is first-build work — [§10](prd-capture-mode.md#10-the-seam-capture-to-collection)'s preamble, R10.6, R10.7.
- **F26** a cue at the moment samples are discarded, no confirm — R7.17, R8.13. **F27** the agreement check is record-only through dogfood — R4.23. **F28** the check runs at a fixed D50/2° — R1.5, R4.9, R4.24. **F29** the lost-unflushed-writes stand-in — R4.13, R11.10. **F30** completion measured per collection over chains — M2, M3, M5, M9.
- **F31** "Leave them all set aside", as clarified three times — R1.7, R3.9, R7.11, R7.16, R8.5, R8.15, R8.1f, R8.1g, R8.1h, R8.1i. **F32** the trigger lockout lasts until the reading returns — R4.3, R4.18, R11.5. **F33** the average is taken across the spectral curves — R4.12. **F34** each collection carries a chosen scan mode — R1.6, R1.10, R4.5. **F35** the prototype protocol runs with the owner only — R10.8.
- **F36** capture proceeds without the spectral entitlement, as clarified — R4.22, R4.24, R4.26, R8.14. **F37** that one counter counts rows, not presses, as amended by F48 — R5.9, R5.14, OQ 3, and [§11](prd-capture-mode.md#11-demo-device-and-verifiability)'s preamble script. **F38** the seam tie-break is symmetric and repeated, as clarified — R10.8. **F39** the review is offered while any set-aside row exists — R8.16 (and [E2](prd-capture-mode-copy.md#error--state-copy)'s finished variant). **F40** that one counter has a floor of 2, as clarified and as amended by F48 — R5.9, OQ 3, and [§11](prd-capture-mode.md#11-demo-device-and-verifiability)'s preamble script.
- **F41** cancelling an offer keeps the part-finished set — R6.3, R7.1, R9.10. **F42** look-through versus in-session review, as clarified five times — R3.6, R3.11, R8.1, R8.2, R8.5, R8.15, R8.1a–R8.1k. **F43** a collection-surface state never hides that surface's entry points — R7.19. **F44** leaving a detour returns to the queue, as clarified twice — R3.8, R8.1, R8.6, R8.1j, R8.1k. **F45** every requirement row is at most two sentences — every row in [§1](prd-capture-mode.md#1-collections) through [§11](prd-capture-mode.md#11-demo-device-and-verifiability). **F46** the restructure — the two companion files, the trims, and the retired IDs listed in the [Legend](prd-capture-mode.md#legend).
- **F47** the first build phases its offers — the [Legend](prd-capture-mode.md#legend)'s priority paragraph, R10.7, M5. **F48** one guard counter — R4.21, R5.9, R5.10, R5.11, R5.14, and OQ 3.
- Retired under F46, never reused: R4.25, R7.4, R7.6, R7.10, R8.11, R10.2, R11.1, R11.2, R11.4, R11.9.

## Rejected findings

- **R1-F22** (agy product-manager, Blocker, round 1): "Immediate undo journey is missing; UJ3.2 mentioned but missing from the detailed text." Rejected: UJ3.2 exists with "Re-take sample" and "Restart item"; the reviewer's cited line numbers do not correspond to the document.
- **R1-F23** (agy product-manager, Major, round 1): "Cut mid-session drag reordering." Rejected as re-litigating fence F5 ("before or during a session"); the one-handed drag ergonomics concern is already a named open question.
- **R5-X1** (agy product-manager, Blocker, round 5): "The Open Questions table lacks an Owner column." Rejected: the house table's "Evidence source" column names the closer (owner, hardware, SDK, dogfood), as the locked device PRD does.
- **R5-X2** (agy staff engineer, Blocker, round 5): quotes the SDK audit as saying scan() while scanning throws IllegalStateException, that no physical button event is exposed, and that every measurement returns all modes simultaneously. None of those sentences is in the audit (it records a "busy" status and a result dictionary keyed by scan mode). The substance about the lockout is carried by R5-F3 / fence F32; OQ 1 and OQ 2 stay open pending hardware.
- **R5-X3** (agy architecture, Major, round 5): global collection-name uniqueness contradicts portable per-collection files. Rejected: rests on the per-collection-file reading that R5-F2 removes; under F1 one file holds the collections and uniqueness within it is well-defined.
- **R5-X4** (agy architecture, Minor, round 5): a column sort mutating every row's queue order synchronously is "architecturally ugly". Rejected as HOW; the WHAT (queue order is a per-row attribute changed only by explicit reorder) stands.
- **R1-F25** (agy test lens, round 1): the whole review cites journeys and text that do not exist in the document (UJ5.2, UJ5.4, a 15-second timeout, a manifest). Non-conforming; not counted as the lens having run on that route. The Claude test lens stands.
- **R9-X1** (agy product-manager, Blocker, round 9): "UJ3.4 step 3 says End session has no cancel button, but E23 adds a Keep scanning action." Rejected: the no-cancel rule (UJ3.4 step 3, R4.16) is about the capture surface — no control abandons a session or a row from the counting surface — while E23 is the confirmation that UJ3.4 step 4 has the operator give; declining a confirmation is not an abandon control. The finding's tense point is PM9-1, accepted as R9-F4.
- **R10-X1** (agy product-marketing, Major, round 10): "E27 and E39 omit the 'only on unsettled rows' condition from their action cells." Rejected: the condition governs whether the set-aside list offers the leave actions (R8.5, R8.15, fence F42), not an action inside the confirmation those offers open; reaching E27 or E39 presupposes the offer was made. All four Claude lenses that judged it called it a false positive.
- **R10-X2** (agy product-marketing, Major, round 10): "E24 must carry the review action F39 requires." Overruled by the owner as fence F43: E24 says the rows stay reachable and carries no review action; the door lives on the collection surface it renders on.
