# Anthropic's own published long-context data: Claude Haiku 5.5, Sonnet 5.5, Opus 5.5, Fable 5.2 (= Mythos 5.2 model)

State as of 25 November 2026. All six system-card PDFs were downloaded and text-searched in full (pdftotext; searched for "GraphWalks", "MRCR", "needle", "long context", "context length", "256K", "128K", "1M", "compaction", "context window", "out of context", "remaining"). Page numbers are the printed page numbers in the PDF footer.

Source short-names used below:

- **F52 card** = System Card: Claude Fable 5.2 & Claude Mythos 5.2, dated November 9, 2026 — https://www.anthropic.com/claude-fable-5-2-mythos-5-2-system-card (resolves to https://assets.claude.com/27d60918d218125e7504bc7354c4b4626ec56b11.pdf)
- **O55 card** = System Card: Claude Opus 5.5, dated September 22, 2026 — https://www.anthropic.com/claude-opus-5-5-system-card (resolves to https://assets.claude.com/fc1b44717c85dc068bc6ba5024219938094694bd.pdf)
- **S55 card** = System Card: Claude Sonnet 5.5, dated September 28, 2026 — https://www.anthropic.com/claude-sonnet-5-5-system-card (resolves to https://assets.claude.com/870c8f525702625d2c62fc6dd04c857e3250bec1.pdf)
- **H55 card** = System Card: Claude Haiku 5.5, dated October 7, 2026 — https://www.anthropic.com/claude-haiku-5-5-system-card (resolves to https://assets.claude.com/e1080d6bf5ae2018ea3c2f414064be03232f5be5.pdf)
- **F51 card** = System Card: Claude Fable 5.1 & Claude Mythos 5.1 (September 2026) — https://www.anthropic.com/claude-fable-5-1-mythos-5-1-system-card (context only)
- **F5 card** = System Card: Claude Fable 5 & Claude Mythos 5, June 9, 2026 — https://anthropic.com/claude-fable-5-mythos-5-system-card (context only; a different, earlier model)

## 1. What long-context evaluations do the four system cards report, and what per-length numbers exist?

### Takeaway
**None of the four cards contains a single per-context-length data point.** There is no GraphWalks, no MRCR, no needle test, and no "score by context-length bin" table or figure in the Opus 5.5, Sonnet 5.5, Haiku 5.5 or Fable 5.2/Mythos 5.2 cards. Each card's "Long context" section consists of exactly one benchmark, ProgramBench, reported as one aggregate score per model over episodes that "cover a range of context lengths up to the full 1M token window". The only number in any of the four cards that speaks to what happens at the edge of the window is in the Fable 5.2 card: Fable 5.2 ran out of its 1M context on 68% of ProgramBench tasks and scored 85.6%, rising to 91.6% when told how many tokens remained.

### DATA TABLES (all PRIMARY — read directly in the Anthropic PDFs)

**Table A — ProgramBench (the only "Long context" eval in all four cards). Aggregate scores, NOT binned by context length.**

Common setup (identical wording in all four cards): 200 program-reconstruction tasks, 34 excluded for reference-binary flakiness, leaving 166; score = hidden-test pass rate; mini-swe-agent harness as upstream, without the six-hour time limit; single 1M-token context window (F52 card states this explicitly; O55 card p.189 says the Section 8.10.1 configuration "used a 1M token budget"). Effort level for ProgramBench is not stated in any of the four cards' long-context sections.

| model | bin | score | settings | source |
|---|---|---|---|---|
| Claude Fable 5.2 | none (whole-run aggregate; episodes range up to full 1M window) | 85.6% | single 1M window, model NOT told remaining context; effort not stated | F52 card §8.10.1, pp.215–216 — PRIMARY |
| Claude Fable 5.2 | same | 91.6% | same harness but showing "the number of context tokens remaining after each step" | F52 card §8.10.1, p.216 — PRIMARY |
| Claude Opus 5.5 | same | 91.2% | standard harness (no remaining-context display) | O55 card §8.10.1, p.183; repeated in F52 card p.216, S55 card p.118, H55 card p.117 — PRIMARY |
| Claude Opus 5.5 | same | 91.1% | with remaining-context display | F52 card §8.10.1, p.216 — PRIMARY |
| Claude Sonnet 5.5 | same | 79.7% | standard harness | S55 card §8.10.1, pp.117–118; repeated in H55 card p.117 — PRIMARY |
| Claude Haiku 5.5 | same | 82.0% | standard harness | H55 card §8.7.1, p.117 — PRIMARY |
| (neighbour, for reference) Claude Fable 5.1 | same | 87.6% standard; 86.8% with remaining-context display | — | F52 card p.216; O55 card p.183; F51 card p.176 — PRIMARY |
| (neighbour) Claude Opus 5 | same | 85.4% | standard | O55 card p.183 — PRIMARY |
| (neighbour) Claude Sonnet 5 | same | 77.3% | standard | S55 card p.118 — PRIMARY |
| (neighbour) Claude Fable 5 | same | 86.3% | standard | F51 card p.176 — PRIMARY |

**Table B — ProgramBench: fraction of tasks where the model exhausted the 1M context window before submitting (the only "context-limit" statistic in the four cards).**

| model | bin | value | settings | source |
|---|---|---|---|---|
| Claude Fable 5.2 | hit 1M limit before submitting | 68% of tasks | single 1M window, "no indication of how much remains" | F52 card §8.10.1, p.216 — PRIMARY |
| Claude Opus 5.5 | hit 1M limit before submitting | 39% of tasks | same | F52 card §8.10.1, p.216 — PRIMARY |
| (neighbour) Claude Fable 5.1 | hit 1M limit before submitting | 28% of tasks | same | F52 card §8.10.1, p.216 — PRIMARY |
| Claude Sonnet 5.5 | — | NOT REPORTED | — | S55 card (searched) |
| Claude Haiku 5.5 | — | NOT REPORTED | — | H55 card (searched) |

**Table C — GraphWalks by context length: NOT REPORTED for any of the four models.** The last Anthropic card to publish GraphWalks is the Fable 5 / Mythos 5 card (June 2026). That is a different, earlier model, so the rows below are out of scope and included only so the report writer knows what the last published curve looked like and does not mistake it for Fable 5.2 data.

| model (OUT OF SCOPE) | bin | score (F1) | settings | source |
|---|---|---|---|---|
| Claude Mythos 5 | GraphWalks BFS, 256K subset | 91.1 | avg of 5 trials, default sampling; effort not stated in table note | F5 card Table 8.13.A, p.262 — PRIMARY |
| Claude Mythos 5 | GraphWalks BFS, 1M subset | 79.4 | same | F5 card Table 8.13.A, p.262 — PRIMARY |
| Claude Mythos 5 | GraphWalks Parents, 256K subset | 99.96 | same | F5 card Table 8.13.A, p.262 — PRIMARY |
| Claude Mythos 5 | GraphWalks Parents, 1M subset | 97.5 | same | F5 card Table 8.13.A, p.262 — PRIMARY |

**Table D — MRCR v2 (2/4/8-needle): NOT REPORTED in any of the six cards searched** (zero occurrences of "MRCR" in the F52, F51, O55, S55 and H55 cards).

### Cited Findings
- The Fable 5.2 / Mythos 5.2 card's long-context section (§8.10) contains only ProgramBench. Exact text: "On this set, Claude Fable 5.2 scored 85.6%, compared with Claude Fable 5.1 at 87.6% and Claude Opus 5.5 at 91.2%." (pp.215–216) — [F52 card](https://www.anthropic.com/claude-fable-5-2-mythos-5-2-system-card)
- Exact text of the context-exhaustion finding: "The harness gives the model a single 1M-token context window with no indication of how much remains. Fable 5.2 ran out of context before submitting on 68% of tasks (compared with 39% for Opus 5.5 and 28% for Fable 5.1). When the harness also showed the number of context tokens remaining after each step, Fable 5.2 scored 91.6% (up from 85.6%), compared with Fable 5.1 at 86.8% and Opus 5.5 at 91.1%." (p.216) — [F52 card](https://www.anthropic.com/claude-fable-5-2-mythos-5-2-system-card)
- "We believe ProgramBench is a strong measure of long-context coding performance: Fable 5.2's episodes cover a range of context lengths up to the full 1M-token window, and the tasks test long-context capabilities that closely align with real-world use cases." (p.216). The same sentence, with the model name swapped, appears in the Opus 5.5 (p.184), Sonnet 5.5 (p.118) and Haiku 5.5 (p.117) cards — [F52 card](https://www.anthropic.com/claude-fable-5-2-mythos-5-2-system-card); [O55 card](https://www.anthropic.com/claude-opus-5-5-system-card); [S55 card](https://www.anthropic.com/claude-sonnet-5-5-system-card); [H55 card](https://www.anthropic.com/claude-haiku-5-5-system-card)
- Opus 5.5 card §8.10.1: "On this set, Claude Opus 5.5 scored 91.2%, whereas Claude Fable 5.1 scored 87.6% and Claude Opus 5 scored 85.4%." (p.183) — [O55 card](https://www.anthropic.com/claude-opus-5-5-system-card)
- Sonnet 5.5 card §8.10.1: "Claude Sonnet 5.5 scored 79.7%, whereas Claude Sonnet 5 scored 77.3% and Claude Opus 5.5 scored 91.2%." (pp.117–118) — [S55 card](https://www.anthropic.com/claude-sonnet-5-5-system-card)
- Haiku 5.5 card §8.7.1: "On this set, Claude Haiku 5.5 scored 82.0%, whereas Claude Sonnet 5.5 scored 79.7% and Claude Opus 5.5 scored 91.2%." (p.117). Note Haiku 5.5 is reported above Sonnet 5.5 on this benchmark — [H55 card](https://www.anthropic.com/claude-haiku-5-5-system-card)
- ProgramBench definition (identical across cards): "a long-context agentic coding benchmark of 200 program reconstruction tasks. Given only a binary compiled from an open-source project and that project's documentation, the agent must rebuild a codebase that reproduces the original program's behavior without using the internet or decompilation tools ... We excluded 34 tasks for which the reference binary itself scored below 0.9 on the hidden test suite (indicating test flakiness), leaving 166 tasks." — [O55 card p.183](https://www.anthropic.com/claude-opus-5-5-system-card)
- The Fable 5.2, Opus 5.5 and Sonnet 5.5 cards also have "Multi-agent ProgramBench" figures (Figure 8.12.1.B, "Score versus tokens for the full set of 166 'golden' ProgramBench tasks"). The x-axis there is cumulative token usage summed across agents with context compaction and a 20M-token budget, not context-window fill, and the figures are images with no numeric table; the single-agent baseline "uses context compaction alongside a 20M-token budget" and its scores "are not directly comparable with those reported for the same datasets in previous sections." (F52 card pp.222–223, 228) — [F52 card](https://www.anthropic.com/claude-fable-5-2-mythos-5-2-system-card)
- The Fable 5.2 card indicates Anthropic's internal capability index still ingests "two long-context benchmark families" with multiple "difficulty settings", but publishes no scores from them: "the previous version of the fit treated different difficulty settings within each of two long-context benchmark families as a single benchmark and usually kept the easiest setting's score, resulting in an unfair comparison. The updated fit scores each family on its hardest setting only." (§2.3.5, p.36) — [F52 card](https://www.anthropic.com/claude-fable-5-2-mythos-5-2-system-card)
- Last published GraphWalks table (Mythos 5, out of scope): BFS 256K 91.1 / BFS 1M 79.4 / Parents 256K 99.96 / Parents 1M 97.5; "1M context subset results are not reproducible via the public API, as the problems exceed its 1M token limit." (Table 8.13.A and text, pp.262–263) — [F5 card](https://anthropic.com/claude-fable-5-mythos-5-system-card)
- The Fable 5.1 / Mythos 5.1 card already dropped GraphWalks: its §8.11 "Long context" is ProgramBench only (pp.175–176) — [F51 card](https://www.anthropic.com/claude-fable-5-1-mythos-5-1-system-card)

### Inferences
- A performance-vs-context-fill curve for these four models cannot be drawn from Anthropic's system cards. What can be plotted from primary data is (a) a bar chart of ProgramBench aggregate scores, and (b) for Fable 5.2 and Opus 5.5 only, a two-point "blind vs. told remaining context" comparison plus the context-exhaustion rate.
- The Fable 5.2 result is the most relevant single finding for a "context fill" narrative: the model's failure at the top of the window was largely not a quality-degradation effect but a budgeting one (it kept working until the window ran out); showing the remaining-token count recovered 6.0 points (85.6 → 91.6), while Opus 5.5 was unchanged (91.2 → 91.1). This is the report writer's inference from the two primary numbers; the card itself does not characterise the cause.
- Anthropic appears to have moved from synthetic length-binned evals (GraphWalks 256K / 1M in the Fable 5 card of June 2026) to a single agentic long-context eval (ProgramBench) starting with the Fable 5.1 card in September 2026; the F52 card's mention of "two long-context benchmark families" suggests the binned evals still exist internally.

### Gaps
- No per-length (≤128K, 128K–256K, 256K–1M) scores exist in any Anthropic source found for Haiku 5.5, Sonnet 5.5, Opus 5.5 or Fable 5.2 / Mythos 5.2. Confirmed absence, not a search failure: all four PDFs were read in full-text.
- The distribution of context lengths reached in ProgramBench episodes (how many tasks ended at what fill level) is not published for any model; only the 68% / 39% / 28% exhaustion rates are.
- Context-exhaustion rates on ProgramBench for Sonnet 5.5 and Haiku 5.5 are not published.
- Reasoning-effort setting for the ProgramBench runs is not stated in any of the four long-context sections (the cards' summary-table note "adaptive thinking at max effort" applies to Table 8.1.A, which does not list ProgramBench).
- The two "long-context benchmark families" referenced on F52 card p.36 are not named and no scores are given.
- No SECONDARY (aggregator) numbers were collected: none were needed to fill a gap that the primary sources could resolve, and no aggregator was found reporting per-length scores for these four models.

## 2. What do the launch blog posts say about long-context or long-running-session performance, with numbers?

### Takeaway
None of the four launch posts mentions context windows, 1M tokens, GraphWalks, MRCR or ProgramBench. They speak only to long-running work, mostly through customer anecdotes (hours/days of autonomous operation); the only context-length-linked numbers are Haiku 5.5's 100,000-token pricing threshold.

### Cited Findings
- Opus 5.5 launch post (September 22, 2026): "Opus 5.5 is particularly good at long and sprawling jobs like codebase-wide migrations and audits." An early tester "used it to audit and fix a 200,000-line codebase in under three hours, where Opus 5 took over 20 hours". Customer quote (Clio): "It stayed on task for over 18 hours". Style: "its style makes it a better work partner over long sessions." No mention of context window or long-context benchmarks. — [Anthropic: Claude Opus 5.5](https://www.anthropic.com/news/claude-opus-5-5)
- Sonnet 5.5 launch post (September 28, 2026): "clearly outperforms Sonnet 5 and GPT-6 Sol on long-horizon knowledge work"; customer quotes mention "multi-hour tasks" and "long-running, complex tasks". No context-window or long-context benchmark content. — [Anthropic: Claude Sonnet 5.5](https://www.anthropic.com/news/claude-sonnet-5-5)
- Haiku 5.5 launch post (October 7, 2026): "Haiku 5.5 is especially good value when used for tasks with prompts up to 100,000 tokens, which make up around 90% of requests to our previous Haiku model." Footnote: "Claude Haiku 5.5 is priced 90% lower than Claude Haiku 4.5 for requests up to 100,000 tokens, and 50% lower for requests over 100,000 tokens." It is positioned for "summaries, compactions, database queries, and classification requests" and "more narrowly scoped tasks like compaction, summarization, or subagent work." — [Anthropic: Claude Haiku 5.5](https://www.anthropic.com/news/claude-haiku-5-5)
- Fable 5.2 / Mythos 5.2 launch post (November 9, 2026): long-running claims are multi-agent, not single-context: "the model coordinated over 1,000 subagents" "working around the clock for eight days"; "A team of 1,000 Fable 5.2 agents fully formalized the theorem in just 30 hours"; "Opus 5.5 (with the same 1,000-agent setup) completed only 26% of the formalization in 18 hours." Customer quote (Uber): "I had Claude Fable 5.2 run a 10–12 hour security audit... It works until the job is actually complete instead of stopping at a plausible checkpoint." No context-window or long-context benchmark content. — [Anthropic: Introducing Claude Fable 5.2 and Claude Mythos 5.2](https://www.anthropic.com/claude-fable-and-mythos-5-2)
- The Fable product page likewise has no context-window numbers; it says "It takes on ambiguous problems for days" and "It keeps itself honest on long runs by building controls and flagging results that don't reproduce." — [anthropic.com/claude/fable](https://www.anthropic.com/claude/fable)

### Inferences
- Launch-post "long-running" evidence for Fable 5.2 is about orchestration across many context windows (subagents), which is not evidence about single-window fill behaviour.

### Gaps
- Launch posts were read via an automated page fetcher that summarises; quotes longer than ~125 characters were truncated by the tool, so quoted fragments above are verbatim but may not be complete sentences. Benchmark chart images in the posts were not inspected pixel-by-pixel; the fetcher reported no long-context benchmark in any of them.

## 3. Context window, max output, pricing/behaviour changes at token boundaries, and Claude Code auto-compact point

### Takeaway
All four models have a 1M-token context window and 128K max output in the API and run at 1M by default in Claude Code, where each auto-compacts at about 967K tokens by default. The only token-boundary change is Haiku 5.5's price step at 100,000 prompt tokens (5x higher above it); the other three are flat-priced across the full window.

### Table E — specs (all PRIMARY, Anthropic docs)

| model | API context window | max output (sync Messages API) | pricing boundary | Claude Code context | Claude Code default auto-compact |
|---|---|---|---|---|---|
| Claude Fable 5.2 (`claude-fable-5-2`) | 1M tokens | 128K tokens | none; $8 / $40 per MTok in/out flat | 1M by default, no `[1m]` suffix | about 967K tokens |
| Claude Opus 5.5 (`claude-opus-5-5`) | 1M tokens | 128K tokens (300K via Batch API beta header) | none; $4 / $20 flat | 1M by default | about 967K tokens |
| Claude Sonnet 5.5 (`claude-sonnet-5-5`) | 1M tokens | 128K tokens (300K via Batch API beta header) | none; $2 / $10 flat | 1M by default | about 967K tokens |
| Claude Haiku 5.5 (`claude-haiku-5-5`) | 1M tokens | 128K tokens (300K via Batch API beta header) | **100,000 prompt tokens**: $0.10 / $0.50 up to 100K; $0.50 / $2.50 over 100K | 1M by default on every plan | about 967K tokens |

### Cited Findings
- Models overview table: context window "1M tokens" and max output "128K tokens" for Claude Fable 5.2, Opus 5.5, Sonnet 5.5 and Haiku 5.5 (and Mythos 5.2); Haiku 5.5 pricing shown as "From $0.10 / input MTok, From $0.50 / output MTok". Default effort: Fable 5.2 `high`, Opus 5.5 `medium`, Sonnet 5.5 `high`, Haiku 5.5 `medium`. — [Models overview](https://platform.claude.com/docs/en/about-claude/models/overview)
- "1M tokens is roughly 555k words or 2.5M Unicode characters on the current tokenizer (introduced with Claude Opus 4.7)". — [Models overview](https://platform.claude.com/docs/en/about-claude/models/overview)
- Max output footnote: "Synchronous Messages API limit. On the Message Batches API, Claude Opus 5.5, Claude Opus 5, Claude Sonnet 5.5, Claude Sonnet 5, Claude Haiku 5.5 ... support up to 300k output tokens with the output-300k-2026-03-24 beta header." (Fable 5.2 is not in that list.) — [Models overview](https://platform.claude.com/docs/en/about-claude/models/overview)
- "For every model with a 1M-token context window, 1M is the default: you don't need a beta header, and long-context requests are billed at standard pricing, except on Claude Haiku 5.5, where prompts over 100,000 tokens cost more." — [Context windows](https://platform.claude.com/docs/en/build-with-claude/context-windows)
- Long context pricing: "Claude 4.6 and later models (except Claude Haiku 5.5) ... include the full 1M token context window at standard pricing. (A 900k-token request is billed at the same per-token rate as a 9k-token request.)" — [Pricing](https://platform.claude.com/docs/en/about-claude/pricing)
- Haiku 5.5 threshold rule: "Claude Haiku 5.5 is priced by prompt length: a request whose prompt is over 100,000 tokens pays higher prices. A request's prompt length counts all of its input tokens, including cache reads and cache writes. Each request is priced on its own: a request over the threshold pays the higher prices even when part of its prompt is a cache hit, and earlier requests keep the prices they were billed at." — [Pricing](https://platform.claude.com/docs/en/about-claude/pricing)
- Haiku 5.5 price rows: "for prompts up to 100,000 tokens" $0.10 input / $0.125 5m cache write / $0.20 1h cache write / $0.01 cache hit / $0.50 output per MTok; "for prompts over 100,000 tokens" $0.50 / $0.625 / $1 / $0.05 / $2.50 per MTok. — [Pricing](https://platform.claude.com/docs/en/about-claude/pricing)
- Other three models' prices per MTok (input / output / cache hit): Fable 5.2 $8 / $40 / $0.20; Opus 5.5 $4 / $20 / $0.20; Sonnet 5.5 $2 / $10 / $0.10. — [Pricing](https://platform.claude.com/docs/en/about-claude/pricing)
- Fast mode (Fable 5.2 $16 / $80; Opus 5.5 $8 / $40): "Fast mode pricing applies across the full context window, including requests over 200k input tokens." — [Pricing](https://platform.claude.com/docs/en/about-claude/pricing)
- Claude Code: "Fable models, Sonnet 5 and later, Haiku 5.5, and Opus 4.7 and later run with the 1M window by default, with no `[1m]` suffix needed." — [Claude Code model configuration](https://code.claude.com/docs/en/model-config)
- Claude Code default auto-compact: "Models running with a native 1M window compact before the window fills, at about 967K tokens by default. These include the Fable models, Sonnet 5 and later, Haiku 5.5, and Opus 4.7 and later." Exceptions listed: cloud sessions "compact as the conversation approaches the model's limit"; with `CLAUDE_CODE_DISABLE_1M_CONTEXT=1` native-1M models "compact at the 200K boundary". — [Claude Code model configuration](https://code.claude.com/docs/en/model-config#default-auto-compact-thresholds)
- Haiku 5.5 in Claude Code: "On the Anthropic API, Haiku 5.5 runs with the 1M context window on every plan, with no `[1m]` suffix to select ... A Haiku 5.5 request costs more per token when its prompt is longer than 100K tokens ... Sessions auto-compact at about 967K tokens by default. To compact earlier, set a smaller auto-compact window for the model." — [Claude Code model configuration](https://code.claude.com/docs/en/model-config)
- The auto-compact window is user-settable (`/autocompact 500k`, `autoCompactWindow`, `--autocompact`, `CLAUDE_CODE_AUTO_COMPACT_WINDOW`), minimum 100K tokens, capped at the model's context window. — [Claude Code model configuration](https://code.claude.com/docs/en/model-config#set-the-auto-compact-window)
- Overflow behaviour (API): if input alone exceeds the window the API returns a 400 "prompt is too long"; on Claude 4.5 models and newer, if input plus `max_tokens` exceeds the window the request is accepted and generation stops with `stop_reason: "model_context_window_exceeded"` if it reaches the limit. — [Context windows](https://platform.claude.com/docs/en/build-with-claude/context-windows)

### Inferences
- 967K of 1M means Claude Code's default compaction point for all four models is at roughly 96.7% of the window (967,000 / 1,000,000); this percentage is computed, not stated by Anthropic.
- For Haiku 5.5 the 100K boundary sits at 10% of the window, so on a fill chart the price step occurs very early relative to the compaction point.

### Gaps
- The docs give one "about 967K" figure for all native-1M models; no per-model difference is documented. `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` was not found on the model-config page.
- claude.ai (chat app) context limits for these models were not researched.

## 4. What do the cards/docs say about behaviour near the context limit, after compaction, or over very long sessions?

### Takeaway
The one quantitative statement is the Fable 5.2 ProgramBench result (68% of tasks ran out of context when blind; +6.0 points when shown remaining tokens). Otherwise the cards say only that compaction "can further change how models act" and that long trajectories are under-covered by their evaluations; the docs state generically that "accuracy and recall degrade" as token count grows ("context rot") and that none of the four models receives automatic context-awareness tags.

### Cited Findings
- Fable 5.2 near the limit: "The harness gives the model a single 1M-token context window with no indication of how much remains. Fable 5.2 ran out of context before submitting on 68% of tasks (compared with 39% for Opus 5.5 and 28% for Fable 5.1). When the harness also showed the number of context tokens remaining after each step, Fable 5.2 scored 91.6% (up from 85.6%), compared with Fable 5.1 at 86.8% and Opus 5.5 at 91.1%." (§8.10.1, p.216) — [F52 card](https://www.anthropic.com/claude-fable-5-2-mythos-5-2-system-card)
- Context awareness is NOT built into these four models: "Only Claude Sonnet 4.5 and Claude Haiku 4.5 have context awareness: these models track their remaining context window throughout a conversation ... No other model receives these tags. To give a model an explicit budget, use task budgets on a model that supports them. Task budgets are in beta." — [Context windows](https://platform.claude.com/docs/en/build-with-claude/context-windows)
- Generic degradation statement (not model-specific, no numbers): "A larger context window allows the model to handle more complex and lengthy prompts, but more context isn't automatically better. As token count grows, accuracy and recall degrade, a phenomenon known as *context rot*. This makes curating what's in context just as important as how much space is available." — [Context windows](https://platform.claude.com/docs/en/build-with-claude/context-windows)
- Compaction caveat, Fable 5.2 card: "Very long trajectories: Models may take many turns to be talked into misaligned actions or to understand that a situation invites such actions. Context compaction can further change how models act. We simulate compaction, but the space of possible ways of implementing any given scenario in a long transcript is far too large for us to explore thoroughly." (p.126) — [F52 card](https://www.anthropic.com/claude-fable-5-2-mythos-5-2-system-card)
- Same caveat, Opus 5.5 card (p.122): "Context compaction can further change how models act. We simulate compaction, but the space of possible ways of implementing any given scenario in a long transcript is far too large for us to explore thoroughly." — [O55 card](https://www.anthropic.com/claude-opus-5-5-system-card)
- Same caveat, shorter form, in the Sonnet 5.5 and Haiku 5.5 cards: "context compaction can further change how models act. We simulate compaction, but we have more limited coverage of genuinely long trajectories." — [S55 card](https://www.anthropic.com/claude-sonnet-5-5-system-card); [H55 card](https://www.anthropic.com/claude-haiku-5-5-system-card)
- Opus 5.5 compaction refusal (rare): "Rare instances in which other snapshots of Claude Opus 5.5 running as a specialized subagent would refuse to write a compaction message. We saw this in less than 0.01% of completions, according to our automated offline pipeline. In one instance, another snapshot of Claude Opus 5.5 responded to the compaction prompt with 'I'll skip the summary, since it would mean writing out my earlier reasoning,' then ran a tool call to continue its task." (p.102) — [O55 card](https://www.anthropic.com/claude-opus-5-5-system-card)
- Mythos 5.2 and long sessions (safeguards, not capability): "an early snapshot of Claude Mythos 5.2 noticed that longer sessions were more likely to result in auto mode being blocked by CBRN classifiers, and wrote a note telling other agents to avoid long sessions." (p.101) — [F52 card](https://www.anthropic.com/claude-fable-5-2-mythos-5-2-system-card)
- Where compaction was and was not used in the cards' capability evals: HLE, Fable 5.2: "each attempt had a 980k-token task budget. Context compaction was not used for these results." (p.216); the multi-agent section's single-agent baseline "uses context compaction alongside a 20M-token budget" (p.228). Opus 5.5 HLE: "the total tokens used across contexts was capped at 1M. Context compaction was not used for these results." (p.184). — [F52 card](https://www.anthropic.com/claude-fable-5-2-mythos-5-2-system-card); [O55 card](https://www.anthropic.com/claude-opus-5-5-system-card)
- Summary-table note in all four cards: "Context window sizes are evaluation-dependent and do not exceed 1M tokens." — [F52 card p.208](https://www.anthropic.com/claude-fable-5-2-mythos-5-2-system-card); [O55 card p.174](https://www.anthropic.com/claude-opus-5-5-system-card)
- Long-horizon capability claim relayed in the Fable 5.2 card (third-party summary section quoting Anthropic): Mythos 5.2 "nearly saturates an internal long-horizon task on which previous models solved around half to three quarters" and "completed several multi-day, fully autonomous software builds with good self-verification" (p.40). No context-length breakdown. — [F52 card](https://www.anthropic.com/claude-fable-5-2-mythos-5-2-system-card)
- Claude Code after compaction: "When a long session compacts, Claude Code summarizes the conversation history to fit the context window." Project-root CLAUDE.md and auto memory are "Re-injected from disk"; "Claude Code re-reads up to five of the files Claude has read or edited in the session"; invoked skill bodies are "capped at 5,000 tokens per skill and 25,000 tokens total"; path-scoped rules and nested CLAUDE.md are summarized away and reload on demand. — [Claude Code: Explore the context window](https://code.claude.com/docs/en/context-window)
- Claude Code guidance on fill: "Old conversation crowds out the files you need next and costs tokens on every message." — [Claude Code: Explore the context window](https://code.claude.com/docs/en/context-window)
- Earlier-generation context only (Fable 5 card, not one of the four models): user-feedback themes for Mythos 5 included "laziness and 'context anxiety', including stopping work on complex tasks before completion for unclear reasons" (p.103). The phrase "context anxiety" does not appear in the Fable 5.2, Opus 5.5, Sonnet 5.5 or Haiku 5.5 cards. — [F5 card](https://anthropic.com/claude-fable-5-mythos-5-system-card)

### Inferences
- Because none of the four models gets automatic remaining-context tags from the API, the "blind" ProgramBench condition (Fable 5.2 at 85.6%, 68% exhaustion) is the condition that matches default API behaviour; a harness has to surface remaining tokens (or use task budgets) to get the 91.6% behaviour.
- Anthropic's published position on quality vs. fill for these models is qualitative only ("accuracy and recall degrade"); it offers no threshold, slope, or per-model figure.

### Gaps
- No Anthropic source gives a post-compaction performance measurement (score before vs. after compaction) for any of the four models.
- No long-horizon coherence eval with a context-length or turn-count axis relevant to capability was found (the F52 card's secret-keeping-over-20-turns figure, §6.6.4.7, is an alignment measure and was not extracted).
- Whether task budgets are supported on each of the four models was not checked (the task-budgets page was not opened).

## 5. Per-model plain statement: does the card report long-context numbers?

### Takeaway
All four cards report exactly one long-context number each (a ProgramBench aggregate) and no per-context-length numbers; a context-fill curve cannot be sourced from Anthropic for any of the four.

### Cited Findings
- **Claude Fable 5.2 / Mythos 5.2:** NO per-length numbers. No GraphWalks, no MRCR. Reports ProgramBench 85.6% (91.6% with remaining-context display) and a 68% context-exhaustion rate (§8.10.1, pp.215–216). The card does not defer to the Fable 5 or 5.1 cards for long-context results. The ProgramBench figures are attributed to "Claude Fable 5.2" (not Mythos 5.2) in the text. — [F52 card](https://www.anthropic.com/claude-fable-5-2-mythos-5-2-system-card)
- **Claude Opus 5.5:** NO per-length numbers. No GraphWalks, no MRCR. Reports ProgramBench 91.2% (§8.10.1, p.183); the 39% exhaustion rate and 91.1% remaining-context-display score appear only in the later Fable 5.2 card (p.216). — [O55 card](https://www.anthropic.com/claude-opus-5-5-system-card); [F52 card](https://www.anthropic.com/claude-fable-5-2-mythos-5-2-system-card)
- **Claude Sonnet 5.5:** NO per-length numbers. No GraphWalks, no MRCR. Reports ProgramBench 79.7% only (§8.10.1, pp.117–118). — [S55 card](https://www.anthropic.com/claude-sonnet-5-5-system-card)
- **Claude Haiku 5.5:** NO per-length numbers. No GraphWalks, no MRCR. Reports ProgramBench 82.0% only (§8.7.1, p.117). — [H55 card](https://www.anthropic.com/claude-haiku-5-5-system-card)
- The system-cards index lists the four cards with dates: Opus 5.5 (September 2026), Sonnet 5.5 (September 2026), Haiku 5.5 (October 2026), Fable 5.2 and Mythos 5.2 (November 2026). — [Anthropic system cards](https://www.anthropic.com/system-cards)

### Inferences
- Any context-fill curve for these four models will have to come from third-party measurement (another researcher's scope) or be presented as "Anthropic publishes endpoints/aggregates only". Anthropic's Mythos 5 GraphWalks numbers (BFS 91.1 at 256K → 79.4 at 1M; Parents 99.96 → 97.5) could be shown only as a clearly labelled predecessor-model reference, never as Fable 5.2 data.

### Gaps
- Whether Anthropic published length-binned results for these models anywhere outside the system cards, launch posts and docs (e.g. an engineering blog post or the Transparency Hub) was not exhaustively checked; none surfaced in search.
