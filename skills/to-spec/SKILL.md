---
name: to-spec
description: Turn a request into a short spec with scope, acceptance criteria, risks and verification. Not for breaking work into slices — use to-issues.
---

# To Spec

Transversal: [CONVENTIONS.md](../../CONVENTIONS.md).

## When to use

- User wants a spec from current discussion.
- **Spec phase** before `to-issues`.

## When not to use

- Tickets/slices only → `to-issues`
- Implementation → `tdd`

## Expected input

- Conversation context and repo understanding (explore if needed).
- Optional: planning location from `docs/ai-workflow/` or local `.scratch/<feature>/`.

Do **not** interview the user — synthesize what you already know.

## Process

1. Explore the repo if needed. Use domain glossary and ADRs in the area you touch.

2. Propose test seams at the highest practical level. Confirm with the user if seams are non-obvious.

3. Write the spec using the template below.

4. **Write** the spec to the configured planning location. If none exists, use `.scratch/<feature>/` (e.g. `PRD.md` or `spec.md`). Apply `ready-for-execution` status when using local markdown triage (see `docs/ai-workflow/triage-labels.md` if present).

**Default: short spec.** Expand sections only when risk, ambiguity, or the user justifies more detail.

<spec-template>

# Spec

## Problem

## Scope

## Out of scope

## Acceptance criteria

## Decisions

## Risks

## Verification

</spec-template>

## Expected output

- Short spec at the planning location (default local: `.scratch/<feature>/`).
- Ready for Plan phase (`to-issues`).

## Minimal checklist

- [ ] Problem and scope clear
- [ ] Acceptance criteria testable
- [ ] Risks and verification noted
- [ ] Written to planning location
