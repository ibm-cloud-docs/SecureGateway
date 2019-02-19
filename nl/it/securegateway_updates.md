---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---

# Log delle modifiche {{site.data.keyword.SecureGateway}}
{: #secure-gateway-change-log}

## v1.7.0 Fixpack 1

### Correzioni

- Corregge l'errore `EADDRINUSE` causato da un non corretto teardown dei listener all'eliminazione della destinazione.
- Risolve il problema per cui non era possibile eliminare le istanze del servizio associate a un'applicazione.

## v1.7.0

### Funzioni

- Introduce nuovi piani del servizio: Essentials, Professional ed Enterprise.
- Il client Secure Gateway ora supporta il proxy SOCKS tra se stesso e la destinazione in loco.

## v1.6.0 Fixpack 1

### Correzioni

- Risolve il problema per cui più client che si disconnettono producono dei processi orfani nell'array di client connesso.
- Ora ripulisce le connessioni orfane causate da listener messi in pausa in modo non corretto.

## v1.6.0

### Funzioni

- L'ACL (Access Control List) ora supporta uno specifico instradamento di percorso per le richieste HTTP/S.
- Le destinazioni adesso supportano gli indicatori di nome server per le connessioni TLS.

## v1.5.1

### Funzioni

- È stata aggiunta una procedura guidata per le destinazioni per la creazione della destinazione.
- I gateway supportano adesso il caricamento di una coppia certificato/chiave.
- I servizi sono ora limitati a 50 gateway e 50 destinazioni per ogni gateway.
- Destinazioni, gateway e servizi possono ora essere esportati/importati.
- Test di latenza tra client e server in grado di essere eseguiti dall'IU di Secure Gateway.

## v1.5.0 Fixpack 1

### Correzioni

- Ripulisce i processi tunnel orfani alla disconnessione di rete.
- Risolve l'assegnazione di porta duplicata causata da un'eliminazione della destinazione non riuscita.
- Risolve il problema HTTP/S con codifica non UTF8.
- Risolve i gestori IPC mancanti sui processi di sostituzione.

## v1.5.0

### Funzioni

- Il client ora dispone di una IU interattiva locale.
- Le connessioni UDP sono adesso supportate.
- Il footprint di memoria del client è stato drasticamente ridotto.
- La modalità a singolo client non è più supportata; multi-client sarà il valore predefinito e sarà trasparente a un client a singolo gateway.
- l'ACL (Access Control List) esegue la sincronizzazione tra i client connessi allo stesso gateway.

## v1.4.2

### Funzioni

- Ai client viene ora assegnato un ID quando si connettono al server SG.
- I client ora vengono terminati in remoto tramite l'API o tramite l'IU di Secure Gateway.
- Lo stato di connesso di un client può essere controllato tramite l'API.
- La TLS lato destinazione ora supporta ora l'autenticazione reciproca.
- Le destinazioni cloud ora supportano i protocolli di autenticazione reciproca.
