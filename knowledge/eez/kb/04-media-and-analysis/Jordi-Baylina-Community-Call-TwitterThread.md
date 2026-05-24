# EEZ Community Call #1 - Twitter Thread
**By: Armagan Ercan, Ecosystem Relations Lead**

---

## Thread Version (for Twitter/X)

**[THREAD] We hosted our first EEZ Community Call with Jordi Baylina (@ZiskDev). If you build on L2s, this is urgent reading. Here's what's coming and what it means for your chain.**

---

**[1/23]**
Today's problem is real: you choose an L2 for good reasons (throughput, cost, community). But that choice isolates you from other L2 ecosystems. Your users can't atomically interact across chains. Your liquidity fragments. Your composability breaks.

Builders have lived with this for years.

---

**[2/23]**
EEZ fixes this. It lets you call a contract on Base and a contract on Optimism in the same transaction, with the same atomicity guarantees as calling contracts on L1.

Not asynchronous. Not through a bridge. Atomic. Real composability.

And your L2 stays sovereign. You keep your sequencer, your rules, your proof system.

---

**[3/23]**
This wasn't possible until now. Synchronous cross-chain composability requires real-time proof generation.

ZK got fast enough. Jordi's team proved an entire Ethereum block in ~7 seconds. Fast enough to prove alongside block construction. Fast enough for EEZ to work.

---

**[4/23]**
How it works: your chains join an EEZ alliance. Developers submit cross-chain transactions. EEZ bundles all the state changes into a single proof Ethereum settles. All succeed or all fail.

Your L2 tracks its own state. You don't cede control. You just gain composability.

---

**[5/23]**
Real implication: DeFi protocols stop fragmenting capital. A flash loan on one chain can power a swap on another. Liquidity aggregates. Yields compound across the entire ecosystem.

Commerce works. Insurance works. Any atomic operation that's broken into async steps today becomes possible.

---

**[6/23]**
For your developers: they write cross-chain code the same way they write L1 composability. No new frameworks. No async recovery logic. Same smart contract patterns.

That matters. Lower barrier to build. More use cases ship faster.

---

**[7/23]**
Architecture note: execution tables define cross-chain calls. Proxy contracts let you call remote contracts locally. A single proof on L1 settles everything.

(For the technically curious, Jordi walked through the full design in the call. Watch the recording.)

---

**[8/23]**
Proof system agnosticism matters. Base doesn't need to trust Arbitrum's prover. Arbitrum doesn't adopt Base's. Each chain picks the proof system it believes in.

That's sovereignty. That's why this works as an ecosystem framework, not a monolithic bet.

---

**[9/23]**
To be direct: this means the EEZ Alliance isn't a single technology lock-in. It's a composability protocol that respects chain independence.

For a chain team, that means: you're not forced to change your prover. You're not hostage to another chain's upgrade cycle.

---

**[10/23]**
Real talk on constraints: synchronous composability couples you to L1 block times initially. ~12 seconds for Ethereum. That's the trade-off for atomicity at launch.

Early chains will prioritise atomic correctness over latency. Later, preconfirmations and hybrid locking will push speed higher.

For DeFi / commerce, correctness first is the right trade.

---

**[11/23]**
On MEV and builders: composers aggregate cross-chain transactions. Builders create blocks. Validators pick builders. That's the same builder-concentration game Ethereum runs. It's not a new risk; it's an existing risk you're already familiar with.

Early adoption favours larger operators. That's the honest assessment.

---

**[12/23]**
Decentralised prover markets will help later. Right now, proving is expensive (12 GPUs per block). Early chains bear that cost or Gnosis carries it. Long term, proving commoditises and decentralises. This is a Phase 1 constraint, not a fundamental limitation.

Answer: Composers aggregate cross-chain transactions. Builders create blocks. Validators decide who to trust.

Risk? MEV concentrates around builders/composers who see transactions first.

But validators have final say. They control decentralization.

---

**[13/23]**
Security model: if a proof system breaks, isolation holds. Other chains keep working. But your smart contracts must validate calls from untrusted chains. Same as calling any external contract. Validation is on you.

That's the model. Straightforward. No cascading failures.

---

**[14/23]**
Use cases for chain teams:

1. **Specialised chains.** Want custom precompiles for voting? ZK-heavy optimisations? Build your own L2, prove it works, be atomic with Ethereum and others.

2. **DeFi aggregation.** Flash loans across chains. Swaps across liquidity pools. Atomic. No slippage tax from sequential hops.

---

**[15/23]**
3. **Commerce and contracts.** Travel: plane + insurance + hotel in one transaction. DeFi: borrow on A, swap on B, lend on C. All atomic or all fail.

This only works if you're part of the EEZ Alliance. If you join, you unlock use cases that don't exist today.

---

**[16/23]**
Why would your chain join? Because composability drives TVL. Composability drives use cases. Use cases drive adoption. That's the network effect you're competing for.

Right now, EEZ is the only framework that delivers it without forcing you to compromise sovereignty.

---

**[17/23]**
What chains can join? Not just EVM. EEZ is protocol-agnostic. Base, Arbitrum, Optimism, Gnosis, Solana-style chains, even Bitcoin if call semantics are defined.

Single requirement: track Ether holdings. That's it. You keep everything else.

---

**[18/23]**
What's the timeline for chain teams?

**Next month:** Draft smart contracts on mainnet. Experimental. For testing.

**June at DappCon:** Live workshops. Testnet with 2-3 chains. Real integration.

**After June:** Proving systems harden. More chains onboard. No L1 hard fork. Everything through smart contracts.

---

**[19/23]**
For chain teams thinking about integration: we've built the integration guideline. We know what adoption looks like. We're signing Wave 1 chains now.

If you're serious about composability, this is the moment to move.

---

**[20/23]**
The frame: Ethereum's next challenge isn't scaling. It's composability. EEZ is the answer.

Every L2 that joins makes the entire Ethereum ecosystem more valuable. That benefits everyone.

---

**[21/23]**
To chain teams: if you want to grow liquidity, unlock new use cases, and stay sovereign, join the Alliance.

To developers: start thinking about cross-chain architectures. Tooling is coming.

To everyone else: watch DappCon. This matters.

---

**[22/23]**
Questions? Read the technical spec. Watch the call recording. Or reach out to our team directly.

This is Phase 1. We're building the foundation. The interesting part comes next.

---

**[23/23]**
One Ethereum. Many chains. Atomic composability.

That's the goal. That's what we're building.

**#EEZ #Ethereum #L2 #Composability**

---

## Thread Notes

**Total tweets:** 23  
**Key talking points covered:**
- Problem (L2 isolation, broken composability)
- Solution (atomic synchronous composability)
- Why chains should join (composability → TVL → adoption)
- Architecture (proof system agnosticism, sovereign chains)
- Trade-offs (block times, builder concentration, early costs)
- Security model (contract-level validation, isolation holds)
- Use cases (DeFi aggregation, specialised chains, commerce)
- Chain agnosticism (EVM, non-EVM, protocol-agnostic)
- Roadmap (integration now, testnet June, hardening after)

**Tone:** Direct, partnership-focused, honest about constraints, calls chain teams to action

**Voice:** Ecosystem Relations Lead. Addresses chains as partners, not just audience. Emphasises adoption, integration, TVL, and network effects.

**Call-to-action:** Join Wave 1. Integrate now. Watch DappCon. Reach out to our team.

---

## LinkedIn Version (Longer Format)

---

### LinkedIn Post: "Building the EEZ Alliance: What Chain Teams Need to Know"

We just hosted the first EEZ Community Call with Jordi Baylina (Zisk), and I want to share what this means for chain teams and builders. We're at the moment when EEZ moves from proof-of-concept to real integration. Here's why that matters.

**The Real Problem**

L2s were supposed to scale Ethereum. They did. But they also fragmented it. Now you have 20+ chains with fragmented liquidity, isolated ecosystems, and composability that breaks at the chain boundary. Developers choose a chain for good reasons (throughput, cost, community). But they lose composability.

For years, the solution was bridges. Bridges are slow, centralised, and async. They don't fix the problem. They just manage it.

**The Answer: Atomic Synchronous Composability**

EEZ lets you call a contract on Base and a contract on Arbitrum in the same transaction. Not async. Not through a bridge. Atomic. And your chain stays sovereign. You keep your sequencer, your proof system, your independence.

This is only possible because ZK proofs got fast enough. Jordi's team proved an Ethereum-scale block in ~7 seconds. Fast enough to prove alongside block construction.

**Why Chains Should Care**

Composability drives TVL. TVL drives adoption. Right now, EEZ is the only framework that gives you composability without forcing you to sacrifice sovereignty.

That's the network effect you're competing for: one Ethereum ecosystem instead of 20 isolated chains.

**What This Means Architecturally**

EEZ is proof-system agnostic. Base doesn't need to trust Arbitrum's prover. Arbitrum doesn't adopt Base's rules. Each chain picks the technology it believes in. They still compose atomically.

That's critical. This isn't a monolithic bet on one system. It's a composability protocol that respects chain independence.

**The Real Constraints (Let's Be Honest)**

Synchronous composability couples you to L1 block times at launch. ~12 seconds for Ethereum. Early chains choose correctness over latency. Later, preconfirmations and hybrid locking improve speed.

Proving is expensive right now (12 GPUs per block). Early chains bear that cost or we carry it. Long term, proving commoditises and decentralises. This is Phase 1. We're still in it.

Builder concentration is real. It's not new—it's the same MEV game Ethereum runs. Validators have final say on which builders they trust. Decentralised prover markets come later.

**For Chain Teams: What This Unlocks**

1. **Specialised chains** that extend Ethereum. Custom precompiles, ZK optimisations, specific use cases. Atomic with the rest of the ecosystem.

2. **DeFi that actually works across chains.** Flash loans, swaps, liquidity aggregation. All atomic. No slippage tax from sequential hops.

3. **Network effects across the entire Ethereum ecosystem.** Not 20 isolated chains. One ecosystem, many chains. That's TVL. That's adoption.

**The Timeline**

We're signing Wave 1 chains now. Testnet goes live at DappCon (June). After that, hardening and gradual onboarding.

No L1 hard fork. Everything through smart contracts.

**The Ask**

If you lead a chain team: we're building the integration guideline. We've mapped the process. We know what adoption looks like. This is the moment to move.

If you build on L2s: start thinking about cross-chain architectures. Tooling is coming.

**The Bottom Line**

Ethereum's scaling problem is solved. The next challenge is composability. EEZ is the answer. It works through smart contracts. It respects chain sovereignty. It scales to any chain.

One Ethereum. Many chains. Atomic composability.

That's what we're building.

**#EEZ #Ethereum #L2 #Composability #DeFi**

