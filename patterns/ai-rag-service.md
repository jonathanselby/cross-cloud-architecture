# Pattern: retrieval-augmented AI service

## Intent

Answer user questions using a foundation model grounded in an authorised document corpus, with traceable retrieval and explicit evaluation.

## Capability model

```text
user
 |
 v
[application/API]
 |
 +------------------------------+
 |                              |
 v                              v
[retrieval]                 [model gateway]
 |                              |
 v                              v
[authorised corpus]         [foundation model]
 |                              |
 +--------------+---------------+
                v
        [structured response]
                |
                v
          [evaluation/log]
```

Supporting capabilities:

- workload identity;
- secret/key management;
- document ingestion;
- embeddings;
- vector/index retrieval;
- metadata and access filters;
- model invocation;
- prompt/version control;
- safety controls;
- trace/provenance;
- evaluation;
- latency and cost telemetry.

## AWS implementation family

Potential components include:

- Bedrock for model access and selected managed AI capabilities;
- Bedrock Knowledge Bases or an independently built retrieval pipeline;
- OpenSearch / database/vector options where required;
- Lambda/ECS/EKS/application runtime;
- S3 corpus storage;
- IAM workload roles;
- CloudWatch/OTel.

## Azure implementation family

Potential components include:

- Microsoft Foundry model capabilities;
- Azure AI Search or independently built retrieval;
- App Service/Functions/Container Apps/AKS application runtime;
- Blob Storage corpus;
- managed identities;
- Azure Monitor/OTel.

## Google Cloud implementation family

Potential components include:

- Vertex AI model services / Model Garden;
- Vertex AI retrieval/search capabilities or independently built retrieval;
- Cloud Run/GKE application runtime;
- Cloud Storage corpus;
- workload identity/service accounts;
- Cloud Operations/OTel.

## What should remain application-owned

Where practical, keep these definitions portable:

- task contract;
- input/output schema;
- acceptance dataset;
- evaluation criteria;
- prompt/instruction versions;
- retrieved-source identifiers;
- authoritative response policy;
- decision about when human review is required.

Provider tooling can execute or accelerate these controls. It should not silently become the only place they exist.

## Non-equivalences to test

- same model/version availability;
- model API schema;
- structured output/tool-calling behaviour;
- safety-filter behaviour;
- region/data-processing boundary;
- embedding model;
- chunking and retrieval logic;
- vector filter semantics;
- access-control propagation;
- citation/provenance model;
- evaluation API and metrics;
- agent/runtime state if used;
- trace format;
- provisioned/on-demand capacity behaviour.

## Portability boundary

A practical boundary is often:

```text
portable/application-owned
--------------------------
request schema
prompt/instruction version
retrieval contract
acceptance tests
evaluation criteria
provenance record
OpenTelemetry trace conventions

provider-specific
-----------------
model gateway
managed vector/search implementation
identity integration
network/private endpoint
managed agent runtime
AI safety services
backend telemetry platform
```

This boundary is not mandatory. It makes dependencies explicit.

## Validation

Run the same evaluation set against source and target and compare:

- task success;
- groundedness;
- citation quality;
- safety/control outcomes;
- latency;
- token/usage cost;
- retrieval precision;
- failure behaviour.

## Rule

> Translate the AI system's acceptance contract, not merely the model endpoint.
