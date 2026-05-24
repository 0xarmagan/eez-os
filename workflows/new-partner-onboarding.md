# Workflow: New Partner Onboarding

The end-to-end playbook for moving a new contact from interest to active alliance member.

## Trigger

- Tally form submission (via https://tally.so/r/NpLo4l)
- Direct outreach from a known builder
- Introduction from an existing alliance member
- Cold contact from an event (EthCC, DappCon, etc.)

## Stages

### 1. Triage (within 48 hours)

- Add the contact to `data/partners.yaml` with `status: prospective`
- Capture: name, org, role, handle, source, why they want in, expertise, date
- Decide: is this a builder operating somewhere in the EVM landscape? If yes, default to welcoming. If unclear, flag to Armagan.

### 2. Alliance overview (within 1 week)

- Send the alliance overview message (private to builders; four areas of contribution; how to join)
- Add to the public Telegram group "EEZ Alliance"
- Confirm: which of the four contribution areas they're most interested in (specification feedback, technical contributions, user/protocol input, early onboarding)

### 3. First touch (within 2 weeks)

- Schedule a short intro call (or async if they prefer)
- Walk through the EEZ thesis (`knowledge/eez/00-overview.md`)
- Discuss their integration interest, if any
- Capture commitments and asks

### 4. Activation (within 4 weeks)

- Move `status: prospective` → `status: active`
- Issue any co-marketing asks if a launch milestone is upcoming
- Brief the Ecosystem Relations Lead (when role is hired) on the new partner
- Add to the Alliance Activation Matrix (in development)

### 5. Maintain

- Monthly check-in cadence for active partners
- Track sentiment in `data/partners.yaml`
- Flag drift (no contact 6+ weeks) for re-engagement

## Sign-offs

- **Welcoming a new partner to the Telegram group** — Armagan (no further sign-off needed for vetted EVM operators)
- **Co-marketing asks** — Armagan + Adz
- **Public amplification of a partner's work** — Armagan + ROM

## Outputs

- Updated `data/partners.yaml`
- Outreach email or Telegram message in `outputs/drafts/`
- Calendar invite for intro call
- Brief in `outputs/drafts/partner-[name]-brief-[date].md`

## Related

- `templates/partner-outreach-email.md`
- `skills/partner-brief/SKILL.md`
- `knowledge/eez/02-alliance.md`
