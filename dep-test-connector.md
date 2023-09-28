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

## Prerequisites
{: #test-connector-prereqs}

Before completeing this tutorial, make sure you’ve done the key research needed to prepare for the migration to Satellite Connector.

- [Understand your migration options](/docs/SecureGateway?topic=SecureGateway-dep-migration-options).
- [Review Satellite Connector as a Secure Gateway replacement](/docs/SecureGateway?topic=SecureGateway-understanding-connector). Compared how Secure Gateway and Satellite Connector to understand their similar capabilities, and their different ways of doing things.
- [Analyze your Secure Gateway environment and review your deployment details](/docs/SecureGateway?topic=SecureGateway-dep-gather-sg-details) for the key information you need to set up Connector.

Before continuing, make sure you have the following details from your Secure Gateway deployments. You will use this information when completing the following steps.

- **Region**: Create your Connector in the same region where your {{site.data.keyword.SecureGateway}} deployment was located.
- **Resource group**: Create your Connector in the same resource group where your {{site.data.keyword.SecureGateway}} deployment was located.
- **Gateways**: Recreate your {{site.data.keyword.SecureGateway}} Gateways as Satellite Connectors.
- **Destinations**: Recreate your {{site.data.keyword.SecureGateway}} destinations as Connector endpoints.
- **Protocol**: Use the same {{site.data.keyword.SecureGateway}} destination protocol value when you create your Connector endpoint.
- **Port**: Use the same port value from your {{site.data.keyword.SecureGateway}} destination as the `Destination port` when setting up your Connector endpoint.
- **Authentication Files** - Get a `.zip` file of your [authenticaion files](https://test.cloud.ibm.com/docs/SecureGateway?topic=SecureGateway-nodejs-tls-ma#tls-ma-download-files). You will upload these {{site.data.keyword.SecureGateway}} authentication files when creating a Connector endpoint in the console when prompted to `Upload certificate`.
- **Clients**: The client establishes the initial connection between the on-premises network and a gateway on the Secure Gateway servers and allows for communication to pass through to the defined destinations. Use your client details to create Connector agents.
- **Proxy**: Include your {{site.data.keyword.SecureGateway}} proxy settings in your Dockerfile when configuring a proxy for your Satellite Connector.


## Goals 
{: #testing-connector-goals}

The goal of this tutorial is to help guide you through setting up a {{site.data.keyword.satelliteshort}} Connector for testing as part of your migration away from {{site.data.keyword.SecureGateway}}.


## Set up your permissions
{: #testing-connector-create-agent}
{: step}

For security, {{site.data.keyword.satelliteshort}} uses API keys and IAM roles to manage access the resources in your account.

Create an IAM API key(s) with the following permissions.

- To create a Connector, you need the **Administrator** Platform role for Satellite.
- To connect an Agent to an existing Connector, you need the **Viewer** Platform role or the **Reader** Service role for Satellite.


## Create a Connector
{: #testing-connector-create}
{: step}



1. Determine which IBM Cloud region you want to create your Connector in. Previously, you examined which regions your Secure Gateway deployment was in.

1. Determine the resource group. For more information about resource groups, see [Managing resource groups](/docs/account?topic=account-rgs&interface=ui).

1. **Optional**: Determine any tags for the connector to organize account  ( see https://cloud.ibm.com/docs/account?topic=account-tag for more info on tags)

1. [Create a Connector in the console](https://cloud.ibm.com/satellite/connectors/create){: external} using the region, resource group, and tags you found in your Secure Gateway deployments.

1. After creating a Connector, make a note of the connector ID and region ID used here. They will be required to attach agents in the next step.

1. Now that you have a Connector, the next step is to attach the agents so they can pass data through.




## Set up your firewall rules
{: #testing-connector-firewall}
{: step}

1. Before setting up an agent, open the required firewall rules, if necessary, to allow outgoing traffic from your agent to Connector. For more information, see [Networking requirements](/docs/satellite?topic=satellite-understand-connectors#network-requirements) for Connector.

1. **Optional**: [Configuring a proxy for your Satellite Connector](/docs/satellite?topic=satellite-config-connector-proxy).


## Run the agent image
{: #testing-connector-run-image}
{: step}

- Complete the [Running the Connector Agent image](/docs/satellite?topic=satellite-run-agent-locally) tutorial to begin testing Connector.
- You can also run the Agent image in [swarm mode for high-availability](/docs/satellite?topic=satellite-run-agent-swarm)
- You can also [try the end-to-end example](/docs/satellite?topic=satellite-end-to-end#create-link-endpoint) which also covers creating endpoints and setting up TLS encryption.


## Set up your endpoints and ACLs
{: #testing-connector-endpoints-acls}
{: step}

1. Review your Secure Gateway destination details before beginning.

    - **Region**: Create your Connector in the same region where your {{site.data.keyword.SecureGateway}} deployment was located.
- **Resource group**: Create your Connector in the same resource group where your {{site.data.keyword.SecureGateway}} deployment was located.
- **Gateways**: Recreate your {{site.data.keyword.SecureGateway}} Gateways as Satellite Connectors.
- **Destinations**: Recreate your {{site.data.keyword.SecureGateway}} destinations as Connector endpoints.
- **Protocol**: Use the same {{site.data.keyword.SecureGateway}} destination protocol value when you create your Connector endpoint.
- **Port**: Use the same port value from your {{site.data.keyword.SecureGateway}} destination as the `Destination port` when setting up your Connector endpoint.
- **Authentication Files** - Get a `.zip` file of your [authenticaion files](https://test.cloud.ibm.com/docs/SecureGateway?topic=SecureGateway-nodejs-tls-ma#tls-ma-download-files). You will upload these {{site.data.keyword.SecureGateway}} authentication files when creating a Connector endpoint in the console when prompted to `Upload certificate`.
- **Clients**: The client establishes the initial connection between the on-premises network and a gateway on the Secure Gateway servers and allows for communication to pass through to the defined destinations. Use your client details to create Connector agents.
- **Proxy**: Include your {{site.data.keyword.SecureGateway}} proxy settings in your Dockerfile when configuring a proxy for your Satellite Connector.

1. Follow the steps to [create an endpoint in the console](/docs/satellite?topic=satellite-connector-create-endpoints#create-connector-endpoint-console). Make sure to use the Secure Gateway deployment details you found earlier as inputs when creating your endpoints.
1. After creating an endpoint, set up an [access control list](/docs/satellite?topic=satellite-connector-create-endpoints#create-connector-rule-console).



## Next steps
{: testing-connector-next}

Now that you have tested Satellite Connector as a Secure Gateway replacement, continue your migration or finalize your set. For more information about Connector, view the following links.

- Try running the Agent image in [swarm mode](/docs/satellite?topic=satellite-run-agent-swarm)
- [Try the end-to-end Connector example](/docs/satellite?topic=satellite-end-to-end#create-link-endpoint) which also covers creating endpoints and setting up TLS.
- [Review the troubleshooting guide](/docs/satellite?topic=satellite-debug-connector).

