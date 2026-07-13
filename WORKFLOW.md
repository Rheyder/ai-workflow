# Workflow

## Phases

1. **Spec** — problem, scope, acceptance criteria
2. **Plan** — vertical slices, order, verification per slice
3. **Branching** — clean baseline, working branch
4. **Build** — smallest useful change (prefer TDD for new behavior or bugs)
5. **Verify** — evidence the change works
6. **Review** — proportional diff review by risk
7. **Simplify** — remove accidental complexity (behavior preserved)
8. **Ship** — final checks, delivery summary, integration

## Rules

- Use only the context needed for the **current phase**.
- Use **adapters** only when configured and relevant ([ADAPTERS.md](ADAPTERS.md)).
- Do **not** depend on a specific tool or IDE.
- Do **not** skip verification.
- Do **not** ship without minimum quality evidence ([CONVENTIONS.md](CONVENTIONS.md#evidence-before-claims)).

## Optional bootstrap

Use `setup-workflow` only to prepare a repository that lacks conventions, context policy, or Definition of Done under `docs/ai-workflow/`.

The Core Workflow does **not** depend on that step.

## Skill map

| Phase | Skill |
|-------|-------|
| Spec | `to-spec` |
| Plan | `to-issues` |
| Branching | `git-workflow-and-versioning` |
| Build | `tdd` |
| Verify | `slice-verification` (per slice); `finishing-a-development-branch` Step 1 (whole branch before ship) |
| Review | `code-review` |
| Simplify | `code-simplification` |
| Ship | `finishing-a-development-branch` |

Optional before Spec/Plan: `context-discovery` (Toolbox). After Review: `improve-codebase-architecture` (Toolbox) when structure, not readability, is the issue.

## Fast path

For **small, safe, localized** changes:

1. Understand the request.
2. Implement the smallest change.
3. Run relevant verification.
4. Review proportional risk.
5. Deliver a short summary.

**Do not use fast path when there is:** new business logic, security impact, data migration, architectural change, external integration, production impact, or unclear expected behavior.

## Further reading

| Document | Purpose |
|----------|---------|
| [workflow/contracts.md](skills/workflow/contracts.md) | Input / action / output per phase |
| [workflow/pipeline-reference.md](skills/workflow/pipeline-reference.md) | HITL, tables, target-repo layout |
| [DEFINITION_OF_DONE.md](DEFINITION_OF_DONE.md) | When a change is ready |
| [CONTEXT_POLICY.md](CONTEXT_POLICY.md) | Minimal context between phases |
| [README.md](README.md) | Core vs Toolbox vs checklists vs adapters |
