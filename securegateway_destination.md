---

copyright:
  years: 2015, 2020
lastupdated: "2020-10-20"

subcollection: SecureGateway

---

# Adding a Destination
{: #add-dest}

A destination is a definition of how to connect to a specific on-premises or cloud resource. Once the destination has been created, the {{site.data.keyword.SecureGateway}} servers will provide it with a unique public endpoint where it will listen for connections while the gateway is connected.

![Dashboard with no destinations](./images/emptyDestinations.png?raw=true "Dashboard with no destinations")

From within your new gateway and on the Destinations tab, click the Add Destination button to open the Add Destination panel.  There are two methods for creating your destination:  a guided setup that shows how each piece fits into the overall architecture and an advanced setup that provides all fields and options on a single panel.  

The guided setup `does not` allow for the configuration of proxy information, server name indicators, reject unauthorized or the upload of a destination-specific cert/key pair.  After creation, all fields are available via the Edit Destination panel.

## Guided Setup Panel
{: #add-dest-guided-setup}

![Guided Setup](./images/guidedLanding.png?raw=true "Guided Setup landing panel")

## Advanced Setup Panel
{: #add-dest-advanced-setup}

![Advanced Setup](./images/advancedLanding.png?raw=true "Advanced Setup landing panel")

## On-premises vs Cloud Destinations
{: #dest-types}

The first question to answer when creating your destination is where the resource resides that you need to connect to.

### On-premises Destination
{: #dest-types-on-prem}
The on-premises destination is for the use case where an application in the public space needs access to a restricted resource located on-premises.
![On Premises destination](./images/onPremDestination.png?raw=true "On-premises Destination")

### Cloud Destination
{: #dest-types-on-cloud}
The cloud destination is for the use case where an application located in a restricted network needs access to a resource that is available in a public space.
![Reverse Destination](./images/reverseDestination.png?raw=true "Cloud Destination")

## Defining your Destination
{: #define-dest}
For both types of destinations, the following information is required:

- `Resource Hostname`: This is the IP or hostname of the resource you need to connect to.
- `Resource Port`: This is the port that your resource is listening on.
- `Protocol`: This is the type of connection your application will be making.  See the table below for the various [protocol options](#protocols-options).  For configuring the type of connection your resource is expecting, check the [Resource authentication](#dest-resource-auth) section.

If you have selected a cloud destination you will also need to provide a `Client Port`.  This is the port that the {{site.data.keyword.SecureGateway}} Client will listen on to allow connections to the associated resource hostname and port.

## Protocol options
{: #protocols-options}

This table provides all of the available options for how your application can initiate connections/requests with {{site.data.keyword.SecureGateway}}.

Protocol | Description
-- | --
TCP | No authentication. Your application can communicate directly with the {{site.data.keyword.SecureGateway}} servers without requiring any certificates.
TLS: Server Side | Transport Layer Security (TLS) is enabled and the server provides a certificate to prove its authenticity.
TLS: Mutual Auth | The server provides a set of certificates and your application must also provide a certificate as part of its connection.
HTTP | TCP connection where the host header is rewritten to match the on-premises hostname.
HTTPS | TLS: Server Side connection where the host header is rewritten to match the on-premises hostname.
HTTPS: Mutual Auth | TLS: Mutual Auth connection where the host header is rewritten to match the on-premises hostname.


## Configuring Mutual Authentication
{: #dest-mutual-auth}

For protocols enforcing mutual authentication, you will need to upload your own certificate or the server will automatically create a self-signed certificate/key pair for your application to use.  This pair can be downloaded alongside the server certificate.
![Mutual Authentication Panels](./images/mutualAuth.png?raw=true "Mutual Authentication panels")

### User Authentication
{: #dest-user-auth}

The user authentication section is for managing the authorization of your requesting/connecting application with {{site.data.keyword.SecureGateway}}.  This field accepts a single certificate which should be the certificate your application will be presenting alongside any connection/request.

### Resource Authentication
{: #dest-resource-auth}

Resource authentication determines how {{site.data.keyword.SecureGateway}} will attempt to connect to the defined resource.  Three options are available:  None, TLS (server-side), and Mutual Auth.  Depending on your choice, different authentication options become available.

Enabling TLS on the connection to your resource is separate from the TLS used for User Authentication.  TLS for User Authentication secures the access between your initial requesting application and {{site.data.keyword.SecureGateway}} (e.g., between your {{site.data.keyword.Bluemix_notm}} app and the {{site.data.keyword.SecureGateway}} servers) while TLS for Resource Authentication secures the connection between {{site.data.keyword.SecureGateway}} and your defined resource (e.g., between the {{site.data.keyword.SecureGateway}} client and your on-premises database).

#### Cloud/On-Premises Authentication
{: #cloud-or-on-prem-auth}

This option becomes available by selecting TLS or Mutual Auth for your [Resource Authentication](#dest-resource-auth).  The name of the field will match the [type of destination](#dest-types) you have chosen.

To reject any connection which is not authorized with the list of supplied CAs, check the box `Reject unauthorized` from the Cloud/On-Premises Authentication panel. Once the box is checked and the resource you are connecting to is using a self-signed certificate, you must upload it, or the connection will be rejected.

It is allows for up to 6 certificates to be uploaded in order to validate the certificate of the resource which you are connecting to. These files will be added to the CA of connection to the resource and should contain the certificate or certificate chain that your resource will be presenting.

The box `Reject unauthorized` is supported only when using Secure Gateway Client v1.8.4 or later, old Secure Gateway Client will still reject unauthorized even if this box is unchecked.

Uncheck the box `Reject unauthorized` will leave you vulnerable to Man-in-the-middle attack.

#### Server Name Indicator (SNI)
{: #dest-sni}
This option becomes available by selecting TLS or Mutual Auth for your [Resource Authentication](#dest-resource-auth).  This is used to allow a separate hostname to be provided to the TLS handshake of the resource connection.

### Client Certificate and Key
{: #dest-client-cert-key}
Where the Client Certificate and Key fields appears depends on the [type of destination](#dest-types) you have chosen.  In both situations, the files provided here will be used by the SG Client to identify itself for TLS connections.  If no files are uploaded, the {{site.data.keyword.SecureGateway}} servers will automatically generate a self-signed pair with a CN of `localhost`.  For instructions on how to generate a certificate/key pair, [click here](/docs/services/SecureGateway?topic=SecureGateway-cert-key-management).

For an on-premises destination, it will appear under [Resource Authentication](#dest-resource-auth) if `Resource Authentication: Mutual Auth` has been selected.  In this case, the SG Client will use this certificate/key pair for its outbound connection to the defined resource, the On-prem resource needs to add this certificate to its CA to communicate with the SG Client.

For a cloud destination, it will appear under [User Authentication](#dest-user-auth) if a TLS protocol has been selected.  In this case, the SG Client will use this certificate/key pair to create TLS listeners, the On-prem app needs to add this certificate to its CA to communicate with the SG Client.

## Configuring Network Security
{: #dest-network-security}
To prevent all but specific IP addresses from connecting to your cloud hosts and ports, you can choose to enforce iptables rules on your on-premises destination.
![Network Security Panel](./images/networkSecurity.png?raw=true "Network Security panel")

To enforce iptables rules, check the box `Restrict cloud access to this destination with iptables rules` from the Network Security panel.  Once the box is checked, you can begin adding the IPs that should be allowed to connect.  If no IPs are provided, all connections to this cloud hosts and ports will be rejected as long as the `Restrict cloud access` box is checked.

<b>Note</b>: The IPs or ports provided must be the external IP address that the {{site.data.keyword.SecureGateway}} servers will see, not the local IP address of the machine making the request.

### Adding iptables rules
{: #dest-iptables}
When adding rules to iptables, you can provide individual IPs or an IP range along with either a single port or a port range.  All ranges provided are inclusive.  The following table has some examples as well as how they will resolve within iptables:

IP Addresses | Ports | Results
-- | -- | --
1.2.3.4 | 5000 | Only IP 1.2.3.4 from port 5000 will be allowed.
1.2.3.4-2.3.4.5 | 5000 | All IPs between 1.2.3.4 and 2.3.4.5 from port 5000 will be allowed.
1.2.3.4 | 5000:10000 | Only IP 1.2.3.4 from ports 5000 to 10000 will be allowed.
1.2.3.4-2.3.4.5 | 5000:10000 | All IPs between 1.2.3.4 and 2.3.4.5 from ports 5000 to 10000 will be allowed.
1.2.3.4 | | Only IP 1.2.3.4 from any port will be allowed.
| 5000 | Any IP from port 5000 will be allowed.

Specific rules can also be associated with an application.  For more information on creating associated rules, see [how to create iptables rules for your app](/docs/services/SecureGateway?topic=SecureGateway-iptables-rules).

## Configuring Proxy Options
{: #dest-proxy}
If your on-premises destination is located behind a SOCKS proxy, you can configure the proxy settings for your destination in the Proxy Options panel.
![Proxy Options Panel](./images/proxyOptions.png?raw=true "Proxy Options panel")

To configure the proxy settings, you only need to provide the hostname and port that the proxy is listening on as well as the SOCKS protocol that is being used (4, 4a, 5).

## Configuring Miscellaneous options

![Miscellaneous Options panel](./images/miscellaneousOptions.png?raw=true "Miscellaneous Options panel")

To compress the data flowing through the wss connection between Secure Gateway Server and Secure Gateway Client, check the box `Enable Compression` from the Miscellaneous Options panel.

`Connection Timeout` is default to 0, which mean disable the connection timeout. To enable the connection timeout, edit the text field with the number of seconds you would like the connection timeout be (minimum 1, maximum 180).

The session TTL of our firewall is 3600 seconds. If the connection between the cloud application and the Secure Gateway Server hangs (no data flowing) over than 3600 seconds, our firewall will drop the connection even if the `Connection Timeout` is disabled. In this case, you can enable the TCP keep-alive on the cloud application, to keep the connections alive.

## Destination Settings
{: #dest-settings}
Once your destination has been created, click the settings icon ![Setting Icon](./images/settingIcon.png?raw=true "Setting Icon") to see the following information:

![Mutual Authentication Info Panel](./images/infoPanelMA.png?raw=true "Mutual Authentication Info Panel")

- The destination ID required to use the API.
- `On-premises destinations will have a cloud host and port.  This information is required by your application in order to connect to your on-premises resource.`
- `Cloud destinations will have a client port.  This is the port the {{site.data.keyword.SecureGateway}} Client will be listening on in order to connect your on-premises application to your cloud resource.`
- The resource host and port this destination is pointed to.
- When the destination was created
- When the destination was last modified

## Reactivate destination
{: #reactivate}

![Manage Activity](./images/manageActivityDestination.png?raw=true "Manage Activity")

After you [reactivate the gateway](/docs/SecureGateway?topic=SecureGateway-add-sg-gw#reactivate), you can reactivate the destination by clicking the wrench button ![Wrench Button](./images/wrenchIcon.png?raw=true "Wrench Button") in the destination panel, you can reactivate your destinations with the arrow button ![Arrow Button](./images/arrowIcon.png?raw=true "Arrow Button"), and the configuration will be applied after you click the `Apply` button. The reactivated On-premises destinations will obtain another cloud host and port.

## Import/Export gateway
{: #import}

If you want to copy the destination configuration to another gateway, you can click the Export button ![Export Button](./images/exportIcon.png?raw=true "Export Button") on the destination tile to export the configuration of the destination in the destination panel of the original gateway, then click the Import button ![Import Button](./images/importIcon.png?raw=true "Import Button") to import the configuration in the destination panel of the target gateway. The imported On-premises destinations will obtain another cloud host and port.
