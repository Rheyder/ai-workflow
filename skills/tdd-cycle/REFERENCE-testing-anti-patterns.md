# Testing anti-patterns (consolidated excerpt)

**Load when:** creating mocks, test helpers, or feeling tempted to expose methods “for tests only”. Consolidated practice for this repo.

## Single principle

The test must prove **real** behavior of the unit under test through valid contracts — not behavior that exists only in doubles/mocks.

## Three iron rules

1. **Never assert mock call/view counts when that only proves the mock exists** — ask: “are we testing behavior useful to the module’s caller or the mock?”
2. **Never add methods used only by tests in production classes/files** → move teardown/setup to test utilities.
3. **Never mock a dependency without understanding the real contract** — otherwise you only encode assumptions.

## Quick gate (before mock asserts)

```
Before asserting on mock element:
→ Does the assertion prove observable behavior through the API under test?
   NO → delete/alternate assertion OR stop mocking that detail
```

## TDD closes these gaps

`tdd-cycle` with a well-observed failure and enough integration avoids mock pyramids where “green” comes only from mock expectations without touching real productive behavior.
