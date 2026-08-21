# Runtime Evidence — external-knowledge v0.2.1

This document records project-level runtime evidence from the standalone `external-knowledge` experiment. It is **not** an upstream vendor contract and must not be treated as a universal law.

The experiment was intentionally downstream of the Agent Skill article synthesis and upstream of changes to `skill-architect`.

## Evidence role

Use these labels:

- `SUPPORTED_BY_RUNTIME`: observed repeatedly in the experiment.
- `SUPPORTED_WITH_SCOPE`: observed, but only for the tested scope/runtime.
- `UNRESOLVED`: the experiment did not establish a general conclusion.

Do not promote a project observation into an official Pattern definition.

## V2 evidence summary

### E1 — Semantic routing does not require literal trigger words

Status: `SUPPORTED_BY_RUNTIME`

A remembered Chinese long-form article query inferred the WeChat ecosystem as a high-fit source even though the user did not say `WeChat`, `微信`, or `公众号`.

Architecture implication:

- route descriptions should describe **user intent and information need**, not merely keyword lists;
- literal trigger examples are examples, not the routing contract itself.

What this does **not** prove:

- the underlying retrieval backend performs vector/semantic search;
- every Chinese-language query should route to WeChat.

### E2 — Operational status and semantic coverage are separate axes

Status: `SUPPORTED_BY_RUNTIME`

The final specialist case required two independent judgments:

1. `operational_status`: `AVAILABLE / UNKNOWN / UNAVAILABLE`;
2. `coverage_grade`: whether a path is semantically `EQUIVALENT / DEGRADED` for the information need.

A specialist could remain `UNKNOWN` while an available general fallback was correctly classified `DEGRADED`.

Architecture implication:

- do not collapse "can I call it?" and "does it cover the same semantic source?" into one status;
- do not use engineering fragility, anti-bot behavior, HTML shape, or provider implementation details as the semantic reason for `DEGRADED` coverage.

### E3 — UNKNOWN is not MISSING

Status: `SUPPORTED_BY_RUNTIME`

Absence observations without an authoritative inventory scope did not justify `MISSING` or a confirmed dependency gap.

Architecture implication:

- `UNKNOWN` must remain a legitimate state;
- absence from one view, PATH probe, manifest, launcher, or session is not automatically authoritative absence;
- installation recommendations require stronger evidence than "not observed here".

### E4 — A degraded fallback may be selected without first proving a confirmed gap

Status: `SUPPORTED_WITH_SCOPE`

When the high-fit specialist was not viable because its operational status was `UNKNOWN`, the only available general-web path was used with explicit coverage disclosure.

Architecture implication:

A fallback can be valid when all are true:

- the preferred path is not currently viable;
- the fallback is actually available;
- the fallback can still make progress on the user's information need;
- semantic coverage loss is disclosed when material;
- no claim is made that the specialist is confirmed missing.

This is distinct from "fallback for completeness".

### E5 — Missing or unverified capability does not imply environment repair

Status: `SUPPORTED_BY_RUNTIME`

Across the experiment, an unavailable or unverified capability did not authorize installing providers, modifying MCP, changing PATH, repairing TLS, or otherwise mutating the runtime.

Architecture implication:

- capability discovery and environment mutation are different phases;
- a design should not silently cross from retrieval/validation into installation or repair.

### E6 — Discovery and reading can be separate stopping points

Status: `SUPPORTED_BY_RUNTIME`

For article discovery, useful candidates were enough to stop. Full-body reading was not required unless the user's goal required synthesis or confirmation from the article body.

Architecture implication:

- do not force downstream phases merely because they exist;
- progressive workflows need goal-dependent stop points.

### E7 — Evidence STOP reduced provider fan-out

Status: `SUPPORTED_BY_RUNTIME`

The tested flows stopped once the user-facing information need and limitation disclosure were satisfied. Additional providers were not called "for completeness".

Architecture implication:

Before another retrieval or phase transition, require a concrete remaining unknown that the action can reduce and that could change the answer, risk judgment, or next action.

### E8 — Tool identity does not prove failure-domain independence

Status: `SUPPORTED_WITH_SCOPE`

The runtime showed control-plane hooks could block different call shapes and could behave non-deterministically across otherwise similar commands. A different command or provider name was therefore not sufficient evidence of an independent failure domain.

Architecture implication:

- fallback independence should be reasoned about from observed failure domain/control plane, not only tool names;
- once two paths are shown to share a failure domain, do not keep enumerating sibling paths from that same domain;
- use a known independent path only when it serves the current goal.

What this does **not** prove:

- hook behavior is universally non-deterministic;
- a specific shell, provider, or network stack is always in the same failure domain.

## Pattern implications

The experiment was useful without proving that the Skill must be assigned a named Pattern.

Therefore:

- do not force every Skill into a named Pattern;
- keep the five documented Pattern names (`Tool Wrapper`, `Generator`, `Reviewer`, `Inversion`, `Pipeline`) distinct from local behavior descriptors;
- `Stateful/Memory`, `Reference-heavy`, coordination style, runtime capability handling, and similar concerns may influence architecture without becoming new Pattern names;
- `Pattern = none` is a valid outcome when no named Pattern materially explains the Skill's core reusable behavior.

## Validation discipline

The four-round experiment reached a stopping condition because new rounds were no longer expected to change the architecture decision.

Do not continue adding cases solely to make every capability/status cell green.

Future evidence should be added only when it can:

- falsify or refine one of the conclusions above;
- expose a new structural failure mode;
- or materially change a design recommendation.
