<!--# Azure-SOC-Honeynet-Project-->
# Building a SOC + Honeynet in Azure (Live Traffic)
![SOC Honeynet (1)](https://github.com/0xbythesecond/Azure-SOC-Honeynet-Project/assets/23303634/43177fa9-4746-4f8d-8774-f9aca74b891d)

## Introduction
In this project, I established a mini honeynet within Azure and aggregated log data from various sources into a Log Analytics workspace. Microsoft Sentinel utilized this data to develop attack maps, trigger alerts, and generate incident reports. I initially assessed security metrics in an unsecured environment over a 24-hour period. Following the implementation of security controls to strengthen the environment, I conducted a subsequent 24-hour assessment of the metrics. The findings are detailed below.


## Azure Resources Deployed, Technologies, and Regulations used:

- A virtual networking service in Azure (VNet)
- Security group for network-level security in Azure (NSG)
- Two Windows 10 Pro virtual machines and one Linux server virtual machine
- Workspace for log analytics using Kusto Query Language (KQL)
- Secure secrets management using Azure Key Vault
- Data storage using an Azure Storage Account
- Security information and event management with Microsoft Sentinel
- Protection of cloud resources via Microsoft Defender for Cloud
- Remote desktop service for accessing Windows remotely
- System management through Command Line Interface (CLI)
- Automation and configuration management with PowerShell
- Applying security controls based on NIST SP 800-53 Rev 5
- Guidance for incident handling according to NIST SP 800-61 Rev 2
 
## Course of Action
- ***Setting Up the Honeynet:*** Initially, I deployed the honeynet using Virtual Machines, configuring it with the internal firewall disabled and the Network Security Group (NSG) set to allow all incoming ports and traffic.

- ***Observation and Analysis:*** I set up Azure to effectively aggregate logs from multiple sources into a designated log analytics workspace. Leveraging Microsoft Sentinel’s advanced capabilities, I developed comprehensive attack maps, triggered precise alerts, and generated detailed incident reports based on the meticulously analyzed log data.

- ***Monitoring and Assessing Security Metrics:*** I monitored the unprotected environment for a full 24-hour period, capturing key security metrics to establish a baseline for subsequent comparison following the implementation of security enhancements.

- ***Resolving Security Issues:*** After analyzing incidents and identifying vulnerabilities, I strengthened the environment by applying best practices and recommended strategies from Azure.

- ***Post-Remediation Analysis:*** I conducted a further 24-hour assessment to review the security metrics post-remediation. The new data was meticulously compared to the initial baseline to facilitate a detailed comparative analysis.

We'll be presenting the following metrics:

- SecurityEvent for tracking Windows Event Logs
- Syslog to capture Linux Event Logs
- SecurityAlert, which encompasses alerts triggered in Log Analytics
- SecurityIncident for incidents identified by Sentinel
- AzureNetworkAnalytics_CL, focusing on malicious flows permitted into our honeynet


## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/gBvHJo4.gif)
Before Implementation:

Initially, all resources were deployed with unrestricted internet exposure. The Virtual Machines had their Network Security Groups and built-in firewalls configured to allow all traffic, while other resources were deployed with public endpoints visible on the Internet, with no use of Private Endpoints.

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/oQtbais.gif)
After Implementation:

During the evaluation phase, we reinforced the Network Security Groups to restrict traffic to only that originating from my administrative workstation. Additionally, we bolstered the security of other resources by configuring their built-in firewalls and implementing Private Endpoint functionality.

## Attack Maps Before Hardening / Security Controls
The attack map displayed provides an overview of attack attempts directed at a publicly exposed Microsoft SQL Server over a 24-hour period. The map highlights the precise locations from which these attacks or login attempts originated.
![MSSQL Allowed Access](https://i.imgur.com/7Jhlpm9.png) <br />

The attack map displayed illustrates various authentication failures logged in the syslog of the Linux server I configured. It highlights unauthorized access attempts originating from sources external to the local network. This underscores the critical need for robust authentication methods and vigilant monitoring of system logs to detect and prevent potential intrusions.
![Linux Syslog Auth Failures](https://i.imgur.com/FkT7IDZ.png) <br />

The attack map presented illustrates a series of failed attempts involving RDP (Remote Desktop Protocol) and SMB (Server Message Block). This map clearly demonstrates ongoing efforts by potential attackers to target these protocols. The visual representation underscores the critical need to enhance the security of remote access and file-sharing services as a defensive strategy against unauthorized access and to mitigate the risk of cyber threats.
![Windows RDP/SMB Auth Failures](https://i.imgur.com/I2KCAi6.png) <br />

The attack map displayed vividly illustrates the impact of leaving the Network Security Group (NSG) open, which permits unchecked malicious network traffic. This visual emphasizes the critical importance of implementing robust security measures, such as enforcing stringent NSG rules, to prevent unauthorized access and mitigate the risks associated with potential threats.
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/kZmi4yf.png)


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
<br />
`Start Time:` 2024-08-04T20:16:39 <br/>
`Stop Time:` 2024-08-05T20:16:39

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 75281
| Syslog                   | 2455
| SecurityAlert            | 7
| SecurityIncident         | 251
| AzureNetworkAnalytics_CL | 103

## Attack Maps After Hardening / Security Controls

  >**Note**: All map queries returned no results, indicating there were no instances of malicious activity detected during the 24-hour period following the hardening process.

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for an additional 24 hours, after we applied security controls:
<br />
`Start Time:` 2024-08-07T17:58:14 <br/>
`Stop Time:` 2024-08-08T17:58:14

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 9290
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Change after Securing Environment
| Metric                                          | Percent
| ----------------------------------------------- | -----
| Security Event (Windows VM)                     |  -87.66%
| Syslog (Linux VM)                               |  -99.96%
| Security Alert (Microsoft Defender for Cloud)   |  -100.00%
| Security Incident (Sentinel Incidents)          |  -100.00%
| NSG Inbound Malicious Flows Allowed             |  -100.00%


## Reflection
Creating this lab and analyzing real-world traffic using attack maps and KQL data has been both challenging and gratifying. It was amazing to see everything come together, demonstrating the stark difference between an insecure and a secure environment. Before implementing security controls, the environment was inundated with malicious traffic. While the resources were still vulnerable, I observed various IP addresses and usernames that bad actors attempted to use to access my virtual machines. However, after completing the hardening process and waiting 24 hours, it was remarkable to see no signs of allowed traffic from these bad actors on the public internet – a truly satisfying outcome.

## Conclusion

In this project, we set up a small-scale honeynet on the Microsoft Azure platform and effectively integrated various log sources into a specific Log Analytics workspace. A key component of this setup was Microsoft Sentinel, which actively generated alerts and initiated incidents based on the ingested logs. We meticulously measured detailed metrics in the vulnerable environment before applying any security controls and then conducted a follow-up evaluation after fortifying the infrastructure. The standout result was a significant decrease in the number of security events and incidents, clearly demonstrating the effectiveness of the security measures we implemented.

It’s important to acknowledge that if the network's resources had been in active use by regular users, it’s likely that a higher volume of security events and alerts might have been generated in the 24-hour period following the implementation of the security controls.
