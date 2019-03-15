---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# IP 表规则
{: #iptables-rulles}

要允许在目标上强制实施 iptables 规则，必须在目标的“网络安全性”面板下选中`限制网络访问`选项。此时，可以添加要强制实施的规则，例如：192.0.0.1 9000（单个 IP 和端口）、192.0.0.1-192.0.0.5 5000:5005（IP 范围和端口范围），或者这些规则的任意组合。请参阅[配置网络安全性](/docs/services/SecureGateway/securegateway_destination.html#dest-network-security)以获取更多信息。

如果要使用 cURL 创建专用目标，那么可以使用类似下面的命令：

```
curl "https://sgmanager.ng.bluemix.net/v1/sgconfig/<gateway_id>/destinations" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"desc":"My Private Destination","ip":"1.1.1.1","port":8000,"private":true}'
```
{: pre}

创建了专用目标后，就可以使用类似下面的命令来添加 IP 表规则：

```
curl -X PUT "https://sgmanager.ng.bluemix.net/v1/sgconfig/<gateway_id>/destinations/<destination_id>/ipTableRule" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"src":"192.0.0.1","spt":"9000"}' -k
```
{: pre}

和

```
curl -X PUT "https://sgmanager.ng.bluemix.net/v1/sgconfig/<gateway_id>/destinations/<destination_id>/ipTableRule" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"src_range":"192.0.0.1-192.0.0.5","spt":"5000:5005"}' -k
```
{: pre}

请注意，第一个命令使用 `src` 来提供单个 IP，而第二个命令使用 `src_range` 来提供 IP 范围。

## 动态 IP 的 IP 表规则
{: #iptables-dynamic-ips}

如果应用程序具有一组动态 IP，但您不知道这些 IP，那么可以利用 {{site.data.keyword.SecureGateway}} REST API 来动态更新 IP 表规则。

例如，以下简短 NodeJS 程序将更新运行多个实例的 Cloud Foundry 应用程序的 IP 表规则。

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

应该在启动应用程序时运行，并且将覆盖之前定义的任何 IP 表规则。每个 IP 表规则使用 `application_id` 和 `CF_INSTANCE_INDEX` 来唯一标识应用程序的每个实例。外部 IP 地址将从 `CF_INSTANCE_IP` 变量中进行检索，并将其应用于 IP 表规则。


{: pre}
