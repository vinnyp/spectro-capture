# Capture Mode PRD — acceptance scenarios

Companion to [the PRD](prd-capture-mode.md). Requirements own behavior; these cases provide fixtures, actions, observable results and source rows. Existing UJ headings remain stable link targets; R IDs are Capture unless qualified, E IDs refer to [Capture copy](prd-capture-mode-copy.md#error--state-copy). Device E IDs are always qualified. Scenarios do not add rules or settle OQs.

Use Device's simulated seam plus Capture R11.5–R11.16 and the Data Foundation readback/fault controls. Declare every exercised seam input under Device R6.25; only noninstant default latency is exempt. A comparison against SAMPLE_TOLERANCE declares below/equal/above fixtures, chosen mode and reference, not values copied from the implementation under test. A display-reference variation uses a pair available in the configured capability fixture; it never asserts a live pair unsupported by OQ 21.

## User Journeys

P0 is bulk capture and bulk deferred-row review. P1 is captured-value correction, ad-hoc/one-row capture and reordering; withhold their actions/variants in P0 as F47 requires. Unless specified otherwise, an assertion concerns the test's existing session, never a carry-over rule for separate one-row sessions (OQ 3).

### The session, end to end

Transition index only; session status is active/interrupted/ended/complete (R3.2), while operator pause, guard pause and device halt are operating states within the active session. The table does not choose a stored representation.

| Case | Given | When | Assert | Rows |
| :--- | :--- | :--- | :--- | :--- |
| T1 | No active session; remembered row pending (or first session); passing readiness | Start capture session | Gate runs; one active session binds connected instrument; remembered/fallback pending row at sample 0 | R3.1–R3.10 |
| T2 | Another collection active, operator-paused, guard-paused or device-halted | Start capture on this collection | E3 names holder; no second active session; interrupted session on another collection does not block | R3.2/R3.5 |
| T3 | This collection already in flight | Start capture again | Return to the existing session; no second record | R3.5 |
| T4 | Same live bulk session; pending rows remain / no pending rows | Open review | Detour / end-of-run according to R8.1; entry mode holds until list closes | R8.1/R8.6 |
| T5 | Bulk session / one-row session cut short | Relaunch | Bulk offers Resume capture or closes complete if finished; one-row closes ended without Resume | R3.13/R7.8/R7.11 |

### Cluster 1 — Set up a place to capture into

### UJ 1. Create a collection

UJ1.1 was folded into UJ1 under F1. Hardware capabilities and the complete selectable pair list remain OQ 1/OQ 21; fixtures do not close them.

| Case | Given | When | Assert | Rows |
| :--- | :--- | :--- | :--- | :--- |
| UJ1-a | Clean file | Create named collection with default settings | Empty queue; N=3, scan mode M1 on Spectro 2, display D50/2°; no library | R1.1/R1.3/R1.5/R1.10 |
| UJ1-b | Existing collection name | Create names differing only under Import R2.3 equivalence; also try empty name | Duplicate gives E1 and one collection remains; empty name cannot create collection | R1.2 |
| UJ1-c | Creation form | Try N=1,3,5 and out-of-range values | Accept only 1–5; N=1 skips agreement check and E18; defaults editable between sessions | R1.3/R1.4 |
| UJ1-d | Empty collection / finished collection with settled rows / no pending but unsettled rows | Open collection; request capture where offered | E2 empty / finished variants; review offer iff any set-aside row; unsettled-only start gates then opens review | R1.1/R1.7/R3.9/R8.16 |
| UJ1-e | Saved samples and spread; spectral / non-spectral fixtures | Change display reference between available pairs | No re-scan, no measurement or agreement-verdict mutation; spectral derivation follows selected reference; non-spectral stays at original reference with required mark | R1.5/R4.9/R4.12/R4.24; DF R3.3/R3.5 |
| UJ1-f | Supported scan modes configured; saved reading includes all modes | Change chosen mode between sessions | All returned modes remain stored; chosen mode selects working values and later agreement checks; no re-scan or prior-reading mutation | R1.6/R1.10/R4.5/R11.8 |

### UJ 1.2 Manage collections — rename, delete

Collection Mode owns these controls; these are inherited contract cases, not Capture-owned UI requirements.

| Case | Given | When | Assert | Rows |
| :--- | :--- | :--- | :--- | :--- |
| UJ1.2-a | Two collections; no conflicting session | Rename one to a unique name, then to the other’s equivalent name | Unique rename persists; duplicate gives E1 and preserves original name | R1.2/R1.8; Collection Mode obligation |
| UJ1.2-b | Collection with current readings/history; active or interrupted session variants | Request deletion | Session must end first; warning names captured/history counts, offers export first; deletion never default | R1.8; DF R6.2 |
| UJ1.2-c | No session blocking deletion; confirmation open | Cancel / export first / confirm deletion | Cancel preserves collection; export does not delete; confirmed deletion follows phase-specific DF contract | R1.8; DF R6.2/R6.2a–c |

### Cluster 2 — Import an inventory

UJ2/UJ2.1/UJ2.2 moved under F49 to [Import acceptance scenarios](../import/prd-inventory-import-journeys.md); use that contract for CSV entry, session blocking and E40 return routes.

### Cluster 3 — The bulk session (the thesis)

### UJ 3. Run a bulk capture session

| Case | Given | When | Assert | Rows |
| :--- | :--- | :--- | :--- | :--- |
| UJ3-a | Pending A/B; N=3; guard and agreement record-only; nonzero latency | Accept three triggers on A, then three on B | One measurement per accepted trigger; sample confirmations through two senses; each durable set precedes row confirmation and one advance; captured +2, pending −2, deferred unchanged | R4.1–R4.8/R4.12–R4.15/R4.23/R11.3 |
| UJ3-b | N=3; enabled agreement; independently declared sample-to-mean distances | Complete sets below/equal to/above SAMPLE_TOLERANCE | Below/equal agrees; above shows E18 and holds; fixed D50/2° independent of display reference; saved samples retained individually | R4.9/R4.12/R11.8 |
| UJ3-c | Above-tolerance set; agreement record-only / enabled | Complete set; in enabled run take each E18 queue action separately | Record-only accepts and records spread without prompt; Take it again repeats and counts failed sets; Accept the average saves spread; Set it aside retains samples and advances once | R4.10/R4.11/R4.23/R5.4 |
| UJ3-d | Measurement held open by seam; extra dead-time configured | Press repeatedly during measurement and configured post-result lockout | No queued/second measurements; in-flight presses have distinct rejection cue; accepted-trigger count equals requested-measurement count | R4.2/R4.3/R11.5 |
| UJ3-e | N=3; complete set ready; store save held/fails | Finish last sample; retry successful save later | No row-success or queue advance until durable save; completed set held through failure; successful retry yields one captured row and one advance | R4.12/R4.13/R7.2; Device R5.9/R5.10 |
| UJ3-f | Measurement started on A; action leaves A unable to take samples | Hold return; exercise Flag, Skip, Pause, jump and P1 Add separately; deliver late result | Result never attaches to another row; discarded late sample logged against A’s attempt; saved-row attribution is checked against accepted-trigger row | R4.18/R11.5/R11.6; M6 |
| UJ3-g | Saved confirmed rows and one partial current set; store-loss injection | Lose unflushed writes or detach volume, then reopen | Every confirmed row present once; partial set not restored as confirmed; appropriate halt/unavailable state; no success for failed save | R4.13/R7.7/R11.10/R11.11 |
| UJ3-h | Each surface focused in turn; shortcut map available | Deliver capture shortcuts, including trigger | Capture shortcuts inert off capture surface; none bound to bare Space/single letter; capture surface offers no Cancel/Abandon; End separate from advance controls | R4.4/R4.16/R10.4/R11.13/R11.15 |
| UJ3-i | Spectral absent / present Demo configurations | Start session and scan | Absent shows persistent E43 Demo variant and still captures with non-spectral basis/version; present omits E43; simulated indicator E35 always present and provenance retained | R4.22/R4.24/R4.26/R11.3; Device R6.4–R6.6 |
| UJ3-j | ROWS_TARGET/ROWS_CEILING and SESSION_LENGTH fixtures with declared budgets | Find/list at ceiling; import at target; hold session over outlasting queue | Meet FIND_BUDGET/IMPORT_BUDGET and last-row TRIGGER_ACK_WINDOW; tallies, elapsed time and order remain correct | R3.12; M8; Import R3.1 |
| UJ3-k | Virtual pacing clock plus independent real clock; observable cues/announcements | Run configured paced captures and induced failures | Record both clocks and cue kind/pre-emption; assert timing budgets and configured distinct cues; VoiceOver policy remains OQ14 and haptic exposure remains hardware-gated | R4.7/R4.8/R4.13/R4.17/R11.7/R11.14; M1/M10 |

### UJ 3.1 A scan fails mid-queue

K and N are declared fixture values, not new release choices. “Sample counter” and “failed-attempt counter” are different observations. Guard lifetime across distinct one-row sessions remains OQ 3; the cases here assert within one bulk session only.

| Case | Given | When | Assert | Rows |
| :--- | :--- | :--- | :--- | :--- |
| UJ3.1-a | Partial good samples; light-leak/temperature/drift injected separately | Trigger failed sample, then retry | E15/E16/E45 respectively; two-sense caution pre-empts success; same row and good samples retained; failed-attempt +1, no advance | R5.1/R5.3/R5.4; Device F31 |
| UJ3.1-b | K_FAILED_ATTEMPTS=3; guard record-only | Refuse twice, accept a sample, refuse again on same row | Third failure auto-defers once (E17); accepted sample did not reset count; deferred +1, pending −1; samples/causes retained | R5.4/R5.13 |
| UJ3.1-c | K=3, N_CONSEC_HARD=2, guard enabled; enough pending rows | Auto-defer two successive rows using three drift refusals each; repeat with two drift+one light refusal each | Each refusal counts once; each deferral counts one guard row; E19 pause only after second row; held row/samples intact; no separate drift bound | R5.4/R5.9–R5.14; Device F31 |
| UJ3.1-d | Same failures, guard record-only | Repeat UJ3.1-c | Same counts and deferrals, no guard pause | R5.12 |
| UJ3.1-e | Guard enabled; one held row; failed sample / partial samples without failure | Skip in each variant | Both defer and advance once, samples retained; only Skip after a failed attempt counts toward guard | R5.5/R5.9/R5.10 |
| UJ3.1-f | At least seven pending rows; guard enabled | Flag each as missing before samples | Five set-aside rows, no guard count/pause; subsequent two counted deferrals pause exactly then | R5.8/R5.10 |
| UJ3.1-g | Just captured A, no trigger on next row yet; separate fixtures after auto-deferral or unattempted Skip | Flag during each flag window | Captured A demoted with prior reading in history; otherwise E20 already-set-aside/still-pending and no new row changed | R5.6/R5.7 |
| UJ3.1-h | Guard paused with partial samples and row failure count | Press trigger; then force-resume | Trigger requests no measurement; deliberate force-resume clears guard count only, retains current samples and row failure count | R5.11 |
| UJ3.1-i | Guard enabled N=2; agreement enabled; same bulk session | One auto-deferral then a Skip after failure; repeat with disagreement set aside as second route | Mixed routes count together and pause once at second row | R5.9/R5.14; F48 |
| UJ3.1-j | Guard count=1 in enabled bulk session | Capture a row, including by Accept the average; then one counted deferral | Captured row resets count; later one deferral does not pause | R5.14/R4.11 |
| UJ3.1-k | Enabled same bulk session; final pending row auto-defers, guard count=1; review opens | In review Skip a different row after failure | Queue→review boundary does not reset count; second counted row pauses | R5.9/R8.4 |

### UJ 3.2 Undo or redo the current item

| Case | Given | When | Assert | Rows |
| :--- | :--- | :--- | :--- | :--- |
| UJ3.2-a | Pending A with two accepted samples, no scan in flight | Re-take sample / Restart item separately | Re-take keeps first sample, counter 1; Restart counter 0; A stays pending/current; no failure or guard count increment | R4.19–R4.21 |
| UJ3.2-b | Measurement held in flight | Re-take sample / Restart item | Reject with existing rejection cue; no sample discarded or failure counted | R4.21 |
| UJ3.2-c | Just-captured A / F16 window after deferral / after unattempted Skip | Invoke either action | E36 / E44 already-set-aside / E44 still-pending respectively; no next-row mutation or spurious attempt | R4.21/R5.7 |

### UJ 3.3 Resolve the deferred-error queue at session end

Cases involving one-row or captured-row re-scan behavior are P1; ordinary bulk deferred-row review is P0. R8.1d forbids scanning while held by guard/device. The matrix remains the single transition authority, with scenarios testing its cells rather than supplying exceptions.

| Case | Given | When | Assert | Rows |
| :--- | :--- | :--- | :--- | :--- |
| UJ3.3-a | For each R8.1a–k entry fixture; partial set where specified | Open list, select a row where allowed, then leave | Assert every owning matrix cell: gate/session behavior, discard or retention, remembered/review-selected state, return target and session outcome; repeat with collection vs capture entry where available | R8.1a–k/R3.6–R3.9/R7.1/R7.16 |
| UJ3.3-b | Settled and unsettled rows with retained samples/causes/notes, including drift | Open list and select eligible row | All rows listed with cause/attempt/sample counts and settlement note; prior values hidden during acquisition; full fresh N samples; earlier samples remain history | R8.2/R8.3/R11.15c |
| UJ3.3-c | Review row with sample refusal; separate disagreeing set; guard/check enabled | Retry, Skip, exhaust K, or use E18 review action in separate runs | Failure holds; counted exits follow R5.9; unsettled deferrals retain prior causes plus new cause; settled rows offer no Leave it set aside | R8.4/R8.5/R5.9 |
| UJ3.3-d | Review row unsettled; guard count below threshold; non-disagreeing failed attempt | Leave it set aside with optional note (E27) | Settled, retained samples/note; no increment toward guard for this route; ordinary failed-attempt record remains | R8.4/R8.5/R5.9 |
| UJ3.3-e | Mixed settled/unsettled rows; one partial current row | Cancel then confirm Leave them all set aside (E39) | Cancel retains state; confirm settles only outstanding rows, common note; discards partial set only if current row is among them with cue/E42; no touching already-settled decisions | R8.15/R7.16/R7.17 |
| UJ3.3-f | No pending rows; final unsettled row; live bulk / interrupted bulk / ended session variants | Capture or deliberately settle last outstanding row | Live completes; interrupted closes complete without gate/Resume; ended session stays ended; appropriate summary still leaves collection entry points reachable | R8.6/R7.11/R7.19/R8.1 |
| UJ3.3-g | End-of-run vs detour review; unresolved rows remain | Leave list | End-of-run ends early with unresolved rows retained; detour returns to held queue row at sample 0 and continues session | R8.6/R8.1f–k |

### UJ 3.4 Pause and end a session early

| Case | Given | When | Assert | Rows |
| :--- | :--- | :--- | :--- | :--- |
| UJ3.4-a | Partial current set; active session | Pause; press trigger; Resume | Distinct discard cue, E22 with counts/current row/dropped samples; trigger does nothing; same session resumes at sample 0 | R7.3/R7.16/R7.17 |
| UJ3.4-b | Active session; partial set, optional deferred rows; next location row/review variants | Open End session offer; Keep scanning / confirm separately | Offer preserves samples on cancel; confirmed End discards partial set and retains row states; E23 conditional lines and next-location variant match; review action iff deferred rows exist | R7.1/R7.5/R8.16/R11.12 |
| UJ3.4-c | Session completes with/without deliberately-set-aside rows | Read E24 then Done | Correct tallies/time/rate; set-aside line iff count nonzero; Done dismisses summary only; collection entry points remain | R7.15/R7.19/R11.12/R11.15f–g |

### UJ 3.5 Resume an interrupted session

| Case | Given | When | Assert | Rows |
| :--- | :--- | :--- | :--- | :--- |
| UJ3.5-a | Bulk active or operator-paused; confirmed A; partial B | Quit/crash/force-quit/power-loss, then relaunch | Confirmed A persists once; B partial samples gone; E25 correct tallies/location; instrument binding released | R3.3/R3.4/R7.7/R7.8 |
| UJ3.5-b | Interrupted bulk; remembered row pending/nonpending; review-selected variants | Resume capture | New linked session after full pre-flight; connected instrument bound and change reported; target follows R3.7/R3.8; failed gate starts nothing | R3.7–R3.10/R7.12/R7.18 |
| UJ3.5-c | Interrupted bulk with no pending rows; unsettled / all settled | Relaunch and resume where offered | Unsettled gives E25 list variant then gated review; all settled closes complete, no gate or Resume, summary on collection | R7.11/R7.13 |
| UJ3.5-d | Saved collection unavailable / remembered-open collection missing from app preferences | Relaunch | Unavailable file gets E26 Find the file; otherwise manually opening file restores session state solely from file | R1.9/R7.8/R7.9 |
| UJ3.5-e | Interrupted bulk; collection surface | End that session | Ends without opening capture; rows retained under ordinary ending contract | R7.13/R7.5 |
| UJ3.5-f | P1 one-row session interrupted with pending / captured-before-re-scan row | Relaunch | One-row session ended, no Resume; existing canonical retained for interrupted correction; bulk interruption unaffected | R3.13/R8.8/R9.7 |
| UJ3.5-g | Three linked sessions with own capture times 10,5,7 minutes and distinct newly captured rows 2,1,3; no flag/demotion/rework | Resume twice and read chain summary/records | Per-session own intervals/outcomes remain attributable; cumulative displays 10/15/22 minutes and 2/3/6 rows; final M2 uses 6 rows / 22 minutes, not summed cumulative displays | R3.1/R7.18/R11.11; M2; F53 |
| UJ3.5-h | Active recording with configured clock | Advance time during capture, operator pause, guard pause, halt, review and between interrupted sessions | Elapsed time includes only capturing intervals; every sample/attempt/discard/decision retains session and event time | R3.1/R8.17/R11.11 |

### UJ 3.6 Device fails mid-session

Device owns detection, causes, reconnect bounds and shipping halt copy. These cases test Capture at that boundary; live device-path changes require the hardware gate. Sleep is simulated only by disconnect plus changed readiness, not by claiming a simulated OS sleep.

| Case | Given | When | Assert | Rows |
| :--- | :--- | :--- | :--- | :--- |
| UJ3.6-a | Active bulk, partial set; each Device halt cause or sleep stand-in | Induce halt, press trigger, recover deliberately | No measurement while halted; partial set discarded with cue; row/counters/recents visible; recovery same bound session and current/fallback row at sample 0 | R3.4/R4.2/R7.1/R7.14/R11.10; Device §5 |
| UJ3.6-b | Completed set held by failed save | Try saving again successfully; Resume scanning | Held set commits once and confirmation advances once; resume reevaluates current row after save | R7.2/R4.13; Device R5.8–R5.10 |
| UJ3.6-c | Halt with/without held unsaved reading | Device E33 End/Quit, cancel then confirm in separate runs | Cancel retains halt/set; End discards unsaved set and gives E23 halted-ended (no Keep scanning); Quit closes/exits with no summary or next-launch Resume | R7.5; Device R5.11/R5.18/F28 |
| UJ3.6-d | Open halt record; confirmed rows plus unsaved data | Crash/force-quit/power-loss; relaunch | Halt closes unresolved—app terminated; bulk interrupted; confirmed rows safe; Resume starts new session through full gate | R3.4/R7.7/R7.12; Device R5.17/R5.19 |

### UJ 3.7 Jump to a different row

| Case | Given | When | Assert | Rows |
| :--- | :--- | :--- | :--- | :--- |
| UJ3.7-a | Pending/set-aside/captured rows with codes/names/alternates | Find by code prefix, name substring, either alternate, and nonmatch | Order pending then set-aside then captured; Import equivalence; alternate-only match E41 correct variant; nonmatch E37; typing creates no rows | R6.1/R6.3 |
| UJ3.7-b | Partial A and pending B | Select B; later resume collection | Discard A samples with cue/E42 naming A; A pending; B current/remembered sample 0; queue order unchanged; resume respects B | R6.2/R3.6/R7.17 |
| UJ3.7-c | Partial A; captured B; P1 | Select B then cancel E28; repeat and confirm | Cancel preserves A samples; confirm discards them before re-scan; B existing value hidden | R6.3/R7.1/R8.7 |
| UJ3.7-d | Pending A/B/C; pass anchor A | Skip/jump unattempted rows; separately defer/move anchor during pass | Skips stay pending, wrap revisits; moving/removing anchor cannot loop forever; all-unattempted-skipped wrap gives E21 | R6.4–R6.6 |
| UJ3.7-e | E21 wrap exhausted | Flag remaining as missing / End session separately | Flag produces deferred rows with missing cause and opens review; End preserves pending rows | R6.6 |

### UJ 3.8 Re-scan an already-captured row

All captured-row re-scan cases are P1. A deferred-row acquisition in a live bulk review remains the P0 path in UJ3.3; do not phase it out as though it were correction of a captured value.

| Case | Given | When | Assert | Rows |
| :--- | :--- | :--- | :--- | :--- |
| UJ3.8-a | Captured row; P1; active/paused/no session/interrupted contexts | Accept identifier-based E28 offer | Correct session context; old value hidden; full N samples; paused bulk reused for one row; interrupted bulk not resumed | R8.7/R9.4/R9.6/R9.7 |
| UJ3.8-b | Captured A; replacement set succeeds | Commit re-scan | New canonical mean, all prior readings retained; confirmation names retained prior; capture-time reason correction-unconfirmed with no mid-loop question | R8.9; DF R2.3/R2.4/R2.8 |
| UJ3.8-c | Captured A with disagreeing or failing replacement | Exhaust K / Skip / Abandon the re-scan in E18 variant | A remains captured with old canonical; attempts retained, no deferral; no Set it aside action | R8.8/R8.10 |
| UJ3.8-d | Bulk A held while B re-scanned; active/paused variants | Finish re-scan | Return to A at sample 0; paused remains paused; B queue position unchanged | R8.12 |
| UJ3.8-e | QC & Comparison implemented; known canonical | Choose Check it / Correct it in E29 | QC never supersedes canonical; correction follows re-scan path; until QC lands, DF E11/E26 owns reason review | QC obligation; DF R2.4/R2.7/R2.8 |

### UJ 3.9 Capture with the Demo Device (Contributor)

Run 1–8 in order, resetting fixtures as declared; run 9 per phase. All use the configured noninstant DEMO_SCAN_CYCLE. The two settings are independent: the agreement check is explicitly enabled for disagreement paths even while the guard is record-only. No scenario treats mutable tallies as globally unchanged.

| Case | Given | When | Assert | Rows |
| :--- | :--- | :--- | :--- | :--- |
| UJ3.9-1 | No hardware/license; Demo selected; K=3; guard record-only; agreement enabled | Run UJ3.1-a/b for light/temperature/drift | State-specific caution/retry and auto-deferral; expected pending→deferred delta, good samples retained; no guard pause | R11.3/R4.23/R5.12/R11.6 |
| UJ3.9-2 | Fresh bulk; guard enabled N=2; agreement enabled | Run UJ3.1-c/i/j/k | Pause/reset/mixed-route/queue→review cases match expected row counts | R5.9–R5.14/R8.4 |
| UJ3.9-3 | Fresh bulk; guard enabled N=2; K=3 | Fail/accept/fail/accept/fail on one row with N samples large enough to avoid completing set | Only one auto-deferral; no session pause; failed count not reset by accepted samples | R5.4/R5.9/R5.13 |
| UJ3.9-4 | Review in same bulk; guard and agreement enabled; fresh counter for each independent route run | Run N_CONSEC_HARD deferrals by each counted route; separate runs of N_CONSEC_HARD unattempted Skips, leaves without attempt, and leaves after non-disagreeing failures | Counted routes each pause at threshold; each excluded-route run never increments counter or pauses; E18 review actions obey settled state | R5.9/R5.10/R8.4/R8.5 |
| UJ3.9-5 | Queue fixture with anchor removed from pending during pass | Run UJ3.7-d/e | Pass ends instead of circling; no duplicate queue membership; expected flag/skip deltas | R6.5/R6.6 |
| UJ3.9-6 | Declared failed-save/disconnect states; one ready-to-capture row | Recover then capture once | Retained set saves once; captured +1/pending −1; recovered row ownership correct | UJ3.6-a/b; R11.5/R11.6 |
| UJ3.9-7 | Bulk with confirmed rows and partial current row | Quit and relaunch; then Resume capture | Confirmed state/counts preserved across interruption; new linked session, cumulative accounting counted once; partial samples discarded | UJ3.5-a/b/g |
| UJ3.9-8 | Open device halt with confirmed rows | Force-quit, relaunch | Old halt unresolved, bulk interrupted; no duplicate rows or saved readings | UJ3.6-d |
| UJ3.9-9 | Both build phases; named state/variant fixtures | Enumerate R11.12 variants and R11.15a–g surfaces; invoke their actions | Conditional copy/actions present only for matching state and phase; E18/E42 re-scan bodies absent in P0; zero-count clauses omitted without suppressing nonzero counts | R11.12/R11.15; §12; F47 |

### UJ 3.10 Reorder the queue

| Case | Given | When | Assert | Rows |
| :--- | :--- | :--- | :--- | :--- |
| UJ3.10-a | P1; pending and nonpending rows; held partial set | Open queue list; drag/sort pending rows | Opening/reorder preserves partial samples; interim repositions pending rows only; captured/set-aside positions and all row states unchanged; queue length unchanged | R6.7/R6.8/R6.10–R6.12/R4.15 |
| UJ3.10-b | P1; A,A2,A10 and tied metadata values; manual order | Sort twice; cancel/confirm E34 separately | Natural numeric order, stable ties; second sort toggles direction; cancel retains manual order; confirmation replaces it | R6.9 |
| UJ3.10-c | P1 captured / set-aside row | Attempt drag | E33 scanned / set-aside variants offer proper recovery; no reorder | R6.10 |
| UJ3.10-d | Saved manual queue order | Relaunch/resume; browse sorted differently; re-import with new rows | Persisted order used for queue/wrap; browse sort never mutates it; import appends new rows | R6.7/R6.11; Import R3.2/R3.3 |

### Cluster 4 — Ad-hoc capture

### UJ 4. Capture a single new item into a collection

| Case | Given | When | Assert | Rows |
| :--- | :--- | :--- | :--- | :--- |
| UJ4-a | P1; no session; metadata form | Enter empty / equivalent duplicate / unique Swatch Code | E31 refuses blank; E30 offers re-scan or different code without duplicate insertion; unique save creates one pending row | R9.1–R9.3 |
| UJ4-b | Saved new metadata, no scan | Leave form then start later bulk | Row remains pending and joins bulk queue; no phantom saved reading | R9.3 |
| UJ4-c | New pending row; no session / interrupted bulk | Scan through passing gate | One-row session binds device, then ends on capture/defer/abandon; remembered bulk row unchanged; interrupted bulk not resumed | R3.11/R3.13/R9.4/R9.7 |
| UJ4-d | Same collection bulk operator-paused; P1 | Capture ad-hoc row | Use existing session/device, lift pause for this row without new start gate; afterwards held queue row returns still paused; tallies include new row | R9.6 |
| UJ4-e | Other collection active, paused or halted | Attempt scan | E3; no second active session | R3.5/R9.5 |
| UJ4-f | One-row ad-hoc run with failures | Exhaust K / Skip; separately complete row then try flag-after-landing | Failure/Skip defers with retained evidence and closes one-row session; no F16 window after one-row capture, correction via collection | R9.8/R9.9/R3.13 |

### UJ 4.1 Insert an unplanned item mid-session

The standalone path in UJ4-c starts a one-row session; this mid-session path uses the already-active bulk session. Insertion failure/deferral beyond the ordinary §5 rules must not imply a new session. INSERT_POSITION remains provisional (OQ 11).

| Case | Given | When | Assert | Rows |
| :--- | :--- | :--- | :--- | :--- |
| UJ4.1-a | P1; bulk A with partial samples | Add a swatch; cancel / submit duplicate or blank / save unique B separately | Cancel E32 retains A samples, no new row; invalid data creates nothing; unique save discards A partial set with cue, inserts B at configured position and makes B current sample 0 | R9.1/R9.2/R9.10/R9.11 |
| UJ4.1-b | New B inserted after held A; bulk session active | Capture B successfully | No second session or new start gate; B captured, R +1 relative to before insert; return A sample 0, continue queue order | R3.11/R9.10/R9.11 |

### Cluster 5 — The seam (made visible, not decided)

### UJ 5. Session ends and the collection is reviewed (the seam)

Prototype constraints only: A = full-window modal takeover; B = capture state of live collection. Neither is the shipping selection until OQ 8 and ADR-0004 close. Implementation planning still defines the fixed script and artifact; no prototype results are invented here.

| Case | Given | When | Assert | Rows |
| :--- | :--- | :--- | :--- | :--- |
| UJ5-a | Prototype A / B; same fixture, owner script and configured pacing | Capture, pause, reach last captured row, end; reorder once P1 lands | Same saved data/row states/vocabulary/counts under both; measure time/steps at pause/end, mode slips and review/recents placement; no winner inferred from research | R10.1/R10.3/R10.7/R10.8 |
| UJ5-b | A modal takeover / B collection-state prototype | Enter capture then move focus where available | A takeover provides mode indication; B at least two redundant indicators; off-focus shortcuts inert; navigation never ends session | R10.4/R10.5/R11.13/R11.15 |
| UJ5-c | Both prototypes; one owner, three runs each | Alternate which reading goes first using identical script | Record participant/run counts, raw slips and steps; fewer total slips wins, then fewer steps to just-captured row per precommitted rule; owner records product call for ADR-0004 | R10.6–R10.8; OQ8 |
