# The EEZ Proof: Synchronous Cross-Rollup Execution

*Source: `knowledge/eez/sources/dappcon-2026-eez-node-architecture.md` (DAPPCon EEZ Workshop, 17 June 2026, Jordi Baylina). Engineering-level founding material. Quote as Jordi's framing, not as approved EEZ comms. EEZ is at roadmap stage and is not deployed yet.*

This explainer is for builders and partners who want to understand the core technical claim behind the Ethereum Economic Zone (EEZ): that it proves the combined execution of many rollups as a single, synchronous transaction. It explains what that sentence means, how the proof model works, why it counts as synchronous composability, what security it claims, and how it differs from bridging.

EEZ is an economic zone built on Ethereum. It is not an L2, and it is not equivalent to any single rollup. It is the shared proving and settlement layer that lets independent rollups call into each other as if they were one machine.

## The core claim

Jordi Baylina states it plainly in the DAPPCon deck: "EEZ proves the combined execution of many rollups as a single, synchronous transaction."

Read that carefully. The unit of proof is not one rollup. It is the combined execution across several rollups, treated as one atomic step. When a contract on one rollup calls a contract on another, both sides of that interaction land in the same proven batch. They either both happen or neither does.

This is the difference that matters. Most cross-chain systems today break an interaction into two separate events on two separate timelines. EEZ keeps it as one event.

## The CALL and RETURN model

The mechanism is a normal Ethereum CALL and RETURN between smart contracts, except the contracts live on different chains.

A contract on Chain 1 issues a CALL to a contract on Chain 2. Chain 2 runs the called function. Chain 2 issues a RETURN back to Chain 1. From the calling contract's point of view, this looks exactly like calling any other contract. It does not know, and does not need to know, that the callee sits on a different rollup.

On L1, the participating contracts exist as proxies of their real counterparts on the rollup. In the deck's notation these proxies are drawn with a star: `UserAA(*)`, `Whitelist(*)`, `DAO(*)`. The proxy is the L1 stand-in for a contract whose real state lives on the rollup. The relevant L1 proxy writes the cross-chain calls and returns into an Execution Table on L1. That Execution Table is the concrete record of who called whom and what came back.

These are proxies, not bridges. The distinction is load-bearing. Proxies are synchronous and they share state. A bridge does neither.

## Why this is synchronous composability

Synchronous composability means the call and its result resolve inside a single proven step, with shared state, before anything settles. The caller gets its answer in the same atomic unit in which it asked the question.

Asynchronous message passing works differently. Chain A emits a message. Some relayer or light client picks it up later. Chain B acts on it in a future block. Chain A may learn the outcome in yet another block after that. Each hop is a separate transaction on a separate timeline, and the gap between them is where bridge risk, replay risk, and stale-state risk live.

EEZ collapses those hops. The CALL, the execution on the other chain, and the RETURN are all captured in one EEZ Trace (the deck calls this "the blob format"), which records each context switch between chains, including how reverts are handled. The proving pipeline then proves that whole trace as one thing. The composer builds the cross-chain bundle and the proof is built over it, with the overlapped bundle-building and proof-building targeting under three seconds so the result fits inside a single L1 slot.

Because the interaction is proven as one step, there is no window in which Chain 1 has acted but Chain 2 has not. State is shared across the step rather than copied between chains after the fact.

## The security claim

The deck's properties list is precise about what EEZ guarantees here. Property 6 reads: security is "warranted across rollups," giving "the same security Ethereum provides to independent smart contracts calling each other."

This is a deliberately concrete benchmark. When contract A calls contract B on Ethereum L1 today, you do not worry that B might receive the call but A never sees the return, or that someone replays the message. The EVM guarantees the call and its result resolve together under one consensus. EEZ aims to extend that same guarantee across rollup boundaries.

The guarantee rests on proof, not on trusted intermediaries. EEZ is proof-system agnostic (ZK, TEE, or multisig are all permitted) and the contract enforces a minimum of two proving systems per rollup. The DAPPCon node diagram shows a single `prover` box, but that is a topology abstraction. The on-chain configuration requires at least two proving systems (for example Zisk plus SP1 plus a TEE) and reverts if fewer are configured. The security of a cross-rollup call therefore rests on multiple independent proofs agreeing, not on one prover or one relayer.

## How this differs from bridging

A bridge moves an asset or a message from one chain to another. It locks or burns on the source side, then mints or releases on the destination side, usually mediated by a set of validators, a light client, or a trusted committee. The two sides are linked by a message, and the link is only as strong as whatever secures that message. The action is asynchronous by construction: source first, destination later.

EEZ does not move anything between chains in that sense. The contracts call each other directly through proxies, and the combined result is proven as one synchronous transaction. There is no separate trusted party sitting between the chains to relay a message, and there is no period where the asset or call is in flight between two independent confirmations.

One accuracy note on speed, because it is easy to blur. EEZ does not have a single finality number. A native rollup interaction settles on the native path in roughly twelve seconds. The async path settles in roughly twenty minutes. When you quote a figure, name the path beside it. Synchronous composability is about the call and return resolving in one proven step, which is a separate property from how long final L1 settlement of a given path takes.

## A worked example

Take the deck's own example: a user account on one rollup wants to vote in a DAO that lives on another rollup, and the DAO only accepts votes from whitelisted accounts.

On Rollup R2 you have three real contracts: `UserAA` (the user's account), `Whitelist` (the eligibility check), and `DAO` (the governance contract). On L1 these appear as proxies: `UserAA(*)`, `Whitelist(*)`, `DAO(*)`.

The flow runs as a chain of normal CALLs and RETURNs:

1. `UserAA` issues a CALL to `DAO` to cast a vote.
2. `DAO` issues a CALL to `Whitelist` to check that `UserAA` is eligible.
3. `Whitelist` runs its check and issues a RETURN with the answer.
4. `DAO` records the vote and issues a RETURN to `UserAA`.

Each of these CALL and RETURN context switches is written into the Execution Table on L1 and captured in the EEZ Trace. The whole sequence, the vote, the eligibility check, the recorded result, is proven as one synchronous transaction by at least two proving systems.

The outcome is atomic. If the whitelist check fails, the vote does not get recorded, and the revert is captured in the trace. There is no state in which the DAO has half-processed a vote from an account it never confirmed was eligible. With a bridge-and-message design, the eligibility check and the vote would be separate cross-chain hops, and the gap between them would be a source of risk. Here there is no gap, because there is no message in flight. There is one proven step.

For clarity, the operations happening inside each native rollup are execution entries, not "transactions." Transaction is the right word for the L1 layer. Inside a native rollup, the units are execution entries.

## Why builders should care

If you build across multiple rollups today, you carry the cost of asynchrony yourself. You write relayer logic, you handle the case where one side confirmed and the other did not, and you trust whatever bridge sits in the middle. EEZ's claim is that you can stop doing that and write a plain contract-to-contract call, with the cross-rollup interaction proven atomically under Ethereum-grade security.

EEZ is not live yet. The roadmap runs through smart-contract cleanup, an audit, the signature and Zisk proving systems, Composer 1.0, Chain Zero, and then connecting Gnosis Chain. So this is a design to build against and plan for, not a network you can join today. The architecture above is what that network is being built to deliver.

## Accuracy notes

These guardrails were relevant to this document and were respected as follows:

- **Economic zone, not L2.** EEZ is described throughout as an economic zone built on Ethereum and the shared proving and settlement layer, never as an L2 or a single network.
- **Async vs native finality.** No single finality figure is applied to EEZ as a whole. The native path (~12 seconds) and the async path (~20 minutes) are named with their figures, and finality is kept distinct from the synchronous-composability claim.
- **Proxies, not bridges.** The cross-chain mechanism is described as proxies, which are synchronous and share state. The bridging section contrasts EEZ with bridges rather than calling any EEZ mechanism a bridge.
- **Multi-prover, not single-prover.** Proving is always described in the plural (minimum two proving systems per rollup). The single `prover` box in the DAPPCon diagram is flagged as a topology abstraction, not a single-prover claim.
- **Execution entries, not transactions.** Operations inside a native rollup are called execution entries. "Transaction" is reserved for the L1 layer and for the deck's own phrasing of the combined-execution claim.
- **Not deployed yet.** The document states EEZ is at roadmap stage and explicitly says builders cannot join today, with the roadmap milestones named.
