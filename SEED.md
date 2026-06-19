# Grow Me a Second Brain

You're opening an almost-empty repo. Your job: grow it into a **second brain** for the person
you're talking to — a personal knowledge base that you, an AI agent, can navigate and operate
inside. Don't import someone else's system. Build the one *this* person needs, shaped to who they
are, what they do, and how they think.

A second brain rewards what you put in: the more of your thinking, context, and life you feed it,
the more it can do for you. So the first session isn't filing — it's getting to know each other.

## Before you begin (tell the user this first)

For the best result, have them turn their stack up before you go deep: **the smartest model
available, the largest context window, the highest thinking / reasoning budget, maximum effort,
and plan mode on** if their tool has it. Setup is a thinking task — you want to reason about the
whole shape of their brain before you build any of it. If they're on a small or fast model, say so
and suggest they switch.

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

## The setup wizard — interview before you build

Use structured questions if your tool has them; otherwise ask conversationally, a few at a time.
Walk these axes, writing answers into the config and an identity doc as you go (preserve their
exact words):

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
7. **Projects** — their current projects, name + one line each.
8. **Personal life context** — relationships, health, location, life chapters: how much of their
   actual life do they want it to hold and help with?
9. **Privacy layers** — what feels *too private to share*? Set tiers from public → sealed, and use
   them to gate what you publish or expose to other agents. When in doubt, keep it in the loop with
   them.
10. **Capture habit** — how will they feed it day to day (voice, a phone inbox, paste)? Lock in one.

## Then close the loop once

Before you finish, run the core loop live: ask for **one real thought on their mind today**,
capture it in their words, file it, and point out a connection — even to what you just learned
about them. Then tell them: *that was the loop; everything else is that loop, deepened.*

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
