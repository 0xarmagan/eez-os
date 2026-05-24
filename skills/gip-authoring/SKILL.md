# GIP Authoring Skill

Use this skill when drafting a GnosisDAO Improvement Proposal (GIP) or any forum post that follows the GIP template.

## When to trigger

- User says "draft a GIP" or "start a GIP"
- User asks to write a forum post following GIP structure
- User wants to formalise a proposal for governance vote
- Slash command `/gip-draft` is invoked

## Required inputs

Before drafting, confirm:
1. **Topic and scope** — what is being proposed?
2. **GIP category** — funding, governance, technical, operational? (Categories are listed on forum.gnosis.io)
3. **Co-authors** — anyone listed alongside Ben?
4. **Urgency** — is this a draft for community discussion or a near-final ready for phase-2?
5. **Sign-off chain** — who must review before posting? Default: relevant product lead for technical claims, Friederike + Adz for legal/comms, Ben for everything else.

If any of these are missing, ask once before drafting.

## Standard GIP structure

```
# GIP-XXX: [Title]

## Abstract
2-3 sentences. What is being proposed and why.

## Motivation
The problem this solves. Why now. What happens if nothing changes.

## Specification
The concrete proposal. Numbers, mechanisms, timelines.

## Rationale
Why this approach over alternatives.

## Backwards compatibility
What this changes for existing users, integrations, partners.

## Implementation
Who builds it. Dependencies. Phases.

## Funding (if applicable)
Amount. Source. Vesting or milestones.

## Open questions
Honest list of unresolved items for community input.

## Author(s) and acknowledgements
Named contributors. Co-authors. Reviewers consulted.
```

## Voice

Apply the style guide (`knowledge/reference/style-guide.md`):
- No em dashes
- Active voice
- British English
- Honest about open questions
- No marketing-speak

## Reference patterns from past GIPs

- **GIP-122** (Gnosis VPN funding): clean two-stage funding pattern (build, then legal wrapper via GIP-129). Good reference for any multi-phase ask.
- **GIP-149** (Revoke.cash): public-goods funding precedent. Good reference for security infra grants.
- **GIP-141** (Strategic Visual Adoption): heavy debate on metrics. Good reminder to include success metrics upfront.
- **Crisis Response Framework v2** (in flight): graduated intervention model. Good reference for any framework-style GIP.

## After drafting

1. Save to `outputs/drafts/gip-[topic]-[date].md`
2. Surface the sign-off chain to Ben before any posting
3. Do not post to the forum yourself; that always requires Ben's explicit go-ahead

## Templates

See `templates/gip.md` for the boilerplate file.
