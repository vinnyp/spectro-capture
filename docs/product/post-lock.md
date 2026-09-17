# Post-lock list

Every PRD that locks records follow-on work at its lock: wording accepted as post-lock rather than fixed before lock, items for build review, questions the hardware spike answers, and amendments its sibling documents owe. This document holds all of it, grouped by the trigger that acts on it, and it is the only place these items live — the review logs' lock records point here instead of carrying their own lists. An item is ticked in the same PR that closes it, with the PR number or commit recorded beside the tick. A new PRD's lock appends its items here rather than to its review log.

**Sources**, as history only: the [Capture Mode lock record](../agent-reviews/2026-09-06-prd-capture-mode-peer-reviews.md), the [Device Management lock record](../agent-reviews/2026-09-08-prd-device-management-refactor-peer-reviews.md), the [Data Foundation and Data Export lock record](../agent-reviews/2026-09-09-prd-data-foundation-peer-reviews.md), and the post-lock amendment sections in the capture and device logs.

## Before or during the hardware spike

The spike runs from [`hardware-spike-brief.md`](../briefs/hardware-spike-brief.md) and its results file lands beside it. That brief's "Before the session" prerequisites are the gate — the throwaway probe harness, both instrument models, the two license variants, the vendor confirmation path for a per-serial refusal, a network-capture tool and a second SDK client, the calibration tile, and the time-box — and each is confirmed before the instrument is booked.

- [ ] **All five PRDs** — the spike runs and its results file lands beside the brief; each PRD's spike-scoped open questions close from that evidence.
- [x] **Device** — the spike-scope amendment lands before the spike is dispatched: the Legend's hardware-spike scope gains the reported wavelength grid, the raw-payload round-trip and the toolkit's spaces, the vendor analytics recipient, and the published reference set (plan PL7-4). — done, PR #13

## Landed sibling amendments

History, kept so no one re-raises them. Each locked PRD recorded amendments its siblings owed; all of them have landed.

- [x] **Capture** — the Data Foundation obligation row gains R1.10 and R4.5; R1.9 and R4.24 name Data Export beside Data Foundation; E29's Body gains the capture-time correction default; OQ 19 closes on fences F2 and F14; OQ 24's Closer records that closing re-opens the Data Foundation PRD's R6.5 inventory. — done, PR #14
- [x] **Device** — the hardware spike's scope; R6.5's `simulated` column named `sc_simulated` in the export PRD's terms; R6.20's declared-state harness gains the file-location axis; M2's start event spans choosing where the file lives; the outbound obligations lines; UJ1, UJ1.1 and UJ1.2 gain "Choose where the file lives" as step 2. — done, PR #13
- [x] **Import** — R2.2 gains that a later import keeps the existing columns' positions and appends its new ones after them; the outbound obligations table gains a Data Export line. — done, PR #14
- [x] **Vision and the product README** — "raw payload canonical" reads as the stored mean. — done, PR #14 (the vision) and PR #12 (the README)

## Next pass over a locked PRD

Document amendments still open. Each waits for the next editing pass over the PRD that owns it.

### Data Foundation

- [ ] **DF** — the obligations-map paragraph at DF `:236` ("which row here carries…") does not name the capture line's R1.10 and R4.5.
- [ ] **DF** — E29 arrives with the QC & Comparison PRD; until then DF E11 is the surface for the capture-time correction default.

### Device Management

Rows and copy:

- [ ] **Device** — R1.8's two landings (collection versus device panel on first run) read as complementary but not as one destination.
- [ ] **Device** — under non-modality the counter also resets on incidental navigation.
- [ ] **Device** — R4.4's "satisfied from P1" over-generalizes.
- [ ] **Device** — R1.23's "or in the device picker" clause binds nothing while Surfaces assigns first-run states to the panel, so E1's non-modality is unstated.
- [ ] **Device** — R6.17's affordance clause names no seam.
- [ ] **Device** — E18's body leads with the withheld control.
- [ ] **Device** — E17's "Check again" while still offline has no stated behaviour, and no row produces "Check again".
- [ ] **Device** — R2.12's "reconnect-once" shorthand collides with §5's device-reconnect vocabulary.
- [ ] **Device** — the copy header's loop-exit rule is written for a marked state where E3's case is a marked action.

Indexes and journeys:

- [ ] **Device** — the journeys' nine "copy, §7" labels.
- [ ] **Device** — the workflow diagram's single AuthBlocked state and its missing leave-setup edges from Pairing and Calibration.
- [ ] **Device** — E12's "Try again" versus the diagram's return to discovery.
- [ ] **Device** — R5.2's forward reference to ADR-0004.

Cross-document:

- [ ] **Device** — the capture PRD's two "moves from P1 into the first build phase" sentences (R11.3 and its obligations table) are stale now that R6.27 is P0, and ride that PRD's OQ 16 amendment.
- [ ] **Device** — OQ 5's and OQ 28's closers name dogfood data no row records.

### Capture Mode

- [ ] **Capture** — R11.8's "set … against" → "relative to"; contested, since two lenses read "against" as R4.9's term.
- [ ] **Capture** — F47's Decision lacks an "(R11.12 added rounds 21–22)" marker.
- [ ] **Capture** — the Surfaces table carries no ‹P1› on P1-only state names.
- [ ] **Capture** — the "Device PRD, simulated layer" obligation cell does not summarise R11.8's spread control.
- [ ] **Capture** — OQ 18's results phrasing predates F42.
- [ ] **Capture** — the Legend's serial comma.
- [ ] **Capture** — F47's map files E18's and E42's re-scan marks as action-level where they are body-variant marks; name the third mark kind once in the copy header.
- [ ] **Capture** — UJ 3.9 step 4's gloss omits the agreement-check setting.
- [ ] **Capture** — §11 step 4's negatives inherit N_CONSEC_HARD by ellipsis.

### Inventory Import

- [ ] **Import** — no row obliges a test to list the actions a non-preview import state offers (E40's three routes). Owner decision (a rule change).
- [ ] **Import** — §4's Traces line points at UJ 3.9.
- [ ] **Import** — the three Swatch field terms are defined in neither Vocabulary.
- [ ] **Import** — the journeys carry no blocked-import path for R3.2 and E40.

### Cross-document

- [x] **Capture** — the device PRD's literal `&amp;` §2 and §7 headings break three links. — done, the F9 refactor (commit `4b0f474`, PR #11): the headings read `&`, and the inbound `#2-licensing--pre-authorization` and `#7-error--state-copy` anchors resolve; the only `&amp;` left under `docs/product` is the fixed string quoted in the device fence file's F9
- [ ] **Capture** — the widened cross-PRD label grep should become a standing check.
- [ ] **DF** — DF R7.7's fixture list and DE's outbound mirror of it are two copies of one list, and will drift on the next fixture added.
- [ ] **DF** — whether a Collection Mode rename moves an imported column's stored name.
- [ ] **DF** — OQ 2's ADR-0006 gloss.

## First build PR

Build-review items and the engineering plan's test matrix.

- [ ] **DF** — a CSV → store → CSV round-trip case for a passthrough value.
- [ ] **DF** — a golden asserting `sc_simulated` false.
- [ ] **DF** — R7.6b listing the versions each file-opening state names.
- [ ] **DF** — R7.2's oracle naming a cleared imported value.
- [ ] **DF** — R7.6k asserting the delete counts on an item with no earlier readings.
- [ ] **DF** — E8's never-scanned rendering.
- [ ] **DE** — E1 at an empty collection.
- [ ] **DF** — the collision tie-break for a literal `import_`-prefixed passthrough column.
- [ ] **Capture** — "Leave it set aside" after a non-disagreeing failed attempt does not count.
- [ ] **Capture** — a mixed-route run counts.
- [ ] **Capture** — "Accept the average" and a captured row reset the counter.
- [ ] **Capture** — a run spanning the queue → review boundary.
- [ ] **Capture** — whether N_CONSEC_HARD carries across consecutive one-row look-through sessions.

## ADR-0003

- [ ] **DF** — the generated ROWS_CEILING corpus's determinism contract; M6, M8 and DE R4.1 read from it.
- [ ] **DF** — the regeneration job's progress record.
- [ ] **DF** — the per-invariant salvage rules.

## Documentation

- [ ] **DF** — the help-docs column dictionary carries a formula-injection note for spreadsheet consumers, and nothing yet gates a release on that dictionary existing.
- [ ] **DF** — the dogfood build's participant-facing onboarding says the interaction record is kept and a delete does not reach it.
- [ ] **DF** — a redacted bug-report artifact.
- [ ] **DF** — the remembered-mapping lifecycle.
- [ ] **DF** — the serial on the device authorization path.
- [ ] **DF** — the README's gamut gloss ("which most screens can't show accurately").

## v2 candidates

Deliberate omissions from v1, recorded so no one re-raises them as gaps. Not work.

- **DE** — field selection at export (serial, measured-at).
- **DE** — no collection-identifier column.
- **DE** — no rule about an export mid-session.
- **DE** — no destination-overwrite rule.

## Judgment calls that stood at lock

The owner may overrule any of these. Not work.

- **DF** — the salvage conflict resolution: the later-recorded reading stays current.
- **DF** — reflectance as a fraction of 1.
- **DF** — times in UTC with `Z`.
- **DF** — a path in both volume classes shows the network state.
- **DF** — the reflectance precision is fixed when the golden is re-cut.
- **DF** — E28–E30 lose the "way back" clause.
- **DF** — the ceiling corpus is generated rather than checked in.
- **DE** — R2.4 returns to fence F13's "exactly as stored", with the storage obligation on DF R1.2.
- **DF** — prose trims to fit the budgets (rounds 9 and 10).
