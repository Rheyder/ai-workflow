# Slice verification report template

Use for **light** per-slice checks after TDD GREEN. Full branch reports use `finishing-a-development-branch/verification-report.md`.

## Template

```markdown
## Slice verification

**Slice:** <name or N>
**Branch:** <branch>
**Date:** <YYYY-MM-DD>

## Expected behavior (this slice)

- <behavior 1>
- ...

## Commands run

| Command | Exit | Result |
|---------|------|--------|
| `<command>` | 0 / non-zero | <summary> |

## Direct behavior tests

| Target | Scenario | Outcome |
|--------|----------|---------|
| ... | happy path | pass / fail |

## Issues found

- (or: None)

## Gaps (not verified)

- <what> — <why>

## Confidence

**Level:** high | medium | low

## Recommendation

- [ ] announce slice ready to commit (`git-workflow-and-versioning`)
- [ ] fix then re-verify slice
- [ ] invoke diagnose
```
