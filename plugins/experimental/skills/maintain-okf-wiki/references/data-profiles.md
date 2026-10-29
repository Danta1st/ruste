# Data profiles

Read this reference only when the task touches a persistent store, migration,
schema ownership, transaction boundary, consistency rule, or retention policy.
Read `admission-and-authority.md` before authoring or changing lifecycle.
Do not read `core-profiles.md` merely because a service owns the data; require an
applicable Decision, Invariant, Domain Concept, System Boundary, or Service.

## Data Ownership

Body: `Owned Data`, `Access Rules`, `Transactional Boundary`, `Consistency`,
`Migrations`, `Retention`.

- Record durable ownership, cross-service access constraints, transaction
  boundaries, consistency semantics, migration responsibility, and retention
  rules.
- Link `resource` to schemas, migrations, models, or infrastructure definitions.
  Those artifacts remain authoritative for implemented shape; do not list their
  tables, columns, or configuration values.
- For a scoped migration task, inspect the named migration and directly affected
  schema resource. If the area index identifies an affected API or Event
  Contract, read `interface-profiles.md` and retrieve that contract.
- A migration alone does not activate rollout or deployment Playbooks; require
  rollout, deployment, or operational evidence in the active task.
- Do not treat persistence entities as universal domain concepts or integration
  contracts.
- Do not document every table or column. A non-obvious data product, governed
  metric, or sanctioned computation needs a separate concept only when it has
  an independent lifecycle and passes the admission gate.
- Defer the full OKF `Attested Computation` profile until a real governed
  calculation requires it.

Microsoft's current [data considerations for microservices](https://learn.microsoft.com/azure/architecture/microservices/design/data-considerations)
support explicit service data ownership and consistency choices. This source is
informative and does not make the profile specific to Azure or microservices.
