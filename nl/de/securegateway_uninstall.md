---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Client deinstallieren

## Docker
{: #docker}

Führen Sie den folgenden Befehl aus, um das Docker-Image des {{site.data.keyword.SecureGateway}}-Clients zu entfernen:

```
docker rmi ibmcom/secure-gateway-client
```
{: pre}

## Mac OS X
{: #mac}

Führen Sie die folgenden Befehle im Terminal aus, um den {{site.data.keyword.SecureGateway}}-Client von Mac OS X zu entfernen:

```
cd /Applications
rm -rf ./ibm
```
{: codeblock}

## Linux
{: #linux}

### Ubuntu/PowerPC
{: #debian-uninstall}

Führen Sie den folgenden Befehl aus, wenn Sie den Installationsstatus des {{site.data.keyword.SecureGateway}}-Clients ermitteln möchten:

```
sudo dpkg -l | grep ibm-securegateway-client
```
{: pre}

Führen Sie den folgenden Befehl aus, wenn Sie den {{site.data.keyword.SecureGateway}}-Client deinstallieren möchten, ohne das Installationsverzeichnis, die Konfigurationsdateien, die ID, die Gruppen-ID und die Protokolldateien zu entfernen:

```
sudo dpkg -r ibm-securegateway-client
```
{: pre}

Führen Sie den folgenden Befehl aus, um den {{site.data.keyword.SecureGateway}}-Client zu deinstallieren und alles zu entfernen:

```
sudo dpkg --purge ibm-securegateway-client
```
{: pre}

### RedHat/SuSE
{: #rpm-uninstall}

Führen Sie den folgenden Befehl aus, wenn Sie den Installationsstatus des {{site.data.keyword.SecureGateway}}-Clients ermitteln möchten:

```
rpm -qa | grep ibm-securegateway-client
```
{: pre}

Führen Sie den folgenden Befehl aus, wenn Sie den {{site.data.keyword.SecureGateway}}-Client deinstallieren möchten, ohne das Installationsverzeichnis, die Konfigurationsdateien, die ID, die Gruppen-ID und die Protokolldateien zu entfernen:

```
rpm -e ibm-securegateway-client
```
{: pre}

Führen Sie den folgenden Befehl aus, um den {{site.data.keyword.SecureGateway}}-Client zu deinstallieren und alles zu entfernen:

```
yum remove ibm-securegateway-client
```
{: pre}

## Windows
{: #windows}

Die Installation eines {{site.data.keyword.SecureGateway}}-Clients kann unter Windows auf zwei Arten entfernt werden:

1. Entfernung im Installationsverzeichnis: Wechseln Sie in das Installationsverzeichnis des {{site.data.keyword.SecureGateway}}-Clients und doppelklicken Sie auf die Datei 'uninstall.exe'.

2. Entfernung über die Systemsteuerung: Entfernen Sie die Anwendung {{site.data.keyword.SecureGateway}} aus dem Abschnitt 'Programme und Funktionen' der Systemsteuerung.
