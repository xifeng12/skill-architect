# Source Semantics

Use this reference when the best source owner is not obvious from the user request.

The routing unit is the **information need**, not a tool name or keyword.

## Source Classes

### Authoritative current state

Use official/current Web sources for facts such as:

- product plans and pricing;
- subscription entitlements;
- current feature availability;
- policy or terms changes;
- product announcements and release notes.

A single authoritative page can be sufficient for a low-risk current-state lookup.

If the user asks **what changed**, a current-only page may be insufficient. Name the missing historical evidence rather than inferring change.

### Versioned technical documentation

Use Context7 when available for version-specific library/framework/SDK/API semantics.

Fallback to official docs when Context7 is unavailable or does not contain the needed version/material.

Do not use Context7 merely because the source is official; release news, company blogs, changelogs, and ordinary product pages are not automatically Context7-owned.

### GitHub-native semantics

Use the GitHub-native plugin/connector when the claim depends on repository structure or Git history, for example:

- why a PR changed something;
- issue history;
- commit/release provenance;
- current repository files;
- review discussions.

A company page hosted on `github.io` or ordinary GitHub marketing/news content is still Web unless repository semantics matter.

### WeChat article discovery

Use WeChat discovery when the user explicitly asks for Official Account articles **or** when the information need strongly matches Chinese long-form/public-account content.

Examples that may trigger implicitly:

- “我记得有人写过一篇中文长文，详细比较 OpenCode Go 和其他编码套餐，帮我找一下。”
- “找一下国内长文对这次 Claude Code 更新的分析。”

Do not infer WeChat merely from Chinese language.

Current discovery may use keyword search under the hood. The agent's semantic job is to recognize source fit and derive useful search terms.

### WeChat article reading

If a canonical `https://mp.weixin.qq.com/s/<id>` URL is known and the user needs the article body, use the WeChat reader.

If the user only asked for candidate titles/links, discovery is sufficient; do not read every result.

### General Web

Use runtime-native Web search/fetch when no specialist source clearly owns the claim.

General Web is the normal starting point for current public information, not a mandatory first step before every specialist.

### Precision challenger

Use Exa or equivalent only after an observable precision problem, such as:

- relevant result cannot be located by ordinary search;
- exact niche technical entity is buried in noisy results;
- normal search returns semantically adjacent but non-answering results.

Do not call it in parallel with normal search simply to increase coverage.

### Extraction challenger

Use Firecrawl or equivalent only when:

- a known URL is required;
- ordinary fetch cannot obtain the needed content;
- the missing content is caused by extraction/rendering limitations rather than absence from the page.

A JavaScript-heavy page is not sufficient evidence by itself.

Provider absence is a runtime condition, not an instruction to install it.

## Decision Examples

| User request | Information need | First owner | Escalation condition |
| --- | --- | --- | --- |
| “OpenCode Go 现在有哪些套餐？” | current product state | official/current Web | official source does not expose current plan |
| “OpenCode Go 套餐最近变了什么？” | current + historical change | official/current Web | current page lacks historical evidence |
| “React 19.2 useEffectEvent 怎么定义？” | version-specific docs | Context7 | version/material missing |
| “这个 PR 为什么删了这个接口？” | repo history | GitHub | connector cannot expose needed history |
| “找 5 篇公众号讨论 Agent Skill 的文章” | WeChat discovery | WeChat discovery | no useful candidates / specialist unavailable |
| “我记得有篇中文长文比较几个编码套餐” | remembered Chinese long-form source | WeChat discovery or Web based on source fit | first source class yields no plausible candidates |
| canonical WeChat URL + “总结这篇” | article body | WeChat reader | reader unavailable/fails |

## STOP Test

Before another retrieval, ask:

> What exact missing evidence would this next call provide, and could it change the conclusion, confidence, material risk, or next action?

If no concrete answer exists, STOP.
