---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---

# {{site.data.keyword.SecureGateway}} Change Log
{: #secure-gateway-change-log}

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
