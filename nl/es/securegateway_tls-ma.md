---

copyright:
  years: 2015, 2017
lastupdated: "2017-04-10"

---
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Autenticación mutua de TLS de Node.js
{: #nodejs-tls-ma}

En este ejemplo se muestra cómo configurar la autenticación mutua en ambos lados de un destino local: autenticación de usuarios y autenticación de recursos.

Sé que el recurso con el que quiero conectar se alojará en la misma máquina que el cliente de Secure Gateway, estará a la escucha en el puerto 8999 y requiere autenticación mutua para permitir una conexión.  Con esta información, puedo empezar a crear mi destino.

![Autenticación mutua de TLS inicial](./images/tlsMA.png?raw=true "Autenticación mutua de TLS inicial")

## Autenticación de usuarios
{: #tls-ma-user-auth}
Voy a crear una nueva aplicación, por lo que no tengo ningún par de certificado/clave existente para que utilice mi aplicación, de modo que dejaré que los servidores de Secure Gateway generen un par automáticamente.  Para ello, solo tengo que dejar vacío el campo de carga de autenticación de usuarios.  Si ya tuviera un par que pudiera utilizar mi aplicación, cargaría mi certificado en este campo.

## Autenticación de recursos
{: #tls-ma-resource-auth}

### Autenticación local
{: #tls-ma-on-prem-auth}
Para que el cliente de Secure Gateway autentique el recurso al que se está conectando, tengo que proporcionar al cliente el certificado (o cadena de certificados) que el recurso presentará.  Mi recurso no tiene una cadena de certificados completa, por lo que solo tengo que cargar su certificado en el campo Autenticación local.  Este campo acepta hasta 6 archivos de certificados independientes (.pem, .cer, .der, .crt).

### Certificado y clave de cliente
{: #tls-ma-client-cert-and-key}
Si tengo que especificar cómo se identificará el cliente de Secure Gateway en mi recurso, puedo cargar un certificado y una clave aquí para que los utilice el cliente.  Puesto que el cliente y el recurso se ejecutan en la misma máquina, puedo dejar este campo vacío y dejar que los servidores de Secure Gateway generen un par automáticamente.  Si mi recurso estuviera en otro host, tendría que [generar un par de certificado/clave que cargar](/docs/services/SecureGateway/securegateway_keygen.html).

![Autenticación mutua de TLS local](./images/localTLSma.png?raw=true "Autenticación mutua de TLS local")

Cuando cree este destino, los servidores de Secure Gateway generarán automáticamente un par de certificado/clave para que lo utilice mi aplicación, así como un par para que lo utilice el cliente de Secure Gateway cuando se conecte a mi recurso local.  El destino también tendrá el certificado de mi recurso local que proporcionar al cliente de Secure Gateway para que lo utilice en la CA durante la conexión.  Después de crearlo, puedo editar mi destino para ver la información siguiente:

![Detalles de la autenticación mutua de TLS local](./images/editLocalTLSma.png?raw=true "Detalles de la autenticación mutua de TLS local")

## Descarga de los archivos de seguridad
{: #tls-ma-download-files}
En el panel de información de mi destino, hay un enlace para descargar un archivo .zip que contiene todos los certificados y claves asociados con mi destino:

![Panel de información de autenticación mutua](./images/infoPanelMA.png?raw=true "Panel de información de autenticación mutua")

Después de descargar el .zip y de extraer los archivos, tengo lo siguiente:

Nombre de archivo | Finalidad
-- | --
secureGatewayCert.pem | Certificado primario del servidor de Secure Gateway
DigiCertCA2.pem | Certificado intermedio del servidor de Secure Gateway
DigiCertTrustedRoot.pem | Certificado raíz del servidor de Secure Gateway
Nn5TJ34LyVQ_i2NOT_cert.pem | Certificado generado automáticamente para que lo utilice mi aplicación cuando se conecte al puerto y al host de la nube
Nn5TJ34LyVQ_i2NOT_key.pem | Clave generada automáticamente para que la utilice mi aplicación cuando se conecte al puerto y al host de la nube
Nn5TJ34LyVQ_i2NOT_destCert.pem | Certificado generado automáticamente para que lo utilice el cliente de SG cuando se conecte al destino
Nn5TJ34LyVQ_i2NOT_destKey.pem | Clave generada automáticamente para que la utilice el cliente de SG cuando se conecte al destino
Nn5TJ34LyVQ_clientCert.pem | Certificado generado automáticamente desde la pasarela para que lo utilice el cliente de SG en el caso de que no tenga un archivo `_destCert` o `_destKey`
localServerCert.pem | El certificado de mi recurso local que se va a utilizar en la CA del cliente de Secure Gateway

## Creación de la aplicación
{: #tls-ma-app-example}
Ahora que tengo mi puerto y host de nube para mi destino, así como los distintos certificados y claves, puedo empezar a escribir mi aplicación para que se conecte a Secure Gateway.  Se trata de una sencilla aplicación Node.js que se conectará a mi servidor TLS local, recibirá una pequeña cantidad de datos y luego cerrará la conexión.

```javascript
let fs = require('fs');
let tls = require('tls');

let client_opts = {
    cert : fs.readFileSync('Nn5TJ34LyVQ_i2NOT_cert.pem'),
    key : fs.readFileSync('Nn5TJ34LyVQ_i2NOT_key.pem'),
    host : 'cap-sg-prd-1.integration.ibmcloud.com',
    port : 17996,
    rejectUnauthorized : true
}

let so = tls.connect(client_opts, function() {
    console.log('Client connected, authorized:', so.authorized);
    if (!so.authorized) console.log('Client authorization error:', so.authorizationError)
});

so.on('data', function(data) {
    console.log('Client data:', data.toString())
});

so.on('error', function(err) {
    console.log('Client error',err);
});

so.on('close', function() {
    console.log('Client connection closing');
});
```
{: codeblock}

## Creación del servidor TLS
{: #tls-ma-server-example}
Tengo mi servidor TLS local, que es una simple aplicación Node.js que escuchará las conexiones, escribirá un mensaje sobre una conexión correcta y luego cerrará dicha conexión.

```javascript
let fs = require('fs');
let tls = require('tls');

let server_opts = {
    cert : fs.readFileSync('localServerCert.pem'),
    key : fs.readFileSync('localServerKey.pem'),
    // pfx : fs.readFileSync(''),
    // passphrase : '',
    ca : [fs.readFileSync('Nn5TJ34LyVQ_i2NOT_destCert.pem')],
    rejectUnauthorized : true,
    requestCert : true
}

// Crear el servidor TLS local con los valores definidos anteriormente
let server = tls.createServer(server_opts, function(socket) {
    socket.write('Successful connection message from the server');
    socket.end();
});

// Configurar los distintos escuchas de sucesos para el servidor y registrar su salida
server.on('error', function(err) {
    console.log('Server error', err);
});

server.on('close', function() {
    console.log('Server closing');
});

server.on('tlsClientError', function(err) {
    console.log('Server tlsClientError', err);
});

server.listen(8999);
```
{: codeblock}

## Actualización de la lista de control de accesos
{: #tls-ma-acl}
Antes de probar mi aplicación, tengo que asegurarme de que la ACL de mi cliente esté configurada correctamente.  He añadido `localhost:8999` a mi ACL:

```
--------------------------------------------------------------------
           -- Secure Gateway Client Access Control List --

 Connections to unlisted rules are currently: denied

 hostname:port                path           value
 localhost:8999                            Allow
--------------------------------------------------------------------
```
{: screen}

## Prueba de la conexión
{: #tls-ma-testing}
Ahora que mi aplicación, mi servidor local y mi cliente se han configurado, obtengo la salida siguiente de mi aplicación y de mi cliente:

### Aplicación
{: #tls-ma-testing-app}

```
Client connected, authorized: true
Client data: Successful connection message from the server
Client connection closing
```
{: screen}

### Cliente de Secure Gateway
{: #tls-ma-testing-client}

```
[2017-04-07 12:22:42.363] [INFO] (Client ID Nn5TJ34LyVQ_qCB) Connection #1 is being established to localhost:8999
[2017-04-07 12:22:42.381] [INFO] (Client ID Nn5TJ34LyVQ_qCB) Connection #1 to localhost:8999 was closed
```
{: screen}

## ¡Configuración correcta!
{: #tls-ma-testing-result}
Hemos configurado correctamente la autenticación mutua para la autenticación de usuarios y la autenticación de recursos.
