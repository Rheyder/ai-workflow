# Review output template

One session per [CONVENTIONS.md](../../CONVENTIONS.md#single-agent-session). Report only axes you loaded in `code-review` Step 3.

## Rules

- Cite `path:line` or a specific diff hunk for each finding
- Severity: **Critical** (merge blocker) | **Important** | **Minor** | **Nit**
- Prefer evidence in the diff; skip generic advice
- Omit dimension sections you did not load
- Cap ~400 words per loaded dimension on large diffs; prioritize Critical and Important

## Template

```markdown
## Summary

<one line: counts by severity + worst issue, if any>

## Blockers

- [Critical] path:line — finding
(or: None)

## Important findings

- [Important] path:line — finding
(or: None)

## Recommended improvements

- [Minor/Nit] path:line — finding
(or: None)

## Evidence reviewed

- Diff: `<fixed-point>...HEAD`
- Axes loaded: <list>
- Optional: slice verification notes, spec/issue refs

## Next step

<single recommended action>

---

<!-- Include ONLY sections below for axes you loaded. -->

## Correctness & Tests

- [Severity] path:line — finding

## Security & Configuration

- [Severity] path:line — finding

## Integrations & Data

- [Severity] path:line — finding

## Reliability & Observability

- [Severity] path:line — finding

## Maintainability & Design

- [Severity] path:line — finding

## Delivery & Quality Gates

- [Severity] path:line — finding

## Spec Audition

### Missing / Partial
- [Severity] — finding (quote spec/issue)

### Scope creep
- [Severity] — finding

### Likely wrong implementation
- [Severity] — finding
```

**Ordering:** Consolidated sections (Summary through Next step) first; detail sections follow for traceability. Duplicate Critical items may appear in both Blockers and the dimension section.
