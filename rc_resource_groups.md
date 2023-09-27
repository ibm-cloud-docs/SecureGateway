---

copyright:
  years: 2014, 2023
lastupdated: "2023-09-27"

keywords: secure gateway, deprecation, migration, resource controller, resource groups

subcollection: SecureGateway

---

{{site.data.keyword.attribute-definition-list}}


# Understanding the change from Cloud Foundry organizations to resource groups
{: #rc_resouce_groups}

Updates needed for {{site.data.keyword.SecureGateway}} instances that use Cloud Foundry organizations.
{: shortdesc}

## What's changing?
{: #rc_changes}

{{site.data.keyword.SecureGateway}} is moving to IBM Cloud resource groups as a replacement for the deprecated Cloud Foundry organizations. As a result, any {{site.data.keyword.SecureGateway}} instances that are not in an IBM Cloud resource group need to be added to one before 30 October 2023.

- There are no changes to the {{site.data.keyword.SecureGateway}} instance.
- There are no changes to the {{site.data.keyword.SecureGateway}} instance operation or function.
- There are no changes the deployment region.
- There is no impact to uptime. This is an atomic operation that happens immediately.


## Timeline
{: #rc_timeline}

Stage

Date

Description

| Date | Stage | Description | 
| --- | --- | --- | 
| 10 October 2023 | Announcement | Secure Gateway is announcing a change in the way it deploys and references instances of the service. This update will move away from Cloud Foundry organizations, and move to IBM Cloud Resource Groups |
| 11 November 2023 | Deadline  | 30 days after the initial announcement is the deadline for update of the Secure Gateway service broker. All users that have executed this update for themselves, will be able to pick the IBM Cloud resource group into which they place their Secure Gateway instance. At this time, the IBM Secure Gateway product team will execute the update for customers that have not done that themselves. By doing so, your Secure Gateway instance will be placed in the IBM Cloud resource group named `default`. |
| 15 November 2023 | Updates completed. All updates will have been completed for Secure Gateway service broker updates. |


## What do I need to do?
{: #rc_what_to_do}

There are no required manual changes needed for this update. The {{site.data.keyword.SecureGateway}} product team will move your {{site.data.keyword.SecureGateway}} instances into the default resource group in your account.

However, if you want to choose the specific resource groups for your {{site.data.keyword.SecureGateway}} instance(s) you can manually complete this process.

Review the following sections for more information about your options.

- [Allow the {{site.data.keyword.SecureGateway}} product team to apply the changes](#rc_auto).
- [Select the resource groups for my instances manually](#rc_manual).

## Allowing the {{site.data.keyword.SecureGateway}} product team to apply the changes
{: #rc_auto}

If are ok with your {{site.data.keyword.SecureGateway}} instances being moving into the default resource group in your account and you don't want to manually select the resource groups, then the IBM {{site.data.keyword.SecureGateway}} product team will perform the update. Note that if you choose this option, only the owning user is granted admin permissions. However, after the update is applied, admin users can then add other admin-level users.

## Manually selecting a resource group for your {{site.data.keyword.SecureGateway}} instances
{: #rc_manual}

You can decide the name of the resource group where your want to move your {{site.data.keyword.SecureGateway}} instance(s) and make the update manually.

If you choose this option, you have until 30 October 2023 to manually update the Resource group.

On 30 October 2023 at 23:59 GMT any remaining {{site.data.keyword.SecureGateway}} instances that are not in a resource group will be moved to the default resource group in your account by the {{site.data.keyword.SecureGateway}} product team.

Differences between manual and automated update - more info needed.

If a user updates their {{site.data.keyword.SecureGateway}} instance into a new Resource group themselves, they can optionally include all IBM Cloud users with existing admin permissions

Whether you manually update the resource group for their {{site.data.keyword.SecureGateway}}, or allow the IBM Cloud {{site.data.keyword.SecureGateway}} team do it for you, there is no difference in operation or function.
