# Pipeline reference

Detailed skill order, boundaries, HITL, and target-repo artifacts. Core phases: [WORKFLOW.md](../WORKFLOW.md). Contracts: [contracts.md](contracts.md).

**Optional bootstrap:** `setup-workflow` may seed `docs/ai-workflow/` — not required before Spec/Plan/Build.

## Pipeline diagram

```
Planning (Core + optional Toolbox):
  [setup-workflow] → [context-discovery] → to-spec → to-issues

Implementation (per slice):
  git-workflow-and-versioning → tdd → slice-verification (light) → [HITL commit]

Close:
  code-review → [code-simplification | improve-codebase-architecture] → finishing-a-development-branch
```

## Planning

| Skill | Role | Invoke when |
|-------|------|-------------|
| `setup-workflow` | Optional: seed `docs/ai-workflow/`, `AI_WORKFLOW.md` | Repo lacks neutral workflow docs |
| `context-discovery` | Challenge plan vs `CONTEXT.md` / ADRs | Optional — ambiguous plan |
| `to-spec` | Short spec from conversation | Spec phase |
| `to-issues` | Vertical slices as executable tasks | Spec or plan ready |

## Implementation and closing

| Skill | Role | Invoke when | HITL |
|-------|------|-------------|------|
| `git-workflow-and-versioning` | Gitflow `feature/*`, one commit per slice | Before `tdd`; after verified slice | Branch name / scope |
| `tdd` | RED→GREEN→refactor on public interfaces | Feature or bugfix test-first | Test / behavior plan |
| `slice-verification` | Light checks + evidence for current slice | After GREEN, before commit | — |
| `code-review` | Risk-based diff review | Before integration | — |
| `finishing-a-development-branch` | Step 1: full branch verify; Step 2: delivery menu | Ready to integrate | Push / merge (user) |

Git checklists: [git-workflow-and-versioning/REFERENCE.md](../git-workflow-and-versioning/REFERENCE.md), [finishing-a-development-branch/REFERENCE.md](../finishing-a-development-branch/REFERENCE.md).

## On demand

| Skill | Role | Invoke when |
|-------|------|-------------|
| `code-simplification` | Clarity refactor, no behavior change | After review flags readability |
| `improve-codebase-architecture` | Seams, depth, shallow modules | After review flags structure |
| `diagnose` | Reproduce → hypothesize → fix | Hard bug, unclear verification |
| `frontend-testing` | UI evidence | UI AC in slice or finishing Step 1 |

## Which skill?

| Question | Skill |
|----------|-------|
| Need neutral workflow docs in repo? | `setup-workflow` (optional) |
| Domain language unclear? | `context-discovery` |
| Need spec? | `to-spec` |
| Need slices / tasks? | `to-issues` |
| Ready to code? | `git-workflow-and-versioning` → `tdd` |
| Does this **slice** work? | `slice-verification` |
| Is the **diff** OK? | `code-review` |
| Whole **branch** meets AC? | `finishing` Step 1 |
| Integrate? | `finishing` Step 2+ |
| Readability only? | `code-simplification` |
| Structure / seams? | `improve-codebase-architecture` |
| Obscure bug? | `diagnose` |
| UI AC? | `frontend-testing` |

**Do not confuse:**

- `slice-verification` (slice) ≠ `finishing` Step 1 (branch)
- `code-review` (diff) ≠ branch runtime verification
- Vertical `tdd` ≠ all tests first (horizontal anti-pattern)

## HITL

The agent prepares; the user decides when git history or remotes change.

| Moment | User | Agent |
|--------|------|-------|
| Branch / scope | Confirms | `git-workflow-and-versioning` |
| Test plan | Approves | `tdd` |
| Commit per slice | Commits (unless asked) | After `slice-verification` |
| Push / merge | Runs git (unless asked) | `finishing` menu |
| Post-review refactor | Confirms scope | `code-simplification`, `improve-codebase-architecture` |

## Evidence

Transversal rules: [CONVENTIONS.md](../CONVENTIONS.md).

| Scope | Skill | Report template |
|-------|-------|-----------------|
| Current slice | `slice-verification` | [slice-verification/verification-report.md](../slice-verification/verification-report.md) |
| Whole branch | `finishing` Step 1 | [finishing-a-development-branch/verification-report.md](../finishing-a-development-branch/verification-report.md) |

## Target-repo artifacts (after optional bootstrap)

```
target-repo/
├── AI_WORKFLOW.md                  # workflow index
├── CONTEXT.md                      # optional
├── docs/ai-workflow/
│   ├── issue-tracker.md
│   ├── triage-labels.md
│   ├── domain.md
│   ├── workflow.md
│   ├── contracts.md
│   ├── pipeline-reference.md
│   ├── definition-of-done.md
│   ├── context-policy.md
│   └── conventions.md
├── .scratch/<feature-slug>/        # default planning fallback
│   ├── PRD.md or spec.md
│   └── issues/<NN>-<slug>.md
```

Triage defaults: `needs-triage`, `needs-info`, `ready-for-execution`, `ready-for-review`, `wontfix` — see [setup-workflow/triage-labels.md](../setup-workflow/triage-labels.md).

Planning layout: [setup-workflow/issue-tracker.md](../setup-workflow/issue-tracker.md).
