---
name: external-knowledge
description: |
  Use when a reliable answer depends on current, external, source-specific, or model-knowledge-external information: product/pricing/plan/policy/release changes, versioned technical docs, GitHub-native facts, WeChat article discovery or reading, remembered Chinese long-form articles, or other source-grounded public lookups. Choose the source by information need, use specialist owners when appropriate, escalate only after an observed evidence gap, and stop when the user's question is sufficiently supported. Do not use when the user's supplied context or stable general knowledge is already enough, or when the user forbids network access.
---

# External Knowledge

Get enough reliable external evidence to answer the user's actual question — no less and no more.

This Skill coordinates source selection. It does not replace the underlying provider Skills, plugins, MCPs, or Web tools.

## Core Pipeline

```text
1. Identify the claim or decision that needs external evidence.
2. Choose the source class that naturally owns that claim.
3. Retrieve the minimum sufficient evidence.
4. If evidence is insufficient, name the exact gap.
5. Escalate only to a provider that can close that gap.
6. Stop when more retrieval would not materially change the answer, confidence, risk, or next action.
```

A simple lookup may legitimately finish after one authoritative source.

## Source Ownership Fast Path

| Information need | Preferred owner |
| --- | --- |
| Current product, pricing, plan, policy, feature, release, announcement | Official/current Web source |
| Version-specific library/framework/SDK/API behavior | Context7 when available; otherwise official docs |
| Repository, commit, PR, issue, review, release, code-history semantics | GitHub-native plugin/connector |
| Discover WeChat Official Account articles by topic | WeChat discovery specialist |
| Read a known canonical `https://mp.weixin.qq.com/s/<id>` article | WeChat reader specialist |
| General current/public information | Runtime-native Web search/fetch |
| General Web has an observable precision gap | Exa or equivalent precision challenger, if available |
| Known URL cannot be read/extracted adequately | Firecrawl or equivalent extraction challenger, if available |

Read `references/source-semantics.md` when ownership is ambiguous.

## WeChat Semantic Routing

Treat WeChat as a source semantic, not a literal-keyword trigger only.

Use WeChat discovery when:

- the user explicitly asks for WeChat / Official Account articles;
- the user is trying to recover or discover a Chinese long-form article and WeChat is a high-fit source;
- the user asks for Chinese public-account commentary or analysis where WeChat is materially relevant.

Do not search WeChat for every Chinese-language query.

The semantic decision happens at the agent layer. The current discovery backend may still be keyword-based: infer that WeChat is the right source, derive a useful search expression, then call the specialist. Do not claim vector/embedding semantic retrieval unless the backend actually provides it.

If the user only wants candidate articles, stop after discovery. If the user needs article content, select/resolve a canonical article and use the reader.

## Evidence Depth

Match retrieval depth to the question.

Increase evidence depth only when needed, for example:

- the user asks what changed over time rather than only the current state;
- the primary source shows current state but not history;
- sources disagree;
- the user asks for community sentiment rather than official facts;
- the question supports a high-impact engineering/operational decision;
- the primary source does not actually support the material claim.

Do not cross-check mechanically when one authoritative source already resolves a low-risk factual lookup.

## Gap-Based Escalation

Escalation requires an observed gap.

Examples:

- Current OpenCode Go page shows today's plan but not what changed -> historical evidence gap -> search dated official announcements or credible historical sources.
- General Web cannot locate an exact niche technical fact -> precision gap -> use Exa if available.
- Web fetch returns an empty shell and required text is absent -> extraction gap -> use Firecrawl if available.
- WeChat discovery finds candidates but the user asks what the article argues -> content gap -> use the WeChat reader.

Do not call multiple providers in parallel merely for coverage.

## Runtime Capability Boundary

Provider availability is runtime-specific.

- Use an available specialist when it clearly owns the claim.
- If a specialist is unavailable, use the nearest reliable fallback when one exists.
- If the missing specialist materially blocks the task, say so briefly.
- Missing capability is not authorization to install, configure, repair, or replace tools.
- Never assume a provider exists because it was available in another runtime.

## STOP Rule

Continue retrieval only if another call could materially change at least one of:

- the conclusion;
- confidence in the conclusion;
- a material risk;
- the user's next action.

Otherwise STOP.

Do not turn a simple lookup into a research project.

## Output Contract

Answer the user's question directly.

For changing products, plans, releases, or policies:

- distinguish current state from historical change;
- use concrete dates when available;
- distinguish confirmed facts from inference;
- cite the sources supporting material claims.

Do not expose internal routing tables unless the user asks how the answer was obtained.

## Boundaries

- Do not retrieve when the user's supplied context already supports the answer.
- Do not retrieve when stable general knowledge is enough.
- Do not override an explicit user instruction not to browse.
- Do not install or configure providers during an information lookup unless explicitly requested.
- Do not invent historical change from a current-only page.
- Do not force General Web when a specialist clearly owns the claim.
- Do not force a specialist when General Web already resolves the claim adequately.
- Do not keep searching after the STOP condition is met.

Read `references/gotchas.md` when reviewing a failure or unexpected routing result.
