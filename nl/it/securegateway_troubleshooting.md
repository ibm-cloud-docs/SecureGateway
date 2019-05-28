---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-09"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Risoluzione dei problemi
{: #troubleshooting}

## Prassi ottimali per l'esecuzione del client Secure Gateway
{: #best-practices}

- Esegui il client Secure Gateway su una partizione del sistema operativo che abbia la visibilità di rete dei servizi con bridging eseguito dal client stesso. Ad esempio,
alcuni ambienti di virtualizzazione su host supportano più modalità di connettività di
rete, tra cui NAT e Bridged. Accertati di selezionare il tipo di connessione corretto
in grado di fornirti accesso ai servizi {{site.data.keyword.Bluemix}}
da Internet.
- Installa il Secure Gateway nel tuo ambiente IT dove è consentito dalla tua politica di sicurezza aziendale. Si
tratta generalmente di un'area gialla protetta o DMZ in cui la tua società
può istituire controlli di sicurezza appropriati per proteggere le
risorse installate in loco. Quando installi il client Secure Gateway, rispetta sempre le istruzioni e le politiche di sicurezza aziendali.
- Prima di installare un client nel tuo ambiente, accertati che sia Internet sia le tue risorse installate in loco siano accessibili
e che tutti i nomi host possano essere risolti da un DNS.

## Passi di risoluzione dei problemi iniziali
{: #initial-troubleshooting}

- Avvia la richiesta malfunzionante dall'applicazione richiedente
- Controlla i log del client Secure Gateway
- Se non sono stati generati log del client dalla richiesta, il problema è presente tra l'applicazione richiedente e i server Secure Gateway.  Può spaziare dall'affidabilità di rete a protocolli di richiesta non corrispondenti e a un handshake di autenticazione reciproca TLS non corretto.
- Se il client ha generato i log del livello di errore dalla richiesta, allora il problema è tra il client SG e la risorsa in loco.  Di seguito è riportata una tabella che contiene gli errori comuni, i problemi che di solito li causano e i possibili metodi per risolverli.

Errore | Caso tipico | Metodi di risoluzione dei problemi
--- | --- | ---
ETIMEDOUT | Il client non è in grado di trovare il nome host o l'ip a cui connettersi a causa di vincoli di rete. | Prova ad eseguire il ping del nome host o dell'ip della destinazione dall'host che esegue il client per assicurarti della connettività di rete.  Se stai eseguendo la versione Docker del client, l'esecuzione del bridging del contenitore con il sistema operativo host utilizzando `--net=host` potrebbe risolvere il problema.
ECONNREFUSED | Il client ha risolto il nome host o l'ip a cui connettersi ma non è in grado di avviare l'handshake della connessione. | Ciò è di norma causato da un protocollo non corrispondente tra il client SG e la risorsa in loco (ad es. il client sta tentando una connessione TCP a una combinazione host:porta che prevede una connessione TLS).  In alcuni casi, una regola del firewall può causare questo errore invece di ETIMEDOUT.
ECONNRESET | Il client ha stabilito una connessione alla destinazione ma si è verificato un problema durante l'handshake (un errore di handshake TLS potrebbe generare anche diversi errori) oppure durante la gestione della richiesta da parte della risorsa in loco. | I log della risorsa in loco devono essere controllati per confermare che nessun errore ha causato l'interruzione della connessione.  Se nei log in loco non viene trovato nulla, la configurazione di destinazione deve essere esaminata per garantire che al client si stiano fornendo i protocolli appropriati (e i certificati, se necessario) per la connessione,
REMOTE_RST | Si è verificato un errore nel lato server SG. <br><br> Per la destinazione in loco, si verifica un errore quando l'applicazione richiedente si connette al server SG o un errore di timeout quando si ricevono dati dalla risorsa in loco. <br><br> Per la destinazione cloud, potrebbe trattarsi di qualsiasi cosa, da un errore di handshake TLS a errori nella risorsa cloud. | Per la destinazione in loco, assicurati che l'applicazione richiedente stia utilizzando i protocolli appropriati per stabilire la connessione al server SG; se l'errore si verifica in fase di ricezione dei dati dalla risorsa in loco, prova ad estendere/disabilitare il timeout. <br><br> Per la destinazione cloud, i log della risorsa cloud devono essere controllati per confermare che nessun errore abbia causato l'interruzione della connessione.  Se non trovi niente nei log della risorsa cloud, devi esaminare la configurazione della destinazione per assicurati che al client si stiano fornendo i protocolli appropriati ( o i certificati, se necessario) per la connessione.

Per molte applicazioni si verificano dei "blocchi" dopo un ECONNRESET che si verifica sull'altra estremità del tunnel. Ciò è previsto. Secure Gateway non può riprodurre il pacchetto RST sull'altra estremità del tunnel dal momento che i pacchetti TCP sono già stati riconosciuti (ACK) per quel lato del tunnel. La definizione di timeout sull'applicazione, che non riceveranno mai una risposta di riconoscimento,
è il solo metodo per terminare la condizione di blocco.

## Configura il tuo client Docker perché venga riavviato quando viene riavviato il tuo server
{: #docker-auto-restart}

### Che sta accadendo
{: #docker-auto-restart-what-is-happening}
Quando riavvii il server in cui viene eseguito il tuo client Secure Gateway, devi riavviare manualmente il client Docker Secure Gateway. Come si imposta il riavvio automatico del client a seguito
del riavvio del sistema?

### Come correggerlo
{: #docker-auto-restart-how-to-fix-it}

- Sui sistemi Linux o UNIX:
- Integra il comando Docker in uno script che possa essere richiamato
come risultato di un lavoro CRON.
- Se stai utilizzando una workstation Linux, puoi creare una configurazione upstart per fare in modo che il client venga avviato ad ogni riavvio del server. Per ulteriori
informazioni, vedi Automatically Start Containers nel sito web Docker.
- Sui sistemi Windows, immetti il seguente comando per avviare il client:

```
for /L %i in (0,0,0) do docker run -it ibmcom/secure-gateway-client <id_gateway>
```
{: pre}

## Messaggio di errore di connessione che indica che l'host non è nel CN del certificato
{: #not-in-cn}

### Che sta accadendo
{: #not-in-cn-what-is-happening}
Stai provando a implementare una TLS lato client in loco utilizzando il client Secure Gateway e ricevi il seguente messaggio di errore.

```
[ERROR] Connection #<connection ID> had error: Host: <host name>. is not cert's CN: <mycommonname>

Where:
    - connection ID is a client assigned connection number.
    - host name is the host name for the connection.
    - mycommonname is the FDQN or common name that is used in the certificate.
```
{: screen}

### Perché sta accadendo
{: #not-in-cn-why-it-is-happening}
Non c'è corrispondenza tra
il nome comune, ad esempio l'FQDN server o il TUO nome, dell'applicazione installata in loco e del certificato
caricato in {{site.data.keyword.Bluemix_notm}} per
questa destinazione.

- Controlla quanto segue:
- Hai generato correttamente il certificato e hai utilizzato
il nome host o l'FQDN server corretto.
- Hai caricato il certificato corretto nella tua destinazione {{site.data.keyword.Bluemix_notm}}
per il client.

### Come correggerlo
{: #not-in-cn-how-to-fix-it}

 1. Nell'IU {{site.data.keyword.Bluemix_notm}}, vai al dashboard Secure Gateway.
 2. Seleziona la tua destinazione e fai clic sull'icona Modifica.
 3. Fai clic su Carica certificato.
 4. Carica il file di certificato PEM da utilizzare per la connessione
al sistema installato in loco.

## L'IP non è nell'elenco del certificato
{: #san}

### Che sta accadendo
{: #san-what-is-happening}
Il CN nel certificato presentato è l'indirizzo IP del gateway, ma il certificato non ha un SAN che corrisponde all'indirizzo IP e il client non riesce a connettersi.  

A causa di problemi di risoluzione del nome host, stiamo usando l'indirizzo IP nella nostra destinazione.  Il CN nel certificato presentato è l'indirizzo IP del gateway, ma il certificato non ha un SAN che corrisponde all'indirizzo IP e il client non riesce a connettersi

Hai creato una destinazione utilizzando TLS ma invece di utilizzare il nome host della destinazione hai utilizzato il suo indirizzo IP.  In fase di connessione del client, viene generato il seguente errore:

```
[2015-10-15 13:00:04.866] [INFO] Connection #0 is being established to 10.3.20.31:443
[2015-10-15 13:00:05.426] [ERROR] Connection #0 to destination 10.3.20.31:443 had error: IP: 10.3.20.31 is not in the cert's list:
[2015-10-15 13:00:05.427] [INFO] Connection #0 to 10.3.20.31:443 was closed
```
{: screen}

### Perché sta accadendo
{: #san-why-it-is-happening}
Ciò che sta accadendo è che il codice di verifica SSL nel client gateway sta trattando questa destinazione in modo diverso,
perché utilizza un indirizzo IP anziché un nome host.  Invece della corrispondenza con il CN del
certificato, sta cercando una corrispondenza dell'indirizzo IP nel SAN del certificato.  Poiché non vi è alcun SAN nel certificato, questa condizione viene rilevata come una connessione non valida e l'handshake SSL non riesce.

### Come correggerlo
{: #san-how-to-fix-it}
Se guardi il messaggio di errore, non parla di CN, (ad es. [ERROR] Connection ## had error: Host: . is not cert&apos;s CN: ) bensì di elenco del certificato, il che mi porta a credere che tu abbia generato il tuo certificato autofirmato in modo non corretto. Il problema riguarda il certificato generato utilizzando un FQDN o un CN con un indirizzo IP, ciò non funzionerà in quanto gli indirizzi IP sono supportati solo quando si utilizza SAN.

Metodo per la generazione di un certificato con un IP come CN con openssl:

1. Crea un file openssl config. Io ho copiato il mio da /usr/lib/ssl/openssl.cnf
2. Aggiungi una sezione alternate_name al file nel modo indicato di seguito:

   ```
   [ alternate_names ]

   IP.1 = &lt;my application&apos;s ip&gt;
   ```
   {: codeblock}

3. Nella sezione [ v3_ca ], aggiungi questa riga:

    ```
    subjectAltName = @alternate_names
    ```
    {: pre}

4. Sotto la sezione CA_default, rimuovi l'indicazione come commento di copy_extensions (opzione di copia delle estensioni: va usata con cautela):

    ```
    copy_extensions = copy
    ```
    {: pre}

5. Genera la chiave privata

    ```
    openssl genrsa -out private.key 3072
    ```
    {: pre}

6. Genera il certificato con le opzioni relative all'organizzazione

    ```
    openssl req -new -x509 -key private.key -sha256 -out certificate.pem -days 730 -config
    ```
    {: pre}

7. Combina i file

    ```
    cat private.key certificate.pem > SAN.pem
    ```
    {: pre}

8. Carica il file SAN.pem nella tua nuova destinazione come certificato TLS del client.
9. Carica il file SAN.pem nella tua applicazione installata in loco e riavvia.

10. La tua destinazione può essere configurata per TCP, HTTP o HTTPS e la tua applicazione lato client dovrebbe ora essere in grado di connettersi alla tua applicazione in loco.

Se riscontri un problema UNABLE_TO_VERIFY_LEAF_SIGNATURE , controlla il tuo file openssl.conf e modifica quanto segue da:

    ```
    keyUsage = digitalSignature, keyEncipherment
    ```
    {: pre}

al valore predefinito:

    ```
    keyUsage = nonRepudiation, digitalSignature, keyEncipherment
    ```
    {: pre}

## Messaggio di errore di connessione: DEPTH_ZERO_SELF_SIGNED_CERT
{: #depth-zero}

### Che sta accadendo
{: #depth-zero-what-is-happening}
Stai provando a implementare una TLS lato client in loco utilizzando il client Secure Gateway e ricevi il seguente messaggio di errore.

```
[ERROR] Connection #<connection ID> had error: DEPTH_ZERO_SELF_SIGNED_CERT Where:

    connection ID is a client assigned connection number.
```
{: screen}

### Perché sta accadendo
{: #depth-zero-why-it-is-happening}
Alla destinazione definita manca un certificato lato client.

### Come correggerlo
{: #depth-zero-how-to-fix-it}
 1. Nell'IU {{site.data.keyword.Bluemix_notm}}, vai al dashboard Secure Gateway.
 2. Seleziona la tua destinazione e fai clic sull'icona Modifica.
 3. Fai clic su Carica certificato.
 4. Carica il file di certificato PEM da utilizzare per la connessione
al sistema installato in loco.


## Come posso caricare in modo interattivo un file ACL nel client Docker?
{: #docker-load-acl}

### Che sta accadendo
{: #docker-load-acl-what-is-happening}
Poiché Docker è un contenitore o un ambiente virtualizzato, non ha accesso diretto al tuo filesystem finché il contenitore non viene effettivamente avviato.  Ciò gli impedisce di leggere il filesystem delle tue macchine host finché non viene effettivamente avviato ed è in esecuzione.

### Come correggerlo
{: #docker-load-acl-how-to-fix-it}
Ecco cosa puoi fare:

- Crea un Dockerfile per includere l'aclfile.txt

```
FROM ibmcom/secure-gateway-client
ADD aclfile.txt /tmp/aclfile.txt
```
{: codeblock}

- Crea una nuova immagine docker

```
docker build -t ads-secure-gateway-client .
```
{: pre}

- Esegui la nuova immagine docker (devi specificare le opzioni -t e -i, altrimenti otterrai un errore di file non trovato oppure ENOENT):

```
docker run -t -i ads-secure-gateway-client1 --F /tmp/aclfile.txt
```
{: pre}

- Dovresti ricevere il seguente output:

```
[2015-09-30 16:50:32.084] [INFO] The current access control list is being reset and replaced by the user provided file: /tmp/aclfile.txt
[2015-09-30 16:50:32.086] [INFO] The ACL file process accepts acl allow :8000
[2015-09-30 16:50:32.087] [INFO] The ACL file process accepts acl deny local
```
{: screen}

## Come ottenere assistenza e supporto aggiuntivi
{: #getting-help-and-support}

Se hai domande tecniche sullo sviluppo o la distribuzione di un'applicazione con Secure Gateway, inserisci la tua domanda in [Stack Overflow ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://stackoverflow.com/search?q=securegateway+ibm-bluemix).  Contrassegna la tua domanda con "ibm-bluemix" e "secure-gateway" in modo che possa essere trovata più agevolmente dai team di sviluppo di {{site.data.keyword.Bluemix_notm}}.

Se hai domande sul servizio o sulle istruzioni introduttive, usa il forum [IBM developerWorks dW Answers ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/answers/topics/securegateway/?smartspace=bluemix) insieme alle tag "bluemix" e "Secure Gateway".

Per ulteriori dettagli sull'utilizzo di questi forum, verifica la [pagina Come ottenere supporto qui](https://cloud.ibm.com/docs/get-support?topic=get-support-getting-customer-support#using-avatar).

Per informazioni su come aprire un ticket di supporto IBM o sui livelli di supporto e sulla
gravità dei ticket, consulta [Come contattare il
supporto](https://cloud.ibm.com/docs/get-support?topic=get-support-support-case-severity#support-case-severity).

Quando inoltri un ticket, fornisci quante più informazioni puoi delle seguenti:

- Su quale sistema operativo è in esecuzione il client?
- Quale versione del client è utilizzata (è possibile trovarla utilizzando il comando 'C' sul client)?
- Se si tratta di un problema dell'IU, incolla o allega le eventuali acquisizioni di schermo e gli eventuali log della console del browser associati
- Incolla o allega gli eventuali log e fuso orario dell'applicazione richiedente associati
- Incolla o allega gli eventuali log e fuso orario del client Secure Gateway associati
- Fornisci i dettagli della destinazione utilizzata (un'acquisizione di schermo oppure compila i campi indicati di seguito):
   - ID destinazione
   - Protocollo
   - Autenticazione lato destinazione
   - Certificati caricati (solo i nomi e il link o l'indicazione della cartella Box su cui sono stati caricati)
