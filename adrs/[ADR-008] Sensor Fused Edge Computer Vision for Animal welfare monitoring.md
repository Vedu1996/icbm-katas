# ADR: Sensor-Fused Edge Computer Vision for Animal Welfare Monitoring

## Status
 - PROPOSED

## Context

The Von Digitalis Estates manages an exotic collection of over 200 animals across 55 enclosures and displays, including land-based species, aquatic displays, and jumping piranhas. Estate profitability depends on early detection of animal health deterioration to reduce veterinary treatment costs and prevent livestock loss.

The primary architectural constraint is the physical environment: park-wide Wi-Fi coverage is patchy, while budget is specifically allocated for MQTT-capable hardware deployed throughout the grounds. Continuous transmission of raw high-resolution video streams from 55 enclosures to a central cloud platform would saturate the wireless network, create a single point of failure during network dropouts, and fail to provide offline safety monitoring.

## Decision

Implement a **Sensor-Fused Edge Computer Vision and IoT Telemetry Architecture** using local embedded hardware at each enclosure cluster:

- **Hardware & Runtime Stack:**
  - **Processing:** Deploy edge compute units running Linux.
  - **Video Preprocessing & Feature Extraction:** Use **OpenCV** for hardware camera interfacing (RTSP/USB), frame decoding, resizing, region-of-interest (ROI) masking, and color space transformations.
  - **Edge ML Inference:** Run quantized deep neural network models (INT8/FP16 optimized via **ONNX Runtime** or **TensorRT**). Implement pre-trained backbones (e.g., YOLO variants) fine-tuned for specific enclosure ROIs to perform bounding-box action detection, spatial-temporal pose estimation for lethargy tracking, and density-map regression CNNs for schooling piranha counts.
  - **Sensor Integration:** Interface environmental probes (water pH, dissolved oxygen, ambient temperature) and feeder load-cell scales directly with edge microcontrollers via MQTT.
- **Sensor Fusion & Event Generation:**
  - Correlate visual features with physical sensor readings locally (e.g., confirm feeding by cross-referencing visual head-drop pose detection with scale delta weight drops).
  - Convert visual analysis into lightweight structured JSON events (under 1 KB) such as `animal.feeding.session_recorded`, `animal.piranha.density_sampled`, and `animal.behavior.motion_anomaly`.
- **Local Broker & Offline Buffering:**
  - Run a lightweight embedded MQTT broker (e.g., **NanoMQ** or **Eclipse Mosquitto**) equipped with a persistent disk-backed FIFO buffer on each edge node.
  - When Wi-Fi is lost, events append to local disk storage.
  - Upon network recovery, events automatically stream to the central cloud MQTT broker using MQTT QoS 1 (at-least-once delivery).
- **Edge Lifecycle Management:**
  - Deploy a lightweight orchestration agent on each node to manage OTA container rollouts, fine-tuned model weight distribution, and device health monitoring across all 55 displays.

## Alternative Systems Considered

| Alternative Approach                       | Mechanism                                                                                                   | Pros                                                                | Cons / Reason for Rejection                                                                                                                                                                                  |
| :----------------------------------------- | :---------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------ | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Cloud-Centric Computer Vision**          | Stream raw video feeds from 55 enclosures directly to cloud inference APIs (AWS Rekognition / Cloud GPUs).  | Centralized model updates; no edge hardware deployment.             | **Rejected:** Patchy park Wi-Fi makes streaming 55+ concurrent video feeds impossible; creates high bandwidth costs and fails during network outages.                                                        |
| **Pure Sensor-Only (No Cameras)**          | Rely solely on RFID/PIT tags, passive infrared (PIR) motion sensors, and scale load cells.                  | Inexpensive; negligible data bandwidth; no vision models needed.    | **Rejected:** Cannot count or monitor schooling aquatic life like jumping piranhas; cannot distinguish nuanced behaviors such as food spillage vs. genuine consumption.                                      |
| **Active Infrared (IR) Grid & Sonar Only** | Install IR break-beam curtains across water surfaces and acoustic sonar transducers for underwater density. | High sampling speed; works regardless of water clarity or lighting. | **Rejected:** Fragile to splashes and physical debris in piranha tanks; requires specialized acoustic hardware with higher component costs than standard optical sensors; lacks behavioral posture analysis. |

````mermaid
flowchart TD
    %% Styling
    classDef hardware fill:#eef2ff,stroke:#6366f1,stroke-width:1.5px,color:#1e1b4b;
    classDef edgeUnit fill:#f8fafc,stroke:#0f172a,stroke-width:2px,color:#0f172a;
    classDef edgeProc fill:#e0f2fe,stroke:#0284c7,stroke-width:1.5px,color:#0369a1;
    classDef buffer fill:#fef3c7,stroke:#d97706,stroke-width:1.5px,color:#78350f;
    classDef network fill:#f1f5f9,stroke:#64748b,stroke-width:2px,stroke-dasharray: 5 5,color:#334155;
    classDef cloud fill:#ecfdf5,stroke:#059669,stroke-width:2px,color:#064e3b;

    %% Enclosure Physical Inputs
    subgraph SENSORS["Physical Enclosure Inputs (55 Displays)"]
        CAM["Enclosure Cameras<br/>(Underwater & Surface Streams)"]:::hardware
        SCALE["Feeder Load Scales<br/>(Weight Delta)"]:::hardware
        ENV["Environmental Probes<br/>(pH, DO, Temp)"]:::hardware
    end

    %% Edge Compute Boundary
    subgraph NODE["Local Edge Hardware Unit (Cluster Node)"]
        subgraph PIPELINE["Edge Processing & Sensor Fusion"]
            CV["OpenCV Frame Processing<br/>(RTSP, ROI Masking, Resizing)"]:::edgeProc
            ML["Edge ML Inference Engine<br/>(Quantized YOLO / ONNX / TensorRT)"]:::edgeProc
            FUSION["Sensor Fusion Logic<br/>(Cross-verify Visuals + Scales + Probes)"]:::edgeProc
        end

        subgraph STORAGE["Local Queue & Offline Reflex"]
            BROKER["Local Embedded MQTT Broker<br/>(NanoMQ / Mosquitto)"]:::buffer
            DISK[("Persistent Disk Buffer<br/>(FIFO Queue)")]:::buffer
            REFLEX["Rule-Based Reflex Engine<br/>(Tier-1 Offline Triggers)"]:::edgeProc
        end

        AGENT["Edge Orchestration Agent<br/>(OTA Updates & Fleet Health)"]:::edgeProc
    end

    LOCAL_OUT["Local Keepers & Enclosure Buzzers"]:::hardware
    WAN{"Patchy Wi-Fi Network"}:::network

    %% Cloud Boundary
    subgraph CLOUD["Central Cloud Platform"]
        CMQTT["Cloud MQTT Broker<br/>(EMQX / AWS IoT Core)"]:::cloud
        KAFKA["Enterprise Event Stream<br/>(Kafka / Kinesis)"]:::cloud
        AI_GW["Cloud Aggregation & AI Gateway<br/>(Longitudinal Health Baselines)"]:::cloud
    end

    %% Flow Connections
    CAM -->|"Raw Video Frames"| CV
    CV -->|"Preprocessed Tensors"| ML
    ML -->|"Extracted Action Detections"| FUSION

    SCALE -->|"Scale Weight Data"| FUSION
    ENV -->|"Water & Air Telemetry"| FUSION

    FUSION -->|"Structured Events (< 1 KB)"| BROKER

    %% Storage & Offline Path
    BROKER <-->|"Buffer / Read Pending"| DISK
    BROKER -->|"Threshold Breach (Offline)"| REFLEX
    REFLEX -->|"Immediate Hardware Alert"| LOCAL_OUT

    %% Uplink Path
    BROKER -->|"Store & Forward (MQTT QoS 1)"| WAN
    WAN -->|"Restored Connection Stream"| CMQTT
    CMQTT -->|"Forward Ingested Events"| KAFKA
    KAFKA -->|"Telemetry & Anomalies"| AI_GW

    %% Management Plane
    CLOUD -.->|"OTA Model & Config Sync"| AGENT
    AGENT -.->|"Manage Runtimes & Weights"| PIPELINE
    ```
````
