# Git workflow — reference

Used by [SKILL.md](SKILL.md). Transversal rules: [CONVENTIONS.md](../CONVENTIONS.md).

## Commit messages

Explain *why*, not only *what*:

```
<type>: <short description>

<optional body — intent, not a file list>
```

**Types:** `feat`, `fix`, `refactor`, `test`, `docs`, `chore`

```
# Good
feat(auth): add email validation on registration route

Prevents invalid emails before persistence; matches Zod pattern in auth.ts.

# Bad
update auth.ts
```

Keep refactors and features in separate commits when they are separate slices.

## Pre-commit hygiene

Before announcing a slice (or before committing when the user asked you to):

```bash
git diff --staged
git diff --staged | grep -iE 'password|secret|api_key|token'  # must be empty
# run project tests/lint/typecheck as applicable
```

Do not commit: `.env`, build artifacts (`dist/`, `.next/`), `node_modules/`. Commit lockfiles/migrations only if the project expects them.

## Verification checklists

**Starting a task:**

- [ ] Feature branch exists from `develop` (or user-approved base)
- [ ] Branch name matches task; user confirmed scope
- [ ] User informed: one commit per slice, HITL commit/push

**Per slice (before announce):**

- [ ] One slice only — diff scope is focused
- [ ] Tests/verification for this slice pass
- [ ] No secrets in diff
- [ ] Suggested commit message uses type + clear description
- [ ] Announcement uses the HITL template; no self-initiated commit/push

**Handoff after last slice:**

- [ ] Suggest `code-review` on the feature branch
- [ ] After review, suggest `finishing-a-development-branch` for full verification + merge/PR
- [ ] Push/PR only if user explicitly requests
