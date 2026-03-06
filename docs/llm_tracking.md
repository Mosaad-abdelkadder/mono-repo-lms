# LLM Token Tracking & Cost Pipeline (llm_tracking.md)

This document outlines the architecture for monitoring, tracking, and analyzing the usage and cost of Large Language Models (LLMs) integrated into the Gamified LMS platform (e.g., AI Mock Interviews, ATS Resume Parsing).

## 1. Objective

Ensure comprehensive observability over LLM utilization to prevent runaway costs, optimize prompts, and attribute usage to specific services or users.

## 2. Architecture Overview

### 2.1 The Data Pipeline

The system utilizes a decoupled, event-driven pipeline:

1. **Event Generation:** Any microservice (e.g., AI Service, Resume Parser) that calls an LLM API must emit an `LLMUsageEvent`.
2. **Message Broker:** The event is published to a dedicated Redpanda/NATS JetStream topic (`ai.telemetry.tokens`).
3. **Ingestion Service (Go):** A high-throughput Go service consumes these events.
4. **Staging & Transformation (DuckDB):** The Go service batches events and uses embedded DuckDB to perform fast, in-memory analytical transformations (e.g., calculating cost based on a pricing matrix).
5. **Persistent Analytics Store (ClickHouse):** The transformed, aggregated data is bulk-inserted into a ClickHouse database optimized for time-series analytics.
6. **Visualization (Grafana/Next.js):** Dashboards query ClickHouse for real-time cost analysis.

## 3. Data Schema

### 3.1 Event Payload (`LLMUsageEvent`)

Emitted by microservices to the message broker (JSON format).

```json
{
  "event_id": "uuid",
  "timestamp": "2023-10-27T10:00:00Z",
  "service_name": "ai-mock-interview",
  "user_id": "user_123",
  "model": "gpt-4-turbo",
  "provider": "openai",
  "prompt_tokens": 150,
  "completion_tokens": 50,
  "total_tokens": 200,
  "latency_ms": 1200,
  "status": "success",
  "context": "React Technical Interview Question 1"
}
```

### 3.2 ClickHouse Table (`llm_token_usage`)

The primary table for storing token logs. It uses the `MergeTree` engine for efficient storage and fast querying.

```sql
CREATE TABLE llm_token_usage
(
    event_id UUID,
    timestamp DateTime,
    service_name String,
    user_id String,
    model String,
    provider String,
    prompt_tokens UInt32,
    completion_tokens UInt32,
    total_tokens UInt32,
    latency_ms UInt32,
    estimated_cost Float32, -- Calculated during ingestion (e.g., using DuckDB)
    status String,
    context String
)
ENGINE = MergeTree()
PARTITION BY toYYYYMM(timestamp)
ORDER BY (service_name, model, timestamp)
SETTINGS index_granularity = 8192;
```

## 4. Ingestion Workflow (Go & DuckDB)

The Go microservice acts as the pipeline orchestrator:

1. **Consume:** Reads a batch of `LLMUsageEvent` messages from Redpanda.
2. **Stage in DuckDB:**
   - The Go service spins up an in-memory DuckDB instance.
   - It loads the JSON batch into a temporary DuckDB table.
   - Example DuckDB query for enrichment (pseudo-code):

     ```sql
     -- Join with a pricing dimension table to calculate cost
     SELECT
         e.*,
         (e.prompt_tokens * p.prompt_price_per_token) +
         (e.completion_tokens * p.completion_price_per_token) AS estimated_cost
     FROM temp_events e
     LEFT JOIN pricing_matrix p ON e.model = p.model;
     ```

3. **Load to ClickHouse:** The enriched dataset is exported from DuckDB (e.g., as Parquet or via direct connection) and bulk-inserted into ClickHouse. This minimizes writes to the analytical database and maximizes throughput.

## 5. Cost Attribution & Dashboards

Using ClickHouse, we can build Next.js/Grafana dashboards to answer critical business questions:

- **Total Cost per Day/Month:** Aggregating `estimated_cost` over time.
- **Cost per Service:** Identify which feature (e.g., Mock Interviews vs. Resume Parsing) is consuming the most tokens.
- **Cost per User:** Track heavy users. If a user exceeds a certain threshold (e.g., >$5/month in token costs), trigger an alert or throttle their API access via the Envoy Gateway rate limiting integration.
- **Latency Analysis:** Monitor `latency_ms` to ensure the user experience remains responsive, switching to faster models (e.g., GPT-3.5 or Llama 3) if latency spikes.

## 6. Implementation Notes for AI Agents

- **Validation:** When scaffolding the `LLMUsageEvent` producer in Next.js or Go, ensure strict validation (e.g., using Zod or Go structs) before publishing to Redpanda.
- **Resilience:** The Go ingestion service must handle Redpanda disconnects gracefully and retry ClickHouse bulk inserts if the database is temporarily unavailable.
