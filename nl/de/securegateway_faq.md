---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-29"

---

# Häufig gestellte Fragen (FAQs)
{: #sg-faq}

## OSI-Modellschicht
{: #faq-osi}

### Frage
{: #osi-question}
Welche Schicht des OSI-Modells stellt der Secure Gateway-Service dar?

### Antwort
{: #osi-answer}
Der Secure Gateway-Service stellt Schicht 4 des OSI-Modells dar.

## Unterstützung der TLS-Version
{: #faq-tls}

### Frage
{: #tls-question}
Welche TLS-Version wird vom Secure Gateway-Service unterstützt?

### Antwort
{: #tls-answer}
Vom Secure Gateway-Service wird die TLS-Version 1.2 unterstützt.

## Wann ist es sinnvoll, mein Gateway oder Ziel zu inaktivieren?
{: #faq-disabled}

### Frage
{: #disabled-question}

Warum sollte ein Gateway oder Ziel inaktiviert werden?

### Antwort
{: #disabled-answer}
Es kann aus den folgenden Gründen sinnvoll sein, ein Ziel oder Gateway zu inaktivieren:

- Sie erstellen ein Ziel, aber Sie haben die Sicherheit noch nicht konfiguriert.  In diesem Fall kann es sinnvoll sein, das Ziel so lange zu inaktivieren, bis die Sicherheit konfiguriert ist.
- Der Service soll für die Benutzer nicht verfügbar sein, weil Sie gerade Aktualisierungen am Service vornehmen.  In diesem Fall können Sie die erforderlichen Gateways vorübergehend inaktivieren und warten, bis der Service aktualisiert wurde.
- Sie haben alle Gateways und Ziele am Front-End eingerichtet, aber das Back-End wurde noch nicht erstellt.  In diesem Fall ist es sinnvoll, die Gateways oder Ziele zu inaktivieren und erst dann zu aktivieren, wenn die Erstellung des Back-Ends abgeschlossen ist.

Weitere Informationen zum Inaktivieren eines Gateways oder Ziels finden Sie unter [Vorgehensweise zum Verwalten der Secure Gateway-Serviceinstanz](/docs/services/SecureGateway?topic=securegateway-manage-sg-service).

## Welcher Ansatz wird für die Erstellungsautomatisierung für mehrere Bereiche empfohlen?
{: #faq-automation-spaces}

### Frage
{: #automation-spaces-question}
Eine Kundenumgebung verfügt über eine Organisation und drei Bereiche.  Ein Bereich ist für die Entwicklung, ein weiterer für das Staging und der letzte für die Produktion vorgesehen.  Soll der Kunde eine einzelne Secure Gateway-Instanz oder mehrere erstellen (zum Beispiel eine für jeden Bereich)?  Gibt es Überlegungen zur Wiederverwendung einer Node.js-Anwendung zum Erstellen eines Gateways und einer Anwendung in jedem Bereich, wenn der Kunde mehrere Gateways erstellen kann?

### Antwort
{: #automation-spaces-answer}

- Sie können für alle drei Bereiche eine einzige Secure Gateway-Instanz erstellen.  Sie müssen hierbei jedoch die [Einschränkungen für den jeweiligen Plan](/docs/services/SecureGateway?topic=securegateway-secure-gateway-service-plans) in Bezug auf Gateways und Ziele berücksichtigen.
- Hinsichtlich einer Wiederverwendung der Node.js-Anwendung müssen keine weiteren Überlegungen berücksichtigt werden, da für Secure Gateway keine Servicebindungen erforderlich sind.


## Welcher Ansatz wird für die Erstellungsautomatisierung für mehrere Organisationen empfohlen?
{: #faq-automation-orgs}

### Frage
{: #automation-orgs-question}
Eine Kundenumgebung weist drei Organisationen auf: eine für die Entwicklung, eine für das Staging und eine für die Produktion.  Ist für jede einzelne Organisation eine Secure Gateway-Serviceinstanz erforderlich und ist die Konfiguration für alle Bereiche in dieser Organisation verfügbar?

### Antwort
{: #automation-orgs-answer}

- Es muss nicht in jeder Secure Gateway-Serviceinstanz eine Organisation vorhanden sein. In einer Organisation kann eine Instanz vorhanden sein und Sie können die Gateways in dieser Instanz von allen anderen Umgebungen aus verwenden.  Bei Verwendung dieser Konfiguration müssen Sie jedoch die [Einschränkungen für den jeweiligen Plan](/docs/services/SecureGateway?topic=securegateway-secure-gateway-service-plans) in Bezug auf Gateways und Ziele berücksichtigen.
- In einzelnen Organisation kann eine Secure Gateway-Serviceinstanz vorhanden sein und die Konfiguration ist in allen Bereichen verfügbar.

## Muss sich meine App in demselben Bereich befinden?
{: #faq-app-space}

### Frage
{: #app-space-question}
Muss die Node.js-App in demselben {{site.data.keyword.Bluemix_notm}}-Bereich wie der Secure Gateway-Service ausgeführt werden?

### Antwort
{: #app-space-answer}
Nein, die App muss nicht in demselben {{site.data.keyword.Bluemix_notm}}-Bereich wie der Secure Gateway-Service ausgeführt werden.

## Können die Secure Gateway-Serverprotokolle abgerufen werden?
{: #faq-server-logs}

### Frage
{: #server-logs-question}
Können Protokoll auf Fehlerebene für den Secure Gateway-Server abgerufen werden?

### Antwort
{: #server-logs-answer}
Die Protokoll auf Fehlerebene auf dem Server können nicht abgerufen werden.  Es können nur Fehler angezeigt werden, die zum Zeitpunkt der Anforderung gemacht wurden.

## Welche Angaben kann Secure Gateway für Statusangaben zur Funktion aufweisen?
{: #faq-states}

### Frage
{: #states-question}
Was sind die verschiedenen Lebenszyklusstatusangaben von Gateways und Zielen?

### Antwort
{: #states-answer}

#### Status 'Nicht funktionsbereit'
{: #states-answer-non-functional}
Mit Release 1.7.0 wurde ein neues Preismodell mit Stufenplänen eingeführt. Im Rahmen dieses Modells ist es möglich, sowohl Gateways als auch Ziele als 'Aktiv' oder 'Inaktiv' zu markieren. Die neue Struktur für den Abrechnungsplan bringt es mit sich, dass dem Benutzer für die Anzahl der Gateways und Ziele Gebühren berechnet werden, die für ihn bereitgehalten werden.

- Inaktive Posten werden nicht abgerechnet.
- Inaktive Ziele verfügen nicht über einen bereitgestellten Cloud-Port.
- Alle Ziele in einem inaktiven Gateway sind ebenfalls inaktiv und können erst wieder aktiviert werden, wenn auch das Gateway aktiviert wird.
- Inaktive Posten werden als 'Nicht funktionsbereit' angesehen. Inaktive Posten können sich nicht im Status 'Funktionsbereit' befinden.

#### Status 'Funktionsbereit'
{: #states-answer-functional}
Wenn ein Gateway oder Ziel als aktiv markiert ist, wird es in Rechnung gestellt. Nachfolgend werden aktive Statusangaben für Gateways und Ziele aufgeführt:

- Aktiviert - Das Gateway oder Ziel ist unter normalen Umständen betriebsbereit.
- Inaktiviert - Das Gateway oder Ziel wurde inaktiviert.
    - Gateways - Von Clients können keine Daten an das inaktivierte Gateway übertragen werden.
    - Ziele - Von Clients können keine Verbindungen zu diesen inaktivierten Zielen hergestellt werden.

## Wie können die Datenaktivitäten des Secure Gateway-Clients ermittelt werden?
{: #faq-data-size}

### Frage
{: #data-size-question}
Wie kann festgestellt werden, welche Datenaktivitäten vom Secure Gateway-Clients ausgeführt werden?

### Antwort
{: #data-size-answer}
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
{: #faq-connect-num}

### Frage
{: #connect-num-question}
Wie können Verbindungsinformationen ermittelt werden, z. B. wie viele Echtzeit-Verbindungen vom Secure Gateway-Client verwendet werden und wie viele Daten gesendet und empfangen werden?

### Antwort
{: #connect-num-answer}

- Gehen Sie in der interaktiven Befehlszeile des Secure Gateway-Clients wie folgt vor:
Geben Sie `s` ein, um die Details zum Verbindungsstatus zu drucken. 
- Klicken Sie in der Secure Gateway-Clientbenutzerschnittstelle auf das Menü 'Verbindungsinformationen'.

Die folgenden Verbindungsstatistikdaten werden angezeigt: 
- Gesamte übertragene Datenmenge
- Gesamte empfangene Datenmenge
- Gesamte Anzahl der Verbindungen
- Anzahl der aktiven Echtzeit-Verbindungen.

Anmerkung: Diese Verbindungsinformationen beziehen sich auf die Clientebene, nicht auf die Gateway-Ebene. Falls Sie Verbindungsinformationen für die Gateway-Ebene benötigen, prüfen Sie alle Clients, die eine Verbindung zu diesem Gateway haben.

## Welche Konfigurationen werden zum Schützen von Verbindungen empfohlen?
{: #faq-secure-app}

### Frage
{: #secure-app-question}
Welche Konfigurationen werden zum Schützen von Verbindungen empfohlen?

### Antwort
{: #secure-app-answer}

#### Gegenseitige Authentifizierung verwenden
{: #secure-app-answer-ma}
Die Aktivierung der gegenseitigen Authentifizierung für beide Seiten der lokalen Ziele erhöht die Sicherheit in Secure Gateway. Aktivieren Sie auf der Seite 'Benutzerauthentifizierung' die gegenseitige Authentifizierung, um den Zugriff auf den Secure Gateway-Cloudknoten durch die Authentifizierung mithilfe eines Clientzertifikats zu beschränken, wenn die Anforderung über TLS/HTTPS gesendet wird. Aktivieren Sie auf der Seite 'Ressourcenauthentifizierung' die gegenseitige Authentifizierung, um einen entsprechenden Berechtigungsnachweis bereitzustellen, wenn eine Verbindung zum Zielendpunkt hergestellt wird, und stellen Sie den sicheren bzw. verschlüsselten Zugriff auf die lokale Ressource sicher. Weitere Informationen finden Sie in [Gegenseitige Authentifizierung konfigurieren](/docs/services/SecureGateway?topic=securegateway-add-dest#dest-mutual-auth) und [Gegenseitige TLS-Authentifizierung mit Node.js](/docs/services/SecureGateway?topic=securegateway-nodejs-tls-ma#nodejs-tls-ma).

#### Regeln für IP-Tabelle festlegen (für lokales Ziel)
{: #secure-app-answer-iptables}
Da sich Host und Port der Secure Gateway-Cloud eines lokalen Ziels im öffentlichen Bereich befinden, ist der Zugriff standardmäßig nicht begrenzt.
Wenn Sie den Datenverkehr kontrollieren möchten, über den auf Secure Gateway zugegriffen wird, legen Sie iptables-Regeln fest, damit nur von bestimmten IPs und Ports aus zugegriffen werden kann, um die lokalen Ressourcen zu schützen. Weitere Informationen zum Konfigurieren der iptables-Regeln für Secure Gateway finden Sie in [IP-Tabellenregeln](/docs/services/SecureGateway?topic=securegateway-add-dest#dest-network-security).

#### Zugriffssteuerungsliste konfigurieren (für lokales Ziel)
{: #secure-app-answer-acl}
Konfigurieren Sie die Unterstützung für die Zugriffssteuerungsliste, um den Zugriff auf lokale Ressourcen zu erteilen oder zu beschränken; durch das Angeben einer Zugriffsberechtigung für Host und Port eines bestimmten Ziels werden die lokalen Ziele sicherer. Es wird empfohlen, die zulässigen oder einschränkten HTTP- bzw. HTTPS-Routen in den ACL-Einträgen zu definieren, um die Sicherheit von lokalen Zielen zu erhöhen. Weitere Informationen hierzu finden Sie in [Zugriffssteuerungsliste](/docs/services/SecureGateway?topic=securegateway-acl#acl) und in [HTTP- und HTTPS-Routensteuerung mithilfe der Zugriffssteuerungsliste](/docs/services/SecureGateway?topic=securegateway-acl#acl-route-control).

#### Kennwort für Secure Gateway-Clientbenutzerschnittstelle festlegen
{: #secure-app-answer-ui-pw}
Es wird empfohlen, ein Benutzerschnittstellenkennwort festzulegen, um den Zugriff auf die Benutzerschnittstelle des Secure Gateway-Clients zu beschränken. Weitere Details um Festlegen des Kennworts mithilfe der Startkonfiguration oder interaktiver Befehle in der Terminalbefehlszeile des Secure Gateway-Clients finden Sie in [Interaktion mit dem Client](/docs/services/SecureGateway?topic=securegateway-client-interacting#client-interacting).

## Was ist eine Gateway-Migration? Aus welchem Grund wurde die Domäne nach Dezember 2018 geändert?
{: #faq-gateway-migration}

### Frage
{: #gateway-migration-question}
Seit der Wartung im Dezember 2018 gibt es eine Schaltfläche 'Migrieren' in der Gateway-Anzeige; welche Funktion hat diese Schaltfläche? Aus welchem Grund wurde die Domäne geändert?

### Antwort
{: #gateway-migration-answer}

Nach der Wartung im Dezember 2018 wurde der Cloud-Host von Secure Gateway so umbenannt, dass die Domäne `securegateway.appdomain.cloud` und nicht mehr die Domäne `integration.ibmcloud.com` verwendet wird; zudem wird zur Gateway-Authentifizierung `securegateway.cloud.ibm.com` und nicht mehr `bluemix.net` verwendet. Aus Gründen der Abwärtskompatibilität verwendet das bereits vorhandene Gateway weiterhin die alte Domäne, bis das Gateway migriert wurde; der SG-Client v180fp9 und frühere Versionen verwenden zur Gateway-Authentifizierung weiterhin `bluemix.net`.

Nach der Migration wird der Cloud-Host der lokalen Ziele so geändert, dass die neue Domäne verwendet wird; die Benutzer/Anwendungen müssen so aktualisiert werden, dass die Anforderung an den neuen Cloud-Host gesendet wird.

Derzeit ist die Migration nicht obligatorisch und es gibt kein genaues Datum, an dem die Unterstützung für die alte Domäne eingestellt wird; sobald die Unterstützung jedoch eingestellt ist, werden die Kunden, die noch den alten Domänennamen verwenden, benachrichtigt.

## Wie können Benachrichtigungen empfangen werden?
{: #faq-notification}

### Frage
{: #notification-question}
Wo erhalte ich Secure Gateway-Benachrichtigungen, insbesondere Benachrichtigungen dazu, ob Wartungsmaßnahmen geplant sind, die zu einer Betriebsunterbrechung führen?

### Antwort
{: #notification-answer}

Benachrichtigungen erhalten Sie über die [Statusseite](https://cloud.ibm.com/status?selected=status). 
- Wenn Sie Benachrichtigungen zu abgeschlossenen bzw. laufenden Wartungsmaßnahmen, die zu einer Betriebsunterbrechung führen, erhalten möchten, suchen Sie nach `Secure Gateway` auf der Registerkarte `Status`.
- Wenn Sie Benachrichtigungen zu geplanten Wartungsmaßnahmen, die zu einer Betriebsunterbrechung führen, erhalten möchten, suchen Sie nach `Secure Gateway` auf der Registerkarte `Geplante Wartung`.

Wenn es zu einer unerwarteten Unterbrechung der Verbindung zum Secure Gateway-Client kommt, rufen Sie die Statusseite auf und prüfen Sie, ob derzeit Wartungsmaßnahmen, die zu einer Betriebsunterbrechung führen, ausgeführt werden. 

Wenn für die Wartungsmaßnahmen eine Unterbrechung von mehr als 10 Minuten erforderlich ist, müssen Sie möglicherweise den Secure Gateway-Client manuell erneut starten, um die Verbindung zum Secure Gateway-Server nach der Wartung wiederherzustellen. Normalerweise beträgt die Ausfallzeit des Service höchstens 10 Minuten und der Secure Gateway-Client (nach Version v180) kann in der Regel die Verbindung zum Secure Gateway-Server automatisch wiederherstellen. 

## Wie kann ich die Secure Gateway-Clientprotokolle in DataPower erfassen?
{: #faq-dp-log}

### Frage
{: #dp-log-question}
Wie kann ich die Secure Gateway-Clientprotokolle erfassen und in eine Datei in DataPower schreiben?

### Antwort
{: #dp-log-answer}

Die Ereigniskategorie der Secure Gateway-Clientprotokolle ist `sgclient`. Sie können ein [Protokollziel](https://www.ibm.com/support/knowledgecenter/en/SS9H2Y_7.7.0/com.ibm.dp.doc/logtarget_logs.html) erstellen, um die Protokolle mit der spezifischen Ereigniskategorie in eine Datei zu schreiben. Ein Beispiel ist nachfolgend aufgeführt:

- In der Standarddomäne: 
    - Wählen Sie in der Seitenleiste der grafischen Benutzerschnittstelle `Objekt` → `Protokollierungskonfiguration` → `Protokollziel` aus. Oder suchen Sie im Feld `Suche` nach `Protokollziel`. 
    - Wählen Sie die Schaltfläche `Hinzufügen` aus, um ein Protokollziel hinzuzufügen. 
- Auf der `Hauptregisterkarte`:
    - Geben Sie den `Namen` ein. 
    - Geben Sie `Datei` als `Zieltyp` an. 
    - Geben Sie `Text` als `Protokollformat` an. 
    - Geben Sie den `Dateinamen` ein, um die Ausgabeposition zu definieren, z. B. `logtemp:///sgclient.log`. 
    - Wählen Sie `Rotation` als `Archivierungsmodus` aus. 
- Auf der Registerkarte `Ereignisabonnement`:
    - Geben Sie den `Namen` ein. 
    - Wählen Sie die Schaltfläche `Hinzufügen` aus, um ein Zielereignisabonnement hinzuzufügen. 
    - Wählen Sie `sgclient` als `Ereigniskategorie` aus. 
    - Geben Sie `Debugging` als `Minimale Ereignispriorität` ein. 
