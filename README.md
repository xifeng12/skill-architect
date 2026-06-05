# Skill Architect

Architectural pattern selector and file-structure advisor for [Agent Skills](https://github.com/anthropics/skills). Helps you choose the right design pattern (Tool Wrapper, Generator, Reviewer, Inversion, Pipeline, or Stateful/Memory) and lay out the corresponding `SKILL.md`, `references/`, and `assets/` tree.

## When to use

Trigger this skill when the user asks:

- "design a skill", "skill architecture", "skill pattern", "skill structure"
- "which pattern should I use", "how to organize the skill files"
- "create a skill template", "skill layout"
- "设计 skill", "skill 架构", "skill 模式"

For the **full skill creation lifecycle** (eval, iterate, package, retire), use [`skill-forge`](https://github.com/xifeng12/skill-forge) instead.

## Quick start

1. Classify the request into one of 9 scene categories (see SKILL.md → Step 0)
2. Pick a design pattern from the 6-row table
3. Gather the information listed in Step 2 for that pattern
4. Generate the directory structure from `assets/skill-template.md`

For content quality rules (description writing, gotchas, state files), always apply [`references/anthropic-content-quality.md`](references/anthropic-content-quality.md).

## Repo layout

```
skill-architect/
├── SKILL.md
├── README.md
├── LICENSE
├── references/
│   ├── design-patterns.md         # pattern deep-dive + content strategy by category
│   └── anthropic-content-quality.md
└── assets/
    └── skill-template.md
```

## License

MIT — see [LICENSE](LICENSE).
