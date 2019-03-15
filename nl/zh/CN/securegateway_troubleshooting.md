---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 故障诊断
{: #troubleshooting}

## 运行 Secure Gateway 客户机的最佳实践
{: #best-practices}

- 在对通过 Secure Gateway 客户机本身桥接的服务具有网络可视性的操作系统 (OS) 分区上运行该客户机。例如，某些托管的虚拟化环境支持多种网络连接方式，包括 NAT 和桥接。确保选择正确的连接类型，以便从因特网访问 {{site.data.keyword.Bluemix}} 服务。
- 将 Secure Gateway 安装到公司安全策略允许的 IT 环境中。此环境通常位于受保护的黄色区域或 DMZ 中，在这些区域中，公司可以建立相应的安全控制来保护内部部署资产。安装 Secure Gateway 客户机时，务必遵循公司安全策略和指示信息。
- 在将客户机安装到环境中之前，请确保因特网和内部部署资产均可访问，并且所有主机名均可由 DNS 进行解析。

## 初始故障诊断步骤
{: #initial-troubleshooting}

- 从发出请求的应用程序启动失败的请求
- 检查 Secure Gateway 客户机日志
- 如果未从请求生成任何客户机日志，那么问题在于发出请求的应用程序和 Secure Gateway 服务器之间。问题范围可能从网络可靠性、不匹配的请求协议一直到不正确的 TLS 相互认证握手。
- 如果客户机从请求生成了错误级别日志，说明问题出在 SG 客户机和内部部署资源之间。下表包含常见错误、通常导致这些错误的问题以及对其进行故障诊断的可能方法。

错误|典型原因|故障诊断方法
--- | --- | ---
ETIMEDOUT|由于网络约束，客户机找不到要连接到的主机名/IP。|尝试从运行客户机的主机对目标的主机名/IP 执行 ping 操作，以确保网络连通性。如果运行的是 Docker 版本的客户机，那么使用 `--net=host` 将容器与主机操作系统桥接在一起可能会解决此问题。
ECONNREFUSED|客户机已解析要连接到的主机名/IP，但无法开始进行连接握手。|导致此错误的原因通常是 SG 客户机与内部部署资源之间的协议不匹配（例如，客户机尝试与 host:port 建立 TCP 连接，但该 host:port 预期建立 TLS 连接）。在某些情况下，防火墙规则可能会导致此错误，而不是 ETIMEDOUT。
ECONNRESET|客户机已建立与目标的连接，但在握手期间发生错误（TLS 握手错误还可能会导致其他错误），或在内部部署资源正在处理请求期间发生错误。|应检查内部部署资源的日志，以确认没有任何错误导致连接中断。如果在内部部署日志中找不到任何错误，那么应检查目标配置以确保向客户机提供了相应的协议（如果需要，还提供相应的证书）来建立连接。
REMOTE_RST|SG 服务器端发生错误。<br><br> 对于内部部署目标，发出请求的应用程序连接到 SG 服务器期间发生错误，或者从内部部署资源接收数据时发生超时错误。<br><br> 对于云目标，这可能是从 TLS 握手失败到云资源中出错的任何错误。|对于内部部署目标，请确保发出请求的应用程序使用相应的协议来与 SG 服务器建立连接；如果从内部部署资源接收数据时发生此错误，请尝试增大/禁用超时。<br><br> 对于云目标，应检查云资源的日志，以确认没有任何错误导致连接中断。如果在云资源日志中找不到任何错误，那么应检查目标配置以确保向客户机提供了相应的协议（如果需要，还提供相应的证书）来建立连接。

在隧道的另一端发生 ECONNRESET 后，许多应用程序会遇到“挂起”问题。预期会有这种问题。Secure Gateway 无法将 RST 包传输到隧道的另一端，因为已对隧道另一端的 TCP 包执行了 ACK。在应用程序上定义超时（应用程序从未收到确认响应）是结束挂起的唯一方法。

## 将 Docker 客户机配置为在服务器重新启动后重新启动
{: #docker-auto-restart}

### 情况描述
{: #docker-auto-restart-what-is-happening}
重新启动运行 Secure Gateway 客户机的服务器后，必须手动重新启动 Secure Gateway Docker 客户机。如何使客户机在系统重新启动后自动启动？

### 解决方法
{: #docker-auto-restart-how-to-fix-it}

- 在 Linux 或 UNIX 系统上：
- 将 Docker 命令集成到可作为 CRON 作业的结果被调用的脚本中。
- 如果使用的是 Linux 工作站，那么可以创建 upstart 配置，以确保客户机会在服务器每次重新启动后启动。有关更多信息，请参阅 Docker Web 站点中的 Automatically Start Containers。
- 在 Windows 系统上，发出以下命令来启动客户机：

```
for /L %i in (0,0,0) do docker run -it ibmcom/secure-gateway-client <gateway_id>
```
{: pre}

## 连接错误消息：证书的 CN 中不包含主机
{: #not-in-cn}

### 情况描述
{: #not-in-cn-what-is-happening}
您尝试使用 Secure Gateway 客户机来实施内部部署客户端 TLS，但收到以下错误消息。

```
[ERROR] Connection #<connection ID> had error: Host: <host name>. is not cert's CN: <mycommonname>

其中：
    - connection ID 是客户机分配的连接编号。
    - host name 是连接的主机名。
    - mycommonname 是证书中使用的 FDQN 或公共名称。
```
{: screen}

### 原因
{: #not-in-cn-why-it-is-happening}
在内部部署应用程序与您上传到 {{site.data.keyword.Bluemix_notm}} 的证书之间，此目标的常用名称（例如，服务器 FQDN 或您的名称）不匹配。

- 检查以下各项：
- 已正确生成证书并使用正确的服务器 FQDN 或主机名。
- 已将正确的证书上传到此客户机的 {{site.data.keyword.Bluemix_notm}} 目标。

### 解决方法
{: #not-in-cn-how-to-fix-it}

 1. 在 {{site.data.keyword.Bluemix_notm}} UI 中，转至 Secure Gateway“仪表板”。
 2. 选择目标，然后单击“编辑”图标。
 3. 单击“上传证书”。
 4. 上传用于连接到内部部署系统的 PEM 证书文件。

## 证书列表中不包含 IP
{: #san}

### 情况描述
{: #san-what-is-happening}
所提供证书中的 CN 是网关的 IP 地址，但该证书没有与该 IP 地址相匹配的 SAN，因此客户机连接失败。  

由于主机名解析问题，我们将使用目标中的 IP 地址。所提供证书中的 CN 是网关的 IP 地址，但该证书没有与该 IP 地址相匹配的 SAN，因此客户机连接失败。

您使用 TLS 创建了目标，但是您没有使用该目标的主机名，而是使用了其 IP 地址。连接客户机时，抛出以下错误。

```
[2015-10-15 13:00:04.866] [INFO] Connection #0 is being established to 10.3.20.31:443
[2015-10-15 13:00:05.426] [ERROR] Connection #0 to destination 10.3.20.31:443 had error: IP: 10.3.20.31 is not in the cert's list:
[2015-10-15 13:00:05.427] [INFO] Connection #0 to 10.3.20.31:443 was closed
```
{: screen}

### 原因
{: #san-why-it-is-happening}
发生的情况是网关客户机中的 SSL 验证码以其他方式处理此目标，因为此目标使用的是 IP 地址，而不是主机名。因此，它不会与证书的 CN 相匹配，而是查找证书的 SAN
来匹配 IP 地址。由于证书中没有 SAN，因此它将其视为错误连接而导致 SSL 握手失败。

### 解决方法
{: #san-how-to-fix-it}
如果您查看错误消息，其中不会显示 CN（例如，[ERROR] Connection ## had error: Host: . is not cert&apos;s CN:），而是显示证书的列表，因此我们认为您错误地生成了自签名证书。问题是，证书是使用 FQDN 或 CN 以及 IP_Address 生成的，这将不起作用，因为仅在使用 SAN 时支持 IP 地址。

使用 openssl 以 IP 作为 CN 生成证书的方法：

1. 创建 openssl 配置文件，我已从 /usr/lib/ssl/openssl.cnf 复制了自己的 openssl 配置文件
2. 在该文件中添加 alternate_names 部分，如下所示：

   ```
   [ alternate_names ]

   IP.1 = &lt;my application&apos;s ip&gt;
   ```
   {: codeblock}

3. 在 [ v3_ca ] 部分中添加以下行：

    ```
    subjectAltName = @alternate_names
    ```
    {: pre}

4. 在 CA_default 部分下，取消注释 copy_extensions（扩展复制选项：请谨慎使用）：

    ```
    copy_extensions = copy
    ```
    {: pre}

5. 生成专用密钥

    ```
    openssl genrsa -out private.key 3072
    ```
    {: pre}

6. 使用与组织相关的选项来生成证书

    ```
    openssl req -new -x509 -key private.key -sha256 -out certificate.pem -days 730 -config
    ```
    {: pre}

7. 组合文件

    ```
    cat private.key certificate.pem > SAN.pem
    ```
    {: pre}

8. 将 SAN.pem 文件作为客户机 TLS 证书装入目标。
9. 将 SAN.pem 文件装入内部部署应用程序，然后重新启动。

10. 可以针对 TCP、HTTP 或 HTTPS 来配置目标，现在您的云端应用程序应该能够连接到内部部署应用程序。

如果您遇到 UNABLE_TO_VERIFY_LEAF_SIGNATURE 问题，请检查 openssl.conf 文件，并将以下内容：

    ```
    keyUsage = digitalSignature, keyEncipherment
    ```
    {: pre}

 更改为缺省值：

    ```
    keyUsage = nonRepudiation, digitalSignature, keyEncipherment
    ```
    {: pre}

## 连接错误消息：DEPTH_ZERO_SELF_SIGNED_CERT
{: #depth-zero}

### 情况描述
{: #depth-zero-what-is-happening}
您尝试使用 Secure Gateway 客户机来实施内部部署客户端 TLS，但收到以下错误消息。

```
[ERROR] Connection #<connection ID> had error: DEPTH_ZERO_SELF_SIGNED_CERT Where:

    connection ID is a client assigned connection number.
```
{: screen}

### 原因
{: #depth-zero-why-it-is-happening}
定义的目标缺少客户机端证书。

### 解决方法
{: #depth-zero-how-to-fix-it}
 1. 在 {{site.data.keyword.Bluemix_notm}} UI 中，转至 Secure Gateway“仪表板”。
 2. 选择目标，然后单击“编辑”图标。
 3. 单击“上传证书”。
 4. 上传用于连接到内部部署系统的 PEM 证书文件。


## 如何在 Docker 客户机中以交互方式装入 ACL 文件？
{: #docker-load-acl}

### 情况描述
{: #docker-load-acl-what-is-happening}
由于 Docker 是一种容器或虚拟环境，因此在容器实际启动之前，无法直接访问文件系统。这样一来，Docker 只有在实际启动并运行之后，才能读取主机的文件系统。

### 解决方法
{: #docker-load-acl-how-to-fix-it}
下面是可以执行的操作：

- 创建 Dockerfile 以包含 aclfile.txt

```
FROM ibmcom/secure-gateway-client
ADD aclfile.txt /tmp/aclfile.txt
```
{: codeblock}

- 构建新的 Docker 映像

```
docker build -t ads-secure-gateway-client .
```
{: pre}

- 运行新的 Docker 映像（需要指定 -t 和 -i 选项，否则将收到错误，提示找不到文件或 ENOENT）：

```
docker run -t -i ads-secure-gateway-client1  --F /tmp/aclfile.txt
```
{: pre}

- 输出结果应如下所示：

```
[2015-09-30 16:50:32.084] [INFO] The current access control list is being reset and replaced by the user provided file: /tmp/aclfile.txt
[2015-09-30 16:50:32.086] [INFO] The ACL file process accepts acl allow :8000
[2015-09-30 16:50:32.087] [INFO] The ACL file process accepts acl deny local
```
{: screen}

## 获取更多帮助和支持
{: #getting-help-and-support}

如果您有关于使用 Secure Gateway 开发或部署应用程序的技术疑问，请将您的疑问发布到 [Stack Overflow ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://stackoverflow.com/search?q=securegateway+ibm-bluemix) 上。使用“ibm-bluemix”和“secure-gateway”标记您的疑问，以便 {{site.data.keyword.Bluemix_notm}} 开发团队可以更轻松地找到该疑问。

如果您有关于该服务的疑问或要了解有关如何入门的指示信息，请使用 [IBM developerWorks dW Answers ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/answers/topics/securegateway/?smartspace=bluemix) 论坛，并使用“bluemix”和“Secure Gateway”标记。

有关使用这些论坛的更多详细信息，请查看[此处的“获取帮助”页面](https://console.ng.bluemix.net/docs/support/index.html#getting-help)。

有关提交 IBM 支持凭单或有关支持级别和凭单严重性的信息，请参阅[联系支持人员](https://console.ng.bluemix.net/docs/support/index.html#contacting-support)。

提交凭单时，请尽可能多地提供以下信息：

- 运行客户机的操作系统
- 使用的客户机版本（可在客户机上使用“C”命令找到版本）
- 如果这是 UI 问题，请粘贴或附加任何关联的浏览器控制台日志和屏幕快照
- 粘贴或附加发出请求的应用程序的任何关联日志和时区
- 粘贴或附加 Secure Gateway 客户机的任何关联日志和时区
- 提供所使用的目标的详细信息（使用屏幕快照或填写下面的字段）：
   - 目标标识
   - 协议
   - 目标端认证
   - 上传的证书（只需提供名称以及证书上传到的 Box 文件夹或链接）
