---
title: Real-Time ZK Proving (Zisk)
type: concept
tags: [ZK, zkVM, Zisk, proving-speed, EEZ, Circom, Baylina]
source_count: 3
updated: 2026-04-09
---

## Definition

Real-time ZK proving refers to the ability to generate a zero-knowledge proof of an Ethereum block's execution within the same 12-second slot that produced the block — fast enough that the proof can be included in (or referenced by) the *next* block's state transitions. Zisk claims to have achieved this.

## Why this is the linchpin

Without real-time proving, synchronous cross-chain composability is impossible. A proof that takes 2 minutes means cross-chain calls take 2 minutes to settle — asynchronous, not synchronous. EEZ's entire category differentiation rests on proving being fast enough to close within a single Ethereum slot.

## What "real-time" precisely means in Zisk's architecture

Proving is **pipelined** with block building. As the Composer builds the block (assembling transactions, computing the state transition table, simulating cross-chain calls), Zisk begins proving in parallel. The critical latency metric is not total proving time — it is the **gap time**: the delay between *finishing block construction* and *having the completed proof available*.

**Zisk's claimed gap time: ~3 seconds (and falling)**

Jordi Baylina (EthCC Cannes, 2026): *"Currently we are around 3 seconds even less and we have margin to improve that."*

The Ethereum slot is 12 seconds. If block construction takes ~9 seconds (typical for a full-gas block), and proof finalization takes ~3 more seconds, the total pipeline fits within a single 12-second slot with margin.

## How ZK recursion eliminates historical replay

Rather than re-executing the entire Ethereum history to prove current state, ZK recursion composes proofs:

- Proof(N) = ZK_verify(Proof(N-1), new_transactions) → new_state
- Each block's proof builds on the previous block's proof
- The chain's current state is a single proof that encompasses all prior history
- Verifying a proof is always fast; only proof *generation* has latency

This is what makes per-block proving computationally tractable — you're not proving Ethereum from genesis every block.

## Scaling properties

ZK proving is **linear and hardware-parallelizable**:
- More transactions = more proving work, but the work is parallelizable across GPUs
- Higher transaction volume → add more proving hardware
- No exponential scaling bottleneck

Baylina: *"Zero proof is very linear... it scales very well and if you want to prove more transactions you just put more hardware."*

## Security specifications

- 128-bit security (standard cryptographic strength)
- Quantum resistant
- Fully open source

## Current state (April 2026)

- **Devnet:** Working demo demonstrated at EthCC Cannes. Live cross-chain atomic call between L1 and L2 completed on devnet.
- **Testnet:** Planned; mainnet "sometime this summer" per Friederike Ernst.
- **Benchmark status:** Internal only — sub-3-second gap time claimed, not yet independently benchmarked at production Ethereum block sizes under adversarial conditions.

## The remaining open question

The 3-second gap time is measured under controlled conditions. Production Ethereum blocks can vary enormously in computational complexity (simple transfers vs. complex DeFi batches at full gas). The p99 gap time at maximum Ethereum block gas usage under concurrent EEZ cross-chain activity remains the critical unknown.

However, Baylina's "margin to improve" language and the hardware-linear scaling property substantially de-risk this — optimization work can address gap time directly, and more proving hardware directly addresses throughput.

## Comparison to prior ZK systems

| System | Use case | Proof generation time (approx.) |
|--------|----------|--------------------------------|
| Polygon zkEVM (production) | L2 block proving | 30–120 seconds |
| zkSync Era (production) | L2 block proving | 60–180 seconds |
| Zisk (EEZ devnet) | Ethereum block proving (gap time) | ~3 seconds |

The gap between Zisk's claim and current production zkEVM systems is ~10–60x. This is consistent with Baylina's 2-year focused optimization effort on a circuit architecture specifically designed for Ethereum block proving speed, not general-purpose ZK computation.

## Related concepts

[[concepts/synchronous-composability]], [[concepts/l2-fragmentation]]

## Sources

- [[sources/eez-announcement-overview]] — initial claim
- [[sources/eez-ethcc-cannes-talk]] — specific 3-second number; pipelining architecture; hardware scaling
