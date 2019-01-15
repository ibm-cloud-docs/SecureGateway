---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Zertifikats- und Schlüsselverwaltung

## Selbst signiertes Zertifikats-/Schlüsselpaar generieren

Führen Sie den folgenden Befehl aus, um ein selbst signiertes Zertifikats-/Schlüsselpaar zu generieren:

```
openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 -keyout serverKey.pem -out serverCert.pem
```
{: pre}


## Zertifikats-/Schlüsselpaar in den Browser hochladen

Wenn Sie auf ein Ziel zugreifen möchten, von dem eine gegenseitige Authentifizierung erzwungen wird, müssen Sie das Zertifikats-/Schlüsselpaar in eine Datei im PKCS#12-Format umwandeln und diese in den Browser hochladen.

Verwenden Sie den folgenden Befehl, um die PKCS#12-Datei zu erstellen:

```
openssl pkcs12 -export -in <path_to_cert> -inkey <path_to_key> -name "my-sg-pair" -out <path_to_write_to>.p12
```
{: pre}

In Firefox kann die .p12-Datei unter Verwendung von 'Extras' > 'Einstellungen' > 'Datenschutz und Sicherheit' > 'Zertifikate' > 'Zertifikate anzeigen' > 'Ihre Zertifikate' > 'Importieren' hochgeladen werden.

In Chrome kann die .p12-Datei unter Verwendung von 'Einstellungen' > 'HTTPS/SSL' > 'Zertifikate verwalten' > 'Import' hochgeladen werden.

In Internet Explorer kann die .p12-Datei unter Verwendung von 'Einstellungen' > 'Inhalt' > 'Zertifikate' > 'Import' hochgeladen werden.
