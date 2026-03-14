---
title: "SWE-CI: Evaluating Agent Capabilities in Maintaining Codebases via Continuous Integration"
authors: [Jialong Chen, Xander Xu, Hu Wei, Chuan Chen, Bing Zhao]
year: 2026
venue: "arXiv"
url: "https://arxiv.org/abs/2603.03823"
date_analyzed: 2026-03-14
tags: [papers, benchmarks, software-engineering, agents, code-maintenance, continuous-integration]
---

# SWE-CI: Evaluating Agent Capabilities in Maintaining Codebases via Continuous Integration

## Summary

SWE-CI is a benchmark that evaluates LLM-powered agents on long-term codebase maintenance rather than one-shot bug fixes. It shifts evaluation from "static, short-term functional correctness" (as in SWE-bench) to "dynamic, long-term maintainability." The benchmark comprises 100 tasks drawn from 68 real Python repositories, with evolution histories averaging 233 days and 71 commits per task. The key finding is that current models still struggle with regression prevention during extended development—only Claude Opus exceeds a 50% zero-regression rate, while most models fall below 25%.

## Key Ideas

- **Evolution-based evaluation** — Instead of measuring whether an agent can produce a correct patch for a single issue, SWE-CI tests whether agents can maintain code quality across a sequence of iterative changes that mirror real CI workflows.
- **Normalized Change metric** — A unified [-1, 1] scale that captures both improvements and regressions asymmetrically: improvements are normalized by the total gap to target, regressions by the baseline. This prevents inflated scores from easy early wins.
- **EvoScore** — A future-weighted mean (using γ ≥ 1) that gives greater weight to later iterations, penalizing agents that accumulate technical debt over time.
- **Dual-agent protocol** — Separates evaluation into an Architect agent (identifies requirements from failing tests) and a Programmer agent (implements fixes), mirroring real development workflows and preventing agents from blindly chasing test failures.

## Method / Approach

### Task Formalization

The framework defines two functions: `require` (identifies functional gaps between current and target codebases) and `code` (modifies a codebase to meet requirements). Unlike snapshot benchmarks, requirements are generated dynamically: each iteration builds on the previous agent-modified codebase.

### Benchmark Construction (4 stages)

1. **Repository collection** — 4,923 Python repos filtered by: 3+ years active maintenance, 500+ stars, test suites, permissive licenses.
2. **Commit span extraction** — 8,311 candidate base/oracle pairs identified across periods with unchanged dependencies, requiring 1,000+ modified source lines.
3. **Environment construction** — Dockerfiles generated for 1,458 viable pairs with self-repair for dependency issues.
4. **Case filtering** — Automated and manual quality checks yielded 100 final samples from 68 repositories.

### Evaluation Protocol

The Architect agent summarizes failing tests, locates implementation deficiencies, and designs a plan limited to five urgent requirements. The Programmer agent then comprehends, plans, and implements solutions. This cycle repeats across iterations, simulating continuous integration.

## Results

- **Accelerating progress:** Models released after 2026 show substantially larger capability gains. Claude Opus and GLM-5 demonstrated leading performance.
- **Provider-specific patterns:** Training philosophies influence maintainability outcomes. Long-term focused: MiniMax, DeepSeek, GPT. Short-term focused: Kimi, GLM. Balanced: Qwen, Doubao, Claude.
- **Regression control is hard:** Zero-regression rates (maintaining all previously passing tests throughout evolution) remain below 25% for most models. Only Claude Opus exceeds 50%. This reveals that passing static benchmarks does not translate to reliable long-term maintenance.

## Insights & Takeaways

- **Maintainability ≠ correctness.** Two agents can score identically on SWE-bench yet diverge dramatically on SWE-CI, because brittle code and extensible code are indistinguishable in single-shot evaluation.
- **Regression is the bottleneck.** The most differentiated capability is not writing new code but avoiding breaking existing functionality during evolution—a skill that current models handle poorly.
- **Training strategy matters.** The consistent within-provider patterns suggest that maintainability is influenced by training data and RLHF priorities, not just raw model scale.
- **Practical implications.** Teams using LLM agents for ongoing codebase work should invest in strong CI guardrails and regression testing, since agents are likely to introduce regressions over iterative changes.
- **Limitations.** The benchmark is Python-only (100 tasks from 68 repos), the dual-agent split may not reflect all real workflows, and the requirement generation depends on oracle codebases that may not perfectly model organic project evolution.

## Citation

> Jialong Chen, Xander Xu, Hu Wei, Chuan Chen, Bing Zhao. "SWE-CI: Evaluating Agent Capabilities in Maintaining Codebases via Continuous Integration." *arXiv*, 2026. [arXiv:2603.03823](https://arxiv.org/abs/2603.03823)
