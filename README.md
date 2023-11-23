# Building a SOC + Honeynet in Azure (Live Traffic)

## Introduction

For this project, I created a small Azure honeynet and used log sources from several sources to fill a workspace for Log Analytics. Microsoft Sentinel then uses this workspace to produce incidents, generate alerts, and develop attack maps. After a day of measuring security metrics in the unsecure environment, I applied security measures to harden the environment and monitored security metrics again for a further day. The outcomes are displayed below.

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)


The architecture of the mini honeynet in Azure are made up of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

When the resources were first deployed, they were entirely online for the "BEFORE" metrics. There was no need for private endpoints because all other resources were deployed with public endpoints visible to the Internet and the Virtual Machines' built-in firewalls and Network Security Groups were both wide open.

Network Security Groups were hardened for the "AFTER" metrics by preventing ALL traffic save from my workstation. All other resources were shielded by both private endpoints and the firewalls that were already installed.

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

This project involved building a mini honeynet in Microsoft Azure and integrating log sources into a workspace for log analytics. Based on the ingested data, Microsoft Sentinel was used to generate incidents and alerts. Furthermore, measurements were taken in the compromised environment both before and after security measures were put in place. Notably, the implementation of security measures resulted in a significant decrease in the quantity of security events and incidents, indicating their effectiveness.

Note that more security events and alerts might have been generated in the 24 hours after the security controls were put in place if regular users were heavily using the network's resources.
