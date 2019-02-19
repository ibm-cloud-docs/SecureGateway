---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 卸载客户机
{: #client-uninstall}

## Docker
{: #docker}

要除去 {{site.data.keyword.SecureGateway}} 客户机 Docker 映像，请运行以下命令：

```
docker rmi ibmcom/secure-gateway-client
```
{: pre}

## Mac OS X
{: #mac}

要从 Mac OS X 中除去 {{site.data.keyword.SecureGateway}} 客户机，请在终端中运行以下命令：

```
cd /Applications
rm -rf ./ibm
```
{: codeblock}

## Linux
{: #linux}

### Ubuntu/PowerPC
{: #debian-uninstall}

要查找 {{site.data.keyword.SecureGateway}} 客户机的安装状态，可以使用以下命令：

```
sudo dpkg -l | grep ibm-securegateway-client
```
{: pre}

要卸载 {{site.data.keyword.SecureGateway}} 客户机而不除去安装目录、配置文件、标识、组标识和日志文件，请使用以下命令：

```
sudo dpkg -r ibm-securegateway-client
```
{: pre}

要卸载 {{site.data.keyword.SecureGateway}} 客户机并除去所有内容，请使用以下命令：

```
sudo dpkg --purge ibm-securegateway-client
```
{: pre}

### RedHat/SuSE
{: #rpm-uninstall}

要查找 {{site.data.keyword.SecureGateway}} 客户机的安装状态，可以使用以下命令：

```
rpm -qa | grep ibm-securegateway-client
```
{: pre}

要卸载 {{site.data.keyword.SecureGateway}} 客户机而不除去安装目录、配置文件、标识、组标识和日志文件，请使用以下命令：

```
rpm -e ibm-securegateway-client
```
{: pre}

要卸载 {{site.data.keyword.SecureGateway}} 客户机并除去所有内容，请使用以下命令：

```
yum remove ibm-securegateway-client
```
{: pre}

## Windows
{: #windows}

有两种方法可以除去 Windows 上安装的 {{site.data.keyword.SecureGateway}} 客户机：

1. 通过安装目录除去：转至 {{site.data.keyword.SecureGateway}} 客户机的安装目录，然后双击 uninstall.exe。

2. 通过“控制面板”除去：在“控制面板”的“程序”部分中，除去 {{site.data.keyword.SecureGateway}} 应用程序。
