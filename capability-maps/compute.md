# Compute

Compute is not one category. Start by identifying the runtime properties the workload needs.

## Capability questions

- Is the process long-lived or request/event-driven?
- Does it need a full OS?
- Does it require privileged access, kernel control or attached devices?
- Is the workload stateless?
- Can it tolerate instance replacement?
- Does it need scale-to-zero?
- Is startup latency material?
- Does it require GPU/accelerator capacity?
- Is workload scheduling provider-managed or team-managed?
- Is the unit of deployment a VM, container image, function or source package?

## Common implementation families

| Runtime need | AWS | Azure | Google Cloud |
|---|---|---|---|
| General virtual machines | EC2 | Azure Virtual Machines | Compute Engine |
| Managed autoscaled VM fleet | Auto Scaling Groups | Virtual Machine Scale Sets | Managed Instance Groups |
| Event/request functions | Lambda | Azure Functions | Cloud Run functions / function-style Cloud Run deployment |
| Managed application runtime | App Runner / Elastic Beanstalk patterns | App Service | Cloud Run / App Engine |
| Batch | AWS Batch | Azure Batch | Google Cloud Batch |
| Kubernetes | EKS | AKS | GKE |

This is an index of candidate services, not an equivalence claim.

## VM translation

VMs are usually the easiest layer to translate semantically, but important differences remain:

- machine families and CPU architecture;
- local/ephemeral storage;
- boot-disk semantics;
- placement and availability constructs;
- autoscaling signals;
- image lifecycle;
- startup metadata;
- instance identity;
- load-balancer integration;
- spot/preemptible behaviour;
- host maintenance behaviour.

Do not map instance types by vCPU/RAM alone when storage throughput, network bandwidth or accelerator characteristics matter.

## Function translation

Function products can differ in:

- execution model;
- concurrency model;
- maximum duration;
- networking behaviour;
- deployment unit;
- cold-start characteristics;
- event integrations;
- filesystem behaviour;
- scaling controls;
- reserved/minimum capacity options.

The architectural requirement should say something such as:

> Execute short, stateless handlers in response to HTTP or events; tolerate replacement; no local durable state; scale automatically; private access to database required.

Then test target runtimes against that requirement.

## Managed runtime translation

A source container on ECS does not prove that the target needs Kubernetes or even containers.

If the real requirement is:

- stateless HTTP service;
- container image packaging;
- automatic horizontal scale;
- managed TLS ingress;
- no host administration;

then Azure Container Apps or Google Cloud Run may satisfy the target more simply than reproducing the source orchestrator.

Conversely, if the workload depends on:

- sidecars;
- daemon sets;
- custom networking;
- stateful sets;
- admission controls;
- Kubernetes APIs;

then Kubernetes itself may be a semantic dependency.

## Operational ownership test

For each candidate, record whether the team owns:

| Responsibility | VM | Managed container/runtime | Function |
|---|---|---|---|
| Guest OS patching | usually team | provider | provider |
| Runtime patching | team/shared | shared/provider-dependent | provider/shared |
| Capacity placement | team/service | provider/service | provider |
| Application health | team | team | team |
| Scaling policy | team | team + platform | mostly platform + configuration |
| Code/package lifecycle | team | team | team |

Exact responsibility varies by product; verify current documentation.

## Non-equivalence triggers

Record a delta if translation changes:

- OS control;
- process lifetime;
- concurrency;
- startup behaviour;
- local storage;
- network attachment;
- autoscaling granularity;
- deployment model;
- failure isolation;
- patch ownership.

## Rule

> Translate the execution contract, not the runtime brand.
