# Concurrency & Multi-Instance Architecture

## Overview

The client supports multiple bot instances running concurrently.

Multithreading was introduced to allow multiple instances to operate
independently while sharing the same underlying application environment.

Concurrency became an important part of the project because the target
deployment required multiple automation instances to remain active at the
same time.

---

## Multi-Instance Model

Each bot instance maintains its own operational workflow.

A simplified representation is:

```text
                    Main Process
                         │
             ┌───────────┼───────────┐
             │           │           │
             ▼           ▼           ▼
          Bot #1       Bot #2       Bot #3
             │           │           │
             ▼           ▼           ▼
        Connection   Connection   Connection
             │           │           │
             ▼           ▼           ▼
        Automation   Automation   Automation
```

Each instance can independently progress through its own connection,
authentication, world processing, and automation workflow.

---

## Threading

The client uses multithreading to support concurrent bot execution.

Conceptually:

```text
Main Program
     │
     ├──► Thread / Bot #1
     │
     ├──► Thread / Bot #2
     │
     ├──► Thread / Bot #3
     │
     └──► Thread / Bot #N
```

This allows the main program to manage multiple active automation instances
without requiring each instance to run sequentially.

---

## Shared Data

Although each bot maintains its own operational state, some resources or
data structures may be accessed by multiple execution contexts.

Shared data introduces synchronization requirements.

```text
Thread A ───────────┐
                    │
                    ▼
              Shared Resource
                    ▲
                    │
Thread B ───────────┘
```

Without appropriate synchronization, concurrent access can produce
unpredictable behavior.

---

## Race Condition

During development, the client experienced intermittent freezes while
multiple threads were active.

The issue was eventually traced to concurrent access to shared data.

A simplified representation of the problem is:

```text
Thread A
   │
   ├── Read Shared Data
   │
   └── Modify Shared Data
              │
              │
              ▼
        Shared Resource
              ▲
              │
   ┌──────────┘
   │
Thread B
   │
   ├── Read Shared Data
   │
   └── Modify Shared Data
```

When multiple threads accessed the same resource without sufficient
synchronization, the program could enter an unsafe state.

---

## Mutex Synchronization

To address the race condition, mutex-based synchronization was introduced
around shared resources that required protected access.

Conceptually:

```text
Thread A ────────┐
                 │
                 ▼
            ┌─────────┐
            │  Mutex  │
            └────┬────┘
                 │
                 ▼
          Shared Resource
                 ▲
                 │
            ┌────┴────┐
            │  Mutex  │
            └─────────┘
                 ▲
                 │
Thread B ────────┘
```

The mutex ensures that protected critical sections are not accessed
simultaneously by conflicting execution paths.

This improved stability during concurrent and long-running operation.

---

## Concurrency Challenges

Supporting multiple instances introduced several practical engineering
considerations:

- Shared data synchronization
- Thread safety
- Memory consumption
- CPU utilization
- Independent instance state
- Network activity occurring concurrently
- Long-running stability

These problems were not always visible when running a single instance,
which made multi-instance testing important.

---

## Reliability Under Concurrency

The concurrency system is closely connected to the reliability mechanisms.

Each instance must be capable of handling its own network lifecycle while
the other instances continue operating.

For example:

```text
Bot #1 ──► Connected ──► Automation
Bot #2 ──► Connection Lost ──► Reconnect
Bot #3 ──► Connected ──► Automation
```

A connection failure in one instance should not require the entire
application to stop.

This separation is important for continuous multi-instance operation.

---

## Engineering Lessons

Working with multiple threads taught me several practical concepts:

- Concurrent execution is different from sequential execution.
- Shared state requires careful synchronization.
- Race conditions can produce intermittent and difficult-to-reproduce bugs.
- Mutexes can protect critical sections from conflicting access.
- Increasing concurrency also increases resource requirements.
- Reliability mechanisms need to operate independently for each instance.

The concurrency problems encountered during development were an important
part of my practical introduction to multithreaded C/C++ programming.
