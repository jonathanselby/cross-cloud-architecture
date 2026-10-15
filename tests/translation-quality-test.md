# Translation quality test

Use this before treating a cross-cloud architecture as complete.

A failed item should remain visible. Do not average it away with a score.

## A. Problem and scope

- [ ] The business/system outcome is stated independently of the source provider.
- [ ] Critical transactions and authoritative state are identified.
- [ ] Hard constraints are separated from preferences.
- [ ] RTO/RPO and location requirements are explicit.

## B. Architecture recovery

- [ ] Every important source product has an architectural role, not just a product name.
- [ ] Required semantics are recorded for data, messaging and runtime.
- [ ] Source operating responsibilities are understood.
- [ ] Source failure assumptions are documented.

## C. Target design

- [ ] Target services were selected after capability extraction.
- [ ] The target is allowed to use a different number of components.
- [ ] The design does not introduce Kubernetes, VMs or other machinery without a requirement.
- [ ] Provider-native services are used deliberately rather than avoided reflexively.

## D. Non-equivalence

- [ ] Resource-hierarchy differences are considered.
- [ ] Identity/federation differences are considered.
- [ ] Network scope/routing/security differences are considered.
- [ ] Runtime/concurrency/scaling differences are considered.
- [ ] Data consistency/replication/failover differences are considered.
- [ ] Queue/event/stream semantics are considered separately.
- [ ] Observability backend differences are considered.
- [ ] Secret/key custody differences are considered.
- [ ] AI/model differences are considered where relevant.
- [ ] Every material delta is classified and has a validation action.

## E. Failure and recovery

- [ ] Runtime failure behaviour is understood.
- [ ] Zone failure behaviour is understood.
- [ ] Regional failure behaviour is understood.
- [ ] Backup and restore are separate from HA.
- [ ] Logical corruption recovery exists where required.
- [ ] Failover assumptions are testable.

## F. Security

- [ ] Human and workload identities are separated.
- [ ] Static credentials are not introduced without explicit justification.
- [ ] Least-privilege scope is understood.
- [ ] Public and private network paths are drawn.
- [ ] TLS termination points are known.
- [ ] Key custody and secret rotation are understood.
- [ ] Administrative activity is auditable.

## G. Integration

- [ ] Command queues, event distribution and streams are not conflated.
- [ ] Ordering requirement is explicit.
- [ ] Duplicate behaviour is explicit.
- [ ] Retry and dead-letter behaviour is explicit.
- [ ] Replay requirement is explicit.
- [ ] Idempotency strategy exists where delivery can repeat.

## H. Operations

- [ ] Patching ownership is explicit.
- [ ] Runtime/cluster upgrade ownership is explicit.
- [ ] Scaling ownership is explicit.
- [ ] Backup/restore ownership is explicit.
- [ ] On-call and incident ownership are explicit.
- [ ] The target team's skills match the operating model or a gap is recorded.

## I. Observability

- [ ] Operator questions are preserved.
- [ ] Metrics/logs/traces required by SLOs are available.
- [ ] Audit telemetry is translated separately from application telemetry.
- [ ] OpenTelemetry or another portable instrumentation boundary is retained where useful.
- [ ] Alerts are revalidated rather than copied mechanically.

## J. Cost

- [ ] Cost is derived from target architecture drivers, not source invoice categories.
- [ ] Data transfer is considered.
- [ ] HA/replica cost is considered.
- [ ] Observability ingestion/retention is considered.
- [ ] Backup and support are considered.
- [ ] Operational labour changes are visible.

## K. Exit

- [ ] Provider-specific dependencies introduced by the target are listed.
- [ ] Data export/recovery path is understood.
- [ ] Rebuild or application-change effort to leave is visible.
- [ ] Portability is not claimed merely because containers or IaC are used.

## L. Evidence

- [ ] Time-sensitive service facts come from current first-party documentation.
- [ ] Region/feature availability is verified for the intended deployment.
- [ ] Quotas and limits that can kill the design are verified.
- [ ] Unknowns are visible rather than inferred.
- [ ] The architecture has an executable validation plan.

## Decision gate

A translation is not ready when any unresolved item can materially change:

- correctness;
- security;
- resilience;
- operability;
- cost;
- compliance;
- migration feasibility.

The objective is not checklist completion. It is a design whose important assumptions can be defended.
