# Pattern: three-tier service

## Intent

Public HTTPS application with stateless application tier and managed relational persistence.

## Capability model

```text
Internet users
      |
      v
[global/regional ingress + TLS + WAF]
      |
      v
[stateless application runtime]
      |
      +----> [object storage]
      |
      v
[managed relational database]
```

Required properties:

- TLS termination;
- application health checks;
- horizontal application scale;
- private application-to-database path;
- managed PostgreSQL-compatible persistence;
- zone-failure tolerance;
- secrets delivered without embedding credentials;
- metrics/logs/traces;
- backup + point-in-time restore.

## AWS implementation family

One plausible design:

```text
Route 53
   |
CloudFront + WAF (if edge/global capability required)
   |
Application Load Balancer
   |
ECS/Fargate or another suitable runtime
   |
RDS/Aurora PostgreSQL

S3 for objects
Secrets Manager + KMS
CloudWatch / OTel
```

## Azure implementation family

```text
Azure DNS
   |
Front Door + WAF (if global edge required)
   |
Container Apps / App Service / AKS depending runtime contract
   |
Azure Database for PostgreSQL

Blob Storage for objects
Managed Identity -> Key Vault
Azure Monitor / OTel
```

Do not insert Application Gateway merely because the source used ALB. Use it only if regional L7/network integration requires it.

## Google Cloud implementation family

```text
Cloud DNS
   |
Global or regional Application Load Balancer + Cloud Armor
   |
Cloud Run / GKE / Compute Engine depending runtime contract
   |
Cloud SQL or AlloyDB for PostgreSQL depending requirements

Cloud Storage for objects
Workload identity -> Secret Manager / Cloud KMS
Cloud Operations / OTel
```

## Translation deltas to test

- global vs regional ingress scope;
- runtime concurrency and scaling model;
- managed identity/workload credential mechanism;
- database failover model;
- private service connectivity;
- log/metric/trace backend;
- backup and restore workflow;
- egress path and data-transfer cost.

## Key lesson

All three target architectures can satisfy the same three-tier intent while using different numbers of products and different managed boundaries.

The architecture is the property set. The diagrams are implementations.
