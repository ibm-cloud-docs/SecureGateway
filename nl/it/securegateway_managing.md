---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---

# Gestione del tuo servizio {{site.data.keyword.SecureGateway}}
{: #manage-sg-service}

## {{site.data.keyword.SecureGateway}} Dashboard
Dal tuo dashboard {{site.data.keyword.SecureGateway}}, puoi rapidamente vedere i seguenti dettagli:

- Il numero corrente di connessioni alle tue destinazioni in tutti i tuoi gateway.
- Il flusso di dati in entrata in tutti i tuoi gateway nelle ultime 12 ore.
- Il flusso di dati in uscita in tutti i tuoi gateway nelle ultime 12 ore.
- Un grafico che visualizza le ultime 12 ore di utilizzo su base singolo gateway.
- I tuoi gateway esistenti, il loro stato corrente e il numero di destinazioni che contengono.

![{{site.data.keyword.SecureGateway}} - dashboard con utilizzo](./images/dashboardUsage.png?raw=true "{{site.data.keyword.SecureGateway}} - dashboard con utilizzo")

### Service Profile
Facendo clic sul pulsante Profile, puoi visualizzare:

Campo | Descrizione
-- | --
Plan Level | Il tuo piano di servizio attualmente selezionato.
Gateway Limit | Il numero di gateway che puoi avere prima che ti vengano addebitati dei gateway supplementari. 
Client Limit | Il numero di client che puoi connettere a qualsiasi singolo gateway.
Destination Limit | Il numero di destinazioni che qualsiasi singolo gateway può contenere.
Data Limit | La quantità di trasferimento di dati (in GB) supportata dal tuo piano prima che ti venga addebitata un'eccedenza di dati. 
Gateway Overages | Il numero corrente di eccedenze gateway a te addebitato.
Data Overages | Il numero di eccedenze dati sostenuto per il ciclo di fatturazione del mese corrente.

### Pannello Gateway Info
Facendo clic sul pulsante Settings su qualsiasi tile di gateway, puoi vedere:

- Il token di sicurezza richiesto per utilizzare l'API o per connettere un client, a seconda che il token sia implementato o meno sulle connessioni client.
- L'ID gateway richiesto per utilizzare l'API o per connettere un client.
- Il nodo su cui è stato creato il tuo gateway.  Questo è il server a cui si connetterà il tuo client.
- La data di creazione del gateway e l'utente che lo ha creato.
- La data dell'ultima modifica del gateway e l'utente che lo ha modificato.
- Un link per rigenerare il certificato e la chiave associati al gateway.  Questi file vengono utilizzati solo con le destinazioni legacy che non contengono una loro coppia certificato/chiave per l'autenticazione reciproca sul client.

Dal pannello Gateway Info, è possibile completare le seguenti attività:

- Modificare o visualizzare ulteriori informazioni relative al tuo gateway e al tuo token di sicurezza. Quando selezioni questa opzione, viene visualizzato un nuovo pannello.
- Disabilitare o abilitare il gateway.
- Eliminare il gateway.

## Dashboard per uno specifico gateway
Facendo clic su uno dei tile di gateway, viene visualizzato il dashboard per tale specifico gateway. Simile al dashboard {{site.data.keyword.SecureGateway}} principale, puoi rapidamente visualizzare i seguenti dettagli:

- Il numero corrente di connessioni alle destinazioni su questo gateway.
- Il flusso di dati in entrata in questo gateway nelle ultime 12 ore.
- Il flusso di dati in uscita in questo gateway nelle ultime 12 ore.
- Un grafico che visualizza le ultime 12 ore di utilizzo su base singola destinazione.
- Le tue destinazioni esistenti, il loro stato corrente e il loro numero corrente di connessioni attive.

![Dashboard per uno specifico gateway](./images/viewGateway.png?raw=true "Dashboard per uno specifico gateway")

### Pannello Destination Info
Facendo clic sul pulsante Settings su qualsiasi tile di destinazione, puoi vedere:

- L'ID destinazione richiesto per l'utilizzare l'API.
- Le destinazioni in loco avranno una porta e un host cloud. Queste informazioni sono richieste dalla tua applicazione per stabilire una connessione alla tua risorsa in loco.
- Le destinazioni cloud avranno una porta client. Questa è la porta su cui il client {{site.data.keyword.SecureGateway}} sarà in ascolto per connettere la tua applicazione in loco alla tua risorsa cloud.
- L'host e la porta della risorsa a cui è puntata questa destinazione.
- La data di creazione della destinazione e l'utente che l'ha creata.
- La data dell'ultima modifica della destinazione e l'utente che l'ha modificata.

Dal pannello Destination Info, è possibile completare le seguenti attività:

- Modificare o visualizzare ulteriori informazioni sulla destinazione.  Quando selezioni questa opzione, viene visualizzato un nuovo pannello.
- Disabilitare o abilitare la destinazione.
- Eliminare la destinazione.
