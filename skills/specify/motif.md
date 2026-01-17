# Design Exploration Template (motif.md)

Use this template when generating `motif.md` files for design decisions.

## Template

```markdown
# {Topic}: Design Exploration

Exploring options for {brief description of the decision}. Review the options below and let me know which resonates with your vision.

---

## A. {Option Name}

{2-3 sentence description of this approach}

**How it works:**
{Brief technical explanation}

**References:**
- [{Reference name}]({url}) - {one-line summary}
- Existing pattern in `{file_path}` - {what it demonstrates}

**Pros:**
- {Advantage 1}
- {Advantage 2}

**Cons:**
- {Disadvantage 1}
- {Disadvantage 2}

**Best for:** {Use case where this shines}

---

## B. {Option Name}

{Same structure as Option A}

---

## C. {Option Name}

{Same structure as Option A - include 2-4 options total}

---

## Summary: Combinations

If options can be combined, show a comparison table:

| Combination | Feel | Best For |
|-------------|------|----------|
| A + C | {Description of combined approach} | {Use case} |
| B only | {Description} | {Use case} |

---

## Next Steps

Please review and answer:

1. Which approach(es) appeal to you most?
2. Are there aspects from different options you'd like to combine?
3. Any options you want to explicitly exclude?

Once you decide, I'll incorporate the choice into the specification.
```

## Guidelines for Creating motif.md

### When to Create

- Multiple valid implementation approaches exist
- Trade-offs are significant (performance vs simplicity, flexibility vs convention)
- User preferences or project constraints should guide the decision
- The decision will affect multiple parts of the system

### When NOT to Create

- Clear best practice exists for the project's context
- Decision is easily reversible
- Impact is isolated to a single component
- User has already expressed a preference

### Option Quality

Each option should:
- Be genuinely viable (don't include straw man options)
- Have real pros AND cons (nothing is perfect)
- Include concrete references when possible
- Be distinct from other options (not just minor variations)

### Number of Options

- Minimum: 2 options (otherwise no decision needed)
- Maximum: 4 options (avoid decision paralysis)
- Sweet spot: 3 options (gives range without overwhelming)

### References

Include references from:
1. **Existing codebase** - Patterns already in use
2. **External docs** - Library documentation, best practices
3. **Industry examples** - How other projects solve this

### Tone

- Neutral (don't bias toward one option)
- Concrete (avoid vague descriptions)
- Honest about trade-offs
- Encouraging of user input
