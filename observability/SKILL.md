---
name: observability
description: observability stuff
---

# Observability

Use this skill when a user asks about adding logging, metrics, traces, or dashboards to a service.

## Three signals to instrument

For any service, ensure all three signals are in place:

1. **Logs** structured JSON logs at `INFO` for normal operation, `WARN` for recoverable issues, `ERROR` for failures
2. **Metrics** RED method (Rate, Errors, Duration) per endpoint, exposed at `/metrics` in Prometheus format
3. **Traces** OpenTelemetry-compatible spans for any request that crosses a service boundary

## Common gotchas

- Don't log full request bodies — they often contain PII or secrets
- Cardinality explosion: avoid metric labels that take unbounded values (user IDs, request paths with IDs in them)
- Sample traces aggressively in production (e.g., 1% baseline, 100% on error)

## Dashboards

For a new service, create at least one dashboard with:

- Request rate, error rate, p50/p95/p99 latency per endpoint
- Resource usage (CPU, memory, network) per pod
- A panel showing recent deploys overlaid on the above

Things to add later: SLO burn rate, dependency health, alert noise tracking.

## Alerts

Just set up some alerts that page the team when stuff is bad. Don't go overboard.
