# Simplification output template

Use this structure for the final report. One agent produces the full summary.

## Rules

- Cite `path:line` or file for each change
- List tests/commands actually run with pass/fail
- If nothing was simplified: say why (already clear, blocked by scope, tests failed, etc.)
- Cap ~300 words; no long narrative

## Template

```markdown
## Summary

<one line: count of files touched + net assessment (clearer / unchanged / reverted)>

## Scope

<what was pinned: paths, diff base, or user-stated slice>

## Changes

- path — what was simplified (e.g. guard clauses, rename, extract helper, remove dead code)

(or: No changes — <reason>)

## Tests

- `<command>` — passed | failed
- ...

## Skipped

- path or pattern — why left unchanged (out of scope, perf-sensitive, unclear behavior, etc.)

(or: None)

## Risks

<residual uncertainty, if any; otherwise: None identified>
```
