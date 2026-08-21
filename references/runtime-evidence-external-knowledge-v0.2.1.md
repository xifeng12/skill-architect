# Runtime Evidence — external-knowledge v0.2.1

This document records project-level runtime evidence from the standalone `external-knowledge` experiment. It is **not** an upstream vendor contract and must not be treated as a universal law.

The experiment was intentionally downstream of the Agent Skill article synthesis and upstream of changes to `skill-architect`.

## Evidence role

Use these labels:

- `OBSERVED_IN_RUNTIME`: directly observed in at least one executed validation case.
- `REPEATED_ACROSS_CASES`: the same structural behavior was observed in more than one relevant case/round.
- `SCOPE_LIMITED`: directly observed, but the conclusion must remain tied to the tested runtime/scenario.
- `UNRESOLVED`: the experiment did not establish a conclusion.

These labels describe this project's validation strength. They are not an official Agent Skill evidence taxonomy.

Do not promote a project observation into an official Pattern definition.

## V2 evidence summary

### E1 — Semantic routing does not require literal trigger words

Status: `OBSERVED_IN_RUNTIME`

In V2-4, a remembered Chinese long-form article query inferred the WeChat ecosystem as a high-fit source even though the user did not say `WeChat`, `微信`, or `公众号`.

Architecture implication:

- route descriptions should describe **user intent and information need**, not merely keyword lists;
- literal trigger examples are examples, not the routing contract itself.

What this does **not** prove:

- the underlying retrieval backend performs vector/semantic search;
- every Chinese-language query should route to WeChat;
- every model/runtime will reproduce the same semantic route without further evals.

### E2 — Operational status and semantic coverage are separate axes

Status: `OBSERVED_IN_RUNTIME`

V2-4 required two independent judgments:

1. `operational_status`: whether the specialist was actually established as viable in the current runtime (`UNKNOWN` in the case);
2. `coverage_grade`: whether a path semantically covered the information need equivalently or only in degraded form.

The specialist remained `UNKNOWN` while an available general fallback was correctly classified `DEGRADED`.

Architecture implication:

- do not collapse "can I call it?" and "does it cover the same semantic source?" into one status;
- do not use engineering fragility, anti-bot behavior, HTML shape, or provider implementation details as the semantic reason for `DEGRADED` coverage.

### E3 — UNKNOWN is not MISSING

Status: `OBSERVED_IN_RUNTIME`

V2-4 had absence observations but no authoritative inventory scope. That evidence did not justify `MISSING` or a confirmed dependency gap.

Architecture implication:

- `UNKNOWN` must remain a legitimate state;
- absence from one view, PATH probe, manifest, launcher, or session is not automatically authoritative absence;
- installation recommendations require stronger evidence than "not observed here".

### E4 — A degraded fallback may be selected without first proving a confirmed gap

Status: `SCOPE_LIMITED`

In V2-4, the high-fit specialist was not viable because its operational status remained `UNKNOWN`. The only available general-web path was used with explicit coverage disclosure and still produced useful discovery candidates.

Architecture implication:

A fallback can be valid when all are true:

- the preferred path is not currently viable;
- the fallback is actually available;
- the fallback can still make progress on the user's information need;
- semantic coverage loss is disclosed when material;
- no claim is made that the specialist is confirmed missing.

This is distinct from "fallback for completeness".

What this does **not** prove:

- every degraded fallback is acceptable;
- a degraded path can satisfy a source-specific requirement that explicitly demands the unavailable specialist.

### E5 — Missing or unverified capability does not imply environment repair

Status: `REPEATED_ACROSS_CASES`

Across the V2 validation sequence, unavailable, blocked, or unverified capabilities did not authorize installing providers, modifying MCP, changing PATH, repairing TLS, or otherwise mutating the runtime.

Architecture implication:

- capability discovery and environment mutation are different phases;
- a design should not silently cross from retrieval/validation into installation or repair.

### E6 — Discovery and reading can be separate stopping points

Status: `OBSERVED_IN_RUNTIME`

In V2-4, useful article candidates were enough to complete the discovery goal. Full-body reading was not required because the user goal was "find it", not synthesis or content verification.

Architecture implication:

- do not force downstream phases merely because they exist;
- progressive workflows need goal-dependent stop points.

### E7 — Evidence STOP reduced provider fan-out

Status: `REPEATED_ACROSS_CASES`

Across the validation sequence, tested flows stopped once the user-facing information need, capability result, or limitation disclosure was sufficient for the case. Additional providers were not called merely "for completeness".

Architecture implication:

Before another retrieval or phase transition, require a concrete remaining unknown that the action can reduce and that could change the answer, risk judgment, or next action.

### E8 — Tool identity does not prove failure-domain independence

Status: `SCOPE_LIMITED`

V2-3 showed that multiple call shapes could be intercepted by the same control plane, and V2-4 again observed that a superficially similar Bash/curl shape could be blocked before execution while an already-known independent network channel remained usable.

Architecture implication:

- fallback independence should be reasoned about from observed failure domain/control plane, not only tool names;
- once two paths are shown to share a failure domain, do not keep enumerating sibling paths from that same domain;
- use a known independent path only when it serves the current goal.

What this does **not** prove:

- hook behavior is universally non-deterministic;
- a specific shell, provider, or network stack is always in the same failure domain;
- two tools that failed once necessarily share a permanent failure domain.

## Pattern implications

The standalone Skill completed useful runtime validation without requiring the experiment to assign it a named Pattern first.

Status: `OBSERVED_IN_RUNTIME`

Architecture implication:

- do not force every Skill into a named Pattern before it can be designed or tested;
- keep the five documented Pattern names (`Tool Wrapper`, `Generator`, `Reviewer`, `Inversion`, `Pipeline`) distinct from local behavior descriptors;
- `Stateful/Memory`, `Reference-heavy`, coordination style, runtime capability handling, and similar concerns may influence architecture without becoming new Pattern names;
- `Pattern = none` is a valid architecture outcome when no named Pattern materially explains the Skill's core reusable behavior.

What this does **not** prove:

- the external-knowledge Skill can never be usefully described as a composition of one or more named Patterns;
- Pattern analysis has no value;
- the five named Patterns are an exhaustive ontology of all possible Agent Skill behavior.

## Validation discipline

The V2 sequence reached a stopping condition after V2-1 onboarding/correction, V2-2 ordinary-query behavior, V2-3 shared-failure-domain behavior, and V2-4 specialist-unverified/degraded-fallback behavior.

At that point, additional dedicated cases were not expected to change the architecture decision.

Do not continue adding cases solely to make every capability/status cell green.

Future evidence should be added only when it can:

- falsify or refine one of the conclusions above;
- expose a new structural failure mode;
- or materially change a design recommendation.
