# EEZ — Competitive Claims Analysis

**Status:** Research — verified against live sources
**Last updated:** 2026-05-31
**Sources:** L2Beat, project docs, ethresear.ch, company websites

---

## Headline positioning

| Project | Headline claim |
|---|---|
| **Superchain (Optimism)** | "Building a Superchain" / "The future of Ethereum" |
| **AggLayer (Polygon)** | "Liquidity and users unified, finally" / "The future of Web3 is aggregated" |
| **ZK Stack (zkSync)** | "Incorruptible Financial Infrastructure" / "The Elastic Network" |
| **Arbitrum Orbit** | "Launch a Chain" / "Powering the programmable economy" |
| **Based rollups (Taiko/Spire)** | "First-ever based rollup scaling Ethereum" / "True extension of Ethereum" |
| **EEZ** | "Ethereum stops being a single chain. It becomes an economic operating system." |

---

## Governance — the load-bearing comparison

| Project | Who governs | Upgrade keys | L2Beat stage |
|---|---|---|---|
| **Superchain** | Optimism Collective (Token House + Citizens' House) | Foundation-managed, path to decentralisation | Stage 1 (Optimism) |
| **AggLayer** | Absent from brand — Pessimistic Proofs substitute for governance | Not disclosed prominently | N/A |
| **ZK Stack** | ZK Nation / Matter Labs whitelisting for new chains | CTM — whitelisting required | Varies per chain |
| **Arbitrum Orbit** | Arbitrum DAO for L2; Orbit chains govern themselves independently | Chains autonomous — DAO does not govern them | Stage 1 (Arbitrum One) |
| **Taiko (based)** | 7/9 Security Council + 4/6 Taiko Multisig + Taiko DAO (veto only) | Security Council — emergency execution, no exit window | **Stage 0** |
| **Spire chains (based)** | Chain operator — full sovereignty explicitly granted | Chain operator controls upgrade cadence and permissioning | N/A |
| **EEZ** | Ethereum's own governance processes (EIP-equivalent) | No multisigs. No upgrade keys. No admin backdoors. | Pre-mainnet |

---

## What based sequencing governs (and what it doesn't)

Based rollups use Ethereum L1 proposers as sequencers. This covers **transaction ordering only**.

| Governance dimension | Based rollups | EEZ |
|---|---|---|
| Transaction ordering | Ethereum validators ✓ | Ethereum validators ✓ |
| Upgrade path | Security Council / multisig | EIP-equivalent community process |
| Proof system changes | Multisig (upgradeable by team) | Community review required |
| Zone/chain parameters | Chain operator | EIP-equivalent community process |
| Bridge parameters | Governance multisig | Community review required |
| Emergency override | Immediate — no exit window (Taiko) | None — no admin backdoor |

Source: L2Beat Taiko assessment (Stage 0, three unmet Stage 1 criteria); Spire docs (`docs.spire.dev/pylon/based-appchains.md`); ethresear.ch based rollups paper.

---

## Taiko's actual governance model (verified)

From L2Beat (2026-05-31):
- **Stage 0** — three unmet Stage 1 criteria
- Unmet: censorship by permissioned operators; upgrades with less than 7-day exit window in emergency; Security Council not properly set up
- Upgrade permissions: `MainnetInbox` (core L1 contract) upgradeable, admin/owner role held by `TaikoDAOController` → `SignerList (Security Council)`
- Emergency upgrades: 7/9 Security Council approval, **execute immediately, no delay**
- ZK verifier contracts: "can be upgraded by Multisigs" (L2Beat)
- Taiko DAO: token holders have veto power only — "limited to veto permissions"

Taiko's own governance claim: "TAIKO token holders control smart contract upgrades, network parameters, treasury allocation" — Taiko DAO governs these, not Ethereum.

Taiko's Ethereum claim: L1 proposers handle sequencing. Governance of the rollup environment is internal to Taiko.

---

## Spire's actual governance model (verified)

From Spire docs (`docs.spire.dev`):
- "Control upgrades, token economics, and governance policies while inheriting settlement security"
- "Choose upgrade cadence and permissioning"
- Credible neutrality defined as: stack doesn't depend on a token, specific service provider, or extract sequencing revenue — this is about Spire as infrastructure, not a governance standard on chains

**Conclusion:** Spire grants each chain full sovereignty over its governance. No governance standard imposed.

---

## Ethereum relationship — the spectrum

| Project | Their framing | Actual governance |
|---|---|---|
| **Based rollups** | "True extension of Ethereum" / "L1-sequenced" | Sequenced by Ethereum; governed by internal actors |
| **EEZ** | "Zones are expressions of Ethereum" | Governed by Ethereum's processes — broader scope than sequencing |
| **Superchain** | "Ethereum-aligned" | Governed by the Collective — not by Ethereum |
| **ZK Stack** | "Inherit Ethereum's security" | Security from Ethereum; governance from ZK Nation |
| **Arbitrum Orbit** | "Securely scales Ethereum" | DAO governance; Orbit chains are autonomous |
| **AggLayer** | Chain-agnostic — deliberately not Ethereum-first | No Ethereum alignment claim |

---

## Competitive white space — what no competitor claims

| Unclaimed territory | Why competitors can't take it |
|---|---|
| "Governed by Ethereum" — full stack under Ethereum's EIP-equivalent process | Superchain: Collective governs. ZK Stack: ZK Nation. Arbitrum: autonomous. AggLayer: avoids governance. Based rollups: sequencing only — upgrade keys remain with team. |
| "No upgrade keys, no multisig, no admin backdoors" | Every project retains some upgrade path control. Taiko: Security Council with immediate emergency execution. Spire: chain operator. This is the hardest commitment to make credibly. |
| "Economic operating system" | Nobody uses this framing. AggLayer's "aggregated" thesis is chain-agnostic, not governance-first. |
| "Zones are expressions of Ethereum, not products built on top" | Requires the upgrade path to run through Ethereum's community processes. No competitor can claim this with their current architecture. |
| "The only scaling standard Ethereum itself can verify" | On-chain verifiable governance is Phase 3 work for EEZ — but when live, no competitor can match it. Taiko is Stage 0 today. |

---

## Sources

- L2Beat Taiko: `https://l2beat.com/scaling/projects/taiko`
- Spire docs: `https://docs.spire.dev/pylon/based-appchains.md`
- Spire credible neutrality: `https://docs.spire.dev/based-stack/open-source-and-credibly-neutral.md`
- ethresear.ch based rollups: `https://ethresear.ch/t/based-rollups-superpowers-from-l1-sequencing/15016`
- Optimism governance: `https://www.optimism.io/blog/the-future-of-optimism-governance`
- AggLayer: `https://polygon.technology/agglayer`
- ZK Stack: `https://docs.zksync.io/zk-stack`
- Arbitrum Orbit: `https://arbitrum.io/orbit`
- Taiko 2025 recap: `https://paragraph.com/@taiko-labs/taiko-in-2025-what-we-proved`
