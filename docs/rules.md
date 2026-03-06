# System Rules & Architectural Guidelines (rules.md)

This document establishes the foundational architectural constraints, coding standards, and best practices for developing the Gamified LMS platform. It serves as a strict rulebook for all human developers and AI Agents.

## 1. Architectural Constraints

### 1.1 Microservices & Domain-Driven Design

- **Rule:** The system must be composed of loosely coupled, independently deployable microservices.
- **Rule:** Each microservice must own its data store (Polyglot Persistence). A service must not directly access another service's database. All inter-service data access must occur via API calls (REST/gRPC) or asynchronous events.
- **Rule:** Avoid Distributed Monoliths. If two services are too chatty, re-evaluate the bounded contexts.

### 1.2 Event-Driven Architecture

- **Rule:** Use asynchronous messaging (Redpanda or NATS JetStream) for state changes that affect multiple domains (e.g., "Course Completed" -> Updates Gamification XP, Notifies HR Service).
- **Rule:** Design events as immutable facts. Include necessary context in the payload to reduce follow-up API calls (Event-Carried State Transfer).
- **Rule:** Ensure idempotency in event consumers. Processing the same event twice must not alter the final state.

### 1.3 API Gateway & Routing

- **Rule:** All external traffic must flow through the **Envoy Gateway** (implementing the Kubernetes Gateway API).
- **Rule:** The Gateway handles cross-cutting concerns: JWT validation, rate limiting, and SSL termination. Microservices should assume requests arriving from the Gateway are authenticated, but must still enforce authorization (RBAC).

## 2. Coding Standards

### 2.1 TypeScript (Next.js & NestJS)

- **Rule:** Use `strict: true` in `tsconfig.json`. No implicit `any`.
- **Rule:** Prefer Interfaces over Types for object shapes.
- **Rule:** Use meaningful, descriptive variable and function names. Avoid abbreviations.
- **Rule:** In NestJS, business logic must reside in Services, not Controllers. Controllers only handle HTTP routing and DTO validation.

### 2.2 Go (Golang)

- **Rule:** Adhere to effective Go idioms (`gofmt`, `golint`).
- **Rule:** Handle all errors explicitly. Do not use blank identifiers (`_`) to ignore errors returned by functions.
- **Rule:** Use Context (`context.Context`) for cancellation and timeout propagation across API boundaries and goroutines.
- **Rule:** Prefer standard library packages unless a third-party module offers significant, proven benefits.

### 2.3 Rust

- **Rule:** Adhere to Rust best practices (use `cargo fmt` and `clippy`).
- **Rule:** Prioritize memory safety and fearless concurrency by fully leveraging the ownership model.
- **Rule:** Keep `unsafe` blocks to an absolute minimum, thoroughly documented, and isolated.

## 3. Database & Data Engineering Rules

- **PostgreSQL:** Use migrations for schema changes. Enforce referential integrity where applicable. Use JSONB only for truly unstructured data; otherwise, use strict columns.
- **MongoDB:** Design documents based on access patterns (read/write frequency), not just relational normalization.
- **ClickHouse:** Optimize tables with appropriate `ORDER BY` clauses (primary keys). Minimize mutations (`UPDATE`/`DELETE`); ClickHouse is designed for immutable inserts (append-only logs). Use DuckDB for staging/transformations if necessary.
- **Vector DBs:** Always store metadata alongside embeddings to allow hybrid search capabilities (e.g., filtering by "Docker Course" while performing semantic search).

## 4. AI & LLM Integration Guidelines

- **Rule:** Treat LLM outputs as untrusted input. Always validate JSON responses using tools like Zod before passing them to the core system.
- **Rule:** Implement strict token tracking and rate limiting for all LLM calls to prevent runaway costs.
- **Rule:** For sensitive user data (Resumes), ensure PII is anonymized or handled securely according to data privacy regulations (GDPR/CCPA) before sending it to external AI models.

## 5. Deployment & Kubernetes Guidelines

- **Rule:** Infrastructure as Code (IaC). All K8s manifests must be stored in Git (GitOps workflow).
- **Rule:** Microservices must be stateless (session state stored in Redis).
- **Rule:** Define Resource Requests and Limits (CPU/Memory) for all containers.
- **Rule:** Implement Health Checks (Liveness and Readiness probes) for every service.
- **Rule:** MVP must run seamlessly on **Kubesolo** to ensure a low barrier to entry for local development.

## 6. Testing Strategy

- **Rule:** Every new feature requires automated tests (Unit, Integration, or E2E).
- **Rule:** Aim for high code coverage (e.g., >80%) on core business logic (Course progression, XP calculation, AI parsing).
- **Rule:** Tests must be deterministic and not rely on external, unpredictable state. Use mocks or test containers (e.g., for Postgres/Redis).
