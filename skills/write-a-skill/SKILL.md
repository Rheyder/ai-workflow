---
name: write-a-skill
description: Create new agent skills with proper structure, progressive disclosure, and bundled resources. Use when user wants to create, write, or build a new skill.
---

# Writing Skills

## Process

1. **Gather requirements** - ask user about:
   - What task/domain does the skill cover?
   - What specific use cases should it handle?
   - Does it need executable scripts or just instructions?
   - Any reference materials to include?

2. **Draft the skill** - create:
   - SKILL.md with concise instructions
   - Additional reference files if content exceeds 500 lines
   - Utility scripts if deterministic operations needed

3. **Review with user** - present draft and ask:
   - Does this cover your use cases?
   - Anything missing or unclear?
   - Should any section be more/less detailed?

## Skill Structure

```
skill-name/
├── SKILL.md           # Main instructions (required); link ../../CONVENTIONS.md for transversal rules
├── REFERENCE.md       # Detailed docs (if needed)
├── EXAMPLES.md        # Usage examples (if needed)
└── scripts/           # Utility scripts (if needed)
    └── helper.js
```

## SKILL.md Template

Align with [CONVENTIONS.md § Skill file shape](../../CONVENTIONS.md#skill-file-shape):

```md
---
name: skill-name
description: [One line: what it does]. Use when [trigger]. Not for [anti-trigger].
---

# Skill Name

Transversal: [CONVENTIONS.md](../../CONVENTIONS.md) — link only sections you need.

## When to use

## When not to use

## Expected input

## Process

## Expected output

## Minimal checklist

## Further reading

[REFERENCE.md](REFERENCE.md) — long examples, checklists, scripts
```

## Description Requirements

The description is **the only thing the agent sees** when deciding which skill to load. It is surfaced alongside all other installed skills. The agent reads these descriptions and picks the relevant skill based on the user's request.

**Goal**: Give the executing agent enough info to know:

1. What capability this skill provides
2. When/why to trigger it (specific keywords, contexts, file types)

**Format**:

- Max 1024 chars
- Write in third person
- First sentence: what it does
- Second sentence: "Use when [specific triggers]"

**Good example** — gives the agent a clear trigger:

```
Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when user mentions PDFs, forms, or document extraction.
```

**Bad example** — too vague for skill selection:

```
Helps with documents.
```

## When to Add Scripts

Add utility scripts when:

- Operation is deterministic (validation, formatting)
- Same code would be generated repeatedly
- Errors need explicit handling

Scripts save tokens and improve reliability vs generated code.

## When to Split Files

Split into separate files when:

- SKILL.md exceeds 100 lines
- Content has distinct domains (finance vs sales schemas)
- Advanced features are rarely needed

## Review Checklist

After drafting, verify:

- [ ] Links [CONVENTIONS.md](../../CONVENTIONS.md) for transversal rules instead of duplicating them
- [ ] Description includes triggers ("Use when...")
- [ ] SKILL.md under 100 lines
- [ ] No time-sensitive info
- [ ] Consistent terminology
- [ ] Concrete examples included
- [ ] References one level deep
