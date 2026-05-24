# Design: EEZ Content Ingestion Workflow

**Date:** 2026-05-24  
**Status:** Approved  
**Author:** Armagan (via brainstorming session)

---

## Overview

A workflow that processes any EEZ-related content shared by the user — generating a transcript and structured summary for video, or extracting + generating a FAQ for blog posts — and writes drafts ready for promotion into the knowledge base.

---

## Components

Three new files added to `eez-agent/`:

| File | Purpose |
|---|---|
| `skills/content-ingestion/SKILL.md` | Core processing logic: detection, fetch, generate, write draft |
| `.claude/commands/ingest.md` | Slash command entry point: `/ingest [url or filepath]` |
| `workflows/content-ingestion.md` | Human-readable SOP with folder routing rules |

---

## Data Flow

```
/ingest [url or filepath]
  │
  ├─ Detect type
  │   ├─ youtube.com / youtu.be / .vtt / .srt  → VIDEO
  │   └─ everything else                        → BLOG / ARTICLE
  │
  ├─ Fetch content
  │   ├─ VIDEO URL   → youtube_transcript_api (api.fetch())
  │   ├─ BLOG URL    → WebFetch
  │   └─ Local file  → Read tool
  │
  ├─ Process
  │   ├─ VIDEO  → raw transcript (.md) + structured summary
  │   │            sections: Overview, Key Topics, Notable Quotes, Q&A, Takeaways
  │   └─ BLOG   → extracted explicit Q&As + generated gap-filling entries
  │                formatted as FAQ document
  │
  ├─ Write to outputs/drafts/media-ingestion/
  │   ├─ VIDEO:  [EventName]-TRANSCRIPT.md + [EventName]-Summary.md
  │   └─ BLOG:   [SourceName]-[Topic]-FAQ.md
  │
  ├─ Report to user: draft path + one-line description
  │
  └─ [After user approval]
      ├─ VIDEO  → knowledge/eez/kb/04-media-and-analysis/
      ├─ BLOG FAQ → knowledge/eez/kb/06-reports/ (or kb/05-research-materials/ if research-oriented)
      └─ Update knowledge/eez/kb/04-media-and-analysis/Media-Coverage.md index
```

---

## Output Formats

### Video

Two files per video:

**`[EventName]-TRANSCRIPT.md`** — cleaned full transcript, speaker-labelled where possible.

**`[EventName]-Summary.md`** — structured summary with:
- Overview (2-3 sentences on context and significance)
- Key Topics (bulleted sections per major theme)
- Notable Quotes (verbatim, attributed)
- Q&A (if the video contains audience questions)
- Takeaways (3-5 bullet points)

### Blog / Article

**`[SourceName]-[Topic]-FAQ.md`** — FAQ document with:
- Extracted Q&As: questions and answers that appear explicitly in the source
- Generated Q&As: gap-filling entries derived from implicit claims and concepts in the content
- Each entry labelled `[extracted]` or `[generated]` for traceability
- Format follows the existing `knowledge/eez/kb/06-reports/FAQ.md` pattern

---

## Folder Routing

| Content type | Draft location | KB destination on approval |
|---|---|---|
| Video (community call, interview, talk) | `outputs/drafts/media-ingestion/` | `kb/04-media-and-analysis/` |
| Blog → FAQ (interview/QA format) | `outputs/drafts/media-ingestion/` | `kb/06-reports/` |
| Blog → FAQ (technical/research post) | `outputs/drafts/media-ingestion/` | `kb/05-research-materials/` |

The skill states its routing choice. User can override before promotion.

After promotion, `Media-Coverage.md` is updated with a one-line index entry linking to the new file.

---

## Edge Cases

| Situation | Behaviour |
|---|---|
| Non-EEZ content | Flags before processing, asks to confirm |
| Paywalled / inaccessible URL | Surfaces error, asks user to paste text directly |
| YouTube with no captions | Falls back, asks for manual transcript file |
| Ambiguous type (blog that reads like a transcript) | Asks once: "treat as video transcript or article?" |
| Blog routing ambiguous (05 vs 06) | Skill declares its choice and allows override |
| Duplicate filename in target folder | Warns before writing |

---

## Constraints

- Drafts go to `outputs/drafts/media-ingestion/` first. Nothing is written to `knowledge/` without user approval.
- Promotion (draft → KB + index update) happens in the same session after user says "approved."
- Consistent with CLAUDE.md §8: live writes (any file write to KB) require explicit confirmation.
- Content type detection is best-effort; ambiguous cases always ask rather than guess.
