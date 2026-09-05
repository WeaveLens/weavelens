# WeaveLens Orchestration

This directory records the implementation phases and repeatable engineering
workflows for WeaveLens. The source code and automated tests remain the source
of truth when a historical phase document differs from the current system.

## Versions

- The latest phase documents are in [`phases/`](phases/README.md).
- [Historical phases](phases/v0/) preserve the original unannotated phase briefs
  before status updates.
- When the current baseline is superseded, archive it in the next `phases/vN/`
  directory and keep the latest documents directly under `phases/`.

## Current Architecture

WeaveLens is currently a Go modular monolith with a Vue frontend. It includes
AWS authentication and discovery, graph construction, an HTTP API, preparatory
protobuf/in-process gRPC adapters, NATS JetStream lifecycle events, persisted
scan history/layout, graph export, resilience helpers, and transport security
controls.

Microservice extraction is not implemented. Phase 13 is a preparation and
decision document only; the current package boundaries should be preserved
until measurable scaling, deployment, or ownership needs justify extraction.

## Validation Baseline

Use the repository commands rather than phase-specific historical commands:

```bash
gofmt -w <changed-go-files>
go build ./...
go test ./internal/...
go vet ./...

cd web
npm run lint
npm test
npm run build
```

Docker build, Compose, LocalStack, credentials, and Docker Hub instructions are
maintained in [`DOCKER.md`](../DOCKER.md).

## Related Documentation

- [Architecture overview](../docs/architecture/overview.md)
- [AWS credential strategy ADR](../docs/adr/001-aws-credential-strategy.md)
- [Frontend development](../web/README.md)
- [Current implementation map](master.md)
