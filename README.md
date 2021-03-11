# AzureSentinel-ShodanMonitor

## Background

This is a proof-of-concept for ingesting Shodan Monitor alerts to Azure Sentinel.

Shodan Monitor is a service for Shodan subscribers that can detect the following alerts in any public endpoints, hosts or addresses you configure for monitoring:
* Services associated with ICS or IOT devices
* Compromised or malware-related services
* New open ports, uncommon open ports
* Open databases
* Known vulnerabilities
* Expired certificates

Shodan Monitor has a Notification service that can send these alerts via email, instant messaging or a Notification Webhook.

The Notification Webhook sends an HTTP POST request every time it detects an alert that matches the alert triggers you have configured in Shodan Monitor. The request contains alert metadata in the HTTP headers and Shodan database based host data in the body.

This solution provides a method of ingesting the Notification Webhook alerts to Azure Sentinel for alerting and hunting. It uses a Logic App HTTP endpoint that listens to incoming requests, parses a selected amount of the alert data into variables and writes those to the Log Analytics Workspace.

You can the official Notification Webhook documentation here: https://help.shodan.io/developer-fundamentals/monitor-webhooks

## Current status

At the moment the solution only gathers the following data:
* SHODAN-ALERT-ID, SHODAN-ALERT-NAME, SHODAN-ALERT-TRIGGER (headers)
* Hostname, IP Address, Port (body)

Reason for the simplicity is that at the moment I don't have enough time to learn certain things about Logic Apps that are still unfamiliar for me. For example I am unsure on the best way to combine the header and post data, instead of creating individual variables via two different methods. 

Another problem I faced is that the Webhook request does not have Content-Type set to JSON, which is something that Logic App expects. This gave me some trouble when I tried to parse the full body schema in a cleaner way.

Here is a visualization from the current version in Logic App designer:

[![Designer](https://github.com/mikoiv/AzureSentinel-ShodanMonitor/blob/main/LogicApp_designer_view.png)](https://github.com/mikoiv/AzureSentinel-ShodanMonitor/blob/main/LogicApp_designer_view.png)

After creating the Logic App, you can find the custom listener URL from the first Logic App action:

[![URL](https://github.com/mikoiv/AzureSentinel-ShodanMonitor/blob/main/LogicApp_URL.png)](https://github.com/mikoiv/AzureSentinel-ShodanMonitor/blob/main/LogicApp_URL.png)

You can configure a new Notification Webhook in Shodan Monitor configuration based on the URL:

[![ShodanConfig](https://github.com/mikoiv/AzureSentinel-ShodanMonitor/blob/main/Shodan_configuration.png)](https://github.com/mikoiv/AzureSentinel-ShodanMonitor/blob/main/Shodan_configuration.png)

Make sure you enable this notification method when you add and configure your assets in Shodan Monitor. 

Depending on your assets and environment you will start getting new alerts sooner or later. A good way to test is to add new networks that you know will generate findings. Shodan Monitor also has a Test Webhook action, but be aware that the test will not contain any data so you will not see much in Sentinel, but you can use it to make sure the basic logic works if you want.

## Results in Sentinel

Here is a screenshot of the current data model in Azure Sentinel:

[![Sentinel](https://github.com/mikoiv/AzureSentinel-ShodanMonitor/blob/main/Sentinel_log_query.png)](https://github.com/mikoiv/AzureSentinel-ShodanMonitor/blob/main/Sentinel_log_query.png)

Even with this base amount of data there are decent use cases for correlating, alerting or hunting for things related to your your public security posture and attack surface with Azure Sentinel.

## Development needs

As you can see, this solution is extremely simple. I am sure the Logic App could be written with better code, for a production setup you would want to think about the following:
* Handle the header and body data in a more robust way
* Map the full Shodan JSON schema to fetch the hostname, vulnerability data, country codes etc
* Think about error handling
* Harden the solution for your security needs

In this proof-of-concept security and authentication is just based on making sure you transfer the custom Logic App HTTP listener URL to Shodan in a secure manner. This can be developed further with the Logic App or even creating a API Management service to act as a secure front-end for your Logic App HTTP listeners.

I created the solution as a quick test in the Logic App UI, so no automation template for you just yet. Manually exported code from the Logic App is included, so you can have a look at that.




