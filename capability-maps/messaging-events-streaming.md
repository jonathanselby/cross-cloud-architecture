# Messaging, events and streaming

Queues, event routers and event streams solve different problems. They should not be collapsed into one 'messaging' row.

## Three distinct capabilities

### Command queue

Use when work must be buffered until a consumer can process it.

Important properties:

- durable retention;
- visibility/lease model;
- competing consumers;
- retry;
- dead-letter handling;
- ordering;
- duplicate delivery;
- delay/scheduling;
- transaction integration.

Typical candidates include SQS, Azure Service Bus queues and Google Cloud Pub/Sub or Cloud Tasks depending the execution model.

### Event distribution

Use when producers publish facts and multiple consumers react independently.

Important properties:

- filtering;
- fan-out;
- delivery destinations;
- retry;
- schema conventions;
- archive/replay;
- cross-account/subscription/project routing;
- ordering requirements.

Typical candidates include EventBridge/SNS patterns, Event Grid, Pub/Sub/Eventarc patterns.

### Event stream

Use when consumers need an ordered/replayable log or high-throughput stream-processing substrate.

Important properties:

- partitioning;
- ordering scope;
- retention window;
- replay;
- consumer offsets;
- throughput scaling;
- exactly-once or idempotency strategy;
- ecosystem compatibility.

Typical candidates include Kinesis/MSK, Event Hubs/Kafka patterns and Pub/Sub/Kafka-oriented Google Cloud patterns depending semantics.

## Why one-for-one mapping fails

A source EventBridge bus used for domain-event routing does not automatically map to a queue.

A source SQS queue used to buffer commands does not automatically map to an event grid.

A Kafka topic used as an ordered retained log does not automatically map to a push event router.

## Semantic checklist

For every channel, record:

| Property | Required? |
|---|---|
| point-to-point competing consumption | yes/no |
| fan-out | yes/no |
| ordering | none / per key / global |
| duplicate tolerance | yes/no |
| retention duration | value |
| replay | yes/no |
| dead-letter | yes/no |
| scheduled delivery | yes/no |
| transaction coupling | yes/no |
| maximum payload | requirement |
| consumer backpressure | behaviour |
| cross-region recovery | behaviour |

## Exactly-once warning

Treat 'exactly once' claims carefully.

Even where a messaging service provides deduplication or exactly-once processing features, end-to-end side effects may still require idempotent application design.

The question is not whether the product page contains the phrase. The question is whether the complete producer-channel-consumer transaction is safe under retry and partial failure.

## Rule

> Translate delivery semantics before translating messaging products.
