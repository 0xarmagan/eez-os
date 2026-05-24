---
name: alliance-outreach
description: Use when drafting any partner-facing communication or partner-related content — outreach emails, check-ins, co-marketing briefs, X threads, announcements, event coordination, or re-engagement of drifting partners.
---

# Alliance Outreach Skill

All partner-facing communication and partner-related content in one skill. Context-aware by mode — same entry point whether you are writing an outreach email or an X thread about a co-marketing campaign.

## Trigger

- "draft outreach to [partner]", "co-marketing brief for [partner]"
- "X thread about [partner/milestone]", "content for [campaign/launch/event]"
- "check in with [partner]", "re-engage [partner]", "recover [partner]"
- "draft [partner] announcement", "post about [milestone]"
- `/alliance-outreach [partner] [mode]`

## Modes

**`first-contact`** — Initial alliance pitch. Pull partner's chain/protocol focus from `data/partners.yaml` and tailor to what EEZ specifically unlocks for them. Private, builder-only framing. End with Tally form CTA. Tone: peer-to-peer, no corporate formality.

**`check-in`** — 3–5 sentences max. Reference the last interaction or specific commitment from `data/partners.yaml`. One concrete question or nudge. No long recap, no summary of everything that's happened.

**`co-marketing`** — Outputs two things: (1) a brief for the partner covering what to post, when, and what EEZ does in parallel; (2) an internal asset checklist. Covers message alignment, posting sequence, amplification window, and what each party provides.

**`event-coordination`** — Pre-event: who to meet, specific ask, what EEZ offers in return. During: side event logistics, interview or panel requests. Post: follow-up referencing the in-person moment by name.

**`recovery`** — Re-engaging a drifting partner. Tone calibrated to drift severity: light (just quiet, no pressure) vs serious (missed commitments, direct but not accusatory). Lead with value. Never guilt.

**`content`** — Partner-related content for public channels. Specify channel; skill applies the right voice rules. Detail below.

---

## Content Mode

### X Threads
- First tweet: always include EEZ context for readers who don't know what EEZ is
- Short punchy paragraphs. No bullet lists. No em dashes.
- Informative over clickbait — explain the actual thing
- One CTA at the end only (follow / read / apply / attend)
- Max ~5 tweets. If it needs more, it's two separate threads.
- Source: `knowledge/reference/voice/channels/x.md`

### Announcements (partner joins, integration live, milestone)
- Lead with the fact: "X is now part of the EEZ Alliance." Not "We're thrilled to welcome..."
- Second sentence: why this partner, what they bring
- Third: what this means for builders or the ecosystem
- End with one concrete next step or link
- Length by channel: X thread (5 tweets), LinkedIn (200–300 words), blog (400–600 words)

### Co-marketing Content (joint posts, shared campaigns)
- Produce two versions: EEZ-voice (for EEZ accounts) and a partner-voice brief (for partner to adapt)
- Message sync check: does the partner copy match EEZ's framing on synchronous composability? Flag divergences.
- Amplification sequence: who posts first, what the other amplifies, timing window (same-day vs 24h)
- Asset checklist: EEZ provides (logo, boilerplate, quote from Jordi/Ben) vs partner provides

### Event Content (recaps, side event posts, live coverage)
- Recap: hook (what happened), 2–3 key moments, one quote if available, CTA/link
- Live coverage: short, first-person, real-time. "At [event], [thing happened]." Not polished prose.
- Post-event thread: what was announced, who was there, what comes next

---

## Data Sources

| Source | Used for |
|---|---|
| `data/partners.yaml` | Partner profile, status, commitments, preferred channel, contacts |
| `knowledge/eez/01-comms-framework.md` | Messaging pillars, positioning |
| `knowledge/reference/voice/channels/x.md` | X format and tone rules |
| `knowledge/reference/voice/channels/linkedin.md` | LinkedIn format |
| `knowledge/reference/brand/eez-boilerplate.md` | Standard EEZ copy blocks |
| `knowledge/reference/brand/eez-guardrails.md` | What not to say |
| `knowledge/reference/style-guide.md` | Voice rules, banned phrases |
| `templates/partner-outreach-email.md` | Outreach patterns, two-asks pattern |
| `personas/partner.md` | Partner persona voice |

## Output

All drafts saved to `outputs/drafts/alliance-outreach-[partner]-[mode]-[date].md`. Never sent directly — always requires Ben's go-ahead.

## Rules

- No em dashes. No "excited to announce." No "leverage" as a verb. Active voice.
- British English when writing as Ben.
- Always check `data/partners.yaml` for partner's preferred channel and contact before drafting.
- Never invent partner quotes or commitments. Use `[PLACEHOLDER — confirm with partner]` if uncertain.
- The two-asks pattern (brand QT + personal note from founder) works well for launch amplification — apply by default for EthCC-style moments.
