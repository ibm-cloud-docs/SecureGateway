---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 访问控制表
{: #acl}

{{site.data.keyword.SecureGateway}} 客户机提供嵌入式访问控制表 (ACL) 支持。通过对客户机的 ACL 进行修改，可以允许或限制（拒绝）对内部部署资源的访问。这可以使用客户机命令以交互方式实现，也可以通过指定包含您希望生效的 ACL 的文件来实现。

从 V1.5.0 开始，访问控制表规则将在连接到同一网关的所有客户机之间同步。因此，您只需在一个客户机上建立/更新 ACL，即可在连接到该网关的所有运行中客户机之间共享。ACL 还将在会话之间持久存储，因此在连接新客户机时也将应用相同的 ACL 规则。

## 访问控制表命令
{: #acl-commands}

支持的 ACL 命令包括：

```
acl allow [<hostname>]:[<port>]
acl deny [<hostname>]:[<port>]
no acl [<hostname>]:[<port>]
no acl
show acl
```
{: screen}

如果表单中未填主机名或端口，那么表示所有主机名或端口。例如，以下 ACL 规则允许所有主机名用于端口 22。

```
acl allow :22
```
{: pre}

下面是另一个示例 ACL 规则，用于允许所有主机名用于所有端口，这实质上是禁用 ACL 支持。<b>建议不要这样做。</b>

```
acl allow :
```
{: pre}

`show acl` 命令将显示当前设置的 ACL 或提供有关总体设置的消息。

返回到[入门 - 添加客户机](/docs/services/SecureGateway/securegateway_client.html)。

## 使用 ACL 的 HTTP/HTTPS 路由控制
{: #acl-route-control}

从 V1.6.0 开始，HTTP/HTTPS 目标还可以在 ACL 条目上强制实施特定路由。这些路由按照添加典型 ACL 条目的相同方式进行添加，但要将路径附加到规则末尾。例如，以下命令仅允许遵循 /my/api 路径的请求通过：

```
acl allow localhost:80/my/api
```
{: pre}

落实此规则后，将允许对 `<cloud host>:<cloud port>/my/api*` 的请求通过。

仅在 `acl allow` 命令上支持路由。

## 访问控制表优先顺序
{: #acl-precedence}

提供 `acl allow :` 命令之后，如果再输入其他 `acl allow` 命令，那么系统将假定您不再希望允许不受限制的访问，从而将 `ALL:ALL` 允许规则（来自 `acl allow:`）从列表中除去。提供 `acl deny :` 命令之后，如果输入另一个 `acl deny` 命令，那么系统将假定您不再希望限制所有访问，从而将 `ALL:ALL` 拒绝规则（来自 `acl deny:`）从列表中除去。如果通过 CLI 命令 `show acl` 列出当前的 ACL 规则，将出现一个指示器，其中显示允许还是拒绝未列出的规则。

## 导入 ACL 文件
{: #import-acl-file}

您可以向 `acl file` 命令提供文件名，该文件中包含客户机在启动时将读取的受支持 ACL 命令。此文件应具有以下格式的命令：

```
acl allow [<hostname>]:[<port>]
acl deny [<hostname>]:[<port>]
no acl [<hostname>]:[<port>]
no acl
```
{: screen}

<b>注：</b>使用 `no acl` 而不包含其他任何参数时，将重置 ACL 表并将访问权设置为 DENY ALL。

您可以在[此处](/docs/services/SecureGateway/securegateway_acl-file.html)找到样本 ACL 文件。

返回到[入门 - 添加客户机](/docs/services/SecureGateway/securegateway_client.html)。

## 将 ACL 文件复制到 {{site.data.keyword.SecureGateway}} Docker 客户机
{: #copy-acl-to-docker}

{{site.data.keyword.SecureGateway}} Docker 客户机实质上在其自己的虚拟化容器中运行。因此，托管机器的文件系统无法由该容器内部运行的进程（包括 {{site.data.keyword.SecureGateway}} 客户机）直接访问。从 Docker Engine V1.8.0 开始，可以使用“docker cp”命令将主机上存在的文件导入到正在运行或已停止的容器中，必须完成此操作才能使用 {{site.data.keyword.SecureGateway}} 客户机的 ACL FILE 交互命令。

要在 Docker 中使用从主机到 Docker 实例的交互式“cp”支持，您必须使用的是 Docker 1.8.0。可以使用 `docker --version` 进行检查。

完成此操作后，您的版本应该显示为如下所示。建议您允许 Docker 以非 root 用户身份运行，因此请在将引擎升级到 1.8.0 或 1.8.2 之后运行建议的命令。

```
Client:
 Version:      1.8.2
 API version:  1.20
 Go version:   go1.4.2
 Git commit:   0a8c2e3
 Built:        Thu Sep 10 19:21:21 UTC 2015
 OS/Arch:      linux/amd64

Server:
 Version:      1.8.2
 API version:  1.20
 Go version:   go1.4.2
 Git commit:   0a8c2e3
 Built:        Thu Sep 10 19:21:21 UTC 2015
 OS/Arch:      linux/amd64
```
{: screen}

然后，要将 ACL 文件列表导入到 Docker 映像，请执行以下步骤：

- 运行“docker ps”命令以查找容器标识

```
CONTAINER ID IMAGE                        COMMAND                CREATED        STATUS  PORTS NAMES
764aadce386b ibmcom/secure-gateway-client "node lib/secgwclient" 27 seconds ago Up 26 seconds condescending_nobel
```
{: screen}

- 使用“docker cp”命令以及容器标识或名称来复制 acl.list：

```
docker cp 01_client.list 764aadce386b:/root/01_client.list
```
{: pre}

- 接下来，在 Docker 中运行的 {{site.data.keyword.SecureGateway}} 客户机中，运行以下命令：

```
cli F /root/01_client.list

 [2015-10-01 08:12:30.091] [INFO] The current access control list is being reset and replaced by the user provided file: /root/01_client.list
 [2015-10-01 08:12:30.093] [INFO] The ACL file process accepts acl allow 127.0.0.1:27017
 [2015-10-01 08:12:30.094] [INFO] The ACL file process accepts acl allow 127.0.0.1:22
```
{: screen}

- 显示 ACL：

```
cli> S
 -------------------------------------------------------------------
           -- Secure Gateway Client Access Control List --          

  hostname                               port                  value
  127.0.0.1                             27017                  Allow
  127.0.0.1                                22                  Allow
 -------------------------------------------------------------------
```
{: screen}

返回到[入门 - 添加客户机](/docs/services/SecureGateway/securegateway_client.html)。
