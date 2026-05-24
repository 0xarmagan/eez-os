---
name: ecosystem-intel
description: Use when needing a current competitive intelligence brief on L2 interoperability — competitor moves, research developments, opportunity flags, and threat signals across Superchain, AggLayer, zkSync Elastic Chain, Arbitrum, and related protocols.
---

# Ecosystem Intel Skill

On-demand competitive and ecosystem intelligence brief for the L2 interoperability landscape. Gives a structured picture of what competitors are doing, what the research community is saying, and where EEZ has an opening or is under pressure.

## How It Works

**This is a triggered skill, not an automated feed.** When you run `/ecosystem-intel`, Claude performs a live web search across the tracked sources, synthesises competitor moves and research signals, and returns a structured brief. Nothing runs in the background automatically. To run it on a schedule, pair with `/loop 7d /ecosystem-intel` or set up a cron.

## Trigger

- "ecosystem intel", "competitive scan", "L2 interop landscape"
- "what are [Superchain / AggLayer / zkSync / Arbitrum] doing"
- "weekly brief", "what should I know this week", "EEZ positioning check"
- `/ecosystem-intel [period]` — default: last 7 days

## Competitors to Monitor

| Protocol | What to watch |
|---|---|
| **Superchain / OP Stack** | New chain additions, sequencer sharing, Optimism/Base/Worldchain/Unichain announcements |
| **AggLayer / Polygon** | Pessimistic proof updates, cross-chain aggregation launches, new chain onboarding |
| **zkSync Elastic Chain** | Native ZK interop releases, chain additions, sequencer/prover updates |
| **Arbitrum Orbit + Stylus** | Custom chain stack updates, cross-chain messaging, Stylus adoption |
| **EigenLayer / restaking** | Shared sequencing AVS, interop-adjacent restaking projects |
| **Flashbots / SUAVE** | Shared sequencing, cross-chain MEV, builder/proposer infrastructure |

## Output Structure

```
## Ecosystem Intel — [date range]

### Competitor Moves
What each tracked competitor announced, shipped, or signalled this period.
One bullet per item. Protocol name first.

### Research and Proposals
Relevant EIPs, Ethereum Research posts, Vitalik writes, L2Beat updates,
academic papers. Flag anything that intersects with EEZ's technical claims.

### Opportunity Flags
Protocols or chains that could be EEZ alliance candidates.
Gaps competitors are not filling. Builder pain points surfacing in public.

### Threat Signals
Competitor launches or narratives that directly contest EEZ positioning.
Be specific — what claim, what framing, which audience.

### EEZ Response — One Line Each
For each threat or opportunity: what does EEZ say or do?
This is the actionable layer — draft talking point, positioning note,
or flag for Ben if it requires a strategic response.
```

## Live Sources (Web Search Required)

Run live search on each brief — no cached source is sufficient for competitive intel.

| Source | What to pull |
|---|---|
| L2Beat (l2beat.com) | Rollup stats, new chain additions, TVL shifts |
| Ethereum Research (ethresear.ch) | Proposals, technical debates relevant to shared sequencing / cross-chain |
| Protocol official blogs and GitHub releases | Announcements, technical specs |
| X / CT | Founder posts, ecosystem reactions, community narratives |
| Bankless, Delphi Digital, The Defiant | Narrative-level analysis, research reports |

Flag if `knowledge/_meta/freshness.yaml` shows cached competitive data is stale — live search takes priority over cached summaries.

## EEZ Positioning Reference

When writing the EEZ response layer, anchor to:
- `knowledge/eez/01-comms-framework.md` — messaging pillars
- `knowledge/eez/00-overview.md` — core thesis and differentiation
- `knowledge/reference/brand/eez-guardrails.md` — what not to claim

Key EEZ differentiators to defend: synchronous composability (same-block atomicity), ZK-based proving (ZisK), chain-agnostic design (any STF), Gnosis backing and governance credibility.

## Output

Structured brief in terminal. Optionally save to `outputs/drafts/ecosystem-intel-[date].md` for sharing with Ben or team.

## Rules

- Always use live web search. Competitive landscape moves fast — cached data is stale by definition.
- Be specific on threat signals. "Superchain is a threat" is not useful. "Superchain announced shared sequencing with 8 chains and framed it as 'atomic composability' in their EthCC keynote" is.
- The EEZ response column is the actionable output — don't skip it.
- Flag anything that requires a Ben-level strategic decision rather than a talking-point response.
