# Cursor configuration

This directory holds **persistent rules**, **skills** (reusable procedures), and **agent definitions** used with Cursor in this repository. The workflow (Normal/Complex tracks, vertical slices, delivery) aligns with the Rheyder`s method; **executable artifacts** (text, prompts, conventions) that the assistant applies day-to-day live here.

**Skills** use progressive disclosure (overview, when to use / when not) and cross-links to [reference/workflow-conventions.md](reference/workflow-conventions.md), consistent with the in-repo **skill-writer** reference tree under [reference/skill-authoring/](reference/skill-authoring/).

**Repository-root artifact conventions** (glossary, ADRs, `plan-*`, `changelog-*`, `validation-*`): see [reference/workflow-conventions.md](reference/workflow-conventions.md).

---

## Layout

| Path | Role |
|-----------|------|
| [rules/](rules/) | Project *always-on* rules (style, language, verification, Git HITL, solo mode). |
| [skills/](skills/) | `SKILL.md` bundles plus references: invoke by **skill name** when the scenario applies. |
| [agents/](agents/) | Method agents (orchestrator and per-slice roles) + prompts under `slice-subagents/`. |
| [reference/](reference/) | Short canonical notes cited by skills (conventions, TDD through public surface). |
| [rheyder-method-v1.0.md](rheyder-method-v1.0.md) | Normative **Rheyder method** baseline (tracks, conventions, appendix); authoritative human-readable companion to `.cursor/` materialization. |

---

## Rules (`rules/`)

`.mdc` files applied automatically by Cursor according to project settings.

| File | Summary |
|------|---------|
| [karpathy-guidelines.mdc](rules/karpathy-guidelines.mdc) | Think before coding, simplicity, surgical changes, verifiable success criteria. |
| [verification-before-completion.mdc](rules/verification-before-completion.mdc) | Do not claim completion without **fresh evidence** (command run, output read, exit code). |
| [language.mdc](rules/language.mdc) | User-facing text in **pt-BR**; everything else in English (`en_US`) unless the user explicitly requests otherwise. |
| [base-creation.mdc](rules/base-creation.mdc) | Self-contained: skills/rules/agents must work from the clone alone (no reliance on external links). |
| [commit-push-hitl.mdc](rules/commit-push-hitl.mdc) | **Commit and push** are always **human-in-the-loop**; the assistant announces a slice ready and runs Git only if the user asks. |
| [modo-solo.mdc](rules/modo-solo.mdc) | Solo mode: orchestrator + developer + reviewer as **roles/prompts**, not separate people; branches `<track>/<artifact-slug>`. |

---

## Skills (`skills/`)

Each skill is a folder with `SKILL.md` at the top; many include supporting files (prompts, references, scripts).

| Skill (slug) | Directory | When to use |
|--------------|-----------|-------------|
| **brainstorming** | [skills/brainstorming/](skills/brainstorming/) | Design or specification **before** code; align with glossary and ADRs; PRD / grill / domain modes. |
| **plan-writing** | [skills/plan-writing/](skills/plan-writing/) | Closed requirements → materialize `plan-*` as vertical slices (Plan step in Normal/Complex tracks). |
| **tdd-cycle** | [skills/tdd-cycle/](skills/tdd-cycle/) | New behavior or fix with mandatory TDD; RED–GREEN–refactor with a failing test first. |
| **code-review** | [skills/code-review/](skills/code-review/) | Review before merge or between slices; large diff vs plan; verify feedback before replying. |
| **systematic-debugging** | [skills/systematic-debugging/](skills/systematic-debugging/) | Bugs, CI, flaky tests, regressions; root cause before patching; see `root-cause-tracing.md`, etc. |
| **task-delivery** | [skills/task-delivery/](skills/task-delivery/) | Slice/track DoD met → prepare changelog, selective stage, commit message, announcement; Git only after explicit request. |
| **git-worktrees** | [skills/git-worktrees/](skills/git-worktrees/) | Multiple parallel initiatives, parallel hotfix, or explicit isolation with worktrees. |
| **improve-codebase-architecture** | [skills/improve-codebase-architecture/](skills/improve-codebase-architecture/) | Deepen architecture with glossary and ADRs; refactoring and testability opportunities. |
| **caveman** | [skills/caveman/](skills/caveman/) | Ultra-compressed communication when the user asks (fewer tokens, no fluff). |

**Additional skills** in the user’s Cursor install (outside this repo), when configured — e.g. PR babysit, canvas, Cursor SDK, rule/skill authoring — still follow this project’s language and verification rules.

---

## Agents (`agents/`)

YAML+Markdown definitions for subagents in the **one slice at a time** flow.

| Agent | File | Role |
|-------|------|------|
| **task-orchestrator** | [agents/task-orchestrator.md](agents/task-orchestrator.md) | Coordinates branch, TDD implementation, two mid-slice reviews, Ralph (limited attempts), DoD; no commit/push on its own. |
| **slice-developer** | [agents/slice-developer.md](agents/slice-developer.md) | Implements the slice; canonical instructions in `slice-subagents/slice-developer-prompt.md`. |
| **slice-spec-reviewer** | [agents/slice-spec-reviewer.md](agents/slice-spec-reviewer.md) | First review: plan/spec ↔ code alignment (SPEC_OK / failures). |
| **slice-quality-reviewer** | [agents/slice-quality-reviewer.md](agents/slice-quality-reviewer.md) | Second review: quality and verification evidence; only after SPEC_OK. |
| **skill-writer** | [agents/skill-writer.md](agents/skill-writer.md) | Authors or revises skills using Superpowers writing-skills + Matt Pocock write-a-skill + Anthropic best practices (in-repo); TDD baseline, CSO descriptions, progressive disclosure. |

**Detailed prompts** (placeholders, rubrics, report formats): [agents/slice-subagents/](agents/slice-subagents/) — includes `slice-developer-prompt.md`, `slice-spec-reviewer-prompt.md`, `slice-quality-reviewer-prompt.md`, `orchestrator-slice-round-prompt.md`.

---

## Reference (`reference/`)

| File | Contents |
|------|----------|
| [workflow-conventions.md](reference/workflow-conventions.md) | Where to put glossary (`CONTEXT.md`), ADRs (`docs/adr/`), naming patterns for `plan-*`, `spec-*`, changelog, validation; **Cross-skill alignment** subsection is the one-line index for orchestration + verification + Git HITL (skills link there instead of repeating). |
| [tdd-public-api-tests.md](reference/tdd-public-api-tests.md) | TDD notes through the **public surface**; complements `tdd-cycle`. |
| [skill-authoring/](reference/skill-authoring/) | Canonical copies of **writing-skills** (Superpowers bundle) and **write-a-skill** (Matt Pocock) for the **skill-writer** agent — under `.cursor` only (no `.ai/...` path). |

---

## Solo flow (summary)

1. **Brainstorming / spec** (skill **brainstorming**) → explicit user acceptance.  
2. **Sliced plan** (skill **plan-writing**) → `plan-*` with verifiable slices.  
3. Per slice: named branch, **task-orchestrator** (or developer role + reviews), TDD where applicable (**tdd-cycle**), spec review then quality review — norms in [workflow-conventions § Cross-skill alignment](reference/workflow-conventions.md#cross-skill-alignment) (verification with real commands, no agent commit/push unless asked).  
4. Announce **slice ready to commit**; **task-delivery** when applicable (templates in **commit-push-hitl**).

---

## Maintenance

- **New skills:** follow [base-creation](rules/base-creation.mdc) (self-contained in the repo) and Cursor’s skill layout (`SKILL.md` + resources referenced by relative path).  
- **Rule changes:** prefer small edits aligned with the methodology when the change is normative.
