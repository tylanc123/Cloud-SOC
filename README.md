# Building a SOC + Honeynet in Azure (Live Traffic)
Cloud Honeynet / SOC ![68747470733a2f2f692e696d6775722e636f6d2f5a5778653033652e6a7067](https://github.com/tylanc123/Cloud-SOC/assets/153654738/c9da5e46-a77b-408d-98f5-0c413455c09c)

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

![nsg-malicious-allowed-in](https://github.com/tylanc123/Cloud-SOC/assets/153654738/441d61cc-5643-49f7-86e6-8dcfcc6389eb>)<br>
![Linux Syslog Auth Failures](https://github.com/tylanc123/Cloud-SOC/assets/153654738/ba1ba890-ce19-4a48-b3ec-4129254a95c3>)<br>
![Windows RDP/SMB Auth Failures](https://github.com/tylanc123/Cloud-SOC/assets/153654738/ad4a8e55-5568-420b-be6b-1752e0defb2e>)<br>


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-12-12 21:11:00
Stop Time 2023-12-13 21:11:00

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 15830
| Syslog                   | 17461
| SecurityAlert            | 5
| SecurityIncident         | 196
| AzureNetworkAnalytics_CL | 1065

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-12-13 14:27:59
Stop Time	2023-12-14 14:27:59

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 9586
| Syslog                   | 3403
| SecurityAlert            | 0
| SecurityIncident         | 29
|AzureNetworkAnalytics_CL  | 0

## Results

Change after security environment

| Metric                   | Count     |
| ------------------------ | --------- |
| SecurityEvent            | -39.44%   |
| Syslog                   | -80.51%   |
| SecurityAlert            | -100.00%  |
| SecurityIncident         | -85.20%   |
| AzureNetworkAnalytics_CL | -100.00%  |



## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.# Cloud-SOC
