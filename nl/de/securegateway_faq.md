---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-08"

---

# Häufig gestellte Fragen (FAQs)

## OSI-Modellschicht
{: #osi}

### Frage
Welche Schicht des OSI-Modells stellt der Secure Gateway-Service dar?

### Antwort
Der Secure Gateway-Service stellt Schicht 4 des OSI-Modells dar.

## Unterstützung der TLS-Version
{: #tls}

### Frage
Welche TLS-Version wird vom Secure Gateway-Service unterstützt?

### Antwort
Vom Secure Gateway-Service wird die TLS-Version 1.2 unterstützt.

## Wann ist es sinnvoll, mein Gateway oder Ziel zu inaktivieren?
{: #disabled}

### Frage

Warum sollte ein Gateway oder Ziel inaktiviert werden?

### Antwort
Es kann aus den folgenden Gründen sinnvoll sein, ein Ziel oder Gateway zu inaktivieren:

- Sie erstellen ein Ziel, aber Sie haben die Sicherheit noch nicht konfiguriert. In diesem Fall kann es sinnvoll sein, das Ziel so lange zu inaktivieren, bis die Sicherheit konfiguriert ist.
- Der Service soll für die Benutzer nicht verfügbar sein, weil Sie gerade Aktualisierungen am Service vornehmen. In diesem Fall können Sie die erforderlichen Gateways vorübergehend inaktivieren und warten, bis der Service aktualisiert wurde.
- Sie haben alle Gateways und Ziele am Front-End eingerichtet, aber das Back-End wurde noch nicht erstellt. In diesem Fall ist es sinnvoll, die Gateways oder Ziele zu inaktivieren und erst dann zu aktivieren, wenn die Erstellung des Back-Ends abgeschlossen ist.

Weitere Informationen zum Inaktivieren eines Gateways oder Ziels finden Sie unter [Vorgehensweise zum Verwalten der Secure Gateway-Serviceinstanz](/docs/services/SecureGateway/securegateway_managing.html).

## Welcher Ansatz wird für die Erstellungsautomatisierung für mehrere Bereiche empfohlen?
{: #automation-spaces}

### Frage
Eine Kundenumgebung verfügt über eine Organisation und drei Bereiche. Ein Bereich ist für die Entwicklung, ein weiterer für das Staging und der letzte für die Produktion vorgesehen. Soll der Kunde eine einzelne Secure Gateway-Instanz oder mehrere erstellen (zum Beispiel eine für jeden Bereich)? Gibt es Überlegungen zur Wiederverwendung einer Node.js-Anwendung zum Erstellen eines Gateways und einer Anwendung in jedem Bereich, wenn der Kunde mehrere Gateways erstellen kann?

### Antwort

- Sie können für alle drei Bereiche eine einzige Secure Gateway-Instanz erstellen. Sie müssen hierbei jedoch die [Einschränkungen für den jeweiligen Plan](/docs/services/SecureGateway/securegateway_plans.html) in Bezug auf Gateways und Ziele berücksichtigen.
- Hinsichtlich einer Wiederverwendung der Node.js-Anwendung müssen keine weiteren Überlegungen berücksichtigt werden, da für Secure Gateway keine Servicebindungen erforderlich sind.


## Welcher Ansatz wird für die Erstellungsautomatisierung für mehrere Organisationen empfohlen?
{: #automation-orgs}

### Frage
Eine Kundenumgebung weist drei Organisationen auf: eine für die Entwicklung, eine für das Staging und eine für die Produktion. Ist für jede einzelne Organisation eine Secure Gateway-Serviceinstanz erforderlich und ist die Konfiguration für alle Bereiche in dieser Organisation verfügbar?

### Antwort

- Es muss nicht in jeder Secure Gateway-Serviceinstanz eine Organisation vorhanden sein. In einer Organisation kann eine Instanz vorhanden sein und Sie können die Gateways in dieser Instanz von allen anderen Umgebungen aus verwenden. Bei Verwendung dieser Konfiguration müssen Sie jedoch die [Einschränkungen für den jeweiligen Plan](/docs/services/SecureGateway/securegateway_plans.html) in Bezug auf Gateways und Ziele berücksichtigen.
- In einzelnen Organisation kann eine Secure Gateway-Serviceinstanz vorhanden sein und die Konfiguration ist in allen Bereichen verfügbar.

## Muss sich meine App in demselben Bereich befinden?
{: #app-space}

### Frage
Muss die Node.js-App in demselben {{site.data.keyword.Bluemix_notm}}-Bereich wie der Secure Gateway-Service ausgeführt werden?

### Antwort
Nein, die App muss nicht in demselben {{site.data.keyword.Bluemix_notm}}-Bereich wie der Secure Gateway-Service ausgeführt werden.

## Können die Secure Gateway-Serverprotokolle abgerufen werden?
{: #server-logs}

### Frage
Können Fehlerprotokolle für den Secure Gateway-Server abgerufen werden?

### Antwort
Die Fehlerprotokolle auf dem Server können nicht abgerufen werden. Es können nur Fehler angezeigt werden, die zum Zeitpunkt der Anforderung gemacht wurden.

## Welche Angaben kann Secure Gateway für Statusangaben zur Funktion aufweisen?
{: #states}

### Frage
Was sind die verschiedenen Lebenszyklusstatusangaben von Gateways und Zielen?

### Antwort

#### Status 'Nicht funktionsbereit'
Mit Release 1.7.0 wurde ein neues Preismodell mit Stufenplänen eingeführt. Im Rahmen dieses Modells ist es möglich, sowohl Gateways als auch Ziele als 'Aktiv' oder 'Inaktiv' zu markieren. Die neue Struktur für den Abrechnungsplan bringt es mit sich, dass dem Benutzer für die Anzahl der Gateways und Ziele Gebühren berechnet werden, die für ihn bereitgehalten werden.

- Inaktive Posten werden nicht abgerechnet.
- Inaktive Ziele verfügen nicht über einen bereitgestellten Cloud-Port.
- Alle Ziele in einem inaktiven Gateway sind ebenfalls inaktiv und können erst wieder aktiviert werden, wenn auch das Gateway aktiviert wird.
- Inaktive Posten werden als 'Nicht funktionsbereit' angesehen. Inaktive Posten können sich nicht im Status 'Funktionsbereit' befinden.

#### Status 'Funktionsbereit'
Wenn ein Gateway oder Ziel als aktiv markiert ist, wird es in Rechnung gestellt. Nachfolgend werden aktive Statusangaben für Gateways und Ziele aufgeführt:

- Aktiviert - Das Gateway oder Ziel ist unter normalen Umständen betriebsbereit.
- Inaktiviert - Das Gateway oder Ziel wurde inaktiviert.
    - Gateways - Von Clients können keine Daten an das inaktivierte Gateway übertragen werden.
    - Ziele - Von Clients können keine Verbindungen zu diesen inaktivierten Zielen hergestellt werden.

## Wie können die Datenaktivitäten des Secure Gateway-Clients ermittelt werden?
{: #data-size}

### Frage
Wie kann festgestellt werden, welche Datenaktivitäten vom Secure Gateway-Clients ausgeführt werden?

### Antwort
Ändern Sie auf dem Secure Gateway-Client die Protokollebene in TRACE. Nach dem Senden von Anforderungen werden die folgenden Informationen angezeigt.

Von der Anforderungsanwendung gesendete Datenmenge:
```
[TRACE] Connection #<connection ID> transmitted data: <size> bytes
```

Vom Ziel gesendete Datenmenge:
```
[TRACE] Connection #<connection ID> received data: <size> bytes
```

## Wie kann festgestellt werden, wie viele Echtzeit-Verbindungen von Secure Gateway verwendet werden?
{: #connect-num}

### Frage
Wie kann ermittelt werden, wie viele Echtzeit-Verbindungen vom Secure Gateway-Client verwendet werden und wie viele Daten gesendet und empfangen werden?

### Antwort

- Gehen Sie in der interaktiven Befehlszeile des Secure Gateway-Clients wie folgt vor:
Geben Sie `s` ein, um die Details zum Verbindungsstatus zu drucken. 
- Klicken Sie in der Secure Gateway-Clientbenutzerschnittstelle auf das Menü 'Verbindungsinformationen'.

Die folgenden Verbindungsstatistikdaten werden angezeigt: 
- Gesamte übertragene Datenmenge
- Gesamte empfangene Datenmenge
- Gesamte Anzahl der Verbindungen
- Anzahl der aktiven Echtzeit-Verbindungen.

Anmerkung: Die Anzahl der aktuellen Verbindungen wird in der Secure Gateway-Benutzerschnittstelle nicht in Echtzeit wiedergegeben. Verwenden Sie die oben beschriebenen Methoden auf dem Secure Gateway-Client, um die Echtzeitverbindungsinformationen abzurufen.

## Welche Konfigurationen werden zum Schützen von Verbindungen empfohlen?
{: #secure-app}

### Frage
Welche Konfigurationen werden zum Schützen von Verbindungen empfohlen?

### Antwort

#### Gegenseitige Authentifizierung verwenden
Die Aktivierung der gegenseitigen Authentifizierung für beide Seiten der lokalen Ziele erhöht die Sicherheit in Secure Gateway. Aktivieren Sie auf der Seite 'Benutzerauthentifizierung' die gegenseitige Authentifizierung, um den Zugriff auf den Secure Gateway-Cloudknoten durch die Authentifizierung mithilfe eines Clientzertifikats zu beschränken, wenn die Anforderung über TLS/HTTPS gesendet wird. Aktivieren Sie auf der Seite 'Ressourcenauthentifizierung' die gegenseitige Authentifizierung, um einen entsprechenden Berechtigungsnachweis bereitzustellen, wenn eine Verbindung zum Zielendpunkt hergestellt wird, und stellen Sie den sicheren bzw. verschlüsselten Zugriff auf die lokale Ressource sicher. Weitere Informationen finden Sie in [Gegenseitige Authentifizierung konfigurieren](/docs/services/SecureGateway/securegateway_destination.html#mutual-auth) und [Gegenseitige TLS-Authentifizierung mit Node.js](/docs/services/SecureGateway/securegateway_tls-ma.html#node-js-tls-mutual-authentication).

#### Regeln für IP-Tabelle festlegen (für lokales Ziel)
Da sich Host und Port der Secure Gateway-Cloud eines lokalen Ziels im öffentlichen Bereich befinden, ist der Zugriff standardmäßig nicht begrenzt. Wenn Sie den Datenverkehr kontrollieren möchten, über den auf Secure Gateway zugegriffen wird, legen Sie iptable-Regeln fest, damit nur von bestimmten IPs und Ports aus zugegriffen werden kann, um die lokalen Ressourcen zu schützen. Weitere Informationen zum Konfigurieren der iptable-Regeln für Secure Gateway finden Sie in [IP-Tabellenregeln](/docs/services/SecureGateway/securegateway_destination.html#configuring-network-security).

#### Zugriffssteuerungsliste konfigurieren (für lokales Ziel)
Konfigurieren Sie die Unterstützung für die Zugriffssteuerungsliste, um den Zugriff auf lokale Ressourcen zu erteilen oder zu beschränken; durch das Angeben einer Zugriffsberechtigung für Host und Port eines bestimmten Ziels werden die lokalen Ziele sicherer. Es wird empfohlen, die zulässigen oder einschränkten HTTP- bzw. HTTPS-Routen in den ACL-Einträgen zu definieren, um die Sicherheit von lokalen Zielen zu erhöhen. Weitere Informationen hierzu finden Sie in [Zugriffssteuerungsliste](/docs/services/SecureGateway/securegateway_acl.html#access-control-list) und [HTTP- und HTTPS-Routensteuerung mithilfe der Zugriffssteuerungsliste](/docs/services/SecureGateway/securegateway_acl.html#routes).

#### Kennwort für Secure Gateway-Clientbenutzerschnittstelle festlegen
Es wird empfohlen, ein Benutzerschnittstellenkennwort festzulegen, um den Zugriff auf die Benutzerschnittstelle des Secure Gateway-Clients zu beschränken. Weitere Details um Festlegen des Kennworts mithilfe der Startkonfiguration oder interaktiver Befehle in der Terminalbefehlszeile des Secure Gateway-Clients finden Sie in [Interaktion mit dem Client](/docs/services/SecureGateway/securegateway_interaction.html#interacting-with-the-client).
