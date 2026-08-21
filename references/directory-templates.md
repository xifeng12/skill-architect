# Directory Templates

Choose the smallest structure that supports the route contract and verified needs.

Templates are examples, not mandatory file trees. Do not create empty directories merely to match a Pattern or category.

## Minimal Skill

Use when the route contract and core behavior fit in one concise root file.

```text
skill-name/
└── SKILL.md
```

This is a valid final structure, not an incomplete Skill.

## Reference-Backed Structure

Use when root should stay small but detailed guidance is useful only in specific situations.

```text
skill-name/
├── SKILL.md
└── references/
    ├── gotchas.md
    └── ...
```

Reference-heavy is a content strategy, not a named Pattern.

## Generator Pattern — Typical Structure

```text
skill-name/
├── SKILL.md
└── assets/
    └── template.md
```

Add `references/schema.md` or examples only when the generator needs them.

## Tool Wrapper Pattern — Typical Structure

```text
skill-name/
├── SKILL.md
└── references/
    └── tool-usage.md
```

Add `scripts/` only when deterministic wrapping, validation, formatting, or conversion materially improves correctness.

Do not create an installer merely because the wrapped capability might be missing at runtime.

## Reviewer Pattern — Typical Structure

```text
skill-name/
├── SKILL.md
└── references/
    └── rubric.md
```

Add deterministic scoring scripts only when the criteria can actually be encoded reliably.

## Inversion Pattern — Typical Structure

```text
skill-name/
├── SKILL.md
└── references/
    └── questions.md   # optional
```

Do not require a questionnaire file if the missing-information gate is simple enough for root `SKILL.md`.

## Pipeline Pattern — Typical Structure

```text
skill-name/
├── SKILL.md
└── references/
    └── workflow.md    # only when the phase logic is too large for root
```

Add `scripts/validate_state.*` only when deterministic state validation is truly required.

A later pipeline phase is not automatically mandatory. Define goal-dependent STOP conditions.

## Skill with State Requirements

State is a local requirement, not a named Pattern.

Start by asking what must persist and why.

Possible structure:

```text
skill-name/
├── SKILL.md
└── references/
    └── state-rules.md
```

Add persisted artifacts such as state files or logs only when the runtime and workflow actually require cross-run continuity or audit history.

Do **not** assume generic names such as `STATE.md`, `.skill-state.json`, or `.skill-log.json` are universally required.

## Skill with Runtime Capability Dependencies

Do not create a provider registry by default.

Root may only need rules such as:

- discover capability availability at runtime;
- keep `UNKNOWN` distinct from confirmed missing;
- separate operational status from semantic coverage;
- use degraded fallback only with a concrete reason and disclosure;
- do not mutate the environment without authorization.

Create a longer runtime-capability reference only if the provider matrix is genuinely complex.

## Split Rule

Keep in root:

- route contract
- use/do-not-use boundary
- core behavior
- safety/phase gates when necessary
- STOP conditions
- reference routing
- output contract when needed

Move to references only when justified:

- long examples
- long explanations
- detailed tables
- anti-patterns
- detailed checklists
- historical/runtime evidence

Move to assets only when justified:

- reusable templates
- schemas
- sample files used by the workflow

Move to scripts only when justified:

- deterministic validation
- scoring that is truly machine-definable
- conversion
- formatting
- repeatable mechanical operations

The smallest structure that reliably changes agent behavior is preferable to the most complete-looking tree.
