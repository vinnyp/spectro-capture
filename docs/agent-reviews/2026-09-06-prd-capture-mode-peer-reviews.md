# Peer-review gate — prd-capture-mode (2026-09-06)

**Mode:** requirements. **Subject:** `docs/product/capture-mode/prd-capture-mode.md` at commit `e5c43d1` (journeys only — no Requirements, Error & State, Success Metrics, or Open Questions tables yet, so every per-row disposition this round is ABSTAIN and findings are journey-level). **Fence file:** `docs/product/capture-mode/prd-capture-mode-fences.md` (F1–F5, passed as `--source`). **Reviewers:** peer-product-manager-reviewer, peer-staff-software-engineer-reviewer, peer-test-reviewer (retargeted), peer-architecture-reviewer. **persona-version:** cache/1.4.0. **Caller:** `operator-agents:writing-prds`, Phase 4 round 1.

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

**Fix pass:** commit `f09c90e`, applied by `operator-agents:product-manager` from `docs/product/capture-mode/prd-capture-mode-round-1-fixes.md` (48 items, all ticked) under fences F6–F13. A first attempt by a general-purpose editor was stopped and reverted at the owner's instruction that the journeys be written by the PM operator in the Cataloger's words, WHAT not HOW.

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

**Fix pass:** commit `7a465d2`, applied by `operator-agents:product-manager` from `docs/product/capture-mode/prd-capture-mode-round-2-fixes.md` (16 items, all ticked) under fences F14–F16 and the F12 amendment.

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

**Fix pass:** commit `6e0c03d`, applied by `operator-agents:product-manager` from `docs/product/capture-mode/prd-capture-mode-round-3-fixes.md` (12 items, all ticked) under fences F1–F17. From this round on, in-line dispatches run on Opus at the owner's instruction.

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

**Fix pass:** commit `abc429c`, applied by `operator-agents:product-manager` (Opus) from `docs/product/capture-mode/prd-capture-mode-round-4-fixes.md` (8 items, all ticked) under fences F1–F18; the jargon sweep returned zero hits across the whole document, hand-off lists included.

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

**Subject:** `docs/product/capture-mode/prd-capture-mode.md` and `docs/product/capture-mode/prd-capture-mode-oq-results.md` at commit `4a07a80` (132 R rows, 37 E rows, 8 M rows, 18 OQs, all ⌛️). **Fences:** F1–F25 as `--source`. **Lenses:** Claude on Opus — product manager, staff engineer, test (retargeted), architecture, product-marketing (added: end-user copy now exists in §12); agy cross-model on all five briefs (the first full round's offer, taken at the owner's standing instruction). **tier-rationale:** PRD tier always-on plus architecture (boundaries with decided architecture and the two hand-off PRDs) plus marketing (§12 copy); privacy/security/database/plan not triggered.

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

## Round 10 — delta verification of round 9 (2026-09-07)

**Subject:** commit `701ec4e` (144 R, 44 E, 11 M; 181 🤝, 18 ⌛️). **Lenses:** the same five on Claude/Opus over the 18 open rows plus any 🤝 row a round-9 edit broke, with the operator's three residue items put to every lens; agy on four (PM, staff, architecture, marketing), advisory.

| lens | route | round-9 items | new findings | open rows OBJECT | aligned rows OBJECT |
|---|---|---|---|---|---|
| product manager | Claude/Opus | all RESOLVED | 2 Major | 3 | none |
| staff engineer | Claude/Opus | all RESOLVED / BY-FENCE (R9-F2 with residue) | 4 Major, 5 questions | 3 | none |
| test (retargeted) | Claude/Opus | all RESOLVED; R9-F2 PARTIAL (carried by R10-T1, R10-T2) | 1 Blocker, 2 Major, 1 Minor | 6 | R3.11, R3.6, R8.3 |
| architecture | Claude/Opus | all RESOLVED | 1 Major, 2 Minor | 1 | E24 (on referral) |
| marketing (copy) | Claude/Opus | all RESOLVED; residue carried | 1 Blocker, 2 Major | 2 | E24 |
| agy (PM, staff, arch, marketing) | agy | all RESOLVED | staff: 2 Major (same as Claude's); marketing: 3 Major (1 accepted as hygiene, 1 rejected, 1 overruled) | R8.1, E2, E23, E27, E39 | E24 |

agy: PM, staff, and marketing rc 0; architecture rc 8 (short body). Advisory; the Claude lenses stand for each. The agy staff's two Majors coincide with R10-F1 and R10-F4. The agy marketing lens's three Majors: unify E2/E23's condition wording — accepted as hygiene (FX10-9); E27/E39 action cells lacking the unsettled condition — **rejected** (R10-X1, fence file; four Claude lenses judged it a false positive); E24 must carry the review action — **overruled** by the owner (fence F43).

### Verify-the-reviewer dispositions (Blockers and Majors)

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R10-F1 | R8.1's F42 clause calls a scan from the browse-opened list a one-row session "like any other (R3.11)"; R3.11 says a one-row session never moves the remembered row, while R3.6 and R8.3 say selecting a row in the review always sets it review-selected and R3.8 routes the next session there | Test R10-T1 (Blocker), Staff R10-F2, Arch A10-1 (Major) | `:775`, `:660`, `:655`, `:777`, `:657` confirmed. | **accept — Blocker.** Owner fork adjudicated: **F42 clarification (1)** — the browse never moves the remembered row; R3.6/R8.3 scoped to an in-session review. R3.6, R8.3, R3.11 re-open. |
| R10-F2 | The same clause has no carve-out for a paused bulk session, where R9.6 says any capture is a row inside that session; R3.2 makes the two exclusive; R8.7 already carries the right carve-out | Staff R10-F1 (Major), agy staff | `:775`, `:813`, `:777` (R8.7) confirmed. | **accept — Major.** Owner fork adjudicated: **F42 clarification (2)** — row inside the paused session, mirroring R9.6/R8.7. |
| R10-F3 | R8.2 now requires the list to show each row's settled mark, decision, and note and cites R11.12 for it; R11.12's set-aside-list clause is unchanged (cause, attempts, samples kept) and enumerates none of the list's actions, so F42's leave-action conditions have no observable | PM PM10-2, Test R10-T2 (Major), Arch and PMM notes | `:776`, `:860` confirmed. | accept — Major. |
| R10-F4 | E39's new body says "That clears the list"; R8.2 says the list shows every set-aside row, settled or not; E27's corrected body contradicts it | PMM PMM10-1 (Blocker), PM PM10-1, Test R10-T3, Staff R10-F4 (Major), agy staff | `:913`, `:776`, `:781` confirmed; round-9 FX9-2 wording. | accept — Blocker by the copy lens's scale; copy fix. |
| R10-F5 | R7.1 now calls Pause "the one deliberate exception" to the discard reason, but ending a session early also discards (R7.5, E23, UJ3.4) and puts nothing under the instrument | Staff R10-F3 (Major) | `:750`, `:754`, `:897` confirmed. | accept — Major. |
| R10-F6 | E24 says "set aside for good" flat and offers only "Open the collection" while E2, E23, E27 now carry the reversal; two lenses say no change is needed, two want it said | PMM PMM10-3 (Major), Arch A10-3 (Minor); PM and Test judged no defect | `:898`, `:764` confirmed. | Owner fork adjudicated: **fence F43** — one reversibility sentence, no new action, one rule that a collection-surface state never hides the surface's entry points. E24 re-opens. |
| R10-F7 | E23 "you can deal with them now or another day" renders when every set-aside row is settled; E2 has the honest line | PMM PMM10-2 (Major), Test R10-T4 (Minor) | `:897`, `:876` confirmed. | accept — Major, copy. |

Minors (Arch A10-2: R8.1's enumeration of the self-acting ways in omits R3.8's; agy PMM10-1 / operator residue: E2 and E23 word one condition two ways) are in `prd-capture-mode-round-10-fixes.md`. Recorded, not fixed: PM's note that E33's set-aside variant offers the review from the queue list, a third home R8.1 does not enumerate (reconcile when F39 is next touched); Test's note that E24's "Open the collection" reads as if E24 were not on the collection (pre-existing); Staff's question whether E25's three-count sentence should drop at ⟨pending⟩ = 0 (copy lens, later).

### Row flips

Flip rule as in round 6. Of the 18 open rows, **7 stay ⌛️** (an objection stands): R7.1, R8.1, R8.2, R8.15, R11.12, E23, E39. **11 flip to 🤝 Aligned**: R1.7, R4.22, R5.9, R7.5, R7.13, R8.5, R11.9, E2, E25, E27, E43. **Re-opened** by findings against 🤝 rows: R3.6, R8.3, R3.11 (R10-F1), E24 (R10-F6). Any 🤝 row a round-10 fix edits returns to ⌛️ "edited in round 10, re-review" (expected: R7.15, and E2/R1.7 if FX10-9 touches them).

**Round-10 status:** partially aligned. **Fix pass:** applied by `operator-agents:product-manager` on Opus (10 items + FX10-0; no new rows). After the pass: 186 🤝, 13 ⌛️ of 199 — 10 of the 11 listed rows flipped (E2 stayed ⌛️ because FX10-9 edited it); R3.6, R3.11, R8.3, E24 re-opened; R7.15 edited from 🤝; R8.2 and R8.15 held untouched. Open: R3.6 R3.11 R7.1 R7.15 R8.1 R8.2 R8.3 R8.15 R11.12 E2 E23 E24 E39. Operator residue for round 11: R7.1's discard list still names "entering the deferred-row review" unqualified while R8.1 scopes the discard to the in-session review; R1.7 states the offer condition in prose rather than quoting the action cell. Next: round 11 delta verification over the 13 open rows.

## Round 11 — delta verification of round 10 (2026-09-07)

**Subject:** commit `738af8b` (144 R, 44 E, 11 M; 186 🤝, 13 ⌛️). **Lenses:** the same five on Claude/Opus over the 13 open rows plus any 🤝 row a round-10 edit broke, with the operator's two residue items put to every lens; agy on four (PM, staff, architecture, marketing), advisory.

| lens | route | round-10 items | new findings | open rows OBJECT | aligned rows OBJECT |
|---|---|---|---|---|---|
| product manager | Claude/Opus | all RESOLVED (E33 note parked) | 2 Major, 1 Minor | 4 | none |
| staff engineer | Claude/Opus | all RESOLVED / BY-FENCE | 1 Major, 2 questions | 2 | none |
| test (retargeted) | Claude/Opus | all RESOLVED | 2 Major | 1 | none |
| architecture | Claude/Opus | all RESOLVED / BY-FENCE | 1 Major | 2 | none |
| marketing (copy) | Claude/Opus | all RESOLVED / BY-FENCE | 1 Blocker, 1 Major, 1 Minor | 8 | none |
| agy (PM, staff, arch, marketing) | agy | all RESOLVED | PM, staff, arch: the R7.1/R8.1 contradiction (same as R11-F1) | R7.1, R8.1 | none |

agy: PM, staff, architecture rc 0 with tables; marketing rc 8 (short body). Advisory; the Claude lenses stand for each. Every agy finding coincides with R11-F1. Both operator residue items were judged: R7.1's unqualified "entering the deferred-row review" is the same defect as R11-F1 seen from the other row (PM, PMM) or vacuous (staff, test, arch); R1.7's prose condition is not a defect (all five).

### Verify-the-reviewer dispositions (Blockers and Majors)

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R11-F1 | FX10-2's browse clause keys browse-versus-review on the surface the offer was taken from and asserts "no session is running behind it"; false in the paused case the same row enumerates, and under Reading B (R10.4 keeps the collection surface live during a session) the same act is R7.1/F19's in-session review with a discard; the end-early summary's "now" route is covered by neither branch | PMM PMM11-1 (Blocker), PM PM11-1, Staff S11-F1, Arch A11-1, Test R11-T1 and R11-T2 (Major), agy PM/staff/arch | `:775`, `:750`, `:655`, `:660`, `:777`, `:834`, `:522-529`, `:897`, UJ3.4 step 4 confirmed. | **accept — Blocker.** **F42 clarification (3)** recorded by the orchestrator, flagged for the owner: keyed on whether a session is capturing; four states enumerated; the E23 "now" route is the in-session review of UJ3.3. |
| R11-F2 | E39's "if you're in a session, it finishes it" and R8.15's "satisfies R8.6's completion condition in a single keypress" over-claim when rows are still pending (R8.6 completes only when every row is captured or left) | PMM PMM11-2 (Major) | `:913`, `:781`, `:780` confirmed. | accept — Major. |

Minors (PM PM11-2 with the copy lens's zero-count family: E23's widened sentence orphans at zero and the Placeholders rule needs a multi-count clause; PM PM11-3: R11.12 words "Leave them all set aside" as per-row; PMM PMM11-3 with Arch's note: E24's "Open the collection" action on a state that is already on the collection surface) are in `prd-capture-mode-round-11-fixes.md`. Recorded, not fixed: Arch's note that F43's rule sits inside R7.15 and may belong beside the Surfaces table if meant generally; E33's third home for the review offer (parked since round 10); PM's note that R8.1 now carries five rules in one row (authoring convenience, not gated).

### Row flips

Flip rule as in round 6. Of the 13 open rows, **10 stay ⌛️** (an objection stands): R3.6, R3.11, R7.1, R8.1, R8.3, R8.15, R11.12, E23, E24, E39. **3 flip to 🤝 Aligned**: R7.15, R8.2, E2. No 🤝 row was broken by a round-10 edit. Any 🤝 row a round-11 fix edits returns to ⌛️ "edited in round 11, re-review" (expected: R7.15 if FX11-5 touches it).

**Round-11 status:** partially aligned. **Fix pass:** applied by `operator-agents:product-manager` on Opus (5 items + FX11-0; no new rows). After the pass: 188 🤝, 11 ⌛️ of 199 — R8.2 and E2 flipped; R7.15 flipped then re-opened because FX11-5 edited it; the 10 held rows all edited. Open: R3.6 R3.11 R7.1 R7.15 R8.1 R8.3 R8.15 R11.12 E23 E24 E39. Operator residue for round 12: R7.1's later sentence still says "mid-session" where R8.1 now says "while a session is capturing"; "Done" added to the Labels paragraph though E24 names it; E24's headline and action both read "Done". Next: round 12 delta verification over the 11 open rows.

## Round 12 — delta verification of round 11 (2026-09-07)

**Subject:** commit `46e9afb` (144 R, 44 E, 11 M; 188 🤝, 11 ⌛️). **Lenses:** the same five on Claude/Opus over the 11 open rows plus any 🤝 row a round-11 edit broke, with the operator's three residue items put to every lens; agy on four, advisory.

| lens | route | round-11 items | new findings | open rows OBJECT | aligned rows OBJECT |
|---|---|---|---|---|---|
| product manager | Claude/Opus | all RESOLVED | 1 Major, 1 Minor | 4 | none |
| staff engineer | Claude/Opus | all RESOLVED | 2 Major, 1 Minor | 2 | R7.5 |
| test (retargeted) | Claude/Opus | all RESOLVED | 1 Blocker, 2 Major | 6 | R7.5 |
| architecture | Claude/Opus | A11-1 PARTIAL (predicate), rest RESOLVED / parked | 1 Blocker, 1 Major | 6 | none |
| marketing (copy) | Claude/Opus | all RESOLVED | 1 Blocker, 2 Major, 2 Minor | 3 | R7.5, Labels paragraph |
| agy (PM, staff, arch, marketing) | agy | all RESOLVED | PM, staff: R7.1's "mid-session" vs R8.1 (= R12-F4); arch: three items on R7.1, R7.15, E24 (residue restated) | R7.1, R8.1, E24 | none |

Operator residue judged by all five: R7.1's "mid-session" sentence is vacuous in the look-through states (staff, test, arch, PM) but is a message defect worth aligning (PMM) — folded into the predicate fix; "Done" in the Labels paragraph is right, the paragraph's framing is what is false (PMM, staff, arch, PM); E24's headline/action "Done" is a copy call (all) — accepted as a Minor.

### Verify-the-reviewer dispositions (Blockers and Majors)

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R12-F1 | The new discriminator "while a session is capturing on the collection" (R3.6, R3.11, R8.3, R7.1, R8.1) is (a) unqualified where R8.1's fourth state says "bulk", so a one-row session under way is covered by no state and answered twice (R3.11/R9.4 vs R3.6/R8.3), and (b) a term R3.1 defines to exclude time in the set-aside list, so read literally it is false at the moment it is evaluated; R3.11's re-keyed closing clause also conflates a one-row session with a row inside a paused session | Test R12-T1, Arch A12-1 (Blocker), PM PM12-1, Staff R12-2 (Major) | `:655`, `:777`, `:660`, `:650`, `:775`, `:811` confirmed. | **accept — Blocker.** **F42 clarification (4)** recorded by the orchestrator, flagged for the owner: the discriminator is a *live bulk session*, defined once in the Vocabulary; the one-row-session case is a look-through. |
| R12-F2 | R7.5 and E23 promise the partial set survives "until the session actually ends" / "when you stop", but R8.1's fourth state makes E23's "Review the set-aside swatches" the in-session review that lets it go on entry | PMM PMM12-1 (Blocker), Staff R12-1, Test R12-T3 (Major) | `:754`, `:897`, `:775` confirmed. | accept — Blocker by the copy lens's scale. R7.5 re-opens. |
| R12-F3 | E39's "if that was the last thing waiting, it finishes your session" and R8.15's completion clause are asserted where no session exists (R8.1's look-through states) and "the queue simply goes on" describes a queue that is not running | Test R12-T2, Arch A12-2 (Major), PM PM12-2 (Minor) | `:913`, `:781`, `:775` confirmed. | accept — Major. |
| R12-F4 | R7.1's later sentence "opening the set-aside list mid-session discards" keeps the retired key inside the row that carries the new one | PMM PMM12-3 (Major); staff, test, arch, PM: vacuous, not a contradiction | `:750` confirmed. | accept as a Minor wording fix inside R12-F1. |

Minors (PMM PMM12-4: E24's headline and action both "Done"; PMM PMM12-5 / Staff R12-3 / Arch note: the Labels paragraph's framing) are in `prd-capture-mode-round-12-fixes.md`. Recorded, not fixed: Arch's deferred question of what closes an unresumed interrupted session once a look-through leave-all leaves nothing pending and nothing unsettled (R7.11 covers relaunch only) — carried as a note for the Data Foundation hand-off; E33's third home (parked); F43's placement inside R7.15 (parked); R8.1's density (parked).

### Row flips

Flip rule as in round 6. Of the 11 open rows, **9 stay ⌛️** (an objection stands): R3.6, R3.11, R7.1, R8.1, R8.3, R8.15, E23, E24, E39. **2 flip to 🤝 Aligned**: R7.15, R11.12. **Re-opened** by findings against 🤝 rows: R7.5 (R12-F2). Any 🤝 row a round-12 fix edits returns to ⌛️ "edited in round 12, re-review" (expected: the Vocabulary is not a row; R3.1 is cited, not edited).

**Round-12 status:** partially aligned. **Fix pass:** applied by `operator-agents:product-manager` on Opus (6 items + FX12-0; no new rows; the Vocabulary gains "live bulk session"). After the pass: 189 🤝, 10 ⌛️ of 199 — R7.15 and R11.12 flipped; R7.5 re-opened; the 9 held rows all edited. Open: R3.6 R3.11 R7.1 R7.5 R8.1 R8.3 R8.15 E23 E24 E39. Operator residue for round 13: R4.22 still opens "While a session is capturing on a license…" (a different sense, the only surviving use of the retired phrase); E24's headline "That's the lot" over a body opening "That's ⟨rate⟩ an hour"; R7.5 is long; R8.15 says an unresumed interrupted session is "left exactly as it was" (Arch's deferred closure question, Data Foundation hand-off). Next: round 13 delta verification over the 10 open rows.

## Round 13 — delta verification of round 12 (2026-09-07)

**Subject:** the round-12 commit (144 R, 44 E, 11 M; 189 🤝, 10 ⌛️). **Lenses:** the same five on Claude/Opus over the 10 open rows plus any 🤝 row a round-12 edit broke, with the operator's four residue items put to every lens; agy on four, advisory.

| lens | route | round-12 items | new findings | open rows OBJECT | aligned rows OBJECT |
|---|---|---|---|---|---|
| product manager | Claude/Opus | all RESOLVED | 2 Major | 2 | none |
| staff engineer | Claude/Opus | all RESOLVED / BY-FENCE | 2 Major, 1 Minor, 3 questions | 3 | R7.11 |
| test (retargeted) | Claude/Opus | all RESOLVED / BY-FENCE; R12-T2 PARTIAL (carried by R13-1) | 1 Major | 1 | R7.11 |
| architecture | Claude/Opus | all RESOLVED / BY-FENCE | 2 Major | 2 | R7.11, E25 |
| marketing (copy) | Claude/Opus | all RESOLVED | 1 Major | 2 | E25 |
| agy (PM, staff, marketing; arch rc 8) | agy | all RESOLVED | PM: 1 Blocker (= R13-F1) plus nits on R7.5, R4.22, E24 | R8.15, R4.22 | none |

Operator residue judged by all five: R4.22's "While a session is capturing on a license…" is a different sense and not a live instance of the retired key (no change); E24's "That's the lot / That's ⟨rate⟩" echo is copy cadence (deferred); R7.5's length is readability, every clause assertable (no change); R8.15's "left exactly as it was" is the defect R13-F1 names.

### Verify-the-reviewer dispositions (Blockers and Majors)

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R13-F1 | A look-through "Leave them all set aside" that settles the last unsettled row beside an unresumed interrupted session leaves the session "exactly as it was" while the collection reads finished: R1.7 shows E2's finished variant, R7.8 shows E25 with no renderable variant, R7.11 closes the session on relaunch only, E39 says nothing is waiting | PMM PMM13-1, Test R13-1, Arch A13-1, Staff R13-2 (Major), agy PM (Blocker) | `:782`, `:761`, `:758`, `:900`, `:605`, `:369` confirmed. | **accept — Major.** **F31 clarification (2)** recorded by the orchestrator, flagged for the owner: closed as complete whenever the condition becomes true. R7.11 and E25 re-open. |
| R13-F2 | "Live bulk session" does not say whose pause: the guard's pause (R5.11) and a device halt are neither an operator Pause nor an interruption, and a pause lifted for one row (R9.6) makes the predicate flip mid-scan, moving the remembered row on a swatch selected from a look-through | PM PM13-1, Staff R13-1 (Major) | `:132`, `:716`, `:814`, `:776`; device PRD `:421`, `:439` confirmed. | **accept — Major.** **F42 clarification (5)** recorded by the orchestrator, flagged for the owner: guard pause and halt are not live; the classification is fixed when the list opens. |
| R13-F3 | R8.15's "rows still pending … the queue simply goes on" contradicts R8.6 and UJ3.3 step 4, which end the session early on leaving the review before completion, for a review entered mid-run | PM PM13-2 (Major), Staff R13-3 (Minor, UJ3.3) | `:782`, `:781`, `:349` confirmed. | **accept — Major.** Owner fork adjudicated: **fence F44** (leaving a review entered with rows pending returns to the queue; the end-early rule is the end-of-queue review's). |
| R13-F4 | E39's "in a session you're running" is true of an interrupted session E25 offers to resume and of a paused one, so it promises a finish R8.15 denies | Arch A13-2 (Major) | `:914`, `:782` confirmed. | accept — Major, copy: "in the session you're capturing in". |

Minors (Test: R7.1's "no exception for looking" is narrowed to live bulk sessions while R8.1 gives the one-row case one and cites R7.1; Arch: R7.1's cue placement does not name the one-row case's surface) are in `prd-capture-mode-round-13-fixes.md`. agy's nits on R7.5 and R4.22 restate residue already judged; not raised.

### Row flips

Flip rule as in round 6. Of the 10 open rows, **4 stay ⌛️** (an objection stands): R3.11, R8.1, R8.15, E39. **6 flip to 🤝 Aligned**: R3.6, R7.1, R7.5, R8.3, E23, E24. **Re-opened** by findings against 🤝 rows: R7.11 (R13-F1), E25 (R13-F1). Any 🤝 row a round-13 fix edits returns to ⌛️ "edited in round 13, re-review" (expected: R8.6, R7.1, and the Vocabulary entry which is not a row).

**Round-13 status:** partially aligned. **Fix pass:** applied by `operator-agents:product-manager` on Opus (6 items + FX13-0; no new rows). After the pass: 191 🤝, 8 ⌛️ of 199 — R3.6, R7.5, R8.3, E23, E24 flipped; R7.1 flipped then re-opened by FX13-5; R7.11 and E25 re-opened; R8.6 edited from 🤝; R3.11, R8.1, R8.15, E39 edited. Orchestrator hygiene edit, not a WHAT change: the F42 citation count in R3.6 and R8.3 bumped from "four times" to "five times" after the flip (status kept). Open: R3.11 R7.1 R7.11 R8.1 R8.6 R8.15 E25 E39. Operator judgment calls for round 14: R8.1's held state gained a sentence that under the guard's pause or a halt there is nothing to scan from the list until the cause is dealt with (R4.2, R5.11); E25 gained no variant because the withdrawn offer means it is never shown for an empty session; rows leaning on the first F31 clarification still cite "as clarified" (only R7.11, R8.15 cite "twice"). Next: round 14 delta verification over the 8 open rows.

## Round 14 — delta verification of round 13 (2026-09-07)

**Subject:** the round-13 commit (144 R, 44 E, 11 M; 191 🤝, 8 ⌛️). **Lenses:** the same five on Claude/Opus over the 8 open rows plus any 🤝 row a round-13 edit broke, with the operator's three residue items put to every lens; agy on four, advisory (three of four returned at logging time; the summary is saved with the scratchpad reviews).

| lens | route | round-13 items | new findings | open rows OBJECT | aligned rows OBJECT |
|---|---|---|---|---|---|
| product manager | Claude/Opus | all RESOLVED / BY-FENCE | 1 Major | 3 | none |
| staff engineer | Claude/Opus | all RESOLVED / BY-FENCE; R13-3 PARTIAL (carried by S14-1) | 1 Blocker, 1 Major, 1 Minor, 4 questions | 4 | E42 |
| test (retargeted) | Claude/Opus | all RESOLVED | 3 Major | 3 | none (UJ3.3 step 4, not a row) |
| architecture | Claude/Opus | all RESOLVED / BY-FENCE | 2 Major | 4 | none |
| marketing (copy) | Claude/Opus | all RESOLVED | 1 Blocker, 1 Major | 4 | none |
| agy (PM, staff rc 8; arch, marketing rc 0) | agy | all RESOLVED | marketing: two Major copy/logic items on R8.15 and E39 (coincide with R14-F3) | R8.15, E39 | none |

Operator residue judged by all five: R8.1's held-state sentence is right in substance (device PRD §5, R5.11) but its reason and its "until" are defective (R14-F4); E25 correctly gained no variant (all five); the F31 citation split is precise (all five).

### Verify-the-reviewer dispositions (Blockers and Majors)

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R14-F1 | FX13-3 wrote F44 under two keys: R8.6 and R8.1 class the end-early summary's "now" with the end-of-run rule, while UJ3.3 step 4 and R8.15 key on rows pending and send that same route to the detour | PMM PMM14-1, Staff S14-1 (Blocker), PM PM14-1 (Major) | `:351`, `:783`, `:778`, `:784`, `:900` confirmed; F44's own text names the summary's "now" as end-of-run. | **accept — Blocker.** **F44 clarification (1)** recorded: the summary's "now" is the end-of-run review whatever is pending. |
| R14-F2 | The detour exit "back on the queue at the remembered row" names a pointer the review's own selection has moved (R3.6, R8.3), so leaving lands on the adjudicated row, not where the operator was; UJ3.3 says "where I was"; R8.12/R9.11 say it correctly | Arch A14-1, Test F14-1 (Major) | `:783`, `:778`, `:784`, `:658`, `:780`, `:660`, `:351` confirmed. | **accept — Major.** **F44 clarification (2)** recorded: a detour review moves nothing; returns to the row the operator left; the review-selected update belongs to the end-of-queue review. R3.6 and R8.3 will be scoped. |
| R14-F3 | R8.15's look-through branch says "finishes no session of the operator's" and then closes an interrupted one as complete; a one-row session already under way from the list is a session the operator is capturing in, and nothing says what settling its row does; E39 says nothing about the summary that then appears | Arch A14-2, PMM PMM14-2 (Major) | `:784`, `:916`, `:778` confirmed. | accept — Major. |
| R14-F4 | R8.1's new held-state clause cites R4.2, which lists the operator's pause in the same breath, so the reason proves too much; its "until the operator has dealt with the cause" opens a state (force-resume or "Resume scanning" with the list still open) no row covers | Test F14-2 (Major), Staff S14-3 (Minor) | `:778`, `:676`, `:663` confirmed. | accept — Major: the reason is the lift mechanism; dealing with the cause closes the look-through. |
| R14-F5 | UJ3.3 step 4's third bullet says "the run I was on is already complete, or there was none", false for a paused/held or interrupted session | Test F14-3 (Major) | `:352`, `:778`, `:784` confirmed. | accept — Major, journey wording. |
| R14-F6 | R7.1 routes the abandoned look-through one-row session to E42, whose body says the swatch "is still waiting in the queue"; it goes back to the set-aside list | Staff S14-2 (Major) | `:753`, `:919`, `:779` confirmed. | accept — Major; E42 re-opens with a set-aside variant. |

Minor (Staff Q3: R7.11's enumeration names only a settle decision as the mid-run trigger while its headline covers a capture clearing the last row) is in `prd-capture-mode-round-14-fixes.md`. Staff Q2 (R7.8's unqualified offer) judged the general-row convention by test and arch; no change. Recorded, not fixed: Arch's suggestion of a per-state × per-exit table for R8.1/R8.15 (authoring aid; consider before lock).

### Row flips

Flip rule as in round 6. Of the 8 open rows, **5 stay ⌛️** (an objection stands): R7.1, R8.1, R8.6, R8.15, E39. **3 flip to 🤝 Aligned**: R3.11, R7.11, E25. **Re-opened** by findings against 🤝 rows: E42 (R14-F6). Any 🤝 row a round-14 fix edits returns to ⌛️ "edited in round 14, re-review" (expected: R3.6, R8.3, R3.8 if its wording changes).

**Round-14 status:** partially aligned. **Fix pass:** applied by `operator-agents:product-manager` on Opus (8 items + FX14-0; no new rows; each seam rule now lives in one row — entry states R8.1, exit precedence R8.6, leave-all effects R8.15, interrupted closure R7.11 — and the others cite it). After the pass: 188 🤝, 11 ⌛️ of 199 — R3.11 and E25 flipped; R7.11 flipped then re-opened by FX14-7; E42 re-opened; R3.6, R3.8, R8.3, R11.12 edited from 🤝; the five held rows edited. Open: R3.6 R3.8 R7.1 R7.11 R8.1 R8.3 R8.6 R8.15 R11.12 E39 E42. Operator residue for round 15: R8.6's unconditional-looking first sentence precedes its scope; R8.1's "never on the surface" sits near the detour test that names the collection surface; Arch's per-state × per-exit table suggestion stands. Next: round 15 delta verification over the 11 open rows.

## Round 15 — delta verification of round 14 (2026-09-07)

**Subject:** the round-14 commit (144 R, 44 E, 11 M; 188 🤝, 11 ⌛️). **Lenses:** the same five on Claude/Opus over the 11 open rows plus any 🤝 row a round-14 edit broke, with the operator's two residue items put to every lens; agy on four, advisory (three of four returned at logging; PM objected R8.1/R8.6 on items coinciding with R15-F1/F2; summary saved with the scratchpad reviews).

| lens | route | round-14 items | new findings | open rows OBJECT | aligned rows OBJECT |
|---|---|---|---|---|---|
| product manager | Claude/Opus | all RESOLVED | 1 Major | 1 | none |
| staff engineer | Claude/Opus | all RESOLVED / BY-FENCE | 1 Major, 4 questions | 3 | none |
| test (retargeted) | Claude/Opus | all RESOLVED | 2 Major | 2 | none |
| architecture | Claude/Opus | all RESOLVED | 1 Major | 2 | none |
| marketing (copy) | Claude/Opus | all RESOLVED | 2 Major | 3 | none |
| agy (PM rc 8; staff, arch, marketing rc 0) | agy | all RESOLVED | staff: R8.1/R8.6 items coinciding with R15-F1/F2; marketing: two Minors restating the operator residue (judged false positives by all five Claude lenses); arch none | R8.1, R8.6 | none |

Operator residue judged by all five: both false positives — R8.6's early-end sentence is scoped by the next sentence; R8.1's "never on the surface" answers how the list opens, the detour test which exit a review has. No change.

### Verify-the-reviewer dispositions (Blockers and Majors)

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R15-F1 | R8.15's new one-row exception discards a part-finished set on settling, and cites R7.1, which does not carry that trigger (its exception list closes at "Two of the operator's own actions") | Arch R15-A1, Test R15-F2 (Major) | `:753`, `:784` confirmed. | accept — Major. R7.1 owns the trigger; "Two" becomes "Three". |
| R15-F2 | R8.6's exit precedence enumerates two end-of-run entries and one detour; R3.8's resume into the review at a review-selected row is neither, and is reachable with rows still pending via the summary's "now" | PM PM15-1, PMM PMM15-2 (Major) | `:783`, `:660`, `:780` confirmed. | accept — Major. **F44 clarification (2)** recorded: three ways in; a resume into the review with rows pending is the detour. |
| R15-F3 | A partial set let go off a captured row mid-re-scan (R8.7) by a halt or jump: R7.1 says the row "returns to sample 0 and stays pending" and E42 has no true variant, contradicting R8.8/R8.10 (the row stays captured with its value) | Staff S15-1 (Major) | `:753`, `:919`, `:793`, `:795` confirmed. | accept — Major. E42 gains a third variant; R7.1 scopes "stays pending" to a row that was pending. |
| R15-F4 | E39 promises "with everything the instrument did read" while the one-row exception lets the part-finished set go at that confirm | PMM PMM15-1 (Major) | `:916`, `:784` confirmed. | accept — Major, copy; conditional line enumerated in R11.12. |
| R15-F5 | R8.15's exception is attached to the interrupted sub-case only, and "No other row under the instrument is touched by either of them" is false for a review-selected row holding a part-finished set (R8.3, R8.4) | Test R15-F1 (Major) | `:784`, `:780`, `:781` confirmed. | accept — Major. **F31 clarification (3)** recorded: leave-all settles the row under the instrument too; its samples go under R7.1. |

Minors: Staff Q4 (R8.15's rows-pending branch should say the session ends on leaving, per R8.6, not on the keypress); PM and Test both note the session state diagram lacks the detour edges (Capturing → Review from the collection surface; Review → Capturing) — accepted as a Minor since F44 made the detour a rule. Staff Q2 (paused look-through abandonment uses E42's set-aside variant) — yes; covered by R7.1's governing clause, one cite added. Recorded, not fixed: Arch's pre-lock pass asserting every discard is in R7.1, every exit in R8.6, every entry state in R8.1; R8.15's held-case reason is imprecise for a save-failure halt (R7.2) — a word if edited; E27 carries the same "everything the instrument did read" phrase (owner's eye).

### Row flips

Flip rule as in round 6. Of the 11 open rows, **7 stay ⌛️** (an objection stands): R3.8, R7.1, R8.6, R8.15, R11.12, E39, E42. **4 flip to 🤝 Aligned**: R3.6, R7.11, R8.1, R8.3. No 🤝 row was broken by a round-14 edit. Any 🤝 row a round-15 fix edits returns to ⌛️ "edited in round 15, re-review" (expected: R8.1 if its one-row sentence is changed to cite R7.1; R3.11 if cited wording changes).

**Round-15 status:** partially aligned. **Fix pass:** applied by `operator-agents:product-manager` on Opus (7 items + FX15-0; no new rows; the session diagram gained the two detour edges). After the pass: 191 🤝, 8 ⌛️ of 199 — R3.6, R7.11, R8.3 flipped; R8.1 flipped then re-opened by FX15-1/2; the other held rows edited. Open: R3.8 R7.1 R8.1 R8.6 R8.15 R11.12 E39 E42. Residue: R3.6's gloss of the end-of-run review names two of R8.6's three ways in; E27 keeps "everything the instrument did read" while E39 now says "already kept on them"; R8.15's held-case reason imprecise for a save-failure halt. **Owner decision, same day (fence F45):** every §7 row is compacted to at most two sentences, concise over prose, the table shrinking not growing; the compaction is one pass, after which round 16 verifies every row against the before-and-after diff. The three residue items are folded into that pass. Measured before compaction: 144 R rows, 17,253 words, mean 120 per row, 13 rows over 250 words, longest 1,035 (R8.1).

## Round 16 — compaction verification and the plan reviewer (2026-09-07)

**Subject:** commit `244608f` (144→160 R rows, 17,253→10,496 words in §7; every R row and E27 at ⌛️ "compacted (F45)"). **Lenses:** the five usual lenses on Claude/Opus verifying every row against the pre-compaction text (`98a2e95`) with the compaction map as source; plus `peer-plan-reviewer`, retargeted on buildability and a simplification cut list at the owner's request; agy on four, advisory (three of four returned at logging).

| lens | route | verdict on the compaction | new findings | open rows OBJECT | other OBJECT |
|---|---|---|---|---|---|
| product manager | Claude/Opus | every rule survives | 1 Major, 3 Minor, 4 CUT | R2.10, R8.15, R11.13 | M4 M5 M9 M11, :971 |
| staff engineer | Claude/Opus | every rule survives | 1 Major, 5 Minor, 9 CUT, 6 questions | R2.7, R6.9, R8.1, R11.15 | M11, OQ 21/24/25, Placeholders |
| test (retargeted) | Claude/Opus | every rule survives | 2 Major, 3 Minor, 9 CUT | R1.7, R4.24, R7.18, R8.15, R8.17, R10.8, R11.12, R11.13, R11.16 | OQ table, :971 |
| architecture | Claude/Opus | faithful | 2 Major, 6 Minor, ~30 CUT | R2.7, R5.4, R6.1, R8.15 | :971, OQ Feeds |
| marketing (copy) | Claude/Opus | every rule preserved | 3 Major, 6 CUT | R8.1 | M11 |
| plan reviewer (retargeted) | Claude/Opus | rules complete; not buildable unattended; twice the length a builder needs | 4 Blocker, 6 Major, ranked cut list, structure proposal | 12 OBJECT, 16 CUT | — |
| agy (PM, marketing rc 0; staff rc 8; arch rc 1) | agy | every rule retained | advisory; marketing objected E27 (copy wording, see the run summary) | E27 | — |

### Verify-the-reviewer dispositions (Blockers and Majors)

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R16-F1 | The sixteen splits moved rules but the tables around §7 still point at the parents: the §12 inherited-obligations paragraph omits R4.24, R7.18, R8.17, R11.16 and lists R8.15, R11.13; M4/M5/M9 cite R8.13 (now R8.17); M11, OQ 24, OQ 25 cite R11.13 (now R11.16); OQ 8's tie-break cites R10.7 (now R10.8); OQ 16 and OQ 21 cite R4.22/R4.12 (now R4.26/R4.24); the Placeholders paragraph cites R7.1 (now R7.17); the OQ Feeds column names no split row | PM, Staff, Test M-1/M-2, Arch A1/A6, PMM PMM16-1, Plan M1/M2 (Major) | `:971`, `:983-990`, `:1000-1022`, `:920` confirmed. | **accept — Major.** Regenerate every row citation outside §7 from the post-split IDs. |
| R16-F2 | R8.15's "It settles the swatch under the instrument too" lost its condition; in a detour the swatch under the instrument is a pending queue row | Arch A2 (Major) | `:826`, `:816` confirmed. | accept — Major: "where that swatch is one of the rows being settled". |
| R16-F3 | The seam table's held row says "the held row keeps its samples", true of the guard's pause and false of a device halt (R7.1, UJ3.6) | PMM PMM16-2 (Major) | `:810`, `:772`, `:391` confirmed. | accept — Major: split the cell. |
| R16-F4 | The compaction map handed to reviewers is pre-tightening (11,155 vs 10,496) | PMM PMM16-3 (Major), Arch A5, Staff | confirmed. | accept — regenerate the map from the committed file. |
| R16-F5 | Plan reviewer Blockers: every row at ⌛️; five constants at "candidate TBD"; DEMO_SCAN_CYCLE without a candidate behind a P1 device-PRD row; 21 P0 obligations on a Data Foundation PRD that does not exist behind a stale ADR gate; and Majors: four P0 rows defer to an OQ with no interim rule, no build order for the two-reading prototype, R7.14 vs device PRD §5 | Plan B1–B4, M3–M6 | `:558`, `:677`, `:695`, `:702`, `:899`, `:841`, `:1027` confirmed. | **Owner decision (fence F46 (4)):** left for the engineering plan, recorded as one short hand-off list in the PRD; B1 clears with round-16 flips. |
| R16-F6 | Plan reviewer cut list and structure proposal (journeys and four diagrams to a companion file; §12 copy to a companion file; OQ Details trimmed; obligations paragraph as a table; fence map to the fence file; intro tables and battery note cut; row-level cuts) | Plan reviewer | reviewed by the orchestrator against the text. | **Owner decision (fence F46 (1)–(3)):** all accepted. |

Minors accepted into the fix file: R2.7's whitespace generality and its named inputs; R6.9's "A" before "A2"; R11.15/R8.15's list-level framing; R11.12's "without matching wording" and E41's variants; R5.4's K_FAILED_ATTEMPTS cite; R4.15's "full"; R2.10's commit-failure hook into R11.6; R8.15's Data Foundation tag; Traceability F39 gloss (moot once the map moves); E44 harmonised with E27; seam table rows lettered; the R3.9 entry confirmed as end-of-run (Staff question 1 — consistent with F42 clarification (3) and F44 clarification (2)). Every CUT clause the five lenses named is applied under F46 (3). agy: see the amended row when its run completes.

### Row flips

Verification round: a row flips where every non-abstaining Claude verification lens ALIGNed. Of the 160 R rows and E27, **16 stay ⌛️** (an objection stands): R1.7, R2.7, R2.10, R4.24, R5.4, R6.1, R6.9, R7.18, R8.1, R8.15, R8.17, R10.8, R11.12, R11.13, R11.15, R11.16. **145 flip to 🤝 Aligned** (144 R rows and E27). The plan reviewer's OBJECTs are buildability items the owner routed to the engineering plan and do not hold rows; its CUTs are applied by the fix pass. Any 🤝 row the round-16 fix pass edits under F46 returns to ⌛️ "cut (F46), verify in round 17"; rows removed under F46 retire their IDs.

**Round-16 status:** compaction verified — no rule lost. **Fix pass (F46 restructure):** applied by `operator-agents:product-manager` on Opus (11 items + FX16-0). The PRD file went from 33,164 to 19,302 words; the journeys and five diagrams moved verbatim to `prd-capture-mode-journeys.md` (6,986 words) and the E1–E44 copy table to `prd-capture-mode-copy.md` (2,959 words); ten row IDs retired (R4.25, R7.4, R7.6, R7.10, R8.11, R10.2, R11.1, R11.2, R11.4, R11.9); 149 R rows plus R11.15 as a table caption, 9,492 requirement words, mean 64, none over two sentences; every OQ, metric, and obligation citation regenerated from the post-split IDs; the compaction map regenerated. After the pass: 101 🤝 (99 R + 2 M) and 59 ⌛️ (35 R cut under F46, 16 R held from round 16, 9 M edited) plus E44 in the copy file. Operator judgment calls: 12k words is not reachable without cutting rules (19.3k is the honest floor: 9.7k rule text, the seam table, the two contract paragraphs, the obligations table, metrics definitions, the trimmed OQ table); R5.2 keeps its halt taxonomy sentence because fence F7 has no other home; the session-lifecycle diagram moved to the journeys file rather than being cut; the Vocabulary stays in the PRD; the OQ table's Evidence/Gated/Depends columns folded into the Closer cell. Next: round 17 verifies the cut rows and the held-row fixes.

## Round 17 — verification of the F46 restructure (2026-09-07)

**Subject:** commit `16cc7f3` (PRD 19,302 words; 149 R rows + R11.15 caption; 59 R/M rows and E39/E42/E44 open). **Lenses:** the five usual lenses on Claude/Opus verifying the open rows against the pre-restructure text (`244608f`); agy on four, advisory (PM rc 0, no objections; staff, arch, marketing rc 8).

| lens | route | round-16 items | new findings | open rows OBJECT | other OBJECT |
|---|---|---|---|---|---|
| product manager | Claude/Opus | all RESOLVED / BY-FENCE (R7.12 CUT not applied, judged not a defect) | 3 Minor, 1 Nit | R11.15 | parity-gate obligation line |
| staff engineer | Claude/Opus | all RESOLVED / BY-FENCE; R16-F4 PARTIAL (map lacks R4.25) | 3 Minor, 3 Nit, 5 questions | R11.15 | none |
| test (retargeted) | Claude/Opus | all RESOLVED | 2 Minor, 4 Nit | R3.12 | F40 map entry, §11 preamble |
| architecture | Claude/Opus | all RESOLVED; §8 two-tables PARTIAL | 5 Minor, 3 Nit | R11.15 | Legend list (R1.5), obligations table cells |
| marketing (copy) | Claude/Opus | all RESOLVED; "deferred/set aside" nit carried | 4 Minor, 3 Nit | R3.12, R4.4, R5.9, R10.4, R11.13 | parity-gate obligation line |
| agy | agy | — | none | — | — |

**No Blocker and no Major.** Every lens confirms the restructure lost no rule: every retired ID's rule has a home, every citation across the three files resolves, and no 🤝 row, journey, or copy row was broken.

### Verify-the-reviewer dispositions (Minors accepted into the fix file)

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R17-M1 | R11.15's capture-surface cell lost "listed the same way under a halt"; and R11.15 is a caption, not a row, so a script scanning rows misses it | Staff S17-1/S17-2, PM PM17-1, Arch A17-5 | `:426`, `:430` confirmed. | accept: restore the clause; restore a one-row line above the table as R8.1 has. |
| R17-M2 | The obligations table's parity-gate line lost retired R11.1's "within the device PRD's closed list of exceptions as amended by R11.3, R4.26"; its Device PRD §5 line lost R7.5 | PM PM17-2, Arch A17-3/A17-4, PMM PMM17-1 (test judged the parity line acceptable) | `:456`, `:457` confirmed. | accept: restore the qualifier and the R7.5 citation. |
| R17-M3 | The Legend's engineering-plan list says R1.5 has no interim rule; OQ 21 carries D50/2° (only the pair list is open) | Staff nit, Arch A17-1 | `:76`, `:505` confirmed. | accept: reword. |
| R17-M4 | R3.12 names three constants with no candidate while the Legend says every constant carries one in-row | PMM PMM17-3, Test T17-1, Staff Q3 | `:191`, `:87` confirmed. | accept: amend the Legend sentence ("here or in its Open Questions entry") — no numbers return to the row. |
| R17-M5 | R11.13 is orphaned (M11 and the obligations table moved to R11.16); R4.4 and R10.4 should cite it | PMM PMM17-4 | confirmed. | accept. |
| R17-M6 | F40's map entry cites retired R11.9 and §11's preamble step 3 no longer cites R5.9's floor; R5.9's "counts toward both" clause was cut (test: the definition's "by either route" carries it; PMM disagrees) | Test T17-2, PMM PMM17-2 | confirmed. | accept: step 3 cites R5.9's floor; F40's map entry repointed; R5.9 gains "a row auto-deferred counts toward both" as a clause of the definition (four words, a rule not rationale). |
| R17-M7 | The obligations table sits under a section titled "Error & State Copy" | Arch A17-2, Staff nit | confirmed. | accept: its own section "Inherited obligations". |
| R17-M8 | Nits: compaction map lacks an R4.25 row and its totals differ from the log by R11.15's counting; the copy file's header says "a requirement row above"; R8.1d's "which" antecedent; :31 says four diagrams (five); four rows still read "verify in round 16"; tally labels "deferred" vs the copy's "set aside" (R4.15) | Staff, Test, PMM | confirmed. | accept as hygiene. |

Judged not defects (recorded): R5.2's dropped hand-off tag (fence F7 already routes drift; the device PRD owns the halt taxonomy); R7.12's second sentence kept; the device PRD's literal `&amp;` heading slugs (pre-existing; note for the OQ 16 amendment); R4.4's "find" without a Labels entry (pre-existing); the Placeholders "E39 alone" wording (pre-existing).

### Row flips

Verification round. Of the 62 open rows, **6 stay ⌛️** (an objection stands): R3.12, R4.4, R5.9, R10.4, R11.13, R11.15. **56 flip to 🤝 Aligned**: the other 50 R/M rows and E39, E42, E44. Any 🤝 row the round-17 fix pass edits returns to ⌛️ "edited in round 17, re-review".

**Round-17 status:** restructure verified — no rule lost, no Blocker or Major. **Fix pass:** applied by `operator-agents:product-manager` on Opus (6 items + FX17-0). Correction to the flip paragraph above: the open set was 62 rows plus R11.15's caption; 55 flipped (R4.15 and R8.1 were edited by FX17-6 and stay ⌛️ with R4.4, R5.9, R10.4, R11.15; R3.12 and R11.13 stay held untouched). After the pass: PRD 18,959 words; 150 R rows (R11.15 is a row again) and 11 M rows; 153 🤝, 8 ⌛️ (R3.12 R4.4 R4.15 R5.9 R8.1 R10.4 R11.13 R11.15); copy file 44 🤝. The inherited-obligations table is now its own section. Operator notes: R8.1 returned to ⌛️ because the seam table's R8.1d cell was edited; held rows that no fix touches keep a stale round note (a rule for round 18: a held, untouched row reads "held since round N"). Next: round 18 as the Phase 5 priority pass and the Phase 6 pre-lock round together (five lenses delta-verify; `peer-plan-reviewer` retargeted on the lock question; `peer-interface-reviewer` as the fresh lens on the cross-document contract).

## Round 18 — priority pass and pre-lock round (2026-09-07)

**Subject:** the round-17 commit (PRD 18,959 words; 150 R + 11 M rows; 153 🤝, 8 ⌛️; copy 44/44). **Lenses:** the five usual lenses on Claude/Opus (delta verification of round 17 plus the priority question); `peer-plan-reviewer` retargeted on the lock question ("Could the follow-on spike and first build phase execute from this document unattended…"); `peer-interface-reviewer` as the fresh lens the pre-lock round requires (first time on this document); agy on four, advisory (PM rc 0; staff, arch, marketing rc 8).

| lens | route | round-17 items | new findings | open rows OBJECT | other OBJECT |
|---|---|---|---|---|---|
| product manager | Claude/Opus | all RESOLVED | 1 Major (priority), 3 Minor, 2 Nit | R11.15 | none |
| staff engineer | Claude/Opus | all RESOLVED | 1 Major (priority), 1 Minor, 3 Nit, 4 questions | R8.1, R11.15 | none |
| test (retargeted) | Claude/Opus | all RESOLVED; T17-2c PARTIAL (carried by T18-1) | 2 Major, 1 Minor, 4 Nit | R4.15, R5.9, R11.15 | none |
| architecture | Claude/Opus | all RESOLVED | 1 Major (priority), 2 Minor, 2 Nit | none | R10.7 (priority) |
| marketing (copy) | Claude/Opus | all RESOLVED | 2 Major (priority; stale labels), 1 Minor, 2 Nit | R3.12, R11.13, R11.15 | R6.3, R8.5 (priority) |
| plan reviewer (lock) | Claude/Opus | B1–B3, M1–M5 RESOLVED / BY-FENCE; B4 PARTIAL | verdict **proceed to lock**; 1 Major (priority), 4 Minor | — | R6.3 (priority) |
| interface (fresh lens) | Claude/Opus | — | 1 Blocker, 8 Major, 7 Minor, 1 Nit | R3.12, R11.13, R11.15 | Placeholders, R4.10, R8.4, Surfaces, Legend status, Traceability, seam table header, obligations §5 and parity lines, OQ table, copy E18/E23/E30, README |
| agy PM | agy | — | 1 Major: R4.15/R8.1 placement gap on OQ 8 (advisory; the Legend's hand-off list already carries it, and F25 closes OQ 8 in the first build) | R4.15, R8.1 | — |

**Priority pass answer (Phase 5):** yes, one deferral breaks the first usable build — P0 rows and states offer "Re-scan" and "Add a swatch" whose rows are P1 (R6.3, R1.7/E2, R4.21/E36, R11.15, R7.19), and R10.7's reorder observable depends on P1 reorder rows (six lenses). Everything else deferred is severable.

### Verify-the-reviewer dispositions (Blockers and Majors)

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R18-F1 | The P0 set is not closed under its own citations: P0 surfaces and states offer P1 actions; R10.7's reorder observable is P1-dependent | PMM PMM18-1, PM PM18-1, Staff S18-1, Test T18-2, Arch A18-1, Plan P18-1 (Major) | `:263`, `:440`, `:133`, `:299`, `:391`, `:268`, `:271` confirmed; F24 makes priority build order. | **accept — Major.** Owner fork adjudicated: **fence F47** (phase the offers; F24 stands; R10.7's observable recorded once reordering lands; M5 from P1). |
| R18-F2 | N_CONSEC_FLAGGED's event set is identical to N_CONSEC_HARD's (a disagreeing set is a failed attempt whose only set-aside route is a Skip after it), so it can never fire first and its absence is undetectable | Test T18-1 (Major) | `:246`, `:240`, `:210`, F18 confirmed. | **accept — Major.** Owner fork adjudicated: **fence F48** (one counter now). |
| R18-F3 | The ⟨dropped⟩ placeholder rule names E39 as the only before-the-action confirmation; E23 is a second, so a literal build computes zero and drops the partial-set warning | Interface I18-1 (Blocker) | `:467`, copy `:34` confirmed. | **accept — Blocker.** Name E23 and E39. |
| R18-F4 | R4.10 names re-take / "Accept the average" / "Skip" where E18 ships Take it again / Accept the average / Set it aside, and §12's tiebreak endorses the row; R8.4's review disagreement has no copy variant (R11.12 says E18 has two) | Interface I18-2, I18-3 (Major) | `:210`, copy `:29`, `:332` confirmed. | accept — Major: R4.10 quotes E18 verbatim; E18 gains a review variant; R11.12 enumerates three. |
| R18-F5 | Two surface registries (the Surfaces table and R11.15's table) disagree on names and membership; E28/E29 declared on the collection surface only while R6.3/R8.7 offer re-scan mid-session | Interface I18-4 (Major), Test T18-7, Staff | `:108-115`, `:432-441` confirmed. | accept — Major: the Surfaces table is the one registry; R11.15 uses its names. |
| R18-F6 | Four status vocabularies (bare value; value + round note; OQ "open"; the copy file's column); Traceability declares three ID families while R8.1a–k is a fourth; the seam table's ID column is headed "#" | Interface I18-5, I18-6 (Major) | confirmed. | accept — Major, lock hygiene: status cells hold a legend value only from this pass on; lettered sub-families declared; columns headed ID. |
| R18-F7 | The obligations table's Device PRD §5 line restates three clauses the device PRD already has and amends one without saying so; the parity-gate line asks CI to exercise a live instrument, which AGENTS.md §5 says CI cannot | Interface I18-7, I18-8 (Major) | `:455`, `:454`, AGENTS.md §5 confirmed. | accept — Major. |
| R18-F8 | Ten placeholder tokens are used in the copy without a registry entry, and the two-way branch construct ⟨a / b⟩ is undefined | Interface I18-9 (Major) | copy file confirmed. | accept — Major: one registry line. |

Minors and nits accepted into the fix file: stale status notes on R3.12/R11.13 (PMM18-2, all lenses); the Legend's "Four P0 rows" header (PMM18-3, Test T18-5); the P2 tier with no rows (PM, Arch, Staff, Plan); R4.15's tally-label clause needs an affordance (Test T18-3); DEMO_SCAN_CYCLE as the one constant with no candidate anywhere (Staff S18-2); F40's fence body still names R11.9 (Staff S18-4, Plan); §10's "seven P0 obligations" unenumerated (Plan); the hand-off list should say the Data Foundation and Collection Mode PRDs are unwritten and name the second copy of the stale seam gate in docs/product/README.md (Plan B4); REORDER_SCOPE undeclared (Test T18-6); OQ id namespace rule (Interface); README index status and the OQ results file (Interface, PMM); "Duplicate code (adding an item)" full name (Interface); R11.15's table rows lettered and its collection cell quoting fixed labels (Interface); E24's headline at zero captured (Interface nit, noted). Routed to the engineering plan under F46 (4), not fixed: R1.3's mid-session interim rule (PM18-4), R4.17/OQ 14's announcement policy (Arch A18-3, Plan risk), R3.4's dependence on a device-PRD row that is P1 there (PM18-3 — carried as a clause on the obligations §5 line).

### Row flips

Of the 8 open rows: R4.4, R10.4 flip to 🤝; R3.12 and R11.13 flip once their stale notes are removed (their only objection); R4.15, R5.9, R8.1, R11.15 stay ⌛️ and are edited. Rows the lock pass edits under F47/F48 and the interface findings (R4.10, R8.4, R6.3, R7.19, R10.7, R5.10, R5.12, R5.14, M5, E18, E23 and any row whose status note is stripped) are re-verified in round 19, the final delta round before lock.

**Round-18 status:** priority pass answered and fenced (F47); pre-lock round run with the retargeted plan reviewer (verdict: proceed to lock) and the fresh interface lens. **Lock pass:** applied by `operator-agents:product-manager` on Opus (10 items + FX18-0). After the pass: PRD 19,396 words; 150 R + 11 M rows; every status cell a bare Legend value; 147 🤝, 14 ⌛️ (R4.10 R4.15 R4.21 R5.9 R5.10 R5.11 R5.14 R6.12 R8.1 R8.4 R10.7 R11.12 R11.15 M5) plus E18 in the copy file; lock checks clean (no placeholders, no template comments, OQ results complete, citations and anchors resolve). Orchestrator hygiene edit: the retired counter name removed from one UJ3.1 diagram node in the journeys file. Operator notes: the obligations §5 line marks two clauses as amendments (current item; resume is a new session start); §10's "seven" obligations were eight rows and are now cited by ID. Next: round 19, the final delta verification over the 15 edited rows before lock. Owner instruction: no PR until the owner has reviewed the final artifact set.

## Round 19 — final delta verification before lock (2026-09-07)

**Subject:** the round-18 lock-pass commit (PRD 19,396 words; 150 R + 11 M; 147 🤝, 14 ⌛️; copy 43/44). **Lenses:** the five usual lenses plus the interface lens on Claude/Opus, each verifying its round-18 findings and the 14 edited rows and E18; agy on four, advisory (PM rc 0 with a Blocker on R5.9's wording vs R4.10/R5.14 — the same operator-agency wording the architecture and interface lenses raised as Minors, folded into FX19-6 — and an objection on M5's F30 citation, folded into FX19-8; the other three briefs returned short bodies).

| lens | route | round-18 items | new findings | rows OBJECT |
|---|---|---|---|---|
| product manager | Claude/Opus | all RESOLVED / BY-FENCE | 2 Major, 2 Minor, 1 Nit | R8.4 |
| staff engineer | Claude/Opus | all RESOLVED / BY-FENCE; R18-F5 PARTIAL (E28/E29 home) | 3 Major, 2 Minor, 2 Nit, 5 questions | R5.10, E18 |
| test (retargeted) | Claude/Opus | all RESOLVED / BY-FENCE | 1 Major, 4 Minor, 2 Nit | R11.12 |
| architecture | Claude/Opus | all RESOLVED / BY-FENCE | 1 Major, 3 Minor, 2 Nit | R5.9, R5.10, R11.15 |
| marketing (copy) | Claude/Opus | all RESOLVED / BY-FENCE | 2 Major, 2 Minor, 2 Nit | none |
| interface (fresh lens, delta) | Claude/Opus | I18-1..I18-9 and Minors RESOLVED | 1 Major, 2 Minor, 1 Nit | copy table (11 states), R11.15 |

Every lens consents to lock every edited row that it did not object to; no lens objects to a 🤝 row inside the PRD. The remaining findings are consistency repairs, most of them outside the requirement rows.

### Verify-the-reviewer dispositions (Majors)

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R19-F1 | Traceability still says "F1–F46" and "the rows do not cite them" while F47/F48 exist and M2/M3/M5/M9 cite F30 | Arch A19-1, PMM PMM19-2 (Major), Test, Staff nits | `:105` confirmed. | accept. |
| R19-F2 | The journeys file's UJ3.1 diagram still offers "Skip" for a disagreeing set where R4.10/E18 now say "Set it aside"; two plurals of "counters" survive F48 | PMM PMM19-1, PM PM19-2, Staff S19-3 (Major), Arch A19-4, Test | journeys `:271`, `:274`, `:42`, `:262` confirmed. | accept. |
| R19-F3 | The merged counter's event list covers only transitions into set-aside, so R8.4's "the guard applies here exactly as in the queue" has no event in the review; R8.4's "moves on only after…" omits a clean capture and "Accept the average"; its "Leave it set aside" is not tied to R8.5 | PM PM19-1 (Major), Test, Interface | `:339`, `:253` confirmed. | accept. |
| R19-F4 | E18's review variant offers "Leave it set aside" on a settled row re-scanned from the list, which F42 forbids | Staff S19-2 (Major) | copy `:29`, `:340`, F42 confirmed. | accept — the action is offered only while the row is unsettled (plain consequence of F42). |
| R19-F5 | F47's phasing lives only in the Legend: the copy states whose only action is P1 carry no phase mark, R11.15's list has no per-phase clause, and some bodies name the withheld action | Interface NEW-1 (Major), Arch A19-3, PM PM19-4 | copy file, `:441-448` confirmed; device PRD `:412` sets the precedent ("render with guidance but no action"). | accept — a ‹P1› mark on the affected action cells, one sentence in the copy file's header and in R11.15's lead row; bodies stay as guidance per the device PRD's convention. |
| R19-F6 | R5.10 says a test can "read back the counter untouched (R11.11)" but R11.11's read-back list has no counter | Staff S19-1 (Major), Test | `:255`, `:427` confirmed. | accept — restate behaviourally (no pause). |
| R19-F7 | R11.12's conditional-line list has E39's samples-let-go line but not E23's (the round-18 Blocker's own line) | Test T19-1 (Major), Staff, Interface | `:428` confirmed. | accept. |

Minors accepted: R5.10 gains "a disagreeing set the operator sets aside is not one of these and counts (R5.9)" and R5.9's route reads "a disagreeing set the operator set aside" (Arch A19-2, Interface, agy); E19's body names the three routes without attributing all to the instrument (PMM19-4); the Surfaces column heading says it lists a surface's flow including its entry offer and cancel notice, and E28/E29 are homed where they render (Staff S19-4, Test, PM nit); M5 cites F47 for its phase clause (PMM, Staff); the companion line names the OQ results file (PMM); F18/F37 bodies and F40's clarification gain "(superseded by F48)" markers (Staff, Arch, Interface nits); R4.10 "as the first one did" (Test nit).

### Row flips

Of the 14 ⌛️ rows and E18: **R4.10, R4.15, R4.21, R5.11, R5.14, R6.12, R8.1, R10.7 flip to 🤝**; **R5.9, R5.10, R8.4, R11.12, R11.15, M5, E18 stay ⌛️** and are edited by the round-19 fix pass, together with the 🤝 rows E19 and the Legend/Traceability/Surfaces paragraphs. Round 20 verifies those edits only, then the document locks.

**Round-19 status:** lock consented on every unedited row. **Fix pass:** applied by `operator-agents:product-manager` on Opus (9 items + FX19-0). After the pass: PRD 19,502 words; 155 🤝, 6 ⌛️ (R5.9 R5.10 R8.4 R11.12 R11.15 M5); the copy file 32 🤝, 12 ⌛️ (E18, E19, and the ten states that gained a ‹P1› mark under F47: E2 E28 E29 E30 E31 E32 E33 E36 E37 E42); the journeys diagram relabelled and checked; R4.10 flipped though FX19-8 added four words to it. Next: round 20, a final delta over exactly those 18 rows, then lock. Owner instruction stands: no PR until the owner has reviewed the artifact set.

## Round 20 — last delta before lock (2026-09-07)

**Subject:** the round-19 commit (PRD 19,502 words; 155 🤝, 6 ⌛️; copy 32/44). **Lenses:** the five usual lenses plus the interface lens on Claude/Opus over the 18 edited rows; agy on four, advisory.

| lens | round-19 items | new findings | rows OBJECT |
|---|---|---|---|
| product manager | all RESOLVED | 1 Major, 2 Nit | R8.4 |
| staff engineer | all RESOLVED | 1 Blocker, 2 Major, 2 Minor, 2 Nit | R5.9, R8.4 |
| test (retargeted) | all RESOLVED / PARTIAL on R8.4 and the diagram | 2 Major, 2 Minor | R5.9, R8.4 |
| architecture | A19-1 PARTIAL (fence file), rest RESOLVED | 1 Major, 3 Minor, 1 Nit | R8.4 |
| marketing (copy) | all RESOLVED; PMM19-4 PARTIAL | 2 Major, 1 Minor, 1 Nit | R8.4 |
| interface | NEW-1, Minors, Nit RESOLVED | 2 Major, 2 Minor | copy header, E29–E34 |
| agy (advisory) | — | objections on R8.4 (= R20-F1) and E18 (the diagram item) | R8.4, E18 |

Every lens consents to lock every edited row except the two below; sixteen of the eighteen rows are consented by all six.

### Verify-the-reviewer dispositions

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R20-F1 | R8.4's new clause "or by 'Skip'" is unqualified and R5.9's list is closed "and nothing else" over queue set-asides, so the two rows disagree on whether and what the guard counts in the review | Staff S20-1 (Blocker), PM PM20-1, Test T20-1, Arch A20-1, PMM PMM20-1 (Major) | `:339`, `:253`, `:255`, F48 confirmed. | accept — Blocker. R8.4 counts "on the same routes as a queue row (R5.9)" without restating them; R5.9's closing clause reads "and nothing else, in the queue and in the review (R8.4)"; §11's script gains the review case. |
| R20-F2 | The UJ3.1 diagram's relabelled "Set it aside" edge runs to the next row and bypasses the guard node, while R5.9 counts that route | PMM PMM20-2, Test T20-2, Staff S20-2 (Major) | journeys `:274`, `:261` confirmed. | accept — route the edge to the deferred-with-cause node; the guard node's label names R5.9's routes; "re-take" → "Take it again". |
| R20-F3 | The ‹P1› notation is action-level but E30–E34 have no P0 existence (their only rows are P1); E32 marks "None"; E34 is unmarked; E29's "Check it" belongs to the QC PRD | Interface I20-1, I20-2 (Major), Staff S20-4, Arch A20-4/A20-5, Test note | copy `:40-45` confirmed. | accept — a state-level ‹P1› on E30–E34 with one header clause ("a state or surface whose own rows are all P1 does not appear until they land"); E32's "None" unmarked; E29 marks "Correct it" and notes "Check it" as the QC PRD's; R11.15g marks its two P1 entry points and the lead row covers surfaces d/e. |
| R20-F4 | E22's (and E23's) ⟨dropped⟩ clause is joined to a non-count clause that the drop-whole rule would take at zero, losing the resume row | Staff S20-3 (Major), Arch A20-3 | copy `:33`, `:34`, `:474` confirmed. | accept — split each into two sentences; no wording changes. |

Minors accepted: E19 "or you set one aside because its samples wouldn't agree" (PMM, Test); R11.12 adds E2's finished-variant set-aside line (Test, Arch, PM nit); the fence file's map for F47 and F48 lists the rows and states that now carry them and its :358 sentence mirrors the PRD's (Arch, Staff, Interface); Traceability "amendments and clarifications" (Staff nit).

### Row flips

Consented by every lens: R5.10, R11.12, R11.15, M5, and E2, E18, E19, E28, E29, E30, E31, E32, E33, E36, E37, E42 → 🤝 (E19, E22, E23, E29–E34, R11.12, R11.15 are then edited by the round-20 fix and re-verified in round 21). Held: R5.9, R8.4. Round 21 is a short delta by the staff, test, architecture, and interface lenses over R5.9, R8.4, the edited copy rows, R11.12, R11.15, and the diagram; then the document locks.

**Round-20 status:** lock consented on 16 of 18 rows. **Fix pass:** applied by `operator-agents:product-manager` on Opus (6 items + FX20-0). After the pass: PRD 19,555 words; 157 🤝, 4 ⌛️ (R5.9 R8.4 R11.12 R11.15); copy 35 🤝, 9 ⌛️ (E19 E22 E23 E29 E30 E31 E32 E33 E34). Orchestrator hygiene edit: the state-level ‹P1› mark moved from the ID cell to the state-name cell on E30–E34 so ID-scanning scripts still see `| E30 |`; the header sentence reads "A state whose name is marked ‹P1›". Next: round 21, a short delta by the staff, test, architecture, and interface lenses over the 13 edited rows and the UJ3.1 diagram; then lock. Owner instruction stands: no PR until the owner has reviewed the artifact set.

## Round 21 — delta on the round-20 edits (2026-09-07)

**Subject:** the round-20 commit (PRD 19,555 words; 157 🤝, 4 ⌛️; copy 35/44). **Lenses:** staff engineer, test (retargeted), architecture, and interface on Claude/Opus over R5.9, R8.4, R11.12, R11.15, E19, E22, E23, E29–E34, and the UJ3.1 diagram. The product-manager and marketing lenses did not run this round: every round-20 item they raised was verified RESOLVED by the four that did, and no copy wording changed beyond the two sentence splits they had asked for. agy did not run (advisory; rc 8 on every prior round).

| lens | round-20 items | new findings | rows OBJECT |
|---|---|---|---|
| staff engineer | all RESOLVED | 1 Minor (post-lock), 1 Nit | none — "ready, proceed to lock" |
| test (retargeted) | all RESOLVED | 2 Major, 1 Minor | R8.4, R11.12 |
| architecture | all RESOLVED; A20-5 PARTIAL (E29) | 1 Major, 1 Minor, 1 Nit | E29 |
| interface | all RESOLVED | 2 Minor, 1 Nit | none — "contract sound" |

### Verify-the-reviewer dispositions

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R21-F1 | §11's script step 4 covers the review's K_FAILED_ATTEMPTS and "Skip" routes but not its third route — a set that disagrees again and is left set aside — which R5.9 counts | Test T21-1 (Major) | `:411` confirmed: step 4 names two routes; `:253` names three. | accept — one clause added to step 4. |
| R21-F2 | R11.12 asserts E33's variants and E18/E42's re-scan variants, all P1-only, with no per-phase clause, while R11.15 gained one in round 20; F47's map omits R11.12 | Test T21-2, Arch A21-2 (Major / Minor) | `:429`, `:436`, fences `:369` confirmed. | accept — the clause is folded into R11.12's first sentence (F45 holds at two); F47's map adds R11.12. |
| R21-F3 | E29's state name is unmarked although no row in this PRD produces it (its only reference is the QC & Comparison obligations line; the P1 re-scan path goes through E28), so the copy header asserts it appears in the first build with zero live actions | Arch A21-1 (Major); the residue of A20-5 | copy `:40`, `:4`; PRD `:460`, `:351` confirmed — no R row cites E29. | accept — the name cell carries the handing-over PRD and the header gains one clause; no new notation beyond the mark already in use. |

Minors accepted: R5.9's lead gloss "rows the instrument set aside by any route" contradicts its own operator route → "rows set aside by any of these routes" (Test, Arch nit); E30/E31/E33 drop the redundant action-level ‹P1› marks (Interface); F47's map enumerates E29–E34 (Interface nit). Declined: the interface lens's optional Legend clause — redundant with the copy header and R11.15's lead row, and F45 says never expand. Post-lock list: whether N_CONSEC_HARD carries across consecutive one-row sessions in a look-through review (Staff, Minor).

### Row flips

Consented by every lens and untouched by the fix: R11.15, E19, E22, E23, E32, E34 → 🤝. Held for round 22 (edited by the fix): R5.9, R8.4, R11.12, E29, E30, E31, E33. Round 22 is a delta by the test, architecture, and interface lenses over those seven rows, §11's script, the copy header, and F47's map; then the document locks.

**Round-21 status:** lock consented on 6 of 13 rows; 7 edited. **Fix pass:** applied by `operator-agents:product-manager` on Opus (5 items + FX21-0). After the pass: PRD 19,585 words; 158 🤝, 3 ⌛️ (R5.9 R8.4 R11.12); copy 40 🤝, 4 ⌛️ (E29 E30 E31 E33). Checks clean (anchors, citations, no `&amp;`, six mermaid blocks, two-sentence rule). Post-lock list adds: the device PRD's literal `&amp;` §7 heading breaks two cross-file links.

## Round 22 — delta on the round-21 edits (2026-09-07)

**Subject:** the round-21 commit `2cf2b4f` (PRD 19,585 words; 158 🤝, 3 ⌛️; copy 40/44). **Lenses:** test (retargeted), architecture, and interface on Claude/Opus over R5.9, R8.4, R11.12, E29, E30, E31, E33, §11's script step 4, the copy header's E29 clause, and F47's map. The staff lens consented to lock in round 21 with no open item; PM and marketing as in round 21. agy did not run.

| lens | round-21 items | new findings | rows OBJECT |
|---|---|---|---|
| test (retargeted) | T21-1 PARTIAL, rest RESOLVED, F47 map PARTIAL | 3 Major, 2 Minor, 1 Nit | R5.9, R8.4 |
| architecture | all RESOLVED, F47 map PARTIAL | 2 Minor, 2 Nit (outside the row set) | none |
| interface | all RESOLVED, F47 map PARTIAL | 4 Minor, 1 Nit | none |

### Verify-the-reviewer dispositions

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R22-F1 | §11 step 4 exercises two of R5.9's three routes — "Skip" after a failed attempt is walked nowhere — and its new disagreeing-set clause has no negative twin, so a build that pauses on any two "Leave it set aside"s passes | Test T22-1, T22-2 (Major); Arch A22-2 (Minor: "two" pins the open constant) | `:411` confirmed: positives name K_FAILED_ATTEMPTS and the disagreeing set only; the sole negative is "Skip" with no attempt. | accept — step 4 asserts each of R5.9's three routes and both exclusions, against N_CONSEC_HARD rather than "two". |
| R22-F2 | No Demo Device control produces a disagreeing set: the device PRD's settable surface (`prd-device-management.md:463`) covers device state, not reading content; R11.8 fixes only a reading's shape; the parity-gate obligation (`:462`) carries the twin only generically | Test T22-3 (Major) | Confirmed by grep: no row lets a test set a generated set's spread; R4.23 promises both paths are walked without hardware. Same shape as R8-F6, fixed by R4.26. | accept — one clause on R11.8 (a test sets a generated set's spread against SAMPLE_TOLERANCE), which also makes R4.9's boundary producible. R11.8 re-opens. |
| R22-F3 | FX21-4's enumeration "E29–E34" is narrower than the "its marked states" it replaced: E2, E18, E28, E36, E37, E42 carry action-level ‹P1› marks under F47 | Arch A22-1, Interface I22-1, Test T22-4 (Minor) | copy file grep: twelve marked states. | accept — the map names the state-level and the action-level sets. |

Minors accepted: F48's Decision gloss "the instrument set aside" amended to match R5.9 (Interface I22-2); the copy preamble's "every promise is backed by a requirement row" gains the E29 exception (Interface I22-3); the Surfaces table's three "QC or correction" cells carry the copy file's ‹QC & Comparison PRD› mark (Test T22-5, Arch nit); R11.12 "[E18]'s and [E42]'s re-scan" (Interface nit); the Legend's per-phase list adds R11.12 (Arch nit). Post-lock list: the review's "Leave it set aside" with no attempt as an engineering-plan test-matrix item; E29's adoption, not re-authoring, by the QC & Comparison PRD.

### Row flips

Consented by every lens and untouched by the fix: E29, E30, E31, E33 → 🤝. Held for round 23 (test OBJECT or edited): R5.9, R8.4, R11.8, R11.12. Round 23 is a delta by the test, architecture, and interface lenses over those four rows, §11's step 4, the Surfaces cells, the Legend list, the copy preamble, and F47/F48's lines; then the document locks.

**Round-22 status:** lock consented on 5 of 7 rows; R5.9/R8.4 held by the test lens; R11.8 re-opened. **Fix pass:** applied by `operator-agents:product-manager` on Opus (8 items + FX22-0). After the pass: PRD 19,614 words; 157 🤝, 4 ⌛️ (R5.9 R8.4 R11.8 R11.12); copy 44 🤝, 0 ⌛️. Checks clean (anchors, citations, no `&amp;`, six mermaid blocks, two-sentence rule).

## Round 23 — delta on the round-22 edits (2026-09-07)

**Subject:** the round-22 commit `8313c2e` (PRD 19,614 words; 157 🤝, 4 ⌛️; copy 44/44). **Lenses:** test (retargeted), architecture, and interface on Claude/Opus over R5.9, R8.4, R11.8, R11.12, §11's step 4, the Surfaces cells, the Legend list, the copy preamble, and F47/F48's lines. agy did not run.

| lens | round-22 items | new findings | rows OBJECT |
|---|---|---|---|
| test (retargeted) | RESOLVED except T22-2 and I22-2 PARTIAL | 1 Major, 3 Minor, 1 Nit | R5.9, R8.4 (by proxy: their text is correct; §11 step 4 cannot run route 3) |
| architecture | RESOLVED except T22-1 PARTIAL (quantifier) | 1 Minor, 3 Nit | none — "sound, lock it" |
| interface | all RESOLVED | 2 Minor, 4 Nit, all post-lock | none — "contract sound" |

### Verify-the-reviewer dispositions

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R23-F1 | §11 step 4's third route (a disagreeing set the operator set aside) needs the agreement check enabled, and the walk names only the guard's setting; R4.23 makes the check a separate setting, record-only through dogfood, so a default build cannot produce the route and the step ticks green with it unwalked. Separately, "N_CONSEC_HARD rows … by each of three routes" admits a one-mixed-run reading at N = 2 | Test T23-1 (Major); Arch A23-1 (Minor) | `:411`, `:218` (R4.23), OQ 3 `:505` confirmed. | accept — step 4 opens "with the agreement check enabled ([R4.23])" and reads "each route on its own run". Rows unchanged. |
| R23-F2 | F48's route-3 gloss "a set that disagreed" is broader than R5.9's "a disagreeing set the operator set aside", under a parenthetical asserting parity | Test T23 Minor | fences `:308` confirmed. | accept — the gloss names the set-aside. |
| R23-F3 | F47's map buckets E29 as a state-level ‹P1› mark, but its state mark is the handing-over mark and its F47 carrier is the action-level "Correct it ‹P1›" | Arch A23-2, Interface nit, Test nit | fences `:369`, copy `:40` confirmed. | accept — the map names the ‹P1› state set (E30–E34), E29's handing-over mark, and E29 in the action-level set. |

Everything else raised this round is post-lock and listed in the round-23 fix file: R11.8's verb (Interface Minor; Architecture and Test read "against" as R4.9's term and praised the clause, so it stays), F47's Decision marker, the Surfaces table's ‹P1› symmetry, the simulated-layer obligation cell, OQ 18's pre-F42 phrasing, the Legend's serial comma, and four engineering-plan test-matrix items.

### Row flips

Consented by every lens: R11.8, R11.12 → 🤝. Held: R5.9, R8.4 (test OBJECT by proxy of step 4). Round 24 is a delta by the test and architecture lenses over step 4, F47's and F48's lines, and those two rows; then the document locks.

**Round-23 status:** lock consented on 2 of 4 rows; R5.9/R8.4 held by proxy of §11 step 4. **Fix pass:** applied by `operator-agents:product-manager` on Opus (3 items + FX23-0). After the pass: PRD 19,622 words; 159 🤝, 2 ⌛️ (R5.9 R8.4); copy 44/44. Checks clean.

## Round 24 — last delta (2026-09-07)

**Subject:** the round-23 commit `0c9b23f` (PRD 19,622 words; 159 🤝, 2 ⌛️; copy 44/44). **Lenses:** test (retargeted) and architecture on Claude/Opus over R5.9, R8.4, §11's step 4, and F47's and F48's lines. agy did not run.

| lens | round-23 items | new findings | rows OBJECT |
|---|---|---|---|
| test (retargeted) | all RESOLVED | 1 Minor (post-lock), 2 Nit | none — "tests trustworthy" |
| architecture | all RESOLVED; A23-2 PARTIAL as a class (E42) | 1 Nit | none — "sound, lock it" |

No Blocker or Major. Both lenses ALIGN on R5.9 and R8.4; the proxy objection is discharged — a contributor can run §11 step 4 on a default Demo Device build, and it fails both naive guards. Post-lock items added to the round-23 fix file's list: F47's map files E18's and E42's marks as action-level where they are body-variant marks; UJ 3.9 step 4's gloss omits the agreement-check setting; step 4's negatives inherit N_CONSEC_HARD by ellipsis.

### Row flips

R5.9, R8.4 → 🤝 (unanimous ALIGN; orchestrator flip recorded here).

## Lock record (2026-09-07)

- **Every row aligned:** 161 R and M rows 🤝 (150 requirement rows, R11.15's eight lettered rows, R8.1's eleven lettered cells are cells of R8.1, 11 M rows); 44 of 44 copy rows 🤝. No owner overrule stands on any row.
- **Mandatory pre-lock round:** round 18 ran the retargeted `peer-plan-reviewer` ("proceed to lock") plus the fresh interface lens; rounds 19–24 were deltas on the edits those rounds produced.
- **OQ contract:** every answered open question has a results section (OQ 15, OQ 18, OQ 10 and 17 folded into OQ 8); the 21 open ones carry an interim rule and a closer in the table.
- **Zero unresolved placeholders; template guidance comments deleted** (checked across the PRD, the copy file, and the journeys file).
- **Mechanical checks:** anchors and citations resolve (the device PRD's literal `&amp;` §7 heading is the one known miss, on the post-lock list); no literal `&amp;` in the capture files; six mermaid blocks render; every requirement row at most two sentences (F45).
- **Fences:** F1–F48 with clarifications; fence → row map in the fence file.
- **Post-lock list:** in `prd-capture-mode-round-23-fixes.md`.
- **Next:** owner review of the artifact set. No PR until the owner says so. The PRD header's `Status:` line is the owner's to flip.

## Round 25 — F49 split verification (2026-09-08)

**Subject:** commit `c45570e` — inventory import split out of the locked capture PRD into `docs/product/import/` (five files) after the directory move at `36ef9f4`. **Lenses:** staff engineer, interface, and test (retargeted) on Claude/Opus over both PRDs and all companions. Each lens independently diffed the moved rows against the pre-split tree: 14 rows, 13 copy states, 3 journeys, M7, OQ 12, F4/F11 all arrived intact and exactly once, byte-identical apart from citation targets.

| lens | new findings | rows OBJECT |
|---|---|---|
| test (retargeted) | 1 Blocker, 1 Major, 2 Minor, 3 Nit | R4.1, M8, OQ 13, the capture Legend/obligations paragraphs |
| staff engineer | 1 Major, 5 Minor, 1 Nit | R4.1, R11.15, the capture Legend/obligations paragraphs |
| interface | 5 Major, 3 Minor, 1 Nit | R4.1, R11.15, the capture Legend/Traceability/obligations paragraphs |

### Verify-the-reviewer dispositions

| # | finding | raised-by | verify | disposition |
|---|---|---|---|---|
| R25-F1 | The import PRD's copy pointer and copy header cite R4.1 for "a named state whose identity is stable… asserted independently of copy"; pre-split that sentence cited R11.12, which stayed in capture, so no row now obliges a test to observe which of the thirteen import states is up | Test (Blocker), Staff (Major), Interface (Major) | import `:174`, copy `:8` cite R4.1; `84e9964` cited R11.12; R4.1 states only the preview rule. | accept — restore the rule as import R4.2 (a carried obligation, not a new rule); both sentences cite it. |
| R25-F2 | Cross-PRD row cites are bare IDs (`[R3.1]`, `[R3.2]`, `[R1.2]`) that also name live capture rows; the Data Foundation obligations cell and OQ 13's Feeds read coherently and wrongly | Test, Interface (Major) | eleven bare cross-PRD labels in capture, two in import; capture has live R1.2, R3.1, R3.2. | accept — labels name the owning PRD; both Legends state the rule. |
| R25-F3 | M7 retired nowhere; F49's retired list omits M7 and R11.15h; Traceability still declares R11.15a–h; Legend line 78 still lists capture's OQ 12 | Interface (Major ×3), Staff (Minor ×2) | `:81`, fences `:374`, `:103`, `:78` confirmed. | accept — all four. |

Minors accepted: "F1–F49"; F4/F11 map wording "copied"; the duplicated engineering-plan entry; the Legend's P0 list; the Labels "this table" clause; R3.12 on the obligations line; obligations owned once and the added Collection Mode clause dropped; README use-case ownership and authoring-order text; the import Vocabulary's unused borrowed terms. Post-lock: §4's UJ 3.9 trace; the Swatch field terms; the device PRD's `&amp;` headings.

### Row flips

All rows in both PRDs hold 🤝 except import R4.1 and the new R4.2, held for round 26 (staff, interface, test over R4.1, R4.2, the cross-PRD cites, and the edited Legend/Traceability/obligations/README paragraphs).
