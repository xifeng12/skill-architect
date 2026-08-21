# Architecture Recommendation Output Format

Use this format when producing a full Skill architecture recommendation.

Do not fill sections mechanically. Omit or mark `none` where the architecture does not need a concept.

## Recommendation

Use: [Skill / AGENTS.md / reference / script / prompt / existing Skill]

Reason:

- ...

## Route Contract

Load when:

- ...

Do not load when:

- ...

Likely user phrases (examples, not the route definition):

- ...

Neighbor boundary:

- `skill-a`: ...
- `skill-b`: ...

## Skill / Non-Skill Decision

Decision: ...

Why:

- ...

Rejected containers when relevant:

- `AGENTS.md`: ...
- prompt: ...
- script: ...
- reference file: ...

## Category

Primary category: ...

Secondary categories (optional):

- ...

Why:

- ...

## Named Pattern

Pattern: [Tool Wrapper / Generator / Reviewer / Inversion / Pipeline / none]

Composition, if two or more named Patterns genuinely apply:

- ...

Why this Pattern helps — or why `none` is better:

- ...

Do not use `Stateful/Memory`, `Reference-heavy`, `Hybrid`, or coordination labels as new Pattern names.

## Local Behaviors

Only include behaviors that materially affect the architecture.

Examples:

- State/history/audit requirement: ... / none
- Reference-heavy content strategy: ... / none
- Coordination notes: free-form description / none
- Runtime capability policy: ... / none

Do not force a closed taxonomy here.

## Runtime Capability Model

Include only when the Skill depends on runtime-specific tools/providers/plugins/MCP.

For each relevant capability distinguish:

- operational status: AVAILABLE / UNKNOWN / UNAVAILABLE (or the project's equivalent evidence-backed states)
- semantic coverage: equivalent / degraded / not applicable
- fallback condition: ...
- coverage disclosure required: yes/no
- environment mutation authorized: yes/no

Do not infer `MISSING` from a non-authoritative absence observation.

## Suggested Structure

```text
skill-name/
├── SKILL.md
├── references/
│   └── ...       # only if needed
├── assets/
│   └── ...       # only if needed
└── scripts/
    └── ...       # only if deterministic behavior justifies them
```

Do not add empty directories merely to match a template.

## Root SKILL.md Plan

Keep in root:

- ...

Move to `references/` only if needed:

- ...

Move to `assets/` only if needed:

- ...

Move to `scripts/` only if needed:

- ...

## Reference Split Plan

- `references/...`: ...
- `assets/...`: ...
- `scripts/...`: ...

Omit unused groups.

## Gotchas / Failure Modes to Capture

Prefer observed or evidence-backed failures.

- When ..., the agent tends to ...
  Correct behavior: ...
  Why it matters: ...
  Evidence: ...

If no meaningful gotcha is yet known, say so rather than inventing one.

## Validation

Should load:

- ...

Should not load:

- ...

Ambiguous:

- phrase: "..."
  expected route: ...
  reason: ...

For runtime-dependent Skills also test when useful:

- specialist available;
- specialist unknown/unverified;
- degraded fallback;
- shared failure domain;
- evidence STOP / no unnecessary fan-out.

Do not test every branch merely for coverage. Add/repeat cases only when they can change an architecture decision.

## Stop Conditions

Architecture-level STOP:

- ...

Runtime/workflow STOP if relevant:

- ...

## Risks

- ...

## Missing Evidence

- ...

Keep `UNKNOWN` as unknown when the evidence does not justify a stronger state.
