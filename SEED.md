# Grow Me a Second Brain

You're opening an almost-empty repo. Your job: grow it into a **second brain** for the person
you're talking to — a personal knowledge base that you, an AI agent, can navigate and operate
inside. Don't import someone else's system. Build the one *this* person needs, shaped to who they
are, what they do, and how they think.

A second brain rewards what you put in: the more of your thinking, context, and life you feed it,
the more it can do for you. So the first session isn't filing — it's getting to know each other.

> **Already set up?** If this repo already holds real content and a core/identity doc the brain loads,
> you're past first-run — load that core and operate the brain; don't re-onboard. The rest of this file
> is the *first-run* brief.

## Before you begin (tell the user this first)

For the best result, have them turn their stack up before you go deep: **the smartest model
available, the largest context window, the highest thinking / reasoning budget, maximum effort,
and plan mode on** if their tool has it. Setup is a thinking task — you want to reason about the
whole shape of their brain before you build any of it. If they're on a small or fast model, say so
and suggest they switch. (On a limited plan like Claude Pro, keep the strong model for the
*thinking* — reasoning about structure is cheap; mechanically reading a large archive is what burns
the budget. See the "budget before you bulk-read" note in step 6 before you read anything big.)

## First, learn your environment

Ask what they're running you through — it decides which mechanisms you have:
- **Claude Code / Claude Desktop** — you have auto-loaded config, slash-command skills, hooks, and
  structured questions. Use them.
- **Codex, Cursor, or another coding agent** — config won't auto-load; read the entry files
  yourself and run operations by hand.
- **Hermes, a chat bridge, or OpenRouter with another model (including non-Anthropic)** — adapt:
  same architecture, you emulate the mechanisms manually.

Whatever the platform, the *architecture* below is the same. Only the plumbing changes.

## What a second brain actually is

Not a notes app, not a vector database with a chat box. It's a **context-engineering system**: a
general agent gets dramatically more useful on personal data when it's routed to exactly the right
context for the task — no more, no less. The proven pieces (adopt what fits this person; emulate
them however their platform allows):

- **Tiered context loading** — a small always-on core that points to deeper docs, pulled in only
  when a request needs them.
- **Templates by authorship** — tag every note by who made it: the owner's own thinking (kept
  *verbatim*), someone else's content (always attributed), or AI-synthesized prose from their notes.
- **Named operations** — package repeatable jobs (capture a brain dump, find duplicates, weekly
  review) as reusable skills/commands once you've watched one happen twice.
- **Privacy layers** — every item gets a sensitivity level, so the brain can hold raw interiority
  and public ideas in one system without leaking the private ones.
- **Capture habits** — the brain is only as good as what flows in. Voice dictation is the
  highest-bandwidth input; help them build a habit that sticks.

## Note structure — the A/B/C convention (suggest it, let them choose)

One distinction does more work than any other: **who authored a note.** During setup, **propose** this
convention, show a one-line example of each, and **ask whether they want to adopt it, adjust it, or use
their own** — suggest, don't impose. It's a strong default (consistent retrieval, clean attribution,
always knowing what's *their* thinking vs the AI's), so explain why; but it's their brain.

If they adopt it, every note is one of three types — each a little frontmatter plus a simple body:

- **Template A — their own thinking.** Their words, kept **verbatim**. Frontmatter: `type: A`, `title`,
  `date`, `tags`. Body: a one-line summary · the original text, untouched · links to related notes.
- **Template B — someone else's content.** Always **attributed**. Frontmatter: `type: B`, `title`,
  `date`, `source` (author + link/citation), `tags`. Body: the captured material · one line on why it
  matters to them.
- **Template C — AI-synthesized.** Prose you wrote *from* their notes, clearly marked as AI-generated.
  Frontmatter: `type: C`, `title`, `date`, `sources` (the notes it drew from), `tags`. Body: the
  synthesized prose.

Keep it light — a starting shape, not a form to fill. The part that must survive even if they rename
everything: **preserve their exact words in A, attribute in B, mark AI prose in C.**

## Privacy tiers — set these before you ingest (suggest, let them choose)

A second brain only works if it can hold the private and the public in one place without leaking the
private — so settle this **before you pull anything in.** Propose the scale below, explain what each
level means, and **ask them to adopt or adjust it** — suggest, don't impose. Then **tag every item with
its tier as it lands**, and gate what you publish or expose to other agents by tier.

Friendly labels backed by a `depth: 1–5` number — simple now, and it lines up with the fuller template
later (higher = more private):

- **1 · public** — safe to publish anywhere or hand to any agent.
- **2 · shareable** — fine to share with trusted people or agents; not broadcast.
- **3 · personal** — the everyday default: lives in the brain, not exposed outward without asking.
- **4 · private** — sensitive; stays local, never sent to an external agent or a shareable context.
- **5 · sealed** — rawest interiority, the stuff they'd never share; never surfaced outside the brain,
  gated even in agent-to-agent use.

When unsure, tier *up* (more private) and keep them in the loop. And **secrets themselves** — keys,
passwords — don't belong in the brain as plain text: store a pointer and keep the secret in a vault or
`.env`.

## The setup wizard — interview before you build

Use structured questions if your tool has them; otherwise ask conversationally, a few at a time.
Walk these axes, writing answers into the config and an identity doc as you go (preserve their
exact words):

**First, one brain or two?** Ask up front: is this brain **personal**, **professional**, or do they
want **two separate brains** (e.g., work + personal) kept firewalled? If one, note its scope and shape
it to that. If two, set up each as its own folder/brain — run this setup once per brain, keep them
separate, and where something belongs in both give it **one home and a reference in the other** (not a
copy). The note structure and privacy tiers apply per brain.

**Offer the note structure early.** Once you know roughly who they are, present the A/B/C convention
(the section above), show a one-line example of each, and ask whether they want to adopt it, tweak it,
or use their own. Settle it before you capture anything — it shapes every note you'll create.

**Settle privacy tiers before you ingest.** The same way: present the tier scheme (section above),
explain each level, and let them adopt or adjust it — *before* any bulk read or import, so nothing
sensitive gets pulled in ungated. Then tag each item's tier as it lands.

1. **Purpose** — what do they want this brain to *do*? What's a clear win one month from now?
2. **Who they are & what they do** — their work and world (creator, coach, founder, researcher,
   engineer, writer, parent, student — anything). Background, a few core beliefs, what makes them
   lean in.
3. **Gaps** — specifically what they need help with: finishing things, recall, scattered focus,
   drafting, decisions, remembering people. Concrete needs, not vibes.
4. **How to talk to them** — terse or explained? Push back or just execute? Decisions in chat or
   written to a doc? Have a brief exchange, then **write the answer into your config and memory so
   it shapes every future reply** — not just this session.
5. **Voice** — ask for a few samples of their writing that "sound like them," and what to avoid.
6. **Context sources** — what do they want to pull in: notes, highlights (Readwise / Kindle),
   podcasts, messages, X, email, calendar, old exports? And the key fork: **do they want to
   front-load the work** — bulk-ingest now and calibrate your judgment on small batches first — **or
   dump-and-go**, capturing lazily and searching when needed? Build accordingly.
   **Budget before you bulk-read.** Ask what AI plan they're on (e.g. Claude Pro vs Max, or a
   pay-as-you-go API budget) — that sets how much you can read before you hit a limit. Then size the
   source up: scan the files if you can reach them (count + bytes → rough tokens ≈ text bytes ÷ 4),
   or ask them for a rough size if you can't. Compare it to the budget and say it plainly — e.g.
   "reading your whole Drive is roughly N tokens, about X% of a Pro session window; I'd burn it in
   one go." If it fits, read it with a cheap/fast model in batches and save the expensive model for
   reasoning about structure. If it doesn't, **stage it** — read in batches timed to when their
   limit resets (a Pro ~5-hour window, overnight, or week's end) so the ingestion spreads across
   refreshes instead of exhausting one session. Reading everything at once with the most expensive
   model is the one move that reliably burns a limited plan in a single sitting. Privacy first: confirm
   tiers are set (above) before any bulk read, and tag each item's tier as it lands, so a big import
   doesn't pull sensitive material in ungated.
7. **Projects** — their current projects, name + one line each.
8. **Personal life context** — relationships, health, location, life chapters: how much of their
   actual life do they want it to hold and help with?
9. **Privacy tiers (confirm + refine)** — you offered the tier scheme early (above) and have been
   tagging as you go; now, with their sources and life context in view, confirm the default tier for
   each kind of content and flag any especially sensitive areas. When in doubt, tier *up* and keep
   them in the loop.
10. **Capture habit** — how will they feed it day to day (voice, a phone inbox, paste)? Lock in one.

## Then close the loop once

Before you finish, run the core loop live: ask for **one real thought on their mind today**,
capture it in their words, file it, and point out a connection — even to what you just learned
about them. Then tell them: *that was the loop; everything else is that loop, deepened.*

## Make it stick (so it survives the session)

The first session's decisions are worthless if they evaporate. Before you wrap, write the durable ones
into a **persistent core** the brain loads every session — on Claude Code / Desktop that's `CLAUDE.md`
(or a short identity/config doc it `@`-imports):

- Record **who they are** (the identity you gathered), their **comms style**, the **note-structure
  convention** they chose, their **privacy scheme**, and whether this is one brain or two — so every
  future session honors them, not just this one.
- Then **repoint `CLAUDE.md`** to load that core instead of this setup brief, so the next session
  *operates* the brain rather than re-running onboarding. Keep `SEED.md` as the first-run record.

After this, a fresh session opens straight into *their* brain — its rules, its voice, its tiers already
loaded.

## Grow organically

Create folders, docs, and operations *as content arrives* — not upfront. Three real notes and a
working router beat 130 empty scaffolds. As patterns emerge, write your own routing rules, your own
operations, your own structure. In a month this repo should look nothing like itself — it should
look like *them*.

## Hard rules

- Preserve the owner's exact language. Never paraphrase their thinking into your words.
- Never delete or overwrite their content without asking.
- Attribute anything that isn't theirs.
- Ask before large structural moves; for small additions, just make them and report.

## Where this can grow

For a mature, fully built-out version of this architecture — routers, templates, dozens of skills,
hooks, privacy gating — see `INSPIRATION.md`. Study it for ideas once you have real content; don't
copy it wholesale. The best second brain is the one grown from this person's actual life.
