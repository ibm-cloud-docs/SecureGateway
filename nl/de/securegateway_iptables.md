---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# IP-Tabellenregeln

Damit die Regeln einer IP-Tabelle (iptable) für ein Ziel erzwungen werden, muss die Option `Netzzugriff beschränken` in der Anzeige 'Netzsicherheit' des Ziels ausgewählt sein. An diesem Punkt können Sie Regeln hinzufügen, die erzwungen werden sollen, zum Beispiel 192.0.0.1 9000 (einzelne IP und einzelner Port), 192.0.0.1-192.0.0.5 5000:5005 (IP-Bereich und Portbereich) oder eine Kombination solcher Angaben. Weitere Informationen finden Sie unter [Netzsicherheit konfigurieren](./securegateway_destination.html#configuring-network-security).

Wenn Sie private Ziele mit cURL erstellen, können Sie einen Befehl wie den folgenden verwenden:

```
curl "https://sgmanager.ng.bluemix.net/v1/sgconfig/<gateway_id>/destinations" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"desc":"My Private Destination","ip":"1.1.1.1","port":8000,"private":true}'
```
{: pre}

Sobald ein privates Ziel erstellt wurde, können Sie IP-Tabellenregeln mit Befehlen wie den folgenden hinzufügen:

```
curl -X PUT "https://sgmanager.ng.bluemix.net/v1/sgconfig/<gateway_id>/destinations/<destination_id>/ipTableRule" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"src":"192.0.0.1","spt":"9000"}' -k
```
{: pre}

und

```
curl -X PUT "https://sgmanager.ng.bluemix.net/v1/sgconfig/<gateway_id>/destinations/<destination_id>/ipTableRule" -H "Authorization: Bearer <security_token>" -H "Content-type: application/json" -d '{"src_range":"192.0.0.1-192.0.0.5","spt":"5000:5005"}' -k
```
{: pre}

Beachten Sie, dass vom ersten Befehl `src` zum Angeben einer einzelnen IP verwendet wird, während vom zweiten Befehl `src_range` zum Angeben eines IP-Bereichs verwendet wird.

## IP-Tabellenregeln für dynamische IPs

Wenn die Anwendung über dynamische IPs verfügt, Sie sie jedoch nicht kennen, können Sie die IP-Tabellenregeln während der Verarbeitung mithilfe der {{site.data.keyword.SecureGateway}}-REST-API aktualisieren.

Im folgenden Beispiel werden IP-Tabellenregeln von einem kurzen Node.js-Programm für eine Cloud Foundry-Anwendung aktualisiert, von der mehrere Instanzen ausgeführt werden.

```javascript
const request = require('request')

//Diese werden am besten mit Umgebungsvariablen konfiguriert.
const GATEWAY_ID = 'XXXXXX'//Ihre Gateway-ID
const DEST_ID = 'YYYYYY' // Die Ziel-ID, für die der Zugriff begrenzt werden soll.
const SEC_TOKEN = 'ZZZZZ' // Das Sicherheitstoken für das Gateway.

const APP_ID = JSON.parse(process.env.VCAP_APPLICATION).application_id
const IP_TABLE_BODY = {
  app: APP_ID + ':' + process.env.CF_INSTANCE_INDEX // Gibt eindeutig die App und Instanz für die IP-Tabellenregel an.
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

Dies muss beim Anwendungsstart ausgeführt werden; auf diese Art werden die IP-Tabellenregeln beim Anwendungsstart neu konfiguriert. Von jeder einzelnen IP-Tabellenregel wird eindeutig eine einzelne Instanz der Anwendung mithilfe der Angaben für `application_id` und `CF_INSTANCE_INDEX` ermittelt. Die vorangestellte IP-Adresse wird von der Variablen `CF_INSTANCE_IP` abgerufen und auf die IP-Tabellenregel angewendet.


{: pre}
