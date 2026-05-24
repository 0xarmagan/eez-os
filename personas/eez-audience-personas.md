# EEZ — Target Audience Personas & Behaviour Chart
**Internal · v1.1 · April 2026 · Confidential**

---

> **What this document is:** Four evidence-based persona profiles and a cross-persona behaviour chart for EEZ's target audiences. These are pre-research hypotheses — precise enough to act on now, calibrated to update once builder interviews are complete.
>
> **Who uses it:** Lukasz and Armagan, for writing and outreach framing; Adrienne, for editorial targeting; Friederike and Jordi, for conference and keynote calibration.

---

## Quick Reference — The Four Personas

| | **Kai** | **Priya** | **Stefan** | **Marcus** |
|--|---------|-----------|-----------|-----------|
| **Archetype** | The Rollup Founder | The Protocol Architect | The Compliance Builder | The ETH Loyalist |
| **Tier** | Chain Teams | DeFi Protocol Devs | Institutional / RWA | ETH Holders / OG |
| **Side** | Supply — Primary | Demand — Primary | Beachhead | Background Narrative |
| **Opens with** | Benchmark + reference chain | Reference implementation | Governance structure | Not direct outreach |
| **Primary fear** | Reorg complexity undefined | Atomicity guarantee imprecise | Token risk / governance change | "Ethereum-aligned" without mechanism |
| **Aha moment** | "Same-block call to Uniswap on L1, no bridge" | "Cross-chain liquidation engine in one tx" | "Swiss foundation, no token, EF co-funded" | Independent EF/Vitalik citation |

---

## Persona Interaction Dynamics

These four personas are not independent. The order in which EEZ wins each one matters. Understanding the influence chain changes how to sequence outreach and content.

**Kai enables Priya.** Priya needs chains to deploy on. If EEZ has 0–1 credible chains, there's nothing for her to evaluate. Every Kai who integrates makes Priya's evaluation more viable. Win the supply side first.

**Priya validates Kai.** Kai's chain becomes more valuable when a DeFi protocol deploys on it. If Priya (or someone like her) announces a deployment citing "shared liquidity without rebuilding infrastructure," Kai's decision to join EEZ looks prescient. Demand validates supply.

**Marcus is the ambient signal.** Kai's investors and advisors follow Marcus-type accounts. If Marcus is publicly skeptical of EEZ, Kai's board asks questions Kai doesn't want to answer. If Marcus becomes a quiet advocate, those conversations get easier. EEZ doesn't target Marcus — but EEZ must not lose him.

**Stefan adds institutional credibility.** Stefan joining doesn't directly pull Kai or Priya, but it changes Marcus's Gnosis-separation concern. If a compliance-native builder has done the legal due diligence and found EEZ sound, Marcus's "is this really a public good or a Gnosis product" question gets an external answer he didn't need to take from EEZ itself.

---
---

# Persona 1 — Kai
## The Rollup Founder

> *"I just want to build the product. Everything else is infrastructure tax."*

*Note: This quote is a synthesized hypothesis — replace with verbatim interview quotes after builder research is complete.*

---

### Snapshot

**Role:** Technical co-founder or CTO of a specialized Ethereum L2 rollup — gaming, identity, privacy, or RWA vertical. Shipped something to testnet or mainnet. Not planning to — has done it.

**Team size:** 4–18 engineers. Most of them on the application layer. A disproportionate few carrying the infrastructure.

**Where they are:** Post-seed or Series A. Proven the concept. Now drowning in operational complexity they didn't anticipate.

**Segment size estimate:** ~50–200 actively building specialized rollups at the right profile globally. Not a large market — a precise one.

**Real-life archetype:** The person at DevConnect who gives the tightest 10-minute talk and then immediately corners three people with the question they couldn't ask publicly.

---


### Primary Job to Be Done

**Functional:** Access Ethereum's full liquidity and security from day one without building or maintaining a DEX, bridge, or oracle layer themselves.

**Emotional:** Stop feeling like every engineering sprint is split between the product he cares about and infrastructure nobody will credit him for.

**Social:** Be the rollup that other builders cite as an example of doing it right — not "they built everything themselves" but "they figured out the right infrastructure choices."

---

### Goals

- Ship the application product faster — reduce the infrastructure surface area the team owns
- Make their chain viable for DeFi protocols to deploy on (liquidity depth, price feeds, composability)
- Differentiate from generic RaaS chains — Caldera and Conduit give him a chain; EEZ gives him Ethereum's entire network
- Survive the window before fragmentation ossifies into entrenched incumbents

---

### Where to Find Kai

- **Conferences:** DevConnect, EthCC, zkSummit — he speaks at them. Find him in the hallways between sessions, not in the front row of panels.
- **Online:** ETH Magicians, GitHub (check who's opening cross-chain issues on rollup repos), Ethereum Research forum threads on sequencing and proving.
- **Warm intros:** Caldera, Conduit, AltLayer alumni network — teams who have used RaaS providers and are at the point of asking "what's next." Gnosis Alliance relationships are the warmest path.
- **Signals he's the right person:** He can describe a specific infrastructure problem his team solved in the last 6 months without being prompted. Generic answers mean wrong profile.
- **Outreach approach:** A direct, short ask for a technical conversation — not a sales call. "We're building something that solves the liquidity bootstrapping problem specifically. 30 minutes to walk through how it works?" He doesn't respond to pitch decks or partnership introductions.

---

### Fears & Blockers

| Fear | Specific form it takes |
|------|----------------------|
| **Reorg complexity** | EEZ chains must reorg with Ethereum. What does that mean operationally for their application layer? Who handles it? What does the UX look like for their users? |
| **Low adoption trap** | If only 3 chains are on EEZ, shared liquidity is an illusion. He needs to see at least 2–3 credible chains before committing. |
| **Lock-in** | He's already locked into a sequencer vendor he's starting to regret. He is allergic to signing another agreement with a single point of failure. |
| **Another bridge abstraction** | He's seen "trustless bridges" fail. "Atomic" sounds like another word for "eventually." He wants to understand the failure mode, not just the happy path. |
| **Credibility risk** | His investors and users are watching. If EEZ doesn't ship as described, Kai looks like he made the wrong call. |

---

### Motivations

- **Primary:** Reduce infrastructure overhead so the team can build the actual product
- **Secondary:** Get access to Ethereum-native liquidity without the cold-start problem
- **Tertiary:** Be on the right side of Ethereum's scaling story — he wants his chain to matter in 5 years, not be a footnote
- **Emotional:** The original Ethereum promise — one network, full composability — resonates. He started building because of that promise. Fragmentation feels like a betrayal.

---

### Decision Process

**Awareness:** Sees EEZ mentioned in a governance thread, a Bankless episode, or from someone he trusts in his extended network. Does not investigate immediately. Files it.

**Passive evaluation:** Starts noticing EEZ appearing in multiple credible contexts over 2–3 weeks. Spends 45 minutes reading the technical documentation and benchmark data. Forms a skeptical first hypothesis.

**Active diligence:** Reaches out to one person he trusts who's familiar with EEZ — not for a sales pitch, for a real conversation about failure modes. Reads the Zisk proving documentation. Looks at who's actually integrated and what they've said publicly.

**Proof threshold:** Needs to see at least one production chain on EEZ that is (a) comparable to his in scale and (b) has published honest data about the integration — including what was hard.

**Commit:** If the diligence passes, Kai commits fast. He's used to making bets. But he needs the diligence to pass cleanly.

**Developer pilot fit:** High. Kai is the ideal pilot program participant — early access to testnet in exchange for honest, public feedback is exactly the trade he'd make. He's incentivised to publish the results (credibility signal for his chain) and his feedback is the most operationally useful EEZ can get.

---

### Information Sources / Media Diet

**Trusted first:** Direct network — other technical founders, protocol engineers he's worked with or seen present at conferences.

**Technical depth:** ETH Magicians, Ethereum Research forum, zkSummit talks, Jordi Baylina's writing specifically.

**Social signal:** Twitter/X — follows a very specific list, mostly protocol engineers and Ethereum researchers. Does not respond to influencer content.

**Long-form:** Reads research papers and implementation reports when they're specifically relevant. Bankless for macro narrative but takes it with a grain of salt.

**Conferences:** DevConnect, EthCC, zkSummit. Speaks at them. Gives weight to people he's met in person.

**Does not read:** PR announcements, token launch coverage, "ecosystem" round-ups.

---

### Evaluation Criteria (in priority order)

1. **Does it actually work at my scale?** Devnet benchmarks with stated hardware conditions. Not marketing claims — verifiable data.
2. **What's the reorg story?** Detailed, honest answer about what happens operationally when Ethereum reorgs.
3. **Who else is on it?** Specific chains, their integration stories, their honest feedback.
4. **What's the governance structure?** Swiss foundation, no token, open source — these are positive signals. Single operator, governance token — red flags.
5. **Can I integrate without forking?** EVM, WASM, or custom — he needs to know his stack is supported.
6. **What's the exit?** If EEZ fails or pivots, what's his path out? Not looking for a guarantee — just a clear answer.

---

### Red Flags That Make Kai Walk Away

- Vague answers about reorg handling ("we handle it seamlessly" with no specifics)
- Benchmark claims without hardware conditions stated
- "Ecosystem partnerships" listed instead of working integrations
- A token on the roadmap (or "community incentives" that smell like one)
- Any answer to "what's the failure mode?" that starts with "well, in theory..."
- Being asked to sign a partnership agreement before the integration works

---

### Objection Handling — Quick Reference

| His objection | The right response |
|---------------|-------------------|
| *"Reorgs are a real operational risk for my application."* | Agree — reorgs are real. In practice they happen a couple of times a day and only for one block. Deep reorgs are vanishingly rare in Ethereum's history. EEZ chains reorg with Ethereum if it does. Walk through what that means operationally for his specific application layer. Don't deflect. |
| *"There are only 2 chains on EEZ — the network isn't there yet."* | He's right about where we are. That's exactly why the window to be a founding chain matters — the second revolution of the flywheel costs less than the first. Show him who else is in the pipeline, honestly. |
| *"I got burned by a bridge abstraction before. What makes atomic different?"* | Name the failure mode directly: "If the proving pipeline fails, here's what happens." Give the specific answer before he asks a second time. |
| *"I'm worried about being locked in again."* | Open source. No token. Swiss foundation with governance rights that don't sit with a single operator. Walk through the exit path specifically — if EEZ disappears, what does he own and what can he take with him. |

---

### How EEZ Loses Kai

1. Kai makes a specific technical claim about EEZ to a peer — reorg handling, proving speed — and later finds it was imprecise or overstated.
2. He doesn't raise it publicly. He just stops responding.
3. When the peer network asks him "what happened with EEZ?", his answer becomes a reference point for the next 5 Kais evaluating.
4. The damage is invisible to EEZ and irreversible. He doesn't write a critical thread — he just goes quiet and his silence is the signal.

**Prevention:** Structural honesty first. Name the limitations before he finds them.

---

### The EEZ Aha Moment

*"A contract on my chain can call a Uniswap pool on Ethereum mainnet and read the result — in the same transaction. Not via a bridge. Not async. Same block."*

When Kai truly internalises this, the product implications become immediately visible: liquidity depth from day one, no AMM to seed, no bridge to bootstrap. The thing that's been blocking his application roadmap has an answer.

---

### What EEZ Says to Kai First

> *"EEZ gives your chain access to Ethereum's full liquidity and security from day one — without building a DEX, a bridge, or an oracle layer yourself."*

**Followed by:** A specific benchmark number, a named reference chain with an honest integration report, and a clear explanation of what reorg handling requires operationally.

---

### Content That Resonates

- Deployment walkthrough written by the engineer who did the integration (honest, including rough parts)
- "We were wrong about X" posts that update earlier claims with current data
- Zisk proving benchmarks with specific hardware and conditions
- Technical posts that name the failure mode before making the positive claim
- Governance documentation: Swiss foundation, no token, open source — explained, not just stated

### Content That Repels

- Launch announcements without specific technical data
- "Thrilled to welcome X to the Alliance" posts with no substance
- Anything that sounds like a product pitch before the primitive is live
- Comparison posts against Polygon or Optimism ("EEZ is better than X")
- Social media presence driven by followers rather than technical depth

---


# Persona 2 — Priya
## The Protocol Architect

> *"If I could execute one transaction that atomically touches two chains, I would build something that doesn't exist yet."*

*Note: This quote is a synthesized hypothesis — replace with verbatim interview quotes after builder research is complete.*

---

### Snapshot

**Role:** Smart contract engineer, protocol architect, or technical co-founder at a DeFi protocol — DEX, lending, derivatives, or structured products. Has personally written or reviewed contracts deployed on more than two chains.

**Team size:** Engineering team of 5–25. Priya is not the founder, usually — she's the person the founder trusts to make the call on technical infrastructure.

**Where they are:** Protocol is live, profitable, multi-chain. The cross-chain deployment cost is now visible in the P&L and in team morale. She's been asked to "expand to X chain" and has had to explain, again, what that actually involves.

**Segment size estimate:** ~500–2,000 protocol engineers globally with the right profile — multi-chain deployment experience, architectural decision-making authority. A broader market than Kai, but harder to reach through broadcast channels.

**Real-life archetype:** The person who writes a 40-tweet technical thread at 11pm because something in the Ethereum research forum annoyed her. Gets cited by people with 10x her follower count.

---


### Primary Job to Be Done

**Functional:** Execute a transaction that atomically touches her protocol on two chains simultaneously — in the same block, without a bridge in the path.

**Emotional:** Stop feeling like the most interesting product ideas are impossible by definition.

**Social:** Be known as the protocol that figured out cross-chain composability before everyone else had to.

---

### Goals

- Build product features that are technically impossible without synchronous composability
- Reduce multi-chain maintenance burden (one shared liquidity pool > four separate ones)
- Get competitive differentiation through a product surface that competitors can't replicate yet
- Maintain existing contract architecture — she doesn't want to rebuild; she wants to extend

---

### Where to Find Priya

- **GitHub:** Look at contributors to cross-chain contract repositories, authors of bridge security post-mortems, developers active on multi-chain deployment discussions.
- **Governance forums:** Uniswap, Compound, Morpho, Euler, Pendle governance — specifically the technical discussions, not the voting threads. She's the one asking the hard engineering question in a proposal discussion.
- **Conferences:** Devcon and EthCC technical tracks (not the ecosystem stages). zkSummit. She usually presents her own work.
- **Twitter/X:** She's findable — but her handle is less important than being found in the right thread. Reply to a technical thread she's already in with something worth reading.
- **Outreach approach:** Do not send a sales email. Send a technical question or a short message referencing something specific she's published. "I read your post on cross-chain state consistency — we're working on something that changes the constraint you described. Worth a technical conversation?" She responds to specificity, not flattery.

---

### Fears & Blockers

| Fear | Specific form it takes |
|------|----------------------|
| **Infrastructure that underperforms at scale** | EEZ looks great at devnet. What happens at 10x her current volume? She needs production-scale benchmarks before she touches this. |
| **Low chain adoption** | If only two chains are on EEZ, the liquidity pool is meaningless. The network value grows with adoption — she needs to bet on where adoption is heading, not where it is today. |
| **Another failed bridge abstraction** | She has a scar from a bridge that failed under load. "Atomic" is a word she's seen used loosely. She will probe the guarantee until she understands exactly what breaks when. |
| **Governance risk** | If EEZ's governance changes the atomicity guarantee after she's built on it, she's exposed. She needs to understand who can change the rules and under what conditions. |
| **Opportunity cost** | Integrating with EEZ is a 4–8 week engineering investment. If it doesn't pan out, she's lost a sprint at the worst possible time. |

---

### Motivations

- **Primary:** Build the product features that are currently on her "impossible" list
- **Secondary:** Reduce multi-chain maintenance cost — the status quo is expensive
- **Tertiary:** Competitive moat — if she's early to EEZ and it compounds, she has a head start competitors can't close easily
- **Emotional:** She finds the technical problem genuinely interesting. Same-block cross-chain atomicity is one of the few unsolved problems at her level of the stack.

---

### Decision Process

**Trigger:** She sees EEZ mentioned in a governance forum post or in a technical thread she was already reading for unrelated reasons. Does not react. Notes it.

**Technical read:** Opens the GitHub repository before reading any documentation. Reads the spec critically. Forms the 3–5 hard questions the spec doesn't answer. Looks for a published answer to those questions somewhere.

**Proof threshold:** If she can't find published answers, she reaches out directly to someone at EEZ with technical credentials — Jordi or the protocol team, not comms. Her message is short and specific: "I have one question about how [X] works in [failure scenario]."

**Reference check:** Looks at what protocols are already using EEZ. Reads their own technical posts about the integration, not EEZ's announcement about them.

**Internal advocacy:** If she's convinced, she has to convince a co-founder or protocol committee. She needs the technical case to be tight enough to present cleanly to someone who hasn't read the spec.

**Time to decision:** 4–10 weeks from serious investigation to internal advocacy.

**Developer pilot fit:** High. Priya is ideal for the pilot program if there are EEZ chains she can actually deploy against. She's motivated by early technical access and public credibility — publishing honest results from the pilot is a positive for her reputation, not a risk.

---

### Information Sources / Media Diet

**Primary:** GitHub. She looks at the EEZ codebase before she reads any marketing material. Open source means she can verify claims directly.

**Technical depth:** Ethereum Research forum, ETH Magicians, Jordi's writing, Bankless Research, Blockworks Research deep dives.

**Social signal:** Twitter/X — follows a curated list of protocol engineers. Trusts technical content from named engineers more than anonymous accounts or project accounts.

**Long-form:** Research papers on ZK proving, bridge security, sequencer design. Reads implementation reports when a specific technical claim needs verification.

**Conferences:** Devcon, EthCC technical tracks, zkSummit. Presents her own work. Builds technical relationships in person.

**Does not engage with:** Marketing content, partnership announcements, "ecosystem" narratives.

---

### Evaluation Criteria (in priority order)

1. **Is the atomicity guarantee real?** She will probe the exact boundary of the guarantee — what breaks, when, under what conditions.
2. **What are the production-scale benchmarks?** Devnet data with hardware conditions stated. She will reproduce the benchmark if she can.
3. **Can I extend my existing contracts without a full rebuild?** She is not migrating. She is extending.
4. **Who has actually shipped something on EEZ?** A reference implementation she can read, not a case study written by EEZ.
5. **What's the governance model?** Who can change the atomicity rules, and how much notice do they have to give?
6. **What does the failure mode look like in practice?** Not theoretical — she wants to know what a real failure looks like and how it's handled.

---

### Red Flags That Make Priya Walk Away

- "Trustless" used without a precise definition of the trust model
- Benchmark data without hardware conditions and scale parameters
- A whitepaper that explains the happy path but not the failure path
- Being connected to a "partnership team" when she asked a technical question
- "We handle that in the smart contract layer" — she is the smart contract layer
- Any hint of a future token that would misalign incentives in the sequencer layer

---

### Objection Handling — Quick Reference

| Her objection | The right response |
|---------------|-------------------|
| *"What does 'atomic' actually mean here — I've seen that word misused."* | Give the precise definition: "In the same block, the same transaction can read state from both the EEZ chain and Ethereum mainnet. If either fails, both revert. That's the guarantee. Here's the failure path when it doesn't hold." Be exact. |
| *"Devnet benchmarks don't tell me what happens at my volume."* | Acknowledge it directly. State what the current data shows and what it doesn't cover. Don't oversell. Offer to run a benchmark together if testnet is live. |
| *"If I build on this and EEZ changes the atomicity rules in 6 months, I'm exposed."* | Walk through the governance structure: Swiss foundation, no token, who holds what rights, what would have to happen for the atomicity guarantee to change. The answer is structural, not a promise. |
| *"I don't want to rebuild my contracts."* | Address this immediately: "You don't rebuild — you extend. Here's the integration surface. Here's what changes on your end and what stays the same." Show the diff, not the description. |

---

### How EEZ Loses Priya

1. She reads the spec and finds an ambiguity in the atomicity guarantee that wasn't disclosed.
2. She reaches out to ask a technical question. She gets routed to a non-technical person who gives a reassuring non-answer.
3. She closes the tab. She doesn't write a critical post — she just never comes back.
4. When asked about EEZ six months later by a colleague, she says "I looked at it — the spec had an open question I couldn't get answered. I moved on."
5. That answer circulates in the protocol engineer community and she never wrote a single critical word.

**Prevention:** Technical questions go to technical people. Ambiguities in the spec are closed or labelled openly as open questions — never left to be discovered.

---

### The EEZ Aha Moment

*"Your contracts can call L1 contracts atomically in the same block — so you can build a cross-chain liquidation engine that touches a Uniswap pool on mainnet and your lending market on an EEZ chain in one transaction. Or a cross-chain vault strategy that reads collateral state from two chains before executing. Both currently impossible. Both possible with EEZ."*

When Priya gets a concrete product example she's actually tried to build and couldn't, the abstract technical claim becomes a real product decision. The conversation shifts from "does this work" to "what would I build first."

---

### What EEZ Says to Priya First

> *"EEZ lets your contracts call Ethereum mainnet atomically from any EEZ chain — in the same transaction, in the same block, with no bridge in the path."*

**Followed by:** A specific reference implementation she can read, benchmark data with full hardware and scale conditions, and an honest answer to "what breaks if the proving pipeline fails."

---

### Content That Resonates

- Reference implementation with commented code
- Honest benchmark reports with methodology stated (hardware, scale, edge cases)
- Technical posts written by Jordi or other protocol engineers that explain failure modes, not just success paths
- "We updated the spec because X was wrong" — this builds trust, not loses it
- Governance documentation explaining who can change the atomicity rules and how

### Content That Repels

- Partnership announcements without technical substance
- "We're excited to partner with X" — she will immediately ask "but what does that mean technically"
- Any claim that isn't verifiable in the codebase or benchmark data
- Long-form content written in marketing language rather than engineering language
- Posts that say "trustless" without defining the trust model

---

# Persona 3 — Stefan
## The Compliance Builder

> *"I can build on anything that I can explain to a regulator, a custodian, and a board in the same sentence."*

---

### Snapshot

**Role:** CTO, Head of Product, or technical co-founder at a company building compliance-heavy products on Ethereum — RWA tokenization, regulated stablecoin infrastructure, on-chain identity, or institutional DeFi.

**Team size:** 10–60 total, 4–15 engineers. The compliance team is as large as the engineering team. Stefan sits at the intersection of both.

**Where they are:** Past the whitepaper stage. Has dealt with a regulator, negotiated with a custodian, or presented to an institutional counterparty's risk committee. Has personal experience with what "blockchain infrastructure" means to people outside crypto.

**Segment size estimate:** ~20–100 compliance-native builder teams globally at the right maturity. A small, high-value market. Stefan knows most of his peers personally.

**Real-life archetype:** The person at EthCC who asks "but what does settlement finality mean under MiCA?" and actually wants the answer, not a deflection.


---

### Primary Job to Be Done

**Functional:** A settlement layer he can present to regulated counterparties — custodians, institutional investors, payment processors — that they can approve without extensive legal carve-outs.

**Emotional:** Stop code-switching between "crypto person" and "regulated institution person" — build a product that doesn't require the user to understand which world they're in.

**Social:** Be the company that institutional players cite when they describe how to build compliant DeFi correctly.

---

### Goals

- Build regulated products with Ethereum's security guarantees without the governance risks of most L2s
- Present infrastructure choices to compliance teams, custodians, and institutional clients in language they can evaluate
- Operate in a legal structure that doesn't create regulatory ambiguity for his product
- Have a settlement layer that won't change its governance model unfavourably after he's built on it

---

### Where to Find Stefan

- **Conferences:** EthCC RWA track, RWA Summit, Permissionless, and — crucially — the side conversations at these events, not the panels. He's more likely to be in a quiet coffee conversation than on stage.
- **Online:** Centrifuge, Monerium, Backed, Ondo community channels; ERC-3643 (T-REX) and ERC-1400 governance forums; LinkedIn more than Twitter for this persona.
- **Warm intros:** Existing Alliance members who operate in the RWA or institutional space are the strongest path. Centrifuge is the named reference. Legal advisors and consultants who work across institutional DeFi clients know multiple Stefans simultaneously.
- **Outreach approach:** A peer reference is worth 10x a cold message. If you can get an existing compliance-native builder to introduce you, do that first. Cold outreach should open with the governance structure, not the technology: "EEZ is governed by a Swiss non-profit, no token, EF co-funded — worth 30 minutes to walk through what that means for regulated product teams?"

---

### Fears & Blockers

| Fear | Specific form it takes |
|------|----------------------|
| **Regulatory ambiguity** | If EEZ's governance can change in ways that affect settlement finality, his product's compliance posture is exposed. He needs to understand what's fixed vs. what's variable in EEZ's operating rules. |
| **Protocol governance risk** | A DAO vote could change sequencer economics, fee structures, or dispute resolution. That's not acceptable for a regulated product. |
| **Credibility risk** | His institutional counterparties will Google EEZ. If they find anything that looks like a token play, an influencer partnership, or a "community incentive" program, they'll ask questions he doesn't want to answer. |
| **Legal structure uncertainty** | "Swiss foundation" is a good answer. But he needs to understand the actual legal structure — who can change the foundation's mandate, what the liability structure is, what happens if a key contributor leaves. |
| **Association with unproven stack** | If EEZ fails or is compromised six months after he builds on it, his product takes the reputational hit. He needs a maturity signal — mainnet track record, audit history, published security posture. |

---

### Motivations

- **Primary:** A settlement layer his compliance team can approve and his institutional clients can trust
- **Secondary:** Ethereum-native finality without the political risk of most rollup ecosystems
- **Tertiary:** The "no token" and Swiss foundation structure are positive differentiators with institutional clients — he can use them as selling points
- **Emotional:** He believes in building infrastructure that lasts. Most crypto projects are built to exit. EEZ's structure suggests it's built to endure.

---

### Decision Process

**Trigger:** Hears about EEZ from a peer — another compliance-native builder or a consultant working across institutional DeFi. Does not investigate immediately.

**Structural screen:** First thing he looks at is not the technology — it's the governance structure. Swiss foundation, no token, EF co-funding. If any of those are wrong, he stops. If they're right, he continues.

**Legal due diligence:** Looks for the foundation's legal documentation, the open-source license, any public statements about governance rights and who holds them. This is a parallel process to technical evaluation, not subsequent to it.

**Technical evaluation:** Evaluates the proving architecture and settlement guarantee with a specific question: "What does Ethereum security mean in practice for my settlement use case, and what are the carve-outs?"

**Reference check:** Looks for other regulated or compliance-native builders who have used or evaluated EEZ. Specifically interested in teams that have presented EEZ to institutional counterparties — what questions came up, how were they answered.

**Counterparty presentation:** If he proceeds, he will present EEZ to at least one institutional counterparty before committing. His decision depends partly on how they react.

**Time to decision:** 8–20 weeks. The longest decision cycle of any EEZ persona. Stefan is not slow — the process requires external approval he doesn't control.

**Developer pilot fit:** Conditional. Stefan will not join a pilot without at least Phase 2 due diligence complete. If he's past the structural screen and wants to test the technical integration, pilot access is exactly what he needs — but the framing matters. Call it a "compliance evaluation track," not a developer pilot.

---

### Information Sources / Media Diet

**Trusted first:** Direct network of compliance-native builders, institutional DeFi operators, and legal advisors who work across both worlds.

**Technical depth:** Reads technical documentation with legal precision — not looking for how it works, but for what the guarantees are and what they exclude.

**Long-form:** Centrifuge, Monerium, and similar teams' public writing. Protocol documentation. Legal and governance publications more than technical research.

**Social signal:** Very low engagement with social media. If he reads it, it's to understand how the project is perceived — not to learn about the technology.

**Conferences:** EthCC's RWA and institutional tracks, RWA Summit, Permissionless. Values one-on-one conversations over panels.

**Does not engage with:** Token launch coverage, "ecosystem" announcements, deep ZK technical content that requires ZK expertise he doesn't have, anything that looks like retail marketing.

---

### Evaluation Criteria (in priority order)

1. **Legal structure:** Swiss non-profit foundation — documented, not just stated. Who holds governance rights? What can the foundation change unilaterally?
2. **No token:** This is non-negotiable for some of his institutional counterparties. He will ask whether a token is on any roadmap.
3. **EF co-funding:** Not just as a credibility signal — as evidence that the Ethereum Foundation has reviewed the technical architecture.
4. **Settlement finality:** What is the actual finality guarantee? How does it relate to Ethereum's slot finality? What are the edge cases?
5. **Audit history:** Has the smart contract stack been audited? By whom? What was found and how was it addressed?
6. **Governance stability:** Who can change the protocol's operating rules? What is the process? What notice is required?

---

### Red Flags That Make Stefan Walk Away

- Any hint of a token, points program, or "community incentive" structure
- "Decentralized governance" that means a token vote can change sequencer economics
- Foundation documentation that's vague about who holds what rights
- Benchmark or technical claims that haven't been independently verified
- A comms posture that prioritizes hype over precision — if the marketing is overclaiming, what else is?
- "We'll figure out the legal structure as we grow" — he has heard this before and knows how it ends

---

### Objection Handling — Quick Reference

| His objection | The right response |
|---------------|-------------------|
| *"What happens if the foundation's governance changes after I've built on this?"* | Walk through what's structurally fixed: Swiss foundation charter, no token, open-source codebase he could fork. The governance rights are documented — share the documentation. Don't give a reassurance. Give the structure. |
| *"Is there any chance of a token in the future?"* | The answer is the answer: "EEZ has no token. The Swiss foundation structure and EF co-funding are the model." No hedging. No "we'll see." If he hears hedging he walks. |
| *"My counterparty is going to Google EEZ. What are they going to find?"* | Walk him through what's public, what it looks like, and what questions it will generate. Prepare him for the review, don't just tell him it will go fine. |
| *"The stack isn't audited yet — that's a blocker for us."* | This is a fair objection and there's no workaround. Agree on what the audit timeline is. Offer to share the audit report as soon as it's available, and if a pre-mainnet evaluation track makes sense for his team. |

---

### How EEZ Loses Stefan

1. Stefan presents EEZ to an institutional counterparty as part of his evaluation.
2. The counterparty finds something in EEZ's public communications that looks like retail token marketing or "community incentives."
3. The counterparty declines. Stefan moves on without making a public statement.
4. He tells two other compliance-native builders privately: "We looked at EEZ. The governance was fine but the comms posture raised flags for our compliance team."
5. EEZ never knows this conversation happened.

**Prevention:** EEZ's public communications must be defensible in a compliance review. Every piece of content that could be found by an institutional counterparty goes through the brand governance check before publishing.

---

### The EEZ Aha Moment

*"EEZ is governed by a Swiss non-profit, has no token, and is co-funded by the Ethereum Foundation — so when my risk committee asks 'who controls this,' there's a real answer."*

The governance structure is the aha moment for Stefan. The technology is secondary. What he needs is a settlement layer whose governance he can explain in one sentence to a person who has never heard of Ethereum — and that sentence is honest.

---

### What EEZ Says to Stefan First

> *"EEZ is governed by a Swiss non-profit foundation, has no token, and is co-funded by the Ethereum Foundation — the structural conditions that make it safe to build regulated products on."*

**Followed by:** The foundation's legal documentation, the open-source license and audit history, and a direct reference to at least one builder who has presented EEZ-based infrastructure to institutional counterparties.

---

### Content That Resonates

- Governance documentation: legal structure, who holds rights, what can change
- Audit reports and their findings — not just "audited by X" but what was found and fixed
- Case studies from compliance-native builders, including what questions came up in institutional due diligence
- "No token — and here's why structurally that matters" explainer
- Precise, lawyerly language — not hype, not vague inspiration

### Content That Repels

- "Community incentives" of any kind
- "Decentralized governance" presented as a feature without explaining the governance mechanism
- Speculative roadmap language ("we plan to," "we're exploring")
- Influencer partnerships or sponsored content
- Deep ZK technical content that requires ZK expertise to evaluate — he is not a ZK researcher
- Any social media presence that looks like retail token marketing

---

# Persona 4 — Marcus
## The ETH Loyalist

> *"I remember when all of DeFi was on one chain and it was incredible. I want that back, but better."*

*Note: This quote is a synthesized hypothesis — replace with verbatim interview quotes after builder research is complete.*

---

### Snapshot

**Role:** Long-time ETH holder, validator, protocol contributor, or OG builder. Has been in the Ethereum ecosystem since 2017–2020. May run a validator, may have contributed to early DeFi protocols, definitely has opinions about what went wrong and when.

**Where they are:** Not building a new chain. Not running a DeFi protocol. But still paying close attention — reading governance forums, attending DevConnect, following Vitalik and the EF closely.

**Segment size estimate:** ~10,000–50,000 engaged ETH holders and OG community members globally. But only ~100–500 are influential enough to matter for EEZ's narrative — the ones whose public statements are read and cited by Kai and Priya.

**Important note:** Marcus is the background narrative audience — not the primary operational target. EEZ does not optimize copy for him. But the narrative must hold for him. If it doesn't, he will say so publicly, and the people EEZ is actually trying to reach will notice.

**Real-life archetype:** The person who quote-tweets an EEZ post with a long technical correction, and whose correction turns out to be partially right and worth engaging with seriously.

---

### Primary Job to Be Done

**Functional:** Understand whether EEZ actually re-aligns L2 economics with ETH value accrual — or whether it's another "Ethereum-aligned" marketing claim from a project that extracts value from the ecosystem.

**Emotional:** Find a reason to believe the original Ethereum vision is recoverable, not just nostalgia.

**Social:** Be the person in his community who correctly identified which scaling approach was actually right, before it became obvious.

---

### Goals

- Validate that EEZ is substantively different from every other "Ethereum alignment" claim
- Understand the specific mechanism by which L2 activity returns economic value to L1
- Find something to believe in that's backed by the engineering and governance credentials he respects

---

### Where to Find Marcus

- **Online:** Ethereum Research forum, ETH Magicians, protocol governance discussions on Snapshot and Agora. He posts under his own name — his identity is his credibility.
- **Twitter/X:** He's visible and findable. But cold-following him or engaging with his posts from a project account without substance is noticed and noted negatively.
- **Conferences:** DevConnect, EthCC. He attends the academic and research sessions. He talks to Ethereum researchers and EF contributors.
- **Outreach approach:** Do not reach out directly to convert him. This is not a persona EEZ solicits. If EEZ is doing its job well — publishing technically credible content, being cited by Jordi and the EF team, handling public criticism well — Marcus finds EEZ on his own. The only exception: if Marcus writes a public critique that contains a factual error, address it directly, on the same channel, with the technical substance. He respects being corrected. He does not respect being ignored.

---

### Fears & Blockers

| Fear | Specific form it takes |
|------|----------------------|
| **Another "Ethereum-aligned" claim that doesn't hold** | He's seen this movie. He will not accept the claim at face value. |
| **L2s that capture value without returning it** | His core concern is economic — if EEZ chains capture sequencer fees without returning value to L1, the "alignment" is marketing. |
| **Gnosis association** | He respects the Gnosis team's shipping record. But he's aware that "Gnosis project" and "Ethereum public good" can be framed either way. He wants to know which it actually is. |
| **Vaporware at the critical window** | The window to define Ethereum's scaling architecture is narrow. If EEZ doesn't ship before fragmentation ossifies, the project was too late regardless of how good it was. |

---

### Motivations

- **Primary:** Intellectual — he wants to understand whether synchronous composability at scale is actually possible
- **Secondary:** Financial — he holds ETH; if EEZ re-aligns L2 economics with ETH value accrual, that's materially good for him
- **Tertiary:** Ideological — he wants the original Ethereum vision to survive

---

### Decision Process

Marcus doesn't make a "decision" in the same way Kai or Priya does. His journey is one of conviction formation:

**Skeptical encounter:** Reads an EEZ post or sees EEZ mentioned in a thread he respects. Initial reaction: skepticism. Files it as "another cross-chain composability claim."

**Technical probe:** If something makes him curious enough, he reads the actual technical documentation. Looks specifically for: the economic model (how does value flow to L1?), the governance structure (who's really in control?), the proof that this is different from existing solutions.

**Conviction formation:** If the technical substance holds up, he writes about it — a thread, a forum post, a reply. This is his way of testing the idea publicly and seeing who pushes back.

**Advocacy:** If no one has successfully challenged his growing conviction, he becomes a quiet advocate — not a cheerleader, but the person who says in a group conversation "EEZ is actually worth paying attention to."

**What EEZ should not do:** Try to convert Marcus through marketing. He converts through technical substance and peer validation.

---

### Information Sources / Media Diet

**Primary:** Ethereum Research forum, ETH Magicians, Vitalik's writing. He reads primary sources, not summaries.

**Technical depth:** Reads implementation specs, governance proposals, and technical threads. Has strong opinions about ZK proving system design.

**Social signal:** Twitter/X, but only follows a very specific list. Trusts Jordi Baylina, Vitalik, protocol engineers he's interacted with directly.

**Long-form:** Bankless podcast (skeptically), Unchained, and any long-form interview with engineers he respects.

**Does not engage with:** Marketing content of any kind. He finds it actively off-putting.

---

### Evaluation Criteria (in priority order)

1. **Is "Ethereum alignment" structural or rhetorical?** He looks for governance mechanisms, not claims.
2. **Does L2 activity actually return value to L1?** The economic model — specifically sequencer fee distribution and ETH as gas token — must hold.
3. **Is Gnosis separation real?** Swiss foundation, not a Gnosis product. He'll verify this.
4. **Does Vitalik or the EF treat it seriously?** EF co-funding is a strong signal. Vitalik mentioning it independently is a stronger one.
5. **Is the technical claim falsifiable?** If EEZ makes a specific proving speed claim, Marcus will look for a way to falsify it. A claim that can't be falsified isn't one.

---

### Red Flags That Make Marcus Disengage Publicly

- Gnosis branding that blurs the separation (he'll point it out and it'll stick)
- "Ethereum-aligned" marketing without a specific structural explanation of the mechanism
- Engagement-bait social media that performs Ethereum loyalty without technical substance
- A timeline claim that slips without immediate public acknowledgment (he will remember)
- Anyone speaking for EEZ who clearly doesn't understand the technical architecture

---

### Objection Handling — Quick Reference

| His objection | The right response |
|---------------|-------------------|
| *"This is just Gnosis rebranding its L2 strategy."* | The answer is structural: Swiss foundation, separate legal entity, EF co-funded, no Gnosis branding in the protocol. Don't argue — point at the structure. If he's right about something specific, acknowledge it. |
| *"Ethereum alignment is what every L2 says. What's the actual mechanism?"* | Give the specific economic answer: every EEZ chain settles on Ethereum every 12 seconds, ETH is the gas token, sequencer economics flow through L1. The mechanism, not the claim. |
| *"You'll never ship in time — fragmentation will have ossified by then."* | Name the timeline honestly. Don't defend it if it's tight. Agree that the window is narrow. Show what's already working at devnet. |
| *"Your proving benchmarks are impressive at 16 GPUs — what happens at 200 chains?"* | This is a good technical question. Give the honest answer: what the scaling model shows, what hasn't been tested yet, what the team is working on. Don't dismiss it. He'll know if you're bluffing. |

---

### How EEZ Loses Marcus

1. EEZ makes an "Ethereum alignment" claim in a post without a specific structural explanation.
2. Marcus writes a 15-tweet thread pointing out that the claim has no mechanism behind it and looks identical to what AggLayer and Superchain have said.
3. The thread is correct, or partially correct.
4. Kai and Priya see the thread. Their due diligence suddenly involves an extra round of questions EEZ has to answer.
5. EEZ responds defensively or not at all. Marcus is confirmed in his skepticism.

**Prevention:** Every "Ethereum-aligned" claim must come with the specific mechanism. EEZ never claims alignment — it demonstrates it by describing the settlement model. And if Marcus writes a technically accurate critique, EEZ engages with it seriously and publicly, in the same thread.

---

### The EEZ Aha Moment

*"Every chain that joins EEZ settles on Ethereum every 12 seconds. L2 activity that was leaving L1 comes back."*

And specifically: *Vitalik or the EF citing EEZ without Gnosis attribution.* That's the signal Marcus is actually waiting for. He won't fully trust EEZ until someone he already trusts treats it as credible independently.

---

### What EEZ Says to Marcus (through substance, not through direct targeting)

> *"Every chain that joins EEZ settles on Ethereum every 12 seconds. L2 activity that was leaving L1 comes back."*

**Note:** Marcus is not the primary audience. EEZ does not write content for him. But the Nostalgia/Hope pillar — narrative essays by Friederike and Martin anchored in the original Ethereum dream — will reach him if it's honest. The conference keynote at EthCC or Devcon is his primary EEZ touchpoint. Jordi's technical writing is the other one.

---

### Content That Resonates

- Nostalgia/Hope narrative essays by Friederike or Martin — when they're specific and rooted in real Ethereum history, not vague inspiration
- Jordi's long-form technical posts on Zisk proving architecture — he reads these as primary source
- Build-in-public updates that include what isn't working yet — the honesty is the signal
- Technical posts on the economic model: how sequencer fees flow to L1, what ETH as gas token means in practice
- EF or Vitalik citations of EEZ in governance or research contexts (earned, not requested)

### Content That Repels

- Any post that says "Ethereum-aligned" without naming the mechanism
- "Ecosystem" language — he's heard it too many times
- Engagement-bait threads ("Hot take: L2s are broken — here's why")
- Partnership announcements without technical substance
- Anything that looks like retail marketing or token-adjacent messaging

---

# Cross-Persona Behaviour Chart

The tables below compare all four personas across key behavioural dimensions. Use this as a quick-reference before writing, targeting, or designing outreach.

---

## Part A — Trust Architecture

| Dimension | Kai (Rollup Founder) | Priya (Protocol Architect) | Stefan (Compliance Builder) | Marcus (ETH Loyalist) |
|-----------|---------------------|--------------------------|--------------------------|----------------------|
| **What builds trust fastest** | Reference chain with honest integration report | Open-source code + reproducible benchmark | Legal structure documentation + no-token confirmation | Vitalik / EF independent citation |
| **Who they trust as a source** | Technical founders they've met in person | Protocol engineers with a verifiable shipping record | Compliance-native builders + legal advisors | Jordi Baylina, Vitalik, EF researchers |
| **What immediately destroys trust** | Vague answer on reorg handling | "Trustless" without a defined trust model | Any hint of token or community incentives | "Ethereum-aligned" claim without structural mechanism |
| **How they respond to honest bad news** | Increased trust if handled well | Increased trust — she respects structural honesty | Neutral to positive — expected in rigorous projects | Increased trust — validates that EEZ isn't hype-driven |
| **Trust recovery after a mistake** | Possible if the correction is fast and specific | Possible if technical — harder if comms-related | Very difficult — institutional credibility is fragile | Possible if EEZ engages the critique publicly and substantively |

---

## Part B — Information & Discovery

| Dimension | Kai (Rollup Founder) | Priya (Protocol Architect) | Stefan (Compliance Builder) | Marcus (ETH Loyalist) |
|-----------|---------------------|--------------------------|--------------------------|----------------------|
| **Primary discovery channel** | Extended technical network / conference | GitHub + governance forums | Peer network of compliance builders | ETH Magicians / Ethereum Research |
| **Most trusted content format** | Deployment walkthrough by the engineer who did it | Technical spec + commented code | Governance documentation + legal structure | Long-form technical essay by a credentialed author |
| **Secondary discovery channel** | Twitter/X — curated technical list | Twitter/X — protocol engineers specifically | Conference conversations (one-on-one) | Twitter/X — direct primary sources |
| **Long-form content read** | Technical posts with specific failure modes | Research papers, implementation reports | Legal and governance publications | Ethereum Research forum posts, EF blog |
| **What they skim past** | Partnership announcements, ecosystem news | Any marketing-language content | Social media, token-adjacent news, deep ZK tech | Marketing content of any kind |

---

## Part C — Decision Dynamics

| Dimension | Kai (Rollup Founder) | Priya (Protocol Architect) | Stefan (Compliance Builder) | Marcus (ETH Loyalist) |
|-----------|---------------------|--------------------------|--------------------------|----------------------|
| **Time to decision** | 3–8 weeks | 4–10 weeks | 8–20 weeks | Non-linear — conviction forms over months |
| **Primary blocker** | Reorg complexity not clearly explained | Atomicity guarantee not precisely defined | Legal structure undocumented; token risk not excluded | Technical claim not independently falsified |
| **Who else must approve** | Co-founder / technical team | Protocol committee or co-founder | Compliance team + institutional counterparty | Nobody — but he broadcasts his conclusion publicly |
| **Decision trigger** | Honest integration report from a comparable chain | Reference implementation he can read and test | Peer reference who has presented EEZ to an institution | Independent mention by someone he already trusts |
| **What makes them move fast** | A builder they respect says "I did this and here's what it was actually like" | "Here's the implementation — go verify it yourself" | "Here's who else has completed compliance review" | Nothing — this forms at its own pace |

---

## Part D — Community Participation

| Dimension | Kai (Rollup Founder) | Priya (Protocol Architect) | Stefan (Compliance Builder) | Marcus (ETH Loyalist) |
|-----------|---------------------|--------------------------|--------------------------|----------------------|
| **Twitter/X behaviour** | Selective — posts when he has something specific to say | Technical threads when something is wrong or incomplete | Very low — mostly reads | Public discourse — threads, corrections, quote-tweets |
| **Discord behaviour** | Lurks in technical channels; jumps in when a specific question is in his domain | Reads changelogs and implementation threads; rarely posts unless there's a technical error | Not present in Discord — not appropriate channel for him | May be present in OG ETH community servers; not EEZ-specific |
| **Conference behaviour** | Presents, networks in the hallways, asks sharp questions | Presents technical work, attends ZK-specific sessions | Attends RWA and institutional tracks, values 1:1 conversations | Attends everything, has opinions on everything, talks to researchers |
| **Response to "join the community"** | Rolls eyes | Ignores | Actively suspicious — is this a token play? | Depends entirely on what "community" means |
| **How they become advocates** | Write a thread about what the integration actually felt like — after it works | Publish a technical post that references EEZ's architecture — after they've verified it | Tell a counterparty and the counterparty approves | Stop writing critical threads and start writing explanatory ones |
| **What makes them vocal critics** | A technical claim that doesn't hold at their scale | A benchmark that can't be reproduced | A governance change that wasn't signalled | An "Ethereum-aligned" claim with no structural backing |

---

## Part E — Content Matching

| Content type | Best for | Worst for | Notes |
|-------------|---------|----------|-------|
| **Build-in-public weekly update** | Marcus (conviction signal), Kai (trust) | Stefan (wrong register) | Must be specific and data-driven — "we made progress" is useless |
| **Deployment walkthrough (honest, engineer-written)** | Kai (primary), Priya (secondary) | Marcus (wants spec, not narrative) | Must include what was hard, not just what worked |
| **Technical benchmark report** | Priya (primary), Kai (secondary) | Stefan (needs governance first) | Full methodology required — hardware, scale, failure conditions |
| **Nostalgia/Hope narrative essay** | Marcus (primary), Kai (secondary) | Stefan (wrong register), Priya (too emotional) | Must be written by Friederike or Martin — not Lucas |
| **Governance / legal structure documentation** | Stefan (primary), Marcus (validates Gnosis separation) | Priya (lower priority than spec) | Legal structure details, not just "Swiss foundation" |
| **FAQ / glossary** | Stefan (governance FAQ primary), Kai (integration FAQ) | Marcus (reads primary sources) | Stefan uses the governance FAQ in counterparty reviews |
| **OG community engagement thread** | Marcus (primary) | Stefan (won't engage), Priya (finds it shallow) | "What did you build when everything was on L1?" — participation, not broadcast |
| **Community issue thread (shitfinders)** | Kai (engaged), Priya (will raise real issues) | Stefan (wrong format) | Must act on what's raised — if it's performative, Marcus will say so |
| **Reference implementation (code)** | Priya (primary), Kai (secondary) | Stefan, Marcus | Open source — she will read the actual code, not the README |
| **Alliance member announcement** | All — tone varies by persona | — | Kai: technical fit. Priya: what it means for the protocol surface. Stefan: compliance posture. Marcus: is it substantive? |
| **Keynote / conference talk** | Marcus (primary), Kai (secondary), Priya (tertiary) | Stefan (prefers 1:1) | Must be technically credible — Marcus will probe it |
| **Email newsletter (monthly)** | Kai (secondary), Stefan (secondary) | Marcus (reads primary sources), Priya (prefers GitHub) | Good for Kai and Stefan synthesis — not a replacement for technical depth |
| **Earned media / Bankless podcast** | Marcus (primary), Kai (secondary) | Stefan (wrong channel), Priya (prefers technical depth) | Friederike or Martin as guest — not a product segment, a narrative conversation |

---

## Part F — The Message That Opens the Conversation

| Persona | Opening message | What must follow immediately |
|---------|----------------|----------------------------|
| **Kai** | "EEZ gives your chain access to Ethereum's full liquidity and security from day one — without building a DEX, a bridge, or an oracle layer yourself." | Specific benchmark data + one named reference chain with honest integration story |
| **Priya** | "EEZ lets your contracts call Ethereum mainnet atomically from any EEZ chain — in the same transaction, in the same block, with no bridge in the path." | Reference implementation code + reproducible benchmark methodology |
| **Stefan** | "EEZ is governed by a Swiss non-profit foundation, has no token, and is co-funded by the Ethereum Foundation — the structural conditions that make it safe to build regulated products on." | Legal foundation documentation + compliance-native reference builder contact |
| **Marcus** | *(Do not open a conversation — let the work speak first)* | Technical substance + independent third-party validation; if he writes a critique, engage it publicly on the same channel |

---

## Part G — Strategic Priority & Phase Alignment

| Dimension | Kai (Rollup Founder) | Priya (Protocol Architect) | Stefan (Compliance Builder) | Marcus (ETH Loyalist) |
|-----------|---------------------|--------------------------|--------------------------|----------------------|
| **Strategic priority** | #1 — without supply-side chains, nothing else exists | #1A — without demand-side protocols, chain value doesn't compound | #2 — adds credibility; not blocking for launch | #3 — narrative must hold; don't optimize content for him |
| **Active in Phase 1 (Pre-Testnet)** | ✅ Build category awareness; start alliance conversations | ⚠️ Limited — nothing to evaluate yet; plant seeds | ⚠️ Limited — too early for compliance due diligence | ✅ Nostalgia/Hope narrative reaches him now |
| **Active in Phase 2 (Testnet)** | ✅ Primary target — deployment walkthroughs, benchmarks | ✅ Primary target — reference implementations go live | ✅ Begins formal evaluation | ⚠️ Watching — needs to see real technical substance |
| **Active in Phase 3 (Mainnet)** | ✅ Full cadence | ✅ Full cadence | ✅ Full cadence | ✅ Confirmation signal — is EEZ everything it claimed to be? |
| **How they influence each other** | Kai joining makes Priya's evaluation viable | Priya deploying validates Kai's choice | Stefan joining answers Marcus's Gnosis-separation concern | Marcus validating reduces friction in Kai's board conversations |
| **Segment size** | ~50–200 globally | ~500–2,000 globally | ~20–100 globally | ~100–500 influential members (of 10,000–50,000 total) |

---

