# Peer-review gate — prd-capture-mode (2026-09-06)

**Mode:** requirements. **Subject:** `docs/product/prd-capture-mode.md` at commit `e5c43d1` (journeys only — no Requirements, Error & State, Success Metrics, or Open Questions tables yet, so every per-row disposition this round is ABSTAIN and findings are journey-level). **Fence file:** `docs/product/prd-capture-mode-fences.md` (F1–F5, passed as `--source`). **Reviewers:** peer-product-manager-reviewer, peer-staff-software-engineer-reviewer, peer-test-reviewer (retargeted), peer-architecture-reviewer. **persona-version:** cache/1.4.0. **Caller:** `operator-agents:writing-prds`, Phase 4 round 1.

**tier-rationale:** PRD tier always-on lenses (product manager, staff engineer, retargeted test). Tier 3 by document shape: `peer-architecture-reviewer` ran because the seam journey (UJ5, gating ADR-0004), the queue-in-the-store durability model, and the flat-collection model after F1 interact with decided architecture (ADR-0001, device PRD §5). Skipped: `peer-product-marketing-manager-reviewer` (no end-user copy yet — copy rows come with the requirements pass), `peer-privacy-reviewer` (no personal data or network paths in scope), Tier 2 security/database/plan (no code, no schema, no plan). `peer-plan-reviewer` is reserved for the mandatory pre-lock round.

**cross-model pass:** run on `agy` for all four lenses at the owner's request (`review-gate cross-model --runtime agy`, serialized; all four rc 0). Outcome per lens: PM and staff and architecture produced conforming reviews and are logged below; the agy test review is **non-conforming** — its findings cite journeys and text that do not exist in the document (UJ5.2, UJ5.4, a 15-second timeout in UJ3.3, a "manifest" in UJ4.3, a haptic pulse in UJ2.1) — so it is recorded as not-run and the Claude test lens stands for that route. Note: the writing-prds skill offers the cross-model pass once on the first full round; this round is journeys-only, so the offer is not consumed.

**Gate outcome:** not aligned — 3 consensus Blockers and 15 consensus/accepted Majors handed to owner adjudication. No row flips this round (no rows exist).

## Round 1

**Lenses run:** Claude route — product manager, staff engineer, test (retargeted), architecture. agy route — product manager, staff engineer, architecture (test non-conforming, see header).

### Findings

#### peer-product-manager-reviewer (Claude)

Verdict: sound after fixing Blockers.

- **Blocker** `:271-272`, `:250-251`, flowchart `:303-304` — UJ3.1 step 3 says the failed row is deferred and the queue advances, and in the same step Retry "re-takes in place (the item stays current)". Both cannot be true. A heads-down operator's reflexive re-press after the warning tone lands on the next row: silent mis-attribution. Fix: pick one; the safe default holds the row, next press is the retry, moving on is a deliberate queue action.
- **Major** `:274` vs `:81-94`, `:290` — Flag row "even though the reading succeeded" acts on an already-captured row; the state diagram has no Captured → Deferred edge and UJ3.8 says a good reading is never demoted.
- **Major** `:250-251`, `:328-333` — no "accept the average" path for a set that disagrees; textured/fabric/metallic swatches (the texchroma pilot the vision cites) are permanently deferred.
- **Major** `:402`, `:406` vs `:260-262` — a pending row can only be skipped; a missing item wraps forever and "session complete" is unreachable. Fix: Flag on a pending row before any trigger; wrap ends into review.
- **Major** `:234` vs `:334` — the deferred-row review is reachable only inside a session, and a session cannot start on zero pending rows; day two with only deferred rows is stranded.
- **Major** `:165`, `:184` — Swatch Code match rule undefined (case, whitespace); a re-export from Numbers doubles the queue.
- Minor: N_CONSEC_DRIFT has no baseline over different colours (`:278`); resume position after skips (`:355`); duplicate-code hard stop vs blank-code list-and-exclude (`:155-156`); rows absent from a re-import file never-delete unstated (`:178`); malformed/ragged rows not a branch and encoding/delimiter vocabulary surfaced by default (`:144-147`); Swatch Name not marked optional (`:186`); samples-per-row has no default (`:109`); UJ5 lacks a decision criterion, a research default, and a closer (`:505-552`); UJ3 step 5 bakes in the instrument button (`:241`). Nits: UJ2 step 5 omits the duplicate-name branch (`:150`); "keyboard advance" misnamed (`:243`).
- Missing for the requirements pass: metrics that instrument the thesis (UI overhead per row above the scan cycle, deferred and rework rate, zero mis-attribution on a Demo Device stress run, items/hour in the session summary).

#### peer-staff-software-engineer-reviewer (Claude)

Verdict: not ready — needs rework.

- **Blocker S1** `:267`, `:269` vs device PRD `:398`, `:436` — calibration drift and no-reading-within-timeout are listed as per-scan failures here; the locked device PRD routes scan-delta drift under §5's halt taxonomy and makes a device that returns no reading within the liveness timeout a halt. As written the operator scans into a dead connection.
- **Blocker S2** `:252`, `:89` vs AGENTS.md §8 `:99-100`, SDK audit `:30` — "averaged into one reading and durably written as the row's canonical value". Canonical is the raw payload; an average of N per-mode raw strings is derived and cannot round-trip. Lands inside ADR-0003.
- **Major S3** `:271-272` — flag-and-continue vs retry-in-place unresolved (same as the PM Blocker).
- **Major S4** `:278` — N_CONSEC_DRIFT infeasible over heterogeneous rows; "hard failure" vs "flagged" undefined.
- **Major S5** `:262`, `:355-357`, `:360`, `:133`, `:343` — "session" is undefined as an entity; elapsed time, device binding across relaunch, in-flight status are not derivable from row states.
- **Major S6** `:274` — Flag row: Captured → Deferred missing; canonical semantics of a flagged captured row undefined; invocation point undefined.
- **Major S7** `:466`, `:412` — ad-hoc capture and out-of-session re-scan run with "no session" but the device PRD's gate (`:411`) and every halt (`:434`) are session-scoped.
- **Major S8** `:165`, `:156`, `:400`, `:459` — Swatch Code normalisation unspecified.
- **Major S9** `:247` — PERCEPTUAL_FUSION_WINDOW attached to the sample confirm, which arrives after the instrument measures; the source binds 100 ms to acknowledging the press.
- Minor S10–S18: illuminant/observer recorded with the reading re-couples them with F2 (`:244`); skip-wrap livelock (`:402`); rollout phasing in a journey conflicts with the legend (`:275` vs `:577`); relaunch resume re-runs the full gate against the runs-to-completion guarantee (`:360` vs device `:441`, `:382`); import atomicity and absent rows unstated; queue order vs view sort conflated (`:434`); undefined terms (out of range, SAMPLE_TOLERANCE statistic, hard failure, partial-set on jump/review); no v1 scale target; two-indicator rule pre-decides a Reading-B property (`:240` vs `:535`). Nits: "collection file" vs F1; find narrows to pending but bullets handle captured/deferred; "not re-imported" vs "updated"; header-signature order.
- 20 clarifying questions for the author (recorded in the round-1 fix file).

#### peer-test-reviewer (Claude, retargeted)

Verdict: trustworthy after fixing Blockers.

- **Blocker T1** `:271-274`, `:303-304` — post-failure row state: two text-conformant builds (auto-defer-and-advance vs hold-and-retry); a test on the deferred tally passes both and asserts nothing about where the next reading lands.
- **Major T2** `:267` vs device `:398` — calibration drift: defer here, halt there.
- **Major T3** `:269` vs device `:436` — SCAN_TIMEOUT vs the liveness halt share one injection (silence); no injection for a measurement that hangs on a responsive device.
- **Major T4** `:245`, `:269` — "reading out of range" is not an SDK error and has no injection or criterion.
- **Major T5** `:250` — SAMPLE_TOLERANCE statistic and colorimetric basis undefined; N=1 unstated.
- **Major T6** `:275`, `:278` — the guard's "first ~10 real sessions" enable condition is untestable and UJ3.9 walks the pause; N_CONSEC_DRIFT has no fixture.
- **Major T7** `:247` — PERCEPTUAL_FUSION_WINDOW unpassable as stated; split into trigger-ack and result-cue windows on the injectable clock.
- **Major T8** `:242`, `:340`, `:368` — "rejected, not queued", "trigger inert", "scan-accept suppressed" assert no measurement command was issued; the simulated layer exposes no measurement-command log.
- **Major T9** `:355`, `:370` — "first queue row with no durably written reading" is ambiguous for a deferred row with zero samples and teleports the operator after a jump.
- **Major T10** `:156`, `:165`, `:184`, `:459`, `:108` — code and collection-name equality undefined.
- **Major T11** `:274` vs `:90`, `:260` — Flag row is post-reading only; a missing swatch makes Complete unreachable.
- **Major T12** `:341`, `:438`, `:401`, `:328`, `:473` — partial-set policy differs per interruption.
- Minor T13–T19: file-changed detection moment; UJ3.9 conflates disconnect with crash; session binding must be persisted and a session-state fixture is missing; find/sort semantics; failed-Retry counters and mid-scan Re-take; re-scan/ad-hoc while a session is paused; no sleep seam. Nit T20: simulated readings carry the live mode set.
- Seams this PRD must require of the simulated layer: measurement-command observability; per-measurement hang injection; declared collection/session state fixture; Demo latency and cue timestamps on the injectable clock.

#### peer-architecture-reviewer (Claude)

Verdict: sound — proceed to the requirements pass — with four boundary contracts pinned.

- **Major A1** `:355`, `:370` vs UJ3.7 `:401`, UJ3.10 `:438`; device PRD `:421` — the inherited "current item" definition is wrong after F5; halt-resume after a jump returns to row 1. Fix: a persisted session cursor (row identity), re-evaluated on resume; device PRD §5 needs an owner-approved note.
- **Major A2** `:252` — averaged reading declared canonical (same as S2).
- **Major A3** `:466`, `:412` — session-less captures vs the session-scoped halt contract (same as S7). Fix: a one-row session.
- **Major A4** `:351`, `:74-77` vs acq v1 `:93` — the durability promise (power loss, drive detachment) is the opposite of the only write-posture recommendation on file; cheap at one write per ~3 s. Pin one requirement row; ADR-0003 chooses the mechanism.
- Minor A5–A10: queue order vs view sort under Reading B; session entity undefined and abandonment (an interrupted session is immortal and device-bound); skip-wrap has no terminal transition; partial-set policy inconsistent; drift guard not computable; history record kinds (retained samples, failed attempts with cause, leave-deferred notes) not handed to Data Foundation. Nits: "collection file" vs F1; "row r of R" unstable under reorder and insert.

#### peer-product-manager-reviewer (agy)

- **Blocker** "Immediate Undo missing; UJ3.2 mentioned in the outline but missing from the detailed text" — **rejected on verification**, see dispositions.
- **Major** UJ4.1 mid-session insert breaks the heads-down contract; cut for v1 — owner call, recorded as Minor.
- **Major** UJ3.10 mid-session drag reordering is ergonomically flawed — re-litigates F5 ("before or during a session"); the ergonomics are already an OQ.
- Minor: prefers Reading A for UJ5 (the fork is deliberately open; recorded as input); Skip invocation method undefined (key bindings are deliberately not in the journeys).
- Line numbers cited (`:101`, `:63`, `:139`, `:34`) do not correspond to the document.

#### peer-staff-software-engineer-reviewer (agy)

- **Blocker** N_CONSEC_DRIFT applied to arbitrary sequential samples; a queue of ten swatches of decreasing lightness pauses a valid session.
- **Major** hardware-trigger assumption conflicts with the SDK (host-initiated `measure()`, no button event).
- **Major** crash resume after a skip jumps the queue backwards to the skipped row.
- Minor: "session complete" unreachable with intentionally skipped rows. Nit: per-row conflict resolution on re-import implies a blocking reconciliation UI not in the steps.

#### peer-test-reviewer (agy)

Non-conforming — findings reference UJ5.2, UJ5.4, "15 seconds" in UJ3.3, a "manifest" in UJ4.3, and a haptic pulse in UJ2.1, none of which exist in the document. Not counted as a lens having run.

#### peer-architecture-reviewer (agy)

- **Blocker** stateless "current item" breaks under jump and insert; a crash after jumping to row 50 resumes at row 1 (`:355`). Fix: a durable `current_row_id` cursor.
- **Major** mid-session reordering requires a queue list on the capture surface; under Reading A that duplicates the collection view inside the modal (`:435`). Restrict to pre-session, or state it is feasible only under Reading B.
- Minor: mid-queue insertion "immediately after the current row" forces a mutable sequence index; append to the end and rely on jump (`:477`).

### Verify-the-reviewer dispositions

Every Blocker and Major, consolidated by theme where several lenses raised the same defect. `verify` records what the document and upstream sources actually say at the cited lines.

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R1-F1 | Per-scan failure: hold the row vs defer-and-advance; both stated in UJ3.1 step 3 | PM (Blocker), Test T1 (Blocker), Staff S3 (Major) | `:271` "the queue advances to the next pending row"; `:272` "Retry re-takes the failed sample in place (the item stays current)". Confirmed contradiction. | **accept — Blocker.** Owner picks hold vs advance; the journey and flowchart are rewritten to one rule. |
| R1-F2 | Failure taxonomy contradicts the locked device PRD: calibration drift and timeout listed per-scan | Staff S1 (Blocker), Test T2 + T3 (Major) | `:267` lists "calibration drift" as per-scan; device PRD `:398` routes scan-delta under §5's halt taxonomy. `:269` defers on SCAN_TIMEOUT; device PRD `:436` halts a device that returns no reading within the liveness timeout. Confirmed. | **accept — Blocker.** Drift and no-reading are device halts. Owner decides whether a shorter per-scan timeout survives (strictly less than liveness, hands off to the liveness probe, needs a hang injection). |
| R1-F3 | Averaged reading declared the canonical value | Staff S2 (Blocker), Arch A2 (Major) | `:252` "averaged into one reading and durably written as the row's canonical value"; AGENTS.md `:99-100` canonical = raw payload, "not a derived value"; SDK audit `:30` persist the raw string. Confirmed. | **accept — Blocker.** Canonical = the set of N raw per-mode sample payloads; the average is derived with a derivation version. Inherited obligation for Data Foundation. |
| R1-F4 | "Current item = first row with no durably written reading" breaks after jump/reorder/skip | Arch A1 (Major), agy-Arch (Blocker), Test T9 (Major), agy-Staff (Major), PM (Minor) | `:355`, `:370` restate the device PRD `:421` definition; `:401` jump leaves the departed row pending; F5 allows reordering. A crash after a jump resumes at the first pending row. Confirmed; also ambiguous for a deferred row with zero retained samples. | **accept — Major (Blocker on the agy route).** Persisted session cursor (row identity), re-evaluated on resume; fall back to first pending. Note the device PRD §5 definition applies to the un-jumped case; record the amendment. |
| R1-F5 | Flag row acts on an already-captured row; no Captured → Deferred edge; canonical semantics undefined | PM (Major), Staff S6 (Major), Test T11 (Major, part) | `:274` "even though the reading succeeded → Flag row defers the row"; `:83-93` no Captured → Deferred; `:416` a good reading is never demoted. Confirmed. | **accept — Major.** Owner picks: (a) Captured → Deferred with the reading demoted to history, or (b) a "suspect" mark orthogonal to state, listed in the review. State the invocation point (last captured row on the recents strip). |
| R1-F6 | Skip-wrap livelock: a missing item can only be skipped; Complete unreachable | PM (Major), Test T11 (Major), Arch A7 (Minor), Staff S11 (Minor), agy-Staff (Minor) | `:402`, `:406` wrap revisits pending rows indefinitely; `:260-262` complete requires no pending rows. Confirmed. | **accept — Major.** Flag on a pending row without a reading (cause "missing"); a wrap that reaches only rows skipped without an attempt this pass enters review. |
| R1-F7 | Review unreachable with zero pending rows | PM (Major) | `:234` nothing-to-capture offers import, add, re-scan — not the review; `:334` says unresolved rows appear in the next session's review. Confirmed. | **accept — Major.** With zero pending and ≥1 deferred, "Start capture session" runs pre-flight and opens into UJ3.3. |
| R1-F8 | No accept-average path for genuinely non-uniform surfaces | PM (Major) | `:251` offers defer or re-take; `:332` offers leave deferred. Confirmed gap; the vision cites the textile pilot. | **accept — Major.** Owner picks: "Accept average" at the caution and in the review with the spread recorded, and/or SAMPLE_TOLERANCE per collection. State N=1. |
| R1-F9 | Swatch Code (and collection name) equality rule undefined | PM (Major), Staff S8 (Major), Test T10 (Major) | `:165`, `:184`, `:156`, `:400`, `:459` compare codes with no rule; acq v2 `:216` names the trap. Confirmed. | **accept — Major.** One normalisation rule stated once in UJ2.2 and cited everywhere. Owner picks case-sensitive (research default, Staff, Test) vs case-insensitive (PM). |
| R1-F10 | Session-less captures vs the session-scoped gate and halt contract | Arch A3 (Major), Staff S7 (Major), Test T18 (Minor) | `:466` "no session to end"; `:412` "with no session open"; device PRD `:411`, `:434` are session-scoped. Confirmed. | **accept — Major.** Ad-hoc capture and out-of-session re-scan are one-row sessions. |
| R1-F11 | The session is never defined as an entity; device binding across relaunch has no home | Staff S5 (Major), Arch A6 (Minor), Test T15 (Minor) | `:262` elapsed time; `:360` binds the same device across relaunch; `:355-357` nothing stored outside the store; `:133` session in flight. Not derivable from row states. Confirmed. | **accept — Major.** Name the session entity (collection, device identity, started/ended, status, cursor); one active per app, one interrupted per collection; owner decides whether an interrupted session's binding survives relaunch. |
| R1-F12 | Durability promise vs the only write-posture recommendation on file | Arch A4 (Major) | `:351` promises recovery from power loss; acq v1 `:93` recommends synchronous=NORMAL, app-crash durability only. Confirmed tension; a requirement row, not a journey defect. | **accept — Major.** Pin one row: a reading whose row-success confirmation reached the operator survives power loss and drive detachment. ADR-0003 chooses the mechanism. |
| R1-F13 | PERCEPTUAL_FUSION_WINDOW attached to the wrong event | Staff S9 (Major), Test T7 (Major) | `:247` "within 100 ms of the trigger" for the sample confirm; acq v1 `:58` binds 100 ms to the button-press acknowledgment. Confirmed. | **accept — Major.** Split into TRIGGER_ACK_WINDOW and RESULT_CUE_LATENCY; both on the injectable clock. |
| R1-F14 | N_CONSEC_DRIFT has no baseline over heterogeneous rows | agy-Staff (Blocker), Staff S4 (Major), Test T6b (Major), Arch A9 (Minor), PM (Minor) | `:278` "drift the same direction from the session's baseline"; acq v2 `:149-151` 10:x presumes one control. Confirmed. | **accept — Major.** Drop from v1 and rely on the SDK scan-delta halt plus within-item spread, or redefine on a periodic tile re-read. Owner picks. |
| R1-F15 | "Reading out of range" is not an SDK error and has no injection or criterion | Test T4 (Major) | `:245`, `:269`; SDK audit `:85` lists light, battery, temperature, scan-delta only. Confirmed. | **accept — Major.** Delete, or define it with a settable-result fixture. |
| R1-F16 | SAMPLE_TOLERANCE statistic and colorimetric basis undefined; N=1 unstated | Test T5 (Major), Staff S16 (Minor) | `:250` "agree within … across the set". Confirmed. | **accept — Major.** Name the statistic and basis; N=1 skips the check. |
| R1-F17 | Guard enable condition ("first ~10 real sessions") is untestable and is rollout phasing inside a journey | Test T6a (Major), Staff S12 (Minor) | `:275`; `:425` UJ3.9 walks the pause; legend `:577`. Confirmed. | **accept — Major.** Guard mode is an explicit setting (enabled / log-only), default an OQ; move the phasing to the OQ closer column. |
| R1-F18 | Measurement-command observability missing from the simulated layer | Test T8 (Major) | `:242`, `:340`, `:368` assert no command issued; device PRD `:463` lists injections and cue observability, no command log. Confirmed. | **accept — Major.** This PRD requires the simulated layer to expose a measurement-command log (start, outcome, initiator) and a per-measurement hang injection. |
| R1-F19 | Partial-set policy differs per interruption | Test T12 (Major), Arch A8 (Minor), Staff S16 (Minor) | `:341` pause discards; `:438` reorder retains; `:401` jump silent; `:473` add-item "held"; `:328` review silent. Confirmed. | **accept — Major.** One rule: any action that scans a different item first discards the held set; only reorder and queue-list viewing hold it. |
| R1-F20 | Instrument button assumed as the trigger; SDK shows host-initiated measure() and no button event | agy-Staff (Major), PM (Minor) | `:241` already flags it unverified and requires a fallback; SDK audit `:40` `measure(completion:)`. Confirmed as wording, not as a contradiction. | **accept as Minor (corrected from Major).** Write UJ3 step 5 trigger-agnostic so the keyboard/on-screen path is designed as primary until the spike says otherwise. |
| R1-F21 | Mid-session reordering under Reading A duplicates the collection view inside the modal | agy-Arch (Major), Arch A5 (Minor) | `:435` opens a queue list from the capture surface; F5 says "before or during a session". Real seam interaction, not a re-litigation. | **accept as Minor (corrected from Major).** Input to UJ5: note that mid-session reorder is cheap under Reading B and costly under Reading A; queue order is distinct from any view sort. |
| R1-F22 | Immediate undo journey is missing | agy-PM (Blocker) | UJ3.2 exists at `:282-291` with "Re-take sample" (discards sample n) and "Restart item". The reviewer's line numbers do not match the document. | **reject.** False positive. |
| R1-F23 | UJ3.10 mid-session drag reordering should be cut | agy-PM (Major) | Fence F5 (2026-09-06) decides reordering "before or during a session"; one-handed drag ergonomics is already a named OQ. | **reject — fenced (F5).** The ergonomics OQ stands. |
| R1-F24 | UJ4.1 mid-session insert should be cut for v1 | agy-PM (Major) | Vision U2 is ad-hoc capture; mid-session insert is this PRD's design with the "Add item" precedent (acq v2 §11 Q11.4). A product call, not a defect. | **accept as Minor — owner call.** Keep, cut, or keep with the insert routed through the same one-key add. |
| R1-F25 | agy test review | agy-Test | Cites UJ5.2, UJ5.4, a 15 s timeout, a manifest, none in the document. | **reject — non-conforming; lens not run on that route.** Claude test lens stands. |

### Consensus summary for owner adjudication

Blockers: R1-F1, R1-F2, R1-F3. Majors: R1-F4 through R1-F19. Corrected to Minor: R1-F20, R1-F21, R1-F24. Rejected: R1-F22, R1-F23, R1-F25.

Minor findings (PM, Staff S10–S18, Test T13–T20, Arch A5–A12, agy-Staff nit) are carried into the round-1 fix file for the editing dispatch; none blocks the round.

### Verification note — round-1 fix pass and delta verification

**Fix pass:** commit `f09c90e`, applied by `operator-agents:product-manager` from `docs/product/prd-capture-mode-round-1-fixes.md` (48 items, all ticked) under fences F6–F13. A first attempt by a general-purpose editor was stopped and reverted at the owner's instruction that the journeys be written by the PM operator in the Cataloger's words, WHAT not HOW.

**Delta verification:** the same lenses re-dispatched against `f09c90e` with the fence file, this log, and the fix file as sources, asked per finding for RESOLVED / PARTIAL / UNRESOLVED plus any regression.

| lens | route | round-1 findings | new findings |
|---|---|---|---|
| product manager | Claude | all RESOLVED (6 by fence) | 1 Blocker, 3 Major, 2 Minor |
| staff engineer | Claude | all RESOLVED (S1–S18, nits, Q1–Q20) | 5 Major, 1 Minor, 1 Nit |
| test (retargeted) | Claude | all RESOLVED; T12 PARTIAL (the new partial-set rule contradicts two other steps) | 5 Major, 2 Minor |
| architecture | Claude | all RESOLVED (A1–A12) | 2 Major, 2 Minor |
| product manager | agy | all RESOLVED; none new | — |
| staff engineer | agy | rc 8, empty body — not run; Claude staff lens stands | — |
| architecture | agy | all RESOLVED (A1–A12) | 1 "Blocker", corrected to Nit (R1-D15) |

**Regressions introduced by the fix pass, verified against `f09c90e` and consolidated (the round-2 fix file `prd-capture-mode-round-2-fixes.md` maps each to a box):**

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R1-D1 | System sleep defined as both a session interruption (`:105`, `:271`, `:377`, `:412`) and a device-PRD halt (`:383`); the locked device PRD `:441` says halt with authorization excluded | PM (Blocker), Test N3, Arch N1, Staff NEW-1 (Major) | Confirmed at all cited lines. | **accept — Blocker.** Sleep is a halt; interrupted = quit, crash, force-quit, power loss. Recorded under fence F14. |
| R1-D2 | "Resume capture is a new session start" vs "tallies carry forward"; no terminal status for the old session | Staff NEW-2 (Major) | `:384` confirmed. | **accept — Major.** Owner chose: new session, old closed as "interrupted — resumed by the next", tallies inherited. Fence F14; F12 amended. |
| R1-D3 | UJ3 step 2 "fresh session opens at the first pending row" (`:244`) vs UJ3.4 step 4 / flowchart "opens at the row the operator was on" (`:367`, `:418`) | Staff NEW-3 (Major), Arch N3 (Minor, review selection) | Confirmed. | **accept — Major.** Owner chose: the collection remembers the last current row; every new session opens there. Fence F15. |
| R1-D4 | Flag key targets the current pending row after auto-advance; a reflexive Flag after "that one was wrong" defers the next row | PM NEW-3 (Major) | `:288-289` confirmed. | **accept — Major.** Owner chose: until the next trigger press, Flag targets the row that just landed. Fence F16. |
| R1-D5 | Review re-scan: "if the re-scan fails again → the review moves to the next row" contradicts F6 hold-and-retry (`:353`); UJ3.8 `:444` likewise silent on K exhaustion | PM NEW-5 (Minor), Test N4 (Major), Staff NEW-5 (Major) | Confirmed; `:353` unchanged since `e5c43d1`. | **accept — Major.** F6 propagated to UJ3.3 and UJ3.8. |
| R1-D6 | UJ3.8 re-scan with disagreeing samples has no Accept-average path (`:444`) | PM NEW-2 (Major) | Confirmed. | **accept — Major.** F10 applied to UJ3.8. |
| R1-D7 | Partial-set rule (`:107`) says any device halt discards held samples, but a completed set awaiting save is held for "Try saving again" (`:265`, `:398`, device PRD `:433`) | Test N1 (Major) | Confirmed. | **accept — Major.** Clause added: a completed set awaiting its save is not a partial set. |
| R1-D8 | Partial-set rule says held samples survive only reorder, but Skip/Flag/K defer "with good samples kept" (`:286-288`) and the review shows how many were kept | Test N2 (Major) | Confirmed. | **accept — Major.** Samples on a row that becomes deferred are kept as its record. |
| R1-D9 | Guard pause vs explicit Pause: partial-set survival and counter reset unstated (`:291-294`, `:60`) | PM NEW-4 (Major), Test N6, Staff NEW-6 (Minor) | Confirmed. | **accept — Major.** Guard pause keeps the held samples; force-resume resets the guard counters, not the row's K count; two Paused states in the diagram. |
| R1-D10 | One-row session has no exit except capture or halt; a stranded one-row session blocks the next bulk session (`:440`, `:481`, `:495`) | Test N5 (Major), PM NEW-6 (Minor) | Confirmed. | **accept — Major.** Ends on capture, deferral, abandonment, or operator end. |
| R1-D11 | Session attach rules unstated: ad-hoc into another collection while one is paused; attaching to an interrupted (unbound) session without a gate (`:105`, `:440`, `:481`) | Arch N2 (Major), Staff NEW-4 (Major) | Confirmed. | **accept — Major.** Cross-collection blocked while another session is active; attaching to interrupted = resume first (F14). |
| R1-D12 | UJ1 step 6 requires a pending row (`:119`) while UJ3 step 2 allows zero pending with deferred rows (`:246`) | Arch N4 (Minor) | Confirmed. | accept — Minor. |
| R1-D13 | "Skipped without an attempt in this pass": "pass" and jumped-away rows undefined (`:272`, `:430`) | Test N7 (Minor) | Confirmed. | accept — Minor. |
| R1-D14 | Row-state diagram omits Skip-after-failure as a Pending → Deferred cause (`:92`) | Staff NEW-7 (Nit) | Confirmed. | accept — Nit. |
| R1-D15 | "Cursor stack contradiction": after an inserted item (UJ4.1 step 3/5) or a mid-session re-scan (UJ3.8 step 5) the journey promises to return to the held row, which "linear advance from the inserted row" would skip | agy-Arch (Blocker) | The journeys promise a user-visible return to the held row; they do not mandate a single sequential cursor — that is the reviewer's mechanism, not the document's. The promise is satisfiable (the app remembers the held row). One real gap: after the held row is then captured, the journey does not say the queue continues with the next pending row after the inserted item. | **corrected to Nit — accept as a one-sentence clarification** (round-2 FX2-16). Not a Blocker: a HOW objection to a WHAT promise. |

Owner adjudication of R1-D2, R1-D3, R1-D4 recorded as fences F14, F15, F16 (2026-09-06). R1-D1 is corrected by the locked device PRD, not by a new owner call.

**Round-1 status:** every round-1 finding is closed. The round is not aligned until the round-2 fix pass lands and delta-verifies; that appends below as "Round 2".

### Round 2 — fix pass and delta verification

**Fix pass:** commit `7a465d2`, applied by `operator-agents:product-manager` from `docs/product/prd-capture-mode-round-2-fixes.md` (16 items, all ticked) under fences F14–F16 and the F12 amendment.

**Delta verification** against `7a465d2`, same lenses, same sources plus the round-2 fix file; each asked per finding for RESOLVED / PARTIAL / UNRESOLVED, any regression, and a YES/NO on readiness for the requirements pass.

| lens | route | round-1 delta findings | new findings | ready? |
|---|---|---|---|---|
| product manager | Claude | all RESOLVED (R1-D1, D4, D5, D6, D9, D10) | 4 Minor, 2 Nit | YES |
| staff engineer | Claude | all RESOLVED (R1-D1, D2, D3, D5, D9, D11, D14) | 3 Minor, 2 Nit | YES |
| test (retargeted) | Claude | all RESOLVED; N7 (pass definition) PARTIAL | 1 Major, 5 Minor, 1 Nit | YES |
| architecture | Claude | all RESOLVED (R1-D1, D3, D11, D12, D15) | 1 Major, 3 Minor, 2 Nit | YES |
| product manager | agy | all RESOLVED | 1 Major | YES |
| architecture | agy | all RESOLVED | 1 "Blocker" (same subject as the agy PM Major) | NO |

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R2-D1 | Attach rule (c): a re-scan or ad-hoc add against an interrupted bulk session forces a full resume and leaves the bulk session active; a one-off scan lands the Cataloger in the live queue (`:454`, `:496`) | agy-PM (Major), agy-Arch (Blocker), Test (Minor, `:395` vs `:454`) | Confirmed at `:454` and `:496`. Not a strict contradiction (the "one-row session of its own" clause is scoped to "no session open"), but a real product defect. The agy-Arch line numbers do not match the document; the substance does. | **accept — Major (corrected from Blocker).** Owner chose: the one-row session runs on its own; the interrupted session is untouched. Fence F17. |
| R2-D2 | "A pass is one trip from the remembered row, around, and back to it" — the remembered row moves on every advance, so the pass is zero-length and the wrap-exhausted check can never fire (`:282`, `:441`, `:114`) | Arch (Major), Staff (Minor), Test N7 (PARTIAL) | Confirmed. | **accept — Major.** Redefine without the remembered row: a pass begins at the row current when the last wrap-check ended (or the session started) and ends when the queue returns to that position; jumps do not restart it. |
| R2-D3 | UJ4.1 step 5 "continues with the next pending row after the inserted item" hard-codes the INSERT_POSITION candidate; under "append and jump" it skips every row between the held row and the end (`:524`) | Test (Major), Arch (Minor) | Confirmed. | **accept — Major.** "Continues in queue order from the held row". |
| R2-D4 | UJ3 step 2 has no branch for starting a bulk session on collection B while collection A's session is active or paused, though one-row sessions have it (`:253-256` vs `:452`) | Staff (Minor), Arch (Minor) | Confirmed. | accept — Minor. |
| R2-D5 | Flag pressed after a "moved on" cue and before any trigger press would defer the next row as missing (`:298-299`) | PM (Minor) | Confirmed by reading F16's boundary. | accept — Minor: after a moved-on cue Flag is inert until the next trigger press. |
| R2-D6 | Partial-set list omits the mid-session re-scan entered via find; UJ3.8 step 5 lacks "at sample 0 of N" (`:116`, `:442`, `:461`) | PM, Staff, Test (Minor) | Confirmed. | accept — Minor. |
| R2-D7 | An interrupted session with nothing pending or deferred needs the full gate to reach "complete" (`:395`) | PM (Minor), Test (Minor) | Confirmed. | accept — Minor: closed as complete from the collection on relaunch, no gate. |
| R2-D8 | "Quit" listed unconditionally as an interruption; the device PRD routes quit-from-a-halt through End session; no Halted → Interrupted edge (`:112`, `:387`, `:72`) | Arch (Minor) | Confirmed against device PRD `:441`. | accept — Minor. |
| R2-D9 | Trigger press during the guard's pause unstated (`:301-304`) | Test (Minor) | Confirmed. | accept — Minor: not accepted, surfaces the pause. |
| R2-D10 | Review-selected remembered row has no observable effect because resume falls back to the next pending row (`:362`, `:389`) | Test (Minor), Staff Q3 | Confirmed. | accept — Minor: a resume or next session whose remembered row was review-selected opens the review at that row. |
| R2-D11 | Nits: `:377` cites F12 for the remembered-row rule (F15); `:406`, `:470` "the row the session remembers"; "empty-queue state" vs "nothing-to-capture state"; lifecycle edges missing (Item → Paused, Gate → Complete, Halted → Interrupted, Flag self-edge); whether an operator Flag counts toward N_CONSEC_FLAGGED; a one-row session has no Flag-after-landing window; each ad-hoc item re-runs the gate; `#open-questions` legend link | PM, Staff, Test, Arch (Nit) | Confirmed. | accept — Nit; the legend link waits for the requirements pass. |

**Round-2 status:** no fence re-opened; every round-1 and round-1-delta finding closed. Round 3 is a wording pass (fix file `prd-capture-mode-round-3-fixes.md`, one owner decision F17) followed by a closing delta check.

### Round 3 — wording pass and closing check

**Fix pass:** commit `6e0c03d`, applied by `operator-agents:product-manager` from `docs/product/prd-capture-mode-round-3-fixes.md` (12 items, all ticked) under fences F1–F17. From this round on, in-line dispatches run on Opus at the owner's instruction.

**Closing check** against `6e0c03d`, same lenses (Claude on Opus; agy PM and architecture). Every R2-D item is RESOLVED or RESOLVED-BY-FENCE on both routes (test lens: R2-D2 PARTIAL on anchor wording only). Both agy reviews returned rc 8 (short body) but carry conforming per-item verdicts and a YES; the Claude lenses stand as primary.

| lens | route | R2-D items | new findings | ready? |
|---|---|---|---|---|
| product manager | Claude/Opus | all RESOLVED | 1 Major, 1 Minor contradiction | NO (one sentence) |
| staff engineer | Claude/Opus | all RESOLVED | 1 Major, 1 Minor contradiction | YES |
| test (retargeted) | Claude/Opus | all RESOLVED (R2-D2 PARTIAL, wording) | 1 Major, 2 Minor | YES |
| architecture | Claude/Opus | all RESOLVED | 1 Major, 1 Minor contradiction | NO (one sentence) |
| product manager | agy | all RESOLVED | none | YES |
| architecture | agy | all RESOLVED | none | YES |

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R3-D1 | Quitting from the operator's own Pause has no status (not interrupted, not ended); with the new cross-collection block, collection B is unreachable "until that session is ended or complete" after a relaunch (`:116`, `:261`, lifecycle `:63-72`) | PM (Major) | Confirmed: "quit means quitting while capturing"; Paused has only Resume and End session out-edges. | **accept — Major.** Quit or crash from the operator's Pause interrupts the session exactly as from Capturing; `Paused --> Interrupted` edge. |
| R3-D2 | The review-selected remembered row never reverts: after the last deferred row is re-captured and the app quits, the next session must both open the review at that row and be the nothing-to-capture state (`:118`, `:257`, `:395`) | Arch (Major) | Confirmed. | **accept — Major.** The review exception holds only while that row is still deferred; once captured or the review is left, the ordinary rule applies. Invalidation added to the Data Foundation hand-off. |
| R3-D3 | "At most one unresumed interrupted session per collection" vs a one-row session beside an interrupted bulk session being itself interrupted until relaunch (`:116`, `:400`); and an interrupted one-row session has no status that fits | Staff (Major), Arch (Minor), Test (risk) | Confirmed. | **accept — Major.** The invariant counts interrupted bulk sessions only; an interrupted one-row session is closed as ended on relaunch. |
| R3-D4 | UJ3.9 step 5 quits the app while still halted, so the device PRD's End-session warning fires and UJ3.5 is never walked (`:479` vs `:393`, device PRD `:441`) | Test (Major) | Confirmed. | **accept — Major.** Resume scanning before quitting; add the force-quit-during-halt variant. |
| R3-D5 | Nothing-to-capture state offers re-scan unconditionally in UJ3 step 2 but only with captured rows in UJ1 step 6 (`:133` vs `:258`) | PM, Staff (Minor contradiction) | Confirmed. | accept — Minor. |
| R3-D6 | N_CONSEC_FLAGGED trigger clause "rows are deferred" contradicts the exclusion of operator deferrals two lines later (`:306` vs `:308`) | Test (Minor contradiction) | Confirmed. | accept — Minor; owner confirmed the scope as fence F18. |
| R3-D7 | Pass anchor "current when the queue last wrapped" reads two ways (`:287`, `:450`) | Test (Minor) | Confirmed. | accept — Minor: "current when the previous wrap-check ended (or when the session started)". |

**Round-3 status:** no fence re-opened; residue is seven wording items (`prd-capture-mode-round-4-fixes.md`) and one owner confirmation (F18). Round 4 is the final wording pass, verified by the two lenses that said NO.
