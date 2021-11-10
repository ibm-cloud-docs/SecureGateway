---

copyright:
  years: 2015, 2021
lastupdated: "2021-03-17"

subcollection: SecureGateway

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:deprecated: .deprecated}
{:external: target="_blank" .external}

# Interacting with the Client
{: #client-interacting}

There are a few ways to interact with the client:

- Through terminal command line prior to startup
- After startup, using the client's interactive command line
- After startup, using the client's local UI

## Startup Configurations
{: #startup}

### Startup Arguments and Options
{: #startup-args}

The following table describes all of the available options that can be provided alongside the startup command for the client.  Using these will allow complete configuration of the client prior to startup rather than requiring individual setup once the client is running.

| Parameter and Arguments | Description |
| ------------- | ----------- |
| &lt;gateway ID&gt; | Connect to {{site.data.keyword.Bluemix_notm}} by using the gateway ID that is provided |
| -F, -\-aclfile &lt;file&gt; | Access control List file |
| -g, -\-gateway &lt;hostname:port&gt; | Used to manually select a specific gateway destination (advanced use only) |
| -l, -\-loglevel &lt;level&gt; | Change the log level to ERROR, INFO, DEBUG or TRACE |
| -p, -\-logpath &lt;file&gt; | Direct logging to a specific file |
| -P, -\-port &lt;port&gt; | The port for the UI to run on.  Defaults to port 9003 |
| -r, -\-reconnect &lt;reconnect attempts&gt; | Reconnect attempts after drop the connection between SG client and SG server, -1 means retry forever. Defaults to 20 |
| -t, -\-sectoken &lt;security token&gt; | The security token to use for this gateway connection |
| -w, -\-password &lt;password&gt; | The password to protect the UI with.  Defaults to no password |
| -x, -\-proxy &lt;proxy agent&gt; | (For SG client v182fp2 and former) The proxy for the port 9000 wss connection |
|                                   | (For SG client v183 and later) The proxy for the port 9000 wss connection and port 443 gateway authentication connection |
| -\-noUI | Prevent the UI from starting up automatically |
| -\-allow | Allows all connections to the client. Is overridden by the ACL file, if provided |
| -\-service | After an initial connection, the parent will restart within 60s if all child clients are terminated |

<b>Note:</b> `--service`, `--allow`, and `--noUI` flags should be the last parameters in the command line arguments.

The most basic use case is to start with a single client connection with the default settings:

```
<gateway_id> -t <security_token>
```
{: pre}

These options can be extended to support the automatic connection of multiple clients, as well.  In order to pass these to multiple client connections, they must be passed in using a specific format.  The gateway IDs can be passed in the same way as with the single client connection (with each gateway ID separated by a space); however, the order of these IDs determine the order the rest of the arguments must follow.  When passing in any of the other arguments, they must be separated by `--` in order to be picked up correctly.  If nothing is passed in for a particular flag, it is assumed to not apply to any of the clients.

If there aren't enough arguments supplied to fulfill all of the gateway IDs, then they will be applied in order until there are no more arguments.  For example, if two gateway IDs are passed in and a single security token is passed in, then the token will be applied to the first gateway ID and not the second one.  

If gateway IDs are provided that require different arguments, then the keyword `none` should be designated in the place of any particular argument that a gateway is not enforcing/supplying.  For example, if three gateway IDs are passed in and the user would like to specify a loglevel for the first and third, the argument would look like `--loglevel DEBUG--none--TRACE`.  In this case, none would then default to INFO.

### Startup Argument Examples
{: #startup-examples}

Single client connection, custom loglevel, no UI:

```
node lib/secgwclient.js <gateway_id> -t <security_token> -l <loglevel> --noUI
```
{: pre}

Multiple client connections, default options:

```
node lib/secgwclient.js <gateway_id_1> <gateway_id_2> -t <security_token_1>--<security_token_2>
```
{: pre}

Multiple client connections, all custom settings:

```
node lib/secgwclient.js <myGatewayID_1> <myGatewayID_2> -t none--<token for gateway 2> -l DEBUG--TRACE -p <full path to log file for gateway 1>--<full path to log file for gateway 2> -F <full path to ACL file for gateway 1>
```
{: pre}

Return to [Getting Started - Adding a Client](/docs/services/SecureGateway?topic=SecureGateway-add-client).

## Interactive Configuration
{: #interactive}

### Interactive Commands
{: #interactive-commands}

The {{site.data.keyword.SecureGateway}} client has a command line interface (cli) and shell prompt for easy configuration and control. The interactive environment supports a richer sets of capabilities than the command line arguments, this is to facilitate better interactive control over the client.

| Interactive Commands | Description       |
| ------------- | ----------- |
| A, acl allow &lt;hostname:port&gt; &lt;worker ID&gt; | Access control list allow |
| D, acl deny &lt;hostname:port&gt; &lt;worker ID&gt; | Access control list deny |
| N, no acl &lt;hostname:port&gt; &lt;worker ID&gt; | Remove an access control list entry |
| S, show acl &lt;worker ID&gt; | Show all access control list entries |
| F, acl file &lt;file&gt; &lt;worker ID&gt; | Access control List file |
| C, displayconfig &lt;worker ID&gt; | Display the current {{site.data.keyword.SecureGateway}} configuration, if available |
| t, sectoken &lt;security token&gt; | The security token to use for the next gateway connection |
| c, connect &lt;gateway ID&gt; | Connect to {{site.data.keyword.Bluemix_notm}} by using the gateway ID that is provided |
| l, loglevel &lt;level&gt; &lt;worker ID&gt; | Change the log level to ERROR, INFO, DEBUG or TRACE |
| p, logpath &lt;file&gt; &lt;worker ID&gt; | Direct logging to a specific file |
| r, reverse &lt;worker ID&gt; | List the ports the client is currently listening on for reverse destinations |
| k, kill &lt;worker ID&gt;  | Terminate the specified worker |
| e, select &lt;worker ID&gt;  | Specifies a worker to perform commands on unless otherwise specified |
| d, deselect | Deselects the previously specified worker.  Issue select command to specify another |
| w, password &lt;old password&gt; &lt;new password&gt; | Set the UI password.  If &lt;new password&gt;  is blank, no password will be enforced. &lt;old password&gt; required on password update. Passwords must contain only letters |
| P, port &lt;new port&gt; | Change the port that the UI is listening on |
| u, uistart &lt;initial password&gt; &lt;port&gt; | Starts the UI on localhost:&lt;port&gt;/dashboard. If &lt;initial password&gt; is blank and no other password has been set for the session, no UI password will be enforced. If &lt;port&gt; is blank the UI will be reachable on 9003 |
| U, uistop | Closes the UI associated with this client session. The session will only be accessible via CLI until a new UI is manually started |
| R, revoke | Clears all UI authorizations associated with this client session |
| q, quit | Disconnect and exit |
| s, status &lt;worker ID&gt;  | Print the status details of the tunnel and its connections |
| L, list | Displays a list of the currently associated workers |

<b>Note:</b> If a connection has been specified with the `select` command and another command is called without providing a worker ID, the command will attempt to run on the connection specified by `select`.

For more details on configuring the Access Control List, [click here](/docs/services/SecureGateway?topic=SecureGateway-acl).

Return to [Getting Started - Adding a Client](/docs/services/SecureGateway?topic=SecureGateway-add-client).

## Client UI
{: #client-ui}

<b>Note:</b> The Client UI is not supported when using Docker on Windows or MacOS.

The client UI provides a local web interface for the user to interact with the {{site.data.keyword.SecureGateway}} Client rather than the CLI.  By default, this UI is available at `localhost:9003/dashboard`  The UI is split into the following pages:

### Connect
{: #ui-connect}

This is the initial landing page for the UI where a user can provide a gateway ID and security token to connect their first client.

### Login
{: #ui-login}

This page will be displayed if the UI has been password protected.  If this page is reached while no password is being enforced, please refresh the page to be redirected.

### Dashboard
{: #ui-dashboard}

This is the main page once a client has been connected.  From here, you can access the View Logs page, the Access Control List page, and the Connection Info page.  At the bottom, you can also choose to disconnect one/many of the connected clients.  On the top of the page, the currently selected client will be displayed, and there will be an option to connect additional clients. 

### View Logs
{: #ui-logs}

This page will allow you to see the logs being generated by the selected client (shown in the upper right of the page).  The displayed logs can be filtered by the check boxes below the logs.

### Access Control List
{: #ui-acl}

This page will allow you to manipulate the Access Control List for the selected client (shown in the upper right of the page).  Rules can be individually added to the allow/deny tables or a file can be uploaded at the bottom of the page.

### Connection Info
{: #ui-info}

This page will show the current connection information for the selected client (shown in the upper right of the page).  Information such as gateway description, number of current connections, and reverse destination listeners can be seen here.

Return to [Getting Started - Adding a Client](/docs/services/SecureGateway?topic=SecureGateway-add-client).

## Remote Client Termination
{: #client-remote}

If a client has been provided an ID, then it can be remotely terminated via the SG UI or through the SG API.  If you terminate a client that is running as a service, the client will restart and obtain a new client ID; however, if the service has multiple clients connected, the terminated client will not restart until all of the remaining clients have been terminated.

## Client behind proxy
{: #client-proxy}

The Secure Gateway client uses outbound port 443 and port 9000 to connect to npm registry and the {{site.data.keyword.Bluemix}} environment as mentioned in the [network requirement](/docs/services/SecureGateway?topic=SecureGateway-client-requirements#network-requirements). If the Secure Gateway client use the proxy for the outbound request, you can use following option to define the proxy configuration.

### Port `443` for gateway authentication connection and port `9000` for WSS connection
You can use the [startup option](#startup-args) `--proxy` to define the proxy agent for the outbound request to the Secure Gateway server, for example, if you are using the auto-start [configuration file](/docs/services/SecureGateway?topic=SecureGateway-auto-start-conf#auto-start-linux):
```
SECGW_ARGS="--no_license --l $LOGLEVEL -x <proxy_info> --service"
```

Note:

For the Secure Gateway client v182fp2 and former, the proxy option only support for the port `9000` wss connection, if you want to define the proxy for the port `443` gateway authentication connection, you can add the environment variable `http_proxy` or `https_proxy` to the configuration file, for example:
```
SECGW_ARGS="--no_license --l $LOGLEVEL -x http://prox-server:3128 --service"
export http_proxy=http://prox-server:3128
```

Or you can define the environment variable before starting the interactive command line interface.

### Port `443` for npm module installation
For the Secure Gateway client v185 and later, You can define the proxy option for the npm module installation during the Secure Gateway client installation. For example (MacOX):
```
Enter the gateway IDs, separated by spaces: <gateway_id>
INFO: The gateway ID for this install is: <gateway_id>

Enter the gateway ID security tokens separated by spaces, enter 'none' for no security token: <token>

Enter the proxy info if you want to start the client with the proxy: <proxy_info>
```

If you are using http proxy, the proxy option should be `http://<proxy host>:<proxy port>`, if you are using https proxy, the proxy option should be `https://<proxy host>:<proxy port>`

## Limitations
{: #limits}

### Connection Limitations
{: #limits-conn}

The SG gateway can only handle 250 concurrent connections. If the number of concurrent requests exceeds the limit, it can result in the connection attempts being rejected and lead to latency. An easy way to fix this is to use connection pooling on the calling app. Please note that the limit of 250 concurrent connections is on the gateway and not client or destination. This limit will be shared across all the client and destinations on the gateway.

### DataPower Client Limitations
{: #limits-datapower}

The {{site.data.keyword.SecureGateway}} DataPower Client is in the process of being updated to match the capabilities of the rest of our clients.  It currently has the following limitations:

- ACL will default to ALLOW ALL
- Enabling and disabling gateways or destinations from the {{site.data.keyword.SecureGateway}} UI is not supported. However, the Administrative State option in the DataPower UI functions as an on/off switch for that particular client.
- Connection status polling for real-time connected and disconnected gateway status updates is not supported.
- Full certificate chains with destination-side TLS are not supported prior to DataPower version 7.5.1.0
- Cloud destinations are not supported prior to DataPower version 7.5.1.0
- The log level can't be changed to TRACE level
- The latest Secure Gateway Client version in DataPower is 1.8.5fp1, please check [here](/docs/services/SecureGateway?topic=SecureGateway-client-install#installing-datapower) for more information
