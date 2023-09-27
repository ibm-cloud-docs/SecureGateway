---

copyright:
  years: 2023, 2023
lastupdated: "2023-09-27"

keywords: secure gateway, deprecation, migration, connector

subcollection: SecureGateway

content-type: tutorial
services: SecureGateway,satellite
account-plan: paid
completion-time: 1hr

---

{{site.data.keyword.attribute-definition-list}}


# Setting up Connector for testing
{: #testing-connector}
{: toc-content-type="tutorial"}
{: toc-services="SecureGateway,satellite"}
{: toc-completion-time="2hr"}

{{site.data.keyword.SecureGateway}} is deprecated. For more information, see the [deprecation details](/docs/SecureGateway?topic=SecureGateway-deprecation).
{: deprecated}

This tutorial is designed for {{site.data.keyword.SecureGateway}} administrators who are considering migrating to {{site.data.keyword.satelliteshort}} Connector.
{: shortdesc}

## Goals 
{: #testing-connector-goals}


The goal of this tutorial is to help guide you through setting up a {{site.data.keyword.satelliteshort}} Connector for testing run through testing to validate the data deliver.

## Gather your {{site.data.keyword.SecureGateway}} instance details
{: #testing-connector-goals}
{: step}

Before setting up Connector, make a note your {{site.data.keyword.SecureGateway}} instance details like the following.

- The region where your {{site.data.keyword.SecureGateway}} instance is deployed.
- The resource group in your account that contains your {{site.data.keyword.SecureGateway}} instances.
- **Optional**: The tags, if any, that you use to organize your account resources. ( see https://cloud.ibm.com/docs/account?topic=account-tag for more info on tags).

## Set up your permissions and firewall rules
{: #testing-connector-create-agent}
{: step}

1. Create an API key(s) with the following permissions.
    - To create a Connector, you need the **Administrator** Platform role for Satellite. - To connect an Agent to an existing Connector, you need the **Viewer** Platform role or the **Reader** Service role for Satellite.

1. need connector id and region id to connect to (from connector setup)

 - (optional) determine tags to attach to the agent instance to organize account ( as above, see https://cloud.ibm.com/docs/account?topic=account-tag for more info on tags)

 - open appropriate firewall rules if necessary to allow outgoing traffic from agent to connector : https://cloud.ibm.com/docs/satellite?topic=satellite-understand-connectors#network-requirements

 - configure proxy setup on host container engine, if needed for access: https://cloud.ibm.com/docs/satellite?topic=satellite-config-connector-proxy

instructions link: https://cloud.ibm.com/docs/satellite?topic=satellite-run-agent-locally

## Create a Connector
{: #testing-connector-create}
{: step}

1. [Create a Connector in the console](https://cloud.ibm.com/satellite/connectors/create){: external}.

1. Make a note of the connector ID and region ID. These values are used as inputs when you attach agents in the next steps.



{{../satellite/connector-create-endpoints.md#create-connector-endpoint-console}}

{{../satellite/connector-create-endpoints.md#create-connector-rule-console}}



