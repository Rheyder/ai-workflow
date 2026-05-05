---
name: task-delivery
description: Use when a slice or track Definition of Done is met after review and fresh verification evidence, and delivery prep is needed (changelog, selective stage, commit message) plus user announcement; when the user explicitly asks for stage/commit/push/merge/PR after that announcement; or when integrating a branch after committed work (local merge, PR, keep, discard).
---

# Task delivery

## Overview

**Evidence → prepare → announce → Git only if the user asks.** Implements announcement and prep from the solo method; templates also live in `.cursor/rules/commit-push-hitl.mdc`.

Norms: [workflow-conventions.md § Cross-skill alignment](../../reference/workflow-conventions.md#cross-skill-alignment).

Optional at start: mention following `task-delivery`.

## When to use

- Slice meets DoD and reviews pass (when applicable).
- User explicitly requests **stage / commit / push / merge / PR** after your announcement.
- Commits already exist and the user needs **integration options** (merge vs PR vs keep vs discard).

## When not to use

- Verification is missing or stale — run commands first; do not announce “ready.”
- User did not ask for Git — **never** commit or push on your own initiative.

## Flow A — Slice ready to commit

1. **Gate:** fresh verification evidence on record; mid-slice review OK when required by track.
2. **Prepare:** touched paths; 1–3 bullet summary; suggested `type(scope): description`; `changelog-*` per initiative if used.
3. **Stage hint:** list paths to include (avoid `git add .` by default).
4. **Announce:**

```markdown
**Slice N ready to commit.**
- Files: `<path1>`, `<path2>` ...
- Change: <1–3 bullets>
- Suggested message: `<type>(<scope>): <short description>`
- Next: <slice N+1 / awaiting decision / track DoD satisfied — ready for push and PR>

Commit yourself or should I run it?
```

5. **Git:** only after explicit user request in that conversation.

## Flow B — Final delivery (track DoD)

Beyond A: cite `validation-*`, CI/harness when relevant, propose PR title/body. **Push and PR remain HITL.**

PR body skeleton:

```markdown
## Summary
- <bullets>

## Verification plan
- [ ] <commands or checklist>

## References
- validation-* / plan-* (paths)
```

## Flow C — Integrate (commits exist)

1. Tests green — else stop with output.
2. Confirm base branch; `git merge-base` if useful.
3. Present **exactly** these options:

```
Committed work. What next?

1. Merge locally into <base>
2. Push and open Pull Request
3. Keep branch as-is
4. Discard this work

Which option?
```

4. Execute **only** the chosen option, with explicit authorization for network/destructive work. **Discard** only after typed confirmation (`discard`) naming branch/commits/paths.

## Quick reference

| Moment | Artifacts | Agent runs Git? |
|--------|-----------|-----------------|
| End of slice | `changelog-*`, announcement | Only if user asks |
| Final | + `validation-*`, PR draft | Push/PR only if user asks |
| Integration | Options 1–4 | After choice + confirmations |

## Common mistakes

- “Ready” without fresh command output.
- Commit/push without explicit ask.
- Vague “what now?” instead of structured announcement or four options.
- Removing worktree while branch still needed.
- Merge or PR with failing tests.

## Red flags

Failing tests • discard without typed `discard` • force-push without explicit request.

## Related

`code-review` (final + `validation-*`); `git-worktrees` (cleanup). Naming: [workflow-conventions.md](../../reference/workflow-conventions.md).
