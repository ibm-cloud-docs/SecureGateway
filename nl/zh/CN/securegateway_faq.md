---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-29"

---

# 常见问题
{: #sg-faq}

## OSI 模型层
{: #faq-osi}

### 问题
{: #osi-question}
Secure Gateway 服务表示的是 OSI 模型中的哪一层？

### 答案
{: #osi-answer}
Secure Gateway 服务表示的是 OSI 模型中的第 4 层。

## TLS 版本支持
{: #faq-tls}

### 问题
{: #tls-question}
Secure Gateway 服务支持哪个版本的 TLS？

### 答案
{: #tls-answer}
Secure Gateway 服务支持 TLS V1.2。

## 为什么我要禁用网关或目标？
{: #faq-disabled}

### 问题
{: #disabled-question}

您为什么会希望禁用网关或目标？

### 答案
{: #disabled-answer}
出于以下某种原因，您可能希望禁用目标或网关：

- 您创建了目标，但尚未设置安全性。在这种情况下，可能要禁用目标，直到为目标设置了安全性为止。
- 您不希望服务可供用户使用，因为您要对服务进行一些更新。在这种情况下，可能要临时禁用必要的网关，并等待服务更新。
- 您已在前端设置所有网关和目标，但后端仍在进行构建。在这种情况下，应禁用网关或目标，直到后端构建完成为止。

有关禁用网关或目标的更多信息，请参阅[如何管理 Secure Gateway 服务实例](/docs/services/SecureGateway?topic=securegateway-manage-sg-service)。

## 在多个空间中创建自动化的建议方法是什么？
{: #faq-automation-spaces}

### 问题
{: #automation-spaces-question}
客户环境有一个组织和三个空间。一个空间用于开发，一个空间用于编译打包，最后一个空间用于生产。客户应该创建一个 Secure Gateway 实例还是多个 Secure Gateway 实例（例如，针对每个空间创建一个实例）？如果客户可以创建多个网关，那么对于在每个空间中复用 Node.js 应用程序来创建网关和目标，是否有任何注意事项？

### 答案
{: #automation-spaces-answer}

- 您可以创建一个 Secure Gateway 实例用于所有三个空间。但是，您必须记住[特定套餐的网关和目标限制](/docs/services/SecureGateway?topic=securegateway-secure-gateway-service-plans)。
- 对于复用 Node.js 应用程序没有其他注意事项，因为 Secure Gateway 不需要任何服务绑定。


## 在多个组织中创建自动化的建议方法是什么？
{: #faq-automation-orgs}

### 问题
{: #automation-orgs-question}
客户环境有三个组织：一个用于开发，一个用于编译打包，一个用于生产。每个组织都需要一个 Secure Gateway 服务实例，并且该组织中的所有空间都可以使用此配置吗？

### 答案
{: #automation-orgs-answer}

- 您无需在每个组织中都有一个 Secure Gateway 服务实例。您可以在一个组织内有一个实例，然后在该实例内使用其他所有环境中的网关。使用此设置时，您必须记住[特定套餐的网关和目标限制](/docs/services/SecureGateway?topic=securegateway-secure-gateway-service-plans)。
- 您可以在每个组织中有一个 Secure Gateway 服务实例，并且所有空间都可以使用此配置。

## 应用程序需要位于同一空间吗？
{: #faq-app-space}

### 问题
{: #app-space-question}
需要在 Secure Gateway 服务所在的 {{site.data.keyword.Bluemix_notm}} 空间中运行 Node.js 应用程序吗？

### 答案
{: #app-space-answer}
不需要，您无需在 Secure Gateway 服务所在的 {{site.data.keyword.Bluemix_notm}} 空间中运行应用程序。

## 可以获取 Secure Gateway 服务器日志吗？
{: #faq-server-logs}

### 问题
{: #server-logs-question}
可以检索 Secure Gateway 服务器的错误级别日志吗？

### 答案
{: #server-logs-answer}
无法检索服务器上的错误级别日志。只能看到在请求时发生的错误。

## Secure Gateway 有哪些功能状态？
{: #faq-states}

### 问题
{: #states-question}
网关和目标有哪些不同的生命周期状态？

### 答案
{: #states-answer}

#### 非功能状态
{: #states-answer-non-functional}
1.7.0 发行版引入了一个新的分层套餐定价模型。此模型随附用于将网关和目标标记为“活动”或“非活动”的功能。在新套餐计费结构中，会根据用户拥有的网关和目标的数量向其收费。

- 不会对不活动项计费。
- 不活动的目标没有供应的云端口。
- 不活动网关中的所有目标也将处于不活动状态，并且在其网关同时设置为“活动”状态时，才能重新激活。
- 不活动项被视为非功能项。不活动项不能处于任何功能状态。

#### 功能状态
{: #states-answer-functional}
网关或目标标记为“活动”时，将对其计费。网关和目标的活动状态如下：

- 已启用 - 在正常情况下网关或目标可以随时使用。
- 已禁用 - 网关或目标已禁用。
    - 网关 - 客户机无法将数据传输到禁用的网关。
    - 目标 - 客户机无法创建与这些已禁用目标的连接。

## 如何在 Secure Gateway 客户机上了解数据活动？
{: #faq-data-size}

### 问题
{: #data-size-question}
如何通过 Secure Gateway 客户机了解数据活动？

### 答案
{: #data-size-answer}
在 Secure Gateway 客户机上，将日志级别更改为 TRACE。发送请求后，将显示以下信息。

从请求应用程序发送的数据大小：
```
[TRACE] Connection #<connection ID> transmitted data: <size> bytes
```

从目标发送的数据大小：
```
[TRACE] Connection #<connection ID> received data: <size> bytes
```

## 如何在 Secure Gateway 上获取实时连接量？
{: #faq-connect-num}

### 问题
{: #connect-num-question}
如何获取连接信息，例如，实时连接量，从 Secure Gateway 客户机发送和接收的数据大小？

### 答案
{: #connect-num-answer}

- 在 Secure Gateway 客户机交互式命令行上：
输入 `s` 可打印连接状态详细信息。 
- 在 Secure Gateway 客户机 UI 上，单击“连接信息”菜单。

这将显示以下连接统计信息： 
- 传输的总体数据大小。
- 接收到总体数据大小。
- 连接总量。
- 实时活动连接数。

注：此连接信息是针对客户机级别而非网关级别的。如果需要网关级别的连接信息，请检查连接到该网关的每个客户机。

## 使连接更安全的建议配置是什么？
{: #faq-secure-app}

### 问题
{: #secure-app-question}
使连接更安全的建议配置是什么？

### 答案
{: #secure-app-answer}

#### 使用相互认证
{: #secure-app-answer-ma}
对内部部署目标的两端启用相互认证可使 Secure Gateway 更加安全。在“用户认证”端上启用相互认证，可在通过 TLS/HTTPS 进行请求时使用客户机证书进行认证，从而限制对 Secure Gateway 云节点的访问。在“资源认证”端启用相互认证，可在连接到目标端点时提供相应凭证，并确保对内部部署资源的安全/加密访问。请参阅[配置相互认证](/docs/services/SecureGateway?topic=securegateway-add-dest#dest-mutual-auth)和 [Node.js TLS 相互认证](/docs/services/SecureGateway?topic=securegateway-nodejs-tls-ma#nodejs-tls-ma)，以获取更多信息。

#### 设置 IP 表规则（对于内部部署目标）
{: #secure-app-answer-iptables}
内部部署目标的 Secure Gateway 云主机和端口位于公共空间中；因此，缺省情况下允许所有人访问。要控制 Secure Gateway 上的流量访问，请将 iptables 规则设置为仅允许特定范围的 IP 和端口访问，从而保护内部部署资源。有关如何在 Secure Gateway 上配置 iptables 规则的更多信息，请参阅 [IP 表规则](/docs/services/SecureGateway?topic=securegateway-add-dest#dest-network-security)。

#### 配置访问控制表（对于内部部署目标）
{: #secure-app-answer-acl}
配置访问控制表支持以允许或限制对内部部署资源的访问，可通过指定特定目标主机和端口上的访问权，从而使内部部署目标更安全。建议还在 ACL 条目上定义允许的或限制的 HTTP/HTTPS 路由，以增强内部部署目标的安全性。请参阅[访问控制表](/docs/services/SecureGateway?topic=securegateway-acl#acl)和[使用 ACL 的 HTTP/HTTPS 路由控制](/docs/services/SecureGateway?topic=securegateway-acl#acl-route-control)，以获取更多信息。

#### 在 Secure Gateway 客户机 UI 上设置密码
{: #secure-app-answer-ui-pw}
建议设置 UI 密码以限制对 Secure Gateway 客户机 UI 的访问。请参阅[与客户机交互](/docs/services/SecureGateway?topic=securegateway-client-interacting#client-interacting)，以获取有关如何在 Secure Gateway 客户机终端命令行上使用启动配置或交互命令来设置密码的更多详细信息。

## 什么是网关迁移？为何在 2018 年 12 月后域发生更改？
{: #faq-gateway-migration}

### 问题
{: #gateway-migration-question}
2018 年 12 月维护后，网关面板上出现一个迁移按钮，该按钮的用途是什么？为何域发生更改？

### 答案
{: #gateway-migration-answer}

2018 年 12 月维护后，Secure Gateway 的云主机重命名为使用 `securegateway.appdomain.cloud` 而不是 `integration.ibmcloud.com`，并且将 `securegateway.cloud.ibm.com` 而不是 `bluemix.net` 用于网关认证。对于向后兼容性，现有网关会将旧域一直使用到迁移网关为止，并且 SG 客户机 v180fp9 和先前版本将一直使用 `bluemix.net` 进行网关认证。

迁移后，内部部署目标的云主机将更改为使用新域，用户/应用程序将需要更新以将请求发送到新的云主机。

目前，迁移不是必需的，并且没有停止支持旧域的确切日期，但是一旦确定该日期，将通知仍在使用旧域名的客户。

## 我可以在何处接收通知？
{: #faq-notification}

### 问题
{: #notification-question}
我可以在何处接收 Secure Gateway 通知，尤其是针对中断性维护的通知？

### 答案
{: #notification-answer}

您可以通过[状态页面](https://cloud.ibm.com/status?selected=status)获取通知。
- 要获取关于已完成/正在进行的中断性维护的通知，请在选项卡`状态`中搜索 `Secure Gateway`。
- 要获取关于计划内的中断性维护的通知，请在选项卡`计划维护`中搜索 `Secure Gateway`。

在 Secure Gateway 客户机意外断开连接时，请转至状态页面以检查此时是否有中断性维护。

如果维护需要中断超过 10 分钟，那么可能需要手动重新启动 Secure Gateway 客户机以在维护后重新连接到 Secure Gateway 服务器。通常，服务停机时间将等于或小于 10 分钟, Secure Gateway 客户机（v180 版本之后）应该能够自动重新连接到 Secure Gateway 服务器。

## 如何在 DataPower 上捕获 Secure Gateway 客户机日志？
{: #faq-dp-log}

### 问题
{: #dp-log-question}
如何捕获 Secure Gateway 客户机日志并将其写入 DataPower 上的文件？

### 答案
{: #dp-log-answer}

Secure Gateway 客户机日志的事件类别是 `sgclient`。 您可以创建[日志目标](https://www.ibm.com/support/knowledgecenter/en/SS9H2Y_7.7.0/com.ibm.dp.doc/logtarget_logs.html)以将具有特定事件类别的日志写入一个文件，示例如下：

- 从缺省域：
    - GUI 侧面板选择`对象` → `日志记录配置` → `日志目标`。 或者在`搜索`字段中搜索`日志目标`。
    - 选择`添加`按钮以添加日志目标
- 在`主要`选项卡中：
    - 填写`名称`
    - `文件`的`目标类型`
    - `文本`的`日志格式`
    - 填写`文件名`以定义输出位置，例如：`logtemp:///sgclient.log`
    - 选择`归档方式`为`轮换`
- 在`事件预订`选项卡中：
    - 填写`名称`
    - 选择`添加`按钮以添加目标事件预订
    - 填写`事件类别`，选择 `sgclient`
    - 填写`最低事件优先级`为 `debug`
