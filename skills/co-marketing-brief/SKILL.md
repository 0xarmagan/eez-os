# Co-Marketing Brief Generator Skill

Use this skill when creating campaign-specific co-marketing briefs for EEZ Alliance partners.

## When to trigger

- User says "draft co-marketing brief for [partner]", "prepare [partner] for [campaign]"
- Planning a launch, milestone, integration, or research campaign
- Need to align partner comms with EEZ messaging framework before they publish
- Slash command `/co-marketing-brief [partner] [campaign-type] [timeline]` is invoked

## What this skill does

Reads from:
1. `data/partners.yaml` — partner profile, commitment, audience
2. `knowledge/eez/01-comms-framework.md` — messaging pillars & frameworks
3. `knowledge/eez/kb/01-getting-started/` — narrative & positioning
4. `personas/partner.md` — partner persona voice guide
5. `wiki/notes/` — partner's past campaign patterns & what worked
6. `templates/partner-outreach-email.md` — outreach patterns

Produces a **campaign-specific brief** that shows:
- Campaign goal & timeline
- Partner's role & amplification opportunity
- Messaging pillars tailored to their audience
- Suggested content types (tweet, blog, video, article, in-call mention)
- Content calendar (what, when, who)
- Gnosis Marketing handoff & review gate
- Brand asset requirements (logos, screenshots, data)
- Success metrics

---

## Required inputs

Before generating, confirm:

1. **Partner name** — must match `data/partners.yaml`
2. **Campaign type** — choose one:
   - `launch` — main EEZ announcement (March 29, 2026 EthCC)
   - `mainnet-milestone` — testnet→mainnet transition, or phase milestone
   - `integration-launch` — partner's integration goes live
   - `research-highlight` — deep-dive research, whitepaper, or spec update
   - `community-event` — community call, workshop, speaking opportunity
   - `custom` — other (describe)

3. **Timeline** — when should this activate?
   - "immediately" (rolling start)
   - specific date (e.g., "May 29, 2026")
   - relative to event (e.g., "T-14 days before mainnet")

4. **Focus audience** (optional) — who should this campaign reach?
   - `technical` (engineers, researchers)
   - `institutional` (VCs, protocols, institutions)
   - `ecosystem` (other partners, builders)
   - `general` (all audiences)

If missing, ask once before generating.

---

## Standard brief structure

```
# Co-Marketing Brief: [Partner] — [Campaign Type]

## Campaign Overview
**Goal:** [One sentence on what we're trying to achieve]  
**Timeline:** [Start date → End date]  
**Partner's role:** [Amplification, integration announcement, research feature, etc.]

## Why This Partner Matters
[2–3 sentences on their influence + audience fit]
- Audience reach: [market segment they influence]
- Credibility signal: [why their voice matters for this campaign]
- Past pattern: [what's worked before with this partner]

## Messaging Framework
[Adapted from `01-comms-framework/` for this partner's audience]

**Core narrative:** [1-sentence headline]
**Pillars for this campaign:**
1. [Pillar 1] — [Why this resonates with their audience]
2. [Pillar 2]
3. [Pillar 3]

**Banned phrases for this partner:** [e.g., "next-gen", "unlock", "game-changer"]

## Suggested Content & Cadence

| Content Type | Timing | Format | Audience | Notes |
|--------------|--------|--------|----------|-------|
| [e.g., "Tweet"] | [T-X days] | [e.g., "280 chars"] | [who sees it] | [e.g., "Use data from briefing"] |

**Content ideas:**
1. [Specific suggestion + why]
2. [Specific suggestion]
3. [Specific suggestion]

## Gnosis Marketing Handoff
**Gnosis role:** [Design, distribution, paid amplification, etc.]  
**What they need from partner:** [Logo, approval, timing confirmation]  
**Success metric:** [Engagement, reach, click-through, or narrative signal]

## Brand Assets & Approvals
- [ ] Logo usage approved
- [ ] Data accuracy reviewed (technical claims)
- [ ] Tone alignment confirmed
- [ ] Timeline locked in

## Timeline & Checklist

| When | Action | Owner |
|------|--------|-------|
| [Date] | Draft sent to partner | [Ben/Ecosystem Lead] |
| [Date] | Partner feedback due | [Partner name] |
| [Date] | Final brief approved | [Ben] |
| [Date] | Campaign launch | [Partner + Gnosis Marketing] |

## Notes for Partner Relationship Manager
- [What to watch for]
- [Relationship context]
- [Anything unusual or time-sensitive]
```

---

## Campaign Type Specifics

### Launch Campaign
- **Goal:** Amplify EEZ announcement across partner audiences
- **Timeline:** Usually rolling (many partners, cascading over 2–4 weeks)
- **Content types:** Tweet, blog post, founder mention, investor note
- **Success metric:** Brand lift, narrative coherence across partners
- **Example:** Linea's March 30, 2026 tweet about EEZ + founder note

### Mainnet Milestone Campaign
- **Goal:** Signal progress & technical credibility
- **Timeline:** Concentrated around go-live date
- **Content types:** Technical deep-dive, architecture thread, "we tested it" post
- **Success metric:** Technical audience engagement, developer adoption signals

### Integration Launch Campaign
- **Goal:** Drive awareness that partner is now EEZ-enabled
- **Timeline:** Partner's go-live date (often not synchronized)
- **Content types:** Announcement, tutorial, use-case story
- **Success metric:** Integration activity, TVL, app downloads (partner's metric)

### Research Highlight
- **Goal:** Establish EEZ as rigorous & research-backed
- **Timeline:** When research is published or spec updated
- **Content types:** Research thread, whitepaper share, technical explainer
- **Success metric:** Developer interest, researcher citation, technical credibility

---

## Voice & Tone Guidelines

Apply from `personas/partner.md`:
- **Partner voice, not EEZ voice** — let them tell the story in their register
- **No marketing fluff** — "next-gen", "game-changer", "unlock" are banned
- **Specific claims** — "enables atomic cross-L2 swaps in 6 seconds" not "synchronous composability"
- **Credibility first** — lead with technical fact, not vision
- **Peer tone** — they're a co-builder, not a customer

---

## After generating

1. Save to `outputs/drafts/co-marketing-[partner]-[campaign]-[date].md`
2. Review brief with Ben before sharing with partner
3. Send brief to partner with 7–10 day feedback window
4. Track approvals in `wiki/log.md` (dates & sign-offs)
5. Coordinate Gnosis Marketing handoff once partner confirms

---

## Common invocations

### "Brief for Linea launch amplification"
```
/co-marketing-brief Linea launch "immediate"
→ Campaign goal: amplify EEZ launch
→ Content ideas tailored to Linea's audience (L2 builders)
→ Timeline for Linea's communications team
→ Gnosis Marketing handoff details
```

### "Integration launch brief for Aave"
```
/co-marketing-brief Aave integration-launch "2026-06-15"
→ Campaign goal: drive awareness of Aave on EEZ
→ Content ideas for DeFi audience (TVL, slippage, atomic swaps)
→ Coordination with Aave's product launch cycle
```

### "Research brief for Nethermind"
```
/co-marketing-brief Nethermind research-highlight "when spec updated"
→ Campaign goal: technical credibility
→ Content ideas for engineer audience
→ Coordination with spec publication date
```

---

## Data sources

- **partners.yaml** — baseline partner info
- **01-comms-framework** — messaging pillars to adapt
- **personas/partner.md** — voice tone for this segment
- **wiki/notes/** — what's worked with this partner before
- **knowledge/eez/kb/** — technical & market context
- **templates/partner-outreach-email.md** — outreach patterns

---

## If missing or conflicting data

- If partner's past campaigns aren't documented, note "No prior campaign data" and suggest general approach
- If timeline is vague, ask for clarification before generating
- If campaign type is custom/unclear, ask for 2–3 example content pieces to guide the brief

---

## See also

- `Partner Brief` — outreach emails & one-pagers
- `Alliance Activation Matrix` — which partners are ready to amplify
- `Messaging Consistency Checker` — validate partner draft against framework
- `Engagement Calendar` — coordinate timing across partners
