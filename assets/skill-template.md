# SKILL.md Architecture Templates

These are architecture examples, not mandatory boilerplate.

Start from the route contract and reusable behavior. Do not begin by choosing a Pattern or filling every section.

## Minimal / Pattern-none Template

Use when no named Pattern is needed.

```markdown
---
name: {skill-name}
description: |
  Use when {describe the user intent / situation that should load this Skill}.
  Do not use when {closest negative route or neighbor boundary}.
---

# {Skill title}

{One-sentence goal.}

## Core behavior

- {decision / rule / reusable behavior}
- {decision / rule / reusable behavior}

## Stop

Stop when {goal-dependent sufficiency condition}.
```

This is a valid complete Skill if it reliably changes the intended behavior.

## Frontmatter Guidance

| Field | Purpose | Example |
|---|---|---|
| `name` | Stable Skill identifier | `skill-architect` |
| `description` | Semantic route contract | `Use when the user asks how to design an Agent Skill...` |

### Description rule

Prefer:

```yaml
description: |
  Use when the user asks {intent / information need / task situation}.
  Do not use when {close negative route}.
```

Literal trigger phrases may be included as examples when useful, but they are not the routing contract.

Avoid feature-summary descriptions such as:

```yaml
description: This skill provides tools for ...
```

## Named Pattern Templates

Use these only after deciding that the named Pattern materially explains the Skill's core behavior.

### Tool Wrapper

```markdown
---
name: {tool-name}-guide
description: |
  Use when the user needs to {perform a task whose correctness depends on using tool-name correctly}.
  Do not use when {the task does not depend on this tool or a better specialist owns it}.
---

# {Tool Name}

## Core usage contract

- {what semantic job the tool owns}
- {critical parameter / scope / gotcha}
- {what to do when the tool is unavailable}

Read `references/tool-usage.md` only when detailed parameters or errors are needed.
```

Do not embed installer/config repair unless environment mutation is explicitly part of the Skill's authorized goal.

### Generator

```markdown
---
name: {name}-generator
description: |
  Use when the user asks to create {structured artifact} under {stable constraints}.
---

# {Name} Generator

## Required information

- {field}: {why required}

## Generation contract

1. Use the supplied information and constraints.
2. Ask only for genuinely blocking missing information.
3. Produce the artifact using `assets/template.md` when the template is actually needed.
4. Validate mandatory constraints.
```

### Reviewer

```markdown
---
name: {name}-reviewer
description: |
  Use when the user asks to review, audit, verify, or critique {target} against {criteria/domain}.
---

# {Name} Reviewer

## Review contract

- Read the target.
- Apply the relevant criteria from `references/checklist.md`.
- Separate evidence, inference, and unknowns when that affects the verdict.
- Stop when additional checking will not change the verdict, severity, or next action.
```

### Inversion

```markdown
---
name: {name}
description: |
  Use when advice or action about {domain} would be unreliable until key missing information is gathered.
---

# {Name}

## Information gate

Before acting, determine which missing facts are genuinely blocking.

Ask only for those facts that cannot be resolved from available context/tools.

Proceed as soon as the gate is satisfied.

Do not repeat questions the user or environment has already answered.
```

Inversion does **not** imply a fixed questionnaire or a rule that every phase must always run.

### Pipeline

```markdown
---
name: {name}
description: |
  Use when {task} requires multiple phases or gates whose ordering materially affects correctness.
---

# {Name}

## Phases

### Phase 1 — {name}

Goal: {goal}
Exit when: {condition}

### Phase 2 — {name}

Enter only when: {condition}
Goal: {goal}
Exit when: {condition}

## Stop

Do not execute later phases merely because they exist. Stop once the user's goal is satisfied or the next phase is not authorized/needed.
```

## Local Behavior Notes

Add only when needed.

Examples:

```markdown
## Runtime capability behavior

- Keep UNKNOWN distinct from confirmed missing.
- Separate operational availability from semantic coverage.
- Use degraded fallback only with a concrete reason and disclose material coverage loss.
- Do not mutate the environment unless authorized.
```

```markdown
## State requirement

Persist {specific state} because {cross-run/audit need}.
```

```markdown
## Coordination

Use bounded parallel work only for {independent tracks}. Do not fan out simple tasks.
```

Do not turn these notes into new Pattern names.

## Naming

- Skill name: lowercase kebab-case when supported by the target runtime, e.g. `code-reviewer`.
- Directory name: normally match the Skill name.
- Reference/asset/script names: descriptive and scoped to actual need.
