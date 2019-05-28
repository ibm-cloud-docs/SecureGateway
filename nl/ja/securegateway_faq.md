---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-29"

---

# よくあるご質問
{: #sg-faq}

## OSI モデル層
{: #faq-osi}

### 質問
{: #osi-question}
Secure Gateway サービスは、OSI モデルのどの層に相当しますか?

### 答え
{: #osi-answer}
Secure Gateway サービスは、OSI モデルの第 4 層に相当します。

## サポートされる TLS バージョン
{: #faq-tls}

### 質問
{: #tls-question}
Secure Gateway サービスは、TLS のどのバージョンをサポートしますか?

### 答え
{: #tls-answer}
Secure Gateway サービスは、TLS バージョン 1.2 をサポートします。

## ゲートウェイまたは宛先を使用不可にする理由
{: #faq-disabled}

### 質問
{: #disabled-question}

ゲートウェイまたは宛先を使用不可にする場合の理由は何ですか?

### 答え
{: #disabled-answer}
以下のいずれかの理由で、宛先またはゲートウェイを使用不可にする場合があります。

- 宛先を作成したが、セキュリティーをセットアップしていない。  この場合、セキュリティーがセットアップされるまで、宛先を使用不可にすることがあります。
- サービスを更新中のため、サービスをユーザーに対して利用不可にしたい。  この場合、必要なゲートウェイを一時的に使用不可にして、サービスが更新されるまで待つことがあります。
- フロントエンドにはゲートウェイおよび宛先をすべてセットアップしたが、バックエンドはまだ構築中である。  この場合、バックエンドの構築が完了するまで、ゲートウェイまたは宛先を使用不可にします。

ゲートウェイまたは宛先を使用不可にする方法について詳しくは、[Secure Gateway サービス・インスタンスの管理方法](/docs/services/SecureGateway?topic=securegateway-manage-sg-service)を参照してください。

## 複数のスペースにわたる自動化を作成するために推奨される手法は、どのようなものですか?
{: #faq-automation-spaces}

### 質問
{: #automation-spaces-question}
お客様の環境に 1 つの組織と 3 つのスペースがあります。  1 つのスペースは開発用、もう 1 つはステージング用、最後の 1 つは実動用です。  このお客様は、単一の Secure Gateway インスタンスを作成するべきですか、または複数のインスタンス (例えば、スペースごとに 1 つ) を作成するべきでしょうか?  このお客様が複数のゲートウェイを作成できる場合、1 つの Node.js アプリケーションを再使用して各スペースにゲートウェイおよび宛先を作成することに関する考慮事項はありますか?

### 答え
{: #automation-spaces-answer}

- 3 つのすべてのスペースに対して単一 Secure Gateway インスタンスを作成することができます。  ただし、ゲートウェイおよび宛先についての[特定のプランでの制限](/docs/services/SecureGateway?topic=securegateway-secure-gateway-service-plans)を考慮する必要があります。
- Secure Gateway はサービス・バインディングを必要としないため、Node.js アプリケーションの再使用に関する追加の考慮事項はありません。


## 複数の組織にわたる自動化を作成するために推奨される手法は、どのようなものですか?
{: #faq-automation-orgs}

### 質問
{: #automation-orgs-question}
お客様の環境に 3 つの組織 (開発用に 1 つ、ステージング用に 1 つ、実動用に 1 つ) があります。  組織ごとに 1 つの Secure Gateway サービス・インスタンスが必要ですか? また、構成はその組織内のすべてのスペースで使用可能ですか?

### 答え
{: #automation-orgs-answer}

- 各組織に Secure Gateway サービス・インスタンスがある必要はありません。いずれかの組織に 1 つのインスタンスを作成すれば、他のすべての環境からそのインスタンス内のゲートウェイを使用できます。  このセットアップの場合は、ゲートウェイおよび宛先についての[特定のプランでの制限](/docs/services/SecureGateway?topic=securegateway-secure-gateway-service-plans)を考慮する必要があります。
- 各組織に 1 つずつ Secure Gateway サービス・インスタンスがあるようにできます。その構成はすべてのスペースで使用可能になります。

## アプリは同じスペース内にある必要がありますか?
{: #faq-app-space}

### 質問
{: #app-space-question}
Node.js アプリは Secure Gateway サービスと同じ {{site.data.keyword.Bluemix_notm}} スペースで実行する必要がありますか?

### 答え
{: #app-space-answer}
いいえ。 Secure Gateway サービスと同じ {{site.data.keyword.Bluemix_notm}} スペースでアプリを実行する必要はありません。

## Secure Gateway サーバー・ログを取得できますか?
{: #faq-server-logs}

### 質問
{: #server-logs-question}
Secure Gateway サーバーのエラー・レベルのログを取り出すことができますか?

### 答え
{: #server-logs-answer}
サーバー上のエラー・レベルのログを取り出すことはできません。  要求を行った時点で生成されたエラーのみが表示可能です。

## Secure Gateway の機能状態には何がありますか?
{: #faq-states}

### 質問
{: #states-question}
ゲートウェイと宛先のライフサイクルの状態には、何がありますか?

### 答え
{: #states-answer}

#### 非機能状態
{: #states-answer-non-functional}
新しい階層型プラン料金モデルが 1.7.0 リリースで導入されました。 このモデルでは、ゲートウェイと宛先の両方に「アクティブ」または「非アクティブ」のマークを付けることができます。 新しいプラン請求構造には、ユーザーが所有するゲートウェイおよび宛先の数に対して課金される部分があります。

- 非アクティブ項目は請求されません。
- 非アクティブな宛先には、プロビジョンされたクラウド・ポートはありません。
- 非アクティブなゲートウェイ内のすべての宛先も非アクティブになり、ゲートウェイがアクティブに設定されるまでは再アクティブ化できません。
- 非アクティブ項目は非機能状態と見なされます。 非アクティブ項目がいずれかの機能状態になることはありません。

#### 機能状態
{: #states-answer-functional}
ゲートウェイまたは宛先は、アクティブのマークが付けられると請求されるようになります。 ゲートウェイおよび宛先のアクティブ状態は以下のとおりです。

- 有効 -ゲートウェイまたは宛先は、通常の環境で使用する準備ができています。
- 無効 -ゲートウェイまたは宛先は無効になっています。
    - ゲートウェイ - クライアントは、無効のゲートウェイにデータを流すことはできません。
    - 宛先 - クライアントは、無効の宛先への接続を作成できません。

## Secure Gateway クライアントでのデータ・アクティビティーを知るには、どうすればよいですか?
{: #faq-data-size}

### 質問
{: #data-size-question}
Secure Gateway クライアントを通過するデータ・アクティビティーを知るには、どうすればよいですか?

### 答え
{: #data-size-answer}
SecureGateway クライアントで、ログ・レベルを TRACE に変更します。 要求が送信された後、以下の情報が表示されるようになります。

要求アプリケーションから送信されたデータ・サイズ:
```
[TRACE] Connection #<connection ID> transmitted data: <size> bytes
```

宛先から送信されたデータ・サイズ:
```
[TRACE] Connection #<connection ID> received data: <size> bytes
```

## Secure Gateway でのリアルタイム接続の量を取得するには、どうすればよいですか?
{: #faq-connect-num}

### 質問
{: #connect-num-question}
リアルタイム接続の量、Secure Gateway クライアントとの間で送受信されたデータ・サイズなどの接続情報を取得するには、どうすればよいですか?

### 答え
{: #connect-num-answer}

- Secure Gateway クライアントの対話式コマンド・ラインで `s` を入力すると、接続状況詳細が出力されます。 
- Secure Gateway クライアント UI で、「接続情報」メニューをクリックします。

以下の接続統計情報が表示されます。 
- 伝送された総データ・サイズ。
- 受信された総データ・サイズ。
- 合計接続の量。
- リアルタイムのアクティブ接続。

注: この接続情報はクライアント・レベルの情報であり、ゲートウェイ・レベルの情報ではありません。 ゲートウェイ・レベルでの接続情報が必要な場合は、そのゲートウェイに接続された各クライアントを確認してください。

## 接続をよりセキュアにするために推奨されるのはどのような構成ですか?
{: #faq-secure-app}

### 質問
{: #secure-app-question}
接続をよりセキュアにするために推奨されるのはどのような構成ですか?

### 答え
{: #secure-app-answer}

#### 相互認証の使用
{: #secure-app-answer-ma}
オンプレミス宛先の両サイドで相互認証を有効にすると、Secure Gateway がよりセキュアになります。 ユーザー認証サイドでは、要求が TLS/HTTPS 経由の場合にクライアント証明書を使用して認証することによって Secure Gateway クラウド・ノードのアクセスを制限するために、相互認証を有効にします。 リソース認証サイドでは、相互認証を有効にして、宛先エンドポイントに接続するときに適切な資格情報を提示し、オンプレミス・リソースへのアクセスを確実に保護/暗号化します。 詳しくは、[相互認証の構成](/docs/services/SecureGateway?topic=securegateway-add-dest#dest-mutual-auth)および [Node.js TLS 相互認証](/docs/services/SecureGateway?topic=securegateway-nodejs-tls-ma#nodejs-tls-ma)を参照してください。

#### IP テーブル・ルールの設定 (オンプレミス宛先用)
{: #secure-app-answer-iptables}
オンプレミス宛先の Secure Gateway クラウド・ホストおよびポートは、パブリック・スペースにあります。したがって、デフォルトでは誰でもアクセスできます。
Secure Gateway 上でアクセスするトラフィックを制御するには、特定の範囲の IP およびポートによるアクセスのみを許可する IP テーブル・ルールを設定して、オンプレミス・リソースを保護します。 Secure Gateway の IP テーブル・ルールの構成方法について詳しくは、[IP テーブル・ルール](/docs/services/SecureGateway?topic=securegateway-add-dest#dest-network-security)を参照してください。

#### アクセス制御リストの構成 (オンプレミス宛先用)
{: #secure-app-answer-acl}
オンプレミス・リソースへのアクセスを許可または制限するアクセス制御リストの構成がサポートされることにより、特定の宛先ホストおよびポートに対するアクセス権限を指定することによって、オンプレミス宛先がよりセキュアになります。 オンプレミス宛先のセキュリティーを強化するために、許可または制限される HTTP/S 経路も ACL 項目に定義することをお勧めします。 詳しくは、[アクセス制御リスト](/docs/services/SecureGateway?topic=securegateway-acl#acl)および [ACL を使用した HTTP/S の経路制御](/docs/services/SecureGateway?topic=securegateway-acl#acl-route-control)を参照してください。

#### Secure Gateway クライアント UI のパスワードの設定
{: #secure-app-answer-ui-pw}
UI パスワードを設定して、Secure Gateway クライアント UI のアクセスを制限することをお勧めします。 開始構成を使用してパスワードを設定する方法、または Secure Gateway クライアント端末コマンド・ラインで対話式コマンドを使用してパスワードを設定する方法について詳しくは、[クライアントとの対話](/docs/services/SecureGateway?topic=securegateway-client-interacting#client-interacting)を参照してください。

## ゲートウェイ移行とは何ですか? 2018 年 12 月以降、ドメインが変更された理由は何ですか?
{: #faq-gateway-migration}

### 質問
{: #gateway-migration-question}
2018 年 12 月の保守の後、ゲートウェイ・パネルに移行ボタンが表示されるようになりましたが、このボタンの用途は何ですか? ドメインが変更された理由は何ですか?

### 答え
{: #gateway-migration-answer}

2018 年 12 月の保守の後、Secure Gateway のクラウド・ホストは、`integration.ibmcloud.com` の代わりに `securegateway.appdomain.cloud` を使用し、また、ゲートウェイ認証には `bluemix.net` でなく `securegateway.cloud.ibm.com` を使用するように名前変更されました。 後方互換性のために、既存のゲートウェイは、ゲートウェイが移行されるまで古いドメインを使用し続け、SG クライアント v180fp9 およびそれ以前のものは、ゲートウェイ認証に `bluemix.net` を使用し続けます。

移行後、オンプレミス宛先のクラウド・ホストは、新しいドメインを使用するように変更されます。ユーザー/アプリケーションは、新しいクラウド・ホストに要求を送信するように更新する必要があります。

現在、移行は必須ではなく、古いドメインがサポート対象から外される正確な日付も未定ですが、まだ古いドメイン・ネームをご使用のお客様には、日付が決まり次第、通知があります。

## 通知はどこで受け取ることができますか?
{: #faq-notification}

### 質問
{: #notification-question}
Secure Gateway の通知、特に、中断を伴う保守についての通知はどこで受け取ることができますか?

### 答え
{: #notification-answer}

通知は、[「状況」ページ](https://cloud.ibm.com/status?selected=status)から取得できます。
- 中断を伴う保守の完了および実行中についての通知を取得するには、`「状況」`タブで`「Secure Gateway」`を検索してください。
- 中断を伴う保守の計画についての通知を取得するには、`「計画保守」`タブで`「Secure Gateway」`を検索してください。

Secure Gateway クライアントが予期せず切断されたときは、「状況」ページにアクセスして、その時間に中断を伴う保守が行われていないかどうか確認してください。

保守に 10 分を超える中断が必要な場合は、保守後に Secure Gateway クライアントを手動で再始動し、Secure Gateway サーバーに再接続する必要が生じる場合もあります。通常、サービスのダウン時間は 10 分以下になり、Secure Gateway クライアント (バージョン v180 以降) は、Secure Gateway サーバーに自動的に再接続できるはずです。

## DataPower で Secure Gateway クライアント・ログをキャプチャーするには、どのようにすればよいですか?
{: #faq-dp-log}

### 質問
{: #dp-log-question}
DataPower で Secure Gateway クライアント・ログをキャプチャーし、それをファイルに書き込むには、どのようにすればよいですか?

### 答え
{: #dp-log-answer}

Secure Gateway クライアント・ログのイベント・カテゴリーは`「sgclient」`です。[ログ・ターゲット](https://www.ibm.com/support/knowledgecenter/en/SS9H2Y_7.7.0/com.ibm.dp.doc/logtarget_logs.html)を作成して、特定のイベント・カテゴリーのログをファイルに書き込むことができます。以下に例を示します。

- デフォルト・ドメインから以下を行います。
    - GUI サイド・パネルで、`「オブジェクト」` → `「ロギング構成 (Logging Configuration)」` → `「ログ・ターゲット (Log Target)」`を選択します。あるいは、`「検索」`フィールドで、`「ログ・ターゲット (Log Target)」`を検索します。
    - `「追加」`ボタンを選択して、ログ・ターゲットを追加します。
- `「メイン」`タブで、以下を行います。
    - `「名前」`を入力します。
    - `「ファイル」`の`「ターゲット・タイプ」`
    - `「テキスト」`の`「ログ形式 (Log format)」`
    - 出力場所を定義するには、`「ファイル名」`を入力します。例: `logtemp:///sgclient.log`
    - `「アーカイブ・モード (Archive Mode)」`に`「ローテーション (Rotate)」`を選択します。
- `「イベント・サブスクリプション (Event Subscription)」`タブで、以下を行います。
    - `「名前」`を入力します。
    - `「追加」`ボタンを選択して、ターゲット・イベント・サブスクリプションを追加します。
    - `「イベント・カテゴリー (Event Category)」`に`「sgclient」`を選択します。
    - `「最小イベント優先順位 (Minimum Event Priority)」`に`「デバッグ」`を入力します。
