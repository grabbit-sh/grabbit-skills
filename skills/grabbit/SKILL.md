---
name: grabbit
description: >-
  Reddit marketing and lead-generation agent powered by the Grabbit MCP
  (grabbit.sh). Use this whenever the user wants to find high-intent leads,
  prospects, or buyers on Reddit; track brand or competitor mentions; monitor
  subreddits or keywords; triage a Grabbit inbox of classified posts/comments;
  or research and draft replies to Reddit threads. Triggers on "grabbit",
  "reddit leads", "reddit marketing", "find prospects on reddit", "brand
  mentions", "monitor subreddits", "who's talking about us on reddit",
  "competitor complaints on reddit", or any task that uses the
  mcp__grabbit__* tools. Covers researching subreddits/keywords, checking
  projects and feeds, building filtered views of classified entries, and
  engaging on Reddit without getting banned.
license: MIT
metadata:
  version: 0.1.0
---

# Grabbit — Reddit marketing agent

Grabbit (grabbit.sh) monitors Reddit for a brand: it watches subreddits, collects
posts and comments, and auto-classifies each one into 56 labels across 11
dimensions (relevancy, intent, sentiment, mentions, requests, competitor signals,
etc.). Your job with this skill is to drive that data — find leads and mentions,
build the right filtered views, and help the user engage — through the
`mcp__grabbit__*` tools.

## Mental model

- **Project** = one brand/product being monitored. It owns **feeds** (subreddits),
  **keywords** (filters), **tags** (labels), and **entries** (the collected +
  classified Reddit content). Almost every tool needs a `projectId`.
- **Entry** = one Reddit post or comment collected from a feed, with an attached
  classification. Filter entries by those classifications to build views.
- **Everything hangs off `list_projects`.** Always start there — it's the only
  source of `projectId`, and it returns the project's positioning
  (`description` / `problems` / `solutions` / `targetAudience`) plus its existing
  feeds, tags, and keywords with their ids. Read the positioning: it's the raw
  material for every reply you draft.

## Golden rules (the stuff that bites)

- **Check `permission` before mutating.** `list_projects` returns `permission`
  per project. It can be `read` — every `add_*`/`create_*`/`set_*`/`remove_*`
  tool needs `write`. Don't attempt writes on a read-only project; tell the user.
- **`add_project_feed` does NOT backfill.** It only collects posts published from
  the moment you attach it. For anything historical, use `search_reddit` (which
  reads Reddit's own index) — not a new feed.
- **List bodies are truncated to 300 chars.** `list_project_entries` gives you a
  preview. Before you judge relevance or draft a reply, pull the full body and
  comments with `get_project_entries`.
- **`set_entry_tags` REPLACES the whole tag set.** It's not additive. Read the
  entry's current tags first (from `list_project_entries`) and pass the full
  desired set, or you'll silently wipe existing tags. `[]` clears all.
- **Analysis filters: same category = OR, different categories = AND.**
  `["relevancy:high","relevancy:medium"]` = high OR medium.
  `["mention:project","sentiment:negative"]` = project mention AND negative.
  This is the entire mental model for building views.
- **`search_reddit` results are ephemeral.** They are NOT stored as project
  entries (cached ~1 day). Use them for ad-hoc/historical discovery; use feeds +
  entries for ongoing monitoring.
- **Mark entries `replied` after engaging.** `set_entry_status` keeps listings and
  the dashboard's analytics honest. Skipping this rots the inbox.

## Workflow

### 1. Orient
Call `list_projects`. Identify the target project's `id` and `permission`. Note
its existing `feeds`, `tags`, `keywords` (with ids — you'll reuse them) and read
its `problems`/`solutions`/`targetAudience`. This positioning tells you what a
"good lead" looks like and how to talk about the product later.

### 2. Research & expand coverage
Only when the user wants to widen what's monitored.
- `search_subreddits(query)` → find candidate communities (name, subscribers, NSFW).
- `list_project_feeds(projectId)` → inspect a feed in full: **posting rules**,
  subscriber count, sync status, current hot posts. Read the rules — most
  subreddits restrict self-promotion, and that governs how you can engage.
- `add_project_feed(projectId, subreddit)` → start monitoring (new posts only,
  counts against the plan's feed limit).
- `create_project_keyword(projectId, query, name)` → precise filtering. See the
  query DSL below; get it right or the filter is useless.
- `search_reddit(queries, range, sort)` → historical / one-off discovery that
  feeds do not backfill. Prefer short quoted phrases like `"brand monitoring"`.

**Keyword query DSL:** an array of groups combined with **AND** — every group must
pass. A group passes when **any** of its `terms` appears in the entry title or
body (**OR**); `negated: true` inverts it (passes only when none appear). Matching
is case-insensitive substring with **no stemming**, so use short stems —
`"track"` also catches `"tracking"`, `"tracked"`.

```
[{"terms":["reddit"],"negated":false},
 {"terms":["lead","mention","monitor"],"negated":false},
 {"terms":["hiring","job"],"negated":true}]
```
= `reddit` AND (`lead` OR `mention` OR `monitor`) AND NOT (`hiring` OR `job`).

### 3. Triage the inbox — the daily loop
This is the core of Reddit marketing with Grabbit.
- `list_project_entries(projectId, analysis, sortBy, range, ...)` → build a view
  using the recipes below. Default sort `relevancy` desc; use `postedAt` for a
  freshness sweep. `range` (`24h`, `7d`, …) scopes recency. `status` defaults to
  `new`+`replied`; pass it explicitly for `archived`/`deleted`.
- `get_project_entries(entryIds)` → full bodies + comments for the entries worth a
  closer look. Always do this before deciding to engage.
- Tag as you go: `create_project_tag` once for reusable labels (e.g. an
  `engage-queue`), then `set_entry_tags(entryId, tagIds)` — remember it replaces.
- `set_entry_status(entryIds, "archived")` to clear noise so the inbox stays real.

### 4. Engage — without getting banned
Reddit punishes spam with bans and brand damage, and Grabbit surfaces the rules
so you use them.
- **Read the subreddit's rules first** via `list_project_feeds` (or check the
  thread's sub). Many communities ban self-promotion outright.
- `get_reddit_thread(url)` → read the *live* conversation before drafting; the
  stored entry can be stale and misses recent replies.
- **Draft to help first.** Lead with a genuinely useful answer to the actual
  question. Disclose affiliation. Mention the product only where it truly fits the
  problem — use the project's `problems`/`solutions` as your positioning, not a
  pitch. If the sub bans promo, help without linking. Grabbit does not post for
  you: hand the user the drafted reply (and thread url) to post themselves.
- After the user posts: `set_entry_status(entryIds, "replied")`.

### 5. Report
Summarize what you found and did: counts of leads / mentions by sentiment, the
top threads worth engaging (with urls), what was tagged/archived, and any drafted
replies pending the user's post.

## Filter recipes

Common `analysis` filter sets for `list_project_entries` (same category OR,
different categories AND):

| View | `analysis` |
|---|---|
| Leads | `["relevancy:medium","relevancy:high"]` |
| Buying leads | `["relevancy:medium","relevancy:high","intent:buying"]` |
| Brand mentions | `["mention:project"]` |
| Negative brand mentions | `["mention:project","sentiment:negative"]` |
| Competitor complaints | `["mention:competitor","competitor:complaint"]` |
| Testimonials | `["experience:customer_testimonial"]` |
| Pain points to solve | `["experience:pain_point","relevancy:high"]` |
| Recommendation requests | `["request:recommendation","request:alternative"]` |

Combine with `keywordIds` (from `list_projects`), `feeds`, `range`, `search`, and
`kinds` (`post`/`comment`) to narrow further.

## Full classification taxonomy

The complete list of 56 labels across all 11 dimensions — read when you need a
label that isn't in the recipes above, or to explain what a classification means.

→ `references/analysis-labels.md`
