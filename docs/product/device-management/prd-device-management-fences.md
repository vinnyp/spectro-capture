# Device Management PRD — owner decisions (fences)

Owner decisions on this PRD. A fence is settled: reviewers do not re-litigate it, and a row that names one does so for provenance only. F1–F8 are the load-bearing decisions from the review arc that locked the document on 2026-09-05 (PR #8), recorded here from the arc's decision record; the rows themselves carry the full set of that arc's adjudications. F9 is the refactor that produced this file. Review log for rounds from F9 on: `../../agent-reviews/2026-09-08-prd-device-management-refactor-peer-reviews.md`.

### F1 — Calibration is strictly pre-flight (2026-09-05, review arc)

**Decision:** The research-prescribed mid-run drift halt is cut; calibration is checked before a session, never during one. Owner override of the research.

### F2 — Explicit resume is the only path out of a halt (2026-09-05, review arc)

**Decision:** Recovery never auto-resumes; every halt ends with the operator resuming.

### F3 — Hold-and-retry on a failed save (2026-09-05, review arc)

**Decision:** A failed save holds and retries; the durability claim is scoped to every scan the queue advanced past.

### F4 — Partial multi-sample sets are discarded on halt (2026-09-05, review arc)

**Decision:** A halt discards the part-finished set; the item re-scans.

### F5 — Sessions run to completion across offline-window expiry (2026-09-05, review arc)

**Decision:** An offline authorization window expiring mid-session does not end the session; whether the SDK enforces otherwise is a spike question.

### F6 — Opt-in telemetry, off by default (2026-09-05, review arc)

**Decision:** A direction change over the research's no-network-path recommendation: telemetry is opt-in, off by default, forks ship their own provider ID; a dedicated telemetry PRD is queued behind a provider spike. STRATEGY.md amended.

### F7 — Every numeric threshold is a named TBD-on-spike constant (2026-09-05, review arc)

**Decision:** No provisional number ships in a row; each constant carries its candidate value and its open-question id.

**Clarification (1), 2026-09-08 (round 1 of the F9 refactor):** A dogfood build is not a release for this rule, so the first sessions run on candidate values and produce the halt-log data the thresholds' closers need; the Legend reads "carrying its candidate value where one exists, and its Open Questions id". Where a constant has no candidate, the engineering plan sets a dogfood value, which the OQ's closer then replaces. Mirrors the capture PRD's fence F23.

### F8 — Priority is build order within v1, not scope (2026-09-05, review arc)

**Decision:** P0 is the pre-spike build phase (Demo Device, test seams, the capture/halt/resume core, the pre-flight gate, the device-identity model); P1 is the post-spike hardware wave; P2 last.

**Clarification (1), 2026-09-08 (round 2):** R6.27, the simulated device's configurable latency, is P0: the capture PRD's locked P0 row R11.3 already places it in the first build phase, and the two documents agree rather than the device PRD carrying a conditional.

### F9 — The refactor: concise rows, companion files, row IDs (2026-09-08)

**Decision:** The document takes the capture-mode PRD's shape. (1) Every requirement row is at most two sentences; the table shrinks and never expands; rationale and citations move out; a row splits only where two rules genuinely differ, and the new row takes the next ID in its section. (2) The journeys and every diagram move to `prd-device-management-journeys.md` (non-normative); §7's copy moves to `prd-device-management-copy.md`; answered and residual open questions get their evidence in `prd-device-management-oq-results.md` and the table takes the capture PRD's seven columns. (3) Every requirement row gains an ID `R<section>.<n>` in document order, every copy state `E<n>` in table order, every metric `M<n>`; IDs are assigned once and never renumbered. (4) The literal `&amp;` in headings and text becomes `&`; anchors are unchanged by that. (5) No rule changes: every row keeps its 🤝 status through the pass and is verified by one round (product manager, staff engineer, test, interface), then the document re-locks.

**Why:** The owner's standing instruction for every PRD: "Simple, concise, designed for agents"; and the capture and import PRDs already cite this document by section, journey, and copy state, so one shape across the three keeps those cites mechanical.

**Clarification (1), 2026-09-08 (round 1):** (a) The pairing and calibration retry counts get one open question, OQ 28, cited from R1.8 and R3.5. (b) R6.9's settable state gains "system audio output muted / not muted" so R4.1's advisory is testable without hardware; the one rule addition of this refactor, on the model of the capture PRD's R4.26. (c) R2.14 keeps "forks ship their own provider ID" — fence F6's text, promoted from OQ 13; not drift. (d) The copy file takes the capture copy file's conventions: a Status cell per row and ‹P1› marks on states and actions whose only citing rows are P1.

**Clarification (2), 2026-09-08 (round 2):** (a) OQ 28 carries "candidate 3 consecutive failures". (b) §1 gains R1.23 (P0): a pairing attempt that fails shows E12 with a retry action — the one copy state no row produced. (c) E18's body drops "— you're online, so you can do it right now"; the sentence stands as guidance in the first build. (d) R6.9's audio state is three-valued (muted / not muted / not determinable), as its calibration-due sibling is, and §6's shell-side seam list names system audio output. (e) E3's "Leave setup for now" action carries the ‹P1› mark, since only R1.8 and R3.5 (P1) define it.

**Clarification (3), 2026-09-08 (round 3):** (a) E12's and E14's bodies close with E13's own exit sentence, "You can come back to this any time from the device panel.", so the first build's pairing and calibration loops carry their exit as guidance until R1.8 and R3.5 land. (b) R2.12 (P0) renews the window silently whenever connectivity exists while either E17 or E18 is shown, so the first build is not bounded by the authorization window; R2.10 and R2.11 stay P1. (c) R1.8's exit generalizes: any first-run setup state offers "Leave setup for now", landing on the device panel; E3 keeps the action, marked ‹P1›. (d) R4.4's round-2 rewording ("a one-tap way forward — re-check after…") is recorded here for provenance. (e) OQ 28's closer, like OQ 5's, is revisited after lock: no row records setup-failure counts yet.

**Clarification (4), 2026-09-08 (round 4):** (a) First-run setup states are non-modal: they render on the device panel, which stays reachable, so leaving one never needs an action; the Surfaces paragraph says so. (b) R1.8's exit is offered from the states that block first-run progress — device authorization (E3), pairing (E12, E13), calibration (E14, E15) — and on first run it lands on the device panel, as R3.5 says; the license states keep no action. (c) R2.12 renews silently whenever connectivity is present, including the moment it returns; E17 shows while offline and E18 while online; E18 gains E17's unmarked "Check again" so a failed or unattempted renewal has a first-build control. (d) E3's body carries the same exit sentence as E12 and E14.

**Clarification (5), 2026-09-08 (round 5, recorded by the orchestrator as precision of (4a)–(4c); the owner may overrule):** (a) Non-modality is a rule: R1.23 (P0) carries it — first-run setup states render non-modally on the device panel or in the device picker, both of which stay reachable — and §6 gains the observation that a test can assert the panel's other affordances stay operable while a setup state shows. (b) E12 and E14 carry "Leave setup for now ‹P1›" in their action cells, as (4b) names them. (c) Leaving setup is the user navigating away from a setup state, by the "Leave setup for now" action or otherwise; both reset the consecutive-failure counter, and the action dismisses the state and returns the device panel to its normal content. (d) "First run" means the app has no saved device yet; the Legend says so. (e) R4.4 reads "with guidance and without that action", as the Legend, §7 and the copy header already do. (f) E18's body names its P0 control and carries E17's coverage hedge; R2.16 cites E17 and E18.

## Fence → row map

Where a fence is named in the PRD, for provenance only. A row not listed here cites no fence; F7, F8 and F9 bind every row by inheritance rather than by citation.

| Fence | Named by |
| :--- | :--- |
| F1 | [§3](prd-device-management.md#3-calibration)'s section note and [R3.4](prd-device-management.md#3-calibration) |
| F2 | [R5.7](prd-device-management.md#5-mid-session-device-failure) |
| F3 | [R5.8](prd-device-management.md#5-mid-session-device-failure), [R5.10](prd-device-management.md#5-mid-session-device-failure) |
| F4 | [R5.15](prd-device-management.md#5-mid-session-device-failure) |
| F5 | [R2.17](prd-device-management.md#2-licensing--pre-authorization); [OQ 11](prd-device-management.md#open-questions) |
| F6 | [R2.14](prd-device-management.md#2-licensing--pre-authorization); [OQ 12, OQ 13, OQ 14](prd-device-management.md#open-questions) |
| F7 | the [Legend](prd-device-management.md#legend)'s provisional-constants paragraph, which every TBD-on-spike constant's row inherits |
| F8 | the [Legend](prd-device-management.md#legend)'s Priority paragraph, which every row's Pri cell inherits |
| F9 | Scope: every row in [§1](prd-device-management.md#1-device-pairing)–[§6](prd-device-management.md#6-mock-device-layer), every [copy state](prd-device-management-copy.md#error--state-copy), every [metric](prd-device-management.md#success-metrics), and this document's [Legend](prd-device-management.md#legend) and [Traceability](prd-device-management.md#traceability) |

## Rejected findings

(none yet)
