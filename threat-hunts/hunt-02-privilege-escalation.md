# Threat Hunt 2: Privilege Escalation via Sudo Abuse

## Hypothesis
An attacker who gained initial low-privilege access is using
sudo to escalate privileges and access sensitive system files
such as /etc/shadow and /etc/passwd.

## Hunt Date
2026-07-19

## Hunter
Hammad Khan - SOC Engineer

## SPL Queries Used

### Query 1 - All sudo activity:
index=linux_logs sourcetype=linux_secure sudo
| stats count

### Query 2 - Sudo activity by user:
index=linux_logs sourcetype=linux_secure sudo
| rex field=_raw "(?i)(?<sudo_user>hammad|root)"
| stats count by sudo_user

### Query 3 - Sensitive file access correlation:
index=linux_logs sourcetype=linux_secure sudo (shadow OR passwd OR root)
| stats count

## Findings

### Finding 1 - High Volume Sudo Activity
- Total sudo events: 180
- User "root": 94 events
- User "hammad": 52 events
- Classification: ELEVATED PRIVILEGE ACTIVITY

### Finding 2 - Sensitive File Access Confirmed
- 104 of 180 events (58%) involved shadow, passwd, or root paths
- Directly correlates with privilege escalation simulation
- Includes access to /etc/shadow (password hash file)

### Finding 3 - User Behavior Pattern
- "hammad" user account escalating to root via sudo
- Consistent with attacker gaining foothold as low-priv user
  then escalating

## Risk Assessment
- Severity: HIGH
- Technique: T1548 - Abuse Elevation Control Mechanism
- Impact: Potential credential theft, full system compromise

## MITRE ATT&CK Mapping
- T1548 - Abuse Elevation Control Mechanism
- T1003 - OS Credential Dumping (attempted via shadow access)
- T1082 - System Information Discovery

## Detection Rule Validated
- Alert: Privilege Escalation via Sudo
- Status: TRIGGERED correctly on this activity

## Recommendations
1. Restrict sudo to specific whitelisted commands only
2. Enable auditd for complete sudo command-line logging
3. Alert specifically on /etc/shadow access attempts
4. Implement least-privilege access reviews

## Conclusion
Hypothesis CONFIRMED. 180 sudo events detected, with 58%
directly touching sensitive credential files. Detection rule
validated as effective against this technique.
