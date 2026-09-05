# Phase 13 — Microservice Extraction

> **Status: planned only. No microservice extraction has been implemented.**
> WeaveLens remains a single Go backend organized as a modular monolith, plus a
> separately served Vue frontend and NATS dependency. This phase is preparation
> for a future architecture decision, not approved implementation work.

## Current Readiness

Useful extraction seams already exist:

* domain and application packages are separated from infrastructure;
* protobuf contracts provide versioned synchronous boundaries;
* NATS subjects provide an asynchronous integration boundary;
* discovery scanners, graph construction, and export are focused packages;
* the backend has health checks, graceful shutdown, and container packaging.

The system is not yet ready for extraction because:

* graph, scan history, and layout ownership still assume one application;
* end-to-end correlation, distributed tracing, and production metrics are not
  complete;
* event delivery, replay, idempotency, and schema compatibility guarantees need
  explicit service-level tests;
* there is no measured scaling, release-cadence, or ownership requirement that
  justifies the operational cost of multiple services;
* deployment currently packages one backend rather than independently versioned
  service images.

## Entry Criteria

Do not begin extraction until all of the following are true:

1. A measured bottleneck or independent ownership/deployment requirement exists.
2. The bounded context and its state ownership are documented.
3. gRPC/event contracts are versioned and have compatibility tests.
4. Requests and events carry correlation IDs and trace context.
5. Retry, idempotency, timeout, and dead-letter behavior are specified.
6. Local multi-process integration and failure tests pass before Kubernetes or
   cloud deployment work begins.

## Recommended Extraction Order

If the entry criteria are met, extract the stateless Export capability first,
then a Discovery worker if independent scan scaling is required. Keep graph and
history ownership in the modular monolith until a durable consistency model is
designed. Extract one boundary at a time and retain a rollback path.

## Objective

Evaluate and prepare for possible extraction of WeaveLens bounded contexts into
independently deployable services. Preparation may produce ADRs, versioned
contracts, event schemas, state-ownership decisions, failure analysis, and
deployment drafts; it must not create services until the entry criteria above
are approved.

This phase MUST NOT begin until the modular monolith is stable and the previous phases are working.

## Possible Target Architecture

```text
                    Web
                     │
                     ▼
                API Gateway
                     │
          ┌──────────┼──────────┐
          │          │          │
         gRPC       gRPC       gRPC
          │          │          │
          ▼          ▼          ▼
     Discovery     Graph      Export
      Service      Service    Service
          │          ▲
          │          │
          └── NATS ──┘
```

## Candidate Service Boundaries

Evaluate only when the entry criteria are met:

### API Gateway

Responsibilities:

* HTTP;
* authentication;
* authorization;
* request validation;
* API composition.

### Discovery Service

Responsibilities:

* AWS authentication;
* AWS resource scanners;
* scan orchestration;
* publishing discovery events.

### Graph Service

Responsibilities:

* consuming discovery events;
* building graph;
* maintaining temporary graph state;
* graph queries.

### Export Service

Responsibilities:

* converting canonical graph data into export formats.

## Communication

Use gRPC for synchronous calls.

Use NATS JetStream for asynchronous events.

Do not introduce direct database coupling between services.

## Service Boundaries

Each service must have:

* its own application layer;
* its own infrastructure layer;
* clear interfaces;
* independent configuration;
* health endpoints;
* graceful shutdown;
* tests.

Shared code must remain minimal.

Do not create a huge shared package that couples every service together.

## Deployment

Prepare for:

* Docker;
* independent containers;
* Kubernetes deployment;
* independent scaling.

Do not add Kubernetes-specific complexity until local service execution works.

## Distributed-System Requirements

Introduce:

* timeout;
* retry;
* idempotency;
* graceful degradation;
* health checks;
* correlation IDs;
* trace propagation.

## Testing

Add:

* service-level tests;
* gRPC integration tests;
* NATS integration tests;
* end-to-end scan flow.

## Migration Strategy

Do not rewrite the entire system.

Extract one bounded context at a time.

Verify behavior after each extraction.

## Possible Future Acceptance Criteria

The system works with:

```text
API Gateway
      ↓
Discovery Service
      ↓
NATS JetStream
      ↓
Graph Service
      ↓
API Gateway
      ↓
Web
```

and Export Service can operate independently.

## Change Control

Any extraction requires an explicitly approved implementation plan, contract and
failure-path tests, deployment rollback, and a focused review. Documentation or
preparation work alone must not be presented as a completed architecture
refactor.

This document describes a possible post-v1 evolution. No microservice extraction
is currently implemented or committed.
