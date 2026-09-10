# Lock preconditions — mechanical sweep before round 6

**Scope.** Data Foundation and Data Export, five files each, in
`/Users/vinnypasceri/Projects/.worktrees/spectro-capture__docs-data-foundation` (branch `docs/data-foundation`, HEAD `69a69df`).
Read-only; nothing edited. Standard applied: `~/Documents/Obsidian/foundry/standards/cross-prd-consistency-check.md` (read, present).
Line numbers are as of HEAD `69a69df`. Paths below are relative to `docs/product/`.

Classification key — **EDITORIAL**: a link target, label, index or map entry fixable without changing a rule. **POST-LOCK**: a locked sibling must change. **DECISION**: needs an Open Question or an owner call.

---

## CHECK 1 — Cross-PRD consistency

### Pairs enumerated

X ∈ {Data Foundation (DF), Data Export (DX)}. Y = any PRD X cites or that cites X.

| Pair | Direction of citation | Basis |
| :--- | :--- | :--- |
| DF ↔ Capture Mode | both | capture `prd-capture-mode.md:421` targets Data Foundation; DF `:233` inbound line |
| DF ↔ Device Management | both | device `:300`, `:301` target Data Foundation; DF `:234` inbound, `:248` outbound |
| DF ↔ Inventory Import | both | import `:169` targets Data Foundation; DF `:236` inbound |
| DF ↔ Data Export | both | DF `:235` inbound / `:242` outbound; DX `:175` inbound / `:184` outbound |
| DX ↔ Capture Mode | DX→capture only | DX `:176` inbound, cites capture R1.9, R4.24, R1.10, OQ 13 |
| DX ↔ Device Management | both | device `:301`; DX `:177` inbound, `:185` outbound |
| DX ↔ Inventory Import | DX→import only | DX `:178` inbound, `:186` outbound |

DF also names Collection Mode, QC & Comparison and Telemetry as targets (`:243`–`:245`). Those PRDs do not exist (README `:18`–`:21`, "queued"), so no pair is checkable and none is reported.

A search of all three locked siblings for links into `data-foundation/` or `export/` returns **nothing** — the siblings were written before either document existed. Every DF/DX row-ID family reference in a sibling is by prose name only (`Data Foundation`, `Data Foundation, export`).

### (a) Inherited obligations agree both ways

| # | Location | Expected | Found | Class |
| :--- | :--- | :--- | :--- | :--- |
| 1a-1 | `data-foundation/prd-data-foundation.md:233` vs `capture-mode/prd-capture-mode.md:421` | DF's inbound Capture line names capture's R4.5 and R1.10 as imposing here; capture's Data Foundation obligation row should list them | Capture's row lists 21 rows and **neither R4.5 nor R1.10**. DF's fence map (`prd-data-foundation-fences.md:198`, F28) records the dependency ("the Capture Mode line … its R1.10, R4.5") but no post-lock amendment line exists in DF's outbound table for it | **POST-LOCK** |
| 1a-2 | `data-foundation/prd-data-foundation.md:233` | Every row capture hands DF is carried by a named DF row, or the line says it is handed over whole | The line says "Both Data Foundation lines … carried whole" **and** names 7 DF rows — both mechanisms at once. Capture hands over 21 rows; DF's body cites only 7 of them (R1.9, R4.12, R4.24, R8.9, R8.13, R11.6, R11.10). **14 are named by no DF row and by nothing in the Rows-here cell**: R3.1, R3.6, R3.8, R4.6, R4.11, R4.13, R6.7, R6.9, R7.12, R7.18, R8.5, R8.17, R11.11, R11.16 (session record, remembered row, queue order, spread, code ordering, activity record, read-back). Several are covered *in substance* (R4.6 by DF R3.2, R11.11 by DF R7.1/R7.2) but none is traceable | **DECISION** |
| 1a-3 | `capture-mode/prd-capture-mode.md:420`, `import/prd-inventory-import.md:168` | A two-way obligations table in each sibling | Capture and Import carry a **one-way** table only ("Target PRD \| Obligation \| Rows"); device-management carries both directions. DF's two outbound Capture lines (`:246`, `:247`) and DX's outbound Device/Import lines (`:185`, `:186`) each say "post-lock there", so the gap is correctly *recorded* — it is an amendment obligation, not a defect | **POST-LOCK** (already recorded) |
| 1a-4 | `export/prd-data-export.md:176` vs `capture-mode/prd-capture-mode.md:421` | Capture's R1.9 queue-order obligation, now consumed by DX R2.3, has a counterpart naming Data Export | Capture's row names **Data Foundation** as the sole target. DX's outbound table (`:184`–`:186`) records post-lock amendments for Device Management and Inventory Import but **not** for capture's re-target | **POST-LOCK** |
| 1a-5 | `export/prd-data-export.md:124` (R1.2) | A Capture Mode inbound line for capture's R4.24 (the basis and the non-spectral mark), which R1.2 cites directly | DX's inbound Capture Mode line (`:176`) covers queue order only. The basis reaches DX legitimately via DF's outbound line (`:242`, "R2.1's … basis"), but R1.2's direct cite of `the capture PRD's R4.24` has no obligations-table line | **EDITORIAL** |
| 1a-6 | `export/prd-data-export.md:123` (R1.1) | A Device Management inbound line for the device PRD's R1.21 snapshot, which R1.1 cites directly | DX's inbound Device Management line (`:177`) covers `sc_simulated` / R6.5 only | **EDITORIAL** |
| 1a-7 | `data-foundation/prd-data-foundation.md:131` (R1.2) | An outbound Inventory Import line for "a later import into the collection keeps the existing columns' positions and appends its new ones after" — a rule on import behaviour | DF's outbound table (`:242`–`:248`) has **no Inventory Import line at all**. DX records the analogous post-lock amendment on import R2.2 (`:186`); DF does not | **EDITORIAL** (add the line in DF) + **POST-LOCK** (import records it) |
| — | `device-management/prd-device-management.md:301` | The `Data Foundation, export` line, addressed to DF, is re-routed to DX and both sides say so | Satisfied: DF `:248` ("the `simulated` column amendment is the export PRD's"), DX `:177` inbound and `:185` outbound post-lock line. **No action** | — |
| — | DF `:235` ↔ DX `:184`; DF `:242` ↔ DX `:175` | One-for-one both ways | Satisfied. DF↔DX inbound/outbound pair up exactly: DX R4.1/R4.2/R4.3 ↔ DF R7.7/R6.5/R7.2, and DF R1.2/R2.1/R2.2/R2.4/R2.9/R3.1/R6.5 ↔ DX R1.1/R1.2/R1.3/R2.1/R2.3/R4.2/R4.3. The 13-item fixture list in DF R7.7 (`:222`) and in DX `:184` matches item-for-item (ordering of "a confirmed correction in history" differs; content identical). DF↔Device (`:234` ↔ device `:300`) and DF↔Import (`:236` ↔ import `:169`) also match. **No action** | — |

### (b) Shared rows agree on priority

| # | Location | Expected | Found | Class |
| :--- | :--- | :--- | :--- | :--- |
| 1b-1 | `capture-mode/prd-capture-mode.md:318` (R8.9, **P1**) vs `data-foundation/prd-data-foundation.md:151` (R2.3, **P0**) | The same version-history rule at the same phase, or a stated conditional on both sides | Divergent Pri on a shared rule, with **no note in DF**. It is deliberate and reconciled — `capture-mode/prd-capture-mode-fences.md:156`–`158` (F24) puts "the version-history record shape (R8.13)" at P0 and "the correction path (re-scan rows in §8)" at P1, its Why naming Data Foundation explicitly — but that reconciliation exists only in the locked sibling's fence file. This is the exact pattern the standard's Origin section names | **EDITORIAL** (record the reconciliation in DF) |
| 1b-2 | `capture-mode/prd-capture-mode.md:241` (R6.9, **P1**) | DF carries the inherited code-ordering rule with a Pri to compare | DF has no row and no Pri for "comparing codes this way wherever they are ordered". Subset of 1a-2 | **DECISION** (folded into 1a-2) |
| — | Device R6.5 **P0** ↔ DX R1.2 **P0**; DX R1.3 **P1** ↔ DF R6.2's "once that PRD's R1.3 lands" (`:201`) and DF copy E8/E14 ‹P1›; DF R6.3 **P1** ↔ DF Legend (`:81`) + F16 (`prd-data-foundation-fences.md:186`); DX R1.3 **P1** ↔ DX Legend (`:59`) + F2 (`prd-data-export-fences.md:84`) | agree | All agree. DF Pri cells: R6.3 alone is P1, all 40 others P0 — matches the Legend. DX Pri cells: R1.3 alone is P1, all 9 others P0 — matches the Legend. **No action** | — |

### (c) Cross-PRD cites name the owning document — **CLEAN**

Standing grep, over all ten files:

```
grep -nE '\[(R|E|M|F|OQ ?)[0-9][^]]*\]\(\.\./' data-foundation/*.md export/*.md
→ no output (exit 1)
```

Every one of the 130 cross-directory links whose label carries a row / state / metric / OQ / fence ID was enumerated and its surrounding text inspected. Each is one of:

- an explicit label — `the capture PRD's R4.12`, `the device PRD's R1.21`, `the import PRD's R2.3`, `the export PRD's R2.1`, `the Data Foundation PRD's R7.7`, `Data Foundation PRD's E8 and E14` (91 links);
- an `its …` / `that PRD's …` anaphor inside an obligations-table cell whose Source/Target column names the document (DF `:233`–`:248`, DX `:175`–`:186`) — 27 links;
- an `its …` anaphor whose antecedent is an explicitly labelled cite in the same sentence — DF R2.1 `:148` ("the capture PRD's R4.12 … its R4.24"), DF R3.3 `:169`, DF R1.4 `:134`, DF R6.2 `:201`, DX R1.1 `:123`, DX R2.3 `:138`, DX R4.2 `:163`, DX R4.3 `:164`, DX Traceability `:101`, DX Background `:17`, DF Legend retired list `:85`, DF fence map `prd-data-foundation-fences.md:175`–`:199` — 12 links.

No bare `[R…](../…)`, `[E…](../…)`, `[M…](../…)`, `[OQ n](../…)` or `[Fn](../…)` exists in either document set. **No finding.**

Separately, a full link-target resolution pass over all ten files (every relative path + every `#anchor` against the target file's real headings) returns **0 broken targets**.

### (d) Retired and moved IDs

| # | Location | Expected | Found | Class |
| :--- | :--- | :--- | :--- | :--- |
| — | DF Legend `:85` ↔ DX Traceability `:76`–`:99` | One-for-one | **Match.** All 22 retired IDs pair exactly: R4.1→R1.1, R4.3→R1.2, R4.4→R1.3, R4.2→R2.1, R4.6→R2.2, R4.7→R2.3, R4.8→R2.4, R4.9→R2.5, R4.5→R3.1, R7.9→R4.1, R7.6a→R4.4a, M3→M1, OQ 8/9/11/16→OQ 1–4, E6/E7/E17/E18→E1–E4, DJ1(J1)→J1. The three **split** rows (R7.6, R7.7, R7.8) are reconciled in prose on both sides — DF `:85` ("stay, each having handed one half over"), DX `:101` ("split rather than moved whole") — and DX's table adds the three destination IDs (R4.2, R4.3, R4.4) that DF's arrow list does not name, which is consistent, not contradictory. **No action** | — |
| — | Retired IDs not used as live IDs | Verified. DF has no §4 (sections run 1, 2, 3, 5, 6, 7); DF copy holds exactly 22 states with no E6/E7/E17/E18; DF's OQ table holds 1–7, 10, 12–15 with no 8/9/11/16; DF journeys hold DJ2–DJ5 with no DJ1. DX's own R4.x / M1 / E1–E4 / OQ 1–4 / J1 are its own families, which is correct. No sibling uses a retired DF ID. **No action** | — |
| 1d-2 | `data-foundation/prd-data-foundation-fences.md:37`, `:109`, `:119`, `:121`, `:139` | A retired ID appearing in preserved fence text is marked as retired, or qualified | Retired IDs appear **unqualified, as if live**, in fence Decision/Why prose that F30 preserves as history: F5's Clarification `:37` "R7.6, the test row that asserts the export's column set, is P0" — this is now DX R4.4, while DF's **live R7.6** is a different rule (its own surfaces), a direct collision; F22 `:109` "(R4.8)"; F24 `:119` "(R4.9)" and `:121` "R4.7's 'column order is fixed'"; F27 `:139` "R4.4's history export". The fence→row **map** entries handle all of these correctly ("No row here any more: moved to the export PRD's …"); only the Decision/Why prose does not. DX's own F2 Clarification (`prd-data-export-fences.md:21`) shows the correct form: "R4.4 — that document's R7.6" | **EDITORIAL** |

### (e) Fence numbering and fence→row maps

| # | Location | Expected | Found | Class |
| :--- | :--- | :--- | :--- | :--- |
| — | Cross-PRD fence cites qualified | Verified: `the export PRD's F1`, `the Data Foundation PRD's F5/F18/F20/F22/F23/F24/F26/F28/F29/F30`, `its F31`, `its fence F27`, and the ten `its F2`–`its F10` in DF's fence map, each preceded in the same sentence by "the export PRD's". **No action** | — |
| — | DF: every row citing a fence appears in that fence's map entry | **Clean, all 24.** R1.8→F14; R1.2→F11,F25; R1.6→F11,F25; R1.3→F2; R1.7→F13; R2.1→F10,F25; R2.3→F17; R2.4→F3,F12; R2.6→F6; R3.1→F27,F28,F31; R3.4→F4; R6.1→F15; R6.3→F7; R6.6→F19; R7.1→F25,F27; Legend→F16; Background→F1,F30; §8→F8. Every one is present in the named map entry. **No action** | — |
| 1e-1 | `export/prd-data-export.md:125` (R1.3) vs `export/prd-data-export-fences.md:91` (F9) | R1.3 cites fence F9, so F9's map entry lists R1.3 | F9's entry lists R1.1, R2.1, R2.3 and the Data Foundation obligation line — **R1.3 is missing**. (All other DX row→fence cites check out: R1.1→F2,F8,F9,F10,F11; R1.2→F6; R2.1→F4,F6; R2.3→F6,F7; R2.4→F5,F6; Legend→F2) | **EDITORIAL** |
| 1e-2 | `data-foundation/prd-data-foundation-fences.md:192` (F22) | The rows F22's map names cite F22 | F22's entry names R1.3 (b), R5.7 (c), R3.5 — **none of the three cites F22**. R1.3 cites F2 only, though its second-file rule comes from F22(b) | **EDITORIAL** (low) |
| 1e-3 | `export/prd-data-export-fences.md:21` vs `:84` | F2's Clarification names R4.4, so F2's map entry lists it | F2's entry lists R1.1, R1.3, the Legend split, R1.2, R2.1, R3.1 and OQ 2 — R4.4 absent | **EDITORIAL** (low) |
| — | Reverse direction (a map entry naming a row that cites no fence) | Occurs ~18× in DF (R1.1, R1.4, R1.9, R2.2, R2.5, R2.8, R3.3, R3.5, R5.7, R6.2, R6.5, R7.4, R7.5, R7.6, R7.7, R7.8, M1, M6) and ~8× in DX (R2.5, R3.1, R4.1, R4.2, E1, M1). **Not a defect**: both maps define "carries" as "the fence's decision is what the row now states" (`prd-data-foundation-fences.md:167`, `prd-data-export-fences.md:79`), which does not require a cite. Informational | — |

---

## CHECK 2 — Index sync

### (i) Fence→row map vs body cites
Covered in 1e above: three misses (1e-1, 1e-2, 1e-3), all EDITORIAL; forward direction otherwise clean in both documents.

### (ii) Surfaces table

| # | Location | Expected | Found | Class |
| :--- | :--- | :--- | :--- | :--- |
| — | DF `:100`–`:114` | Every listed Copy ID exists; every listed Req-ID exists | Verified. 13 surfaces (R7.6b–R7.6n; R7.6a correctly retired to DX). Copy-ID union = E1–E5, E8–E16, E19–E26 = **exactly the 22 states in the copy file**. Every Req-ID (R1.3, R1.5, R1.7–R1.10, R2.4, R2.6, R2.8, R2.9, R3.2, R3.5, R5.1, R5.5, R5.8, R6.2, R6.3, R6.6) exists. The two "—" cells (R7.6j, R7.6n) are explained at `:252`. **No action** | — |
| 2ii-1 | DF `:102`/`:104`, `:107`/`:108`, `:107`/`:109` | Each copy state listed on exactly one surface, or explained | Three states listed on **two** surfaces each, unexplained: **E10** on R7.6b and R7.6d; **E4** on R7.6g and R7.6h; **E11** on R7.6g and R7.6i. Each is plausibly deliberate (E10 belongs to both file-opening and file-switching), but §8 (`:252`) explains only the two "—" cells | **EDITORIAL** (confirm intent, then say so) |
| — | DX `:107`–`:109` | Same checks | Clean. One surface (R4.4a), Req-IDs R1.1–R1.3, R2.1–R2.5, R3.1 all exist, Copy IDs E1–E4 all exist, no duplication. **No action** | — |

### (iii) Copy states ↔ requirement rows — **CLEAN, both documents**
DF: all 22 states are cited by at least one row (E1←R5.3; E2←R5.1; E3←R5.2; E4←R2.9,R5.5; E5←R2.2,R5.5,R7.3; E8←R6.2; E9←R1.5; E10←R1.5; E11←R2.4,R2.8,R2.9; E12←R5.1,R5.8; E13←R1.8; E14←R6.2; E15←R1.10; E16←R5.7; E19–E21←R1.9,R7.8; E22←R1.3; E23←R5.2; E24←R5.8; E25←R1.7; E26←R2.8). Every E cited by a row exists. The one out-of-document cite, `the capture PRD's E26` at DF `:218`, is qualified.
DX: E1←R1.3,R2.4; E2/E3/E4←R3.1. All exist, all cited.

### (iv) User Journeys index ↔ companion

| # | Location | Expected | Found | Class |
| :--- | :--- | :--- | :--- | :--- |
| — | DF `:55`–`:60` ↔ `prd-data-foundation-journeys.md` | Index rows match companion sections | Names, rows-exercised and anchors match **exactly** for all four (J2/DJ2 R2.2–R2.5+R2.8; J3/DJ3 R2.9+R5.1–R5.8; J4/DJ4 R6.1–R6.4; J5/DJ5 R1.1–R1.3+R1.6+R3.2+R5.6). **No action** | — |
| 2iv-1 | DF `:55`–`:60` vs `prd-data-foundation-journeys.md:10`,`:31`,`:58`,`:80` | One ID scheme per journey | The index column says **J2–J5**; the companion headings are **DJ2–DJ5**, and the Entry column links `DJ2`…`DJ5`. Two names for one journey, reconciled nowhere except obliquely in the Legend's retired list (`:85`, "DJ1, this document's J1"). DX has no such split (index J1 ↔ heading J1) | **EDITORIAL** |
| 2iv-2 | DX `:43` vs `prd-data-export-journeys.md:10` | Rows-exercised agree | Index: "R1.1–R1.3, R2.1–R2.5, R3.1". Companion adds "and the Data Foundation PRD's R3.4 and R3.5". Index omits the two cross-PRD rows | **EDITORIAL** (low) |
| 2iv-3 | DF `:55`–`:60`, DX `:43` vs `vision.md:81`–`:138` | Journey numbers are project-global and unqualified (standard, check 3) | **Collision.** The vision owns J1–J7 (J1 First run, J2 The bulk session, J4 Data out …). DF re-uses J2–J5 and DX re-uses J1 for **different** journeys, and both documents disambiguate the vision's with a qualifier ("[vision J4]" DX `:11`, `:117`; "[vision J7]" DX `:206`; DF Background) — which the standard says a journey number should not need. Device Management sidesteps this with a `UJ` prefix; Capture and Import carry no numbered journey index | **DECISION** |

### (v) Answered OQs ↔ results file — **CLEAN, both documents**
DF: answered = 1, 3, 4, 7, 10 (five). Results file has `## OQ 1`, `3`, `4`, `7`, `10` — five sections, one-for-one, no orphans. Open = 2, 5, 6, 12, 13, 14, 15 with no sections, correct. Retired 8, 9, 11, 16 not reused; the results file (`:3`) notes only OQ 8 and OQ 9 moved, which is right — 11 and 16 were open and had no sections to move.
DX: answered = 1, 2; results file has `## OQ 1`, `## OQ 2`. Open = 3, 4 with no sections.

### (vi) Provisional constants

An ALL-CAPS sweep of both PRD bodies, copy files and journeys returns **exactly** the eleven named tokens and no twelfth: SQLITE_READER_FLOOR (13), ROWS_CEILING (12), WAVELENGTH_GRID (5), DERIVATION_VERSION (4), STORE_SIZE_BUDGET (3), INTEGRITY_CHECK_BUDGET (3), DERIVATION_TOLERANCE (3), HISTORY_RETENTION (2), GAMUT_RENDERING_INTENT (2), GAMUT_REFERENCE_SPACE (2), DELETE_UNDO_WINDOW (2). Every one carries an OQ id or a fence at its defining use site:

| Constant | Defining site | Carrier | OK |
| :--- | :--- | :--- | :--- |
| SQLITE_READER_FLOOR | DF R1.2 `:131` | OQ 2 | ✓ |
| STORE_SIZE_BUDGET | DF R2.6 `:155`, M6 `:264` | OQ 5 | ✓ |
| DERIVATION_TOLERANCE | DF R7.5 `:220`, M2 `:261` | OQ 6 | ✓ |
| INTEGRITY_CHECK_BUDGET | DF R5.4 `:188`, M8 `:266` | OQ 13 | ✓ |
| WAVELENGTH_GRID | DX R2.3 `:138` | OQ 4 | ✓ |
| GAMUT_REFERENCE_SPACE / _RENDERING_INTENT | DF R3.4 `:170` | fence F4 | ✓ |
| HISTORY_RETENTION | DF R2.6 `:155` | fence F6 | ✓ |
| DELETE_UNDO_WINDOW | DF R6.3 `:202` | fence F7 | ✓ |
| DERIVATION_VERSION | DF Legend `:83` ("a stamp, not a constant"), DX Legend `:61` | declared, not an OQ | ✓ |
| ROWS_CEILING | DF Legend `:83`, DX Legend `:61` | the capture PRD's OQ 13 (`prd-capture-mode.md:477`, confirmed to define ROWS_CEILING 10,000) | ✓ |

Residual index gaps:

| # | Location | Expected | Found | Class |
| :--- | :--- | :--- | :--- | :--- |
| 2vi-1 | DF OQ 2's Feeds cell `:274` | Names every row using SQLITE_READER_FLOOR | Feeds names R1.2, R7.1, M4. **Omits R5.1 (`:184`), R5.6 (`:182`), R6.5 (`:204`), R7.2 (`:217`), M9 (`:267`)**, all of which use the constant | **EDITORIAL** |
| 2vi-2 | DX OQ 4's Feeds cell `:207` | Names every row using WAVELENGTH_GRID | Feeds names R2.3 and R4.2. **Omits R2.5 (`:140`)**, which uses it; **names R4.2 (`:163`), which does not** | **EDITORIAL** |
| 2vi-3 | DF R5.4 `:188` | An unambiguous OQ id | "candidate ≤ 1 s at ROWS_CEILING — OQ 13, M8" — ROWS_CEILING belongs to the **capture PRD's** OQ 13 while INTEGRITY_CHECK_BUDGET is **DF's own** OQ 13, and the two sit three words apart. The Legend's "unqualified OQ n is this document's" rule resolves it, but the sentence reads both ways | **EDITORIAL** |
| 2vi-4 | DF `:118`, DX `:113` | Every "per SDK docs" marker carries its OQ id (each document's own closing rule, DF `:288`, DX `:209`) | Both **Evidence base** sections carry a bare "per SDK docs" with no OQ id: DF `:118` "(the SDK's own analytics, per SDK docs)"; DX `:113` "(the raw string round-trips a measurement … — per SDK docs)" — and DX has no OQ of its own for it (it is DF's OQ 15). The other five markers (DF R2.1, R3.1, OQ 12, OQ 15; DX OQ 4) all carry one | **EDITORIAL** |

### (vii) README sync — **CLEAN**
`README.md:16` "| 4 | Data Foundation | U5, U6 | draft |" ↔ `prd-data-foundation.md:3` "Status: draft" ✓.
`README.md:17` "| 5 | Data Export | U6 | draft — split out of Data Foundation on 2026-09-09 under its fence F30 |" ↔ `prd-data-export.md:3` "Status: draft" ✓.
`README.md:49` names DF's four companions; `README.md:55` names DX's four companions by filename — all eight exist and match the five-file shapes. `README.md:88` and `:104` route U6 to both documents; `:117` records the split. No stale row, no missing row.

---

## CHECK 3 — Latent-decision inventory

| # | Item | Location | Found | Class |
| :--- | :--- | :--- | :--- | :--- |
| — | Constants used in rows with no OQ id or fence | — | **None.** See 2(vi) | — |
| — | Copy states with no producing row | — | **None**, either document. See 2(iii) | — |
| 3-1 | Rows an upstream PRD places at a higher priority | capture R8.9 P1 / DF R2.3 P0; capture R6.9 P1 / no DF row | See 1b-1 and 1b-2 | **EDITORIAL** / **DECISION** |
| 3-2 | "golden" | DF `:235` ("their goldens") | Defined in **DX's** Vocabulary (`:53`) only. DF uses the term with no definition and no cite to DX's Vocabulary; DF's Vocabulary (`:66`–`:75`) does not list it | **EDITORIAL** |
| 3-3 | "fixture" | DF R7.7 `:222`, R7.2 `:217`, M5/M6/M8/M9; DX `:156`, `:162`–`:163`, `:184`, `:198` | Load-bearing in both documents and defined in **neither** Vocabulary. DX's Golden entry says "one per fixture", which presumes it | **DECISION** |
| 3-4 | "salvage output" | DF R5.5 `:189`, R7.3 `:218`, Surfaces R7.6b `:102` | Not in DF's Vocabulary; described only obliquely by E5's copy ("write out everything it can still read into a fresh file") | **EDITORIAL** |
| 3-5 | "chosen condition" | DF R3.1 `:167`, Surfaces `:107`, outbound `:242`; DX Vocabulary `:51`, R1.1 `:123`, R1.3 `:125`, R2.1 `:136`, F9 | Load-bearing across both documents and defined **only inside a requirement row** (DF R3.1: "the set for the collection's chosen scan mode"). DX's Vocabulary *uses* it to define Canonical export without defining it, and defers to DF's Vocabulary, which has no entry. Also note it silently equates "condition" with capture's "scan mode" | **DECISION** |
| 3-6 | "current" / the state-column token set | DX R1.1 `:123` ("never-scanned, quarantined or current", plus "superseded" in the history export) | These four literal tokens are a closed set a consumer parses, defined nowhere as such. "current" is equated to canonical only by DF's state diagram (`:25`) and R2.2 | **DECISION** |
| — | "sequence" | DF R2.1 `:148`; DX R1.3 `:125` | Defined in-row in DF; DX cites it correctly as "the version ordinal — the Data Foundation PRD's R2.1's sequence". Not a Vocabulary entry but adequately anchored. Informational | — |
| — | "basis" | DF R2.1 `:148`; DX R1.2 `:124` | Both cite `the capture PRD's R4.24`, which defines it ("curves or colour values"). Adequately anchored. Informational | — |
| — | "declared state" | DF R7.2 `:217` | DF enumerates its own declared states in-row and cites `the capture PRD's R11.6`, which establishes the mechanism. Informational | — |
| 3-7 | ‹P1› marker semantics | DF copy `:23` (E8), `:24` (E14) | The copy header (`:4`) defines ‹P1› as "does not appear until **its own rows** land", but E8/E14's ‹P1› sentence "Exporting with full history carries every earlier reading too" is gated on **DX's** R1.3, another PRD's row. The undo ‹P1› in the same cells is correctly DF's own R6.3 | **EDITORIAL** |
| — | `{{placeholder}}` | all ten files | **0** | — |
| — | `TBD` / `TODO` / `XXX` | all ten files | **0** | — |
| — | `<!-- guidance: … -->` comments | per file | `prd-data-foundation.md` **3** (Background `:9`, User Journeys `:51`, Legend `:79`); `prd-data-export.md` **3** (`:9`, `:37`, `:57`); all eight companion files **0**. Total **6**, to be deleted at lock | — |
| — | Row statuses other than 🤝 / ⌛️ | both PRDs + both copy files | **None.** 100 rows 🤝 Aligned; 8 rows ⌛️ Ready for Alignment — DF R1.2, M6, E13; DX R1.1, R1.3, R4.1, R4.3, E1. The other four glyphs appear only inside the two Legend blocks | — |

---

## Counts

| Class | Count |
| :--- | :--- |
| **EDITORIAL** | 17 — 1a-5, 1a-6, 1a-7(DF half), 1b-1, 1d-2, 1e-1, 1e-2, 1e-3, 2ii-1, 2iv-1, 2iv-2, 2vi-1, 2vi-2, 2vi-3, 2vi-4, 3-2, 3-4, 3-7 *(1a-7 counted once)* |
| **POST-LOCK** | 4 — 1a-1, 1a-3 (already recorded), 1a-4, 1a-7(import half) |
| **DECISION** | 5 — 1a-2 (with 1b-2), 2iv-3, 3-3, 3-5, 3-6 |
| Clean checks | 1(c), 1(d) arrow parity, 2(iii), 2(v), 2(vii), link/anchor resolution (0/0), placeholders (0), statuses (0 stray) |

## Verdict

**Not yet lock-ready on the mechanical checks — but the failures are shallow.** The two hardest preconditions pass outright: check 1(c) is clean on the standing grep with all 130 cross-PRD cites labelled, and the retired/moved ID ledger (1(d)) reconciles one-for-one in both directions including the three split rows, which is the check the arc has failed before. Every relative link and anchor across all ten files resolves; there are no placeholders, no stray row statuses, and no constant used without an OQ id or a fence. What remains is 17 editorial repairs — three fence→row map omissions, four index/Feeds gaps, a handful of undefined-in-Vocabulary terms, and retired IDs still reading as live inside preserved fence prose (1d-2 is the sharpest of these: DF's F5 Clarification says "R7.6" meaning what is now DX's R4.4, while DF's own live R7.6 is a different rule) — all fixable without touching a requirement.

The four POST-LOCK items are amendment obligations to *record*, not defects to fix here, and three of the four are of a kind the documents already handle correctly elsewhere; the pattern to close is that capture-mode and inventory-import carry no inbound obligations table, so every obligation DF and DX push at them has to be logged as a post-lock line, and two (capture's R4.5/R1.10 hand-over, capture's R1.9 re-target to Data Export) currently are not.

The five DECISION items are the real gate. **1a-2 is the blocker**: DF's inbound Capture Mode line claims both "carried whole" and a seven-row list, leaving 14 of the 21 rows capture hands over untraceable to any DF row — the owner needs to say whether whole-hand-over is the intended mechanism (and drop the row list) or whether DF owes rows for the session record, queue order, remembered row and activity record. The other four are vocabulary and numbering calls that should be settled before round 6 rather than discovered in it: "fixture" and "chosen condition" are load-bearing in both documents and defined in neither Vocabulary, DX's four-token state column is an unwritten closed set a consumer will parse, and the J-number collision with the vision's J1–J7 needs either a prefix (as Device Management uses `UJ`) or an explicit note that journey numbers are per-document here.
