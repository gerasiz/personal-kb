---
title: "Structured Context Engineering for File-Native Agentic Systems"
authors: [Damon McMillan]
year: 2026
venue: "arXiv preprint"
url: "https://arxiv.org/abs/2602.05447"
date_analyzed: 2026-03-15
tags: [papers, context-engineering, llm-agents, file-native, text-to-sql, schema-formats, enterprise-ai]
---

# Structured Context Engineering for File-Native Agentic Systems

## Summary

This paper systematically studies how to structure context for LLM agents that operate through programmatic file interfaces (grep, read) rather than receiving everything in a prompt. Using SQL generation on the TPC-DS benchmark as a proxy task, the authors ran 9,649 experiments across 11 models, 4 schema formats (YAML, Markdown, JSON, TOON), 2 architectures (file agent vs prompt baseline), and schemas ranging from 10 to 10,000 tables. The central finding is that architectural and format decisions should be tailored to model capability tier rather than following universal best practices — model capability is the dominant performance factor, dwarfing format or architecture effects.

## Key Ideas

- **Architecture choice is model-dependent.** File-native retrieval benefits frontier models (+2.7%, p=0.029) but hurts open source models in aggregate (-7.7%, p<0.001). The effect varies widely even within tiers.
- **Format doesn't significantly affect accuracy.** Across all models, no statistically significant difference between YAML (75.4%), Markdown (74.9%), JSON (72.3%), and TOON (72.3%) — chi-squared=2.45, p=0.484. Individual models do show format preferences.
- **Model capability dominates.** A 21 percentage-point gap separates frontier (86.0%) from open source (64.6%) tiers, far exceeding any format or architecture effect. One-way ANOVA: F(10, 8390)=30.55, p<0.001.
- **File-native agents scale to 10,000 tables.** Domain-based partitioning (~250 tables per domain file) maintains high navigation accuracy at enterprise scale while keeping per-query context bounded.
- **The "grep tax."** Compact or novel formats can increase token consumption despite smaller file sizes. TOON is ~25% smaller than YAML on disk but consumes 38% more tokens on average, due to denser grep output and pattern unfamiliarity causing retry loops.

## Method / Approach

**Task:** SQL generation against the TPC-DS benchmark (retail data warehouse). Queries span 5 complexity tiers (L1 direct lookup through L5 multi-step reasoning with subqueries).

**Architectures compared:**
1. **File Agent** — agents use grep and read tools to selectively retrieve schema information from files on demand.
2. **Prompt Baseline** — full schema (~6,000 tokens for TPC-DS) embedded directly in the system prompt with no tool access.

**Formats:** YAML (hierarchical, grep-friendly), Markdown (documentation-style with pipe tables), JSON (machine-parseable, verbose), TOON (Token-Oriented Object Notation — ~25% smaller via abbreviated keywords).

**Models (11 total, 3 tiers):**
- *Frontier:* claude-opus-4.5, gpt-5.2, gemini-2.5-pro
- *Frontier Lab:* claude-haiku-4.5, gpt-5-mini, gemini-2.5-flash
- *Open Source:* DeepSeek-V3.2, kimi-k2, llama-4-maverick, llama-4-scout, qwen3-32b

**Evaluation:** Jaccard similarity between agent-generated and ground-truth result sets. Success threshold: Jaccard >= 0.9. Temperature=0 for deterministic single-run evaluation. Statistical tests: independent t-tests, chi-square, ANOVA with Benjamini-Hochberg correction.

**Scale experiments (S0–S9):** 10 to 10,000 tables. S0–S5 use single-file schemas; S6–S9 use domain-partitioned multi-file schemas. Scale experiments used Claude models only.

## Results

### R1: File Agent vs Prompt Engineering
- Frontier + Frontier Lab combined: file agent 79.8% vs prompt 77.1% (+2.7%, p=0.029, significant).
- Open Source: file agent 64.6% vs prompt 72.3% (-7.7%, p<0.001, significant but in wrong direction).
- Best individual file agent advantage: GPT-5-mini (+7.7%). Worst: Qwen (-21.9%).
- Claude Opus achieved 89.0% (file agent only, no prompt baseline available).

### R2: Format Effects
- No significant aggregate effect (p=0.484).
- YAML was best format for 5 models, MD for 4, JSON for 2, TOON for 0.
- Open source models showed larger format sensitivity (spread 9.8–20.1%) vs frontier (1.6–5.4%).

### R3: Model Tier Effects
- Frontier: 86.0%, Frontier Lab: 76.7%, Open Source: 64.6%.
- All tiers similar at L1 (94–96%); diverge sharply at higher complexity. At L5: frontier 64%, open source 27%.

### R4: Navigation at Scale
- Single-file schemas: near-perfect navigation accuracy up to 1,000 tables.
- Domain-partitioned schemas: high accuracy at 10,000 tables, per-query context stays bounded.

### R5: Efficiency
- YAML most token-efficient (12,729 avg tokens), then JSON (+28%), TOON (+38%), MD (+60%).
- MD overhead from verbose pipe-table formatting in grep results.
- TOON overhead from two sources: (1) token density — compact syntax returns denser grep output; (2) pattern unfamiliarity — models try familiar formats (DDL, JSON) before discovering TOON syntax, causing retry loops. Opus handled this gracefully (max 4 tool calls); Haiku worst case: 16 grep attempts vs 3 for YAML.

## Insights & Takeaways

- **Invest in model capability before optimizing format.** The 21pp tier gap dwarfs everything else. Upgrading from an open source model to a frontier model yields far more than any format or architecture change.
- **File-native architecture is not universally better.** It benefits strong models that use tools well but can hurt weaker models. Validate before deploying.
- **YAML is the pragmatic default** for file-native agents: best token efficiency, grep-friendly patterns, and tied for best accuracy. Markdown is good for human readability but costs 60% more tokens.
- **Novel formats carry hidden costs.** TOON's file-size savings don't translate to runtime savings due to the grep tax. Format-specific prompt guidance (grep examples) might mitigate this but wasn't tested.
- **Domain partitioning solves enterprise scale.** ~250 tables per domain file keeps context bounded even at 10K tables. This is a practical architecture for real-world deployments.
- **Format influences interpretation, not accuracy.** Different formats guide models toward different valid SQL paths (e.g., which join path to use). This matters for ambiguous queries even if aggregate accuracy is unchanged.
- **Practical selection guides (from paper):**
  - Architecture: Frontier → file agent; Frontier Lab → file agent (validate first); Open Source → prompt engineering.
  - Format by priority: Token efficiency → YAML; Human readability → Markdown; Programmatic generation → YAML or JSON; Custom formats → ensure grep-friendly patterns.

## Citation

> McMillan, D. "Structured Context Engineering for File-Native Agentic Systems: Evaluating Schema Accuracy, Format Effectiveness, and Multi-File Navigation at Scale." *arXiv preprint arXiv:2602.05447*, 2026. [link](https://arxiv.org/abs/2602.05447)
