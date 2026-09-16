# Data Foundation and Data Export PRDs — round 8 fixes

Resume point for round 8 (log: "Round 8"). One file for both documents: DF = Data Foundation, DE = Data Export (`../export/`). Fences DF F1–F32 and DE F1–F13 are the owner's authorization; process rules 1–8 bind every edit — two sentences per row, cite rather than restate, never expand where a clause will do; every clause below is the shortest wording that carries the rule, and the editor may shorten but not lengthen. Cross-PRD cites name the owning document in the visible label. Tick each box with a one-line **Landed:** note. Budgets after the pass: DF 8,000 (F21; 78 words of headroom before this pass), DE 4,000 (DE F1; 58 before this pass).

**Rule-6 notes.** An Aligned requirement or metric row edited here keeps 🤝 Aligned and its Commit PR cell gains "edited FX8-n, alignment kept" (appended after any existing note with "; "). A held row (⌛️) edited here stays held and its Commit PR cell gains "edited FX8-n". A copy row keeps its Status unless FX8-17 says otherwise; the log records the edit. Surface rows have no Commit PR cell; the log records them.

## Majors

- [x] **FX8-1** (T F8-T1, A8-1, SE8-1, PL8-2, SR8-2) — DF R7.1: replace "or carrying an illuminant, observer or condition other than its collection's" with "and every spectral reading whose working set is on an illuminant, observer or condition other than its collection's". Status → ⌛️ Ready for Alignment; Commit PR "edited FX8-1".
  - **Landed:** R7.1's second sentence now enumerates "every spectral reading whose working set is on an illuminant, observer or condition other than its collection's"; Status → ⌛️ Ready for Alignment, Commit PR gains "edited FX8-1".
- [x] **FX8-2** (PL8-1) — DF R7.5: replace "interrupting and resuming that run the same way" with "interrupting and resuming that run and asserting it completes with [R7.1]'s mismatch enumeration empty". Status → ⌛️; Commit PR "edited FX8-2".
  - **Landed:** R7.5's condition-change clause now asserts the resumed run completes with [R7.1](#7-verifiability)'s mismatch enumeration empty; Status → ⌛️, Commit PR gains "edited FX8-2".
- [x] **FX8-3** (IF R8-IF-1, SR8-1) — DE R2.4: "a passthrough column's value emitted as stored (fence F13)" becomes "a passthrough column's value emitted exactly as its import file gave it (fence F13)". Status → ⌛️; Commit PR "edited FX8-3".
  - **Landed:** DE R2.4's first sentence now reads "emitted exactly as its import file gave it (fence F13)"; Status → ⌛️, Commit PR gains "edited FX8-3".
- [x] **FX8-4** (T F8-T2, IF R8-IF-2, SR8-1 rider) — DF R7.7's fixture list: after "one whose rename collides again" add ", an imported value a parser would reshape"; DE's outbound Data Foundation fixture list mirrors it in the same place. R7.7 Status → ⌛️; Commit PR "edited FX8-4". The outbound line is index text.
  - **Landed:** "an imported value a parser would reshape" added to DF R7.7's fixture list and to DE's outbound Data Foundation line, in both cases after the collision pair and before "a column mapped to identity" (in DF that puts it after the collision pair's "says what collides" parenthetical, which stays bound to what it explains); R7.7 Status → ⌛️, Commit PR gains "edited FX8-4".
- [x] **FX8-5** (PM-F8-1, MK8-1, A8-2, IF R8-IF-4, SR8-3, PL8-4, SE8-5, SR8-7) — Both copy headers (DF `prd-data-foundation-copy.md` Placeholders block; DE `prd-data-export-copy.md`'s matching clause) replace the zero-count sentence with: "A count that would be zero, or a phrase naming something the item does not have, is never shown as "0" or as a claim: the phrase or clause it governs is dropped with the conjunction or punctuation that joined it, and the rest of the sentence stands; a sentence, list line or action that exists only for that phrase, or that would not stand without it, is left out." E8 and E14 bodies unchanged. E8 stays ⌛️; E14 Status → ⌛️ (marketing OBJECT MK8-1).
  - **Landed:** the zero-count sentence replaced verbatim in both Placeholders blocks; E14's Status cell → ⌛️ Ready for Alignment, E8 left ⌛️, both bodies untouched.
- [x] **FX8-6** (PL8-3, SR8-4) — DE R2.2: "Every app column's value is written in one form too — reflectance as a fraction of 1, times in RFC 3339 with an explicit UTC offset," becomes "Every app column's value but identity's is written in one form too — reflectance as a fraction of 1, times in RFC 3339 in UTC with the offset written as `Z`,". Status → ⌛️; Commit PR "edited FX8-6".
  - **Landed:** DE R2.2's second sentence now excepts identity and writes times in UTC with the offset as `Z`; Status → ⌛️, Commit PR gains "edited FX8-6".

## Minors — rows

- [x] **FX8-7** (T F8-T5, PL8-5, A8 nit) — DF R5.7's second sentence: "That state names the file's version…" becomes "That state and [R5.2](#5-migration-and-compatibility)'s refusal name the file's version, the app's, and the last app version that can still change it ([E16], [E23], [R7.3])" — add the E23 cite in the same link shape as E16's. Rule-6 note on R5.7. E23 Status → ⌛️ (plan OBJECT PL8-5); body unchanged.
  - **Landed:** R5.7's second sentence gains R5.2's refusal and the E23 cite in E16's link shape; R5.7 keeps 🤝 Aligned with Commit PR "edited FX8-7, alignment kept", E23's Status cell → ⌛️ Ready for Alignment with its body unchanged.
- [x] **FX8-8** (T F8-T3) — DF R7.2: "taking a confirmation's export-first action leaves the item and its history present with the confirmation still open" becomes "taking a confirmation's export-first action runs the export and leaves what the confirmation names present, the confirmation still open". Rule-6 note.
  - **Landed:** R7.2's second sentence now reads "runs the export and leaves what the confirmation names present, the confirmation still open"; Commit PR gains "edited FX8-8, alignment kept".
- [x] **FX8-9** (RL8-1) — DF R7.3's induced list: after "each failure of a copy, a snapshot and a salvage ([E28]–[E30])" add ", a crash during any of the three". Rule-6 note.
  - **Landed:** R7.3's induced list gains ", a crash during any of the three" after the copy/snapshot/salvage failures; Commit PR gains "edited FX8-9, alignment kept".
- [x] **FX8-10** (RL8-2) — DE R4.1: "updating the golden in place with the diff as the review artifact" becomes "updating the golden in place with the diff, confined to the derived-value and derivation-version columns, as the review artifact". Stays ⌛️; Commit PR gains "edited FX8-10".
  - **Landed:** DE R4.1's first sentence now confines the golden-update diff to the derived-value and derivation-version columns; stays ⌛️, Commit PR gains "edited FX8-10".
- [x] **FX8-11** (IF R8-IF-5) — DF R6.2: "— the canonical export ([the export PRD's E1](…)), or, until its history option lands ([that PRD's R1.3](…)), the complete copy the state points at ([R5.8](…)) —" becomes "— the canonical export ([the export PRD's E1](…)), the state also pointing at the complete copy ([R5.8](…)) until its history option lands ([that PRD's R1.3](…)) —" (same three links, reordered). Stays ⌛️; Commit PR gains "edited FX8-11".
  - **Landed:** R6.2's export-first parenthetical reordered as written, all three link targets unchanged; stays ⌛️, Commit PR gains "edited FX8-11".
- [x] **FX8-12** (T F8-T6) — DF R7.6d: "in [E25]'s and [E27]'s words" becomes "in the vocabulary [E25] and [E27] use" (same links). Surface row; log records it.
  - **Landed:** surface row R7.6d's "What the test lists" cell now reads "in the vocabulary [E25] and [E27] use", both link targets unchanged; no Status or Commit PR change.
- [x] **FX8-13** (MK8-2, F8-T4, SE8-3, RL8-4, A8 nit, SR8-5) — DF §8 (the Error & State Copy paragraph): "E4, E10 and E11 are each listed on two surfaces" becomes "E4, E10, E11 and E28–E30 are each listed on two surfaces". Editorial.
  - **Landed:** §8's closing sentence now names "E4, E10, E11 and E28–E30".

## Copy and journeys

- [x] **FX8-14** (SE8-4, IF R8-IF-7, RL8-3) — DF E28's headline "There isn't room for that copy" becomes "There isn't room to finish that". E28 stays ⌛️.
  - **Landed:** E28's Headline cell now reads "There isn't room to finish that"; Status left ⌛️ Ready for Alignment.
- [x] **FX8-15** (privacy Info, F8-T7, SE8-2, RL8-5, SR8-6, IF R8-IF-3) — DE journeys EJ1 step 12: "a column removed, renamed, moved, or changed in meaning, or the set of wavelength columns changing" becomes "a column removed, renamed, moved, changed in meaning or in the form its values are written in, or the set of wavelength columns changing".
  - **Landed:** EJ1 step 12's version-bump list now carries "changed in meaning or in the form its values are written in".
- [x] **FX8-16** (A8 nit) — DF journeys DJ3's disk-full branch: "The app says so, and on a local disk my file is exactly as it was before the write it couldn't finish. There is never a half-written reading, and never a file that won't open next time." becomes "The app says so, and on a local disk my file is exactly as it was before the write it couldn't finish — never a half-written reading, never a file that won't open next time."
  - **Landed:** DJ3's disk-full branch is now the single em-dash sentence as written.

## Bookkeeping

- [x] **FX8-17** — Status flips authorised by the log's round-8 flip note: DF E29, E30 → 🤝 Aligned. Held (⌛️) after this pass: DF R1.7 (no edit; Commit PR unchanged), R6.2, R7.1, R7.5, R7.7, R7.6b (no edit), E8, E14, E23, E28; DE R2.2, R2.4, R4.1. Set R1.7's Status to ⌛️ Ready for Alignment (interface OBJECT R8-IF-6) without editing its text; same for R7.6b (marketing OBJECT MK8-2).
  - **Landed:** DF E29 and E30 → 🤝 Aligned; R1.7 and R7.6b Status cells → ⌛️ Ready for Alignment with no text change and R1.7's Commit PR untouched; the held set is now exactly DF R1.7, R6.2, R7.1, R7.5, R7.7, R7.6b, E8, E14, E23, E28 and DE R2.2, R2.4, R4.1.
- [x] **FX8-18** — Word counts after the pass, by the method the log uses (HTML comments, links' targets, code fences and table pipes stripped): DF ≤ 8,000, DE ≤ 4,000. Report both in the Landed note.
  - **Landed:** DF 7,988 words (12 under the 8,000 budget); DE 3,976 words (24 under the 4,000 budget).

## No edit (recorded in the log)

Declined: R8-IF-6 (R1.7); PL8-6 (M2/M7 population cells); the test lens's R6.5 "left untouched" note; SE8-6. Post-lock list: privacy's formula-injection note for the help-docs column dictionary; reliability's note that nothing gates a release on that dictionary; the two-copy fixture list (DF R7.7 ↔ DE outbound); PL7-4 (the device PRD's spike-scope amendment lands before the spike is dispatched).
