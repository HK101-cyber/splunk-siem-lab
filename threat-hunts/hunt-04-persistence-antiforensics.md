# Threat Hunt 4: Windows Persistence and Anti-Forensics Activity

## Hypothesis
An attacker with access to the Windows endpoint is establishing
persistence mechanisms and attempting to cover their tracks by
clearing system event logs.

## Hunt Date
2026-07-19

## Hunter
Hammad Khan - SOC Engineer

## SPL Queries Used

### Query 1 - Persistence and log-clearing events:
index=windows_logs (EventCode=4698 OR EventCode=1102 OR EventCode=104)
| stats count by EventCode

### Query 2 - Full timeline detail:
index=windows_logs (EventCode=4698 OR EventCode=1102 OR EventCode=104)
| table _time, EventCode, sourcetype
| sort _time

## Findings

### Finding 1 - Scheduled Task Persistence
- Event ID: 4698 (Scheduled Task Created)
- Timestamp: 2026-07-16 20:44:56
- Source: WinEventLog:Security
- Classification: CONFIRMED PERSISTENCE MECHANISM

### Finding 2 - Log Clearing / Anti-Forensics
- Event ID: 104 (Event Log Cleared - Application log)
- Timestamp: 2026-07-17 21:29:06
- Source: WinEventLog:System
- Classification: CONFIRMED ANTI-FORENSIC ACTIVITY

### Finding 3 - Attack Sequence Timeline
- Day 1 (Jul 16, 20:44): Attacker establishes persistence
  via scheduled task
- Day 2 (Jul 17, 21:29): Attacker clears logs to cover tracks,
  occurring in the same operational window as the SSH brute
  force and lateral movement activity documented in Hunts 1-3

## Risk Assessment
- Severity: CRITICAL
- Impact: Persistent access maintained + evidence destruction
  in progress
- Combined with Hunts 1-3, this represents a complete attack
  lifecycle: brute force -> lateral movement -> privilege
  escalation -> persistence -> anti-forensics

## MITRE ATT&CK Mapping
- T1053.005 - Scheduled Task/Job: Scheduled Task
- T1070.001 - Indicator Removal: Clear Windows Event Logs

## Detection Rules Validated
- Alert: Scheduled Task Created - TRIGGERED
- Alert: Event Log Cleared - TRIGGERED

## Recommendations
1. Forward logs to a centralized, immutable log server in
   near-real-time to reduce the impact of local log clearing
2. Alert immediately on any Application/System/Security log
   clear event (EventCode 104/1102) regardless of time of day
3. Review all scheduled tasks weekly for unauthorized entries
4. Enable Windows Event Log size increase + backup to reduce
   successful clearing window

## Conclusion
Hypothesis CONFIRMED. Both persistence (scheduled task) and
anti-forensic (log clearing) activity were detected and
correctly triggered their respective alerts, completing the
full attack kill chain visibility across all four threat hunts.
