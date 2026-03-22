# Best Practices

Engineering best practices for development, testing, deployment, and operations across all **invitrek-swt** repositories.

## Table of Contents

- [Development Workflow](#development-workflow)
- [Code Reviews](#code-reviews)
- [Testing](#testing)
- [Security](#security)
- [Observability](#observability)
- [CI/CD](#cicd)
- [Dependency and Supply Chain](#dependency-and-supply-chain)
- [Incident Response](#incident-response)

---

## Development Workflow

- Work in short-lived feature branches; merge to `main` frequently.
- Keep the `main` branch always deployable.
- Use feature flags to ship incomplete features safely.
- Link every pull request to a related issue for traceability.
- Do not commit directly to `main` or `release` branches.

---

## Code Reviews

### For Authors

- Keep pull requests small and focused (aim for < 400 lines of diff).
- Write a clear PR description explaining *what* changed and *why*.
- Self-review your diff before requesting review.
- Annotate non-obvious changes with comments in the PR.
- Address all review comments and mark them resolved.

### For Reviewers

- Review within **2 business days** of assignment.
- Understand the intent before critiquing the implementation.
- Check for correctness, security, performance, and maintainability.
- Distinguish blocking issues from optional suggestions (prefix optional suggestions with `nit:`).
- Approve only when you are satisfied the change is safe and correct.
- Do not approve PRs with failing CI checks unless there is a documented reason.

---

## Testing

### Strategy

Follow the testing pyramid:

1. **Unit tests** — Fast, isolated, testing a single function or class. Should make up the majority of tests.
2. **Integration tests** — Test interactions between components (e.g., service + database).
3. **End-to-end tests** — Test complete user flows through the system. Keep these few and valuable.

### Requirements

- Every new feature must include unit tests.
- Every bug fix must include a regression test.
- Aim for **>80% line coverage** for application code; coverage alone is not a quality metric — meaningful assertions matter.
- Tests must be deterministic (no flakiness). Flaky tests must be fixed immediately or quarantined and tracked in an issue.
- Tests must not depend on execution order.
- Use test fixtures and factories to create test data; do not share mutable state between tests.
- Mock external services and I/O in unit tests; use real integrations in integration tests with a test database.

### Naming

- Test names should read as sentences: `it should return 404 when user is not found`.
- Group related tests logically (e.g., `describe('UserService', ...)` or test class `TestUserService`).

---

## Security

### Secrets Management

- **Never** commit credentials, API keys, tokens, or passwords to source code.
- Use environment variables loaded from a secrets manager (e.g., GitHub Actions secrets, HashiCorp Vault, AWS Secrets Manager).
- Rotate secrets regularly and immediately upon suspected compromise.

### Input Validation and Output Encoding

- Validate and sanitize all external inputs at the application boundary.
- Encode outputs appropriately for the context (HTML, SQL, shell, etc.) to prevent injection attacks.
- Use parameterized queries or an ORM for all database interactions — never interpolate user input into SQL.

### Authentication and Authorization

- Use industry-standard protocols: OAuth 2.0 / OIDC for authentication; RBAC or ABAC for authorization.
- Enforce the principle of least privilege for service accounts and users.
- Require MFA for all human user accounts with production access.
- Never roll your own cryptography; use well-audited libraries.

### Dependency Security

- Scan dependencies for known vulnerabilities in CI (e.g., `npm audit`, `pip-audit`, `govulncheck`, Dependabot).
- Do not merge PRs that introduce high or critical vulnerability alerts without a documented remediation plan.
- Review the source and license of all new third-party dependencies before adding them.

### GitHub Security Features

Enable these features on every repository:
- Dependabot version updates
- Dependabot security updates
- Code scanning (CodeQL or equivalent)
- Secret scanning with push protection

---

## Observability

Every production service must implement the three pillars of observability:

### Logs

- Use structured logging (JSON) with consistent field names: `timestamp`, `level`, `service`, `trace_id`, `message`.
- Log at the appropriate level: `DEBUG` for development detail, `INFO` for significant events, `WARN` for recoverable anomalies, `ERROR` for failures.
- Do not log sensitive information (PII, credentials, tokens).

### Metrics

- Expose application metrics (request rate, error rate, latency) using Prometheus or a compatible format.
- Define SLIs and SLOs for critical user journeys.
- Set up alerting rules based on SLO burn rates.

### Traces

- Instrument services with distributed tracing (OpenTelemetry).
- Propagate trace context across service boundaries via standard headers.
- Sample traces appropriately; aim for 100% sampling of error traces.

---

## CI/CD

- All tests and lint checks must run on every pull request.
- The `main` branch must always be in a deployable state.
- Deployments to production must be preceded by a successful deployment to a staging environment.
- Use immutable artifact versioning — build once, promote the same artifact through environments.
- Implement automated rollback triggered by error rate or latency SLO breaches.
- Keep CI pipelines fast (target < 10 minutes for PR checks); parallelize independent jobs.
- Treat CI configuration as code — review changes to CI workflows with the same rigor as application code.

---

## Dependency and Supply Chain

- Pin dependencies to specific versions in CI and production environments.
- Commit lock files (`package-lock.json`, `poetry.lock`, `go.sum`, etc.) to version control.
- Verify the integrity of downloaded artifacts (checksums, sigstore, etc.) in CI where possible.
- Prefer dependencies with clear ownership, active maintenance, and permissive or compatible licenses.

---

## Incident Response

1. **Detect** — Monitor dashboards and alerts proactively.
2. **Triage** — Assess severity and impact; declare an incident if users are affected.
3. **Communicate** — Notify stakeholders early and update frequently.
4. **Mitigate** — Prioritize restoring service over finding root cause.
5. **Resolve** — Apply a permanent fix once service is restored.
6. **Post-mortem** — Conduct a blameless post-mortem within 5 business days. Document findings and action items in the repository under `docs/postmortems/`.

Incident severity levels:

| Severity | Definition |
|----------|-----------|
| P0 | Complete outage or data loss affecting all users |
| P1 | Significant degradation affecting many users |
| P2 | Partial degradation or feature unavailability |
| P3 | Minor issue with a known workaround |
