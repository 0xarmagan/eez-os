# Sourcing Policy — Cache First, Live When Stale

The agent uses a hybrid sourcing model. Cached corpus is fast and consistent; live data is current. Both have failure modes.

## Decision tree

1. **Always check the cache first.** Look in `knowledge/` and `data/`.
2. **Live-fetch if any of these are true:**
   - The freshness TTL in `knowledge/_meta/freshness.yaml` is exceeded
   - The question is explicitly about current state ("what's the latest", "is it live", "current vote count", "now")
   - The user is asking about a forum post, GIP vote, or news item by date
3. **When live and cached disagree:** surface the conflict. Do not silently pick one. State both, note the timestamp gap, and ask which to use.
4. **When live fails:** fall back to cached and flag the staleness.

## What lives where

### Cache wins (default)

- EEZ thesis, positioning, vision (`knowledge/eez/00-overview.md`)
- Messaging pillars and voice (`knowledge/eez/01-comms-framework.md`, `knowledge/reference/style-guide.md`)
- Product fundamentals (`knowledge/gnosis/products/*.md`)
- Partner relationships and history (`data/partners.yaml`)
- Stakeholder map (`knowledge/reference/stakeholder-map.md`)
- Workflows and templates

### Live wins (always fetch)

- Snapshot proposal status, vote counts, quorum
- Forum thread status, post counts, latest replies
- GitHub PR/issue status
- Current GIP phase
- Live KPIs from Dune or Gnosis Data API
- Anything labelled "current", "today", "latest", "now"

### Hybrid (cache + targeted live check)

- GIP register (`data/gips.yaml`) — cache for context, live for status/votes
- Product KPIs (`data/kpis.yaml`) — cache for trend, live for current value
- Partner pipeline (`data/partners.yaml`) — cache for context, live for new ethCC-form-style submissions

## Live write rules

Reads are cheap. Writes are not. Any tool call that posts, sends, or modifies external state requires explicit Ben confirmation, regardless of persona.

This includes:
- Posting to the forum
- Tweeting from any account
- Sending email
- Updating Notion or Google Drive
- Calling MCP write tools

## Conflict-handling examples

> Cache: "GIP-149 is in phase-3, 8 posts."
> Live: "GIP-149 is now closed, 14 posts."
> Response: "GIP-149 has moved to closed since the cache was last refreshed (cache: phase-3, 8 posts; live: closed, 14 posts). Using live."

> Cache: "Linea is a founding partner."
> Live: (no contradiction)
> Response: proceed with cache, no flag needed.

> Cache: partner X marked "positive sentiment"
> Live: partner X has not posted in 6 weeks
> Response: "The cache says positive sentiment but partner X has been quiet for 6 weeks. Worth a re-engagement check."
