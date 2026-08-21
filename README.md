# Skill Architect

Architecture advisor for [Agent Skills](https://github.com/anthropics/skills). It helps decide whether something should become a Skill, define the route contract, determine whether one of the five named design Patterns is actually useful, and split `SKILL.md`, `references/`, `assets/`, and `scripts/` without over-designing the Skill.

## Pattern naming boundary

This repository reserves **Pattern** for five names:

- Tool Wrapper
- Generator
- Reviewer
- Inversion
- Pipeline

A Skill does **not** have to select one. `Pattern = none` is valid when none of the five materially explains the Skill's core reusable behavior.

`Stateful/Memory`, `Reference-heavy`, coordination style, runtime capability policy, and similar concerns are architecture characteristics or local behaviors, not additional Pattern names.

## When to use

Trigger this skill when the user asks:

- "design a skill", "skill architecture", "skill pattern", "skill structure"
- "which pattern should I use", "does this skill need a pattern"
- "how should I organize SKILL.md and references"
- "should this behavior be a Skill, AGENTS.md rule, script, reference, or prompt"
- "设计 skill", "skill 架构", "skill 模式"

For the **full skill creation lifecycle** (implementation, eval, iterate, package, publish, retire), use [`skill-forge`](https://github.com/xifeng12/skill-forge) instead.

## Architecture order

1. Define the route contract and negative routes.
2. Decide Skill vs non-Skill container.
3. Identify the reusable behavior and scene/category.
4. Ask whether any of the five named Patterns actually helps explain the design; allow `none`.
5. Record optional local behaviors separately from Pattern naming.
6. Define neighbor boundaries.
7. Keep root `SKILL.md` operational; move detail to references/assets/scripts only when justified.
8. Define trigger/content/runtime validation appropriate to the claim.

Do not start from a Pattern label and work backward.

## Runtime-informed architecture

The standalone `external-knowledge` experiment added project-level evidence for several architecture rules:

- semantic routing can work without literal trigger words;
- operational capability status and semantic coverage are separate axes;
- `UNKNOWN` is not `MISSING`;
- a degraded fallback can be valid without proving the preferred specialist is missing;
- missing/unverified capability does not authorize environment mutation;
- discovery and downstream reading/action may have separate stop points;
- evidence sufficiency should stop provider fan-out;
- different tool names do not by themselves prove independent failure domains.

These are project runtime findings, not vendor contracts. See [`references/runtime-evidence-external-knowledge-v0.2.1.md`](references/runtime-evidence-external-knowledge-v0.2.1.md).

## Repo layout

```text
skill-architect/
├── SKILL.md
├── README.md
├── LICENSE
├── references/
│   ├── design-patterns.md
│   ├── category-patterns.md
│   ├── anthropic-content-quality.md
│   ├── anti-patterns.md
│   └── runtime-evidence-external-knowledge-v0.2.1.md
└── assets/
    └── ...
```

## License

MIT — see [LICENSE](LICENSE).
