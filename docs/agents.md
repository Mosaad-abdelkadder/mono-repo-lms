# AI Agent Roles & Responsibilities (agents.md)

This document defines the specialized roles for AI Agents (such as those running in Kilo Code, Gemini-CLI, AntiGravity, Cursor, or autonomous pipelines) collaborating on the Gamified LMS platform.

## 1. Frontend Agent (Next.js / UI)

**Role:** Implement user interfaces, integrate with backend APIs, and manage client-side state.
**Directives:**

- Use **Next.js App Router** and **React Server Components** for optimized rendering.
- Apply **Tailwind CSS** and **shadcn-ui** for consistent, accessible design.
- Translate Figma designs (via tools like Google Stitch) into reusable React components.
- Implement robust error handling and loading states (Suspense).
- Ensure mobile responsiveness and accessibility (WCAG compliance).

## 2. Backend Agent (NestJS / Node.js)

**Role:** Develop the core business logic microservices (User, Course, Community).
**Directives:**

- Use **NestJS** decorators, modules, and dependency injection.
- Implement strictly typed **TypeScript** interfaces for DTOs and entities.
- Integrate with **PostgreSQL** (via TypeORM/Prisma) for relational data and **MongoDB** for document storage.
- Expose RESTful or GraphQL APIs.
- Publish and subscribe to events via **Redpanda / NATS JetStream**.
- Write comprehensive unit and E2E tests using Jest.

## 3. High-Performance Backend Agent (Go / Rust)

**Role:** Develop high-throughput, low-latency microservices (Gamification, Leaderboards, Data Ingestion, ATS parsing).
**Directives:**

- For **Go**: Utilize goroutines and channels for concurrent event processing. Focus on memory efficiency and fast startup times.
- For **Rust**: Leverage the ownership model for safe, fearless concurrency. Optimize CPU-bound tasks like resume parsing or complex analytical algorithms.
- Connect to **Redis** for real-time leaderboards and caching.
- Write data to **ClickHouse** for high-speed analytics.
- Implement structured logging and OpenTelemetry tracing.

## 4. AI Integration Agent

**Role:** Implement LLM features, RAG pipelines, and intelligent matching algorithms.
**Directives:**

- Develop the **AI Mock Interview** workflow (speech-to-text, LLM prompt engineering, evaluation, text-to-speech).
- Build the **ATS Resume Detection** system (parsing PDFs, generating embeddings, storing in Vector DBs like pgvector/Milvus).
- Implement the semantic search for the Course Engine.
- Monitor and track LLM token utilization (cost management).
- Optimize prompts for latency and accuracy.

## 5. DevOps & Infrastructure Agent

**Role:** Manage the Kubernetes deployment, API Gateway, and CI/CD pipelines.
**Directives:**

- Configure **Envoy Gateway** (Kubernetes Gateway API) for routing, rate limiting, and SSL termination.
- Write Helm charts or Kubernetes manifests (Deployments, Services, ConfigMaps, Secrets) for all microservices.
- Ensure the MVP can run locally via **Kubesolo** or Minikube.
- Design the production deployment strategy for high scalability (EKS/GKE).
- Set up monitoring dashboards (Prometheus, Grafana, OpenTelemetry).

## 6. Data Engineering Agent

**Role:** Build and maintain the data pipeline for analytics and reporting.
**Directives:**

- Configure **Redpanda/NATS JetStream** for event streaming.
- Use **DuckDB** for fast, embedded, in-memory analytical transformations during data ingestion.
- Design **ClickHouse** schemas (MergeTree tables) for storing massive volumes of event logs (XP, watch time, platform analytics).
- Build the data layer for the **HR Connect** dashboard (aggregating user skills, course completions, and interview scores).

## 7. Quality Assurance (QA) Agent

**Role:** Ensure code quality, security, and performance across the entire platform.
**Directives:**

- Run static analysis and linting (ESLint, Prettier, golangci-lint, clippy).
- Execute security scanning (dependency checks, SAST/DAST).
- Perform load testing (k6, JMeter) on critical endpoints (Gateway, Leaderboards).
- Verify End-to-End user flows (Playwright/Cypress) for course enrollment, video playback, and mock interviews.
