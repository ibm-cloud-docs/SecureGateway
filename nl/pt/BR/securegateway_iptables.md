---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Regras de tabela de IPs
{: #iptables-rules}

Para permitir a imposição das regras iptables em seu destino, deve-se ter a opção `Restringir acesso à rede` marcada no painel Segurança de rede do seu destino. Nesse ponto, é possível incluir
as regras que deseja impor, tais como: 192.0.0.1 9000 (IP único e porta), 192.0.0.1-192.0.0.5 5000:5005 (intervalo de IPs e intervalo de portas) ou qualquer combinação dessas regras. Veja [Configurando a segurança de rede](/docs/services/SecureGateway/securegateway_destination.html#dest-network-security) para obter mais informações.

Se estiver criando seus destinos privados com cURL, você poderá usar um comando como:

```
curl "https://sgmanager.ng.bluemix.net/v1/sgconfig/<gateway_id>/destinations" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"desc":"My Private Destination","ip":"1.1.1.1","port":8000,"private":true}'
```
{: pre}

Quando seu destino privado for criado, será possível incluir regras de tabela de IPs com comandos como:

```
curl -X PUT "https://sgmanager.ng.bluemix.net/v1/sgconfig/<gateway_id>/destinations/<destination_id>/ipTableRule" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"src":"192.0.0.1","spt":"9000"}' -k
```
{: pre}

e

```
curl -X PUT "https://sgmanager.ng.bluemix.net/v1/sgconfig/<gateway_id>/destinations/<destination_id>/ipTableRule" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"src_range":"192.0.0.1-192.0.0.5","spt":"5000:5005"}' -k
```
{: pre}

Observe que o primeiro comando usa `src` para fornecer um único IP, enquanto o segundo usa `src_range` para fornecer um intervalo de IPs.

## Regras de tabela de IPs para IPs dinâmicos
{: #iptables-dynamic-ips}

Se seu aplicativo tiver um conjunto dinâmico de IPs, mas você não os souber, será possível alavancar a API de REST do {{site.data.keyword.SecureGateway}}
para atualizar as regras de tabela de IPs instantaneamente.

Como um exemplo, esse programa NodeJS curto atualizará as regras de tabela de IPs para um aplicativo Cloud Foundry que executa múltiplas instâncias.

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

Isso deve ser executado ao iniciar o aplicativo, e qualquer regra de tabela de IP que tenha sido
definida antes será sobrescrita. Cada regra de tabela de IPs
identifica exclusivamente cada instância do aplicativo usando o `application_id` e `CF_INSTANCE_INDEX`. O endereço IP externo é recuperado da variável `CF_INSTANCE_IP`
e aplicado à regra de tabela de IPs.


{: pre}
