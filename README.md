# Claude Code Skills

A collection of custom Claude Code skills I've created and am experimenting with. These skills extend Claude Code's capabilities for specific workflows.

## Skills

| Skill | Description |
|-------|-------------|
| [/specify](#specify) | Specification-driven development through JTBD decomposition |
| [/north-star](#north-star) | One-screen status of a project against its vision: what's built, what's open, what has drifted |

---

## Installation

Copy the skill folder(s) you want to your Claude Code skills directory:

```bash
# Personal skills (available in all projects)
cp -r skills/<skill-name> ~/.claude/skills/

# Or project-specific skills
cp -r skills/<skill-name> .claude/skills/
```

**Quick install for /specify:**
```bash
cp -r skills/specify ~/.claude/skills/
```

**Quick install for /north-star:**
```bash
cp -r skills/north-star ~/.claude/skills/
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

## /north-star

Prints one screen that answers "where are we against the vision?" for the project in the working directory.

### What It Does

After a handful of sessions, it's easy for both you and the agent to lose track of what the project is for and where it stands. `/north-star` rebuilds that picture from the record instead of from memory:

1. **Finds the vision doc.** It follows the docs that `CLAUDE.md` or `AGENTS.md` point to, or looks for files like `VISION.md`, `BET.md` or `ROADMAP.md`.
2. **Lists the goals.** These are the dated or measurable outcomes the doc names, such as milestones and success signals.
3. **Gathers the work.** Built work comes from `git log` on the default branch. Open work comes from open PRs and the issue tracker, through its MCP server or CLI. Planned work is goal scope that nothing has built or picked up, plus unchecked `- [ ]` items in the docs.
4. **Ties each item to a goal.** Work that serves no goal, and goals that nothing is moving, show up as drift.
5. **Prints the screen.** It checks every claim against its source first.

When docs disagree with git or the tracker, the record wins and the stale doc is named. A backlog ticket counts as open but parked, so a goal with only backlog tickets reads as idle.

### No vision doc yet?

If the project has none, `/north-star` asks three questions once (who it's for, what success looks like with dates or numbers, what's out of scope), with draft answers taken from the README and the code. It writes `docs/NORTH_STAR.md` and adds a pointer line to `CLAUDE.md`, so later sessions read the vision before planning work.

### Example Output

```markdown
# Trailcam

Trail-camera owners get their photos sorted by species on their own machine. Success is a
90%-accurate sort of a real memory card by 2026-12-31.

**Now:** G1 is 35 days out and idle, because bundling the local model has no ticket. G2 is moving on ENG-12.

### G1 · First real sort · Nov 15 · 35 days · idle
- **Built:** a folder-per-species sorter and the CLI entry point.
- **Left:** bundling the local model, and a labeled test set.
- **Checklist:** 2 unchecked. The soonest is collecting 500 labeled photos, due Nov 1 (docs/NORTH_STAR.md).

### G2 · Single binary · Jan 31 · 112 days · moving
- **Open:** ENG-12, a release build for Linux and macOS.

### Drift
- G1 is idle. Bundling the local model has no ticket.

*Sources: docs/NORTH_STAR.md · 14 commits since 2026-09-02 · Linear: 1 of 9 open*
```

### Requirements

- A git repository. `gh` is needed for open PRs.
- An issue tracker reachable through an MCP server or CLI, if you want tickets on the screen. Without one, the sources line names the gap.

### Files

```
skills/north-star/
├── SKILL.md       # Main skill instructions
├── INTERVIEW.md   # One-time interview for projects with no vision doc
└── LICENSE.txt    # Apache 2.0 license
```

---

## License

Skills in this repository are licensed under Apache 2.0 unless otherwise noted. See individual skill directories for specific license files.
