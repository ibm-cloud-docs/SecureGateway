---

copyright:
  years: 2014, 2023
lastupdated: "2023-09-20"

keywords: secure gateway, deprecation, migration

subcollection: SecureGateway

content-type: tutorial
services: SecureGateway,satellite
account-plan: paid
completion-time: 2hr

---

{{site.data.keyword.attribute-definition-list}}


# Reviewing {{site.data.keyword.satelliteshort}} Connector as a {{site.data.keyword.SecureGateway}} replacement
{: #understanding-connector}
{: toc-content-type="tutorial"}
{: toc-services="SecureGateway,satellite"}
{: toc-completion-time="2hr"}

{{site.data.keyword.SecureGateway}} is deprecated. For more information, see the [deprecation details](/docs/SecureGateway?topic=SecureGateway-deprecation).
{: deprecated}

This tutorial is designed for {{site.data.keyword.SecureGateway}} administrators who are considering migrating to {{site.data.keyword.satelliteshort}} Connector.
{: shortdesc}


## Goals
{: #migration_goals}


- Introduce you to the key {{site.data.keyword.satelliteshort}} Connector concepts.
- Cover frequently asked questions about {{site.data.keyword.satelliteshort}} Connector
- Provide a terminology mapping, so that you can learn more about Connector through familiar {{site.data.keyword.SecureGateway}} terms.
- Explain how the {{site.data.keyword.satelliteshort}} Connector features compare to {{site.data.keyword.SecureGateway}}.


## Learn the concepts
{: #migration_concepts}
{: step}



Connector {: #term-connector}
:   A connector provides a secure connection between a specific remote location and {{site.data.keyword.cloud_notm}}.
  
Agent {: #term-agent}
:   Each connector needs an agent running on your location to establish the connection.
  
Endpoint {: #term-endpoint}
:   An endpoint allows you to securely connect to a server, service, or app that runs in your {{site.data.keyword.satelliteshort}} location from a client that is connected to the {{site.data.keyword.cloud_notm}} private network.
  
Access control list {: #term-acl}
:   Access control list (ACL) controls which clients can access location endpoint resources. You can create ACL rules and use them to control which clients can use the endpoint to connect to the destination resource that runs in your location.

## Compare the terms
{: #migration_terms}
{: step}

{{../satellite/connector-and-secure-gateway.md#connector-sg-comp-table}}

## Compare the capabilities
{: #migration_compare}
{: step}

{{../satellite/connector-and-secure-gateway.md#connector-capabilities-table}}


## Review the requirements and FAQs
{: #migration_next}

1. [Review the Connector overview and requirements](/docs/satellite?topic=satellite-understand-connectors).
1. [Review the Connector FAQs](/docs/satellite?topic=satellite-connector-faq).



