---
depth: 2
---
# Second Brain Seed

> **DOWNLOAD AS ZIP** — the green **Code** button above → **Download ZIP**. Don't clone unless you
> know what a remote is: a clone keeps pointing at this public repo, and a `git push` months from
> now would send your private thinking here. Unzip it into the folder you want your brain to live in
> (name the folder what you'll call the brain); the agent sets up version history fresh.

> **Here for a session with Lorin?** Book at [loringtonian.github.io/links](https://loringtonian.github.io/links/).
> More about the Seed, sessions and the community: [trimtablabs.xyz](https://trimtablabs.xyz/).

One prompt that grows into your own AI second brain. No 130-file template to strip down — just a
seed an agent reads to build a personal knowledge base shaped to *you*.

Download it, open it in your AI agent **from inside that folder**, and say **"build my second brain."** The agent reads `SEED.md`,
interviews you (purpose, your work, your gaps, your voice, what to pull in, how private to keep
things), and grows the structure around what you actually put in.

> **Turn your stack up first.** Best results come from the smartest model, the biggest context
> window, the highest thinking budget, and plan mode on — setup is a thinking task. (More in → more
> out.)

## Start

- **Claude Code** — the terminal, or the **Code** tab of the Claude Desktop app (*not* Cowork: it has
  no command line or git, so setup can't run there): start Claude Code **inside the brain folder**
  (memory and config load from the folder you start in) and say *"build my second brain."*
  (`SEED.md` auto-loads via `CLAUDE.md`.) During setup the agent gives you a one-word shortcut — a
  shell alias named whatever you call the brain (`2b`, `exo`, `cortex`, `sec`, `brain`…) — so every
  later session opens in the right folder without the cd-then-launch ritual. If it doesn't, ask for one.
- **Codex / Cursor / other coding agents:** point it at `AGENTS.md`, or paste `SEED.md` as your
  first message.
- **Any other model (Hermes, OpenRouter, etc.):** paste `SEED.md` as the system / first prompt.

That's it. Answer a few questions, capture one real thought, and you have a working brain that grows
from there.

**Already know what you want, or short on patience?** Hand the agent a few lines on who you are and
what the brain is for. It files them as your spec, skips every question they answer, and still
writes the core doc — the short path is built in.

The seed ships one example skill pair — `/nano-banana-flash` and `/nano-banana-pro` (image
generation; add a free `GEMINI_API_KEY` to `.env`) — as the pattern for growing your own, and three
note templates in `templates/` (your own thinking · others' content · AI synthesis) so every note
starts from the same shape.

## Where it can grow

Get through setup and live with your brain for a week, and it will tell you how to see a fully
built-out one — routers, templates, dozens of skills, hooks. Overkill on day one.
