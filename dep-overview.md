---

copyright: 
  years: 2023, 2023
lastupdated: "2023-09-29"

keywords: secure gateway, deprecation, migration

subcollection: SecureGateway

---

{{site.data.keyword.attribute-definition-list}}


# Overview and timeline
{: #dep-overview}

{{site.data.keyword.cloud}} is announcing the full deprecation of {{site.data.keyword.SecureGateway}}. See the deprecation dates, details, and specific implications.
{: shortdesc}

{{site.data.keyword.SecureGateway}} has provided secure communications for numerious IBM Cloud customers and workloads over many years. But as technology moves on, newer and more sophisticated technologies have become available for our {{site.data.keyword.cloud}} users. Details below will walk you through those details, migration steps, and reach out for asssitance.




**Internal users**: Review the following notes.
{: important}

- This is a pre-release test version of the announcement for the Secure Gateway deprecation. 
- This page is internally available starting on 28 September 2023 - it is not visible users in production documentation
- It will be externally announced and moved to production on 26 October 2023.
- The short URL land page for this page is https://ibm.biz/ibmsg-announce.
- This is confidential outside of IBM until 26 October 2023 and cannot be shared with any external clients without Secure Gateway Product Management engagement.
- Enage with this process, the key teams, and the internal information and support here: https://ibm.biz/sg-future.




## Timeline
{: #deprecation_timeline}

The following table describes the details of the deprecation, possible migration targets for your applications, and additional information.

| Date | Stage | Description |
| --- | --- | --- |
| 26 October 2023 | Deprecation announcement |  Announcement of the {{site.data.keyword.SecureGateway}} deprecation. All current {{site.data.keyword.SecureGateway}} users will receive an announcement email with information about the deprecation. Notification details will be put into the console and related screens. New deployments for the Secure Gateway service have been discontinued. Existing users as of this date will continue to be able to deploy Secure Gateway instances as needed for updates and replacement. For more infromation, see [Deprecation details](#deprecation_details). |
| Ongoing | Reminders | Periodic reminders will be sent to all users with running {{site.data.keyword.SecureGateway}} instances that the end of support date is coming.
| 26 October 2024 | End of support | {{site.data.keyword.SecureGateway}} end of support. All remaining Secure Gateway instances will be deprovisioned and delete. |


## {{site.data.keyword.SecureGateway}} deprecation details
{: #deprecation_details}

Deprecation is a process, and we've announced the beginning of that process. It begins with the announcement and continues until the end of support.

- No other IBM Cloud service or product will be affected other than {{site.data.keyword.SecureGateway}}
- Be sure to migrate to another method of secure data communication before the end of support date.
- Only paid versions of {{site.data.keyword.SecureGateway}} will remain fully functional until the end of support date - that includes “Professional” and “Enterprise”. The “Essentials” free version ends on the above timeline. Changing from “Essentials” to one of the other two versions will be required.
- Reminders of the approaching "end of support" date will continue to be sent periodically by email to all users that have {{site.data.keyword.SecureGateway}} instances running. Those reminder emails will end when all the {{site.data.keyword.SecureGateway}} instances in your account are deleted.
- After the end of support date, any remaining {{site.data.keyword.SecureGateway}} instances will be permanently disabled and de-provisioned by the {{site.data.keyword.SecureGateway}} team. You must assume your {{site.data.keyword.SecureGateway}} instances will be no longer available at the end of support time.

## Migration options
{: #dep-migration-options}

See [Reviewing your migration options](/docs/SecureGateway?topic=SecureGateway-dep-migration-options) for an overview and comparison of {{site.data.keyword.satellitelong}} Connector and a Virtual Private Network.

## Migration steps
{: #dep-migration-steps}

Of the listed migration options, the most similar to {{site.data.keyword.SecureGateway}} is {{site.data.keyword.satellitelong}} Connector. So here is a phased approach to moving to that solution.

Phase 1: Investigate {{site.data.keyword.satellitelong}} Connector versus {{site.data.keyword.SecureGateway}}
:   Understanding the product {{site.data.keyword.satellitelong}}, and it's capability {{site.data.keyword.satellitelong}} Connector, is the first key step. This section [Reviewing Satellite Connector as a Secure Gateway replacement](/docs/SecureGateway?topic=SecureGateway-understanding-connector) will familiar you with that product. It will also explain through the differences and similarities between the two products. When done with this phase, you should understand the new product, how it relates to the existing {{site.data.keyword.SecureGateway}} product, and key information moving forward.

Phase 2: Reviewing your {{site.data.keyword.SecureGateway}} deployment details
:   Some users may know everything they need to know about how their {{site.data.keyword.SecureGateway}} is deployed, what it's layout details are, and ow it flows data. But in many cases not only is a review of this a good idea, but this will allow you to ensure you have all the key detailed noted and captured before the final migration phase. This section [Reviewing your Secure Gateway deployment details](/docs/SecureGateway?topic=SecureGateway-dep-gather-sg-details) is to help guide you through gathering the key information from your {{site.data.keyword.SecureGateway}} instances that you will need when you move forward with the migration.

Phase 3: Setting up {{site.data.keyword.satellitelong}} Connector
:   The two prevous phases involved learning about the new product, comparing it to your existing product, and gathering information needed to do the migration. Now it's time to do that setup and use the knowledge and data collected to set it up in a similar way to your {{site.data.keyword.SecureGateway}} infrastructure. When you complete the steps in [Setting up {{site.data.keyword.satellitelong}} Connector](/docs/SecureGateway?topic=SecureGateway-testing-connector), you will know the migration is complete and the same data is flowing that you need into the IBM Cloud.

The only step left after you've completely these 3 phases us to disable your {{site.data.keyword.SecureGateway}} and delete them from the IBM Cloud.


## Migration assistance
{: #dep-migration-assistance}

If your organization needs help with your IBM Cloud secure data communication migration, or wants to engage another organization to assist you, here are some partners that have a great deal of skill and experience with cloud application migrations and cloud native development improvements and transformations. Contact them if needed to discuss your specific requirements. These are for-pay migration assistance options and you will need to engage with those teams directly.

IBM Cloud Expert Labs
:   IBM Cloud Expert Labs is the IBM Cloud Platform Professional Services team providing deep technical expertise and a proven approach to modernize applications on IBM Cloud platform technologies. For your IBM Cloud secure data transfer  needs, we offer both Advisory Services to assess, recommend, and plan your modernization approach and Migration Services to assist with data transfer and migration.

## Customer Support
These materials and resources should help you through your IBM Cloud Secure Gateway migration. If you have questions, or need help, contact [IBM Cloud® Customer Support](https://cloud.ibm.com/unifiedsupport/supportcenter){: external} for additional information and assistance.






