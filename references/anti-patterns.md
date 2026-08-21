# Skill Architecture Anti-Patterns

Avoid these design problems.

| Mistake | Why it is bad | Fix |
| --- | --- | --- |
| Description explains what the Skill does | Weak routing | Rewrite as an intent-based `Use when...` route contract |
| Routing depends only on literal keywords | Misses semantically equivalent requests | Describe user intent/information need; keep trigger phrases as examples |
| Skill starts from Pattern choice | May design the wrong Skill | Define route contract and reusable behavior first |
| Every Skill must choose a Pattern | Forces taxonomy onto behavior that may not fit | Allow `Pattern = none` |
| `Stateful/Memory`, `Reference-heavy`, or `Hybrid` are promoted to new Pattern names | Mixes orthogonal architecture dimensions with named Patterns | Reserve Pattern for Tool Wrapper / Generator / Reviewer / Inversion / Pipeline; describe other concerns separately |
| Root `SKILL.md` becomes a manual | Context bloat | Move details to references |
| Every workflow becomes a Skill | Skill sprawl | Apply Skill / non-Skill gate |
| One Skill covers too many unrelated intents | False positives | Split when route contracts and reusable behaviors truly differ |
| Too many tiny Skills | Route fragmentation | Merge if boundaries are weak |
| No neighbor boundary | Route collision | Add do-not-use and neighbor notes |
| No evidence that a gotcha changes behavior | Skill may add noise without benefit | Prefer observed failure modes and validate model delta |
| Step-by-step overconstraint | Brittle behavior | Use goals, constraints, gates, signals, and real stop conditions |
| Scriptable logic stays in prose | Inconsistent deterministic execution | Move genuinely deterministic logic to scripts |
| Examples override rules | Overfitting | Mark examples as examples, not universal law |
| Reference routing says `read all references` | Context overload | Load references conditionally |
| Skill duplicates an existing Skill | Maintenance cost | Merge or clarify route boundary |
| Skill relies on non-existent neighbor names | Dead route | Align with actual local Skill names |
| Skill description lacks exclusions | False positives | Add `Do not use when...` for close neighbors |
| Operational availability and semantic coverage are collapsed into one status | A callable fallback may not cover the same source semantics | Track availability separately from equivalent/degraded coverage |
| `UNKNOWN` capability is silently rewritten as `MISSING` | Absence observation becomes an unsupported dependency claim | Require authoritative absence before calling it missing |
| Missing/unverified capability automatically triggers installation or environment repair | Retrieval/validation silently becomes implementation | Separate capability discovery from environment mutation and require authorization |
| Fallback is used `for completeness` | Provider fan-out and duplicated work | Require a named gap, non-viable preferred path, or material disagreement |
| Degraded fallback is hidden | User may overestimate source coverage | Disclose material semantic coverage loss |
| Discovery automatically continues into reading/action | Existence of a later phase becomes false obligation | Give each phase its own goal-dependent STOP |
| Different tool names are assumed to be independent fallbacks | Shared control plane/failure domain can make retries redundant | Infer independence from evidence, not tool identity |
| Validation continues after new rounds cannot change the architecture decision | Over-testing creates noise and side effects | Stop when more evidence will not change conclusion, risk, or next action |

## Review Questions

When reviewing a Skill, ask:

- Is the description a route trigger rather than a feature summary?
- Can the route work from meaning, not only literal trigger words?
- Are negative routes explicit?
- Does root `SKILL.md` stay small?
- Are references loaded conditionally?
- Is a named Pattern actually needed? If yes, is it one of the five reserved names?
- Are local behaviors being mislabeled as Patterns?
- Are neighboring Skills named accurately?
- Are there trigger tests?
- Are gotchas based on observed risk or evidence rather than template completion?
- For runtime-dependent Skills, are operational status and semantic coverage separate?
- Is `UNKNOWN` preserved when authoritative absence is not established?
- Does fallback have a concrete reason and coverage disclosure when degraded?
- Are phase boundaries and STOP conditions explicit?
- Does the Skill claim production readiness without sufficient eval/runtime evidence?
