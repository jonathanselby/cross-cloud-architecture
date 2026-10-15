# Cross-Cloud Architecture Skill

Use this skill to translate an architecture among AWS, Azure and Google Cloud.

## Objective

Produce a target architecture that preserves required system properties without assuming one-to-one product equivalence.

## Rules

1. **Start from architecture, not cloud products.**
2. **Do not invent facts.** If a current service limit, feature or region availability matters, verify it from first-party documentation.
3. **Separate known, inferred and unknown.**
4. **Preserve required semantics.** Especially data consistency, transactions, ordering, delivery, replay, identity and failure behaviour.
5. **Do not force one source component to one target component.**
6. **Do not add Kubernetes, VMs or other operating machinery without a requirement.**
7. **Do not call two services equivalent without stating the property and conditions that make them equivalent enough.**
8. **Treat operations as architecture.** Patch, scale, failover, backup, restore, upgrade and incident ownership must remain visible.
9. **Treat exit as part of the design.** Record provider-specific dependencies introduced.
10. **Prefer executable validation over confident prose.**

## Required workflow

### 1. Recover system context

Capture:

- purpose;
- users/calling systems;
- critical transactions;
- authoritative state;
- criticality;
- RTO/RPO;
- location constraints;
- security constraints;
- operating model.

### 2. Convert source products to capability statements

For each source component output:

| Source product | Architectural role | Required property |
|---|---|---|

Bad:

`SQS -> Service Bus`

Good:

`SQS -> durable asynchronous command buffer; competing consumers; duplicate-safe; dead-letter recovery required`

### 3. Extract semantic constraints

Explicitly check:

- database engine/dialect/extensions;
- transactions;
- consistency;
- queue/event/stream role;
- ordering;
- duplicates;
- replay;
- runtime concurrency;
- identity claims;
- network scope;
- object retention;
- key custody;
- telemetry protocol.

### 4. Record operating assumptions

Who owns:

- OS/runtime patching;
- cluster/runtime upgrades;
- scaling;
- failover;
- backup;
- restore;
- secret/key rotation;
- observability;
- incident response.

### 5. Design the target natively

Use target-cloud products only after the requirements are stable.

Allow:

- one source component -> several target components;
- several source components -> one target service;
- topology changes;
- managed-service changes;
- provider-native optimisation.

### 6. Classify every material mapping

Use only:

- **Equivalent enough**
- **Equivalent with conditions**
- **Different by design**
- **Not equivalent**

For conditions/differences include rationale and validation.

### 7. Validate the whole target

Review:

- failure;
- security;
- data;
- integration;
- operations;
- observability;
- cost drivers;
- exit.

## Required output structure

```markdown
# Cross-cloud architecture translation

## Executive summary

## System contract

## Source architecture

## Required capabilities

## Semantic constraints

## Target architecture

## Mapping table

## Non-equivalences

## Failure model

## Security model

## Operating-model changes

## Cost drivers

## Exit consequences

## Unknowns

## Validation plan

## Recommendation
```

## Mapping-table format

| Capability | Source | Target | Classification | Evidence | Validation |
|---|---|---|---|---|---|

## Challenge prompts

Before finishing, ask internally:

- Have I translated a product name instead of a property?
- Have I preserved a source topology merely because it is familiar?
- Have I introduced Kubernetes without a Kubernetes requirement?
- Have I confused a queue with an event bus or stream?
- Have I assumed PostgreSQL compatibility means operational equivalence?
- Have I assumed 'multi-zone' means identical failover behaviour?
- Have I introduced a static secret where the source had workload identity?
- Have I claimed portability because containers or IaC exist?
- Have I checked provider facts that could have changed?
- Can I explain what would fail differently in the target?

If any answer is weak, the translation is not finished.
