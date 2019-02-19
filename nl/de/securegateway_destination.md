---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---

# Ziel hinzufügen

Ein Ziel ist eine Definition für die Vorgehensweise zur Herstellung einer Verbindung zu einer bestimmten lokalen Ressource oder Cloudressource. Sobald das Ziel erstellt ist, wird es von den {{site.data.keyword.SecureGateway}}-Servern mit einem eindeutigen öffentlichen Endpunkt bereitgestellt, an dem es für Verbindungen empfangsbereit ist, während eine Verbindung zum Gateway besteht.

![Dashboard ohne Ziele](./images/emptyDestinations.png?raw=true "Dashboard ohne Ziele")

Klicken Sie vom neuen Gateway aus in der Registerkarte 'Ziele' auf die Schaltfläche 'Ziel hinzufügen', um die Anzeige 'Ziel hinzufügen' zu öffnen. Ein Ziel kann auf zwei Arten erstellt werden: Mithilfe einer Installationsanleitung, in der beschrieben wird, wie alle Bestandteile zu einer Gesamtarchitektur zusammengefügt werden und mithilfe einer erweiterten Konfiguration, von der alle Felder und Optionen in einer einzigen Anzeige bereitgestellt werden.  

Bei Verwendung der Installationsanleitung ist die Konfiguration von Proxy-Informationen, Servernamensindikatoren oder das Hochladen eines zielspezifischen Paars aus Zertifikat und Schlüssel <b>nicht möglich</b>. Nach der Erstellung sind alle Felder über die Anzeige 'Ziel bearbeiten' verfügbar.

## Anzeige der Installationsanleitung

![Installationsanleitung](./images/guidedLanding.png?raw=true "Startanzeige der Installationsanleitung")

## Anzeige der erweiterten Konfiguration

![Erweiterte Konfiguration](./images/advancedLanding.png?raw=true "Startanzeige der erweiterten Konfiguration")

## Vergleich zwischen lokalen Zielen und Zielen in der Cloud
{: #dest-types}

Die erste Frage, die Sie beim Erstellen eines Ziels beantworten müssen, ist die nach der Position der Ressource, zu der Sie eine Verbindung herstellen müssen.

### Lokales Ziel
Ein lokales Ziel ist für einen Anwendungsfall vorgesehen, bei dem eine Anwendung im öffentlichen Bereich Zugriff auf eine lokale Ressource benötigt, deren Zugriff beschränkt ist.
![Lokales Ziel](./images/onPremDestination.png?raw=true "Lokales Ziel")


### Cloudziel
Das Cloudziel ist für einen Anwendungsfall vorgesehen, bei dem von einer Anwendung in einem beschränkten Netz auf eine Ressource zugegriffen werden muss, die sich im öffentlichen Bereich befindet.
![Umgekehrtes Ziel](./images/reverseDestination.png?raw=true "Cloudziel")

## Ziel definieren
Für beide Zielarten sind die folgenden Informationen erforderlich:

- <b>Ressourcenhostname</b>: Hierbei handelt es sich um die IP-Adresse oder den Hostnamen der Ressource, zu der Sie eine Verbindung herstellen müssen.
- <b>Ressourcenport</b>: Hierbei handelt es sich um den Port, an dem die Ressource empfangsbereit ist.
- <b>Protokoll</b>: Hierbei handelt es sich um die Art der Verbindung, die von der Anwendung hergestellt wird. In der folgenden Tabelle finden Sie Informationen zu den unterschiedlichen [Protokolloptionen](#protocols). Informationen zum Konfigurieren des Verbindungstyps, der von der Ressource erwartet wird, finden Sie im Abschnitt [Ressourcenauthentifizierung](#resource-auth).

Wenn Sie ein Cloudziel ausgewählt haben, müssen Sie auch einen <b>Client-Port</b> angeben. Hierbei handelt es sich um den Port, an dem der {{site.data.keyword.SecureGateway}}-Client empfangsbereit ist, um alle Verbindungen zum zugeordneten Hostnamen und Port der Ressource zuzulassen.

## Protokolloptionen
{: #protocols}

Diese Tabelle enthält alle verfügbaren Optionen für die Art und Weise, in der die Anwendung Verbindungen/Anforderungen mit {{site.data.keyword.SecureGateway}} aufbauen kann.

Protokoll | Beschreibung 
-- | --
TCP | Keine Authentifizierung. Ihre Anwendung kann direkt mit den {{site.data.keyword.SecureGateway}}-Servern kommunizieren, Zertifikate sind nicht erforderlich.
TLS: Serverseitig | Transport Layer Security (TLS) ist aktiviert und vom Server wird ein Zertifikat als Nachweis für die Authentizität bereitgestellt.
TLS: Gegenseitige Authentifizierung | Vom Server werden mehrere Zertifikate bereitgestellt und auch von der Anwendung muss eine Zertifikat für die Verbindung bereitgestellt werden.
HTTP | TCP-Verbindung, bei der der Host-Header neu geschrieben wird, damit er mit dem lokalen Hostnamen übereinstimmt.
HTTPS | Eine Verbindung des Typs 'TLS: Serverseitig', bei der der Host-Header neu geschrieben wird, damit er mit dem lokalen Hostnamen übereinstimmt.
HTTPS: Gegenseitige Authentifizierung | Eine Verbindung des Typs 'TLS: Gegenseitige Authentifizierung', bei der der Host-Header neu geschrieben wird, damit er mit dem lokalen Hostnamen übereinstimmt.


## Gegenseitige Authentifizierung konfigurieren
{: #mutual-auth}

Bei Protokollen, von denen eine gegenseitige Authentifizierung erzwungen wird, müssen Sie Ihr eigenes Zertifikat hochladen; alternativ wird vom Server automatisch ein selbst signiertes Zertifikats-/Schlüsselpaar zur Verwendung durch die Anwendung erstellt. Dieses Paar kann mit dem Serverzertifikat heruntergeladen werden.
![Anzeigen für gegenseitige Authentifizierung](./images/mutualAuth.png?raw=true "Anzeigen für gegenseitige Authentifizierung")

### Benutzerauthentifizierung
{: #user-auth}

Der Abschnitt für die Benutzerauthentifizierung ist zum Verwalten der Autorisierung der Anwendung konzipiert, wenn diese eine Anforderung an {{site.data.keyword.SecureGateway}} sendet oder eine Verbindung aufbaut. In diesem Feld wird ein einziges Zertifikat akzeptiert; es muss das Zertifikat sein, das von der Anwendung zusammen mit jeder Verbindung bzw. Anforderung präsentiert wird.

### Ressourcenauthentifizierung
{: #resource-auth}

Durch die Ressourcenauthentifizierung wird festgelegt, wie {{site.data.keyword.SecureGateway}} versucht, eine Verbindung zu einer definierten Ressource herzustellen. Es stehen drei Optionen zur Verfügung: 'Keine', 'TLS: Serverseitig' und 'TLS: Gegenseitige Authentifizierung'. Je nach Auswahl sind unterschiedliche Authentifizierungsoptionen verfügbar.

Die Aktivierung von TLS für die Verbindung zur Ressource ist von der Verwendung der TLS-Option für die Benutzerauthentifizierung unabhängig. Durch TLS für die Benutzerauthentifizierung wird der Zugriff zwischen der ursprünglich anfordernden Anwendung und {{site.data.keyword.SecureGateway}} gesichert (zum Beispiel zwischen einer {{site.data.keyword.Bluemix_notm}}-App und {{site.data.keyword.SecureGateway}}-Servern); von TLS für die Ressourcenauthentifizierung wird dagegen die Verbindung zwischen {{site.data.keyword.SecureGateway}} und der definierten Ressource gesichert (zum Beispiel zwischen einem {{site.data.keyword.SecureGateway}}-Client und einer lokalen Datenbank).

#### Cloudauthentifizierung/Lokale Authentifizierung

Diese Option wird verfügbar, wenn Sie für 'Ressourcenauthentifizierung' die Option 'TLS' oder 'Gegenseitige Authentifizierung' auswählen. Der Name des Felds ist mit dem [Typ des Ziels](#dest-types) identisch, den Sie ausgewählt haben. Dieses Feld ermöglicht das Hochladen von bis zu 6 Zertifikaten, um das Zertifikat der Ressource zu validieren, zu der Sie eine Verbindung herstellen. Diese Dateien werden für die Zertifizierungsstelle der jeweiligen Verbindung zur Ressource hinzugefügt und müssen das Zertifikat oder die Zertifikatskette enthalten, das bzw. die von der Ressource angezeigt wird.

#### Servernamensindikator (SNI)
Diese Option wird verfügbar, wenn Sie für 'Ressourcenauthentifizierung' die Option 'TLS' oder 'Gegenseitige Authentifizierung' auswählen. Sie wird verwendet, damit für den TLS-Handshake der Ressourcenverbindung ein separater Hostname verwendet werden kann.

### Clientzertifikat und -schlüssel
Ob die Felder für Clientzertifikat und Clientschlüssel angezeigt werden, hängt vom [Typ des Ziels](#dest-types) ab, den Sie ausgewählt haben. In beiden Fällen werden die hier bereitgestellten Dateien vom SG-Client für seine Identifizierung bei TLS-Verbindungen verwendet. Falls keine Dateien hochgeladen werden, wird von den {{site.data.keyword.SecureGateway}}-Servern automatisch ein selbst signiertes Paar mit der CN-Angabe `localhost` generiert. Für Anweisungen zum Generieren eines Paars aus Zertifikat und Schlüssel [klicken Sie hier](/docs/services/SecureGateway/securegateway_keygen.html).

Bei einem lokalen Ziel wird es unter 'Ressourcenauthentifizierung' angezeigt, wenn für 'Ressourcenauthentifizierung' die Option 'Gegenseitige Authentifizierung' ausgewählt wurde. In diesem Fall verwendet der Client dieses Zertifikats-/Schlüsselpaar für seine abgehende Verbindung zur definierten Ressource. Die Zertifizierungsstelle dieser Verbindung enthält die Zertifikate, die im Feld [Cloudauthentifizierung/Lokale Authentifizierung](#resource-auth) bereitgestellt werden.

Bei einem Cloudziel wird es unter 'Benutzerauthentifizierung' angezeigt, wenn ein TLS-Protokoll ausgewählt wurde. In diesem Fall verwendet der Client dieses Zertifikats-/Schlüsselpaar, um TLS-Listener mit der Datei zu erstellen, die in der Zertifizierungsstelle zur [Benutzerauthentifizierung](#user-auth) hochgeladen wurde.  

## Netzsicherheit konfigurieren
Damit nur bestimmte IP-Adressen eine Verbindung zum Host und Port in der Cloud herstellen können, können Sie auf dem lokalen Ziel iptable-Regeln erzwingen.
![Anzeige für Netzsicherheit](./images/networkSecurity.png?raw=true "Anzeige für Netzsicherheit")

Wenn Sie iptable-Regeln erzwingen möchten, markieren Sie das Kontrollkästchen <b>Cloudzugriff auf dieses Ziel durch iptable-Regeln beschränken</b> in der Anzeige 'Netzsicherheit'. Sobald das Kontrollkästchen aktiviert ist, können Sie mit dem Hinzufügen der IPs beginnen, für die ein Verbindungsaufbau zulässig ist. Wenn keine IPs angegeben werden, werden alle Verbindungen zu diesem Host und Port in der Cloud abgelehnt, solange das Kontrollkästchen <b>Cloudzugriff beschränken</b> markiert ist.

<b>Anmerkung:</b> Die bereitgestellten IPs oder Ports müssen externe IP-Adressen sein, die von den {{site.data.keyword.SecureGateway}}-Servern erkannt werden können; sie dürfen nicht die lokale IP-Adresse der Maschine sein, von der die Anforderung erstellt wird.

### iptable-Regeln hinzufügen
Wenn Sie Regeln zu den IP-Tabellen (iptables) hinzufügen, können Sie zusammen mit einem einzelnen Port oder einem Portbereich auch einzelne IPs oder einen IP-Bereich angeben. Alle angegebenen Bereiche sind inklusiv. In der folgenden Tabelle werden einige Beispiele und Angaben zur Auflösung in den IP-Tabellen (iptables) dargestellt:

IP-Adressen | Ports | Ergebnisse
-- | -- | --
1.2.3.4 | 5000 | Nur IP 1.2.3.4 ist an Port 5000 zulässig.
1.2.3.4-2.3.4.5 | 5000 | Alle IPs zwischen IP 1.2.3.4 und 2.3.4.5 sind an Port 5000 zulässig.
1.2.3.4 | 5000:10000 | Nur IP 1.2.3.4 ist an den Ports 5000 bis 10000 zulässig.
1.2.3.4-2.3.4.5 | 5000:10000 | Alle IPs zwischen IP 1.2.3.4 und 2.3.4.5 sind an den Ports 5000 bis 10000 zulässig.
1.2.3.4 | | Nur IP 1.2.3.4 ist an allen Ports zulässig.
| 5000 | Jede IP ist an Port 5000 zulässig.

Einer Anwendung können auch bestimmte Regeln zugeordnet werden. Weitere Informationen zum Erstellen zugeordneter Regeln finden Sie unter [Vorgehensweise zum Erstellen von iptable-Regeln für die Anwendung](./iptables.html).

## Proxy-Optionen konfigurieren
Falls sich das lokale Ziel hinter einem SOCKS-Proxy befindet, können Sie die Proxy-Einstellungen für das Ziel in der Anzeige 'Proxy-Optionen' konfigurieren.
![Anzeige für Proxy-Optionen](./images/proxyOptions.png?raw=true "Anzeige für Proxy-Optionen")

Zum Konfigurieren der Proxy-Einstellungen benötigen Sie nur den Hostnamen und den Port, an dem der Proxy empfangsbereit ist, und das SOCKS-Protokoll, das verwendet wird (4, 4a, 5).

## Zieleinstellungen
Klicken Sie nach der Erstellung des Ziels auf das Einstellungssymbol, um die folgenden Informationen anzuzeigen:

- Die Ziel-ID, die für die Verwendung der API erforderlich ist.
- <b>Lokale Ziele verfügen über einen Host und einen Port in der Cloud. Diese Informationen sind für die Anwendung zum Herstellen einer Verbindung zur lokalen Ressource erforderlich. </b>
- <b>Cloudziele verfügen über einen Client-Port. Hierbei handelt es sich um den Port, an dem der {{site.data.keyword.SecureGateway}}-Client empfangsbereit ist, um eine Verbindung von der lokalen Anwendung zur Cloudressource herzustellen.</b>
- Der Host und der Port der Ressource, auf die von diesem Ziel verwiesen wird.
- Der Zeitpunkt der Erstellung und der Benutzer, der das Ziel erstellt hat.
- Der Zeitpunkt der letzten Änderung sowie der Benutzer, der die Änderung vorgenommen hat.
