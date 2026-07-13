---
name: setup-workflow
description: Bootstrap neutral workflow docs for a repository. Optional; not required for normal execution. Invoke by name. Toolbox.
disable-model-invocation: true
---

# Setup Workflow

**Optional bootstrap.** The Core Workflow works without this skill.

Use only when the target repository lacks neutral workflow docs (`docs/ai-workflow/`, `AI_WORKFLOW.md`) or needs a refresh after package updates.

Invoke by name when auto-invocation is off in your host.

## When to use

- First-time neutral setup in a target repo.
- Refresh `docs/ai-workflow/` after package updates.

## When not to use

- Mid-slice implementation — use Core skills directly.
- Repo already has planning location and workflow docs configured.

## What it scaffolds (optional)

- **Planning location** — default local markdown under `.scratch/`
- **Triage labels** — `ready-for-execution`, `ready-for-review`, etc.
- **Domain docs** — `CONTEXT.md`, ADRs
- **Workflow copies** — eight phases, contracts, pipeline reference, DoD, context policy

Prompt-driven: explore, present findings, confirm one section at a time, then write.

## Process

### 1. Explore

Read whatever exists; do not assume:

- `AI_WORKFLOW.md` — existing workflow index block?
- `docs/ai-workflow/` — prior bootstrap output
- Optional host adapters: `AGENTS.md`, `CLAUDE.md` (see [ADAPTERS.md](../../ADAPTERS.md)) — do not require them
- `CONTEXT.md`, `CONTEXT-MAP.md`, `docs/adr/`
- `.scratch/` — existing feature folders

### 2. Present findings and ask

Summarise present vs missing. Walk **one section at a time**.

**Section A — Planning location (default: local markdown)**

Specs and tasks under `.scratch/<feature-slug>/` unless the user configures another path. Seed from [issue-tracker.md](./issue-tracker.md).

Confirm:

- Existing `.scratch/` conventions to preserve
- Default feature slug for current work (if known)

**Section B — Triage vocabulary**

Used when writing from `to-spec` / `to-issues`:

- `needs-triage`, `needs-info`
- `ready-for-execution` — AFK default (`to-spec`)
- `ready-for-review` — HITL default (`to-issues`)
- `wontfix`

Defaults: [triage-labels.md](./triage-labels.md).

**Section C — Domain docs**

`improve-codebase-architecture`, `diagnose`, `tdd` may consume glossary + ADRs.

- **Single-context** — `CONTEXT.md` + `docs/adr/`
- **Multi-context** — `CONTEXT-MAP.md` + per-context `CONTEXT.md`

**Section D — Workflow docs**

Present [WORKFLOW.md](../../WORKFLOW.md) and [workflow/pipeline-reference.md](../workflow/pipeline-reference.md). Confirm Toolbox skills for this repo.

### 3. Confirm and edit

Draft for user edit:

- `AI_WORKFLOW.md` index block (or update existing)
- `docs/ai-workflow/issue-tracker.md`
- `docs/ai-workflow/triage-labels.md`
- `docs/ai-workflow/domain.md`
- `docs/ai-workflow/workflow.md`
- `docs/ai-workflow/contracts.md`
- `docs/ai-workflow/pipeline-reference.md`
- `docs/ai-workflow/definition-of-done.md`
- `docs/ai-workflow/context-policy.md`
- `docs/ai-workflow/conventions.md`

Optional: mirror the index block into a host adapter file (`AGENTS.md`, `CLAUDE.md`) only if the user wants it.

### 4. Write

**Primary file:** create or update `AI_WORKFLOW.md` at repo root.

If the user requests a host adapter, copy the same index block there — never duplicate conflicting blocks.

**Index block template:**

```markdown
## AI Workflow

### Planning

Local markdown under `.scratch/` unless configured otherwise. See `docs/ai-workflow/issue-tracker.md`.

### Triage labels

[summary]. See `docs/ai-workflow/triage-labels.md`.

### Domain docs

[summary]. See `docs/ai-workflow/domain.md`.

### Workflow

[summary]. See `docs/ai-workflow/workflow.md`, `contracts.md`, `pipeline-reference.md`.

### Definition of Done

[summary]. See `docs/ai-workflow/definition-of-done.md`.

### Context policy

[summary]. See `docs/ai-workflow/context-policy.md`.

### Conventions

[summary]. See `docs/ai-workflow/conventions.md`.
```

**Seed files** (verbatim at write time):

- [issue-tracker.md](./issue-tracker.md) → `docs/ai-workflow/issue-tracker.md`
- [triage-labels.md](./triage-labels.md) → `docs/ai-workflow/triage-labels.md`
- [domain.md](./domain.md) → `docs/ai-workflow/domain.md`
- [../../WORKFLOW.md](../../WORKFLOW.md) → `docs/ai-workflow/workflow.md`
- [../workflow/contracts.md](../workflow/contracts.md) → `docs/ai-workflow/contracts.md`
- [../workflow/pipeline-reference.md](../workflow/pipeline-reference.md) → `docs/ai-workflow/pipeline-reference.md`
- [../../DEFINITION_OF_DONE.md](../../DEFINITION_OF_DONE.md) → `docs/ai-workflow/definition-of-done.md`
- [../../CONTEXT_POLICY.md](../../CONTEXT_POLICY.md) → `docs/ai-workflow/context-policy.md`
- [../../CONVENTIONS.md](../../CONVENTIONS.md) → `docs/ai-workflow/conventions.md`

### 5. Done

Tell the user bootstrap is complete. Skills may read `docs/ai-workflow/*` when present. Re-run only to refresh copies after package updates.
