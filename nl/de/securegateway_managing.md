---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---

# {{site.data.keyword.SecureGateway}}-Service verwalten
{: #manage-sg-service}

## {{site.data.keyword.SecureGateway}}-Dashboard
Im {{site.data.keyword.SecureGateway}}-Dashboard können Sie schnell folgende Details anzeigen:

- Die aktuelle Anzahl der Verbindungen zu den Zielen für alle Gateways.
- Den eingehenden Datenfluss für alle Gateways in den letzten 12 Stunden.
- Den ausgehenden Datenfluss für alle Gateways in den letzten 12 Stunden.
- Ein Diagramm, in dem die letzten 12 Stunden der Nutzung pro Gateway anzeigt werden.
- Die vorhandenen Gateways, ihr aktueller Status und die Anzahl der Zieladressen, die sie enthalten.

![{{site.data.keyword.SecureGateway}}-Dashboard mit Nutzung](./images/dashboardUsage.png?raw=true "{{site.data.keyword.SecureGateway}}-Dashboard mit Nutzung")

### Serviceprofil
Wenn Sie auf die Schaltfläche 'Profil' klicken, wird Folgendes angezeigt:

Feld | Beschreibung 
-- | --
Planebene | Der derzeit ausgewählte Serviceplan.
Gateway-Grenzwert | Die Anzahl der Gateways, über die Sie verfügen können, ohne dass weitere Gebühren für zusätzliche Gateways abgerechnet werden. 
Clientgrenzwert | Die Anzahl der Clients, zu denen Sie mit einem einzigen Gateway eine Verbindung aufbauen können.
Zielgrenzwert | Die Anzahl der Ziele, über die ein einzelnes Gateway verfügen kann.
Datennutzungsgrenzwert | Die Datenübertragungsmenge (in GB), die vom Plan unterstützt wird, bevor aufgrund einer Überschreitung weitere Gebühren abgerechnet werden.
Gateway-Nutzungsüberschreitungen | Die aktuelle Anzahl der oberhalb des Gateway-Grenzwerts liegenden Gateways, für die Ihnen Gebühren berechnet werden.
Datennutzungsüberschreitungen | Die Anzahl der Datennutzungsüberschreitungen, die im Abrechnungszyklus des aktuellen Monats aufgetreten sind.

### Anzeige 'Gateway-Informationen'
Wenn Sie auf die Schaltfläche 'Einstellungen' auf einer der Gateway-Kacheln klicken, wird Folgendes angezeigt:

- Das Sicherheitstoken, das für die Verwendung der API oder für die Verbindung eines Clients erforderlich ist (hängt davon ab, ob das Token für Clientverbindungen erzwungen wird).
- Die Gateway-ID, die für die Verwendung der API oder die Verbindung eines Clients erforderlich ist.
- Der Knoten, auf dem das Gateway erstellt wurde. Hierbei handelt es sich um den Server, zu dem vom Client eine Verbindung hergestellt wird.
- Der Zeitpunkt, an dem das Gateway erstellt wurde und der Benutzer, der es erstellt hat.
- Der Zeitpunkt, an dem das Gateway geändert wurde und der Benutzer, der es geändert hat.
- Ein Link zum erneuten Generieren des Zertifikats und des Schlüssels, die beide dem Gateway zugeordnet sind. Diese Dateien werden nur mit traditionellen Zielen verwendet, die kein eigenes Zertifikats-/Schlüsselpaar für die gegenseitige Authentifizierung auf dem Client enthalten.

In der Anzeige 'Gateway-Informationen' können auch die folgenden Tasks ausgeführt werden:

- Bearbeiten oder Anzeigen weiterer Informationen zu Gateway und Sicherheitstoken. Wenn Sie diese Option auswählen, wird eine neue Anzeige angezeigt.
- Inaktivieren oder Aktivieren des Gateways.
- Löschen des Gateways.

## Dashboard für ein bestimmtes Gateway
Wenn Sie auf eine der Gateway-Kacheln klicken, wird das Dashboard für dieses konkrete Gateway angezeigt. Analog zum {{site.data.keyword.SecureGateway}}-Hauptdashboard können Sie schnell folgende Details anzeigen:

- Die aktuelle Anzahl der Verbindungen zu den Zielen auf diesem Gateway.
- Den eingehenden Datenfluss für dieses Gateway in den letzten 12 Stunden.
- Den abgehenden Datenfluss für dieses Gateway in den letzten 12 Stunden.
- Ein Diagramm, in dem die letzten 12 Stunden der Nutzung pro Ziel anzeigt werden.
- Die vorhandenen Ziele, ihr aktueller Status und ihre aktuelle Anzahl der aktiven Verbindungen.

![Dashboard für bestimmtes Gateway](./images/viewGateway.png?raw=true "Dashboard für bestimmtes Gateway")

### Anzeige 'Zielinformationen'
Wenn Sie auf die Schaltfläche 'Einstellungen' auf einer der Zielkacheln klicken, wird Folgendes angezeigt:

- Die Ziel-ID, die für die Verwendung der API erforderlich ist.
- Lokale Ziele verfügen über einen Host und einen Port in der Cloud. Diese Informationen sind für die Anwendung zum Herstellen einer Verbindung zur lokalen Ressource erforderlich.
- Cloudziele verfügen über einen Client-Port. Hierbei handelt es sich um den Port, an dem der {{site.data.keyword.SecureGateway}}-Client empfangsbereit ist, um eine Verbindung von der lokalen Anwendung zur Cloudressource herzustellen.
- Der Host und der Port der Ressource, auf die von diesem Ziel verwiesen wird.
- Der Zeitpunkt der Erstellung und der Benutzer, der das Ziel erstellt hat.
- Der Zeitpunkt der letzten Änderung sowie der Benutzer, der die Änderung vorgenommen hat.

In der Anzeige 'Zielinformationen' können auch die folgenden Tasks ausgeführt werden:

- Bearbeiten oder Anzeigen weiterer Informationen zum Ziel. Wenn Sie diese Option auswählen, wird eine neue Anzeige angezeigt.
- Inaktivieren oder Aktivieren des Ziels.
- Löschen des Ziels.
