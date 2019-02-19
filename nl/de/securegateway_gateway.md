---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---

# Gateway hinzufügen
{: #add-sg-gw}

Ein Gateway kann als Möglichkeit zum Identifizieren eines bestimmten Netzes oder einer bestimmten Umgebung betrachtet werden. Mit seiner Hilfe werden vom Secure Gateway-Client Verbindungen zu den Secure Gateway-Servern hergestellt und es kann mehrere Ressourcendefinitionen oder Ziele enthalten.

![Secure Gateway-Dashboard](./images/newDashboard.png?raw=true "Secure Gateway-Dashboard")

Klicken Sie im Dashboard von Secure Gateway auf die Schaltfläche 'Gateway hinzufügen', um die Anzeige 'Gateway hinzufügen' zu öffnen.

![Gateway hinzufügen](./images/addGateway.png?raw=true "Gateway hinzufügen")

Die einzige erforderliche Eingabe in dieser Anzeige ist der Name des Gateways. Standardmäßig sind `Sicherheitstoken erforderlich` und `Tokenablauf` ausgewählt.

Wenn ein Sicherheitstoken für die Verbindungen von Clients erforderlich ist, müssen Sie bei jedem Start des Secure Gateway-Clients sowohl die Gateway-ID als auch das Sicherheitstoken angeben. Falls Sie die Auswahl des Kästchens `Sicherheitstoken erforderlich` zurücknehmen, müssen Sie nur die Gateway-ID für den Client angeben, für den die Verbindung aufgebaut werden soll.

Das Standardablaufdatum für das Sicherheitstoken beträgt 90 Tage ab dem Datum seiner Erstellung. Falls Sie das Ablaufdatum ändern möchten, behalten Sie die Auswahl des Kästchens `Tokenablauf` bei und legen Sie im Textfeld für die Anzahl der Tage die von Ihnen gewünschte Anzahl ein (Minimum ist ein Tag, Maximum sind 365 Tage). Falls Sie ein Token erstellen möchten, das nicht abläuft, nehmen Sie die Auswahl des Kästchens `Tokenablauf` zurück.  

Wenn Sie die Erstellung des Gateways fertigstellen möchten, klicken Sie auf 'Gateway hinzufügen'. Sobald das Gateway erstellt wurde, wird von der Seite automatisch zum neuen Gateway navigiert.

![Neues Gateway](./images/newGateway.png?raw=true "Neues Gateway")

Da Sie jetzt über ein neu erstelltes Gateway verfügen, können Sie [eine Verbindung für den ersten Client erstellen](/docs/services/SecureGateway/securegateway_client.html).
