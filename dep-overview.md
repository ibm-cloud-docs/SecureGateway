---

copyright: 
  years: 2023, 2024
lastupdated: "2024-03-05"

keywords: secure gateway, deprecation, migration

subcollection: SecureGateway

---

{{site.data.keyword.attribute-definition-list}}


# Overview and timeline
{: #dep-overview}

{{site.data.keyword.cloud}} is announcing the full deprecation of {{site.data.keyword.SecureGateway}}. See the deprecation dates, details, and specific implications.
{: shortdesc}

{{site.data.keyword.SecureGateway}} has provided secure communications for many IBM Cloud customers and workloads over many years. But as technology moves on, newer and more sophisticated technologies have become available for our {{site.data.keyword.cloud}} users. Details below will walk you through those details, migration steps, and reach out for assitance.


## Timeline
{: #deprecation_timeline}

The following table describes the details of the deprecation, possible migration targets for your applications, and additional information.

| Date | Stage | Description |
| --- | --- | --- |
| 26 October 2023 | Deprecation announcement |  Announcement of the {{site.data.keyword.SecureGateway}} deprecation. All current {{site.data.keyword.SecureGateway}} users will receive an announcement email with information about the deprecation. Notification details will be put into the console and related screens. New deployments for the Secure Gateway service have been discontinued - Existing IBM Cloud products that use Secure Gateway as part of a bundled solution will continue to be able to deploy Secure Gateway as part of their process. Existing users as of this date will continue to be able to deploy Secure Gateway instances as needed for updates and replacement. For more information, see [Deprecation details](#deprecation_details). |
| Ongoing | Reminders | Periodic reminders will be sent to all users with running {{site.data.keyword.SecureGateway}} instances that the end of support date is coming.
| 26 October 2024 | End of support and End of life | {{site.data.keyword.SecureGateway}} end of support. All remaining Secure Gateway instances will be deprovisioned and deleted. |


## {{site.data.keyword.SecureGateway}} deprecation change log
{: #deprecation_changelog}

November 2023
- All user instances have been converted to resource group service connections.

December 2023
- All user instance gateways have been disable that had no usage since November 2023.

January 2024
- Satellite Connector now supports a new agent deployment method in Windows, allowing it to be run as an installer, not requiring direct Docker interaction calls. Follow the instructions in [Running the agent on Windows](/docs/satellite?topic=satellite-run-agent-locally#run-agent-windows) to install and connect through that method.

March 2024
- Reminder to all users with recent usage.


## {{site.data.keyword.SecureGateway}} deprecation details
{: #deprecation_details}

Deprecation is a process, and we've announced the beginning of that process. It begins with the announcement and continues until the end of support.

- No other IBM Cloud service or product will be affected other than {{site.data.keyword.SecureGateway}}
- Be sure to migrate to another method of secure data communication before the end of support date.
- Reminders of the approaching "end of support" date will continue to be sent periodically by email to all users that have {{site.data.keyword.SecureGateway}} instances running. Those reminder emails will end when all the {{site.data.keyword.SecureGateway}} instances in your account are deleted.
- After the end of support date, any remaining {{site.data.keyword.SecureGateway}} instances will be permanently disabled and de-provisioned by the {{site.data.keyword.SecureGateway}} team. You must assume your {{site.data.keyword.SecureGateway}} instances will be no longer available at the end of support time.

## Migration options
{: #dep-migration-options}

See [Reviewing your migration options](/docs/SecureGateway?topic=SecureGateway-dep-migration-options) for an overview and comparison of {{site.data.keyword.satellitelong}} Connector and a Virtual Private Network.

## Migration steps
{: #dep-migration-steps}

Of the listed migration options, the most similar to {{site.data.keyword.SecureGateway}} is {{site.data.keyword.satellitelong}} Connector. So here is a phased approach to moving to that solution.

Phase 1: Investigate {{site.data.keyword.satellitelong}} Connector versus {{site.data.keyword.SecureGateway}}
:   Understanding the product {{site.data.keyword.satellitelong}}, and its capability {{site.data.keyword.satellitelong}} Connector, is the first key step. This section [Reviewing Satellite Connector as a Secure Gateway replacement](/docs/SecureGateway?topic=SecureGateway-understanding-connector) will familiar you with that product. It will also explain through the differences and similarities between the two products. When done with this phase, you should understand the new product, how it relates to the existing {{site.data.keyword.SecureGateway}} product, and key information moving forward.

Phase 2: Reviewing your {{site.data.keyword.SecureGateway}} deployment details
:   Some users may know everything they need to know about how their {{site.data.keyword.SecureGateway}} is deployed, what its layout details are, and ow it flows data. But in many cases not only is a review of this a good idea, but this will allow you to ensure you have all the key detailed noted and captured before the final migration phase. This section [Reviewing your Secure Gateway deployment details](/docs/SecureGateway?topic=SecureGateway-dep-gather-sg-details) is to help guide you through gathering the key information from your {{site.data.keyword.SecureGateway}} instances that you will need when you move forward with the migration.

Phase 3: Setting up {{site.data.keyword.satellitelong}} Connector
:   The two previous phases involved learning about the new product, comparing it to your existing product, and gathering information needed to do the migration. Now you can use the knowledge and data collected to set it up in a similar way to your {{site.data.keyword.SecureGateway}} infrastructure. When you complete the steps in [Setting up {{site.data.keyword.satellitelong}} Connector](/docs/SecureGateway?topic=SecureGateway-testing-connector), you will know the migration is complete and the same data is flowing that you need into the IBM Cloud.

Last step: Disable Secure Gateway
:   The only step left after you have completely these three phases is to disable your Secure Gateway gateways and then delete your Secure Gateway instance from your account.


## Migration assistance with IBM Technology Expert Labs 
{: #dep-migration-assistance}

If your organization needs help with your IBM Cloud secure data communication migration, or wants to engage another organization to assist you, here are some partners that have a great deal of skill and experience with cloud application migrations and cloud native development improvements and transformations. Contact them if needed to discuss your specific requirements. These are for-pay migration assistance options and you will need to engage with those teams directly.

Technology Expert Labs for IBM Cloud is the IBM Cloud platform professional services team providing deep technical expertise and a proven approach to assist customers to quickly and confidently adopt IBM Cloud technologies. They provide two professional services offerings to either guide and enable customers to migrate to Satellite Connector themselves (Architect for IBM Cloud Satellite) or to perform the migrate for customers (Migrate to IBM Cloud Satellite).

- **Architect for IBM Cloud Satellite** - Use this offering for professional services to create a solution design, a migration plan and/or hands on guidance and enablement to migrate from Secure Gateway to Satellite Connector. This offering provides design, planning, enablement and guidance only, not actual migration services. 1 unit provides 1 week of professional services that will enable customers to perform the migration themselves. The offering is available via PID in SQO or the IBM Cloud Catalog. See [full details](https://cloud.ibm.com/media/docs/downloads/satellite/architect.pdf){: external}.

- **Migrate to IBM Cloud Satellite** - Use this offering for professional services to perform the migration from Secure Gateway to Satellite Connector. This offering will set up and configure a Satellite Connector service to replace a Secure Gateway service. 1 unit provides 1 week of professional services that will perform the migration. The offering is available via PID in SQO or the IBM Cloud Catalog. See [full details](https://cloud.ibm.com/media/docs/downloads/satellite/migrate.pdf){: external}.


## Customer Support
These materials and resources should help you through your IBM Cloud Secure Gateway migration. If you have questions, or need help, contact [IBM Cloud® Customer Support](https://cloud.ibm.com/unifiedsupport/supportcenter){: external} for additional information and assistance.







