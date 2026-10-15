# Worked example: Bafflewick depot dispatch

## Scenario

**Bafflewick Consolidated Pty Ltd** operates twelve regional depots inherited through acquisitions, divestments and one merger that Legal still describes as historically complicated.

The depot-dispatch service assigns delivery work, stores manifests and sends dispatch events to finance, customer-notification and warehouse systems.

It currently runs on AWS. Bafflewick wants credible Azure and Google Cloud target architectures for a possible future platform consolidation.

The objective is **not** to produce a mechanically identical deployment.

The objective is to preserve the required system properties while using each target cloud coherently.

---

# 1. Requirements

## Business

- Dispatch must be available during depot operating hours.
- A lost dispatch command can create a missed delivery and is unacceptable.
- Duplicate delivery commands are tolerable only if the application detects them.
- Downstream event consumers may be temporarily unavailable without blocking dispatch.
- Australian data residency is required.

## Application

- Stateless HTTPS API.
- Container image is available but Kubernetes APIs are not used.
- PostgreSQL is the authoritative transactional store.
- Standard PostgreSQL drivers are used.
- PDF/CSV manifests are stored as objects.
- Workers process dispatch commands asynchronously.
- Domain events fan out to several downstream consumers.
- OpenTelemetry instrumentation is already present.

## Resilience

- Single runtime failure: automatic recovery.
- Single-zone failure: service should continue without manual rebuild.
- Region failure: restore service within four hours.
- Maximum accepted transactional data loss: fifteen minutes for regional disaster; materially lower for ordinary component failure.
- Point-in-time database restore required for logical corruption.

## Security

- Internet users access HTTPS only.
- Database must not be publicly reachable.
- Workloads should use short-lived native identities where possible.
- Application secrets must not be stored in deployment files.
- Customer-managed encryption keys are required for selected data stores.
- Administrative changes must be auditable.

## Operations

- Small platform team.
- No requirement to operate Kubernetes.
- Prefer managed runtime/database capabilities when they remove undifferentiated maintenance.
- Application team owns application code, SLOs, deployment and incident response.

---

# 2. Current AWS implementation

```text
                         Internet
                            |
                            v
                       Route 53
                            |
                            v
                 CloudFront + AWS WAF
                            |
                            v
                  Application Load Balancer
                            |
                            v
                 ECS service on Fargate
                    /       |        \
                   /        |         \
                  v         v          v
        RDS PostgreSQL     S3       SQS command queue
             Multi-AZ                   |
                                        v
                                  Fargate worker
                                        |
                                        v
                                   EventBridge
                                  /     |      \
                                 v      v       v
                              Finance Notify Warehouse

Workload IAM roles
Secrets Manager
AWS KMS
CloudWatch + OpenTelemetry
```

## Important observation

The product list is **not** the portable architecture.

The portable architecture is:

```text
public HTTPS ingress
        |
stateless managed runtime
        |
        +--> PostgreSQL-compatible authoritative store
        +--> durable object store
        +--> durable asynchronous command queue --> worker
                                                   |
                                                   v
                                            domain-event fan-out

plus:
workload identity
secret/key management
telemetry
backup/restore
regional recovery
```

---

# 3. Capability extraction

| Capability | Required property | Source implementation |
|---|---|---|
| DNS | public service name | Route 53 |
| Edge ingress | HTTPS, WAF, optional caching, health-aware routing | CloudFront + WAF + ALB |
| App runtime | stateless container, automatic scale/replacement | ECS/Fargate |
| Relational data | PostgreSQL, HA, PITR, private access | RDS PostgreSQL Multi-AZ |
| Objects | durable manifest storage | S3 |
| Command queue | durable, retryable, competing consumers, DLQ | SQS |
| Event distribution | fan-out domain events with filtering | EventBridge |
| Worker runtime | stateless async processor | ECS/Fargate |
| Workload identity | short-lived native credential | IAM task role |
| Secrets | managed application secrets | Secrets Manager |
| Keys | customer-managed encryption keys | KMS |
| Telemetry | metrics/logs/traces + OTel | CloudWatch + OTel |

---

# 4. Semantic constraints

These are more important than the AWS service names.

## Command queue

- at-least-once delivery is acceptable;
- duplicates are expected and consumers are idempotent;
- no global ordering requirement;
- commands must survive worker outage;
- dead-letter handling required.

## Domain events

- multiple independent consumers;
- one failed consumer must not block others;
- filtering by event type required;
- no transaction is assumed between downstream consumption and original dispatch command.

## Database

- PostgreSQL wire/driver compatibility;
- ordinary transactions;
- no proprietary Aurora extension dependency;
- point-in-time recovery;
- private application connectivity.

## Runtime

- OCI container image exists;
- no Kubernetes API dependency;
- no privileged mode;
- no persistent local disk;
- no daemon-set/sidecar requirement.

That last point matters: **container portability does not create a Kubernetes requirement.**

---

# 5. Azure target

One coherent Azure design is:

```text
                         Internet
                            |
                            v
                       Azure DNS
                            |
                            v
                  Azure Front Door + WAF
                            |
                            v
                    Azure Container Apps
                    /       |         \
                   /        |          \
                  v         v           v
 Azure Database for       Blob       Service Bus
 PostgreSQL Flexible      Storage       queue
 Server                              + dead-letter
                                        |
                                        v
                              Container Apps worker
                                        |
                                        v
                                   Event Grid
                                  /    |      \
                                 v     v       v
                              Finance Notify Warehouse

Managed identities
Key Vault
customer-managed keys where required
Azure Monitor + OpenTelemetry
```

## Why Container Apps rather than AKS?

The source uses containers, but the requirements do not include Kubernetes semantics.

Using AKS only because ECS is an orchestrator would add a cluster lifecycle and Kubernetes operating model that the workload does not require.

Container Apps is therefore a plausible **different-by-design** translation if current runtime limits, networking and availability characteristics satisfy the workload tests.

## Why Service Bus queue?

The command needs durable asynchronous work, retry and dead-letter handling.

That is a queue problem, not an event-grid problem.

## Why Event Grid separately?

The domain event needs fan-out and event routing to independent consumers.

The target deliberately preserves the distinction between **command** and **event**.

## Azure non-equivalences

| Source property | Azure target | Classification | Validation |
|---|---|---|---|
| ECS/Fargate task model | Container Apps | Different by design | concurrency, min replicas, networking, deployment and failure tests |
| SQS queue | Service Bus queue | Equivalent with conditions | duplicate, lock/settlement, DLQ and retry semantics |
| EventBridge routing | Event Grid | Equivalent with conditions | filtering, retry, dead-letter and event contract |
| IAM task role | managed identity | Equivalent enough for requirement | verify token path and RBAC scopes |
| RDS PostgreSQL | Azure Database for PostgreSQL | Equivalent with conditions | extensions, HA, failover, PITR, maintenance, connection limits |
| S3 | Blob Storage | Equivalent with conditions | retention/versioning/event differences if used |
| CloudWatch | Azure Monitor | Different by design | preserve OTel traces; rebuild queries/alerts/dashboards |

---

# 6. Google Cloud target

One coherent Google Cloud design is:

```text
                         Internet
                            |
                            v
                        Cloud DNS
                            |
                            v
 Global external Application Load Balancer
                   + Cloud Armor
                            |
                            v
                        Cloud Run
                    /       |         \
                   /        |          \
                  v         v           v
          Cloud SQL for   Cloud      Pub/Sub command
           PostgreSQL     Storage    topic/subscription
                                         |
                                         v
                                  Cloud Run worker
                                         |
                                         v
                              Pub/Sub domain events
                                  /    |       \
                                 v     v        v
                              Finance Notify Warehouse

Workload service identity / service account
Secret Manager
Cloud KMS
Cloud Monitoring/Logging + OpenTelemetry
```

## Why Cloud Run?

Again, the requirement is a stateless container runtime, not Kubernetes.

Cloud Run can be a simpler target if its concurrency, scaling, networking and execution characteristics fit the service.

## Why Pub/Sub appears twice?

The product may be the same family while the **architectural roles remain separate**.

One topic/subscription arrangement carries dispatch commands to competing processing logic. Another event topology distributes domain events to independent subscribers.

The design must preserve:

- separate contracts;
- separate retention/retry policies;
- separate DLQs/dead-letter topics where used;
- idempotency;
- consumer isolation.

If command execution needs stronger task-dispatch controls than the selected Pub/Sub design provides, Cloud Tasks becomes another candidate. The requirement decides.

## Google Cloud network delta

The source AWS VPC is regional. A Google Cloud VPC network is global with regional subnets.

That changes the network object model even if the application remains deployed in an Australian region.

The target must therefore record region placement at the resource/subnet/service level rather than assuming the VPC object itself creates the same regional boundary.

## Google Cloud non-equivalences

| Source property | Google target | Classification | Validation |
|---|---|---|---|
| AWS VPC regional boundary | global VPC + regional subnet/resources | Not equivalent as network construct | address, route, policy and region design |
| ECS/Fargate | Cloud Run | Different by design | concurrency, scaling, networking, failure behaviour |
| SQS | Pub/Sub or Cloud Tasks depending final semantics | Equivalent with conditions | command delivery and retry acceptance tests |
| EventBridge | Pub/Sub/Eventarc pattern | Equivalent with conditions | filtering, delivery and consumer isolation |
| IAM task role | service-account/workload identity path | Equivalent enough for requirement | least privilege + short-lived credential test |
| RDS PostgreSQL | Cloud SQL PostgreSQL | Equivalent with conditions | extensions, HA, PITR, failover, connection behaviour |
| S3 | Cloud Storage | Equivalent with conditions | retention/versioning/events if required |
| CloudWatch | Cloud Operations | Different by design | OTel continuity + rebuild alerts/queries |

---

# 7. What is intentionally not preserved

The translation does **not** preserve:

- the number of boxes;
- the ECS orchestration model;
- AWS account/VPC object semantics;
- CloudWatch query/dashboard implementation;
- AWS IAM object names;
- EventBridge/SQS product boundaries where another provider packages capabilities differently.

Those are source implementation details unless a requirement proves otherwise.

---

# 8. What must be preserved

The target must preserve or deliberately renegotiate:

- lost-command risk;
- idempotent command processing;
- independent event consumers;
- PostgreSQL application compatibility;
- authoritative-state integrity;
- private database connectivity;
- Australian data placement;
- workload identity without embedded static credentials;
- encryption/key requirements;
- zone-failure behaviour;
- backup/restore objectives;
- operator visibility across synchronous and asynchronous processing.

---

# 9. Validation plan

## Application

- deploy identical container build where practical;
- execute API acceptance suite;
- run PostgreSQL integration tests;
- verify manifest read/write behaviour.

## Queue

- deliver same command twice;
- kill worker after receive but before completion;
- poison a command;
- disable workers and build backlog;
- recover dead-letter command;
- verify idempotency.

## Events

- disable one consumer;
- confirm other consumers continue;
- validate filters;
- validate retry/dead-letter behaviour;
- test schema-version handling.

## Resilience

- terminate runtime instance/replica;
- simulate zone-level dependency loss where feasible;
- database failover exercise;
- point-in-time restore exercise;
- regional recovery tabletop plus executable recovery steps.

## Security

- verify no static cloud credential exists in application configuration;
- inspect workload identity token/role path;
- confirm database public access is disabled;
- test least-privilege denial cases;
- verify key and secret audit events.

## Observability

- trace one dispatch through API -> queue -> worker -> database/event publication;
- confirm trace context across async boundaries;
- reproduce SLO alerts;
- verify audit trail for infrastructure changes.

## Cost

Run representative traffic and measure target-specific drivers rather than translating the AWS bill line by line.

---

# 10. Decision record

This worked example does not declare Azure or Google Cloud 'better'.

It demonstrates a defensible translation process:

1. recover required properties;
2. separate command/event/runtime/data semantics;
3. allow target topology to change;
4. make non-equivalences explicit;
5. validate the system rather than trusting a mapping table.

The key result is not three similar diagrams.

It is three architectures that can be compared because they are all tied back to the **same explicit system contract**.
