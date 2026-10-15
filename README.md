# Cross-Cloud Architecture

**Translate architectures across AWS, Azure and Google Cloud without pretending similarly named services are identical.**

Cloud translation is often reduced to a three-column product table.

That is useful for vocabulary. It is weak architecture.

An architecture is a set of required behaviours, boundaries, failure assumptions, operating responsibilities and trade-offs. Product names are only one implementation of those choices. Moving an application from one cloud to another therefore should not start with *"what is the Azure equivalent of this AWS service?"* It should start with *"what property of the system was this component providing, and does the target architecture still need that property?"*

The method in this repository is simple:

> **Translate the architecture, not the product catalogue.**

Map required capabilities and operating assumptions first. Map provider products second.

## Why

Superficially similar services often differ in scope, control plane, failure domain, identity model, networking behaviour, consistency, delivery semantics, scaling model and operational burden.

Some examples:

- an AWS account, Azure subscription and Google Cloud project can all act as workload or governance boundaries, but they sit differently in each provider's hierarchy;
- an AWS VPC and Azure virtual network are regional constructs, while a Google Cloud VPC network is global and contains regional subnets;
- a queue, an event router and an event stream solve different problems even when vendor diagrams place them beside one another;
- a managed relational database may preserve an engine API while changing replication, scaling, failover and operational control;
- OpenTelemetry can make instrumentation portable while the storage, query, alerting and retention systems remain provider-specific;
- foundation-model access, custom model training and managed agent runtimes do not line up as one product category across the three clouds.

The differences matter because architecture lives in the differences.

## What this repository produces

A cross-cloud translation should produce five things:

1. **Capability model** — what the workload actually requires, independent of provider products.
2. **Operating assumptions** — who owns scaling, patching, failover, identity, networking, observability and recovery.
3. **Candidate target architecture** — an architecture that uses the target cloud coherently rather than mechanically reproducing the source topology.
4. **Non-equivalence record** — where the translation changes system behaviour, risk or operating burden.
5. **Validation questions** — what still needs to be tested or confirmed before the translation is treated as real.

## Translation flow

```text
SOURCE ARCHITECTURE
        |
        v
+---------------------------+
| Recover architectural     |
| intent and system         |
| properties                |
+---------------------------+
        |
        v
+---------------------------+
| Extract required          |
| capabilities              |
+---------------------------+
        |
        v
+---------------------------+
| Record operating and      |
| failure assumptions       |
+---------------------------+
        |
        v
+---------------------------+
| Identify semantic         |
| constraints               |
+---------------------------+
        |
        v
+---------------------------+
| Map target-cloud          |
| services                  |
+---------------------------+
        |
        v
+---------------------------+
| Test every important      |
| non-equivalence           |
+---------------------------+
        |
        v
TARGET ARCHITECTURE
        |
        v
VALIDATE FAILURE / SECURITY /
OPERATIONS / COST / EXIT
```

See [`METHOD.md`](METHOD.md) and [`TRANSLATION-FLOW.txt`](TRANSLATION-FLOW.txt).

## Architecture before products

The translation starts with questions such as:

- What is the workload trying to do?
- What is its criticality?
- Which state must survive which failures?
- What RTO and RPO are genuinely required?
- Which components need ordering, replay, transactions or deduplication?
- Which interfaces must remain compatible?
- Where must data reside?
- Which identities are human, workload or external-federated identities?
- Which network paths must be private?
- What must scale independently?
- Which responsibilities is the team deliberately handing to the provider?
- Which provider-specific dependencies are acceptable?

Only after those questions are answered should product mapping begin.

## Repository map

### Foundations

Cloud services sit inside a provider operating model. These files establish the boundaries that affect everything above them.

- [`foundations/resource-hierarchy.md`](foundations/resource-hierarchy.md) — organisation, account/subscription/project and policy inheritance.
- [`foundations/regions-zones-resilience.md`](foundations/regions-zones-resilience.md) — location, failure domains and regional design.
- [`foundations/identity-access.md`](foundations/identity-access.md) — workforce, workload and resource identity.
- [`foundations/networking.md`](foundations/networking.md) — network scope, routing, security and hub connectivity.

### Capability maps

These are not equivalence tables. Each starts with the required behaviour and then maps plausible implementations.

- [`capability-maps/compute.md`](capability-maps/compute.md)
- [`capability-maps/containers-serverless.md`](capability-maps/containers-serverless.md)
- [`capability-maps/edge-ingress.md`](capability-maps/edge-ingress.md)
- [`capability-maps/object-storage.md`](capability-maps/object-storage.md)
- [`capability-maps/relational-data.md`](capability-maps/relational-data.md)
- [`capability-maps/messaging-events-streaming.md`](capability-maps/messaging-events-streaming.md)
- [`capability-maps/observability.md`](capability-maps/observability.md)
- [`capability-maps/secrets-keys.md`](capability-maps/secrets-keys.md)
- [`capability-maps/ai-model-services.md`](capability-maps/ai-model-services.md)

### Non-equivalences

[`non-equivalences/README.md`](non-equivalences/README.md) collects the cases most likely to produce a bad architecture when treated as one-for-one substitutions.

### Reference patterns

The patterns translate whole architectural intents rather than individual products:

- [`patterns/three-tier-service.md`](patterns/three-tier-service.md)
- [`patterns/event-driven-integration.md`](patterns/event-driven-integration.md)
- [`patterns/ai-rag-service.md`](patterns/ai-rag-service.md)

### Worked example

[`examples/bafflewick-consolidated/`](examples/bafflewick-consolidated/) translates a fictional depot-dispatch workload from AWS into coherent Azure and Google Cloud architectures.

The example deliberately allows the topology to change where the target cloud makes a different design more natural.

### Reusable artefacts

- [`templates/capability-requirements.md`](templates/capability-requirements.md) — extract requirements without embedding provider choices.
- [`templates/architecture-translation.md`](templates/architecture-translation.md) — record a translation and its deltas.
- [`tests/translation-quality-test.md`](tests/translation-quality-test.md) — test whether a translation is architecturally defensible.
- [`SKILL.md`](SKILL.md) — instructions for using the method with an AI assistant or architecture workflow.

## The main non-equivalence classes

A cross-cloud review should actively look for differences in at least these classes:

| Class | What can change |
|---|---|
| Resource hierarchy | policy inheritance, billing boundary, delegated administration |
| Identity | workforce federation, workload identity, resource identity, credential lifecycle |
| Network scope | regional/global boundaries, routing model, firewall attachment, private service access |
| Failure domain | zonal/regional/global service behaviour, failover responsibility |
| Runtime | process/container/function/VM assumptions, concurrency, scale-to-zero, execution limits |
| Data | consistency, replication, engine compatibility, failover, export and recovery |
| Messaging | queue/event/stream semantics, ordering, deduplication, transactions, replay |
| Observability | telemetry protocol, backend schema, query language, alerting and retention |
| Key custody | secrets, encryption keys, HSM boundaries and rotation ownership |
| AI platform | model access, training, inference, agent runtime, retrieval, evaluations and governance |
| Edge | L4/L7 scope, TLS termination, WAF/CDN integration, global versus regional ingress |
| Operations | patching, upgrades, on-call responsibility, scaling and incident recovery |

A product substitution that changes one of these may still be the right design. It simply is not an equivalent design.

## Three translation rules

### Preserve required properties, not boxes

If the source architecture has twelve boxes and the target architecture needs eight, use eight.

The purpose of translation is to preserve required system behaviour, not the visual shape of the source diagram.

### Re-evaluate every managed boundary

Managed services move responsibility. They do not eliminate it.

When the target product manages more or less than the source product, record what changes in patching, scaling, failover, backup, observability, security, support and recovery.

### Make unknowns visible

Cloud services change continuously. Quotas, region availability, pricing, service tiers and preview/GA status are time-sensitive facts.

Do not fill gaps from memory. Mark the unknown and verify it against current first-party documentation.

## Worked example at a glance

Bafflewick Consolidated operates a depot-dispatch service used by regional logistics teams. Its source implementation happens to run on AWS.

The workload requires:

- public HTTPS ingress;
- private application-to-database connectivity;
- stateless application compute;
- PostgreSQL compatibility;
- durable object storage for manifests;
- asynchronous dispatch commands that must not be silently lost;
- domain events for downstream consumers;
- managed secrets and encryption keys;
- OpenTelemetry instrumentation;
- Australian hosting;
- no requirement for Kubernetes.

The naive translation is a product substitution exercise.

The useful translation first separates **command queueing** from **event distribution**, preserves PostgreSQL as an interface requirement rather than a vendor product, keeps OpenTelemetry at the instrumentation boundary, and chooses target runtimes based on operational requirements rather than the fact that the source used containers.

See [`examples/bafflewick-consolidated/README.md`](examples/bafflewick-consolidated/README.md).

## Current-service note

Cloud product names and boundaries change. This repository therefore distinguishes durable architectural reasoning from volatile product facts.

Provider-specific statements are checked against first-party documentation and dated in [`SOURCES.md`](SOURCES.md). They should be rechecked before a production decision.

This is especially important for newer or fast-moving areas such as serverless runtimes, AI platforms, managed agent services and regional feature availability.

## Design choices

### AWS, Azure and Google Cloud only

The first edition goes deep enough to be useful across the three major hyperscalers rather than shallowly covering every provider.

### No universal weighted score

Architecture choices are not made safer by converting unlike risks into one synthetic number. Fatal constraints remain fatal. Unknowns remain unknown.

### Native services are allowed

Portability does not mean reducing every cloud to virtual machines and PostgreSQL.

Use provider-native capabilities when they create enough value. Record the dependency and exit consequence explicitly.

### No lowest-common-denominator architecture

The method is intended to help a workload move across clouds, not force all clouds to look the same.

### Operating consequences are architecture

Who patches, scales, restores, rotates, upgrades and responds to failure is part of the design.

## Limitations

This repository is an architecture reference, not a substitute for current provider documentation, security review, workload testing, cost modelling or region-specific service validation.

It deliberately avoids hard-coded pricing and quota tables because those change quickly.

The mappings describe common architectural roles, not contractual equivalence or guaranteed feature parity.

The worked examples are fictional and use synthetic requirements.

## Version

Current version: **0.1.0**

See [`CHANGELOG.md`](CHANGELOG.md).

## Sources

Provider-specific material is sourced from AWS documentation, Microsoft Learn and Google Cloud documentation.

See [`SOURCES.md`](SOURCES.md).

## Licence

MIT. See [`LICENSE`](LICENSE).
