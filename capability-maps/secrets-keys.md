# Secrets and keys

Secrets management and cryptographic key management are related but different capabilities.

## Separate the requirements

### Secret

A secret is application data that must be protected and retrieved at runtime, such as:

- database password;
- API credential;
- signing token;
- third-party client secret.

### Cryptographic key

A key is used to perform cryptographic operations and may require stronger custody controls:

- data-encryption key wrapping;
- signing;
- asymmetric operations;
- customer-managed encryption;
- HSM-backed custody.

Do not translate one category into the other simply because a provider portal presents both under security tooling.

## Common service families

| Capability | AWS | Azure | Google Cloud |
|---|---|---|---|
| Secret storage | Secrets Manager; Parameter Store for some configuration/secret patterns | Key Vault secrets | Secret Manager |
| Managed cryptographic keys | AWS KMS | Key Vault keys / Managed HSM where required | Cloud KMS / Cloud HSM options |
| Certificates | ACM plus service-specific stores; Secrets Manager for some custom patterns | Key Vault certificates | Certificate Manager plus service-specific integrations |

## Translation dimensions

### Identity path

How does the workload retrieve the secret or invoke the key?

Prefer a native workload identity over a stored bootstrap credential.

### Rotation

Record:

- who initiates rotation;
- whether rotation is automatic;
- whether the target system can update the downstream credential;
- how consumers receive the new value;
- how rollback works.

A service that can schedule rotation is not equivalent to a complete rotation process unless the downstream system participates safely.

### Key custody

For customer-managed encryption, establish:

- who creates the key;
- where key material resides;
- HSM boundary if required;
- import/external-key requirements;
- deletion protection;
- rotation behaviour;
- cross-region availability;
- audit logging;
- separation of key administrators and data administrators.

### Envelope encryption

Do not assume all 'customer-managed key' options expose the same data-key hierarchy or control point.

Understand whether the managed service:

- uses the key directly;
- wraps service-managed data keys;
- caches grants/authorisation;
- requires the key online during read/write operations.

## Common mistakes

### Replacing workload identity with stored secrets

This moves the architecture backwards even if functionality is preserved.

### Treating secret rotation as a vault feature only

Rotation is an end-to-end interaction among vault, downstream credential authority and consuming workload.

### Treating encryption checkbox as custody equivalence

Provider-managed encryption, customer-managed keys and customer-controlled HSM/external key models represent different control boundaries.

## Rule

> Translate who controls the credential or key, how it is used, and how it changes — not just where it is stored.
