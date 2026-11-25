# Opus 5.5 (and Sonnet 5.5 / Haiku 5.5 / Fable 5.2) performance vs context length: leaderboards, benchmark sites, other labs' tables

Pass date: 25 November 2026. Excludes, as instructed, the already-known Google Gemini 4 Argon GraphWalks BFS pair (90.6% / 66.8%) except where a site merely re-hosts it (noted, not re-reported as new). No predecessor models reported.

## Data table (everything new found in this pass)

| model | benchmark | context size / bin | score | settings | who ran it | date | URL |
|---|---|---|---|---|---|---|---|
| Claude Opus 5.5 | AA-LCR v1.1 (single-point) | document sets 10K-100K tokens (cl100k), not binned | 84.7% (0.8467) | effort max, "Default Fallback" | Artificial Analysis | page read 25 Nov 2026 | https://artificialanalysis.ai/models/claude-opus-5-5 |
| Claude Opus 5.5 | AA-LCR v1.1 (single-point) | same | 84.7% (0.8467) | effort xhigh | Artificial Analysis | same | same |
| Claude Opus 5.5 | AA-LCR v1.1 (single-point) | same | 82.7% (0.8267) | effort high | Artificial Analysis | same | same |
| Claude Opus 5.5 | AA-LCR v1.1 (single-point) | same | 84.3% (0.8433) | effort medium | Artificial Analysis | same | same |
| Claude Opus 5.5 | AA-LCR v1.1 (single-point) | same | 80.7% (0.8067) | effort low | Artificial Analysis | same | same |
| Claude Sonnet 5.5 | AA-LCR v1.1 (single-point) | same | 82.7% (0.8267) | effort max | Artificial Analysis | same | same |
| Claude Fable 5.2 | AA-LCR v1.1 (single-point) | same | 83.3% (0.8333) | effort max | Artificial Analysis | same | same |
| Claude Fable 5.2 | AA-LCR v1.1 (single-point) | same | 82.7% (0.8267) | effort xhigh | Artificial Analysis | same | same |
| Claude Fable 5.2 | AA-LCR v1.1 (single-point) | same | 83.3% (0.8333) | effort high | Artificial Analysis | same | same |
| Claude Fable 5.2 | AA-LCR v1.1 (single-point) | same | 80.0% (0.80) | effort medium | Artificial Analysis | same | same |
| Claude Fable 5.2 | AA-LCR v1.1 (single-point) | same | 80.0% (0.80) | effort low | Artificial Analysis | same | same |
| Claude Opus 5.5 | GDP.pdf "all-pass" (AA "Long-Context" category; single-point, no length bin) | not stated | 26.2% max / 26.6% xhigh / 28.8% high / 25.6% medium / 25.6% low | by effort | Artificial Analysis | same | same |
| Claude Sonnet 5.5 | GDP.pdf all-pass (single-point) | not stated | 25.8% | effort max | Artificial Analysis | same | same |
| Claude Fable 5.2 | GDP.pdf all-pass (single-point) | not stated | 24.2% max / 21.4% xhigh / 21.8% high / 23.8% medium / 26.8% low | by effort | Artificial Analysis | same | same |
| Claude Opus 5.5 | MLCR-AA (medical long-context reasoning) overall (single-point) | not stated | 27.8% max / 25.0% xhigh / 15.0% high / 22.2% medium / 19.4% low | by effort | Artificial Analysis | same | same |
| Claude Sonnet 5.5 | MLCR-AA overall (single-point) | not stated | 41.7% | effort max | Artificial Analysis | same | same |
| Claude Fable 5.2 | MLCR-AA overall (single-point) | not stated | 43.9% | effort max | Artificial Analysis | same | same |
| Claude Opus 5.5 | LongBench v2, 30-question PILOT, "alone" control: length = Short | LongBench v2 "short" bucket (official definition: <32K words) | 70.0% (7/10) | w/ CoT protocol; read via Claude Code sub-agents using Read on 12,000-token parts; cap 450K o200k tokens; NOT single-prompt context fill | SAWM / Basira (memory-product vendor, self-run) | undated (2026) | https://github.com/SAWM-io/basira-longbench-v2 |
| Claude Opus 5.5 | same: length = Medium | "medium" bucket (32K-128K words) | 80.0% (8/10) | same | same | same | same |
| Claude Opus 5.5 | same: length = Long | "long" bucket (128K-2M words); 5 texts middle-truncated to cap | 80.0% (8/10) | same | same | same | same |
| Claude Opus 5.5 | same: overall | all | 76.7% (23/30), Wilson 95% CI 59.1-88.2% | same | same | same | same |

Notes on the table:
- AA values were read from the structured data embedded in the page HTML (keys `lcr`, `gdpPdfAllPass`, `mlcrOverall`), not from a rendered chart; percentages are my rounding of the decimals shown in brackets. The WebFetch text rendering of the same page did not show them (charts are client-rendered).
- The mapping of JSON key `mlcrOverall` to "MLCR-AA overall" and `gdpPdfAllPass` to "GDP.pdf all-pass" is my reading of the key names plus the AA evaluation pages; treat labels as high-confidence but not explicitly captioned.
- The LongBench v2 bucket word ranges are from the LongBench v2 paper's definitions (my recollection of the paper, not restated on the Basira page).

## Does any source give Opus 5.5 (or Sonnet 5.5 / Haiku 5.5 / Fable 5.2) scores at two or more context sizes on the same test?

### Takeaway
No new clean series was found. Beyond the already-known Google GraphWalks pair, the only thing with more than one length bucket is a 30-question vendor pilot of LongBench v2 (10 questions per bucket, read in chunks by sub-agents), which is too small and methodologically off-target to plot as a context-fill curve. Every third-party site that shows binned Opus 5.5 numbers is re-hosting Google's two GraphWalks points.

### Cited Findings
- Basira/SAWM LongBench v2 pilot, Opus 5.5 "alone": Short 70.0% (7/10), Medium 80.0% (8/10), Long 80.0% (8/10), overall 76.7% (23/30, Wilson 95% CI 59.1-88.2%); Easy 7/7, Hard 16/23. Opus read the text through Claude Code sub-agents using the Read tool on 12,000-token parts, with a 450,000-token cap and middle truncation for 5 texts. Page is undated. — [SAWM-io/basira-longbench-v2](https://github.com/SAWM-io/basira-longbench-v2)
- OpenAI's GPT-6 Astra launch page has a Long Context table (MRCR v2 8-needle 256K-512K and 512K-1M) with Claude columns for Fable 5.1, Fable 5 and Opus 5 only, and every Claude cell is "-". Opus 5.5 / Sonnet 5.5 / Fable 5.2 do not appear anywhere on the page; no GraphWalks rows. — [OpenAI GPT-6 Astra](https://openai.com/index/gpt-6-astra/)
- Context Arena, re-checked 25 Nov 2026: the 8-needle endpoint returns 184 model entries; the newest Anthropic slugs are claude-opus-5 (max), claude-sonnet-5 (max), claude-opus-4.8 (max). No slug containing opus-5.5, sonnet-5.5, haiku-5.5, fable or mythos. The only "5.5"/"5.2" matches are openai/gpt-5.5, z-ai/glm-5.2, qwen/qwen3.5-27b. — [Context Arena API, needles=8](https://contextarena.ai/api/needle-summary?needles=8)
- Context Arena 2-needle and 4-needle endpoints return an empty `models` array and empty `available_bins` (0 models), i.e. no data for anyone at those endpoints. — [needles=2](https://contextarena.ai/api/needle-summary?needles=2); [needles=4](https://contextarena.ai/api/needle-summary?needles=4)
- docsbot.ai's Opus 5.5 vs GPT-6 Astra page shows two binned rows (GraphWalks BFS 0K-128K and 256K-1M) and attributes both to the "Google Gemini 4 comparison table"; no MRCR row. (An earlier search snippet suggested the two models' numbers were swapped on a mirror page; the opened page shows Opus 5.5 = 90.6 / 66.8, consistent with Google.) — [docsbot.ai](https://docsbot.ai/models/compare/claude-opus-5-5/gpt-6-astra)
- klu.ai's GraphWalks page lists only Google's two BFS numbers for Opus 5.5, states Google ran both, has no parents-task result, no entries for Sonnet 5.5 / Haiku 5.5 / Fable 5.2 / Mythos 5.2, and says Anthropic's Opus 5.5 system card reports no GraphWalks result. — [klu.ai GraphWalks](https://klu.ai/benchmarks/graphwalks)
- benchlm.ai's Opus 5.5 page lists ~150 benchmarks; the only binned long-context item is GraphWalks BFS 256K-1M 66.8%, labelled as from the Google Gemini 4 Argon launch chart. No MRCR or LongBench v2 score. — [benchlm.ai Opus 5.5](https://benchlm.ai/models/claude-opus-5-5)
- benchlm.ai's reasoning leaderboard (last verified 23 Nov 2026) lists LongBench v2, MRCR v2 and GraphWalks BFS 256K-1M as weighted inputs and MRCR v2 64K-128K, MRCR v2 128K-256K, GraphWalks BFS 128K as "display only", but the page text gives no per-model values for any of them. — [benchlm.ai reasoning](https://benchlm.ai/reasoning)
- llm-stats.com's Opus 5.5 page has a section titled "Performance by conversation depth — How Claude Opus 5.5 holds up as conversations get longer" and a context slider (8K / 128K / 1M), but the chart is a client-side loading placeholder; no values are in the served HTML and no data endpoint is exposed in the page source. — [llm-stats.com Opus 5.5](https://llm-stats.com/models/claude-opus-5-5)
- sota2.com's composite long-context leaderboard (MRCR v2, GraphWalks, LongBench v2, RULER, AA-LCR) has only GLM-5 / IndexCache rows dated 2026.03; no Claude entries; page looks templated/auto-generated. — [sota2.com](https://www.sota2.com/research/sota/long-context-language-modeling-on-composite-suite-mrcr-v2-graphwalks-longbench-v2-ruler-aa-lcr)
- The akitaonrails "LLM Benchmark v4" post (23 Sep 2026) includes Opus 5.5 but measures a single coding task (score 100.0, $16.63, 64 min); no context-size variation. — [akitaonrails](https://akitaonrails.com/en/2026/09/23/llm-benchmark-v4-opus-5-5-gpt-6-sol-luna-mimo-2-6-grok-4-7/)

### Inferences
- The Basira series should not be plotted as "score vs tokens in context": the model never held the whole text in one context window (it read 12K-token chunks via sub-agents), n = 10 per bucket means each question moves the score 10 points, the buckets are word-count ranges rather than token counts, and the run is by a vendor selling a memory layer. If used at all, label it as an agentic-reading pilot.
- llm-stats' "conversation depth" chart is the one lead that might be a real multi-point series; it would need a real browser to render. It is unknown whether it is measured data or derived, and what the x-axis unit is.
- As of this date the aggregator ecosystem (benchlm, klu, docsbot) all trace binned Opus 5.5 numbers back to the one Google table, which strongly suggests no second public binned series exists on indexed sites.
- UNVERIFIED (search snippets only, pages not opened, low-quality blogs): apineed.com claims "100% recall" for Opus 5.5 at 1M tokens with no methodology; do not use.

### Gaps
- llm-stats "Performance by conversation depth" values: not retrievable without JavaScript rendering.
- LMArena: could not open (HTTP 403).
- LiveBench: page opened but served only a JS shell (title "LiveBench 2.0"); no model data readable. LiveBench has no context-length-binned category that I could confirm.
- xAI, Meta, Mistral, DeepSeek, Qwen, Moonshot, Zhipu launch posts: an extended search for any Sept-Nov 2026 launch table comparing against Opus 5.5 on MRCR / long-context returned nothing; I did not open individual launch posts, so this is "nothing surfaced", not "confirmed absent".
- GPT-5.6 launch post / OpenAI system cards: not opened separately (GPT-5.6 Sol predates Opus 5.5; the Astra page was opened and has no 5.5-generation Claude data).
- NoLiMa (nolima.github.io returned 404), LongMemEval, LoCoMo, BABILong, Oolong, LongCodeBench, Michelangelo, HELMET, Loong: no page with Opus 5.5 bare-model binned results surfaced in search. LongMemEval hits were memory-system vendor results using Opus 5.5 as the reader (e.g. Recallium), not bare-model context-length data; not recorded as data.
- Sonnet 5.5 / Haiku 5.5 / Fable 5.2: no binned long-context series found anywhere in this pass. Haiku 5.5 has no long-context number at all in what I could read.

## Does any source give a single long-context score for these models?

### Takeaway
Yes: Artificial Analysis has AA-LCR v1.1 scores for Opus 5.5 at all five effort levels (80.7% to 84.7%), Fable 5.2 at five effort levels (80.0% to 83.3%) and Sonnet 5.5 at max (82.7%). AA-LCR documents are 10K-100K tokens and results are not binned by length, so these are single points near the low end of the context range.

### Cited Findings
- Opus 5.5 AA-LCR v1.1: max 0.8467, xhigh 0.8467, high 0.8267, medium 0.8433, low 0.8067. Sonnet 5.5 (max): 0.8267. Fable 5.2: max 0.8333, xhigh 0.8267, high 0.8333, medium 0.80, low 0.80. Read from embedded page data. — [Artificial Analysis Opus 5.5 model page](https://artificialanalysis.ai/models/claude-opus-5-5)
- AA-LCR is described as measuring ability to "extract, reason about, and synthesize information from long-form documents ranging from 10k to 100k tokens (measured using the cl100k_base tokenizer)"; score is average pass rate. Leaders on the page: Kimi K3 (Max) 88.7%, Step 5 Preview 88.3%, Kimi K3.1 Pro 86.3%. — [AA-LCR evaluation page](https://artificialanalysis.ai/evaluations/artificial-analysis-long-context-reasoning)
- Artificial Analysis' launch article (22 Sep 2026) says Opus 5.5 "remains behind on CritPt, AA-LCR, and GDP.pdf" and that it was run at all five effort levels with default fallback enabled; 1M context window. — [AA article](https://artificialanalysis.ai/articles/claude-opus-5-5)
- benchlm.ai independently displays "AA-LCR 84.7%" for Opus 5.5, attributed to Artificial Analysis, matching the max/xhigh value above. — [benchlm.ai Opus 5.5](https://benchlm.ai/models/claude-opus-5-5)
- Same AA model page data: GDP.pdf all-pass for Opus 5.5 = 0.262 (max), 0.266 (xhigh), 0.288 (high), 0.256 (medium), 0.256 (low); Sonnet 5.5 max 0.258; Fable 5.2 0.242 (max), 0.214 (xhigh), 0.218 (high), 0.238 (medium), 0.268 (low). MLCR overall: Opus 5.5 0.278 (max), 0.25 (xhigh), 0.15 (high), 0.222 (medium), 0.194 (low); Sonnet 5.5 max 0.417; Fable 5.2 max 0.439. — [Artificial Analysis Opus 5.5 model page](https://artificialanalysis.ai/models/claude-opus-5-5)
- LongBench v2 (pilot, overall): Opus 5.5 alone 76.7% on 30 questions; vendor-run, non-standard protocol. — [SAWM-io/basira-longbench-v2](https://github.com/SAWM-io/basira-longbench-v2)

### Inferences
- Opus 5.5's AA-LCR is essentially flat across effort (about 81-85%), and it sits roughly 4 points below the AA-LCR leader; this gives one anchor point for "performance at <=100K tokens" on a reasoning-over-documents task but says nothing about degradation beyond 100K.
- AA-LCR points can't be joined to the GraphWalks points on one curve: different tasks, different scoring.

### Gaps
- No AA-LCR value was found for Haiku 5.5 (model name appears in AA's page data but I did not extract an `lcr` value for it) or for Sonnet 5.5 at efforts other than max.
- No official LongBench v2 leaderboard entry: longbench2.github.io opened (HTTP 200) and contains no string matching Opus/Sonnet/Haiku 5.5 or Fable/Mythos 5.2.
- Exact AA run dates per score are not shown; only the 22 Sep 2026 article date and today's page read.

## For each place checked: has data / confirmed no entry / could not open

### Takeaway
Only Artificial Analysis has first-party-run long-context numbers for these models (single-point). Everything else is either a re-host of Google's GraphWalks pair, has no entry, or could not be rendered.

### Cited Findings
- HAS DATA (single-point): Artificial Analysis model page, AA-LCR v1.1 / GDP.pdf / MLCR. — [AA](https://artificialanalysis.ai/models/claude-opus-5-5)
- HAS DATA (3 length buckets, weak): Basira LongBench v2 pilot. — [GitHub](https://github.com/SAWM-io/basira-longbench-v2)
- RE-HOSTS GOOGLE ONLY: docsbot.ai, klu.ai, benchlm.ai model page. — [docsbot](https://docsbot.ai/models/compare/claude-opus-5-5/gpt-6-astra); [klu](https://klu.ai/benchmarks/graphwalks); [benchlm](https://benchlm.ai/models/claude-opus-5-5)
- CONFIRMED NO ENTRY (page opened): OpenAI GPT-6 Astra launch page (Claude long-context cells all "-", no 5.5-generation models). — [OpenAI](https://openai.com/index/gpt-6-astra/)
- CONFIRMED NO ENTRY: Context Arena 8-needle (184 entries, none for these models); 2- and 4-needle endpoints empty for all models. — [Context Arena](https://contextarena.ai/api/needle-summary?needles=8)
- CONFIRMED NO LONG-CONTEXT BENCHMARK: Vals AI Opus 5.5 page lists 35 benchmarks (Vals Index 68.70% etc.), none long-context or length-binned. — [Vals AI](https://www.vals.ai/models/anthropic_claude-opus-5-5)
- CONFIRMED NO LONG-CONTEXT BENCHMARK: Scale SEAL leaderboard index lists Opus 5.5 / Fable 5.2 / Sonnet 5.5 / Haiku 5.5, but its leaderboards (audiomc, distressbench, drugdiscoverybench, fortress, hil, hle-diamond, hss, mcp_atlas, multichallenge, prbench, rli, swe-atlas, swe_bench_pro_public_v2) include no long-context test. — [Scale SEAL](https://scale.com/leaderboard)
- CONFIRMED NO LONG-CONTEXT BENCHMARK: Epoch AI benchmarking hub page names no long-context benchmark; mentions Fable 5.2 only as top ECI score. — [Epoch AI](https://epoch.ai/benchmarks)
- CONFIRMED NO BENCHMARK DATA: OpenRouter Opus 5.5 page (pricing / API info only). — [OpenRouter](https://openrouter.ai/anthropic/claude-opus-5.5)
- CONFIRMED NO ENTRY: LongBench v2 official site; Chroma "Context Rot" report page (no matching model strings in HTML). — [LongBench v2](https://longbench2.github.io/); [Chroma context rot](https://research.trychroma.com/context-rot)
- CONFIRMED NO ENTRY: sota2.com composite long-context leaderboard. — [sota2](https://www.sota2.com/research/sota/long-context-language-modeling-on-composite-suite-mrcr-v2-graphwalks-longbench-v2-ruler-aa-lcr)
- OPENED BUT DATA NOT RENDERED: llm-stats.com (conversation-depth chart placeholder); benchlm.ai/reasoning (column names, no values); LiveBench (JS shell); Kaggle benchmarks (JS shell, no matching strings). — [llm-stats](https://llm-stats.com/models/claude-opus-5-5); [benchlm reasoning](https://benchlm.ai/reasoning); [LiveBench](https://livebench.ai/); [Kaggle](https://www.kaggle.com/benchmarks)
- COULD NOT OPEN: LMArena leaderboard (HTTP 403); nolima.github.io (404, likely wrong URL). — [LMArena](https://lmarena.ai/leaderboard)

### Inferences
- The user's belief that "someone benchmarked Opus 5.5 at several context sizes" is, on public evidence two months after release, supported only by Google's two-bin GraphWalks row; Context Arena (the natural home for an MRCR curve) stopped adding Anthropic models after Opus 5 / Sonnet 5.
- The most promising unchecked route for a real curve is a rendered-browser read of llm-stats' conversation-depth chart, and Anthropic's own Opus 5.5 / Sonnet 5.5 system cards (out of scope for this pass; benchlm cites a "Sonnet 5.5 system card" as a source for other numbers).

### Gaps
- Not opened: xAI / Meta / Mistral / DeepSeek / Qwen / Moonshot / Zhipu launch posts individually; GPT-5.6 launch post; OpenAI system card PDFs; LoCoMo / BABILong / Oolong / LongCodeBench / HELMET / Loong / Michelangelo leaderboards individually. Searches for these combined with "Opus 5.5" returned nothing relevant.
