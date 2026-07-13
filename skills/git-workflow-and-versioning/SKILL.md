---
name: git-workflow-and-versioning
description: Create feature branches and announce slice-ready commits (HITL). Use before tdd and after slice-verification. Not for push/PR — use finishing-a-development-branch.
---

# Git Workflow and Versioning

**Branching phase** — Gitflow, one commit per slice, HITL for commit/push.

Transversal: [CONVENTIONS.md](../../CONVENTIONS.md).

## When to use

- Before `tdd` — create or confirm `feature/<task>` from `develop`.
- After `slice-verification` — announce slice ready to commit.
- After all slices — suggest `code-review` then `finishing` (push still HITL).

## When not to use

- Push, PR, merge menu → `finishing-a-development-branch`
- Running tests → `slice-verification` / `tdd`

## Expected input

- Task or slice scope; integration branch (default `develop`).

## Expected output

- Feature branch ready; per-slice commit announcements with suggested message.
- User performs `git commit` / `git push` unless explicitly asked otherwise.

## Overview

Gitflow keeps production (`main`) and integration (`develop`) separate. Each verified slice becomes one commit.

## Workflow Position

```
git-workflow-and-versioning → tdd → slice-verification (light) → [commits per slice]
  → code-review → finishing-a-development-branch (full verify + push/PR)
```

Run this skill first on a new task. Do not start `tdd` on `develop` or `main` without a dedicated feature branch unless the user explicitly says otherwise.

## Gitflow Branching

| Branch | Branch from | Merge into | Purpose |
|--------|-------------|------------|---------|
| `main` | — | — | Production-ready |
| `develop` | `main` | — | Integration |
| `feature/<task>` | `develop` | `develop` | One task/issue per branch |
| `release/<version>` | `develop` | `main` + `develop` | Release stabilization (when needed) |
| `hotfix/<desc>` | `main` | `main` + `develop` | Urgent production fix |

**Naming:** `feature/<short-description>`, `fix/<short-description>`, `chore/<short-description>` (e.g. `feature/task-creation`).

**Rules:** One task per feature branch; keep branches short-lived; delete merged feature branches. If `develop` is missing, ask whether to create it from `main` or which branch is the integration base.

## Pre-TDD Setup

1. Fetch latest `develop` (or confirmed integration branch).
2. Create and checkout `feature/<task>` from `develop`.
3. Confirm with the user: branch name, task scope, first slice.
4. State: commits are one per slice; commit and push are HITL.
5. Hand off: proceed with `tdd` on this branch.

```bash
git checkout develop
git pull
git checkout -b feature/<task>
```

## Slice Discipline

A **slice** is one vertical, verifiable increment (same idea as TDD tracer bullets).

```
Implement slice → slice-verification → announce ready to commit → next slice
```

**One slice = one commit.** Stage only files for that slice. Target ~100 lines per commit when possible; split if a slice grows beyond ~300 lines without a clear single purpose.

## During Implementation

After each slice passes `slice-verification`:

1. Review `git status` and `git diff` — scope matches one slice only.
2. Check staged diff for secrets (`password`, `secret`, `api_key`, `token`).
3. **Announce** slice ready (template below). Do not run `git commit` unless the user explicitly asks in that turn.
4. Continue on the same feature branch. Save-point: `git reset --hard HEAD` returns to the last committed slice if the next change breaks tests.

## Human-in-the-Loop: Commit and Push

**Canonical rule:** commit and push are always done by the user unless they explicitly instruct the agent to run Git in that interaction.

**Flow:** slice DoD met → announce **slice ready to commit** → user commits or tells agent to `stage + commit` (+ `push` only if asked) → never push/PR without explicit instruction in that turn.

**Post-slice announcement — minimum format:**

```markdown
**Slice N ready to commit.**
- Files: `<path1>`, `<path2>` ...
- Change: <1–3 bullets>
- Suggested message: `<type>(<scope>): <short description>`
- Next: <slice N+1 / awaiting decision / track DoD satisfied — ready for push and PR>

Commit yourself or should I run it?
```

- **Intermediate slice** — announcement only; no push unless the user or plan requires it.
- **Final delivery** — include validation status; propose PR/merge via `finishing-a-development-branch`; push and PR remain HITL.

## Commit messages, hygiene, verification

See [REFERENCE.md](REFERENCE.md) for message examples, pre-commit commands, and checklists.

## Red flags

- Working on `develop` or `main` without a feature branch for a multi-slice task
- Multiple slices in one commit; agent `git commit`/`git push` without explicit instruction
- Vague messages (`fix`, `update`, `misc`); formatting-only mixed with behavior
- Force-push to shared branches; secrets or `.env` in the diff

## Minimal checklist

- [ ] Feature branch from integration base (not `main`/`develop` for multi-slice work)
- [ ] One slice per commit announcement
- [ ] No commit/push without user instruction in that turn
