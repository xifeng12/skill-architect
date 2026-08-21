# Architecture Recommendation Output Format

Use this format when producing a full Skill architecture recommendation.

## Recommendation

Use: [Skill / AGENTS.md / reference / script / prompt / existing Skill]

Reason:

- ...

## Route Contract

Load when:

- ...

Do not load when:

- ...

Likely user phrases:

- ...

Neighbor boundary:

- `skill-a`: ...
- `skill-b`: ...

Remember: neighbor routing is part of Route. Do not label a `Do not load; use skill-b` boundary as runtime handoff unless the current Skill actually transfers task ownership after loading.

## Skill / Non-Skill Decision

Decision: ...

Why:

- ...

Rejected containers:

- `AGENTS.md`: ...
- prompt: ...
- script: ...
- reference file: ...

## Category — Provisional

Dominant category: ...

Secondary categories:

- ...

Why:

- ...

Category taxonomy is not frozen. Do not invent a new Local Behavior Pattern to compensate for a category mismatch.

## Local Behavior

Dominant pattern: [Tool Wrapper / Generator / Reviewer / Inversion / Pipeline / none]

Secondary canonical patterns:

- ...

Why this dominant pattern is stable across the major local execution paths:

- ...

If `none`, state why no canonical pattern remains invariantly dominant after runtime coordination is ignored:

- ...

Do not use Stateful / Memory, Reference-heavy, Hybrid, Router, Orchestrator, or strategy names as pattern values.

## Coordination

Mechanism: [none / handoff / delegate]

Ownership rule:

- If `handoff`: what independent executor receives primary task ownership?
- If `delegate`: what bounded subtask is assigned, and how does this Skill retain overall ownership?
- If `none`: confirm that tool/script/API/reference use is not being mistaken for Coordination.

Strategy name, if useful: ...

Strategy descriptors:

- selection policy: ...
- cardinality: ...
- scheduling: ...
- aggregation: ...
- termination: ...

Coordination Strategy is open vocabulary. Keep domain-specific names here rather than promoting them into the closed Mechanism enum.

## Capability — Provisional

Cross-cutting capability candidates:

- state / memory: ...
- persistent audit: ...
- reference-heavy organization: ...
- deterministic validation scripts: ...
- other: ...

Capability taxonomy is not frozen yet. These properties must not be promoted into the Local Behavior Pattern namespace.

## Suggested Structure

```text
skill-name/
├── SKILL.md
├── references/
│   └── ...
├── assets/
│   └── ...
└── scripts/
    └── ...
```

## Root SKILL.md Plan

Keep in root:

- ...

Move to `references/`:

- ...

Move to `assets/`:

- ...

Move to `scripts/`:

- ...

## Reference Split Plan

- `references/...`: ...
- `references/...`: ...
- `assets/...`: ...
- `scripts/...`: ...

## Gotchas to Capture

- When ..., the agent tends to ...
  Correct behavior: ...
  Why it matters: ...

- When ..., the agent tends to ...
  Correct behavior: ...
  Why it matters: ...

## Validation

Should load:

- ...
- ...

Should not load:

- ...
- ...

Ambiguous:

- phrase: "..."
  expected route: ...
  reason: ...

Architecture regression checks:

- Does Local Behavior use only the canonical five pattern names or justified `none`?
- Is numbered-step structure being mistaken for Pipeline without real dependency or gates?
- Is neighbor routing being mistaken for runtime handoff?
- Is tool/script/API usage being mistaken for Coordination?
- If delegation exists, is ownership retention explicit?
- Are coordination strategy names kept out of the Mechanism enum?

## Risks

- ...

## Missing Evidence

- ...
