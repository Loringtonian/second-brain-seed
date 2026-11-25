# Forums, video descriptions and non-English writing: context-fill data for Opus 5.5, Sonnet 5.5, Fable 5.2 / Mythos 5.2

Research date: 25 November 2026. Scope: only Claude Opus 5.5 (released 22 Sep 2026), Claude Sonnet 5.5 (28 Sep 2026), Claude Fable 5.2 / Mythos 5.2 (9 Nov 2026).

## Data table of measured results (top-of-notes summary)

**No measured result was found.** In every platform searched, nobody has published numbers for any of the three models at two or more context sizes on the same task. The table below therefore holds only the quantified *informal* observations (single-person anecdotes, no method, no sample size). Do not present these as measurements.

| # | Model(s) | Claim (token level) | Type | Method / n | Author, date | Lang | URL |
|---|---|---|---|---|---|---|---|
| 1 | "current Opus/Fable models" (posted in the Fable 5.2 launch thread; Opus 5.5 was current Opus) | "I haven't subjectively felt any degradation up to like 500-600k tokens"; hands off to a fresh session after that; does not use compaction | INFORMAL anecdote | subjective, n = 1 user | mnicky, 10 Nov 2026 | EN | https://news.ycombinator.com/item?id=50447143 |
| 2 | "Current frontier models" (no model named; Fable 5.2 launch thread) | "get noticeably more stupid at context sizes of only 100k tokens or so" | INFORMAL anecdote | subjective, n = 1 user | marmarama, 10 Nov 2026 | EN | https://news.ycombinator.com/item?id=50445291 |
| 3 | Opus 5.5 (thread is about Opus 5.5 in Claude Code; comment says "claude") | Ran one job "almost 3 days straight", handing off to a new session "when context hit about 800k" | INFORMAL anecdote (a working practice, not a failure threshold) | n = 1 project | Merad, 4 Oct 2026 | EN | https://news.ycombinator.com/item?id=49950333 |
| 4 | Opus 5.5 vs Sonnet 5.5 | Multi-service workspace with 1M windows "filled to the brim": "works fine with Opus 5.5 ... It doesn't work equally well with Sonnet 5.5" | INFORMAL anecdote, no numbers beyond "1M" | n = 1 user | Slartie, 6 Oct 2026 | EN | https://news.ycombinator.com/item?id=49979781 |
| 5 | Opus 5.5 | "more forgetful than GPT-5.6 or 6.1 or Fable, so despite having a 1M context window it gets lost in a big codebase" | INFORMAL anecdote, no token level | n = 1 user | porker, 9 Nov 2026 | EN | https://news.ycombinator.com/item?id=50431997 |

Related numbers that are NOT context-size measurements (included so the report writer does not mistake them for fill curves):

| Item | Numbers | Why it is not a fill curve | URL |
|---|---|---|---|
| Claude Code auto-compact threshold on the 1M window (Opus 5.5, Claude Code 2.1.280) | "roughly 967K tokens" | Harness setting, attributed by a third-party guide to the docs; says nothing about quality | https://www.buildthisnow.com/ja/blog/guide/mechanics/claude-opus-5-5-in-claude-code |
| "Opus 5.5 nerfed" session-log analyses (as relayed by a secondary handbook page; primary not opened) | ~1,400 sessions: responses starting with "I'm" rose "from 20-30% to 61.3%", median thinking tokens 84 → 54, error indicators +47%. Separate 6,600-response analysis: responses needing correction 5.2% → 25.4% | Change over calendar time, not over context fill; secondary source, UNVERIFIED against the primary | https://book.st-hakky.com/en/data-science/opus-5-5-performance-degradation-triage |
| LiveNerf daily benchmark of Opus 5.5 | "no change" on 78 hard tasks as of 10 Nov 2026 (Claude Code 2.1.280, Max plan) | Change over calendar time, not context fill; secondary relay | https://book.st-hakky.com/en/data-science/opus-5-5-performance-degradation-triage |

---

## Key question 1: Has anyone published numbers for any of the three models at two or more context sizes on the same task?

### Takeaway
No. Across Hacker News (full comment text via the Algolia API), LessWrong, and Chinese, Japanese, Korean, Russian, German, French and Spanish web search, I found zero experiments that report Opus 5.5, Sonnet 5.5 or Fable 5.2 / Mythos 5.2 at two or more context sizes on one task. The public argument about these models in forums is almost entirely about "nerfing over time", effort levels and cost, not context fill.

### Cited Findings
- Hacker News launch threads exist for all three models and are large (Opus 5.5: 1,806 points / 1,134 comments; Sonnet 5.5: 884 / 613; Fable 5.2 and Mythos 5.2: 926 / 1,056). Searching the comments of these threads for "context rot", "1M context", "context window", "compaction", "400k", "500k", "200k" returned 54 comments, none of which reports a measurement at multiple context sizes. — [Opus 5.5 thread](https://news.ycombinator.com/item?id=49803892); [Sonnet 5.5 thread](https://news.ycombinator.com/item?id=49881850); [Fable 5.2 / Mythos 5.2 thread](https://news.ycombinator.com/item?id=50429068)
- A commenter in the Opus 5.5 launch thread observed that the topic has dropped out of discussion: "It seems context length has completely fallen out of the discussion since we hit 1M, is that just going to be what it is now?" (ramesh31, 22 Sep 2026). — [HN comment](https://news.ycombinator.com/item?id=49804970)
- HN story search for "context rot" since mid-September 2026 returns only low-engagement items (3-5 points), none specific to these three models: an arXiv paper "Length Is Cheap, Competition Is Not: Attention Predicts Context Rot in Retrieval" (submitted 4 Nov 2026, 3 points, 0 comments) and a blog "Quantifying Context Rot" (16 Sep 2026, before Opus 5.5 shipped). I did not open either; whether the arXiv paper tests any of the three models is unknown. — [HN item 50371624](https://news.ycombinator.com/item?id=50371624); [HN item 49734038](https://news.ycombinator.com/item?id=49734038)
- A secondary English/Japanese handbook page on Opus 5.5 degradation (last updated 17 Nov 2026) states that the 1M-token window and 128K max output are "capacity limits, not performance metrics for response quality in long-history scenarios", proposes comparing fresh and long-running sessions, and reports no results from such a comparison. — [book.st-hakky.com](https://book.st-hakky.com/en/data-science/opus-5-5-performance-degradation-triage)
- A third-party guide to Opus 5.5 in Claude Code (author "speedy_devv", 24 Sep 2026, English body served on a /ja/ URL) contains no quality measurement at different context sizes; its only performance claims are Anthropic's coding evaluations by effort level. — [Build This Now](https://www.buildthisnow.com/ja/blog/guide/mechanics/claude-opus-5-5-in-claude-code)
- UNVERIFIED (search-snippet level): Chinese search surfaced Zhihu, Juejin, CSDN and Tencent Cloud articles on Opus 5.5 and its 1M window. The search summary says a CSDN hands-on article ("Claude Opus 5.5 '焚诀' 实测: 提示词心法、长上下文与Agent边界" — "Opus 5.5 hands-on: prompting, long context and agent limits") mentions improved ultra-long-document retrieval but also information decay, with no figures visible in the snippet. The page returned HTTP 521 when fetched, so this is not confirmed. — [CSDN](https://blog.csdn.net/weixin_29029995/article/details/167154485)
- UNVERIFIED (search-snippet level): a Zhihu article on the Opus 5.5 launch cautions that a larger window "cannot simply be understood as the model automatically remembering everything" (translated from Chinese by the search tool); no measurement cited. — [Zhihu](https://zhuanlan.zhihu.com/p/2086488465454245242)
- UNVERIFIED (search-snippet level): Japanese search surfaced note.com and Hatena posts about Opus 5.5, all on the "nerf" debate and effort levels, e.g. a note.com post titled (English rendering) "With 130,000 records, it's clear: Opus 5.5 is fine on low", reporting a dissatisfaction-rate table of 5.2% at low effort vs 4.0% at medium. This is an effort comparison, not a context-size comparison. — [note.com](https://note.com/ai_arai_ally/n/n34d04843c042?hl=en)
- UNVERIFIED (search-snippet level): Korean-language search returned no Korean-language test at all; the search tool relayed an English third-party figure that on Terminal-Bench 4.1 "Claude Code / Opus 5.5 (max)" scored 65.8% ± 3.1 and "Claude Code / Fable 5.2 (max)" 65.8% ± 2.8. This is a single-condition benchmark, not a context-size comparison, and I did not trace the source page. — [morphllm.com (listed in results)](https://www.morphllm.com/opus-5-5)
- Russian search returned no Habr test of these three models; the only Habr long-context article surfaced concerns Opus 4.6 on MRCR v2 (out of scope). — [Habr](https://habr.com/ru/articles/993326)
- German/French/Spanish search returned no test. A French comparison page titled "opus 5.5 vs gemini 3.8 vs deepseek contexte 1m 2026" was surfaced with the snippet "la taille brute de la fenêtre de contexte ne dit rien sur la qualité de restitution" ("raw context-window size says nothing about recall quality"); the page returned HTTP 410 Gone when fetched, so UNVERIFIED and no numbers obtained. — [tech-insider.org/fr](https://tech-insider.org/fr/opus-5-5-vs-gemini-3-8-vs-deepseek-contexte-1m-2026/)
- LessWrong search surfaced only a system-card review post for Opus 5.5, with no long-transcript experiment. — [LessWrong](https://www.lesswrong.com/posts/vMNTWTDWLorDqd3LS/claude-opus-5-5-the-system-card)

### Inferences
- The absence is itself the finding: two months after Opus 5.5 and two weeks after Fable 5.2, the forum and non-English ecosystem has produced no context-fill curve for these models. Community measurement effort went into "is it nerfed?" trackers (LiveNerf, NerfBench/BridgeBench) that measure drift over days, at a fixed task size.
- Several out-of-scope items appear prominently in search and are easy to misattribute to the 5.5 / 5.2 models: GitHub issues about Opus 4.6 (degradation "starting at 40%"), an Opus 5 issue ("out of context at 28-50% usage"), a Towards AI piece on a "400K cliff" for Sonnet 5, and an aimultiple test of Fable 5 at 850,000 tokens. None of these is about the three target models.

### Gaps
- I could not open the CSDN, Juejin or French pages (521 / "Please wait" / 410), so I cannot rule out that one of them contains a small test.
- linux.do, V2EX, Zenn, Qiita, velog and tistory produced no hits in web search for these models plus context terms; I did not search those sites through their own search functions.
- The arXiv paper 2611.04169 on context rot was not opened; it may or may not include these models (likely covered by the English-papers pass).

---

## Key question 2: What quantified informal observations exist?

### Takeaway
Only a handful, all single-user anecdotes on Hacker News, and they disagree: one user feels no degradation up to 500-600k tokens on current Opus/Fable, another says frontier models get "noticeably more stupid" at about 100k, and one runs Opus 5.5 to about 800k before handing off. Nobody logged error rates or instruction-adherence counts against context fill.

### Cited Findings
All items below are INFORMAL. None gives a method, a task definition or a sample size.

- mnicky (10 Nov 2026, Fable 5.2 launch thread), replying to the 100k claim: "It may be domain/task/prompting dependent etc, but my experience is different. I haven't subjectively felt any degradation up to like 500-600k tokens with current Opus/Fable models. After which I usually do a manual hand-off to a fresh session anyway. I don't use context compaction though." — [HN comment](https://news.ycombinator.com/item?id=50447143)
- marmarama (10 Nov 2026, same thread): "The mistake rate goes up rapidly as context grows. Current frontier models get noticeably more stupid at context sizes of only 100k tokens or so, and inference becomes slower as the context grows, leading to more rework and more token usage." No model named. — [HN comment](https://news.ycombinator.com/item?id=50445291)
- Merad (4 Oct 2026, "Getting the most out of Opus 5.5" thread): "I even did one fairly complex POC for work that had claude implementing for almost 3 days straight, with me stopping it and handing off to a new session when context hit about 800k." — [HN comment](https://news.ycombinator.com/item?id=49950333)
- Slartie (6 Oct 2026): "I am frequently running agents on a multi-microservice application workspace where I really need the 1M context windows, because they are filled to the brim when implementing features that require changes on several services and APIs. This works fine with Opus 5.5. But it also works fine with GPT 6.1 Sol, Kimi K3 and MiMo 2.6 Pro. It doesn't work equally well with Sonnet 5.5, interestingly." — [HN comment](https://news.ycombinator.com/item?id=49979781)
- porker (9 Nov 2026): Opus 5.5 is "more forgetful than GPT-5.6 or 6.1 or Fable, so despite having a 1M context window it gets lost in a big codebase or goes of target." — [HN comment](https://news.ycombinator.com/item?id=50431997)
- In the "Ask HN: Opus 5.5 suddenly dumb as rocks?" thread (late Oct 2026), user Bender ties degradation to turn count rather than tokens ("if I let Opus 5.5 run for too many turns"; "the context rot is getting worse for me personally"), with no token level; jrockway counters with a daily benchmark link and concludes the model "is not getting dumber." Quotes are as extracted by the fetch tool from the thread. — [HN thread](https://news.ycombinator.com/item?id=50296367)
- Compaction frequency complaints, not quality measurements: toephu2 (22 Sep 2026) "even at xhigh I get context compaction quite a bit" on Opus 5.5; BoorishBears (9 Nov 2026) on Fable 5.2 in Claude Code: "immediately getting a cartoonish number of compactions." — [HN comment](https://news.ycombinator.com/item?id=49808190); [HN comment](https://news.ycombinator.com/item?id=50430883)
- The Fable 5.2 compaction complaints at launch have a harness explanation given by other users: Leary (9 Nov) "Context window maxes out at 200k in Claude Code instead of 1M on my $200 ultra plan?"; user43928 (10 Nov) "It needs the latest Claude Code update otherwise context is set to 200k and compacts before even responding." — [HN comment](https://news.ycombinator.com/item?id=50430567); [HN comment](https://news.ycombinator.com/item?id=50438136)
- Practice of capping the window: Roark66 (27 Oct 2026) runs "a supervisor model like Opus 5.5 ... limited to 200k of context and told to keep it's progress updated in a text file", for speed and cost reasons, with no quality numbers. — [HN comment](https://news.ycombinator.com/item?id=50261248)
- Third-party guide: Claude Code's default auto-compact threshold sits "at roughly 967K tokens on a 1M window" for Opus 5.5, and `CLAUDE_CODE_DISABLE_1M_CONTEXT=1` caps native 1M models at 200K (attributed by the author to the docs; I did not check the docs page). — [Build This Now](https://www.buildthisnow.com/ja/blog/guide/mechanics/claude-opus-5-5-in-claude-code)

### Inferences
- The informal range for "where it starts to feel worse" on these models spans 100k to above 600k tokens, which is too wide to support any threshold. The two most specific reports (500-600k with no felt degradation; hand-off at 800k) both come from users who avoid compaction and hand off manually, so they describe uncompacted transcripts.
- The only model-to-model contrast at a full window is one user's statement that Sonnet 5.5 handles a brim-full 1M workspace worse than Opus 5.5. It is one unquantified report.
- Early Fable 5.2 "constant compaction" reports are confounded by an outdated Claude Code client holding the window at 200k, so they should not be read as evidence about the model at 1M.

### Gaps
- No one in the sources reached logged error rates, tool-call failure rates or instruction-adherence counts as a function of context fill for these models.
- Reddit and X content could only be seen through search summaries; no Reddit thread on this topic for these models surfaced at all, so there is nothing snippet-level to report beyond a secondhand note (via a Chinese blog summary) that Reddit had many quality-drop complaints about Opus 5.5 in late September to early October 2026 — [blog.ccino.org](https://blog.ccino.org/p/opus-55-nerf-livenerf-baseline-2026/) (UNVERIFIED, snippet-level).
- No YouTube video description with a context-length test of these models surfaced in search.

---

## Key question 3: What do Anthropic staff say publicly about where these specific models degrade?

### Takeaway
I found no Anthropic staff statement that names a token level at which Opus 5.5, Sonnet 5.5 or Fable 5.2 degrades. The only Anthropic-authored text I found that names two of these models alongside context rot is a cookbook page, which describes the effect qualitatively and gives no threshold; the Fable 5.2 prompting guide does not discuss context length at all.

### Cited Findings
- Anthropic cookbook, "Context engineering: memory, compaction, and tool clearing", byline Isabella He (@isabella-anthropic), page dated "Published on March 20, 2026" but its text now names the 5.5 models (so it has been revised since): "Claude Opus 5.5 and Claude Sonnet 5.5 both provide a 1M-token context window. For this task, the baseline's total input stays under that limit: the agent reads the full corpus and synthesizes without hitting a hard wall." — [Claude cookbook](https://platform.claude.com/cookbook/tool-use-context-engineering-context-engineering-tools)
- Same page: "On a 1M window the agent keeps running, but context rot sets in as the window fills: an early document read is still technically present, but by the end of the run it's buried under everything read since, and the model's ability to recall its details degrades." — [Claude cookbook](https://platform.claude.com/cookbook/tool-use-context-engineering-context-engineering-tools)
- Same page: "Studies on needle-in-a-haystack style benchmarking have uncovered the concept of context rot: as the number of tokens in the context window increases, the model's ability to accurately recall information from that context decreases." and "So, even before the hard context limit is reached, the agent may be getting less out of each token." — [Claude cookbook](https://platform.claude.com/cookbook/tool-use-context-engineering-context-engineering-tools)
- Same page: "The baseline keeps climbing until it either hits a context-window limit (a hard stop on smaller windows) or accumulates enough tokens that context rot meaningfully degrades recall." The notebook's code uses `claude-sonnet-5-5`. I read only the first 100,000 of about 124,000 characters and saw no quality-vs-fill numbers in that portion. — [Claude cookbook](https://platform.claude.com/cookbook/tool-use-context-engineering-context-engineering-tools)
- Anthropic's "Prompting Claude Fable 5.2" guide (read in full) has no section on context length, context rot or long-transcript degradation. Its only related statements concern harness behaviour: compaction and context editing are the supported ways to shorten history under preserved thinking, and "Claude Fable 5.2 works well on long-running asynchronous tasks". — [Anthropic docs](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5-2)
- "Getting the most out of Opus 5.5 in Claude and Claude Code" (claude.dev blog, by Addy Osmani, 22 Sep 2026; the page does not state whether it is an official Anthropic publication or the author's affiliation, so I do not count it as an Anthropic staff statement): "A long run fills the context window, and Claude Code then summarizes older turns. A list in a file survives that, and it shows you at a glance what's done and what's left." It gives no token threshold. — [claude.dev](https://claude.dev/blog/getting-the-most-out-of-opus-5-5/)
- UNVERIFIED (search-snippet level, and not specific to the three models): Anthropic's Claude Code session-management guide is summarized by the search tool as saying that, due to context rot, the model is at its least intelligent point when compacting; the search tool attributes a session-management guide dated April 15 to Anthropic engineer Thariq Shihipar. This predates all three models. — [claude.com blog](https://claude.com/blog/using-claude-code-session-management-and-1m-context)

### Inferences
- Anthropic's public position, as far as these sources show, is qualitative: context rot exists on the 1M window for the 5.5 models and recall of early material degrades as the window fills, with no published threshold or curve for these models in docs, cookbook or prompting guides.

### Gaps
- No X/Twitter posts by Anthropic staff about these models' long-context behaviour were retrievable; X is not searchable from here beyond what web search surfaces, and nothing surfaced.
- No Anthropic staff comments on the subject were found in the three HN launch threads through keyword search; I did not enumerate all comments by known staff accounts.
- The system cards (Sonnet 5.5, Fable 5.2 / Mythos 5.2) were surfaced as HN links but not opened here; they are covered by a separate note (anthropic_cards.md).

---

## Key question 4: Platform-by-platform coverage statement

### Takeaway
Hacker News was fully reached and searched and yielded only anecdotes. LessWrong, Reddit, X, YouTube, Substack and all non-English platforms were covered at web-search level only, and none yielded a measurement for the three models.

### Cited Findings
- **Hacker News — reached and searched.** Used the public Algolia API: story search for each model name and "context rot"; comment search inside the three launch threads and the "Getting the most out of Opus 5.5" thread; site-wide comment search since late September 2026 on eleven query phrases, filtered for comments mentioning a model, the word "context" and a number. Found: no measured experiment; five quantified anecdotes (table at top). — [HN Algolia API](https://hn.algolia.com/api/v1/search?query=Opus%205.5&tags=story)
- **Hacker News "Ask HN: Opus 5.5 suddenly dumb as rocks?" — reached (page fetched).** Found: turn-count complaints, no token levels, plus links to daily "nerf" trackers. — [HN thread](https://news.ycombinator.com/item?id=50296367)
- **LessWrong / Alignment Forum — snippet-level only.** Web search surfaced one Opus 5.5 system-card review post and no long-transcript experiment on any of the three models. Page not opened. — [LessWrong](https://www.lesswrong.com/posts/vMNTWTDWLorDqd3LS/claude-opus-5-5-the-system-card)
- **Reddit — snippet-level only; nothing found.** A targeted search for r/ClaudeCode-style threads on Opus 5.5 / Fable 5.2 context rot returned no Reddit results; the results were GitHub issues and blogs about older models. — [example out-of-scope result, Opus 4.6](https://github.com/anthropics/claude-code/issues/34685)
- **X — snippet-level only; nothing on context fill.** Two X posts appear as HN submissions, both about alleged nerfing over time rather than context: "Is Opus 5.5 nerfed? Thinking patterns changed on November 3" (827 points on HN) and "Silent degradation is back, Fable and Mythos 5.2 perform at level of Opus 5.5". Neither X page was opened. — [HN item 50422069](https://news.ycombinator.com/item?id=50422069); [HN item 50448772](https://news.ycombinator.com/item?id=50448772)
- **YouTube — snippet-level only; nothing found.** No video or description with a context-length test of these models surfaced. The only video-related hit was a text write-up of a Theo Browne review of Opus 5.5 about cost at max effort (title only, not opened). — [BigGo Finance](https://finance.biggo.com/news/d88fb484e898ec12)
- **Substack / personal blogs — snippet-level plus three pages fetched.** Fetched: claude.dev (Osmani), Build This Now, st-hakky handbook. None contains a context-size measurement for these models. Zvi's "Fable and Mythos 5.2: The System Card" appeared as an HN link and was not opened. — [HN item 50472555](https://news.ycombinator.com/item?id=50472555)
- **Chinese (Zhihu, CSDN, Juejin, Tencent Cloud; V2EX and linux.do) — snippet-level only.** Zhihu, Juejin, CSDN and Tencent Cloud articles on Opus 5.5 surfaced; no V2EX or linux.do thread surfaced. CSDN returned HTTP 521 and Juejin returned only a "Please wait..." interstitial, so both are **not reachable**. Nothing about Fable 5.2 or Sonnet 5.5 long-context tests surfaced in Chinese. — [Juejin](https://juejin.cn/post/7696387624982380570); [CSDN](https://blog.csdn.net/weixin_29029995/article/details/167154485)
- **Japanese (Zenn, Qiita, note.com, Hatena) — snippet-level only.** note.com and Hatena posts surfaced, all about the Opus 5.5 "nerf" debate and effort levels; no Zenn or Qiita context-fill test surfaced. — [Hatena](https://mshima8086.hatenablog.com/entry/2026/11/10/174238); [GeekNews JP relay of LiveNerf](https://ja.news.hada.io/topic?id=34519)
- **Korean (velog, tistory) — snippet-level only; nothing found.** No Korean-language post surfaced. — [search result example, English](https://www.promptzone.com/mauricio_arellano/claude-opus-55-performance-degradation-reports-testing-and-alternatives-474e.md)
- **Russian (Habr) — snippet-level only; nothing found** for these models; one Habr article on context rot concerns Opus 4.6. — [Habr](https://habr.com/ru/articles/993326)
- **German / French / Spanish — snippet-level only; nothing found.** One French comparison page was **not reachable** (HTTP 410). No Spanish results. — [tech-insider.org/fr](https://tech-insider.org/fr/opus-5-5-vs-gemini-3-8-vs-deepseek-contexte-1m-2026/)
- **Not reachable / not read:** a claude.ai public chat artifact and a LinkedIn post titled "1M context window: breakthrough or fiction?" recurred in search results for these queries; neither could be read with the available tools, so their contents are unknown. — [claude.ai public artifact](https://claude.ai/public/artifacts/fd417b17-9df5-486e-bddb-26bbebca0384); [LinkedIn profile result](https://ru.linkedin.com/in/dzitskiy)

### Inferences
- Because the web search tool is US-only and summarizes results, non-English coverage here is shallow: a negative result means "nothing surfaced in general web search", not "nothing exists on the platform".
- The strongest negative is Hacker News, where full comment text was searched directly.

### Gaps
- Native site search on Zhihu, V2EX, linux.do, Zenn, Qiita, velog, tistory and Habr was not attempted.
- Reddit, X and YouTube were not opened directly; any measurement posted only there and not picked up by blogs or HN would be missed.
- The recurring claude.ai public artifact and LinkedIn post remain unread.
