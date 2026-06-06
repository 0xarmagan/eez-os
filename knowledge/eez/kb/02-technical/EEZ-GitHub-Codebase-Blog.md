---
title: EEZ's Technical Stack: What's Built and How It Works
platform: Paragraph / Mirror
byline: Armagan Ercan
date: 2026-06-06
status: draft
word-count: ~1200
sources: eez-association GitHub org (adversarially verified, 2026-06-05)
---

## EEZ's Technical Stack: What's Built and How It Works

### 1. What EEZ is building and where it stands

EEZ is being built to solve Ethereum's fragmentation problem: rollups that cannot compose with each other, liquidity that cannot move atomically across chains, and applications that must choose one L2 ecosystem and accept the tradeoffs. The solution is a shared settlement layer that lets rollups execute cross-chain operations atomically, within a single Ethereum block, without requiring bridges or external trust assumptions.

That is the vision. This post is about the implementation — three active repositories, what each one does, and the architectural decisions that define how synchronous composability actually works at the protocol level. Every claim here traces to a specific file in the eez-association GitHub organisation.

---

### 2. Three repositories, three layers

The eez-association org contains three active repositories that together describe a complete synchronous rollup system.

The first is `sync-rollups-protocol`. This is the L1 contract layer: around 69KB of active Solidity, Foundry tests, and two formal specification documents. The core contracts are `EEZ.sol`, `IEEZ.sol`, and `Rollups.sol`. The specs, `SYNC_ROLLUPS_PROTOCOL_SPEC.md` and `EXECUTION_TABLE_SPEC.md`, were last pushed on 2026-06-05. This is where the execution model and proof interface live.

The second is `sync-rollups-composer`. This is a Rust implementation based on the reth client. The key detail: reth is an upstream pin, not a fork. The repo includes `DERIVATION.md`, which specifies how any node re-derives the full L2 chain from L1 data alone. The test suite runs 529 tests.

The third is `eez-rollup0`. This is a custom sequencer that connects to a rollup execution client via the Engine API. It targets L2 blocks every 2 seconds, with batches posted to L1 every 60 seconds. It was last pushed on 2026-06-05.

Together, these three repositories cover L1 contracts, L2 derivation, and sequencer logic. The design is layered, and the boundaries between layers are clearly documented.

---

### 3. Key insight 1: They simplified the execution model

The most interesting engineering decision in the codebase is not something that was added. It is something that was removed.

An earlier version of the protocol used a recursive reentrancy model for cross-chain calls. `ActionType` was an enum that classified cross-chain operations. `scope[]` tracked execution scope. The execution engine handled `RESULT`, `REVERT`, and `REVERT_CONTINUE` states, and recursive reentrancy was a live possibility that the contract had to manage.

All of that is gone. `CHANGES_FROM_PREVIOUS.md` documents the removal explicitly.

What replaced it is described in `SYNC_ROLLUPS_PROTOCOL_SPEC.md`, section D. Cross-chain call sequencing is now handled by a non-recursive while loop. The loop consumes entries from `ExpectedL1ToL2Call` one at a time. There is no recursion. There is no scope stack. The contract processes a flat, ordered list of expected cross-rollup interactions and either satisfies them or reverts the batch.

Why does this matter? Recursive reentrancy in cross-chain contexts is one of the hardest correctness problems in smart contract engineering. Every level of recursion multiplies the number of states you need to reason about. The redesign pushes complexity to the prover, which generates a single proof per batch submitted via `postBatch()`. The on-chain contract does accounting, not navigation. It checks that the actual sequence of L2 actions matches the pre-computed state transitions. If it does, the batch is accepted. If it does not, the batch reverts.

This is a meaningful architectural choice. Simpler on-chain logic is easier to audit, easier to formally verify, and reduces the attack surface for settlement bugs.

Grep the repo: `ActionType` returns zero results. The removal was complete.

---

### 4. Key insight 2: Any node can re-derive the full chain from L1

`DERIVATION.md` in `sync-rollups-composer` specifies something worth reading carefully: the full L2 chain state is re-derivable from `BatchPosted` events on L1 alone. No other data source is required.

The formula for L2 timestamps is explicit:

```
l2_timestamp = deployment_timestamp + ((l2_block_number + 1) × 12s)
```

L2 block timestamps are deterministic functions of L2 block number and a single deployment-time constant. `Rollups.sol` is the canonical source of truth on L1. Any node that can read Ethereum can reconstruct the complete L2 history.

This is the concrete form of the based rollup property. A based rollup derives its sequencing from L1 rather than a privileged sequencer. The EEZ design takes this further: derivation is specified formally, the formula is deterministic, and `BatchPosted` is the sole input.

The practical consequence is that sequencer failure becomes a liveness problem, not a safety problem. If the sequencer goes offline, no new batches are posted. But no history is lost, no state is corrupted, and any operator with access to L1 data can reconstruct everything that happened up to the last posted batch. Safety is inherited from Ethereum. The sequencer is a posting mechanism, not a trust assumption.

---

### 5. The canonical example: cross-chain flash loans

The `sync-rollups-protocol` README documents a specific use case verbatim: a cross-chain flash loan executed atomically within a single L1 block.

The sequence works as follows. A user borrows an asset on Rollup A. They use that asset on Rollup B, perhaps to exploit an arbitrage or provide liquidity. They repay the loan on Rollup A. All three steps settle in one L1 block. There is no intermediate state where the loan is outstanding across blocks.

Today, flash loans are single-chain. Moving assets between rollups requires a bridge, which introduces latency (often minutes to hours) and a trust assumption on the bridge operator or security model. Atomicity across chains is not possible with existing bridge designs because the two chains settle independently.

The atomicity guarantee in EEZ comes from single L1 settlement. Because all participating rollups post their state transitions to the same L1 block, and because the `postBatch()` call validates a single proof covering the entire cross-rollup execution, the outcome is atomic by construction. Either everything succeeds and is included in the L1 block, or nothing is. There is no partial settlement.

This is not a theoretical property. It is the direct consequence of the flat sequential execution model and the single-proof-per-batch design described in section 3.

---

### 6. Honest on the proof system

The current proof system is `ECDSAProofSystem.sol`. The contract is annotated in the source: "Temporary proof system." It uses ECDSA signatures, not zero-knowledge proofs.

ZK is the target. The `IProofSystem` interface is already designed to be ZK/ECDSA interchangeable. Swapping in a ZK verifier is a contract-level upgrade, not a protocol redesign. The interface abstracts over proof type, so the rest of the protocol does not need to change.

`MockZKVerifier.sol` exists in the test suite. It is a mock used for testing the interface, not a production verifier.

As of 2026-06-05, no production ZK verifier exists in any repository in the eez-association org. That is a statement of current state, not a criticism. Separating the proof system from the protocol and designing for swappability from the start is the correct approach. Shipping ECDSA first lets the team validate the execution model and derivation logic before committing to a specific zkVM. The proof system is a component with a defined interface. When the ZK verifier is ready, the protocol accepts it without redesign.

---

### 7. What's still open

Four questions the codebase does not yet answer.

First, which zkVM will be used for the production proof system? The `IProofSystem` interface is zkVM-agnostic. The choice has not been made public in any repository file.

Second, what does the Stage 3 and Stage 4 architecture look like? The specs describe the current protocol. Multi-stage rollup architecture is not yet documented.

Third, how does the protocol relate to EIP-8079? Based rollup standardisation on Ethereum is an active area. The codebase does not reference a specific EIP alignment.

Fourth, which alliance members have testnet integrations running against `eez-rollup0`? The sequencer is live. Integration status across the ecosystem is not visible from the public repositories.

For anyone who wants to go deeper, `SYNC_ROLLUPS_PROTOCOL_SPEC.md` and `DERIVATION.md` are the most complete public documentation of a synchronous based-rollup system on Ethereum. Start there.
