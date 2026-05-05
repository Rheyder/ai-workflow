---
name: brainstorming
description: Use when the user needs design or a spec before code; mentions brainstorming, new feature, stress-test or plan grill; wants alignment with domain glossary and ADRs; or asks for PRD/spec from conversation only without new interview questions.
---

# Brainstorming

## Overview

Move from intent to **accepted interface-design / spec** before implementation. Works with glossary, ADRs, and [workflow-conventions.md](../../reference/workflow-conventions.md) for file placement.

## Hard gate

No production code and no “closed” handoff to implementation until the user **accepts** the spec (or PRD-only outcome) in the chat — including small tasks (approval can be short).

## When to use

- New feature, behavior change, or architecture choice needing agreement.
- Need stress-testing or domain alignment before `plan-writing`.

## When not to use

- **Closed spec already accepted** and user only wants `plan-*` → `plan-writing`.
- **Pure how-to** on existing behavior (read code / docs) without new design — no gate, but don’t scope-creep into spec.

## Entry modes

| Signal | Mode |
|--------|------|
| PRD/spec from chat only, “don’t interview” | **PRD** — [REFERENCE.md](REFERENCE.md); use repo facts, minimal new questions. |
| grill / stress-test / challenge plan | **Grill** — one question at a time; after each: **recommendation** + decision tree until closed. |
| Glossary / ADRs / domain docs relevant | **Domain** — section [Domain and documentation](#domain-and-documentation) on every pass. |
| Default | **Standard flow** below. |

If the answer is in the **repository**, read it instead of asking.

## Standard flow (order)

1. **Profile / track** — infer or confirm hotfix/express/full; express stays short.
2. **Project context** — files, docs, commits; decompose independent subsystems before deep-diving one.
3. **Visual companion** — if UI/layout questions dominate, offer once ([visual-companion.md](visual-companion.md)); if declined, text only.
4. **Clarifications** — one question per message; multiple choice when possible; nail purpose, constraints, success metric.
5. **Approaches** — 2–3 options + trade-offs + **grounded** recommendation.
6. **Design** — sections scale with complexity; validate **by section** (architecture, components, data flow, errors, tests). Walk back if seams are muddy.
7. **Write spec** — path per [workflow-conventions.md](../../reference/workflow-conventions.md); full track → PRD-ish sections in [REFERENCE.md](REFERENCE.md); express → shorter bullets.
8. **Self-review** — placeholders, contradictions, ambiguity, scope; fix inline.
9. **User gate** — explicit review of file; then close brainstorming.
10. **Next step** — point to `plan-*` / track under `.cursor`; **do not implement here.**

Optional: [spec-document-reviewer-prompt.md](spec-document-reviewer-prompt.md).

## Domain and documentation

When glossary/context map/ADRs exist (see [workflow-conventions.md](../../reference/workflow-conventions.md)):

- **Conflict** with glossary → call out immediately.
- **Precision** — use canonical terms.
- **Scenarios** — stress relationships and edge cases.
- **Code vs narrative** — state contradictions plainly.
- **Update glossary** as terms stabilize — [CONTEXT-FORMAT.md](CONTEXT-FORMAT.md).
- **ADR** only for hard-to-reverse, surprising trade-offs — [ADR-FORMAT.md](ADR-FORMAT.md).

## Grill mode

Each question includes your **recommended answer**; **one at a time**; wait for reply; continue until the decision tree is closed.

## Materialization and Git

Write spec to disk with a stable name. **Commit/push** — user only (`.cursor/rules/commit-push-hitl.mdc`).

## Principles

One question per message when clarifying. YAGNI. Alternatives before locking. In existing codebases, match local patterns; surgical edits only for the current goal.
