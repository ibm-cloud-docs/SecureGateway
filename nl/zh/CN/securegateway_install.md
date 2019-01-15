---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-11"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 安装客户机

## Docker
{: #docker}

Docker 是一个第三方平台，可提供一种快速轻松安装应用程序的容器方法，该方法只需要少量配置或者不需要任何配置。{{site.data.keyword.SecureGateway}} 服务提供了可在工作站上安装 Docker 实用程序之后使用的 Docker 映像。要安装 Docker，请参阅 Docker 安装 Web 站点并遵循适合您系统的指示信息进行操作。

### 安装和更新客户机
{: #docker-install}

安装和更新 {{site.data.keyword.SecureGateway}} 客户机 Docker 映像使用的是相同的命令：

```
docker pull ibmcom/secure-gateway-client
```
{: pre}

### 启动交互式客户机会话
{: #docker-run}

要运行 IBM {{site.data.keyword.SecureGateway}} 的 Docker 容器，请发出以下命令：

```
docker run -it ibmcom/secure-gateway-client <gateway ID> -t <security token>
```
{: pre}

通常，该命令会采用两个参数，即 {{site.data.keyword.SecureGateway}} 网关标识和网关的安全性令牌，这两个参数都可通过 {{site.data.keyword.SecureGateway}} 仪表板提供。

### 支持的 Docker 命令
{: #docker-commands}

{{site.data.keyword.SecureGateway}} 客户机仅支持将 `pull` 和 `run` 命令用于操作容器。

返回到[入门 - 添加客户机](./securegateway_client.html)。

## Mac OS X
{: #mac}

### 在 Mac OS X 上运行的需求
{: #mac-requirements}

在 Mac OS X 上运行客户机之前，需要满足以下先决条件。
 1. 安装 Xcode 开发工具，此平台上的某些节点模块需要这些工具。

### Mac OS X 安装过程
{: #mac-install}

根据系统的安全设置，您可能需要管理特权才能执行此安装。

 1. 通过“双击”在 {{site.data.keyword.SecureGateway}} UI 中下载的 DMG 映像来安装该映像。
 2. 这应该会显示新的“访达”窗口。此窗口应该包含应用程序“快捷方式”图标，请将应用程序拖放到快捷方式上。如果未包含该图标，请“双击”已安装的卷，然后将应用程序图标拖放到“访达”侧边栏中的“应用程序”图标。

### 启动交互式客户机会话
{: #mac-run}

要启动客户机，请运行位于缺省安装位置 `/Applications/ibm/` 中的 `secgw.command` 文件。

返回到[入门 - 添加客户机](./securegateway_client.html)。

## Linux
{: #linux}

此安装包含 {{site.data.keyword.SecureGatewayfull}} 客户机以及 IBM 的 nodejs 包的安全版本。这两项都会安装在系统上的 /opt/ibm 目录下。安装程序会创建或更新以下内容：

|目录|文件名|描述|
| ------------- | ------------- | ----------- |
|/etc/ibm|sgenvironment.conf|upstart 的配置文件|
|/etc/ibm|passwd|创建用户：secgwadmin:x:501:501::/home/secgwadmin:/bin/bash|
|/etc/ibm|group|创建组：secgwadmin:x:501:|
|/etc/init|securegateway_client.conf|upstart 文件|
|/opt/ibm|node-v&lt;version&gt;-linux-x64|IBM 的 Node JS 安装|
|/opt/ibm|securegateway|{{site.data.keyword.SecureGateway}} 客户机安装|
|/usr/local/bin|node|为 IBM 的节点可执行文件创建的符号链接|
|/usr/local/bin|npm|为 IBM 的 npm 可执行文件创建的符号链接|
|/var/log/securegateway|client_console.log|将客户机作为自动启动进程运行时创建的日志文件|

您需要有 root 用户特权或管理特权才能执行此安装。

### Ubuntu/PowerPC 安装
{: #debian-install}

 1. 安装 {{site.data.keyword.SecureGateway}} 客户机。例如，如果使用的是 Debian 包，请发出以下命令：

    ```
    sudo dpkg -i <secure-gateway-debian-package>
    ```
    {: pre}

 2. 客户机安装程序启动后，系统将提示您提供以下信息。

    <b>自动启动过程 Yes/No</b>
        如果这是升级或重新安装，那么必须选择是否要在安装进程运行时停止现有客户机。
        选择 Y/y 以停止现有客户机。否则，客户机包将升级，而不重新启动客户机，这
        意味着可以等待相应的“软件更新”窗口来执行重新启动。如果选择 N/n，安装将
        继续，并且必须手动重新启动客户机。

    <b>网关标识</b>
        设置客户机的网关标识。网关标识是在创建 {{site.data.keyword.SecureGateway}} 服务时定义的。
        如果客户机连接失败，可以通过编辑 .conf 文件来更改选择。

    <b>安全性令牌</b>
        如果支持网关标识检查安全性令牌，那么必须立即提供安全性令牌。如果网关标识无需安全性令牌，请将此项留空。

    <b>日志记录级别</b>
        缺省设置为 INFO。其他有效值为 TRACE、DEBUG 和 ERROR。

    <b>访问控制表</b>
        输入包含访问控制表的文件的文件名的绝对路径，该访问控制表列出有权访问内部部署资源的用户。
        有关更多信息，请参阅 /opt/ibm/securegateway 中的 README.md 文件或参阅“访问控制表”。

    <b>客户机 UI</b>
        选择是否要使用客户机 UI。如果要使用，那么用户可以更改用于 UI 的端口。缺省值为 9003。

    <b>注：</b>您不必回答任何提示，所有提示都将采用定义的缺省值，或者在 sgenvironment.conf 文件中保留为空白。这将允许安装进程在没有用户交互的情况下运行。

    <b>注：</b>每次使用系统的 upstart 进程来启动或重新启动客户机时，都会读取 sgenvironment.conf。可以随时编辑 /etc/ibm/sgenvironment.conf 文件以更改配置，然后重新启动客户机以使这些更改生效。

    <b>注：</b>通过更改 /etc/ibm/sgenvironment.conf 文件中的 `LANGUAGE` 参数，可更改 {{site.data.keyword.SecureGateway}} 客户机服务日志的语言。服务日志将在服务重新启动之后更改为所选语言。

 3. 如果重新启动客户机，为了确保客户机在正常运行，请发出以下命令：

    ```
    cat /var/log/securegateway/client_console.log
    ```
    {: pre}

 4. 如果要编辑配置文件，为了确保配置文件已正确更新，请发出以下命令：

    ```
    sudo vi /etc/ibm/sgenvironment.conf
    ```
    {: pre}

 5. 要检查客户机安装的状态，请发出以下命令：

    ```
    sudo dpkg --status ibm-securegateway-client
    ```
    {: pre}

将返回以下输出示例：

```
Package: ibm-securegateway-client
Status: install ok installed
Priority: extra
Section: IBM/net
Maintainer: cloudoe-dev@webconf.ibm.com
Architecture: amd64
Source: ibm-securegateway+securegateway-client
Version: 1.7.0
Description: IBM Secure Gateway Client for Bluemix
IBM Secure Gateway client package for Bluemix, supports secure connections to on-premises connections.
Homepage: https://ng.bluemix.net/
License: http://www.ibm.com/software/sla/sladb.nsf/lilookup/986C7686F22D4D3585257E13004EA6CB?OpenDocument
```
{: screen}

### RedHat/SuSE 安装
{: #rpm-install}

1. 安装 {{site.data.keyword.SecureGateway}} 客户机。例如，如果使用的是 RPM 包，请发出以下命令：

   ```
   rpm -ivhf <secure-gateway-rpm-package>
   ```
   {: pre}


   客户机安装程序会启动并安装客户机，这将在 /etc/ibm 中创建 sgenvironment.conf 文件。

2. 可选：如果要使用系统的 upstart 进程，您必须编辑此文件并提供以下内容，客户机才能正常启动。请参阅[使用 Upstart](./securegateway_auto-start.html#linux)，以获取有关编辑此配置文件的更多信息。

3. 如果使用 upstart 启动了客户机，请检查日志文件以确保它在正常运行。

   ```
   cat /var/log/securegateway/client_console.log
   ```
   {: pre}

4. 要检查客户机安装的状态，请发出以下命令：

   ```
   rpm -q ibm-securegateway-client
   ```
   {: pre}

### 启动交互式客户机会话
{: #linux-run}

要启动客户机，请运行以下命令：

```
cd /opt/ibm/securegateway/client
node lib/secgwclient.js <gateway ID> -t <security token>
```
{: codeblock}

通常，该命令会采用两个参数，即 {{site.data.keyword.SecureGateway}} 网关标识和网关的安全性令牌，这两个参数都可通过 {{site.data.keyword.SecureGateway}} 仪表板提供。

返回到[入门 - 添加客户机](./securegateway_client.html)。

## Windows
{: #windows}

### 安装客户机
{: #windows-install}

根据系统的安全设置，您可能需要管理特权才能执行此安装。

 1. 将 EXE 安装程序文件复制到您的系统。此外，还提供了 MD5 sum，您可以利用它来检查安装包的完整性。

 2. 通过双击该文件并回答提示进行安装，或者从命令提示符运行该可执行文件来进行安装。

 3. 系统将提示用户选择其所选的安装目录。缺省位置是 Program Files (x86) 目录。

 4. 系统将提示用户选择要用于启动命令行界面的语言。如果未选择语言，那么将缺省为英语。

 5. 系统将提示用户将客户机安装为 Windows 服务。如果用户选择这样做，那么客户机会使用用户在后续对话框中提供的配置，在后台作为 Windows 服务运行。在“控制面板”的服务页面中可检查该服务的状态。该服务的名称为“IBM Bluemix Secure Gateway Service”。

 6. 安装程序会确定机器上是否存在现有安装。如果检测到现有安装，那么会询问用户是否要使用现有配置，如果用户不想使用现有配置，那么可以选择输入新配置。可在此处提供每个网关的网关标识、安全性令牌、ACL 文件和日志级别等详细信息。如果用户选择将客户机作为 Windows 服务运行，那么客户机启动时会使用所提供的配置。如果用户未选择将客户机作为 Windows 服务启动，那么将存储这些配置，以供未来使用。不管哪种情况，这些配置都会存储在 %Installation_directory%/ibm/securegateway/client/securegw_service.config 中。

 7. 从 V1.5.0 开始，客户机随附完整功能的客户机 UI。您可以通过安装程序本身对其进行配置。用户可以提供密码和端口号（缺省值为 9003）以用于启动客户机 UI。

 如果用户选择通过与多个网关的连接来启动客户机，请谨慎提供配置值。网关标识需要以空格分隔。安全性令牌、ACL 文件和日志级别应该以 -- 定界。如果不想为特定网关提供这三个值中的任何一个值，请使用“none”。

 <b>注：</b>请确保没有残余的空格。

 <b>注：</b>请注意，ACL 文件应该放入 %Installation_directory%/ibm/securegateway/client 目录中，或者放入该路径的相对路径中。

### 启动交互式客户机会话
{: #windows-run}

要启动客户机，请使用管理特权打开命令提示符，然后运行以下命令：

```
cd "<Installation_directory>\ibm\securegateway\client"
secgw.cmd
```
{: codeblock}

或者，浏览至 `<Installation_directory>\ibm\securegateway\client`，然后双击 `secgw.cmd`。

<b>注：</b>您可以选择使用存储在 `<Installation_directory>\ibm\securegateway\client\securegw_service.config` 文件中的配置，或以交互方式提供详细信息。

返回到[入门 - 添加客户机](./securegateway_client.html)。

## DataPower
{: #datapower}

DataPower 具有嵌入式版本的 {{site.data.keyword.SecureGateway}} 客户机。根据 DataPower 版本，您可能具有其他版本的 {{site.data.keyword.SecureGateway}} 客户机。请注意任何适用的 [DataPower 客户机限制](./securegateway_interaction.html#limits-datapower)。使用旧 Secure Gateway 客户机可能会遇到意外错误。

|DataPower 版本|{{site.data.keyword.SecureGateway}} 客户机版本|
| -- | --  |
|7.2.0.0、7.5.0.0|1.1.0|
|7.5.1.0、7.7.0|1.4.2|
|7.5.2.4|1.6.1|
|7.5.2.6、7.6.0.0|1.7.0|
|7.5.2.14、7.6.0.7、7.7.1.0|1.8.0fp6|

### 启动客户机会话
{: #datapower-run}

1. 登录到 DataPower Web GUI
2. 浏览至“对象”->“云”->“Secure Gateway 客户机”，或搜索 Secure Gateway 客户机
3. 单击`添加`以配置新的客户机连接
4. 提供名称、网关标识和安全性令牌（如果适用），然后应用更改。

返回到[入门 - 添加客户机](./securegateway_client.html)。
