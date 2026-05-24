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
