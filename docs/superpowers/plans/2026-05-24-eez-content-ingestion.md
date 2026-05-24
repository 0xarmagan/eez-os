# EEZ Content Ingestion Workflow — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a `/ingest` command and supporting skill that processes EEZ-related videos and blog posts into structured KB documents, with a draft-first review step before KB promotion.

**Architecture:** A skill file defines all processing logic (detection, fetch, generate, route). A slash command provides the entry point. A workflow doc serves as the human-readable SOP. Drafts land in `outputs/drafts/media-ingestion/` before promotion to `knowledge/eez/kb/`.

**Tech Stack:** Markdown skill files, Claude Code slash commands, `youtube_transcript_api` (existing), WebFetch, Read tool.

---

## File Map

| Action | Path | Responsibility |
|---|---|---|
| Create | `skills/content-ingestion/SKILL.md` | All processing logic: detection, fetch, generate, write, promote |
| Create | `.claude/commands/ingest.md` | Slash command entry point |
| Create | `workflows/content-ingestion.md` | Human-readable SOP and folder routing reference |
| Create | `outputs/drafts/media-ingestion/README.md` | Documents the staging folder purpose |
| Modify | `STRUCTURE.md` | Add `media-ingestion/` subfolder and `content-ingestion` workflow to navigation |

---

## Task 1: Create the staging folder

**Files:**
- Create: `outputs/drafts/media-ingestion/README.md`

- [ ] **Step 1: Create the README**

Write `outputs/drafts/media-ingestion/README.md` with this exact content:

```markdown
# Media Ingestion Staging

Holding area for content processed by the `/ingest` command, pending review and promotion to the knowledge base.

## Contents

Files here are drafts. Nothing in this folder has been reviewed or canonicalised.

## Promotion

After review, say "approved" and the agent will:
1. Move the files to the correct `knowledge/eez/kb/` subfolder
2. Update `knowledge/eez/kb/04-media-and-analysis/Media-Coverage.md`

## Routing reference

| Content type | KB destination |
|---|---|
| Video (community call, interview, talk) | `kb/04-media-and-analysis/` |
| Blog FAQ (interview / QA format) | `kb/06-reports/` |
| Blog FAQ (technical post, forum, research) | `kb/05-research-materials/` |
```

- [ ] **Step 2: Commit**

```bash
git add outputs/drafts/media-ingestion/README.md
git commit -m "feat: add media-ingestion staging folder"
```

---

## Task 2: Write the content-ingestion skill

**Files:**
- Create: `skills/content-ingestion/SKILL.md`

- [ ] **Step 1: Create the skill file**

Write `skills/content-ingestion/SKILL.md` with this exact content:

```markdown
# Content Ingestion Skill

**Trigger:** User shares a URL or file path to process for the EEZ knowledge base, or invokes `/ingest`.

---

## Step 1: Detect Content Type

Examine the input:
- `youtube.com` or `youtu.be` in URL → VIDEO
- File extension `.vtt` or `.srt` → VIDEO TRANSCRIPT (skip fetch, proceed to Step 4)
- Everything else → BLOG / ARTICLE

If the type is ambiguous (e.g. a blog structured as an interview transcript), ask once:
> "Should I treat this as a video transcript or an article?"

---

## Step 2: EEZ Relevance Check

Before fetching, confirm the content is EEZ-relevant. If the URL or filename does not clearly reference EEZ, Gnosis, L2 interop, or ZK proving, ask:
> "This content doesn't appear to be EEZ-related. Process it anyway?"

Wait for confirmation before continuing.

---

## Step 3: Fetch Content

**VIDEO URL — use youtube_transcript_api:**
```python
from youtube_transcript_api import YouTubeTranscriptApi
api = YouTubeTranscriptApi()
# Extract video_id from URL (the part after ?v= or the short youtu.be/[id])
transcript = api.fetch(video_id)
```
If the tool is unavailable, invoke the `claude-research:search-youtube` skill.

**BLOG / ARTICLE URL — use WebFetch:**
Retrieve the full page text.

**Local file (.vtt, .srt, .md, .txt, .pdf) — use Read tool.**

**Fallback (paywall, no captions, access error):**
State the specific error, then ask:
> "Please paste the text directly and I'll process it from there."

---

## Step 4: Process Content

### VIDEO — produce two files

**File 1: `[EventName]-TRANSCRIPT.md`**

Clean and format the raw transcript:
- Add speaker labels where identifiable (e.g. `**Jordi Baylina:** ...`)
- Add paragraph breaks at natural topic shifts
- Remove filler words (`[uh]`, `[um]`) and false starts
- Preserve all technical content verbatim

**File 2: `[EventName]-Summary.md`**

Use this exact structure:

```
# [Event Name] — Summary

## Overview
[2-3 sentences: what this is, who spoke, why it matters for EEZ]

## Key Topics
### [Topic 1]
[Paragraph or bullet points covering the main points]

### [Topic 2]
[...]

## Notable Quotes
> "[Quote]"
> — [Speaker], [context]

## Q&A
[Only include this section if the video has audience Q&A. Format each pair as:]
**Q:** [Question]
**A:** [Answer]

## Takeaways
- [3-5 bullet points summarising the most important points]
```

---

### BLOG / ARTICLE — produce one file

**File: `[SourceName]-[Topic]-FAQ.md`**

Use this exact structure:

```
# [Source Name]: [Topic] — FAQ

*Source: [URL or filename] | Processed: [YYYY-MM-DD]*

---

**Q: [Question]** `[extracted]`
**A:** [Answer as stated in the source]

**Q: [Question]** `[generated]`
**A:** [Answer derived from implicit content]
```

Rules:
- Label each entry `[extracted]` if the Q&A appears explicitly in the source, or `[generated]` if it fills a gap.
- Aim for 8-15 total entries.
- Prioritise technical concepts, use-case questions, and common misconceptions.
- Follow the format and depth of `knowledge/eez/kb/06-reports/FAQ.md`.

---

## Step 5: Name the Output Files

Use kebab-case, no spaces. Examples:
- `EEZ-DappCon-Berlin-Talk-TRANSCRIPT.md`
- `EEZ-DappCon-Berlin-Talk-Summary.md`
- `Bankless-EEZ-Episode-FAQ.md`

---

## Step 6: Check for Duplicates

Before writing, check whether a file with a similar name already exists in:
- `outputs/drafts/media-ingestion/`
- The target KB folder

If a potential duplicate exists, warn:
> "A file named `[name]` already exists at `[path]`. Overwrite, rename, or cancel?"

---

## Step 7: Write Drafts

Write all output files to `outputs/drafts/media-ingestion/`.

---

## Step 8: Report to User

State:
1. File names created
2. Location (`outputs/drafts/media-ingestion/`)
3. Target KB folder on approval
4. Routing rationale if non-obvious

Example:
> "Draft ready:
> - `outputs/drafts/media-ingestion/EEZ-DappCon-Talk-TRANSCRIPT.md`
> - `outputs/drafts/media-ingestion/EEZ-DappCon-Talk-Summary.md`
>
> On approval these move to `knowledge/eez/kb/04-media-and-analysis/`. Review and say 'approved' to promote."

---

## Step 9: Promote on Approval

When the user says "approved", "looks good", "promote it", or similar:

1. Move the draft files from `outputs/drafts/media-ingestion/` to the target KB folder.
2. Add an index entry to `knowledge/eez/kb/04-media-and-analysis/Media-Coverage.md`:
   - Video → under the relevant section (Community Calls, Fireside Chats & Technical Interviews, etc.)
   - Blog FAQ → under a "Blog Posts & Articles" section (create the section if it does not exist)
3. Confirm:
   > "Promoted to `[path]`. Media-Coverage.md updated."

---

## Folder Routing Reference

| Content type | KB destination |
|---|---|
| Video: community call, interview, fireside chat, talk | `kb/04-media-and-analysis/` |
| Blog FAQ (interview / QA format) | `kb/06-reports/` |
| Blog FAQ (technical post, forum discussion, research) | `kb/05-research-materials/` |

Default: video → `kb/04-media-and-analysis/`, blog → `kb/06-reports/`. State your routing choice before promoting and allow the user to override.
```

- [ ] **Step 2: Verify skill structure**

Check that the skill file covers all required steps by scanning for these section headers:
- `## Step 1: Detect Content Type`
- `## Step 2: EEZ Relevance Check`
- `## Step 3: Fetch Content`
- `## Step 4: Process Content`
- `## Step 5: Name the Output Files`
- `## Step 6: Check for Duplicates`
- `## Step 7: Write Drafts`
- `## Step 8: Report to User`
- `## Step 9: Promote on Approval`

All nine must be present.

- [ ] **Step 3: Commit**

```bash
git add skills/content-ingestion/SKILL.md
git commit -m "feat: add content-ingestion skill"
```

---

## Task 3: Write the slash command

**Files:**
- Create: `.claude/commands/ingest.md`

- [ ] **Step 1: Create the command file**

Write `.claude/commands/ingest.md` with this exact content:

```markdown
Process the provided URL or file for the EEZ knowledge base.

Invoke the `content-ingestion` skill from `skills/content-ingestion/SKILL.md` and follow it exactly, using $ARGUMENTS as the input URL or filepath.

If $ARGUMENTS is empty, ask: "What URL or file path would you like to ingest?"
```

- [ ] **Step 2: Verify**

Confirm the file exists at `.claude/commands/ingest.md` and that it references `$ARGUMENTS` (the Claude Code convention for passing slash command arguments).

- [ ] **Step 3: Commit**

```bash
git add .claude/commands/ingest.md
git commit -m "feat: add /ingest slash command"
```

---

## Task 4: Write the workflow SOP

**Files:**
- Create: `workflows/content-ingestion.md`

- [ ] **Step 1: Create the workflow file**

Write `workflows/content-ingestion.md` with this exact content:

```markdown
# Workflow: Content Ingestion

End-to-end process for adding EEZ-related media and articles to the knowledge base.

## Trigger

- You have a YouTube URL, blog post URL, or local file to add to the KB
- Invoke with: `/ingest [url or filepath]`

## Stages

### 1. Ingest (automated)

The `/ingest` command invokes `skills/content-ingestion/SKILL.md`, which:
- Detects content type (video or article)
- Fetches the content
- Generates the appropriate output (transcript + summary, or FAQ)
- Writes drafts to `outputs/drafts/media-ingestion/`

Duration: 1-3 minutes depending on content length.

### 2. Review

Read the draft files at `outputs/drafts/media-ingestion/`. Check for:
- Accuracy of technical claims
- Correct speaker attribution (video transcripts)
- FAQ entries that are misleading or out of scope
- Routing decision (correct KB folder?)

### 3. Approve and promote

Say "approved" (or "approved, but change X to Y" for inline corrections). The agent will:
1. Apply any corrections
2. Move files to the target KB folder
3. Add an index entry to `Media-Coverage.md`

## Folder Routing

| Content type | KB destination |
|---|---|
| Video (community call, interview, fireside chat, talk) | `knowledge/eez/kb/04-media-and-analysis/` |
| Blog FAQ (interview / QA format) | `knowledge/eez/kb/06-reports/` |
| Blog FAQ (technical post, forum, research) | `knowledge/eez/kb/05-research-materials/` |

Override the routing at approval time if needed: "approved, but route to `kb/05-research-materials/` instead."

## Outputs

- Draft files in `outputs/drafts/media-ingestion/`
- Promoted files in the target `knowledge/eez/kb/` subfolder
- Updated `knowledge/eez/kb/04-media-and-analysis/Media-Coverage.md`

## Related

- `skills/content-ingestion/SKILL.md` — processing logic
- `.claude/commands/ingest.md` — slash command definition
- `knowledge/eez/kb/04-media-and-analysis/Media-Coverage.md` — KB index
```

- [ ] **Step 2: Commit**

```bash
git add workflows/content-ingestion.md
git commit -m "feat: add content-ingestion workflow SOP"
```

---

## Task 5: Update STRUCTURE.md

**Files:**
- Modify: `STRUCTURE.md`

- [ ] **Step 1: Add `media-ingestion/` to the outputs section**

In `STRUCTURE.md`, find this block (around line 27):

```
├── outputs/                  ← Symlink to canonical project location
│   └── drafts/ ────→ ../projects/ethereum-economic-zone/outputs/drafts/
│       ├── INDEX.md
│       ├── community-events/     (call scripts, moderator notes, QA)
│       ├── technical-deep-dives/ (Zisk, proving, architecture)
│       └── planning-reference/   (governance, tokenomics, roadmaps)
```

Replace with:

```
├── outputs/                  ← Symlink to canonical project location
│   └── drafts/ ────→ ../projects/ethereum-economic-zone/outputs/drafts/
│       ├── INDEX.md
│       ├── community-events/     (call scripts, moderator notes, QA)
│       ├── technical-deep-dives/ (Zisk, proving, architecture)
│       ├── planning-reference/   (governance, tokenomics, roadmaps)
│       └── media-ingestion/      (ingest staging area — pending KB promotion)
```

- [ ] **Step 2: Add `content-ingestion` to the workflows section**

In `STRUCTURE.md`, find:

```
├── workflows/                ← Standard processes
│   └── new-partner-onboarding.md
```

Replace with:

```
├── workflows/                ← Standard processes
│   ├── new-partner-onboarding.md
│   └── content-ingestion.md
```

- [ ] **Step 3: Add the `/ingest` command to the Skills section**

In `STRUCTURE.md`, find:

```
└── .claude/                  ← Claude Code configuration
    ├── settings.json
    └── commands/
        └── eez-brief.md
```

Replace with:

```
└── .claude/                  ← Claude Code configuration
    ├── settings.json
    └── commands/
        ├── eez-brief.md
        └── ingest.md
```

- [ ] **Step 4: Update the Statistics table**

Find the Statistics table and update:

| Category | Files | Status |
|---|---|---|
| Workflows | 1 → **2** | 🚧 Minimal → ✅ Growing |
| Skills | 2 (GIP, partner-brief) → **6 (GIP, partner-brief, alliance-activation-matrix, co-marketing-brief, partner-sentiment-tracker, content-ingestion)** | ✅ Functional |

- [ ] **Step 5: Commit**

```bash
git add STRUCTURE.md
git commit -m "docs: update STRUCTURE.md for content-ingestion workflow"
```

---

## Task 6: Smoke test

No automated tests apply (these are instruction files, not code). Verify manually:

- [ ] **Step 1: Confirm all files exist**

```bash
ls eez-agent/skills/content-ingestion/SKILL.md
ls eez-agent/.claude/commands/ingest.md
ls eez-agent/workflows/content-ingestion.md
ls eez-agent/outputs/drafts/media-ingestion/README.md
```

Expected: all four paths resolve with no error.

- [ ] **Step 2: Invoke the skill with a known YouTube URL**

In a new session, run:
```
/ingest https://www.youtube.com/watch?v=<any EEZ community call URL>
```

Verify:
- Agent detects type as VIDEO
- Agent fetches transcript (or asks for fallback if captions unavailable)
- Agent writes two files to `outputs/drafts/media-ingestion/`
- Agent reports the draft path and target KB folder

- [ ] **Step 3: Invoke with a blog URL**

Run:
```
/ingest https://[any EEZ blog post URL]
```

Verify:
- Agent detects type as BLOG
- Agent produces a FAQ file at `outputs/drafts/media-ingestion/`
- FAQ entries are labelled `[extracted]` or `[generated]`

- [ ] **Step 4: Test approval flow**

After Step 2 or 3, say "approved." Verify:
- Files move from `outputs/drafts/media-ingestion/` to the correct `kb/` subfolder
- `Media-Coverage.md` gains a new index entry

- [ ] **Step 5: Final commit if any fixes were needed**

```bash
git add -A
git commit -m "fix: adjust content-ingestion skill based on smoke test"
```
