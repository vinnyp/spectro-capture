# Lock preconditions — the final sweeps (2026-09-16)

Two sweeps of the three mechanical lock checks: the first at `1e8830a` (the lock commit) found three editorial misses and one post-lock item; the second at `4e9826f`, after those four were closed, is clean. Both are reproduced below as written.

---

# Lock preconditions — mechanical sweep at HEAD `1e8830a`

**Scope.** Data Foundation (DF) and Data Export (DE), five files each, in
`/Users/vinnypasceri/Projects/.worktrees/spectro-capture__docs-data-foundation` (branch `docs/data-foundation`, HEAD **`1e8830a`** — "lock the Data Foundation and Data Export PRDs…"; working tree clean). Read-only; nothing edited, staged or committed. Re-runs the three checks of
`docs/agent-reviews/2026-09-15-data-foundation-lock-checks-pre-lock.md` (run at `5f428e6`), whose item numbers are cited below as *(was 1e-2)* etc. Landed since: rounds 8–11 and the lock commit (`3d62dac`, `f006bad`, `1e8830a`). Line numbers are as of `1e8830a`. Paths are relative to `docs/product/` unless shown otherwise.

Classification key — **EDITORIAL**: a link target, label, index or map entry fixable without changing a rule. **POST-LOCK**: a locked sibling must change. **DECISION**: needs an Open Question or an owner call.

**Headline.** All nine editorial misses and all three post-lock items from the `5f428e6` sweep are closed. The state that locks is clean on every hard precondition. Three low-grade residue items remain (two are the same map-entry class the prior sweep flagged as "arguably exempt"), one genuine post-lock item is *new* — the decision-queue row that the lock commit's README fix left behind — and DE's word count now sits at exactly its budget.

---

## CHECK 1 — Cross-PRD consistency

### Pairs enumerated

X ∈ {DF, DE}. Y = any PRD X cites or that cites X.

| Pair | Direction | Basis |
| :--- | :--- | :--- |
| DF ↔ Capture Mode | both | `capture-mode/prd-capture-mode.md:421` + `:423` target Data Foundation; DF `:231` inbound, `:236` the 21-row map, `:246`–`:249` outbound ×4 |
| DF ↔ Device Management | both | `device-management/prd-device-management.md:300`; DF `:232` inbound, `:250`, `:251` outbound |
| DF ↔ Inventory Import | both | `import/prd-inventory-import.md:169`; DF `:234` inbound, `:252` outbound |
| DF ↔ Data Export | both | DF `:233` inbound / `:242` outbound; DE `:170` inbound / `:179` outbound |
| DF ↔ Vision | DF→vision | DF `:253` outbound (post-lock on `vision.md:163`) |
| DE ↔ Capture Mode | both | DE `:171` inbound (R1.9, R4.24), `:181` outbound |
| DE ↔ Device Management | both | `device-management/prd-device-management.md:300`, `:301`; DE `:172` inbound (R6.5, R1.21), `:180` outbound |
| DE ↔ Inventory Import | both | DE `:173` inbound (R2.2), `:182` outbound |

DF also names Collection Mode (`:243`), QC & Comparison (`:244`) and Telemetry (`:245`) as targets; those PRDs do not exist (`README.md:18`–`:21`, "queued"), so no pair is checkable. A fresh search of the three locked siblings, `vision.md`, `AGENTS.md`, the root `README.md` and `docs/decisions/README.md` for links into `data-foundation/` or `export/` returns **nothing** — only `README.md` links in (`:16`, `:17`, `:49`, `:55`, `:88`, `:104`). Every DF/DE reference in a sibling is by prose name.

### (a) Inherited obligations agree both ways

Resolved since `5f428e6`: **was 1a-1** (capture R4.24 → Data Export now recorded at DE `:181`, "its R4.24 basis obligation names this PRD too"); **was 1a-2** (device R1.21 → Data Export now recorded at DE `:180`, "and its R1.21 snapshot obligation names this PRD too"); **was 1a-3** (DF `:253` now names the README's two rows beside the vision's phrasing — and the product README's own two rows were corrected in the lock commit itself).

Verified clean this pass:

- **The 21-row map is complete and exact.** `capture-mode/prd-capture-mode.md:421` lists R1.9, R3.1, R3.6, R3.8, R4.6, R4.11, R4.12, R4.13, R4.24, R6.7, R6.9, R7.12, R7.18, R8.5, R8.9, R8.13, R8.17, R11.6, R11.10, R11.11, R11.16 — 21 rows. DF `:236` names all 21 and no others (6+1+4+1+1+4+3+1 = 21), with R6.9 explicitly carried by no DF row and handed to Collection Mode (mirrored at DF `:243`) and R11.16 → R6.5 under fence F32. `capture:423`'s second Data Foundation line (the gamut mark) is named as carried whole by R3.4 (DF `:231`, `:236`).
- **DF ↔ DE mirrors one-for-one.** DF inbound `:233` names DE R4.1, R4.2, R4.3 (plus its R1.1 as the source of the not-exportable states) ↔ DE outbound `:179` Rows = R4.1, R4.2, R4.3. DE outbound `:179` names DF R7.7, R7.2, R7.3, R6.5 ↔ DF inbound `:233` Rows here = R6.5, R7.2, R7.3, R7.7. DF outbound `:242` names DF R1.2, R2.1, R2.2, R2.4, R2.9, R3.1, R6.5 ↔ DE inbound `:170` names the identical seven and lists seven DE rows (R1.1, R1.2, R1.3, R2.1, R2.3, R4.2, R4.3). The 13-item fixture list at DF `:220` and DE `:179` matches item-for-item.
- **DF ↔ Device, DF ↔ Import** match exactly (`device:300` R1.21+R1.22 ↔ DF `:232`; `import:169` R2.3+R3.2 ↔ DF `:234`).
- **DE ↔ Device, DE ↔ Capture, DE ↔ Import** each have both halves recorded (DE `:172`↔`:180`, `:171`↔`:181`, `:173`↔`:182`).
- `device:301` ("Data Foundation, export | CSV export emits a `simulated` column | R6.5") names Data Foundation as a co-target and DF carries no counterpart inbound line. **Not a miss:** DE `:180` records the re-target ("its 'Data Foundation, export' obligation line names this PRD as its target", fence F6), and the column is wholly DE's. **No action.**
- Capture and Import still carry no inbound obligations table; every obligation DF and DE push at them is logged as a post-lock line (DF `:246`–`:249`, `:252`; DE `:181`, `:182`). **No action.**

| # | Location | Expected | Found | Class |
| :--- | :--- | :--- | :--- | :--- |
| 1a-1 | `../decisions/README.md:22` vs `README.md:125`, `data-foundation/prd-data-foundation.md:253` | Every site carrying the "raw payload canonical" phrasing DF's outbound Vision line corrects is either fixed or named | The lock commit `1e8830a` corrected `README.md:105` and `README.md:125` (`:125` now reads "the stored mean canonical with the raw payload archived beside it"). Its own source-of-truth counterpart, the ADR-0003 decision-queue row at `../decisions/README.md:22`, still reads "Storage schema: **canonical raw payload**, version history, derived-value recompute…" — the two rows are now divergent, and DF `:253` names only "the v1 feature list … and the README's two matching rows" | **POST-LOCK** (correct the decision-queue row; optionally extend DF `:253` to name it) |
| 1a-2 | `data-foundation/prd-data-foundation.md:253` | A post-lock line names work still outstanding | The line names "the README's two matching rows" alongside `vision.md:163`. Both README rows were fixed in the same commit that locked the PRD, so only `vision.md:163` remains outstanding. The sentence is not false (it states the correct meaning), merely stale on one of its two halves | **EDITORIAL** (nit — trim to the vision, or add the decision-queue row from 1a-1) |

### (b) Shared rows agree on priority — **CLEAN**

- Capture R8.9 **P1** vs DF R2.3 **P0**: reconciled in DF itself at `:236` ("Its R8.9 is P1 where [R2.3] is P0 because that PRD's fence F24 puts the record shape at P0 and the correction path at P1").
- Capture R6.9 **P1**: carried by no DF row, stated at DF `:236` and DF `:243`.
- Every other handed row is **P0** upstream and **P0** here, verified against each sibling's Pri cells: capture R1.9/R3.1/R3.6/R3.8/R4.5/R4.6/R4.11/R4.12/R4.13/R4.24/R6.7/R7.12/R7.18/R8.5/R8.13/R8.17/R11.6/R11.10/R11.11/R11.16 and R1.2/R1.3/R1.5/R1.8/R1.10; import R2.2, R2.3, R3.2, R3.3, R3.5; device R1.21, R1.22, R6.5, R6.9, R6.12, R6.13, R6.17, R6.20.
- Pri cells: DF has exactly one P1 (R6.3, `:200`) and 45 P0, matching its Legend (`:79`). DE has exactly one P1 (R1.3, `:120`) and 12 P0, matching its Legend (`:54`).
- Conditionals matched both ways: DF R7.2 `:215` (×2) and M9 `:272` gate on "once [R6.3] lands"; DF R6.2 `:199` and the DF copy header (`prd-data-foundation-copy.md:4`) gate on "the export PRD's R1.3"; DE R4.1 `:157` and R4.2 `:158` gate on "once [R1.3] lands"; DE E1's ‹P1› sentence and ‹P1› action match.

### (c) Cross-PRD cites name the owning document — **CLEAN**

Standing grep, over all ten files:

```
grep -nE '\[(R|E|M|F|OQ ?|DJ|EJ|UJ|J)[0-9][^]]*\]\(\.\./' data-foundation/prd-data-foundation*.md export/prd-data-export*.md
→ no output (exit 1)
```

All **862** links in the ten files were enumerated and every cross-directory label tabulated (full census run). Every one is (i) an explicit owner label — `the capture PRD's R4.24`, `the device PRD's R1.21`, `the import PRD's R2.3`, `the export PRD's R2.3`, `the Data Foundation PRD's R7.7`, `vision J4`; (ii) an `its …` / `Its …` / `that PRD's …` anaphor inside an obligations-table cell whose Source/Target column names the document (DF `:231`–`:234`, `:242`–`:253`; DE `:170`–`:173`, `:179`–`:182`); or (iii) an `its …` anaphor whose antecedent is an explicitly labelled cite in the same sentence (DF `:131`, `:132`, `:146`, `:167`, `:202`, `:236`; DE `:118`, `:120`, `:133`, `:158`, `:159`; DE Traceability `:96`; DF fence map `:181`–`:207`). **No bare-ID label exists.** Non-link prose cites (DF `:236`'s 21-row map, DE `:179`'s "its R6.5") sit inside cells whose Source/Target column names the owner.

Cross-PRD fence cites are likewise all qualified: `the export PRD's F1`–`F10` (DF fence map, each preceded by "moved to the export PRD's …"), `its F31` (DE `:15`, `:170`), `its fence F27` (DE `:120`), `that document's fence F30` (DE `:67`), `that PRD's fence F24` (DF `:236`, the capture PRD's). Unqualified `fence Fn` is always the citing document's own.

### (d) Retired and moved IDs — **CLEAN**

*(was 1d-1 — closed.)* Every retired ID surviving in preserved fence prose is now qualified: **M3** at F18 `prd-data-foundation-fences.md:93`, F26 `:133`, F29 `:149`; **OQ 9** at F5 `:35`; **OQ 8** at F20 `:101` and F23 `:113`; **OQ 16** at F24 `:119`; **R4.4** at F5's clarification `:37`, F27's Why `:139` and the F27 map entry `:203`; **R4.7**/**R4.8**/**R4.9** at `:121`/`:109`/`:119`. The only unqualified occurrences are inside **F30 itself** (`:155`), the fence that declares the move — self-qualifying. A full retired-ID scan of all five DF files returns no other hit.

DF's Legend retired list (`:83`) and DE's Traceability table (`:71`–`:94`) reconcile **one-for-one**, all 24 pairs, including the three split rows (R7.6, R7.7, R7.8) reconciled in prose on both sides (DF `:83`, DE `:96`). No retired ID is reused as a live ID: DF has no §4 rows (sections run 1, 2, 3, 5, 6, 7), no R7.9, no R7.6a (surfaces are R7.6b–R7.6n), no M3 (M1, M2, M4–M9), no E6/E7/E17/E18 (the copy file holds exactly E1–E5, E8–E16, E19–E30 = 26), no DJ1 (DJ2–DJ5), and OQs 1–7, 10, 12–15 with no 8/9/11/16.

### (e) Fence numbering and fence → row maps

Numbering is per PRD (DF F1–F32, DE F1–F13) with no overlap in meaning, and every cross-PRD fence cite is qualified (above). Map completeness: **DF 32/32** map rows (`prd-data-foundation-fences.md:177`–`:208`), **DE 13/13** (`prd-data-export-fences.md:95`–`:107`).

Forward direction re-derived from scratch: every fence cite in both PRD bodies was extracted per line and matched against its map entry. **All requirement-row cites resolve, both documents** — including the three the prior sweep flagged: *(was 1e-1)* DE F4's entry now reads `R2.1, R2.4; OQ 1.` (`prd-data-export-fences.md:98`); *(was 1e-2)* DF F13's entry now reads "R1.7; **the dead-process hold in R1.5 and R7.3**; R1.4's second clause; R7.4's volume-class scoping and M1's population." (`prd-data-foundation-fences.md:189`); *(was 1e-3)* all five obligations-line / scope-statement cites now have counterparts — F2 `:178` and F14 `:190` name the Capture Mode OQ 19 line, F14 `:190` names the Device Management (UJ1) line, F10 `:186` names the Vision line, F25 `:201` / F27 `:203` / F28 `:204` / F31 `:207` each name the outbound Data Export line, and DE F9 `:103` names the Background scope statement.

Reverse direction (a map entry naming a row that cites no fence) occurs in both maps and is **not** a defect: both maps define "carries" as "the fence's decision is what the row now states" (`prd-data-foundation-fences.md:173`, `prd-data-export-fences.md:91`). DE F3 and F12 are reverse-only and correctly so.

| # | Location | Expected | Found | Class |
| :--- | :--- | :--- | :--- | :--- |
| 1e-1 | `data-foundation/prd-data-foundation.md:257` vs `prd-data-foundation-fences.md:184` (F8) | A non-row statement citing a fence appears in that fence's map entry, as the five *(was 1e-3)* fixes established | §8 reads "wording the Telemetry PRD and the help docs own (**fence F8**, [R6.6])", of a Surfaces cell that stays "—". F8's entry reads `R6.6 …; the Telemetry and help-docs line in Inherited obligations …; OQ 12's ownership half.` — the §8 statement and the "—" cell are **not named**. Pre-dates `5f428e6`; the prior sweep's 1e-3 enumerated five other cites and did not reach this one | **EDITORIAL** (low; arguably exempt under the map's "carries" definition) |
| 1e-2 | `data-foundation/prd-data-foundation.md:291` vs `prd-data-foundation-fences.md:206` (F30) | Same | The Open Questions retirement note reads "Numbers 8, 9, 11 and 16 are retired under **fence F30**…". F30's entry names "The Legend's retired-ID list; the Background scope statement; R7.7, R7.8 and R7.6 …; the obligations tables both ways" — the **OQ retirement note is not named** (the Legend's list is a different location). Same class and vintage as 1e-1 | **EDITORIAL** (low; same exemption argument) |

---

## CHECK 2 — Index sync

### (i) Fence → row maps vs body cites
Covered in 1(e): forward direction **clean in both documents**, all three prior misses fixed; two low-grade non-row residue items (1e-1, 1e-2). Both map tables complete (32/32, 13/13).

### (ii) Surfaces tables — **CLEAN, both documents**

DF's 13 surfaces (`:100`–`:112`) yield a Copy-ID union of E1–E5, E8–E16, E19–E30 = **exactly the 26 states** in `prd-data-foundation-copy.md`, and every Req-ID named (R1.3, R1.5, R1.7–R1.10, R2.4, R2.6, R2.8, R2.9, R3.2, R3.5, R5.1–R5.8, R6.2, R6.3, R6.6) exists. Containment holds in every cell: each Copy ID's producing row is among that surface's Req-IDs — *(was 2ii-1, secondary)* R7.6d's Req-IDs now include **R1.5** (`:102`), which produces E10.

*(was 2ii-1)* §8 `:257` now reads "**E4, E10, E11 and E28–E30** are listed on two surfaces because each opens from both." The set of states appearing on ≥2 surfaces is exactly E4 (R7.6g, R7.6h), E10 (R7.6b, R7.6d), E11 (R7.6g, R7.6i), E28/E29/E30 (R7.6b, R7.6m) — **exact match, no more and no fewer**.

*(was 2ii-2)* R7.6d `:102` no longer links E25/E27 from its "What the test lists" cell; it reads "a local disk, a syncing folder or a network volume" in plain words. The only remaining state-link-in-a-test-cell convention gap is gone.

*Note (informational, not a miss):* a strict "every E appears in exactly the Surfaces rows its producing rows imply" reading does not hold by design — R5.1/R5.5/R5.8 sit in both R7.6b's and R7.6m's Req-IDs but produce E12 only on R7.6m, and R5.5 produces E4 on R7.6g/h rather than R7.6b. The operative rule is coverage (union = the live state set) plus containment (each Copy ID's producing row ∈ that surface's Req-IDs), and both hold.

DE's one surface (`:104`) is clean: Req-IDs R1.1–R1.3, R2.1–R2.5, R3.1 and Copy IDs E1–E4 all exist, E1←R1.3/R2.4 and E2–E4←R3.1 all within the Req-ID set, no duplication.

### (iii) Copy states ↔ producing rows — **CLEAN, both documents**
DF: all 26 states are cited by at least one requirement row (E1←R5.3; E2←R5.1; E3←R5.2; E4←R2.9,R5.5; E5←R2.2,R5.5,R7.3; E8←R6.2; E9,E10←R1.5; E11←R2.4,R2.8,R2.9; E12←R5.1,R5.8; E13←R1.8; E14←R6.2; E15←R1.10; E16←R5.7; E19,E21←R1.9,R7.8; E22←R1.3; E23←R5.2,R5.7; E24←R5.8; E25,E27←R1.7; E26←R2.8; E28,E30←R5.5,R5.8,R7.3). E20 and E29 are cited only inside the ranges `[E19]–[E21]` and `[E28]–[E30]`, the documents' established form. Every E cited by a row exists; the one out-of-document cite, `the capture PRD's E26` at DF `:216`, is qualified.
DE: E1←R1.3, R2.4; E2/E3/E4←R3.1. All exist, all cited.

### (iv) Journey indexes ↔ companions — **CLEAN, both documents**
DF's index (`:53`–`:56`) reads DJ2–DJ5, matching the companion headings and anchors, with the DJ prefix stated at `:49`; rows-exercised agree exactly for all four (DJ2 R2.2–R2.5 + R2.8; DJ3 R2.9 + R5.1–R5.8; DJ4 R6.1–R6.4; DJ5 R1.1–R1.3 + R1.6 + R3.2 + R5.6). Every row cited in either journeys companion exists (full extract: R1.1, R1.3, R1.6, R2.2, R2.5, R2.8, R2.9, R3.2, R5.1, R5.6, R5.8, R6.1, R6.4, plus OQ 2 and the `[DJ2]` back-link). DE's journey is EJ1 throughout (index `:39`, Traceability `:94`, companion heading, anchor `#ej1-export-the-collection`), its index row carries "and the Data Foundation PRD's R3.4, R3.5" matching the companion, and every row it cites (R1.1, R1.3, R2.1, R2.5, R3.1) exists. The only `J1`–`J7` tokens left are `vision J4` (DE `:9`, `:112`, journeys `:10`) and `vision J7` (DE `:202`), all qualified.

### (v) Answered OQs ↔ results files — **CLEAN, both documents**
DF answered = 1, 3, 4, 7, 10; the results file holds `## OQ 1`, `3`, `4`, `7`, `10` — five sections, one-for-one, no orphans. Open = 2, 5, 6, 12, 13, 14, 15, with no sections. DE answered = 1, 2; results holds `## OQ 1`, `## OQ 2`. Open = 3, 4.

### (vi) Copy header markers — **CLEAN, both documents**
DF's header (`prd-data-foundation-copy.md:4`) defines four markers and all four are used, with no fifth: ‹floor› ×2 (definition + E16's state name, `:16`), ‹OQ 14› ×2 (definition + E9's body, `:26`), ‹P1› ×6 (two in the header, two each in E8 `:23` and E14 `:24`), ‹until P1› ×7 (one in the header, three each in E8 and E14). Every use leads a whole sentence or trails a whole action, as the header requires. The header names the cross-PRD seam explicitly ("E8's and E14's full-history sentence waits on the export PRD's R1.3"). DE's header (`prd-data-export-copy.md:4`) defines ‹P1› only and uses it ×3 (definition, E1's body sentence, E1's second action `:16`) — no undefined marker, no unused definition. No marker glyph appears in any of the other eight files.

### (vii) Provisional constants and Feeds cells — **CLEAN**

An UPPER_CASE sweep of all ten files returns **twelve** tokens and no thirteenth: SQLITE_READER_FLOOR (16), ROWS_CEILING (12), DERIVATION_VERSION (6), WAVELENGTH_GRID (5), HISTORY_RETENTION (5), GAMUT_RENDERING_INTENT (5), GAMUT_REFERENCE_SPACE (5), DELETE_UNDO_WINDOW (5), STORE_SIZE_BUDGET (4), INTEGRITY_CHECK_BUDGET (3), DERIVATION_TOLERANCE (3), COMPATIBILITY_FLOOR (3) — the same twelve as at `5f428e6`. Every one carries an OQ id or a fence at its defining use site: SQLITE_READER_FLOOR → OQ 2 (DF `:129`); STORE_SIZE_BUDGET → OQ 5 (`:153`); DERIVATION_TOLERANCE → OQ 6 (`:218`); INTEGRITY_CHECK_BUDGET → OQ 13 (`:186`); WAVELENGTH_GRID → OQ 4 (DE `:133`); GAMUT_* → fence F4 (DF `:168`), OQ 7 (`:284`); HISTORY_RETENTION → fence F6 (`:153`); DELETE_UNDO_WINDOW → fence F7 (`:200`); DERIVATION_VERSION declared "a stamp, not a constant" (DF Legend `:81`, DE Legend `:56`); ROWS_CEILING the capture PRD's OQ 13 (DF `:81`, DE `:56`); COMPATIBILITY_FLOOR appears only in fence prose (`prd-data-foundation-fences.md:109`, `:198`; `prd-data-export-fences.md:39`) and is declared *not to exist* until a release raises the floor (DF R5.7 `:181`) — not a latent decision.

All seven "per SDK docs" markers carry an OQ id: DF `:116` (OQ 12, OQ 15), `:146` (OQ 15), `:165` (OQ 15), `:286` and `:289` (inside their own OQ rows); DE `:108` ("the Data Foundation PRD's OQ 15") and `:203` (inside OQ 4's row).

*(was 2vii-1)* DF OQ 2's Feeds (`:279`) now names R1.2, R5.1, R5.2, R5.6, R6.5, R7.1, R7.2, R7.3, M4, M9 — **exactly the ten DF rows that use SQLITE_READER_FLOOR** (R1.2 `:129`, R5.6 `:180`, R5.1 `:182`, R5.2 `:184`, R6.5 `:202`, R7.1 `:214`, R7.2 `:215`, R7.3 `:216`, M4 `:267`, M9 `:272`), no more and no fewer. *(was 2vii-2)* DE OQ 4's Feeds (`:203`) now names R2.3, R2.5 **and R4.1**, matching its own Interim-rule cell.

### (viii) Word counts

Method as specified — strip HTML comments, link targets `](…)`, code fences and table pipes, then count whitespace-separated tokens. The script reproduces the prior sweep's figures exactly at `5f428e6` (7,922 / 3,942), so the method is the same one.

| Document | Count | Budget | Headroom |
| :--- | :--- | :--- | :--- |
| `data-foundation/prd-data-foundation.md` | **7,997** | 8,000 (fence F21, raised 2026-09-14) | **3** |
| `export/prd-data-export.md` | **4,000** | 4,000 (DE fence F1) | **0** |

Both within budget. Trajectory: 7,996 / 3,998 at `3d62dac` and `f006bad` (matching the round-11 commit message), then +1 / +2 at the lock commit `1e8830a` — the `Status: draft` → `Status: locked (2026-09-16)` header change. Deleting the six guidance comments cost nothing (the method strips them already). **DE now sits exactly on its cap: any further addition to the DE body needs a compaction pass or an F1 amendment.** Flagged as informational, not a miss.

### (ix) README sync — **CLEAN**
`README.md:16` "| 4 | [Data Foundation](…) | U5, U6 | **Locked** — review gate closed 2026-09-16 after 12 rounds, both PRDs under one log; owner merge pending |" ↔ `prd-data-foundation.md:3` "Status: locked (2026-09-16)". `README.md:17` "| 5 | [Data Export](…) | U6 | **Locked** — split out of Data Foundation on 2026-09-09 under its fence F30 and locked with it 2026-09-16 |" ↔ `prd-data-export.md:3` "Status: locked (2026-09-16)". Both rows 4–5 read **Locked**. `:49` and `:55` describe the two five-file shapes; all eight companions exist. `:88` routes U6 to both; `:104` routes the CSV P0 to Data Export; `:117` records the split. `AGENTS.md:86`–`:87` list both directories; `AGENTS.md:105`–`:106` carry the fence-F10 vocabulary amendment. No stale or missing row. (The one substantive index item is 1a-1 above, in `docs/decisions/README.md`, not in either README.)

---

## CHECK 3 — Latent-decision inventory

| # | Item | Location | Found | Class |
| :--- | :--- | :--- | :--- | :--- |
| — | Constants with no OQ or fence | all ten files | **None.** Twelve tokens, every one anchored. See 2(vii) | — |
| — | Copy states with no producing row | both copy files | **None.** See 2(iii) | — |
| — | Rows an upstream PRD places at a higher priority | — | **None.** Capture R8.9 P1 / DF R2.3 P0 and capture R6.9 P1 / no DF row are both stated in DF (`:236`, `:243`); every other handed row is P0 on both sides. DF's only P1 (R6.3) and DE's only P1 (R1.3) have no upstream counterpart | — |
| — | Undefined terms the rows lean on | both Vocabulary sections | **None outstanding.** *(was 3-1)* The two-sensed "volume class" is resolved: the phrase now appears only at M1 `:265` ("each recording its volume class", "read per volume class") in the durability sense, whose classes R7.4 `:217` enumerates ("local volumes … USB external and network volumes … each run recording its class"); the warning sense is now worded "the file's location" / "local, sync-managed, or a network volume" (R1.7 `:133`, R7.2 `:215`, R7.6d `:102`, R7.6e `:103`). One taxonomy, one phrase. *Informational:* "volume class" is defined in-row (R7.4) rather than in the Vocabulary | — |
| — | "working set" | `data-foundation/prd-data-foundation.md:72`, used `:214` | **Resolves.** The Chosen-condition entry reads "a reading's working set is the set the app works it from ([R3.1])", and R3.1 `:165` says "the set for the collection's chosen scan mode … is the one the app works from". R7.1 `:214`'s use — "every spectral reading carrying a working set stamped with an illuminant, observer or condition other than its collection's" — reads correctly against that. Defined inside another entry rather than as its own bullet; no ambiguity | — |
| — | `{{placeholder}}` | all ten files | **0** | — |
| — | `TBD` / `TODO` / `XXX` | all ten files | **0** | — |
| — | HTML comments in either PRD body | all ten files | **0** — the six guidance comments were deleted at `1e8830a`; all eight companions carry 0 | — |
| — | "Owner:" / author line | all ten files | **0** (both fence files declare the shape as "no author line", DF F1 `:11`, DE F1 `:11`) | — |
| — | Status cell not one of the Legend's six | both PRDs + both copy files | **None.** The other five glyphs appear only inside the two Legend blocks (`prd-data-foundation.md:87`–`:88`, `prd-data-export.md:60`–`:61`) | — |
| — | Rows whose Status is not 🤝 Aligned | both PRDs + both copy files | **Zero — as expected.** DF: 67 🤝 (46 R rows + 13 surfaces + 8 metrics) and 26 🤝 states; DE: 15 🤝 (13 R rows + 1 surface + 1 metric) and 4 🤝 states. The six non-Aligned rows the prior sweep listed (DE R4.1, DF R6.2, DF E8/E28/E29/E30) are all 🤝 Aligned now; DF R5.7 and R7.5 carry "owner overrule" in Commit PR, DE R4.1 "aligned round 12" | — |
| — | Headers | `prd-data-foundation.md:3`, `prd-data-export.md:3` | Both read **"Status: locked (2026-09-16)"** | — |
| — | Product README rows 4–5 | `README.md:16`, `:17` | Both read **Locked** | — |
| — | Fix files under the two directories | `data-foundation/`, `export/` | **None.** Exactly five files each; all thirteen `*-fixes.md` were removed at `1e8830a` | — |

---

## Link and anchor resolution

| Set | Links checked | Broken |
| :--- | :--- | :--- |
| The ten files (every relative path + every `#anchor` against the target file's real headings, strict GitHub slug — underscores kept, `*` emphasis stripped, em-dashes vanishing) | **862** | **0** |
| All product docs — the ten, the three siblings and their companions, `vision.md`, `product/README.md`, plus root `README.md`, `AGENTS.md`, `docs/decisions/README.md` | **2,197** | **0** |

**No genuine miss anywhere.** The one out-of-scope miss the prior sweep recorded (`prd-data-foundation-round-7-fixes.md:9`) is gone with the fix files.

**Slugger artefacts (not misses).** The run was repeated with a strict GitHub slugger that keeps `_` (GitHub does not treat a literal underscore inside a heading as emphasis) — `prd-data-export-fences.md:43` → `#f4--the-gamut-clipped-export-column-is-sc_srgb_gamut_clipped-2026-09-09` and both links at `prd-data-export-oq-results.md:11` resolve under it and are the false positives a `_`-stripping slugger reports. Result is unchanged at 862/0 under both slug variants, so no finding depends on the choice. The em-dash rule behaves as described ("F8 — The CSV" → `f8--the-csv`), and the `sc_`-bearing fence headings are the only place it matters.

---

## Counts

| Class | Count | Items |
| :--- | :--- | :--- |
| **EDITORIAL** | **3** | 1a-2, 1e-1, 1e-2 |
| **POST-LOCK** | **1** | 1a-1 |
| **DECISION** | **0** | — |
| Informational | 2 | DE at exactly 4,000/4,000 words; "volume class" defined in-row rather than in the Vocabulary |
| Clean checks | — | 1(b) priorities, 1(c) cites, 1(d) retired-ID ledger, 1(e) forward fence→row both maps, 2(i)–2(vii) and 2(ix), 3 in full (constants, copy↔rows, upstream priority, terms, placeholders, comments, author lines, statuses, headers, README, fix files), link/anchor resolution (862/0 and 2,197/0) |

Closed since `5f428e6`: **all nine editorial items** (1d-1, 1e-1, 1e-2, 1e-3, 2ii-1, 2ii-2, 2vii-1, 2vii-2, 3-1) and **all three post-lock items** (1a-1, 1a-2, 1a-3).

## Verdict

**The state that locks is clean on every mechanical precondition.** Both hard gates pass outright: check 1(c) is clean on the standing grep across all 862 links in the ten files, with every cross-directory label an explicit owner label or an anaphor with a named antecedent in the same cell or sentence; and the retired/moved ID ledger reconciles one-for-one in both directions with no reuse and — new since the last sweep — no unqualified retired ID left anywhere in the preserved fence prose. Every link and anchor in the ten files resolves, and so does every link in all 36 product documents around them. Both word counts are within budget, both headers read locked, both README rows read Locked, no fix file survives under either directory, no HTML comment survives in either body, and not one row in either PRD or either copy file is anything other than 🤝 Aligned.

The three remaining editorial items are shallow and none touches a rule. Two (1e-1, 1e-2) are the same low-grade class the prior sweep called "arguably exempt under the map's 'carries' definition": a fence cited from a non-row statement — DF §8's F8 cite and the Open Questions retirement note's F30 cite — whose map entry does not name that statement. Both pre-date `5f428e6` and were simply not among the five the prior sweep enumerated; the fix is one clause in each map entry. The third (1a-2) is a post-lock line that half-outran itself: DF `:253` names "the README's two matching rows" alongside the vision's phrasing, and the lock commit fixed both of those rows in the same breath, leaving only `vision.md:163` outstanding.

The one post-lock item is genuinely new and worth the owner's eye: the lock commit corrected `README.md:105` and `README.md:125` to "the stored mean canonical with the raw payload archived beside it", but left `docs/decisions/README.md:22` — the ADR-0003 decision-queue row that `README.md:125` links to and mirrors — reading "Storage schema: canonical raw payload". The two now disagree, and the disagreement sits in the decision queue that ADR-0003 will be written from, which is the sharpest one-way door in the project. It is a one-line correction in a document neither PRD owns, and it is the same fence-F10 phrasing DF `:253` already records against the vision; extending that line to name it would close the loop.

The one number to watch is DE's body: 4,000 words against a 4,000-word budget, zero headroom. DF has 3. Neither blocks the lock, but any later amendment to either body — including the two one-clause map fixes, which live in the fence files and cost nothing against the budget — needs to know that the PRD bodies themselves have no room left.


---

# Lock preconditions — mechanical sweep at HEAD `4e9826f`

**Scope.** Data Foundation (DF) and Data Export (DE), five files each, in
`/Users/vinnypasceri/Projects/.worktrees/spectro-capture__docs-data-foundation` (branch `docs/data-foundation`, HEAD **`4e9826f`** — "close the final lock-check sweep's three editorial misses and the ADR-0003 queue row's phrasing"; working tree clean). Read-only; nothing edited, staged or committed.

Re-runs, **in full and derived from scratch on this state**, the three checks of the `1e8830a` sweep (`scratchpad/lock-checks-final.md`), whose item numbers are cited below as *(was 1a-1)* etc. Landed since: `4e9826f` only (3 files, 4 lines). Line numbers are as of `4e9826f`. Paths are relative to `docs/product/` unless shown otherwise.

Classification key — **EDITORIAL**: a link target, label, index or map entry fixable without changing a rule. **POST-LOCK**: a locked sibling must change. **DECISION**: needs an Open Question or an owner call.

**Headline.** **All four items from the `1e8830a` sweep are closed and the re-run surfaces no new miss in any of the three checks.** Every count that the prior sweep pinned reproduces exactly on independent derivation: 862/0 and 2,197/0 links, 32/32 and 13/13 fence maps, 26/26 and 4/4 copy states, 67/15/26/4 🤝 cells, twelve constants, zero placeholders/comments/author lines/fix files. DF's word count **fell 6 to 7,991** (the Vision-line trim), restoring headroom from 3 to 9; DE is untouched at exactly 4,000/4,000. **Zero EDITORIAL, zero POST-LOCK, zero DECISION.**

---

## CHECK 1 — Cross-PRD consistency

### Pairs enumerated

X ∈ {DF, DE}. Y = any PRD X cites or that cites X. Re-enumerated from scratch; the pair set is unchanged.

| Pair | Direction | Basis |
| :--- | :--- | :--- |
| DF ↔ Capture Mode | both | `capture-mode/prd-capture-mode.md:421` + `:423` target Data Foundation; DF `:231` inbound, `:236` the 21-row map, `:246`–`:249` outbound ×4 |
| DF ↔ Device Management | both | `device-management/prd-device-management.md:300`; DF `:232` inbound, `:250`, `:251` outbound |
| DF ↔ Inventory Import | both | `import/prd-inventory-import.md:169`; DF `:234` inbound, `:252` outbound |
| DF ↔ Data Export | both | DF `:233` inbound / `:242` outbound; DE `:170` inbound / `:179` outbound |
| DF ↔ Vision | DF→vision | DF `:253` outbound (post-lock on `vision.md:163`) |
| DE ↔ Capture Mode | both | DE `:171` inbound (R1.9, R4.24), `:181` outbound |
| DE ↔ Device Management | both | `device-management/prd-device-management.md:300`, `:301`; DE `:172` inbound (R6.5, R1.21), `:180` outbound |
| DE ↔ Inventory Import | both | DE `:173` inbound (R2.2), `:182` outbound |

DF also names Collection Mode (`:243`), QC & Comparison (`:244`) and Telemetry (`:245`) as targets; those PRDs do not exist (`README.md:18`–`:21`, "queued"), so no pair is checkable. A fresh search of the three locked siblings, `vision.md`, `AGENTS.md`, the root `README.md` and `docs/decisions/README.md` for links into `data-foundation/` or `export/` returns **nothing** — only `README.md` links in (`:16`, `:17`, `:49`, `:55`, `:88`, `:104`). Every DF/DE reference in a sibling is by prose name.

### (a) Inherited obligations agree both ways — **CLEAN**

*(was 1a-1, POST-LOCK — closed.)* `docs/decisions/README.md:22` now reads "Storage schema: **the stored mean canonical with the raw payload archived beside it**, version history, derived-value recompute, and the schema-migration mechanism". The fence-F10 phrasing now reconciles across every site that carries it: `README.md:105` ("stored mean canonical, raw payload archived beside it"), `README.md:125`, `docs/decisions/README.md:22`, `AGENTS.md:105`–`:106`. **`vision.md:163` is the sole remaining site**, and it is exactly what DF `:253` now names — the loop is closed.

*(was 1a-2, EDITORIAL — closed.)* DF `:253` now reads "Post-lock amendment there: [the v1 feature list](../vision.md#v1)'s "raw payload canonical" phrasing is the stored mean, the payload archived beside it (fence F10)". The stale "and the README's two matching rows" clause is gone; the line names only work that is genuinely outstanding.

Re-verified clean this pass, derived independently:

- **The 21-row map is complete and exact.** `capture-mode/prd-capture-mode.md:421` lists R1.9, R3.1, R3.6, R3.8, R4.6, R4.11, R4.12, R4.13, R4.24, R6.7, R6.9, R7.12, R7.18, R8.5, R8.9, R8.13, R8.17, R11.6, R11.10, R11.11, R11.16 — 21 rows. DF `:236` names all 21 and no others (6+1+4+1+1+4+3+1 = 21), with R6.9 explicitly carried by no DF row and handed to Collection Mode (mirrored at DF `:243`) and R11.16 → R6.5 under fence F32. `capture:423`'s second Data Foundation line (the gamut mark) is named as carried whole by R3.4 (DF `:231`, `:236`).
- **DF ↔ DE mirrors one-for-one.** DF `:233` Rows-here cell = R6.5, R7.2, R7.3, R7.7 ↔ DE `:179` names exactly those four DF rows; DE `:179` Rows = R4.1, R4.2, R4.3 ↔ DF `:233` names its R4.1, R4.2, R4.3 (plus its R1.1 as the source of the not-exportable states). DF `:242` Rows cell = R1.2, R2.1, R2.2, R2.4, R2.9, R3.1, R6.5 (7) ↔ DE `:170` names the identical seven and lists seven DE rows (R1.1, R1.2, R1.3, R2.1, R2.3, R4.2, R4.3). The fixture list at DF `:220` and DE `:179` matches item-for-item (see the informational note below on its item count).
- **DF ↔ Device, DF ↔ Import** match exactly (`device:300` R1.21+R1.22 ↔ DF `:232`; `import:169` R2.3+R3.2 ↔ DF `:234`).
- **DE ↔ Device, DE ↔ Capture, DE ↔ Import** each have both halves recorded (DE `:172`↔`:180`, `:171`↔`:181`, `:173`↔`:182`), including "its R1.21 snapshot obligation names this PRD too" (`:180`) and "its R4.24 basis obligation names this PRD too" (`:181`).
- `device:301` ("Data Foundation, export | CSV export emits a `simulated` column | R6.5") names Data Foundation as a co-target with no DF counterpart inbound line. **Not a miss:** DE `:180` records the re-target under fence F6; the column is wholly DE's.
- Capture and Import carry no inbound obligations table; every obligation DF and DE push at them is logged as a post-lock line (DF `:246`–`:249`, `:252`; DE `:181`, `:182`).

**Misses: none.**

### (b) Shared rows agree on priority — **CLEAN**

- Capture R8.9 **P1** (`capture:318`) vs DF R2.3 **P0**: reconciled in DF at `:236` ("Its R8.9 is P1 where [R2.3] is P0 because that PRD's fence F24 puts the record shape at P0 and the correction path at P1").
- Capture R6.9 **P1** (`capture:241`): carried by no DF row, stated at DF `:236` and DF `:243`.
- Every other handed row is **P0** upstream and **P0** here, re-verified against each sibling's Pri cells: capture R1.9, R3.1, R3.6, R3.8, R4.6, R4.11, R4.12, R4.13, R4.24, R6.7, R7.12, R7.18, R8.5, R8.13, R8.17, R11.6, R11.10, R11.11, R11.16; import R2.3, R3.2; device R1.21, R1.22, R6.5.
- Pri census re-derived: DF **45 P0 + 1 P1** (R6.3, `:200`) = 46, matching its Legend's "P1 is the delete undo (fence F16)". DE **12 P0 + 1 P1** (R1.3, `:120`) = 13, matching its Legend's "P1 is the history export (R1.3, fence F2)".
- Conditionals matched both ways: DF R7.2 `:215` (×2) and M9 `:272` gate on "once [R6.3] lands"; DF R6.2 `:199` gates on "until its history option lands" and the DF copy header (`prd-data-foundation-copy.md:4`) names the export PRD's R1.3 as that gate; DE R4.1 `:157` and R4.2 `:158` gate on "once [R1.3] lands".

**Misses: none.**

### (c) Cross-PRD cites name the owning document — **CLEAN**

Standing grep, re-run over all ten files:

```
grep -nE '\[(R|E|M|F|OQ ?|DJ|EJ|UJ|J)[0-9][^]]*\]\(\.\./' data-foundation/prd-data-foundation*.md export/prd-data-export*.md
→ no output (exit 1)
```

All **862** links in the ten files were enumerated and every cross-directory label tabulated (full census re-run; 176 distinct labels). Every one is (i) an explicit owner label — `the capture PRD's R4.24`, `the device PRD's R1.21`, `the import PRD's R2.3`, `the export PRD's R2.3`, `the Data Foundation PRD's R7.7`, `vision J4`; (ii) an `its …` / `Its …` / `that PRD's …` anaphor inside an obligations-table cell whose Source/Target column names the document (DF `:231`–`:234`, `:242`–`:253`; DE `:170`–`:173`, `:179`–`:182`); or (iii) an `its …` anaphor whose antecedent is an explicitly labelled cite in the same sentence (DF `:131`, `:132`, `:146`, `:167`, `:202`, `:236`; DE `:118`, `:120`, `:133`, `:158`, `:159`; DE Traceability `:96`; DF fence map `:181`–`:207`). **No bare-ID label exists.** Non-link prose cites (DF `:236`'s 21-row map, DE `:179`'s "its R6.5") sit inside cells whose Source/Target column names the owner. DF `:253`'s new shorter form uses the non-ID label `the v1 feature list` inside a cell whose Target column reads "Vision" — conforming.

Cross-PRD fence cites are likewise all qualified, re-checked at source: `the export PRD's F1`–`F10` (DF fence map, each preceded by "moved to the export PRD's …"), `its F31` (DE `:15`, `:170`), `its fence F27` (DE `:120`), `that document's fence F30` (DE `:67`), `that PRD's fence F24` (DF `:236`, the capture PRD's). Unqualified `fence Fn` is always the citing document's own.

**Misses: none.**

### (d) Retired and moved IDs — **CLEAN**

Every retired ID surviving in preserved fence prose is qualified. Full re-scan of the DF fence file for `M3 | OQ 8 | OQ 9 | OQ 11 | OQ 16 | R4.1–R4.9 | R7.9 | R7.6a | E6 | E7 | E17 | E18 | DJ1` returns 14 hits, each carrying "now the export PRD's …" or "moved to the export PRD's …" (`:35`, `:37`, `:93`, `:109`, `:113`, `:119`, `:133`, `:149`, `:200`, `:202`, `:203`), plus `:204`'s "(its R1.10, R4.5)" whose antecedent is the named Capture Mode line. The only unqualified occurrences are inside **F30 itself** (`:155`), the fence that declares the move — self-qualifying. The other four DF files carry only qualified hits: `prd-data-foundation-oq-results.md:3` ("OQ 8's and OQ 9's sections moved to the export PRD's results file … those numbers are retired here"), `prd-data-foundation-copy.md:4` ("E6, E7, E17 and E18 — moved to the export PRD's copy file"), `prd-data-foundation.md:49` (DJ1), `:83` (the Legend list), and the live cross-PRD cites `the export PRD's R4.1/R4.2/R4.3` (`:202`, `:208`, `:220`, `:221`, `:233`) and the capture PRD's `R4.5`/`R4.6` (`:231`, `:236`, `:248`).

DF's Legend retired list (`:83`) and DE's Traceability table (`:71`–`:94`) reconcile **one-for-one, all 24 pairs**, including the three split rows (R7.7, R7.8, R7.6) reconciled in prose on both sides (DF `:83`, DE `:96`).

No retired ID is reused as a live ID — re-derived from the row-ID inventories: DF has 46 live R rows (R1.1–R1.10, R2.1–R2.9, R3.1–R3.5, R5.1–R5.8, R6.1–R6.6, R7.1–R7.8) with **no §4 rows, no R7.9, no R7.6a** (surfaces are R7.6b–R7.6n), 8 metrics **M1, M2, M4–M9 (no M3)**, 26 copy states **E1–E5, E8–E16, E19–E30 (no E6/E7/E17/E18)**, **DJ2–DJ5 (no DJ1)**, and OQs **1–7, 10, 12–15 (no 8/9/11/16)**.

**Misses: none.**

### (e) Fence numbering and fence → row maps — **CLEAN**

Numbering is per PRD (DF F1–F32, DE F1–F13) with no overlap in meaning, and every cross-PRD fence cite is qualified (above). Map completeness re-counted against the fence headings themselves: **DF 32 headings / 32 map rows** (`prd-data-foundation-fences.md:177`–`:208`), **DE 13 / 13** (`prd-data-export-fences.md:95`–`:107`).

Forward direction re-derived from scratch: every fence cite in both PRD bodies was extracted per line, tagged with its row ID or `(non-row)`, and matched against its map entry. **All cites resolve, both documents** — including the two the prior sweep flagged:

- *(was 1e-1, EDITORIAL — closed.)* F8's map entry (`prd-data-foundation-fences.md:184`) now reads `R6.6 …; the Telemetry and help-docs line in Inherited obligations …; **§8's Error & State Copy paragraph**; OQ 12's ownership half.` — covering the F8 cite at `prd-data-foundation.md:257`.
- *(was 1e-2, EDITORIAL — closed.)* F30's map entry (`:206`) now reads `The Legend's retired-ID list; **the Open Questions' retirement note**; the Background scope statement; …` — covering the F30 cite at `prd-data-foundation.md:291`.

Every other non-row cite resolves too: F1↔`:15` (Background scope statement), F2/F14↔`:247` (the Capture Mode OQ 19 line), F3/F12↔`:246` (the Capture Mode E29 line) and `:280` (OQ 3, F12's entry naming "OQ 3's amendment"), F10↔`:62` (Vocabulary) and `:253` (the Vision line), F14↔`:250` (Device Management), F16↔`:79` (the Legend's P0/P1 split), F8/F19↔`:245` and `:286` (OQ 12's two halves), F25/F27/F28/F31/F30↔`:242` (the outbound Data Export line), F32↔`:236` and `:249` (the Capture Mode line). DF's `:236` also cites **the capture PRD's** F24 — qualified, so no DF map entry is owed. DE's F27/F30/F31 body hits are all qualified cross-PRD cites into DF (`:15`, `:67`, `:120`, `:170`).

Reverse direction (a map entry naming a row that cites no fence) occurs in both maps and is **not a defect**: both maps define "carries" as "the fence's decision is what the row now states" (`prd-data-foundation-fences.md:173`, `prd-data-export-fences.md:91`). DF F24 and DE F3/F12 are reverse-only and correctly so.

**Misses: none.**

---

## CHECK 2 — Index sync

### (i) Fence → row maps vs body cites — **CLEAN, both documents**
Covered in 1(e): forward direction clean in both documents, **both prior residue items (1e-1, 1e-2) closed**. Both map tables complete (32/32, 13/13).

### (ii) Surfaces tables — **CLEAN, both documents**

DF's 13 surfaces (`:100`–`:112`) yield a Copy-ID union of E1–E5, E8–E16, E19–E30 = **exactly the 26 states** in `prd-data-foundation-copy.md`, re-derived cell by cell. Every Req-ID named (R1.3, R1.5, R1.7–R1.10, R2.4, R2.6, R2.8, R2.9, R3.2, R3.5, R5.1–R5.8, R6.2, R6.3, R6.6) exists in the 46-row inventory. **Containment holds in every one of the 13 rows**: each Copy ID's producing row is among that surface's Req-IDs — R7.6d's Req-IDs include **R1.5** (`:102`), which produces E10.

§8 `:257` reads "**E4, E10, E11 and E28–E30** are listed on two surfaces because each opens from both." Re-derived, the set of states appearing on ≥2 surfaces is exactly E4 (R7.6g, R7.6h), E10 (R7.6b, R7.6d), E11 (R7.6g, R7.6i), E28/E29/E30 (R7.6b, R7.6m) — **exact match, no more and no fewer**.

The two "—" Surfaces cells are R7.6j (a number, R2.6) and R7.6n (wording the Telemetry PRD and the help docs own, R6.6) — exactly the two §8 `:257` names. R7.6d `:102`'s "What the test lists" cell carries no state link: it reads "a local disk, a syncing folder or a network volume" in plain words.

*Note (informational, not a miss):* a strict "every E appears in exactly the Surfaces rows its producing rows imply" reading does not hold by design — R5.1/R5.5/R5.8 sit in both R7.6b's and R7.6m's Req-IDs but produce E12 only on R7.6m, and R5.5 produces E4 on R7.6g/h rather than R7.6b. The operative rule is coverage (union = the live state set) plus containment, and both hold.

DE's one surface (`:104`, R4.4a) is clean: Req-IDs R1.1–R1.3, R2.1–R2.5, R3.1 and Copy IDs E1–E4 all exist, E1←R1.3/R2.4 and E2–E4←R3.1 all within the Req-ID set, no duplication.

### (iii) Copy states ↔ producing rows — **CLEAN, both documents**

DF: all 26 states are cited by at least one requirement row — re-derived per row: E1←R5.3; E2←R5.1; E3←R5.2; E4←R2.9, R5.5; E5←R2.2, R5.5, R7.3; E8←R6.2; E9, E10←R1.5; E11←R2.4, R2.8, R2.9; E12←R5.1, R5.8; E13←R1.8; E14←R6.2; E15←R1.10; E16←R5.7; E19, E21←R1.9, R7.8; E22←R1.3; E23←R5.2, R5.7; E24←R5.8; E25, E27←R1.7; E26←R2.8; E28, E30←R5.5, R5.8, R7.3. E20 and E29 are cited only inside the ranges `[E19]–[E21]` (`:128`, `:221`) and `[E28]–[E30]` (`:183`, `:187`, `:216`), the documents' established form. Every E cited by a row exists. The two out-of-document cites — `the export PRD's E1` at DF `:199` (R6.2) and `the capture PRD's E26` at DF `:216` (R7.3) — are both qualified.

DE: E1←R1.3, R2.4; E2/E3/E4←R3.1. All four exist, all cited.

### (iv) Journey indexes ↔ companions — **CLEAN, both documents**

DF's index (`:53`–`:56`) reads DJ2–DJ5, matching the companion headings (`prd-data-foundation-journeys.md:10`, `:31`, `:58`, `:80`) and their anchors, with the DJ prefix and DJ1's retirement stated at `:49` and mirrored at the companion's `:6`. Rows-exercised agree for all four (DJ2 R2.2–R2.5 + R2.8; DJ3 R2.9 + R5.1–R5.8; DJ4 R6.1–R6.4; DJ5 R1.1–R1.3 + R1.6 + R3.2 + R5.6): the companion's full row extract is R1.1, R1.3, R1.6, R2.2, R2.5, R2.8, R2.9, R3.2, R5.1, R5.6, R5.8, R6.1, R6.4 — every one inside its journey's declared span, plus OQ 2 and the `[DJ2]` back-link. DE's journey is EJ1 throughout (index `:39`, Traceability `:94`, companion heading `:8`, anchor `#ej1-export-the-collection`), its index row carries "and the Data Foundation PRD's R3.4, R3.5" matching the companion, and every row it cites (R1.1, R1.3, R2.1, R2.5, R3.1) exists. The only `J1`–`J7` tokens left are `vision J4` (DE `:9`, `:112`, journeys `:10`) and `vision J7` (DE `:202`), all qualified.

### (v) Answered OQs ↔ results files — **CLEAN, both documents**

Re-derived from the Status column. DF answered = **1, 3, 4, 7, 10**; the results file holds `## OQ 1`, `3`, `4`, `7`, `10` — five sections, one-for-one, no orphans. DF open = 2, 5, 6, 12, 13, 14, 15, with no sections. DE answered = **1, 2**; results holds `## OQ 1`, `## OQ 2`. DE open = 3, 4.

### (vi) Copy header markers — **CLEAN, both documents**

DF's header (`prd-data-foundation-copy.md:4`) defines four markers and all four are used, with no fifth: **‹floor› ×2** (definition + E16's state name, `:16`), **‹OQ 14› ×2** (definition + E9's body, `:26`), **‹P1› ×6** (two in the header, two each in E8 `:23` and E14 `:24`), **‹until P1› ×7** (one in the header, three each in E8 and E14). Every use leads a whole sentence or trails a whole action, as the header requires. The header names the cross-PRD seam explicitly ("E8's and E14's full-history sentence waits on the export PRD's R1.3"). DE's header (`prd-data-export-copy.md:4`) defines ‹P1› only and uses it **×3** (definition, E1's body sentence, E1's second action `:16`) — no undefined marker, no unused definition. **No marker glyph appears in any of the other eight files** (all report 0).

### (vii) Provisional constants and Feeds cells — **CLEAN**

An UPPER_CASE sweep of all ten files returns **twelve** tokens and no thirteenth: SQLITE_READER_FLOOR (16), ROWS_CEILING (12), DERIVATION_VERSION (6), WAVELENGTH_GRID (5), HISTORY_RETENTION (5), GAMUT_RENDERING_INTENT (5), GAMUT_REFERENCE_SPACE (5), DELETE_UNDO_WINDOW (5), STORE_SIZE_BUDGET (4), INTEGRITY_CHECK_BUDGET (3), DERIVATION_TOLERANCE (3), COMPATIBILITY_FLOOR (3) — the same twelve, at the same frequencies, as at `1e8830a`. Every one carries an OQ id or a fence at its defining use site: SQLITE_READER_FLOOR → OQ 2 (DF `:279`); STORE_SIZE_BUDGET → OQ 5 (`:282`); DERIVATION_TOLERANCE → OQ 6 (`:283`); INTEGRITY_CHECK_BUDGET → OQ 13 (`:287`); HISTORY_RETENTION → fence F6 + OQ 4 (`:153`, `:281`); DELETE_UNDO_WINDOW → fence F7 + OQ 10 (`:200`, `:285`); GAMUT_* → fence F4 + OQ 7 (`:168`, `:284`); WAVELENGTH_GRID → OQ 4 (DE `:203`); DERIVATION_VERSION declared "a stamp, not a constant" (DF Legend `:81`, DE Legend `:56`); ROWS_CEILING the capture PRD's OQ 13 (DF `:81`, DE `:56`); COMPATIBILITY_FLOOR appears **only in fence prose** (`prd-data-foundation-fences.md` ×2, `prd-data-export-fences.md` ×1 — zero hits in either PRD body) and is declared not to exist until a release raises the floor (DF R5.7 `:181`) — not a latent decision.

All seven "per SDK docs" markers carry an OQ id: DF `:116` (OQ 12, OQ 15), `:146` (OQ 15), `:165` (OQ 15), `:286` and `:289` (inside their own OQ rows); DE `:108` ("the Data Foundation PRD's OQ 15") and `:203` (inside OQ 4's row). The two remaining hits (DF `:293`, DE `:205`) are the rule statements themselves, not markers.

Feeds cells re-derived: **DF OQ 2's Feeds (`:279`) names R1.2, R5.1, R5.2, R5.6, R6.5, R7.1, R7.2, R7.3, M4, M9 — exactly the ten DF rows that use SQLITE_READER_FLOOR** (independently derived at `:129`, `:180`, `:182`, `:184`, `:202`, `:214`, `:215`, `:216`, `:267`, `:272`), no more and no fewer. **DE OQ 4's Feeds (`:203`) names R2.3, R2.5 and R4.1** — R2.3 (`:133`) and R2.5 (`:135`) being the two DE rows that use WAVELENGTH_GRID, and R4.1 named in its own Interim-rule cell.

### (viii) Word counts

Method as specified — strip HTML comments, link targets `](…)`, code fences and table pipes, then count whitespace-separated tokens. **Method validated by reproduction:** the script returns 7,922 / 3,942 at `5f428e6` and 7,997 / 4,000 at `1e8830a`, matching both prior sweeps exactly.

| Document | Count | Budget | Headroom |
| :--- | :--- | :--- | :--- |
| `data-foundation/prd-data-foundation.md` | **7,991** | 8,000 (fence F21, raised 2026-09-14) | **9** |
| `export/prd-data-export.md` | **4,000** | 4,000 (DE fence F1) | **0** |

Both within budget. DF fell **6 words** from `1e8830a`'s 7,997 — exactly the clause trimmed from the Vision line at `:253` ("and the README's two matching rows") — restoring headroom from 3 to 9. DE is byte-identical to `1e8830a` and remains **exactly on its cap**: any further addition to the DE body needs a compaction pass or an F1 amendment. Informational, not a miss.

### (ix) README sync — **CLEAN**

`README.md:16` "| 4 | [Data Foundation](…) | U5, U6 | **Locked** — review gate closed 2026-09-16 after 12 rounds, both PRDs under one log; owner merge pending |" ↔ `prd-data-foundation.md:3` "Status: locked (2026-09-16)". `README.md:17` "| 5 | [Data Export](…) | U6 | **Locked** — split out of Data Foundation on 2026-09-09 under its fence F30 and locked with it 2026-09-16 |" ↔ `prd-data-export.md:3` "Status: locked (2026-09-16)". Both rows 4–5 read **Locked**. `:49` and `:55` describe the two five-file shapes; all eight companions exist. `:88` routes U6 to both; `:104` routes the CSV P0 to Data Export; `:117` records the split and the lock. `AGENTS.md:86`–`:87` list both directories; `AGENTS.md:105`–`:106` carry the fence-F10 vocabulary amendment. No stale or missing row. `docs/decisions/README.md:22` — the one substantive index item the prior sweep flagged — is now aligned with `README.md:125`, which links to it.

**Misses: none.**

---

## CHECK 3 — Latent-decision inventory

| # | Item | Location | Found | Class |
| :--- | :--- | :--- | :--- | :--- |
| — | Constants with no OQ or fence | all ten files | **None.** Twelve tokens, every one anchored. See 2(vii) | — |
| — | Copy states with no producing row | both copy files | **None.** 26/26 and 4/4. See 2(iii) | — |
| — | Rows an upstream PRD places at a higher priority | — | **None.** Capture R8.9 P1 / DF R2.3 P0 and capture R6.9 P1 / no DF row are both stated in DF (`:236`, `:243`); every other handed row is P0 on both sides. DF's only P1 (R6.3) and DE's only P1 (R1.3) have no upstream counterpart | — |
| — | Undefined terms the rows lean on | both Vocabulary sections | **None outstanding.** "volume class" appears **exactly once across the ten files** — M1 `:265` ("each recording its volume class", "read per volume class") in the durability sense, whose classes R7.4 `:217` enumerates. The warning sense is worded "the file's location" / "local, sync-managed, or a network volume" (R1.7 `:133`, R7.2 `:215`, R7.6d `:102`, R7.6e `:103`). One taxonomy, one phrase. *Informational:* defined in-row (R7.4) rather than in the Vocabulary | — |
| — | "working set" | `prd-data-foundation.md:72`, used `:214` | **Resolves.** The Chosen-condition entry reads "a reading's working set is the set the app works it from ([R3.1])", and R3.1 `:165` says "the set for the collection's chosen scan mode … is the one the app works from". R7.1 `:214`'s use reads correctly against that. Defined inside another entry rather than as its own bullet; no ambiguity | — |
| — | `{{placeholder}}` | all ten files | **0** | — |
| — | `TBD` / `TODO` / `XXX` / `FIXME` | all ten files | **0** | — |
| — | HTML comments | all ten files | **0** in every one of the ten | — |
| — | "Owner:" / author line | all ten files | **0** (both fence files declare the shape as "no author line", DF F1 `:11`, DE F1 `:11`) | — |
| — | Status cell not one of the Legend's six | both PRDs + both copy files | **None.** The other five glyphs (⌛️ ✋ 🦺 ✅ ✂️) appear **only** inside the two Legend blocks (`prd-data-foundation.md:87`–`:88`, `prd-data-export.md:60`–`:61`) | — |
| — | Rows whose Status is not 🤝 Aligned | both PRDs + both copy files | **Zero.** A grep for any non-🤝 status cell across the four dispositionable tables returns nothing. Counts: DF **67** 🤝 (46 R rows + 13 surfaces + 8 metrics) and **26** 🤝 states; DE **15** 🤝 (13 R rows + 1 surface + 1 metric) and **4** 🤝 states | — |
| — | Headers | `prd-data-foundation.md:3`, `prd-data-export.md:3` | Both read **"Status: locked (2026-09-16)"** | — |
| — | Product README rows 4–5 | `README.md:16`, `:17` | Both read **Locked** | — |
| — | Fix files under the two directories | `data-foundation/`, `export/` | **None.** Exactly five `.md` files each; no `*-fix*` file | — |

---

## Link and anchor resolution

| Set | Links checked | Broken |
| :--- | :--- | :--- |
| The ten files (every relative path + every `#anchor` against the target file's real headings, strict GitHub slug — underscores kept, `*` emphasis stripped, em-dashes vanishing, duplicate-heading `-1` suffixing) | **862** | **0** |
| All product docs — the ten, the three siblings and their companions, `vision.md`, `product/README.md`, plus root `README.md`, `AGENTS.md`, `docs/decisions/README.md` (30 files) | **2,197** | **0** |

**No genuine miss anywhere.** Both totals reproduce the `1e8830a` sweep exactly, confirming `4e9826f` added and removed no link.

**Slugger artefacts (not misses).** Re-run with a `_`-stripping slugger, three links report as broken — `prd-data-export-fences.md:43` → `#f4--the-gamut-clipped-export-column-is-sc_srgb_gamut_clipped-2026-09-09` and both links at `prd-data-export-oq-results.md:11`. All three resolve under the strict GitHub slugger (GitHub does not treat a literal underscore inside a heading as emphasis), which is the correct one. Result is 862/0 and 2,197/0 under the strict variant, so no finding depends on the choice. The em-dash rule behaves as described ("F8 — The CSV" → `f8--the-csv`), and the `sc_`-bearing fence headings are the only place the underscore matters.

---

## Counts

| Class | Count | Items |
| :--- | :--- | :--- |
| **EDITORIAL** | **0** | — |
| **POST-LOCK** | **0** | — |
| **DECISION** | **0** | — |
| Informational | 3 | DE at exactly 4,000/4,000 words (DF has 9); "volume class" defined in-row (R7.4) rather than in the Vocabulary; the R7.7 fixture list reads as 14 clauses where the prior sweep labelled it "13-item" (the two collision fixtures read as one concept if grouped) — DF `:220` and DE `:179` match item-for-item either way, so the label is a nuance, not a divergence |
| Clean checks | **all** | 1(a) obligations both ways incl. the DF↔DE one-for-one mirror and the 21-row map, 1(b) priorities, 1(c) cites, 1(d) retired-ID ledger, 1(e) fence numbering + both fence→row maps; 2(i)–2(ix) in full; 3 in full (constants, copy↔rows, upstream priority, terms, placeholders, comments, author lines, statuses, non-Aligned rows, headers, README, fix files); link/anchor resolution (862/0 and 2,197/0) |

Closed since `1e8830a`: **all three editorial items** (1a-2, 1e-1, 1e-2) and **the one post-lock item** (1a-1). Closed since `5f428e6`: all nine editorial and all three post-lock items.

## Verdict

**The state that locks is clean on every mechanical precondition, with no residue.** This is the first sweep in the series to return a fully empty miss table.

Both hard gates pass outright. Check 1(c) is clean on the standing grep across all 862 links in the ten files, with every cross-directory label an explicit owner label or an anaphor with a named antecedent in the same cell or sentence — including DF `:253`'s newly shortened form, whose `the v1 feature list` label sits in a cell whose Target column reads "Vision". The retired/moved ID ledger reconciles one-for-one in both directions across all 24 pairs, with no reuse anywhere in the live ID space and no unqualified retired ID left in the preserved fence prose.

The three editorial fixes land exactly where the misses were. F8's map entry now names §8's Error & State Copy paragraph and F30's names the Open Questions' retirement note, closing the last two instances of the "a fence cited from a non-row statement whose map entry does not name that statement" class — a class that had survived three sweeps. DF `:253` no longer claims the README rows are outstanding, which they have not been since the lock commit fixed them.

The post-lock item is closed at its source rather than papered over. `docs/decisions/README.md:22` now carries the same fence-F10 phrasing as `README.md:105`, `README.md:125` and `AGENTS.md:105`–`:106`, so the decision queue that ADR-0003 will be written from — the sharpest one-way door in the project — no longer contradicts the PRD that gates it. `vision.md:163` is now the single remaining site of the old "raw payload canonical" wording, and it is precisely and solely what DF `:253` records as post-lock work.

Every derived index reproduces its stated counterpart on independent re-derivation: 32/32 and 13/13 fence maps, a 26-state Surfaces union against a 26-state copy file, a double-listed set of exactly {E4, E10, E11, E28, E29, E30}, five and two OQ result sections against five and two answered questions, four and one journey against four and one companion heading, twelve anchored constants, and Feeds cells naming exactly the ten and three rows that use their constants.

The one number to watch is unchanged: DE's body sits at 4,000 words against a 4,000-word budget, zero headroom. DF now has 9, up from 3. Neither blocks the lock, but any later amendment to the DE body needs a compaction pass or an F1 amendment first.
