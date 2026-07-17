# Attack Simulation 6: Log File Tampering

## Objective
Simulate an attacker attempting to cover their tracks by
tampering with or deleting log files.

## Attack Machine
Ubuntu Splunk Server (internal)

## Attack Commands
```bash
logger "Suspicious command executed: truncate -s 0 /var/log/auth.log"
logger "Suspicious command executed: rm -rf /var/log/old_logs"
```

## MITRE ATT&CK Mapping
- Technique: T1070.002 - Clear Linux or Mac System Logs
- Tactic: Defense Evasion

## Detection Rule Triggered
- Alert: Log File Tampering
- SPL: search "truncate" OR "rm -rf /var/log"
- Status: TRIGGERED

## Result
CONFIRMED - Anti-forensic command patterns detected
