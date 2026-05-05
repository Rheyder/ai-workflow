# Feedback loop — techniques to build signal

Reference for **Phase A** in `SKILL.md`. The loop is the product: without a fast, repeatable pass/fail run (ideally by agent), hypotheses and instrumentation cannot close root cause with confidence.

**Suggested order** — try in sequence; stop at the first that yields enough signal to iterate.

1. **Failing test** at the right seam — unit, integration, or e2e as fits the bug.
2. **Curl / HTTP script** against dev server.
3. **CLI with fixture** — fixed input, compare stdout to “good” snapshot.
4. **Headless browser** (Playwright / Puppeteer) — UI, DOM, console, network.
5. **Trace replay** — real request, payload, or log captured on disk; replay isolated path.
6. **Disposable harness** — minimal subset (one service, mocked deps), one call covers bug path.
7. **Property / fuzz** — when symptom is “sometimes wrong output”, hundreds or thousands of random inputs.
8. **Bisection harness** — bug appeared between two states (commit, dataset, version); automate “checkout X, verify, repeat” for `git bisect run`.
9. **Differential loop** — same input on old vs new version/config; diff outputs.
10. **HITL bash** — last resort: copy/edit [`scripts/hitl-loop.template.sh`](scripts/hitl-loop.template.sh); human runs steps; structured output returns to agent.

## Iterate on the loop itself

Treat the loop as product:

- **Faster** — setup cache, less init, narrower test scope.
- **Sharper signal** — assert exact symptom, not only “did not crash”.
- **More deterministic** — fixed time, RNG seed, isolated FS, frozen network.

A ~30s flaky loop is barely better than none; a ~2s deterministic loop is strong leverage.

## Non-deterministic bugs

Goal is not a “clean” repro but **higher reproduction rate**: repeat trigger serial/parallel, stress, timing windows, injected sleeps. Raise rate until debuggable; ~1% flake is often unusable.

## When no credible loop

Stop and say so explicitly. List what was tried. Ask user for: (a) environment where it reproduces, (b) artifact (HAR, log dump, core dump, timestamped recording), or (c) permission for temporary production instrumentation. **Do not advance to serious hypotheses without a loop that supports falsification.**
