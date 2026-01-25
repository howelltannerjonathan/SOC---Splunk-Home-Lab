Networking and Remote Access

The Splunk SIEM server is configured with dual network adapters:

A host-only adapter for internal log ingestion from Windows endpoints

A NAT adapter to support outbound connectivity and secure remote access via Tailscale

This design allows internal SOC traffic to remain isolated while enabling secure remote access to Splunk Web without exposing the lab to the public internet.
