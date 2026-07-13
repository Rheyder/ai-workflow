# Finishing a branch — git integration reference

Used by [SKILL.md](SKILL.md) Steps 5–6. Branch verification: [verification-report.md](verification-report.md). Evidence: [CONVENTIONS.md](../CONVENTIONS.md#evidence-before-claims).

## Step 5: Execute choice

### Option 1: Merge locally

```bash
MAIN_ROOT=$(git -C "$(git rev-parse --git-common-dir)/.." rev-parse --show-toplevel)
cd "$MAIN_ROOT"

git checkout <base-branch>
git pull
git merge <feature-branch>

# Verify tests on merged result — fresh run, same discipline as Step 1
<test command>

git branch -d <feature-branch>
```

Cleanup worktree (Step 6) only after merge succeeds.

### Option 2: Push and create PR

```bash
git push -u origin <feature-branch>

# Create PR/MR via configured adapter or project CLI — see docs/ai-workflow/issue-tracker.md if present
# Title/body should include Summary and Test Plan from branch verification report
```

Do **not** clean up worktree. Do **not** push unless the user explicitly asked in this turn.

### Option 3: Keep as-is

Report: "Keeping branch <name>. Worktree preserved at <path>." Do not cleanup worktree.

### Option 4: Discard

Confirm first:

```
This will permanently delete:
- Branch <name>
- All commits: <commit-list>
- Worktree at <path>

Type 'discard' to confirm.
```

Wait for exact `discard`. Then `cd` to main repo root (Step 6), then `git branch -D <feature-branch>`.

## Step 6: Cleanup workspace

**Only for Options 1 and 4.**

```bash
GIT_DIR=$(cd "$(git rev-parse --git-dir)" 2>/dev/null && pwd -P)
GIT_COMMON=$(cd "$(git rev-parse --git-common-dir)" 2>/dev/null && pwd -P)
WORKTREE_PATH=$(git rev-parse --show-toplevel)
```

**If `GIT_DIR == GIT_COMMON`:** Done.

**If worktree under `.worktrees/` or `worktrees/`** (repo-local agent worktrees):

```bash
MAIN_ROOT=$(git -C "$(git rev-parse --git-common-dir)/.." rev-parse --show-toplevel)
cd "$MAIN_ROOT"
git worktree remove "$WORKTREE_PATH"
git worktree prune
```

**Otherwise:** Host or harness owns the workspace — do **not** remove it.

## Quick reference

| Option | Merge | Push | Keep worktree | Cleanup branch |
|--------|-------|------|---------------|----------------|
| 1. Merge locally | yes | - | - | yes |
| 2. Create PR | - | yes | yes | - |
| 3. Keep as-is | - | - | yes | - |
| 4. Discard | - | - | - | yes (force) |

## Red flags

**Never:** Offer git menu without Step 1 in this session; claim tests pass without output; proceed with failing checks; merge without re-verify on merged result; delete work without typed `discard`; force-push without explicit request; remove worktree before merge success; run `git worktree remove` from inside that worktree; clean up harness-owned worktrees.

**Always:** Complete branch verification report before menu; detect environment first; present exactly 4 options (3 detached HEAD); `cd` to main repo root before worktree removal; `git worktree prune` after removal.
