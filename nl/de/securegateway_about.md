---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---

# Informationen zu {{site.data.keyword.SecureGateway}}
{: #about-sg}

## Verbindungssicherheit
Von {{site.data.keyword.SecureGatewayfull}} wird eine einzige persistente verschlüsselte Verbindung (TLS Version 1.2) zwischen dem {{site.data.keyword.SecureGateway}}-Client (im lokalen Netz) und den {{site.data.keyword.SecureGateway}}-Servern bereitgestellt. Diese bidirektionale Verbindung ermöglicht das sichere Übertragen von Daten zwischen den Cloudressourcen und den Vor-Ort-Ressourcen. Für die Verbindungen an den beiden Enden (zwischen Cloudressource und SG-Servern und zwischen lokalen Ressourcen und SG-Client) legt der Benutzer das Protokoll (TCP, TLS, HTTP, HTTPS, UDP) und die geltenden Sicherheitsmaßnahmen (gegenseitige Authentifizierung, iptable) fest.  

## Bidirektionale Unterstützung
Wenn das traditionelle lokale Ziel mit dem neuen Ziel in der Cloud kombiniert wird, wird eine vollständige bidirektionale Unterstützung bereitgestellt. Das neue Cloudziel ermöglicht es lokalen Services bzw. Anwendungen, Informationen oder Anfragen über den {{site.data.keyword.SecureGateway}}-Client an eine Cloudanwendung zu senden. Wenn ein neues Cloudziel erstellt wird, enthält das Ziel Angaben für `resource host` (Hostname oder IP der Cloudanwendung), `resource port` (Port, an dem die Anwendung empfangsbereit ist) und `client port` (Port, an dem der {{site.data.keyword.SecureGateway}}-Client empfangsbereit ist). Sobald dieses Ziel eingerichtet ist, kann der lokale Service eine Verbindung zum Client-Port herstellen und mit dem Senden von Daten beginnen, die über den Client an den {{site.data.keyword.SecureGateway}}-Server und danach an die angegebene Cloud-Anwendung gesendet werden.

Der Client-Port, der für ein Ziel angegeben wird, muss für das zugeordnete Gateway eindeutig sein. Ein einzelnes Gateway kann zum Beispiel nicht über zwei umgekehrte Ziele verfügen und an Port 12000 empfangsbereit sein, aber zwei Gateways können über über ein eigenes Ziel verfügen und an Port 12000 empfangsbereit sein. Falls im letzteren Fall jedoch beide Gateways mit demselben Client verbunden sind, kann nur ein Ziel an Port 12000 empfangsbereit sein.

### Cloudziel
![Cloudziel](./images/reverseDestination.png?raw=true "Cloudziel")

### Lokales Ziel
![Lokales Ziel](./images/onPremDestination.png?raw=true "Lokales Ziel")

## Hochverfügbarkeit
Erstellen Sie mehrere Clientverbindungen zu derselben Gateway-ID, um eine hohe Verfügbarkeit zu erreichen. Hierzu können mehrere Clients eines einzelnen Gateways eine Verbindung zu derselben Gateway-ID herstellen und/oder ein einzelner Mehrfach-Gateway-Client mehrere Verbindungen zu derselben Gateway-ID herstellen. Die Verbindungen werden im Umlaufverfahren an alle verbundenen Clients verteilt.
