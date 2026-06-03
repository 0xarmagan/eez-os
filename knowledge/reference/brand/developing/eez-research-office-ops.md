# EEZ Research Office — Operations Guide

**For:** Research Office lead, Armagan Ercan
**Status:** Internal — not published externally
**Last updated:** 2026-06-01
**Source:** Derived from `eez-phase3-engagement.md`

---

## What the Research Office is

The EEZ zone standard won't be verifiable on-chain until mainnet. The Research Office is the structured process for building and testing it in public until then — four outputs per quarter, external contributors credited by name.

Concretely: the Research Office publishes four documents per quarter on a fixed schedule. Each quarter produces a governance RFC (a specific governance question put to public comment), an economic modeling paper (a technical analysis of how zone economics work), a spec update (a changelog showing what changed and why), and a builder brief (a plain-language summary of the other three). These documents accumulate into a public record that builders and researchers can read, reference, and contribute to before mainnet.

The Research Office does three things that plain social media activity cannot:

**It makes the governance claim testable in real time.** The governance RFC is an open comment period on a real spec decision. External contributors are named in the published spec update when their input changes something. This is not a consultation process — it is a live governance mechanism running while the full on-chain model is being built.

**It gives builders a reason to stay engaged before there is anything to deploy on.** A builder who finds EEZ at DappCon has nowhere to go if the next 18 months are only thesis posts. The Research Office gives them RFCs to comment on, spec updates to track, and economic papers to cite in their own work. It converts passive attention into active participation.

**It produces citable evidence that the standard is being built seriously.** Academic researchers, protocol economists, and DeFi teams that evaluate infrastructure make decisions based on published technical work, not conference panels. Each economic deep-dive and spec update is written to be referenceable in external research.

One person operates the Research Office. The Research Office lead manages the publication schedule, drafts three of the four outputs, coordinates with Friederike Ernst (governance RFCs) and Jordi Baylina (economic modeling and spec accuracy), and tracks the builder cohort. Armagan reviews all outputs before publication and owns the partner communication layer.

The external-facing name "EEZ Research Office" appears on all outputs. It is used consistently because it signals an institution with a schedule, not a project with intermittent posts.

---

## Who operates it

**Research Office lead** — the person this guide is written for. Primary operational owner of all four outputs, the metrics dashboard, the builder cohort, and the RFC lifecycle. Estimated time: 15–20 hours per week at peak (RFC open periods); 8–12 hours per week in non-RFC weeks.

**Armagan Ercan** — EEZ Coordinator. Reviews all outputs before publication. Signs off on RFC topic selection. Manages alliance partner notifications. Does not draft primary outputs.

**Friederike Ernst** — intellectual voice for governance RFCs. Reviews draft RFCs for governance accuracy and provides framing language. Not a full-time contributor — budget 2–4 hours per RFC cycle.

**Jordi Baylina** — technical author for economic modeling deep-dives and spec updates. Budget 4–6 hours per quarter per output. All technical claims about ZK proving costs, native rollup economics, and zone parameter modeling require his review or co-authorship.

**External contributors** — named in spec update changelogs. Their comments close the credibility loop. Do not treat them as passive audience; treat their RFC submissions as editorial input.

---

## Quarterly cycle overview

Each quarter runs the same four outputs in sequence, not in parallel.

```
Week 1–2    RFC topic selected and drafted (Research Office lead + Friederike)
Week 3      RFC published, open for external comment (30 days)
Week 4–6    RFC open period — monitor comments, synthesize weekly, reply publicly
Week 7      RFC closes — Research Office lead writes synthesis note
Week 8–9    Spec update drafted (Research Office lead + Jordi) — incorporates RFC synthesis
Week 8–9    Economic deep-dive drafted (Research Office lead + Jordi)
Week 10     Armagan reviews all outputs
Week 11     Spec update and economic deep-dive published simultaneously
Week 12     Quarterly builder brief published (Research Office lead solo)
```

The RFC closes before the spec update is drafted — the spec update must reflect RFC feedback, not precede it. Do not run these in parallel.

---

## Output 1: Governance RFC

### Purpose

A formal request for comments on a specific governance question. The RFC is the mechanism by which external builders shape the EEZ spec, not just observe it.

### Format

- Published as a GitHub issue in the EEZ research repo OR as a post on Ethereum Magicians forum.
- The choice of venue signals the audience: GitHub for technically-focused builders, Ethereum Magicians for governance-focused community.
- Open for exactly 30 days. Post the closing date in the RFC header.

### RFC template

```markdown
# RFC #[N]: [Title]

**Status:** Open for comment
**Open until:** [date — 30 days from publication]
**Authors:** [names]
**Related spec section:** [link]

## Summary

[2–3 sentences: what question this RFC addresses and why it matters now]

## Background

[What the current spec says. What problem or gap this RFC addresses. What happens if we get this wrong.]

## Proposed approach

[The specific mechanism, rule, or parameter EEZ is considering. Written as a draft spec change, not as a question.]

## Open questions

[Numbered list of specific questions for commenters. Not "what do you think?" — specific: "Should the exit window be 7 days or 14 days? What is the tradeoff?" Each question should be answerable with evidence or reasoning.]

## How to comment

Comment on this issue. Comments from non-alliance builders are explicitly invited and will be credited in the published spec update if they change or refine the approach. Close date: [date].
```

### RFC topic selection per quarter

| Quarter | RFC topic | Why this quarter |
|---|---|---|
| Q3 2026 | Zone governance model | Foundation — must be established before composability rules can be written |
| Q4 2026 | Multi-zone composability rules | Depends on Q3 governance model being settled |
| Q1 2027 | Governance participation thresholds | Who can propose and vote — requires composability model to be resolved |
| Q2 2027 | Mainnet readiness criteria | What must be true before a zone deploys — requires all prior RFCs resolved |

Do not swap the order. Each RFC depends on the prior one.

### Monitoring during open period

- Check comments every 48 hours during the open period.
- Acknowledge every substantive comment with a brief reply within 72 hours.
- When two commenters disagree: do not take sides in the thread. Note the disagreement and ask each to clarify their assumption.
- When a comment raises a point not covered in the RFC: acknowledge it publicly, note whether it will be addressed in this RFC or deferred to the next one.

### Synthesis note (Week 7)

After the RFC closes, write a 300–500 word synthesis note that:
- Summarises what changed as a result of external comments (be specific — cite the comment)
- Names contributors whose input changed or refined the approach
- States what was NOT adopted and why
- Publishes this note as a reply to the original RFC issue before closing it

This synthesis note is the source material for the spec update changelog.

---

## Output 2: Economic modeling deep-dive

### Purpose

A technical paper on one economic question per quarter, written for external citation. Academic register, explicit assumptions, version-tracked. This is the output that research teams and protocol economists reference.

### Format

- Published on Mirror (for reach) with a version-tracked copy in the research repo.
- Minimum 1,500 words. No upper limit, but do not pad.
- All assumptions stated explicitly in an "Assumptions" section. This is not optional — it is what makes the paper citable.
- All models and calculations reproducible. If a spreadsheet or model underlies the numbers, link to it in the repo.

### Deep-dive topic per quarter

| Quarter | Topic | Key question |
|---|---|---|
| Q3 2026 | Fee distribution and zone economics | How do transaction fees flow in a native rollup architecture? What share reaches Ethereum validators vs. zone operators vs. provers? |
| Q4 2026 | Incentive alignment across zones | What prevents zones from defecting from the governance standard? What makes staying in EEZ economically rational? |
| Q1 2027 | Zone economics under load | What happens to fee distribution and governance participation when a zone reaches high transaction volume? |
| Q2 2027 | Economic projections for first zone | What does the first zone's economic model look like at launch? What are the key assumptions a zone team must validate? |

### Drafting process

1. Research Office lead writes a 500-word brief: the question, the key variables, the expected shape of the answer.
2. Brief goes to Jordi for a 1-hour review call. Jordi confirms whether the technical model is sound or identifies the correct framing.
3. Research Office lead drafts the full paper.
4. Jordi reviews the draft — specifically the model, the fee mechanics, and any claims about ZK proving costs.
5. Armagan reviews for voice and external claims (no over-claiming).
6. Publish simultaneously with the spec update (Week 11).

### Economic model deep-dive template

```markdown
# EEZ Economic Research: [Title]

**Version:** 1.0
**Published:** [date]
**Authors:** [names]
**Status:** Research — not a commitment to specific parameters

## Summary

[3–5 sentences: the question, the approach, the key finding]

## Background

[Why this question matters for EEZ's architecture. What the current spec assumes and what this paper tests or validates.]

## Assumptions

[Numbered list. Every assumption explicit. E.g.: "We assume the prover market is competitive, with at least 3 independent provers operating at mainnet launch." State what happens to the conclusion if the assumption is wrong.]

## Model

[The economic model. Show the math. Reference sources.]

## Findings

[What the model shows. Be specific about ranges and conditions.]

## Open questions

[What this paper does not resolve. What would change the conclusion. What the next quarter's paper will address.]

## Appendix

[Data sources, links to models, related research]
```

---

## Output 3: Technical spec update

### Purpose

A changelog entry showing the state of the EEZ zone standard after incorporating RFC feedback. Three sections: what changed, what is resolved, what is still open. The "still open" list is a standing invitation.

### Format

- Published as a changelog entry in the EEZ research repo (`CHANGELOG.md` or equivalent).
- One entry per quarter. Not a full rewrite — a delta from the prior state.
- Mirrored as a post on Mirror for reach (link back to the repo).

### Spec update template

```markdown
# EEZ Zone Standard — Spec Update [Q/Year]

**Published:** [date]
**Prior state:** [link to prior changelog entry or spec version]
**RFC this update incorporates:** [link to RFC #N and synthesis note]

## What changed

[Numbered list. Each item: what was in the prior spec, what changed, why — citing the RFC comment or finding that drove the change. Name the contributor if applicable.]

Example:
> 1. Exit window increased from 7 days to 14 days. Prior spec specified 7 days; RFC #1 comment by @[handle] identified that 7 days is insufficient for validator coordination under peak network load. Adopted as proposed.

## What is resolved

[Questions from prior versions or RFCs that are now settled. These will not change in subsequent updates unless a new RFC reopens them.]

## What is still open

[Numbered list of questions the spec has not yet answered. Each item: what the question is, when we expect to address it, and how external contributors can help. This is a standing invitation — treat it as one.]

## External contributors credited this quarter

[Names or handles of external contributors whose input changed or refined this update. Link to their specific comment.]
```

---

## Output 4: Quarterly builder brief

### Purpose

A short summary of the quarter's three primary outputs for builders who do not read full specs or technical papers. The builder brief is the only Research Office output that is explicitly not academic — it is conversational and action-oriented.

### Format

- 300–500 words maximum.
- Published as a thread on @EthEconomicZone OR as a short Mirror post, linked from the Discord.
- Written in first-person plural ("we published," "the RFC found").
- Three sections: what we published, what changed in the spec, and what's coming next quarter.

### Builder brief template

```markdown
**EEZ Research Office — Q[N] 2026 update**

This quarter we ran RFC #[N] on [topic], published our [economic deep-dive topic] deep-dive, and updated the zone spec. Here's what changed and what's next.

**RFC #[N]: [topic]**
[2–3 sentences: what the question was, how many external comments, what changed as a result. Name the change specifically.]

**Economic modeling: [topic]**
[2–3 sentences: the key finding in plain language. Link to the full paper.]

**Spec update**
[2 sentences: the most important thing that changed in the spec this quarter. Link to the changelog.]

**Next quarter: RFC #[N+1]**
[1–2 sentences: what the next RFC will cover and when it opens. How to participate.]

If you're an EEZ Builder, you'll get RFC #[N+1] notification when it opens. Not an EEZ Builder yet — [link to How to Track EEZ].
```

---

## Builder cohort management

### How builders enter the cohort

Three independent actions — any one is sufficient:
1. Add `eez-builder` topic to a GitHub repo
2. Join EEZ Discord and self-assign the Builder role
3. Add "EEZ Builder" to X bio

Research Office lead does not gate entry. Builders self-select.

### How to track the cohort

**GitHub:** Search `topic:eez-builder` monthly. Record count in the metrics dashboard. Note any repos that appear to be doing serious EEZ-relevant work — these are alliance candidates.

**Discord:** Check Builder role member count monthly. Scan recent messages for active contributors (posting questions, sharing work). These are alliance candidates.

**X:** Optional — search "EEZ Builder" in bios monthly. Lower signal than GitHub.

### Identifying alliance candidates

A builder becomes an alliance candidate when two conditions are met:
1. Their work is progressing (new commits, published research, active Discord participation)
2. Their work is directly relevant to an EEZ zone type (proving infrastructure, sequencing, DeFi protocol that would benefit from atomic composability, compliance architecture)

When a candidate is identified, flag to Armagan. Armagan initiates the alliance conversation — not the Research Office lead. The Research Office lead provides context: "this builder has been active in RFCs, here's their work."

Do not contact builders directly without Armagan's instruction.

### Metrics dashboard

Maintain a simple spreadsheet updated monthly:

| Month | GitHub builder count | Discord builder count | RFC #N external comments | Papers cited externally | Alliance pipeline from cohort |
|---|---|---|---|---|---|

Target thresholds (for tone transition trigger):
- Builder cohort: 50 teams triggers Condition A
- RFC external comments: exceeds alliance comments in one RFC period triggers Condition B

When either threshold is met, alert Armagan immediately. The tone transition (dropping "exclusive underground" posture) requires a deliberate published statement — see `eez-phase3-engagement.md` for the exact steps.

---

## Communication workflows

### RFC launch notification

When an RFC publishes:
1. Post to EEZ Discord (#research-office channel): link, topic summary, closing date, how to comment
2. Post from @EthEconomicZone: link + 2-sentence summary of the question
3. Notify alliance partners via the partner comms channel (or email if no channel yet): same information + "your comments are particularly useful given your work on X"
4. Post to Ethereum Magicians forum if the RFC is not already there — cross-post or link

Do not send individual DMs to builders. The notification goes to channels; builders who are in them see it.

### RFC close notification

When an RFC closes:
1. Post to Discord: "RFC #N has closed. [N] external comments received. Synthesis note published [link]. Thank you to: [names of contributors]."
2. Post from @EthEconomicZone: "RFC #N closed. [Key change] as a result of external input. [N] contributors credited. Spec update publishes in [N] weeks."

### Spec update and deep-dive publication

When spec update and economic deep-dive publish simultaneously (Week 11):
1. Post to Discord: both links with one-sentence summaries
2. Post from @EthEconomicZone: two separate posts, same day, spaced 4+ hours apart (not a thread)
3. Flag to Armagan for partner amplification coordination

### Quarterly builder brief publication

Armagan posts the builder brief from @EthEconomicZone. Research Office lead drafts it and sends to Armagan for posting. Research Office lead does not post from the EEZ account.

---

## Quarterly checklist

Use this at the start of each quarter to verify nothing is missed.

**Week 1–2 (RFC drafting):**
- [ ] RFC topic confirmed with Armagan
- [ ] RFC draft shared with Friederike for governance review
- [ ] RFC draft reviewed and changes incorporated
- [ ] RFC draft reviewed by Armagan

**Week 3 (RFC launch):**
- [ ] RFC published (GitHub issue or Ethereum Magicians)
- [ ] Discord notification posted
- [ ] @EthEconomicZone post published
- [ ] Alliance partner notification sent
- [ ] Closing date confirmed and visible in the RFC header

**Weeks 4–6 (RFC open period):**
- [ ] Comments checked every 48 hours
- [ ] Substantive comments acknowledged within 72 hours
- [ ] Synthesis note drafted in background as comments accumulate

**Week 7 (RFC close):**
- [ ] RFC closed on the named date
- [ ] Synthesis note finalized and published as reply to RFC
- [ ] External contributors listed
- [ ] Discord close notification posted

**Weeks 8–9 (spec update + economic deep-dive drafting):**
- [ ] Spec update drafted, incorporating RFC synthesis
- [ ] Economic deep-dive brief to Jordi (1-hour review)
- [ ] Economic deep-dive drafted
- [ ] Both outputs reviewed by Jordi (technical accuracy)
- [ ] Both outputs reviewed by Armagan (voice + claims)

**Week 11 (publication):**
- [ ] Spec update published (changelog + Mirror)
- [ ] Economic deep-dive published (Mirror + repo)
- [ ] Discord notification posted (both links)
- [ ] @EthEconomicZone posts (two separate posts, same day)
- [ ] Armagan coordinates partner amplification

**Week 12 (builder brief):**
- [ ] Quarterly builder brief drafted
- [ ] Reviewed by Armagan
- [ ] Armagan posts from @EthEconomicZone

**End of quarter (metrics):**
- [ ] Metrics dashboard updated (builder counts, citation count, RFC comment ratio)
- [ ] Alliance candidate list reviewed with Armagan
- [ ] Next quarter RFC topic confirmed

---

## What this guide does not cover

- Alliance membership process — see `eez-coalition-charter.md`
- Speaker briefing and governance Q&A — see `eez-governance-qa.md`
- Partner communications charter — see `eez-coalition-charter.md`
- Visual identity and mark — see `visual-identity.md` and brand strategy
- Tone of voice for public posts — see `knowledge/reference/style-guide.md`
