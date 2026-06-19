# Proxies and the Execution Table

![L1 proxies write to the Execution Table; one shared state across the divider](../diagrams/02-proxies-and-execution-table.png)

*EEZ architecture explainers, 2 of 7. For builders and Alliance partners.*

Source: `knowledge/eez/sources/dappcon-2026-eez-node-architecture.md` (DAPPCon 2026 EEZ workshop, Jordi Baylina, 17 June 2026). Quote the mechanism, not the brand. EEZ is not deployed yet. Nothing here is something you can run today.

---

## The one idea to take away

A contract living on one rollup can be called by a contract on another rollup as if both sat on the same machine. The call is a normal Ethereum `CALL`. The reply is a normal `RETURN`. No asset gets wrapped. No message gets relayed. No third party gets trusted to carry the message across.

EEZ does this with **proxies** and an **Execution Table** on L1. This explainer makes that pairing concrete. It is the heart of "proxies, not bridges".

---

## What a proxy is

The Ethereum Economic Zone is not an L2. It is an economic zone built on Ethereum. Inside it, rollups stay sovereign. Each rollup keeps its own state, its own rules, and its own accepted proof systems.

When a contract on one rollup needs to be reachable from another, EEZ represents it on L1 as a **proxy** of the real contract. The deck draws proxies with a star. `UserAA(*)`, `Whitelist(*)` and `DAO(*)` are the L1 proxies. The real `UserAA`, `Whitelist` and `DAO` live on the rollup (the deck uses R2 as the example).

Read the star as "this is the L1 stand-in for a contract that actually executes elsewhere". The proxy is not a copy of the contract's logic. It is the synchronous reference point that lets L1, and other rollups through L1, address that contract and share its state.

Two properties matter here, and they are exactly what a bridge does not give you.

**Synchronous.** A proxy resolves within the same proving window as the call that touched it. It does not wait for a separate confirmation cycle on another chain.

**Shared state.** The proxy and its real contract refer to one state, not two reconciled copies. There is no source-of-truth contract on one side and a mirror on the other.

---

## What the Execution Table is

The Execution Table is a structure on L1 that records the cross-chain calls and returns as they happen.

When the L1 `DAO(*)` proxy takes part in a cross-chain interaction, it writes the call, and later the return, into the Execution Table on L1. The table is the on-L1 ledger of every context switch between chains for that combined execution. A `CALL` from chain 1 into a contract on chain 2 is an entry. The matching `RETURN` is an entry. A revert is recorded too.

Think of the Execution Table as the shared notebook that all participating chains write into on L1. Because the notebook lives on L1, every chain agrees on what happened without any chain having to sync another chain's full state. A node that follows one rollup builds its state from L1 and its own chain only.

The deck pairs this with the proving side. The combined execution of many rollups is proved as a single, synchronous transaction. The EEZ Trace (the blob format) records each context switch, and the proof attests that the whole thing, across all the chains involved, is valid together. The Execution Table is the L1-visible face of that combined execution.

---

## Why this is not a bridge

A bridge moves value or messages between two chains that do not share state. To do that, a bridge wraps assets, relays messages, and asks you to trust whatever relays them. The two chains stay separate. They reconcile after the fact.

EEZ does none of that.

No wrapped assets. ETH stays ETH. For native-ETH rollups, a `CALL` can carry ETH directly. There is no wrapped representation to mint and burn.

No message relay. The interaction is a plain `CALL` and `RETURN` between smart contracts, written into the Execution Table on L1. There is no separate messaging layer carrying a payload from A to B.

No relay trust. You are not trusting a set of validators or a multisig to attest that a message crossed. The combined execution is proved by the proving systems each rollup configures. EEZ is proof-system agnostic and built for multiple independent proofs (for example Zisk plus SP1, or a proof system plus a TEE), with each rollup choosing its own systems and threshold. The trust rests on proofs, not on a relayer or a committee.

Shared state, not reconciled state. Because the proxy and its real contract share one state through L1, there is nothing to reconcile later. The deck states the security goal plainly: the same security Ethereum gives independent smart contracts that call each other.

So the precise framing is this. A bridge connects two separate states. A proxy exposes one state in two places. That is the whole difference, and it is why "bridge" is the wrong word for anything EEZ-native.

---

## Cross-rollup ETH accounting

Native rollups can include ETH inside a cross-rollup `CALL`. The value does not get wrapped, so EEZ needs a way to keep the books straight when ETH moves between rollups.

It does this with **rollup-level ETH accounting maintained in L1**. The accounting tracks how much ETH each native-ETH rollup holds relative to the others, and it lives on L1 where every chain can see it. A cross-rollup ETH transfer updates that L1-level accounting rather than minting a wrapped token on the receiving side.

L1 is the settlement point and the shared record. The ETH itself stays native on each side. The accounting on L1 keeps the totals honest across rollups.

---

## What a developer sees versus what happens underneath

What you write looks ordinary. Your contract on rollup A calls a contract on rollup B. In your code it is a `CALL` to an address, with a `RETURN` coming back, and ETH attached if you need it. You do not write bridge glue, you do not mint or burn a wrapped token, and you do not wait on a relayer.

What happens underneath. On L1, the target contract is represented by its proxy (the starred contract). The proxy writes the `CALL` into the Execution Table on L1. The combined execution across the involved rollups is proved together by the rollups' configured proving systems, with each context switch recorded in the EEZ Trace. The `RETURN` is written back into the Execution Table, and any ETH movement updates the rollup-level accounting on L1.

The point of the design is that the second paragraph stays out of your way. You get a cross-chain call that reads and behaves like a same-chain call, with shared state and no wrapped assets, because the proxy and the Execution Table do the work on L1.

---

## A note on timing

How long settlement takes depends on the path, so always name the path beside any figure.

The **native path** between native rollups targets roughly 12 seconds, in line with an L1 slot. The deck describes bundle building and proof building running overlapped with a target under 3 seconds, so the cross-chain interaction can fit inside a slot.

The **async path** is the slower route, roughly 20 minutes.

There is no single finality number for EEZ as a whole. Quote ~12s for native and ~20 min for async, and say which one you mean.

---

## Accuracy notes

- **Proxies, not bridges.** This is the load-bearing distinction. Proxies are synchronous and share state. Do not call any EEZ-native mechanism a bridge. If a partner runs their own bridge, scope that to them explicitly.
- **No wrapped assets, no message relay, no relay trust.** These are the three things a bridge needs and a proxy does not. ETH stays native.
- **Execution entries, not transactions, inside a rollup.** "Transaction" is used here only for the L1 layer. Calls and returns recorded in the Execution Table are entries in the combined execution, not L1 transactions in their own right.
- **Proof-system agnostic and multi-prover-capable.** EEZ is proof-system agnostic and built for multiple independent proofs. Each rollup sets its own threshold (one or more) on its manager contract, so there is no protocol-enforced minimum of two. Avoid singular framing for EEZ as a whole ("a prover", "the prover"), but do not claim a contract floor of two. Diagrams that show one prover box are a topology abstraction, not a single-prover claim.
- **Async ~20 min vs native ~12s.** No single finality number applies to EEZ. Name the path beside the figure.
- **Economic zone, not an L2.** EEZ is an economic zone built on Ethereum. Native rollups are an L2 construction EEZ builds on, not a description of EEZ itself.
- **Not deployed yet.** EEZ is pre-deployment. Roadmap milestones include Composer 1.0, Chain Zero, and connecting Gnosis Chain. Do not imply anyone can participate today.
- **Source framing.** The deck is Jordi Baylina's engineering-level founding material. Treat it as the mechanism's design, not as approved EEZ comms.
