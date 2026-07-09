# Detection Rules — Splunk Alerts

## All 10 Alerts Deployed and Active

### Linux Alerts (6)
| # | Alert | SPL Trigger | MITRE | Severity |
|---|-------|-------------|-------|----------|
| 1 | SSH Brute Force Detection | Failed count > 5 by src_ip | T1110 | High |
| 2 | Privilege Escalation via Sudo | Sudo events > 10 | T1548 | High |
| 3 | Successful Login After Failed Attempts | failed>3 AND success>0 | T1110 | Critical |
| 9 | Reverse Shell Indicators | Pattern match nc/bash/tcp | T1059.004 | Critical |
| 10 | Log File Tampering | Pattern match truncate/rm | T1070.002 | Critical |

### Windows Alerts (5)
| # | Alert | SPL Trigger | MITRE | Severity |
|---|-------|-------------|-------|----------|
| 4 | Windows Failed Login Threshold | EventCode=4625 count>3 | T1110 | High |
| 5 | New Admin Account Created | EventCode=4720/4732 | T1136.001 | Critical |
| 6 | Scheduled Task Created | EventCode=4698 | T1053.005 | High |
| 7 | Event Log Cleared | EventCode=1102/104 | T1070.001 | Critical |
| 8 | PowerShell Process Execution | EventCode=4688 + powershell | T1059.001 | High |

## Schedule
All alerts run every 5 minutes via cron: */5 * * * *

## Known Limitation — Sysmon via Splunk Universal Forwarder

During deployment, Sysmon telemetry could not be ingested through
the Splunk Universal Forwarder due to a Windows Event Log channel
ACL restriction (errorCode=5, access denied) when subscribing to
the Microsoft-Windows-Sysmon/Operational channel.

Troubleshooting performed:
- Verified Sysmon itself was logging correctly (44,930 events
  confirmed directly in Windows Event Viewer)
- Verified inputs.conf configuration was correct
- Attempted channel ACL modification via wevtutil sl
- Attempted running forwarder service under different security context

Root cause: Windows restricts programmatic subscription to the
Sysmon Operational channel more strictly than to Security/System/
Application channels, even for services with elevated privileges,
without additional channel-level permission grants that require
further investigation.

Workaround implemented: Alert 8 (PowerShell Process Execution) was
rebuilt using Windows Security Event ID 4688 (Process Creation) with
command-line auditing enabled via auditpol and registry policy,
achieving equivalent detection coverage without relying on Sysmon.

Comparison note: This exact use case (Sysmon telemetry into a SIEM)
was successfully implemented in the ELK Stack lab (Project 1) using
Winlogbeat, which does not encounter this same ACL restriction.
This is documented as a valuable real-world comparison between
Splunk Universal Forwarder and Winlogbeat agent architectures.

## Status: 10/10 Alerts Active ✅
