# 🛡️ Splunk SIEM Home Lab – Log Ingestion & Threat Hunting

## 📌 Project Overview
Built a fully functional SIEM environment from scratch on Ubuntu Linux to move beyond guided labs and understand log pipelines end-to-end — from ingestion and indexing to real-time threat detection.

---

## 🏗️ 1. Architecture & Log Forwarding Configuration

<p align="center">
  <img src="./images/siem_1_architecture_forwarder.png" alt="Splunk Universal Forwarder Configuration">
</p>
<p align="center"><em>Figure 1: Splunk Enterprise receiver configuration listening on TCP Port 9997.</em></p>

### 📄 Infrastructure Breakdown
* **SIEM Core:** Deployed **Splunk Enterprise 10.2.3** on Ubuntu Linux as the central indexer and search head.
* **Log Ingestion:** Configured a native receiving pipeline on **port 9997** to collect streamed endpoint data.
* **Telemetry Sources:** Shipped live system telemetry including `syslog` and `/var/log/auth.log` into a searchable index.

---

## 📥 2. Telemetry Ingestion & Volume Verification

<p align="center">
  <img src="./images/siem_2_ingestion_verification.png" alt="Splunk Ingestion Verification Query">
</p>
<p align="center"><em>Figure 2: Statistical verification in Splunk confirming active indexed event counts.</em></p>

### 📄 Ingestion Analysis
* **Live Telemetry:** Successfully indexed **11,475 real system events** (live host activity, not sample/pre-canned data).
* **Data Integrity:** Verified sourcetype parsing across system and authentication logs to ensure timestamps and host headers were correctly indexed.

---

## ⚡ 3. Attack Simulation & Threat Detection

<p align="center">
  <img src="./images/siem_3_attack_detection.png" alt="SSH Brute Force Detection Query">
</p>
<p align="center"><em>Figure 3: Targeted SPL query isolating SSH brute-force patterns across authentication logs.</em></p>

### 📄 Detection Logic & Attack Scenario
* **Attack Simulation:** Executed an SSH brute-force attack against the host to generate realistic failed-login activity for detection validation.
* **SPL Query Construction:** Built real-time search queries targeting authentication failure signatures (`Failed password` / `invalid user`).

```spl
index=main sourcetype=syslog "Failed password" OR "invalid user"
| stats count by src_ip, user, host
| sort - count
