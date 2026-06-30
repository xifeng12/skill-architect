# Directory Templates

Choose the smallest structure that supports the route contract.

## Minimal Skill

Use for small, personal, or narrow Skills.

```text
skill-name/
└── SKILL.md
```

## Reference-Backed Skill

Use when root should stay small but the domain needs detailed guidance.

```text
skill-name/
├── SKILL.md
└── references/
    ├── gotchas.md
    ├── examples.md
    └── checklist.md
```

## Generator Skill

```text
skill-name/
├── SKILL.md
├── assets/
│   └── template.md
└── references/
    ├── schema.md
    └── examples.md
```

## Tool Wrapper Skill

```text
skill-name/
├── SKILL.md
├── references/
│   ├── tool-usage.md
│   ├── parameters.md
│   └── errors.md
└── scripts/
    └── validate_input.py
```

## Reviewer Skill

```text
skill-name/
├── SKILL.md
├── references/
│   ├── rubric.md
│   ├── severity.md
│   └── examples.md
└── scripts/
    └── score.py
```

## Pipeline Skill

```text
skill-name/
├── SKILL.md
├── references/
│   ├── workflow.md
│   ├── gates.md
│   └── recovery.md
└── scripts/
    └── validate_state.py
```

## Stateful Skill

```text
skill-name/
├── SKILL.md
├── STATE.md
├── .skill-state.json
├── .skill-log.json
└── references/
    ├── state-rules.md
    └── recovery.md
```

## Split Rule

Keep in root:

- route contract
- safety gates
- minimal workflow
- reference routing
- output contract

Move to references:

- examples
- long explanations
- tables
- anti-patterns
- detailed checklists
- historical lessons

Move to assets:

- templates
- schemas
- sample files
- reusable prompt blocks

Move to scripts:

- deterministic validation
- scoring
- conversion
- packaging
- formatting
