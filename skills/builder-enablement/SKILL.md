---
name: builder-enablement
description: Use when a builder, protocol, or developer wants to integrate with or build on EEZ — to qualify their use case, recommend the right deployment path, set honest expectations, route to resources, and draft follow-up.
---

# Builder Enablement Skill

First-contact skill for any project interested in EEZ. Qualifies fit, recommends native rollup vs async path, discloses constraints honestly, and hands off to the right resources. The use case qualification step is the highest-value part — get this right before recommending anything.

## Trigger

- "help [project] build on EEZ", "evaluate [project] for EEZ fit"
- "builder intake for [project]", "someone wants to integrate with EEZ"
- "what can [project] do with EEZ", "is EEZ right for [project]"
- "answer builder Q&A", "what path is right for [use case]"
- `/builder-intake [project]`

## Workflow

1. **Qualify** — 2–3 targeted questions or infer from context: what does the project do, what chain are they on, what cross-chain problem are they solving?
2. **Match use case** — map to one of the use case patterns below
3. **Recommend path** — native rollup vs async, with rationale
4. **Disclose constraints** — always: current testnet status, ~12s block time, proving requirements
5. **Route to resources** — specific KB files, technical contacts, DappCon Berlin workshops
6. **Draft follow-up** — short email or Telegram: path summary, next steps, links

---

## Use Case Patterns

### Lending Protocols (Aave, Morpho-style)

**Pain:** Liquidations are siloed per chain. Capital on Chain B can't liquidate a position on Chain A without a bridge.

**EEZ unlock:**
- Synchronous cross-chain liquidation — single atomic transaction reads position on Chain A, executes with capital from Chain B, same block
- Cross-chain flash loans — borrow on Chain A, use on Chain B, repay in same block, no bridge risk

**Path:** Native rollup. Async path is not suitable — liquidations need guaranteed execution, not eventual finality.

**Constraint:** ~12s block time (coupled to L1). Not sub-second. If they need faster liquidations, EEZ is not the answer today. Say so.

---

### DEXs and AMMs (Uniswap, Balancer-style)

**Pain:** Liquidity is siloed. A pool on Gnosis Chain can't atomically arbitrage against a pool on Arbitrum.

**EEZ unlock:**
- Cross-chain atomic arbitrage in one transaction
- Unified liquidity views across EEZ chains
- Cross-chain MEV — builders can compose trades across chains in one block

**Path:** Native rollup preferred. Async path works for lower-frequency cross-chain routing.

**Constraint:** Arbitrageurs need access to EEZ block builder/proposer infrastructure. Early-stage — not fully permissionless yet.

---

### Bridges and Asset Movement

**Pain:** Bridges introduce trust assumptions, delay, and capital lock-up.

**EEZ unlock:** Native cross-chain asset movement between EEZ chains without a bridge. If both chains are EEZ native rollups, moving assets is an atomic state transition, not a bridge operation.

**Path:** Native rollup on both source and destination chains.

**Constraint:** Both chains must be EEZ native rollups. If one chain is not, fallback to async path (intents-based relay). EEZ does not replace bridges for chains outside the EEZ zone — only between EEZ chains.

---

### Restaking and Yield Protocols

**Pain:** Yield opportunities are chain-specific. Rebalancing across chains is slow and expensive.

**EEZ unlock:** Cross-chain position management in one atomic transaction — read yield rates across chains, rebalance in the same block.

**Path:** Native rollup for full atomicity. Async path for directional rebalancing with looser timing requirements.

---

### Gaming and NFTs

**Pain:** Assets locked on one chain. Cross-chain use requires bridges with long finality times.

**EEZ unlock:** Cross-chain asset reads and state updates in one block. An NFT on Chain A can gate access to game state on Chain B atomically.

**Path:** Async path is often sufficient (lower value, less atomicity-critical). Native rollup for high-value in-game asset operations.

**Constraint:** Gaming needs sub-second UX for most real-time interactions. EEZ at ~12s is suitable for settlement and ownership verification, not real-time gameplay mechanics.

---

### Infrastructure (Sequencers, Provers, DA Layers)

**Pain:** Every rollup runs its own proving and sequencing infrastructure. High fixed cost, duplicated effort.

**EEZ unlock:** Shared proving infrastructure (ZisK), shared sequencing across EEZ chains. Reduces cost of running a rollup.

**Path:** This is the "deploy a chain on EEZ" path, not "use EEZ from an existing chain." Requires implementing the inbox/outbox interface.

**Constraint:** Real-time ZK proving currently requires approximately 12 GPUs. Not trivial to operate independently. Shared proving infrastructure is the value proposition here.

---

### Stablecoins and Cross-Chain Collateral

**Pain:** Stablecoin collateral is siloed. A collateral position on one chain can't back a mint on another without a bridge.

**EEZ unlock:** Atomic cross-chain collateral reads — mint on Chain B against collateral verified atomically on Chain A, same block.

**Path:** Native rollup for full atomicity guarantees.

---

## Path Decision

| Condition | Recommend |
|---|---|
| Needs same-block atomicity (lending, DEX, stablecoins) | Native rollup |
| Needs cross-chain reads with guaranteed execution | Native rollup |
| Wants to deploy a new chain / rollup | Native rollup (inbox/outbox interface) |
| Can accept eventual finality (gaming settlement, directional rebalancing) | Async path |
| Lower barrier, wants to experiment first | Async path |
| Needs sub-second finality | Neither — say so clearly |
| Chain is not EEZ-compatible and won't implement interface | Neither — flag honestly |

## Poor Fit — Say Clearly

If the project needs sub-second finality, ultra-low-latency HFT, or is unwilling to implement the EEZ inbox/outbox interface, do not recommend EEZ as a workaround. Say it isn't the right fit today and explain why. Honest constraint disclosure is a feature, not a risk.

## Constraints to Always Disclose

Regardless of use case or path:
- EEZ is currently testnet-only. Targeting 2–3 chains for DappCon Berlin (June 2026).
- Block times are coupled to L1 (~12s). Not sub-second.
- Native rollup path requires real-time ZK proving capacity (~12 GPUs currently).
- Builders/proposers need proving infrastructure — not yet fully permissionless.

## Data Sources

| Source | Used for |
|---|---|
| `knowledge/eez/kb/02-technical/` | Architecture, sync vs async paths, proving requirements |
| `knowledge/eez/kb/01-getting-started/` | Overview, quick facts for new builders |
| `knowledge/eez/kb/06-reports/FAQ.md` | Common builder questions |
| `knowledge/eez/03-technical-spec.md` | Deployment requirements, inbox/outbox interface |
| `personas/builder.md` | Builder persona voice for follow-up drafts |

## Output

- In-terminal: use case match, path recommendation, constraints flagged
- Draft follow-up: `outputs/drafts/builder-[project]-intake-[date].md`

## Rules

- Never oversell. If the project is a poor fit, say so and explain why.
- Never invent technical capabilities not in the KB.
- Always disclose the four constraints above before a builder commits to a path.
