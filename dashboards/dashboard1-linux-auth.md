# Dashboard 1 — Linux Authentication Analytics

## Purpose
Monitor all SSH and authentication activity on
Linux endpoints to detect brute force attacks,
unauthorized access attempts, and suspicious
login patterns.

## Panels

| Panel | Type | Purpose |
|-------|------|---------|
| 1 — Total Failed Logins | Single Value | Overall failed login count |
| 2 — Failed Login Timeline | Line Chart | Attack timeline visualization |
| 3 — Top Source IPs | Bar Chart | Top attacking IP addresses |
| 4 — Top Targeted Users | Bar Chart | Most targeted usernames |
| 5 — Successful Logins | Single Value | Successful SSH session count |
| 6 — Events by Hour | Column Chart | Hourly authentication pattern |

## SPL Queries Used

### Panel 1 — Total Failed Logins:
index=linux_logs sourcetype=linux_secure Failed
| stats count as "Failed Logins"

### Panel 2 — Timeline:
index=linux_logs sourcetype=linux_secure Failed
| timechart count as "Failed Attempts"

### Panel 3 — Top Source IPs:
index=linux_logs sourcetype=linux_secure Failed
| rex field=_raw "from (?<src_ip>\d+.\d+.\d+.\d+)"
| stats count by src_ip
| sort -count
| head 10

### Panel 4 — Top Usernames:
index=linux_logs sourcetype=linux_secure Failed
| rex field=_raw "for (?<username>\w+) from"
| stats count by username
| sort -count
| head 10

### Panel 5 — Successful Logins:
index=linux_logs sourcetype=linux_secure Accepted
| stats count as "Successful Logins"

### Panel 6 — Events by Hour:
index=linux_logs sourcetype=linux_secure
| eval hour=strftime(_time, "%H")
| stats count by hour
| sort hour

## Key SPL Concepts Demonstrated
- stats count — counts total matching events
- timechart — creates time based visualization
- rex — extracts custom fields using regex
- eval strftime — formats timestamp into hour
- sort -count — sorts results descending
- head 10 — limits output to top 10

## Attack Data Generated
- Tool used: Hydra from Kali Linux
- Target: Ubuntu SSH (192.168.56.101)
- Result: Hundreds of failed login events
- All visible in dashboard panels

## Status: ✅ Complete
