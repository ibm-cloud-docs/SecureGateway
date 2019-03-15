---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# IP 表格規則
{: #iptables-rulles}

若要容許在目的地上強制執行 iptables 規則，您必須在目的地的「網路安全」畫面下勾選`限制網路存取`選項。此時，您可以新增要強制執行的規則，例如：192.0.0.1 9000（單一 IP 及埠）、192.0.0.1-192.0.0.5 5000:5005（某範圍的 IP 及某範圍的埠）或這些規則的任意組合。如需相關資訊，請參閱[配置網路安全](/docs/services/SecureGateway/securegateway_destination.html#dest-network-security)。

如果您要使用 cURL 建立專用目的地，則可以使用類似如下的指令：

```
curl "https://sgmanager.ng.bluemix.net/v1/sgconfig/<gateway_id>/destinations" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"desc":"My Private Destination","ip":"1.1.1.1","port":8000,"private":true}'
```
{: pre}

建立專用目的地之後，您可以使用類似如下的指令來新增 IP 表格規則：

```
curl -X PUT "https://sgmanager.ng.bluemix.net/v1/sgconfig/<gateway_id>/destinations/<destination_id>/ipTableRule" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"src":"192.0.0.1","spt":"9000"}' -k
```
{: pre}

以及

```
curl -X PUT "https://sgmanager.ng.bluemix.net/v1/sgconfig/<gateway_id>/destinations/<destination_id>/ipTableRule" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"src_range":"192.0.0.1-192.0.0.5","spt":"5000:5005"}' -k
```
{: pre}

請注意，第一個指令使用 `src` 來提供單一 IP，而第二個指令使用 `src_range` 來提供某範圍的 IP。

## 動態 IP 的 IP 表格規則
{: #iptables-dynamic-ips}

如果您的應用程式有一組動態 IP，但您不知道，則可以運用 {{site.data.keyword.SecureGateway}} REST API 即時更新 IP 表格規則。

舉例來說，這個簡短的 NodeJS 程式將更新執行多個實例之 Cloud Foundry 應用程式的 IP 表格規則。

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

這應該在啟動應用程式時執行，先前定義的任何 iptables 規則都將改寫。每個 iptables 規則都可以使用 `application_id` 及 `CF_INSTANCE_INDEX` 來唯一地識別每一個應用程式實例。外部 IP 位址擷取自 `CF_INSTANCE_IP` 變數，並套用至 iptables 規則。


{: pre}
