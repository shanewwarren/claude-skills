---
name: specify
description: Specification-driven development skill. Guides users through Jobs-to-Be-Done (JTBD) decomposition to generate comprehensive specs. Use when user wants to create specifications, define features, plan a new project, or says /specify.
user-invocable: true
license: Apache-2.0
---

# Specification Generator

Transform Jobs to Be Done (JTBD) into structured specification documents.

## Sub-commands

Parse `$ARGUMENTS` to determine which sub-command to run:

| Command | Description |
|---------|-------------|
| (empty) | Full interactive workflow |
| `init` | Initialize specs/ structure only |
| `decompose` | JTBD decomposition only |
| `research <topic>` | Research specific topic |
| `generate` | Generate specs from existing decomposition |

## When to Use Each Sub-command

### Starting Fresh?
→ Use `/specify` (full workflow)

### Already Have specs/ Directory?
→ Use `/specify init` (structure only)

### Just Need to Break Down a Feature?
→ Use `/specify decompose`

### Need to Research a Specific Area?
→ Use `/specify research <topic>`

### Have Decomposition, Ready to Write Specs?
→ Use `/specify generate`

## Dependencies

This skill works best when the following are available:
- **Context7 MCP** - For external library documentation research
- **Task tool with Explore agent** - For codebase pattern discovery
- **Write/Edit tools** - For generating spec files

## State Management

Track workflow state in `.claude/specify-state.json`:

```json
{
  "jtbd": "string - the job to be done statement",
  "topics": [
    {
      "id": "topic-slug",
      "name": "Topic Name",
      "description": "One sentence description",
      "status": "pending|researched|specified",
      "designDecisions": []
    }
  ],
  "phase": "decompose|research|design|generate",
  "projectType": "detected project type"
}
```

## Phase 1: JTBD Decomposition

### Workflow

1. **Detect project context**
   - Check for existing `specs/` directory
   - Detect project type from package.json, Cargo.toml, go.mod, etc.
   - If specs exist, offer to extend or start fresh

2. **Capture JTBD**
   - Ask: "What job are your users trying to accomplish?"
   - Focus on outcome, not solution
   - Example: "Users need to track personal finances to understand spending"

3. **Decompose into topics**
   - Break JTBD into 3-7 distinct topics
   - Each topic must pass the "One Sentence Without 'And'" test
   - Topics should be 5-15 words describing a single concern

4. **Validate topics**
   - No semantic overlap between topics
   - Each topic is actionable (uses verbs like: track, display, manage, validate, sync)
   - Topics are bounded and testable

5. **Present for refinement**
   - Show topics in a table with status
   - Ask clarifying questions about ambiguous areas
   - Allow user to accept, modify, split, or merge topics

### Example Decomposition

```
JTBD: "Users need to track personal finances to understand spending"

Topics:
1. transaction-tracking - Recording income and expense transactions
2. category-management - Organizing transactions into categories
3. budget-system - Setting and tracking spending limits
4. reporting-analytics - Generating insights and visualizations
5. data-import - Importing data from bank statements
```

## Phase 2: Research

For each topic, spawn an Explore subagent to gather:

1. **Codebase patterns**
   - Existing similar features
   - Relevant types, traits, modules
   - Architectural patterns in use

2. **External research**
   - Best practices for the domain
   - API patterns if applicable
   - Use Context7 for library documentation

3. **Design decisions**
   - Identify architectural choices needed
   - Prepare options with trade-offs
   - Flag decisions requiring user input

### Research Output

Update state with per-topic research:

```json
{
  "id": "topic-slug",
  "research": {
    "existingPatterns": ["pattern descriptions"],
    "externalReferences": ["urls or summaries"],
    "designDecisions": [
      {
        "question": "How should X be implemented?",
        "options": ["Option A", "Option B"],
        "tradeoffs": "A is simpler, B is more flexible"
      }
    ],
    "dependencies": ["other-topic-id"],
    "complexity": "low|medium|high"
  }
}
```

## Phase 3: Design Decisions (motif.md)

When significant design decisions exist, generate `motif.md` exploration document.

See [motif.md](motif.md) for the template.

### When to Generate motif.md

- Multiple valid architectural approaches exist
- User preferences matter for the outcome
- Trade-offs are significant and not obvious

### Workflow

1. Create `motif.md` in project root
2. Present options with pros/cons and references
3. Ask user to review and choose
4. Record decisions in state

## Phase 4: Spec Generation

Generate specification files using templates.

See [spec.md](spec.md) for the full template.

### Files to Create

1. **`specs/README.md`** - Index of all specifications
2. **`specs/{topic}.md`** - One file per topic
3. **Update `CLAUDE.md`** - Add Specifications section if missing

### Spec File Sections

Each spec should include (as applicable):
- Overview (purpose, goals, non-goals)
- Architecture (component structure, diagrams)
- Core Types (with field tables)
- API / Behaviors
- Database Schema (if applicable)
- Configuration
- Security Considerations
- Implementation Phases

## Phase 5: CLAUDE.md Update

If `CLAUDE.md` exists but lacks a Specifications section, inject:

```markdown
## Specifications

**IMPORTANT:** Before implementing any feature, consult `specs/README.md`.

- **Assume NOT implemented.** Specs describe intent; code describes reality.
- **Check the codebase first.** Search actual code before concluding.
- **Use specs as guidance.** Follow design patterns in relevant spec.
- **Spec index:** `specs/README.md` lists all specs by category.
```

### Merge Logic

1. Check if `CLAUDE.md` exists
2. Check if `## Specifications` section exists
3. If missing, insert after first heading or at document start
4. Preserve all existing content

## User Interaction Pattern

### Full Workflow Example

```
User: /specify

Claude: I'll help create specifications for your project.

**Project Detection:**
- Type: TypeScript/Node.js
- Existing specs: No

**Step 1: Define Your Job to Be Done**
What problem are you solving for your users? (Focus on outcome, not solution)

---

User: Users need to manage their reading list and track books they've read

Claude: **Topics Identified:**

| # | Topic | Description | Status |
|---|-------|-------------|--------|
| 1 | book-catalog | Storing and retrieving book information | Ready |
| 2 | reading-status | Tracking read/unread/in-progress state | Ready |
| 3 | list-management | Creating and organizing reading lists | Ready |
| 4 | reading-progress | Recording pages read and notes | Ready |

**Questions:**
1. Should users be able to share lists with others?
2. Do you need integration with book databases (OpenLibrary, Google Books)?

---

User: Yes to sharing, no external integrations for now

Claude: [Updates topics, spawns research subagents]

**Research Complete. Design Decision Required:**

Created `motif.md` for sharing architecture. Please review and let me know which approach you prefer.

---

User: I like Option B - simple link sharing

Claude: Generating specifications...

**Created:**
- specs/README.md
- specs/book-catalog.md
- specs/reading-status.md
- specs/list-management.md
- specs/reading-progress.md
- specs/sharing.md
- Updated CLAUDE.md

Ready for implementation planning.
```

### Example Output

#### Generated `specs/README.md`

```markdown
# Reading Tracker Specifications

Design documentation for a personal reading list management application.

## Overview

This directory contains specifications for the project's features and systems.

**Status Legend:**
- **Planned** - Design complete, not yet implemented
- **In Progress** - Currently being implemented
- **Implemented** - Feature complete and in production

---

## Core Features

| Spec | Status | Purpose |
|------|--------|---------|
| [book-catalog.md](./book-catalog.md) | Planned | Storing and retrieving book information |
| [reading-status.md](./reading-status.md) | Planned | Tracking read/unread/in-progress state |
| [list-management.md](./list-management.md) | Planned | Creating and organizing reading lists |
| [reading-progress.md](./reading-progress.md) | Planned | Recording pages read and notes |

## Social Features

| Spec | Status | Purpose |
|------|--------|---------|
| [sharing.md](./sharing.md) | Planned | Link-based list sharing with others |
```

#### Generated `specs/book-catalog.md` (excerpt)

```markdown
# Book Catalog Specification

**Status:** Planned
**Version:** 1.0
**Last Updated:** 2025-01-15

---

## 1. Overview

### Purpose

The book catalog provides the core data model for storing and retrieving book information. It serves as the foundation for all other features in the reading tracker.

### Goals

- **Complete book metadata** - Store title, author, ISBN, cover image, and page count
- **Fast lookup** - Support search by title, author, or ISBN
- **Extensible schema** - Allow custom fields for user-defined metadata

### Non-Goals

- **External sync** - No integration with Goodreads or other services (deferred)
- **Social features** - Book recommendations handled in separate spec

---

## 2. Architecture

### Component Structure

src/
├── models/
│   └── book.ts          # Book entity and types
├── repositories/
│   └── book-repo.ts     # Data access layer
└── services/
    └── book-service.ts  # Business logic
```

## Init Sub-command

When `$ARGUMENTS` is `init`:

1. Create `specs/` directory if not exists
2. Create `specs/README.md` with basic structure
3. Update `CLAUDE.md` with Specifications section
4. Do NOT run full workflow

## Decompose Sub-command

When `$ARGUMENTS` is `decompose`:

1. Run Phase 1 only
2. Save state to `.claude/specify-state.json`
3. Do NOT proceed to research or generation

## Research Sub-command

When `$ARGUMENTS` starts with `research`:

1. Load state from `.claude/specify-state.json`
2. Research the specified topic (or all if none specified)
3. Update state with research results
4. Do NOT generate specs

## Generate Sub-command

When `$ARGUMENTS` is `generate`:

1. Load state from `.claude/specify-state.json`
2. Require completed decomposition
3. Run Phase 4 and 5 only
4. Generate all spec files

## Template Usage

**MANDATORY**: Before generating specs, read the template files:
- `motif.md` - For design exploration documents
- `spec.md` - For specification files
- `readme.md` - For the specs index

Follow these templates exactly for consistent output.

## Verification

After running /specify, verify success by checking:

1. **Spec files exist**: `ls specs/*.md`
2. **README links work**: All links in `specs/README.md` resolve
3. **CLAUDE.md updated**: Specifications section present
4. **State cleaned**: `.claude/specify-state.json` can be deleted if workflow complete

## Quick Reference

| Phase | Output | Key Files |
|-------|--------|-----------|
| Decompose | Topics list | `.claude/specify-state.json` |
| Research | Design decisions | state.json updated |
| Design | Architecture choices | `motif.md` (if needed) |
| Generate | Specifications | `specs/*.md`, `specs/README.md` |
| Finalize | CLAUDE.md update | `CLAUDE.md` |
