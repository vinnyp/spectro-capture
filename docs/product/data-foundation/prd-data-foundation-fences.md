# Data Foundation PRD — owner decisions (fences)

Owner decisions on this PRD. A fence is settled: reviewers do not re-litigate it, and a row that names one does so for provenance only. Owner-locked rows: none yet. Review log: `../../agent-reviews/<date-of-round-1>-prd-data-foundation-peer-reviews.md` (recorded at round 1).

## Phase 0 — research inventory (2026-09-09)

Ingested before any round: every brief under `docs/briefs/` on `main` at `dd43c13`; the browsing-a-collection-at-scale research results v2 (brought into `docs/briefs/` from the foundry in this PR; supersedes the first-pass results on any point where they conflict); the X-Rite SDK and open-source prior-art findings (brought in the same way; CxF and export shapes). The three locked PRDs and their obligations tables. Deferred by the owner: the Spectro 2 software competitive brief (positioning, not data). No project-supplied prior-art query command.

### F1 — Data Foundation is a slim data contract, not a schema document (2026-09-09)

**Decision:** This PRD states what the user-owned file guarantees: what is kept and never destroyed, what "canonical value" and version history mean to someone reading the file, which derived spaces are present and what the gamut-clipped flag asserts, what the CSV export contains, what a schema migration does to a file a user already has, what the user may delete, and the obligations the capture-mode, import, and device-management PRDs already impose on Data Foundation, gathered here by citation. The storage schema, blob layout, library choice, and migration mechanism are out of scope: they belong to ADR-0003 and a technical spec that follows this document. Word budget for the PRD body: 4,000 words. Shape: the capture PRD's three-file shape plus this fence file and an OQ results file; no author line; row IDs `R<section>.<n>`, `E<n>`, `M<n>`; every requirement row at most two sentences.

**Why:** U5 and U6 are promises to users about the data itself, and the data-consumer persona has no other PRD; the rest is engineering, and reviewing schema through product lenses produces the "solution smuggled into a requirement" findings the gate exists to reject. Everything this PRD can inherit it cites rather than restates.

## Fence → row map

(filled as fences land)

## Rejected findings

(none yet)
