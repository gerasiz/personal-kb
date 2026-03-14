# Papers

Guidance for analyzing academic and technical papers in this knowledge base.

## Default workflow

When asked to **"analyze this paper"**, follow these steps:

1. **Create a folder** under `research/papers/` using the naming convention below.
2. **Store the paper** — save the PDF inside the folder. If the PDF is not available, create a `source.md` file containing the paper title, authors, publication date, and a link to the original.
3. **Create `analysis.md`** — the core deliverable (see template below).
4. **Create `prerequisites.md`** — a glossary of assumed concepts (see template below).
5. **Commit and push** with a message like `research: add paper analysis for <slug>`.

## Folder naming convention

Use the pattern:

```
<first-author-last-name>-<year>-<short-topic-slug>
```

Examples:

```
research/papers/vaswani-2017-attention/
research/papers/brown-2020-language-models/
research/papers/shannon-1948-information-theory/
```

- Lowercase, hyphens as separators.
- Keep the slug short (2–4 words max).
- Use the first author's last name and the publication year.

## Template for `analysis.md`

```markdown
---
title: "<Full paper title>"
authors: [<Author 1>, <Author 2>, ...]
year: <YYYY>
source: "<URL or DOI>"
date_analyzed: <YYYY-MM-DD>
tags: [papers, <topic tags>]
---

# <Paper title>

## Summary

A concise 3–5 sentence summary of the paper: what problem it addresses, what
approach it takes, and what the main results are.

## Key ideas

- **Idea 1** — explanation.
- **Idea 2** — explanation.
- ...

## Method / approach

Describe the methodology or architecture in enough detail to understand the
contribution without re-reading the full paper.

## Results and findings

Highlight the most important quantitative and qualitative results.

## Insights and commentary

Personal observations, connections to other work, open questions, potential
applications, or criticisms.

## Citation

> Authors. "Title." *Venue*, Year. [link](<URL>)
```

## Template for `prerequisites.md`

```markdown
---
title: "Prerequisites — <Paper title>"
---

# Prerequisites

Concepts and background knowledge assumed by the paper. Each entry gives a brief
explanation and a pointer to learn more.

| Concept | Brief explanation | Learn more |
|---------|-------------------|------------|
| Example concept | One-sentence definition or intuition. | [Resource name](URL) |
| ... | ... | ... |
```

Alternatively, if the list is short (fewer than 5 items), you may include it as
an **Appendix** section at the end of `analysis.md` instead of a separate file.

## Tips

- **Always cite the source.** Include a DOI or URL in the front-matter and in the Citation section.
- **Link related work.** If the paper references or builds on other papers already analyzed in this repo, add relative links between them.
- **Keep it concise.** The analysis should let you (or a reader) recall the paper's contribution without re-reading it. Aim for clarity over completeness.
- **Use figures sparingly.** If a key diagram from the paper is essential for understanding, save it in the folder and reference it from the markdown.
