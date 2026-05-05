---
name: plan-writing
description: Use when spec or closed requirements exist and a `plan-*` must be materialized as vertical slices before coding; when asked to break a plan into issues/tickets; or at the Plan step of Normal/Complex workflow tracks.
---

# Plan writing

## Overview

Turn agreed specs into **`plan-*`** documents: **vertical slices**, verifiable `done-when`, TDD-shaped tasks when code changes, AFK/HITL typing. Aligned with [task-orchestrator.md](../../agents/task-orchestrator.md) and [workflow-conventions.md](../../reference/workflow-conventions.md).

**Announce at the start:** `Using the plan-writing skill to produce the plan.`

## When to use

- **Prerequisite:** accepted `spec-*` (or equivalent). If still in product discovery → `brainstorming` first.
- **Parallel work** — prefer dedicated worktree per [git-worktrees](../git-worktrees/SKILL.md).

## When not to use

- Spec not accepted or still ambiguous at acceptance level.
- Single trivial change with no slice risk — still may need a one-slice plan; don’t default to “no plan” for Normal/Complex tracks without user call.

## Before tasks — file map

Decide files to touch and each file’s **single primary role** before listing steps.

- Clear boundaries; things that change together stay adjacent.
- Follow existing codebase patterns; no wide reorg unless justified in the plan.

## Document header (required)

```markdown
# [Name] — Implementation plan

> **For agents:** execute **slice by slice**; track status; satisfy `done-when` before advancing.

**Objective:** [one sentence]

**Architecture:** [2–3 sentences]

**Stack:** [key technologies]

**Spec:** [link or path]

---
```

Complex track: add **Rollback** per method/orchestrator norms.

## Per slice (required)

Each slice is **end-to-end value**, not one layer only.

```markdown
### Slice N — <name>
interface-design: <spec sections, ADR, OpenAPI, UI — or minimal contract bullets>
vertical-slice: <end-to-end criterion / user-visible value>
type: AFK | HITL
status: pending | in-progress | done | blocked
done-when:
  - <binary verifiable criterion>
  - <…>
blocked-by: <slice or external dependency; or "none">
```

- **AFK:** no mandatory human gate to implement.
- **HITL:** architecture decision, secrets, manual deploy, mandatory external sign-off, etc.

### Minute tasks (when code changes)

````markdown
#### Tasks

**Files (this slice):**
- Create: `exact/path.ext`
- Change: `existing/path.ext`
- Tests: `path/to/test.ext`

- [ ] **Step 1 — Write failing test**
```…
```
- [ ] **Step 2 — Run test; expect failure**
  - Command: `…`
  - Expected: …
- [ ] **Step 3 — Minimal implementation**
```…
```
- [ ] **Step 4 — Run test; expect pass**
  - Command: `…`
- [ ] **Step 5 — Commit** (HITL: user owns commit/push — [Cross-skill alignment](../../reference/workflow-conventions.md#cross-skill-alignment))
````

## Plan defects — never ship in the doc

- `TBD` / vague placeholders.
- “Handle errors appropriately” / “add validation” without criteria.
- “Write tests for the above” with no test bodies or commands.
- “Same as task N” without restating needed work.
- Types/functions referenced nowhere in earlier tasks.

Prefer **repo-relative paths**, **full commands**, **expected outputs** on verification steps.

## Author self-review

1. **Coverage** — every requirement maps to a slice or explicit out-of-scope.
2. **Placeholder scan** — patterns above.
3. **Consistency** — same names for APIs/types across slices.

Optional: [plan-document-reviewer-prompt.md](plan-document-reviewer-prompt.md).

## Optional — tracker issues (“to-issues”)

1. One issue per vertical slice; **tracer bullet** scope (narrow full stack: schema, API, UI, tests — per project).
2. Fields: Title, **HITL/AFK**, **Blocked by**, user stories if present.
3. **Quiz user** on granularity and dependencies; iterate until approved.
4. Create in dependency order; suggested body:

```markdown
## Parent
<reference if any>

## What to build
<end-to-end behavior>

## Acceptance criteria
- [ ] …

## Blocked by
<link or "None — can start">
```

If no tracker — stay in markdown or ask vocabulary once.

## Handoff

1. State paths of `plan-*` and `spec-*`.
2. **Next:** `task-orchestrator` / developer + `tdd-cycle` where applicable; reviews + `task-delivery`.
3. Ask whether to **publish issues**.

## Principles

DRY, YAGNI, **vertical** slices, binary `done-when`, vocabulary aligned with glossary/ADRs ([workflow-conventions.md](../../reference/workflow-conventions.md)).
