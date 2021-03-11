# AzureSentinel-ShodanMonitor

## Background

This is a proof-of-concept for an Azure Sentinel Playbook for ingesting Shodan Monitor alerts.

Shodan Monitor is a solution that can detect the following alerts in any public endpoints, hosts or addresses you configure it to monitor:
* Services associated with ICS or IOT devices
* Compromised or malware-related services
* New open ports, uncommon open ports
* Open databases
* Known vulnerabilities
* Expired certificates

Shodan Monitor has a Notification service that can send these alerts via email, instant messaging or a Notification Webhook.

The Notification Webhook sends an HTTP POST request every time it detects an alert that matches the rules you have set up in Monitor. The request contains alert metadata in the HTTP headers and Shodan database based host data in the body.

This solution relies on the Notification Webhook to integrate Shodan Monitor to Azure Sentinel. It uses a Logic App HTTP endpoint that listens to incoming requests, parses a selected amount of data into variables and writes that to an Azure Log Analytics Workspace.

You can the official Notification Webhook documentation here: https://help.shodan.io/developer-fundamentals/monitor-webhooks

## Current status

At the moment the solution only gathers the following data:
* SHODAN-ALERT-ID, SHODAN-ALERT-NAME, SHODAN-ALERT-TRIGGER (headers)
* Hostname, IP Address, Port (body)

Reason for the simplicity is that at the moment I don't have enough time to learn some things about Logic Apps that are yet unfamiliar for me. For example I am unsure on the best way to combine the header and post data, instead of creating individual variables via two different methods. 

Another problem I faced is that the Webhook request does not have Content-Type set to JSON, which is something that Logic App expects. This gave me some trouble when I tried to parse the full body schema in a cleaner way.

Here is a visualization from current version in Logic App designer:

[![Designer](https://github.com/mikoiv/AzureSentinel-ShodanMonitor/blob/main/LogicApp-DesignerView.png)](https://github.com/mikoiv/AzureSentinel-ShodanMonitor/blob/main/LogicApp-DesignerView.png)

## Development needs

As you can see, this solution is extremely simple. I am sure the Logic App could be written with better code, for a production setup you would want to think about the following:
* Handle the header and body data in a more robust way
* Map the full Shodan JSON schema to fetch the hostname, vulnerability data, country codes etc
* Think about error handling
* Harden the solution for your security needs

In this proof of concept security and authentication is just based on making sure you transfer the custom Logic App HTTP listener URL to Shodan in a secure manner.

I created the solution as a quick test in the Logic App UI, so no automation template for you just yet. Manually exported code from the Logic App is included, so you can have a look at that.




