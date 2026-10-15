# Networking

Networking is one of the strongest examples of why cross-cloud translation cannot be done by product names alone.

## Architectural question

> What connectivity, isolation, routing and inspection behaviour does the workload require?

## High-level difference

A useful starting point is scope:

- **AWS VPC** — regional network boundary containing subnets in availability zones.
- **Azure Virtual Network** — regional network boundary containing subnets.
- **Google Cloud VPC network** — global network construct containing regional subnets.

That difference alone can change hub design, address planning, route distribution and cross-region assumptions.

## Translate these capabilities separately

### Address space

Record:

- private address ranges;
- overlap constraints;
- IPv4/IPv6 requirements;
- subnet purpose;
- expansion room;
- on-premises dependencies.

### Routing

Identify:

- local routes;
- custom routes;
- propagated routes;
- default egress path;
- appliance insertion;
- hybrid routes;
- cross-region routes;
- route priority behaviour.

### Security policy

Do not treat all cloud firewalls as the same attachment model.

Record where policy is applied:

- interface/workload;
- subnet;
- network;
- central firewall;
- edge service;
- service endpoint/private-access boundary.

### Hub connectivity

Possible families include:

| Need | AWS | Azure | Google Cloud |
|---|---|---|---|
| Hub/transit networking | Transit Gateway / Cloud WAN patterns | Virtual WAN / hub-spoke VNet patterns | Network Connectivity Center / Shared VPC patterns |
| Private hybrid connectivity | Direct Connect / VPN | ExpressRoute / VPN | Cloud Interconnect / VPN |
| Private service access | PrivateLink and service endpoints/patterns | Private Link / service endpoints | Private Service Connect / private Google access patterns |

These rows identify architectural roles, not feature equivalence.

## Ingress translation

Separate:

- global traffic steering;
- DNS;
- CDN;
- WAF;
- L7 reverse proxy/load balancing;
- L4 load balancing;
- TLS termination;
- private ingress;
- service-to-service traffic.

A single source 'load balancer' may need more than one target service because providers split global edge, regional L7 and L4 capabilities differently.

## Egress translation

Make outbound behaviour explicit:

- internet NAT;
- fixed source IP;
- proxy/inspection;
- domain filtering;
- private access to managed services;
- cross-cloud connectivity;
- data-transfer implications.

## Private does not mean isolated

Private IP addressing does not establish:

- least privilege;
- absence of internet reachability;
- traffic inspection;
- private control-plane access;
- protection from lateral movement.

Translate each control separately.

## Validation questions

- Is the network boundary regional or global?
- Which route domains exist?
- Which routes are implicit?
- Which security rules are stateful?
- Where are rules attached?
- How are shared services connected?
- How is DNS resolved across hybrid boundaries?
- How do managed services obtain private connectivity?
- Is egress predictable and observable?
- Which traffic crosses regions and creates cost/latency?
- What fails when the hub fails?

## Rule

> Translate packet paths and trust boundaries, not VPC/VNet names.
