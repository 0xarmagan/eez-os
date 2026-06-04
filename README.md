# EEZ Specialist Agent

A Claude Code agent for the **Ethereum Economic Zone (EEZ)**, the **EEZ Alliance**, **GnosisDAO governance**, and Gnosis products. Operated by Armagan Ercan (Ecosystem Relations Lead, Gnosis Ltd).

## EEZ — current framing

**Thesis:** Ethereum has a fragmentation problem, not a scaling problem. Every L2 that launched with its own sequencer and bridge became a walled garden. EEZ activates Ethereum as a unified economic operating system.

**How it works:**
- L2s publish transaction data and validity proofs directly to Ethereum L1
- A **composer** aggregates cross-chain execution tables and settles them atomically on L1
- No bridges, no multisig upgrades, no trusted relayers — Ethereum's security model applies end to end

**Key architectural points:**
- EEZ does not impose a universal block cadence. Each chain defines its own block structure. EEZ only requires a synchronisation/finality point for the composable state transition.
- Based rollups and centralised-sequencer rollups can both participate. The integration path differs; the eligibility does not.
- Finality is required for atomic composability, but EEZ does not prescribe the proving mechanism. Each chain and its users decide what constitutes finality (ZK, TEE, validator committee, or combination).
- Composer fee market is an open design space — payment can be an L1 fee, L2 fee, direct payment, or private agreement.

**Alliance:** 10 founding members (Aave, Safe, Flashbots, CoW Swap, Nethermind, Centrifuge, Titan, Beaver Build, Monerium, xStocks). Open to any rollup that meets the finality and commitment rules.

## What this repo is

- `CLAUDE.md` — agent identity and operating manual
- `knowledge/` — cached corpus (EEZ thesis, technical spec, media, alliance, Gnosis products)
- `data/` — structured YAML (partners, GIPs)
- `skills/` — five canonical skills (see below)
- `personas/` — three access scopes with routing logic
- `outputs/` — drafts land here; all external artifacts require human review before publishing

## Skills

| Skill | Purpose |
|---|---|
| `alliance-outreach` | Partner check-ins, co-marketing, event content, X threads |
| `builder-enablement` | Integration qualification and path recommendations |
| `content-ingestion` | Ingesting new sources into the knowledge base |
| `ecosystem-intel` | Competitive landscape, L2 interop intel, weekly brief |
| `gip-authoring` | GnosisDAO governance proposals, lifecycle management |

## Personas

1. **Internal** — Full corpus access including partner status, draft GIPs, internal strategy.
2. **Partner** — Co-marketing context, public messaging, their own record only.
3. **Builder** — Public technical material only.

Persona detection runs first on every conversation. Default fallback: `builder` (least-privilege). See `personas/routing.md`.

## Sourcing model

Cached corpus first, live lookup second. Live wins for governance state (Snapshot, forum). Cached wins for positioning, voice, and partner relationships.

## Repo structure

```
eez-agent/
├── CLAUDE.md
├── README.md
├── STRUCTURE.md              # Full navigation guide
├── .claude/
│   └── commands/             # Slash commands
├── personas/                 # Internal / partner / builder + routing
├── knowledge/
│   ├── eez/                  # Thesis, comms, alliance, technical spec, KB
│   ├── gnosis/               # Products + DAO governance
│   ├── ecosystem/            # L2 / Ethereum landscape
│   └── reference/            # Style guide, brand, stakeholder map
├── skills/                   # Five canonical skills
├── workflows/                # Multi-step playbooks
├── templates/                # Reusable artifact scaffolds
├── data/                     # partners.yaml, gips.yaml
├── docs/                     # Event summaries & transcripts
└── outputs/
    └── drafts/               # Work-in-progress (gitignored)
```

All external-facing artifacts require Armagan's review before publishing.
