# Data Foundation and Data Export PRDs — round 9 fixes

Resume point for round 9 (log: "Round 9"). One file for both documents: DF = Data Foundation, DE = Data Export (`../export/`). Fences DF F1–F32 and DE F1–F13 are the owner's authorization; process rules 1–8 bind every edit — two sentences per row, cite rather than restate, never expand where a clause will do; every clause below is the shortest wording that carries the rule, and the editor may shorten but not lengthen. Cross-PRD cites name the owning document in the visible label. Tick each box with a one-line **Landed:** note. Budgets after the pass: DF 8,000 (F21; 1 word of headroom before this pass — the pass is net word-negative, apply the removals FX9-12 and FX9-3 first), DE 4,000 (DE F1; 3 before). Copy, journeys, fence and README files are outside both budgets.

**Rule-6 notes.** An Aligned requirement or metric row edited here keeps 🤝 Aligned and its Commit PR cell gains "edited FX9-n, alignment kept" (appended after any existing note with "; "). A held row edited here stays ⌛️ and its Commit PR cell gains "edited FX9-n". A row FX9-17 names goes to the status it says. Copy rows keep their Status unless FX9-17 says otherwise. Surface rows have no Commit PR cell.

## Majors

- [x] **FX9-1** (T F9-T1, A9-2, IF R9-IF-1, SR8-1 residue, PL9-3, PM-F9-2; fence F13) — The passthrough value's chain of custody. DF R1.2's first sentence: "every column an import brought in under the name and in the position its import file gave it" becomes "every column an import brought in under the name, position and value its import file gave it, the value the user's to change ([R2.3](#2-canonical-value-and-version-history))". DF R7.1: "under the name and in the position its import file gave it" becomes "under the name, position and value its import file gave it". DE R2.4: "a passthrough column's value emitted exactly as its import file gave it (fence F13)" becomes "a passthrough column's value emitted exactly as stored, nothing here reshaping it (fence F13)". Rule-6 note on R1.2; R7.1 stays ⌛️ ("edited FX9-1"); R2.4 stays ⌛️ ("edited FX9-1").
  - **Landed:** DF R1.2 and R7.1 take the name/position/value wording (R1.2 gaining the [R2.3] cite), DE R2.4 reads "exactly as stored, nothing here reshaping it"; R1.2 → ⌛️ "edited FX9-1", R7.1 and DE R2.4 gain "edited FX9-1". DF 7,999, DE 3,999.
- [ ] **FX9-2** (SR9-2, IF R9-IF-2, RL9-1, T F9-T4, A9-3, SE9-2, PL9-2) — DE R4.1: "updating the golden in place with the diff, confined to the derived-value and derivation-version columns, as the review artifact" becomes "updating the golden in place with the diff, a bump's confined to the derived-value and derivation-version columns, as the review artifact". Stays ⌛️ ("edited FX9-2").
  - **Blocked: over budget by 3 words** — DE R4.1's "a bump's" is +2 and its "edited FX9-2" note +2; DE stood at 3,999 of 4,000 when it came up (measured: the body edit alone took DE to 4,001).
- [x] **FX9-3** (SR9-1, T F9-T5, SE9-4, PL9-4; IF and A9 Nits) — DF Vocabulary, the "Chosen condition" entry: "its derived set is the one the app works from ([R3.1])" becomes "its derived set is the reading's working set ([R3.1])". DF R7.1: "and every spectral reading whose working set is on an illuminant, observer or condition other than its collection's" becomes "and every spectral reading carrying a working set on an illuminant, observer or condition other than its collection's". (R7.1 note as FX9-1's.)
  - **Landed:** Vocabulary "Chosen condition" reads "the reading's working set", R7.1 "carrying a working set on"; R7.1's note is FX9-1's as the item says. DF 7,988 after this edit.
- [ ] **FX9-4** (SE9-1) — DF R7.7: "and a corpus at ROWS_CEILING carrying a second condition's set on every reading" becomes "and a generated corpus at ROWS_CEILING carrying a second condition's set on every reading"; DE's outbound Data Foundation fixture list mirrors the word. R7.7 stays ⌛️ ("edited FX9-4").
  - **Blocked: over budget by 2 words** — "generated" took DF to exactly 8,000 (measured), leaving no room for the required "edited FX9-4" on R7.7 (+2); the DE mirror was left unapplied so the two documents stay in step.
- [x] **FX9-5** (MK9-1) — Repository `README.md` line 12: "colors that fall outside your monitor's gamut are marked as such" becomes "colors that fall outside standard sRGB are marked as such". No PRD row.
  - **Landed:** README line 12 now reads "colors that fall outside standard sRGB are marked as such".

## Minors — rows

- [ ] **FX9-6** (T F9-T2, RL9-3, A9-4, PL9-5) — DF R7.3: remove ", a crash during any of the three" from the induced list, and its assertion "and for the copy, snapshot and salvage failures that nothing partial was left behind and no file went missing" becomes "and for the copy, snapshot and salvage failures and a crash during any of the three that nothing partial was left behind and no file went missing". Rule-6 note.
  - **Blocked: over budget by 4 words** — the two R7.3 edits net +1, taking DF to exactly 8,000 (measured), and the Rule-6 note "edited FX9-6, alignment kept" is a further +4.
- [x] **FX9-7** (PM-F9-4, IF released Minor) — DF surface R7.6h: "both ways back" becomes "each way back". Surface row; log records it.
  - **Landed:** surface R7.6h reads "each way back"; no Status change, DF unchanged at 7,999.
- [x] **FX9-8** (T F9-T7) — DF surface R7.6d: drop ", in the vocabulary the sync and network warnings use" so the cell reads "…a local disk, a syncing folder or a network volume — the actions, each move failure named". Surface row.
  - **Landed:** surface R7.6d drops the vocabulary clause, −9 words, DF 7,990.
- [ ] **FX9-9** (test Nit) — DF R7.7's fixture "an imported value a parser would reshape" becomes "an imported value the dialect would reshape"; DE's outbound mirror likewise.
  - **Blocked: over budget by 2 words** — the wording swap itself is word-neutral in both documents, but R7.7 is a held row edited here, so Rule-6 requires "edited FX9-9" on its Commit PR cell (+2) and DF had 1 word of room; left unapplied in both documents rather than land an edited held row with no note.

## Copy and journeys

- [x] **FX9-10** (RL9-2, SR9-4, SE9-3, PL9-1, IF R9-IF-3; MK9-3) — DF E23's body becomes: "⟨code⟩'s reading from ⟨date⟩ can't be carried into this version, so the upgrade was refused and nothing was changed — your file is exactly as it was, still version ⟨version⟩, and this is version ⟨version⟩. You can work in it read-only, open it in version ⟨version⟩ — the last one that can still change it — or see whether a newer version handles it." DF E16's first sentence becomes "It was made by version ⟨version⟩ and this is version ⟨version⟩, which opens files from ⟨version⟩ onwards." E23 stays ⌛️; E16 Status → ⌛️ Ready for Alignment (edited; the ‹floor› marker stays).
  - **Landed:** E23's body rewritten as given, E16's first sentence replaced and its Status → ⌛️ Ready for Alignment; ‹floor› kept, E23 still ⌛️.
- [x] **FX9-11** (PM-F9-1; PM-F9-6) — DF E8's body: "It takes the reading it has now, its ⟨n⟩ earlier readings, and everything recorded about scanning it." becomes "It takes the swatch and everything you imported with it, the reading it has now, its ⟨n⟩ earlier readings, and everything recorded about scanning it."; its ‹P1› sentence "Exporting with full history carries every earlier reading too, still not the scanning record." becomes "Exporting with full history carries every earlier reading as well as the current one, still not the scanning record." DF E14's body: "This throws away all ⟨n⟩ swatches in it, the ⟨n⟩ readings behind them, and everything recorded about scanning them." becomes "This throws away all ⟨n⟩ swatches in it with everything you imported, the ⟨n⟩ readings behind them, and everything recorded about scanning them."; the same ‹P1› sentence change. Both stay ⌛️.
  - **Landed:** E8 and E14 bodies gain the imported-data phrase and both ‹P1› sentences read "as well as the current one"; both stay ⌛️.
- [x] **FX9-12** (PM-F9-3, SR9-3, A9-1, MK9-2, IF Nit) — Both copy headers: "or a phrase naming something the item does not have" becomes "or a phrase naming something that is not there" (keep the two headers byte-identical).
  - **Landed:** both copy headers read "or a phrase naming something that is not there"; the shared Placeholders block is byte-identical across the two files (verified).
- [x] **FX9-13** (PM-F9-5, SE9-6) — DE E1: "and a column saying which of the two it is" becomes "and a column saying which it is". E1 Status → ⌛️ Ready for Alignment.
  - **Landed:** DE E1 reads "and a column saying which it is"; Status → ⌛️ Ready for Alignment.
- [x] **FX9-14** (MK9-4, SR9-6, IF Nit, RL8-3 residue) — DF E28, E29, E30 State cells: "Copy didn't finish — no room / can't write there / destination gone" become "Copy, snapshot or salvage didn't finish — no room / can't write there / destination gone". Labels only; Status cells unchanged.
  - **Landed:** E28, E29 and E30 State cells now read "Copy, snapshot or salvage didn't finish — …"; Status cells untouched.
- [x] **FX9-15** (PL9-6, IF Nit) — DE journeys EJ1 step 12: "A column appended at the end doesn't bump it" becomes "A column appended to the app columns doesn't bump it".
  - **Landed:** EJ1 step 12 reads "A column appended to the app columns doesn't bump it".

## Bookkeeping

- [x] **FX9-16** — Fence labels: DE fence file's F13 map row "(emitted as stored)" already matches R2.4 after FX9-1; confirm and leave it. No other fence edit.
  - **Landed (confirm only):** DE fence F13's map row still reads "(emitted as stored)" and matches R2.4 after FX9-1; no fence edit made.
- [ ] **FX9-17** — Status flips authorised by the log's round-9 flip note: DF R1.7, R6.2, R7.6b, E28 → 🤝 Aligned (Commit PR "aligned round 9" on R1.7 and R6.2); DE R2.2 → 🤝 Aligned ("aligned round 9"). Held after this pass: DF R1.2 (edited; set ⌛️ Ready for Alignment, "edited FX9-1"), R7.1, R7.5 (no edit; stays ⌛️), R7.7, R5.7 (no edit; set ⌛️ — reliability RL9-2, staff SE9-3), R7.6h (edited; no Status change on surface rows — the log holds it), E8, E14, E23, E16; DE R2.4, R4.1, E1.
  - **Blocked: over budget by 1 word** — the flip set is net +2 on DF (R7.6b −2, R1.7 +1, R6.2 +1, R5.7 +2) against 1 word of room; R5.7's 🤝 → ⌛️ (+2) is the single piece that does not fit, the other five flips (incl. DE R2.2, +1 → 4,000, and E28, free) would. R1.2's ⌛️ + "edited FX9-1" landed with FX9-1 and stands.
- [x] **FX9-18** — Word counts after the pass, by the log's method: DF ≤ 8,000, DE ≤ 4,000; report both.
  - **Landed:** by the log's method — DF 7,999 of 8,000; DE 3,999 of 4,000.

## No edit (recorded in the log)

Declined for budget, both released by the lens: F9-T3 (R7.6k's zero-count fixture), F9-T6 (R7.2's "or both"). Post-lock list: a CSV → store → CSV round-trip case for the passthrough value; the generated corpus's determinism contract (ADR-0003); E8's never-scanned rendering checked at build review; PL7-4.

**Owner decision after the first pass (2026-09-16):** the budgets and the count method stand; the five items the pass blocked on budget (FX9-2, FX9-4, FX9-6, FX9-9, FX9-17) land by trimming meaning-neutral words elsewhere in each body — see FX9-19.

- [ ] **FX9-19** (owner: trim prose to fit) — Trim at least 14 DF words and 6 DE words from prose paragraphs outside the requirement, copy-index, surface and obligations tables — the Background, the Legend's explanatory paragraphs, §4's and §6's index prose, §8's paragraph, the Open Questions' interim/closer cells — removing only words that carry no rule (doubled qualifiers, "the fact that", restated cites, a parenthetical the row already states). Never touch a requirement row's rule text, a copy body, a status or Commit PR cell, or a fence cite. Record each trim as "before → after" in the Landed note. Then apply FX9-2, FX9-4, FX9-6, FX9-9 and FX9-17 in full, with their rule-6 notes as written, and report the final counts under FX9-18.
