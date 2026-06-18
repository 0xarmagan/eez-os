# EEZ Node Architecture: The Two Binaries and the Block Model

*Source: `knowledge/eez/sources/dappcon-2026-eez-node-architecture.md` (DAPPCon EEZ Workshop, 17 June 2026, Jordi Baylina). Part 2 transcribes the hand-drawn node-software diagram ("Martin's Draw"); Part 3 carries the expert review. Engineering-level founding material. Quote as Jordi's framing, not as approved EEZ comms. EEZ is at roadmap stage and is not deployed yet.*

This explainer is for builders and partners who want to understand the node software behind the Ethereum Economic Zone (EEZ). It walks through the two binaries that run a participating chain, the block and fork model that governs when cross-chain work happens, the follower and node roles, and the two deployment modes. It also names, honestly, the parts the diagram leaves open.

EEZ is an economic zone built on Ethereum. It is not an L2, and it is not equivalent to any single rollup. The node software described here is what an operator runs to take part in that zone.

## The two binaries

Jordi's diagram splits the node software into two separate binaries. They run on different timescales and have different jobs, so keeping them apart is a deliberate design choice, not an accident of drawing.

### Binary 1: the sequencer, per chain

There is one instance of Binary 1 per chain. It produces that chain's blocks and nothing else. Inside it sit three named parts, `sequencer`, `driver`, and `eez-node`, plus the execution client they drive.

The `sequencer` orders the chain's work and decides what goes into the next block. The diagram carries a short note here: "if permissioned, the key is here." In other words, when a chain runs with a permissioned sequencer, the signing key that authorises its blocks lives inside this process. That makes the key location explicit, and it also makes the single sequencer a single point of liveness, which we return to under the open gaps.

The `eez-node` and `driver` talk to the execution client over the Engine API. The execution client is EEZ reth, a reth-based L2 execution client. The Engine API link carries two calls the diagram names directly: `builtBlock`, which returns a block the execution client has assembled, and `forkChoiceUpdate`, which tells the execution client which head to build on. The diagram adds one extra detail on this link: a flag marked "sync block." That flag is how the node signals that the block it is asking for is a sync block rather than an ordinary block. The precise semantics of that flag are an EEZ reth protocol change, and the diagram does not fully specify them. We flag that under the open gaps too.

So Binary 1, read top to bottom, is: the sequencer decides and (if permissioned) signs, the driver and eez-node drive the Engine API with `builtBlock` and `forkChoiceUpdate` and the sync-block flag, and EEZ reth executes and stores the chain's state.

### Binary 2: the composer

Binary 2 is the composer. It handles cross-chain orchestration and settlement, and it is the subject of its own explainer, so we summarise it here and cross-reference. (See explainer 5 on the composer for the full treatment.)

In short, the composer runs an `orchestrator`, an L1 execution-layer client with a nested `inspector`, a cross-chain mempool that holds both L1 and L2 transactions, an L2-only mempool, and a "propose settlement" step. That settlement step takes the last few blocks and builds the `postBatch` payload, which it submits first to the proving pipeline and then to L1. The composer is permissionless: anybody can run one. For how it builds the bundle, streams helper data to the prover, and submits to the L1 builder, read explainer 5.

The reason the composer is a separate binary is that its work spans all participating chains and the L1, whereas Binary 1 only ever knows its own chain plus L1. The two have genuinely different scopes and failure domains.

## The block and fork model

The top of the diagram is a timeline. L1 advances `L1-100 → L1-101 → L1-102` with normal L1 block propagation. Below it, each EEZ chain produces its own blocks, and those blocks come in two kinds.

### Async blocks versus sync blocks

Most blocks on an EEZ chain are async blocks. An async block contains work that touches only its own chain. It does not depend on anything happening on another chain in the same step, so it can be produced on the chain's own schedule without coordination. On the native path, a native rollup interaction settles in roughly twelve seconds. The async path, used when an interaction crosses to L1 without that native coupling, settles in roughly twenty minutes. Name the path beside any figure; there is no single finality number for EEZ as a whole.

A sync block is different. A sync block is the unit that carries a cross-chain interaction, the kind that has to resolve atomically with work on another chain. The diagram shows `sync block 1` and `sync block 2` on EEZ chain 1, alongside a step it labels "sync block preparation (chain split)." Preparing a sync block splits the chain's normal flow so the cross-chain step can be assembled and proven as one thing rather than being interleaved with unrelated local work.

### Sync blocks are event-triggered

The single most important note in this part of the diagram is short: sync blocks are "only needed if L1↔L2 tx pending." Sync blocks are event-triggered, not always-on. A chain does not produce a steady stream of them. It produces one only when there is a pending interaction that crosses between L1 and L2 and so needs the atomic, coordinated treatment. The rest of the time the chain produces async blocks and runs on its own.

This matters for how you reason about cost and coordination. Coordination overhead is paid only when a cross-chain interaction is actually in flight. Quiet chains stay cheap.

### Independent of fork versus dependent of fork

The diagram tags transactions two ways: "independent of fork" and "dependent of fork." A transaction that is independent of the fork does not care which way a pending cross-chain decision resolves; it can be included regardless. A transaction that is dependent of the fork does care, because its validity is tied to the outcome of the cross-chain step that the sync block is resolving. That dependence is why the chain splits during sync-block preparation: the fork-dependent work has to be held against the outcome, while fork-independent work can proceed. The diagram also shows "unused blocks," which are the branches of that split that do not end up on the final chain once the cross-chain outcome is known.

## Node and follower roles

Not every participant produces blocks. The diagram distinguishes two follower roles by what they read, and that determines what each can verify.

A `follower L1 only` reads L1 and nothing else. It can verify everything that is settled and proven on L1, which includes the `postBatch` results and the proven cross-chain outcomes once they land. What it cannot do is independently reconstruct an individual L2 chain's full internal state, because it never reads that L2's own data. In the diagram the L2 it does not follow is drawn dashed, meaning derived rather than directly observed.

A `follower L1 + sequencer` reads both L1 and a chosen L2. It can do everything the L1-only follower can, and on top of that it can rebuild and verify the full state of the L2 it follows. This matches the deck's client model: a node following a single L2 chain should be able to build its state from L1 plus that one chain, without synchronising any other L2. You follow your chain and L1, and that is enough to verify your chain.

The practical takeaway is that the verification you get scales with the data you choose to read. L1-only gives you settled, proven results. L1-plus-one-chain gives you full local verification of that chain. Neither role requires you to sync every chain in the zone.

## Deployment modes: binding and non-binding

The bottom of the diagram shows two ways to deploy, and they map onto two chain types.

In **non-binding mode**, the composer runs with a `simulator` and a sequencer that is "not binding," and this stack can be replicated across several chains. The sequencer is not committed in advance to the cross-chain outcome the composer is building. The composer simulates, and the chains follow without a hard pre-commitment. This mode maps to rollups. It is more flexible, but the expert review notes the cost: non-binding degrades same-block atomicity to "same-batch, eventually" rather than guaranteed same-block.

In **binding mode**, each composer instance runs with a sequencer that is "binding," and the diagram shows three such composer instances feeding sequenced chains. A binding sequencer commits to the cross-chain step the composer is assembling. This mode maps to sequenced chains (centralised-sequencer chains), where the sequencer can be made to coordinate directly with the composer. It gives stronger atomicity because the commitment is made up front, at the cost of requiring that commitment.

The deck's chain-types material lines up with this. A centralised-sequencer rollup is treated as a base rollup whose state transitions are signed by the sequencer, and that sequencer must coordinate with the composer to include cross-chain transactions. The deck names two coordination methods for this: optimistic, which allows reorgs, and pessimistic, which uses locking that need not lock the whole chain. Those two methods are the design intent behind the diagram's "chain split."

## Known open gaps

The diagram is a correct topology sketch, not a finished specification. Several things are genuinely open, and the honest position is to name them. The deck answers some of them at the level of intent without giving a full protocol.

**Reorg and cancellation protocol.** The deck gives the two intended coordination methods (optimistic reorgs, pessimistic locking), which is the design intent behind the chain split. What is still missing is a per-step cancellation protocol for an in-flight proving run once an L1 reorg invalidates a sync block. That is roadmap work, not a solved problem.

**Multi-composer coordination.** Anybody can run a composer, and the deck confirms there is no protocol-defined public pool of cross-chain transactions yet. So how multiple composers coordinate, especially in binding mode, is an explicitly open design area rather than an oversight.

**Engine API sync-block flag semantics.** The sync-block flag on the Engine API link is a real EEZ reth protocol change. It needs a precise specification, and the diagram does not provide one.

**Single points of liveness.** There is one sequencer per chain, with its key in the process and no standby shown, and a single composer per binding deployment. The diagram does not show failover for either.

**Composer economics.** The deck states openly that fee incentives for composers are undefined. Binding mode requires commitment, but cross-chain fee revenue is irregular, so rational operators might default to non-binding or optimistic behaviour in quiet periods. This is a named open question, not a design that has been settled.

One thing that is *not* an open gap, despite how the diagram looks. The diagram shows a single `prover` box. That is a topology abstraction. EEZ enforces a minimum of two proving systems per rollup in the on-chain contract (for example Zisk plus SP1 plus a TEE), and the contract reverts if fewer are configured. Do not read the single box as a single-prover design.

EEZ is not live yet. The roadmap runs through smart-contract cleanup, documentation, a request for comments, an audit, the signature and Zisk proving systems, Composer 1.0, Chain Zero, and then connecting Gnosis Chain. The node architecture above is what operators are being built to run, not software you can run in production today.

## Accuracy notes

These guardrails were relevant to this document and were respected as follows:

- **Economic zone, not L2.** EEZ is described as an economic zone built on Ethereum throughout. EEZ reth is named as an L2 execution client and the chains as rollups or sequenced chains, but EEZ itself is never called an L2 or a single network.
- **Async vs native finality.** No single finality figure is applied to EEZ as a whole. The native path (~12 seconds) and the async path (~20 minutes) are named with their paths beside the figures.
- **Proxies, not bridges.** No EEZ cross-chain mechanism is called a bridge. Cross-chain interaction is described as the coordinated, proven work carried in sync blocks.
- **Multi-prover, not single-prover.** Proving is described as a minimum of two proving systems per rollup, enforced in-contract. The single `prover` box in the diagram is explicitly flagged as a topology abstraction, not a single-prover claim.
- **Execution entries, not transactions.** Operations inside native rollups are referred to as execution entries where relevant. "Transaction" is used for the L1 layer and for the diagram's own labels (the L1↔L2 tx, the cross-chain mempool, fork-dependent transactions), and for the deck's own phrasing.
- **Not deployed yet.** The document states EEZ is at roadmap stage and that the node software is not something you can run in production today, with the roadmap milestones named.
