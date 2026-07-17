# Attack Simulation 1: SSH Brute Force

## Objective
Simulate an external attacker attempting to crack SSH credentials
using an automated password-guessing tool.

## Attack Machine
Kali Linux (192.168.56.100) -> Ubuntu Splunk Server (192.168.56.101)

## Attack Command
```bash
hydra -l root -P /usr/share/wordlists/rockyou.txt 192.168.56.101 ssh -t 4 -V
```

## MITRE ATT&CK Mapping
- Technique: T1110 - Brute Force
- Tactic: Credential Access

## Detection Rule Triggered
- Alert: SSH Brute Force Detection
- SPL: index=linux_logs sourcetype=linux_secure Failed | stats count by src_ip | where count > 5
- Status: TRIGGERED

## Result
CONFIRMED - Alert fired within 5 minute scheduled window
