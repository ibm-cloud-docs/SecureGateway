---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# ACL (Access Control List)

Il client {{site.data.keyword.SecureGateway}} fornisce il supporto ACL (Accesss Control List) integrato. Puoi consentire o limitare (rifiutare) l'accesso alle risorse in loco apportando modifiche all'ACL per il client. Tale operazione può essere eseguita interattivamente utilizzando i comandi client oppure specificando un file che contiene gli ACL che desideri siano implementati.

A partire dalla versione 1.5.0, le regole dell'ACL (Access Control List) saranno sincronizzate tra tutti i client connessi allo stesso gateway. In questo modo, hai bisogno solo di stabilire/aggiornare il tuo ACL da un singolo client che verrà quindi condiviso tra tutti i client in esecuzione connessi a tale gateway. L'ACL verrà
mantenuto anche tra le sessioni, in modo tale che la connessione di un nuovo client applicherà anche le stesse
regole ACL.

## Comandi ACL (Access Control List)
{: #commands}

I comandi ACL supportati sono:

```
acl allow [<nomehost>]:[<porta>]
acl deny [<nomehost>]:[<porta>]
no acl [<nomehost>]:[<porta>]
no acl
show acl
```
{: screen}

I formati in cui hai tralasciato un nome host o una porta implicano tutti i nomi host o tutte le porte. Ad esempio, la seguente è una regola ACL per consentire tutti i nomi host per la porta 22.

```
acl allow :22
```
{: pre}

La seguente è un'altra regola ACL di esempio per consentire tutti i nomi host per tutte le porte, in sostanza disabilitando il supporto ACL. <b>Ciò è sconsigliato</b>

```
acl allow :
```
{: pre}

Il comando `show acl` mostrerà l'ACL attualmente impostato oppure fornirà un messaggio relativo all'impostazione complessiva.

Ritorna a [Introduzione - Aggiunta di un client](/docs/services/SecureGateway/securegateway_client.html).

## Controllo della rotta HTTP/S utilizzando l'ACL
{: #routes}

A partire dalla versione 1.6.0, le destinazioni HTTP/S possono anche implementare specifiche rotte sulle voci ACL. Vengono aggiunte nello stesso modo delle voci ACL tipiche, ma con il percorso accodato alla fine della regola. Ad esempio, quanto segue consentirà il transito solo alle richieste che seguono il percorso /my/api:

```
acl allow localhost:80/my/api
```
{: pre}

Con questa regola implementata, sarà consentito il transito alle richieste a `<cloud host>:<cloud port>/my/api*`.

Le rotte sono supportate solo sui comandi `acl allow`.

## Precedenza dell'ACL (Access Control List)
{: #precedence}

Dopo che hai fornito un comando `acl allow :`, se vengono immessi altri comandi `acl
allow`, la regola allow `ALL:ALL` (da `acl
allow:`) verrà rimossa dall'elenco, sulla base del presupposto che non desideri più consentire l'accesso
illimitato. Dopo che hai fornito un comando `acl deny :`, se viene immesso un altro comando `acl deny`,
la regola deny `ALL:ALL` (da `acl deny:`) verrà
rimossa dall'elenco, sulla base del presupposto che non desideri più limitare tutto l'accesso. Se
elenchi le tue regole ACL correnti tramite il comando `show acl` nella CLI, ci sarà
un indicatore che mostra se le regole non elencate verranno consentite o respinte.

## Importazione di un file ACL
{: #import}

Puoi fornire un nome file al comando `acl file` che contiene i comandi ACL supportati che verranno letti dal client all'avvio. Questo file dovrebbe avere i comandi nel seguente formato:

```
acl allow [<hostname>]:[<port>]
acl deny [<hostname>]:[<port>]
no acl [<hostname>]:[<port>]
no acl
```
{: screen}

<b>Nota:</b> `no acl` senza alcun altro parametro REIMPOSTA la tabella ACL e imposta l'accesso su DENY ALL.

Puoi trovare un file ACL di esempio [qui](/docs/services/SecureGateway/securegateway_acl-file.html).

Ritorna a [Introduzione - Aggiunta di un client](/docs/services/SecureGateway/securegateway_client.html).

## Copia del file ACL nel client Docker {{site.data.keyword.SecureGateway}}
{: #docker}

Il client docker {{site.data.keyword.SecureGateway}} in sostanza viene eseguito nel suo contenitore di virtualizzazione. Il filesystem della macchina host non è pertanto direttamente accessibile ai processi eseguiti all'interno del contenitore, compreso il client {{site.data.keyword.SecureGateway}}.  A partire dalla versione 1.8.0 del motore Docker, puoi utilizzare
il comando 'docker cp' per eseguire il push dei file presenti nel tuo host al contenitore mentre è in esecuzione o arrestato. Ciò deve essere fatto per utilizzare il comando interattivo ACL FILE del client {{site.data.keyword.SecureGateway}}.

Per utilizzare il supporto 'cp' interattivo in docker dal tuo host all'istanza docker, devi essere al docker 1.8.0. Può verificarlo utilizzando `docker --version`

Una volta fatto ciò, la tua versione dovrebbe essere visualizzata come segue. TI consigliamo di consentire al docker l'esecuzione come utente non root, quindi esegui il comando consigliato dopo che hai eseguito l'upgrade del tuo motore alla 1.8.0 o alla 1.8.2.

```
Client:
 Version:      1.8.2
 API version:  1.20
 Go version:   go1.4.2
 Git commit:   0a8c2e3
 Built:        Thu Sep 10 19:21:21 UTC 2015
 OS/Arch:      linux/amd64

Server:
 Version:      1.8.2
 API version:  1.20
 Go version:   go1.4.2
 Git commit:   0a8c2e3
 Built:        Thu Sep 10 19:21:21 UTC 2015
 OS/Arch:      linux/amd64
```
{: screen}

Quindi, per eseguire il push del tuo file acl all'immagine docker, attieniti alla seguente procedura:

- Esegui il comando 'docker ps' per trovare il tuo ID contenitore

```
CONTAINER ID IMAGE                        COMMAND                CREATED        STATUS  PORTS NAMES
764aadce386b ibmcom/secure-gateway-client "node lib/secgwclient" 27 seconds ago Up 26 seconds condescending_nobel
```
{: screen}

- Copia il tuo elenco acl con il comando 'docker cp' utilizzando l'ID o il nome contenitore:

```
docker cp 01_client.list 764aadce386b:/root/01_client.list
```
{: pre}

- Quindi, nel client {{site.data.keyword.SecureGateway}} in esecuzione nel docker:

```
cli F /root/01_client.list

 [2015-10-01 08:12:30.091] [INFO] The current access control list is being reset and replaced by the user provided file: /root/01_client.list
 [2015-10-01 08:12:30.093] [INFO] The ACL file process accepts acl allow 127.0.0.1:27017
 [2015-10-01 08:12:30.094] [INFO] The ACL file process accepts acl allow 127.0.0.1:22
```
{: screen}

- Visualizza l'ACL:

```
cli> S
 -------------------------------------------------------------------
           -- Secure Gateway Client Access Control List --

  hostname                               port                  value
  127.0.0.1                             27017                  Allow
  127.0.0.1                                22                  Allow
 -------------------------------------------------------------------
```
{: screen}

Ritorna a [Introduzione - Aggiunta di un client](/docs/services/SecureGateway/securegateway_client.html).
