# Development History

## Background

The project began in 2025 from an existing open-source C/C++ codebase that I
found while looking for a starting point for network-based automation.

At the time, I had limited experience with C/C++ and did not initially
understand the complete implementation.

The original project was no longer functional for my intended use and mainly
provided basic connectivity functionality without the automation workflow
that I needed.

Rather than using it unchanged, I studied the codebase and gradually modified
and extended its implementation.

---

## Initial Stage

The first stage focused on understanding the existing implementation.

I had to learn how several unfamiliar components worked together, including:

- C/C++ application structure
- ENet networking
- Network packet processing
- Byte-oriented data
- Connection management
- Existing application state

This stage was also part of my practical learning process in C/C++.

---

## Networking Improvements

One of the early problems was unreliable reconnection behavior.

The client could sometimes stop communicating with the server without
immediately producing a conventional disconnect event.

I investigated the connection lifecycle and implemented additional timeout
and inactivity detection mechanisms.

This allowed the client to recognize inactive connections and restart its
connection workflow.

---

## Protocol and Data Processing

Another significant challenge was processing the data received from the
server.

The initial world-data decoding did not produce the expected results.

I investigated the raw byte representation and studied how the data was
structured.

After understanding the data layout, I modified the processing logic so that
the received information could be interpreted correctly by the application.

This was one of the stages where I gained practical experience with
byte-level data processing.

---

## Automation Development

After the networking and protocol-processing components became functional,
I progressively developed the automation workflow.

The system evolved to support:

- World navigation
- Pathfinding
- Planting
- Harvesting
- PNB
- Resource management
- Continuous operation

These components were integrated with the networking and world-state
processing layers.

---

## Multi-Instance Support

The project was later extended to support multiple bot instances running
concurrently.

This introduced new engineering challenges involving:

- Thread management
- Shared data
- Race conditions
- Synchronization
- Memory consumption
- Long-running stability

The application was adapted to use multithreaded execution and mutex-based
synchronization where shared resources required protection.

---

## Resource Optimization

Because the application was intended to operate on low-end hardware, memory
usage became an important consideration.

I optimized parts of the implementation to reduce unnecessary memory
consumption and improve stability during extended operation.

The headless terminal architecture also reduced the need for graphical
resources.

---

## Current Architecture

The project has evolved from its original starting point into a substantially
modified personal system consisting of several interconnected layers:

```text
┌──────────────────────────────┐
│      Terminal / Control      │
├──────────────────────────────┤
│      Bot Management          │
├──────────────────────────────┤
│      Automation Engine       │
├──────────────────────────────┤
│      World State Processing  │
├──────────────────────────────┤
│      Protocol Processing     │
├──────────────────────────────┤
│      ENet Networking         │
└──────────────────────────────┘
```

The system is designed to operate continuously in a Linux/Termux
environment and can run multiple instances concurrently.

---

## Learning Outcome

Working on this project gave me practical experience in areas that I had
previously only encountered at a conceptual level.

These include:

- C/C++ development
- Network programming
- ENet
- Protocol processing
- Byte-level data handling
- Multithreading
- Mutex synchronization
- Memory optimization
- Linux/Termux environments
- Debugging
- Long-running software reliability

The project also taught me how to approach an unfamiliar codebase,
investigate failures, understand underlying mechanisms, and progressively
modify a system rather than relying only on pre-existing functionality.

---

## Project Philosophy

The project represents a progression from learning existing code to
understanding and modifying the underlying systems.

The most important outcome was not simply the final automation functionality,
but the engineering experience gained from debugging, reverse-engineering
data structures, solving concurrency problems, optimizing resources, and
building a system capable of long-running operation.
