# Alliance Activation Matrix Skill

Use this skill when generating or updating the live tracker that shows partner commitments, timelines, blockers, and status across the EEZ founding alliance.

## When to trigger

- User says "generate alliance matrix", "update activation matrix", "partner status report"
- User needs a snapshot of where all ~20 partners stand
- Weekly or monthly reporting to leadership
- Identifying which partners are at risk
- Slash command `/alliance-matrix [period]` is invoked

## What this skill does

Reads from canonical data sources:
1. `data/partners.yaml` — partner contact, commitment, status, timeline, owner
2. `wiki/notes/` — recent interaction logs and decision history
3. `projects/.../04-roles/` — partner-specific ownership assignments
4. `knowledge/eez/kb/03-ecosystem/Alliance-Members.md` — alliance structure

Synthesizes into a **live status table** showing:
- Partner name
- Product commitment (what they're integrating)
- Target timeline
- Responsible engineer (owner)
- Current status (spec review → testnet → mainnet-ready)
- Days since last contact
- Known blockers (technical, operational, relationship)
- Next milestone
- Action required (yes/no)

## Required inputs

Before generating, confirm:
1. **Period** — "current snapshot", "as of May 16", "monthly update"
2. **Format** — table (for spreadsheet import), markdown (for wiki), or detailed (with notes per partner)
3. **Scope** — all partners, or specific subset (e.g., "founding cohort only", "dormant partners")
4. **Owner filter** (optional) — "who is responsible for partner X?" for escalation tracking

If missing, ask once before generating.

## Standard matrix structure

```
| Partner | Commitment | Target Timeline | Owner | Status | Last Contact | Blockers | Next Milestone | Action |
|---------|-----------|-----------------|-------|--------|--------------|----------|-----------------|--------|
| [Name]  | [Product] | [Date]          | [Name]| [Stage]| [Days ago]   | [List]   | [Milestone]     | [Y/N]  |
```

**Status stages:**
- spec-review (reading & feedback on spec)
- testnet-prep (planning integration)
- testnet-live (integrated on testnet)
- mainnet-ready (passed all checks, ready for mainnet)
- mainnet-live (active on mainnet)
- dormant (relationship paused)
- escalated (needs leadership attention)

**Blockers examples:**
- Technical (e.g., "waiting for L1 bridge contract", "client not ready")
- Operational (e.g., "team capacity constrained", "roadmap priorities")
- Relational (e.g., "communication drift", "unclear next step")
- Timing (e.g., "can't integrate until after their mainnet launch")

## Voice & tone

- Matter-of-fact, no marketing language
- Accurate status (if unknown, mark "unconfirmed")
- Flag action items explicitly ("Action: Re-engage Linea re: timeline")
- Surface early warning signals (e.g., "47 days since contact, expected cadence is 30")

## After generating

1. Save to `outputs/drafts/alliance-matrix-[period]-[date].md`
2. Review with the Ecosystem Growth Lead before sharing with leadership
3. Flag any partners that require immediate re-engagement or escalation
4. Update `data/partners.yaml` if new information discovered (Ben reviews before committing)

## Common patterns

### "Show me all partners at risk"
Matrix filtered to:
- Days since contact > expected cadence
- Status = "dormant" or "escalated"
- Blockers = unresolved for >2 weeks
- Action = yes

### "Monthly partner health check"
Full matrix ordered by:
1. Partners needing action (top)
2. By timeline urgency (soonest first)
3. Include trend signal (improving, stable, degrading)

### "Prepare for leadership sync"
Executive summary:
- Count by status stage
- Count by blocker type
- Top 3 risks
- Top 3 wins
- Recommended actions

## Data sources to cross-reference

- **partners.yaml** — source of truth for partner records
- **wiki/notes/** — search for partner name to find recent interactions
- **wiki/log.md** — key decisions & pivots that affected timeline
- **projects/.../04-roles/** — ownership assignments and responsible engineers
- **knowledge/eez/kb/03-ecosystem/** — context on what each partner amplifies

## If data is missing or conflicting

- If `wiki/notes/` has no recent entry, mark "Last Contact: Unknown" and flag for verification
- If two sources disagree on timeline, note both and escalate to Ben
- If blocker is unconfirmed, mark as "Reported by [source]" not as fact

---

**Example invocation:**

```
/alliance-matrix "as of May 16"
→ Reads current partner data + recent wiki interactions
→ Generates full matrix with blockers
→ Flags 3 partners for re-engagement (>30 days without contact)
→ Saves to outputs/drafts/alliance-matrix-may16-snapshot.md
```

---

## See also

- `Partner Sentiment Tracker` — deeper analysis of relationship health
- `Partner Engagement Calendar` — upcoming events & milestones
- `Re-Engagement Playbook` — when a partner shows drift signals
- `new-partner-onboarding` workflow — for partners just onboarded
