---
title: EEZ GitHub Codebase — Technical Reference
source: github.com/eez-association (sync-rollups-protocol, sync-rollups-composer, eez-rollup0)
date: 2026-06-06
verification: adversarial multi-agent review — 99 agents, 25 claims checked, 13 confirmed, 12 killed
confidence: high (primary source code only)
status: verified
---

## Repo Inventory

| Repo | Stack | Last Push | Role |
|---|---|---|---|
| sync-rollups-protocol | Solidity + Foundry | 2026-06-05 | L1 contracts, formal specs |
| sync-rollups-composer | Rust + reth (upstream pin) | 2026-05-02 | Based rollup node, derivation logic, 529 tests |
| eez-rollup0 | Rust + Engine API | 2026-06-05 | Reference sequencer implementation |

---

## Verified Technical Claims

**C1 — Atomic cross-rollup execution**
- Claim: Pre-computed state transitions + single ZK proof per batch via postBatch() enable atomic cross-rollup calls within a single L1 block.
- Source: sync-rollups-protocol / README, IEEZ.sol, EEZ.sol
- Confidence: high (3-0 + 2-1 verified)

**C2 — Flat sequential execution model**
- Claim: ActionType enum, scope[], RESULT/REVERT/REVERT_CONTINUE action types, and recursive scope navigation are removed. Cross-chain reentrancy resolved by consuming ExpectedL1ToL2Call entries via non-recursive while loop in _consumeNestedAction.
- Source: sync-rollups-protocol / CHANGES_FROM_PREVIOUS.md, SYNC_ROLLUPS_PROTOCOL_SPEC.md §D
- Confidence: high (2-1 + 3-0 verified; removed symbols grep-confirmed absent)

**C3 — Derivation from L1 alone**
- Claim: Full L2 chain re-derivable from BatchPosted events alone. Formula: l2_timestamp = deployment_timestamp + ((l2_block_number + 1) × 12s). Rollups.sol is canonical source of truth. reth is upstream pin (paradigmxyz/reth), not a fork.
- Source: sync-rollups-composer / DERIVATION.md, Cargo.toml
- Confidence: high (2-1 + 3-0 verified)

**C4 — Proof system status**
- Claim: Current proof mechanism is development-grade ECDSA only (tmpECDSAVerifier). ECDSAProofSystem.sol annotated "Temporary proof system." IProofSystem interface supports ZK/ECDSA interchangeably. MockZKVerifier.sol is test stub (accepts all proofs, development only). No production ZK verifier exists.
- Source: sync-rollups-protocol / ECDSAProofSystem.sol, IProofSystem.sol, MockZKVerifier.sol
- Confidence: high (3-0 verified)

**C5 — eez-rollup0 parameters**
- Claim: L2 block time 2 seconds (BLOCK_TIME=2s). Batch interval 60 seconds (DEFAULT_INTERVAL_SECS=60). Sequencer drives reth exclusively via forkchoiceUpdated + newPayload. Stage 2 batches contain L2 user transactions only; cross-chain entries deferred to Stage 4.
- Source: eez-rollup0 / main.rs, config.rs, composer.rs, block_committer.rs
- Confidence: high (3-0 + 3-0 verified)

**C6 — Cross-chain flash loan use case**
- Claim: Borrow on Rollup A, use on Rollup B, repay on A, atomic within one L1 block. Documented verbatim in README.
- Source: sync-rollups-protocol / README
- Confidence: high (part of C1 verification)

**C7 — RIPs are optional**
- Claim: "All RIPs are optional. RIPs and RollCall are not a governance process." EVM-equivalent rollups can selectively adopt with no mandatory compliance.
- Source: github.com/ethereum/RIPs / README
- Confidence: high (3-0 verified)

**C8 — Spec docs actively updated**
- Claim: SYNC_ROLLUPS_PROTOCOL_SPEC.md and EXECUTION_TABLE_SPEC.md last pushed 2026-06-05.
- Source: sync-rollups-protocol repo commit history
- Confidence: high (3-0 verified)

---

## Key Artifacts

| Artifact | Repo | Purpose |
|---|---|---|
| SYNC_ROLLUPS_PROTOCOL_SPEC.md | sync-rollups-protocol | Formal protocol spec |
| EXECUTION_TABLE_SPEC.md | sync-rollups-protocol | Execution table structure spec |
| CHANGES_FROM_PREVIOUS.md | sync-rollups-protocol | Architectural change log (removed symbols documented) |
| IEEZ.sol | sync-rollups-protocol | Canonical L1 interface |
| EEZ.sol | sync-rollups-protocol | 69KB active Solidity implementation |
| IProofSystem.sol | sync-rollups-protocol | ZK/ECDSA-interchangeable proof interface |
| ECDSAProofSystem.sol | sync-rollups-protocol | Current (temporary) ECDSA proof system |
| DERIVATION.md | sync-rollups-composer | L2 derivation spec with deterministic timestamp formula |
| Cargo.toml | sync-rollups-composer | Confirms reth is upstream pin, not a fork |

---

## Not Yet Implemented

- Production ZK verifier (IProofSystem slot exists; ECDSAProofSystem fills it for now)
- Stage 3: reorg handling + eez-follower derivation-based fullnode
- Stage 4: cross-chain composer with full L1-L2 proof

---

## Open Questions

1. Which zkVM is targeted for production integration? No public roadmap or EIP/RIP references it.
2. Stage 3-4 architectural design — spec documents not yet public.
3. EIP-8079 (native rollups) compatibility — relationship to EEZ smart-contract-based settlement not addressed in public specs.
4. Which founding alliance members have active testnet integrations vs press-release commitments?

---

## Refuted Claims — Do Not Use in Drafts

Secondary blogs generated 8 of 12 killed claims in adversarial review. Do not cite or repeat:

- ZisK/RISC-V running at 1.5 GHz for real-time ZK proving (0-3, no primary source)
- "EEZ enables contracts across L2s without bridges" (0-3, oversimplification of how settlement works)
- "60+ rollups unified, ETH as default gas with no bridging" (0-3, unsourced)
- Specific postBatch(entries, blobCount, callData, proof) interface signature (0-3, refuted against actual source)
- Stages 3-4 not yet implemented as definitive claim (1-2, split vote, unverifiable from public source)
- L2 blocks on 12-second schedule in eez-rollup0 (0-3, eez-rollup0 uses 2s; 12s is sync-rollups-composer's derivation formula, not block time)
- "EEZ enables fully synchronous cross-chain execution identical to same-chain calls" (0-3, overclaim)

Sources that generated the most refuted claims: BlockEden, Etherspot blog, CoinMarketCap Academy. Do not cite.
