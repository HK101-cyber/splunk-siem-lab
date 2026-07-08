# Dashboard 5 — SOC Operations Dashboard

## Purpose
Operational health monitoring dashboard showing overall
SIEM performance, data ingestion health, and event volume
metrics that a SOC manager would monitor daily.

## Panels

| Panel | Type | Purpose |
|-------|------|---------|
| 1 | Single Value | Total events ingested in 24h |
| 2 | Pie Chart | Log volume by source index |
| 3 | Single Value | High severity event count |
| 4 | Area Chart | Event volume timeline |
| 5 | Bar Chart | Data source health check |
| 6 | Single Value | Average indexing latency |

## Key SPL Concepts
- Cross-index aggregation (index=A OR index=B)
- _indextime vs _time for latency calculation
- avg() statistical function

## Status: ✅ Complete — All 5 Dashboards Done
