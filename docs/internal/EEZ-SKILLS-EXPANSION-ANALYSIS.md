# EEZ Agent Skills Expansion Analysis

**Goal:** Design new Claude Code skills for the Ecosystem Relations Lead role.

**Date:** May 16, 2026  
**Analysis of:** EEZ project scope + Head of Ecosystem Relations role requirements

---

## Current State

### Existing Skills (2)

1. **GIP Authoring** — Draft GnosisDAO improvement proposals
2. **Partner Brief** — Draft partner outreach emails and 1-pagers

### Current Gaps

The Ecosystem Relations Lead spends significant time on:
- ❌ Tracking partner activation status (manual spreadsheets)
- ❌ Monitoring relationship drift & engagement signals
- ❌ Generating co-marketing campaign briefs
- ❌ Creating & maintaining integration guidelines
- ❌ Mapping new partner candidates
- ❌ Synthesizing partner feedback
- ❌ Ensuring messaging consistency across partners
- ❌ Scheduling & tracking partner check-ins

---

## Role Analysis: What the Ecosystem Lead Does

From `04-roles/EEZ-Head-of-Ecosystem-Relations-v4.md`:

### Core Responsibilities

**1. Ecosystem Building & Partner Relations (50%)**
- Onboard founding alliance members (~20 partners)
- Manage monthly check-ins with each partner
- Maintain Alliance Activation Matrix (live tracker)
- Track blockers and timelines
- Design engagement cadence and escalation paths
- Map Wave 1 candidate chains
- Co-own researcher relations

**2. Ecosystem Communications & Messaging Execution (30%)**
- Co-develop messaging framework
- Create briefs for Gnosis Marketing
- Translate positioning into co-marketing programs
- Ensure consistency across partner communications
- Track technical claims review process

**3. Internal Coordination (20%)**
- Sync with Ecosystem Growth Lead weekly
- Maintain ecosystem calendar
- Surface relationship drift early
- Track partner sentiment

---

## Skill Opportunities (Priority Order)

### Tier 1: High Impact, High Frequency

#### 1. **Alliance Activation Matrix Generator**
**Problem:** Manual tracking of 20+ partners across commitments, timelines, blockers, status.  
**Automation:** Auto-generate/update activation matrix from:
- Partner data (data/partners.yaml)
- Engagement logs (wiki/notes/)
- GIP submissions and milestones
- Integration stage data

**Trigger:**
```
/alliance-matrix [period]
→ outputs/drafts/alliance-status-[date].md
```

**Output:** Table showing:
- Partner name | Product commitment | Timeline | Owner | Status | Last contact | Blockers

---

#### 2. **Partner Sentiment Tracker**
**Problem:** Early detection of relationship drift requires manual log review.  
**Automation:** Analyze partner interaction history and flag:
- Days since last contact (vs. expected cadence)
- Sentiment shift signals (less responsive, timeline slips, feedback tone)
- Blockers preventing activation
- Re-engagement opportunities

**Trigger:**
```
/partner-sentiment [partner-name] OR /relationship-health [all]
→ outputs/drafts/partner-sentiment-[partner]-[date].md
```

**Output:** For each partner:
- Current status (founding/prospective/dormant/active/drifting)
- Sentiment signal (strong/stable/cautious/drift-risk/requires-escalation)
- Days since last contact
- Expected next milestone
- Recommended next action (check-in, re-engagement, escalation)

---

#### 3. **Co-Marketing Brief Generator**
**Problem:** Creating tailored co-marketing briefs for each partner + campaign type (launch, milestone, integration).  
**Automation:** Generate campaign-specific brief with:
- Partner context (commitment, audience, past patterns)
- Campaign goal & timeline
- Messaging pillars (from positioning framework)
- Suggested content types
- Gnosis Marketing handoff

**Trigger:**
```
/co-marketing-brief [partner] [campaign-type] [timeline]
→ outputs/drafts/co-marketing-[partner]-[campaign]-[date].md
```

**Campaign types:** launch, mainnet-milestone, integration-launch, research-highlight

---

### Tier 2: High Impact, Medium Frequency

#### 4. **Integration Guideline Assistant**
**Problem:** Creating & maintaining the integration guideline (critical doc for onboarding).  
**Automation:** Generate sections based on integration stage and known blockers:
- Requirements by stage (spec review → testnet → mainnet)
- Technical dependencies & timelines
- Escalation paths for blockers
- Checklist for completion criteria

**Trigger:**
```
/integration-guide [stage] [partner-type]
→ outputs/drafts/integration-guide-[stage]-[date].md
```

---

#### 5. **Ecosystem Mapping & Candidate Qualification**
**Problem:** Identifying Wave 1 partner candidates from Tally submissions + network intelligence.  
**Automation:** Score candidates using qualification framework:
- Strategic fit (market position, impact on EEZ adoption)
- Technical readiness (dev capacity, existing EVM experience)
- Motivation signal (Tally submission quality, commitment areas)
- Engagement probability

**Trigger:**
```
/ecosystem-candidates [wave] [max-count]
→ outputs/drafts/candidates-wave-[wave]-[date].md
```

---

#### 6. **Partner Engagement Calendar**
**Problem:** Tracking 20+ partner events, go-lives, mainnet milestones across calendar.  
**Automation:** Aggregate partner commitment data + ecosystem events into calendar:
- Product launches & timelines
- Chain go-lives
- Mainnet readiness milestones
- Visibility & co-marketing windows

**Trigger:**
```
/engagement-calendar [months-ahead]
→ outputs/drafts/engagement-calendar-[date].md
```

---

### Tier 3: Medium Impact, High Frequency

#### 7. **Messaging Consistency Checker**
**Problem:** Ensuring partner drafts align with positioning framework before publication.  
**Automation:** Review partner draft against positioning platform:
- Claim accuracy (vs. technical spec)
- Tone consistency (vs. partner persona voice guide)
- Messaging pillar alignment
- Flag claims needing technical review

**Trigger:**
```
/check-messaging [draft-file-path]
→ Console report + outputs/drafts/messaging-review-[date].md
```

---

#### 8. **Partner Feedback Synthesis**
**Problem:** Aggregating feedback from 20+ partners into actionable insights.  
**Automation:** Review partner interaction notes (wiki/notes/) and synthesize:
- Common feedback themes
- Technical blockers (by frequency)
- Messaging concerns
- Activation risks (by partner segment)

**Trigger:**
```
/partner-feedback-summary [period]
→ outputs/drafts/feedback-synthesis-[date].md
```

---

### Tier 4: Process Automation

#### 9. **Monthly Partner Check-In Reminder & Brief**
**Problem:** Manually scheduling 20 check-ins + preparing context each month.  
**Automation:** Generate:
- List of partners due for check-in (by cadence)
- 1-page brief per partner (status, last topics, agenda suggestions, docs to share)

**Trigger:**
```
/check-in-prep [month]
→ outputs/drafts/check-in-briefs-[month]-[date]/ (one per partner)
```

---

#### 10. **Relationship Re-Engagement Playbook**
**Problem:** Structured approach to re-engaging partners showing drift signals.  
**Automation:** Generate re-engagement sequence based on drift type:
- Light touch (check-in, share updates, ask for feedback)
- Medium (flag blocker, offer support, escalate if needed)
- Escalation (leadership check-in, reset expectations)

**Trigger:**
```
/re-engage [partner] [drift-type]
→ outputs/drafts/re-engagement-[partner]-[date].md
```

---

## Implementation Roadmap

### Phase 1 (Immediate) — Tier 1 Skills
- [ ] Alliance Activation Matrix Generator
- [ ] Partner Sentiment Tracker
- [ ] Co-Marketing Brief Generator

**Why first:** 70% of the role's time and 90% of the relationship management depends on these.

### Phase 2 (Week 2) — Tier 2 Skills
- [ ] Integration Guideline Assistant
- [ ] Ecosystem Mapping & Candidate Qualification
- [ ] Partner Engagement Calendar

### Phase 3 (Week 3) — Tier 3 Skills
- [ ] Messaging Consistency Checker
- [ ] Partner Feedback Synthesis

### Phase 4 (Week 4) — Tier 4 Process Automation
- [ ] Monthly Check-In Reminder & Brief
- [ ] Relationship Re-Engagement Playbook

---

## Data Dependencies

### Required Files (Already Exist)
- `data/partners.yaml` — partner data (name, status, contact, commitment, timeline)
- `wiki/notes/` — interaction logs with partners
- `01-brand-foundation/` — positioning platform, tone guide
- `02-go-to-market/` — audience research, messaging pillars
- `knowledge/eez/kb/03-ecosystem/` — partner profiles, alliance structure

### New Data Files Needed
- `data/activation-matrix.yaml` — structured partner status (feeds Activation Matrix Generator)
- `data/integration-stages.yaml` — stage definitions and requirements
- `data/qualification-framework.yaml` — scoring rubric for Wave 1 candidates
- `data/engagement-cadence.yaml` — check-in frequency by partner tier

---

## Success Metrics

| Metric | Before | Target | Timeline |
|--------|--------|--------|----------|
| Time to generate Alliance Matrix | 2–3 hours | 5 minutes | Week 1 |
| Partner sentiment visibility | Monthly review | Real-time alerts | Week 2 |
| Co-marketing brief turnaround | 4–6 hours | 15 minutes | Week 1 |
| Partner check-in prep time | 1.5 hours/month | 20 minutes/month | Week 2 |
| Messaging consistency issues caught | Post-publication | Pre-submission | Week 3 |

---

## Integration with Existing Skills

### GIP Authoring (keep as-is)
- Used when: EEZ governance decisions need formal proposal
- Example: "Draft a GIP for [governance topic]"

### Partner Brief (expand)
- Current: Partner outreach emails & 1-pagers
- Expand to: Co-marketing briefs, relationship re-engagement sequences, messaging briefs

### New Skills (add above 9)
- Automate tracking, analysis, and relationship management
- Generate decision-ready documents for ecosystem lead review
- Surface early warnings (drift, blockers, consistency issues)

---

## Skill Design Principle

**All skills follow same pattern:**
1. **Input:** Partner name, context, period, or request type
2. **Process:** Read from canonical data (partners.yaml, wiki/, knowledge/)
3. **Output:** Markdown brief → `outputs/drafts/` for human review before action
4. **Review Gate:** Human always reviews before sending/publishing

---

## Next Steps

1. **Validate:** Does this match the Ecosystem Relations Lead's actual workflow?
2. **Prioritize:** Confirm Phase 1 skills are correct
3. **Design:** Create SKILL.md files for Phase 1 (3 skills)
4. **Build:** Implement Phase 1 skills
5. **Test:** Run with real partner data
6. **Iterate:** Feedback loop to Phase 2

---

*Analysis by Claude | Recommendations ready for review*
