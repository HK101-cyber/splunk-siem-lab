# Threat Hunt Reports - Splunk SIEM Lab

## All Hunts Completed

| # | Hunt Name | Key Finding | Result |
|---|-----------|-------------|--------|
| 1 | SSH Brute Force Patterns | 173 attempts from 192.168.56.102 targeting root | ✅ CONFIRMED |
| 2 | Privilege Escalation | 180 sudo events, 58% touching sensitive files | ✅ CONFIRMED |
| 3 | Lateral Movement | 1 anomalous login vs 13-session baseline | ✅ CONFIRMED |
| 4 | Persistence & Anti-Forensics | Scheduled task + log clearing detected | ✅ CONFIRMED |

## Complete Attack Narrative (Reconstructed via Threat Hunting)

1. Attacker (192.168.56.102) launches 173-attempt SSH brute
   force against root account (Hunt 1)
2. Attacker successfully authenticates as "hammad" using valid
   credentials, standing out against a clean 13-session
   Windows-sourced baseline (Hunt 3)
3. Attacker escalates privileges via sudo, accessing sensitive
   files including /etc/shadow (Hunt 2)
4. Attacker establishes persistence via a Windows scheduled
   task (Hunt 4)
5. Attacker attempts to cover tracks by clearing the Windows
   Application event log (Hunt 4)

## Methodology
Each hunt followed hypothesis-driven investigation:
1. Form hypothesis based on MITRE ATT&CK technique
2. Write and refine SPL queries
3. Analyze results for confirmation or refutation
4. Document findings with exact evidence
5. Correlate with deployed detection rules
6. Provide actionable recommendations

## Total Hunts: 4
## Confirmed Threats: 4/4 (100%)
## Detection Rules Validated: 6 (across all hunts)
