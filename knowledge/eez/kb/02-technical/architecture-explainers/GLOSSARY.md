# Glossary

Single home for the terms used across the EEZ Architecture Explainer series. Explainers link a term here on first use rather than re-defining it. See also [Conventions & Caveats](00-conventions-and-caveats.md).

| Term | Definition |
|---|---|
| **EEZ (Ethereum Economic Zone)** | An economic zone built on Ethereum: the shared proving and settlement layer that lets independent rollups call into each other as one machine. Not an L2, not a single rollup. |
| **Synchronous composability** | A cross-rollup CALL and its RETURN resolving inside one proven step, with shared state, before anything settles. Operationally: minimising proof-generation latency so the step fits in one L1 slot. |
| **Proxy** | An L1 stand-in for a contract whose real state lives on a rollup (drawn with a star, e.g. `DAO(*)`). Synchronous and shares state with its real contract. Not a copy, not a bridge. |
| **Execution Table** | The L1 structure recording every cross-chain CALL, RETURN, and revert for a combined execution. The on-L1 face of the combined step. *Coined term for this series:* there is no symbol literally named "Execution Table" in the L1 contract. The on-L1 structure is per-rollup execution queues plus a transient batch table bound by a rolling hash. The literal `loadExecutionTable` name appears only on the L2 side. |
| **Execution entries** | The cross-rollup CALL/RETURN unit (the term the contracts use), as defined for this series. *Term drift:* the older sibling spec [`Technical-Proposals.md`](../Technical-Proposals.md) calls the same unit a **"partial transaction."** Same concept, different name. Readers of the older spec should not be confused. |
| **EEZ Trace** | The deck's "blob format": one ordered record of every context switch (CALL / RETURN / revert) across all chains in a step. What the proving pipeline proves over. |
| **Composer** | Permissionless node software that builds and sends the `postAndVerifyBatch` transaction, assembling the cross-chain bundle, streaming to provers, and landing it on L1 via a builder. ([Explainer 5](05-the-composer.md)) |
| **Sequencer** | Per-chain binary that orders a chain's work and produces its blocks. If permissioned, the signing key lives here. |
| **Sync block** | An event-triggered block carrying a cross-chain interaction that must resolve atomically with another chain. Produced only when an L1↔L2 interaction is pending. |
| **Async block** | An ordinary block containing only own-chain work; produced on the chain's own schedule with no cross-chain coordination. |
| **ADSTF (Action-Driven State Transition Function)** | The deck's conceptual term for a per-rollup STF that takes an action-in + initial state and emits final state, action-out, and revert info. Not a literal type in the shipped code. ([Explainer 7](07-real-time-proving-zisk.md)) |
| **Accumulator** | The order-independent, hash-like mechanism in the recursion pipeline. Proofs add elements when they assume, subtract when they prove; a final sum of zero means everything assumed was proved. Enables aggregate-as-you-generate. |
| **Manager contract** | The per-rollup contract holding that rollup's owner-set proving threshold and allowed proof systems. `checkProofSystemsAndGetVkeys` reverts `ThresholdNotMet` / `ProofSystemNotAllowed`. |
| **Binding / non-binding sequencer** | Binding: the sequencer commits up front to the composer's cross-chain ordering, supporting same-block atomicity. Non-binding: the composer simulates and proposes; atomicity degrades to "same-batch, eventually." |
| **Optimistic / pessimistic coordination** | Optimistic: proceed assuming inclusion, clean up with reorgs (liveness-first). Pessimistic: lock the affected state first (safety-first); the lock need not be the whole chain. ([Explainer 4](04-chain-types-and-coordination.md)) *Disambiguation:* here "pessimistic" is a **coordination method** (state locking vs optimistic reorgs). It is **not** the AggLayer-style "pessimistic proof" (a token-conservation / no-negative-balances proof) referenced in [`Technical-Proposals.md`](../Technical-Proposals.md). Same word, two unrelated meanings. |
| **Native rollup** | A rollup that brings its own STF (WASM, RISC-V, custom) and accepted proof systems; can include ETH directly in cross-rollup CALLs via L1-level ETH accounting. |
| **Validium** | A chain that keeps data off-chain. May omit self-only work from blobs, but must expose cross-chain interactions (or a coordination mechanism) so composers can include them. |
| **`postAndVerifyBatch`** | The transaction the composer sends to L1 carrying the blobs, call data, state roots, and execution tables for a batch. |
| **ZisK** | An open-source, RISC-V 64-bit ZKVM (post-quantum, 128-bit security), at 1.0 alpha. **One** proof system EEZ supports, not the EEZ prover. |
| **Proof system** | Whatever a chain configures to attest its state: a ZK system, a TEE, or a validator multisig. The choice and threshold are the chain's, set on its manager contract. |
