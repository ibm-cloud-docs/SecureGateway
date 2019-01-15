---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Règles de table d'IP

Pour autoriser l'application de règles de table d'IP (iptable) à votre destination, l'option `Restreindre l'accès réseau` doit être sélectionnée dans le panneau Sécurité du réseau de votre destination.  Ensuite, vous pouvez ajouter les règles que vous voulez appliquer, telles que 192.0.0.1 9000 (IP et port uniques),  192.0.0.1-192.0.0.5 5000:5005 (plage d'adresses IP et plage de ports) ou toute combinaison de celles-ci. Pour plus d'informations, voir [Configuration de la sécurité du réseau](./securegateway_destination.html#configuring-network-security).

Si vous créez vos destinations privées avec cURL, vous pouvez utiliser une commande telle que :

```
curl "https://sgmanager.ng.bluemix.net/v1/sgconfig/<ID_passerelle>/destinations" -H "Authorization: Bearer <jeton_sécurité>" -H "Content-type: application/json" -d '{"desc":"Ma destination privée","ip":"1.1.1.1","port":8000,"private":true}'
```
{: pre}

Une fois votre destination privée créée, vous pouvez ajouter des règles de table d'IP avec des commandes telles que :

```
curl -X PUT "https://sgmanager.ng.bluemix.net/v1/sgconfig/<ID_passerelle>/destinations/<ID_destination>/ipTableRule" -H "Authorization: Bearer <jeton_sécurité>" -H "Content-type: application/json" -d '{"src":"192.0.0.1","spt":"9000"}' -k
```
{: pre}

et

```
curl -X PUT "https://sgmanager.ng.bluemix.net/v1/sgconfig/<ID_passerelle>/destinations/<ID_destination>/ipTableRule" -H "Authorization: Bearer <jeton_sécurité>" -H "Content-type: application/json" -d '{"src_range":"192.0.0.1-192.0.0.5","spt":"5000:5005"}' -k
```
{: pre}

Notez que la première commande utilise `src` pour fournir une unique adresse IP tandis que la seconde utilise `src_range` pour fournir une plage d'adresses IP.

## Règles de table d'IP pour des adresses IP dynamiques

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
  uri: `https://sgmanager.ng.bluemix.net/v1/sgconfig/$GATEWAY_ID/destinations/$DEST_ID/ipTableRule`
  headers: {
    'Authorization': `Bearer $SEC_TOKEN`
  }
  json: true, // Content-Type: application/json
  body: IP_TABLE_BODY
  }, console.log.bind(console)) 
```

Ce programme doit s'exécuter au démarrage de l'application et reconfigurera alors la règle de table d'IP lors de ce démarrage. Chaque règle de table d'IP identifie de manière unique chaque instance de l'application à l'aide de l'`ID_application` et de `CF_INSTANCE_INDEX`. La première adresse IP est extraite de la variable `CF_INSTANCE_IP` et appliquée à la règle de table d'IP.


{: pre}
