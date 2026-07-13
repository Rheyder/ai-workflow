# Spec Audition

Focus: whether the diff implements what the spec, issue, or PRD asked for. Orthogonal to coding standards and technical dimensions.

## Spec discovery

Resolve the spec in this order; stop at the first hit:

1. Path or issue URL the user passed with the review request
2. Issue refs in commit messages (`#123`, `Closes #45`, etc.) — fetch via configured issue tracker adapter or `docs/ai-workflow/issue-tracker.md`; otherwise ask the user to paste the task body
3. PRD or spec file under `docs/`, `specs/`, or `.scratch/` matching the branch or feature name
4. Ask the user for the spec location or text

If no spec after step 4: report `N/A — no spec available` for this dimension and skip the checks below.

## Checks

- [ ] Every stated requirement has corresponding implementation in the diff
- [ ] Partial implementations flagged (started but incomplete)
- [ ] Behaviour in the diff not requested by the spec (scope creep)
- [ ] Implemented requirements that look incorrect vs spec intent
- [ ] Quote spec line or issue bullet for each finding

## Report grouping

Use these subsections in the review output (see `reviewer-prompt.md`):

- **Missing / Partial** — required work absent or incomplete
- **Scope creep** — changes not asked for in the spec
- **Likely wrong implementation** — present in diff but does not match spec intent

Do not invent requirements when the spec is ambiguous; flag ambiguity and ask the user.
