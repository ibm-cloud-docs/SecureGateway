---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-19"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 与客户机进行交互
{: #client-interacting}

有多种方法可与客户机交互：

- 在启动之前，通过终端命令行
- 在启动之后，使用客户机的交互命令行
- 在启动之后，使用客户机的本地 UI

## 启动配置
{: #startup}

### 启动自变量和选项
{: #startup-args}

下表描述了可以与客户机的 startup 命令一起提供的所有可用选项。通过使用这些选项，可以在启动之前完成客户机的配置，而不是需要在客户机开始运行后单独进行设置。

|参数和自变量|描述|
| ------------- | ----------- |
|&lt;gateway ID&gt;|使用提供的网关标识连接到 {{site.data.keyword.Bluemix_notm}}|
| -F, -\-aclfile &lt;file&gt; |访问控制表文件|
| -g, -\-gateway &lt;hostname:port&gt; |用于手动选择特定网关目标（仅限高级使用）|
| -l, -\-loglevel &lt;level&gt; |将日志级别更改为 ERROR、INFO、DEBUG 或 TRACE|
| -p, -\-logpath &lt;file&gt; |将日志直接记录到特定文件|
| -t, -\-sectoken &lt;security token&gt; |用于此网关连接的安全性令牌|
| -P, -\-port &lt;port&gt; |用于运行 UI 的端口。缺省为端口 9003 |
| -w, -\-password &lt;password&gt; |用于保护 UI 的密码。缺省为无密码|
| -x, -\-proxy &lt;proxy agent&gt; |端口 9000 连接的代理|
| -\-noUI |阻止 UI 自动启动|
| -\-allow |允许与客户机的所有连接。将被 ACL 文件（如果提供）覆盖|
| -\-service |建立初始连接后，如果终止了所有子客户机，父代将在 60 秒内重新启动|

<b>注：</b>`--service`、`--allow` 和 `--noUI` 标志应该是命令行自变量中位于最后的参数。

最基本的用例是使用缺省设置开始建立单个客户机连接：

```
<gateway_id> -t <security_token>
```
{: pre}

还可以扩展这些选项以支持多个客户机自动连接。要将这些选项传递到多个客户机连接，必须使用特定格式进行传递。网关标识可以按照与单个客户机连接相同的方式传递（各网关标识之间以空格分隔）；不过，这些标识的顺序决定了其余自变量必须遵循的顺序。传入的其他任何自变量都必须以 `--` 分隔，才能够被正确获取。如果没有为特定标志传入任何内容，即认为不将该标志应用于任何客户机。

如果提供的自变量不足以满足所有网关标识，那么会按顺序应用，用完为止。例如，如果传入两个网关标识，但仅传入一个安全性令牌，那么该令牌将应用于第一个网关标识，而不会应用于第二个网关标识。  

如果提供的网关标识需要不同的自变量，那么应该在网关没有强制实施/提供的任何特定自变量处指定关键字 `none`。例如，如果传入三个网关标识，且用户想要为第一个和第三个网关标识指定日志级别，那么该自变量将类似于：`--loglevel DEBUG--none--TRACE`。在本例中，none 随后会缺省为 INFO。

### 启动自变量示例
{: #startup-examples}

单个客户机连接，定制日志级别，无 UI：

```
node lib/secgwclient.js <gateway_id> -t <security_token> -l <loglevel> --noUI
```
{: pre}

多个客户机连接，缺省选项：

```
node lib/secgwclient.js <gateway_id_1> <gateway_id_2> -t <security_token_1>--<security_token_2>
```
{: pre}

多个客户机连接，所有定制设置：

```
node lib/secgwclient.js <myGatewayID_1> <myGatewayID_2> -t none--<token for gateway 2> -l DEBUG--TRACE -p <full path to log file for gateway 1>--<full path to log file for gateway 2> -F <full path to ACL file for gateway 1>
```
{: pre}

返回到[入门 - 添加客户机](/docs/services/SecureGateway/securegateway_client.html)。

## 交互式配置
{: #interactive}

### 交互命令
{: #interactive-commands}

{{site.data.keyword.SecureGateway}} 客户机具有命令行界面 (cli) 和 shell 提示符，方便您进行配置和控制。此交互环境支持的功能集比命令行自变量更丰富，有利于更好地对客户机进行交互式控制。

|交互命令|描述|
| ------------- | ----------- |
|A, acl allow &lt;hostname:port&gt; &lt;worker ID&gt;|允许访问控制表|
|D, acl deny &lt;hostname:port&gt; &lt;worker ID&gt;|拒绝访问控制表|
|N, no acl &lt;hostname:port&gt; &lt;worker ID&gt;|除去访问控制表条目|
|S, show acl &lt;worker ID&gt;|显示所有访问控制表条目|
|F, acl file &lt;file&gt; &lt;worker ID&gt;|访问控制表文件|
|C, displayconfig &lt;worker ID&gt;|显示当前 {{site.data.keyword.SecureGateway}} 配置（如果可用的话）|
|a, authorize &lt;worker ID&gt;|为指定工作程序切换出站 TLS 连接的 rejectUnauthorized 参数的覆盖|
|t, sectoken &lt;security token&gt;|用于下一个网关连接的安全性令牌|
|c, connect &lt;gateway ID&gt;|使用提供的网关标识连接到 {{site.data.keyword.Bluemix_notm}}|
|l, loglevel &lt;level&gt; &lt;worker ID&gt;|将日志级别更改为 ERROR、INFO、DEBUG 或 TRACE|
|p, logpath &lt;file&gt; &lt;worker ID&gt;|将日志直接记录到特定文件|
|r, reverse &lt;worker ID&gt;|列出客户机当前在其上侦听逆向目标的端口|
|k, kill &lt;worker ID&gt;|终止指定的工作程序|
|e, select &lt;worker ID&gt;|指定要在其上执行命令的工作程序（除非另有指定）|
|d, deselect|取消选择先前指定的工作程序。发出 select 命令可以指定其他连接|
|w, password &lt;old password&gt; &lt;new password&gt;|设置 UI 密码。如果 &lt;new password&gt; 为空白，那么不会强制实施任何密码。在密码更新时需要使用 &lt;old password&gt;。密码只能包含字母|
|P, port &lt;new port&gt;|更改 UI 正在其上进行侦听的端口|
|u, uistart &lt;initial password&gt; &lt;port&gt;|在 localhost:&lt;port&gt;/dashboard 上启动 UI。如果 &lt;initial password&gt; 为空白，并且没有为会话设置其他密码，那么不会强制实施任何 UI 密码。如果 &lt;port&gt; 为空白，可在 9003 上访问 UI|
|U, uistop|关闭与此客户机会话相关联的 UI。在手动启动新 UI 之前，只能通过 CLI 访问该会话|
|R, revoke|清除与此客户机会话相关联的所有 UI 授权|
|q, quit|断开连接并退出|
|s, status &lt;worker ID&gt;|打印隧道及其连接的状态详细信息|
|L, list|显示当前关联的工作程序的列表|

<b>注：</b>如果使用 `select` 命令指定了连接，然后在未提供工作程序标识的情况下调用了其他命令，那么该命令会尝试在 `select` 指定的连接上运行。

有关配置访问控制表的更多详细信息，请[单击此处](/docs/services/SecureGateway/securegateway_acl.html)。

返回到[入门 - 添加客户机](/docs/services/SecureGateway/securegateway_client.html)。

## 客户机 UI
{: #client-ui}

<b>注：</b>在 Windows 或 Mac OS 上使用 Docker 时，不支持客户机 UI。

客户机 UI 为用户提供了本地 Web 界面（而非 CLI）来与 {{site.data.keyword.SecureGateway}} 客户机交互。缺省情况下，此 UI 在 `localhost:9003/dashboard` 处提供。此 UI 拆分为以下页面：

### 连接
{: #ui-connect}

这是 UI 的初始登录页面，用户可以在此处提供网关标识和安全性令牌以连接其第一个客户机。

### 登录
{: #ui-login}

如果 UI 受到密码保护，那么会显示此页面。如果在未强制实施密码的情况下显示此页面，请刷新页面以重定向。


### 仪表板
{: #ui-dashboard}

这是客户机连接后显示的主页。在此页面中，可以访问“查看日志”页面、“访问控制表”页面和“连接信息”页面。在底部，还可以选择将一个/多个连接的客户机断开连接。在页面顶部，将显示当前所选客户机，并且将提供用于连接更多客户机的选项。 

### 查看日志
{: #ui-logs}

此页面将允许您查看所选客户机（显示在页面右上角）生成的日志。使用日志下方的复选框可过滤显示的日志。

### 访问控制表
{: #ui-acl}

此页面将允许您操作所选客户机（显示在页面右上角）的访问控制表。您可以将规则分别添加到允许/拒绝表，也可以在页面底部上传文件。

### 连接信息
{: #ui-info}

此页面将显示所选客户机（显示在页面右上角）的当前连接信息。在此处可查看网关描述、当前连接数和逆向目标侦听器等信息。

返回到[入门 - 添加客户机](/docs/services/SecureGateway/securegateway_client.html)。

## 远程客户机终止
{: #client-remote}

如果已为客户机提供标识，那么可通过 SG UI 或 SG API 远程终止该客户机。如果终止作为服务运行的客户机，那么客户机将重新启动并获取新的客户机标识；但是，如果有多个客户机连接到该服务，那么仅当所有其余的客户机均已终止后，终止的客户机才会重新启动。

## 限制
{: #limits}

### 连接限制
{: #limits-conn}

SG 网关只能处理 250 个并发连接。如果并发请求数超过限制，那么会导致连接尝试被拒绝并产生等待时间。解决此问题的一个简单方法是在调用应用程序上使用连接池。请注意，250 个并发连接的限制是指网关上的限制，而不是客户机或目标上的限制。此限制将在网关上的所有客户机和目标之间共享。

### DataPower 客户机限制
{: #limits-datapower}

{{site.data.keyword.SecureGateway}} DataPower 客户机正在进行更新，以匹配其余客户机的功能。目前，该客户机具有以下限制：

- ACL 将缺省为 ALLOW ALL
- 不支持从 {{site.data.keyword.SecureGateway}} UI 启用和禁用网关或目标。但是，DataPower UI 中的“管理状态”选项可充当该特定客户机的启用/禁用开关。
- 不支持对连接和断开连接的实时网关状态更新进行连接状态轮询。
- DataPower V7.5.1.0 之前的版本不支持使用目标端 TLS 的完整证书链
- DataPower V7.5.1.0 之前的版本不支持云目标
- 日志级别不能更改为 TRACE 级别
- DataPower 中最新的 Secure Gateway 客户机版本为 1.8.0fp6，请检查[此处](/docs/services/SecureGateway/securegateway_install.html#installing-datapower)以获取更多信息
