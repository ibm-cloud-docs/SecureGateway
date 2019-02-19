---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---

# {{site.data.keyword.SecureGateway}}-Änderungsprotokoll
{: #secure-gateway-change-log}

## Version 1.7.0 Fixpack 1

### Korrekturen

- Behebt den Fehler `EADDRINUSE`, der von Listenern verursacht wird, die beim Löschen des Ziels nicht ordnungsgemäß umgerüstet werden.
- Beheben Sie das Problem, das auftritt, wenn an eine Anwendung gebundene Serviceinstanzen nicht gelöscht werden können.

## Version 1.7.0

### Features

- Führt neue Servicepläne ein: Essentials, Professional und Enterprise.
- Vom Secure Gateway-Client wird jetzt ein SOCKS-Proxy zwischen ihm selbst und einem lokalen Ziel unterstützt.

## Version 1.6.0 Fixpack 1

### Korrekturen

- Behebt das Problem, das auftritt, wenn das Trennen der Verbindungen mehrerer Clients zu verwaisten Prozessen in den verbundenen Client-Arrays führt.
- Verwaiste Verbindungen, die fälschlicherweise angehaltene Listener zur Folge hatten, werden jetzt bereinigt.

## Version 1.6.0

### Features

- Von der Zugriffssteuerungsliste wird jetzt die spezifische Pfadweiterleitung für HTTP- bzw. HTTPS-Anforderungen unterstützt.
- Von Zielen werden jetzt Servernamensindikatoren (Server Name Indicators, SNSs) für TLS-Verbindungen unterstützt.

## Version 1.5.1

### Features

- Zur Erstellung von Zielen wurde ein Zielassistent hinzugefügt.
- Von Gateways wird jetzt das Hochladen eines angepassten Zertifikats-/Schlüsselpaars unterstützt.
- Die Services sind jetzt auf 50 Gateways und 50 Ziele pro Gateway beschränkt.
- Ziele, Gateways und Services können jetzt exportiert und importiert werden.
- Latenztests zwischen Client und Server können über die Secure Gateway-Benutzerschnittstelle ausgeführt werden.

## Version 1.5.0 Fixpack 1

### Korrekturen

- Bereinigt verwaiste Tunnelprozesse in der Netzverbindung.
- Beseitigt eine doppelte Portzuordnung, die durch die fehlgeschlagene Löschung eines Ziels verursacht wird.
- Behebt einen HTTP- bzw. HTTPS-Fehler bei Nicht-UTF8-Verschlüsselung.
- Behebt Probleme durch fehlende IPC-Handler bei Ersetzungsprozessen.

## Version 1.5.0

### Features

- Der Client verfügt jetzt über eine lokale interaktive Benutzerschnittstelle.
- Jetzt werden UDP-Verbindungen unterstützt.
- Der Speicherbedarf des Clients wurde erheblich reduziert.
- Der Einzelclientmodus wird nicht mehr unterstützt; die Verwendung des Modus für mehrere Clients ist die Standardeinstellung und für einen Client mit einem einzelnen Gateway transparent.
- Die Zugriffssteuerungsliste wird zwischen Clients synchronisiert, die mit demselben Gateway verbunden sind.

## Version 1.4.2

### Features

- Den Clients wird jetzt eine ID zugeordnet, wenn eine Verbindung zum SG-Server hergestellt wird.
- Clients können jetzt fern über eine API oder über die Secure Gateway-Benutzerschnittstelle beendet werden.
- Der Verbindungsstatus eines Clients kann jetzt über eine API überprüft werden.
- Die gegenseitige Authentifizierung wird jetzt bei zielseitiger TLS-Verschlüsselung unterstützt.
- Die Protokolle der gegenseitigen Authentifizierung werden jetzt von Cloudzielen unterstützt.
