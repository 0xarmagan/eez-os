# Real-Time Proving and Synchronous Composability Between Rollups (DAPPCon talk)

Source: **DAPPCon Berlin, 16 June 2026**, Jordi Baylina. Talk title "Real-Time Proving and Synchronous Composability Between Rollups" (27:38). YouTube: https://www.youtube.com/watch?v=f5POkcq015s. This is the spoken companion to the EEZ Architecture deck (`dappcon-2026-eez-node-architecture.md`). Primary source; quote as Jordi's framing, not approved comms.

## Key points (extracted)

### ZisK status
- **ZisK 1.0 alpha announced at this talk.** Code frozen ("everything is in place, can be used"). Kept as "alpha" because soundness is not yet 100% confirmed.
- RISC-V 64-bit ZKVM, fully open source, designed post-quantum ("quantum persistent"), 128-bit security.
- Next phase: auditing and AI-assisted formal verification of the circuits. For teams building their own proving systems, it is usable now.
- **ZK-ETH-1** (name uncertain from audio): a from-scratch stateless full Ethereum client, written with AI, giving roughly 10-50% reduction in proving cost.

### The EEZ proof, what gets proved
Synchronous composability = cross-chain context switching (a call to a contract on another chain, then a return) collected as an ordered log inside blobs. Three things must be proved:
1. **Blob proof**, parse the blobs, check consistency (no lost calls, format correct), and prove the blob commitment matches what L1 says was posted.
2. **ADSTF adapter proof**, each per-rollup context ("green square") is correct according to that chain's own rules, defined by the verification key of its circuit.
3. **L1 hash composition**, collect everything coming from L1 (blob commitments, starting/ending rollup states, all L1 interactions, per-rollup verification keys) into a single hash that links the proof to L1.

### Action-Driven State Transition Function (ADSTF)
- Jordi's own term in the talk: "we call it action-driven state transition function." A chain processes an *action* (a call, a return, or a transaction), changes state, and emits a new action plus revert info. Each chain defines its own STF (EVM, WASM, or anything), in full control.
- Confirms the explainer's flag: ADSTF is Jordi's conceptual name (deck + talk); the shipped contracts express it with execution entries / state deltas / rolling hash, not a literal `ADSTF` type.

### Cryptographic accumulator (the aggregation mechanism)
- An accumulator is hash-like but **order-independent** (set semantics): hashing two elements gives the same result regardless of order.
- Each proof **adds** elements (assumptions) and **subtracts** elements (things it proves). Aggregating two proofs just adds their accumulators.
- At the end the accumulator must be **zero**: everything assumed somewhere must be proved somewhere. Example: parsing a blob *assumes* each STF is correct (adds); the ADSTF adapter *proves* that STF (subtracts). Connected blobs cancel at the join, leaving only the start and end states in the accumulator, which is what goes into the L1 hash.

### Latency / pipelining
- Aggregation is order-independent, so you can **aggregate as you generate** (no fixed order). One aggregation circuit ≈ **80 ms**.
- Proof building starts at the *beginning* of the batch (as blobs and transactions are created), so that when block-building stops the remaining proving work is minimal.
- The **final part is the ~3-second target** (currently 3s, expect to go lower). This is the proof-generation latency, the real challenge for EEZ.
- Pipeline end: aggregate to a single proof, a **final circuit** verifies the aggregation plus the L1 hash, then convert to a **PLONK** proof that is easily verifiable on-chain.

### The ADSTF-adapter recursion trick (why future rollups can join)
- The verification key is variable per rollup. A rollup added later has its own circuit and VK.
- The adapter does one recursion level: it recursively verifies the rollup's own circuit against the VK configured in the smart contract, and adds the result to the full proof's accumulator.
- This is what lets EEZ prove state transitions for rollups whose STF is **not defined yet**, only the (short) verification key needs to be known.

### Client / following model
- To follow one chain you do not need to follow the others: from L1 plus the blobs you can extract the inputs and outputs that affect your chain and rebuild your state, ignoring the rest. Only the **composer** needs to synchronise the chains it connects.

### Validiums
- Validiums can connect to EEZ. To build a validium's *full* state you need extra data from other sources (off-DA). To route calls to or from a validium, the **composer needs access to that data**; other chains connecting to it only need the interactions recorded in the blobs.

### Named limitation
- **Data availability.** EEZ puts a lot of information into blobs, so it depends on high Ethereum data availability. Jordi names this explicitly as a limitation and wants L1 to scale.

### Q&A / context
- Many chains are not only about scalability but about extending Ethereum's functionality (specialised, opinionated, or private chains) while keeping composability. A rollup may legitimately change its own state root after a hack, which is unacceptable for L1 but acceptable for a specific rollup.
- Next talk: Philip Shamos (Gnosis Head of Infrastructure) on the technical implementation path for Gnosis Chain with the EEZ framework. Corroborates the "Connecting Gnosis Chain" roadmap item.

---

*Filed 2026-06-18. Companion to `dappcon-2026-eez-node-architecture.md`. Transcript fetched via youtube_transcript_api.*
