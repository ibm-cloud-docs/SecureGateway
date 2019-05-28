---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-07"

---

# Voraussetzungen für die Ausführung des Clients
{: #client-requirements}

## Systemvoraussetzungen
{: #system-requirements}

Der Secure Gateway-Client wird in den folgenden Umgebungen unterstützt:

| Name | Versionen          |
| ------------- | ----------- |
| Windows Desktop | 7, 8.1, 10 und höher |
| Windows Server | 2012 R2 und höher |
| Red Hat Linux | 7.3 und höher |
| CentOS | 7.3 und höher |
| SuSE Linux | 12 und höher |
| Ubuntu Linux | 14.04 (LTS) und höher |
| Power Machine | ppc64el-Architektur |
| Ubuntu Z-Linux | - |
| AIX | 7.1 TL4 und höher |
| Mac OS X | 10.10 (Yosemite) und höher |
| Docker | 1.7.0 und höher, alle unterstützten Betriebssysteme |

<b>Anmerkung:</b> Für native Clientinstallationen werden derzeit nur 64-Bit-Umgebungen unterstützt.

## Netzanforderungen
{: #network-requirements}

Vom Secure Gateway-Client werden die abgehenden Ports 443 und 9000 verwendet, um eine Verbindung zur npm-Registry und zur {{site.data.keyword.Bluemix}}-Umgebung herzustellen:
- Port `443` für npm-Installation
  - Während der Installation wird vom Installationsprogramm eine Verbindung zur npm-Registry hergestellt und `npm install` ausgeführt, um die für den Secure Gateway-Client erforderlichen Abhängigkeiten zu installieren. Stellen Sie vor der Installation sicher, dass von der Maschine, auf der der Client installiert wird, eine Verbindung zur Website einer npm-Registry hergestellt werden kann. npm ist standardmäßig für die Verwendung der öffentlichen Registry von npm, Inc. konfiguriert, die unter `https://registry.npmjs.org` verfügbar ist. <br><br>
Falls in Ihrer Umgebung ein npm-Unternehmensserver vorhanden ist, nehmen Sie alle Abhängigkeiten des Secure Gateway-Clients auf dem npm-Unternehmensserver in die Whitelist auf. Die Liste der Abhängigkeiten finden Sie in der Datei `<Installation_directory>\ibm\securegateway\client\package.json`.<br><br>

- Port `443` für Gateway-Authentifizierung
  - Für SG-Client `v180fp9 und früher`


  | Region  | Host  |
  | --  | --  |
  | Vereinigte Staaten (Süden)  | sgmanager.ng.bluemix.net  |
  | Vereinigte Staaten (Osten)  | sgmanager.us-east.bluemix.net  |
  | Vereintes Königreich  | sgmanager.eu-gb.bluemix.net  |
  | Deutschland  | sgmanager.eu-de.bluemix.net  |
  | Sydney  | sgmanager.au-syd.bluemix.net  |

  - Für SG-Client `v181 und später`
  
  
  | Region  | Host  |
  | --  | --  |
  | Vereinigte Staaten (Süden)  | sgmanager.us-south.securegateway.cloud.ibm.com  |
  | Vereinigte Staaten (Osten)  | sgmanager.us-east.securegateway.cloud.ibm.com  |
  | Vereintes Königreich  | sgmanager.eu-gb.securegateway.cloud.ibm.com  |
  | Deutschland  | sgmanager.eu-de.securegateway.cloud.ibm.com  |
  | Sydney  | sgmanager.au-syd.securegateway.cloud.ibm.com  |

- Port `9000`

  Der Knoten des SG-Gateways, der in der Konfiguration des Gateways enthalten ist. Da sich die Gateways nicht alle auf demselben Knoten befinden, müssen Sie den Hostnamen des Knotens bei jeder Erstellung des Gateways bestätigen.


Stellen Sie sicher, dass Sie zusätzliche Firewallregeln und IP-Tabellenregeln überprüfen oder ändern, falls diese zutreffen. Es ist jedoch nicht empfehlenswert, Regeln via IP festzulegen; Regeln sollten speziell für den Hostnamen und den Port festgelegt werden, da die IPs für die Gateway-Authentifizierung und das SG-Gateway von Bluemix gesteuert werden und Änderungen unterliegen. Wenn die Netzadministratoren aktuelle IPs für einen bestimmten Hostnamen benötigen, [setzen Sie sich mit dem Support in Verbindung, um diese für Ihre Umgebung anzufordern](/docs/services/SecureGateway?topic=securegateway-troubleshooting#getting-help-and-support).


## Hardwarevoraussetzungen ermitteln
{: #hardware-requirements}

Die Spezifikationen der Maschine, auf der der Secure Gateway-Client ausgeführt wird, hängen im Wesentlichen vom Datenverkehr ab, der über jede Verbindung übertragen wird.  Jede Clientinstanz ist ein einzelner Prozess, von dem bis zu 250 gleichzeitig bestehende Verbindungen bereitgestellt werden.  Zur Schätzung eines durchschnittlichen Maximums, das die Maschine unterstützen muss, bestimmen Sie die durchschnittliche Größe einer Transaktion auf dem Client (sowohl Anforderung als auch Antwort) und skalieren diese anschließend auf 250 gleichzeitig ablaufende Transaktionen.  In Anbetracht der Tatsache, dass diese Größe eine Kombination aus Anfragen und Antworten ist, darf der Client diesen Speicherbedarf nicht überschreiten.  Für präzisere Schätzungen sollte eine Mischung aus Anfragegrößen und Antwortgrößen verwendet werden, um ein realistischeres Transaktionsszenario zu simulieren.

Für die Ausführung einer einzelnen Instanz des Secure Gateway-Clients wird ein Minimum von 2 Kernen und 4 GB Speicher empfohlen.

Für Hochverfügbarkeitsszenarios, in denen mehrere Instanzen des Clients auf derselben Maschine ausgeführt werden, wird ein Minimum von 4 Kernen und 8 GB Speicher empfohlen.
