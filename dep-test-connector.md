---

copyright: 
  years: 2023, 2023
lastupdated: "2023-10-25"

keywords: secure gateway, deprecation, migration, connector

subcollection: SecureGateway

content-type: tutorial
services: SecureGateway,satellite
account-plan: paid
completion-time: 1hr

---

{{site.data.keyword.attribute-definition-list}}


# Setting up Connector
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

Before completing this tutorial, make sure you’ve done the key research needed to prepare for the migration to Satellite Connector.

- [Understand your migration options](/docs/SecureGateway?topic=SecureGateway-dep-migration-options).
- [Review Satellite Connector as a Secure Gateway replacement](/docs/SecureGateway?topic=SecureGateway-understanding-connector). Compared how Secure Gateway and Satellite Connector to understand their similar capabilities, and their different ways of doing things.
- [Analyze your Secure Gateway environment and review your deployment details](/docs/SecureGateway?topic=SecureGateway-dep-gather-sg-details) for the key information you need to set up Connector.
- Concurrent deployments. It is possible to set up the Satellite Connector infrastructure along side your Secure Gateway infrastructure and run them at the same time. The only detail you would need to understand is the data that is being sent over both the Secure Gateway client and the Satellite Connector agent. As mentioned before, you would need to ensure that parallel data being sent, perhaps duplicate data detection of some sort, is examined and handled properly. Or you could set some data to go over the Secure Gateway connection, and other data to go over the Satellite Connector connection.

Before continuing, make sure you have the following details from your Secure Gateway deployments. You will use this information when completing the following steps.

- **Region**: Create your Connector in the same region where your {{site.data.keyword.SecureGateway}} deployment was located.
- **Resource group**: Create your Connector in the same resource group where your {{site.data.keyword.SecureGateway}} deployment was located.
- **Gateways**: Recreate your {{site.data.keyword.SecureGateway}} Gateways as Satellite Connectors.
- **Destinations**: Recreate your {{site.data.keyword.SecureGateway}} destinations as Connector endpoints.
- **Protocol**: Use the same {{site.data.keyword.SecureGateway}} destination protocol value when you create your Connector endpoint.
- **Port**: Use the same port value from your {{site.data.keyword.SecureGateway}} destination as the `Destination port` when setting up your Connector endpoint.
- **Network Security**: Use your Secure Gateway Network Security settings when setting up your Connector endpoint access control lists (ACLs).
- **Authentication Files**: Get a `.zip` of your [authentication files](https://test.cloud.ibm.com/docs/SecureGateway?topic=SecureGateway-nodejs-tls-ma#tls-ma-download-files). You will upload these authentication files later when creating a Connector endpoint in the console.
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

This apikey can be shared between the agents connecting to a specific Connector.


## Create a Connector
{: #testing-connector-create}
{: step}

The first step is to set up the Satellite Connector. As explained in previous steps, the Satellite **Connector** is similar to each Secure Gateway **Gateway**. So if you had 1 Gateway, you will have 1 Connector. If you had 3 Gateways, you’ll have 3 Connectors. Complete the following steps to create a Connector.

1. Determine which IBM Cloud region you want to create your Connector in. Previously, you examined into which regions your Secure Gateway was deployed. You don't have to pick the same region, but you probably want to.

1. Determine the resource group. For more information about resource groups, see [Managing resource groups](/docs/account?topic=account-rgs&interface=ui). 

1. **Optional**: Determine any tags for the connector to organize account. For more information, see [Working with tags](/docs/account?topic=account-tag).

1. [Create a Connector in the console](https://cloud.ibm.com/satellite/connectors/create){: external} using the region, resource group, and tags you found in your Secure Gateway deployments.

After creating a Connector, make a note of the Connector ID and Region ID used here. They will be used later when you attach agents in step 3.

However, before setting up an agent, make sure your have the required firewall rules to allow data to pass through.


## Set up your firewall rules
{: #testing-connector-firewall}
{: step}

You will likely want to deploy the Satellite Connector **Agent** on the same host you had deployed your Secure Gateway **client**. They can both run on the same machine, it probably matches the layout you want, and once the Satellite Connector Agent is set up and tested, you can stop the Secure Gateway clients on that same host.

1. Before setting up an agent, open the required firewall rules, if necessary, to allow outgoing traffic from your agent to Connector. For more information, see [Networking requirements](/docs/satellite?topic=satellite-understand-connectors#network-requirements) for Connector.

1. **Optional**: [Configuring a proxy for your Satellite Connector](/docs/satellite?topic=satellite-config-connector-proxy).


## Run the agent image
{: #testing-connector-run-image}
{: step}

Connector agents are similar to Secure Gateway clients. When you set up your agents, you will enter the Connector ID and region ID that you retrieved in step 2 when you created the Connector.


- Complete the [Running the Connector Agent image](/docs/satellite?topic=satellite-run-agent-locally) tutorial to begin testing Connector.
- You can also run the Agent image in [swarm mode for high-availability](/docs/satellite?topic=satellite-run-agent-swarm)
- You can also [try the end-to-end example](/docs/satellite?topic=satellite-end-to-end#create-link-endpoint) which also covers creating endpoints and setting up TLS encryption.


## Set up your endpoints and ACLs
{: #testing-connector-endpoints-acls}
{: step}

1. Follow the steps to [create an endpoint in the console](/docs/satellite?topic=satellite-connector-create-endpoints#create-connector-endpoint-console). Make sure to use the Secure Gateway deployment details you found earlier as inputs when creating your endpoints.
1. After creating an endpoint, set up an [access control list](/docs/satellite?topic=satellite-connector-create-endpoints#create-connector-rule-console). ACLs are similar to your Secure Gateway Network Security options, so if you want, set the ACLs up similar to what you collected from the Secure Gateway deployment.


## Deployment Validation
{: #testing-connector-setup-validation}
{: step}
Although you were able to validate and test each part of the setup during each previous steps, let's review them to ensure that the parity between Secure Gateway and Satellite Connector matches. At the very end, you want to know that all data that was getting transferred via Secure Gateway, is now getting transferred via Satellite Connector.

1.  Validate your Satellite Connector and running agents.

    - From the console.
        1. [From the console](https://cloud.ibm.com/satellite/connectors), click **Connectors**, then click the Connector you created earlier.
        1. Check the status. If the status is not **Online**, hover over the status for more information.
        1. Click the **Active agents** tab to verify your agent is connected.

    - From the CLI.
        1. Verify the running agent in the CLI by running the following commands on the system where the agent is running.
            ```sh
            podman ps -a
            ```
            {: pre}

            Example output of a running container.

            ```sh
            CONTAINER ID  IMAGE                                                            COMMAND               CREATED         STATUS             PORTS               NAMES
            30957e99d68a  icr.io/ibm/satellite-connector/satellite-connector-agent:latest                        26 minutes ago  Up 26 minutes ago                      happy_cray
            ```
            {: screen}

        1. Check the logs inside the agent for errors. Replace `CONTAINER-ID` with the CONTAINER ID in the output from the previous step.
            ```sh
            podman logs CONTAINER-ID
            ```
            {: pre}

        1. If the pod is not running, check logs from the last run to see why it failed.

        ```sh
        podman logs -p 30957e99d68a
        ```
        {: pre}

        A common problem is incorrect environment variables in your agent set up. To review the variables, see [setting up the agents](https://cloud.ibm.com/docs/satellite?topic=satellite-run-agent-locally).
        {: note}

1. Validate an endpoint.
    1. [From the console](https://cloud.ibm.com/satellite/connectors), click **Connectors**, then click the Connector you created earlier.
    1. Click the **Endpoints** tab to view your endpoints and verify that it is `Enabled`.
    1. Click the endpoint for more informaton about the traffic throughput and connection count.

1. Validate your data throughput. The final step to validating the correct function of transfered data is for you to actually verify that the data you are attempting to transfer via Connector is arriving in the final location. This involves what you determined during your Secure Gateway analysis and ends when the end target of the received data, you can see it having arrive. This will be highly dependent on what is receiving this data, and the tools available to examine that data.



## Next steps
{: testing-connector-next}

Now that you have setup and validated Satellite Connector as a Secure Gateway replacement, continue your migration or finalize all needed steps. For more information about Connector, view the following links.

- Repeat the steps for additional Connectors and Gateways. This tutorial has walked you through setting up a single Satellite Connector, and matches the Secure Gateway concept of a "Gateway", with data passing through it. If you have more Gateways from Secure Gateway, you will want to repeat these steps until all the data that went through Gateways is now going through a Satellite Connector.
- Try running the Agent image in [swarm mode for high-availability](/docs/satellite?topic=satellite-run-agent-swarm)
- [Try the end-to-end Connector example](/docs/satellite?topic=satellite-end-to-end#create-link-endpoint) which also covers creating endpoints and setting up TLS.
- [Review the troubleshooting guide](/docs/satellite?topic=satellite-debug-connector).


After you set up Satellite Connector to replicate your Secure gateway deployment, the final steps are to stop your Secure Gateway clients. Then, disable and delete your Secure Gateway gateways. When that is done your migration is complete.
