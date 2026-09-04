## ADR [001]: Event-Driven Architecture with Central Event Bus & Edge Store-and-Forward

### Status
- PROPOSED

### Context
The Von Digitalis Estates require a scalable architecture to support ticket sales, park foot-traffic analytics, and exotic animal monitoring (e.g., piranha counting, feeder tracking). The system must grow from 5,000 to 15,000+ daily visitors over three years across 40 rides and 55 animal enclosures.  The physical park environment presents significant operational constraints:
- Patchy Wi-Fi Coverage: Intermittent connectivity across the large estate means direct API requests to cloud services will fail during outages.  
- Mixed Data Ingestion: System inputs range from high-frequency IoT telemetry (MQTT hardware devices) to transactional operations (ticket purchases) and asynchronous AI analytical pipelines.

### Decision
We will adopt an Event-Driven Architecture (EDA) anchored by a high-throughput Central Event Bus in the cloud (e.g., AWS EventBridge / Apache Kafka), complemented by a Local Store-and-Forward mechanism at the park edge.  
Key structural elements include:
1. Local Edge Gateways: Edge hardware (MQTT brokers like Mosquitto) will collect sensor and turnstile events locally. During network outages, events are persisted to local non-volatile storage queues.  
2. Asynchronous Edge-to-Cloud Sync: Once connectivity is restored, edge nodes flush queued events to the cloud event bus using original device-generated UUIDs (event_id) and timestamps (created_at).  
3. Cloud Event Bus Routing: The central bus routes events asynchronously to targeted downstream domain services, AI analytics engines, and operational dashboards.  
4. Consumer-Side Deduplication: Downstream consumers enforce idempotency via a distributed key-value cache (e.g., Redis TTL check on event_id) to discard duplicate messages generated during network connection recovery

The trade-offs here would be:
- Eventual Consistency: Real-time dashboards and cloud AI models will experience processing delays for data originating from disconnected park zones until connectivity resumes.

### Alternatives Considered
None

### References
- [GeeksForGeeks](https://www.geeksforgeeks.org/system-design/event-driven-architecture-system-design/)
- [Salesforce Trailhead](https://trailhead.salesforce.com/content/learn/modules/platform_events_basics/platform_events_architecture)
- [Cavli Wireless](https://www.cavliwireless.com/blog/nerdiest-of-things/what-is-the-mqtt-protocol)

### Date
2026-09-03


```mermaid
flowchart TD
    %% Define Node Styles
    classDef edgeStyle fill:#e1f5fe,stroke:#0288d1,stroke-width:2px,color:#000;
    classDef cloudStyle fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px,color:#000;
    classDef consumerStyle fill:#e8f5e9,stroke:#388e3c,stroke-width:2px,color:#000;

    subgraph ParkEdge["Park Edge (Local Network)"]
        direction TB
        Devices["Park Sensors, Turnstiles & MQTT Devices<br/><i>(Rides & Enclosures)</i>"]
        Gateway["Local Edge Gateway<br/><i>(Mosquitto MQTT Broker)</i>"]
        Storage_Queue["Local Non-Volatile Storage_Queue<br/><i>(Store-and-Forward)</i>"]
        
        Devices -->|"Telemetry / Events"| Gateway
        Gateway -->|"Intermittent Wi-Fi (Drop/Offline)"| Storage_Queue
    end

    subgraph CloudInfra["Cloud Infrastructure"]
        direction TB
        CentralBus["Central Event Bus<br/><i>(AWS EventBridge / Kafka)</i>"]
        
        subgraph Consumers["Downstream Domain Consumers"]
            Deduper{"Consumer-Side Deduplication<br/><i>(Redis Cache TTL check on event_id)</i>"}
            DomainSvc["Domain Services<br/><i>(Ticketing & Operations)</i>"]
            AIAnalytics["AI Analytics Engine<br/>"]
            Dashboards["Operational Dashboards<br/><i>(Foot-Traffic & Capacity)</i>"]
        end
    end

    %% Flow Connections
    Gateway -->|"Direct Sync (Online)"| CentralBus
    Storage_Queue -->|"Asynchronous Edge-to-Cloud Sync<br/>(Restored Network Connection)"| CentralBus
    
    CentralBus -->|"Route Events"| Deduper
    Deduper -->|"Valid / Unique Event"| DomainSvc
    Deduper -->|"Valid / Unique Event"| AIAnalytics
    Deduper -->|"Valid / Unique Event"| Dashboards
    Deduper -.-x|"Duplicate Event Discarded"| Discard[("Discard Duplicate")]

    %% Apply Styles
    class Devices,Gateway,Storage_Queue edgeStyle;
    class CentralBus cloudStyle;
    class Deduper,DomainSvc,AIAnalytics,Dashboards consumerStyle;