---
name: code-simplification
description: Simplify code in scope without behavior change. Use after tests pass or review readability findings. Not for structural seams — use improve-codebase-architecture.
---

# Code Simplification

**Simplify phase** — clarity only; behavior unchanged.

Transversal: [CONVENTIONS.md](../CONVENTIONS.md). Patterns: [patterns.md](patterns.md#eligibility).

## When to use

- After review flags readability; tests green.
- User asks to simplify names, duplication, nesting.

## When not to use

- Behavior or spec change → `tdd`
- Shallow modules / seams → `improve-codebase-architecture`
- Full diff review → `code-review`

## Expected input

- Scope: paths, diff since branch/commit, or user slice.

## Expected output

- [completion-report.md](completion-report.md); tests pass unchanged.

## Process

### 1. Pin scope

Paths, directory, commit range, branch vs `main`, or "recent changes." If unspecified, ask: "What should I simplify — files, a diff since a branch/commit, or recent changes?"

Capture once: explicit paths only; or `git diff <fixed-point>...HEAD`; or last commit / user slice.

### 2. Load conventions

Skim repo standards per [CONVENTIONS.md](../CONVENTIONS.md#repo-standards-discovery).

### 3. Understand before editing

Answer: responsibility, callers, edge cases, test coverage (run tests before the first edit). Use `git blame` when the shape looks intentional. No edits until context is clear.

### 4. Plan simplifications

Scan with [patterns.md](patterns.md) (Java examples). List `path:line` + intended action; high-impact, low-risk first. If scope is **500+ lines**, propose automation instead of hand edits.

### 5. Apply incrementally

One logical change → run tests → revert on failure → continue. Do not batch untested edits or change test expectations to green-light refactors.

### 6. Report

Output using [completion-report.md](completion-report.md). Confirm: tests pass unchanged, lint/build OK, diff within scope, no weakened error handling.

## Minimal checklist

- [ ] Scope pinned; tests run before first edit
- [ ] Incremental edits with tests after each step
- [ ] completion-report produced; behavior unchanged

## Red flags

Revert or stop: tests need expectation changes; result is harder to read; edits outside scope; mixed bugfix/feature/refactor without user split.
