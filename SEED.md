# Grow Me a Second Brain

You're opening an almost-empty repo. Your job: grow it into a **second brain** for the person
you're talking to — a personal knowledge base that you, an AI agent, can navigate and operate
inside. Don't import someone else's system. Build the one *this* person needs, shaped to who they
are, what they do, and how they think.

A second brain rewards what you put in: the more of your thinking, context, and life you feed it,
the more it can do for you. So the first session isn't filing — it's getting to know each other.

> **Returning to this file? The test is the core doc, not the content.** If a filled core/identity
> doc exists and the entry files load it, you're past first-run — load that core and operate the
> brain; don't re-onboard. **If the repo holds real content but NO filled core doc, setup is
> INCOMPLETE — no matter how lived-in it looks.** Don't re-interview what's already answered:
> compile what exists into the core and resume at the persistence gates below. The rest of this
> file is the *first-run* brief.

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
  structured questions. Use them. (Claude Code also runs in the browser at claude.ai/code against a
  GitHub repo — no terminal needed; config and skills load there, hooks don't.)
- **Codex, Cursor, or another coding agent** — config won't auto-load; read the entry files
  yourself and run operations by hand.
- **Hermes, a chat bridge, or OpenRouter with another model (including non-Anthropic)** — adapt:
  same architecture, you emulate the mechanisms manually.

Whatever the platform, the *architecture* below is the same. Only the plumbing changes.

## Make the repo theirs (before the first commit)

If this was cloned from the public seed repo, `origin` still points at it — which means a naive
`git push` months from now targets a stranger's repo with the owner's private thinking. **Before
any commit lands: `git remote remove origin`** (tell them why in one line). The owner picks their
remote posture later — a private repo of their own, or local-only — and you record the choice in
the core doc; detaching now is what makes that a real choice instead of an accident.

## One core, many doors (adopt this convention from the start)

The brain's durable config — who the owner is, how to talk to them, the conventions they chose,
their privacy scheme, their standing reminders — lives in **one platform-neutral core doc**:
`BRAIN_CORE.md` at the repo root. Every agent entry file is a **thin pointer to it**:

- `CLAUDE.md` — Claude Code / Claude Desktop auto-load this; it imports the core (`@BRAIN_CORE.md`).
- `AGENTS.md` — Codex, Cursor, and most other agents read this; it says "read `BRAIN_CORE.md`
  first, then proceed."

Keep BOTH pointer files present and current even if the owner uses only one tool today — the core
is plain markdown, so the convention costs nothing and the brain stays portable across vendors. If
the owner is provider-agnostic on principle, this IS the answer to that principle: the identity
lives in neutral files they own, and each vendor's entry file is a disposable three-line pointer.

The rule that keeps it healthy: **every config file in the repo is either filled or deleted —
never blank.** A blank `CLAUDE.md`/`AGENTS.md` is worse than a missing one: it looks configured,
loads nothing, and leaves the agent with no written identity — which is how an agent ends up
refusing work as "not its job" or passively accompanying the owner instead of executing.

**Watch for the bypass pattern.** Some owners route around config-writing without meaning to:
they arrive with a pre-written spec of their own (see the fast-path in the wizard below), they
pre-build their own folder structure, or their portability principles make vendor-named files
feel like lock-in. Whatever the route in, your job is unchanged: an always-loaded, filled core
on every platform the owner uses.

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
  review) as reusable skills/commands once you've watched one happen twice. One worked example ships
  in `.claude/skills/` — the `/nano-banana-flash` + `/nano-banana-pro` image-generation pair (add a
  free `GEMINI_API_KEY` to `.env` to use them); copy their shape when you package this person's own
  operations.
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

**Gate 0 — before you ask anything: create `BRAIN_CORE.md` now.** Seed it with what little you
already know (platform, date, owner's name if you have it) plus one line: *"Setup in progress —
resume the wizard at question N."* Then repoint `CLAUDE.md` and `AGENTS.md` at it (thin pointers,
per the convention above). A partial core that survives a dropped session beats a perfect core
that never got written — first sessions get cut short by time, tokens, and life, and the tail of
a session is exactly the part that dies. From here on, persistence happens at **gates**, not at
the end:

- **Gate 1 — after questions 1–4** (purpose, who they are, gaps, comms style): write those into
  the core, in their exact words, before moving on.
- **Gate 2 — after the conventions are settled** (note structure + privacy tiers): write the
  chosen conventions in.
- **Gate 3 — at wrap** ("Make it stick" below): standing reminders, memory split, durability.

If the session ends between gates, the core says exactly where to resume.

**Arriving with a spec? (the fast-path)** If the owner hands you a pre-written intent spec or any
purpose doc of their own — power users do this — file it verbatim as `INTENT_SPEC.md`, skip the
questions it already answers, and **compile its durable decisions into `BRAIN_CORE.md`
immediately. That's Gate 1 arriving early, never a reason to skip the gates** — a spec sitting in
a file nothing auto-loads configures nothing.

Walk these axes, writing answers into the core as you go (preserve their exact words):

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

## Make it stick (Gate 3 — the wrap)

If you've been passing the gates, `BRAIN_CORE.md` already holds their identity, comms style,
conventions, and privacy scheme. At wrap, complete it:

- Sweep the session for **durable decisions the gates didn't catch** — one brain or two, capture
  habit, anything they corrected mid-session — and write them into the core in their words.
- **Verify the Gate-0 repoint still holds:** `CLAUDE.md` and `AGENTS.md` both point at the core,
  neither still routes to this setup brief, and neither is blank. Keep `SEED.md` as the first-run
  record.
- On Claude Code, also record the durable decisions in its **auto memory** (on by default — it loads
  its `MEMORY.md` index every session). Division of labor: the core doc carries the
  *owner's instructions*; memory accumulates *your learnings about them* — corrections, preferences
  discovered mid-session, what worked. Memory lives on the machine
  (`~/.claude/projects/<project>/memory/`), not in the repo — it won't travel with a clone, which is
  one more reason the durability choice below matters.
- Keep a short **standing-reminders list** in the core doc — open commitments and in-progress
  intentions that every fresh session surfaces until they're done. It's how a promise made on
  Monday survives to Thursday without the owner having to remember to re-mention it.

## Done means verified — the fresh-session check

Setup isn't finished when the files exist; it's finished when a **fresh session proves they
load.** Have the owner close this session, open a new one, and ask: *"Who am I, and how do we
work together?"* The answer must come from their core — their name, their comms style, their
conventions — not generic assistant boilerplate. Until that check passes, setup is **UNVERIFIED**:
say so plainly, and make fixing whatever didn't load the first job of the next session. (On
platforms where nothing auto-loads, the equivalent check: open a fresh session, say "read the
entry file and begin," and the same question must come back answered from the core.)

## Make it durable — version it from day one

A brain that holds someone's thinking should never be one crash or one bad edit from gone. Set this
up early — ideally before the first real note lands:

- **Check for `git`** (`git --version`); if it's missing, offer to install it (macOS:
  `xcode-select --install`; Windows: git-scm.com; Linux: the distro package manager).
- **`git init` the brain and commit as you go** — every ingestion or structural change gets a commit.
  History doubles as an undo button and a record of how the brain grew — and an audit trail: when
  your behavior changes and the owner asks "why did you start doing that?", you can trace the
  answer through the history to the day a rule was added. The owner never needs to touch git
  themselves; you drive it.
- **Offer a GitHub home.** Suggest a GitHub account and the `gh` CLI (`gh auth login`), then let them
  choose — and record the choice in the core doc:
  - **Private GitHub remote** — off-machine backup, and it unlocks running the brain from the browser
    at claude.ai/code (no terminal needed; skills work there, hooks don't). Remind them everything
    rides along, including the most private tiers — so private repo, their own account, their call.
  - **Local-only + a backup habit** — most private; with no remote the laptop is the single copy, so
    pair it with something real (an external drive, a synced folder).

## Grow organically

Create folders, docs, and operations *as content arrives* — not upfront. Three real notes and a
working router beat 130 empty scaffolds. The brain doesn't know what it should be until it sees
the content the person is trying to organize — so **ingest before architecting**, and resist the
urge to perfect a structure (or a pretty human-facing front-end) for material that isn't in yet.

Two structural defaults that pay off as it grows:
- **The brain is the root of everything** — like the `C:\` of their digital life. Their code
  projects, automations, and working documents live *inside* it (a `Projects/` folder), not beside
  it, so one agent context reaches all of it.
- **Every project follows the same shape** (a status/plan doc, same sections each time) — uniform
  structure is what lets you operate ten projects as easily as one.

As patterns emerge, write your own routing rules, your own operations, your own structure. In a
month this repo should look nothing like itself — it should look like *them*.

## Hard rules

- Preserve the owner's exact language. Never paraphrase their thinking into your words.
- Never delete or overwrite their content without asking.
- Attribute anything that isn't theirs.
- Ask before large structural moves; for small additions, just make them and report.

## Where this can grow

For a mature, fully built-out version of this architecture — routers, templates, dozens of skills,
hooks, privacy gating — see `INSPIRATION.md`. Study it for ideas once you have real content; don't
copy it wholesale. The best second brain is the one grown from this person's actual life.
