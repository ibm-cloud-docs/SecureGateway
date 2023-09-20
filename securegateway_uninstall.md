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

# Uninstalling the Client
{: #client-uninstall}

{{_include-segments/list-note.md}}

## Docker
{: #docker-uninstall}

To remove the {{site.data.keyword.SecureGateway}} Client Docker image, run the following command:

```
docker rmi ibmcom/secure-gateway-client
```
{: pre}

## Mac OS X
{: #mac-uninstall}

To remove the {{site.data.keyword.SecureGateway}} Client from Mac OS X, run the following commands from the terminal:

```
cd /Applications
rm -rf ./ibm
```
{: codeblock}

## Linux
{: #linux-uninstall}

### Ubuntu/PowerPC
{: #debian-uninstall}

To find the install status of the {{site.data.keyword.SecureGateway}} client, you can use the following command:

```
sudo dpkg -l | grep ibm-securegateway-client
```
{: pre}

To uninstall the {{site.data.keyword.SecureGateway}} client without removing the installation dir, configuration files, id, group id,
and log files, then use the following:

```
sudo dpkg -r ibm-securegateway-client
```
{: pre}

To uninstall the {{site.data.keyword.SecureGateway}} client removing everything use the following:

```
sudo dpkg --purge ibm-securegateway-client
```
{: pre}

### RedHat/SuSE
{: #rpm-uninstall}

To find the install status of the {{site.data.keyword.SecureGateway}} client, you can use the following command:

```
rpm -qa | grep ibm-securegateway-client
```
{: pre}

To uninstall the {{site.data.keyword.SecureGateway}} client without removing the installation dir, configuration files, id, group id,
and log files, then use the following:

```
rpm -e ibm-securegateway-client
```
{: pre}

To uninstall the {{site.data.keyword.SecureGateway}} client removing everything use the following:

```
yum remove ibm-securegateway-client
```
{: pre}

## Windows
{: #windows-uninstall}

There are two ways to remove Windows installation for {{site.data.keyword.SecureGateway}} client:

1. Removing from the installation directory: Go to the installation directory for {{site.data.keyword.SecureGateway}} client and double-click on the uninstall.exe.

2. Removing from Control Panel: Remove the {{site.data.keyword.SecureGateway}} application from the Programs section of the Control Panel.
