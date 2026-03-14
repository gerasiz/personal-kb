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

## Workflow

**Always run `git pull` before making any changes.** The user may edit files in parallel, so pulling first avoids conflicts.

1. **Pull** — `git pull` to get the latest changes.
2. **Edit** — make your changes.
3. **Review** — check the diff with `git diff` before committing.
4. **Commit** — commit with a clear, descriptive message.
5. **Push** — `git push` to publish.

If `git pull` reports merge conflicts, resolve them before proceeding: edit the conflicted files, remove the conflict markers, stage the resolved files, and commit the merge.

## Conventions

- Default format is **Markdown** (`.md`).
- Use lowercase filenames and folder names with hyphens as separators (e.g., `my-topic.md`, `acme-corp/`).
- Keep one concept per file; prefer many small files over few large ones.
- Entities and research topics are always folders (never single files). Store multiple markdown documents inside each folder.
- Use YAML front-matter when metadata (date, tags, status) is useful.
- Each directory has its own `CLAUDE.md` with specific guidance.
