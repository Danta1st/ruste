# Core profiles

Read this reference for library or package boundaries, domain-model work, or
core concepts shared by multiple repository shapes. Read
`admission-and-authority.md` before authoring or changing lifecycle.

Use the profile matching the knowledge's meaning, not the kind of source file
being changed. Every new concept uses the shared envelope and local rules in
`okf-v0.2.md`.

## Decision

Answers why an approved direction was chosen.

Body: `Context`, `Decision`, `Consequences`.

- Require explicit approval. Keep unresolved alternatives outside the stable
  bundle as proposals.
- Replace changed operative meaning; never rewrite it in place.

## Invariant

States what must remain true.

Body: `Rule`, `Scope`, `Rationale`, `Enforcement`.

- Record durable constraints whose enforcement or rationale is not obvious
  from nearby code and tests.
- Replace changed operative meaning and preserve the deprecated record.

## Domain Concept

Defines a term within one bounded context.

Body: `Definition`, `Boundaries`, `Examples`, `Related Concepts`.

- Scope terminology to its bounded context. Different contexts may use the same
  term differently.
- Do not turn database entities or wire types into universal domain concepts.

## System Boundary

Describes responsibility and ownership for a component boundary.

Body: `Responsibility`, `Owns`, `Does Not Own`, `Dependencies`.

- Verify descriptive ownership against current public interfaces and
  representative implementation evidence.
- Do not inventory every project or dependency.

## Service

Describes a deployable business capability and its failure boundary.

Body: `Responsibility`, `Interfaces`, `Owned Data`, `Dependencies`,
`Failure Boundary`, `Operations`.

- Create one only for a deployable boundary, not every project, package,
  process class, endpoint, or job.
- Link interface, data, and operational concepts rather than repeating them.
- Verify the deployable boundary against manifests or equivalent artifacts.

## Concept granularity

Keep related knowledge in one readable concept until a fact is reused, needs an
independent lifecycle, or needs separate verification. Passing the admission
gate does not require splitting a record by heading or type.
