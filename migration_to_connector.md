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

In general, {{site.data.keyword.satelliteshort}} Connector has a number of improvements over {{site.data.keyword.SecureGateway}}.

- Supports the latest generation of VPC networking.
- Supports only cloud private endpoints.
- Supports several integrations including standard IBM Cloud tools like Activity Tracker, LogDNA, and Sysdig.
- Supports more concurrent incoming connections than {{site.data.keyword.SecureGateway}}.
- Supports a higher number of client connections for client-side HA purposes.
- Supports server-side HA for increased reliability and uptime.
- Requires fewer exposed firewall ports which reduces need for proxy work arounds for very restrictive customer firewalls.
- Supports new protocol for endpoints: HTTP-Tunnel (in addition to TCP/TLS/HTTP/HTTPS).
- Has no bandwidth egress limits.

Review the following table for more information and a comparison of capabilities between {{site.data.keyword.satelliteshort}} Connector and {{site.data.keyword.SecureGateway}}.

{{../satellite/connector-and-secure-gateway.md#connector-capabilities-table}}


## Review the requirements and FAQs
{: #migration_next}
{: step}

1. [Review the Connector overview and requirements](/docs/satellite?topic=satellite-understand-connectors).
1. [Review the Connector FAQs](/docs/satellite?topic=satellite-connector-faq).



## Next steps
{: #migration_next}

Continue your evaluating and preparing for your migration by Reviewing your current {{site.data.keyword.SecureGateway}} setup.


