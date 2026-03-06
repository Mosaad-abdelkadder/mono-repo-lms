# Project Execution Plan & Tasks (tasks.md)

This document outlines the iterative implementation plan to build the Gamified LMS platform, starting from an MVP deployed locally via **Kubesolo** to a highly scalable, multi-cluster **Kubernetes** deployment.

## Phase 1: Foundation & Scaffolding (MVP Preparation)

### Objective: Set up the Mono-repo structure, basic infrastructure, and foundational services

- [ ] **Task 1.1: Mono-repo Setup**
  - Scaffold the workspace using Nx or Turborepo.
  - Set up shared ESLint, Prettier, and TypeScript configurations across all apps.
- [ ] **Task 1.2: Infrastructure Provisioning (Local)**
  - Initialize the **Kubesolo** cluster for local development.
  - Deploy **Envoy Gateway** (Gateway API) with basic routing rules.
  - Deploy basic datastores via Helm charts: PostgreSQL, MongoDB, Redis.
- [ ] **Task 1.3: User Identity Service (NestJS + Postgres)**
  - Implement Auth (JWT, OAuth2).
  - Define user profiles (Learner, HR, Admin).
  - **Milestone:** Users can register, log in, and receive a token from the Gateway.

## Phase 2: Core LMS & Gamification Engine

### Objective: Deliver the core educational experience with the primary gamification loop

- [ ] **Task 2.1: Course Engine Service (NestJS + MongoDB)**
  - Implement CRUD for Courses, Modules, and Lessons (focusing on the initial Docker & K8s courses).
  - Build progress tracking (User -> Course Completion Status).
- [ ] **Task 2.2: Gamification Service (Go + Redis + Postgres)**
  - Implement the rules engine for awarding XP (e.g., "Complete Module" = +50 XP).
  - Build real-time Leaderboards using Redis Sorted Sets.
  - **Milestone:** Users can complete lessons and see their XP increase on a global leaderboard.
- [ ] **Task 2.3: Frontend Application (Next.js)**
  - Build the Learner Dashboard.
  - Integrate course navigation and video/text content rendering.
  - Display the Gamification overlay (Badges, Current Level).

## Phase 3: AI Integration & Event Streaming

### Objective: Introduce AI-powered features and transition to an event-driven architecture

- [ ] **Task 3.1: Message Broker Setup (Redpanda/NATS JetStream)**
  - Deploy Redpanda/NATS in the Kubesolo cluster.
  - Refactor the User and Course services to emit events (e.g., `CourseCompleted`, `UserRegistered`).
- [ ] **Task 3.2: AI Service (Go/Rust or Python + Vector DB)**
  - Deploy a Vector Database (pgvector or Milvus).
  - Implement the **AI Mock Interview** feature (Speech-to-Text -> LLM Prompt -> Evaluation).
  - Implement the **ATS Resume Detection** pipeline (PDF parsing -> embeddings -> Vector DB storage).
- [ ] **Task 3.3: LLM Tracking & Cost Pipeline (Go + DuckDB + ClickHouse)**
  - Intercept all LLM requests (via a reverse proxy or middleware) to log token usage.
  - Use DuckDB for staging analytics data.
  - Load aggregated token usage into ClickHouse for cost-monitoring dashboards.

## Phase 4: Community, HR Connect, & Advanced Features

### Objective: Foster community engagement and bridge the gap between learners and recruiters

- [ ] **Task 4.1: Community Service (NestJS + MongoDB)**
  - Implement Discussion Forums linked to specific courses.
  - Implement peer-to-peer messaging.
- [ ] **Task 4.2: HR Connect Dashboard (Next.js + ClickHouse)**
  - Build a portal for recruiters.
  - Aggregate learner performance (Course completion, Mock Interview Scores, XP Level).
  - Use the AI Service to match candidate embeddings (skills/resume) against job descriptions.
- [ ] **Task 4.3: Competitions Module**
  - Implement time-bound challenges (e.g., "K8s Debugging Speedrun") to earn massive XP multipliers.

## Phase 5: Production Scalability & Observability

### Objective: Transition from Kubesolo MVP to a production-ready Kubernetes deployment

- [ ] **Task 5.1: Cloud Infrastructure Provisioning**
  - Set up Terraform modules for EKS/GKE/AKS.
  - Configure robust node groups and auto-scaling policies.
- [ ] **Task 5.2: Observability Stack Deployment**
  - Deploy Prometheus for metrics scraping.
  - Deploy OpenTelemetry collectors for distributed tracing across NestJS, Go, and AI services.
  - Deploy Grafana for comprehensive dashboards.
- [ ] **Task 5.3: CI/CD & GitOps**
  - Finalize GitHub Actions pipelines for running unit, integration, and E2E tests.
  - Set up ArgoCD for automated continuous delivery to the production cluster.
- [ ] **Task 5.4: Final Polish & Load Testing**
  - Perform stress tests using k6 on the API Gateway and Leaderboard endpoints.
  - Optimize ClickHouse queries for the HR Dashboard.
