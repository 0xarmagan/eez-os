---
title: The Architecture Behind EEZ's Synchronous Settlement
platform: Paragraph / Mirror
byline: Armagan Ercan
date: 2026-06-06
status: draft
sources: eez-association/sync-rollups-protocol — SYNC_ROLLUPS_PROTOCOL_SPEC.md, EXECUTION_TABLE_SPEC.md, IEEZ.sol (adversarially verified)
---

# The Architecture Behind EEZ's Synchronous Settlement

EEZ enables rollups to settle atomically on Ethereum and compose synchronously — a smart contract on one chain calls another, gets a return value, and the whole execution either commits or reverts in a single L1 block. The mechanism that makes this possible is specified in two documents: `SYNC_ROLLUPS_PROTOCOL_SPEC.md` and `EXECUTION_TABLE_SPEC.md`. Together they are the most complete public specification of a synchronous based-rollup system on Ethereum. This post walks through what they define.

---

## The settlement model

Settlement in EEZ is a single function: `postBatch()`.

The sequencer calls it once per batch. The arguments contain L2 state transitions for all participating rollups, the ordered list of cross-chain call entries, and a single proof covering the whole batch. `Rollups.sol` receives the submission, checks the proof against the registered verifier for each rollup, and either applies all state updates or reverts the entire transaction.

That last point is worth pausing on. There is no partial settlement. One L1 transaction settles the state of every rollup included in the batch. If anything in the batch is invalid — wrong proof, mismatched call entry, state commitment mismatch — the whole transaction fails. This is where the atomic guarantee comes from. It is not a coordination mechanism bolted on top; it is a property of the settlement function itself.

`IEEZ.sol` is the canonical L1 interface. `EEZ.sol` is the full implementation — approximately 69KB of active Solidity. The size reflects the scope: state update logic, proof system dispatch, cross-rollup call verification, and registry management are all on-chain. The settlement logic does not delegate the hard parts to off-chain services; it verifies them.

---

## The execution table

`EXECUTION_TABLE_SPEC.md` defines the structure that makes cross-rollup sequencing deterministic.

Before a batch is submitted, the prover constructs an ordered list of every expected L1-to-L2 interaction that will occur during execution. This list is the execution table. Each entry is an `ExpectedL1ToL2Call` — a data structure that encodes one expected cross-chain call: the source, the target rollup, the calldata, and its position in the sequence.

When the batch is validated on-chain, the contract walks the execution table in order. The protocol uses a flat sequential model: `_consumeNestedAction` is a non-recursive while loop that consumes entries from the list one at a time. There is no `ActionType` enum, no `scope[]` array, no recursive reentrancy. The sequencing logic is explicit and linear.

The separation of concerns here is significant. The on-chain verifier is stateless and minimal: it checks that the submitted execution matches the committed table, entry by entry. The prover handles the complex call graph off-chain and commits to it before submission. Once committed, the execution is fixed. The on-chain contract does not need to reason about call ordering at validation time — that work was done before `postBatch()` was ever called.

This is how pre-computed state transitions work in EEZ. Transitions are generated off-chain and submitted with the batch. The on-chain contract validates them against the committed `ExpectedL1ToL2Call` entries. The chain does not re-execute; it verifies.

---

## The proof interface

`IProofSystem` is a small interface with significant architectural consequences.

It decouples proof verification from the protocol. Any registered verifier that satisfies `IProofSystem` can validate a rollup's state transitions. The contract does not care whether verification is ZK or ECDSA — it dispatches to whatever system is registered for that rollup and accepts or rejects based on the result.

The current implementation, `ECDSAProofSystem.sol`, is explicitly temporary. It exists to allow development and testing while ZK proof systems mature. The interface is designed for ZK to be swapped in without changing the protocol.

This matters for how the spec ages. A rollup can upgrade its registered proof system independently. A new zkVM integration does not require a protocol change — it requires a new contract that satisfies `IProofSystem` and a registry update. The protocol spec remains stable; the proof system evolves underneath it.

For researchers evaluating proof-system agnostic designs in rollup protocols, this is the live reference implementation.

---

## What the spec does not yet cover

Two stages remain unspecified in public documents.

Stage 3 is described in internal documentation as covering reorg handling and `eez-follower` derivation for full nodes. Stage 4 is described as covering a full cross-chain composer with ZK proof. Neither is documented in the current public spec.

This is worth stating plainly. The spec covers settlement, execution table semantics, and the proof interface. It does not cover reorg recovery or the full ZK-backed composer. These are clearly bounded gaps — the spec itself does not claim otherwise. It is an active development document, not a finished protocol.

Anyone building on the spec or evaluating EEZ for integration should note the boundary. What is specified is specified in detail. What is not specified is not present.

---

## Why read the spec directly

Most Ethereum protocol work is documented across EIPs, ethresear.ch posts, and client implementations. Formal specs at the settlement and execution level — covering function semantics, data structure definitions, and proof interface abstractions — are less common.

`SYNC_ROLLUPS_PROTOCOL_SPEC.md` fills a gap. There is no other public document that specifies, at this level of detail, how a synchronous based-rollup system settles state across multiple rollups in a single L1 transaction, how cross-rollup call sequencing is committed before execution, and how proof verification is abstracted from the protocol.

If you are working on rollup architecture and have been relying on secondary summaries of EEZ, the spec is the better source. It is dense in places and incomplete in others. It is also the most precise thing available.

---

*Armagan Ercan is Coordinator at EEZ.*
