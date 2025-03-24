# Interactive Discovery Protocol (IDP) – Architectural Specification

**Version:** 1.0.0  
**Author:** Don Johnson 
**License:** MIT  

---

## Overview

The **Interactive Discovery Protocol (IDP)** defines a self-describing, structured API interaction framework designed explicitly for clear context handling and robust state management. This document outlines IDP’s architecture, state management concepts, interaction patterns, and schema structure.

---

## Architectural Components

```mermaid
graph TD
    Client -->|Discovery Request| DiscoveryEndpoint[/Discovery Endpoint/]
    DiscoveryEndpoint -->|Protocol Metadata| Client
    Client -->|Structured Request| InteractionEndpoint[/Interaction Endpoint/]
    InteractionEndpoint -->|Manage State| StateMgmt[State Management]
    StateMgmt --> ContextStorage[(Context Storage)]
    ContextStorage --> InteractionEndpoint
    InteractionEndpoint -->|Structured Response| Client
```

---

## State & Context Lifecycle

The lifecycle clearly defines the state transitions within IDP.

```mermaid
stateDiagram-v2
    [*] --> Initialized
    Initialized --> Active : Session Initiated
    Active --> Updated : Context Modified
    Updated --> Active : Context Persisted
    Active --> Expired : Session Timeout
    Updated --> Expired : Session Timeout
    Expired --> [*] : Clean-up Context
```

---

## Interaction Patterns

Typical interaction flows between clients and IDP endpoints:

```mermaid
sequenceDiagram
    participant Client
    participant Discovery
    participant Interaction
    participant StateMgmt

    Client->>Discovery: GET /context
    Discovery-->>Client: Protocol Schema
    Client->>Interaction: Structured Request
    Interaction->>StateMgmt: Update Context
    StateMgmt-->>Interaction: Context Updated
    Interaction-->>Client: Structured Response
```

---

## Schema Definition

The schema is detailed in [`idp-schema.yaml`](./idp-schema.yaml), following OpenAPI v3.1 standards, enabling automated tooling and client generation.
