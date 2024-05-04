# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, applied some security controls to harden the environment, measured metrics for another 24 hours, and then showed the results below. The metrics we will show are:

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
- SQL Server on 1 windows vm
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed and exposed to the Internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources were deployed with public endpoints visible to the Internet—aka, there was no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![nsg-malicious-allowed-in (before)](https://github.com/jerrycoriolan/Cloud-SOC/assets/168882662/0c407712-30f1-48fb-bcde-19e4413e6555)
![linux-ssh-auth-fail (before)](https://github.com/jerrycoriolan/Cloud-SOC/assets/168882662/5cd64e6c-1a96-4a0d-b18b-18039c72243c)
![windows-rdp-auth-fail (before)](https://github.com/jerrycoriolan/Cloud-SOC/assets/168882662/c5dd8d22-36c9-4c17-8d71-1f1c64c5adbf)
![mssql-auth-fail (before)](https://github.com/jerrycoriolan/Cloud-SOC/assets/168882662/3d464da8-0edb-4f2a-8cf9-94afa29845a2)

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:

Start Time 2024-05-01T19:40:05.7043047Z
Stop Time  2024-05-02T19:40:05.7043047Z

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 87466
| Syslog                   | 7157
| SecurityAlert            | 1
| SecurityIncident         | 131
| AzureNetworkAnalytics_CL | 2080

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24-hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we applied security controls:

Start Time 2024-05-03T00:21:57.4467683Z
Stop Time	 2024-05-04T00:21:57.4467683Z

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 27395
| Syslog                   | 1
| SecurityAlert            | 1
| SecurityIncident         | 6
| AzureNetworkAnalytics_CL | 0

| Results                  | Change %
| ------------------------ | -----
| SecurityEvent            | -68.68%
| Syslog                   | -99.99%
| SecurityAlert            | 0.00%
| SecurityIncident         | -95.42%
| AzureNetworkAnalytics_CL | -100.00%

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure, and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel triggered alerts and created incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied and then again after implementing security measures. Notably, the number of security events and incidents drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
