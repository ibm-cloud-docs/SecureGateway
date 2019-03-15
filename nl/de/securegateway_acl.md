---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Zugriffssteuerungsliste
{: #acl}

Vom {{site.data.keyword.SecureGateway}}-Client wird Unterstützung für eine eingebettete Zugriffssteuerungsliste (Access Control List, ACL) bereitgestellt. Sie können den Zugriff auf lokale Ressourcen durch Änderungen an der Zugriffssteuerungsliste für den Client zulassen oder beschränken (verweigern).  Dies ist interaktiv mithilfe von Clientbefehlen oder durch Angeben einer Datei mit Zugriffssteuerungslisten möglich, die angewendet werden sollen.

Ab Version 1.5.0 werden die ACL-Regeln für alle Clients synchronisiert, die mit demselben Gateway verbunden sind.  Somit müssen Sie nur die Zugriffssteuerungsliste (ACL) eines einzigen Clients erstellen bzw. aktualisieren, damit diese Aktion auf allen aktiven Clients synchron ausgeführt wird, die mit diesem Gateway verbunden sind.  Da die ACL auch für mehrere Sitzungen erhalten bleibt, gelten dieselben ACL-Regeln auch für die Verbindung zu einem neuen Client.

## Befehle für die Zugriffssteuerungsliste
{: #acl-commands}

Für die Zugriffssteuerungsliste werden folgende Befehle unterstützt:

```
acl allow [<hostname>]:[<port>]
acl deny [<hostname>]:[<port>]
no acl [<hostname>]:[<port>]
no acl
show acl
```
{: screen}

Wenn weder ein Hostname noch ein Port angegeben ist, sind alle Hostnamen und Ports eingeschlossen.  Die folgende ACL-Regel lässt zum Beispiel für Port 22 alle Hostnamen zu.

```
acl allow :22
```
{: pre}

Im folgenden Beispiel sind laut ACL-Regel alle Hostnamen für alle Ports zulässig, was die ACL-Unterstützung prinzipiell inaktiviert. <b>Dies wird nicht empfohlen.</b>

```
acl allow :
```
{: pre}

Mit dem Befehl `show acl` wird die aktuell festgelegte ACL angezeigt oder eine Nachricht über die Gesamteinstellung bereitgestellt.

Kehren Sie zu [Einführung - Client hinzufügen](/docs/services/SecureGateway/securegateway_client.html) zurück.

## HTTP- und HTTPS-Routensteuerung mithilfe der Zugriffssteuerungsliste
{: #acl-route-control}

Ab Version 1.6.0 können von HTTP- und HTTPS-Zielen auch bestimmte Routen für ACL-Einträge erzwungen werden.  Diese werden auf dieselbe Art wie herkömmliche ACL-Einträge hinzugefügt, am Ende der Regel wird jedoch noch der Pfad angehängt. Im folgenden Beispiel werden nur Anforderungen zugelassen, auf die der Pfad /my/api folgt:

```
acl allow localhost:80/my/api
```
{: pre}

Wenn diese Regel wirksam ist, sind Anforderungen an `<cloud host>:<cloud port>/my/api*` zulässig.

Routen werden nur für Befehle des Typs `acl allow` unterstützt.

## Rangfolge der Zugriffssteuerungsliste
{: #acl-precedence}

Nach dem Angeben des Befehls `acl allow :` und dem Eingeben weiterer Befehle des Typs `acl allow` wird die Zulassungsliste `ALL:ALL` (von `acl allow :`) aus der Liste entfernt, da davon ausgegangen wird, dass unbegrenzter Zugriff nicht mehr zulässig sein soll.  Nach dem Angeben des Befehls `acl deny :` und dem Eingeben des Befehls `acl deny` wird die Ablehnungsliste `ALL:ALL` (von `acl deny :`) aus der Liste entfernt, da davon ausgegangen wird, dass der Zugriff nicht länger vollständig beschränkt werden soll.  Falls Sie die aktuellen ACL-Regeln mithilfe des Befehls `show acl` in der Befehlszeilenschnittstelle auflisten, wird anhand eines Indikators angezeigt, ob nicht aufgelistete Regeln zulässig sind oder verweigert werden.

## ACL-Datei importieren
{: #import-acl-file}

Sie können mit dem Befehl `acl file` den Namen einer Datei angeben, in der unterstützte ACL-Befehle enthalten sind, die vom Client beim Start gelesen werden. Die Befehle in dieser Datei müssen das folgenden Format aufweisen:

```
acl allow [<hostname>]:[<port>]
acl deny [<hostname>]:[<port>]
no acl [<hostname>]:[<port>]
no acl
```
{: screen}

<b>Anmerkung:</b> Wenn `no acl` ohne weitere Parameter angegeben wird, wird die ACL-Tabelle ZURÜCKGESETZT und für den Zugriff DENY ALL festgelegt.

Ein einfaches Beispiel für eine ACL-Datei finden Sie [hier](/docs/services/SecureGateway/securegateway_acl-file.html).

Kehren Sie zu [Einführung - Client hinzufügen](/docs/services/SecureGateway/securegateway_client.html) zurück.

## ACL-Datei in {{site.data.keyword.SecureGateway}}-Docker-Client kopieren
{: #copy-acl-to-docker}

Vom {{site.data.keyword.SecureGateway}}-Docker-Client wird im Wesentlichen der eigene Virtualisierungscontainer ausgeführt.  Auf das Dateisystem der Hostmaschine kann somit nicht direkt von Prozessen zugegriffen werden, die im Container ausgeführt werden, einschließlich des {{site.data.keyword.SecureGateway}}-Clients.  Ab Version 1.8.0 der Docker-Engine können Sie den Befehl 'docker cp' verwenden, um Dateien, die auf dem Host vorhanden sind, in den Container zu importieren, während er aktiv oder gestoppt ist; dieser Vorgang ist notwendig, damit der interaktive Befehl ACL FILE des {{site.data.keyword.SecureGateway}}-Clients verwendet werden kann.

Damit die interaktive Unterstützung von 'cp' in Docker durch den Host für die Docker-Instanz gewährleistet ist, muss die Docker-Version 1.8.0 verwendet werden. Sie können dies mit `docker -- version` überprüfen.

Daraufhin sollten für die Version folgende Daten angezeigt werden. Es wird empfohlen, für die Ausführung von Docker durch Benutzer ohne Rootberechtigung zuzulassen; führen Sie somit den Befehl aus, der nach dem Upgrade der Engine auf 1.8.0 oder 1.8.2 empfohlen wird.

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

Führen Sie anschließend die folgenden Schritte aus, um die ACL-Dateiliste in das Docker-Image zu importieren:

- Befehl 'docker ps' zum Suchen der Container-ID ausführen

```
CONTAINER ID IMAGE                        COMMAND                CREATED        STATUS  PORTS NAMES
764aadce386b ibmcom/secure-gateway-client "node lib/secgwclient" 27 seconds ago Up 26 seconds condescending_nobel
```
{: screen}

- Kopieren Sie 'acl.list' mit dem Befehl 'docker cp' entweder mit der Container-ID oder dem Namen:

```
docker cp 01_client.list 764aadce386b:/root/01_client.list
```
{: pre}

- Gehen Sie danach im {{site.data.keyword.SecureGateway}}-Client bei Ausführung von Docker wie folgt vor:

```
cli F /root/01_client.list

 [2015-10-01 08:12:30.091] [INFO] The current access control list is being reset and replaced by the user provided file: /root/01_client.list
 [2015-10-01 08:12:30.093] [INFO] The ACL file process accepts acl allow 127.0.0.1:27017
 [2015-10-01 08:12:30.094] [INFO] The ACL file process accepts acl allow 127.0.0.1:22
```
{: screen}

- Zeigen Sie die Zugriffssteuerungsliste an:

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

Kehren Sie zu [Einführung - Client hinzufügen](/docs/services/SecureGateway/securegateway_client.html) zurück.
