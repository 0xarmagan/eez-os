# EEZ Agent Project Structure

Complete navigation guide for the EEZ ecosystem project.

---

## Quick Navigation

```
eez-agent/
├── README.md                 ← Project overview
├── CLAUDE.md                 ← System instructions & context
├── STRUCTURE.md              ← This file
│
├── docs/                     ← Event summaries & transcripts
│   ├── EEZ-Community-Call-May13.md
│   ├── eez-community-call-twitter-thread.md
│   ├── eez-thread-audit.md
│   ├── eez-transcript-summary.md
│   └── eez-transcript-summary-readable.md
│
├── outputs/                  ← Symlink to canonical project location
│   └── drafts/ ────→ ../projects/ethereum-economic-zone/outputs/drafts/
│       ├── INDEX.md
│       ├── community-events/     (call scripts, moderator notes, QA)
│       ├── technical-deep-dives/ (Zisk, proving, architecture)
│       ├── planning-reference/   (governance, tokenomics, roadmaps)
│       └── media-ingestion/      (ingest staging area — pending KB promotion)
│
├── knowledge/                ← Core knowledge base
│   ├── eez/                  ← EEZ-specific materials
│   │   ├── 00-overview.md
│   │   ├── 01-comms-framework.md
│   │   ├── 02-alliance.md
│   │   ├── 03-technical-spec.md
│   │   ├── 04-launch-ethcc.md
│   │   ├── ZisK-Jordi-Baylina-Transcript.md
│   │   ├── kb/               ← Knowledge base (structured)
│   │   │   ├── 01-getting-started/
│   │   │   ├── 02-technical/
│   │   │   ├── 03-ecosystem/
│   │   │   ├── 04-media-and-analysis/
│   │   │   ├── 05-research-materials/
│   │   │   ├── 06-reports/
│   │   │   └── START-HERE.md
│   │   └── sources/          ← Primary sources & forum posts
│   │
│   ├── gnosis/               ← Gnosis context (products, DAO, governance)
│   │   ├── dao/
│   │   └── products/
│   │
│   ├── ecosystem/            ← Market & audience context
│   │   └── audience-and-landscape.md
│   │
│   ├── reference/            ← Reference materials
│   │   ├── brand/            ← Brand guidelines, visual identity, guardrails
│   │   ├── voice/            ← Tone, channels, messaging (X, blog, email, etc.)
│   │   ├── content-pillars.md
│   │   ├── messaging-and-narrative.md
│   │   ├── stakeholder-map.md
│   │   ├── style-guide.md
│   │   └── sourcing-policy.md
│   │
│   └── network-effect-dashboard.md
│
├── personas/                 ← Audience segmentation
│   ├── eez-audience-README.md
│   ├── eez-audience-personas.md
│   ├── builder.md
│   ├── partner.md
│   ├── internal.md
│   └── routing.md
│
├── skills/                   ← EEZ-specific Claude skills
│   └── gip-authoring/
│       └── SKILL.md
│
├── templates/                ← Reusable content templates
│   └── partner-outreach-email.md
│
├── workflows/                ← Standard processes
│   ├── new-partner-onboarding.md
│   └── content-ingestion.md
│
├── data/                     ← Reference data
│   ├── gips.yaml
│   └── partners.yaml
│
└── .claude/                  ← Claude Code configuration
    ├── settings.json
    └── commands/
        ├── eez-brief.md
        └── ingest.md
```

---

## Important: Consolidated Outputs

⚠️ **As of May 16, 2026:** All outputs/drafts are now stored in the canonical location:

```
eez-agent/outputs/drafts/  →  (symlink)  →  projects/ethereum-economic-zone/outputs/drafts/
```

**Why?** Single source of truth. Agent generates drafts, human approves them in place, then moves to `outputs/approved/`. No duplication.

**Workflow:**
1. Agent writes draft → `../projects/.../outputs/drafts/[category]/`
2. You review → approve or iterate
3. Approved drafts → move to `../projects/.../outputs/approved/`
4. Log decision → `../projects/.../wiki/log.md`

---

## By Use Case

### 🚀 Starting a New Task

**Getting oriented?** Start here:
1. `README.md` — Project overview
2. `knowledge/eez/kb/START-HERE.md` — Quick facts, key people, overview
3. `personas/eez-audience-README.md` — Who are we talking to?

### 📝 Writing Community Content

**Creating blog posts, threads, emails?**
1. `knowledge/reference/voice/` — Tone, brand voice, channels
2. `knowledge/reference/brand/` — Brand guidelines, visual identity
3. `knowledge/reference/messaging-and-narrative.md` — Key narratives
4. `templates/` — Reusable templates
5. `outputs/drafts/community-events/` — Past examples

### 🔧 Technical Content

**Explaining architecture or proving systems?**
1. `knowledge/eez/03-technical-spec.md` — Overall spec
2. `knowledge/eez/kb/02-technical/` — Deep technical docs
3. `outputs/drafts/technical-deep-dives/` — Public explanations
4. `knowledge/eez/sources/` — Primary technical sources

### 🤝 Partner Outreach

**Reaching out to alliance members?**
1. `knowledge/gnosis/` — Gnosis context & products
2. `personas/partner.md` — Partner segment profile
3. `templates/partner-outreach-email.md` — Email template
4. `workflows/new-partner-onboarding.md` — Onboarding process
5. `data/partners.yaml` — Partner data

### 🎙️ Event Planning

**Running a community call or panel?**
1. `outputs/drafts/community-events/` — Past event scripts
2. `personas/` — Audience segments
3. `knowledge/eez/kb/04-media-and-analysis/` — Call summaries & transcripts
4. `outputs/drafts/planning-reference/` — Event planning docs

### 📊 Strategic Analysis

**Making high-level decisions?**
1. `knowledge/eez/kb/06-reports/` — Executive summaries & reports
2. `knowledge/eez/kb/05-research-materials/` — Research & strategic docs
3. `knowledge/network-effect-dashboard.md` — Network metrics
4. `knowledge/reference/stakeholder-map.md` — Stakeholder landscape

---

## Content Types

### Docs (Event-Sourced)
Final, archival materials from events.
- **Location:** `docs/`
- **Examples:** Call transcripts, summaries, thread audits
- **Update Frequency:** Post-event (rare updates)

### Outputs/Drafts (Work-in-Progress)
Live working materials organized by type.
- **Location:** `outputs/drafts/`
- **Examples:** Scripts, segment plans, content outlines
- **Update Frequency:** Weekly (event-driven)

### Knowledge (Reference)
Canonical, well-researched materials for reuse.
- **Location:** `knowledge/`
- **Examples:** Technical specs, messaging frameworks, brand guidelines
- **Update Frequency:** Monthly (curriculum-based)

### Templates (Reusable)
Standard formats for common tasks.
- **Location:** `templates/`
- **Examples:** Email templates, proposal outlines, GIP drafts

---

## Key Decisions

### Three-Tier Communication (Knowledge → Outputs → Docs)

1. **Knowledge Tier** — Research, spec, frameworks (input)
2. **Outputs Tier** — Drafts, scripts, iterations (work)
3. **Docs Tier** — Finalized, archived (output)

Flow: `knowledge/eez/03-technical-spec.md` → `outputs/drafts/technical-deep-dives/Zisk-Real-Time-Proving-Summary.md` → `docs/EEZ-Community-Call-May13.md`

### Semantic Organization (Not Chronological)

Folders are named by **purpose** (community-events, technical-deep-dives, planning-reference), not date. This makes discovery easier and scales as more materials accumulate.

### Audience-First Routing

`personas/routing.md` maps audience segments → persona → recommended content/contact. Use this when deciding what to send where.

---

## Extending the System

### Adding a New Resource Type

1. Choose the tier: Knowledge (canonical), Outputs (draft), or Docs (archived)
2. Choose the category: `eez/` (EEZ-specific), `gnosis/` (context), or `reference/` (reference)
3. Create the file with a clear, semantic name
4. Update this STRUCTURE.md if it's a new major category

### Adding a New Persona

1. Create `personas/[persona-name].md` with audience segment definition
2. Add to `personas/routing.md` with recommended channels & content
3. Update audience-specific content

### Adding a New Workflow

1. Create `workflows/[process-name].md` with step-by-step instructions
2. Link from relevant templates or docs
3. Test with a real-world scenario

---

## Statistics

| Category | Files | Status |
|---|---|---|
| Knowledge Base | 79 | ✅ Active |
| Outputs/Drafts | 18 (3 categories) | ✅ Active |
| Docs (Archived) | 5 | ✅ Stable |
| Personas | 7 | ✅ Complete |
| Templates | 1 | 🚧 Minimal |
| Workflows | 2 | ✅ Growing |
| Skills | 2 (GIP, partner-brief) | ✅ Functional |

---

## Version History

- **2026-05-14** — Restructured `outputs/drafts/` into semantic categories (community-events, technical-deep-dives, planning-reference)
- **2026-05-14** — Created comprehensive STRUCTURE.md navigation guide
- **2026-04-27** — Initial project setup with knowledge base, personas, templates, workflows

---

*Last updated: 2026-05-14*
