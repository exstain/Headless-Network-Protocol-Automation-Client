# Automation System

## Overview

The automation layer is responsible for executing predefined workflows after
the client successfully establishes a session and enters the required
environment.

The system combines network events, world-state information, movement logic,
and task execution to perform repetitive operations without requiring
continuous manual interaction.

---

## Automation Workflow

The general workflow can be represented as:

```text
Connection
    │
    ▼
Authentication
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
Pathfinding / Movement
    │
    ▼
Plant
    │
    ▼
Wait for Required Condition
    │
    ▼
Harvest
    │
    ▼
PNB
    │
    ▼
Resource Management
    │
    ▼
Repeat
```

The workflow is designed to continue operating for extended periods while
responding to changes in the current application state.

---

## World Processing

Before automation can operate reliably, the client needs information about
the current world.

The networking layer receives the relevant data, while the protocol
processing layer interprets it into information that can be used by the
automation system.

Conceptually:

```text
Server Data
     │
     ▼
Network Receive
     │
     ▼
Protocol Processing
     │
     ▼
World Representation
     │
     ▼
Automation Logic
```

The resulting world information can be used by the automation system to
determine movement and task execution.

---

## Pathfinding and Movement

The automation system includes movement logic for navigating through the
world.

The general process is:

```text
Current Position
       │
       ▼
Determine Target
       │
       ▼
Analyze Available Path
       │
       ▼
Generate Movement Actions
       │
       ▼
Execute Movement
       │
       ▼
Check Current State
       │
       └──────► Continue / Recalculate
```

Pathfinding allows repetitive tasks to be performed without manually
controlling the character.

---

## Planting

The planting workflow uses the current world state and movement logic to
perform planting actions across the required area.

A simplified workflow is:

```text
Load World
   │
   ▼
Determine Planting Area
   │
   ▼
Navigate
   │
   ▼
Plant
   │
   ▼
Move to Next Position
   │
   └──────► Repeat
```

---

## Harvesting

After the required condition for harvesting is reached, the automation
system performs the harvesting workflow.

```text
Check World State
       │
       ▼
Determine Harvest Targets
       │
       ▼
Navigate
       │
       ▼
Harvest
       │
       ▼
Update State
       │
       ▼
Continue Workflow
```

---

## PNB Workflow

PNB is another automated stage of the workflow.

The system uses movement and interaction logic to perform the required
sequence automatically after harvesting.

The simplified workflow is:

```text
Harvest Completed
       │
       ▼
Enter PNB Stage
       │
       ▼
Navigate
       │
       ▼
Perform Required Actions
       │
       ▼
Monitor Result
       │
       ▼
Continue / Complete
```

---

## Resource Management

The automation system also includes resource-management logic.

The general workflow can be represented as:

```text
Automation Output
       │
       ▼
Resource Accumulation
       │
       ▼
Check Resource Threshold
       │
       ├── Below Threshold ──► Continue Automation
       │
       └── Threshold Reached
                  │
                  ▼
             Resource Action
                  │
                  ▼
             Store / Manage
                  │
                  ▼
             Resume Automation
```

This allows the system to continue operating without requiring constant
manual resource management.

---

## Continuous Operation

The automation system is designed around a repeating workflow rather than a
single execution.

```text
┌─────────────────────────────────────┐
│                                     │
│       Load / Process World          │
│                 │                   │
│                 ▼                   │
│             Farming                 │
│                 │                   │
│                 ▼                   │
│             Harvest                 │
│                 │                   │
│                 ▼                   │
│               PNB                   │
│                 │                   │
│                 ▼                   │
│        Resource Management          │
│                 │                   │
│                 └───────────────────┘
│                         │
│                         ▼
│                    Repeat
└─────────────────────────────────────┘
```

The network reliability layer operates alongside this workflow so that
connection problems can trigger recovery and reconnection when necessary.

---

## Automation and Network Events

The automation system does not operate independently from the networking
layer.

Network events can change the current application state and therefore affect
automation decisions.

```text
Network Event
      │
      ▼
Protocol Processing
      │
      ▼
State Update
      │
      ▼
Automation Decision
      │
      ▼
Action
      │
      ▼
Network Request
      │
      ▼
Server
```

This creates a continuous feedback loop between network communication and
automation behavior.

---

## Design Goals

The automation system was designed with several goals:

- Reduce repetitive manual interaction
- Operate continuously
- Respond to application state
- Integrate with the networking layer
- Support multiple concurrent instances
- Recover from network interruptions
- Operate within limited hardware resources

The result is a headless automation workflow capable of running for extended
periods with minimal manual intervention.
