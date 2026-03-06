# System Specifications (specs.md)

## 1. System Overview

The Gamified Learning Management System (LMS) is a microservices-based platform designed for high scalability, performance, and engaging user experiences. It leverages modern architectural patterns to provide interactive courses, AI-driven features (mock interviews, resume ATS scoring), a community hub, and advanced analytics for HR connect.

## 2. Architecture Diagram & Pattern

The system uses an **Event-Driven Microservices Architecture**, coordinated by an API Gateway.

**Key Components:**

- **Client layer:** Next.js (React) SSR & CSR for learners, HR, and admins.
- **API Gateway:** Envoy Gateway (Kubernetes Gateway API) handling routing, rate limiting, and SSL termination.
- **Service Mesh:** Istio or Linkerd (future-proofing for highly scalable Kubernetes deployment).
- **Backend Microservices:** Domain-driven services for Users, Courses, Gamification, AI, and Analytics.
- **Event Bus / Message Broker:** High-throughput streaming platform.
- **Data Layer:** Polyglot persistence strategy optimized for specific microservice needs.

## 3. Technology Stack & Trade-off Analysis

### 3.1 Frontend

- **Framework:** Next.js (React)
- **Styling:** Tailwind CSS / shadcn-ui
- **State Management:** Zustand / React Query
- **Design:** Figma via Google Stitch for UI/UX translation.

### 3.2 Backend & Concurrency

The initial requirement mentioned NestJS, but high concurrency needs suggest evaluating Go and Rust.

- **NestJS (Node.js/TypeScript):** Excellent for rapid development, built-in ecosystem, great for CRUD-heavy microservices (User Service, Course Catalog). Concurrency is event-loop based (single-threaded async).
- **Go (Golang):** Ideal for high-throughput, low-latency microservices (Gateway auth, Data Pipeline ingestion, Leaderboards). Uses Goroutines for lightweight concurrency. Best for network-bound services.
- **Rust:** Ideal for CPU-intensive tasks (ATS parsing algorithms, video processing). Uses Ownership and fearless concurrency. Steep learning curve.

**Decision:** Hybrid Approach.

- Use **NestJS** for primary business logic services (Courses, Users, Community) to accelerate MVP.
- Use **Go** for the Data Pipeline and Gamification Engine (Leaderboards) to handle high-frequency events and high concurrency.

### 3.3 Message Broker / Event Streaming (Kafka Alternatives)

- **Apache Kafka:** Industry standard, highly scalable, but complex to manage.
- **Redpanda:** C++ drop-in replacement for Kafka. Much faster, eliminates JVM overhead, single binary.
- **NATS JetStream:** Lightweight, high performance, extremely simple to deploy in Kubernetes. Excellent for microservices communication.
- **RabbitMQ:** Good for traditional task queuing but less ideal for high-throughput event sourcing.

**Decision:** **Redpanda** or **NATS JetStream**. For a highly scalable kubernetes-native setup that is easy to manage from MVP to Enterprise, **NATS JetStream** is selected for internal microservice communication, with **Redpanda** as an option if heavy data-pipeline integration with analytics tools is required later. Let's default to **Redpanda** for Kafka compatibility without the JVM overhead.

### 3.4 API Gateway

- **Envoy Gateway:** Implements the Kubernetes Gateway API. Highly performant, dynamically configurable, and natively integrates with the modern K8s ecosystem. Used for ingress routing, JWT validation offloading, and rate limiting.

### 3.5 Database Strategy (Polyglot Persistence)

- **Relational DB (PostgreSQL):** Core source of truth for Users, Subscriptions, and Transactions.
- **Document DB (MongoDB):** Flexible schema for Course Content, Quizzes, and unstructured Community Posts.
- **Analytics DB (ClickHouse):** High-speed columnar database for storing and querying millions of event logs (XP gained, video watch time, platform analytics). Excellent for HR Connect dashboards.
- **Vector DB (Milvus / Qdrant / pgvector):** Storing embeddings for AI Mock Interviews, Semantic Search of course content, and ATS Resume matching.
- **In-Memory / Caching (Redis):** Session caching, fast leaderboard generation, and real-time pub/sub for notifications.
- **Embedded Analytics (DuckDB):** Used in the data pipeline for fast, in-memory analytical transformations before loading into ClickHouse.

### 3.6 AI & LLM Ecosystem

- **LLM Integration:** OpenAI / Anthropic or self-hosted models via vLLM.
- **Tools:** LLM-based IDEs (Kilo Code, Gemini-cli, AntiGravity, Cursor) for agentic development.

## 4. Deployment Strategy

- **MVP Deployment:** Kubesolo (local/lightweight Kubernetes for rapid testing).
- **Production Deployment:** Multi-node Kubernetes (EKS/GKE/AKS).
- **CI/CD:** GitHub Actions / ArgoCD for GitOps.
- **Observability:** Prometheus, Grafana, OpenTelemetry, and ELK stack.

## 5. Microservices Breakdown

1. **User Identity Service:** Auth, Profiles, RBAC. (NestJS + Postgres)
2. **Course Engine Service:** Course catalog, modules, progress tracking. (NestJS + MongoDB)
3. **Gamification Service:** XP calculation, Badges, Leaderboards. (Go + Redis + Postgres)
4. **Community Service:** Forums, messaging, peer-to-peer interactions. (NestJS + MongoDB)
5. **AI Service:** Mock interviews, ATS resume parsing, content generation. (Python/FastAPI or Go + Vector DB)
6. **Data Pipeline Service:** Event ingestion, tracking, token usage. (Go + ClickHouse + DuckDB)
7. **HR Connect Service:** Job board, candidate matching, analytics dashboard. (NestJS + ClickHouse)
