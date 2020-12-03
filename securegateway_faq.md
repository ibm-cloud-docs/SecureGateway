---

copyright:
  years: 2015, 2020
lastupdated: "2020-10-20"

subcollection: SecureGateway

---
{:shortdesc: .shortdesc}
{:faq: data-hd-content-type='faq'}
{:external: target="_blank" .external}

# Frequently Asked Questions
{: #sg-faq}

FAQs for Secure Gateway might include questions about automation or logs. To find all FAQs for {{site.data.keyword.cloud}}, see our FAQ library. {: shortdesc}

## What layer of the OSI model does the Secure Gateway service represent?
{: #faq-osi}
{: faq}

The Secure Gateway service represents layer 4 of the OSI model.

## What version of TLS does the Secure Gateway service support?
{: #faq-tls}
{: faq}

The Secure Gateway service supports TLS version 1.2.

## Why would I disable my gateway or destination?
{: #faq-disabled}
{: faq}

You might want to disable a destination or gateway for one of the following reasons:

- You create a destination, but you have not set up the security.  In this case, you might disable the destination until its security is set up.
- You do not want the service to be available to users because you are making some updates to the service.  In this case, you might temporarily disable the necessary gateways and wait for the service to be updated.
- You have set up all your gateways and destinations on the front end, but your backend is still building.  In this case, you would disable your gateways or destinations until the backend build is complete.

For more information on disabling a gateway or a destination, see [how to manage your Secure Gateway service instance](/docs/services/SecureGateway?topic=SecureGateway-manage-sg-service).

## What is the recommended approach to creation automation across multiple spaces?
{: #faq-automation-spaces}
{: faq}

For example, your environment has one org and three spaces.  One space is for development, another for staging, and the final one for production.  Should you create a single Secure Gateway instance or multiple (that is, one for each space)?  If you can create multiple gateways, are there any considerations for reusing a Node.js application to create a gateway and destination in each space?

The approach is as follows:
- You can create a single Secure Gateway instance for all three spaces.  However, you must remember the gateway and destination [limitations for your specific plan](/docs/services/SecureGateway?topic=SecureGateway-secure-gateway-service-plans).
- There are no additional considerations for reusing a Node.js application as no service bindings are required by Secure Gateway.


## What is the recommended approach to creation automation across multiple orgs?
{: #faq-automation-orgs}
{: faq}

Consider an environment that has three orgs: one for development, one for staging, and one for production.  Is a Secure Gateway service instance required for each org and is the configuration available to all spaces within that org?

For automation across orgs, the approach is as follows:
- You are not required to have a Secure Gateway service instance in each org.  You could have an instance in one org and use the gateways within that instance from all of your other environments.  With this setup, you must remember the gateway and destination [limitations for your specific plan](/docs/services/SecureGateway?topic=SecureGateway-secure-gateway-service-plans).
- You can have a Secure Gateway service instance in each org and the configuration will be available to all your spaces.

## Does my app need to be in the same space?
{: #faq-app-space}
{: faq}

Do I need to run the Node.js app in the same {{site.data.keyword.Bluemix_notm}} space as the Secure Gateway service?
No, you do not need to run your app in the same {{site.data.keyword.Bluemix_notm}} space as the Secure Gateway service.

## Can a Secure Gateway client handle two gateways on the same machine?
{: #faq-multiple}
{: faq}

Yes.  For a client to handle two gateways, you can specify multiple connections through the command line as shown in this excerpt from [Startup Argument Examples](/docs/SecureGateway?topic=SecureGateway-client-interacting#startup-examples):
```
node lib/secgwclient.js <gateway_id_1> <gateway_id_2> -t <security_token_1>--<security_token_2>
```

## Can I retrieve error level logs for the Secure Gateway server?
{: #faq-server-logs}
{: faq}

Error level logs on the server cannot be retrieved.  Only errors that are made at the time of the request can be seen.

## Where can I find the client logs for the Secure Gateway client? 
{: #faq-client-logs}
{: faq}

By default, the client logs can be found at the following locations:

- Windows: `%Installation_directory%/ibm/securegateway/client/logs/securegw_win_service.log`
- Linux: `/var/log/securegateway/client_console.log`
- AIX: `/opt/ibm/securegateway/client_console.log`

You can change the location by using the `-p` option when starting the Secure Gateway client. See also [Startup Arguments and Options](/docs/SecureGateway?topic=SecureGateway-client-interacting#startup-args).


## What are the functional states of Secure Gateway?
{: #faq-states}
{: faq}

The different lifecycle states of gateways and destinations are as follows:

### Non-functional State
{: #states-answer-non-functional}
The 1.7.0 release introduced a new tiered plan pricing model. With this model came the ability to mark both Gateways and Destinations as 'Active' or 'Inactive'. Part of the new plan billing structure charges the user for the number of Gateways and Destinations that they have.

- Inactive items are not billed
- Inactive Destinations do not have a provisioned cloud port.
- All Destinations within an inactive gateway will also be inactive and cannot be reactivated until their Gateway is also set Active.
- Inactive items are considered Non Functional. Inactive items cannot be in any of the Functional states.

When you [downgrade the plan](/docs/SecureGateway?topic=SecureGateway-secure-gateway-service-plans#changing-plans), it will update all gateways to be inactive, all provisioned cloud port of the destinations will be reset. If you want to reactivate your gateway, you can click the wrench button in the gateway panel to configure the Non-functional State. For details, see [Reactivate gateway](/docs/SecureGateway?topic=SecureGateway-add-sg-gw#reactivate).

### Functional States
{: #states-answer-functional}
When a gateway or destination is marked active it will be billed. Active states for Gateways and Destinations are below:

- Enabled - the gateway or destination is ready for use under normal circumstances.
- Disabled - the gateway or destination has been disabled.
    - Gateways - clients will be unable to flow data to the disabled gateway.
    - Destinations - clients will be unable to create connections to these disabled destinations.

## How do I know the data activities on the Secure Gateway Client?
{: #faq-data-size}
{: faq}

On SecureGateway Client, change the log level to TRACE to know the data activities on the client. The following information will be displayed after requests are sent.

Data size sent from request application:
```
[TRACE] Connection #<connection ID> transmitted data: <size> bytes
```

Data size sent from destination:
```
[TRACE] Connection #<connection ID> received data: <size> bytes
```

## How do I get the amount of the real-time connections on Secure Gateway?
{: #faq-connect-num}
{: faq}

### To get connections information such as the amount of the real-time connections, the data size sent and received from Secure Gateway Client, do the following:

- On Secure Gateway client interactive command line:
Type `s` to print the connection status details. 
- On Secure Gateway client UI, click the Connection Information menu

The following connection statistics would be displayed: 
- Overall data size transmitted.
- Overall data size received.
- The amount of total connections.
- Real-time active connections.

Note: This connections information is on Client level, not in Gateway level. If you need connections information in Gateway level, please check each Client which connected to that Gateway.

## How many concurrent connections can Secure Gateway handle?
{: #faq-concurrent}
{: faq}

If the connections are being rejected or having latency, the connections might be exceeding concurrent connections limit.

The Secure Gateway can handle only 250 concurrent connections. For details, see [Limitations](/docs/SecureGateway?topic=SecureGateway-client-interacting#limits).

For SG client `v186 and later`, there will be error shown in SG client log in each 30 seconds when the concurrent connections exceed the limit on cloud side.

## What are the recommended configurations to make my connections more secure?
{: #faq-secure-app}
{: faq}

Set these configuations to make connections more secure:

### Reject unauthorized
{: #secure-app-answer-reject-unauth}
To protect against Man-in-the-middle attack, reject connections to the resource which is not authorized with the list of supplied CAs. On the Resource Authentication, check the box `Reject unauthorized`, then upload the certificate if the certificate of the resource is self-signed. Please see [Cloud/On-Premises Authentication](/docs/services/SecureGateway?topic=SecureGateway-add-dest#cloud-or-on-prem-auth) for more information.

### Use Mutual Authentication
{: #secure-app-answer-ma}
Enable Mutual Authentication for both sides of on-premise destinations makes Secure Gateway more secure. On User Authentication side, enable mutual authentication to restrict the access of Secure Gateway cloud node by authenticating using a client certificate when the request is over TLS/HTTPS. On Resource Authentication side, enable mutual authentication to provide appropriate credential when connecting to destination endpoint, and ensure secure/encrypted access to on-premise resource. Please see [Configuring Mutual Authentication](/docs/services/SecureGateway?topic=SecureGateway-add-dest#dest-mutual-auth) and [Node.js TLS Mutual Authentication](/docs/services/SecureGateway?topic=SecureGateway-nodejs-tls-ma#nodejs-tls-ma) for more information.

### Set IP Table Rules (For on-premises destination)
{: #secure-app-answer-iptables}
The Secure Gateway cloud host and port of an on-premises destination is in the public space; therefore it is allowed everyone to access by default.
To control the traffic accessing on Secure Gateway, set iptables rules to only allow access by a specific range of IPs and ports to secure on-premises resources. Please see [IP Table Rules](/docs/services/SecureGateway?topic=SecureGateway-add-dest#dest-network-security) for more information about how to configure the iptables rules on Secure Gateway.

### Configure Access Control List (For on-premises destination)
{: #secure-app-answer-acl}
Configure Access Control List support to allow or restrict access to on-premises resources would make the on-premises destinations more secure by specifying the access right on the specific destination host and port. It is recommended to define the allowed or restricted HTTP/S routes on the ACL entries as well to enhance the security of on-premises destination. Please see [Access Control List](/docs/services/SecureGateway?topic=SecureGateway-acl#acl) and [HTTP/S Route Control using the ACL](/docs/services/SecureGateway?topic=SecureGateway-acl#acl-route-control) for more information.

### Set password on the Secure Gateway Client UI
{: #secure-app-answer-ui-pw}
It is recommended to set the UI password to restrict the access of the Secure Gateway Client UI. Please see [Interacting with the Client](/docs/services/SecureGateway?topic=SecureGateway-client-interacting#client-interacting) for more details about how to set the password using startup configuration or interactive commands on Secure Gateway Client terminal command line.

## What is gateway migration? Why is the domain changed after 2018 December?
{: #faq-gateway-migration}
{: faq}

After 2018 December maintenance, the cloud host of Secure Gateway is getting renamed to use the `securegateway.appdomain.cloud` instead of `integration.ibmcloud.com`, and use the `securegateway.cloud.ibm.com` for gateway authentication instead of `bluemix.net`. For backward compatibility, the existing gateway will keep using the old domain until the gateway is migrated, and the SG client v180fp9 and former will keep using `bluemix.net` for gateway authentication.  To support this change, there is a migrate button on the gateway panel.

After the migration, the cloud host of the on-premises destinations will change to use the new domain, the users/applications will need to update to send the request to the new cloud host.

Currently the migration is not mandatory and there is not an exact date about when the old domain will be out of support, but once this is settled, the customer who are still using the old domain name will be notified.

## Where can I receive Secure Gateway notifications, especially for disruptive maintenance?
{: #faq-notification}
{: faq}

You can get notifications via our [status page](https://cloud.ibm.com/status?selected=maintenance){: external}.
- To get the notifications about completed disruptive maintenance, please search `Secure Gateway` in the tab [History](https://cloud.ibm.com/status?query=Secure+Gateway&selected=history){: external}.
- To get the notifications about on going or planned disruptive maintenance, please search `Secure Gateway` in the tab [Planned maintenance](https://cloud.ibm.com/status?query=Secure+Gateway&selected=maintenance){: external}.

When the Secure Gateway client disconnected unexpectedly, please go to the status page to check whether there is disruptive maintenance at that time.

## How can we avoid manually restart after the disruptive maintenance?
{: #faq-manually-restart}
{: faq}

If the maintenance needs to have disruption over 10 minutes, then you might need to manually restart the Secure Gateway client to reconnect to the Secure Gateway server after the maintenance. In this case, you can use the [startup options](/docs/services/SecureGateway?topic=SecureGateway-client-interacting#startup-args) `--service` when starting up the Secure Gateway client, such that the parent process of the Secure Gateway client will restart within 60s if all child clients are terminated. Beside that, you can also use the startup options `--reconnect` to define the reconnect attempts after the connection between Secure Gateway client and Secure Gateway server drop.

Normally, the service downtime will be equal to or less than 10 minutes, the Secure Gateway client (after version v180) should be able to reconnect to the Secure Gateway server automatically.

## How do I run the Secure Gateway client as a daemon on AIX?
{: #faq-autostart-aix}
{: faq}

To run the Secure Gateway client as a daemon, you use `forever` along with a script to start the Secure Gateway client as a background process on AIX.  Steps and details are in the [AIX](/docs/SecureGateway?topic=SecureGateway-auto-start-conf#auto-start-aix) section of [Configuring Auto-start for the Client](/docs/SecureGateway?topic=SecureGateway-auto-start-conf).

## How can I capture the Secure Gateway Client logs on DataPower?
{: #faq-dp-log}
{: faq}

The event category of Secure Gateway Client logs is `sgclient`. You can create a [log target](https://www.ibm.com/support/knowledgecenter/en/SS9H2Y_7.7.0/com.ibm.dp.doc/logtarget_logs.html){: external} to write the logs with specific event category to a file on DataPower. Following is an example:

- From the default domain:
    - GUI side panel select `Object` → `Logging Configuration` → `Log Target`. Or search for `Log Target` in the `Search` field.
    - Select the `Add` button to add a log target
- In the `Main` tab:
    - Fill in `Name`
    - `Target Type` of `File`
    - `Log format` of `Text`
    - Fill in `File Name` to define the output location, for example: `logtemp:///sgclient.log`
    - Select `Archive Mode` to `Rotate`
- In the `Event Subscription` Tab:
    - Fill in `Name`
    - Select the `Add` button to add a target event subscription
    - Fill in the `Event Category` selecting `sgclient`
    - Fill in the `Minimum Event Priority` of `debug`

## Which ports does the Secure Gateway client use?
{: #faq-ports}
{: faq}

The Secure Gateway Client uses outbound port 443 and port 9000 to connect to npm registry and the IBM Cloud environment.  See [Network requirements](/docs/SecureGateway?topic=SecureGateway-client-requirements#network-requirements) for details.

## Does the Secure Gateway server support High Availability?
{: #faq-ha}
{: faq}

The server does not support High Availability (HA) nor provide redundancy.  To avoid interruption during planned maintenance or outages, you could use the gateway server in another region.  You can achieve HA with multiple client connections to the same gateway ID as allowed by your [Secure Gateway Service Plan](/docs/SecureGateway?topic=SecureGateway-secure-gateway-service-plans).  For additional information, see [High Availability](/docs/SecureGateway?topic=SecureGateway-about-sg#about-ha).

## What should I do when the security token is expired?
{: #faq-token-expired}
{: faq}

The security token could be regenerated in the edit board of the gateway panel, you can click the link `Regenerate Token` which next to `Token Expiration` to regenerate the token. The `Save` button cannot be used to regenerate the token. For additional information, see [Regenerate security token](/docs/SecureGateway?topic=SecureGateway-add-sg-gw#regen-sectoken)
