---
title: "Dr. Zero: Self-Evolving Search Agents without Training Data"
authors: [Zhenrui Yue, Kartikeya Upasani, Xianjun Yang, Suyu Ge, Shaoliang Nie, Yuning Mao, Zhe Liu, Dong Wang]
year: 2026
venue: "arXiv preprint arXiv:2601.07055"
url: "https://arxiv.org/abs/2601.07055"
date_analyzed: 2026-03-29
tags: [papers, self-evolution, search-agents, reinforcement-learning, data-free, multi-hop-qa]
---

# Dr. Zero: Self-Evolving Search Agents without Training Data

## Summary

Dr. Zero (DeepResearch-Zero) is a framework that enables LLM-based search agents to self-evolve without any training data, human questions, or answer annotations. It uses a proposer-solver co-evolution loop where one agent generates increasingly diverse and difficult multi-hop questions while the other learns to solve them using an external search engine. The key innovation is that this symbiotic feedback loop — combined with a novel optimization method called HRPO — produces search agents that match or surpass fully supervised baselines, demonstrating that complex reasoning and search capabilities can emerge purely from self-play with a search engine.

## Key Ideas

- **Zero-data self-evolution for search agents** — Unlike prior self-evolution work that targets math/coding (narrow, structured domains), Dr. Zero tackles open-domain question answering where question diversity is critical. It eliminates the need for any human-curated training data by relying solely on an external search engine as the source of supervision signal.

- **Proposer-solver co-evolution** — Two agents (initialized from the same base LLM) evolve in tandem: the proposer generates QA pairs using multi-turn search, and the solver attempts to answer them. The proposer is rewarded for generating questions that are solvable but non-trivial (not too easy, not too hard), creating an automatic curriculum that ratchets up in difficulty as the solver improves.

- **Hop-grouped relative policy optimization (HRPO)** — Standard GRPO requires nested sampling (m queries × n rollouts per query), which is prohibitively expensive for multi-turn search agents. HRPO avoids this by grouping questions by structural complexity (number of reasoning hops) and computing advantages within each hop group. This gives a stable baseline without needing multiple samples per prompt, cutting compute dramatically.

- **Difficulty-guided reward design** — The proposer reward peaks when exactly one solver prediction is correct out of n attempts, and decays as more are correct (trivial) or none are correct (unsolvable). This incentivizes the "Goldilocks zone" of question difficulty that maximizes learning signal.

## Method / Approach

Both proposer and solver start from the same base LLM (Qwen2.5-3B/7B-Instruct) with access to a search engine (E5-base over English Wikipedia). Training alternates between:

1. **Proposer update (HRPO):** The proposer generates a batch of QA pairs with varying hop counts (1–4 hops, default ratio 4:3:2:1). The solver attempts each question n times. The proposer's reward is computed from the solver's pass rate — rewarding questions where the solver partially succeeds. Advantages are standardized within each hop group, eliminating the need for multi-sample baselines per prompt.

2. **Solver update (GRPO):** The solver trains on the proposer's generated questions using standard GRPO with outcome-based reward (exact match against the proposer's synthesized answer).

This cycle repeats for multiple iterations (typically 3), with each iteration producing stronger versions of both agents.

## Evidence / Results (high level)

The data-free Dr. Zero matches or exceeds fully supervised search agents (Search-R1, SFT, R1-Instruct) across seven open-domain QA benchmarks spanning single-hop and multi-hop tasks. On single-hop benchmarks it outperforms the supervised Search-R1 by up to 22.9%. It also substantially outperforms other data-free methods (R-Zero, SQLM) by ~27–40% on average. Most gains materialize in the first two training iterations, with diminishing returns after iteration 3.

## Insights & Takeaways

- **Self-play + external tools = powerful combination.** The search engine provides the grounding that makes open-domain self-evolution possible. Without it, the proposer would have no way to generate verifiable factual questions. This pattern — self-play bootstrapped by tool access — likely generalizes beyond search to other tool-augmented agent settings.

- **Curriculum emerges naturally from the feedback loop.** The proposer doesn't need an explicit curriculum schedule; the difficulty-guided reward and the solver's improving competence create an automatic curriculum. This is elegant and avoids the brittle hand-tuning of difficulty schedules.

- **HRPO is a practical contribution.** The compute savings from avoiding nested sampling are substantial for multi-turn agents where each rollout involves multiple search interactions. The insight that structurally similar questions (same hop count) can serve as each other's baseline is simple but effective.

- **Limitations to note:** Performance plateaus after 2–3 iterations, and the 7B model sometimes shows instability (entropy collapse, inconsistent token IDs across multi-turn steps). The authors acknowledge future work on stability, reward hacking prevention, and bias amplification — all real risks in unsupervised self-evolution loops.

- **Practical utility:** The framework is most relevant for teams wanting to build search/research agents without expensive annotation pipelines. The HRPO technique is independently useful for any RL training of multi-turn agents where rollouts are expensive.

## Citation

> Yue, Z., Upasani, K., Yang, X., Ge, S., Nie, S., Mao, Y., Liu, Z., & Wang, D. "Dr. Zero: Self-Evolving Search Agents without Training Data." *arXiv preprint arXiv:2601.07055*, 2026. [link](https://arxiv.org/abs/2601.07055)
