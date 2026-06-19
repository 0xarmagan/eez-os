# EEZ Core Protocol: Spec Diagrams

Visual maps of the EEZ core protocol contracts, one per spec doc in `eez-association/eez-core-protocol/docs`. Open the `.excalidraw` files in Excalidraw to edit. The `.png` is the rendered view.

| Diagram | Covers |
|---|---|
| `core-protocol` | L1 cross-chain execution model: the postAndVerifyBatch flow, consume and execute, integrity invariants |
| `multi-prover` | Per-rollup-manager model, two-stage public inputs, threshold semantics, trust model, what was removed |
| `execution-entry` | ExecutionEntry anatomy (L1 vs L2), the flat-call partition, which structure to use for which call |
| `lookup` | Nested vs top-level lookups, static and failed modes, the three resolution paths, match keys |
| `caveats` | Four edge cases: one deterministic consumption order, transient self-containment, indistinguishable revert reasons, proxy opcode differences |

Source specs: CORE_PROTOCOL_SPEC, MULTI_PROVER_SPEC, EXECUTION_ENTRY_SPEC, LOOKUP_SPEC, CAVEATS.

Status: the core-protocol docs are living and pre-final (branch `feature/lookUP`, post-`feature/flatten`). Verify against the repo before any external use. Multi-prover is capable, not a fixed minimum; synchronous cross-rollup composability is bounded to rollups co-verified in one batch.
