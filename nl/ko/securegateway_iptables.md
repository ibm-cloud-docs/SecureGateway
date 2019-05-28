---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-09"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# IP 테이블 규칙
{: #iptables-rules}

대상에서 iptable 규칙을 적용하려면 대상의 네트워크 보안 패널에 있는 `네트워크 액세스 제한` 옵션이 선택되어 있어야 합니다.  해당 위치에서 적용하려는 규칙을 추가할 수 있습니다(예: 192.0.0.1 9000(단일 IP 및 포트), 192.0.0.1-192.0.0.5 5000:5005(IP 범위 및 포트 범위) 또는 해당 규칙의 조합). 자세한 정보는 [네트워크 보안 구성](/docs/services/SecureGateway?topic=securegateway-add-dest#dest-network-security)을 참조하십시오.

cURL로 사설 대상을 작성하는 경우 다음과 같이 명령행을 사용할 수 있습니다.

```
curl "https://sgmanager.us-south.securegateway.cloud.ibm.com/v1/sgconfig/<gateway_id>/destinations" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"desc":"My Private Destination","ip":"1.1.1.1","port":8000,"private":true}'
```
{: pre}

사설 대상이 작성되면 다음과 같이 명령행을 사용하여 IP 테이블 규칙을 추가할 수 있습니다.

```
curl -X PUT "https://sgmanager.us-south.securegateway.cloud.ibm.com/v1/sgconfig/<gateway_id>/destinations/<destination_id>/ipTableRule" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"src":"192.0.0.1","spt":"9000"}' -k
```
{: pre}

및

```
curl -X PUT "https://sgmanager.us-south.securegateway.cloud.ibm.com/v1/sgconfig/<gateway_id>/destinations/<destination_id>/ipTableRule" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"src_range":"192.0.0.1-192.0.0.5","spt":"5000:5005"}' -k
```
{: pre}

첫 번째 명령에서는 `src`를 사용하여 단일 IP를 제공하는 반면 두 번째 명령에서는 `src_range`를 사용하여 IP 범위를 제공한다는 점에 유의하십시오.

## 동적 IP에 대한 IP 테이블 규칙
{: #iptables-dynamic-ips}

애플리케이션에 동적 IP 세트가 있지만 해당 IP 세트를 모르는 경우 {{site.data.keyword.SecureGateway}}
REST API를 활용하여 즉시 IP 테이블 규칙을 업데이트할 수 있습니다.

예를 들어 다음의 짧은 NodeJS 프로그램은 복수의 인스턴스를 실행하는 Cloud Foundry 애플리케이션에 대한 IP 테이블 규칙을 업데이트합니다.

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
  uri: `https://sgmanager.us-south.securegateway.cloud.ibm.com/v1/sgconfig/$GATEWAY_ID/destinations/$DEST_ID/ipTableRule`
  headers: {
    'Authorization': `Bearer $SEC_TOKEN`
  }
  json: true, // Content-Type: application/json
  body: IP_TABLE_BODY
  }, console.log.bind(console)) 
```

이는 애플리케이션 시작 시 실행되어야 하며, 이전에 정의된 IP 테이블이 겹쳐쓰여집니다. 각각의 IP 테이블 규칙은
`application_id` 및 `CF_INSTANCE_INDEX`를 사용하여 각각의 애플리케이션 인스턴스를 고유하게 식별합니다. 외부 IP 주소는
`CF_INSTANCE_IP` 변수에서 검색되어 IP 테이블 규칙에 적용됩니다.


{: pre}
