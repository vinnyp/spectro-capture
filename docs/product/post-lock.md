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

- [x] **Device / Export** — R6.5 and its export obligation define true/false from known snapshots, empty without one; Export F19 / PR #18.
- [x] **DF / Export** — R7.7n adds the empty-collection fixture; R7.7a/e/i cover header comparison variants, all three simulation values and payload-absence branches; both obligation directions updated under Export F19/F20/F23/F26 / PR #18.
- [x] **Capture / Export** — queue-order obligation points to Export R1.1g beside R2.3 in both directions; PR #18.

## Next pass over a locked PRD

Document amendments still open. Each waits for the next editing pass over the PRD that owns it.

### Data Foundation

- [ ] **DF** — M10: readable readings absent from the salvage output, target 0, method R7.3. Proposed metric deferred under PR #17, F49; no PRD metric row yet.

- [x] **DF** — the Capture obligation map names R1.10 → DF R3.1 and R4.5 → DF R2.1. — PR #17 agent-build amendment.
- [ ] **DF** — E29 arrives with the QC & Comparison PRD; until then DF E11 is the surface for the capture-time correction default.

- [ ] **DF / Data Export** — DF OQ 21 tracks a future distinct unavailable-archive export mark and version bump; interim empty payload cells and quarantine precedence are settled by PR #17, DF F37 / DE F14.

### Device Management

Rows and copy:

- [x] **Device** — R1.8's two landings (collection versus device panel on first run) read as complementary but not as one destination. — [PR #19](https://github.com/vinnyp/spectro-capture/pull/19), including review fixes under F15–F31.
- [x] **Device** — under non-modality the counter also resets on incidental navigation. — [PR #19](https://github.com/vinnyp/spectro-capture/pull/19), including review fixes under F15–F31.
- [x] **Device** — R4.4's "satisfied from P1" over-generalizes. — [PR #19](https://github.com/vinnyp/spectro-capture/pull/19), including review fixes under F15–F31.
- [x] **Device** — R1.23's "or in the device picker" clause binds nothing while Surfaces assigns first-run states to the panel, so E1's non-modality is unstated. — [PR #19](https://github.com/vinnyp/spectro-capture/pull/19), including review fixes under F15–F31.
- [x] **Device** — R6.17's affordance clause names no seam. — [PR #19](https://github.com/vinnyp/spectro-capture/pull/19), including review fixes under F15–F31.
- [x] **Device** — E18's body leads with the withheld control. — [PR #19](https://github.com/vinnyp/spectro-capture/pull/19), including review fixes under F15–F31.
- [x] **Device** — E17's offline Check again is R2.20; E19/E20 Check again and full recheck are R4.7/R4.2 (closed with these review fixes). — [PR #19](https://github.com/vinnyp/spectro-capture/pull/19), including review fixes under F15–F31.
- [x] **Device** — R2.12's "reconnect-once" shorthand collides with §5's device-reconnect vocabulary. — [PR #19](https://github.com/vinnyp/spectro-capture/pull/19), including review fixes under F15–F31.
- [x] **Device** — the copy header's loop-exit rule is written for a marked state where E3's case is a marked action. — [PR #19](https://github.com/vinnyp/spectro-capture/pull/19), including review fixes under F15–F31.

Indexes and journeys:

- [x] **Device** — the journeys' nine "copy, §7" labels. — [PR #19](https://github.com/vinnyp/spectro-capture/pull/19), including review fixes under F15–F31.
- [x] **Device** — the workflow diagram's single AuthBlocked state and its missing leave-setup edges from Pairing and Calibration. — [PR #19](https://github.com/vinnyp/spectro-capture/pull/19), including review fixes under F15–F31.
- [x] **Device** — E12's "Try again" versus the diagram's return to discovery. — [PR #19](https://github.com/vinnyp/spectro-capture/pull/19), including review fixes under F15–F31.
- [x] **Device** — R5.2's forward reference to ADR-0004. — [PR #19](https://github.com/vinnyp/spectro-capture/pull/19), including review fixes under F15–F31.

Cross-document:

- [x] **Device** — the capture PRD's two "moves from P1 into the first build phase" sentences (R11.3 and its obligations table) are stale now that R6.27 is P0, and ride that PRD's OQ 16 amendment. — [PR #19](https://github.com/vinnyp/spectro-capture/pull/19), including review fixes under F15–F31.
- [x] **Device** — OQ 5's and OQ 28's closers name dogfood data no row records. — [PR #19](https://github.com/vinnyp/spectro-capture/pull/19), including review fixes under F15–F31.

### Capture Mode

- [x] **Capture** — R11.8's "set … against" → "relative to"; contested, since two lenses read "against" as R4.9's term. — Capture agent-build amendment F51–F55 (PR pending).
- [x] **Capture** — F47's Decision lacks an "(R11.12 added rounds 21–22)" marker. — Capture agent-build amendment F51–F55 (PR pending).
- [x] **Capture** — the Surfaces table carries no ‹P1› on P1-only state names. — Capture agent-build amendment F51–F55 (PR pending).
- [x] **Capture** — the "Device PRD, simulated layer" obligation cell does not summarise R11.8's spread control. — Capture agent-build amendment F51–F55 (PR pending).
- [x] **Capture** — OQ 18's results phrasing predates F42. — Capture agent-build amendment F51–F55 (PR pending).
- [x] **Capture** — the Legend's serial comma. — Capture agent-build amendment F51–F55 (PR pending).
- [x] **Capture** — F47's map files E18's and E42's re-scan marks as action-level where they are body-variant marks; name the third mark kind once in the copy header. — Capture agent-build amendment F51–F55 (PR pending).
- [x] **Capture** — UJ 3.9 step 4's gloss omits the agreement-check setting. — Capture agent-build amendment F51–F55 (PR pending).
- [x] **Capture** — §11 step 4's negatives inherit N_CONSEC_HARD by ellipsis. — Capture agent-build amendment F51–F55 (PR pending).

### Inventory Import

- [x] **Import** — non-preview actions, including all three E40 routes, are enumerated by R4.1 and exercised in UJ 2.1; R3.8 owns transitions. Owner explicitly ratified the rule change in the [2026-09-17 PR #16 decisions](https://github.com/vinnyp/spectro-capture/pull/16#issuecomment-5723537817); transitions and action coverage completed in PR #16.
- [x] **Import** — §4 now links to its own UJ 2–2.2 acceptance scenarios — PR #16 (commit `cf9974f`).
- [x] **Import** — Vocabulary defines Swatch Name and both alternates, including their search-only role versus the re-import match key — PR #16 (commit `cf9974f`).
- [x] **Import** — UJ 2.1 covers active, paused and interrupted sessions, a session starting after preview, and E40's three routes — PR #16 (commit `cf9974f`).

### Cross-document

- [x] **Import / Capture** — round-2 owner fence F63: Capture §12’s token index includes Import’s preview counts/settings and generated-name placeholders; E43 follows its existing zero-count rule. — PR #16

- [x] **Import / DF / DE** — 2026-09-17 owner fence F59: DF’s inbound Import line gains field preservation, decoded values directly queryable, and measurement preservation; both obligation tables carry Rows. DF R1.2 and Export’s inbound mirror use Import R2.5/R2.6’s first-seen resolved column names; Capture’s import line mirrors both E40 session routes. — PR #16

- [x] **Capture** — the device PRD's literal `&amp;` §2 and §7 headings break three links. — done, the F9 refactor (commit `4b0f474`, PR #11): the headings read `&`, and the inbound `#2-licensing--pre-authorization` and `#7-error--state-copy` anchors resolve; the only `&amp;` left under `docs/product` is the fixed string quoted in the device fence file's F9
- [x] **Capture** — the widened cross-PRD label grep should become a standing check. — Capture §12 standing label check, F55 (PR pending).
- [x] **DF** — R7.7a–m is the sole fixture inventory; Data Export links to it instead of mirroring the list. — PR #17 agent-build amendment.
- [ ] **DF** — whether a Collection Mode rename moves an imported column's stored name.
- [x] **DF** — OQ 2 distinguishes ADR-0003's required SQLite features from ADR-0006's macOS floor; owner ratifies the reader floor after both. — PR #17 agent-build amendment.

## First build PR

Build-review items and the engineering plan's test matrix.

- [ ] **DF** — implement the CSV → store → CSV passthrough-value case now specified by R7.7b; contract clarified in PR #17, build evidence pending.
- [ ] **DF** — implement R7.7e's golden asserting `sc_simulated` false (and true) via mock simulated and hand-authored live-kind snapshots; contract clarified in PR #17, build evidence pending.
- [ ] **DF** — implement R7.6b's file/app/compatibility-version and snapshot-path assertions; contract clarified in PR #17, build evidence pending.
- [ ] **DF** — implement R7.2/R6.2d's cleared-imported-value recovery oracle independently of cleared notes; contract clarified in PR #17, build evidence pending.
- [ ] **DF** — implement R7.6k/R7.7h delete counts for an item with no earlier readings; contract clarified in PR #17, build evidence pending.
- [ ] **DF** — implement R7.7h/DJ4's never-scanned E8 rendering with no nonexistent measurement clauses; contract clarified in PR #17, build evidence pending.
- [ ] **DE** — implement E1/R1.1r/R4.1h empty-collection preview and header-only export assertions; specified in PR #18 under F27, build evidence pending.
- [ ] **DF / DE** — implement DF R7.7a/k with DE R2.4a–c’s stored-order collision tie-break, including literal `import_` names and repeated imports; specified in PR #18 under DE F22/F23, build evidence pending.
- [ ] **Capture** — "Leave it set aside" after a non-disagreeing failed attempt does not count.
- [ ] **Capture** — a mixed-route run counts.
- [ ] **Capture** — "Accept the average" and a captured row reset the counter.
- [ ] **Capture** — a run spanning the queue → review boundary.
- [ ] **Capture** — whether N_CONSEC_HARD carries across consecutive one-row look-through sessions. — Explicitly registered under Capture OQ 3 by F54; owner decision and build evidence still pending.

## ADR-0003

- [ ] **Import** — choose comparison-data version governance; re-check existing identifiers on table upgrades and verify the R2.3 comparator against the eventual ADR-0006 macOS floor (F57; [decision queue](../decisions/README.md#decision-queue)).

- [ ] **DF** — the generated ROWS_CEILING corpus's determinism contract; M6, M8 and DE R4.1 read from it.
- [ ] **DF** — the regeneration job's progress record; first settle OQ 17's partial-run presentation and overlapping-request behavior, preserving R3.3a–g.
- [ ] **DF** — the per-invariant salvage rules, including equal record-time conflicts; the general salvage promise is unchanged (PR #17, F36).
- [ ] **DF** — OQ 18–20: read-only extras and future mid-capture move/re-read policy remain open beyond PR #17’s defined floors (F38/F39); settle P1 undo visibility/identity conflicts before implementation.

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
