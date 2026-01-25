# Splunk SOC Home Lab

+ Overview
I built this Splunk SOC home lab to get hands-on experience with how a SIEM actually operates in a real SOC-style environment. The goal wasn’t just to install Splunk and ingest logs, but to understand how SIEMs behave when you have to manage storage, services, data flow, and troubleshooting issues that come up in practice.

This lab focuses on **defensive security**, **SIEM operations**, and **incident investigation**, rather than offensive exploitation or red-team tooling.


+ What This Lab Includes

- A Linux-based Splunk Enterprise SIEM
- A Windows endpoint generating realistic security telemetry
- Windows Security Event Logs and Sysmon data
- Proper index separation and log routing
- Real-world troubleshooting and operational fixes

The environment is designed to serve as a foundation for building detections, investigating incidents, and documenting SOC workflows.


+ Architecture Summary

+ Splunk SIEM Server
- **OS:** Ubuntu Server 22.04 LTS  
- **SIEM:** Splunk Enterprise  
- **Role:** Central log ingestion and analysis platform  
- **Service Management:** systemd running under a non-root service account  
- **Storage:** Virtual disk expanded and resized using LVM  

Splunk is configured to:
- Receive logs over TCP/9997
- Run persistently across reboots
- Separate security telemetry into dedicated indexes
- Enforce proper disk usage thresholds


+ Windows Endpoint
- **OS:** Windows 10  
- **Role:** Monitored endpoint  
- **Log Sources:**
  - Windows Security Event Logs
  - Windows System and Application Logs
  - Sysmon Operational Logs  

**Installed tools:**
- Sysmon for enhanced endpoint visibility
- Splunk Universal Forwarder for log collection

This system generates authentication events, process activity, and system behavior that can be used for detection and investigation practice.


+ Index Strategy

To keep data clean and investigation-friendly, logs are separated into dedicated indexes:

- **windows** – Windows Security, System, and Application logs  
- **sysmon** – Sysmon Operational telemetry  
- **Splunk system indexes** (`_internal`, `_metrics`, `_introspection`) – used for Splunk health and performance data  

No security data is stored in the default `main` index.



## Key Skills Demonstrated

This project demonstrates practical experience with:

- Splunk Enterprise installation and configuration
- Forwarder deployment and connectivity troubleshooting
- Linux disk expansion and LVM resizing
- systemd service management
- Running Splunk as a non-root service account
- Log ingestion validation and index hygiene
- SOC-style troubleshooting and verification


+ Challenges Encountered and Resolved

While building this lab, I worked through several real-world issues, including:

- Splunk searches failing due to disk pressure
- Expanding the VM disk and resizing the Linux filesystem
- Fixing Splunk startup failures caused by root-owned installations
- Migrating Splunk to run correctly as a service-managed user
- Verifying persistent startup and service health

These issues helped reinforce how important infrastructure stability is in a SOC environment.



+ Current State

- Environment is stable and operational
- Logs are actively flowing from the Windows endpoint
- Indexing and searching are functioning normally
- Disk and service issues have been resolved
- VM snapshots are in place for safe rollback



+ Planned Next Steps

- Build and document SOC detections
- Perform incident investigations and write reports
- Map activity to MITRE ATT&CK techniques
- Create dashboards and visualizations
- Introduce controlled attack simulations to validate detections



+ Disclaimer
This lab is for educational and defensive security purposes only. All activity occurs in an isolated environment, and no external systems are targeted.
