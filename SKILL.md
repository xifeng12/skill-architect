---
name: skill-architect
description: |
  Use when the user asks how to design an Agent Skill, choose a skill pattern, split SKILL.md and references, write routing descriptions, or decide whether content belongs in a Skill, AGENTS.md, script, prompt, or reference. Also use for Chinese skill-design requests like "这个 skill 怎么设计". Do not use to create, test, package, publish, or optimize a Skill; use skill-forge. Do not use for general task escalation; use dynamic-workflow.
---

# Skill Architect

Design Agent Skills as routable, reusable context modules.

This Skill is for architecture decisions, not full implementation.

It helps decide:

- whether something should become a Skill
- when the Skill should load
- which local behavior pattern fits, if any
- whether runtime coordination is needed
- how to split `SKILL.md`, `references/`, `assets/`, and `scripts/`
- how to avoid routing overlap with neighboring Skills

## Core Principle

Design the route before designing the body.

A Skill is useful only if the agent loads it at the right time and does not load it at the wrong time.

The frontmatter `description` is a routing trigger, not a feature summary.

Prefer:

```yaml
description: Use when the user asks ...
```

Avoid:

```yaml
description: This skill provides ...
```

## Route Contract

Load when the user asks about:

- designing an Agent Skill
- choosing a Skill architecture pattern
- deciding whether a workflow should become a Skill
- deciding whether content belongs in `AGENTS.md`, a Skill, a reference file, a script, or a prompt
- splitting `SKILL.md` and `references/`
- writing or improving Skill routing descriptions
- preventing route collision between neighboring Skills
- converting recurring operational experience, gotchas, or workflows into reusable context

Common Chinese triggers:

- "这个 skill 应该怎么设计？"
- "这个适合做成 skill 吗？"
- "应该放 AGENTS.md 还是 skill？"
- "这个 skill 应该用什么模式？"
- "SKILL.md 和 references 怎么拆？"
- "这个 description 怎么写更容易触发？"
- "这个 workflow 要不要沉淀成 skill？"

Do not load when the user asks to:

- create, test, package, publish, or optimize a Skill
- run Skill evals
- build `.skill` artifacts
- execute the workflow that another specialized Skill already covers
- solve a one-off task where no reusable Skill design is needed
- escalate a broad uncertain task into a multi-agent or multi-step workflow

Neighbor boundary:

| User intent | Preferred route |
| --- | --- |
| "这个 skill 应该怎么设计？" | `skill-architect` |
| "帮我创建并测试这个 skill" | `skill-forge` |
| "把这个 skill 打包发布" | `skill-forge` |
| "评估这个 skill 是否退役" | `skill-forge` |
| "自动迭代优化这个 skill" | `skill-forge` or dedicated optimization Skill if present |
| "这个复杂任务该不该升级成 workflow？" | `dynamic-workflow` |
| "这个生产变更怎么安全执行？" | safe-change / production safety Skill |

Route boundaries are not runtime coordination. If routing selects a neighboring Skill before this Skill loads, record that under Route, not Coordination.

## Minimal Workflow

Follow this order:

```text
1. Define route contract
2. Decide Skill / non-Skill container
3. Classify category (provisional taxonomy)
4. Classify local behavior using the canonical five-pattern set
5. Classify runtime coordination separately
6. Identify neighboring Skills and route collisions
7. Design root SKILL.md responsibility
8. Split long material into references/assets/scripts
9. Define trigger tests
10. Produce architecture recommendation
```

## Skill / Non-Skill Decision

Use a Skill when most are true:

- the workflow repeats across sessions or projects
- the agent regularly makes domain-specific mistakes without guidance
- the task needs routing by user intent
- the guidance should load only when relevant
- the process has stable constraints, gates, gotchas, or templates
- the workflow benefits from structured references, scripts, or assets

Do not recommend a Skill when most are true:

- it is a one-off instruction
- it is better expressed as a script
- it is better stored as project documentation
- it is a stable global behavior rule that should always apply
- it is only a short prompt template
- it changes so often that reuse would create stale guidance

Container decision:

| Best container | Use when |
| --- | --- |
| `AGENTS.md` | Always-on workspace rule, safety invariant, routing trigger, hard stop condition |
| Skill | Reusable workflow loaded only for certain user intents |
| `references/` | Long explanation, examples, API notes, checklists, gotchas |
| `assets/` | Templates, schemas, sample files, reusable prompt blocks |
| `scripts/` | Deterministic validation, formatting, conversion, scoring, setup |
| Prompt | One-off wording or temporary instruction |

## Local Behavior Pattern

Only these five names are canonical **Local Behavior Patterns** in this Skill Architect:

```text
Needs external tool/API correctness? -> Tool Wrapper
Needs structured generation? -> Generator
Needs checking, audit, or judgment? -> Reviewer
Needs discovery before action? -> Inversion
Needs ordered phases with dependency or validation gates? -> Pipeline
```

Do not add `Stateful / Memory`, `Reference-heavy`, `Hybrid`, `Router`, or `Orchestrator` to the pattern namespace. They answer different architectural questions.

### Dominant Pattern Test

A dominant Local Behavior Pattern is optional, not mandatory.

1. Temporarily ignore inter-Skill / inter-agent coordination.
2. Inspect the Skill's major successful execution paths.
3. Select a dominant pattern only when one of the canonical five is the stable behavior skeleton across those major paths and is responsible for the core outcome.
4. Use `none` when the Skill is primarily a meta-controller or policy selector whose major paths legitimately use different canonical patterns and no one pattern remains invariantly dominant.
5. Also use `none` when removing Coordination leaves no independent user-facing local behavior.

Do not choose Pipeline merely because a Skill contains numbered steps. Pipeline requires meaningful ordered dependency, phase progression, or validation gates.

Secondary patterns are allowed when they describe genuine supporting behavior. `Hybrid` is not a sixth pattern; it means dominant + secondary composition.

For detailed mapping, read `references/category-patterns.md` and `references/design-patterns.md`.

## Coordination

Classify runtime coordination separately from Local Behavior.

### Coordination Mechanism

The mechanism vocabulary is closed:

| Mechanism | Meaning |
| --- | --- |
| `none` | No task responsibility is assigned to another independent Skill / agent / executor. |
| `handoff` | The current Skill transfers primary task ownership to another independent Skill / agent / executor. |
| `delegate` | The current Skill retains overall ownership while assigning a bounded subtask to another independent Skill / agent / executor and consuming its result. |

Rules:

- Neighbor routing in frontmatter or `Do not load` boundaries is Route, not Coordination.
- Tool calls, scripts, references, files, APIs, or parallel data processing are not Coordination by themselves.
- `handoff` is about ownership transfer; `delegate` is about ownership retention.

### Coordination Strategy

Strategy is intentionally open, not a closed taxonomy.

A strategy may describe:

- selection policy: static / dynamic
- cardinality: one / many
- scheduling: sequential / parallel / adaptive
- aggregation: none / synthesize / compare / review / other
- termination: handoff-complete / all-complete / first-success / confidence threshold / explicit stop / other

Skills may use domain-specific strategy names such as `Classify-and-act` or `Fan-out-and-synthesize`. Do not promote those names into the Coordination Mechanism enum.

## Category and Capability Status

Category and Capability are separate architectural dimensions, but their canonical vocabularies are **not frozen yet**.

Current category tables are provisional guidance, not a closed taxonomy.

State, memory, reference-heavy organization, deterministic validation scripts, persistent audit, and similar cross-cutting properties must not be treated as Local Behavior Patterns. Record them as capability candidates until the Capability taxonomy is validated against real Skills.

## Root File Responsibility

Keep root `SKILL.md` small and operational.

Keep in root:

- route contract
- use / do-not-use boundary
- core principle
- minimal workflow
- local behavior fast path
- coordination contract when relevant
- safety gates
- output contract
- reference routing

Move out of root:

- long examples
- directory templates
- large classification tables
- detailed gotcha guidance
- anti-pattern lists
- detailed capture checklists
- historical lessons

Reference routing:

- Read `references/description-examples.md` when improving or reviewing frontmatter descriptions.
- Read `references/category-patterns.md` when category, local behavior, or coordination choice is unclear.
- Read `references/design-patterns.md` for the canonical five Local Behavior Patterns and composition rules.
- Read `references/capture-guide.md` when collecting information before designing a Skill.
- Read `references/directory-templates.md` when proposing a file structure.
- Read `references/gotchas-guide.md` when extracting domain gotchas or lessons learned.
- Read `references/anti-patterns.md` when reviewing an existing Skill for design problems.
- Read `references/output-format.md` when producing a full architecture recommendation.

## Quality Checklist

Before delivering a Skill architecture, verify:

- [ ] Description is written as a route trigger, not a feature summary
- [ ] Load and do-not-load cases are explicit
- [ ] Neighbor Skills are named when relevant
- [ ] Neighbor routing is not mislabeled as runtime handoff
- [ ] Skill / non-Skill decision is justified
- [ ] Local Behavior uses only the canonical five pattern names, or `none` with a stated reason
- [ ] A dominant pattern is selected only when it passes the stable-behavior test
- [ ] Secondary pattern does not bloat the design
- [ ] Coordination mechanism, when present, is `handoff` or `delegate` with ownership semantics stated
- [ ] Coordination strategy is kept separate from mechanism
- [ ] Stateful / Memory, Reference-heavy, and Hybrid are not mislabeled as patterns
- [ ] Root `SKILL.md` remains small and operational
- [ ] Long guidance is pushed to `references/`
- [ ] Deterministic checks are pushed to `scripts/`
- [ ] Templates and schemas are pushed to `assets/`
- [ ] At least two gotchas are identified or requested
- [ ] Basic trigger tests are included

## Relationship to Other Skills

`skill-architect` is upstream of implementation Skills.

Use it to decide architecture.

Use `skill-forge` when the user asks to create, test, package, publish, optimize, evaluate, or retire a Skill.

Use `dynamic-workflow` when the user asks whether a broad, uncertain, multi-step task should be escalated into a structured workflow.

Use safe-change or production safety Skills when the task involves risky production-like changes.

Boundary rule:

```text
Architecture-only question -> skill-architect
Build / test / package request -> skill-forge
General complex task escalation -> dynamic-workflow
Production-risk execution -> safe-change discipline
```

## Final Response Contract

When responding as Skill Architect, include:

- clear recommendation
- route contract
- Skill / non-Skill decision
- category, explicitly marked provisional when relevant
- Local Behavior: dominant / secondary / none with reason
- Coordination Mechanism and Strategy when runtime coordination exists
- capability candidates, explicitly marked provisional
- suggested structure
- root `SKILL.md` plan
- reference split plan
- boundary with neighboring Skills
- gotchas to capture
- validation method

For full recommendations, use `references/output-format.md`.

If confidence is not high, state what evidence is missing.

Do not claim a Skill is production-ready unless trigger evals, content checks, and neighbor confusion cases have been considered.
