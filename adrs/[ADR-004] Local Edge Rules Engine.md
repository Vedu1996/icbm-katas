## ADR [004]: Local Edge Rules Engine for Immediate Emergency Processing

### Status
- PROPOSED

### Context
* **Problem Statement**: The Von Digitalis Estates span a wide area where Wi-Fi coverage is patchy and cloud network connectivity cannot be guaranteed. The park operates 40 amusement rides and houses over 200 exotic animals across 55 displays, creating strict operational safety demands.
* **Emergency Requirements**: Critical incidents—such as ride safety triggers, animal containment breaches, or urgent health alerts—demand immediate, sub-second responses and local notifications that function independently of WAN connectivity.


### Decision
* We will deploy a **Local Rules Engine** that directly subscribes to the **Local MQTT Broker** on the estate network.
* The Rules Engine continuously evaluates incoming event streams from POS devices, environmental sensors, and AI edge hardware.
* Upon detecting emergency conditions or safety threshold breaches, the engine triggers **Local Alerting** and automated safeguards immediately at the edge.
* All event telemetry is subsequently published asynchronously to the Cloud MQTT Broker for ingestion into the Time-series DB, feeding predictive ML models and high-level BI dashboards.

### Alternatives Considered
1. *Cloud-Only Processing*: Routing all local telemetry directly to cloud services for decision-making. **Rejected** because patchy Wi-Fi introduces unpredictable latency and risks failure during cloud outages.
2. *Sensor-Embedded Static Rules*: Hardcoding fixed logic directly onto individual microcontrollers. **Rejected** due to maintenance friction across distributed hardware and inability to evaluate complex multi-sensor events.


### Date
2026-09-15