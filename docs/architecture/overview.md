# Architecture Overview

WeaveLens is a modular monolith. One Go backend process wires application,
discovery, graph, export, HTTP, and event components; the Vue frontend is served
separately. Microservices are a possible post-v1 evolution, not the current
runtime architecture.

## Core Components

- **Discovery**: AWS resource discovery layer
- **Graph**: Infrastructure relationship graph construction
- **API**: Active HTTP API; protobuf contracts and in-process gRPC-style adapters
  are preparatory and do not run a gRPC server
- **Events**: NATS JetStream carries scan lifecycle events; graph construction
  currently invokes discovery directly
- **Web**: Vue application consuming the HTTP API; GraphQL is not implemented
- **Export**: JSON, Draw.io, and SVG graph export
- **Persistence**: Capped scan/graph history and canvas layouts stored in local
  JSON files

## Technology Stack

- Language: Go
- Architecture: Modular monolith
- Communication: HTTP and NATS JetStream; protobuf reserved for future gRPC
- Visualization: Vue and Cytoscape.js

## Constraints

- No external dependencies unless necessary
- Clear separation between internal packages
- No business logic in main package
- Preserve modular boundaries before considering process extraction
- Local JSON state is not suitable for horizontally scaled services
