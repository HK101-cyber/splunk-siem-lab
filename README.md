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
| Windows Event Logs | Universal Forwarder | windows_logs | 9,743+ |
| Windows Sysmon | Universal Forwarder | windows_logs | Active |

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
| 3 | Threat Timeline | 6 | ⬜ Pending |
| 4 | User Behavior Analytics | 6 | ⬜ Pending |
| 5 | SOC Operations | 6 | ⬜ Pending |

---

## Detection Alerts

| # | Alert | MITRE | Severity | Status |
|---|-------|-------|----------|--------|
| 1 | SSH Password Spray | T1110.003 | High | ⬜ Pending |
| 2 | Linux Reverse Shell | T1059.004 | Critical | ⬜ Pending |
| 3 | Cron Job Modified | T1053.003 | High | ⬜ Pending |
| 4 | Log File Cleared | T1070.002 | High | ⬜ Pending |
| 5 | Sudo to Root | T1548.003 | High | ⬜ Pending |
| 6 | New Admin Account | T1136.001 | Critical | ⬜ Pending |
| 7 | Scheduled Task Created | T1053.005 | High | ⬜ Pending |
| 8 | Event Log Cleared | T1070.001 | High | ⬜ Pending |
| 9 | Encoded PowerShell | T1059.001 | High | ⬜ Pending |
| 10 | Login After Failures | T1110 | Medium | ⬜ Pending |

---

## Attack Simulations

| # | Attack | Tool | MITRE | Status |
|---|--------|------|-------|--------|
| 1 | SSH Password Spray | Hydra | T1110.003 | ⬜ Pending |
| 2 | Reverse Shell | Netcat | T1059.004 | ⬜ Pending |
| 3 | Cron Persistence | crontab | T1053.003 | ⬜ Pending |
| 4 | Log Tampering | truncate | T1070.002 | ⬜ Pending |
| 5 | New Admin Account | net user | T1136.001 | ⬜ Pending |
| 6 | Scheduled Task | schtasks | T1053.005 | ⬜ Pending |
| 7 | Event Log Clearing | wevtutil | T1070.001 | ⬜ Pending |
| 8 | Encoded PowerShell | PowerShell | T1059.001 | ⬜ Pending |

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
│   └── README.md
├── dashboards/
│   ├── linux-auth-dashboard.xml
│   └── dashboard1-linux-auth.md
├── attack-simulations/
│   ├── screenshots/
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
| stats count by src_ip user
| sort -count

# Top Processes (Sysmon)
index=windows_logs source="WinEventLog:Sysmon"
EventCode=1
| stats count by process_name
| sort -count
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

---

## Related Projects
- [soc-home-lab](https://github.com/HK101-cyber/soc-home-lab) — ELK SIEM Lab
- [splunk-siem-lab](https://github.com/HK101-cyber/splunk-siem-lab) — This project

---

*Part of a complete cybersecurity portfolio built command
by command in a real lab environment.*

**Connect:** [LinkedIn](https://linkedin.com/in/hammad-khan101)
**GitHub:** [github.com/HK101-cyber](https://github.com/HK101-cyber)
