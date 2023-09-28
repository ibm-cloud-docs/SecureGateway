---

copyright:
  years: 2015, 2021
lastupdated: "2021-03-17"

subcollection: SecureGateway

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:deprecated: .deprecated}
{:external: target="_blank" .external}

# Cert/Key Management
{: #cert-key-management}

{{site.data.keyword.SecureGateway}} is deprecated. For more information, see the [deprecation details](/docs/SecureGateway?topic=SecureGateway-dep-overview).
{: deprecated}

## Generating a self-signed certifiate/key pair
{: #self-signed-cert-gen}

To generate a self-signed certificate/key pair, run the following command:

```
openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 -keyout serverKey.pem -out serverCert.pem
```
{: pre}


## Uploading a cert/key pair to the browser
{: #upload-cert-to-browser}

To access a destination which enforcing mutual authentication, you must convert your cert/key pair to a PKCS#12 file and upload it to your browser.

To create the PKCS#12 file, use the following command:

```
openssl pkcs12 -export -in <path_to_cert> -inkey <path_to_key> -name "my-sg-pair" -out <path_to_write_to>.p12
```
{: pre}

In Firefox, the .p12 file can be uploaded to Options > Preferences > Advanced > Certificates > View Certificates > Your Certificates > Import

In Chrome, the .p12 file can be uploaded to Settings > HTTPS/SSL > Manage certificates > Import

In Internet Explorer, the .p12 file can be uploaded to Settings > Content > Certificates > Import
