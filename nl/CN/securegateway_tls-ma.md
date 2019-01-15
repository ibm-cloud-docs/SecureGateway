---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Node.js TLS 相互认证

此样本将说明如何为内部部署目标的两端配置相互认证：用户认证和资源认证。

我知道要连接的资源将与 Secure Gateway 客户机在同一台机器上托管，该资源将侦听端口 8999，并且需要相互认证才允许连接。使用这些信息，我可以开始创建自己的目标。

![初始 TLS 相互认证](./images/tlsMA.png?raw=true "初始 TLS 相互认证")

## 用户认证
我将创建全新的应用程序，所以我没有任何预先存在的证书/密钥对可供应用程序使用，因此将由 Secure Gateway 服务器自动为我生成证书/密钥对。为此，我只需要将“用户认证上传”字段保留为空即可。如果我已有可供应用程序使用的证书/密钥对，那么可改为将证书上传到此字段中。

## 资源认证

### 内部部署认证
为了使 Secure Gateway 客户机能够对其连接的资源进行认证，我必须向该客户机提供资源将要呈现的证书（或证书链）。我的资源没有完整的证书链，因此我只需要将其证书上传到“内部部署认证”字段即可。此字段最多可接受 6 个不同的证书文件（.pem、.cer、.der 和 .crt）。

### 客户机证书和密钥
如果需要指定 Secure Gateway 客户机如何向资源表明自己的身份，那么可以在此上传证书和密钥以供客户机使用。由于客户机和资源是在同一台机器上运行，因此可以使这些信息保留为空，而让 Secure Gateway 服务器自动生成证书/密钥对。如果资源位于单独的主机上，那么需要[生成要上传的证书/密钥对](./securegateway_keygen.html)。

![本地 TLS 相互认证](./images/localTLSma.png?raw=true "本地 TLS 相互认证")

创建此目标时，Secure Gateway 服务器会自动生成一个证书/密钥对供应用程序使用，还会自动生成一个证书/密钥对供 Secure Gateway 客户机在连接到本地资源时使用。目标还将具有本地资源的证书，用于在连接期间提供给 Secure Gateway 客户机以在 CA 中使用。创建后，可以编辑目标以查看以下信息：

![本地 TLS 相互认证详细信息](./images/editLocalTLSma.png?raw=true "本地 TLS 相互认证详细信息")

## 下载安全文件
在目标的目标信息面板中，有一个链接可用于下载 .zip 文件，该文件包含与我的目标关联的所有证书和密钥：

![相互认证信息面板](./images/infoPanelMA.png?raw=true "相互认证信息面板")

下载该 .zip 并解压出文件后，我现在有以下文件：

文件名|用途
-- | --
secureGatewayCert.pem|Secure Gateway 服务器的主证书
DigiCertCA2.pem|Secure Gateway 服务器的中间证书
DigiCertTrustedRoot.pem|Secure Gateway 服务器的根证书
Nn5TJ34LyVQ_i2NOT_cert.pem|自动生成的证书，供应用程序在连接到云主机和端口时使用
Nn5TJ34LyVQ_i2NOT_key.pem|自动生成的密钥，供应用程序在连接到云主机和端口时使用
Nn5TJ34LyVQ_i2NOT_destCert.pem|自动生成的证书，供 SG 客户机在连接到目标时使用
Nn5TJ34LyVQ_i2NOT_destKey.pem|自动生成的密钥，供 SG 客户机在连接到目标时使用
Nn5TJ34LyVQ_clientCert.pem|从网关自动生成的证书，供 SG 客户机在没有 `_destCert` 或 `_destKey` 文件时使用
localServerCert.pem|本地资源的证书，供在 Secure Gateway 客户机的 CA 中使用

## 创建应用程序
既然我已有了用于目标的云主机和端口以及各种证书和密钥，现在我可以开始编写应用程序以连接到 Secure Gateway。下面是一个简单的 Node.js 应用程序，将连接到本地 TLS 服务器，接收少量数据，然后关闭连接。

```javascript
let fs = require('fs');
let tls = require('tls');

let client_opts = {
    cert : fs.readFileSync('Nn5TJ34LyVQ_i2NOT_cert.pem'),
    key : fs.readFileSync('Nn5TJ34LyVQ_i2NOT_key.pem'),
    host : 'cap-sg-prd-1.integration.ibmcloud.com',
    port : 17996,
    rejectUnauthorized : true
}

let so = tls.connect(client_opts, function() {
    console.log('Client connected, authorized:', so.authorized);
    if (!so.authorized) console.log('Client authorization error:', so.authorizationError)
});

so.on('data', function(data) {
    console.log('Client data:', data.toString())
});

so.on('error', function(err) {
    console.log('Client error',err);
});

so.on('close', function() {
    console.log('Client connection closing');
});
```
{: codeblock}

## 创建 TLS 服务器
我具有本地 TLS 服务器，这是一个简单的 Node.js 应用程序，将侦听连接，在连接成功时写入消息，然后关闭该连接。

```javascript
let fs = require('fs');
let tls = require('tls');

let server_opts = {
    cert : fs.readFileSync('localServerCert.pem'),
    key : fs.readFileSync('localServerKey.pem'),
    // pfx : fs.readFileSync(''),
    // passphrase : '',
    ca : [fs.readFileSync('Nn5TJ34LyVQ_i2NOT_destCert.pem')],
    rejectUnauthorized : true,
    requestCert : true
}

// Create the local TLS Server with the settings defined above
let server = tls.createServer(server_opts, function(socket) {
    socket.write('Successful connection message from the server');
    socket.end();
});

// Set up the various event listeners for the server and log their output
server.on('error', function(err) {
    console.log('Server error', err);
});

server.on('close', function() {
    console.log('Server closing');
});

server.on('tlsClientError', function(err) {
    console.log('Server tlsClientError', err);
});

server.listen(8999);
```
{: codeblock}

## 更新客户机访问控制表
测试应用程序之前，我需要确保在客户机上正确配置了 ACL。我已向 ACL 添加了 `localhost:8999`：

```
--------------------------------------------------------------------
           -- Secure Gateway Client Access Control List --

 Connections to unlisted rules are currently: denied

 hostname:port                path           value
 localhost:8999                            Allow
--------------------------------------------------------------------
```
{: screen}

## 测试连接
既然我的应用程序、本地服务器和客户机都已配置好，现在我可从应用程序和客户机获得以下输出：

### 应用程序

```
Client connected, authorized: true
Client data: Successful connection message from the server
Client connection closing
```
{: screen}

### Secure Gateway 客户机

```
[2017-04-07 12:22:42.363] [INFO] (Client ID Nn5TJ34LyVQ_qCB) Connection #1 is being established to localhost:8999
[2017-04-07 12:22:42.381] [INFO] (Client ID Nn5TJ34LyVQ_qCB) Connection #1 to localhost:8999 was closed
```
{: screen}

## 成功！
我们已为用户认证和资源认证成功配置相互认证！
