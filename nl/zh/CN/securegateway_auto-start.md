---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 为客户机配置自动启动
{: #auto-start-conf}

## Linux
{: #auto-start-linux}

如果已选择使用系统的自动启动工具，请使用下列其中一种方法来启动客户机。客户机将使用的配置文件可在以下位置找到：

```
/etc/ibm/sgenvironment.conf
```
{: screen}

<b>注：</b>Red Hat Linux 6 无法使用自动启动功能。

此文件包含要设置的以下重要变量：

|环境变量|描述|
| ------------- | ----------- |
|RESTART_CLIENT|安装或升级期间停止并重新启动客户机（Yes 或 No）|
|GATEWAY_ID|在 Bluemix Secure Gateway UI 上创建的网关标识|
|SECTOKEN|此网关标识的安全性令牌（如果在创建网关时选择了强制实施安全性）|
|ACL_FILE|要用于限制对资源的内部部署访问的访问控制表文件|
|LOGLEVEL|要为服务设置的日志级别（缺省值为 INFO）|
|USE_UI|如果不想启动客户机 UI，请将此项设置为“N”|
|UI_PORT|要在其上启动客户机 UI 的端口（缺省值为 9003）|
|LANGUAGE|客户机登录时要使用的语言（缺省值为 en）|

<b>注：</b>仅当使用系统的自动启动工具时，才会读取此文件。如果是手动运行客户机的，那么将忽略此文件。

### Upstart
{: #auto-start-upstart}

### 启动客户机
{: #upstart-start}

如果是使用 upstart 功能使 Secure Gateway 客户机在系统启动时自动运行的，那么必须首先检查其配置 (/etc/ibm/sgenvironment.conf)。配置 upstart 服务后，可以使用以下命令来启动该服务：

```
sudo initctl start securegateway_client
```
{: pre}

要重新启动该服务，请运行以下命令：

```
sudo initctl restart securegateway_client
```
{: pre}

### 停止客户机
{: #upstart-stop}

要停止该服务，请运行以下命令：

```
sudo initctl stop securegateway_client
```
{: pre}

### SystemD
{: #auto-start-systemd}


### 启动客户机
{: #systemd-start}

如果是使用 systemD 功能使 Secure Gateway 客户机在系统启动时自动运行的，那么必须首先检查其配置 (/etc/ibm/sgenvironment.conf)。配置 upstart 服务后，可以使用以下命令来启动该服务：

```
systemctl start securegateway_client
```
{: pre}

要重新启动该服务，请运行以下命令：

```
systemctl restart securegateway_client
```
{: pre}

要获取有关该服务的状态，请运行以下命令：

```
systemctl status securegateway_client
```
{: pre}

### 停止客户机
{: #systemd-stop}

要停止该服务，请运行以下命令：

```
systemctl stop securegateway_client
```
{: pre}

### 系统 V
{: #auto-start-system-v}

在安装期间不会设置 System V，这与其他自动启动工具不同。安装目录 /opt/ibm/securegateway/client/upstart 中提供了脚本，这些脚本包括：

```
secgw.sh
securegateway_clientd
```
{: screen}

<b>注：</b>此部分将影响 SuSE/SLES 11 的用户。

可选：如果运行的是使用系统 V 自动启动的 SuSE V11，那么提供了可用于配置此过程的脚本。请遵循以下过程：

```
cd /opt/ibm/securegateway/client/upstart/systemV
cp secgw.sh /usr/local/bin
chmod +x /usr/local/bin/secgw.sh
cp securegateway_clientd /usr/local/bin
chmod +x /usr/local/bin/securegateway_clientd
cd /etc/init.d
ln -s /usr/local/bin/securegateway_clientd
insserv securegateway_clientd
vi /etc/ibm/sgenvironment.conf
```
{: codeblock}

执行了这些步骤后，就可以使用 YasT 和系统 V 命令来启动/停止守护程序。

返回到[入门 - 添加客户机](/docs/services/SecureGateway?topic=securegateway-add-client)。

## Windows
{: #auto-start-windows}

要变更 Windows 服务的状态，请以管理员特权打开命令窗口。

```
cd "<Installation_directory>\ibm\securegateway\client"
```
{: pre}

如果服务已在运行，用户可以使用以下命令将其停止。

```
windowsService.cmd uninstall
```
{: pre}

要启动服务，请使用以下命令：

```
windowsService.cmd install
```
{: pre}

<b>注：</b>该服务将基于存储在以下位置的配置来运行：

```
%Installation_directory%/ibm/securegateway/client/securegw_service.config
```
{: pre}

Windows 服务的应用程序日志将存储在以下位置：

```
%Installation_directory%/ibm/securegateway/client/logs/securegw_win_service.log
```
{:pre}

 这些日志每日滚动为新文件。

返回到[入门 - 添加客户机](/docs/services/SecureGateway?topic=securegateway-add-client)。
