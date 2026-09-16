# Data Foundation and Data Export PRDs — round 10 fixes

Resume point for round 10 (log: "Round 10"). One file for both documents: DF = Data Foundation, DE = Data Export (`../export/`). Fences DF F1–F32 and DE F1–F13 are the owner's authorization; process rules 1–8 bind every edit — two sentences per row, cite rather than restate, never expand where a clause will do; the editor may shorten but not lengthen. Cross-PRD cites name the owning document in the visible label. Tick each box with a one-line **Landed:** note. Budgets: DF 8,000 (9 words of headroom before this pass), DE 4,000 (9 before). Apply FX10-8's trims and FX10-3 (word-negative) first, then the rest, counting after each DF-body edit.

**Rule-6 notes.** A held row edited here stays ⌛️ and its Commit PR cell gains "edited FX10-n". An Aligned row edited here keeps 🤝 Aligned and gains "edited FX10-n, alignment kept". Copy rows FX10-9 flips go to 🤝 Aligned. The Vocabulary and the obligations tables have no cells for notes; the log records them.

## Majors

- [ ] **FX10-1** (SR10-1, RL10-1, A10-2; IF10 watch item, PL10 and T10 Nits) — DF R7.1: "and every imported column under the name, position and value its import file gave it" becomes "and every imported column under the name, position and value [R1.2](#1-the-file-the-user-owns) keeps"; and "every spectral reading carrying a working set on an illuminant, observer or condition other than its collection's" becomes "every spectral reading carrying a working set stamped with an illuminant, observer or condition other than its collection's, or none and no absent mark ([R3.5](#3-derived-values-and-gamut-honesty))". Stays ⌛️; Commit PR gains "edited FX10-1".

## Minors — rows and entries

- [ ] **FX10-2** (SR10-2, A10-2, SE10-6, PL10/T10 Nits) — DF Vocabulary, the "Chosen condition" entry: "its derived set is the reading's working set ([R3.1])" becomes "a reading's working set is the set the app works it from ([R3.1])" (same link).
- [ ] **FX10-3** (SE10-3, A10-1, T10 Nit) — DF R7.7: "…differs from its items' insertion order, and a generated corpus at ROWS_CEILING carrying a second condition's set on every reading ([M6], [M8])." becomes "…differs from its items' insertion order; a generated corpus at ROWS_CEILING carries a second condition's set on every reading ([M6], [M8])." (same links; word-negative). Stays ⌛️; Commit PR gains "edited FX10-3". DE's outbound mirror keeps "a generated corpus at ROWS_CEILING carrying…" unchanged.
- [ ] **FX10-4** (SE10-2, IF10-1 half) — DF R1.2: "the value the user's to change ([R2.3](#2-canonical-value-and-version-history))" becomes "the value the user's to change ([R2.3](#2-canonical-value-and-version-history), [the import PRD's R3.5](../import/prd-inventory-import.md#3-preview-and-commit))" — confirm the anchor slug against the import PRD's §3 heading before writing it. Stays ⌛️; Commit PR gains "edited FX10-4".
- [ ] **FX10-5** (SE10-1) — DE R2.4: "emitted exactly as stored, nothing here reshaping it (fence F13)" becomes "emitted exactly as stored, nothing but [R2.2](#2-columns-names-dialect-and-the-version)'s quoting reshaping it (fence F13)". Stays ⌛️; Commit PR gains "edited FX10-5".
- [ ] **FX10-6** (IF10-2, SE10-5) — DE R4.1: "with the diff, a bump's confined to" becomes "with the diff, a bump's diff confined to". Stays ⌛️; Commit PR gains "edited FX10-6".
- [ ] **FX10-7** (PL10 Nit) — DF outbound Inventory Import line: "keeps the existing columns' positions and appends its new ones after them" becomes "keeps the existing columns' positions and values and appends its new ones after them". Index text.
- [ ] **FX10-8** (owner's round-9 decision applied once more) — Trim at least 8 words from DF prose paragraphs outside the requirement, copy-index, surface, metrics and obligations tables, under exactly the rules FX9-19 stated (rule-free words only; never a rule cell, a copy body, a status or Commit PR cell, a fence cite, a link or a ‹marker›; each trim recorded before → after). Zones not yet trimmed: the Legend's Priority and Provisional-constants paragraphs are off limits (template-shared); prefer the Background's second paragraph, §4/§6/§8 prose, the Surfaces table's preamble, and OQ Interim-rule cells other than OQ 2's.

## Companions

- [ ] **FX10-9** (SR10-3) — DE fence file, F13's **Why**: "…an imported `0007` or `1,234.5` comes back out as it went in." becomes "…an imported `0007` or `1,234.5` comes back out as the file holds it, the app reshaping nothing on the way." Fence file, unbudgeted.
- [ ] **FX10-10** (IF10-3, SE10-7, privacy Info) — DE journeys EJ1 step 11: "a column saying which of the two it is" becomes "a column saying which it is" (line 22 only; line 29's "which of the two" names the two export kinds and stays).

## Bookkeeping

- [ ] **FX10-11** — Status flips authorised by the log's round-10 flip note: DF E8, E14, E16, E23 → 🤝 Aligned; DE E1 → 🤝 Aligned. Held after this pass: DF R1.2, R7.1, R7.5 (no edit), R7.7; DE R2.4, R4.1. R5.7 stays ⌛️ (owner overrule pending, no edit).
- [ ] **FX10-12** — Word counts after the pass, by the log's method: DF ≤ 8,000, DE ≤ 4,000; report both.

## No edit (recorded in the log)

Declined for budget, each released by its lens and on the post-lock list: T10-1 (R7.6b's versions), P10-1 (R7.2's cleared imported value), SE10-4 (`sc_simulated` false golden), IF10-1 (Collection Mode rename), SE10-8/A10-3 (OQ 2's ADR-0006 gloss — `docs/decisions/README.md` registers it), SR10-4 (R7.3's induced-list wording), SR10-5, SR10-6, SR10-7, PM's README gloss and DE E1 one-zero rendering.
