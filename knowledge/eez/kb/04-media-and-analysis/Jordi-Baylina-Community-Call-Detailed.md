# EEZ Community Call #1 - Detailed Transcript Summary
**Guest:** Jordi Baylina (Zisk)  
**Topic:** Synchronous Composability Between Rollups via EEZ  
**Date:** May 2026

---

## Executive Summary

EEZ enables **atomic synchronous composability** across multiple rollups and L1, allowing smart contracts to interact across chains within a single transaction with all-or-nothing execution semantics. This contrasts with asynchronous bridges where operations fail partially and require recovery mechanisms.

The core innovation is using **validity proofs generated in real-time** (enabled by ZK proving maturity) to bundle cross-chain state transitions into a single atomic proof that Ethereum verifies on L1.

---

## Core Problem & Vision

### The Problem
- Multiple L2s exist but operate independently ("they don't talk to each other")
- Current bridges are **asynchronous** and **centralized**—breaking Ethereum's composability model
- Cross-chain operations fail partially, creating zombie states and requiring manual recovery

### The Vision
**From a single rollup, call a smart contract on another rollup in the same transaction**, just as if you were calling contracts on Ethereum mainnet or a single chain.

### Why Now?
- **ZK Proving Maturity**: Recent evolution of zero-knowledge proving makes block-scale proofs feasible
- **Real-time Proving**: Can generate validity proofs (~7 seconds for Ethereum-scale blocks with 12 GPUs) fast enough to accompany block construction
- This wasn't practically possible before; asynchronous was the only natural approach

---

## How EEZ Works

### Key Components

**1. Execution Tables**
- A sequence of actions describing cross-chain interactions with their state transitions
- Each entry contains: **Action** (CALL or RESULT), L2 state transitions, **nextAction**
- Defines what happens on each chain atomically

**2. Proxy Contracts**
- Deployed on each chain representing remote contracts
- When a contract calls a proxy, it triggers cross-chain execution table processing
- From the user/developer perspective: calling a proxy = calling the remote contract directly

**3. Single Proof Across Chains**
- All participating rollups' state transitions are bundled into **one atomic validity proof**
- Ethereum verifies this single proof on L1
- Proof can be valid for 2, 3, or any number of chains simultaneously
- Creates atomic guarantees: all succeed or all revert

### Architecture Flexibility: Proof System Agnosticism

**Critical feature:** EEZ is **NOT tied to a single proof system**

- Each rollup freely chooses which proof systems it trusts (ZK, SP1, multi-sec, T-based, etc.)
- Rollups can trust **one or multiple** proof systems
- Different rollups can use entirely different proof systems and still interoperate

**How:** When chains with different proof systems interact:
- Chain A proves: "here's what happened on my side"
- Chain B assumes: "these calls came from Chain A"
- Both sides exchange "prove/assume" records via a hash passed to both proof systems
- Both proof systems independently verify the interaction is correct
- **Result:** Base doesn't need to understand Arbitrum's execution model, and vice versa

---

## The Composability Example: Train-Hotel Problem

**Scenario:** User buys a plane ticket on Base and books a hotel on Optimism atomically

**Smart Contract Code Equivalent:**
```solidity
function buyTicketAndHotel() {
  call1 = airlineContract.buyTicket()
  call2 = hotelContract.bookRoom()
  
  if (call1.success && call2.success) {
    // Both succeed, user gets both
  } else {
    // Either fails, everything reverts
  }
}
```

**With EEZ:**
- User submits one transaction
- Execution table bundles: [Base ticket call] → [Optimism hotel call] → [results verified]
- Single proof covers both state transitions
- From user perspective: identical to L1 composability, but contracts are on different chains
- **Key difference from L1:** Each chain can have its own rules, precompiles, and fork independently while maintaining atomic composability

---

## Critical Technical Trade-offs & Constraints

### 1. Block Times: Atomicity vs. Speed

**Trade-off:** Synchronous blocks are inherently coupled to L1 block time (~12 seconds)

**Options:**
- **Pessimistic approach**: Lock the L2 while waiting for L1 finality
- **Optimistic approach**: Allow reorgs if L1 reorgs (accept the risk)
- **Hybrid approach**: Partially lock certain pieces of state (experimental, needs exploration)

**Workaround (not ideal):**
- Use a centralized sequencer to build 3-second blocks
- Bundle multiple EEZ blocks into one L1 block
- Trades decentralization for speed within EEZ, but still atomic

**Future optimization:** Preconfirmations on Ethereum could accelerate this

### 2. Block Builder Concentration & MEV

**Who builds these blocks?**
- **Composers** aggregate cross-chain transactions from different rollups into atomic bundles
- **Builders** create actual blocks from composer bundles
- **Validators** (final authority) decide which composer/builder to trust

**Risk:** MEV concentrates around builders/composers who see cross-chain transactions first

**Mitigation:**
- Validators have final say (not builders or composers)
- Validators control decentralization through builder selection
- This is similar to current Ethereum builder concentration issue
- Long-term: decentralized prover markets could commoditize composer/builder roles

**Not a new problem:** Same economics as Ethereum L1 block building

### 3. Chain Sovereignty & Proof System Independence

**L2 Sovereignty is maintained:**
- Each L2 keeps its own rules, state transition function, upgradability
- Proof system integration is **voluntary**—no L2 is forced to trust any proof system
- L2s can sync independently without watching other chains
- RPC servers only need to track their own chain

**Trust Model:**
- Base decides: "I trust Proof System X and Proof System Y"
- Arbitrum decides: "I trust Proof System Z"
- When they interact, both proof systems verify the cross-chain calls
- **Each contract decides:** "Do I trust Base? Do I trust Arbitrum?" independently
- Similar to: trusting a specific smart contract—if it's malicious, good luck

---

## Security Model & Attack Scenarios

### What happens if one proof system breaks?

**Scenario:** Base's proof system is compromised; attacker creates fake state transitions

**Attacker can:** Make fake outbox calls (e.g., "Vitalik sends his USDC to Justin Sun")

**Arbitrum's protection:**
- Arbitrum receives these fake calls in its inbox
- Arbitrum's proof system is still working (not broken)
- Users **only using Arbitrum** are **completely unaffected**—no stolen funds
- Arbitrum's own transaction processing is unimpacted

**Arbitrum contracts calling Base:**
- They see the fake calls returning
- **Application responsibility:** If your smart contract is secure, it ignores malicious responses
- Similar to calling a malicious contract on Ethereum—it's the contract's job to validate

**Key insight:** Atomicity is guaranteed within a proof system's domain, but applications must defend against calls from compromised external chains

---

## Roadmap & Implementation Plan

### Near-term (Next Month)
1. **Smart Contract Release**: Draft of EEZ smart contracts (smaller, easier without proving system logic)
2. **Mainnet Deployment**: Run experimental/non-production version on Ethereum mainnet
3. **Testing Infrastructure**: Bundle testing with composer dynamics

### Short-term (DappCon Berlin, next month)
1. Workshops explaining EEZ mechanics in detail
2. Testnet with 2-3 chains running:
   - One new chain
   - Gnosis Chain (or a copy for testing)
   - Possibly other chains joining
3. Stabilize infrastructure, stress-test

### Medium-term
1. **Proving System Development**: Heavy lift—needs extensive work
2. **Proof of Concepts**: Already have working POCs, need cleanup and production readiness
3. **Community Onboarding**: Open infrastructure to community for ideas/feedback

### Not Hard Forking L1
- Entire mechanism works through smart contracts
- No Ethereum protocol changes required
- Orthogonal to preconfirmation discussions
- Compatible with account abstraction standards (EIP-7702, EIP-4337)

---

## Use Cases & Applications

### High Excitement (Jordi's perspective)

**1. Extending Ethereum with Specialized Chains**
- Applications wanting custom precompiles, ZK optimizations, elliptic curves
- Currently expensive or infeasible on L1
- EEZ allows: build your own L2, prove your extensions work, interact atomically with Ethereum
- **Application:** Voting systems (cryptographically expensive), identity systems, specialized compute

**2. High-Volume Atomic Operations**
- **Plane + Hotel example**: atomic multi-chain booking
- **DeFi:** Move liquidity across chains, take flash loans, convert, return atomically
- **Insurance:** Buy ticket, contract insurance, and book hotel in one transaction
- No partial failures, no slippage from sequential async calls

**3. Complex Cross-Chain Workflows**
- Any operation that's currently broken into async steps can become atomic
- Examples: Insurance, voting, multi-asset swaps, micro-payments

### Why Not Just Use One L2?
- Different L2s have different economics, community, and sovereignty
- Specialized L2s can focus on specific use cases (voting L2, identity L2, general purpose L2)
- Atomicity enables ecosystem composition without sacrificing specialization

---

## Chain Compatibility & Flexibility

### What chains can join EEZ?

**Inclusive design:**
- **EVM chains** (Base, Optimism, Arbitrum, Gnosis)
- **Non-EVM chains** (Solana-style, CosmWasm with adaptation)
- **Even Bitcoin** (if you define how Bitcoin calls and receives calls)

**Only requirement:** Ether accountability
- System must track each chain's ETH holdings
- Prevents calls exceeding available balance

**Protocol minimalism:**
- No hard fork required
- Minimal protocol agreement: inboxes/outboxes standard
- L2s only need to define: how to process incoming calls, how to initiate outgoing calls
- State transition function remains fully sovereign

**Example:**
- Arbitrum could kick out EVM, go Stylus-only
- Base continues with EVM
- They still interoperate
- Neither needs to understand the other's execution model

---

## Known Gaps & Open Questions

### 1. Versioning & Breaking Changes
**Not addressed in transcript:** What happens if EEZ protocol evolves and breaks existing contracts?
- Potential solution: Versioned proxy contracts
- Gradual migration paths?
- **Action item for speakers:** Clarify breaking change strategy

### 2. Stale Execution Prevention
**From ethresear.ch post:** Pre-loaded execution tables that don't get consumed persist in storage
- Cleanup mechanism?
- Cost implications?
- Not detailed in transcript

### 3. Prover Decentralization Timeline
- Currently: 12 GPUs, expensive (builder duopoly)
- Aspiration: Decentralized prover markets
- **Reality:** Early adoption favors big builders; decentralization is aspirational, not guaranteed

### 4. L1 Finality Coupling
- How tight is the coupling to L1 block times?
- Preconfirmations could help, but details TBD

---

## Key Takeaways for Developers

1. **Atomicity is the feature**: All-or-nothing across chains is new and powerful
2. **Signature looks the same**: Smart contract code is identical to L1 composability
3. **Sovereignty is preserved**: L2s don't lose autonomy; proof system independence is real
4. **Trust is granular**: You trust specific proof systems, specific chains, specific contracts—not a monolithic bet
5. **Early adopters face risks**: Prover centralization, unproven proving systems, operational complexity
6. **Block time trade-off**: True synchronous composability means L1-coupled block times (initial constraint)

---

## Comparison Matrix

| Aspect | Asynchronous Bridges | EEZ | L1-only |
|--------|---------------------|-----|---------|
| **Atomicity** | Partial (can fail mid-way) | Full (all-or-nothing) | Full |
| **Latency** | Minutes/hours | ~12 seconds (L1 slot) | ~12 seconds |
| **Composability** | Broken | Intact | Intact |
| **Developer UX** | Complex (handle failures) | Simple (single call) | Simple |
| **Chain sovereignty** | Maintained | Maintained | N/A |
| **Operational complexity** | Low | High (proving, composers) | N/A |
| **Proof system requirement** | None (signatures) | Yes (ZK or other) | None |

---

## Next Steps for the EEZ Ecosystem

1. **Smart contracts go live** (next month): Experimental mainnet deployment
2. **DappCon workshops**: Education and testing with real chains
3. **Proving system hardening**: Substantial work remaining
4. **Community feedback loop**: Open infrastructure to gather ideas
5. **Gradual chain onboarding**: 2-3 chains initially, scale as proven

---

## Questions for Future Sessions

1. How will breaking protocol changes be handled without hard forks?
2. Timeline for decentralizing prover/composer role?
3. Exact gas cost breakdown vs. asynchronous alternatives?
4. Preconfirmation integration timeline?
5. Which chains are actively integrating?
6. Production-readiness criteria before mainnet launch?
7. Governance model for EEZ protocol evolution?
