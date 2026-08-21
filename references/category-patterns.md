# Category, Local Behavior, and Coordination Mapping

This reference separates three different questions:

1. **Category** — what kind of problem/domain is this Skill for?
2. **Local Behavior Pattern** — how does the Skill itself behave after loading?
3. **Coordination** — does the Skill transfer or delegate responsibility to other independent executors?

Do not collapse these axes into one taxonomy.

## Category Classification — Provisional

The category vocabulary is not frozen yet. Treat this table as working guidance derived from existing practice, not as a closed set.

| Category | Content focus | Natural local behavior fit |
| --- | --- | --- |
| Library / API reference | Correct usage, API calls, known gotchas | Tool Wrapper |
| Product validation | Acceptance criteria, edge cases, review rubric | Reviewer / Pipeline |
| Data retrieval and analysis | Data sources, query format, auth, output contract | Tool Wrapper / Pipeline |
| Business workflow automation | Trigger conditions, state transitions, error handling | Pipeline |
| Code scaffolding | Templates, naming rules, generated files | Generator |
| Code quality review | Checklist, severity levels, examples | Reviewer |
| CI/CD deployment | Gate order, rollback, verification | Pipeline |
| Runbook / diagnostics | Symptoms, evidence, hypotheses, report structure | Pipeline + Inversion |
| Infrastructure operations | Procedure, safety gates, rollback strategy | Pipeline + Tool Wrapper |
| Skill governance | Routing, evals, packaging, retirement | Pipeline / Reviewer |
| Knowledge capture | Failure modes, lessons learned, examples | No fixed pattern implied |

If the request spans categories, select one dominant category only when that improves design clarity. Record secondary categories as context, not as a substitute for Local Behavior analysis.

## Canonical Local Behavior Patterns

Only these five names are Local Behavior Patterns:

| Pattern | Use when | Common files |
| --- | --- | --- |
| Tool Wrapper | The Skill teaches correct use of an external tool, CLI, API, SDK, MCP, or library | `SKILL.md`, `references/api.md`, optional `scripts/` |
| Generator | The Skill produces structured content, code, config, docs, prompts, or templates | `SKILL.md`, `assets/template.*`, `references/schema.md` |
| Reviewer | The Skill judges, audits, scores, verifies, or critiques something | `SKILL.md`, `references/checklist.md`, optional `scripts/score.*` |
| Inversion | The Skill gathers information before advice or action because missing input would materially change the result | `SKILL.md`, `references/question-bank.md` |
| Pipeline | The Skill has meaningful ordered phases, dependency, progression rules, or validation gates | `SKILL.md`, `references/workflow.md`, optional `scripts/validate.*` |

`Stateful / Memory`, `Reference-heavy`, `Hybrid`, `Router`, and `Orchestrator` are not Local Behavior Patterns.

### Dominant Pattern Rubric

A dominant pattern is optional.

Use this test:

1. Temporarily ignore inter-Skill / inter-agent Coordination.
2. Inspect the Skill's major successful execution paths.
3. Choose a dominant pattern only if one canonical pattern remains the stable behavior skeleton across those major paths and directly supports the core outcome.
4. Use `none` when the Skill is primarily a meta-controller or policy selector whose major paths legitimately use different canonical patterns and no one pattern is invariantly dominant.
5. Also use `none` when removing Coordination leaves no independent user-facing local behavior.

Do not classify a Skill as Pipeline merely because it has numbered instructions. Pipeline requires meaningful ordered dependency, phase progression, or validation gates.

## Pattern Composition

Composition does not create new pattern names.

Represent composition as:

```text
Dominant: <one canonical pattern | none>
Secondary: <zero or more canonical patterns>
```

Examples:

- Pipeline + Inversion: diagnostics workflow that must gather evidence before later gated phases
- Pipeline + Tool Wrapper: deployment workflow that must use CLI commands correctly inside ordered stages
- Reviewer + Generator: review an artifact, then produce a corrected version

`Hybrid` means composition; it is not a sixth pattern.

If `Dominant: none`, explain why no canonical pattern is stable across the major execution paths. Do not use `none` as an escape hatch for incomplete analysis.

## Coordination

Coordination is orthogonal to Local Behavior.

### Coordination Mechanism — Closed Vocabulary

| Mechanism | Definition |
| --- | --- |
| `none` | No task responsibility is assigned to another independent Skill / agent / executor. |
| `handoff` | Primary task ownership transfers to another independent Skill / agent / executor. |
| `delegate` | The current Skill retains overall ownership while assigning a bounded subtask to another independent Skill / agent / executor and consuming its result. |

Decision rule:

```text
Does another independent executor receive task responsibility?
  no -> none
  yes -> Does current Skill retain overall ownership?
           no -> handoff
           yes -> delegate
```

Boundary rules:

- Frontmatter `Do not use ... use <neighbor>` is routing, not runtime handoff.
- Calling a tool, script, API, reference file, or processing multiple data items does not by itself create Coordination.
- Parallelism alone does not imply delegation; identify the executor and ownership boundary first.

### Coordination Strategy — Open Vocabulary

Strategy names are intentionally extensible. They describe how a coordination mechanism is applied, not a new mechanism type.

Useful descriptors include:

| Dimension | Example values |
| --- | --- |
| Selection policy | static / dynamic |
| Cardinality | one / many |
| Scheduling | sequential / parallel / adaptive |
| Aggregation | none / synthesize / compare / review / domain-specific |
| Termination | handoff-complete / all-complete / first-success / confidence threshold / explicit stop / domain-specific |

A Skill may use names such as `Classify-and-act` or `Fan-out-and-synthesize`. Keep those in Strategy; do not add them to the Mechanism enum.

## Capability — Provisional

Capability is a separate cross-cutting axis and is not frozen yet.

Candidates such as state, memory, persistent audit, reference-heavy organization, deterministic validation scripts, or similar properties must not be promoted into the Local Behavior Pattern namespace while the Capability taxonomy is still under validation.
