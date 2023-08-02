# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this repository, I have developed a compact honeynet within the Azure framework. The project involves gathering log data from diverse sources, which is then directed into a Log Analytics workspace. This workspace serves as input for Microsoft Sentinel, enabling the creation of attack visualizations, activation of alerts, and incident generation. The process entails measuring security metrics within an initially vulnerable environment over a 24-hour period. Subsequently, security enhancements are applied to bolster the environment, followed by another 24-hour metric assessment. The subsequent findings are presented below, encompassing the following metrics:

SecurityEvent (Windows Event Logs)
Syslog (Linux Event Logs)
SecurityAlert (Generated Log Analytics Alerts)
SecurityIncident (Incidents Generated by Sentinel)
AzureNetworkAnalytics_CL (Identified Malicious Flows Infiltrating the Honeynet)

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

In the "BEFORE" measurements, all assets were initially set up and made accessible via the internet. The Virtual Machines had unrestricted Network Security Groups and open built-in firewalls. Similarly, all remaining resources were deployed with public endpoints, devoid of any utilization of Private Endpoints.

In the "AFTER" measurements, Network Security Groups were fortified by implementing a comprehensive block on ALL traffic, except that originating from my administrative workstation. Additionally, all other resources were shielded by their inherent firewalls, complemented by the integration of Private Endpoints.

## Attack Maps Before Hardening / Security Controls
![Windows RDP SMB Auth Fail](https://i.imgur.com/HEETLvE.png)<br>
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/unH1Xjw.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/eb0JsaP.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/QCEoYZE.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 7/27/2023 12:13:41
Stop Time 7/28/2023 12:34:13

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 53336
| Syslog                   | 4286
| SecurityAlert            | 4
| SecurityIncident         | 256
| AzureNetworkAnalytics_CL | 919

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 8/1/2023 11:46:57
Stop Time	8/2/2023 11:48:27

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 9107
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

Within this endeavor, a compact honeynet was established within the Microsoft Azure framework. The integration of log sources occurred within a Log Analytics workspace, enabling Microsoft Sentinel to initiate alerts and establish incidents using the ingested logs. Furthermore, an evaluation of metrics took place in an initially vulnerable environment, both prior to the enforcement of security measures and subsequent to their implementation. A noteworthy observation is the considerable reduction in the count of security events and incidents post the activation of security controls, effectively showcasing their efficacy.

It's important to acknowledge that if the network's resources were extensively utilized by regular users, there is a likelihood of increased generation of security events and alerts during the 24-hour period following the implementation of the security controls.
