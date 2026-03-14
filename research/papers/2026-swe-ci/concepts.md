---
title: "Concepts — SWE-CI"
paper: 2026-swe-ci
---

# Prerequisite Concepts

Concepts and background knowledge assumed by the paper. Each entry gives a brief
explanation and a pointer to learn more.

## SWE-bench

**What it is:** A benchmark that evaluates LLM agents on their ability to resolve real GitHub issues by generating code patches. Tasks are drawn from popular Python repositories and verified against existing test suites.

**Why it matters here:** SWE-CI is explicitly positioned as addressing SWE-bench's limitations—namely that SWE-bench only measures one-shot functional correctness and cannot capture long-term maintainability.

**Learn more:** [SWE-bench paper (arXiv)](https://arxiv.org/abs/2310.06770) · [SWE-bench website](https://www.swebench.com/)

## Continuous Integration (CI)

**What it is:** A development practice where code changes are frequently merged into a shared mainline, with automated builds and test suites running on each integration to catch regressions early.

**Why it matters here:** SWE-CI models its iterative evaluation protocol after CI workflows—each iteration runs the test suite against agent modifications, and the benchmark specifically measures whether agents can avoid regressions across successive changes.

**Learn more:** [Martin Fowler — Continuous Integration](https://martinfowler.com/articles/continuousIntegration.html)

## Agentic Coding Workflows

**What it is:** Systems where LLM-powered agents autonomously plan, write, test, and iterate on code—going beyond single-turn code generation to multi-step tool-using workflows (reading files, running tests, editing code).

**Why it matters here:** SWE-CI's dual-agent protocol (Architect + Programmer) is an agentic workflow. The benchmark evaluates how well these agents handle the iterative, multi-step nature of real software maintenance.

**Learn more:** [OpenAI — Practices for Governing Agentic AI Systems](https://openai.com/index/practices-for-governing-agentic-ai-systems/)

## Software Regression

**What it is:** A defect where previously working functionality breaks after a code change. Regression testing re-runs existing tests to detect such breakage.

**Why it matters here:** Regression control is the paper's most differentiating metric. The zero-regression rate (share of tasks where an agent never breaks a previously passing test across all iterations) is where most models fail hardest—below 25%.

**Learn more:** [Wikipedia — Regression testing](https://en.wikipedia.org/wiki/Regression_testing)

## Technical Debt

**What it is:** The implied cost of future rework caused by choosing an expedient but limited solution now instead of a better approach that would take longer.

**Why it matters here:** SWE-CI's EvoScore uses future-weighting (γ ≥ 1) specifically to penalize agents that accumulate technical debt—producing quick fixes early that degrade code quality in later iterations.

**Learn more:** [Martin Fowler — Technical Debt](https://martinfowler.com/bliki/TechnicalDebt.html)

## Evaluation Metrics: Normalized Change & EvoScore

**What it is:** Two novel metrics introduced by this paper. Normalized Change maps agent progress to a [-1, 1] scale (positive = improvement toward target, negative = regression from baseline). EvoScore aggregates these across iterations using a future-weighted mean that rewards sustained quality.

**Why it matters here:** These metrics are the paper's primary contribution to evaluation methodology, replacing simple pass/fail with a nuanced measure of maintenance trajectory.

**Learn more:** See the [SWE-CI paper](https://arxiv.org/abs/2603.03823), Section 3.
