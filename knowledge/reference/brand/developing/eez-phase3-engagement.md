# EEZ Phase 3 Engagement Loop

**Status:** Planning — publish at DappCon
**Owner:** Armagan Ercan (EEZ Coordinator and comms lead)
**Audience:** Builders who encounter EEZ at DappCon or through referral; alliance partners who will refer builders into the loop

---

## The problem this solves

DappCon generates public attention at scale. Builders see the panel. They read the Mirror essay. They may follow the EEZ account. Then nothing happens for 12-18 months until first zone mainnet.

That gap is the problem. Without a mechanism, DappCon attention dissipates. Builders who are genuinely interested have no way to stay connected. There is no landing surface for someone who wants to track EEZ without committing to alliance membership. The attention EEZ earns at DappCon has nowhere to go.

This document designs the engagement loop that bridges Phase 2 (DappCon) and Phase 4 (first zone mainnet).

---

## Two builder signals

EEZ has two distinct signals for builders. They are not interchangeable.

### EEZ Alliance member (existing, unchanged)

Full membership in the alliance. Requires alignment on the thesis, active technical or ecosystem contribution, and EEZ approval. Grants the EEZ Alliance mark for display on partner properties. Listed on eez.io/alliance. Membership is reviewed; it is not permanent.

### EEZ Builder (new, lightweight)

A lightweight signal for builders who want to track EEZ without committing to alliance membership. No approval required. No obligations. No permanence needed.

**How to become an EEZ Builder:**

Three independent actions, any one is sufficient:

1. Add the `eez-builder` topic to a relevant GitHub repository.
2. Join the EEZ Discord and self-assign the Builder role in the designated channel.
3. Add "EEZ Builder" to your X bio (optional, public signal only).

Any of these can be done within 7 days of DappCon, without contacting EEZ.

**What EEZ Builder gives:**

- RFC notifications when a new Request for Comments opens (GitHub issue or forum post).
- Access to Research Office outputs as they publish: economic modeling deep-dives, technical spec updates, governance proposals.
- Inclusion in the tracked builder cohort that EEZ monitors for alliance conversations.

**What EEZ Builder does not give:**

- The EEZ Alliance mark. The mark requires full alliance membership.
- A listing on eez.io/alliance.
- Any speaking or governance rights.
- Any commercial relationship with EEZ, Gnosis, ZisK, or the Ethereum Foundation.

**The funnel:**

EEZ Builder is a tracked cohort, not a waiting room. EEZ monitors the cohort for builders whose work is maturing toward alliance-level alignment. When that threshold is reached, EEZ reaches out to open an alliance conversation. The builder does not need to apply; EEZ initiates.

---

## The engagement loop

The loop runs continuously during Phase 3. It is explicit and repeatable.

1. Builder discovers EEZ at DappCon or through a referral from an alliance partner.
2. Builder reads "How to Track EEZ" and takes one action: GitHub topic, Discord, or X bio.
3. Builder is now an EEZ Builder. They receive RFC notifications and Research Office outputs.
4. Governance RFC #N opens. EEZ notifies builders and explicitly invites external comments.
5. Builder reads the RFC and submits comments on the GitHub issue or forum post.
6. RFC closes. EEZ publishes the updated spec. **RFC contributors are credited by name in the published spec update changelog.**
7. The credit closes the loop: builder sees their contribution reflected in the public record.
8. Builder's work matures. EEZ monitors the cohort and reaches out when work aligns with alliance-level criteria.
9. Builder joins the alliance. Both sides announce. Builder displays the EEZ Alliance mark.
10. Alliance partner refers the next builder into step 1.

The feedback mechanism in step 6 is not optional. Credit in the public record is what makes the loop credible. It signals that external participation changes the spec, not just the comment count.

---

## Research Office cadence

Research Office is not a person or organisation. It is a cadence and publishing model. Four outputs per quarter, every quarter during Phase 3.

### Four outputs per quarter

**1. Governance RFC**
- Format: GitHub issue or forum post (Ethereum Magicians or EEZ forum).
- Open for 30 days.
- External comment explicitly invited in the opening post.
- RFC contributors credited in the published spec update.

**2. Economic modeling deep-dive**
- Format: Mirror or research repo.
- Designed for citation. Academic register, explicit assumptions, version-tracked.
- Covers one economic question per quarter: fee distribution, zone economics, incentive alignment, or governance participation thresholds.

**3. Technical spec update**
- Format: changelog entry in the research repo.
- Three sections: what changed, what is resolved, what is still open.
- "Still open" is a standing invitation for external contribution.

**4. Quarterly builder brief**
- Format: short document or thread.
- Summarises the quarter's outputs for builders who do not read full specs.
- Links to all three primary outputs.

### Quarterly schedule

| Quarter | Governance RFC | Economic deep-dive | Spec update |
|---|---|---|---|
| Q3 2026 | RFC #1: zone governance model | Fee distribution and zone economics | Initial spec changelog, post-DappCon state |
| Q4 2026 | RFC #2: multi-zone composability rules | Incentive alignment across zones | Spec update reflecting RFC #1 feedback |
| Q1 2027 | RFC #3: governance participation thresholds | Zone economics under load | Spec update reflecting RFC #2 feedback |
| Q2 2027 | RFC #4: mainnet readiness criteria | Economic projections for first zone | Pre-mainnet spec freeze and open issues list |

---

## DappCon activation deliverable

Two items publish at DappCon or within 3 days after.

### 1. "How to Track EEZ" page

A single public page (eez.io/track or equivalent) containing:

- Reading order: what to read first, what to read next (overview, comms framework, technical spec).
- Where to find EEZ: GitHub organisation, Discord, X account.
- How to become an EEZ Builder: three actions, any one sufficient, all doable in under 10 minutes.
- When RFC #1 opens: specific quarter and where to find the notification.
- What Research Office outputs look like: one sentence each, with links to examples when available.

This page is the concrete action surface. A builder who saw the DappCon panel can arrive here, take one action, and be in the loop. No email required. No approval gate.

### 2. Research Office announcement

A post on Mirror or the EEZ forum, published within 3 days of DappCon, that:

- Names the Research Office concept explicitly: what it is, what it is not, why EEZ runs it this way.
- Publishes the Q3 2026 calendar (RFC #1 date, economic deep-dive topic, spec update date).
- Explicitly invites non-alliance researchers and builders to participate in RFC #1.
- States the credit mechanism: external contributors named in the published spec update.

This announcement is the signal to the Ethereum research community that EEZ is running a serious, open process. It is not a marketing post. It reads like a research organization publishing its work schedule.

---

## Phase 3 engagement metrics

| Metric | Source | Target |
|---|---|---|
| EEZ Builder cohort size | GitHub topic count + Discord builder role count | 50 teams by end of Q3 2026 |
| RFC external comment rate | GitHub issue or forum: ratio of non-alliance to alliance comments per RFC | 1:1 by RFC #2 (external comments equal alliance comments) |
| Research Office output reach | Mirror reads + research repo stars per quarter | Each output cited by at least 3 external teams within 30 days |
| Alliance pipeline from Builder cohort | Builders who became Alliance members after starting as EEZ Builder | 3 teams per quarter from Q4 2026 onward |
| Spec contribution rate | Named credits in spec update changelog | At least 5 distinct external contributors credited across Q3+Q4 2026 |

---

## Phase 3 tone transition trigger

Phase 3 starts with the "exclusive underground" posture from Phase 1 still active. That posture is honest when the circle is genuinely small and curated. It becomes performative if maintained artificially. Plan the transition before it is needed.

### Named threshold

The tone transition triggers when either condition is met:

- **Condition A:** EEZ Builder cohort exceeds 50 teams.
- **Condition B:** RFC participation generates more external comments than alliance comments in a single RFC period.

Either condition signals that EEZ is no longer a small insider conversation. Continuing to act like one is no longer honest.

### The switch

The transition is deliberate, not gradual. When the threshold is reached:

1. Publish a short statement acknowledging the shift. Name it explicitly. "EEZ is no longer early-insider territory. Here is what that means for how we communicate."
2. Drop "for builders who need it" from all standing copy. Replace with "here is the work."
3. The EEZ account shifts account cadence from thesis-posting to research-office-reporting: weekly or biweekly updates on RFC progress, spec updates, and builder contributions replace the slow-burn intellectual framing posts.
4. Tone in new posts shifts from "cinematic structural" to "institutional calm." The voice stays precise and technically honest. The posture stops signaling scarcity and starts signaling reliability.

The brand strategy is explicit that institutional calm scales and manufactured exclusivity does not. This is the operational trigger for that transition.

### What does not trigger the switch

Media coverage, follower count, or the number of alliance partners alone. The trigger is builder participation in the open process, not external attention.
