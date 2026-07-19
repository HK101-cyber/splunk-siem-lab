# Threat Hunt 3: Lateral Movement via SSH

## Hypothesis
An attacker is using SSH to move laterally between machines
in the lab network, pivoting from Kali into the Splunk server
using valid credentials.

## Hunt Date
2026-07-19

## Hunter
Hammad Khan - SOC Engineer

## SPL Queries Used

### Query 1 - All successful SSH logins:
index=linux_logs sourcetype=linux_secure Accepted
| rex field=_raw "for (?<username>\w+) from (?<src_ip>\d+.\d+.\d+.\d+)"
| table _time, username, src_ip

### Query 2 - Breakdown by source IP:
index=linux_logs sourcetype=linux_secure Accepted
| rex field=_raw "for (?<username>\w+) from (?<src_ip>\d+.\d+.\d+.\d+)"
| stats count by src_ip
| sort -count

## Findings

### Finding 1 - Baseline Admin Access
- Source IP: 192.168.56.1 (Windows Host PC)
- Successful logins: 13
- Username: hammad
- Classification: LEGITIMATE ADMIN ACCESS PATTERN

### Finding 2 - Lateral Movement Event Detected
- Source IP: 192.168.56.102 (Kali Linux)
- Successful logins: 1
- Username: hammad
- Timestamp: 2026-07-17 21:17:59
- Classification: CONFIRMED LATERAL MOVEMENT

### Finding 3 - Attack Correlation
- This successful login occurred shortly after the
  173-attempt brute force burst from the same Kali source
  (documented in Hunt 1), though ultimately used valid
  credentials rather than a cracked password, consistent
  with a simulated "attacker already possesses credentials"
  lateral movement scenario.

## Risk Assessment
- Severity: CRITICAL
- Impact: Attacker with valid creds gained direct server access
- Baseline established: 13 legitimate Windows-sourced logins
  vs 1 anomalous Kali-sourced login

## MITRE ATT&CK Mapping
- T1021.004 - Remote Services: SSH
- T1078 - Valid Accounts

## Detection Rule Validated
- Alert: Successful Login After Failed Attempts
- Status: TRIGGERED correctly, correlating prior failures
  with the subsequent successful session

## Recommendations
1. Whitelist SSH access to known admin source IPs only
2. Implement MFA for SSH sessions
3. Alert on any successful login from non-baseline IPs
4. Monitor for further activity from 192.168.56.102

## Conclusion
Hypothesis CONFIRMED. A single anomalous successful SSH login
from the attacker machine (192.168.56.102) was identified
against a clean baseline of 13 legitimate admin sessions from
the Windows host, demonstrating the value of source IP
baselining for lateral movement detection.
