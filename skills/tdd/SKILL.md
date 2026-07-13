---
name: tdd
description: Build or fix behavior test-first (RED-GREEN-refactor, vertical slices). Use for features and bugs. Not for diff review or branch verification.
---

# Test-Driven Development

**Build phase** — create or change behavior **test-first** (RED→GREEN→refactor). Not for:

- Running verification after GREEN → `slice-verification`
- Reviewing test adequacy in a diff → `code-review` / `correctness-and-tests.md`
- Full branch AC before ship → `finishing-a-development-branch` Step 1

Transversal: [CONVENTIONS.md](../../CONVENTIONS.md).

## When to use

- New behavior, business logic, or bugfix with test-first approach.
- User mentions red-green-refactor or TDD.

## When not to use

- Post-GREEN evidence → `slice-verification`
- Review test adequacy in diff → `code-review`

## Expected input

- Slice scope; approved test plan (interfaces, prioritized behaviors).
- `feature/<task>` branch from `git-workflow-and-versioning`.

## Expected output

- Passing tests for current slice; minimal implementation; refactor notes if any.

## Minimal checklist

- [ ] One test at a time (vertical, not horizontal)
- [ ] Public interfaces only
- [ ] After GREEN → `slice-verification` before commit announcement

## Philosophy

**Core principle**: Tests should verify behavior through public interfaces, not implementation details. Code can change entirely; tests shouldn't.

**Good tests** are integration-style: they exercise real code paths through public APIs. They describe _what_ the system does, not _how_ it does it. A good test reads like a specification - "user can checkout with valid cart" tells you exactly what capability exists. These tests survive refactors because they don't care about internal structure.

**Bad tests** are coupled to implementation. They mock internal collaborators, test private methods, or verify through external means (like querying a database directly instead of using the interface). The warning sign: your test breaks when you refactor, but behavior hasn't changed. If you rename an internal function and tests fail, those tests were testing implementation, not behavior.

See [tests.md](tests.md) for examples and [mocking.md](mocking.md) for mocking guidelines.

## Anti-Pattern: Horizontal Slices

**DO NOT write all tests first, then all implementation.** This is "horizontal slicing" - treating RED as "write all tests" and GREEN as "write all code."

This produces **crap tests**:

- Tests written in bulk test _imagined_ behavior, not _actual_ behavior
- You end up testing the _shape_ of things (data structures, function signatures) rather than user-facing behavior
- Tests become insensitive to real changes - they pass when behavior breaks, fail when behavior is fine
- You outrun your headlights, committing to test structure before understanding the implementation

**Correct approach**: Vertical slices via tracer bullets. One test → one implementation → repeat. Each test responds to what you learned from the previous cycle. Because you just wrote the code, you know exactly what behavior matters and how to verify it.

```
WRONG (horizontal):
  RED:   test1, test2, test3, test4, test5
  GREEN: impl1, impl2, impl3, impl4, impl5

RIGHT (vertical):
  RED→GREEN: test1→impl1
  RED→GREEN: test2→impl2
  RED→GREEN: test3→impl3
  ...
```

## Workflow

**Prerequisite:** Use `git-workflow-and-versioning` first to create or confirm a `feature/<task>` branch from `develop`. Do not create ad-hoc branches here.

**After each GREEN slice:** Run `slice-verification` (light only) before `git-workflow-and-versioning` announces the slice ready to commit (HITL). Do not run full branch verification during TDD.

**When all slices are committed:** Suggest `code-review`, then `finishing-a-development-branch` when the user is ready to push/PR (full verification runs there).

### 1. Planning

When exploring the codebase, use **language, framework, and repository standards** when available (glossary, ADRs, `docs/ai-workflow/`, style guides). Match test names and public interfaces to project vocabulary.

Before writing any code:

- [ ] Confirm with user what interface changes are needed
- [ ] Confirm with user which behaviors to test (prioritize)
- [ ] Identify opportunities for [deep modules](deep-modules.md) (small interface, deep implementation)
- [ ] Design interfaces for [testability](interface-design.md)
- [ ] List the behaviors to test (not implementation steps)
- [ ] Get user approval on the plan

Ask: "What should the public interface look like? Which behaviors are most important to test?"

**You can't test everything.** Confirm with the user exactly which behaviors matter most. Focus testing effort on critical paths and complex logic, not every possible edge case.

### 2. Tracer Bullet

Write ONE test that confirms ONE thing about the system:

```
RED:   Write test for first behavior → test fails
GREEN: Write minimal code to pass → test passes
```

This is your tracer bullet - proves the path works end-to-end.

### 3. Incremental Loop

For each remaining behavior:

```
RED:   Write next test → fails
GREEN: Minimal code to pass → passes
```

Rules:

- One test at a time
- Only enough code to pass current test
- Don't anticipate future tests
- Keep tests focused on observable behavior

### 4. Refactor

After all tests pass, look for [refactor candidates](refactoring.md):

- [ ] Extract duplication
- [ ] Deepen modules (move complexity behind simple interfaces)
- [ ] Apply SOLID principles where natural
- [ ] Consider what new code reveals about existing code
- [ ] Run tests after each refactor step

**Refactor routing:**

- Local clarity (names, duplication, nesting) → optional `code-simplification` after tests pass (user confirms)
- Structural seams / shallow modules → note for `code-review`; offer `improve-codebase-architecture` after review (not mid-RED loop)

**Never refactor while RED.** Get to GREEN first.

## Checklist Per Cycle

```
[ ] Test describes behavior, not implementation
[ ] Test uses public interface only
[ ] Test would survive internal refactor
[ ] Code is minimal for this test
[ ] No speculative features added
```
