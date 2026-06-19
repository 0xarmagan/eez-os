---
title: Cross-Chain Flash Loans: How EEZ Makes Them Possible
platform: Paragraph / Mirror
byline: Armagan Ercan
date: 2026-06-19
status: draft
sources: eez-association/eez-core-protocol (formerly sync-rollups-protocol), README, CORE_PROTOCOL_SPEC.md, IntegrationTestFlashLoan.t.sol; Gnosis Chain Dappcon prototype (re-verified 2026-06-19)
---

# Cross-Chain Flash Loans: How EEZ Makes Them Possible

Flash loans are one of DeFi's most interesting primitives. They work because the EVM guarantees atomicity: borrow and repay in a single transaction, or the whole thing reverts. Nothing moves unless the repayment is there. That guarantee is what makes the mechanic useful.

But today, flash loans stop at the chain boundary. The moment you want to borrow on one rollup and act on another, you lose atomicity. Cross-chain messages are asynchronous. There is no existing mechanism that gives you the same borrow-use-repay guarantee across two separate chains.

EEZ is designed to make this possible. Cross-chain flash loans are a demonstrated capability: borrow on one rollup, act on another, repay on the first, all atomic, all within a single Ethereum block. This post walks through how the protocol achieves it, what the current implementation looks like, and what constraints are still in play.

---

## Why Flash Loans Break at the Chain Boundary

A flash loan works because of what the EVM does not allow: partial execution. Within a single transaction, the VM either applies all state changes or none of them. The lender encodes a repayment check at the end of the transaction. If the repayment is not there, the transaction reverts. The loan never happened.

That property, atomicity, is what makes the entire mechanic safe and useful. It lets protocols lend capital without collateral because they know, by construction, that capital will be returned.

Cross-chain breaks this. If you borrow on Rollup A and want to act on Rollup B, you have to send a message between chains. That message is asynchronous. By the time it arrives on Rollup B, your transaction on Rollup A has already settled. There is no shared execution context. The lender cannot enforce repayment because the two events happen in different blocks, possibly minutes apart.

Bridges and cross-chain messaging protocols do not solve this. They move data between chains, but they do not give you the ability to say "both of these things happen atomically, or neither does." The repayment guarantee breaks.

---

## How EEZ Provides Cross-Chain Atomicity

The key is where settlement happens. In the EEZ protocol, multiple rollups participate in a shared batch that settles on Ethereum L1. All participating rollups submit their state transitions together. They all settle in the same L1 block via a single `postAndVerifyBatch()` call.

This is not a message-passing scheme. The cross-chain interactions are encoded as `ExpectedL1ToL2Call` entries before execution begins. These entries define the pre-committed sequence of cross-chain calls in the batch. The sequencer includes all of them in a single batch submission.

The provers then compute the full execution trace off-chain, covering the entire cross-chain sequence. The batch carries one proof per configured proof system (a `bytes[] proofs` array), and the on-chain contract verifies all of them against the committed list of expected calls. Each rollup sets its own threshold for how many proof systems must attest, an M-of-N rule the rollup owner chooses. If verification passes for every participating rollup, the L1 contract applies all state updates atomically across all of them.

The critical property: all rollups settle in the same L1 block, or all revert. There is no partial settlement. A state update on Rollup A and a state update on Rollup B either both happen in that block or neither does. That is what makes atomic cross-chain flash loans constructable.

---

## The Flash Loan, Step by Step

Here is what a cross-chain flash loan looks like in this model.

A user initiates the sequence. The batch includes the following pre-committed steps:

1. **Rollup A** decrements the borrower's balance. The flash loan is disbursed. This state change is part of the batch.
2. **Rollup B** receives the funds in the same execution trace. Whatever the borrower wants to do (arbitrage, liquidation, collateral rebalancing) runs here.
3. **Rollup A** receives the repayment. The repayment is encoded as an `ExpectedL1ToL2Call` entry in the same batch.

The provers attest to all three steps as one inseparable trace. The trace covers the full cross-chain execution, computed off-chain. `postAndVerifyBatch()` verifies the batch's proofs against the committed entry list, and the batch is all-or-nothing: every configured proof system must pass for the rollups it covers, or the whole batch reverts.

If the repayment entry is missing or incorrect, verification fails. The entire batch reverts. The flash loan cannot succeed without repayment. Not by convention, but by the same construction that makes single-chain flash loans safe. The enforcement mechanism is the proof, not a runtime check by the borrower.

The atomicity guarantee is end-to-end: borrow on A, act on B, repay on A, all within one L1 block, all standing or falling together.

---

## What This Opens Up

Flash loans are the clearest example because the repayment constraint makes the atomicity requirement explicit. But the same property applies to other cross-chain operations.

Cross-chain liquidations become possible without a solver fronting capital on both chains. If you can prove that collateral on Chain A is being liquidated and that the proceeds cover a position on Chain B, you can execute both atomically. Cross-chain arbitrage does not require pre-positioned capital. If the arbitrage closes correctly, the proof validates; if not, it reverts.

The general point: any operation that requires "A happens if and only if B happens" across two rollups becomes expressible. Flash loans are the simplest version. They are useful to focus on because the invariant is so clean: you either have a repayment or you do not.

---

## Current State and What Is Not Yet Live

The cross-chain flash loan is no longer just a spec example. At Dappcon in Berlin in June 2026, Gnosis Chain showed it running. The April 2026 prototype is a based rollup built on reth, live on the Chiado testnet, and the demo covered instant cross-chain deposits, calls that return values, cross-chain flash loans, and nested calls five levels deep, all atomic. The integration test in the protocol repo (`IntegrationTestFlashLoan.t.sol`) walks the same borrow-act-repay flow end to end.

The proof system is designed to be swapped without restructuring the protocol. The `IProofSystem` interface treats ZK, ECDSA, and multisig systems as interchangeable, and each rollup's contract holds its own set of accepted proof systems and its own M-of-N threshold. The repo ships an ECDSA proof system annotated as temporary and development-grade. Near-term, Gnosis Chain plans to start with a BLS validator multisig as its proof system (an M-of-N attestation from the validator set) and move to zk over time. On the zk side, ZisK has reached 1.0 alpha: an open-source RISC-V 64-bit zkVM, post-quantum, 128-bit security, now in audit and formal verification. Production zk proving is still the target; the near-term proof is a validator multisig, not ECDSA alone.

Mainnet is not live. The prototype runs on the Chiado testnet, and the plan to make Gnosis Chain the first EEZ-compatible L2 is still pre-GIP. So the mechanism is demonstrated, but not yet in production.

For DeFi builders, the architecture is worth understanding now. The settlement model is different from every async cross-chain approach. As the proof systems harden and batch latency tightens, the composability surface opens up considerably, and the flash loan is the sharpest illustration of why.

---

*Armagan Ercan is Coordinator at EEZ.*
