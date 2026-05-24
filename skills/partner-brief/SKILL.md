# Partner Brief Skill

Use this skill when producing partner-facing materials: outreach emails, briefs, talking points, co-marketing asks.

## When to trigger

- User says "draft a partner brief", "outreach to [partner]", "co-marketing ask for [partner]"
- Slash command `/eez-brief` or `/partner-outreach` is invoked
- User is preparing for a partner call

## Required inputs

1. **Partner name** — must match a record in `data/partners.yaml`, or be flagged as new
2. **Purpose** — onboarding, amplification ask, integration check-in, escalation, recovery from drift?
3. **Channel** — email, Telegram DM, Slack, in-person follow-up?
4. **Specific ask** — what does Ben want from them? What does EEZ offer in return?

## Brief template (1-pager)

```
# [Partner] — Brief for [Purpose]

## Status
Founding / prospective / dormant. Last contact: [date]. Sentiment: [signal].

## Why this partner matters
1-2 sentences on their strategic fit. What they amplify. What they integrate.

## Current commitments
What they've signed up for: brand QT, founder note, video, integration, etc.

## Current asks
What we want next.

## Their asks of us
What they need from EEZ. Visibility, intros, technical access.

## Voice notes
Their style, who internally drives the relationship, sensitivities.

## Suggested next action
One concrete next step.
```

## Outreach email template (use templates/partner-outreach-email.md)

The two-asks pattern (brand-account QT + personal note from a founder) has worked well in past launches. For new partners, lead with the alliance pitch (private, builder-only, four areas of contribution) and the Tally form.

## Voice rules for partner-facing material

Apply the partner persona voice (`personas/partner.md`):
- Professional, peer-to-peer
- No Gnosis-internal opinions
- Concrete asks, concrete offers
- No talking down
- Match the partner's register

## After drafting

1. Save to `outputs/drafts/partner-[name]-[purpose]-[date].md`
2. Pass to Ben for review before sending
3. Never send partner-facing material directly; this requires explicit Ben go-ahead

## Reference: past patterns that worked

- **EthCC two-asks pattern**: brand account QT on launch day, personal note from founder in following days. Plus: ask if they'll be at the event for an interview. Worked with Linea, Nethermind, Octant, Centrifuge, Blockscout, GrowThePie.
- **Alliance overview message**: short, ends with the Tally form, explicitly states private-to-builders.
- **Drop the em dashes**: Ben asks for this on every draft. Apply by default.
