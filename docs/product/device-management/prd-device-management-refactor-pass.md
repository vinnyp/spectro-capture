# Device Management PRD — the F9 refactor pass

Resume point for fence F9 (`prd-device-management-fences.md`). Tick each box with a one-line note. Statuses are bare Legend values ("🤝 Aligned" stays on every row; nothing flips). The capture PRD at `../capture-mode/` is the shape to mirror: its header, Legend, Traceability, Surfaces, Inherited-obligations table, copy pointer, and companion-file headers.

## Layout (after the pass)

- `prd-device-management.md` — header (`Status: draft (F9 refactor; re-lock pending)`, no author line, a Companion files line like the capture PRD's), Background (Use cases and Feature list stay; the Device workflow diagram moves to the journeys file and Background links to it), User Journeys (a pointer paragraph to the journeys file), Requirements with Legend, Traceability, Surfaces, §1–§6, Inherited obligations, §7 as the copy pointer section, Success Metrics, Open Questions.
- `prd-device-management-journeys.md` — UJ 1 through UJ 6.1 verbatim with their diagrams, plus the Device workflow diagram, under a header like the capture journeys file's (non-normative; the rows are the rules).
- `prd-device-management-copy.md` — every §7 state, one row each with an `E<n>` ID in table order, the §7 preamble rules (Labels, Placeholders, whatever §7 states) carried into its header exactly as the capture copy file carries §12's; state names unchanged.
- `prd-device-management-oq-results.md` — one `## OQ <n> — <question>` section per open question whose table row today holds evidence or an answer (the eight "residual" rows and any row reading ANSWERED), carrying that evidence verbatim; the table keeps a one-line "Decision so far".

## Boxes

- [x] **RP-1** — IDs. Every requirement row in §1–§6 gains `R<section>.<n>` in document order as the first column (columns become ID | Release | Pri | Requirement | Status | Commit PR, as the capture PRD's); every §7 state gains `E<n>`; every metric `M<n>`. A Traceability section (the capture PRD's text, adapted) declares the three families and the never-renumber rule, and records that IDs were assigned by F9.
  - _Result:_ Done: 100 `R<section>.<n>` rows (§1=22, §2=19, §3=5, §4=6, §5=20, §6=28), 32 `E<n>` copy states, 5 `M<n>` metrics; the columns are now ID | Release | Pri | Requirement | Status | Commit PR, and the new Traceability section declares the three families, the never-renumber rule, and that F9 assigned them.
- [x] **RP-2** — Compaction (F9 (1)). Every requirement row at most two sentences; concise over prose; the table shrinks and never expands. Rationale, examples, and "because" clauses go; a citation to another row, another PRD, or an open question stays in link form. Where a row holds two rules that genuinely differ, split it and give the second the next ID in the section; record every split and every rewrite in a compaction map at the end of this file (old row text's first six words → new IDs). Report row-text word count before and after (5,118 before).
  - _Result:_ Done: every one of the 100 rows is at most two sentences (counting rule below); row-text words 4,998 → 4,791 across 83 → 100 rows, so the table shrank while no rule, constant, named state, citation, or fence left it. 14 rows split into 31, adding 17; the map is at the end of this file.
- [x] **RP-3** — Journeys out. UJ 1–6.1 and the Device workflow diagram move verbatim to the journeys file; every `#uj-…` and workflow anchor in this PRD and in the capture, import, and README files is rewritten to the journeys file; the five mermaid blocks render.
  - _Result:_ Done: the Device workflow diagram and UJ1–UJ6.1 moved verbatim to `prd-device-management-journeys.md` (205 non-blank lines byte-identical once link targets and `&amp;` are normalised); every `#uj-…` and `#device-workflow` anchor in this PRD and in the capture files now names that file, and all five mermaid blocks parse.
- [x] **RP-4** — Copy out. §7's table moves to the copy file with E-IDs; §7 in the PRD becomes a pointer section like the capture PRD's §12; every "§7 state" reference in the PRD (Surfaces table, rows, Legend) becomes an E-ID link with the state name; the capture and import files' links to `#7-error--state-copy` are rewritten to `prd-device-management-copy.md#error--state-copy`.
  - _Result:_ Done: §7's 32 states moved verbatim into `prd-device-management-copy.md` with E1–E32 in table order; §7 is now a pointer section on the capture PRD's §12 model; every state reference in the Surfaces table and in the rows is an E-ID link carrying the state name (E1–E32 all cited), and the capture PRD's and capture copy file's `#7-error--state-copy` links now point at the copy file.
- [x] **RP-5** — Open questions. The table takes the capture PRD's columns (# | Question | Decision so far | Interim rule | Closer | Feeds | Status); Details, Evidence that closes it, Evidence source, Gated on, Depends on fold into those or into the results file per the Layout; every OQ keeps its number; the Legend names each provisional constant with its OQ id as the capture PRD's does.
  - _Result:_ Done: the table now has the capture PRD's seven columns; Details split into Decision so far and Interim rule, Evidence that closes it + Evidence source + Gated on + Depends on folded into Closer, Feeds became row links. Every number kept; the eight `residual` questions carry their evidence verbatim in `prd-device-management-oq-results.md`, and the Legend now lists each provisional constant with the row that holds it and its OQ id.
- [x] **RP-6** — Inherited obligations. A table like the capture PRD's, two parts: what this PRD imposes on other PRDs (the collection-mode banner and any other "inherited obligation" note in the text), and what other PRDs impose on this one — every "Device PRD" line in the capture PRD's and the import PRD's Inherited-obligations tables and every capture row marked "(Inherited note for the device PRD …)", cited as `the capture PRD's R…` with the link convention both PRDs use.
  - _Result:_ Done: a two-part Inherited obligations section — what this PRD imposes (Data Foundation, export, Collection Mode, Capture Mode, Telemetry) and what the capture and import PRDs impose on it, five lines drawn from the capture PRD's own table plus its R4.22/R4.26/R7.14/R11.3 inherited notes and the import PRD's §4 build-on line, every cross-PRD label naming the owning PRD.
- [x] **RP-7** — `&amp;` → `&` in the device PRD (11), `../README.md`, and `../vision.md`; the Legend's status list becomes the capture PRD's bare-value list; the Companion files line and the README's row 1 name the four companions.
  - _Result:_ Done: all 12 literal `&amp;` in the device PRD (the spec said 11) plus 5 in `../README.md`, 1 in `../vision.md` and 8 in the browsing brief became `&`; slugs are unchanged, so `#2-licensing--pre-authorization` and `#7-error--state-copy` keep their anchors. The Legend's status list is now the capture PRD's bare-value list, and the Companion files line and the README's Device Management row and section name the four companions.
- [x] **RP-8** — Checks: every citation and anchor in the device files and in every file that links into them (capture, import, README, vision, briefs) resolves — the three links that were broken by the `&amp;` headings must now resolve; `grep -rn '&amp;' docs/` is empty; no bare cross-PRD row label; every requirement row at most two sentences; all mermaid blocks render; counts: rows per family, statuses, word counts per file before and after.
  - _Result:_ Done: 1,278 relative links and anchors resolve across every doc; the only 4 misses are pre-existing, all in `docs/agent-reviews/2026-09-06-…`, and identical at HEAD. `grep -rn '&amp;' docs/` is not empty — the residue is this file, the F9 fence, and the 2026-09-06 review log, each quoting `&amp;` as the thing being fixed. The cross-PRD bare-label grep is empty; all five mermaid blocks parse; every requirement row is at most two sentences.

## Compaction map

Every row was rewritten for concision; 14 rows split into 31, adding 17 rows for 100 in all. "Old row" numbers the 83 pre-F9 rows in document order; the text is that row's first six words. No row lost a rule, a constant, a named state, a citation, an owner decision, or its 🤝 Aligned status.

**Counting rule for "at most two sentences":** a sentence is a run of text ending in `.`, `?` or `!` that is followed by end-of-cell or by whitespace and then a capital letter, a digit, an opening quote, or an em dash — with terminators inside backticks, inside a markdown link target, inside parentheses, inside double quotes (so quoted shipping copy such as "Readings are generated, not measured." counts as part of its sentence), inside ⟨…⟩ tokens, and inside `e.g.` / `i.e.` / `vs.` / `4.2.3` / `0.5` not counted.

**Row-text word count:** 4,998 before, 4,791 after — whitespace-delimited tokens in the Requirement cell of every §1–§6 row. (The spec's stated baseline of 5,118 is 120 higher; the difference is a counting-rule difference, not lost text — this pass measured the same 83 cells before and after with the same tokeniser.)

| Old row | First six words | New IDs |
| :--- | :--- | :--- |
| 1 | User must explicitly invoke an action | R1.1 |
| 2 | User can connect to a Nix | R1.2 |
| 3 | App must be able to discover | R1.3 |
| 4 | For Nix devices, the user activates | R1.4 |
| 5 | If the app doesn't automatically discover | R1.5 |
| 6 | The list of discovered devices must | R1.6 |
| 7 | App must show a display name | R1.7 |
| 8 | After repeated consecutive pairing failures (retry | R1.8 |
| 9 | A device becomes a known device | R1.9 |
| 10 | On launch, the app auto-reconnects to | R1.10 |
| 11 | When the last-used known device is | R1.11 |
| 12 | The app connects to exactly one | R1.12 |
| 13 | A capture session binds to the | R1.13 |
| 14 | If the last-used known device is | R1.14 |
| 15 | Single connection authority: at most one | R1.15 |
| 16 | Auto-reconnect may open a probe connection | R1.16 |
| 17 | When a post-connect serial read matches | R1.17 |
| 18 | User can remove a known device | R1.18 |
| 19 | Removing a known device never deletes | R1.19 |
| 20 | Re-adding a removed device follows the | R1.20 |
| 21 | Every measurement permanently records the acquiring | R1.21, R1.22 **split** |
| 22 | User enters the license credential once | R2.1, R2.2 **split** |
| 23 | The license credential (both parts) never | R2.3 |
| 24 | A device's first-ever connect requires an | R2.4 |
| 25 | License and authorization failures are distinct | R2.5 |
| 26 | A valid license, an authorized serial, | R2.6, R2.7 **split** |
| 27 | The vendor SDK carries its own | R2.8 |
| 28 | The device panel shows "Offline use | R2.9 |
| 29 | While online, the user can invoke | R2.10 |
| 30 | When online, the app renews the | R2.11 |
| 31 | If the pre-authorization window has expired | R2.12 |
| 32 | Device-authorization renewal, opt-in telemetry (off by | R2.13, R2.14 **split** |
| 33 | The app persists the offline-use end | R2.15, R2.16 **split** |
| 34 | A session started inside the offline-use | R2.17 |
| 35 | Invoking "Extend offline use" while offline | R2.18 |
| 36 | The license credential at rest — | R2.19 |
| 37 | A guided QR-tile calibration flow runs | R3.1 |
| 38 | The SDK's calibration-due signal is authoritative: | R3.2, R3.3 **split** |
| 39 | Capture mode is never interrupted for | R3.4 |
| 40 | After repeated consecutive calibration failures (retry | R3.5 |
| 41 | The device panel's readiness zone covers | R4.1, R4.2 **split** |
| 42 | Each check reports pass / warn | R4.3, R4.4, R4.5, R4.6 **split** |
| 43 | On the first BLE/USB disconnect, a | R5.1, R5.2 **split** |
| 44 | The error alert cannot be dismissed | R5.3 |
| 45 | A halt alerts through at least | R5.4, R5.5 **split** |
| 46 | Recovery: the app auto-retries reconnection to | R5.6 |
| 47 | Recovery never auto-resumes capture. When the | R5.7, R5.8, R5.9 **split** |
| 48 | A completed scan whose save fails | R5.10 |
| 49 | Every halt offers an explicit "End | R5.11 |
| 50 | Every halt writes a local record | R5.12 |
| 51 | A device that accepts no command | R5.13 |
| 52 | The battery-low halt clears when the | R5.14 |
| 53 | A halt that occurs after one | R5.15 |
| 54 | While capture is halted, the scan-accept | R5.16 |
| 55 | A halt record left open by | R5.17 |
| 56 | Quitting the app from a halted | R5.18, R5.19 **split** |
| 57 | Every halt state posts an accessibility | R5.20 |
| 58 | The app ships a "Demo Device | R6.1 |
| 59 | The simulated device requires no hardware | R6.2 |
| 60 | Selecting the Demo Device never invokes | R6.3 |
| 61 | Every measurement from the simulated device | R6.4, R6.5 **split** |
| 62 | The app behaves identically against the | R6.6 |
| 63 | Against the Demo Device, all four | R6.7 |
| 64 | CI exercises the simulated-device path on | R6.8 |
| 65 | The simulated device exposes an explicit | R6.9 |
| 66 | One simulated licensing layer with two | R6.10 |
| 67 | An injectable clock, so offline-use window | R6.11 |
| 68 | An injectable network-reachability state, so online/offline | R6.12 |
| 69 | Store fault injection — fail the | R6.13 |
| 70 | Simulated discovery entries carry exactly id, | R6.14 |
| 71 | A test can set the outcome | R6.15 |
| 72 | The simulated layer can present multiple | R6.16 |
| 73 | Every outbound network attempt the app | R6.17 |
| 74 | A test can set the reported | R6.18 |
| 75 | A test can set device presence | R6.19 |
| 76 | A test can start the app | R6.20 |
| 77 | A SpectroDevice conformance suite runs the | R6.21 |
| 78 | Error-case parity, assertable: for every error | R6.22, R6.23 **split** |
| 79 | Telemetry is fire-and-forget — it can | R6.24 |
| 80 | Any simulated device or service seam | R6.25 |
| 81 | The app always declares the Bluetooth | R6.26 |
| 82 | The simulated device has a configurable | R6.27 |
| 83 | The simulated device can replay fixtures | R6.28 |

**Whole-file words.** `prd-device-management.md` 12,618 → 11,162, with 1,813 in the journeys file, 1,602 in the copy file, and 696 in the open-question results file. The four files total 15,273. The requirement table shrank; the growth is F9's own scaffolding — the Traceability section (163 words), the Legend's provisional-constant list (+297), the Inherited obligations tables (813, most of it obligations imported from the capture and import PRDs), and the results file's verbatim evidence (696), which the Layout asks to sit alongside the table's one-line "Decision so far".
