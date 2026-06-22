# Conventions & Caveats

The shared reference for the EEZ Architecture Explainer series. Every explainer links here once instead of repeating these points. If a claim in an explainer seems to contradict this page, this page wins.

> **Status: read first (verified 17 June 2026).** EEZ is **not in production**. Frame all participation as a roadmap question. The contracts are in a **"semi-finalized" state**: working, with many tests passing, but another iteration (gas savings, less complexity) is planned before anything is declared final. What is live now: an internal **Devnet** plus a **Chiado** (Gnosis testnet) deployment running the most-recent contracts with **partial functionality**. **End of summer:** a deploy on Ethereum mainnet itself, communicated as a **testnet**: "use at your own risk," only "slightly audited," "do not use with real money." **Rollup 0** deploys in **August**, then hardens into **Rollup 1** (the fully-based reference implementation). **Gnosis Chain** becomes the first EEZ L2 **optimistically by end of year**, in **LIMITED capacity** (synchronous single calls, **not** unlimited nested back-and-forth) with a **prover compromise** (not purely ZK; partially ZK plus a multisig/TEE). Source material is Jordi Baylina's and Phillipe Schommers' DAPPCon Berlin 2026 sessions. Quote it as their engineering-level framing, **not** as approved EEZ comms. Any externally published version is a draft until Armagan signs off.

## Canonical timing model

There is **no single finality number for EEZ**. Always name the path beside any figure.

| Path / metric | Figure | What it means |
|---|---|---|
| Native path | ~12 s | A native-rollup interaction settling, in line with an L1 slot. |
| Async path | ~20 min | The slower route, used when an interaction crosses to L1 without native coupling. |
| Proof-generation target | < 3 s | The proof budget *inside one L1 slot* so a synchronous step lands in a slot. **Not a finality number.** |

The < 3 s target is the minimal work left at the end of an otherwise continuous, overlapped pipeline (see [Explainer 7](07-real-time-proving-zisk.md)). It is not total proving time.

In the workshop's spoken usage, "async" referred to cheap static reads against a possibly-stale L1 header, and the ~12–20 min figure also surfaced as the stall of a full-chain *pessimistic* lock. The table figures above are code-verified and stand.

## The five distinctions (enforced throughout)

1. **Async vs native finality.** Use the table above. Never attach one finality number to EEZ as a whole.
2. **Execution entries, not transactions.** Operations *inside a native rollup* are execution entries. "Transaction" is reserved for the L1 layer and for partner chains (based, centralized-sequencer, validium) that run their own transaction model, scoped to them.
3. **Proxies, not bridges.** Any EEZ-native cross-chain mechanism is a proxy: synchronous, shared state. Never call it a bridge. A partner's own bridge is scoped to them explicitly.
4. **Proof-system agnostic, multi-prover-capable.** Each rollup sets its own threshold (an M-of-N choice, one or more) on its **own manager contract**. There is **no protocol-enforced minimum of two**. The manager reverts `ThresholdNotMet` below the rollup's chosen threshold. A single `prover` box in any diagram is a topology abstraction, not a single-prover claim. Avoid singular framing ("the EEZ prover") for the zone as a whole, but do **not** claim a contract floor of two.
5. **Economic zone, not an L2.** EEZ is built on Ethereum and is not equivalent to an L2. Native rollups are an L2-style construction EEZ builds *on top of*, not a description of EEZ itself.

## Verification status

Conceptual claims (proxies not bridges, execution entries, `postAndVerifyBatch`, sovereign rollups, the per-rollup proving threshold) were verified **18 June 2026** against `eez-association/eez-core-protocol` (branded "Sync Rollups"). The repo is early-stage and unaudited; interfaces may shift.

The **EEZ BuilderRoom workshop recording (17 June 2026, Friederike / Martin / Jordi)** is a corroborating primary source for Explainers 1–6 and the status note above.

Two scope notes that override any broader reading:
- **Chain types** (custom native, based/centralized, validium) describe the deck's *vision*. The shipped code currently scopes synchronous composability to **based rollups sharing the same L1 sequencer**. Read each chain type as a roadmap question. (See [Explainer 4](04-chain-types-and-coordination.md).)
- **ADSTF** is the deck's conceptual term. The shipped contracts express it as execution entries, state deltas, and a rolling hash. There is no type literally named ADSTF. (See [Explainer 7](07-real-time-proving-zisk.md).)

See [GLOSSARY.md](GLOSSARY.md) for term definitions and [STYLE-CHECKLIST.md](STYLE-CHECKLIST.md) for the reviewer's per-document checklist.
