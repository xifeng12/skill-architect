# Category and Pattern Mapping

Category determines content strategy.

Pattern determines file structure.

They are related but not identical.

## Category Classification

| Category | Content focus | Natural pattern fit |
| --- | --- | --- |
| Library / API reference | Correct usage, API calls, known gotchas | Tool Wrapper |
| Product validation | Acceptance criteria, edge cases, review rubric | Reviewer / Pipeline |
| Data retrieval and analysis | Data sources, query format, auth, output contract | Tool Wrapper / Pipeline |
| Business workflow automation | Trigger conditions, state transitions, error handling | Pipeline |
| Code scaffolding | Templates, naming rules, generated files | Generator |
| Code quality review | Checklist, severity levels, examples | Reviewer |
| CI/CD deployment | Gate order, rollback, verification | Pipeline |
| Runbook / diagnostics | Symptoms, evidence, hypotheses, report structure | Pipeline + Inversion |
| Infrastructure operations | Procedure, safety gates, rollback strategy | Pipeline + Tool Wrapper |
| Skill governance | routing, evals, packaging, retirement | Pipeline / Reviewer |
| Knowledge capture | failure modes, lessons learned, examples | Reference-heavy Skill |

If the request spans categories, select one dominant category and record secondary categories for `references/` structure.

## Pattern Selection

| Pattern | Use when | Common files |
| --- | --- | --- |
| Tool Wrapper | The Skill teaches correct use of an external tool, CLI, API, SDK, MCP, or library | `SKILL.md`, `references/api.md`, optional `scripts/` |
| Generator | The Skill produces structured content, code, config, docs, prompts, or templates | `SKILL.md`, `assets/template.*`, `references/schema.md` |
| Reviewer | The Skill judges, audits, scores, verifies, or critiques something | `SKILL.md`, `references/checklist.md`, optional `scripts/score.*` |
| Pipeline | The Skill coordinates multiple phases, gates, or artifacts | `SKILL.md`, `references/workflow.md`, optional `scripts/validate.*` |
| Inversion | The Skill must gather information before advice or action | `SKILL.md`, `references/question-bank.md` |
| Stateful / Memory | The Skill needs history, config, audit log, or cross-run continuity | `SKILL.md`, `STATE.md`, `.skill-state.json`, `.skill-log.json` |
| Reference-heavy | The Skill mainly routes to compact, situation-specific references | `SKILL.md`, `references/*.md` |
| Hybrid | The task genuinely combines two patterns | Keep one dominant pattern; make secondary pattern explicit |

## Hybrid Rule

A hybrid Skill should still have one dominant pattern.

Examples:

- Pipeline + Inversion: diagnostics workflow that must gather evidence before proposing fixes
- Pipeline + Tool Wrapper: deployment workflow that must use CLI commands safely
- Reviewer + Generator: review first, then generate a corrected artifact
- Reference-heavy + Pipeline: operational runbook with branch-specific references

Avoid allowing the secondary pattern to bloat root `SKILL.md`.
