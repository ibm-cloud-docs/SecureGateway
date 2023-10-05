---

copyright: 
  years: 2023, 2023
lastupdated: "2023-10-05"

keywords: secure gateway, deprecation, migration, resource controller, resource groups

subcollection: SecureGateway

---

{{site.data.keyword.attribute-definition-list}}


# Updating a Secure Gateway instance from Cloud Foundry org and space to resource groups
{: #rc-update}



[Cloud Foundry is deprecated](https://ibm.biz/ibmcf-announce) and no longer supported as of 1 June 2023. This means Secure Gateway instances that use Cloud Foundry need to be migrated from using the Cloud Foundry service broker to using IBM Cloud resource groups. In most cases, this can be done by the Secure Gateway Product team, but it’s better to be done by Secure Gateway instance owners to fully control the process.
{: shortdesc}


There is a specific timeframe and deadline to make this change. All details for that timeline and steps needed can be find in the [Resource group update](/docs/SecureGateway?topic=SecureGateway-rc_resouce_groups) doc.

Updating the Secure Gateway instance pointer from CF org/space to resource groups does not impact the running service instance (SG connections), or the VPN. This update impacts only the service governance model which means the Secure Gateway service instance authorization model will be migrated to IAM from CF. After the migration, any creation or removal of secure gateway service instances will require IAM privileges.

## Prerequisites and notes
{: #rc-update-prereq}

- **Only one instance of SG is allowed per resource group**: You can have only a single service instance of SG per resource group. So, you might need to create multiple resource groups to host multiple service instances. You can have up to 1000 resource groups in IBM Cloud if you have a billable account.

- **Resource groups can't be changed after migrating**: When you migrate existing Cloud Foundry service instances (like a Secure Gateway instance) to a resource group, the resource group that you choose can't be changed after the update is complete. So, it's essential to plan how you want to organize resources in the account before you do this update.

## Migration steps using UI.
{: #rc-update-console}

1. Login to https://www.cloud.ibm.com and Select the **Navigation** menu icon , select **Resource** list.
1. Click the **Integration** drop down menu to see the list of Secure Gateway instance(s). 
    ![Resource list](./images/image2.png "Resource list")
1. Click the **Migrate** button next to the Secure Gateway instance you want to migrate.
    ![Migrate button](./images/image4.png "Migrate button")
1. Click **Continue** in the confirmation pop-up.
    ![Confirmation](./images/image6.png "Confirmation")
1. Select your **Resource Group** from the drop down.
    ![Resource group selection](./images/image7.png "Resource group selection")
1. Confirm that the instance is migrated for you.
    ![Successful migration](./images/image8.png "Successful migration")  

1. After you successfully migrate an instance, you can see it in the **Resource list** page. Note that the alias icon stays in the Cloud Foundry section. It is created to represent the existing Cloud Foundry instance. You can delete the alias instance anytime.
    ![Alias instance](./images/image11.png "Alias instance")  


## Migration steps using the CLI
{: #rc-update-cli}

1. Login to CLI and target the `cf` endpoints in the region you want to migrate Secure Gateway instances. Because IBM Cloud Foundry Public is deprecated, you need to enable the CF feature flag by running below command before target cf endpoint. 

    ```sh
    export IBM_CF_EXTENSION=true
    ```
    {: pre}

    ```sh
    ibmcloud target [-r REGION_NAME] [-g (RESOURCE_GROUP_NAME | RESOURCE_GROUP_ID [--cf-api ENDPOINT] [-o ORG] [-s SPACE
    ```
    {: pre}

    ```sh
    Example commands and output.

    User$ export IBM_CF_EXTENSION=true

    User$ ibmcloud target -r eu-gb -g Default --cf-api https://api.eu-gb.cf.cloud.ibm.com -o <org> -s <space>

    Targeted resource group Default
    Switched to region eu-gb
    Targeted Cloud Foundry (https://api.eu-gb.cf.cloud.ibm.com)
    Targeted org <org>
    Targeted space <space>
                      
    API endpoint:      https://cloud.ibm.com
    Region:            eu-gb
    User:              user@ibm.com
    Account:           User's Account (xxxxxxx)
    Resource group:    Default
    CF API endpoint:   https://api.eu-gb.cf.cloud.ibm.com (API version: 2.205.0)
    Org:               <org>
    Space:             <space>
    ```
    {: screen}

1. Run the following command to migrate the instance.
    ```sh
    ibmcloud resource cf-service-instance-migrate (SERVICE_INSTANCE_NAME | SERVICE_INSTANCE_ID) [--resource-group-name RESOURCE_GROUP_NAME | --resource-group-id RESOURCE_GROUP_ID] [-f, --force] [-q, --quiet]
    ```
    {: pre}

    Example commands and output of a successful migration.
    ```sh
    User$ ibmcloud resource cf-service-instance-migrate "Secure Gateway-z0" --resource-group-name Default
    Really migrate Cloudfoundry service instance Secure Gateway-z0 into resource group Default?> y
    Migrating Cloudfoundry service instance Secure Gateway-z0 into resource group Default as user@ibm.com...
    OK
    Service instance Secure Gateway-z0 was migrated into resource group Default successfully
    ```
    {: screen}


1. Confirm the instance was migrated by running the following command and reviewing the resource group ID.
   
    ```sh
    ibmcloud resource service-instances
    ```
    {: pre}

    Example output

    ```sh
    Retrieving instances with type service_instance in all resource groups in all locations under account User’s Account as user@ibm.com...
    OK
    Name                           Location   State    Type               Resource Group ID
    Secure Gateway-z0              eu-gb      active   service_instance   1b32c7b28b1c4ff786cc5a74376bc757
    ```
    {: screen}



## Important steps after migration
{: #rc-update-after}

Manage users’ access to resources
:   After you migrate your Cloud Foundry service instances to a resource group, you need to ensure that the users in your account have the required level of access to the resources in the account resource groups. The users need to be assigned the **Operator** or **Editor** role in IAM to manage the instance. This also provide access for users to create new service instances in the account resource groups. For more information about assigning access to resources in your resource groups, see [Managing access to resources](/docs/account?topic=account-assign-access-resources&interface=ui#assign-access-resources).

Review the Billing dashboard:
:   After migration, the billing is confirmed under Cloud Foundry Org until the end of the month. The next month the billing can be checked under Resource group.

![Billing dashboard](./images/image10.png "Billing dashboard") 





