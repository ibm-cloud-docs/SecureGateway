---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Automatischen Start für Client konfigurieren
{: #auto-start-conf}

## Linux
{: #auto-start-linux}

Wenn Sie sich für die Verwendung der automatischen Startfunktion des Systems entschieden haben, verwenden Sie eine der folgenden Methoden, um den Client zu starten.  Die vom Client verwendete Konfigurationsdatei befindet sich im folgenden Pfad:

```
/etc/ibm/sgenvironment.conf
```
{: screen}

<b>Anmerkung:</b> Die Funktion für den automatischen Start ist unter Red Hat Linux 6 nicht verfügbar.

In dieser Datei sind wichtige Variablen enthalten, die festgelegt werden sollten:

| Umgebungsvariable | Beschreibung       |
| ------------- | ----------- |
| RESTART_CLIENT | Client während Installation oder Upgrade stoppen oder erneut starten (Ja oder Nein) |
| GATEWAY_ID | Gateway-ID, die für Secure Gateway für die Bluemix-Benutzerschnittstelle erstellt wurde |
| SECTOKEN | Sicherheitstoken für diese Gateway-ID, falls bei der Erstellung des Gateways festgelegt wurde, dass die Sicherheit erzwungen werden soll |
| ACL_FILE | ACL-Datei, die verwendet werden soll, um den lokalen Zugriff auf die Ressourcen zu begrenzen |
| LOGLEVEL | Die Protokollebene, die für den Service festgelegt werden soll (Standardeinstellung ist INFO) |
| USE_UI   | Den Wert 'N' festlegen, wenn die Benutzerschnittstelle des Clients nicht gestartet werden soll |
| UI_PORT  | Der Port, an dem die Benutzerschnittstelle des Clients gestartet werden soll (Standardwert ist 9003) |
| LANGUAGE | Die Sprache, die für die Protokolle des Clients verwendet werden soll (Standardeinstellung ist en) |

<b>Anmerkung:</b> Diese Datei wird nur gelesen, wenn Sie die Funktion für den automatischen Start des Systems verwenden.  Wenn Sie den Client manuell ausführen, wird diese Datei ignoriert.

### Upstart
{: #auto-start-upstart}

### Client starten
{: #upstart-start}

Falls Sie die Funktion 'upstart' verwenden, damit der Secure Gateway-Client beim Systemstart automatisch ausgeführt wird, müssen Sie zunächst die Konfiguration dieser Funktion überprüfen (/etc/ibm/sgenvironment.conf).  Sobald Sie den Service 'upstart' konfiguriert haben, können Sie ihn mit dem folgenden Befehl starten:

```
sudo initctl start securegateway_client
```
{: pre}

Gehen Sie wie folgt vor, um den Service erneut zu starten:

```
sudo initctl restart securegateway_client
```
{: pre}

### Client stoppen
{: #upstart-stop}

Führen Sie zum Stoppen des Service den folgenden Befehl aus:

```
sudo initctl stop securegateway_client
```
{: pre}

### SystemD
{: #auto-start-systemd}


### Client starten
{: #systemd-start}

Falls Sie die Funktion 'systemD' verwenden, damit der Secure Gateway-Client beim Systemstart automatisch ausgeführt wird, müssen Sie möglicherweise zunächst die Konfiguration dieser Funktion überprüfen (/etc/ibm/sgenvironment.conf).  Sobald Sie den Service 'upstart' konfiguriert haben, können Sie ihn mit dem folgenden Befehl starten:

```
systemctl start securegateway_client
```
{: pre}

Gehen Sie wie folgt vor, um den Service erneut zu starten:

```
systemctl restart securegateway_client
```
{: pre}

Für den Status des Service:

```
systemctl status securegateway_client
```
{: pre}

### Client stoppen
{: #systemd-stop}

Führen Sie zum Stoppen des Service den folgenden Befehl aus:

```
systemctl stop securegateway_client
```
{: pre}

### System V
{: #auto-start-system-v}

System V wird während der Installation nicht wie die anderen Funktionen für den automatischen Start konfiguriert. Im Installationsverzeichnis '/opt/ibm/securegateway/client/upstart' sind hierfür folgende Scripts verfügbar:

```
secgw.sh
securegateway_clientd
```
{: screen}

<b>Anmerkung:</b> Dieser Abschnitt betrifft die Benutzer von SuSE/SLES 11.

Optional: Wenn Sie die SuSE-Version 11 ausführen, die die automatischen System V-Startfunktion verwendet, werden Scripts bereitgestellt, die zum Konfigurieren dieses Prozesses verwendet werden können. Gehen Sie gemäß den folgenden Anweisungen vor:

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

Sobald diese Schritte ausgeführt wurden, können die YasT- und System V-Befehle verwendet werden, um den Dämon zu starten bzw. zu stoppen.

Kehren Sie zu [Einführung - Client hinzufügen](/docs/services/SecureGateway/securegateway_client.html) zurück.

## Windows
{: #auto-start-windows}

Öffnen Sie zum Ändern des Status des Windows-Diensts ein Befehlsfenster mit Administratorberechtigungen.

```
cd "<Installation_directory>\ibm\securegateway\client"
```
{: pre}

Wenn der Service bereits ausgeführt wird, kann der Benutzer ihn mit dem folgenden Befehl stoppen:

```
windowsService.cmd uninstall
```
{: pre}

Verwenden Sie den folgenden Befehl, um den Service zu starten:

```
windowsService.cmd install
```
{: pre}

<b>Anmerkung:</b> Der Service wird auf der Basis der Konfigurationen ausgeführt, die im folgenden Pfad gespeichert sind:

```
%Installation_directory%/ibm/securegateway/client/securegw_service.config
```
{: pre}

Die Anwendungsprotokolle für den Windows-Dienst werden an der folgenden Position gespeichert:

```
%Installation_directory%/ibm/securegateway/client/logs/securegw_win_service.log
```
{:pre}

 Die Protokolle rollieren täglich in eine neue Datei.

Kehren Sie zu [Einführung - Client hinzufügen](/docs/services/SecureGateway/securegateway_client.html) zurück.
