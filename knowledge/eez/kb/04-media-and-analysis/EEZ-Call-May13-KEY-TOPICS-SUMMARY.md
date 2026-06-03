# EEZ Community Call #1 (May 13, 2026) — Key Topics Summary

**Duration:** 51 minutes  
**Speakers:** Armagan (moderator), Friederike Ernst, Jordi Baylina, Eduardo (Edu), Jesus, Philip, Yesus (ZisK team)  
**Format:** Technical deep-dive + Economic analysis

---

## 🎯 THE CORE PROBLEM & SOLUTION

### Problem: Fragmentation (Not Scaling)

**Key Insight from Friederike:**
- Ethereum HAD a scaling problem 2–3 years ago (transaction costs >$10)
- Rollups SOLVED the scaling problem
- But they CREATED a new problem: **fragmentation**

**The Fragmentation Issue:**
- Ethereum used to be ONE coherent economic zone
- Rollups divided it into 100+ isolated L2 chains
- Each L2 is its own island → separate liquidity, separate applications, separate ecosystems
- Users can't seamlessly use protocols across L2s in a single transaction
- Result: "Mini Ethereums" that each need to replicate everything

**The LEGO Analogy:**
- Before L2s: One pile of LEGOs. Anyone could build on anyone else's blocks.
- After L2s: Multiple separate LEGO piles. Can't mix pieces between piles.
- EEZ restores: One coherent LEGO pile (with multiple rollups inside it).

---

### Solution: Synchronous Composability (via Real-Time Proving)

**How it works:**
1. Smart contracts on EEZ rollups can call Ethereum L1 contracts
2. And L1 contracts can call other EEZ rollups
3. **All in the same transaction** (synchronously)
4. No bridge risk, no time delay, no asynchronous complexity

**Why this is possible NOW:**
- **Real-time proving** = new ZK technology (Zisk contribution)
- Can prove the state of one chain to another chain in ~12 seconds (block time)
- Previously: proving was slow/impossible
- Now: state proofs happen in real-time, enabling atomic cross-L2 execution

**Result:** Users don't see fragmentation anymore. It's just Ethereum.

---

## 🏗️ TECHNICAL ARCHITECTURE

### 1. Smart Contract Core (`Jesus Segment`)

**Central Component:** EEZ Settlement Contract (on L1)
- Manages all rollup interactions
- Settles multiple rollups atomically in ONE transaction

**Key Properties:**

#### A. Permissionless Rollup Registration
- Anyone can register a new rollup
- Each rollup has its own contract with custom rules
- No central authority gatekeeping

#### B. Pluggable Proof Systems
- Each rollup chooses its own proving system(s)
- Can mix multiple provers: ZK (SP1, openBM), signatures, multi-sig
- Example: "I accept SP1 + openBM + 2-of-3 multisig"
- Completely agnostic to prover type
- Allows cost sharing (5 rollups using same prover = shared proof)

#### C. Atomic Settlement
- Multiple rollups settle together in ONE transaction
- Prevents double-spending across rollups
- All interactions between rollups must be verified before settlement

#### D. Execution Entries (State Management)
- When a transaction happens that involves L1 ↔ L2 interactions
- System creates "execution entries"
- These define expected state changes BEFORE the proof is submitted
- Enables synchronous composability

---

### 2. Synchronous Composability Examples

#### Example 1: Simple Cross-L2 Transfer
```
User on L1 wants to send ether to a user on Rollup A

PROCESS:
1. User calls proxy contract on L1
2. L1 creates execution entry: "Expect user B to receive 1 ETH on Rollup A"
3. EEZ verifies this
4. If correct, state is updated atomically
5. User gets confirmation in same transaction
```

#### Example 2: Contract Call with Return Value
```
User on L1 calls Uniswap contract on Rollup B

PROCESS:
1. User calls Uniswap proxy on L1
2. L1 executes: "Call Uniswap on Rollup B with this call data"
3. EEZ verifies + executes
4. Uniswap returns swap result
5. Result returned to user in same transaction (UX = native L1 experience)
```

#### Example 3: Nested Calls (L1 → L2 → L1 → L2)
```
L1 contract calls Rollup A contract
Rollup A contract calls back to L1
L1 calls Rollup B contract
Rollup B returns value

RESULT: All in one transaction, arbitrarily deep nesting
```

---

### 3. Multi-Proving System Architecture (`Jordi Segment`)

**Key Innovation:** Multiple provers work in parallel, one transaction

**How it works:**

#### State Transition Table
- Every transaction creates "chunks" of execution
- Example: User calls L2 contract which calls L1 contract
- = 3 chunks: (L1 call → L2 execution → L1 return)
- With 10 transactions calling each other = thousands of chunks

#### Proving Process
1. **From rollup perspective:** Each chunk is a state transition function
   - Input: previous state
   - Execution: transaction logic
   - Output: new state

2. **From L1 perspective:** Create a "derivative table"
   - Hash all the interactions together
   - Prove that table transformation is correct

3. **Multiple provers verify different chunks:**
   - SP1 proves chunks A, B, C
   - openBM proves chunks D, E
   - Each prover verifies only the chunks relevant to them

4. **Atomic verification:**
   - Smart contract checks: "Do all proofs verify their chunks correctly?"
   - If YES → all rollups atomically update
   - If ANY proof fails → entire batch fails

#### Safety Model
- If one prover is malicious: Only rollups using that prover are affected
- If a rollup uses "2 out of 3" provers: Safe even if 1 is malicious
- System can have different rollups with different requirements (no single point of failure)

---

### 4. Cross-Chain Proxies (UX Innovation)

**Problem:** How do users interact with L2s without complexity?

**Solution: Permissionless proxy creation**
```
For any L2 address, anyone can create a proxy on L1

Example:
- Uniswap is deployed on Rollup A at address 0x123
- Anyone creates proxy on L1 at some address
- Users call the proxy like any L1 contract
- Proxy forwards call to Rollup A
- Result returned in same transaction
```

**Result:** Users don't need to know which rollup is where. Just interact like native L1.

---

## 📊 ROADMAP & TIMELINES

### Current Status (May 2026)
- ✅ Smart contract design: ~80% done (finalizing now)
- ✅ POC completed: Proved concept works
- 🔄 Moving to production-ready version

### Upcoming Milestones

**Mid-July 2026:**
- Smart contract audit completion
- Final version ready with community feedback incorporated
- Expect to start audit as soon as possible

**Mid-June 2026:**
- First rollup goes live (Rollup Zero)
- Limited functionality: L2→L1 calls, L1→L2 calls, proving system only
- Reduced feature set (not full composability yet)

**Summer 2026 (August):**
- Rollup One: Full-featured rollup
- Complete synchronous composability
- Shows how any rollup can use atomic compatibility
- Reference implementation for the network

### Parallel Work Streams
1. **Smart contract hardening** (audit-ready)
2. **Composer development** (data provisioning layer)
3. **Multi-prover system** (verifier implementation)
4. **Base rollups** (Rollup Zero, Rollup One)

---

## 💰 ECONOMIC IMPLICATIONS

### Why Not Just Stage 2 Rollups?

**Stage 2 Rollups (Arbitrum, Optimism, etc.):**
- ✅ Same security as Ethereum (ZK-proven)
- ❌ Block times locked to 12s (can't go faster)
- ❌ Chicken-and-egg problem: Low adoption, high costs
- ❌ Must replicate everything: Liquidity, DEXs, lending, oracles, stables
- Result: "Mini Ethereums" = fragmentation

**EEZ Different:**
- ✅ Same security guarantees
- ✅ **Network effects** through synchronous composability
- ✅ Don't replicate existing protocols (use L1/other L2 versions directly)
- ✅ Permissionless: Any rollup (or non-rollup) can join
- ✅ Complements L2 innovation (privacy, faster blocks, custom logic)

### The Key Insight: Composition > Replication

**Current L2 Problem:**
- Developers on Rollup A need their own: Uniswap, Aave, stablecoins, oracles
- Liquidity fragmented across multiple Uniswaps
- Result: Terrible UX, dead markets, high costs

**EEZ Solution:**
- Developers on Rollup A can call L1 Uniswap atomically
- Or call Aave's Rollup B instance directly in same transaction
- Liquidity is unified (one Uniswap + one Aave = single market)
- Developers focus on their innovation, not replication

**Economics:**
- Rollup A focuses on: "What unique value do we add?"
  - Privacy? → Privacy rollup
  - Speed? → High-frequency rollup  
  - Custom logic? → Application-specific rollup
  - Consortium? → Permissioned validator set

- NOT "How do we replicate Uniswap + Aave + Curve + Lido + ...?"

---

## 🔑 KEY TECHNICAL INNOVATIONS

### 1. Real-Time Proving (ZisK)
- Proves state of one chain to another in ~12 seconds
- Enables synchronous composability (was impossible before)
- Core enabler of entire EEZ architecture

### 2. Flexible Proof Requirements (Per-Rollup)
- Not "1 prover type for all"
- Each rollup: "I accept these combinations"
- Enables heterogeneous trust models in one system

### 3. Atomic Batch Settlement
- All interacting rollups settle in ONE Ethereum transaction
- No time windows where partial settlement is possible
- Prevents double-spending, maintains invariants

### 4. Execution Entries
- Pre-verify expected state changes
- Enable synchronous L1 ↔ L2 calls
- Not "optimistic" (no fraud proofs, no delays)
- Deterministic (knows result before submitting to chain)

### 5. Permissionless Everything
- Anyone registers a rollup
- Anyone creates a proxy for cross-chain calls
- Anyone defines a proving system
- No central control → maximally credible neutrality

---

## ❓ CRITICAL QUESTIONS ADDRESSED

### Q: "Why is EEZ different from Stage 2 rollups?"
**A:** Stage 2 rollups can't do synchronous composability (no real-time proving). EEZ can. This solves network effects problem.

### Q: "Won't this fragment further?"
**A:** No, because fragments are composable. One Uniswap (on any rollup) + synchronous access = one market, not 100.

### Q: "Isn't this slow?"
**A:** Proofs happen in ~block time (12s). Users see atomic execution in same transaction = no perceived latency.

### Q: "What if a prover is malicious?"
**A:** Rollups can require 2-of-3 provers. 1 malicious ≠ rollup is broken.

### Q: "Do all rollups have to use EEZ?"
**A:** No. Permissionless. Join or don't. The architecture works for partial adoption.

---

## 📈 NARRATIVE & POSITIONING

### Problem Statement
Ethereum fragmented into 100+ L2s. Users can't use protocols across chains atomically. Network effects broken.

### Solution
Synchronous composability via real-time proving. All chains settle atomically in Ethereum. Networks effects restored.

### Differentiation
- **vs. Stage 2 rollups:** More permissive (any rollup type), network effects, no replication needed
- **vs. Bridging:** Atomic, no bridge risk, instant settlement
- **vs. Single L2:** Heterogeneous innovation (each rollup is different), but composable

### The Vision
One Ethereum. Multiple rollups. Different trade-offs. Same economy. Users don't see fragmentation.

---

## 🎯 KEY TAKEAWAYS

| Topic | Key Point |
|-------|-----------|
| **Problem** | L2 fragmentation (100+ isolated chains), not scaling |
| **Solution** | Synchronous composability (atomic cross-L2 calls) |
| **Technology** | Real-time proving + multi-prover system + execution entries |
| **UX** | Users interact like native Ethereum (no bridging, no delays) |
| **Economics** | Network effects restored, no replication needed, permissionless |
| **Timeline** | Rollup Zero (June), Rollup One (August), audits by July |
| **Differentiation** | Permissionless, heterogeneous, composable, credibly neutral |

---

## 📚 RELATED DOCUMENTS

- `knowledge/eez/03-technical-spec.md` — Full technical specification
- `knowledge/eez/01-comms-framework.md` — Messaging pillars
- `outputs/drafts/technical-deep-dives/Zisk-Real-Time-Proving-Summary.md` — Proving system details
- `EEZ-Call-1-SEGMENT-SCRIPTS.md` — Full script with Q&A framework

---

*Summary created from May 13, 2026 Community Call transcript*  
*45-minute technical + economic presentation to EEZ Alliance & community*
