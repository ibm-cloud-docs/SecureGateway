---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---

# Requisiti per l'esecuzione del client
{: #system-requirements}

## Requisiti di sistema
{: #system}

Il client Secure Gateway è supportato nei seguenti ambienti:

| Nome | Versioni          |
| ------------- | ----------- |
| Windows Desktop | 7, 8.1, 10 e superiore |
| Windows Server | 2012 R2 e superiore |
| Red Hat Linux | 6.5 e superiore |
| CentOS | 7.2 e superiore |
| SuSE Linux | 11.0<sup>*</sup> e superiore |
| Ubuntu Linux | 14.04 (LTS) e superiore |
| Power Machine | Architettura ppc64el |
| Ubuntu Z-Linux | - |
| AIX | 7.1 e superiore |
| Mac OS X | 10.10 (Yosemite) e superiore |
| Docker | 1.7.0 e superiore, tutti i sistemi operativi supportati |

<sup> * </sup>- Il supporto SLES 11 è solo con il Service Pack 4

<b>Nota:</b> solo gli ambienti a 64 bit sono attualmente supportati per l'installazione del client nativo.

## Requisiti di rete
{: #network}

Il client Secure Gateway utilizza la porta in uscita 443 e la porta 9000 per stabilire una connessione al registro npm e all'ambiente {{site.data.keyword.Bluemix}}:
- Porta 443 per l'installazione npm
  - Durante l'installazione, il programma di installazione si connetterà al registro npm ed eseguirà `npm install` per installare le dipendenze richieste dal client Secure Gateway. Prima dell'installazione, assicurati che la macchina su cui verrà installato il client possa connettersi a un sito web di registro npm. Per impostazione predefinita, npm è configurato per utilizzare il registro pubblico di npm, Inc. all'indirizzo https://registry.npmjs.org. <br><br>
Se nel tuo ambiente è presente il server Enterprise di npm, inserisci in whitelist tutte le dipendenze del client Secure Gateway sul server Enteprise di npm. Per l'elenco di dipendenze, fai riferimento al file `<Installation_directory>\ibm\securegateway\client\package.json`.<br><br>

- Porta 443 per l'autenticazione del gateway


  | Regione  | Host  |
  | --  | --  |
  | Stati Uniti Sud  | sgmanager.ng.bluemix.net  |
  | Stati Uniti Est | sgmanager.us-east.bluemix.net  |
  | Regno Unito | sgmanager.eu-gb.bluemix.net  |
  | Germania  | sgmanager.eu-de.bluemix.net  |
  | Sydney  | sgmanager.au-syd.bluemix.net  |


- Porta 9000

  Il nodo del gateway SC, disponibile nella configurazione del gateway; ciascun gateway non sarà sullo stesso nodo; conferma il nome host del nodo ogni volta che crei il gateway.


Accertati di verificare o modificare le regole supplementari della tabella IP del firewall eventualmente applicabili.Se i tuoi amministratori di rete richiedono degli IP specifici, [contatta il supporto per richiederli per il tuo ambiente](/docs/services/SecureGateway/securegateway_troubleshooting.html#support).


## Determinazione dei requisiti hardware
{: #hardware}

Le specifiche della macchina che esegue il client Secure Gateway dipendono in ampia misura dal traffico che transiterà attraverso ciascuna connessione. Ogni istanza del client è un processo individuale che fornisce fino a 250 connessioni contemporanee.  Per stimare una quantità massima che, in media, la macchina dovrebbe supportare, determina la dimensione media di una transazione nel client (sia la richiesta che la risposta) e quindi ridimensionala a 250 transazioni simultanee. Dato che questo numero ha combinato entrambe le dimensioni di richiesta e risposta, il client non dovrebbe superare questo footprint di memoria. Per delle stime più precise, dovrebbe essere utilizzato un misto di dimensioni di richiesta e risposta per simulare meglio uno scenario di transazione reale.

Per eseguire una singola istanza del client Secure Gateway, consigliamo un minimo di 2 core e 4 GB di memoria.

Per gli scenari HA che eseguiranno più istanze del client sulla stessa macchina, consigliamo un minimo di 4 core e 8 GB di memoria.
