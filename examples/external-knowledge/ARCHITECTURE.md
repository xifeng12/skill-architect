# External Knowledge — Architecture Validation Example

This example exists to validate the design rules in `skill-architect` against a real, non-trivial Agent Skill.

It is **not** a packaging or production-readiness exercise. The question is whether the architecture produces correct runtime behavior.

## Skill / Non-Skill Decision

**Decision: Skill.**

Reasons:

- The workflow repeats across many unrelated user tasks.
- Correct behavior depends on intent-sensitive routing rather than always-on global rules.
- The model can make recurring domain mistakes without guidance: over-searching, using generic Web when a specialist owns the claim, forcing unavailable providers, or continuing after evidence is sufficient.
- The guidance should load only when an answer genuinely depends on current or external information.
- The workflow has stable boundaries, escalation signals, and STOP conditions.

This should **not** live only in `AGENTS.md`: the full retrieval policy is not an always-on workspace invariant. A short global rule may say “use external sources when needed,” but the source-selection workflow belongs in a routable Skill.

## Category

Dominant category from `skill-architect/references/category-patterns.md`:

**Data retrieval and analysis**

The Skill retrieves external evidence and decides when that evidence is sufficient.

## Dominant Pattern

**Pipeline**

The value is not correct syntax for one API. The value is an adaptive evidence-acquisition pipeline:

```text
information need
  -> source semantic
  -> best available owner
  -> retrieve
  -> evidence sufficient?
       YES -> STOP
       NO  -> name exact gap
              -> targeted escalation
              -> STOP when resolved
```

The pipeline is deliberately **goal-driven rather than fixed-step**. It must be legal for a simple product lookup to complete after one authoritative source.

## Secondary Pattern

**Reference-heavy**

Provider boundaries, source semantics, and failure modes belong in references so the root `SKILL.md` stays operational.

This is **not** primarily Tool Wrapper:

- Context7, GitHub, WeChat Discovery, WeChat Reader, Web search, Exa, and optional extraction tools remain independent owners.
- This Skill should not duplicate their commands, auth setup, or backend implementation.
- Provider-specific execution details should stay with the provider's own Skill/plugin/tool contract.

## Route Contract

Load when reliable answering requires **current, external, source-specific, or model-knowledge-external information**, including:

- current product/pricing/plan/policy/release/feature state or changes;
- version-specific technical documentation;
- GitHub-native repository/PR/issue/commit history;
- WeChat article discovery or reading;
- finding a remembered Chinese long-form article where WeChat is a high-fit source;
- source-grounded public-information lookup where stale model memory is insufficient.

Do not load when:

- the user already supplied sufficient source material;
- stable general knowledge is enough;
- the task is rewriting, summarization, translation, coding from provided context, or another non-retrieval task;
- the user explicitly forbids network access.

## Neighbor Skills / Owners

| Need | Neighbor owner | External Knowledge responsibility |
| --- | --- | --- |
| Versioned library/framework/SDK docs | Context7 | Select it when the information need is version-specific docs |
| GitHub repo/PR/issue/commit semantics | GitHub plugin/connector | Route GitHub-native claims to it |
| WeChat topic discovery | `wechat-article-search` | Infer WeChat source fit and derive a search expression |
| Known canonical WeChat article body | `weixin_articles.read_article` | Use only when body content is needed |
| General current Web | runtime-native Web search/fetch | Default when no specialist clearly owns the claim |
| Precision gap in general Web | Exa or equivalent | Escalate only after an observable precision gap |
| Known URL extraction gap | Firecrawl or equivalent | Optional challenger; absence is not permission to install it |

## Key Architectural Hypotheses

This example is intended to falsify or support these claims from the Skill architecture discussion:

1. **Route description should encode when to load, not advertise features.**
2. **Source selection should follow information need / source semantics, not literal tool keywords.**
3. **A broad Skill can coexist with specialist Skills if ownership boundaries are explicit.**
4. **Goal + constraints + escalation signals outperform a rigid multi-tool procedure.**
5. **Progressive disclosure works:** root stays small; ambiguous source ownership loads references on demand.
6. **STOP is part of architecture:** one authoritative source may be the correct complete execution.
7. **Runtime capability is contextual:** an unavailable provider is a degraded option, not an installation trigger.
8. **Semantic WeChat routing can happen at the agent layer even if the underlying discovery backend is keyword-based.**

## First Runtime Target: ZCode

The first runtime test is ZCode.

Known runtime condition supplied by the user:

```text
Firecrawl = uninstalled / unavailable
```

This condition belongs in the test profile, not the portable Skill contract.

Expected behavior:

- do not reinstall Firecrawl during lookup tests;
- use other available owners normally;
- only record extraction-challenger coverage as unavailable/deferred.

## Validation Focus

The first pass should test behavior, not answer quality alone:

```text
TRIGGERED
INITIAL_OWNER
PROVIDERS_CALLED
OBSERVED_GAP
ESCALATION
STOP_AFTER_SUFFICIENT
UNNECESSARY_CALLS
```

A useful Skill must improve these decisions without turning simple lookups into research projects.
