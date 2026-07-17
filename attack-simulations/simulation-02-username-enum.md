# Attack Simulation 2: Username Enumeration

## Objective
Simulate an attacker probing for valid usernames before a targeted
credential attack.

## Attack Machine
Kali Linux (192.168.56.100) -> Ubuntu Splunk Server (192.168.56.101)

## Attack Command
```bash
for user in root admin administrator test oracle postgres guest; do
  sshpass -p wrongpass ssh -o StrictHostKeyChecking=no -o ConnectTimeout=3 $user@192.168.56.101
done
```

## MITRE ATT&CK Mapping
- Technique: T1110.001 - Password Guessing
- Tactic: Credential Access

## Detection Rule Triggered
- Alert: SSH Brute Force Detection (same rule catches enumeration)
- Status: TRIGGERED

## Result
CONFIRMED - Multiple failed logins across different usernames detected
