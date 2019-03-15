---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Regole tabella IP
{: #iptables-rulles}

Per consentire l'implementazione di regole iptables sulla tua destinazione, è necessario che l'opzione `Restrict network access` sia selezionata nel pannello Network Security della tua destinazione. A questo punto, puoi aggiungere le regole che vuoi che vengano implementate, come ad esempio: 192.0.0.1 9000 (IP e porta singoli),  192.0.0.1-192.0.0.5 5000:5005 (intervallo di IP e intervallo di porte) oppure una qualsiasi combinazione di queste regole. Per ulteriori informazioni, vedi [Configurazione della sicurezza di rete](/docs/services/SecureGateway/securegateway_destination.html#dest-network-security).

Se stai creando le tue destinazioni private con cURL, puoi utilizzare un comando come:

```
curl "https://sgmanager.ng.bluemix.net/v1/sgconfig/<gateway_id>/destinations" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"desc":"My Private Destination","ip":"1.1.1.1","port":8000,"private":true}'
```
{: pre}

Dopo che la tua destinazione privata è stata creata, puoi aggiungere delle regole di tabella IP con comandi come:

```
curl -X PUT "https://sgmanager.ng.bluemix.net/v1/sgconfig/<gateway_id>/destinations/<destination_id>/ipTableRule" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"src":"192.0.0.1","spt":"9000"}' -k
```
{: pre}

e

```
curl -X PUT "https://sgmanager.ng.bluemix.net/v1/sgconfig/<gateway_id>/destinations/<destination_id>/ipTableRule" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"src_range":"192.0.0.1-192.0.0.5","spt":"5000:5005"}' -k
```
{: pre}

Nota: il primo comando usa `src` per fornire un singolo IP mentre il secondo usa `src_range` per fornire un intervallo di IP.

## Regole di tabella IP per gli IP dinamici
{: #iptables-dynamic-ips}

Se la tua applicazione ha un set dinamico di IP, ma tu non li conosci, puoi avvalerti dell'API REST {{site.data.keyword.SecureGateway}} per aggiornare le regole di tabella IP senza causare alcuna interruzione.

Ad esempio, questo breve programma NodeJS aggiornerà le regole di tabella IP per un'applicazione Cloud Foundry che segue più istanze.

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

Questo dovrebbe essere eseguito quando avvii l'applicazione, tutte le regole di tabella IP che sono state prima definite saranno sovrascritte. Ogni regola di tabella IP identifica in modo univoco ciascuna istanza dell'applicazione utilizzando `application_id` e `CF_INSTANCE_INDEX`. L'indirizzo IP esterno
viene richiamato dalla variabile `CF_INSTANCE_IP` e applicato alla regola di tabella IP.


{: pre}
