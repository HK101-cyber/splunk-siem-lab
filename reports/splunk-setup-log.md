# Splunk SIEM Lab — Setup Log

## Environment
- OS: Ubuntu Server 22.04 LTS
- IP: 192.168.56.101
- Splunk Version: 10.4.0
- Web UI Port: 8000
- Forwarder Port: 9997

## Indexes Created
| Index | Purpose |
|-------|---------|
| linux_logs | Linux syslog and auth logs |
| windows_logs | Windows Event Logs via forwarder |
| soc_alerts | Custom alert data |

## Log Sources Configured
| Source | Index | Sourcetype |
|--------|-------|------------|
| /var/log/auth.log | linux_logs | linux_secure |
| /var/log/syslog | linux_logs | syslog |
| Windows Event Logs | windows_logs | WinEventLog |

## Phase 1 — Installation ✅
- [x] Splunk 10.4.0 installed on Ubuntu
- [x] Web UI accessible at 192.168.56.101:8000
- [x] Port 8000 bound to 0.0.0.0
- [x] Boot start enabled
- [x] Firewall ports 8000 and 9997 opened

## Phase 2 — Log Ingestion ✅
- [x] linux_logs index created
- [x] windows_logs index created
- [x] soc_alerts index created
- [x] auth.log monitoring configured
- [x] syslog monitoring configured
- [x] 218+ events ingested from Linux
- [x] Windows Universal Forwarder pending

## Phase 3 — Dashboards (In Progress)
- [ ] Dashboard 1 — Linux Auth Analytics
- [ ] Dashboard 2 — Windows Security Analytics
- [ ] Dashboard 3 — Threat Timeline
- [ ] Dashboard 4 — User Behavior Analytics
- [ ] Dashboard 5 — SOC Operations

## Phase 4 — Detection Alerts (Pending)
- [ ] 10 SPL detection alerts

## Phase 5 — Attack Simulations (Pending)
- [ ] 8 attack simulations

## Phase 6 — Threat Hunts (Pending)
- [ ] 4 threat hunt reports

## Phase 3 — Windows Universal Forwarder ✅
- [x] Splunk Universal Forwarder installed on Windows
- [x] Receiving indexer set to 192.168.56.101:9997
- [x] Security, System, Application logs configured
- [x] Sysmon Operational logs configured
- [x] Port 9997 enabled on Splunk server
- [x] Windows logs flowing into windows_logs index
- [x] Screenshots taken and pushed
