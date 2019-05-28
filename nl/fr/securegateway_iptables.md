---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-09"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Règles de table d'IP
{: #iptables-rules}

Pour autoriser l'application de règles de table d'IP (iptables) à votre destination, l'option `Restreindre l'accès réseau` doit être sélectionnée dans le panneau Sécurité du réseau de votre destination.  Ensuite, vous pouvez ajouter les règles que vous voulez appliquer, par exemple 192.0.0.1 9000 (IP et port uniques), 192.0.0.1-192.0.0.5 5000:5005 (plage d'adresses IP et de ports) ou toute autre combinaison de ces règles. Pour plus d'informations, voir [Configuration de la sécurité du réseau](/docs/services/SecureGateway?topic=securegateway-add-dest#dest-network-security).

Si vous créez vos destinations privées avec cURL, vous pouvez utiliser une commande telle que :

```
curl "https://sgmanager.us-south.securegateway.cloud.ibm.com/v1/sgconfig/<gateway_id>/destinations" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"desc":"My Private Destination","ip":"1.1.1.1","port":8000,"private":true}'
```
{: pre}

Une fois votre destination privée créée, vous pouvez ajouter des règles de table d'IP avec des commandes telles que :

```
curl -X PUT "https://sgmanager.us-south.securegateway.cloud.ibm.com/v1/sgconfig/<gateway_id>/destinations/<destination_id>/ipTableRule" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"src":"192.0.0.1","spt":"9000"}' -k
```
{: pre}

et

```
curl -X PUT "https://sgmanager.us-south.securegateway.cloud.ibm.com/v1/sgconfig/<gateway_id>/destinations/<destination_id>/ipTableRule" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"src_range":"192.0.0.1-192.0.0.5","spt":"5000:5005"}' -k
```
{: pre}

Notez que la première commande utilise `src` pour fournir une unique adresse IP tandis que la seconde utilise `src_range` pour fournir une plage d'adresses IP.

## Règles de table d'IP pour des adresses IP dynamiques
{: #iptables-dynamic-ips}

Si votre application dispose d'un ensemble dynamique d'adresses IP mais que vous ne les connaissez pas, vous pouvez optimiser l'API REST {{site.data.keyword.SecureGateway}} afin de mettre à jour les règles de table d'IP à mesure des besoin.

Par exemple, ce court programme NodeJS mettra à jour des règles de table d'IP pour une application Cloud Foundry qui exécute plusieurs instances.

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

Cette action doit être exécutée au démarrage de l'application. Toutes les règles de table d'IP définies avant seront écrasées. Chaque règle de table d'IP identifie de manière unique chaque instance de l'application à l'aide de l'`ID_application` et de `CF_INSTANCE_INDEX`. L'adresse IP externe est extraite de la variable `CF_INSTANCE_IP` et appliquée à la règle de table d'IP.


{: pre}
