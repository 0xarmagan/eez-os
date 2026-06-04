# Community Technical Q&A on EEZ Architecture

Technical questions from community discussion with EEZ team responses.

---

## Block Production & Finality

### 1. Does EEZ impose a universal L2 block cadence that undermines fast block times?

**Question frame:** EEZ-participating L2s would be fixed to a universal block rhythm, unable to reach finality faster than this cadence — undermining the speed advantage that operational centralisation gives L2s today. Based preconfirmations may be part of the design space to address this.

**EEZ response:** The concept of a "block number" is defined by each chain, not by the EEZ itself. When a batch is committed, it may include one block or many blocks.

Compatible models include:
- One L2 block = one L1 block (in the extreme case, L2 block is defined as the L1 block)
- L2 block number increments every time a batch is received
- A single batch transaction includes multiple L2 blocks (e.g. a special "block marker" transaction, so the same batch contains N blocks with M transactions each — likely model for centralised sequencers)
- Some rollups may define one block per transaction

**EEZ does not impose a universal L2 block cadence.** It imposes a synchronisation/finality point for the composable state transition, while each rollup remains free to define its own internal block structure and cadence.

---

## Based vs Non-Based Rollups

### 2. Does EEZ participation depend on being a based rollup? Is there a utility difference?

**Question frame:** EEZ participation seems not contingent on being based or not. What is the distinction? Is there any difference in utility derived from EEZ participation for based vs. non-based rollups? Does it follow that based rollups enjoy synchronous composability of preconfirmations while non-based rollups can only do so on L1 cadence?

**EEZ response:** There is no fundamental difference between based rollups and centralised-sequencer rollups from the EEZ point of view.

One way to look at a centralised sequencer rollup: it is a based rollup where all transactions or blocks must additionally be signed or authorised by the sequencer.

The main issue is how the composer and the sequencer synchronise in order to guarantee consistency of the L2 state. A based rollup may have a more natural integration path because sequencing is already closer to the L1/composer flow. But centralised-sequencer rollups can also participate as long as the sequencing and commitment rules are clear and enforceable.

**Status:** ⚠️ Integration path for centralised sequencers is an open design space; synchronisation mechanism TBD.

---

## Finality Requirements

### 3. Is real-time withdrawal finality via validity proving a hard requirement for EEZ participation?

**Question frame:** Many L2s are transitioning to validity proving / multiproving but have not yet enabled instant withdrawal finality even when the maximum multiproving threshold is met (e.g. Base). Would this prevent atomic interoperability inside EEZ?

**EEZ response:** Yes. To perform atomic composable transactions between chains that may not trust each other, the relevant state transition must be final.

Each rollup can choose which proving system, or combination of systems, provides that finality:
- zkVMs (including ZisK) — working toward safe, reliable, cost-effective ZK proving
- TEE-based approaches
- Validator committees or multisig-style security models
- Combinations of the above

The EEZ does not prescribe which finality mechanism must be used. It only requires that the participating chains agree on what constitutes finality. The decision is ultimately for each chain and its users to make.

**Status:** ✓ **Addressed. Finality is required; mechanism is chain's choice.**

---

## Proof Aggregation & Composer Economics

### 4. How will composers cover the growing cost of proof aggregation as rollup participation scales?

**Question frame:** Proof aggregation at the composer level enables atomic synchronous composability, but proving costs (especially ZK) grow significantly with increased rollup participation and transaction volume. Since composers must cover these costs themselves, will there need to be L1 revenue share changes? How do composers achieve sufficient compensation?

**EEZ response:** Composers will choose transactions that are economically profitable for them.

The payment model is not defined initially. It could take different forms:
- An L1 fee
- An L2 fee
- A direct payment to the composer
- A private agreement
- Some other mechanism

The core assumption: composers will only include transactions when expected compensation is enough to cover operational and proving costs. The exact fee market or revenue-sharing model is still an open design space.

**Status:** ⚠️ **Open design space. Fee market / revenue-sharing mechanism TBD.**
