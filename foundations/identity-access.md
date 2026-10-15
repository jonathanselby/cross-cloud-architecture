# Identity and access

Identity translation is one of the easiest places to preserve the product label while changing the security model.

## Architectural question

> Who or what is asking for access, how is that identity established, and how does the target resource authorise it?

Separate three identity classes:

1. **Workforce identity** — employees, administrators, engineers and support staff.
2. **Workload identity** — applications, functions, containers, VMs and automation.
3. **External identity** — partners, customers, CI systems and identities federated from another authority.

## Common capability mapping

| Capability | AWS | Azure | Google Cloud |
|---|---|---|---|
| Workforce federation / SSO | IAM Identity Center with external IdP support | Microsoft Entra ID | Workforce Identity Federation / Cloud Identity or external IdP integration |
| Resource authorisation | IAM policies, roles and resource policies | Azure RBAC plus service-specific controls | IAM roles/policies plus service-specific controls |
| Workload identity | IAM roles for services, instance/task/function roles, OIDC federation | Managed identities, workload identity federation, service principals where required | Service accounts and Workload Identity Federation |
| Short-lived credentials | STS and role assumption | Entra-issued tokens / managed identity flows | STS-based federation and service account credentials |

These are architectural families, not claims of feature parity.

## Translation dimensions

### Authority

Who is the source of identity?

- corporate directory;
- cloud-native directory;
- CI/CD platform;
- Kubernetes issuer;
- external partner;
- application identity store.

### Credential lifetime

Prefer short-lived, automatically issued workload credentials where the platform supports them.

A translation that replaces a source workload role with a stored client secret is usually a security regression even if the target application still authenticates successfully.

### Binding model

Establish whether access is granted through:

- identity role membership;
- resource-level policy;
- subscription/project/account-level role;
- group membership;
- workload federation;
- service-specific ACL.

### Inheritance

Authorisation inherited from organisation, management-group, subscription, folder or project scope can change blast radius substantially.

### Resource identity

Azure managed identities are attached to Azure resources or created as user-assigned identities. Google service accounts and AWS IAM roles express workload identity differently. Do not model them as identical objects merely because all can remove embedded credentials.

## Translation questions

- Can the target workload obtain short-lived credentials without a stored secret?
- Which trust policy allows the workload to assume/use the identity?
- Where is authorisation attached?
- What is the smallest sensible scope?
- Does the target introduce a tenant-wide or project-wide dependency?
- How are break-glass identities handled?
- How are human admin and workload permissions separated?
- How are access changes logged?
- What happens when the identity provider is impaired?

## Common anti-pattern

```text
SOURCE:
container -> native workload role -> database

BAD TRANSLATION:
container -> static client secret -> application identity -> database
```

The second design may function while introducing a long-lived credential, rotation burden and secret-distribution path that did not exist before.

## Rule

> Preserve the trust model and credential properties, not the IAM object name.
