# System Architecture

## Overview

The client is designed as a headless network automation system that operates entirely through a Linux/Termux terminal environment.

The system consists of several major components:

- Runtime and configuration
- Network communication
- Protocol and packet processing
- World-state processing
- Automation logic
- Multithreaded bot management
- Reliability and recovery mechanisms
- Resource and memory optimization

---

## High-Level Architecture

```text
                    ┌──────────────────────┐
                    │   Terminal / CLI     │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │ Configuration Layer  │
                    │                      │
                    │ Account / Bot Config │
                    └──────────┬───────────┘
                               │
                               ▼
              ┌────────────────────────────────┐
              │       Automation Engine        │
              │                                │
              │ Movement / Farming / Harvest   │
              │ PNB / Resource Management      │
              └───────────────┬────────────────┘
                              │
                              ▼
              ┌────────────────────────────────┐
              │        Protocol Layer          │
              │                                │
              │ Packet Processing              │
              │ World Data Processing          │
              │ State / Event Handling         │
              └───────────────┬────────────────┘
                              │
                              ▼
              ┌────────────────────────────────┐
              │          ENet Layer            │
              │                                │
              │ Network Connection             │
              │ Send / Receive Events          │
              └───────────────┬────────────────┘
                              │
                              ▼
                       Network Server
```

---

## Program Lifecycle

The general execution flow is:

```text
Program Start
     │
     ▼
Initialize Runtime
     │
     ▼
Load Configuration
     │
     ▼
Initialize Network Layer
     │
     ▼
Connect
     │
     ├── Failed ──► Retry / Reconnect
     │
     ▼
Authenticate
     │
     ├── Failed ──► Recovery / Retry
     │
     ▼
Enter Game
     │
     ▼
Enter Target World
     │
     ▼
Load World Data
     │
     ▼
Process World State
     │
     ▼
Automation
     │
     ├── Plant
     ├── Harvest
     ├── PNB
     └── Resource Management
     │
     ▼
Continuous Operation
     │
     └── Connection Failure ──► Reconnect
```

---

## Multi-Instance Operation

The client supports multiple bot instances through multithreaded execution.

Each instance maintains its own operational state while some resources and data structures may be shared between execution contexts.

Concurrency introduced additional synchronization requirements. During development, race conditions caused intermittent freezes, which led to the implementation of mutex-based synchronization for shared data.

---

## Design Goals

The main engineering goals of the project are:

- Headless operation
- Continuous execution
- Low resource consumption
- Network reliability
- Multiple concurrent instances
- Automated recovery from connection failures
- Operation on resource-constrained hardware
