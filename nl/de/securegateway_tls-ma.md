---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Gegenseitige TLS-Authentifizierung mit Node.js
{: #nodejs-tls-ma}

In diesem Beispiel wird die Konfiguration der gegenseitigen Authentifizierung für beide Seiten eines lokalen Ziels beschrieben: Die Benutzerauthentifizierung und die Ressourcenauthentifizierung.

Die Ressource, zu der eine Verbindung hergestellt werden soll, wird auf derselben Maschine wie der Secure Gateway-Client gehostet, Port 8999 wird überwacht und für eine Verbindung ist die gegenseitige Authentifizierung erforderlich.  Mit diesen Informationen kann die Erstellung des Ziels begonnen werden.

![Erstmalige gegenseitige TLS-Authentifizierung](./images/tlsMA.png?raw=true "Erstmalige gegenseitige TLS-Authentifizierung")

## Benutzerauthentifizierung
{: #tls-ma-user-auth}
Da eine absolut neue Anwendung erstellt wird, ist kein Zertifikats-/Schlüsselpaar vorhanden, das für die Anwendung verwendet werden kann; somit wird das Paar von den Secure Gateway-Servern automatisch generiert.  Hierzu muss das Feld zum Hochladen in der Benutzerauthentifizierung leer bleiben.  Falls bereits ein Paar vorhanden ist, das von der Anwendung verwendet wird, wird in diesem Feld stattdessen das jeweilige Zertifikat eingegeben.

## Ressourcenauthentifizierung
{: #tls-ma-resource-auth}

### Lokale Authentifizierung
{: #tls-ma-on-prem-auth}
Damit vom Secure Gateway-Client die Ressource authentifiziert werden kann, mit der er verbunden ist, muss dem Client das Zertifikat (oder die Zertifikatskette) bereitgestellt werde, das bzw. die von der Ressource für die Authentifizierung verwendet wird.  Da die Ressource nicht über eine vollständige Zertifikatskette verfügt, muss im Feld 'Lokale Authentifizierung' nur ihr Zertifikat hochgeladen werden.  In diesem Feld sind bis zu sechs separate Zertifikatsdateien zulässig (.pem, .cer, .der, .crt).

### Clientzertifikat und -schlüssel
{: #tls-ma-client-cert-and-key}
Falls angegeben werden muss, wie die Identifizierung des Secure Gateway-Clients an der Ressource durchgeführt werden soll, können zu diesem Zweck für den Client ein Zertifikat und ein Schlüssel hochgeladen werden.  Da sich der Client und die Ressource auf derselben Maschine befinden, ist diese nicht erforderlich, und von den Secure Gateway-Servern kann automatisch ein Paar generiert werden.  Wenn sich die Ressource auf einem anderen Host befindet, muss [ein Zertifikats-/Schlüsselpaar generiert und hochgeladen werden](/docs/services/SecureGateway/securegateway_keygen.html).

![Lokale gegenseitige TLS-Authentifizierung](./images/localTLSma.png?raw=true "Lokale gegenseitige TLS-Authentifizierung")

Wenn dieses Ziel erstellt wird, wird von den Secure Gateway-Servern automatisch ein Zertifikats-/Schlüsselpaar für die Anwendung und ein Paar für den Secure Gateway-Client für den Verbindungsaufbau zur lokalen Ressource erstellt.  Das Ziel verfügt außerdem über das Zertifikat der lokalen Ressource, das dem Secure Gateway-Client für die Verwendung in der Zertifizierungsstelle während der Verbindung bereitgestellt wird.  Nach der Erstellung kann das Ziel bearbeitet werden, sodass die folgenden Informationen angezeigt werden:

![Details der lokalen gegenseitigen TLS-Authentifizierung](./images/editLocalTLSma.png?raw=true "Details der lokalen gegenseitigen TLS-Authentifizierung")

## Sicherheitsdateien herunterladen
{: #tls-ma-download-files}
In der Anzeige der Zielinformationen des Ziels befindet sich ein Link zum Herunterladen einer komprimierten Datei, in der alle Zertifikate und Schlüssel enthalten sind, die dem Ziel zugeordnet sind:

![Informationsanzeige für gegenseitige Authentifizierung](./images/infoPanelMA.png?raw=true "Informationsanzeige für gegenseitige Authentifizierung")

Nach dem Herunterladen der komprimierten Datei und Extrahieren der Daten wird Folgendes angezeigt:

Dateiname | Verwendungszweck
-- | --
secureGatewayCert.pem | Primäres Zertifikat des Secure Gateway-Servers
DigiCertCA2.pem | Zwischenzertifikat des Secure Gateway-Servers
DigiCertTrustedRoot.pem | Stammzertifikat des Secure Gateway-Servers
Nn5TJ34LyVQ_i2NOT_cert.pem | Automatisch generiertes Zertifikat zur Verwendung durch die Anwendung beim Verbindungsaufbau zu Host und Port in der Cloud
Nn5TJ34LyVQ_i2NOT_key.pem | Automatisch generierter Schlüssel zur Verwendung durch die Anwendung beim Verbindungsaufbau zu Host und Port in der Cloud
Nn5TJ34LyVQ_i2NOT_destCert.pem | Automatisch generiertes Zertifikat für den SG-Client zur Verwendung beim Verbindungsaufbau zum Ziel
Nn5TJ34LyVQ_i2NOT_destKey.pem | Automatisch generierter Schlüssel für den SG-Client zur Verwendung beim Verbindungsaufbau zum Ziel
Nn5TJ34LyVQ_clientCert.pem | Automatisch generiertes Zertifikat vom Gateway für den SG-Client, das verwendet werden soll, falls keine `_destCert`- oder `_destKey`-Datei vorhanden ist.
localServerCert.pem | Das Zertifikat der lokalen Ressource, das in der Zertifizierungsstelle des Secure Gateway-Clients verwendet werden soll.

## Anwendung erstellen
{: #tls-ma-app-example}
Ein Cloud-Host und -Port für das Ziel sowie die verschiedenen Zertifikate und Schlüssel sind vorhanden; das Schreiben der Anwendung zum Verbinden zu Secure Gateway kann jetzt beginnen.  Hierbei handelt es sich um eine einfach Node.js-Anwendung, von der eine Verbindung zum lokalen TLS-Server aufgebaut wird, eine kleine Datenmenge empfangen wird und danach die Verbindung geschlossen wird.

```javascript
let fs = require('fs');
let tls = require('tls');

let client_opts = {
    cert : fs.readFileSync('Nn5TJ34LyVQ_i2NOT_cert.pem'),
    key : fs.readFileSync('Nn5TJ34LyVQ_i2NOT_key.pem'),
    host : 'cap-sg-prd-1.integration.ibmcloud.com',
    port : 17996,
    rejectUnauthorized : true
}

let so = tls.connect(client_opts, function() {
    console.log('Client connected, authorized:', so.authorized);
    if (!so.authorized) console.log('Client authorization error:', so.authorizationError)
});

so.on('data', function(data) {
    console.log('Client data:', data.toString())
});

so.on('error', function(err) {
    console.log('Client error',err);
});

so.on('close', function() {
    console.log('Client connection closing');
});
```
{: codeblock}

## TLS-Server erstellen
{: #tls-ma-server-example}
Der lokale TLS-Server ist eine einfache Node.js-Anwendung, die für Verbindungen empfangsbereit ist, eine Nachricht bei einer erfolgreichen Verbindung schreibt und diese Verbindung anschließend schließt.

```javascript
let fs = require('fs');
let tls = require('tls');

let server_opts = {
    cert : fs.readFileSync('localServerCert.pem'),
    key : fs.readFileSync('localServerKey.pem'),
    // pfx : fs.readFileSync(''),
    // passphrase : '',
    ca : [fs.readFileSync('Nn5TJ34LyVQ_i2NOT_destCert.pem')],
    rejectUnauthorized : true,
    requestCert : true
}

// Lokalen TLS-Server mit oben definierten Einstellungen erstellen
let server = tls.createServer(server_opts, function(socket) {
    socket.write('Successful connection message from the server');
    socket.end();
});

// Verschiedene Ereignislistener für Server konfigurieren und ihre Ausgabe protokollieren
server.on('error', function(err) {
    console.log('Server error', err);
});

server.on('close', function() {
    console.log('Server closing');
});

server.on('tlsClientError', function(err) {
    console.log('Server tlsClientError', err);
});

server.listen(8999);
```
{: codeblock}

## Zugriffssteuerungsliste des Clients aktualisieren
{: #tls-ma-acl}
Vor dem Testen der Anwendung muss sichergestellt werden, dass die Zugriffssteuerungsliste ordnungsgemäß konfiguriert ist.  Zur Zugriffssteuerungsliste wurde `localhost:8999` hinzugefügt:

```
--------------------------------------------------------------------
           -- Secure Gateway Client Access Control List --

 Connections to unlisted rules are currently: denied

 hostname:port                path           value
 localhost:8999                            Allow
--------------------------------------------------------------------
```
{: screen}

## Verbindung testen
{: #tls-ma-testing}
Die Anwendung, der lokale Server und der Client wurden konfiguriert; die Ausgabe der Anwendung und des Clients lautet wie folgt:

### Anwendung
{: #tls-ma-testing-app}

```
Client connected, authorized: true
Client data: Successful connection message from the server
Client connection closing
```
{: screen}

### Secure Gateway-Client
{: #tls-ma-testing-client}

```
[2017-04-07 12:22:42.363] [INFO] (Client ID Nn5TJ34LyVQ_qCB) Connection #1 is being established to localhost:8999
[2017-04-07 12:22:42.381] [INFO] (Client ID Nn5TJ34LyVQ_qCB) Connection #1 to localhost:8999 was closed
```
{: screen}

## Erfolg!
{: #tls-ma-testing-result}
Die gegenseitige Authentifizierung wurde sowohl für die Benutzerauthentifizierung als auch für Ressourcenauthentifizierung erfolgreich konfiguriert.
