# EEZ Specialist Agent

A Claude Code agent that serves as a specialist on the **Ethereum Economic Zone (EEZ)**, the **EEZ Alliance**, **GnosisDAO governance**, and the seven **Gnosis products**.

## What this is

A repo where:
- `CLAUDE.md` is the agent's identity and operating manual
- `knowledge/` is the cached corpus (the agent's long-term memory)
- `data/` holds structured situational awareness (partners, GIPs, KPIs)
- `skills/` and `.claude/` hold specialised capabilities
- `personas/` defines who the agent is talking to and what they can see
- `integrations/` documents live data sources (Snapshot, forum, Notion)
- `outputs/` is where drafts land before human review

## Personas

The agent serves three:
1. **Internal** — Ben Carvill (Head of DAO & Ecosystem) and the wider Gnosis team. Full corpus access.
2. **Partner** — EEZ Alliance member. Public + their own record.
3. **Builder** — External developer or researcher. Public material only.

Persona detection is the first action on every conversation. See `personas/routing.md`.

## Hybrid sourcing

Cached corpus first, live lookup second. Live wins for governance state (Snapshot, forum), cached wins for positioning, voice, and partner relationships. See `knowledge/reference/sourcing-policy.md`.

## Build sequence (recommended)

1. **Week 1** — `CLAUDE.md`, EEZ knowledge, product corpus (this repo's starting state)
2. **Week 2** — Skills and templates
3. **Week 3** — Subagents and slash commands
4. **Week 4** — Live integrations (Snapshot, forum, Notion, Dune)

## Repo structure

**Full navigation guide:** See `STRUCTURE.md` for detailed directory map with use-case routing.

```
eez-agent/
├── CLAUDE.md                 # Identity + operating manual
├── README.md                 # This file
├── STRUCTURE.md              # Complete navigation guide
├── .claude/
│   ├── commands/             # Slash commands
│   ├── agents/               # Subagents
│   └── hooks/                # Pre/post-tool hooks
├── personas/                 # Internal / partner / builder + routing
├── knowledge/
│   ├── eez/                  # EEZ thesis, comms, alliance, technical spec
│   ├── gnosis/
│   │   ├── products/         # 7 product files
│   │   └── dao/              # Governance hub, moderation, crisis response
│   ├── ecosystem/            # Wider L2 / Ethereum landscape
│   ├── reference/            # Style guide, stakeholder map, sourcing policy
│   └── _meta/                # Corpus manifest, freshness TTLs
├── skills/                   # GIP authoring, partner brief, governance analysis, etc.
├── workflows/                # Multi-step playbooks
├── templates/                # Reusable artifact scaffolds
├── data/                     # YAML data files (partners, GIPs, KPIs)
├── integrations/             # MCP / API connection notes
├── docs/                     # Event summaries & transcripts (archived)
└── outputs/                  # Organized by type
    ├── drafts/               # Work-in-progress materials
    │   ├── community-events/      # Calls, scripts, moderator notes
    │   ├── technical-deep-dives/  # Architecture, proving, explanations
    │   └── planning-reference/    # Governance, roadmaps, tokenomics
    └── approved/             # Finalized, human-reviewed
```

## Open design decisions

- **Source of truth for partners and GIPs** — repo YAML, Notion, or live Snapshot/forum?
- **Public vs private repo** — affects whether the corpus can include unredacted partner contacts and internal strategy.
- **Output review gate** — every external-facing artifact through human review, or some commands ship directly?

These are flagged in the original plan and need confirmation before live integrations are wired.

## Owners

- **Repo owner:** Ben Carvill (Head of DAO & Ecosystem, Gnosis Ltd)
- **Operating contractor:** Timesthree.io Ltd
