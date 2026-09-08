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

### F8 — Priority is build order within v1, not scope (2026-09-05, review arc)

**Decision:** P0 is the pre-spike build phase (Demo Device, test seams, the capture/halt/resume core, the pre-flight gate, the device-identity model); P1 is the post-spike hardware wave; P2 last.

### F9 — The refactor: concise rows, companion files, row IDs (2026-09-08)

**Decision:** The document takes the capture-mode PRD's shape. (1) Every requirement row is at most two sentences; the table shrinks and never expands; rationale and citations move out; a row splits only where two rules genuinely differ, and the new row takes the next ID in its section. (2) The journeys and every diagram move to `prd-device-management-journeys.md` (non-normative); §7's copy moves to `prd-device-management-copy.md`; answered and residual open questions get their evidence in `prd-device-management-oq-results.md` and the table takes the capture PRD's seven columns. (3) Every requirement row gains an ID `R<section>.<n>` in document order, every copy state `E<n>` in table order, every metric `M<n>`; IDs are assigned once and never renumbered. (4) The literal `&amp;` in headings and text becomes `&`; anchors are unchanged by that. (5) No rule changes: every row keeps its 🤝 status through the pass and is verified by one round (product manager, staff engineer, test, interface), then the document re-locks.

**Why:** The owner's standing instruction for every PRD: "Simple, concise, designed for agents"; and the capture and import PRDs already cite this document by section, journey, and copy state, so one shape across the three keeps those cites mechanical.

## Fence → row map

F1–F8: the rows that carry them are the record; this pass adds no provenance marks. F9: every row in §1–§6, every copy state, every metric.

## Rejected findings

(none yet)
