---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---

# {{site.data.keyword.SecureGateway}}-Änderungsprotokoll
{: #secure-gateway-change-log}

## Version 1.7.0 Fixpack 1
{: #v171fp1}

### Korrekturen
{: #v171fp1-fixes}

- Behebt Fehler des Typs `EADDRINUSE`, die durch Listener verursacht wurden, die eine Umrüstung nicht ordnungsgemäß ausführen, wenn ein Ziel gelöscht wird.
- Beheben Sie das Problem, das auftritt, wenn an eine Anwendung gebundene Serviceinstanzen nicht gelöscht werden können.

## Version 1.7.0
{: #v170}

### Features
{: #v170-features}

- Führt neue Servicepläne ein: Essentials, Professional und Enterprise.
- Vom Secure Gateway-Client wird jetzt ein SOCKS-Proxy zwischen ihm selbst und einem lokalen Ziel unterstützt.

## Version 1.6.0 Fixpack 1
{: #v160fp1}

### Korrekturen
{: #v160fp1-fixes}

- Behebt das Problem, das auftritt, wenn das Trennen der Verbindungen mehrerer Clients zu verwaisten Prozessen in den verbundenen Client-Arrays führt.
- Verwaiste Verbindungen, die fälschlicherweise angehaltene Listener zur Folge hatten, werden jetzt bereinigt.

## Version 1.6.0
{: #v160}

### Features
{: #v160-features}

- Von der Zugriffssteuerungsliste wird jetzt die spezifische Pfadweiterleitung für HTTP- bzw. HTTPS-Anforderungen unterstützt.
- Von Zielen werden jetzt Servernamensindikatoren (Server Name Indicators, SNSs) für TLS-Verbindungen unterstützt.

## Version 1.5.1
{: #v151}

### Features
{: #v151-features}

- Zur Erstellung von Zielen wurde ein Zielassistent hinzugefügt.
- Von Gateways wird jetzt das Hochladen eines angepassten Zertifikats-/Schlüsselpaars unterstützt.
- Die Services sind jetzt auf 50 Gateways und 50 Ziele pro Gateway beschränkt.
- Ziele, Gateways und Services können jetzt exportiert und importiert werden.
- Latenztests zwischen Client und Server können über die Secure Gateway-Benutzerschnittstelle ausgeführt werden.

## Version 1.5.0 Fixpack 1
{: #v150fp1}

### Korrekturen
{: #v150fp1-fixes}

- Bereinigt verwaiste Tunnelprozesse in der Netzverbindung.
- Beseitigt eine doppelte Portzuordnung, die durch die fehlgeschlagene Löschung eines Ziels verursacht wird.
- Behebt einen HTTP- bzw. HTTPS-Fehler bei Nicht-UTF8-Verschlüsselung.
- Behebt Probleme durch fehlende IPC-Handler bei Ersetzungsprozessen.

## Version 1.5.0
{: #v150}

### Features
{: #v150-features}

- Der Client verfügt jetzt über eine lokale interaktive Benutzerschnittstelle.
- Jetzt werden UDP-Verbindungen unterstützt.
- Der Speicherbedarf des Clients wurde erheblich reduziert.
- Der Einzelclientmodus wird nicht mehr unterstützt; die Verwendung des Modus für mehrere Clients ist die Standardeinstellung und für einen Client mit einem einzelnen Gateway transparent.
- Die Zugriffssteuerungsliste wird zwischen Clients synchronisiert, die mit demselben Gateway verbunden sind.

## Version 1.4.2
{: #v142}

### Features
{: #v142-features}

- Den Clients wird jetzt eine ID zugeordnet, wenn eine Verbindung zum SG-Server hergestellt wird.
- Clients können jetzt fern über eine API oder über die Secure Gateway-Benutzerschnittstelle beendet werden.
- Der Verbindungsstatus eines Clients kann jetzt über eine API überprüft werden.
- Die gegenseitige Authentifizierung wird jetzt bei zielseitiger TLS-Verschlüsselung unterstützt.
- Die Protokolle der gegenseitigen Authentifizierung werden jetzt von Cloudzielen unterstützt.
