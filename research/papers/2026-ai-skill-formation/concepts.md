---
title: "Concepts — How AI Impacts Skill Formation"
paper: 2026-ai-skill-formation
---

# Prerequisite Concepts

Concepts and background knowledge assumed by the paper. Each entry gives a brief
explanation and a pointer to learn more.

## Cognitive Offloading

**What it is:** The use of external tools or aids to reduce the mental effort required for a task — for example, using a calculator instead of doing arithmetic mentally, or relying on GPS instead of learning a route. In the AI context, it refers to delegating thinking (problem decomposition, debugging, conceptual reasoning) to an AI assistant.

**Why it matters here:** The paper's central finding is essentially about cognitive offloading: when developers delegate reasoning to AI, they bypass the cognitive processes that produce learning. The interaction patterns that preserve learning are precisely those that resist full offloading.

**Learn more:** [Risko & Gilbert, 2016 — Cognitive Offloading](https://doi.org/10.1016/j.tics.2016.07.002)

## Experiential Learning / Problem-Based Learning

**What it is:** Educational frameworks (Kolb's experiential learning cycle, Problem-Based Learning) that emphasize learning through doing — encountering problems, making errors, reflecting on them, and building understanding through direct experience rather than passive instruction.

**Why it matters here:** The paper's theoretical framework treats coding tasks as experiential learning opportunities. The control group follows a natural experiential cycle (encounter problem → make errors → debug → learn), while AI assistance shortcuts this cycle by resolving errors before the learner engages with them.

**Learn more:** [Kolb, 2014 — Experiential Learning](https://www.google.com/books/edition/Experiential_Learning/jpbeBQAAQBAJ)

## Structured Concurrency (Trio)

**What it is:** A programming paradigm for concurrent code where the lifetimes of concurrent tasks are bounded by lexical scopes (e.g., a `nursery` block in Trio). All child tasks must complete before the parent scope exits, making control flow predictable and error handling straightforward compared to traditional async/await patterns.

**Why it matters here:** Trio's structured concurrency model was chosen as the experimental task because it introduces genuinely new concepts (nurseries, cancellation scopes, memory channels) that even experienced Python developers are unlikely to know — making it a clean test of skill acquisition.

**Learn more:** [Trio documentation](https://trio.readthedocs.io/en/stable/) · [Notes on structured concurrency (Njs)](https://vorpus.org/blog/notes-on-structured-concurrency-or-go-statement-considered-harmful/)

## Between-Subjects Experimental Design

**What it is:** A research design where different participants are assigned to different conditions (here, AI vs. no-AI), as opposed to within-subjects designs where the same participants experience all conditions. This avoids carryover effects but requires careful balancing of participant characteristics across groups.

**Why it matters here:** The study uses this design to isolate the causal effect of AI assistance on learning. Participants were balanced across coding experience, Python familiarity, and async programming knowledge. The pre-registered design and power analysis (based on pilot studies) strengthen the causal claims.

**Learn more:** [Research Methods Knowledge Base — Between-Subjects](https://conjointly.com/kb/between-subjects-and-within-subjects/)
