---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Disinstallazione del client
{: #client-uninstall}

## Docker
{: #docker}

Per rimuovere l'immagine Docker del client {{site.data.keyword.SecureGateway}}, esegui questo comando:

```
docker rmi ibmcom/secure-gateway-client
```
{: pre}

## Mac OS X
{: #mac}

Per rimuovere il client {{site.data.keyword.SecureGateway}} da Mac OS X, esegui questi comandi dal terminale:

```
cd /Applications
rm -rf ./ibm
```
{: codeblock}

## Linux
{: #linux}

### Ubuntu/PowerPC
{: #debian-uninstall}

Per trovare lo stato di installazione del client {{site.data.keyword.SecureGateway}},
puoi utilizzare il seguente comando:

```
sudo dpkg -l | grep ibm-securegateway-client
```
{: pre}

Per disinstallare il client {{site.data.keyword.SecureGateway}} senza rimuovere la directory di installazione, i file di configurazione, l'ID, l'ID gruppo e i file di log, utilizza:

```
sudo dpkg -r ibm-securegateway-client
```
{: pre}

Per disinstallare il client {{site.data.keyword.SecureGateway}} rimuovendo tutto, utilizza:

```
sudo dpkg --purge ibm-securegateway-client
```
{: pre}

### RedHat/SuSE
{: #rpm-uninstall}

Per trovare lo stato di installazione del client {{site.data.keyword.SecureGateway}},
puoi utilizzare il seguente comando:

```
rpm -qa | grep ibm-securegateway-client
```
{: pre}

Per disinstallare il client {{site.data.keyword.SecureGateway}} senza rimuovere la directory di installazione, i file di configurazione, l'ID, l'ID gruppo e i file di log, utilizza:

```
rpm -e ibm-securegateway-client
```
{: pre}

Per disinstallare il client {{site.data.keyword.SecureGateway}} rimuovendo tutto, utilizza:

```
yum remove ibm-securegateway-client
```
{: pre}

## Windows
{: #windows}

Ci sono due modi per rimuovere l'installazione di Windows per il client {{site.data.keyword.SecureGateway}}:

1. Rimozione dalla directory di installazione: vai alla directory di installazione per il client {{site.data.keyword.SecureGateway}} e fai doppio clic su uninstall.exe.

2. Rimozione dal Pannello di controllo: rimuovi l'applicazione {{site.data.keyword.SecureGateway}} dalla sezione Programmi del Pannello di controllo.
