## ADR [006]: Direct Cloud Data Warehouse Architecture vs. Data Lake Ecosystem

### Status
- PROPOSED

### Context
* **Estate Operational Scope:** Von Digitalis Estates operates **40 amusement rides** and **55 exotic animal enclosures** housing **200+ animals**.
* **Visitor Capacity:** The estate currently hosts **5,000 visitors per day**, with plans to scale to **15,000 visitors per day** over the next three years.
* **Telemetry &amp; Sensors:** The estate relies on MQTT-capable hardware devices and edge sensors spread across patchy Wi-Fi network zones.
* **Analytical Goals:** Data must fuel real-time operational alerts, manager dashboards, predictive machine learning models for ride maintenance, and crowd flow analytics.
* **Architectural Question:** Should the estate build a traditional **Data Lake / Lakehouse** (e.g., S3/GCS raw object storage + Apache Spark/Iceberg + Trino/Presto query engine) or stream directly into a managed **Cloud Data Warehouse / Time-Series Analytical Database** (e.g., Snowflake, BigQuery, or ClickHouse/TimescaleDB)


### **Mathematical Walkthrough & Data Scale Assumptions**

To evaluate the storage and throughput requirements, we calculate the estimated daily and annual data volume generated across the park at peak capacity (15,000 visitors/day):

#### **1. Edge Sensor & Telemetry Data (40 Rides + 55 Enclosures)**
* **Sensor Count:** Assume 2–3 environmental/vibration sensors per enclosure and ride, plus turnstile/POS interfaces \\(\approx 250\\) active MQTT edge nodes.
* **Sampling Rate:** MQTT telemetry published every 10 seconds per node.
* **Daily Message Volume:** 
  \\[\text{250 nodes} \times \left(\frac{\text{86,400 sec/day}}{\text{10 sec/ping}}\right) = 2,160,000 \text{ telemetry events/day}\\]
* **Payload Size:** Standard JSON metric payload (device ID, timestamp, temperature, vibration, water level, status) \\(\approx 0.5 \text{ KB}\\).
* **Daily Ingest Volume:** \\(2,160,000 \times 0.5 \text{ KB} \approx \mathbf{1.08 \text{ GB/day}}\\).

#### **2. Visitor Observability & Ticketing Events**
* **Visitor Events:** 15,000 visitors/day generating \\(\approx 20\\) event pings/day (turnstile entry, ride entry, POS transaction, app feedback scan).
* **Daily Event Volume:** \\(15,000 \times 20 = 300,000 \text{ events/day}\\).
* **Payload Size:** Transaction / location ping payload \\(\approx 0.5 \text{ KB}\\).
* **Daily Ingest Volume:** \\(300,000 \times 0.5 \text{ KB} \approx \mathbf{0.15 \text{ GB/day}}\\).

#### **3. Edge Computer Vision Metadata (Animal Health & Piranha Counts)**
* *Note:* Unstructured high-bitrate video streams (e.g., camera feeds monitoring jumping piranha tanks) are **processed locally at the Edge AI node**. Only processed structured metadata summaries (e.g., `{"tank_id": 4, "piranha_count": 42, "movement_index": 0.88}`) are sent to the cloud every 60 seconds.
* **Metadata Daily Volume:** 55 enclosures \\(\times 1,440 \text{ min/day} \times 0.5 \text{ KB} \approx \mathbf{0.04 \text{ GB/day}}\\).

---

#### **Total Data Volume Summary**
* **Daily Raw Volume:** \\(\approx \mathbf{1.27 \text{ GB / day}}\\)
* **Annual Uncompressed Storage:** \\(1.27 \text{ GB/day} \times 365 \text{ days} \approx \mathbf{463.5 \text{ GB / year}}\\) (~0.46 TB/year)
* **5-Year Accumulated Storage (with Columnar Compression ~4:1):** 
  \\[\frac{463.5 \text{ GB/year} \times 5 \text{ years}}{4} \approx \mathbf{580 \text{ GB total stored}}\\]

---


### Decision
We will **skip building a Data Lake** and directly stream structured/semi-structured telemetry from the Cloud MQTT Broker into a managed **Cloud Data Warehouse / Time-Series Database** (such as Snowflake, BigQuery, or ClickHouse).

### **Justification: Why a Data Lake is Over-Engineering**
* **Gigabyte-Scale vs. Petabyte-Scale Realities:** Data Lakes (and Delta Lake / Iceberg architectures) are designed to solve storage-cost efficiency at **Petabyte to Exabyte scale**. At under **0.5 TB per year**, the entire multi-year history of Von Digitalis Estates easily fits within a single cost-effective Data Warehouse instance or serverless cloud tier.
* **No Unstructured Data Storage Needs in Cloud:** Data Lakes excel at storing raw images, audio, and video files. However, high-bandwidth camera feeds are processed by **Edge AI vision nodes** on-site to handle patchy Wi-Fi constraints[3][4]. Only structured JSON metadata enters the cloud, eliminating the need for object-storage data lakes.
* **Operational Simplicity &amp; Engineering Overhead:**
* **Data Lake Complexity:** Requires managing S3 bucket partitioning strategies, Parquet file compaction jobs, schema drift handlers, AWS Glue / Spark ETL jobs, and query engines (Trino/Athena). This creates heavy DevOps toil for a small estate engineering team.
* **Data Warehouse Simplicity:** Supports direct JSON ingestion (`VARIANT` or `JSON` data types), automatic partitioning, auto-clustering, and native SQL querying out of the box with zero infrastructure maintenance.
* **Query Latency for Real-Time Analytics &amp; ML:** Cloud Data Warehouses offer sub-second analytical query performance for BI Dashboards and direct SQL connectors for Predictive ML models, whereas querying raw Data Lake files requires additional query orchestration layers.


### **Consequences & Trade-offs**

#### **Positive Consequences:**
* **Zero ETL Pipeline Maintenance:** Ingestion pipelines flow directly from Cloud MQTT \\(\rightarrow\\) Ingestion Service \\(\rightarrow\\) Data Warehouse tables.
* **Lower Total Cost of Ownership (TCO):** Eliminates dedicated Big Data / Spark engineering staff; operational effort is spent on AI models and business logic.
* **Unified Security & Governance:** Role-Based Access Control (RBAC), auditing, and data encryption are managed in a single SQL platform.

#### **Negative Consequences & Mitigations:**
* **Storage Cost per GB is Higher than Raw S3:** *Mitigation:* At ~500 GB total multi-year scale, cloud warehouse storage costs are negligible (<\$15/month).
* **Future Unstructured Data Requirements:** *Mitigation:* If raw video snapshots from enclosures are ever required for model retraining, they can be stored directly in a simple cloud object bucket (e.g., S3) linked as an external stage to the Data Warehouse.

---

### Date
2026-09-15