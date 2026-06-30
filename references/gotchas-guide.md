# Gotchas Guide

A gotcha is not a generic warning.

Good gotchas describe:

- the wrong default behavior
- the condition that triggers the mistake
- the correct behavior
- the consequence of getting it wrong

## Template

```markdown
- When [condition], the agent tends to [wrong behavior].
  Correct behavior: [right behavior].
  Why it matters: [risk or failure].
```

## Bad

```markdown
- Be careful with database changes.
```

## Better

```markdown
- When a database table is framework-managed, the agent tends to update rows directly and assume the UI will reflect the change.
  Correct behavior: identify the official save, publish, refresh, or cache-invalidation path before proposing direct writes.
  Why it matters: direct writes can bypass validation, hooks, cache invalidation, versioning, or audit behavior.
```

## Production-Oriented Gotcha Types

Capture gotchas around:

- hidden state
- cache invalidation
- generated files
- framework-managed data
- idempotency
- rollback assumptions
- dry-run gaps
- config precedence
- version mismatch
- permission boundary
- environment-specific behavior
- partial failure
- stale documentation
- misleading UI state

## Gotcha Quality Test

A gotcha is useful if removing it would make the agent more likely to perform a wrong action.

If removing it would not change behavior, move it to examples or delete it.
