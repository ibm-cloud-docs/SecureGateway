---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-25"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Gestión de certificados/claves
{: #cert-key-management}

## Generación de un par certificado/clave autofirmado
{: #self-signed-cert-gen}

Para generar un par de certificado/clave autofirmado, ejecute el mandato siguiente:

```
openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 -keyout serverKey.pem -out serverCert.pem
```
{: pre}


## Carga de un par de certificado/clave en el navegador
{: #upload-cert-to-browser}

Para acceder a un destino que impone una autenticación mutua, debe convertir el par certificado/clave a un archivo PKCS#12 y cargarlo en el navegador.

Para crear el archivo PKCS#12, utilice este mandato:

```
openssl pkcs12 -export -in <path_to_cert> -inkey <path_to_key> -name "my-sg-pair" -out <path_to_write_to>.p12
```
{: pre}

En Firefox, el archivo .p12 se puede cargar en Opciones > Preferencias > Avanzadas > Certificados > Ver certificados > Sus certificados > Importar

En Chrome, el archivo .p12 se puede cargar en Configuración > HTTPS/SSL > Gestionar certificados > Importar

En Internet Explorer, el archivo .p12 se puede cargar en Configuración > Contenido > Certificados > Importar
