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
  version: 0.4.0
---

# Grabbit — Reddit marketing agent

Grabbit (grabbit.sh) monitors Reddit for a brand: it watches subreddits, collects
posts and comments, and auto-classifies each one into 56 labels across 11
dimensions (relevancy, intent, sentiment, mentions, requests, competitor signals,
etc.). Your job with this skill is to drive that data — find leads and mentions,
build the right filtered views, and help the user engage — through the
`mcp__grabbit__*` tools.

Run it as a **self-driving loop**: pull the highest-intent leads, and for each one
produce a ready-to-post reply draft — right voice, right angle, rule-safe — that
the user only has to paste. The reply craft in step 4 is where this skill earns
its keep; don't shortcut it.

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

### 4. Engage — draft replies that convert without getting banned
This is where the agent earns its keep. A reply that reads like an ad gets
downvoted, removed, and can get the account banned; a reply that reads like a
helpful regular wins the lead. For each thread worth engaging, run this sequence —
never skip straight to drafting.

1. **Read the live thread *and its comments*.** `get_reddit_thread(url)` — not just
   the post. The comments are the single most useful input you have: they set the
   community's register, reveal what's already been said (never repeat it), and
   show which answers the crowd actually rewards (score).

2. **Check the rules.** `list_project_feeds(projectId)` exposes each feed's posting
   rules; if the thread's sub isn't a feed, judge from its norms. Many subs ban
   links or self-promo outright. The rules decide whether you may name the product,
   link it, or only help. When in doubt, help without linking.

3. **Copy the voice of a good comment.** Pick the top-upvoted, natural-sounding
   comment in the thread (skip mods and other ads) and mirror it: length, tone,
   formality, formatting (paragraph vs. bullets vs. one-liner), jargon vs. plain
   talk. Your reply should be stylistically indistinguishable from a regular
   contributor — that's the entire game. A three-paragraph pitch under a thread of
   terse one-liners screams "marketer" and dies.

4. **Find the angle from the classification.** The entry's labels hand you the
   opening — match the reply to what they're actually doing:
   - `request:recommendation` / `intent:comparing` → give an honest shortlist, put
     the product in it as one you tried, don't trash competitors.
   - `competitor:complaint` / `competitor:feature_gap` → empathize with the pain,
     then show how you'd solve *that specific* problem.
   - `experience:pain_point` / `request:advice` → answer the real question first;
     the product is at most the last sentence.
   - `request:pricing` / `intent:buying` → be concrete about cost and fit; these
     are ready to convert, don't be coy.

5. **Draft help-first, speak as a user, earn the mention.** Lead with a genuinely
   useful answer built from the thread. Frame the product as something you tried
   and it worked ("i switched to X", "X fixed this for me"), not something you
   sell. First person as a happy user reads as a recommendation; "we built X" reads
   as an ad. Mention it only where it's the honest answer to what they asked, one
   line, no link if the sub forbids it. Pull substance from the project's
   `problems`/`solutions` (via `list_projects`), never a slogan.

6. **Hand off and record.** Grabbit does not post for the user. Output the drafted
   reply plus the thread url so they can post in their own account, then
   `set_entry_status(entryIds, "replied")` once posted.

**Match the room — example** (use placeholders; fill `[your product]` etc. from the
project's positioning). Thread in a SaaS sub, comments are short and casual:
*"what are you all using for [category]? doing it manually is killing me."*

❌ Screams marketer (wrong register, slogan, feature-dump):
> Great question! Doing [category] manually is a significant challenge many teams
> face. Fortunately, [your product] leverages a robust, seamless approach to
> streamline this and empower you to [outcome] automatically in real time...

✅ Matches the room (mirrors the casual one-liner voice, help-first, product as a
user's own experience):
> same, doing it by hand was eating my mornings. what worked for me was [the free
> approach] so only the stuff worth acting on reached me instead of scrolling. i use
> [your product] for it now, but even [the free approach] beats doing it manually.
> can share how i set it up if useful.

**Reply checklist before you hand it over:** register matches a real comment in the
thread · opens with actual help · product framed as your own experience, not a
pitch, and ≤1 line · no link if rules forbid · adds something not already said.

**The full comment craft lives in `references/writing-comments.md` — read it before
drafting.** It covers reading the room (tone/format/rules), voice-matching a real
commenter, real human comment samples to study, the AI-tell filter (banned
words/phrases/punctuation that out an LLM), mechanical humanization, per-lead-type
examples with placeholders, and account strategy (the ~3:1 help-to-mention ratio)
so the account doesn't get pattern-matched as a shill and shadowbanned.

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
