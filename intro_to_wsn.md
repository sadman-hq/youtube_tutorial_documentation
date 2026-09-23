# Wireless Sensor Networks (WSNs) — Structured Notes

## Table of Contents
1. [Introduction](#1-introduction)
2. [Enabling Technologies](#2-enabling-technologies)
3. [Definition](#3-definition)
4. [Architecture](#4-architecture)
5. [Node Components](#5-node-components)
6. [Characteristics](#6-characteristics)
7. [WSNs vs. Traditional Networks](#7-wsns-vs-traditional-networks)
8. [Design Challenges](#8-design-challenges)
9. [Hardware & Software Requirements](#9-hardware--software-requirements)
10. [Operating Systems & Middleware](#10-operating-systems--middleware)
11. [Example Platform: ZigBex](#11-example-platform-zigbex)
12. [Applications](#12-applications)
13. [Case Study: Great Duck Island](#13-case-study-great-duck-island)
14. [Advantages & Limitations](#14-advantages--limitations)
15. [Operational Flow](#15-operational-flow)
16. [Core Design Principle](#16-core-design-principle)
17. [Summary](#17-summary)

---

## 1. Introduction

A **Wireless Sensor Network (WSN)** is a distributed network of small, battery-powered devices — called **sensor nodes** or **motes** — that combine sensing, processing, storage, and wireless communication. Nodes are deployed (often ad hoc) throughout a physical environment and cooperate to sense phenomena and relay information to one or more **base stations**.

WSNs sit at the convergence of:
- Sensing
- Embedded computing
- Wireless communication
- Networking
- Distributed systems

The goal is not just data collection, but enabling many resource-constrained devices to **collaboratively** perform tasks a single device could not.

---

## 2. Enabling Technologies

| Enabler | Description |
|---|---|
| **Embedded networked sensing** | Small computing/sensing devices embedded in the environment, able to sense, process, communicate, cooperate, and act |
| **Miniaturization** | MEMS, NEMS, VLSI, and integrated circuits drive down size and cost |
| **Wireless communication** | Low-power, low-data-rate, short-range radios reduce energy use |
| **Computing & networking advances** | Cheap processors, compilers, OS concepts, networking theory, open-source development, regulatory changes, increased bandwidth |

---

## 3. Definition

A WSN is a network of spatially distributed sensor nodes that communicate wirelessly and collaboratively monitor physical phenomena. Nodes are typically:

- Small
- Battery-powered
- Wireless
- Capable of sensing, processing, and communication

**Conceptually:**
> WSN = Sensing + Processing + Communication + Storage + Power

Individually, a node is resource-constrained; collectively, the network can perform sophisticated monitoring and coordination.

---

## 4. Architecture

A typical WSN data path:

```
Physical Environment
        │
        ▼
  ┌─────────────┐
  │ Sensor Nodes │  (sensor, processor, memory, radio, power)
  └──────┬──────┘
         │  wireless network (multi-hop)
         ▼
  ┌─────────────┐
  │   Gateway /  │
  │ Base Station │
  └──────┬──────┘
         ▼
      Server
         ▼
      Internet
```

A sensor field contains motes with sensor interfaces and radios, connected through a gateway to a server and, ultimately, the Internet.

---

## 5. Node Components

| Component | Role | Notes |
|---|---|---|
| **Sensor** | Converts physical phenomena into measurable signals | Passive (temperature, humidity, seismic, acoustic, IR, salinity) vs. Active (radar, sonar — higher energy use) |
| **Processor** | Local computation & control | Low-power; handles filtering, aggregation, protocol execution |
| **Memory/Storage** | Holds program code, buffers, measurements | Limited — favors local aggregation over raw storage/transmission |
| **Radio** | Wireless communication | Low power, low data rate, short range → enables multi-hop routing |
| **Power** | Supplies energy to all components | Usually battery-based; Available Energy ≪ Energy in conventional computers |

Energy is consumed by sensing, processing, transmission, reception, memory operations, and even idle states — and is generally the **scarcest resource**, directly determining network lifetime.

---

## 6. Characteristics

| Characteristic | Description |
|---|---|
| **Energy constrained** | Trade-off between network performance and network lifetime |
| **Distributed operation** | Many small nodes instead of one powerful device |
| **Self-organization** | Nodes configure themselves without central management |
| **Self-healing** | Network continues functioning despite individual node failures |
| **Scalability** | Can scale to hundreds or thousands of nodes |
| **Dynamic topology** | Changes due to node failure, mobility, power loss, or link disruption |
| **Dense deployment** | Example density ~20 nodes/m³ |
| **Broadcast-oriented communication** | Unlike point-to-point traditional networks |
| **Limited/absent global IDs** | Global addressing may be impractical at large scale |

---

## 7. WSNs vs. Traditional Networks

| Feature | Traditional Network | WSN |
|---|---|---|
| Energy | Comparatively abundant | Severely constrained |
| Node count | Relatively fewer | Hundreds–thousands |
| Processing | More capable | Limited |
| Memory | Larger | Limited |
| Communication | Higher capacity | Low-power, low-rate |
| Topology | Relatively stable | Frequently changing |
| Node role | Endpoint or router | Both host and router |
| Communication style | Point-to-point | Mainly broadcast |
| Deployment | Usually planned | Often ad hoc |
| Failures | Less central | Expected/common |
| Identification | Global addressing common | Global IDs often impractical |

**Host + router duality:** A node can simultaneously sense/process its own data *and* forward data from neighbors (e.g., Sensor A → Sensor B → Sensor C → Base Station), enabling multi-hop delivery.

---

## 8. Design Challenges

- **Energy efficiency** — maximize network lifetime without sacrificing performance
- **Fault tolerance** — continue operating despite node failures (critical in remote/hostile/long-term deployments)
- **Self-configuration** — impractical to manually configure large networks
- **Heterogeneity** — varying processing power, communication capability, sensors, energy, and roles
- **Adaptability** — respond to changing conditions, failures, and application needs
- **Security & privacy** — protect sensitive data, especially in hostile or unattended environments

---

## 9. Hardware & Software Requirements

### Hardware Goals
Small, low-cost, energy-efficient, robust, sensing/communication-capable.

**Example platforms:** BTnode, Atlas, Mica Mote, XYZ node, WINS, SensiNet Smart Sensors, Smart Dust, COTS Dust, Sensor Webs, EYES, Hoarder Board.

### Software Requirements
| Requirement | Purpose |
|---|---|
| Lifetime maximization | Minimize unnecessary energy use |
| Robustness | Function under changing conditions/partial failure |
| Fault tolerance | Tolerate individual node failures |
| Self-configuration | Automatic network setup |
| Security | Protect communication and data |
| Mobility support | Handle mobile nodes/base stations |
| Middleware | Abstract hardware from application logic |

---

## 10. Operating Systems & Middleware

### Operating Systems
TinyOS (embedded OS for Berkeley motes, most prominent), Contiki, BTnut Nut/OS, MANTIS, SOS, SenOS, EYESOS, MagnetOS, CORMOS, Bertha.

### Middleware Approaches
1. Distributed database approaches
2. Mobile-agent approaches
3. Event-based approaches

**Examples:** AutoSec, COMiS, COUGAR, DSWare, Enviro-Track, Global Sensor Networks (GSN), Impala, MagnetOS, MiLAN, SensorWare, SINA, TinyDB, TinyGALS.

---

## 11. Example Platform: ZigBex

| Component | Specification |
|---|---|
| Microcontroller | Atmel 8-bit RISC |
| Program memory | 128 KB Flash |
| SRAM | 4 KB |
| Radio transceiver | Chipcon CC2420 |
| Radio range | ~130 m |
| Data rate | 240 Kbit/s |
| Frequency | 2.4 GHz ISM |
| Operating systems | TinyOS, Nano-Qplus |
| Extra capability | RFID reader/tag |
| Sensors | Base sensor + multimodal sensor board |

---

## 12. Applications

### 12.1 General Engineering
Automotive telematics, industrial plant sensing/maintenance, aircraft monitoring, smart offices, goods/container tracking, vehicle safety and traffic systems.

### 12.2 Agricultural & Environmental
- Precision agriculture (crop, livestock, fertilizer, environmental monitoring)
- Planetary exploration
- Geophysical (seismic) monitoring
- Freshwater quality monitoring
- Wildlife monitoring
- Disaster detection
- Contaminant transport assessment

### 12.3 Civil Engineering
Structural monitoring, urban planning, disaster detection.

### 12.4 Military
Troop/weapons/supply monitoring, surveillance, battle-space monitoring, urban warfare, self-healing minefields, targeting, battle damage assessment, NBC (nuclear/biological/chemical) attack detection.

### 12.5 Healthcare & Medical
- Medical sensing (temperature, blood pressure, pulse)
- MEMS-based microsurgery
- Hospital monitoring (doctors, patients, drug administration)
- Elderly assistance

### 12.6 Home
Home automation and smart environments.

### 12.7 Commercial
Office environmental control, interactive museums, vehicle theft detection, inventory management, vehicle tracking.

---

## 13. Case Study: Great Duck Island

- **Start:** Spring 2002
- **Partners:** Intel Research Laboratory (Berkeley), College of the Atlantic, UC Berkeley
- **Location:** Great Duck Island, Maine
- **Goal:** Monitor microclimates around nesting burrows of the **Leach's Storm Petrel**
- **Significance:** Demonstrated non-intrusive, continuous habitat/wildlife monitoring without disturbing the environment

---

## 14. Advantages & Limitations

### Advantages
- Distributed monitoring over large areas
- Remote operation in hard-to-access environments
- High spatial and temporal resolution
- Scalability to large node counts
- Autonomous (self-configuring, self-healing) operation
- Multi-functional/multi-modal sensing

### Limitations
- Limited energy (finite battery life)
- Limited processing power
- Limited storage
- Limited communication range
- Unreliable wireless links
- Frequent node failures
- Security and privacy concerns

---

## 15. Operational Flow

1. **Deployment** — nodes distributed, often ad hoc
2. **Sensing** — nodes measure physical phenomena
3. **Local processing** — data processed/filtered at the node
4. **Communication** — nodes exchange data over wireless links
5. **Multi-hop forwarding** — data relayed through intermediate nodes
6. **Gateway/base station** — data aggregated at the network edge
7. **Server/application** — data delivered for analysis, visualization, or control

---

## 16. Core Design Principle

> **Many Resource-Constrained Nodes → Collaborative Sensing → Distributed Processing → Useful Information**

Each node is intentionally small and limited; system-level intelligence emerges from **cooperation among many nodes** — this is the defining distinction between a WSN and a standalone sensing device.

---

## 17. Summary

WSNs combine sensing, embedded computing, wireless communication, and distributed networking to observe and interact with the physical world. A typical node contains a **sensor, processor, memory/storage, radio, and power source**.

**Key characteristics:** severe energy constraints, limited processing/storage, low-power wireless communication, large node counts, dense deployment, dynamic topology, self-organization, self-healing, heterogeneity, adaptability, and security/privacy needs.

**Enabled by:** wireless technology, MEMS/NEMS, VLSI, embedded computing, networking theory, and low-cost hardware.

**Used across:** environmental monitoring, agriculture, industrial systems, civil engineering, military, healthcare, smart homes, offices, inventory management, wildlife monitoring, and vehicle tracking.

The central idea is not simply *wireless sensing*, but **collaborative distributed sensing** — many small, resource-constrained devices cooperating to produce information no single isolated sensor could provide.
