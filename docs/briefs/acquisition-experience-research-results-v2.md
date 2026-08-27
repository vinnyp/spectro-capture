# Research Results v2: Acquisition Experience — Gap Closure

> Second pass over the **21 unanswered questions**

## 1. Queue Model and Task Framing

**Q1.1 — queue row lifecycle states.** ANSWERED, DOCUMENTED. Four shipped state machines retrieved, one from source XML.

- **SENAITE** (open-source LIMS, bulk sample intake) ships its state machine as versioned XML. The *analysis* workflow — the row-level unit, closest analogue to a capture queue row — declares ten states: `registered`, `unassigned`, `assigned`, `cancelled`, `to_be_verified`, `retracted`, `rejected`, `verified`, `published`, `locked`, with transitions `initialize`, `assign`, `unassign`, `submit`, `retest`, `retract`, `reject`, `multi_verify`, `verify`, `publish`.
- **ODK Collect**: three states — draft → finalized → sent.
- **Goobi** (library/museum mass digitisation), per-task: Locked / Open / In queue / In process / Completed / Error / Deactivated.
- **Odoo** warehouse picking: Draft → Waiting Another Operation → Waiting → Ready → Done → Cancelled.
- **Literature model**: Russell, van der Aalst, ter Hofstede & Edmond (2005), *Workflow Resource Patterns* — `created` → (`offered to a single resource` | `offered to multiple` | `allocated`) → `started` → (`completed` | `failed` | `suspended` ⇄ resumed).

**Two transferable findings.** SENAITE has **no "in progress" state at row level** — the measurement act is instantaneous from the model's point of view; failure is not a state but `retracted`/`rejected`, both terminal, with a *new* row for the retest. And: **"skipped" exists in none of the four shipped systems, nor in the literature model.** They model *cancellation* (a recorded decision) or leave the row pending. If you want "skipped," you are designing it, not inheriting it.

**Q1.2 — re-opening and correcting a captured row.** ANSWERED, DOCUMENTED. The answer is sharper than "add an edit button": **shipped systems do not mutate a captured row; they append a correcting row and mark the original.**

- **Retract-and-retest (SENAITE)** — `retract` moves the original to `retracted` and `retest` spawns a fresh row. The erroneous reading stays in the record. Strongest precedent for a measurement system.
- **Correction sent backwards through the queue (Goobi)** — a later worker returns a process to an earlier step with an error report; "A red Correction button with a warning symbol in the My tasks list will immediately show the responsible person that the task listed in that row is one that was previously completed but that has now been reopened."
- **Finalize as a one-way door, relaxed only opt-in (ODK)** — editing finalized submissions arrived in Collect v2025.2, "off by default and must be explicitly enabled in the form definition," with edits tracked in the submission activity feed.
- **Non-destructive re-navigation (Lightroom/Capture One)** — no "captured" lock at all; re-flag by navigating back and pressing the key again.

*Residual:* whether in-flow correction beats end-of-session correction on throughput or data quality — **(c) inherently empirical**. Follow-up: instrument two builds, measure session wall-clock plus post-hoc audit error count over two 300-item runs.

---

## 2. Heads-Down Input and Interaction Modality

**Q2.1 — input modalities compared.** ANSWERED, DOCUMENTED, with real numbers. The first run said "missing comparative throughput data."

Velloso, Schmidt, Alexander, Gellersen & Bulling (2015), *ACM Computing Surveys* 48(2), Table VI — ratios of foot to hand:

| Study | Foot device | Hand device | n | time ratio | error ratio |
|---|---|---|---|---|---|
| Springer et al. 1996 | Joystick | Mouse | 17 | 2.3 | 1.5 |
| Pakkanen et al. 2004 | Trackball | Trackball | 9 | 1.6 | 1.2 |
| **Dearman et al. 2010** | **Pedals** | **Tilt** | 24 | **1.05** | **1.20** |
| **Dearman et al. 2010** | **Pedals** | **Touch** | 24 | **0.98** | **1.87** |
| Garcia & Vu 2011 | Joystick | Trackball | 16 | 1.58 | — |

**Read the Dearman rows:** pedals are as fast as the hand (0.98–1.05) and roughly **twice as error-prone** (1.87). Foot input costs accuracy, not throughput, on a discrete trigger. The survey's design conclusion: feet "excel at performing simple tasks," and "can effectively complement the hands, offering additional input channels **with no homing time**."

**Foot pedal vs keyboard for *mode* — the pedal wins decisively.** Sellen, Kurtenbach & Buxton (1992): "kinesthetic was more effective than visual feedback both in terms of reducing errors and in terms of reducing the cognitive load associated with mode management." Experiment 2: keyboard produced significantly more mode errors than a latching pedal (p < .007), and the latching pedal more than a sustained pedal (p < .0002). The rule: **user-maintained mode states prevent mode errors** — a held pedal cannot be forgotten; a toggled key can.

**Barcode-as-identity.** Poon et al. (2010), *NEJM*: transcription errors "occurred at a rate of 6.1% on units without the bar-code eMAR but were **completely eliminated**" with it. Identity transcription error goes to zero because a human never types the identifier. *[abstract/extract — NEJM full text paywalled.]*

**Voice loses.** Zhou et al. (2018), *JAMA Network Open*: speech-recognition output carried 7.4 errors per 100 words; 0.4% after transcriptionist review; 0.3% in the physician-signed version. Roughly an 18× error multiple vs a reviewed channel.

*Residual:* no study compares all five modalities in one bulk-capture task — **(a)**, searched the Velloso survey's own 300-entry bibliography. The instrument-button arm is **(c)** — depends on your device's button placement and travel.

**Q2.2 — accessibility of a heads-down, timing-sensitive capture mode.** ANSWERED for the implications; **(a)+(b)** for shipped-app precedent.

**Four hard implications, all documented:**

1. **A single-key capture loop is a WCAG Level A issue by construction.** SC 2.1.4 Character Key Shortcuts requires one of: turn-off, remap to include a non-printable key, or **active only on focus**. The third is your escape hatch and it fits — bind the keys to the focused capture surface so they are inert elsewhere. That is conformance, not a workaround.
2. **macOS already claims single letters.** VoiceOver's single-key Quick Nav maps bare letters to navigation (b/B for next/previous button, etc.), toggled with VO-Q. A user in Quick Nav will never reach your loop — the screen reader eats the keystroke. Full Keyboard Access separately claims Tab/Shift-Tab/**Space**. **Space is the worst possible advance key.**
3. **Apple's HIG warns against exactly this timing property**: "Minimize use of time-boxed interface elements. Views and controls that auto-dismiss on a timer can be problematic for people who need longer to process information, and for people who use assistive technologies that require more time to traverse the interface."
4. **Audio-only confirmation is an accessibility regression.** HIG: "**Use haptics in addition to audio cues.** If your interface conveys information through audio cues — such as a success chime, error sound, or game feedback — consider pairing that sound with matching haptics for people who can't perceive the audio or have their audio turned off." *Orchestrator-verified present in the HIG JSON.* **This qualifies the first run's earcon recommendation** — earcons are right, audio-*only* is not.

A fifth, easy to miss: Apple requires announcing state changes to VoiceOver via `AccessibilityNotification`. In a loop advancing every ~3 seconds, naive per-capture announcements produce continuous speech that never completes. The throttling policy is yours to design.

*Gap:* **no shipped app documents VoiceOver behaviour for a fast keyboard loop.** Neither Adobe nor Capture One has an accessibility section in their keyboard-shortcut documentation. Classification **(a)** with a **(b)** component — the likely holders are Apple's WWDC transcripts "Tailor the VoiceOver experience in your data-rich apps" and "Refine accessibility for custom controls." Follow-up: fetch those transcripts; then build a SwiftUI capture surface bound to single-key `onKeyPress` and measure whether the handler fires with VoiceOver+Quick Nav on, with Full Keyboard Access on, and the key-down-to-handler delta in all four combinations.

**Q2.3 — keyboard-shortcut conventions for advance/retry/skip/flag.** ANSWERED. **The negative half is as valuable as the positive.**

**The macOS HIG sets no precedent** — the agent read the *Keyboards* page in full, including both reserved-shortcut tables. Not one entry maps to advance, retry, skip, or flag. Nearest neighbours are `Esc` and `Command-Period` (cancel) and `Control-Arrow` (move focus within a view). Constraints it *does* impose: prefer Command as the main modifier; prefer Shift as secondary; **avoid Control**; list modifiers in the order Control, Option, Shift, Command. And the HIG sanctions bare single keys **only for games** ("a key binding"). A single-key capture loop on macOS is outside documented convention — permitted, unprecedented, and colliding with the Q2.2 constraints.

**So: there is no domain convention. Design freely.** That is a finding, not a research failure.

**The de-facto convention comes from photo culling, and it is a complete grammar:**

| Operation | Lightroom Classic |
|---|---|
| Flag as keep / keep + advance | `P` / `Shift+P` |
| Flag as reject / reject + advance | `X` / `Shift+X` |
| Clear flag / clear + advance | `U` / `Shift+U` |
| Cycle flag states | `` ` `` (back quote) |
| Step flag state up/down | `Command + Up/Down Arrow` |
| Rate 1–5 / rate + advance | `1`–`5` / `Shift+1`–`5` |
| Advance / retreat without classifying | `Right` / `Left Arrow` |

Extracted: *bare key = classify in place; Shift + same key = classify and advance; arrow = advance without classifying; back-quote = cycle; Command+arrow = step an ordered scale.*

**Capture One reaches the same behaviour via a persistent mode instead of a modifier** — "Select Next When" auto-advances on star rating or colour tagging. **For a heads-down loop the sticky mode is the better fit**: a modifier requires a two-key chord on every item.

Anki supplies a third data point and the only "retry"-adjacent binding found anywhere: `Ctrl+Z` = undo the last queue action.

**What does not exist anywhere: a convention for skip, or for retry.** Lightroom has no skip (advancing without flagging *is* the skip).

*Also:* the widely-reported "Caps Lock auto-advances" behaviour does **not** appear on either official Adobe page read in full. Treat as unverified.

---

## 3. Pacing, Latency, and Feedback

**Q3.1 — preventing input loss when the user outpaces the device.** ANSWERED. Five shipped patterns across four domains, with a decision rule.

- **Hardware lockout with tuned dead time** — Zebra scanners' "Timeout Between Decodes, Same Symbol," programmable 0.0–9.9 s in 0.1 s increments, **default 0.5 s**. Input inside the window is *discarded*, because a re-read of the same symbol is not new information.
- **Buffer, show depth, degrade the rate** — Nikon burst: buffer depth is displayed *while the user is pressing*, and "Frame rates may drop when the memory buffer is full." Every queued input is still valid.
- **Expose the policy as a user setting** — Nikon's AF-C Priority Selection: **Release** (shoot whenever pressed) vs **Focus** (only when in focus), plus hybrids that *throttle* rather than block. The app names the trade-off and lets the operator pick.
- **Hard lockout as safety interlock** — PCA infusion pump lockout intervals, 5–15 min; presses inside are discarded because honouring them would be harmful.
- **Framework-level deferred processing** — LabVIEW's "Lock front panel (defer processing of user actions)." Documented failure mode: with buffering, "user clicks and keystrokes will be queued up and executed rapidly once the busy event case completes, leading to unpredictable UI behaviours."

**The decision rule** (inferred, but the five behave consistently): *is a queued input still valid when it executes?* Yes → buffer, show depth, degrade. No → lockout, discard, tuned dead time.

**SpectroCapture falls firmly on the lockout side.** A second trigger fired mid-scan is not a request to measure a second item — the user has not physically moved the instrument, so honouring it later would re-measure the same item or measure the next one before it is positioned. The Zebra case exactly. **Refinement from Nikon: lockout with immediate non-visual rejection feedback beats silent discard**, because silent discard is indistinguishable from a dropped keystroke to someone not looking at the screen.

*Residual:* no source quantifies throughput/error cost of lockout-with-feedback vs buffer-and-throttle for a hardware-paced human loop — **(c)**. Follow-up: both policies behind a flag, 200 items per arm, deliberately over-eager operator; measure items/min, **readings attributed to the wrong queue row**, and triggers rejected. The middle number decides it — a buffered mis-attribution is a silent data-integrity bug; a rejected trigger is a visible annoyance.

---

## 4. Mid-Run Error Handling

**Q4.1 — error-message design in a heads-down context.** ANSWERED — **from safety-critical alerting standards, not UX literature.** The first run looked in the wrong place.

**The mainstream UX guidance is inapplicable.** NN/g's Error-Message Guidelines are built entirely on visual channels — display near the source, bold/high-contrast/red styling, severity-based visual differentiation. Every guideline presupposes gaze on the interface. That is a real gap in the UX literature.

**FAA Advisory Circular 25.1322-1 solves precisely this problem, and the rule is a hard requirement:** functional elements for warning and caution alerts "**must provide timely attention-getting cues, resulting in immediate flightcrew awareness, through at least two different senses**." Advisory-level alerts are exempt and "are normally provided through a single sense."

That gives a three-tier taxonomy tied to sensory cost:

| Tier | Requirement | SpectroCapture analogue |
|---|---|---|
| Warning | ≥2 senses | Instrument disconnected, calibration lapsed mid-run, disk write failing |
| Caution | ≥2 senses | Reading out of tolerance, multi-sample variance exceeded |
| Advisory | 1 sense | Row flagged for later review |

Four more transferable rules from the same AC:

- **Disambiguate a reused sound**: "If using aural alerts with multiple meanings, a corresponding visual, tactile, or haptic alert should be provided to resolve any potential uncertainty."
- **Synchronise channels**: "the onset of the master visual alert should normally occur simultaneously with the onset of the master aural alert."
- **Serialise audio; never overlap**: "**only one aural alert is presented at a time**… an active aural alert should finish before another begins. However, active aural alerts must be interrupted by alerts from higher urgency levels." At a 3-second cycle this is the rule that matters most — your error tone must **pre-empt** an in-flight success tone.
- **Make dismissal deliberate**: any suppression means "must not be readily available… so that it can be operated inadvertently or by habitual, reflexive action." **Your error must not be dismissible by the same key that advances the queue**, or the operator will muscle-memory past every failure.

*Residual:* the exact auditory parameters live in **IEC 60601-1-8 Annex F**, paywalled — **(b)**, document named. Cheaper follow-up: the open-access evaluation at PMC12085777 reproduces the acoustic characteristics it measured.

**Q4.2 — runs of consecutive failures and automatic-halt heuristics.** ANSWERED with **explicit numeric thresholds**. The first run said "no explicit threshold metric present in the literature." Incorrect — clinical laboratory QC has published exactly that since 1981, and it is the closest domain to yours.

**Westgard multirules** *(orchestrator-verified: `10:x` and `4:1s` appear verbatim on westgard.com)*:

| Rule | Definition | Action |
|---|---|---|
| **1:2s** | one measurement exceeds ±2s | **Warning only** — triggers inspection, not rejection |
| **1:3s** | one exceeds ±3s | Reject |
| **2:2s** | **2 consecutive** exceed the same ±2s limit | Reject |
| **R:4s** | one exceeds +2s and another −2s within a group | Reject (within-run) |
| **4:1s** | **4 consecutive** exceed the same ±1s limit | Reject |
| **10:x** | **10 consecutive** fall on one side of the mean | Reject |

**Four ideas to steal wholesale.** A single anomaly is a warning, never a halt. **Consecutiveness is the systemic-vs-random discriminator**, and the threshold scales inversely with severity (2 large / 4 small / 10 same-sided). **Directionality matters more than magnitude** — 10:x fires on ten readings merely *on the same side of the mean*, none individually out of range, which catches drift (lamp degradation, calibration decay, a dirty tile) that a magnitude-only rule never sees. **For a spectrophotometer this is the single highest-value rule in the table.** And the rules "must often be used across runs" — the failure window can span sessions.

**The Circuit Breaker pattern is documented but explicitly scoped out by its own source.** Microsoft's Azure Architecture Center lists "This pattern might not be suitable when: You need to manage access to **local private resources**… In this environment, a circuit breaker adds overhead" and "You have a message-driven or event-driven architecture, because they often route failed messages to a dead letter queue." A local, single-device, offline capture loop with a deferred-error queue hits both exclusions. **The Westgard rules are the right import; a textbook circuit breaker is not.** Two provisions worth keeping anyway: **manual override** ("provide a manual reset option that enables an administrator to close a circuit breaker and reset the failure counter") and **accelerated breaking** (trip immediately on a response that carries enough information — a BLE disconnect should not wait for three occurrences).

**Recommended policy:**

| Condition | Analogue | Action |
|---|---|---|
| 1 failed/out-of-tolerance reading | 1:2s | Warning tone, flag row, continue |
| 2 consecutive hard failures | 2:2s | Halt, offer recalibrate/reconnect |
| 4 consecutive flagged readings | 4:1s | Halt, offer recalibrate |
| **10 consecutive drifting the same direction vs session baseline** | 10:x | Halt, prompt recalibration — **the lamp-drift detector** |
| BLE/USB disconnect, disk write failure | accelerated breaking | Halt immediately on first occurrence |
| Any halt | manual override | User can force-resume and reset the counter |

*Residual:* Westgard's numbers are calibrated for clinical analysers, not a hand-placed handheld instrument — **(c)**. Follow-up: log every reading's tolerance status across your first 10 real sessions **with halting disabled**, then compute offline how many false halts each candidate rule would have produced. Tune on your own data before shipping a threshold.

---

## 5. Pre-Flight and Session Readiness

**Q5.1 — communicating a time-boxed readiness window.** ANSWERED, and one source is your own instrument vendor.

**X-Rite's eXact does not surface a "valid until" date at all.** It "will calibrate automatically every 4 hours," and prompts only when the instrument is open and the previous calibration has **timed out**, or when the measurement condition switch changes without a calibration in that position. Three transferable decisions: the window is a **fixed interval**, not a per-instrument variable date; renewal is **automatic where possible**; and the prompt is **contextual and blocking** — at the moment of use, not as an ambient banner. **The shipped precedent in your exact hardware domain is: don't show a countdown, block at point of use.**

**PKI supplies the number.** Let's Encrypt recommends checking **ACME Renewal Information at least twice a day** (the authority nominates the window), with the backstop heuristic of renewing "when they have **a third of their total lifetime left**" — for 90-day certs, day 60. Plus exponential-backoff retry (1 min → 10 min → 100 min → daily) and randomised scheduling.

**The one-third rule is the most portable number here.** Applied to a 4-hour calibration window: **warn when 80 minutes remain**. Applied to session gating: prompt for recalibration before a session if less than one third of the window remains, regardless of expected session length.

**ISO/IEC 17025:2017 §6.4.13** requires records including "the **due date of next calibration**" — but mandates that the status be *known and traceable*, not displayed. That maps to a `calibration_events` table with a computed `valid_until`. *[abstract/extract — paywalled; **(b)**, readable via the ISO Online Browsing Platform or a university subscription.]*

**Synthesis:** do not build a countdown chip. The instrument vendor doesn't, and Apple's HIG independently warns against time-boxed UI. Instead: store `calibrated_at`/`valid_until`; **gate at the session boundary using the one-third rule**; a single ambient indicator in the pre-flight panel with a blocking prompt only at the session boundary and at expiry; and **never surface a countdown during the capture loop** — the user cannot read it, and per Q4.1 the correct mid-run channel is an audible caution.

*Residual:* whether a visible countdown improves or harms pre-session recalibration compliance — **(c)**. Follow-up: log `calibration_age_at_session_start` for 20 real sessions under a no-countdown build; if the median exceeds two-thirds of the window, the ambient indicator isn't working.

---

## 6. Durability, Crash Safety, and Resumability

**Q6.4 — storing a raw opaque payload alongside derived values, and recomputation when derivation logic changes.** ANSWERED. Four independent traditions converge on the same architecture.

- **Processing levels / reprocessing (NASA EOSDIS)** — Level 0 is "Reconstructed, unprocessed instrument and payload data at full resolution"; Level 2 is "Derived geophysical variables at the same resolution and location as L1 source data." Standard products are periodically **reprocessed** from the retained lower level and published as a new **Collection version**, not a silent in-place edit. *The transferable rule: the derived value carries a version stamp naming the algorithm that produced it, and a version bump is a bulk regeneration event, not a migration.*
- **Event sourcing (Fowler)** — "all you need to do is make the fix and reprocess the events." Retroactive Event handles the case where a wrong derived value already escaped: revert to a snapshot before the branch point and replay, or build the correction in a Parallel Model and merge.
- **Stream reprocessing (Kreps)** — "Code will always change. So, if you have code that derives output data from an input stream, whenever the code changes, you will need to recompute your output." Prescription: retain the full input log, run the new derivation into a **new output table**, cut over once caught up. **The dual-write-then-swap detail is the operationally important one** — do not mutate derived rows in place while the app runs against them. Same idea as the medallion Bronze layer and dbt's `--full-refresh`.
- **Analytical instruments — closest to your domain.** *AnIML* (ASTM's XML standard for spectroscopy) separates acquisition from derived structurally, with `AuditTrailEntrySet` recording timestamp, author, action, **software used**, rationale, and a machine-readable `Diff`. The "software used" field is the derivation-version stamp, formalised. **ISO 13655:2017** splits Clause 4 (*Spectral measurement requirements*) from Clause 5 (*Colorimetric computation requirements*); normative references include CIE 15:2004 and CIE 167:2005; CxF/X-4 (ISO 17972-4) is the interchange format, carrying "metadata to describe the target and measurement conditions."

**The single most important consequence for your schema:** colorimetry is **not derivable from spectra alone**. You need the illuminant/observer pair (e.g. D50/2°) and the ISO 13655 measurement condition (M0/M1/M2/M3). **If those are not stored on the raw row, the derivation is not reproducible and recomputation is impossible** — which defeats the entire raw-payload-as-canonical strategy.

**SQLite implementation primitives, version-pinned:**

| Mechanism | Introduced | Fitness for "derivation changes later" |
|---|---|---|
| `GENERATED ALWAYS AS (…) VIRTUAL` | 3.31.0 (2020-01-22) | Cheap to add; poor for costly colour maths on every read |
| `GENERATED ALWAYS AS (…) STORED` | 3.31.0 | **Poor** — "There is no way to change the expression of a generated column after creation," and it cannot be added by `ALTER TABLE ADD COLUMN`. A derivation change forces a table rebuild |
| Plain derived columns + explicit recompute pass | — | **Best fit** — lets you version-stamp and bulk-regenerate |
| `JSONB` | 3.45.0 (2024-01-15) | **Reject** — an SQLite-internal encoding that breaks external queryability |

**Recommendation:** store the vendor payload as an immutable `BLOB` plus the ISO 13655 measurement-condition metadata as plain columns; derived colour values as ordinary (non-generated) columns with a `derivation_version` integer; treat a derivation change as a Kreps-style regeneration that writes new values and bumps the stamp.

---

## 7. Inventory Import and Column Mapping

**Q7.3 — identifier selection, duplicates, missing identifiers.** ANSWERED, DOCUMENTED. Four shipped products span the design space, differing on *who chooses the key*.

- **User chooses, per import (Airtable)** — merge requires picking a field "ideally containing a unique value." Two easily-missed semantics: matching is **case-sensitive** ("sampleemail@example.com" ≠ "SampleEmail@example.com") but **leading/trailing whitespace is ignored**. Limits: 25,000 rows, 5 MB.
- **Declared once on the schema (Salesforce)** — a field is *declared* an External ID; upsert matches on it with three specified outcomes: no match → **201** create; exactly one → **200** update; **more than one → 300 error, and the record is neither created nor updated.** The strictest posture and, for your use case, the most correct: **an ambiguous key is a hard failure, never a guess.**
- **Fixed by convention (Shopify)** — Handle is the key; the user cannot nominate another column.
- **No key at all — the documented anti-pattern (Notion)** — "CSV merges add rows. They don't update existing rows." Re-importing the same file duplicates everything. Worth naming because it is the default a naïve implementation lands on.

**Duplicate identifiers, two documented resolutions:** fail loudly (Salesforce 300) or **first row wins, silently** (Airtable — "the extension will only use the first of those rows, and subsequent rows will be ignored").

**Missing identifiers — the SQLite trap.** "For the purposes of UNIQUE constraints, **NULL values are considered distinct from all other values, including other NULLs**." A `UNIQUE` column admits unlimited NULLs, so a nullable identifier silently accumulates unmatched rows and **the UPSERT conflict target never fires**. Compounded: "According to the SQL standard, PRIMARY KEY should always imply NOT NULL. Unfortunately, due to a bug in some early versions, this is not the case in SQLite" — unless the column is `INTEGER PRIMARY KEY`, or the table is `WITHOUT ROWID` or `STRICT`, or the column is declared `NOT NULL`.

**The modelling rule (Kimball):** the imported natural key should not be the table's primary key — natural keys "are subject to business rules outside the control of the DW/BI system." Maintain a separate **durable key**. For SpectroCapture: the CSV's marker code is a **match key**, not the row identity.

**Q7.5 — re-importing an updated inventory.** ANSWERED, DOCUMENTED. Three conflict-resolution shapes in increasing order of user control:

1. **A single global toggle (Shopify)** — "Overwrite products with matching handles"; unselected, matching products are ignored. **The transferable detail:** a column present but blank **overwrites the existing value to blank**; a column omitted entirely **leaves the existing value untouched**. That distinction is how you let a user update *some* fields without nulling the rest.
2. **Merge toggle + user-chosen key + a counted preview (Airtable)** — before committing, the panel shows counts of records to be **updated, unchanged, and newly created**. The three-way count *is* the reconciliation interface: "1,200 new, 0 updated" tells the user the key didn't match, before any writes land.
3. **Per-row upsert with explicit outcomes (Salesforce)**.

**The conceptual frame** is Kimball's slowly-changing-dimension taxonomy — type 1 (overwrite), type 2 (add a row), type 3 (add a column) — with the documented cost: "Type 1 destroys the history of a particular field." Not academic here: if a re-imported CSV renames marker #417 from "Cool Grey 3" to "Cool Grey III," a Type 1 overwrite silently rewrites the label on measurements already taken under the old name.

**The primitive:** UPSERT added in **SQLite 3.24.0 (2018-06-04)**, generalized in **3.35.0 (2021-03-12)** to permit multiple `ON CONFLICT` clauses and `DO UPDATE` without a conflict target. Caveats: "UPSERT does not currently work for virtual tables" (relevant if the inventory ever sits behind FTS5), and "The conflict resolution algorithm for the update operation of the DO UPDATE clause is always ABORT… the entire INSERT statement rolls back and halts." **And per Q7.3, the conflict target must be a real UNIQUE constraint that a nullable column will not satisfy.**

---

## 8. Progress, Orientation, and Session Completion

**Q8.4 — partially-completed sessions.** ANSWERED. All three strategies shipped, in four domains, differing chiefly in *who decides the session is over*.

- **Explicit user-declared state with a one-way transition (ODK Collect)** — draft / finalized / sent. Drafts carry a per-draft validation badge: red **Errors** or blue **No errors**. Three version facts worth copying rather than rediscovering: error-checking during entry arrived in **v2023.2.0**; "**Prior to Collect v2024.1, finalized forms could go back to the draft state. This was removed to better satisfy the goals of the finalized state**" — i.e. the project deliberately made the transition one-way *after* shipping the reversible version; editing finalized submissions returned in **v2025.2**, off by default.
- **Implicit save with a timeout that auto-converts abandonment (Qualtrics)** — a separate **Responses in Progress** list showing start time, last activity, expiration date and progress percentage; a configurable window (1 hour → 1 year) after which the partial is either recorded or deleted; and afterwards the partial is distinguishable by **two metadata fields: `Finished = False` and `Progress`**. A hard backstop: always resolved within 1 year. *Transferable: a separate in-progress list, a boolean + a scalar, and a policy that resolves abandonment automatically rather than leaving it open forever.*
- **A named status vocabulary as a colour-coded grid (REDCap)** — Incomplete / Unverified / Complete. *[Institutional manual, not vendor spec — treat the vocabulary as reliable, the colour mapping as institution-specific.]*
- **Closest analogue: warehouse cycle counting (Dynamics 365)** — Open → In process → Pending review → Closed, with invariants stated: "there can't be two Open, In process, or Pending review cycle counting work records for the same location" — **one in-flight session per subject, enforced structurally**. Two interface details for a heads-down loop: **Skip** "skips the entire cycle counting work record" rather than the current item (a shipped product documenting its own confusing semantic — make skip mean *this item*), and "**To prevent accidental selection, the Cancel button isn't shown during cycle counting mobile workflows**" — the product removes the abandon affordance from the capture surface entirely.

**macOS mechanics, version-pinned:** `SceneStorage` **macOS 11.0** — "The system makes no guarantees as to when and how often the data will be persisted"; `Scene.restorationBehavior(_:)` **macOS 15.0**. **Note the explicit no-guarantee clause:** `SceneStorage` is fit for restoring *which* session was open, and **unfit as the durable record of session progress** — that must live in SQLite.

---

## 9. SwiftUI Focus-Mode Capture Surface

**Q9.3 — state ownership, `@Observable` vs `ObservableObject`, version gates.** ANSWERED, DOCUMENTED from Apple primary sources via the DocC JSON API. *Orchestrator-verified: `metadata.platforms` returns macOS 14.0 for Observation.*

| API | Min macOS | Role |
|---|---|---|
| `@State` | **10.15** | Now the source-of-truth wrapper for `@Observable` reference types too |
| `@StateObject` | **11.0** | Legacy; replaced by `@State` after migration |
| `@Observable` (Observation) | **14.0** | Replaces `ObservableObject` + `@Published` |
| `@Bindable` | **14.0** | Replaces `@ObservedObject` *only where a binding is needed* |

**The performance argument, which is why this mattered.** Apple: "when tracking as `Observable`, SwiftUI updates a view only when an observable property changes and the view's `body` reads the property directly… In contrast, a view updates when **any** published property of an `ObservableObject` instance changes, even if the view doesn't read the property that changes."

**For a capture surface this is decisive.** Under `ObservableObject`, a session model holding `currentIndex`, `capturedCount`, `elapsedTime`, `lastReading` and `flaggedItems` invalidates **every** observing view on **every** mutation — including a per-second timer tick.

**Where session state should live**, from Apple's "Managing model data in your app" (macOS 14.0), with several counter-intuitive rules:

- Source of truth: `@State private var library = Library()` on the `App` struct, injected with `.environment(library)`.
- **Pass-through is free.** "If a view doesn't have any dependencies, SwiftUI doesn't update the view when data changes. This approach allows an observable model data object to pass through multiple layers of a view hierarchy without each intermediate view forming a dependency."
- **But storing a reference is not free.** "a view that stores a reference to the observable object updates if the reference changes… because the stored reference is part of the view's value and not because the object is observable."
- **Collections track structurally, not deeply** — insert/delete/move/replace. A `List`'s `@escaping` row closure means each row depends on its own item's properties, not the parent.
- "Observation tracks changes to any observable property that appears in the **execution scope** of a view's `body`" — execution scope, not lexical text. That single word is why the over-broad-dependency trap exists.

**Migration mechanics:** drop `ObservableObject` conformance for `@Observable`; delete `@Published`; mark exclusions `@ObservationIgnored`; `@StateObject` → `@State`; `.environmentObject(_:)` → `.environment(_:)`; `@EnvironmentObject` → `@Environment`; `@ObservedObject` → **nothing** unless a binding is required, then `@Bindable`. Migration is incremental — "Your app can mix data model types that use different observation systems."

**Recommendation:** one `@Observable` `CaptureSession` owned by `@State` at scene level and injected via `.environment(_:)`; **per-queue-row `@Observable` view models** rather than views reading into a shared array; `@ObservationIgnored` on the instrument SDK handle and any high-frequency non-UI field. macOS 14 is the floor for all of it.

**Q9.6 — view-update cost and profiling.** ANSWERED for guidance and tooling; the *measurement* clause is **(c)**.

**The tool:** the **SwiftUI instrument in Instruments 26**, requiring Xcode 26 on the host and a current OS on the profiled machine. Lanes: **Update Groups** ("If CPU usage is spiking during a time when this lane is empty, you'll know that your problem likely lies somewhere outside of SwiftUI"), **Long View Body Updates**, **Long Representable Updates**, **Other Long Updates** — coloured orange and red "based on how likely they are to contribute to a hitch or hang."

**The threshold is the frame deadline, not an absolute millisecond count.** Apple is explicit that it is not only about single slow bodies: "there are a large number of relatively fast updates that all have to happen during this frame. All this extra work results in the app missing the deadline."

**Why a debugger won't substitute:** "Because SwiftUI is declarative, **you can't use the backtrace to understand why your view is updating**." The reframe: "When I ask 'why did my view body run?' the real question is 'what marked my view body as outdated?'" — which is what the **Cause & Effect Graph** answers.

**The failure mode most likely to hit a capture surface, demonstrated by Apple.** In the WWDC25 Landmarks demo each row called `modelData.isFavorite(landmark)`, which internally read the whole favourites array: "This causes `@Observable` to establish a dependency between each item view and the whole array of favorites. So, whenever I add a favorite to the array, **every item view's body runs**." Fix: per-row `@Observable` view models. **Map this straight onto SpectroCapture — if each queue row reads `session.capturedIDs.contains(row.id)`, every visible row's body runs on every scan.**

Two further hazards: **environment reads cost even when the body doesn't run** ("avoid storing values that update really often, such as geometry values or timers, in the environment" — so no elapsed-time or throughput counter in the environment); and **`List`/`ForEach` identity is gathered eagerly**, so a conditional row or an `AnyView` makes the row count variable and forces building all content. Filter in the data collection, not the row builder.

`Self._printChanges()` remains available but carries Apple's warning: "it is never guaranteed to always exist and may even be removed in a future release, so **never submit a call to this method to the app store**… It's only meant for debugging and has a runtime performance impact."

**The (c), stated precisely:** **no published quantitative benchmark** exists for SwiftUI view-update cost on a rapid hardware-paced capture surface. The agent found blog posts asserting "20–30% fewer view redraws" for `@Observable` and **explicitly declined to cite them** for lack of a primary source or methodology. Follow-up: a 1,200-row `List` on macOS 26 / Xcode 26 backed by an `@Observable` `CaptureSession`, two arms — (A) each row reads `session.capturedIDs.contains(row.id)`, (B) per-row `@Observable` — driven at 1 Hz and 3 Hz, recording body-update counts per capture event and the Long View Body Updates lane. Apple's walkthrough predicts arm A produces O(visible rows) body runs per capture and arm B exactly 1; **the measurement you actually need is arm B's wall-clock cost at your real row complexity against the frame deadline.**

---

## 10. Measuring Acquisition Throughput

**Q10.3 — published throughput benchmarks.** ANSWERED, DOCUMENTED. The first run's verdict ("hard baseline targets missing") and its follow-up ("interview two domain experts") were **both wrong** — this literature is large, quantitative, and open access.

**Powell et al. (2021)**, *Applications in Plant Sciences* 9(4) — herbarium digitisation, 1,660 logged imaging hours *(orchestrator-verified: "2.30" and "specimens per minute" appear in the source)*:

| Task | Volume | Rate | Per hour | Per item |
|---|---|---|---|---|
| Imaging | 229,333 specimens | 2.30/min over 1,660 h | ~138 | ~26 s |
| Skeletal databasing | 231,307 | 3.14/min over 1,228 h | ~188 | ~19 s |
| Barcode application | 180,949 | 4.07/min over 740 h | ~244 | ~15 s |
| Project-wide (primary tasks) | 213,863 | 0.983/min | ~59 | ~61 s |
| Project-wide (all logged hours) | 213,863 | 0.691/min | ~41 | ~87 s |

The same paper fits **experience curves** — imaging `y = 1.95170 + 0.02118x`, skeletal databasing `y = 2.55659 + 0.02760x` (y = specimens/min, x = cumulative hours) — and reports that longer technician retention "can reduce labor requirements by 20%."

**Workflow-design deltas — the strongest published support for your product thesis.** Tulig et al. (2012), NYBG *(orchestrator-verified: "125 records" and "30 records" both present)*: manual data entry **10 records/hour**; streamlined collection-event entry **30/hour**; semi-automated partial-record creation **125/hour**, with imaging at 85 exposures/hour. **The 12.5× spread is entirely a function of interaction design, not hardware.**

**Not a valid target:** the Smithsonian conveyor line at 4,000–4,500 specimens/day requires 5,000 specimens pre-staged before an 8AM start and three conveyor operators plus a handler. A mechanized multi-person line, not a person at a workstation.

**The target to set:** the closest structural analogue (one person, one item at a time, instrument-paced, no per-item metadata) is barcode application at ~244/hour (~15 s/item); imaging at ~138/hour (~26 s/item) is realistic once physical handling is involved. **A sustained 120–240 items/hour, 15–30 s/item, is the documented human-throughput envelope** — so a 1,200-item collection is a 5–10 hour job. The instrument's scan cycle sets the floor; the question the architecture must answer is whether the UI adds anything on top.

*Bounded gap — **(a)**:* no published throughput benchmark exists for spectrophotometric bulk colour capture specifically. Vendors publish instrument scan-cycle times but not operator session throughput. **The figures above are cross-domain analogues and the ADR should label them as such.**

**Q10.4 — detecting user fatigue.** ANSWERED in two parts, with one genuine negative.

**The decrement is real and fast.** Al-Shargie et al. (2019): "target detection performance decreases by **15% in 30 minutes** during a monotonous task," manifesting as increased reaction time and error rate. Thirty minutes is well inside a single session.

**The canonical instrument is response-time distribution, not mean rate.** Basner et al. (2021), *Sleep* 44(1): the operative metric is the **lapse**, "RTs ≥500 ms," alongside response speed as the reciprocal of reaction time. And a measurement-precision requirement for anyone instrumenting inter-scan intervals: "a response bias of up to ±5 ms with a standard deviation of up to ±10 ms are tolerable." **A timing pipeline sloppier than that cannot support a fatigue inference.**

**The mitigation has a hard number, and it is counter-intuitive.** Galinsky et al. (2000, 2007), NIOSH field studies on real data-entry operators: adding four 5-minute breaks to a conventional two-15-minute-break schedule — "Discomfort and eyestrain were significantly lower with supplementary breaks… **Data-entry speed was significantly faster with supplementary breaks so that work output was maintained, despite replacing 20 min of work time with break time**." **Twenty minutes of break per day cost zero output.**

**One shipped system does exactly what SpectroCapture would need**, and its design is public: Mercedes-Benz ATTENTION ASSIST — "A steering sensor is coupled to smart software that uses 70 parameters to establish a unique driver profile during the first 20 minutes of driving… the system identifies the erratic steering corrections drivers make as they begin to get drowsy and triggers an audible warning, and a 'Time for a Rest?' message with a coffee cup icon." **The transferable architecture is exact:** per-user baseline captured early → detect deviation in input-timing *micro-variability*, not absolute speed → respond with a non-blocking audible cue plus one glanceable dismissible prompt. **It never stops the car.**

*Bounded gap — **(a)**:* **no shipped desktop capture, cataloguing, or digitisation application detects operator fatigue and acts on it.** The break-reminder category (Workrave, Stretchly, Time Out) fires on **elapsed time**, not detected degradation — a different and much weaker mechanism. **If SpectroCapture ships this, it is very likely first in its category.**

**The signal to instrument:** the coefficient of variation of the inter-scan interval plus a lapse count against a per-session baseline. **Rising variance is the fatigue signal; a falling mean is not**, because the instrument's cycle dominates the mean.

**Q10.5 — telemetry for a local-first, offline, open-source app.** ANSWERED, DOCUMENTED. Four shipped postures and one instructive failure.

| Posture | Precedent | Specifics |
|---|---|---|
| **No telemetry at all** | Obsidian | "We do not collect any telemetry data." Plugin developers are barred: "capturing client-side telemetry data is prohibited" |
| **Opt-in, per-incident, reviewable before send** | Audacity | "you are shown the relevant information and given the option to send it or not send it to us as a report" |
| **Opt-in aggregate, results published publicly** | Debian popularity-contest | Explicit opt-in; random 128-bit ID; raw submissions retained 24 h, anonymised ≤20 days; aggregates public |
| **Opt-out aggregate** | Homebrew | `brew analytics off`; payload "does not contain a user identifier or an IP-address field"; 365-day retention |
| **The failure to avoid** | Fedora | Opt-out telemetry proposal **withdrawn**. Its rationale — "we know that opt-in metrics are not very useful. Few users would opt in" — drew the charge that a default-on toggle is a dark pattern |

**The load-bearing lesson:** Fedora and Audacity both attempted telemetry that was conservative by commercial standards and **both were forced to retreat**. Homebrew's opt-out survives only because it is a developer tool with no user identifier and no per-user history. **For a privacy-positioned app, opt-out is off the table**, and even opt-in aggregate transmission buys reputational risk disproportionate to the data it yields.

**Recommendation — the posture that costs nothing and answers the actual question.** The metrics SpectroCapture needs are **single-user, single-session, and diagnostic**: the developer does not need a population, the *operator* needs their own numbers.

1. Write session timings to a local SQLite table, never transmitted (Obsidian's posture — no policy, no consent flow, no server).
2. Surface them as a **session summary** — items/hour, mean and variance of inter-scan interval, hardware-wait vs UI-wait split, flagged count. **This converts telemetry from something taken from the user into something given to them.**
3. Provide a user-initiated **"Export diagnostics"** writing a plain-text file the user reads in full before attaching it to an issue — Audacity's review-before-send flow with the transmission step removed entirely. **The user is the transport.**
4. If aggregate data is ever wanted, follow Debian: explicit opt-in, no default, publish the aggregate publicly so the collection is auditable.

**Do not build a UUID. Do not build an opt-out. Do not build a network path at all in v1** — a codebase with no telemetry endpoint cannot be accused of having one, and that claim is itself a feature for this product's positioning.

---

## 11. The Seam — Capture to Collection

**Q11.2 — where a just-captured item goes.** ANSWERED, DOCUMENTED. Four shipped patterns, one with a documented failure that settles the design question.

1. **Immediate insert into the real collection, auto-select the latest, with an explicit opt-out (Lightroom Classic)** — tethered captures go straight into the Library session folder. "By default, when shooting tethered the focus will shift to each new photo as it appears… but you can disable that behavior by checking the Disable Auto Advance checkbox." **Note the direction of the default: auto-follow-the-newest is on; pinning is the opt-out.** The capture bar "can be minimized to show only the shutter button or hidden entirely using keyboard shortcuts without ending the session."
2. **A named staging collection that is also an ordinary browsable folder (Capture One)** — Sessions create Capture/Selects/Output/Trash; promotion to Selects is an explicit later act. The staging area is not a special mode-scoped buffer.
3. **Deferred insertion at session end — and its documented cost (Capture One ReTether)** — shoot to card for up to 2 hours while disconnected, then auto-import on reconnection, recommended ceiling 100–300 images. **The documented failure is on the same page:** after a connection drop the re-import produced duplicates — "Now I have hundreds of duplicate files that I need to scroll through and delete." Capture One's 16.8 notes acknowledge the class of bug.
4. **Deferred *exception* queue, not deferred insertion (Dynamics 365)** — every count writes immediately; only counts outside deviation limits route to "Pending review." **The data lands; only the adjudication defers.**

**Weaker evidence flagged and rejected:** Apple Photos' "Recently Saved" is scoped by Apple's docs to items "saved from other apps — like Messages, Safari, and Mail," so it is **not** clean evidence for a capture-recents strip.

**The answer:** write each reading into the real collection store immediately; make it findable through a persistent recents strip in the capture surface; auto-follow the newest by default with an explicit pin; **defer only adjudication of flagged items, not insertion**. **Do not build deferred insertion at session end** — ReTether is the field's own experiment in that design and its published failure is duplicate records on reconciliation, the precise bug class that costs a user trust in an inventory they cannot independently verify.

**Q11.4 — entering capture mode from within a collection.** ANSWERED, DOCUMENTED. Dynamics 365's Warehouse Management app documents this more completely than any photography product, and maps almost one-to-one.

**Four named entry modes:** *User directed* (worker specifies an existing Open work ID — resume a known task); *System directed* (system assigns the next — fresh session); *Cycle count grouping* ("groups cycle counting work IDs that are specific to a particular location, zone, or work pool" — **capture into a specific subset**); and ***Spot cycle counting*** — the answer to re-scanning: "the worker counts items in a warehouse location at any time, even if no open cycle counting work exists… If no open cycle counting work exists for that location, then the system creates a new work record… If open cycle counting work does exist, then the existing work record is used."

**That last mechanism is the pattern to copy: the identifier is the entry point.** Scan or enter an item's ID; the system attaches to existing work or creates it. No separate "re-scan this row" navigation is needed, and it works identically whether the operator arrived from a browse view or from nowhere.

**Four adjacent decisions in the same document:** an **"Add LP or item" button** for items present but not in the worklist; **blind re-capture** — "The system never shows the expected quantity to count. This design prevents intentional miscounts," with a **"Number of attempts"** setting instead (**if SpectroCapture re-scans an item that already has a reading, showing the prior value biases the operator**); the **skip trap** already noted; and **no Cancel button** during counting flows.

*Bounded gap — **(a)**, narrow:* no source documents a *per-item deep link* from a browse row into a scoped capture surface. The shipped mechanism is identifier-driven attachment, achieving the same outcome by a different route. The first run's proposed follow-up ("a context menu action that pushes the item to the head of a new Capture modal") is a reasonable design but is **not** the shipped pattern.

**Q11.6 — avoiding "two apps bolted together."** ANSWERED at explicitly mixed evidence strength — **and this contradicts the first run's closing recommendation.**

**On post-mortems — (a), leaning (b):** **there are no formal post-mortems of an application failing at the capture/collection seam.** Nothing exists at that level of rigour. Leaning (b): if such material exists it is most likely in practitioner books — **Raskin's *The Humane Interface* (2000)** is the specific source likely to hold a rigorous treatment of the underlying mode problem. The well-documented software-redesign post-mortems that do exist (the Sonos 2024 app) concern feature removal and infrastructure rewrites, not this seam; citing them would be a stretch and the agent declined to.

**On design guidance — DOCUMENTED and load-bearing.** Laubheimer (2019), NN/g *(orchestrator-verified: "quasimode" and "two visual indicators" appear verbatim)*:

1. **Modes are justified when input channels run out** — they help "when we have too many different options that we want to make available to users, and not enough available types of input to accommodate them all." A heads-down loop reassigning the whole keyboard is exactly this case. **A capture mode is defensible on NN/g's own criteria.**
2. **Redundant mode signalling is required** — "at least two visual indicators… to ensure that users are aware of the currently active mode."
3. **Avoid modes where a slip destroys work.**
4. **Quasimodes are the wrong tool here** — spring-loaded modes prevent slips but are "less efficient in cases where the user wants to remain in the mode for an extended set of actions," which is the entire session. **A held-modifier capture mode is ruled out.**

Also documented: modes damage **discoverability and findability** — which is the mechanism by which "two apps bolted together" actually hurts, stated as a design principle rather than a post-mortem.

**On named critiques — journalist strength, and one rejected source.** The one clean retrievable critique of a shipped module split is Lightroom Classic: "Having to keep swapping between the Library and Develop modules for organising and editing can be annoying" (Life after Photoshop, 2021). **Strength: a single named reviewer's opinion piece.** The agent **checked and rejected** an Adobe community thread proposing to strip Lightroom Classic to Library+Develop — it argues **performance**, not fragmentation, and was rebutted in-thread. Reported because it is precisely the kind of forum thread a less careful pass would cite as user-complaint evidence.

> ### ⚠️ This contradicts the first run's closing recommendation
>
> The first run recommended "an opaque, full-window modal takeover" for the capture surface, **citing no shipped product**. The two most-used tethered-capture applications in the world both do the opposite:
>
> - **Lightroom**: the tethered capture bar is a *floating bar over the normal Library/Develop interface*, minimizable to just a shutter button "without ending the session." The library never goes away; the capture affordance shrinks.
> - **Capture One**: captures land in a Session Capture folder that is simultaneously an ordinary browsable collection. **There is no capture-private buffer to reconcile.**
> - **Dynamics 365** (non-photo): counted inventory goes into the same on-hand records the rest of the system reads; only exceptions park in a review queue.
>
> The first run's stated rationale — that background re-renders of a large grid would steal main-thread time — is a **performance** argument, and the answer to a performance argument is *don't render the grid*, not *hide the collection behind an opaque window*. **This should be re-opened before the ADR is written.**

**The synthesis:** avoiding "two apps bolted together" is not a navigation-model choice; it is a **data-model and vocabulary** choice. One store, one identifier vocabulary, one set of item states visible from both surfaces, and a capture surface that is a *state of the collection* rather than a different place. Then satisfy Laubheimer's requirement with at least two redundant, unmistakable indicators, and protect against the mode slip that costs work — which Dynamics 365 does by simply not rendering a Cancel button.

---

## Corrections to the first run

| Q | First run | This run |
|---|---|---|
| 1.1 lifecycle states | Unanswered — "literature focuses on pipeline states" | **Answered.** Four shipped state machines (one from source XML) + the academic model |
| 1.2 in-flow correction | Unanswered — "literature omits inline correction" | **Answered.** Four patterns; the dominant one is append-a-correction |
| 2.1 input modalities | Unanswered — "missing comparative data" | **Answered.** Velloso Table VI, Sellen's controlled experiment, Zhou on voice, Poon on barcode |
| 2.2 accessibility | Unanswered — "documentation entirely absent" | **Answered for implications.** WCAG 2.1.4 Level A; VoiceOver Quick Nav; HIG time-boxed warning; **audio-only is a hearing regression** |
| 2.3 shortcut conventions | Unanswered — "HIG does not define standards" | **Answered.** HIG confirmed silent (full text read) — a real "design freely" finding — plus Adobe's complete grammar |
| 3.1 input loss | Unanswered — "literature does not detail buffering" | **Answered.** Five patterns, four domains, decision rule supplied |
| 4.1 heads-down errors | Unanswered — "sources lack guidance" | **Answered** from FAA AC 25.1322-1. UX literature confirmed inapplicable |
| 4.2 consecutive failures | Unanswered — "no explicit threshold or algorithm" | **Answered.** Westgard thresholds; circuit breaker documented *and* scoped out by its own source |
| 5.1 readiness window | Unanswered — "UI patterns not documented" | **Answered.** X-Rite's own behaviour + Let's Encrypt's one-third rule + ISO 17025 |
| 6.4, 7.3, 7.5, 8.4, 9.3, 9.6 | Unanswered — retrieval failures | **All answered from primary sources** via the DocC JSON API |
| 10.3 throughput | Unanswered — "interview two domain experts" | **Answered.** Three papers with per-task rates. No interviews needed |
| 10.5 telemetry | Unanswered — "not detailed" | **Answered.** Five shipped policies incl. a documented failure |
| 11.4 re-scan entry | Unanswered — "deep linking missing" | **Answered.** Four named entry modes; spot counting is the exact pattern |

## Residual gaps after this pass

**(a) No literature exists** — no five-way input-modality comparison; no shipped-app VoiceOver-vs-fast-loop precedent; no spectrophotometric throughput benchmark; no shipped capture app that detects fatigue; no formal seam post-mortems; no per-item browse→capture deep-link pattern.

**(b) Exists but unreachable** — IEC 60601-1-8 Annex F (auditory alarm parameters; open-access proxy at PMC12085777); ISO/IEC 17025:2017 §6.4.13 (ISO OBP or university subscription); Goobi's status enum (reachable via `StepStatus` in the source); Raskin's *The Humane Interface*; Apple's two VoiceOver WWDC transcripts.

**(c) Inherently empirical** — in-flow vs end-of-session correction; the instrument-button input arm; lockout vs buffer throughput cost; Westgard thresholds tuned to your instrument; countdown effect on recalibration compliance; SwiftUI view-update cost at your row complexity.

**Every (c) has a specified experiment in its section above.** That is the actionable residue: what remains is measurement in the hardware spike, not more reading.