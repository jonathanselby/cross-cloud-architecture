# Relational data

'Managed SQL database' is not a sufficient translation target.

## Start with the database contract

Record:

- engine and version;
- SQL dialect;
- extensions;
- stored procedures/triggers;
- transaction requirements;
- isolation level assumptions;
- connection model;
- replication;
- failover;
- read replicas;
- backup and point-in-time recovery;
- maintenance windows;
- encryption/key control;
- private connectivity;
- vertical/horizontal scale assumptions;
- maximum acceptable write latency;
- data residency.

## Common service families

| Need | AWS | Azure | Google Cloud |
|---|---|---|---|
| Managed common relational engines | RDS | Azure SQL family / Azure Database for PostgreSQL and MySQL | Cloud SQL |
| Cloud-optimised PostgreSQL-compatible service | Aurora PostgreSQL | Azure Database for PostgreSQL / specialised Azure offerings depending requirement | AlloyDB for PostgreSQL |
| Cloud-native globally scalable relational | Aurora global patterns are not the same category | Azure Cosmos DB is not relational SQL equivalence; other distributed relational patterns vary | Spanner |

The third row exists mainly to show why category labels become dangerous. A globally distributed relational requirement should be translated from semantics, not by forcing every provider into one named product slot.

## Engine compatibility is only the first test

Two services may both expose PostgreSQL while differing in:

- extension availability;
- superuser permissions;
- replication mechanisms;
- storage architecture;
- failover behaviour;
- connection limits;
- maintenance control;
- logical replication support;
- performance characteristics;
- backup/export behaviour.

## Availability translation

Record explicitly:

- single-zone vs multi-zone deployment;
- synchronous/asynchronous replica behaviour;
- failover trigger;
- endpoint behaviour after failover;
- expected failover time;
- read replica role;
- cross-region recovery.

## Scaling translation

Separate:

- CPU/memory vertical scale;
- storage auto-growth;
- read scaling;
- write scaling;
- connection scaling;
- serverless/elastic capacity;
- sharding/distribution.

A target service that scales storage automatically but not write throughput is not equivalent to a source architecture whose primary design need was elastic write capacity.

## Migration path

Before declaring compatibility, test:

1. schema conversion;
2. extensions;
3. data types;
4. procedural code;
5. transaction semantics;
6. drivers;
7. connection pooling;
8. migration replication tooling;
9. cutover;
10. rollback;
11. backup export and restore.

## Exit

A managed relational service is easier to leave when:

- it preserves a standard engine interface;
- data can be exported in ordinary formats;
- proprietary extensions are limited;
- application logic is not coupled to provider-specific APIs.

That does not make provider-native optimisation wrong. It makes the dependency visible.

## Rule

> Preserve the database contract and failure behaviour, not merely the SQL logo.
