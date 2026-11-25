# Only Opus 5.5 has a measured curve

As of 25 November 2026, exactly one same-test-at-different-context-sizes series exists for any of the four current models, and it has two points: Google's own GraphWalks run puts **Claude Opus 5.5 at 90.6% F1 up to 128K tokens and 66.8% in the 256K–1M range, a loss of about 24 points** ([Google, Gemini 4 Argon evaluation](https://storage.googleapis.com/deepmind-media/gemini/gemini_4_argon_model_evaluation.pdf)). Nothing length-binned exists for Haiku 5.5, Sonnet 5.5 or Fable 5.2, from Anthropic or anyone else, and nobody has measured any of the four at several fill levels inside Claude Code. Anthropic's four system cards each report one long-context number, a ProgramBench aggregate, with no per-length breakdown; the last Anthropic length-binned table was for Mythos 5 in June 2026. The most informative in-scope finding is about budgeting, not rot: Fable 5.2 ran out of its 1M window on 68% of ProgramBench tasks when it could not see how much context remained, and gained 6 points when it could ([Fable 5.2 system card](https://www.anthropic.com/claude-fable-5-2-mythos-5-2-system-card)). Predecessor models supply the only real curves: Opus 5 holds 91% on 8-needle retrieval at 128K, then falls to 66% at 256K and 43% at 512K, while Sonnet 5 is already at 68% by 64K ([Context Arena](https://contextarena.ai/api/needle-summary?needles=8)). In agentic work the one predecessor test shows a far gentler slope, flat with irrelevant filler and about 10 points down with task-related filler at 500K ([Boolean AI](https://www.boolean.ai/blog/context-rot-quantified)). So the honest answer to "how badly" is: about a quarter of multi-hop reasoning accuracy lost past 256K for Opus 5.5 on one synthetic test, unknown for the other three, and unmeasured for coding in Claude Code for all four.

## Anthropic's four cards publish aggregates, not curves

All four system cards were searched in full text for GraphWalks, MRCR, needle tests and context-length bins, and **none contains a single per-context-length data point**. Each card's long-context section holds one benchmark, ProgramBench: 166 program-reconstruction tasks run in the mini-swe-agent harness inside a single 1M-token window, scored by hidden-test pass rate, with episodes that "cover a range of context lengths up to the full 1M-token window" ([Opus 5.5 system card](https://www.anthropic.com/claude-opus-5-5-system-card)). The distribution of fill levels those episodes reached is not published, so the scores cannot be turned into a curve. The reasoning-effort setting for these runs is not stated in any card.

Table 1. ProgramBench, the only long-context evaluation in the four cards. Whole-run aggregates, not binned by context length.

| Model | Status | Score, model not told remaining context | Score, remaining tokens shown each step | Tasks that exhausted the 1M window |
|---|---|---|---|---|
| Claude Fable 5.2 | in scope | 85.6% | 91.6% | 68% |
| Claude Opus 5.5 | in scope | 91.2% | 91.1% | 39% |
| Claude Sonnet 5.5 | in scope | 79.7% | not reported | not reported |
| Claude Haiku 5.5 | in scope | 82.0% | not reported | not reported |
| Claude Fable 5.1 | STAND-IN (predecessor) | 87.6% | 86.8% | 28% |
| Claude Fable 5 | STAND-IN (predecessor) | 86.3% | not reported | not reported |
| Claude Opus 5 | STAND-IN (predecessor) | 85.4% | not reported | not reported |
| Claude Sonnet 5 | STAND-IN (predecessor) | 77.3% | not reported | not reported |

Sources: [Fable 5.2 card, §8.10.1](https://www.anthropic.com/claude-fable-5-2-mythos-5-2-system-card); [Opus 5.5 card, §8.10.1](https://www.anthropic.com/claude-opus-5-5-system-card); [Sonnet 5.5 card, §8.10.1](https://www.anthropic.com/claude-sonnet-5-5-system-card); [Haiku 5.5 card, §8.7.1](https://www.anthropic.com/claude-haiku-5-5-system-card); [Fable 5.1 card](https://www.anthropic.com/claude-fable-5-1-mythos-5-1-system-card).

The Fable 5.2 row is the closest thing Anthropic offers to an edge-of-window measurement. The card states the harness gives "a single 1M-token context window with no indication of how much remains" and that **Fable 5.2 "ran out of context before submitting on 68% of tasks"**; showing the remaining token count after each step lifted its score from 85.6% to 91.6%, while Opus 5.5 barely moved (91.2% to 91.1%) ([Fable 5.2 card](https://www.anthropic.com/claude-fable-5-2-mythos-5-2-system-card)). The card does not name a cause. The reading that fits both numbers is that Fable 5.2's shortfall at the top of the window is mostly a failure to budget, not a decline in the quality of its work, because a model whose output had rotted would not recover 6 points from a token counter. This matters in practice because the blind condition is the default one: Anthropic's API docs say only Sonnet 4.5 and Haiku 4.5 receive automatic remaining-context tags, and "no other model receives these tags" ([Claude API docs, context windows](https://platform.claude.com/docs/en/build-with-claude/context-windows)).

Anthropic stopped publishing length-binned results after the Fable 5 and Mythos 5 card of June 2026; the Fable 5.1 card of September already carried ProgramBench only ([Fable 5.1 card](https://www.anthropic.com/claude-fable-5-1-mythos-5-1-system-card)). The Fable 5.2 card mentions that Anthropic's internal capability index still uses "two long-context benchmark families" with several difficulty settings, but names neither and gives no scores ([Fable 5.2 card, §2.3.5](https://www.anthropic.com/claude-fable-5-2-mythos-5-2-system-card)). The last published Anthropic curve is below. It belongs to Mythos 5, the model in the June card, and is not Fable 5.2 data.

Table 2. STAND-IN (predecessor): GraphWalks F1 by context length, Claude Mythos 5, Anthropic-run, average of five trials, effort not stated.

| Task | 256K subset | 1M subset | Loss |
|---|---|---|---|
| GraphWalks BFS (multi-hop traversal) | 91.1 | 79.4 | 11.7 points |
| GraphWalks Parents (one-hop lookup) | 99.96 | 97.5 | 2.5 points |

Source: [Fable 5 and Mythos 5 card, Table 8.13.A](https://anthropic.com/claude-fable-5-mythos-5-system-card). The card notes that the 1M subset "results are not reproducible via the public API, as the problems exceed its 1M token limit."

The launch posts add nothing measurable. None of the four mentions context windows, GraphWalks, MRCR or ProgramBench; their long-running claims are customer anecdotes (Opus 5.5 "stayed on task for over 18 hours") or multi-agent runs across many windows, such as the 1,000 Fable 5.2 agents in the launch post, which say nothing about a single window filling ([Opus 5.5 launch](https://www.anthropic.com/news/claude-opus-5-5); [Fable 5.2 launch](https://www.anthropic.com/claude-fable-and-mythos-5-2)). The docs' standing statement is qualitative and not model-specific: "As token count grows, accuracy and recall degrade, a phenomenon known as context rot" ([Claude API docs](https://platform.claude.com/docs/en/build-with-claude/context-windows)).

## Google's two bins put Opus 5.5 down 24 points past 256K

The single independent series comes from a competitor. Google's October 2026 evaluation document for Gemini 4 Argon states that "Graphwalks results for all models are self-computed", so these are Google-run numbers, not relayed Anthropic figures ([Google, Argon evaluation PDF](https://storage.googleapis.com/deepmind-media/gemini/gemini_4_argon_model_evaluation.pdf)). The test is single-turn graph traversal with no tools, which is the hard end of long-context work.

Table 3. GraphWalks BFS (F1) by context length, run by Google, results as of October 2026. Google says it defaults to the maximum thinking settings for competitor models; the exact setting per row is not stated.

| Model | Status | Up to 128K (650 items) | 256K to 1M (200 problems) | Loss |
|---|---|---|---|---|
| Claude Opus 5.5 | in scope | 90.6% | 66.8% | 23.8 points |
| Claude Fable 5.1 | STAND-IN for Fable 5.2 | 91.4% | 65.0% | 26.4 points |
| Gemini 4 Argon | comparison only | 99.7% | 84.2% | 15.5 points |
| GPT-6 Astra | comparison only | 98.7% | 71.8% | 26.9 points |

Source: [Google, Gemini 4 Argon model evaluation, pp. 3 and 5](https://storage.googleapis.com/deepmind-media/gemini/gemini_4_argon_model_evaluation.pdf); same table on the [model card page](https://deepmind.google/models/model-cards/gemini-4-argon/).

Three limits apply before reading this as a curve. The upper bin pools everything from 256K to 1M, and its composition is unknown: Google counts 200 problems, while Epoch AI describes the benchmark's over-128K set as 200 prompts at 256K plus 200 at 1M ([Epoch AI](https://epoch.ai/benchmarks/graphwalks/review)). The right way to plot it is a bar spanning 256K–1M, not a point at 1M. Second, the token counts are the benchmark's nominal ones, and Claude's tokenizer produces more tokens for the same text; Boolean AI measured about 1.7 Claude tokens per GPT token on its material ([Boolean AI](https://www.boolean.ai/blog/context-rot-quantified)). Google does not say how prompts that overflow a Claude window were handled. Third, Opus 5.5 and Fable 5.1 are within two points of each other in both bins, which is inside the noise of 200 items, so the table does not rank them.

The Google numbers also should not be laid beside Anthropic's Mythos 5 table as though they formed a trend. Fable 5.1 at 65.0% in Google's 256K–1M bin and Mythos 5 at 79.4% on Anthropic's 1M subset come from different runners, different prompts and different bin definitions; the gap says nothing reliable about whether the Fable line got worse.

Everywhere else the four models are absent. Context Arena's registry of 545 models has no entry for Haiku 5.5, Sonnet 5.5, Opus 5.5, Fable 5.1 or Fable 5.2, and Fable 5 is registered with zero results ([Context Arena API](https://contextarena.ai/api/models)). Fiction.LiveBench has been on hiatus since spring 2026 and a mirror lists all four as missing ([pareto.my](https://pareto.my/benchmark/fiction-livebench)). Vals AI's Opus 5.5 page has 35 benchmarks with none binned by length ([Vals AI](https://www.vals.ai/models/anthropic_claude-opus-5-5)). Searches of NoLiMa, LongBench v2, LongCodeBench and similar leaderboards surfaced nothing naming these models, though those were searched and not each opened, so that absence is weaker.

## Predecessor curves show retrieval falling off after 128K

With no fine-grained series on the current models, the predecessor data is the only way to see the shape between the endpoints. Context Arena's independently run MRCR test, which asks the model to pick out one of eight near-identical items planted in a long conversation, gives seven bins for Opus 5 and Sonnet 5 at maximum reasoning.

Table 4. STAND-IN (predecessors): MRCR 8-needle mean score by context length, run by Context Arena, reasoning "max", 861 runs per model, retrieved 25 November 2026. Bin label is the upper edge in tokens.

| Bin | Claude Opus 5 (stand-in for Opus 5.5) | 95% CI | n | Claude Sonnet 5 (stand-in for Sonnet 5.5) | 95% CI | n |
|---|---|---|---|---|---|---|
| 8K | 0.987 | 0.962–1.000 | 80 | 0.964 | 0.917–1.000 | 80 |
| 16K | 0.999 | 0.999–1.000 | 80 | 0.949 | 0.889–0.987 | 80 |
| 32K | 0.993 | 0.978–1.000 | 136 | 0.879 | 0.823–0.929 | 136 |
| 64K | 1.000 | 0.999–1.000 | 85 | 0.680 | 0.588–0.772 | 85 |
| 128K | 0.913 | 0.856–0.961 | 103 | 0.529 | 0.439–0.620 | 103 |
| 256K | 0.656 | 0.577–0.730 | 141 | 0.522 | 0.438–0.602 | 141 |
| 512K | 0.425 | 0.363–0.484 | 236 | 0.320 | 0.264–0.375 | 236 |
| 1M | no entry | – | – | no entry | – | – |

Source: [Context Arena, needle-summary API](https://contextarena.ai/api/needle-summary?needles=8). Scores rounded to three places; the 64K Opus 5 value is 0.9995.

The two models have different shapes. **Opus 5 is at ceiling through 64K, loses 9 points by 128K, then drops to 0.66 at 256K and 0.43 at 512K.** Sonnet 5 starts sliding at 32K and has lost nearly half its score by 128K. If the 5.5 generation kept those shapes, the Sonnet tier degrades about four times earlier in absolute tokens than the Opus tier on this task, but that is an extrapolation: no MRCR result exists for either 5.5 model. No series reaches a true 1M point. Context Arena returns nothing above the 512K bin for these models, and the 1,048,576-token bin cannot fit inside a 1,000,000-token window.

Retrieval of near-duplicates is the task most hostile to long context, and the one agentic stand-in looks much milder. Boolean AI prefilled the context before each of 20 terminal-bench v4 tasks and measured Opus 5's pass rate.

Table 5. STAND-IN (predecessor): agentic task pass rate by prefilled context, Claude Opus 5, run by Boolean AI, 15 September 2026, 20-task subset of terminal-bench v4, trial count and effort not stated.

| Prefill content | 0 tokens | 250K tokens | 500K tokens |
|---|---|---|---|
| Unrelated (Apache Spark Q&A) | 0.50 | 0.52 | 0.48 |
| Related (task-specific Q&A) | 0.50 | 0.425 | 0.398 |

Source: [Boolean AI, "Context Rot Quantified"](https://www.boolean.ai/blog/context-rot-quantified).

Half a million tokens of irrelevant material cost Opus 5 nothing measurable; the same volume of task-related material cost about 10 points, a fifth of its baseline. Read with Table 4, this says that what fills the window matters more than how full it is, and that retrieval benchmarks overstate the loss an agent sees. It rests on one small study of a predecessor, with no test above 500K.

## In Claude Code the measured numbers are triggers, costs and bugs

No source measures coding or instruction-following quality for any of the four models at several fill levels in Claude Code. What has been measured in or near the harness is where compaction fires, what long contexts cost, and how often a compaction summary breaks a task. All four models run a 1M window by default in Claude Code and auto-compact "at about 967K tokens by default", which is roughly 97% full; the window is user-settable down to 100K ([Claude Code docs, model configuration](https://code.claude.com/docs/en/model-config#default-auto-compact-thresholds)). Haiku 5.5 alone has a price step tied to fill, with prompts over 100,000 tokens billed at five times the lower rate ([Claude API pricing](https://platform.claude.com/docs/en/about-claude/pricing)).

Table 6. In-scope measurements at different context sizes that are not quality curves.

| Model | Test | Smaller-context arm | Larger-context arm | What it shows |
|---|---|---|---|---|
| Claude Opus 5.5 | Vending-Bench 2, Epoch AI, one run per arm, not a coding harness | 69K window with truncation (fired 114 times): final balance $9,459 | Auto-compaction at 900K: $5,254 (−44%) | Epoch attributes the drop to a worse strategy and says context "had no discernible impact" |
| Claude Haiku 5.5 | Claude Code 45-file read, one run per setup, cost only | 130K window (compacts near 97K): $0.145, 0 of 48 requests over 100K | Default window (compacts near 967K): $0.508, 36 of 48 requests over 100K | Cost rises 3.5x; no quality score reported |
| Claude Haiku 5.5 | same test | 100K window (compacts near 67K): main session "looped and never finished" | – | A harness floor: 49K of fixed starting context leaves too little room |

Sources: [Epoch AI, Vending-Bench 2 review](https://epoch.ai/benchmarks/vending-bench-2/review); [Wmedia](https://wmedia.es/en/tips/claude-code-haiku-5-5-compact-at-130k).

The Epoch result is the only published data point with one of the four models near 900K, and it cannot carry weight. It is a single run in a business simulation, compaction and window size changed together, and GPT-6 Astra moved the opposite way (+17%) under the same change.

Compaction loss has one measurement that names an in-scope model, as the writer of the summary. In Compact-Bench, each model writes a summary using Claude Code's compaction prompt and a fixed agent then attempts a SWE-bench repair from the summary alone.

Table 7. Compaction summaries that broke a repair which succeeded with full history (n = 22 repairs; "stable" means failed in at least 2 of 3 runs).

| Summarizer | Status | Stable damaged | 95% CI |
|---|---|---|---|
| Claude Opus 5.5 | in scope | 45% (10 of 22) | 27–65% |
| Claude Fable 5.1 | STAND-IN for Fable 5.2 | 36% (8 of 22) | 20–57% |
| Claude Fable 5 | STAND-IN for Fable 5.2 | 36% (8 of 22) | 20–57% |

Source: [Compact-Bench, arXiv 2610.31710](https://arxiv.org/html/2610.31710). The range across 14 summarizers ran from 5% (Claude Opus 4.8) to 64%.

The intervals overlap, so this does not rank Opus 5.5 below the Fable models; it shows that a Claude-Code-style summary written by a current flagship lost task-critical detail in somewhere between a quarter and two thirds of small repairs. Sonnet 5.5, Haiku 5.5 and Fable 5.2 are not in the tables read. A related study found that process instructions are dropped from Claude Code summaries about six times more often than build specifications, but its Claude model appears to be Opus 4.6, not one of the four ([Salle et al., arXiv 2610.31778](https://arxiv.org/abs/2610.31778)).

User reports do not fill the gap. On the same day in the Fable 5.2 launch thread, one Hacker News user wrote "I haven't subjectively felt any degradation up to like 500-600k tokens with current Opus/Fable models" and another that "current frontier models get noticeably more stupid at context sizes of only 100k tokens or so" ([Hacker News](https://news.ycombinator.com/item?id=50429068)). That five-fold spread is a range of opinion, not a threshold. No long-session report naming Sonnet 5.5 was found at all, and Haiku 5.5 reports concern cost.

Reports about Fable 5.2 from its first two weeks are contaminated by a client bug. Desktop builds bundling Claude Code 2.1.317 and 2.1.319 had no catalog entry for the model and treated it as a 200K model: one user logged auto-compactions at 168K–179K, and another logged **eight compactions in 23 minutes, each freeing only 5K–10K tokens** ([claude-code #108640](https://github.com/anthropics/claude-code/issues/108640); [claude-code #108785](https://github.com/anthropics/claude-code/issues/108785)). Several compaction-correctness fixes also shipped during October and November, including summaries that lost part of the conversation and automatic compaction reusing a stale system prompt ([Claude Code changelog](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)). A complaint from this period that quality fell after auto-compact is as likely to describe the harness as the model.

## Conclusion

The evidence sorts the four models into three states. Opus 5.5 has one two-bin synthetic curve, a confounded 900K data point and a compaction-summary result. Fable 5.2 has no curve but one useful behavioural fact, that it works until the window runs out unless told how much is left. Sonnet 5.5 and Haiku 5.5 have nothing but an aggregate each. Anthropic has moved its public long-context reporting from length-binned tests to a single agentic aggregate while raising Claude Code's default compaction point to 97% of a 1M window, so the range between roughly 256K and 967K is both where sessions are now allowed to run and where no agentic measurement exists for any current model.

Two things follow that the earlier, model-general picture did not show. Where stand-in data exists, the tier matters more than the generation: Sonnet 5's retrieval curve bends at 32K–64K and Opus 5's at 128K–256K, which argues for a much lower auto-compact window on the Sonnet and Haiku tiers than on Opus or Fable until 5.5 data says otherwise. And for Fable 5.2 the lever with measured payoff is a visible context budget, not a lower ceiling; six points on ProgramBench came from showing a token counter. The measurement that would settle the question is cheap to describe and still missing: one coding task set, one harness, each of the four models, at 100K, 250K, 500K and 900K of realistic session history.
