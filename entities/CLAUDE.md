# Entities

Profiles of people, organizations, projects, tools, and other named entities.

## What belongs here

- People you interact with (colleagues, contacts)
- Companies and organizations
- Projects and products
- Tools, services, or platforms worth tracking
- **Your own profile** lives in `entities/me/`

## Organization

- **One folder per entity.** Never store an entity as a single loose markdown file.
  Each entity gets its own directory under `entities/`, named after the entity.
- Inside each entity folder, store one or more markdown files covering different
  aspects (e.g., `overview.md`, `interactions.md`, `notes.md`).
- Start the primary file with a `# Name` heading.
- Include context on how/why this entity is relevant to you.

## Naming conventions

- Folder names: lowercase, hyphens as separators (e.g., `acme-corp/`, `jane-doe/`).
- File names inside folders: lowercase, hyphens, descriptive (e.g., `overview.md`, `meeting-notes.md`).

## Examples

```
entities/
  acme-corp/
    overview.md
    contracts.md
  jane-doe/
    overview.md
    interactions.md
  me/
    bio.md
    goals.md
    values.md
```
