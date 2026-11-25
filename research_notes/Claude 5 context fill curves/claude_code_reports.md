# Claude Code long-session reports for Haiku 5.5, Sonnet 5.5, Opus 5.5 and Fable 5.2 (state as of 25 Nov 2026)

Labels used throughout: **[measured]** = numbers from a described procedure; **[anecdote]** = user report, no controlled method; **[official]** = Anthropic docs, changelog or staff; **[secondary]** = third-party write-up of someone else's data.

Reader warning up front: I found **no source that measures any of the four models' coding or instruction-following quality at several context sizes** (50K / 200K / 500K / 900K) inside Claude Code. Every fill-level number below is either a harness threshold (where compaction fires), a cost measurement, a compaction-loss measurement, or an anecdote. A performance-versus-fill curve for these four models cannot be drawn from published data; it can only be annotated.

## 1. Are there experiments that measure these models' quality at several context sizes in Claude Code or a similar harness?

### Takeaway
No. The closest measured items are (a) one Epoch AI elicitation run that let Opus 5.5 fill to 900K before compacting, in Vending-Bench rather than a coding harness, n=1, and (b) a one-run-per-setup Haiku 5.5 cost test at three compact windows that reports no quality score. Nothing at all was found for Sonnet 5.5 or Fable 5.2.

### Cited Findings
- **Opus 5.5, [measured], n=1 per arm, not Claude Code.** Epoch AI's Vending-Bench 2 review (dated 21 Oct 2026) used Claude Opus 5.5. Official harness: stated context window 69,000 tokens, counted with OpenAI's tiktoken; when full, the harness keeps the system prompt, first message and "the most recent 61% of the other messages, deleting the rest without summary"; this truncation fired 114 times for Opus (86 for GPT-6 Astra). Encrypted reasoning was a median 43% of Opus's harness-counted window. — [Epoch AI](https://epoch.ai/benchmarks/vending-bench-2/review)
- **Opus 5.5, [measured], same source.** Elicitation experiment with larger windows plus automatic compaction: compaction trigger 900,000 tokens for Opus, 250,000 for Astra. Opus fell from $9,459 to $5,254 (−44%); Astra rose from $15,366 to $18,044 (+17%). Epoch attributes Opus's drop to a worse strategy and says context "had no discernible impact". The figure shows one run per agent. — [Epoch AI](https://epoch.ai/benchmarks/vending-bench-2/review)
- **Opus 5.5, same source.** Opus left about 130 emails unread per night, so unread-email reports took roughly 40% of its 69K window. — [Epoch AI](https://epoch.ai/benchmarks/vending-bench-2/review)
- **Haiku 5.5, [measured for cost only], one run per setup, Claude Code v2.1.293+, 9 Oct 2026, author unnamed (Wmedia).** Task: read 45 files, one per message, 48 requests. Default window (compacts ~967K): 36 of 48 main-session requests were over 100K tokens, $0.508; subagent 27 of 48 over 100K, $0.372. 130K window (compacts ~97K): 0 of 48 over 100K, $0.145 main (−71%), $0.076 subagent (−79%). 100K window (compacts ~67K): main session "looped and never finished"; subagent finished but compacted 5 times instead of 2, $0.088. Starting context was 49K main, 5K subagent. No correctness or quality score is reported. — [Wmedia](https://wmedia.es/en/tips/claude-code-haiku-5-5-compact-at-130k)
- **Haiku 5.5, [anecdote inside the same test].** Haiku requested all 45 files in a single turn despite an instruction to go one at a time; that run cost $0.438 with the window set versus $0.149 without. — [Wmedia](https://wmedia.es/en/tips/claude-code-haiku-5-5-compact-at-130k)
- **Opus 5.5, [measured for usage, not quality], Clara Jensen, LLM API Guide, last updated 16 Nov 2026 (site sells API relay access).** One Max 20x account on Opus 5.5 at xhigh: 23 threads logged 19.5M input, 38.8M output, 113.9M cache-write and 4.9B cache-read tokens (97% cache hit). At a 700K–800K context, one or two "OK" keep-alive pings cost about 1% of the 5-hour quota (author's estimate). Quality claims in the same post are unquantified. — [LLM API Guide](https://llmapiguide.com/en/posts/claude-opus-5-5-getting-worse)
- **General context, [official], not model-specific.** Anthropic's context-window docs, as quoted in a GitHub issue: "more context isn't automatically better. As token count grows, accuracy and recall degrade, a phenomenon known as context rot." — [GitHub #81988](https://github.com/anthropics/claude-code/issues/81988)

### Inferences
- The Epoch run is the only published data point with one of these models at ~900K fill, and it cannot separate context effects from strategy variance (n=1, different domain, compaction confounded with window size).
- The Haiku test shows a harness failure floor, not a quality curve: with a 49K fixed base, a 100K window leaves too little headroom and the session loops. Haiku 5.5 is also the only one of the four with a price step tied to fill (over 100K), which is why its practitioners cap far lower than the others.

### Gaps
- No multi-point quality measurement (any metric) for Haiku 5.5, Sonnet 5.5, Opus 5.5 or Fable 5.2 at stated token counts in Claude Code. Searched for 200K/500K/900K experiments, repos and papers naming these models; none found.
- A third-party search summary attributed "reliable at 0–20% context, 1 in 4 retrievals fail at 1M" to a GitHub issue; I could not find that claim in the cited page (morphllm.com) or any issue, so it is excluded as unverified.
- Anthropic's system cards for these models were not retrieved; any long-context evals in them are not covered here.

## 2. What do users report, by model, about where quality slips, post-compaction behaviour, and differences between the four?

### Takeaway
Anecdotes cluster on Opus 5.5 and "Opus/Fable" jointly, with thresholds ranging from "~100K" to "no degradation up to 500–600K"; none is measured. For Fable 5.2 the best-documented long-session problem is a client bug (Desktop treating it as a 200K model), not model behaviour. No long-session anecdotes naming Sonnet 5.5 were found, and Haiku 5.5 reports are about cost, not quality.

### Cited Findings
**Opus 5.5**
- [anecdote] HN user porker, ~11 Nov 2026 (shown as "14 days ago" on 25 Nov): "despite having a 1M context window it gets lost in a big codebase." — [HN 50429068](https://news.ycombinator.com/item?id=50429068)
- [anecdote] Clara Jensen, updated 16 Nov 2026: degradation appears after "a few dozen turns"; at max/xhigh effort, looping and over-engineering as the window fills; at low effort, dropped edge cases and forgotten constraints. Recommends capping active context at 400K–600K and estimates this "stretches your effective quota roughly 1.5x". No data shown for the quality claim. — [LLM API Guide](https://llmapiguide.com/en/posts/claude-opus-5-5-getting-worse)
- [anecdote, weak] HN thread "Is Opus 5.5 nerfed? Thinking patterns changed on November 3" (~10 Nov 2026): several users report declining quality; one (ianberdin) re-ran a fixed benchmark and reports identical output in September and November. No comment in the portion read ties the complaints to a token count. — [HN 50422069](https://news.ycombinator.com/item?id=50422069)
- [measured, harness threshold] In one Desktop session with `autoCompactWindow: 500000`, Opus 5.5 auto-compacted at preTokens 470,849 / 466,986 / 467,467 (other sessions 466K–479K). — [GitHub #108640](https://github.com/anthropics/claude-code/issues/108640)

**Opus 5 (predecessor, labelled)**
- [anecdote] GitHub issue #81988, user amalic, 28 Jul 2026, "Opus 5 (1M context)", VS Code extension: in long sessions "the project's CLAUDE.md working-mode rules stop being followed. Nothing marks the transition." No token count or % given. Closed by stale bot 6 Sep 2026, locked 4 Nov 2026; no Anthropic reply. Cites related #80938 (compaction drops working-mode directives) and #80003. — [GitHub #81988](https://github.com/anthropics/claude-code/issues/81988)

**Fable 5.2**
- [measured, harness behaviour] GitHub #108640, hyeon1035298, 9 Nov 2026, Desktop Code tab with bundled Claude Code 2.1.317: Fable 5.2 auto-compacted at preTokens 178,625 / 169,821 / 168,940 (other sessions 167K–177K) with the warning "Auto-compact window for claude-fable-5-2 set to 1m tokens (capped to model limit of 200k)". Cause per reporter: 2.1.317 and 2.1.319 have no catalog entry for the model and fall back to 200K; 2.1.321 has `context:{window:1e6,native_1m:!0}`. Closed by reporter 11 Nov 2026; no Anthropic reply. — [GitHub #108640](https://github.com/anthropics/claude-code/issues/108640)
- [measured, harness behaviour] GitHub #108785, OmarMohamed03, 10 Nov 2026, Claude Code 2.1.317, macOS Desktop: eight auto-compactions in 23 minutes on `claude-fable-5-2`; each started at 168K–174K and the first turn afterwards was already 158K–165K, freeing only about 5K–10K. Summary grew from ~33K to ~47K characters over the 8 runs. Same user's `opus[1m]` default "never looped". Open, no Anthropic reply. A commenter adds that 15 of 25 quick cache misses came straight after a compaction. — [GitHub #108785](https://github.com/anthropics/claude-code/issues/108785)
- [anecdote] HN user postalcoder, ~11 Nov 2026: Fable 5.1 frequently hit max-output-token overflows; "I got none of these issues with 5.2." (Output limit, not context fill.) — [HN 50429068](https://news.ycombinator.com/item?id=50429068)

**Opus/Fable jointly (version not stated by commenter; thread is the Fable 5.2 launch thread)**
- [anecdote] HN user mnicky, ~12 Nov 2026: "I haven't subjectively felt any degradation up to like 500-600k tokens with current Opus/Fable models." Does a manual handoff to a fresh session past that and does not use compaction. — [HN 50429068](https://news.ycombinator.com/item?id=50429068)
- [anecdote] HN user marmarama, ~12 Nov 2026: "Current frontier models get noticeably more stupid at context sizes of only 100k tokens or so." No model named. — [HN 50429068](https://news.ycombinator.com/item?id=50429068)

**Haiku 5.5**
- [anecdote/guide] Third-party guides note Haiku 5.5 does not auto-compact until ~967K, so sessions run well past its 100K price step; advice is a 130K window. Quality in long sessions is not assessed. — [Wmedia](https://wmedia.es/en/tips/claude-code-haiku-5-5-compact-at-130k)

**Sonnet 5.5**
- No long-session or context-fill report naming Sonnet 5.5 was found on GitHub, HN or practitioner blogs.

### Inferences
- The two HN anecdotes (100K vs 500–600K) differ by a factor of five on the same day in the same thread; the briefing should plot them as a range of opinion, not a threshold.
- The 467K figure for Opus 5.5 and ~170K for mis-catalogued Fable 5.2 both equal the configured window minus roughly 30–33K, consistent with the ~33K auto-compact buffer the Wmedia author reports for v2.1.293. They are harness trigger points and say nothing about quality at those sizes.
- Fable 5.2's first two weeks of long-session reports are contaminated by the Desktop 200K fallback: users on bundled 2.1.317/2.1.319 experienced a 200K model that compacted every one or two turns.

### Gaps
- Only about 300K of the 430K characters of HN thread 50429068 and 100K of 269K of thread 50422069 were read; further relevant comments may exist.
- No GitHub issue naming Sonnet 5.5 or Haiku 5.5 for long-session quality was found. GitHub issue search was done via web search only, not the issues API.
- HN dates are derived from relative timestamps and are approximate (±1 day).

## 3. What have Anthropic staff said publicly since these models launched?

### Takeaway
I found no public statement from Claude Code team members that names any of the four models and addresses long sessions or when to compact. The standing guidance predates them (April 2026) and gives no token threshold in its official form.

### Cited Findings
- [official, predates all four models] Thariq Shihipar (Anthropic, Claude Code), 15 Apr 2026, "Using Claude Code: session management and 1M context": names no model version and gives no token threshold. Advice: new task means new session; rewind after a wrong path; `/compact` with a hint for sessions bloated with stale debugging; subagents for noisy intermediate work; compact proactively because the model is at its least capable when auto-compact fires. — [claude.com](https://claude.com/resources/articles/using-claude-code-session-management-and-1m-context)
- [secondary, unverified] A search summary of Thariq's X post of the same period attributes to him "context rot on the 1M model appears around ~300–400k tokens, but it is highly dependent on the task", and a community repo reports 400K as his suggested auto-compact compromise. The claude.com article does not contain these figures; I did not open the X post. — [X/trq212](https://x.com/trq212/status/2044548257058328723); [shanraisshan repo](https://github.com/shanraisshan/claude-code-best-practice/blob/main/tips/claude-thariq-tips-16-apr-26.md)
- [official, predates all four models] Boris Cherny on X (March 2026 by post ID; date not confirmed): "I've been using exclusively 1M context for the last few months and loving it. You can also customize your auto-compact threshold with the CLAUDE_CODE_AUTO_COMPACT_WINDOW env var." — [X/bcherny](https://x.com/bcherny/status/2032514809418109438)
- [official, Fable 5.2 docs, no context-fill content] "What's new in Claude Fable 5.2" positions it "for demanding reasoning and long-horizon agentic work", 1M window default and maximum, 128K output. Behaviour notes relevant to long runs: "A turn can end before the work is done" (more at low/medium effort); "Long runs of tool calls can pass with little user-facing text"; subagents "can account for a large part of what a session costs". Nothing on quality versus token count. Compaction on demand is offered in beta (`compact-2026-09-04`). — [Claude Platform Docs](https://platform.claude.com/docs/en/models/fable-5-2/whats-new-fable-5-2)
- [official absence] No Anthropic staff replied on issues #81988, #108640 or #108785. — [#81988](https://github.com/anthropics/claude-code/issues/81988); [#108640](https://github.com/anthropics/claude-code/issues/108640); [#108785](https://github.com/anthropics/claude-code/issues/108785)

### Inferences
- The only quasi-official number in circulation for where rot starts on a 1M window (~300–400K, task-dependent) dates from April 2026, was said about earlier models (Opus 4.6/4.7 era), and is not repeated in Anthropic's own article. It should be labelled as pre-dating all four models if used.

### Gaps
- X/Threads posts by Claude Code staff after 22 Sep 2026 were not directly searchable; statements may exist that I did not find.
- A secondary source (a GitHub issue in another repo) says the Opus 5.5 system card lists "very long trajectories and compaction" among blind spots of its automated behavioural audit; I did not verify this against the system card, so it is not listed as a finding.

## 4. Claude Code changelog entries since these models launched that changed context handling

### Takeaway
The default auto-compact point for all four models is about 967K on the 1M window (set in mid-2026 before they launched and inherited). Since Opus 5.5 landed, the material changes are per-model `/autocompact` windows, idle compaction at 200K+, subagent compact windows, and the unknown-model fallback moving from 200K to 1M. No entry changes the default threshold for these four models.

### Cited Findings
All entries are [official] from the changelog; version dates come from the docs changelog page (2.1.328–332) or from newsletters (earlier versions), as noted. — [CHANGELOG.md](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md); [docs changelog](https://code.claude.com/docs/en/changelog); dates via [Claude Code Masterclass newsletter, 21 Oct 2026](https://newsletter.claudecodemasterclass.com/p/last-week-in-claude-code-7-haiku) and [buildthisnow, 24 Sep 2026](https://www.buildthisnow.com/blog/guide/mechanics/claude-opus-5-5-in-claude-code)

Model arrivals
- 2.1.280 (22 Sep 2026): "Added Claude Opus 5.5 (`claude-opus-5-5`), now the default Opus model — 1M context, $4/$20 per Mtok".
- 2.1.284 (between 22 Sep and 6 Oct 2026; exact date not found): "Added Claude Sonnet 5.5 … 1M context, $2/$10 per Mtok".
- 2.1.293 (7 Oct 2026): "Added Claude Haiku 5.5 … 1M context, $0.10/$0.50 per Mtok ($0.50/$2.50 for prompts over 100K)".
- 2.1.321 (on or before 9 Nov 2026, inferred from issue #108640): "Added Claude Fable 5.2 (`claude-fable-5-2`), now the default Fable model — 1M context, $8/$40 per Mtok".

Threshold background (before these models, sets the inherited default)
- 2.1.247: "Changed Sonnet 5's default auto-compact window to its full 1M context, so sessions on the 1M window now auto-compact at about 967K tokens instead of about 934K".
- 2.1.260: "Improved auto-compact for 1M-context models: Opus and Fable sessions now compact shortly before the 1M-token limit".
- 2.1.273: "Fixed the context meter and auto-compact counting advisor-tool turns at roughly twice their real context size, which made auto-compact fire at about half the real window".

Since Opus 5.5
- 2.1.285: custom `ANTHROPIC_BASE_URL` sessions use the 1M window; "run `/autocompact 200k` if your gateway stops at 200K".
- 2.1.286: fallback notice and thrashing error now say when a fallback dropped the window from 1M to 200K.
- 2.1.287: Opus 4.7+ and Fable use 1M by default on Bedrock, Vertex, Foundry and the apps gateway; MCP `alwaysLoad: false` defers a server's tools behind tool search.
- 2.1.288: "Changed `/autocompact` to save the auto-compact window per model"; fixed "Prompt is too long" instead of auto-compacting when the last reply reported zero usage.
- 2.1.296 (9 Oct 2026): "Added `autoCompactWindow` to subagent frontmatter and `--agents` definitions"; fixed token counts for Haiku 5.5.
- 2.1.299 (14 Oct 2026): "Added idle compaction in the terminal: a 200K+ token conversation left idle for about 54 minutes is compacted before its one-hour prompt cache expires"; `CLAUDE_CODE_IDLE_COMPACT_MIN_TOKENS`; `/usage` warnings for always-on context and large tool results.
- 2.1.301: switching to a smaller-window model now has the previous model summarize first.
- 2.1.302 (16 Oct 2026): `Idle compaction` row in `/config`.
- 2.1.304 (19 Oct 2026): `CLAUDE_CODE_AUTO_COMPACT_WINDOW=200k` had silently set a 100K window, now fixed; tool search on by default behind custom base URLs.
- 2.1.307: fixed some "Autocompact is thrashing" errors in turns with many large tool results.
- 2.1.311: standard-context sessions over 200K now compact and continue instead of stopping.
- 2.1.312: a subagent's `autoCompactWindow` can now lower the env-var window.
- 2.1.314: idle compaction added to VS Code and Desktop Code tab.
- 2.1.316: `/model <id>` now warns when the model is unrecognized "and auto-compact will assume a 200K context window".
- 2.1.318: "Fixed idle compaction replacing a conversation with a reply that was not a summary".
- 2.1.323: "Changed the context window assumed for a model ID that Claude Code doesn't recognize from 200K to 1M".
- 2.1.326: `/autocompact` slider.
- 2.1.328 (19 Nov 2026): "Fixed automatic compaction reusing the session's earlier system prompt instead of rebuilding it the way `/compact` does"; shorter Code Mode tool descriptions.
- 2.1.329 (19 Nov 2026): pre-warmed sessions ignoring project `autoCompactWindow` fixed.
- 2.1.330 (20 Nov 2026): fixed multi-minute freeze right after compacting a very long conversation; `/context` undercount fixed.
- 2.1.331 (23 Nov 2026): "Fixed /compact and auto-compact sometimes losing part of the conversation summary when the summary quoted certain XML tags".
- 2.1.332 (25 Nov 2026): fixed turns ending in "Prompt is too long" after repeated auto-compaction failures; summary that overruns the output limit now retried shorter; `/context` output improved; Desktop Code tab ignores `CLAUDE_CODE_AUTO_COMPACT_WINDOW` under 200K beside a gateway address.

Settings as described by a practitioner guide
- `/autocompact 500k`, `autoCompactWindow` (per model), `--autocompact`, `CLAUDE_CODE_AUTO_COMPACT_WINDOW`; allowed range 100,000 to 1,000,000; after compaction up to 5 recent files are kept, 5,000 tokens per invoked skill, 25,000 total. Kevin Welter, 31 Oct 2026, updated 13 Nov 2026. — [kevinwelter.com](https://kevinwelter.com/en/blog/claude-code-kontext)

### Inferences
- Several fixes (2.1.318, 2.1.328, 2.1.331, 2.1.332) are compaction-correctness bugs live during these models' first weeks, so "quality dropped after auto-compact" anecdotes from October–November 2026 may reflect harness bugs as much as model behaviour.
- From 2.1.288 the compact window is per model, so two users on the same model can have very different effective fill ceilings; anecdotes without a stated window are hard to compare.

### Gaps
- Exact release dates for 2.1.284, 2.1.285–288, 2.1.305–327 were not retrieved (the GitHub changelog has no dates; the docs page was read only for 2.1.328–332).
- No changelog entry mentioning "microcompact" or tool-result clearing defaults was found in the 2.1.224–2.1.332 range by keyword search; issue #108785 mentions `DISABLE_MICROCOMPACT=1` being set by the Desktop Code tab, which is user-reported only.

## 5. Published compaction-loss or long-session measurements that include these specific models

### Takeaway
Two measured sources include Opus 5.5: Epoch's Vending-Bench review (section 1) and Compact-Bench (arXiv 2610.31710), where Opus 5.5 as a summarizer damaged 45% of repairs (n=22) versus 36% for Fable 5 and Fable 5.1, with overlapping intervals. arXiv 2610.31778 does not name any of the four. Nothing measured was found for Sonnet 5.5, Haiku 5.5 or Fable 5.2, and no vendor blog (Factory, Amp, Cursor, Cognition) naming these models was found.

### Cited Findings
- **Compact-Bench, arXiv 2610.31710, [measured].** Task-preservation backbone study: each model writes the compaction summary using the Claude Code compaction prompt; a fixed continuation agent (Kimi-K3) then tries the SWE-bench repair from the summary only; n = 22 repairs that pass with full history. "Stable damaged" = summary-only run fails in at least 2 of 3 runs. Claude Opus 5.5: round-1 damaged 45% (10), stable 45% (10), 95% Wilson CI 27–65%, tied 8th–10th of 14 summarizers. Claude Fable 5: 41% (9) round 1, 36% (8) stable, CI 20–57%. Claude Fable 5.1: 36% (8) and 36% (8), CI 20–57%. Range across 14 summarizers: 5% (Claude Opus 4.8) to 64% (DeepSeek-Flash). Sonnet 5.5, Haiku 5.5 and Fable 5.2 are not in the tables read. — [arXiv 2610.31710](https://arxiv.org/html/2610.31710)
- **arXiv 2610.31778, "Benchmarks Are Surprisingly Compaction-Friendly", Salle, Bhatnagar, Bethge, Abdelnabi, Andriushchenko; v1 22 Oct 2026, v2 20 Nov 2026, [measured].** Harness is OpenCode; benchmarks PaperBench, ProgramBench, PostTrainBench plus SWE-chat sessions. For the two 1M-context models the compaction threshold did not measurably affect task performance; on ProgramBench the first summary dropped about one third of anti-cheating rules, rising with each compaction; on SWE-chat, how-to-work instructions were dropped about 6 times more often than what-to-build specs. The abstract page does not name the Claude model; a search snippet of the PDF names Claude Opus 4.6 and GPT-5.4, i.e. **not** any of the four models here. — [arXiv 2610.31778](https://arxiv.org/abs/2610.31778); [PDF](https://arxiv.org/pdf/2610.31778)
- **CompAct-Bench (arXiv 2611.25673), [measured], labelled Opus 5 not 5.5.** 200 instances; trajectories cut at 25/50/75% and compacted to 10% of length; executors Opus 4.8 and GPT-5.5 Codex. Mean post-compaction accuracy 56.7% across 12 compactors; best is Claude Opus 5 (Max) at 68.5%. Trajectories are only 8.8K–122.8K tokens. The project page gives no per-model numbers for Opus 5.5, Sonnet 5.5, Haiku 5.5 or Fable. — [CompAct-Bench](https://compaction-eval.github.io/CompAct-Bench/)
- **Epoch AI Vending-Bench 2 review, Opus 5.5** — see section 1: 69K window with 114 truncations; 900K auto-compact arm scored 44% lower in one run. — [Epoch AI](https://epoch.ai/benchmarks/vending-bench-2/review); news coverage dated 23 Nov 2026 at [AlphaSignal](https://alphasignal.ai/news/epoch-finds-3-of-4-agent-benchmarks-are-fundamentally-broken)
- **Andon Labs leaderboard context, [secondary via search].** Vending-Bench results: Opus 5 $11,182, Opus 5.5 $9,235. — [Andon Labs](https://andonlabs.com/blog/opus-5-5-gpt-6-sol-grok-4-7-vending-bench)

### Inferences
- Compact-Bench measures Opus 5.5 as the writer of summaries, not how Opus 5.5 behaves at high fill or after receiving a summary. It supports only "Opus 5.5's Claude-Code-style summaries lost task-critical detail in roughly 4–5 of 10 small repairs, no better than Fable 5/5.1 and worse than Opus 4.8 in this sample", with n=22 too small to rank.
- The 2610.31778 finding that process instructions are dropped far more often than build specs matches the Opus 5 anecdote in #81988 (CLAUDE.md working-mode rules lost) but was not measured on any of the four models.

### Gaps
- The last ~28K characters of the Compact-Bench HTML (appendices) were not read; additional per-model tables may exist.
- I did not open the 2610.31778 PDF to confirm the model list; the Opus 4.6 identification rests on a search snippet.
- No posts from Factory, Sourcegraph Amp, Cursor or Cognition naming these models and reporting long-context or compaction measurements were found (one search only).

## 6. Model-by-model evidence status

### Takeaway
Opus 5.5 has thin measured evidence and the most anecdotes; Haiku 5.5 has one small cost test; Fable 5.2 has harness-bug reports and one joint anecdote; Sonnet 5.5 has no model-specific long-session evidence at all.

### Cited Findings
- **Opus 5.5:** measured = Epoch 69K/900K arms (n=1) and Compact-Bench summarizer damage 45% (n=22); anecdotes = "gets lost in a big codebase", 400–600K cap advice. — [Epoch AI](https://epoch.ai/benchmarks/vending-bench-2/review); [arXiv 2610.31710](https://arxiv.org/html/2610.31710); [HN 50429068](https://news.ycombinator.com/item?id=50429068); [LLM API Guide](https://llmapiguide.com/en/posts/claude-opus-5-5-getting-worse)
- **Haiku 5.5:** measured = cost at three compact windows, one run each, plus a loop failure at a 100K window; price step above 100K is official. No quality-versus-fill evidence. — [Wmedia](https://wmedia.es/en/tips/claude-code-haiku-5-5-compact-at-130k); [CHANGELOG.md](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
- **Fable 5.2:** released into Claude Code in 2.1.321 (about two weeks before 25 Nov 2026). Evidence = Desktop 200K-fallback compaction loop (bug) and one HN anecdote covering "current Opus/Fable" to 500–600K. Fable 5 and 5.1 (labelled) have Compact-Bench summarizer numbers of 36% stable damage. — [#108640](https://github.com/anthropics/claude-code/issues/108640); [#108785](https://github.com/anthropics/claude-code/issues/108785); [HN 50429068](https://news.ycombinator.com/item?id=50429068); [arXiv 2610.31710](https://arxiv.org/html/2610.31710)
- **Sonnet 5.5:** only catalog facts (1M window, added in 2.1.284). — [CHANGELOG.md](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)

### Inferences
- Any chart for the briefing can honestly show, per model: the 1M ceiling, the ~967K default compact trigger, Haiku's 100K price step, the idle-compaction 200K floor, and anecdote ranges (100K to 600K) as shaded opinion bands. It cannot show a measured quality curve for any of the four.

### Gaps
- No evidence comparing the four models with each other at matched high fill.
- No evidence on post-auto-compact quality by model beyond the Compact-Bench summarizer result and harness bug reports.
