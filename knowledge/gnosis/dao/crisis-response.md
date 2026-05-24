# Crisis Response Framework — GIP (in flight)

Co-authored by Sebastian Bürgel, Rich (HOPR), and Ben Carvill. Status: drafting v2 with public roundtable input from 26 March 2026.

## Purpose

A framework for how GnosisDAO responds to security incidents, exploits, and crises affecting protocols on Gnosis Chain or in the Gnosis ecosystem. Replaces ad-hoc response with a documented process.

## v2 design principles

- **Hierarchy of intervention** — six levels from light-touch to network-wide:
  1. Account-level
  2. Module-level
  3. Protocol-level
  4. Asset-level
  5. State change
  6. Network scope

- **Minimum necessary intervention** — always the lowest level that resolves the issue.

- **Impartiality safeguards** — protocol-blind scoring, mandatory conflict of interest disclosure, recusal for Gnosis-affiliated parties, permanent public audit trail. Includes an explicit impartiality check that flags Gnosis Pay / Balancer relationship in any retrospective.

- **Prevention and systemic risk reduction** — TVL/validator concentration monitoring (flagging StakeWise >33% issue), security culture requirements (audit standards, bug bounties, 7-day disclosure SLA), and a technical path to obsolescence via Shutter / encrypted mempools with concrete milestones.

- **Multi-channel communication** — forum, Discord, Telegram, X all mandatory for high-importance notifications. Forum is canonical.

## Reference incident

The Balancer hack response is the working test case for v2. Retrospective applied to the framework checks whether the new graduated model would have produced a better outcome.

## Status

- v1 draft posted, gathered initial feedback (8 posts from e3o8o, mrtdlgc, TheVoidFreak)
- v2 draft incorporating community input (graduated intervention, impartiality, prevention)
- Public roundtable held 26 March 2026 with Sebastian Bürgel and Rich
- Next: phase-2 temperature check after final v2 draft is posted

## Internal notes (internal persona only)

Co-authoring with Sebastian and Rich is well-coordinated. The key open item is how to formalise the prevention layer without it reading as "Gnosis Ltd dictating standards to ecosystem teams". Framing matters here.

## Related files

- `data/gips.yaml` — `in_flight.crisis_response`
- `knowledge/gnosis/dao/governance-hub.md`
- `skills/gip-authoring/SKILL.md`
