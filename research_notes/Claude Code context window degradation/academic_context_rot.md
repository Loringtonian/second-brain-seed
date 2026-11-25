# Academic and lab research on long-context degradation ("context rot"), as of 2026-11-25

Reading notes for the report writer:
- **Verification labels.** "PRIMARY" = I read the original source. "SECONDARY" = the number reached me through an aggregator or blog that attributes it to a primary source I did not open (most Anthropic system-card figures below are in this class; the cards are PDFs I did not fetch). Treat SECONDARY numbers as needing a check against the card before they are quoted as fact.
- **Model generations.** 2023 = Claude 1.3/2.x, GPT-3.5/4. 2024 = Claude 3/3.5, GPT-4o, Gemini 1.5. 2025 = Claude 3.7/4/4.5, GPT-4.1/o3/GPT-5, Gemini 2.5/3 Pro. 2026 = Claude Opus 4.6/4.7/4.8, Mythos 5, Fable 5.x, Opus 5.5; GPT-5.4/5.5/GPT-6; Gemini 3.1/4.
- Scores from different labs are not comparable with each other (different thinking settings, prompts, graders, token-range binning). This caveat is stated by the sources themselves.

## 1. What did Chroma's "Context Rot" study (2025) find, specifically for Claude models vs. others? Any follow-ups in 2026?

### Takeaway
Chroma (July 2025; Claude 3.5/3.7/4 generation) found every one of 18 models gets less reliable as input grows even on trivial tasks, and that Claude's distinctive failure mode is abstention rather than hallucination: lowest hallucination rates, but the largest focused-vs-full gap on LongMemEval. Chroma published few headline numbers (results are mostly charts), and I found no Chroma follow-up re-running the study on 2026 models; 2026 follow-ups come from other groups.

### Cited Findings
- PRIMARY. "Context Rot: How Increasing Input Tokens Impacts LLM Performance", Kelly Hong, Anton Troynikov, Jeff Huber, Chroma technical report, published 2025-07-14. — [Chroma](https://www.trychroma.com/research/context-rot)
- PRIMARY. 18 models: Claude Opus 4, Sonnet 4, Sonnet 3.7, Sonnet 3.5, Haiku 3.5; o3, GPT-4.1, 4.1 mini, 4.1 nano, GPT-4o, GPT-4 Turbo, GPT-3.5 Turbo; Gemini 2.5 Pro, 2.5 Flash, 2.0 Flash; Qwen3-235B-A22B, Qwen3-32B, Qwen3-8B. No Claude 4.5 or later, no GPT-5, no Gemini 3. — [Chroma](https://www.trychroma.com/research/context-rot)
- PRIMARY. Design: each needle/haystack combination tested at 8 input lengths and 11 needle positions, temperature 0, 194,480 LLM calls in total, GPT-4.1 as judge (>0.99 alignment with human labels). — [Chroma](https://www.trychroma.com/research/context-rot)
- PRIMARY. Needle-question similarity: performance degrades faster with input length when the needle is semantically less similar to the question. Needle position showed no notable effect in this experiment. — [Chroma](https://www.trychroma.com/research/context-rot)
- PRIMARY. Distractors: one distractor lowers performance against the needle-only baseline; four lower it further; the effect grows with input length. Claude models show the lowest hallucination rates; Sonnet 4 and Opus 4 are especially conservative and often abstain ("no answer can be found"). GPT models show the highest hallucination rates (confident wrong answers). — [Chroma](https://www.trychroma.com/research/context-rot)
- PRIMARY. Haystack structure: shuffled (incoherent) haystacks gave better retrieval than logically coherent ones across all 18 models. The authors suggest an attention-related cause but did not test it. — [Chroma](https://www.trychroma.com/research/context-rot)
- PRIMARY. LongMemEval, 306 prompts: full prompts average about 113K tokens, focused prompts about 300 tokens. All models scored significantly higher on focused prompts. Claude models showed the largest focused-vs-full gap, mostly from abstentions under ambiguity; Opus 4 and Sonnet 4 scored lower on full prompts than older Claude models. Thinking modes improved both conditions but a gap remained. — [Chroma](https://www.trychroma.com/research/context-rot)
- PRIMARY. Repeated-words task (25 to 10,000 words, 1,090 variations): all models degrade with length. Claude Opus 4 had the slowest degradation rate but refused in 2.89% of attempts (the only Claude model to refuse), typically from about 2,500 words. Claude Sonnet 3.5 outperformed newer Claude models up to its 8,192-token output cap. GPT-4.1 refused about 2.55%; Gemini models emitted random words from about 500 to 750 words. — [Chroma](https://www.trychroma.com/research/context-rot)
- PRIMARY. Chroma offers no tested mechanism; code is public. — [Chroma](https://www.trychroma.com/research/context-rot); [GitHub](https://github.com/chroma-core/context-rot)
- 2026 follow-ups by other groups that adopt the "context rot" term:
  - PRIMARY (abstract only). "Classifier Context Rot: Monitor Performance Degrades with Context Length", Sam Martin and Fabien Roger, arXiv 2605.12366, 2026-05-12. Opus 4.6, GPT 5.4 and Gemini 3.1 used as monitors miss a subtly dangerous coding-agent action 2x to 30x more often when it occurs after 800K tokens of benign activity than when shown alone. Periodic reminders partially mitigate. — [arXiv 2605.12366](https://arxiv.org/abs/2605.12366)
  - Search-result level only. "Diagnosing and Mitigating Context Rot in Long-horizon Search", Xia et al., arXiv 2606.29718 (v1 2026-06-30, v2 2026-08-05): four open-source models; long context causes "premature termination" (giving up or answering uncertainly early), and termination rate correlates positively with context length. — [arXiv 2606.29718](https://arxiv.org/abs/2606.29718)
  - PRIMARY. "Length Is Cheap, Competition Is Not: Attention Predicts Context Rot in Retrieval", Yinuo Liu (UCSD), arXiv 2611.04169, 2026-11-03. See section 3. — [arXiv 2611.04169](https://arxiv.org/html/2611.04169v1)

### Inferences
- Chroma's Claude finding and the 2026 Xia et al. "premature termination" finding describe the same behaviour from two angles: under long, ambiguous context, models decline or stop rather than answer wrongly. For Claude this trades hallucination for abstention, which scores as failure on recall benchmarks. The same explanation is offered by a secondary source for Opus 4.7's MRCR drop (section 2), but I could not trace that to an Anthropic statement.
- Chroma's Claude results apply to Claude 3.5 through Opus 4 at up to 200K windows. They should not be quoted as describing Claude 4.6 or later.

### Gaps
- Chroma reports results as charts; I could not extract per-model accuracy at specific token counts. A secondary blog's claim that Claude 3.5 Sonnet's errors grow past about 8,000 words is unverified ([Substack](https://cobusgreyling.substack.com/p/llm-context-rot)).
- No Chroma re-run on 2026 models was found.
- Martin and Roger: per-model miss rates and intermediate lengths are not in the abstract; the full paper was not read.

## 2. What do NoLiMa, LongMemEval, Lost in the Middle, BABILong, LongBench v2, Michelangelo/LSQ, Fiction.LiveBench, MRCR, GraphWalks and similar show about degradation curves versus context length?

### Takeaway
2023 to 2025 benchmarks show steep curves at small absolute lengths (NoLiMa: Claude 3.5 Sonnet falls from 87.5 to 29.8 by 32K tokens). 2026 lab-reported MRCR v2 and GraphWalks show the curve pushed outward by roughly an order of magnitude but still present: every frontier model with published numbers scores materially lower at 1M than at 256K.

### Cited Findings

**Lost in the Middle (2023; GPT-3.5, Claude 1.3)**
- Search-result level. Liu et al., arXiv 2307.03172 (TACL 2024): multi-document QA accuracy is U-shaped in the position of the answer, dipping by 20+ percentage points when the answer sits mid-context; adding more retrieved documents gained Claude 1.3 only about 1%. — [arXiv 2307.03172](https://arxiv.org/pdf/2307.03172); [TACL](https://aclanthology.org/2024.tacl-1.9.pdf)

**BABILong (2024; GPT-4 generation)**
- Search-result level. Kuratov et al., arXiv 2406.10149 (NeurIPS 2024): "popular LLMs effectively utilize only 10-20% of the context" and performance declines sharply as reasoning complexity rises. — [arXiv 2406.10149](https://arxiv.org/abs/2406.10149)

**Michelangelo / Latent Structure Queries (2024-09; Claude 3/3.5, GPT-4o, Gemini 1.5)**
- Search-result level. Google DeepMind, arXiv 2409.12640: retrieving facts does not imply being able to synthesise over the full context. Tasks: Latent List, MRCR, IDK. Claude 3.5 Sonnet and Claude 3 Opus have "strikingly parallel" MRCR curves. A secondary summary says Claude 3.5 Sonnet scored highest on IDK, Gemini led MRCR, GPT led Latent List. — [arXiv 2409.12640](https://arxiv.org/html/2409.12640v2); [DeepMind](https://deepmind.google/research/publications/michelangelo-long-context-evaluations-beyond-haystacks-via-latent-structure-queries/); [TechTalks](https://bdtechtalks.substack.com/p/testing-long-context-llms-beyond)

**NoLiMa (2025-02, v3 2025-07-09, ICML 2025; Claude 3.5 Sonnet generation)** PRIMARY
- Needle-in-haystack with no literal keyword overlap between question and needle. "Effective length" = longest context at which the model keeps at least 85% of its short-context base score. — [arXiv 2502.05167v3](https://arxiv.org/html/2502.05167v3)
- Accuracy (%) by context length:

| Model | Claimed | Effective | Base | 1K | 2K | 4K | 8K | 16K | 32K |
|---|---|---|---|---|---|---|---|---|---|
| GPT-4o | 128K | 8K | 99.3 | 98.1 | 98.0 | 95.7 | 89.2 | 81.6 | 69.7 |
| Claude 3.5 Sonnet | 200K | 4K | 87.5 | 85.4 | 84.0 | 77.6 | 61.7 | 45.7 | 29.8 |
| Gemini 1.5 Pro | 2M | 2K | 92.6 | 86.4 | 82.7 | 75.4 | 63.9 | 55.5 | 48.2 |
| Gemini 2.0 Flash | 1M | 4K | 89.4 | 87.7 | 87.5 | 77.9 | 64.7 | 48.2 | 41.0 |
| Llama 3.3 70B | 128K | 2K | 97.3 | 94.2 | 87.4 | 81.5 | 72.1 | 59.5 | 42.7 |
| GPT-4o mini | 128K | <1K | 84.8 | 67.7 | 58.2 | 44.2 | 32.6 | 20.6 | 13.7 |

  — [arXiv 2502.05167v3](https://arxiv.org/html/2502.05167v3)
- At 32K tokens, 11 of 13 models score below half of their short-context baseline. — [arXiv 2502.05167v3](https://arxiv.org/html/2502.05167v3)
- Later additions (Appendix E): GPT-4.1 (1M claimed) effective length 16K; base 97.0, 84.9 at 16K, 79.8 at 32K, 69.7 at 64K, 64.7 at 128K. Gemini 2.5 Flash effective length 2K; 48.4 at 32K. — [arXiv 2502.05167v3](https://arxiv.org/html/2502.05167v3)
- Reasoning models on NoLiMa-Hard: o1 base 99.9, then 92.0 (4K), 78.0 (8K), 60.1 (16K), 31.1 (32K). o3-mini base 98.8, then 52.8 (4K), 18.9 (32K). — [arXiv 2502.05167v3](https://arxiv.org/html/2502.05167v3)

**LongMemEval (2025 use in Chroma; Claude 3.5 to Opus 4)**
- PRIMARY. About 113K-token full history vs about 300-token focused excerpt: all models significantly better focused; Claude shows the largest gap (abstention). No per-model numbers extracted. — [Chroma](https://www.trychroma.com/research/context-rot)

**LongBench v2 and LongBench Pro (2024-12 and 2026-01)** SECONDARY
- LongBench v2 (503 questions): Gemini 2.5 Pro 63.3%, human baseline 53.7%, GPT-4o 46.0%, Claude 3.5 Sonnet 41.0%. — [yage.ai survey, 2026-03-15](https://yage.ai/share/long-context-benchmark-en-20260315.html); paper at [arXiv 2412.15204](https://arxiv.org/pdf/2412.15204)
- LongBench Pro (arXiv 2601.02872): Gemini 2.5 Pro 73.42 overall, 74.50 at 8K and 71.77 at 256K (a shallow slope); GPT-5 72.61; Claude-4-Sonnet 69.87. — [yage.ai survey](https://yage.ai/share/long-context-benchmark-en-20260315.html); paper at [arXiv 2601.02872](https://arxiv.org/html/2601.02872v1)

**Fiction.LiveBench (2025 onward)**
- PRIMARY (description). 36 questions over 30 stories, each story cut to several lengths; tests theory of mind, chronology and implicit information, designed to be harder than needle-in-haystack. — [Epoch AI](https://epoch.ai/benchmarks/fictionlivebench)
- SECONDARY. o3 matched Gemini 2.5 Pro up to 128K tokens and dropped sharply at 192K (2025). — [The Decoder](https://the-decoder.com/openais-o3-achieves-near-perfect-performance-on-long-context-benchmark/)
- SECONDARY. Aggregator figures at the 16K tier: GPT-5 (medium) and o3-pro 97.2%; Claude Sonnet 3.7 (standard mode) 50.00. — [DataLearner](https://www.datalearner.com/en/benchmarks/fiction-livebench)
- SECONDARY. Commentary (2025): most models begin degrading between 16K and 64K tokens; Grok 4 and Gemini were the standouts above 192K. — [PromptHub](https://prompthub.substack.com/p/do-long-context-windows-actually)

**MRCR v2, 8-needle (2025 to 2026)**
- PRIMARY. OpenAI, "Introducing GPT-5.5", 2026-04-23. Accuracy by absolute token range:

| Token range | GPT-5.5 | GPT-5.4 | Claude Opus 4.7 |
|---|---|---|---|
| 4K-8K | 98.1% | 97.3% | not reported |
| 8K-16K | 93.0% | 91.4% | not reported |
| 16K-32K | 96.5% | 97.2% | not reported |
| 32K-64K | 90.0% | 90.5% | not reported |
| 64K-128K | 83.1% | 86.0% | not reported |
| 128K-256K | 87.5% | 79.3% | 59.2% |
| 256K-512K | 81.5% | 57.5% | not reported |
| 512K-1M | 74.0% | 36.6% | 32.2% |

  — [OpenAI](https://openai.com/index/introducing-gpt-5-5/)
- SECONDARY (attributed to Anthropic Sonnet 4.6 System Card, Table 2.16.A, 2026-02-17, max thinking). Aggregates at 256K / 1M: Claude Opus 4.6 93.0% / 76.0%; Claude Sonnet 4.6 90.3% / 65.8%; Claude Sonnet 4.5 10.8% / 18.5%; Gemini 3 Pro 45.4% / 24.5% and Gemini 3 Flash 58.5% / 32.6% (via contextarena.ai); GPT-5.2 at 256K 63.9% (contextarena) or 70.0% (OpenAI xhigh). — [yage.ai survey](https://yage.ai/share/long-context-benchmark-en-20260315.html); card at [Anthropic](https://www-cdn.anthropic.com/bbd8ef16d70b7a1665f14f306ee88b53f686aa75.pdf)
- SECONDARY, conflicting. A different blog gives Opus 4.6 as 91.9% at 256K and 78.3% at 1M, and Opus 4.7 as 59.2% and 32.2%, citing the Opus 4.7 system card. The Opus 4.6 figures differ from the 93.0 / 76.0 above; likely a different thinking setting, unconfirmed. — [wentuo.ai, 2026-04-18](https://blog.wentuo.ai/en/claude-opus-4-7-long-context-regression-en.html); contradicted on Opus 4.6 by [yage.ai survey](https://yage.ai/share/long-context-benchmark-en-20260315.html)
- SECONDARY. Gemini 3.1 Pro (Feb 2026 model card): 84.9% at 128K, 26.3% at 1M. Gemini 2.5 Pro (Jun 2025): about 58% at up to 128K, 16.4% at 1M. Gemini 2.0 Flash: 18.4% at up to 128K, 10.2% above 1M. — [yage.ai survey](https://yage.ai/share/long-context-benchmark-en-20260315.html)
- SECONDARY. GPT-5.2 (Dec 2025) by range: 98.2% (4K-8K), 85.6% (64K-128K), 77.0% (128K-256K). — [yage.ai survey](https://yage.ai/share/long-context-benchmark-en-20260315.html)
- SECONDARY, weak. A reposted comment attributed to an Anthropic employee says MRCR is being phased out in favour of GraphWalks as a better signal for applied long-context reasoning. — [Telegram mirror](https://t.me/s/mirra_bcherny)

**GraphWalks (2025 to 2026), F1**
- PRIMARY. OpenAI's table, 2026-04-23: BFS 256K: GPT-5.5 73.7%, GPT-5.4 62.5%, Opus 4.7 76.9%. BFS 1M: GPT-5.5 45.4%, GPT-5.4 9.4%, Opus 4.6 41.2%. Parents 256K: GPT-5.5 90.1%, GPT-5.4 82.8%, Opus 4.7 93.6%. Parents 1M: GPT-5.5 58.5%, GPT-5.4 44.4%, Opus 4.6 72.0%. — [OpenAI](https://openai.com/index/introducing-gpt-5-5/)
- SECONDARY (attributed to Anthropic Opus 4.8 card Table 8.9.A, 2026-05-28, and Fable 5 / Mythos 5 card Table 8.13.A, 2026-06-09; Anthropic prompt, 5-trial average):

| Model | BFS 256K | BFS 1M | Parents 256K | Parents 1M |
|---|---|---|---|---|
| Claude Mythos 5 | 91.1% | 79.4% | 99.96% | 97.5% |
| Claude Mythos Preview | 85.7% | 74.3% | 99.9% | 95.5% |
| Claude Opus 4.8 | 85.9% | 68.1% | 99.3% | 83.3% |
| Claude Opus 4.7 | 76.9% | 40.3% | 93.6% | 56.6% |
| Claude Opus 4.6 | 61.1% | 16.3% | 95.4% | 48.6% |
| GPT-5.5 (OpenAI-reported) | 73.7% | 45.4% | 90.1% | 58.5% |

  — [Klu.ai](https://klu.ai/benchmarks/graphwalks); Opus 4.8 figures repeated by [llm-stats](https://llm-stats.com/blog/research/claude-opus-4-8-launch) and [DataCamp](https://www.datacamp.com/blog/claude-opus-4-8-vs-gpt-5-5)
- SECONDARY, conflicting. The Feb 2026 Opus 4.6 card (Table 2.18.A) reported Opus 4.6 at BFS 1M 38.7% (max effort) or 41.2% (64k thinking) and Parents 1M 72.0% / 71.1%; Sonnet 4.5 at BFS 44.9% (256K) and 25.6% (1M), Parents 81.0% and 50.2%. The May 2026 card re-reports Opus 4.6 1M as 16.3% and 48.6%. The 256K figures match; the 1M definition evidently changed, and Anthropic has not explained the mapping. — [Klu.ai](https://klu.ai/benchmarks/graphwalks)
- SECONDARY. Anthropic's cards say the 1M subset results "are not reproducible via the public API, as the problems exceed its 1M token limit"; an internal setting was used. — [Klu.ai](https://klu.ai/benchmarks/graphwalks); [DataCamp](https://www.datacamp.com/blog/claude-opus-4-8-vs-gpt-5-5)
- SECONDARY (Google self-reported, Oct 2026, different bins: up to 128K / 256K-1M BFS): Gemini 4 Argon 99.7% / 84.2%; GPT-6 Astra 98.7% / 71.8%; Claude Fable 5.1 91.4% / 65.0%; Claude Opus 5.5 90.6% / 66.8%; Gemini 4 Carbon (High) 96.2% at 256K-1M. Grader and settings for competitor models are not specified. — [Klu.ai](https://klu.ai/benchmarks/graphwalks)
- SECONDARY. The Anthropic Opus 5.5 and Fable 5.1 cards and the OpenAI GPT-6 cards reportedly publish no GraphWalks numbers; the Opus 5.5 card is said to use ProgramBench as its long-context evaluation. — [Klu.ai](https://klu.ai/benchmarks/graphwalks)

**Third-party 1M tests (2026)** SECONDARY, low confidence
- "NIAH-2" at 1M (Digital Applied): single needle GPT-5.5 96%, Opus 4.7 89%, Gemini 3 99%, DeepSeek V4 Pro 78%; 8 needles 74% / 56% / 89% / 41%. — [CodingFleet, 2026-05-29](https://codingfleet.com/blog/context-window-lie-how-well-ai-models-use-1m-tokens-2026/)
- One tester's multi-fact reasoning run: Gemini 3.1 Pro 78.4% at 500K and 64.1% at 1M; in July 2026, Opus 5 (capped at 500K) 84% vs Gemini 3.5 Pro 79% at the same depth, with both near-perfect on needle-in-haystack. Methods not inspected. — [Contra Collective](https://contracollective.com/blog/gemini-3-1-pro-vs-claude-opus-4-7-long-context-reasoning-2026); [Contra Collective, July 2026](https://contracollective.com/blog/gemini-3-5-pro-vs-claude-opus-5-1m-long-context-reasoning-2026)

### Inferences
- On the one benchmark with a consistent Anthropic-run series (GraphWalks, as relayed), each Claude generation in 2026 degrades less from 256K to 1M than its predecessor: BFS loss of about 45 points (Opus 4.6), 37 (Opus 4.7), 18 (Opus 4.8), 12 (Mythos 5). The loss never reaches zero.
- Opus 4.7 is a non-monotonic case: worse than Opus 4.6 on MRCR v2 (32.2% vs 76-78% at 1M) but better on GraphWalks (BFS 1M 40.3% vs 16.3%). "Newer model degrades less" depends on the benchmark, and MRCR penalises abstention.
- MRCR curves are not smooth (GPT-5.5 scores higher at 128K-256K than at 64K-128K), so single-bucket comparisons carry noise.

### Gaps
- I did not open any Anthropic system card PDF; all Claude 4.6+ numbers are relayed by aggregators. Verify against the cards before publication.
- No Claude scores at 120K/192K on Fiction.LiveBench were found; the live leaderboard was not reachable through the aggregators searched.
- No exact Claude numbers extracted for Michelangelo, BABILong or LongBench v2 beyond those listed.
- No MRCR numbers found for Opus 4.8, Mythos 5, Fable 5.x, Opus 5.5, GPT-6 or Gemini 4.
- No independent (non-vendor) GraphWalks leaderboard exists, per [Klu.ai](https://klu.ai/benchmarks/graphwalks).

## 3. What mechanisms are proposed?

### Takeaway
The literature offers several partly overlapping explanations (positional U-shape, absence of literal lexical cues, distractor interference, attention competition, abstention under ambiguity) and almost no causal evidence on frontier models. The strongest 2026 mechanistic claim, that competition for attention rather than raw length drives retrieval loss, is demonstrated only on 0.5B to 3B models.

### Cited Findings
- Positional bias / U-shaped recall (2023; GPT-3.5, Claude 1.3): accuracy is highest when the answer is at the start or end and dips 20+ points mid-context. — [arXiv 2307.03172](https://arxiv.org/pdf/2307.03172)
- Lexical-cue dependence (2025; Claude 3.5 generation): without literal matches, attention has a harder time locating the fact as context grows; a single literal distractor cuts GPT-4o's effective length to about 1K; two-hop questions degrade faster than one-hop; chain-of-thought helps only partly, and two-hop still struggles beyond 16K. For two-hop tasks, context length matters more than depth, which the authors attribute to attention limits, not positional encoding. A lost-in-the-middle dip appears at 32K. — [arXiv 2502.05167v3](https://arxiv.org/html/2502.05167v3)
- Distractor interference and semantic similarity (2025; Claude 3.5 to Opus 4): lower needle-question similarity and more distractors both steepen the length curve; coherent haystacks are harder than shuffled ones. Not mechanistically tested. — [Chroma](https://www.trychroma.com/research/context-rot)
- Attention competition (2026-11; small open models only): across 126,473 trials, log inverse attention mass on the evidence explains 35% to 58% of the variance in loss, log length at most 8%. Neutral filler up to 32K tokens causes almost no loss; same-format conflicting statements drive Qwen2.5-1.5B accuracy to roughly 0.11 to 0.21 at 8K to 32K. Stated limits: predictive not causal, one synthetic task, models up to 3B (one 7B replication). — [arXiv 2611.04169](https://arxiv.org/html/2611.04169v1)
- Filler-answer interference (2026-05; Chinese-lab models incl. DeepSeek-V4-Pro, Kimi-K2.6, GLM-5.1; 8K to 64K): 76% of middle-position errors reproduce the answer to a filler question, vs 22% at the end. Duplicating the target at the end restores accuracy at 8K for all nine models but only for some at 64K. Authors say this is a consistency check, not mechanism isolation. — [arXiv 2605.23170](https://arxiv.org/html/2605.23170v1)
- Abstention / premature termination (2025 to 2026): Claude Opus 4 and Sonnet 4 abstain under ambiguity in long prompts ([Chroma](https://www.trychroma.com/research/context-rot)); open models in long-horizon search give up earlier as context grows ([arXiv 2606.29718](https://arxiv.org/abs/2606.29718)).
- Effective vs advertised context: BABILong (2024) puts effective use at 10-20% of the window ([arXiv 2406.10149](https://arxiv.org/abs/2406.10149)); NoLiMa (2025) puts effective length at 1K to 16K for models advertising 128K to 2M ([arXiv 2502.05167v3](https://arxiv.org/html/2502.05167v3)).
- SECONDARY. A blog quotes the Opus 4.7 system card as saying "Opus 4.7 shows early signs of mid-context blindness, especially beyond 128k tokens" and that "Opus 4.6 with 64k extended-thinking mode dominates 4.7 on long-context multi-needle retrieval". Not verified against the card. — [wentuo.ai](https://blog.wentuo.ai/en/claude-opus-4-7-long-context-regression-en.html)

### Inferences
- "Attention dilution" as commonly stated (more tokens means less attention per token) is not well supported as a pure length effect; the 2025 to 2026 evidence points to what the extra tokens contain (similar, conflicting or answer-shaped content) as the main driver. This matters for coding agents: a context full of near-duplicate file versions, stale tool outputs and superseded plans is the high-competition case.
- Positional bias appears to be shrinking in newer models under benign filler but persists under distractor-like filler ([arXiv 2605.23170](https://arxiv.org/html/2605.23170v1)); this is measured on non-Claude models only.

### Gaps
- No interpretability study of long-context degradation on a frontier closed model was found.
- No primary Anthropic statement on the cause of the Opus 4.7 MRCR drop was verified; the "calibration tradeoff / refuses when information is missing" explanation in [CodingFleet](https://codingfleet.com/blog/context-window-lie-how-well-ai-models-use-1m-tokens-2026/) is unsourced there.

## 4. Is degradation tied to absolute token count or to percentage of the window filled? Evidence on 200K vs 1M-context models?

### Takeaway
No study found isolates the two. All primary data is reported in absolute tokens, and what exists points to absolute length and content as the drivers, with the advertised window being a poor predictor: models with 1M to 2M windows degraded at the same few-thousand-token scale as 128K models in 2025.

### Cited Findings
- 2025: Gemini 1.5 Pro (2M window) has a NoLiMa effective length of 2K; Gemini 2.0 Flash (1M) 4K; Claude 3.5 Sonnet (200K) 4K; GPT-4o (128K) 8K; GPT-4.1 (1M) 16K. Window size does not track effective length. — [arXiv 2502.05167v3](https://arxiv.org/html/2502.05167v3)
- 2024: BABILong states the finding as a percentage (10-20% of context used effectively). — [arXiv 2406.10149](https://arxiv.org/abs/2406.10149)
- 2026: lab reporting uses absolute bins (4K-8K up to 512K-1M for MRCR; 256K and 1M subsets for GraphWalks). — [OpenAI](https://openai.com/index/introducing-gpt-5-5/)
- 2026: among 1M-window models, the score in the same 512K-1M absolute bin ranges from 26.3% (Gemini 3.1 Pro) to 74.0% (GPT-5.5) and about 76% (Opus 4.6), so equal fill percentage gives very different reliability. — [OpenAI](https://openai.com/index/introducing-gpt-5-5/); [yage.ai survey](https://yage.ai/share/long-context-benchmark-en-20260315.html)
- SECONDARY. Claude stayed at 200K from March 2024 until the Opus 4.6 1M beta (Feb 2026; GA 2026-03-13 at standard pricing); Sonnet 4 had a 1M beta in Aug 2025. — [yage.ai survey](https://yage.ai/share/long-context-benchmark-en-20260315.html)
- SECONDARY. Sonnet 4.5 (a 200K-native model with 1M beta) scored 10.8% at 256K and 18.5% at 1M on MRCR v2 vs Opus 4.6 at 93.0% and 76.0%: the largest single generational jump in the data. — [yage.ai survey](https://yage.ai/share/long-context-benchmark-en-20260315.html)
- SECONDARY, unverified heuristics: "effective capacity is 50-65% of nominal"; usable context about 500K for Opus 4.6 and GPT-5.5, about 200K for Opus 4.7, about 128K for Gemini 3.1 Pro (the blog's own threshold of multi-needle above 75%). — [CodingFleet](https://codingfleet.com/blog/context-window-lie-how-well-ai-models-use-1m-tokens-2026/)

### Inferences
- A percent-of-window rule of thumb has no experimental basis in the sources found. Degradation is better described per model as a curve over absolute tokens, modulated by content. A 1M-window model at 200K is not "20% full and therefore safe" by construction; whether it is reliable there depends on that model's curve (Opus 4.8 and Mythos 5 are reported near-saturated on GraphWalks at 256K; Opus 4.7 was at 59.2% on MRCR at 128K-256K).
- No source compares the same model weights served with a 200K cap against a 1M cap, so no claim can be made that the 1M variant behaves differently at equal token counts.

### Gaps
- No controlled experiment varying window size while holding absolute tokens fixed (or the reverse).
- No published degradation data specific to agentic coding transcripts for Claude, other than the monitor study in section 1.

## 5. What is the newest research (2026), and do newer models degrade less?

### Takeaway
Yes on the available vendor-reported long-context reasoning numbers, with exceptions: the 256K-to-1M drop shrinks generation over generation for Claude (GraphWalks) and GPT (MRCR), yet degradation at 1M remains measurable for every model, Opus 4.7 regressed on MRCR, and independent 2026 work still finds 2x to 30x worse monitoring at 800K tokens for Opus 4.6-class models.

### Cited Findings
- GPT-5.4 to GPT-5.5 (Mar to Apr 2026), MRCR v2 512K-1M: 36.6% to 74.0%; GraphWalks BFS 1M: 9.4% to 45.4%. — [OpenAI](https://openai.com/index/introducing-gpt-5-5/)
- SECONDARY. Claude, GraphWalks BFS 1M, Anthropic-run series: Opus 4.6 16.3%, Opus 4.7 40.3%, Opus 4.8 68.1% (2026-05-28), Mythos 5 79.4% (2026-06-09). Parents 1M: 48.6%, 56.6%, 83.3%, 97.5%. — [Klu.ai](https://klu.ai/benchmarks/graphwalks)
- SECONDARY. Oct 2026 Google-reported BFS at 256K-1M: Gemini 4 Argon 84.2%, Gemini 4 Carbon (High) 96.2%, GPT-6 Astra 71.8%, Claude Opus 5.5 66.8%, Claude Fable 5.1 65.0%; all above 90% at up to 128K. — [Klu.ai](https://klu.ai/benchmarks/graphwalks)
- Counter-example: Opus 4.7 at 32.2% on MRCR v2 512K-1M vs Opus 4.6 at about 76-78%. — [OpenAI](https://openai.com/index/introducing-gpt-5-5/); [yage.ai survey](https://yage.ai/share/long-context-benchmark-en-20260315.html)
- Counter-example: Gemini 3 Pro to Gemini 3.1 Pro shows no reported improvement at 1M on MRCR (both listed at 26.3%; the blog itself flags a possible copy error). — [CodingFleet](https://codingfleet.com/blog/context-window-lie-how-well-ai-models-use-1m-tokens-2026/)
- Independent 2026 results: monitors (Opus 4.6, GPT 5.4, Gemini 3.1) miss dangerous actions 2x to 30x more often after 800K tokens ([arXiv 2605.12366](https://arxiv.org/abs/2605.12366)); newer Chinese-lab models show smaller middle-position drops under benign filler but still -16 to -56 points under question-like filler ([arXiv 2605.23170](https://arxiv.org/html/2605.23170v1)).
- SECONDARY. Anthropic's launch blog is quoted as saying "Opus 4.7 delivered the most consistent long-context performance of any model we tested", which sits awkwardly beside the MRCR figure. — [wentuo.ai](https://blog.wentuo.ai/en/claude-opus-4-7-long-context-regression-en.html)

### Inferences
- The frontier of reliable long-context reasoning moved from tens of thousands of tokens (2025) to a few hundred thousand (mid-2026), with the best reported models keeping most of their accuracy to 1M on graph-traversal tasks. Results from 2023 to 2025 (Lost in the Middle, NoLiMa, Chroma) overstate the degradation a late-2026 model shows at the same token counts, but the direction of the effect is unchanged.
- Benchmarks are saturating at 256K (Parents 99%+), so the visible signal has shifted to 1M and to harder tasks; absence of a measured drop at 256K on a saturated benchmark is weak evidence about real agentic workloads.

### Gaps
- For the newest Claude models (Opus 5.5, Fable 5.1, Fable 5.2), I found no Anthropic-published degradation curve; the only numbers are Google's competitor measurements.
- No 2026 re-run of NoLiMa, Chroma's suite or Fiction.LiveBench on Claude 4.6+ was found, so "newer models degrade less" rests on MRCR and GraphWalks, both vendor-reported.

## 6. Simple retrieval vs. reasoning / multi-hop over long context

### Takeaway
The split holds across every generation: single-fact lexical retrieval is near-ceiling even at 1M for 2026 models, while multi-needle, non-lexical, multi-hop and graph-traversal tasks lose tens of points over the same span.

### Cited Findings
- SECONDARY (2026). At 1M tokens, single-needle scores are 89-99% for Opus 4.7, GPT-5.5 and Gemini 3, while 8-needle scores are 56%, 74% and 89%. — [CodingFleet](https://codingfleet.com/blog/context-window-lie-how-well-ai-models-use-1m-tokens-2026/)
- PRIMARY (2026). Same model, same length, harder task: at 1M GPT-5.5 scores 58.5% on GraphWalks Parents (one-hop lookup) and 45.4% on BFS (multi-hop traversal); at 256K, 90.1% vs 73.7%. — [OpenAI](https://openai.com/index/introducing-gpt-5-5/)
- SECONDARY (2026). Opus 4.8: Parents 99.3% (256K) and 83.3% (1M) vs BFS 85.9% and 68.1%. — [Klu.ai](https://klu.ai/benchmarks/graphwalks)
- PRIMARY (2025). NoLiMa: removing literal overlap alone takes Claude 3.5 Sonnet from 87.5 to 29.8 by 32K; two-hop degrades faster than one-hop. — [arXiv 2502.05167v3](https://arxiv.org/html/2502.05167v3)
- 2024: Michelangelo's central claim is that fact retrieval ability does not imply ability to synthesise over the full context. — [arXiv 2409.12640](https://arxiv.org/html/2409.12640v2)
- 2024: BABILong: performance "declines sharply with increased reasoning complexity". — [arXiv 2406.10149](https://arxiv.org/abs/2406.10149)
- 2025: a Gemini 2.5 Flash study reports no lost-in-the-middle effect for simple factoid QA (search-result level; paper not opened). — [arXiv 2511.05850](https://arxiv.org/pdf/2511.05850v1)
- SECONDARY (2023). Anthropic's Claude 2.1 write-up: a one-sentence prompt prefill lifted 200K-token single-fact recall from 27% to 98%, showing early retrieval failures were partly reluctance, not inability. — [DEV Community](https://dev.to/gabrielanhaia/lost-in-the-middle-is-still-real-in-2026-even-on-1m-token-models-2ehj)

### Inferences
- Classic needle-in-haystack results say little about agentic coding sessions, which resemble the hard cases: many similar items (file versions, repeated tool outputs), ordering-dependent facts (which edit came last, as in MRCR), and multi-hop dependency tracing (as in GraphWalks BFS).
- The gap between Parents and BFS at equal length is a direct, same-model measure of how much of the loss is reasoning depth and not retrieval.

### Gaps
- The third-party single-vs-multi-needle 1M numbers come from one blog relaying another (Digital Applied); the methodology was not inspected.
- No benchmark found measures long-context degradation on real coding-agent transcripts with task success as the outcome.
