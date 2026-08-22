# HEADLESS NETWORK PROTOCOL AUTOMATION CLIENT

A headless C/C++ network automation client designed to operate continuously
from a Linux/Termux terminal environment on resource-constrained hardware.

The project uses ENet for network communication and implements protocol
processing, automated reconnection, multithreaded bot management,
world-state processing, pathfinding, and long-running automation workflows.

> **Project Status:** Personal Project — Actively Used
>
> **Source Code:** Private

---

## Overview

This project is a C/C++ network automation client that I developed and
extensively modified from an existing open-source starting point.

I studied the original implementation, rebuilt parts of its functionality,
and progressively extended the system with network processing, automation,
multithreading, reliability mechanisms, and resource optimization.

The project evolved into a headless, multi-instance system designed for
continuous operation on resource-constrained hardware.

---

## Project Highlights

### Network Programming

Built and extended a C/C++ networking system using ENet, including
application-level packet processing, connection monitoring, and automatic
reconnection handling.

### Protocol Data Processing

Implemented application-level processing for network data and investigated
byte-oriented data structures to correctly interpret received world-state
information.

### Concurrent Automation

Developed a multithreaded architecture capable of running multiple
automation instances concurrently, with mutex-based synchronization for
shared resources.

### Reliability Engineering

Implemented timeout and inactivity detection mechanisms to recover from
network interruptions during long-running operation.

### Resource-Constrained Deployment

Optimized parts of the application for memory usage and designed the system
to operate without a graphical environment on low-end hardware.

### Practical C/C++ Development

The project became a practical learning environment for C/C++, networking,
multithreading, debugging, memory management, and Linux/Termux development.

---

## Demo

The client operates entirely through a terminal interface without requiring
a graphical user interface.

### Terminal Operation

<img src="screenshots/terminal-demo.png" alt="Terminal Demo" width="400">

### Low-End Hardware Deployment

The client has also been deployed on repurposed low-end hardware for
continuous headless operation.

<img src="screenshots/hardware-deployment.jpg" alt="Low-End Hardware Deployment" width="650">

---

## Key Features

- C/C++ implementation
- ENet-based networking
- Headless terminal operation
- Multi-instance bot execution
- Automated connection and reconnection
- Connection timeout and inactivity detection
- Network packet processing
- World-state data processing
- Character movement and pathfinding
- Automated planting and harvesting
- Automated PNB workflow
- Resource management
- Multithreaded execution
- Mutex-based synchronization
- Memory optimization for low-end hardware
- Long-running 24/7 operation

---

## System Architecture

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

## Technology Stack

| Technology | Purpose |
|---|---|
| C/C++ | Core application |
| ENet | Network communication |
| Linux / Termux | Runtime environment |
| g++ / clang++ | Compilation |
| Bash | Build automation |
| Multithreading | Concurrent bot instances |
| Mutex | Shared-data synchronization |

---

## Program Lifecycle

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

## Engineering Challenges

The project involved solving several practical engineering problems during
development.

### Network Reliability

The client could sometimes remain inactive without receiving a conventional
disconnect event.

I implemented additional timeout and inactivity detection logic so that the
client could identify inactive connections and restart the connection
workflow automatically.

### World Data Processing

World data received from the network was initially decoded incorrectly.

I investigated the byte-level representation of the received data and
modified the processing logic to correctly interpret the information.

### Race Conditions

During multi-instance operation, concurrent access to shared data caused
intermittent freezes.

I investigated the concurrency behavior and introduced mutex-based
synchronization around shared resources.

### Memory Optimization

The client was designed to operate on low-end hardware.

I optimized parts of the implementation to reduce unnecessary memory usage
and improve stability during long-running operation.

---

## Documentation

More detailed technical documentation is available in the `docs/` directory.

- [System Architecture](docs/architecture.md)
- [Automation System](docs/automation.md)
- [Networking & Protocol Processing](docs/networking.md)
- [Concurrency & Multi-Instance Architecture](docs/concurrency.md)
- [Performance & Resource Optimization](docs/optimization.md)
- [Technical Challenges](docs/technical-challenges.md)

---

## Deployment

The client is designed for Linux/Termux environments and can be compiled
using common C++ toolchains.

Typical build environments include:

- g++
- clang++
- build-essential
- Bash

The application is designed to run without a graphical desktop environment.

---

## Hardware Deployment

The software has also been used on repurposed low-end hardware configured
to operate as a lightweight continuous automation system.

The deployment concept focuses on:

- Low resource consumption
- Headless operation
- Continuous execution
- Remote or terminal-based management
- Long-running stability

---

## Project Status

This project remains a personal and actively used system.

The core source code is intentionally kept private because the software is
still in active use.

This repository serves as a technical portfolio and case study documenting
the architecture, engineering challenges, development process, and technical
capabilities of the project.
