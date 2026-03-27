---
title: "Concepts — AVO: Agentic Variation Operators for Autonomous Evolutionary Search"
paper: 2026-avo-agentic-evolutionary-search
---

# Prerequisite Concepts

Concepts and background knowledge assumed by the paper. Each entry gives a brief
explanation and a pointer to learn more.

## Evolutionary Search / Genetic Algorithms

**What it is:** A family of optimization algorithms inspired by natural selection. A population of candidate solutions evolves over generations through selection (keeping fit candidates), crossover (combining parts of two parents), and mutation (random perturbation).

**Why it matters here:** AVO keeps the population-based selection loop but replaces crossover and mutation with LLM agents, so understanding the classical framework is essential to grasp what changed.

**Learn more:** [Introduction to Evolutionary Computing (Eiben & Smith)](https://link.springer.com/book/10.1007/978-3-662-44874-8)

## Variation Operators (Mutation & Crossover)

**What it is:** The mechanisms in evolutionary algorithms that create new candidate solutions from existing ones. Mutation perturbs a single solution; crossover recombines material from two or more parents.

**Why it matters here:** AVO's central claim is that LLM agents are superior variation operators because they can reason about *why* a solution works, rather than making blind stochastic changes.

**Learn more:** [Wikipedia — Genetic operator](https://en.wikipedia.org/wiki/Genetic_operator)

## Flash Attention

**What it is:** An exact attention algorithm that reduces memory usage from O(N²) to O(N) by tiling the computation and avoiding materialization of the full attention matrix. Introduced by Dao et al. (2022) and iterated through multiple versions.

**Why it matters here:** The primary benchmark domain is attention kernel optimization. AVO's discovered kernels are compared against FlashAttention-4 as a strong baseline.

**Learn more:** [FlashAttention paper (Dao et al., 2022)](https://arxiv.org/abs/2205.14135)

## GPU Kernel Optimization

**What it is:** Writing and tuning low-level compute kernels (CUDA, PTX, or via compilers like Triton) to maximize throughput on GPU hardware. Involves managing memory hierarchies, thread scheduling, occupancy, and instruction-level parallelism.

**Why it matters here:** AVO agents generate and refine GPU kernel code. The fitness evaluator measures actual execution time on NVIDIA Blackwell hardware, making this a real-world systems optimization problem.

**Learn more:** [CUDA C++ Programming Guide](https://docs.nvidia.com/cuda/cuda-c-programming-guide/)

## Grouped-Query Attention (GQA)

**What it is:** A variant of multi-head attention where multiple query heads share a single key-value head, reducing the KV-cache size and memory bandwidth requirements. Introduced by Ainslie et al. (2023).

**Why it matters here:** AVO demonstrates transfer by adapting optimized attention kernels to GQA with minimal additional search time (~30 minutes).

**Learn more:** [GQA paper (Ainslie et al., 2023)](https://arxiv.org/abs/2305.13245)

## FunSearch / LLM-in-the-Loop Search

**What it is:** A paradigm where LLMs generate candidate programs or solutions inside a search loop, with an evaluator providing fitness signals. DeepMind's FunSearch (2023) is a prominent example, using LLMs to discover new mathematical constructions.

**Why it matters here:** AVO extends this paradigm with structured agent roles (exploration, refinement, hybridization) and lineage-aware prompting, distinguishing it from simpler generate-and-evaluate approaches.

**Learn more:** [FunSearch paper (Romera-Paredes et al., 2023)](https://arxiv.org/abs/2312.17259)
