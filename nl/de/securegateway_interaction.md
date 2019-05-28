---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-19"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Interaktion mit dem Client
{: #client-interacting}

Für die Interaktion mit dem Client gibt es mehrere Möglichkeiten:

- Über die Terminalbefehlszeile vor dem Start
- Nach dem Start über die interaktive Befehlszeile des Clients
- Nach dem Start unter Verwendung der lokalen Benutzerschnittstelle des Clients

## Startkonfigurationen
{: #startup}

### Startargumente und -optionen
{: #startup-args}

In der folgenden Tabelle werden alle verfügbaren Optionen beschrieben, die mit dem Startbefehl für den Client angegeben werden können.  Mit ihnen ist die vollständige Konfiguration des Clients vor dem Start möglich und eine individuelle Konfiguration nach dem Start des Clients ist nicht erforderlich.

| Parameter und Argumente | Beschreibung |
| ------------- | ----------- |
| &lt;gateway ID&gt; | Verbindung zu {{site.data.keyword.Bluemix_notm}} mithilfe der angegebenen Gateway-ID |
| -F, -\-aclfile &lt;file&gt; | Datei der Zugriffssteuerungsliste |
| -g, -\-gateway &lt;hostname:port&gt; | Wird verwendet, um ein bestimmtes Gateway-Ziel manuell auszuwählen (nur für erfahrene Benutzer) |
| -l, -\-loglevel &lt;level&gt; | Protokollebene in ERROR, INFO, DEBUG oder TRACE ändern |
| -p, -\-logpath &lt;file&gt; | Direkte Protokollierung in eine bestimmte Datei |
| -t, -\-sectoken &lt;security token&gt; | Das Sicherheitstoken, das für diese Gateway-Verbindung verwendet werden soll |
| -P, -\-port &lt;port&gt; | Der Port für die Benutzerschnittstelle zur Ausführung.  Standardmäßig Port 9003 |
| -w, -\-password &lt;password&gt; | Das Kennwort, mit dem die Benutzerschnittstelle geschützt werden soll.  Standardmäßig kein Kennwort |
| -x, -\-proxy &lt;proxy agent&gt; | Der Proxy für die Verbindung zu Port 9000 |
| -\-noUI | Verhindert, dass die Benutzerschnittstelle automatisch gestartet wird |
| -\-allow | Ermöglicht alle Verbindungen zum Client. Wird durch eine ACL-Datei außer Kraft gesetzt, sofern angegeben |
| -\-service | Nach einer einleitenden Verbindung wird der übergeordnete Client innerhalb von 60 Sekunden neu gestartet, falls alle untergeordneten Clients beendet wurden |

<b>Anmerkung:</b> Die Flags `--service`, `--allow` und `--noUI` müssen die letzten Parameter in Befehlszeilenargumenten sein.

Der einfachste Anwendungsfall ist der Start mit einer einzigen Clientverbindung unter Verwendung der Standardeinstellungen:

```
<gateway_id> -t <security_token>
```
{: pre}

Diese Optionen können erweitert werden, sodass auch eine automatische Verbindung mehrerer Clients unterstützt wird.  Damit diese an mehrere Clientverbindungen übergeben werden, müssen sie in einem bestimmten Format übergeben werden.  Die Gateway-IDs können auf wie bei einer einzelnen Clientverbindung übergeben werden (wobei die Gateway-IDs durch ein Leerzeichen voneinander getrennt werden müssen); aus der Reihenfolge dieser IDs ergibt sich jedoch die Reihenfolge, in der die übrigen Argumente folgen müssen.  Wenn Sie in anderen Argumenten übergeben werden, müssen Sie durch `--` voneinander getrennt werden, damit sie ordnungsgemäß erfasst werden.  Wenn für ein Flag nichts angegeben wird, wird davon ausgegangen, dass es auf keinen Client angewendet werden soll.

Wenn nicht genügend Argumente für alle Gateway-IDs angegeben werden, werden sie in der Reihenfolge angewendet, bis keine Argumente mehr vorhanden sind.  Wenn zum Beispiel zwei Gateway-IDs übergeben werden und ein einziges Sicherheitstoken übergeben wird, wird das Token auf die erste Gateway-ID und nicht auf die zweite Gateway-ID angewendet.  

Wenn Gateway-IDs angegeben werden, für die unterschiedliche Argumente erforderlich sind, muss das Schlüsselwort `none` für das Gateway angegeben werden, für das kein Argument erzwungen bzw. angegeben wird.  Wenn zum Beispiel drei Gateway-IDs übergeben werden und der Benutzer eine Protokollebene für das erste und dritte angeben möchte, werden die Argument wie folgt angegeben: `--loglevel DEBUG--none--TRACE`.  In diesem Fall wird für 'none' der Standardwert INFO verwendet.

### Beispiele für Startargumente
{: #startup-examples}

Einzelne Clientverbindung, angepasste Protokollebene, keine Benutzerschnittstelle:

```
node lib/secgwclient.js <gateway_id> -t <security_token> -l <loglevel> --noUI
```
{: pre}

Mehrere Clientverbindungen, Standardoptionen:

```
node lib/secgwclient.js <gateway_id_1> <gateway_id_2> -t <security_token_1>--<security_token_2>
```
{: pre}

Mehrere Clientverbindungen, alle angepassten Einstellungen:

```
node lib/secgwclient.js <myGatewayID_1> <myGatewayID_2> -t none--<token for gateway 2> -l DEBUG--TRACE -p <full path to log file for gateway 1>--<full path to log file for gateway 2> -F <full path to ACL file for gateway 1>
```
{: pre}

Kehren Sie zu [Einführung - Client hinzufügen](/docs/services/SecureGateway?topic=securegateway-add-client) zurück.

## Interaktive Konfiguration
{: #interactive}

### Interaktive Befehle
{: #interactive-commands}

Der {{site.data.keyword.SecureGateway}}-Client verfügt zur leichteren Konfiguration und Steuerung über eine Befehlszeilenschnittstelle und eine Shelleingabeaufforderung. Von der interaktiven Umgebung werden mehr Funktionen als von den Befehlszeilenargumenten unterstützt; so soll eine bessere interaktive Steuerung des Clients ermöglicht werden.

| Interaktive Befehle | Beschreibung       |
| ------------- | ----------- |
| A, acl allow &lt;hostname:port&gt; &lt;worker ID&gt; | Zugriffssteuerungsliste zulassen |
| D, acl deny &lt;hostname:port&gt; &lt;worker ID&gt; | Zugriffssteuerungsliste verweigern |
| N, no acl &lt;hostname:port&gt; &lt;worker ID&gt; | Eintrag in Zugriffssteuerungsliste entfernen |
| S, show acl &lt;worker ID&gt; | Alle Einträge in Zugriffssteuerungsliste anzeigen |
| F, acl file &lt;file&gt; &lt;worker ID&gt; | Datei der Zugriffssteuerungsliste |
| C, displayconfig &lt;worker ID&gt; | Aktuelle {{site.data.keyword.SecureGateway}}-Konfiguration anzeigen (sofern verfügbar) |
| a, authorize &lt;worker ID&gt; | Überschreiben des Parameters 'rejectUnauthorized' für abgehende TLS-Verbindungen für angegebenen Worker ein-/ausschalten |
| t, sectoken &lt;security token&gt; | Das Sicherheitstoken, das für die nächste Gateway-Verbindung verwendet werden soll |
| c, connect &lt;gateway ID&gt; | Verbindung zu {{site.data.keyword.Bluemix_notm}} mithilfe der angegebenen Gateway-ID |
| l, loglevel &lt;level&gt; &lt;worker ID&gt; | Protokollebene in ERROR, INFO, DEBUG oder TRACE ändern |
| p, logpath &lt;file&gt; &lt;worker ID&gt; | Direkte Protokollierung in eine bestimmte Datei |
| r, reverse &lt;worker ID&gt; | Ports auflisten, an denen der Client derzeit für umgekehrte Ziele empfangsbereit ist |
| k, kill &lt;worker ID&gt;  | Angegebenen Worker beenden |
| e, select &lt;worker ID&gt;  | Gibt einen Worker an, für den Befehle ausgeführt werden sollen (sofern nicht anders angegeben) |
| d, deselect | Nimmt Auswahl des vorher angegebenen Workers zurück.  Auswahlbefehl zum Angeben eines anderen absetzen |
| w, password &lt;old password&gt; &lt;new password&gt; | Legt das Kennwort für die Benutzerschnittstelle fest.  Wenn für &lt;new password&gt; nichts angegeben ist, wird kein Kennwort erzwungen. Die Angabe von &lt;altes kennwort&gt; ist für die Aktualisierung des Kennworts erforderlich. Kennwörter dürfen nur Buchstaben enthalten. |
| P, port &lt;new port&gt; | Port ändern, an dem die Benutzerschnittstelle empfangsbereit ist |
| u, uistart &lt;initial password&gt; &lt;port&gt; | Startet die Benutzerschnittstelle unter Verwendung von localhost:&lt;port&gt;/dashboard. Wenn für &lt;initial password&gt; kein Wert angegeben ist und kein anderes Kennwort für die Sitzung festgelegt wurde, wird kein Benutzerschnittstellenkennwort erzwungen. Falls für &lt;port&gt; kein Wert angegeben ist, ist die Benutzerschnittstelle an Port 9003 erreichbar. |
| U, uistop | Schließt die Benutzerschnittstelle, die dieser Clientsitzung zugeordnet ist. Auf die Sitzung kann nur über die Befehlszeilenschnittstelle zugegriffen werden, bis die neue Benutzerschnittstelle manuell gestartet wird. |
| R, revoke | Löscht alle Berechtigungen für die Benutzerschnittstelle, die dieser Clientsitzung zugeordnet sind |
| q, quit | Verbindung trennen und beenden |
| s, status &lt;worker ID&gt;  | Statusdetails des Tunnels und seiner Verbindungen drucken |
| L, list | Zeigt eine Liste der derzeit zugeordneten Worker an |

<b>Anmerkung:</b> Wenn eine Verbindung mit dem Befehl `select` angegeben wurde und ein weiterer Befehl ohne Angabe der Worker-ID aufgerufen wird, wird versucht, den Befehl für die Verbindung auszuführen, die mithilfe von `select` angegeben wurde.

Weitere Informationen zum Konfigurieren der Zugriffssteuerungsliste erhalten Sie, wenn Sie [hier klicken](/docs/services/SecureGateway?topic=securegateway-acl).

Kehren Sie zu [Einführung - Client hinzufügen](/docs/services/SecureGateway?topic=securegateway-add-client) zurück.

## Clientbenutzerschnittstelle
{: #client-ui}

<b>Anmerkung:</b> Die Clientbenutzerschnittstelle wird nicht unterstützt, wenn Docker unter Windows oder Mac OS verwendet wird.

Von der Clientbenutzerschnittstelle wird eine lokale Webschnittstelle bereitgestellt, über die der Benutzer mit dem {{site.data.keyword.SecureGateway}}-Client interagieren kann, anstatt die Befehlszeilenschnittstelle zu verwenden.  Diese Benutzerschnittstelle ist standardmäßig unter `localhost:9003/dashboard` verfügbar; sie besteht aus den folgenden Seiten:

### Verbindung herstellen
{: #ui-connect}

Dies ist die erste Landing-Page für die Benutzerschnittstelle, auf der der Benutzer eine Gateway-ID und ein Sicherheitstoken für die Verbindung zum ersten Client angeben kann.

### Anmeldung
{: #ui-login}

Diese Seite wird angezeigt, wenn die Benutzerschnittstelle kennwortgeschützt ist.  Wenn diese Seite angezeigt wird, obwohl kein Kennwort erzwungen wird, aktualisieren Sie die Seite, damit Sie weitergeleitet werden.

### Dashboard
{: #ui-dashboard}

Dies ist die Hauptseite, sobald eine Verbindung zu einem Client hergestellt wurde.  Von dieser Seite aus können Sie auf die Seiten 'Protokolle anzeigen', 'Zugriffssteuerungsliste' und 'Verbindungsinformationen' zugreifen.  Unten auf der Seite können Sie auch eine Verbindung oder mehrere Verbindungen zu verbundenen Clients trennen.  Im oberen Bereich der Seite werden der derzeit ausgewählte Client sowie eine Option zum Herstellen von Verbindungen zu weiteren Clients angezeigt. 

### Protokolle anzeigen
{: #ui-logs}

Auf dieser Seite können Sie die Protokolle anzeigen, die vom ausgewählten Client (der in der rechten oberen Ecke der Seite angezeigt wird) generiert werden.  Die angezeigten Protokolle können mithilfe der Kontrollkästchen unter den Protokollen gefiltert werden.

### Zugriffssteuerungsliste
{: #ui-acl}

Auf dieser Seite können Sie die Zugriffssteuerungsliste für den ausgewählten Client (der in der rechten oberen Ecke der Seite angezeigt wird) bearbeiten.  Zu den Tabellen für das Zulassen bzw. Verweigern des Zugriffs können Regeln hinzugefügt werden; alternativ kann ganz unten auf der Seite eine Datei hochgeladen werden.

### Verbindungsinformationen
{: #ui-info}

Auf dieser Seite werden die aktuellen Verbindungsinformationen für den ausgewählten Client (der in der rechten oberen Ecke der Seite angezeigt wird) angezeigt.  Hier werden Informationen wie die Beschreibung des Gateways, die Anzahl der aktuellen Verbindungen und die Listener für umgekehrte Verbindungen angezeigt.

Kehren Sie zu [Einführung - Client hinzufügen](/docs/services/SecureGateway?topic=securegateway-add-client) zurück.

## Beendigung des fernen Clients
{: #client-remote}

Wenn für einen Client eine ID angegeben wurde, kann er über die SG-Benutzerschnittstelle oder über die SG-API über Fernzugriff beendet werden.  Wenn Sie einen Client beenden, der als Service ausgeführt wird, wird der Client erneut gestartet und erhält eine neue Client-ID; wenn der Service jedoch mit mehreren Clients verbunden ist, wird der beendete Client erst erneut gestartet, wenn alle verbleibenden Clients beendet wurden.

## Einschränkungen
{: #limits}

### Verbindungseinschränkungen
{: #limits-conn}

Vom SG-Gateway können nur 250 gleichzeitig bestehende Verbindungen verarbeitet werden. Wenn die Anzahl der gleichzeitigen  Anforderungen den Grenzwert überschreitet, kann dies das Zurückweisen von Verbindungsversuchen und eine Latenzzeit zur Folge haben. Eine einfache Möglichkeit zur Vermeidung dieser Situation ist die Bündelung von Verbindungen zur aufrufenden App. Beachten Sie, dass der Grenzwert der 250 gleichzeitig bestehenden Verbindungen für den Gateway und nicht für den Client oder das Ziel gilt. Dieser Grenzwert wird für alle Clients und Ziele auf dem Gateway gemeinsam genutzt.

### Einschränkungen für DataPower-Client
{: #limits-datapower}

Der DataPower-Client für {{site.data.keyword.SecureGateway}} wird derzeit aktualisiert, damit von ihm die Funktionen der restlichen Clients bereitgestellt werden können.  Er weist derzeit die folgenden Einschränkungen auf:

- Der Standardwert für die Zugriffssteuerungsliste ist ALLOW ALL
- Die Aktivierung und Inaktivierung von Gateways oder Zielen über die {{site.data.keyword.SecureGateway}}-Benutzerschnittstelle wird nicht unterstützt. Die Option für den Verwaltungsstatus als Funktion der DataPower-Benutzerschnittstelle kann jedoch als Ein-/Ausschalter für diesen Client verwendet werden.
- Die Abfrage des Verbindungsstatus wird bei Echtzeit-Statusaktualisierungen für verbundene und getrennte Gateways nicht unterstützt.
- Vollständige Zertifikatsketten mit TLS auf der Zielseite werden vor DataPower Version 7.5.1.0 nicht unterstützt.
- Cloudziele werden vor DataPower Version 7.5.1.0 nicht unterstützt.
- Die Protokollebene kann nicht in die Ebene TRACE geändert werden.
- Die neueste Version des Secure Gateway-Clients in DataPower ist 1.8.2fp1; weitere Informationen finden Sie [hier](/docs/services/SecureGateway?topic=securegateway-client-install#installing-datapower).
