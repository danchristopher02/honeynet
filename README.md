# Building a SOC + Honeynet in Azure (Live Traffic)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![windows-rdp-auth-fail (Before)](https://github.com/user-attachments/assets/7a6d8603-90f7-4a9e-bdff-007d2c5a6975)
![linux-ssh-auth-fail (Before)](https://github.com/user-attachments/assets/9f400dae-a0ef-4354-819a-56c40f994cc3)
![nsg-malicious-allowed-in (Before)](https://github.com/user-attachments/assets/0973ca18-fd5d-4cab-a721-37bf116af036)
![mssql-auth-fail (Before)](https://github.com/user-attachments/assets/fedc4a5e-1a60-49fe-8452-3cec4130dea6)



## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:

Start Time 4/13/2024  7:01:32 PM
Stop Time 4/14/2024  7:01:32 PM

|  Metric                                         | Count
|  --------------------------------------         | -----
|  Security Events (Windows VMs)                  | 37216
|  Syslog (Linux VMs)                             | 23628
|  SecurityAlert (Microsoft Defender for Cloud)   | 0
|  SecurityIncident (Sentinel Incidents)          | 294
|  NSG Inbound Malicious Flows Allowed            | 2061

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:

Start Time 4/17/2024  11:49:49 PM
Stop Time	4/18/2024  11:49:49 PM

| Metric                                         | Count
| --------------------------------------         | -----
| Security Events (Windows VMs)                  | 766
| Syslog (Linux VMs)                             | 0
| SecurityAlert (Microsoft Defender for Cloud)   | 0
| SecurityIncident (Sentinel Incidents)          | 0
| NSG Inbound Malicious Flows Allowed            | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
