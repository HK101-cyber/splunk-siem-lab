# Splunk SIEM Lab — Enterprise Security Monitoring

![Status](https://img.shields.io/badge/Status-Active%20Build-green)
![Splunk](https://img.shields.io/badge/Splunk-10.4.0-orange)
![Platform](https://img.shields.io/badge/Platform-Ubuntu%2022.04-blue)

> A complete enterprise Splunk SIEM lab with Linux and Windows
> log ingestion, SPL detection rules, attack simulations,
> and threat hunting reports.

---

## Lab Details

| Field | Details |
|-------|---------|
| Author | Hammad Khan |
| Start Date | June 2026 |
| Splunk Version | Enterprise 10.4.0 |
| Status | Active Build |
| GitHub | github.com/HK101-cyber |

---

## Lab Architecture

| Component | Details |
|-----------|---------|
| SIEM Server | Ubuntu Server 22.04 (192.168.56.101) |
| Attacker | Kali Linux (192.168.56.100) |
| Windows Host | Windows 10/11 |
| Splunk Web | http://192.168.56.101:8000 |
| Forwarder Port | 9997 |

---

## Data Sources

| Source | Agent | Index | Events |
|--------|-------|-------|--------|
| Ubuntu auth.log | Splunk Monitor | linux_logs | 753+ |
| Ubuntu syslog | Splunk Monitor | linux_logs | 22,212+ |
| Windows Security/System/App | Universal Forwarder | windows_logs | 9,743+ |
| Windows Sysmon | Local Event Viewer | N/A | 44,930 (see limitation note) |

**Note:** Sysmon telemetry is fully operational (44,930 events)
but not ingested via Splunk Universal Forwarder due to a
Windows Event Log channel ACL restriction. Full details in
`detection-rules/README.md`. This same telemetry source is
successfully ingested in the companion ELK lab via Winlogbeat.

---

## Indexes Created

| Index | Purpose |
|-------|---------|
| linux_logs | All Linux log sources |
| windows_logs | All Windows Event Logs |
| soc_alerts | Custom alert data |

---

## Dashboards Built

| # | Dashboard | Panels | Status |
|---|-----------|--------|--------|
| 1 | Linux Authentication Analytics | 6 | ✅ Complete |
| 2 | Windows Security Analytics | 6 | ✅ Complete |
| 3 | Threat Timeline | 6 | ✅ Complete |
| 4 | User Behavior Analytics | 6 | ✅ Complete |
| 5 | SOC Operations | 6 | ✅ Complete |

**Total: 5 dashboards, 30 panels**

---

## Detection Alerts — 10/10 Active

| # | Alert | MITRE | Severity | Status |
|---|-------|-------|----------|--------|
| 1 | SSH Brute Force Detection | T1110 | High | ✅ Active |
| 2 | Privilege Escalation via Sudo | T1548 | High | ✅ Active |
| 3 | Successful Login After Failed Attempts | T1110 | Critical | ✅ Active |
| 4 | Windows Failed Login Threshold | T1110 | High | ✅ Active |
| 5 | New Admin Account Created | T1136.001 | Critical | ✅ Active |
| 6 | Scheduled Task Created | T1053.005 | High | ✅ Active |
| 7 | Event Log Cleared | T1070.001 | Critical | ✅ Active |
| 8 | PowerShell Process Execution | T1059.001 | High | ✅ Active |
| 9 | Reverse Shell Indicators | T1059.004 | Critical | ✅ Active |
| 10 | Log File Tampering | T1070.002 | Critical | ✅ Active |

All alerts run on schedule: `*/5 * * * *`

---

## Attack Simulations

| # | Attack | Tool | MITRE | Status |
|---|--------|------|-------|--------|
| 1 | SSH Brute Force | Hydra | T1110 | ✅ Data Generated |
| 2 | Reverse Shell Indicators | logger/simulated | T1059.004 | ✅ Data Generated |
| 3 | Log Tampering | logger/simulated | T1070.002 | ✅ Data Generated |
| 4 | Privilege Escalation | sudo commands | T1548 | ✅ Data Generated |
| 5 | New Admin Account (Windows) | net user | T1136.001 | ✅ Confirmed Formal Sim |
| 6 | Scheduled Task (Windows) | schtasks | T1053.005 | ✅ Data Generated |
| 7 | Event Log Clearing (Windows) | wevtutil | T1070.001 | ✅ Data Generated |
| 8 | PowerShell Execution (Windows) | PowerShell -EncodedCommand | T1059.001 | ✅ Data Generated |

**Note:** Data has been generated for alert testing purposes.
Formal simulation write-ups with full evidence chains pending
in `attack-simulations/` folder.

---

## Threat Hunts

| # | Hunt | Status |
|---|------|--------|
| 1 | Password Spray vs Brute Force | ⬜ Pending |
| 2 | Persistence Mechanisms | ⬜ Pending |
| 3 | Anti-Forensics Activity | ⬜ Pending |
| 4 | Lateral Movement Kill Chain | ⬜ Pending |

---

## Repository Structure
splunk-siem-lab/
├── configs/
│   ├── inputs.conf
│   ├── web.conf
│   ├── server.conf
│   └── windows-forwarder-setup.md
├── searches/
│   └── spl-queries.md
├── detection-rules/
│   └── README.md          (all 10 alerts + Sysmon limitation)
├── dashboards/
│   ├── linux-auth-dashboard.xml
│   ├── Windows Security Analytics.xml
│   ├── Threat Timeline.xml
│   ├── User Behavior Analytics.xml
│   ├── SOC Operations Dashboard.xml
│   └── dashboard1-5 documentation (.md)
├── attack-simulations/
│   └── README.md
├── threat-hunts/
│   └── README.md
├── reports/
│   └── splunk-setup-log.md
└── screenshots/

---

## ELK vs Splunk Comparison

| Feature | ELK Stack | Splunk |
|---------|-----------|--------|
| Cost | Free | Free Trial |
| Query Language | KQL | SPL |
| Setup Complexity | Complex | Easier |
| Enterprise Adoption | Growing | Dominant |
| Dashboards | Kibana | Splunk Web |
| Agents | Filebeat | Universal Forwarder |
| Sysmon Ingestion | ✅ Works (Winlogbeat) | ⚠️ ACL restriction (documented) |
| Learning Curve | Moderate | Moderate |

---

## Key SPL Searches

```spl
# Failed SSH Logins
index=linux_logs sourcetype=linux_secure Failed
| stats count by src_ip
| sort -count

# Authentication Timeline
index=linux_logs sourcetype=linux_secure
| timechart count by action

# Windows Failed Logins
index=windows_logs EventCode=4625
| stats count

# PowerShell Execution (Security log workaround)
index=windows_logs sourcetype="WinEventLog:Security" EventCode=4688
| search "*powershell*"
| stats count
```

---

## Lab Build Timeline

| Date | Milestone |
|------|-----------|
| Jun 15, 2026 | Splunk 10.4.0 installed on Ubuntu |
| Jun 15, 2026 | linux_logs index created, auth.log monitored |
| Jun 20, 2026 | Web UI fixed, accessible at port 8000 |
| Jun 20, 2026 | Windows Universal Forwarder installed |
| Jun 20, 2026 | windows_logs index receiving Windows events |
| Jul 2, 2026 | Dashboard 1 — Linux Auth Analytics complete |
| Jul 5, 2026 | Dashboards 2-5 complete — all 30 panels live |
| Jul 8, 2026 | All 10 detection alerts deployed and active |
| Jul 9, 2026 | Sysmon ACL limitation identified, documented, workaround built |

---

## Related Projects
- [soc-home-lab](https://github.com/HK101-cyber/soc-home-lab) — ELK SIEM Lab
- [splunk-siem-lab](https://github.com/HK101-cyber/splunk-siem-lab) — This project

---

*Part of a complete cybersecurity portfolio built command
by command in a real lab environment.*

**Connect:** [LinkedIn](https://linkedin.com/in/hammad-khan101)
**GitHub:** [github.com/HK101-cyber](https://github.com/HK101-cyber)
