# Design Guidelines

These guidelines govern architecture, API design, and UI/UX decisions across all **invitrek-swt** projects.

## Table of Contents

- [Architecture Principles](#architecture-principles)
- [API Design](#api-design)
  - [RESTful APIs](#restful-apis)
  - [GraphQL APIs](#graphql-apis)
  - [gRPC / Protocol Buffers](#grpc--protocol-buffers)
- [Data Modeling](#data-modeling)
- [Asynchronous and Event-Driven Design](#asynchronous-and-event-driven-design)
- [Service Boundaries and Modularity](#service-boundaries-and-modularity)
- [UI / UX Guidelines](#ui--ux-guidelines)
- [Database Design](#database-design)

---

## Architecture Principles

1. **Separation of concerns** — Keep business logic, data access, and presentation layers distinct.
2. **Loose coupling, high cohesion** — Services and modules should have well-defined interfaces and minimal dependencies on each other's internals.
3. **Design for failure** — Assume that network calls, external services, and I/O operations will fail. Use retries with exponential back-off, circuit breakers, and graceful degradation.
4. **Observability** — Every service must emit structured logs, metrics, and distributed traces. If you cannot observe it, you cannot operate it.
5. **Twelve-Factor App** — Follow [12factor.net](https://12factor.net/) principles for cloud-native applications.
6. **Document decisions** — Use Architectural Decision Records (ADRs) stored in `docs/adr/` to document significant design choices and their rationale.

---

## API Design

### RESTful APIs

- Follow REST constraints: stateless, resource-oriented, uniform interface.
- Use **nouns** for resource paths, not verbs: `/users/{id}` not `/getUser`.
- Use standard HTTP methods appropriately:

  | Action | Method | Example |
  |--------|--------|---------|
  | Read list | `GET` | `GET /users` |
  | Read single | `GET` | `GET /users/{id}` |
  | Create | `POST` | `POST /users` |
  | Replace | `PUT` | `PUT /users/{id}` |
  | Partial update | `PATCH` | `PATCH /users/{id}` |
  | Delete | `DELETE` | `DELETE /users/{id}` |

- Use standard HTTP status codes. Do not return `200 OK` with an error body.
- Version APIs from the start: `/v1/users`. Use URL versioning for public APIs.
- Paginate collection responses; use cursor-based pagination for large or frequently changing datasets.
- Use consistent envelope formats for responses:
  ```json
  {
    "data": { ... },
    "meta": { "total": 100 }
  }
  ```
  And for errors:
  ```json
  {
    "error": {
      "code": "VALIDATION_ERROR",
      "message": "The 'email' field is required.",
      "details": [...]
    }
  }
  ```
- Document all APIs with OpenAPI 3.x specifications stored in the repository.

### GraphQL APIs

- Define a single, coherent schema; avoid field explosion.
- Use connections (Relay cursor-based pagination) for list fields.
- Avoid N+1 query problems — use DataLoaders.
- Provide meaningful error types; use the `errors` array, not HTTP error codes.
- Version via deprecation, not breaking schema changes.

### gRPC / Protocol Buffers

- Store `.proto` files in a dedicated `proto/` directory.
- Follow [Google's API Design Guide](https://cloud.google.com/apis/design) for service and method naming.
- Use semantic versioning for proto packages: `myservice.v1`.
- Maintain backward compatibility — add fields, do not remove or renumber them.

---

## Data Modeling

- Use UUIDs (v4 or v7) for primary keys in distributed systems; use auto-incrementing integers only in single-database setups.
- Store dates and times in UTC; use ISO 8601 format in APIs.
- Prefer explicit field names over generic ones (`created_at` over `date`, `user_id` over `id`).
- Avoid storing derived data that can be computed from existing fields, unless caching for performance is justified and documented.
- Validate data at the boundary (API layer) and enforce constraints at the database level.

---

## Asynchronous and Event-Driven Design

- Use message queues or event buses for cross-service communication that does not require an immediate response.
- Define event schemas with a registry or schema file (e.g., Avro, Protobuf, or JSON Schema).
- Include in every event: `event_type`, `event_id`, `timestamp`, `version`, and a `payload`.
- Design consumers to be idempotent — events may be delivered more than once.
- Use dead-letter queues (DLQ) for messages that cannot be processed after a configurable number of retries.
- Document event flows with sequence or event-flow diagrams.

---

## Service Boundaries and Modularity

- Define service boundaries around business domains, not technical layers.
- Each service owns its data; do not share databases between services.
- Expose capabilities through well-defined interfaces (APIs or events).
- Keep services small enough to be understood by a single team.
- Prefer an internal module/package over a separate service until the team structure or scalability demands otherwise.

---

## UI / UX Guidelines

- Follow [WCAG 2.1 AA](https://www.w3.org/WAI/WCAG21/quickref/) accessibility standards for all user-facing interfaces.
- Use a design system or component library consistently across the product.
- Provide meaningful loading states, empty states, and error states for every data-dependent UI component.
- Optimize for mobile-first responsive design.
- Validate user inputs on the client side for responsiveness, but always validate on the server side for correctness and security.
- Use skeleton screens over spinners for content loading to reduce perceived wait time.

---

## Database Design

- Write schema migrations as versioned, incremental scripts (e.g., Flyway, Alembic, golang-migrate).
- Every migration must be reversible unless a rollback is provably destructive and documented.
- Add database indices for all foreign keys and commonly queried columns; avoid over-indexing write-heavy tables.
- Use soft deletes (`deleted_at`) only when historical data must be preserved; document the rationale.
- Use connection pooling in application code; do not open unbounded connections.
- Test migrations against a copy of production data volume in staging before deploying.
