# Correctness & Tests

**Review axis only** — loaded by `code-review` when applicable. Do not confuse with:

- **`tdd`** — creates or changes behavior test-first (Build phase).
- **`slice-verification`** — runs checks and captures evidence for the **current slice** (Verify phase).
- **`finishing` Step 1** — full branch verification before ship (Verify/Ship).

Focus: whether the **diff** is logically correct and sufficiently tested — not re-executing the full test suite here.

## Checks

- [ ] Changed code matches stated intent (commit messages, comments, API contracts)
- [ ] Happy path works; error paths handled (not only success branches)
- [ ] Edge cases: null/empty, boundaries, zero, max size, invalid input
- [ ] No obvious off-by-one, race, or stale-state bugs in changed code
- [ ] State transitions and invariants hold after the change
- [ ] Regressions: existing behaviour preserved unless intentionally changed
- [ ] Tests exist for new or changed behaviour
- [ ] Tests assert behaviour, not implementation details
- [ ] Edge cases and failure modes covered where risk is non-trivial
- [ ] Test names describe scenario and expected outcome
- [ ] Tests would fail if the implementation were wrong (not tautological)
- [ ] Public API or contract changes documented where users depend on them

If nothing in the diff touches this dimension: report `N/A — no applicable changes`.
