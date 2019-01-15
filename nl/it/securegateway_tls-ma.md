---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Autenticazione reciproca TLS di Node.js

Questo esempio coprirà la modalità di configurazione dell'autenticazione reciproca (Mutual Authentication) per entrambi i lati di una destinazione in loco: autenticazione utente (User Authentication) e autenticazione risorsa (Resource Authentication).

So che la risorsa a cui voglio connettermi sarà ospitata sulla stessa macchina del client Secure Gateway e sarà in ascolto sulla porta 8999 e che richiede l'autenticazione reciproca per consentire una connessione. Con queste informazioni, posso iniziare a creare la mia destinazione.

![Autenticazione reciproca TLS iniziale](./images/tlsMA.png?raw=true "Autenticazione reciproca TLS iniziale")

## User Authentication
Sto creando una nuova applicazione e quindi non ho alcuna coppia certificato/chiave preesistente per la mia applicazione da utilizzare; lascerò quindi che i server Secure Gateway generino automaticamente una coppia per me. A tale fine, devo semplicemente lasciare vuoto il campo di caricamento User Authentication. Se avessi già una coppia che verrebbe utilizzata dalla mia applicazione, in questo campo caricherei invece il mio certificato.

## Resource Authentication

### On-Premises Authentication
Per consentire al client Secure Gateway di autenticare la risorsa a cui si sta connettendo, devo fornire al client il certificato (o la catena di certificati) che la risorsa presenterà.  La mia risorsa non ha una catena di certificati completa, quindi devo solo caricare il suo certificato nel campo On-Premises Authentication. Questo campo accetta fino a 6 file di certificato separati (.pem, .cer, .der, .crt).

### Client Cert and Key
Se devo specificare il modo in cui il client Secure Gateway si identificherà presso la mia risorsa, posso caricare un certificato e una chiave qui per l'utilizzo da parte del client. Poiché il Client e la risorsa sono in esecuzione sulla stessa macchina, posso lasciarli vuoti e lasciare che i server Secure Gateway generino automaticamente una coppia per me. Se la mia risorsa si trovasse su un host separato, dovrei [generare una coppia certificato/chiave da caricare](./securegateway_keygen.html).

![Autenticazione reciproca TLS locale](./images/localTLSma.png?raw=true "Autenticazione reciproca TLS locale")

Quando creo questa destinazione, i server Secure Gateway genereranno automaticamente una coppia certificato/chiave per l'utilizzo da parte della mia applicazione, nonché una coppia per l'utilizzo da parte del client Secure Gateway in fase di connessione alla mia risorsa locale. La destinazione avrà anche il certificato della mia risorsa locale da fornire al client Secure Gateway per l'utilizzo nella CA durante la connessione. Dopo la creazione, posso modificare la mia destinazione per visualizzare le seguenti informazioni:

![Dettagli dell'autenticazione reciproca TLS locale](./images/editLocalTLSma.png?raw=true "Dettagli dell'autenticazione reciproca TLS locale")

## Download dei file di sicurezza
Nel pannello Destination Info della mia destinazione, c'è un link per scaricare un file .zip che contiene tutti i certificati e tutte le chiavi associati alla mia destinazione:

![Pannello di informazioni sull'autenticazione reciproca](./images/infoPanelMA.png?raw=true "Pannello di informazioni sull'autenticazione reciproca")

Dopo aver scaricato lo .zip ed estratto i file, dispongo di quanto segue:

Nome file | Scopo
-- | --
secureGatewayCert.pem | Certificato primario del server Secure Gateway
DigiCertCA2.pem | Certificato intermedio del server Secure Gateway
DigiCertTrustedRoot.pem | Certificato root del server Secure Gateway
Nn5TJ34LyVQ_i2NOT_cert.pem | Certificato generato automaticamente per l'utilizzo da parte della mia applicazione in fase di connessione a host e porta cloud
Nn5TJ34LyVQ_i2NOT_key.pem | Chiave generata automaticamente per l'utilizzo da parte della mia applicazione in fase di connessione a host e porta cloud
Nn5TJ34LyVQ_i2NOT_destCert.pem | Certificato generato automaticamente per l'utilizzo da parte del client SG in fase di connessione alla destinazione
Nn5TJ34LyVQ_i2NOT_destKey.pem | Chiave generata automaticamente per l'utilizzo da parte del client SG in fase di connessione alla destinazione
Nn5TJ34LyVQ_clientCert.pem | Certificato generato automaticamente dal tuo gateway per l'utilizzo da parte del client SG nel caso in cui non abbia un file `_destCert` o `_destKey`.
localServerCert.pem | Il certificato della mia risorsa locale da utilizzare nella CA del client Secure Gateway

## Creazione dell'applicazione
Ora che ho il mio host e la mia porta cloud per la mia destinazione nonché i vari certificati e le varie chiavi, posso iniziare a scrivere la mia applicazione per la connessione a Secure Gateway. Questa è una semplice applicazione Node.js che si connetterà al mio server TLS locale, riceverà una piccola quantità di dati e chiuderà quindi la connessione.

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

## Creazione del server TLS
Ho il mio server TLS locale che è una semplice applicazione Node.js che sarà in ascolto per le connessioni, scriverà un messaggio in caso di connessione stabilita correttamente e chiuderà quindi tale connessione.

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

// Create the local TLS Server with the settings defined above
let server = tls.createServer(server_opts, function(socket) {
    socket.write('Successful connection message from the server');
    socket.end();
});

// Set up the various event listeners for the server and log their output
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

## Aggiorna l'ACL (Access Control List) del client
Prima di testare la mia applicazione, devo assicurarmi che l'ACL sul mio client sia configurato in modo appropriato. Ho aggiunto `localhost:8999` al mio ACL:

```
--------------------------------------------------------------------
           -- Secure Gateway Client Access Control List --

 Connections to unlisted rules are currently: denied

 hostname:port                path           value
 localhost:8999                            Allow
--------------------------------------------------------------------
```
{: screen}

## Esecuzione di un test della connessione
Ora che la mia applicazione, il mio server locale e il mio client sono stati tutti configurati, ottengo il seguente output dalla mia applicazione e dal mio client:

### Applicazione

```
Client connected, authorized: true
Client data: Successful connection message from the server
Client connection closing
```
{: screen}

### Client Secure Gateway

```
[2017-04-07 12:22:42.363] [INFO] (Client ID Nn5TJ34LyVQ_qCB) Connection #1 is being established to localhost:8999
[2017-04-07 12:22:42.381] [INFO] (Client ID Nn5TJ34LyVQ_qCB) Connection #1 to localhost:8999 was closed
```
{: screen}

## Operazione eseguita correttamente.
Abbiamo configurato correttamente l'autenticazione reciproca (Mutual Authentication) sia per l'autenticazione utente (User Authentication) che per l'autenticazione risorsa (Resource Authentication).
