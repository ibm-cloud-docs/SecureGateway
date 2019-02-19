---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Node.js TLS 相互認証

このサンプルでは、オンプレミス宛先の両サイドに相互認証 (ユーザー認証とリソース認証) を構成する方法について説明します。

接続したいリソースは Secure Gateway クライアントと同じマシン上でホストされ、ポート 8999 で listen すること、および接続を許可するために相互認証を必要とするとが分かっているとします。この情報を使用して、宛先の作成を開始できます。

![初期 TLS 相互認証](./images/tlsMA.png?raw=true "初期 TLS 相互認証")

## ユーザー認証
この例では、まったく新しくアプリケーションを作成しようとしていて、アプリケーション用の証明書/鍵ペアは事前には存在しないため、Secure Gateway サーバーがペアを自動的に生成するようにします。これを行うために必要なことは、「ユーザー認証」のアップロード・フィールドを空のままにしておくことのみです。アプリケーションが使用するためのペアが既にある場合は、代わりに、保有している証明書をこのフィールドにアップロードすることになります。

## リソース認証

### オンプレミス認証
接続しようとしているリソースを Secure Gateway クライアントが認証するには、リソースが提示する証明書 (または証明書チェーン) をクライアントに提供する必要があります。この例では、リソースは完全な証明書チェーンを持っていないと想定しているため、リソースの証明書を「オンプレミス認証」フィールドにアップロードすればいいだけです。このフィールドには、最大 6 つの別個の証明書ファイル (.pem、.cer、.der、.crt) を指定できます。

### クライアント証明書およびクライアント鍵
Secure Gateway クライアントがそれ自身をリソースに識別させる方法を指定する必要がある場合、クライアントが使用するための証明書および鍵をアップロードできます。クライアントとリソースは同じマシン上で実行されているため、これらを空のままにすることができます。そうすると、Secure Gateway サーバーがペアを自動的に生成してくれます。リソースが別のホスト上にある場合は、[アップロードするための証明書/鍵ペアを生成](/docs/services/SecureGateway/securegateway_keygen.html)する必要があります。

![ローカル TLS 相互認証](./images/localTLSma.png?raw=true "ローカル TLS 相互認証")

この宛先を作成すると、Secure Gateway サーバーは、アプリケーションが使用するための証明書/鍵ペアと、ローカル・リソースに接続するときに Secure Gateway クライアントが使用するためのペアを自動的に生成します。この宛先には、接続時に CA で使用するために Secure Gateway クライアントに提供する、ローカル・リソースの証明書もあります。作成後、宛先を編集して以下の情報を確認できます。

![ローカル TLS 相互認証の詳細](./images/editLocalTLSma.png?raw=true "ローカル TLS 相互認証の詳細")

## セキュリティー・ファイルのダウンロード
宛先の宛先情報パネルには、宛先に関連付けられたすべての証明書と鍵を含んでいる .zip ファイルをダウンロードするためのリンクがあります。

![相互認証情報パネル](./images/infoPanelMA.png?raw=true "相互認証情報パネル")

.zip をダウンロードし、ファイルを解凍すると、以下が得られます。

ファイル名 | 目的
-- | --
secureGatewayCert.pem | Secure Gateway サーバーの 1 次証明書
DigiCertCA2.pem | Secure Gateway サーバーの中間証明書
DigiCertTrustedRoot.pem | Secure Gateway サーバーのルート証明書
Nn5TJ34LyVQ_i2NOT_cert.pem | クラウドのホストおよびポートへの接続時にアプリケーションが使用する自動生成された証明書
Nn5TJ34LyVQ_i2NOT_key.pem | クラウドのホストおよびポートへの接続時にアプリケーションが使用する自動生成された鍵
Nn5TJ34LyVQ_i2NOT_destCert.pem | 宛先への接続時に SG クライアントが使用するための自動生成された証明書
Nn5TJ34LyVQ_i2NOT_destKey.pem | 宛先への接続時に SG クライアントが使用するための自動生成された鍵
Nn5TJ34LyVQ_clientCert.pem | `_destCert` ファイルまたは `_destKey` ファイルがない場合に SG クライアントが使用するための、ゲートウェイからの自動生成された証明書
localServerCert.pem | Secure Gateway クライアントの CA で使用されるローカル・リソースの証明書

## アプリケーションの作成
これで、宛先用のクラウド・ホストとポート、およびさまざまな証明書と鍵が得られたので、Secure Gateway に接続するためのアプリケーションの作成を開始できます。これは、ローカル TLS サーバーに接続し、少量のデータを受信してから接続を閉じる単純な Node.js アプリケーションです。

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

## TLS サーバーの作成
接続を listen し、接続が成功したらメッセージを書き込んで接続を閉じる単純な Node.js アプリケーションであるローカル TLS サーバーがあります。

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

## クライアントのアクセス制御リストの更新
アプリケーションをテストする前に、クライアントの ACL が適切に構成されていることを確認する必要があります。ACL に `localhost:8999` を追加しました。

```
--------------------------------------------------------------------
           -- Secure Gateway クライアントのアクセス制御リスト --

 現時点でのリストにないルールへの接続: denied

 hostname:port                path           value
 localhost:8999                            Allow
--------------------------------------------------------------------
```
{: screen}

## 接続のテスト
アプリケーション、ローカル・サーバー、およびクライアントがすべて構成されたので、アプリケーションとクライアントから以下の出力を受け取ります。

### アプリケーション

```
Client connected, authorized: true
Client data: Successful connection message from the server
Client connection closing
```
{: screen}

### Secure Gateway クライアント

```
[2017-04-07 12:22:42.363] [INFO] (Client ID Nn5TJ34LyVQ_qCB) Connection #1 is being established to localhost:8999
[2017-04-07 12:22:42.381] [INFO] (Client ID Nn5TJ34LyVQ_qCB) Connection #1 to localhost:8999 was closed
```
{: screen}

## これで完了です。
ユーザー認証とリソース認証の両方の相互認証が正常に構成されました。
