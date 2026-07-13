# AI-Workflow v2.0

Tool-independent workflow for software changes with clarity, quality, and low context use.

Skills are based primarily on [mattpocock/skills](https://github.com/mattpocock/skills), with additional influence from [obra/superpowers](https://github.com/obra/superpowers).

Transversal rules: [CONVENTIONS.md](CONVENTIONS.md). Portuguese overview: [README-pt_br.md](README-pt_br.md).

## Core Workflow

**Spec → Plan → Branching → Build → Verify → Review → Simplify → Ship**

Per slice: Branching → Build → Verify → (HITL commit) → repeat. After all slices: Review → Simplify (if needed) → Ship.

| Phase | Skill |
|-------|-------|
| Spec | `to-spec` |
| Plan | `to-issues` |
| Branching | `git-workflow-and-versioning` |
| Build | `tdd` |
| Verify | `slice-verification` (per slice) |
| Review | `code-review` |
| Simplify | `code-simplification` |
| Ship | `finishing-a-development-branch` |

## Layers

| Layer | Role |
|-------|------|
| **Core Workflow** | Default path — one skill per phase |
| **Review checklists** | Loaded on demand by `code-review` |
| **Toolbox** | Setup, diagnosis, teaching, handoff, architecture, UI probes, skill authoring |
| **Adapters** | Optional integrations — see [ADAPTERS.md](ADAPTERS.md) |

**Rule:** The Core Workflow must work **without** adapters.

## Start here

1. Read [WORKFLOW.md](WORKFLOW.md) — phases and fast path.
2. Use [DEFINITION_OF_DONE.md](DEFINITION_OF_DONE.md) when judging readiness.
3. Use [CONTEXT_POLICY.md](CONTEXT_POLICY.md) to limit what you load each phase.
4. Follow Core skills on the default path; use Toolbox only when needed.

**Bootstrap is optional.** Use `setup-workflow` only when the target repo lacks `docs/ai-workflow/` or neutral workflow docs. Repos already configured can skip it.

## Main files

| File | Role |
|------|------|
| [WORKFLOW.md](WORKFLOW.md) | Eight phases, rules |
| [workflow/contracts.md](skills/workflow/contracts.md) | Input / action / output per phase |
| [workflow/pipeline-reference.md](skills/workflow/pipeline-reference.md) | HITL, tables, target-repo layout |
| [DEFINITION_OF_DONE.md](DEFINITION_OF_DONE.md) | Ready-to-ship checklist |
| [CONTEXT_POLICY.md](CONTEXT_POLICY.md) | Minimal context between phases |
| [CONVENTIONS.md](CONVENTIONS.md) | Evidence, session, concise communication |
| [ADAPTERS.md](ADAPTERS.md) | Optional integrations |

## Review checklists

Loaded **on demand** by `code-review` — not on the default path:

- [correctness-and-tests.md](skills/code-review/correctness-and-tests.md)
- [maintainability-and-design.md](skills/code-review/maintainability-and-design.md)
- [security-and-configuration.md](skills/code-review/security-and-configuration.md)
- [reliability-and-observability.md](skills/code-review/reliability-and-observability.md)
- [integrations-and-data.md](skills/code-review/integrations-and-data.md)
- [delivery-quality-gates.md](skills/code-review/delivery-quality-gates.md)
- [spec-audition.md](skills/code-review/spec-audition.md)

## Toolbox

Not on the default path. Use when the problem requires it:

| Skill | Use when |
|-------|----------|
| `setup-workflow` | Optional bootstrap of `docs/ai-workflow/` in target repo |
| `context-discovery` | Plan vs `CONTEXT.md` / ADRs needs stress-testing |
| `diagnose` | Hard bug, unclear verification failure |
| `improve-codebase-architecture` | Structural seams, not readability-only |
| `handoff` | Compact state for next session |
| `teach` | Guided learning in workspace |
| `zoom-out` | Map unfamiliar module |
| `write-a-skill` | Extend this package |
| `caveman` | User asks for ultra-short replies |
| `frontend-testing` | UI evidence for slice or ship |

## Install skills

Each skill is a folder with `SKILL.md` (YAML frontmatter + body). Register folders with your agent host; invoke by name or matching `description`.

Skills describe **outcomes** (read files, run tests), not a specific IDE. Repo commands come from the **target project**.

## Example journey

Feature *export report as CSV* (planning in `.scratch/export-csv/`):

| Step | Skill | Outcome |
|------|-------|---------|
| Spec | `to-spec` | Short spec at configured planning location |
| Plan | `to-issues` | Slice tasks with acceptance criteria |
| Branch | `git-workflow-and-versioning` | `feature/export-csv` |
| Build + verify | `tdd` → `slice-verification` | RED→GREEN + evidence |
| Review | `code-review` | Diff report |
| Ship | `finishing-a-development-branch` | Branch verify; delivery summary |

Details: [workflow/pipeline-reference.md](skills/workflow/pipeline-reference.md).

## Philosophy

- **Simple before complete** — load the phase you need.
- **Verifiable before opinion** — evidence before claims.
- **Tool-agnostic before coupled** — any host or human can follow the docs.
- **Less context before more** — [CONTEXT_POLICY.md](CONTEXT_POLICY.md).

## Extend

[write-a-skill/SKILL.md](skills/write-a-skill/SKILL.md). Optionally re-run `setup-workflow` to refresh `docs/ai-workflow/` after package updates.
