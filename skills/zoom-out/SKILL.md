---
name: zoom-out
description: Map modules one level up when unfamiliar with an area. Invoke by name. Toolbox — not on the default path.
disable-model-invocation: true
---

# Zoom Out

Transversal: [CONVENTIONS.md](../../CONVENTIONS.md).

## When to use

- Unfamiliar code area; need module map and callers.

## When not to use

- Implementing a slice — use `tdd` after map is enough.

## Process

Go up one abstraction layer. Map relevant modules and callers using domain glossary.

## Expected output

- Module/caller map; no implementation changes unless asked.
