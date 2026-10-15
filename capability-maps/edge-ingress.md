# Edge and ingress

'Load balancer' is too broad a category for reliable cloud translation.

## Separate the required capabilities

- authoritative DNS;
- global traffic steering;
- anycast/global edge ingress;
- CDN caching;
- DDoS protection;
- web application firewall;
- TLS termination;
- L7 HTTP routing;
- L4 TCP/UDP balancing;
- private/internal load balancing;
- origin health checking;
- certificate lifecycle;
- source-IP preservation;
- session affinity.

## Common service families

| Capability | AWS | Azure | Google Cloud |
|---|---|---|---|
| DNS | Route 53 | Azure DNS | Cloud DNS |
| Global/edge web ingress | CloudFront + ALB/API/other origin patterns, Global Accelerator where appropriate | Azure Front Door | Cloud Load Balancing global external application load balancer + Cloud CDN where required |
| Regional L7 | Application Load Balancer | Application Gateway | Regional external/internal application load balancers |
| L4 | Network Load Balancer | Azure Load Balancer | Proxy or passthrough network load balancers depending requirement |
| WAF | AWS WAF | Web Application Firewall on Front Door/Application Gateway | Cloud Armor |
| CDN | CloudFront | Front Door/CDN capabilities | Cloud CDN |

These are candidate building blocks, not strict equivalents.

## Translation dimensions

### Scope

Is the ingress:

- global;
- regional;
- zonal;
- internal only?

### Protocol

- HTTP/1.1;
- HTTP/2;
- HTTP/3;
- gRPC;
- WebSocket;
- raw TCP;
- UDP.

### Routing

Does the source rely on:

- host routing;
- path routing;
- header routing;
- weighted routes;
- canary traffic;
- geographic routing;
- failover routing?

### Security

Where are:

- TLS certificates;
- private keys;
- WAF rules;
- DDoS controls;
- client certificates;
- authentication policies?

### Origin model

Can the target origin be:

- VM;
- container;
- function;
- object storage;
- private endpoint;
- external origin?

## Common translation error

```text
Source: CloudFront -> ALB -> ECS
Target: one regional load balancer -> containers
```

This may accidentally remove global edge termination, caching, WAF integration or geographic failover that lived above the ALB.

The reverse can also happen: a source regional ALB does not justify adding a global edge service in the target unless the requirements need one.

## Rule

> Decompose ingress into edge, protocol, routing and security capabilities before selecting a target load-balancing product.
