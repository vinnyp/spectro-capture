# SpectroCapture

Digitize a whole physical color collection as fast as you can scan it.

SpectroCapture is an open-source macOS app for **bulk color acquisition**. If you own a spectrophotometer and a collection of markers, swatches, paints, or other physical color samples, it lets you import an inventory and then scan heads-down through it — no metadata entry between scans — until the whole collection lands in a local database that's yours: portable, queryable, and never locked to a vendor account.

It is not verification software. Every existing desktop app for this hardware class exists to answer "does this sample match the standard?", one scan at a time. SpectroCapture answers a different question: can I get my whole collection scanned in efficiently? It's built for collectors, creatives, and makers who want their colors as data, not for a print-QC pressroom, and it's meant to be the only open-source option in an ecosystem where the open, instrument-agnostic color tools people already trust can't talk to this hardware at all.

What it isn't, on purpose:
- Not a color-matching tool — no PANTONE, RAL, or NCS library lookup.
- Not cross-platform — macOS only in v1; iOS and Windows/Linux ports are explicit non-goals for now.
- Not a way to hide reality — colors that fall outside your monitor's gamut are marked as such, not silently rendered as the "closest" color.

## Why this exists

If you search for software that talks to a consumer spectrophotometer, what you'll find — Nix Print Pro, SpotOn Spec, MeasureColor, and the RIP/tinting tools around them — is all built the same way: measure one sample, compare it to a standard, report the delta. That's the right tool if you're checking a print against a reference. It's the wrong shape if you're trying to get a 200-item collection into a database, one scan at a time, stopping to enter metadata between every item. Nobody — first-party, partner, or open source — builds for inventory-first bulk capture. That gap is what SpectroCapture is for.

## Status

**Pre-code.** There is no application yet — this repository currently holds product vision and research only. Product direction and competitive positioning are settled; architecture decisions (how the app is actually built) are in progress and will land as ADRs in `docs/decisions/` (not yet created). Until an ADR exists for a given question, treat any specific technical approach you read about as a recommendation, not a commitment.

Read the docs before assuming anything about how this will be built:
- [`docs/product/vision.md`](docs/product/vision.md) — what this is, who it's for, scope
- [`docs/briefs/`](docs/briefs) — the research behind the architecture decisions still to come

## How it will work

- **Import your inventory.** Bring a CSV of what you're digitizing — one row per item, mapped to whatever identifier and metadata columns you already track. This is the wedge: get the list in before you pick up the instrument.
- **Scan heads-down.** Connect your instrument and work through the queue, one item after another, without stopping to type anything in between scans. The pace is set by how fast your instrument can take a reading, not by the software.
- **Browse and query your own data.** Everything — the raw instrument payload and the derived color spaces — lands in a local SQLite file you own. Export to CSV, or query the file directly with any SQLite client. Nothing about your collection lives anywhere else.

## Hardware

v1 targets the **Nix Spectro 2 / Spectro L** (Nix Sensor), over BLE or USB. The vendor SDK is a licensed dependency, not something this repository can include — you'll need your own vendor license to run the app against real hardware. The SDK is architecturally single-device and single-session: one instrument, one scan in flight, at a time, so that's what v1 supports.

## Contributing

Contributions are welcome once the architecture ADRs land. See [CONTRIBUTING.md](CONTRIBUTING.md) for the current stage and how to get involved before then.

## License

[MIT](LICENSE)
