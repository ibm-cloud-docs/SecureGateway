---

copyright:
  years: 2015, 2021
lastupdated: "2022-11-30"

subcollection: SecureGateway

---
{:deprecated: .deprecated}
{:external: target="_blank" .external}

# Requirements to run the Client
{: #client-requirements}

## System Requirements
{: #system-requirements}

The Secure Gateway Client is supported in the following environments:

| Name | Versions          |
| ------------- | ----------- |
| Windows Desktop | 7, 8.1, 10 and greater |
| Windows Server | 2012 R2 and greater |
| Red Hat Linux | 7.3 and greater |
| CentOS | 7.3 and greater |
| SuSE Linux | 12 and greater |
| Ubuntu Linux | 14.04 (LTS) and greater |
| Power Machine | ppc64el architecture |
| Ubuntu Z-Linux | - |
| AIX | 7.1 TL5 and greater |
| Mac OS X | 10.15 and greater |
| Docker | 1.7.0 and greater, all supported operating systems |

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
