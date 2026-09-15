## ADR [005]: Buy and Integrate Commercial Task Management System vs. Custom Build

### Status
- PROPOSED

### Context
* **Operational Scale:** Managing daily park operations across **40 amusement rides** and **55 exotic animal displays** (housing 200+ animals) requires continuous, real-time dispatching for feeding, cleaning, ride maintenance, and emergency treatments.
* **Visitor Growth:** Visitor volume is projected to grow from **5,000 to 15,000 visitors per day**, significantly increasing daily task volume and incident reports.
* **Automated Triggers:** Park sensors and local rules engines publish real-time alerts over MQTT (e.g., enclosure water level drops, ride vibration anomalies, piranha count discrepancies), which must automatically generate actionable tasks.
* **Field Constraints:** Employees operate across a sprawling estate with **patchy Wi-Fi connectivity**, requiring mobile task management capabilities that support offline updates and background synchronization.
* **Engineering Focus:** Building a bespoke task engine from scratch (including mobile apps, notification pipelines, offline sync, and scheduling interfaces) consumes substantial engineering capital, distracting from the team's primary differentiator: **AI-driven animal health monitoring and crowd flow predictive analytics**

### Decision
We will **buy** a commercial, enterprise-grade Task / Field Service Management platform (e.g., Jira Service Management, Salesforce Field Service, or Monday.com) and **integrate** it into our cloud infrastructure via an internal **Task Management API Abstraction Service**

* **Preserves Core Focus on AI:** Engineering resources remain concentrated on high-value AI solutions (e.g., computer vision models for animal health and crowd analytics) rather than generic workflow software.
* **Off-the-Shelf Offline Mobile Support:** Field service vendors provide battle-tested mobile clients with native offline caching, push notifications, and automatic sync when reconnecting to Wi-Fi.
* **Speed to Market:** Eliminates months of custom UI/UX development, role-based access control (RBAC), and calendar/scheduling development.

### **Consequences &amp; Trade-offs**

#### **Positive Consequences:**

* Rapid operational readiness for park staff and managers.
* Lower total cost of ownership (TCO) compared to ongoing maintenance of a custom-built workforce platform.
* Elastic scalability as daily visitors scale up to 15,000.

#### **Negative Consequences &amp; Mitigations:**

* **Vendor SaaS Licensing Costs:** *Mitigation:* Offset by avoiding in-house mobile/backend developer maintenance overhead.
* **Vendor Lock-in Risk:** *Mitigation:* The cloud **Task Management Service** acts as an internal abstraction layer (wrapper), decoupling park hardware and database triggers from the third-party vendor's specific REST API endpoints.
* **Third-Party API Outages:** *Mitigation:* Inbound automated events are queued in the cloud MQTT / messaging bus until the third-party API endpoint responds successfully


### Date
2026-09-15