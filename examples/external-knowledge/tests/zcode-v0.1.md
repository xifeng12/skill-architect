# ZCode Runtime Validation — v0.1

Purpose: validate the **architecture hypotheses**, not exhaustively benchmark search providers.

## Runtime Condition

User-provided current condition:

```text
Firecrawl = UNINSTALLED / UNAVAILABLE in ZCode
```

Rules:

- Do not reinstall Firecrawl during these tests.
- Do not count Firecrawl absence as a routing failure unless the task truly requires an extraction challenger and no viable fallback exists.
- Do not modify ZCode provider configuration as part of a lookup test.

## Record Format

For each case record:

```text
CASE =
TRIGGERED = YES / NO / UNCLEAR
INITIAL_OWNER =
PROVIDERS_CALLED =
OBSERVED_GAP = NONE / <exact gap>
ESCALATION = NONE / <provider>
STOP_AFTER_SUFFICIENT = YES / NO
UNNECESSARY_CALLS = <count>
RESULT = PASS / PARTIAL / FAIL
NOTES =
```

Do not grade only the factual answer. Grade routing and work proportionality.

## First Pass — Run These 6

### Z1 — Simple fresh product change

Prompt:

> 查询一下 OpenCode Go 套餐最近有什么变化。

Expected architecture behavior:

- `external-knowledge` should trigger.
- Start from current/official product information.
- If the source proves both current state and change, STOP.
- If it only proves current state, name the historical-evidence gap before another search.
- Do not use Context7, GitHub, WeChat, Exa, or Firecrawl without a concrete reason.

Primary hypothesis: minimal sufficient work / STOP.

### Z2 — Implicit WeChat source semantic

Prompt:

> 我记得前段时间有人写过一篇中文长文，详细比较 OpenCode Go 和其他编码套餐，帮我找一下。

Expected architecture behavior:

- `external-knowledge` should trigger.
- The user did not say “微信/公众号”, but WeChat discovery is a legitimate high-fit owner.
- General Web may also be reasonable if the runtime judges the source memory too ambiguous; the run should explain the source choice through behavior, not by printing internal policy.
- If WeChat discovery returns plausible candidates, do not automatically read every article.

Primary hypothesis: source-semantic routing is not literal keyword routing.

### Z3 — Explicit WeChat discovery

Prompt:

> 找 5 篇公众号里讨论 Agent Skill 设计的文章，给我标题、来源和链接。

Expected architecture behavior:

- Route to WeChat discovery when available.
- Stop after candidate discovery because article bodies were not requested.
- Do not invoke reader on all results.

Primary hypothesis: specialist ownership + progressive work.

### Z4 — Versioned technical docs

Prompt:

> React 19.2 的 useEffectEvent 当前官方语义是什么？

Expected architecture behavior:

- Route to Context7 when available.
- Do not start with generic Web if Context7 clearly owns the versioned-doc claim.
- STOP when the versioned docs support the answer.

Primary hypothesis: neighbor specialist ownership.

### Z5 — Stable knowledge negative trigger

Prompt:

> B-tree 和 B+tree 的核心区别是什么？

Expected architecture behavior:

- `external-knowledge` should not be necessary unless the user adds a version/current-source constraint.
- No network call solely because the Skill exists.

Primary hypothesis: description/do-not-load boundary.

### Z6 — User-supplied-context negative trigger

Prompt with a short provided passage, then ask:

> 只根据我上面贴的内容总结三点，不要联网。

Expected architecture behavior:

- Do not retrieve external information.
- Explicit no-network instruction dominates.

Primary hypothesis: route boundary and user constraint.

## Second Pass — Only If First Pass Reveals No Structural Failure

### Z7 — GitHub-native semantics

Prompt:

> 看一下某个真实 PR 为什么删除了一个接口，并根据 PR/commit/review 证据解释原因。

Use a real repository/PR fixture available to the tester.

Expected: GitHub-native owner; ordinary Web only for non-repository background if needed.

### Z8 — Canonical WeChat reading

Prompt:

> 总结这篇文章：<confirmed canonical mp.weixin.qq.com/s/... URL>

Expected: use WeChat reader when available; no discovery step needed for a known canonical URL.

### Z9 — Precision challenger

Only run when an actual ordinary-search precision gap occurs naturally.

Expected: Exa may be used **after** the gap is observable. Do not manufacture a failure merely to exercise Exa.

### Z10 — Extraction challenger

Deferred in current ZCode runtime because Firecrawl is uninstalled.

Do not install it to make this test green.

## Stop Rule for the Eval Itself

Stop the test iteration when additional cases are not revealing a new routing, boundary, escalation, or STOP failure.

Do not turn Skill validation into provider benchmarking.
