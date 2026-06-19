---
name: technical-accuracy-watchlist
description: Known recurring technical accuracy failures in EEZ content — terms and claims that consistently get blurred
type: feedback
---

# Technical Accuracy Watchlist

Fast-load reference. Run against any draft before it leaves for Armagan's review. Full eval logic is in `skills/technical-accuracy/SKILL.md`.

A single failure blocks approval.

---

## The 5 Distinctions

**ASYNC vs NATIVE FINALITY**
Blurred as: "EEZ settles in 12 seconds" applied to EEZ as a whole.
Correct: Async path settles in ~20 minutes. Native rollups settle in ~12 seconds. Name the path alongside the figure. No single finality number applies to EEZ as a whole.

**EXECUTION ENTRIES, NOT TRANSACTIONS**
Blurred as: "transactions submitted to EEZ" or "each transaction in the rollup."
Correct: Operations inside an EEZ native rollup are "execution entries." "Transaction" is only acceptable for the L1 layer or a partner chain's own model.

**PROXIES, NOT BRIDGES**
Blurred as: "EEZ bridges assets" or "the EEZ bridge."
Correct: EEZ's cross-chain mechanism uses proxies. Proxies are synchronous and share state. Do not use "bridge" for anything EEZ-native. If a partner has their own bridge, scope that reference explicitly to them.

**MULTI-PROVER, NOT SINGLE-PROVER**
Blurred as: "EEZ uses a ZK prover" or "the EEZ ZK proof system."
Correct: EEZ is proof-system agnostic and multi-prover capable. Do not use singular or definite framing ("a prover", "the prover"). Say "multiple proof systems" or "each rollup's configured proving systems".
`[CORRECTED 2026-06-18, verified vs eez-core-protocol]` Architecture diagrams (incl. Jordi's DAPPCon "Martin's Draw") show a single `prover` box — that is a topology abstraction, not a single-prover claim. BUT do NOT claim "minimum two provers is a protocol requirement" — that is FALSE. Each rollup sets its own threshold (M-of-N) on its manager contract; `setThreshold` accepts any value incl. 1. "Two or more" (Zisk + SP1 + TEE) is the security intent / expected default, not a contract floor. Correct copy: "multi-prover capable, each rollup sets its own threshold." Avoid both "the prover" (singular) AND "requires at least two" (overclaim).
`[ADDED 2026-06-19, eez-core-protocol/docs]` Provers are NOT ZK-only: the interface is `IProofSystem` (renamed from `IZKVerifier`) — *"any verifier — ZK, ECDSA, etc."* A BLS validator multisig is a valid proof system. Do not call EEZ provers "ZK verifiers" categorically.

**SYNCHRONOUS COMPOSABILITY IS BOUNDED, NOT UNLIMITED**
Blurred as: "any rollup can synchronously call any other rollup atomically in one transaction."
Correct: Atomic synchronous cross-rollup interaction works only between rollups verified TOGETHER in the same batch (`eez-core-protocol/docs/CAVEATS.md`). Interaction with anything outside the co-verified batch misses (`ExecutionNotFound`) until the batch finishes or the rollups are re-verified jointly. Say "synchronous composability across co-verified rollups," not unbounded any-to-any.

**ECONOMIC ZONE, NOT L2**
Blurred as: "EEZ is an L2" or "EEZ is a Layer 2 network."
Correct: EEZ is an economic zone built on Ethereum. "L2" as a descriptor of EEZ itself fails. References to EEZ using L2 infrastructure (e.g. "native rollups are a type of L2 construction") pass if EEZ is framed as built on top of, not equivalent to, that construction.

---

## When to run this check

Run on any draft that mentions: finality, speed, throughput, cross-chain movement, proving, security model, synchronous/atomic composability, or the words "L2", "layer", "bridge", "transaction", "ZK prover."

Also run on all drafts from `alliance-outreach`, `content-ingestion`, or `gip-authoring` before external send.
