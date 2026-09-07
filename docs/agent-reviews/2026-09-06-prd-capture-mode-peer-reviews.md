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

### Round 4 — final wording pass and final check

**Fix pass:** commit `abc429c`, applied by `operator-agents:product-manager` (Opus) from `docs/product/prd-capture-mode-round-4-fixes.md` (8 items, all ticked) under fences F1–F18; the jargon sweep returned zero hits across the whole document, hand-off lists included.

**Final check** against `abc429c` by the two lenses that returned NO in round 3 (Claude on Opus).

| lens | R3-D items | new findings | ready? |
|---|---|---|---|
| product manager | R3-D1, R3-D5 RESOLVED | 1 Minor (flowchart label) | YES |
| architecture | R3-D2, R3-D3 RESOLVED | 2 Minor (flowchart label; review-mark rule stated three ways) | YES |

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R4-D1 | Session-core flowchart node K still reads "deferred rows reach N_CONSEC_FLAGGED" against fence F18 (`:342`) | PM, Arch (Minor) | Confirmed; FX4-6 scoped the prose, constants, and assumptions, not the diagram. | accept — fixed by the orchestrator in place: "instrument-caused deferrals reach N_CONSEC_FLAGGED". |
| R4-D2 | Review-mark invalidation stated three ways: three conditions at `:124`, "still deferred" at `:263`, `:402`, `:652` — after "Leave deferred" the weaker rule re-opens the review, and `:652` is the line Data Foundation inherits | Arch (Minor) | Confirmed. | accept — fixed by the orchestrator in place: all four sites carry the three conditions (captured, deliberately left deferred, or the operator leaves the review). |

Both edits re-parsed (six mermaid blocks OK) and committed with this log entry.

**Gate outcome for the journeys phase: aligned.** Every finding from rounds 1–4 on both routes is RESOLVED, RESOLVED-BY-FENCE, or owner-rejected on the record; every lens that ran the final or closing check says the journeys are ready for the requirements pass. No requirement, error/state, or success-metric rows exist yet, so no row flipped; the requirements pass creates them at pre-alignment and round 5 of this log reviews them. Fences F1–F18 and the `#open-questions` legend link carry forward.

## Round 5 — first full round over the requirement rows

**Subject:** `docs/product/prd-capture-mode.md` and `docs/product/prd-capture-mode-oq-results.md` at commit `4a07a80` (132 R rows, 37 E rows, 8 M rows, 18 OQs, all ⌛️). **Fences:** F1–F25 as `--source`. **Lenses:** Claude on Opus — product manager, staff engineer, test (retargeted), architecture, product-marketing (added: end-user copy now exists in §12); agy cross-model on all five briefs (the first full round's offer, taken at the owner's standing instruction). **tier-rationale:** PRD tier always-on plus architecture (boundaries with decided architecture and the two hand-off PRDs) plus marketing (§12 copy); privacy/security/database/plan not triggered.

**Per-row dispositions:** every lens returned a full 177-row table. Rows with no OBJECT from any non-abstaining lens this round: none flip yet — the flip rule requires the round's fix pass to land and the objections to be verified resolved; all rows stay ⌛️ pending round 6's delta verification.

| lens | route | verdict | Blockers | Majors | rows OBJECT |
|---|---|---|---|---|---|
| product manager | Claude/Opus | builds the right thing; no Blockers | 0 | 7 | 22 R, 7 E, 2 M |
| staff engineer | Claude/Opus | not ready | 3 | 14 | 45 R, 6 E, 4 M |
| test (retargeted) | Claude/Opus | tests don't prove the behaviour | 5 | 13 | 37 R, 12 E, 5 M |
| architecture | Claude/Opus | build after addressing the Blocker | 1 | 5 | 21 R, 2 E, 2 M |
| marketing (copy) | Claude/Opus | lands after fixing Blockers | 1 | 8 | 8 R, 19 E |
| product manager | agy | wrong thing (on one rejected Blocker) | 1 (rejected) | 2 | 4 |
| staff engineer | agy | ready with Blockers | 1 (rejected: misquotes the SDK audit) | 1 (rejected) | 5 |
| test | agy | rc 8, short body; 3 items folded | — | — | 11 |
| architecture | agy | needs redesign | 2 (1 accepted as Major, 1 as Minor) | 2 (1 accepted, 1 rejected) | 10 |
| marketing | agy | outstanding copy | 0 | 2 | 3 |

### Verify-the-reviewer dispositions (Blockers and Majors, consolidated)

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R5-F1 | E10 and R2.7's test teach that "cg-3" and "CG 3" match; F11's rule makes them differ | PMM (Blocker) | `:861`, `:618` confirmed. | **accept — Blocker.** |
| R5-F2 | R1.9 asserts a per-collection file ("moving it moves everything"); R7.9, E26 likewise; contradicts F1, the vision, device PRD `:412`; regresses FX34 | Arch (Blocker), Test m8 | `:601` confirmed. | **accept — Blocker.** Strike the clause; storage granularity → Data Foundation OQ. |
| R5-F3 | An in-flight measurement has no owner when a queue action fires; R4.3 conflates "in flight" with a 0.5 s dead time | Staff B1 (Blocker), Test M1, Test M8, agy-Staff (Blocker, substance only) | `:668` confirmed: only a trigger press is covered. | **accept — Blocker.** Fence F32. |
| R5-F4 | The set's mean has no decided basis (spectral vs colour) and no OQ | Staff B2 (Blocker), Test M10 | `:674`, `:677` confirmed. | **accept — Blocker.** Fence F33. |
| R5-F5 | R1.5 drops F2's D50/2° default; the check is parameterised on a mutable display preference | Staff B3 (Blocker), PM5-6, Arch A4 | `:597`, `:674` confirmed. | **accept — Blocker.** Fence F28. |
| R5-F6 | No read-back observable: version history, kept samples, attempts, session chain unverifiable | Test B1 (Blocker) | `:839` confirmed: R11.6 declares only. | **accept — Blocker.** |
| R5-F7 | No presented-state or surface-contents observable; absence claims unassertable | Test B2 (Blocker) | `:847` claim vs no row; confirmed. | **accept — Blocker.** |
| R5-F8 | Power-loss durability has no injection; force-quit is a false green; durability vs latency tension on removable media | Test B3 (Blocker), Staff M-K, agy-Arch (Blocker) | `:678`, `:843` confirmed. | **accept — Blocker.** Fence F29. |
| R5-F9 | Timing budgets measured on the controllable clock are vacuous | Test B4 (Blocker) | `:840` confirmed. | **accept — Blocker.** |
| R5-F10 | Re-take sample / Restart item have no rows; E36 unbacked | Test B5 (Blocker), PM5-10, Staff M-G | `:659`, `:887` confirmed. | **accept — Blocker.** |
| R5-F11 | "row r of R" as defined never advances | PM5-1, Staff M-C, Test M4 | `:680` confirmed. | accept — Major. |
| R5-F12 | Flag after a Skip or Flag-as-missing defers the next row | PM5-2 | `:700-701` confirmed. | accept — Major. |
| R5-F13 | Partial sets discarded with no disclosure before the act; R8.13 overpromises | PM5-3, Staff m5 | `:739`, `:782` confirmed. | accept — Major. Fence F26 (cue, no confirm). |
| R5-F14 | M3 (and M2, M5) penalise multi-day and crash chains | PM5-4, Arch A10, Staff M-I, Test M7, agy-PM | `:902` confirmed. | accept — Major. Fence F30. |
| R5-F15 | SAMPLE_TOLERANCE prompts on a placeholder during dogfood | PM5-5 | `:675`, `:705` confirmed. | accept — Major. Fence F27. |
| R5-F16 | E24 has no requirement row; zero-state copy broken; no items per hour | PM5-7, Staff m9, Test m4, PMM7, agy-PMM2 | `:875` confirmed. | accept — Major. |
| R5-F17 | Action labels diverge between rows and §12 | PMM8, agy-PMM1 | confirmed. | accept — Major: "Accept the average"; §12 governs. |
| R5-F18 | R1.7 internally contradictory; E2 wrong for a finished collection | PMM2, PM5-8, Staff M-F | `:599`, `:853` confirmed. | accept — Major. |
| R5-F19 | A file missing at commit gets the encoding recovery | PMM3, Staff m2 | `:614`, `:856` confirmed. | accept — Major. |
| R5-F20 | E32/E28 hide a mandated discard | PMM4, PMM5, PM5-9 | confirmed. | accept — Major (with F26). |
| R5-F21 | E19 blames the instrument for textured material; ⟨n⟩ wrong for the flagged counter | PMM6, Staff m3, Test m2, agy-Staff | `:870` confirmed. | accept — Major. |
| R5-F22 | E35 overclaims parity | PMM9, Test n3 | `:886` vs R11.1 confirmed. | accept — Major. |
| R5-F23 | Seven P0 hand-offs land on tables ADR-0003 defers to ADR-0004 | Arch A2 | decisions/README `:22` confirmed. | accept — Major: R10.1/R10.3 release the schema. |
| R5-F24 | Attach-to-paused creates a second session entity and could release the binding | Arch A3, Staff M-H, agy-Arch | `:798`, `:800`, `:645` confirmed. | accept — Major. |
| R5-F25 | Collection/session fixture routed through the device mock | Arch A5 | `:839` confirmed. | accept — Major. |
| R5-F26 | ADR-0004 observables lack reorder, a mode-slip protocol, a tie-break, and a focus/dispatch observable | Arch A6, Staff M-N, Test M11, PM5-14 | `:822` confirmed. | accept — Major. |
| R5-F27 | Guard counters have no reset rule; "failed attempt" unit undefined | PM5-12, Staff M-A, M-B, Test M2 | `:697`, `:702` confirmed. | accept — Major. |
| R5-F28 | Matching rule lacks Unicode normalisation, case-folding basis, whitespace class | Staff M-D, Test M9 | `:618` confirmed. | accept — Major. |
| R5-F29 | Default for changed metadata on captured rows unspecified | Staff M-E | `:631` confirmed. | accept — Major. |
| R5-F30 | Two-sense promise has no fallback if device OQ 20 resolves negative | Staff M-J | `:672` confirmed. | accept — Major. |
| R5-F31 | Legal illuminant/observer set unspecified; spectral entitlement | Staff M-L | `:597` confirmed. | accept — Major. |
| R5-F32 | Which scan mode feeds the agreement check | Staff M-M | `:670`, `:674` confirmed. | accept — Major. |
| R5-F33 | Pass anchor by row identity can leave the pending set | Test M3, PM5-19 | `:720` confirmed. | accept — Major. |
| R5-F34 | Elapsed capture time undefined | Test M5 | `:644` confirmed. | accept — Major. |
| R5-F35 | M1 target double-counts | Staff m1, Test M6 | `:900` confirmed. | accept — Major. |
| R5-F36 | Measurement record lacks the target row | Test M8 | `:838` confirmed. | accept — Major (with R5-F3). |
| R5-F37 | Accessibility announcements not observable | Test M12, agy-Test | `:682` confirmed. | accept — Major. |
| R5-F38 | Scale constants with no verifiable property | Test M13, Staff m8 | `:655` confirmed. | accept — Major. |
| R5-F39 | No bulk "Leave all set aside" | agy-PM (Major) | R8.6 + E21 confirmed. | accept — Major. Fence F31. |
| R5-F40 | A one-row session beside an interrupted bulk session could move the remembered row | agy-Arch (Blocker) | plausible; F17 silent on it. | accept — corrected to Minor. |
| R5-X1..X4 | agy items rejected on verification | agy-PM, agy-Staff, agy-Arch | see the fence file's rejected list. | **reject.** |

Minors and nits from every lens are carried in `prd-capture-mode-round-5-fixes.md` (68 items, item-for-item map at its end). Owner adjudication: eight forks → fences F26–F33.

**Round-5 status:** not aligned; fix pass dispatched; round 6 delta-verifies per row and per finding.

**Round-5 fix pass:** commits `e0336fc` (68 items) and `425038e` (fences F34, F35 plus three orchestrator-decided items), applied by `operator-agents:product-manager` on Opus. Rows after the pass: 143 R, 41 E, 11 M, 23 OQs.

## Round 6 — delta verification of round 5 (2026-09-07)

**Subject:** commit `425038e`. **Lenses:** the same five on Claude/Opus; agy on four (its test lens skipped after two non-conforming rounds). Each lens returned per-finding verdicts on its round-5 items and a complete 195-row disposition table.

| lens | route | round-5 items | new findings | rows OBJECT |
|---|---|---|---|---|
| product manager | Claude/Opus | all RESOLVED / BY-FENCE | 3 Major | 4 |
| staff engineer | Claude/Opus | 38 of 40 RESOLVED; m7 PARTIAL; Q19 not in the log | 2 Blocker, 1 Major, 2 Minor | 4 |
| test (retargeted) | Claude/Opus | 30 of 33 RESOLVED; B2, M13 PARTIAL | 1 Blocker, 3 Major, 1 Minor | 14 |
| architecture | Claude/Opus | A1–A12 all RESOLVED / BY-FENCE | 4 Major, 3 Minor | 15 |
| marketing (copy) | Claude/Opus | all RESOLVED; PMM2, PMM8 PARTIAL | 5 Major, 1 Minor | 12 |
| product manager | agy | all RESOLVED | none | 0 |
| staff / architecture / marketing | agy | rc 8, short bodies, all-ALIGN tables; advisory only | none | 0 |

### Verify-the-reviewer dispositions (Blockers and Majors)

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R6-F1 | R11.12's surface enumeration covers only the capture surface; R8.3, R8.7, R7.15 cite it for other surfaces, so blind re-capture and E24's conditional lines are unassertable | Test T1 (Blocker) | `:856`, `:773`, `:785`, `:760` confirmed. | **accept — Blocker.** |
| R6-F2 | The spectral mean (F33) has no basis when the license lacks the spectral entitlement, a state the locked device PRD lets reach capture | Staff S6-F1 (Blocker) | `:680`, `:599`; device PRD `:367`, `:412` confirmed. | **accept — Blocker.** Owner: capture proceeds on the colour-value mean, reading marked non-spectral. Fence F36. |
| R6-F3 | R11.9's interleaved case asserts the row must not defer while accepted samples intervene; R5.4 says accepted samples do not reset the count, so it does defer | Staff S6-F2 (Blocker) | `:853` vs `:704` confirmed. | **accept — Blocker.** |
| R6-F4 | A deliberately-left row stays deferred, so a collection completed by adjudication reopens the review forever and never shows the finished message | Arch A-N1 (Major), PMM-2 (Major) | `:601`, `:655`, `:775-777`, `:894` confirmed. | **accept — Major.** F31 clarified: a left row is settled; routing and completion treat settled rows as adjudicated. |
| R6-F5 | R11.3 claims the Demo trigger source is in the device PRD's closed exception list; it is not, and neither that nor the latency-row pull is handed forward | Arch A-N3, Staff S6-F3, Test T4 (Major) | `:847` vs device PRD `:460` confirmed. | **accept — Major.** Inherited notes for the device PRD §6. |
| R6-F6 | With the guard enabled, N_CONSEC_HARD (presses) trips before any row can auto-defer | Arch A-N4 (Major) | `:709`, `:853` confirmed. | **accept — Major.** Owner: count consecutive rows. Fence F37. |
| R6-F7 | The seam tie-break disqualifies Reading B on one slip with n = 1; asymmetric | Arch A-N5 (Major) | `:833` confirmed. | **accept — Major.** Owner: symmetric, repeated (PROTOTYPE_RUNS). Fence F38. |
| R6-F8 | After a Skip-before-any-attempt the row is pending, but E20 says it is already set aside | PM6-1 (Major) | `:890`, `:705`, `:726` confirmed. | accept — Major. |
| R6-F9 | Re-take / Restart in the post-landing window either do nothing or name the new row | PM6-2 (Major) | `:689`, `:682` confirmed. | accept — Major. |
| R6-F10 | ⟨dropped⟩ is carried by three states; a jump, review entry, and a completed Add item disclose nothing; R11.12 enumerates no such field | PM6-3, PMM-6 (Major) | `:746`, `:771`, `:856` confirmed. | accept — Major. |
| R6-F11 | Rows quote "Add item" / "Leave all set aside" where §12 writes "Add a swatch" / "Leave them all set aside" | PMM-1 (Major) | `:725`, `:813`, `:777`, `:909` confirmed. | accept — Major. |
| R6-F12 | E18 offers "Set it aside" on a re-scan where R8.10 says abandon keeps the existing value | PMM-3 (Major) | `:888`, `:788` confirmed. | accept — Major. |
| R6-F13 | The placeholder rule "⟨code⟩ always names the swatch the operator will land on" is false for E17 and E20 | PMM-4 (Major) | `:866`, `:887`, `:890` confirmed. | accept — Major. |
| R6-F14 | R11.13 is a test affordance, not a record of a human run; R10.7 and M11 depend on a record | Test T2 (Major) | `:857` confirmed. | accept — Major. |
| R6-F15 | Attempt records carry no session or time, so M4, M5, M9 are not computable from the read-back they cite | Test T3 (Major) | `:791`, `:855` confirmed. | accept — Major. |

Minors and nits (Arch A-N2, A-N6, A-N7; Staff S6-F4, S6-F5, Q19; Test T5 and nits; PMM-5 and nits) are carried in `prd-capture-mode-round-6-fixes.md`. Staff Q19 ("is amending 🤝 Aligned device-PRD rows an agreed process?") is answered here: yes — through inherited notes recorded in this PRD's closing paragraph and OQ 16, applied to the device PRD only by an owner-approved amendment.

### Row flips

Flip rule: a row flips to 🤝 Aligned when no Claude lens OBJECTed to it in round 6 and at least one ALIGNed (the agy tables object to nothing and are advisory). **36 rows stay ⌛️** because at least one round-6 objection stands against them: R1.7, R3.7, R3.9, R3.12, R4.12, R4.18, R4.21, R5.4, R5.9, R6.3, R7.1, R7.15, R8.1, R8.3, R8.6, R8.7, R8.10, R8.13, R8.15, R9.9, R9.10, R10.7, R11.3, R11.9, R11.11, R11.12, R11.13, E2, E17, E18, E20, E24, M4, M5, M9, M11. **Every other row (159 of 195) flips to 🤝 Aligned** with this log entry as the recorded authorization; any aligned row the round-6 fix pass edits returns to ⌛️ for re-review.

**Round-6 status:** partially aligned. **Fix pass:** applied by `operator-agents:product-manager` on Opus (23 items + FX6-0; one new row E42; PROTOTYPE_RUNS named). After the pass: 144 🤝 Aligned, 52 ⌛️ (the 36 held here, 15 returned to ⌛️ because the fixes edited them — R1.5, R4.4, R4.9, R4.14, R4.17, R5.7, R5.10, R6.2, R7.11, R8.5, R11.1, E19, E32, E39, E41 — and E42). Round 7 delta-verifies those 52.

## Round 7 — delta verification of round 6 (2026-09-07)

**Subject:** commit `9ea4c57` (143 R, 42 E, 11 M; 144 🤝, 52 ⌛️). **Lenses:** the same five on Claude/Opus; agy on four (all-ALIGN, no findings, advisory).

| lens | route | round-6 items | new findings | open rows OBJECT | aligned rows OBJECT |
|---|---|---|---|---|---|
| product manager | Claude/Opus | 2 RESOLVED, PM6-2 PARTIAL | 3 Major | 5 | E28 |
| staff engineer | Claude/Opus | all RESOLVED / BY-FENCE | 3 Major | 3 | none |
| test (retargeted) | Claude/Opus | all RESOLVED, T2 PARTIAL | 1 Blocker, 4 Major, 1 Minor | 8 | none |
| architecture | Claude/Opus | A-N1–A-N7 all RESOLVED / BY-FENCE | 3 Major, 4 Minor | 7 | none |
| marketing (copy) | Claude/Opus | all RESOLVED, PMM2 PARTIAL | 1 Blocker, 3 Major | 4 | E27 |
| agy (PM, staff, arch, marketing) | agy | all RESOLVED | none | 0 | none |

### Verify-the-reviewer dispositions (Blockers and Majors)

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R7-F1 | R11.13's interaction record carries no time and no current row, so R10.7's time/steps and M11's per-row count are not derivable; and it is an unbounded durable write on the hot path with no owner | Test F7-B1 (Blocker), Arch A7-2 (Major) | `:857`, `:833`, `:934` confirmed. | **accept — Blocker.** Add time and current row; need not be durable before the ack; bounded to the session and instrumented/dogfood builds; hand to Data Foundation. |
| R7-F2 | F36's capture-proceeds path contradicts the locked device PRD's copy ("None (requires a license change)"); nothing handed forward; no capture-side indicator | PMM7-1 (Blocker), PM7-A (Major) | device PRD `:367`, `:499` confirmed. | **accept — Blocker.** F36 clarification recorded. |
| R7-F3 | R4.21's post-landing window asserts "captured and confirmed" even after a moved-on cue, Skip, or Flag-as-missing | PM7-B, Test F7-M3, Arch A7-1, Staff S7-F3 (Major) | `:689`, `:707` confirmed. | accept — Major. |
| R7-F4 | E28 says the samples were let go at the re-scan offer; R7.1 discards at the re-scan itself | PM7-C (Major) | `:898`, `:746`, `:725` confirmed. | accept — Major; E28 re-opens. |
| R7-F5 | F6 still says "consecutive"; R5.4 now counts across accepted samples | Test F7-M1 (Major) | confirmed. | accept — Major. F6 amendment recorded. |
| R7-F6 | R11.9's interleaved step names no guard setting; vacuous in record-only | Test F7-M2 (Major) | `:853` confirmed. | accept — Major. |
| R7-F7 | R7.1's disclosure promise covers a device halt whose copy is the device PRD's and carries no count | Test F7-M4, Arch A7-3 (Major), Staff Q5 | `:746`, `:862` confirmed. | accept — Major. E42's line on the capture surface, which stays visible under a halt. |
| R7-F8 | E24's conditional review action is unreachable at completion | PMM7-3, Staff S7-F2 (Major), Arch A7-4, PM | `:894`, `:776` confirmed. | accept — Major. |
| R7-F9 | E27 "or just not today" promises impermanence; F31 settles the row | PMM7-4 (Major) | `:897` confirmed. | accept — Major; E27 re-opens. |
| R7-F10 | R1.7 and E2 disagree on the finished variant's actions | PMM7-2 (Major) | `:601`, `:872` confirmed. | accept — Major. |
| R7-F11 | R8.1 route 1, UJ3 step 11, and the diagram still say "queue exhausted with rows set aside" without "unsettled" | Staff S7-F1 (Major) | `:771`, `:274`, `:84` confirmed. | accept — Major. |

Minors (Test F7-m1; Arch A7-5, A7-6, A7-7, the two missing notes; Staff Q4, Q6, Q7; PM's E24/M11 notes; Test's binding-table observable) are in `prd-capture-mode-round-7-fixes.md`. Staff Q7 is answered by the F38 clarification (PROTOTYPE_RUNS fixed at 3).

### Row flips

Flip rule as in round 6. Of the 52 open rows, **16 stay ⌛️** (an objection stands): R1.5, R1.7, R4.12, R4.17, R4.21, R5.4, R6.3, R7.1, R8.1, R10.7, R11.3, R11.9, R11.13, E2, E24, M11. **36 flip to 🤝 Aligned**: R3.7, R3.9, R3.12, R4.4, R4.9, R4.14, R4.18, R5.7, R5.9, R5.10, R6.2, R7.11, R7.15, R8.3, R8.5, R8.6, R8.7, R8.10, R8.13, R8.15, R9.9, R9.10, R11.1, R11.11, R11.12, E17, E18, E19, E20, E32, E39, E41, E42, M4, M5, M9. Two aligned rows re-open on a round-7 objection: **E27** (PMM7-4) and **E28** (PM7-C). Net after this round's flips and before the fix pass: 178 🤝, 18 ⌛️.

**Round-7 status:** partially aligned. **Fix pass:** applied by `operator-agents:product-manager` on Opus (22 items + FX7-0; new rows R4.22 and E43; OQ 24, OQ 25). After the pass: 169 🤝, 29 ⌛️ of 198 — the 16 held, E27 and E28 re-opened, 5 listed rows that stayed ⌛️ because a fix edited them (R3.12, R5.7, R11.12, E17, E42), 4 aligned rows a fix edited (R5.6, R7.5, R8.14, E23), and the two new rows. Round 8 delta-verifies those 29.

## Round 8 — delta verification of round 7 (2026-09-07)

**Subject:** commit `e3991a7` (144 R, 43 E, 11 M; 169 🤝, 29 ⌛️). **Lenses:** the same five on Claude/Opus over the 29 open rows plus any 🤝 row a round-7 edit broke; agy on four (PM, staff, architecture, marketing), advisory.

| lens | route | round-7 items | new findings | open rows OBJECT | aligned rows OBJECT |
|---|---|---|---|---|---|
| product manager | Claude/Opus | all RESOLVED / BY-FENCE, PM6-2 RESOLVED | 1 Major, 1 Minor | 3 | none |
| staff engineer | Claude/Opus | all RESOLVED / BY-FENCE | 3 Major, 4 questions | 4 | E20 |
| test (retargeted) | Claude/Opus | all RESOLVED except R7-F3, R7-F4, R7-F2 PARTIAL (each carried by a new finding) | 3 Major, 1 Minor | 4 | E20 |
| architecture | Claude/Opus | all RESOLVED, A7-2 PARTIAL (carried by A8-3) | 4 Major, 1 missing journey line | 5 | E20 |
| marketing (copy) | Claude/Opus | all RESOLVED / BY-FENCE, PMM2 RESOLVED | 2 Major, 1 Minor | 4 | Surfaces table (not a row) |
| agy (PM, staff, arch, marketing) | agy | all RESOLVED | none | 0 | none |

agy: all four briefs returned rc 8 (short bodies, 1.0–2.1 KB). The PM body carried a full all-ALIGN table; staff, architecture, and marketing returned "Aligned rows: no objection" with an empty table. Advisory, as in rounds 5–7; the Claude lenses stand for each. Saved under the scratchpad reviews.

### Verify-the-reviewer dispositions (Blockers and Majors)

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R8-F1 | R4.21 routes "Re-take sample" and "Restart item" in a non-landing F16 window to E20, whose name and both bodies are written for Flag ("There's nothing here to flag"); R11.12 cannot tell the origins apart; UJ3.2 step 4 has no line for the branch | Staff S8-F1, Test R8-T1, Arch A8-1 (Major) | `:691`, `:893`, `:859`, `:299-301` confirmed. | **accept — Major.** E20 re-opens. New state for the undo actions rather than a third Flag variant. |
| R8-F2 | Three conditions gate one offer: R8.1 route 2 "unsettled", E23 "any set aside", R7.5 "deferred rows exist"; R7.13 "deferred rows waiting"; E23's action label differs from the Labels paragraph; the summary route is not among R8.1's "four ways"; and the unsettled gate strands F31's settled-row re-scan from the list (R3.9, R8.5, R8.15, E33) | PM PM8-1, Staff S8-F2, Arch A8-2, PMM PMM8-1 (Major) | `:774`, `:896`, `:753`, `:761`, `:867`, `:129`, `:657`, `:778`, `:780`, `:906` confirmed; fence F31 clarification confirmed. | **accept — Major.** Owner fork adjudicated: **fence F39** (offered while any set-aside row exists; "unsettled" governs completion and auto-entry only). |
| R8-F3 | R11.9 claims no OQ 3 tuning can invalidate its interleaved step, but at N_CONSEC_HARD = 1 the guard pauses on that row's auto-deferral; R5.9's no-pause promise fails the same way; F37 left no floor | Staff S8-F3 (Major) | `:856`, `:712`, `:948` confirmed. | **accept — Major.** Owner fork adjudicated: **fence F40** (N_CONSEC_HARD ≥ 2). |
| R8-F4 | R4.22 asserts the missing entitlement never blocks a scan, but no row states that the pre-flight authorization check warns rather than blocks on it; device PRD §7 copy is still a dead end | Test R8-T3 (Major) | `:692`, `:682`; device PRD `:409`, `:412`, `:499` confirmed. | accept — Major. Stated in R4.22 under F36 (capture runs); added to the closing hand-off note. |
| R8-F5 | R7.1's opening sentence says samples survive "only a reorder and looking at the queue list", but R6.3/E28 add a cancelled re-scan offer; and a cancelled "Add a swatch" (R9.10, E32) discards while a cancelled re-scan keeps, with no reason | Test R8-T2 (Major) | `:749`, `:728`, `:816`, `:905` confirmed. | **accept — Major.** Owner fork adjudicated: **fence F41** (cancelled add keeps the samples). R9.10 and E32 re-open. |
| R8-F6 | R4.22's test cannot run against the Demo Device: the device PRD exempts licensing for it, its simulated state surface has no settable spectral entitlement, and the "spectral entitlement absent" outcome sits under the simulated authorization service | Arch A8-4 (Major), Staff Q4 | device PRD `:460`, `:463`, `:464`; `:851` confirmed. | accept — Major. Inherited note for device PRD §6 on the R11.3 pattern; OQ 16 already feeds R4.22. |
| R8-F7 | R11.13 says the record reads back "per session and per session chain" and, later, "covers one session at a time"; OQ 24 repeats the bound | Arch A8-3 (Major) | `:860`, `:969`, `:760` confirmed. | accept — Major. Per-session record; a chain's read follows R7.12's links, and the row says so instead of claiming two scopes. |
| R8-F8 | E43 names "colour is worked out under D50/2°" as the consequence of the missing entitlement, which is true with or without it (R4.9, R4.12); the real loss (no reflectance curve kept, so the reading cannot be re-derived under another light) is unsaid | PMM PMM8-2 (Major) | `:916`, `:679`, `:682` confirmed. | accept — Major, copy. |

Minors (PM PM8-2 / Test R8-T4: E23 names the count but not the swatch R7.5 requires; PMM PMM8-3: the Surfaces table's Capture "Shows" column lacks the non-spectral indicator; Arch's missing UJ3.2 step 4 line) are in `prd-capture-mode-round-8-fixes.md`. Arch's risk note on R3.12 (a wall-clock soak that cannot sit in per-PR CI) is recorded here as a note for the engineering plan, not a finding; R3.12 is unchanged.

### Row flips

Flip rule as in round 6 (every non-abstaining Claude lens ALIGN, at least one opined; agy advisory). Of the 29 open rows, **9 stay ⌛️** (an objection stands): R4.21, R4.22, R7.1, R7.5, R8.1, R11.9, R11.13, E23, E43. **20 flip to 🤝 Aligned**: R1.5, R1.7, R3.12, R4.12, R4.17, R5.4, R5.6, R5.7, R6.3, R8.14, R10.7, R11.3, R11.12, E2, E17, E24, E27, E28, E42, M11. **Re-opened** by findings against 🤝 rows: E20 (R8-F1), R9.10 and E32 (R8-F5). Any 🤝 row a round-8 fix edits returns to ⌛️ "edited in round 8, re-review" (expected: R5.9, R7.13, R11.12, R3.9/R8.5/R8.15/E33 only if their wording changes).

**Round-8 status:** partially aligned. **Fix pass:** applied by `operator-agents:product-manager` on Opus (12 items + FX8-0; new row E44 "Nothing to undo"; FX8-12 added mid-pass for the UJ3 flowchart edges the pass surfaced). After the pass: 182 🤝, 17 ⌛️ of 199 — the 9 held rows edited and re-review; E20, R9.10, E32 re-opened; R5.7, R11.12 stayed ⌛️ because a fix edited them; R5.9, R7.13 edited from 🤝; E44 new. Fences F39–F41 recorded. Next: round 9 delta verification over the 17 open rows.

## Round 9 — delta verification of round 8 (2026-09-07)

**Subject:** commit `9aa4b0f` (144 R, 44 E, 11 M; 182 🤝, 17 ⌛️). **Lenses:** the same five on Claude/Opus over the 17 open rows plus any 🤝 row a round-8 edit broke; agy on four (PM, staff, architecture, marketing), advisory.

| lens | route | round-8 items | new findings | open rows OBJECT | aligned rows OBJECT |
|---|---|---|---|---|---|
| product manager | Claude/Opus | all RESOLVED / BY-FENCE | 1 Major, 1 Minor | 1 | none |
| staff engineer | Claude/Opus | all RESOLVED / BY-FENCE (R8-F2 with residue) | 2 Major, 2 Minor, 5 questions | 2 | R1.7, E2, R8.2, E27 |
| test (retargeted) | Claude/Opus | all RESOLVED; R8-F2, R8-F3 PARTIAL (carried by R9-T2, R9-T1) | 2 Major | 4 | none |
| architecture | Claude/Opus | all RESOLVED / BY-FENCE | 1 Minor | 1 | none |
| marketing (copy) | Claude/Opus | all RESOLVED / BY-FENCE | 2 Major | 3 | E27 |
| agy (PM, staff, arch, marketing) | agy | all RESOLVED | 1 Blocker (PM; rejected, R9-X1) | E23 (rejected) | none |

agy: PM, staff, and architecture returned rc 0 with per-row tables (all ALIGN except the PM's E23); marketing rc 8 (short body). Advisory on the same terms as rounds 5–8; the Claude lenses stand for each. The agy PM Blocker — "UJ3.4 step 3 says End session has no cancel button, but E23 offers Keep scanning" — is **rejected** (R9-X1, fence file): UJ3.4 step 3, R4.16, and R7.4 say the *capture surface* carries no cancel or abandon control, so a session cannot be thrown away from the counting surface; E23 is the confirmation the same journey's step 4 says the operator gives, and declining a confirmation is not an abandon control. Its tense point is PM9-1, already accepted (R9-F4).

### Verify-the-reviewer dispositions (Blockers and Majors)

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R9-F1 | Under F39 the review offer stands on the collection surface whenever any set-aside row exists, but E2's finished variant fixes three actions with no review door, R1.7 says "the three actions", and R11.12's surface enumeration never lists the collection surface, so a finished collection with no way into the list passes every observable | Staff R9-F1, Test R9-T2 (Major), Arch risk note | `:604`, `:876`, `:775`, `:860`, `:581` confirmed. | **accept — Major.** E2's finished variant gains the conditional action; R1.7 and R11.12 follow. R1.7 and E2 re-open. |
| R9-F2 | Under F39 the list can be opened with every row settled and no row says what it shows or offers there (settled mark, leave actions on settled rows, "Leave them all" with nothing outstanding, session or browse) | Staff R9-F2 (Major), Staff Q2–Q4 | `:776`, `:779`, `:781`, `:913` confirmed. | **accept — Major.** Owner fork adjudicated: **fence F42** (browse, not a session; leave actions only on unsettled rows; settled rows show their decision). R8.2 re-opens. |
| R9-F3 | F40 floors N_CONSEC_HARD only, but one auto-deferred row counts toward both counters, so N_CONSEC_FLAGGED tuned to 1 still pauses on a single row and falsifies R5.9's promise and R11.9's step | Test R9-T1 (Major) | `:713` ("by either of those routes"), `:857`, `:950` confirmed. | **accept — Major.** Owner asked: **F40 clarification** recorded — no guard counter below 2. |
| R9-F4 | E23 is a prompt the operator can back out of, yet says the samples "were let go"; under F41 and R7.1 they are still there while it is up | PM PM9-1 (Major) | `:897`, `:750` confirmed; E22's past tense correct (Pause discards at once). | accept — Major, copy tense. |
| R9-F5 | E23 and E25 promise "picks up at ⟨code⟩" but a session with nothing pending and an unsettled set-aside row opens into the set-aside list at no row (R3.7, R3.9); R7.5 and R7.13 mandate the wording; the Placeholders rule inherits the gap | PMM PMM9-1 (Major) | `:897`, `:899`, `:656`, `:658`, `:870` confirmed. | accept — Major. Second variant on both states. |
| R9-F6 | E27 "It won't come back in the review; you can scan it any time from your collection" is false under F39 (the list still shows it; R8.5 names the list as a place to re-scan from) | PMM PMM9-2 (Major), Staff R9-F3 (Minor) | `:901`, `:775`, `:779` confirmed. | accept — Major, copy. E27 re-opens. |
| R9-F7 | E43's license wording is shown on a Demo Device session started without spectral data, to a contributor who has no license (R4.22 says the indicator shows exactly as live) | Staff R9-F4 (Minor, contradiction), Staff Q5 | `:917`, `:693`; device PRD `:457` confirmed. | accept as Major for the fix pass (a simulated variant of E43), since it is a row-level contradiction. |

Minors (Arch A9-1: R4.22's "nothing to set" clause vs device PRD `:464`; PM PM9-2: R7.1's Pause exception) are in `prd-capture-mode-round-9-fixes.md`. Recorded here, not fixed: PM's watch that three places say colour is shown under D50/2° while R1.5 offers illuminant/observer as display defaults (OQ 21 and R8.14 own it); Arch's note that E20 and E44 share headline strings (copy lens's call, not raised); PMM's residue that E23's "deal with them now or another day" is thin when every row is settled.

### Row flips

Flip rule as in round 6. Of the 17 open rows, **9 stay ⌛️** (an objection stands): R4.22, R5.9, R7.5, R7.13, R8.1, R11.9, R11.12, E23, E43. **8 flip to 🤝 Aligned**: R4.21, R5.7, R7.1, R9.10, R11.13, E20, E32, E44. **Re-opened** by findings against 🤝 rows: R1.7, E2 (R9-F1), R8.2 (R9-F2), E27 (R9-F6). Any 🤝 row a round-9 fix edits returns to ⌛️ "edited in round 9, re-review" (expected: R7.1, R8.5, R8.15, E25, E39).

**Round-9 status:** partially aligned. **Fix pass:** applied by `operator-agents:product-manager` on Opus (10 items + FX9-0; no new rows). After the pass: 181 🤝, 18 ⌛️ of 199 — 7 of the 8 listed rows flipped (R7.1 stayed ⌛️ because FX9-9 edited it); R1.7, E2, R8.2, E27 re-opened; the 9 held rows edited and re-review; R8.5, R8.15, E25, E39 edited from 🤝. Fence F42 and the F40 clarification recorded; agy R9-X1 rejected. Operator residue for round 10: E2 and E23 word the same offer condition two ways; E27/E39 do not carry the "only on unsettled rows" condition in their action cells as E23/E2 do; E24 (session complete) carries no review offer under F39 (owner call). Next: round 10 delta verification over the 18 open rows.
