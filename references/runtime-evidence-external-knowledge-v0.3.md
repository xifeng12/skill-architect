# Runtime Evidence — external-knowledge v0.3

This document records project-level runtime evidence from the standalone `external-knowledge` experiment at v0.3-beta.1. It complements `runtime-evidence-external-knowledge-v0.2.1.md`; the v0.2.1 record is not rewritten or replaced.

It is **not** an upstream vendor contract and must not be treated as a universal law.

## Evidence basis

- Package: `external-knowledge` v0.3-beta.1 (frozen, `STATUS = COMPLETE` for real-work validation), 2026-08-22.
- Evidence chain: real runtime experiment → runtime evidence → external-knowledge repository → generalizable architecture conclusions → this skill.
- Validation: 25 unit tests + `py_compile` + Doctor smoke, non-mutating scan showed no subprocess/network mutation by Doctor or planner.
- Primary real case: `WECHAT-DISCOVERY-REAL-1` (find WeChat article《ArcPy使用之一：SDE数据定时备份》, publisher 图说新语).

## Evidence labels

- `OBSERVED_IN_RUNTIME`: directly observed in at least one executed validation case.
- `REPEATED_ACROSS_CASES`: the same structural behavior observed in more than one relevant case/round.
- `SCOPE_LIMITED`: directly observed, but the conclusion stays tied to the tested runtime/scenario.
- `UNRESOLVED`: the experiment did not establish a conclusion.
- `UNIT_VALIDATED` / `DESIGN_VALIDATED`: design or unit-level validation only; distinct from `REPEATED_ACROSS_REAL_CASES`.

These labels describe this project's validation strength; they are not an official taxonomy.

## E1 — Provider and Capability are separate axes

Status: `OBSERVED_IN_RUNTIME`

The WeChat case goal used source semantic `wechat.article-discovery` and capability `wechat.discovery` in the v0.3-beta.1 adapter. That capability existed independently of any single provider's inventory status. Earlier discussion sometimes used the conceptual label `article-discovery.wechat`; the current adapter Source of Truth is `wechat.discovery`. `complex-web.read` is a separate extraction/read capability and is not the capability ID for this discovery case.

Architecture implication:

- `Capability ≠ Provider`. One capability can have several providers.
- `Provider missing ≠ Capability missing` — provider inventory absence must not be collapsed into capability absence.
- Do not turn Provider, Exposure, Discoverability, Capability, or Runtime authority into new Pattern names; keep the five named Patterns.

## E2 — Carrier ≠ Independent Execution Surface

Status: `OBSERVED_IN_RUNTIME`

The WeChat specialist's carrier was its skill directory (`SKILL.md` / skill directory); its real execution surface was `scripts/search_wechat.js` (Node + cheerio, backend `weixin.sogou.com`). The v0.3 adapter contract did not declare that surface at case time.

Architecture implication:

- Carrier (skill directory, plugin, config registration) is not the same as an independent execution surface (runnable entry point).
- Count only real execution surfaces. Do not double-count `plugin → MCP` or `skill → CLI/local_script` as two independent exposures.

## E3 — Static exposure present ≠ provider available

Status: `OBSERVED_IN_RUNTIME`

A declared or discoverable execution surface proves only that the surface exists. `AVAILABLE` requires runtime evidence: runtime exposure plus a representative probe or equivalent direct-callable evidence.

Architecture implication:

- Static presence is a `PRESENT` claim, not an `AVAILABLE` claim.
- Prefer judging provider availability from runtime exposure + representative probe.

## E4 — Scoped authority of absence evidence

Status: `OBSERVED_IN_RUNTIME`

`which("tool") = None` authoritatively proves absence of the tool on the current process PATH exposure; it does not prove the software is absent everywhere on the machine.

Architecture implication:

- Status judgments bind evidence + authority + scope together.
- State the scope alongside the claim; wider claims need wider authority.

## E5 — Source Discoverability is a third axis

Status: `OBSERVED_IN_RUNTIME` (confined to `WeChat article discovery`; `SCOPE_LIMITED`)

For the WeChat source semantic:

- specialist: Operational = `AVAILABLE`, Semantic Coverage = `EQUIVALENT`, Source Discoverability = `LIMITED_OBSERVED`;
- general web fallback: Operational = `AVAILABLE`, Semantic Coverage = `DEGRADED`, Source Discoverability = `VERY_LOW_OBSERVED`.

Observed repeatedly: target article zero hits, `site:mp.weixin.qq.com` could not surface the target, materially different queries returned fully/highly identical Top-N, and the user later supplied the article link proving the content existed.

Architecture implication:

- Discoverability is a `Provider × Source Semantic` property, not a provider-global attribute.
- Allowed states: `LIMITED_OBSERVED`, `VERY_LOW_OBSERVED`, `UNKNOWN`. Do not create a strong state like `STRUCTURAL_BLIND_SPOT` without mechanism-level evidence.
- These are runtime/project evidence, not an official universal taxonomy.
- `AVAILABLE` must not be read as "will find the article".

### F-correction — causal calibration

Status: `OBSERVED_IN_RUNTIME`; specialist zero-hit root cause `UNRESOLVED`

Two independent problems coexisted in the WeChat case:

- A. Exposure visibility defect: provider actually runnable, but adapter had not declared the execution surface → provider remained `UNKNOWN` in inventory until direct probing.
- B. Retrieval-quality limitation: after direct specialist invocation, exact-title-like query still returned `total=0`.

Do not collapse the case into "the root cause was only an undeclared exposure contract"; the specialist probe itself returned zero results.

## E6 — Retrieval-quality STOP signal

Status: `OBSERVED_IN_RUNTIME` (local/runtime evidence signal, not a new Pattern)

`LOW_QUERY_DISCRIMINATION`: materially different queries + Top-N highly/fully identical + results semantically irrelevant → stop repeated query rewriting; another query is unlikely to change the answer, risk judgment, or next action.

Ambiguity rule (SCOPE_LIMITED): if context already resolves ambiguity, at most one contextual rewrite; mismatch persists → STOP. Clarify with the user only when context cannot resolve ambiguity.

Architecture implication:

- Retrieval quality is a separate signal from operational status and semantic coverage.
- It is a local signal; do not require it as a universal gate for every retrieval task.

## E7 — Approval-bound execution model

Status: `UNIT_VALIDATED` / `DESIGN_VALIDATED` (not `REPEATED_ACROSS_REAL_CASES`)

Designed and unit-validated flow: `Doctor → Capability Plan → explicit approval boundary → mutation → verification`. In real runtime, `Plan → Approval → Install → Verify` has not yet completed end-to-end because no legitimate provisioning candidate appeared naturally.

Architecture implication:

- Capability discovery and environment mutation remain separate phases with an explicit approval boundary.
- Do not label an approval-bound execution model as `REPEATED_ACROSS_REAL_CASES` when only design/unit validation exists.
- Do not fabricate fixtures solely to complete the provisioning path.

## Architecture implications (summary)

- Provider / Exposure / Discoverability / Capability / Runtime authority are all architecture axes; none is a new Pattern name.
- Provider missing ≠ Capability missing. Exposure PRESENT ≠ Provider AVAILABLE. Operational Status ≠ Semantic Coverage ≠ Discoverability.
- Absence evidence is scoped to its authority; state evidence + authority + scope together.
- Retrieval quality signals can justify STOP without naming a new Pattern.

## What this does NOT prove

- NOT: Bing or general web is globally bad.
- NOT: `AVAILABLE` implies a specific article/object is retrievable.
- NOT: a universal discoverability taxonomy valid for all runtimes.
- NOT: `LOW_QUERY_DISCRIMINATION` is a required gate for every retrieval task.
- NOT: every Skill must build a Capability/Provider/Exposure model; these checks apply only to runtime-dependent Skills.

## STOP condition

Add future evidence only when it can falsify or refine one of the conclusions above, expose a new structural failure mode, or materially change a design recommendation. Do not add cases solely to make every capability/status cell green.
