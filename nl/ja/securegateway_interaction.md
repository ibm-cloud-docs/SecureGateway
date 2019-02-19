---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-04"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# クライアントとの対話

クライアントと対話する方法はいくつかあります。

- 開始前に端末コマンド・ラインを使用する
- 開始後にクライアントの対話式コマンド・ラインを使用する
- 開始後にクライアントのローカル UI を使用する

## 開始構成
{: #startup}

### 開始の引数とオプション
{: #startup-args}

次の表に、クライアントの startup コマンドと一緒に指定できるすべての使用可能なオプションを示します。これらを使用すると、開始前にクライアントを完全に構成することができ、クライアントが実行中になった後に個々のセットアップを行う必要はありません。

| パラメーターと引数 | 説明 |
| ------------- | ----------- |
| &lt;gateway ID&gt; | 指定のゲートウェイ ID を使用して、{{site.data.keyword.Bluemix_notm}} に接続します |
| -F, -\-aclfile &lt;file&gt; | アクセス制御リストのファイル |
| -g, -\-gateway &lt;hostname:port&gt; | 特定のゲートウェイ宛先を手動で選択するために使用されます (高度な用途専用) |
| -l, -\-loglevel &lt;level&gt; | ログ・レベルを ERROR、INFO、DEBUG、または TRACE に変更します |
| -p, -\-logpath &lt;file&gt; | ロギングを特定のファイルに転送します |
| -t, -\-sectoken &lt;security token&gt; | このゲートウェイ接続に使用するセキュリティー・トークン |
| -P, -\-port &lt;port&gt; | UI が実行されるポート。デフォルトのポートは 9003 です |
| -w, -\-password &lt;password&gt; | UI を保護するためのパスワード。デフォルトはパスワードなしです |
| -x, -\-proxy &lt;proxy agent&gt; | ポート 9000 接続用のプロキシー |
| -\-noUI | UI が自動的に開始しないようにします |
| -\-allow | クライアントへのすべての接続を許可します。ACL ファイルが指定されていれば、それによってオーバーライドされます |
| -\-service | 初期接続の後、すべての子クライアントが終了すると 60 秒以内に親が再始動されます |

<b>注:</b> `--service`、`--allow`、および `--noUI` フラグは、コマンド・ライン引数の最後のパラメーターでなければなりません。

最も基本的なユース・ケースは、デフォルト設定を使用して単一クライアント接続で開始することです。

```
<gateway_id> -t <security_token>
```
{: pre}

これらのオプションは、複数のクライアントの自動接続もサポートするように拡張できます。これらを複数のクライアント接続に渡すには、特定の形式を使用して渡す必要があります。ゲートウェイ ID は、単一クライアント接続の場合と同じようにして (各ゲートウェイ ID をスペースで区切って) 渡すことができます。
ただし、これらの ID の順序によって、残りの引数で従う順序が決まります。  他の引数を渡すときには、正しく受け取られるように `--` で引数を区切ってください。  特定のフラグについて何も渡されないと、それは、どのクライアントにも適用されないと想定されます。

すべてのゲートウェイ ID に対応した数の引数が指定されない場合、引数がなくなるまで順番に適用されます。例えば、2 つの ゲートウェイ ID が渡されていて、1 つのセキュリティー・トークンが渡された場合、
そのトークンは最初のゲートウェイ ID に適用され、2 番目のゲートウェイ ID には適用されません。  

異なる引数を必要とするゲートウェイ ID が複数指定される場合は、ゲートウェイが強制/指定しない特定の引数の場所にキーワード
`none` を指定してください。  例えば、3 つのゲートウェイ ID が渡されていて、ユーザーが 1 番目と 3 番目のゲートウェイにログ・レベルを指定したい場合、`--loglevel DEBUG--none--TRACE` のように引数を指定します。この場合、none は、デフォルトの INFO になります。

### 開始引数の例
{: #startup-examples}

単一のクライアント接続、カスタム・ログ・レベル、UI なし:

```
node lib/secgwclient.js <gateway_id> -t <security_token> -l <loglevel> --noUI
```
{: pre}

複数のクライアント接続、デフォルト・オプション:

```
node lib/secgwclient.js <gateway_id_1> <gateway_id_2> -t <security_token_1>--<security_token_2>
```
{: pre}

複数のクライアント接続、すべてカスタム設定:

```
node lib/secgwclient.js <myGatewayID_1> <myGatewayID_2> -t none--<token for gateway 2> -l DEBUG--TRACE -p <full path to log file for gateway 1>--<full path to log file for gateway 2> -F <full path to ACL file for gateway 1>
```
{: pre}

[概説 - クライアントの追加](/docs/services/SecureGateway/securegateway_client.html)に戻ります。

## 対話式構成
{: #interactive}

### 対話式コマンド
{: #interactive-commands}

{{site.data.keyword.SecureGateway}} クライアントにはコマンド・ライン・インターフェース (cli) とシェル・プロンプトが用意されており、構成と制御が簡単に行えるようになっています。 この対話式環境では、対話式でクライアントを簡単に制御できるよう、コマンド・ライン引数よりも豊富な機能が一式サポートされています。

| 対話式コマンド | 説明       |
| ------------- | ----------- |
| A, acl allow &lt;hostname:port&gt; &lt;worker ID&gt; | アクセス制御リスト許可 |
| D, acl deny &lt;hostname:port&gt; &lt;worker ID&gt; | アクセス制御リスト拒否 |
| N, no acl &lt;hostname:port&gt; &lt;worker ID&gt; | アクセス制御リストのエントリーを削除します |
| S, show acl &lt;worker ID&gt; | アクセス制御リストのエントリーをすべて表示します |
| F, acl file &lt;file&gt; &lt;worker ID&gt; | アクセス制御リスト・ファイル |
| C, displayconfig &lt;worker ID&gt; | 現在の {{site.data.keyword.SecureGateway}} 構成 (使用可能な場合) を表示します |
| a, authorize &lt;worker ID&gt; | 指定されたワーカーについてアウトバウンド TLS 接続用の rejectUnauthorized パラメーターのオーバーライドを切り替えます |
| t, sectoken &lt;security token&gt; | 次のゲートウェイ接続に使用するセキュリティー・トークン |
| c, connect &lt;gateway ID&gt; | 指定のゲートウェイ ID を使用して、{{site.data.keyword.Bluemix_notm}} に接続します |
| l, loglevel &lt;level&gt; &lt;worker ID&gt; | ログ・レベルを ERROR、INFO、DEBUG または TRACE に変更します |
| p, logpath &lt;file&gt; &lt;worker ID&gt; | ロギングを特定のファイルに送信します |
| r, reverse &lt;worker ID&gt; | クライアントがリバース宛先を現在 listen しているポートをリストします |
| k, kill &lt;worker ID&gt;  | 指定されたワーカーを終了します |
| e, select &lt;worker ID&gt;  | 特に指定がない場合にコマンドの実行対象とするワーカーを指定します。 |
| d, deselect | 前に指定したワーカーを選択解除します。  別の接続を指定するには、select コマンドを発行してください。 |
| w, password &lt;old password&gt; &lt;new password&gt; | UI パスワードを設定します。  &lt;new password&gt; がブランクの場合、パスワードは適用されません。 パスワード更新時には &lt;old password&gt; が必要です。パスワードは英字のみでなければなりません |
| P, port &lt;new port&gt; | UI が listen しているポートを変更します |
| u, uistart &lt;initial password&gt; &lt;port&gt; | localhost:&lt;port&gt;/dashboard で UI を開始します。 &lt;initial password&gt; がブランクになっていて、当該セッションに対して他にパスワードが設定されていないと、UI パスワードは適用されません。 &lt;port&gt; がブランクになっていると、この UI には 9003 で到達できます |
| U, uistop | このクライアント・セッションと関連付けられている UI を閉じます。 新しい UI が手動で開始されるまでは、このセッションへのアクセスは CLI を介してのみ可能です |
| R, revoke | このクライアント・セッションと関連付けられている UI 許可をすべてクリアします |
| q, quit | 切断して終了します |
| s, status &lt;worker ID&gt;  | トンネルとその接続の状況の詳細を表示します |
| L, list | 現在関連付けられているワーカーのリストを表示します |

<b>注:</b> `select` コマンドで接続が指定されていて、別のコマンドがワーカー ID を指定せずに呼び出された場合、そのコマンドは、`select` で指定された接続で実行されます。

アクセス制御リストの構成について詳しくは、[ここをクリック](/docs/services/SecureGateway/securegateway_acl.html)してください。

[概説 - クライアントの追加](/docs/services/SecureGateway/securegateway_client.html)に戻ります。

## クライアント UI
{: #ui}

<b>注:</b> Windows または MacOS で Docker を使用している場合、クライアント UI はサポートされません。

クライアント UI は、CLI ではなく {{site.data.keyword.SecureGateway}} クライアントと対話するためにユーザーが使用するローカル Web インターフェースを提供します。デフォルトでは、この UI は `localhost:9003/dashboard` で使用可能です。この UI は以下のページに分かれています。

### 接続
{: #ui-connect}

これは UI の最初のランディング・ページであり、ユーザーは最初のクライアントを接続するためのゲートウェイ ID およびセキュリティー・トークンをここで指定できます。

### ログイン
{: #ui-login}

このページは、 UI がパスワードで保護されている場合に表示されます。パスワードが施行されていないときにこのページが表示された場合は、ページを最新表示してリダイレクトされるようにしてください。

### ダッシュボード
{: #ui-dashboard}

これは、クライアントが接続された後のメインページです。ここから、「ログの表示」ページ、「アクセス制御リスト」ページ、および「接続情報」ページにアクセスできます。下部で、接続された 1 つ以上のクライアントを切断することもできます。ページの上部には、現在選択されているクライアントと、追加のクライアントを接続するオプションが表示されます。

### ログの表示 (View Logs)
{: #ui-logs}

このページでは、選択したクライアント (ページの右上に表示されます) によって生成されるログを表示できます。表示されるログは、ログの下のチェック・ボックスでフィルタリングできます。

### アクセス制御リスト
{: #ui-acl}

このページでは、選択したクライアント (ページの右上に表示されます) のアクセス制御リストを操作できます。ページの下部で、ルールを許可/拒否テーブルに個別に追加するか、ファイルをアップロードできます。

### 接続情報 (Connection Info)
{: #ui-info}

このページには、選択したクライアント (ページの右上に表示されます) の現行接続情報が表示されます。ここには、ゲートウェイの説明、現行接続の数、逆方向の宛先のリスナーなどの情報を表示できます。

[概説 - クライアントの追加](/docs/services/SecureGateway/securegateway_client.html)に戻ります。

## リモート・クライアントの終了
{: #remote}

クライアントに ID が提供されている場合、SG UI または SG API を介してリモートでクライアントを終了できます。サービスとして実行されているクライアントを終了した場合、クライアントは再始動して新しいクライアント ID を取得します。ただし、接続されたクライアントがサービスに複数ある場合は、終了したクライアントは、残りすべてのクライアントが終了されるまで再始動されません。

## 制限
{: #limits}

### 接続の制限
{: #limits-conn}

SG ゲートウェイは、250 の同時接続しか処理できません。同時要求の数が制限を超えると、接続の試行が拒否され、待ち時間が発生する可能性があります。これを修正する簡単な方法は、呼び出し側アプリケーションで接続プールを使用することです。250 の同時接続の制限はゲートウェイに対してであり、クライアントでも宛先でもないことに注意してください。この制限は、ゲートウェイ上のすべてのクライアントと宛先で共有されます。

### DataPower クライアント制限
{: #limits-datapower}

{{site.data.keyword.SecureGateway}} DataPower クライアントは、他のクライアントの機能に合わせて更新中です。現在、以下のような制限があります。

- ACL はデフォルトで ALLOW ALL になります。
- {{site.data.keyword.SecureGateway}} UI からのゲートウェイまたは宛先の使用可能化および使用不可化はサポートされません。 ただし、DataPower UI の「管理状態」オプションが、その特定クライアントに対する on/off スイッチとして機能します。
- ゲートウェイの接続と切断の状況をリアルタイムで更新するための接続状況ポーリングはサポートされません。
- DataPower バージョン 7.5.1.0 より前では、宛先側 TLS を備えた完全な証明書チェーンはサポートされません。
- DataPower バージョン 7.5.1.0 より前では、クラウドの宛先はサポートされません。
- ログ・レベルを TRACE レベルに変更できません。
