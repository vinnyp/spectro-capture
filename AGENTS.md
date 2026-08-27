# AGENTS.md

Rules for any agent (Claude Code, codex, agy, or otherwise) working in this repo. Terse and imperative. If you want narrative context or a human-facing pitch, read [README.md](README.md) instead.

## 1. What this is

SpectroCapture is an open-source macOS desktop app for **bulk color acquisition**: digitizing an entire physical color collection — a marker set, a swatch book, a product line — as fast as a person can physically scan it, at measurement-grade fidelity. Every existing desktop app for this hardware class is **verification software** (does this sample match the standard?), one scan at a time. SpectroCapture is **acquisition-first**: import an inventory, then scan heads-down through it with no per-item metadata entry between scans. That inversion is the entire product thesis, not a feature among features. The primary persona is the **Cataloger** — someone who wants their whole collection digitized accurately in one sitting, not a QC technician checking one sample against a standard.

## 2. Read before changing anything

Source of truth, in this order:

- [`docs/product/vision.md`](docs/product/vision.md) — what the product is, why it exists, and v1/v2/non-goal scope. Start here.
- [`docs/briefs/`](docs/briefs) — research, not decisions. Each topic is a `*-brief.md` (the questions asked) and a `*-results.md` (the findings). Where a `-results-v2.md` exists, it supersedes the first-pass results on any point where they conflict, and says so explicitly. Three topics so far:
  - acquisition experience — the capture-mode design (queue model, error handling, the seam); has a `-results-v2.md` gap-closure pass
  - browsing a collection at scale — the collection-mode design (data model, rendering, color honesty)
  - macOS SwiftUI app architecture — the chassis: module boundaries, state ownership, concurrency, testability without hardware
- `docs/decisions/` — Architectural Decision Records. **This directory does not exist yet.** It is the only place a recommendation becomes a decision.

**If it isn't in an ADR, it is not decided.** A research brief can recommend an approach with strong evidence behind it; that is still a recommendation, not a commitment, until someone writes the ADR. Do not treat brief language ("recommended," "best practice," "the evidence favors") as settled architecture.

## 3. Decided / Recommended / Open

**Decided** — settled in the product vision, treated as fixed input by every research brief, and not up for re-litigation in code review:
- macOS-only (no iOS, Windows, or Linux)
- SwiftUI
- Local SQLite file, owned by the user, portable and directly queryable
- Offline-first, no server, no cloud sync — the SQLite file is the sync strategy
- Open source from day one
- CSV inventory import in v1
- `AsyncStream` bridge over the vendor SDK

**Recommended, no ADR yet** — the outcome of the architecture research, treat as a strong default rather than a rule until an ADR lands:
- MV over MVVM/TCA for app architecture
- SPM module split around a `SpectroDevice` protocol with `Mock` and `Live` implementations
- GRDB as the SQLite library
- swift-dependencies for the device seam
- Swift 6 strict concurrency on the app target
- Swift Testing for new tests, XCTest retained for UI tests
- Developer ID signing + notarization, Sparkle for updates
- gitleaks + pre-commit + CI with a project-specific licence-key rule
- a `needs-hardware-verify` PR label — proposed, not yet created; whoever opens the first device-touching PR that needs it creates the label

**Open** — no direction yet, don't assume one and don't build as if one exists:
- Sandboxed vs. unsandboxed. The architecture research is explicit that this is "a deliberate decision, not an assumed requirement" — don't default to either.
- Minimum macOS version. Cited APIs across the research span macOS 13.0–15.0; nothing pins a floor.
- The capture → collection "seam" — the two capture-mode research passes disagree on this. The first pass recommended capture as a full-window modal takeover that hands off to the collection when the session ends; the v2 pass reverses that, recommends writing directly into the live collection, and says explicitly it should be re-opened before the ADR is written. Do not implement either version's proposal as if it were settled.
- Mobile-app data export / CxF migration path — called out in the vision as an open research gap.
- The blob and derived-value schema for stored measurements.

## 4. Non-negotiables

- Local-first. The user owns the database file.
- No per-item metadata entry during capture — capture stays heads-down.
- Corrections never destroy data. A re-scan updates the canonical value; prior values persist as version history.
- Gamut honesty. Mark clipped / non-renderable colors explicitly. Never silently substitute the "closest color" as if it were faithful.
- One device at a time. The vendor SDK is architecturally single-device, single-session — do not design around concurrent devices.
- No PANTONE/RAL/NCS or other color-library matching.
- No iOS.

## 5. Hardware & the public-repo boundary

The v1 instrument family is the Nix Spectro 2 / Spectro L (Nix Sensor), connected over BLE or USB. The vendor SDK is a **package dependency, never vendored**.

- No SDK binaries, headers, or licence keys go in this repo, ever, in any commit.
- CI can build this project. CI **cannot** exercise any device code path — there is no hardware or licence key available to it.
- Every PR that touches device-facing code needs a human with real hardware to verify it before merge.
- All device access must go through the `SpectroDevice` seam so that everything above it is testable against the mock implementation without hardware. If you're writing code that talks to the instrument directly instead of through that seam, stop and reconsider.

## 6. How work lands

Worktree → branch → PR → attended merge. Never commit or push directly to `main` — there is no "tiny fix" exception; a one-character typo goes through a PR too. Commit messages are imperative ("add X", not "added X" or "adds X"). If a PR touches device code, say in the PR body what needs to be verified on real hardware. See [CONTRIBUTING.md](CONTRIBUTING.md) for the full human-facing procedure — this file doesn't duplicate it.

## 7. Repo map

There is no code yet. Current tree:

```
docs/
  product/vision.md          — product vision, scope, personas, journeys
  briefs/                    — research briefs + results (see §2)
docs/decisions/               — ADRs (planned, not yet created)
.compound-engineering/        — Compound Engineering config; docs/ is the CE artifact root
LICENSE                       — MIT
```

Where source code lands, and how it's modularized, is not yet decided beyond the SPM-split recommendation in §3 — see the architecture research results for the reasoning. Don't invent paths beyond what's written there. If you are asked to write the first code and no ADR covers module layout, stop and propose the ADR (or ask) rather than picking a layout on your own.

## 8. Vocabulary

- **Capture mode** — heads-down, hardware-paced scanning through a queue.
- **Collection mode** — browsing, searching, and editing after capture.
- **The seam** — the boundary between capture mode and collection mode (see §3, open).
- **Cataloger** — the primary persona: someone digitizing their whole collection in one sitting.
- **Canonical value** — an item's current authoritative measurement, which is its raw payload (not a derived value); a re-scan supersedes it into version history, never overwrites it, and QC scans compare against it.
- **Raw payload** — the instrument's raw measurement bytes. This is what the store keeps as canonical; Lab/XYZ/LCh/Luv/sRGB/HSL are derived from it.
- **Multi-sample averaging** — 1–5 readings of one item averaged into a single value.
- **ΔE / ΔE2000** — the color-difference metric used for QC comparisons.
- **Gamut-clipped** — the flag on a derived sRGB value that fell outside the display gamut.
- **Dead-letter / deferred-error queue** — failed scan items set aside and resolved at the end of a session, not mid-queue.
- **Pre-authorization window** — the offline licence-validity period for the vendor SDK.
- **Version history** — prior measurements kept, never destroyed, after a correction.

## 9. Agent-specific notes

- **Claude Code**: `CLAUDE.md` at the repo root imports this file (`@AGENTS.md`) — it is not a separate set of rules. The Compound Engineering artifact root is `docs/` (`.compound-engineering/config.yaml`). `.claude/settings.local.json`, if present, is untracked local config — don't assume its contents apply to other agents or other machines.
- **Codex / agy**: read this file natively; no import step needed.
