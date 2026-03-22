# Contributing to invitrek-swt Projects

Thank you for your interest in contributing! These guidelines apply to all repositories under the **invitrek-swt** organization unless a repository provides its own `CONTRIBUTING.md`.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [How to Contribute](#how-to-contribute)
- [Branching Strategy](#branching-strategy)
- [Commit Messages](#commit-messages)
- [Pull Requests](#pull-requests)
- [Code Review](#code-review)
- [Reporting Bugs](#reporting-bugs)
- [Requesting Features](#requesting-features)

---

## Code of Conduct

All contributors must follow our [Code of Conduct](CODE_OF_CONDUCT.md). Please read it before participating.

---

## Getting Started

1. **Fork** the repository (external contributors) or **clone** it directly (team members).
2. Create a feature branch from `main` (see [Branching Strategy](#branching-strategy)).
3. Make your changes, following our [Coding Standards](docs/coding-standards.md).
4. Write or update tests as needed.
5. Ensure all tests pass locally before opening a pull request.
6. Open a pull request against `main`.

---

## How to Contribute

### Reporting Bugs

Use the **Bug Report** issue template. Include:
- A clear title and description
- Steps to reproduce
- Expected vs. actual behavior
- Environment details (OS, runtime version, etc.)

### Requesting Features

Use the **Feature Request** issue template. Include:
- The problem the feature solves
- A proposed solution or approach
- Any alternatives considered

### Submitting Code

All code contributions must go through a pull request. Direct pushes to `main` are not permitted.

---

## Branching Strategy

| Branch type | Pattern | Purpose |
|-------------|---------|---------|
| Feature | `feature/<short-description>` | New functionality |
| Bug fix | `fix/<short-description>` | Bug fixes |
| Chore | `chore/<short-description>` | Maintenance, dependency updates |
| Release | `release/<version>` | Release preparation |
| Hotfix | `hotfix/<short-description>` | Critical production fixes |

Branch from `main` and merge back to `main` via pull request.

---

## Commit Messages

Follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
<type>(<scope>): <short summary>

[optional body]

[optional footer(s)]
```

**Types:** `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`

**Examples:**
```
feat(auth): add OAuth2 login support
fix(api): handle null response from user endpoint
docs(readme): update installation instructions
```

- Use the imperative mood in the summary ("add feature", not "added feature").
- Keep the summary line under 72 characters.
- Reference related issues in the footer: `Closes #123` or `Refs #456`.

---

## Pull Requests

- Fill in the pull request template completely.
- Keep PRs focused — one logical change per PR.
- Link the PR to a related issue when applicable.
- Add reviewers from the appropriate team.
- Ensure CI checks pass before requesting review.
- Rebase or merge `main` into your branch if it has diverged significantly.

---

## Code Review

**Authors:**
- Respond to all review comments before merging.
- Mark comments as resolved once addressed.
- Avoid force-pushing after review has begun; use additional commits instead.

**Reviewers:**
- Review within 2 business days of assignment.
- Be constructive and specific — explain the *why* behind suggestions.
- Distinguish between blocking issues and optional nits (prefix with `nit:`).
- Approve only when you are confident the change is correct and safe.

See the full [Code Review Best Practices](docs/best-practices.md#code-reviews) for details.

---

## Questions?

Open a [Discussion](../../discussions) or contact the team via the channels listed in [SUPPORT.md](SUPPORT.md).
