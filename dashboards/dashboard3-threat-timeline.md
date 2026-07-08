# Dashboard 3 — Threat Timeline Dashboard

## Purpose
Provide unified view of threats across both Linux and
Windows environments, showing attack correlation and
technique distribution over time.

## Panels

| Panel | Type | Purpose |
|-------|------|---------|
| 1 | Line Chart | Combined attack timeline Linux + Windows |
| 2 | Single Value | Total threat events |
| 3 | Pie Chart | Linux vs Windows attack distribution |
| 4 | Bar Chart | Top attacking IPs |
| 5 | Line Chart | Attack techniques over time |
| 6 | Single Value | Sudo command executions |

## Key SPL Concepts
- Cross-index correlation using OR
- case() function for technique classification
- Combined index searches

## Status: ✅ Complete
