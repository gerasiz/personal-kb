# Papers

Analyzed academic and technical papers. Each paper lives in its own subfolder.

## Default workflow

When asked to **"analyze this paper"**, follow these steps:

1. **Create a folder** under `research/papers/` using the naming convention below.
2. **Store the paper** — place the PDF in the folder. If the PDF is not available, create a `source.md` file containing the paper's title, authors, publication venue, date, and a link to the original source.
3. **Create `overview.md`** — summary, key ideas, and insights (see template below).
4. **Create `concepts.md`** — prerequisite/assumed concepts with brief explanations and links to learn more (see template below).
5. **Commit and push** with a message like `research: add paper analysis for <slug>`.

## Folder naming convention

Use the pattern **`YYYY-short-title/`**, where:

- `YYYY` is the publication year.
- `short-title` is a 2–4 word lowercase slug derived from the paper title, separated by hyphens.

Examples:

```
research/papers/
  2017-attention-is-all-you-need/
  2020-gpt3-language-models/
  2023-llm-agents-survey/
```

## Folder structure

```
research/papers/YYYY-short-title/
  paper.pdf              # the PDF (or source.md if unavailable)
  overview.md            # summary + key ideas + insights
  concepts.md            # prerequisite concepts and definitions
  # optional additional files:
  #   notes.md           — personal reading notes, questions, open threads
  #   figures/           — extracted or redrawn figures
```

## Template for `overview.md`

```markdown
---
title: "Full Paper Title"
authors: [Author One, Author Two]
year: YYYY
venue: "Conference / Journal / arXiv"
url: "https://..."
date_analyzed: YYYY-MM-DD
tags: [papers, topic-a, topic-b]
---

# Full Paper Title

## Summary

A 3–5 sentence summary of what the paper does, why it matters, and the main result.

## Key Ideas

- **Idea 1** — brief explanation.
- **Idea 2** — brief explanation.
- ...

## Method / Approach

Describe the core method, architecture, or technique introduced by the paper.

## Evidence / Results (high level)

Brief statement of whether the approach works and in what setting.
Do not enumerate metrics or benchmark tables — just the takeaway.

## Insights & Takeaways

Personal observations: what is novel, what is useful, what are the limitations,
and how this connects to other work or projects.

## Citation

> Authors. "Title." *Venue*, Year. [link](URL)
```

## Template for `concepts.md`

```markdown
---
title: "Concepts — Full Paper Title"
paper: YYYY-short-title
---

# Prerequisite Concepts

Concepts and background knowledge assumed by the paper. Each entry gives a brief
explanation and a pointer to learn more.

## Concept Name

**What it is:** One or two sentence explanation.

**Why it matters here:** How the paper uses or builds on this concept.

**Learn more:** [Resource title](https://...)

## Another Concept

...
```

Alternatively, if the list is short (fewer than 5 items), you may include it as
an **Appendix** section at the end of `overview.md` instead of a separate file.

## Analysis focus

- **Central ideas & novel contributions first.** Prioritize what the paper introduces that is new and why it matters.
- **Practical utility.** Highlight what a practitioner can take away and apply.
- **Skip metric deep-dives.** Do not describe evaluation metrics, benchmarks, or numeric results in detail unless the user explicitly asks. A high-level statement of evidence (e.g., "outperforms prior work on X" or "scales to Y") is sufficient.

## Tips

- **Always cite the source.** Include a DOI or URL in the front-matter and in the Citation section of `overview.md`.
- **Link related work.** If the paper references or builds on other papers already analyzed in this repo, add relative links between them.
- **Keep it concise.** The analysis should let you (or a reader) recall the paper's contribution without re-reading it. Aim for clarity over completeness.
- **Write in your own words.** Avoid copying large blocks of text from the paper.
- **Tag papers** in the YAML front-matter to enable future search and grouping.
- **Use figures sparingly.** If a key diagram from the paper is essential for understanding, save it in the folder and reference it from the markdown.
