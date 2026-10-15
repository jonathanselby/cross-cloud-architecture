# Method

## Purpose

This method translates an architecture from one cloud to another without assuming product equivalence.

The unit of translation is the **required system property**: behaviour, boundary, failure assumption, control or operating responsibility.

A service name may help identify the source implementation. It is not the requirement.

## The seven-pass method

### Pass 1 — Recover intent

Start with the system, not the bill of materials.

Record:

- users and external systems;
- business outcome;
- critical transactions;
- state and authoritative data;
- criticality;
- latency and throughput expectations;
- RTO and RPO;
- security and regulatory constraints;
- data residency;
- required interfaces and protocols;
- current operating model.

Ask of every source component:

> What property of the system is this component providing?

Do not accept the product name as the answer.

### Pass 2 — Extract capabilities

Rewrite product choices as capabilities.

Examples:

| Source implementation | Capability statement |
|---|---|
| EC2 autoscaling group | run long-lived compute instances and replace failed capacity |
| Lambda | execute code in response to requests/events without server management |
| S3 bucket | durable object storage with defined access, retention and replication behaviour |
| RDS PostgreSQL | PostgreSQL-compatible managed relational persistence |
| SQS queue | durable asynchronous command queue with explicit delivery semantics |
| EventBridge bus | route events between producers and consumers using event rules |
| CloudWatch | collect/query operational telemetry and trigger alerts |

The second column is portable. The first is not.

### Pass 3 — Record semantic constraints

Some behaviours are part of the application contract.

Identify them explicitly:

- SQL dialect and extensions;
- transaction boundaries;
- message ordering;
- delivery guarantee;
- deduplication behaviour;
- replay period;
- object versioning;
- consistency expectations;
- filesystem assumptions;
- source IP requirements;
- session affinity;
- TLS termination point;
- identity claims;
- token format;
- key ownership;
- API protocol;
- telemetry protocol.

If the application depends on a semantic property, a product with a similar category label is not equivalent unless it preserves that property or the application changes.

### Pass 4 — Record operating assumptions

For each capability, record who owns:

- provisioning;
- patching;
- runtime upgrades;
- scaling;
- health checks;
- replacement of failed capacity;
- backup;
- restore;
- replication;
- failover;
- key rotation;
- certificate rotation;
- telemetry collection;
- incident response;
- capacity planning;
- cost control.

A change in ownership is an architecture change.

### Pass 5 — Map candidate target services

Only now map products.

For each capability:

1. identify one or more plausible target services;
2. identify what each service manages;
3. compare semantic behaviour;
4. compare failure scope;
5. compare identity and network integration;
6. compare operational burden;
7. record provider-specific dependencies introduced.

Do not force one source box to one target box.

A single source component may become several target components. Several source components may collapse into one managed target service.

### Pass 6 — Record non-equivalences

Use four states:

- **Equivalent enough** — material required properties are preserved for this workload.
- **Equivalent with conditions** — acceptable only if stated assumptions hold.
- **Different by design** — target architecture deliberately changes implementation while preserving required outcome.
- **Not equivalent** — a required property cannot be preserved without application or operating-model change.

For every material delta, record:

| Field | Question |
|---|---|
| Property | What changed? |
| Cause | Why does the target behave differently? |
| Consequence | What does this change for the system? |
| Mitigation | Is a design or application change required? |
| Evidence | What proves the conclusion? |
| Validation | What still needs testing? |

### Pass 7 — Validate the whole architecture

A collection of locally sensible mappings can still produce a bad system.

Validate end to end:

#### Failure

- What happens on instance/container/process failure?
- What happens on zone failure?
- What happens on regional impairment?
- Which services are zonal, regional or global?
- Which failovers are automatic?
- Which require application logic or operator action?

#### Security

- Where are trust boundaries?
- How do workloads obtain identity?
- Are long-lived credentials introduced?
- Where are keys and secrets held?
- Where does TLS terminate?
- Which paths are public or private?

#### Data

- Is the same authoritative state preserved?
- Are consistency and transaction properties sufficient?
- Are backup and restore tested?
- Is export possible in a usable form?

#### Integration

- Are queue/event/stream semantics preserved?
- Is ordering required?
- Is replay required?
- Can duplicates occur?
- What happens during partial failure?

#### Operations

- Does the team have to operate more or less infrastructure?
- What new skills are required?
- What must be monitored?
- What must be patched or upgraded?
- Does the new service change incident diagnosis?

#### Cost

Do not compare list-price line items in isolation.

Model cost drivers created by the architecture:

- steady capacity;
- burst capacity;
- requests/executions;
- storage volume;
- operations on stored data;
- data movement;
- observability ingestion/retention;
- HA/replicas;
- backup;
- support and operational labour.

#### Exit

- Which interfaces remain portable?
- Which managed semantics have become application dependencies?
- Can data be exported?
- Can configuration be recreated?
- What would have to change to leave the target service?

## Translation artefact

A complete translation should contain:

```text
1. system context
2. required capabilities
3. semantic constraints
4. operating assumptions
5. source architecture
6. candidate target architecture
7. mapping rationale
8. non-equivalences
9. failure model
10. security model
11. operational deltas
12. cost drivers
13. exit consequences
14. unknowns
15. validation plan
```

## Depth rule

Do not document every possible cloud feature.

Go deep where a difference can change:

- correctness;
- security;
- resilience;
- operability;
- cost;
- portability;
- delivery effort.

Stop when additional provider detail no longer changes the architectural decision.

## Anti-patterns

### Product-name substitution

`S3 -> Blob Storage -> Cloud Storage` is vocabulary, not an architecture translation.

### Diagram tracing

Recreating the same boxes and arrows in another provider usually preserves source-cloud assumptions that may not belong in the target.

### Lowest-common-denominator design

Avoiding every managed service to keep all clouds identical can increase cost and operating burden without creating useful portability.

### Certification-table architecture

Service categories are useful indexes. They do not establish behavioural equivalence.

### Ignoring management scope

A managed service that takes over patching, failover or scaling changes the operating model even if application interfaces remain similar.

### Portability theatre

Containers, Kubernetes or Terraform can improve portability of some layers. They do not automatically make identity, networking, data, observability or managed-service semantics portable.

## Final test

A reviewer should be able to answer:

> If the provider names were removed from this document, would the required architecture still make sense?

If not, the translation probably started too low in the stack.
