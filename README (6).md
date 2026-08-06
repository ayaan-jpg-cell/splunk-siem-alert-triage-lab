# Splunk SIEM Monitoring & Alert Triage Lab

## Objective
Replicate a production SOC SIEM pipeline in an environment with no centralized log visibility — from log forwarding through alert triage — to practice the monitoring and triage duties a SOC Tier I analyst performs daily.

## Environment
- SIEM: Splunk (Universal Forwarder + Indexer)
- Log Source: Ubuntu VM
- Indexer/Search Head: Windows Enterprise server
- Query Language: SPL (Search Processing Language)

## Steps Performed

### 1. Deploy Splunk Universal Forwarder
Installed the Splunk Universal Forwarder on the Ubuntu VM to collect and forward local logs (system, auth, application) to the central indexer.
```bash
sudo dpkg -i splunkforwarder-*.deb
sudo /opt/splunkforwarder/bin/splunk start --accept-license
```

### 2. Configure Log Forwarding
Set the forwarder's `outputs.conf` to point to the receiving indexer on the Windows Enterprise server, and configured `inputs.conf` to monitor relevant log paths (e.g. `/var/log/auth.log`, `/var/log/syslog`).

### 3. Configure the Receiving Indexer
On the Windows Enterprise server, enabled a receiving port in Splunk and confirmed the forwarder's connection was established and actively streaming data.

### 4. Verify Real-Time Log Ingestion
Confirmed logs were arriving in real time by searching the index:
```spl
index=* sourcetype=linux_secure
| head 20
```

### 5. Write SPL Queries to Surface Anomalies
Built SPL searches to flag suspicious activity patterns, for example repeated failed logins:
```spl
index=* sourcetype=linux_secure "Failed password"
| stats count by src_ip
| where count > 5
```
and unusual process/service activity surfaced through similar aggregation searches.

### 6. Detect and Triage Simulated Security Events
Generated simulated suspicious activity on the Ubuntu host, confirmed the events were ingested and matched by the SPL queries above, and walked through the triage steps a Tier I analyst would take: identify the alert, pull the relevant context (source IP, account, timestamp), assess severity, and determine escalation vs. close-out.

## Outcome
A working end-to-end SIEM pipeline — from log forwarding to real-time ingestion to SPL-based anomaly detection — that successfully surfaced and triaged simulated security events, directly replicating the SIEM monitoring, alert triage, and IDS/IPS notification duties of a SOC Tier I analyst.

## Skills Demonstrated
`Splunk` · `SIEM` · `SPL` · `Log Ingestion` · `Alert Triage` · `IOC Identification` · `Linux (Ubuntu)` · `Windows Server`
