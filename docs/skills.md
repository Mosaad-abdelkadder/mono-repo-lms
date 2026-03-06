# Competencies & Skills Requirements (skills.md)

## 1. Overview

This document outlines the required skills and competencies for developing and maintaining the Gamified LMS platform. It is designed to act as a reference for both human developers and AI Agents (e.g., Kilo Code, Gemini-cli, AntiGravity, Cursor) participating in the project.

## 2. Core Technological Competencies

### 2.1 Frontend Development

- **Next.js (App Router):** Server-Side Rendering (SSR), Static Site Generation (SSG), caching strategies.
- **React 18+:** Hooks, Context API, Suspense, Concurrent Mode.
- **State Management:** Zustand (global state), React Query (server state caching).
- **Styling:** Tailwind CSS, shadcn-ui, accessibility (a11y) best practices.
- **Figma to Code:** Experience using tools like Google Stitch or equivalent plugins to translate UI/UX designs into React components.

### 2.2 Backend Development (NestJS)

- **Framework Proficiency:** Modules, Controllers, Providers, Guards, Interceptors, Pipes.
- **TypeScript:** Advanced typing, generics, decorators.
- **Data Access:** TypeORM or Prisma integration.
- **Architecture:** CQRS (Command Query Responsibility Segregation), Event-Driven design.
- **Testing:** Jest, Supertest (E2E).

### 2.3 Backend Development (Go/Rust)

- **Go (Golang):** Goroutines, Channels, Context, clean architecture, high-concurrency microservices (Leaderboards, Data Ingestion).
- **Rust:** Ownership model, lifetimes, async programming (Tokio), CPU-bound processing (ATS algorithms, complex matching logic).
- **Performance:** Profiling, memory optimization, zero-copy techniques.

### 2.4 Data Engineering & Databases

- **Relational:** PostgreSQL (ACID properties, complex joins, JSONB).
- **Document:** MongoDB (Schema design, aggregation framework, indexing).
- **Analytics:** ClickHouse (Columnar storage, Materialized Views, high-speed aggregation for HR dashboards).
- **In-Memory/Caching:** Redis (Pub/Sub, sorted sets for leaderboards).
- **Embedded Analytics:** DuckDB (In-memory analytical transformations).
- **Vector DBs:** Milvus, Qdrant, or pgvector (Similarity search, embeddings storage).

### 2.5 Event Streaming & Message Brokers

- **Redpanda / Kafka:** Topics, partitions, consumer groups, offset management, schema registry.
- **NATS JetStream:** Lightweight messaging, exactly-once delivery semantics.

### 2.6 DevOps, Orchestration & Cloud Native

- **Containerization:** Docker (multi-stage builds, optimization).
- **Orchestration:** Kubernetes (Deployments, Services, ConfigMaps, Secrets, StatefulSets).
- **Local Dev:** Kubesolo, Minikube, or kind.
- **API Gateway:** Envoy Gateway (Kubernetes Gateway API implementation, traffic shaping, rate limiting).
- **CI/CD:** GitHub Actions, ArgoCD (GitOps workflow).
- **Observability:** OpenTelemetry, Prometheus, Grafana, structured logging.

### 2.7 AI Integration & Engineering

- **LLM APIs:** Integration with OpenAI, Anthropic, or open-source models (Llama 3, Mistral) via vLLM.
- **RAG Architecture:** Retrieval-Augmented Generation workflows.
- **Prompt Engineering:** Designing robust prompts for AI mock interviews, content generation, and resume analysis.
- **Token Management:** Monitoring, rate limiting, cost optimization strategies for LLM usage.

## 3. Specialized Domain Skills

### 3.1 Gamification Design

- Experience implementing XP (Experience Points) systems.
- Designing engaging Leaderboards (Global, Friends, Cohort).
- Structuring Competitions and Badges/Achievements logic.

### 3.2 EdTech & LMS Standards

- Understanding of learning paths, modules, and assessment logic.
- Content tracking and completion verification.

### 3.3 HR & ATS Integration

- Resume parsing logic (Extracting skills, experience).
- Semantic matching between candidate profiles and job descriptions.

## 4. AI Agent Configuration Directives

When configuring an AI Agent (e.g., inside an `AGENTS.md` or system prompt) to work on this repository, ensure the agent is instructed to:

1. Prioritize **type safety** (TypeScript/Go/Rust).
2. Adhere to **clean architecture** principles (Separation of concerns).
3. Write **tests** for business logic and critical data paths.
4. Consider **scalability** and **concurrency** when designing backend features.
5. Utilize the **Envoy Gateway** for routing and cross-cutting concerns (auth, rate limiting) rather than duplicating logic in microservices.
