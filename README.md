# Creating a SOC + Honeynet in Azure with Live Traffic
![image](https://github.com/TechRockyP/Azure-SOC/assets/151791347/b887b73b-5f93-4717-b9d8-18bad47b80aa)


## Introduction

For this project, I constructed a miniature honeynet within Azure, integrating log sources from various resources into a Log Analytics workspace. Microsoft Sentinel was used in this workspace to craft attack maps, trigger alerts, and document incidents. After assessing security metrics in an unsecured environment over a 24-hour period, I implemented the needed security controls to fortify the system. Following this, I went ahead and conducted a second round of metric measurements over the next 24 hours, with the results outlined below.

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecure Before Hardening

![image](https://github.com/TechRockyP/Azure-SOC/assets/151791347/7367d69d-0400-4138-9a19-55aa7447eab2)

## Architecure After Hardening

![image](https://github.com/TechRockyP/Azure-SOC/assets/151791347/0081c9cd-29ed-4c07-90fc-1cb0f9c711b2)


The architecture of the mini honeynet in Azure are made up of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

In the initial "BEFORE" metrics phase, all resources were initially deployed and made accessible on the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls configured with unrestricted access, and all other resources were deployed with public endpoints that were openly visible to the Internet. As a result, there was no necessity for Private Endpoints during this phase.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my own workstation, and all other resources were protected by their own built-in firewalls as well as Private Endpoint.

## Attack Maps Before Hardening / Security Controls

[NSG Allowed Inbound Malicious Flows]![image](https://github.com/TechRockyP/Azure-SOC/assets/151791347/dbbbfa5e-40ce-4ae5-9cab-3c38f595591b)


[Windows RDP/SMB Auth Failures]
![image](https://github.com/TechRockyP/Azure-SOC/assets/151791347/f66dc6f4-ce39-409e-a0b7-65d565410dac)


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-11-15 17:56:04
Stop Time 2023-11-16 17:56:04

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 19641
| Syslog                   | 2957
| SecurityAlert            | 3
| SecurityIncident         | 98
| AzureNetworkAnalytics_CL | 999

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-11-20 18:05:29
Stop Time	2023-11-21 18:05:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 2391
| Syslog                   | 135
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

For this project, a compact honeynet was built within Microsoft Azure, incorporating log sources into a Log Analytics workspace. Microsoft Sentinel was used to initiate alerts and generate incidents by analyzing the ingested logs. Furthermore, security metrics were assessed in the unsecured environment both prior to the implementation of security controls and subsequently after their application. Notably, the implementation of security measures resulted in a significant decrease in the quantity of security events and incidents, showing it's effictiveness.

Note that more security events and alerts might have been generated in the 24 hours after the security controls were put in place if regular users were heavily using the network's resources.
