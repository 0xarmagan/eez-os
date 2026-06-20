# EEZ BuilderRoom Workshop — Full Recording Digest (DAPPCon 2026)

**Source:** the complete recording of the EEZ BuilderRoom workshop, **17 June 2026** (~2 hours, ~30k words). This is the *live recording* of the session the other DAPPCon source digests (deck, node-architecture diagram, proving talk) were drawn from — so it is the most faithful primary source for the mechanism, the Q&A nuance, and the project status. Engineering-level founding material; quote as the builders' framing, **not** approved EEZ comms.

**Presenters**
- **Friederike Ernst** — high-level rationale and roadmap (the "why").
- **Martin Köppelmann** — technical overview and the live demo (the "how", plus block-building and gas).
- **Jordi Baylina** — smart-contract and proving-system deep dive (chain types, multi-prover, EEZ Trace, the accumulator).

**Format.** Deliberately participatory — "2 hours of talk and 2 hours of workshop … intermingled." Three escalating layers: Friederike's rationale → Martin's technical overview + live demo → Jordi's hardcore contract/proving deep dive. The room was told to interrupt with questions throughout, and did.

**Audience (technical, persistent)**
- **A protocol researcher** who repeatedly pressed on whether the model captures *state transitions / side-effects* (not just function returns), and raised "Amsterdam"/Block-Access-List semantics.
- **An Ethereum researcher** who probed async static calls in depth using Optimism's quarter-second blocks as the worked case.
- **A SAFE engineer** on cross-chain address portability (a known SAFE user-friction point).
- Several others on gas, ordering, nonces, and proving decentralisation.

---

## 1 — The opening (and a note on tooling)

Martin opened by asking the room for a simple example contract to deploy live — "something that gets a value, returns a different value, maybe changes its state." The room debated a **counter** (the canonical example) versus an **ENS-style key-value store**; they went with the **key-value registry**. Notably, the team used **Claude to generate the raw calldata** during the demo ("Just use Claude. Give me the call data"), and Friederike's aside — "Martin and Claude are on it" — frames the live build as AI-assisted.

---

## 2 — Why EEZ (Friederike)

**The status quo.** Ethereum is surrounded by chains with genuinely useful infrastructure ("not vaporware"), but you cannot use protocols on those chains the way you could if they were co-located. Canonical bridges settle slowly:
- "30 minutes or 2 hours" for some;
- a "technically 7-day settlement window" for Base / Arbitrum / Celo — "a lot like sending a postcard."

Third-party bridges bypass the wait but open "another can of worms," and none of this provides **synchronous, contract-level composability** — you can *send* things, not *compose* on them.

**The idea.** Build a zone that **collapses settlement time to Ethereum's 12 seconds**, so everything inside is "de facto synchronous with Ethereum." This restores the "money and infrastructure Lego world that we had before L2s." Worked example: Polymarket on Polygon would love to use **sDAI** (yield-bearing) instead of USDC as collateral — impossible across today's bridges, natural inside EEZ.

**The end state.** Ethereum as the core settlement/coordination layer; every participating network composes synchronously — and critically **not just hub-and-spoke** (Ethereum ↔ one L2) but **any L2 ↔ any L2** inside the zone.

---

## 3 — The live demo (Martin)

Martin deployed a key-value registry (an ENS-like "set a name → value, become its owner, read the value") **on an L2**, and pre-registered `ez = "version 0"`. He then demonstrated calling it **from Ethereum (L1)**.

**The proxy.**
- Every contract on any chain has a **deterministic proxy contract on any other chain**. The proxy address is determined by **exactly two things: the target contract address + the chain ID** (CREATE2-style determinism). With, say, 20 chains × ~1M accounts you get ~20M potential proxy addresses, so in practice you give a **hint** about which (contract, chainID) a call touches.
- The proxy "doesn't have any code … doesn't know anything about the state of the actual contract." It is an **extremely generic fallback handler** that forwards the complete calldata + value to the EEZ contract — "just a forwarder," supporting any ABI, recording input/output pairs.

**The composer's role.** The proxy forwards to a **lookup table** that the **composer** must prepare ahead of time: it *anticipates* the call, *simulates* it on the L2, captures the correct return value, and stores it so the proxy can return it immediately. If the proxy isn't deployed yet, the user can send to the (codeless, EOA-looking) deterministic address with a **hint**, and a bundler deploys it on the fly.

**The bundle (up to 3 txs).** (1) deploy the proxy if needed; (2) load the lookup table ("if this call comes, do this"); (3) the user tx hits the proxy, which looks up and returns. Submitted together as a bundle.

**Ordering inside the L1 block.** The composer/proof ("post-batch") transaction must land **first**; the **user transaction second**. Only the user tx actually triggers the L2 state change — if it doesn't land, the name isn't registered. (In the explorer, Martin showed L1 block ~503 with this ordering.)

**The demo reality.** The first attempts **reverted** ("Okay, failed. Interesting… lifetimes are hard"); he fell back to a counter, then eventually got the registry working — `DEPCON = 2026`, registered **from L1**, with the entry's owner shown as the **proxy contract representing his L1 EOA** (`0x5D…53`). The repeated failures are an honest signal of maturity.

---

## 4 — Block building and timing (Martin)

An L2 can run faster blocks (e.g. 2 s), but anything that touches L1 must wait for the next L1 block (earliest inclusion). Pure-based operation can have intermediate blocks but is never faster than Ethereum without preconfirmations; alternatively a sequencer with ordering privilege runs the show.

**Three block types.**
1. **L2-only blocks** — fully sequencer-controlled; "no reason to ever reorg those."
2. **Sync blocks** — L1↔L2 back-and-forth; carry a **strong commitment to a specific Ethereum block** and **must reorg if that Ethereum block reorgs**.
3. **L1-hash-update blocks** — push the latest L1 state root onto the L2 so it can serve cheap static reads against fresh Ethereum state.

**"Pausing the chain."** ~4 s before the next L1 block, the sequencer commits to a synchronous block by "time-travelling": it builds a block that assumes a specific incoming L1 call with specific parameters, and assumes the return values of everything that L1 call touches. It can only *know* the outcome once the L1 block propagates. **The first naive implementation literally pauses the chain** until this resolves. A smarter version keeps building L2-only, **state-independent** transactions (e.g. plain token transfers) and gives users confirmations during the pause — but resolving conflicts ("is this user tx in conflict with the pending settlement?") requires the sequencer to track dependencies. Demand context: on Gnosis Chain there are only ~100–150 bridge txs/day, so a sync block is needed "only every couple of minutes."

**Why fast L1 state matters.** Pushing Ethereum's state root onto the L2 ASAP makes **static reads essentially free / pure-L2 cost**. Trade-off: Ethereum block propagation is delayed by validator timing games (~1 s), so ~2 s after a slot the block may not be available; making it available too early (low attestations) raises reorg risk, so there's a tunable wait.

**Settlement robustness (two levers).**
- **Multi-version proving.** Suppose 95 of 100 txs are pure-L2 and 5 each trade a *different* Uniswap pool — each of the 5 is a risk because the settlement commits to a return value. If proving is cheap you prove **multiple subset-versions** (4-of-5 trades, different 4-of-5, …, all 5) and submit them as bundles so the all-5 bundle pays the most; the builder includes the best feasible one, and you **fall back** to fewer trades rather than failing.
- **Less return-value dependence.** Instead of bridging an *exact* output amount (which any prior trade on that pool breaks), use a **slippage check that returns a bool** ("returns true if I got ≥ X"), so the value fed back to the L2 is robust.

---

## 5 — Proxy semantics, identities, gas, and the "one caveat"

**Message-sender aliasing.** Call the ENS contract from your L1 EOA and, on the L2, the `msg.sender` is the **proxy that represents your EOA** (a system address calls that proxy). So the registered entry is owned by the proxy, which is controlled only by your L1 EOA. Jordi's framing: each chain is its own "domain name"; **every address has a deterministic alias (proxy) in every chain**. You effectively have two identities — your L1 EOA (with its nonce) and your L2 proxy (no real nonce of its own). Your EOA's address *on the L2* is a different account from your *proxy* on the L2; this matters most for smart-contract accounts (e.g. Safes), where same-address-different-chain can mean different owners.

**Nonces and the cross-chain interface.** A transaction starts on one chain and is signed there, so its nonce lives on that chain. Chains are independent; the *only* thing connecting them is a **call**: from-address → to-address, optional value, calldata, returning (success, return-data). An L1 transaction's chain-ID makes its signature invalid on the L2 — so on the L2 the interaction is **not a raw transaction** but a **system transaction** that carries the call (system address → intermediate contract → proxy).

**The "one caveat."** Call a proxy **outside the composer bundle** and it reverts — the lookup isn't there. The SAFE engineer pressed the consequence: ERC-20's `balanceOf` must *not* revert, yet an L1 ERC-20 proxy would revert outside a bundle. Jordi conceded "this is the one caveat essentially," but argued that depending on a revert for useful logic is "a design flaw" (gas games can force out-of-gas reverts mid-contract anyway), so the practical exposure is limited. Consequence: you **cannot use the public L1 mempool** (it would just revert); orderflow must reach the composer — cleanest via a **bundle or account abstraction**.

**Who pays the gas.** The **user pays**. The user's own transaction is small (it just calls the proxy) but carries a **larger tip**; the composer fronts the two extra transactions, sends the builder the 3-tx bundle, and the builder returns enough to the composer to make it worthwhile. Paying on the L2 generally doesn't work (you have no, and want no, L2 funds).
- **Trust assumption (current):** a builder could include the user tx *without* the composer tx, causing a revert — this is not enforced by the L1 protocol.
- **Account abstraction fix:** bundle the whole sequence (deploy proxy, load calls, user tx) into one transaction → you can use **transient storage** (much cheaper) and arrange that the **user pays only if the transaction doesn't revert**, defeating the attack.
- **Cost figures:** the costly part is putting all the call returns / execution table into real on-chain storage — "roughly ± 400,000 gas"; a normal L1→L2 call "about 300K"; the meta-tx / transient-storage path "10 times less." (A "30K" figure was also mentioned in passing.)

---

## 6 — Following a chain; node software (Jordi)

**Two levels.** At the **composer** level you must synchronise all chains a transaction touches, execute them in order, and build the proof. But to merely **follow** a chain as an RPC/full node, you only need to follow **L1 and the L1 blobs**, extracting the information that affects your chain — you do **not** need to follow any other chain. The blobs contain everything required to rebuild your state. This is the independence guarantee: "one chain cannot mess up all the other chains"; a chain just receives/sends calls (special transactions).

**Malicious chains.** "What happens if a chain is malicious? Well, what happens if a smart contract is malicious?" It can hurt its own callers, not other chains — same as two contracts calling each other. **Preconfirmations are orthogonal** to EEZ ("introducing preconfs in the protocol at this stage is worthless" — useful later, but absolutely separable).

**Clients.** Today the clients are internal, **Rust-based**. The team wants to invite other client teams once definitions stabilise. Even with a multisig prover, having the different signers run **different implementations** is "a big security increase" — divergence → halt / manual intervention.

---

## 7 — Smart contracts: registering rollups and multi-prover (Jordi)

**Registering a rollup.** You pass a **rollup (manager) contract** implementing a specific interface, plus an initial state (genesis, or an imported current state). The manager implements three things:
1. an **`onRollupRegistered`** hook (do setup if needed);
2. **`checkProofSystem` + `getVerificationKeys`** — the key one;
3. a hook to **pass information from L1 to L2** (usually the state root or block hash; could be a bridge hash, a timestamp, etc.).

**How proving is chosen.** Each batch, the **composer chooses which proof systems apply to each rollup** ("rollup 1 → Zisk, rollup 2 → SP1, rollup 3 → Zisk, rollup 4 → TEE, …"). The manager is then asked whether that combination is acceptable; it can say no (revert) or yes and return the verification keys for those rollups. Each rollup **decides its own logic** — e.g. "prove with **Zisk OR SP1 AND a signature**." Jordi was explicit: "**it's not 2 of the 3. It's whatever you want** … you decide the logic." There is **no protocol-enforced two-prover floor.**

**Worked example.** 5 rollups: 2 use Zisk, 3 use SP1. For cross-chain activity among them you produce **two proofs** (one per system); when a batch is created **both proofs must be valid** — one invalid proof reverts everything. Each proof proves everything *except* the rollups it isn't responsible for (it "accepts anything" there). A rollup using only Zisk doesn't need to understand or trust SP1 — both ultimately **trust Ethereum** (the proof system is a contract that returns true). If a rollup uses *both* systems, it carries two verification keys and is proven by both.
**Rationale:** if all rollups share one system and stay L2-only, the L1 footprint is minimal — "essentially just the new state hashes," plus calldata, or with a validium not even that — and a single aggregated proof suffices. So EEZ gives full flexibility *and* the ability to share proof systems.

**Sovereignty.** Rollups are sovereign. A native rollup is "a smart contract" with a verification key; it can change the key via governance, a DAO, or an owner — entirely the rollup's choice.

---

## 8 — Chain types (Jordi)

- **Base rollups.** Anyone submits transactions; no coordination problem.
- **Centralized-sequencer rollups.** Understandable as a base rollup, except every transaction *and* state transition must be **signed by the sequencer**. Atomicity is framed like a database update:
  - **Optimistic** — "do it, and if something goes wrong, roll back" = **accept reorgs** (an L1 reorg can force the L2 to reorg).
  - **Pessimistic** — **locking**. Lock the whole chain while waiting for L1 finality ("maybe 20 minutes … or 12 minutes … whatever it takes") — inefficient — or **lock only the touched state** (partial lock).
- **Validiums.** "Perfectly fine in EEZ," provided the **composer** (and any follower) has access to the off-chain data that isn't stored on-chain.
- **Non-EVM / specialized state-transition functions.** The STF "can be anything" — EVM-based, non-EVM, a privacy chain (Zcash-like, a privacy pool), or "an Ethereum with some precompiles." "A way of extending Ethereum in your own way."

**Action-driven STF (ADSTF).** Because EEZ executes mid-call (a call can provoke a nested call, a return, a revert), a rollup's state transition isn't "block in → state out." It's **action-driven**: input an *action* (a transaction start, an incoming call, or a return), update the state, and emit the next action. Reverts and some optional info are carried too.

---

## 9 — The composer (Jordi)

The composer is "the entity building the batch of all the transactions" — collecting transactions from different chains that touch each other, executing the calls in order, and building the big transaction + blobs. It will "work very close together with builders." Conceptually the **composer and prover are separable roles**, and there can be **many provers** (and many composers, though "in principle you have one").

**Two things deliberately left open in v1:**
- **Composer incentive** — "we explicitly, at least in this first version, don't want to define an explicit mechanism" (but it's acknowledged necessary — candidate mechanisms: fees, higher fees, private flow, L2 fees).
- **Cross-chain mempool** — "we are not thinking in defining any public pool of transactions for L1↔L2 … at this stage." Centralized sequencers will likely collect transactions.

**Ordering.** The composer selects ordering; within a rollup, ultimately Ethereum decides. Two L2 swaps touching the same L1 state are ordered either by Ethereum (two separate L1 txs) or by the composer (same batch). Sequencer and composer can be separated — "every combination is possible."

---

## 10 — The batch / blob format: "EEZ Trace" (Jordi)

A cross-chain call is recorded as a series of **context switches**, like threads on a processor. Log-entry types: **transaction (start), end-transaction, call, return.** A nested flow (e.g. start on one L2 → call L1 → nested call to another L2 → another L2) produces deep context-switching, recorded in the blob with padding that shows the nesting. **Reverts** get a special log entry indicating **how many calls back** to unwind; occasionally you must execute a call even knowing it will revert.

This data sits in a **normal Ethereum L1 blob** — an **"EEZ blob/block"** that is **not chain-specific** (it can carry interactions across many chains); the EEZ contract on L1 reads it. Small payloads can go in **calldata** instead (cheaper). Recording every cross-chain interaction on L1 is what lets a follower rebuild its state from L1 alone — "How do you know somebody called you? … all the interactions between one chain and another are recorded somewhere in some block."

**Proof public inputs (`postAndVerifyBatch`).** A single struct carrying: the **execution-entry + lookup-entry tables** (L2 interactions; some in transient, some in real storage); the **proof-systems list** (no repeats); the **rollup × proving-system matrix** (each rollup decides acceptance, reverts otherwise); **blob indices**; a **calldata blob**; the **proofs (one per proof system, all must be valid)**; and a **block number** (info handed to the manager for L1→L2, usually a block hash). All of these variable-size inputs are compressed into a **single hash** the proof commits to (covering blob commitments, all L1 interactions, the manager info, and per-proof-system verification keys per rollup). The **verifier** is a single contract with one function returning true/false on that hash.

**Worked entry examples (Jordi).**
- *Single L2 transaction:* proxy-entry table is 0 (nothing to wait for); the entry is just a state delta — executes immediately. Scales identically to a million L2 txs, or to L2s calling each other (just update each rollup's state) — "from the L1 perspective you just update state."
- *L1 → L2 call:* the proxy-entry table is non-zero (you must wait for the L1 call). The info is stored; when the actual proxy call lands, it's matched against the expected (target, calldata, source, from) and the state deltas are processed — possibly across many rollups from a single call.
- *L2 → L1 call:* recorded as an L2 call with proxy-entry 0; a separate entry says "to execute this, do these L1 calls." The contract executes those L1 calls; if returns match (via a rolling hash), the state updates.
- *Static / async calls* use a separate **lookup table** (no state transition).

---

## 11 — Real-time proving (Jordi)

A proving system checks three things: blobs parse and are consistent; each rollup's STF is correct; and an **L1-hash parser** confirms the L1 interactions match the blob. Implemented (in Zisk) as roughly three provers: a blob parser, the STF provers, and the L1-hash parser.

**The cryptographic accumulator — the clever core.** A hash for a **set** (order-independent — `H(a,b) = H(b,a)`) that supports **addition and subtraction** (lattice-based / a "big hash" via elliptic curves; a sparse Merkle tree is the conceptual cousin). The rule: **add when you assume something, subtract when you prove it; at the end the accumulator must be 0** — everything assumed has been proven somewhere. Examples: the blob-data circuit computes a blob commitment and subtracts it; an initial state is assumed (add) and the final state is proven; consecutive blobs cancel (one's proof cancels the next's assumption), so you only need start and end states to match.

**The verification-key trick.** A rollup's VK can change (a new rollup defines a new, untrusted circuit). So rather than verifying directly, a **separate circuit verifies a circuit under a given VK** and subtracts in the accumulator, while a **"one-hash" circuit** proves the rollup-ID → VK mapping (the VK comes from the manager contract and is folded into the hash). Jordi called this "the fun thing — you need to prove something you don't know beforehand."

**Aggregation & output.** Everything is recursively aggregated ("infinite aggregation" — heavily used in Zisk); a final circuit checks the accumulator is 0; a **Plonk wrapper** produces the proof actually verified by the L1 contract.

**Speed.** Start proving as early as possible — even before any L1 interactions, on the early L2-only blocks, using "block markers" — and parallelise. **Currently ~3 seconds** of remaining work after an otherwise continuous, overlapped pipeline (measured "after the position of the bundle"); the whole thing must fit the **12-second** slot. The proof starts ~3–4 s before the slot and is published to the builder ~0.5 s before, inserted as a regular transaction. Bundle-building time depends on the number of batches.

**Hardware.** A cluster of **standard GPUs**, which scales well. Special ZK hardware exists but is immature because "the ZK is evolving a lot" — "no miracles … at this point it's GPUs." ZK is **not strictly required**: a TEE or a multisig can substitute (cheaper/faster, with a trust trade-off).

**Cycles & side-effects.** Cyclic call paths (L1→L2→L1) do **not** break proving — "any combination is possible." On the protocol-researcher's repeated push that "state transition generally includes side effects, not just returns": for **L2** side-effects, the proof commits to the **final state root** of every L2 touched, so all writes are implicitly proven (only the minimal data goes on L1); for **L1** calls, EEZ only cares about the **return value** matching expectation — side-effects on L1 are L1's business.

---

## 12 — Async / static calls (Martin + the Ethereum-researcher exchange)

A static read (or oracle lookup) is **free if you accept a possibly-outdated Ethereum block header** (an "async" call against the last header) — near-zero / pure-L2 cost. If you need the **latest** Ethereum state, you must **actually execute** the call on Ethereum, because you can't know you're top-of-block; any state you depend on could change before your settlement lands. Clean expression: **two proxies per Ethereum address** — one for synchronous calls, one for async static-on-last-header reads.

The Ethereum researcher worked the Optimism-quarter-second-blocks case: if a chain already knows "last Ethereum slot was 100," you can treat each L2 block as happening "epsilon after" block 100 and preserve a consistent history for reads. Martin's position: it's **use-case-dependent** — fine for resources you solely control; risky for shared resources where timing matters; and regardless, outgoing L1 calls are *executed and verified*, not assumed. The protocol researcher extended this to an "Amsterdam"/**Block-Access-List** world (per-transaction state deltas), where you might know state at every transaction rather than only top-of-block; Martin: they execute and verify returns either way.

**Ordering across chains.** The composer selects it; ultimately Ethereum decides within a rollup. "In EEZ everything is sequenced — Ethereum is the sequencer" (with the caveat that parallel L2s don't have a strict total order).

---

## 13 — Tokens and bridges (Jordi + Martin)

EEZ is **one level below bridges** — a general arbitrary-call / message-passing protocol. "How do you send ETH to an L2? You send ETH to your proxy contract on L1, and that implicitly results in a call with value."

**Three token patterns:**
1. **Own the token directly on Ethereum via a proxy.** Unlike today's bridges (which need a vault/lock-and-mint), an L2 user can *own* an L1 token through a proxy. But it's inefficient — every transfer needs an L1 call.
2. **A wrapper token on the L2.** Wrap once; then all activity is cheap L2 activity.
3. **One canonical L2 holding wrappers**, with everything L2↔L2 — "much, much cheaper," no L1 gas; you still pay the cross-chain composition friction, but far less than touching L1.

**For Gnosis concretely.** Wire the existing **AMB (Arbitrary Message Bridge)** and its token bridge into EEZ so that "from a user perspective, all the tokens stay the same," but you gain synchronous power. Today a Gnosis deposit takes ~half an hour; under EEZ you could, in one transaction, bridge from Ethereum, get a token on Gnosis, swap it, and come back. Two examples **already running on the devnet**:
- **Split a trade across an L1 AMM and an L2 AMM in parallel**, reverting both legs if the overall return isn't met.
- A **cross-chain flash loan**: borrow on L1, bridge to L2, swap, arbitrage two pools at different prices, repay — all in one transaction.

Bridges / token wrappers are **periphery contracts, not core EEZ** ("the core element is really one level deeper"). A standard/canonical bridge will be provided, plus a **helper** that ensures the recipient proxy exists (and creates it if not).

**Stateless proxies.** The proxies are **stateless**: their bytecode encodes the EEZ contract and which (address, chain) they represent; no storage, no local owner (it's held in the EEZ contract), ETH is forwarded immediately, and they **cannot self-destruct** (self-destruct is being removed from the EVM anyway).

---

## 14 — Builder / PBS integration and proving decentralisation

The composer must know the L1 state at the start of proving and ultimately deliver the proof; it's "certainly better to submit it to a builder as a bundle" than directly to L1. The proof targets the **next** L1 block: you're a couple of seconds into the current slot, building a proof valid in the upcoming block, making assumptions that the relevant L1 state won't change. L1 block-building happens in the last sub-seconds of a slot, so the EEZ proof (started much earlier) can be handed over ~0.5 s before and inserted as a regular transaction.

Stronger integration is possible but **not required**: if a validator agreed to give EEZ a guaranteed **top-of-block** slot, EEZ chains would lose the risk of building a proof that's rejected due to wrong L1-state assumptions. "If validators strongly integrate EEZ as a concept, it would make it stronger, but it's not necessary."

A proof can also be submitted in a **single transaction** (for L2-originated state transitions); the incentive to do so is to **collect the fees**, which is a potential path to **decentralising proving**.

---

## 15 — Status and roadmap

- **Contracts:** in a **"semi-finalised" state** — fully working with many passing tests, but the team wants one more iteration (gas savings, reduced complexity) and is "not feeling like fully happy to declare as final." Jordi: "everything at the end hangs on these smart contracts."
- **Live now:** an internal **Devnet** (previous contract version) and a more stable deployment with the **most-recent contracts** (partial functionality) on **Chiado** (the Gnosis test network).
- **August:** the reference rollup deploys as **Rollup 0**, hardening into **Rollup 1** — a vanilla, fully based rollup sequenced by L1 validators, "as pure as possible."
- **End of summer:** a deployment on **Ethereum mainnet** that can hold real value — but explicitly shipped as a **testnet**: "use at your own risk," only "slightly audited," with "a big warning to **not use it with real money**."
- **Optimistically by end of year:** **Gnosis Chain becomes the first EEZ L2**, in **limited capacity** — synchronous **single calls** ("probably not unlimited nested back-and-forth reverts"), with **full shared liquidity targeted for the same window but nested calls coming later**. The launch prover is a **compromise**: "certainly not purely ZK-based, hopefully already partially ZK-based, complemented by ultimately a multisig or maybe fancy multisig called TE[E]."
- **Beyond:** a couple of other L2s have expressed interest (no concrete timeline — "they want to see it work first"). Clients are internal/Rust today; inviting other client teams is "soonish."

**Asks from the room:** application teams to test against; client developers to build alternative implementations; and any spec feedback. There's at least one Telegram group. Contact named on the call: **"Arman is the best person"** (Armagan). Jordi's closing: "it's really an important project for bringing the Ethereum community together … the missing piece we want to build … the Ethereum way: here is a smart contract, if you like it, use it."

---

## Notable open questions / honest gaps (as stated on the call)

- **Composer incentive** and a **cross-chain mempool** are deliberately undefined in v1.
- The current sync-block implementation **pauses the chain** (keep-building-during-pause is future work).
- The **outside-the-bundle revert** breaks must-not-revert specs (ERC-20 `balanceOf`) — a real, named caveat.
- Builder cooperation is an **unenforced trust assumption** today; validator top-of-block integration would strengthen it but isn't required.
- **Address portability** across chains (the SAFE concern) is acknowledged UX friction. Jordi would have preferred per-chain addresses ("then you do a call and already know which chain"), but backwards-compatibility with 20-byte addresses forced the proxy approach. The EF chain-specific-address initiative was raised; treated as "a problem for later," with the note that EEZ "doesn't take anything away" if you can get the same address on another chain.

---

## Key numbers at a glance

| Figure | Meaning |
|---|---|
| **~12 s** | Native synchronous settlement (one L1 slot) — the headline |
| **~3 s** | Current proof-generation work remaining inside a slot (a target, **not** a finality number) |
| **~20 min / ~12 min** | L1-finality wait — the stall cost of a full-chain pessimistic lock |
| **30 min – 7 days** | Today's bridge settlement times EEZ is replacing |
| **~400K / ~300K gas** | Full real-storage path / normal L1→L2 call (AA + transient ≈ 10× cheaper) |
| **~100–150/day** | Gnosis Chain bridge txs → a sync block needed only every couple of minutes |
| **2 things** | What fixes a proxy address: target contract address + chain ID |
| **3 txs** | Max composer bundle: deploy proxy → load lookup table → user tx |
| **5 rollups / 2 systems → 2 proofs** | Multi-prover worked example; both proofs must be valid or all reverts |

---

## Appendix — Audience Q&A

Reconstructed from the recording (diarisation is imperfect, so audience roles are inferred from content — the SAFE engineer, the Ethereum researcher, the protocol researcher — and the team responder is named where clear).

### Proxies & addressing
1. **Q: What does the proxy/stub on L1 look like — canonical, just the API?** → *Martin:* a generic contract with a single fallback handler; supports any ABI; forwards the full calldata + value to the EEZ contract — "just a forwarder" recording input/output pairs.
2. **Q: Must the proxy address equal the deployed contract's address, or can it route anywhere?** → *Martin:* the L2 target keeps its own address; the L1 proxy is different but **deterministic from two things — contract address + chain ID**.
3. **Q: So it's easy to determine where the proxy is?** → *Martin:* easy-ish, but ~20 chains × ~1M accounts ≈ 20M potential proxies, so you usually pass a **hint** (CREATE2 addresses are possible).
4. **Q: Could it auto-deploy proxies if they don't exist?** → *Martin:* yes — the composer bundles (1) deploy proxy, (2) load lookup table, (3) user tx. Three txs as a bundle.
5. **Q (SAFE): Is there msg.sender aliasing? Calling from L1 do I get a different sender?** → *Martin:* yes — on L2 the `msg.sender` is the **proxy of your EOA**; the entry is owned by that proxy, controlled only by your L1 EOA.
6. **Q (SAFE): What if I call the proxy outside the composer bundle?** → *Martin:* it looks up the call, doesn't find it, and **reverts**.
7. **Q (SAFE): But ERC-20 `balanceOf` must not revert, and the proxy does outside a bundle.** → *Jordi:* "this is the one caveat." Depending on a revert for logic is a design flaw; you can't use the public L1 mempool — route via composer/bundle/AA.

### Gas, payment & transactions
8. **Q: Who pays the gas for the composer transaction?** → *Martin:* the **user** — a small tx plus a larger tip; the composer fronts the extra txs and the builder reimburses it from the tip.
9. **Q: A builder could include the user tx *without* the composer tx, so it reverts — allowed since it's not part of the L1 protocol.** → *Martin:* correct, a current **trust assumption**. AA + transient storage fixes it (user pays only if it doesn't revert). ~400K gas full real-storage, ~300K normal L1→L2, ~10× cheaper via transient.
10. **Q: An L1 tx to the proxy is mirrored on L2 — but the L1 chain ID makes the signature invalid on L2.** → *Martin:* it isn't a raw tx on L2; it's a **system transaction** carrying the call.

### Nonces, identities & ordering
11. **Q: What's the nonce of a tx from mainnet vs one a second later on Gnosis?** → *Jordi/Martin:* the nonce lives on the **originating chain**; chains connect only via calls. You have **two identities** (L1 EOA + L2 proxy).
12. **Q: So the message sender differs by origin?** → *Martin:* yes — from L1 it's the proxy of your EOA; directly on L2 it's just the EOA.
13. **Q (Ethereum researcher): How do you order cross-chain transactions — do L2s compete with tips?** → *Martin/Jordi:* the **composer** selects ordering; within a rollup, ultimately Ethereum decides. Same-L1-state swaps are ordered by Ethereum (separate txs) or the composer (same batch).

### Node architecture & following chains
14. **Q: Same client or two clients, and what's the communication?** → *Jordi:* distinguish the **composer** (must synchronise every touched chain) from a **follower** (only needs L1 + blobs, never other chains).
15. **Q (protocol researcher): You can't validate an L1→L2 tx without seeing the other chain.** → *Jordi:* you **prove** it — the proof asserts the call is in the blobs and the STF followed the rules; once on Ethereum it's settled history; chains stay independent via proofs.
16. **Q (protocol researcher): Your diagram shows an interchain dependency with no blob.** → *Jordi:* it's **guaranteed via proof** (ZK or TEE).

### Static / async calls
17. **Q: Can static calls that don't touch state be done free of charge?** → *Martin:* essentially yes — that's why they push Ethereum's state root onto the L2 fast; static reads are near-free / pure-L2 cost.
18. **Q (audience): ERC-20 is non-commutative — a later tx could invalidate a transfer.** → *Jordi:* as sequencer you have full control and can resolve whether something conflicts.
19. **Q: Is that on the sequencer side?** → *Martin:* if it wants to give fast confirmations it tracks dependencies; otherwise the user learns later that it reverted.
20. **Q (Ethereum researcher): Optimism quarter-second blocks — can an L2 block static-call L1 and stay a consistent history "epsilon after" the L1 block?** → *Martin:* use-case-dependent — fine for solely-controlled resources, risky for shared/timing-sensitive ones; outgoing L1 calls are **executed and verified**, never assumed top-of-block.
21. **Q: How do you express the sync-vs-async preference?** → *Martin:* **two proxies** for the same address — one synchronous, one static-on-last-block-header.
22. **Q (protocol researcher): Why make the start of the block a special case? / In an Amsterdam BAL world you don't know state per-tx.** → *Martin:* if you could assume top-of-block everything's deterministic, but you can't, so you execute and verify the returns regardless.
23. **Q: Would async oracle calls break atomicity?** → *Martin:* same as the key lookup — async-on-last-header is cheap pure-L2; needing the latest forces an L1 execution.

### Multi-prover & proof systems
24. **Q (after break): Special hardware / a GPU cluster for proving?** → *Jordi:* Zisk uses **standard GPUs** in a cluster, scales well; special ZK hardware is immature — "no miracles… it's GPUs."
25. **Q: With 3 rollups on 2 proof systems, are same-system proofs aggregated?** → *Jordi:* yes — rollup 1 Zisk, rollup 2 both (two VKs), rollup 3 SP1; **two proofs total**, each proving everything except the rollups it isn't responsible for; **both must be valid or all reverts**.
26. **Q: Does a cycle in the calls (L1→L2→L1) break proving?** → *Jordi:* no — any nesting/combination works.
27. **Q (protocol researcher): You prove returns, but real execution has side effects too.** → *Jordi:* for L2 the proof commits to each touched L2's **final state root** (all writes implied); for L1 EEZ only cares the **return value** matches.

### Proving internals, timing & blobs
28. **Q: The ~3 seconds — that's after the position of the bundle?** → *Jordi:* yes.
29. **Q: How long does bundle-building take / what boundary are we hitting?** → *Jordi:* depends on batch count; the whole thing must land within the **12-second** slot.
30. **Q: What are the blobs in the proof — the whole rollup blob?** → *Jordi:* a **normal Ethereum L1 blob** ("EEZ block"), not chain-specific; it carries the interchain context-switching.
31. **Q: Will the composer prove all txs on all chains? / Can parts be pre-built?** → *Jordi/Martin:* prover and composer are **separable** (many provers); chains can pre-prove their own L2-only parts; the split is open.

### Builder/PBS integration & proving decentralisation
32. **Q (SAFE): Do composer/prover need a builder for the initial L1 state?** → *Martin:* the prover is part of the composer; you submit a **bundle to a builder**; the L1 state must be known at proving start.
33. **Q: With a builder you get a guarantee you can't get otherwise?** → *Martin:* yes — you make an assumption; **validator top-of-block integration** would remove the risk but isn't necessary.
34. **Q: A proof can be submitted in a single tx — what's the incentive, could it decentralise proving?** → *Martin:* for L2-originated transitions the incentive is **collecting the fees** — a potential proving-decentralisation path.
35. **Q (protocol researcher): Current or next block? By the time you see the block it's too late.** → *Martin:* you build for the **next** block; the proof starts ~3–4 s before the slot, is published ~0.5 s before, and the builder inserts it as a regular tx.

### Tokens, bridges & status
36. **Q: Will you implement bridging contracts as part of EEZ?** → *Martin:* EEZ is **one level below bridges** (arbitrary calls); sending ETH = a call with value; token bridges are **periphery** — a canonical bridge + a proxy-creating helper will be provided.
37. **Q: How do you solve tokens with Gnosis?** → *Martin:* wire the existing **AMB** into EEZ so tokens stay the same; today a deposit takes ~30 min, but EEZ lets you bridge + swap + return in one tx (devnet already runs a split-trade and a flash-loan arbitrage).
38. **Q: Are the proxies completely stateless — a nonce, self-destruct?** → *Jordi/Martin:* **stateless** (bytecode = EEZ contract + which (address, chain) it represents), no storage/local owner, ETH forwarded immediately, **cannot self-destruct**.
39. **Q: Can we use it today? Which L2s are registered — testnet or mainnet?** → *Martin:* Devnet + Chiado now; contracts semi-finalised; Ethereum **testnet** end of summer ("not for real money"); Gnosis the first EEZ L2 by end of year, limited.
40. **Q (SAFE): This changes the portable-address paradigm — align with the EF chain-specific-address initiative?** → *Jordi/Martin:* they'd prefer per-chain addresses, but backwards-compatibility forced proxies; "a problem for later," and EEZ "doesn't take anything away."
41. **Q: Who do we contact to test an application?** → *Team:* **"Arman is the best person"**; there's a Telegram group.

---

*Digest compiled 2026-06-20 from the 17 June 2026 BuilderRoom workshop recording. This is the full-recording companion to the deck digest (`dappcon-2026-eez-node-architecture.md`), the proving talk (`dappcon-2026-realtime-proving-talk.md`), Martin's overview (`dappcon-2026-martin-eez-overview-talk.md`), and the Gnosis-chain talk (`dappcon-2026-gnosis-chain-eez-talk.md`). Engineering-level founding material — quote as the builders' framing, not approved EEZ comms.*
