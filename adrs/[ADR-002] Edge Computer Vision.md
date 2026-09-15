## ADR [002]: Edge Computer Vision for Crowd Density with Anonymized Aggregation vs. BLE/RFID Tracking**

### Status
- PROPOSED

### Context
* **Estate Scale & Visitor Volume:** Von Digitalis Estates manages **40 amusement rides** and **55 exotic animal displays** housing 200+ animals, hosting **5,000 visitors per day** with growth projections reaching **15,000 daily visitors**.
* **Business Requirement:** Park management needs to understand crowd flow, zone popularity, and queue bottlenecks to optimize staff deployment and capital investment across the estate.
* **Technical & Physical Constraints:** Estate Wi-Fi coverage is patchy across the sprawling grounds, but there is an allocated budget for MQTT-capable hardware devices.
* **Privacy & Regulatory Requirements:** Visitor tracking must comply with strict privacy standards (e.g., GDPR / EU AI Act) and strictly avoid capturing or storing Personally Identifiable Information (PII) or biometric identity data.
* **Architectural Question:** How can we measure park popularity and crowd density reliably without creating physical bottlenecks, privacy liability, or heavy network overhead?

### Decision
We will deploy **Edge Computer Vision (CV) micro-nodes** running lightweight object detection models (e.g., YOLOv8 nano / MobileNet) on fixed camera hardware at key park zones (ride queues, enclosure viewing areas, and main pathways).


#### **Key Architectural & Privacy Patterns:**
1. **On-Device Local Inference:** Video frames captured by optical sensors are processed entirely in-memory within volatile RAM at the edge node. Raw video or image frames are **never recorded, stored, or transmitted** across the network.
2. **Anonymized Data Aggregation:** Edge models perform non-biometric spatial counting (detecting generic human bounding boxes) and transform visual data into purely quantitative telemetry metrics (e.g., headcount, crowd density, flow direction).
3. **MQTT Event Publishing:** Edge nodes publish lightweight, anonymized JSON payloads directly to the **Local MQTT Broker** at regular intervals (e.g., every 30–60 seconds).
   * **Sample Payload:** `{"zone_id": "ride_ferris_wheel_queue", "occupancy_count": 42, "density_level": "HIGH", "est_wait_time_min": 15, "timestamp": "2026-09-15T14:30:00Z"}`
4. **Resilience to Network Drops:** If cloud connectivity drops due to patchy Wi-Fi, metrics are queued locally at the Local MQTT Broker and synchronized asynchronously once connectivity resumes.

---


### **Alternative Considered: Physical BLE / RFID Visitor Tracking**
The alternative approach considered was distributing **Bluetooth Low Energy (BLE) or RFID wristbands/badges** to visitors at entry turnstiles, tracked by a dense network of beacon receivers across the park.

While BLE/RFID provides precise individual movement trajectories, it was **rejected** due to significant operational and architectural drawbacks:
* **Severe Physical & Operational Friction:** Handing out, registering, retrieving, sanitizing, and charging 15,000 wristbands daily creates massive entry turnstile queues and recurring replacement costs from lost hardware.
* **Privacy & Regulatory Liability:** BLE/RFID wristbands continuously broadcast unique device IDs, generating explicit individual movement profiles across time. This creates significant PII compliance burdens under regulations like GDPR.
* **Infrastructure Overhead:** Requires installing and maintaining hundreds of BLE/RFID receiver hubs across a sprawling estate with patchy Wi-Fi.


---

### **Consequences & Trade-offs**

#### **Positive Consequences:**
* Full GDPR/PII compliance out of the box with zero identity exposure.
* Instant real-time alerts for local rules engines when queue capacities exceed thresholds.
* Low maintenance and low network bandwidth footprint.

#### **Negative Consequences & Mitigations:**
* **Lack of Individual Path Trajectory:** *Mitigation:* Individual visitor trajectories are unnecessary for operational optimization. Zone-to-zone aggregate vector flows provide identical macro-level crowd insights without privacy risks.
* **Optical Occlusion & Lighting Variations:** *Mitigation:* Micro-nodes utilize multi-spectrum or infrared-assisted camera modules at indoor/nighttime displays (such as aquatic/piranha enclosures) with background subtraction filtering.
* **Model Misbehavior / Drift:** *Mitigation:* Implement periodic synthetic validation checks and confidence threshold monitoring to detect optical degradation (e.g., dirty camera lenses).

---

### Date
2026-09-15