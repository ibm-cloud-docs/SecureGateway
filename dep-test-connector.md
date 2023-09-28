---

copyright: 
  years: 2023, 2023
lastupdated: "2023-09-28"

keywords: secure gateway, deprecation, migration, connector

subcollection: SecureGateway

content-type: tutorial
services: SecureGateway,satellite
account-plan: paid
completion-time: 1hr

---

{{site.data.keyword.attribute-definition-list}}


# Setting up Connector for testing Secure Gateway migration
{: #testing-connector}
{: toc-content-type="tutorial"}
{: toc-services="SecureGateway,satellite"}
{: toc-completion-time="2hr"}

{{site.data.keyword.SecureGateway}} is deprecated. For more information, see the [deprecation details](/docs/SecureGateway?topic=SecureGateway-dep-overview).
{: deprecated}

This tutorial is designed for {{site.data.keyword.SecureGateway}} administrators who are considering migrating to {{site.data.keyword.satelliteshort}} Connector.
{: shortdesc}

## Goals 
{: #testing-connector-goals}

The goal of this tutorial is to help guide you through setting up a {{site.data.keyword.satelliteshort}} Connector for testing as part of your migration away from {{site.data.keyword.SecureGateway}}.

## Make a note of your {{site.data.keyword.SecureGateway}} instance details
{: #testing-connector-details}
{: step}

Before setting up Connector, make a note your {{site.data.keyword.SecureGateway}} instance details like the following.

- The region where your {{site.data.keyword.SecureGateway}} instance is deployed.
- The resource group in your account that contains your {{site.data.keyword.SecureGateway}} instances.
- **Optional**: The tags, if any, that you use to organize your account resources.

## Set up your permissions
{: #testing-connector-create-agent}
{: step}

Create an API key(s) with the following permissions.

- To create a Connector, you need the **Administrator** Platform role for Satellite.
- To connect an Agent to an existing Connector, you need the **Viewer** Platform role or the **Reader** Service role for Satellite.


## Create a Connector
{: #testing-connector-create}
{: step}

1. [Create a Connector in the console](https://cloud.ibm.com/satellite/connectors/create){: external}. Make sure to use the same region and resource group that you found in the previous steps.

1. Make a note of the connector ID and region ID. These values are used as inputs when you attach agents in the next steps.


## Set up your firewall rules
{: #testing-connector-firewall}
{: step}

1. Before setting up an agent, open the required firewall rules, if necessary, to allow outgoing traffic from your agent to Connector. For more information, see [Networking requirements](/docs/satellite?topic=satellite-understand-connectors#network-requirements) for Connector.

1. **Optional**: [Configure a proxy setup on your host container engine](/docs/satellite?topic=satellite-config-connector-proxy).


## Run the agent image
{: #testing-connector-run-image}
{: step}

- Complete the [Running the Connector Agent image locally](/docs/satellite?topic=satellite-run-agent-locally) tutorial To begin testing Connector.
- You can also run the Agent image in [swarm mode for high-availability](/docs/satellite?topic=satellite-run-agent-swarm)
- You can also [try the end-to-end example](/docs/satellite?topic=satellite-end-to-end#create-link-endpoint) which also covers creating endpoints and setting up TLS.


## Set up your endpoints and ACLs
{: #testing-connector-endpoints-acls}
{: step}

1. Follow the steps to [create an endpoint in the console](/docs/satellite?topic=satellite-connector-create-endpoints#create-connector-endpoint-console).
1. After creating an endpoint, set up an [access control list](/docs/satellite?topic=satellite-connector-create-endpoints#create-connector-rule-console).



## Next steps
{: testing-connector-next}

- Try running the Agent image in [swarm mode](/docs/satellite?topic=satellite-run-agent-swarm)
- [Try the end-to-end Connector example](/docs/satellite?topic=satellite-end-to-end#create-link-endpoint) which also covers creating endpoints and setting up TLS.
- [Review the troubleshooting guide](/docs/satellite?topic=satellite-debug-connector).

