---
name: to-issues
description: Split a spec into small executable tasks with acceptance criteria and verification. Not for writing the spec — use to-spec.
---

# To Issues

Transversal: [CONVENTIONS.md](../../CONVENTIONS.md).

## When to use

- Spec or plan ready for **Plan phase** tasks.
- User asks to break work into slices.

## When not to use

- No spec yet → `to-spec`
- Coding → `git-workflow-and-versioning` → `tdd`

## Expected input

- Plan, spec, PRD, or parent task (read from path or reference if given).
- Optional: planning layout from `docs/ai-workflow/` or `.scratch/<feature>/`.

## Process

### 1. Gather context

Work from conversation context. If the user passes a path or reference, read that artifact (file, URL, or external issue if an adapter is configured).

### 2. Explore the codebase (optional)

If needed, explore to ground slice titles in domain language and ADRs.

### 3. Draft vertical slices

Break the plan into **tracer bullet** tasks. Each slice is a thin vertical path through all layers end-to-end, not a horizontal layer.

Slices may be **HITL** or **AFK**. Prefer AFK where possible.

<vertical-slice-rules>
- Each slice delivers a narrow but COMPLETE path through every layer
- A completed slice is demoable or verifiable on its own
- Prefer many thin slices over few thick ones
</vertical-slice-rules>

### 4. Quiz the user

Present the breakdown as a numbered list. For each slice:

- **Title**
- **Type**: HITL / AFK
- **Blocked by**: dependencies
- **Acceptance**: which criteria this slice covers

Ask whether granularity, dependencies, and HITL/AFK labels are correct. Iterate until approved.

### 5. Write tasks

For each approved slice, write to the configured planning location. If none exists, use `.scratch/<feature>/issues/<NN>-<slug>.md`.

Apply triage by slice type unless instructed otherwise:

- **AFK** — `ready-for-execution`
- **HITL** — `ready-for-review`

Publish in dependency order so blockers can reference real identifiers.

<issue-template>

## Parent

Reference to parent spec/task if applicable; otherwise omit.

## What to build

Concise end-to-end description of this vertical slice. Avoid file paths unless encoding a non-obvious decision (state machine, schema shape) from a prototype.

## Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2

## Blocked by

- Blocking task reference, or "None — can start immediately"

## Verification

How this slice will be verified (command, test, or check).

</issue-template>

Do not close or rewrite the parent spec unless asked.

## Expected output

- Executable tasks in dependency order with AFK/HITL status when using local markdown.
- User-approved breakdown before write.

## Minimal checklist

- [ ] Vertical slices (not horizontal layers)
- [ ] User approved granularity and dependencies
- [ ] Each task has acceptance criteria and verification
- [ ] Written to planning location
