---
title: "Codified Context: Infrastructure for AI Agents in a Complex Codebase"
authors:
  - Aristidis Vasilopoulos
year: 2026
venue: arXiv preprint
url: https://arxiv.org/abs/2602.20478
date_analyzed: 2026-03-14
tags:
  - agentic-coding
  - context-engineering
  - multi-agent-systems
  - developer-tools
  - knowledge-management
  - LLM-infrastructure
---

# Codified Context: Infrastructure for AI Agents in a Complex Codebase

## Summary

This paper presents a three-tier "codified context" infrastructure designed to give LLM-based coding agents persistent, structured project knowledge across sessions. Developed during construction of a 108,000-line C# distributed real-time simulation, the infrastructure comprises a project constitution (hot memory), 19 specialized domain-expert agents, and a 34-document knowledge base (cold memory) served via MCP. Across 283 development sessions the author tracked how this infrastructure grew organically and propagated knowledge, finding that it prevented repeated failures, eliminated re-derivation of past solutions, and enabled collaborative debugging of subtle bugs that stalled unguided sessions. The total context infrastructure reached ~26,200 lines — roughly 24% of the codebase — suggesting that for complex domains, the knowledge *about* code rivals the code itself in importance.

## Key Ideas

- **Three-tier memory architecture.** A single always-loaded constitution (~660 lines) for conventions and routing, 19 specialist agent specifications (~9,300 lines) embedding deep domain knowledge, and 34 retrieval-served knowledge base documents (~16,250 lines) scoped to individual subsystems.
- **Trigger tables for agent routing.** File-pattern-based rules in the constitution automatically route tasks to the appropriate specialist agent, encoding institutional knowledge about which expertise each area of the codebase requires.
- **Agents as domain-priming mechanisms.** Over half of each agent specification is project-domain knowledge rather than behavioral instructions — they function less as workflow coordinators and more as pre-loaded expert context.
- **Knowledge written for AI consumption.** All documents are authored explicitly for machine readers: exact file paths, parameter names, expected behaviors, and symptom-cause-fix tables distilled from debugging sessions.
- **Codified context as infrastructure.** The paper frames project documentation as load-bearing artifacts that agents depend on to produce correct output, analogous to how build systems or CI pipelines are infrastructure.
- **Brevity bias counterpoint.** Challenges the trend toward minimal prompts, showing that specialized agents require substantial embedded domain knowledge (~711 lines average for high-capability agents) to perform reliably.

## Method / Approach

The author built the infrastructure iteratively over 70 part-time days while developing a networked multiplayer game. The study is observational and descriptive, not experimental.

**Infrastructure design:** The constitution is loaded into every session automatically. Specialist agents are invoked via trigger tables or explicit developer request. The knowledge base is served through an MCP server (~1,600 lines Python) providing keyword-based retrieval with five search tools (list subsystems, get files, find relevant context, search documents, suggest agent).

**Data collection:** 283 development sessions were logged, yielding 2,801 human prompts (avg 9.9/session), 1,197 agent invocations, 16,522 autonomous agent turns, and 1,478 MCP retrieval calls across 218 sessions. Sessions were classified as ad-hoc (~87%) or structured plan-execute-review (~13%).

**Analysis:** Quantitative metrics track infrastructure growth across three phases. Four detailed case studies illustrate how codified context propagates across sessions: (1) a save system specification referenced in 74 sessions with zero related bugs over four weeks, (2) UI sync routing patterns preventing re-derivation of delivery solutions, (3) a drop system spec created on-demand when a gap was detected, and (4) deterministic RNG debugging where an agent's embedded domain knowledge resolved bugs that stalled five prior unguided attempts.

## Results / Claims

- Infrastructure grew in three phases: basic constitution (days 1-10), initial specialists for high-failure domains (days 11-30), then MCP retrieval integration and full agent pool (days 31-57).
- 57% of specialist invocations were project-specific domain agents (not generic coding agents).
- The save system spec (283 lines) was referenced in 74 sessions with zero save-related bugs across four weeks and five dependent features.
- The networking agent (~915 lines, ~65% domain knowledge) identified three desynchronization bugs missed in five prior attempts requiring 84 code edits and 5 context window exhaustions.
- Maintenance overhead: ~5 min/session when specs are affected, 30-45 min biweekly review, totaling 1-2 hours/week.
- Primary failure mode: specification staleness — agents trust documentation absolutely, so outdated specs cause silent failures.

## Insights & Takeaways

- **Context-to-code ratio is significant.** At 24% of codebase size, the context infrastructure suggests that for complex projects, maintaining knowledge for AI agents is a substantial engineering investment — but one that compounds over time.
- **Agents need deep knowledge, not just instructions.** The finding that >50% of agent specs are domain knowledge challenges the common pattern of writing thin system prompts. This aligns with experience using CLAUDE.md files in practice.
- **Staleness is the critical risk.** The most dangerous failure mode isn't missing documentation but *wrong* documentation. Agents follow outdated specs confidently, producing plausible but incorrect code. Drift detection tooling is essential.
- **Single-developer limitation is significant.** The infrastructure was built by one person who has perfect knowledge of what's documented and what's stale. Team dynamics — conflicting updates, differing conventions, ownership ambiguity — could fundamentally change the maintenance equation.
- **Observational methodology limits causal claims.** Without controlled experiments, we can't isolate how much improvement comes from the infrastructure vs. the developer simply gaining experience over 70 days.
- **Domain specificity.** Real-time distributed simulation with deterministic networking is an unusually documentation-heavy domain. The 24% ratio may not generalize to CRUD apps or data pipelines.

## Citation

Vasilopoulos, A. (2026). Codified Context: Infrastructure for AI Agents in a Complex Codebase. *arXiv preprint arXiv:2602.20478*. https://arxiv.org/abs/2602.20478
