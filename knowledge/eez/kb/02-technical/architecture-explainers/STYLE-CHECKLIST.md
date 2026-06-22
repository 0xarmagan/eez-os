# Reviewer Style Checklist

Internal QA for the explainer series. **Not reader-facing.** This is the checklist a reviewer runs a draft against before publishing. It replaces the per-document "Accuracy notes" sections that used to ship inside the explainers. Run every box against each draft.

## Prose & style

- [ ] **No em dashes.** Use a comma, parentheses, or a full stop. House style. A full stop (two short sentences) is usually the cleanest fix.
- [ ] **Active voice, one idea per sentence.** Break run-ons. Vary sentence length and openings so paragraphs don't fall into a single rhythm.
- [ ] **No mechanical patterning.** Avoid back-to-back "rule of three" triads ("no X, no Y, no Z") and "not X, but Y" splices when they read as a template rather than the clearest phrasing.
- [ ] **Plain English over jargon.** Define a term once (link the [GLOSSARY](GLOSSARY.md)); don't re-explain it. No marketing filler (no "leverage", "seamless", "powerful", "game-changer").

## The five distinctions (full statements in [Conventions & Caveats](00-conventions-and-caveats.md))

- [ ] **Async vs native finality.** No single finality number applied to EEZ. Any figure names its path (~12 s native / ~20 min async). The < 3 s figure is flagged as a proof-generation target, not finality.
- [ ] **Execution entries, not transactions.** Inside native rollups = execution entries. "Transaction" only for L1 and partner chains' own models.
- [ ] **Proxies, not bridges.** No EEZ-native mechanism called a bridge. A partner's own bridge scoped to them.
- [ ] **Proof-system agnostic, multi-prover-capable.** Each rollup sets its own threshold on its manager contract; no protocol floor of two; single `prover` box flagged as topology abstraction; no "the EEZ prover" singular framing. Don't conflate `ThresholdNotMet` (threshold check) with `InvalidProofSystemConfig()` (registry-side structural error).
- [ ] **Economic zone, not an L2.** EEZ never called an L2. Native rollups are an L2-style construction EEZ builds on.

## Status & sourcing

- [ ] **Not deployed.** No "join today" / "run a composer today" claim. Participation framed as roadmap.
- [ ] **Source framing.** Claims attributed to Jordi Baylina's / Phillipe Schommers' DAPPCon sessions as engineering-level framing, not approved EEZ comms.
- [ ] **Gnosis connection (Explainer 8).** Pre-GIP; check current GIP status before any external comms; testnet prototype, not mainnet.

## Scope guards

- [ ] **Chain types (Explainer 4).** Three-type picture flagged as the deck's vision; shipped code scopes to based rollups sharing one L1 sequencer.
- [ ] **ADSTF (Explainer 7).** Flagged as the deck's conceptual term, not a literal code type.
- [ ] **The "one caveat."** A proxy call made outside the composer bundle reverts, which breaks specs that must not revert (e.g. ERC-20 `balanceOf`). There is no public L1 mempool for these calls, so they must route via composer / bundle / AA. Don't describe EEZ cross-chain calls as fully transparent without this caveat.
- [ ] **Nested calls are roadmap, not shipped.** The running prototype does single synchronous calls; "nested back-and-forth" is explicitly later (and excluded from the near-term Gnosis scope). Never list nested calls under "already built."

## Editorial

- [ ] Caveats are linked to [Conventions & Caveats](00-conventions-and-caveats.md), not re-stated in full.
- [ ] Defined terms link to [GLOSSARY.md](GLOSSARY.md) on first use rather than re-defining.
- [ ] Series position label is correct ("Explainer N of 8").
- [ ] In-contract function names (`checkProofSystemsAndGetVkeys`, `_fetchVkMatrix`, etc.) are in a "For builders" context, not load-bearing for partner readers.
- [ ] Comparison material (chain types, optimistic/pessimistic, the 7 properties, timing) uses tables, not paragraphs.
