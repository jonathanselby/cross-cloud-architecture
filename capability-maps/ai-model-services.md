# AI and model services

AI platforms change quickly, and their product boundaries do not line up cleanly across providers.

This file therefore starts with capabilities rather than product names.

## Separate the capability layers

### 1. Foundation-model access

Requirement examples:

- invoke hosted third-party or provider models;
- select among multiple model families;
- control region/data handling;
- obtain usage telemetry;
- apply safety controls;
- provision throughput or use on-demand capacity.

Common service families include Amazon Bedrock, Microsoft Foundry model capabilities and Vertex AI Model Garden / generative AI services.

### 2. Custom model development

Requirement examples:

- notebooks/development environments;
- training jobs;
- hyperparameter tuning;
- experiment tracking;
- model registry;
- custom containers;
- GPU/accelerator scheduling.

Common service families include SageMaker AI, Azure Machine Learning / Microsoft Foundry integrations, and Vertex AI custom training.

### 3. Managed inference

Requirement examples:

- online endpoint;
- autoscaling;
- dedicated/provisioned capacity;
- batch inference;
- traffic splitting;
- private endpoint;
- model monitoring.

### 4. Retrieval and grounding

Requirement examples:

- embeddings;
- vector index;
- document ingestion;
- chunking;
- retrieval API;
- access-aware retrieval;
- citation/grounding metadata.

### 5. Agent/runtime orchestration

Requirement examples:

- tool calling;
- state/session management;
- managed agent runtime;
- workflow orchestration;
- guardrails;
- human approval;
- tracing/evaluation.

### 6. Evaluation and governance

Requirement examples:

- offline evaluation datasets;
- model/prompt version trace;
- safety testing;
- human review;
- latency/cost measurement;
- quality acceptance criteria;
- auditability.

## Why the service-table approach breaks down

A source architecture may use one provider product for model access, another for vector retrieval and custom application code for orchestration.

A target cloud may bundle some of those capabilities into a broader platform.

One-to-one mapping can therefore either:

- recreate unnecessary source complexity; or
- accidentally move application logic into a proprietary managed runtime.

Both may be valid choices. They are different architectures.

## Translation dimensions

### Model portability

Record:

- exact model family/version;
- API schema;
- system/tool calling behaviour;
- context limits;
- structured output support;
- safety filtering;
- regional availability;
- provisioned-capacity model;
- model retirement/change policy.

Do not assume the same named third-party model behaves identically through every provider gateway. Provider integrations can expose different versions, features, quotas or surrounding controls.

### Prompt/application portability

Separate:

- model-facing prompt/instructions;
- provider API wrapper;
- retrieval logic;
- tool orchestration;
- session state;
- evaluations;
- safety controls.

The more these are entangled inside provider-specific managed features, the higher the translation effort.

### Retrieval portability

A vector index is only one part of RAG.

Also translate:

- ingestion pipeline;
- chunking;
- embeddings;
- metadata filters;
- authorisation;
- ranking;
- citations;
- refresh/delete semantics;
- source provenance.

### Evaluation portability

Keep acceptance criteria outside the provider where possible.

For example:

```text
input set
   |
   v
model/application under test
   |
   v
structured result
   |
   +-- task success
   +-- groundedness
   +-- latency
   +-- cost
   +-- safety/control outcome
```

Provider tooling can help execute evaluation, but the definition of acceptable behaviour should remain owned by the application/team.

## Non-equivalence triggers

Record a material delta if translation changes:

- model/version;
- model API contract;
- prompt behaviour;
- safety filtering;
- retrieval semantics;
- vector/embedding model;
- agent state model;
- tool-calling behaviour;
- evaluation method;
- trace/audit data;
- region/data-processing boundary;
- capacity/performance model.

## Rule

> Translate the AI system as a chain of capabilities. Do not treat a provider's AI platform as one portable box.
