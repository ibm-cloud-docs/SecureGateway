---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Node.js TLS 交互鑑別
{: #nodejs-tls-ma}

此範例將說明如何為內部部署目的地兩端配置「交互鑑別」：「使用者鑑別」及「資源鑑別」。

我知道我要連接的資源將於與「Secure Gateway 用戶端」相同的機器上進行管理、它將會在埠 8999 上進行接聽，並且需要交互鑑別以容許連線。透過此資訊，我可以開始建立我的目的地。

![起始 TLS 交互鑑別](./images/tlsMA.png?raw=true "起始 TLS 交互鑑別")

## 使用者鑑別
{: #tls-ma-user-auth}
我正在建立全新的應用程式，因此沒有任何既存的憑證/金鑰配對可供應用程式使用，所以我會讓 Secure Gateway 伺服器自動為我產生配對。要這麼做，我只需要將「使用者鑑別」上傳欄位保留空白。如果我已有應用程式要使用的配對，則會改為將憑證上傳至此欄位。

## 資源鑑別
{: #tls-ma-resource-auth}

### 內部部署鑑別
{: #tls-ma-on-prem-auth}
為了讓「Secure Gateway 用戶端」鑑別它所連接的資源，我必須提供用戶端該資源將提出的憑證（或憑證鏈）。我的資源沒有完整的憑證鏈，因此我只需要將其憑證上傳至「內部部署鑑別」欄位。此欄位最多接受 6 個個別的憑證檔（.pem、.cer、.der、.crt）。

### 用戶端憑證及金鑰
{: #tls-ma-client-cert-and-key}
如果我需要指定「Secure Gateway 用戶端」如何對我的資源識別它自己，則可以在這裡上傳憑證及金鑰，以供用戶端使用。因為用戶端及資源都是在相同的機器上執行，所以我可以將這些項目保留為空白，並讓 Secure Gateway 伺服器自動為我產生配對。如果我的資源以前位於個別主機上，則我需要[產生要上傳的憑證/金鑰配對](/docs/services/SecureGateway?topic=securegateway-cert-key-management)。

![本端 TLS 交互鑑別](./images/localTLSma.png?raw=true "本端 TLS 交互鑑別")

當我建立此目的地時，Secure Gateway 伺服器會自動產生一個憑證/金鑰配對以供我的應用程式使用，以及一個配對以供「Secure Gateway 用戶端」在連接至我的本端資源時使用。目的地也會有本端資源的憑證，以便提供給「Secure Gateway 用戶端」在連線期間用於 CA。建立之後，我可以編輯我的目的地，以查看下列資訊：

![本端 TLS 交互鑑別詳細資料](./images/editLocalTLSma.png?raw=true "本端 TLS 交互鑑別詳細資料")

## 下載安全檔案
{: #tls-ma-download-files}
從目的地的目的地資訊畫面中，有一個鏈結可下載 .zip 檔案，其中包含與目的地相關聯的所有憑證及金鑰：

![「交互鑑別資訊」畫面](./images/infoPanelMA.png?raw=true "「交互鑑別資訊」畫面")

下載 .zip 並解壓縮檔案之後，我現在有下列項目：

檔名 | 用途
-- | --
secureGatewayCert.pem | Secure Gateway 伺服器的主要憑證
DigiCertCA2.pem | Secure Gateway 伺服器的中繼憑證
DigiCertTrustedRoot.pem | Secure Gateway 伺服器的主要憑證
Nn5TJ34LyVQ_i2NOT_cert.pem |自動產生的憑證，在連接至雲端主機及埠時，供我的應用程式使用
Nn5TJ34LyVQ_i2NOT_key.pem |自動產生的金鑰，在連接至雲端主機及埠時，供我的應用程式使用
Nn5TJ34LyVQ_i2NOT_destCert.pem |自動產生的憑證，在連接至目的地時，供「SG 用戶端」使用
Nn5TJ34LyVQ_i2NOT_destKey.pem |自動產生的金鑰，在連接至目的地時，供「SG 用戶端」使用
Nn5TJ34LyVQ_clientCert.pem |來自閘道且自動產生的憑證，在「SG 用戶端」沒有 `_destCert` 或 `_destKey` 檔案的情況下使用
localServerCert.pem | 要在「Secure Gateway 用戶端」的 CA 中使用的本端資源憑證

## 建立應用程式
{: #tls-ma-app-example}
既然我已具有目的地的雲端主機及埠以及各種憑證和金鑰，便可以開始撰寫應用程式來連接至 Secure Gateway。這個簡單的 Node.js 應用程式將連接至我的本端 TLS 伺服器、接收少量的資料，然後關閉連線。

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

## 建立 TLS 伺服器
{: #tls-ma-server-example}
我的本端 TLS 伺服器是一個簡單的 Node.js 應用程式，它會接聽連線、在成功連線時撰寫訊息，然後關閉該連線。

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

## 更新用戶端存取控制清單
{: #tls-ma-acl}
測試我的應用程式之前，我需要確定已適當配置我用戶端上的 ACL。我已將 `localhost:8999` 新增至 ACL：

```
--------------------------------------------------------------------
           -- Secure Gateway Client Access Control List --

 Connections to unlisted rules are currently: denied

 hostname:port                path           value
 localhost:8999                            Allow
--------------------------------------------------------------------
```
{: screen}

## 測試連線
{: #tls-ma-testing}
既然我的應用程式、我的本端伺服器及我的用戶端已全部配置，我便會從我的應用程式及用戶端取得下列輸出：

### 應用程式
{: #tls-ma-testing-app}

```
Client connected, authorized: true
Client data: Successful connection message from the server
Client connection closing
```
{: screen}

### Secure Gateway 用戶端
{: #tls-ma-testing-client}

```
[2017-04-07 12:22:42.363] [INFO] (Client ID Nn5TJ34LyVQ_qCB) Connection #1 is being established to localhost:8999
[2017-04-07 12:22:42.381] [INFO] (Client ID Nn5TJ34LyVQ_qCB) Connection #1 to localhost:8999 was closed
```
{: screen}

## 成功！
{: #tls-ma-testing-result}
我們已順利配置「使用者鑑別」及「資源鑑別」的「交互鑑別」！
