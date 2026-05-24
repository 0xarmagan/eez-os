# Vitalik's Questions on Synchronous Composability → EEZ Responses

---

## Technical Design Questions

### 1. How can synchronous composability be combined with fast L2 block times?

**Vitalik's frame:** Based rollups are slow (coupled to L1 ~12s), sequenced rollups are fast (250ms+) but break composability.

**EEZ response:** Accept L1 block time coupling at launch. Speed optimizations come later via preconfirmations and hybrid locking strategies. Trade-off is explicit: atomicity now, speed later.

**Status:** ✓ **Addressed. Trade-off accepted.**

---

### 2. What's the optimal timing mechanism for sequencers to release slot-ending blocks?

**Vitalik's frame:** Sequencers must play a timing game—release slot-ending blocks late enough for builders to include them, but early enough to avoid L1 reorg risk.

**EEZ response:** Not explicitly detailed in community call. Implied: sequencers coordinate with composers/builders to submit execution tables just before L1 blocks are proposed. Composers aggregate and bundle atomically.

**Status:** ⚠️ **Partially addressed. Implementation details TBD.**

---

### 3. How should L2s handle L1 reorgs without cascading failures?

**Vitalik's frame:** If L1 reorgs, based blocks revert. Any sequenced blocks on top also revert. L2s must accept this risk.

**EEZ response:** L2s joining EEZ must be willing to revert if L1 reverts. This is a foundational constraint, not a bug. Early adopters accept L1 reorg risk for atomic composability. Preconfirmations will reduce reorg likelihood over time.

**Status:** ✓ **Addressed explicitly. Constraint is clear.**

---

### 4. Can forced-inclusion channels enable true permissionlessness in based blocks?

**Vitalik's frame:** Without forced-inclusion, based blocks require sequencer certificates, breaking permissionlessness.

**EEZ response:** EEZ doesn't require forced-inclusion at launch. Composers aggregate transactions. Permissionlessness is a later optimization. Early phase: trust Gnosis/Zisk as composer. Decentralised prover markets come after proving systems harden.

**Status:** ⚠️ **Acknowledged. Permissionlessness deferred to Phase 2.**

---

## Security & Trade-offs

### 5. How secure is a design where based blocks only work during current L1 slots?

**Vitalik's frame:** Time-windowed based blocks introduce complexity—what if the window closes unexpectedly?

**EEZ response:** EEZ doesn't use time windows. Instead: execution tables define atomic call sequences. Proofs settle on L1. No slot-specific constraints. Simpler model than Vitalik's hybrid design.

**Status:** ✓ **Different approach. Avoids the slot-window problem.**

---

### 6. What happens if L1 proposers are missing or defective?

**Vitalik's frame:** Missing proposers cause attestation delays and reorg risk.

**EEZ response:** Not explicitly addressed. Implied: Gnosis/Ethereum L1 handle proposer selection. EEZ proofs settle atomically once included. No L2-specific proposal mechanism.

**Status:** ⚠️ **Inherited from Ethereum L1. Not EEZ-specific.**

---

### 7. How tight can the coupling be between L1 and L2 finality?

**Vitalik's frame:** Tight coupling = low latency but high reorg risk. Loose coupling = safety but long delays.

**EEZ response:** L1 slot time (~12s) is the baseline. Early chains accept L1 finality as their finality. Later: preconfirmations will allow intermediate checkpoints without waiting for full L1 finality.

**Status:** ✓ **Addressed. Explicit constraint with path forward.**

---

### 8. Can L2s achieve synchronous composability without reverting on L1 reorgs?

**Vitalik's frame:** To avoid reversion, L2s would need to wait for L1 finality (~15 min). Too slow.

**EEZ response:** No. Early EEZ chains accept revert risk. Preconfirmations later will reduce reorg probability. But synchronous composability requires accepting *some* reorg risk.

**Status:** ✓ **Honest constraint. No way around it.**

---

## Operational Constraints

### 9. What's the realistic latency window for builders to include based blocks?

**Vitalik's frame:** Builders need time to construct blocks from sequencer data—how much?

**EEZ response:** EEZ proofs are generated in ~7 seconds (12 GPUs). Builders can include them alongside normal L1 blocks. Window is tight but feasible. Early adopters will see ~12s latency (L1 slot time).

**Status:** ✓ **Quantified. 7s proof generation + L1 slot time.**

---

### 10. How does the sequencer's timing game affect actual throughput vs. theoretical throughput?

**Vitalik's frame:** Sequencer timing delays reduce effective throughput.

**EEZ response:** Early EEZ uses a centralized Composer (Gnosis/Zisk). Throughput is limited by L1 slot time (~12s per batch). Multiple EEZ blocks can bundle into one L1 block, but overall throughput is L1-constrained at launch.

**Status:** ✓ **Addressed. Throughput is L1-constrained.**

---

### 11. How long is the delay in cases of missing L1 proposers?

**Vitalik's frame:** Missing proposers cause L1 attestation delays, which cascade to L2 delays.

**EEZ response:** Not specifically addressed. Inherits Ethereum L1's proposer selection and backup mechanisms. Not an EEZ-specific concern.

**Status:** ⚠️ **Inherited from L1. Out of EEZ's scope.**

---

## Protocol Design

### 12. Should based blocks require a sequencer certificate from slot-ending blocks?

**Vitalik's frame:** Certificates tie based blocks to sequencers, reducing permissionlessness but ensuring ordering.

**EEZ response:** EEZ doesn't use based blocks or sequencer certificates. Instead: execution tables + atomic proofs. Ordering is implicit in proof settlement on L1. No sequencer veto power.

**Status:** ✓ **Different design. Avoids the permission question.**

---

### 13. Can forced-inclusion channels fully replace sequencer permissions?

**Vitalik's frame:** Forced-inclusion enables permissionlessness but adds complexity.

**EEZ response:** EEZ will need forced-inclusion later, but not at launch. Phase 1: Gnosis/Zisk composes transactions. Phase 2+: decentralised. Permissionlessness is a roadmap item, not a guarantee yet.

**Status:** ⚠️ **Deferred. Roadmap includes it, but later.**

---

## Summary: EEZ vs. Vitalik's Design Space

| Dimension | Vitalik's Hybrid Model | EEZ Approach |
|-----------|----------------------|--------------|
| **Block timing** | Slot-based (regular + slot-ending + based) | Execution tables + atomic proofs |
| **Sequencer role** | Plays timing game, releases slot-ending blocks | Composers aggregate; no timing game |
| **Permissionlessness** | Requires forced-inclusion | Phase 1: centralized, Phase 2+: decentralised |
| **L1 coupling** | Tight (slot-level) | L1 block time level (~12s) |
| **Reorg handling** | L2 reverts if L1 reverts | L2 reverts if L1 reverts (same constraint) |
| **Finality assumption** | Attestation-based | L1 slot finality |
| **Launch state** | Complex hybrid design | Simpler atomic proof model |

---

## Key Divergences

**1. Where Vitalik optimises for flexibility, EEZ optimises for simplicity.**
- Vitalik: Slot-based windows, sequencer certificates, forced-inclusion channels
- EEZ: Execution tables, atomic proofs, composer aggregation

**2. Where Vitalik requires permissionlessness upfront, EEZ accepts centralized coordination initially.**
- Vitalik: Forced-inclusion as a design requirement
- EEZ: Decentralisation as a Phase 2 goal, not a Phase 1 requirement

**3. Both accept the same fundamental constraint: L1 reorg risk.**
- Vitalik: Inherent to based blocks
- EEZ: Inherent to atomic proofs settling on L1

---

## Open Questions EEZ Still Needs to Answer

1. **Versioning & breaking changes:** If EEZ protocol evolves, how do existing contracts migrate?
2. **Stale execution table cleanup:** Who removes unused execution tables, and at what cost?
3. **Prover decentralisation timeline:** When do decentralised prover markets launch?
4. **Forced-inclusion implementation:** How does EEZ add forced-inclusion without breaking atomicity?
5. **Multi-prover systems:** Can EEZ chains use different provers (SP1, Plonk, STARK) and still interoperate?

---

**Source:** 
- Vitalik's ethresearch post: "Combining preconfirmations with based rollups for synchronous composability"
- EEZ Community Call #1 with Jordi Baylina (Zisk)
- EEZ transcript summaries (Technical + Simplified versions)
