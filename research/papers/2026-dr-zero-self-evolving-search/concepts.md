---
title: "Concepts — Dr. Zero: Self-Evolving Search Agents without Training Data"
paper: 2026-dr-zero-self-evolving-search
---

# Prerequisite Concepts

Concepts and background knowledge assumed by the paper. Each entry gives a brief explanation and a pointer to learn more.

## Group Relative Policy Optimization (GRPO)

**What it is:** A reinforcement learning algorithm for LLMs that constructs baselines from groups of sampled responses to the same prompt, rather than using a learned value function. Advantages are computed by standardizing rewards within each group, reducing variance without a separate critic model.

**Why it matters here:** GRPO is the foundation that Dr. Zero builds on for solver training, and HRPO is a direct modification of GRPO designed to avoid its expensive nested sampling requirement.

**Learn more:** [DeepSeek-R1 paper (Shao et al., 2024)](https://arxiv.org/abs/2402.03300)

## Multi-Hop Question Answering

**What it is:** QA tasks that require combining information from multiple sources or reasoning steps to arrive at an answer. A "2-hop" question requires chaining two facts; higher hops require more steps.

**Why it matters here:** Dr. Zero specifically targets multi-hop QA as the setting where prior self-evolution methods fail — they tend to generate only simple 1-hop questions. The hop count is also the grouping key for HRPO.

**Learn more:** [HotpotQA (Yang et al., 2018)](https://arxiv.org/abs/1809.09600)

## Self-Play / Self-Evolution in LLMs

**What it is:** A training paradigm where a model improves by generating its own training data — proposing problems, solving them, and learning from the outcomes — without external human annotation. Related to self-play in game-playing AI (e.g., AlphaZero).

**Why it matters here:** Dr. Zero extends this paradigm from narrow domains (math, coding) to open-domain search, using a split proposer-solver architecture rather than a single unified model.

**Learn more:** [R-Zero (Huang et al., 2025)](https://arxiv.org/abs/2508.05004)

## Retrieval-Augmented Generation (RAG)

**What it is:** A technique where an LLM is given access to an external retrieval system (e.g., a search engine over a document corpus) to ground its responses in factual information. The model generates search queries, receives results, and incorporates them into its reasoning.

**Why it matters here:** Dr. Zero's agents are RAG-style search agents — both proposer and solver use multi-turn search over Wikipedia via an E5-base retrieval engine. The search engine is the sole external knowledge source enabling data-free self-evolution.

**Learn more:** [RAG (Lewis et al., 2020)](https://arxiv.org/abs/2005.11401)

## Policy Gradient Methods / REINFORCE

**What it is:** A family of RL algorithms that optimize a policy by computing gradients of expected reward with respect to policy parameters. REINFORCE is the simplest variant; more advanced methods (PPO, GRPO) add variance reduction techniques.

**Why it matters here:** HRPO is a policy gradient method. The paper discusses why simpler variants like REINFORCE++ fail for this setting (high variance from diverse query structures) and how hop-based grouping stabilizes gradients.

**Learn more:** [Sutton & Barto, RL: An Introduction, Ch. 13](http://incompleteideas.net/book/the-book.html)
