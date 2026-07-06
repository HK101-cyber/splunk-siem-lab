# Dashboard 2 — Windows Security Analytics

## Purpose
Monitor Windows security events across the enterprise
environment including login activity, account changes,
and system events.

## Panels

| Panel | Type | SPL Query | Purpose |
|-------|------|-----------|---------|
| 1 | Single Value | EventCode=Security \| stats count | Total security events |
| 2 | Line Chart | timechart count | Security timeline |
| 3 | Single Value | EventCode=4625 \| stats count | Failed logins |
| 4 | Bar Chart | stats count by EventCode | Top event IDs |
| 5 | Single Value | EventCode=4624 \| stats count | Successful logins |
| 6 | Pie Chart | stats count by sourcetype | Log sources |

## Key Windows Event IDs
| Event ID | Description |
|----------|-------------|
| 4624 | Successful logon |
| 4625 | Failed logon |
| 4720 | User account created |
| 4732 | User added to group |
| 4698 | Scheduled task created |
| 1102 | Audit log cleared |

## Data Generated
- Failed logins: 21 (Event 4625)
- Successful logins: Active
- Total security events: 6,250+

## Status: ✅ Complete
