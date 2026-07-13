# Security & Configuration

Focus: secrets, auth, input validation, dependencies, and environment safety.

## Checks

- [ ] User and external input validated and sanitized at boundaries
- [ ] No secrets, tokens, or credentials in code, logs, or committed config
- [ ] Environment variables and config used correctly (no prod values in dev paths)
- [ ] Authentication and authorization checked on new or changed endpoints/handlers
- [ ] SQL and queries parameterized; no string concatenation for user data
- [ ] Outputs encoded where needed (XSS, injection into templates/logs)
- [ ] Sensitive data not logged or exposed in error messages
- [ ] New or updated dependencies from trusted sources; no known critical CVEs in added deps
- [ ] Least privilege: permissions, scopes, and roles not broader than needed
- [ ] External data (APIs, webhooks, files) treated as untrusted until validated

If nothing in the diff touches this dimension: report `N/A — no applicable changes`.
