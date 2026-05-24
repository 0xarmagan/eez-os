# Source: Vitalik & Jordi Baylina — Fireside Chat

**Video Title:** "Vitalik & Jordi Baylina Fireside chat"  
**Participants:** Vitalik Buterin (Ethereum Foundation) & Jordi Baylina (Zisk)  
**URL:** https://www.youtube.com/watch?v=prgWmgUbSVQ  
**Format:** Fireside chat — Vitalik asks, Jordi answers

---

## Overview

High-signal technical conversation between Vitalik Buterin and Jordi Baylina on the EEZ architecture. Vitalik probes the system's assumptions, security model, block building economics, and developer experience. The most technically rigorous public examination of EEZ to date — Vitalik both validates the approach and stress-tests it with adversarial questions.

---

## Key Moments by Timestamp

| Timestamp | Topic |
|-----------|-------|
| `0:46` | Jordi defines EEZ: synchronous cross-rollup calls, same-transaction atomicity across chains |
| `2:05` | Why EEZ is possible now: ZK maturity reached real-time block proving |
| `4:08` | Core mechanic: Ethereum as shared trust layer + single aggregated ZK proof per bundle |
| `6:20` | Proof system flexibility: rollups choose their own, can trust multiple — EEZ isn't locked to one |
| `9:55` | Sync vs. async: atomic cross-chain DeFi impossible to guarantee asynchronously |
| `13:32` | Block building: "composer" role introduced; validators remain final arbiters |
| `17:20` | Block time trade-off: sub-12s possible with centralized sequencer; pre-confs may help |
| `23:30` | Developer UX: proxy addresses for cross-chain contracts; same Solidity call/revert semantics |
| `31:51` | Security model: broken proof system infects only callers of that chain; isolated users unaffected |
| `37:05` | Roadmap: smart contracts + docs imminent; experimental mainnet + Devcon Berlin workshops |

---

## Vitalik's Questions & Jordi's Answers

### Q1 — `1:49` — Origin of the idea
**Vitalik:** "When did you first decide this dream was important? What made you realize it?"

**Jordi:** ZK proving matured to the point where real-time block proofs are feasible. Synchronous composability seemed impossible for years — the recent capability leap in proving speed changed that.

---

### Q2 — `3:37` — EEZ vs. a single advanced rollup
**Vitalik:** "Walk me through the components. What's the difference between EEZ and just having one existing rollup like Base going to stage 2?"

**Jordi:** EEZ is about multiple sovereign rollups sharing Ethereum as settlement. A single ZK proof covers transactions across all participating chains atomically — it's not about one chain getting better, it's about making all chains interact as if they were one.

---

### Q3 — `5:58` — Multi-proof-system integration
**Vitalik:** "This requires deep integration between L2s. If each has its own proof system, how do merged proofs work?"

**Jordi:** EEZ doesn't mandate a single proof system. Each rollup defines which proof systems it trusts (can be multiple, including non-ZK systems like multisig). Proofs aggregate per proof system, not per chain.

---

### Q4 — `9:07` — Why synchronous over asynchronous
**Vitalik:** "Async is much easier to implement. What can sync enable that async cannot?"

**Jordi:** Atomic multi-chain transactions. Example: buy a plane ticket on one chain + hotel on another + insurance on a third, all in one transaction. In DeFi: flash loan on chain A, swap on chain B, return — all atomically. Async requires complex application-level coordination and cannot guarantee atomicity; sync pushes complexity to infrastructure and simplifies the application layer.

---

### Q5 — `12:00` / `13:05` — Block building economics and centralization
**Vitalik:** "Who builds these blocks? Once TPS goes above a threshold, you get to a point where only one actor can build everything. How does this not become a monopoly?"

**Jordi:** Introduces the "composer" role: aggregates transactions from multiple chains into a bundle, hands to a standard Ethereum builder. Not meaningfully different from current MEV/PBS dynamics. Validators retain final say over which builders and composers they use — centralization risk is the same as existing Ethereum block building, not a new problem.

---

### Q6 — `16:09` — Block time vs. composability trade-off
**Vitalik:** "L2s have 250ms block times. If EEZ syncs with L1, you can't go below 12 seconds. Is this a problem? Can you have both speed and composability?"

**Jordi:** Trade-off is real. Sub-12s is possible with a centralized sequencer (valid, just requires a trust assumption). Pre-confirmations on Ethereum may help. Partial locking is another option — lock only the accounts involved in cross-chain calls rather than the entire L2. Both options are feasible but require explicit design choices.

---

### Q7 — `20:19` — Developer and user experience (train/hotel problem)
**Vitalik:** "Base joins EEZ. Eurowings sells flight NFTs on Base. A hotel sells bookings on L1. A user wants to buy both atomically. What does the developer's smart contract look like? How different from L1?"

**Jordi:** Identical to L1. Same function, two calls, reverts if either fails. User notices nothing different. Each contract on another chain has a proxy address on Ethereum — calling the proxy is equivalent to calling it natively. If the remote call reverts, the entire transaction reverts.

---

### Q8 — `23:24` — Proxy address model
**Vitalik:** "So every object on Base gets a virtual address on Ethereum?"

**Jordi:** Yes — a proxy address represents the smart contract. Calling the proxy triggers the cross-chain execution; reverts and return values propagate back correctly.

---

### Q9 — `24:23` — Case for tight coupling with proof systems
**Vitalik:** "L2s worry about tight coupling with Ethereum and proof systems. What's your case for them to embrace it?"

**Jordi:** Chains with different proof systems can still communicate. Each side maintains a "proof record" — what it proved and what it assumed. Both sides must agree on the same record hash. Neither proof system needs to understand the other's execution. Sovereignty is fully preserved.

---

### Q10 — `26:27` — Proof system isolation
**Vitalik:** "Does Base's proof system need to understand Arbitrum's execution to verify cross-chain calls?"

**Jordi:** No. Each chain proves only its own side. Arbitrum could be pure WASM (Stylus); Base doesn't need to know. Each chain also only needs to follow itself — no requirement to sync or understand other chains' state transitions.

---

### Q11 — `27:39` — Who authorises cross-chain trust
**Vitalik:** "Arbitrum is WASM-only. Base doesn't understand WASM. When a call flows from Base → Arbitrum → Base, who on the Base side authorises trusting the Arbitrum result they cannot verify?"

**Jordi:** Each rollup's smart contract on Ethereum defines its accepted proof systems and upgrade rules. The protocol proves that both sides agreed on the same cross-chain interaction record — one side proves, one side assumes, and both must agree on the hash. Neither needs to understand the other's VM.

---

### Q12 — `30:07` — Attack vector: Base proof system breaks
**Vitalik:** "Let's say Base's proof system breaks. An attacker forges Base's outbox with crazy calls — sending Vitalik's tokens to Justin Sun, minting USDC to Justin Moon. Arbitrum records these calls in its inbox. Why doesn't this break Arbitrum?"

**Jordi:** A pure Arbitrum user is safe — no one can steal their funds from broken Base. If you're in Arbitrum and you called into Base, you'll see the (malicious) Base return value accepted as fact. But this only affects contracts that chose to call into Base. It's equivalent to calling a malicious smart contract — your risk, not a systemic Arbitrum failure.

**Vitalik's follow-up summary (`32:37`):** "So a normal Arbitrum contract sees a bunch of random incoming calls from addresses it doesn't recognise. If the contract is secure, it ignores them."
**Jordi:** Exactly. Beyond that, each application decides whether it trusts a given chain and proof system — same as trusting any external contract.

---

### Q13 — `33:50` — Most exciting applications
**Vitalik:** "What applications are you personally most excited about that can't happen on L1 and can't happen with everyone on one L2?"

**Jordi:** (1) Extending Ethereum: custom chains with precompiles too expensive on L1 — specialised elliptic curves, ZK-specific operations. (2) On-chain voting requiring complex cryptographic primitives at millions-of-votes scale, secured by Ethereum. (3) DeFi, insurance, identity, micro-payments. Core thesis: EEZ lets teams experiment with new Ethereum primitives without forking the main chain.

---

### Q14 — `36:57` — Near-term roadmap
**Vitalik:** "What's the first thing Ethereum land can expect from you?"

**Jordi:** Smart contracts + full specification/documentation release imminently. Experimental mainnet deployment (not production-ready). Workshops at Devcon Berlin (next month at time of recording). Goal: two chains running — likely a Gnosis Chain variant plus one new chain. Open community process to follow and stabilise from there.

---

### Q15 — `39:13` — Long-term fit: any L2 that wouldn't work?
**Vitalik:** "Is there any L2 today that would not be a good fit for EEZ?"

**Jordi:** No. EEZ is designed to be maximally flexible — non-EVM chains, WASM chains, even Bitcoin in theory if you define the inbox/outbox interface. The only requirement: implement the standard for accepting and making cross-chain calls.

---

## Key Takeaways

1. **Vitalik validates the security model.** His adversarial questioning (broken proof system attack) concludes with: "it sounds like a good architecture." This is significant external validation from the most credible voice in Ethereum.
2. **Proof system agnosticism is the core differentiator.** Chains don't need to trust each other's VMs — only agree on a shared interaction record hash.
3. **Developer UX is identical to L1.** Same Solidity patterns, same revert semantics. EEZ is infrastructure-level complexity hidden from application developers.
4. **Sovereignty preserved.** Each chain follows only its own state; no chain needs to sync or understand others. RPC nodes don't change.
5. **Composer role is the new block builder.** Centralisation risk acknowledged as equivalent to existing Ethereum PBS — not new, not worse.
6. **Bitcoin can join.** EEZ is not EVM-specific — any VM with a definable state transition function can participate.

---

## Marketing Angles from This Source

- **"Vitalik calls it a good architecture"** — highest-credibility external endorsement possible
- **"Same Solidity code, now cross-chain"** — developer adoption narrative
- **"Broken L2 can't break your Arbitrum funds"** — security isolation message
- **"Not locked to one proof system"** — ecosystem-neutral, anti-lock-in positioning
- **"Even Bitcoin could join"** — universal composability narrative

---

## Related Files

- [Paul-Barron-Interview.md](Paul-Barron-Interview.md) — retail-facing overview with Jordi and Friederike
- [ZisK-Jordi-Baylina-Transcript-ETHPRAGUE.md](ZisK-Jordi-Baylina-Transcript-ETHPRAGUE.md) — Jordi's solo technical deep-dive on ZisK proving
- [EEZ-Community-Call-Transcript-Summary.md](EEZ-Community-Call-Transcript-Summary.md) — community call technical summary

---

*Last updated: 2026-05-19*
