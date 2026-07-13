# Agent workflow

This repo uses AI-Workflow skills.

## Eight phases

See `docs/ai-workflow/workflow.md` for the core map. Details: `docs/ai-workflow/pipeline-reference.md`.

| Phase | Skill |
|-------|-------|
| Spec | `to-spec` |
| Plan | `to-issues` |
| Branching | `git-workflow-and-versioning` |
| Build | `tdd` |
| Verify | `slice-verification` (slice); `finishing` Step 1 (branch) |
| Review | `code-review` |
| Simplify | `code-simplification` |
| Ship | `finishing-a-development-branch` |

## Optional bootstrap

| Skill | When |
|-------|------|
| `setup-workflow` | Repo lacks `docs/ai-workflow/` — not required for Core path |

## Planning

| Skill | When |
|-------|------|
| `context-discovery` | Optional — stress-test plan vs CONTEXT / ADRs |
| `to-spec` | Short spec |
| `to-issues` | Vertical slices as tasks |

## Per slice

`git-workflow-and-versioning` → `tdd` → `slice-verification` → **user commits** (HITL)

## Close

`code-review` → optional `code-simplification` / `improve-codebase-architecture` → `finishing-a-development-branch`

## Docs in this repo

| File | Purpose |
|------|---------|
| `docs/ai-workflow/workflow.md` | Eight phases + fast path |
| `docs/ai-workflow/contracts.md` | Phase contracts |
| `docs/ai-workflow/pipeline-reference.md` | HITL, tables, boundaries |
| `docs/ai-workflow/definition-of-done.md` | Ready-to-ship |
| `docs/ai-workflow/context-policy.md` | Minimal context |
| `docs/ai-workflow/conventions.md` | Evidence, session rules |

## Boundaries

- `slice-verification` (slice) ≠ `finishing` Step 1 (branch)
- `code-review` (diff) ≠ branch runtime verification
- `tdd` (build) ≠ `correctness-and-tests` checklist (review only)
