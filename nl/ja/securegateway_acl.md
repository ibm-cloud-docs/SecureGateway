---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# アクセス制御リスト
{: #acl}

{{site.data.keyword.SecureGateway}} クライアントは、組み込みアクセス制御リスト (ACL) をサポートします。 クライアントの ACL を変更することによって、オンプレミス・リソースへのアクセスを許可または制限 (拒否) することができます。  これは、クライアントのコマンドを使用して対話式に行うことも、作用するようにしたい一連の ACL を含んでいるファイルを指定することで行うこともできます。

v1.5.0 以降、アクセス制御リストのルールは、同じゲートウェイに接続されたすべてのクライアント間で同期されます。  これにより、単一のクライアントから ACL を設定/更新するだけで、そのゲートウェイに接続されたすべての実行中のクライアント間でそれが共有されます。  また、新しいクライアントの接続も同じ ACL ルールを適用するように、ACL はセッション間で持続されます。

## アクセス制御リストのコマンド
{: #acl-commands}

以下の ACL コマンドがサポートされています。

```
acl allow [<hostname>]:[<port>]
acl deny [<hostname>]:[<port>]
no acl [<hostname>]:[<port>]
no acl
show acl
```
{: screen}

ホスト名またはポートが指定されないままの場合は、すべてのホスト名またはポートが暗黙指定されます。  例えば、以下は、ポート22 のすべてのホスト名を許可する ACL ルールです。

```
acl allow :22
```
{: pre}

以下は、すべてのポートのすべてのホスト名を許可する、実質的に ACL サポートを無効にする ACL ルールの例です。 <b>これは推奨されません。</b>

```
acl allow :
```
{: pre}

`show acl` コマンドは、現在設定されている ACL を表示するか、設定全体についてのメッセージを提供します。

[概説 - クライアントの追加](/docs/services/SecureGateway/securegateway_client.html)に戻ります。

## ACL を使用した HTTP/S の経路制御
{: #acl-route-control}

v1.6.0 以降、HTTP/S 宛先も ACL 項目で特定経路を強制できるようになりました。  これらは、通常の ACL 項目と同じ方法で追加されますが、ルールの末尾にパスが付加されます。 例えば、以下の例では、/my/api パスに続く要求のみが通過を許可されます。

```
acl allow localhost:80/my/api
```
{: pre}

このルールが設定されると、`<cloud host>:<cloud port>/my/api*` への要求は通過することができます。

経路は `acl allow` コマンドでのみサポートされます。

## アクセス制御リストの優先順位
{: #acl-precedence}

`acl allow :` コマンドを入力した後、さらに `acl allow` コマンドを入力した場合、無制限アクセスを許可しなくなったという想定に基づいて `acl allow:` の `ALL:ALL` 許可ルールはリストから削除されます。  `acl deny :` コマンドを入力した後、別の `acl deny` コマンドを入力した場合、すべてのアクセスを制限しなくなったという想定に基づいて `acl deny:` の `ALL:ALL` 拒否ルールはリストから削除されます。  現在の ACL ルールを CLI の `show acl` コマンドを通じてリストする場合、リストされないルールが許可されているか拒否されているかについて標識に表示されます。

## ACL ファイルのインポート
{: #import-acl-file}

`acl file` コマンドに、サポートされる ACL コマンドを含んでいるファイルのファイル名を指定できます。それらの ACL コマンドはクライアントによって始動時に読み取られます。 このファイルは以下の形式のコマンドを含んでいる必要があります。

```
acl allow [<hostname>]:[<port>]
acl deny [<hostname>]:[<port>]
no acl [<hostname>]:[<port>]
no acl
```
{: screen}

<b>注:</b> 他のパラメーターなしで `no acl` を指定すると、ACL テーブルがリセットされ、アクセスは DENY ALL に設定されます。

サンプル ACL ファイルが[ここ](/docs/services/SecureGateway/securegateway_acl-file.html)にあります。

[概説 - クライアントの追加](/docs/services/SecureGateway/securegateway_client.html)に戻ります。

## {{site.data.keyword.SecureGateway}} Docker クライアントへの ACL ファイルのコピー
{: #copy-acl-to-docker}

基本的に、{{site.data.keyword.SecureGateway}} Docker クライアントは独自の仮想化コンテナー内で実行されます。  したがって、ホスティング・マシンのファイル・システムへは、{{site.data.keyword.SecureGateway}} クライアントを含め、そのコンテナー内で稼働しているプロセスから直接アクセスすることはできません。  Docker Engine のバージョン 1.8.0 以降では、コンテナーが稼働中でも停止している間でも、'docker cp' コマンドを使用して、ホストに存在するファイルをコンテナーにインポートできます。{{site.data.keyword.SecureGateway}} クライアントの ACL FILE 対話式コマンドを使用するためには、これを実行する必要があります。

Docker の対話式 'cp' サポートをホストから Docker インスタンスに使用するには、docker 1.8.0 でなければなりません。 `docker --version` を使用してこれを確認できます。

これを実行すると、以下のようにバージョンが表示されます。 docker が非 root ユーザーとして稼働できるようにすることをお勧めします。そのため、エンジンを 1.8.0 または 1.8.2 にアップグレードした後に、提案されたコマンドを実行してください。

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

次に、以下の手順に従って、ACL ファイル・リストを Docker イメージにインポートします。

- 'docker ps' コマンドを実行してコンテナー ID を確認します。

```
CONTAINER ID IMAGE                        COMMAND                CREATED        STATUS  PORTS NAMES
764aadce386b ibmcom/secure-gateway-client "node lib/secgwclient" 27 seconds ago Up 26 seconds condescending_nobel
```
{: screen}

- コンテナー ID または名前を指定して 'docker cp' コマンドを実行して、ACL リストをコピーします。

```
docker cp 01_client.list 764aadce386b:/root/01_client.list
```
{: pre}

- 次に、Docker で稼働している {{site.data.keyword.SecureGateway}} クライアントで以下のようにします。

```
cli F /root/01_client.list

 [2015-10-01 08:12:30.091] [INFO] The current access control list is being reset and replaced by the user provided file: /root/01_client.list
 [2015-10-01 08:12:30.093] [INFO] The ACL file process accepts acl allow 127.0.0.1:27017
 [2015-10-01 08:12:30.094] [INFO] The ACL file process accepts acl allow 127.0.0.1:22
```
{: screen}

- ACL を表示します。

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

[概説 - クライアントの追加](/docs/services/SecureGateway/securegateway_client.html)に戻ります。
