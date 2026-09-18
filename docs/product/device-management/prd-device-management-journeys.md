# Device Management PRD — acceptance scenarios

Companion to [the PRD](prd-device-management.md). Requirement rows own behavior; these scenarios supply starting conditions, actions and observable results, not additional rules. Existing UJ headings remain stable link targets. `R` IDs refer to the PRD; `E` IDs refer to [shipping copy](prd-device-management-copy.md#error--state-copy).

Run device/core assertions against the mock; use shell doubles for OS and interaction assertions. R6.21 separately requires the real generated-boundary contract suite and human live-device conformance results. Configure every exercised seam explicitly (R6.25), including nonzero dogfood timing until OQ 10 closes; hardware assumptions remain provisional.

## Device workflow

This transition index replaces the narrative diagrams. The scenarios below supply acceptance cases; the linked rows remain authoritative.

| Starting state / event | Result | Rows |
| :--- | :--- | :--- |
| Picker opened, no credential | Demo available; live discovery requires E6 | R1.4, R6.1–R6.3 |
| Live activation: invalid / expired | E4 / E5; no live discovery | R2.1–R2.3 |
| Active license; explicit Add Device | Discovery, then selected-device pairing | R1.1–R1.5 |
| Later launch, active license, known live device | Auto-reconnect that identity; E16 while unreachable, or E2 under the permission condition | R1.10, R1.14–R1.16 |
| First device authorization: offline / unreachable service / distinguishable serial refusal | E3 / E10 / E8 respectively | R2.5, R2.16; OQ 18 |
| Pairing failure; Try again | Retry selected-device pairing (E12); E13 at failure limit in P1 | R1.23, R1.8 |
| Calibration failure; retry | Repeat calibration (E14); E15 at failure limit in P1 | R3.1, R3.5 |
| Leave setup from E3/E12/E13/E14/E15 in P1 | First run: normal device panel; otherwise collection; reset relevant failure count | R1.8, R3.5 |
| Connected | Advisory readiness check; no automatic capture | R4.2 |
| First capture action against loaded queue | Blocking pre-flight; start only when no check blocks | R4.1–R4.4 |
| Effective offline window expired | E17 offline / E18 online; Check again uses R2.20; returning connectivity attempts renewal | R2.12, R2.15, R2.20 |
| Active session; disconnect / silence / low battery / save failure | Halt; suppress scan acceptance; discard partial set or hold completed failed-save reading | R5.1–R5.16 |
| All halt causes cleared | Enable Resume scanning; remain halted until explicit Resume | R5.7–R5.9 |
| Process terminated; Resume capture after relaunch | New session, full pre-flight, then-connected instrument bound | Capture R7.12/R7.14; PRD §5 definition |

## User Journeys

### UJ 1. First run

| Case | Given | When | Assert | Rows |
| :--- | :--- | :--- | :--- | :--- |
| UJ1-a | Clean persisted state; live-device fixture; online device authorization; calibration success | Choose file location, enter both credential strings, discover and pair, calibrate | Activation precedes discovery, activation makes no network call, first serial authorization is online, known record persists, readiness evaluates | R1.4, R1.9, R2.1, R3.1, R4.2, R6.20; Data Foundation R1.8 |
| UJ1-b | Invalid/incomplete or expired local credential | Activate in each variant | E4 or E5 respectively, never an internet error; no live operation starts | R1.4, R2.2, R2.5 |
| UJ1-c | No discoverable device; clock configured | Run discovery to DISCOVERY_TIMEOUT, then Search again | E1 in non-modal picker; retry performs discovery; panel and picker navigation remain usable | R1.5, R1.23, R6.29 |
| UJ1-d | Discovery entries over USB/BLE with known signal strengths; serial unavailable before connect | Inspect list, connect, then read identity | Strongest signal first; provisional name/transport before serial; final `(kind, model, serial)` identity deduplicates existing known record and updates handles | R1.3, R1.6, R1.7, R1.17, R6.14 |
| UJ1-e | Selected entry; pairing failures configured | Fail once, Try again, then fail through PAIRING_FAILURE_LIMIT | E12 retry targets pairing, not discovery; P1 E13 at limit; P0 withholds P1 state/action but keeps retry and exit guidance | R1.23, R1.8; copy phase convention |
| UJ1-f | First-run E3/E12/E13/E14/E15; nonzero relevant failure count | Navigate away incidentally; return; separately invoke P1 Leave setup for now | Navigation stays operable and resets count; explicit exit dismisses to normal device panel; repeat with a saved device to assert collection destination | R1.8, R3.5, R6.29 |

### UJ 1.1 Cannot complete a first run

| Case | Given | When | Assert | Rows |
| :--- | :--- | :--- | :--- | :--- |
| UJ1.1-a | Clean state, chosen file location, valid credential, offline network, Bluetooth denied | Activate, discover | Activation succeeds offline; E2 offers settings and USB; USB discovery does not depend on Bluetooth permission | R1.3, R2.1, R6.15; Data Foundation R1.8; OQ 24 still owns permission recovery timing |
| UJ1.1-b | Same state, discovered USB device needing first serial authorization | Pair; retry while offline; navigate away | E3, not invalid-license; state is non-modal; P1 exit present and P0 guidance remains | R2.5, R1.8, R1.23 |
| UJ1.1-c | Active license, online; service unreachable or distinguishable serial refusal injected separately | Authorize | E10 or E8, not generic failure; no promise that internet alone fixes refusal | R2.5, R2.16, R6.10; OQ 18 gates live distinguishability |

### UJ 1.2 First run with no hardware (Contributor)

| Case | Given | When | Assert | Rows |
| :--- | :--- | :--- | :--- | :--- |
| UJ1.2-a | Plain clone, no hardware/key, explicitly configured mock and shell state | Choose file location, select Demo, pair/calibrate, capture | No credential prompt or activation/authorization invocation; all four checks reported, authorization not applicable; nonzero default pacing; saved reading and capture indicator simulated | R6.1–R6.7, R6.17, R6.20, R6.27; Data Foundation R1.8 |
| UJ1.2-b | Demo spectral switch absent; other checks pass | Start session and save | Capture runs without spectral data, with persistent Demo variant of Capture E43 and the recorded non-spectral basis; repeat available variant | R2.6, R6.9; Capture R4.22/R4.24/R4.26 |
| UJ1.2-c | Simulated canonical reading, then live correction | Browse and export canonical/history | Canonical badge/banner follows current provenance; history retains simulated provenance; known exported snapshots yield true/false and unknown snapshots empty | R6.4–R6.5; Export R1.2 |
| UJ1.2-d | One required seam deliberately unconfigured | Exercise it | Test fails, no plausible default response | R6.25 |

### UJ 2. Start acquisition

| Case | Given | When | Assert | Rows |
| :--- | :--- | :--- | :--- | :--- |
| UJ2-a | Persisted last-used live device, valid stored credential, passing health | Relaunch, connect, then first capture action | Silent offline activation, automatic reconnect, advisory at connect; blocking pre-flight repeats at first action; no extra confirmation when checks pass | R1.10, R2.1, R4.1–R4.2 |
| UJ2-b | Last-used live device absent | Launch, then make it present | E16 and background discovery; connects without dismissal; denied/undetermined Bluetooth plus no reachable USB instead gives E2 | R1.14 |
| UJ2-c | Auto-reconnect probes candidate with different serial | Complete probe | Release immediately, never show as connected or bind to session | R1.16 |
| UJ2-d | Background attempt in flight | User initiates connect/switch; deliver cancellation/outcome through controlled seam | Background attempt cancelled; at most one connect/disconnect/retry in flight; cancelled attempt cannot switch the selected identity | R1.12, R1.15, R6.17 |
| UJ2-e | Active session bound to A; B available | Request switch, inspect confirmation; recover A over another transport | E31 names A and B; B cannot take over the active session; same-identity recovery may use available transport, with transport visible | R1.11–R1.13, R6.19 |
| UJ2-f | No active session; known live device reachable over USB and BLE | Connect in P1, then remove selected transport during capture | Prefer USB under provisional OQ 9 rule; no live transport migration; loss raises halt | R1.11, R6.19 |
| UJ2-g | Configured pre-flight battery/storage boundaries | Inject pass/warn/block, then applicable remediation | Advisory at connect, block prevents new start, warn does not; storage checks active-file/default-file volume; unknown battery polling retains last-known value and age | R4.1–R4.6; values remain OQ 4/4b/5 gated |
| UJ2-h | Muted / not muted / not determinable shell audio variants | Evaluate pre-flight | Advisory only for verified muted; no fifth check and no pass/warn/block value | R4.1, R6.9 |
| UJ2-i | Last-used device is Demo | Relaunch | No automatic Demo connection; explicit picker selection required | R1.10, R6.6 |

### UJ 3. Remove a saved device

| Case | Given | When | Assert | Rows |
| :--- | :--- | :--- | :--- | :--- |
| UJ3-a | Known device with canonical measurements and history | Confirm Remove in P1, then re-add | E32 names device; only saved connection record removed; immutable kind/model/serial/firmware snapshots remain; re-add uses normal pairing | R1.18–R1.22 |
| UJ3-b | Simulated and live fixtures share model/serial | Save both identities and acquire readings | Separate records by kind; firmware belongs to snapshots, not identity key | R1.21–R1.22 |

### UJ 4. Calibrate a connected device

| Case | Given | When | Assert | Rows |
| :--- | :--- | :--- | :--- | :--- |
| UJ4-a | Connected device | Request calibration on demand; inject success, then separate failure | Guided tile placement; success recorded; E14 and retry on failure | R3.1–R3.3 |
| UJ4-b | SDK not due; elapsed time exceeds candidate pre-emptive interval | Evaluate gate before OQ 3 closes; repeat SDK due | Elapsed candidate alone does not trip interim gate; due signal shows E21 before start | R3.3, F11 |
| UJ4-c | Active session | Change SDK due signal | No calibration interruption or countdown in capture; currency is ambient in panel | R3.4 |
| UJ4-d | Calibration failures | Reach CALIBRATION_FAILURE_LIMIT in P1, then retry/leave | E15 with retry and exit; success or navigation resets count; first-run exit returns normal panel | R3.5, R1.8 |

### UJ 5. Device failure during a session

| Case | Given | When | Assert | Rows |
| :--- | :--- | :--- | :--- | :--- |
| UJ5-a | Active session, success tone in flight, incomplete multi-sample set | Inject disconnect, silence, or low battery separately | Halt on configured detection boundary; full-surface state and interrupting tone pre-empt success; partial set discarded; scan attempts suppressed | R5.1–R5.5, R5.13–R5.16 |
| UJ5-b | Disconnect halt, bound identity absent | Reach attempt or duration bound; later restore device | E24 with no active retries; listening continues; Try reconnecting now retries; return enables Resume only, never auto-resumes | R5.6–R5.9 |
| UJ5-c | Disconnect raised before storage failure | Clear disconnect while storage still fails, then clear storage | First unresolved cause displayed at each step; Resume disabled until all clear; explicit Resume required | R5.7–R5.10 |
| UJ5-d | Completed reading; disk-full / unreachable / unexpected save failure | Retry save, fail again, then succeed; Resume | E27/E28/E29 respectively; same reading retained/retried, no new measurement; durable write before advance; resume does not recapture saved row | R5.2, R5.8–R5.10 |
| UJ5-e | Held failed-save reading | End or quit; cancel, then repeat and confirm | Warning first; cancel preserves halted session/reading; confirmed End discards only held reading and leaves its row pending; quit path available in P0 | R5.11, R5.18; Capture R7.5 |
| UJ5-f | Battery halt | Connect power without recovered level, then raise level above resume threshold | Power alone does not clear; explicit level check used when polling unavailable; resume threshold strictly above halt | R5.14; OQ 4/4b |
| UJ5-g | Unknown SDK error | Inject error; then successful device command | E30; clears on successful command, still explicit Resume; every enumerated known error has an injection | R5.7, R6.22–R6.23 |
| UJ5-h | Active session with remembered/current row; separately jump/reorder beforehand | Halt and recover | Capture's current-row rules preserved; no reset to queue head; incomplete set restarts at sample 0 | R5.8, R5.15; Capture R6.2/R7.14 |
| UJ5-i | Open halt and durable prior rows | Terminate process, relaunch, Resume capture | Old halt closes unresolved—app terminated; Capture interruption recovery creates new session with full pre-flight and then-connected instrument, preserving saved rows | R5.17; Capture R7.7/R7.12/R7.14/R7.18 |
| UJ5-j | Active session in P1 | Sleep, let authorization/calibration fall due, wake | Same-session halt, not interruption; other health causes may arise; due authorization/calibration alone do not interrupt; explicit Resume | R5.19, R2.17, R3.4 |
| UJ5-k | Store unavailable when halt occurs | Attempt halt-log write, then restore store | Logging failure does not cascade/block; save-failure halt record persists when writable; record contains required fields and resolution | R5.12 |
| UJ5-l | Assistive technology active in P1 | Raise halt | Announcement without focus; capture surface remains navigable | R5.20 |

### UJ 6. Pre-authorize before going offline

| Case | Given | When | Assert | Rows |
| :--- | :--- | :--- | :--- | :--- |
| UJ6-a | Cache/license expiry independently set, including both orderings and equality | Evaluate displayed/gating date | Earlier date used consistently; derived date labelled approximate | R2.9, R2.15, R6.10 |
| UJ6-b | Online valid short window in P1; renewal supported in fixture | Extend explicitly; separately cross configured renewal trigger | Date updated on success; background renewal unobtrusive; failure retries at 1 min → 10 min → 100 min → daily in virtual time | R2.10–R2.11, R6.11, R6.17; live capability OQ 8 |
| UJ6-c | Offline in P1, window valid / lapsed | Extend offline use | E11 uses correct date variant; no spinner-only or silent failure | R2.18 |
| UJ6-d | Session started inside valid window | Advance clock beyond expiry | App lets current session complete; next session blocked; simulated SDK refusal instead follows halt path, with live enforcement still OQ 11 | R2.17; OQ 11 |
| UJ6-e | Offline activation, Demo, or telemetry-off configurations | Exercise corresponding flow | Zero activation network attempts; Demo zero activation/device-auth invocations; telemetry-off zero telemetry attempts; permitted app paths remain distinguishable from vendor SDK analytics | R2.8, R2.13, R6.17, R6.24 |

### UJ 6.1 Authorization window expired while offline

| Case | Given | When | Assert | Rows |
| :--- | :--- | :--- | :--- | :--- |
| UJ6.1-a | Expired effective window, offline, P0 | Attempt start; Check again while offline | E17, no session starts, no renewal network attempt; browsing/export remain available | R2.12, R2.20; E17 |
| UJ6.1-b | E17 visible | Restore connectivity; separately Check again online | E18 while expired; silent/explicit renewal attempted; valid effective window clears expiry block; other pre-flight checks still apply | R2.12, R2.15, R2.20 |
| UJ6.1-c | Renewal fails, with unreachable service / distinguishable serial refusal / still-expired window | Check again online in each variant | E10 / E8 / E18 respectively; no guaranteed-success promise; P0 E18 retains Check again and does not instruct use of a withheld control | R2.16, R2.20; OQ 18 |

## Test controls and observations

This maps existing requirements to test surfaces; it does not choose module layout (OQ 21 / ADR-0005).

| Surface | Controlled input | Observable result | Rows |
| :--- | :--- | :--- | :--- |
| SpectroDevice mock | Identity, transports, discovery fields, signal strength, calibration, battery, errors, deterministic measurements, spectral switch, latency, held-open measurement | Identity/binding, transport used, measurement requests, outcomes and error parity | R6.9, R6.14–R6.16, R6.19, R6.22–R6.23, R6.27; Capture R11.5/R11.8 |
| Licensing / authorization doubles | Local validation outcomes; device-service outcomes; both expiries; reachability; virtual clock | Local activation log separate from outbound destination/time/payload; expiry choice; renewal attempts | R6.10–R6.12, R6.17 |
| Storage / persisted-state harness | Nth-write and commit failures, unavailable store, free bytes, initial saved devices/credential/calibration/file location | Same held reading retried, durable-before-advance, restart state, local halt records | R6.13, R6.18, R6.20; R5.2/R5.10/R5.12 |
| Shell doubles | Bluetooth permission, credential store, audio state, picker and panel interactions | Present/enabled entry points and invocation outcomes, cue kind/time/pre-emption, non-modal navigation | R6.9, R6.15, R6.17, R6.29 |
| Conformance and release gates | Same applicable behavioral cases; pinned SDK error enumeration; recorded hardware fixtures in P2 | Mock CI, actual generated-boundary CI, named hardware-owner results; usage-string check; hardware OQs retained | R6.8, R6.21–R6.23, R6.26, R6.28 |

Unknown SDK capability/state is not silently converted to a passing reading: apply only the documented interim rule, otherwise retain its OQ as a build gate. OQ 3 still decides unknown calibration-state handling; OQ 4/4b still determine battery behavior where no usable level exists.
