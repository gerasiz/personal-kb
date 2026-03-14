# Personal Knowledge Base

A structured personal knowledge base stored as Markdown files.

## Repository Structure

| Directory | Purpose |
|-----------|---------|
| `notes/` | Freeform notes, thoughts, and observations |
| `entities/` | Profiles of people, organizations, projects, and other named entities. One folder per entity. Personal profile lives at `entities/me/`. |
| `research/` | Deep-dive research on specific topics. One folder per topic. |
| `docs/` | Reference documentation, how-tos, and guides |
| `indexes/` | Generated indexes and derived data (git-ignored) |

## Conventions

- Default format is **Markdown** (`.md`).
- Use lowercase filenames and folder names with hyphens as separators (e.g., `my-topic.md`, `acme-corp/`).
- Keep one concept per file; prefer many small files over few large ones.
- Entities and research topics are always folders (never single files). Store multiple markdown documents inside each folder.
- Use YAML front-matter when metadata (date, tags, status) is useful.
- Each directory has its own `CLAUDE.md` with specific guidance.
