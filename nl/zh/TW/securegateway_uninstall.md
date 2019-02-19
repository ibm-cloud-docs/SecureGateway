---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 解除安裝用戶端
{: #client-uninstall}

## Docker
{: #docker}

若要移除 {{site.data.keyword.SecureGateway}} Client Docker 映像檔，請執行下列指令：

```
docker rmi ibmcom/secure-gateway-client
```
{: pre}

## Mac OS X
{: #mac}

若要從 Mac OS X 移除「{{site.data.keyword.SecureGateway}} 用戶端」，請從終端機執行下列指令：

```
cd /Applications
rm -rf ./ibm
```
{: codeblock}

## Linux
{: #linux}

### Ubuntu/PowerPC
{: #debian-uninstall}

若要尋找 {{site.data.keyword.SecureGateway}} 用戶端的安裝狀態，您可以使用下列指令：

```
sudo dpkg -l | grep ibm-securegateway-client
```
{: pre}

若要解除安裝 {{site.data.keyword.SecureGateway}} 用戶端，而不移除安裝目錄、配置檔、ID、群組 ID 及日誌檔，則請使用下列指令：

```
sudo dpkg -r ibm-securegateway-client
```
{: pre}

若要解除安裝 {{site.data.keyword.SecureGateway}} 用戶端，並移除所有項目，請使用下列指令：

```
sudo dpkg --purge ibm-securegateway-client
```
{: pre}

### RedHat/SuSE
{: #rpm-uninstall}

若要尋找 {{site.data.keyword.SecureGateway}} 用戶端的安裝狀態，您可以使用下列指令：

```
rpm -qa | grep ibm-securegateway-client
```
{: pre}

若要解除安裝 {{site.data.keyword.SecureGateway}} 用戶端，而不移除安裝目錄、配置檔、ID、群組 ID 及日誌檔，則請使用下列指令：

```
rpm -e ibm-securegateway-client
```
{: pre}

若要解除安裝 {{site.data.keyword.SecureGateway}} 用戶端，並移除所有項目，請使用下列指令：

```
yum remove ibm-securegateway-client
```
{: pre}

## Windows
{: #windows}

有兩種方法可以移除 {{site.data.keyword.SecureGateway}} 用戶端的 Windows 安裝：

1. 從安裝目錄移除：移至 {{site.data.keyword.SecureGateway}} 用戶端的安裝目錄，然後按兩下 uninstall.exe。

2. 從控制台移除：從「控制台」的「程式」區段移除 {{site.data.keyword.SecureGateway}} 應用程式。
