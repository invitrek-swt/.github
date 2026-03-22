# Security Policy

## Supported Versions

Only the latest stable release of each project is actively supported with security patches unless otherwise stated in a project's own `SECURITY.md`.

| Version | Supported |
|---------|-----------|
| Latest  | ✅        |
| Older   | ❌        |

## Reporting a Vulnerability

**Please do not report security vulnerabilities through public GitHub issues.**

If you discover a security vulnerability in any repository under the **invitrek-swt** organization, please report it using [GitHub's private security advisory feature](https://docs.github.com/en/code-security/security-advisories/guidance-on-reporting-and-writing/privately-reporting-a-security-vulnerability):

1. Navigate to the affected repository on GitHub.
2. Click **Security** → **Advisories** → **Report a vulnerability**.
3. Fill in the details of the vulnerability.

### What to Include

- Description of the vulnerability and its potential impact
- Steps to reproduce the issue
- Any proof-of-concept code or screenshots
- Affected versions (if known)
- Suggested remediation (if any)

### Response Timeline

| Action | Target timeframe |
|--------|-----------------|
| Initial acknowledgement | 2 business days |
| Severity assessment | 5 business days |
| Resolution or mitigation | Depends on severity (see below) |

| Severity | Target resolution |
|----------|------------------|
| Critical | 7 days |
| High | 14 days |
| Medium | 30 days |
| Low | 90 days |

## Security Best Practices

All contributors are expected to follow the security guidelines in [docs/best-practices.md](docs/best-practices.md#security).

Key requirements:
- Never commit credentials, API keys, or secrets to source code.
- Use environment variables or a secrets manager for sensitive configuration.
- Keep dependencies up to date; use Dependabot alerts.
- Enable GitHub Advanced Security features (code scanning, secret scanning) on all repositories.
