# EEZ Explained Simply
**Community Call #1 with Jordi Baylina (Zisk)**  
**Date:** May 2026

---

## What is EEZ?

Think of EEZ as a way for different blockchain rollups to work together **as if they were one chain**.

Right now, if you want to use two different Layer 2 chains (like Base and Optimism), they can't really talk to each other easily. EEZ fixes that.

**The dream:** You should be able to call a contract on Base and a contract on Optimism in the **same transaction**, just like you would if everything was on Ethereum.

---

## Why is This Hard?

For years, Jordi thought synchronous composability (everything happening at once) was impossible or too hard. Everyone assumed you needed asynchronous bridges (things happening slowly, one after another).

**What changed?** Zero-knowledge proofs got really fast.

Now we can prove an entire Ethereum block in about 7 seconds using 12 GPUs. This is fast enough to do real-time proof generation.

That speed made EEZ possible.

---

## How Does It Work? (Simple Version)

### Step 1: You make one transaction
```
User: "Buy plane ticket on Base AND book hotel on Optimism"
```

### Step 2: Both happen at the same time
- Plane contract on Base executes
- Hotel contract on Optimism executes
- Both are proven in a single proof

### Step 3: Everything settles on Ethereum
Ethereum sees one proof saying "both these things happened correctly on their respective chains"

### Step 4: Result
✅ Both succeed, or ❌ both fail. No in-between.

---

## The Real Example: Plane + Hotel

You want to:
1. Buy a plane ticket on Base
2. Book a hotel on Optimism
3. In one transaction

**Without EEZ (asynchronous):**
- Buy ticket on Base ✅
- Wait for confirmation
- Try to book hotel on Optimism
- Hotel booking fails because you're out of funds
- Stuck: you have a ticket but no hotel 😞

**With EEZ (synchronous):**
- Both happen together
- If either fails, both revert
- You get both or nothing
- Simple and safe 🎉

---

## How Do Different Chains Talk?

Each chain gets a "representative address" on Ethereum. 

When you call this address, it's like calling the real contract on that chain.

```
Your contract on Ethereum:
  → calls Base's representative
  → calls Optimism's representative
  → everything happens atomically
```

From a developer's perspective, it looks **exactly like calling contracts on L1**.

---

## Each Chain Can Use Different Proof Systems

This is important: **Base and Optimism don't have to agree on the same proof system**.

- Base could use one ZK system
- Optimism could use a different one
- They still work together

How? Both proof systems verify that the interaction between the chains is correct. They both check: "Did Base's call return what it says it returned?"

**Result:** Chains stay independent but composable.

---

## The Trade-off: Speed vs. Atomicity

With EEZ, block times are tied to Ethereum's block time (~12 seconds).

You can't have a super-fast 250ms block time and also have atomic composability across chains.

**Options:**
- Use a centralized sequencer to make faster blocks, but they still get bundled into Ethereum blocks
- Or keep everything slow but truly atomic

It's a choice, not a bug.

---

## Who Builds These Blocks?

This is where it gets tricky.

**Three players:**
1. **Composers** — combine cross-chain transactions into bundles
2. **Builders** — create actual blocks from these bundles
3. **Validators** — decide which builder/composer is trusted

The risk: builders see cross-chain transactions first, giving them MEV advantage.

**The honest answer:** This is the hardest question. It's similar to today's Ethereum builder concentration problem. Validators ultimately have control.

---

## What Happens If a Proof System Breaks?

**Scenario:** Base's proof system is compromised. An attacker fakes a transaction: "Vitalik is sending his USDC to me."

**What happens to Arbitrum?**
- Arbitrum receives this fake call
- But Arbitrum's own proof system still works fine
- If you're only using Arbitrum, you're completely safe

**What if your Arbitrum contract called Base?**
- Your contract receives the fake response
- **Your responsibility:** Check if the response makes sense
- It's like calling a malicious contract—if your code is secure, you ignore the bad data

**Bottom line:** Breaking one proof system doesn't cascade to others.

---

## Real Use Cases

### DeFi
Move funds across chains atomically:
- Take a flash loan on one chain
- Swap tokens on another
- Return the loan on the first
- All in one transaction

### Voting
Decentralized voting is cryptographically expensive on L1. With EEZ:
- Run a voting chain with millions of votes
- Secured by Ethereum
- Fast and cheap
- Atomic with the rest of DeFi

### Specialized Chains
Some applications want custom features (special precompiles, different rules). EEZ lets them:
- Run their own chain
- Add whatever they want
- Still be atomic with Ethereum and other chains

### Insurance + Booking
Buy plane ticket, insurance, and hotel booking—all or nothing.

---

## What Can Join EEZ?

**Not just EVM chains.**

- ✅ Base (EVM)
- ✅ Arbitrum (EVM, or Stylus)
- ✅ Gnosis Chain (EVM)
- ✅ Solana-style chains (with adaptation)
- ✅ Even Bitcoin (if you define how it calls and receives calls)

The only requirement: the chain must track its Ether holdings (to prevent overdrafts).

**Why so flexible?** The protocol is minimal. Chains only need to agree on how to handle incoming and outgoing calls.

---

## What's the Roadmap?

### Next Month
- Release draft smart contracts on Ethereum mainnet
- Mark as "experimental, not production-ready"
- Start testing with real dynamics

### DappCon (Berlin, next month)
- Run workshops explaining how this works
- Launch testnet with 2-3 chains
- Get feedback from community

### After That
- Harden the proving systems (lots of work)
- Clean up proof-of-concepts
- Open to more chains gradually

---

## Key Points to Remember

1. **It looks like Ethereum:** For developers, coding for EEZ is identical to coding for L1

2. **Chains stay sovereign:** Base doesn't have to trust Arbitrum's rules. Each chain picks which proof systems it trusts.

3. **Atomicity is the feature:** All-or-nothing execution is new and powerful for cross-chain apps

4. **No L1 changes needed:** Everything works through smart contracts. No Ethereum hard fork required.

5. **Early adoption has risks:** Prover centralization is real right now. It might decentralize later.

6. **Block times are coupled to L1:** You can't have 250ms blocks and atomic composability at the same time.

---

## The Bottom Line

EEZ solves a real problem: **making it easy for apps to exist across multiple chains while maintaining composability.**

Without it, you're building isolated apps on separate chains.

With it, you're building a single Ethereum ecosystem spread across many chains.

That's powerful.

---

## What Happens Next?

Developers should:
- Watch for smart contracts release next month
- Join DappCon workshops to learn the mechanics
- Start thinking about cross-chain applications
- Give feedback to the team

The team is putting this together now. They have working proof-of-concepts. Next is cleanup and launch.
