# Interface profiles

Read this reference only when the task touches an HTTP, gRPC, GraphQL, RPC,
public SDK, message, or event boundary. Select `core-profiles.md` independently
only when a Domain Concept, Service, System Boundary, Decision, or Invariant is
applicable; an interface's host or producer alone is not enough. Read
`admission-and-authority.md` before authoring or changing lifecycle.

Executable schemas own machine-readable shape. Concepts record durable meaning
and compatibility rules the schema cannot express. Use `resource` for the
authoritative OpenAPI, protobuf, GraphQL, AsyncAPI, JSON Schema, Avro, or
canonical message artifact; do not reproduce endpoints, payload fields, or the
schema in prose.

## API Contract

Body: `Purpose`, `Consumers`, `Semantics`, `Compatibility`, `Security`,
`Idempotency`, `Failure Behavior`.

- Distinguish public or client-facing APIs from internal service-to-service
  interfaces when their compatibility policies differ.
- Record business semantics, compatibility commitments, authorization meaning,
  idempotency, and failure behavior only when durable and non-obvious.
- For a semantic interface change, retrieve and hand off linked Domain Concepts
  that define the affected business entity or operation. A same-area Domain
  Concept whose title or description names that entity or operation is a
  positive retrieval match.
- For a synchronous API change, retrieve the applicable Service concept that
  owns the boundary when the area index identifies one, and read
  `core-profiles.md` for that concept.
- Do not create one concept per endpoint by default. Split only for independent
  lifecycle or costly semantics.

## Event Contract

Body: `Meaning`, `Producer`, `Consumers`, `Delivery Semantics`,
`Ordering and Idempotency`, `Evolution`.

- Capture business meaning, producer ownership, delivery assumptions,
  retry/idempotency behavior, ordering, and evolution only when durable and
  non-obvious.
- A consumer retry or idempotency task does not activate the producer's Service
  concept unless producer responsibility or failure boundaries are also in
  scope.
- Do not use database entities as event semantics or copy message schemas.
- Do not create one concept per event by default. Split only for independent
  lifecycle or costly semantics.

Microsoft's current microservice guidance supports distinguishing public from
internal APIs, defining semantics and versioning, and publishing event schemas;
it is informative, not an OKF authority:

- [API design](https://learn.microsoft.com/azure/architecture/microservices/design/api-design)
- [Microservices architecture style](https://learn.microsoft.com/azure/architecture/guide/architecture-styles/microservices)
- [Data considerations for microservices](https://learn.microsoft.com/azure/architecture/microservices/design/data-considerations)
