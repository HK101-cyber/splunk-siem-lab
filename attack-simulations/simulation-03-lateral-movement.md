# Attack Simulation 3: Lateral Movement via SSH

## Objective
Simulate an attacker successfully pivoting from Kali into the
Splunk server using valid credentials.

## Attack Machine
Kali Linux (192.168.56.100) -> Ubuntu Splunk Server (192.168.56.101)

## Attack Commands
```bash
ssh hammad@192.168.56.101
ifconfig
cat /etc/hosts
whoami
```

## MITRE ATT&CK Mapping
- Technique: T1021.004 - Remote Services: SSH
- Tactic: Lateral Movement

## Detection Rule Triggered
- Alert: Successful Login After Failed Attempts
- SPL: correlates failed>3 AND success>0 by src_ip
- Status: TRIGGERED

## Result
CONFIRMED - Successful login following prior failed attempts detected
