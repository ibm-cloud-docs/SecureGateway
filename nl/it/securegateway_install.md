---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-07"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Installazione del client
{: #client-install}

## Docker
{: #installing-docker}

Docker è una piattaforma di
terze parti che fornisce un approccio di contenitore per installare
applicazioni in modo rapido e semplice con poca o nessuna configurazione necessaria. Il servizio {{site.data.keyword.SecureGateway}}
fornisce un'immagine Docker da utilizzare una volta che il programma di utilità Docker viene installato
sulla workstation.  Per installare il Docker, vedi il sito web di installazione del Docker e attieniti alle istruzioni per il tuo sistema.

### Installazione e aggiornamento del client
{: #docker-install}

Installazione e aggiornamento dell'immagine Docker del client {{site.data.keyword.SecureGateway}}, utilizzano entrambi lo stesso comando:

```
docker pull ibmcom/secure-gateway-client
```
{: pre}

### Avvio di una sessione client interattiva
{: #docker-run}

Per eseguire il contenitore Docker per {{site.data.keyword.SecureGateway}}, immetti il seguente comando:

```
docker run -it ibmcom/secure-gateway-client <gateway ID> -t <security token>
```
{: pre}

Di norma utilizza due parametri, il tuo ID gateway {{site.data.keyword.SecureGateway}} e il token di sicurezza del gateway, entrambi disponibili mediante il dashboard {{site.data.keyword.SecureGateway}}.

### Comandi Docker supportati
{: #docker-commands}

Il client {{site.data.keyword.SecureGateway}} supporta solo i comandi `pull` e `run` per la gestione del contenitore.

Ritorna a [Introduzione - Aggiunta di un client](/docs/services/SecureGateway?topic=securegateway-add-client).

## Mac OS X
{: #installing-mac}

### Requisiti per l'esecuzione su Mac OS X
{: #mac-requirements}

Sono richiesti i seguenti prerequisiti prima di eseguire il client su Mac OS X.
 1. Installa gli strumenti di sviluppo Xcode; ciò è richiesto da alcuni dei moduli del nodo su questa piattaforma.

### Procedura di installazione di Mac OS X
{: #mac-install}

Potresti aver bisogno di privilegi amministrativi per eseguire questa installazione, a seconda della configurazione della sicurezza del tuo sistema.

 1. Monta l'immagine DMG che era stata scaricata dall'IU {{site.data.keyword.SecureGateway}}, normalmente "facendo doppio clic" su di essa.
 2. Dovrebbe essere visualizzata una nuova finestra 'finder'. In caso contrario, 'fai doppio clic' sul volume montato. Questa finestra dovrebbe contenere un'icona cartella "ibm" e un'icona "collegamento rapido" Applications; trascina e rilascia la cartella "ibm" nel collegamento rapido.

### Avvio di una sessione client interattiva
{: #mac-run}

Per avviare il client, esegui il file `secgw.command` che si trova nell'ubicazione di installazione predefinita: `/Applications/ibm/`.

Ritorna a [Introduzione - Aggiunta di un client](/docs/services/SecureGateway?topic=securegateway-add-client).

## Linux
{: #installing-linux}

L'installazione include il client {{site.data.keyword.SecureGatewayfull}} nonché una versione protetta del pacchetto nodejs di IBM.  Entrambi sono installati nella directory /opt/ibm sul sistema.  Il programma di installazione creerà o aggiornerà quanto segue:

| Directory | Nome file | Descrizione          |
| ------------- | ------------- | ----------- |
| /etc/ibm | sgenvironment.conf | file di configurazione per upstart |
| /etc/ibm | passwd | crea l'utente: secgwadmin:x:501:501::/home/secgwadmin:/bin/bash |
| /etc/ibm | group | crea il gruppo: secgwadmin:x:501: |
| /etc/init | securegateway_client.conf | file upstart |
| /opt/ibm | node-v&lt;version&gt;-linux-x64 | Installazione di Node JS di IBM |
| /opt/ibm | securegateway | installazione del client {{site.data.keyword.SecureGateway}} |
| /usr/local/bin | node | symlink creato per l'eseguibile del nodo di IBM |
| /usr/local/bin | npm | symlink creato per l'eseguibile npm di IBM |
| /var/log/securegateway | client_console.log | file di log creato in fase di esecuzione del client come un processo di avvio automatico |

Per eseguire questa installazione, avrai bisogno di privilegi root o amministrativi.

### Installazione Ubuntu/PowerPC
{: #debian-install}

 1. Installa il client {{site.data.keyword.SecureGateway}}. Ad esempio, se stai utilizzando un pacchetto Debian, immetti il seguente comando.

    ```
    sudo dpkg -i <secure-gateway-debian-package>
    ```
    {: pre}

 2. All'avvio del programma di installazione del client, ti vengono richieste le seguenti informazioni.

    <b>Auto-start process Yes/No</b>
        Se stai effettuando un upgrade o una reinstallazione, devi scegliere se desideri che il client
        esistente venga arrestato durante l'esecuzione del processo di installazione. Scegli Y/y per arrestare il client esistente. In caso contrario, l'upgrade del pacchetto client viene         eseguito senza riavviare il client, il che significa che puoi attendere una finestra di aggiornamento software appropriata per eseguire un riavvio. Se scegli N/n, l'installazione continua e devi riavviare il client
        manualmente.

    <b>Gateway ID</b>
        Imposta l'ID gateway per il client. L'ID gateway viene definito quando crei un servizio {{site.data.keyword.SecureGateway}}. Se il client non riesce a connettersi,
        puoi cambiare la selezione modificando il file .conf.

    <b>Security token</b>
        Se l'ID gateway è abilitato a controllare la presenza di un token di sicurezza, è necessario fornirlo ora. Se l'ID
gateway non richiede un token di sicurezza, lascia vuoto questo campo.

    <b>Logging level</b>
        L'impostazione predefinita è INFO. Altri valori validi sono TRACE, DEBUG ed ERROR.

    <b>Access Control List</b>
        Immetti il percorso assoluto del nome file che contiene l'ACL (Access Control List) su quanto ha accesso alle
        tue risorse in loco. Per ulteriori informazioni, consulta il file README.md in /opt/ibm/securegateway o l'ACL (Access Control List).

    <b>Client UI</b>
        Scegli se vuoi usare l'IU client. In caso affermativo, l'utente può modificare la porta per l'IU. Il valore predefinito è 9003.

    <b>Nota:</b> non devi necessariamente rispondere alle richieste; per tutti i valori ad esse relativi verrà utilizzato il valore predefinito che è stato definito oppure verranno lasciati vuoti nel file
sgenvironment.conf. Ciò consente di completare il processo di installazione
senza interazione da parte dell'utente.

    <b>Nota:</b> il file sgenvironment.conf viene letto ogni volta che avvii o riavvii il client utilizzando il processo di upstart del sistema. Puoi modificare il file /etc/ibm/sgenvironment.conf in qualsiasi momento per apportare modifiche alla tua configurazione e riavviare il client per rilevare tali modifiche.

    <b>Nota:</b> la lingua dei log del servizio client {{site.data.keyword.SecureGateway}} può essere cambiata modificando il parametro `LANGUAGE` nel file /etc/ibm/sgenvironment.conf. I log del servizio verranno modificati nella lingua selezionata dopo il riavvio del servizio.

 3. Se hai riavviato il client, per assicurati che il client sia in esecuzione correttamente, immetti il seguente comando:

    ```
    cat /var/log/securegateway/client_console.log
    ```
    {: pre}

 4. Se vuoi modificare il file di configurazione, per assicurarti che il file di configurazione venga aggiornato correttamente, immetti questo comando:

    ```
    sudo vi /etc/ibm/sgenvironment.conf
    ```
    {: pre}

 5. Per controllare lo stato dell'installazione del client, immetti il
seguente comando:

    ```
    sudo dpkg --status ibm-securegateway-client
    ```
    {: pre}

Viene restituito un esempio dell'output:

```
Package: ibm-securegateway-client
Status: install ok installed
Priority: extra
Section: IBM/net
Maintainer: cloudoe-dev@webconf.ibm.com
Architecture: amd64
Source: ibm-securegateway+securegateway-client
Version: 1.7.0
Description: IBM Secure Gateway Client for Bluemix
IBM Secure Gateway client package for Bluemix, supports secure connections to on-premises connections.
Homepage: https://ng.bluemix.net/
License: http://www.ibm.com/software/sla/sladb.nsf/lilookup/986C7686F22D4D3585257E13004EA6CB?OpenDocument
```
{: screen}

### Installazione di RedHat/SuSE
{: #rpm-install}

1. Installa il client {{site.data.keyword.SecureGateway}}. Ad esempio, se stai utilizzando un pacchetto RPM, immetti il seguente comando:

   ```
   rpm -ivhf <secure-gateway-rpm-package>
   ```
   {: pre}


   Il programma di installazione del client avvia e installa il client; creerà un file sgenvironment.conf in /etc/ibm.

2. Facoltativo: se vuoi usare il processo upstart del sistema, devi modificare questo file e fornire quanto segue perché il client venga avviato correttamente. Vedi la sezione relativa all'[utilizzo di upstart](/docs/services/SecureGateway?topic=securegateway-auto-start-conf#auto-start-linux) per ulteriori informazioni sulla modifica di questo file di configurazione.

3. Se hai avviato il client usando upstart, controlla il file di log per assicurarti che sia in esecuzione correttamente.

   ```
   cat /var/log/securegateway/client_console.log
   ```
   {: pre}

4. Per controllare lo stato dell'installazione del client, immetti il
seguente comando:

   ```
   rpm -q ibm-securegateway-client
   ```
   {: pre}

### Installazione di AIX
{: #aix-install}

1. Assicurati che l'autorizzazione eseguibile sia configurata per il pacchetto. Se necessario, modifica le autorizzazioni file immettendo il seguente comando:
    ```
    chmod a+x <secure-gateway-bin-package>
    ```
2. Estrai il pacchetto immettendo il seguente comando:
    ```
    ./<secure-gateway-bin-package>
    ```

Nota:
assicurati se il tuo sistema AIX dispone dei requisiti installati per l'esecuzione di Node.js e ksh.

### Avvio di una sessione client interattiva
{: #linux-run}

Per avviare il client, esegui questi comandi:

```
cd /opt/ibm/securegateway/client
node lib/secgwclient.js <gateway ID> -t <security token>
```
{: codeblock}

Di norma utilizza due parametri, un ID gateway {{site.data.keyword.SecureGateway}} e il token di sicurezza del gateway, entrambi disponibili mediante il dashboard {{site.data.keyword.SecureGateway}}.

Ritorna a [Introduzione - Aggiunta di un client](/docs/services/SecureGateway?topic=securegateway-add-client).

## Windows
{: #installing-windows}

### Installazione del client
{: #windows-install}

Potresti aver bisogno di privilegi amministrativi per eseguire questa installazione, a seconda della configurazione della sicurezza del tuo sistema.

 1. Copia il file del programma di installazione EXE nel tuo sistema.  Viene fornita anche una somma MD5 che puoi utilizzare per controllare
l'integrità del pacchetto di installazione.

 2. Installalo facendo doppio clic su di esso e rispondendo alle richieste oppure eseguendo l'eseguibile dal prompt dei comandi.

 3. All'utente verrà richiesto di scegliere la directory di installazione che preferisce. L'ubicazione predefinita è la directory Program Files (x86).

 4. All'utente verrà richiesto di selezionare la lingua in cui vuole avviare la CLI (Command Line Interface). Se non viene selezionata una lingua, verrà utilizzata. per impostazione predefinita, la lingua inglese.

 5. All'utente verrà richiesto di installare il client come un servizio Windows. Se l'utente sceglie di procedere in tal senso, il client verrà eseguito
in background come un servizio Windows con le configurazioni fornite dall'utente nella finestra di dialogo successiva. Lo stato del servizio può essere controllato nella pagina dei servizi del Pannello di controllo. Il nome del servizio è "IBM Bluemix Secure Gateway Service".

 6. Il programma di installazione identifica se c'è un'installazione esistente sulla macchina. Se viene rilevata, all'utente viene chiesto se vuole utilizzare le configurazioni esistenti, altrimenti l'utente ha l'opzione di immettere delle nuove configurazioni. Qui è possibile fornire dettagli quali gli ID gateway, i token di sicurezza, i file acl e il livello di log per ogni gateway. Se l'utente ha scelto di eseguire il client come un servizio Windows, il client verrà avviato con le configurazioni fornite. Se l'utente non ha scelto di avviare il client come un servizio Windows, le configurazioni verranno archiviate per un utilizzo futuro. In entrambi i casi, le configurazioni sono archiviate in %Installation_directory%/ibm/securegateway/client/securegw_service.config.

 7. A partire dalla versione 1.5.0, il client viene fornito con un'IU client pienamente funzionale. Puoi configurarla dal programma di installazione stesso. L'utente può fornire la password e il numero porta (il valore predefinito è 9003) da cui verrà avviata l'IU client.

 Se sceglie di avviare il client con la connessione a più gateway, l'utente è invitato a prestare attenzione quando fornisce i valori di configurazione. Gli ID gateway devono essere separati da spazi. I token di sicurezza, i file ACL e i livelli di log devono essere delimitati da --. Se non vuoi fornire alcuno di questi tre valori per uno specifico gateway, usa 'none'.

 <b>Nota:</b> assicurati che non ci siano spazi vuoti residui.

 <b>Nota:</b> i file acl devono essere inseriti nella directory %Installation_directory%/ibm/securegateway/client o in quella relativa a tale percorso.

### Avvio di una sessione client interattiva
{: #windows-run}

Per avviare il client, apri un prompt dei comandi con privilegi amministrativi ed esegui questi comandi:

```
cd "<Installation_directory>\ibm\securegateway\client"
secgw.cmd
```
{: codeblock}

In alternativa, vai a `<Installation_directory>\ibm\securegateway\client` e fai doppio clic su `secgw.cmd`.

<b>Nota:</b>puoi scegliere di utilizzare le configurazioni memorizzate nel file `<Installation_directory>\ibm\securegateway\client\securegw_service.config` oppure fornire i dettagli in modo interattivo.

Ritorna a [Introduzione - Aggiunta di un client](/docs/services/SecureGateway?topic=securegateway-add-client).

## DataPower
{: #installing-datapower}

DataPower ha una versione integrata del client {{site.data.keyword.SecureGateway}}.  A seconda della versione di DataPower, potresti avere una versione differente del client {{site.data.keyword.SecureGateway}}.  Tieni presente le eventuali [limitazioni del client DataPower](/docs/services/SecureGateway?topic=securegateway-client-interacting#limits-datapower) applicabili. Se utilizzi il vecchio client Secure Gateway, potresti riscontrare degli errori imprevisti.

| Versione di DataPower | Versione del client {{site.data.keyword.SecureGateway}}  |
| -- | --  |
| 7.2.0.0, 7.5.0.0 | 1.1.0  |
| 7.5.1.0, 7.7.0 | 1.4.2  |
| 7.5.2.4 | 1.6.1  |
| 7.5.2.6, 7.6.0.0 | 1.7.0  |
| 7.5.2.14, 7.6.0.7, 7.7.1.0, 2018.4.1.0 |  1.8.0fp6  |
| 2018.4.1.4 | 1.8.2  |
| 7.6.0.15, 2018.4.1.6 | 1.8.2fp1 |

### Avvio di una sessione client
{: #datapower-run}

1. Accedi alla GUI web DataPower
2. Passa a Objects -> Cloud -> Secure Gateway Client oppure cerca il client Secure Gateway
3. Fai clic su `Add` per configurare una nuova connessione client
4. Fornisci un nome (Name), l'ID gateway (Gateway ID) e il token di sicurezza (Security Token) (se applicabile) e applica quindi le modifiche.

Ritorna a [Introduzione - Aggiunta di un client](/docs/services/SecureGateway?topic=securegateway-add-client).
