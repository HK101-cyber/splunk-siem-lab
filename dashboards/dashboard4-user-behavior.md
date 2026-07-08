# Dashboard 4 — User Behavior Analytics

## Purpose
Monitor user activity patterns across Linux and Windows
to establish behavioral baselines and detect anomalies.

## Panels and SPL

### Panel 1 — User Login Activity:
index=linux_logs sourcetype=linux_secure Accepted
| rex field=_raw "for (?<username>\w+) from"
| stats count by username
| sort -count

### Panel 2 — Sudo Usage by User:
index=linux_logs sourcetype=linux_secure sudo
| rex field=_raw "(?i)(?<sudo_user>hammad|root)"
| stats count by sudo_user
| head 10

### Panel 3 — Windows User Activity:
index=* EventCode=4624
| eval Windows_User = mvindex(Account_Name, 1)
| search Windows_User!="*$" Windows_User!="SYSTEM"
| stats count by Windows_User
| sort -count
| head 10

### Panel 4 — Login Time Patterns:
index=linux_logs sourcetype=linux_secure Accepted
| eval hour=strftime(_time, "%H")
| stats count by hour
| sort hour

### Panel 5 — Unique Users with Failed Logins:
index=windows_logs EventCode=4625 OR EventID=4625
| eval Failed_User = coalesce(TargetUserName, Account_Name, user)
| search Failed_User!="*$" Failed_User!="SYSTEM"
| stats dc(Failed_User) as "Unique Failed Users"

### Panel 6 — Command Execution Volume:
index=linux_logs sourcetype=linux_secure
| timechart count as "Total Commands"

## Advanced SPL Concepts Demonstrated
- mvindex() — extracts value from multivalue field
- coalesce() — returns first non-null value from multiple fields
- Filtering system accounts with != "*$" and != "SYSTEM"
- Case-insensitive regex with (?i) flag
- dc() — distinct count function

## Status: ✅ Complete
