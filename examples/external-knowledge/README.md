# External Knowledge Example

A real Skill used to validate the architecture rules in [`skill-architect`](../../SKILL.md).

This is intentionally an **architecture experiment**, not a production/package claim.

## Architecture Summary

- **Category:** Data retrieval and analysis
- **Dominant pattern:** Pipeline
- **Secondary pattern:** Reference-heavy
- **Primary design question:** Can a broad external-information Skill route by information need without becoming a mega-router or over-searching?

Read [`ARCHITECTURE.md`](ARCHITECTURE.md) for the design decision and falsifiable hypotheses.

## What It Coordinates

The Skill does not implement search backends. It coordinates existing owners:

- official/current Web sources;
- Context7 for version-specific docs;
- GitHub-native connectors for repository semantics;
- WeChat discovery for topic/article discovery;
- WeChat reader for canonical article content;
- general Web search/fetch;
- Exa only after a real precision gap;
- Firecrawl or equivalent only after a real extraction gap and only when available.

## WeChat Semantics

WeChat routing is intentionally semantic at the agent layer.

A prompt such as:

> 我记得前段时间有人写过一篇中文长文，详细比较 OpenCode Go 和其他编码套餐，帮我找一下。

may route to WeChat discovery even without the literal words “微信” or “公众号”.

This does **not** claim that the underlying WeChat search backend is vector/embedding semantic search.

## ZCode First Test

Current user-supplied runtime condition:

```text
Firecrawl = uninstalled / unavailable in ZCode
```

Do not reinstall it for this experiment.

Use [`tests/zcode-v0.1.md`](tests/zcode-v0.1.md). Start with the six first-pass cases and stop if a structural failure already falsifies the architecture.

For the first test, load/copy this directory as a local Skill in ZCode using the mechanism ZCode currently supports. Do not modify the portable `SKILL.md` merely to reflect one runtime's provider inventory.

## Files

```text
external-knowledge/
├── ARCHITECTURE.md
├── README.md
├── SKILL.md
├── references/
│   ├── gotchas.md
│   └── source-semantics.md
└── tests/
    └── zcode-v0.1.md
```

## What Success Means

Success is not “called many tools”.

Success means:

- correct trigger / non-trigger behavior;
- source owner selected from information need;
- specialists retain ownership of specialist claims;
- challengers are used only after an observed gap;
- missing providers do not trigger unauthorized installation;
- retrieval stops when the evidence is sufficient.
