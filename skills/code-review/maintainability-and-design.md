# Maintainability & Design

Focus: readability, structure, duplication, scope, and fit with the codebase. **Review only** — do not refactor here.

**Execution handoffs (user must confirm):**

- Readability / duplication / nesting → offer `code-simplification` on cited paths
- Shallow modules, tangled seams, poor test surface, hidden coupling → offer `improve-codebase-architecture` on cited scope (same flag-then-execute pattern as simplification)

## Checks

- [ ] Names describe intent; consistent with surrounding code
- [ ] Functions and modules have a clear single responsibility
- [ ] Control flow easy to follow (avoid deep nesting without reason)
- [ ] No unnecessary duplication; shared logic extracted only when duplication is real
- [ ] No speculative abstractions or features beyond what the change needs (YAGNI)
- [ ] Change follows existing patterns unless deviation is justified in context
- [ ] Module boundaries respected; dependencies flow in the expected direction
- [ ] No dead code, commented-out blocks, or unused imports introduced
- [ ] File size growth reasonable for this change (flag only what this diff added)
- [ ] Documented repo standards respected where applicable — [CONVENTIONS.md](../../CONVENTIONS.md#repo-standards-discovery)

If nothing in the diff touches this dimension: report `N/A — no applicable changes`.
