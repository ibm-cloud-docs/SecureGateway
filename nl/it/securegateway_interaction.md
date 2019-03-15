---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-19"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Interazione con il client
{: #client-interacting}

Ci sono diversi modi per interagire con il client:

- Tramite la riga di comando del terminale prima dell'avvio
- Dopo l'avvio, mediante la riga di comando interattiva del client
- Dopo l'avvio, utilizzando l'IU locale del client

## Configurazioni di avvio
{: #startup}

### Argomenti e opzioni di avvio
{: #startup-args}

La seguente tabella descrive tutte le opzioni disponibili che possono essere fornite insieme al comando di avvio per il client.  Il loro utilizzo consentirà la configurazione completa del client prima dell'avvio piuttosto che richiedere la singola impostazione una volta che il client è in esecuzione.

| Parametro e arguenti | Descrizione |
| ------------- | ----------- |
| &lt;gateway ID&gt; | Stabilisci una connessione a {{site.data.keyword.Bluemix_notm}} utilizzando
l'ID gateway fornito |
| -F, -\-aclfile &lt;file&gt; | Access control List file |
| -g, -\-gateway &lt;hostname:port&gt; | Used to manually select a specific gateway destination (advanced use only) |
| -l, -\-loglevel &lt;level&gt; | Change the log level to ERROR, INFO, DEBUG or TRACE |
| -p, -\-logpath &lt;file&gt; | Direct logging to a specific file |
| -t, -\-sectoken &lt;security token&gt; | The security token to use for this gateway connection |
| -P, -\-port &lt;port&gt; | The port for the UI to run on.  Defaults to port 9003 |
| -w, -\-password &lt;password&gt; | The password to protect the UI with.  Defaults to no password |
| -x, -\-proxy &lt;proxy agent&gt; | The proxy for the port 9000 connection |
| -\-noUI | Prevent the UI from starting up automatically |
| -\-allow | Allows all connections to the client. Is overridden by the ACL file, if provided |
| -\-service | After an initial connection, the parent will restart within 60s if all child clients are terminated |

<b>Nota:</b> gli indicatori `--service`, `--allow` e `--noUI` devono essere gli ultimi parametri negli argomenti della riga di comando.

Il caso d'uso più di base consiste nell'iniziare con una connessione client singola con le impostazioni predefinite:

```
<gateway_id> -t <security_token>
```
{: pre}

Queste opzioni possono anche essere estese per supportare la connessione automatica di più client.  Per eseguirne la trasmissione a più connessioni client, è necessario farlo utilizzando uno specifico formato.  Gli ID gateway possono essere trasmessi nello stesso modo utilizzato per la connessione a gateway singolo (con ciascun ID gateway separato da uno spazio); tuttavia, l'ordine di questi ID determina
l'ordine che deve seguire il resto degli argomenti.  Quando trasmetti uno qualsiasi degli altri argomenti, essi devono
essere separati da `--` affinché vengano rilevati correttamente.  Se non viene trasmesso alcun argomento per un determinato indicatore, si presume che non si applichi a nessuno dei client.

Se non vengono forniti argomenti sufficienti per soddisfare tutti gli ID gateway, questi verranno applicati in ordine finché non ci saranno più argomenti.  Ad esempio, se vengono trasmessi due ID gateway e un singolo token di sicurezza, il token verrà applicato al primo ID gateway e non al secondo.  

Se vengono forniti ID gateway che richiedono argomenti differenti, è necessario designare la
parola chiave `none` al posto di qualsiasi specifico argomento che un gateway non stia implementando/fornendo.  Ad esempio, se vengono trasmessi tre ID gateway e l'utente vuole specificare
un livello di log (loglevel) per il primo e il terzo, l'argomento sarà simile a `--loglevel DEBUG--none--TRACE`.  In questo caso, none assumerà quindi per impostazione predefinita un valore di INFO.

### Esempi di argomento di avvio
{: #startup-examples}

Connessione client singola, livello di log (loglevel) personalizzato, nessuna IU:

```
node lib/secgwclient.js <gateway_id> -t <security_token> -l <loglevel> --noUI
```
{: pre}

Connessioni client multiple, opzioni predefinite:

```
node lib/secgwclient.js <gateway_id_1> <gateway_id_2> -t <security_token_1>--<security_token_2>
```
{: pre}

Connessioni client multiple, tutte le impostazioni personalizzate:

```
node lib/secgwclient.js <myGatewayID_1> <myGatewayID_2> -t none--<token for gateway 2> -l DEBUG--TRACE -p <full path to log file for gateway 1>--<full path to log file for gateway 2> -F <full path to ACL file for gateway 1>
```
{: pre}

Ritorna a [Introduzione - Aggiunta di un client](/docs/services/SecureGateway/securegateway_client.html).

## Configurazione interattiva
{: #interactive}

### Comandi interattivi
{: #interactive-commands}

Il client {{site.data.keyword.SecureGateway}} dispone di
un'interfaccia riga di comando (CLI) e un prompt di shell per facilitarne la configurazione e il controllo. L'ambiente interattivo
supporta un insieme più ricco di funzionalità rispetto agli argomenti della riga di comando al fine di facilitare
un migliore controllo interattivo sul client.

| Comandi interattivi | Descrizione       |
| ------------- | ----------- |
| A, acl allow &lt;nomehost:porta&gt; &lt;ID nodo di lavoro&gt; | Consente l'ACL (Access Control List) |
| D, acl deny &lt;nomehost:porta&gt; &lt;ID nodo di lavoro&gt; | Rifiuta l'ACL (Access Control List) |
| N, no acl &lt;nomehost:porta&gt; &lt;ID nodo di lavoro&gt; | Rimuove un inserimento dell'ACL (Access Control List) |
| S, show acl &lt;ID nodo di lavoro&gt; | Mostra tutti gli inserimenti dell'ACL (Access Control List) |
| F, acl file &lt;file&gt; &lt;ID nodo di lavoro&gt; | File ACL (Access Control List) |
| C, displayconfig &lt;ID nodo di lavoro&gt; | Visualizza la configurazione {{site.data.keyword.SecureGateway}} corrente,
se disponibile |
| a, authorize &lt;ID nodo di lavoro&gt; | Attiva/disattiva la sovrascrittura del parametro rejectUnauthorized per le connessioni TLS in uscita per il nodo di lavoro specificato |
| t, sectoken &lt;token sicurezza&gt; | Il token di sicurezza da utilizzare per la connessione gateway successiva |
| c, connect &lt;ID gateway&gt; | Stabilisci una connessione a {{site.data.keyword.Bluemix_notm}} utilizzando
l'ID gateway fornito |
| l, loglevel &lt;livello&gt; &lt;ID nodo di lavoro&gt; | Modifica il livello di log in ERROR, INFO, DEBUG o TRACE |
| p, logpath &lt;file&gt; &lt;ID nodo di lavoro&gt; | Indirizza la registrazione a uno specifico file |
| r, reverse &lt;ID nodo di lavoro&gt; | Elenca le porte su cui il client è in ascolto per le destinazioni inverse |
| k, kill &lt;ID nodo di lavoro&gt;  | Termina il nodo di lavoro specificato |
| e, select &lt;ID nodo di lavoro&gt;  | Specifica un nodo di lavoro su cui eseguire comandi, se non specificato altrimenti |
| d, deselect | Deseleziona il nodo di lavoro precedentemente specificato.  Immetti il comando di selezione per specificarne
un'altra |
| w, password &lt;vecchia password&gt; &lt;nuova password&gt; | Imposta la password dell'IU.  Se il valore &lt;nuova password&gt; è vuoto, non verrà implementata alcuna password. &lt;vecchia password&gt; è un valore richiesto quando si aggiorna la password. Le password devono contenere solo lettere |
| P, port &lt;nuova porta&gt; | Modifica la porta su cui è in ascolto l'IU |
| u, uistart &lt;password iniziale&gt; &lt;porta&gt; | Avvia l'IU su localhost:&lt;porta&gt;/dashboard. Se il valore &lt;password iniziale&gt; è vuoto e non sono state impostate altre password per
la sessione, non verrà implementata alcuna password dell'IU. Se il valore &lt;porta&gt; è vuoto, l'IU sarà raggiungibile su 9003 |
| U, uistop | Chiude l'IU associata a questa sessione del client. La sessione sarà accessibile solo tramite la CLI fino a quando non verrà avviata manualmente una nuova IU |
| R, revoke | Cancella tutte le autorizzazioni IU associate a questa sessione del client |
| q, quit | Disconnette e chiude |
| s, status &lt;ID nodo di lavoro&gt;  | Stampa i dettagli di stato del tunnel e delle rispettive
connessioni |
| L, list | Visualizza un elenco dei nodi di lavoro attualmente associati. |

<b>Nota:</b> se una connessione è stata specificata con il comando `select` e viene richiamato un altro comando senza fornire un ID nodo di lavoro, il comando tenterà l'esecuzione sulla connessione specificata da `select`.

Per ulteriori dettagli sulla configurazione dell'ACL (Access Control List), [fai clic qui](/docs/services/SecureGateway/securegateway_acl.html).

Ritorna a [Introduzione - Aggiunta di un client](/docs/services/SecureGateway/securegateway_client.html).

## IU client
{: #client-ui}

<b>Nota:</b> L'IU client non è supportata quando si utilizza Docker su Windows o MacOS.

L'IU client fornisce un'interfaccia web locale che consente all'utente di interagire con il client {{site.data.keyword.SecureGateway}} anziché con la CLI.  Per impostazione predefinita, questa IU è disponibile in `localhost:9003/dashboard` L'IU è suddivisa nelle seguenti pagine:

### Connessione
{: #ui-connect}

Questa è la pagina di destinazione iniziale per l'IU in cui un utente può fornire un ID gateway e un token di sicurezza per connettere il suo primo client.

### Accesso
{: #ui-login}

Questa pagina verrà visualizzata se l'IU è stata protetta da una password.  Se raggiungi questa pagina mentre non è applicata alcuna password, aggiorna la pagina per essere reindirizzato.

### Dashboard
{: #ui-dashboard}

Questa è la pagina principale dopo che un client è stato connesso.  Da qui, puoi accedere alla pagina Visualizza log, alla pagina ACL (Access Control List) e alla pagina Informazioni sulla connessione.  Nella parte inferiore, puoi anche scegliere di disconnettere uno o più client connessi.  Nella parte superiore della pagina, sarà visualizzato il client attualmente selezionato e sarà presente un'opzione per connettere client aggiuntivi. 

### Visualizza log
{: #ui-logs}

Questa pagina ti consentirà di visualizzare i log generati dal client selezionato (visualizzato nella parte superiore destra della pagina).  I log visualizzati possono essere filtrati mediante le caselle di spunta poste sotto i log.

### ACL (Access Control List)
{: #ui-acl}

Questa pagina ti consentirà di gestire l'ACL (Access Control List) per il client selezionato (visualizzato nell'angolo superiore destro della pagina).  È possibile aggiungere singolarmente le regole alle tabelle allow/deny oppure caricare un file nella parte inferiore della pagina.

### Informazioni sulla connessione
{: #ui-info}

Questa pagina visualizzerà le informazioni sulla connessione corrente per il client selezionato (visualizzato nella parte superiore destra della pagina).  Qui possono essere visualizzate informazioni come la descrizione del gateway, il numero di connessioni
correnti e i listener di destinazione inversa.

Ritorna a [Introduzione - Aggiunta di un client](/docs/services/SecureGateway/securegateway_client.html).

## Terminazione del client remoto
{: #client-remote}

Se a un client è stato fornito un ID, può essere terminato in remoto mediante l'IU SG o tramite l'API SG.  Se termini un client che è in esecuzione come un servizio, il client verrà riavviato e otterrà un nuovo ID client; tuttavia, se al servizio sono connessi più client, il client terminato non verrà riavviato finché tutti i client rimanenti non saranno stati terminati.

## Limitazioni
{: #limits}

### Limitazioni di connessione
{: #limits-conn}

Il gateway SG può gestire solo 250 connessioni simultanee. Se il numero di richieste simultanee supera il limite, è possibile che i tentativi di connessione vengano rifiutati e potrebbe verificarsi una latenza. Un modo facile per correggere questo problema consiste nell'utilizzare un pooling di connessioni sull'applicazione chiamante. Nota: il limite di 250 connessioni simultanee è sul gateway, non sul client o sulla destinazione. Questo limite verrà condiviso tra tutti i client e tutte le destinazioni sul gateway.

### Limitazioni del client DataPower
{: #limits-datapower}

Il client {{site.data.keyword.SecureGateway}} DataPower è in fase di aggiornamento in modo che corrisponda alle funzionalità del resto dei nostri client.  Attualmente presenta le seguenti limitazioni:

- L'ACL verrà impostata automaticamente su ALLOW ALL
- L'abilitazione e la disabilitazione di gateway o destinazioni dall'IU {{site.data.keyword.SecureGateway}} non è supportata. Tuttavia, l'opzione Administrative State nell'IU DataPower funziona come un interruttore di attivazione/disattivazione (on/off) per tale specifico client.
- Il polling dello stato della connessione per gli aggiornamento di stato del gateway connesso e disconnesso in tempo reale non è supportato.
- Le catene di certificati complete con la TLS lato destinazione non sono supportate prima di DataPower versione 7.5.1.0
- Le destinazioni cloud non sono supportate prima di DataPower versione 7.5.1.0
- Il livello di log non può essere modificato al livello TRACE
- L'ultima versione client di Secure Gateway Client in DataPower è 1.8.0fp6, consulta [qui](/docs/services/SecureGateway/securegateway_install.html#installing-datapower) per ulteriori informazioni
