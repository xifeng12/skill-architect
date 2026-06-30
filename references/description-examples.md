# Description Examples

The frontmatter `description` is a routing trigger, not a feature summary.

## Good Description Rules

A good description:

- starts with "Use when..." or "Load when..."
- describes user intent, not internal implementation
- includes realistic user phrasing
- names important exclusions when nearby Skills exist
- is short enough to act as a routing trigger
- avoids listing every capability
- avoids workflow summaries
- avoids marketing language

## Bad Description Smells

Refactor the description if it:

- starts with "This skill provides..."
- explains the whole workflow
- lists internal files
- says "helps with X" without trigger conditions
- uses vague words like "improve", "optimize", "support", "assist"
- overlaps with another Skill without boundary language
- is so broad that it could load for many unrelated tasks

## Template

```yaml
description: |
  Use when the user asks [intent], [intent], or [intent]. Also use for phrases like "[real phrase]" and "[real phrase]". Do not use when [neighbor intent]; use [neighbor-skill] instead.
```

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
  Use when the user needs a safe database migration plan, dry-run strategy, rollback design, batch execution guard, or production data change checklist.
```

Bad:

```yaml
description: |
  Helps design Agent Skills.
```

Better:

```yaml
description: |
  Use when the user asks how to design an Agent Skill, choose a skill pattern, split SKILL.md and references, define routing conditions, or decide whether content belongs in a Skill, AGENTS.md, script, prompt, or reference file. Do not use to build or package the Skill; use skill-forge.
```

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

Every description should have at least these tests:

```markdown
Should load:
- [realistic user phrase]
- [realistic user phrase]
- [edge phrase still inside scope]

Should not load:
- [neighbor intent]
- [generic query]
- [one-off task better handled without Skill]

Ambiguous:
- phrase: "..."
  expected route: ...
  reason: ...
```
