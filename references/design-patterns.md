# Canonical Local Behavior Patterns

This reference defines the five canonical **Local Behavior Patterns** used by Skill Architect.

These five names form a closed pattern namespace:

1. Tool Wrapper
2. Generator
3. Reviewer
4. Inversion
5. Pipeline

Do not add Stateful / Memory, Reference-heavy, Hybrid, Router, Orchestrator, or coordination strategy names to this list. Those belong to other architectural dimensions.

A Skill is not required to have a dominant Local Behavior Pattern. Use `none` only when no canonical pattern is stable across the Skill's major execution paths, or when no independent local user-facing behavior remains after runtime coordination is removed.

---

## 1. Tool Wrapper

**Purpose**: teach correct use of an external library, CLI, API, SDK, MCP, or tool.

```text
skill-name/
├── SKILL.md           # invocation rules, safety boundaries, common usage
├── references/
│   └── api-docs.md    # compact API notes, parameters, gotchas
└── scripts/           # optional deterministic wrappers or checks
```

**Design signals**:

- the model already knows the domain but repeatedly uses a tool incorrectly
- tool-specific conventions, auth, parameters, deprecations, or gotchas matter
- correct tool use is the core local outcome

Do not classify something as Tool Wrapper merely because it happens to call a tool.

---

## 2. Generator

**Purpose**: produce structured content or artifacts under explicit output constraints.

```text
skill-name/
├── SKILL.md           # generation contract and required fields
├── references/
│   └── conventions.md # rules and domain constraints
└── assets/
    └── template.md    # reusable output template
```

**Design signals**:

- success is primarily defined by the generated artifact
- output schema, template, naming, or required fields materially constrain quality
- generation is the stable local behavior across major execution paths

---

## 3. Reviewer

**Purpose**: judge, audit, critique, verify, score, or compare an artifact, plan, state, or result.

```text
skill-name/
├── SKILL.md           # review protocol and output contract
├── references/
│   └── checklist.md   # rubric, standards, severity definitions
└── scripts/
    └── score.*        # optional deterministic checks
```

**Design signals**:

- the main outcome is a judgment or finding set
- acceptance criteria, evidence, severity, or comparison matter
- the Skill should challenge or validate rather than mainly create

---

## 4. Inversion

**Purpose**: gather missing information before advice or action when incomplete input would materially change the result.

```text
skill-name/
├── SKILL.md           # information-gathering contract and completion criteria
├── references/
│   └── questions.md   # optional question bank
└── assets/
    └── form.md        # optional structured capture template
```

**Design signals**:

- acting before information capture creates material risk or low-quality output
- the Skill must expose uncertainty and collect missing facts first
- information acquisition, not merely "asking a question", is the stable local behavior

Do not require a rigid interview sequence unless the task genuinely needs ordered gates; otherwise that rigidity belongs neither to Inversion nor Pipeline by default.

---

## 5. Pipeline

**Purpose**: enforce meaningful ordered phases where dependency, progression rules, or validation gates matter.

```text
skill-name/
├── SKILL.md           # phase contract, gate conditions, transition rules
├── references/
│   └── workflow.md    # detailed phase rules
└── scripts/
    └── validate.*     # optional deterministic gate checks
```

**Design signals**:

- later work depends on earlier work being completed or validated
- phase transitions have explicit requirements or gates
- skipping or reordering stages changes correctness

A numbered list alone is not Pipeline. Do not expand Pipeline to mean "anything with multiple steps".

---

## Dominant Pattern Test

Use this test before assigning a dominant pattern:

1. Temporarily ignore inter-Skill / inter-agent coordination.
2. Inspect the Skill's major successful execution paths.
3. Choose a dominant pattern only when one canonical pattern is the stable behavior skeleton across those paths and directly supports the core outcome.
4. Use `none` when the Skill is primarily a meta-controller or policy selector whose major paths legitimately use different canonical patterns and no one pattern remains invariantly dominant.
5. Also use `none` when removing Coordination leaves no independent user-facing local behavior.

If a dominant pattern exists, secondary canonical patterns may be listed when they describe genuine supporting behavior.

---

## Pattern Composition

Composition is expressed as dominant + secondary. `Hybrid` is not a pattern name.

| Composition | Example |
| --- | --- |
| Pipeline + Inversion | later gated phases depend on evidence collected first |
| Pipeline + Generator | ordered stages produce one or more structured artifacts |
| Reviewer + Generator | review first, then generate a corrected artifact |
| Tool Wrapper + Pipeline | ordered workflow requires tool-specific correctness inside stages |

Do not let the secondary pattern bloat root `SKILL.md`.

---

## Coordination Is Separate

Router/orchestrator semantics are not Local Behavior Patterns.

Runtime coordination uses a separate model:

```text
Mechanism: none | handoff | delegate
Strategy: open, domain-specific
```

- `handoff`: primary task ownership transfers to another independent executor.
- `delegate`: current Skill retains overall ownership while assigning a bounded subtask to another independent executor.
- frontmatter neighbor routing is Route, not Coordination.
- tools, scripts, references, APIs, parallel file processing, or multiple records do not by themselves imply Coordination.

Read `references/category-patterns.md` for the full coordination contract.

---

## Capability Is Separate

State, memory, persistent audit, reference-heavy organization, deterministic validation scripts, and similar cross-cutting properties are not Local Behavior Patterns.

The Capability taxonomy remains provisional until validated against real repository Skills.

---

## General Design Rules

1. **Single responsibility**: keep one core architectural outcome per Skill.
2. **Route first**: frontmatter description states when to load, not a marketing summary.
3. **Keep root operational**: move long rules, examples, and gotchas into references.
4. **Prefer deterministic helpers**: use scripts for repeatable validation, transformation, or scoring.
5. **Do not railroad the agent**: use strict order only when dependency or risk justifies it.
6. **Capture gotchas**: preserve domain-specific failure modes the base model would otherwise repeat.
7. **Test routing separately from behavior**: trigger accuracy and task-quality improvement are different eval targets.

---

## Provisional Content Guidance by Category

The category vocabulary below is working guidance, not a frozen taxonomy.

### Library / API reference
- Lead with tool-specific gotchas.
- Include auth, rate limits, deprecated parameters, and version-sensitive behavior.
- Do not restate generic API documentation the model already handles correctly.

### Product validation
- Define precise acceptance criteria and observable pass/fail evidence.
- Include real edge cases from prior failures or evals.

### Data retrieval and analysis
- Document source locations, query formats, authentication, pagination, and data-shape gotchas.

### Business workflow automation
- Define trigger and termination conditions, error handling, and recovery boundaries.
- Preserve agent flexibility where exact step order is not semantically required.

### Code scaffolding
- Define template constraints, naming rules, required files, and mandatory sections.

### Code quality review
- Provide a rubric with severity definitions and evidence expectations.

### CI/CD deployment
- Define true dependency order, gates, rollback triggers, and post-deploy verification.

### Runbook / diagnostics
- Organize evidence, hypotheses, investigation, and structured output.
- Do not force tool order unless the order itself carries safety or dependency meaning.

### Infrastructure operations
- Define pre-flight checks, safety gates, rollback, audit, and verification requirements.

### All categories
- Apply `references/anthropic-content-quality.md` before finalizing Skill content.
