# Attack Simulation 8: Scheduled Task, PowerShell Abuse, Log Clearing

## Objective
Simulate a multi-stage Windows post-exploitation sequence:
persistence via scheduled task, encoded PowerShell execution,
and anti-forensic log clearing.

## Attack Machine
Windows Host PC (post-compromise, internal)

## Attack Commands
```powershell
schtasks /create /tn "WindowsUpdateCheck" /tr "cmd.exe" /sc daily /st 09:00
powershell -EncodedCommand "aQBwAGMAbwBuAGYAaQBnAA=="
wevtutil cl Application
```

## MITRE ATT&CK Mapping
- T1053.005 - Scheduled Task (Persistence)
- T1059.001 - PowerShell (Execution)
- T1070.001 - Clear Windows Event Logs (Defense Evasion)

## Detection Rules Triggered
- Alert: Scheduled Task Created (EventCode=4698)
- Alert: PowerShell Process Execution (EventCode=4688)
- Alert: Event Log Cleared (EventCode=1102/104)
- Status: ALL TRIGGERED

## Result
CONFIRMED - Full attack chain detected across 3 separate alerts
