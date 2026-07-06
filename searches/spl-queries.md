# SPL Query Library — Splunk SIEM Lab

## Linux Authentication Queries

### Failed SSH Logins — Count:
index=linux_logs sourcetype=linux_secure Failed
| stats count as "Failed Logins"

### Failed SSH Logins — By Source IP:
index=linux_logs sourcetype=linux_secure Failed
| rex field=_raw "from (?<src_ip>\d+.\d+.\d+.\d+)"
| stats count by src_ip
| sort -count
| head 10

### Failed SSH Logins — By Username:
index=linux_logs sourcetype=linux_secure Failed
| rex field=_raw "for (?<username>\w+) from"
| stats count by username
| sort -count
| head 10

### Failed SSH Timeline:
index=linux_logs sourcetype=linux_secure Failed
| timechart count as "Failed Attempts"

### Successful SSH Logins:
index=linux_logs sourcetype=linux_secure Accepted
| stats count as "Successful Logins"

### Auth Events by Hour:
index=linux_logs sourcetype=linux_secure
| eval hour=strftime(_time, "%H")
| stats count by hour
| sort hour

### Sudo Usage:
index=linux_logs sourcetype=linux_secure sudo
| stats count by _raw
| head 20

## Windows Security Queries

### Total Security Events:
index=windows_logs sourcetype="WinEventLog:Security"
| stats count as "Security Events"

### Failed Windows Logins (Event 4625):
index=windows_logs sourcetype="WinEventLog:Security" EventCode=4625
| stats count as "Failed Logins"

### Successful Windows Logins (Event 4624):
index=windows_logs sourcetype="WinEventLog:Security" EventCode=4624
| stats count as "Successful Logins"

### Top Windows Event IDs:
index=windows_logs sourcetype="WinEventLog:Security"
| stats count by EventCode
| sort -count
| head 10

### Windows Log Sources Distribution:
index=windows_logs
| stats count by sourcetype

### Sysmon Process Creation:
index=windows_logs sourcetype="WinEventLog:Sysmon" EventCode=1
| stats count by process_name
| sort -count
| head 10

## Combined Queries

### All Failed Logins (Linux + Windows):
(index=linux_logs sourcetype=linux_secure Failed) OR
(index=windows_logs EventCode=4625)
| stats count by index

### Attack Timeline (Both Sources):
(index=linux_logs sourcetype=linux_secure Failed) OR
(index=windows_logs EventCode=4625)
| timechart count by index
