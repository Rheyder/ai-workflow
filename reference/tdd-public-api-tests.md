# Tests through the public surface (TDD notes)

Companion to `.cursor/skills/tdd-cycle/SKILL.md`. Prefer **behavior** and **contracts callers rely on** over internals.

## Principles

- Exercise the module through **stable entry points** (public API, exported functions, HTTP handlers the product uses).
- Name tests after **observable outcomes**, not after private helpers.
- Prefer **one failing assertion driver per cycle**; grow coverage vertically (one behavior, RED, GREEN, then next).
- Use **real collaborators** when cheap; introduce doubles only when the real dependency is slow, non-deterministic, or unavailable — and understand the **real contract** first (see `.cursor/skills/tdd-cycle/REFERENCE-testing-anti-patterns.md`).
- Avoid testing “that the mock was called” unless that directly proves user-visible behavior through the unit under test.

## Planning before first RED

Align public interface and **domain vocabulary** with the project glossary (see `.cursor/reference/workflow-conventions.md`). Confirm priority behaviors with the human; do not write a large batch of imagined tests before agreement.
