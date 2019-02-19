---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Node.js TLS 상호 인증
{: #nodejs-tls-ma}

이 샘플은 온프레미스 대상의 양쪽 측면에 대한 상호 인증(사용자 인증 및 리소스 인증)을 구성하는 방법에 대해 설명합니다.

연결하려는 리소스가 Secure Gateway 클라이언트와 동일한 시스템에서 호스팅되고, 포트 8999에서 청취되며, 연결을 허용하기 위해 상호 인증이 필요함을 알고 있습니다. 이 정보를 사용하여 내 대상 작성을 시작할 수 있습니다.

![초기 TLS 상호 인증](./images/tlsMA.png?raw=true "초기 TLS 상호 인증")

## 사용자 인증
새로운 애플리케이션을 작성 중이기 때문에 사용할 애플리케이션을 위한 기존 인증서/키 쌍을 보유하고 있지 않으므로 Secure Gateway 서버에서 자동으로 쌍을 생성하도록 합니다. 이 경우 사용자 인증 업로드 필드를 비어 있는 상태로 놔두어야 합니다. 애플리케이션에서 사용하는 쌍을 이미 보유하고 있는 경우 대신 이 필드에 내 인증서를 업로드합니다.

## 리소스 인증

### 온프레미스 인증
Secure Gateway 클라이언트에서 연결 중인 리소스를 인증하기 위해 클라이언트에 해당 리소스가 제공할 인증서(또는 인증서 체인)를 제공해야 합니다. 내 리소스는 전체 인증서 체인을 보유하고 있지 않으므로 온프레미스 인증 필드에 해당 인증서만 업로드하면 됩니다. 이 필드에서는 최대 6개의 개별 인증서 파일(.pem, .cer, .der, .crt)이 허용됩니다.

### 클라이언트 인증서 및 키
Secure Gateway 클라이언트가 내 리소스에 자신을 식별할 방법을 지정해야 하는 경우 여기에서 사용할 클라이언트를 위한 인증서 및 키를 업로드할 수 있습니다. 클라이언트와 리소스가 동일한 시스템에서 실행 중이기 때문에 이 항목을 비어 있는 상태로 놔두고 Secure Gateway 서버에서 자동으로 쌍을 생성하도록 할 수 있습니다. 내 리소스가 별도의 호스트에 있는 경우 [업로드할 인증서/키 쌍을 생성](/docs/services/SecureGateway/securegateway_keygen.html)해야 합니다.

![로컬 TLS 상호 인증](./images/localTLSma.png?raw=true "로컬 TLS 상호 인증")

이 대상을 작성하는 경우 Secure Gateway 서버는 로컬 리소스에 연결할 때 내 애플리케이션에서 사용할 인증서/키 쌍 및 Secure Gateway 클라이언트에서 사용할 쌍을 자동으로 생성합니다. 또한 이 대상에는 연결 중에 CA에서 사용하기 위해 Secure Gateway 클라이언트에 제공할 내 로컬 리소스의 인증서도 포함됩니다. 작성이 완료되면 다음 정보를 확인하기 위해 내 대상을 편집할 수 있습니다.

![로컬 TLS 상호 인증 세부사항](./images/editLocalTLSma.png?raw=true "로컬 TLS 상호 인증 세부사항")

## 보안 파일 다운로드
내 대상의 대상 정보 패널에는 내 대상과 연관된 모든 인증서 및 키가 포함된 .zip 파일을 다운로드하기 위한 링크가 존재합니다.

![상호 인증 정보 패널](./images/infoPanelMA.png?raw=true "상호 인증 정보 패널")

.zip을 다운로드하여 파일의 압축을 해제하면 다음과 같은 항목이 생성됩니다.

파일 이름 |용도
-- | --
secureGatewayCert.pem | Secure Gateway 서버의 기본 인증서
DigiCertCA2.pem | Secure Gateway 서버의 중간 인증서
DigiCertTrustedRoot.pem | Secure Gateway 서버의 루트 인증서
Nn5TJ34LyVQ_i2NOT_cert.pem | 클라우드 호스트 및 포트에 연결할 때 내 애플리케이션에서 사용하기 위해 자동으로 생성된 인증서
Nn5TJ34LyVQ_i2NOT_key.pem | 클라우드 호스트 및 포트에 연결할 때 내 애플리케이션에서 사용하기 위해 자동으로 생성된 키
Nn5TJ34LyVQ_i2NOT_destCert.pem | 대상에 연결할 때 SG 클라이언트에서 사용하기 위해 자동으로 생성된 인증서
Nn5TJ34LyVQ_i2NOT_destKey.pem | 대상에 연결할 때 SG 클라이언트에서 사용하기 위해 자동으로 생성된 키
Nn5TJ34LyVQ_clientCert.pem | `_destCert` 또는 `_destKey` 파일이 없을 경우 SG 클라이언트에서 사용하기 위해 게이트웨이에서 자동으로 생성된 인증서
localServerCert.pem | Secure Gateway 클라이언트의 CA에서 사용하기 위한 내 로컬 리소스의 인증서

## 애플리케이션 작성
내 대상을 위한 클라우드 호스트 및 포트와 다양한 인증서 및 키를 보유하고 있기 때문에 Secure Gateway에 연결할 내 애플리케이션 작성을 시작할 수 있습니다. 이 애플리케이션은 내 로컬 TLS 서버에 연결하여 소량의 데이터를 수신한 후 연결을 닫는 단순한 Node.js 애플리케이션입니다.

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

## TLS 서버 작성
연결을 청취하고, 연결이 정상적으로 완료되는 경우 메시지를 작성하고, 해당 연결을 닫는 단순한 Node.js 애플리케이션인 내 로컬 TLS 서버가 있습니다.

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

## 클라이언트 액세스 제어 목록 업데이트
내 애플리케이션을 테스트하기 전에 내 클라이언트의 ACL이 적절하게 구성되어 있는지 확인해야 합니다. 내 ACL에 `localhost:8999`를 추가했습니다.

```
--------------------------------------------------------------------
           -- Secure Gateway Client Access Control List --

 Connections to unlisted rules are currently: denied

 hostname:port                path           value
 localhost:8999                            Allow
--------------------------------------------------------------------
```
{: screen}

## 연결 테스트
내 애플리케이션, 내 로컬 서버 및 내 클라이언트가 모두 구성되었기 때문에 내 애플리케이션 및 내 클라이언트에서 다음과 같은 출력이 표시됩니다.

### 애플리케이션

```
Client connected, authorized: true
Client data: Successful connection message from the server
Client connection closing
```
{: screen}

### Secure Gateway 클라이언트

```
[2017-04-07 12:22:42.363] [INFO] (Client ID Nn5TJ34LyVQ_qCB) Connection #1 is being established to localhost:8999
[2017-04-07 12:22:42.381] [INFO] (Client ID Nn5TJ34LyVQ_qCB) Connection #1 to localhost:8999 was closed
```
{: screen}

## 성공했습니다!
사용자 인증 및 리소스 인증 모두를 위한 상호 인증이 정상적으로 구성되었습니다!
