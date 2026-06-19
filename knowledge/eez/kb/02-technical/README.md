# 02 — Technical

EEZ technical knowledge for the agent: foundational references, the architecture explainer series, the core-protocol spec, and blog drafts. Diagrams are shared from `diagrams/`.

## Layout

| Path | What it is |
|---|---|
| `Technical-Architecture.md`, `Synchronous-Composability.md`, `L2-Fragmentation.md`, `Technical-Proposals.md`, `ZK-Real-Time-Proving.md`, `EEZ-Smart-Contract-Technical-Deep-DIVE.md` | Foundational reference docs. Cross-referenced across the KB; cite these for technical claims. |
| `architecture-explainers/` | Eight-part explainer series (the EEZ proof, proxies, properties, chain types, composer, node architecture, ZisK proving, Gnosis Chain), each with an embedded diagram. |
| `core-protocol-spec/` | Spec-level diagrams sourced from `eez-association/eez-core-protocol` (core protocol, execution entry, lookup, multi-prover, caveats). |
| `blogs/` | Publication drafts (Paragraph / Mirror), each with an embedded diagram. Drafts until Armagan signs off. |
| `diagrams/` | All diagram sources (`.excalidraw`) and renders (`.png`) for the explainers and blogs. |

## Accuracy

Follow the technical-accuracy watchlist: proxies not bridges, execution entries not transactions, multi-prover capable with per-rollup thresholds (no minimum-two claim), economic zone not an L2, async (~20 min) vs native (~12s) finality named per path. EEZ is not deployed; frame participation as roadmap.

## Provenance

Architecture explainers, blogs, and diagrams were re-verified on 2026-06-19 against `eez-association/eez-core-protocol` (formerly `sync-rollups-protocol`) and `sync-rollups-composer`, and updated with the Dappcon Berlin 2026 talks. Source digests live in `../../sources/`.
