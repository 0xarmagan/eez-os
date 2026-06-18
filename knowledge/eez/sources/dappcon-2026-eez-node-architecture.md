# EEZ Node Architecture — Martin's DAPPCon Draft

Source: **DAPPCon EEZ Workshop, 17 June 2026**, presented by Jordi Baylina (@jbaylina), co-branded Ethereum Economic Zone × ZisK VM. Hand-drawn architecture diagram ("Martin's Draw"). Treat as a **topology sketch from a primary technical source, not an implementation spec or canonical comms.** This is engineering-level founding material — quote as Jordi's/Martin's framing, not as approved EEZ messaging.

Assets in this directory:
- `dappcon-2026-eez-node-architecture.png` — the diagram (dark canvas; transcription below is the readable version)
- `dappcon-2026-eez-architecture-cover.pdf` — workshop title slide (cover only, no content slides)

---

## Diagram transcription

### Top — block / fork model
- L1 timeline: `L1-100 → L1-101 → L1-102`, with "L1 block propagation".
- **EEZ chain 1** block sequence containing: **async block**, **sync block 1**, **sync block 2**, **sync block preparation (chain split)**, **unused blocks**. Transactions classified as **tx independent of fork** vs **tx dependent of fork**. Note: *"only needed if L1↔L2 tx pending."*
- **EEZ chain 2** and **EEZ chain 3** rows: normal blocks interleaved with dashed blocks marked **async**.

### Middle — binaries / components
- **Binary 1 — `sequencer per chain`**: holds `sequencer / driver / eez-node` (*"if permissioned, key is here"*) → **engineAPI** (`builtBlock` / `forkChoiceUpdate`, flag: *sync block*) → `L2 execution client / EEZ reth`.
- **Binary 2 — `composer`**: holds `orchestrator`, `L1 EL client` (with nested `inspector`), `crosschain-mempool (L1 and L2 tx)`, `mem-Pool (L2 only)`, and **"Propose settlement"** — *"take the last n blocks and create the 'postBatch' payload that can be submitted first to the prover and next on L1."* Receives `proposeBlock`.
- **`prover`** (stacked): *"request: postBatch Data + potentially helper data (data streamed for prover to start early)"*; reads L1.
- Flow: `composer` —**submit bundle: (postBatch)(L1 user tx)**→ **`L1 Builder`** → **`L1`**.
- **Followers**: `follower L1 only` (reads L1); `follower L1 + sequencer` (reads L1 and L2). `L2` shown as dashed/derived state.

### Bottom — deployment variants
- Chain types: `Rollup 1`, `Rollup 2`; `sequenced 1 / 2 / 3`.
- **Non-binding mode**: `composer` containing `simulator` + `sequencer not binding` (stacked ×N) → maps to Rollups.
- **Binding mode**: three `composer` instances each with `sequencer binding` → map to sequenced chains.

---

## Expert review synthesis (agent-mesh: Web3 Protocol Architect + Technical Advisor)

### Convergent findings
- The macro decomposition is **sound**: sequencer-per-chain (L2 block production), composer (cross-chain orchestration + settlement), prover, and L1 Builder are genuinely distinct timescales and failure domains.
- The **sync-block / async-block** model maps cleanly to the KB's native-path vs async-path. New detail not in the KB: sync blocks are **event-triggered** ("only needed if L1↔L2 tx pending"), not always-on. Operationally realistic, but introduces an unanswered cross-chain-tx → sync-block-inclusion latency question.
- "Stream helper data so the prover starts early" is **feasible** and matches ZisK pipelining — but only the L1-state-independent portion can start early; the proxy state-root portion still waits on the final L1 block hash.

### Load-bearing gaps (Choice Points)
1. **Reorg / cancellation protocol is the #1 missing piece.** Composer submits postBatch → prover starts → if L1 reorgs, nothing shows who tells the prover its streamed data is invalid, or what state each binary resets to. Both experts flagged this as where bugs surface first.
2. **Multi-composer coordination (binding mode).** Three independent binding composers cannot atomically settle a cross-chain tx spanning their chains without a coordination layer that is not drawn. The hardest part of the binding design is invisible.
3. **Binding vs non-binding = trust delta, not config.** Non-binding degrades "same-block atomicity" to "same-batch, eventually." If binding is a self-declared label with no slashing at the settlement contract, it is a social guarantee, not a cryptographic one.
4. **Engine API "sync block flag" semantics** are an actual EEZ-reth protocol change needing a precise spec (does it gate sealing? block next-block production during a chain split?).
5. **Single points of liveness:** one sequencer-per-chain (key in process, no standby) and a single composer per binding deployment, with no HA / redundancy shown. (Note: the single stacked `prover` box is a topology abstraction — EEZ requires a *minimum of two* proving systems as a protocol requirement, e.g. Zisk + SP1 + TEE. Do not read the diagram as single-prover.)

### Blind spot
- **Sequencer economics:** binding requires capital lockup but sync-block fee revenue is irregular (only when cross-chain txs pend). Rational operators may defect to non-binding in quiet periods, collapsing the synchronous guarantee to best-effort.

### Robust read
Treat the diagram as a **correct topology sketch, not an implementation spec.** The happy path is well understood; the deferred decisions (reorg/cancellation, multi-composer coordination, Engine-API flag semantics) are exactly the load-bearing ones an implementer would need resolved.

---

*Filed 2026-06-18. Companion to `martin-l2-talk.md` (same source author lineage). Review produced via agent-mesh orchestrator with isolated Web3 Protocol Architect and Technical Advisor experts, grounded against this repo's `02-technical/` KB and `open-questions-v2.md`.*
