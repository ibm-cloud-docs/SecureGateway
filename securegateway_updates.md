---

copyright:
  years: 2015, 2019
lastupdated: "2019-09-02"

subcollection: securegateway

---

# {{site.data.keyword.SecureGateway}} Change Log
{: #secure-gateway-change-log}

Our regression test only cover last 3 versions and current supported DataPower.

Please ensure your Secure Gateway Client does not fall more than 3 versions behind, or you might get unexpected behaviour. If you are using DataPower, please ensure your DataPower is current supported.


## v1.8.3 Fixpack 1
{: #v183fp1}

### Features and Fixes
{: #v183fp1-features-and-fixes}

- Resolves error when sending the large data with HTTP/HTTPS request to on-cloud destiantion
- Resolves data truncated issue when sending data to on-perm destination
- Resolves the docker version SG client UI issue

## v1.8.3
{: #v183}

### Features and Fixes
{: #v183-features-and-fixes}

- Resolves Windows installer vulnerability for SG client UI Password
- Resolves unexpected behaviour when configure SG client UI port and password
- Resolves the error when generating logs
- Add the startup option to define the reconnect attempts
- Add `addtional startup option` field to the auto-start config
- Add proxy support for the gateway authentication request to port 443

## v1.8.2 Fixpack 2
{: #v182fp2}

### Fixes
{: #v182fp2-fixes}

- Resolves SG client UI vulnerability
- Resolves resource auth vulnerability
- Enhance the handling when both side close the connection in the same time
- Resolves collapsed resource when the on-prem destination connection close too fast

## v1.8.2 Fixpack 1
{: #v182fp1}

### Fixes
{: #v182fp1-fixes}

- Upgrade Node.js to 8.15.1
- Resolves undefined environment info in Deutsch
- Resolves the incorrect concurrent connection limit for reverse connections
- Resolves collapsed connections when both side end the connection in the same time

## v1.8.2
{: #v182}

### Features and Fixes
{: #v182-features-and-fixes}

- Upgrade Node.js to 8.15.0
- Node modules vulnerabilities fix

## v1.8.1
{: #v181}

### Features and Fixes
{: #v181-features-and-fixes}

- Resolves regular connections hanging issue when closing
- Uew new path for gateway authentication ([Click here for more info](/docs/services/SecureGateway?topic=securegateway-client-requirements#network-requirements))
- Use new mechanism for log generating (Log translation enhance)

## v1.8.0 Fixpack 9
{: #v180fp9}

### Fixes
{: #v180fp9-fixes}

- Separate the ConnIndicies for reverse and regular destination
- Accessibility enhancements on client UI.
- Resolves unexpected behaviour when set UI password on command line.
- Resolves unexpected crash when UI closed

## v1.8.0 Fixpack 8
{: #v180fp8}

### Fixes
{: #v180fp8-fixes}

- Enhance the logs
- Add the data size checking when transmitted data

## v1.8.0 Fixpack 7
{: #v180fp7}

### Fixes
{: #v180fp7-fixes}

- Added the TRACE level log for transmitted data
- Add Error level log when reverse dest hit concurrent limit
- Resolves the undefined Secure Gateway Client fixpack info in gateway panel

## v1.8.0 Fixpack 6
{: #v180fp6}

### Fixes
{: #v180fp6-fixes}

- Enhance the logs
- Resolves unexpected behaviour in docker version

## v1.8.0 Fixpack 5
{: #v180fp5}

### Fixes
{: #v180fp5-fixes}

- Resolves the corrupted header
- Enhance the error log

## v1.8.0 Fixpack 4
{: #v180fp4}

### Fixes
{: #v180fp4-fixes}

- Resolves ACL configuration issue

## v1.8.0 Fixpack 3
{: #v180fp3}

### Fixes
{: #v180fp3-fixes}

- Resolves the truncated message
- Resolves the hang connection
- Add option to define timeout on destination

## v1.8.0 Fixpack 2
{: #v180fp2}

### Fixes
{: #v180fp2-fixes}

- Resolves issue when the connection closed by the cloud side
- Resolves issue related to the host header

## v1.8.0 Fixpack 1
{: #v180fp1}

### Fixes
{: #v180fp1-fixes}

- Resolves Docker version Secure Gateway Client UI issue
- Enhance error log when error occur on server side
- Resolves connection issue between Secure Gateway Client and Secure Gateway Server

## v1.8.0
{: #v180}

### Features
{: #v180-features}
- Performance improvement
- Add Secure Gateway Client reconnect logic

## v1.7.1
{: #v171}

### Features
{: #v171-features}
- Resolves Secure Gateway Client UI vulnerability
- Resolves issue related to UDP connections
- Resolves issue related to quality test
- Refactor log generater
- Add the startup option to define the proxy for the 9000 ws connection

## v1.7.0 Fixpack 1
{: #v171fp1}

### Fixes
{: #v171fp1-fixes}

- Fixes `EADDRINUSE` error caused by listeners that do not execute a teardown appropriately when a destination is deleted.
- Resolves issue where service instances bound to an application could not be deleted.

## v1.7.0
{: #v170}

### Features
{: #v170-features}

- Introduces new service plans: Essentials, Professional, and Enterprise.
- The Secure Gateway Client now supports SOCKS proxy between itself and the on-prem destination.

## v1.6.0 Fixpack 1
{: #v160fp1}

### Fixes
{: #v160fp1-fixes}

- Resolves issue where disconnecting multiple clients results in orphaned processes in the connected clients array.
- Now cleans up orphaned connections that resulted in incorrecty paused listeners.

## v1.6.0
{: #v160}

### Features
{: #v160-features}

- Access Control List now supports specific path routing for HTTP/S requests.
- Destinations now support Server Name Indicators for TLS connections.

## v1.5.1
{: #v151}

### Features
{: #v151-features}

- Destination wizard added for destination creation.
- Gateways now support uploading a custom cert/key pair.
- Services are now limited to 50 gateways and 50 destinations per gateway.
- Destinations/Gateways/Services can now be exported/imported.
- Latency tests between client and server capable of being executed from Secure Gateway UI.

## v1.5.0 Fixpack 1
{: #v150fp1}

### Fixes
{: #v150fp1-fixes}

- Cleans up orphaned tunnel processes on network disconnect.
- Resolves duplicate port allocation caused by failed destination deletion.
- Resolves HTTP/S issue with non-UTF8 encoding.
- Resolves missing IPC handlers on replacement processes.

## v1.5.0
{: #v150}

### Features
{: #v150-features}

- Client now has a local interactive UI.
- UDP connections are now supported.
- Memory footprint of the client has been drastically reduced.
- Single client mode is no longer supported; multi-client will be default and transparent to a single-gateway client.
- Access Control List synchronizes between clients connected to the same gateway.

## v1.4.2
{: #v142}

### Features
{: #v142-features}

- Clients are now assigned an ID when connecting to the SG server.
- Clients are now terminated remotely via API or the Secure Gateway UI.
- The connected status of a client can be checked via API.
- Destination-side TLS now supports Mutual Authentication.
- Cloud destinations now support Mutual Authentication protocols.
