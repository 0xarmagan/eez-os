# /eez-brief

Produce a 1-page brief on a specific partner.

## Usage

```
/eez-brief [partner-name]
```

Example: `/eez-brief Linea`

## What this command does

1. Looks up the partner in `data/partners.yaml` (internal scope required)
2. Pulls related context from `knowledge/eez/02-alliance.md`
3. Loads the partner-brief skill from `skills/partner-brief/SKILL.md`
4. Produces a 1-page brief covering status, commitments, asks, and next action
5. Saves to `outputs/drafts/partner-[name]-brief-[date].md`

## Persona requirement

Internal only. If invoked by a partner or builder persona, decline with a short explanation.

## Output template

Use the brief template defined in `skills/partner-brief/SKILL.md`.

## Defaults

- If the partner is not in `data/partners.yaml`, flag as new and produce a research-stub brief instead, noting what is missing.
- If sentiment is "dormant" or "drift", lead with a re-engagement angle.
- If multiple partners share the name (rare), ask Ben to disambiguate.
