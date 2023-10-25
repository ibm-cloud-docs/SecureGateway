---

copyright: 
  years: 2023, 2023
lastupdated: "2023-10-25"

keywords: secure gateway, deprecation, migration

subcollection: SecureGateway

---

{{site.data.keyword.attribute-definition-list}}


# Reviewing {{site.data.keyword.SecureGateway}} deployment details
{: #dep-gather-sg-details}

{{site.data.keyword.SecureGateway}} is deprecated. For more information, see the [deprecation details](/docs/SecureGateway?topic=SecureGateway-dep-overview).
{: deprecated}

You can use the following steps to analyze the current usage of {{site.data.keyword.SecureGateway}} for your secure data communications and collect the necessary information to create corresponding {{site.data.keyword.satelliteshort}} Connectors.
{: shortdesc}


## Goals 
{: #dep-gather-sg-goals}

The goal of this tutorial is to help guide you through gathering the key information from your {{site.data.keyword.SecureGateway}} instances that you will need when you migrate to {{site.data.keyword.satelliteshort}} Connector. 

## Review the {{site.data.keyword.SecureGateway}} concepts
{: #dep-gather-sg-terms}

You might need to review the common terms and concepts of {{site.data.keyword.SecureGateway}}. For more information, see the following links.

- Review the [Getting started with {{site.data.keyword.SecureGateway}}](/docs/SecureGateway?topic=SecureGateway-getting-started-with-sg) in general.
- Review the key {{site.data.keyword.SecureGateway}} component topics.
    - [Gateways](/docs/SecureGateway?topic=SecureGateway-add-sg-gw).
    - [Clients](/docs/SecureGateway?topic=SecureGateway-add-client).
    - [Destinations](/docs/SecureGateway?topic=SecureGateway-add-dest).

## Access your {{site.data.keyword.SecureGateway}} instances
{: step}
- The first step is to see what Secure Gateway instance(s) you have deployed. Most users only have a single instance, but some have multiple depending on the size of their deployment.
- Access your list of [Secure Gateway service instances](https://cloud.ibm.com/resources?product=Secure) - see their names, what resource group they are in, what region they are deployed, their status, and any tags they might have
- For each instance follow the next step to gather additional details.


## Access your {{site.data.keyword.SecureGateway}} instance details
{: #dep-gather-sg-details-console}
{: step}


1. In this instance, you can see on the first page the total traffic, and a list of the Gateways in that instance. There may be no gateways, 1 gateway, or more gateways. If there are no gateways created, it means you are not using this instance to transfer traffic.
1. In any Gateway box, click the ![**Settings icon**](./images/settingIcon.png "Setting Icon") to review the following details for that Gateway.
    - Gateway name
    - Gateway key
    - Gateway ID
    - Node it's attached to
    - Created & modified information
    - Whether it is enabled or disabled


1. Click on a Gateway to review the **Gateway** page.

1. Review the **Destinations** tab for a list of destinations. In any Destination box, click the ![**Settings icon**](./images/settingIcon.png "Setting Icon") to review the following details for that Destination.
    - Destination ID
    - Cloud host & port
    - Resource host & port
    - Created & modified information
    - Security protocal

1. Click the **Clients** tab to review the Clients that are connected to that Gateway.


1. Back on the Gateway screen information, you can extract all the information about that Gateway by clicking the **Export** button ![Export Button](./images/exportIcon.png "Export Button"). Note that the file is saved with the unique ID for that gateway to your Download directory.


1. Complete the next steps to to parse the file and gather the data you need to set up Satellite Connector.

Keep in mind when reviewing your Secure Gateway details that each Gateway is similar to a {{site.data.keyword.satelliteshort}} Connector. So as you review your might have multiple Secure Gateway instances, and you might have multiple gateways and destinations set up within that instance. Also each of your Secure Gateaway Destinations are similar to Satellite Connector endpoints.
{: note}

## Parse the extracted {{site.data.keyword.SecureGateway}} gateway files to gather data
{: #dep-gather-sg-details-parse}
{: step}

In the previous step, if you extracted data about each Gateway, you can parse it using simple CLI tools to get the information easily. You can also get this data using the console, but the CLI allows you to examine and save with fewer manually copy and paste steps.

1. Prepare your machine.
  - This is most useful on a linux-type environment - so use Mac OS terminal, Linux, or Windows Linux support terminal window
  - You can load the json file into a browser or JSON viewing tool, but you also might want to use a JSON processor like [JQ](https://jqlang.github.io/jq/). 
  - If you want to use JQ as we do in the following example, you need to download it before beginning.
  - **Optional** Each of the files saved have an extension `.gateway`. You can use them directly, but it also might help to pull into an editor if you rename them `.json`.

2. Extract the data.
  - You can run a series of commands to get various pieces of data

  - Set the filename for convenience.
    ```sh
    filename="<name of the file you want to example>"
    ```
    {: pre}
    
    
  - Display the whole file.
    ```sh
    cat $filename | jq "."
    ```
    {: pre}
    

  - Get the Gateway name.
    ```sh
    cat $filename | jq ".desc"
    ```
    {: pre}

  - Get the destiniations with all the sub array data.
    ```sh
    cat $filename | jq ".destinations[]"
    ```
    {: pre}

  - Get just the destination names in all the destinations. This also tells you know how many destinations you have for that gateway.
    ```sh
    cat $filename | jq ".destinations[] .desc"
    ```
    {: pre}

  - Get the details for a specfic destination where "0" is the number of the destination in the array of destinations.
    ```sh
    cat $filename | jq ".destinations[0]"
    ```
    {: pre}


## Access your {{site.data.keyword.SecureGateway}} instance details in the CLI
{: #dep-gather-sg-details-cli}
{: step}

If you prefer working in the command line, you can obtain a number of the above details, with even less usage of the IBM Cloud console.
If you already gathered your instance information, you can continue with this step as you like. You will need to have the IBM Cloud CLI set up and the "Cloud Foundry plugin installed.

1. Enable the command line feature flag that will permit to use Cloud Foundry commands.
    ```sh
    export IBM_CF_EXTENSION=true
    ```
    {: pre}

1. Install the `cf` plug-in.
    ```sh
    ibmcloud cf install
    ```
    {: pre}

1. Target a CF org and space.
    ```sh
    ibmcloud target --cf -r REGION -o ORG -s SPACE
    ```
    {: pre}


1. [Get an IAM refresh token for your session](/docs/account?topic=account-iamtoken_from_apikey#iamtoken).
    ```sh
    ibmcloud iam oauth-tokens 
    ```
    {: pre}

1. List your {{site.data.keyword.SecureGateway}} instance details. Make a note of the `Organization ID` and `Space ID`. You will use these values as inputs in the next step.

    ```sh
    ibmcloud resource search 'name: *Secure*Gateway*'
    ```
    {: pre}

    Example output.
    ```txt
    Name:              Secure Gateway-qj
    Location:          eu-gb
    Family:            cloud_foundry
    Resource Type:     cf-service-instance
    Organization ID:   8891a43f-cdac-4e48-a4f7-8cdaf399c183
    Space ID:          e832ed2e-3fe4-4d4f-9394-7f2b2b037eed
    CRN:               crn:v1:bluemix:public:securegateway:eu-gb:s/e832ed2e-3fe4-4d4f-9394-7f2b2b037eed:86684bec-8174-4037-baed-70a0a4604220:cf-service-instance:
    Tags:
    Service Tags:
    Access Tags:
    ```
    {: screen}

1. Get the details for each of your {{site.data.keyword.SecureGateway}} instances by running the following `curl` command. Make sure to replace `ORG-ID` and `SPACE-ID` with the CF org and space IDs that you found in the previous step.

    ```sh
    curl -X GET   -H 'Authorization: Bearer TOKEN' 'https://sgmanager.us-south.securegateway.cloud.ibm.com/v1/sgconfig?org_id=ORG-ID&space_id=SPACE-ID'
    ```
    {: pre}

    Review the output and make a note of the `_id`

    ```json
    [{"_id":"AAAA","org_id":"ORG-ID","desc":"Disconnected Gateway","hostname":"cap-sg-prd-2.securegateway.appdomain.cloud","port":49998,"status":"ENABLED","jwt”:”xxxx”,”enf_tok_sec":true,"connected":false,"created_by":null,"created_at":"2023-05-22T14:39:53.807Z","modified_by":null,"last_status_change":"2023-09-27T14:11:39.882Z","authorization":{"cert":"CERT","key":"KEY"},"recentlyDisconnected":[{"id":"ID","disconnectedAt":1684773248414},{"id":"ID","disconnectedAt":1684767669028},{"id":"ID","disconnectedAt":1684766756637}],"active":true,"connectedClientsArr":[],"expiry":1703599899000},]
    ```
    {: screen}


1. Use the `_id` you found in the previous step to get your destination details.

    ```sh
    curl -X GET -H 'Authorization: Bearer TOKEN' 'https://sgmanager.us-south.securegateway.cloud.ibm.com/v1/sgconfig/SG-ID/destinations'
    ```
    {: pre}

    Example output.

    ```json
    [{"_id":"AAA","configuration":"SG-ID","type":null,"port":18453,"connection_info":{"OnPremHost":"172.17.0.2","OnPremPort":"80","clientPort":null,"sni":"","Password":""},"proxy":{"ip":null,"port":null,"type":null},"enforceProxy":false,"certs":{},"keys":{},"TLS":"none","protocol":"HTTP","private":false,"enable_client_tls":false,"client_tls":"none","status":"ENABLED","created_at":"2023-07-05T14:10:07.678Z","created_by":null,"modified_by":null,"last_status_change":"2023-07-05T14:10:07.744Z","timeout":0,"compressData":true,"rejectUnauth":true,"exempt":null,"ip_table_rules":[],"org_id":"ORG-ID","space_id":"SPACE-ID","hostname":"cap-sg-prd-3.securegateway.appdomain.cloud","dedicatedIP":null,"desc":"perf-test-http"}]
    ```
    {: screen}

1. Get the details of the connected clients.

    ```sh
    curl -X GET -H 'Authorization: Bearer TOKEN' 'https://sgmanager.us-south.securegateway.cloud.ibm.com/v1/sgconfig/SG-ID/clients'
    ```
    {: pre}

    ```json
    [{"id":"AAA","version":189,"version_detail":"Version 1.8.9","host":"0c081089b1c3","type":"docker"}]%
    ```
    {: screen}


## Analysis Summary
{: #dep-gather-sg-details-summary}

Let's summarize the information you have gathered about your {{site.data.keyword.SecureGateway}} deployment
  
1. **Instance list**: You know how many instances you have, and their names, groups, locations, status, and tags.

2. **Gateway list**: For each instance, you know the information about the created gateways - how many you have, and for each one: key token, ID, node, key dates, and the enable/disable status.
  
3. **Destination list**: For each Gateway, you know the incoming destination(s) and details for each: name, host & port, authentication method, network security, proxy settings, and other miscellaneous info.

3. **Client list**: For each gateway, you know the connected clients.


## Next steps
{: #dep-gather-sg-details-next-steps}
  
You can now use the output from the previous steps to begin [Setting up Connector for testing {{site.data.keyword.SecureGateway}} migration](/docs/SecureGateway?topic=SecureGateway-testing-connector).

