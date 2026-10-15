# Architecture translation template

## 1. Source context

**Source cloud:**  
**Target cloud:**  
**System:**  
**Reason for translation:**  

## 2. Required capabilities

Link or copy the completed capability-requirements artefact.

## 3. Source architecture

```text
<diagram>
```

## 4. Source implementation map

| Component | Product | Architectural role | Required property |
|---|---|---|---|
| | | | |

## 5. Candidate target architecture

```text
<diagram>
```

## 6. Capability mapping

| Capability | Source implementation | Target implementation | Classification | Rationale |
|---|---|---|---|---|
| | | | Equivalent enough / Equivalent with conditions / Different by design / Not equivalent | |

## 7. Non-equivalences

For each material delta:

### Delta N

**Property:**  
**Source behaviour:**  
**Target behaviour:**  
**Consequence:**  
**Mitigation/design change:**  
**Evidence:**  
**Validation required:**  

## 8. Failure model

| Failure | Source behaviour | Target behaviour | Meets requirement? |
|---|---|---|---|
| process/instance | | | |
| zone | | | |
| region | | | |
| dependency | | | |
| logical corruption | | | |

## 9. Security model

**Workforce identity:**  
**Workload identity:**  
**Trust boundaries:**  
**Public paths:**  
**Private paths:**  
**Secret path:**  
**Key custody:**  
**Audit path:**  

## 10. Data and integration semantics

**Transactions:**  
**Consistency:**  
**Ordering:**  
**Duplicates:**  
**Replay:**  
**Retention:**  
**Export:**  

## 11. Operating-model delta

| Responsibility | Source owner | Target owner | Change |
|---|---|---|---|
| patch | | | |
| scale | | | |
| failover | | | |
| backup | | | |
| restore | | | |
| upgrade | | | |
| keys/secrets | | | |
| telemetry | | | |
| incident response | | | |

## 12. Cost drivers

Do not paste list prices. Record drivers.

- steady compute:
- burst compute:
- storage:
- requests/executions:
- database capacity:
- replicas/HA:
- data transfer:
- observability:
- backup:
- support/operations:

## 13. Exit consequences

**Portable interfaces retained:**  
**Provider-specific dependencies introduced:**  
**Data export path:**  
**Rebuild/migration requirement:**  

## 14. Unknowns

| Unknown | Why it matters | Evidence required | Owner |
|---|---|---|---|
| | | | |

## 15. Validation plan

- functional acceptance:
- failure injection:
- restore:
- performance:
- security:
- telemetry:
- cost observation:

## 16. Decision

**Recommendation:**  
**Conditions:**  
**Residual risks:**  
**What would change the decision:**  
