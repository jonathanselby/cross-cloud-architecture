# Object storage

Object storage looks simple until architecture depends on retention, replication, access or event behaviour.

## Capability questions

- Is the object immutable or mutable?
- Is versioning required?
- Is retention legally enforced?
- Is write-once/read-many behaviour required?
- Is cross-region replication required?
- Must objects remain in one country or region?
- Is strong read-after-write behaviour assumed?
- Are object-created events part of the integration design?
- Is public delivery required?
- Are lifecycle tiers required?
- Is customer-managed key control required?
- Are object names part of an application contract?

## Common services

| Capability family | AWS | Azure | Google Cloud |
|---|---|---|---|
| General object storage | Amazon S3 | Azure Blob Storage | Cloud Storage |
| Archive/cold tiers | S3 storage classes | Blob access tiers / archive | Cloud Storage classes |
| Retention / immutability | S3 Object Lock and retention features | Immutable storage for Blob Storage | Bucket/object retention features |
| Event integration | S3 notifications / EventBridge patterns | Event Grid / storage events | Eventarc / Pub/Sub patterns |

## What to compare

### Consistency

Modern object stores generally provide strong consistency for common object operations, but do not rely on a remembered historical behaviour. Verify the current guarantee for the exact API and operation used.

### Namespace and location

Translate:

- bucket/container scope;
- region/multi-region choice;
- replication placement;
- naming/global-namespace implications;
- private endpoint behaviour.

### Retention

Separate:

- lifecycle deletion;
- version retention;
- legal hold;
- governance retention;
- compliance/WORM retention.

A lifecycle rule that keeps an object for seven years is not necessarily the same control as an immutable retention policy that prevents deletion.

### Access

Record:

- identity-based access;
- resource policies;
- signed URLs/tokens;
- public-access blocks;
- cross-account/subscription/project access;
- encryption-key dependencies.

### Events

If object creation triggers processing, validate:

- event delivery guarantee;
- ordering;
- duplicate behaviour;
- filtering;
- retry behaviour;
- destination types.

Do not assume the storage product itself provides a transactional event with the object write.

## Portability decision

Object APIs are often easier to abstract than managed databases or identity, but provider-native access control, retention and event integration can create substantial dependencies.

## Rule

> Translate the data durability, access and retention contract; do not stop at 'object storage'.
