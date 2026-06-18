---
name: youtube-trends
description: Calls Traikker Tools MCP/HTTP endpoints and saves HTML research reports for YouTube trends and tool-backed data. Use when user asks for Traikker tool results, trends, YouTube research, MCP calls, saved reports, channel whitelists, or channel blacklists.
---

# Youtube Trends

## Quick start

Need tool JWT with scope, e.g. `youtube.trends`.

```bash
TOOLS_URL="${TOOLS_URL:-https://tools.traikker.workers.dev}"
TOKEN="<tool-jwt>"
OUT="$HOME/trends-$(date +%Y%m%d-%H%M%S).html"
```

Call MCP `youtube.trends`, parse `result.structuredContent`, generate HTML report, save to `$OUT`.

## Ask user

Before running:
- Main query?
- Additional queries?
- Time window? default: last 3 weeks
- `minViews`? default: 100
- Channel whitelist? optional
- Channel blacklist? optional
- Output path? default: `~/trends-<datetime>.html`

## YouTube trends defaults

```json
{
  "categoryId": "28",
  "region": "US",
  "language": "en",
  "sort": "date",
  "maxResults": 50,
  "minViews": 100,
  "rankingLimit": 10,
  "topicLimit": 10
}
```

Categories:
- `28` Science & Technology
- `27` Education fallback

## Multiple queries

If user adds queries:
1. Run `youtube.trends` once per query.
2. Merge `currentSnapshots`.
3. De-dupe by `videoId`.
4. Apply channel filters.
5. Apply date/minViews filters.
6. Sort by chosen metric:
   - default: `viewCount desc`
   - recent: `publishedAt desc`
   - topic report: tool topic score

## Recent-only filtering

Tool may not enforce `publishedAfter`.

For “last N days/weeks”:
- Fetch `sort: "date"`
- Post-filter `publishedAt >= cutoff`
- Mention report uses post-filtered current snapshots if `historyStatus: insufficient`

## Channel whitelist

Whitelist = actively seek channels.

If user provides handles/names:
1. Resolve to channel IDs if possible.
2. Run query normally.
3. Also run targeted queries with `channelIds`.
4. Merge results.
5. Mark whitelisted rows with badge: `WHITELIST`.

Example:

```json
{
  "channelWhitelist": ["@t3dotgg", "@NateBJones"]
}
```

## Channel blacklist

Blacklist = exclude unwanted channels after fetch.

Match against:
- `channelId`
- `channelTitle`
- handle if known

Example:

```json
{
  "channelBlacklist": ["TED", "Marques Brownlee"]
}
```

Never show blacklisted videos unless user asks for audit list.

## HTML output

Save to:

```bash
~/trends-$(date +%Y%m%d-%H%M%S).html
```

HTML sections:
- Summary
- Query parameters
- Applied filters
- Top videos table
- Top topics table, if useful
- Recommended Titles
- Notes/limitations

Video table columns:
- Rank
- Title
- Channel
- Views
- Published date
- Query source
- Badges
- Link

Recommended titles:
Provide a table of 3 recommended titles and descriptions for the most trending video based on the research
- Title
- Description
- Score

Use full links:

```text
https://www.youtube.com/watch?v=<videoId>
```

## HTML style

Generate standalone HTML:
- `<meta charset="utf-8">`
- readable table CSS
- clickable links
- numeric views with commas
- escape HTML

## Limitation note

If `historyStatus: "insufficient"` or `observation.status: "disabled"` include:

> Trend history unavailable; report uses current YouTube search snapshots, post-filtered by date/channel/views, not growth-over-time ranking.
