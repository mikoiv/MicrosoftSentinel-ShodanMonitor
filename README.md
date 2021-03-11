# AzureSentinel-ShodanMonitor

## Background

This is a proof-of-concept for an Azure Sentinel Playbook for ingesting Shodan Monitor alerts.

The solution uses a Logic App HTTP endpoint that listens to incoming requests from the Shodan Monitor "Notification Webhook". The Webhook sends an HTTP POST request that has alert metadata in the HTTP headers and Shodan database based host data in the body.

The Notification Webhook sends a lot of data, but for now we only gather the following:
* SHODAN-ALERT-ID, SHODAN-ALERT-NAME, SHODAN-ALERT-TRIGGER (headers)
* Hostname, IP Address, Port (body)

You can the official data description in https://help.shodan.io/developer-fundamentals/monitor-webhooks

As you can see, this solution is extremely simple. I am sure the Logic App could be written with better code, for a production setup you would want to think about the following:
* Map the Shodan JSON schema based on your needs
* Include more data 
* Think about error handling
* Harden the solution for your security needs

In this proof of concept security and authentication is just based on making sure you transfer the custom Logic App HTTP listener URL to Shodan in a secure manner.

## How does it look like currently

Here is a visualization from the Logic App designer:

[![Designer](https://github.com/mikoiv/AzureSentinel-ShodanMonitor/blob/master/LogicApp_designer_view.png)](https://github.com/mikoiv/AzureSentinel-ShodanMonitor/blob/master/LogicApp_designer_view.png)

## How-to

I created the solution as a quick test in the Logic App UI, so no automation template for you just yet.




