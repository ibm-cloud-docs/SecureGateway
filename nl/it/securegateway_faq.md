---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-16"

---

# Domande frequenti (FAQ - Frequently Asked Questions)
{: #sg-faq}

## Livello modello OSI
{: #faq-osi}

### Domanda
{: #osi-question}
Quale livello del modello OSI rappresenta il servizio Secure Gateway?

### Risposta
{: #osi-answer}
Il servizio Secure Gateway rappresenta il livello 4 del modello OSI.

## Supporto versione TLS
{: #faq-tls}

### Domanda
{: #tls-question}
Quale versione di TLS supporta il servizio Secure Gateway?

### Risposta
{: #tls-answer}
Il servizio Secure Gateway supporta TLS versione 1.2.

## Perché vorrei disabilitare il mio gateway o la mia destinazione?
{: #faq-disabled}

### Domanda
{: #disabled-question}

Perché vorresti disabilitare un gateway o una destinazione?

### Risposta
{: #disabled-answer}
Potresti voler disabilitare una destinazione o gateway per uno dei seguenti motivi:

- Crei una destinazione ma non hai impostato la sicurezza.  In questo caso, potresti disabilitare
la destinazione finché la sicurezza non sarà impostata.
- Non vuoi che il servizio sia disponibile agli utenti perché stai apportando alcuni aggiornamenti al
servizio.  In questo caso, potresti disabilitare temporaneamente i gateway necessari e attendere che il servizio venga aggiornato.
- Hai configurato tutti i tuoi gateway e tutte le tue destinazioni sul front-end, ma il tuo backend è ancora in fase di creazione.  In questo caso, potresti disabilitare i gateway o le destinazioni finché la creazione del backend non
sarà completata.

Per ulteriori informazioni sulla disabilitazione di un gateway o di una destinazione, vedi il documento relativo alla [modalità di gestione della tua istanza del servizio Secure Gateway](/docs/services/SecureGateway/securegateway_managing.html).

## Qual è l'approccio consigliato all'automazione della creazione su più spazi?
{: #faq-automation-spaces}

### Domanda
{: #automation-spaces-question}
Un ambiente del cliente ha un'organizzazione e tre spazi.  Uno spazio è per lo sviluppo, un altro per la preparazione e l'ultimo per la produzione.  Il cliente deve creare una singola istanza Secure Gateway o più di una (ad es. una per ogni spazio)? Se il cliente può creare più gateway, ci sono delle considerazioni da fare per il riutilizzo di un'applicazione Node.js per creare un gateway e una destinazione in ogni spazio?

### Risposta
{: #automation-spaces-answer}

- Puoi creare una singola istanza Secure Gateway per tutti e tre gli spazi.  Devi tuttavia ricordarti delle [limitazioni per il tuo specifico piano](/docs/services/SecureGateway/securegateway_plans.html) relative a gateway e destinazione.
- Non ci sono ulteriori considerazioni per il riutilizzo di un'applicazione Node.js poiché Secure Gateway non richiede alcun bind di servizio.


## Qual è l'approccio consigliato all'automazione della creazione in più organizzazioni?
{: #faq-automation-orgs}

### Domanda
{: #automation-orgs-question}
Un ambiente del cliente ha tre organizzazioni: una per lo sviluppo, una per la preparazione e una per la produzione.  È richiesta un'istanza del servizio Secure Gateway per ciascuna organizzazione e la configurazione è disponibile per tutti gli spazi all'interno di tale organizzazione?

### Risposta
{: #automation-orgs-answer}

- Non è obbligatorio che tu abbia un'istanza del servizio Secure Gateway in ciascuna organizzazione. Puoi avere un'istanza in un'organizzazione e utilizzare i gateway all'interno di tale istanza da tutti gli altri tuoi ambienti.  Con questa configurazione, devi ricordarti delle [limitazioni per il tuo specifico piano](/docs/services/SecureGateway/securegateway_plans.html) relative a gateway e destinazione.
- Puoi avere un'istanza del servizio Secure Gateway in ciascuna organizzazione e la configurazione sarà disponibile per tutti i tuoi spazi.

## La mia applicazione si deve trovare nello stesso spazio?
{: #faq-app-space}

### Domanda
{: #app-space-question}
Devo eseguire l'applicazione Node.js nello stesso spazio {{site.data.keyword.Bluemix_notm}} del servizio Secure Gateway?

### Risposta
{: #app-space-answer}
No, non devi necessariamente eseguire la tua applicazione nello stesso spazio {{site.data.keyword.Bluemix_notm}} del servizio Secure Gateway.

## Posso ottenere i log del server Secure Gateway?
{: #faq-server-logs}

### Domanda
{: #server-logs-question}
Posso richiamare i log del livello di errore per il server Secure Gateway?

### Risposta
{: #server-logs-answer}
Non è possibile richiamare i log dl livello di errore sul server. Possono essere visti solo gli errori che si verificano al momento della richiesta.

## Quali sono gli stati funzionali di Secure Gateway?
{: #faq-states}

### Domanda
{: #states-question}
Quali sono i diversi stati del ciclo di vita di gateway e destinazioni?

### Risposta
{: #states-answer}

#### Stato non funzionale
{: #states-answer-non-functional}
La release 1.7.0 ha introdotto un nuovo modello di prezzo dei piani a livelli. Con questo modello è stata fornita la capacità di contrassegnare sia i gateway che le destinazioni come 'Attivo' o 'Inattivo'. Parte della nuova struttura di fatturazione del piano addebita all'utente il numero di gateway e destinazioni che ha.

- Gli elementi inattivi non sono fatturati
- Le destinazioni inattive non hanno una porta cloud di provisioning.
- Anche tutte le destinazioni in un gateway inattivo saranno inattive e non potranno essere riattivate finché anche il loro gateway non sarà impostato come Attivo.
- Gli elementi inattivi sono considerati non funzionali. Gli elementi inattivi non possono essere in alcuno degli stati funzionali.

#### Stati funzionali
{: #states-answer-functional}
Quando sono contrassegnati come attivi, un gateway o una destinazione saranno fatturati. Gli stati attivi per gateway e destinazioni sono indicati di seguito.

- Abilitato - il gateway o la destinazione sono pronti per l'utilizzo in circostanze normali.
- Disabilitato - il gateway o la destinazione sono stati disabilitati.
    - Gateway - i client non saranno in grado di far fluire i dati al gateway disabilitato.
    - Destinazioni - i client non saranno in grado di creare connessioni a queste destinazioni disabilitate.

## Come faccio a conoscere le attività dei dati sul client Secure Gateway?
{: #faq-data-size}

### Domanda
{: #data-size-question}
Come faccio a conoscere le attività di dati tramite il client Secure Gateway?

### Risposta
{: #data-size-answer}
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
{: #faq-connect-num}

### Domanda
{: #connect-num-question}
Come ottengo le informazioni sulle connessioni come ad esempio il numero di connessioni in tempo reale e la dimensione dei dati inviata e ricevuta dal client Secure Gateway?

### Risposta
{: #connect-num-answer}

- Sulla riga di comando interattiva del client Secure Gateway:
immetti `s` per stampare i dettagli dello stato della connessione. 
- Nell'IU del client Secure Gateway, fai clic sul menu Connection Information

Saranno visualizzate le seguenti statistiche relative alla connessione: 
- La dimensione dei dati complessiva trasmessa.
- La dimensione dei dati complessiva ricevuta.
- Il numero totale di connessioni.
- Le connessioni attive in tempo reale.

Nota: queste informazioni sulle connessioni sono al livello del client, non a quello del gateway. Se hai bisogno di informazioni sulle connessioni al livello del gateway, controlla ogni client collegato a tale gateway.

## Quali sono le configurazioni consigliate per rendere le mie connessioni più sicure?
{: #faq-secure-app}

### Domanda
{: #secure-app-question}
Quali sono le configurazioni consigliate per rendere le mie connessioni più sicure?

### Risposta
{: #secure-app-answer}

#### Utilizza l'autenticazione reciproca
{: #secure-app-answer-ma}
Abilitare l'autenticazione reciproca per entrambi i lati delle destinazioni in loco rende Secure Gateway più sicuro. Sul lato dell'autenticazione utente (User Authentication), abilita l'autenticazione reciproca per limitare l'accesso del nodo cloud Secure Gateway autenticando l'utilizzo di un certificato client quando la richiesta è su TLS/HTTPS. Sul lato dell'autenticazione risorsa (Resource Authentication), abilita l'autenticazione reciproca per fornire delle credenziali appropriate in fase di connessione all'endpoint di destinazione e garantire un accesso protetto/crittografato alla risorsa in loco. Per ulteriori informazioni, vedi [Configurazione dell'autenticazione reciproca](/docs/services/SecureGateway/securegateway_destination.html#dest-mutual-auth) e [Autenticazione reciproca TLS di Node.js](/docs/services/SecureGateway/securegateway_tls-ma.html#nodejs-tls-ma).

#### Imposta le regole tabella IP (per la destinazione in loco)
{: #secure-app-answer-iptables}
Host e porta cloud Secure Gateway di una destinazione in loco si trovano nello spazio pubblico e pertanto, per impostazione predefinita, tutti possono accedervi.
Per controllare il traffico che accede su Secure Gateway, imposta le regole iptables per consentire l'accesso solo a uno specifico intervallo di IP e porte per proteggere le risorse in loco. Per ulteriori informazioni su come configurare le regole iptables su Secure Gateway, vedi [Regole tabella IP](/docs/services/SecureGateway/securegateway_destination.html#dest-network-security).

#### Configura l'ACL (Access Control List) (per la destinazione in loco)
{: #secure-app-answer-acl}
Configurare il supporto ACL (Access Control List) per consentire o limitare l'accesso alle risorse in loco renderà le destinazioni in loco più sicure specificando il diritto di accesso sull'host e sulla porta di destinazione specifici. Si consiglia di definire le rotte HTTP/S consentite o limitate anche sulle voci ACL per migliorare la sicurezza della destinazione in loco. Per ulteriori informazioni, vedi [Access Control List](/docs/services/SecureGateway/securegateway_acl.html#acl) e [Controllo della rotta HTTP/S utilizzando l'ACL](/docs/services/SecureGateway/securegateway_acl.html#acl-route-control).

#### Imposta la password sull'IU del client Secure Gateway
{: #secure-app-answer-ui-pw}
Si consiglia di impostare la password dell'IU per limitare l'accesso dell'IU del client Secure Gateway. Vedi [Interazione con il client](/docs/services/SecureGateway/securegateway_interaction.html#client-interacting) per ulteriori dettagli su come impostare la password utilizzando la configurazione di avvio o i comandi interattivi sulla riga di comando del terminale client Secure Gateway.

## Cos'è la migrazione del gateway? Perché il dominio viene modificato dopo dicembre 2018?
{: #faq-gateway-migration}

### Domanda
{: #gateway-migration-question}
Dopo la finestra di manutenzione di dicembre 2018, è presente un pulsante di migrazione sul pannello gateway, qual è l'utilizzo di questo pulsante? Perché il dominio viene modificato?

### Risposta
{: #gateway-migration-answer}

Dopo la finestra di manutenzione di dicembre 2018, l'host cloud di Secure Gateway è stato ridenominato per utilizzare `securegateway.appdomain.cloud` invece di `integration.ibmcloud.com` e utilizzare `securegateway.cloud.ibm.com` per l'autenticazione gateway invece di `bluemix.net`. Per la compatibilità con le versioni precedenti, il gateway esistente continuerà ad utilizzare il vecchio dominio finché non viene migrato il gateway e il client SG v180fp9 e precedenti continuerà ad utilizzare `bluemix.net` per l'autenticazione gateway.

Dopo la migrazione, l'host cloud delle destinazioni in loco verrà modificato per utilizzare il nuovo dominio, gli utenti/applicazioni dovranno eseguire l'aggiornamento per inviare la richiesta al nuovo host cloud.

Al momento la migrazione non è obbligatoria e non esiste una data precisa in cui il vecchio dominio non sarà più supportato, ma quando verrà decisa, il cliente che ancora sta utilizzando il vecchio nome del dominio riceverà sarà avvertito.

## Dove posso ricevere le notifiche?
{: #faq-notification}

### Domanda
{: #notification-question}
Dove posso ricevere le notifiche Secure Gateway, specialmente per la manutenzione con interruzione del servizio?

### Risposta
{: #notification-answer}

Puoi ricevere le notifiche tramite la nostra [pagina di stato](https://console.bluemix.net/status), cerca `Secure Gateway` in tale pagina.

Quando il client Secure Gateway si disconnette in modo non previsto, vai alla pagina di stato per controllare se in quel momento sta venendo eseguita una manutenzione con interruzione del servizio.
