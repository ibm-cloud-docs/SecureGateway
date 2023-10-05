---

copyright: 
  years: 2023, 2023
lastupdated: "2023-09-28"

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
    - Review [Gateways](/docs/SecureGateway?topic=SecureGateway-add-sg-gw).
    - Review [Clients](/docs/SecureGateway?topic=SecureGateway-add-client).
    - Review [Destinations](/docs/SecureGateway?topic=SecureGateway-add-dest).

## Access your {{site.data.keyword.SecureGateway}} instances
{: step}
- Access your list of [Secure Gateway service instances](https://cloud.ibm.com/resources?product=Secure) - see their names, what group they are in, where they are deployed, status, and the tags
- For each instance follow the next step to gather additional details


## Access your {{site.data.keyword.SecureGateway}} instance details
{: #dep-gather-sg-details-console}
{: step}

1. In this instance, you can see on the first page the total traffic, and a list of the Gateways in that instance. There may be no gateways, 1 gateway, or more gateways. No gateways means you're not using this instance to transfer traffic.
2. In any Gateway box, click on the ![**Settings icon**](./images/settingIcon.png "Setting Icon") and it will show you the details for that gateway
    - Gateway name
    - Gateway key
    - Gateway ID
    - Node it's attached to
    - Created & modified information

In a below stop we'll show you how to extract this information to a file and get information from it that way

DEREK: Can we get a picture here from Sid for this.

3. In any Destination box, click on the ![**Settings icon**](./images/settingIcon.png "Setting Icon") and it will show you the details for that detination
    - Destination ID
    - Cloud host & port
    - Resource hos & port
    - Created & modified information
    - Security protocal


4. Back on the Gateway screen information, you can extract all the information about that Gateway to a file you can use to gather all the details
  - Click the Export button ![Export Button](./images/exportIcon.png "Export Button") 
  - It will be saved with the unique ID for that gateway to your download directory
  - In a following step we'll show you how to parse that file and gather data



DEREK: We need this, but now I'm not sure where
    - Every {{site.data.keyword.satelliteshort}} Connector is functionally similar to each Gateway. So you might have multiple Secure Gateway instances, and you might have multiple Gateway destinations set up. You will create a Satellite Connector endpoint for each of the Secure Gateway Destinations you have set up.
    - Select each Destination in turn and click on the ![**Settings icon**](./images/settingIcon.png "Setting Icon") to view the details for that destination and collect the necessary data for creating an equivalent Connector endpoint.




## Parse extracted {{site.data.keyword.SecureGateway}} gateway files to gather data
{: #dep-gather-sg-details-parse}
{: step}

In the previous step, if you extracted data about each Gateway, you can parse it using simple CLI tools to get that information easily.
You could get all of this using the UI, but this allows you to examine and savae without many copy and past steps.

1. Prep your machine
  - This is most useful on a linux-type environment - so us Mac OS terminal, Linux, or Windows Linux support terminal window
  - You can just load the json file into a browser or JSON viewing tool, but you also might want to use a JSON processor like [JK](https://jqlang.github.io/jq/). Our example will show you using JK, so you will need to install that if you want to use this method
  - Each of the files saved have an extension .gateway. You can use them directly, but it also might help to pull into an editor if you rename them.json - but it is optional

2. Extracting data

filename="<name of the file you want to example>"
cat $filename | jq "."
This will show you the whole file

  
cat $filename | jq ".desc"
Shows you the Gateway name
  
  
cat $filename | jq ".destinations[]"
Shows you the destiniations with all the sub array data
  
cat $filename | jq ".destinations[] .desc"
Shows you just the destination names - now you know how many destinations you have for that gateway

cat $filename | jq ".destinations[0]"
Will list the details for just that destination - where "0" is the number of the destination in the array of destinations



cat $filename | jq '.destinations[] | .desc + " " + .ip'
Will show you some key info for this destiniation
  
  
TODO: We need to spend more time here - how to extract what we want and need
TODO: We could script this
TODO: We could dump things to create a CSV
TODO: We need Nathan and Sid to decide what is critical
TODO: We need to say things like 
TODO:     If your protocol="HTTPS", then you need to do and watch out for this
TODO:     If you enable_client_tls="XYZ", then you need to do this
TODO: 
  
cat $filename | jq 

cat $filename | jq 

cat $filename | jq 

cat $filename | jq 

cat $filename | jq 





## Access your {{site.data.keyword.SecureGateway}} instance details in the CLI
{: #dep-gather-sg-details-cli}
{: step}

If you prefer working in the command line, you can obtain a number of the above details, with even less usage of the IBM Cloud console.
If you already gathered your instance information, you can continue with this step as you like.

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
  
1. Instance list: 
    - You know how many instances you have, and their names, group, location, status, and tags

2. Instance gateway list
    - For each instance, you know the informaiton about the created gateways: key token, ID, node, and dates
    - You know how many gateways you have
  
3. Instance gateway destination list
    - For each gateway, you know the 
  
4. Instance gateway destination details


 
Review the information you compiled from your {{site.data.keyword.SecureGateway}} deployments and how they map to the inputs you need for setting up a Connector.

- **Region**: Create your Connector in the same region where your {{site.data.keyword.SecureGateway}} deployment was located.
- **Resource group**: Create your Connector in the same resource group where your {{site.data.keyword.SecureGateway}} deployment was located.
- **Gateways**: Recreate your {{site.data.keyword.SecureGateway}} Gateways as Satellite Connectors.
- **Destinations**: Recreate your {{site.data.keyword.SecureGateway}} destinations as Connector endpoints.
- **Protocol**: Use the same {{site.data.keyword.SecureGateway}} destination protocol value when you create your Connector endpoint.
- **Port**: Use the same port value from your {{site.data.keyword.SecureGateway}} destination as the `Destination port` when setting up your Connector endpoint.
- **Network Security**: Use your Secure Gateway Network Security settings when setting up your Connector endpoint access control lists (ACLs).
- **Authentication Files**: Get a `.zip` of your [authenticaion files](https://test.cloud.ibm.com/docs/SecureGateway?topic=SecureGateway-nodejs-tls-ma#tls-ma-download-files). You will upload these authentication files when creating a Connector endpoint in the console.
- **Clients**: The client establishes the initial connection between the on-premises network and a gateway on the Secure Gateway servers and allows for communication to pass through to the defined destinations. Use your client details to create Connector agents.
- **Proxy**: Include your {{site.data.keyword.SecureGateway}} proxy settings in your Dockerfile when configuring a proxy for your Satellite Connector.

## Next steps
{: #dep-gather-sg-details-next-steps}
  
You can now use the output from the previous steps to begin [Setting up Connector for testing {{site.data.keyword.SecureGateway}} migration](/docs/SecureGateway?topic=SecureGateway-testing-connector).

