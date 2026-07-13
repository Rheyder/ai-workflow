---
name: frontend-testing
description: Collect UI behavior evidence for verification. Use when slice or finishing needs UI proof. Optional helper — not a core phase.
---

# Frontend Testing

Provide **runtime UI evidence** for verification skills. Review happens in `code-review`; full branch checks in `finishing-a-development-branch` Step 1.

## When to invoke

- `slice-verification` — this slice changed UI behavior
- `finishing-a-development-branch` Step 1 — feature has UI acceptance criteria

**Do not** replace automated unit/integration tests. Add UI checks when the public interface is the UI.

## Process

Use the project's configured frontend, browser, or end-to-end testing approach.

1. **Scope** — Which flows or pages this change touches (from slice/feature AC).
2. **Setup** — Discover project test config (`e2e/`, `tests/e2e/`, scripts in `package.json`, Makefile, etc.).
3. **Run** — Targeted scenarios (happy path + at least one error/edge if applicable).
4. **Evidence** — Collect when available:
   - test output;
   - screenshot;
   - trace;
   - console errors;
   - network failures;
   - accessibility findings.
5. **Hand back** — Return to the calling skill to complete the verification report. State pass/fail with command output ([CONVENTIONS.md](../../CONVENTIONS.md#evidence-before-claims)).

## Boundaries

- **vs code-review** — No diff review here; behavior evidence only.
- **vs slice-verification / finishing** — Helper step, not a standalone gate.

No single tool (Playwright, Cypress, Selenium, manual checks, etc.) is required.

## Red flags

- Claiming UI works without evidence
- Full regression when only one flow changed (stay scoped)
