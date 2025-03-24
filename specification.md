# Interactive Discovery Protocol (IDP) – Architectural Specification

**Version:** 1.0.0  
**Maintainer:** [copyleftdev](https://github.com/copyleftdev)  
**License:** MIT  

---

## Overview

The **Interactive Discovery Protocol (IDP)** is a schema-driven, language-agnostic protocol designed for explicit state management, comprehensive context tracking, and secure, structured API interactions. IDP defines clear interaction patterns, lifecycle events, and security guidelines.

---

## Architectural Components

```mermaid
graph TD
    Client -->|Discovery Request| DiscoveryEndpoint[/Discovery Endpoint/]
    DiscoveryEndpoint -->|Protocol Metadata| Client
    Client -->|API Request| InteractionEndpoint[/Interaction Endpoint/]
    InteractionEndpoint -->|Manage State| StateMgmt[State Management]
    StateMgmt --> ContextStorage[(Context Storage)]
    ContextStorage --> InteractionEndpoint
    InteractionEndpoint -->|Structured Response| Client
    InteractionEndpoint -->|Trigger Event| WebhookHandler[Webhook/Event Handler]
    WebhookHandler -->|Notify| Client
```

---

## State & Context Lifecycle

```mermaid
stateDiagram-v2
    [*] --> Initialized
    Initialized --> Active : Session Start
    Active --> Updated : Context Modified
    Updated --> Active : Persisted
    Active --> Expired : Session Expired
    Updated --> Expired : Session Expired
    Expired --> [*] : Cleanup
```

---

## Interaction Patterns

Typical interaction flow between clients and IDP endpoints:

```mermaid
sequenceDiagram
    participant Client
    participant Discovery
    participant Interaction
    participant StateMgmt
    participant WebhookHandler

    Client->>Discovery: GET /context
    Discovery-->>Client: Protocol Metadata
    Client->>Interaction: Structured Request
    Interaction->>StateMgmt: Update Context
    StateMgmt-->>Interaction: Context Updated
    Interaction-->>Client: Structured Response
    Interaction->>WebhookHandler: Trigger Event
    WebhookHandler-->>Client: Push Notification
```

---

## Error Handling

All errors follow standardized JSON schema responses:

```json
{
  "error_code": "InvalidRequest",
  "error_message": "Detailed error description."
}
```

---

## Security & Authentication

IDP implementations require:

- **TLS** for all interactions.
- **Bearer Token Authentication** (JWT recommended).
- **Rate limiting** and input validation (OWASP API Security Guidelines).

---

## Versioning Strategy

IDP follows Semantic Versioning (`MAJOR.MINOR.PATCH`):

- **Major**: Breaking changes  
- **Minor**: Backward-compatible additions  
- **Patch**: Bug fixes or minor improvements  

Clients identify supported versions through `/context`.

---

## Observability & Logging

Implementations must support observability best practices:

- Structured logging (JSON format recommended)
- Standardized metrics (OpenTelemetry)

---

## Internationalization (i18n)

All user-facing messages should support localization:

```json
{
  "en": "Operation successful.",
  "es": "Operación exitosa."
}
```

---

## Schema Definition

Complete schema definitions available in [`idp-schema.yaml`](./idp-schema.yaml), following OpenAPI v3.1 standards.

---

## Contribution Guidelines

Contributions and feedback are encouraged. Submit pull requests or issues via the [official repository](https://github.com/copyleftdev).
