# Using EEZ Agent + Project Folder Effectively

A guide to coordinating two complementary EEZ systems.

---

## The Two Systems

### 1. `projects/ethereum-economic-zone/` — Strategic Project Folder (108M)

**Purpose:** Canonical source of truth for EEZ strategic work, brand, market research, and decision logs.

**What it contains:**
- **01-brand-foundation/** — Positioning platform, tone of voice, brand governance (canonical)
- **02-go-to-market/** — Content strategy, audience research framework, strategic plans
- **03-creative/** — Creative briefs, storyboards, use-case animations
- **04-roles/** — Role specifications (Head of Ecosystem Relations)
- **EEZ-Knowledge-Hub/** — 12-file research synthesis (overview, architecture, alliance, market, strategy, scenarios, sources, quotes, timeline, open questions)
- **wiki/** — Operational log (decisions, notes, sources, entities, concepts, meta)
- **CLAUDE.md** — Project schema (defines structure for automated agents)

**You use this when:** Making strategic decisions, reviewing past decisions, researching technical/market context, designing new initiatives.

**Update frequency:** Weekly (strategic decisions, partner updates, research additions)

---

### 2. `eez-agent/` — Claude Code Specialist Agent (1.3M)

**Purpose:** An automated agent designed to run in Claude Code, using the project folder as its knowledge base.

**What it contains:**
- **CLAUDE.md** — Agent identity and operating instructions
- **knowledge/eez/** — Copy/extension of strategic materials (references project folder as canonical)
- **outputs/drafts/** — Agent's work-in-progress (scripts, briefs, planning docs)
- **personas/** — Audience segment definitions for routing
- **skills/** — Claude skills (GIP authoring, partner brief)
- **workflows/** — Reusable multi-step processes

**You use this when:** Automating EEZ tasks (partner emails, GIP drafting, event scripts, announcements).

**Update frequency:** Daily (task-driven, event-sourced)

---

## The Relationship

```
projects/ethereum-economic-zone/
  ├─ wiki/ (decisions, notes, meta-learning)
  ├─ EEZ-Knowledge-Hub/ (canonical research)
  ├─ 01-brand-foundation/ (canonical brand)
  ├─ 02-go-to-market/ (canonical strategy)
  └─ CLAUDE.md (project schema)
       ↑
       │ references & extends
       │
eez-agent/
  ├─ knowledge/eez/
  │   └─ kb/ (structured access to project materials)
  ├─ outputs/drafts/
  │   └─ (agent-generated drafts, sent back to project folder)
  ├─ CLAUDE.md (agent identity)
  └─ skills/ (automation layer)
       ↓
       │ serves
       │
     YOU (in Claude Code)
```

---

## Division of Labor

### What Belongs in `projects/ethereum-economic-zone/`

✅ **Keep here (canonical):**
- Strategic decisions and their rationale
- Brand guidelines, positioning, tone of voice
- Market research, audience analysis
- Product/technical specifications
- Historic decision logs and lessons learned
- Meeting notes, external communications received
- Role definitions, organizational structure

❌ **Don't put here:** Work-in-progress drafts, temporary files, quick iterations

---

### What Belongs in `eez-agent/`

✅ **Keep here (agent corpus & outputs):**
- Structured knowledge (kb/ subdirectories organized for agent lookup)
- Agent-generated drafts (scripts, emails, briefs, announcements)
- Persona definitions and routing logic
- Automation skills and workflows
- Agent operational log and metadata

❌ **Don't put here:** Strategic decisions, brand decisions, meeting notes (those go to project folder)

---

## Workflow: Making This Work

### Scenario 1: You Need to Draft a Partner Email

**Step 1:** Check the canonical source
```
projects/ethereum-economic-zone/02-go-to-market/
→ EEZ-Audience-Research-Framework.md (who is the partner?)
→ 01-brand-foundation/EEZ-Tone-of-Voice-Guide.md (how do we sound?)
→ wiki/entities/ (who have we contacted before?)
```

**Step 2:** Ask eez-agent to draft it
```
In Claude Code, invoke: /partner-outreach [partner-name] [ask]
→ Agent reads eez-agent/knowledge/eez/ (cached copy of brand + audience)
→ Agent uses templates/ + skills/partner-brief/
→ Produces draft in eez-agent/outputs/drafts/partner-outreach/
```

**Step 3:** Human review & approval
```
Review in eez-agent/outputs/drafts/ → decide to send
→ Copy final version to projects/ethereum-economic-zone/wiki/notes/
→ Log decision in projects/.../wiki/log.md
```

---

### Scenario 2: You Need to Update Brand Guidance

**Step 1:** Update canonical source
```
projects/ethereum-economic-zone/01-brand-foundation/
→ Edit EEZ-Tone-of-Voice-Guide.md
→ Update CLAUDE.md if structure changes
→ Log decision in wiki/log.md
```

**Step 2:** Sync to agent (periodic)
```
Copy updated materials to eez-agent/knowledge/eez/
→ Update eez-agent/CLAUDE.md if needed
→ Next agent invocation reads fresh copies
```

---

### Scenario 3: You Need to Plan a Community Call

**Step 1:** Check past decisions & materials
```
projects/ethereum-economic-zone/wiki/
→ Check notes/ for past call learnings
→ Check log.md for decisions
```

**Step 2:** Use agent to draft materials
```
In Claude Code: /eez-event-plan [date] [audience]
→ Agent reads eez-agent/knowledge/eez/
→ Produces draft scripts, moderator questions in outputs/drafts/community-events/
```

**Step 3:** Finalize & archive
```
Review & approve → copy final to projects/.../wiki/notes/
→ Finalize docs/ subfolder with approved transcript/summary
```

---

## Data Sync Strategy

### Canonical Source (Project Folder)

These should **never** be duplicated:
- Strategic decisions (wiki/log.md)
- Brand guidelines (01-brand-foundation/)
- Organizational structure (04-roles/)
- Market research (02-go-to-market/)

**Rule:** Edit in project folder, then sync to agent.

### Agent Corpus (eez-agent/)

These can be **local copies** for performance:
- `eez-agent/knowledge/eez/kb/` — Structured copy of research
- `eez-agent/personas/` — Audience definitions
- `eez-agent/templates/` — Email/brief templates

**Rule:** Refresh monthly or when strategic doc changes.

### Agent Outputs (eez-agent/outputs/drafts/)

**Do not sync back to project folder automatically.** Instead:
1. Agent generates draft in `outputs/drafts/[category]/`
2. Human reviews and approves
3. Human copies final version to `projects/.../wiki/notes/` if archival is needed
4. Human logs decision in `projects/.../wiki/log.md`

---

## Sync Checklist

**Monthly (or after major strategic updates):**

```
[ ] Brand updates? projects/→ eez-agent/knowledge/eez/
    - 01-brand-foundation/ → knowledge/eez/
    - Update eez-agent/CLAUDE.md if structure changed
    
[ ] Audience/personas changed? projects/→ eez-agent/
    - 02-go-to-market/ audience → eez-agent/personas/
    
[ ] New strategic decisions? projects/→ agent
    - wiki/log.md decisions → knowledge/eez/kb/
    - Update eez-agent/knowledge/eez/04-launch-ethcc.md if roadmap changed
    
[ ] New sources or research? projects/→ agent
    - EEZ-Knowledge-Hub/ → eez-agent/knowledge/eez/kb/05-research-materials/
    
[ ] Template updates? projects/→ agent
    - templates/ → eez-agent/templates/
    - Update skill definitions if format changed
```

---

## File Naming Convention (Avoid Conflicts)

**Project Folder:** `EEZ-[Type]-[Topic]-[Version].md`
- Examples: `EEZ-Content-Strategy.md`, `EEZ-Positioning-Platform.docx`

**Agent Outputs:** `[Topic]-[Type]-[Date].md`
- Examples: `Partner-Outreach-Acme-May14.md`, `Call-Script-EthCC-May15.md`

This prevents accidental overwrites.

---

## Quick Reference

| I need to… | Go to | Then |
|---|---|---|
| **Make a strategic decision** | `projects/.../wiki/log.md` | Log it, then sync knowledge to agent |
| **Check what we decided before** | `projects/.../wiki/log.md` | Read the decision log |
| **Update brand/messaging** | `projects/.../01-brand-foundation/` | Update canonical, then sync to agent |
| **Draft a partner email** | Claude Code: `/partner-outreach` | Review in `eez-agent/outputs/drafts/` |
| **Plan a community event** | Claude Code: `/eez-event-plan` | Review in `eez-agent/outputs/drafts/community-events/` |
| **Research EEZ context** | `projects/.../EEZ-Knowledge-Hub/` | Read the canonical synthesis |
| **Understand past decisions** | `projects/.../wiki/meta/` | Check meta-learning and decision rationale |
| **Find who we've contacted** | `projects/.../wiki/entities/` | Cross-reference contacts and conversations |

---

## Anti-Patterns to Avoid

❌ **Don't edit brand in both places**
- Edit in `projects/`, sync to `eez-agent/`
- NOT: make different edits in both

❌ **Don't use eez-agent outputs as canonical decisions**
- Draft in agent ✓
- Approve in human review ✓
- Log decision in project folder ✓
- NOT: treat agent draft as final until logged

❌ **Don't let knowledge drift**
- Sync monthly ✓
- Update eez-agent when strategic docs change ✓
- NOT: let agent corpus become stale

❌ **Don't duplicate long strategic documents**
- Keep in `projects/`
- Reference in `eez-agent/`
- NOT: maintain two copies in sync

---

## Version Control

Both folders are in git, but with different commit cadences:

**projects/ethereum-economic-zone/**
- Commit on strategic decisions (weekly)
- Message: "Decide: [decision name]" or "Update: [doc name]"

**eez-agent/**
- Commit on task completions (daily)
- Message: "Draft: [artifact type]" or "Sync: [doc name from project]"

**Multi-expert-intelligence-skills-main/**
- Commit both in one go if they're related
- Message: "Strategy+Agent: [topic] — strategic decision + agent sync"

---

## Getting Started

**Week 1:** Set up sync
- [ ] Review both folder structures
- [ ] Copy key strategic docs to eez-agent/knowledge/eez/
- [ ] Update eez-agent/CLAUDE.md with canonical sources
- [ ] Test: invoke `/partner-outreach [test-partner]`

**Week 2+:** Establish rhythm
- [ ] Use project folder for strategic work
- [ ] Use agent for automation
- [ ] Sync monthly or on strategic changes
- [ ] Log decisions in project wiki

---

## Examples

### Example: New Partner Added

1. **Add to project folder:**
   ```
   projects/ethereum-economic-zone/
   → wiki/entities/[partner-name].md (partner profile)
   → 02-go-to-market/[industry]/[partner].md (outreach strategy)
   → wiki/log.md (logged decision to onboard)
   ```

2. **Sync to agent:**
   ```
   eez-agent/knowledge/eez/kb/03-ecosystem/Alliance-Members.md
   → Add partner with link to project folder profile
   
   eez-agent/personas/partner.md
   → Add this partner to the routing table
   ```

3. **Invoke agent:**
   ```
   Claude Code: /partner-outreach [partner] "introduce EEZ"
   → Agent generates draft in eez-agent/outputs/drafts/partner-outreach/
   → You review and send
   → Log in project wiki/log.md
   ```

---

## Questions?

- **"Where should I put X?"** → Is it a strategic decision? → Project folder. Is it a draft or automation output? → Agent folder.
- **"My agent doesn't know Y."** → Sync the project folder knowledge to eez-agent/ and re-invoke.
- **"Should I edit in both?"** → No. Edit canonical source (project), then sync to agent.

---

*Last updated: 2026-05-14*
