# EEZ Overview, and Why Gnosis Will Become an L2 (Dappcon talk)

Source: **Dappcon Berlin, 16 June 2026**, Martin Köppelmann. Talk "Fast sequencer or global censorship-resistant consensus? Getting the best of both with EEZ" (deck title "EEZ and why Gnosis will now become an L2"), 29:03. Opens the EEZ block, followed by Jordi Baylina (real-time proving) and Phillipe Schommers (Gnosis Chain transition), then the Vitalik/Friederike panel. Primary source; quote as Martin's framing, not approved comms.

## The problem: fragmentation
Ethereum's value was permissionless composability (Lego pieces). Today those pieces are fragmented across chains: ESA (interest-earning) is on Gnosis but the conditional-token framework instance is on Polygon; privacy pools on Arbitrum; Self protocol on Celo; euro liquidity split across chains. Bridging is slow (seven days for fraud-proof rollups), so combining them is impractical. Martin attributes much of Ethereum's under-adoption to this fragmentation.

## What EEZ is
A set of chains much more tightly connected to Ethereum, doing real-time settlement. "Real time" = every 12 seconds (one Ethereum block). Chains that join get synchronous composability: instant cross-chain reads and writes, atomic, one transaction.

## Two requirements to be an EEZ chain (Martin's framing)
1. **Commit your new state to Ethereum within 12 seconds, with a firm, non-reversible commitment.** This needs zk proofs, or a trust-based method (TEE or even a multisig) as long as the commitment is firm and fast. This is the real-time component.
2. **Accept Ethereum as the ultimate source of truth.** Ethereum usually only goes forward, but reorgs happen roughly three times a day. When Ethereum reorgs, your L2 must follow.

## Time-traveling intuition
The L2 peeks ~6 seconds into the future: it builds a block now whose transactions will only land in the Ethereum block ~6s ahead, anticipating the result, then commits and checks the settlement landed as expected. For fork-dependent transactions, run multiple versions in parallel (this settlement, else that one), confirming the fork-independent transactions regardless.

## Why still have L2s (four arguments)
1. **Cost.** A swap on Ethereum is ~15 cents (at ~$10 base cost); fine sometimes, but Gnosis Pay buying a $2 coffee needs sub-cent fees, impossible on L1.
2. **Speed.** 12-second L1 blocks; many use cases need faster.
3. **Distribution.** CEX "franchise model" chains; with EEZ their activity feeds back into Ethereum's network effects.
4. **Neutrality, or being opinionated.** Ethereum should stay credibly neutral even at the cost of letting hackers keep stolen funds. L2s can be opinionated: e.g. an L2 can have processes to revert a large hack.

## Bootstrapping advantage
Past "fix interop" attempts (shared sequencing zones) need many chains to join before they are useful. EEZ is useful from day one because Ethereum is part of it: the first chain to join already has synchronous composability with Ethereum.

## Gnosis Chain plan
- Gnosis history: deployed on xDai side chain, took it over in 2021 (became Gnosis Chain), did the merge with its own beacon chain, was critical of the L2 roadmap (Martin's "limits of L2" talk), then "native L2s" at Devcon 2024, now EEZ.
- **Two tracks:** (a) Gnosis Chain becomes an EEZ chain while keeping a sequencer and faster block times; (b) **Rollup One**, a fully based EEZ chain (uses Ethereum timestamps / 12s block time, keeps GNO as gas/guest token), which starts as a testnet **Rollup Zero** and turns into Rollup One.
- **Who must permission EEZ: no one.** Permissionless, no hard fork needed. Builders and validators can help by making settlement more predictable, but are not required.
- **Timeline:** summer 2026 something live on Ethereum (Rollup Zero testnet); by end of year, Gnosis Chain with limited EEZ functionality (synchronous calls, perhaps one-direction or single-return first).
- **Who benefits:** short-term, most interesting for chains that have not reached escape velocity; Base and Arbitrum are already strong so less urgent; long term, network effects make it interesting for everyone.

---

*Filed 2026-06-19. Companion to the other Dappcon source digests.*
