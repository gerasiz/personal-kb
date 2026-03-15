---
title: Prerequisite Concepts
paper: 2026-xskill-continual-learning
---

# Prerequisite Concepts

## Continual Learning (Non-Parametric)

**What it is:** A paradigm where agents improve over time by accumulating knowledge from past interactions without updating model weights. Contrasts with parametric continual learning (fine-tuning), which risks catastrophic forgetting and requires compute-intensive retraining.

**Why it matters here:** XSkill's core contribution is a non-parametric continual learning loop: knowledge is stored externally as structured text (skills and experiences), retrieved at inference, and refined through usage feedback — all without touching model parameters.

**Learn more:** https://arxiv.org/abs/2302.00487

## Multimodal Large Language Models (MLLMs)

**What it is:** LLMs extended with vision capabilities, able to process both text and images as input. Examples include Gemini, GPT-4V/GPT-5, and Qwen-VL. They enable agents to reason about visual information alongside textual context.

**Why it matters here:** XSkill is built for multimodal agents that must interpret visual observations (screenshots, images) during tool use. The framework's key innovation is *visually-grounded* knowledge extraction — interleaving image observations with trajectory text during summarization and critique.

**Learn more:** https://arxiv.org/abs/2306.13549

## Tool-Augmented Agents

**What it is:** LLM-based agents that interact with external tools (code interpreters, search APIs, browsers) to accomplish tasks beyond pure text generation. The agent plans which tools to invoke, interprets results, and chains actions across multiple turns.

**Why it matters here:** XSkill operates on top of tool-augmented agents, improving their tool selection (via experiences) and tool orchestration workflows (via skills). The benchmark tasks all require multi-step tool composition — code execution, web search, image search, and page visits.

**Learn more:** https://arxiv.org/abs/2304.08354

## Agent Trajectories / Rollouts

**What it is:** The complete sequence of observations, reasoning steps, tool calls, and results from a single agent execution on a task. Multiple rollouts on the same task (with sampling temperature > 0) produce diverse trajectories that may succeed or fail in different ways.

**Why it matters here:** XSkill's accumulation phase runs multiple rollouts per training task and performs cross-rollout critique — contrastive analysis between successful and failed trajectories to identify what caused success or failure. This multi-path approach is more informative than learning from single executions.

## Retrieval-Augmented Generation (RAG)

**What it is:** A pattern where relevant knowledge is retrieved from an external store (using embedding similarity or other methods) and injected into an LLM's context at inference time, rather than relying solely on parametric knowledge.

**Why it matters here:** XSkill's inference phase uses RAG-style retrieval: tasks are decomposed into subtasks, embedding-based similarity retrieves relevant experiences (top-k=3 per subtask), and retrieved knowledge is adapted before injection into the agent's system prompt. The innovation over vanilla RAG is the task decomposition and context-aware rewriting steps.

**Learn more:** https://arxiv.org/abs/2005.11401

## Cross-Rollout Critique

**What it is:** A contrastive analysis method that compares multiple trajectory summaries from the same task to identify causal factors behind success or failure. Rather than summarizing individual runs, it asks *why* some paths succeeded while others failed.

**Why it matters here:** This is one of XSkill's key technical contributions. The critique generates structured experience updates — (add, experience) or (modify, id, experience') operations — grounded in evidence from divergent outcomes. Ablations show this component is among the most impactful, with its removal costing ~4 points.

## Knowledge Consolidation

**What it is:** The process of merging, deduplicating, and quality-filtering accumulated knowledge entries to prevent redundancy and maintain a compact, high-quality knowledge store. Often uses embedding similarity thresholds and capacity limits.

**Why it matters here:** XSkill consolidates both knowledge streams: experiences are merged when cosine similarity exceeds 0.70 and pruned above 120 entries; skills are integrated into coherent documents capped at 1,000 words. Quality is assessed via generalizability (does it apply beyond one task?) and actionability (does it contain concrete guidance?).
