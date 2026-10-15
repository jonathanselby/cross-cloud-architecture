# Resource hierarchy

Resource hierarchy is not administrative trivia. It determines where policy, billing, access, quotas and delegated control are applied.

## Architectural question

> What boundary should own this workload, and what governance should inherit into it?

## Common hierarchy shapes

| Concern | AWS | Azure | Google Cloud |
|---|---|---|---|
| Top-level organisation | AWS Organizations organisation | Microsoft Entra tenant plus Azure management-group hierarchy | Organization resource |
| Intermediate grouping | Organizational units | Management groups | Folders |
| Common workload/billing boundary | Account | Subscription | Project |
| Resource grouping below that | Service-specific resources/tags; no universal resource-group layer | Resource groups | Service resources within projects; labels/tags for grouping |
| Policy mechanisms | SCPs, IAM policies, service policies, tag policies | Azure Policy, RBAC, management-group inheritance | Organization Policy, IAM inheritance, tags |

These rows are **analogous roles**, not strict equivalents.

## Important differences

### AWS accounts are strong isolation boundaries

AWS designs multi-account estates around separate accounts for workloads, environments and security boundaries. Accounts have independent service quotas, billing dimensions and IAM contexts inside the organisation.

Moving an AWS workload into one Azure subscription or one Google Cloud project may collapse boundaries that were previously separated across several accounts.

### Azure separates identity tenant from resource hierarchy

Microsoft Entra ID supplies the identity tenant. Azure resources then sit under management groups, subscriptions and resource groups.

A resource group is a useful lifecycle and RBAC scope, but it is not an AWS account or Google Cloud project analogue.

### Google Cloud projects are fundamental resource containers

Projects are a core boundary for APIs, IAM, quotas and billing association. They inherit through folders and the organisation resource.

Do not translate several AWS accounts into one project merely because both are sometimes called 'projects' informally by delivery teams.

## Translation questions

Before mapping resources, ask:

- Is the source boundary primarily for blast radius, delegated administration, billing, quota isolation or environment separation?
- Which policies must inherit from above?
- Which teams need independent administration?
- Which identities cross the boundary?
- Which logs must be centralised outside the workload boundary?
- Does the target need multiple accounts/subscriptions/projects to preserve source isolation?
- Are shared network or security services deliberately centralised?

## Example

Source AWS estate:

```text
Organization
+-- Security OU
|   +-- Log Archive account
|   +-- Security Tooling account
+-- Workloads OU
    +-- Dispatch Prod account
    +-- Dispatch NonProd account
```

A weak Azure translation is:

```text
One subscription
+-- rg-security
+-- rg-dispatch-prod
+-- rg-dispatch-nonprod
```

That may remove subscription-level isolation, quota separation and delegated administration that the source architecture relied on.

A more faithful target may use multiple subscriptions under management groups, with resource groups *inside* each subscription.

## Rule

> Translate the reason for the boundary before translating the boundary name.
