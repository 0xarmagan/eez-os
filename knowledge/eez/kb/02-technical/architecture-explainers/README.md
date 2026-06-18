# EEZ Architecture Explainer Series

Builder and partner-facing explainers derived from the DAPPCon EEZ Workshop (17 June 2026, Jordi Baylina, co-branded EEZ × ZisK VM). Primary source: `../../sources/dappcon-2026-eez-node-architecture.md` (the 56-slide deck digest + the node-architecture diagram + the agent-mesh expert review).

These are reference-grade explainers, not approved external comms. Any externally published version is a draft until Armagan signs off.

## The series

1. [The EEZ Proof — Synchronous Cross-Rollup Execution](01-the-eez-proof.md) — what "many rollups as one synchronous transaction" means; CALL/RETURN; why it is not message passing.
2. [Proxies and the Execution Table](02-proxies-and-execution-table.md) — the L1 cross-chain mechanism; proxies, not bridges; shared state.
3. [EEZ Properties and Rollup Sovereignty](03-eez-properties-and-sovereignty.md) — the 7 properties; what sovereign means; proof-system agnosticism.
4. [Chain Types and Cross-Chain Coordination](04-chain-types-and-coordination.md) — native, based/centralized, validiums; optimistic vs pessimistic coordination; binding vs non-binding.
5. [The Composer](05-the-composer.md) — permissionless bundler; `postAndVerifyBatch`; the open incentive and mempool questions.
6. [EEZ Node Architecture](06-node-architecture.md) — sequencer-per-chain + composer binaries; sync vs async blocks; followers; known open gaps.
7. [Real-Time Proving with ZisK](07-real-time-proving-zisk.md) — ADSTF; EEZ Trace blob format; the recursion pipeline; the minimum-two-prover requirement; the <3s target.

## Reading order

Explainers 1–3 are conceptual (the model, the mechanism, the properties). Explainers 4–6 are operational (chain types, the composer, the node software). Explainer 7 is the proving layer that makes the synchronous model possible. They cross-reference each other and can be read independently.

## Roadmap context (handle with care)

The deck's roadmap (slide 55) lists: cleanup smart contract, cleanup documentation, request for comments, audit smart contracts, signature and Zisk proving system, **Composer 1.0**, **Chain Zero**, and **Connecting Gnosis Chain**.

"Connecting Gnosis Chain" relates to the pre-GIP Gnosis-L2 plan. Treat it as internal context. Do not build external comms around the Gnosis connection without checking the current GIP status first. EEZ itself is not deployed: frame participation as a roadmap question on both sides, never as available today.

## Accuracy guardrails (applied to every explainer)

Each explainer ends with an "Accuracy notes" section. The five distinctions enforced throughout (see `../../../../memory/technical-accuracy-watchlist.md`):

1. **Async vs native finality** — async path ~20 min, native ~12s. Name the path beside any figure. The <3s figure in explainer 7 is the proof-generation target inside an L1 slot, not a finality number.
2. **Execution entries, not transactions** — for operations inside a native rollup. "Transaction" is scoped to L1 or a partner chain's own model.
3. **Proxies, not bridges** — for any EEZ-native cross-chain mechanism.
4. **Multi-prover, not single-prover** — a minimum of two proving systems per rollup is enforced in-contract. Single `prover` boxes in diagrams are topology abstractions.
5. **Economic zone, not an L2** — EEZ is built on Ethereum, not equivalent to an L2.

*Series drafted 2026-06-18 via agent-mesh (parallel subagents), reviewed against the technical-accuracy watchlist.*
