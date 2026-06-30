# Skill Architecture Anti-Patterns

Avoid these design problems.

| Mistake | Why it is bad | Fix |
| --- | --- | --- |
| Description explains what the Skill does | Weak routing | Rewrite as "Use when..." |
| Skill starts from pattern choice | May design the wrong Skill | Define route contract first |
| Root `SKILL.md` becomes a manual | Context bloat | Move details to references |
| Every workflow becomes a Skill | Skill sprawl | Apply Skill / non-Skill gate |
| One Skill covers too many intents | False positives | Split by route contract |
| Too many tiny Skills | Route fragmentation | Merge if boundaries are weak |
| No neighbor boundary | Route collision | Add do-not-use and neighbor notes |
| No gotchas | Skill adds little value | Capture model failure modes |
| Step-by-step overconstraint | Brittle behavior | Use goals, constraints, gates, and signals |
| Scriptable logic stays in prose | Inconsistent execution | Move deterministic logic to scripts |
| Examples override rules | Overfitting | Mark examples as examples, not universal law |
| Reference routing says "read all references" | Context overload | Load references conditionally |
| Skill duplicates an existing Skill | Maintenance cost | Merge or clarify route boundary |
| Skill relies on non-existent neighbor names | Dead route | Align with actual local Skill names |
| Skill description lacks exclusions | False positives | Add "Do not use when..." for close neighbors |

## Review Questions

When reviewing a Skill, ask:

- Is the description a route trigger?
- Are negative routes explicit?
- Does root `SKILL.md` stay small?
- Are references loaded conditionally?
- Is the dominant pattern clear?
- Are neighboring Skills named accurately?
- Are there trigger tests?
- Are gotchas specific enough to change behavior?
- Does the Skill claim production readiness without evals?
