## ADR [007]: Adoption of a Time-Series Database for Telemetry and Event Data Storage

### Status
- PROPOSED

### Context
* **Problem Statement**: The Von Digitalis Estates deploy a wide network of MQTT-capable IoT sensors, turnstiles, POS terminals, and edge computer vision hardware across 40 amusement rides and 55 animal displays. These devices generate continuous streams of time-stamped telemetry (e.g., ride vibration, enclosure temperatures, water pH, turnstile counts).
* **Requirements**:
  * Ingest high-throughput, time-ordered metric streams forwarded asynchronously from edge brokers to the cloud.
  * Enable fast time-bucket aggregations and moving-average queries to monitor animal health and park usage.
  * Serve as the historical data repository to feed batch machine learning training jobs and trigger automated operational tasks.

### Decision
* We will deploy a **Time-Series Database (Timeseries DB)** in the cloud as the primary store for all cleaned, enhanced, and classified event telemetry coming from the Cloud MQTT Broker.
* The Timeseries DB will act as the core event and metric repository driving two primary cloud workflows:
  1. **Database Triggers / CRUD Jobs**: Automatically creating or prioritizing work orders in the **Task Management Service** when metrics cross operational thresholds over time.
  2. **Batch Jobs for Predictive ML**: Supplying structured historical datasets to **Predictive ML Models** to power ride maintenance forecasts, visitor flow analysis, and **BI Dashboards**.

### Alternatives Considered
1. *Relational Database (RDBMS - PostgreSQL / MySQL)*: Excellent for ACID-compliant transactional data (e.g., ticket purchases, employee profiles), but expensive to scale and index under continuous, high-frequency time-stamped insert workloads.
2. *General NoSQL Document Store (MongoDB)*: Flexible schema support, but lacks specialized time-partitioning, automated data downsampling/retention policies, and optimized columnar compression for metrics.


### Consequences
* **Positive**:
  * **Optimized Storage & Ingestion**: Columnar compression and time-partitioned indices easily handle continuous data streams from dozens of rides and animal enclosures without degrading query performance.
  * **Built-in Temporal Aggregations**: Enables efficient windowing and trend analysis (e.g., 5-minute average temperature spikes or hourly turnstile throughput).
  * **Seamless AI Pipeline Integration**: Provides a structured, time-aligned data foundation for training predictive machine learning models.
* **Trade-offs**:
  * **Dual Database Complexity**: Non-telemetry relational data (e.g., user profiles, ticketing transactions, assigned task lists) remains in transactional databases, requiring developers to maintain a polyglot persistence model.
  * **Cloud Latency Boundary**: Because telemetry reaches the TSDB via asynchronous cloud MQTT synchronization, the TSDB is designed for trend analysis, cloud task generation, and ML training rather than immediate local sub-second emergency responses.

### Date
2026-09-15