---
title: How EEZ Handles Cross-Chain Reentrancy, and Why the Approach Matters
platform: Paragraph / Mirror
byline: Armagan Ercan
date: 2026-06-19
status: draft
sources: eez-association/eez-core-protocol (formerly sync-rollups-protocol), CORE_PROTOCOL_SPEC.md, MULTI_PROVER_SPEC.md (re-verified 2026-06-19)
---

# How EEZ Handles Cross-Chain Reentrancy, and Why the Approach Matters

*By Armagan Ercan*

---

Atomic cross-chain execution is EEZ's central capability. A smart contract on one rollup calls a contract on another, gets a return value, and the whole sequence either commits or reverts, all within a single Ethereum block. That property sounds simple. Making it work is not.

The hardest part is cross-chain reentrancy: what happens when an L2 call triggers an L1 callback that calls back into another L2? Left unhandled, this creates an unbounded recursion problem on-chain. The way EEZ resolves it is the most important architectural decision in the protocol. It is documented in detail in CORE_PROTOCOL_SPEC.md, and anyone can confirm the result by reading the current eez-core-protocol codebase.

---

## The old model

Cross-chain execution is inherently recursive in structure. An action on L1 triggers a call on L2. That call may trigger a response back on L1. That response may trigger another L2 action. You end up with a tree.

The previous EEZ model treated this tree as a runtime object. It introduced an `ActionType` enum to classify each operation at execution time. A `scope[]` array tracked the current depth and position within the call tree. Three action states (`RESULT`, `REVERT`, and `REVERT_CONTINUE`) handled the different ways a node in that tree could resolve. And when the protocol needed to move from one node to the next, it navigated the tree recursively.

On its own terms, this is a coherent design. It is also expensive. Every level of recursion multiplies the number of states the contract must correctly handle. A bug at depth two propagates to every deeper level. Gas costs scale with traversal. And every new action type added to the enum is a new branch in the on-chain logic that an auditor must check. The attack surface grows with the expressiveness of the model.

---

## What changed

Every one of those constructs is now gone. `ActionType` returns zero results in a grep across the current codebase. `scope[]` is absent. `RESULT`, `REVERT`, and `REVERT_CONTINUE` no longer exist as distinct states. Recursive scope navigation was removed completely.

In their place: `ExpectedL1ToL2Call` entries and a non-recursive while loop.

The mechanism works like this. Before a cross-chain sequence executes, the prover computes the full execution trace off-chain. Every expected L1-to-L2 call in the sequence is committed in advance as an ordered list. On-chain, the contract does not navigate a tree at runtime. It walks the `ExpectedL1ToL2Call` list sequentially via `_consumeNestedAction`, which is implemented as a plain while loop with no recursion. Each entry is consumed in order. The contract checks actual execution against what was committed. Verification, not interpretation.

The L1 to L2 to L1 to L2 chains that the old model handled via runtime tree traversal are now handled by the pre-committed list. If the actual call sequence matches the committed entries, the batch is valid. The prover does the navigation. The contract does the accounting.

The abstraction boundary is clean: complexity lives off-chain, where it can be verified with tooling not constrained by gas limits. The on-chain component remains minimal. The batch is settled by `postAndVerifyBatch()`, which validates the committed entries, verifies the proofs, and marks the executions verified in one call.

This flat, sequential consumption model now coexists with a newer per-rollup multi-prover layer. A batch carries multiple proof systems (`ProofSystemBatchPerVerificationEntries`), and each rollup sets its own threshold through its own `Rollup.sol` manager. During settlement, `checkProofSystemsAndGetVkeys` reverts with `ThresholdNotMet` if a rollup's chosen threshold is not satisfied. EEZ is multi-prover capable; the threshold is the rollup's choice, not a fixed protocol minimum. Each rollup also has its own deferred execution queue (`verificationByRollup`), so the sequential accounting stays per-rollup even as proofs are checked across several systems. The two layers are independent: the flat consumer handles call ordering, the threshold layer handles how many provers must agree.

---

## Why this matters for security

Re-entrancy is Ethereum's most well-known vulnerability class. The DAO hack in 2016 was a re-entrancy bug. The pattern (a contract calling back into itself or into another contract before state is settled) has cost the ecosystem hundreds of millions of dollars and is still a primary focus in smart contract audits today.

Recursive on-chain logic in a cross-chain context is a harder version of that same problem. You are not just managing re-entrancy within a single contract. You are managing it across chains, with asynchronous message passing, where the contract cannot directly observe what is happening on the other side. The state space is large, the execution is non-local, and bugs are difficult to reproduce in test environments that do not replicate the full cross-chain interaction graph.

The sequential model eliminates that problem class at the design level. The contract is not an interpreter navigating a call tree at runtime. It is a verifier checking a committed list. There is no recursive call path on-chain for a bug to propagate through. The logic that produces the execution trace lives in the prover, where it can be tested and audited with standard software engineering tooling.

The attack surface shrinks. The on-chain code becomes easier to audit because it does less. When something goes wrong, the failure mode is a mismatch between the committed list and the actual execution, a simple boolean check rather than a state machine with multiple exit conditions.

---

## The signal in the code

What is notable here is not a changelog. It is the current clean state of the codebase itself. The old machinery does not survive as dead code, commented-out branches, or half-removed abstractions. It is simply gone. A reader can confirm this directly: grep the current eez-core-protocol repository for `ActionType` and it returns zero results. `scope[]` is absent. `RESULT`, `REVERT`, and `REVERT_CONTINUE` do not exist as states. The only place the legacy model is mentioned is a single pointer in CORE_PROTOCOL_SPEC.md noting that the document now covers the flat sequential model.

That combination, complete removal with a grep-confirmable outcome, is a sign of architectural discipline. Simplification that happens gradually and unintentionally does not produce a result like this. It produces accumulated dead code, partially removed abstractions, and legacy branches no one is confident enough to delete.

The complete removal of `ActionType`, `scope[]`, and the recursive navigation machinery means the simplification was a deliberate decision, executed cleanly, leaving a codebase that reviewers can read top to bottom without tripping over a model that no longer applies. For a protocol that will eventually be audited and potentially formally verified, that kind of housekeeping is not cosmetic. It is part of the correctness argument.

---

## Where to read more

For protocol engineers evaluating EEZ, two documents are worth reading. CORE_PROTOCOL_SPEC.md §D shows the implementation: how `_consumeNestedAction` works, and what the sequential consumption of `ExpectedL1ToL2Call` entries looks like in practice. MULTI_PROVER_SPEC.md shows the per-rollup threshold layer that now sits alongside it. Together they describe a protocol that chose auditability over expressiveness, pushed complexity to the prover, and came out with a simpler on-chain surface for it.

This clean surface is now heading exactly where it was built to go: ZisK, one of the proof systems in the picture, has reached 1.0 alpha, and the next phase is auditing and AI-assisted formal verification. That is a design trade-off worth understanding before building on top of it.
