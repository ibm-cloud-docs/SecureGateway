---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-08"

---

# Domande frequenti (FAQ - Frequently Asked Questions)

## Livello modello OSI
{: #osi}

### Domanda
Quale livello del modello OSI rappresenta il servizio Secure Gateway?

### Risposta
Il servizio Secure Gateway rappresenta il livello 4 del modello OSI.

## Supporto versione TLS
{: #tls}

### Domanda
Quale versione di TLS supporta il servizio Secure Gateway?

### Risposta
Il servizio Secure Gateway supporta TLS versione 1.2.

## Perché vorrei disabilitare il mio gateway o la mia destinazione?
{: #disabled}

### Domanda

Perché vorresti disabilitare un gateway o una destinazione?

### Risposta
Potresti voler disabilitare una destinazione o gateway per uno dei seguenti motivi:

- Crei una destinazione ma non hai impostato la sicurezza.  In questo caso, potresti disabilitare
la destinazione finché la sicurezza non sarà impostata.
- Non vuoi che il servizio sia disponibile agli utenti perché stai apportando alcuni aggiornamenti al
servizio.  In questo caso, potresti disabilitare temporaneamente i gateway necessari e attendere che il servizio venga aggiornato.
- Hai configurato tutti i tuoi gateway e tutte le tue destinazioni sul front-end, ma il tuo backend è ancora in fase di creazione. In questo caso, potresti disabilitare i gateway o le destinazioni finché la creazione del backend non
sarà completata.

Per ulteriori informazioni sulla disabilitazione di un gateway o di una destinazione, vedi il documento relativo alla [modalità di gestione della tua istanza del servizio Secure Gateway](/docs/services/SecureGateway/securegateway_managing.html).

## Qual è l'approccio consigliato all'automazione della creazione su più spazi?
{: #automation-spaces}

### Domanda
Un ambiente del cliente ha un'organizzazione e tre spazi. Uno spazio è per lo sviluppo, un altro per la preparazione e l'ultimo per la produzione. Il cliente deve creare una singola istanza Secure Gateway o più di una (ad es. una per ogni spazio)? Se il cliente può creare più gateway, ci sono delle considerazioni da fare per il riutilizzo di un'applicazione Node.js per creare un gateway e una destinazione in ogni spazio?

### Risposta

- Puoi creare una singola istanza Secure Gateway per tutti e tre gli spazi. Devi tuttavia ricordarti delle [limitazioni per il tuo specifico piano](/docs/services/SecureGateway/securegateway_plans.html) relative a gateway e destinazione.
- Non ci sono ulteriori considerazioni per il riutilizzo di un'applicazione Node.js poiché Secure Gateway non richiede alcun bind di servizio.


## Qual è l'approccio consigliato all'automazione della creazione in più organizzazioni?
{: #automation-orgs}

### Domanda
Un ambiente del cliente ha tre organizzazioni: una per lo sviluppo, una per la preparazione e una per la produzione. È richiesta un'istanza del servizio Secure Gateway per ciascuna organizzazione e la configurazione è disponibile per tutti gli spazi all'interno di tale organizzazione?

### Risposta

- Non è obbligatorio che tu abbia un'istanza del servizio Secure Gateway in ciascuna organizzazione. Puoi avere un'istanza in un'organizzazione e utilizzare i gateway all'interno di tale istanza da tutti gli altri tuoi ambienti. Con questa configurazione, devi ricordarti delle [limitazioni per il tuo specifico piano](/docs/services/SecureGateway/securegateway_plans.html) relative a gateway e destinazione.
- Puoi avere un'istanza del servizio Secure Gateway in ciascuna organizzazione e la configurazione sarà disponibile per tutti i tuoi spazi.

## La mia applicazione si deve trovare nello stesso spazio?
{: #app-space}

### Domanda
Devo eseguire l'applicazione Node.js nello stesso spazio {{site.data.keyword.Bluemix_notm}} del servizio Secure Gateway?

### Risposta
No, non devi necessariamente eseguire la tua applicazione nello stesso spazio {{site.data.keyword.Bluemix_notm}} del servizio Secure Gateway.

## Posso ottenere i log del server Secure Gateway?
{: #server-logs}

### Domanda
Posso richiamare i log degli errori per il server Secure Gateway?

### Risposta
Non è possibile richiamare i log degli errori sul server. Possono essere visti solo gli errori che si verificano al momento della richiesta.

## Quali sono gli stati funzionali di Secure Gateway?
{: #states}

### Domanda
Quali sono i diversi stati del ciclo di vita di gateway e destinazioni?

### Risposta

#### Stato non funzionale
La release 1.7.0 ha introdotto un nuovo modello di prezzo dei piani a livelli. Con questo modello è stata fornita la capacità di contrassegnare sia i gateway che le destinazioni come 'Attivo' o 'Inattivo'. Parte della nuova struttura di fatturazione del piano addebita all'utente il numero di gateway e destinazioni che ha.

- Gli elementi inattivi non sono fatturati
- Le destinazioni inattive non hanno una porta cloud di provisioning.
- Anche tutte le destinazioni in un gateway inattivo saranno inattive e non potranno essere riattivate finché anche il loro gateway non sarà impostato come Attivo.
- Gli elementi inattivi sono considerati non funzionali. Gli elementi inattivi non possono essere in alcuno degli stati funzionali.

#### Stati funzionali
Quando sono contrassegnati come attivi, un gateway o una destinazione saranno fatturati. Gli stati attivi per gateway e destinazioni sono indicati di seguito.

- Abilitato - il gateway o la destinazione sono pronti per l'utilizzo in circostanze normali.
- Disabilitato - il gateway o la destinazione sono stati disabilitati.
    - Gateway - i client non saranno in grado di far fluire i dati al gateway disabilitato.
    - Destinazioni - i client non saranno in grado di creare connessioni a queste destinazioni disabilitate.

## Come faccio a conoscere le attività dei dati sul client Secure Gateway?
{: #data-size}

### Domanda
Come faccio a conoscere le attività di dati tramite il client Secure Gateway?

### Risposta
Sul client Secure Gateway, modifica il livello di log in TRACE. Le seguenti informazioni verranno visualizzate dopo l'invio delle richieste.

Dimensione dei dati inviata dall'applicazione di richiesta:
```
[TRACE] Connection #<connection ID> transmitted data: <size> bytes
```

Dimensione dei dati inviata dalla destinazione:
```
[TRACE] Connection #<connection ID> received data: <size> bytes
```

## Come ottengo il numero di connessioni in tempo reale su Secure Gateway?
{: #connect-num}

### Domanda
Come ottengo nel informazioni sulle connessioni in tempo reale e la dimensione dei dati inviata e ricevuta dal client Secure Gateway?

### Risposta

- Sulla riga di comando interattiva del client Secure Gateway:
immetti `s` per stampare i dettagli dello stato della connessione. 
- Nell'IU del client Secure Gateway, fai clic sul menu Connection Information

Saranno visualizzate le seguenti statistiche relative alla connessione: 
- La dimensione dei dati complessiva trasmessa.
- La dimensione dei dati complessiva ricevuta.
- Il numero totale di connessioni.
- Le connessioni attive in tempo reale.

Nota: il numero di connessioni correnti sull'IU di Secure Gateway non viene riprodotto in tempo reale. Utilizza le modalità su indicate sul client Secure Gateway per richiamare le informazioni sulle connessioni in tempo reale.

## Quali sono le configurazioni consigliate per rendere le mie connessioni più sicure?
{: #secure-app}

### Domanda
Quali sono le configurazioni consigliate per rendere le mie connessioni più sicure?

### Risposta

#### Utilizza l'autenticazione reciproca
Abilitare l'autenticazione reciproca per entrambi i lati delle destinazioni in loco rende Secure Gateway più sicuro. Sul lato dell'autenticazione utente (User Authentication), abilita l'autenticazione reciproca per limitare l'accesso del nodo cloud Secure Gateway autenticando l'utilizzo di un certificato client quando la richiesta è su TLS/HTTPS. Sul lato dell'autenticazione risorsa (Resource Authentication), abilita l'autenticazione reciproca per fornire delle credenziali appropriate in fase di connessione all'endpoint di destinazione e garantire un accesso protetto/crittografato alla risorsa in loco. Per ulteriori informazioni, vedi [Configurazione dell'autenticazione reciproca](/docs/services/SecureGateway/securegateway_destination.html#mutual-auth) e [Autenticazione reciproca TLS di Node.js](/docs/services/SecureGateway/securegateway_tls-ma.html#node-js-tls-mutual-authentication).

#### Imposta le regole tabella IP (per la destinazione in loco)
Host e porta cloud Secure Gateway di una destinazione in loco si trovano nello spazio pubblico e pertanto, per impostazione predefinita, tutti possono accedervi.
Per controllare il traffico che accede su Secure Gateway, imposta le regole iptable per consentire l'accesso solo a uno specifico intervallo di IP e porte per proteggere le risorse in loco. Per ulteriori informazioni su come configurare le regole iptable su Secure Gateway, vedi [Regole tabella IP](/docs/services/SecureGateway/securegateway_destination.html#configuring-network-security).

#### Configura l'ACL (Access Control List) (per la destinazione in loco)
Configurare il supporto ACL (Access Control List) per consentire o limitare l'accesso alle risorse in loco renderà le destinazioni in loco più sicure specificando il diritto di accesso sull'host e sulla porta di destinazione specifici. Si consiglia di definire le rotte HTTP/S consentite o limitate anche sulle voci ACL per migliorare la sicurezza della destinazione in loco. Per ulteriori informazioni, vedi [Access Control List](/docs/services/SecureGateway/securegateway_acl.html#access-control-list) e [Controllo delle rotte HTTP/S](/docs/services/SecureGateway/securegateway_acl.html#routes).

#### Imposta la password sull'IU del client Secure Gateway
Si consiglia di impostare la password dell'IU per limitare l'accesso dell'IU del client Secure Gateway. Vedi [Interazione con il client](/docs/services/SecureGateway/securegateway_interaction.html#interacting-with-the-client) per ulteriori dettagli su come impostare la password utilizzando la configurazione di avvio o i comandi interattivi sulla riga di comando del terminale client Secure Gateway.
