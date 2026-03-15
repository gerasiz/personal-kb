---
title: "Concepts — Structured Context Engineering for File-Native Agentic Systems"
paper: 2026-structured-context-engineering
---

# Prerequisite Concepts

Concepts and background knowledge assumed by the paper. Each entry gives a brief
explanation and a pointer to learn more.

## Context Engineering

**What it is:** The practice of systematically designing and optimizing the information payload delivered to an LLM — encompassing retrieval, formatting, ordering, and management of context. Distinct from prompt engineering, which focuses on instruction wording; context engineering focuses on the structured data the model receives.

**Why it matters here:** The paper is fundamentally a study of context engineering — how schema format, delivery architecture, and scale affect agent performance. It references the formalization of context engineering as a discipline via Mei et al.'s survey of over 1,400 papers.

**Learn more:** [A survey of context engineering for large language models (Mei et al., 2025)](https://arxiv.org/abs/2507.13334)

## File-Native Agentic Systems

**What it is:** LLM agent architectures where the model retrieves context from files on disk using programmatic tools (grep, read, cat) rather than receiving all context in the prompt. Examples include CLAUDE.md, AGENTS.md, Cursor Rules, and llms.txt — all structured files that agents read at runtime.

**Why it matters here:** This is the core architecture under study. The paper compares file-native retrieval against prompt-based context injection to determine when each approach is effective.

**Learn more:** [Model Context Protocol specification](https://modelcontextprotocol.io/)

## Text-to-SQL

**What it is:** The task of translating natural language questions into SQL queries that can be executed against a relational database. A standard benchmark task for evaluating LLM comprehension of structured data.

**Why it matters here:** Used as the proxy task for all experiments. SQL generation requires schema comprehension, relationship understanding, and correct syntax — making it a good test of how well an agent understands structured context.

**Learn more:** [Spider benchmark (Yu et al., 2018)](https://arxiv.org/abs/1809.08887)

## TPC-DS Benchmark

**What it is:** A decision support benchmark from the Transaction Processing Performance Council. Defines a retail data warehouse schema with 24 tables and standardized query patterns across varying complexity levels.

**Why it matters here:** The paper derives all experimental queries from TPC-DS query patterns, curated into 5 complexity tiers (L1–L5). The 24-table schema serves as the base for the core experiment, and synthetic extensions scale it to 10,000 tables.

**Learn more:** [TPC-DS benchmark specification](https://www.tpc.org/tpcds/)

## TOON (Token-Oriented Object Notation)

**What it is:** A compact serialization format designed specifically for LLM context efficiency. Achieves ~25% smaller file sizes than YAML through abbreviated keywords (TABLE, COL, FK) and minimal syntax.

**Why it matters here:** Included as a test of whether file-size optimization translates to runtime efficiency. The paper finds it does not — TOON incurs a "grep tax" from denser output and pattern unfamiliarity, consuming 38% more tokens than YAML despite being 25% smaller on disk.

**Learn more:** [TOON Format specification](https://toonformat.dev/)

## Jaccard Similarity

**What it is:** A set similarity metric defined as J(A, B) = |A ∩ B| / |A ∪ B|, ranging from 0 (no overlap) to 1 (identical sets). Commonly used for comparing result sets.

**Why it matters here:** The evaluation metric for SQL accuracy. Agent-generated and ground-truth result sets are compared using Jaccard similarity, with a threshold of >= 0.9 for success. This allows credit for semantically equivalent but syntactically different SQL.

**Learn more:** [Jaccard index (Wikipedia)](https://en.wikipedia.org/wiki/Jaccard_index)

## Benjamini-Hochberg Correction

**What it is:** A statistical procedure for controlling the false discovery rate (FDR) when performing multiple hypothesis tests. Less conservative than Bonferroni correction, it ranks p-values and adjusts thresholds to limit the expected proportion of false positives.

**Why it matters here:** Applied to control for multiple comparisons across the many model/format/architecture combinations tested (11 models × 4 formats × 2 architectures). Ensures reported significant results are robust.

**Learn more:** [Benjamini & Hochberg (1995)](https://doi.org/10.1111/j.2517-6161.1995.tb02031.x)

## ReAct Framework

**What it is:** An agent framework where LLMs interleave chain-of-thought reasoning with tool invocations (actions). The model reasons about what to do, takes an action, observes the result, and repeats.

**Why it matters here:** The file-native agent architecture builds on ReAct-style reasoning-plus-acting. Agents reason about which schema sections to grep, observe results, and iterate — the quality of this loop varies by model tier.

**Learn more:** [ReAct: Synergizing Reasoning and Acting in Language Models (Yao et al., 2023)](https://arxiv.org/abs/2210.03629)
