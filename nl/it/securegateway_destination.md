---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---

# Aggiunta di una destinazione
{: #add-dest}

Una destinazione è una definizione di come stabilire una connessione a una specifica risorsa in loco o cloud. Dopo che la destinazione è stata creata, i server {{site.data.keyword.SecureGateway}} forniranno ad essa un endpoint pubblico univoco dove sarà in ascolto per le connessioni mentre il gateway è connesso.

![Dashboard senza alcuna destinazione](./images/emptyDestinations.png?raw=true "Dashboard senza alcuna destinazione")

Dall'interno del tuo nuovo gateway e nella scheda Destinations, fai clic sul pulsante Add Destination per aprire il pannello Add Destination.  Ci sono due metodi per creare la tua destinazione: una configurazione guidata che mostra come ogni parte si inserisce nell'architettura complessiva e una configurazione avanzata che fornisce tutti i campi e tutte le opzioni in un singolo pannello.  

La configurazione guidata <b>non</b> consente la configurazione delle informazioni sul proxy, gli indicatori di nome server o il caricamento di una coppia certificato/chiave specifica per la destinazione.  Dopo la creazione, tutti i campi sono disponibili tramite il pannello Edit Destination.

## Pannello di configurazione guidato
{: #add-dest-guided-setup}

![Guided Setup](./images/guidedLanding.png?raw=true "Pannello di destinazione Guided Setup")

## Pannello di configurazione avanzato
{: #add-dest-advanced-setup}

![Advanced Setup](./images/advancedLanding.png?raw=true "Pannello di destinazione Advanced Setup")

## Confronto tra destinazioni in loco e cloud
{: #dest-types}

La prima domanda a cui rispondere quando crei la tua destinazione è dove si trova la risorsa a cui ti devi connettere.

### Destinazione in loco
{: #dest-types-on-prem}
La destinazione in loco è per il caso d'uso in cui un'applicazione nello spazio pubblico deve accedere a una risorsa con restrizioni ubicata in loco.
![Destinazione in loco](./images/onPremDestination.png?raw=true "Destinazione in loco")

### Destinazione cloud
{: #dest-types-on-cloud}
La destinazione cloud è per il caso d'uso in cui un'applicazione ubicata in una rete con restrizioni deve accedere a una risorsa disponibile in uno spazio pubblico.
![Destinazione inversa](./images/reverseDestination.png?raw=true "Destinazione cloud")

## Definizione della tua destinazione
{: #define-dest}
Per entrambi i tipi di destinazioni sono necessarie le seguenti informazioni:

- <b>Resource Hostname</b>: questo è l'IP o il nome host della risorsa a cui hai bisogno di connetterti.
- <b>Resource Port</b>: questa è la porta su cui la tua risorsa è in ascolto.
- <b>Protocol</b>: questo è il tipo di connessione che la tua applicazione stabilirà.  Vedi la tabella di seguito per le diverse [opzioni di protocollo](#protocols-options).  Per configurare il tipo di connessione previsto dalla tua risorsa, controlla la sezione [Resource authentication](#dest-resource-auth).

Se hai selezionato una destinazione cloud, dovrai fornire anche una <b>Client Port</b>.  Questa è la porta su cui il client {{site.data.keyword.SecureGateway}} sarà in ascolto per consentire le connessioni al nome host e alla porta risorsa associate.

## Opzioni di protocollo
{: #protocols-options}

Questa tabella fornisce tutte le opzioni disponibili relative ai modi in cui la tua applicazione può avviare connessioni/richieste con {{site.data.keyword.SecureGateway}}.

Protocollo | Descrizione
-- | --
TCP | Nessuna autenticazione. La tua applicazione può comunicare direttamente con i server {{site.data.keyword.SecureGateway}} senza richiedere alcun certificato.
TLS: Server Side | TLS (Transport Layer Security) è abilitato e il server fornisce un certificato per provare la sua autenticità.
TLS: Mutual Auth | Il server fornisce una serie di certificati e la tua applicazione deve anche fornire un certificato come parte della sua connessione.
HTTP | La connessione TCP in cui viene riscritta l'intestazione host in modo da corrispondere al nome host in loco.
HTTPS | La connessione TLS: Server Side in cui viene riscritta l'intestazione host in modo da corrispondere al nome host in loco.
HTTPS: Mutual Auth | La connessione TLS: Mutual Auth in cui viene riscritta l'intestazione host in modo da corrispondere al nome host in loco.


## Configurazione dell'autenticazione reciproca
{: #dest-mutual-auth}

Per i protocolli che implementano l'autenticazione reciproca, dovrai caricare il tuo certificato, altrimenti il server creerà automaticamente una coppia certificato/chiave autofirmata disponibile per l'utilizzo da parte della tua applicazione.  La coppia può essere scaricata insieme al certificato server.
![Pannelli Mutual Authentication](./images/mutualAuth.png?raw=true "Pannelli Mutual Authentication")

### User Authentication
{: #dest-user-auth}

La sezione User Authentication serve a gestire l'autorizzazione della tua applicazione richiedente, o che sta stabilendo una connessione, presso {{site.data.keyword.SecureGateway}}.  Questo campo accetta un singolo certificato che dovrebbe essere il certificato che la tua applicazione presenterà insieme a qualsiasi connessione/richiesta.

### Resource Authentication
{: #dest-resource-auth}

Resource Authentication determina il modo in cui {{site.data.keyword.SecureGateway}} proverà a stabilire una connessione alla risorsa definita.  Sono disponibili tre opzioni: None, TLS (server-side) e Mutual Auth. A seconda della tua scelta, diventano disponibili opzioni di autenticazione differenti.

L'abilitazione della TLS sulla connessione alla tua risorsa è separata dalla TLS utilizzata per User Authentication.  La TLS per User Authentication protegge l'accesso tra la tua applicazione richiedente iniziale e {{site.data.keyword.SecureGateway}} (ad es. tra la tua applicazione {{site.data.keyword.Bluemix_notm}} e i server {{site.data.keyword.SecureGateway}}) mentre la TLS per Resource Authentication protegge la connessione tra {{site.data.keyword.SecureGateway}} e la tua risorsa definita (ad es. tra il client {{site.data.keyword.SecureGateway}} e il tuo database in loco).

#### Cloud/On-Premises Authentication
{: #cloud-or-on-prem-auth}

Questa opzione diventa disponibile selezionando TLS o Mutual Auth per [Resource Authentication](#dest-resource-auth).  Il nome del campo corrisponderà al [tipo di destinazione](#dest-types) che hai scelto.  Questo campo consente di caricare fino a 6 certificati per convalidare il certificato della risorsa a cui ti stai connettendo.  Questi file verranno aggiunti alla CA di connessione alla risorsa e devono contenere il certificato o la catena di certificati che la tua risorsa presenterà.

#### Server Name Indicator (SNI)
{: #dest-sni}
Questa opzione diventa disponibile selezionando TLS o Mutual Auth per [Resource Authentication](#dest-resource-auth).  Viene utilizzata per consentire di fornire un nome host separato all'handshake TLS della connessione della risorsa.

### Client Cert and Key
{: #dest-client-cert-key}
La visualizzazione o meno dei campi Client Certificate e Key dipende dal [tipo di destinazione](#dest-types) che hai scelto.  In entrambe le situazioni, i file qui forniti verranno utilizzati dal client SG per identificarsi per le connessioni TLS.  Se non viene caricato alcun file, i server {{site.data.keyword.SecureGateway}} genereranno automaticamente una coppia autofirmata con un CN `localhost`.  Per istruzioni su come generare una coppia certificato/chiave, [fai clic qui](/docs/services/SecureGateway?topic=securegateway-cert-key-management).

Per una destinazione in loco, comparirà sotto [Resource Authentication](#dest-resource-auth), se è stata selezionata `Resource Authentication: Mutual Auth`.  In questo caso, il client SG utilizzerà questa coppia certificato/chiave per la sua connessione in uscita alla risorsa definita, la risorsa in loco deve aggiungere questo certificato alla propria CA per comunicare con il client SG.

Per una destinazione cloud, comparirà sotto [User Authentication](#dest-user-auth), se è stato selezionato un protocollo TLS.  In questo caso, il client SG utilizzerà questa coppia certificato/chiave per creare i listener TLS, l'applicazione in loco deve aggiungere questo certificato alla propria CA per comunicare con il client SG.

## Configurazione della sicurezza di rete
{: #dest-network-security}
Per impedire a tutti gli indirizzi IP tranne quelli specificamente indicati di connettersi ai tuoi host e porte cloud, puoi scegliere di implementare delle regole iptables sulla tua destinazione in loco.
![Pannello Network Security](./images/networkSecurity.png?raw=true "Pannello Network Security")

Per implementare le regole iptables, seleziona la casella <b>Restrict cloud access to this destination with iptables rules</b> dal pannello Network Security.  Dopo che la casella è stata selezionata, puoi iniziare ad aggiungere gli IP a cui deve essere consentita la connessione.  Se non viene fornito alcun IP, tutte le connessioni a questi host e a queste porte cloud verranno rifiutate fintanto che la casella <b>Restrict cloud access</b> sarà selezionata.

<b>Nota</b>: gli IP o le porte forniti devono essere l'indirizzo IP esterno che i server {{site.data.keyword.SecureGateway}} vedranno, non l'indirizzo IP locale della macchina che effettua la richiesta.

### Aggiunta di regole iptables
{: #dest-iptables}
Quando aggiungi regole alle iptable, puoi fornire singoli IP oppure un intervallo di IP insieme a una singola porta o a un intervallo di porte.  Tutti gli intervalli forniti sono inclusivi.  la seguente tabella presenta alcuni esempi, nonché il modo in cui verranno risolti all'interno delle iptable:

Indirizzi IP | Porte | Risultati
-- | -- | --
1.2.3.4 | 5000 | Sarà consentito solo IP 1.2.3.4 dalla porta 5000.
1.2.3.4-2.3.4.5 | 5000 | Saranno consentiti tutti gli IP tra 1.2.3.4 e 2.3.4.5 dalla porta 5000.
1.2.3.4 | 5000:10000 | Sarà consentito solo l'IP 1.2.3.4 dalle porte che vanno da 5000 a 10000.
1.2.3.4-2.3.4.5 | 5000:10000 | Saranno consentiti tutti gli IP tra 1.2.3.4 e 2.3.4.5 dalle porte che vanno da 5000 a 10000.
1.2.3.4 | | Sarà consentito solo l'IP 1.2.3.4 da qualsiasi porta.
| 5000 | Sarà consentito qualsiasi IP dalla porta 5000.

È anche possibile associare delle regole specifiche a un'applicazione.  Per ulteriori informazioni sulla creazione di regole associate, vedi il documento relativo a [come creare regole iptables per la tua applicazione](/docs/services/SecureGateway?topic=securegateway-iptables-rules).

## Configurazione delle opzioni proxy
{: #dest-proxy}
Se la destinazione in loco si trova dietro un proxy SOCKS, puoi configurare le impostazioni proxy per la tua destinazione nel pannello Proxy Options.
![Pannello Proxy Options](./images/proxyOptions.png?raw=true "Pannello Proxy Options")

Per configurare le impostazioni proxy, devi solo fornire il nome host e la porta su cui il proxy è in ascolto, nonché il protocollo SOCKS utilizzato (4, 4a, 5).

## Impostazioni della destinazione
{: #dest-settings}
Dopo che la tua destinazione è stata creata, fai clic sull'icona delle impostazioni per visualizzare le seguenti informazioni:

- L'ID destinazione richiesto per l'utilizzare l'API.
- <b>Le destinazioni in loco avranno una porta e un host cloud.  Queste informazioni sono richieste dalla tua applicazione per stabilire una connessione alla tua risorsa in loco.</b>
- <b>Le destinazioni cloud avranno una porta client.  Questa è la porta su cui il client {{site.data.keyword.SecureGateway}} sarà in ascolto per connettere la tua applicazione in loco alla tua risorsa cloud.</b>
- L'host e la porta della risorsa a cui è orientata questa destinazione.
- La data di creazione della destinazione e l'utente che l'ha creata.
- La data dell'ultima modifica della destinazione e l'utente che l'ha modificata.
