---

copyright:
  years: 2015, 2024
lastupdated: "2024-01-29"

subcollection: SecureGateway

---
{:deprecated: .deprecated}
{:external: target="_blank" .external}

# Requirements to run the Client
{: #client-requirements}

{{site.data.keyword.SecureGateway}} is deprecated. For more information, see the [deprecation details](/docs/SecureGateway?topic=SecureGateway-dep-overview).
{: deprecated}

## System Requirements
{: #system-requirements}

Secure Gateway does not support a platform version if a vendor has expired support
for it. In other words, Secure Gateway does not support running on End-of-Life (EoL)
platforms. This is true regardless of entries in the table below.


| Operating System | Architectures    | Versions                          |  Notes                                |
| ---------------- | ---------------- | --------------------------------- |  ------------------------------------ |
| GNU/Linux        | x64              | kernel >= 4.18, glibc >= 2.28     | e.g. Ubuntu 20.04, Debian 10, RHEL 8 |
| GNU/Linux        | ppc64le >=power8 | kernel >= 4.18, glibc >= 2.28     | e.g. Ubuntu 20.04, RHEL 8            |
| GNU/Linux        | s390x            | kernel >= 4.18, glibc >= 2.28     | e.g. RHEL 8                          |
| Windows          | x64              | version >= Windows 10/Server 2016 |                                      |
| macOS            | x64              | version >= 11.0                   |                                      |
| AIX              | ppc64be >=power8 | version >= 7.2 TL04               |                                      |
| Docker           |                  | version >= 1.7.0                  | all supported operating systems      |

<b>Note:</b> Only 64-bit environments are currently supported for native client installation.

## Network Requirements
{: #network-requirements}

The Secure Gateway Client uses outbound port 443 and port 9000 to connect to npm registry and the {{site.data.keyword.Bluemix}} environment:

- Port `443` for npm installation
  - During the installation, the installer will connect to npm registry and run `npm install` to install the dependencies required by Secure Gateway Client. Before the installation, please make sure the machine which the client will be installed on can connect to a npm registry website. npm is configured to use npm, Inc.'s public registry at `https://registry.npmjs.org` by default. <br><br>
If there's npm Enterprise server in your environment, please whitelist all of the dependencies of Secure Gateway Client on the npm Enterprise server. For the list of dependencies, please refer to `<Installation_directory>\ibm\securegateway\client\package.json` file.

- Port `9000` for WSS connection between Secure Gateway Client and Secure Gateway Server
  - The node of the SG gateway, which can be found in the configuration of the gateway. Since each gateway will not be on the same node, please confirm the hostname of the node every time you create the gateway

- Port `443` for gateway authentication when Secure Gateway Client start up
  - US South: sgmanager.us-south.securegateway.cloud.ibm.com
  - US East: sgmanager.us-east.securegateway.cloud.ibm.com
  - United Kingdom: sgmanager.eu-gb.securegateway.cloud.ibm.com
  - Germany: sgmanager.eu-de.securegateway.cloud.ibm.com
  - Sydney: sgmanager.au-syd.securegateway.cloud.ibm.com


Ensure you check or modify additional firewall and IP Table rules that might apply. However, we do not recommend setting rules by IP, rules should be set specific to host name and port as the IPs for gateway authentication and SG gateway are controlled by IBM Cloud and are subject to change.


## Determining Hardware Requirements
{: #hardware-requirements}

The specifications of the machine running the Secure Gateway Client is largely dependent on the traffic that will be passing through each connection.  Each client instance is an individual process that provides up to 250 concurrent connections.  To estimate an average maximum that the machine would need to support, determine the average size of a transaction across the client (both request and response) and then scale that to 250 concurrent transactions.  Given that this number has combined both request and response sizes, the client should not exceed this memory footprint.  For more precise estimates, a mixture of request sizes and response sizes should be used to better simulate a real-world transaction scenario.

For running a single instance of the Secure Gateway Client, we suggest a minimum of 2 cores and 4 GB of memory.

For HA scenarios that will be running multiple instances of the client on the same machine, we suggest a minimum of 4 cores and 8 GB of memory.
