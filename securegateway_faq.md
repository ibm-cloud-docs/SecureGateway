---

copyright:
  years: 2015, 2018
lastupdated: "2018-12-20"

---

# Frequently Asked Questions

## OSI Model Layer
{: #osi}

### Question
What layer of the OSI model does the Secure Gateway service represent?

### Answer
The Secure Gateway service represents layer 4 of the OSI model.

## TLS Version Support
{: #tls}

### Question
What version of TLS does the Secure Gateway service support?

### Answer
The Secure Gateway service supports TLS version 1.2.

## Why would I disable my gateway or destination?
{: #disabled}

### Question

Why would you want to disable a gateway or destination?

### Answer
You might want to disable a destination or gateway for one of the following reasons:

- You create a destination, but you have not set up the security.  In this case, you might disable the destination until its security is set up.
- You do not want the service to be available to users because you are making some updates to the service.  In this case, you might temporarily disable the necessary gateways and wait for the service to be updated.
- You have set up all your gateways and destinations on the front end, but your backend is still building.  In this case, you would disable your gateways or destinations until the backend build is complete.

For more information on disabling a gateway or a destination, see [how to manage your Secure Gateway service instance](./securegateway_managing.html).

## What is the recommended approach to creation automation across multiple spaces?
{: #automation-spaces}

### Question
A customer environment has one org and three spaces.  One space is for development, another for staging, and the final one for production.  Should the customer create a single Secure Gateway instance or multiple (e.g., one for each space).  If the customer can create multiple gateways, are there any considerations for reusing a Node.js application to create a gateway and destination in each space?

### Answer

- You can create a single Secure Gateway instance for all three spaces.  However, you must remember the gateway and destination [limitations for your specific plan](./securegateway_plans.html).
- There are no additional considerations for reusing a Node.js application as no service bindings are required by Secure Gateway.


## What is the recommended approach to creation automation across multiple orgs?
{: #automation-orgs}

### Question
A customer environment has three orgs: one for development, one for staging, and one for production.  Is a Secure Gateway service instance required for each org and is the configuration available to all spaces within that org?

### Answer

- You are not required to have a Secure Gateway service instance in each org.  You could have an instance in one org and use the gateways within that instance from all of your other environments.  With this setup, you must remember the gateway and destination [limitations for your specific plan](./securegateway_plans.html).
- You can have a Secure Gateway service instance in each org and the configuration will be available to all your spaces.

## Does my app need to be in the same space?
{: #app-space}

### Question
Do you need to run the Node.js app in the same {{site.data.keyword.Bluemix_notm}} space as the Secure Gateway service?

### Answer
No, you do not need to run your app in the same {{site.data.keyword.Bluemix_notm}} space as the Secure Gateway service.

## Can I get the Secure Gateway server logs?
{: #server-logs}

### Question
Can you retrieve error logs for the Secure Gateway server?

### Answer
Error logs on the server cannot be retrieved.  Only errors that are made at the time of the request can be seen.

## What are the functional states of Secure Gateway?
{: #states}

### Question
What are the different lifecycle states of gateways and destinations?

### Answer

#### Non-functional State
The 1.7.0 release introduced a new tiered plan pricing model. With this model came the ability to mark both Gateways and Destinations as 'Active' or 'Inactive'. Part of the new plan billing structure charges the user for the number of Gateways and Destinations that they have.

- Inactive items are not billed
- Inactive Destinations do not have a provisioned cloud port.
- All Destinations within an inactive gateway will also be inactive and cannot be reactivated until their Gateway is also set Active.
- Inactive items are considered Non Functional. Inactive items cannot be in the any of the Functional states.

#### Functional States
When a gateway or destination is marked active it will be billed. Active states for Gateways and Destinations are below:

- Enabled - the gateway or destination is ready for use under normal circumstances.
- Disabled - the gateway or destination has been disabled.
    - Gateways - clients will be unable to flow data to the disabled gateway.
    - Destinations - clients will be unable to create connections to these disabled destinations.

## How do I know the data activities on the Secure Gateway Client?
{: #data-size}

### Question
How do I know the data activities through Secure Gateway Client?

### Answer
On SecureGateway Client, change the log level to TRACE. The following information will be displayed after requests are sent.

Data size sent from request application:
```
[TRACE] Connection #<connection ID> transmitted data: <size> bytes
```

Data size sent from destination:
```
[TRACE] Connection #<connection ID> received data: <size> bytes
```

## How do I get the real-time connection number on Secure Gateway?
{: #connect-num}

### Question
How do I get the real-time connection information, the data size sent and received from Secure Gateway Client?

### Answer

- On Secure Gateway client interactive command line:
Type `s` to print the connection status details. 
- On Secure Gateway client UI, click the Connection Information menu

The following connection statistics would be displayed: 
- Overall data size transmitted.
- Overall data size received.
- The number of total connections.
- Real-time active connections.

Note: The number of Current Connections on Secure Gateway UI is not rendered in real-time. Please use the above ways on Secure Gateway client to retrieve the real-time connection information.

## What are the recommended configurations to make my connections more secure?
{: #secure-app}

### Question
What are the recommended configurations to make my connections more secure?

### Answer

#### Use Mutual Authentication
Enable Mutual Authentication for both sides of on-premise destinations makes Secure Gateway more secure. On User Authentication side, enable mutual authentication to restrict the access of Secure Gateway cloud node by authenticating using a client certificate when the request is over TLS/HTTPS. On Resource Authentication side, enable mutual authentication to provide appropriate credential when connecting to destination endpoint, ensure secure/encrypted access to on-premise resource. Please see [Configuring Mutual Authentication](./securegateway_destination.html#mutual-auth) and [Node.js TLS Mutual Authentication](./securegateway_tls-ma.html#node-js-tls-mutual-authentication) for more information.

#### Set IP Table Rules (For on-premise destination)
The Secure Gateway cloud host and port of an on-premise destination is in the public space; therefore it is allowed everyone to access by default.
To control the traffic accessing on Secure Gateway, set iptable rules to only allow access by a specific range of IPs and ports to secure on-premise resources. Please see [IP Table Rules](./securegateway_destination.html#configuring-network-security) for more information about how to configure the iptable rules on Secure Gateway.

#### Configure Access Control List (For on-premise destination)
Configure Access Control List support to allow or restrict access to on-premises resources would make the on-premises destinations more secure by specifying the access right on the specific destination host and port. It is recommended to define the allowed or restrict HTTP/S routes on the ACL entries as well to enhance the security of on-premises destination. Please see [Access Control List](./securegateway_acl.html#access-control-list) and [HTTPS/Route Control using the ACL](./securegateway_acl.html#routes) for more information.

#### Set password on the Secure Gateway Client UI
It is recommended to set the UI password to restrict the access of Secure Gateway Client UI. Please see [Interacting with the Client](./securegateway_interaction.html#interacting-with-the-client) for more details about how to set the password using startup configuration or interactive commands on Secure Gateway Client terminal command line.

## What is gateway migration?
{: #gateway-migration}

### Question
After 2018 December maintenance, there is a migrate button on the gateway panel, what is the usage of that button?

### Answer

After 2018 December maintenance, the cloud host of Secure Gateway is getting renamed to use the `securegateway.appdomain.cloud` instead of `integration.ibmcloud.com`. For backward compatibility, the existing gateway will keep using the old domain until the gateway is migrated.

After the migration, the cloud host of the on-premise destinations will change to use the new domain, the users/applications will need to update to send the reqeust to the new cloud host.

Currently the migration is not mandatory and there is not an exact date about when the old domain will be out of support, but once this is settled, the customer who are still using the old domain name will be notified.

## Where can we receive notifications?
{: #notification}

### Question
Where can we receive Secure Gateway notifications, especially for disruptive maintenance?

### Answer

You can get notifications via our [status page](https://console.bluemix.net/status), please search `Secure Gateway` in that page.
