---
name: git-worktrees
description: Use when the same repository has more than one active initiative, a Complex track, a hotfix parallel to another track, or explicit isolation is required before plan/implementation; mentions worktree, parallel branch, or `.worktrees`. Do not use when there is a single initiative without parallelism and the method does not require a worktree.
---

# Git worktrees

## Overview

Isolate **one checkout per branch** in the same repo without flipping the main working tree’s branch.

**Principle:** choose directory **systematically** → **ignore-check** for in-repo paths → **verifiable baseline** before heavy work.

Alignment: [workflow-conventions.md § Cross-skill alignment](../../reference/workflow-conventions.md#cross-skill-alignment) and [workflow-conventions.md](../../reference/workflow-conventions.md) for artifact paths.

## When to use

Any one of:

- Another active initiative in the **same** repo.
- **Complex** track.
- **Hotfix** parallel to other work.

**Branch name:** `<track>/<artifact-slug>` — `.cursor/rules/modo-solo.mdc`.

## When not to use

- Single stream of work and method does not require isolation → stay in current workspace.

## Choose directory (fixed order)

1. **Existing project convention** — if exactly one known folder exists, use it; **`.worktrees` wins over `worktrees`** if both exist (`Test-Path` / `test -d`).
2. **Documented in repo** — per [workflow-conventions.md](../../reference/workflow-conventions.md).
3. **None** — ask: `.worktrees/`, `worktrees/`, or path outside repo.

## Safety — paths inside the repo

Before `git worktree add` **inside** the checkout:

```bash
git check-ignore -q .worktrees
# or chosen folder, e.g.:
git check-ignore -q worktrees
```

- **Not ignored:** add ignore rule, **announce** (HITL commit), **then** create worktree. Do not silently commit.
- **Outside repo:** no `check-ignore` requirement.

**Typical failure:** worktree files appear in `git status`.

## Create worktree

1. Resolve `<track>/<slug>` with user/plan.
2. `git worktree add "<path>/<folder>" -b "<track>/<slug>"` then `cd` there.
3. Install/sync deps once (`package.json`, `Cargo.toml`, etc.).
4. Run project verification; if baseline fails, report evidence and **ask** whether to proceed.
5. Report **absolute path**, baseline result, branch name.

## Parallel branches on overlapping paths

- Compare: `git diff main...branch-a -- <paths>` vs `branch-b`.
- Overlap → merge order per `plan-*`; contract/safety → HITL.

## Quick reference

| Situation | Action |
|-----------|--------|
| Criteria not met | Current workspace only |
| `.worktrees/` or `worktrees/` exists | Prefer `.worktrees` |
| Folder not ignored | Fix ignore + HITL before add |
| Baseline fails | Report; no silent proceed |
| Remove worktree | `git worktree remove <path>` after merge/save per flow |

## Common mistakes

| Problem | Fix |
|---------|-----|
| Worktree for every small task | Apply “when to use” |
| Guess path | Follow fixed order |
| Skip `check-ignore` | Tracked noise / leaks |
| Ignore overlapping diffs | Define integration order in plan |

## Integration

Use **before** parallel slices when triggers match. Merge/rebase/remove per branch flow; commit/push remains user-owned (HITL).
