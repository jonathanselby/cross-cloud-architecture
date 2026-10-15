# Observability

Observability should be translated as an information model, not as a monitoring-product substitution.

## Required capabilities

Separate:

- metrics;
- logs;
- traces;
- profiles;
- service health;
- audit/control-plane logs;
- dashboards;
- alerting;
- SLO measurement;
- incident correlation;
- retention;
- export to external tooling.

## Common service families

| Capability | AWS | Azure | Google Cloud |
|---|---|---|---|
| Metrics/logs | CloudWatch | Azure Monitor / Log Analytics | Cloud Monitoring / Cloud Logging |
| Tracing | X-Ray / CloudWatch tracing integrations | Application Insights / Azure Monitor tracing | Cloud Trace |
| Audit logs | CloudTrail | Azure Activity Log / resource logs | Cloud Audit Logs |
| OpenTelemetry support | AWS Distro for OpenTelemetry / native integrations | Azure Monitor OpenTelemetry distro/integrations | Google-built OpenTelemetry integrations / managed collectors and exporters |

## Portable boundary

OpenTelemetry can preserve instrumentation conventions across providers:

- traces;
- metrics;
- logs where supported;
- resource attributes;
- context propagation.

It does not standardise:

- backend query language;
- dashboard model;
- alert configuration;
- retention pricing;
- log routing;
- provider-native audit events;
- service health events;
- platform-specific metrics.

## Translation method

### 1. Define the questions operators must answer

Examples:

- Is the service available?
- Which dependency is slow?
- Which release caused the error increase?
- Which request crossed which services?
- Has queue age breached the SLO?
- Did a human or automation change the production resource?

### 2. Identify required telemetry

For each question, identify the signals required.

### 3. Separate application and platform telemetry

Application telemetry can often be made portable.

Platform telemetry is inherently tied to the provider's services and control plane.

### 4. Translate alert semantics

Do not only recreate alert thresholds. Check:

- aggregation window;
- missing-data behaviour;
- evaluation period;
- dimension/cardinality support;
- notification routing;
- suppression;
- incident grouping.

### 5. Recalculate cost drivers

Observability economics can change materially due to:

- ingestion volume;
- log indexing;
- metric cardinality;
- trace sampling;
- retention;
- cross-region export;
- archive/replay.

## Anti-pattern

```text
CloudWatch -> Azure Monitor -> Cloud Monitoring
```

This says almost nothing about whether the same operational questions can be answered.

## Better translation

```text
Requirement:
Trace one dispatch request across ingress -> application -> queue -> worker -> database.

Portable layer:
W3C trace context + OpenTelemetry instrumentation.

Provider layer:
collector/export path + managed trace backend + logs/metrics correlation + alerting.
```

## Rule

> Preserve the operator's ability to explain the system, not the dashboard layout.
