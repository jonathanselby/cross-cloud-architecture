# Containers and serverless

Containers and serverless are packaging and operating models, not automatic portability layers.

## Start with the workload contract

Record whether the workload needs:

- a container image;
- Kubernetes APIs;
- long-running processes;
- background work;
- sidecars;
- custom network policy;
- service mesh;
- persistent volumes;
- GPU support;
- scale-to-zero;
- per-request isolation;
- scheduled jobs;
- event triggers;
- private ingress only;
- predictable minimum capacity.

## Common service families

| Need | AWS | Azure | Google Cloud |
|---|---|---|---|
| Managed Kubernetes | EKS | AKS | GKE |
| Managed container tasks/services | ECS with EC2 or Fargate | Container Apps / App Service containers | Cloud Run |
| Serverless container execution | Fargate-backed ECS patterns / App Runner | Container Apps | Cloud Run |
| Event-driven functions | Lambda | Functions | Cloud Run functions / function deployment model |
| Batch container jobs | AWS Batch / ECS tasks | Container Apps jobs / Azure Batch | Cloud Run jobs / Batch |

## Kubernetes is a dependency when the API matters

If the system relies on Kubernetes objects, operators, admission policies, CRDs, service-mesh integration or storage/network plugins, then Kubernetes itself may be part of the application contract.

If the source merely used Kubernetes as a convenient scheduler for a stateless web service, carrying the cluster into the target may preserve operating complexity rather than required capability.

## Serverless is not one operating model

Before translating a serverless workload, record:

- unit of scale;
- concurrency per instance;
- execution duration;
- warm/minimum capacity;
- ingress model;
- outbound networking;
- identity attachment;
- event delivery semantics;
- persistent local storage assumptions;
- background execution rules;
- CPU allocation outside request handling;
- deployment artifact.

## Portability layers

A container image can preserve:

- application binary/runtime;
- library dependencies;
- filesystem layout inside the image;
- process start command.

It does **not** preserve automatically:

- identity;
- ingress;
- DNS;
- persistent storage;
- service discovery;
- autoscaling;
- secrets;
- logging backend;
- event integration;
- load balancing;
- network security;
- database semantics.

## Decision rule

Use the simplest runtime that preserves required behaviour and accepted control.

A useful order is:

```text
Does the workload require full host control?
        |
        +-- yes -> VM / host-based runtime
        |
        no
        v
Does it require Kubernetes semantics?
        |
        +-- yes -> managed Kubernetes
        |
        no
        v
Does it fit a managed app/container runtime?
        |
        +-- yes -> managed container/app runtime
        |
        no
        v
Is it short/event/request-oriented code?
        |
        +-- yes -> function/runtime option
```

This is not a universal preference hierarchy. It is a way of forcing the requirement to justify added operating machinery.

## Rule

> Packaging portability is not architectural portability.
