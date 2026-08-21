---
name: skill-architect
description: |
  Use when the user asks how to design an Agent Skill, decide whether a Skill is the right container, determine whether one of the named Skill design Patterns applies, split SKILL.md and references, write routing descriptions, or decide whether content belongs in a Skill, AGENTS.md, script, prompt, or reference. Also use for Chinese skill-design requests like "这个 skill 怎么设计". Do not use to create, test, package, publish, or optimize a Skill; use skill-forge. Do not use for general task escalation; use dynamic-workflow.
---

# Skill Architect

Design Agent Skills as routable, reusable context modules.

This Skill is for architecture decisions, not full implementation.

It helps decide:

- whether something should become a Skill
- when the Skill should load
- whether one of the five named Patterns actually fits, or `Pattern = none`
- how to split `SKILL.md`, `references/`, `assets/`, and `scripts/`
- how to avoid routing overlap with neighboring Skills
- which local behaviors matter without turning them into new Pattern names

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

Literal trigger phrases are examples. The route contract should describe user intent and information need well enough to support semantic matching.

## Route Contract

Load when the user asks about:

- designing an Agent Skill
- deciding whether a workflow should become a Skill
- deciding whether content belongs in `AGENTS.md`, a Skill, a reference file, a script, or a prompt
- deciding whether one of the five named Patterns fits or no named Pattern is needed
- splitting `SKILL.md` and `references/`
- writing or improving Skill routing descriptions
- preventing route collision between neighboring Skills
- converting recurring operational experience, gotchas, or workflows into reusable context

Common Chinese triggers:

- "这个 skill 应该怎么设计？"
- "这个适合做成 skill 吗？"
- "应该放 AGENTS.md 还是 skill？"
- "这个 skill 需要 pattern 吗？"
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

## Minimal Workflow

Follow this order:

```text
1. Define route contract
2. Decide Skill / non-Skill container
3. Identify the reusable behavior and scene/category
4. Decide whether any named Pattern materially fits; allow none
5. Record optional local behaviors separately
6. Identify neighbor Skills
7. Design root SKILL.md responsibility
8. Split long material into references/assets/scripts only when justified
9. Define trigger/content/runtime validation
10. Produce architecture recommendation
```

Do not force a Pattern label merely to complete the workflow.

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
| `AGENTS.md` | Always-on workspace rule, safety invariant, hard stop condition, or global project contract |
| Skill | Reusable workflow loaded only for certain user intents |
| `references/` | Long explanation, examples, API notes, checklists, gotchas |
| `assets/` | Templates, schemas, sample files, reusable prompt blocks |
| `scripts/` | Deterministic validation, formatting, conversion, scoring, setup |
| Prompt | One-off wording or temporary instruction |

## Named Pattern Boundary

Reserve the word **Pattern** for these five names:

| Pattern | Use when |
| --- | --- |
| Tool Wrapper | The Skill's core reusable behavior is correct use of an external tool, CLI, API, SDK, MCP, or library |
| Generator | The Skill's core behavior is producing structured artifacts from constraints/templates |
| Reviewer | The Skill's core behavior is judging, auditing, verifying, or critiquing against criteria |
| Inversion | The Skill must deliberately gather required information before advice/action |
| Pipeline | The Skill's core behavior is coordinating multiple phases/gates/artifacts whose ordering matters |

If none materially explains the Skill, use:

```text
Pattern = none
```

Do not rename local architecture characteristics as new Patterns.

For detailed pattern definitions, read `references/design-patterns.md`.

## Local Behaviors

Local behaviors are optional architecture notes, not a mandatory taxonomy.

Examples include:

- state/history/audit requirements
- reference-heavy content strategy
- coordination shape
- runtime capability handling
- source ownership and fallback behavior

A Skill may have none of these.

Do not force coordination into a closed enum unless the project has evidence that such a taxonomy is stable and useful.

## Runtime-Informed Architecture Checks

When a Skill depends on tools, providers, plugins, MCP servers, or runtime-specific capabilities, keep these questions separate:

1. **Operational status** — is the capability actually callable in this runtime?
2. **Semantic coverage** — if used, does it cover the user's required source/meaning equivalently, or only partially?

Do not collapse `UNKNOWN` into `MISSING` merely because a capability was not observed in one inventory or session.

A degraded fallback can be architecturally valid when the preferred path is not currently viable, the fallback is actually available, it can make progress, and any meaningful coverage loss is disclosed.

Missing or unverified capability does not by itself authorize installation, MCP edits, PATH changes, TLS repair, or other environment mutation. Treat capability discovery and environment mutation as separate phases.

Different tool names or command shapes do not by themselves prove independent failure domains. If runtime evidence shows two paths share a control plane/failure domain, stop enumerating siblings from that same domain unless a new path serves the current goal.

Read `references/runtime-evidence-external-knowledge-v0.2.1.md` when designing runtime-dependent routing, fallback, capability, or stop behavior.

## Root File Responsibility

Keep root `SKILL.md` small and operational.

Keep in root:

- route contract
- use / do-not-use boundary
- core principle
- minimal workflow
- named Pattern choice only when useful
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
- runtime evidence logs

Reference routing:

- Read `references/description-examples.md` when improving or reviewing frontmatter descriptions.
- Read `references/category-patterns.md` when category, named Pattern, or local behavior choice is unclear.
- Read `references/design-patterns.md` for the five named Pattern definitions.
- Read `references/capture-guide.md` when collecting information before designing a Skill.
- Read `references/directory-templates.md` when proposing a file structure.
- Read `references/gotchas-guide.md` when extracting domain gotchas or lessons learned.
- Read `references/anti-patterns.md` when reviewing an existing Skill for design problems.
- Read `references/runtime-evidence-external-knowledge-v0.2.1.md` for runtime-dependent routing/fallback evidence.
- Read `references/output-format.md` when producing a full architecture recommendation.

## Quality Checklist

Before delivering a Skill architecture, verify:

- [ ] Description is written as a route trigger, not a feature summary
- [ ] Load and do-not-load cases are explicit
- [ ] Neighbor Skills are named when relevant
- [ ] Skill / non-Skill decision is justified
- [ ] A named Pattern is selected only if it materially fits; otherwise `none`
- [ ] Local behaviors are not mislabeled as Patterns
- [ ] Root `SKILL.md` remains small and operational
- [ ] Long guidance is pushed to `references/`
- [ ] Deterministic checks are pushed to `scripts/` when they are truly needed
- [ ] Templates and schemas are pushed to `assets/` when they are truly needed
- [ ] Runtime-dependent claims distinguish operational status from semantic coverage
- [ ] `UNKNOWN` capability is not silently rewritten as `MISSING`
- [ ] Fallback and further phases have explicit reasons and stop conditions
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
- dominant category when useful
- named Pattern choice, composition, or `none`
- relevant local behavior notes without forcing a taxonomy
- suggested structure
- root `SKILL.md` plan
- reference split plan
- boundary with neighboring Skills
- gotchas to capture when evidence exists
- validation method

For full recommendations, use `references/output-format.md`.

If confidence is not high, state what evidence is missing.

Do not claim a Skill is production-ready unless trigger evals, content checks, neighbor confusion cases, and any runtime-dependent capability assumptions have been considered.
