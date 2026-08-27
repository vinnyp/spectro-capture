# Contributing to SpectroCapture

Thanks for your interest. Please read this whole document before opening a PR — the project is at an early stage and the rules here reflect that.

## Where the project is

SpectroCapture is currently **design, not code**. The repository holds a product vision and three research briefs (with results) that feed the app-architecture decisions still to be made. No source code exists yet, and no architecture ADRs have been written.

**Code contributions will open once the architecture ADRs land.** Until then, the most useful way to contribute is to:
- Read [`docs/product/vision.md`](docs/product/vision.md) and the research in [`docs/briefs/`](docs/briefs)
- Open a GitHub Issue or Discussion if you spot a gap, a contradiction, or a question the research didn't answer
- Weigh in on open questions once they're posted for discussion (see [AGENTS.md](AGENTS.md) §3 for the current Decided / Recommended / Open list)

If you show up with a PR that adds source code before an architecture ADR exists for the area it touches, expect it to be redirected to a design discussion first, not merged.

## Ground rules

These apply to every contribution, code or otherwise. See [AGENTS.md](AGENTS.md) §4 for the full list; the ones most likely to bite a contributor:

- **Never commit vendor SDK binaries, headers, or license keys.** The instrument SDK is a licensed dependency pulled at build time, not something this repository can distribute. If your change needs the SDK, add it as a package dependency — don't vendor any part of it.
- **No cloud features.** This is an offline-first, local-first app. A local SQLite file the user owns is the entire sync strategy — don't propose adding a server, an account system, or a sync service.

## Workflow

1. Fork the repository (or branch directly if you have write access).
2. Create a branch for your change.
3. Open a PR against `main`. Keep it to **one logical change per PR** — don't bundle a docs fix with an unrelated proposal.
4. A maintainer reviews it. Expect discussion before merge, especially for anything touching scope or architecture.
5. Once approved, a maintainer merges it. Nobody pushes directly to `main`.

Once code exists: any PR that touches device-facing code should carry the `needs-hardware-verify` label. That label is **proposed, not yet created** — if you open the first such PR, create it.

## Testing without hardware

The recommended architecture (see the app-architecture research results) puts all device access behind a `SpectroDevice` protocol with a `Mock` implementation, specifically so contributors without a Nix Spectro 2 / Spectro L can build, run, and test the app. Once that seam exists:

- The mock device is the default for local development and CI. You should not need real hardware to contribute to most of the app.
- If your change does touch a real-hardware code path, say so explicitly in the PR body, and describe what you verified on a real instrument (or flag that it still needs verification from someone who owns one).
- CI can build the project but cannot exercise any device code path — there's no hardware or license key available to it. Human verification is the only check for that surface.

## Decisions

Proposing an architecture change means writing an **ADR** (Architectural Decision Record) in `docs/decisions/` (this directory doesn't exist yet — the first ADR creates it). Use the Nygard format, one file per decision:

```
docs/decisions/NNNN-title.md
```

with `Context`, `Decision`, and `Consequences` sections. A recommendation in a research brief's results is evidence for an ADR, not a substitute for one — don't cite "the research recommends X" as if it settled the question. See [AGENTS.md](AGENTS.md) §3 for what's currently Decided, Recommended, or Open.

## Commit & PR style

- Commit subject: imperative mood, 72 characters or fewer (`add CSV column mapping`, not `added` or `adds`).
- PR body: what changed, why, and how it was verified — including hardware verification if applicable.

## Code style

Not yet defined. SwiftFormat/SwiftLint configuration will land with the first code PR, once the architecture ADRs settle what the codebase looks like. Don't assume a style convention that isn't written down yet.

## Reporting issues

Use GitHub Issues. If your issue involves a license or SDK problem, describe the symptom — **do not paste your vendor license key** or any other credential into an issue, comment, or log excerpt.
