# Partner Sentiment Tracker Skill

Use this skill when analyzing partner relationship health, detecting drift signals, and identifying re-engagement opportunities.

## When to trigger

- User says "is [partner] still engaged?", "relationship health check", "detect partner drift"
- User needs to surface early warning signals before a relationship becomes an incident
- Monthly or quarterly relationship health reviews
- Preparing for difficult conversations (e.g., timeline slip)
- Slash command `/partner-sentiment [partner-name]` or `/relationship-health [all]` is invoked

## What this skill does

Analyzes partner interaction history from:
1. `wiki/notes/` — conversation logs, feedback, sentiment cues
2. `data/partners.yaml` — partner metadata (status, last contact, commitment)
3. `wiki/log.md` — key decisions & timeline changes
4. `knowledge/eez/kb/03-ecosystem/` — context on partner role in alliance

Produces a **relationship health assessment** showing:
- Current sentiment (strong, stable, cautious, drift-risk, escalation-required)
- Engagement trend (improving, flat, degrading)
- Days since last contact (vs. expected cadence for partner tier)
- Communication quality signal (responsive, slow, unresponsive)
- Key blockers from their perspective
- Upcoming milestone or trigger point
- **Recommended next action** (no action, routine check-in, re-engagement, escalation)

## Required inputs

Before generating, confirm:
1. **Partner name** — must match a record in `data/partners.yaml`
2. **Scope** — single partner or "all" (all founding partners)
3. **Depth** — quick sentiment (1-minute) or detailed analysis (notes + recommendations)

If scope is "all", generate prioritized list (drift-risk partners first).

## Standard sentiment signals

### Engagement Health Indicators

**Strong ✅**
- Last contact <14 days ago (for tier-1 partners)
- Responsive to messages (reply within 48h)
- Proactive sharing of updates
- Regular participation in calls
- Clear timeline commitment

**Stable ✅**
- Last contact <30 days ago
- Expected cadence being met
- Responsive when contacted
- Working through milestones on schedule

**Cautious ⚠️**
- Last contact 30–45 days ago
- Slower response times (48–72h)
- Reduced proactivity (waiting for us to initiate)
- Vague or delayed on timeline commitments
- Fewer updates shared

**Drift Risk ⚠️**
- Last contact >45 days ago
- Delayed responses (>72h)
- Uncommitted responses ("still thinking", "depends on X")
- Timeline slips without explanation
- Reduced participation in calls
- Blocker unresolved for >2 weeks without progress

**Escalation Required 🔴**
- Last contact >60 days (unplanned)
- Not responding to outreach
- Public or private signal of reduced commitment
- Timeline slip is critical path
- Relationship feels compromised
- Technical or relationship blocker preventing progress

### Communication Quality Cues

Read from `wiki/notes/` interactions:
- **Responsive** — replies within 48h, engages substantively
- **Slow** — replies within 3–5 days, brief responses
- **Unresponsive** — >5 days, one-word replies, cancels calls
- **Proactive** — shares updates without prompting, asks questions
- **Reactive** — only responds when asked, minimal elaboration

### Sentiment Shift Detection

Compare last 3 interactions vs. baseline:
- "Was collaborative, now guarded" → sentiment degrading
- "Was skeptical, now asking implementation questions" → improving
- "Consistent energy, hitting milestones" → stable
- "Sudden silence after active period" → possible blocker

## Standard sentiment report structure

```
# [Partner] — Relationship Health Report

## Sentiment Summary
**Current Status:** [strong/stable/cautious/drift-risk/escalation-required]
**Trend:** [improving/flat/degrading]
**Risk Level:** [low/medium/high]

## Engagement Timeline
- Last contact: [date] ([X days ago])
- Expected cadence: [frequency] (e.g., every 30 days)
- Status: [on-track/overdue]

## Communication Quality
- Responsiveness: [responsive/slow/unresponsive]
- Proactivity: [proactive/reactive]
- Tone: [positive/neutral/guarded/tense]

## Recent Signals
- [Signal 1] — [date]
- [Signal 2] — [date]
- [Signal 3] — [date]

## Blockers From Their Perspective
- [Blocker 1] — [status: unresolved/being worked on/cleared]
- [Blocker 2]

## Upcoming Milestone or Trigger Point
- [Milestone] — [date]
- Relevance: [why this matters for sentiment]

## Recommended Next Action
1. **Primary:** [action] — [rationale]
2. **Timeline:** [when to do this] (e.g., "this week")
3. **Prepared by:** [skill execution date]

## Notes for Relationship Manager
- [Context for Ben or Ecosystem Growth Lead]
- [Anything unusual or worth flagging]
```

## Drift Detection Rules

Auto-flag partners for re-engagement if ANY of:
- Days since contact > expected cadence + 14 days
- Last 3 messages have degrading tone signal
- Responsive time increased >3x from baseline
- Unresolved blocker >2 weeks without progress updates
- Timeline slip announced without explanation
- Cancellation of scheduled call

## After generating

1. Save to `outputs/drafts/partner-sentiment-[name]-[date].md`
2. If status is "drift-risk" or "escalation-required", surface to Ben immediately
3. Recommended action is prepared but requires Ben sign-off before execution
4. Update `data/partners.yaml` with refreshed "last contact" date (Ben reviews)

## Common invocations

### "Quick sentiment check on Linea"
```
/partner-sentiment Linea
→ 1-minute sentiment assessment
→ Engagement trend + recommended action
→ Focus on: current status, days since contact, blocker status
```

### "Relationship health check on all partners"
```
/relationship-health all
→ All 20 partners scored
→ Prioritized by risk level (escalation-required, drift-risk, cautious first)
→ One-liner summary per partner
→ Actionable list for the month
```

### "Deep sentiment analysis for partner call"
```
/partner-sentiment Aave [detailed]
→ Full report with all signals
→ Context for difficult conversation
→ Suggestions for re-engagement
```

## Voice

- Clinical, not emotional ("drift-risk" not "they're ghosting us")
- Evidence-based (cite specific dates/signals)
- Actionable (clear recommendation, not vague concern)
- Curious, not accusatory (consider their context: timing, capacity, other priorities)

## Data quality & caveats

- If `wiki/notes/` has no recent entries for a partner, mark "Last Contact: Unknown" and flag for verification
- If sentiment is ambiguous, note "Unclear signal, recommend check-in" rather than guessing
- If blocker is external (e.g., "waiting for their board approval"), note that this is not relationship drift

## See also

- `Alliance Activation Matrix` — partner status across all dimensions
- `Re-Engagement Playbook` — how to re-engage partners showing drift
- `Partner Engagement Calendar` — what's happening in their world
- `Partner Feedback Synthesis` — aggregate feedback across cohort
