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
Correct: EEZ supports multiple proof systems in parallel. Do not use singular or definite framing ("a prover", "the prover"). Say "multiple proof systems" or avoid singular framing entirely.
`[NEW 2026-06-18]` Architecture diagrams (incl. Jordi's DAPPCon "Martin's Draw") show a single `prover` box — that is a topology abstraction, not a single-prover claim. Minimum two proving systems (e.g. Zisk + SP1 + TEE) is a protocol requirement. Do not let a single-box diagram justify singular framing in copy.

**ECONOMIC ZONE, NOT L2**
Blurred as: "EEZ is an L2" or "EEZ is a Layer 2 network."
Correct: EEZ is an economic zone built on Ethereum. "L2" as a descriptor of EEZ itself fails. References to EEZ using L2 infrastructure (e.g. "native rollups are a type of L2 construction") pass if EEZ is framed as built on top of, not equivalent to, that construction.

---

## When to run this check

Run on any draft that mentions: finality, speed, throughput, cross-chain movement, proving, security model, or the words "L2", "layer", "bridge", "transaction", "ZK prover."

Also run on all drafts from `alliance-outreach`, `content-ingestion`, or `gip-authoring` before external send.
