# Collection Mode PRD — round 3 fixes (review round 3, 2026-09-25)

The resume point for the fix pass over the findings of review round 3 (subject commit `c4ec992`), before round 4. The
fence file `prd-collection-mode-fences.md` (F1–F134) is the written authorization for every change below: a **fix**
names the fence that authorizes it, or reads "editorial/testability — no new WHAT" where it only adds a case, a
readback grant, a fixture, a cite, or a wording fix that makes copy or a hand-off match an existing row. A change no
fence names is not made: it is reported to the orchestrator as an unratified WHAT. Every **needs owner** box waits for
the owner's answer and its own dated fence (F135 on). Each box is ticked, with a Result note, as its fix lands.

**Word budget.** The PRD body's budget is 12,000 words by rule 14's method, and it stands at 12,000 (check 12's count,
re-run at `cd84fd4`): no headroom. Every word a fix adds to the body names, in its Result note, the rule-free text it
replaces in the same pass. A fix that cannot be paid for is not squeezed in: it is flagged to the orchestrator. The
companions (journeys, copy, fences) sit outside the budget, so a fix that can land in a companion lands there. The
fixes below that need no owner answer net about −14 words (see Checks); the owner answers could add up to about +81.
Candidate trims the reviewers proposed, each re-checked rule-free before it goes (counts by check 12's method; the
reviewers' own counts differ by a word or two):

- **§8's preamble paragraph** — "The rows here hold the operating envelope … a class left out is a latent decision."
  (50 words; its class list alone 17) — offered by 3MJ2, 3MN3, 3MN5, R3-M1, R3-m1, R3-m10 and PERF3-5 as an
  authoring checklist that binds no build. **Caveat:** the agent-prd v1 template scaffolds this paragraph as body
  text, not as a guidance comment (`prd-template.md`, the operating-envelope section: "Not conditional — each of those
  classes is answered by at least one row here…"), and this PRD already carries a shortened form of it. No mechanical
  check parses it, but it is trimmed only once the orchestrator confirms the format does not require it.
- **The Open questions trailer** — "Every open question carries an interim rule, or its P0 rows are in the Legend's
  no-interim list; there is no third option." (22) — PM3-3, as a repeat of the Legend's two-list preamble. **Same
  caveat:** the template scaffolds it as body text.
- **The Build contract's last sentence** — "The storage schema (ADR-0003) and the minimum macOS version (ADR-0006) are
  open too, and no row here chooses either." (19) — PM3-2; the Build dependencies paragraph and R8.7 say the same.
  Check first that paragraph 2 still names the open fork (ADR-0004) as its guidance asks.
- **The Build contract's provenance parentheticals** — "(the vision's non-goals)" and "(the vision's v2 candidates)"
  (7) — R3-M1.
- **M1's population wording** (−14 net) — PERF3-5, a fix in its own right below.
- **R8.2's ", history never being aged out"** (5) — 3MJ1, which spends it inside its own fix.
- **OQ 1's Decision-cell status clause** "nothing of this app is measured" (6) — N3-m2.
- **OQ 7's Decision-cell provenance** "F40 adds Bradford adaptation at zero tolerance and F64 ratifies the rest of the
  interim." (15; the Interim cell states it) — ARCH3-5, which would spend it on answer 12's sentence.
- **The Build dependencies paragraph's "(F65)" and "(F124)"** (2) — ARCH3-1.
- **R8.10's purpose clause** "to assert what each action left there" (7) — PRIV-12 (carried).
- **R3.9's "now"** (1) — claimed by both R3-IF-3 and R3-IF-6; spend it once.
- **R4.9's "(E11 in the same detail)"** (5) — R3-IF-9, a fix in its own right, which rewrites flip-eligible R4.9.
- **R1.7's "— this row is not built until that question closes —"** (about 10) — R3-IF-4. R1.7 is aligned and
  flip-eligible, and a trim rewrites it (round 2 moved R4.5 to pre-alignment for a trim), so this one is **not
  recommended**; prefer prose outside the flip-eligible rows throughout.

The findings are the eight round-3 delta reviews in `docs/agent-reviews/2026-09-24-prd-collection-mode-peer-reviews.md`
(Round 3 › Per-lens reviews (verbatim)), in log order; that log's Round 3 verify-the-reviewer table ("log row n" below)
records the accepted Majors. Its rows 4, 6, 7 and 8 are fixes; its four "owner decision" rows are Owner-needed items
1–4 (A–D), which the orchestrator is putting to the owner separately: row 1 is A, row 3 is B, row 5 is C and row 2 is
D. Each box carries the reviewer's own ID and severity. A round-2 finding that a review's delta table marks PARTIAL or
UNRESOLVED gets its own box, labelled "carried: <round-2 ID>". One box carries each fix, the first in log order, and
every other box raising the same defect says **covered by** it; where a later box adds a part the first lacks, that
part is its own. An item a review raised only in its Missing or Deferred list, carried by no rated finding, gets a box
marked *unrated*. Sibling IDs name their document ("DF", "Capture", "Import"); a bare ID is this PRD's own.

Dispositions: **fix**, **covered by**, and **needs owner** — each needs-owner box names its item in the Owner-needed
list below, where the four items asked separately are marked *(asked separately)*. Needs owner is strict: any new or
changed product rule, copy meaning, constant or interim, closure gate, or sibling rule that no fence F1–F134 states.

## Status bookkeeping

- [x] **Flip to aligned** (the log's round-3 flip record, less the holds below) — in the PRD's tables and the copy
      file's Status lines. **37 newly:** E3, E4, E5, E10, E11, E13, E19, M2, M4, R1.10, R2.3, R2.4, R2.4a–i, R3.5,
      R3.7, R3.8, R4.8, R4.9, R5.2, R5.2a–e, R5.5, R5.7, R7.1, R7.2, R8.5. **32 stay aligned:** E1, E2, E7, E14, E15,
      E16, E18, M3, R1.1, R1.2, R1.5, R1.6, R1.7, R1.8, R1.9, R2.1, R2.2, R2.6, R2.7, R2.9, R2.10, R3.1, R3.2, R3.4,
      R3.6, R4.4, R4.6, R5.6, R5.8, R6.1, R8.4, R8.7. No fix box changes their meaning, except that each lands at
      pre-alignment instead if rewritten: R4.9 if R3-IF-9's R4.9 half lands (the orchestrator's call — the bare "E11"
      there is a real cite defect); E13 if answer 11 adds "Undo change" to its actions (not recommended); R1.7 if
      R3-IF-4's trim is taken (not recommended).
      Result: 36 flipped in the PRD tables and the copy file's Status lines — E3, E4, E5, E10, E11, E13, E19, M2, M4, R1.10, R2.3, R2.4 and R2.4a–i, R3.5, R3.7, R3.8, R4.8, R5.2 and R5.2a–e, R5.5, R5.7, R7.1, R7.2, R8.5 — and the 32 stay aligned; R4.9 lands at pre-alignment instead, R3-IF-9's R4.9 half having been taken (its bare "E11" was a real cite defect, and the trim saves 5 body words); E13 and R1.7 are unchanged.
- [x] **Sub-rows hold with their lead — flag to the orchestrator.** The record also lists R4.2a, R4.2b, R4.2d–h,
      R8.1a, R8.10a and R8.10f (10), but a lettered sub-row flips only with its lead and every sibling: R4.2 and
      R4.2c (R3-m9), R8.1 and R8.10 are objected, so all three families hold and carry their lead's status.
      Result: flagged in the pass report; the R4.2 family holds at needs-discussion with its lead, and the R8.1 and R8.10 families sit at pre-alignment with theirs, both leads rewritten this pass.
- [x] **An aligned row now objected — flag to the orchestrator.** R5.1, aligned since round 2, is objected by R3-M3;
      its fix (3MN7) lands in the journeys only, so R5.1 moves to needs-discussion — the one aligned row this round
      moves back.
      Result: R5.1 reads needs-discussion; flagged in the pass report.
- [x] **Objected rows this pass rewrites → pre-alignment:** R3.9 (R3-IF-6; R3-IF-3's trim if taken); R8.1 and
      R8.1a–g (3MJ1, 3N1); R8.2 (3MJ1); R8.3 (R3-IF-3; answers 2, 3, 6, 7); R8.8 (R3-IF-7); R8.10 and R8.10a–f (R3-m1,
      R3-m6, R3-IF-10; PRIV-12); E6 (PM3-7; answers 2, 6, 7); E9 (N3-m4); M1 (PERF3-5).
      Result: every listed row, family, state and metric reads pre-alignment: R3.9, R8.1 and R8.1a–g, R8.2, R8.3, R8.8, R8.10 and R8.10a–f, E6, E9 and M1.
- [x] **Objected rows an owner answer rewrites** → pre-alignment if the answer lands in this pass, else
      needs-discussion: R1.4 (6), R3.3 (8), R4.1 (10), R4.7 (1, 11), R6.2 (9), R8.11 (3, 5 — if either rewrites it),
      E8 (1). R2.5 joins them if answer 12 rewrites it.
      Result: answers landing this pass rewrote R3.3 (8), R4.1 (10), R4.7 (1, 11), R6.2 (9) and E8 (1), each at pre-alignment; answer 6 landed in R8.3 and the copy rather than R1.4, and neither answer 3 nor 5 rewrote R8.11, so both stay objected and unchanged at needs-discussion; answer 12 left R2.5 unchanged (needs-discussion).
- [x] **Objected rows this pass leaves → needs-discussion** (answered in a companion only): R1.3, R2.5, R2.8, R2.11,
      R4.2 and R4.2a–h, R4.3, R4.5, R5.1, R5.3, R5.4, R6.3, R6.4, R8.6, R8.9, E12, E17.
      Result: every listed row, state and family reads needs-discussion, R5.1 moving there from aligned.
- [x] **After the pass:** report the final lists — the record's 79 flip-eligible IDs and 43 objected reconcile to
      the PRD's 122. Expected before any owner answer: 69 aligned (32 staying, 37 new); 22 pre-alignment (19 objected
      rows, states and metrics plus the held R8.1a, R8.10a and R8.10f); 31 needs-discussion (17, R4.2's 7 held
      sub-rows, and the 7 owner-waiting rows) — 29 and 24 once all seven owner-waiting rows are rewritten. New rows,
      states, variants or metrics an owner answer adds enter at pre-alignment.
      Result: the 122 reconcile — 68 aligned (32 staying, 36 new); 28 pre-alignment (15 lead rows, states and metrics, plus R8.1a–g and R8.10a–f); 26 needs-discussion (18 lead rows and states, R1.4 and R8.11 among them, plus R4.2a–h). No owner answer added a row, state, variant or metric; only cases.

## peer-product-manager-reviewer (Claude route)

- [x] **PM3-1 (Major) — E8's undo promise still outruns R4.7, and R4.7's capture exemption builds two ways** (E8 body
      and "clear" variant; R4.7; UJ6.4-f, UJ6.4-m; the capture PRD's R3.6). **needs owner (1 — A, asked separately;
      log row 1).** Its UJ6.4 case — edit, then Skip, Flag and End session, then list the actions — lands with the
      answer.
      Result: landed under F135: R4.7 reads "no write a capture session makes counting" (−1 word); E8's body and "clear" variant name what ends the history — the file closing or being read again, importing, hiding or showing a column, or anything but editing swatches' details, codes or names or renaming a column or collection; UJ6.4-o edits, then skips, flags or ends the session, and undoes.
- [x] **PM3-2 (Minor) — E6's "full" waits on E10's "Undo", so if OQ 10 defers R1.7 a refused P1 action shows a body
      naming none of them** (R8.3, E6 "full"; R1.7, F57; the Harness's in-flight rule). **needs owner (7).**
      Result: landed under F141: E6's "full" drops "and undoing a delete" and is gated on "every P1 action that variant names" in R8.3, the copy's condition and the Harness, so R1.7's deferral never holds it back; dated line under F111.
- [x] **PM3-3 (Minor) — F107's return to E9 covers closing a result, not deleting, flagging, restoring or answering
      one; E9 is not among the surfaces R8.1c updates** (R4.1, R8.1c, E9; UJ3.4-e). **needs owner (10).** Its case
      (delete FS-002 from E9) lands with the answer.
      Result: landed under F144: R4.1 returns to E9 "worked out again", and a deleted or removed item's views close "as closing does, the table showing if E9's item is gone" (+7 words); UJ3.4-j deletes FS-002 from E9 and UJ3.4-k restores a result out of range.
- [x] **PM3-4 (Minor) — E10's "Undo" of a large delete is not refused while a session runs on another collection**
      (R8.3, R8.1g, R8.11; UJ4.4-g). **needs owner (2 — B, asked separately; log row 3).**
      Result: landed under F136: R8.3 refuses E10's "Undo" of a multi-item delete and an "Undo change" of a bulk set or clear on any collection (+15 words); cases UJ1.3-g, UJ6.3-g and UJ6.4-p, and UJ4.4-j for a one-item undo elsewhere staying available; Capture F73 and its obligation line mirror it.
- [x] **PM3-5 (Nit) — "offered only where its latest change's collection is shown" is ambiguous in the All items
      view, whose E13 offers no "Undo change"** (R4.7, E13; UJ7.1-d). **needs owner (11).**
      Result: landed under F145: R4.7 adds "never in the All items view" (+6 words), E13 unchanged; UJ7.1-o.
- [x] **PM3-6 (Nit) — UJ6.4-l's first run lists the actions on Inks, where "Undo change" is never offered, so it
      cannot catch a wrong build** (UJ6.4-l). **fix (F104 — testability; "offered only where its latest change's
      collection is shown"):** choose Studio Markers before listing the actions.
      Result: UJ6.4-l's runs choose Studio Markers before listing the actions.
- [x] **PM3-7 (Nit) — E6 "elsewhere" says a bulk change "waits until it ends", the queue F100 rejected** (E6
      "elsewhere"). **fix (F100 — copy honesty to R8.3's "change nothing"):** N3-m1's rewrite, which also replaces
      "running" with the variant's own "active, paused or halted" — "Scanning in ⟨collection⟩ is active, paused or
      halted. Until that session ends, changes that touch many swatches at once aren't available in any collection,
      so the session's saves aren't held up. Nothing has been changed — do it again once the session has ended." —
      worded to cover whatever answer 2 (B) adds to R8.3's any-collection group.
      Result: E6 "elsewhere" reads N3-m1's rewrite verbatim.
- [x] **PM3-8 (Nit) — DF E34 offers only "Try again" and "OK", with no way forward if permission can't be restored**
      (DF E34; DF R1.9, DF R7.3j). **needs owner (4 — D, asked separately; log row 2).**
      Result: landed under F138 in the Data Foundation PRD: E34 is cause-neutral, hedged on a local disk as E15 is, and offers Choose the file again, run by DF R7.3j and tested by DF R7.6p and a DJ3 line, under DF F54; the ADR-0007 row takes the input naming where access is restored.
- [x] **carried: PM2-1 (PARTIAL) — E8's endings still disagree with R4.7 on imports, column hide/show and capture
      writes** (E8, R4.7). **covered by PM3-1.**
      Result: covered by PM3-1, landed.
- [x] **carried: PM2-2 (PARTIAL) — capture writes other than a save are unclassed, and the All items view is
      ambiguous** (R4.7). **covered by PM3-1** (capture writes) **and PM3-5** (All items).
      Result: covered by PM3-1 and PM3-5, both landed.

## peer-staff-software-engineer-reviewer (Claude route)

- [x] **3MJ1 (Major) — R8.1's lead row applies HISTORY_READINGS_CEILING to every budget; F118 bounds history opens
      only** (R8.1, R8.1b, R8.2, the constants table; F118). **fix (log row 4; F118 — its own scope, no new
      decision):** R8.1's lead drops "and HISTORY_READINGS_CEILING readings an item" (−5); R8.1b reads "opening or
      closing an item detail, or a version history view of up to HISTORY_READINGS_CEILING readings" (+6); R8.2 drops
      ", history never being aged out", which "every reading it holds, however many" already says (−5) — net −4. The
      constants table's owning row for HISTORY_READINGS_CEILING becomes R8.1b; F118's Carried by and map line add
      R8.1b.
      Result: R8.1's lead drops "and HISTORY_READINGS_CEILING readings an item" (−5), R8.1b reads "opening or closing an item detail, or a version history view of up to HISTORY_READINGS_CEILING readings" (+6), R8.2 drops ", history never being aged out" (−5): net −4; the constants table's owning row is R8.1b; F118's Carried by and map line add R8.1b.
- [x] **3MJ2 (Major) — nothing orders a session's start or resume, or another write, against a bulk write or delete
      already running (up to DELETE_WRITE_BUDGET, 10 s); UJ9.5-g's delete run types into a surface no row names**
      (R8.3, R8.1f, R8.1g, R8.11, R2.2, R1.4; Capture F72; UJ9.5-d, UJ9.5-g, UJ1.2-a, UJ1.2-f). The ordering: **needs
      owner (3 — C, asked separately; log row 5)**, its Import half item 15; the mid-write session start joins UJ9.5-g
      with the answer. UJ9.5-g's surface: **fix (F99, F128 — testability; R8.1g's "the surface"):** its
      collection-delete run sends its keystrokes and selections, during the delete, to another of UJ9.5-b's
      collections (PERF3-9's words).
      Result: the ordering landed under F137: R8.1f — R8.1g as it — shows every other write this PRD offers and a session's start or resume disabled until the write lands, marked an inherited obligation for Capture (+24 words), and the Outbound Capture row names its R3.5, which Capture F73 amends (with T7); UJ9.5-g's collection-delete run sends its keystrokes and selections to another of UJ9.5-b's collections and tries the capture PRD's start there mid-delete. The Import half landed as Import F66 under F149.
- [x] **3MN1 (Minor) — E10's "Undo" is refused only on "that collection", and an in-flight "Undo change" of a bulk
      set or clear is refused only through R4.7's re-check, with no case** (R8.3, R8.1g, R4.7; UJ6.4). **covered by
      PM3-4** — both halves are item 2 (B); its UJ6.4 case (undo a 10-item clear with a session on Gouache Set in
      flight → E6 "elsewhere") lands with the answer.
      Result: covered by PM3-4, landed; UJ6.4-p undoes an 11-item clear with a session on Gouache Set in flight, rendering E6 "elsewhere".
- [x] **3MN2 (Minor) — which E6 variant renders when "interrupted" meets "elsewhere", or the body or "full"** (R1.4,
      R8.3, E6; the capture PRD's R3.2, R3.5, R9.7; UJ1.2). **needs owner (6).**
      Result: landed under F140: E6's "interrupted" condition adds "and no session is in flight"; R8.3 reads "never R1.4's "interrupted"" in place of "beside" (word-neutral); UJ1.2-g (interrupted here, a session elsewhere → "elsewhere") and UJ1.2-h (interrupted beside a one-row session → the body).
- [x] **3MN3 (Minor) — E6's "full" waits on an action OQ 10 may defer past v1** (R8.3, E6 "full", R1.7). **covered by
      PM3-2.**
      Result: covered by PM3-2, landed.
- [x] **3MN4 (Minor) — R4.7 exempts only a capture save or a session start, so a skip, set-aside, pause, end, resume
      or queue-list reorder ends the history although E8 says scanning doesn't count** (R4.7, E8; UJ6.4-i).
      **covered by PM3-1** (its skip-then-undo case included).
      Result: covered by PM3-1, landed (UJ6.4-o's Skip run included).
- [x] **3MN5 (Minor) — in the All items majority, does a mismatched non-spectral reading vote under its value's pair,
      under its collection's, or not at all** (R3.3; UJ7.1-n). **needs owner (8).** The cases that tell the readings
      apart (R3-m10's inks, plus a second fixture) land with the answer.
      Result: landed under F142: R3.3 adds "a mismatched non-spectral reading not counting" (+6 words); UJ7.1-p (R3-m10's inks, which tells not counting from counting under the value's pair) and UJ7.1-q (a second fixture, which tells it from counting under the collection's pair).
- [x] **3MN6 (Minor) — UJ9.5-e and UJ9.5-g run "a Release build" but read R8.10b, which F132 keeps to test builds**
      (R8.10, R8.10b; UJ9.4-e, UJ9.5-e, UJ9.5-g). **fix (F132 — testability; the Harness only):** a timing case
      that reads R8.10b or R8.10c runs a Release-configuration test build; "a Release build" everywhere else, UJ9.4-e's
      included, is the shipped configuration.
      Result: the Harness's new Phases and builds paragraph: a timing case reading R8.10b or R8.10c runs a Release-configuration test build, and "a Release build" is otherwise the shipped configuration.
- [x] **3MN7 (Minor) — UJ5.1-a and UJ5.1-b assert E17 with no variant, which a correct build fails once R5.5 lands**
      (UJ5.1-a, UJ5.1-b, UJ5.3-n; R5.3, E17; the Harness's in-flight rule). **fix (log row 7; F111, F97 —
      testability):** the Harness's substitution rule covers E6, E17 and E18 — a case asserting one of them with no
      variant asserts its phase variant ("full", "restore", "restore") in a build where every P1 action that variant
      names has landed (R8.3, R5.3, R4.9), E6's gate as answer 7 sets it — and states that a case runs in every build
      from the phase that lands its rows, the Named defaults' "Build phase" naming the build under test rather than an
      R8.10a input (SSE question 11; R3-IF-1's grant point).
      Result: the Harness's Phases and builds paragraph: a case runs in every build from the phase that lands its rows, and one asserting E6, E17 or E18 with no variant asserts its phase variant where every P1 action that variant names has landed; the Named defaults' Build phase names the build under test, not a declared input.
- [x] **3MN8 (Minor) — the per-word byte check matches a 3-letter word ("Sky") in high-entropy bytes by chance** (the
      Harness's byte-check rule; T6, UJ4.2-a). **fix (F35, F102 — testability):** in raw bytes the per-word rule
      checks words of four or more letters; every word is still checked in the text values read at
      SQLITE_READER_FLOOR.
      Result: the byte check's raw scan checks words of four or more letters; the SQL read at SQLITE_READER_FLOOR checks every word.
- [x] **3N1 (Nit; pre-existing) — R8.1f's "or an undo of one of them" takes in "Use as scan order", which nothing
      undoes** (R8.1f). **fix (editorial — R4.7 undoes no reorder; F99's "their undo"):** "or an undo of a set or
      clear" (word-neutral).
      Result: R8.1f reads "or an undo of a set or clear" (+1 word, not word-neutral as estimated).
- [x] **3N2 (Nit) — no row says what Return fires inside E8; R1.6's cancel default covers deletes only** (R6.2, E8;
      R1.6). **needs owner (9).**
      Result: landed under F143: R6.2 reads "or E8's "Cancel", which Return fires there" (+4 words); UJ6.2-l.
- [x] **3N3 (Nit) — E9's list and distances are stale on return after a restore in the opened detail** (R4.1, R3.9,
      E9). **covered by PM3-3.**
      Result: covered by PM3-3, landed.
- [x] **carried: 2MJ2 (PARTIAL) — (ii) a session starting, or another write landing, while a bulk write or delete
      runs** (R8.3). **covered by 3MJ2.**
      Result: covered by 3MJ2, landed.
- [x] **carried: 2MN3 (PARTIAL) — whether a mismatched non-spectral reading counts toward the majority** (R3.3).
      **covered by 3MN5.**
      Result: covered by 3MN5, landed.
- [x] **carried: 2MN5 (PARTIAL) — capture writes other than a save or a session start are unclassed** (R4.7).
      **covered by PM3-1** (through 3MN4).
      Result: covered by PM3-1, landed.
- [x] *unrated* **SSE Deferred (to test) — UJ9.5-f has no functional timeout** (UJ9.5-f). **fix (F129 —
      testability):** UJ9.5-f declares a functional timeout per step, as UJ2.1-d does, long enough to catch a hang
      and never read as a budget (R8.1 promises none above a ceiling).
      Result: UJ9.5-f asserts each step completes within 60 s, a functional timeout and not a budget.

SSE's other Deferred items are carried by boxes above or below: 3MJ2's arbitration mechanism → 3MJ2; F102 under WAL
against concurrent reads → ARCH3-4; an import commit in flight (its question 14) → item 15; 3MJ1's infeasibility →
3MJ1; 3MN6–3MN8 → their boxes; E6's "waits" → PM3-7; E8's "scanning doesn't count" → PM3-1; E8's Return → 3N2.
Whether R8.1g's 10 s holds under the F102 scrub needs no box: the performance lens answers it (PERF2-1 RESOLVED —
3.12 s measured, est. 6–8 s on the M1 Air against 10 s). The review's own note stands: 3MN8 targets the Harness's
byte-check rule, which carries no row ID, and question 14 targets the Import PRD.

## peer-test-reviewer (Claude route)

- [x] **R3-M1 (Major) — an in-flight "Undo change" of a bulk set or clear, and E10's "Undo" of a delete elsewhere,
      stay allowed, and no case fires either** (R8.3, R8.11, R8.1f, R8.1g, E6 "elsewhere"; F100). **covered by PM3-4**
      (2 — B). Its cases — a clear of Family on 11 items, a session on Gouache Set in flight in each in-flight state,
      "Undo change" → E6 "elsewhere" and the 11 still empty; Gouache Set deleted, Studio Markers in flight, E10's
      "Undo" → E6 "elsewhere" — and the capture PRD's F72 mirror land with the answer.
      Result: covered by PM3-4, landed: UJ6.4-p (an 11-item Family clear, a session on Gouache Set in each in-flight state, "Undo change" → E6 "elsewhere", the 11 still empty) and UJ1.3-g (Gouache Set deleted, Studio Markers in flight, E10's "Undo" → "elsewhere"); Capture F73 mirrors F72.
- [x] **R3-M2 (Major; carried R2-m14) — "the file's bytes equal those before" fails correct SQLite builds
      (statistics written at close, checkpoints, side files present only while open)** (UJ2.1-d, UJ9.4-b, the capture
      PRD's UJ3.3-l; R2.5, R8.6). **fix (log row 6; F6, F20, F53, F84 — testability):** each becomes "read with the
      app closed at SQLITE_READER_FLOOR, every table holds the same rows as the seeded file read before launch,
      SQLite's own statistics tables aside"; UJ9.4-b keeps its byte check for qqq; the capture PRD's UJ3.3-l asserts
      the main file unchanged plus the same row-level read, in a dated line under Capture F71.
      Result: UJ2.1-d and UJ9.4-b assert the row-level read with SQLite's own statistics tables aside, UJ9.4-b keeping its byte check for qqq; the capture PRD's UJ3.3-l asserts the main file's bytes unchanged plus the row-level read, in a dated line under Capture F71 (Capture F73).
- [x] **R3-M3 (Major) — UJ5.1-a fails a correct build from R5.5's phase on** (UJ5.1-a; R5.3, E17 "restore"; the
      Harness). **covered by 3MN7.**
      Result: covered by 3MN7, landed.
- [x] **R3-m1 (Minor; carried R2-M3) — E12's marks and the history lines' never-true and awaiting-answer marks have
      no symbol readback** (R8.10c, R2.8, R8.9, E12; UJ2.1-k, UJ9.6-a; the Test-controls map). **fix (F38 —
      testability, a readback grant as 2MN16's):** R8.10c reads "Each chip's, E12's and each history line's marks by
      identifier" (+5 words, paid from the trims above); UJ2.1-k asserts E12's eleven identifiers, each symbol equal
      to the chip's; UJ9.6-a reads never-true and awaiting-answer on a history line; the map's accessibility line
      cites R8.10c.
      Result: R8.10c reads "Each chip's, E12's and each history line's marks by identifier" (+5, paid from the Build contract trims); UJ2.1-k asserts E12's eleven identifiers, each symbol equal to its chip's or history line's; UJ9.6-a reads never-true on ZX-014's P1 line and awaiting-answer on ZX-013's T2 line; the map's accessibility line cites R8.10c.
- [x] **R3-m2 (Minor) — only R3.9's unlist direction is tested, and R6.4's keep branch on a search change is
      unasserted** (R3.9, R6.4; UJ9.1-j, UJ6.1-c). **fix (F44 — testability):** a case with the row-state filter value
      captured in which a capture save on ZX-010 lists it; a case with ZX-013 selected in which typing zx-01 keeps it
      selected.
      Result: UJ9.1-m (the captured filter, a capture save lists ZX-010) and UJ9.1-n (ZX-013 selected, typing zx-01 keeps it).
- [x] **R3-m3 (Minor) — no case runs R8.3's per-collection list with the session on another collection** (R8.3).
      **fix (F14, F30, F100 — testability):** with a session on Gouache Set in flight, "Delete swatch" on Studio
      Markers' ZX-010 opens the Data Foundation PRD's E8, not E6.
      Result: UJ4.4-i.
- [x] **R3-m4 (Minor) — the Undo-history cases list actions on an undeclared surface** (R4.7; UJ6.4-f, UJ6.4-g,
      UJ6.4-h, UJ6.4-l, UJ6.4-m). UJ6.4-l: **covered by PM3-6**; the rest: **fix (F104 — testability):** UJ6.4-f's
      import and re-read runs, UJ6.4-g and UJ6.4-h end "choose Studio Markers and list the actions offered", and
      UJ6.4-m adds UJ9.1-a's "show Studio Markers' collection surface" step before listing.
      Result: UJ6.4-f's runs, UJ6.4-g and UJ6.4-h end "choose Studio Markers and list the actions offered"; UJ6.4-m shows Studio Markers' collection surface before listing.
- [x] **R3-m5 (Minor) — the byte check needs calibration: short words, app-storage matching, thin controls,
      prefix-compressed index terms** (the Harness's byte check; R8.10, R8.10d, R8.10e; UJ9.8-c; UJ3.3-f, UJ9.4-b,
      UJ6.4-j, UJ6.4-k, UJ10.1-d). (a) **covered by 3MN8**; (b)–(d) **fix (F35, F53, F102 — testability):** every
      R8.10e read uses the byte check's matching, unified-log entries read decoded; UJ9.8-c gains control runs — Warm
      as UTF-16LE in a side file, and warm in an internal table; any full-text index's vocabulary is read at
      SQLITE_READER_FLOOR.
      Result: (b)–(d) in the Harness: every read of the app's own storage for a text uses the byte check's matching, unified-log entries read decoded; any full-text index's vocabulary is read at SQLITE_READER_FLOOR; UJ9.8-c gains the UTF-16LE side-file and internal-table control runs, its copy now T1's state (ZX-010 deleted) so that PRIV3-7's whole-text exemption does not exempt the control's own text.
- [x] **R3-m6 (Minor; carried R2-m13) — a side file can hold "zx-010", which R8.10e counts as app storage** (R8.10e,
      the Vocabulary; UJ3.3-f). **fix (F53 — testability):** R8.10e's "never the user's file itself" → "never the
      file's bytes" (−1 word).
      Result: R8.10e reads "never the file's bytes" (−1).
- [x] **R3-m7 (Minor; carried R2-m10) — R8.1c's kinds other than capture saves are untimed, F112's history half has
      no functional case, and UJ9.5-a sends 20 where M1 says 200 of each kind** (R8.1c, M1; UJ9.5-a, UJ9.1-l). **fix
      (F41, F112 — testability):** UJ9.5-a times 200 of each R8.1c kind and 200 each of display moves, Table/Grid
      switches and swatch-size changes; a case answers ZX-013's re-scan with its history open and reads T3's reason
      as re-measurement in E17. M1's wording is PERF3-5's.
      Result: UJ9.5-a times 200 of each R8.1c kind and 200 each of display moves, Table/Grid switches and swatch-size changes, its Given declaring 200 re-scan items and 200 pending; UJ9.1-o answers ZX-013's re-scan with its history open; M1's wording is PERF3-5's.
- [x] **R3-m8 (Minor) — nothing tests history above HISTORY_READINGS_CEILING, so a view capped at 500 passes** (R8.1,
      R8.2; UJ9.5-c, UJ9.5-e). **fix (F118, F129 — testability; R8.2's "however many"):** UJ9.5-c adds an item
      holding 1,000 readings, E17's ⟨n⟩ and its list both reading 1,000.
      Result: UJ9.5-c's third item holds twice HISTORY_READINGS_CEILING readings, 1,000 at its candidate, E17's ⟨n⟩ and list reading 1,000; its Rows add R5.1 (P0) rather than R8.2 (P1) so the case keeps its first-phase run.
- [x] **R3-m9 (Minor) — F105's precisions are asserted as formats, so values swapped between slots pass** (R2.11,
      R4.2, R4.2c; UJ4.1-a). **fix (F47, F105 — testability):** the Named defaults declare ZX-001's six derived sets
      and UJ4.1-a asserts their values — the reviewer's u* −32.0, v* −44.7, X 26.79, Y 30.40, Z 49.87, sRGB (91, 157,
      211), HSL (207°, 58%, 59%), recomputed by the Harness's checked-in computation before they land.
      Result: ZX-001's Named-defaults line declares its six derived sets and UJ4.1-a asserts them. Recomputed here by CIE 15 at D50/2° with Bradford to D65: u* −31.99, v* −44.67, Y 30.403, Z 49.867, sRGB (90.53, 156.96, 210.86) → (91, 157, 211), HSL (206.9°, 57.7%, 59.1%) → (207°, 58%, 59%), matching the reviewer; but X is 26.79497, on the two-place rounding boundary, contrary to the review's "none sits at a rounding edge", so X is asserted as 26.79 or 26.80 and the line says why; each value is re-checked by the Harness's checked-in computation before a case reads it.
- [x] **R3-m10 (Minor) — the like-pair count is ambiguous and no case tells the readings apart** (R3.3; UJ7.1-n).
      **covered by 3MN5.**
      Result: covered by 3MN5, landed.
- [x] **R3-m11 (Minor) — nothing tests "exists only in test builds"** (R8.10; UJ9.4-e). **fix (F132 — testability):**
      UJ9.4-e also tries each in-app readback — R8.10b and R8.10c — against the shipped Release build, and none
      answers. The reviewer wrote R8.10b–e; R8.10d and R8.10e are reads a test makes from outside the app, which a
      Release build cannot refuse (PRIV3-6 runs them there) — reported.
      Result: UJ9.4-e also tries R8.10b and R8.10c against the Release build, and neither answers; R8.10d and R8.10e are outside reads and are not tried there (reported).
- [x] **R3-n1 (Nit) — UJ9.1-f's and UJ9.1-g's Rows omit R4.9 and R4.7, which their Givens need** (UJ9.1-f, UJ9.1-g).
      **fix (editorial — the preamble phases cases by their Rows):** UJ9.1-f adds R4.9, UJ9.1-g adds R4.7.
      Result: UJ9.1-f adds R4.9 and UJ9.1-g adds R4.7.
- [x] **R3-n2 (Nit) — UJ4.7-d reads the set-aside cause by its label** (UJ4.7-d; R8.10b; the capture copy file's
      Set-aside cause labels). **fix (F96 — testability):** it reads ZX-001's cause by its key through the capture
      PRD's R11.11.
      Result: UJ4.7-d reads ZX-001's cause by its key through the capture PRD's R11.11.
- [x] **R3-n3 (Nit) — UJ9.6-b's surfaces omit the collection list, and UJ9.5-g's delete run names no surface**
      (UJ9.6-b, UJ9.5-g). UJ9.6-b: **fix (F75 — testability):** add the collection list. UJ9.5-g: **covered by
      3MJ2.**
      Result: UJ9.6-b adds the collection list; UJ9.5-g landed with 3MJ2.
- [x] **R3-n4 (Nit) — "interrupted" and "elsewhere" can both apply** (R1.4, R8.3). **covered by 3MN2.**
      Result: covered by 3MN2, landed.
- [x] **carried: R2-M3 (PARTIAL) — E12 and the history lines have no symbol readback** (R8.10c). **covered by
      R3-m1.**
      Result: covered by R3-m1, landed.
- [x] **carried: R2-m10 (PARTIAL) — R8.1c's other kinds untimed; M1's per-kind claim unmatched** (R8.1c, M1).
      **covered by R3-m7.**
      Result: covered by R3-m7, landed.
- [x] **carried: R2-m13 (PARTIAL) — R8.10e still leaves side files readable as app storage** (R8.10e). **covered by
      R3-m6.**
      Result: covered by R3-m6, landed.
- [x] **carried: R2-m14 (PARTIAL; superseded) — the byte-equality checks landed as raw equality** (UJ2.1-d, UJ9.4-b).
      **covered by R3-M2.**
      Result: covered by R3-M2, landed.

## peer-interface-reviewer (Claude route)

- [x] **R3-IF-1 (Minor) — only E6 has a phase-variant rule, so UJ5.1-a and UJ5.3-n contradict once R5.5 lands, and
      "Build phase" is a seam input R8.10a doesn't grant** (the Harness; UJ5.1-a, UJ5.1-b, UJ5.3-n; E17, E18;
      R8.10a). **covered by 3MN7.**
      Result: covered by 3MN7, landed.
- [x] **R3-IF-2 (Minor) — no rule picks between E6's variants when two apply, and R8.3's "beside R1.4's
      'interrupted'" reads as both rendering** (R1.4, R8.3, E6; UJ1.2-b). **covered by 3MN2** (item 6), R8.3's
      wording following the answer.
      Result: covered by 3MN2, landed; R8.3's "beside" is gone.
- [x] **R3-IF-3 (Minor) — E6's "full" is gated three ways (the copy, the Harness, R8.3), which split on "Use as scan
      order", and all three wait on R1.7** (R8.3, E6 "full"; the Harness). The R1.7 half: **covered by PM3-2**; one
      gate: **fix (F111 — F97's pattern gates a variant on the actions it names; editorial):** the copy's condition,
      the Harness and R8.3 all read "every P1 action that variant names", worded to answer 7 (+1 word in R8.3).
      Result: one gate: R8.3's "every P1 action that variant names" (+1), E6 "full"'s condition and the Harness read the same, worded to answer 7.
- [x] **R3-IF-4 (Minor) — once R1.7 lands, undoing a collection delete runs a 10 s write during another collection's
      session** (R8.3, R8.1g, R8.11; F100). **covered by PM3-4.**
      Result: covered by PM3-4, landed.
- [x] **R3-IF-5 (Minor) — E8 promises an undo that R4.7 withdraws after an import** (E8, R4.7; UJ6.4-f). **covered by
      PM3-1.**
      Result: covered by PM3-1, landed.
- [x] **R3-IF-6 (Minor; pre-existing) — R3.9's one list, which R6.4 now cites, leaves out a code change** (R3.9,
      R6.4; R4.3, R4.7). **fix (F44 — its "any change"):** R3.9's "an edit" → "a metadata change" (R4.7's term, which
      takes in a bulk set; +1 word); a case in R4.4's phase: search zx-00 with ZX-001 selected, change its code to
      ZX-100, and it is unlisted and deselected.
      Result: R3.9 reads "a metadata change" (+1), paid by R3.9's "now" (−1); UJ9.1-p (search zx-00, ZX-001 selected, its code changed to ZX-100 → unlisted and deselected), in R4.4's phase.
- [x] **R3-IF-7 (Minor) — R8.8's "that PRD's E34" reads as the capture PRD's, which has its own E34** (R8.8; the
      cite rule). **fix (editorial — the cite rule; −2 words):** the capture clause goes last — "…renders that PRD's
      E15, because another copy of the app holds the file its E10, because permission to the file was lost its E34,
      and because the volume is gone the capture PRD's E26, each changing nothing".
      Result: R8.8's capture clause goes last (−2).
- [x] **R3-IF-8 (Nit) — the copy file's sibling-owned list omits the Data Foundation PRD's E34** (the copy file's
      States preamble). **fix (F101 — editorial):** add it.
      Result: the copy file's sibling-owned list adds the Data Foundation PRD's E34.
- [x] **R3-IF-9 (Nit; pre-existing) — sibling IDs written bare** (UJ2.1-g, UJ2.1-h, UJ2.1-j, UJ8.1-c, UJ8.1-d;
      R4.9). **fix (editorial — the cite rule):** the cases name the device PRD's E22, the capture PRD's E23 and the
      capture PRD's E34; R4.9 drops "(E11 in the same detail)", rationale F78 already carries (−5 words), which
      rewrites flip-eligible R4.9 (see Status bookkeeping).
      Result: UJ2.1-g and UJ2.1-h name the device PRD's E22, UJ2.1-j the capture PRD's E23, UJ8.1-c and UJ8.1-d the capture PRD's E34; R4.9 drops "(E11 in the same detail)" (−5) and lands at pre-alignment.
- [x] **R3-IF-10 (Nit) — R8.10b says "the capture PRD's cause key" where that table's column is "Cause", and UJ4.7-d
      still reads the label** (R8.10b; UJ4.7-d). R8.10b: **fix (F96 — editorial):** "by the capture PRD's Cause
      column" (word-neutral). UJ4.7-d: **covered by R3-n2.**
      Result: R8.10b reads "by the capture PRD's Cause column".
- [x] **R3-IF-11 (Nit) — E6 "elsewhere" reads as a queue** (E6 "elsewhere"). **covered by PM3-7.**
      Result: covered by PM3-7, landed.
- [ ] **carried: IF-16 (UNRESOLVED; deferred by design) — `docs/product/README.md` line 18 still reads "queued"**
      (the product README). **fix (F56 — editorial), at the bookkeeping close**, as round 2's Out-of-scope list
      holds; nothing in this pass.
      Result: not landed, as the box states: it lands at the bookkeeping close, with the product README's index status.

## peer-privacy-reviewer (Claude route)

- [x] **PRIV3-2 (Medium) — nothing places or checks copies of item content kept outside the file, and F103's
      index-or-scan input makes one likely** (R8.2, R8.6, R8.10e; R1.3, R1.4, R4.3, R4.5, R6.2, R6.3; the ADR-0003
      row; T1, UJ1.3-c, UJ4.4-b, UJ6.3-b, UJ6.4-j, UJ6.4-k, UJ3.3-f, UJ9.4-b; DF R1.1). **fix (log row 8; F53, F103
      and the Data Foundation PRD's R1.1 — testability and hand-off; no body words):** (a) the Harness's byte check
      also reads the app's own storage (R8.10e), with the matching R3-m5 sets; (b) a P1 case: after UJ7.1-m's All
      items search, quit and read the app's own storage, which holds none of Sky Blue, Cerulean or Purple; (c) on the
      orchestrator's say-so, as F98 allowed, the ADR-0003 row's input reads "an index in the file, or a scan" (DF
      R1.1).
      Result: (a) the Harness's byte check also reads the app's own storage, with the same matching; (b) UJ7.1-r; (c) the ADR-0003 row reads "an index kept in the file, or a scan (Data Foundation R1.1)", and DF F53 (5) follows in a dated line; no body words.
- [x] **PRIV3-1 (Low; carried PRIV-10) — the carve-out "unless the case names its moment" lets four byte cases read
      closed only** (the Harness's byte check; T1, UJ1.3-c, UJ4.2-c, UJ6.3-b; R8.10d). **fix (F102 —
      testability):** "unless the case's When names the moment at which it reads the bytes"; the four Asserts' "read
      with the app closed" governs only their row-level read.
      Result: the Harness reads "unless the case's When names the moment at which it reads the bytes", and an Assert naming the file read with the app closed governs only its row-level read.
- [x] **PRIV3-3 (Low) — after a crash the SQL read can checkpoint and delete the evidence before the raw scan runs**
      (the Harness's byte check; R8.10d). **fix (F102 — testability):** at each moment the file and every file beside
      it are copied byte for byte before any SQLite connection opens them; the raw scan reads that copy, and the SQL
      read a second copy, read-only.
      Result: the Harness copies the file and every file beside it byte for byte at each moment before any SQLite connection opens them; the raw scan reads that copy, the SQL read a second copy, read-only.
- [x] **PRIV3-5 (Low) — UJ9.4-e asserts no socket or service of any kind, stricter than R8.10, and names no listing**
      (UJ9.4-e; R8.10; F132; AGENTS.md §3's Sparkle). **fix (F132 — testability; F132 stands):** UJ9.4-e asserts no
      listening socket and no cross-process service, as lsof lists the process's TCP, UDP and Unix sockets and launchd
      the services the app registers or offers, that answers with an R8.10b–e value. The reviewer's "no listening
      socket at all" is narrowed to R8.10's scope, which also meets ARCH3-7 — reported.
      Result: UJ9.4-e asserts no listening socket and no cross-process service answering with an R8.10b–e value, as lsof and launchd list them; the network map line names both.
- [x] **PRIV3-4 (Info) — "beside it" has three scopes: the Vocabulary's "the app keeps", DF's and the ADR-0003 row's
      "any … other file", the Harness's "every file"** (the Vocabulary; DF R2.3, DF R6.2a; the ADR-0003 row; the
      Harness). **fix (F102 — editorial; its "journal, log or index beside it"):** the Data Foundation PRD's R2.3 and
      R6.2a say "the app keeps", in a dated line under DF F53; the ADR-0003 row likewise, on the orchestrator's
      say-so; the Harness says a case's folder holds only the file, the files the app keeps beside it and any control
      file the case declares, fixture sources such as UJ6.4-f's CSV sitting elsewhere (NIT-2).
      Result: the Data Foundation PRD's R2.3 and R6.2a say "the app keeps", under DF F54 with a dated line under DF F53; the ADR-0003 row likewise; the Harness says a case's folder holds only the file, the files the app keeps beside it and any control file the case declares, fixture sources such as UJ6.4-f's CSV sitting elsewhere.
- [x] **PRIV3-6 (Info) — no byte or storage case runs against a Release build** (UJ9.4-b, UJ9.4-d; R8.10, F132).
      **fix (F53, F132 — testability; F132 not reopened):** UJ9.4-b and UJ9.4-d run in turn a Release build and a test
      build, as UJ9.4-e does, R8.10d's and R8.10e's reads being made from outside the app.
      Result: UJ9.4-b and UJ9.4-d run in turn a Release build and a test build.
- [x] **PRIV3-7 (Info) — the word exemption keys on the build's own file, and a case-only change or a repeated value
      can't satisfy the whole-text rule** (the Harness's matching; R1.3, R4.3; UJ1.1-d, UJ4.3-g, UJ4.6-i). **fix (F35,
      F102 — testability):** the exemption is worked out from the case's declared post-action values, and the whole
      text is exempt where a declared post-action value contains it in any letter case.
      Result: the Harness exempts a word, and the whole text, where a value the case declares for after the action contains it, in any letter case.
- [x] **PRIV3-8 (Info) — bookkeeping: R1.3's byte clause sits in no fence's Carried by, and the map's app-storage
      line omits the open read UJ6.4-j makes** (F102, the fence → row map; the Test-controls map). **fix
      (editorial):** F102's Carried by and map line add R1.3; the map's app-storage line reads "with the app closed,
      open or after a crash".
      Result: F102's Carried by and map line add R1.3; the map's app-storage line reads "closed, open or after a crash".
- [x] **carried: PRIV-10 (PARTIAL) — four byte cases can be read as closed-only** (T1, UJ1.3-c, UJ4.2-c, UJ6.3-b).
      **covered by PRIV3-1.**
      Result: covered by PRIV3-1, landed.
- [x] **carried: PRIV-12 (PARTIAL) — the app's storage is still "wherever the build puts it", and crash reports are
      read nowhere** (R8.10e; the Harness; the Test-controls map). **fix (F53, F130 — testability):** the Harness and
      the map name the app's storage by location under either sandbox reading — its container when sandboxed; its
      Application Support, Caches, Preferences and Saved Application State entries otherwise — with its crash
      reports; R8.10e adds "crash reports" (+2 words), paid by R8.10's rule-free purpose clause "to assert what each
      action left there" (−7).
      Result: the Harness and the map name the app's storage by location under either sandbox reading, with its crash reports; R8.10e adds "crash reports" (+2), paid by R8.10's purpose clause (−7).

## peer-product-marketing-manager-reviewer (Claude route)

- [x] **N3-M1 (Major) — DF E34's Get Info pointer assumes a permission model ADR-0007 has not chosen, and fixes
      neither a privacy-setting nor a sandbox loss** (DF E34; R8.8, R8.10a; DF R1.10, DF R7.6p; the ADR-0007 row).
      **needs owner (4 — D, asked separately; log row 2).**
      Result: landed under F138, as PM3-8's result records.
- [x] **N3-m1 (Minor) — E6 "elsewhere" describes the queue F100 turned down, and "running" contradicts "active, paused
      or halted"** (E6 "elsewhere"). **covered by PM3-7.**
      Result: covered by PM3-7, landed.
- [x] **N3-m2 (Minor) — if R1.7 is deferred, E6 never names the refused action** (R8.3, E6 "full"; R1.7, OQ 10).
      **covered by PM3-2.**
      Result: covered by PM3-2, landed.
- [x] **N3-m3 (Minor) — E8's undo window ends on a column hide the reader can't see coming** (E8; R4.7, R2.10;
      UJ6.4-l). **covered by PM3-1.**
      Result: covered by PM3-1, landed.
- [x] **N3-m4 (Minor) — the not-compared reasons omit the viewing angle, and E9 makes an unscanned swatch sound like a
      condition fault** (E9 body and "none" variant; the copy file's R5.4 distance line; R3.7, R5.4). **fix (F16,
      F27, F114 — copy honesty to R3.7 and R5.4):** E9: "⟨excluded⟩ swatches weren't compared: they have no colour, or
      none under their collection's measurement condition, or theirs was worked out for a different light, viewing
      angle or measurement condition"; the R5.4 line: "Not compared — worked out for a different light, viewing angle
      or measurement condition".
      Result: E9's body and "none" variant and the History lines' not-compared line read the box's wording.
- [x] **N3-n1 (Nit) — E34's "OK" silently discards the change, and "nothing in your file changed" is unhedged** (DF
      E34; DF R1.7). **needs owner (4 — D, asked separately).**
      Result: landed under F138: "nothing in your file changed" becomes "On a local disk your file is exactly as it was before it", as E15 says; F138's chosen text carries no "OK leaves it unsaved" clause, so OK's effect is not restated (reported).
- [x] **N3-n2 (Nit) — the copy file's sibling-owned list omits DF E34** (the copy file's States preamble). **covered
      by R3-IF-8.**
      Result: covered by R3-IF-8, landed.
- [x] *unrated* **PMM Missing (optional) — once ADR-0007 lands, add E34 to an F134-style owner reading** (DF E34; the
      ADR-0007 row; the dogfood line beside M4). **needs owner (16).**
      Result: landed under F150 as the ADR-0007 row's second input, recorded in DF F54.

PMM's ungraded pointer in its Missing list (E10's "Undo" elsewhere, R8.3 against R8.1g) is PM3-4's defect.

## peer-architecture-reviewer (Claude route)

- [x] **ARCH3-1 (Major) — E10's "Undo" of a selection or collection delete elsewhere is the one bulk write still
      allowed in flight, against R8.11 and the capture PRD's F72 mirror** (R8.3, R8.1g, R8.8, R8.11; Capture F72 and
      its obligations line; UJ4.4-g, UJ9.5-g). **covered by PM3-4** (2 — B); its capture mirror, dated F100 line and
      case land with the answer.
      Result: covered by PM3-4, landed; UJ1.3-g is its case and Capture F73 its mirror.
- [x] **ARCH3-2 (Minor) — R4.7's exemption depends on how capture groups its writes into transactions, and E8
      promises more** (R4.7, E8; the capture PRD's R3.6, R5.8, R6.2, R6.8; UJ6.4-m). **covered by PM3-1.**
      Result: covered by PM3-1, landed.
- [x] **ARCH3-3 (Minor) — F103's ADR-0003 input omits the conditions that decide index or scan: the first keystroke
      and the cold first rows** (R8.2, R8.1d, R3.1; the ADR-0003 row; DF F53; UJ9.5-b). **fix (log row 8; F103, F87,
      F88 — hand-off; no body words):** on the orchestrator's say-so, the ADR-0003 input reads "…either one under that
      byte rule, answering every keystroke from the first character within BROWSE_RESPONSE_BUDGET from the first rows
      of a cold open, and within R8.1f's, R8.1g's and R8.2's budgets on OQ 1's Mac (R8.2, R8.1d)" — PERF3-1's budget
      clause folded in — citing the performance lens's round-3 measurements in the review log as evidence (its probes
      sit in a session scratchpad, outside the repo); the same sentence joins DF F53's search input in a dated line.
      It mandates neither an index nor a scan (PERF3-1's caution).
      Result: the ADR-0003 row's search input reads "…an index kept in the file, or a scan (Data Foundation R1.1), either one under that byte rule, answering every keystroke from the first character within BROWSE_RESPONSE_BUDGET from the first rows of a cold open, and within Collection Mode R8.1f's, R8.1g's and R8.2's budgets on its OQ 1 Mac (its R8.2, R8.1d)", mandating neither and citing the review log's round-3 measurements; the same sentence joins DF F53 (5) in a dated line.
- [x] **ARCH3-4 (Minor) — F102 makes the erasure moment wait on readers of the file, and no row says who waits**
      (R8.8, R8.11, R8.3; DF R1.5, DF R2.3; the ADR-0003 row). **needs owner (5).**
      Result: landed under F139: R8.1's lead reads "or a capture save from when it lands" and R8.1c's cell drops "from when the write lands" (net −3 words); the ADR-0003 input and DF R1.5's help-docs line under DF F54; UJ9.5-d's second run loads All items during the edits.
- [x] **ARCH3-5 (Minor) — the stored, exported gamut flag now depends on an open question in this PRD** (R2.5, OQ 7;
      DF R3.4, DF F53; the export PRD's R2.1). **needs owner (12).**
      Result: landed under F146: DF R3.4 states the test itself under DF F54; OQ 7's Decision cell swaps its F40/F64 sentence and cites DF R3.4 (−3 words); the ADR-0003 row's flag input follows.
- [x] **ARCH3-6 (Minor) — DF E34's advice assumes a POSIX cause while ADR-0007 is open** (DF E34; Build dependencies
      row 1). **covered by N3-M1.**
      Result: covered by N3-M1, landed.
- [x] **ARCH3-7 (Minor) — UJ9.4-e's oracle is broader than R8.10 and pre-empts ADR-0007 and ADR-0008** (UJ9.4-e;
      R8.10). **covered by PRIV3-5.**
      Result: covered by PRIV3-5, landed.
- [x] **NIT-1 (Nit) — DF R6.2 and R6.2d still say "unrecoverable from the active file"** (DF R6.2, DF R6.2d). **fix
      (F102 — editorial; a dated line under DF F53):** each reads "as R6.2a states".
      Result: DF R6.2 and R6.2d read "as R6.2a states", under DF F54 with a dated line under DF F53.
- [x] **NIT-2 (Nit) — the Harness reads "every file beside it", wider than the Vocabulary, so an import CSV in the
      folder would false-match** (the Harness; UJ6.4-f, UJ6.4-g). **covered by PRIV3-4.**
      Result: covered by PRIV3-4, landed.
- [x] **carried: A18 (PARTIAL) — E10's "Undo" of a multi-item delete elsewhere** (R8.3). **covered by ARCH3-1**
      (PM3-4).
      Result: covered by ARCH3-1, landed.
- [x] **carried: A22 (PARTIAL) — session writes other than a save or a start end the history** (R4.7, E8). **covered
      by ARCH3-2** (PM3-1).
      Result: covered by ARCH3-2, landed.

## peer-performance-reviewer (Claude route)

- [x] **PERF3-1 (Major) — ADR-0003's "an index or a scan, either one" invites the stock full-text index, measured at
      ~39 s per 1,000 deleted items under F102 and unable to answer 1–2-character searches** (the ADR-0003 row, the
      Outbound DF row; R8.2, R8.1f, R8.1g; UJ9.5-b, UJ9.5-g; F103). **covered by ARCH3-3** (log row 8).
      Result: covered by ARCH3-3, landed.
- [x] **PERF3-2 (Major) — F100 refuses one way only: a session can start while a bulk write holds the writer for up
      to 10 s** (R8.3, R8.11, R8.1g; Capture F72, the capture PRD's R3.1, R3.5, R3.10; UJ9.5-g). **covered by 3MJ2**
      (3 — C).
      Result: covered by 3MJ2, landed.
- [x] **PERF3-3 (Minor; carried PERF2-5) — the first-rows probe types a prefix the codes answer, so a lazily loaded
      imported-value corpus passes** (R8.1d, R8.2, R3.1; UJ9.5-a, UJ9.5-b; the Timing workload). **fix (F88, F103 —
      testability):** at each first-rows frame the case also types the workload's imported-value text and asserts it
      lists exactly the items holding that text within BROWSE_RESPONSE_BUDGET.
      Result: UJ9.5-a and UJ9.5-b type, at each first-rows frame, the all-match prefix and then the imported-value text, which lists exactly the items holding it within the budget.
- [x] **PERF3-4 (Minor) — Page Down paging never exercises the continuous scrolling F41 budgets** (R8.1e,
      DROPPED_FRAME_SHARE; the Timing workload; UJ9.5-a; F41, F66). **fix (F41 — testability; F66's "paging"
      kept):** the workload pages again as a continuous scroll at one screenful per 100 ms, UJ9.5-a doing both in the
      table and in the grid.
      Result: the Timing workload pages again as a continuous scroll at one screenful per 100 ms; UJ9.5-a does both, in the table and in the grid.
- [x] **PERF3-5 (Minor) — M1's population disagrees with UJ9.5-a and with R8.1's file** (M1; UJ9.5-a; R8.1). The
      counts: **covered by R3-m7**; M1's wording: **fix (F41, F129 — editorial; M1 cites its worked oracle):**
      "Population: each kind as UJ9.5-a delivers it, in a Release build on OQ 1's interim Mac, warm." (−14 words).
      Result: M1's Population reads PERF3-5's sentence (−14); its Start names a capture save (+1).
- [x] **PERF3-6 (Minor) — R8.1g budgets E10's undo of a ceiling delete, but nothing sizes what the undo holds while
      F102 keeps the bytes clean** (R8.1g, R1.7, OQ 10; DF R6.2b, DF OQ 20). **needs owner (13).**
      Result: landed under F147: DF OQ 20's question adds what an undoable ceiling delete holds, in a dated line under DF F50 (DF F54); R1.7 stays gated and no row here changes.
- [x] **PERF3-7 (Minor) — under F102 a removing write can't land while an older read is open, and R8.1c's clock
      starts only when it lands** (R8.8, R8.1c, R8.11; F100, F102; UJ9.5-d). **covered by ARCH3-4** (5); its UJ9.5-d
      run with All items cold-loading lands with the answer. The reviewer grades it Major if the export PRD lets an
      export run while a session is in flight.
      Result: covered by ARCH3-4, landed with its UJ9.5-d run.
- [x] **PERF3-8 (Nit) — HISTORY_READINGS_CEILING closes by a timing run, which shows 500 readings are met, not that
      500 is enough** (the constants table; OQ 1). **needs owner (14).**
      Result: landed under F148: HISTORY_READINGS_CEILING's closure evidence reads F148's text (+10 words, not +6, F148 adding "a real item reaches"); dated line under F118.
- [x] **PERF3-9 (Nit) — UJ9.5-g's delete run doesn't say where its inputs land** (UJ9.5-g; R8.1f). **covered by
      3MJ2.**
      Result: covered by 3MJ2, landed.
- [x] **carried: PERF2-5 (PARTIAL) — the first-rows probe can't see a lazily loaded imported-value corpus** (R8.2,
      UJ9.5-a, UJ9.5-b). **covered by PERF3-3.**
      Result: covered by PERF3-3, landed.

**Owner decisions, 2026-09-25.** Items 1–4 were answered as fences F135–F138 (1 → F135, 2 → F136, 3 → F137, 4 → F138), consistent with the recommendations below. Items 5–16 are fences F139–F150 (item n is F(134+n)), approved as recommended — except item 15, which the owner approved as stated to them: an import commit also waits while any capture session is running, an Import PRD amendment in this change (F149 governs).

## Owner-needed

Each item: the question, the boxes it answers, and a one-line recommended answer. Items 1–4 are the log's four "owner
decision" rows (1, 3, 5 and 2), which the orchestrator is putting to the owner separately. An answer becomes its own
dated fence (F135 on), with a dated Clarified line under any earlier fence it refines. Items 5–13 are the ones the
orchestrator named; 14–16 are added here because the strict rule reaches them (a closure gate, a sibling rule, a new
owner check).

1. **(A) Which session writes end "Undo change" history, and E8's wording** *(asked separately)* — PM3-1, ARCH3-2,
   3MN4, N3-m3, R3-IF-5; carried PM2-1, PM2-2, 2MN5, A22. *Recommend:* no write a capture session makes ends it —
   R4.7's "a capture save or a session starting not" becomes "no write a capture session makes counting"
   (−1 word, whatever capture's transaction layout) — and E8 names actions, not what changed: "…until the file
   closes or is read again, or you do anything but edit swatches' details, codes or names or rename a column or
   collection — importing and hiding a column included; scanning doesn't count", with a UJ6.4 case (edit, then Skip,
   Flag and End session; "Undo change" still offered).
2. **(B) E10's "Undo", and an "Undo change" of a bulk set or clear, refused on any collection in flight** *(asked
   separately)* — PM3-4, 3MN1, R3-M1, R3-IF-4, ARCH3-1; carried A18. *Recommend:* yes, extending F100 — R8.3 moves
   E10's "Undo" to its any-collection group and adds "or an "Undo change" of a bulk set or clear" (+9 body words); the
   capture PRD's F72 and obligations line follow in a dated line under Capture F72; the two cases R3-M1 names; E6
   "elsewhere" worded to cover undos.
3. **(C) A session starting or resuming while a bulk write or delete runs** *(asked separately)* — 3MJ2, PERF3-2;
   carried 2MJ2. *Recommend:* refuse rather than queue, as F100 does — while a bulk write or delete this PRD makes
   runs, no session starts or resumes and no other write this PRD offers starts, each shown disabled until it lands —
   carried by a capture PRD row (Capture F73, a dated line under Capture F72) and this PRD's Inbound line (about +6
   body words; about +30 if R8.3 carries it instead); UJ9.5-g fires a start mid-delete. Item 15 asks whether Import
   follows.
4. **(D) The Data Foundation PRD's E34 recovery wording while ADR-0007 is open** *(asked separately)* — N3-M1,
   ARCH3-6, PM3-8, N3-n1. *Recommend:* copy that holds under either ADR-0007 reading — "…so the last thing you did
   wasn't saved. Check that the file isn't locked and that SpectroCapture is still allowed to change it, or choose the
   file again, then try again; OK leaves it unsaved." — with a choose-the-file action through DF R7.3j (the route E15
   offers), "nothing in your file changed" hedged "on a local disk" as E15 is, and an ADR-0007 row input naming where
   access is restored once 0007 picks the model; DF copy and a dated line under DF F53 (DF F54 if the action is new);
   no body words.
5. **Removing writes against open readers** — ARCH3-4, PERF3-7. *Recommend:* capture saves never wait on a removing
   edit — an ADR-0003 input that a text-removing write lands once every earlier read has ended, this app's own reads
   (an export, All items' cold load) running in transactions no longer than BROWSE_RESPONSE_BUDGET; an outside
   reader's hold leaves the edit shown not yet saved, and DF R1.5's help-docs line says reading the file elsewhere
   during capture may delay saves; R8.1c times a user's own write from the input (+8 body words); the UJ9.5-d run
   PERF3-7 names.
6. **Which E6 variant renders when two apply** — 3MN2, R3-IF-2, R3-n4. *Recommend:* an in-flight session wins — the
   body, "full" or "elsewhere" — and "interrupted" renders only while no session is in flight, because resuming the
   interrupted session is refused while another is active; the copy's "interrupted" condition adds "and no session is
   in flight" (no body words), R8.3's "beside" reworded word-neutrally, and a UJ1.2 case for each collision.
7. **E6's "full" gate if R1.7 is deferred** — PM3-2, 3MN3, R3-IF-3, N3-m2. *Recommend:* "full" renders once every P1
   action it names lands, E10's "Undo" aside, and drops "and undoing a delete" (the headline covers a delete made
   before the session); one gate, "every P1 action that variant names", in R8.3 (+3 words), the copy and the Harness;
   a dated line under F111.
8. **How a mismatched non-spectral reading counts toward the All items like pair** — 3MN5, R3-m10; carried 2MN3.
   *Recommend:* it doesn't count — F113 makes it never like, so the majority counts only readings that can be like
   ("…the most common among listed items with a value, a mismatched non-spectral reading not counting", about +6
   words); R3-m10's inks case plus a second fixture, because the first cannot tell "not counting" from "counting
   under its collection's pair".
9. **What Return fires in E8** — 3N2. *Recommend:* "Cancel", as in every delete confirmation (R1.6) — E8 exists to stop
   an accidental change to many swatches, and a second press of Return would otherwise apply it unread; R6.2 gains
   "in E8 Return fires "Cancel"" (about +6 words) and a UJ6.2 case.
10. **E9 after a result opened from it is deleted, flagged, restored or answered** — PM3-3, 3N3. *Recommend:* on
    return E9 is worked out again for the same source — a deleted result gone, a changed one at its new distance or
    no longer listed — and deleting a result opened from E9 returns to E9, the table showing only if the source
    itself is gone (about +8 words in R4.1); a case deletes FS-002 from E9 and one restores a result.
11. **"Undo change" and the All items view** — PM3-5. *Recommend:* the All items view doesn't offer it, E13 unchanged;
    an edit made from it is undone in the item detail or on its collection's surface, R4.7's "offered only where its
    latest change's collection is shown" adding "never in the All items view" (+5 body words).
12. **Who owns the stored gamut flag's test parameters** — ARCH3-5. *Recommend:* the Data Foundation PRD — its R3.4
    states the test itself (Bradford to the display white, relative colorimetric, zero tolerance, the adaptation its
    sRGB derivation uses) under DF F54, closing this PRD's OQ 7 changing DF R3.4 only through a DF fence and a new
    derivation version; OQ 7's Decision cell swaps its F40/F64 provenance sentence for that (word-neutral) and cites
    DF for the sRGB case; the ADR-0003 input follows.
13. **What an undoable ceiling delete holds** — PERF3-6. *Recommend:* yes — the Data Foundation PRD's OQ 20 question
    adds "and what an undoable delete of ROWS_CEILING items holds in memory, or writes when the window ends, on OQ 1's
    Mac", in a dated line under DF F50 as IF-10's was; R1.7 stays gated and no row changes.
14. **HISTORY_READINGS_CEILING's closure evidence** — PERF3-8. *Recommend:* yes — "OQ 1 — the owner's estimate of the
    longest history a real item reaches, timed by UJ9.5-e", as FILE_ITEMS_CEILING closes by estimate (+6 body words
    in the constants table); a dated line under F118.
15. **An import commit while a session is in flight** — 3MJ2's Import half; SSE question 14 (deferred to
    architecture, ungraded). *Recommend:* decide with 3 (C) — if C refuses a session start during a bulk write, ask the
    import PRD's owner whether an import commit likewise waits while a session is in flight on another collection,
    landing as Import F66 only on that answer; nothing here changes.
16. **E34 in an owner reading once ADR-0007 lands** — PMM *unrated* (optional). *Recommend:* yes, as an input on the
    ADR-0007 row — revoke access the way the chosen model allows, follow E34 and confirm it recovers — decided with 4
    (D); no row changes.

## Checks

- [x] **Testability pairing (process rule 3).** Every row, state and metric changed above has its acceptance case and
      its verification-seam line (R8.10, the Harness, the named defaults, the Test-controls map) updated in the same
      pass, and every owner answer that lands brings its case. Expected new cases: R3-m2 (two), R3-m3, R3-m7 (one),
      R3-IF-6, PRIV3-2 (b), R3-m5's control runs, and those of answers 1, 2 (two), 3, 5, 6 (two), 8 (two), 9 and 10
      (two); readback grants R8.10c (R3-m1) and R8.10e (PRIV-12) each paired with its cases and map line.
      Result: every changed row, state and metric has its case and seam line; new cases UJ1.2-g/h, UJ1.3-g, UJ3.4-j/k, UJ4.4-i/j, UJ6.2-l, UJ6.3-g, UJ6.4-o/p, UJ7.1-o/p/q/r and UJ9.1-m/n/o/p, UJ9.8-c's control runs, and the Harness's Phases and builds paragraph; the readback grants R8.10c and R8.10e each with its cases and map line; sibling cases Capture T7, a DF DJ3 line and an Import UJ 2.1 line.
- [x] **Word count** of the PRD body by rule 14's method (budget 12,000; start 12,000, check 12 re-run at
      `cd84fd4`). The fixes needing no owner answer net about −14: 3MJ1 −4, R3-m1 +5, R3-m6 −1, R3-IF-3 +1, R3-IF-6
      +1, R3-IF-7 −2, PERF3-5 −14; 3N1 and R3-IF-10 word-neutral; optional R3-IF-9 (R4.9) −5 and PRIV-12 −5. The
      owner answers could add: 1 −1; 2 +9; 3 about +6 (or +30); 5 +8; 6 up to +1; 7 +3; 8 about +6; 9 about +6; 10
      about +8; 11 +5; 14 +6; the rest none. Each Result note that adds body words names its trim from the candidate
      list; report the final count; flag any fix that cannot be paid for, and any template-scaffolded paragraph
      trimmed without the orchestrator's confirmation.
      Result: 11,996 by rule 14's method (from 12,000). Paid for by trimming rule-free prose: the Build contract's last sentence (−19) and its two provenance parentheticals (−7); the Build dependencies paragraph's "(F65)" and "(F124)" (−2); OQ 1's status clause (−6); R8.10's purpose clause (−7, PRIV-12); R3.9's "now" (−1, R3-IF-6); and the sibling-fence provenance parentheticals in the Outbound and Inbound cells (−43), the category round 2 trimmed; with the net-negative fixes (3MJ1, R3-m6, R3-IF-7, PERF3-5, OQ 7's swap, R3-IF-9's R4.9 half). The §8 preamble, the Open-questions trailer and R1.7 were not trimmed. No box was held back. Companions, unbudgeted: journeys 17,834 (from 15,936), copy 3,469 (from 3,432), fences 17,452 (from 16,725), OQ results 350.
- [x] **Fences and Carried-by.** Each owner answer becomes a dated fence from F135, in the fence file's grammar, its
      Carried by and fence → row map line filled with actual IDs; an answer refining an earlier fence adds a dated
      Clarified line under it (e.g. F104 and F29 by 1 and 11; F100, F14 and F30 by 2 and 3; F102 and F35 by 5; F111
      and F97 by 6 and 7; F113, F54 and F68 by 8; F117 by 9; F107 and F46 by 10; F127 and F40 by 12; F118 by 14). F118's
      Carried by and map line add R8.1b (3MJ1); F102's add R1.3 (PRIV3-8). Traceability's "F1–F134" follows. No
      placeholder left.
      Result: F135–F150's Carried by and map lines are filled with IDs (F138, F146, F147 and F150 by Data Foundation IDs and fences, F149 by Import IDs); dated Clarified lines under F14, F29, F30, F35, F40, F46, F54, F68, F97, F102, F104, F107, F111, F113, F117, F118 and F127 (F100, F101 and F104 already carried the owner record's D28–D31 lines); F118 adds R8.1b and F102 adds R1.3; Traceability reads F1–F150; no placeholder left.
- [x] **Sibling halves (process rule 12).** Next free sibling fences, verified at `cd84fd4`: DF F54, Capture F73,
      Import F66, Export F32, Device F33; this PRD's own next is F135. A half inside an existing sibling fence's
      decision lands as a dated Clarified line under it; a new decision takes the next free number, its Authority
      citing this PRD's fence; each status-line clause is extended, peer review pending. This pass: DF — NIT-1,
      PRIV3-4 and ARCH3-3 (dated lines under DF F53); Capture — R3-M2's UJ3.3-l (a dated line under Capture F71).
      Owner-dependent: DF (4 under DF F53 or as DF F54; 5's help-docs line; 12 as DF F54; 13 under DF F50), Capture
      (2 under Capture F72; 3 as Capture F73 with a line under F72), Import (15 as Import F66).
      Result: DF F54 (new, next free) with dated lines under DF F50 and F53 and its status-line clause; Capture F73 (new) with dated lines under Capture F71 and F72, R3.5 amended with alignment kept, T7 added, Traceability F1–F73 and its status-line clause; Import F66 (new) with R3.2 amended with alignment kept, E40's another-collection variant, a UJ 2.1 case line, its map line and status-line clause. Export and Device unchanged.
- [x] **ADR queue and post-lock.** `docs/decisions/README.md`'s 0003 row (ARCH3-3 with PERF3-1, PRIV3-2, PRIV3-4;
      answers 5 and 12) and 0007 row (answers 4 and 16) are edited only on the orchestrator's say-so, as F98
      authorized in round 1b — report. `docs/product/post-lock.md` changes only if an answer adds a line.
      Result: ADR-0003 row: eight inputs — F139's removing-write input added, the byte rule's scope "the app keeps", the flag by DF R3.4's own test, and the search input as ARCH3-3, PERF3-1 and PRIV3-2 ask, citing the review log. ADR-0007 row: F138's and F150's inputs. post-lock.md unchanged: no answer added a line.
- [x] **Constants and OQs.** 3MJ1 moves HISTORY_READINGS_CEILING's owning row to R8.1b in the constants table; answer
      14 sets its closure evidence; answer 12 rewrites OQ 7's Decision cell; OQ 1's Interim-stated line is unchanged
      and the no-interim list stays None, derived from the Open questions table.
      Result: HISTORY_READINGS_CEILING's owning row is R8.1b and its closure evidence F148's; OQ 7's Decision cell rewritten; OQ 1's Decision cell trimmed of its status clause only, its Interim-stated line unchanged; the no-interim list stays None.
- [x] **Traceability.** New case IDs are assigned once, from each journey's next free letter (after UJ1.2-f, UJ4.4-h,
      UJ6.2-k, UJ6.4-n, UJ9.1-l, UJ9.8-c and so on); nothing is renumbered.
      Result: new case IDs take each journey's next free letter; nothing renumbered.
- [x] **Standing label check** (check 13) re-run after the copy changes: E6 "elsewhere" (PM3-7), E9's body and "none"
      variant and the History lines' not-compared line (N3-m4), the sibling-owned list (R3-IF-8), the journeys'
      sibling cites (R3-IF-9), R8.10b's "Cause column" (R3-IF-10), and any label an owner answer adds or changes (1
      and 9 — E8; 4 — DF E34; 6 and 7 — E6; 11 — E13).
      Result: run after the copy changes: direction 2 prints nothing, every quoted string in a row or case being a copy label; no label was added or renamed here — E6's, E8's and E9's variant names are unchanged, and DF E34's new action is written in the Data Foundation copy file.

## Out of scope for this pass

- `docs/product/README.md` (IF-16's index status) — at the bookkeeping close.
- Deleting guidance comments — at lock.
- The post-lock tick for this PRD's item — when the PR exists.
- Any change a fence above does not authorize, and every **needs owner** item until the owner answers it.
