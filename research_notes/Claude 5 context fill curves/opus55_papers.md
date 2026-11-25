# Opus 5.5 / Sonnet 5.5 / Haiku 5.5 / Fable 5.2 performance versus context length in research papers (search date 25 Nov 2026)

**Bottom line:** No paper found gives a controlled score-versus-context-tokens curve (same task, filler varied) for Opus 5.5, Sonnet 5.5, Haiku 5.5 or Fable/Mythos 5.2. The closest measured data is CTXAgent (arXiv 2610.34425), which ran the same sessions with Opus 5.5 / Sonnet 5.5 / Haiku 5.5 at different live context sizes (about 42k to 289k mean, 77k to 688k peak). Scores there are essentially flat. No paper found tests Fable 5.2 or Mythos 5.2 against context length at all.

## Data table (every row opened and read in the paper's HTML full text)

All CTXAgent rows: model called via OpenRouter, provider pinned, reasoning effort high, output reserve 20k, 3 seeds pooled unless a seed is given. "Mean/peak" = prompt tokens per request measured on the wire. Context size is a consequence of the harness/compaction setting, not of added filler.

| model | task | context size / condition | score | settings | paper | table/figure | URL |
|---|---|---|---|---|---|---|---|
| Opus 5.5 | CTXAgent mixed sessions (101-115 turns; 34 Aider-polyglot Python exercises + 6-7 SciCode problems + planted facts) | ctxagent arm A, self-compaction: mean prompt 42k, peak 77k, 3.7 cuts/session | success 0.968 (all scored turns); 77/80 solvable SciCode steps; memory 1.000 | 1M window, effort high | CTXAgent | Table 4 | https://arxiv.org/html/2610.34425 |
| Opus 5.5 | same | ctxagent arm B, no context management: mean prompt 127k, peak 244k, 0 cuts | success 0.979; 80/80 steps; memory 1.000 | 1M window, effort high | CTXAgent | Table 4 | https://arxiv.org/html/2610.34425 |
| Sonnet 5.5 | same | arm A: mean 107k, peak 202k, 1.0 cuts/session | success 0.926; 65/80 steps; memory 1.000 | same | CTXAgent | Table 4 | https://arxiv.org/html/2610.34425 |
| Sonnet 5.5 | same | arm B: mean 163k, peak 333k, 0 cuts | success 0.954; 73/80 steps; memory 1.000 | same | CTXAgent | Table 4 | https://arxiv.org/html/2610.34425 |
| Haiku 5.5 | same | arm A: mean 174k, peak 339k, 0.3 cuts/session | success 0.873; 50/80 steps; memory 1.000 | same | CTXAgent | Table 4 | https://arxiv.org/html/2610.34425 |
| Haiku 5.5 | same | arm B: mean 203k, peak 417k, 0 cuts | success 0.870; 49/80 steps; memory 1.000 | same | CTXAgent | Table 4 | https://arxiv.org/html/2610.34425 |
| Opus 5.5 | CTXAgent mixed sessions, per seed (0/1/2) | ctxagent 1M: mean 44k/41k/39k, peak 78k/80k/72k, 3/4/4 cuts | success 0.966 / 0.989 / 0.951 | effort high | CTXAgent | Table 6 | https://arxiv.org/html/2610.34425 |
| Opus 5.5 | same | Claude Code 2.1.301, explicit 200k window: mean 94k/88k/92k, peak 169k (all seeds), 1 cut | success 0.966 / 0.989 / 0.980 | effort high | CTXAgent | Table 6 | https://arxiv.org/html/2610.34425 |
| Opus 5.5 | same | Claude Code native window: mean 142k/141k/148k, peak 263k/255k/277k, 0 cuts | success 0.966 / 0.989 / 0.980 | effort high | CTXAgent | Table 6 | https://arxiv.org/html/2610.34425 |
| Opus 5.5 | same | Claude Code with [1m] window: mean 289k/146k/255k, peak 654k/259k/688k, 0 cuts | success 0.966 / 0.989 / 0.970 | effort high | CTXAgent | Table 6 | https://arxiv.org/html/2610.34425 |
| Opus 5.5 | CTXAgent hard sessions (35-36 turns; 3 SWE-bench Pro + 3 Terminal-Bench 4.0 + 3 SciCode-Verified per seed) | arm A, self-compaction: peak 222k | success 0.869; 39/51 questions (SWE 8/9, TB 6/9, steps 25/33) | 1M window, effort high | CTXAgent | Table 5 | https://arxiv.org/html/2610.34425 |
| Opus 5.5 | same | arm B, no management: peak 426k | success 0.826; 35/51 (SWE 7/9, TB 6/9, steps 22/33) | same | CTXAgent | Table 5 | https://arxiv.org/html/2610.34425 |
| Sonnet 5.5 | same | arm A: peak 308k | success 0.793; 32/51 (SWE 7, TB 4, steps 21) | same | CTXAgent | Table 5 | https://arxiv.org/html/2610.34425 |
| Sonnet 5.5 | same | arm B: peak 366k | success 0.793; 32/51 (SWE 7, TB 4, steps 21) | same | CTXAgent | Table 5 | https://arxiv.org/html/2610.34425 |
| Haiku 5.5 | same | arm A: peak 554k | success 0.760; 29/51 (SWE 6, TB 1, steps 22) | same | CTXAgent | Table 5 | https://arxiv.org/html/2610.34425 |
| Haiku 5.5 | same | arm B: peak 625k | success 0.782; 31/51 (SWE 8, TB 2, steps 21) | same | CTXAgent | Table 5 | https://arxiv.org/html/2610.34425 |
| Opus 5.5 | CTXAgent marathon (one 212-turn session, about 1.5M tokens raw history, 168 scored questions) | ctxagent: 11 compactions (median 70k), peak 96k | success 0.986 | 1 run, effort high | CTXAgent | Figure 9 caption + Sec. 4.2 | https://arxiv.org/html/2610.34425 |
| Opus 5.5 | same | Claude Code, 200k window (5 compactions per Sec. 4.2) | success 0.981 | 1 run | CTXAgent | Figure 9 caption | https://arxiv.org/html/2610.34425 |
| Opus 5.5 | same | OpenCode 1.19.3, 200k window | success 0.980 | 1 run | CTXAgent | Figure 9 caption | https://arxiv.org/html/2610.34425 |
| Opus 5.5 | CTXAgent clean SciCode chains (no context pressure, 86 steps over 2 reps) | ctxagent 1M: peak 23-38k, 0 cuts | 78/86 (0.895 per chat) | effort high | CTXAgent | Table 3 | https://arxiv.org/html/2610.34425 |
| Opus 5.5 | same | Claude Code 200k: peak 38-51k, 0 cuts | 78/86 (0.895 per chat) | effort high | CTXAgent | Table 3 | https://arxiv.org/html/2610.34425 |
| Opus 5.5 (and all harness arms) | CTXAgent planted-fact probes by turn distance | distance d = 3, 10, 25, 50, 90 turns, with or without a compaction in between | retention 1.000 in every cell (ceiling) | mixed sessions | CTXAgent | note under Table 6 | https://arxiv.org/html/2610.34425 |
| Opus 5.5 | HealthCLBench, Full-context minus Query-only case score (points), by user-turn bin | 1 turn (N=157) | +29.02 | thinking/high; different cases per bin | HealthCLBench | Table 18 | https://arxiv.org/html/2611.22362 |
| Opus 5.5 | same | 2-5 turns (N=499) | +33.66 | same | HealthCLBench | Table 18 | https://arxiv.org/html/2611.22362 |
| Opus 5.5 | same | 6-10 turns (N=234) | +31.89 | same | HealthCLBench | Table 18 | https://arxiv.org/html/2611.22362 |
| Opus 5.5 | same | 11-20 turns (N=139) | +26.53 | same | HealthCLBench | Table 18 | https://arxiv.org/html/2611.22362 |
| Opus 5.5 | same | 21-40 turns (N=56) | +23.95 | same | HealthCLBench | Table 18 | https://arxiv.org/html/2611.22362 |
| Opus 5.5 | same | >40 turns (N=18) | +65.74 (absolute full-context case score in this bin: 82.56) | same | HealthCLBench | Table 18 + Sec. 6.3 | https://arxiv.org/html/2611.22362 |
| Opus 5.5 (as summarizer only; continuation agent is Kimi-K3) | Compact-Bench Task Preservation: share of 22 full-history SWE-bench successes broken when history is replaced by the summary | Round 1 / stable (3-run) | 45% (10/22) / 45% (10/22), 95% CI 27-65 | Claude Code compaction prompt, default thinking | Compact-Bench | Table 2 | https://arxiv.org/html/2610.31710 |

## Does any paper give Opus 5.5 (or Sonnet 5.5 / Haiku 5.5 / Fable 5.2) results at two or more context sizes, compaction thresholds, turn counts or trajectory lengths on the same task?

### Takeaway
Yes, but only one paper comes close: CTXAgent (arXiv 2610.34425, 25 Oct 2026) reports Opus 5.5, Sonnet 5.5 and Haiku 5.5 on identical session scripts under several context regimes, and the scores barely move with context size. It is a harness/compaction comparison, not a controlled filler sweep, so it gives 2-4 points per model rather than a curve, and the authors say the score differences are not significant.

### Cited Findings
- CTXAgent design: seven models run through one agent on the same three "mixed" sessions (101-115 turns) in arm A (model may compact/curate) and arm B (no context management, "the context grows monotonically to 230-430k tokens by the end of a session"); common settings are "same model id via OpenRouter, provider pinned; reasoning effort high; output reserve 20k". — [CTXAgent, Sec. 3.4 and 4.1](https://arxiv.org/html/2610.34425)
- Opus 5.5, mixed sessions: arm A success 0.968, 77/80 SciCode steps, peak 77k, mean prompt 42k; arm B success 0.979, 80/80 steps, peak 244k, mean prompt 127k. — [CTXAgent, Table 4](https://arxiv.org/html/2610.34425)
- Sonnet 5.5, mixed sessions: arm A 0.926, 65/80, peak 202k, mean 107k; arm B 0.954, 73/80, peak 333k, mean 163k. — [CTXAgent, Table 4](https://arxiv.org/html/2610.34425)
- Haiku 5.5, mixed sessions: arm A 0.873, 50/80, peak 339k, mean 174k; arm B 0.870, 49/80, peak 417k, mean 203k. — [CTXAgent, Table 4](https://arxiv.org/html/2610.34425)
- Opus 5.5 in Claude Code on the same mixed sessions, per seed: 200k window (peak 169k, 1 cut) 0.966/0.989/0.980; native window (peak 255-277k, 0 cuts) 0.966/0.989/0.980; [1m] window (peak 654k/259k/688k, 0 cuts) 0.966/0.989/0.970; ctxagent (peak 72-80k) 0.966/0.989/0.951. — [CTXAgent, Table 6](https://arxiv.org/html/2610.34425)
- The paper states Claude Code "did not auto-compact at its default window for Opus 5.5: its context reached 277k tokens without a cut, and with the [1m] suffix it grew by 7k instead of 3k per turn, reaching 688k and costing 53% more for the same answers." — [CTXAgent, Sec. 4.2](https://arxiv.org/html/2610.34425)
- Hard sessions (SWE-bench Pro + Terminal-Bench 4.0 + SciCode-Verified): Opus 5.5 arm A 0.869 (39/51, peak 222k) vs arm B 0.826 (35/51, peak 426k); Sonnet 5.5 0.793 (32/51) in both arms (peaks 308k vs 366k); Haiku 5.5 0.760 (29/51, peak 554k) vs 0.782 (31/51, peak 625k). — [CTXAgent, Table 5](https://arxiv.org/html/2610.34425)
- Marathon (212 turns, about 1.5M tokens raw history): success 0.986 / 0.981 / 0.980 for Opus 5.5 with ctxagent / Claude Code / OpenCode; ctxagent compacted eleven times and peaked at 96k; "every memory item was kept in every arm". — [CTXAgent, Figure 9 caption and Sec. 4.2](https://arxiv.org/html/2610.34425)
- Recall probes are at ceiling: "Retention by distance and compaction-between was 1.000 in every cell of every arm" (distances 3, 10, 25, 50, 90 turns). — [CTXAgent, note under Table 6](https://arxiv.org/html/2610.34425)
- Significance: paired McNemar Claude Code vs ctxagent for Opus is 3/0, p=0.25; the authors write "the success differences are not significant" and "the number of benchmark runs was very limited". — [CTXAgent, Figure 8 caption and Sec. 5.3](https://arxiv.org/html/2610.34425)
- Figure 2 of CTXAgent plots context size per request against user turn for each model (a context-growth trace, not a score curve); no numeric values are given in the figure beyond those in the tables. — [CTXAgent, Figure 2](https://arxiv.org/html/2610.34425)
- HealthCLBench bins 1,103 health-dialogue cases by user-turn count; for Claude Opus 5.5 (thinking/high) the Full-context minus Query-only difference is +29.02 (1 turn), +33.66 (2-5), +31.89 (6-10), +26.53 (11-20), +23.95 (21-40), +65.74 (>40, N=18). — [HealthCLBench, Table 18](https://arxiv.org/html/2611.22362)
- HealthCLBench gives one absolute long-dialogue score: in the >40-turn group (18 cases) "Claude's full-context case score is 82.56 versus GPT's 66.06". It does not report absolute scores for the other turn bins. — [HealthCLBench, Sec. 6.3](https://arxiv.org/html/2611.22362)
- Compact-Bench uses Opus 5.5 only as a summarizer (continuation agent is Kimi-K3): replacing full history with its summary broke 45% (10/22) of validated repairs in round 1 and 45% (10/22) stably, 95% CI 27-65; Opus 4.8 was best. This is a full-history-vs-summary contrast, not a context-size series. — [Compact-Bench, Table 2](https://arxiv.org/html/2610.31710)
- "Can an LLM dream?" runs Claude Opus 5.5 (claude-opus-5-5, medium effort, Claude Code headless) over ten simulated 130,000-token days with a memory capped at 19,500 to 117,000 tokens; final recall is 14 ± 4% without nightly rewrite and 76 ± 5% with clean rewrite. This varies memory policy, not context fill. — [arXiv 2611.21648, Sec. 3.1, 3.4, 4.1](https://arxiv.org/html/2611.21648)
- TasteVal caps researcher context at 200k tokens and reports only qualitatively that "preliminary experiments found that performance was the same when comparing capped to uncapped window"; no numbers. — [arXiv 2610.06824](https://arxiv.org/html/2610.06824)
- METR's Opus 5.5 post contains no results by context length, token budget or transcript length (read via page summary; no tables or figures reported). — [METR, 22 Sep 2026](https://metr.org/blog/2026-09-22-claude-opus-5-5/)

### Inferences
- Plotting CTXAgent mixed-session success against mean prompt size for Opus 5.5 gives roughly: 42k -> 0.968, about 91k -> 0.978 (mean of the three Claude Code 200k seeds), 127k -> 0.979, about 144k -> 0.978 (native), about 230k -> 0.975 (1M). That is flat within seed noise. These seed means are my arithmetic from Table 6, not figures the paper prints.
- These points are confounded: lower-context conditions differ by having compaction (information loss) rather than less material, the harness differs between rows (ctxagent vs Claude Code), and tasks were near ceiling. They cannot show "context rot" cleanly and should not be presented as a degradation curve.
- For Sonnet 5.5 the larger-context arm scored higher (73 vs 65 steps), and for Opus 5.5 on hard sessions the smaller-context arm scored higher (39 vs 35 of 51); with 3 seeds neither direction is established.
- HealthCLBench turn bins contain different cases, and the metric is a benefit-of-context difference, so it is not a same-task curve either; the >40-turn bin has only 18 cases.
- The user's belief that "someone benchmarked Opus 5.5 on the same test at several context sizes" is most plausibly a memory of CTXAgent, or of the vendor-reported two-bin result already known; I found no multi-point sweep in the research literature.

### Gaps
- No paper found with a controlled sweep (NIAH, MRCR, NoLiMa, GraphWalks, LongBench-style) over context lengths for Opus 5.5, Sonnet 5.5, Haiku 5.5, Fable 5.2 or Mythos 5.2.
- No Fable 5.2 / Mythos 5.2 result versus context size or turn count found anywhere; arXiv abstracts name Fable 5.2 in only 12 papers and Mythos 5.2 in 2, none about context length.
- arXiv search only matches titles/abstracts/metadata, so a paper that mentions these models only in body tables would be missed. Google Scholar / Semantic Scholar "cited by" lists for NoLiMa, Context Rot, MRCR, GraphWalks, LongBench v2, LongMemEval and "Lost in Conversation" were not traversed directly; web search for those benchmark names plus "Opus 5.5" returned nothing.
- OpenReview (ICLR 2027) was reached only through web search, which returned no relevant submission; the site itself was not browsed.
- Epoch AI, Apollo, UK AISI, Transluce and Anthropic research/alignment posts were not opened individually; one web search across evaluator names returned no report with context-length results. Only METR's post was opened.
- CTXAgent's released result rows (github.com/MatthiasHBusch/CTXAgent) were not downloaded; they may allow per-turn success versus live context size, which would be a genuine curve.
- 24 of the 328 candidate papers had no arXiv HTML version and were not scanned (PDF only).

## For each paper opened, which Claude models did it actually test?

### Takeaway
Of the seven papers named in the brief, only Compact-Bench includes a target model (Opus 5.5, as a summarizer); the other six test older Claude models or none. A full-text scan of 304 further Sept-Nov 2026 arXiv papers whose abstracts or metadata name the target models surfaced CTXAgent and HealthCLBench as the only ones with context-size or turn-count breakdowns.

### Cited Findings
- arXiv 2611.05306 "You don't need long context" (Belcak et al., NVIDIA, 3 Nov 2026): the only Claude model in the full text is Claude Opus 4.5. No Opus 5.5 / Sonnet 5.5 / Haiku 5.5 / Fable 5.2. — [arXiv HTML](https://arxiv.org/html/2611.05306)
- arXiv 2611.04169 "Length Is Cheap, Competition Is Not" (3 Nov 2026): no Claude models; five open models of 0.5B-3B parameters (abstract page only). — [arXiv abs](https://arxiv.org/abs/2611.04169)
- arXiv 2611.08285 "Hard Issue, Weak Model, or Bad Run?" (5 Nov 2026): Claude Haiku 4.5 and Claude Opus 4.5 only; no context-length breakdown. — [arXiv HTML](https://arxiv.org/html/2611.08285)
- arXiv 2610.31778 "Benchmarks Are Surprisingly Compaction-Friendly" (22 Oct 2026, v2 20 Nov): Claude models named are Sonnet 5, Opus 4.6 and Haiku 4.5; none of the target models. It does vary compaction threshold, but for Sonnet 5 and other non-target models. — [arXiv HTML](https://arxiv.org/html/2610.31778)
- arXiv 2610.31710 Compact-Bench (22 Oct 2026): summarizer backbones include claude-opus-4-8, claude-fable-5, claude-fable-5-1 and claude-opus-5-5; results are not binned by context size. — [arXiv HTML, Appendix G and Table 2](https://arxiv.org/html/2610.31710)
- arXiv 2610.35496 "The Hidden Costs of Forgetting" (26 Oct 2026): no Claude model; "Both agents use the same model, DeepSeek-V4-Flash". — [arXiv HTML](https://arxiv.org/html/2610.35496)
- arXiv 2609.26779 CliffCompaction (22 Sep 2026): Claude Opus 4.5, 4.6, 4.7 and Sonnet 3.5/3.7 are named; no target models. — [arXiv HTML](https://arxiv.org/html/2609.26779)
- arXiv 2610.34425 CTXAgent (25 Oct 2026): Claude Opus 5.5, Sonnet 5.5, Haiku 5.5 and Fable 5.1, plus GPT-6.1 Sol, Astra 6, Muse Wonder. — [arXiv HTML](https://arxiv.org/html/2610.34425)
- arXiv 2611.22362 HealthCLBench: Claude Opus 5.5 is the only Claude model. — [arXiv HTML](https://arxiv.org/html/2611.22362)
- arXiv 2611.21648 "Can an LLM dream?": Claude Opus 5.5 only. — [arXiv HTML](https://arxiv.org/html/2611.21648)
- Papers checked and found not to cover target models despite relevant titles: Continuous Context Management, 2609.35540 (Sonnet 4.6, Opus 4.6); "Fragility of Tasks under Composition", 2610.27190 (Opus 5); "Evaluating safeguarding in multi-turn conversations", 2611.26009 (Sonnet 5, Opus 5); "How Intervention History Changes AI Agent Sabotage", 2610.26935 (Opus 4.8, 4.5); "Higher-Scoring Coding Models Did Not Get More Work Done", 2611.23216 (Fable 5, Opus 4.8, Sonnet 5 and older). — [2609.35540](https://arxiv.org/html/2609.35540); [2610.27190](https://arxiv.org/html/2610.27190); [2611.26009](https://arxiv.org/html/2611.26009); [2610.26935](https://arxiv.org/html/2610.26935); [2611.23216](https://arxiv.org/html/2611.23216)
- Papers that name target models and mention context terms but report no per-context-size results for them: "The Last Mile to the Real World", 2611.07461 (its 32K-512K scaling study uses GPT-4.1); "To Jev or Not to Jev", 2611.00505 (Haiku 5.5 as a judge; LongRewardBench 4K-64K reported as one pooled accuracy in the main table); MKA, 2611.25228 (compaction fixed at 70% of context); "People Making Conversation", 2610.12802 (Opus 5.5 is the grader, not the system under test); SWE-CoWork, 2611.20218 (reports "no consistent relationship between interaction volume and delivery", detail in its Appendix G, not extracted). — [2611.07461](https://arxiv.org/html/2611.07461); [2611.00505](https://arxiv.org/html/2611.00505); [2611.25228](https://arxiv.org/html/2611.25228); [2610.12802](https://arxiv.org/html/2610.12802); [2611.20218](https://arxiv.org/html/2611.20218)
- A DEV Community roundup on long-context benchmarks that surfaced in search covers only Opus 4.6/4.7 and Sonnet 4.5/4.6; no target models. — [dev.to](https://dev.to/owen_fox/long-context-llm-benchmarks-2026-which-model-actually-holds-accuracy-past-200k-tokens-17ka)

### Inferences
- The compaction literature of autumn 2026 mostly ran its experiments before or just as Opus 5.5 shipped, which is why the papers named in the brief stop at Opus 4.x / Sonnet 5 / Fable 5.x.
- arXiv abstract/metadata search returned 162 hits for "Opus 5.5", 263 for claude-opus-5-5, 44 for "Sonnet 5.5", 25 for "Haiku 5.5", 12 for "Fable 5.2" and 2 for "Mythos 5.2"; almost all are agent or domain benchmarks that report a single score per model.

### Gaps
- UNVERIFIED (search snippets only, pages not opened): a readmodelmag.com "Opus nerf tracker" story and an llmapiguide.com post claiming Opus 5.5 degrades "past a few dozen turns"; both look anecdotal and no numbers were seen.
- SWE-CoWork Appendix G (usage and compaction counts versus delivery, Opus 5.5 among nine models) and the long-context judging appendix E.12 of 2611.00505 (Haiku 5.5 by length bucket, if broken out) were not extracted; they may hold per-bucket numbers.
- The per-length LongRewardBench breakdown, if any, and any figure-only data in the papers above were not read from charts.
