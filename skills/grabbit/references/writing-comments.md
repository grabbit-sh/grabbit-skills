# Writing Reddit replies that convert (and don't read like ads)

The reply is the whole product. Grabbit finds the lead; a reply that reads like
marketing wastes it — downvoted, removed, sometimes the account banned. A reply
that reads like a helpful regular wins it. The entire craft is three things:
**sound like a real person in *that* sub, help before you sell, and mention the
product only when it's the honest answer.**

Everything below is a field-tested method for writing Reddit-native comments. It
uses placeholders — `[your product]`, `[competitor]`, `[the pain]` — because the
same technique works for any brand. Fill them from the project's
`problems`/`solutions`/`targetAudience` in `list_projects`.

## Contents
1. Before you type — read the room
2. The core move — ghostwrite a real commenter
3. Study real Reddit voices (product-agnostic samples)
4. Why AI replies get spotted
5. The AI-tell filter (run on every draft)
6. Mechanical humanization
7. The mention — help first, earn it, user voice
8. The final gate before you hand it over
9. Account strategy (so the account survives to sell)

---

## 1. Before you type — read the room

Never draft blind. Pull the thread with `get_reddit_thread(url)` and, if the sub is
a project feed, `list_project_feeds(projectId)` for its rules. In ~30 seconds
capture five things — this is what separates a native comment from an obvious
outsider:

- **Tone** — raw/vulnerable? dry/sarcastic? technical? buttoned-up?
- **Formatting** — plain text walls, or bullets/bold? (most casual subs: plain)
- **Capitalization** — proper caps, all-lowercase, or mixed?
- **Swearing** — common, occasional, or never?
- **What gets upvoted** — stories? blunt hot takes? specific data? one-liners?

And the one hard gate: **the rules**. Can you name a product? Link? Many subs ban
self-promo outright (`r/startups`, `r/ExperiencedDevs`) — there you help with zero
mention. Others (`r/SideProject`) invite it. The rules decide what's even possible.

## 2. The core move — ghostwrite a real commenter

This is the single highest-leverage step. In the thread, find the **top-upvoted,
natural-sounding comment** (skip mods and other ads) and write *as that person* —
not "inspired by," actually ghostwrite them. Copy six things:

- **Sentence length and rhythm** — short jabs vs. long run-ons
- **Punctuation habits** — do they skip commas? use periods as beats? ALL CAPS?
- **Paragraph shape** — one fat block, or lots of little ones
- **Vocabulary and register** — jargon-heavy vs. plain talk
- **Messiness** — fragments, lowercase, missing apostrophes, typos left in
- **Emotional directness** — blunt, warm, dry, ranty

The sample gives you *how* to write. The lead's classification labels (section 7)
give you *what* to say. No good comment yet? Match the **post's** voice instead.

## 3. Study real Reddit voices

These are verbatim human comments from real threads — no product in them, just
texture. Notice how *unlike* an LLM they are. This is the bar.

**One-sentence reaction, 2,415 upvotes:**
> It's funny how accurate this is to my current situation

**Blunt two-liner, 343 upvotes:**
> No, games aren't CRUD, generally embedded isn't, or creative apps like Adobe, or
> industrial.
>
> No magic to it, apply for jobs you like the look of.
> *Dismissive but genuinely helpful. Zero padding, zero hedging.*

**All-lowercase dry rant, trailing off:**
> it was exhausting doing 3 months of engineering roadmap planning and then, 2 weeks
> later, the CEO would barge in and demand we throw G into the mix. and also Q...
>
> now i work for big tech. it's a different type of bullshit. but at least i get
> paid better
> *Lowercase throughout, trailing ellipsis, dry final line doing the emotional work.*

**Practical advice, note the typo left in, 244 upvotes:**
> I've been through this. Best thing to do it document and explain but don't get too
> emotionally invested. At some point upper management is going to be disappointed
> and you want to be able to defend that you tried.

**One fat paragraph, ALL CAPS for emphasis instead of bold:**
> We get ~80 page outs per week, not even joking. 99% of which are false alarms...
> everyone seems to agree its absolutely insane, but we MUST have these alarms, set
> by SRE. If I don't wake up within 5 min it escalates. And they happen MULTIPLE
> times a night.
> *No structure, ALL CAPS not bold, "its" without apostrophe, hard numbers.*

The pattern across all of them: **uneven length, specific numbers, no announced
structure, typos survive, emphasis is ALL CAPS not markdown, and they stop when the
thought stops** — not with a neat summary.

## 4. Why AI replies get spotted

Reddit has a finely tuned nose for AI. The tells, each with the fix:

**AI writes symmetrically. Humans don't.**
> AI: three paragraphs, two sentences each, all the same size.
> Human: one fat block of six sentences, then a one-liner. Wildly uneven.

**AI covers every angle. Humans fixate.**
> AI: "On one hand X, on the other hand Y — both have merits."
> Human: "honestly the migration isnt worth it, we tried and rolled back in a week"

**AI over-punctuates. Humans run thoughts together.**
> AI: "It's fast. I've benchmarked it. However, the cost is steep."
> Human: "its fast ive benchmarked it but the cost gets rough at scale"

**AI announces its points. Humans just say them.**
> AI: "Here's the thing: most people don't actually need [category]."
> Human: "most people dont need [category] they need like 3 things it does"

**AI mirrors OP's structure. Humans don't.** Your length comes from *your* take, not
from matching OP's paragraph count.

## 5. The AI-tell filter (run on every draft)

Mechanical check. If any of these appear in the drafted reply, rewrite — they out
an LLM instantly:

- **Banned words**: crucial, notable, significant, essential, fundamental,
  straightforward, delve, landscape, realm, navigate (metaphorical), foster,
  facilitate, pivotal, nuanced, robust, comprehensive, multifaceted, intriguing,
  compelling, leverage, synergy, seamless, streamline, game-changer, unlock, empower
- **Banned phrases**: "it's worth noting", "the reality is", "what's interesting
  is", "in essence", "at the end of the day", "at its core", "great question",
  "happy to help", "hope this helps"
- **Banned transitions**: However, Furthermore, Additionally, Moreover, That said,
  In conclusion → use *but, and, also, tho, so*
- **Banned punctuation**: em dashes (—), semicolons, colon-before-an-explanation
- **No hedging openers**: don't start with "I think", "in my opinion", "to be fair"
- **No sucking up**: never open with "Great question!", "Thanks for sharing!" —
  respond to the content, not the person
- **No formatting in casual subs**: no bullets/bold/numbered lists unless the sub's
  own top comments use them

## 6. Mechanical humanization

Real Redditors don't write like press releases. Borrow the texture of the sample
you picked — not as a trick, but because clean corporate prose *is* the tell:

- **Drop 1-3 apostrophes** across the reply: *dont, youre, isnt, thats, wont, its*
- **Occasionally start a sentence lowercase** (or the whole comment)
- **ALL CAPS for emphasis**, never bold or italics
- **Uneven paragraphs**, fragments are fine, trail off instead of concluding
- **Numbers and specifics beat adjectives** — "cut it from ~2h to 20min" lands;
  "dramatically faster" is noise

Calibrate to *that thread's* level — don't paste sloppiness onto a buttoned-up sub.

## 7. The mention — help first, earn it, user voice

The lead's classification labels hand you the opening. **Speak as a user who tried
it, not the vendor** — "i switched to X", "X fixed this for me" reads as a
recommendation; "we built X" reads as an ad and dies. Product mention is at most one
line, no link if the rules forbid it, and only where it's the honest answer.

### `request:recommendation` / `intent:comparing`
Thread: *"what's everyone using for [category]? [competitor] keeps [failing at X]."*

❌ Ad:
> Great question! For [category] at scale, [your product] leverages a robust,
> seamless approach to streamline your workflow without the usual headaches...

✅ Honest shortlist, user voice:
> [competitor] failing at X is usually [root cause]. depends what you need, if you
> just want [basic case] the free options are fine. i needed [specific case] so i
> switched to [your product], mainly because [the one concrete thing]. not magic but
> it stopped [the recurring pain]. worth a trial before you commit tho.

### `competitor:complaint` / `competitor:feature_gap`
Thread: *"[competitor] raised prices again and still no [feature]. looking around."*

❌ Ad:
> Sorry to hear that! [your product] offers a compelling alternative with a
> comprehensive [feature] that empowers teams to...

✅ Empathize, then solve the *specific* gap:
> the [missing feature] thing got me too, ended up hacking around it which was dumb.
> moved to [your product] mostly because [feature] was there out of the box. pricing
> is [model] so it scales with usage instead of punishing you. try it before you
> commit tho, your [use case] might differ.

### `experience:pain_point` / `request:advice`
Thread: *"[spent hours on the painful thing] and it broke again. i want to quit."*

❌ Ad that ignores the human:
> This is a common pain point! [your product] solves [the pain] by...

✅ Answer the real pain first, product optional and last:
> yeah [the pain] is a losing game when [condition]. two things that helped me,
> [genuinely useful tip 1], and [tip 2] instead of [the brittle approach]. if you
> want to skip it entirely i use [your product] for it now, but honestly even just
> [the free tip] will stop most of the breakage.

### `request:pricing` / `intent:buying`
Thread: *"is [tool] worth it at [scale] or is there something cheaper?"*

These are ready to convert. Be concrete, not coy:
> at [scale] youre past most free tiers so it comes down to [cost driver].
> [your product] runs me around [$X per unit], cheaper if you only need [basic
> tier]. the real question is whether you need [the expensive feature], that roughly
> doubles cost everywhere. tell me your [key variable] and i can ballpark it.

### Negative brand mention (`mention:project` + `sentiment:negative`)
Someone already named your product and hit a wall. Don't get defensive — fix it as
a user who hit the same thing:
> had the same [problem] at first. for me it was [the real cause], or [the config
> gotcha]. theres a [setting/flag] thats easy to miss, flipping it fixed my run.
> what [context] were you on? might be the same thing.

### When the sub bans self-promo
Rules first. If promo is banned, help with **zero** product mention and no link —
the goodwill still compounds and the account stays clean:
> for that you want [the genuinely useful technique], its way more stable than
> [the common wrong approach]. [one concrete specific]. that alone fixed it for me.

## 8. The final gate before you hand it over

Before presenting a draft, it must pass all of these — same bar the account holds
itself to:

- [ ] Register matches a real comment in *that* thread (section 1–2)
- [ ] Passes the AI-tell filter — no banned words/phrases/transitions (section 5)
- [ ] No em dashes, no semicolons
- [ ] Opens with actual help, not a pitch or a compliment
- [ ] Product mention is earned, ≤1 line, framed as your own experience
- [ ] No link if the sub's rules forbid it
- [ ] Adds something not already said in the thread

Grabbit doesn't post for the user. Output the reply plus the thread url so they post
it in their own account, then `set_entry_status(entryIds, "replied")`.

## 9. Account strategy (so the account survives to sell)

A reply is only as good as the account behind it. An account that only ever shows
up to plug a product gets pattern-matched as a shill and shadowbanned.

- **~3 helpful comments for every 1 that mentions the product.** Build reputation
  first; the mentions land far better from an account with real history.
- **Comment on rising threads (<2h old).** That's where a good reply gets seen and
  upvoted; old threads are dead air.
- **Focus 4–5 subs**, not every sub at once — depth reads as belonging, spray reads
  as spam.
- **End messy.** A trailing thought or a question beats a neat summary; summaries
  are an AI tell.
- **Length comes from the room**, never a fixed size — match the thread's comments.
- **Never fabricate.** You can share experience and opinion freely, but don't invent
  benchmarks, prices, or outcomes you can't stand behind. Getting caught lying is
  the one thing that kills a brand faster than spamming.
