# X-Rite SDK, the Argyll route, and open-source prior art — findings

> **Provenance & how to read this.** Web research performed 2026-08-17 across X-Rite's public OEM pages, the ArgyllCMS documentation and licensing, a GitHub API repository sweep, and vendor product pages, answering two questions: whether X-Rite offers an equivalent of the Nix SDK and how its license terms would land on an open repo, and whether anything resembling SpectroCapture already exists on GitHub. Nothing here was tested against instrument hardware — the project owns no X-Rite or Calibrite unit — so every claim is paper evidence, with inferences and unverified items flagged inline. **Role in this repo:** research, not decisions ([AGENTS.md §2](../../AGENTS.md#2-read-before-changing-anything)) — it is the evidence base behind the queued [ADR-0002](../decisions/README.md#decision-queue) on v1 instrument scope, and its licensing readings (AGPL boundaries, OEM terms) are lay readings, not legal advice. A companion competitive brief on Spectro 2 software is not published in this repo.

## 1. The X-Rite SDK — exists, and is the wrong door for us

X-Rite runs a formal **OEM SDK program** covering the i1Pro 3 / i1Pro 3 Plus, i1Studio, eXact/eXact 2, i1D3 colorimeters, Prism, and a Pantone API. Access is **request-gated**: log into My X-Rite, click "Request SDK," and a sales-mediated process follows — fees, NDA requirements, and redistribution terms are all **undisclosed** on the public pages.

**License impact on us:** the official SDK route is effectively incompatible-by-default with an open repo. Everything that makes ADR-0001's pattern work for Nix is missing here: no public distribution repo to reference as a dependency, no published EULA to reason about, no free non-commercial tier advertised, and an approval funnel built for commercial OEM partners — not open-source hobby tools. The honest comparison: **Nix's demo SDK (public download, published DocC, free eval keys, SDK hosted on Nix's own public GitHub) is the unusually OSS-friendly one.** We've been treating Nix's license gate as the project's constraint; among instrument vendors it's the *good* deal.

## 2. The viable X-Rite route is the opposite door: ArgyllCMS

**ArgyllCMS (AGPL-3.0)** ships open, reverse-engineered drivers for the i1Pro 3 and i1Pro 3 Plus, i1Studio/ColorMunki, i1Pro 2 and original i1Pro (secondhand), i1Display colorimeters, and older DTP units — with `spotread` as the spot-measurement CLI. No Nix support (and none possible; the wall runs the other way).

**AGPL impact on us — navigable, because we're open source:**
- Cleanest shape: **subprocess boundary** — SpectroCapture invokes `spotread` as a separate executable and parses its output; our code never links Argyll, so our repo license (MIT/Apache, TBD) stays intact.
- If we *bundle* Argyll binaries, AGPL distribution obligations attach (provide corresponding source) — routine for an OSS project, but it's a deliberate choice: "download Argyll separately" vs "bundled with source offer." Needs its own ADR when we get there.
- Graeme Gill sells commercial non-GPL licenses (ArgyllPRO) — an escape hatch we shouldn't need.
- ⚠️ Lay reading, same caveat as ADR-0001; the AGPL boundary question deserves care before any bundling.

**The leverage insight:** an Argyll adapter is not "X-Rite support" — it's **one integration that unlocks every instrument Argyll speaks** (X-Rite, Klein, JETI, DataColor; ~30 models). Proof this driver base can power a real product: Gill's own **ArgyllPRO ColorMeter** (Android, ~US$99) does field measurement + logging on exactly that instrument set.

**Hardware reality check:** i1Pro 3 is a ~$2k instrument (pro print/photo users). The consumer-tier i1 lineage now ships as **Calibrite** (ColorChecker Studio ≈ i1Studio, ~$500 class) — UNVERIFIED whether current Calibrite units enumerate as Argyll-supported i1Studio; check at spike time. We own no X-Rite hardware today, so any X-Rite path ships unverified until we do.

## 3. GitHub sweep — the white space holds on the open side too

Searched repositories across spectrophotometer/color-capture/Argyll/Nix terms. **Nothing does acquisition-first collection capture.** What exists:

| Prior art | What it is | Why it isn't SpectroCapture |
|---|---|---|
| **texchroma** (Python, active Jun 2026) | Pilot study: colour cataloguing of *textile collections* using a Nix sensor, Pantone matching, k-means, VLMs | Closest conceptual neighbor — and evidence the collection-digitization need is real (GLAM/textile world). A research pilot, not a product; no bulk workflow, no app. |
| **ArgyllPRO ColorMeter** (closed, Android, ~$99) | Measurement + 10k-entry log, TSV export, ~30 instruments | Measurement *logging*, not inventory-first acquisition; no collections/versioning/QC model; no Nix; mobile. |
| **MBPI-Spectro** (web) | Internal QC tool for a masterbatch manufacturer | Verification-shaped, single-company. |
| **mmmunki, x-rite-i5-tools, konicars, nix_csv_parser** | Per-device CLI utilities/parsers | Plumbing, not products. |
| **Argyll printer-profiling wrappers; DisplayCAL lineage** | Profiling/calibration workflows | Different job entirely. |

Two signals worth keeping: **texchroma** validates the use case beyond markers (textile/heritage collections — a future persona), and **nobody combines device breadth with an acquisition workflow** — ColorMeter has the breadth, nothing has the workflow.

## 4. Strategic implications

1. **"Support X-Rite from the start" should mean the architecture, not the integration.** Two integration regimes (licensed vendor SDK with entitlement/auth lifecycle vs AGPL subprocess adapter) in v1 doubles the device-layer surface before the wedge is proven — on hardware we don't own. The device-service seam (mock layer) is the from-the-start commitment; the Argyll adapter is the first fast-follow. See [ADR-0002](../decisions/README.md#decision-queue) (proposed).
2. **Never pursue the X-Rite OEM SDK.** Wrong terms-shape for an open repo; the Argyll route is better *and* broader.
3. **Reframe the v2 ambition:** not "add X-Rite" but "add the Argyll universe" — one adapter, N instrument families. This also de-risks the Nix dependency: if the Nix relationship ever sours, the product survives on Argyll-supported hardware.
4. **Evidence that would flip the sequencing:** an i1/Calibrite instrument on hand at spike time, or the primary persona widening beyond the marker collection (e.g., textile/GLAM interest à la texchroma) before v1 ships.
5. **Monitor:** Argyll release notes (new instrument support), texchroma (does the pilot become a tool?), Calibrite–Argyll compatibility reports.

## Gaps

- [ ] X-Rite SDK actual terms (would require submitting a request — do not, per implication 2, unless something changes).
- [ ] Calibrite ColorChecker Studio ↔ Argyll i1Studio compatibility — verify before recommending hardware to future users.
- [ ] AGPL boundary details (bundle vs external install) — ADR when the adapter is scheduled.
