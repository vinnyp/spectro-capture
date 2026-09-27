# Nix Toolkit import amendment — round 8 fixes

Resume point for round 8, the confirmation of f6860b5 by all nine lenses (review log, Round 8). Every lens aligned on every row it reviews, with no Blocker, Major or Minor. One box per Nit. Lens prefixes: PM-R8, SSE8, IF8, PL8 (plan; its review numbers them R8-*), and the other lenses, which raised nothing. Each fix below is editorial: it changes no row's meaning, the orchestrator verified the diff word for word, and every row keeps its alignment (process rule 6).

## Nits, fixed editorially

- [x] **R3.8k's "each"** — PM-R8-n2. "R3.6 field changes, each field change with the stored value it replaces" limits the clause to field changes, as the round-7 fix file intended.
- [x] **`Finish` absent in the column-rename case** — PL8 R8-n1, SSE8-n1, SSE8 question 1. TK-1–TK-3 are "all pending with `Finish` absent", using the Import Vocabulary's term. That is the only reading the case's counts assertion admits.
- [x] **The target, not the items, has no imported columns** — SSE8-n1(b), SSE8 question 2. The rename case with a captured `Sample Rust` reads "each with a live current reading, the target having no imported columns".
- [x] **E40's label in UJ 2.1** — IF8-n1. The E40 case's action reads "Go to the session", as the copy and R3.8h do.
- [x] **F67's round-7 line** — IF8-n2. It credits "Review again" to the source-change case alone.
- [x] **Export F35's "Peer review pending."** — PL8 R8-n2. Closed by the bookkeeping close's dated Closed line under F35.

## Recorded without change

- [x] **E13's choices sentence on routes without E14** — PM-R8-n1. The sentence is true but says nothing on a pending-only route. Changing copy is not editorial, so the rewording goes on the post-lock list as a copy choice.
- [x] **ZZ-1's own value after the column rename** — DB8 optional polish. An added Assert is not editorial, and R6.8e's absent-item cases already assert that absent items are preserved. It goes on the post-lock list for build review.
- [x] **Rewriting 3ae25c1's earlier log wording out of history** — PRIV8 optional. The branch is unpushed. Whether to rewrite before the first push is the owner's call, weighed against the review log's citations of the round subjects' commit IDs.
