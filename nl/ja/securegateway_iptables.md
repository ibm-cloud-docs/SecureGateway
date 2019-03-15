---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# IP テーブル・ルール
{: #iptables-rulles}

宛先に IP テーブル・ルールを適用できるようにするには、宛先の「ネットワーク・セキュリティー」パネルの`「ネットワーク・アクセスの制限」`オプションにチェック・マークが付いている必要があります。その時点で、適用するルールを追加できます。例えば、192.0.0.1 9000 (単一の IP およびポート)、192.0.0.1-192.0.0.5 5000:5005 (IP の範囲とポートの範囲)、またはこれらのルールの組み合わせです。詳しくは、[ネットワーク・セキュリティーの構成](/docs/services/SecureGateway/securegateway_destination.html#dest-network-security)を参照してください。

cURL を使用してプライベート宛先を作成する場合は、以下のようなコマンドを使用できます。

```
curl "https://sgmanager.ng.bluemix.net/v1/sgconfig/<gateway_id>/destinations" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"desc":"My Private Destination","ip":"1.1.1.1","port":8000,"private":true}'
```
{: pre}

プライベート宛先が作成されたら、以下のようなコマンドを使用して IP テーブル・ルールを追加できます。

```
curl -X PUT "https://sgmanager.ng.bluemix.net/v1/sgconfig/<gateway_id>/destinations/<destination_id>/ipTableRule" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"src":"192.0.0.1","spt":"9000"}' -k
```
{: pre}

および

```
curl -X PUT "https://sgmanager.ng.bluemix.net/v1/sgconfig/<gateway_id>/destinations/<destination_id>/ipTableRule" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"src_range":"192.0.0.1-192.0.0.5","spt":"5000:5005"}' -k
```
{: pre}

最初のコマンドは `src` を使用して単一の IP を指定しており、2 番目のコマンドは `src_range` を使用して IP の範囲を指定していることに注意してください。

## 動的 IP を指定する IP テーブル・ルール
{: #iptables-dynamic-ips}

アプリケーションに IP の動的なセットがあるが、それらが分からない場合は、{{site.data.keyword.SecureGateway}} REST API を利用して IP テーブル・ルールをその場で更新できます。

例えば、次の短い NodeJS プログラムは、複数のインスタンスを実行する Cloud Foundry アプリケーション用の IP テーブル・ルールを更新します。

```javascript
const request = require('request')

//These are best configured using environment variables.
const GATEWAY_ID = 'XXXXXX'//Your Gateway ID
const DEST_ID = 'YYYYYY' // The Destination ID to restrict access to.
const SEC_TOKEN = 'ZZZZZ' // The Security Token for the Gateway.

const APP_ID = JSON.parse(process.env.VCAP_APPLICATION).application_id
const IP_TABLE_BODY = {
  app: APP_ID + ':' + process.env.CF_INSTANCE_INDEX //uniquely identifies the app and instance for ip table rule.
  src: process.env.CF_INSTANCE_IP 
 }
 
request({
  method: 'PUT',
  uri: `https://sgmanager.ng.bluemix.net/v1/sgconfig/$GATEWAY_ID/destinations/$DEST_ID/ipTableRule`
  headers: {
    'Authorization': `Bearer $SEC_TOKEN`
  }
  json: true, // Content-Type: application/json
  body: IP_TABLE_BODY
  }, console.log.bind(console)) 
```

これは、アプリケーションの開始時に実行する必要があります。それより前に定義された IP テーブル・ルールは上書きされます。各 IP テーブル・ルールは、`application_id` および `CF_INSTANCE_INDEX` を使用して、アプリケーションの各インスタンスを一意的に識別します。 外部 IP アドレスが `CF_INSTANCE_IP` 変数から取り出され、IP テーブル・ルールに適用されます。


{: pre}
