# EEZ Skills Redesign — Spec

**Date:** 2026-05-24
**Role:** Ecosystem Relations Lead, EEZ Alliance / Gnosis Ltd
**Decision:** Full redesign. Replace all 5 existing skills with 3 purpose-built ones. Keep `gip-authoring` untouched.

---

## What Changes

| Old skills (removed) | New skill |
|---|---|
| `partner-brief` + `co-marketing-brief` | `alliance-outreach` |
| `alliance-activation-matrix` + `partner-sentiment-tracker` | *(dropped — not needed)* |
| *(none)* | `builder-enablement` |
| *(none)* | `ecosystem-intel` |
| `gip-authoring` | unchanged |

---

## Skill 1 — `alliance-outreach`

**Purpose:** All partner-facing communication AND partner-related content creation. Context-aware by mode — same skill whether you are writing an outreach email or drafting an X thread about a co-marketing campaign.

**Trigger phrases:**
- "draft outreach to [partner]", "co-marketing brief for [partner]"
- "X thread about [partner/milestone]", "post about [campaign/announcement]"
- "check in with [partner]", "re-engage [partner]"
- "content for [event/launch]", "draft [partner] announcement"
- `/alliance-outreach [partner] [mode]`

**Modes:**

`first-contact` — Initial alliance pitch. Private, builder-only framing. Ends with Tally form CTA. Tone: peer-to-peer, no corporate formality. Pull partner's focus area from `partners.yaml` and tailor the pitch to what EEZ unlocks for their specific chain/protocol.

`check-in` — Lightweight relationship maintenance. 3–5 sentences. References the last interaction or commitment. Asks one concrete question or makes one nudge. No long recap.

`co-marketing` — Full campaign coordination brief. Outputs two things: (1) a brief for the partner telling them what to do, when, and what EEZ will do in parallel; (2) internal asset checklist. Covers: message alignment, asset handoff, posting sequence, amplification window.

`event-coordination` — Pre/during/post event comms. Pre: who we want to meet, what we want from them, what we offer. During: side event logistics, interview requests, shared panel intros. Post: follow-up message referencing the in-person moment.

`recovery` — Re-engaging a drifting partner. Tone calibrated to drift severity: light (just quiet) vs serious (missed commitments). Never accusatory. Leads with value, not pressure.

`content` — Partner-related content for public channels. See content detail below.

---

### Content Mode — Expanded

This mode handles all written content where EEZ or a partner is the subject. Specify the channel and format; the skill applies the right voice rules automatically.

**X threads (most common):**
- First tweet must include EEZ context for cold readers who don't know what EEZ is
- Short punchy paragraphs, no bullet lists, no em dashes
- Informative over clickbait — explain the actual thing, not just hype it
- CTA at the end (follow, read, apply, attend — one ask only)
- Max ~5 tweets. If it needs more, it's two threads.
- Pull from: `knowledge/reference/voice/channels/x.md`

**Announcement posts (partner joins alliance, integration live, milestone):**
- Lead with the fact, not the excitement. "X is now part of the EEZ Alliance." Not "We're thrilled to welcome..."
- Second sentence: why this partner, what they bring
- Third: what this means for builders / the ecosystem
- End with one concrete next step or link
- Adapt length to channel: X thread (5 tweets), LinkedIn (200–300 words), blog (400–600 words)

**Co-marketing content (joint posts, shared campaigns):**
- Produce two versions: EEZ-voice version (for EEZ accounts) and partner-voice brief (for partner to adapt)
- Message sync check: does the partner copy match EEZ's framing on synchronous composability? Flag divergences.
- Amplification sequence: who posts first, what the other amplifies, timing window (same day vs 24h apart)
- Asset checklist: what EEZ provides (logo, boilerplate, quote from Jordi/Ben) vs what partner provides

**Event content (recaps, side event posts, live coverage):**
- Recap format: hook (what happened), 2–3 key moments, one quote if available, link or CTA
- Live coverage: short, real-time, first-person. "At [event], [thing happened]." Not polished prose.
- Post-event thread: what was announced, who was there, what comes next

**Data sources for content mode:**
- `knowledge/reference/voice/channels/x.md` — X format and tone
- `knowledge/reference/voice/channels/linkedin.md` — LinkedIn format
- `knowledge/reference/brand/eez-boilerplate.md` — standard EEZ copy blocks
- `knowledge/reference/brand/eez-guardrails.md` — what not to say
- `knowledge/eez/01-comms-framework.md` — messaging pillars
- `knowledge/reference/style-guide.md` — voice rules, banned phrases

**Output for all content:** Draft saved to `outputs/drafts/content-[partner/topic]-[channel]-[date].md`. Always a draft, never published directly.

---

**Key rules across all modes:**
- No em dashes. No "excited to announce." Active voice. British English when writing as Ben.
- Always check `data/partners.yaml` for partner's preferred channel and contact before drafting.
- Never invent partner quotes or commitments. If uncertain, leave a `[PLACEHOLDER — confirm with partner]` note.
- Drafts go to `outputs/drafts/`. Ben approves before anything goes out.

---

## Skill 2 — `builder-enablement`

**Purpose:** Turn inbound builder interest into a structured EEZ integration path. This is the first substantive touchpoint for any project that wants to build on EEZ — it qualifies fit, recommends the right path, sets honest expectations, and hands off to the right resources.

**Trigger phrases:**
- "help [project] build on EEZ", "evaluate [project] for EEZ fit"
- "builder intake for [project]", "someone wants to integrate with EEZ"
- "what can [project] do with EEZ", "is EEZ right for [project]"
- `/builder-intake [project]`

---

### Use Case Qualification — Expanded

This is the highest-value step. Before recommending a path, understand what the builder actually needs. Ask or infer from context:

**Lending protocols (e.g. Aave, Morpho-style)**
- Key pain: liquidations are slow and siloed per chain. A position on Chain A can't be liquidated using collateral or capital from Chain B.
- EEZ unlock: synchronous cross-chain liquidation. Liquidator calls a single atomic transaction that reads the position on Chain A and executes using funds on Chain B in the same block.
- Also unlocks: cross-chain flash loans. Borrow on Chain A, use on Chain B, repay in same block. No bridge risk.
- Path: native rollup (full atomicity). Async path not suitable — liquidations need guaranteed execution, not eventual finality.
- Constraint to disclose: ~12s block time (coupled to L1). Not sub-second. If they need faster liquidations, EEZ is not the answer today.

**DEXs / AMMs (e.g. Uniswap, Balancer-style)**
- Key pain: liquidity is siloed. A pool on Gnosis Chain can't atomically arbitrage against a pool on Arbitrum.
- EEZ unlock: cross-chain atomic arbitrage in a single transaction. Unified liquidity views across EEZ chains.
- Also unlocks: cross-chain MEV — builders/proposers can compose trades across chains in one block.
- Path: native rollup preferred. Async path works for lower-frequency cross-chain routing.
- Constraint: arbitrageurs need access to the EEZ block builder / proposer infrastructure. Early-stage — not permissionless yet.

**Bridges and asset movement**
- Key pain: bridges introduce trust assumptions, delay, and capital lock-up.
- EEZ unlock: native cross-chain asset movement without a bridge. If both chains are EEZ native rollups, moving assets is atomic state transition, not a bridge operation.
- Path: native rollup on both source and destination chains.
- Constraint: both chains must be EEZ native rollups. If one chain is not, fallback to async path (intents-based relay).
- Honest framing: EEZ does not replace bridges for chains outside the EEZ zone. It replaces bridges between EEZ chains.

**Restaking / yield protocols**
- Key pain: yield opportunities are chain-specific. Rebalancing across chains is slow and expensive.
- EEZ unlock: cross-chain position management in one atomic transaction. Read yield rates across chains, rebalance in same block.
- Path: native rollup for full atomicity. Async for directional rebalancing with looser timing requirements.

**Gaming and NFTs**
- Key pain: assets are locked on one chain. Cross-chain asset use requires bridges with long finality times.
- EEZ unlock: cross-chain asset reads and state updates in one block. An NFT on Chain A can gate access to a game state on Chain B atomically.
- Path: async path is often sufficient here (lower value, less atomicity-critical). Native rollup for high-value in-game asset operations.
- Note: gaming needs sub-second UX for most interactions. EEZ at ~12s is suitable for settlement, not real-time gameplay.

**Infrastructure (sequencers, provers, DA layers)**
- Key pain: every rollup runs its own proving and sequencing infrastructure. High fixed cost.
- EEZ unlock: shared proving infrastructure (ZisK), shared sequencing across EEZ chains. Reduces cost of running a rollup.
- Path: becoming an EEZ native rollup. Requires implementing inbox/outbox interface.
- This is the "deploy a chain on EEZ" path, not "use EEZ from an existing chain."
- Constraint: real-time ZK proving currently requires ~12 GPUs. Not trivial to operate independently.

**Stablecoins and cross-chain collateral**
- Key pain: stablecoin collateral is siloed. A collateral position on one chain can't back a stablecoin mint on another without a bridge.
- EEZ unlock: atomic cross-chain collateral reads. Mint on Chain B against collateral verified atomically on Chain A.
- Path: native rollup for full atomicity guarantees.

---

### Path Recommendation Logic

After qualification, recommend one of two paths:

**Native rollup path** — for: lending (liquidations, flash loans), DEX arbitrage, bridges between EEZ chains, high-value asset operations, stablecoins with cross-chain collateral.
- Full synchronous composability
- ~12s block time (L1 coupled)
- Requires real-time ZK proving capacity
- Inbox/outbox interface implementation required
- Entry point: DappCon Berlin workshops (June 2026), testnet with 2–3 chains

**Async path** — for: gaming (settlement layer), directional rebalancing, cross-chain routing with looser timing, any use case where eventual finality is acceptable.
- Cross-chain intents via relayers
- No proving requirement on the builder's side
- Lower barrier to entry
- Finality: eventual (not same-block)
- Entry point: async path documentation in `knowledge/eez/kb/02-technical/`

**Poor fit (say clearly):** Any use case requiring sub-second finality, ultra-low-latency HFT, or chains not willing to implement EEZ interface. Do not recommend EEZ as a workaround. Flag and suggest alternatives.

---

### Workflow Steps

1. **Qualify** — ask 2–3 targeted questions or infer from context: what does the project do, what chain are they on, what cross-chain problem are they trying to solve?
2. **Match use case** — map to one of the use case patterns above
3. **Recommend path** — native rollup vs async, with honest constraints
4. **Disclose constraints** — always: current testnet status, block time, proving requirements, timeline
5. **Route to resources** — specific KB files, technical contacts, upcoming workshops
6. **Draft follow-up** — short email or Telegram message: path summary, next steps, links

**Data sources:**
- `knowledge/eez/kb/02-technical/` — architecture, paths, constraints
- `knowledge/eez/kb/01-getting-started/` — overview, quick facts
- `knowledge/eez/kb/06-reports/FAQ.md` — common questions
- `knowledge/eez/03-technical-spec.md` — deployment requirements
- `personas/builder.md` — builder persona voice

**Output:**
- In-terminal: qualification summary + path recommendation + constraints
- Draft follow-up: `outputs/drafts/builder-[project]-intake-[date].md`

**Key rules:**
- Never oversell. If the project is a poor fit, say so and explain why.
- Honest constraint disclosure is a feature of the agent's credibility, not a risk.
- Never invent technical capabilities not in the KB.

---

## Skill 3 — `ecosystem-intel`

**Purpose:** On-demand competitive and ecosystem intelligence brief for the L2 interoperability landscape. Gives Armagan a structured picture of what competitors are doing, what the research community is saying, and where EEZ has an opening or is under pressure.

**How it works — not a bot:**
This is a triggered skill, not an automated feed. When you run `/ecosystem-intel`, Claude executes a live web search across the tracked sources, synthesises the results, and returns a structured brief. Nothing runs in the background. If you want it on a schedule, pair it with `/loop` or a cron setup — but by default it only runs when you ask.

**Trigger phrases:**
- "ecosystem intel", "competitive scan", "L2 interop landscape"
- "what are [Superchain / AggLayer / zkSync] doing"
- "weekly brief", "what should I know this week"
- `/ecosystem-intel [period]` (default: last 7 days)

**Competitors tracked:**
- Superchain / OP Stack (Optimism, Base, Worldchain, Unichain)
- AggLayer / Polygon (cross-chain aggregation, pessimistic proof)
- zkSync Elastic Chain (native interop via ZK proofs)
- Arbitrum Orbit + Stylus (custom chain stack, cross-chain messaging)
- EigenLayer / restaking-based interop (shared sequencing, AVS)
- Flashbots / SUAVE (shared sequencing, cross-chain MEV)

**Output structure:**

```
## Ecosystem Intel — [date range]

### Competitor Moves
[What each tracked competitor announced, shipped, or signalled this week]

### Research & Proposals
[Relevant EIPs, Ethereum Research posts, Vitalik writes, L2Beat updates, academic papers]

### Opportunity Flags
[Protocols or chains that could be EEZ alliance candidates. Gaps competitors aren't filling.]

### Threat Signals
[Competitor launches or narratives that directly contest EEZ positioning]

### EEZ Response — One Line Each
[For each threat or opportunity: what does EEZ say or do in response?]
```

**Live sources (web search required):**
- L2Beat (l2beat.com) — rollup stats, new chain additions
- Ethereum Research (ethresear.ch) — proposals, technical debates
- Official protocol blogs and GitHub releases
- X / CT — announcements, founder posts, community reactions
- Bankless, Delphi Digital, The Defiant — narrative-level analysis

**Freshness:** Always live search. Flag if `knowledge/_meta/freshness.yaml` shows cached competitive data is stale.

**Output:** Structured brief in terminal. Optionally save to `outputs/drafts/ecosystem-intel-[date].md` for sharing with Ben or team.

---

## Files to Delete

- `skills/partner-brief/`
- `skills/co-marketing-brief/`
- `skills/alliance-activation-matrix/`
- `skills/partner-sentiment-tracker/`

## Files to Create

- `skills/alliance-outreach/SKILL.md`
- `skills/builder-enablement/SKILL.md`
- `skills/ecosystem-intel/SKILL.md`
