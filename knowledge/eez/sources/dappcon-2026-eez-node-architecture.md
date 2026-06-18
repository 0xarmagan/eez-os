# EEZ Architecture — DAPPCon 2026 Workshop (Jordi Baylina)

Source: **DAPPCon EEZ Workshop, 17 June 2026**, presented by Jordi Baylina (@jbaylina), co-branded Ethereum Economic Zone × ZisK VM. Two assets, both engineering-level founding material — quote as Jordi's framing, not as approved EEZ comms.

Assets in this directory:
- `dappcon-2026-eez-architecture-cover.pdf` — **the full 56-slide workshop deck** (definition, properties, chain types, the composer, EEZ Trace / blob format, the ADSTF + ZisK proving pipeline, and the roadmap).
- `dappcon-2026-eez-node-architecture.png` — Jordi's hand-drawn node-software topology ("Martin's Draw"). Transcription + expert review below.

---

## Part 1 — The 56-slide deck

### Core definition (sharper than the KB)
> "EEZ proves the **combined execution of many rollups as a single, synchronous transaction**."

Cross-chain interaction is a **normal Ethereum CALL → RETURN** between smart contracts on different chains (Chain 1 SmartContract A `CALL` Chain 2 SmartContract B, then `RETURN`). Not message passing, not a bridge.

### The proxy / Execution Table mechanism
On L1, the participating contracts exist as **proxies** (drawn with a `*`: `UserAA(*)`, `Whitelist(*)`, `DAO(*)`) of their real counterparts on the rollup (R2). The L1 `DAO(*)` proxy writes cross-chain calls/returns into an **Execution Table** on L1. This is the concrete form of "proxies, not bridges" — proxies are synchronous and share state.

### EEZ Properties (canonical 7-point spec)
1. Permissionless smart contract
2. Not upgradable
3. Roll-ups can opt-in / opt-out freely
4. **Proof-system agnostic** — ZK based, TEE, or Multisig
5. **Rollups are sovereign**, governed by a rollup smart contract — each defines its own rules, its own state transformation function, and its own accepted proving systems
6. **Security warranted across rollups** — "the same security Ethereum provides to independent smart contracts calling each other"
7. **Orthogonal to L1 pre-confirmations**

### Chain types (each gets a slide)
- **Custom native rollups** — own STF (WASM, RISCV, custom); cross-rollup interaction via normal Ethereum CALLs; ETH-as-native rollups can include ETH in CALLs; cross-rollup ETH transfers use rollup-level accounting maintained in L1; rollups are sovereign and define their own update mechanism; permissionless rollup creation.
- **Based / Centralized Sequencer Rollups** — any centralized rollup is treated as a base rollup with all state transitions signed by the sequencer. The L2 sequencer must coordinate with the composer to include cross-chain txs. **Two coordination methods:** *optimistic = reorgs*; *pessimistic = locking (not necessarily the full chain)*.
- **Validiums** — may omit self-chain-only TXs from blobs, but must provide that info or a coordination mechanism with composers if their TXs are to touch other chains.

### The composer
- Builds and sends the `postAndVerifyBatch` transaction/bundle.
- **Anybody can be a composer** (permissionless).
- **EEZ does not currently define an incentive mechanism** for routing fees to the composer — expects TXs to carry implicit/explicit reward so composers are incentivized to include them.
- **EEZ does not currently define a public pool of crosschain TXs.**

### Client model
A node following a single L2 chain "should be able to build its state without synchronizing any other L2 (only L1 and his chain)."

### The ZisK proving pipeline (the novel half, absent from the diagram)
- **Action-Driven State Transition Function (ADSTF):** ACTION IN → INITIAL STATE → ADSTF → FINAL STATE / ACTION OUT / REVERT INFO.
- **EEZ Trace / "the blob format"** — records each context switch (CALL and RETURN) between chains, including revert handling.
- **Multi-prover capable, per-rollup threshold:** structures `ProofSystemBatchPerVerificationEntries`, `RollupIdWithProofSystems`, and `_fetchVkMatrix(...)` build a per-rollup **VK matrix**. Each rollup's own manager contract holds an owner-set `threshold` (M-of-N); `checkProofSystemsAndGetVkeys(...)` reverts `ThresholdNotMet` below it. **Correction (verified against `eez-core-protocol` 2026-06-18):** there is NO protocol-enforced minimum of two — `setThreshold` accepts any value including 1 or 0. Multi-prover is the security intent, not a contract floor. (`InvalidProofSystemConfig()` is a separate registry-side structural error, not the threshold check.)
- **Recursion chain:** ADSTF Adaptor (verifies a user-defined circuit VK) → **L1 Hash Builder** (blob commitment, starting/ending blob rollup state, L1 interactions, `RollupId → PS, VK`) → **Aggregation Circuit** (verifies 2 circuits, adds their accumulators) → **Final Circuit** (verifies root aggregation + L1 Hash builder, checks accumulator sum = 0) → **PLONK Circuit** (generates an easily on-chain-verifiable PLONK proof).
- **Synchronous Composability = minimizing proof-generation latency:** Bundle Building (composer) and Proof Building run overlapped, total target **< 3s** (fits inside an L1 slot).

### Roadmap (slide 55)
1. Cleanup smart contract · 2. Cleanup documentation · 3. Request for comments · 4. Audit smart contracts · 5. Signature and Zisk proving system · 6. **Composer 1.0** · 7. **Chain Zero** · 8. **Connecting Gnosis Chain**.

"Connecting Gnosis Chain" corroborates the Gnosis-L2/EEZ pivot; "Chain Zero" and "Composer 1.0" are named milestones.

---

## Part 2 — The node-architecture diagram ("Martin's Draw")

### Top — block / fork model
- L1 timeline `L1-100 → L1-101 → L1-102` with "L1 block propagation".
- **EEZ chain 1**: **async block**, **sync block 1/2**, **sync block preparation (chain split)**, **unused blocks**. Tx labeled **independent of fork** vs **dependent of fork**. Note: *"only needed if L1↔L2 tx pending."*
- **EEZ chain 2 / chain 3** rows: normal blocks interleaved with dashed **async** blocks.

### Middle — binaries / components
- **Binary 1 — `sequencer per chain`**: `sequencer / driver / eez-node` (*"if permissioned, key is here"*) → **engineAPI** (`builtBlock` / `forkChoiceUpdate`, flag: *sync block*) → `L2 execution client / EEZ reth`.
- **Binary 2 — `composer`**: `orchestrator`, `L1 EL client` (nested `inspector`), `crosschain-mempool (L1 and L2 tx)`, `mem-Pool (L2 only)`, and **"Propose settlement"** — *"take the last n blocks and create the 'postBatch' payload submitted first to the prover and next on L1."* Receives `proposeBlock`.
- **`prover`** (stacked): *"request: postBatch Data + potentially helper data (streamed so the prover starts early)"*; reads L1.
- Flow: `composer` —**submit bundle: (postBatch)(L1 user tx)**→ **`L1 Builder`** → **`L1`**.
- **Followers**: `follower L1 only` (reads L1); `follower L1 + sequencer` (reads L1 and L2). `L2` shown as dashed/derived.

### Bottom — deployment variants
- Chain types: `Rollup 1`, `Rollup 2`; `sequenced 1 / 2 / 3`.
- **Non-binding mode**: `composer` with `simulator` + `sequencer not binding` (stacked ×N) → Rollups.
- **Binding mode**: three `composer` instances each with `sequencer binding` → sequenced chains.

---

## Part 3 — Expert review synthesis (agent-mesh: Web3 Protocol Architect + Technical Advisor)

### Convergent findings
- The macro decomposition is **sound**: sequencer-per-chain (L2 block production), composer (cross-chain orchestration + settlement), prover, and L1 Builder are genuinely distinct timescales / failure domains.
- The **sync-block / async-block** model maps to the KB's native-path vs async-path, with a new detail: sync blocks are **event-triggered** ("only needed if L1↔L2 tx pending"), not always-on.
- "Stream helper data so the prover starts early" is **feasible** and matches ZisK pipelining — but only the L1-state-independent portion can start early; the proxy state-root portion still waits on the final L1 block hash. The deck's `<3s` overlapped Bundle/Proof building is the same idea made concrete.

### Gaps the diagram left open — now partly answered by the deck
1. **Reorg / cancellation protocol.** The deck's *Based/Centralized Sequencer Rollups* slide gives the two intended methods — **optimistic = reorgs**, **pessimistic = locking (not necessarily the full chain)** — which is the design intent behind the diagram's "chain split". Still no per-step cancellation protocol for an in-flight prover run once an L1 reorg invalidates a sync block.
2. **Multi-composer coordination (binding mode).** The deck confirms "anybody can be a composer" and that there is **no protocol-defined public crosschain-tx pool yet** — so coordination across composers is explicitly an open design area, not an oversight.
3. **Binding vs non-binding = trust delta.** Non-binding degrades same-block atomicity to "same-batch, eventually."
4. **Engine API "sync block flag"** is a real EEZ-reth protocol change needing a precise spec.
5. **Single points of liveness:** one sequencer-per-chain (key in process, no standby) and a single composer per binding deployment. (The single stacked `prover` box is a topology abstraction. EEZ is proof-system agnostic and multi-prover *capable*; each rollup sets its own threshold, e.g. Zisk + SP1 + TEE, but the protocol does not enforce a minimum of two. Do not read the diagram as single-prover, and do not claim a contract floor of two.)

### Blind spot
- **Sequencer / composer economics:** the deck openly states fee incentives for composers are undefined. Binding requires commitment but sync-block / cross-chain fee revenue is irregular — rational operators may default to non-binding / optimistic in quiet periods.

### Robust read
The diagram is a **correct topology sketch**; the deck is the spec-level companion. Together they answer most of the "happy path" and name the genuinely open problems (reorg/cancellation protocol, multi-composer coordination, composer fee incentives) as roadmap work rather than hidden gaps.

---

*Filed 2026-06-18 (deck reviewed in full after initial diagram-only pass). Companion to `martin-l2-talk.md`. Review via agent-mesh orchestrator (isolated Web3 Protocol Architect + Technical Advisor), grounded against this repo's `02-technical/` KB and `open-questions-v2.md`.*
