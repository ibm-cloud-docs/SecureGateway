---

copyright:
  years: 2015, 2019
lastupdated: "2019-09-03"

subcollection: securegateway

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Troubleshooting
{: #troubleshooting}

## Best practices for running the Secure Gateway Client
{: #best-practices}

- Run the Secure Gateway client on an operating system (OS) partition that has network visibility of the services that are bridged by the client itself. For instance, some hosted virtualization environments support multiple network connectivity modes, including NAT and Bridged. Be sure to choose the correct connection type that provides you with access to the {{site.data.keyword.Bluemix}} services from the internet.
- Install the Secure Gateway into your IT environment where your corporate security policy allows. This would typically be in a protected yellow zone or DMZ where your company can institute the appropriate security controls to protect on-premises assets. Always follow your corporate security policies and instructions when you install the Secure Gateway client.
- Before you install a client into your environment, ensure that both the internet and your on-premises assets are accessible and all host names are resolvable by a DNS.

## Initial Troubleshooting Steps
{: #initial-troubleshooting}

- Initiate the failing request from the requesting application
- Check the Secure Gateway Client logs
- If no client logs have been generated from the request, the issue is between the requesting application and the Secure Gateway Servers.  This could range from network reliability to mismatched request protocols to an improper TLS mutual authentication handshake.
- If the client has generated error level logs from the request, then the issue is between the SG Client and the on-prem resource.  Below is a table containing common errors, the issues that typically cause them, and potential methods to troubleshoot them.

Error | Typical Cause | Troubleshooting Methods
--- | --- | ---
ETIMEDOUT | The client is unable to find the hostname/ip to connect to due to network constraints. | Attempt to ping the destination's hostname/ip from the host running the client to ensure network connectivity.  If running the Docker version of the client, bridging the container with the host OS  with `--net=host` may resolve the issue.
ECONNREFUSED | The client has resolved the hostname/ip to connect to but is unable to begin the connection handshake | This is typically caused by a mismatched protocol between the SG Client and the on-prem resource (e.g., the client is attempting a TCP connection to a host:port that is expecting a TLS connection).  In some cases, a firewall rule may cause this error instead of ETIMEDOUT.
ECONNRESET | The client has established a connection to the destination but something went wrong either during the handshake (a TLS handshake error might result in different errors, as well) or while the request was being handled by the on-prem resource. | The logs of the on-prem resource should be checked to confirm no error has caused the connection to be interrupted.  If nothing is found in the on-prem logs, then the destination configuration should be examined to ensure the appropriate protocols (and certificates, if necessary) are being provided to the client for the connection.
REMOTE_RST | There is error occurred in SG Server side. <br><br> For on-prem destination, there is error during the requesting app connecting to the SG Server, or a timeout error when receiving data from the on-prem resource. <br><br> For cloud destination, it could be anything from TLS handshake failure, to errors in the cloud resource | For on-prem destination, please ensure the requesting app using the appropriate protocols to establish the connection with SG Server, if the error occurs when receiving data from on-prem resource, please try to extend/disable the timeout. If the timeout is disabled but still getting this error 3600 seconds after the last data transmitted/received, please implement the keep-alive logic when sending the request on cloud side. <br><br> For cloud destination, the logs of the cloud resource should be checked to confirm no error has caused the connection to be interrupted.  If nothing is found in the cloud resource logs, then the destination configuration should be examined to ensure the appropriate protocols (and certificates, if necessary) are being provided to the client for the connection.

Many applications experience "hangs" after an ECONNRESET occurring on the other end of the tunnel. This is expected. Secure 
Gateway can not replay the RST packet on the other end of the tunnel since the TCP packets have already been ACKed for that
side of the tunnel. Define the timeouts on the application, which never receives an acknowledging response, are the only
method to end the hang.

## Configure your Docker client to restart when your server restarts
{: #docker-auto-restart}

### What is happening
{: #docker-auto-restart-what-is-happening}
When you restart the server where your Secure Gateway client runs, you must manually restart the Secure Gateway Docker client. How can you get the client to start automatically after a restart of the system?

### How to fix it
{: #docker-auto-restart-how-to-fix-it}

- On Linux or UNIX systems:
- Integrate the Docker command into a script that can be called as a result of a CRON job.
- If you are using a Linux work station, you can create an upstart configuration to ensure that the client is started every time that the server is restarted. For more information, see Automatically Start Containers in the Docker website.
- On Windows systems, issue the following command to start the client:

```
for /L %i in (0,0,0) do docker run -it ibmcom/secure-gateway-client <gateway_id>
```
{: pre}

## Connection error message: host is not in cert's CN
{: #not-in-cn}

### What is happening
{: #not-in-cn-what-is-happening}
You are trying to implement on-premises client-side TLS by using the Secure Gateway client and you receive the following error message.

```
[ERROR] Connection #<connection ID> had error: Host: <host name>. is not cert's CN: <mycommonname>

Where:
    - connection ID is a client assigned connection number.
    - host name is the host name for the connection.
    - mycommonname is the FDQN or common name that is used in the certificate.
```
{: screen}

### Why it is happening
{: #not-in-cn-why-it-is-happening}
The Common Name, for example, the server FQDN or YOUR name, between your on-premises application and the certificate that you uploaded into {{site.data.keyword.Bluemix_notm}} for this destination do not match.

- Check the following items:
- You generated the certificate correctly and that you used the correct server FQDN or host name.
- You uploaded the correct certificate into your {{site.data.keyword.Bluemix_notm}} destination for this client.

### How to fix it
{: #not-in-cn-how-to-fix-it}

 1. In the {{site.data.keyword.Bluemix_notm}} UI, go to the Secure Gateway Dashboard.
 2. Select your destination and click the Edit icon.
 3. Click Upload certificate.
 4. Upload the PEM certificate file that is to be used to connect to the on-premises system.

## IP is not in the cert's list
{: #san}

### What is happening
{: #san-what-is-happening}
The CN in the certificate presented is the IP address of the gateway, but the certificate does not have a SAN matching the IP address and the client fails to connect.  

Due to hostname resolution issues we are using the IP address in our destination.  The CN in the certificate presented is the IP address of the gateway, but the certificate does not have a SAN matching the IP address and the client fails to connect

You have created a destination using TLS, but instead of using the destination&apos;s hostname you have used it&apos;s IP address.  When connecting the client the following error is being thrown.

```
[2015-10-15 13:00:04.866] [INFO] Connection #0 is being established to 10.3.20.31:443
[2015-10-15 13:00:05.426] [ERROR] Connection #0 to destination 10.3.20.31:443 had error: IP: 10.3.20.31 is not in the cert's list:
[2015-10-15 13:00:05.427] [INFO] Connection #0 to 10.3.20.31:443 was closed
```
{: screen}

### Why it is happening
{: #san-why-it-is-happening}
What is going on is that the SSL verification code in the gateway client is treating this destination differently because it uses an IP address rather than a hostname.  Instead of matching with the cert's CN, it is looking in the cert's SAN for a match of the IP address.  Since there is no SAN in the cert, it sees it as a bad connection and fails the SSL handshake.

### How to fix it
{: #san-how-to-fix-it}
If you look at the error message it does not say CN, (e.g. [ERROR] Connection ## had error: Host: . is not cert&apos;s CN: ), but the cert&apos;s list, which leads me to believe you have generated your self-signed cert incorrectly. The problem is that the cert was generated using an FQDN or CN with an IP_Address, this will not work since IP addresses are only supported when using SAN.

Method for generating a certificate with an IP as the CN with openssl:

1. Create an openssl config file, I copied mine from /usr/lib/ssl/openssl.cnf
2. Add an alternate_names section to the file like below:

   ```
   [ alternate_names ]

   IP.1 = &lt;my application&apos;s ip&gt;
   ```
   {: codeblock}

3. In the [ v3_ca ] section, add this line:

    ```
    subjectAltName = @alternate_names
    ```
    {: pre}

4. Under the CA_default section, uncomment copy_extensions (extension copying option: use with caution):

    ```
    copy_extensions = copy
    ```
    {: pre}

5. Generate the private key

    ```
    openssl genrsa -out private.key 3072
    ```
    {: pre}

6. Generate the certificate with options about organization

    ```
    openssl req -new -x509 -key private.key -sha256 -out certificate.pem -days 730 -config
    ```
    {: pre}

7. Combine the files

    ```
    cat private.key certificate.pem > SAN.pem
    ```
    {: pre}

8. Load the SAN.pem file into your destination as the client-TLS cert.
9. Load the SAN.pem file into your on-premises application and restart.

10. Your destination can be configured for either TCP, HTTP or HTTPS and you cloud side application should now be able to connect to your on-premises application.

If you encounter an UNABLE_TO_VERIFY_LEAF_SIGNATURE problem, check your openssl.conf file, change the following from:

    ```
    keyUsage = digitalSignature, keyEncipherment
    ```
    {: pre}

to the default:

    ```
    keyUsage = nonRepudiation, digitalSignature, keyEncipherment
    ```
    {: pre}

## Connection error message: DEPTH_ZERO_SELF_SIGNED_CERT
{: #depth-zero}

### What is happening
{: #depth-zero-what-is-happening}
You are trying to implement on-premises client-side TLS by using the Secure Gateway client and you receive the following error message.

```
[ERROR] Connection #<connection ID> to destination <target host>:<target port> had error: DEPTH_ZERO_SELF_SIGNED_CERT

Where:
    - connection ID is a client assigned connection number.
    - target is the on-premise endpoint
```
{: screen}

### Why it is happening
{: #depth-zero-why-it-is-happening}
The destination you defined is missing a client-side certificate.

### How to fix it
{: #depth-zero-how-to-fix-it}
 1. In the {{site.data.keyword.Bluemix_notm}} UI, go to the Secure Gateway Dashboard.
 2. Select your destination and click the Edit icon.
 3. Click Upload certificate.
 4. Upload the PEM certificate file that is to be used to connect to the on-premises system.
 - You can verify your certificate file using following command:
    ```
    openssl s_client -CAfile <CA> -connect <target_host>:<target_port>
    ```
    Where: CA is the certificate file you uploaded, target is the on-premise endpoint.


## How can I interactively load an ACL file in the Docker client?
{: #docker-load-acl}

### What is happening
{: #docker-load-acl-what-is-happening}
Since Docker is a container or virtualized environment, it does not have direct access to your filesystem until the container is actually launched.  This prevents it from reading your host machines filesystem until it is actually launched and running.

### How to fix it
{: #docker-load-acl-how-to-fix-it}
This is what you can do:

- Create a Dockerfile to include the aclfile.txt

```
FROM ibmcom/secure-gateway-client
ADD aclfile.txt /tmp/aclfile.txt
```
{: codeblock}

- Build a new docker image

```
docker build -t ads-secure-gateway-client .
```
{: pre}

- Run new docker image (need to specify -t and -i options, otherwise you will get error, file not found or ENOENT):

```
docker run -t -i ads-secure-gateway-client1  --F /tmp/aclfile.txt
```
{: pre}

- You should get the following output:

```
[2015-09-30 16:50:32.084] [INFO] The current access control list is being reset and replaced by the user provided file: /tmp/aclfile.txt
[2015-09-30 16:50:32.086] [INFO] The ACL file process accepts acl allow :8000
[2015-09-30 16:50:32.087] [INFO] The ACL file process accepts acl deny local
```
{: screen}

## Why do I get 401 Unauthorized when accessing Secure Gateway gateway panel?
{: #gateway-ui-unauthorized}

### What is happening
{: #gateway-ui-unauthorized-what-is-happening}
When you access the Secure Gateway instance via the url `https://cloud.ibm.com/services/securegateway/<sg_instance_id>?env_id=<region>`, and you receive the error message `401 Unauthorized`.

### How to fix it
{: #gateway-ui-unauthorized-how-to-fix-it}
- Access Secure Gateway instance via the portal `https://cloud.ibm.com/resources`.
- Secure Gateway cannot be accessed with space role `Auditor`, please confirm whether you have `Developer` access of the space.

## Getting additional help and support
{: #getting-help-and-support}

If you have technical questions about developing or deploying an application with Secure Gateway, post your question on [Stack Overflow ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://stackoverflow.com/search?q=securegateway+ibm-cloud).  Tag your question with "ibm-cloud" and "secure-gateway" so that it can be found more readily by {{site.data.keyword.Bluemix_notm}} development teams.

If you have question about the service or instructions on how to get started, use the [IBM developerWorks dW Answers ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/topics/securegateway) forum along with the "ibm-cloud" and "securegateway" tags.

For more details on on using these forums, check the [Getting help page here ](https://cloud.ibm.com/docs/get-support?topic=get-support-getting-customer-support#using-avatar).

For information about opening an IBM support ticket, or about support levels and ticket severities, see [Contacting support](https://cloud.ibm.com/docs/get-support?topic=get-support-support-case-severity#support-case-severity).

Please provide as much of the following information as possible when submitting a ticket:

- What OS is the client running on?
- What version of the client is being used (this can be found using the 'C' command on the client)?
- If this is a UI issue, paste or attach any associated browser console logs and screenshots
- Paste or attach any associated requesting application logs and timezone
- Paste or attach any associated Secure Gateway Client logs and timezone
- Provide the details of the destination being used (either a screenshot or fill out the fields below):
   - Destination ID
   - Protocol
   - Destination-side Authentication
   - Uploaded certificates (just names and which Box folder or link they were uploaded to)
