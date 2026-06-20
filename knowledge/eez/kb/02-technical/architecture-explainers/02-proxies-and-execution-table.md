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

## Cross-rollup ETH transfers

Native rollups can include ETH inside a cross-rollup `CALL`. The value does not get wrapped, so the transfer rides on the call itself rather than on a separate ledger.

A cross-rollup ETH transfer is a plain `CALL` carrying value. You send ETH to the proxy on L1, then make the call with value attached, and the EEZ contract records that interaction in the Execution Table. There is no dedicated per-rollup ETH ledger tracking balances; the movement is just another entry in the table, like any other call and return.

L1 is the settlement point and the shared record. The ETH itself stays native on each side. The Execution Table keeps the record honest across rollups.

---

## What a developer sees versus what happens underneath

What you write looks ordinary. Your contract on rollup A calls a contract on rollup B. In your code it is a `CALL` to an address, with a `RETURN` coming back, and ETH attached if you need it. You do not write bridge glue, you do not mint or burn a wrapped token, and you do not wait on a relayer.

What happens underneath. On L1, the target contract is represented by its proxy (the starred contract). The proxy writes the `CALL` into the Execution Table on L1. The combined execution across the involved rollups is proved together by the rollups' configured proving systems, with each context switch recorded in the EEZ Trace. The `RETURN` is written back into the Execution Table, and any ETH movement rides on the call as value, recorded in the same table.

The point of the design is that the second paragraph stays out of your way. You get a cross-chain call that reads and behaves like a same-chain call, with shared state and no wrapped assets, because the proxy and the Execution Table do the work on L1.

---

## How the proxy gets its answer: the composer and the 3-tx bundle

A proxy on L1 does not hold the contract's logic. So when it is called, where does its return value come from? The **composer** supplies it ahead of time.

The composer anticipates the call, simulates it on the L2, and pre-loads a **lookup table** so the proxy can return the right value. It packages this as a bundle of up to **three transactions**:

1. Deploy the proxy, if it does not already exist.
2. Load the lookup table.
3. The user transaction, which hits the proxy, looks up its answer, and returns it.

Ordering inside the L1 block matters. The composer's transaction (carrying the proof and the lookup table) lands **first**, and the user transaction lands **second**. Only the user transaction triggers the L2 state change. If there is no user transaction, nothing happens on the L2.

---

## The one caveat: calling a proxy outside the bundle reverts

There is a single sharp edge to know about. A proxy call made **outside the composer bundle reverts**, because the lookup is not found.

This breaks any spec that must not revert. A `view` function like ERC-20 `balanceOf` is the obvious case: callers assume it always returns, and a revert there is a problem. As Jordi put it, depending on a revert for your logic is a design flaw.

There is also **no public L1 mempool** for these calls. You cannot fire a proxy call from the open mempool and expect it to resolve. You route it through the composer, a bundle, or account abstraction. Plan for that path from the start.

---

## Who pays, and the gas picture

The **user pays**, in the normal case. The user submits a small transaction that calls the proxy, plus a larger tip. The composer fronts the cost of the two extra transactions in the bundle, sends the three-transaction bundle to the builder, and the builder returns enough to the composer to cover them.

This carries a **trust assumption**. A builder could include the user transaction without the composer transaction, in which case it reverts. The L1 protocol does not enforce that the two stay together.

There is a cheaper path. With **account abstraction**, the whole sequence runs in **one transaction** using transient storage, and the user pays only if it does not revert. Rough figures from the workshop: the full real-storage version is around **400K gas**, a normal L1 to L2 call is around **300K gas**, and the AA / transient-storage path is roughly **10x cheaper**. Always name the path beside the figure.

---

## A note on the proxies themselves

The proxies are **stateless forwarders**. The bytecode encodes the EEZ contract plus the `(address, chain)` the proxy stands for, and nothing more. There is no storage and no locally stored owner; the owner is held in the EEZ contract. The proxy forwards any ETH it receives immediately, and it **cannot self-destruct**.

Two identity points follow from this. When you call from L1, on the L2 the **`msg.sender` is the proxy of your EOA**, not your EOA directly. A system address calls the proxy, and the proxy is what the target contract sees. Each chain is a kind of domain name, and every address has a **deterministic alias (proxy) per chain**, fixed by `(address, chainID)`. Your L1 EOA keeps its own nonce; the L2 proxy has no real nonce of its own. Nonces stay on the originating chain.

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
