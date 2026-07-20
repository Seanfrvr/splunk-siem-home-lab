# Splunk SIEM Home Lab — Log Ingestion & Threat Hunting

## Objective
Built a fully functional SIEM environment from scratch on Ubuntu to move beyond guided labs and understand log pipelines end-to-end — from ingestion, to indexing, to real-time threat detection.

## Architecture & Setup
- Deployed **Splunk Enterprise 10.2.3** on Ubuntu Linux as the indexer/search head
- Configured a **receiving pipeline on port 9997** so a Universal Forwarder could ship logs to the indexer
- Shipped live **syslog** and **auth.log** data into a searchable index, reaching **11,475 events** — real system logs, not sample data

## Attack Simulation
Simulated an SSH brute-force attack against the host to generate realistic failed-login activity for detection testing.

## Detection
Used SPL (Search Processing Language) to isolate every failed login attempt in real time:

```spl
index=main source="/var/log/auth.log" "Failed password"
```

This surfaced each failed SSH attempt as it happened — including invalid users, source IPs, and ports — the exact detection workflow SOC analysts run daily.

## Findings
- Identified repeated failed login attempts for invalid users from a consistent source, consistent with brute-force behavior
- Confirmed the full pipeline (forwarder → receiver → index → search) was reliable enough to support real-time triage

## Skills Demonstrated
- SIEM deployment and pipeline configuration
- Log ingestion and index management
- SPL query writing for threat detection
- SSH brute-force attack pattern recognition

## Screenshots

**Splunk Enterprise deployed and running on Ubuntu**
![Splunk Enterprise dashboard](screenshots/1-splunk-dashboard.png)

**Receiving pipeline configured (port 9997)**
![Receiving pipeline](screenshots/2-receiving-pipeline.png)

**Live logs flowing into Splunk (11,000+ events)**
![Live logs](screenshots/3-live-logs.png)

**Threat detected — failed SSH logins isolated via SPL**
![Failed SSH logins](screenshots/4-failed-ssh-logins.png)
