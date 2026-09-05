# Current WeaveLens Phases

These are the latest phase documents for the current WeaveLens baseline. The
source code and automated tests remain authoritative when a phase brief differs
from implemented behavior.

Historical, unannotated phase briefs are archived under [`v0/`](v0/).

## Status

| Phase | Area | Current status |
| --- | --- | --- |
| [00](phase-00-foundation.md) | Foundation | Implemented |
| [01](phase-01-architecture.md) | Architecture and domain model | Mostly implemented; JSON persistence was added |
| [02](phase-02-go-skeleton.md) | Go application skeleton | Implemented |
| [03](phase-03-aws-authentication.md) | AWS authentication | Mostly implemented; classification can improve |
| [04](phase-04-aws-client.md) | AWS client layer | Implemented and expanded |
| [05](phase-05-aws-discovery.md) | AWS discovery | Implemented and expanded; hardening gaps remain |
| [06](phase-06-graph.md) | Graph engine | Implemented |
| [07](phase-07-grpc.md) | gRPC contracts | Preparatory only; no gRPC runtime |
| [08](phase-08-nats.md) | NATS JetStream | Partially implemented; lifecycle events only |
| [09](phase-09-web-cloud-connection-ux.md) | Web and connection UX | Substantially implemented; contract/acceptance gaps remain |
| [09a](phase-09a-responsive-ui-fix.md) | UI stabilization | Historical corrective phase; manual verification remains |
| [10](phase-10-resilience.md) | Concurrency and resilience | Helpers exist but are not integrated into discovery |
| [11](phase-11-export.md) | Graph export | Partially implemented; PNG remains planned |
| [12](phase-12-observability-security.md) | Observability and security | Partially implemented; continue hardening |
| [13](phase-13-microservices.md) | Microservice extraction | Planned only; not implemented |

## Maintenance Rules

- Update the current-state note when implementation behavior materially changes.
- Keep the latest phase documents directly in this directory.
- Archive superseded snapshots under a version directory such as `v1/`.
- Keep Phase 13 preparatory until its entry criteria are explicitly approved.
- Verify implementation and tests instead of inferring completion from a phase.
