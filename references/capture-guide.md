# Information Capture Guide

Collect only the information needed for the selected pattern.

## Capture by Pattern

| Pattern | Required capture |
| --- | --- |
| Tool Wrapper | tool name, supported operations, auth, parameters, common errors, examples, when not to use |
| Generator | output type, required fields, schema, style constraints, examples, invalid examples |
| Reviewer | review object, checklist, severity scale, pass/fail criteria, output format |
| Pipeline | phases, gates, stop conditions, verification, rollback or recovery notes |
| Inversion | unknowns, question order, minimum viable facts, stop/continue condition |
| Stateful / Memory | state trigger, state file, update rules, audit requirement, retention boundaries |
| Reference-heavy | reference map, when to load each reference, gotchas, examples |
| Hybrid | dominant pattern capture plus only the secondary pattern details needed |

## Universal Questions

Ask or infer:

- What user intent should load this Skill?
- What should not load it?
- What neighboring Skills could conflict?
- What does the agent currently do wrong without this Skill?
- What evidence shows this workflow repeats?
- What content must be always visible in root?
- What content can be deferred to references?
- What deterministic checks belong in scripts?

## Minimum Gotcha Requirement

Collect at least two specific gotchas before calling a Skill production-ready.

A useful prompt:

```text
What does the agent tend to do wrong in this domain without guidance?
```

## Stop Condition

If the route contract is unclear, do not recommend a final Skill design.

Instead, provide:

- candidate routes
- competing containers
- missing facts
- recommended next evidence to collect
