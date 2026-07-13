# Issue tracker: Local Markdown (default)

Copied to `docs/ai-workflow/issue-tracker.md` at optional bootstrap.

**Default for this package:** specs and tasks as markdown under `.scratch/`. External issue trackers are optional adapters — see [ADAPTERS.md](../../ADAPTERS.md).

## Conventions

- One feature per directory: `.scratch/<feature-slug>/`
- Spec: `.scratch/<feature-slug>/PRD.md` or `spec.md`
- Tasks: `.scratch/<feature-slug>/issues/<NN>-<slug>.md`, numbered from `01`
- Triage: `Status:` line near the top (see `triage-labels.md`)
- Discussion: append under `## Comments` when needed

## When a skill says "write to the planning location"

Create or update files under the configured path. If none is configured, use `.scratch/<feature-slug>/`.

## When a skill says "fetch the relevant task"

Read the file at the referenced path, or use an external issue adapter if configured.
