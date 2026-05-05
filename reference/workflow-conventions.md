# Workflow conventions (repository artifacts)

Skills cite **only** paths under `.cursor/`. This document is the canonical summary of **where** domain artifacts, plans, and ADRs normally live **in the repository workspace** (repository root = checkout root).

## Domain glossary

- **Single bounded context:** `CONTEXT.md` at repository root.
- **Multiple contexts:** `CONTEXT-MAP.md` at repository root lists contexts and paths to each glossary file.

Authoring format: `.cursor/skills/brainstorming/CONTEXT-FORMAT.md`.

## Architecture Decision Records

- **Directory:** `docs/adr/` at repository root (create when first ADR is written).
- **Files:** `0001-slug.md`, sequential numbering.

Authoring format: `.cursor/skills/brainstorming/ADR-FORMAT.md`.

## Plans, specs, changelog, validation

Typical filenames at repository root unless the team defines otherwise:

- Plans: `plan-YYYY-MM-DD-<slug>.md`
- Specs: `spec-YYYY-MM-DD-<slug>.md`
- Changelog slices: `changelog-YYYY-MM-DD-<name>.md`
- Validation notes: `validation-YYYY-MM-DD-<name>.md`

## Orchestration (solo / slice flow)

- Round flow, Ralph cap, two mid-slice gates: `.cursor/agents/task-orchestrator.md`
- Branch naming: `.cursor/rules/modo-solo.mdc`
- Commit and push are human-in-the-loop: `.cursor/rules/commit-push-hitl.mdc`
- Verification before success claims: `.cursor/rules/verification-before-completion.mdc`

### Cross-skill alignment

Skills and prompts should **not** restate the bullets above in long form; link here instead. Summary: slice execution follows `.cursor/agents/task-orchestrator.md`; any “green”, “ready”, or “fixed” claim needs **fresh evidence** per `.cursor/rules/verification-before-completion.mdc`; `git commit` / `git push` only after an **explicit** user request per `.cursor/rules/commit-push-hitl.mdc`; branches per `.cursor/rules/modo-solo.mdc`. Regressions proven by tests: use **`tdd-cycle`**.

## Worktree parent directory

Prefer an existing team convention (`.worktrees/` vs `worktrees/`). Before asking the human, scan docs they maintain under the repository root (for example `README`, assistant instruction files, domain glossary) for an explicit worktree path policy.
