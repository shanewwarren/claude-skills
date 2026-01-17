# Specifications README Template

Use this template when generating `specs/README.md`.

## Template

```markdown
# {Project Name} Specifications

Design documentation for {brief project description}.

## Overview

This directory contains specifications for the project's features and systems. Each spec describes the design intent, architecture, and implementation guidance for a specific concern.

**Status Legend:**
- **Planned** - Design complete, not yet implemented
- **In Progress** - Currently being implemented
- **Implemented** - Feature complete and in production

---

## {Category Name}

| Spec | Status | Purpose |
|------|--------|---------|
| [{feature-name}.md](./{feature-name}.md) | Planned | {Brief description} |
| [{feature-name}.md](./{feature-name}.md) | Planned | {Brief description} |

## {Another Category}

| Spec | Status | Purpose |
|------|--------|---------|
| [{feature-name}.md](./{feature-name}.md) | Planned | {Brief description} |

---

## Using These Specs

### For Implementers

1. **Read the spec first** before writing code
2. **Check existing code** - specs describe intent, code describes reality
3. **Follow the patterns** outlined in each spec's Architecture section
4. **Update status** when implementation begins/completes

### For Reviewers

1. **Compare against spec** during code review
2. **Flag deviations** that aren't documented
3. **Propose spec updates** when implementation reveals better approaches

### Updating Specs

Specs are living documents. Update them when:
- Implementation reveals a better approach
- Requirements change
- New edge cases are discovered

---

## Related Documentation

- [CLAUDE.md](../CLAUDE.md) - Project-level AI guidance
- [Contributing](../CONTRIBUTING.md) - How to contribute (if exists)
```

## Category Guidelines

### Suggested Categories

Organize specs by domain, not by technical layer:

**Good categories:**
- User Management
- Content System
- Analytics
- Integrations
- Core Infrastructure

**Avoid categories like:**
- Frontend
- Backend
- Database
- API

### When to Create Categories

- 3+ specs naturally group together
- Single category is fine for small projects
- Don't over-categorize (avoid 1-spec categories)

## Linking Strategy

### Relative Links

Always use relative links within specs:
- `[other-spec.md](./other-spec.md)` - sibling spec
- `[../CLAUDE.md](../CLAUDE.md)` - project root file

### Code Links

When referencing implementation:
- `[src/feature/](../src/feature/)` - directory
- `[src/feature/index.ts](../src/feature/index.ts)` - specific file

## Minimal README Example

For simple projects with few specs:

```markdown
# Project Specifications

| Spec | Status | Purpose |
|------|--------|---------|
| [auth.md](./auth.md) | Planned | User authentication and sessions |
| [data-sync.md](./data-sync.md) | Planned | Real-time data synchronization |
| [notifications.md](./notifications.md) | Planned | Push and email notifications |

## Usage

Read the relevant spec before implementing or modifying a feature.
```
