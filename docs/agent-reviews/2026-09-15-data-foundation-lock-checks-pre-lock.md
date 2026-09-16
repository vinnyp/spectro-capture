# Lock preconditions — mechanical sweep at HEAD `5f428e6`

**Scope.** Data Foundation (DF) and Data Export (DE), five files each, in
`/Users/vinnypasceri/Projects/.worktrees/spectro-capture__docs-data-foundation` (branch `docs/data-foundation`, HEAD `5f428e6`).
Read-only; nothing edited, staged or committed. Re-runs the three checks of
`docs/agent-reviews/2026-09-10-data-foundation-lock-checks-pre-round-6.md` (run at `69a69df`), whose item numbers are cited below as
*(was 1a-2)* etc. Landed since: `prd-data-foundation-lock-checks-fixes.md` (LC-1…LC-11), `-round-6-fixes.md` (FX6-1…FX6-31),
`-round-7-fixes.md` (FX7-1…FX7-25). Line numbers are as of `5f428e6`. Paths are relative to `docs/product/` unless shown otherwise.

Classification key — **EDITORIAL**: a link target, label, index or map entry fixable without changing a rule. **POST-LOCK**: a locked sibling must change. **DECISION**: needs an Open Question or an owner call.

---

## CHECK 1 — Cross-PRD consistency

### Pairs enumerated

X ∈ {DF, DE}. Y = any PRD X cites or that cites X.

| Pair | Direction | Basis |
| :--- | :--- | :--- |
| DF ↔ Capture Mode | both | capture `prd-capture-mode.md:421` + `:423` target Data Foundation; DF `:237` inbound, `:242` the 21-row map, `:252`–`:255` outbound ×4 |
| DF ↔ Device Management | both | device `:300`, `:301`; DF `:238` inbound, `:256`, `:257` outbound |
| DF ↔ Inventory Import | both | import `:169`; DF `:240` inbound, `:258` outbound |
| DF ↔ Data Export | both | DF `:239` inbound / `:248` outbound; DE `:176` inbound / `:185` outbound |
| DF ↔ Vision | DF→vision | DF `:259` outbound (post-lock on `vision.md:163`) |
| DE ↔ Capture Mode | DE→capture | DE `:177` inbound (R1.9, R4.24), `:187` outbound |
| DE ↔ Device Management | both | device `:301`; DE `:178` inbound (R6.5, R1.21), `:186` outbound |
| DE ↔ Inventory Import | DE→import | DE `:179` inbound, `:188` outbound |

DF also names Collection Mode (`:249`) and QC & Comparison (`:250`) and Telemetry (`:251`) as targets; those PRDs do not exist (`README.md:18`–`:21`, "queued"), so no pair is checkable. A fresh search of the three locked siblings for links into `data-foundation/` or `export/` still returns **nothing** — only `README.md` links in (`:16`, `:17`, `:49`, `:55`, `:88`, `:104`). Every DF/DE reference in a sibling is by prose name.

### (a) Inherited obligations agree both ways

Resolved since `69a69df`: **was 1a-1, 1a-4** (capture's R4.5/R1.10 hand-over and the R1.9 re-target now recorded, DF `:254` / DE `:187`); **was 1a-5, 1a-6** (DE's inbound Capture Mode and Device Management lines now carry R4.24 and R1.21, DE `:177`, `:178`); **was 1a-7** (DF's outbound Inventory Import line, DF `:258`); **was 1a-2** (the 21-row map, DF `:242`).

Verified clean this pass:

- **The 21-row map is complete and exact.** `capture-mode/prd-capture-mode.md:421` lists R1.9, R3.1, R3.6, R3.8, R4.6, R4.11, R4.12, R4.13, R4.24, R6.7, R6.9, R7.12, R7.18, R8.5, R8.9, R8.13, R8.17, R11.6, R11.10, R11.11, R11.16 — 21 rows. DF `:242` names all 21 and no others (6+1+4+1+1+4+3+1 = 21), with R6.9 explicitly carried by no DF row and handed to Collection Mode (mirrored at DF `:249`) and R11.16 → R6.5 under fence F32. The second Data Foundation line (`capture:423`, the gamut mark) is named as carried whole by R3.4.
- **DF ↔ DE mirrors one-for-one.** DF inbound `:239` names DE R4.1, R4.2, R4.3 ↔ DE outbound `:185` Rows = R4.1, R4.2, R4.3. DE outbound `:185` names DF R7.7, R7.2, R7.3, R6.5 ↔ DF inbound `:239` Rows here = R6.5, R7.2, R7.3, R7.7. DF outbound `:248` names DF R1.2, R2.1, R2.2, R2.4, R2.9, R3.1, R6.5 ↔ DE inbound `:176` names the identical seven and lists seven DE rows (R1.1, R1.2, R1.3, R2.1, R2.3, R4.2, R4.3). The 13-item fixture list at DF `:226` and DE `:185` matches item-for-item (only the position of "a confirmed correction in history" differs — content identical; informational).
- **DF ↔ Device, DF ↔ Import** match exactly (`device:300` ↔ DF `:238`; `import:169` ↔ DF `:240`).

| # | Location | Expected | Found | Class |
| :--- | :--- | :--- | :--- | :--- |
| 1a-1 | `export/prd-data-export.md:177` vs `capture-mode/prd-capture-mode.md:186`, `:421` | DE's inbound Capture Mode line names two capture rows (R1.9, R4.24); each has a recorded counterpart naming Data Export | Only **R1.9**'s re-target is recorded, at DE `:187` and DF `:254`. Capture R4.24's obligation row names "the Data Foundation PRD" alone, and **no outbound line in either document records that it must also name Data Export**. Mitigated in substance: the basis also reaches DE through DF `:248` ("[R2.1]'s … basis") | **POST-LOCK** (extend DE `:187`) |
| 1a-2 | `export/prd-data-export.md:178` vs `device-management/prd-device-management.md:300` | Same, for the two device rows DE's inbound line names (R6.5, R1.21) | Only **R6.5** is recorded, at DE `:186`. Device `:300`'s R1.21 obligation targets Data Foundation only; nothing records that it also reaches Data Export, though DE R1.1 (`:124`) cites `the device PRD's R1.21` directly. Mitigated the same way, via DF `:248`'s "device snapshot" | **POST-LOCK** (extend DE `:186`) |
| 1a-3 | `README.md:105`, `README.md:125` vs `data-foundation/prd-data-foundation.md:259` | The "raw payload canonical" phrasing DF's outbound Vision line corrects is named wherever it appears | DF `:259` names only `vision.md:163` ("[the v1 feature list](../vision.md#v1)"). The same phrasing stands at `README.md:105` (the P0 feature row) and `README.md:125` (the ADR-0003 scope row) | **POST-LOCK** (low — extend the recorded line) |
| — | Capture and Import carry no inbound obligations table | A two-way table in each sibling | Unchanged and correctly recorded: every obligation DF and DE push at them is logged as a post-lock line (DF `:252`–`:255`, `:258`; DE `:187`, `:188`). **No action** | — |

### (b) Shared rows agree on priority — **CLEAN**

- Capture R8.9 **P1** vs DF R2.3 **P0**: the reconciliation is now stated in DF itself, at `:242` ("Its R8.9 is P1 where [R2.3] is P0 because that PRD's fence F24 puts the record shape at P0 and the correction path at P1"). *(was 1b-1)*
- Capture R6.9 **P1**: carried by no DF row, stated at DF `:242` and DF `:249`. *(was 1b-2)*
- Every other handed row is **P0** upstream and **P0** here: capture R1.9/R3.1/R3.6/R3.8/R4.6/R4.11/R4.12/R4.13/R4.24/R6.7/R7.12/R7.18/R8.5/R8.13/R8.17/R11.6/R11.10/R11.11/R11.16; import R2.3, R3.2, R2.2; device R1.21, R1.22, R6.5, R6.9, R6.12, R6.13, R6.17, R6.20. Verified against each sibling's Pri cells.
- Pri cells: DF has exactly one P1 (R6.3) and 45 P0, matching its Legend (`:85`). DE has exactly one P1 (R1.3) and 12 P0, matching its Legend (`:60`).
- Conditionals matched both ways: DF R7.2 `:221` and M9 `:278` gate on "once [R6.3] lands" (R6.3 P1); DF R6.2 `:205` and the DF copy header (`prd-data-foundation-copy.md:4`) gate on "the export PRD's R1.3" (DE R1.3 P1); DE R4.1 `:163` and R4.2 `:164` gate on "once [R1.3] lands"; DE E1's ‹P1› sentence and ‹P1› action match.

### (c) Cross-PRD cites name the owning document — **CLEAN**

Standing grep, over all ten files:

```
grep -nE '\[(R|E|M|F|OQ ?)[0-9][^]]*\]\(\.\./' data-foundation/prd-data-foundation*.md export/prd-data-export*.md
→ no output (exit 1)
```

All **279** cross-directory links were enumerated and their labels tabulated. Every one is (i) an explicit owner label — `the capture PRD's R4.24`, `the device PRD's R1.21`, `the import PRD's R2.3`, `the export PRD's R2.3`, `the Data Foundation PRD's R7.7`, `vision J4`; (ii) an `its …` / `that PRD's …` / `Its …` anaphor inside an obligations-table cell whose Source/Target column names the document (DF `:237`–`:240`, `:248`–`:259`; DE `:176`–`:179`, `:185`–`:188`); or (iii) an `its …` anaphor whose antecedent is an explicitly labelled cite in the same sentence (DF `:152`, `:242`, `:257`; DE `:124`, `:126`, `:139`, `:163`, `:165`, `:185`, `:209`; DF fence map `:181`–`:207`). **No bare-ID label exists.** Non-link prose cites (DF `:242`'s 21-row map, DE `:185`'s "its R6.5") sit inside cells whose Source/Target column names the owner.

Cross-PRD fence cites are likewise all qualified: `the export PRD's F1`, `its F2`–`its F10` (DF fence map, each preceded by "moved to the export PRD's …"), `its F31` (DE `:17`, `:176`), `its fence F27` (DE `:126`), `that document's fence F30` (DE `:73`), `that PRD's fence F24` (DF `:242`). Unqualified `fence Fn` is always the citing document's own.

### (d) Retired and moved IDs

Verified clean: DF's Legend retired list (`:89`) and DE's Traceability table (`:77`–`:100`) reconcile **one-for-one**, all 24 pairs, including the three split rows (R7.6, R7.7, R7.8) reconciled in prose on both sides (DF `:89`, DE `:102`). No retired ID is reused as a live ID: DF has no §4 rows (sections run 1, 2, 3, 5, 6, 7), no R7.9, no R7.6a (surfaces are R7.6b–R7.6n), no M3 (M1, M2, M4–M9), no E6/E7/E17/E18 (the copy file holds exactly E1–E5, E8–E16, E19–E30), no DJ1 (DJ2–DJ5), and OQs 1–7, 10, 12–15 with no 8/9/11/16. *(was 1d-2's sharpest case — F5's colliding R7.6 — fixed by LC-4.)*

| # | Location | Expected | Found | Class |
| :--- | :--- | :--- | :--- | :--- |
| 1d-1 | `data-foundation/prd-data-foundation-fences.md:35`, `:93`, `:101`, `:113`, `:119`, `:133`, `:149`, `:203` | A retired ID in preserved fence prose is qualified, as LC-4 did for the five named at `69a69df` | LC-4 qualified R4.4 (F5 `:37`), R4.8 (F22 `:109`), R4.9 and R4.7 (F24 `:119`, `:121`) and R4.4 (F27 `:139`). The same class survives untouched elsewhere: **M3** unqualified at F18 `:93`, F26 `:133`, F29 `:149` (now the export PRD's M1); **OQ 9** at F5 `:35`; **OQ 8** at F20 `:101` and F23 `:113` (now its OQ 1); **OQ 16** at F24 `:119` (now its OQ 4); **R4.4** at the F27 map entry `:203`. None collides with a live DF ID, so the reading is unambiguous — unlike the R7.6 case LC-4 fixed | **EDITORIAL** (low) |

### (e) Fence numbering and fence → row maps

Numbering is per PRD (DF F1–F32, DE F1–F13) with no overlap in meaning, and every cross-PRD fence cite is qualified (above). Map completeness: **DF 32/32** map rows (`prd-data-foundation-fences.md:177`–`:208`), **DE 13/13** (`prd-data-export-fences.md:95`–`:107`).

Forward direction (a row citing a fence appears in that fence's map entry) is clean for every DF requirement row — all 24 checked at `69a69df` plus R1.3→F22 (LC-5 landed "fence F22" beside E22 at DF `:137`) — and for every DE requirement row except R2.4. Reverse direction (a map entry naming a row that cites no fence) occurs ~18× in DF and ~8× in DE and is **not** a defect: both maps define "carries" as "the fence's decision is what the row now states" (`prd-data-foundation-fences.md:173`, `prd-data-export-fences.md:91`).

| # | Location | Expected | Found | Class |
| :--- | :--- | :--- | :--- | :--- |
| 1e-1 | `export/prd-data-export.md:140` vs `export/prd-data-export-fences.md:98` | R2.4 cites "fences F4 and F6", so F4's map entry lists R2.4 | F4's entry reads `R2.1; OQ 1.` — **R2.4 absent**, although DE's own OQ 1 Feeds cell (`:206`) and OQ 1's results section (`prd-data-export-oq-results.md:11`) both say the answer is "Carried by R2.1 and R2.4". Exactly the shape of the 1e-3 miss LC-5 fixed on F2 | **EDITORIAL** |
| 1e-2 | `data-foundation/prd-data-foundation-fences.md:189` (F13) vs `prd-data-foundation.md:140` (R1.5), `:222` (R7.3) | F13's decision includes "A hold left by a process that is gone never blocks the owner from opening their file" (`:73`), so the rows carrying it appear in its map entry | F13's entry reads `R1.7; R1.4's second clause; R7.4's volume-class scoping and M1's population.` — **R1.5 and R7.3 absent**, though FX6-3 put that decision into both ("a hold left by a dead process never blocks the owner", R1.5; "a hold left by a process that is gone, which opens the file rather than a state", R7.3) | **EDITORIAL** |
| 1e-3 | DF `:248`, `:253`, `:256`, `:259`; DE `:17` | An obligations-table line or scope statement citing a fence appears in that fence's map entry, as F3's, F8's, F12's, F14's, F15's, F19's, F30's and F32's entries already do for their lines | Five cites have no counterpart: DF `:253` cites "fences F2 and F14" — F14's entry names the Capture Mode OQ 19 line, **F2's (`:178`) does not**; DF `:256` cites fence F14 — its entry (`:190`) does not name the Device Management line; DF `:259` cites fence F10 — its entry (`:186`) does not name the Vision line; DF `:248` cites F25, F27, F28, F31 — none of those entries (`:201`, `:203`, `:204`, `:207`) names the outbound Data Export line (F30's, `:206`, does: "the obligations tables both ways"); DE `:17` cites fence F9 — its entry (`:103`) does not name the Background scope statement (F1's, `:95`, does) | **EDITORIAL** (low; arguably exempt under the map's "carries" definition) |

---

## CHECK 2 — Index sync

### (i) Fence → row maps vs body cites
Covered in 1(e): three misses (1e-1, 1e-2, 1e-3). Both map tables are complete (32/32, 13/13) and every named row genuinely carries its fence.

### (ii) Surfaces tables

DF's 13 surfaces (`:106`–`:118`) yield a Copy-ID union of E1–E5, E8–E16, E19–E30 = **exactly the 26 states** in `prd-data-foundation-copy.md`, and every Req-ID named (R1.3, R1.5, R1.7–R1.10, R2.4, R2.6, R2.8, R2.9, R3.2, R3.5, R5.1–R5.8, R6.2, R6.3, R6.6) exists. DE's one surface (`:110`) is clean: Req-IDs R1.1–R1.3, R2.1–R2.5, R3.1 and Copy IDs E1–E4 all exist, no duplication.

| # | Location | Expected | Found | Class |
| :--- | :--- | :--- | :--- | :--- |
| 2ii-1 | `data-foundation/prd-data-foundation.md:263` (§8) vs `:106` (R7.6b), `:117` (R7.6m) | Every state listed on two surfaces is named in §8's explanation | §8 reads "E4, E10 and E11 are each listed on two surfaces because each of those states opens from both" — the LC-6 sentence. **E28, E29 and E30 are now also on two surfaces each** (R7.6b and R7.6m, added by FX6-2 and FX7-7) and are not named. Secondary: E10 sits on R7.6d whose Req-IDs (R1.3, R1.9) do not include its producing row R1.5 — the only such case | **EDITORIAL** |
| 2ii-2 | `data-foundation/prd-data-foundation.md:108` (R7.6d) | *(the item specifically asked about)* — E25 and E27 are linked from R7.6d's "What the test lists" cell but absent from its Copy IDs cell | **A mere cite, not a miss under the index rule.** The cell reads "…a local disk, a syncing folder or a network volume, in [E25]'s and [E27]'s words" — it borrows those states' *wording* for a value the location surface displays; it does not assert that R7.6d presents them. Consistent with the rule: the Copy IDs column "nam[es] states, never strings" (`:102`), E25/E27's producing row is R1.7 (not among R7.6d's Req-IDs R1.3, R1.9), and R1.7's own surface R7.6e (`:109`) carries them. It is, however, the **only** place in either Surfaces table where a state is linked from a "What the test lists" cell without appearing in Copy IDs, so the convention is nowhere stated | **EDITORIAL** (nit — one clause in §8, or drop the two links and say "in the volume-class states' words") |

### (iii) Copy states ↔ producing rows — **CLEAN, both documents**
DF: all 26 states are cited by at least one requirement row (E1←R5.3; E2←R5.1; E3←R5.2; E4←R2.9,R5.5; E5←R2.2,R5.5,R7.3; E8←R6.2; E9,E10←R1.5; E11←R2.4,R2.8,R2.9; E12←R5.1,R5.8; E13←R1.8; E14←R6.2; E15←R1.10; E16←R5.7; E19–E21←R1.9,R7.8; E22←R1.3; E23←R5.2; E24←R5.8; E25,E27←R1.7; E26←R2.8; E28–E30←R5.5,R5.8,R7.3). E20 and E29 are cited only inside the ranges `[E19]–[E21]` and `[E28]–[E30]`, which is the documents' established form. Every E cited by a row exists; the one out-of-document cite, `the capture PRD's E26` at DF `:222`, is qualified.
DE: E1←R1.3, R2.4; E2/E3/E4←R3.1. All exist, all cited.

### (iv) Journey indexes ↔ companions — **CLEAN, both documents**
*(was 2iv-1, 2iv-2, 2iv-3 — all resolved by LC-7.)* DF's index (`:57`–`:60`) reads DJ2–DJ5, matching the companion headings and anchors, with the DJ prefix stated at `:53`; rows-exercised agree exactly for all four (DJ2 R2.2–R2.5+R2.8; DJ3 R2.9+R5.1–R5.8; DJ4 R6.1–R6.4; DJ5 R1.1–R1.3+R1.6+R3.2+R5.6) and every cited row exists. DE's journey is EJ1 throughout (index `:43`, Traceability `:100`, companion heading, anchor `#ej1-export-the-collection`), and its index row now carries "and the Data Foundation PRD's R3.4, R3.5", matching the companion. The vision's J1–J7 collision is gone: the only `J1`–`J7` tokens left in either set are `vision J4` (DE `:11`, `:118`, journeys `:10`) and `vision J7` (DE `:208`), all qualified.

### (v) Answered OQs ↔ results files — **CLEAN, both documents**
DF answered = 1, 3, 4, 7, 10; the results file holds `## OQ 1`, `3`, `4`, `7`, `10` — five sections, one-for-one, no orphans. Open = 2, 5, 6, 12, 13, 14, 15, with no sections. DE answered = 1, 2; results holds `## OQ 1`, `## OQ 2`. Open = 3, 4.

### (vi) Copy header markers — **CLEAN, both documents**
DF's header (`prd-data-foundation-copy.md:4`, `:6`) defines four markers and all four are used, with no fifth: ‹floor› ×2 (definition + E16's state name), ‹OQ 14› ×2 (definition + E9's body), ‹P1› ×6 (two in the header, two each in E8 and E14), ‹until P1› ×7 (one in the header, three each in E8 and E14). Every use leads a whole sentence or trails a whole action, as the header requires. The header now names the cross-PRD seam explicitly ("E8's and E14's full-history sentence waits on the export PRD's R1.3"), closing *was 3-7*, and names what each ‹until P1› sentence stands in for. DE's header (`prd-data-export-copy.md:4`) defines ‹P1› only and uses it ×3 (definition, E1's body sentence, E1's second action) — no undefined marker.

### (vii) Provisional constants and Feeds cells

An UPPER_CASE sweep of all ten files returns **twelve** tokens and no thirteenth: SQLITE_READER_FLOOR (16), ROWS_CEILING (12), DERIVATION_VERSION (6), WAVELENGTH_GRID (5), HISTORY_RETENTION (5), GAMUT_RENDERING_INTENT (5), GAMUT_REFERENCE_SPACE (5), DELETE_UNDO_WINDOW (5), STORE_SIZE_BUDGET (4), INTEGRITY_CHECK_BUDGET (3), DERIVATION_TOLERANCE (3), COMPATIBILITY_FLOOR (3). Every one carries an OQ id or a fence at its defining use site — SQLITE_READER_FLOOR → OQ 2 (DF `:135`); STORE_SIZE_BUDGET → OQ 5 (`:159`); DERIVATION_TOLERANCE → OQ 6 (`:224`); INTEGRITY_CHECK_BUDGET → OQ 13 (`:192`); WAVELENGTH_GRID → OQ 4 (DE `:139`); GAMUT_* → fence F4 (`:174`); HISTORY_RETENTION → fence F6 (`:159`); DELETE_UNDO_WINDOW → fence F7 (`:206`); DERIVATION_VERSION declared "a stamp, not a constant" (DF Legend `:87`, DE Legend `:62`); ROWS_CEILING the capture PRD's OQ 13 (DF `:87`, DE `:62`). **COMPATIBILITY_FLOOR** is new to the list only because this sweep includes the fence files; it appears solely in fence prose (`prd-data-foundation-fences.md:109`, `:198`; `prd-data-export-fences.md:39`) and is declared *not to exist* under fence F22 until a release raises the floor (DF R5.7 `:187`) — not a latent decision.

All seven "per SDK docs" markers now carry an OQ id, closing *was 2vi-4*: DF `:122` (OQ 12, OQ 15), `:152` (OQ 15), `:171` (OQ 15), `:292` and `:295` (inside their own OQ rows); DE `:114` ("the Data Foundation PRD's OQ 15") and `:209` (inside OQ 4's row). DF R5.4's ambiguity (*was 2vi-3*) is fixed at `:192`: "ROWS_CEILING ([the capture PRD's OQ 13]) — this document's OQ 13".

| # | Location | Expected | Found | Class |
| :--- | :--- | :--- | :--- | :--- |
| 2vii-1 | `data-foundation/prd-data-foundation.md:285` (OQ 2's Feeds) | Names every row using SQLITE_READER_FLOOR | Feeds names R1.2, R5.1, R5.6, R6.5, R7.1, R7.2, M4, M9 — LC-8's five additions landed. It still **omits R5.2 (`:190`) and R7.3 (`:222`)**, both of which gained "reads back identical at SQLITE_READER_FLOOR" in FX6-23 after the Feeds cell was last touched | **EDITORIAL** |
| 2vii-2 | `export/prd-data-export.md:209` (OQ 4's Feeds) | Names every row the question feeds | Feeds names R2.3 and R2.5 — LC-8's fix landed (R2.5 added, R4.2 dropped). It **omits R4.1 (`:163`)**, whose golden is gated on OQ 4 by FX6-12 and FX7-16, and which **OQ 4's own Interim rule cell names** ("the golden leaves the wavelength block out … ([R4.1])") | **EDITORIAL** |

### (viii) Word counts

Method as specified — strip HTML comments, link targets `](…)`, code fences and table pipes, then count whitespace-separated tokens:

| Document | Count | Budget | Headroom |
| :--- | :--- | :--- | :--- |
| `data-foundation/prd-data-foundation.md` | **7,922** | 8,000 (fence F21, raised 2026-09-14) | 78 |
| `export/prd-data-export.md` | **3,942** | 4,000 (DE fence F1) | 58 |

Both under. Both figures reproduce FX7-25's reported counts exactly. Deleting the six guidance comments at lock will not change either number (the method strips them already).

### (ix) README sync — **CLEAN**
`README.md:16` "| 4 | Data Foundation | U5, U6 | draft |" ↔ `prd-data-foundation.md:3` "Status: draft". `README.md:17` "| 5 | Data Export | U6 | draft — split out of Data Foundation on 2026-09-09 under its fence F30 |" ↔ `prd-data-export.md:3` "Status: draft". `:49` and `:55` describe the two five-file shapes; all eight companions exist. `:88` routes U6 to both; `:104` routes the CSV P0 to Data Export; `:117` records the split. No stale or missing row. (The one substantive README item is 1a-3 above, which is about the vision's phrasing, not the index.)

---

## CHECK 3 — Latent-decision inventory

| # | Item | Location | Found | Class |
| :--- | :--- | :--- | :--- | :--- |
| — | Constants with no OQ or fence | — | **None.** See 2(vii) | — |
| — | Copy states with no producing row | — | **None**, either document. See 2(iii) | — |
| — | Rows an upstream PRD places at a higher priority | — | **None.** Capture R8.9 P1 / DF R2.3 P0 and capture R6.9 P1 / no DF row are both now stated in DF (`:242`, `:249`); every other handed row is P0 on both sides. DF's only P1 (R6.3) and DE's only P1 (R1.3) have no upstream counterpart | — |
| 3-1 | "volume class" / "that class" | DF `:109` (R7.6e), `:108` (R7.6d), `:139` (R1.7), `:221` (R7.2), `:223` (R7.4), `:271` (M1) | One phrase, **two taxonomies**, defined in neither Vocabulary. R7.4 and M1 use it for the durability classes — "local volumes … USB external and network volumes … each run recording its class". R1.7, R7.2, R7.6d and R7.6e use it for the warning classes — "local disk, a syncing folder or a network volume" / "local, sync-managed, or a network volume". A sync-managed folder on an internal disk is *local* under the first and *sync-managed* under the second. R7.6e's "the right one appeared for the volume class" is the sharpest: it reads on the durability set and means the warning set | **EDITORIAL** (a Vocabulary entry, or distinct words — "volume class" vs "location class"); becomes a **DECISION** only if the owner intends them as one set |
| — | Other terms round 6/7 introduced | "salvage output", "fixture", "chosen condition", "golden", "state column" | All now carry Vocabulary entries — DF `:76`–`:79`, DE `:53`–`:54` — closing *was 3-2 through 3-6*. "measuring build" / "release build" (DF `:208`) is anchored to `the capture PRD's R11.16`; "errata" (DE `:163`), "build configuration" (DF `:208`) and "passthrough column" (DE `:139`) are plain-English or defined in-row. Informational | — |
| — | `{{placeholder}}` | all ten files | **0** | — |
| — | `TBD` / `TODO` / `XXX` | all ten files | **0** | — |
| — | "Owner:" / author line | all ten files | **0** (both fence files declare the shape as "no author line", DF F1 `:11`, DE F1 `:11`) | — |
| — | Status cell not one of the Legend's six | both PRDs + both copy files | **None.** The other four glyphs appear only inside the two Legend blocks (`prd-data-foundation.md:93`–`:94`, `prd-data-export.md:66`–`:67`) | — |
| — | Rows not 🤝 Aligned | 6 rows | Exactly the expected set, no more: **DE R4.1** (`prd-data-export.md:163`); **DF R6.2** (`prd-data-foundation.md:205`); **DF E8** (`prd-data-foundation-copy.md:23`), **E28** (`:37`), **E29** (`:38`), **E30** (`:39`). Totals: DF 88 🤝 / 4 ⌛️ (45 R rows + 13 surfaces + 8 metrics + 26 states), DE 18 🤝 / 1 ⌛️ (13 R rows + 1 surface + 1 metric + 4 states) | — |
| — | Template guidance comments still in the PRD bodies — **delete at lock, listed here, not deleted** | 6 total | `prd-data-foundation.md:9` (Background), `:51` (User Journeys), `:83` (Legend); `prd-data-export.md:9` (Background), `:37` (User Journeys), `:58` (Legend). All eight companion files carry **0** | — |

---

## Link and anchor resolution

| Set | Links checked | Broken |
| :--- | :--- | :--- |
| The ten files (every relative path + every `#anchor` against the target file's real headings) | **846** | **0** |
| All 36 docs — the ten, the three siblings and their companions, `vision.md`, `README.md`, `AGENTS.md`, plus the seven fix logs | **2,176** | **1**, and it is out of scope |

The one miss is `docs/product/data-foundation/prd-data-foundation-round-7-fixes.md:9` → `#6-deletion-and-privacy`, a same-file anchor inside a *quoted* clause the fix log reproduces verbatim from DF R7.2. It is a historical record, not one of the ten files, and the clause resolves correctly where it actually lives (`prd-data-foundation.md:221`). **No genuine miss in the ten files.**

**Slugger artefacts (not misses).** A first pass flagged three anchors — `prd-data-export-fences.md:43` → `#f4--the-gamut-clipped-export-column-is-sc_srgb_gamut_clipped-2026-09-09` and both links at `prd-data-export-oq-results.md:11`. All three are correct GitHub slugs; the false positives came from a slugger that stripped `_` along with `*` as markdown emphasis. GitHub keeps underscores. With that corrected the set resolves. The em-dash rule behaves as described ("F8 — The CSV" → `f8--the-csv`), and the `sc_`-bearing fence headings are the only place it matters.

---

## Counts

| Class | Count | Items |
| :--- | :--- | :--- |
| **EDITORIAL** | **9** | 1d-1, 1e-1, 1e-2, 1e-3, 2ii-1, 2ii-2, 2vii-1, 2vii-2, 3-1 |
| **POST-LOCK** | **3** | 1a-1, 1a-2, 1a-3 |
| **DECISION** | **0** | — |
| Clean checks | — | 1(b) priorities, 1(c) cites, 1(d) retired-ID ledger, 2(iii) copy↔rows, 2(iv) journeys, 2(v) OQ results, 2(vi) copy markers, 2(viii) word counts, 2(ix) README, link/anchor resolution (846/0), placeholders (0), statuses (0 stray, 6 non-Aligned as expected) |

## Verdict

**Lock-ready on the mechanical checks, with nine shallow editorial repairs outstanding and no decision left open.** All five DECISION items from the pre-round-6 sweep are closed: the 21-row capture hand-over map is complete and exact (21/21, with R6.9's non-coverage stated rather than papered over), "fixture", "chosen condition", "golden", "salvage output" and "state column" all have Vocabulary entries, and the journey-number collision with the vision is gone behind the `DJ`/`EJ` prefixes. Both hard preconditions pass outright again: check 1(c) is clean on the standing grep across all 279 cross-directory links, and the retired/moved ID ledger reconciles one-for-one in both directions with no reuse. Every link and anchor in the ten files resolves. Both word counts are under budget — DF 7,922 of 8,000, DE 3,942 of 4,000 — though the headroom (78 and 58 words) will not absorb the nine repairs plus a Vocabulary entry for 3-1 without a compaction pass or another F21 amendment; the repairs themselves are index cells and map entries, so most cost a handful of words.

The nine editorial items cluster into three shapes. **Four are index entries that rounds 6 and 7 outgrew**: F13's map entry predates FX6-3's dead-process-hold rows, F4's (DE) predates nothing but was simply never extended to R2.4 the way LC-5 extended F2 to R4.4, OQ 2's Feeds predates FX6-23's two new SQLITE_READER_FLOOR uses, and OQ 4's Feeds omits the one row its own Interim rule names. **Two are explanatory sentences left behind by new content**: §8 names the three two-surface states from LC-6 but not E28–E30, which FX6-2 and FX7-7 put on two surfaces each; and R7.6d's E25/E27 links — the item specifically asked about — are a wording cite rather than an index miss, but they are the only such cite in either Surfaces table and nothing says the convention exists. **Three are low-grade residue**: retired M3/OQ 8/OQ 9/OQ 16 still reading as live in preserved fence prose LC-4 did not reach (no collision, unlike the R7.6 case it did fix), obligations-table lines whose fence map entries do not name them, and the two-sensed "volume class".

The three POST-LOCK items are all of one kind and all mitigated: DE's inbound tables now cite capture R4.24 and device R1.21 directly (LC-2's fix), but only the R1.9 and R6.5 re-targets have a recorded counterpart — the other two obligations reach DE legitimately through DF's outbound Data Export line, so nothing is untraceable; the amendment lines at DE `:186` and `:187` simply need extending. The third, the README's two "raw payload canonical" rows, is the same phrasing DF `:259` already records against the vision. None of the three is a defect in the documents about to lock.
