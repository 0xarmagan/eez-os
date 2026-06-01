# Brand Guardrails

## Brand Center

**A playful subversive on a mission to protect human agency.**

This is the single orienting idea. Every piece of content, every design choice, every product decision should pass the test: *Would a playful subversive protecting human agency do it this way?*

### Personality

Gnosis is a playful subversive on a mission to protect human agency. Technically sharp, deliberately imperfect, joyfully irreverent. Having more fun than the stakes should allow. Quick to laugh at absurdity, at the industry, at every sacred cow — but never at the mission or the people it serves.

Understate the big stuff and celebrate the small stuff. Only ever punch up. And ideology is the punchline, not the premise.

**Think Deadpool, not Batman.** We genuinely care about people and will absolutely fight for them, but we don't brood about it. We're bold, visible, self-aware, and having fun. We know the tropes of our industry — the self-serious manifestos about sovereignty and decentralization — and we'd rather show you what ownership looks like in practice than lecture you about why it matters.

The owl isn't hiding in the shadows — right there, sometimes in the center, sometimes in unexpected places, always present. A playful guardian who sees what others don't and hands power back to the people.

### Emotional Trigger: Irreverent Joy

The feeling that someone is having more fun than they should be, given the circumstances. The stakes are real — financial systems, human agency, the future of money — but the energy is alive, not heavy. It makes you want to be part of it before you fully understand why.

Irreverent joy is the delivery mechanism for the serious mission. It grants permission to care without performing and lets ideology land without preaching.

---

## Brand Tramlines

The center tells you what to do. The tramlines tell you where the lines are — four guardrails that keep the playful subversive on the road. Run any draft, design, or campaign through these in sequence.

**Start with them** — Every piece of content answers "so what?" from their side, not ours. If you can't say why they'll care in one sentence, bin it.

**Show, don't say** — Actions over announcements. Don't explain why you burned the tokens — just burn them and move on. The work speaks.

**Fewer words, more intent** — Grade 6 reading level. No jargon. Bold presence. Every element earns its place. We don't hide and we don't shout.

**Be technically precise** — Every claim must be accurate and verifiable. Gnosis was built by engineers who ship real infrastructure — the brand must reflect that rigour. Wrong numbers, overstated capabilities, or hand-wavy technical claims erode trust faster than silence. If you can't verify it, don't publish it. If the number has changed, update it. Precision is not the enemy of personality — it's what earns the right to be playful.

---

## How these guardrails get applied

Every piece of external-facing copy is routed through the `voice-dna` skill (`.claude/skills/voice-dna/SKILL.md`). The skill auto-triggers on any writing task and stacks four layers of voice, in order:

1. These guardrails plus the base writing rules and banned phrases (universal, always applied).
2. The brand-family voice: `_brand/tone-of-voice/gnosis/gnosis.md` (for Gnosis and its sub-brands), `_brand/tone-of-voice/eez/eez.md`, or `_brand/tone-of-voice/circles/circles.md`.
3. A Gnosis sub-brand delta (when relevant): `_brand/tone-of-voice/gnosis/sub-brands/<sub>/<sub>.md`.
4. The channel overlay: `_brand/tone-of-voice/channels/<channel>.md`.

If a brand, sub-brand, or channel cannot be inferred from the task, the skill asks before drafting. It does not default silently.

Team updates to voice live under `_brand/tone-of-voice/`. They ripple into every future piece of copy on the next run.
