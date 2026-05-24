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

---

## Pre-Draft Step (mandatory for content and co-marketing modes)

Before writing a single word of copy:

1. **Read the partner or campaign's own website and comms guidelines.** Not the PDF summary — the actual site. Their language, their framing, their philosophy.
2. **Check if they have explicit comms guidelines** (e.g. "ecosystem-first, no single team as owner"). If yes, apply them. If they conflict with EEZ's voice, flag the tension before drafting.
3. **Identify EEZ's genuine position on the topic** — not a reaction to their framing, but EEZ's actual stance expressed in EEZ's voice.
4. **Never borrow the partner's language.** If their tagline is "If we build together, we win together" — that's their line. Find EEZ's equivalent position in EEZ's words.

Skipping this step is what produces generic campaign-speak. Do the research first.

---

## EEZ Voice — Embedded Reference

**Note:** `knowledge/reference/voice/eez.md` and `knowledge/reference/voice/channels/x.md` are currently empty (TODO). Use this section as the authoritative voice reference until those files are populated.

**Register:** Satoshi x Deadpool (Gnosis parent) / Sage x Everyman (EEZ). The expert who doesn't perform expertise. Laid-back, clear, grounded. Complexity made easy. Names the problem directly. Dry delivery. Takes a position — doesn't just endorse.

**The test:** Does it sound like Martin Köppelmann on Bankless or a core dev on All Core Devs? Matter-of-fact, builder-credible, no hype. If it sounds like a campaign press release, rewrite it.

**Structure on X:**
- Short sentences. One idea per sentence.
- Lead with the insight or the problem, not the endorsement.
- Position first, then support. "Every proprietary intent protocol is just another island. OIF is building it right." — not "We're excited to support OIF."
- One CTA at the end only. Max ~5 tweets.

**Canonical EEZ vocabulary — use these:**
- "reverse fragmentation" (not "fix" or "solve" — those imply a patch; "reverse" implies undoing damage)
- "One Ethereum" — the mission statement
- "not a hundred islands" — the fragmentation framing
- "synchronous composability" — the technical differentiator
- "same block, no bridges" — what synchronous composability means in plain terms
- "credibly neutral" — governance framing

**Vocabulary to avoid:**
- "fix fragmentation" — too patch-like
- Campaign slogans borrowed from partners
- "unlock value", "next-generation", "game-changer"
- Any line that sounds like it came from the partner's brief

**Ecosystem campaign participation (e.g. OIF, EF campaigns):**
When EEZ joins an ecosystem-wide campaign as a QT participant:
- The branded asset does the brand identity work. The tweet does the voice work.
- Express EEZ's genuine position on the topic — don't echo the campaign's own language back at them.
- If their comms guidelines say "no single team as owner" — take a position without claiming a layer. "Every proprietary intent protocol is just another island. OIF is building it right." Names the problem, endorses the solution, claims nothing.
- Check if EEZ/Gnosis is already listed as a named supporter — this changes the framing (participating vs endorsing from outside).

**Fallback voice sources (when voice files are empty):**
1. `knowledge/eez/01-comms-framework.md` — five messaging pillars, partner framings, banned phrases
2. `knowledge/reference/messaging-and-narrative.md` — "Everyone says / We say" pairs, canonical one-liners, product voice
3. `knowledge/reference/brand/eez-guardrails.md` — what not to say

---

## Modes

**`first-contact`** — Initial alliance pitch. Pull partner's chain/protocol focus from `data/partners.yaml` and tailor to what EEZ specifically unlocks for them. Private, builder-only framing. End with Tally form CTA. Tone: peer-to-peer, no corporate formality.

**`check-in`** — 3–5 sentences max. Reference the last interaction or specific commitment from `data/partners.yaml`. One concrete question or nudge. No long recap.

**`co-marketing`** — Run pre-draft step first. Outputs two things: (1) a brief for the partner covering what to post, when, and what EEZ does in parallel; (2) internal asset checklist. Covers message alignment, posting sequence, amplification window, and what each party provides.

**`event-coordination`** — Pre-event: who to meet, specific ask, what EEZ offers in return. During: side event logistics, interview or panel requests. Post: follow-up referencing the in-person moment by name.

**`recovery`** — Re-engaging a drifting partner. Tone calibrated to drift severity: light (just quiet, no pressure) vs serious (missed commitments, direct but not accusatory). Lead with value. Never guilt.

**`content`** — Run pre-draft step first. Partner-related content for public channels. Specify channel; apply voice rules above.

---

## Content Mode

### X Threads
- Run pre-draft step. Read the campaign/partner site before writing.
- Short punchy paragraphs. No bullet lists. No em dashes.
- Lead with the position or the problem — not the endorsement.
- First tweet: EEZ's stance on the topic. Readers don't need to know what EEZ is to find it interesting.
- One CTA at the end only.
- Max ~5 tweets.

### Announcements (partner joins, integration live, milestone)
- Lead with the fact: "X is now part of the EEZ Alliance." Not "We're thrilled to welcome..."
- Second sentence: why this partner, what they bring
- Third: what this means for builders or the ecosystem
- End with one concrete next step or link
- Length by channel: X thread (5 tweets), LinkedIn (200–300 words), blog (400–600 words)

### Co-marketing Content (joint campaigns, ecosystem campaigns)
- Run pre-draft step. Read their comms guidelines.
- Produce two versions: EEZ-voice (for EEZ accounts) and a partner-voice brief (for partner to adapt)
- Message sync check: does the partner copy match EEZ's framing? Flag divergences.
- Amplification sequence: who posts first, what the other amplifies, timing window
- Asset checklist: EEZ provides (logo, boilerplate, quote from Jordi/Armagan) vs partner provides

### Event Content (recaps, side event posts, live coverage)
- Recap: hook (what happened), 2–3 key moments, one quote if available, CTA/link
- Live coverage: short, first-person, real-time. Not polished prose.
- Post-event thread: what was announced, who was there, what comes next

---

## Data Sources

| Source | Status | Used for |
|---|---|---|
| `data/partners.yaml` | ✅ Active | Partner profile, status, commitments, channel, contacts |
| `knowledge/eez/01-comms-framework.md` | ✅ Active | Messaging pillars, positioning, banned framings |
| `knowledge/reference/messaging-and-narrative.md` | ✅ Active | EEZ/Gnosis voice, one-liners, contrarian pairs |
| `knowledge/reference/brand/eez-boilerplate.md` | ✅ Active | Standard EEZ copy blocks |
| `knowledge/reference/brand/eez-guardrails.md` | ✅ Active | What not to say |
| `knowledge/reference/style-guide.md` | ✅ Active | Voice rules, banned phrases |
| `templates/partner-outreach-email.md` | ✅ Active | Outreach patterns |
| `personas/partner.md` | ✅ Active | Partner persona voice |
| `knowledge/reference/voice/channels/x.md` | ⚠️ Empty | Use EEZ Voice section above instead |
| `knowledge/reference/voice/eez.md` | ⚠️ Empty | Use EEZ Voice section above instead |

---

## Output

All drafts saved to `outputs/drafts/alliance-outreach-[partner]-[mode]-[date].md`. Never sent directly — always requires Armagan's go-ahead.

## Rules

- No em dashes. No "excited to announce." No "leverage" as a verb. Active voice.
- British English when writing as Armagan.
- Always check `data/partners.yaml` for partner's preferred channel and contact before drafting.
- Never invent partner quotes or commitments. Use `[PLACEHOLDER — confirm with partner]` if uncertain.
- Never borrow the partner's campaign language. Find EEZ's position in EEZ's words.
- The two-asks pattern (brand QT + personal note from founder) works well for launch amplification.
- When voice files are empty, fall back to `knowledge/eez/01-comms-framework.md` and `knowledge/reference/messaging-and-narrative.md`.
