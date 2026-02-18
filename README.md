# Azure and Microsoft Sentinel Honeypot SOC Lab

## 🎯 Overview
Cloud-based honeypot using Microsoft Sentinel SIEM to detect and respond to real-world RDP brute force attacks through custom KQL detection rules, automated threat intelligence enrichment, and structured incident response playbooks.

## 🔍 Lab Scope

#### Objectives

- Deploy an intentionally exposed Azure VM as a honeypot to attract real-world attacks.
- Configure centralised log collection and forwarding using Microsoft Sentinel.
- Detect and analyse real-world RDP brute force attacks using KQL.
- Enrich attack data with geographic context using a GeoIP watchlist.
- Visualise global attack patterns through a custom Sentinel Workbook dashboard.

#### Technologies Used

- Cloud Platform: Microsoft Azure.
- Virtual Machine: Windows 10.
- SIEM: Microsoft Sentinel.
- Log Management: Azure Log Analytics Workspace.
- Query Language: KQL (Kusto Query Language).
- Security Tools: Azure Network Security Groups (NSG), Windows Event Viewer.
- Threat Intelligence: GeoIP CSV Watchlist.

## Architecture & Network Diagram
This lab involved several key components:
1. **Azure Resource Group:** Resource Group that contained all lab resources. 
2. **Azure Virtual Network:** Virtual Network with a default subnet so that the virtual machine could have a new and believable public facing IP address.
3. **Azure Virtual Machine:** Windows 10 (default settings, connected to the virtual network).
4. **Network Security Group:** Configured to allow all inbound traffic from any source and protocol.
5. **Logs Analytics Workspace:** Workspace to view and house all logs that were transmitted to Microsoft Sentinel.
6. **Microsoft Sentinel:** Linked to the virtual machine to transmit all incoming security event logs (from Windows Event Viewer on the virtual machine), using a data collection rule. 

Here is the network configuration for the project:
<img src="https://i.imgur.com/ttxpgBB.png" height="80%" width="80%"/>

## Walkthrough
### 1. Azure Environment Setup
Created a free Azure subscription and set up the core infrastructure — a Resource Group to house all lab resources, and a Virtual Network with a default subnet to provide the VM with a public-facing IP address.
<img src="https://i.imgur.com/9ngikcu.jpeg" height="500%" width="5000%"/>

### 2. Deploying the Honeypot VM
Deployed a Windows 10 Virtual Machine connected to the virtual network. The machine was intentionally configured to be exposed — the Network Security Group (cloud firewall) was set to allow all inbound traffic from any source, and the internal Windows Defender Firewall was fully disabled.
<img src="https://i.imgur.com/nMze3Sa.jpeg" height="120%" width="120%"/>

### 3. Centralised Log Collection
Created a Log Analytics Workspace as a central log repository and linked it to a Microsoft Sentinel instance. Configured a Data Collection Rule using the Azure Monitoring Agent to forward all Windows Security Event logs from the VM into the workspace.

### 4. Querying Attacks with KQL
Once logs began flowing, used KQL to query the SecurityEvent table and filter for Event ID 4625 (failed logon attempts), revealing real-world brute force attempts against the honeypot within minutes of deployment.
<img src="https://i.imgur.com/N3h8O3T.jpeg" height="80%" width="80%"/>

### 5. GeoIP Enrichment
Uploaded a GeoIP CSV dataset as a Sentinel Watchlist, mapping IP address ranges to geographic coordinates, city, and country. Used KQL to join this watchlist against the attack logs, enriching raw IP addresses with real-world location data.
<img src="https://i.imgur.com/fnFwMrc.jpeg" height="80%" width="80%"/>

### 6. Attack Map Visualisation
Built a Microsoft Sentinel Workbook to visualise the enriched data on an interactive world map, displaying the global distribution and volume of attacks against the honeypot in real time.
<img src="https://i.imgur.com/BpbZGBg.jpeg" height="80%" width="80%"/>

## Lab Outcomes
- Deployed and integrated a cloud-based honeypot into Microsoft Sentinel, configuring data collection rules to ingest real-time Windows Security Event logs from an intentionally exposed Azure VM.
- Wrote KQL queries to detect RDP brute force attempts, password spraying, and account enumeration, translating raw logs into actionable security findings.
- Enriched attack data with geographic context by integrating a GeoIP CSV watchlist, and built a Sentinel Workbook to visualise the global distribution of attack sources on an interactive map.
- Observed real-world attack behaviour from external threat actors within hours of the VM being exposed to the public internet.


## Next Steps
This project so far covers the first steps for a SOC lab environment. In the next stage of the project, I am going to:
- Create custom Sentinel analytic rules to automatically detect and alert on RDP brute force activity.
- Investigate and document two brute force attack incidents end-to-end.
- Develop new Sentinel Workbook dashboards to support incident investigation.
- Produce remediation recommendations to harden against brute force attacks, using automated playbooks and manual response procedures.
