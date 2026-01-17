# /specify - Specification-Driven Development Skill

A Claude Code skill that transforms Jobs-to-Be-Done (JTBD) into structured specification documents through guided conversation.

## What is /specify?

`/specify` helps you create comprehensive technical specifications before writing code. Instead of diving straight into implementation, it guides you through:

1. **Defining the job** - What are users trying to accomplish?
2. **Decomposing into topics** - Breaking the job into distinct, manageable concerns
3. **Researching patterns** - Finding existing code patterns and best practices
4. **Making design decisions** - Exploring architectural options when trade-offs exist
5. **Generating specs** - Creating detailed specification documents

The result is a `specs/` directory with structured documentation that serves as a blueprint for implementation.

## Installation

Copy the `skills/specify` folder to your Claude Code skills directory:

```bash
# Personal skills (available in all projects)
cp -r skills/specify ~/.claude/skills/

# Or project-specific skills
cp -r skills/specify .claude/skills/
```

Restart Claude Code after installation.

## Usage

### Full Workflow

Run the complete specification workflow:

```
/specify
```

Claude will guide you through an interactive conversation:

1. **Project Detection** - Identifies your project type and existing specs
2. **JTBD Capture** - Asks what job your users are trying to accomplish
3. **Topic Decomposition** - Breaks the job into 3-7 distinct topics
4. **Research** - Analyzes codebase patterns and external best practices
5. **Design Decisions** - Creates `motif.md` when architectural choices are needed
6. **Spec Generation** - Outputs `specs/README.md` and individual spec files

### Sub-commands

| Command | Description |
|---------|-------------|
| `/specify` | Full interactive workflow |
| `/specify init` | Initialize `specs/` structure only |
| `/specify decompose` | JTBD decomposition without research or generation |
| `/specify research <topic>` | Research a specific topic |
| `/specify generate` | Generate specs from existing decomposition state |

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

## Output Structure

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

## Topic Decomposition Rules

Topics are validated against these criteria:

- **One Sentence Without 'And'** - Each topic must be describable in a single sentence without using "and"
- **5-15 Words** - Concise but descriptive
- **No Overlap** - Topics should be semantically distinct
- **Actionable** - Uses verbs like track, display, manage, validate, sync

## Design Decisions (motif.md)

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

## Philosophy

`/specify` implements Phase 1 of specification-driven development:

1. **SPECIFYING** (this skill) - Transform JTBD into structured specs
2. **PLANNING** - Generate implementation tasks from specs
3. **BUILDING** - Execute the implementation plan

Specs describe *intent* and *design*, not implementation details. They provide structure for planning while preserving implementation flexibility.

## Files Included

```
skills/specify/
├── SKILL.md              # Main skill instructions
├── motif-template.md     # Design decision document template
├── readme-template.md    # Specs index template
└── spec-template.md      # Individual spec file template
```

## License

MIT
