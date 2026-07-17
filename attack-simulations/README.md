# Attack Simulations - Splunk SIEM Lab

## Summary

| # | Simulation | Machine | MITRE | Alert Status |
|---|-----------|---------|-------|---------------|
| 1 | SSH Brute Force | Kali -> Ubuntu | T1110 | TRIGGERED |
| 2 | Username Enumeration | Kali -> Ubuntu | T1110.001 | TRIGGERED |
| 3 | Lateral Movement | Kali -> Ubuntu | T1021.004 | TRIGGERED |
| 4 | Privilege Escalation | Ubuntu internal | T1548 | TRIGGERED |
| 5 | Reverse Shell Indicators | Ubuntu internal | T1059.004 | TRIGGERED |
| 6 | Log File Tampering | Ubuntu internal | T1070.002 | TRIGGERED |
| 7 | New Admin Account | Windows internal | T1136.001 | TRIGGERED |
| 8 | Scheduled Task + PowerShell + Log Clear | Windows internal | T1053.005, T1059.001, T1070.001 | TRIGGERED |

## Detection Rate: 10/10 Alerts Fired (100%)

## Attack Kill Chain Narrative
1. Reconnaissance and username enumeration (Sim 2)
2. Brute force credential attack (Sim 1)
3. Successful lateral movement into server (Sim 3)
4. Privilege escalation to root (Sim 4)
5. Post-exploitation reverse shell attempt (Sim 5)
6. Anti-forensic log tampering (Sim 6)
7. Pivot to Windows endpoint - backdoor account (Sim 7)
8. Windows persistence, execution, and log clearing (Sim 8)

This represents a complete, realistic attack lifecycle from
initial access through persistence and defense evasion,
detected end-to-end by the Splunk SIEM.
