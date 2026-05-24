# EEZ Technical Specification — Public Material

**Persona scope:** All. This file contains only public technical material. Internal research and unpublished spec details are not in this file.

---

## Architectural premise

EEZ rollups publish transaction data directly to Ethereum L1 and provide validity proofs in real time. This recovers a form of composability that closely resembles Ethereum's original synchronous execution model.

By anchoring all communication, events, and proofs on Ethereum L1, the architecture:
- Avoids trusted bridges and multisig upgrade mechanisms
- Preserves Ethereum's security model and credible neutrality
- Allows heterogeneous execution environments to coexist within a single composable system

## Composability matrix

| Direction | Reads | Writes |
|---|---|---|
| L2 → L1 | Synchronous | Synchronous |
| L1 → L2 | Asynchronous | Synchronous |
| L2 → L2 | Asynchronous | Synchronous |

From any EEZ rollup you can read L1 state in real time during execution. You can atomically write to multiple L2s in one transaction. You cannot synchronously read the *result* of a cross-L2 or L1→L2 state change in the same execution context.

## Design constraints

### No multisigs, ever

It would be unthinkable for Ethereum to deploy something with a multisig upgrade mechanism. This directly contrasts with every major L2 today. EEZ rollups inherit Ethereum's credible neutrality precisely because they inherit Ethereum's governance constraints.

### Multi-client, multi-prover from day one

At minimum two independent implementations of the proof systems, mirroring Ethereum L1's approach to client diversity. Single-prover risk is the largest attack surface in any ZK rollup; EEZ does not accept that risk.

### Rigorous, community-wide review

Thousands of eyes. Public dev calls. Public discussion. The full Ethereum security culture applied to the rollups themselves. This culture cannot be exported to third-party L2s automatically just because they deploy code on Ethereum.

### Distinct address namespaces

Each EEZ rollup uses a unique salt so the same address on different rollups cannot have different owners (the Safe problem today). This also allows an L2-native contract to hold tokens on L1 using its unique address.

## Native economics

Sequencing value, MEV, and fees flow back to Ethereum rather than to private operators.

Ethereum's issuance mechanism (currently used for validator rewards) could be extended to fund ZK proof generation for these native rollups, making proving a protocol-level public good rather than an external cost.

## Deployment characteristics

The vision is to deploy a set of identical instances delivering a step change in effective EVM block space. Each instance shares the same security model, the same governance process, and the same relationship to Ethereum L1.

**Deployment choice is simple:**
- If you need composability with specific apps, pick the rollup they are on.
- If you just need L1 proximity, pick the least-used (cheapest) one.
- All are equally close to L1.

## What this enables

- **Synchronous cross-rollup calls.** A contract on rollup A calls a contract on rollup B and gets the result back in the same transaction.
- **Single-deployment liquidity.** Pools and protocols deploy once and are accessible across the EEZ.
- **L1 state reads from L2.** Real-time access to L1 oracles, registries, and state during L2 execution.
- **Atomic multi-L2 writes.** Either everything happens or nothing does, across multiple rollup instances.

## What this does not do

- Does not retroactively make existing L2s native rollups.
- Does not require users or protocols to migrate.
- Does not solve cross-rollup composability with non-EEZ rollups (those fall back to async).
- Does not eliminate the cost of ZK proving; it changes who pays for it.

## Co-builders and funding

- **Built by:** the teams behind Gnosis and ZisK.
- **Co-funded by:** the Ethereum Foundation.
- **Shaped by:** the EEZ Alliance (~20 founding partners plus growing membership).

## Open questions (public)

- Final number of native rollup instances at launch
- Specific timeline for mainnet
- Detailed economic model for proof generation funding
- Final spec for L1↔L2 messaging primitives

These are areas where Alliance member feedback is actively sought.

## References

- The Bankless episode with Martin Köppelmann and Fredrik covers the thesis in conversational form. See `knowledge/eez/04-launch-ethcc.md` for the clip log.
- For the comms-framework version of this content, see `knowledge/eez/01-comms-framework.md`.
