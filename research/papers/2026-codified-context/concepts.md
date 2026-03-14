---
title: Prerequisite Concepts
paper: 2026-codified-context
---

# Prerequisite Concepts

## Model Context Protocol (MCP)

**What it is:** An open protocol for connecting LLM applications to external data sources and tools via a standardized server interface. Clients send requests; servers expose resources and tools.

**Why it matters here:** The paper's cold-memory tier is served through an MCP server that provides keyword-based retrieval of knowledge base documents. This is how agents access subsystem specifications on demand without loading everything into the context window.

**Learn more:** https://modelcontextprotocol.io/

## Context Window / Context Engineering

**What it is:** The fixed-size token budget available to an LLM in a single interaction. Context engineering is the practice of designing what goes into that window to maximize model performance on a task.

**Why it matters here:** The entire paper is fundamentally about context engineering at project scale. The three-tier architecture (hot/warm/cold) is a strategy for managing what knowledge is always present vs. loaded on demand vs. embedded in specialist agents, all constrained by context window limits.

**Learn more:** https://simonwillison.net/tags/context-engineering/

## Entity Component System (ECS)

**What it is:** An architectural pattern common in game development where entities are IDs, components are pure data, and systems contain logic that operates on entities with specific component combinations. Favors composition over inheritance.

**Why it matters here:** The target codebase uses ECS extensively (45+ systems). Several specialist agents and knowledge base documents are scoped to ECS subsystems, and the architecture's complexity motivates the need for codified context.

**Learn more:** https://en.wikipedia.org/wiki/Entity_component_system

## Deterministic Lockstep Networking

**What it is:** A multiplayer networking model where all clients execute the same simulation given identical inputs. Only inputs are transmitted, not game state. Requires bitwise-deterministic execution across all clients.

**Why it matters here:** The paper's most dramatic case study involves debugging desynchronization bugs in a deterministic networking system. The networking agent's ~915-line specification embeds full determinism theory because partial knowledge risks introducing subtle desyncs.

**Learn more:** https://gafferongames.com/post/deterministic_lockstep/

## Agentic Coding Assistants

**What it is:** LLM-based tools that autonomously write, edit, and debug code through multi-step tool use — going beyond single-turn code completion to execute plans across files and tools.

**Why it matters here:** The paper's infrastructure is built specifically for agentic coding workflows (Claude Code with sub-agents). The 283 tracked sessions included 16,522 autonomous agent turns, showing how agents operate with substantial autonomy within the codified context framework.

**Learn more:** https://www.anthropic.com/products/claude-code

## Multi-Agent Systems

**What it is:** Architectures where multiple specialized AI agents collaborate on tasks, each with defined roles, tools, and knowledge scopes. Contrasts with monolithic single-agent approaches.

**Why it matters here:** The paper deploys 19 specialist agents split by capability tier. The constitution's trigger tables route tasks to appropriate specialists, creating an orchestration layer that encodes institutional knowledge about which expertise each part of the codebase requires.

**Learn more:** https://arxiv.org/abs/2402.01680

## Project Manifests (CLAUDE.md / .cursorrules)

**What it is:** Convention files placed in a repository root that provide project-specific instructions to AI coding assistants. They describe architecture, conventions, and constraints in a format the tool loads automatically.

**Why it matters here:** The paper's constitution is an evolution of this pattern. Related work notes that 72.6% of Claude Code projects specify architecture via CLAUDE.md, but the paper argues single-file manifests don't scale — complex projects need the full three-tier approach.

**Learn more:** https://docs.anthropic.com/en/docs/claude-code/memory
