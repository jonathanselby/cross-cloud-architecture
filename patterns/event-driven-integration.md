# Pattern: event-driven integration

## Intent

Accept a business command reliably, process it asynchronously, then publish a domain event to multiple downstream consumers.

The command and event are different architectural objects.

## Capability model

```text
caller
  |
  v
[API / ingress]
  |
  v
[DURABLE COMMAND QUEUE]
  |
  v
[worker]
  |
  +----> [authoritative state]
  |
  v
[DOMAIN EVENT]
  |
  +----> consumer A
  +----> consumer B
  +----> consumer C
```

## Required semantics

Command path:

- caller receives acknowledgement after durable acceptance;
- message survives consumer outage;
- retry is expected;
- consumer is idempotent;
- dead-letter path exists;
- duplicate delivery is safe.

Event path:

- multiple independent consumers;
- publisher does not know consumer count;
- filtering is useful;
- consumers can fail independently;
- event contract is versioned.

## AWS implementation family

```text
API Gateway / ALB / service ingress
        |
       SQS
        |
 worker (Lambda/ECS/etc.)
        |
 authoritative state
        |
 EventBridge and/or SNS pattern
        |
 downstream consumers
```

## Azure implementation family

```text
API Management / Front Door / application ingress
        |
 Service Bus queue
        |
 worker (Functions/Container Apps/etc.)
        |
 authoritative state
        |
 Event Grid or Service Bus topic depending semantics
        |
 downstream consumers
```

## Google Cloud implementation family

```text
API Gateway / load-balanced service ingress
        |
 Pub/Sub subscription or Cloud Tasks depending command semantics
        |
 worker (Cloud Run/etc.)
        |
 authoritative state
        |
 Pub/Sub / Eventarc pattern
        |
 downstream consumers
```

The exact Google Cloud choice is deliberately conditional: Pub/Sub and Cloud Tasks represent different execution semantics. Do not select one merely to fill a three-column table.

## Transaction boundary

A harder problem appears when state change and event publication must not diverge.

Possible approaches include:

- transactional outbox;
- change-data capture;
- provider-specific transactional messaging where appropriate;
- idempotent reconciliation.

Do not assume a queue or event bus makes database + publish atomic.

## Validation tests

- kill the consumer after receiving a command but before completion;
- deliver the command twice;
- disable one event consumer;
- poison one message;
- fill backlog and observe age/scale behaviour;
- replay or resend events where the design requires it;
- verify dead-letter recovery;
- confirm trace propagation across asynchronous boundaries.

## Rule

> Translate command, event and stream semantics separately, even if the source provider groups them under one messaging portfolio.
