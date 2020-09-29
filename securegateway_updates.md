---

copyright:
  years: 2015, 2020
lastupdated: "2020-09-29"

subcollection: SecureGateway

---

# {{site.data.keyword.SecureGateway}} Change Log
{: #secure-gateway-change-log}

Our regression tests only cover last 3 versions.

Please ensure your Secure Gateway Client does not fall more than 3 versions behind, or you might get unexpected behaviour. 

If you are using DataPower as the Secure Gateway Client, please ensure it is a currently supported DataPower version as well.

## v1.8.6
{: #v186}

Published date: 2020-09-22

### Fixes
{: #v186-fixes}

- Resolves potential vulnerabilities
- Resolves unexpected websocket fatal error
- Resolves error when update destination too frequently
- Resolves incorrect UI version info
- Resolves DataPower SG client unexpected restart issue
- Enhance concurrent connection limit counting and logging
- Resolved incorrect UI version info

## v1.8.5fp2
{: #v185fp2}

Published date: 2020-04-22

### Fixes
{: #v185fp2-fixes}

- Resolves the error when starting the Secure Gateway Client UI in Windows version

## v1.8.5fp1
{: #v185fp1}

Published date: 2020-02-12

### Features and Fixes
{: #v185fp1-features-and-fixes}

- Upgrade Node.js to 10.19.0
- Resolves the error when starting the Secure Gateway Client as Windows service

## v1.8.5
{: #v185}

Published date: 2020-01-07

### Features
{: #v185-features}

- Upgrade Node.js to 10.17.0
- Node modules vulnerabilities fix
- Resolves the Node.js conflict on Windows installer
- Add proxy option to the installer for npm module installation

### Breaking changes
{: #v185-breaking-changes}

Please be aware the breaking changes for the Node.js version upgrade, for example, the OpenSSL is upgraded to 1.1.1d because of Node.js upgrade which might cause some breaking changes on the TLS connections since the security policy is stricter than before, customer will get error when enable `Reject unauthorized` and connect to the endpoint which using cert/key which generate with insecure algorithm.

## v1.8.4
{: #v184}

Published date: 2019-10-22

### Features
{: #v184-features}

- Support the toggle `rejectUnauthorized` for Resource Authentication

## v1.8.3 Fixpack 1
{: #v183fp1}

Published date: 2019-08-21

### Features and Fixes
{: #v183fp1-features-and-fixes}

- Resolves error when sending the large data with HTTP/HTTPS request to on-cloud destiantion
- Resolves data truncated issue when sending data to on-perm destination
- Resolves the docker version Secure Gateway Client UI issue

## v1.8.3
{: #v183}

Published date: 2019-07-05

### Features and Fixes
{: #v183-features-and-fixes}

- Resolves Windows installer vulnerability for Secure Gateway Client UI Password
- Resolves unexpected behaviour when configure Secure Gateway Client UI port and password
- Resolves the error when generating logs
- Add the startup option to define the reconnect attempts
- Add `addtional startup option` field to the auto-start config
- Add proxy support for the gateway authentication request to port 443

### Breaking changes
{: #v183-breaking-changes}

For Windows installer, the old Secure Gateway Client UI Password configuration cannot be reused since this release. Please remove the UI Password configuration before you reuse the old config file during installation.

## v1.8.2 Fixpack 2
{: #v182fp2}

Published date: 2019-05-21

### Fixes
{: #v182fp2-fixes}

- Resolves Secure Gateway Client UI vulnerability
- Resolves resource auth vulnerability
- Enhance the handling when both side close the connection in the same time
- Resolves collapsed resource when the on-prem destination connection close too fast

### Breaking changes
{: #v182fp2-breaking-changes}

You might get certificate error when the Secure Gateway Client is sending connection to the endpoint since this release, to fix the vulnerabilities, we start to check whether the certificate of the endpoint is authorized with the list of supplied CAs.

To fix the error, you can upload the CAs to the [resource authentication](/docs/services/SecureGateway?topic=SecureGateway-add-dest#cloud-or-on-prem-auth) such that the Secure Gateway Client can trust it, or you can upgrade the Secure Gateway Client to v1.8.4 or later, then disable the `Reject unauthorized` in the [resource authentication](/docs/services/SecureGateway?topic=SecureGateway-add-dest#cloud-or-on-prem-auth).

## v1.8.2 Fixpack 1
{: #v182fp1}

Published date: 2019-04-30

### Fixes
{: #v182fp1-fixes}

- Upgrade Node.js to 8.15.1
- Resolves undefined environment info in Deutsch
- Resolves the incorrect concurrent connection limit for reverse connections
- Resolves collapsed connections when both side end the connection in the same time

## v1.8.2
{: #v182}

Published date: 2019-03-12

### Features and Fixes
{: #v182-features-and-fixes}

- Upgrade Node.js to 8.15.0
- Node modules vulnerabilities fix

### Breaking changes
{: #v182-breaking-changes}

The system requirement is changed since this release. For more information, see [System Requirements](/docs/services/SecureGateway?topic=SecureGateway-client-requirements#system-requirements)

## v1.8.1
{: #v181}

Published date: 2019-01-13

### Features and Fixes
{: #v181-features-and-fixes}

- Resolves regular connections hanging issue when closing
- Uew new path for gateway authentication ([Click here for more info](/docs/services/SecureGateway?topic=SecureGateway-client-requirements#network-requirements))
- Use new mechanism for log generating (Log translation enhance)

### Breaking changes
{: #v181-breaking-changes}

The network requirement is changed since this release. For more information, see [Network Requirements](/docs/services/SecureGateway?topic=SecureGateway-client-requirements#network-requirements)

## v1.8.0 Fixpack 9
{: #v180fp9}

Published date: 2018-11-08

### Fixes
{: #v180fp9-fixes}

- Separate the ConnIndicies for reverse and regular destination
- Accessibility enhancements on client UI.
- Resolves unexpected behaviour when set UI password on command line.
- Resolves unexpected crash when UI closed

## v1.8.0 Fixpack 8
{: #v180fp8}

Published date: 2018-10-03

### Fixes
{: #v180fp8-fixes}

- Enhance the logs
- Add the data size checking when transmitted data

## v1.8.0 Fixpack 7
{: #v180fp7}

Published date: 2018-08-31

### Fixes
{: #v180fp7-fixes}

- Added the TRACE level log for transmitted data
- Add Error level log when reverse dest hit concurrent limit
- Resolves the undefined Secure Gateway Client fixpack info in gateway panel

## v1.8.0 Fixpack 6
{: #v180fp6}

Published date: 2018-03-06

### Fixes
{: #v180fp6-fixes}

- Enhance the logs
- Resolves unexpected behaviour in docker version

## v1.8.0 Fixpack 5
{: #v180fp5}

Published date: 2018-02-13

### Fixes
{: #v180fp5-fixes}

- Resolves the corrupted header
- Enhance the error log

## v1.8.0 Fixpack 4
{: #v180fp4}

Published date: 2017-11-10

### Fixes
{: #v180fp4-fixes}

- Resolves ACL configuration issue

## v1.8.0 Fixpack 3
{: #v180fp3}

Published date: 2017-10-30

### Fixes
{: #v180fp3-fixes}

- Resolves the truncated message
- Resolves the hang connection
- Add option to define timeout on destination

## v1.8.0 Fixpack 2
{: #v180fp2}

Published date: 2017-08-17

### Fixes
{: #v180fp2-fixes}

- Resolves issue when the connection closed by the cloud side
- Resolves issue related to the host header

## v1.8.0 Fixpack 1
{: #v180fp1}

Published date: 2017-08-03

### Fixes
{: #v180fp1-fixes}

- Resolves Docker version Secure Gateway Client UI issue
- Enhance error log when error occur on server side
- Resolves connection issue between Secure Gateway Client and Secure Gateway Server

## v1.8.0
{: #v180}

Published date: 2017-07-06

### Features
{: #v180-features}
- Performance improvement
- Add Secure Gateway Client reconnect logic

## v1.7.1
{: #v171}

Published date: 2017-05-23

### Features
{: #v171-features}
- Resolves Secure Gateway Client UI vulnerability
- Resolves issue related to UDP connections
- Resolves issue related to quality test
- Refactor log generater
- Add the startup option to define the proxy for the 9000 ws connection
- Fixes `EADDRINUSE` error caused by listeners that do not execute a teardown appropriately when a destination is deleted.
- Resolves issue where service instances bound to an application could not be deleted.

## v1.7.0
{: #v170}

Published date: 2017-03-16

### Features
{: #v170-features}

- Introduces new service plans: Essentials, Professional, and Enterprise.
- The Secure Gateway Client now supports SOCKS proxy between itself and the on-prem destination.

## v1.6.1
{: #v161}

Published date: 2017-02-03

### Fixes
{: #v161-fixes}

- Resolves issue where disconnecting multiple clients results in orphaned processes in the connected clients array.
- Now cleans up orphaned connections that resulted in incorrecty paused listeners.

## v1.6.0
{: #v160}

Published date: 2016-10-06

### Features
{: #v160-features}

- Access Control List now supports specific path routing for HTTP/S requests.
- Destinations now support Server Name Indicators for TLS connections.

## v1.5.1
{: #v151}

Published date: 2016-08-10

### Features
{: #v151-features}

- Destination wizard added for destination creation.
- Gateways now support uploading a custom cert/key pair.
- Services are now limited to 50 gateways and 50 destinations per gateway.
- Destinations/Gateways/Services can now be exported/imported.
- Latency tests between client and server capable of being executed from Secure Gateway UI.
- Cleans up orphaned tunnel processes on network disconnect.
- Resolves duplicate port allocation caused by failed destination deletion.
- Resolves HTTP/S issue with non-UTF8 encoding.
- Resolves missing IPC handlers on replacement processes.

## v1.5.0
{: #v150}

Published date: 2016-05-26

### Features
{: #v150-features}

- Client now has a local interactive UI.
- UDP connections are now supported.
- Memory footprint of the client has been drastically reduced.
- Single client mode is no longer supported; multi-client will be default and transparent to a single-gateway client.
- Access Control List synchronizes between clients connected to the same gateway.

## v1.4.2
{: #v142}

Published date: 2016-03-21

### Features
{: #v142-features}

- Clients are now assigned an ID when connecting to the SG server.
- Clients are now terminated remotely via API or the Secure Gateway UI.
- The connected status of a client can be checked via API.
- Destination-side TLS now supports Mutual Authentication.
- Cloud destinations now support Mutual Authentication protocols.
