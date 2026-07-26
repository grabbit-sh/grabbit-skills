# Writing Reddit replies that convert (and don't read like ads)

The reply is the whole product. Grabbit finds the lead; a reply that reads like
marketing wastes it — downvoted, removed, sometimes banned. A reply that reads
like a helpful regular wins it. This doc is the craft behind step 4 of `SKILL.md`.

## Contents
1. The one method: ghostwrite a real commenter
2. Why AI replies get spotted
3. The AI-tell filter (run before you hand off)
4. Match the mechanical texture
5. Angle by lead type — worked examples
6. Tone by situation
7. Rhythm, length, and mentioning the product

---

## 1. The one method: ghostwrite a real commenter

Before drafting, pull the thread with `get_reddit_thread(url)` and read the
comments. Pick the **top-upvoted, natural-sounding comment** (skip mods and other
ads) and write *as that person* — not "inspired by," actually ghostwrite them.
Copy six things:

- **Sentence length and rhythm** — short jabs vs. long run-ons
- **Punctuation habits** — do they skip commas? use periods as beats?
- **Paragraph breaks** — one fat block, or lots of little ones
- **Vocabulary and register** — jargon-heavy vs. plain talk
- **Messiness** — fragments, lowercase, missing apostrophes
- **Emotional directness** — blunt, warm, dry, ranty

The sample gives you *how* to write. The lead's classification (section 5) gives
you *what* to say. If there's no good comment yet, match the **post's** voice
instead.

## 2. Why AI replies get spotted

Reddit has a finely tuned nose for AI. Four tells, with the fix:

**AI writes symmetrically. Humans don't.**
> AI: three paragraphs, two sentences each, all the same size.
> Human: one fat block of six sentences, then a one-liner. Wildly uneven.

**AI covers every angle. Humans fixate.**
> AI: "On one hand X, on the other hand Y — both have merits."
> Human: "honestly the migration isnt worth it, we tried and rolled back in a week"

**AI over-punctuates. Humans run thoughts together.**
> AI: "It's fast. I've benchmarked it. However, the cost is steep."
> Human: "its fast ive benchmarked it but the cost is rough at scale"

**AI announces its points. Humans just say them.**
> AI: "Here's the thing: most people don't actually need a full crawler."
> Human: "most people dont need a full crawler they need like 3 pages scraped"

Your length comes from *your* take, never from matching OP's paragraph count.

## 3. The AI-tell filter (run before you hand off)

Mechanical check on every draft. If it trips, rewrite — these are the words that
out an LLM instantly:

- **Banned words**: crucial, notable, significant, essential, fundamental,
  straightforward, delve, landscape, realm, navigate (metaphorical), foster,
  facilitate, pivotal, nuanced, robust, comprehensive, multifaceted, intriguing,
  compelling, leverage, synergy, seamless, game-changer, unlock, empower
- **Banned phrases**: "it's worth noting", "the reality is", "what's interesting
  is", "in essence", "at the end of the day", "at its core", "great question"
- **Banned transitions**: However, Furthermore, Additionally, Moreover, That said
  → use *but, and, also, tho*
- **Banned punctuation**: em dashes (—), semicolons, colon-before-an-explanation
- **No hedging openers**: don't start with "I think", "in my opinion", "to be fair"
- **No sucking up**: never open with "Great question!", "Thanks for sharing!" —
  respond to the content, not the person
- **No formatting in casual subs**: no bullets/bold/numbered lists unless the sub's
  own top comments use them

## 4. Match the mechanical texture

Real Redditors don't write like press releases. To blend, borrow the texture of
the sample you picked — not as a trick, but because clean corporate prose is the
tell:

- Contractions often lose the apostrophe: *dont, youre, isnt, thats, wont*
- Sentences start lowercase sometimes
- Paragraphs are uneven; fragments are fine
- Numbers and specifics beat adjectives ("cut my scraping time from ~2h to 20min"
  lands; "dramatically faster" doesn't)

Don't overdo it into performative sloppiness — match *that thread's* level.

## 5. Angle by lead type — worked examples

Each lead's classification labels hand you the opening. Full before/after pairs:

### `request:recommendation` / `intent:comparing`
Thread: *"Whats everyone using to scrape sites for LLM context? Firecrawl keeps
timing out on me."*

❌ Ad:
> Great question! For reliable web scraping at scale, ScrapeGraphAI leverages
> natural language prompts to extract structured data seamlessly without proxies
> or CSS selectors. It's a robust, comprehensive solution...

✅ Blends in, honest shortlist, user voice:
> firecrawl timeouts are usually the big js-heavy pages. depends what you need, if
> you just want clean markdown the reader-style APIs are fine. i needed structured
> json out of messy pages so i switched to scrapegraphai, you give it a prompt
> instead of selectors so it doesnt break when the layout changes. not magic but
> the no-selectors part saved me a ton of maintenance.

### `competitor:complaint` / `competitor:feature_gap`
Thread: *"Ugh, [competitor] just raised prices again and still no async API. Looking
for alternatives."*

❌ Ad:
> Sorry to hear that! ScrapeGraphAI offers a compelling alternative with a robust
> async API and transparent pricing that empowers developers to...

✅ Empathize, then solve the *specific* gap:
> the async thing is what got me too, ended up queueing everything myself which was
> dumb. i moved to scrapegraphai mainly because the async endpoint was there out of
> the box. pricing is per-request not seat based so it scales with usage instead of
> punishing you. worth a trial before you commit tho.

### `experience:pain_point` / `request:advice`
Thread: *"Spent all weekend writing selectors and the site redesigned Monday. I want
to quit."*

❌ Ad that ignores the human:
> This is a common pain point! ScrapeGraphAI solves selector maintenance by...

✅ Answer the real pain first, product is the last line and optional:
> yeah selectors are a losing game on sites that redesign a lot. two things that
> helped me, scrape the rendered dom not the raw html, and target stable stuff like
> aria labels or the json in script tags instead of css classes. if you want to
> skip selectors entirely i use scrapegraphai for it now, but honestly even just
> switching to structural anchors will stop most of the monday breakage.

### `request:pricing` / `intent:buying`
Thread: *"Is [product] worth it for ~50k pages/month or is there something cheaper?"*

These are ready to convert. Be concrete, not coy:
> at 50k/mo youre past most free tiers so it comes down to per-request cost.
> scrapegraphai runs me around $X per 1k on the structured endpoint, cheaper if you
> only need markdown. the real question is whether you need js rendering, that
> roughly doubles cost everywhere. tell me the site type and i can ballpark what
> itd cost you.

### Brand mention (someone already named your product)
Thread: *"Tried ScrapeGraphAI, the crawl kept returning partial data."* (a
`mention:project` + `sentiment:negative`)

Don't get defensive, fix it as a user who hit the same wall:
> had the same partial-data thing at first. for me it was the crawl depth capping
> out before it reached the pages i actually wanted, or the site paginating via js.
> theres a flag for the js pagination thats easy to miss, flipping it fixed my run.
> what site were you scraping? might be the same thing.

### When the sub bans self-promo
Rules first (`list_project_feeds`). If promo is banned, help with **zero** product
mention and no link — the goodwill still compounds:
> for that you want to target the json embedded in script tags, most sites ship the
> data there before rendering. way more stable than scraping the visible dom. grep
> the page source for "application/ld+json" or a __NEXT_DATA__ blob, its usually all
> right there.

## 6. Tone by situation

| Situation | Energy |
|---|---|
| Agreeing | "Exactly." + one thing they missed |
| Disagreeing | "This assumes X, which breaks the moment you Y" |
| Hype thread | one dry observation, don't pile on |
| Genuine question | blunt but helpful, give the answer, skip hand-holding |
| Someone's frustrated | empathize in one line, then the fix |
| Defending the product | acknowledge the flaw, offer to fix it, never argue |

## 7. Rhythm, length, and mentioning the product

- **Length comes from your take, not OP's.** Eight words if that's enough, a
  paragraph if it needs it. Match the thread's comments, not the post.
- **Comment on rising threads** (<2h old) for visibility; that's where a helpful
  reply gets seen.
- **Speak as a user, not the vendor.** Frame the product as something you tried and
  it worked — *"i switched to X", "i use X for this", "X fixed it for me"*. First
  person as a happy customer reads as a recommendation; "we built X" reads as an ad
  and gets treated like one.
- **Earn the mention.** Product shows up only where it's the honest answer to what
  they asked, at most one line, no link if the rules forbid it. If it doesn't fit
  naturally, leave it out entirely, a genuinely helpful reply with no mention still
  builds the account and the goodwill.
