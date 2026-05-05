# Extended PRD / spec (full)

Use in **PRD mode** or when the track asks for **full** output in `spec-*`. In PRD mode: **do not interview** — synthesize the conversation + repository state.

Before writing: explore the repo if context is missing; use domain glossary vocabulary and respect applicable ADRs (locations in [workflow-conventions.md](../../reference/workflow-conventions.md)).

## Modules (alignment step)

Sketch modules to create or change; favor **deep modules** (much functionality behind a simple, stable interface). Confirm with the user that the cut makes sense and which modules need explicit tests in the plan.

## Template

### Problem Statement

The problem from the user’s point of view.

### Solution

The solution from the user’s point of view.

### User Stories

Long numbered list. Format:

`N. As <actor>, I want <capability>, so that <benefit>.`

Cover all relevant aspects of the capability.

### Implementation Decisions

Decisions taken, for example:

- Modules to build or change
- Interfaces to expose or change
- Technical clarifications
- Architectural decisions
- Schema, API contracts, interactions

**Do not** include file paths or snippets — they go stale.

### Testing Decisions

- What defines a good test (observable behavior, not implementation detail)
- Which modules will have tests
- Reference similar tests in the repo

### Out of Scope

What is explicitly excluded from this spec.

### Further Notes

Additional notes.

---

## Issue tracker

If the project has tracker integration or triage labels (for example `needs-triage`), publish per that flow. Otherwise deliver Markdown and state the manual next step.
