# Practitioner and community evidence: Claude Code quality as the context window fills (state as of 25 Nov 2026)

Evidence labels used throughout: **[OFFICIAL]** = Anthropic docs/staff; **[MEASURED]** = a described experiment or benchmark with numbers; **[ANECDOTE]** = individual practitioner report; **[FOLKLORE]** = widely repeated figure with no traceable test; **[VENDOR]** = from a company selling a related product.

Research limits: about 25 tool calls. Sources I read directly are cited as such; where I only saw a search-engine summary of a page I say "via search summary". I did not reach Reddit threads, X/Twitter posts, or YouTube transcripts directly.

## 1. What rules of thumb circulate, who originated them, and what backs each?

### Takeaway
Every widely cited threshold (40%, 40-60%, 70%, ~150K, 100K, 300-400K) traces back to practitioner judgment or to a model's self-report, not to a published test of Claude Code quality versus fill level. The most rigorous-sounding ones borrow authority from general long-context research (Chroma "Context Rot", NoLiMa) that did not test Claude Code.

### Cited Findings

**"Keep utilization at 40-60%" / "frequent intentional compaction" / research-plan-implement (Dex Horthy, HumanLayer, ~Aug 2025; Claude Opus 4 / Sonnet 4 era, 200K window)** [ANECDOTE, case studies]
- The HumanLayer write-up says the workflow is about "keeping utilization in the 40%-60% range (depends on complexity of the problem)" and defines frequent intentional compaction as "designing your ENTIRE WORKFLOW around context management." — [HumanLayer ace-fca.md](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents/blob/main/ace-fca.md)
- Evidence offered is case-study, not a fill-level test: a bug-fix PR to the ~300k LOC Rust BAML codebase approved the next morning; ~35k LOC of cancellation/WASM work in ~7 hours (3 research/plan, 4 implement) versus a team estimate of 3-5 senior-engineer days each; one acknowledged failure (parquet-java, 7 hours, "it did not go well"); team of three "averaging about $12k on opus per month." — [HumanLayer ace-fca.md](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents/blob/main/ace-fca.md)
- The document states "Subagents are about context control", not role-play. — [HumanLayer ace-fca.md](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents/blob/main/ace-fca.md)
- The phrase "dumb zone" does NOT appear in that written document. It is attributed to Horthy's later AI Engineer talk ("No Vibes Allowed" / "Context engineering for complex codebases", AI Engineer Code 2025); secondary write-ups say he puts diminishing returns "around 40%" and stresses it varies by model and task complexity. I saw this via search summary only, not the transcript. — [AI Engineer talk page](https://ai.engineer/talks/context-engineering-for-complex-codebases); [Pragmatic Engineer interview with Dex Horthy](https://newsletter.pragmaticengineer.com/p/context-engineering-with-dex-horthy)
- Secondary sources disagree on where the "dumb zone" starts: "some report that the 'dumb zone' threshold starts at 60k tokens, others push it as far as 600k" (via search summary). — [Towards AI](https://pub.towardsai.net/the-truth-about-huge-llms-context-windows-912b36e8d6d6)
- Propagation example: HN user MikeNotThePope (thread on 1M GA, ~March 2026): "I try to keep usage under 40%, or about 80k tokens, to avoid what Dex Horthy calls the dumb zone." — [HN 47367129](https://news.ycombinator.com/item?id=47367129)

**"~170k of usable context", "one thing per loop", fresh context every iteration (Geoffrey Huntley, Ralph, 14 Jul 2025)** [ANECDOTE]
- "The name of the game is that you only have approximately 170k of context window to work with." No model named, no measurement method given. — [ghuntley.com/ralph](https://ghuntley.com/ralph/)
- "To get good outcomes with Ralph, you need to ask Ralph to do one thing per loop." — [ghuntley.com/ralph](https://ghuntley.com/ralph/)
- A linked Huntley post preview says "I've noticed that the quality of output clips at the 147k-152k mark" against Claude 3.7's advertised 200k. Personal observation, no method described. — [ghuntley.com/ralph](https://ghuntley.com/ralph/)
- "Ralph requires a mindset of not allocating to the primary context window. Instead, what you should do is spawn subagents"; the primary window "should operate as a scheduler." The post never mentions compaction. — [ghuntley.com/ralph](https://ghuntley.com/ralph/)
- HumanLayer quotes the same 170k line, linking the two lineages. — [HumanLayer ace-fca.md](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents/blob/main/ace-fca.md)

**"Never compact; hand off instead" (Amp, 23 Oct 2025)** [VENDOR, ANECDOTE]
- Amp removed compaction and replaced it with Handoff: compaction is "lossy", and "compaction, we found, encourages long, meandering threads"; "What we want to encourage are focused threads, because we think that's how agents yield the best results." No data, benchmark or user study is cited. No author named on the page (Thorsten Ball not confirmed as author). — [ampcode.com/news/handoff](https://ampcode.com/news/handoff)
- A later Product Hunt listing for "Neo by Amp" reportedly describes "automatic compaction instead of manual handoff", suggesting Amp's stance shifted (search summary only; unverified). — [Product Hunt](https://www.producthunt.com/p/amp-free/neo-by-amp)
- Quote attributed to Dan Mac (Amp): "You should basically never use compaction." (quoted second-hand). — [Albert Sikkema, 23 Apr 2026](https://www.albertsikkema.com/ai/development/tools/2026/04/23/smaller-context-window-better-claude-code.html)

**"New task = new session", rewind over correction, steer /compact (Thariq Shihipar, Anthropic Claude Code team, 15 Apr 2026)** [OFFICIAL guidance, no numbers]
- "Context rot is the observation that model performance degrades as context grows"; "when you start a new task, you should also start a new session"; "Rewind is often the better approach to correction"; "bad compacts can happen when the model can't predict the direction your work is going"; because of context rot "the model is at its least intelligent point when compacting." — [claude.com blog](https://claude.com/blog/using-claude-code-session-management-and-1m-context)
- CONFLICT on thresholds attributed to this article: one secondary summary says Thariq puts onset "around 300,000 to 400,000 tokens in the 1M model" (not a strict rule); Sikkema says Thariq "suggests proactive compaction at 50-60% capacity". My direct read of the article (via a summarizing fetch) found no token threshold in it. Another guide reportedly posted a correction dated 9 Oct 2026 saying "Anthropic publishes no context rot threshold" and withdrew the 300-400k estimate (search summary only; I could not locate that correction on the page I fetched). Treat any numeric threshold "from Anthropic" as unverified. — [search summary of weihaoqu wiki](https://weihaoqu.github.io/learnAIDoc/wiki/claude-code-session-management-1m/); [Sikkema](https://www.albertsikkema.com/ai/development/tools/2026/04/23/smaller-context-window-better-claude-code.html); [claude.com blog](https://claude.com/blog/using-claude-code-session-management-and-1m-context)

**"Compact at 70%" / "compact at 30%" / "stay under 100k"** [FOLKLORE / ANECDOTE]
- Sikkema sets `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE=70` on a 200K-capped window (~140k trigger); his own preference, not tested. — [Sikkema](https://www.albertsikkema.com/ai/development/tools/2026/04/23/smaller-context-window-better-claude-code.html)
- yurukusa (GitHub, Mar 2026, Opus 4.6 1M, "hundreds of sessions daily"): compact at ~30% rather than 50%; estimated effective high-quality context 300-400K with heavy tool use; on the 200K model saw instruction-following degrade past ~150K. Self-reported, no method. — [anthropics/claude-code#34685](https://github.com/anthropics/claude-code/issues/34685)
- Dan Geabunea (12 May 2026, Sonnet 4.6 / Opus 4.7): "80k-100k tokens seems to be the optimal ceiling for high-stakes reasoning work", based on "the data and my practical experience using Claude Code in enterprise Java systems"; leans on the Chroma study, no experiment of his own; says thresholds "are not hard limits". — [dangeabunea.com](https://www.dangeabunea.com/blog/why-claude-code-gets-worse-the-longer-you-use-it-and-how-to-fix-it)
- Blog claims of "output quality noticeably drops at 60% capacity" and "degradation beginning measurably around 400K tokens, retrieval unreliable past 600K on Sonnet 4.6" exist but conflict and are not backed by a described method (search summary). — [note.com Hack-Log](https://note.com/hacklog_stealth/n/nc3fbbd11c496?hl=en); [Verdent guide](https://www.verdent.ai/guides/claude-code-1m-context-window)

### Inferences
- The 40% figure has drifted from "40-60% depending on complexity" (a working band on a 200K window, i.e. roughly 80-120K tokens) into a hard "40% = dumb zone" line. Applied to a 1M window, 40% is 400K tokens, a very different absolute amount; users who carried the percentage over (see issue #34685) may be reasoning from the wrong unit.
- Absolute token counts cited by independent practitioners on 200K-era models cluster around 80K-170K (Horthy ~80-120K, Geabunea 80-100K, yurukusa ~150K, Huntley 147-170K). That clustering is agreement among anecdotes, not a measurement.

### Gaps
- I did not verify any Boris Cherny or Simon Willison statements on context-fill thresholds; no primary source surfaced in my searches. Do not attribute thresholds to them from these notes.
- Did not read the Horthy talk transcript, so the exact "dumb zone" wording and whether he presents any data is unconfirmed.
- Origin of "compact at 70%" as a community rule was not traced to a first author.

## 2. Independent measurements of Claude Code quality vs. context fill

### Takeaway
I found no rigorous, published experiment that holds a coding task fixed and varies Claude Code's context fill level. What exists is adjacent: general long-context benchmarks (Chroma, NoLiMa, MRCR), one arXiv paper comparing a different scaffold against Claude Code, and small single-user experiments.

### Cited Findings
- [MEASURED, adjacent] arXiv "LCM: Lossless Context Management" compared a system called Volt with Claude Code on long-context tasks from 8K to 1M tokens: parity at 8K and 16K, Volt ahead from 32K up; raw Opus 4.6 without scaffold "showed steep degradation beyond 65K tokens" (search summary; paper not read in full, and the authors are promoting their own system). — [arXiv 2605.04050](https://arxiv.org/pdf/2605.04050)
- [MEASURED, vendor-reported, retrieval not coding] Opus 4.6 scores cited at 76% / 78.3% (sources differ) on Anthropic's 8-needle MRCR v2 at 1M tokens; Sonnet 4.5 cited at 18.5% at 1M (search summaries of third-party posts). — [getunblocked.com context rot](https://getunblocked.com/blog/context-rot-claude-code/); [claudecodecamp](https://www.claudecodecamp.com/p/claude-code-1m-context-window)
- [MEASURED, general LLM, cited second-hand] NoLiMa: 11 of 12 models fell below 50% of short-context performance at 32k tokens; JetBrains Research: observation masking cut cost 52% and raised solve rate 2.6%; Chroma "Context Rot" evaluated 18 LLMs varying input length with task complexity constant. None used Claude Code as the harness. — [Sikkema](https://www.albertsikkema.com/ai/development/tools/2026/04/23/smaller-context-window-better-claude-code.html); [dangeabunea.com](https://www.dangeabunea.com/blog/why-claude-code-gets-worse-the-longer-you-use-it-and-how-to-fix-it)
- [ANECDOTE, n=1] DEV experiment building the same Tic Tac Toe app four ways: as tokens increased, quality stayed flat or dropped; one app, one reviewer (search summary). — [dev.to middlebrooks314](https://dev.to/middlebrooks314/claude-code-experiment-more-tokens-doesnt-mean-better-code-4l4l)
- [ANECDOTE, circumstantial] hyperdev: when Claude Code showed "10% context remaining until auto-compact", the session had used about 64% of the window (search summary). — [hyperdev.matsuoka.com](https://hyperdev.matsuoka.com/p/how-claude-code-got-better-by-protecting)
- [NOT RELEVANT, despite the name] Bicameral's Claude Code "Code Mode" benchmark notes none of its tests runs long enough to fill a large window. — [Bicameral-AI/claude-code-mode-benchmark](https://github.com/Bicameral-AI/claude-code-mode-benchmark)
- [SELF-REPORT, not a measurement] In issue #34685 the model itself offered thresholds (0.4-0.5 noticeable, 0.6 "noticeably worse", 0.8+ "gets rough"). These are the model's guesses about itself. — [#34685](https://github.com/anthropics/claude-code/issues/34685)

### Inferences
- Fill-band tables in vendor blogs (e.g. 0-25% / 25-50% / 50-75% quality bands) should be treated as illustrative, since no experiment is cited for them.
- Retrieval benchmarks (needle tasks) likely understate or mismeasure what practitioners complain about, which is instruction adherence and multi-step reasoning in tool-heavy transcripts.

### Gaps
- No telemetry analysis from Anthropic or a third party relating fill level to task success was found.
- No GitHub repo with a reproducible fill-level harness for Claude Code was found.

## 3. Symptoms reported at high fill, and GitHub issues

### Takeaway
The symptom list is consistent across reports (lost decisions, redo/revert loops, ignored CLAUDE.md rules, false "fixed" claims, post-compact amnesia, premature wrap-up), but all of it is anecdotal; the key issues were auto-closed as stale with no Anthropic staff reply. Current docs say project-root CLAUDE.md is re-injected after compaction, which dates many older "forgets CLAUDE.md" issues.

### Cited Findings
- [ANECDOTE] Issue #34685 (opened 15 Mar 2026 by mannewalis, `claude-opus-4-6[1m]`): at ~20% fill lost track of earlier decisions, circular reasoning; at ~40% compaction fired; at ~48% Claude recommended a new session. Symptoms: applying, reverting and re-applying the same fix; falsely claiming fixes worked (checked via screenshots); re-deriving architecture decisions wrongly. Session had hundreds of tool calls plus MCP browser screenshots. ~17 comments, 6 thumbs-up; no Anthropic reply; closed "not planned" by bot 30 May 2026. — [#34685](https://github.com/anthropics/claude-code/issues/34685)
- [ANECDOTE] Same thread: mannewalis got "Context limit reached" at 18% then 16% shown usage; kienmarkdo (28 Apr 2026) saw degradation at ~47% on Opus 4.6 [1m]; internationils found sessions under 200K of 1M "worked normally". — [#34685](https://github.com/anthropics/claude-code/issues/34685)
- [ANECDOTE] Issue #40547 (29 Mar 2026, Opus 4.6 1M): misread clear instructions, excessive clarifying questions, confirmation loops, redo work; no numbers; 3 thumbs-up, no Anthropic reply, bot-closed 10 May 2026. Commenter theories (4.6-1M "is actually 4.5"; caching reduces attention) are unverified. — [#40547](https://github.com/anthropics/claude-code/issues/40547)
- [ANECDOTE, many reports] Cluster of issues on CLAUDE.md being ignored after `/compact` or auto-compact: #4017, #6354, #1027, #9796, #11545, #14890, #15258, #16106, #23063, #24460, #24572 (seen via a mirror site; statuses mixed, several closed as duplicates). — [mirror of #4017](https://claudeissues.com/issue/4017-bug-compact-causes-claude-code-to-ignore-claude-md); [mirror of #14890](https://claudeissues.com/issue/14890-bug-claude-code-ignores-claude-md-protocols-during-auto-compact-operations); [mirror of #24460](https://claudeissues.com/issue/24460-claude-md-context-lost-after-compact-re-inject-project-instructions)
- [OFFICIAL] Current docs: after compaction, project-root CLAUDE.md, unscoped rules, auto memory and the plan-mode plan are "re-injected from disk"; skill descriptions rebuilt (v2.1.304+); up to five recently modified files re-read (files over 5,000 tokens come back as a path reference); invoked skill bodies capped at 5,000 tokens each / 25,000 total. Path-scoped rules and nested CLAUDE.md are summarized away until re-triggered; context added by hooks is summarized. — [code.claude.com context-window](https://code.claude.com/docs/en/context-window.md)
- [ANECDOTE] Post-compact loss of intent: after mid-task auto-compaction Claude committed all uncommitted files unasked, having lost that no commit was requested (Opus 4.6). — [Sikkema](https://www.albertsikkema.com/ai/development/tools/2026/04/23/smaller-context-window-better-claude-code.html)
- [ANECDOTE] HN: roygbiv2 "I've found compactation kills the whole thing"; 8note "something about compaction has been dropping everything that might be useful"; contrary view garciasn "Now it rarely gets dumb after compaction like it did before." — [HN 47367129](https://news.ycombinator.com/item?id=47367129)
- [VENDOR OBSERVATION, no numbers] "Context anxiety" (Cognition, 29 Sep 2025, Sonnet 4.5 in Devin, not Claude Code): the model is aware of its window and takes shortcuts or leaves tasks incomplete near the perceived end; "the model consistently underestimates how many tokens it has left—and it's very precise about these wrong estimates"; enabling the 1M beta but capping use at 200k removed the behaviour; reminders were needed at both start and end of the prompt; self-written notes were not reliable enough to replace their own memory management. — [Cognition blog](https://cognition.com/blog/devin-sonnet-4-5-lessons-and-challenges)

### Inferences
- "Forgets CLAUDE.md after compact" reports from 2025 to early 2026 may describe behaviour that the harness has since changed (root CLAUDE.md re-injection); remaining exposure is in path-scoped rules, nested CLAUDE.md, hook-added context, and large files/skills that exceed re-injection caps. I did not find a changelog entry dating the change.
- Screenshots and other image/tool-output-heavy sessions appear over-represented in the worst reports, suggesting transcript composition matters as much as fill percentage.

### Gaps
- No issue with a reproducible test case for hallucinated file contents at high fill was found.
- No evidence on whether "context anxiety" persists in 2026 models inside Claude Code.

## 4. 1M-token context in Claude Code: which models, when, and what users report

### Takeaway
1M went GA for Opus 4.6 and Sonnet 4.6 around March 2026 and is the default (no suffix) for Opus 4.7 and later, Sonnet 5 and later, Haiku 5.5 and Fable models, with auto-compact at about 967K. User reports on Opus 4.6 1M are split: a vocal group says quality falls off at the same absolute token counts as before (so far earlier in percentage terms), while others say they run 500K+ daily without a "dumb zone". I found almost no dated, model-specific user evidence for the models released after Opus 4.7.

### Cited Findings
- [OFFICIAL] "Fable models, Sonnet 5 and later, Haiku 5.5, and Opus 4.7 and later run with the 1M window by default"; Opus 4.6 and Sonnet 4.6 reach 1M only via `[1m]`, plan-dependent (Opus 4.6 1M included on Max/Team/Enterprise; Pro needs usage credits). No premium for tokens beyond 200K except Haiku 5.5 above 100K. — [code.claude.com model-config](https://code.claude.com/docs/en/model-config.md)
- [OFFICIAL] Native-1M models "compact before the window fills, at about 967K tokens by default"; `CLAUDE_CODE_DISABLE_1M_CONTEXT=1` caps at 200K; `/autocompact 500k`, `autoCompactWindow`, `--autocompact`, `CLAUDE_CODE_AUTO_COMPACT_WINDOW` set an earlier trigger (minimum 100K). The status line's `used_percentage` is measured against the full window, so it does not indicate when compaction runs if a smaller window is set. — [code.claude.com model-config](https://code.claude.com/docs/en/model-config.md)
- [OFFICIAL] Alias history: `opus` moved to Opus 4.8 (v2.1.154), Opus 5 (v2.1.219), Opus 5.5 (v2.1.280); `sonnet` to Sonnet 5 (v2.1.197), 5.5 (v2.1.284); `fable` to Fable 5.2 (v2.1.321). — [code.claude.com model-config](https://code.claude.com/docs/en/model-config.md)
- [ANECDOTE, negative] mannewalis: the 1M model "seems barely more capable at around 20% than the previous model at 75%"; called it "a longer progress bar"; switched back to 200K Opus. Tudor-MS compacts or clears at 20% (200K). — [#34685](https://github.com/anthropics/claude-code/issues/34685)
- [ANECDOTE, negative] HN (1M GA thread, ~Mar 2026): one user says 90k tokens gives equally bad results in a 100k or 1M window; another puts the drop at 150-200k (search summary). — [HN 47367129](https://news.ycombinator.com/item?id=47367129)
- [ANECDOTE, positive] Same thread: virtualritz "I haven't hit the 'dumb zone' any more since two months"; hu3 "I use 500k+ context on these on a daily basis"; furyofantares "I still have not had the dumb zone"; wat10000 "It seems to remain very capable with large contexts at least in that scenario." — [HN 47367129](https://news.ycombinator.com/item?id=47367129)
- [ANECDOTE] Sikkema (23 Apr 2026, Opus 4.6): "The 1M context window is a capacity increase, not a quality increase"; capped back to 200K. Notes compaction on 1M discards far more at once (his arithmetic: ~400k vs ~80k tokens). — [Sikkema](https://www.albertsikkema.com/ai/development/tools/2026/04/23/smaller-context-window-better-claude-code.html)
- [OFFICIAL claim] Anthropic said Opus 4.6 tracks information over hundreds of thousands of tokens with less drift (search summary of launch post). — [anthropic.com Opus 4.6](https://anthropic.com/news/claude-opus-4-6)
- [ANECDOTE, bug] Issue #53327 (Opus 4.7, ~v2.1.120): picker showed 200K but session ran 1M, burning 18% of a 5-hour Max limit on a small prompt (search summary). — [#53327](https://github.com/anthropics/claude-code/issues/53327)
- [GUIDE, Aug 2026 update] wmedia tip updated for native-1M models still advises "room to fill is not a reason to fill it" and says lowering the window "doesn't save you anything, it just makes you compact more often"; gives no model-specific quality results. — [wmedia.es](https://wmedia.es/en/tips/claude-code-1m-context-window-tips)

### Inferences
- The disagreement between "degrades at 20-40% of 1M" and "fine at 500K+" is consistent with degradation tracking absolute tokens and transcript noise rather than percentage of window, but nobody has tested this.
- A default auto-compact at ~967K means a user who never intervenes will, on current default models, run far past every community threshold before the harness acts; early compaction is opt-in.

### Gaps
- No user reports found that specifically compare Opus 4.8 / 5 / 5.5, Sonnet 5 / 5.5 or Fable 5.x long-session quality with Opus 4.6; my searches returned only Opus 4.6 to 4.7 era material (Mar to May 2026). Whether newer models hold quality deeper into the window is unanswered by evidence here.
- Exact GA date of 1M for Opus 4.6/Sonnet 4.6 not confirmed from a primary source (HN thread shows only "8 months ago", i.e. ~March 2026).

## 5. Context consumed before the user types; MCP/tool bloat

### Takeaway
Anthropic's illustrative baseline is small (under ~8K tokens with MCP tool names deferred), but community reports for setups with several MCP servers range from ~15K to ~70K before the first prompt. The larger numbers are mostly second-hand or composite, and tool search / deferred loading has reduced the problem; `/context` is the only authoritative figure for a given setup.

### Cited Findings
- [OFFICIAL, illustrative] Example startup load: system prompt 4,200; auto memory 680; environment info 280; MCP tool names 120; skill descriptions 450; `~/.claude/CLAUDE.md` 320; project CLAUDE.md 1,800 (about 7,850 total). Docs say counts "are illustrative and vary"; run `/context` for real numbers. — [code.claude.com context-window](https://code.claude.com/docs/en/context-window.md)
- [OFFICIAL] In that timeline only MCP tool *names* load up front, i.e. schemas are deferred. — [code.claude.com context-window](https://code.claude.com/docs/en/context-window.md)
- [VENDOR, composite not measured] Unblocked (Dennis Pilarinos, 14 May 2026): seven-server composite reaching ~71,300 tokens (GitHub ~42,000; Playwright ~13,600; mcp-omnisearch ~14,000; others ~9,800; system prompt ~3,500; CLAUDE.md/rules ~2,400); also cites a "community-measured" 50,000-67,000 for 5-10 servers. No model, version, or method given; attribution of the 42K figure is internally inconsistent; the vendor sells a context product. — [getunblocked.com](https://getunblocked.com/blog/mcp-token-budget-autopsy/)
- [ANECDOTE, second-hand] After tool search, one production session reportedly fell from ~51K to ~8.5K main-thread tokens (Joe Njenga, cited by Unblocked; method not given). — [getunblocked.com](https://getunblocked.com/blog/mcp-token-budget-autopsy/)
- [ANECDOTE] 2025 issue #3406: ~11,600 tokens without MCP, ~15,000 with four MCP servers on first message (search summary of mirror). — [mirror of #3406](https://claudeissues.com/issue/3406-bug-built-in-tools-mcp-descriptions-load-on-first-message-causing-10-20k-token-o)
- [THIRD-PARTY FAQ, unverified against docs] Tool search "auto" defers MCP tool descriptions when they exceed 10% of the window (search summary). — [claudelog.com](https://claudelog.com/faqs/do-mcp-servers-use-claude-code-context/)
- [ANECDOTE] Other ranges quoted: 25,000-35,000 tokens for system prompt + tool schemas + MCP + CLAUDE.md (search summary). — [morphllm.com](https://www.morphllm.com/claude-code-token-limit)

### Inferences
- Pre-2026 "MCP eats 40-70K" figures describe eager schema loading; on current Claude Code with deferred tool loading they are likely overstated for the same server set, though the per-call cost of large tool results remains.
- As a fraction of a 1M default window, even a 70K preload is 7%, versus 35% of 200K; the bloat argument has shifted from "runs out of room" to "adds noise and cost per turn".

### Gaps
- No primary `/context` screenshots or dumps were read directly; Reddit threads with user `/context` outputs were not reached.
- No measured study of whether tool-definition bloat reduces task quality in Claude Code (as opposed to consuming tokens).

## 6. Mitigation workflows recommended and reported effective

### Takeaway
There is strong convergence between Anthropic's guidance and practitioners on the same handful of practices (fresh session per task, steered manual compaction or handoff files, plan files on disk, subagents for exploration, visible context meter). Reported effectiveness is anecdotal throughout; no controlled comparison of these workflows was found.

### Cited Findings
- [OFFICIAL] Docs list: `/compact` with a focus ("/compact focus on the auth bug fix"); `/rewind` then "Summarize from here / up to here"; `/autocompact <tokens>` to compact earlier; `/clear` when switching to unrelated work ("Old conversation crowds out the files you need next and costs tokens on every message"); delegate large reads to a subagent. — [code.claude.com context-window](https://code.claude.com/docs/en/context-window.md)
- [OFFICIAL] Docs example: a subagent consumed 9,890 tokens in its own window and returned a 420-token summary to the parent. — [code.claude.com context-window](https://code.claude.com/docs/en/context-window.md)
- [OFFICIAL] SessionStart hooks matching the `compact` source run after compaction and can re-inject context; plan-mode plans are re-injected from disk. — [code.claude.com context-window](https://code.claude.com/docs/en/context-window.md)
- [OFFICIAL] Thariq: new task = new session; rewind rather than correct; `/clear` "usually with a brief you've distilled"; subagent test is "will I need this tool output again, or just the conclusion?" — [claude.com blog](https://claude.com/blog/using-claude-code-session-management-and-1m-context)
- [ANECDOTE, case studies] Research → plan → implement with artifacts on disk, status compacted into the plan file after each verified phase, subagents for search/summarize. — [HumanLayer ace-fca.md](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents/blob/main/ace-fca.md)
- [ANECDOTE] Ralph loop: one item per iteration, fresh context each loop, primary context as scheduler, subagents do the allocation-heavy work. — [ghuntley.com/ralph](https://ghuntley.com/ralph/)
- [VENDOR] Amp Handoff: user states the goal, the agent drafts a new-thread prompt plus relevant file list. — [ampcode.com/news/handoff](https://ampcode.com/news/handoff)
- [ANECDOTE] Sikkema: `CLAUDE_CODE_DISABLE_1M_CONTEXT=1` + `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE=70`, separate sessions for research / planning / building, `/clear` between unrelated tasks: "Two environment variables and a habit of starting fresh. That is the whole trick." — [Sikkema](https://www.albertsikkema.com/ai/development/tools/2026/04/23/smaller-context-window-better-claude-code.html)
- [ANECDOTE] HN practices: plan first then implement in a fresh context with the plan saved to a file (girvo, Huppie, kaizenb); Huppie keeps implementing without reset "when context is still < 60% full"; progress tracked in a markdown file (roygbiv2, s900mhz); compact instructions in CLAUDE.md (comboy, brookst); task.md with checkable gates plus a hook showing the next open gate (visarga). — [HN 47367129](https://news.ycombinator.com/item?id=47367129)
- [ANECDOTE] yurukusa: checkpoint state to files (todo.md), delegate exploration to subagents, treat screenshots as "context killers"; also a `UserPromptSubmit` hook that injects a "QUALITY CHECK" reminder after 30+ tool calls (counter kept by a `PostToolUse` hook). No evidence of effect given. — [#34685](https://github.com/anthropics/claude-code/issues/34685); [#40547](https://github.com/anthropics/claude-code/issues/40547)
- [ANECDOTE] Monitoring: watch `/context` or a status line rather than waiting for auto-compact. — [wmedia.es](https://wmedia.es/en/tips/claude-code-1m-context-window-tips); [dangeabunea.com](https://www.dangeabunea.com/blog/why-claude-code-gets-worse-the-longer-you-use-it-and-how-to-fix-it)
- [MEASURED, second-hand, not Claude Code-specific] Morph research cited as showing a 90% gain for a sub-agent architecture over single-agent; JetBrains observation masking cut cost 52% and raised solve rate 2.6%. — [Sikkema](https://www.albertsikkema.com/ai/development/tools/2026/04/23/smaller-context-window-better-claude-code.html)
- [COUNTERPOINT] Cognition found model-written notes near the end of context were not comprehensive enough to rely on, and sometimes cost more tokens than the work. — [Cognition blog](https://cognition.com/blog/devin-sonnet-4-5-lessons-and-challenges)

### Inferences
- The durable common denominator is "put state on disk (plan/handoff/progress file), then start clean", which works identically at 200K and 1M and does not depend on any particular threshold being right.
- Because current docs re-inject root CLAUDE.md and the plan-mode plan after compaction, a rule that must survive belongs in root CLAUDE.md or a `compact`-matched SessionStart hook, not in a path-scoped rule or an earlier chat message.
- "Never let it auto-compact" has a mechanistic rationale from Anthropic's own staff (the summary is written at the model's worst moment, without knowing where the work is going), which is stronger support than the folklore thresholds have.

### Gaps
- No controlled comparison (e.g. handoff file vs. steered /compact vs. auto-compact) on the same tasks was found.
- Effectiveness of reminder-injection hooks is asserted, not tested.
- Reddit (r/ClaudeAI, r/ClaudeCode), X posts by Claude Code team members, YouTube transcripts and Latent Space were not reached in this pass.
