# Bankless — "Will The Ethereum Economic Zone (EEZ) Rebuild $ETH Dominance?"

| Field | Value |
|-------|-------|
| Channel | Bankless |
| Date | April 9, 2026 |
| Duration | ~61 min |
| Guests | Martin Köppelmann (Gnosis Co-founder), Friederike Ernst (Gnosis Co-founder) |
| URL | https://www.youtube.com/watch?v=rkRiLs8wl28 |
| Views | 3,928 |

---

## Summary

Bankless hosted Martin and Friederike following Friederike's EthCC presentation alongside Jordi Baylina (Zisk). The episode covers what EEZ is, why real-time proving makes it possible now, the Alliance structure, and a summer launch timeline. The host calls it "the main quest line of Ethereum." Most prominent crypto-native media appearance for EEZ to date.

---

## Key Quotes

| Speaker | Quote | Context |
|---------|-------|---------|
| Friederike | "Everything stays where it is. It's just instantly accessible from wherever you are in the EEZ." | Defining what EEZ is NOT — not a migration, not a new chain |
| Friederike | "The Ethereum Economic Zone is the domain around Ethereum where transactions feel like you're on the same network." | Non-technical definition |
| Martin | "There will just be a translation of how you convert this address to a proxy address — a representation on the chain you are at. And then the promise is you can just call this proxy representation as if it would live on your chain." | Proxy architecture for cross-chain composability |
| Martin | "We have 10 years of smart contracts that are available and we want all of this to work with those contracts that are already out there." | Backwards compatibility |
| Martin | "Even if dapps do not adapt to this at all [...] it would still make the state of affairs immeasurably better than it is today." | Floor value of EEZ without dApp changes |
| Martin | "The generalist chain — I see it on the way down." | Prediction on L2 landscape post-EEZ |
| Martin | "That was always one of my criticisms of the previous L2 roadmap — it wouldn't really create network effects. And that equation changes here." | EEZ vs. current L2 fragmentation |
| Friederike | "It's very much a community project. We initiated this as Gnosis [...] but it's for everyone." | Alliance and funding framing |
| Martin | "That's in the summer. It's not a multi-year project. We're well underway and it's coming very soon." | Launch timeline |
| Host | "This feels like the main quest line of Ethereum." | Editorial endorsement |

---

## EEZ Definition

Two framings offered in the episode:

**Non-technical (Friederike):** The domain around Ethereum where transactions feel like you're on the same network.

**Economic (Martin):** Shared liquidity across chains — one price per token, one market, because liquidity is instantly accessible from any EEZ chain without moving.

Host emphasis: EEZ is not a new rollup standard. Not "come to our chain." Everything stays where it is and becomes composable.

---

## Technical Architecture

Cross-chain calls work via a proxy model. An address on chain A maps to a proxy address on chain B. Contracts on chain B call the proxy as if it lives locally — the cross-chain execution is abstracted away.

Backwards compatibility is a design constraint, not an afterthought. Existing contracts on EEZ chains work without modification. dApps that do adapt (e.g. Aave reading global collateral across all EEZ chains) unlock more capability, but the baseline improvement is significant even with zero dApp changes.

---

## Real-Time Proving

Martin presented a similar vision at Devcon Bangkok (~1.5 years prior) under the label "native rollups." The difference: back then you could affect L1 and L2 state in one transaction but couldn't read the result from the L2 in the same transaction. Real-time ZK proving closes that gap — full synchronous composability is now achievable.

Both guests confirmed this is the specific technology unlock that made EEZ worth building and funding now.

---

## EEZ Alliance

**Definition (Friederike):** An informal group of projects committed to supporting EEZ from day one. Not a legal entity. The underlying software is free and open source.

**Named members:** Aave, Spark, Safe, Cow, Monerium, XStocks, Centrifuge, Fiber

**Two participation tracks:**
- Block builders — execute to EEZ standard as early as possible
- dApps — understand and surface cross-chain state in their product logic

---

## Funding

Confirmed contributors: Gnosis, Ethereum Foundation, Octant. Titan considering.

---

## Reference Chain

A new rollup is being built as the first EEZ-native chain: no sequencers, only Ethereum validators — designed to be maximally aligned with Ethereum ("native rollup" model). Gnosis Chain will join in parallel via a settlement/bridge mechanism upgrade.

First chain to enable synchronous composable transactions = EEZ launch. Targeted for summer 2026.

---

## L2 Adoption

Large generalist L2s face the most friction: synchronising with Ethereum reorgs is manageable for simple payment flows but complex for chains where in-flight liquidity state changes are common. Friederike expects them to join eventually, not first.

Martin's structural view: once composability is cheap and default, the "table stakes" advantage of generalist L2s disappears. App chains and niche chains become more competitive. The era of the generalist chain is ending.

---

## Network Effects

Current L2 landscape has no positive-sum dynamic — a new L2 launching does not make other L2s more valuable. EEZ inverts this: each new chain that joins expands the composable surface for all other chains. More chains = more attractive to join.

---

## Builder Implications

Builders in the EEZ will have access to state across all member chains by default — the same paradigm as Ethereum L1 pre-2020, before fragmentation. The host's worked example: deposit collateral on Ethereum L1, borrow against it on Aave Chain, receive funds on a third chain — one transaction.

---

## Use For

**Audience:** DeFi professionals, protocol teams, L2 operators, technical builders  
**Tone:** Substantive, technically credible, founder-led  
**Best angles:**
- "Everything stays where it is" — anti-migration framing for chains nervous about switching costs
- Summer timeline — concrete anchor for urgency
- Named Alliance members — social proof for other dApps considering joining
- Network effects framing — structural argument for why EEZ wins long-term
- "Main quest line" endorsement — third-party editorial credibility from Bankless

---

## Proposed Tweets

**Angle: definition**
```
"Everything stays where it is. It's just instantly accessible from wherever you are in the EEZ."

@friederike_ernst on @BanklessHQ explaining what EEZ actually is — not a migration, not a new chain. A composability layer.

Full episode: [url]
```

**Angle: timeline**
```
Martin Köppelmann on EEZ launch: "That's in the summer. It's not a multi-year project."

@BanklessHQ sat down with @koeppelmann and @friederike_ernst for the most in-depth EEZ conversation yet.

[url]
```

**Angle: network effects**
```
One of the most underrated points from the @BanklessHQ EEZ episode:

Current L2s have no positive-sum dynamic. A new L2 doesn't make other L2s more valuable.

EEZ flips this. Every chain that joins expands the composable surface for everyone else.

[url]
```

**Angle: backwards compatibility**
```
EEZ doesn't ask you to rebuild.

"We have 10 years of smart contracts. We want all of this to work with contracts that are already out there." — @koeppelmann

Existing contracts on EEZ chains work without modification. dApps that do adapt unlock more. Those that don't still benefit.

[url]
```

---

## Last Updated

May 26, 2026 — Full transcript review and template reformat.
