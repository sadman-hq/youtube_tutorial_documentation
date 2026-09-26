# Edge Computing — Structured Notes

## Table of Contents
1. [Introduction](#1-introduction)
2. [Evolution of Computing Architectures](#2-evolution-of-computing-architectures)
3. [What Is the Edge?](#3-what-is-the-edge)
4. [Core Principle: Compute Closer to the Source](#4-core-principle-compute-closer-to-the-source)
5. [Why Edge Computing Is Needed](#5-why-edge-computing-is-needed)
6. [Key Enablers](#6-key-enablers)
7. [Many Edges, Not One](#7-many-edges-not-one)
8. [Benefits & Core Functions](#8-benefits--core-functions)
9. [Edge Machine Learning](#9-edge-machine-learning)
10. [Reliability & High Availability](#10-reliability--high-availability)
11. [Data Preprocessing at the Edge](#11-data-preprocessing-at-the-edge)
12. [Reference Architecture](#12-reference-architecture)
13. [Applications](#13-applications)
14. [Challenges](#14-challenges)
15. [Resource & Network Constraints](#15-resource--network-constraints)
16. [Security Challenges](#16-security-challenges)
17. [Microservices at the Edge](#17-microservices-at-the-edge)
18. [Edge Networking & Addressing](#18-edge-networking--addressing)
19. [Operational Management](#19-operational-management)
20. [Notable Tools & Platforms](#20-notable-tools--platforms)
21. [Cloud + Edge: Complementary, Not Competing](#21-cloud--edge-complementary-not-competing)
22. [Cloud IoT Platforms](#22-cloud-iot-platforms)
23. [Next-Generation Gateways](#23-next-generation-gateways)
24. [Edge vs. Cloud Computing](#24-edge-vs-cloud-computing)
25. [Data Processing Pipeline: Cloud vs. Edge](#25-data-processing-pipeline-cloud-vs-edge)
26. [Advantages & Limitations](#26-advantages--limitations)
27. [Worked Example: Smart Surveillance](#27-worked-example-smart-surveillance)
28. [Edge Computing for AI/ML](#28-edge-computing-for-aiml)
29. [Edge Computing and IoT](#29-edge-computing-and-iot)
30. [Centralized vs. Distributed Responsibilities](#30-centralized-vs-distributed-responsibilities)
31. [Development Lifecycle](#31-development-lifecycle)
32. [Overall Conceptual Model](#32-overall-conceptual-model)
33. [Key Takeaways](#33-key-takeaways)
34. [Conclusion](#34-conclusion)

---

## 1. Introduction

**Edge computing** is a distributed computing paradigm that brings computation, storage, and data processing closer to where data is generated — near sensors, devices, users, or local networks — rather than sending everything to a centralized cloud.

**Guiding principle:**
> *"Centralize where you can, distribute where you must."*

**Drivers:** the growing number of connected devices, rising data volumes, and the need for real-time processing are pushing architectures from heavily centralized cloud models toward more distributed ones.

---

## 2. Evolution of Computing Architectures

| Era | Model | Characteristics |
|---|---|---|
| **Mainframe** | Centralized | Powerful central system; limited endpoint capability; strong dependence on the core |
| **Client-Server** | Distributed | Clients interact with services; servers provide compute/storage; responsibilities split |
| **Cloud** | Centralized (at scale) | Large/geographically distributed data centers; dominant architecture for web, mobile, IoT |
| **Edge** | Distributed | Selected compute moved closer to data sources |

**Historical progression:**
> Mainframes → Client-Server → Cloud → Edge
> (Centralized → Distributed → Centralized → Distributed)

---

## 3. What Is the Edge?

> *"Edge is everything outside of the core cloud."*

The edge can include compute resources near:
IoT devices, sensors, industrial equipment, vehicles, cameras, mobile users, smart infrastructure, local networks, retail/enterprise sites, and telecom infrastructure.

---

## 4. Core Principle: Compute Closer to the Source

**Traditional IoT flow:**
`Sensor → Network → Cloud → Processing → Response`

**Edge-enabled flow:**
`Sensor → Edge Node → Local Processing → Response` (only select data forwarded to the cloud)

This matters most when applications need rapid responses, face limited connectivity, generate large data volumes, or require local processing.

---

## 5. Why Edge Computing Is Needed

| Driver | Explanation |
|---|---|
| **Internet of Things** | Explosive growth of connected devices (temperature sensors, cameras, industrial machines, smart meters, vehicles, wearables) generating continuous data streams |
| **Increasing data volume** | Sending all raw data to the cloud causes network congestion, higher bandwidth needs, transmission costs, and centralized processing load |
| **Real-time processing** | Applications like autonomous vehicles, industrial control, security systems, AR, robotics, and real-time monitoring need short decision latencies |
| **Increasing compute resources at the edge** | Modern embedded systems and SoCs offer strong performance at low power, making local processing practical |

---

## 6. Key Enablers

| Enabler | Role |
|---|---|
| **Cloud-native computing** | Containers, microservices, orchestration, CI/CD simplify packaging and deploying distributed apps |
| **5G** | Improved connectivity, capacity, and latency support for distributed environments |
| **Machine learning** | Models trained centrally, deployed for local inference; edge-specific training possible under certain data/environmental policies |
| **Efficient hardware** | Power-efficient SoCs enable sophisticated processing outside traditional data centers |

---

## 7. Many Edges, Not One

> *"There are many edges."*

Edge environments exist at multiple points between end devices and the cloud:

`Device → Local Edge → Regional Edge → Cloud`

Applications place computation at different levels depending on latency, resource, privacy, and connectivity needs.

---

## 8. Benefits & Core Functions

### 8.1 Low-Latency Processing
- **Local event processing** — react to sensor/scheduled/device events without round-tripping to the cloud
- **Compute offloading** — move resource-intensive tasks (AR/VR rendering, computer vision, local analytics) to edge hardware

---

## 9. Edge Machine Learning

**Typical split:**

| Cloud | Edge |
|---|---|
| Model training | Model deployment |
| Large-scale data processing | Local inference |
| Model management | Real-time decisions |

**Example:** a computer-vision model trained in the cloud is deployed to an edge camera system, which performs inference locally instead of streaming every frame to the cloud. Edge-specific training is also possible when environmental conditions or data policies require it.

---

## 10. Reliability & High Availability

### 10.1 Buffering and Batching
Pattern: `Store → Process/Buffer → Forward` — known as **store-and-forward**, often implemented via brokers running on edge nodes.

### 10.2 Local Caching
Edge nodes keep partial local databases/cached data, reducing reliance on constant cloud retrieval, and synchronize with the cloud or other edge nodes as connectivity allows.

---

## 11. Data Preprocessing at the Edge

| Function | Description |
|---|---|
| **Data sensitivity** | Special handling for privacy/regulatory needs (e.g., GDPR) |
| **Data normalization** | Convert raw device data into structured, consistent formats |
| **Data analytics** | Determine what's relevant: `Raw sensor data → Edge analysis → Relevant information → Cloud`; can combine multiple sources before forwarding |
| **Metadata addition** | Enrich data with location, identity, or security information |

---

## 12. Reference Architecture

```
                 Cloud
        Storage / Analytics / Model
        Training / Business Services
                   │
            Network Connection
                   │
             ┌─────┴─────┐
             │ Edge Layer │
             │ Node/Gateway/Server │
             │ Processing/Caching/ML │
             │ Filtering/Analytics │
             └─────┬─────┘
        ┌──────────┼──────────┐
        ▼          ▼          ▼
     Sensor      Camera     Device
```

The edge does not eliminate the cloud — it distributes processing responsibilities between the two layers.

---

## 13. Applications

| Domain | Use Cases |
|---|---|
| **Large-scale IoT / IIoT** | Local monitoring, machine analytics, predictive maintenance, industrial automation, real-time control |
| **Smart infrastructure** | Smart buildings, cities, transportation, utilities |
| **Gaming** | Reduced latency via distributed processing |
| **VR/AR** | Rapid rendering closer to users |
| **AI/ML** | Inference near the data source |
| **Automotive / autonomous vehicles** | Local processing of camera, radar, lidar data; latency-critical decisions |
| **Security & surveillance** | Local video analysis instead of transmitting raw streams |

---

## 14. Challenges

Distributing computation across many locations introduces management and infrastructure challenges, organized around three planes:

| Plane | Concerns |
|---|---|
| **Infrastructure** | Managing/monitoring geographically distributed nodes; workload distribution; failure handling; heterogeneous hardware |
| **Control plane** | Workload deployment, scheduling, resource allocation, prioritization, monitoring, lifecycle management |
| **Data plane** | Communication between edge↔cloud and edge↔edge sites |

---

## 15. Resource & Network Constraints

**Limited edge resources:**
- Fewer compute nodes than public clouds
- No easy on-demand scaling (unlike the cloud)
- Competing workload priorities require careful triage

**Network constraints:**
- Limited/variable capacity
- Workloads differ in network requirements, priorities, policies, and latency needs — closely tied to resource management

---

## 16. Security Challenges

| Concern | Description |
|---|---|
| **Unattended operation** | Devices may run without continuous human supervision |
| **Physical security** | Hardware may be accessible to unauthorized individuals |
| **Image integrity** | Software/container images must be authenticated and protected |
| **Secure secret delivery** | Credentials, keys, and certificates need secure distribution |
| **Unauthorized microservices** | Prevent unapproved services from deploying/running |
| **Controlled resource access** | Grant only the permissions/resources each service needs |
| **Remote shutdown** | Ability to securely disable compromised/malfunctioning nodes remotely |

---

## 17. Microservices at the Edge

Decomposing applications into independently deployable microservices lets components run wherever most appropriate.

**Considerations:** deployment, resource management, pod priorities, communication, security, matching services to hardware, preventing unauthorized outbound communication.

**Example decomposition:**
```
Application
 ├── Data Collection Service
 ├── Analytics Service
 ├── ML Inference Service
 └── Data Synchronization Service
```
Each service can run on a different edge node based on resource/latency needs.

---

## 18. Edge Networking & Addressing

### Edge Networking
Involves hybrid cloud, microservice architectures, agile integration, and multiple private subnetworks. Unlike traditional client-server networks with well-defined boundaries, edge environments span many independent networks — complicating service discovery and communication.

### Application-Layer Addressing
Addresses services at the application level rather than relying purely on network topology — important when services span different edge clusters, private/public networks, and IP schemes.

### Security Implications of Application Addressing
| Mechanism | Purpose |
|---|---|
| **Access control** | Enforced at address, service, process, or business-function level |
| **Locked-down network membership** | Mutual TLS between sites |
| **Limited public exposure** | Cross-cluster apps don't need conventional Kubernetes network exposure; ingress can be tightly controlled |
| **Trusted/untrusted edges** | Different trust levels require different security policies |

---

## 19. Operational Management

Monitoring should extend beyond low-level infrastructure metrics to **business-resolution** visibility — insight into application and business-level behavior — which becomes increasingly important as the number of distributed nodes grows.

---

## 20. Notable Tools & Platforms

| Tool/Platform | Role |
|---|---|
| **Skupper.io** | Multi-cluster networking: easy deployment, no elevated privileges needed, supports overlapping CIDR subnets, IPv4/IPv6, redundant topologies (no single point of failure), and protocols beyond messaging (HTTP, TCP, UDP) |
| **Eclipse ioFog** | Open-source platform for managing edge computing infrastructure |
| **Edge Compute Network (ECN)** | Networking/computing environment connecting distributed edge resources so applications can operate across multiple edge locations |

---

## 21. Cloud + Edge: Complementary, Not Competing

> *"Cloud is not obsolete."*

Cloud and edge work together:

```
                CLOUD
   Global Analytics / Model Training /
   Long-Term Storage / Business Services
                  │
             Cloud Network
                  │
         ┌────────┴────────┐
       EDGE A             EDGE B
   Local ML/Cache/     Local ML/Cache/
      Analytics           Analytics
        │                    │
     Devices              Devices
```

---

## 22. Cloud IoT Platforms

**Eclipse Hono** and **Eclipse Ditto** remain relevant cloud IoT platforms — edge computing extends, rather than replaces, this infrastructure.

**Eclipse Hono** — connects devices to backend services via protocol adapters:
```
Devices → Protocol Adapters → AMQP/Messaging Network → Business Services
```

**Eclipse Ditto** — part of the broader Eclipse IoT ecosystem, supporting management of connected-device representations and services alongside distributed edge deployments.

---

## 23. Next-Generation Gateways

Traditional gateways are evolving via **cloud-native development** into more capable platforms, adding: more compute resources, caching, analytics, machine learning, and CI/CD — making them a key component of modern edge architectures.

---

## 24. Edge vs. Cloud Computing

| Aspect | Cloud Computing | Edge Computing |
|---|---|---|
| Processing location | Centralized/remote | Near data source |
| Primary objective | Large-scale centralized computing | Local, distributed processing |
| Latency | Higher (network-distance dependent) | Often lower |
| Data movement | Large volumes to cloud | Filtered locally |
| Resources | Highly scalable | Often constrained |
| Connectivity dependency | Strong | Can operate locally during outages |
| Deployment | Fewer, large data centers | Many distributed nodes |
| Management | Relatively centralized | More complex/distributed |
| Physical security | Controlled data centers | Potentially exposed locations |
| Typical workloads | Storage, large-scale analytics, model training | Real-time processing, local inference, filtering |

Neither replaces the other — modern systems combine both.

---

## 25. Data Processing Pipeline: Cloud vs. Edge

**Traditional cloud-centric:**
`Data Source → Network → Cloud → Processing → Result`

**Edge-enabled:**
```
Data Source → Edge Processing
              ├── Local Decision
              ├── Filtering
              ├── Analytics
              ├── ML Inference
              └── Caching
                    ↓
              Selected Data → Cloud
```

---

## 26. Advantages & Limitations

### Advantages
- Reduced latency
- Reduced data transmission (only relevant/processed data sent onward)
- Continued local processing during connectivity issues
- Improved resilience (buffering, caching, store-and-forward)
- Distributed machine learning near the data source
- Data preprocessing (normalize, filter, combine, enrich)
- Support for real-time applications (AR/VR, autonomous vehicles, industrial systems, surveillance)

### Limitations
- Limited computational resources
- Limited network capacity
- Complex distributed infrastructure and workload scheduling
- Physical security risks
- Software/image integrity concerns
- Secure secret management
- Complex inter-edge networking
- Harder monitoring/observability
- Heterogeneous hardware
- More complicated deployment and maintenance

Moving computation to the edge is not simply relocating a cloud architecture — it requires real architectural and operational changes.

---

## 27. Worked Example: Smart Surveillance

**Without edge computing:**
`Camera → Raw Video → Network → Cloud → Video Analysis → Alert`
Requires substantial bandwidth since raw video is transmitted continuously.

**With edge computing:**
```
Camera → Edge Computer → Object Detection → Relevant Event?
                                             ├── No  → Discard/store locally
                                             └── Yes → Event Metadata → Cloud
```
The edge performs local analysis and forwards only relevant information — illustrating local processing, data reduction, metadata enrichment, and cloud-edge cooperation.

---

## 28. Edge Computing for AI/ML

```
        CLOUD
  Data Collection / Model
  Training / Model Management
            │
      Model Deployment
            │
            ▼
         EDGE
  Local Inference / Event
  Detection / Data Filtering
            │
            ▼
         Devices
```

Computationally intensive training stays centralized while latency-sensitive inference happens near users/devices.

---

## 29. Edge Computing and IoT

IoT systems commonly combine large device counts, continuous data generation, heterogeneous protocols, limited device resources, real-time requirements, and variable connectivity — making edge computing especially valuable for **large-scale IoT and IIoT**, while cloud IoT platforms remain important alongside distributed edge deployments.

---

## 30. Centralized vs. Distributed Responsibilities

| Suitable for the Edge | Suitable for the Cloud |
|---|---|
| Real-time inference | Large-scale model training |
| Local event detection | Long-term storage |
| Data filtering | Global analytics |
| Local caching | Centralized business services |
| Immediate control decisions | Cross-site aggregation |
| Latency-sensitive computation | Large-scale resource management |
| Data preprocessing | |

The optimal architecture usually combines both.

---

## 31. Development Lifecycle

Cloud-native approaches support distributed edge development through: containerized applications, microservices, automated deployment, CI/CD, service discovery, distributed monitoring, and workload orchestration — reflected in the movement toward cloud-native gateway development.

---

## 32. Overall Conceptual Model

```
                        CLOUD
        Global Analytics / Model Training /
        Long-Term Storage / Business Services /
                  IoT Platforms
                        │
                 Cloud Connectivity
                        │
        ┌───────────────┼───────────────┐
        ▼                ▼                ▼
   EDGE SITE A       EDGE SITE B       EDGE SITE C
  Processing/         Processing/       Processing/
  Caching/Filtering   Analytics/ML      ML-AI/Filtering
        │                ▼                │
  IoT Sensors      Cameras/Devices    Vehicles/IoT
```

Computation and services are distributed across multiple locations while remaining integrated with centralized cloud infrastructure.

---

## 33. Key Takeaways

1. Edge computing brings compute resources closer to data sources.
2. The edge exists outside the core cloud.
3. IoT and increasing data volumes are major drivers.
4. Real-time processing is a major motivation.
5. 5G, cloud-native computing, ML, and efficient hardware enable edge computing.
6. There is no single edge — many distributed edges can exist.
7. Edge nodes perform local computation and data preprocessing.
8. Caching and store-and-forward mechanisms improve reliability.
9. Edge AI lets ML models execute near data sources.
10. Edge computing supports IoT, IIoT, AR/VR, AI/ML, automotive, smart infrastructure, gaming, and surveillance.
11. Resource and network limitations make edge management challenging.
12. Security matters more since edge nodes may operate unattended and physically exposed.
13. Microservices and cloud-native technologies are key to edge deployment.
14. Application-layer addressing simplifies communication across distributed edges.
15. Cloud computing is not obsolete — cloud and edge work together.
16. Gateways are evolving into more capable cloud-native edge platforms.

---

## 34. Conclusion

Edge computing represents a shift toward distributed computing closer to where data is generated and consumed, driven by growing IoT scale, rising data volumes, real-time processing needs, and increasingly capable edge hardware.

Its fundamental principle is not to eliminate the cloud but to **distribute computation appropriately**: edge nodes handle latency-sensitive processing, ML inference, preprocessing, caching, analytics, and local decisions, while the cloud provides large-scale storage, model training, centralized analytics, and business services.

It also introduces real challenges — resource limitations, networking, workload management, security, distributed deployment, and operational monitoring — addressed through cloud-native computing, microservices, 5G, machine learning, efficient hardware, and open-source edge platforms.

> **Centralize where you can, distribute where you must.**

The future of computing is not a choice between cloud and edge, but a coordinated architecture where both work together according to each application's computational, latency, data, networking, and operational needs.
