---
name: improve-codebase-architecture
description: Use when finding refactor or testability opportunities anchored in domain glossary and ADRs (locations in `.cursor/reference/workflow-conventions.md`); when the user wants to improve architecture, consolidate tightly-coupled modules, deepen shallow interfaces, or make a codebase more testable and AI-navigable.
---

# Improve codebase architecture

## Overview

Surface **deepening** opportunities: refactors that increase **leverage** (rich behavior behind small interfaces) and **locality** (changes cluster where maintainers expect). Informed by glossary and ADRs — do not re-litigate recorded decisions without real friction ([workflow-conventions.md](../../reference/workflow-conventions.md)).

**Vocabulary:** use [LANGUAGE.md](LANGUAGE.md) terms consistently (**module**, **interface**, **seam**, **adapter**, **depth**, **shallow**, **leverage**, **locality**).

## When to use

- Exploring refactor paths after repeated bugs or hard-to-test seams.
- Post-debugging follow-up when investigation exposed structural problems.
- User asks for an architecture pass with testability goals.

## When not to use

- **Greenfield MVP** with no code yet — use `brainstorming` / `plan-writing`.
- **Cosmetic style-only** requests — stay in normal editing; don’t force “depth” jargon.

## Principles (compressed)

- **Deletion test:** remove the module mentally — if complexity scatters to many callers, it was earning keep; if it was pass-through, merge or delete.
- **Interface = test surface.**
- **One adapter → hypothetical seam. Two adapters → real seam.**

Full nuance: [LANGUAGE.md](LANGUAGE.md). Execution patterns when deepening: [DEEPENING.md](DEEPENING.md). Alternative interface shapes: [INTERFACE-DESIGN.md](INTERFACE-DESIGN.md).

## Process

### 1. Explore

Read glossary and ADRs for the area first ([workflow-conventions.md](../../reference/workflow-conventions.md)). Then explore the codebase (e.g. Explore subagent) and note friction:

- Understanding one concept requires hopping many small modules?
- **Shallow** modules (interface as complex as implementation)?
- Pure extractions for tests, but real bugs live in call choreography (**locality** lost)?
- Coupling leaks across seams?
- Large untested surfaces or tests that fight the public contract?

Apply the **deletion test** to suspects.

### 2. Present candidates

Numbered list. Each item:

- **Files** involved
- **Problem** (friction in plain language)
- **Solution** (what would change)
- **Benefits** in **locality**, **leverage**, and **test** terms

Use **glossary** words for domain; use **LANGUAGE.md** terms for architecture shape. If an idea **contradicts an ADR**, flag only when friction merits reopening — not every theoretical conflict.

**Do not** propose final interfaces yet. Ask which candidate to drill into.

### 3. Grilling loop

After user picks one: walk constraints, dependencies, seam shape, tests that should survive.

**Side effects as decisions land:**

- New concept missing from glossary → add via [CONTEXT-FORMAT.md](../brainstorming/CONTEXT-FORMAT.md) (lazy file creation).
- Term sharpened → update glossary in place.
- Load-bearing rejection → offer ADR per [ADR-FORMAT.md](../brainstorming/ADR-FORMAT.md) only when future explorers need the guardrail.
- Interface alternatives → [INTERFACE-DESIGN.md](INTERFACE-DESIGN.md).

## Common mistakes

| Mistake | Fix |
|---------|-----|
| Jargon without glossary alignment | Tie suggestions to domain terms |
| Every refactor contradicts some ADR | Only escalate real conflicts |
| Jump to code before candidate pick | Stage 2 is a menu, not a patch |

## Related skills

- **`systematic-debugging`** — often precedes this when bugs expose seams.
- **`tdd-cycle`** — lock behavior while reshaping modules.
- **`brainstorming`** — when the gap is product/spec, not structure.
