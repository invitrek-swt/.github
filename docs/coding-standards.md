# Coding Standards

These standards apply to all repositories under the **invitrek-swt** organization. Individual repositories may extend (but not reduce) these standards with language- or project-specific additions.

## Table of Contents

- [General Principles](#general-principles)
- [Naming Conventions](#naming-conventions)
- [File and Directory Structure](#file-and-directory-structure)
- [Documentation and Comments](#documentation-and-comments)
- [Language-Specific Standards](#language-specific-standards)
  - [Python](#python)
  - [JavaScript / TypeScript](#javascript--typescript)
  - [Go](#go)
  - [Java / Kotlin](#java--kotlin)
- [Linting and Formatting](#linting-and-formatting)
- [Dependency Management](#dependency-management)

---

## General Principles

1. **Clarity over cleverness** — Write code that a new team member can understand without context.
2. **Consistency** — Follow the conventions of the file and project you are editing.
3. **Single Responsibility** — Functions, classes, and modules should do one thing well.
4. **DRY (Don't Repeat Yourself)** — Abstract duplicated logic; but prefer clear duplication over a premature or poorly named abstraction.
5. **Fail fast** — Validate inputs early and return or raise errors as soon as a problem is detected.
6. **No magic numbers or strings** — Use named constants instead of inline literals.

---

## Naming Conventions

| Element | Convention | Example |
|---------|-----------|---------|
| Variables | `camelCase` (JS/TS), `snake_case` (Python/Go) | `userName`, `user_name` |
| Constants | `UPPER_SNAKE_CASE` | `MAX_RETRY_COUNT` |
| Classes / Types | `PascalCase` | `UserRepository` |
| Functions / Methods | `camelCase` (JS/TS/Java), `snake_case` (Python/Go) | `getUserById`, `get_user_by_id` |
| Files | `kebab-case` (web), `snake_case` (Python/Go), `PascalCase` for classes (Java) | `user-service.ts`, `user_service.py` |
| Interfaces / Protocols | `PascalCase`, no `I` prefix | `UserRepository` (not `IUserRepository`) |
| Boolean variables | Prefix with `is`, `has`, `can`, or `should` | `isActive`, `hasPermission` |
| Test files | Mirror source file name with `.test.`, `.spec.`, or `_test` suffix | `user-service.test.ts`, `test_user_service.py` |

Names must be meaningful and unambiguous. Single-letter variable names are acceptable only in short loop indices or mathematical contexts.

---

## File and Directory Structure

- Keep files short and focused. A file above ~400 lines is a signal to consider splitting.
- Group related files in directories named after the domain concept they represent.
- Avoid deep nesting — prefer flat structures where possible.
- Place tests alongside (or near) the source files they test.

```
src/
  users/
    user.model.ts
    user.service.ts
    user.service.test.ts
    user.repository.ts
  orders/
    ...
```

---

## Documentation and Comments

- **Public APIs** (functions, classes, modules) must have doc-comments describing purpose, parameters, return values, and exceptions/errors.
- **Inline comments** should explain *why*, not *what*. If a comment is needed to explain what the code does, consider refactoring instead.
- Keep comments up to date. Stale comments are worse than no comments.
- Use `TODO(username): description` for deferred work; open a tracking issue and reference it.

---

## Language-Specific Standards

### Python

- Target **Python 3.10+**.
- Follow [PEP 8](https://peps.python.org/pep-0008/) for style.
- Use [Black](https://black.readthedocs.io/) for formatting (line length: 88).
- Use [Ruff](https://docs.astral.sh/ruff/) for linting.
- Use type hints for all function signatures ([PEP 484](https://peps.python.org/pep-0484/)).
- Use `pathlib.Path` instead of `os.path`.
- Prefer `dataclasses` or `pydantic` models over plain dicts for structured data.

### JavaScript / TypeScript

- Prefer **TypeScript** over plain JavaScript for all new projects.
- Use [ESLint](https://eslint.org/) with the organization's shared config.
- Use [Prettier](https://prettier.io/) for formatting (2-space indent, single quotes, trailing commas in multi-line).
- Avoid `any`; use `unknown` and narrow types explicitly.
- Use `async/await` over raw Promises and `.then()` chains.
- Prefer `const` over `let`; never use `var`.
- Use ES modules (`import`/`export`), not CommonJS (`require`/`module.exports`), in new code.

### Go

- Follow the [Effective Go](https://go.dev/doc/effective_go) guidelines.
- Run `gofmt` / `goimports` before every commit.
- Use `golangci-lint` with the project's `.golangci.yaml` configuration.
- Return errors explicitly; never panic in library code.
- Use the standard `log/slog` package for structured logging.
- Write table-driven tests using the standard `testing` package.

### Java / Kotlin

- Target **Java 17 LTS** or **Kotlin 1.9+** for new projects.
- Follow [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html) for Java.
- Use [ktlint](https://ktlint.github.io/) for Kotlin formatting.
- Prefer constructor injection for dependencies (Spring, Guice, etc.).
- Use `Optional<T>` to represent nullable return values in public APIs (Java); use nullable types in Kotlin.
- Mark classes and methods `final` by default unless designed for extension.

---

## Linting and Formatting

Every repository must have linting and formatting checks that run in CI. Pull requests must pass all checks before merging.

- Formatting changes must be made via the formatter, not by hand.
- Lint warnings should be treated as errors in CI.
- Suppressions (e.g., `# noqa`, `// eslint-disable`) must include a justification comment.

---

## Dependency Management

- Pin dependencies to exact or minor-version ranges in production code.
- Use a lock file (`package-lock.json`, `poetry.lock`, `go.sum`, etc.) and commit it.
- Enable [Dependabot](https://docs.github.com/en/code-security/dependabot) for automated dependency updates.
- Do not add a dependency when the functionality can be implemented with a few lines using the standard library.
- Prefer well-maintained, widely-used packages. Evaluate security posture before adding new dependencies.
