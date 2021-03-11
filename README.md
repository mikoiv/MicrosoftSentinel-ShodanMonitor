# AzureSentinel-ShodanMonitor

## Background

This is a proof-of-concept for an Azure Sentinel Playbook for ingesting Shodan Monitor alerts.

The solution uses a Logic App HTTP endpoint that listens to incoming requests from the Shodan Monitor "Notification Webhook". 

The Notification Webhook is a configuration in Shodan Monitor, that sends an HTTP POST request every time it detects an alert that matches to the rules you have set up in Monitor. The request contains alert metadata in the HTTP headers and Shodan database based host data in the body.

You can the official data description in https://help.shodan.io/developer-fundamentals/monitor-webhooks


## Current status

At the moment the solution only gathers the following data:
* SHODAN-ALERT-ID, SHODAN-ALERT-NAME, SHODAN-ALERT-TRIGGER (headers)
* Hostname, IP Address, Port (body)

Reason for the simplicity is that at the moment I don't have enough time to learn some things about Logic Apps that are yet unfamiliar for me. For example I am unsure on how to combine the header and post data in a unified way, instead of creating individual variables. 

Another problem I faced is that the Webhook request does not have Content-Type set to JSON, which is something that Logic App expects. This gave me some trouble when I tried to parse the full body schema in a cleaner way.

Here is a visualization from the Logic App designer:

[![Designer](https://github.com/mikoiv/AzureSentinel-ShodanMonitor/blob/main/LogicApp_designer_view.png)](https://github.com/mikoiv/AzureSentinel-ShodanMonitor/blob/main/LogicApp_designer_view.png)

## Develoment

As you can see, this solution is extremely simple. I am sure the Logic App could be written with better code, for a production setup you would want to think about the following:
* Handle the header and body data in a more robust way
* Map the full Shodan JSON schema 
* Think about error handling
* Harden the solution for your security needs

In this proof of concept security and authentication is just based on making sure you transfer the custom Logic App HTTP listener URL to Shodan in a secure manner.

I created the solution as a quick test in the Logic App UI, so no automation template for you just yet.




