---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Fehlerbehebung

## Bewährte Verfahren für die Ausführung des Secure Gateway-Clients
{: #best}

- Führen Sie den Secure Gateway-Client in einer Betriebssystempartition aus, in der die Services im Netz erkannt werden, die mit dem Client über Bridges verwendet werden. Von einigen gehosteten Virtualisierungsumgebungen werden zum Beispiel mehreren Modi für Netzkonnektivität unterstützt, unter anderem auch 'NAT' und 'Bridged'. Stellen Sie sicher, dass Sie den richtigen Verbindungstyp verwenden, der den Zugriff auf die {{site.data.keyword.Bluemix}}-Services im Internet bereitstellt.
- Installieren Sie Secure Gateway in einer IT-Umgebung, in der die unternehmensweite Sicherheitsrichtlinie dies erlaubt. In der Regel ist dies die eine geschützte gelbe Zone (Yellow Zone) oder DMZ, in der das Unternehmen die entsprechenden Sicherheitsmaßnahmen zum Schutz der lokalen Assets verwenden kann. Gehen Sie immer gemäß den unternehmensweiten Sicherheitsrichtlinien und Anweisungen vor, wenn Sie den Secure Gateway-Client installieren.
- Stellen Sie vor der Installation eines Clients in einer Umgebung sicher, dass sowohl auf das Internet als auch auf die lokalen Assets zugegriffen werden kann und dass alle Hostnamen über ein DNS aufgelöst werden können.

## Erste Schritte zur Fehlerbehebung

- Fehlgeschlagene Anforderung von anfordernder Anwendung starten
- Protokolle des Secure Gateway-Clients überprüfen
- Falls für die Anforderung keine Clientprotokolle generiert wurden, tritt das Problem zwischen der anfordernden Anwendung und den Secure Gateway-Servern auf. Hierbei kann es sich um ein Problem bei der Netzzuverlässigkeit, ein nicht übereinstimmendes Anforderungsprotokoll bis hin zu einem falschen TLS-Handshake bei der gegenseitigen Authentifizierung handeln.
- Falls vom Client Fehlerprotokolle für die Anforderung generiert wurden, tritt das Problem zwischen dem SG-Client und der lokalen Ressource auf. In der unten folgenden Tabelle werden gängige Fehler, die Ursachen für diese Fehler und die Möglichkeiten zu ihrer Behebung aufgeführt.

Fehler | Typische Ursache | Verfahren zur Fehlerbehebung
--- | --- | ---
ETIMEDOUT | Der Client kann den Hostnamen bzw. die IP-Adresse zum Aufbauen einer Verbindung aufgrund von Netzeinschränkungen nicht finden. | Versuchen Sie, den Hostnamen bzw. die IP-Adresse des Ziels von dem Host, auf dem der Client ausgeführt wird, mit Ping zu überprüfen, um die Netzkonnektivität sicherzustellen. Wenn die Docker-Version des Clients ausgeführt wird, kann das Problem durch die Überbrückung des Containers mit dem Hostbetriebssystem unter Verwendung von `--net=host` behoben werden.
ECONNREFUSED | Der Client hat den Hostnamen bzw. die IP-Adresse zum Aufbauen der Verbindung aufgelöst, aber der Verbindungshandshake kann nicht gestartet werden. | Dies wird in der Regel durch nicht übereinstimmende Protokolle des SG-Clients und der lokalen Ressource verursacht (Beispiel: der Client versucht, eine TCP-Verbindung zu einem Host/Port herzustellen, von dem eine TLS-Verbindung erwartet wird). In manchen Fällen kann eine Firewallregel diesen Fehler anstatt ETIMEDOUT verursachen.
ECONNRESET | Vom Client wurde eine Verbindung zum Ziel hergestellt, aber während des Handshakes (ein TLS-Handshakefehler kann auch zu unterschiedlichen Fehlern führen) oder während der Verarbeitung der Anforderung durch die lokale Ressource ist ein Fehler aufgetreten. | Die Protokolle der lokalen Ressource müssen überprüft werden, um sicherzustellen, dass die Unterbrechung der Verbindung nicht durch einen Fehler verursacht wurde. Falls in den lokalen Protokollen nichts gefunden wird, muss durch eine Überprüfung der Zielkonfiguration sichergestellt werden, dass die entsprechenden Protokolle (und Zertifikate, sofern erforderlich) dem Client für die Verbindung zur Verfügung gestellt werden.
REMOTE_RST | Auf einem Secure Gateway-Server ist ein Fehler aufgetreten. <br><br> Bei einem lokalen Ziel kann es sich um einen Fehler handeln, der auftritt, wenn die anfordernde App eine Verbindung zum SG-Server aufbaut oder wenn eine Zeitüberschreitung beim Empfangen von Daten von einer lokalen Ressource auftritt.<br><br> Bei einem Ziel in der Cloud kann es sich um Fehler im Zusammenhang mit einem TLS-Handshake bis zu Fehlern in der Cloudressource handeln. | Stellen Sie bei einem lokalen Ziel sicher, dass die anfordernde App mithilfe der passenden Protokolle eine Verbindung zum SG-Server herstellt; falls der Fehler beim Empfangen von Daten von einer lokalen Ressource auftritt, versuchen Sie, das Zeitlimit zu erweitern bzw. zu inaktivieren.<br><br> Bei einem Ziel in der Cloud müssen die Protokolle der Cloudressource überprüft werden, um sicherzustellen, dass die Unterbrechung der Verbindung nicht durch einen Fehler verursacht wurde. Falls in den Protokollen der Cloudressource nichts gefunden wird, muss durch eine Überprüfung der Zielkonfiguration sichergestellt werden, dass die entsprechenden Protokolle (und Zertifikate, sofern erforderlich) dem Client für die Verbindung zur Verfügung gestellt werden.

Viele Anwendungen sind blockiert, nachdem der Fehler ECONNRESET am anderen Ende des Tunnels aufgetreten ist. Hierbei handelt es sich um ein erwartetes Verhalten. Von Secure Gateway kann das RST-Paket am anderen Ende des Tunnels nicht wiederholt werden, da die TCP-Pakete für diese Seite des Tunnels bereits bestätigt wurden. Die einzige Methode zum Beenden der Blockierung sind Zeitlimitwerte auf Anwendungsebene, bei deren Verwendung die Anwendung nie eine Bestätigungsantwort erhält.

## Neustart des Docker-Clients bei Neustart des Servers konfigurieren
{: #docker}

### Ausgangssituation
Wenn Sie den Server erneut starten, auf dem der Secure Gateway-Client ausgeführt wird, müssen Sie den Docker-Client von Secure Gateway manuell erneut starten. Wie können Sie festlegen, dass der Client nach einem Neustart des Systems automatisch gestartet wird?

### Fehlerbehebung

- Unter Linux- oder UNIX-Systemen:
- Fügen Sie den Docker-Befehl in ein Script ein, das durch einen Cron-Job aufgerufen werden kann.
- Falls Sie eine Linux-Workstation verwenden, können Sie eine upstart-Konfiguration verwenden, um sicherzustellen, dass der Client bei jedem Neustart des Servers gestartet wird. Weitere Informationen finden Sie auf der Docker-Website im Abschnitt zum automatischen Starten von Containern (Automatically Start Containers).
- Setzen Sie unter Windows-Systemen den folgenden Befehl zum Starten des Clients ab:

```
for /L %i in (0,0,0) do docker run -it ibmcom/secure-gateway-client <gateway_id>
```
{: pre}

## Verbindungsfehlernachricht: Host befindet sich nicht im CN des Zertifikats
{: #not-in-cn}

### Ausgangssituation
Sie versuchen, TLS lokal auf der Clientseite mithilfe des Secure Gateway-Clients zu implementieren, und Sie erhalten die folgende Fehlernachricht.

```
[ERROR] Connection #<connection ID> had error: Host: <host name>. is not cert's CN: <mycommonname>

Where:
    - connection ID is a client assigned connection number.
    - host name is the host name for the connection.
    - mycommonname is the FDQN or common name that is used in the certificate.
```
{: screen}

### Entstehung des Problems
Der allgemeine Name (zum Beispiel der FQDN des Servers oder der eigene Name) der lokalen Anwendung und des Zertifikats, das Sie in {{site.data.keyword.Bluemix_notm}} für dieses Ziel hochladen, stimmen nicht überein.

- Überprüfen Sie die folgenden Punkte:
- Das Zertifikat wurde ordnungsgemäß generiert und der korrekte Server-FQDN bzw. Hostname werden verwendet.
- In das {{site.data.keyword.Bluemix_notm}}-Ziel für diesen Client wurde das korrekte Zertifikat hochgeladen.

### Fehlerbehebung

 1. Wechseln Sie in der {{site.data.keyword.Bluemix_notm}}-Benutzerschnittstelle zum Secure Gateway-Dashboard.
 2. Wählen Sie das entsprechende Ziel aus und klicken Sie auf das Symbol 'Bearbeiten'.
 3. Klicken Sie auf 'Zertifikat hochladen'.
 4. Laden Sie die PEM-Zertifikatsdatei hoch, die für den Verbindungsaufbau zum lokalen System verwendet werden soll.

## IP befindet sich nicht in Liste des Zertifikats
{: #san}

### Ausgangssituation
Der allgemeine Name im Zertifikat ist die IP-Adresse des Gateways, aber das Zertifikat weist keinen SAN (Subject Alternative Name) auf, der mit der IP-Adresse übereinstimmt; somit schlägt der Verbindungsversuch des Clients fehl.  

Da sich der Hostname nicht auflösen lässt, wird die IP-Adresse im Ziel verwendet. Der allgemeine Name im Zertifikat ist die IP-Adresse des Gateways, aber das Zertifikat weist keinen SAN (Subject Alternative Name) auf, der mit der IP-Adresse übereinstimmt; somit schlägt der Verbindungsversuch des Clients fehl.

Es wurde ein Ziel mit TLS erstellt, aber anstatt den Hostnamen des Ziels zu verwenden, wurde die IP-Adresse des Ziels verwendet. Beim Verbinden des Clients wird der folgende Fehler ausgelöst.

```
[2015-10-15 13:00:04.866] [INFO] Connection #0 is being established to 10.3.20.31:443
[2015-10-15 13:00:05.426] [ERROR] Connection #0 to destination 10.3.20.31:443 had error: IP: 10.3.20.31 is not in the cert's list:
[2015-10-15 13:00:05.427] [INFO] Connection #0 to 10.3.20.31:443 was closed
```
{: screen}

### Entstehung des Problems
Vom SSL-Überprüfungscode im Gateway-Client wird dieses Ziel anders behandelt, weil eine IP-Adresse anstelle eines Hostnamens verwendet wird. Anstatt einen Abgleich mit dem allgemeinen Namen des Zertifikats durchzuführen, wird der SAN des Zertifikats durchsucht, um einen Abgleich mit der IP-Adresse durchzuführen. Da das Zertifikat keinen SAN enthält, wird dies als schlechte Verbindung interpretiert, und der SSL-Handshake schlägt fehl.

### Fehlerbehebung
In der Fehlernachricht wird zwar nicht der allgemeine Name angegeben (Beispiel: [ERROR] Connection ## had error: Host: . is not cert&apos;s CN: ), aber die Liste der Zertifikate; dies deutet darauf hin, dass das selbst signierte Zertifikat fehlerhaft generiert wurde. Durch dieses Problem wird ein Zertifikat unter Verwendung eines FQDN oder allgemeinen Namens mit einer IP-Adresse generiert. Dies funktioniert nicht, da IP-Adressen nur unterstützt werden, wenn ein SAN (Subject Alternative Name) verwendet wird.

Methode zum Generieren eines Zertifikats mit einer IP als allgemeiner Name mit OpenSSL:

1. Erstellen Sie eine OpenSSL-Konfigurationsdatei (sie kann von '/usr/lib/ssl/openssl.cnf' kopiert werden).
2. Fügen Sie der Datei wie unten dargestellt den Abschnitt 'alternate_names' hinzu:

   ```
   [ alternate_names ]

   IP.1 = &lt;my application&apos;s ip&gt;
   ```
   {: codeblock}

3. Fügen Sie im Abschnitt [ v3_ca] die folgende Zeile hinzu:

    ```
    subjectAltName = @alternate_names
    ```
    {: pre}

4. Entfernen Sie unter Abschnitt 'CA_default' die Kopiererweiterungen 'copy_extensions' (verwenden Sie die Option zum Kopieren von Erweiterungen mit Sorgfalt):

    ```
    copy_extensions = copy
    ```
    {: pre}

5. Generieren Sie einen privaten Schlüssel:

    ```
    openssl genrsa -out private.key 3072
    ```
    {: pre}

6. Generieren Sie ein Zertifikat mit Optionen zur Organisation:

    ```
    openssl req -new -x509 -key private.key -sha256 -out certificate.pem -days 730 -config
    ```
    {: pre}

7. Kombinieren Sie die Dateien:

    ```
    cat private.key certificate.pem > SAN.pem
    ```
    {: pre}

8. Laden Sie die Datei 'SAN.pem' als Client-TLS-Zertifikat in das Ziel.
9. Laden Sie die Datei 'SAN.pem' in die lokale Anwendung und führen Sie einen Neustart durch.

10. Das Ziel kann entweder für TCP, HTTP oder HTTPS konfiguriert werden; die Anwendung auf der Cloudseite muss jetzt in der Lage sein, eine Verbindung zur lokalen Anwendung herzustellen.

Wenn der Fehler UNABLE_TO_VERIFY_LEAF_SIGNATURE auftritt, überprüfen Sie die OpenSSL-Datei und ändern Folgendes:

    ```
    keyUsage = digitalSignature, keyEncipherment
    ```
    {: pre}

in die Standardeinstellung:

    ```
    keyUsage = nonRepudiation, digitalSignature, keyEncipherment
    ```
    {: pre}

## Verbindungsfehlernachricht: DEPTH_ZERO_SELF_SIGNED_CERT
{: #depth-zero}

### Ausgangssituation
Sie versuchen, TLS lokal auf der Clientseite mithilfe des Secure Gateway-Clients zu implementieren, und Sie erhalten die folgende Fehlernachricht.

```
[ERROR] Connection #<connection ID> had error: DEPTH_ZERO_SELF_SIGNED_CERT Where:

    connection ID is a client assigned connection number.
```
{: screen}

### Entstehung des Problems
Im definierten Ziel fehlt ein clientseitiges Zertifikat.

### Fehlerbehebung
 1. Wechseln Sie in der {{site.data.keyword.Bluemix_notm}}-Benutzerschnittstelle zum Secure Gateway-Dashboard.
 2. Wählen Sie das entsprechende Ziel aus und klicken Sie auf das Symbol 'Bearbeiten'.
 3. Klicken Sie auf 'Zertifikat hochladen'.
 4. Laden Sie die PEM-Zertifikatsdatei hoch, die für den Verbindungsaufbau zum lokalen System verwendet werden soll.


## Wie kann eine ACL-Datei interaktiv in den Docker-Client geladen werden?
{: #docker-acl}

### Ausgangssituation
Da Docker ein Container oder eine virtualisierte Umgebung ist, kann von Docker erst dann direkt auf das Dateisystem zugegriffen werden, wenn der Container tatsächlich gestartet ist. So wird verhindert, dass das Dateisystem auf den Hostmaschinen von Docker gelesen wird, bis Docker tatsächlich gestartet und ausgeführt wird.

### Fehlerbehebung
So können Sie den Fehler beheben:

- Erstellen Sie eine Dockerfile, um die Datei 'aclfile.txt' einzuschließen:

```
FROM ibmcom/secure-gateway-client
ADD aclfile.txt /tmp/aclfile.txt
```
{: codeblock}

- Erstellen Sie ein neues Docker-Image:

```
docker build -t ads-secure-gateway-client .
```
{: pre}

- Führen Sie ein neues Docker-Image aus (Sie müssen die Optionen -t und -i angeben, da andernfalls Fehler auftreten, die Datei nicht gefunden wird oder ENOENT auftritt):

```
docker run -t -i ads-secure-gateway-client1  --F /tmp/aclfile.txt
```
{: pre}

- Das Ergebnis ist die folgende Ausgabe:

```
[2015-09-30 16:50:32.084] [INFO] The current access control list is being reset and replaced by the user provided file: /tmp/aclfile.txt
[2015-09-30 16:50:32.086] [INFO] The ACL file process accepts acl allow :8000
[2015-09-30 16:50:32.087] [INFO] The ACL file process accepts acl deny local
```
{: screen}

## Weitere Hilfe und Unterstützung anfordern
{: #support}

Wenn Sie technische Fragen zum Entwickeln oder Bereitstellen einer Anwendung mit Secure Gateway haben, posten Sie Ihre Frage auf [Stack Overflow ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://stackoverflow.com/search?q=securegateway+ibm-bluemix). Kennzeichnen Sie Ihre Frage mit "ibm-bluemix" und "secure-gateway", sodass sie von den {{site.data.keyword.Bluemix_notm}}-Entwicklerteams leichter gefunden werden können.

Wenn Sie Fragen zum Service oder zu den Anweisungen für den Einstieg haben, verwenden Sie das Forum [IBM developerWorks dW Answers ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/answers/topics/securegateway/?smartspace=bluemix) und die Markierungen "bluemix" und "Secure Gateway".

Weitere Details zur Verwendung dieser Foren finden Sie unter [Hilfeseite aufrufen](https://console.ng.bluemix.net/docs/support/index.html#getting-help).

Informationen zum Öffnen eines IBM Support-Tickets oder zu Supportebenen und Dringlichkeit für Tickets finden Sie in [Unterstützung anfordern](https://console.ng.bluemix.net/docs/support/index.html#contacting-support).

Geben Sie beim Erstellen eines Tickets möglichst viele der folgenden Informationen an:

- Unter welchem Betriebssystem wird der Client ausgeführt?
- Welche Version des Clients wird verwendet (dies kann mithilfe des Befehls 'C' auf dem Client festgestellt werden)?
- Wenn es sich um ein Problem der Benutzerschnittstelle handelt, fügen Sie alle zugehörigen Konsolenprotokolle und Screenshots des Browsers ein oder hängen Sie diese an.
- Fügen Sie alle zugehörigen Protokolle der anfordernden Anwendung ein oder hängen Sie diese an.
- Fügen Sie alle zugeordneten Protokolle des Secure Gateway-Clients ein oder hängen Sie diese an.
- Geben Sie die Details des verwendeten Ziels an (senden Sie entweder einen Screenshot oder füllen Sie die folgenden Felder aus):
   - Ziel-ID
   - Protokoll
   - Zielseitige Authentifizierung
   - Hochgeladene Zertifikate (nur Namen und Bereiche nach dem Hochladen)
