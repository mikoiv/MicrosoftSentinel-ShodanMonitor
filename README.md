# MicrosoftSentinel-ShodanMonitorAlerts

## Introduction

Shodan Monitor is a service for Shodan subscribers that can detect the following issues in publicly available networks and hosts:
* Services associated with ICS or IoT devices
* Compromised or malware-related services
* New open ports, uncommon open ports
* Open databases
* Known vulnerabilities
* Expired certificates

In brief it provides a service for managing public attack surface, usually for your own assets.

This repository contains an Azure Logic App for ingesting Shodan Monitor alerts for querying, alerting and hunting in Microsoft Sentinel:

[![Log query](https://github.com/mikoiv/MicrosoftSentinel-ShodanMonitor/blob/main/Images/sentinel-logquery.png)](https://github.com/mikoiv/MicrosoftSentinel-ShodanMonitor/blob/main/Images/sentinel-logquery.png)

For further details you can read the following writeup: 

**http://secopslab.fi/2021-12-microsoftsentinel-shodanmonitor/**

## Deployment

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmikoiv%2FMicrosoftSentinel-ShodanMonitor%2Fmain%2Fazuredeploy.json)

## Parameters 

When deploying the template you have the following parameters to configure: 

 | Parameter  | Description |
| ------------- | ------------- |
| **Resource Group** | Resource group for deployed resources |
| **Region**| Azure region for deployed resources |
| **Playbook Name**  | Logic App name (default: ShodanMonitor-Sentinel) | 
| **Log Analytics Connection Name** | API connection name (default: loganalyticsconnection-ShodanMonitor-Sentinel)|
| **Log Analytics Workspace ID**|Enter the unique ID of your Azure Log Analytics workspace|
| **Log Analytics Workspace Key**|Enter the primary or secondary key of your Azure Log Analytics workspace|

## Logic App design

Sceenshot from Logic App designer view for reference:

[![Log query](https://github.com/mikoiv/MicrosoftSentinel-ShodanMonitor/blob/main/Images/logicapp-designer-light.png)](https://github.com/mikoiv/MicrosoftSentinel-ShodanMonitor/blob/main/Images/logicapp-designer-light.png)
