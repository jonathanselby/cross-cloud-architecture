# Important non-equivalences

This catalogue records recurring translation traps.

It is intentionally more important than a service-equivalence table.

## 1. Account != subscription != project

They can all be workload/governance boundaries, but policy inheritance, identity context, billing and resource grouping differ.

**Test:** what source boundary is being preserved and why?

## 2. VPC != VNet != VPC network

AWS VPCs and Azure VNets are regional network constructs. Google Cloud VPC networks are global and contain regional subnets.

**Consequence:** hub topology, route design, address planning and cross-region patterns can change.

## 3. Security group != NSG != VPC firewall policy/rule

All control network traffic, but attachment, scope, evaluation and hierarchy differ.

**Test:** draw the exact packet path and mark every enforcement point.

## 4. IAM role != managed identity != service account

All can represent workload authority, but creation, attachment, federation and authorisation differ.

**Test:** how does the workload obtain short-lived credentials and what trust allows it?

## 5. Queue != event router != event stream

SQS, Service Bus, Pub/Sub, EventBridge, Event Grid, Eventarc, Kinesis and Event Hubs occupy overlapping but distinct roles.

**Test:** write down ordering, replay, retention, competing-consumer and delivery requirements before selecting a product.

## 6. Function != function

Function/serverless products differ in concurrency, runtime, event integration, networking, duration, scaling and deployment model.

**Test:** capture the complete execution contract.

## 7. Container != portable architecture

A container image can move while identity, ingress, storage, secrets, events and observability remain deeply provider-specific.

**Test:** list every dependency outside the image.

## 8. Managed Kubernetes != identical Kubernetes environment

Kubernetes provides a portable API surface, but cluster identity, CNI/networking, load balancers, storage classes, node lifecycle, policy, observability and upgrades remain provider-specific.

**Test:** separate Kubernetes API dependencies from cloud integrations.

## 9. Managed PostgreSQL != identical PostgreSQL

Engine compatibility does not guarantee extensions, permissions, failover, replication, maintenance control or performance equivalence.

**Test:** migrate the schema and operational procedures, not only the driver.

## 10. Object store != identical retention control

Object stores all hold objects, but immutable retention, legal hold, versioning, replication and event integration differ.

**Test:** translate the retention and recovery policy explicitly.

## 11. 'Multi-zone' != same failure behaviour

Providers and individual products automate placement and failover differently.

**Test:** identify what happens to traffic and state during a zone failure.

## 12. 'Global' != same global architecture

A global DNS service, a global network, a global edge proxy and a globally replicated database represent different meanings of global.

**Test:** identify the global control/data plane and what remains regional.

## 13. Load balancer != load balancer

Products can differ by L4/L7, global/regional, proxy/passthrough, external/internal, TLS/WAF/CDN integration and supported backends.

**Test:** decompose ingress requirements before choosing the target.

## 14. Private endpoint != complete private architecture

Private connectivity to a managed service does not automatically remove internet egress, DNS dependencies or public control-plane paths.

**Test:** draw DNS resolution and the full data/control path.

## 15. Customer-managed key != same custody model

The phrase can describe different key hierarchies, HSM boundaries and service dependencies.

**Test:** identify where key material exists, who can use it and when the managed service requires it.

## 16. OpenTelemetry != portable observability backend

OTel can standardise application telemetry. It does not make platform logs, query language, alerts or audit events equivalent.

**Test:** preserve operator questions and application telemetry, then redesign the provider backend.

## 17. AI platform != AI platform

Bedrock, Microsoft Foundry/Azure ML capabilities and Vertex AI package model access, training, retrieval, agents and evaluation differently.

**Test:** decompose the AI system into model, inference, retrieval, orchestration, evaluation and governance layers.

## 18. Same model name != same AI endpoint contract

Provider gateways can expose different versions, feature sets, quotas, safety controls or regional availability.

**Test:** pin the model/version and run the same acceptance suite.

## 19. Infrastructure as code != cross-cloud abstraction

Terraform/OpenTofu/Bicep/CloudFormation can automate deployment, but provider resources still expose provider semantics.

**Test:** distinguish reusable delivery practice from portable architecture.

## 20. Same RTO/RPO statement != same recovery mechanism

The target may meet the same objective through a different combination of replicas, backups, traffic routing and operator action.

**Test:** prove the target with failure and restore exercises.

## Classification

For each mapping use one of:

- **Equivalent enough**
- **Equivalent with conditions**
- **Different by design**
- **Not equivalent**

Avoid the word *equivalent* without the property and workload context that make it true.
