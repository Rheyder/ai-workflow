# Branch verification (Step 1)

Procedure and report template for `finishing-a-development-branch`. Follow [CONVENTIONS.md](../../CONVENTIONS.md#evidence-before-claims) (branch scope) before offering push/PR/merge.

## Iron law

Branch-specific: never substitute prior slice reports, "should pass", or diff review for Step 1. **RUN** must use full commands here, not only a prior slice report.

---

## Procedure

### 1. Understand expected behavior

Read issue/spec/PRD acceptance criteria. Map happy path, relevant error scenarios, and affected endpoints, services, jobs, DB, config, integrations.

**Stop if** scope is unclear — ask the user before running checks.

### 2. Start the application (when runtime checks apply)

Ensure the app starts. Check startup logs for errors, migration failures, misconfiguration.

**Stop if** the app cannot start — report blocker; do not offer git menu.

Skip only when the feature is fully covered by automated tests with no runtime surface — document the gap in the report.

### 3. Run automated checks

Detect commands from the repo (`package.json`, `pom.xml`, `build.gradle`, `Makefile`, `pyproject.toml`, CI config). Run **full** relevant test suite, build, lint/typecheck for the branch.

**Stop on failure** — see Failure routing.

Common patterns (use **only** what the repo defines — do not assume a stack):

| Stack | Where to look |
|-------|----------------|
| Node | `package.json` scripts |
| JVM | `pom.xml`, `build.gradle`, wrapper scripts |
| Go | module root, `Makefile` |
| Python | `pyproject.toml`, `Makefile`, `tox.ini` |

### 4. Test behavior directly

Exercise the feature through its public interface:

- **HTTP** — request against affected routes (CLI client, integration test, or project harness)
- **CLI** — valid/invalid inputs
- **UI** — invoke `frontend-testing` when browser validation is needed

Validate happy path. For APIs, cover at least two relevant error scenarios when applicable.

### 5. Check regressions

Probe adjacent behavior: neighboring endpoints, public contracts, migrations, queues, events, external calls.

### 6. Inspect runtime evidence

Review logs during exercised scenarios. Look for exceptions, stack traces, warnings, retries, timeouts. Confirm logs do not expose secrets.

### 7. Produce report

Output using the template below. Include recommendation: safe for push/PR or fix first.

**On failure:**

```
Branch verification failed. Cannot offer push/PR/merge until fixed:

[Failures and report summary]

Next: fix and re-run Step 1; use diagnose if cause is unclear.
```

**Do not proceed to git menu.**

---

## Failure routing

```
Validation failed
  ├─ Cause unclear → invoke diagnose (optional)
  ├─ Cause clear → fix, re-run Step 1
  └─ UI-only gap → invoke frontend-testing when available, then re-run Step 1
```

---

## Report template

One agent produces the full report after running checks.

### Rules

- List commands actually run with exit code and brief outcome
- Cite endpoints, scenarios, or CLI invocations tested
- State explicitly what was **not** verified and why
- Cap narrative; evidence over assumptions
- Confidence must match evidence (do not mark `high` with large gaps)
- No "ready for push/PR" without evidence per [CONVENTIONS.md](../../CONVENTIONS.md#evidence-before-claims)

### Template

```markdown
## Branch verification

**Feature / issue:** <name>
**Branch:** <branch>
**Base:** <main|develop|...>
**Date:** <YYYY-MM-DD>

## Expected behavior (acceptance criteria)

- <criterion 1>
- ...

## Commands run

| Command | Exit | Result |
|---------|------|--------|
| `<command>` | 0 / non-zero | <pass — N tests / fail — summary> |
| ... | | |

## Direct behavior tests

| Target | Scenario | Outcome |
|--------|----------|---------|
| `<method> <path>` or `<cli>` | happy path | pass / fail |
| ... | | |

## Regressions checked

- <adjacent behavior or contract> — pass / fail / not run — <reason if not run>

## Runtime evidence

- Logs: <clean / warnings / errors — cite relevant lines>
- Secrets in logs: none observed | concern — <detail>

## Issues found

- <issue> — severity — <evidence>
- (or: None)

## Gaps (not verified)

- <what> — <why skipped>

## Confidence

**Level:** high | medium | low

**Rationale:** <one or two sentences tied to evidence above>

## Recommendation

- [ ] proceed to git integration menu (push / PR / merge)
- [ ] fix then re-run Step 1
- [ ] invoke diagnose
```

---

## HTTP scenario examples

| Scenario | What to check |
|----------|----------------|
| GET happy path | 200, expected body shape, required fields |
| POST/PUT valid payload | 2xx, persisted side effect if applicable |
| Invalid payload | 4xx, error body, no partial corrupt state |
| Missing required field | 400/422 with clear error |
| Unauthorized / forbidden | 401/403 when auth applies |
| Not found | 404 for missing resource |

## Confidence guide

| Level | When to use |
|-------|-------------|
| **high** | All planned checks ran; happy path and required error scenarios pass; no open issues; regressions probed |
| **medium** | Core behavior verified but minor gaps |
| **low** | App did not run, tests skipped, or failures unresolved — do not offer push/PR menu |
