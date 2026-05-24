---
title: EEZ Smart Contract Overview — Community Call Summary (X Thread)
source: Community Call #1, May 13 2026
speaker: Jesus Ligero (@JesusLigero3), ZisK (@ziskvm)
published: May 2026
format: X/Twitter thread
status: final
---

If you're a developer thinking about building on or integrating with EEZ, this is the thread to read.

EEZ had its first community call last week. @JesusLigero3 from @ziskvm walked through the smart contract architecture — first time it's been explained publicly in this level of detail.
Here's how the contracts actually work.
EEZ is a framework for rollups to settle together on Ethereum and actually compose — L1 calls L2, gets a return value, same transaction.
L2s solved scaling. they didn't solve fragmentation. Every rollup is its own island right now — different bridges, different liquidity, different UX. What EEZ is trying to build is a shared settlement layer underneath all of them.

One contract that speaks to all of them.

---

## 1. Rollup Registration

The registration model is simple and that's intentional. You deploy a rollup contract, register it with EEZ, and declare your accepted proof systems via cryptographic fingerprints that define what a valid proof looks like for your chain. EEZ doesn't mandate anything — ZK, multisig, multi-prover, your choice.

Nice property: if multiple rollups share the same prover, they share one proof. Five rollups, one ZK proof — verification cost splits across all five. That's a real scalability property built into the registration design.

---

## 2. Settlement happens through one function: postAndVerifyBatches

Settlement happens through one function: postAndVerifyBatches. A sequencer calls it with the batch data and all required proofs. EEZ checks each rollup's requirements, calls each verifier in turn, and collects the result. Every verifier has to approve. One rejection anywhere reverts the entire thing — no partial settlement.

---

## 3. After verification: state updates + ExecutionEntry

The thing to understand about ExecutionEntry: the proof already ran. L2 execution is done and verified before any L1 interaction happens. The entry is the committed outcome — EEZ is saying "these are the L1 calls we expect next, and here are their return values."

When those L1 calls come in, they just match against it. The result was locked cryptographically before the L1 transaction was made.

---

## 4. User <> CrossChainProxy <> EEZ

To make this invisible to users, EEZ uses CrossChainProxies. Any address on any rollup can have a proxy deployed on L1. Permissionless and permanent once deployed. Calling it looks like calling any other contract on Ethereum — same interface, no special handling.

Under the hood: proxy forwards to EEZ → EEZ hashes the call params → finds the matching ExecutionEntry → updates rollup state → returns the committed value.

User sees: one tx, one chain, instant return value. They have no idea the execution happened on L2. They interacted with what looked like an Ethereum contract — the logic just ran on L2.

From the user's side:
one transaction.
one chain.
instant result.
no bridge.
no cross-chain signing.
That's the UX goal and it's genuinely elegant when you see how it's constructed.

---

## 5. Reentrant L1→L2 with expectedL1ToL2Calls

Reentrant case is where it gets interesting.

L1 → L2 → L1 → L2. a user calls a proxy, that hits a contract on Rollup A, that contract calls back to an L1 contract, that L1 contract calls into another rollup. EEZ handles the whole chain.

The ExecutionEntry's L2ToL1Calls array holds every outbound call made during L2 execution. EEZ replays them in order after the initial proxy call. When an L1 contract reenters a proxy, EEZ matches it against expectedL1ToL2Calls and returns the committed value.

Depth doesn't change the trust model. Once the ZK proof covers the full trace, adding another hop is the same operation. All the complexity is on the prover, invisible to the user.

---

## 6. Full system overview

The last slide ties it together. Jesus opened with it and came back to it at the end — makes a lot more sense the second time.

sequencer → postAndVerifyBatches → EEZ (rollup states + ExecutionEntry[]) ↔ rollup contracts (vKeys + hooks) → proof systems. users hit CrossChainProxies, EEZ handles the rest.

Sequencer posts batches.
Rollup contracts define their accepted provers.
Proof systems each verify their chunk of the batch.
EEZ settles state atomically.

What this enables that isn't possible today: flash loan on L1 + liquidation on a rollup + repayment back to L1, atomic, single tx. no solver fronting capital, no bridge delay, no extra trust assumption.

If any of this was interesting, the full community call recording is worth watching — Jesus goes through each diagram live.

If you are interested in further details, please ping me; I will get in touch with you regarding the details and the workshop we are planning for June at @dappcon.
