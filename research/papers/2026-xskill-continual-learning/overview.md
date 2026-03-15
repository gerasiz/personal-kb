---
title: "XSkill: Continual Learning from Experience and Skills in Multimodal Agents"
authors:
  - Guanyu Jiang
  - Zhaochen Su
  - Xiaoye Qu
  - Yi R. Fung
year: 2026
venue: arXiv preprint
url: https://arxiv.org/abs/2603.12056
date_analyzed: 2026-03-15
tags:
  - continual-learning
  - multimodal-agents
  - tool-use
  - knowledge-accumulation
  - experience-learning
  - skill-extraction
  - dual-stream-architecture
  - zero-shot-generalization
---

# XSkill: Continual Learning from Experience and Skills in Multimodal Agents

## Summary

XSkill is a dual-stream continual learning framework that enables multimodal agents to improve over time without parameter updates. It addresses two bottlenecks in current agents: inefficient tool use and inflexible tool orchestration. The framework maintains two complementary knowledge forms — *skills* (task-level structured workflow documents) and *experiences* (action-level tactical prompts) — both grounded in visual observations from agent trajectories. Knowledge is accumulated from multi-path rollouts via visually-grounded summarization and cross-rollout critique, then retrieved and adapted to new tasks at inference time using current visual context. Evaluated across five benchmarks and four backbone models, XSkill achieves 2.58–6.71 point average gains over tool-only baselines and demonstrates strong zero-shot transfer between related benchmarks.

## Key Ideas

- **Dual-stream knowledge.** Two complementary knowledge forms target distinct agent weaknesses: *skills* provide structural robustness for task execution (workflow sequences, code templates), while *experiences* provide tactical flexibility for tool selection decisions (condition-action pairs, 64-word max). Neither alone is sufficient — ablations show removing either degrades performance by 3–4 points.
- **Visually-grounded extraction.** Unlike prior text-only approaches, XSkill interleaves image observations with trajectory text during knowledge extraction. This bridges the visual-semantic gap by recording which visual evidence motivated specific decisions.
- **Cross-rollout critique.** The framework performs contrastive analysis between successful and failed trajectories to identify causal factors behind outcomes, generating structured experience updates (add or modify operations) rather than simply summarizing what happened.
- **Hierarchical consolidation.** Experiences are merged when exceeding a similarity threshold (θ_sim=0.70) and pruned at a capacity limit (N_max=120). Skills are integrated into global documents and refined when exceeding 1,000 words. This reduces redundancy while maintaining generalizability.
- **Context-aware retrieval and adaptation.** At inference, tasks are decomposed into abstract subtasks for targeted retrieval. Retrieved experiences are rewritten to match the current visual context and task, and skills are pruned/adapted accordingly — making knowledge non-prescriptive and situationally relevant.
- **Continual learning loop.** Usage history from inference feeds back into the accumulation phase, forming a closed loop that refines knowledge over time without retraining.

## Method / Approach

The framework operates in two phases:

**Phase I — Accumulation:** Given training tasks, the system performs multiple rollouts per task. A knowledge-building MLLM (MLLM_kb) processes each rollout set with interleaved images and trajectory text, producing trajectory summaries and skill fragments. Cross-rollout critique then compares summaries to extract structured experience updates. Finally, a consolidation step merges similar experiences (cosine similarity > 0.70), prunes excess entries, and integrates skill fragments into coherent global skill documents assessed for generalizability and actionability.

**Phase II — Inference:** For a new task, the system: (1) decomposes it into abstract subtasks and generates subtask-specific retrieval queries, (2) retrieves top-k=3 experiences per subtask via embedding similarity, (3) rewrites retrieved experiences using the current task and visual context (filtering inapplicable ones), (4) adapts the skill document by pruning irrelevant sections and integrating rewritten experiences, and (5) injects the adapted knowledge non-prescriptively into the agent's system prompt.

**Tools available to agents:** code interpreter (Python), web search, image search, and webpage visit.

**Key hyperparameters:** merge threshold θ_sim=0.70, max experiences=120, max skill length=1,000 words, max experience length=64 words, retrieval k=3, temperature=0.6 (extraction) / 0.3 (adaptation), max agent turns=20.

## Results / Claims

**Main results (Table 2):** Across five benchmarks and four models (Gemini-2.5-Pro, Gemini-3-Flash, GPT-5-mini, o4-mini), XSkill consistently outperforms tool-only baselines and three learning-based baselines (AWM, Dynamic CheatSheet, Agent-KB). Strongest gains on Gemini-3-Flash: +4.75 on VisualToolBench, +11.13 on TIR-Bench, +3.91 on MMSearch-Plus (all Average@4).

**Ablation study (Table 3):** On VisualToolBench with Gemini-2.5-Pro, removing experiences costs 3.04 points, removing skills costs 3.85 points. Phase I components (experience/skill managers) are more critical than Phase II components (task decomposition/adaptation), with drops of 3.62–4.09 vs. 1.28–1.52.

**Error analysis:** Skills dramatically reduce execution failures — overall error rate drops from 29.9% (experience-only) to 15.3% (skill-only). Tool name errors nearly eliminated (16 → 2). Experiences shift tool usage distributions toward task-appropriate patterns.

**Cross-model transfer:** Knowledge accumulated with Gemini-3-Flash transfers effectively to GPT-5-mini and o4-mini, achieving 2.58–4.16 point gains without model-specific accumulation.

**Zero-shot transfer:** Knowledge from VisualToolBench generalizes to TIR-Bench (2–3 point gains), and MMSearch-Plus knowledge transfers to MMBrowseComp, outperforming all baselines.

**Open-source models:** Mixed results — effective for stronger models (Qwen3-VL-235B Pass@4 improves) but less reliable for weaker ones, indicating base model capability is a prerequisite for effective knowledge transfer.

## Insights & Takeaways

- **Complementary knowledge streams address distinct failure modes.** Skills reduce structural/execution errors (syntax, tool naming) while experiences guide tactical decisions (which tool to use when). This separation is a useful design pattern for any agent learning system.
- **Visual grounding matters for multimodal agents.** The consistent advantage over text-only baselines (AWM, Dynamic CheatSheet) validates that knowledge extraction from multimodal trajectories must attend to image observations, not just action sequences.
- **Accumulation quality trumps retrieval sophistication.** Ablations show Phase I components (cross-rollout critique, consolidation) have roughly 3x the impact of Phase II components (decomposition, adaptation). Getting the knowledge right matters more than getting the retrieval right.
- **Non-parametric learning is competitive.** Achieving consistent gains without any parameter updates is practically significant — it avoids the catastrophic forgetting, compute costs, and alignment risks of fine-tuning.
- **Base model capability gates knowledge transfer.** Weaker open-source models struggle to effectively use accumulated knowledge, suggesting a capability threshold below which external knowledge injection adds noise rather than signal.
- **Single accumulation-then-test limitation.** The evaluation uses one accumulation cycle, so the continual learning loop's iterative refinement isn't fully demonstrated. Future work could show how knowledge quality evolves over multiple cycles.

## Citation

Jiang, G., Su, Z., Qu, X., & Fung, Y. R. (2026). XSkill: Continual Learning from Experience and Skills in Multimodal Agents. *arXiv preprint arXiv:2603.12056*. https://arxiv.org/abs/2603.12056
