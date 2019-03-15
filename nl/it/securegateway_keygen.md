---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Gestione certificato/chiave
{: #cert-key-management}

## Generazione di una coppia certificato/chiave autofirmata
{: #self-signed-cert-gen}

Per generare una coppia certificato/chiave autofirmata, esegui questo comando:

```
openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 -keyout serverKey.pem -out serverCert.pem
```
{: pre}


## Caricamento di una coppia certificato/chiave sul browser
{: #upload-cert-to-browser}

Per accedere a una destinazione che implementa l'autenticazione reciproca, devi convertire la tua coppia certificato/chiave in un file PKCS#12 e caricarlo sul tuo browser.

Per creare il file PKCS#12, utilizza il seguente comando:

```
openssl pkcs12 -export -in <path_to_cert> -inkey <path_to_key> -name "my-sg-pair" -out <path_to_write_to>.p12
```
{: pre}

In Firefox, il file .p12 può essere caricato in Opzioni > Preferenze > Avanzate > Certificati > Mostra certificati > Certificati personali > Importa

In Chrome, il file .p12 può essere caricato in Impostazioni > HTTPS/SSL > Gestisci certificati > Importa

In Internet Explorer, il file p12 può essere caricato in Impostazioni > Contenuto > Certificati > Importa
