---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-08"

---

# 常见问题

## OSI 模型层
{: #osi}

### 问题
Secure Gateway 服务表示的是 OSI 模型中的哪一层？

### 答案
Secure Gateway 服务表示的是 OSI 模型中的第 4 层。

## TLS 版本支持
{: #tls}

### 问题
Secure Gateway 服务支持哪个版本的 TLS？

### 答案
Secure Gateway 服务支持 TLS V1.2。

## 为什么我要禁用网关或目标？
{: #disabled}

### 问题

您为什么会希望禁用网关或目标？

### 答案
出于以下某种原因，您可能希望禁用目标或网关：

- 您创建了目标，但尚未设置安全性。在这种情况下，可能要禁用目标，直到为目标设置了安全性为止。
- 您不希望服务可供用户使用，因为您要对服务进行一些更新。在这种情况下，可能要临时禁用必要的网关，并等待服务更新。
- 您已在前端设置所有网关和目标，但后端仍在进行构建。在这种情况下，应禁用网关或目标，直到后端构建完成为止。

有关禁用网关或目标的更多信息，请参阅[如何管理 Secure Gateway 服务实例](./securegateway_managing.html)。

## 在多个空间中创建自动化的建议方法是什么？
{: #automation-spaces}

### 问题
客户环境有一个组织和三个空间。一个空间用于开发，一个空间用于编译打包，最后一个空间用于生产。客户应该创建一个 Secure Gateway 实例还是多个 Secure Gateway 实例（例如，针对每个空间创建一个实例）？如果客户可以创建多个网关，那么对于在每个空间中复用 Node.js 应用程序来创建网关和目标，是否有任何注意事项？

### 答案

- 您可以创建一个 Secure Gateway 实例用于所有三个空间。但是，您必须记住[特定套餐的网关和目标限制](./securegateway_plans.html)。
- 对于复用 Node.js 应用程序没有其他注意事项，因为 Secure Gateway 不需要任何服务绑定。


## 在多个组织中创建自动化的建议方法是什么？
{: #automation-orgs}

### 问题
客户环境有三个组织：一个用于开发，一个用于编译打包，一个用于生产。每个组织都需要一个 Secure Gateway 服务实例，并且该组织中的所有空间都可以使用此配置吗？

### 答案

- 您无需在每个组织中都有一个 Secure Gateway 服务实例。您可以在一个组织内有一个实例，然后在该实例内使用其他所有环境中的网关。使用此设置时，您必须记住[特定套餐的网关和目标限制](./securegateway_plans.html)。
- 您可以在每个组织中有一个 Secure Gateway 服务实例，并且所有空间都可以使用此配置。

## 应用程序需要位于同一空间吗？
{: #app-space}

### 问题
需要在 Secure Gateway 服务所在的 {{site.data.keyword.Bluemix_notm}} 空间中运行 Node.js 应用程序吗？

### 答案
不需要，您无需在 Secure Gateway 服务所在的 {{site.data.keyword.Bluemix_notm}} 空间中运行应用程序。

## 可以获取 Secure Gateway 服务器日志吗？
{: #server-logs}

### 问题
可以检索 Secure Gateway 服务器的错误日志吗？

### 答案
无法检索服务器上的错误日志。只能看到在请求时发生的错误。

## Secure Gateway 有哪些功能状态？
{: #states}

### 问题
网关和目标有哪些不同的生命周期状态？

### 答案

#### 非功能状态
1.7.0 发行版引入了一个新的分层套餐定价模型。此模型随附用于将网关和目标标记为“活动”或“非活动”的功能。在新套餐计费结构中，会根据用户拥有的网关和目标的数量向其收费。

- 不会对不活动项计费。
- 不活动的目标没有供应的云端口。
- 不活动网关中的所有目标也将处于不活动状态，并且在其网关同时设置为“活动”状态时，才能重新激活。
- 不活动项被视为非功能项。不活动项不能处于任何功能状态。

#### 功能状态
网关或目标标记为“活动”时，将对其计费。网关和目标的活动状态如下：

- 已启用 - 在正常情况下网关或目标可以随时使用。
- 已禁用 - 网关或目标已禁用。
    - 网关 - 客户机无法将数据传输到禁用的网关。
    - 目标 - 客户机无法创建与这些已禁用目标的连接。

## 如何在 Secure Gateway 客户机上了解数据活动？
{: #data-size}

### 问题
如何通过 Secure Gateway 客户机了解数据活动？

### 答案
在 Secure Gateway 客户机上，将日志级别更改为 TRACE。发送请求后，将显示以下信息。

从请求应用程序发送的数据大小：
```
[TRACE] Connection #<connection ID> transmitted data: <size> bytes
```

从目标发送的数据大小：
```
[TRACE] Connection #<connection ID> received data: <size> bytes
```

## 如何在 Secure Gateway 上获取实时连接数？
{: #connect-num}

### 问题
如何获取实时连接信息以及通过 Secure Gateway 客户机发送和接收的数据大小？

### 答案

- 在 Secure Gateway 客户机交互式命令行上：
输入 `s` 可打印连接状态详细信息。 
- 在 Secure Gateway 客户机 UI 上，单击“连接信息”菜单。

这将显示以下连接统计信息： 
- 传输的总体数据大小。
- 接收到总体数据大小。
- 连接总数。
- 实时活动连接数。

注：Secure Gateway UI 上的“当前连接数”不会实时呈现。请使用以上方法在 Secure Gateway 客户机上检索实时连接信息。

## 使连接更安全的建议配置是什么？
{: #secure-app}

### 问题
使连接更安全的建议配置是什么？

### 答案

#### 使用相互认证
对内部部署目标的两端启用相互认证可使 Secure Gateway 更加安全。在“用户认证”端上启用相互认证，可在通过 TLS/HTTPS 进行请求时使用客户机证书进行认证，从而限制对 Secure Gateway 云节点的访问。在“资源认证”端上启用相互认证，可在连接到目标端点时提供相应凭证，确保对内部部署资源的安全/加密访问。请参阅[配置相互认证](./securegateway_destination.html#mutual-auth)和 [Node.js TLS 相互认证](./securegateway_tls-ma.html#node-js-tls-mutual-authentication)，以获取更多信息。

#### 设置 IP 表规则（对于内部部署目标）
内部部署目标的 Secure Gateway 云主机和端口位于公共空间中；因此，缺省情况下允许所有人访问。要控制 Secure Gateway 上的访问流量，请将 iptable 规则设置为仅允许特定范围的 IP 和端口访问内部部署资源，从而保护内部部署资源。有关如何在 Secure Gateway 上配置 iptable 规则的更多信息，请参阅 [IP 表规则](./securegateway_destination.html#configuring-network-security)。

#### 配置访问控制表（对于内部部署目标）
配置访问控制表支持以允许或限制对内部部署资源的访问，可通过指定特定目标主机和端口上的访问权，从而使内部部署目标更安全。建议在 ACL 条目上定义允许的 HTTP/HTTPS 路径或限制 HTTP/HTTPS 路径，以增强内部部署目标的安全性。请参阅[访问控制表](./securegateway_acl.html#access-control-list)和[使用 ACL 的 HTTPS/路由控件](./securegateway_acl.html#routes)，以获取更多信息。

#### 在 Secure Gateway 客户机 UI 上设置密码
建议设置 UI 密码以限制对 Secure Gateway 客户机 UI 的访问。请参阅[与客户机交互](./securegateway_interaction.html#interacting-with-the-client)，以获取有关如何在 Secure Gateway 客户机终端命令行上使用启动配置或交互命令来设置密码的更多详细信息。
