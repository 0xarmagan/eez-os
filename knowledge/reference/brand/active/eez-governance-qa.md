# EEZ — Governance Q&A (Speaker Prep)

**For:** Friederike Ernst, Jordi Baylina, Armagan Ercan
**Use:** DappCon panel prep, journalist interviews, technical community Q&A
**Status:** Working document — update when governance milestones change

---

## How to use this document

Each Q&A pair has three parts: a calibrated answer, a north star redirect, and a phrase to avoid. Use the calibrated answer as your default. Use the north star when the questioner pushes past "how it works today" toward "where it's going." The phrase-to-avoid line names the exact failure mode for each question.

---

## Q1: "How exactly is EEZ governed by Ethereum? Where is that on-chain today?"

**Calibrated answer:**
Today, governance runs through GnosisDAO. GNO holders vote on proposals that shape EEZ's direction — that is a DAO-based mechanism with a public vote record, not a company making unilateral calls. EEZ is an open-source framework — anyone can build a zone using it. There are no multisigs with upgrade authority, no admin backdoors, and no private keys that can override protocol behavior. Those are the hard structural commitments. What is not yet on-chain end-to-end is protocol parameter decisions. Full on-chain verifiability across all governance surfaces is Phase 3 work.

**North star redirect:**
The destination is a governance model where protocol parameter decisions are verifiable on-chain, with no trusted intermediary in the decision path. We are building toward that. The GnosisDAO phase is the accountable bridge, not the endpoint.

**Phrase to avoid:**
"EEZ is governed by Ethereum." Alone and unqualified, this claim outruns current state. Governance runs through a DAO on Ethereum, not through Ethereum itself as a governing body. The distinction matters to technical audiences and will be challenged immediately.

---

## Q2: "How is this different from Optimism's governance or AggLayer?"

**Calibrated answer:**
These are serious systems and the comparison is worth making precisely. Optimism's governance covers protocol upgrades and public goods funding for the OP Stack ecosystem — it is L2-specific governance over a foundation-led stack. AggLayer is a coordination layer; it does not offer shared governance across chains. EEZ is an open-source framework — anyone can build a zone using it. EEZ's governance applies to the protocol standard itself: the EIP-equivalent process that governs the EEZ spec, the technical interoperability requirements, the no-upgrade-keys commitment. What no one governs in EEZ is who gets to use it — that is open. What is not yet on-chain end-to-end is protocol parameter decisions — that is Phase 3 work.

**North star redirect:**
The north star is a world where the EEZ standard evolves through a fully on-chain community process — any builder can read the spec, implement it, propose changes, and verify what was decided. That is a different problem than what Optimism or AggLayer are solving.

**Phrase to avoid:**
"Unlike Optimism and AggLayer, EEZ actually has decentralized governance." This is defensive and overstated. EEZ's governance is still maturing. Engage technically: explain what EEZ governance is governing, not that it is better than others.

---

## Q3: "Who decides which zones are legitimate EEZ zones?"

**Calibrated answer:**
Anyone. EEZ is an open-source framework. Any team can build a zone that implements the EEZ technical standard — there is no application, no committee, no approval process. What EEZ governs is the standard itself: the technical interoperability requirements, the no-upgrade-keys commitment, the composability protocol. A zone is a legitimate EEZ zone if it implements the standard. The EEZ Alliance is a separate, voluntary layer — teams who build on EEZ and want to participate in the broader ecosystem coordination join it. But that is opt-in, not a prerequisite for using the framework.

**North star redirect:**
The standard itself evolves through an EIP-equivalent community process — open to input from anyone building with EEZ. That is the governance surface: the spec, not the permission to use it.

**Phrase to avoid:**
"You need to apply to join EEZ." There is no application to use an open-source framework. The alliance mark is a separate voluntary signal. Do not conflate the two — it misrepresents EEZ as a permissioned system when it is not.

---

## Q4: "What happens if Gnosis decides to change direction or withdraw support?"

**Calibrated answer:**
Gnosis co-initiated EEZ and remains a major contributor. Gnosis does not own EEZ. The protocol is fully open source. The EEZ Swiss non-profit foundation holds the brand and governance structure independent of Gnosis Ltd. If Gnosis withdrew tomorrow, the codebase, the alliance relationships, and the governance structure would remain. Other contributors could continue. This is the same resilience property you would expect from any credibly neutral infrastructure project. The co-funding from the Ethereum Foundation is also independent of Gnosis's ongoing participation.

**North star redirect:**
The goal is a governance structure where no single contributor, including Gnosis, can unilaterally determine EEZ's direction or shut it down. The foundation structure and the open-source codebase are the current guarantees. On-chain governance is the longer-term one.

**Phrase to avoid:**
"Gnosis is just one participant." While technically accurate, said too quickly it sounds like a deflection from the real answer. Acknowledge the co-initiation clearly, then explain the structural separation. The questioner deserves a direct answer, not a minimization.

---

## Q5: "Is there a token? Will there be a token?"

**Calibrated answer:**
No token. GnosisDAO uses GNO, and EEZ governance flows through GnosisDAO during this phase. There is no EEZ-specific token, no points program, no airdrop planned. The model is a Swiss non-profit foundation with Ethereum Foundation co-funding. That is the structural commitment. This was a deliberate decision: a token would create a financial layer that complicates the credibly neutral governance thesis. The answer is not "not yet." The answer is no. One honest note: GnosisDAO governance currently runs through Snapshot, which is off-chain voting. Fully on-chain governance — including any future decisions about protocol parameters — is Phase 3 work. As with all governance claims, EEZ does not commit to on-chain structures it has not yet built — if token design ever becomes relevant, that decision runs through governance, not through a team announcement.

**North star redirect:**
The governance model scales through on-chain DAO mechanisms using existing infrastructure, not through a new token. If that changes, it will be a public governance decision with a public rationale — not a surprise announcement.

**Phrase to avoid:**
"We'll never say never." This phrase exists to leave a door open. There is no door to leave open here. A token would directly contradict the governance and neutrality commitments EEZ has made publicly. Giving the impression of flexibility on this point will create a persistent narrative problem that is difficult to undo.

---

## General principles for live governance questions

- Lead with what is true now, then name the north star. Do not imply the north star is already live.
- Acknowledge what is early-stage explicitly. "Full on-chain governance of protocol parameters is Phase 3 work" is not a weakness; saying it confidently is a strength.
- Engage technically, not defensively. AggLayer and Optimism governance are serious systems. Compare them on substance.
- Do not speak for Vitalik's presence at DappCon. His co-presence signals technical alignment. It is not an endorsement of every governance claim. Frame passively: "Vitalik will be there" — not "Vitalik endorses this."
- When uncertain, say so and commit to follow-up. "I don't have that number with me — I'll get it to you after the panel" is a better answer than a confident approximation that turns out to be wrong.
