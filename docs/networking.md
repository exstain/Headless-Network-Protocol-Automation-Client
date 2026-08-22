# Networking & Protocol Processing

## Overview

The client uses ENet as its network communication layer.

ENet provides the underlying network transport and connection management,
while the application implements additional processing logic to interpret
and construct application-level data used by the client.

The networking architecture can be viewed as several layers:

```text
┌─────────────────────────────────────┐
│         Automation Logic            │
├─────────────────────────────────────┤
│      Application State / Events     │
├─────────────────────────────────────┤
│       Protocol Processing           │
│                                     │
│  Packet Parsing / Data Processing   │
│  Packet Construction / Formatting   │
├─────────────────────────────────────┤
│              ENet                   │
│      Network Communication          │
├─────────────────────────────────────┤
│       Underlying Network            │
└─────────────────────────────────────┘
```

---

## ENet Integration

ENet is responsible for the underlying network communication used by the
client.

The client relies on ENet for connection management and network events,
while application-specific logic processes the data received from the
server.

This separation allows the networking layer and application logic to remain
conceptually distinct.

---

## Receiving Data

When network data is received, it is passed from the ENet layer to the
application's packet-processing logic.

The general processing flow is:

```text
Server
  │
  ▼
ENet Receive
  │
  ▼
Incoming Packet
  │
  ▼
Packet Processing
  │
  ▼
Data Interpretation
  │
  ├── World Data
  ├── Character Position
  ├── Chat / Events
  └── Other Protocol Data
  │
  ▼
Application State
  │
  ▼
Automation Logic
```

The client processes the received data according to the expected application
protocol format rather than treating the incoming packet as an opaque
message.

---

## Sending Data

The client uses different packet-sending paths depending on the type of
data being transmitted.

Conceptually, the implementation provides two forms of packet transmission:

### String-Based Packets

Used when the protocol data can be represented through a formatted string
structure.

```text
Application Data
      │
      ▼
Format Packet
      │
      ▼
String Representation
      │
      ▼
ENet
      │
      ▼
Server
```

### Raw Data Packets

Used when the application needs to transmit data represented directly as
raw bytes.

```text
Application Data
      │
      ▼
Construct Raw Data
      │
      ▼
Byte Sequence
      │
      ▼
ENet
      │
      ▼
Server
```

This distinction allows the client to handle different protocol message
requirements without forcing every message into the same representation.

---

## Byte-Level Data Processing

One of the more challenging parts of the project involved processing data
represented as a sequence of bytes.

Some server responses contain multiple pieces of information packed into
the same byte-oriented representation.

During development, I initially received incorrect results when processing
world data.

I investigated the byte layout and studied how the individual bytes
represented different pieces of information.

This required moving from a high-level view of network messages to a
byte-level understanding of the data.

The resulting processing pipeline can be summarized as:

```text
Raw Network Data
      │
      ▼
Byte Sequence
      │
      ▼
Protocol Interpretation
      │
      ▼
Structured Data
      │
      ├── World Information
      ├── Position Information
      ├── Events
      └── Other State
      │
      ▼
Application State
```

---

## Connection Lifecycle

The client maintains a connection lifecycle that allows it to recover from
network interruptions.

```text
START
  │
  ▼
INITIALIZE
  │
  ▼
CONNECT
  │
  ├── Failure ────────► RETRY
  │
  ▼
CONNECTED
  │
  ▼
AUTHENTICATION
  │
  ├── Failure ────────► RECOVERY
  │
  ▼
ACTIVE SESSION
  │
  ▼
AUTOMATION
  │
  ├── Connection Failure
  │
  ▼
RECONNECT
  │
  ▼
AUTHENTICATION
  │
  ▼
RESUME OPERATION
```

---

## Inactivity Detection

A network connection cannot always be considered healthy simply because a
conventional disconnect event has not been received.

To handle this situation, the client also monitors communication activity.

Conceptually:

```text
Monitor Network Activity
          │
          ▼
   Activity Received?
      /          \
    YES           NO
     │             │
     ▼             ▼
 Continue       Timeout
 Operation      Threshold
                   │
                   ▼
             Mark Inactive
                   │
                   ▼
                Reconnect
```

This mechanism improves reliability during long-running operation.

---

## Relationship With Automation

Networking is closely coupled with the automation layer.

Network events update the internal application state, which can then trigger
automation decisions.

For example:

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
Packet Construction
     │
     ▼
ENet
     │
     ▼
Server
```

This creates a continuous feedback loop between network state and automation
behavior.

---

## Engineering Considerations

The networking implementation was designed around several requirements:

- Reliable connection handling
- Automatic recovery
- Application-level packet processing
- Byte-oriented data interpretation
- Separation between network transport and automation logic
- Continuous operation
- Compatibility with resource-constrained hardware

The project helped me develop a practical understanding of network
programming beyond simply opening a socket or sending a message.
