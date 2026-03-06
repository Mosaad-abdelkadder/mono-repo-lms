# Platform Features & Ecosystem (features.md)

This document details the core features of the Gamified LMS platform, including the AI-driven ecosystem, community engagement tools, and the bridge between learning and recruitment (HR Connect).

## 1. Gamified Learning Engine

- **XP & Levels:** Earn Experience Points (XP) for completing lessons, quizzes, and practical labs. Reach new levels to unlock advanced courses.
- **Badges & Achievements:** Showcase mastery with badges (e.g., "Docker Architect", "Kubernetes Master").
- **Leaderboards:** Real-time global, cohort, and skill-specific rankings driven by a high-concurrency Go & Redis backend.
- **Interactive Labs:** In-browser environments (via WebAssembly or ephemeral containers) for hands-on practice without leaving the platform.

## 2. AI-Powered Features

### 2.1 AI Mock Interviews

- **Simulated Environments:** Practice technical interviews (e.g., React, Go, System Design) against an AI persona.
- **Real-time Feedback:** Receive instant analysis on communication style, technical accuracy, and code quality.
- **Speech-to-Text & Text-to-Speech:** Immersive conversational experience using WebRTC and advanced LLMs.
- **Scoring System:** Interviews are scored and factored into the user's overall platform profile.

### 2.2 ATS Resume Detection & Parsing

- **Automated Parsing:** Users upload their resumes (PDF/Docx), which are parsed by the AI Service to extract skills, experience, and education.
- **ATS Compatibility Score:** The system evaluates the resume against standard Applicant Tracking System (ATS) algorithms, providing actionable feedback to improve keyword matching and formatting.
- **Skill Gap Analysis:** Based on the parsed resume and the user's desired career path, the AI recommends specific courses (e.g., "Take the Docker Deep Dive to improve your DevOps profile").

## 3. Community Hub

- **Course Forums:** Context-specific discussion boards linked to individual courses and modules.
- **Peer-to-Peer Mentoring:** Match experienced users (high level/XP) with beginners for pair programming or code reviews.
- **Study Groups:** Create or join private groups based on shared interests (e.g., "Go Backend Engineers", "K8s Certification Prep").
- **Real-time Chat:** Integrated messaging (WebSockets via NestJS) for instant collaboration.

## 4. HR Connect Dashboard

The HR Connect feature bridges the gap between skilled learners and recruiters.

- **Talent Pool Search:** Recruiters can search for candidates based on verified skills (acquired via course completion and lab assessments), not just self-reported resume data.
- **Semantic Matching:** Recruiters upload a Job Description, and the system uses Vector DB embeddings to find the closest matching learner profiles.
- **Verified Portfolios:** Profiles showcase a user's XP, badges, completed projects, and AI Mock Interview scores.
- **Direct Messaging:** Recruiters can reach out directly to top-performing candidates within the platform.

## 5. Automated Workflows & Data Pipelines

The platform relies heavily on event-driven architecture to automate processes and generate insights.

- **Event Streaming:** All significant actions (course completion, interview score, forum post) emit events to Redpanda / NATS JetStream.
- **LLM Token Tracking:** A dedicated data pipeline (Go + DuckDB + ClickHouse) monitors LLM usage across the platform to manage costs and optimize performance.
- **Automated Certifications:** Upon completing a learning path, an automated workflow generates a verified certificate and mints it (optionally as a verifiable credential or NFT).
- **Engagement Analytics:** The Data Engineering Agent processes event logs into ClickHouse to power the HR dashboard and internal admin reporting (e.g., drop-off rates, most popular modules).

## 6. Ecosystem Integration

The LMS is designed not just as a standalone app, but as a cohesive ecosystem:

- **LLM IDE Integration:** Code snippets and assignments can be generated or evaluated by AI Agents (Kilo Code, Gemini-CLI, AntiGravity).
- **Figma to Code:** UI/UX designs are seamlessly translated into Next.js components using tools like Google Stitch.
- **Kubernetes Native:** Deployed via Kubesolo for local development and scalable to multi-cluster EKS/GKE environments using the Envoy Gateway API.
