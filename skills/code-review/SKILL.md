---
name: code-review
description: Review a diff by risk before ship. Use checklists only when relevant. Not for runtime verification — use finishing-a-development-branch.
disable-model-invocation: true
---

# Code Review

Review a change with focus on **real risk**. Follow [CONVENTIONS.md](../../CONVENTIONS.md#single-agent-session). Load **only applicable** checklists, then report via [reviewer-prompt.md](reviewer-prompt.md).

## When to use

- Diff is ready on a feature branch (slices passed light `slice-verification` before commit).
- Before push/PR, after implementation.

## When not to use

- Full branch AC / runtime proof — `finishing-a-development-branch` Step 1.
- Per-slice smoke test — `slice-verification`.
- Writing tests first — `tdd`.

## Expected input

- Fixed point for diff (branch, SHA, tag) — ask if missing.
- Optional: spec, issue, verification notes from Verify phase.

## Process

### 1. Understand the change

- Goal of the change (issue, PRD, conversation).
- `git diff <fixed-point>...HEAD` and `git log <fixed-point>..HEAD --oneline`.

### 2. Skim repo context (light)

If present in the repo under review: `CONTEXT.md`, `docs/adr/`, `docs/ai-workflow/*`, `AI_WORKFLOW.md`, `STYLE.md` / `STANDARDS.md`. Do not re-litigate what CI/lint already enforces.

## Optional adapters

If configured, use relevant adapters for:

- repository documentation;
- organization or language standards;
- security policies;
- delivery gates.

See [ADAPTERS.md](../../ADAPTERS.md). **Do not require adapters** to complete a review.

### 3. Select review axes

**Default for any code diff:** correctness-and-tests + maintainability-and-design.

Add axes when the diff touches:

| Axis | Load when |
|------|-----------|
| [security-and-configuration.md](security-and-configuration.md) | Auth, secrets, config, env, permissions, crypto |
| [integrations-and-data.md](integrations-and-data.md) | APIs, queues, DB schema/migrations, external I/O |
| [reliability-and-observability.md](reliability-and-observability.md) | Retries, timeouts, logging, metrics, failure modes |
| [delivery-quality-gates.md](delivery-quality-gates.md) | CI/workflow, build scripts, lint config, release paths |
| [spec-audition.md](spec-audition.md) | Spec, PRD, or issue AC in scope |

Skip an axis when nothing in the diff applies — do not load the file.

### 4. Apply selected checklists

For each loaded file, apply checklist items to the diff only. Classify: **Critical** (merge blocker) | **Important** | **Minor** | **Nit**. Blockers before cosmetics.

### 5. Report

Use [reviewer-prompt.md](reviewer-prompt.md). Put **Blockers** first in the consolidated section.

### 6. Next step (one at a time)

```
Review complete
├─ Readability / duplication → offer code-simplification (user confirms)
├─ Structure / seams → offer improve-codebase-architecture (user confirms)
├─ Critical / Important on diff → fix via tdd; re-run code-review
├─ Spec gap in code → tdd or update spec/issue
└─ No blockers → finishing-a-development-branch (branch verify + ship menu)
```

Do not run `code-simplification` or `improve-codebase-architecture` without explicit user approval.

## Expected output

Report per reviewer-prompt: Summary, Blockers, Important findings, Recommended improvements, Evidence reviewed, Next step; plus detail sections **only for axes loaded**.

## Minimal checklist

- [ ] Fixed point pinned
- [ ] Applicable axes selected (not all by default)
- [ ] Findings cite `path:line` or diff hunk
- [ ] Blockers stated before nits
- [ ] Next step offered

## Boundaries

- **correctness-and-tests** — adequacy of tests in the diff; does not replace running the suite (`slice-verification` / `finishing`).
- **delivery-quality-gates** — PR readiness signals; does not replace `finishing` Step 1.
- **Runtime verification** — `finishing-a-development-branch` Step 1 only.
