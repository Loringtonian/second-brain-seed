# Anthropic's official statements on context-window degradation and context management (Claude models + Claude Code), as of 25 Nov 2026

Reading notes for the writer:
- All pages were fetched on 2026-11-25. Docs pages (code.claude.com, platform.claude.com) are undated living documents; treat them as "current as of Nov 2026". Blog/engineering posts carry their own dates.
- Quotes marked "verbatim" were returned as page text. Quotes from engineering/blog posts came back through a summarising fetch tool as short excerpts; they are believed verbatim but are fragments, so do not stitch them into longer sentences.
- Items marked UNVERIFIED come from search-result snippets only (the primary page was not opened).

## 1. What does "Effective context engineering for AI agents" (Sept 2025) say, and what later engineering posts (2025-2026) cover long-running agents, harness design or context management?

### Takeaway
Anthropic's Sept 29, 2025 post is the foundational statement: context is a finite "attention budget", recall degrades as tokens grow ("context rot"), and the remedies are compaction, structured note-taking, sub-agents and just-in-time retrieval. Three later posts (Nov 2025, Mar 2026, Apr 2026) add that compaction alone is not sufficient for long tasks, name "context anxiety" (Sonnet 4.5), and report that Opus 4.5 removed it, making context resets "dead weight".

### Cited Findings
**"Effective context engineering for AI agents" — Anthropic Engineering, Sep 29, 2025 (still the post the current API docs link to; not superseded)**
- Context rot: as tokens in the window increase, "the model's ability to accurately recall information from that context decreases." — [Effective context engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- LLMs have an "attention budget" that they draw on when parsing large volumes of context. — [Effective context engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- Architectural reason given: transformer attention "results in n² pairwise relationships for n tokens." — [Effective context engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- Compaction defined: "Compaction is the practice of taking a conversation nearing the context window limit, summarizing its contents" (and restarting a new window with the summary). — [Effective context engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- How Claude Code did it at the time (Sept 2025): "passing the message history to the model to summarize and compress the most critical details"; "The model preserves architectural decisions, unresolved bugs, and implementation details"; the agent can then "continue with this compressed context plus the five most recently accessed files". — [Effective context engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- "One of the safest lightest touch forms of compaction is tool result clearing". — [Effective context engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- Structured note-taking: "the agent regularly writes notes persisted to memory outside of the context window." — [Effective context engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- Sub-agents: each sub-agent may use a lot of tokens exploring but "returns only a condensed, distilled summary of its work (often 1,000-2,000 tokens)." — [Effective context engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- Just-in-time retrieval: "agents built with the "just in time" approach maintain lightweight identifiers" (file paths, queries, links) and load data at runtime with tools. — [Effective context engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- Claude Code as a hybrid: "CLAUDE.md files are naively dropped into context up front", while tools like glob/grep fetch the rest just in time; an agent "might employ a hybrid strategy, retrieving some data up front for speed". — [Effective context engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)

**"Effective harnesses for long-running agents" — Anthropic Engineering, Nov 26, 2025**
- "However, compaction isn't sufficient." Even "a frontier coding model like Opus 4.5 running on the Claude Agent SDK" in a loop falls short of a production-quality app from a high-level prompt alone. — [Effective harnesses](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)
- Solution was two prompts: "an initializer agent that sets up the environment on the first run" and "a coding agent that is tasked with making incremental progress in every session", bridged by artifacts: "a claude-progress.txt file that keeps a log of what agents have done", a feature-requirements file (features start failing; agents only flip a `passes` field), and git commits "with descriptive commit messages". — [Effective harnesses](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)
- Named failure modes: "Claude declares victory on the entire project too early."; "Claude marks features as done prematurely."; "Claude leaves the environment in a state with bugs or undocumented progress." — [Effective harnesses](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)

**"Harness design for long-running application development" — Prithvi Rajasekaran (Anthropic Labs), Mar 24, 2026**
- "models tend to lose coherence on lengthy tasks as the context window fills". — [Harness design](https://www.anthropic.com/engineering/harness-design-long-running-apps)
- Defines "context anxiety": models "begin wrapping up work prematurely as they approach what they believe is their context limit." — [Harness design](https://www.anthropic.com/engineering/harness-design-long-running-apps)
- Compaction vs reset: "While compaction preserves continuity, it doesn't give the agent a clean slate." / "A reset provides a clean slate." — [Harness design](https://www.anthropic.com/engineering/harness-design-long-running-apps)
- "we found Claude Sonnet 4.5 exhibited context anxiety strongly enough that compaction alone wasn't sufficient"; later, "Opus 4.5 largely removed that behavior on its own, so I was able to drop context resets from this harness entirely." — [Harness design](https://www.anthropic.com/engineering/harness-design-long-running-apps)
- Cost data: solo run 20 min / $9 vs full harness 6 hr / $200 ("over 20x more expensive, but the difference in output quality was immediately apparent"); the updated Opus 4.6 harness ran 3 hr 50 min / $124.70. — [Harness design](https://www.anthropic.com/engineering/harness-design-long-running-apps)

**"Managed Agents" engineering post — Apr 8, 2026**
- Sonnet 4.5 wrapping up early is "a behavior sometimes called "context anxiety.""; "But when we used the same harness on Claude Opus 4.5, we found that the behavior was gone." "The resets had become dead weight." — [Managed Agents](https://www.anthropic.com/engineering/managed-agents)
- General lesson: "harnesses encode assumptions about what Claude can't do on its own" and "those assumptions need to be frequently questioned". — [Managed Agents](https://www.anthropic.com/engineering/managed-agents)
- On lossy context management: "irreversible decisions to selectively retain or discard context can lead to failures."; their answer is a durable session log that serves "as a context object that lives outside Claude's context window", which a `getEvents` interface "allows the brain to interrogate context by selecting positional slices of the event stream." — [Managed Agents](https://www.anthropic.com/engineering/managed-agents)

### Inferences
- Anthropic's position evolved rather than reversed: Sept 2025 recommends compaction; Nov 2025 says compaction alone is insufficient (need external state files); Mar-Apr 2026 says the model-side cause of early wrap-up (context anxiety) was fixed in Opus 4.5, so harness workarounds should be re-tested per model.
- The "summary is irreversible, raw log is recoverable" idea in the Managed Agents post is the closest Anthropic comes to stating that compaction itself can cause failures.

### Gaps
- The Sept 2025 post gives no token count or percentage at which context rot begins; it describes a gradient, not a threshold.
- Search results also surfaced an anthropic.com/research/managed-agents URL; only the /engineering/ URL was opened. Not checked whether they are the same text.
- I did not find (and did not exhaustively search for) other 2026 engineering posts specifically on context rot measurements.

## 2. What do the Claude Code docs say about the context window (auto-compact, /compact, /clear, /context, tool-result clearing, subagents, CLAUDE.md guidance, "performance degrades" statements)?

### Takeaway
As of Nov 2026 the Claude Code docs have changed tone: auto-compaction by default runs at the model's context limit (about 967K tokens on native-1M models), is configurable in tokens via `/autocompact`, and the docs now explicitly list "the context window is your main constraint, manage it aggressively" as outdated advice. They still say a focused context gives better answers, that early instructions can be lost in compaction, and that CLAUDE.md should be short.

### Cited Findings
**When auto-compaction triggers (current)**
- Verbatim: "If you don't set an auto-compact window, Claude Code compacts when the conversation reaches the model's context limit, except in these sessions": cloud sessions "compact as the conversation approaches the model's limit"; "Sonnet 4.6 and Opus 4.6 without extended context compact at the 200K boundary"; with `CLAUDE_CODE_DISABLE_1M_CONTEXT=1`, native-1M models "compact at the 200K boundary"; "Models running with a native 1M window compact before the window fills, at about 967K tokens by default. These include the Fable models, Sonnet 5 and later, Haiku 5.5, and Opus 4.7 and later." — [Model configuration: default auto-compact thresholds](https://code.claude.com/docs/en/model-config#default-auto-compact-thresholds)
- The docs express the threshold in tokens, not as a percentage. No "% full" trigger is stated anywhere in the pages read. (967K of 1M is roughly 97%, my arithmetic, not Anthropic's wording.) — [Model configuration](https://code.claude.com/docs/en/model-config#default-auto-compact-thresholds)
- Configuration: `/autocompact 500k` (saved per model in `modelSettings`; `/autocompact auto` returns to "the window tuned for your model"); `autoCompactWindow` in settings.json for every model; `--autocompact` flag for one launch; `CLAUDE_CODE_AUTO_COMPACT_WINDOW` env var; per-subagent setting. "The command and the flag accept a window size of 100K tokens or more"; "Claude Code caps it at the model's context window." Per-model saving arrived in v2.1.288. — [Model configuration: set the auto-compact window](https://code.claude.com/docs/en/model-config#set-the-auto-compact-window)
- "The status line's `used_percentage` measures against the model's full context window, so while the variable is set, that percentage doesn't indicate when compaction will run." — [Model configuration](https://code.claude.com/docs/en/model-config#set-the-auto-compact-window)
- `DISABLE_COMPACT` "disables all compaction"; with auto-compaction off, sessions stop at the boundary with a context-limit error. — [Model configuration](https://code.claude.com/docs/en/model-config#correct-the-window-for-a-gateway-or-custom-model-id)
- Idle compaction: "On the one-hour cache, a session past about 200K tokens that sits idle is compacted shortly before the cache expires, so after lunch you continue from a summary." (`idleCompaction` setting.) — [Best practices](https://code.claude.com/docs/en/best-practices)
- Thrashing guard: "If a single file or tool output is so large that context refills immediately after each summary, Claude Code stops auto-compacting after a few attempts and shows an error instead of looping." — [How Claude Code works](https://code.claude.com/docs/en/how-claude-code-works)

**What compaction keeps and loses**
- "It compacts the conversation into a summary. The summary keeps your requests and key code snippets, but detailed instructions from early in the conversation may be lost. Put persistent rules in CLAUDE.md." Control it with a "Compact Instructions" section in CLAUDE.md or `/compact focus on the API changes`. — [How Claude Code works](https://code.claude.com/docs/en/how-claude-code-works)
- Survives: system prompt and output style; project-root CLAUDE.md and unscoped rules ("Re-injected from disk"); auto memory; skill descriptions (v2.1.304+); plan-mode plan; "Files Claude read or edited — Claude Code re-reads up to five, most recently modified first"; invoked skill bodies "capped at 5,000 tokens per skill and 25,000 tokens total; oldest dropped first". "A file over 5,000 tokens comes back as a path reference without its content". — [Explore the context window](https://code.claude.com/docs/en/context-window)
- Lost/summarised: path-scoped rules and nested CLAUDE.md files ("compaction summarizes them away with everything else" until re-triggered), and "Context that hooks added earlier". — [Explore the context window](https://code.claude.com/docs/en/context-window)
- A stated instruction "can get lost when the conversation compacts, so anything that matters belongs in a deny rule." — [Best practices](https://code.claude.com/docs/en/best-practices)
- Illustrative example: 22,225 tokens before `/compact`, 17,095 after, including "a 1,725-token conversation summary" (docs say counts are illustrative). — [Explore the context window](https://code.claude.com/docs/en/context-window)

**Commands**
- `/compact <focus>`: "The summary keeps what you choose instead of what the automatic pass guesses is important." Cost caveat: "On a long session `/compact` is itself a big request, since it reads the whole conversation to summarize it". — [Explore the context window](https://code.claude.com/docs/en/context-window); [Best practices](https://code.claude.com/docs/en/best-practices)
- `/clear`: "run `/clear` when switching to unrelated work. Old conversation crowds out the files you need next and costs tokens on every message." Also: "after two failed corrections run `/clear` and write a better prompt instead of a third." — [Explore the context window](https://code.claude.com/docs/en/context-window); [Best practices](https://code.claude.com/docs/en/best-practices)
- `/rewind` → "Summarize from here" / "Summarize up to here" compacts part of a conversation. — [Explore the context window](https://code.claude.com/docs/en/context-window)
- `/context`: "a live breakdown by category with optimization suggestions, including which CLAUDE.md and auto memory files loaded"; `/context all` confirms CLAUDE.md loaded. `/usage` flags "long context or frequent cache misses once one of them passes 10% of your usage." — [Explore the context window](https://code.claude.com/docs/en/context-window); [Best practices](https://code.claude.com/docs/en/best-practices)

**Quality statements (current wording)**
- Outdated-advice table, verbatim: old advice "The context window is your main constraint, manage it aggressively" → now "Most current models have a 1M-token window and compaction happens on its own, so running out is rare. `/clear` between tasks is still worth doing, because a focused context gets you better answers and costs less." — [Best practices](https://code.claude.com/docs/en/best-practices)
- Old advice "Use subagents to protect your context window" → now "Running out of context is rarely the problem anymore. Subagents are still great for parallel work, keeping noisy output out of your way, and independent review, but each of those costs extra tokens." And: "Handing the same task to a subagent doesn't make the answer any better." — [Best practices](https://code.claude.com/docs/en/best-practices)
- "With a 1M-token window on most current models and compaction near the top of it, you'll rarely run out of context. You can still *pay* for context you don't need". — [Best practices](https://code.claude.com/docs/en/best-practices)
- On repeated corrections: "a third correction mostly adds to a conversation that already contains two failed attempts for Claude to keep re-reading." — [Best practices](https://code.claude.com/docs/en/best-practices)

**Subagents**
- "Subagents work in their own context window. A subagent starts fresh unless it's a fork... the subagent's tool calls stay out of your context, and Claude gets back a summary when the subagent finishes." Example: subagent used 9,890 tokens in its own window and returned a 420-token summary. — [How Claude Code works](https://code.claude.com/docs/en/how-claude-code-works); [Explore the context window](https://code.claude.com/docs/en/context-window)
- Cost: "a fresh subagent first has to gather context your conversation already had, so it can end up *slower*"; "Ask for short summaries back, so twenty results don't fill your context anyway." — [Best practices](https://code.claude.com/docs/en/best-practices)

**CLAUDE.md, memory, skills, MCP**
- "Keep CLAUDE.md short": it loads "into every session, which means every line in it takes a little of Claude's attention on *every* task, whether it's relevant or not." If a line is ignored, "Either the file has grown long enough that the line gets lost, or the line is a rule that can't have exceptions" (→ hook). — [Best practices](https://code.claude.com/docs/en/best-practices)
- Auto memory: "The first 200 lines or 25KB of MEMORY.md, whichever comes first, load at session start". — [How Claude Code works](https://code.claude.com/docs/en/how-claude-code-works)
- Skills load on demand (descriptions only at start); "MCP tool definitions are deferred by default and loaded on demand via tool search, so only tool names and server instructions consume context until Claude uses a specific tool." — [How Claude Code works](https://code.claude.com/docs/en/how-claude-code-works)

### Inferences
- The current docs frame context management mainly as a cost and focus issue, not a capacity issue. The degradation claim survives in softer form ("a focused context gets you better answers"; "every line... takes a little of Claude's attention").
- Earlier Claude Code docs (2025 to early 2026) are widely quoted as saying performance degrades as context fills and that context is the key resource to manage. The current page explicitly labels that framing outdated, so any report citing the older wording should mark it superseded.
- Because default auto-compaction on native-1M models happens at ~967K, a user who wants to stay in a smaller, "fresher" window must opt in with `/autocompact <N>` (minimum 100K) or `CLAUDE_CODE_DISABLE_1M_CONTEXT=1`.

### Gaps
- No documented percentage trigger for auto-compact. Community figures such as "95%" or "~80%" are not in the current docs pages read.
- "Microcompaction" / automatic clearing of old tool outputs inside Claude Code: not described in the pages read. The current "When context fills up" section mentions only summarisation. Older docs reportedly said older tool outputs are cleared first; I could not confirm that wording is still present. Claude Code CHANGELOG on GitHub was not checked.
- No numeric CLAUDE.md size limit on the best-practices page; the memory docs page (code.claude.com/docs/en/memory) was not opened and may state one.
- The exact earlier wording ("performance degrades as context fills") was not retrieved from an archived version.

## 3. What context window sizes do current Claude models have in Claude Code (200K vs 1M; models, plans, pricing), and what has Anthropic said about quality at 1M?

### Takeaway
1M tokens is now the default window for most current models (Fable/Mythos 5.x, Opus 4.7+, Sonnet 5+, Haiku 5.5) at standard pricing; Opus 4.6 and Sonnet 4.6 reach 1M only via a `[1m]` variant with plan conditions. Anthropic claims accuracy holds across the full 1M window while its API docs simultaneously state that accuracy and recall degrade as tokens grow.

### Cited Findings
- 1M-token models (verbatim list): "Claude Fable 5.2, Claude Fable 5.1, and Claude Fable 5"; "Claude Mythos 5.2, Claude Mythos 5.1, Claude Mythos 5, and Claude Mythos Preview"; "Claude Opus 5.5, Claude Opus 5, Claude Opus 4.8, Claude Opus 4.7, and Claude Opus 4.6"; "Claude Sonnet 5.5, Claude Sonnet 5, and Claude Sonnet 4.6"; "Claude Haiku 5.5". "Other Claude models have a 200k-token context window." Max output 128k tokens. — [Context windows (API docs)](https://platform.claude.com/docs/en/build-with-claude/context-windows)
- "For every model with a 1M-token context window, 1M is the default: you don't need a beta header, and long-context requests are billed at standard pricing, except on Claude Haiku 5.5, where prompts over 100,000 tokens cost more." — [Context windows (API docs)](https://platform.claude.com/docs/en/build-with-claude/context-windows)
- In Claude Code: "Fable models, Sonnet 5 and later, Haiku 5.5, and Opus 4.7 and later run with the 1M window by default, with no `[1m]` suffix needed." "Opus 4.6 and Sonnet 4.6 reach 1M only through their `[1m]` variant". — [Model configuration: extended context](https://code.claude.com/docs/en/model-config#extended-context)
- Plans for the 4.6 `[1m]` variants: Max, Ultra, Team, Enterprise: Opus 4.6 1M "Included with subscription", Sonnet 4.6 1M "Requires usage credits"; Pro: both require usage credits; API/pay-as-you-go: full access. — [Model configuration: extended context](https://code.claude.com/docs/en/model-config#extended-context)
- "On the Anthropic API, the 1M context window uses standard model pricing with no premium for tokens beyond 200K, except on Haiku 5.5". Opt out with `CLAUDE_CODE_DISABLE_1M_CONTEXT=1`. — [Model configuration](https://code.claude.com/docs/en/model-config#extended-context)
- Current Claude Code defaults: "the default Opus 5.5 handles most coding well"; `fable` alias → Fable 5.2 (v2.1.321); `sonnet` → Sonnet 5.5; `haiku` → Haiku 5.5. — [Best practices](https://code.claude.com/docs/en/best-practices); [Model configuration: version history](https://code.claude.com/docs/en/model-config)
- History (superseded): at Opus 4.6 launch (Feb 5, 2026), 1M was beta with a surcharge: "Premium pricing applies for prompts exceeding 200k tokens ($10/$37.50 per million input/output tokens), available only on the Claude Platform." — [Claude Opus 4.6 launch](https://www.anthropic.com/news/claude-opus-4-6)
- Mar 13, 2026 GA post removed the surcharge: "Standard pricing applies across the full window — $5/$25 per million tokens for Opus 4.6 and $3/$15 for Sonnet 4.6." "There's no multiplier: a 900K-token request is billed at the same per-token rate as a 9K one." "1M context is now included in Claude Code for Max, Ultra, Team, and Enterprise users with Opus 4.6." — [1M context GA](https://claude.com/blog/1m-context-ga)
- Quality at 1M: "Claude Opus 4.6 and Sonnet 4.6 maintain accuracy across the full 1M window." "Opus 4.6 scores 78.3% on MRCR v2, the highest among frontier models at that context length." Claude Code sessions "can use the full 1M context window automatically, meaning fewer compactions". — [1M context GA](https://claude.com/blog/1m-context-ga)
- Opus 4.6 launch (Feb 5, 2026): context rot described as "where performance degrades as conversations exceed a certain number of tokens"; on the 8-needle 1M variant of MRCR v2, "Opus 4.6 scores 76%, whereas Sonnet 4.5 scores just 18.5%."; the model "holds and tracks information over hundreds of thousands of tokens with less drift." — [Claude Opus 4.6 launch](https://www.anthropic.com/news/claude-opus-4-6)
- Counter-statement in current API docs (verbatim): "more context isn't automatically better. As token count grows, accuracy and recall degrade, a phenomenon known as *context rot*. This makes curating what's in context just as important as how much space is available." — [Context windows (API docs)](https://platform.claude.com/docs/en/build-with-claude/context-windows)
- Compaction overview (verbatim): compaction "keeps the active context small, because response quality degrades as a conversation grows." — [Compaction overview](https://platform.claude.com/docs/en/build-with-claude/compaction)
- Customer quotes in the GA post (third-party testimony hosted by Anthropic): Ramp: "Then compaction kicks in. Details vanish."; Obvious: "We've seen a 15% decrease in compaction events."; Hex: "the agent runs more efficiently — it actually uses fewer tokens overall." — [1M context GA](https://claude.com/blog/1m-context-ga)

### Inferences
- Two MRCR v2 numbers exist for Opus 4.6 at 1M (76% in the Feb launch post, 78.3% in the Mar GA post). They may be different variants or re-runs; cite each with its source rather than merging.
- Even Anthropic's best published 1M retrieval figure for Opus 4.6 (76-78%) is well short of 100%, which is consistent with its own "context rot" statement; "maintain accuracy across the full 1M window" is a marketing-level claim without a per-length curve.

### Gaps
- UNVERIFIED: a search snippet attributes "Sonnet 4.6 scores 68.4% on GraphWalks BFS at 1M tokens" to the GA post; the fetched page text did not surface that number.
- A search snippet lists the Claude Code plans as "Max, Team, and Enterprise" while the fetched page says "Max, Ultra, Team, and Enterprise"; the page appears to have been updated after publication.
- No published Anthropic accuracy-versus-context-length curve for the Claude 5 family (Opus 5/5.5, Sonnet 5/5.5, Fable/Mythos) was retrieved.
- Haiku 5.5 over-100K per-token rates not retrieved (pricing page not opened).

## 4. What API-level features exist (context editing, memory tool, server-side compaction, context awareness) and what performance numbers did Anthropic report?

### Takeaway
The API offers server-side compaction (now the "primary strategy", in two beta forms), context editing (tool-result and thinking clearing, default trigger 100K input tokens), a stable client-side memory tool, and task budgets. Context awareness is limited to Sonnet 4.5 and Haiku 4.5. The only published performance numbers are from Sept 2025: +39% (memory + context editing), +29% (context editing alone), and 84% token reduction on internal evals.

### Cited Findings
**Performance numbers (Sept 29, 2025; old, not updated since as far as found)**
- "On an internal evaluation set for agentic search, we tested how context management improves agent performance": "combining the memory tool with context editing improved performance by 39% over baseline." "Context editing alone delivered a 29% improvement." — [Managing context on the Claude Developer Platform](https://claude.com/blog/context-management)
- "In a 100-turn web search evaluation, context editing enabled agents to complete workflows" that would otherwise fail from context exhaustion, "while reducing token consumption by 84%." — [Managing context on the Claude Developer Platform](https://claude.com/blog/context-management)

**Server-side compaction (beta)**
- Positioning: "For long-running conversations and agentic workflows, server-side compaction is the primary strategy for context management." "It is available in beta for Claude 4.6 and later models and Claude Mythos Preview." — [Context windows](https://platform.claude.com/docs/en/build-with-claude/context-windows)
- Threshold compaction: beta header `compact-2026-01-12`, type `compact_20260112`; "compaction starts at 150,000 input tokens by default"; "`value` must be at least 50,000 tokens"; `pause_after_compaction` (default false); `instructions` "replace it completely" (the default prompt). Supported model list includes Haiku 4.5, Opus 4.6-4.8, Sonnet 4.6, and all 5.x models. — [Compaction at a token threshold](https://platform.claude.com/docs/en/build-with-claude/compaction-threshold)
- A default summarisation prompt (defaults vary by model): "You have written a partial transcript for the initial task above. Please write a summary of the transcript. The purpose of this summary is to provide continuity so you can continue to make progress towards solving the task in a future context, where the raw history above may not be accessible and will be replaced with this summary. Write down anything that would be helpful, including the state, next steps, learnings etc. You must wrap your summary in a <summary></summary> block." — [Compaction at a token threshold](https://platform.claude.com/docs/en/build-with-claude/compaction-threshold)
- Stated loss: thinking blocks from before a compaction block "aren't carried forward, so the summary is all the model has of that earlier work." "Compaction requires an additional sampling step, which contributes to rate limits and billing." — [Compaction at a token threshold](https://platform.claude.com/docs/en/build-with-claude/compaction-threshold)
- Newer on-demand compaction: beta header `compact-2026-09-04`, top-level `compaction` parameter; supports keeping recent turns word for word, background compaction, and custom prompts "when the default summary drops something a later turn needs." Docs say "Use on-demand compaction wherever it is available." — [Compaction overview](https://platform.claude.com/docs/en/build-with-claude/compaction)

**Context editing (beta header `context-management-2025-06-27`)**
- `clear_tool_uses_20250919`: default `trigger` 100,000 input tokens; default `keep` 3 tool uses; `clear_at_least`, `exclude_tools`, `clear_tool_inputs` (default false); cleared results replaced by placeholder text. — [Context editing](https://platform.claude.com/docs/en/build-with-claude/context-editing)
- `clear_thinking_20251015`: default keep `{type: "thinking_turns", value: 1}`. — [Context editing](https://platform.claude.com/docs/en/build-with-claude/context-editing)
- Cache trade-off: tool result clearing "Invalidates cached prompt prefixes when content is cleared." — [Context editing](https://platform.claude.com/docs/en/build-with-claude/context-editing)
- With the memory tool: "When your conversation context approaches the configured clearing threshold, Claude receives an automatic warning to preserve important information". — [Context editing](https://platform.claude.com/docs/en/build-with-claude/context-editing)
- The page "contains no benchmark or performance measurements" (fetch-tool summary of the page, first 100K characters). — [Context editing](https://platform.claude.com/docs/en/build-with-claude/context-editing)

**Memory tool (stable, `memory_20250818`, client-side)**
- "Memory supports just-in-time context retrieval... This keeps the active context focused on the current task, which matters for long-running sessions that would otherwise overwhelm the context window." Available on all Claude 4 and later models; no beta header. — [Memory tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/memory-tool)
- Auto-injected system prompt text includes: "ASSUME INTERRUPTION: Your context window might be reset at any moment, so you risk losing any progress that is not recorded in your memory directory." — [Memory tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/memory-tool)
- "For long-running agents, consider using both: compaction keeps the active context small without client-side bookkeeping, and memory preserves the information that must survive summarization." — [Memory tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/memory-tool)

**Context awareness and task budgets**
- "Only Claude Sonnet 4.5 and Claude Haiku 4.5 have context awareness". The API injects `<budget:token_budget>200000</budget:token_budget>` in the system prompt and, after each tool call, `<system_warning>Token usage: 35000/200000; 165000 remaining</system_warning>`. "No other model receives these tags." Sonnet 4.5 is deprecated on Anthropic-operated platforms (deprecations page anchor dated 2026-09-30). — [Context windows](https://platform.claude.com/docs/en/build-with-claude/context-windows)
- Replacement for newer models: task budgets (beta `task-budgets-2026-03-13`): "The model receives a running countdown and uses it to prioritize work and finish gracefully"; advisory, "a soft hint, not a hard cap"; minimum total 20,000 tokens. "Task budgets are not supported on Claude Code or Cowork surfaces." — [Task budgets](https://platform.claude.com/docs/en/build-with-claude/task-budgets)
- Warning: "A budget that is too small for the task can cause refusal-like behavior... it might decline to attempt the task at all, scope it down aggressively, or stop early with a partial result". — [Task budgets](https://platform.claude.com/docs/en/build-with-claude/task-budgets)
- Overflow: on Claude 4.5 models and newer, hitting the window during generation stops with `stop_reason: "model_context_window_exceeded"`; input alone over the window returns a 400 "prompt is too long". — [Context windows](https://platform.claude.com/docs/en/build-with-claude/context-windows)

### Inferences
- The 39% / 29% / 84% figures are internal, relative, on Sonnet-4.5-era models, with no absolute scores or task definitions published. They show context editing helps versus an unmanaged baseline; they do not measure compaction.
- Context awareness (telling the model its remaining window) was not carried forward to post-4.5 models, which fits the Mar-Apr 2026 posts tying context anxiety to Sonnet 4.5; the budget-countdown idea survives as opt-in task budgets, with a documented side effect of early stopping when the budget looks too small.

### Gaps
- No Anthropic benchmark was found comparing task success with versus without server-side compaction.
- The last ~2K characters of the context-editing page and ~26K of the threshold-compaction page were not read.
- Which API mechanisms Claude Code itself uses internally (server-side compaction vs its own summariser) is not stated in the pages read.

## 5. What do model system cards / launch posts (Sonnet 4.5, Opus 4.5/4.6, Claude 5 family) say about long-context performance, endurance, "context anxiety" or behaviour near the limit?

### Takeaway
Anthropic's own account is that Sonnet 4.5 wrapped up early near its perceived limit ("context anxiety") and that Opus 4.5 largely removed this; Opus 4.6 was launched as holding information "with less drift" at 1M. For the Claude 5 family I found only search-snippet evidence from system cards, not full text.

### Cited Findings
- Sonnet 4.5 (Sept 2025) shipped with "built-in context awareness—tracking available tokens throughout conversations." — [Managing context on the Claude Developer Platform](https://claude.com/blog/context-management)
- Sonnet 4.5 "exhibited context anxiety strongly enough that compaction alone wasn't sufficient"; "Opus 4.5 largely removed that behavior on its own". — [Harness design](https://www.anthropic.com/engineering/harness-design-long-running-apps)
- Opus 4.6 (Feb 5, 2026): "plans more carefully, sustains agentic tasks for longer, can operate more reliably in larger codebases"; "Opus 4.6 is our first Opus-class model with 1M token context."; MRCR v2 8-needle 1M: 76% vs Sonnet 4.5 18.5%. — [Claude Opus 4.6 launch](https://www.anthropic.com/news/claude-opus-4-6)
- Current prompting guide (covers Fable 5.2 through Haiku 4.5): "Claude's latest models handle long-horizon reasoning tasks with strong state tracking... This capability especially emerges over multiple context windows or task iterations, where Claude can work on a complex task, save the state, and continue with a fresh context window." — [Prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)
- Same guide, behaviour near the limit: in a harness that compacts (as in Claude Code), tell the model so; "Otherwise, Claude might sometimes naturally attempt to bring its work to a close as it approaches the context limit." Suggested prompt begins "Your context window will be automatically compacted as it approaches its limit..." and tells the model not to stop "tasks early due to token budget concerns" and to "save your current progress and state to memory before the context window refreshes." — [Prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)
- Same guide, fresh start vs compaction: "When a context window is cleared, consider starting with a brand new context window rather than using compaction. Claude's latest models are extremely effective at discovering state from the local filesystem. In some cases, you might want to take advantage of this over compaction." Recommends `tests.json`, `progress.txt`, `init.sh`, and git for state. — [Prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)
- UNVERIFIED (search snippet): the Claude Opus 5 System Card (dated July 24, 2026) describes a long-context evaluation where, in the tool-enabled setup, compaction was "triggered at 200k tokens" to go past the 1M window, with scores rising across episodes (Opus 5 83% → 93% by the fifth; Opus 4.8 80% → 90%; Mythos 5 84% → 93%). — [Claude Opus 5 System Card](https://www.anthropic.com/claude-opus-5-system-card)
- UNVERIFIED (search snippet): the Claude Opus 5.5 System Card notes that context compaction can further change how models act, and that Anthropic simulated compaction but "the space of possible ways of implementing any given scenario in a long transcript is far too large for us to explore thoroughly." — [Claude Opus 5.5 System Card (PDF)](https://www-cdn.anthropic.com/fc1b44717c85dc068bc6ba5024219938094694bd/Claude%20Opus%205.5%20System%20Card.pdf)

### Inferences
- The prompting guide still warns that Claude "might sometimes naturally attempt to bring its work to a close as it approaches the context limit", so Anthropic does not claim the early-wrap-up tendency is fully gone in current models, only that it was strong in Sonnet 4.5 and largely absent in Opus 4.5 in one harness.
- Anthropic's own recommendation to prefer a fresh window plus filesystem state over compaction "in some cases" is an implicit acknowledgment that compaction summaries are lossy relative to re-deriving state.

### Gaps
- System cards for Sonnet 4.5, Opus 4.5, Opus 4.6, Opus 5, Opus 5.5 and Fable/Mythos 5.x were not opened in full; the two Opus 5/5.5 items above are snippet-level and must be checked before quoting.
- A launch post for Claude Fable 5 / Mythos 5 (reported as June 9, 2026, anthropic.com/news/claude-fable-5-mythos-5) was surfaced only as a citation in a search snippet; its content on long context is unknown.
- I found no system-card use of the phrase "context anxiety"; in what I read, the term appears only in the Mar and Apr 2026 engineering posts.

## 6. Any published Anthropic data on how compaction affects task success?

### Takeaway
I found no Anthropic-published controlled measurement of task success with versus without compaction. What exists is qualitative: compaction "isn't sufficient", early instructions "may be lost", irreversible discard "can lead to failures", plus one customer-reported 15% drop in compaction events at 1M.

### Cited Findings
- "However, compaction isn't sufficient." (Nov 26, 2025) — [Effective harnesses](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)
- "While compaction preserves continuity, it doesn't give the agent a clean slate." (Mar 24, 2026) — [Harness design](https://www.anthropic.com/engineering/harness-design-long-running-apps)
- "irreversible decisions to selectively retain or discard context can lead to failures." (Apr 8, 2026) — [Managed Agents](https://www.anthropic.com/engineering/managed-agents)
- Claude Code docs: "detailed instructions from early in the conversation may be lost." — [How Claude Code works](https://code.claude.com/docs/en/how-claude-code-works)
- API docs: after compaction "the summary is all the model has of that earlier work." — [Compaction at a token threshold](https://platform.claude.com/docs/en/build-with-claude/compaction-threshold)
- Customer testimony hosted by Anthropic (Mar 13, 2026): Ramp, "Then compaction kicks in. Details vanish."; Obvious, "We've seen a 15% decrease in compaction events." — [1M context GA](https://claude.com/blog/1m-context-ga)
- Nearest quantitative data is for context editing, not compaction: +29% (context editing), +39% (with memory tool), 84% fewer tokens (Sept 29, 2025, internal evals). — [Managing context on the Claude Developer Platform](https://claude.com/blog/context-management)
- UNVERIFIED (search snippet): Opus 5 System Card long-context eval used compaction triggered at 200k tokens in the tool-enabled setup. — [Claude Opus 5 System Card](https://www.anthropic.com/claude-opus-5-system-card)

### Inferences
- Anthropic's public guidance treats compaction as necessary plumbing with known information loss, mitigated by external state (CLAUDE.md, memory files, progress logs, git) rather than by a claim that summaries are lossless.
- The 15% figure measures how often compaction happens, not what compaction does to outcomes.

### Gaps
- No Anthropic number for task-success change per compaction event, for summary fidelity, or for quality as a function of the number of compactions in a session.
- Not checked: Claude Code CHANGELOG on GitHub, Anthropic postmortems or status posts about compaction bugs, and full system-card PDFs, any of which could contain relevant data.
