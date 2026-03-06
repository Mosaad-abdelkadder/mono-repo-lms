# Prompts & Scaffolding Guidelines (prompts.md)

This document provides a collection of structured prompts for LLM-based IDEs (like Kilo Code, Gemini-CLI, AntiGravity, Cursor) to bootstrap and scaffold the microservices, frontend application, and AI pipelines for the Gamified LMS system.

## 1. Frontend Scaffolding (Next.js & Tailwind)

**Context:** You are building the user-facing LMS dashboard.

> **Prompt:**
> "I am building a Gamified LMS dashboard using Next.js (App Router), React 18, Tailwind CSS, and shadcn-ui.
>
> 1. Initialize a Next.js project with TypeScript.
> 2. Scaffold a responsive layout with a Sidebar (navigation: Courses, Leaderboard, Community, HR Connect, Profile) and a Topbar (User Avatar, XP Level, Current Streak).
> 3. Create a `CourseCard` component that displays the course title (e.g., 'Docker Mastery'), a progress bar, and the total XP available.
> 4. Ensure all components use Tailwind CSS classes and are fully accessible (ARIA labels).
> Please output the `layout.tsx`, `page.tsx`, and the `CourseCard.tsx` files."

## 2. Backend Scaffolding (NestJS - User Service)

**Context:** You are building the core Identity microservice.

> **Prompt:**
> "I need a NestJS microservice for User Identity within an Event-Driven Architecture.
>
> 1. Set up a NestJS module named `UsersModule`.
> 2. Integrate TypeORM with a PostgreSQL database.
> 3. Create a `User` entity with the following fields: `id` (UUID), `email` (string, unique), `passwordHash` (string), `role` (enum: LEARNER, HR, ADMIN), `xp` (integer, default 0), `level` (integer, default 1).
> 4. Create a `UsersService` that handles creating a user and retrieving a user by email.
> 5. Create a `UsersController` exposing REST endpoints for registration and fetching a profile.
> 6. Ensure the service uses asynchronous dependency injection for the database connection.
> Output the relevant Entity, DTOs, Service, and Controller files."

## 3. High-Performance Scaffolding (Go - Gamification & Leaderboards)

**Context:** You are building the high-concurrency event ingestion and leaderboard engine.

> **Prompt:**
> "I am building a Gamification Engine in Go (Golang) that handles high-throughput events from a message broker (e.g., Redpanda/NATS).
>
> 1. Set up a Go module with a clean architecture (cmd, internal, pkg).
> 2. Create an event listener that connects to a generic message queue and listens for an `Event` struct: `{ UserID: string, EventType: string (e.g., 'COURSE_COMPLETED'), XPValue: int }`.
> 3. Implement a Redis service using `go-redis` to maintain a real-time global leaderboard using Redis Sorted Sets (`ZADD`, `ZREVRANGE`).
> 4. Write a function that processes an incoming event, adds the XP to the user's score in PostgreSQL, and updates their rank in the Redis Sorted Set.
> Ensure robust error handling and use `context.Context` for timeout management. Output the main event processing logic and the Redis integration."

## 4. AI Service Scaffolding (Python/Go - ATS Resume Parser & Vector DB)

**Context:** You are building the AI microservice that processes resumes and matches them to jobs.

> **Prompt:**
> "I need an AI microservice snippet that parses a PDF resume, generates embeddings, and stores them in a Vector Database (like pgvector or Milvus).
>
> 1. Assume the PDF text is already extracted. Write a function that calls the OpenAI `text-embedding-ada-002` (or equivalent open-source model) API to generate embeddings for a string of text.
> 2. Implement a function to connect to a Vector DB and insert the generated embedding alongside metadata (`user_id`, `skills_extracted`).
> 3. Implement a semantic search function that takes a job description (string), generates its embedding, and queries the Vector DB to find the top 5 closest matching resumes based on cosine similarity.
> Ensure the code is production-ready, handles API rate limits (e.g., retries with exponential backoff), and includes necessary type hints."

## 5. DevOps Scaffolding (Kubernetes & Envoy Gateway)

**Context:** You are setting up the infrastructure for the Kubesolo MVP.

> **Prompt:**
> "I need Kubernetes manifests to deploy my Gamified LMS microservices locally using Kubesolo and the Kubernetes Gateway API (Envoy Gateway).
>
> 1. Create a `Gateway` resource that listens on port 80 and 443.
> 2. Create `HTTPRoute` resources to route traffic:
>    - `/api/users/*` -> `user-service` (Port 3000)
>    - `/api/courses/*` -> `course-service` (Port 3001)
>    - `/` -> `frontend-service` (Port 8080)
> 3. Create standard `Deployment` and `Service` manifests for the `user-service` and `frontend-service` (assume simple container images).
> Please output valid YAML manifests for these resources, strictly adhering to the Gateway API v1 spec."

## 6. Data Pipeline Scaffolding (DuckDB & ClickHouse)

**Context:** You are building an analytics pipeline to track LLM Token usage and platform events.

> **Prompt:**
> "I am building a data ingestion pipeline that tracks LLM token utilization and stores it in ClickHouse.
>
> 1. Design a ClickHouse `MergeTree` table schema named `token_usage_logs` with columns: `timestamp` (DateTime), `service_name` (String), `model_name` (String), `prompt_tokens` (UInt32), `completion_tokens` (UInt32), and `total_cost` (Float32).
> 2. Write a short script (in Python or Go) that demonstrates using DuckDB as an embedded, in-memory staging area:
>    - Load a batch of JSON log files into DuckDB.
>    - Perform a fast aggregation query to calculate total tokens per service.
>    - Output the transformed data (or demonstrate how you would export it to be bulk-inserted into ClickHouse).
> Focus on the table schema design and the analytical query logic."
