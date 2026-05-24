# Tone of Voice

Canonical tone files for every brand in the Gnosis Universe. Voice-dna (`.claude/skills/voice-dna/SKILL.md`) auto-loads from here whenever a human-facing piece of copy gets drafted.

## Layout

```
_brand/tone-of-voice/
├── CLAUDE.md                             (this file)
├── channels/                             universal channel overlays
│   ├── blog.md  x.md  linkedin.md
│   ├── email.md  website.md  press-release.md
│   └── video.md
├── gnosis/
│   ├── gnosis.md                         BASE voice for the Gnosis branded house
│   ├── examples/<channel>/*.md           optional umbrella Gnosis examples
│   └── sub-brands/
│       ├── gnosis-pay/gnosis-pay.md + examples/
│       ├── gnosis-chain/ ...
│       ├── gnosis-app/ ...
│       ├── gnosis-business/ ...
│       └── gnosis-dao/ ...
├── eez/
│   ├── eez.md                            independent brand voice (no inheritance)
│   └── examples/<channel>/*.md           optional
└── circles/
    ├── circles.md                        independent brand voice (no inheritance)
    └── examples/<channel>/*.md           optional
```

## The stacked matrix

Any piece of copy gets a voice assembled from four additive layers. The full resolution lives in `.claude/skills/voice-dna/SKILL.md`; the short version:

1. Always: `_brand/brand-guardrails.md` plus voice-dna base rules and banned phrases.
2. Brand family (one of three): `gnosis/gnosis.md`, `eez/eez.md`, or `circles/circles.md`.
3. Sub-brand (Gnosis only, optional): `gnosis/sub-brands/<sub>/<sub>.md`.
4. Channel (always one): `channels/<channel>.md`.
5. Examples (optional, if the folder exists): `<brand-path>/examples/<channel>/*.md`.

Branded house versus independent brand:

- Gnosis is a **branded house**. Sub-brands inherit the base Gnosis voice and only define deltas in their own files.
- EEZ and Circles are **independent brands**. They define their full voice from scratch. No inheritance from Gnosis.

## Examples folders

Examples are optional. Drop on-voice samples into `<brand-path>/examples/<channel>/` as individual `.md` files. Voice-dna pulls them when it resolves a piece for that brand and channel. No examples means no examples; the skill does not fail.

Keep each example small and self-contained. A single tight piece of real, on-voice copy beats a three-paragraph explanation of what "on-voice" means.

## Who owns this

The brand team owns every file in this folder. Propose changes via PR. Voice-dna reads these files as truth; it does not edit them.
