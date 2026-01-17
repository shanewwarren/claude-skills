# Claude Code Skills

A collection of custom Claude Code skills I've created and am experimenting with. These skills extend Claude Code's capabilities for specific workflows.

## Skills

| Skill | Description |
|-------|-------------|
| [/specify](#specify) | Specification-driven development through JTBD decomposition |

---

## Installation

Copy the skill folder(s) you want to your Claude Code skills directory:

```bash
# Personal skills (available in all projects)
cp -r skills/<skill-name> ~/.claude/skills/

# Or project-specific skills
cp -r skills/<skill-name> .claude/skills/
```

Restart Claude Code after installation.

---

## /specify

Transforms Jobs-to-Be-Done (JTBD) into structured specification documents through guided conversation.

### What It Does

`/specify` helps you create comprehensive technical specifications before writing code. Instead of diving straight into implementation, it guides you through:

1. **Defining the job** - What are users trying to accomplish?
2. **Decomposing into topics** - Breaking the job into distinct, manageable concerns
3. **Researching patterns** - Finding existing code patterns and best practices
4. **Making design decisions** - Exploring architectural options when trade-offs exist
5. **Generating specs** - Creating detailed specification documents

The result is a `specs/` directory with structured documentation that serves as a blueprint for implementation.

### Sub-commands

| Command | Description |
|---------|-------------|
| `/specify` | Full interactive workflow |
| `/specify init` | Initialize `specs/` structure only |
| `/specify decompose` | JTBD decomposition without research or generation |
| `/specify research <topic>` | Research a specific topic |
| `/specify generate` | Generate specs from existing decomposition state |

### When to Use Each

- **Starting fresh?** → Use `/specify` (full workflow)
- **Already have specs/ directory?** → Use `/specify init` (structure only)
- **Just need to break down a feature?** → Use `/specify decompose`
- **Need to research a specific area?** → Use `/specify research <topic>`
- **Have decomposition, ready to write specs?** → Use `/specify generate`

### Example Session

```
You: /specify

Claude: I'll help create specifications for your project.

**Project Detection:**
- Type: TypeScript/Node.js
- Existing specs: No

**Step 1: Define Your Job to Be Done**
What problem are you solving for your users?

---

You: Users need to manage their reading list and track books they've read

Claude: **Topics Identified:**

| # | Topic | Description |
|---|-------|-------------|
| 1 | book-catalog | Storing and retrieving book information |
| 2 | reading-status | Tracking read/unread/in-progress state |
| 3 | list-management | Creating and organizing reading lists |
| 4 | reading-progress | Recording pages read and notes |

**Questions:**
1. Should users be able to share lists with others?
2. Do you need integration with book databases?

---

You: Yes to sharing, no external integrations

Claude: Generating specifications...

**Created:**
- specs/README.md
- specs/book-catalog.md
- specs/reading-status.md
- specs/list-management.md
- specs/reading-progress.md
- specs/sharing.md
- Updated CLAUDE.md
```

### Output Structure

After running `/specify`, your project will have:

```
your-project/
├── CLAUDE.md              # Updated with Specifications section
├── motif.md               # Design exploration (when needed)
├── .claude/
│   └── specify-state.json # Workflow state
└── specs/
    ├── README.md          # Index of all specifications
    ├── topic-one.md       # Individual spec per topic
    ├── topic-two.md
    └── ...
```

### Spec File Contents

Each spec file includes (as applicable):

- **Overview** - Purpose, goals, non-goals
- **Architecture** - Component structure and diagrams
- **Core Types** - Data structures with field descriptions
- **API / Behaviors** - Endpoints or public methods
- **Database Schema** - Tables and migrations
- **Configuration** - Environment variables and options
- **Security Considerations** - Auth, validation, data protection
- **Implementation Phases** - Suggested build order

### Design Decisions (motif.md)

When significant architectural choices exist, `/specify` generates a `motif.md` file presenting options:

```markdown
# Sharing: Design Exploration

## A. Simple Link Sharing
Share via unique URLs...

## B. User Accounts with Permissions
Full access control...

## Summary
| Option | Best For |
|--------|----------|
| A | MVP, simple use cases |
| B | Teams, enterprise |
```

You review the options and tell Claude your preference before specs are generated.

### Files

```
skills/specify/
├── SKILL.md       # Main skill instructions
├── LICENSE.txt    # Apache 2.0 license
├── motif.md       # Design decision document template
├── readme.md      # Specs index template
└── spec.md        # Individual spec file template
```

---

## License

Skills in this repository are licensed under Apache 2.0 unless otherwise noted. See individual skill directories for specific license files.
