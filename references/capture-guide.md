# Information Capture Guide

Collect only the information needed to make the architecture decision and support the chosen route contract.

Do **not** collect fields merely because a template or Pattern exists.

## Universal Capture

Ask or infer:

- What user intent should load this Skill?
- What should not load it?
- What neighboring Skills could conflict?
- What reusable behavior is missing without this Skill?
- What evidence shows the behavior repeats or is worth loading conditionally?
- What content must be visible in root?
- What content can be deferred to references?
- What deterministic behavior, if any, actually belongs in scripts?
- What should make the workflow stop?

## Named Pattern Capture

First decide whether a named Pattern materially fits. `Pattern = none` is valid.

If one applies, collect only its relevant information:

| Pattern | Useful capture |
| --- | --- |
| Tool Wrapper | tool semantic role, supported scope, auth/config boundary, critical parameters, common failures, unavailable-capability behavior |
| Generator | output type, required information, schema/template constraints, valid/invalid examples |
| Reviewer | review target, criteria, evidence requirements, pass/fail/severity semantics when actually needed |
| Pipeline | real phases, entry/exit gates, stop conditions, recovery/rollback only where relevant |
| Inversion | genuinely blocking unknowns, minimum sufficient facts, how to avoid re-asking already-known information |

Do not capture `Stateful/Memory`, `Reference-heavy`, or `Hybrid` as additional Patterns.

## Optional Local Behavior Capture

Collect these only when they matter.

### State/history/audit

- What must persist?
- Across which scope/runs?
- Why is persistence required?
- What retention/audit boundary applies?

Do not invent state files before answering those questions.

### Reference-heavy content strategy

- Which details are too long/situational for root?
- What signal should cause each reference to load?
- Can the root remain sufficient without reading every reference?

### Coordination

Describe concrete behavior, not taxonomy labels:

- Are there genuinely independent tracks?
- Is parallel work useful or would it duplicate effort?
- What result must be synthesized?
- What is the stop condition?

Do not force coordination into a closed enum.

### Runtime capability handling

When tools/providers/plugins/MCP matter, capture separately:

- operational status evidence;
- semantic coverage requirement;
- whether fallback is equivalent or degraded;
- what authoritative evidence, if any, establishes missing capability;
- whether environment mutation is actually authorized;
- known failure-domain relationships when relevant.

Keep `UNKNOWN` if evidence does not support a stronger state.

## Gotchas and Failure Modes

Do not require an arbitrary minimum count.

Capture a gotcha when there is evidence that:

- the agent repeatedly makes the mistake;
- the domain/tool has a non-obvious failure mode;
- or an eval/runtime trace exposes a structural risk.

A useful prompt:

```text
What does the agent tend to do wrong in this domain without guidance, and what evidence do we have?
```

If no meaningful gotcha is known yet, record that as missing evidence rather than inventing two examples to complete a template.

## Stop Condition

Stop architecture discovery when enough information exists to choose the container, route contract, Pattern/none decision, local behaviors, and validation strategy.

Do not keep interviewing merely because more fields could be collected.

If a genuinely blocking architecture fact remains unresolved, provide:

- candidate routes/containers;
- the exact unknown;
- why it changes the design;
- the smallest next evidence needed.
