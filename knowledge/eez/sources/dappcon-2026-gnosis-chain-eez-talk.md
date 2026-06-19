# Gnosis Chain: The First EEZ Chain (Dappcon talk)

Source: **Dappcon Berlin, 18 June 2026** (Day 1), Phillipe Schommers (Gnosis Head of Infrastructure, @filoozom). Talk "The First EEZ Chain", 32 slides, co-branded Gnosis Chain × EEZ. Companion decks the same day: Martin Köppelmann, "EEZ and why Gnosis will now become an L2"; Jordi Baylina, "Real-Time Proving and Synchronous Composability Between Rollups". Primary source; quote as the speakers' framing, not approved comms. Pre-GIP: the Gnosis-as-EEZ-chain plan is now public via this talk, but check current GIP status before any external comms.

## Headline
Gnosis Chain intends to be the first EEZ chain. "Not an add-on: it touches blocks, proofs, validators, consensus." Gnosis Chain has been live since 2018 and is basically Ethereum with slight variations, which is what makes the move feasible.

## What makes an EEZ chain: four integration points (sovereign in the rest)
1. **Reorgs with Ethereum**, Ethereum's fork choice becomes the chain's own.
2. **EEZ contracts**, one contract set on Ethereum; many chains are proven and batched together.
3. **A composer**, the chain runs, or shares, the builder that simulates synchronous blocks.
4. **Cross-chain proxies**, the chain addresses the rest of the zone through local stand-ins.

Everything else stays sovereign: block production, permissioning, proofs, and data availability.

## Trust model is a parameter: multisig today, zk tomorrow
- **Today: BLS × N validators.** A multisig. Signatures from known validators are the proof system. Bridge validators re-execute every block on diverse clients (client diversity), then sign. The M-of-N attestation is the proof, and Ethereum's EEZ contracts verify it.
- **Next: BLS + zk hybrid.** Add a zk verifier to the set, then another, and raise the threshold.
- **Goal: zk × 3.** Drop the signers; the permissioned actors retire from the critical path.
- Key line: **"Same protocol, different proof systems. Upgrading trust never changes the contracts."** This is the eez-core-protocol per-rollup threshold model in practice: the proof system and the M-of-N threshold are the chain's configurable choice, and zk provers later slot into the same threshold the multisig used. "Same slot, swappable proof."

## Already built (prototype)
- **Feb 2026:** Jordi Baylina's ethresear.ch post. **April 2026:** a full prototype, a based rollup on reth, live on **Chiado** (Gnosis testnet). Roughly three months from post to running prototype.
- Demonstrated: atomic instant deposits and withdrawals (both directions); calls with return values (L1 reads an answer computed on L2 in the same transaction); cross-chain flash loans (borrow, work, repay, one transaction); nested calls five levels deep (ping-pong between chains mid-transaction).
- "Independent full nodes re-derive identical state from L1 alone." Confirms the follow-one-chain client model.

## Design decisions
- **Sequenced, 2-second blocks; multisig start; blobs on Ethereum.**
- **Why sequenced:** three things EEZ needs that the Gnosis beacon chain was never built for (sequencing enables the synchronous sync block).
- **What happens to the validator set:** the current set is most likely deprecated; the DAO decides.

## Timing and pre-building
- Inclusion is fast; proving eats the end of the slot. On a 12-second Ethereum slot, the visible cost is mostly timing.
- **Pre-building:** build t+10 and the sync block early (at t+8) so the prover finishes before the L1 block. The empty blocks at the end of the slot disappear; the pre-built ones run on mildly stale data. This is the same pipelining Jordi describes for proof generation.

## Close
- Roadmap: from a forum post to Gnosis Chain on the EEZ. Closing line: "One zone. One UX."

## Notes for the corpus
- This validates the corrected multi-prover framing: a "proof system" can be a BLS multisig (validator attestation), not only a zk system, and the M-of-N threshold is the chain's choice. Do not say EEZ requires zk or a minimum of two zk provers.
- Mainnet is not live. The prototype is on Chiado. Frame Gnosis-on-EEZ as roadmap.

---

*Filed 2026-06-19. Companion to `dappcon-2026-eez-node-architecture.md` and `dappcon-2026-realtime-proving-talk.md`.*
