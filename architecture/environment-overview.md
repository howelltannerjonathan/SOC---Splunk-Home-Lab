# Environment Overview

+ Purpose
I built this environment to get hands-on experience with how a SIEM actually operates in a SOC-style setup, beyond just installing tools and clicking through dashboards. The goal was to understand log ingestion, indexing, service management, and how Splunk behaves when things don’t go perfectly.

This lab is focused on **defensive security**, **SIEM operations**, and **incident investigation**, rather than offensive testing.


+ Network Design

- VirtualBox-based lab environment
- Host-only networking used for log traffic between systems
- NAT enabled only when needed for updates
- No external exposure of the monitored endpoint
- Splunk Web accessible only from the host machine

This setup keeps everything isolated while still allowing realistic internal monitoring, similar to how a SOC environment is segmented in practice.


+ Virtual Machines

 SplunkSIEM

- **Operating System:** Ubuntu Server 22.04 LTS  
- **Role:** Central SIEM server  
- **CPU:** 2 vCPUs  
- **Memory:** 8 GB RAM  
- **Storage:** Virtual disk expanded and resized using LVM  
- **SIEM:** Splunk Enterprise  
- **Service Management:** systemd running under a non-root service account  

**Listening Ports:**
- 8000 – Splunk Web interface  
- 8089 – Splunk management port  
- 9997 – Forwarder receiving port  

Splunk is configured to run as a dedicated `splunk` user and start automatically on boot, mirroring how it would be deployed in a real environment.


+ Windows Endpoint

- **Operating System:** Windows 10  
- **Role:** Monitored endpoint  
- **Log Sources:**
  - Windows Security Event Logs
  - Windows System and Application Logs
  - Sysmon Operational Logs  

**Installed Tools:**
- Sysmon for enhanced endpoint visibility  
- Splunk Universal Forwarder for log collection  

This system generates normal user, process, and system activity that can be used to practice detection and investigation workflows.


+ Index Strategy

Logs are separated into dedicated indexes to keep data organized and easier to work with during investigations:

- **windows** – Windows Security, System, and Application logs  
- **sysmon** – Sysmon Operational telemetry  
- **Splunk system indexes** (`_internal`, `_metrics`, `_introspection`) – used for Splunk health and performance monitoring  

No security logs are stored in the default `main` index.


+ Data Flow

1. Events are generated on the Windows endpoint  
2. The Splunk Universal Forwarder collects the logs  
3. Logs are sent to the SplunkSIEM server over TCP/9997  
4. Splunk parses and indexes the data into the appropriate indexes  
5. The data is available for searching, detections, and investigations  


+ Current State

- Environment is stable and functioning as expected  
- Windows and Sysmon logs are actively flowing into Splunk  
- Search and indexing are working normally  
- Disk space and service startup issues have been resolved  
- VM snapshots are in place for recovery and rollback  


+ Future Plans

- Build and document SOC detections  
- Perform basic incident investigations and write reports  
- Map activity to MITRE ATT&CK techniques  
- Create dashboards and visualizations  
- Introduce controlled attack simulations for detection testing
