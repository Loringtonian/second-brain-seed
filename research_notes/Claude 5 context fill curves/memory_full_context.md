# Memory / full-context evidence for Claude Opus 5.5, Sonnet 5.5 and Fable 5.2 (Mythos 5.2)

Research pass run 25 November 2026. Scope: only Opus 5.5, Sonnet 5.5, Fable 5.2 / Mythos 5.2.

**Headline: no source opened gives a same-task accuracy curve at two or more context/history sizes for a true "everything in the window" condition on a memory or conversational-history benchmark for any of the three models.** What exists is: (a) one Google-run synthetic length-binned result for Opus 5.5; (b) one single-conversation academic head-to-head of full context vs retrieval for Opus 5.5 at ~29K tokens; (c) vendor-run retrieval-only results for Sonnet 5.5 at three history sizes (the model never saw the full history); (d) nothing at all for Fable 5.2 / Mythos 5.2.

## Data tables (all rows opened and read unless marked UNVERIFIED)

### Table A. Same model, same task, two or more context sizes

| model | benchmark | context/history size or condition | score | n | who ran it | date | URL |
|---|---|---|---|---|---|---|---|
| Opus 5.5 | GraphWalks BFS (F1), everything in context | up to 128K tokens | 90.6% | 650 items | Google (self-computed for a competitor's model card; max/best reasoning) | results "as of October 2026" | https://storage.googleapis.com/deepmind-media/gemini/gemini_4_argon_model_evaluation.pdf |
| Opus 5.5 | GraphWalks BFS (F1), everything in context | 256K to 1M tokens | 66.8% | 200 problems | Google (same) | Oct 2026 | same PDF, results table p.5 |
| Sonnet 5.5 | BEAM 100K, **retrieval only** (supermemory feeds ~6.1K tokens; model never sees the full history) | history 116,199 tokens; 6,095 sent (5.2%) | 0.712 | 400 q / 20 conversations | supermemory (vendor-run) | undated page; listed as Nov 2026 runs | https://supermemory.ai/research/runs/beam-100k/ |
| Sonnet 5.5 | BEAM 500K, retrieval only | history 508,776 tokens; 6,096 sent (1.2%) | 0.692 | 700 q / 35 conversations | supermemory (vendor-run) | same | https://supermemory.ai/research/runs/beam-500k/ |
| Sonnet 5.5 | BEAM 1M, retrieval only | history 965,633 tokens; 6,090 sent (0.63%) | 0.689 | 700 q / 35 conversations | supermemory (vendor-run) | same | https://supermemory.ai/research/runs/beam-1m/ |
| Fable 5.2 / Mythos 5.2 | none found | - | - | - | - | - | - |

Notes on Table A: the Google rows are a synthetic graph-traversal task, not personal or conversational history, run by a competitor, with the long-bin problem selection not published. The supermemory rows show a retrieval pipeline holding roughly flat as the history grows 8x while the prompt stays ~6K tokens; they say nothing about how Sonnet 5.5 reads a full window.

### Table B. "Everything in context" versus "retrieve a subset"

| model | benchmark | context/history size or condition | score | n | who ran it | date | URL |
|---|---|---|---|---|---|---|---|
| Opus 5.5 (reasoning effort high) | LoCoMo, conv-26 only | Full context, ~28,900 input tokens/question, $0.117/question | 95% | 152 q, one run per cell | Zedong Peng, Shanghai Jiao Tong Univ. (academic; arXiv "MemoryTax") | 21 Oct 2026 | https://arxiv.org/html/2610.29823 |
| Opus 5.5 | same | BM25 retrieval, $0.013/question | 90% | 152 | same | same | same |
| Opus 5.5 | same | Mem0 OSS 2.1.0, $0.074/question + $25.876 build | 95% | 152 | same | same | same |
| Opus 5.5 | same | Zep (Graphiti OSS 0.30.2), $0.0077/question + $6.224 build | 87% | 152 | same | same | same |
| Opus 5.5 | same | Pi agent over files, ~10,000 input tokens/question (~7,900 cached), $0.046/question | 99% | 152 | same | same | same |
| Opus 5.5 | same | Codex agent, $0.167/question | 98% | 152 | same | same | same |
| Opus 5.5 | same | Claude Code agent, $0.187/question | 97% | 152 | same | same | same |
| Opus 5.5 | LongMemEval-S | Recallium memory, 3.8 results served on avg; "about 90% less than the full history"; 12.6K tokens/question (page does not tie this figure to a specific reader) | 98.4% | 500 | Recallium (vendor-run) | 30 Sep 2026 | https://recallium.ai/benchmarks |
| Opus 5.5 | LongMemEval-S | No-memory control (not a full-history run) | 6.6% of answerable questions | 470 answerable | Recallium (vendor-run) | 30 Sep 2026 | same |
| Sonnet 5.5 | LongMemEval-S | supermemory retrieval: 5,012 tokens sent of 103,437-token history (4.8%) | 94.0% | 500 | supermemory (vendor-run) | undated; Nov 2026 runs | https://supermemory.ai/research/runs/longmemeval/ |
| Sonnet 5.5 | LoCoMo | supermemory retrieval: 4,760 tokens sent of 20,082-token conversation (24%) | 93.0% | 1,540 | supermemory (vendor-run) | undated; Nov 2026 runs | https://supermemory.ai/research/runs/locomo/ |
| Sonnet 5.5 | LoCoMo | uteke end-to-end (condition not stated in PR; Sonnet 5.5 is both answerer and judge) | 72.2% (1,112/1,540); 80.4% under a Haiku 4.5 judge | 1,540 | uteke open-source project | 9 Nov 2026 | https://github.com/codecoradev/uteke/pull/1697 |
| Fable 5.2 / Mythos 5.2 | none found | - | - | - | - | - | - |

Per-category detail for the Opus 5.5 MemoryTax rows (single / multi / temporal / open / all): Full context 93/94/97/100/95; BM25 93/81/92/92/90; Mem0 93/97/97/100/95; Zep 83/94/89/85/87; Pi 99/100/100/100/99; Codex 99/100/95/100/98; Claude Code 99/94/97/100/97. Judge GPT-4o-mini with Mem0's 2025 rubric. Rollout totals: full context $17.84, Pi $6.95, Codex $25.41, Claude Code $28.41.

**The full-context baselines printed next to the Sonnet 5.5 vendor numbers are NOT Sonnet 5.5.** They are older published GPT-4o / GPT-4o-mini / Haiku 4.5 figures: LongMemEval-S full context 60.2% (GPT-4o, Zep paper) or 64.0% (GPT-4o, LongMemEval paper); LoCoMo full context 72.9% (GPT-4o-mini, Mem0 paper); BEAM 100K whole conversation 0.533 (Claude Haiku 4.5, Honcho run). Do not read these as a Sonnet 5.5 full-context score.

### Table C. Single-size long-document or adjacent results (no length bins)

| model | benchmark | context/history size or condition | score | n | who ran it | date | URL |
|---|---|---|---|---|---|---|---|
| Sonnet 5.5 | AA-LCR v1.1 (documents 10K to 100K tokens, no per-length split) | single condition | 82.7% | 100 q | Artificial Analysis, as relayed by BenchLM (aggregator); AA's own page did not expose the number in fetched text | Nov 2026 | https://benchlm.ai/models/claude-sonnet-5-5 |
| Opus 5.5 | AA-LCR v1.1 | single condition, "max effort" | 85% | 100 q | UNVERIFIED (search snippet of a third-party guide only) | - | https://artificialanalysis.ai/evaluations/artificial-analysis-long-context-reasoning (benchmark description opened; score not shown) |
| Opus 5.5 | Vectara hallucination leaderboard (short-document summarisation; no document-length breakdown in the PR) | single condition | 9.9% hallucination rate | not stated | Vectara | 22-23 Oct 2026 | https://github.com/vectara/hallucination-leaderboard/pull/192 |
| Opus 5.5 | CL-Bench database task: stateless vs file memory across sessions (agent memory, not context length) | stateless 0.655 / with memory 0.838 | mean reward | n not stated | Anthropic (vendor-run on own model) | 6 Nov 2026 | https://www.anthropic.com/research/components-of-agent-memory |
| Sonnet 5.5 | same | stateless 0.644 / with memory 0.833 | mean reward | n not stated | Anthropic | 6 Nov 2026 | same |

## For each of the three models: any accuracy figures at two or more context/history sizes on the same task?

### Takeaway
Only Opus 5.5 has a genuine two-point length curve with the material in the window, and it is a competitor-run synthetic graph task: 90.6% up to 128K falling to 66.8% at 256K-1M. Sonnet 5.5 has three history sizes only under a retrieval system that keeps the prompt at ~6K tokens; Fable 5.2 / Mythos 5.2 has nothing.

### Cited Findings
- Google's Gemini 4 Argon evaluation PDF reports GraphWalks BFS F1 for Claude Opus 5.5 of 90.6% ("Up to 128k") and 66.8% ("256k to 1M"). In the same table Gemini 4 Argon is 99.7% / 84.2%, GPT-6 Astra 98.7% / 71.8%, Claude Fable 5.1 91.4% / 65.0% (Fable 5.1 is out of scope; shown only to make clear the Fable row is 5.1, not 5.2). I read the results table directly from the rendered PDF page. — [Google, Gemini 4 Argon Model evaluation](https://storage.googleapis.com/deepmind-media/gemini/gemini_4_argon_model_evaluation.pdf)
- Methodology in the same PDF: "Graphwalks results for all models are self-computed. The 'Up to 128k' subset uses the 650 items with context length up to 128k tokens. The '256 to 1M' subset uses the 200 GraphWalks problems with context lengths between 256k and 1M tokens." Non-Gemini models default to "maximum thinking/reasoning settings available." — [Google, Gemini 4 Argon Model evaluation](https://storage.googleapis.com/deepmind-media/gemini/gemini_4_argon_model_evaluation.pdf)
- A benchmark tracker states that Anthropic's own Opus 5.5 system card reports no GraphWalks result, so Google's run is the only view; it lists no GraphWalks result for Sonnet 5.5, Fable 5.2 or Mythos 5.2. — [Klu, GraphWalks](https://klu.ai/benchmarks/graphwalks)
- DataCamp (19 Nov 2026) repeats the 66.8% long-bin figure for Opus 5.5 and attributes it to Google's model card. — [DataCamp](https://www.datacamp.com/blog/gemini-4-neon-vs-claude-opus-5-5)
- supermemory with Claude Sonnet 5.5 as the answering model scores 0.712 on BEAM 100K (n=400; 6,095 of 116,199 tokens sent), 0.692 on BEAM 500K (n=700; 6,096 of 508,776 tokens), 0.689 on BEAM 1M (n=700; 6,090 of 965,633 tokens). Judge gpt-4.1-mini with the BEAM authors' rubric; answers "from the search results only." Vendor-run. — [BEAM 100K](https://supermemory.ai/research/runs/beam-100k/); [BEAM 500K](https://supermemory.ai/research/runs/beam-500k/); [BEAM 1M](https://supermemory.ai/research/runs/beam-1m/)
- Per-category movement for Sonnet 5.5 in those runs, 100K / 500K / 1M: information extraction 0.956 / 0.810 / 0.857; abstention 1.000 / 0.914 / 0.857; knowledge update 0.800 / 0.700 / 0.757; contradiction resolution 0.569 / 0.475 / 0.448; event ordering 0.207 / 0.243 / 0.227; temporal reasoning 0.500 / 0.537 / 0.443; summarization 0.581 / 0.679 / 0.688; multi-session reasoning 0.694 / 0.705 / 0.735; instruction following 0.875 / 0.879 / 0.931; preference following 0.938 / 0.980 / 0.942. — same three supermemory pages
- The supermemory BEAM 1M page reports no whole-conversation run for Sonnet 5.5 or any other model; the 100K page's only whole-conversation row is Claude Haiku 4.5 at 0.533. — [BEAM 1M](https://supermemory.ai/research/runs/beam-1m/); [BEAM 100K](https://supermemory.ai/research/runs/beam-100k/)
- DataCamp (10 Nov 2026): "OpenAI has published long-context retrieval results for Astra; Anthropic hasn't for Fable 5.2." — [DataCamp, Fable 5.2 vs GPT-6 Astra](https://www.datacamp.com/blog/claude-fable-5-2-vs-gpt-6-astra)
- llm-stats lists Mythos 5.2 as released 9 Nov 2026 with a 1M input window, and shows no benchmark scores of any kind (no long-context, memory or document-QA rows). — [llm-stats, Claude Mythos 5.2](https://llm-stats.com/models/claude-mythos-5-2)
- MRCR v2 8-needle 1M and GraphWalks 256K-1M aggregator leaderboards list none of the four model names. — [aireleasetracker MRCR v2 1M](https://aireleasetracker.com/benchmark/mrcr-v2-1m); [BenchGecko GraphWalks BFS 256K-1M](https://benchgecko.ai/benchmark/graphwalks-bfs-256k)

### Inferences
- The Opus 5.5 GraphWalks drop (about 24 points from the ≤128K bin to the 256K-1M bin) is the only measured sign of length degradation for any of the three models found in this pass. It is a multi-hop traversal over synthetic graph edges, which is harder than and different from question-answering over personal notes; treat it as an upper-ish bound on how much a hard reasoning-over-whole-context task can lose, not as a prediction for a second brain.
- The supermemory Sonnet 5.5 series (0.712 → 0.692 → 0.689 while history grows from 116K to 966K) is evidence that a retrieve-6K-tokens design is nearly insensitive to corpus size on this benchmark. It is not evidence about Sonnet 5.5's own long-window behaviour, and it comes from the vendor selling the retrieval system.
- Because Fable 5.2 shares a model with Mythos 5.2 and both were released 9 Nov 2026, sixteen days before this pass, the absence of third-party long-context or memory numbers is most likely a recency gap, not a signal either way.

### Gaps
- No full-context (whole history in window) score for Sonnet 5.5 on LongMemEval, LoCoMo or BEAM at any size was found.
- No full-context score for Opus 5.5 above ~29K tokens on a conversational-memory benchmark was found.
- Nothing for Fable 5.2 / Mythos 5.2 on any memory, long-context or document-QA benchmark.
- Context Arena (the MRCR-by-length leaderboard) is rendered client-side; the fetched page contained no data, so I could not confirm whether it lists these models. A targeted search for the three model IDs on it returned nothing.
- I could not open the Anthropic Opus 5.5 / Sonnet 5.5 / Fable 5.2 system cards in this pass; secondary sources disagree on whether the Opus 5.5 card carries a numeric MRCR result (one aggregator says "published qualitatively without a numeric variant-level score"; UNVERIFIED, search snippet of https://armes.ai/docs/models/anthropic/claude-opus-5.5).
- "ProgramBench (long context, up to 1M tokens)": Opus 5.5 91.2%, Sonnet 5.5 79.7% appear in third-party blogs and BenchLM attributes the Sonnet figure to the Sonnet 5.5 system card under coding benchmarks. Single size, not confirmed to be a long-context measure. UNVERIFIED at source.

## Any head-to-head of "everything in context" versus "retrieve a small relevant subset" for these models, with accuracy and token cost?

### Takeaway
One, for Opus 5.5 only, at small scale: on a single ~29K-token LoCoMo conversation, full context scored 95%, tied with Mem0 and ahead of BM25 (90%) and Zep (87%), but below an agent searching files (97-99%), at about 9x the per-question cost of BM25. No such head-to-head exists for Sonnet 5.5 or Fable 5.2, and none for any of the three at 100K+ tokens.

### Cited Findings
- "MemoryTax: The Bitter Lesson of Agent Memory" (Zedong Peng, SJTU; arXiv 2610.29823v1, 21 Oct 2026) runs LoCoMo conv-26 only (419 turns, 19 sessions, 152 questions; adversarial excluded), each cell once, GPT-4o-mini judge. With Claude Opus 5.5 (reasoning effort high) as reader: Full context 95%, BM25 90%, Mem0 95%, Zep 87%, Pi 99%, Codex 98%, Claude Code 97%. — [arXiv 2610.29823](https://arxiv.org/html/2610.29823)
- Costs with Opus 5.5 in that paper: full context $0.117/question at about 28,900 input tokens; BM25 $0.013; Mem0 $0.074 plus $25.876 build; Zep $0.0077 plus $6.224 build; Pi $0.046 at about 10,000 input tokens (about 7,900 cached); Codex $0.167; Claude Code $0.187. — [arXiv 2610.29823](https://arxiv.org/html/2610.29823)
- Same paper, weaker reader for contrast (not in scope, context only): with GPT-6 Luna, full context 92%, BM25 82%, Mem0 80%, Zep 51%. UNVERIFIED beyond search snippet for this specific row. — [arXiv 2610.29823](https://arxiv.org/html/2610.29823)
- Recallium (vendor) reports Claude Opus 5.5 as reader at 98.4% on LongMemEval-S (n=500, GPT-4o judge, 30 Sep 2026), with the reader "given about 90% less than the full history"; misses were 8 of 500 (3 retrieval, 3 reader, 2 answered when it should have declined). The page's full-history baselines are GPT-4o only (60.6 / 64.0 / 60.2), with no Opus 5.5 full-history run. — [Recallium benchmarks](https://recallium.ai/benchmarks)
- supermemory (vendor) reports Sonnet 5.5 at 94.0% on LongMemEval-S with 5,012 of 103,437 tokens sent, and 93.0% on LoCoMo with 4,760 of 20,082 tokens sent; its full-context comparison rows are published GPT-4o and GPT-4o-mini numbers, stated on the page as "other parties' own published figures, not runs under our setup." Prompts were tuned on 31 LongMemEval questions (94.2% without them) and 152 LoCoMo questions (92.6% without). — [LongMemEval run](https://supermemory.ai/research/runs/longmemeval/); [LoCoMo run](https://supermemory.ai/research/runs/locomo/)
- Anthropic's own agent-memory research (6 Nov 2026) compares stateless vs persistent file memory, not full context vs retrieval: Opus 5.5 0.655 → 0.838, Sonnet 5.5 0.644 → 0.833 on a CL-Bench database task. On a separate company-document corpus (25 questions, model not tied to 5.5 in the extract), wiki plus search scored 61%, search alone 53%, wiki alone 48%, and "Using search and the wiki also saved about 20,000 tokens per question." The post also states "Injected memories use up valuable tokens" and that Claude Code's auto memory loads only the first 200 lines or 25 KB of MEMORY.md. — [Anthropic, The components of agent memory](https://www.anthropic.com/research/components-of-agent-memory)
- Price arithmetic only (no accuracy measured): on Sonnet 5.5 list prices checked 1 Oct 2026, a 400K-token corpus costs about $0.805 per answer uncached, about $0.085 cached, versus about $0.017 for a 6K-token RAG prompt. — [beri.net](https://www.beri.net/article/long-context-vs-rag-cost-prompt-caching-crossover-2026)

### Inferences
- At ~29K tokens Opus 5.5 does not show a full-context penalty: stuffing the whole conversation ties the best memory product and beats two retrieval pipelines. The retrieval systems lost accuracy by dropping evidence, not because the reader failed. This supports "small corpora: just load it" for Opus 5.5, on n=152 from one conversation.
- In the same study the best result came from letting the model search files agentically (Pi 99% at roughly a third of the full-context cost), which is the pattern a second brain's router-plus-on-demand-reads approximates.
- For 100K-1M histories the only numbers are vendor retrieval runs with no matched full-context arm for the same model, so the crossover point where retrieval starts to beat full context for these models is unmeasured.

### Gaps
- No matched full-context vs retrieval run for Sonnet 5.5 or Fable 5.2 / Mythos 5.2 at any size.
- No matched run for Opus 5.5 beyond one 29K-token conversation; no confidence intervals (each cell run once).
- MemoryTax does not publish per-query token counts for the Mem0, Zep and BM25 arms under Opus 5.5 (only cost), so token cost comparison rests on dollars.
- The uteke LoCoMo figure for Sonnet 5.5 (72.2%) does not state whether it is a memory or full-context condition, and the judge swap alone moved it 8.2 points, so it cannot be used as a baseline.

## Any length-binned document-QA results for these models?

### Takeaway
None found. The only length-binned result is the synthetic GraphWalks pair for Opus 5.5; the document-QA benchmarks that list these models (AA-LCR) report a single score with no length split, and LongBench v2 leaderboards do not list them at all.

### Cited Findings
- AA-LCR v1.1 is 100 questions over document sets of 10K to 100K tokens (cl100k_base), pass/fail graded by an LLM; the page gives no per-length breakdown and did not expose Claude 5.5 scores in fetched text. — [Artificial Analysis, AA-LCR](https://artificialanalysis.ai/evaluations/artificial-analysis-long-context-reasoning)
- BenchLM lists Claude Sonnet 5.5 at 82.7% on AA-LCR, labelled an independent Artificial Analysis run; it has no MRCR-by-bin, GraphWalks or LongBench v2 rows for Sonnet 5.5. — [BenchLM, Claude Sonnet 5.5](https://benchlm.ai/models/claude-sonnet-5-5)
- BenchLM's LongBench v2 leaderboard (25 models, Nov 2026) lists none of the three models; the only Claude entry is Opus 4.5, and there is no short/medium/long split. — [BenchLM, LongBench v2](https://benchlm.ai/benchmarks/longbench-v2)
- Artificial Analysis's Fable 5.2 model page shows no AA-LCR score; it gives release 9 Nov 2026, 1M context, Intelligence Index 65 (Max) vs 59 for Opus 5.5 and 55 for Sonnet 5.5. — [Artificial Analysis, Claude Fable 5.2](https://artificialanalysis.ai/models/claude-fable-5-2)
- Vectara's leaderboard added claude-opus-5-5 at 9.9% hallucination rate on 22 Oct 2026; the PR has no document-length breakdown and does not add Sonnet 5.5, Fable or Mythos. — [vectara/hallucination-leaderboard PR 192](https://github.com/vectara/hallucination-leaderboard/pull/192)

### Inferences
- AA-LCR tops out at ~100K tokens, a tenth of these models' window, so even its single score says nothing about the 200K-1M range a large second brain would occupy.

### Gaps
- Loong, DocFinQA, FinanceBench long-context, NovelQA and legal/contract benchmarks: searches surfaced no entries naming these three models; I did not open each leaderboard individually.
- Opus 5.5 AA-LCR (85%) and Sonnet 5.5 (83%) from blog tables are UNVERIFIED at Artificial Analysis itself (Kaggle and AA pages list the models but returned no numbers in fetched text).
- No Chroma, Pinecone, Weaviate, Voyage, Cohere, Jina or Contextual AI study from Sept-Nov 2026 naming these models was found; Chroma's Context Rot report is July 2025.

## For each source opened, does it list these models?

### Takeaway
Of roughly forty sources opened, eight carry a number for Opus 5.5 or Sonnet 5.5 and none carries one for Fable 5.2 / Mythos 5.2; most memory-vendor pages still benchmark with GPT-4o-class or Haiku 4.5 readers.

### Cited Findings
Sources that DO list a target model with numbers:
- Opus 5.5, GraphWalks two bins, Google-run. — [Gemini 4 Argon evaluation PDF](https://storage.googleapis.com/deepmind-media/gemini/gemini_4_argon_model_evaluation.pdf); relayed by [Klu](https://klu.ai/benchmarks/graphwalks) and [DataCamp](https://www.datacamp.com/blog/gemini-4-neon-vs-claude-opus-5-5)
- Opus 5.5, LoCoMo conv-26 full context vs retrieval vs agents. — [MemoryTax, arXiv 2610.29823](https://arxiv.org/html/2610.29823)
- Opus 5.5 as reader over a memory system, LongMemEval-S. — [Recallium](https://recallium.ai/benchmarks)
- Sonnet 5.5 as reader over a memory system: LongMemEval-S, LoCoMo, BEAM 100K/500K/1M. — [supermemory research](https://supermemory.ai/research/) and its run pages
- Sonnet 5.5 LoCoMo end-to-end, judge-sensitive. — [uteke PR 1697](https://github.com/codecoradev/uteke/pull/1697)
- Opus 5.5 and Sonnet 5.5, stateless vs agent memory. — [Anthropic research](https://www.anthropic.com/research/components-of-agent-memory)
- Opus 5.5 hallucination rate. — [Vectara PR 192](https://github.com/vectara/hallucination-leaderboard/pull/192)
- Sonnet 5.5 AA-LCR. — [BenchLM](https://benchlm.ai/models/claude-sonnet-5-5)

Sources that mention a target model but give no usable length/memory number:
- Sonnet 5.5 used only to audit LoCoMo's answer key (1,344 correct / 88 minor / 56 ambiguous / 52 wrong of 1,540 references), not as a reader. Full-context baselines on the page are gpt-4o-mini and gemini-3.1-flash-lite. — [braindetox.kr, 17 Oct 2026](https://braindetox.kr/en/posts/memory_benchmark_scores_reliability_2026.html)
- Sonnet 5.5 used only as a second judge; headline reader resolved to claude-opus-5 (not 5.5); no full-history condition. — [arXiv 2609.38021v2](https://arxiv.org/html/2609.38021v2)
- Opus 5.5 used only for curation/capability labelling and cited in references; evaluation backbones are Qwen-3.8 27B and GPT-6 Luna. — [Twists and Turns, arXiv 2611.06414](https://arxiv.org/pdf/2611.06414)
- Specs only, no long-context numbers: [llm-stats Mythos 5.2](https://llm-stats.com/models/claude-mythos-5-2); [Artificial Analysis Fable 5.2](https://artificialanalysis.ai/models/claude-fable-5-2); [DataCamp Fable 5.2 vs Astra](https://www.datacamp.com/blog/claude-fable-5-2-vs-gpt-6-astra); [apiyi comparison, 30 Sep 2026](https://help.apiyi.com/en/claude-sonnet-5-5-vs-claude-opus-5-5-comparison-en.html)
- Price arithmetic for Sonnet 5.5, no accuracy. — [beri.net](https://www.beri.net/article/long-context-vs-rag-cost-prompt-caching-crossover-2026)

Sources opened that do NOT list any of the three models:
- "You don't need long context" (arXiv 2611.05306, NVIDIA authors, 3 Nov 2026): full text searched; models are Claude Opus 4.5, GPT 5.6, Qwen 3.6, Kimi K2.6, GLM 5.2, MiniMax M2.7. No Opus 5.5, Sonnet 5.5, Fable or Mythos. — [arXiv 2611.05306](https://arxiv.org/abs/2611.05306)
- Longitudinal clinical reasoning context strategies (arXiv 2610.00562): full text searched; open-weight models only (GLM-4, Qwen, Llama). — [arXiv 2610.00562](https://arxiv.org/pdf/2610.00562)
- LongHarness (arXiv 2610.13075): full text searched; none of the target names. — [arXiv 2610.13075](https://arxiv.org/pdf/2610.13075)
- MemDelta (arXiv 2606.29914, June 2026, predates the models): "Sonnet" unversioned. — [arXiv 2606.29914](https://arxiv.org/abs/2606.29914)
- Honcho evals: baseline is Claude Haiku 4.5 full context (LongMem S 62.6%, LoCoMo 75.6%, BEAM 100K 0.533; no baseline at 500K+). — [Honcho evals](https://honcho.dev/evals/)
- Mem0 benchmark overview (23 Nov 2026): none of the target names. — [Mem0 blog](https://mem0.ai/blog/ai-memory-benchmarks-in-2026)
- past.dev BEAM leaderboard: readers are gpt-5.6-luna or unstated; no full-context rows. — [past.dev](https://past.dev/benchmarks/beam)
- Severian OmniMemEval (Oct 2026): reader gpt-4.1-mini; "full text (replicate)" arms use Qwen3.8-27B. — [Severian](https://github.com/Sahil-SS9/Severian/blob/main/docs/benchmarks/omnimemeval-2026-10.md)
- Eidentic LongMemEval post (June 2026): model unnamed. — [dev.to](https://dev.to/eidentic/memory-beats-full-context-on-longmemeval-and-the-wins-we-dont-get-303c)
- Boolean "Context Rot Quantified" (15 Sep 2026): Claude Opus 5 and GPT-5.6 Sol only. — [Boolean](https://www.boolean.ai/blog/context-rot-quantified)
- LongBench v2 leaderboard, MRCR v2 1M and GraphWalks 256K-1M aggregators. — [BenchLM](https://benchlm.ai/benchmarks/longbench-v2); [aireleasetracker](https://aireleasetracker.com/benchmark/mrcr-v2-1m); [BenchGecko](https://benchgecko.ai/benchmark/graphwalks-bfs-256k)
- AA-LCR evaluation page: benchmark described, no Claude 5.5 / 5.2 scores in fetched text. — [Artificial Analysis](https://artificialanalysis.ai/evaluations/artificial-analysis-long-context-reasoning)
- Could not be read (client-rendered, empty fetch): [Context Arena](https://contextarena.ai/); [Agent Memory Benchmark](https://agentmemorybenchmark.ai/dataset/beam). The Kaggle AA-LCR page names all four models but returned no scores: [Kaggle AA-LCR](https://www.kaggle.com/benchmarks/artificial-analysis/aa-lcr).

### Inferences
- The memory-benchmark ecosystem lags model releases: vendors report their own system with a new reader but reuse year-old GPT-4o full-context baselines. The result is that the published "memory beats full context" gaps for Sonnet 5.5 (for example 94.0% vs 60.2% on LongMemEval-S) compare a 2026 reader against a 2024 reader and overstate the benefit attributable to retrieval.
- The one study that held the reader fixed (MemoryTax, Opus 5.5) found no full-context penalty at ~29K tokens, which is the opposite direction from the vendor charts.

### Gaps
- Zep/Graphiti, Letta, Cognee, LangMem, Memobase, Hindsight, Mastra, EverMind, Papr, MemPalace: searches for these vendors combined with the three model names returned no posts or repos naming the models; I did not open each vendor's site individually beyond those listed.
- LongMemEval-V2, MemoryAgentBench, MemBench, PersonaMem, DMR, LoCoMo-Plus, MSC, ConvoMem: no leaderboard entry naming these models surfaced in search; leaderboards not individually opened.
- Hugging Face spaces for long-context or memory evaluation: no entries naming these models surfaced.
- Anthropic system cards for the three models not opened.
