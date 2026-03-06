# Example Courses & Gamification Design (courses.md)

This document outlines two example courses (Docker & Kubernetes) designed to demonstrate the platform's gamified learning experience, including XP points, leaderboards, and competitions.

## 1. Gamification Framework

The Gamified LMS uses the following mechanics to drive engagement:

- **Experience Points (XP):** Earned by completing modules, quizzes, and practical exercises.
- **Levels:** Users level up based on accumulated XP (e.g., Level 1: 0-100 XP, Level 2: 101-300 XP).
- **Badges/Achievements:** Awarded for specific milestones (e.g., "First Container", "Cluster Commander").
- **Leaderboards:** Real-time ranking of users based on total XP or course-specific XP. (Implemented via Go + Redis).
- **Streaks:** Bonus XP multipliers for logging in and learning consecutive days.

---

## 2. Course 1: Docker Deep Dive

**Description:** From zero to containerization expert. Learn how to build, run, and optimize Docker containers.
**Total Available XP:** 1,500 XP
**Prerequisites:** None.

### Module 1: The Container Revolution (200 XP)

- **Lesson 1:** What is a Container? (Video) -> *+20 XP*
- **Lesson 2:** Installing Docker Desktop (Interactive Guide) -> *+30 XP*
- **Quiz 1:** Container Concepts (5 Questions) -> *+50 XP (10 per correct answer)*
- **Hands-on Lab:** Run your first `hello-world` container -> *+100 XP*
- **Achievement Unlocked:** 🐋 "First Splash" Badge.

### Module 2: Building Images with Dockerfile (500 XP)

- **Lesson 1:** The Anatomy of a Dockerfile (Article) -> *+50 XP*
- **Lesson 2:** Multi-stage Builds for Optimization (Video) -> *+100 XP*
- **Interactive Challenge:** Write a Dockerfile for a Node.js app -> *+200 XP*
  - *Note:* The AI Service evaluates the submitted Dockerfile for best practices (layer caching, non-root user). Bonus +50 XP for perfect score.
- **Quiz 2:** Image Optimization (10 Questions) -> *+100 XP*

### Module 3: Docker Compose & Networking (800 XP)

- **Lesson 1:** Orchestrating Multi-Container Apps (Video) -> *+100 XP*
- **Lesson 2:** Docker Networks (Bridge, Host, Overlay) -> *+150 XP*
- **Final Project:** Containerize a full-stack app (React Frontend, Node Backend, Postgres DB) using `docker-compose.yml`.
  - *Verification:* Automated test runner executes `docker compose up` and pings the frontend endpoint. -> *+500 XP*
- **Achievement Unlocked:** 🏗️ "Architect" Badge.

---

## 3. Course 2: Kubernetes Mastery (Kubesolo MVP Focus)

**Description:** Master container orchestration. Deploy, scale, and manage applications using Kubernetes.
**Total Available XP:** 3,000 XP
**Prerequisites:** Docker Deep Dive (Course 1) Completion.

### Module 1: K8s Architecture & Pods (500 XP)

- **Lesson 1:** Control Plane vs. Worker Nodes (Video) -> *+50 XP*
- **Lesson 2:** The Pod Lifecycle (Article) -> *+50 XP*
- **Hands-on Lab:** Deploy a Nginx Pod via `kubectl` on local Kubesolo -> *+200 XP*
- **Quiz 1:** K8s Components (10 Questions) -> *+200 XP*

### Module 2: Services, Ingress, & Gateway API (1,000 XP)

- **Lesson 1:** Exposing Pods (ClusterIP, NodePort, LoadBalancer) -> *+100 XP*
- **Lesson 2:** Modern Routing with Envoy Gateway API -> *+200 XP*
- **Interactive Challenge:** Write an `HTTPRoute` manifest to route traffic to two different services based on URL paths.
  - *Verification:* AI Service validates YAML syntax and Gateway API v1 compliance. -> *+500 XP*
- **Quiz 2:** Networking (10 Questions) -> *+200 XP*
- **Achievement Unlocked:** 🌐 "Traffic Controller" Badge.

### Module 3: StatefulSets & Persistent Volumes (1,500 XP)

- **Lesson 1:** Managing State in K8s (Video) -> *+200 XP*
- **Lesson 2:** PVCs and Storage Classes (Article) -> *+300 XP*
- **Final Project:** Deploy a highly available Redis cluster using a StatefulSet and Persistent Volumes. -> *+1,000 XP*
- **Achievement Unlocked:** 💾 "Data Guardian" Badge.

---

## 4. Competitions & Community Events

To drive continuous engagement beyond standard courses, the platform hosts time-bound events.

### Event: The "Chaos Monkey" Challenge (Weekend Hackathon)

- **Objective:** Participants are given access to a broken Kubernetes namespace (e.g., CrashLoopBackOff pods, misconfigured services, network policies blocking traffic).
- **Task:** Diagnose and fix all issues using `kubectl` within 60 minutes.
- **Scoring:** Time-based. Fastest completion earns the most points.
- **Rewards:**
  - 1st Place: 10,000 XP + "Chaos Tamer" Profile Title (visible to HR).
  - 2nd-10th Place: 5,000 XP.
  - All participants who resolve the issues: 1,000 XP.

### Global & Cohort Leaderboards

- **Global:** Ranks all users on the platform based on total accumulated XP. Updates in near real-time via Go + Redis.
- **Cohort (Company/University):** Users can join private leaderboards for internal friendly competition.
- **Skill-Specific:** Leaderboards dedicated to specific tracks (e.g., "Top 100 in Kubernetes", "Top 50 in Go Lang").
