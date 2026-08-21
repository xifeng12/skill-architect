# Content Quality Guidance — Provenance Aware

> Compatibility note: this file keeps its historical path `anthropic-content-quality.md`, but **not every rule in this file is an Anthropic-defined contract**. The sections below separate article-supported guidance from project synthesis and runtime evidence.

## ARTICLE-SUPPORTED

### 1. Description is a discovery/routing surface

Agent Skill metadata is used to help the model decide whether a Skill is relevant.

Architecture consequence:

- describe **when** the Skill should load;
- use intent/situation language rather than a feature catalogue;
- include close exclusions when they materially reduce route collision.

Literal user phrases are useful examples, but do not reduce the route contract to keyword matching.

### 2. Progressive disclosure

Keep the always-visible/initial Skill surface compact. Put longer situational detail in referenced files that are loaded only when needed.

Architecture consequence:

- root `SKILL.md` contains routing, core behavior, critical constraints, stop conditions, and reference routing;
- long examples, detailed tables, historical evidence, and deep API notes usually belong in `references/`;
- do not instruct the agent to read every reference on every invocation.

### 3. Evaluate actual agent behavior

Judge a Skill by what changes in agent behavior and trajectories, not by how complete the document looks.

Architecture consequence:

- test realistic positive, negative, and ambiguous routes;
- inspect tool/provider selection and unnecessary actions when relevant;
- distinguish "the domain gotcha is real" from "putting this rule in the Skill improves the model".

## PROJECT-SYNTHESIS

These are project design conclusions derived from article discussion and engineering experience. Do not attribute them to Anthropic as named rules.

### 4. Keep only behavior-changing content

Before keeping an instruction, ask:

```text
If this line is removed, is the agent materially more likely to choose the wrong route, action, evidence level, or stop condition?
```

If not, prefer deleting or moving it out of root.

### 5. Gotchas should be evidence-driven

A useful gotcha is a non-obvious failure mode that changes behavior.

Recommended capture:

```text
Scenario:
Observed/default failure:
Correct behavior:
Evidence:
Skill rule intended to change it:
Validation status:
```

Do not require an arbitrary number of gotchas. If no meaningful gotcha has been observed, record missing evidence instead of inventing examples.

Do not say "gotchas never delete" as a universal rule. A gotcha may be retired, scoped, or removed when evidence changes; preserve history in version control when useful.

### 6. Config is optional, not a default Skill artifact

Do not add `config.json` just because a Skill could have preferences.

Add configuration only when:

- runtime/user/project-specific values are genuinely required;
- the target runtime has a clear location/ownership model for that configuration;
- missing configuration has defined behavior.

Do not put secrets in a repository-owned config template unless the target system's security model explicitly supports it.

### 7. State/Memory is a requirement, not a Pattern or default file set

If cross-run state is needed, first define:

- what must persist;
- why it must persist;
- scope and lifetime;
- ownership and update rules;
- privacy/audit implications.

Do **not** assume generic files such as `.skill-log.json`, `.skill-state.json`, or `STATE.md` are required or portable.

No cross-session state needed → no state artifact required.

### 8. Hooks are optional mechanisms

Do not add a hook merely because a rule is important.

A hook is justified only when:

- the target runtime actually supports the relevant hook mechanism;
- behavior must run deterministically at a specific event/boundary;
- a normal Skill instruction is insufficient or unreliable for that requirement;
- the hook's blast radius and failure behavior are understood.

Do not generalize runtime-specific hooks into universal Agent Skill architecture.

### 9. Do not force Pattern or file-tree completeness

- Pattern may be `none`.
- Local behaviors may be `none`.
- A one-file Skill can be complete.
- `references/`, `assets/`, `scripts/`, state files, hooks, and config are all optional until justified.

## RUNTIME-EVIDENCE

The project's `external-knowledge v0.2.1` validation supports additional runtime-aware quality checks:

- semantic routing can succeed without literal trigger words;
- operational status and semantic coverage should be separate;
- `UNKNOWN` should not be silently rewritten as `MISSING`;
- degraded fallback may be valid when disclosed;
- missing/unverified capability does not authorize environment mutation;
- later phases are not mandatory merely because they exist;
- evidence sufficiency should stop provider fan-out;
- tool identity alone does not prove failure-domain independence.

See `runtime-evidence-external-knowledge-v0.2.1.md` for scope and limitations.

## Final Quality Check

Before finalizing an architecture, ask:

- Is the route contract semantic rather than keyword-only?
- Does root contain only high-value behavior-changing guidance?
- Are long details progressively disclosed?
- Are Pattern/local behavior/file choices optional and justified?
- Are source/vendor claims separated from project synthesis and runtime evidence?
- Are unknowns kept as unknowns?
- Are fallback, phase transitions, environment mutations, and extra calls gated by concrete reasons?
- Is there a STOP condition for both architecture discovery and the runtime workflow when relevant?
