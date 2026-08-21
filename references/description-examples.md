# Description Examples

The frontmatter `description` is a routing/discovery surface, not a feature summary.

## Good Description Rules

A good description:

- starts from `Use when...` / `Load when...` semantics;
- describes user intent, information need, or task situation rather than internal implementation;
- can include realistic user phrasing as examples;
- names important exclusions when nearby Skills exist;
- is concise enough to route effectively;
- avoids listing every capability or provider;
- avoids workflow summaries and marketing language.

The route should still work when the user paraphrases the intent without using the example trigger words.

## Bad Description Smells

Refactor the description if it:

- starts with `This skill provides...`;
- explains the whole workflow;
- lists internal files/tools as the main routing signal;
- says `helps with X` without trigger conditions;
- uses vague words like `improve`, `optimize`, `support`, `assist` without a situation boundary;
- overlaps with another Skill without boundary language;
- is so broad that it could load for many unrelated tasks;
- depends on exact literal keywords even though the underlying intent has common paraphrases.

## Template

```yaml
description: |
  Use when the user needs {intent / information need / task situation}.
  Also use when {semantic paraphrase or adjacent in-scope situation}.
  Do not use when {neighbor intent}; use {neighbor-skill} instead.
```

Literal phrases may be added after the semantic route when they improve discoverability, but they are examples rather than a closed trigger list.

## Examples

Bad:

```yaml
description: |
  This skill provides a workflow for diagnosing server upgrade problems.
```

Better:

```yaml
description: |
  Use when a server upgrade, middleware change, restart, deployment, or configuration update causes a production service to fail, regress, or behave differently than before.
```

Bad:

```yaml
description: |
  A skill for creating database migration plans.
```

Better:

```yaml
description: |
  Use when the user needs a safe database migration plan, dry-run strategy, rollback design, batch execution guard, or production data-change procedure.
```

Bad:

```yaml
description: |
  Helps design Agent Skills.
```

Better:

```yaml
description: |
  Use when the user asks how to design an Agent Skill, decide whether a Skill is the right container, determine whether one of the named Patterns applies, split SKILL.md and references, define routing conditions, or decide whether content belongs in a Skill, AGENTS.md, script, prompt, or reference file. Do not use to build, test, package, or publish the Skill; use skill-forge.
```

## Semantic Trigger Example

Suppose a source-specialist Skill should handle remembered Chinese long-form articles.

Too literal:

```yaml
description: Use when the user says WeChat, 微信, or 公众号.
```

Better:

```yaml
description: |
  Use when the user wants to discover Official Account articles or is trying to find a remembered Chinese long-form article for which the WeChat ecosystem is a high-fit source. Chinese language alone is not sufficient.
```

This lets the route activate from meaning without claiming that the underlying search backend itself performs semantic/vector retrieval.

## Neighbor Boundary Template

```markdown
## Neighbor Boundary

- `skill-a`: Use this Skill when ...
- `skill-b`: Use `skill-b` when ...
- Ambiguous phrase: "..."
  - Route to this Skill if ...
  - Route to neighbor if ...
```

## Trigger Eval Set

At minimum, test behavior that could falsify the route contract:

```markdown
Should load:
- [realistic direct phrase]
- [semantic paraphrase without the obvious literal trigger]

Should not load:
- [neighbor intent]
- [generic/stable query]

Ambiguous:
- phrase: "..."
  expected route: ...
  reason: ...
```

Do not keep adding paraphrase cases once new runs no longer change the routing conclusion.
