---
name: content-learning
description: Periodic skill for surfacing what the agent consistently gets wrong in drafts vs approved output. Run monthly or after 5+ pieces are approved. Produces concrete improvement suggestions for alliance-outreach and other skills.
---

# Content Learning Skill

Analyses the diff between `outputs/drafts/` and `outputs/approved/` to find recurring correction patterns. Surfaces concrete, actionable rule updates for skill files — never applies them automatically.

## Trigger

- "reflect on recent outputs", "what am I getting wrong"
- "content learning review", "monthly review"
- `/content-learning`
- Run after 5+ pieces are approved, or monthly, whichever comes first

---

## Pre-Check (run first)

Before doing any diff work:

1. List files in `outputs/approved/` (or the symlinked path under `../projects/ethereum-economic-zone/outputs/approved/`).
2. If fewer than 5 approved files exist, output the early-exit message below and stop.
3. If 5+ approved files exist, proceed to the analysis process.

**Early-exit message:**
```
No approved output to analyse yet — run after first 5 approved pieces.
```

---

## Process

### Step 1 — Inventory

List all files in both directories:
- `outputs/drafts/` — all `.md` files, sorted by date
- `outputs/approved/` — all `.md` files, sorted by date

Match each approved file to its corresponding draft by name pattern. The draft naming convention is `[skill]-[partner]-[mode]-[date].md`; approved files will share the same stem or a close variant. Flag any approved file with no matching draft (it may have been written directly — note it, skip it for pattern analysis).

### Step 2 — Semantic diff per file pair

For each matched pair, identify what changed substantively between draft and approved version. Ignore:
- Whitespace and formatting
- Punctuation that doesn't change meaning
- Date or placeholder fills that Armagan added

Flag:
- **Voice/register corrections** — a phrase rewritten to sound less corporate, more direct, or more Armagan-specific
- **Technical accuracy fixes** — a claim corrected, a number changed, a product name corrected
- **Structural changes** — sections reordered, an intro cut, a CTA moved
- **Vocabulary substitutions** — a banned phrase replaced, a canonical term swapped in
- **Cuts** — whole sentences or paragraphs removed entirely (note what type of content was cut)

Write one line per change noting: category, the original phrase, the corrected phrase, and which file pair it came from.

### Step 3 — Pattern identification

Group the changes from Step 2. A pattern is the same category of edit appearing in 3 or more separate file pairs. Single-file corrections are one-off judgment calls and are not reported as patterns.

For each pattern:
- Name it concisely (e.g. "Em dash slippage", "Partner-language borrowing", "Excited-to opener")
- Show the best representative example (draft → approved)
- Count how many times it appeared
- Map it to a specific rule or voice guideline that, if tightened, would prevent it

### Step 4 — Skill mapping

For each pattern, identify which skill file contains the rule that should be updated (or where a new rule should be added). Most patterns will map to `alliance-outreach/SKILL.md` (voice, vocabulary, structure) or the style guide fallback block.

---

## Output Format

Save to `outputs/drafts/content-learning-[YYYY-MM-DD].md` and print to screen.

```markdown
## Content Learning Report — [YYYY-MM-DD]

### Patterns Observed ([N] drafts reviewed)

**Recurring corrections:**
- [Pattern name]: "[draft phrase]" → "[approved phrase]" (seen [N] times)

**Systematic gaps:**
- [What the agent consistently misses or over-includes — e.g. "Leads with endorsement instead of position on X threads"]

### Suggested Skill Updates

**alliance-outreach/SKILL.md:**
- Add to rules: [specific rule derived from pattern — quote the exact wording to add]
- Change voice guidance: [specific wording change, with before/after]

**[other skill if applicable]:**
- [specific suggestion]

### Next Steps
- [ ] Review suggestions above and apply to skill files manually
- [ ] Delete draft files for approved content (optional cleanup)
```

---

## Rules

- Never auto-apply suggestions to skill files. Surface for Armagan to review and approve before any skill is edited.
- Only report patterns seen 3+ times. Single corrections may be one-off judgment calls — do not over-generalise.
- If the approved version is missing a matching draft, note it in the report but do not include it in pattern counts.
- If the diff is ambiguous (unclear whether a change is voice or accuracy), err on the side of voice — those are more actionable as rules.
- If no approved output exists yet, output: "No approved output to analyse yet — run after first 5 approved pieces."
- Save the report to `outputs/drafts/content-learning-[YYYY-MM-DD].md`. Never save to `outputs/approved/` — the report itself is a draft.
- When suggesting rule additions, write the exact text that would go into the skill file, not a vague description. Make the suggestion copy-pasteable.
