---
name: handoff
description: Write a compact handoff doc for the next session. Use when switching sessions. Toolbox — not on the default path.
argument-hint: "What will the next session be used for?"
---

# Handoff

Transversal: [CONVENTIONS.md](../../CONVENTIONS.md).

## When to use

- Ending a session; next agent or human needs continuity.

## When not to use

- Mid-slice implementation — finish the phase or use normal artifacts (PRD, issues).

## Expected input

- Current conversation; optional hint for next session focus.

## Process

Write a handoff summarising state so a fresh agent can continue. Save to the **OS temp directory** — not the workspace.

Include **Suggested skills** — smallest set from lists below (skill in backticks + one line why).

**Core:** `to-spec`, `to-issues`, `git-workflow-and-versioning`, `tdd`, `slice-verification`, `code-review`, `code-simplification`, `finishing-a-development-branch`

**Toolbox:** `setup-workflow`, `context-discovery`, `diagnose`, `improve-codebase-architecture`, `frontend-testing`, `zoom-out`, `caveman`, `teach`, `write-a-skill`

Suggest the **smallest set** that matches the next session's goal (e.g. mid-slice work → `tdd` + `slice-verification`; ready to merge → `finishing-a-development-branch`; explore unfamiliar area → `zoom-out`).

Do not duplicate content already captured in other artifacts (PRDs, plans, ADRs, issues, commits, diffs). Reference them by path or URL instead.

Redact any sensitive information, such as API keys, passwords, or personally identifiable information.

If the user passed arguments, treat them as a description of what the next session will focus on and tailor the doc accordingly.

## Expected output

- Handoff file path; suggested skills; references to PRD/issues/commits (not duplicates).

## Minimal checklist

- [ ] Secrets redacted
- [ ] Artifacts referenced by path, not copied in full
