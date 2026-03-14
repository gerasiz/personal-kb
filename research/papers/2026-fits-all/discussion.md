---
title: 'Discussion — "One Size Fits All" and the Prompt-as-Platform Thesis'
paper: 2026-fits-all
related_tweet_paper: "https://x.com/dominiktornow/status/2031301110955421922"
related_tweet_thread: "https://x.com/dominiktornow/status/2028515744682508730"
date: 2026-03-14
tags: [papers, discussion, llm, platforms, durable-execution, formal-methods]
---

# Discussion: "One Size Fits All" Meets the Prompt-as-Platform Thesis

This document connects Stonebraker & Cetintemel's "One Size Fits All" paper with ideas
from Dominik Tornow's tweet threads on LLM-driven system design.

- **Paper link:** <https://x.com/dominiktornow/status/2031301110955421922>
- **Thread link:** <https://x.com/dominiktornow/status/2028515744682508730>

## Tornow's Key Ideas

### The Prompt Is the Platform

The core thesis: when you give an LLM a prompt, you are not just asking a question — you
are defining a platform. The prompt specifies the behavior, constraints, and interface of
a system. In this framing, the prompt replaces traditional platform configuration,
middleware setup, and even some application code. The LLM becomes the universal runtime
and the prompt becomes the specialization layer.

### Platforms on Demand

Rather than building and maintaining bespoke platforms for each use case, LLMs enable
"platforms on demand" — you describe what you need, and the model assembles the right
combination of capabilities at inference time. This collapses the traditional build/deploy
cycle into a single prompt-response interaction. Each prompt effectively instantiates a
specialized, ephemeral platform.

### Specification and Verification Harness

Tornow emphasizes that LLMs need formal specification and verification to be trustworthy
system components. The idea is to pair LLM-generated behavior with a spec/verification
harness: the LLM proposes actions, and a formal layer checks them against constraints
before execution. This separates the creative/generative capability (LLM) from the
correctness guarantee (formal methods), combining the flexibility of natural language with
the rigor of formal verification.

### Durable Execution Example

Durable execution — where workflow state survives failures and can be replayed
deterministically — serves as a concrete example of this pattern. Traditional durable
execution requires specialized frameworks (Temporal, Restate). In the prompt-as-platform
model, you could describe a durable workflow in a prompt, have the LLM generate the
execution plan, and use a verification harness to ensure the plan satisfies durability
and idempotency properties. The infrastructure becomes implicit rather than explicit.

## Connection to "One Size Fits All"

Stonebraker's paper argued that the monolithic RDBMS would fragment into specialized
engines because no single architecture could serve all workloads optimally. Twenty years
later, Tornow's thesis suggests LLMs might *reverse* this fragmentation — not by building
one system that does everything, but by making specialization cheap and dynamic.

### Parallels

| Stonebraker (2005) | Tornow (2026) |
|---|---|
| One RDBMS can't serve all workloads | One platform can't serve all use cases |
| Solution: specialized engines per domain | Solution: LLM generates specialized behavior per prompt |
| Common SQL parser over divergent engines | Common natural-language interface over dynamic capabilities |
| System factoring is a historical artifact | Platform boundaries are historical artifacts |
| Stream processing needs inbound architecture | Each task needs its own "architecture" assembled on demand |

### Tensions

- **Stonebraker's insight was about *deep* architectural specialization** — column stores vs row stores, inbound vs outbound processing. These are not superficial differences. Can an LLM truly generate deeply optimized, domain-specific systems from prompts, or will prompt-generated systems be "good enough" generalists?
- **The verification gap.** Stonebraker's specialized engines were hand-optimized and extensively tested. Tornow's spec/verification harness is the proposed answer to this for LLM systems, but it remains an open challenge — especially for performance-critical workloads where "correct but slow" is not acceptable.
- **Operational complexity.** Stonebraker underweighted the cost of maintaining many specialized engines. Tornow's prompt-as-platform model addresses this by making specialization ephemeral — but shifts complexity to the prompt engineering and verification layers.

### Synthesis

The deepest connection is about **system factoring**. Stonebraker's Section 6 questioned
whether the traditional decomposition of system software (DBMS + message bus + app server)
was optimal. Tornow extends this question to a more radical conclusion: if an LLM can
generate the right specialized behavior for each task, then perhaps no fixed factoring is
needed at all. The "platform" becomes whatever the prompt says it is, verified against
whatever formal properties the use case demands.

This is "one size fits all" turned inside out: instead of one rigid system trying to serve
every workload, you have one flexible system that *becomes* a different specialized system
for each workload. Whether this actually delivers the deep architectural optimization that
Stonebraker showed matters (178x performance gaps) remains the key open question.
