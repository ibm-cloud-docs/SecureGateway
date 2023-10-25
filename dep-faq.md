---

copyright: 
  years: 2023, 2023
lastupdated: "2023-10-25"

keywords: secure gateway, deprecation, migration, faq, questions

subcollection: SecureGateway


---

{{site.data.keyword.attribute-definition-list}}

# Deprecation and migration FAQ
{: #dep-faq}



Review the following frequently asked questions about the {{site.data.keyword.SecureGateway}} Deprecation and migration steps.
{: shortdesc}

## What is the timeline for deprecation?
{: #faq-timeline}

Review the [Overview and timeline](/docs/SecureGateway?topic=SecureGateway-dep-overview) for dates and details.

## Is there a quick overview that compares the two solutions?
{: #faq-compare}

Yes! For more information, see [Reviewing {{site.data.keyword.satelliteshort}} Connector as a {{site.data.keyword.SecureGateway}} replacement](/docs/SecureGateway?topic=SecureGateway-understanding-connector).


{{../satellite/connector-faq.md#connector-faq-endpoints}}


{{../satellite/connector-faq.md#connector-faq-restrict}}


{{../satellite/connector-faq.md#connector-faq-acl-implementation}}


{{../satellite/connector-faq.md#conector-faq-permissions}}


{{../satellite/connector-faq.md#connector-faq-con-per-region}}

{{../satellite/connector-faq.md#connector-faq-endpoints-per-conn}}

{{../satellite/connector-faq.md#connector-faq-instance-limits}}


## What is the recommended approach for access across multiple resource groups?
{: #dep-faq-multi-rg}

For example, your environment is set up in one account with three resource groups: one is for development, another for staging, and the final one for production. Should you create a single Satellite Connector instance or multiple (that is, one for each resource group)? If you can create multiple Connectors, are there any considerations for reusing a Node.js application to create a connector and endpoint in each space?

You can create a single Connector with endpoints that are accessible across all of your resource groups.  For the example case, it could be appropriate to create separate endpoints linking to different services for each: development, staging, production, and then locking down access to each of those to their respective RG instances via ACLs.  There is no binding required, simply set the endpoint hostname:port to use the correct end instance.

## What is the recommended approach for access across multiple accounts?
{: #dep-faq-multi-account}

Consider an environment that has two separate accounts: one for development and staging, and a second for production.

## Is a Satellite Connector required for each account and is the configuration available to all resource groups within that account?
{: #dep-faq-multi-account-availablility}

You can create a single Connector with endpoints that are accessible across accounts within IBM Cloud, as well as across resource groups. For the example case, it could be appropriate to setup a connector in the dev/staging account with a separate endpoint for each (locked down via ACLs), and a second one in the production account with its own endpoint, also locked down via ACL.  Depending on the desires for network management, it could also be appropriate to put a single connector in the production account with endpoints for all three, and again lock down access to each via the ACLs for each.  The first option has better built in isolation; the second has less moving parts to be managed.

## Can a Satellite Connector agent connect to more than one connector from the same machine?
{: #dep-faq-connector-machine}

A single Connector agent can only connect to a single connector. However, you can run multiple connector agents in separate containers up to the cpu/memory/network limitations on your host, each connecting to its own specified connector, and exposing that connector as desired via a chosen port through the container engine (the local ports as exposed cannot be the same).

## Does Connector support public endpoints?
{: #dep-faq-endpoint-public}

No, as mentioned in [Creating and managing Connector endpoints](/docs/satellite?topic=satellite-connector-create-endpoints#create-connector-rule-console), endpoints are only accessbile through the IBM Cloud private network.


