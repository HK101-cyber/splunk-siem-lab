# Attack Simulation 4: Privilege Escalation via Sudo

## Objective
Simulate an attacker who has gained low-privilege access
attempting to escalate to root and access sensitive files.

## Attack Machine
Ubuntu Splunk Server (post-compromise, internal)

## Attack Commands
```bash
sudo cat /etc/shadow
sudo cat /etc/passwd
sudo ls /root
sudo find / -perm -4000 2>/dev/null
sudo netstat -tlnp
```

## MITRE ATT&CK Mapping
- Technique: T1548 - Abuse Elevation Control Mechanism
- Tactic: Privilege Escalation

## Detection Rule Triggered
- Alert: Privilege Escalation via Sudo
- SPL: index=linux_logs sourcetype=linux_secure sudo | stats count
- Status: TRIGGERED

## Result
CONFIRMED - Sudo activity accessing sensitive files detected
