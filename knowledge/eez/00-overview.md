# Ethereum Economic Zone (EEZ) — Overview

**Status:** Public. Announced at EthCC Cannes, 29 March 2026.
**Owners:** Gnosis (lead), ZisK (co-builder), Ethereum Foundation (co-funder).
**EEZ comms lead:** Ben Carvill.

---

## What it is

The Ethereum Economic Zone (EEZ) is a new rollup framework where, thanks to advances in ZK technology, rollup instances operate with full economic and social alignment to Layer 1, without introducing additional trust assumptions for users.

EEZ rollups are ZK-proven EVM rollup instances that are developed, deployed, and governed by Ethereum itself. They are not third-party infrastructure that settles to Ethereum. They are a native extension of the protocol.

> **The distinction that matters:** these are not L2s built *on* Ethereum. They are L2s built *by* Ethereum.

## Why now

Ethereum's L2 security promise has become misleading. The top L2s on L2beat carry "funds can be stolen" warnings. 99% of L2 economic value chooses not to use Ethereum for sequencing. Assets increasingly do not originate from Ethereum. The relationship between rollups and Ethereum is getting weaker.

EEZ is the response. It re-establishes Ethereum as the leading decentralised economic zone by deploying ZK-proven rollup instances that share Ethereum's security model, governance, and economics.

## What an EEZ rollup is, structurally

- **Governed by Ethereum's social and technical processes.** Same public core dev calls, same community review, same multi-year deliberation as any Ethereum protocol change. No company behind them. No foundation operating them.
- **No multisigs, no upgrade keys, no admin backdoors.** EEZ rollups inherit Ethereum's credible neutrality precisely because they inherit Ethereum's governance constraints.
- **Multi-client, multi-prover from day one.** At least two independent prover implementations, mirroring Ethereum L1's client diversity.
- **Economically aligned with Ethereum.** Sequencing value, MEV, and fees flow back to the protocol rather than to private operators. Ethereum's issuance mechanism could be extended to fund ZK proof generation as a protocol-level public good.

## Composability model

EEZ introduces a dual composability model.

| Direction | Reads | Writes |
|---|---|---|
| L2 → L1 | Synchronous | Synchronous |
| L1 → L2 | Asynchronous | Synchronous |
| L2 → L2 | Asynchronous | Synchronous |

**Synchronous composability** (the headline feature): a contract on one EEZ rollup can read Ethereum L1 state, or trigger an action on another EEZ rollup, as part of the same transaction. Instantly, atomically, with no bridge and no third-party messenger.

**Asynchronous composability** is the fallback for interactions with non-native rollups, preserving flexibility without forcing every rollup into a single model.

Result: liquidity stops behaving like separate markets and starts behaving like a single market without any infrastructure actually moving. **Everything stays where it is.**

## What EEZ is not

- **Not a competitor to existing L2s.** EEZ is the commodity layer in the Ethereum stack. L2s innovate on top with customisation specific to their general-purpose ecosystem or applications.
- **Not a single-team project.** It is built by the teams behind Gnosis and ZisK, co-funded by the Ethereum Foundation, and shaped by an alliance of EVM infrastructure providers, protocols, block builders, and ecosystem facilitators.
- **Not a new chain users need to migrate to.** Existing L2s, applications, and users are not asked to move.

## The 18–24 month journey

This is the start of an 18–24 month journey to unify the community and ensure Ethereum continues to prosper as the leading decentralised economic zone.

## Key links and assets

- **Lead account:** @EthEconomicZone (X)
- **Comms hub:** internal Google Doc (see `integrations/notion.md` or ask Ben)
- **Alliance Tally form:** https://tally.so/r/NpLo4l
- **Public Telegram:** "EEZ Alliance"

## Related files

- `knowledge/eez/01-comms-framework.md` — messaging pillars and example framings
- `knowledge/eez/02-alliance.md` — alliance structure, membership, partners
- `knowledge/eez/03-technical-spec.md` — technical detail for builder persona
- `knowledge/eez/04-launch-ethcc.md` — launch context, partner videos, post-launch sequence
- `knowledge/eez/sources/` — primary source materials (Martin's L2 talk, forum post, technical proposals, positioning, visual identity)
- `knowledge/eez/kb/` — full EEZ Knowledge Base mirrored from `gnosis-marketing/eez/Knowledge-Base/`. Six sections: getting-started, technical, ecosystem, media-and-analysis, research-materials, reports. Use this as the canonical reference for facts, FAQs, partner details, and media coverage.
