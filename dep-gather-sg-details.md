---

copyright: 
  years: 2023, 2023
lastupdated: "2023-09-27"

keywords: secure gateway, deprecation, migration

subcollection: SecureGateway

---

{{site.data.keyword.attribute-definition-list}}


# Reviewing your {{site.data.keyword.SecureGateway}} instance details
{: #dep-gather-sg-details}

{{site.data.keyword.SecureGateway}} is deprecated. For more information, see the [deprecation details](/docs/SecureGateway?topic=SecureGateway-deprecation).
{: deprecated}

You can use the following steps to analyze the current usage of {{site.data.keyword.SecureGateway}} for your secure data communications and collect the necessary information to create corresponding {{site.data.keyword.satelliteshort}} Connectors.
{: shortdesc}


## Goals 
{: #testing-connector-goals}

The goal of this tutorial is to help guide you through gathering the key information from your {{site.data.keyword.SecureGateway}} instances that you will need when you migrate to {{site.data.keyword.satelliteshort}} Connector. 

## Review the {{site.data.keyword.SecureGateway}} terms
{: #testing-connector-details}
{: step}

You might need to review the common terms and concepts of {{site.data.keyword.SecureGateway}}. For more information, see the following links.

- Review the [Getting started with {{site.data.keyword.SecureGateway}}](/docs/SecureGateway?topic=SecureGateway-getting-started-with-sg)
- Review the [Secure Gateway service instances](https://cloud.ibm.com/catalog/services/secure-gateway) and the versions you have.
- Review [Gateways](/docs/SecureGateway?topic=SecureGateway-add-sg-gw).
- Review [Clients](/docs/SecureGateway?topic=SecureGateway-add-client).
- Review [Destinations](/docs/SecureGateway?topic=SecureGateway-add-dest).


## Access your {{site.data.keyword.SecureGateway}} instances
{: #testing-connector-details}
{: step}

1. Access your {{site.data.keyword.SecureGateway}} instances from the [Resource list in the console](https://cloud.ibm.com/resources?product=Secure){: external}.

1. For each {{site.data.keyword.SecureGateway}} instances in your account, review your Gateways, Destinations, and connection details in the console. Make sure to save the information for later when you create {{site.data.keyword.satelliteshort}} Connectors.

    1. Every {{site.data.keyword.satelliteshort}} Connector is functionally similar to each Gateway. So you might have multiple Secure Gateway instances, and you might have multiple Gateway destinations set up. You will create a Satellite Connector for each of the Secure Gateway Destinations you have set up.
    1. Select each destination in turn and click on the **Gear** icon to view the details for that destination and collect the necessary data for creating an equivalent Connector endpoint.
    1. Review the example screenshot. 
    1. From this window, collect the **Resource Host** and **Port**, and use the **Download Authentication Files** button to pull any necessary certificates and keys for doing authentication in the connection.
    1. You will also need to go click the **Edit** button and check the following sections
        - **Network Security** to get the ip restrictions for Connector ACL setup.
        - **Proxy Options** to get the required proxy setup that will be needed for the docker agents to access the onsite service.
        - **Miscellaneous Options** to get the connection timeout setting.

You will repeat the previous steps for each Gateway and each destination in your Secure Gateway instance. Keep all of this information ready for when you set up your Connectors, Agents, and Endpoints in the next tutorial.



