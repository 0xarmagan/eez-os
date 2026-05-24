# EEZ Network Effect Dashboard — Specification

**Status:** Draft — Iteration #1  
**Owner:** Armagan  
**Last updated:** 2026-05-05  
**Context:** Devnet phase; May 13 community call (Phase 1 credibility narrative)

---

## Problem Statement

EEZ's value proposition is **synchronous composability** — atomic cross-chain execution that enables better liquidity efficiency and user experience. But narrative claims ("EEZ unlocks composability") need proof.

**What the dashboard does:** Demonstrates that as more rollups join EEZ, liquidity works harder, users get better execution, and capital becomes more efficient. Proves the primitive works.

**Stage:** Devnet proof-of-concept → May 13 narrative (Phase 1 credibility) → Testnet validation → Mainnet scaling

---

## Network Effects Definition for EEZ

A network effect exists when the value of EEZ increases as more rollups and users participate.

**Mechanism:**
1. More rollups adopt EEZ → more composable liquidity pools
2. More liquidity depth → better execution price (lower slippage)
3. Better execution → more users discover atomic transactions
4. More users → more capital locked → higher composability density

**Economic signal:** Same dollar amount of liquidity supports more transaction volume with less slippage. Capital efficiency increases.

---

## Core Metrics (Three Categories)

### 1. Adoption Velocity

Measures: "Is EEZ growing as a network?"

| Metric | Definition | Devnet target (May 13) | Success signal |
|--------|-----------|----------------------|-----------------|
| **Rollup count** | Number of EEZ-native rollups live | 2 (Gnosis + 1 independent) | Independent non-Gnosis rollup committed |
| **Active users** | Distinct addresses executing ≥1 atomic transaction | 500–1,000 | Week-over-week growth ≥10% |
| **Total value locked** | TVL across all EEZ rollups | $2–5M | Positive trajectory |
| **Partner pipeline** | Named rollups signed/in-progress for EEZ integration | 3–5 | At least 1 non-Gnosis in active testing |

**Why it matters:** Shows EEZ is attracting real participants, not just Gnosis building for itself.

---

### 2. Liquidity Efficiency Gains

Measures: "Does EEZ make liquidity work better?"

| Metric | Definition | How to measure | Devnet target |
|--------|-----------|-----------------|----------------|
| **Capital efficiency ratio** | Capital required in EEZ pools ÷ capital required in non-EEZ pools (for same liquidity depth) | Compare pool models: EEZ atomic pools vs. bridge+swap pools | 0.65–0.75 (EEZ needs 25–35% less capital) |
| **Price improvement** | Slippage on atomic swaps via EEZ vs. non-EEZ (bridge+swap) | Simulate 10 representative swaps; compare execution prices | 0.3–0.5% better slippage |
| **Composability density** | Atomic transactions per user per day | Sum(atomic txs) ÷ sum(active users) per day | 1.2–1.5 txs/user/day (shows reuse) |
| **Liquidity velocity** | TVL turnover rate (transaction volume ÷ TVL) | Monthly on-chain data | 2–3x per month (shows active use) |

**Why it matters:** The economic proof. Shows EEZ actually enables better execution, not just technical capability.

---

### 3. User Growth & Behavior

Measures: "Are users adopting EEZ? Are they returning?"

| Metric | Definition | Devnet target | Indicates |
|--------|-----------|----------------|-----------|
| **New users/week** | Unique addresses executing first atomic transaction | 50–100/week | Adoption momentum |
| **Repeat user rate** | % of users executing 2+ atomic transactions | 60–70% | Stickiness (not 1-time testers) |
| **Cohort retention** | % of users active 2+ weeks after first transaction | 40–50% | Product-market fit signals |
| **Use case distribution** | % of transactions by category: arbitrage, vault, liquidation, yield, other | Arb 40%, Vault 25%, Liq 20%, other 15% | Diversified use cases (not 1 driver) |

**Why it matters:** Shows EEZ is useful for real users, not just testnet noise.

---

## Dashboard Components

### Component 1: Adoption Overview (Real-time)
```
┌─────────────────────────────────────────┐
│ EEZ Adoption Snapshot                   │
├─────────────────────────────────────────┤
│ 🔗 Rollups Live: 2 (Gnosis, [TBD])      │
│ 👥 Active Users: 847                    │
│ 💰 Total TVL: $3.2M                     │
│ 📈 New Users (7d): +142                 │
│ 🤝 Partners in Pipeline: 4              │
└─────────────────────────────────────────┘
```

**Data sources:** Gnosis Chain RPC, EEZ smart contract events, internal partner tracker

---

### Component 2: Liquidity Efficiency (Simulated + Real)
```
┌──────────────────────────────────────────────────────┐
│ Capital Efficiency: EEZ vs Non-EEZ                    │
├──────────────────────────────────────────────────────┤
│                                                       │
│ Non-EEZ (Bridge+Swap):     $1.00M liquidity needed  │
│ EEZ (Atomic Pools):        $0.72M liquidity needed  │
│                            ▓▓▓▓▓░░░░                │
│                                                       │
│ Savings: 28% less capital for same depth             │
│                                                       │
│ Price Improvement on $100k swap:                     │
│ Non-EEZ: -0.65% slippage                            │
│ EEZ:     -0.18% slippage                            │
│ Better: +0.47% (472 bps improvement)                │
└──────────────────────────────────────────────────────┘
```

**Data sources:** Devnet pool simulations, transaction logs, pricing models

---

### Component 3: User Behavior (Cohort Analysis)
```
┌──────────────────────────────────────────────────────┐
│ User Cohorts & Retention                             │
├──────────────────────────────────────────────────────┤
│ Week 1: 150 new users                                │
│ ├─ Week 2: 102 active (68% retention)               │
│ │  ├─ Week 3: 71 active (70% of week 2)             │
│ │  └─ Avg txs/user/day: 1.3                         │
│                                                       │
│ Use Case Distribution (last 7d):                     │
│ Arbitrage:    42% (358 txs)                         │
│ Vault ops:    28% (238 txs)                         │
│ Liquidation:  21% (179 txs)                         │
│ Yield:        6% (51 txs)                           │
│ Other:        3% (26 txs)                           │
└──────────────────────────────────────────────────────┘
```

**Data sources:** On-chain transaction logs, user wallet tracking, transaction tagging

---

## Data Sources & Freshness

| Metric | Source | Refresh | Caveat |
|--------|--------|---------|--------|
| Rollup count, TVL, active users | Gnosis Chain RPC, EEZ smart contract | Real-time | Devnet-only; small user base |
| Price improvement, slippage | Devnet transaction logs | Daily | Simulated liquidity; not mainnet conditions |
| Capital efficiency ratio | Pool models + simulation | Weekly | Theoretical; needs real pool testing |
| Cohort retention, use cases | On-chain transaction analysis | Daily | Depends on wallet labeling accuracy |
| Partner pipeline | Internal tracker | Weekly manual | Requires stakeholder update |

---

## May 13 Narrative Framing

**What this dashboard proves:** "EEZ synchronous composability works at devnet scale. Same liquidity depth requires 25–35% less capital. Atomic swaps outperform bridge+swap by 0.3–0.5%. Users return for real use cases (arbitrage, vaults, liquidation), not novelty."

**What it does NOT claim yet:** Mainnet-scale adoption, economic viability, governance finality, independent rollup proof-of-concept.

**May 13 messaging:** 
- "Devnet metrics validate the EEZ primitive"
- "Roadmap: Testnet (Q2) with independent rollup; Mainnet (Q3) with multi-rollup proof"
- "Gnosis Chain is first adopter; 4 partners in active pipeline"

---

## Build Sequence (Proposed)

| Phase | Deliverable | Timeline | Effort |
|-------|-------------|----------|--------|
| **Phase 1** | Metrics spec + data collection setup | Week 1 (by May 6) | 2–3 days |
| **Phase 2** | Dashboard skeleton (static mock) | Week 2 (by May 13) | 3–5 days |
| **Phase 3** | Live data feeds (RPC, smart contracts) | Post-May-13 | 5–7 days |
| **Phase 4** | Refinement + narrative alignment | Testnet prep | 2–3 days |

---

## Open Questions

- [ ] **Data accuracy:** How do we tag user intent (arbitrage vs. vault vs. liquidation) from on-chain data? Label transactions manually or infer from contract interaction?
- [ ] **Simulation vs. reality:** Should May 13 show simulated pool models (theoretical) or wait for real on-chain pools? Trade-off: credibility vs. completeness.
- [ ] **Independent rollup data:** Once a second rollup is live, do we aggregate metrics or show per-rollup breakdowns?
- [ ] **Audience:** Is this dashboard internal-only (Ben + team) or public (May 13 presentation)? Affects design (internal: raw data; public: narrative with annotations).
- [ ] **Update frequency:** Real-time (RPC polling) or daily batch? Cost vs. freshness trade-off.

---

## Notes for Iteration

**Next steps:**
1. Confirm May 13 audience (internal / investor / public)
2. Validate data sources (RPC access, contract ABIs, labeling approach)
3. Decide simulation vs. live data for May 13 baseline
4. Design dashboard UI (Dune? Grafana? Static HTML?)
5. Assign ownership for data pipeline

**Feedback welcome on:**
- Metric choice (are these the right signals?)
- Target numbers (are 2–5M TVL / 500–1k users realistic for devnet?)
- Narrative framing (does this support Phase 1 credibility story?)
- Technical feasibility (can we collect this data by May 6?)
