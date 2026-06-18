# EEZ Architecture Explainer Series

Builder and partner-facing explainers derived from the DAPPCon EEZ Workshop (17 June 2026, Jordi Baylina, co-branded EEZ × ZisK VM). Primary source: `../../sources/dappcon-2026-eez-node-architecture.md` (the 56-slide deck digest + the node-architecture diagram + the agent-mesh expert review).

These are reference-grade explainers, not approved external comms. Any externally published version is a draft until Armagan signs off.

## The series

1. [The EEZ Proof: Synchronous Cross-Rollup Execution](01-the-eez-proof.md): what "many rollups as one synchronous transaction" means; CALL/RETURN; why it is not message passing.
2. [Proxies and the Execution Table](02-proxies-and-execution-table.md): the L1 cross-chain mechanism; proxies, not bridges; shared state.
3. [EEZ Properties and Rollup Sovereignty](03-eez-properties-and-sovereignty.md): the 7 properties; what sovereign means; proof-system agnosticism.
4. [Chain Types and Cross-Chain Coordination](04-chain-types-and-coordination.md): native, based/centralized, validiums; optimistic vs pessimistic coordination; binding vs non-binding.
5. [The Composer](05-the-composer.md): permissionless bundler; `postAndVerifyBatch`; the open incentive and mempool questions.
6. [EEZ Node Architecture](06-node-architecture.md): sequencer-per-chain + composer binaries; sync vs async blocks; followers; known open gaps.
7. [Real-Time Proving with ZisK](07-real-time-proving-zisk.md): ADSTF (deck term); EEZ Trace blob format; the recursion pipeline; per-rollup proving threshold; the <3s target.

## Reading order

Explainers 1–3 are conceptual (the model, the mechanism, the properties). Explainers 4–6 are operational (chain types, the composer, the node software). Explainer 7 is the proving layer that makes the synchronous model possible. They cross-reference each other and can be read independently.

## Roadmap context (handle with care)

The deck's roadmap (slide 55) lists: cleanup smart contract, cleanup documentation, request for comments, audit smart contracts, signature and Zisk proving system, **Composer 1.0**, **Chain Zero**, and **Connecting Gnosis Chain**.

"Connecting Gnosis Chain" relates to the pre-GIP Gnosis-L2 plan. Treat it as internal context. Do not build external comms around the Gnosis connection without checking the current GIP status first. EEZ itself is not deployed: frame participation as a roadmap question on both sides, never as available today.

## Accuracy guardrails (applied to every explainer)

Each explainer ends with an "Accuracy notes" section. The five distinctions enforced throughout (see `../../../../memory/technical-accuracy-watchlist.md`):

1. **Async vs native finality**: async path ~20 min, native ~12s. Name the path beside any figure. The <3s figure in explainer 7 is the proof-generation target inside an L1 slot, not a finality number.
2. **Execution entries, not transactions**: for operations inside a native rollup. "Transaction" is scoped to L1 or a partner chain's own model.
3. **Proxies, not bridges**: for any EEZ-native cross-chain mechanism.
4. **Proof-system agnostic and multi-prover capable**: each rollup sets its own threshold (an M-of-N choice, one or more) on its manager contract. There is NO protocol-enforced minimum of two; the manager reverts `ThresholdNotMet` below the rollup's chosen threshold. Single `prover` boxes in diagrams are topology abstractions. Avoid singular framing for EEZ as a whole, but do not claim a contract floor of two.
5. **Economic zone, not an L2**: EEZ is built on Ethereum, not equivalent to an L2.

## Verification status

The conceptual claims (proxies not bridges, execution entries, `postAndVerifyBatch`, sovereign rollups, per-rollup proving threshold) were verified on 2026-06-18 against the `eez-association/eez-core-protocol` repo (branded "Sync Rollups"). Corrections applied after that audit: the "minimum two provers" claim was removed (the protocol uses a per-rollup configurable threshold, not a global floor); `ADSTF` is flagged as the deck's conceptual term, not a literal type in the code; explainer 4 carries a scope caveat (the core protocol currently targets based rollups sharing the same L1 sequencer; the three-chain-type picture is the deck's broader vision). The repo is early-stage and not audited, so interfaces may shift.

*Series drafted 2026-06-18 via agent-mesh (parallel subagents), reviewed against the technical-accuracy watchlist and audited against eez-core-protocol.*
