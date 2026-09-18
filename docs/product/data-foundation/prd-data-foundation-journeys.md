# Data Foundation PRD — user journeys

Executable acceptance scenarios for [the PRD](prd-data-foundation.md). The PRD’s journeys index is the sole exercised-row inventory; nothing here adds a rule. Requirement rows are authoritative; state IDs refer to [shipping copy](prd-data-foundation-copy.md#error--state-copy), independently of literal wording. Fixtures come from R7.7; no instrument is required except the separately gated reference evidence in OQ 6/15.

DJ1 moved to [Data Export EJ1](../export/prd-data-export-journeys.md#ej1-export-the-collection) under F30; its ID is retired. DJ2–DJ5 and their anchors remain stable.

## DJ2. Fix a bad scan by measuring it again

| Initial state | Action | Stored/result oracle | State |
| :--- | :--- | :--- | :--- |
| Item's current reading A exists | Save a full re-scan B during capture, using Capture's sample/acceptance rules | B current, every sample and A retained; B correction-unconfirmed; no correction question mid-loop and no display of the old value during measurement ([Capture R8.7](../capture-mode/prd-capture-mode.md#8-deferred-row-review-and-corrections)) | E11/E26 outside the loop |
| A → B unresolved | Answer “The old reading was wrong” | B reason correction; A marked never true, retained in history, excluded from over-time views and QC reference selection | E11 dismissed |
| A → B unresolved | Answer “The swatch has changed” | B reason re-measurement; both legitimate time points, ordered by measurement time | E11 dismissed |
| A → B unresolved | Ask me later; end session; return to review | No expiry or inferred answer; unresolved pair remains enumerable from item and global review | E11/E26 |
| A → B → C, both relationships unresolved | Answer B's question, then C's independently | First answer affects A's status, second B's; C stays current throughout, timestamps unchanged | E11/E26 |
| Readable earlier reading H exists | Restore H | New reading/sequence equal to H with its own sample/decoded-value copies; six spaces freshly derived at current version under collection reference (R3.5 exceptions); H's measurement time retained, new record time; H and its sets unchanged | History shows added restore |
| Item has current B | Take a QC comparison | Separate comparison record, no canonical change or supersession reason; excluded from item reading counts | QC PRD owns surface |

## DJ3. Open a file made by an older or a newer app

| Initial state | Action / induced failure | Result oracle | State |
| :--- | :--- | :--- | :--- |
| Supported older format | Open; allow snapshot and upgrade | Snapshot openable before upgrade starts, named path retained after upgrade and a second upgrade; every reading preserved; progress observable | E2 |
| Older format, no snapshot space | Open; retry; or open read-only | No upgrade before safe snapshot; retry rechecks; read-only leaves source unchanged | E24 |
| Upgrade in progress | Fail partway; retry or reveal snapshot | Source remains original version; name found/expected versions and snapshot; retry has same prerequisites | E3 |
| Migration would lose a reading | Open; choose read-only | Refuse upgrade, name reading/file/app/last compatible versions; no loss | E23 |
| Newer format / beneath compatibility floor | Open | Read-only with item list, canonical values and history marks; versions named, source unchanged; no export or falsely complete partial interpretation; extras remain OQ 18 | E1 / E16 |
| Damaged current measurement | Open / inspect item after reopen; scan again or restore readable history | No current value or automatic promotion; damaged record retained; new initial/restore reading on recovery; initial recovery never raises E11 or enters E26 | E4 current |
| Damaged historical measurement, healthy current | Inspect history after reopen | Current selection/values unchanged; only damaged history quarantined, not offered for restore | E4 history |
| Only a sample archive is unreadable | Inspect reading after reopen | Mean, decoded measurements, derived values and canonical selection unchanged; damaged archive retained/marked | E31 |
| Derived set unreadable, measurement intact | Rebuild colour values / Not now | Mark set; requested rebuild uses current algorithm version without bump, keeps original damaged set, measurement/current selection unchanged; defer preserves mark | E32 |
| Fabricated prior-format fixture | Open and exercise each upgrade branch | E2 success, E3 failure, E23 lossy refusal and E24 snapshot-full all tested in v1; M5 has nonempty population | E2/E3/E23/E24 |
| File-level damage, including two current readings | Save what is readable to fresh destination | Original untouched; readable output complete, opens read-write, resolutions listed; later-recorded reading selected, no reading discarded; per-invariant rules remain ADR-0003’s without narrowing salvage | E5 |
| Move/copy/snapshot/salvage destination | Induce no room, no permission, disappearance, mid-write failure or crash | Original retained, no partial output, no failed copy advertised as recovery; retry/choose-elsewhere preserve operation identity | E19–E21 / E28–E30 |
| Another running app / stale hold from dead app | Open; retry / choose different file | Live holder refuses; dead holder never blocks; retry rechecks ownership | E10 / normal open |
| Capture active, paused or halted | Open a different file; Cancel / Go to the session | Refuse without changing current file/session; pause does not permit switching; return action opens same session | E22 |
| Persisted interrupted session, no in-flight capture | Open another file | Target prechecked before original closes, interrupted session retained there; reopening original still offers normal resume, not automatic capture | Target's opening state |
| Target fails version/integrity/ownership checks | Request file switch | Original remains open and unchanged; show target opening state, no new no-file-open state | Target's opening state |
| Any applicable picker | Cancel before starting output | No move, copy, salvage or upgrade initiated by cancellation | Prior surface |

Move/re-read are disabled during active, paused or halted capture; E9/E15/E25/E27 explain why and route to End session, while interruption enables them (F38). Full store-volume failures follow R1.10/E15 and Capture's collection-unavailable state when the source disappears.

## DJ4. Delete a swatch

| Initial state | Action | Result oracle | State |
| :--- | :--- | :--- | :--- |
| Never-scanned item | Request delete | Identity/import data named; no current or historical measurement claimed; delete not default | E8 |
| One reading, no history / several readings / whole collection | Request delete | Correct item/reading/history counts, never sample counts; actual undo availability shown | E8 / E14 |
| Delete confirmation | Cancel | All content unchanged | Confirmation dismissed |
| Delete confirmation | Export first, succeed or cancel/fail export | Delete unperformed, confirmation retained; canonical versus full-history behavior follows Export R1.3's independent P1 gate | Export E1 then E8/E14 |
| P0 delete | Confirm | Deletion final, content unrecoverable from active file at reader floor | Item/collection removed |
| P1 pending delete | Undo in same open-file lifetime | Original content, identities, samples, readings/history, attempts and decisions restored; OQ 20 must settle outside-reader visibility and reused identifiers before implementation | Item/collection restored |
| P1 pending delete | Independently test quit, crash, file close and file switch; reopen | Delete stands, no surviving undo, no recoverable deleted content in active file | Item/collection absent |
| Note / imported value contains known text | Clear each independently | Previous text unrecoverable from active file | Updated item |
| Copy/snapshot predates delete/clear | Delete/clear in active file | Existing copy retains its prior content; it is not an erasure target | E12 lists retained copies |
| Measuring-build session record present | Delete item/collection | Session-scoped interaction record remains per R6.5 | No claim of deleting that record |
| Saved device used by readings | Remove saved device | Every measurement snapshot unchanged | Device record removed |

No action deletes one reading out of history. Whole-file deletion remains a Finder operation documented in help; the app exposes item/collection deletion only.

## DJ5. Query the file without the app

| Query / action | Expected result |
| :--- | :--- |
| Read format metadata before collection data | Stable file-format location/form, distinct from CSV export format version |
| Query item identity, imported fields and canonical selection | Match app values; first-seen resolved column names/positions retained, additions appended after later import |
| Read each sample and stored mean | Decoded spectrum/colour values and conditions available without archived vendor bytes |
| Query current/history, supersession reason, never-true, quarantine and sample archive-unavailable marks, unreadable-set marks and unresolved relationships | Match R2.3a–i and R5.5a–c/f, including A → B → C and restore time axes |
| Query derived sets and gamut marks | Six spaces when condition exists, explicit absent mark otherwise; illuminant/observer/condition/version present, superseded sets retained |
| Enumerate stale stamps or spectral reference mismatches during regeneration, then after completion | Interrupted work findable; resume skips healthy sets at target version/reference; completion excludes damage (listed separately), explicitly absent conditions and non-spectral fixed-reference mismatches |
| Reopen app and explicitly re-read externally changed data | Refresh through R7.3c; no claim of automatic detection or safe concurrent editing |

Help states the required reader floor and warns against editing while SpectroCapture has the file open. SQLite can detect external commits; automatic detection and its response remain OQ 14, and this journey does not select a library or notification mechanism.
