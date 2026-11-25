# Independent performance-vs-context-length experiments on Claude Opus 5.5 / Sonnet 5.5 / Haiku 5.5 / Fable 5.2

Research date: 25 November 2026. Scope: independent (non-Anthropic) testers only; predecessors excluded.

## Data table (model | task | context size | score | n | who ran it | date | URL)

**No row qualifies as a context-length curve.** No independent source that could be opened publishes scores for Opus 5.5, Sonnet 5.5, Haiku 5.5 or Fable/Mythos 5.2 at two or more context sizes on the same task.

The only independent long-context numbers found for these models are single-point (one context band, not a curve) and search-snippet-only:

| model | task | context size | score | n | who ran it | date | URL | status |
|---|---|---|---|---|---|---|---|---|
| Haiku 5.5 (Max effort) | AA-LCR v1.1 (long-context reasoning) | docs of 10k-100k tokens, one pooled band | 83% | not stated | Artificial Analysis | not stated | https://artificialanalysis.ai/models/comparisons/claude-haiku-5-5-vs-claude-sonnet-5-5-xhigh | UNVERIFIED (search snippet only) |
| Sonnet 5.5 (Xhigh effort) | AA-LCR v1.1 | 10k-100k pooled | 80% | not stated | Artificial Analysis | not stated | same URL | UNVERIFIED (search snippet only) |
| Haiku 5.5 (Xhigh effort) | AA-LCR v1.1 | 10k-100k pooled | 78% | not stated | Artificial Analysis | not stated | https://artificialanalysis.ai/models/comparisons/claude-haiku-5-5-xhigh-vs-claude-sonnet-5-5-medium | UNVERIFIED (search snippet only) |
| Sonnet 5.5 (Medium effort) | AA-LCR v1.1 | 10k-100k pooled | 76% | not stated | Artificial Analysis | not stated | same URL | UNVERIFIED (search snippet only) |
| Opus 5.5 | AA-LCR | 10k-100k pooled | no number; only "remains behind on ... AA-LCR" | - | Artificial Analysis | 22 Sep 2026 | https://artificialanalysis.ai/articles/claude-opus-5-5 | opened; no figure |

These AA-LCR rows cannot be plotted as a curve: AA-LCR pools documents of 10k-100k tokens into one score and publishes no per-length breakdown.

## Has anyone published scores for Opus 5.5 (or Sonnet 5.5 / Haiku 5.5 / Fable 5.2) at two or more context sizes on the same task?

### Takeaway
Not that could be found. Across about 30 searches and page reads, no independent tester has published a multi-context-size measurement for any of the four models; the one unresolved lead is Context Arena (independent MRCR v2 leaderboard), whose data could not be read in this session.

### Cited Findings
- A vendor page updated for Opus 5.5 states "The Opus 5.5 announcement published no new long-context benchmark"; all of its measured numbers are for Opus 4.6, Sonnet 4.5, GPT-4o and Chroma's 2025 study. — [Morph, Claude context window](https://www.morphllm.com/claude-context-window)
- Boolean's "Context Rot Quantified" (15 Sep 2026) tested only GPT-5.6 Sol and Claude Opus 5; no addendum, no repo link, no follow-up. The blog index lists three posts, the newest being that one. — [Boolean post](https://www.boolean.ai/blog/context-rot-quantified); [Boolean blog index](https://www.boolean.ai/blog)
- Chroma's Context Rot report is still the 14 Jul 2025 version (last note 16 Jul 2025); newest Claude models are Opus 4 and Sonnet 4; no update mentioned. — [Chroma](https://www.trychroma.com/research/context-rot)
- AA-LCR uses documents "ranging from 10k to 100k tokens" and shows no breakdown by context length. — [Artificial Analysis AA-LCR](https://artificialanalysis.ai/evaluations/artificial-analysis-long-context-reasoning)
- Artificial Analysis's Opus 5.5 launch article gives no AA-LCR number, saying only that the model "remains behind on CritPt, AA-LCR, and GDP.pdf". — [AA Opus 5.5 article](https://artificialanalysis.ai/articles/claude-opus-5-5)
- Context Arena describes itself as an "LLM long-context benchmark leaderboard" and its front-end exports per-row fields `context_len`, `score`, `bin` for GDM MRCR v2 runs, i.e. exactly the shape of data wanted. Its page is a JavaScript app and returned no leaderboard text. — [Context Arena](https://contextarena.ai/)
- The legacy Context Arena site lists Claude only up to Opus 4.5 / Sonnet 4.5 / Haiku 4.5. — [old.contextarena.ai](https://old.contextarena.ai)
- A PromptZone article titled "Claude Opus 5.5 Performance Degradation" (header date 9 Nov 2026) contains no measurements: it is built on user reports, misstates the context window as 200K, and has internally inconsistent dates and prices. — [PromptZone](https://www.promptzone.com/mauricio_arellano/claude-opus-55-performance-degradation-reports-testing-and-alternatives-474e)
- Vellum's "Claude Opus 5.5 Benchmarks Explained" (22 Sep 2026) reproduces Anthropic's launch table and has no long-context results. — [Vellum](https://www.vellum.ai/md/blog/claude-opus-5-5-benchmarks-explained)
- Two long-context roundups that rank for these queries predate the models (21 May 2026 and 29 May 2026) and cover Opus 4.6/4.7, GPT-5.5, Gemini 3.x only. — [dev.to / ofox.ai](https://dev.to/owen_fox/long-context-llm-benchmarks-2026-which-model-actually-holds-accuracy-past-200k-tokens-17ka); [CodingFleet](https://codingfleet.com/blog/context-window-lie-how-well-ai-models-use-1m-tokens-2026/)
- Fiction.liveBench mirrors show data only through Jan 2026; newest Claude entries are Opus 4 and Sonnet 4. — [llmrun.dev](https://llmrun.dev/benchmark/fiction-livebench?show=all)
- The one GitHub repo matching "context rot benchmark" updated after the Opus 5.5 release is a needle-in-haystack harness with only a Gemini provider and no published results. — [sachin00810/context-rot-benchmark](https://github.com/sachin00810/context-rot-benchmark)

### Inferences
- The user's recollection most plausibly matches one of: (a) Boolean's Opus 5 study (0 / 250k / 500k tokens), which is the predecessor; (b) Anthropic's own system-card long-context tables; or (c) a Context Arena MRCR entry. None of (a) or (b) is an independent Opus 5.5 curve.
- A search summary claimed the Opus 5.5 system card reports "91.2% versus 85.4%" on a benchmark including contexts up to 1M tokens; sources conflicted on which benchmark, it is vendor data, and it was not opened. Treat as UNVERIFIED and out of scope here.

### Gaps
- Context Arena (contextarena.ai) could not be read: the page is a client-side app, and a direct call to its data API was blocked by the session's permission system. Whether it has Opus 5.5 / Sonnet 5.5 / Haiku 5.5 / Fable 5.2 MRCR v2 rows by context bin is unknown. This is the single most likely place for the curve; a human opening it in a browser would settle the question in a minute.
- The X/Twitter account of Context Arena's maker (@DillonUzar) could not be checked: the mirror host backlist.sdan.io is outside the session's allowed network domains.
- AA-LCR figures for Sonnet 5.5 / Haiku 5.5 come from search snippets only; the Artificial Analysis comparison pages were not opened, and the model page opened showed no AA-LCR number.

## If someone published a chart without numbers, describe it

### Takeaway
No chart-only publication for these models was found.

### Cited Findings
- None. No page opened contained a context-length chart for Opus 5.5, Sonnet 5.5, Haiku 5.5 or Fable 5.2. Artificial Analysis pages have charts, but for single pooled scores, not per-length series. — [AA-LCR page](https://artificialanalysis.ai/evaluations/artificial-analysis-long-context-reasoning)

### Inferences
- Context Arena renders per-bin plots in the browser; if the 5.5 models are listed there, that would be the chart, with downloadable numbers behind it.

### Gaps
- YouTube, X and Reddit are poorly covered by the search tool; no video or thread with such a chart surfaced, which is weak evidence of absence.

## For each major place checked: has data / opened and found nothing / could not open

### Takeaway
Nothing has data. Most primary places were opened and are empty for these models; the vendor engineering blogs were covered by search only.

### Cited Findings
Opened, nothing for these models:
- Boolean AI blog and post — [index](https://www.boolean.ai/blog), [post](https://www.boolean.ai/blog/context-rot-quantified)
- Chroma Context Rot — [page](https://www.trychroma.com/research/context-rot)
- Morph context-window page — [page](https://www.morphllm.com/claude-context-window)
- Vellum Opus 5.5 explainer — [page](https://www.vellum.ai/md/blog/claude-opus-5-5-benchmarks-explained)
- Artificial Analysis: Opus 5.5 article, Opus 5.5 model page, AA-LCR evaluation page (no per-length data; no Opus 5.5 LCR number visible) — [article](https://artificialanalysis.ai/articles/claude-opus-5-5), [model page](https://artificialanalysis.ai/models/claude-opus-5-5), [AA-LCR](https://artificialanalysis.ai/evaluations/artificial-analysis-long-context-reasoning)
- Fiction.liveBench mirror — [llmrun.dev](https://llmrun.dev/benchmark/fiction-livebench?show=all)
- Legacy Context Arena — [old.contextarena.ai](https://old.contextarena.ai)
- PromptZone degradation article (anecdote only) — [page](https://www.promptzone.com/mauricio_arellano/claude-opus-55-performance-degradation-reports-testing-and-alternatives-474e)
- dev.to/ofox and CodingFleet long-context roundups (pre-date the models) — [dev.to](https://dev.to/owen_fox/long-context-llm-benchmarks-2026-which-model-actually-holds-accuracy-past-200k-tokens-17ka), [CodingFleet](https://codingfleet.com/blog/context-window-lie-how-well-ai-models-use-1m-tokens-2026/)
- GitHub repo sachin00810/context-rot-benchmark (Gemini only) — [repo](https://github.com/sachin00810/context-rot-benchmark)

Could not open:
- Context Arena current site (JS app; API call blocked) — [contextarena.ai](https://contextarena.ai/)
- backlist.sdan.io mirror of @DillonUzar posts (host not allowed in this session)
- Fiction.liveBench mirror at datalearner.com (redirected to llmlearner.com; not followed, the llmrun.dev mirror was read instead)

### Inferences
- GitHub code search returned zero hits for `"claude-opus-5-5"` combined with each of: mrcr, nolima, graphwalks, longmemeval, context_length, "context rot", needle haystack; and zero for `"claude-fable-5-2" needle`, `"claude-sonnet-5-5" mrcr`, `"claude-haiku-5-5" needle`. A plain `"claude-opus-5-5"` search did return results, so the search itself was working. (Run via the gh CLI on 25 Nov 2026; no URL.)
- Web searches for Opus 5.5 / Fable 5.2 / Sonnet 5.5 / Haiku 5.5 with "context rot", MRCR, needle, NoLiMa, LongMemEval, graphwalks, Fiction.liveBench, and with vendor names (Cursor, Cognition, Factory, Amp, Letta, Zep, Supermemory, Morph) and analyst names (Simon Willison, Zvi, Latent Space) returned no context-length experiment. These vendor and newsletter sites were not opened individually.

### Gaps
- Not individually opened: the engineering blogs of Cursor, Cognition, Factory, Sourcegraph/Amp, Augment, Warp, Zed, Replit, Vercel, LangChain, LlamaIndex, Letta, Mem0, Zep, Supermemory, Relace, Greptile, CodeRabbit, Qodo, Continue, OpenHands, JetBrains Research, Braintrust, Arize, Galileo, Patronus, Baseten; Latent Space, Simon Willison, Zvi, Mollick, Pragmatic Engineer, Interconnects, Every, AI Explained; Hacker News launch threads; Hugging Face datasets/spaces. Search-only coverage found nothing, but an unindexed post could exist.
- No follow-ups were found to the Wmedia Haiku 5.5 cost test or Epoch's Vending-Bench review; neither was re-opened.
