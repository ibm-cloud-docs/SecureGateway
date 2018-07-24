---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---

# Requirements to run the Client

## System Requirements
{: #system}

The Secure Gateway Client is supported in the following environments:

| Name | Versions          |
| ------------- | ----------- |
| Windows Desktop | 7, 8.1, 10 and greater |
| Windows Server | 2012 R2 and greater |
| Red Hat Linux | 6.5 and greater |
| CentOS | 7.2 and greater |
| SuSE Linux | 11.0<sup>*</sup> and greater |
| Ubuntu Linux | 14.04 (LTS) and greater |
| Power Machine | ppc64el architecture |
| Ubuntu Z-Linux | - |
| AIX | 7.1 and greater |
| Mac OS X | 10.10 (Yosemite) and greater |
| Docker | 1.7.0 and greater, all supported operating systems |

<sup> * </sup>- SLES 11 support is only with Service Pack 4

<b>Note:</b> Only 64-bit environments are currently supported for native client installation.

## Network Requirements
{: #network}

The Secure Gateway Client uses outbound port 443 and port 9000 to connect to the {{site.data.keyword.Bluemix}} environment:

<table>
    <thead>
        <tr>
            <th>Rigon</th>
            <th>Port</th>
            <th>Host</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan=6>US South</td>
            <td rowspan=5>9000</td>
            <td>cap-sg-prd-1.integration.ibmcloud.com</td>
        </tr>
        <tr>
            <td>cap-sg-prd-2.integration.ibmcloud.com</td>
        </tr>
        <tr>
            <td>cap-sg-prd-3.integration.ibmcloud.com</td>
        </tr>
        <tr>
            <td>cap-sg-prd-4.integration.ibmcloud.com</td>
        </tr>
        <tr>
            <td>cap-sg-prd-5.integration.ibmcloud.com</td>
        </tr>
        <tr>
            <td>443</td>
            <td>sgmanager.ng.bluemix.net</td>
        </tr>
        <tr>
            <td rowspan=6>US East</td>
            <td rowspan=5>9000</td>
            <td>cap-us-east-prd-sg-bm-01.integration.ibmcloud.com</td>
        </tr>
        <tr>
            <td>cap-us-east-prd-sg-bm-02.integration.ibmcloud.com</td>
        </tr>
        <tr>
            <td>cap-us-east-prd-sg-bm-03.integration.ibmcloud.com</td>
        </tr>
        <tr>
            <td>cap-us-east-prd-sg-bm-04.integration.ibmcloud.com</td>
        </tr>
        <tr>
            <td>cap-us-east-prd-sg-bm-05.integration.ibmcloud.com</td>
        </tr>
        <tr>
            <td>443</td>
            <td>sgmanager.us-east.bluemix.net</td>
        </tr>
        <tr>
            <td rowspan=6>United Kingdom</td>
            <td rowspan=5>9000</td>
            <td>caplonsgprd-1.integration.ibmcloud.com</td>
        </tr>
        <tr>
            <td>caplonsgprd-2.integration.ibmcloud.com</td>
        </tr>
        <tr>
            <td>caplonsgprd-3.integration.ibmcloud.com</td>
        </tr>
        <tr>
            <td>caplonsgprd-4.integration.ibmcloud.com</td>
        </tr>
        <tr>
            <td>caplonsgprd-5.integration.ibmcloud.com</td>
        </tr>
        <tr>
            <td>443</td>
            <td>sgmanager.eu-gb.bluemix.net</td>
        </tr>
        <tr>
            <td rowspan=6>Germany</td>
            <td rowspan=5>9000</td>
            <td>cap-eu-de-prd-sg-bm-01.integration.ibmcloud.com</td>
        </tr>
        <tr>
            <td>cap-eu-de-prd-sg-bm-02.integration.ibmcloud.com</td>
        </tr>
        <tr>
            <td>cap-eu-de-prd-sg-bm-03.integration.ibmcloud.com</td>
        </tr>
        <tr>
            <td>cap-eu-de-prd-sg-bm-04.integration.ibmcloud.com</td>
        </tr>
        <tr>
            <td>cap-eu-de-prd-sg-bm-05.integration.ibmcloud.com</td>
        </tr>
        <tr>
            <td>443</td>
            <td>sgmanager.eu-de.bluemix.net</td>
        </tr>
        <tr>
            <td rowspan=6>Sydney</td>
            <td rowspan=5>9000</td>
            <td>cap-eu-de-prd-sg-bm-01.integration.ibmcloud.com</td>
        </tr>
        <tr>
            <td>cap-eu-de-prd-sg-bm-02.integration.ibmcloud.com</td>
        </tr>
        <tr>
            <td>cap-eu-de-prd-sg-bm-03.integration.ibmcloud.com</td>
        </tr>
        <tr>
            <td>cap-eu-de-prd-sg-bm-04.integration.ibmcloud.com</td>
        </tr>
        <tr>
            <td>cap-eu-de-prd-sg-bm-05.integration.ibmcloud.com</td>
        </tr>
        <tr>
            <td>443</td>
            <td>sgmanager.au-syd.bluemix.net</td>
        </tr>
    </tbody>
</table>

Ensure you check or modify additional firewall and IP Table rules that might apply. If your network administrators require specific IPs, please [contact support to request these for your environment](./securegateway_troubleshooting.html#support).


## Determining Hardware Requirements
{: #hardware}

The specifications of the machine running the Secure Gateway Client is largely dependent on the traffic that will be passing through each connection.  Each client instance is an individual process that provides up to 250 concurrent connections.  To approximate an average maximum that the machine would need to support, determine the average size of a transaction across the client (both request and response) and then scale that to 250 concurrent transactions.  Given that this number has combined both request and response sizes, the client should not exceed this memory footprint.  For more precise estimates, a mixture of request sizes and response sizes should be used to better simulate a real-world transaction scenario.

For running a single instance of the Secure Gateway Client, we suggest a minimum of 2 cores and 4 GB of memory.

For HA scenarios that will be running multiple instances of the client on the same machine, we suggest a minimum of 4 cores and 8 GB of memory.
