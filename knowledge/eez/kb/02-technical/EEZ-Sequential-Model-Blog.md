---
title: How EEZ Handles Cross-Chain Reentrancy — and Why the Approach Matters
platform: Paragraph / Mirror
byline: Armagan Ercan
date: 2026-06-06
status: draft
sources: eez-association/sync-rollups-protocol — CHANGES_FROM_PREVIOUS.md, SYNC_ROLLUPS_PROTOCOL_SPEC.md (adversarially verified)
---

# How EEZ Handles Cross-Chain Reentrancy — and Why the Approach Matters

*By Armagan Ercan*

---

Atomic cross-chain execution is EEZ's central capability. A smart contract on one rollup calls a contract on another, gets a return value, and the whole sequence either commits or reverts — all within a single Ethereum block. That property sounds simple. Making it work is not.

The hardest part is cross-chain reentrancy: what happens when an L2 call triggers an L1 callback that calls back into another L2? Left unhandled, this creates an unbounded recursion problem on-chain. The way EEZ resolves it is the most important architectural decision in the protocol — and it is documented in detail in CHANGES_FROM_PREVIOUS.md and SYNC_ROLLUPS_PROTOCOL_SPEC.md.

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

The abstraction boundary is clean: complexity lives off-chain, where it can be verified with tooling not constrained by gas limits. The on-chain component remains minimal.

---

## Why this matters for security

Re-entrancy is Ethereum's most well-known vulnerability class. The DAO hack in 2016 was a re-entrancy bug. The pattern -- a contract calling back into itself or into another contract before state is settled -- has cost the ecosystem hundreds of millions of dollars and is still a primary focus in smart contract audits today.

Recursive on-chain logic in a cross-chain context is a harder version of that same problem. You are not just managing re-entrancy within a single contract. You are managing it across chains, with asynchronous message passing, where the contract cannot directly observe what is happening on the other side. The state space is large, the execution is non-local, and bugs are difficult to reproduce in test environments that do not replicate the full cross-chain interaction graph.

The sequential model eliminates that problem class at the design level. The contract is not an interpreter navigating a call tree at runtime. It is a verifier checking a committed list. There is no recursive call path on-chain for a bug to propagate through. The logic that produces the execution trace lives in the prover, where it can be tested and audited with standard software engineering tooling.

The attack surface shrinks. The on-chain code becomes easier to audit because it does less. When something goes wrong, the failure mode is a mismatch between the committed list and the actual execution -- a simple boolean check rather than a state machine with multiple exit conditions.

---

## The signal in the changelog

CHANGES_FROM_PREVIOUS.md is not a technical novelty. Changelogs exist in most codebases. What is notable here is the specificity. The document names the exact constructs that were removed. It states why they were removed. And because the removals can be confirmed by grepping the current codebase, the document is verifiable, not just claimed.

That combination -- explicit removal log, stated rationale, grep-confirmable outcome -- is a sign of architectural discipline. Simplification that happens gradually and unintentionally does not produce a document like this. It produces accumulated dead code, partially removed abstractions, and legacy branches no one is confident enough to delete.

The complete removal of `ActionType`, `scope[]`, and the recursive navigation machinery means the simplification was a deliberate decision, executed cleanly, with the result documented for reviewers who come later. For a protocol that will eventually be audited and potentially formally verified, that kind of housekeeping is not cosmetic. It is part of the correctness argument.

---

## Where to read more

For protocol engineers evaluating EEZ: two documents are worth reading in sequence. CHANGES_FROM_PREVIOUS.md shows the reasoning -- what was removed, why the recursive model was rejected, and what design principles drove the replacement. SYNC_ROLLUPS_PROTOCOL_SPEC.md §D shows the implementation -- how `_consumeNestedAction` works, what the sequential consumption of `ExpectedL1ToL2Call` entries looks like in practice.

One document shows the thinking. The other shows the result. Together they describe a protocol that chose auditability over expressiveness, pushed complexity to the prover, and came out with a simpler on-chain surface for it. That is a design trade-off worth understanding before building on top of it.
