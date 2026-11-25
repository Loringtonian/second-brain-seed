# Independent / third-party performance-vs-context-length data for Claude Haiku 5.5, Sonnet 5.5, Opus 5.5, Fable 5.2

State as of 25 November 2026. All retrievals made on that date.

**Headline:** only ONE non-Anthropic length-binned series exists for any of the four models: Google's self-computed GraphWalks BFS run, two bins, covering Opus 5.5 (and Fable 5.1 as a stand-in for Fable 5.2). Nothing was found for Haiku 5.5, Sonnet 5.5 or Fable 5.2 itself. Context Arena and Fiction.LiveBench list none of the four (confirmed by reading their data endpoints directly, not just search).

## Data tables (plot-ready)

### Table A — in-scope series (the four models, plus labelled stand-ins)

| benchmark | model | bin | score | settings | who ran it | date | source | PRIMARY/SECONDARY |
|---|---|---|---|---|---|---|---|---|
| GraphWalks BFS (F1) | Claude Opus 5.5 | "Up to 128k" (650 items with context <= 128k tokens) | 90.6% | Single-turn; effort not stated explicitly for this row (doc says it "defaults to maximum thinking/reasoning settings available" for competitor models) | Google (self-computed for all models) | Results "as of October, 2026"; methodology "as of September, 2026" | [Gemini 4 Argon model evaluation PDF, p.3 + p.5](https://storage.googleapis.com/deepmind-media/gemini/gemini_4_argon_model_evaluation.pdf); same table on [model card page](https://deepmind.google/models/model-cards/gemini-4-argon/) | PRIMARY |
| GraphWalks BFS (F1) | Claude Opus 5.5 | "256k to 1M" (200 problems, context between 256k and 1M tokens) | 66.8% | as above | Google (self-computed) | Oct 2026 | same | PRIMARY |
| GraphWalks BFS (F1) | Claude Fable 5.1 (STAND-IN for Fable 5.2; not 5.2) | "Up to 128k" | 91.4% | as above | Google (self-computed) | Oct 2026 | same | PRIMARY |
| GraphWalks BFS (F1) | Claude Fable 5.1 (STAND-IN for Fable 5.2; not 5.2) | "256k to 1M" | 65.0% | as above | Google (self-computed) | Oct 2026 | same | PRIMARY |

Comparison rows in the same Google table (for chart context only, not Claude): Gemini 4 Argon 99.7% / 84.2%; GPT-6 Astra 98.7% / 71.8% (same PDF, p.5).

Aggregator copies of these same four numbers (SECONDARY, no new data): [benchlm.ai GraphWalks BFS 256K–1M](https://benchlm.ai/benchmarks/graphwalks-bfs-256k-1m) (lists 66.8 / 65.0 only; mislabels them "lab-reported, from another publisher's document"), [klu.ai/benchmarks/graphwalks](https://klu.ai/benchmarks/graphwalks) (lists all four, correctly attributes to Google self-computed, Argon PDF p.5).

### Table B — confirmed absences (no row for any of the four)

| source | what was checked | result | date checked |
|---|---|---|---|
| Context Arena (MRCR) | `contextarena.ai/api/models` (545 model slugs) and `/api/needle-summary?needles=8` (184 scored model/effort entries) | No slug for claude-haiku-5.5, claude-sonnet-5.5, claude-opus-5.5, claude-fable-5.1, claude-fable-5.2 or any mythos. `anthropic/claude-fable-5` is registered but has zero results (`summaries: []`, `has_sufficient_data: false`). Newest scored Anthropic entries: claude-opus-5 (max) and claude-sonnet-5 (max). | 25 Nov 2026 |
| Fiction.LiveBench (fiction.live) | Story node API + chapter content | Post titled "Fiction.liveBench April 04 2026", status "hiatus", last touched 3 May 2026 (before Fable 5 launched). Newest Claude named in content: opus-4 / sonnet-4. None of the four. | 25 Nov 2026 |
| Fiction.LiveBench mirrors (pareto.my, benchgecko.ai, both via Epoch AI hub) | Leaderboard tables | Only Claude 3.7 Sonnet, Opus 4, Sonnet 4. pareto.my explicitly lists Fable 5.2, Opus 5.5, Sonnet 5.5, Haiku 5.5 as "missing". Single score, no per-length columns. | 25 Nov 2026 |
| Epoch AI GraphWalks review | Review page | No per-model scores at all; methodology only. | 25 Nov 2026 |
| Vals AI Opus 5.5 page | Model page | 35 benchmarks, none binned by context length, none long-context. | 25 Nov 2026 |
| Artificial Analysis Opus 5.5 article (22 Sep 2026) | Article | Says Opus 5.5 "remains behind on ... AA-LCR" but gives no number and no length bins. | 25 Nov 2026 |
| llm-stats Fable 5.2 page | Model page | No MRCR / GraphWalks / length-binned values. | 25 Nov 2026 |
| OpenAI GPT-6 Astra launch (via secondary coverage) | Search | MRCR v2 8-needle bins reported only for OpenAI models (Astra, GPT-5.6 Sol); no Claude comparison rows found. | 25 Nov 2026 |

### Table C — OUT OF SCOPE predecessors (NOT one of the four; NOT an approved stand-in). Recorded only so the report writer knows what the nearest independent curve looks like; do not plot as 5.5 data.

Context Arena, MRCR 8-needle, mode "full", run by Context Arena, 861 runs per model, retrieved 25 Nov 2026 from [contextarena.ai/api/needle-summary?needles=8](https://contextarena.ai/api/needle-summary?needles=8) (site: [contextarena.ai](https://contextarena.ai/)). PRIMARY. Bin label = upper edge in tokens; score = mean pointwise `avg_score`; 95% CI from the API.

| bin | Claude Opus 5 (reasoning: max) | n | 95% CI | Claude Sonnet 5 (reasoning: max) | n | 95% CI |
|---|---|---|---|---|---|---|
| 8K (8,192) | 0.9874 | 80 | 0.962–1.000 | 0.9642 | 80 | 0.917–1.000 |
| 16K (16,384) | 0.9993 | 80 | 0.999–1.000 | 0.9491 | 80 | 0.889–0.987 |
| 32K (32,768) | 0.9926 | 136 | 0.978–1.000 | 0.8789 | 136 | 0.823–0.929 |
| 64K (65,536) | 0.9995 | 85 | 0.999–1.000 | 0.6804 | 85 | 0.588–0.772 |
| 128K (131,072) | 0.9131 | 103 | 0.856–0.961 | 0.5291 | 103 | 0.439–0.620 |
| 256K (262,144) | 0.6564 | 141 | 0.577–0.730 | 0.5222 | 141 | 0.438–0.602 |
| 512K (524,288) | 0.4253 | 236 | 0.363–0.484 | 0.3201 | 236 | 0.264–0.375 |
| 1M (1,048,576) | no entry returned | – | – | no entry returned | – | – |
| AUC @128k | 0.9747 | | | 0.7161 | | |
| AUC @1M | 0.4574 | | | 0.3376 | | |
| cumulative avg @128k / @1M | 0.9784 / 0.7467 | | | 0.8003 / 0.6055 | | |

Boolean AI "Context Rot Quantified" (agentic, terminal-bench v4 subset of 20 tasks, context prefilled before the task; Claude Opus 5, effort not stated; 15 Sep 2026; [source](https://www.boolean.ai/blog/context-rot-quantified)). PRIMARY. Task pass rate:

| prefill | 0 tokens | 250k tokens | 500k tokens |
|---|---|---|---|
| Unrelated context (Apache Spark Q&A) | 0.50 | 0.52 | 0.48 |
| Related context (task-specific Q&A) | 0.50 | 0.425 | 0.398 |

## Context Arena (MRCR per-bin): does it list any of the four?

### Takeaway
No. As of 25 Nov 2026 Context Arena has no entry for Haiku 5.5, Sonnet 5.5, Opus 5.5, Fable 5.1 or Fable 5.2; Fable 5 is registered but has no results. Only the 8-needle setting is currently served.

### Cited Findings
- The model registry returns 545 slugs; the only slugs containing "5.5", "5.1" or "5.2" are OpenAI and Z.ai models. No Anthropic 5.5 slug, no mythos slug — [contextarena.ai/api/models](https://contextarena.ai/api/models)
- `anthropic/claude-fable-5` exists with reasoning levels max/xhigh/high/medium/low but `has_sufficient_data: false`, and its per-model summary is empty (`"summaries":[]`) — [contextarena.ai/api/models/anthropic%2Fclaude-fable-5/summary](https://contextarena.ai/api/models/anthropic%2Fclaude-fable-5/summary)
- `/api/available-needles` returns `{"mode":"full","needles":[8]}` (and the same for lite mode): 2- and 4-needle leaderboards are not currently exposed — [contextarena.ai/api/available-needles](https://contextarena.ai/api/available-needles)
- Bins served: 8192, 16384, 32768, 65536, 131072, 262144, 524288, 1048576 — [contextarena.ai/api/needle-summary?needles=8](https://contextarena.ai/api/needle-summary?needles=8)
- Newest scored Anthropic models are Opus 5 (max) and Sonnet 5 (max); full per-bin series in Table C. Others scored: Opus 4.6, 4.7, 4.8, Sonnet 4.6, Haiku 4.5 — same URL.
- The benchmark dataset is OpenAI's MRCR (the site links `huggingface.co/datasets/openai/mrcr`) — [contextarena.ai](https://contextarena.ai/)

### Inferences
- For Opus 5 and Sonnet 5 the API lists `max_context_length: 1000000` and returns bin metrics only up to the 524,288 bin; a 1,048,576-token bin cannot fit a 1,000,000-token window, so even when 5.5 models are added the top plottable bin will probably be 512K, not a true 1M.
- The site is client-rendered, so WebFetch/search engines see only the title; the JSON API is the only reliable way to read it. Search-engine "no result" for Opus 5.5 is consistent with the API.

### Gaps
- No MRCR series from Context Arena for any of the four models. No explanation on the site for why Fable 5 is registered but unscored.
- I did not find an archived copy of older 2-/4-needle tables.

## Fiction.LiveBench: does it list any of the four?

### Takeaway
No. The benchmark post appears dormant since spring 2026 and predates all four models.

### Cited Findings
- The benchmark story node is titled "Fiction.liveBench April 04 2026", `storyStatus: "hiatus"`, last-updated timestamp 1777842308513 ms = 3 May 2026 UTC (and that last touch is a reader chat message, not a results update) — [fiction.live/api/node/oQdzQvKHw8JyXbN87](https://fiction.live/api/node/oQdzQvKHw8JyXbN87)
- Chapter content of that post mentions Claude only as sonnet 3.7, opus-4, sonnet-4; no 5.x Claude names — [fiction.live benchmark post](https://fiction.live/stories/Fiction-liveBench-Feb-21-2025/oQdzQvKHw8JyXbN87)
- Epoch-hub mirror via pareto.my lists four Claude rows (3.7 Sonnet, 3.7 Sonnet 8K, Opus 4, Sonnet 4) and names Fable 5.2, Opus 5.5, Sonnet 5.5, Haiku 5.5 as missing — [pareto.my](https://pareto.my/benchmark/fiction-livebench)
- benchgecko mirror: Claude 3.7 Sonnet 83.3, Opus 4 61.1, Sonnet 4 46.9; no 5.x — [benchgecko.ai](https://www.benchgecko.ai/benchmark/fiction-livebench)

### Inferences
- The results table on fiction.live is an image, so model names inside it were not machine-readable; the absence claim rests on the 3 May 2026 last-update date (before Fable 5's June 2026 launch and Opus 5.5's 22 Sep 2026 launch) plus the mirrors.

### Gaps
- Could not OCR the latest results image; if the image was silently replaced without a timestamp change, it would not be detected.

## Other labs' posts/model cards with these Claude models as per-length comparison rows

### Takeaway
Google's Gemini 4 Argon evaluation document (October 2026) is the primary source for the reported numbers, and it is a genuine third-party run: Google states GraphWalks was self-computed for all models. It has exactly two bins and BFS only; there are no other bins. No OpenAI or xAI table with Claude 5.5-generation rows was found.

### Cited Findings
- Results table (p.5): GraphWalks "Up to 128k, BFS (F1)": Argon 99.7%, GPT-6 Astra 98.7%, Claude Fable 5.1 91.4%, Claude Opus 5.5 90.6%. "256k to 1M, BFS (F1)": Argon 84.2%, Astra 71.8%, Fable 5.1 65.0%, Opus 5.5 66.8% — [Gemini 4 Argon model evaluation PDF](https://storage.googleapis.com/deepmind-media/gemini/gemini_4_argon_model_evaluation.pdf)
- Methodology (p.3): "Graphwalks results for all models are self-computed. The 'Up to 128k' subset uses the 650 items with context length up to 128k tokens. The '256 to 1M' subset uses the 200 GraphWalks problems with context lengths between 256k and 1M tokens." — same PDF
- General methodology (p.2): non-Gemini results are provider self-reported "unless otherwise mentioned"; for Astra, Fable 5.1 and Opus 5.5 "we default to reporting maximum thinking/reasoning settings available". Benchmarks "as of September, 2026"; results "as of October, 2026" — same PDF
- The same two rows appear on the HTML model card — [deepmind.google model card](https://deepmind.google/models/model-cards/gemini-4-argon/)
- Epoch AI's review of GraphWalks (29 Sep 2026): OpenAI reports four numbers (BFS <128k, BFS >128k, Parents <128k, Parents >128k); the >128k subset is "200 prompts at 256k tokens and 200 prompts at 1M tokens"; harness is single-turn, no system prompt, no tools — [epoch.ai/benchmarks/graphwalks/review](https://epoch.ai/benchmarks/graphwalks/review)
- Secondary coverage states Anthropic has not published GraphWalks for Opus 5.5 or Fable 5.1, so the Google run is the only source for those models (unverified against the Anthropic cards by me) — [klu.ai/benchmarks/graphwalks](https://klu.ai/benchmarks/graphwalks)
- OpenAI's GPT-6 Astra launch MRCR v2 8-needle bins (Astra 100% at 256K–512K, 96.3% at 512K–1M; GPT-5.6 Sol 91.5% / 73.8%) are reported for OpenAI models only in the coverage found; no Claude rows — [pickaxe.co](https://pickaxe.co/post/gpt-6-astra) (SECONDARY; OpenAI primary post not fetched)

### Inferences
- The top bin is NOT a pure 1M bin: it pools prompts from 256k up to 1M. Google says 200 problems; Epoch describes the >128k set as 200 at 256k plus 200 at 1M. These counts conflict, so the mix inside Google's "256k to 1M" bin is unknown (could be the 1M half, the 256k half, or a sample). Plot it as a range bar (256k–1M), not a point at 1M.
- Token counts are presumably in the benchmark's nominal (OpenAI-tokenizer) units; Claude tokenizers count more tokens for the same text (Boolean AI measured ~1.7 Claude tokens per GPT token for its material — [source](https://www.boolean.ai/blog/context-rot-quantified)), so "1M" nominal prompts may not fit, or may fill more of, a Claude 1M window. Google does not say how over-length prompts were handled.
- Fable 5.1 and Opus 5.5 are statistically close in both bins (91.4 vs 90.6; 65.0 vs 66.8); with 200 items in the top bin, a ~2-point gap is within noise.
- With only two bins per model this is a two-point "curve": a ~24–26 point F1 drop from <=128k to 256k–1M.

### Gaps
- No Fable 5.2, Sonnet 5.5 or Haiku 5.5 rows in any other lab's table found.
- Exact effort/thinking setting Google used for the Claude GraphWalks runs is not stated row-by-row.
- GraphWalks "parents" bins not reported by Google.
- xAI and other labs' posts: nothing found with these models in length-binned tables (search only; individual launch posts not fetched).
- OpenAI's primary Astra post not fetched; absence of Claude rows rests on secondary coverage.

## Other length-binned benchmarks (NoLiMa, LongBench v2, LongCodeBench, LoCoMo/LongMemEval, AA-LCR, Epoch, Vals, LiveBench, Chroma, arXiv)

### Takeaway
No length-binned results for any of the four models were found on any of these. Where the models appear (Vals, LiveBench, Artificial Analysis) the scores are single aggregates with no context-length breakdown.

### Cited Findings
- Vals AI's Opus 5.5 page lists 35 benchmarks (effort "max", 128k max output); none is broken down by context length — [vals.ai](https://www.vals.ai/models/anthropic_claude-opus-5-5)
- Artificial Analysis's Opus 5.5 article (22 Sep 2026) mentions AA-LCR only to say Opus 5.5 "remains behind"; no score, no bins — [artificialanalysis.ai](https://artificialanalysis.ai/articles/claude-opus-5-5)
- Epoch's GraphWalks review page has no per-model scores — [epoch.ai](https://epoch.ai/benchmarks/graphwalks/review)
- llm-stats Fable 5.2 page: "Performance by conversation depth" section has a heading but no values; no MRCR/GraphWalks numbers — [llm-stats.com](https://llm-stats.com/models/claude-fable-5-2)
- A third-party guide notes the Opus 5.5 announcement published no new long-context benchmark — [codersera.com](https://codersera.com/blog/claude-opus-5-5-complete-guide-2026/) (SECONDARY, as surfaced by search; page not fetched)

### Inferences
- The absence across aggregators that normally scrape vendor cards suggests the vendor cards for the 5.5 generation carry little or no per-length long-context data either (another researcher covers Anthropic's own material).

### Gaps
- NoLiMa, LongBench v2, LongCodeBench, LoCoMo, LongMemEval, Chroma context-rot follow-ups: searches surfaced no results naming these four models; the individual leaderboards were not each opened, so this is "not found", weaker than the confirmed absences in Table B.
- LiveBench has an Opus 5.5 row (CSV dated 2026-10-25 surfaced in search) but LiveBench is not length-binned; not fetched.

## Agentic coding as context grows (success vs tokens in context, compaction ablations, Claude Code)

### Takeaway
No study measuring any of the four models' agentic success as a function of context fill was found. The nearest evidence is Boolean AI's September 2026 test on Claude Opus 5 (predecessor, out of scope): flat with irrelevant prefill, about a 15–20% relative drop with task-relevant prefill at 250k–500k tokens.

### Cited Findings
- Boolean AI, "Context Rot Quantified" (Sanyam Satia, Shrey Kumar Shahi, 15 Sep 2026): 20-task subset of terminal-bench v4, context prefilled to 0 / 250k / 500k tokens. Claude Opus 5 pass rate: unrelated prefill 0.50 / 0.52 / 0.48; related prefill 0.50 / 0.425 / 0.398. Opus 5 "collapse" rate 0% at all levels. Trial count and Claude effort not stated. No 5.5-generation or Fable models tested — [boolean.ai](https://www.boolean.ai/blog/context-rot-quantified)
- arXiv 2611.05306 "You don't need long context" (Belcak et al., NVIDIA authors, 3 Nov 2026): abstract claims frontier models lose little to no accuracy when held in short contexts via frequent compaction; abstract names no specific models — [arxiv.org/abs/2611.05306](https://arxiv.org/abs/2611.05306)
- CliffCompaction (arXiv 2609.26779) ran Claude Code on Terminal-Bench with GLM 5.3 Flash, not a Claude model: 76.69% at ~45K mean peak context vs 70.97% for Claude Code autocompaction at the same budget and 73.03% for the default 200K configuration — [arxiv.org/pdf/2609.26779](https://arxiv.org/pdf/2609.26779) (as summarised by search; PDF not read)
- A community GitHub issue researching Opus 5 degradation thresholds concluded there was no community evidence that context size degrades Opus 5 quality (informal) — [github.com/BaseInfinity/claude-sdlc-harness/issues/483](https://github.com/BaseInfinity/claude-sdlc-harness/issues/483) (as summarised by search; not read)

### Inferences
- Boolean's result suggests that for the Opus line, what fills the context (task-relevant vs irrelevant material) matters more than how full it is; a fill-vs-performance chart built on retrieval benchmarks (MRCR, GraphWalks) may overstate degradation in agentic use. This is one small study on a predecessor model.

### Gaps
- No compaction-threshold ablation or long-session evaluation on Haiku 5.5, Sonnet 5.5, Opus 5.5 or Fable 5.2, in Claude Code or elsewhere.
- Full text of arXiv 2611.05306 was not read; it may include Claude models in its tables.

## Per-series metadata summary (who ran it, date, effort, is the top bin really ~1M?)

### Takeaway
The single in-scope series is Google-run, October 2026, effort unstated (probably maximum), and its top bin is a 256k–1M pool, not a true 1M point.

### Cited Findings
- Google GraphWalks: self-computed by Google; F1 metric; bins "up to 128k" (650 items) and "256k to 1M" (200 problems); results as of Oct 2026 — [Argon evaluation PDF](https://storage.googleapis.com/deepmind-media/gemini/gemini_4_argon_model_evaluation.pdf)
- Context Arena (predecessors only): run by Context Arena; reasoning "max"; 861 runs; top returned bin 524,288 tokens; model max context listed as 1,000,000 — [contextarena.ai API](https://contextarena.ai/api/needle-summary?needles=8)
- Boolean AI (predecessor only): run by Boolean AI, 15 Sep 2026; top level 500k tokens; effort for Claude not stated — [boolean.ai](https://www.boolean.ai/blog/context-rot-quantified)

### Inferences
- No independent source found puts any Claude 5.x model at a verified ~1M-token point. Every "1M" label found is either a pooled range (Google) or stops at ~500k (Context Arena, Boolean).

### Gaps
- No independent data at all for Haiku 5.5, Sonnet 5.5, or Fable 5.2 specifically.
