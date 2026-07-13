# Integrations & Data

Focus: HTTP/API contracts, external clients, databases, migrations, and transactions.

## Checks

- [ ] API contract changes backward-compatible or versioned/breaking change documented
- [ ] Request/response shapes, status codes, and error payloads consistent with existing API style
- [ ] HTTP clients: timeouts, retries, and backoff configured appropriately
- [ ] Idempotency for retried or duplicate requests where side effects matter
- [ ] External failures handled; callers get actionable errors
- [ ] Database migrations safe (reversible or rollout plan noted); no destructive surprise on deploy
- [ ] Transactions scope correct; no long-held locks or partial commits
- [ ] Queries efficient for changed paths (no obvious N+1 in new code)
- [ ] Data integrity: constraints, uniqueness, and FK usage where required
- [ ] Compatibility with existing consumers of changed integration points

If nothing in the diff touches this dimension: report `N/A — no applicable changes`.
