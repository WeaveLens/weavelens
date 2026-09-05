# WeaveLens Implementation Map

This file is the concise current-state companion to the latest phase documents
in [`phases/`](phases/README.md).

## Implemented Baseline

- Go modular monolith under `cmd/` and `internal/`.
- AWS profile/default/AssumeRole authentication and identity verification.
- Registry-based AWS discovery and relationship construction across 22 scanners.
- In-memory infrastructure graph with persisted scan history and layouts.
- HTTP transport, preparatory protobuf/in-process gRPC adapters, and NATS
  JetStream scan lifecycle events.
- Vue graph UI with multi-region scanning and composable canvas filters.
- JSON, Draw.io, and SVG graph export.
- Docker images and Compose-based local execution with optional LocalStack.

## Remaining Hardening

- Complete production telemetry and metrics export.
- Decide whether circuit breaking is justified around AWS operations.
- Add PNG export only if a concrete consumer requires it.
- Continue integration, race, security, and failure-path testing.
- Keep runtime scan/layout JSON out of Git and document migration when storage
  ownership changes.

## Deferred Architecture

Microservices are not part of the current implementation. The preparation notes
and entry criteria are in
[`phase-13-microservices.md`](phases/phase-13-microservices.md). Do not split
the backend solely to match that target diagram; require evidence that an
independently owned or scaled boundary offsets the distributed-system cost.

## Required Validation

```bash
go build ./...
go test ./internal/...
go vet ./...

cd web
npm run lint
npm test
npm run build
```
