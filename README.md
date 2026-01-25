Splunk SOC Home Lab
Overview

I built this Splunk SOC home lab to get hands-on experience with how a SIEM actually operates in a real SOC-style environment. The goal wasn’t just to install Splunk and ingest logs, but to understand how SIEMs behave when you have to manage storage, services, data flow, and troubleshoot issues that come up in practice.

This lab focuses on defensive security, SIEM operations, and incident investigation rather than offensive exploitation or red-team tooling.

What This Lab Includes

This lab consists of a Linux-based Splunk Enterprise SIEM and a Windows endpoint generating realistic security telemetry. It includes Windows Security Event Logs, System and Application logs, and Sysmon data. Logs are routed to dedicated indexes, and the environment incorporates real-world troubleshooting and operational fixes.

The lab is designed to serve as a foundation for building detections, investigating incidents, and documenting SOC workflows.

Architecture Summary
Splunk SIEM Server

The SIEM runs on Ubuntu Server 22.04 LTS with Splunk Enterprise installed as the primary log ingestion and analysis platform. Splunk is managed using systemd and runs under a non-root service account. The virtual disk was expanded and resized using LVM to support sustained log ingestion.

Splunk is configured to receive logs over TCP on port 9997, run persistently across reboots, separate security telemetry into dedicated indexes, and enforce proper disk usage thresholds to prevent search failures.

Windows Endpoint

The monitored endpoint runs Windows 10 and is configured to generate realistic security telemetry. Log sources include Windows Security, System, and Application logs, as well as Sysmon Operational logs.

Sysmon is installed to provide enhanced endpoint visibility, and the Splunk Universal Forwarder is used to collect and forward logs to the SIEM. This endpoint generates authentication events, process activity, and system behavior suitable for detection and investigation practice.

Index Strategy

To keep data organized and investigation-friendly, logs are separated into dedicated indexes. Windows Security, System, and Application logs are stored in the windows index, while Sysmon telemetry is stored in the sysmon index. Splunk system indexes are used only for internal health and performance data. No security data is stored in the default main index.

Key Skills Demonstrated

This project demonstrates practical experience with Splunk Enterprise installation and configuration, Universal Forwarder deployment, forwarder connectivity troubleshooting, Linux disk expansion and LVM resizing, systemd service management, running Splunk as a non-root service account, log ingestion validation, index hygiene, and SOC-style troubleshooting and verification.

Challenges Encountered and Resolved

While building this lab, I worked through several real-world issues. These included Splunk searches failing due to disk pressure, expanding the virtual disk and resizing the Linux filesystem, fixing startup failures caused by root-owned Splunk installations, migrating Splunk to run correctly as a service-managed user, and verifying persistent startup and overall service health.

Working through these issues reinforced how important infrastructure stability and visibility are in a SOC environment.

Current State

The environment is stable and fully operational. Logs are actively flowing from the Windows endpoint, indexing and searching are functioning normally, disk and service issues have been resolved, and VM snapshots are in place for safe rollback.

Planned Next Steps

Next steps for this lab include building and documenting SOC detections, performing incident investigations and writing reports, mapping activity to MITRE ATT&CK techniques, creating dashboards and visualizations, and introducing controlled attack simulations to validate detections.

Troubleshooting and Lessons Learned
Sysmon Ingestion Issue

During setup, Windows event logs were successfully ingesting into Splunk, but Sysmon events were not appearing, even though Sysmon was installed and generating logs locally on the Windows endpoint.

Windows Security, System, and Application logs were searchable, but the sysmon index remained empty. Inputs appeared to be configured correctly, and Sysmon activity was visible locally, which made the issue non-obvious.

After validating the Sysmon service, local event generation, forwarder connectivity, and input configuration, the root cause was identified as a service permission issue. The Splunk Universal Forwarder was not running with sufficient privileges to read the Sysmon Operational event log, which is a protected Windows event channel.

The issue was resolved by changing the SplunkForwarder service to run under the Local System account. Once the service was restarted, Sysmon events immediately began ingesting into Splunk and became searchable in the sysmon index.
