---
title: "How AI Impacts Skill Formation"
authors: [Judy Hanwen Shen, Alex Tamkin]
year: 2026
venue: "arXiv preprint"
url: "https://arxiv.org/abs/2601.20245"
date_analyzed: 2026-03-21
tags: [papers, ai-assistance, skill-formation, cognitive-offloading, software-engineering, learning, human-ai-interaction]
---

# How AI Impacts Skill Formation

## Summary

This paper investigates whether AI coding assistance impairs the development of new programming skills. Through a randomized experiment (n=52) where developers learned the Python Trio async library with or without AI help, the authors find that AI use significantly reduces conceptual understanding, code reading, and debugging abilities (17% lower quiz scores) without delivering meaningful speed gains. A qualitative analysis of screen recordings identifies six distinct AI interaction patterns, three of which preserve learning outcomes by keeping the developer cognitively engaged.

## Key Ideas

- **Productivity-learning tradeoff** — AI assistance lets workers complete unfamiliar tasks without actually acquiring the underlying skills. The paper frames this as a shortcut that bypasses the learning stage: novices reach task completion but miss the conceptual development that comes from struggling with errors independently.

- **No significant speed gain** — Contrary to expectations, AI-assisted participants were not meaningfully faster (p=0.391). Much of the potential time savings was consumed by formulating queries and interpreting AI responses — some participants spent over 30% of task time just interacting with the assistant.

- **Six AI interaction patterns** — Through manual annotation of all 51 screen recordings, the authors categorize AI usage into six behavioral clusters along two axes: cognitive engagement and AI reliance.
  - *Low-scoring patterns* (heavy delegation, low engagement): **AI Delegation** (19%), **Progressive AI Reliance** (35%), **Iterative AI Debugging** (24%).
  - *High-scoring patterns* (active thinking, selective AI use): **Conceptual Inquiry** (65%), **Hybrid Code-Explanation** (68%), **Generation-Then-Comprehension** (86%).

- **Debugging is the most impacted skill** — The largest gap between AI and non-AI groups appeared in debugging questions. The control group encountered and resolved more errors independently, which directly built debugging competence.

- **Error exposure drives learning** — Control participants hit more errors (both syntax and library-specific) and the process of resolving them was a primary mechanism for skill acquisition. AI-assisted participants were shielded from this productive struggle.

## Method / Approach

A between-subjects randomized experiment using the Python Trio library (chosen for being unfamiliar to participants and requiring genuinely new concepts like structured concurrency). Participants completed two coding tasks within 35 minutes — the treatment group had access to a GPT-4o-based chat assistant, the control group did not. Both groups then took a 14-question quiz (no AI allowed) covering debugging, code reading, and conceptual understanding. Four pilot studies (n=186 total) refined the design, addressing non-compliance, local item dependence, and confounding variables.

## Evidence / Results (high level)

AI-assisted participants scored significantly lower on the post-task knowledge quiz (Cohen's d=0.738, p=0.01) — a 17% reduction translating to roughly two grade points on a 27-point quiz. Task completion time showed no significant difference between groups. The learning deficit held across all experience levels (1–3 years through 7+ years). The three high-scoring interaction patterns all shared a common trait: participants maintained cognitive engagement rather than delegating thinking to the AI.

## Insights & Takeaways

- **Not all AI use is equal.** The six interaction patterns show that *how* you use AI matters enormously. Asking conceptual questions or requesting explanations of generated code preserves learning; asking the AI to generate and debug code wholesale does not.

- **Implications for onboarding and training.** Organizations that encourage AI use during skill acquisition (e.g., onboarding new engineers) may be inadvertently undermining long-term competence. This is especially concerning in safety-critical domains where human oversight of AI-generated code requires deep understanding.

- **The "AI interaction tax."** The absence of speed gains suggests that interacting with AI is not free — composing good queries and interpreting responses takes real time, partially or fully offsetting code generation speed.

- **Designing for learning-preserving AI use.** The high-scoring patterns suggest concrete design guidelines: AI tools could nudge users toward conceptual questions, require engagement with explanations before providing code, or flag when usage patterns shift toward full delegation.

- **Limitations.** The study is small (n=52), uses a single library (Trio) and a single AI model (GPT-4o), and measures short-term skill formation only. The tasks are relatively constrained compared to real-world engineering work. Long-term effects and transfer to other domains remain open questions.

## Citation

> Shen, J. H. & Tamkin, A. "How AI Impacts Skill Formation." *arXiv preprint arXiv:2601.20245*, 2026. [link](https://arxiv.org/abs/2601.20245)
