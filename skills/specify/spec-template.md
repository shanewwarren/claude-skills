# Specification Template

Use this template when generating specification files in `specs/{topic}.md`.

## Template

```markdown
# {Feature Name} Specification

**Status:** Planned | In Progress | Implemented
**Version:** 1.0
**Last Updated:** {YYYY-MM-DD}

---

## 1. Overview

### Purpose

{2-3 sentences explaining why this feature exists and what problems it solves}

### Goals

- **{Goal Name}** - {Description of what success looks like}
- **{Goal Name}** - {Description}

### Non-Goals

- **{Non-Goal}** - {Why this is explicitly out of scope}
- **{Non-Goal}** - {Reason}

---

## 2. Architecture

### Component Structure

```
{directory}/
├── {file}.{ext}          # {Description}
├── {file}.{ext}          # {Description}
└── {subdirectory}/
    └── {file}.{ext}      # {Description}
```

### Component Diagram

```
┌─────────────────┐     ┌─────────────────┐
│   {Component}   │────▶│   {Component}   │
└─────────────────┘     └─────────────────┘
         │
         ▼
┌─────────────────┐
│   {Component}   │
└─────────────────┘
```

### Data Flow

{Description of how data moves through the system for this feature}

---

## 3. Core Types

### 3.1 {Type Name}

{Brief description of what this type represents}

```{language}
{type/interface/struct definition}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| {name} | {type} | Yes/No | {Description} |

### 3.2 {Type Name}

{Continue for each significant type}

---

## 4. API / Behaviors

### 4.1 {Operation Name}

**Purpose:** {What this operation does}

| Attribute | Value |
|-----------|-------|
| Method | GET/POST/PUT/DELETE |
| Path | `/api/v1/{resource}` |
| Auth | Required/Optional/None |

**Request:**

```{language}
{request body shape}
```

**Response:**

```{language}
{response body shape}
```

**Errors:**

| Code | Reason |
|------|--------|
| 400 | {When this occurs} |
| 404 | {When this occurs} |

### 4.2 {Operation Name}

{Continue for each API endpoint or behavior}

---

## 5. Database Schema

{Include only if feature requires data persistence}

### Tables

```sql
CREATE TABLE {table_name} (
    id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    {column}    {type} NOT NULL,
    {column}    {type},
    created_at  TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_at  TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_{table}_{column} ON {table_name}({column});
```

### Migrations

| Version | Description |
|---------|-------------|
| 001 | Create {table_name} table |
| 002 | Add {column} column |

---

## 6. Configuration

{Include only if feature has configurable behavior}

| Variable | Type | Description | Default |
|----------|------|-------------|---------|
| `{VAR_NAME}` | string | {Description} | `{value}` |
| `{VAR_NAME}` | number | {Description} | `{value}` |

---

## 7. Security Considerations

### Authentication & Authorization

{How access is controlled}

### Data Protection

{How sensitive data is handled}

### Input Validation

{What validation is performed and why}

---

## 8. Implementation Phases

| Phase | Description | Dependencies | Complexity |
|-------|-------------|--------------|------------|
| 1 | {What's built first} | None | Low/Medium/High |
| 2 | {What comes next} | Phase 1 | Low/Medium/High |
| 3 | {Final phase} | Phase 2 | Low/Medium/High |

---

## 9. Open Questions

{List any unresolved decisions or areas needing clarification}

- [ ] {Question 1}
- [ ] {Question 2}
```

## Section Guidelines

### Which Sections to Include

Not all sections are needed for every spec. Use judgment:

| Section | Include When |
|---------|--------------|
| Overview | Always |
| Architecture | Feature has multiple components |
| Core Types | Feature introduces new data structures |
| API / Behaviors | Feature exposes endpoints or public methods |
| Database Schema | Feature persists data |
| Configuration | Feature has runtime options |
| Security | Feature handles auth, PII, or external access |
| Implementation Phases | Feature is complex enough to stage |
| Open Questions | Decisions are still pending |

### Spec Quality Checklist

Before finalizing a spec, verify:

- [ ] Purpose is clear to someone unfamiliar with the project
- [ ] Goals are measurable or verifiable
- [ ] Non-goals explicitly exclude common misunderstandings
- [ ] Types are complete enough to implement from
- [ ] API contracts are unambiguous
- [ ] Security considerations address OWASP top 10 where applicable
- [ ] Implementation phases form a logical progression

### Language-Specific Type Examples

**TypeScript:**
```typescript
interface User {
  id: string;
  email: string;
  createdAt: Date;
}
```

**Rust:**
```rust
pub struct User {
    pub id: Uuid,
    pub email: String,
    pub created_at: DateTime<Utc>,
}
```

**Go:**
```go
type User struct {
    ID        string    `json:"id"`
    Email     string    `json:"email"`
    CreatedAt time.Time `json:"created_at"`
}
```

**Python:**
```python
@dataclass
class User:
    id: str
    email: str
    created_at: datetime
```

Use the appropriate syntax for the project's primary language.
