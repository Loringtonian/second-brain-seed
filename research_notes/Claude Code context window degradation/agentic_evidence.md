# Agentic evidence: how coding-agent performance changes over long sessions / as context fills (state of knowledge, 25 Nov 2026)

Verification legend used below:
- **[P]** = number read from the primary source page during this research.
- **[S]** = number seen only in a search-result summary of the primary source (not opened in full) — treat as likely-correct but unverified.
- **[2nd]** = secondary source only; unverified.

Overall caveat: almost no public study plots "task success vs. % of context window used" for Claude inside Claude Code. The evidence is indirect: (a) context-management ablations on SWE-bench-style benchmarks, mostly with non-Claude models; (b) failure-mode audits; (c) vendor self-reports; (d) one late-2026 academic paper that tests Claude Opus 4.6 (1M) under varied compaction thresholds.

---

## 1. What studies measure success vs. trajectory length / tokens / context-management strategy in agentic coding?

### Takeaway
Controlled ablations consistently find that trimming or compacting agent history costs little or nothing in benchmark solve rate while roughly halving cost — i.e. the unmanaged "raw" long context is not helping and sometimes hurts — but the effect is model-specific (one of five model configs lost 4–9 pp) and benchmark scores hide losses (re-fetching, longer trajectories, dropped constraints). Failed runs are modestly longer than successful ones, but length is largely confounded with task difficulty and model.

### Cited Findings

**JetBrains Research — "The Complexity Trap" (arXiv 2508.21433, Aug 2025; v3; NeurIPS 2025 DL4Code workshop). SWE-agent on SWE-bench Verified (n=500). Models: Qwen3-32B, Qwen3-Coder 480B, Gemini 2.5 Flash (with/without thinking). No Claude models tested.**
- Observation (tool-output) tokens are ~84% of an average SWE-agent turn [P] — [arXiv 2508.21433v3](https://arxiv.org/html/2508.21433v3)
- Solve rate / instance cost, Raw agent vs Observation Masking (keep last M=10 observations) vs LLM-Summary (summarize N=21 turns, keep 10) [P] — [arXiv 2508.21433v3](https://arxiv.org/html/2508.21433v3):
  - Qwen3-Coder 480B: 53.4% / $1.29 raw; 54.8% / $0.61 masking (cost −52.7%); 53.8% / $0.64 summary (−50.4%).
  - Gemini 2.5 Flash: 32.8% / $0.41 raw; 35.6% / $0.18 masking (−56.1%); 36.0% / $0.24 summary (−41.5%).
  - Gemini 2.5 Flash (thinking): 40.4% / $0.56 raw; 36.4% masking (−4.0 pp, p=0.04); 31.4% summary (−9.0 pp, p<0.0001) — the one config where context management significantly hurt.
  - Qwen3-32B: 17.0% raw; 15.0% masking; 16.0% summary (cost −51% / −55%).
  - Qwen3-32B (thinking): 23.0% raw; 24.6% masking; 24.8% summary.
- "Trajectory elongation": LLM summarization made runs longer — Gemini 2.5 Flash mean 52 turns with summary vs 44 with masking (+15%) vs 50 raw; Qwen3-Coder summary runs 15% longer than raw and 13% longer than masking. Authors' interpretation: summaries smooth over failure signals so the agent keeps going [P] — [arXiv 2508.21433v3](https://arxiv.org/html/2508.21433v3)
- Summary-generation calls cost 0.65–7.2% of instance cost [P]; a hybrid (mask first, summarize rarely) cut cost a further 7% vs masking and 11% vs summary, with +2.6 pp solve rate vs raw on a 50-instance slice (Qwen3-Coder) [P] — [arXiv 2508.21433v3](https://arxiv.org/html/2508.21433v3)
- Turn limit 250; note Gemini 2.5 Flash has a 1M window, so the raw agent was not being truncated — the raw-context run was simply no better than the masked one [P] — [arXiv 2508.21433v3](https://arxiv.org/html/2508.21433v3)
- OpenHands generalization: 50-instance slice, Gemini 2.5 Flash; after tuning window size (M=58) masking matches summary on cost and solve rate; exact figures only in a figure (not extracted) [P] — [arXiv 2508.21433v3](https://arxiv.org/html/2508.21433v3)

**OpenHands context condenser (blog, 9 Apr 2025; model not confirmed in this research)**
- Condenser gives "up to 2x" per-turn API cost reduction; on a subset of SWE-bench Verified the condensed agent solved ~54% vs ~53% baseline; baseline per-turn cost grows without bound while condensed cost stays bounded [S] — [OpenHands blog](https://openhands.dev/blog/openhands-context-condensensation-for-more-efficient-ai-agents)

**"The Hidden Costs of Forgetting: A Study of Trajectory Deletion in SWE Agents" (arXiv 2610.35496, 26 Oct 2026; Vallecillos-Ruiz, Bouzenia, Pradel, Moonen)**
- 42 trajectory-reduction configurations, two agents, SWE-bench Verified, 9,400 runs. Deleting large amounts of older content (up to 100% of reasoning text) can preserve effectiveness, but only if deletion is delayed; immediate deletion significantly degrades effectiveness [P, abstract only] — [arXiv 2610.35496](https://arxiv.org/abs/2610.35496)
- Agents often re-fetch deleted content, increasing turns and tokens. Under flat pricing, cost fell in 26/42 configs; under cache-aware pricing, cost fell in 0/42, because reduction breaks the cached prefix [P, abstract only] — [arXiv 2610.35496](https://arxiv.org/abs/2610.35496)

**"Beyond Token Savings: A Systematic Study of Context Compression in LLM Agents" (arXiv 2609.32961, 26 Sep 2026; Satish, Sinha, Kawada, Yadwadkar)**
- ~35,000 agent runs, three open-weight models, SWE-bench Verified + Terminal-Bench 1.0. Fewer tokens does not mean faster or cheaper: on Terminal-Bench with Qwen, policies using ~1/3 the tokens of the uncompressed agent took 20–80% longer. Policies with similar overall success solve different tasks; the same policy behaves very differently across models [P, abstract only] — [arXiv 2609.32961](https://arxiv.org/abs/2609.32961)

**AutoCompact (arXiv 2610.02163, Oct 2026)**
- Training the agent to decide when/what to compact: SWE-bench Verified 39.6% vs 30.4% base; SWE-PolyBench Verified 24.5% vs 19.5%; authors report proactive context management helps even when the full history fits in the window [S] — [arXiv 2610.02163](https://arxiv.org/abs/2610.02163)

**Trajectory length vs. success (correlational)**
- Empirical study of OpenHands, SWE-agent, Prometheus trajectories: failing SWE-agent runs ~18.5% longer than successful ones on SWE-bench Verified; not uniform across agents [S] — [arXiv 2511.00197](https://arxiv.org/pdf/2511.00197)
- "What Resolve Rate Hides" (2026): failed runs shift toward more steps but distributions overlap; harder tasks have longer runs and more failures [S] — [arXiv 2607.06184](https://arxiv.org/html/2607.06184v1)
- "Hard Issue, Weak Model, or Bad Run?" (Nov 2026): issue difficulty and model identity explain most failure prediction on public SWE-bench Verified trajectories; trajectory features add almost nothing beyond them [S] — [arXiv 2611.08285](https://arxiv.org/pdf/2611.08285)
- SWE-TRACE (2026) states longer trajectories are generally associated with lower solve rates [S] — [arXiv 2604.14820](https://arxiv.org/pdf/2604.14820)

**SWE-bench Pro (Scale AI, arXiv 2509.16941, Sep 2025) — failure audit includes Claude**
- SWE-agent scaffold, 50-turn cap. Public set (N=731): Claude Sonnet 4.5 43.6%, Claude Sonnet 4 42.7%, GPT-5 (high) 41.8%, Claude Haiku 4.5 39.5%. Commercial set (N=276): Claude Opus 4.1 17.8%, GPT-5 (high) 15.7%, Claude Sonnet 4 9.1% [P] — [arXiv 2509.16941](https://arxiv.org/html/2509.16941)
- Failure-mode analysis (50-turn + $2 cap; LLM judge on last 20 turns of unresolved runs). Paper prose: Claude Sonnet 4's primary failure mode is context overflow (35.6%), plus endless file reading (17.0%); Opus 4.1 fails mainly by wrong solution (35.9%) [P]. The table in the arXiv HTML gives different shares (Sonnet 4 "long-context" 57.4% and "stuck in loop" 33.9% of not-submitted failures; Opus 4.1 long-context 28.7%; GPT-5 high 2.5%) — the prose and table appear to use different denominators or versions; **check before quoting** [P] — [arXiv 2509.16941](https://arxiv.org/html/2509.16941)

**Manus (blog, Jul 2025) — practitioner numbers**
- Average input:output token ratio ~100:1 in Manus agents; KV-cache hit rate called the single most important production metric; cached Claude Sonnet input $0.30/MTok vs $3/MTok uncached (10x) [S] — [Manus blog](https://manus.im/blog/Context-Engineering-for-AI-Agents-Lessons-from-Building-Manus)

**Cognition / Devin (blog, 29 Sep 2025, Claude Sonnet 4.5)**
- Sonnet 4.5 is "aware of its own context window"; near its believed limit it takes shortcuts / leaves work unfinished ("context anxiety") even when room remains; parallel tool calls burn context faster and aggravate it. Rebuilt Devin reported 2x faster, +12% on internal Junior Developer Evals [S] — [Cognition blog](https://cognition.ai/blog/devin-sonnet-4-5-lessons-and-challenges)

### Inferences
- The best controlled evidence says the marginal value of keeping old tool outputs in context is ~zero on SWE-bench-style tasks; this is a statement about benchmark tasks with persistent files, not about long interactive sessions with user-specific instructions.
- Because Gemini 2.5 Flash (1M window) got the same or better solve rate with only the last 10 observations visible, a bigger window alone did not buy accuracy in that setup.
- Prompt caching changes the economics: under cache-aware pricing (how Claude Code is actually billed/limited), aggressive trimming may not save money (0/42 configs) — so the practical argument for compaction is quality/window-limit, not cost.
- Trajectory length is a weak predictor of failure once task difficulty is controlled; "long sessions fail more" in raw data is mostly "hard tasks take longer and fail more".

### Gaps
- No Claude model was tested in The Complexity Trap; I found no public equivalent ablation (raw vs masked vs summarized) with Claude Sonnet/Opus on SWE-bench Verified.
- No published curve of success vs. context-utilization % for Claude Code, Cursor, or Codex CLI. Blog claims such as "compact at 80%" are heuristics with no measurement behind them — [example secondary](https://d-central.tech/agentic-engineering/context-engineering/).
- Sourcegraph Amp and Letta/MemGPT: no quantitative agentic-coding results located in this research (not searched in depth).
- Full-text numbers for arXiv 2610.35496, 2609.32961, 2610.02163, 2511.00197, 2607.06184, 2611.08285 were not read; only abstracts/snippets.

---

## 2. What does multi-turn degradation research show (incl. Claude)?

### Takeaway
"Lost in Conversation" (Microsoft/Salesforce, May 2025; ICLR 2026) found an average 39% performance drop when the same task is revealed over several turns instead of one, driven by unreliability (+112%) rather than lost aptitude (−16%); Claude 3.7 Sonnet kept ~66% of its single-turn score. A Feb 2026 follow-up reports the effect persists in GPT-5.2. These are simulated chat settings, not tool-using agent loops.

### Cited Findings
- 200,000+ simulated conversations, 15 LLMs, six generation tasks (Code, Database, Actions, Data-to-Text, Math, Summary): average drop 39% from fully-specified single turn to "sharded" multi-turn; aptitude −16%, unreliability +112% [P] — [arXiv 2505.06120](https://arxiv.org/html/2505.06120v1)
- Claude results (Table 1 ratios): Claude 3.7 Sonnet — concatenated (all shards in one turn) 100.4% of full score, sharded 65.9% (≈34% drop). Claude 3 Haiku — concat 91.6%, sharded 52.4% (≈48% drop) [P] — [arXiv 2505.06120](https://arxiv.org/html/2505.06120v1)
- Because "concat" ≈ full, the loss comes from multi-turn structure, not from rephrasing; any conversation with two or more shards triggered it; stronger models (Claude 3.7 Sonnet, Gemini 2.5, GPT-4.1) degrade 30–40%, similar to small models; reasoning models (o3, DeepSeek-R1) also degrade [P/S] — [arXiv 2505.06120](https://arxiv.org/html/2505.06120v1)
- Mechanism claimed: premature answer attempts and over-reliance on earlier (wrong) assumptions — "when LLMs take a wrong turn, they get lost and do not recover" [S] — [arXiv 2505.06120](https://arxiv.org/abs/2505.06120)
- Mitigations: temperature 0 leaves ~30% unreliability in sharded setting; "snowball" (restating all prior info each turn) recovers only ~15–20% of the gap (GPT-4o/4o-mini). Authors advise starting a new conversation with consolidated requirements [P] — [arXiv 2505.06120](https://arxiv.org/html/2505.06120v1)
- Paper accepted at ICLR 2026 (oral) [S] — [ICLR 2026](https://iclr.cc/virtual/2026/oral/10009147)
- Follow-up "Intent Mismatch Causes LLMs to Get Lost in Multi-Turn Conversation" (arXiv 2602.07338, Feb 2026): effect persists; GPT-5.2 drops from 92.7% single-turn to 48.5% multi-turn; attributes cause to intent mismatch rather than unreliability [S] — [alphaXiv 2602.07338](https://www.alphaxiv.org/abs/2602.07338)
- Other 2026 follow-ups: RAG does not remove the effect ([arXiv 2609.36700](https://arxiv.org/html/2609.36700)); training-based fix "Found in Conversation" ([arXiv 2605.24432](https://arxiv.org/html/2605.24432)); entropy-guided context resetting ERGO ([arXiv 2510.14077](https://arxiv.org/pdf/2510.14077)) [S]

### Inferences
- The finding supports "restart with a consolidated brief" as a remedy for sessions where requirements were drip-fed and the agent locked onto an early wrong assumption — a mechanism distinct from context-window fill (these conversations are short).
- It should not be cited as evidence that Claude degrades with token count; shard count, not length, is the variable.

### Gaps
- No Claude 4.x/Opus 4.5+/Fable-era Claude results on this benchmark found; the Claude data are for Claude 3 Haiku and 3.7 Sonnet (2024–early 2025 models).
- No replication in tool-using coding agents found.
- Award status (Outstanding/Best Paper) conflicts between secondary sources; not verified.

---

## 3. METR time horizons and long-horizon benchmarks: how long can Claude stay coherent, and what role does context management play?

### Takeaway
Measured autonomy has grown to the many-hours range (METR lists Claude Opus 4.6 and Claude Mythos Preview near or beyond its ~16-hour measurement ceiling), and the long-running demos that work do so by externalizing state to files/tests and using many fresh sessions, not by one ever-growing context. Benchmarks rarely isolate context management; where it was varied (Epoch's Vending-Bench 2 audit, one run each) results were inconsistent.

### Cited Findings

**METR**
- METR's time-horizons page lists Claude 3.7 Sonnet, Opus 4.1, Sonnet 4.5, Opus 4.5, Opus 4.6 (added 20 Feb 2026) and Claude Mythos Preview (early) (added 8 May 2026); page last updated 8 Sep 2026 and states it is no longer actively updated; "Measurements above 16 hrs are unreliable with our current task suite"; runs have token and time limits; scaffolds differ by model (ReAct, Triframe, Claude Code, Codex) [P] — [METR time horizons](https://metr.org/time-horizons/)
- Numeric horizons were not readable from the page (loaded from a data file). Secondary figures: Opus 4.6 50% horizon ~12 h (718.8 min) per one reading of METR's data file, ~14.5 h per another write-up; Mythos Preview ~17.4 h at 50% and ~3.1 h at 80%, CI 509–3,304 min [2nd, conflicting] — [OfficeChai](https://officechai.com/ai/exponential-progress-claude-opus-4-6-has-50-time-horizon-of-14-5-hours-on-metr-time-horizons-benchmark); [FourWeekMBA](https://fourweekmba.com/ai-metr-tested-ai-at-17-hours-viral-240-hour-mark-is-estimated/)
- METR note (20 Mar 2026): as the suite saturates, results are sensitive to analysis choices; Opus 4.6's 50% horizon falls ~40% when public tasks are excluded [S] — [METR note](https://metr.org/notes/2026-03-20-impact-of-modelling-assumptions-on-time-horizon-results/)
- METR cautions that a 50% horizon of X hours does not mean tasks under X hours can be delegated [S] — [METR limitations note](https://metr.org/notes/2026-01-22-time-horizon-limitations/)
- Time horizon is measured in human-task-time, not agent wall-clock or tokens, and METR does not report results by context-management strategy [P] — [METR time horizons](https://metr.org/time-horizons/)

**Vending-Bench (Andon Labs)**
- Original paper (arXiv 2502.15840, Feb 2025; Claude 3.5 Sonnet, o3-mini etc.): performance often deteriorates over long horizons though each subtask is simple; all models have runs that derail ("meltdowns", e.g. a Claude 3.5 Sonnet run trying to contact the FBI); high variance [S] — [arXiv 2502.15840](https://arxiv.org/abs/2502.15840)
- Secondary reading of the paper: breakdowns were not tied to the point where memory/context became full [2nd; not verified against paper text] — [The Decoder](https://the-decoder.com/?p=21804)
- Epoch AI review of Vending-Bench 2 (21 Oct 2026, verdict "Flawed"): default harness gives a 69,000-token window counted with tiktoken (encrypted reasoning counted as text, ~43% of Claude Opus 5.5's window; effective limit lower), and on overflow deletes the oldest ~39% of messages with no summary — this happened 114 times in an Opus 5.5 run [P] — [Epoch review](https://epoch.ai/benchmarks/vending-bench-2/review)
- Epoch elicitation test (one run per agent): replacing deletion with auto-compaction at 900K tokens for Claude Opus 5.5 → final balance fell 44% ($9,459 → $5,254), which Epoch attributes to a worse strategy ("Context had no discernible impact"); GPT-6 Astra with compaction at 250K rose 17% ($15,366 → $18,044) [P] — [Epoch review](https://epoch.ai/benchmarks/vending-bench-2/review)

**Long-running demos / harness reports (Anthropic, self-reported)**
- C compiler (Feb 2026): 16 parallel Claude Opus 4.6 agents, ~2,000 Claude Code sessions over ~2 weeks, ~100K lines of Rust, compiles the Linux kernel; ~$20K API cost per The Register; continuity through repo, tests, and progress files rather than conversation memory [S] — [Anthropic engineering](https://anthropic.com/engineering/building-c-compiler); [The Register](https://www.theregister.com/2026/02/09/claude_opus_46_compiler/)
- "Harness design for long-running application development" (Anthropic engineering, ~Mar–Apr 2026): states models lose coherence on lengthy tasks as the window fills and some show "context anxiety"; compaction preserves continuity but "doesn't give the agent a clean slate", so context resets with structured handoff were needed for Sonnet 4.5; per secondary summaries, Opus 4.5 could run as one session with auto-compaction and Opus 4.6 allowed dropping the sprint/reset structure [S for quotes; 2nd for model progression] — [Anthropic engineering](https://www.anthropic.com/engineering/harness-design-long-running-apps); [InfoQ](https://www.infoq.com/news/2026/04/anthropic-three-agent-harness-ai)
- SWE-bench Pro (above): with a 50-turn cap, context overflow was Claude Sonnet 4's top failure mode (35.6% per prose) — [arXiv 2509.16941](https://arxiv.org/html/2509.16941)

### Inferences
- "Hours of coherent work" in METR and in Anthropic's demos is achieved with scaffolds that manage context (compaction, resets, file-based memory); the numbers do not show that a single uncompacted context stays coherent that long.
- Anthropic's own narrative is that sensitivity to context fill has decreased generation to generation (Sonnet 4.5 → Opus 4.5 → Opus 4.6), which is a qualitative vendor claim without published numbers.
- Epoch's single-run Vending-Bench result is too thin to conclude compaction helps or hurts Claude.

### Gaps
- Exact METR horizon values and CIs for Claude models were not verified from METR's primary data; secondary sources disagree (12 h vs 14.5 h for Opus 4.6).
- No METR or Terminal-Bench breakdown by context-management strategy found. Terminal-Bench was not researched beyond its use in arXiv 2609.32961.
- No time-horizon figures located for Claude models released after Mythos Preview (e.g. Opus 5.5, Fable-series); METR's page stopped updating Sep 2026.

---

## 4. Evidence quantifying the cost of compaction, and the benefit of fresh-context sub-agents / multi-agent orchestration

### Takeaway
Compaction's measured cost on benchmark scores is near zero for 1M-window models, but probe-based and constraint-tracking studies show real information loss: ~19–25% of user constraints vanish from a Claude Code compaction summary (process and scope rules ~36%), and file/artifact tracking scores only ~2.2–2.5 out of 5 after compression. Multi-agent benefits are large in Anthropic's research eval (+90.2%) at ~15x token cost, but that is a research task, not coding.

### Cited Findings

**"Benchmarks Are Surprisingly Compaction-Friendly" (arXiv 2610.31778, Oct 2026; Salle, Bhatnagar, Bethge, Abdelnabi, Andriushchenko — ELLIS/MPI/Tübingen). Scaffold: OpenCode. Models: Claude Opus 4.6 (1M), GPT-5.4 (1.05M), GPT-5.3-Codex (400K), Gemini 3.1 Pro (1M). Benchmarks: PaperBench (3 papers, 10 h), ProgramBench (2 tasks, 6 h), PostTrainBench.**
- Compaction thresholds 160K–380K tokens: Opus 4.6 and GPT-5.4 task scores essentially flat across thresholds; GPT-5.3-Codex on PaperBench ~0.38 at 160K vs ~0.60 at ≥260K [P] — [arXiv 2610.31778](https://arxiv.org/html/2610.31778v1)
- No alternative strategy (tool-output pruning, filesystem offloading, dropping reasoning blocks, small-model summaries by Claude Haiku 4.5) reliably beat default self-summarization. Haiku-written summaries left Opus 4.6 within its CI but dropped GPT-5.4 0.59 → 0.43 and GPT-5.3-Codex 0.38 → 0.18 [P] — [arXiv 2610.31778](https://arxiv.org/html/2610.31778v1)
- Constraint erosion (ProgramBench, 3 anti-cheating rules stated at start): rules dropped per compaction — Opus 4.6 0.51 of 3, GPT-5.4 1.76, GPT-5.3-Codex 2.73; loss compounds across compactions (average ~0.92 dropped at 1st compaction, 1.93 by 10th, 2.41 by 15th, 2.65 by 30th) because summaries are built from earlier summaries [P] — [arXiv 2610.31778](https://arxiv.org/html/2610.31778v1)
- Real Claude Code sessions (SWE-chat dataset): 44 sessions with ≥1 compaction, 269 user constraints; 81% preserved in the next compaction summary, 19% dropped (24.5% with a second judge). Feature requirements dropped 6.2%; process instructions 35.9%; scope exclusions 36.8% [P] — [arXiv 2610.31778](https://arxiv.org/html/2610.31778v1)
- Authors' conclusion: benchmarks are "compaction-friendly" because the objective is fixed and progress persists on disk; compaction can still silently remove behavioral/integrity rules that scores do not measure. Limitations: few tasks, one scaffold, two runs per config [P] — [arXiv 2610.31778](https://arxiv.org/html/2610.31778v1)

**Factory.ai — "Evaluating Context Compression" (16 Dec 2025; vendor study, GPT-5.2 judge)**
- 36,611 messages from production coding sessions; probes (recall, artifact, continuation, decision) asked after compression. Overall quality (0–5): Factory 3.70, Anthropic (Claude SDK compaction) 3.44, OpenAI 3.35. Accuracy 4.04 / 3.74 / 3.43. Artifact trail (which files were touched) 2.45 / 2.33 / 2.19 — weakest dimension for all, called "an unsolved problem" [P] — [Factory](https://factory.com/news/evaluating-compression)
- Compression removed 98.6–99.3% of tokens; lost details were typically file paths, endpoints, error messages; authors argue compression ratio is the wrong metric because lost details force re-fetching [P] — [Factory](https://factory.com/news/evaluating-compression)

**Anthropic context editing + memory tool (announcement 29 Sep 2025; internal eval, self-reported)**
- On an internal agentic-search eval, memory tool + context editing improved performance 39% over baseline; context editing alone 29%. In a 100-turn web-search eval, context editing let agents finish workflows that otherwise failed from context exhaustion and cut token consumption 84% [S] — [Anthropic](https://www.anthropic.com/news/context-management)

**Sub-agents / multi-agent**
- Anthropic multi-agent research system (13 Jun 2025): Claude Opus 4 lead + Claude Sonnet 4 subagents beat single-agent Opus 4 by 90.2% on an internal research eval; agents use ~4x the tokens of chat, multi-agent ~15x; token usage alone explained 80% of performance variance (with tool-call count and model choice as the other factors). Anthropic notes most coding tasks are less parallelizable and a poorer fit [S] — [Anthropic engineering](https://www.anthropic.com/engineering/multi-agent-research-system)
- Anthropic advanced tool use (24 Nov 2025): keeping intermediate tool results out of the model's context via programmatic tool calling cut average tokens 43,588 → 27,297 (−37%) and raised accuracy on internal knowledge retrieval 25.6% → 28.5% and GIA 46.5% → 51.2% [P] — [Anthropic engineering](https://www.anthropic.com/engineering/advanced-tool-use)
- Trajectory-deletion study: agents re-fetch deleted content, a hidden cost of forgetting [P, abstract] — [arXiv 2610.35496](https://arxiv.org/abs/2610.35496)

### Inferences
- The most decision-relevant number for Claude Code users: roughly one in five user-stated constraints, and roughly one in three "how to work" / "don't touch X" instructions, do not survive into a compaction summary; with repeated compactions the loss compounds. Instructions that live in files (CLAUDE.md, plan files) are not subject to this.
- Opus 4.6 was the most robust model tested on both constraint retention and tolerance to cheap summaries, so compaction cost is model-dependent and seems to be shrinking for Claude — but nonzero.
- Sub-agent evidence shows that isolating exploration in separate contexts raises quality on breadth-first research; no controlled coding benchmark was found that isolates "fresh sub-agent context vs. single long context" for Claude Code.

### Gaps
- No public before/after task-success measurement for Claude Code's own `/compact` or auto-compact found (only the summary-content audit above).
- No controlled study found of Claude Code sub-agents vs. single-agent on a coding benchmark.
- A "May 2026 Factory benchmark" cited by a secondary blog could not be confirmed.
- The +39%/+29%/−84% and +90.2% figures are Anthropic internal evals with undisclosed task sets.

---

## 5. Does tool-definition / MCP bloat measurably hurt tool selection?

### Takeaway
Yes, in the published tests: loading all tool schemas upfront both consumes tens of thousands of tokens and lowers tool-selection accuracy; on-demand tool discovery raised Claude Opus 4 from 49% to 74% and Opus 4.5 from 79.5% to 88.1% on Anthropic's MCP evals. The gap narrows with newer models but did not vanish.

### Cited Findings
- Anthropic (24 Nov 2025): typical tool-definition footprints — GitHub MCP 35 tools ≈ 26K tokens; Slack 11 tools ≈ 21K; five servers (58 tools) ≈ 55K; Jira alone ≈ 17K; Anthropic has seen 134K tokens of tool definitions before optimization [P] — [Anthropic engineering](https://www.anthropic.com/engineering/advanced-tool-use)
- Tool Search Tool (deferred loading): ~72K tokens of definitions for 50+ MCP tools reduced to ~500 tokens upfront + ~3K for 3–5 discovered tools (~8.7K total); 85% token reduction; accuracy on MCP evaluations Opus 4 49% → 74%, Opus 4.5 79.5% → 88.1% [P] — [Anthropic engineering](https://www.anthropic.com/engineering/advanced-tool-use)
- Tool-use examples raised accuracy on complex parameter handling 72% → 90% (internal) [P] — [Anthropic engineering](https://www.anthropic.com/engineering/advanced-tool-use)
- RAG-MCP (arXiv 2505.03275, May 2025; not Claude-specific): retrieving relevant tools instead of listing all cut prompt tokens >50% and raised tool-selection accuracy to 43.13% vs 13.62% baseline [S, abstract] — [arXiv 2505.03275](https://arxiv.org/abs/2505.03275)
- Vendor counter-tests at very large scale (competitors; different conditions): Stacklok reports Anthropic's Tool Search Tool selecting correctly 34% of the time across 2,792 tools (vs 94% for its own product); Arcade reports 56% (regex) / 64% (BM25) retrieval accuracy across 4,027 tools [S; vendor-affiliated] — [Stacklok](https://stacklok.com/blog/stackloks-mcp-optimizer-vs-anthropics-tool-search-tool-a-head-to-head-comparison/); [Arcade](https://www.arcade.dev/blog/anthropic-tool-search-claude-mcp-runtime)

### Inferences
- With Opus 4.5 the penalty for loading everything upfront was ~8.6 pp (79.5 vs 88.1), versus 25 pp for Opus 4 — bloat sensitivity falls with model strength, but the token overhead (25–35% of a 200K window in Anthropic's examples) remains.
- Deferred tool loading shifts the failure mode from "wrong tool chosen" to "tool not found by search" at thousands-of-tools scale.

### Gaps
- No independent, peer-reviewed 2026 study of tool-count vs. accuracy on current Claude models found; claims like "accuracy degrades past 10–15 tools" appear unsourced in secondary blogs.
- A reported RAG-MCP baseline drop "85% with 5 tools → 45% with 20 tools" could not be verified against the paper.
- Anthropic's MCP eval set and tool counts per test are not published.

---

## 6. 2026 papers on 1M-token context models in agentic coding; "bigger window" vs "better context management"

### Takeaway
The only direct test found (arXiv 2610.31778, Oct 2026) indicates that for 1M-window models (Claude Opus 4.6, GPT-5.4) it made no measurable difference to benchmark scores whether compaction fired at 160K or 380K tokens, while a 400K-window model suffered when compacted early. There is no evidence in these sources that filling a 1M window improves agentic coding outcomes over managed context, and some evidence that managed context matches it at lower cost.

### Cited Findings
- Opus 4.6 (1M) and GPT-5.4 (1.05M) in OpenCode: scores flat across compaction thresholds 160K–380K (16–38% of window) on PaperBench and ProgramBench; GPT-5.3-Codex (400K): ~0.38 at 160K vs ~0.60 at ≥260K on PaperBench; filesystem offloading kept 1M models near baseline but dropped GPT-5.3-Codex 0.38 → ~0.24 [P] — [arXiv 2610.31778](https://arxiv.org/html/2610.31778v1)
- The study did not test thresholds above 380K, i.e. it does not measure performance when a 1M window is actually near full [P] — [arXiv 2610.31778](https://arxiv.org/html/2610.31778v1)
- Gemini 2.5 Flash (1M window) on SWE-bench Verified: raw full-history agent 32.8% vs 35.6% with only the last 10 observations kept, at 44% of the cost (non-thinking); with thinking enabled, raw was better (40.4% vs 36.4%) [P] — [arXiv 2508.21433v3](https://arxiv.org/html/2508.21433v3)
- AutoCompact authors report proactive compaction helps even when the full history would fit (39.6% vs 30.4% SWE-bench Verified; model details not verified) [S] — [arXiv 2610.02163](https://arxiv.org/abs/2610.02163)
- Epoch's Vending-Bench 2 test gave Claude Opus 5.5 a 900K compaction threshold instead of a 69K sliding window; score fell 44% in a single run, attributed by Epoch to strategy variance rather than context [P] — [Epoch review](https://epoch.ai/benchmarks/vending-bench-2/review)
- Other 2026 long-horizon coding benchmarks surfaced but not read: SWE-Marathon ([arXiv 2606.07682](https://arxiv.org/html/2606.07682v1)), DeepSWE ([arXiv 2607.07946](https://arxiv.org/pdf/2607.07946)), SWE-INTERACT ([arXiv 2606.30573](https://arxiv.org/pdf/2606.30573)), SWE-EVO ([arXiv 2512.18470](https://arxiv.org/pdf/2512.18470)), "An Empirical Study of Harness Design for Coding Agents" ([arXiv 2609.20804](https://arxiv.org/pdf/2609.20804)) [S, titles only]

### Inferences
- For current 1M-window Claude models, *when* compaction happens (within the first ~40% of the window) appears not to matter for task score; what compaction *drops* (user constraints) matters more than the token threshold.
- Larger windows mainly help weaker-at-compaction models avoid frequent lossy compactions; they are a tolerance margin rather than a demonstrated accuracy gain.
- The open empirical question is the 400K–1M range: no public agentic-coding measurement of Claude at 50–100% of a 1M window was found.

### Gaps
- No paper found that directly compares "run to ~1M without compaction" against "compact early" for Claude on a coding benchmark.
- No public data on Claude Code [1m] sessions' success rate by context fill.
- The unread 2026 benchmarks above may contain relevant per-length breakdowns.
