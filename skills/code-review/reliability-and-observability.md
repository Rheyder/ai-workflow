# Reliability & Observability

Focus: logging, metrics, errors, queues, workers, and production behaviour.

## Checks

- [ ] Errors logged with enough context (correlation id, operation, safe metadata)
- [ ] No swallowed exceptions without logging or rethrow where appropriate
- [ ] Log levels appropriate (no debug noise in hot paths; errors not downgraded to info)
- [ ] Metrics or tracing hooks for new critical paths where the repo already uses them
- [ ] Background jobs: retry policy, dead-letter or failure visibility, idempotency
- [ ] Queue/worker changes handle partial failure and duplicate delivery
- [ ] Resource limits: unbounded loops, unbounded fetches, or missing pagination on lists
- [ ] Timeouts and cancellation on long-running or external operations
- [ ] Performance: no obvious regression in hot paths (sync I/O, large allocations in loops)

If nothing in the diff touches this dimension: report `N/A — no applicable changes`.
