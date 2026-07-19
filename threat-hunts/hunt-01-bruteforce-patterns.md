# Threat Hunt 1: SSH Brute Force Pattern Analysis

## Hypothesis
Threat actors are systematically attempting to brute force
SSH credentials against our Splunk-monitored Ubuntu server
using automated tools from an external/internal source.

## Hunt Date
2026-07-19

## Hunter
Hammad Khan - SOC Engineer

## Tools Used
- Splunk Search and Reporting
- SPL queries against linux_logs index

## SPL Queries Used

### Query 1 - Extract failed login details:
index=linux_logs sourcetype=linux_secure Failed
| rex field=_raw "from (?<src_ip>\d+.\d+.\d+.\d+)"
| rex field=_raw "for (?<username>\w+) from"
| table _time, src_ip, username

### Query 2 - Aggregate by source and target:
index=linux_logs sourcetype=linux_secure Failed
| rex field=_raw "from (?<src_ip>\d+.\d+.\d+.\d+)"
| rex field=_raw "for (?<username>\w+) from"
| stats count by src_ip, username
| sort -count

## Findings

### Finding 1 - Confirmed Brute Force Attack
- Source IP: 192.168.56.102 (Kali Linux)
- Target username: root
- Total attempts: 173
- Attack window: 21:15:30 to 21:15:42 (12 seconds)
- Classification: CONFIRMED AUTOMATED BRUTE FORCE

### Finding 2 - Attack Speed Analysis
- 173 attempts in 12 seconds indicates automated tooling
- Consistent with Hydra password-list attack pattern
- No manual login attempt would achieve this rate

### Finding 3 - Single Target Focus
- 100% of attempts targeted the "root" account
- No username diversity observed in this attack window
- Suggests attacker had prior intelligence root was likely valid

## Risk Assessment
- Severity: HIGH
- Likelihood of success: LOW (strong password policy in place)
- Impact if successful: CRITICAL (root access to SIEM server)
- Current status: BLOCKED - no successful root login recorded

## MITRE ATT&CK Mapping
- T1110 - Brute Force
- T1110.001 - Password Guessing

## Detection Rule Validated
- Alert: SSH Brute Force Detection
- Threshold: count > 5 by src_ip
- Result: 173 >> 5, alert correctly triggered

## Recommendations
1. Disable direct root SSH login
2. Implement Fail2ban for automatic IP blocking
3. Enforce SSH key-based authentication only
4. Add rate limiting at network/firewall level

## Conclusion
Hypothesis CONFIRMED. A clear, automated brute force attack
was detected, fully attributed to source IP 192.168.56.102
targeting the root account. Detection rule performed as designed.
