---
title: EEZ GitHub Codebase — X Thread
format: X/Twitter thread
voice: Armagan Ercan, personal account
status: draft
tweets: 6
source: eez-association GitHub org (primary-source verified, 2026-06-05)
---

[1/6]
EEZ has had a lot of coverage. Most of it got the technical details wrong. I went through the actual code in github.com/eez-association. Three repos, recent commits, real implementation. Here's what's actually there.

---

[2/6]
The most interesting thing in the sync-rollups-protocol codebase isn't what was added. It's what was removed. ActionType enum, scope arrays, recursive reentrancy handling — all gone. Cross-chain calls are now resolved by consuming pre-computed entries sequentially, one at a time. The complexity is on the prover. The on-chain contract just walks through a committed list.

---

[3/6]
sync-rollups-composer has a derivation spec worth reading. The formula for every L2 block timestamp is deterministic from L1 data alone: l2_timestamp = deployment_timestamp + ((l2_block_number + 1) × 12s). BatchPosted is the only event you need. Any node can reconstruct the full chain without trusting the sequencer.

---

[4/6]
The README documents a verbatim use case: borrow on Rollup A, use the funds on Rollup B, repay on Rollup A — atomic within a single L1 block. No bridge delay. No solver fronting capital. Either the full trace is verified and all state updates apply, or none of them do.

---

[5/6]
On ZK: every repo describes ZK proofs as the production target. The current implementation is ECDSA. ECDSAProofSystem.sol is annotated "Temporary proof system." IProofSystem already supports ZK and ECDSA interchangeably — the upgrade path is designed in. No production ZK verifier exists in any repo today.

---

[6/6]
The formal specs are the most complete public documentation of a synchronous based-rollup system on Ethereum right now. SYNC_ROLLUPS_PROTOCOL_SPEC.md and DERIVATION.md are worth the read if you work on L2 architecture. Full write-up: [blog link]
