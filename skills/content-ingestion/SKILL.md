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

## Model
- **Model:** claude-sonnet-4-6
- **Effort:** medium

## Governance
- **Owner:** unassigned
- **Evals:** none defined
- **Retirement trigger:** unused for 90 days, or superseded by a more capable skill
