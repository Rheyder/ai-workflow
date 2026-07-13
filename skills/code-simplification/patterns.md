# Simplification patterns

Apply only within pinned scope. Each row is a signal to investigate — not an automatic rewrite.

## Eligibility

**Use when:** tests pass but code feels heavy; user asks to simplify; acting on `code-review` maintainability findings (user wants edits applied).

**Skip when:** already clear; behavior not understood; perf-critical simplification without user OK; module will be rewritten soon.

## Principles

1. Preserve behavior — if unsure, do not change it.
2. Match repo conventions and neighboring code.
3. Clarity over cleverness — explicit beats dense.
4. Balance — keep helpers that name concepts; keep test/extension abstractions.
5. Stay in pinned scope — no drive-by refactors.

## Structural complexity

| Signal | Typical action |
|--------|----------------|
| Nesting 3+ levels | Guard clauses or small named helpers |
| Function 50+ lines or multiple responsibilities | Split by responsibility with descriptive names |
| Nested ternaries | `if`/`else`, `switch`, or `Map` lookup |
| Boolean flag parameters (`save(entity, true, false)`) | Parameter object, overloads, or separate methods |
| Same condition repeated | Named predicate function |

## Names and noise

| Signal | Typical action |
|--------|----------------|
| Generic names (`data`, `result`, `temp`, `val`) | Rename to describe content |
| Non-universal abbreviations (`usr`, `cfg`, `btn`) | Use full words unless project uses the abbreviation |
| Name does not match behavior (e.g. `get` mutates) | Rename to actual behavior |
| Comment states the obvious ("increment counter") | Remove comment |
| Comment explains why (retries, platform quirk) | Keep |

## Redundancy and excess

| Signal | Typical action |
|--------|----------------|
| Same 5+ lines in multiple places | Extract shared function when duplication is real |
| Unreachable code, unused vars, commented-out blocks | Remove after confirming dead |
| Wrapper adds no naming or boundary value | Inline call to underlying API |
| Single-strategy "framework" (factory-of-one) | Direct call if scope allows |
| Redundant casts (`(String) name`) or `instanceof` chains | Remove when type is already known; prefer pattern matching where idiomatic |

## Do not

- Remove or weaken error handling for "cleaner" code
- Simplify code you do not understand
- Change public API contracts unless the user explicitly includes that in scope
- Refactor security-sensitive or integration-heavy paths without tests covering them
- Touch performance-critical blocks without user confirmation

## Quick example

```java
// Before: nested checks
Optional<Result> process(User user) {
    if (user != null) {
        if (user.isActive()) {
            if (user.hasPermission()) {
                return Optional.of(doWork(user));
            }
        }
    }
    return Optional.empty();
}

// After: guard clauses (same behavior)
Optional<Result> process(User user) {
    if (user == null) {
        return Optional.empty();
    }
    if (!user.isActive()) {
        return Optional.empty();
    }
    if (!user.hasPermission()) {
        return Optional.empty();
    }
    return Optional.of(doWork(user));
}
```
