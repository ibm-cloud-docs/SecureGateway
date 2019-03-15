---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Client installieren
{: #client-install}

## Docker
{: #installing-docker}

Docker ist eine Plattform eines Drittanbieters, von der eine Containermethode zum schnellen und einfachen Installieren von Anwendungen bereitgestellt wird, für die nur wenig oder keine Konfiguration erforderlich ist. Vom {{site.data.keyword.SecureGateway}}-Service wird ein Docker-Image bereitgestellt, das verwendet werden kann, nachdem das Dienstprogramm Docker auf der Workstation installiert ist.  Informationen zum Installieren von Docker finden Sie auf der Website für die Installation von Docker und und in den Anweisungen für Ihr System.

### Client installieren und aktualisieren
{: #docker-install}

Für die Installation und Aktualisierung des Docker-Images für den {{site.data.keyword.SecureGateway}}-Client wird derselbe Befehl verwendet:

```
docker pull ibmcom/secure-gateway-client
```
{: pre}

### Interaktive Clientsitzung starten
{: #docker-run}

Setzen Sie den folgenden Befehl ab, um den Docker-Container für IBM {{site.data.keyword.SecureGateway}} auszuführen:

```
docker run -it ibmcom/secure-gateway-client <gateway ID> -t <security token>
```
{: pre}

Normalerweise sind zwei Parameter erforderlich: Die {{site.data.keyword.SecureGateway}}-Gateway-ID und das Sicherheitstoken des Gateways; beide sind über das {{site.data.keyword.SecureGateway}}-Dashboard verfügbar.

### Unterstützte Docker-Befehle
{: #docker-commands}

Vom {{site.data.keyword.SecureGateway}}-Client werden nur Befehle des Typs `pull` und `run` zum Bearbeiten des Containers unterstützt.

Kehren Sie zu [Einführung - Client hinzufügen](/docs/services/SecureGateway/securegateway_client.html) zurück.

## Mac OS X
{: #installing-mac}

### Voraussetzungen für die Ausführung unter Mac OS X
{: #mac-requirements}

Vor der Ausführung des Clients unter Mac OS X sind die folgenden Voraussetzungen erforderlich.
 1. Installieren Sie die Xcode-Entwicklungstools, sie sind für einige Knotenmodule auf dieser Plattform erforderlich.

### Installationsverfahren für Mac OS X
{: #mac-install}

Je nach Sicherheitskonfiguration Ihres Systems benötigen Sie möglicherweise Administratorrechte, um diese Installation auszuführen.

 1. Hängen Sie das DMG-Image an, das von der {{site.data.keyword.SecureGateway}}-Benutzerschnittstelle heruntergeladen wurde (normalerweise durch Doppelklicken).
 2. Es sollte ein neues 'Finder'-Fenster angezeigt werden. Ist dies nicht der Fall, doppelklicken Sie auf den angehängten Datenträger. Dieses Fenster muss das Symbol für den Ordner 'ibm' und das Direktaufrufsymbol für Anwendungen enthalten; ziehen Sie den Ordner 'ibm' und legen Sie ihn auf dem Direktaufruf ab.

### Interaktive Clientsitzung starten
{: #mac-run}

Führen Sie zum Starten des Clients die Datei `secgw.command` aus, die sich an der Standardinstallationsposition `/Applications/ibm/` befindet.

Kehren Sie zu [Einführung - Client hinzufügen](/docs/services/SecureGateway/securegateway_client.html) zurück.

## Linux
{: #installing-linux}

Die Installation umfasst den {{site.data.keyword.SecureGatewayfull}}-Client sowie eine sichere Version des Node.js-Pakets von IBM.  Beide sind im Verzeichnis '/opt/ibm' auf dem System installiert.  Vom Installationsprogramm wird Folgendes erstellt oder aktualisiert:

| Verzeichnis | Dateiname | Beschreibung          |
| ------------- | ------------- | ----------- |
| /etc/ibm | sgenvironment.conf | Konfigurationsdatei für upstart |
| /etc/ibm | passwd | erstellt Benutzer: secgwadmin:x:501:501::/home/secgwadmin:/bin/bash |
| /etc/ibm | group | erstellt Gruppe: secgwadmin:x:501: |
| /etc/init | securegateway_client.conf | upstart-Datei |
| /opt/ibm | node-v&lt;version&gt;-linux-x64 | Installation des Node.js-Pakets von IBM |
| /opt/ibm | securegateway | Installation des {{site.data.keyword.SecureGateway}}-Clients |
| /usr/local/bin | node | symbolische Verbindung für ausführbare Datei des Knotens von IBM erstellt |
| /usr/local/bin | npm | symbolische Verbindung für ausführbare npm-Datei von IBM erstellt |
| /var/log/securegateway | client_console.log | Protokolldatei beim Ausführen des Clients als Autostartprozess erstellt |

Sie benötigen die Berechtigungen eines Roots oder Administrators zum Ausführen dieser Installation.

### Ubuntu/PowerPC-Installation
{: #debian-install}

 1. Installieren Sie den {{site.data.keyword.SecureGateway}}-Client. Setzen Sie zum Beispiel den folgenden Befehl ab, wenn Sie ein Debian-Paket verwenden.

    ```
    sudo dpkg -i <secure-gateway-debian-package>
    ```
    {: pre}

 2. Wenn das Clientinstallationsprogramm gestartet wird, werden Sie zum Angeben der folgenden Informationen aufgefordert.

    <b>Autostartprozess Ja/Nein</b>
        Falls dies ein Upgrade oder eine Installation ist, müssen Sie auswählen, ob der vorhandene Client gestoppt werden soll, während
        der Installationsprozess ausgeführt wird. Wählen Sie J oder j aus, um den vorhandenen Client zu stoppen. Andernfalls wird für das
        Clientpaket ohne einen Neustart des Clients ein Upgrade durchgeführt; somit können Sie auf das entsprechende Fenster für das
        Software-Update warten, um den Neustart durchzuführen. Falls Sie N oder n auswählen, wird die Installation fortgesetzt und Sie
        müssen den Client manuell erneut starten.

    <b>Gateway-ID</b>
        Legen Sie die Gateway-ID für den Client fest. Die Gateway-ID wird definiert, wenn Sie einen {{site.data.keyword.SecureGateway}}-Service
        erstellen. Wenn die Verbindung nicht vom Client aufgebaut werden kann, können Sie die Auswahl durch die Bearbeitung der .conf-Datei ändern.

    <b>Sicherheitstoken</b>
        Wenn die Gateway-ID aktiviert ist, damit eine Überprüfung auf Sicherheitstoken durchgeführt werden kann, müssen Sie sie jetzt angeben. Falls
        für die Gateway-ID kein Sicherheitstoken erforderlich ist, lassen Sie das Feld leer.

    <b>Protokollebene</b>
        Die Standardeinstellung ist INFO. Weitere gültige Werte sind TRACE, DEBUG und ERROR.

    <b>Zugriffssteuerungsliste</b>
        Geben Sie den absoluten Pfad zu dem Namen der Datei ein, in der die Zugriffssteuerungsliste enthalten ist, durch die der Zugriff auf die lokalen
        Ressourcen gesteuert wird. Weitere Informationen finden Sie in der Datei README.md in /opt/ibm/securegateway oder in der Zugriffssteuerungsliste.

    <b>Clientbenutzerschnittstelle</b>
        Wählen Sie aus, ob Sie die Clientbenutzerschnittstelle verwenden möchten. Falls ja, kann der Benutzer den Port für die Benutzerschnittstelle ändern. Der Standardwert ist 9003.

    <b>Anmerkung:</b> Sie müssen auf keine der Eingabeaufforderungen reagieren, für alle Werte wird die definierte Standardeinstellung verwendet oder es werden keine Werte in der Datei 'sgenvironment.conf' angegeben. So kann der Installationsprozess ohne Benutzerinteraktion ausgeführt werden.

    <b>Anmerkung:</b> Die Datei 'sgenvironment.conf' wird bei jedem Start bzw. Neustart des Clients im Verlauf des upstart-Prozesses des Systems gelesen. Sie können die Datei '/etc/ibm/sgenvironment.conf' jederzeit bearbeiten, um Änderungen an der Konfiguration vorzunehmen und den Client erneut zu starten, um diese Änderungen zu übernehmen.

    <b>Anmerkung:</b> Die Sprache des Serviceprotokolls des {{site.data.keyword.SecureGateway}}-Clients kann durch das Ändern des Werts für den Parameter `LANGUAGE` in der Datei '/etc/ibm/sgenvironment.conf' geändert werden. Die für die Serviceprotokolle ausgewählte Sprache wird nach dem Neustart des Service geändert.

 3. Wenn Sie den Client erneut gestartet haben, setzen Sie den folgenden Befehl ab, um sicherzustellen, dass der Client ordnungsgemäß ausgeführt wird:

    ```
    cat /var/log/securegateway/client_console.log
    ```
    {: pre}

 4. Wenn Sie die Konfigurationsdatei bearbeiten möchten, setzen Sie den folgenden Befehl ab, um sicherzustellen, dass die Konfigurationsdatei ordnungsgemäß aktualisiert wird:

    ```
    sudo vi /etc/ibm/sgenvironment.conf
    ```
    {: pre}

 5. Setzen Sie den folgenden Befehl ab, um den Status der Clientinstallation zu überprüfen:

    ```
    sudo dpkg --status ibm-securegateway-client
    ```
    {: pre}

Als Ausgabe wird folgendes Beispiel zurückgegeben:

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

### RedHat/SuSE-Installation
{: #rpm-install}

1. Installieren Sie den {{site.data.keyword.SecureGateway}}-Client. Setzen Sie zum Beispiel den folgenden Befehl ab, wenn Sie ein RPM-Paket verwenden:

   ```
   rpm -ivhf <secure-gateway-rpm-package>
   ```
   {: pre}


   Das Clientinstallationsprogramm wird gestartet; von ihm werden der Client installiert und die Datei 'sgenvironment.conf' im Pfad '/etc/ibm' erstellt.

2. Optional: Falls Sie den upstart-Prozess des Systems verwenden möchten, müssen Sie diese Datei bearbeiten und Folgendes angeben, damit der Client ordnungsgemäß gestartet wird. Weitere Informationen zum Bearbeiten dieser Konfigurationsdatei finden Sie unter [Upstart verwenden](/docs/services/SecureGateway/securegateway_auto-start.html#auto-start-linux).

3. Wenn Sie den Client mit 'upstart' gestartet haben, überprüfen Sie die Protokolldatei, um sicherzustellen, dass die Datei ordnungsgemäß ausgeführt wird.

   ```
   cat /var/log/securegateway/client_console.log
   ```
   {: pre}

4. Setzen Sie den folgenden Befehl ab, um den Status der Clientinstallation zu überprüfen:

   ```
   rpm -q ibm-securegateway-client
   ```
   {: pre}

### AIX-Installation
{: #aix-install}

1. Stellen Sie sicher, dass für das Paket eine Berechtigung zur Ausführung vorliegt. Falls dies erforderlich sein sollte, ändern Sie die Dateiberechtigungen durch Absetzen des folgenden Befehls:
    ```
    chmod a+x <secure-gateway-bin-package>
    ```
2. Extrahieren Sie das Paket durch Absetzen des folgenden Befehls:
    ```
    ./<secure-gateway-bin-package>
    ```

Anmerkung:
Stellen Sie sicher, dass Ihr AIX-System die Voraussetzungen für die Ausführung der installierten Software Node.js und der Korn-Shell verfügt.

### Interaktive Clientsitzung starten
{: #linux-run}

Führen Sie zum Starten des Clients die folgenden Befehle aus:

```
cd /opt/ibm/securegateway/client
node lib/secgwclient.js <gateway ID> -t <security token>
```
{: codeblock}

Normalerweise sind zwei Parameter erforderlich: Die {{site.data.keyword.SecureGateway}}-Gateway-ID und das Sicherheitstoken des Gateways; beide sind über das {{site.data.keyword.SecureGateway}}-Dashboard verfügbar.

Kehren Sie zu [Einführung - Client hinzufügen](/docs/services/SecureGateway/securegateway_client.html) zurück.

## Windows
{: #installing-windows}

### Client installieren
{: #windows-install}

Je nach Sicherheitskonfiguration Ihres Systems benötigen Sie möglicherweise Administratorrechte, um diese Installation auszuführen.

 1. Kopieren Sie die EXE-Installationsdatei auf das System.  Es wird auch MD5 SUM bereitgestellt, damit Sie die Integrität des Installationspakets überprüfen können.

 2. Führen Sie die Installation entweder durch Doppelklicken auf die Datei und Beantworten der Eingabeaufforderungen oder durch Ausführen der ausführbaren Datei in der Eingabeaufforderung aus.

 3. Der Benutzer wird aufgefordert, das gewünschte Installationsverzeichnis auszuwählen. Standardposition ist das Verzeichnis 'Programme (x86)'.

 4. Der Benutzer wird aufgefordert, die Sprache auszuwählen, in der die Befehlszeilenschnittstelle gestartet werden soll. Wenn keine Sprache ausgewählt ist, wird für die Sprache standardmäßig Englisch festgelegt.

 5. Der Benutzer wird aufgefordert, den Client als Windows-Dienst zu installieren. Wenn sich der Benutzer dafür entscheidet, wird der Client im Hintergrund als Windows-Dienst mit den Konfigurationen ausgeführt, die der Benutzer im nachfolgenden Dialog festlegt. Der Status des Service kann auf der Seite der Services in der Systemsteuerung angezeigt werden. Der Name des Service lautet 'IBM Bluemix Secure Gateway Service'.

 6. Vom Installationsprogramm wird erkannt, ob auf dem Computer bereits eine Installation vorhanden ist. Wenn eine solche erkannt wird, wird der Benutzer gefragt, ob er die vorhandenen Konfigurationen verwenden möchte; andernfalls hat der Benutzer die Möglichkeit, neue Konfigurationen einzugeben. An dieser Stelle können Details wie Gateway-IDs, Sicherheitstoken, ACL-Dateien und Protokollebenen für jedes Gateway angegeben werden. Wenn sich der Benutzer für die Ausführung des Clients als Windows-Dienst entschieden hat, wird der Client mit den bereitgestellten Konfigurationen gestartet. Wenn sich der Benutzer gegen das Starten des Clients als Windows-Dienst entschieden hat, werden die Konfigurationen für die weitere Verwendung gespeichert. In beiden Fällen werden die Konfigurationen in '%Installation_directory%/ibm/securegateway/client/securegw_service.config' gespeichert.

 7. Ab Version 1.5.0 wird der Client mit einer voll funktionsfähigen Clientbenutzerschnittstelle bereitgestellt. Sie können sie über das Installationsprogramm konfigurieren. Der Benutzer kann das Kennwort und die Portnummer (Standardeinstellung ist 9003) für den Start der Clientbenutzerschnittstelle angeben.

 Wenn sich der Benutzer dazu entscheidet, den Client mit Verbindungen zu mehreren Gateways zu starten, sollten die Konfigurationswerte mit Sorgfalt festgelegt werden. Die Gateway-IDs müssen durch Leerschritte voneinander getrennt werden. Als Begrenzer für Sicherheitstoken, ACL-Dateien und Protokollebenen muss die Zeichenfolge '--' verwendet werden. Wenn Sie keinen dieser drei Werte für ein bestimmtes Gateway angeben möchten, verwenden Sie 'none'.

 <b>Anmerkung:</b> Stellen Sie sicher, dass keine restlichen Leerzeichen vorhanden sind.

 <b>Anmerkung:</b> Beachten Sie, dass sich die ACL-Dateien im Verzeichnis '%Installation_directory%/ibm/securegateway/client' oder einem Verzeichnis relativ zu diesem Pfad befinden müssen.

### Interaktive Clientsitzung starten
{: #windows-run}

Öffnen Sie zum Starten des Clients eine Eingabeaufforderung mit Administratorberechtigungen und führen Sie die folgenden Befehle aus:

```
cd "<Installation_directory>\ibm\securegateway\client"
secgw.cmd
```
{: codeblock}

Alternativ können Sie zum Verzeichnis `<Installation_directory>\ibm\securegateway\client` navigieren und auf `secgw.cmd` doppelklicken.

<b>Anmerkung:</b> Sie können nicht die Konfigurationen verwenden, die in der Datei `<Installation_directory>\ibm\securegateway\client\securegw_service.config` gespeichert sind oder die Details interaktiv angeben.

Kehren Sie zu [Einführung - Client hinzufügen](/docs/services/SecureGateway/securegateway_client.html) zurück.

## DataPower
{: #installing-datapower}

DataPower verfügt über eine eingebettete Version des {{site.data.keyword.SecureGateway}}-Clients.  Abhängig von der DataPower-Version haben Sie möglicherweise eine andere Version des {{site.data.keyword.SecureGateway}}-Clients.  Beachten Sie alle maßgeblichen [Einschränkungen des DataPower-Clients](/docs/services/SecureGateway/securegateway_interaction.html#limits-datapower). Bei Verwendung des alten Secure Gateway-Clients können unerwartete Fehler auftreten.

| DataPower-Version | {{site.data.keyword.SecureGateway}}-Clientversion  |
| -- | --  |
| 7.2.0.0, 7.5.0.0 | 1.1.0  |
| 7.5.1.0, 7.7.0 | 1.4.2  |
| 7.5.2.4 | 1.6.1  |
| 7.5.2.6, 7.6.0.0 | 1.7.0  |
| 7.5.2.14, 7.6.0.7, 7.7.1.0 |  1.8.0fp6  |

### Clientsitzung starten
{: #datapower-run}

1. Melden Sie sich an der Web-GUI von DataPower an.
2. Navigieren Sie zu 'Objects' -> 'Cloud' -> 'Secure Gateway Client' oder suchen Sie nach dem Secure Gateway-Client.
3. Klicken Sie auf `Add`, um eine neue Clientverbindung zu konfigurieren.
4. Geben Sie einen Namen, die Gateway-ID und das Sicherheitstoken (sofern anwendbar) an wenden Sie die Änderungen anschließend an.

Kehren Sie zu [Einführung - Client hinzufügen](/docs/services/SecureGateway/securegateway_client.html) zurück.
