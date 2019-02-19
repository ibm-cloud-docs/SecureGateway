---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# トラブルシューティング
{: #troubleshooting}

## Secure Gateway クライアントの実行のベスト・プラクティス
{: #best}

- Secure Gateway クライアントは、クライアント自体によってブリッジされたサービスに対するネットワークの可視性を備えたオペレーティング・システム (OS) パーティションで実行してください。 例えば、一部のホストされる仮想化環境では、NAT やブリッジを含む複数のネットワーク接続モードをサポートします。 必ず、インターネットから {{site.data.keyword.Bluemix}} サービスへのアクセスを提供する正しい接続タイプを選択してください。
- 会社のセキュリティー・ポリシーで許可されている、ご使用の IT 環境に Secure Gateway をインストールします。 これは通常、オンプレミス資産を保護するために会社が適切なセキュリティー管理を導入できる保護された警戒区域 (DMZ) にあります。 Secure Gateway クライアントをインストールする際には、常に自社のセキュリティー・ポリシーと指示に従ってください。
- ご使用の環境にクライアントをインストールする前に、インターネットとオンプレミス資産の両方がアクセス可能であること、およびすべてのホスト名が DNS によって解決可能であることを確認してください。

## 初期トラブルシューティング手順

- 失敗した要求を要求側アプリケーションから開始します
- Secure Gateway クライアントのログを調べます。
- 要求からクライアント・ログが生成されていない場合、問題は、要求側アプリケーションと Secure Gateway サーバーの間にあります。これには、ネットワーク信頼性、要求プロトコルの不一致、不適切な TLS 相互認証ハンドシェークなどがあります。
- クライアントが要求からエラー・ログを生成した場合、問題は SG クライアントとオンプレミス・リソースの間にあります。以下の表に、一般的なエラー、その原因となる典型的な問題、およびトラブルシューティング方法を示します。

エラー| 典型的な原因 | トラブルシューティング方法
--- | --- | ---
ETIMEDOUT | クライアントは、ネットワーク制約が原因で接続先のホスト名/IP を検出できません。| クライアントが実行されているホストから宛先のホスト名/IP を ping してみて、ネットワーク接続を確認します。Docker バージョンのクライアントが実行されている場合、`--net=host` を使用してコンテナーをホスト OS とブリッジングすると問題が解決することがあります。
ECONNREFUSED | クライアントは、接続先のホスト名/IP を解決しましたが、接続ハンドシェークを開始できません。| 典型的な原因は、SG クライアントとオンプレミス・リソースとの間のプロトコル不一致です (例えば、TLS 接続を予期しているホスト:ポートにクライアントが TCP 接続を試行している)。場合によっては、ファイアウォール・ルールが原因で ETIMEDOUT ではなくこのエラーが発生することがあります。
ECONNRESET | クライアントは、宛先への接続を確立しましたが、何らかの問題が、ハンドシェーク中 (TLS ハンドシェークのエラーが結果として別のエラーになることもあります) またはオンプレミス・リソースによって要求が処理されている間に発生しました。| オンプレミス・リソースのログを調べて、接続の中断を引き起こしたエラーがないことを確認します。オンプレミス・ログに何も見つからない場合、宛先構成を調べて、接続のための適切なプロトコル (および必要に応じて証明書) がクライアントに提供されていることを確認します。
REMOTE_RST | SG サーバー・サイドで発生したエラーがあります。<br><br> オンプレミス宛先の場合、要求側アプリケーションが SG サーバーに接続する際のエラー、または、オンプレミス・リソースからデータを受信する際のタイムアウト・エラーがあります。<br><br> クラウド宛先の場合、TLS ハンドシェーク障害からクラウド・リソースのエラーまでの何かである可能性があります。| オンプレミス宛先の場合、要求側アプリケーションが適切なプロトコルを使用して SG サーバーとの接続を確立していることを確認し、オンプレミス・リソースからデータを受信しているときにエラーが発生する場合はタイムアウトを長くするか無効にしてみてください。<br><br> クラウド宛先の場合、クラウド・リソース・ログを調べて、接続の中断を引き起こしたエラーがないことを確認します。クラウド・リソース・ログに何も見つからない場合、宛先構成を調べて、接続のための適切なプロトコル (および必要に応じて証明書) がクライアントに提供されていることを確認します。

多くのアプリケーションは、トンネルの相手側で ECONNRESET が発生した後にハングします。これは、予期されていることです。Secure Gateway は、トンネルの相手側で RST パケットを再生することはできません。なぜなら、TCP パケットはトンネルのその側で既に ACK されているからです。アプリケーション・レベルでのタイムアウト (アプリケーションは確認応答を受け取らない) が、ハングを終わらせる唯一の方法です。

## サーバーの再始動時に Docker クライアントを再始動するように構成する
{: #docker}

### 状況
Secure Gateway クライアントが実行されているサーバーを再始動する場合、手動で Secure Gateway Docker クライアントを再始動する必要があります。 システムの再始動後にクライアントが自動的に開始するようにするには、どうすればよいでしょうか。

### 修正方法

- Linux または UNIX システムの場合:
- CRON ジョブの結果として呼び出せるスクリプトに Docker コマンドを組み込みます。
- Linux ワークステーションを使用している場合は、サーバーが再始動されるたびにクライアントが再始動されるよう、upstart 構成を作成することができます。 詳しくは、Docker の Web サイトの「Automatically Start Containers」を参照してください。
- Windows システムの場合は、以下のコマンドを発行してクライアントを始動します。

```
for /L %i in (0,0,0) do docker run -it ibmcom/secure-gateway-client <gateway_id>
```
{: pre}

## 接続エラー・メッセージ: host is not in cert's CN
{: #not-in-cn}

### 状況
Secure Gateway クライアントを使用してオンプレミスのクライアント・サイド TLS を実装しようとすると、以下のエラー・メッセージを受け取ります。

```
[ERROR] Connection #<connection ID> had error: Host: <host name>. is not cert's CN: <mycommonname>

各部の内容は以下のとおりです。
    - connection ID は、クライアントが割り当てた接続番号です。
    - host name は、接続のホスト名です。
    - mycommonname は、FQDN か、または証明書で使用される共通名です。
```
{: screen}

### 発生する理由
オンプレミス・アプリケーションと、この宛先用に {{site.data.keyword.Bluemix_notm}} にアップロードされた証明書との間で、共通名 (例えばサーバー FQDN または自分の名前) が一致しません。

- 以下の項目を確認してください。
- 証明書を正しく生成したこと、および正しいサーバー FQDN またはホスト名を使用したこと。
- このクライアント用に、 {{site.data.keyword.Bluemix_notm}} 宛先に正しい証明書をアップロードしたこと。

### 修正方法

 1. {{site.data.keyword.Bluemix_notm}} UI で、Secure Gateway ダッシュボードに移動します。
 2. 宛先を選択し、「編集」アイコンをクリックします。
 3. 「証明書のアップロード」をクリックします。
 4. オンプレミス・システムに接続するために使用される PEM 証明書ファイルをアップロードします。

## IP is not in the cert's list
{: #san}

### 状況
提示された証明書内の CN はゲートウェイの IP アドレスですが、証明書にはその IP アドレスに一致する SAN がないため、クライアントは接続に失敗します。  

ホスト名解決の問題があるため、宛先内で IP アドレスが使用されます。提示された証明書内の CN はゲートウェイの IP アドレスですが、証明書にはその IP アドレスに一致する SAN がないため、クライアントは接続に失敗します

ユーザーは TLS を使用して宛先を作成しましたが、宛先のホスト名を使用する代わりに、宛先の IP アドレスを使用しました。クライアントの接続時に、次のエラーがスローされています。

```
[2015-10-15 13:00:04.866] [INFO] Connection #0 is being established to 10.3.20.31:443
[2015-10-15 13:00:05.426] [ERROR] Connection #0 to destination 10.3.20.31:443 had error: IP: 10.3.20.31 is not in the cert's list:
[2015-10-15 13:00:05.427] [INFO] Connection #0 to 10.3.20.31:443 was closed
```
{: screen}

### 発生する理由
ゲートウェイ・クライアントの SSL 検証コードは、この宛先ではホスト名ではなく IP アドレスが使用されているため、それを異なる方法で処理しています。証明書の CN と突き合わせる代わりに、証明書の SAN で IP アドレスの一致を探しています。証明書内に SAN がないため、誤った接続と認識して、SSL ハンドシェークを失敗させます。

### 修正方法
エラー・メッセージで言及されているのは CN ではなく (例: [ERROR] 接続 ## でエラーが発生しました: Host: . is not cert&apos;s CN: ) 証明書のリストです。このことから、自己署名証明書が正しく生成されなかったと考えられます。問題は、IP アドレスを含む CN または FQDN を使用して証明書を生成することです。IP アドレスは SAN を使用する場合にのみサポートされるため、これは機能しません。

openssl で CN として IP を含む証明書を生成する方法は次のとおりです。

1. openssl 構成ファイルを作成します。この例では /usr/lib/ssl/openssl.cnf にあったものをコピーしました。
2. 以下のように、このファイルに alternate_names セクションを追加します。

   ```
   [ alternate_names ]

   IP.1 = &lt;my application&apos;s ip&gt;
   ```
   {: codeblock}

3. [ v3_ca ] セクションに、以下の行を追加します。

    ```
    subjectAltName = @alternate_names
    ```
    {: pre}

4. CA_default セクションで、copy_extensions のコメントを外します (これは、エクステンションをコピーするオプションであり、使用には注意が必要です)。

    ```
    copy_extensions = copy
    ```
    {: pre}

5. 秘密鍵を作成します。

    ```
    openssl genrsa -out private.key 3072
    ```
    {: pre}

6. 組織についてのオプションを指定して証明書を作成します。

    ```
    openssl req -new -x509 -key private.key -sha256 -out certificate.pem -days 730 -config
    ```
    {: pre}

7. ファイルを結合します。

    ```
    cat private.key certificate.pem > SAN.pem
    ```
    {: pre}

8. SAN.pem ファイルをクライアント TLS 証明書として宛先にロードします。
9. SAN.pem ファイルをオンプレミス・アプリケーションにロードして再始動します。

10. 宛先は TCP、HTTP、または HTTPS 用に構成できます。これで、クラウド・サイドのアプリケーションはオンプレミス・アプリケーションに接続できるはずです。

UNABLE_TO_VERIFY_LEAF_SIGNATURE 問題が発生した場合、openssl.conf ファイルを調べて、以下を見つけます。

    ```
    keyUsage = digitalSignature, keyEncipherment
    ```
    {: pre}

上記を次のようにデフォルトに変更してください。

    ```
    keyUsage = nonRepudiation, digitalSignature, keyEncipherment
    ```
    {: pre}

## 接続エラー・メッセージ: DEPTH_ZERO_SELF_SIGNED_CERT
{: #depth-zero}

### 状況
Secure Gateway クライアントを使用してオンプレミスのクライアント・サイド TLS を実装しようとすると、以下のエラー・メッセージを受け取ります。

```
[ERROR] 接続 #<connection ID> でエラーが発生しました: DEPTH_ZERO_SELF_SIGNED_CERT

    connection ID は、クライアントで割り当てられた接続番号です。
```
{: screen}

### 発生する理由
定義した宛先に、クライアント・サイド証明書がありません。

### 修正方法
 1. {{site.data.keyword.Bluemix_notm}} UI で、Secure Gateway ダッシュボードに移動します。
 2. 宛先を選択し、「編集」アイコンをクリックします。
 3. 「証明書のアップロード」をクリックします。
 4. オンプレミス・システムに接続するために使用される PEM 証明書ファイルをアップロードします。


## Docker クライアントで ACL ファイルを対話式にロードするには、どうすればよいですか?
{: #docker-acl}

### 状況
Docker は、コンテナーつまり仮想化環境なので、コンテナーが実際に起動されるまでファイル・システムに直接アクセスすることができません。このため、実際に起動されて実行中となるまで、ホスト・マシンのファイル・システムを読み取ることができません。

### 修正方法
修正方法は次のとおりです。

- aclfile.txt を組み込むための Dockerfile を作成します。

```
FROM ibmcom/secure-gateway-client
ADD aclfile.txt /tmp/aclfile.txt
```
{: codeblock}

- 新しい Docker イメージをビルドします。

```
docker build -t ads-secure-gateway-client .
```
{: pre}

- 新しい Docker イメージを実行します (-t オプションと -i オプションを指定する必要があります。そうしないと、「ファイルが見つからない」エラーまたは ENOENT エラーが発生します)。

```
docker run -t -i ads-secure-gateway-client1 --F /tmp/aclfile.txt
```
{: pre}

- 次のような出力が得られます。

```
[2015-09-30 16:50:32.084] [INFO] The current access control list is being reset and replaced by the user provided file: /tmp/aclfile.txt
[2015-09-30 16:50:32.086] [INFO] The ACL file process accepts acl allow :8000
[2015-09-30 16:50:32.087] [INFO] The ACL file process accepts acl deny local
```
{: screen}

## 追加ヘルプとサポートの利用
{: #support}

Secure Gateway を使用したアプリケーションの開発またはデプロイに関する技術上の質問がある場合は、[スタック・オーバーフロー![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://stackoverflow.com/search?q=securegateway+ibm-bluemix) に質問を投稿します。質問に「ibm-bluemix」および「secure-gateway」のタグを付けて、{{site.data.keyword.Bluemix_notm}} 開発チームが簡単に見つけられるようにしてください。

サービスまたは開始方法に関する質問がある場合は、[IBM developerWorks dW Answers ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/answers/topics/securegateway/?smartspace=bluemix) フォーラムで「bluemix」タグおよび「Secure Gateway」タグを使用します。

これらのフォーラムの使用について詳しくは、[ヘルプの利用についてのページ](https://console.ng.bluemix.net/docs/support/index.html#getting-help)を参照してください。

IBM サポート・チケットのオープン、またはサポート・レベルとチケットの重大度については、
[サポートへのお問い合わせ](https://console.ng.bluemix.net/docs/support/index.html#contacting-support)を参照してください。

チケットの送信時には、できるだけ多くの情報を提供してください。

- クライアントはどの OS で稼働していますか?
- どのバージョンのクライアントが使用されていますか? (これは、クライアントで「C」コマンドを使用して判別できます)
- UI の問題の場合、関連するブラウザー・コンソール・ログおよびスクリーン・ショットを貼り付けるか添付してください
- 関連する要求側アプリケーション・ログをすべて貼り付けるか添付してください
- 関連する Secure Gateway クライアント・ログをすべて貼り付けるか添付してください
- 使用されている宛先の詳細を提供してください (スクリーン・ショットを提供するか、以下のフィールドを埋めてください)。
   - 宛先 ID
   - プロトコル
   - 宛先側の認証
   - アップロードされた証明書 (名前とアップロード先のボックスのみ)
