---
title: Microsoft Defender for Cloud data security | Microsoft Docs
description: Learn how data is managed and safeguarded in Microsoft Defender for Cloud.
ms.topic: overview
ms.author: benmansheim
author: bmansheim
ms.date: 11/09/2021
---
# Microsoft Defender for Cloud data security

To help customers prevent, detect, and respond to threats, Microsoft Defender for Cloud collects and processes security-related data, including configuration information, metadata, event logs, and more. Microsoft adheres to strict compliance and security guidelines—from coding to operating a service.

This article explains how data is managed and safeguarded in Defender for Cloud.

## Data sources
Defender for Cloud analyzes data from the following sources to provide visibility into your security state, identify vulnerabilities and recommend mitigations, and detect active threats:

- **Azure services**: Uses information about the configuration of Azure services you have deployed by communicating with that service’s resource provider.
- **Network traffic**: Uses sampled network traffic metadata from Microsoft’s infrastructure, such as source/destination IP/port, packet size, and network protocol.
- **Partner solutions**: Uses security alerts from integrated partner solutions, such as firewalls and antimalware solutions.
- **Your machines**: Uses configuration details and information about security events, such as Windows event and audit logs, and syslog messages from your machines.

## Data protection

### Data segregation
Data is kept logically separate on each component throughout the service. All data is tagged per organization. This tagging persists throughout the data lifecycle, and it is enforced at each layer of the service.

### Data access
To provide security recommendations and investigate potential security threats, Microsoft personnel may access information collected or analyzed by Azure services, including process creation events, and other artifacts, which may unintentionally include customer data or personal data from your machines. 

We adhere to the [Microsoft Online Services Data Protection Addendum](https://www.microsoftvolumelicensing.com/Downloader.aspx?DocumentId=17880), which states that Microsoft will not use Customer Data or derive information from it for any advertising or similar commercial purposes. We only use Customer Data as needed to provide you with Azure services, including purposes compatible with providing those services. You retain all rights to Customer Data.

### Data use
Microsoft uses patterns and threat intelligence seen across multiple tenants to enhance our prevention and detection capabilities; we do so in accordance with the privacy commitments described in our [Privacy Statement](https://privacy.microsoft.com/privacystatement).

## Manage data collection from machines
When you enable Defender for Cloud in Azure, data collection is turned on for each of your Azure subscriptions. You can also enable data collection for your subscriptions in Defender for Cloud. When data collection is enabled, Defender for Cloud provisions the Log Analytics agent on all existing supported Azure virtual machines and any new ones that are created.

The Log Analytics agent scans for various security-related configurations and events it into [Event Tracing for Windows](/windows/win32/etw/event-tracing-portal) (ETW) traces. In addition, the operating system will raise event log events during the course of running the machine. Examples of such data are: operating system type and version, operating system logs (Windows event logs), running processes, machine name, IP addresses, logged in user, and tenant ID. The Log Analytics agent reads event log entries and ETW traces and copies them to your workspace(s) for analysis. The Log Analytics agent also enables process creation events and command line auditing.

If you aren't using Microsoft Defender for Cloud's enhanced security features, you can also disable data collection from virtual machines in the Security Policy. Data Collection is required for subscriptions that are protected by enhanced security features. VM disk snapshots and artifact collection will still be enabled even if data collection has been disabled.

You can specify the workspace and region where data collected from your machines is stored. The default is to store data collected from your machines in the nearest workspace as shown in the following table:

| VM Geo                                      | Workspace Geo  |
|---------------------------------------------|----------------|
| United States, Brazil, South Africa         | United States  |
| Canada                                      | Canada         |
| Europe (Excluding United Kingdom)           | Europe         |
| United Kingdom                              | United Kingdom |
| Asia (Excluding India, Japan, Korea, China) | Asia Pacific   |
| Korea                                       | Asia Pacific   |
| India                                       | India          |
| Japan                                       | Japan          |
| China                                       | China          |
| Australia                                   | Australia      |


> [!NOTE]
> **Microsoft Defender for Storage** stores artifacts regionally according to the location of the related Azure resource. Learn more in [Overview of Microsoft Defender for Storage](defender-for-storage-introduction.md).


## Data consumption
Customers can access Defender for Cloud related data from the following data streams:

| Stream                                                                                | Data types                                                                                                                                                                                                          |
|---------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Azure Activity log](../azure-monitor/essentials/activity-log.md)                       | All security alerts, approved Defender for Cloud [just-in-time](just-in-time-access-usage.md) access requests, and all alerts generated by [adaptive application controls](adaptive-application-controls.md).|
| [Azure Monitor logs](../azure-monitor/data-platform.md)                      | All security alerts.                                                                                                                                                                                                |
| [Azure Resource Graph](../governance/resource-graph/overview.md)                      | Security alerts, security recommendations, vulnerability assessment results, secure score information, status of compliance checks, and more.                                                                       |
| [Microsoft Defender for Cloud REST API](/rest/api/securitycenter/) | Security alerts, security recommendations, and more.                                                                                                                                                                |


## Next steps

In this document, you learned how data is managed and safeguarded in Microsoft Defender for Cloud. 

To learn more about Microsoft Defender for Cloud, see [What is Microsoft Defender for Cloud?](defender-for-cloud-introduction.md)
