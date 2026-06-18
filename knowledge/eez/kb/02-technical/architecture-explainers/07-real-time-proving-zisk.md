# Real-Time Proving with ZisK

*Source: `knowledge/eez/sources/dappcon-2026-eez-node-architecture.md` (DAPPCon EEZ Workshop, 17 June 2026, Jordi Baylina, co-branded Ethereum Economic Zone × ZisK VM). Engineering-level founding material. Quote as Jordi's framing, not as approved EEZ comms. EEZ is at roadmap stage and is not deployed yet. The ZisK proving system is itself a roadmap item, listed under "Signature and Zisk proving system" on the DAPPCon roadmap slide.*

This explainer is for builders and partners who want to understand how the Ethereum Economic Zone (EEZ) generates proofs fast enough to make synchronous cross-rollup execution work. It covers the Action-Driven State Transition Function (ADSTF), the EEZ Trace blob format, the recursion pipeline step by step, the in-contract rule that forces at least two proving systems per rollup, and why low proof-generation latency is the thing that makes a single synchronous cross-rollup step possible at all.

One framing note before we start. The DAPPCon talk is branded around ZisK, so it is easy to read EEZ as a single-prover system. It is not. ZisK is one proving system among several. EEZ requires a minimum of two proving systems per rollup, and the requirement is enforced in the contract. Hold that point through the whole document. We return to it in the multi-prover section.

EEZ is an economic zone built on Ethereum. It is not an L2, and it is not equivalent to any single rollup. It is the shared proving and settlement layer that lets independent rollups call into each other inside one atomic step.

## The Action-Driven State Transition Function (ADSTF)

A normal state transition function takes a starting state and a block of transactions, and produces an ending state. EEZ needs more than that, because a rollup in EEZ does not run in isolation. It can be called by another rollup, and it can call out to another rollup, all inside the same proven step.

The ADSTF captures this. It runs like so:

```
ACTION IN  →  INITIAL STATE  →  ADSTF  →  FINAL STATE
                                       →  ACTION OUT
                                       →  REVERT INFO
```

Read the inputs and outputs as a single shape. The ADSTF takes an incoming action and the rollup's initial state. It produces three things. First, the final state of that rollup. Second, an outgoing action, which is the rollup's own CALL or RETURN to another chain. Third, revert information, which records what should be undone if any part of the combined step fails.

The action in and action out are how the function expresses cross-chain behaviour. Inside a native rollup, the work the function performs is made of execution entries, not transactions. The action in is a CALL or RETURN arriving from another chain. The action out is a CALL or RETURN this rollup sends to another chain. Because the function names both its inputs and its cross-chain effects explicitly, the proof can reason about cross-chain interaction directly, rather than treating it as an external side effect that happens somewhere off to the side.

The revert info matters for atomicity. A synchronous cross-rollup step either fully happens or fully unwinds. The ADSTF emits enough information at each step to reverse it, so a revert anywhere in the combined execution can roll the whole step back cleanly.

## The EEZ Trace blob format

The ADSTF describes what one rollup does. The EEZ Trace describes how the rollups hand control to each other.

The deck calls it "the blob format." It records each context switch between chains. A context switch is a CALL, where one chain hands execution to another, or a RETURN, where execution comes back. The trace records both directions. It also records reverts, so the unwinding of a failed step is part of the same record as the forward execution.

Think of the EEZ Trace as the authoritative log of the combined execution. It is not a per-chain log stitched together after the fact. It is one ordered record of every boundary crossing in the step. Chain 1 calls Chain 2. The trace records the switch. Chain 2 runs and returns. The trace records the switch back. If Chain 2 reverts, the trace records that too, alongside the revert info the ADSTF produced.

This single ordered record is what the proving pipeline proves over. The pipeline does not prove each chain on its own and hope the pieces fit. It proves that the whole trace, every context switch in order, is consistent and valid.

## The recursion pipeline, step by step

EEZ proves the combined execution through a chain of recursive circuits. Each circuit verifies the output of the one before it, so the final proof stands in for all the work underneath it. This is genuinely sequential, so a numbered walk-through is the clearest way to read it.

1. **ADSTF Adaptor.** This is the entry point. Each rollup defines its own state transition function and supplies a circuit for it. The ADSTF Adaptor verifies a user-defined circuit verification key (VK). In plain terms, it checks that the rollup ran the state transition function the rollup itself committed to, and nothing else. Because rollups are sovereign and define their own rules and their own accepted proving systems, this adaptor step is where each rollup's own choices get checked against its own declared VK.

2. **L1 Hash Builder.** This circuit ties the proof to L1. It handles the blob commitment, so the data the proof covers is the data actually posted. It records the starting and ending blob rollup state, so the step has a defined before and after on L1. It captures the L1 interactions for the step. And it carries the mapping from each RollupId to the proof system and VK that rollup uses. This mapping is the bridge between the abstract proof and the concrete in-contract rule about which proving systems are allowed, which we cover in the next section.

3. **Aggregation Circuit.** This circuit verifies two circuits at once and adds their accumulators together. An accumulator is a running value that lets later circuits defer some verification work cheaply rather than redoing it. By combining two proofs and summing their accumulators, the aggregation circuit folds many sub-proofs into one as the tree builds up. It runs repeatedly, pairing proofs together until a single root aggregation remains.

4. **Final Circuit.** This circuit verifies the root aggregation together with the L1 Hash Builder, then performs a critical check: it confirms the accumulator sum equals zero. The zero check is the closing argument. If every deferred verification across the whole tree was honest, the accumulators cancel out to zero. A non-zero sum means something in the tree did not verify, and the proof fails. So this one check stands in for the correctness of everything aggregated below it.

5. **PLONK Circuit.** The recursive proof up to this point is efficient to build but not cheap to verify on chain. The final stage wraps it in a PLONK proof, which is easy to verify on chain. This is the proof the L1 contract actually checks. The whole pipeline exists to compress the combined execution of many rollups into this one onchain-verifiable artifact.

Read the pipeline as a funnel. Many per-rollup proofs go in at the ADSTF Adaptor. The L1 Hash Builder anchors them to L1. The Aggregation Circuit folds them together. The Final Circuit checks the fold closed cleanly with the zero-accumulator test. The PLONK Circuit produces the single proof that settles on L1.

## Multi-prover enforcement

Now the point we flagged at the top. EEZ does not trust one proving system. It requires at least two per rollup, and the contract enforces it.

The mechanism is concrete. The contract uses structures named `ProofSystemBatchPerVerificationEntries` and `RollupIdWithProofSystems`, and a function `_fetchVkMatrix(...)` that builds a per-rollup VK matrix. A second function, `checkProofSystemsAndGetVkeys(...)`, validates that matrix. If a rollup's configuration does not meet the requirement, the contract reverts with `InvalidProofSystemConfig()`.

So the minimum of two proving systems is not a recommended default and not a convention. It is a hard requirement. A rollup that tries to settle with a single proving system gets its batch reverted. The VK matrix is how the contract keeps track of which proving systems each rollup must satisfy, and the revert is the enforcement.

This is why ZisK branding on the talk does not make EEZ a ZisK system. ZisK is one valid proving system. The EEZ properties list names ZK, TEE, and multisig as acceptable proof system types, and a rollup is free to choose. But it must choose more than one. A workable configuration is something like ZisK plus SP1 plus a TEE. The reason is robustness. If one proving system has a bug, the others still have to agree before a batch settles, so a single compromised prover cannot push an invalid state through.

When you describe EEZ proving to anyone, describe it as multi-prover. Never write "the EEZ prover" or "the EEZ ZK proof system" in the singular. The architecture is plural by requirement.

## Latency and synchronous composability

The deck defines synchronous composability in operational terms: it means minimising proof-generation latency. That sounds narrow, but it is the heart of the design.

Here is the constraint. EEZ wants a cross-rollup CALL and its RETURN to resolve inside one atomic, proven step that settles on L1. For that step to fit in normal L1 operation, the proof for the combined execution has to be ready within a single L1 slot. So the pipeline targets under three seconds for proof generation.

Name what that figure refers to, because EEZ has several timing numbers and they are easy to confuse. The under-three-seconds target is the proof-generation budget inside one L1 slot. It is not a finality number. Native rollups in EEZ reach finality in around twelve seconds. The async path takes around twenty minutes. The three-second figure is a different thing again: it is how fast the proof itself must be produced so the synchronous step lands in one slot.

EEZ hits the target by overlapping work. Bundle building, done by the composer, and proof building run at the same time rather than one after the other. The composer assembles the cross-rollup bundle while the prover is already at work. Helper data can be streamed to the prover so it starts early, before the full bundle is final. The L1-state-independent portion of the proof can begin immediately. Only the part that depends on the final L1 block hash has to wait. By the time the bundle is complete, much of the proof is already done.

This is why real-time proving is the enabling piece, not a performance nicety. Synchronous cross-rollup execution means a contract on one rollup calls a contract on another and gets its answer inside the same proven step, with shared state, before anything settles. That is only possible if the proof of the combined execution can be produced inside the settlement window. If proving took minutes, the interaction would have to break into separate steps on separate timelines, which is exactly the asynchronous model EEZ is built to avoid. Fast proving is what keeps the CALL and the RETURN in one atomic unit. Take the speed away and the synchronous property goes with it.

So the pipeline and the latency target are two halves of one idea. The recursion pipeline makes the combined execution provable and cheap to verify on chain. The overlapped bundle and proof building make that proof arrive in time. Together they let many rollups behave, for the length of one step, like a single machine.

## Accuracy notes

- **Multi-prover is a protocol requirement, enforced in-contract.** Minimum two proving systems per rollup, checked by `checkProofSystemsAndGetVkeys(...)` against the VK matrix from `_fetchVkMatrix(...)`, reverting with `InvalidProofSystemConfig()`. ZisK is one proving system, not the whole system. Never use singular proving framing for EEZ.
- **The under-three-seconds figure is a proof-generation target inside one L1 slot.** It is not finality. Native rollups finalise in around twelve seconds. The async path takes around twenty minutes. Name which figure you mean whenever you cite a number.
- **Proxies, not bridges.** EEZ's cross-chain mechanism uses proxies, which are synchronous and share state. Do not call anything EEZ-native a bridge.
- **Execution entries, not transactions, inside native rollups.** "Transaction" applies to the L1 layer or to a partner chain's own model, not to operations inside an EEZ native rollup.
- **Economic zone, not L2.** EEZ is an economic zone built on Ethereum. It is not an L2 and is not equivalent to any single rollup.
- **EEZ is not deployed yet, and the ZisK proving system is a roadmap item.** No one can participate today. Frame everything here as design and roadmap, sourced from Jordi Baylina's DAPPCon workshop, not as live capability or approved EEZ comms.
