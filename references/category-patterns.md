# Category, Pattern, and Local Behavior Mapping

Category, Pattern, and local behavior answer different questions.

- **Category** describes the kind of problem/content.
- **Pattern** is optional and uses only one of the five named design Patterns.
- **Local behavior** captures architecture characteristics that matter locally but should not be promoted into new Pattern names.

Do not force one axis to stand in for another.

## Category Classification

| Category | Content focus | Possible named Pattern fit |
| --- | --- | --- |
| Library / API reference | Correct usage, API calls, known gotchas | Tool Wrapper, or none |
| Product validation | Acceptance criteria, edge cases, review rubric | Reviewer; sometimes Pipeline |
| Data retrieval and analysis | Source semantics, query behavior, evidence, output contract | none; sometimes Tool Wrapper / Pipeline |
| Business workflow automation | Trigger conditions, state transitions, error handling | Pipeline; sometimes none |
| Code scaffolding | Templates, naming rules, generated files | Generator |
| Code quality review | Checklist, severity levels, examples | Reviewer |
| CI/CD deployment | Gate order, rollback, verification | Pipeline |
| Runbook / diagnostics | Symptoms, evidence, hypotheses, report structure | Inversion and/or Pipeline when those behaviors are truly central |
| Infrastructure operations | Procedure, safety gates, rollback strategy | Pipeline and/or Tool Wrapper when they truly fit |
| Skill governance | routing, evals, packaging, retirement | Reviewer / Pipeline, or none |
| Knowledge capture | failure modes, lessons learned, examples | none by default; may be reference-heavy as a content strategy |

If the request spans categories, identify the category that best explains the reusable behavior. Secondary categories may guide reference organization, but do not force a dominant Pattern merely because a category row lists one.

## Named Pattern Selection

Reserve **Pattern** for these five names:

| Pattern | Use when | Common files |
| --- | --- | --- |
| Tool Wrapper | Correct use of an external tool, CLI, API, SDK, MCP, or library is the Skill's core reusable behavior | `SKILL.md`, `references/api.md`, optional `scripts/` |
| Generator | Producing structured content/code/config/docs from constraints/templates is the core behavior | `SKILL.md`, `assets/template.*`, `references/schema.md` |
| Reviewer | Judging, auditing, scoring, verifying, or critiquing against criteria is the core behavior | `SKILL.md`, `references/checklist.md`, optional deterministic checks |
| Inversion | The Skill must gather required information before advice/action | `SKILL.md`, optional `references/question-bank.md` |
| Pipeline | Coordinating multiple phases/gates/artifacts whose ordering matters is the core behavior | `SKILL.md`, optional `references/workflow.md`, validation scripts when justified |

If none materially explains the Skill, record:

```text
Pattern = none
```

Do not select a Pattern solely because every architecture report has a Pattern field.

## Pattern Composition

More than one of the five named Patterns may genuinely contribute to a Skill.

Describe that as **composition**, not as a sixth `Hybrid` Pattern.

Examples:

- Pipeline + Inversion: a multi-phase workflow whose first phase must gather missing evidence.
- Pipeline + Generator: a multi-stage artifact-generation process.
- Inversion + Reviewer: gather required context before assessment.
- Tool Wrapper + Pipeline: a multi-phase workflow in which correct tool use is also central.

Do not require every composition to declare a dominant Pattern. If one clearly dominates, say so; otherwise describe the composition without inventing hierarchy.

## Local Behaviors

Local behaviors are optional. `none` is valid.

Examples:

### Reference-heavy content strategy

Use when the root contract should stay small and most domain detail is loaded conditionally from `references/`.

This is a content strategy, not a Pattern.

### Stateful requirement

Use when history, persisted configuration, audit trail, or cross-run continuity is genuinely required.

Statefulness is a requirement/implementation characteristic, not a Pattern name.

Do not add state files merely because a Skill has multiple steps.

### Coordination shape

Describe concrete coordination behavior only when it matters: for example, bounded parallel research, sequential handoff, or synthesis after independent tracks.

Do not force coordination into a closed enum until project evidence supports a stable taxonomy. Different labels can describe different abstraction levels.

### Runtime capability policy

For Skills that depend on providers/tools/plugins/MCP:

- separate `operational_status` from semantic `coverage`;
- keep `UNKNOWN` distinct from confirmed absence;
- do not make installation/repair an implicit fallback;
- reason about failure-domain independence from evidence, not tool names.

See `runtime-evidence-external-knowledge-v0.2.1.md` for the project evidence behind these rules.

## Decision Rule

Use this order:

```text
Reusable behavior
    ↓
Category (if useful)
    ↓
Does one of the five named Patterns materially explain the core behavior?
    ├─ no  → Pattern = none
    └─ yes → select one or describe real composition
    ↓
Add only the local behaviors actually required
```

The goal is explanatory value, not taxonomy coverage.
