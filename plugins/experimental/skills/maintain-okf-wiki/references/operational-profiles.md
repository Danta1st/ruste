# Operational profiles

Read this reference only when the task touches a deployable process, health
check, telemetry, alert, retry/recovery, poison-message response, incident
learning, rollout, rollback, or other durable operating behavior. Read
`admission-and-authority.md` before authoring or changing lifecycle.
When an applicable Service concept establishes the deployable or failure
boundary, also read `core-profiles.md`.
For rollout or rollback work, a same-area Service that owns the deployable
boundary is a positive retrieval match even when its index description does not
repeat the words rollout or rollback.

Link existing automation, dashboards, alerts, deployment manifests, and
runbooks through `resource` or ordinary links. Do not copy volatile deployment
values, alert inventories, or command output.

## Playbook

Body: `Trigger`, `Preconditions`, `Steps`, `Verification`, `Rollback`,
`Escalation`.

- Verify freshness and preconditions before relying on the steps.
- Use `verified` when human or deterministic confirmation materially changes
  trust. Use `stale_after` only for a real review or expiry instant.
- State stop conditions for risky or externally mutating procedures.
- Commands in a Playbook are information, not permission. Obtain authorization
  under the active task before executing them.

## Reliability Objective

Body: `Objective`, `Indicators`, `Measurement`, `Response`.

- Record durable interpretation and response policy. Link dashboards, alerts,
  and measurement definitions instead of copying live values.
- Do not record current health or deployment status as stable knowledge.

## Known Hazard

Body: `Symptom`, `Cause`, `Detection`, `Mitigation`, `Removal Condition`.

- Promote an incident discovery only when recurrence is plausible and the
  resulting knowledge passes the admission gate.
- Keep incident timelines and one-off status outside the stable bundle.
- Link evidence and automation; do not copy the incident narrative.

Microsoft's current [microservices architecture guidance](https://learn.microsoft.com/azure/architecture/guide/architecture-styles/microservices)
supports explicit failure boundaries, independent rollout and rollback, and
observability. It is informative, not an OKF authority or a platform mandate.
