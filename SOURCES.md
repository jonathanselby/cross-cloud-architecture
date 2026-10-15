# Source register

Provider-specific facts in this repository should be verified against current first-party documentation before a production decision.

**Reference check date for v0.1.0: 15 October 2026.**

This file is a source index, not a frozen copy of provider documentation.

## AWS

### Organisation and identity

- AWS Organizations: https://docs.aws.amazon.com/organizations/latest/userguide/orgs_introduction.html
- AWS IAM: https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html
- AWS IAM Identity Center: https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html
- AWS STS: https://docs.aws.amazon.com/STS/latest/APIReference/welcome.html

### Network and resilience

- Amazon VPC: https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html
- VPC route tables: https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html
- Security groups: https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html
- AWS Transit Gateway: https://docs.aws.amazon.com/vpc/latest/tgw/what-is-transit-gateway.html
- AWS Regions and Availability Zones: https://docs.aws.amazon.com/global-infrastructure/latest/regions/aws-regions-availability-zones.html
- Elastic Load Balancing: https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/what-is-load-balancing.html

### Compute

- Amazon EC2: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html
- Amazon ECS: https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html
- AWS Fargate: https://docs.aws.amazon.com/AmazonECS/latest/developerguide/AWS_Fargate.html
- Amazon EKS: https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html
- AWS Lambda: https://docs.aws.amazon.com/lambda/latest/dg/welcome.html
- AWS Batch: https://docs.aws.amazon.com/batch/latest/userguide/what-is-batch.html

### Storage and data

- Amazon S3: https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html
- S3 Object Lock: https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html
- Amazon RDS: https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html
- Amazon Aurora: https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP_AuroraOverview.html

### Messaging and events

- Amazon SQS: https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html
- Amazon SNS: https://docs.aws.amazon.com/sns/latest/dg/welcome.html
- Amazon EventBridge: https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html
- Amazon Kinesis Data Streams: https://docs.aws.amazon.com/streams/latest/dev/introduction.html

### Security and observability

- AWS Secrets Manager: https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html
- AWS KMS: https://docs.aws.amazon.com/kms/latest/developerguide/overview.html
- Amazon CloudWatch: https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html
- AWS CloudTrail: https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html
- AWS Distro for OpenTelemetry: https://aws-otel.github.io/

### AI

- Amazon Bedrock: https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html
- Amazon SageMaker AI: https://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html

---

## Microsoft Azure

### Organisation and identity

- Azure resource hierarchy / management groups: https://learn.microsoft.com/azure/governance/management-groups/overview
- Azure subscriptions and resource groups: https://learn.microsoft.com/azure/cloud-adoption-framework/ready/azure-setup-guide/organize-resources
- Microsoft Entra ID: https://learn.microsoft.com/entra/fundamentals/whatis
- Managed identities: https://learn.microsoft.com/entra/identity/managed-identities-azure-resources/overview
- Azure RBAC: https://learn.microsoft.com/azure/role-based-access-control/overview

### Network and resilience

- Azure Virtual Network: https://learn.microsoft.com/azure/virtual-network/virtual-networks-overview
- Network security groups: https://learn.microsoft.com/azure/virtual-network/network-security-groups-overview
- User-defined routes: https://learn.microsoft.com/azure/virtual-network/virtual-networks-udr-overview
- Azure Virtual WAN: https://learn.microsoft.com/azure/virtual-wan/virtual-wan-about
- Azure regions and availability zones: https://learn.microsoft.com/azure/reliability/regions-overview
- Azure Front Door: https://learn.microsoft.com/azure/frontdoor/front-door-overview
- Application Gateway: https://learn.microsoft.com/azure/application-gateway/overview
- Azure Load Balancer: https://learn.microsoft.com/azure/load-balancer/load-balancer-overview

### Compute

- Azure Virtual Machines: https://learn.microsoft.com/azure/virtual-machines/overview
- Azure Functions: https://learn.microsoft.com/azure/azure-functions/functions-overview
- Azure Container Apps: https://learn.microsoft.com/azure/container-apps/overview
- Azure Kubernetes Service: https://learn.microsoft.com/azure/aks/what-is-aks
- Azure App Service: https://learn.microsoft.com/azure/app-service/overview

### Storage and data

- Azure Blob Storage: https://learn.microsoft.com/azure/storage/blobs/storage-blobs-introduction
- Blob immutable storage: https://learn.microsoft.com/azure/storage/blobs/immutable-storage-overview
- Azure SQL: https://learn.microsoft.com/azure/azure-sql/azure-sql-iaas-vs-paas-what-is-overview
- Azure Database for PostgreSQL: https://learn.microsoft.com/azure/postgresql/flexible-server/overview

### Messaging and events

- Azure Service Bus: https://learn.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview
- Azure Event Grid: https://learn.microsoft.com/azure/event-grid/overview
- Azure Event Hubs: https://learn.microsoft.com/azure/event-hubs/event-hubs-about

### Security and observability

- Azure Key Vault: https://learn.microsoft.com/azure/key-vault/general/overview
- Azure Monitor: https://learn.microsoft.com/azure/azure-monitor/fundamentals/overview
- Application Insights: https://learn.microsoft.com/azure/azure-monitor/app/app-insights-overview
- OpenTelemetry with Azure Monitor: https://learn.microsoft.com/azure/azure-monitor/app/opentelemetry-enable

### AI

- Microsoft Foundry: https://learn.microsoft.com/azure/ai-foundry/what-is-azure-ai-foundry
- Azure Machine Learning: https://learn.microsoft.com/azure/machine-learning/overview-what-is-azure-machine-learning
- Azure AI Search: https://learn.microsoft.com/azure/search/search-what-is-azure-search

---

## Google Cloud

### Organisation and identity

- Resource hierarchy: https://cloud.google.com/resource-manager/docs/cloud-platform-resource-hierarchy
- Cloud IAM overview: https://cloud.google.com/iam/docs/overview
- Service accounts: https://cloud.google.com/iam/docs/service-account-overview
- Workload Identity Federation: https://cloud.google.com/iam/docs/workload-identity-federation
- Workforce Identity Federation: https://cloud.google.com/iam/docs/workforce-identity-federation

### Network and resilience

- VPC overview: https://cloud.google.com/vpc/docs/vpc
- VPC firewall rules: https://cloud.google.com/firewall/docs/firewalls
- Network Connectivity Center: https://cloud.google.com/network-connectivity/docs/network-connectivity-center/concepts/overview
- Regions and zones: https://cloud.google.com/compute/docs/regions-zones
- Cloud Load Balancing: https://cloud.google.com/load-balancing/docs/load-balancing-overview
- Cloud Armor: https://cloud.google.com/armor/docs/cloud-armor-overview

### Compute

- Compute Engine: https://cloud.google.com/compute/docs/overview
- GKE: https://cloud.google.com/kubernetes-engine/docs/concepts/kubernetes-engine-overview
- Cloud Run: https://cloud.google.com/run/docs/overview/what-is-cloud-run
- Cloud Run functions: https://cloud.google.com/functions/docs/concepts/overview
- Batch: https://cloud.google.com/batch/docs/get-started

### Storage and data

- Cloud Storage: https://cloud.google.com/storage/docs/introduction
- Object retention: https://cloud.google.com/storage/docs/object-lock
- Cloud SQL: https://cloud.google.com/sql/docs/introduction
- AlloyDB: https://cloud.google.com/alloydb/docs/overview
- Spanner: https://cloud.google.com/spanner/docs/overview

### Messaging and events

- Pub/Sub: https://cloud.google.com/pubsub/docs/overview
- Eventarc: https://cloud.google.com/eventarc/docs/overview
- Cloud Tasks: https://cloud.google.com/tasks/docs

### Security and observability

- Secret Manager: https://cloud.google.com/secret-manager/docs/overview
- Cloud KMS: https://cloud.google.com/kms/docs/key-management-service
- Cloud Monitoring: https://cloud.google.com/monitoring/docs/monitoring-overview
- Cloud Logging: https://cloud.google.com/logging/docs/overview
- Cloud Trace: https://cloud.google.com/trace/docs/overview
- Google Cloud OpenTelemetry: https://cloud.google.com/stackdriver/docs/instrumentation/opentelemetry

### AI

- Vertex AI: https://cloud.google.com/vertex-ai/docs/start/introduction-unified-platform
- Model Garden: https://cloud.google.com/vertex-ai/generative-ai/docs/model-garden/explore-models
- Vertex AI custom training: https://cloud.google.com/vertex-ai/docs/training/overview

---

## Source discipline

When a service-specific claim materially affects a design:

1. prefer first-party provider documentation;
2. record the exact feature/behaviour being relied on;
3. record the date checked;
4. verify target region availability separately;
5. verify quotas/limits separately;
6. test behaviour where failure, delivery or correctness matters.

Provider documentation establishes stated behaviour. Production testing establishes whether the complete architecture meets the workload requirement.
