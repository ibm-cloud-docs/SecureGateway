---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-07"

---

# Requisiti per l'esecuzione del client
{: #client-requirements}

## Requisiti di sistema
{: #system-requirements}

Il client Secure Gateway è supportato nei seguenti ambienti:

| Nome | Versioni          |
| ------------- | ----------- |
| Windows Desktop | 7, 8.1, 10 e superiore |
| Windows Server | 2012 R2 e superiore |
| Red Hat Linux | 7.3 e superiore |
| CentOS | 7.3 e superiore |
| SuSE Linux | 12 e superiore |
| Ubuntu Linux | 14.04 (LTS) e superiore |
| Power Machine | Architettura ppc64el |
| Ubuntu Z-Linux | - |
| AIX | 7.1 TL4 e superiore |
| Mac OS X | 10.10 (Yosemite) e superiore |
| Docker | 1.7.0 e superiore, tutti i sistemi operativi supportati |

<b>Nota:</b> solo gli ambienti a 64 bit sono attualmente supportati per l'installazione del client nativo.

## Requisiti di rete
{: #network-requirements}

Il client Secure Gateway utilizza la porta in uscita 443 e la porta 9000 per stabilire una connessione al registro npm e all'ambiente {{site.data.keyword.Bluemix}}:
- Porta `443` per l'installazione npm
  - Durante l'installazione, il programma di installazione si connetterà al registro npm ed eseguirà `npm install` per installare le dipendenze richieste dal client Secure Gateway. Prima dell'installazione, assicurati che la macchina su cui verrà installato il client possa connettersi a un sito web di registro npm. Per impostazione predefinita, npm è configurato per utilizzare il registro pubblico di npm, Inc. all'indirizzo `https://registry.npmjs.org`. <br><br>
Se nel tuo ambiente è presente il server Enterprise di npm, inserisci in whitelist tutte le dipendenze del client Secure Gateway sul server Enteprise di npm. Per l'elenco di dipendenze, fai riferimento al file `<Installation_directory>\ibm\securegateway\client\package.json`.<br><br>

- Porta `443` per l'autenticazione del gateway
  - Per il client SG `v180fp9 and former`


  | Regione  | Host  |
  | --  | --  |
  | Stati Uniti Sud  | sgmanager.ng.bluemix.net  |
  | Stati Uniti Est  | sgmanager.us-east.bluemix.net  |
  | Regno Unito  | sgmanager.eu-gb.bluemix.net  |
  | Germania  | sgmanager.eu-de.bluemix.net  |
  | Sydney  | sgmanager.au-syd.bluemix.net  |

  - Per il client SG `v181 and later`
  
  
  | Regione  | Host  |
  | --  | --  |
  | Stati Uniti Sud  | sgmanager.us-south.securegateway.cloud.ibm.com  |
  | Stati Uniti Est  | sgmanager.us-east.securegateway.cloud.ibm.com  |
  | Regno Unito  | sgmanager.eu-gb.securegateway.cloud.ibm.com  |
  | Germania  | sgmanager.eu-de.securegateway.cloud.ibm.com  |
  | Sydney  | sgmanager.au-syd.securegateway.cloud.ibm.com  |

- Porta `9000`

  Il nodo del gateway SG, disponibile nella configurazione del gateway. Poiché ciascun gateway non sarà sullo stesso nodo; conferma il nome host del nodo ogni volta che crei il gateway.


Accertati di verificare o modificare le regole supplementari della tabella IP del firewall eventualmente applicabili. Tuttavia, non ti consigliamo di configurare le regole per gli IP, le regole dovrebbero essere configurate specificatamente per il nome host e la porta poiché gli IP per l'autenticazione gateway e il gateway SG sono controllati da Bluemix e sono soggetti a modifiche. Se i tuoi amministratori di rete richiedono degli IP correnti per nomi host specifici, [contatta il supporto per richiederli per il tuo ambiente](/docs/services/SecureGateway/securegateway_troubleshooting.html#getting-help-and-support).


## Determinazione dei requisiti hardware
{: #hardware-requirements}

Le specifiche della macchina che esegue il client Secure Gateway dipendono in ampia misura dal traffico che transiterà attraverso ciascuna connessione.  Ogni istanza del client è un processo individuale che fornisce fino a 250 connessioni contemporanee.  Per stimare una quantità massima che, in media, la macchina dovrebbe supportare, determina la dimensione media di una transazione nel client (sia la richiesta che la risposta) e quindi ridimensionala a 250 transazioni simultanee. Dato che questo numero ha combinato entrambe le dimensioni di richiesta e risposta, il client non dovrebbe superare questo footprint di memoria.  Per delle stime più precise, dovrebbe essere utilizzato un misto di dimensioni di richiesta e risposta per simulare meglio uno scenario di transazione reale.

Per eseguire una singola istanza del client Secure Gateway, consigliamo un minimo di 2 core e 4 GB di memoria.

Per gli scenari HA che eseguiranno più istanze del client sulla stessa macchina, consigliamo un minimo di 4 core e 8 GB di memoria.
