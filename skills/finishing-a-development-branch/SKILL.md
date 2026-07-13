---
name: finishing-a-development-branch
description: Verify whole branch then offer merge/PR menu (HITL). Use when ready to ship. Not for per-slice checks or diff-only review.
---

# Finishing a Development Branch

**Ship phase** — whole-branch verification (Step 1) then git integration menu (Step 2). Not for:

- Per-slice smoke after GREEN → `slice-verification`
- Diff review → `code-review`
- PR readiness checklist only → `delivery-quality-gates.md` in review

Transversal: [CONVENTIONS.md](../CONVENTIONS.md). Done criteria: [DEFINITION_OF_DONE.md](../DEFINITION_OF_DONE.md).

## Overview

Complete development work: **verify the whole branch with evidence**, then handle git integration (merge, PR, cleanup).

**Core principle:** Verify branch → Detect environment → Present options → Execute choice → Clean up.

**Announce at start:** "I'm using the finishing-a-development-branch skill to complete this work."

## When to use

- Ready to **push, open PR, or merge** — not mid-slice.
- **Ship phase** after review/simplify (or accepted risk).

## When not to use

- Single slice after GREEN → `slice-verification`
- Diff review only → `code-review`

## Expected input

- Feature branch; optional AC/spec references.

## Expected output

- Step 1: branch verification report with evidence.
- Step 2: user-chosen git option (HITL unless user asks agent to run git).

**Push and PR remain HITL** unless the user explicitly asks the agent to run git in that turn.

## Minimal checklist

- [ ] Step 1 complete with fresh evidence before git menu
- [ ] Exactly 4 options (3 on detached HEAD)
- [ ] No merge without re-verify on merged result

## Related skills (optional)

| Skill | When |
|-------|------|
| `slice-verification` | Per-slice light checks — not a substitute for Step 1 here |
| `code-review` | Diff quality — does not replace runtime AC checks in Step 1 |
| `frontend-testing` | UI evidence during Step 1 |
| `diagnose` | Step 1 failed, cause unclear |

---

## Step 1: Verify branch

Follow [verification-report.md](verification-report.md) (procedure + report template).

If Step 1 passes → Step 2. If it fails → fix and re-run; do not show the git menu.

---

## Step 2: Detect environment

```bash
GIT_DIR=$(cd "$(git rev-parse --git-dir)" 2>/dev/null && pwd -P)
GIT_COMMON=$(cd "$(git rev-parse --git-common-dir)" 2>/dev/null && pwd -P)
```

| State | Menu | Cleanup |
|-------|------|---------|
| `GIT_DIR == GIT_COMMON` (normal repo) | Standard 4 options | No worktree to clean up |
| `GIT_DIR != GIT_COMMON`, named branch | Standard 4 options | Provenance-based (Step 6) |
| `GIT_DIR != GIT_COMMON`, detached HEAD | Reduced 3 options (no merge) | No cleanup (externally managed) |

## Step 3: Determine base branch

```bash
git merge-base HEAD main 2>/dev/null || git merge-base HEAD master 2>/dev/null || git merge-base HEAD develop 2>/dev/null
```

Or ask: "This branch split from develop/main — is that correct?"

## Step 4: Present options

**Normal repo and named-branch worktree — exactly 4 options:**

```
Branch verified. What would you like to do?

1. Merge back to <base-branch> locally
2. Push and create a Pull Request
3. Keep the branch as-is (I'll handle it later)
4. Discard this work

Which option?
```

**Detached HEAD — exactly 3 options:**

```
Branch verified. You're on a detached HEAD (externally managed workspace).

1. Push as new branch and create a Pull Request
2. Keep as-is (I'll handle it later)
3. Discard this work

Which option?
```

Keep options concise — no extra explanation.

## Steps 5–6: Execute and cleanup

Follow [REFERENCE.md](REFERENCE.md) for bash per option, worktree cleanup, quick-reference table, and red flags.

**Never:** Offer the git menu without Step 1 in this session; claim tests pass without fresh output; merge without re-verify on merged result.

**Always:** Complete the branch verification report before the menu; present exactly 4 options (3 on detached HEAD).
