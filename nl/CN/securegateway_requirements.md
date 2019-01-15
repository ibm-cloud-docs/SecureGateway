---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---

# 运行客户机的需求

## 系统需求
{: #system}

以下环境中支持 Secure Gateway 客户机：

|名称|版本|
| ------------- | ----------- |
|Windows Desktop|7、8.1、10 及更高版本|
|Windows Server|2012 R2 及更高版本|
|Red Hat Linux|6.5 及更高版本|
|CentOS|7.2 及更高版本|
|SuSE Linux|11.0<sup>*</sup> 及更高版本|
|Ubuntu Linux| 14.04 (LTS) 及更高版本|
|Power 机器|ppc64el 体系结构|
|Ubuntu Z-Linux| - |
|AIX|7.1 及更高版本|
|Mac OS X|10.10 (Yosemite) 及更高版本|
|Docker|1.7.0 及更高版本，所有支持的操作系统|

<sup> * </sup>- 仅 Service Pack 4 支持 SLES 11

<b>注：</b>对于本机客户机安装，当前仅支持 64 位环境。

## 网络需求
{: #network}

Secure Gateway 客户机使用出站端口 443 和端口 9000 来连接到 npm 注册表和 {{site.data.keyword.Bluemix}} 环境：
- 端口 443，用于 npm 安装
  - 在安装期间，安装程序将连接到 npm 注册表，并运行 `npm install` 来安装 Secure Gateway 客户机所需的依赖项。在安装之前，请确保安装客户机的机器可以连接到 npm 注册表 Web 站点。缺省情况下，npm 配置为使用 npm, Inc. 的公共注册表：https://registry.npmjs.org。<br><br>
如果环境中有 npm Enterprise 服务器，请在 npm Enterprise 服务器上将 Secure Gateway 客户机的所有依赖项列入白名单。有关依赖项的列表，请参阅 `<Installation_directory>\ibm\securegateway\client\package.json` 文件。<br><br>

- 端口 443，用于网关认证


  |区域|主机|
  | --  | --  |
  |美国南部|sgmanager.ng.bluemix.net|
  |美国东部|sgmanager.us-east.bluemix.net|
  |英国|sgmanager.eu-gb.bluemix.net|
  |德国|sgmanager.eu-de.bluemix.net|
  |悉尼|sgmanager.au-syd.bluemix.net|


- 端口 9000

  SG 网关的节点可在网关配置中找到，每个网关不会位于同一节点上，请在每次创建网关时确认节点的主机名。


确保检查或修改可能适用的其他防火墙和 IP 表规则。如果网络管理员需要特定 IP，请[联系支持人员以请求用于您环境的 IP](./securegateway_troubleshooting.html#support)。


## 确定硬件需求
{: #hardware}

运行 Secure Gateway 客户机的机器的规范在很大程度上取决于将通过每个连接的流量。每个客户机实例都是一个单独的进程，最多可提供 250 个并行连接。要估算机器需要支持的平均最大数量，请确定客户机上事务的平均大小（包括请求和响应），然后将其扩展到 250 个并行事务。鉴于此数字组合了请求和响应大小，客户机不应该超出此内存占用量。如果要进行更精确的估算，应该使用请求大小和响应大小的混合，以更好地模拟现实世界事务方案。

要运行一个 Secure Gateway 客户机实例，建议至少使用 2 个核心和 4 GB 内存。

对于将在同一机器上运行多个客户机实例的 HA 方案，建议至少使用 4 个核心和 8 GB 内存。

