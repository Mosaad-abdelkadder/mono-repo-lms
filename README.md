# Gamified Learning Management System (LMS)

A highly scalable, microservices-based Learning Management System designed for engaging, gamified education. This platform leverages cutting-edge technology and AI integration to provide interactive courses, real-time leaderboards, automated mock interviews, and a seamless bridge to recruiters via the HR Connect dashboard.

## Overview

The Gamified LMS platform is built with a modern event-driven architecture designed to evolve from a local MVP to a globally scalable, multi-cluster Kubernetes deployment.

Key pillars of the platform include:

- **Gamification Engine:** Earn XP, level up, and unlock achievements by completing interactive modules. Real-time leaderboards are powered by Go and Redis.
- **AI-Powered Ecosystem:** Integrated LLMs power features like automated Mock Interviews (with speech-to-text feedback) and ATS Resume Parsing.
- **Polyglot Persistence:** Optimized data storage using PostgreSQL (Relational), MongoDB (Documents), ClickHouse (Analytics), DuckDB (Embedded Analytics), and Vector Databases (Embeddings).
- **High Concurrency & Event Streaming:** Built using Next.js (Frontend), NestJS (Core APIs), and Go/Rust (High-throughput services), communicating asynchronously via Redpanda or NATS JetStream.

## Project Structure & Documentation

The architecture, design guidelines, and implementation plans are extensively documented. AI Agents and human developers alike should refer to the `docs/` folder for comprehensive context:

- **[System Specifications (specs.md)](docs/specs.md):** Detailed architecture diagram, component breakdown, and technology stack justification.
- **[Competencies & Skills Requirements (skills.md)](docs/skills.md):** Required technical skills for contributing to the frontend, backend, data pipeline, and DevOps infrastructure.
- **[AI Agent Roles & Responsibilities (agents.md)](docs/agents.md):** Specialized roles and directives for AI agents (e.g., Kilo Code, Gemini-CLI, Cursor) collaborating on the repository.
- **[System Rules & Architectural Guidelines (rules.md)](docs/rules.md):** The strict rulebook defining architectural constraints, coding standards (TypeScript/Go/Rust), database rules, and AI integration guidelines.
- **[Project Execution Plan & Tasks (tasks.md)](docs/tasks.md):** An iterative roadmap from MVP deployment (using Kubesolo) to production scalability on multi-cluster Kubernetes environments.
- **[Prompts & Scaffolding Guidelines (prompts.md)](docs/prompts.md):** A collection of structured prompts to bootstrap and scaffold services using LLM-based IDEs.
- **[Example Courses & Gamification Design (courses.md)](docs/courses.md):** Detailed design of sample courses (Docker & Kubernetes), complete with XP structures, modules, quizzes, and competitions.
- **[LLM Token Tracking & Cost Pipeline (llm_tracking.md)](docs/llm_tracking.md):** The data pipeline architecture (Go, Redpanda, DuckDB, ClickHouse) for monitoring, tracking, and analyzing LLM usage and costs.
- **[Platform Features & Ecosystem (features.md)](docs/features.md):** Overview of core features, including the Community Hub, AI Mock Interviews, ATS detection, and the HR Connect Dashboard.

## Getting Started (MVP)

The initial deployment strategy utilizes **Kubesolo** for rapid, lightweight local development. The system routes all traffic through the **Envoy Gateway** implementing the Kubernetes Gateway API.

*Detailed setup instructions will be provided in subsequent phases of development (see `docs/tasks.md`).*

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
