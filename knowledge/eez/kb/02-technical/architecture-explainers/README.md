# EEZ Architecture Explainer Series

Builder and partner-facing explainers on how the Ethereum Economic Zone (EEZ) works, derived from Jordi Baylina's and Phillipe Schommers' DAPPCon Berlin 2026 sessions.

Reference-grade, **not** approved external comms. Any externally published version is a draft until Armagan signs off.

## Start here

- **[Conventions & Caveats](00-conventions-and-caveats.md):** status, the canonical timing model, the five distinctions, verification status. Read once; every explainer links back to it.
- **[Glossary](GLOSSARY.md):** single definition for every term in the series.

## The series

| # | Explainer | Covers |
|---|---|---|
| 1 | [The EEZ Proof](01-the-eez-proof.md) | "Many rollups as one synchronous transaction," the CALL/RETURN model, why it is not message passing. |
| 2 | [Proxies and the Execution Table](02-proxies-and-execution-table.md) | The L1 cross-chain mechanism: proxies rather than bridges, shared state. |
| 3 | [EEZ Properties and Rollup Sovereignty](03-eez-properties-and-sovereignty.md) | The seven properties, what "sovereign" means, proof-system agnosticism. |
| 4 | [Chain Types and Cross-Chain Coordination](04-chain-types-and-coordination.md) | The three chain types, optimistic vs pessimistic coordination, binding vs non-binding sequencers. |
| 5 | [The Composer](05-the-composer.md) | The permissionless bundler, `postAndVerifyBatch`, the open incentive and mempool questions. |
| 6 | [EEZ Node Architecture](06-node-architecture.md) | The two binaries, sync vs async blocks, follower roles, known open gaps. |
| 7 | [Real-Time Proving with ZisK](07-real-time-proving-zisk.md) | The ADSTF, the EEZ Trace, the recursion pipeline, the per-rollup threshold, the sub-3-second target. |
| 8 | [Gnosis Chain: The First EEZ Chain](08-gnosis-chain-first-eez-chain.md) | Case study: the four integration points, the multisig-to-zk trust path, the Chiado prototype, pre-GIP status. |

## Reading order

1–3 are conceptual (model, mechanism, properties). 4–6 are operational (chain types, composer, node software). 7 is the proving layer that makes the synchronous model possible. 8 is the Gnosis Chain case study. Each cross-references the others and can be read independently.

## Source materials

- `knowledge/eez/sources/dappcon-2026-eez-node-architecture.md`: the 56-slide workshop deck, the node-architecture diagram transcription, and the expert review.
- `knowledge/eez/sources/dappcon-2026-realtime-proving-talk.md`: the spoken talk digest (ZisK 1.0 alpha, the proving pipeline, the accumulator, the data-availability limitation).
- `knowledge/eez/sources/dappcon-2026-gnosis-chain-eez-talk.md`: Phillipe Schommers' "The First EEZ Chain" digest.
- `eez-association/eez-core-protocol` (branded "Sync Rollups"): the contracts the conceptual claims were verified against, 18 June 2026.

## Roadmap context

The deck's roadmap, in order: cleanup smart contract, cleanup documentation, request for comments, audit smart contracts, the signature and ZisK proving system, **Composer 1.0**, **Chain Zero**, **Connecting Gnosis Chain**. ZisK reached 1.0 alpha (code frozen, in audit and formal verification), so the proving system is in progress, not finished.

"Connecting Gnosis Chain" relates to the pre-GIP Gnosis-as-an-EEZ-chain plan. Do not build external comms around the Gnosis connection without checking the current GIP status first. EEZ itself is not deployed. Frame participation as a roadmap question on both sides, never as available today.

## For reviewers

The per-document "Accuracy notes" sections were retired into a single [STYLE-CHECKLIST.md](STYLE-CHECKLIST.md) (the checklist a reviewer runs a draft against before publishing). It is internal QA, not reader-facing. The five distinctions it enforces live in [Conventions & Caveats](00-conventions-and-caveats.md).

*Series drafted 18 June 2026 via agent-mesh, verified against eez-core-protocol; restructured 19 June 2026 (shared Conventions + Glossary, accuracy notes moved to reviewer checklist).*
