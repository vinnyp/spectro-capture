---
canonical-id: 8cac21eb-0fa7-4416-86ba-bb3f77651b2a
name: SpectroCapture
last_updated: 2026-09-01
---

# SpectroCapture Strategy

See [docs/product/vision.md](docs/product/vision.md) for scope, personas, journeys, and the feature list; this document carries direction.

## Purpose

Someone with a spectrophotometer and a 200-item collection faces per-scan metadata entry in every existing tool — the workflow is shaped for verifying one sample against a standard, not acquiring a collection, and no price fixes it. Because the instrument's measurements are also stranded in vendor apps, solving the workflow must end in a database the user owns, not another silo.

## Positioning

Acquisition-first is the organizing bet: import the inventory first, then scan heads-down with zero per-item entry — accepting mere parity on verification. Open source and a user-owned local SQLite file are the delivery commitments that make the bet trustworthy and structurally unmatchable by account-model vendors.

## Users

**Primary:** The Cataloger — hiring SpectroCapture to digitize their whole collection accurately, in one sitting, without delay.

**Secondary:** The Data consumer — hiring it to get full-fidelity spectral data out (CSV, direct SQLite) into their own tools. Remaining personas: see vision.md.

## Boundaries

- No QC depth beyond ΔE vs the canonical value — no tolerance presets, pass/fail workflows, or QC reports. Parity-minimum by design.
- No metric-driven shortcuts: calibration gates never move to make a speed number look better.
- Product non-goals (PANTONE/RAL/NCS, iOS, cloud sync, …) live in vision.md's non-goals table.

_Resist a change when:_ it makes SpectroCapture better at verifying than at acquiring, or improves a speed metric at the cost of measurement fidelity.

## Key metrics

All measured by dogfooding real sessions (offline-first product, no telemetry); adoption metrics deliberately start at first public release.

- **Seconds per item, bulk session** — median wall-clock per scanned item across a real session; target: bounded by the device's scan cycle, not the UI.
- **Launch-to-first-scan** — seconds from app open to first successful scan on a known device, with calibration gates intact.
- **Session completion rate** — share of an imported queue completed in one sitting, plus the dead-letter rate.

## Tracks

### Capture experience

The heads-down loop: inventory import, queued scanning, multi-sample averaging, inline failure handling, session flow.

_Why it serves the approach:_ It is the inversion itself — the entire bet made tangible.

### Device & instrument layer

Connect, calibrate, offline pre-authorization, and the SpectroDevice seam with its mock. The seam is open to community instruments at any time (merged when a hardware owner verifies); the core team builds and validates Nix Spectro 2/L only through v1.

_Why it serves the approach:_ Reliable hardware keeps capture heads-down, and the seam is the open-source moat in code form.

### Data foundation

The user-owned SQLite file: canonical raw payloads, version history, derived color spaces, CSV export.

_Why it serves the approach:_ The ownership commitment — acquisition has to land somewhere the user controls, or the inversion just builds another silo.

### Honest collection view

Browsing and plotting with gamut honesty: swatch grid, absolute-space plot, renderable-vs-not marked explicitly.

_Why it serves the approach:_ A collection is only worth acquiring if you can see it truthfully afterward.

## Brand

**One-liner:** Digitize an entire physical color collection as fast as you can physically scan, with measurement-grade fidelity.
