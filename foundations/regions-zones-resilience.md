# Regions, zones and resilience

Cross-cloud architecture fails quickly when location terms are treated as interchangeable.

## Architectural question

> Which failures must the workload survive, and which components actually provide that survival in the target cloud?

## Common location concepts

All three hyperscalers expose regions and availability-zone concepts, but service behaviour across those locations differs by product.

Do not infer a service's failure behaviour from the provider's region/zone vocabulary alone.

For each service, establish:

- whether the resource is zonal, regional, multi-regional or global;
- whether replicas are implicit or explicit;
- whether traffic failover is automatic;
- whether state failover is automatic;
- whether failover changes endpoint or connection behaviour;
- whether recovery requires operator action;
- whether cross-region replication is synchronous or asynchronous;
- which control-plane actions remain possible during an outage.

## Resilience layers

```text
PROCESS / INSTANCE FAILURE
        |
        v
ZONE FAILURE
        |
        v
REGION FAILURE
        |
        v
PROVIDER / CONTROL-PLANE IMPAIRMENT
        |
        v
APPLICATION OR DATA CORRUPTION
```

A multi-zone design does not solve data corruption. A second region does not solve a bad deployment. A managed database does not prove the restore path works.

## Translation procedure

### 1. State the required failure tolerance

For example:

- individual runtime failure: automatic recovery;
- single-zone failure: no material user outage;
- regional failure: restore service within four hours;
- data-loss objective: no more than fifteen minutes;
- destructive application error: point-in-time recovery required.

### 2. Map the source mechanisms

Identify which source components provide:

- health detection;
- traffic redistribution;
- replica placement;
- database failover;
- durable storage replication;
- backup;
- cross-region copy;
- DNS/global routing;
- orchestration.

### 3. Rebuild in the target

Do not ask for 'the equivalent HA service'. Ask whether the target combination meets the same failure objective.

## Common mistakes

### Counting zones instead of testing service behaviour

A region with multiple zones does not mean every managed service is deployed or failed over in the same way.

### Treating backup as HA

Backups protect recoverability. They do not keep a service available during an infrastructure failure.

### Treating replicas as backups

Replicated corruption is still corruption.

### Assuming regional services imply regional independence

Dependencies such as identity, DNS, control planes, CI/CD, secrets, logging or external APIs may introduce wider failure coupling.

## Evidence to capture

| Question | Evidence |
|---|---|
| What failure domains exist? | current provider service documentation |
| Where are replicas placed? | service configuration + docs |
| How is failover triggered? | architecture/configuration + runbook |
| What is the actual RTO? | tested failover/recovery exercise |
| What is the actual RPO? | replication/backup settings + restore test |
| What changes in the target? | translation delta record |

## Rule

> Translate the required failure outcome, not the source provider's resilience label.
