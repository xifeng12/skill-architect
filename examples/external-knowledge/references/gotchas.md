# External Knowledge Gotchas

These are architecture-level failure modes the example is intended to test. Keep them specific: each should describe a plausible wrong default behavior and the expected correction.

## G-01 — Simple lookup becomes deep research

- **Scenario:** User asks a low-risk current fact such as the current OpenCode Go plan.
- **Wrong default:** Search multiple providers, cross-check community sources, or invoke challengers merely for confidence.
- **Correct behavior:** Prefer the authoritative current source; if it resolves the question, STOP.
- **Architecture control:** `SKILL.md` Evidence Depth + STOP Rule.
- **Status:** OPEN until runtime evidence confirms behavior.

## G-02 — Tool-name routing instead of source-semantic routing

- **Scenario:** User asks for a remembered Chinese long-form article but does not say “微信” or name a provider.
- **Wrong default:** Use only generic Web because no explicit provider keyword appeared.
- **Correct behavior:** Infer whether WeChat is a high-fit source from the information need and use WeChat discovery when appropriate.
- **Architecture control:** WeChat Semantic Routing + `references/source-semantics.md`.
- **Status:** OPEN until runtime evidence confirms behavior.

## G-03 — Chinese query causes unconditional WeChat search

- **Scenario:** User asks a Chinese-language question that is best answered by official docs or ordinary Web.
- **Wrong default:** Search WeChat because the query language is Chinese.
- **Correct behavior:** Language alone is not source semantics; choose the owner from the claim type.
- **Architecture control:** WeChat Semantic Routing boundary.
- **Status:** OPEN until runtime evidence confirms behavior.

## G-04 — Specialist absence becomes environment modification

- **Scenario:** An optional provider such as Firecrawl is unavailable in the current runtime.
- **Wrong default:** Install, configure, repair, or replace the provider during an information lookup.
- **Correct behavior:** Use a viable fallback or state the limitation; environment changes require explicit authorization.
- **Architecture control:** Runtime Capability Boundary.
- **Status:** OPEN until runtime evidence confirms behavior.

## G-05 — General Web steals specialist-owned claims

- **Scenario:** User asks for a version-specific SDK semantic or PR/issue history.
- **Wrong default:** Start with generic Web search even though Context7 or GitHub-native tooling clearly owns the claim.
- **Correct behavior:** Route directly to the specialist owner.
- **Architecture control:** Source Ownership Fast Path.
- **Status:** OPEN until runtime evidence confirms behavior.

## G-06 — Challenger invoked without observed gap

- **Scenario:** General Web already returns sufficient authoritative evidence.
- **Wrong default:** Invoke Exa/Firecrawl anyway to maximize coverage.
- **Correct behavior:** Challenger requires a named precision/extraction gap.
- **Architecture control:** Gap-Based Escalation + STOP Rule.
- **Status:** OPEN until runtime evidence confirms behavior.

## G-07 — Discovery automatically expands into reading every article

- **Scenario:** User asks for a shortlist of relevant WeChat articles.
- **Wrong default:** Resolve and read all candidate article bodies.
- **Correct behavior:** Stop after discovery when titles/links satisfy the request; read only when content is needed.
- **Architecture control:** WeChat Semantic Routing.
- **Status:** OPEN until runtime evidence confirms behavior.
