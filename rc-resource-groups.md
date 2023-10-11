---

copyright: 
  years: 2023, 2023
lastupdated: "2023-10-05"

keywords: secure gateway, deprecation, migration, resource controller, resource groups

subcollection: SecureGateway

---

{{site.data.keyword.attribute-definition-list}}


# Resource group update
{: #rc_resouce_groups}




**Internal users**: Review the following notes.
{: important}

- This page is in draft stage
- It's a fairly minor change we do need SG users to execute or we will do it for them
- It's NOT attached to the deprecation announcement, but clearly it is related to it
- we will be doing two announcements - this one is first - Oct 10th - 15th or so it will go out
- it has a 30 day time frame - the SG PM will track all users carefully
- then we will execute the update for them if they don't
- Input on this is appreciated


## Overview
{: #overview}

{{site.data.keyword.SecureGateway}} users need to make a change to how their {{site.data.keyword.SecureGateway}} instances are connected to by IBM Cloud.
{: shortdesc}

## What's changing?
{: #rc_changes}

{{site.data.keyword.SecureGateway}} is moving to IBM Cloud resource groups as a replacement for the deprecated Cloud Foundry organizations. As a result, any {{site.data.keyword.SecureGateway}} instances that are not in an IBM Cloud resource group need to be added to one before the below timeline deadline.

- There are no changes to the {{site.data.keyword.SecureGateway}} instance, it's operation, or function.
- There are no changes to the deployment region.
- There is no impact to uptime or availability of your instance.
- There is no impact to data transfer through the Secure Gateway.


## Timeline
{: #rc_timeline}


| Date | Stage | Description | 
| --- | --- | --- | 
| 16 October 2023 | Announcement | Secure Gateway is announcing a change in the way it deploys and references instances of the service. This update will move away from Cloud Foundry organizations, and move to IBM Cloud Resource Groups |
| 16 November 2023 | Deadline  | 30 days after the initial announcement is the deadline for the update of the Secure Gateway instances into Resource Groups. All steps listed below will be taken at that time based on each instance status. |
| 30 November 2023 | Updates completed | All updates will have been completed for Secure Gateway instance resource group updates - either the users will have made them or the Secure Gateway Product team will have made them. |


## What do I need to do?
{: #rc_what_to_do}

There are two methods below for this change to be made - one done by yourself and one done by the {{site.data.keyword.SecureGateway}} product team.

Review the following sections for more information about your options.

## Manually selecting a resource group for your {{site.data.keyword.SecureGateway}} instances
{: #rc-manual}

Doing this change yourself allows you to control the name of the Resource Group into which you will place the Secure Gateway instance.

It also allows you to fully control security admin permissions to existing Secure Gateway instance(s).

If you do not make the changes by the stated deadline, the {{site.data.keyword.SecureGateway}} product team will do the below step.



## Allowing the {{site.data.keyword.SecureGateway}} product team to apply the changes
{: #rc-auto}

If you do not do the above step, the {{site.data.keyword.SecureGateway}} product team will make this update.
However this can only be done if you have a single Secure Gateway instance in the same region. If you have more than one Secure Gateway instance in the same region, we are unable to make the update for you, and the previous step is required.

The product team will complete the following:


- Move your {{site.data.keyword.SecureGateway}} instance into the resource group named 'Default' in your account
- Only the account owner will retain admin privilages for the {{site.data.keyword.SecureGateway}} instance, all other users that previously had admin privilages for that {{site.data.keyword.SecureGateway}} instance will be removed
- The {{site.data.keyword.SecureGateway}} instance account owner will then need to optionally add admin privlages back for other IBM Cloud users

- As a final step, if a Secure Gateway instance has Gateways with no usage traffic in August, September, or October of 2023 - 3 months of no usage - we will put that Gateway into the disabled state. That will denote for us that the Gateway has no traffic or usage. Users can enable it again as they like and continue usage - easy instructions to do that are in [Managing the Secure Gateway service](/docs/SecureGateway?topic=SecureGateway-manage-sg-service)


## Update Steps
{: #rc_how_to_do_it}

When you are ready to make the udpate changes to your {{site.data.keyword.SecureGateway}} instance, see the instructions [Updating Secure Gateway service instances to resource group](/docs/SecureGateway?topic=SecureGateway-rc-update)


## Summary
{: #rc-summary}

Whether you manually update the resource group for their {{site.data.keyword.SecureGateway}}, or allow the IBM Cloud {{site.data.keyword.SecureGateway}} product team do it for you, there is no difference in operation or function for your Secure Gateway instances, no changes, and no downtime required.

## Customer Support
These materials and resources should help you through your IBM Cloud Secure Gateway update to resource groups. If you have questions, or need help, contact [IBM Cloud® Customer Support](https://cloud.ibm.com/unifiedsupport/supportcenter){: external} for additional information and assistance.
