# Windows Universal Forwarder Setup

## Version
Splunk Universal Forwarder 10.4.0

## Receiving Indexer
- Host: 192.168.56.101
- Port: 9997

## Log Sources Configured
| Source | Index | Sourcetype |
|--------|-------|------------|
| Security.evtx | windows_logs | WinEventLog:Security |
| System.evtx | windows_logs | WinEventLog:System |
| Application.evtx | windows_logs | WinEventLog:Application |
| Sysmon Operational | windows_logs | WinEventLog:Sysmon |

## Key Windows Event IDs Monitored
| Event ID | Description |
|----------|-------------|
| 4624 | Successful logon |
| 4625 | Failed logon |
| 4720 | User account created |
| 4732 | User added to group |
| 4698 | Scheduled task created |
| 4702 | Scheduled task updated |
| 7045 | New service installed |
| 1102 | Audit log cleared |
| 1 | Sysmon process creation |
| 3 | Sysmon network connection |
| 11 | Sysmon file created |

## Status
- Installed: ✅
- Service running: ✅
- Receiving indexer configured: ✅
- Windows logs flowing: ✅
- Sysmon logs flowing: ✅
