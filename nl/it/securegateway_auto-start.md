---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Configurazione dell'avvio automatico per il client

## Linux
{: #linux}

Se hai scelto di utilizzare la funzione di avvio automatico del tuo sistema, usa uno dei metodi indicati di seguito per avviare il client. Il file di configurazione che verrà utilizzato dal client è disponibile in:

```
/etc/ibm/sgenvironment.conf
```
{: screen}

<b>Nota:</b> la funzionalità di avvio automatico non è disponibile per Red Hat Linux 6.

Questo file include le seguenti importanti variabili da impostare:

| Variabile di ambiente | Descrizione       |
| ------------- | ----------- |
| RESTART_CLIENT | Arresta e riavvia il client durante l'installazione o l'upgrade (Yes o No)|
| GATEWAY_ID | Il tuo ID gateway come è stato creato nell'IU di Secure Gateway for Bluemix |
| SECTOKEN | Token di sicurezza per questo ID gateway, se scegli di implementare la sicurezza quando lo crei. |
| ACL_FILE | File ACL (Access Control List) che vuoi utilizzare per limitare l'accesso in loco alle risorse.|
| LOGLEVEL | Il livello di log che vuoi impostare per il tuo servizio (il valore predefinito è INFO) |
| USE_UI   | Imposta questa variabile su 'N' se non vuoi avviare l'IU client |
| UI_PORT  | La porta su cui vuoi avviare l'IU client (il valore predefinito è 9003) |
| LANGUAGE | La lingua in cui desideri siano i log client (il valore predefinito è en) |

<b>Nota:</b> questo file viene letto solo se stai utilizzando la funzione di avvio automatico del tuo sistema. Se stai eseguendo il client manualmente, questo file viene ignorato.

### Upstart
{: #upstart}

### Avvio del client
{: #upstart-start}

Se stai utilizzando la funzionalità upstart affinché il client Secure Gateway venga eseguito automaticamente all'avvio del sistema, devi controllare prima la sua configurazione (/etc/ibm/sgenvironment.conf). Dopo che hai configurato il servizio upstart, puoi utilizzare il seguente comando per avviarlo:

```
sudo initctl start securegateway_client
```
{: pre}

Per riavviare il servizio:

```
sudo initctl restart securegateway_client
```
{: pre}

### Arresto del client
{: #upstart-stop}

Per arrestare il servizio, esegui questo comando:

```
sudo initctl stop securegateway_client
```
{: pre}

### SystemD
{: #systemd}


### Avvio del client
{: #systemd-start}

Se stai utilizzando la funzionalità systemD affinché il client Secure Gateway venga eseguito automaticamente all'avvio del sistema, devi controllare prima la sua configurazione (/etc/ibm/sgenvironment.conf). Dopo che hai configurato il servizio upstart, puoi utilizzare il seguente comando per avviarlo:

```
systemctl start securegateway_client
```
{: pre}

Per riavviare il servizio:

```
systemctl restart securegateway_client
```
{: pre}

Per lo stato sul servizio:

```
systemctl status securegateway_client
```
{: pre}

### Arresto del client
{: #systemd-stop}

Per arrestare il servizio, esegui questo comando:

```
systemctl stop securegateway_client
```
{: pre}

### SystemV
{: #systemv}

System V non viene configurato durante l'installazione come le altre funzioni di avvio automatico. Nella directory di installazione /opt/ibm/securegateway/client/upstart sono disponibili gli script:

```
secgw.sh
securegateway_clientd
```
{: screen}

<b>Nota:</b> questa sezione interesserà gli utenti di SuSE/SLES 11.

Facoltativo: se stai eseguendo SuSE versione 11 che utilizza l'avvio automatico di systemV, vengono forniti degli script che possono essere utilizzati per configurare questo processo. Attieniti alla seguente procedura:

```
cd /opt/ibm/securegateway/client/upstart/systemV
cp secgw.sh /usr/local/bin
chmod +x /usr/local/bin/secgw.sh
cp securegateway_clientd /usr/local/bin
chmod +x /usr/local/bin/securegateway_clientd
cd /etc/init.d
ln -s /usr/local/bin/securegateway_clientd
insserv securegateway_clientd
vi /etc/ibm/sgenvironment.conf
```
{: codeblock}

Una volta eseguiti questi passi, puoi utilizzare i comandi YasT e systemV per avviare/arrestare il daemon.

Ritorna a [Introduzione - Aggiunta di un client](./securegateway_client.html).

## Windows
{: #windows}

Per modificare lo stato del servizio Windows, apri una finestra comandi con privilegi di amministratore.

```
cd "<Installation_directory>\ibm\securegateway\client"
```
{: pre}

Se il servizio è già in esecuzione, puoi utilizzare il comando indicato di seguito per arrestarlo

```
windowsService.cmd uninstall
```
{: pre}

Per avviare il servizio, utilizza il seguente comando

```
windowsService.cmd install
```
{: pre}

<b>Nota:</b> il servizio verrà eseguito in base alle configurazioni archiviate in:

```
%Installation_directory%/ibm/securegateway/client/securegw_service.config
```
{: pre}

I log dell'applicazione per il servizio Windows verranno archiviati in:

```
%Installation_directory%/ibm/securegateway/client/logs/securegw_win_service.log
```
{:pre}

 I log vengono assemblati in un nuovo file su base giornaliera.

Ritorna a [Introduzione - Aggiunta di un client](./securegateway_client.html).
