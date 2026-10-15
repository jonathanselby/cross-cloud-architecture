# Capability requirements template

Use this before naming target-cloud services.

## System

**Name:**  
**Purpose:**  
**Users / calling systems:**  
**Critical business transactions:**  
**Authoritative state:**  
**Criticality:**  

## Location and resilience

**Required regions/countries:**  
**Zone-failure tolerance:**  
**Regional-failure objective:**  
**RTO:**  
**RPO:**  
**Backup/restore objective:**  

## Runtime

**Execution model:** long-running / request / event / batch  
**Deployment unit:** VM / container / function / source package / other  
**OS control required:**  
**Kubernetes semantics required:**  
**Scale-to-zero required:**  
**Minimum capacity required:**  
**Stateful local storage required:**  
**Accelerator required:**  

## Data

**Database interface/engine:**  
**Transaction requirements:**  
**Consistency requirements:**  
**Extensions/features:**  
**Read/write scaling:**  
**Private connectivity:**  
**Export/exit requirement:**  

## Objects/files

**Object/file requirement:**  
**Versioning:**  
**Retention/immutability:**  
**Replication:**  
**Event triggers:**  

## Messaging and events

For each channel record:

| Channel | Command/event/stream | Ordering | Delivery | Retention | Replay | DLQ | Duplicate-safe |
|---|---|---|---|---|---|---|---|
| | | | | | | | |

## Identity

**Workforce authority:**  
**Workload identity requirement:**  
**Federation:**  
**Static credentials permitted:** yes/no  
**Privileged access model:**  

## Networking

**Public ingress:**  
**Private ingress:**  
**Private service access:**  
**Hybrid connectivity:**  
**Fixed egress IP:**  
**Inspection/proxy:**  
**DNS dependencies:**  

## Keys and secrets

**Secret storage:**  
**Rotation:**  
**Customer-managed keys:**  
**HSM/external key requirement:**  

## Observability

**Required operator questions:**  
**Metrics:**  
**Logs:**  
**Traces:**  
**Audit logs:**  
**SLOs:**  
**Retention:**  
**External export:**  

## AI/model capabilities

**Model/version requirement:**  
**Inference:**  
**Training:**  
**Retrieval:**  
**Agent/runtime:**  
**Evaluation:**  
**Safety/governance:**  
**Data-processing constraints:**  

## Operating model

| Responsibility | Current owner | Required target owner |
|---|---|---|
| OS patching | | |
| runtime upgrades | | |
| scaling | | |
| failover | | |
| backup | | |
| restore | | |
| key rotation | | |
| telemetry | | |
| incident response | | |

## Hard constraints

- 

## Preferences

- 

## Unknowns to verify

- 
